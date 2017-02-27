---
title: "Webdienst der mobilen App für den Azure MFA-Server | Microsoft-Dokumentation"
description: "Die Microsoft Authenticator-App bietet eine zusätzliche Out-of-Band-Authentifizierungsoption.  Diese ermöglicht es, dass der MFA-Server Pushbenachrichtigungen an Benutzer sendet."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 6c8d6fcc-70f4-4da4-9610-c76d66635b8b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/15/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 999361daa2faebe3e88cab0b6085a938d6f40e9d
ms.openlocfilehash: d3a122d7d26635e13281b1cba450937519ed4be6


---
# <a name="getting-started-with-the-mfa-server-mobile-app-web-service"></a>Erste Schritte mit dem Webdienst der mobilen App für den MFA-Server
Die Microsoft Authenticator-App bietet eine zusätzliche Out-of-Band-Überprüfungsoption. Anstelle eines automatischen Telefonanrufs oder einer SMS sendet Azure Multi-Factor Authentication bei der Benutzeranmeldung eine Pushbenachrichtigung an die Microsoft Authenticator-App auf dem Smartphone oder Tablet des Benutzers. Der Benutzer tippt in der App einfach auf **Überprüfen** (oder gibt eine PIN ein und tippt auf „Authentifizieren“), um sich anzumelden. 

Die Verwendung einer mobilen App für die Prüfung in zwei Schritten empfiehlt sich, wenn kein zuverlässiges Mobilfunknetz zur Verfügung steht. Bei Verwendung als OAuth-Token-Generator benötigt die App keine Netzwerk- oder Internetverbindung. 

Wenn Sie das Benutzerportal auf einem anderen Server als dem Azure Multi-Factor Authentication-Server installieren möchten, sind folgende Schritte erforderlich:

1. Installieren des Webdienst-SDK
2. Installieren des Webdiensts der mobilen App
3. Konfigurieren der Einstellungen für die mobile App im Azure Multi-Factor Authentication-Server
4. Aktivieren der Microsoft Authenticator-App für Endbenutzer

## <a name="requirements"></a>Anforderungen

Zur Verwendung der Microsoft Authenticator-App müssen folgende Voraussetzungen erfüllt sein, damit die App erfolgreich mit dem Webdienst der mobilen App kommunizieren kann:

