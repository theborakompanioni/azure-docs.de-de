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
ms.date: 09/06/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.translationtype: HT
ms.sourcegitcommit: eeed445631885093a8e1799a8a5e1bcc69214fe6
ms.openlocfilehash: 2b4ad3e7bda1346e606b2c185c204154b8f19f87
ms.contentlocale: de-de
ms.lasthandoff: 09/07/2017

---

# <a name="enable-remote-access-to-sharepoint-with-azure-ad-application-proxy"></a>Aktivieren des Remotezugriffs auf SharePoint per Azure AD-Anwendungsproxy

In diesem Artikel wird beschrieben, wie Sie einen lokalen SharePoint-Server mit dem Azure Active Directory (Azure AD)-Anwendungsproxy integrieren.

Führen Sie die Abschnitte in diesem Artikel Schritt für Schritt aus, um den Remotezugriff auf SharePoint mit dem Azure AD-Anwendungsproxy zu aktivieren.

## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie in Ihrer Umgebung bereits über SharePoint 2013 oder höher verfügen. Beachten Sie auch die folgenden erforderlichen Voraussetzungen:

* SharePoint enthält native Kerberos-Unterstützung. Aus diesem Grund können Benutzer, die über den Azure AD-Anwendungsproxy per Remoteverbindung auf interne Websites zugreifen, davon ausgehen, dass sie das einmalige Anmelden (Single Sign-On, SSO) nutzen können.

* Dieses Szenario umfasst einige Konfigurationsänderungen an Ihrer SharePoint Server-Instanz. Wir empfehlen Ihnen die Verwendung einer Stagingumgebung. Auf diese Weise können Sie zuerst Updates an Ihrem Stagingserver vornehmen und dann einen Testzyklus durchführen, bevor die Produktionsphase beginnt.

