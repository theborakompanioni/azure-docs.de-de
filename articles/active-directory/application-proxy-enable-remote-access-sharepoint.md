---
title: Aktivieren des Remotezugriffs auf SharePoint per Azure AD-Anwendungsproxy | Microsoft-Dokumentation
description: Hier finden Sie grundlegende Informationen zu Azure AD-Anwendungsproxyconnectors.
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/12/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 8436238546515b66b58b31673d54586e4a20f50f
ms.openlocfilehash: 86f10c04368d1c382939b418c6909ca807b3bf5a


---

# <a name="enable-remote-access-to-sharepoint-with-azure-ad-app-proxy"></a>Aktivieren des Remotezugriffs auf SharePoint per Azure AD-Anwendungsproxy

In diesem Artikel wird beschrieben, wie Sie SharePoint Server lokal in den Azure AD-Anwendungsproxy integrieren.

> [!NOTE]
> Das Feature "Anwendungsproxy" ist nur verfügbar, wenn Sie Azure Active Directory auf die Premium oder Basic Edition aktualisiert haben. Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](active-directory-editions.md).
> 

##<a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie bereits über SharePoint 2013 oder höher verfügen und die Anwendung in Ihrer Umgebung eingerichtet wurde und ausgeführt wird. Beachten Sie auch die folgenden erforderlichen Voraussetzungen:

* SharePoint enthält native Kerberos-Unterstützung. Aus diesem Grund können Benutzer, die über den Azure AD-Anwendungsproxy per Remoteverbindung auf interne Websites zugreifen, davon ausgehen, dass sie das nahtlose einmalige Anmelden nutzen können.

* Sie müssen an Ihrer SharePoint Server-Instanz einige Konfigurationsänderungen vornehmen. Wir empfehlen Ihnen die Verwendung einer Stagingumgebung. Auf diese Weise können Sie zuerst Updates an Ihrem Stagingserver vornehmen und dann einen Testzyklus durchführen, bevor die Produktionsphase beginnt.

