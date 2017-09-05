---
title: "Webdienst der mobilen App für den Azure MFA-Server | Microsoft-Dokumentation"
description: "Die Microsoft Authenticator-App bietet eine zusätzliche Out-of-Band-Authentifizierungsoption.  Diese ermöglicht es, dass der MFA-Server Pushbenachrichtigungen an Benutzer sendet."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 6c8d6fcc-70f4-4da4-9610-c76d66635b8b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/23/2017
ms.author: joflore
ms.reviewer: alexwe
ms.custom: it-pro
ms.translationtype: HT
ms.sourcegitcommit: 646886ad82d47162a62835e343fcaa7dadfaa311
ms.openlocfilehash: bf758d1241f2a56eba4d5c92ace713d6e563df65
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---
# <a name="enable-mobile-app-authentication-with-azure-multi-factor-authentication-server"></a>Aktivieren der Mobile App-Authentifizierung mit dem Azure Multi-Factor Authentication-Server

Die Microsoft Authenticator-App bietet eine zusätzliche Out-of-Band-Überprüfungsoption. Anstelle eines automatischen Telefonanrufs oder einer SMS sendet Azure Multi-Factor Authentication bei der Benutzeranmeldung eine Pushbenachrichtigung an die Microsoft Authenticator-App auf dem Smartphone oder Tablet des Benutzers. Der Benutzer tippt in der App einfach auf **Überprüfen** (oder gibt eine PIN ein und tippt auf „Authentifizieren“), um sich anzumelden.

Die Verwendung einer mobilen App für die Prüfung in zwei Schritten empfiehlt sich, wenn kein zuverlässiges Mobilfunknetz zur Verfügung steht. Bei Verwendung als OAuth-Token-Generator benötigt die App keine Netzwerk- oder Internetverbindung.

Abhängig von Ihrer Umgebung können Sie den Webdienst der mobilen App auf dem gleichen Server wie den Azure Multi-Factor Authentication-Server oder auf einem anderen, mit dem Internet verbundenen Server bereitstellen.

## <a name="requirements"></a>Anforderungen

Zur Verwendung der Microsoft Authenticator-App müssen folgende Voraussetzungen erfüllt sein, damit die App erfolgreich mit dem Webdienst der mobilen App kommunizieren kann:

