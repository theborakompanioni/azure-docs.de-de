---
title: Aktivieren des Remotezugriffs auf SharePoint per Azure AD-Anwendungsproxy | Microsoft-Dokumentation
description: Behandelt die Grundlagen zur Integration eines lokalen SharePoint-Servers mit Azure AD-Anwendungsproxy.
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
ms.date: 03/22/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 0d6f6fb24f1f01d703104f925dcd03ee1ff46062
ms.openlocfilehash: 93b36891c960582563a4ff9c622cd5ac3198dfeb
ms.lasthandoff: 04/17/2017


---

# <a name="enable-remote-access-to-sharepoint-with-azure-ad-application-proxy"></a>Aktivieren des Remotezugriffs auf SharePoint per Azure AD-Anwendungsproxy

In diesem Artikel wird beschrieben, wie Sie einen lokalen SharePoint-Server mit dem Azure Active Directory (Azure AD)-Anwendungsproxy integrieren.

Führen Sie die Abschnitte in diesem Artikel Schritt für Schritt aus, um den Remotezugriff auf SharePoint mit dem Azure AD-Anwendungsproxy zu aktivieren.

## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie bereits über SharePoint 2013 oder höher verfügen und die Anwendung in Ihrer Umgebung eingerichtet wurde und ausgeführt wird. Beachten Sie auch die folgenden erforderlichen Voraussetzungen:

* Das Anwendungsproxy-Feature ist nur verfügbar, wenn Sie Azure Active Directory auf die Premium oder Basic Edition upgegradet haben. Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](active-directory-editions.md).

* SharePoint enthält native Kerberos-Unterstützung. Aus diesem Grund können Benutzer, die über den Azure AD-Anwendungsproxy per Remoteverbindung auf interne Websites zugreifen, davon ausgehen, dass sie das nahtlose einmalige Anmelden (seamless single sign-on, SSO) nutzen können.

* Sie müssen an Ihrer SharePoint Server-Instanz einige Konfigurationsänderungen vornehmen. Wir empfehlen Ihnen die Verwendung einer Stagingumgebung. Auf diese Weise können Sie zuerst Updates an Ihrem Stagingserver vornehmen und dann einen Testzyklus durchführen, bevor die Produktionsphase beginnt.

