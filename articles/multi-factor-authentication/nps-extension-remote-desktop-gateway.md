---
title: Remotedesktopgateway-Integration in Azure MFA mit NPS-Erweiterung | Microsoft-Dokumentation
description: "Dieser Artikel beschreibt die Integration Ihrer Remotedesktopgateway-Infrastruktur in Azure MFA mit der Netzwerkrichtlinienserver-Erweiterung (Network Policy Server, NPS) für Microsoft Azure."
services: active-directory
keywords: Azure MFA, Integrieren von Remotedesktopgateway, Azure Active Directory, Netzwerkrichtlinienserver-Erweiterung
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: kgremban
ms.reviewer: jsnow
ms.custom: it-pro
ms.translationtype: HT
ms.sourcegitcommit: 540180e7d6cd02dfa1f3cac8ccd343e965ded91b
ms.openlocfilehash: 6ff9a341b31e5005949dcc0ecb2591060269846e
ms.contentlocale: de-de
ms.lasthandoff: 08/16/2017

---
#  <a name="integrate-your-remote-desktop-gateway-infrastructure-using-the-network-policy-server-nps-extension-and-azure-ad"></a>Integrieren Sie Ihre Remotedesktopgateway-Infrastruktur mit der Netzwerkrichtlinienserver-Erweiterung (Network Policy Server, NPS) und Azure AD

Dieser Artikel bietet nähere Informationen zur Integration Ihrer Remotedesktopgateway-Infrastruktur in Azure Multi-Factor Authentication (MFA) mit der Netzwerkrichtlinienserver-Erweiterung (Network Policy Server, NPS) für Microsoft Azure. 

Die Netzwerkrichtlinienserver-Erweiterung (NPS) für Azure ermöglicht Kunden, ihre RADIUS-Clientauthentifizierung (Remote Authentication Dial-in User Service) mit cloudbasierter [Azure Multi-Factor Authentication (MFA)](multi-factor-authentication.md) von Azure zu schützen. Diese Lösung ermöglicht die Überprüfung in zwei Schritten, um eine zweite Sicherheitsebene für Benutzeranmeldungen und Transaktionen hinzuzufügen.

Dieser Artikel enthält schrittweise Anleitungen zum Integrieren der NPS-Infrastruktur in Azure MFA mithilfe der NPS-Erweiterung für Azure. Dies ermöglicht eine sichere Überprüfung von Benutzern, die sich bei einem Remotedesktopgateway anmelden. 

Die Netzwerkrichtlinien- und Zugriffsdienste (Network Policy and Access Services, NPS) bieten Organisationen folgende Möglichkeiten:
* Definieren zentraler Orte für die Verwaltung und Steuerung von Netzwerkanforderungen, um Folgendes anzugeben: wer eine Verbindung herstellen kann, zu welchen Tageszeiten Verbindungen zugelassen sind, die Dauer der Verbindungen und die Sicherheitsstufe, die Clients verwenden müssen, um eine Verbindung herstellen zu können usw. Anstatt diese Richtlinien auf jedem VPN oder Remotedesktopgateway-Server (RD) anzugeben, können diese Richtlinien einmal an einem zentralen Ort angegeben werden. Das RADIUS-Protokoll ermöglicht die zentralisierte Authentifizierung, Autorisierung und Ressourcenerfassung (Authentication, Authorization, Accounting – AAA). 
* Richten Sie Netzwerkzugriffsschutz-Clientintegritätsrichtlinien (Network Access Protection, NAP) ein, die bestimmen, ob Geräten uneingeschränkter oder eingeschränkter Zugriff auf Netzwerkressourcen gewährt wird, und erzwingen Sie deren Durchsetzung.
* Bieten Sie eine Möglichkeit zum Erzwingen der Authentifizierung und Autorisierung für den Zugriff auf 802.1x-fähige Drahtloszugriffspunkte und Ethernet-Switches.    

In der Regel verwenden Organisationen NPS (RADIUS) zur Vereinfachung und Zentralisierung der Verwaltung von VPN-Richtlinien. Viele Organisationen verwenden NPS jedoch auch zur Vereinfachung und Zentralisierung der Verwaltung der RD-Verbindungsautorisierungsrichtlinien (RD-CAPs, Remote Desktop Connection Authorization Policies). 

Organisationen können NPS auch in Azure MFA integrieren, um die Sicherheit zu erhöhen und ein hohes Maß an Kompatibilität bereitzustellen. Dadurch wird sichergestellt, dass Benutzer die Überprüfung in zwei Schritten zur Anmeldung beim Remotedesktopgateway einrichten. Damit Benutzern Zugriff gewährt wird, müssen sie die von ihnen festgelegte Kombination aus Benutzername und Kennwort angeben. Diese Informationen müssen vertrauenswürdig und dürfen nicht problemlos duplizierbar sein, z.B. eine Mobiltelefonnummer, Festnetznummer, Anwendung auf einem mobilen Gerät usw.

Vor der Verfügbarkeit der NPS-Erweiterung für Azure mussten Kunden, die die Überprüfung in zwei Schritten für integrierte NPS- und Azure MFA-Umgebungen implementieren wollten, einen separaten MFA-Server in der lokalen Umgebung konfigurieren und verwalten, wie in [Remotedesktopgateway und Azure Multi-Factor Authentication-Server mithilfe von RADIUS](multi-factor-authentication-get-started-server-rdg.md) dokumentiert.