* Azure Multi-Factor Authentication-Server ab Version&6;.0
* Installieren Sie den Webdienst der mobilen App auf einem Webserver mit Internetverbindung, auf dem Microsoft® [Internetinformationsdienste (IIS) 7.x oder höher](http://www.iis.net/) ausgeführt wird.
* ASP.NET v4.0.30319 ist installiert, registriert und auf „Zulässig“ festgelegt.
* Erforderliche Rollendienste umfassen ASP.NET und IIS 6-Metabasiskompatibilität.
* Der Webdienst der mobilen App ist über eine öffentliche URL erreichbar.
* Der Webdienst der mobilen App ist durch ein SSL-Zertifikat geschützt.
* Installieren Sie das Azure Multi-Factor Authentication-Webdienst-SDK in IIS 7.x oder höher auf dem gleichen Server, auf dem auch der Azure Multi-Factor Authentication-Server installiert ist.
* Das Azure Multi-Factor Authentication-Webdienst-SDK ist durch ein SSL-Zertifikat geschützt.
* Der Webdienst der mobilen App kann eine SSL-Verbindung mit dem Azure Multi-Factor Authentication-Webdienst-SDK herstellen.
* Der Webdienst der mobilen App kann sich beim Azure Multi-Factor Authentication-Webdienst-SDK mit den Anmeldeinformationen eines Dienstkontos authentifizieren, das Mitglied der Sicherheitsgruppe „PhoneFactor Admins“ ist. Dieses Dienstkonto und diese Gruppe sind in Active Directory vorhanden, wenn sich der Azure Multi-Factor Authentication-Server auf einem in die Domäne eingebundenen Server befindet. Dieses Dienstkonto und diese Gruppe sind lokal auf dem Azure Multi-Factor Authentication-Server vorhanden, wenn dieser nicht in eine Domäne eingebunden ist.


## <a name="install-the-web-service-sdk"></a>Installieren des Webdienst-SDK
Wenn das Azure Multi-Factor Authentication-Webdienst-SDK nicht bereits auf dem Azure Multi-Factor Authentication-Server (Azure MFA-Server) installiert ist, wechseln Sie zu diesem Server, und öffnen Sie den Azure MFA-Server. 

1. Klicken Sie auf das Symbol für das Webdienst-SDK.
2. Klicken Sie auf **Webdienst-SDK installieren**, und folgen Sie den Anweisungen auf dem Bildschirm. 

Das Webdienst-SDK muss durch ein SSL-Zertifikat geschützt sein. Für diesen Zweck ist ein selbst signiertes Zertifikat ausreichend. Importieren Sie das Zertifikat in den Speicher „Vertrauenswürdige Stammzertifizierungsstellen“ des lokalen Computerkontos auf dem Benutzerportal-Webserver, damit dieser das Zertifikat beim Initiieren einer SSL-Verbindung als vertrauenswürdig einstuft.

![Einrichtung](./media/multi-factor-authentication-get-started-server-webservice/sdk.png)

## <a name="install-the-mobile-app-web-service"></a>Installieren des Webdiensts der mobilen App
Beachten Sie folgende Punkte, bevor Sie den Webdienst der mobilen App installieren:

* Wenn das Azure MFA-Benutzerportal bereits auf dem Server mit Internetverbindung installiert ist, können der Benutzername, das Kennwort und die URL für das Webdienst-SDK aus der Datei „web.config“ des Benutzerportals kopiert werden.
* Es ist hilfreich, auf dem mit dem Internet verbundenen Webserver einen Webbrowser zu öffnen und die URL des Webdienst-SDK aufzurufen, die in die Datei "Web.config" eingegeben wurde. Wenn der Webdienst erfolgreich im Browser aufgerufen werden kann, werden Sie zur Eingabe von Anmeldeinformationen aufgefordert. Geben Sie den Benutzernamen und das Kennwort, die in die Datei "Web.config" eingegeben wurden, genau wie in der Datei angezeigt ein. Stellen Sie sicher, dass keine Zertifikatswarnungen oder -fehler angezeigt werden.
* Wenn sich vor dem Webserver mit dem Webdienst der mobilen App ein Reverseproxy oder eine Firewall befindet und eine SSL-Abladung durchführt, können Sie die Datei „web.config“ des Webdiensts der mobilen App bearbeiten, damit der Webdienst der mobilen App HTTP anstelle von HTTPS verwenden kann. Für die Verbindung zwischen der mobilen Anwendung und der Firewall/dem Reverseproxy ist weiterhin SSL erforderlich. Fügen Sie dem Abschnitt \<appSettings\> den folgenden Schlüssel hinzu: 

        <add key="SSL_REQUIRED" value="false"/>

### <a name="install-the-service"></a>Installieren des Diensts

1. Öffnen Sie auf dem Azure Multi-Factor Authentication-Server den Windows-Explorer, und navigieren Sie zu dem Ordner, in dem der Azure MFA-Server installiert ist (üblicherweise „C:\Programme\Azure Multi-Factor Authentication Server“). Wählen Sie die 32- oder 64-Bit-Version der Installationsdatei „Azure Multi-Factor AuthenticationPhoneAppWebServiceSetup“ aus. Kopieren Sie die Installationsdatei auf den mit dem Internet verbundenen Server.

2. Führen Sie die Setupdatei mit Administratorrechten auf dem mit dem Internet verbundenen Webserver aus. Öffnen Sie eine Eingabeaufforderung als Administrator, und navigieren Sie zu dem Verzeichnis, in das die Installationsdatei kopiert wurde.

3. Führen Sie die Installationsdatei „Multi-Factor AuthenticationMobileAppWebServiceSetup“ aus, ändern Sie ggf. die Website, und legen Sie das virtuelle Verzeichnis auf einen kurzen Namen wie etwa „PA“ fest.

  Es wird empfohlen, für das virtuelle Verzeichnis einen kurzen Namen festzulegen, da der Benutzer die URL des Webdiensts der mobilen App bei der Aktivierung auf dem mobilen Gerät eingeben muss.

4. Navigieren Sie nach Abschluss der Installation von Azure Multi-Factor AuthenticationMobileAppWebServiceSetup zu "C:\inetpub\wwwroot\PA" (oder zum entsprechenden Verzeichnis basierend auf dem Namen des virtuellen Verzeichnisses), und bearbeiten Sie die Datei "Web.config". 

5. Suchen Sie die Schlüssel „WEB_SERVICE_SDK_AUTHENTICATION_USERNAME“ und „WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD“. Legen Sie die Werte für den Benutzernamen und das Kennwort auf das Dienstkonto fest, das Mitglied der Sicherheitsgruppe „PhoneFactor Admins“ ist. Dabei kann es sich um das gleiche Konto handeln, das als Identität des Azure Multi-Factor Authentication-Benutzerportals verwendet wird, sofern dieses bereits installiert wurde. Geben Sie den Benutzernamen und das Kennwort zwischen den Anführungszeichen am Ende der Zeile ein (value=""/>). Verwenden Sie einen qualifizierten Benutzernamen (etwa „Domäne\Benutzername“ oder „Computer\Benutzername“).  

6. Suchen Sie die Einstellung „pfMobile App Web Service_pfwssdk_PfWsSdk“. Ändern Sie den Wert *http://localhost:4898/PfWsSdk.asmx* in die URL des Webdienst-SDKs, das auf dem Azure Multi-Factor Authentication-Server ausgeführt wird (beispielsweise „https://computer1.domain.local/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx“). 

  Da für diese Verbindung SSL verwendet wird, müssen Sie auf das Webdienst-SDK mit dem Servernamen (und nicht mit der IP-Adresse) verweisen. Das SSL-Zertifikat wurde für den Servernamen ausgestellt, und die verwendete URL muss dem Namen aus dem Zertifikat entsprechen. Der Servername lässt sich unter Umständen nicht in eine IP-Adresse des Servers mit Internetzugriff auflösen. Fügen Sie in diesem Fall der Hostdatei auf diesem Server einen Eintrag hinzu, um den Namen des Azure Multi-Factor Authentication-Servers seiner IP-Adresse zuzuordnen. Speichern Sie die Datei "Web.config", nachdem Änderungen vorgenommen wurden.

7. Wenn die Website, unter der der Webdienst der mobilen App installiert wurde, noch nicht mit einem öffentlich signierten Zertifikat gebunden wurde, installieren Sie das Zertifikat auf dem Server, öffnen Sie den IIS-Manager, und binden Sie das Zertifikat an die Website.

8. Öffnen Sie auf einem beliebigen Computer einen Webbrowser, und navigieren Sie zu der URL, unter der der Webdienst der mobilen App installiert wurde (etwa „https://www.publicwebsite.com/PA“). Stellen Sie sicher, dass keine Zertifikatswarnungen oder -fehler angezeigt werden.

### <a name="configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server"></a>Konfigurieren der Einstellungen für die mobile App im Azure Multi-Factor Authentication-Server
Nachdem Sie den Webdienst der mobilen App installiert haben, müssen Sie den Azure Multi-Factor Authentication-Server für die Verwendung mit dem Portal konfigurieren.

1. Klicken Sie im Azure MFA-Server auf das Symbol für das Benutzerportal. Wenn Benutzer ihre Authentifizierungsmethode selbst steuern dürfen, aktivieren Sie auf der Registerkarte „Einstellungen“ unter **Methodenauswahl durch Benutzer zulassen** die Option **Mobile App**. Wenn diese Funktion nicht aktiviert ist, müssen Endbenutzer sich an den Helpdesk wenden, um die Aktivierung für die mobile App abzuschließen.
2. Aktivieren Sie das Kontrollkästchen **Aktivierung der mobilen Anwendung durch Benutzer zulassen**.
3. Aktivieren Sie das Kontrollkästchen **Benutzerregistrierung zulassen**.
4. Klicken Sie auf das Symbol "Mobile App".
5. Geben Sie die URL für das virtuelle Verzeichnis ein, das bei der Installation von „Azure Multi-Factor AuthenticationMobileAppWebServiceSetup“ erstellt wurde. Ein Kontoname kann in das Eingabefeld eingegeben werden. Dieser Firmenname wird in der mobilen App angezeigt. Wenn Sie das Feld leer lassen, wird der Name des Anbieters für mehrstufige Authentifizierung angezeigt, der im klassischen Azure-Portal erstellt wurde.

<center>![Einrichtung](./media/multi-factor-authentication-get-started-server-webservice/mobile.png)</center>



<!--HONumber=Feb17_HO3-->