* Wir setzen voraus, dass Sie SSL für SharePoint bereits eingerichtet haben, da wir SSL für die veröffentlichte URL benötigen. Auf Ihrer internen Website muss SSL aktiviert sein, um sicherzustellen, dass Links richtig gesendet bzw. zugeordnet werden. Wenn Sie SSL nicht konfiguriert haben, finden Sie die Anweisungen unter [Configure SSL for SharePoint 2013 (Konfigurieren von SSL für SharePoint 2013)](https://blogs.msdn.microsoft.com/fabdulwahab/2013/01/20/configure-ssl-for-sharepoint-2013). Stellen Sie außerdem sicher, dass das von Ihnen ausgestellte Zertifikat für den Connectorcomputer vertrauenswürdig ist. (Das Zertifikat muss nicht öffentlich ausgestellt sein.)

## <a name="step-1-set-up-single-sign-on-to-sharepoint"></a>Schritt 1: Einrichten des einmaligen Anmeldens (SSO) für SharePoint

Unsere Kunden wünschen sich für ihre Back-End-Anwendungen (hier: SharePoint-Server) die größtmögliche Benutzerfreundlichkeit durch das einmalige Anmelden. Bei diesem häufigen Azure AD-Szenario müssen Benutzer sich nur einmal authentifizieren, da sie anschließend keine Aufforderung zur Authentifizierung mehr erhalten.

Für lokale Anwendungen, für die die Windows-Authentifizierung erforderlich ist oder verwendet wird, können Sie SSO über das Kerberos-Authentifizierungsprotokoll und die Funktion „Eingeschränkte Kerberos-Delegierung“ (Kerberos Constrained Delegation, KCD) erreichen. Bei Konfiguration von KCD kann mit dem Anwendungsproxyconnector ein Windows-Ticket/-Token für einen Benutzer abgerufen werden. Dies gilt auch, wenn sich der Benutzer nicht direkt bei Windows angemeldet hat. Weitere Informationen zu KCD finden Sie unter [Eingeschränkte Kerberos-Delegierung: Übersicht](https://technet.microsoft.com/library/jj553400.aspx).

Um KCD für einen SharePoint-Server einzurichten, verwenden Sie die Verfahren in den folgenden Abschnitten.

### <a name="ensure-that-sharepoint-is-running-under-a-service-account"></a>Sicherstellen, dass SharePoint unter einem Dienstkonto ausgeführt wird

Stellen Sie zuerst sicher, dass SharePoint unter einem definierten Dienstkonto ausgeführt wird, und nicht unter einem lokalen System, lokalen Dienst oder Netzwerkdienst. Dies ist erforderlich, damit Sie die Dienstprinzipalnamen (SPNs) einem gültigen Konto zuordnen können. Mithilfe von SPNs können vom Kerberos-Protokoll verschiedene Dienste identifiziert werden. Außerdem wird das Konto für die spätere Konfiguration von KCD benötigt.

Gehen Sie wie folgt vor, um sicherzustellen, dass Ihre Websites unter einem definierten Dienstkonto ausgeführt werden:

1. Öffnen Sie die Website **SharePoint 2013-Zentraladministration**.
2. Navigieren Sie zu **Sicherheit**, und wählen Sie die Option **Dienstkonten konfigurieren**.
3. Wählen Sie **Webanwendungspool – SharePoint – 80**. Die Optionen können je nach Name Ihres Webpools oder bei der standardmäßigen Verwendung von SSL leicht variieren.

  ![Optionen für das Konfigurieren eines Dienstkontos](./media/application-proxy-remote-sharepoint/remote-sharepoint-service-web-application.png)

4. Wenn für **Wählen Sie ein Konto für diese Komponente aus** die Option **Lokaler Dienst** oder **Netzwerkdienst** festgelegt ist, müssen Sie ein Konto erstellen. Wenn nicht, müssen Sie nichts mehr tun und können mit dem nächsten Abschnitt fortfahren.
5. Wählen Sie die Option **Neues verwaltetes Konto registrieren**. Nachdem das Konto erstellt wurde, müssen Sie den **Webanwendungspool** festlegen, bevor Sie das Konto verwenden können.

> [!NOTE]
Sie müssen über ein vorab erstelltes Azure AD-Konto für den Dienst verfügen. Es ist ratsam, die automatische Kennwortänderung zuzulassen. Weitere Informationen zu allen Schritten und zur Problembehandlung finden Sie unter [Konfigurieren der automatischen Kennwortänderung in SharePoint 2013](https://technet.microsoft.com/library/ff724280.aspx).

### <a name="configure-sharepoint-for-kerberos"></a>Konfigurieren von SharePoint für Kerberos

KCD wird verwendet, um das einmalige Anmelden für den SharePoint-Server zu ermöglichen. Dies funktioniert nur mit Kerberos.

Gehen Sie wie folgt vor, um Ihre SharePoint-Website für die Kerberos-Authentifizierung zu konfigurieren:

1. Öffnen Sie die Website **SharePoint 2013-Zentraladministration**.
2. Navigieren Sie zu **Anwendungsverwaltung**, wählen Sie **Webanwendungen verwalten**, und wählen Sie Ihre SharePoint-Website aus. In diesem Beispiel ist dies **SharePoint – 80**.

  ![Auswählen der SharePoint-Website](./media/application-proxy-remote-sharepoint/remote-sharepoint-manage-web-applications.png)

3. Klicken Sie in der Symbolleiste auf **Authentifizierungsanbieter**.
4. Klicken Sie im Feld **Authentifizierungsanbieter** auf **Standardzone**, um die Einstellungen anzuzeigen.
5. Scrollen Sie im Dialogfeld **Authentifizierung bearbeiten** nach unten, bis **Anspruchsauthentifizierungstypen** angezeigt wird. Stellen Sie sicher, dass sowohl die Option **Windows-Authentifizierung aktivieren** als auch die Option **Integrierte Windows-Authentifizierung** aktiviert ist.
6. Vergewissern Sie sich im Dropdownfeld, dass **Aushandlung (Kerberos)** ausgewählt ist.

  ![Dialogfeld „Authentifizierung bearbeiten“](./media/application-proxy-remote-sharepoint/remote-sharepoint-service-edit-authentication.png)

7. Klicken Sie unten im Dialogfeld **Authentifizierung bearbeiten** auf **Speichern**.

### <a name="set-a-service-principal-name-for-the-sharepoint-service-account"></a>Festlegen eines Dienstprinzipalnamens für das SharePoint-Dienstkonto

Vor dem Konfigurieren von KCD müssen Sie den SharePoint-Dienst identifizieren, der als das weiter oben konfigurierte Dienstkonto ausgeführt wird. Dazu müssen Sie einen Dienstprinzipalnamen (SPN) festlegen. Weitere Informationen finden Sie unter [Dienstprinzipalnamen](https://technet.microsoft.com/library/cc961723.aspx).

Das SPN-Format lautet wie folgt:

```
<service class>/<host>:<port>
```

Im SPN-Format:

* _service class_ ist ein eindeutiger Name für den Dienst. Für SharePoint verwenden Sie **HTTP**.

* _host_ ist die vollqualifizierte Domäne oder der NetBIOS-Name des Hosts, auf dem der Dienst ausgeführt wird. Bei einer SharePoint-Website muss dies unter Umständen die URL der Website sein. Dies richtet sich nach der von Ihnen verwendeten IIS-Version.

* _port_ ist optional.

Falls der FQDN der SharePoint Server-Instanz wie folgt lautet:

```
sharepoint.demo.o365identity.us
```

Dann lautet der SPN:

```
HTTP/ sharepoint.demo.o365identity.us demo
```

Sie müssen ggf. auch SPNs für bestimmte Websites auf Ihrem Server festlegen. Weitere Informationen finden Sie unter [Konfigurieren der Kerberos-Authentifizierung](https://technet.microsoft.com/library/cc263449(v=office.12).aspx). Achten Sie besonders auf den Abschnitt „Erstellen von Dienstprinzipalnamen für Ihre Webanwendungen mithilfe der Kerberos-Authentifizierung“.

Dazu halten Sie sich am besten an die SPN-Formate, die für Ihre Website ggf. bereits vorhanden sind. Kopieren Sie diese SPNs für die Registrierung gegenüber dem Dienstkonto. Gehen Sie dazu folgendermaßen vor:

1. Greifen Sie von einem anderen Computer aus mit dem SPN auf die Website zu.
 Hierbei werden die relevanten Kerberos-Tickets auf dem Computer zwischengespeichert. Diese Tickets enthalten den SPN der Zielwebsite, auf die Sie zugegriffen haben.

2. Sie können den SPN für diese Website mit dem Tool [Klist](http://web.mit.edu/kerberos/krb5-devel/doc/user/user_commands/klist.html) abrufen. Führen Sie in einem Befehlsfenster, das in demselben Kontext wie der Benutzer ausgeführt wird, der im Browser auf die Website zugegriffen hat, den folgenden Befehl aus:
```
Klist
```
Klist gibt dann die Ziel-SPNs zurück. In diesem Beispiel ist der hervorgehobene Wert der erforderliche SPN:

  ![Klist-Beispielergebnisse](./media/application-proxy-remote-sharepoint/remote-sharepoint-target-service.png)

4. Nachdem Sie nun über den SPN verfügen, müssen Sie sicherstellen, dass er richtig für das Dienstkonto konfiguriert ist, das Sie zuvor für die Webanwendung eingerichtet haben. Führen Sie den folgenden Befehl an der Befehlseingabeaufforderung als Administrator der Domäne aus:

 ```
 setspn -S http/sharepoint.demo.o365identity.us demo\sp_svc
 ```

 Mit diesem Befehl wird der SPN für das SharePoint-Dienstkonto festgelegt, das als _demo\sp_svc_ ausgeführt wird.

 Ersetzen Sie _http/sharepoint.demo.o365identity.us_ durch den SPN für Ihren Server und _demo\sp_svc_ durch das Dienstkonto in Ihrer Umgebung. Mit dem Befehl „setspn“ wird nach dem SPN gesucht, bevor er hinzugefügt wird. In diesem Fall wird ggf. der Fehler **Duplicate SPN Value** (Doppelter SPN-Wert) angezeigt. Stellen Sie bei Anzeige dieses Fehlers sicher, dass der Wert dem Dienstkonto zugeordnet ist.

Sie können überprüfen, ob der SPN hinzugefügt wurde, indem Sie den Befehl „setspn“ mit der Option „-l“ ausführen. Weitere Informationen zu diesem Befehl finden Sie unter [Setspn](https://technet.microsoft.com/library/cc731241.aspx).

### <a name="ensure-that-the-connector-is-set-as-a-trusted-delegate-to-sharepoint"></a>Sicherstellen, dass der Connector als vertrauenswürdiger Delegat für SharePoint festgelegt wurde

Konfigurieren Sie KCD so, dass der Azure AD-Anwendungsproxydienst Benutzeridentitäten an den SharePoint-Dienst delegieren kann. Hierfür richten Sie den Anwendungsproxyconnector so ein, dass er Kerberos-Tickets für Benutzer abruft, die in Azure AD authentifiziert wurden. Anschließend übergibt dieser Server den Kontext an die Zielanwendung bzw. in diesem Fall an SharePoint.

Wiederholen Sie zum Konfigurieren von KCD die folgenden Schritte für jeden Connectorcomputer:

1. Melden Sie sich als Domänenadministrator an einem Domänencontroller an, und öffnen Sie dann **Active Directory-Benutzer und -Computer**.
2. Suchen Sie nach dem Computer, auf dem der Connector ausgeführt wird. In diesem Beispiel ist dies derselbe SharePoint-Server.
3. Doppelklicken Sie auf den Computer, und klicken Sie anschließend auf die Registerkarte **Delegierung**.
4. Stellen Sie sicher, dass in den Delegierungseinstellungen **Computer bei Delegierungen angegebener Dienste vertrauen** festgelegt ist, und wählen Sie dann die Option **Beliebiges Authentifizierungsprotokoll verwenden**.

  ![Delegierungseinstellungen](./media/application-proxy-remote-sharepoint/remote-sharepoint-delegation-box.png)

5. Klicken Sie auf die Schaltfläche **Hinzufügen**, klicken Sie auf **Benutzer oder Computer**, und suchen Sie das Dienstkonto.

  ![Hinzufügen des SPN für das Dienstkonto](./media/application-proxy-remote-sharepoint/remote-sharepoint-users-computers.png)

6. Wählen Sie in der Liste der SPNs denjenigen aus, den Sie zuvor für das Dienstkonto erstellt haben.
7. Klicken Sie auf **OK**. Klicken Sie erneut auf **OK**, um die Änderung zu speichern.

## <a name="step-2-enable-remote-access-to-sharepoint"></a>Schritt 2: Aktivieren des Remotezugriffs auf SharePoint

Nachdem Sie SharePoint für Kerberos aktiviert und KCD konfiguriert haben, können Sie das einmalige Anmelden für SharePoint einrichten. Anschließend können Sie die SharePoint-Farm mit dem Connector für den Remotezugriff über den Azure AD-Anwendungsproxy veröffentlichen.

Zum Ausführen der folgenden Schritte müssen Sie im Azure Active Directory-Konto Mitglied der Rolle „Globaler Administrator“ sein.

1. Melden Sie sich beim [Azure-Portal](https://manage.windowsazure.com) an, und suchen Sie Ihren Azure AD-Mandanten.
2. Klicken Sie auf **Anwendungen** und dann auf **Hinzufügen**.
3. Wählen Sie **Eine Anwendung veröffentlichen, die von außerhalb Ihres Netzwerks aufgerufen werden kann**. Falls diese Option nicht angezeigt wird, sollten Sie sicherstellen, dass im Mandanten Azure AD Basic oder Premium eingerichtet ist.
4. Füllen Sie die einzelnen Optionen wie folgt aus:
 * **Name**: Verwenden Sie einen beliebigen Wert, z.B. **SharePoint**.
 * **Interne URL**: Dies ist die URL der internen SharePoint-Website, z.B. **https://SharePoint/**. Achten Sie in diesem Beispiel darauf, dass Sie **https** verwenden.
 * **Präauthentifizierungsmethode**: Wählen Sie die Option **Azure Active Directory**.

  ![Optionen beim Hinzufügen einer Anwendung](./media/application-proxy-remote-sharepoint/remote-sharepoint-add-application.png)

5. Klicken Sie nach dem Veröffentlichen der App auf die Registerkarte **Konfigurieren**.
6. Scrollen Sie nach unten zur Option **URL in Headern übersetzen**. Der Standardwert lautet **JA**. Ändern Sie ihn zu **NEIN**.

 SharePoint verwendet den Wert _Hostheader_, um die Website zu suchen. Es generiert auch Links auf der Grundlage dieses Werts. Hiermit wird Folgendes sichergestellt: Bei allen von SharePoint generierten Links handelt es sich um eine veröffentlichte URL, für die die Verwendung der externen URL korrekt festgelegt ist. Wenn der Wert auf **JA** festgelegt wird, kann der Connector die Anforderung auch an die Back-End-Anwendung weiterleiten. Wenn der Wert jedoch auf **NEIN** festgelegt wird, bedeutet dies, dass der Connector den internen Hostnamen nicht sendet. Stattdessen sendet der Connector den Hostheader als veröffentlichte URL an die Back-End-Anwendung.

 Um sicherzustellen, dass diese URL von SharePoint akzeptiert wird, müssen Sie eine weitere Konfiguration für den SharePoint-Server durchführen. Dies erledigen Sie im nächsten Abschnitt.

7. Ändern Sie **Interne Authentifizierungsmethode** zu **Integrierte Windows-Authentifizierung**. Wenn für Ihren Azure AD-Mandanten in der Cloud ein anderer UPN als in der lokalen Umgebung verwendet wird, sollten Sie beachten, dass auch **Delegierte Identität für Anmeldung** aktualisiert werden muss.
8. Legen Sie **Interner Anwendungs-SPN** auf den zuvor festgelegten Wert fest. Beispiel: **http/sharepoint.demo.o365identity.us**.
9. Weisen Sie die Anwendung Ihren Zielbenutzern zu.

Ihre Anwendung sollte in etwa wie folgt aussehen:

  ![Fertige Anwendung](./media/application-proxy-remote-sharepoint/remote-sharepoint-internal-application-spn.png)

## <a name="step-3-ensure-that-sharepoint-knows-about-the-external-url"></a>Schritt 3: Sicherstellen, dass SharePoint über die externe URL informiert ist

Mit dem letzten Schritt stellen Sie sicher, dass SharePoint die Website anhand der externen URL finden kann, um Links basierend auf dieser externen URL zu erstellen. Hierfür konfigurieren Sie alternative Zugriffszuordnungen für die SharePoint-Website.

1. Öffnen Sie die Website **SharePoint 2013-Zentraladministration**.
2. Wählen Sie unter **Systemeinstellungen** die Option **Alternative Zugriffszuordnungen konfigurieren**.

 Das Feld **Alternative Zugriffszuordnungen** wird geöffnet.

  ![Feld „Alternative Zugriffszuordnungen“](./media/application-proxy-remote-sharepoint/remote-sharepoint-alternate-access1.png)

3. Wählen Sie in der Dropdownliste neben **Alternate Access Mapping Collection** (Alternative Zugriffszuordnungssammlung) die Option **Change Alternate Access Mapping Collection** (Alternative Zugriffszuordnungssammlung ändern).
4. Wählen Sie Ihre Website aus, z.B. **SharePoint – 80**.

  ![Auswählen einer Website](./media/application-proxy-remote-sharepoint/remote-sharepoint-alternate-access2.png)

5. Sie können wählen, ob Sie die veröffentlichte URL entweder als interne URL oder als öffentliche URL hinzufügen möchten. In diesem Beispiel wird eine öffentliche URL als Extranet verwendet.
6. Klicken Sie unter dem Pfad **Extranet** auf **Öffentliche URLs bearbeiten**, und geben Sie dann den Pfad für die veröffentlichte Anwendung ein (wie im vorherigen Schritt). Beispiel: **https://sharepoint-iddemo.msappproxy.net**.

  ![Eingeben des Pfads](./media/application-proxy-remote-sharepoint/remote-sharepoint-alternate-access3.png)

7. Klicken Sie auf **Speichern**.

 Sie können auf die SharePoint-Website jetzt extern über den Azure AD-Anwendungsproxy zugreifen.

## <a name="next-steps"></a>Nächste Schritte

- [Bereitstellen von sicherem Remotezugriff auf lokale Anwendungen](active-directory-application-proxy-get-started.md)
- [Grundlegendes zu Azure AD-Anwendungsproxyconnectors](application-proxy-understand-connectors.md)
- [Publishing SharePoint 2016 and Office Online Server with Azure AD Application Proxy (Veröffentlichen von SharePoint 2016 und Office Online Server mit dem Azure AD-Anwendungsproxy)](https://blogs.technet.microsoft.com/dawiese/2016/06/09/publishing-sharepoint-2016-and-office-online-server-with-azure-ad-application-proxy/)