Die Verfügbarkeit der NPS-Erweiterung für Azure bietet Organisationen jetzt die Möglichkeit, entweder eine lokal basierte oder eine cloudbasierte MFA-Lösung zum Schützen der RADIUS-Clientauthentifizierung bereitzustellen.

## <a name="authentication-flow"></a>Authentifizierungsablauf

Damit Benutzern der Zugriff auf Netzwerkressourcen über ein Remotedesktopgateway erteilt wird, müssen sie die in einer Remotedesktop-Verbindungsautorisierungsrichtlinie (RD-CAP) und einer Remotedesktop-Ressourcenautorisierungsrichtlinie (RD Resource Authorization Policy, RD-RAP) angegebenen Bedingungen erfüllen. RD-CAPs geben an, wer zum Herstellen einer Verbindung mit RD-Gateways autorisiert ist. RD-RAPs geben die Netzwerkressourcen an, wie z.B. Remotedesktops oder Remote-Apps, mit denen der Benutzer über das RD-Gateway eine Verbindung mit zulässig ist. 

Ein RD-Gateway kann konfiguriert werden, um einen zentralen Richtlinienspeicher für RD-CAPs zu verwenden. RD-RAPs können keine zentrale Richtlinie verwenden, da sie auf dem RD-Gateway verarbeitet werden. Ein Beispiel eines RD-Gateways, das zur Verwendung eines zentralen Richtlinienspeichers für RD-CAPs konfiguriert ist, ist ein RADIUS-Client einer anderen NPS-Serverinstanz, die als zentraler Richtlinienspeicher dient.

Wenn die NPS-Erweiterung für Azure in NPS und Remotedesktopgateway integriert ist, lautet der erfolgreiche Authentifizierungsablauf wie folgt:

1. Der Remotedesktopgateway-Server empfängt eine Authentifizierungsanforderung von einem Remotedesktop-Benutzer zum Herstellen der Verbindung mit einer Ressource, z.B. einer Remotedesktopsitzung. Als RADIUS-Client fungierend konvertiert der Remotedesktopgateway-Server die Anforderung in eine RADIUS-Zugriffsanforderungsnachricht und sendet die Nachricht an den RADIUS-Server (NPS), auf dem die NPS-Erweiterung installiert ist. 
2. Die Kombination aus Benutzername und Kennwort wird in Active Directory überprüft, und der Benutzer ist authentifiziert.
3. Wenn alle Bedingungen entsprechend den Angaben in der NPS-Verbindungsanforderung und den Netzwerkrichtlinien erfüllt sind (z.B. Uhrzeit- oder Gruppenmitgliedschaftseinschränkungen), löst die NPS-Erweiterung eine Anforderung der sekundären Authentifizierung mit Azure MFA aus. 
4. Azure MFA kommuniziert mit Azure AD, ruft die Details des Benutzers ab und führt die sekundäre Authentifizierung mithilfe der Überprüfungsmethode aus, die vom Benutzer konfiguriert ist (Textnachricht, mobile App usw.). 
5. Bei erfolgreicher Authentifizierung durch MFA kommuniziert Azure MFA das Ergebnis an die NPS-Erweiterung.
6. Der NPS-Server, auf dem die Erweiterung installiert ist, sendet eine RADIUS-Zugriffsakzeptierungsnachricht für die RD-CAP-Richtlinie an den Remotedesktopgateway-Server.
7. Der Benutzer erhält über das RD-Gateway Zugriff auf die angeforderte Netzwerkressource.

## <a name="prerequisites"></a>Voraussetzungen
In diesem Abschnitt werden die erforderlichen Voraussetzungen zur Integration von Azure MFA in das Remotedesktopgateway erörtert. Bevor Sie beginnen, müssen folgende Voraussetzungen erfüllt sein:  

* Remotedesktopdienste-Infrastruktur (Remote Desktop Services, RDS)
* Azure MFA-Lizenz
* Windows Server-Software
* Netzwerkrichtlinien- und Zugriffsdienste-Rolle (Network Policy and Access Services, NPS)
* Azure AD synchronisiert mit lokalem AD 
* GUID-ID von Azure Active Directory