* Wir setzen voraus, dass Sie SSL für SharePoint bereits eingerichtet haben, da wir SSL für die veröffentlichte URL benötigen. Auf Ihrer internen Website muss SSL aktiviert sein, um sicherzustellen, dass Links richtig gesendet bzw. zugeordnet werden. Falls Sie SSL nicht konfiguriert haben, helfen Ihnen die Informationen unter [Configure SSL for SharePoint 2013](https://blogs.msdn.microsoft.com/fabdulwahab/2013/01/20/configure-ssl-for-sharepoint-2013) (Konfigurieren von SSL für SharePoint 2013) weiter. Der Artikel enthält eine Anleitung zum Einrichten von SSL. Stellen Sie außerdem sicher, dass das von Ihnen ausgestellte Zertifikat für den Connectorcomputer vertrauenswürdig ist. (Dies muss kein öffentlich ausgestelltes Zertifikat sein.)

##<a name="steps-to-set-up-sharepoint"></a>Schritte zum Einrichten von SharePoint

Führen Sie diese Schritte aus, um den Remotezugriff auf SharePoint per Azure AD-Anwendungsproxy zu aktivieren:

**Teil 1: Konfigurieren des einmaligen Anmeldens (SSO)**

  * Schritt A: Sicherstellen, dass SharePoint Server als Dienstkonto ausgeführt wird
  * Schritt B: Konfigurieren von SharePoint für Kerberos
  * Schritt C: Festlegen eines Dienstprinzipalnamens (SPN) für das Konto, das SharePoint zugewiesen ist
  * Schritt D: Sicherstellen, dass der Connector als vertrauenswürdiger Delegat für SharePoint festgelegt wurde

**Teil 2: Ermöglichen des sicheren Remotezugriffs**

 * Veröffentlichen Sie die SharePoint-Farm über den Azure AD-Anwendungsproxy.

**Teil 3: Sicherstellen, dass SharePoint über die externe URL informiert ist**

 * Legen Sie in SharePoint alternative Zugriffszuordnungen fest.

### <a name="part-1-set-up-single-sign-on-sso-to-sharepoint"></a>Teil 1: Einrichten des einmaligen Anmeldens (SSO) an SharePoint

Unsere Kunden wünschen sich für ihre Back-End-Anwendungen (hier: SharePoint Server) die größtmögliche Benutzerfreundlichkeit durch das einmalige Anmelden. Bei diesem häufigen Azure AD-Szenario müssen Benutzer sich nur einmal authentifizieren, da sie anschließend keine Aufforderung zur Authentifizierung mehr erhalten.

Für lokale Anwendungen, für die die Windows-Authentifizierung erforderlich ist oder verwendet wird, kann dies über das Kerberos-Authentifizierungsprotokoll und die Funktion „Eingeschränkte Kerberos-Delegierung“ (Kerberos Constrained Delegation, KCD) erreicht werden. Bei Konfiguration von KCD kann mit dem Anwendungsproxyconnector ein Windows-Ticket/-Token für einen bestimmten Benutzer abgerufen werden. Dies gilt auch, wenn sich der Benutzer nicht direkt an Windows angemeldet hat. Weitere Informationen zu KCD finden Sie unter [Eingeschränkte Kerberos-Delegierung: Übersicht](https://technet.microsoft.com/en-us/library/jj553400.aspx).

Führen Sie die unten angegebenen Schritte aus, um dies für eine SharePoint Server-Instanz einzurichten.

**Schritt A: Sicherstellen, dass SharePoint unter einem Dienstkonto ausgeführt wird, und nicht unter einem lokalen System, lokalen Dienst oder Netzwerkdienst**

Zunächst müssen Sie sicherstellen, dass SharePoint unter einem definierten Dienstkonto ausgeführt wird. Dies ist erforderlich, damit die Dienstprinzipalnamen (SPNs) einem gültigen Konto zugeordnet werden können. Mithilfe von SPNs können vom Kerberos-Protokoll verschiedene Dienste identifiziert werden. Außerdem wird dies für die spätere Konfiguration von KCD benötigt.

Gehen Sie wie folgt vor, um sicherzustellen, dass Ihre Websites unter einem definierten Dienstkonto ausgeführt werden:

1. Öffnen Sie die Website **SharePoint 2013-Zentraladministration**.
2. Navigieren Sie zu **Sicherheit**, und wählen Sie die Option **Dienstkonten konfigurieren**.
3. Wählen Sie **Webanwendungspool – SharePoint – 80**. Die Optionen können je nach Name Ihres Webpools oder bei der standardmäßigen Verwendung von SSL leicht variieren.

  ![Azure AD-Anwendungsproxyconnectors](./media/application-proxy-remote-sharepoint/remote-sharepoint-service-web-application.png)

4. Wenn für **Wählen Sie ein Konto für diese Komponente aus** die Option **Lokaler Dienst** oder **Netzwerkdienst** festgelegt ist, müssen Sie ein Konto erstellen. Wenn nicht, müssen Sie nichts mehr tun und können mit dem nächsten Schritt fortfahren. 
5. Wählen Sie die Option **Neues verwaltetes Konto registrieren**. Nachdem das Konto erstellt wurde, müssen Sie den **Webanwendungspool** festlegen, bevor Sie das Konto verwenden können.

> [!NOTE]
Sie müssen über ein vorab erstelltes AD-Konto für den Dienst verfügen. Es ist ratsam, die automatische Kennwortänderung zuzulassen. Weitere Informationen zu allen Schritten und zur Problembehandlung finden Sie unter [Konfigurieren der automatischen Kennwortänderung in SharePoint 2013](https://technet.microsoft.com/EN-US/library/ff724280.aspx). 

**Schritt B: Konfigurieren von SharePoint für Kerberos**

Wir verwenden KCD, um das einmalige Anmelden (SSO) für den SharePoint-Server zu ermöglichen. Dies funktioniert nur mit Kerberos. 

Gehen Sie wie folgt vor, um Ihre SharePoint-Website für die Kerberos-Authentifizierung zu konfigurieren:

1. Öffnen Sie die Website **SharePoint 2013-Zentraladministration**.
2. Navigieren Sie zu **Anwendungsverwaltung**, wählen Sie **Webanwendungen verwalten**, und wählen Sie Ihre SharePoint-Website aus. In diesem Beispiel ist dies **SharePoint – 80**.

  ![Azure AD-Anwendungsproxyconnectors](./media/application-proxy-remote-sharepoint/remote-sharepoint-manage-web-applications.png)
  
3. Klicken Sie in der Symbolleiste auf **Authentifizierungsanbieter**.
4. Klicken Sie im Feld **Authentifizierungsanbieter** auf **Standardzone**, um die Einstellungen anzuzeigen.
5. Scrollen Sie im Feld **Authentifizierung bearbeiten** nach unten, bis **Anspruchsauthentifizierungstypen** angezeigt wird. Stellen Sie sicher, dass sowohl die Option **Windows-Authentifizierung aktivieren** als auch die Option **Integrierte Windows-Authentifizierung** aktiviert ist. 
6. Vergewissern Sie sich im Dropdownfeld, dass **Aushandlung (Kerberos)** ausgewählt ist.

  ![Azure AD-Anwendungsproxyconnectors](./media/application-proxy-remote-sharepoint/remote-sharepoint-service-edit-authentication.png)

7. Klicken Sie ganz unten im Feld **Authentifizierung bearbeiten** auf **Speichern**.

**Schritt C: Festlegen eines SPN für das SharePoint-Dienstkonto**

Vor dem Konfigurieren von KCD müssen wir den SharePoint-Dienst identifizieren, der als das weiter oben konfigurierte Dienstkonto ausgeführt wird. Hierzu können wir einen Dienstprinzipalnamen (SPN) festlegen. Weitere Informationen finden Sie unter [Dienstprinzipalnamen](https://technet.microsoft.com/en-us/library/cc961723.aspx).

Das SPN-Format lautet wie folgt:

```
<service class>/<host>:<port>
```

_service class_ ist ein eindeutiger Name für den Dienst. Für SharePoint verwenden wir HTTP.

_host_ ist die vollqualifizierte Domäne oder der NetBIOS-Name des Hosts, auf dem der Dienst ausgeführt wird. Bei einer SharePoint-Website muss dies unter Umständen die URL der Website sein. Dies richtet sich nach der von Ihnen verwendeten IIS-Version.

_port_ ist optional. Falls der FQDN der SharePoint Server-Instanz wie folgt lautet:

```
sharepoint.demo.o365identity.us
```

Lautet der SPN: 

```
HTTP/ sharepoint.demo.o365identity.us demo
```

Darüber hinaus müssen Sie ggf. auch SPNs für bestimmte Websites auf Ihrem Server festlegen. Weitere Informationen finden Sie unter [Konfigurieren der Kerberos-Authentifizierung](https://technet.microsoft.com/en-us/library/cc263449(v=office.12).aspx). Achten Sie besonders auf den Abschnitt „Erstellen von Dienstprinzipalnamen für Ihre Webanwendungen mithilfe der Kerberos-Authentifizierung“. 

Dies erreichen Sie am einfachsten, indem Sie sich an die SPN-Formate halten, die für Ihre Website ggf. bereits vorhanden sind. Kopieren Sie diese SPNs für die Registrierung gegenüber dem Dienstkonto. Gehen Sie dazu folgendermaßen vor:

1. Greifen Sie von einem anderen Computer aus mit dem SPN auf die Website zu. 
 Hierbei werden die relevanten Kerberos-Tickets auf dem Computer zwischengespeichert. Diese Tickets enthalten den SPN der Zielwebsite, auf die Sie zugegriffen haben. Wir können den SPN für diese Website mit dem Tool [Klist](http://web.mit.edu/kerberos/krb5-devel/doc/user/user_commands/klist.html) abrufen.
 
2. Führen Sie in einem Befehlsfenster, das in demselben Kontext wie der Benutzer ausgeführt wird, der im Browser auf die Website zugegriffen hat, den folgenden Befehl aus: 
```
Klist
```
3. Hiermit werden die Zieldienst-SPNs zurückgegeben. In diesem Beispiel ist der hervorgehobene Wert der erforderliche SPN:

  ![Azure AD-Anwendungsproxyconnectors](./media/application-proxy-remote-sharepoint/remote-sharepoint-target-service.png)
  
4. Nachdem Sie nun über den SPN verfügen, müssen Sie sicherstellen, dass er richtig für das Dienstkonto konfiguriert ist, das Sie für die Webanwendung zuvor eingerichtet haben. Führen Sie die Schritte im nächsten Abschnitt aus.

**Festlegen des SPN**

Führen Sie zum Festlegen des SPN den unten angegebenen Befehl an der Befehlseingabeaufforderung als Administrator der Domäne aus.

```
setspn -S http/sharepoint.demo.o365identity.us demo\sp_svc
```

Mit diesem Befehl wird der SPN für das SharePoint-Dienstkonto festgelegt, das als _demo\sp_svc_ ausgeführt wird.

Ersetzen Sie _http/sharepoint.demo.o365identity.us_ durch den SPN für Ihren Server und _demo\sp_svc_ durch das Dienstkonto in Ihrer Umgebung. Mit dem Befehl „setspn“ wird nach dem SPN gesucht, bevor er hinzugefügt wird. In diesem Fall wird ggf. der Fehler **Duplicate SPN Value** (Doppelter SPN-Wert) angezeigt. Stellen Sie bei Anzeige dieses Fehlers sicher, dass der Wert dem Dienstkonto zugeordnet ist.

Sie können überprüfen, ob der SPN hinzugefügt wurde, indem Sie den Befehl „setspn“ mit der Option „-l“ ausführen. Weitere Informationen zum Tool „Setspn“ finden Sie unter [Setspn](https://technet.microsoft.com/en-us/library/cc731241.aspx).

**Schritt D: Sicherstellen, dass Connectors für die Delegierung zu SharePoint als vertrauenswürdig angesehen werden**

In diesem Schritt konfigurieren Sie KCD so, dass der Azure AD-Anwendungsproxydienst Benutzeridentitäten an den SharePoint-Dienst delegieren kann. Hierfür richten Sie den Anwendungsproxyconnector so ein, dass er zum Abrufen von Kerberos-Tickets für Benutzer verwendet werden kann, die in Azure AD authentifiziert wurden. Anschließend übergibt dieser Server den Kontext an die Zielanwendung bzw. in diesem Fall an SharePoint. 

Zum Konfigurieren von KCD müssen Sie die folgenden Schritte für jeden Connectorcomputer wiederholen:

1. Melden Sie sich als Domänenadministrator an einem Domänencontroller an, und öffnen Sie dann **Active Directory-Benutzer und -Computer**.
2. Suchen Sie nach dem Computer, auf dem der Connector ausgeführt wird. In diesem Beispiel ist dies dieselbe SharePoint Server-Instanz.
3. Doppelklicken Sie auf den Computer und anschließend auf die Registerkarte **Delegierung**.
4. Stellen Sie sicher, dass in den Delegierungseinstellungen **Computer bei Delegierungen angegebener Dienste vertrauen** festgelegt ist, und wählen Sie dann die Option **Beliebiges Authentifizierungsprotokoll verwenden**.

  ![Azure AD-Anwendungsproxyconnectors](./media/application-proxy-remote-sharepoint/remote-sharepoint-delegation-box.png)
  
5. Als Nächstes müssen Sie den SPN hinzufügen, den Sie zuvor für das Dienstkonto erstellt haben. Klicken Sie auf die Schaltfläche **Hinzufügen** und dann auf **Benutzer oder Computer**, und suchen Sie nach dem zuvor erstellten Konto.

  ![Azure AD-Anwendungsproxyconnectors](./media/application-proxy-remote-sharepoint/remote-sharepoint-users-computers.png)

 Es sollte eine Liste mit SPNs angezeigt werden, die Sie auswählen können. Fügen Sie hier den oben festgelegten SPN hinzu. 
6. Wählen Sie den entsprechenden Eintrag aus, und klicken Sie auf **OK**. Klicken Sie erneut auf **OK**, um die Änderung zu speichern.

### <a name="part-2-enable-remote-access-to-sharepoint"></a>Teil 2: Aktivieren des Remotezugriffs auf SharePoint

Nachdem Sie SharePoint für Kerberos aktiviert und KCD konfiguriert haben, können Sie das einmalige Anmelden (SSO) für SharePoint einrichten. Anschließend können Sie SharePoint mit dem Connector für den Remotezugriff über den Azure AD-Anwendungsproxy veröffentlichen.

**Veröffentlichen von SharePoint mit dem Azure AD-Anwendungsproxy**

Zum Ausführen der unten angegebenen Schritte müssen Sie unter dem Azure Active Directory-Konto Mitglied der Rolle „Globaler Administrator“ sein.

1. Melden Sie sich am Azure-Verwaltungsportal unter „https://manage.windowsazure.com“ an, und suchen Sie nach Ihrem Azure AD-Mandanten.
2. Klicken Sie auf **Anwendungen** und dann auf **Hinzufügen**.
3. Wählen Sie **Eine Anwendung veröffentlichen, die von außerhalb Ihres Netzwerks aufgerufen werden kann**. Falls diese Option nicht angezeigt wird, sollten Sie sicherstellen, dass im Mandanten Azure AD Basic oder Premium eingerichtet ist.
4. Füllen Sie die einzelnen Optionen im angezeigten Dialogfeld wie folgt aus: 
 * **Name**: Beliebiger Wert, z.B. _SharePoint_.
 * **Interne URL**: Dies ist die URL der internen SharePoint-Website, z.B. „https://SharePoint/“. Achten Sie in diesem Beispiel darauf, dass Sie „https“ verwenden.
 * **Pre-authentication Method** (Methode für die Vorauthentifizierung): Wählen Sie die Option _Azure Active Directory_.

  ![Azure AD-Anwendungsproxyconnectors](./media/application-proxy-remote-sharepoint/remote-sharepoint-add-application.png)

5. Klicken Sie nach dem Veröffentlichen der App auf die Registerkarte **Konfigurieren**.
6. Scrollen Sie nach unten zur Option **URL in Headern übersetzen**. Ändern Sie den Standardwert _JA_ in _NEIN_. 

 SharePoint verwendet den Wert _Hostheader_, um nach der Website zu suchen, und generiert basierend auf diesem Wert auch Links. Hiermit wird Folgendes sichergestellt: Bei allen von SharePoint generierten Links handelt es sich um eine veröffentlichte URL, für die die Verwendung der externen URL korrekt festgelegt ist. Wenn der Wert auf _JA_ festgelegt wird, kann der Connector die Anforderung auch an die Back-End-Anwendung weiterleiten. Die Einstellung auf _NEIN_ bedeutet dagegen, dass der Connector den internen Hostnamen nicht sendet. Stattdessen wird der Hostheader als veröffentlichte URL an die Back-End-Anwendung gesendet.

 Um sicherzustellen, dass diese URL von SharePoint akzeptiert wird, müssen Sie eine weitere Konfiguration für die SharePoint Server-Instanz durchführen. Diesen Schritt führen Sie im nächsten Abschnitt aus.

7. Ändern Sie die **Interne Authentifizierungsmethode** in _Integrierte Sicherheit von Windows_. Wenn für Ihren Azure AD-Mandanten in der Cloud ein anderer UPN als in der lokalen Umgebung verwendet wird, sollten Sie beachten, dass auch **Delegierte Identität für Anmeldung** aktualisiert werden muss.
8. Legen Sie **Interner Anwendungs-SPN** auf den obigen Wert fest. Beispiel: _http/sharepoint.demo.o365identity.us_.
9. Sie können die Anwendung jetzt Ihren Zielbenutzern zuweisen.

Ihre Anwendung sollte in etwa wie folgt aussehen:

  ![Azure AD-Anwendungsproxyconnectors](./media/application-proxy-remote-sharepoint/remote-sharepoint-internal-application-spn.png)

###<a name="part-3-ensure-sharepoint-knows-about-the-external-url"></a>Teil 3: Sicherstellen, dass SharePoint über die externe URL informiert ist

Mit dem letzten erforderlichen Schritt stellen Sie sicher, dass SharePoint die Website anhand der externen URL finden kann, um Links basierend auf dieser externen URL zu erstellen. Hierfür konfigurieren Sie alternative Zugriffszuordnungen für SharePoint.

**Konfigurieren eines alternativen Namens für die SharePoint-Website**

1. Öffnen Sie die Website **SharePoint 2013-Zentraladministration**.
2. Wählen Sie unter **Systemeinstellungen** die Option **Alternative Zugriffszuordnungen konfigurieren**. 

 Das Feld **Alternative Zugriffszuordnungen** wird geöffnet.

  ![Azure AD-Anwendungsproxyconnectors](./media/application-proxy-remote-sharepoint/remote-sharepoint-alternate-access1.png)

3. Wählen Sie in der Dropdownliste neben **Alternative Access Mapping Collections** (Alternative Zugriffszuordnungssammlungen) die Option **Change Alternative Access Mapping Collection** (Alternative Zugriffszuordnungssammlung ändern).
4. Wählen Sie Ihre Website aus, z.B. **SharePoint – 80**.

  ![Azure AD-Anwendungsproxyconnectors](./media/application-proxy-remote-sharepoint/remote-sharepoint-alternate-access2.png)

5. Sie können wählen, ob Sie die veröffentlichte URL entweder als interne URL oder als öffentliche URL hinzufügen möchten. In diesem Beispiel wird eine **öffentliche URL** als Extranet verwendet.
6. Klicken Sie unter dem Pfad **Extranet** auf **Öffentliche URLs bearbeiten**, und geben Sie dann den Pfad für die Veröffentlichung der Anwendung ein (wie im vorherigen Schritt). Beispiel: _https://sharepoint-iddemo.msappproxy.net_.

  ![Azure AD-Anwendungsproxyconnectors](./media/application-proxy-remote-sharepoint/remote-sharepoint-alternate-access3.png)

7. Klicken Sie auf **Speichern**. 

 Sie können auf die SharePoint-Website jetzt extern über den Azure AD-Anwendungsproxy zugreifen.

##<a name="next-steps"></a>Nächste Schritte

[Bereitstellen von sicherem Remotezugriff auf lokale Anwendungen](active-directory-application-proxy-get-started.md)<br>
[Grundlegendes zu Azure AD-Anwendungsproxyconnectors](application-proxy-understand-connectors.md)<br>
[Publishing SharePoint 2016 and Office Online Server with Azure AD Application Proxy](https://blogs.technet.microsoft.com/dawiese/2016/06/09/publishing-sharepoint-2016-and-office-online-server-with-azure-ad-application-proxy/) (Veröffentlichen von SharePoint 2016 und Office Online Server mit dem Azure AD-Anwendungsproxy)









<!--HONumber=Feb17_HO1-->