* Azure Multi-Factor Authentication-Server ab Version 6.0
* Installieren Sie den Webdienst der mobilen App auf einem Webserver mit Internetverbindung, auf dem Microsoft® [Internetinformationsdienste (IIS) 7.x oder höher](http://www.iis.net/) ausgeführt wird.
* ASP.NET v4.0.30319 ist installiert, registriert und auf „Zulässig“ festgelegt.
* Erforderliche Rollendienste umfassen ASP.NET und IIS 6-Metabasiskompatibilität.
* Der Webdienst der mobilen App ist über eine öffentliche URL erreichbar.
* Der Webdienst der mobilen App ist durch ein SSL-Zertifikat geschützt.
* Installieren Sie das Azure Multi-Factor Authentication-Webdienst-SDK in IIS 7.x oder höher auf dem **gleichen Server, auf dem auch der Azure Multi-Factor Authentication-Server installiert ist**.
* Das Azure Multi-Factor Authentication-Webdienst-SDK ist durch ein SSL-Zertifikat geschützt.
* Der Webdienst der mobilen App kann eine SSL-Verbindung mit dem Azure Multi-Factor Authentication-Webdienst-SDK herstellen.
* Der Webdienst der mobilen App kann sich beim Azure Multi-Factor Authentication-Webdienst-SDK mit den Anmeldeinformationen eines Dienstkontos authentifizieren, das Mitglied der Sicherheitsgruppe „PhoneFactor Admins“ ist. Dieses Dienstkonto und diese Gruppe sind in Active Directory vorhanden, wenn sich der Azure Multi-Factor Authentication-Server auf einem in die Domäne eingebundenen Server befindet. Dieses Dienstkonto und diese Gruppe sind lokal auf dem Azure Multi-Factor Authentication-Server vorhanden, wenn dieser nicht in eine Domäne eingebunden ist.

## <a name="install-the-mobile-app-web-service"></a>Installieren des Webdiensts der mobilen App

Beachten Sie folgende Punkte, bevor Sie den Webdienst der mobilen App installieren:

* Sie benötigen ein Dienstkonto, das der Gruppe „PhoneFactor Admins“ angehört. Dabei kann es sich um das gleiche Konto handeln, das Sie auch für die Installation des Benutzerportals verwendet haben.
* Es ist hilfreich, auf dem mit dem Internet verbundenen Webserver einen Webbrowser zu öffnen und die URL des Webdienst-SDK aufzurufen, die in die Datei "Web.config" eingegeben wurde. Wenn der Webdienst erfolgreich im Browser aufgerufen werden kann, werden Sie zur Eingabe von Anmeldeinformationen aufgefordert. Geben Sie den Benutzernamen und das Kennwort, die in die Datei "Web.config" eingegeben wurden, genau wie in der Datei angezeigt ein. Stellen Sie sicher, dass keine Zertifikatswarnungen oder -fehler angezeigt werden.
* Wenn sich vor dem Webserver mit dem Webdienst der mobilen App ein Reverseproxy oder eine Firewall befindet und eine SSL-Abladung durchführt, können Sie die Datei „web.config“ des Webdiensts der mobilen App bearbeiten, damit der Webdienst der mobilen App HTTP anstelle von HTTPS verwenden kann. Für die Verbindung zwischen der mobilen Anwendung und der Firewall/dem Reverseproxy ist weiterhin SSL erforderlich. Fügen Sie dem Abschnitt \<appSettings\> den folgenden Schlüssel hinzu:

        <add key="SSL_REQUIRED" value="false"/>

### <a name="install-the-web-service-sdk"></a>Installieren des Webdienst-SDK

Unabhängig vom Szenario gilt: Falls das Azure Multi-Factor Authentication-Webdienst-SDK noch **nicht** auf dem Azure Multi-Factor Authentication-Server (Azure MFA-Server) installiert ist, führen Sie die folgenden Schritte aus.

1. Öffnen Sie die Konsole des Multi-Factor Authentication-Servers.
2. Wählen Sie im **Webdienst-SDK** die Option **Webdienst-SDK installieren** aus.
3. Führen Sie die Installation unter Verwendung der Standardeinstellungen durch, es sei denn, Sie müssen sie aus irgendeinem Grund ändern.
4. Binden Sie ein SSL-Zertifikat an die Website in IIS.

Antworten auf Fragen zum Konfigurieren eines SSL-Zertifikats für einen IIS-Server finden Sie im Artikel [How to Set Up SSL on IIS 7](https://docs.microsoft.com/en-us/iis/manage/configuring-security/how-to-set-up-ssl-on-iis) (Einrichten von SSL in IIS 7).

Das Webdienst-SDK muss durch ein SSL-Zertifikat geschützt sein. Für diesen Zweck ist ein selbst signiertes Zertifikat ausreichend. Importieren Sie das Zertifikat in den Speicher „Vertrauenswürdige Stammzertifizierungsstellen“ des lokalen Computerkontos auf dem Benutzerportal-Webserver, damit dieser das Zertifikat beim Initiieren einer SSL-Verbindung als vertrauenswürdig einstuft.

![MFA-Serverkonfiguration – Einrichten des Webdienst-SDKs](./media/multi-factor-authentication-get-started-server-webservice/sdk.png)

### <a name="install-the-service"></a>Installieren des Diensts

1. **Auf dem Server MFA:** Navigieren Sie zum Installationspfad.
2. Navigieren Sie zum Installationsordner des Azure MFA-Servers (standardmäßig **C:\Programme\Azure Multi-Factor Authentication**).
3. Suchen Sie die Installationsdatei **MultiFactorAuthenticationMobileAppWebServiceSetup64**. Sollte der Server **nicht** mit dem Internet verbunden sein, kopieren Sie die Installationsdatei auf den mit dem Internet verbundenen Server.
4. Sollte der MFA-Server **nicht** mit dem Internet verbunden sein, wechseln Sie zum **Server mit Internetzugriff**.
5. Führen Sie die Installationsdatei **MultiFactorAuthenticationMobileAppWebServiceSetup64** als Administrator aus, ändern Sie ggf. den Standort, und legen Sie bei Bedarf das virtuelle Verzeichnis auf einen Kurznamen fest.
6. Navigieren Sie nach Abschluss der Installation zu **C:\inetpub\wwwroot\MultiFactorAuthMobileAppWebService** (oder zum entsprechenden Verzeichnis auf der Grundlage des Namens des virtuellen Verzeichnisses), und bearbeiten Sie die Datei „Web.Config“.

   * Suchen Sie den Schlüssel **"WEB_SERVICE_SDK_AUTHENTICATION_USERNAME"**, und ändern Sie **value=""** in **value="DOMÄNE\Benutzer"**, wobei es sich bei „DOMÄNE\Benutzer“ um ein Dienstkonto handelt, das der Gruppe „PhoneFactor Admins“ angehört.
   * Suchen Sie den Schlüssel **"WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD"**, und ändern Sie **value=""** in **value="Kennwort"**, wobei es sich bei „Kennwort“ um das Kennwort für das Dienstkonto aus der vorherigen Zeile handelt.
   * Suchen Sie die Einstellung **pfMobile App Web Service_pfwssdk_PfWsSdk**, und ändern Sie den Wert von **http://localhost:4898/PfWsSdk.asmx** in die URL des Webdienst-SDKs (Beispiel: https://mfa.contoso.com/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx).
   * Speichern Sie die Datei „Web.Config“, und schließen Sie den Editor.

   > [!NOTE]
   > Da für diese Verbindung SSL verwendet wird, müssen Sie auf das Webdienst-SDK mit dem **vollqualifizierten Domänennamen (Fully Qualified Domain Name (FQDN))** (und **nicht mit der IP-Adresse**) verweisen. Das SSL-Zertifikat wurde für den FQDN ausgestellt, und die verwendete URL muss dem Namen aus dem Zertifikat entsprechen.

7. Wenn die Website, unter der der Webdienst der mobilen App installiert wurde, noch nicht mit einem öffentlich signierten Zertifikat gebunden wurde, installieren Sie das Zertifikat auf dem Server, öffnen Sie den IIS-Manager, und binden Sie das Zertifikat an die Website.
8. Öffnen Sie auf einem beliebigen Computer einen Webbrowser, und navigieren Sie zu der URL, unter der der Webdienst der mobilen App installiert wurde (Beispiel: https://mfa.contoso.com/MultiFactorAuthMobileAppWebService). Stellen Sie sicher, dass keine Zertifikatswarnungen oder -fehler angezeigt werden.

## <a name="configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server"></a>Konfigurieren der Einstellungen für die mobile App im Azure Multi-Factor Authentication-Server

Nachdem Sie den Webdienst der mobilen App installiert haben, müssen Sie den Azure Multi-Factor Authentication-Server für die Verwendung mit dem Portal konfigurieren.

1. Klicken Sie in der Konsole des Multi-Factor Authentication-Servers auf das Benutzerportal-Symbol. Wenn Benutzer ihre Authentifizierungsmethode selbst steuern dürfen, aktivieren Sie auf der Registerkarte „Einstellungen“ unter **Methodenauswahl durch Benutzer zulassen** die Option **Mobile App**. Wenn diese Funktion nicht aktiviert ist, müssen sich Endbenutzer an den Helpdesk wenden, um die Aktivierung für die mobile App abzuschließen.
2. Aktivieren Sie das Kontrollkästchen **Aktivierung der mobilen Anwendung durch Benutzer zulassen**.
3. Aktivieren Sie das Kontrollkästchen **Benutzerregistrierung zulassen**.
4. Klicken Sie auf das Symbol **Mobile App**.
5. Geben Sie im Feld **Webdienst-URL der mobilen Anwendung:** die URL für das virtuelle Verzeichnis ein, das bei der Installation von „MultiFactorAuthenticationMobileAppWebServiceSetup64“ erstellt wurde (Beispiel: https://mfa.contoso.com/MultiFactorAuthMobileAppWebService/).
6. Geben Sie im Feld **Kontoname** den Unternehmens- oder Organisationsnamen ein, der in der mobilen Anwendung für dieses Konto angezeigt werden soll.
   ![MFA-Server-Konfiguration – Einstellungen für die mobile App](./media/multi-factor-authentication-get-started-server-webservice/mobile.png)

## <a name="next-steps"></a>Nächste Schritte

- [Erweiterte Szenarien mit Azure Multi-Factor Authentication und Drittanbieter-VPNs](multi-factor-authentication-advanced-vpn-configurations.md)