### <a name="remote-desktop-services-rds-infrastructure"></a>Remotedesktopdienste-Infrastruktur (Remote Desktop Services, RDS)
Eine funktionsfähige Remotedesktopdienste-Infrastruktur (RDS) muss vorhanden sein. Wenn dies nicht der Fall ist, können Sie diese Infrastruktur schnell in Azure mithilfe der folgenden Schnellstartvorlage erstellen: [Create Remote Desktop Session Collection deployment](https://github.com/Azure/azure-quickstart-templates/tree/ad20c78b36d8e1246f96bb0e7a8741db481f957f/rds-deployment) (Erstellen der Bereitstellung einer Remotedesktopsitzungs-Sammlung). 

Wenn Sie eine lokale RDS-Infrastruktur schnell manuell zu Testzwecken erstellen möchten, führen Sie die Schritte zu deren Bereitstellung aus. 
**Erfahren Sie mehr**: [Seamlessly deploy RDS with ARM and Azure Marketplace](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-in-azure) (Nahtlose RDS-Bereitstellung mit ARM und Azure Marketplace) und [Deploy your Remote Desktop environment](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-deploy-infrastructure) (Bereitstellen Ihrer Remotedesktopumgebung). 

### <a name="licenses"></a>Lizenzen
Eine Lizenz für Azure MFA ist erforderlich, die über ein Azure AD Premium-, Enterprise Mobility plus Security-Abonnement (EMS) oder MFA-Abonnement verfügbar ist. Weitere Informationen finden Sie unter [Beziehen von Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md). Zu Testzwecken können Sie ein Testabonnement verwenden.

### <a name="software"></a>Software
Die NPS-Erweiterung erfordert Windows Server 2008 R2 SP1 oder höher mit installiertem NPS-Rollendienst. Alle Schritte in diesem Abschnitt wurden mit Windows Server 2016 ausgeführt.

### <a name="network-policy-and-access-services-nps-role"></a>Netzwerkrichtlinien- und Zugriffsdienste-Rolle (Network Policy and Access Services, NPS)
Der NPS-Rollendienst bietet sowohl die RADIUS-Server- und -Clientfunktionalität als auch den Netzwerkzugriffsrichtlinien-Integritätsdienst. Diese Rolle muss auf mindestens zwei Computern in Ihrer Infrastruktur installiert werden: dem Remotedesktopgateway und einem anderen Mitgliedsserver oder Domänencontroller. Standardmäßig ist die Rolle bereits auf dem Computer vorhanden, der als Remotedesktopgateway konfiguriert ist.  Sie müssen auch die NPS-Rolle auf mindestens einem anderen Computer installieren, z.B. einen Domänencontroller oder Mitgliedsserver.

Informationen zum Installieren des NPS-Rollendiensts für Windows Server 2012 oder älter finden Sie unter [Install a NAP Health Policy Server](https://technet.microsoft.com/library/dd296890.aspx) (Installieren eines NAP-Integritätsrichtlinienservers). Eine Beschreibung der bewährten Methoden für NPS einschließlich der Empfehlung zum Installieren von NPS auf einem Domänencontroller finden Sie unter [Best Practices for NPS](https://technet.microsoft.com/library/cc771746) (Bewährte Methoden für NPS).

### <a name="azure-active-directory-synched-with-on-premises-active-directory"></a>Azure Active Directory synchronisiert mit der lokalen Active Directory-Instanz 
Um die NPS-Erweiterung zu verwenden, müssen lokale Benutzer mit Azure AD synchronisiert und für MFA aktiviert werden. Dieser Abschnitt setzt voraus, dass lokale Benutzer mithilfe von AD Connect mit Azure AD synchronisiert werden. Informationen zu Azure AD Connect finden Sie unter [Integrieren Ihrer lokalen Identitäten in Azure Active Directory](../active-directory/connect/active-directory-aadconnect.md). 

### <a name="azure-active-directory-guid-id"></a>GUID-ID von Azure Active Directory
Um NPS zu installieren, müssen Sie die GUID von Azure AD kennen. Nachfolgend sind die Anweisungen für die Suche nach der GUID von Azure AD aufgeführt.

## <a name="configure-multi-factor-authentication"></a>Konfigurieren der Multi-Factor Authentication 
Dieser Abschnitt enthält Anweisungen für die Integration von Azure MFA in Remotedesktopgateway. Als Administrator müssen Sie den Azure MFA-Dienst konfigurieren, bevor Benutzer selbst ihre Multi-Factor-Geräte oder Anwendungen registrieren können.

Führen Sie die Schritte in [Erste Schritte mit Azure Multi-Factor Authentication in der Cloud](multi-factor-authentication-get-started-cloud.md) aus, um MFA für Ihre Azure AD-Benutzer zu aktivieren. 

### <a name="configure-accounts-for-two-step-verification"></a>Konfigurieren von Konten für die Überprüfung in zwei Schritten
Sobald ein Konto für MFA aktiviert ist, können Sie sich erst dann bei Ressourcen anmelden, die der MFA-Richtlinie unterliegen, wenn sie erfolgreich mit der Überprüfung in zwei Schritten ein vertrauenswürdiges Gerät für den zweiten Authentifizierungsfaktor konfiguriert haben.

Führen Sie die Schritte in [Was ist Azure Multi-Factor Authentication?](./end-user/multi-factor-authentication-end-user.md) aus, um Ihre Geräte ordnungsgemäß mit Ihrem Benutzerkonto für MFA zu konfigurieren.

## <a name="install-and-configure-nps-extension"></a>Installieren und Konfigurieren der NPS-Erweiterung
Dieser Abschnitt enthält Anweisungen zum Konfigurieren der RDS-Infrastruktur zur Verwendung der Azure MFA für die Clientauthentifizierung mit dem Remotedesktopgateway.

### <a name="acquire-azure-active-directory-guid-id"></a>Abrufen der GUID-ID von Azure Active Directory

Im Rahmen der Konfiguration der NPS-Erweiterung müssen Sie Administratoranmeldeinformationen und Azure AD-ID für Ihren Azure AD-Mandanten angeben. Die folgenden Schritte veranschaulichen das Abrufen der Mandanten-ID.

1. Melden Sie sich im [Azure-Portal](https://portal.azure.com) als globaler Administrator des Azure-Mandanten an.
2. Wählen Sie im linken Navigationsbereich das **Azure Active Directory**-Symbol.
3. Wählen Sie **Eigenschaften** aus.
4. Klicken Sie auf dem Blatt „Eigenschaften“ neben der Verzeichnis-ID wie unten dargestellt auf das **Kopieren**-Symbol um die ID in die Zwischenablage kopieren.

 ![Eigenschaften](./media/nps-extension-remote-desktop-gateway/image1.png)

### <a name="install-the-nps-extension"></a>Installieren der NPS-Erweiterung
Installieren Sie die NPS-Erweiterung auf einem Server, auf dem die Netzwerkrichtlinien- und Zugriffsdienste-Rolle (NPS) installiert ist. Diese fungiert als RADIUS-Server für den Entwurf. 

>[!Important]
> Installieren Sie auf keinen Fall die NPS-Erweiterung auf Ihrem Remotedesktopgateway-Server.
> 

1. Laden Sie die [NPS-Erweiterung](https://aka.ms/npsmfa) herunter. 
2. Kopieren Sie die ausführbare Setupdatei (NpsExtnForAzureMfaInstaller.exe) auf den NPS-Server.
3. Doppelklicken Sie auf dem NPS-Server auf **NpsExtnForAzureMfaInstaller.exe**. Klicken Sie nach Aufforderung auf **Ausführen**.
4. Überprüfen Sie im Dialogfeld „NPS-Erweiterung für Azure MFA“ die Softwarelizenzbedingungen, aktivieren Sie **Ich stimme den Lizenzbedingungen zu**, und klicken Sie auf **Installieren**.
 
  ![Azure MFA-Setup](./media/nps-extension-remote-desktop-gateway/image2.png)

5. Klicken Sie im Dialogfeld „NPS-Erweiterung für Azure MFA“ auf „Schließen“. 

  ![NPS-Erweiterung für Azure MFA](./media/nps-extension-remote-desktop-gateway/image3.png)

### <a name="configure-certificates-for-use-with-the-nps-extension-using-a-powershell-script"></a>Konfigurieren von Zertifikaten für die Verwendung mit der NPS-Erweiterung mithilfe eines PowerShell-Skripts
Als Nächstes müssen Sie Zertifikate für die Verwendung durch die NPS-Erweiterung konfigurieren, um die sichere Kommunikation zu gewährleisten. Die NPS-Komponenten umfassen ein Windows PowerShell-Skript, das ein selbstsigniertes Zertifikat zur Verwendung mit NPS konfiguriert. 

Dieses Skript führt folgende Aktionen aus:

* Erstellen eines selbstsignierten Zertifikats
* Zuordnen des öffentlichen Schlüssels des Zertifikats zum Dienstprinzipal in Azure AD
* Speichern des Zertifikats im Speicher des lokalen Computers
* Gewähren des Zugriffs auf den privaten Schlüssel des Zertifikats für den Netzwerkbenutzer
* Neustart des Netzwerkrichtlinienserver-Diensts

Wenn Sie Ihre eigenen Zertifikate verwenden möchten, müssen Sie den öffentlichen Teil Ihres Zertifikats dem Dienstprinzipal in Azure AD zuordnen usw.

Um das Skript zu verwenden, geben Sie die Erweiterung mit Ihren Azure AD-Administratoranmeldeinformationen und die Azure AD-Mandanten-ID ein, die Sie zuvor kopiert haben. Führen Sie das Skript auf jedem NPS-Server aus, auf dem Sie die NPS-Erweiterung installiert haben. Gehen Sie wie folgt vor:

1. Öffnen Sie eine administrative Windows PowerShell-Eingabeaufforderung.
2. Geben Sie an der PowerShell-Eingabeaufforderung **cd ‘c:\Programme\Microsoft\AzureMfa\Config’** ein, und drücken Sie die **EINGABETASTE**.
3. Geben Sie _.\AzureMfsNpsExtnConfigSetup.ps1_ ein, und drücken Sie die **EINGABETASTE**. Das Skript überprüft, ob das Azure Active Directory PowerShell-Modul installiert ist. Wenn es nicht installiert ist, installiert das Skript das Modul für Sie.

  ![Azure AD PowerShell](./media/nps-extension-remote-desktop-gateway/image4.png)
  
4. Nachdem das Skript die Installation des PowerShell-Moduls überprüft hat, wird das Dialogfeld des Azure Active Directory PowerShell-Moduls angezeigt. Geben Sie im Dialogfeld Ihre Azure AD-Administratoranmeldeinformationen und das Kennwort ein, und klicken Sie auf **Anmelden**.

  ![Öffnen des PowerShell-Kontos](./media/nps-extension-remote-desktop-gateway/image5.png)

5. Wenn Sie aufgefordert werden, fügen Sie die Mandanten-ID ein, die Sie vorher in die Zwischenablage kopiert haben, und drücken Sie die **EINGABETASTE**.

  ![Eingeben der Mandanten-ID](./media/nps-extension-remote-desktop-gateway/image6.png)

6. Das Skript erstellt ein selbstsigniertes Zertifikat und führt andere Änderungen an der Konfiguration durch. Die Ausgabe sollte dem folgenden Bild ähneln.

  ![Selbstsigniertes Zertifikat](./media/nps-extension-remote-desktop-gateway/image7.png)

## <a name="configure-nps-components-on-remote-desktop-gateway"></a>Konfigurieren von NPS-Komponenten auf dem Remotedesktopgateway
In diesem Abschnitt konfigurieren Sie die Verbindungsautorisierungsrichtlinien für das Remotedesktopgateway und andere RADIUS-Clienteinstellungen.

Der Authentifizierungsablauf erfordert, dass RADIUS-Nachrichten zwischen dem Remotedesktopgateway und dem NPS-Server, auf dem die NPS-Serverinstanz installiert ist, ausgetauscht werden. Dies bedeutet, dass Sie RADIUS-Clienteinstellungen sowohl auf dem Remotedesktopgateway als auch dem NPS-Server, auf dem die NPS-Erweiterung installiert ist, konfigurieren müssen. 

### <a name="configure-remote-desktop-gateway-connection-authorization-policies-to-use-central-store"></a>Konfigurieren von Verbindungsautorisierungsrichtlinien für das Remotedesktopgateway zur Verwendung im zentralen Speicher
Verbindungsautorisierungsrichtlinien für Remotedesktop (RD-CAPs) geben die Anforderungen für das Herstellen einer Verbindung mit einem Remotedesktopgateway-Server an. RD-CAPs können lokal gespeichert werden (Standard) oder in einem zentralen RD-CAP-Speicher, wo NPS ausgeführt wird. Um die Integration von Azure MFA in RDS zu konfigurieren, müssen Sie die Verwendung eines zentralen Speichers angeben.

1. Öffnen Sie auf dem RD-Gatewayserver den **Server-Manager**. 
2. Klicken Sie auf der Menüleiste auf **Tools**, zeigen Sie auf **Remotedesktopdienste**, und klicken Sie dann auf **Remotedesktopgateway-Manager**.

  ![Remotedesktopdienste](./media/nps-extension-remote-desktop-gateway/image8.png)

3. Klicken Sie im RD-Gateway-Manager mit der rechten Maustaste auf **\[Servername\] (Lokal)**, und klicken Sie auf **Eigenschaften**.

  ![Servername](./media/nps-extension-remote-desktop-gateway/image9.png)

4. Wählen Sie im Dialogfeld „Eigenschaften“ die Registerkarte **RD-CAP-Speicher**.
5. Wählen Sie auf der Registerkarte „RD-CAP-Speicher“ die Option **Zentraler NPS-Server**. 
6. Geben Sie in das Feld **Name oder IP-Adresse für den Server mit NPS eingeben:** die IP-Adresse bzw. den Servernamen des Servers ein, auf dem Sie die NPS-Erweiterung installiert haben.

  ![Name oder IP-Adresse eingeben](./media/nps-extension-remote-desktop-gateway/image10.png)
  
7. Klicken Sie auf **Hinzufügen**.
8. Geben Sie im Dialogfeld **Gemeinsamer geheimer Schlüssel** einen gemeinsamen geheimen Schlüssel ein, und klicken Sie dann auf **OK**. Denken Sie daran, diesen gemeinsamen geheimen Schlüssel zu notieren und die Notiz an einem sicheren Ort aufzubewahren.

 >[!NOTE]
 >Mit dem gemeinsamen geheimen Schlüssel wird eine Vertrauensstellung zwischen den RADIUS-Servern und -Clients hergestellt. Erstellen Sie ein langes und komplexes Kennwort.
 >

 ![Gemeinsamer geheimer Schlüssel](./media/nps-extension-remote-desktop-gateway/image11.png)

9. Klicken Sie auf **OK**, um das Dialogfeld zu schließen.

### <a name="configure-radius-timeout-value-on-remote-desktop-gateway-nps"></a>Konfigurieren des RADIUS-Timeoutwerts für Remotedesktopgateway-NPS
Um sicherzustellen, dass genügend Zeit zum Überprüfen der Anmeldeinformationen des Benutzers, Ausführen der Überprüfung in zwei Schritten, Empfangen von Antworten und Antworten auf RADIUS-Nachrichten zur Verfügung steht, müssen Sie den RADIUS-Timeoutwert anpassen.

1. Klicken Sie im RD-Gatewayserver im Server-Manager auf **Tools** und dann auf **Netzwerkrichtlinienserver**. 
2. Erweitern Sie in der Konsole **NPS (Lokal)** **RADIUS-Clients und Server**, und wählen Sie **Remote-RADIUS-Server**.

 ![Remote-RADIUS-Server](./media/nps-extension-remote-desktop-gateway/image12.png)

3. Doppelklicken Sie im Detailbereich auf **TS-GATEWAYSERVERGRUPPE**.

 >[!NOTE]
 >Diese RADIUS-Server-Gruppe wurde erstellt, als Sie den zentralen Server für die NPS-Richtlinien konfigurierten. Das RD-Gateway leitet RADIUS-Nachrichten an diesen Server weiter, bzw. an eine Gruppe von Servern, wenn mehrere Server vorhanden sind.
 >

4. Wählen Sie im Dialogfeld **TS-GATEWAYSERVERGRUPPEN-Eigenschaften** die IP-Adresse oder den Namen des NPS-Servers, den Sie zum Speichern von RD-CAPs konfiguriert haben, und klicken Sie dann auf **Bearbeiten**. 

 ![TS-Gatewayservergruppe](./media/nps-extension-remote-desktop-gateway/image13.png)

5. Wählen Sie im Dialogfeld **RADIUS-Server bearbeiten** die Registerkarte **Lastenausgleich**.
6. Ändern Sie in der Registerkarte **Lastenausgleich** im Feld **Sekunden ohne Antwort, bevor eine Anforderung als verworfen gilt** den Standardwert von 3 in einen Wert zwischen 30 und 60 Sekunden.
7. Ändern Sie im Feld **Sekunden zwischen Anforderungen, nach denen der Server als nicht verfügbar identifiziert wird** den Standardwert von 30 Sekunden in einen Wert, der mindestens so groß ist wie der Wert, den Sie im vorherigen Schritt angegeben haben.

 ![RADIUS-Server bearbeiten](./media/nps-extension-remote-desktop-gateway/image14.png)

8.  Klicken Sie auf „OK“, um das Dialogfeld zu schließen.

### <a name="verify-connection-request-policies"></a>Überprüfen der Verbindungsanforderungsrichtlinien 
Wenn Sie das RD-Gateway zur Verwendung eines zentralen Richtlinienspeichers für Verbindungsautorisierungsrichtlinien konfigurieren, wird standardmäßig das RD-Gateway zum Weiterleiten von CAP-Anforderungen an den NPS-Server konfiguriert. Der mit der Azure MFA-Erweiterung installierte NPS-Server verarbeitet die RADIUS-Zugriffsanforderung. In den folgenden Schritten lernen Sie das Überprüfen der Standard-Verbindungsanforderungsrichtlinie. 

1. Erweitern Sie auf dem RD-Gateway in der Konsole „NPS (Lokal)“ **Richtlinien**, und wählen Sie **Verbindungsanforderungsrichtlinien**.
2. Klicken Sie mit der rechten Maustaste auf **Verbindungsanforderungsrichtlinien**, und doppelklicken Sie auf **TS-GATEWAYAUTORISIERUNGSRICHTLINIE**.
3. Klicken Sie im Dialogfeld **TS-GATEWAYAUTORISIERUNGSRICHTLINIE-Eigenschaften** auf die Registerkarte **Einstellungen**.
4. Klicken Sie auf der Registerkarte **Einstellungen** unter „Weiterleitungsverbindungsanforderung“ auf **Authentifizierung**. Der RADIUS-Client ist zum Weiterleiten von Anforderungen für die Authentifizierung konfiguriert.

 ![Authentifizierungseinstellungen](./media/nps-extension-remote-desktop-gateway/image15.png)
 
5. Klicken Sie auf **Abbrechen**. 

## <a name="configure-nps-on-the-server-where-the-nps-extension-is-installed"></a>Konfigurieren von NPS auf dem Server, auf dem die NPS-Erweiterung installiert ist
Der NPS-Server, auf dem die NPS-Erweiterung installiert ist, muss die RADIUS-Nachrichten mit dem NPS-Server auf dem Remotedesktopgateway austauschen können. Um diesen Nachrichtenaustausch zu aktivieren, müssen Sie die NPS-Komponenten auf dem Server konfigurieren, auf dem der NPS-Erweiterungsdienst installiert ist. 

### <a name="register-server-in-active-directory"></a>Registrieren des Servers in Active Directory
Die ordnungsgemäße Funktionsweise des NPS-Servers in diesem Szenario setzt seine Registrierung in Active Directory voraus.

1. Öffnen Sie den **Server-Manager**.
2. Klicken Sie im Server-Manager auf **Tools** und dann auf **Netzwerkrichtlinienserver**. 
3. Klicken Sie in der Netzwerkrichtlinienserver-Konsole mit der rechten Maustaste auf **NPS (Lokal)**, und klicken Sie dann auf **Server in Active Directory registrieren**. 
4. Klicken Sie zweimal auf **OK**.

 ![Registrieren des Servers in AD](./media/nps-extension-remote-desktop-gateway/image16.png)

5. Lassen Sie die Konsole für den nächsten Vorgang geöffnet.

### <a name="create-and-configure-radius-client"></a>Erstellen und Konfigurieren des RADIUS-Clients 
Das Remotedesktopgateway muss als RADIUS-Client des NPS-Servers konfiguriert werden. 

1. Klicken Sie auf dem NPS-Server, wo die NPS-Erweiterung installiert ist, in der **NPS (Lokal)**-Konsole mit der rechten Maustaste auf **RADIUS-Clients**, und klicken Sie auf **Neu**.

 ![Neue RADIUS-Clients](./media/nps-extension-remote-desktop-gateway/image17.png)

2. Geben Sie im Dialogfeld **Neuer RADIUS-Client** einen Anzeigenamen wie z.B. _Gateway_ ein und die IP-Adresse oder den DNS-Namen des Remotedesktopgateway-Servers. 
3. Geben Sie in den Feldern **Gemeinsamer geheimer Schlüssel** und **Gemeinsamen geheimen Schlüssel bestätigen:** den gleichen geheimen Schlüssel ein, den Sie zuvor verwendet haben.

 ![Name und Adresse](./media/nps-extension-remote-desktop-gateway/image18.png)

4. Klicken Sie auf **OK**, um das Dialogfeld „Neuer RADIUS-Client“ zu schließen.

### <a name="configure-network-policy"></a>Konfigurieren der Netzwerkrichtlinie
Denken Sie daran, dass der NPS-Server mit der Azure MFA-Erweiterung der angegebene zentrale Richtlinienspeicher für die Verbindungsautorisierungsrichtlinie (CAP) ist. Aus diesem Grund müssen Sie auf dem NPS-Server eine CAP zum Autorisieren gültiger Verbindungsanforderungen implementieren.  

1. Erweitern Sie in der Konsole „NPS (Lokal)“ **Richtlinien**, und klicken Sie auf **Netzwerkrichtlinien**.
2. Klicken Sie mit der rechten Maustaste auf **Verbindungen mit anderen Zugriffsservern**, und klicken Sie auf **Duplizieren der Richtlinie**. 

 ![Duplizieren der Richtlinie](./media/nps-extension-remote-desktop-gateway/image19.png)

3. Klicken Sie mit der rechten Maustaste auf **Kopie von Verbindungen mit anderen Zugriffsservern**, und klicken Sie auf **Eigenschaften**.

 ![Netzwerkeigenschaften](./media/nps-extension-remote-desktop-gateway/image20.png)

4. Geben Sie im Dialogfeld **Kopie der Verbindungen mit anderen Zugriffsservern** in „Richtlinienname“ einen geeigneten Namen ein, z.B. **RDG_CAP**. Aktivieren Sie **Richtlinie aktiviert**, und wählen Sie **Zugriff gewähren**. Wählen Sie optional unter „Typ des Netzwerkzugriffs“ **Remotedesktopgateway**, oder behalten Sie **Nicht angegeben** bei.

 ![Kopie von Verbindungen](./media/nps-extension-remote-desktop-gateway/image21.png)

5.  Klicken Sie auf die Registerkarte **Einschränkungen**, und aktivieren Sie **Clientverbindungen ohne Aushandlung einer Authentifizierungsmethode zulassen**.

 ![Zulassen von Clientverbindungen](./media/nps-extension-remote-desktop-gateway/image22.png)

6. Klicken Sie optional auf die Registerkarte **Bedingungen**, und fügen Sie Bedingungen hinzu, die für die Autorisierung der Verbindung erfüllt werden müssen, z.B. die Mitgliedschaft in einer bestimmten Windows-Gruppe.

 ![Bedingungen](./media/nps-extension-remote-desktop-gateway/image23.png)

7. Klicken Sie auf **OK**. Wenn Sie gefragt werden, ob Sie das entsprechende Hilfethema anzeigen möchten, klicken Sie auf **Nein**.
8. Stellen Sie sicher, dass Ihre neue Richtlinie am oberen Rand der Liste angezeigt wird, dass die Richtlinie aktiviert ist und Zugriff gewährt.

 ![Netzwerkrichtlinien](./media/nps-extension-remote-desktop-gateway/image24.png)

## <a name="verify-configuration"></a>Überprüfen der Konfiguration
Um die Konfiguration zu überprüfen, müssen Sie sich mit einem geeigneten RDP-Client beim Remotedesktopgateway anmelden. Achten Sie darauf,, ein Konto zu verwenden, das von Ihren Verbindungsautorisierungsrichtlinien zugelassen und für Azure MFA aktiviert ist. 

Wie im folgenden Bild gezeigt, können Sie die Seite **Web Access für Remotedesktop** verwenden.

![Web Access für Remotedesktop](./media/nps-extension-remote-desktop-gateway/image25.png)

Nach erfolgreicher Eingabe Ihrer Anmeldeinformationen für die primäre Authentifizierung zeigt das Dialogfeld „Remotedesktopverbindung“ wie unten dargestellt den Status „Remoteverbindung wird initiiert“ an. 

Nach Ihrer erfolgreichen Authentifizierung mit der sekundären Authentifizierungsmethode, die Sie zuvor in Azure MFA konfiguriert haben, werden Sie mit der Ressource verbunden. Falls die sekundäre Authentifizierung jedoch nicht erfolgreich ist, wird Ihnen der Zugriff auf die Ressource verweigert. 

![Initiieren der Remoteverbindung](./media/nps-extension-remote-desktop-gateway/image26.png)

Im folgenden Beispiel wird die Authenticator-App auf einem Windows Phone verwendet, um die sekundäre Authentifizierung bereitzustellen.

![Konten](./media/nps-extension-remote-desktop-gateway/image27.png)

Nachdem Sie erfolgreich mit der sekundären Authentifizierungsmethode authentifiziert wurden, werden Sie wie gewohnt beim Remotedesktopgateway angemeldet. Da Sie jedoch aufgefordert wurden, eine sekundäre Authentifizierungsmethode mit einer mobilen App auf einem vertrauenswürdigen Gerät zu verwenden, ist der Anmeldeprozess sicherer als sonst.

### <a name="view-event-viewer-logs-for-successful-logon-events"></a>Anzeigen der Protokolle der Ereignisanzeige für erfolgreiche Anmeldeereignisse
Um die erfolgreichen Anmeldeereignisse in den Protokollen der Windows-Ereignisanzeige anzuzeigen, können Sie den folgenden Windows PowerShell-Befehl zum Abfragen der Windows-Terminaldienste und Windows-Sicherheitsprotokolle ausgeben.

Verwenden Sie zur Abfrage erfolgreicher Anmeldungsereignisse in den Gatewaybetriebsprotokollen (_Ereignisanzeige\Anwendungs- und Dienstprotokolle\Microsoft\Windows\TerminalServices-Gateway\Operational_) die folgenden Befehle:

* _Get-WinEvent -Logname Microsoft-Windows-TerminalServices-Gateway/Operational_ | where {$_.ID -eq '300'} | FL 
* Dieser Befehl zeigt Windows-Ereignisse an, die belegen, dass der Benutzer die Anforderungen der Ressourcenautorisierungsrichtlinie (RD-RAP) erfüllt hat und ihm Zugriff erteilt wurde.

![Anzeigen der Ereignisanzeige](./media/nps-extension-remote-desktop-gateway/image28.png)

* _Get-WinEvent -Logname Microsoft-Windows-TerminalServices-Gateway/Operational_ | where {$_.ID -eq '200'} | FL
* Dieser Befehl zeigt Ereignisse an, die belegen, dass der Benutzer die Anforderungen der Verbindungsautorisierungsrichtlinie erfüllt hat.

![Verbindungsautorisierung](./media/nps-extension-remote-desktop-gateway/image29.png)

Sie können bei der Anzeige dieses Protokolls auch nach den Ereignis-IDs 300 und 200 filtern. Verwenden Sie zum Abfragen erfolgreicher Anmeldeereignisse in den Protokollen der Sicherheitsereignisanzeige den folgenden Befehl:

* _Get-WinEvent -Logname Security_ | where {$_.ID -eq '6272'} | FL 
* Dieser Befehl kann entweder auf dem zentralen NPS oder dem RD-Gatewayserver ausgeführt werden. 

![Erfolgreiche Anmeldeereignisse](./media/nps-extension-remote-desktop-gateway/image30.png)

Sie können auch das Sicherheitsprotokoll oder die benutzerdefinierte Ansicht der Netzwerkrichtlinien- und Zugriffsdienste anzeigen, wie unten dargestellt:

![Netzwerkrichtlinien- und Zugriffsdienste](./media/nps-extension-remote-desktop-gateway/image31.png)

Auf dem Server, auf dem Sie die NPS-Erweiterung für Azure MFA installiert haben, finden Sie spezifische Ereignisanzeige-Anwendungsprotokolle für die Erweiterung unter _Anwendungs- und Dienstprotokolle\Microsoft\AzureMfa_. 

![Ereignisanzeige-Anwendungsprotokolle](./media/nps-extension-remote-desktop-gateway/image32.png)

## <a name="troubleshoot-guide"></a>Leitfaden zur Problembehandlung

Wenn die Konfiguration nicht wie erwartet funktioniert, sollten Sie die Problembehandlung idealerweise mit der Überprüfung beginnen, ob der Benutzer für die Verwendung von Azure MFA konfiguriert ist. Lassen Sie den Benutzer eine Verbindung mit dem [Azure-Portal](https://portal.azure.com) herstellen. Wenn der Benutzer zur sekundären Überprüfung aufgefordert wird und sich erfolgreich authentifizieren kann, können Sie eine fehlerhafte Azure MFA-Konfiguration ausschließen.

Wenn Azure MFA für den Benutzer funktioniert, prüfen Sie die relevanten Ereignisprotokolle. Dazu gehören Sicherheitsereignis-, Gatewaybetriebs- und Azure MFA-Protokolle, die im vorherigen Abschnitt erläutert wurden. 

Es folgt eine Beispielausgabe des Sicherheitsprotokolls mit einem fehlerhaften Anmeldeereignis (Ereignis-ID 6273):

![Fehlerhaftes Anmeldeereignis](./media/nps-extension-remote-desktop-gateway/image33.png)

Es folgt ein verwandtes Ereignis aus den Azure MFA-Protokollen:

![Azure MFA-Protokoll](./media/nps-extension-remote-desktop-gateway/image34.png)

Zum Ausführen erweiterter Problembehandlungsoptionen nutzen Sie die NPS-Datenbankformat-Protokolldateien dort, wo der NPS-Dienst installiert ist. Diese Protokolldateien werden im Ordner _%SystemRoot%\System32\Logs_ als durch Trennzeichen getrennte Textdateien erstellt. 

Eine Beschreibung dieser Protokolldateien finden Sie unter [Interpret NPS Database Format Log Files](https://technet.microsoft.com/library/cc771748.aspx) (Interpretieren von NPS-Datenbankformat-Protokolldateien). Die Einträge in diesen Protokolldateien können schwierig zu interpretieren sein, ohne sie in eine Tabelle oder eine Datenbank zu importieren. Sie finden online mehrere IAS-Parser, die Ihnen die Interpretation der Protokolldateien erleichtern. 

Das folgende Bild zeigt die Ausgabe einer solchen herunterladbaren [Sharewareanwendung](http://www.deepsoftware.com/iasviewer). 

![Shareware-App](./media/nps-extension-remote-desktop-gateway/image35.png)

Schließlich können Sie für zusätzliche Problembehandlungsoptionen ein Protokollanalyseprogramm wie [Microsoft Message Analyzer](https://technet.microsoft.com/library/jj649776.aspx) verwenden. 

Die Abbildung unten aus dem Microsoft Message Analyzer zeigt den Netzwerkverkehr, der mit dem RADIUS-Protokoll gefiltert ist, das den Benutzernamen **Contoso\AliceC** enthält.

![Microsoft Message Analyzer](./media/nps-extension-remote-desktop-gateway/image36.png)

## <a name="next-steps"></a>Nächste Schritte
[Beziehen von Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md)

[Remotedesktop-Gateway und Azure Multi-Factor Authentication-Server mithilfe von RADIUS](multi-factor-authentication-get-started-server-rdg.md)

[Integrieren Ihrer lokalen Verzeichnisse in Azure Active Directory](../active-directory/connect/active-directory-aadconnect.md)