* Für die veröffentlichte URL ist SLL erforderlich. Auf Ihrer internen Website muss SSL aktiviert sein, um sicherzustellen, dass Links richtig gesendet bzw. zugeordnet werden. Wenn Sie SSL nicht konfiguriert haben, finden Sie die Anweisungen unter [Configure SSL for SharePoint 2013 (Konfigurieren von SSL für SharePoint 2013)](https://blogs.msdn.microsoft.com/fabdulwahab/2013/01/20/configure-ssl-for-sharepoint-2013). Stellen Sie außerdem sicher, dass das von Ihnen ausgestellte Zertifikat für den Connectorcomputer vertrauenswürdig ist. (Das Zertifikat muss nicht öffentlich ausgestellt sein.)

## <a name="step-1-set-up-single-sign-on-to-sharepoint"></a>Schritt 1: Einrichten des einmaligen Anmeldens (SSO) für SharePoint

Für lokale Anwendungen, für die die Windows-Authentifizierung verwendet wird, können Sie einmaliges Anmelden (SSO) über das Kerberos-Authentifizierungsprotokoll und das Feature „Eingeschränkte Kerberos-Delegierung“ (Kerberos Constrained Delegation, KCD) erreichen. Bei Konfiguration von KCD kann mit dem Anwendungsproxyconnector ein Windows-Token für einen Benutzer abgerufen werden. Dies gilt auch, wenn sich der Benutzer nicht direkt bei Windows angemeldet hat. Weitere Informationen zu KCD finden Sie unter [Eingeschränkte Kerberos-Delegierung: Übersicht](https://technet.microsoft.com/library/jj553400.aspx).

Um KCD für einen SharePoint-Server einzurichten, verwenden Sie die Verfahren in den folgenden Abschnitten:

### <a name="ensure-that-sharepoint-is-running-under-a-service-account"></a>Sicherstellen, dass SharePoint unter einem Dienstkonto ausgeführt wird

Stellen Sie zuerst sicher, dass SharePoint unter einem definierten Dienstkonto ausgeführt wird, und nicht unter einem lokalen System, lokalen Dienst oder Netzwerkdienst. Führen Sie dies durch, damit Sie die Dienstprinzipalnamen (SPNs) einem gültigen Konto zuordnen können. Mithilfe von SPNs können vom Kerberos-Protokoll verschiedene Dienste identifiziert werden. Außerdem wird das Konto für die spätere Konfiguration von KCD benötigt.

> [!NOTE]
Sie müssen über ein vorab erstelltes Azure AD-Konto für den Dienst verfügen. Es ist ratsam, die automatische Kennwortänderung zuzulassen. Weitere Informationen zu allen Schritten und zur Problembehandlung finden Sie unter [Konfigurieren der automatischen Kennwortänderung in SharePoint 2013](https://technet.microsoft.com/library/ff724280.aspx).

Führen Sie die folgenden Schritte aus, um sicherzustellen, dass Ihre Websites unter einem definierten Dienstkonto ausgeführt werden:

1. Öffnen Sie die Website **SharePoint 2013-Zentraladministration**.
2. Navigieren Sie zu **Sicherheit**, und wählen Sie die Option **Dienstkonten konfigurieren**.
3. Wählen Sie **Webanwendungspool – SharePoint – 80**. Die Optionen können je nach Name Ihres Webpools oder bei der standardmäßigen Verwendung von SSL leicht variieren.

  ![Optionen für das Konfigurieren eines Dienstkontos](./media/application-proxy-remote-sharepoint/service-web-application.png)

4. Wenn das Feld **Wählen Sie ein Konto für diese Komponente aus** auf **Lokaler Dienst** oder **Netzwerkdienst** festgelegt ist, müssen Sie ein Konto erstellen. Wenn nicht, müssen Sie nichts mehr tun und können mit dem nächsten Abschnitt fortfahren.
5. Wählen Sie die Option **Neues verwaltetes Konto registrieren**. Nachdem das Konto erstellt wurde, müssen Sie den **Webanwendungspool** festlegen, bevor Sie das Konto verwenden können.

### <a name="configure-sharepoint-for-kerberos"></a>Konfigurieren von SharePoint für Kerberos

KCD wird verwendet, um das einmalige Anmelden auf dem SharePoint-Server zu ermöglichen.

Gehen Sie wie folgt vor, um Ihre SharePoint-Website für die Kerberos-Authentifizierung zu konfigurieren:

1. Öffnen Sie die Website **SharePoint 2013-Zentraladministration**.
2. Navigieren Sie zu **Anwendungsverwaltung**, wählen Sie **Webanwendungen verwalten**, und wählen Sie Ihre SharePoint-Website aus. In diesem Beispiel ist dies **SharePoint – 80**.

  ![Auswählen der SharePoint-Website](./media/application-proxy-remote-sharepoint/manage-web-applications.png)

3. Klicken Sie in der Symbolleiste auf **Authentifizierungsanbieter**.
4. Klicken Sie im Feld **Authentifizierungsanbieter** auf **Standardzone**, um die Einstellungen anzuzeigen.
5. Scrollen Sie im Dialogfeld **Authentifizierung bearbeiten** nach unten, bis **Anspruchsauthentifizierungstypen** zu sehen ist. Stellen Sie sicher, dass **Windows-Authentifizierung aktivieren** und **Integrierte Windows-Authentifizierung** aktiviert sind.
6. Überprüfen Sie, ob im Dropdownfeld für die integrierte Windows-Authentifizierung **Aushandeln (Kerberos)** aktiviert ist.

  ![Dialogfeld „Authentifizierung bearbeiten“](./media/application-proxy-remote-sharepoint/service-edit-authentication.png)

7. Klicken Sie unten im Dialogfeld **Authentifizierung bearbeiten** auf **Speichern**.

### <a name="set-a-service-principal-name-for-the-sharepoint-service-account"></a>Festlegen eines Dienstprinzipalnamens für das SharePoint-Dienstkonto

Vor dem Konfigurieren von KCD müssen Sie den SharePoint-Dienst identifizieren, der als das weiter oben konfigurierte Dienstkonto ausgeführt wird. Sie identifizieren den Dienst, indem Sie einen SPN (Dienstprinzipalnamen) festlegen. Weitere Informationen finden Sie unter [Dienstprinzipalnamen](https://technet.microsoft.com/library/cc961723.aspx).

Das SPN-Format lautet wie folgt:

```
<service class>/<host>:<port>
```

Im SPN-Format:

* _service class_ ist ein eindeutiger Name für den Dienst. Für SharePoint verwenden Sie **HTTP**.

* _host_ ist die vollqualifizierte Domäne oder der NetBIOS-Name des Hosts, auf dem der Dienst ausgeführt wird. Für eine SharePoint-Website muss dieser Text unter Umständen die URL der Website sein. Dies richtet sich nach der von Ihnen verwendeten IIS-Version.

* _port_ ist optional.

Falls der FQDN der SharePoint Server-Instanz wie folgt lautet:

```
sharepoint.demo.o365identity.us
```

Dann lautet der SPN:

```
HTTP/sharepoint.demo.o365identity.us demo
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

Konfigurieren Sie KCD so, dass der Azure AD-Anwendungsproxydienst Benutzeridentitäten an den SharePoint-Dienst delegieren kann. Sie konfigurieren KCD, indem Sie den Anwendungsproxyconnector so einrichten, dass er Kerberos-Tickets für Benutzer abruft, die in Azure AD authentifiziert wurden. Anschließend übergibt dieser Server den Kontext an die Zielanwendung bzw. in diesem Fall an SharePoint.

Wiederholen Sie zum Konfigurieren von KCD die folgenden Schritte für jeden Connectorcomputer:

1. Melden Sie sich als Domänenadministrator an einem Domänencontroller an, und öffnen Sie dann **Active Directory-Benutzer und -Computer**.
2. Suchen Sie nach dem Computer, auf dem der Connector ausgeführt wird. In diesem Beispiel ist dies derselbe SharePoint-Server.
3. Doppelklicken Sie auf den Computer, und klicken Sie anschließend auf die Registerkarte **Delegierung**.
4. Stellen Sie sicher, dass in den Delegierungseinstellungen **Computer bei Delegierungen angegebener Dienste vertrauen** festgelegt ist. Wählen Sie dann die Option **Beliebiges Authentifizierungsprotokoll verwenden** aus.

  ![Delegierungseinstellungen](./media/application-proxy-remote-sharepoint/delegation-box.png)

5. Klicken Sie auf die Schaltfläche **Hinzufügen**, klicken Sie auf **Benutzer oder Computer**, und suchen Sie das Dienstkonto.

  ![Hinzufügen des SPN für das Dienstkonto](./media/application-proxy-remote-sharepoint/users-computers.png)

6. Wählen Sie in der Liste der SPNs denjenigen aus, den Sie zuvor für das Dienstkonto erstellt haben.
7. Klicken Sie auf **OK**. Klicken Sie erneut auf **OK**, um die Änderung zu speichern.

## <a name="step-2-enable-remote-access-to-sharepoint"></a>Schritt 2: Aktivieren des Remotezugriffs auf SharePoint

Nachdem Sie SharePoint für Kerberos aktiviert und KCD konfigurierten haben, können Sie die SharePoint-Farm für den Remotezugriff über den Azure AD-Anwendungsproxy veröffentlichen.

1. Veröffentlichen Sie die SharePoint-Website mit den folgenden Einstellungen. Eine ausführliche Anleitung finden Sie unter [Veröffentlichen von Anwendungen mit Azure AD-Anwendungsproxy](application-proxy-publish-azure-portal.md). 
   - **Interne URL:** Dies ist die interne URL der SharePoint-Website, z.B. **https://SharePoint/**. Achten Sie in diesem Beispiel darauf, dass Sie **HTTPS** verwenden.
   - **Präauthentifizierungsmethode:** Azure Active Directory
   - **URL in Headern übersetzen:** Nein

   >[!TIP]
   >SharePoint verwendet den Wert _Hostheader_, um die Website zu suchen. Es generiert auch Links auf der Grundlage dieses Werts. Hiermit wird Folgendes erreicht: Bei allen von SharePoint generierten Links handelt es sich um eine veröffentlichte URL, für die die Verwendung der externen URL korrekt festgelegt ist. Wenn der Wert auf **JA** festgelegt wird, kann der Connector die Anforderung auch an die Back-End-Anwendung weiterleiten. Wenn der Wert jedoch auf **NEIN** festgelegt wird, bedeutet dies, dass der Connector den internen Hostnamen nicht sendet. Stattdessen sendet der Connector den Hostheader als veröffentlichte URL an die Back-End-Anwendung.

   ![Veröffentlichen von SharePoint als Anwendung](./media/application-proxy-remote-sharepoint/publish-app.png)

2. Nachdem die App veröffentlicht wurde, konfigurieren Sie die Einstellungen für einmaliges Anmelden anhand der folgenden Schritte:

   1. Wählen Sie auf der Anwendungsseite im Portal **Einmaliges Anmelden** aus.
   2. Legen Sie den Modus für einmaliges Anmelden auf **Integrierte Windows-Authentifizierung** fest.
   3. Legen Sie „Interner Anwendungs-SPN“ auf den zuvor festgelegten Wert fest. In diesem Beispiel: **http/sharepoint.demo.o365identity.us**.

   ![Konfigurieren der integrierten Windows-Authentifizierung für einmaliges Anmelden](./media/application-proxy-remote-sharepoint/configure-iwa.png)

3. Zum Abschluss der Anwendungseinrichtung wechseln Sie zum Abschnitt **Benutzer und Gruppen**. Weisen Sie dort Benutzer zu, die Zugriff auf diese Anwendung erhalten. 

## <a name="step-3-ensure-that-sharepoint-knows-about-the-external-url"></a>Schritt 3: Sicherstellen, dass SharePoint über die externe URL informiert ist

Mit dem letzten Schritt stellen Sie sicher, dass SharePoint die Website anhand der externen URL finden kann, um Links basierend auf dieser externen URL zu erstellen. Hierfür konfigurieren Sie alternative Zugriffszuordnungen für die SharePoint-Website.

1. Öffnen Sie die Website **SharePoint 2013-Zentraladministration**.
2. Wählen Sie unter **Systemeinstellungen** die Option **Alternative Zugriffszuordnungen konfigurieren**. Das Feld „Alternative Zugriffszuordnungen“ wird geöffnet.

  ![Feld „Alternative Zugriffszuordnungen“](./media/application-proxy-remote-sharepoint/alternate-access1.png)

3. Wählen Sie in der Dropdownliste neben **Alternate Access Mapping Collection** (Alternative Zugriffszuordnungssammlung) die Option **Change Alternate Access Mapping Collection** (Alternative Zugriffszuordnungssammlung ändern).
4. Wählen Sie Ihre Website aus, z.B. **SharePoint – 80**.
5. Sie können wählen, ob Sie die veröffentlichte URL entweder als interne URL oder als öffentliche URL hinzufügen möchten. In diesem Beispiel wird eine öffentliche URL als Extranet verwendet.
6. Klicken Sie unter dem Pfad **Extranet** auf **Öffentliche URLs bearbeiten**, und geben Sie dann die externe URL ein, die beim Veröffentlichen der Anwendung erstellt wurde. Beispiel: **https://sharepoint-iddemo.msappproxy.net**.

  ![Eingeben des Pfads](./media/application-proxy-remote-sharepoint/alternate-access3.png)

7. Klicken Sie auf **Speichern**.

Sie können auf die SharePoint-Website jetzt extern über den Azure AD-Anwendungsproxy zugreifen.

## <a name="next-steps"></a>Nächste Schritte

- [Arbeiten mit benutzerdefinierten Domänen im Azure AD-Anwendungsproxy](active-directory-application-proxy-custom-domains.md)
- [Grundlegendes zu Azure AD-Anwendungsproxyconnectors](application-proxy-understand-connectors.md)


