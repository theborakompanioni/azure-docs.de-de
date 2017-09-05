---
title: "Benutzerportal für Azure MFA-Server | Microsoft-Dokumentation"
description: "Dies ist die Seite für Azure Multi-Factor Authentication, auf der die ersten Schritte mit Azure MFA und dem Benutzerportal beschrieben werden."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 06b419fa-3507-4980-96a4-d2e3960e1772
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
ms.sourcegitcommit: 5b6c261c3439e33f4d16750e73618c72db4bcd7d
ms.openlocfilehash: a4eb403d3d21b7dbe63c2645b488a7bddb6d39fd
ms.contentlocale: de-de
ms.lasthandoff: 08/28/2017

---
# <a name="user-portal-for-the-azure-multi-factor-authentication-server"></a>Benutzerportal für den Azure Multi-Factor Authentication-Server

Das Benutzerportal ist eine IIS-Website, die es Benutzern ermöglicht, sich für Azure Multi-Factor Authentication (MFA) zu registrieren und ihre Konten zu verwalten. Die Benutzer können ihre Telefonnummer oder PIN ändern oder die Überprüfung in zwei Schritten bei der nächsten Anmeldung umgehen.

Die Benutzer melden sich mit ihrem normalen Benutzernamen und Kennwort am Benutzerportal an und rufen dann entweder die Überprüfung in zwei Schritten auf oder beantworten Sicherheitsfragen, um die Authentifizierung abzuschließen. Wenn die Benutzerregistrierung zulässig ist, konfigurieren Benutzer ihre Telefonnummer und PIN, wenn sie sich zum ersten Mal beim Benutzerportal anmelden.

Es können Benutzerportaladministratoren eingerichtet werden, und ihnen kann die Berechtigung zum Hinzufügen neuer Benutzer und zum Aktualisieren vorhandener Benutzer erteilt werden.

Abhängig von Ihrer Umgebung können Sie das Benutzerportal auf dem gleichen Server wie den Azure Multi-Factor Authentication-Server oder auf einem anderen, mit dem Internet verbundenen Server bereitstellen.

![MFA-Benutzerportal](./media/multi-factor-authentication-get-started-portal/portal.png)

> [!NOTE]
> Das Benutzerportal ist nur mit Multi-Factor Authentication-Server verfügbar. Wenn Sie Multi-Factor Authentication in der Cloud verwenden, können Sie Ihre Benutzer auf [Einrichten meines Kontos für die Überprüfung in zwei Schritten](./end-user/multi-factor-authentication-end-user-first-time.md) oder auf [Verwalten der Einstellungen für die Überprüfung in zwei Schritten](./end-user/multi-factor-authentication-end-user-manage-settings.md) verweisen.

## <a name="install-the-web-service-sdk"></a>Installieren des Webdienst-SDK

Unabhängig vom Szenario gilt: Falls das Azure Multi-Factor Authentication-Webdienst-SDK noch **nicht** auf dem Azure Multi-Factor Authentication-Server (Azure MFA-Server) installiert ist, führen Sie die folgenden Schritte aus.

1. Öffnen Sie die Konsole des Multi-Factor Authentication-Servers.
2. Wählen Sie im **Webdienst-SDK** die Option **Webdienst-SDK installieren** aus.
3. Führen Sie die Installation unter Verwendung der Standardeinstellungen durch, es sei denn, Sie müssen sie aus irgendeinem Grund ändern.
4. Binden Sie ein SSL-Zertifikat an die Website in IIS.

Antworten auf Fragen zum Konfigurieren eines SSL-Zertifikats für einen IIS-Server finden Sie im Artikel [How to Set Up SSL on IIS 7](https://docs.microsoft.com/en-us/iis/manage/configuring-security/how-to-set-up-ssl-on-iis) (Einrichten von SSL in IIS 7).

Das Webdienst-SDK muss durch ein SSL-Zertifikat geschützt sein. Für diesen Zweck ist ein selbst signiertes Zertifikat ausreichend. Importieren Sie das Zertifikat in den Speicher „Vertrauenswürdige Stammzertifizierungsstellen“ des lokalen Computerkontos auf dem Benutzerportal-Webserver, damit dieser das Zertifikat beim Initiieren einer SSL-Verbindung als vertrauenswürdig einstuft.

![MFA-Serverkonfiguration – Einrichten des Webdienst-SDKs](./media/multi-factor-authentication-get-started-portal/sdk.png)

## <a name="deploy-the-user-portal-on-the-same-server-as-the-azure-multi-factor-authentication-server"></a>Bereitstellen des Benutzerportals auf demselben Server wie für den Azure Multi-Factor Authentication-Server

Wenn Sie das Benutzerportal auf dem **gleichen Server** installieren möchten wie den Azure Multi-Factor Authentication-Server, ist Folgendes erforderlich:

* IIS, einschließlich ASP.NET und IIS 6-Metabasiskompatibilität (ab IIS 7)
* Ein Konto mit Administratorrechten für den Computer und die Domäne, falls zutreffend. Das Konto benötigt Berechtigungen zum Erstellen von Active Directory-Sicherheitsgruppen.
* Schützen Sie das Benutzerportal mit einem SSL-Zertifikat.
* Schützen Sie das Azure Multi-Factor Authentication-Webdienst-SDK mit einem SSL-Zertifikat.

Führen Sie die folgenden Schritte aus, um das Benutzerportal bereitzustellen:

1. Öffnen Sie die Konsole des Azure Multi-Factor Authentication-Servers, klicken Sie im linken Menü auf das Symbol **Benutzerportal**, und klicken Sie anschließend auf **Benutzerportal installieren**.
2. Führen Sie die Installation unter Verwendung der Standardeinstellungen durch, es sei denn, Sie müssen sie aus irgendeinem Grund ändern.
3. Binden Sie ein SSL-Zertifikat an die Website in IIS.

   > [!NOTE]
   > Bei diesem Zertifikat handelt es sich in der Regel um ein öffentlich signiertes SSL-Zertifikat.

4. Öffnen Sie auf einem beliebigen Computer einen Webbrowser, und navigieren Sie zu der URL, unter der das Benutzerportal installiert wurde (Beispiel: https://mfa.contoso.com/MultiFactorAuth). Stellen Sie sicher, dass keine Zertifikatswarnungen oder -fehler angezeigt werden.

![Installation des MFA-Server-Benutzerportals](./media/multi-factor-authentication-get-started-portal/install.png)

Antworten auf Fragen zum Konfigurieren eines SSL-Zertifikats für einen IIS-Server finden Sie im Artikel [How to Set Up SSL on IIS 7](https://docs.microsoft.com/en-us/iis/manage/configuring-security/how-to-set-up-ssl-on-iis) (Einrichten von SSL in IIS 7).

## <a name="deploy-the-user-portal-on-a-separate-server"></a>Bereitstellen des Benutzerportals auf einem separaten Server

Wenn der Server, auf dem der Azure Multi-Factor Authentication-Server ausgeführt wird, über keine Internetverbindung verfügt, sollten Sie das Benutzerportal auf einem **separaten Server mit Internetzugriff** installieren.

Falls Ihre Organisation die Microsoft Authenticator-App als eine der Überprüfungsmethoden nutzt und das Benutzerportal auf einem eigenen Server bereitgestellt werden soll, müssen die folgenden Voraussetzungen erfüllt sein:

* Verwenden Sie Version 6.0 des Azure Multi-Factor Authentication-Servers (oder höher).
* Installieren Sie das Benutzerportal auf einem Webserver mit Internetzugriff, auf dem Microsoft Internetinformationsdienste (IIS) 6.x oder höher ausgeführt wird.
* Stellen Sie bei Verwendung von IIS 6.x sicher, dass ASP.NET v2.0.50727 installiert, registriert und auf **Zulässig** festgelegt ist.
* Bei Verwendung von IIS 7.x oder einer höheren Version benötigen Sie IIS einschließlich Standardauthentifizierung, ASP.NET und IIS 6-Metabasiskompatibilität.
* Schützen Sie das Benutzerportal mit einem SSL-Zertifikat.
* Schützen Sie das Azure Multi-Factor Authentication-Webdienst-SDK mit einem SSL-Zertifikat.
* Stellen Sie sicher, dass das Benutzerportal eine SSL-Verbindung mit dem Azure Multi-Factor Authentication-Webdienst-SDK herstellen kann.
* Stellen Sie sicher, dass das Benutzerportal beim Azure Multi-Factor Authentication-Webdienst-SDK mit den Anmeldeinformationen eines Dienstkontos der Sicherheitsgruppe „PhoneFactor Admins“ authentifiziert werden kann. Dieses Dienstkonto und diese Gruppe müssen in Active Directory vorhanden sein, wenn der Azure Multi-Factor Authentication-Server auf einem in die Domäne eingebundenen Server ausgeführt wird. Dieses Dienstkonto und diese Gruppe sind lokal auf dem Azure Multi-Factor Authentication-Server vorhanden, wenn dieser nicht in eine Domäne eingebunden ist.

Wenn Sie das Benutzerportal auf einem anderen Server als dem Azure Multi-Factor Authentication-Server installieren möchten, sind folgende Schritte erforderlich:

1. **Auf dem MFA-Server:** Navigieren Sie zum Installationspfad (Beispiel: C:\Programme\Multi-Factor Authentication-Server), und kopieren Sie die Datei **MultiFactorAuthenticationUserPortalSetup64** an einen Ort, auf den der mit dem Internet verbundene Server zugreifen kann und an dem die Installation erfolgen soll.
2. **Auf dem mit dem Internet verbundenen Webserver:** Führen Sie die Installationsdatei „MultiFactorAuthenticationUserPortalSetup64“ als Administrator aus, ändern Sie ggf. den Standort, und legen Sie bei Bedarf das virtuelle Verzeichnis auf einen Kurznamen fest.
3. Binden Sie ein SSL-Zertifikat an die Website in IIS.

   > [!NOTE]
   > Bei diesem Zertifikat handelt es sich in der Regel um ein öffentlich signiertes SSL-Zertifikat.

4. Navigieren Sie zu **C:\inetpub\wwwroot\MultiFactorAuth**.
5. Bearbeiten Sie die Datei „Web.Config“ im Editor.

    * Suchen Sie den Schlüssel **"USE_WEB_SERVICE_SDK"**, und ändern Sie **value="false"** in **value="true"**.
    * Suchen Sie den Schlüssel **"WEB_SERVICE_SDK_AUTHENTICATION_USERNAME"**, und ändern Sie **value=""** in **value="DOMÄNE\Benutzer"**, wobei es sich bei „DOMÄNE\Benutzer“ um ein Dienstkonto handelt, das der Gruppe „PhoneFactor Admins“ angehört.
    * Suchen Sie den Schlüssel **"WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD"**, und ändern Sie **value=""** in **value="Kennwort"**, wobei es sich bei „Kennwort“ um das Kennwort für das Dienstkonto aus der vorherigen Zeile handelt.
    * Suchen Sie den Wert **https://www.contoso.com/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx**, und ändern Sie diese Platzhalter-URL in die Webdienst-SDK-URL aus Schritt 2.
    * Speichern Sie die Datei „Web.Config“, und schließen Sie den Editor.

6. Öffnen Sie auf einem beliebigen Computer einen Webbrowser, und navigieren Sie zu der URL, unter der das Benutzerportal installiert wurde (Beispiel: https://mfa.contoso.com/MultiFactorAuth). Stellen Sie sicher, dass keine Zertifikatswarnungen oder -fehler angezeigt werden.

Antworten auf Fragen zum Konfigurieren eines SSL-Zertifikats für einen IIS-Server finden Sie im Artikel [How to Set Up SSL on IIS 7](https://docs.microsoft.com/en-us/iis/manage/configuring-security/how-to-set-up-ssl-on-iis) (Einrichten von SSL in IIS 7).

## <a name="configure-user-portal-settings-in-the-azure-multi-factor-authentication-server"></a>Konfigurieren der Benutzerportaleinstellungen auf dem Azure Multi-Factor Authentication-Server

Nachdem das Benutzerportal installiert wurde, müssen Sie den Azure Multi-Factor Authentication-Server für die Verwendung des Portals konfigurieren.

1. Klicken Sie in der Konsole des Azure Multi-Factor Authentication-Servers auf das Symbol **Benutzerportal**. Geben Sie auf der Registerkarte „Einstellungen“ im Textfeld **Benutzerportal-URL** die URL des Benutzerportals ein. Sofern die E-Mail-Funktion aktiviert wurde, wird diese URL in E-Mails eingefügt, die an Benutzer gesendet werden, wenn diese in den Azure Multi-Factor Authentication-Server importiert werden.
2. Wählen Sie die Einstellungen aus, die Sie im Benutzerportal verwenden möchten. Wenn beispielsweise Benutzer ihre Authentifizierungsmethoden auswählen dürfen, sollten Sie sicherstellen, dass die Option **Methodenauswahl durch Benutzer zulassen** zusammen mit den zur Auswahl stehenden Methoden aktiviert ist.
3. Definieren Sie auf der Registerkarte **Administratoren** die gewünschten Administratoren. Mithilfe der Kontrollkästchen und Dropdownlisten der Felder „Hinzufügen“/„Bearbeiten“ lassen sich präzise Administratorberechtigungen erstellen.

Optionale Konfiguration:
- **Sicherheitsfragen:** Definieren Sie genehmigte Sicherheitsfragen für Ihre Umgebung und die Sprache, in der sie angezeigt werden.
- **Vergangene Sitzungen:** Konfigurieren Sie die Benutzerportalintegration in eine formularbasierte Website mit MFA.
- **Vertrauenswürdige IPs:** Lassen Sie zu, dass Benutzer die MFA überspringen, wenn sie sich über eine IP-Adresse authentifizieren, die in einer Liste mit vertrauenswürdigen IP-Adressen oder -Bereichen enthalten ist.

![MFA-Server – Konfiguration des Benutzerportals](./media/multi-factor-authentication-get-started-portal/config.png)

Der Azure Multi-Factor Authentication-Server stellt mehrere Optionen für das Benutzerportal bereit. Die folgende Tabelle enthält eine Liste dieser Optionen sowie eine Erläuterung zu deren Verwendung.

| Benutzerportal – Einstellungen | Beschreibung |
|:--- |:--- |
| Benutzerportal-URL | Geben Sie die URL ein, unter der das Portal gehostet wird. |
| Primäre Authentifizierung | Geben Sie den Typ der Authentifizierung für die Anmeldung beim Portal an. Mögliche Optionen: Windows-, Radius- oder LDAP-Authentifizierung. |
| Benutzeranmeldung zulassen | Ermöglicht Benutzern die Eingabe von Benutzername und Kennwort auf der Anmeldeseite des Benutzerportals. Wenn diese Option nicht ausgewählt ist, sind die Felder abgeblendet. |
| Benutzerregistrierung zulassen | Ermöglicht dem Benutzer die Registrierung bei der Multi-Factor Authentication. Dazu wird der Benutzer auf einem Setupbildschirm zur Eingabe zusätzlicher Informationen wie beispielsweise einer Telefonnummer aufgefordert. Die Eingabeaufforderung für die Ersatzrufnummer ermöglicht dem Benutzer die Angabe einer sekundären Telefonnummer. Die Eingabeaufforderung für das Drittanbieter-OATH-Token ermöglicht dem Benutzer die Angabe eines entsprechenden Tokens. |
| Benutzern das Initiieren einer Einmalumgehung ermöglichen | Ermöglicht Benutzern das Initiieren einer Einmalumgehung. Wenn ein Benutzer diese Option einrichtet, wird sie bei seiner nächsten Anmeldung wirksam. Die Eingabeaufforderung für die Gültigkeit der Umgehung in Sekunden zeigt dem Benutzer ein Feld an, in dem er den Standardwert von 300 Sekunden ändern kann. Andernfalls gilt die Einmalumgehung nur für 300 Sekunden. |
| Benutzern die Auswahl der Methode ermöglichen | Ermöglicht Benutzern das Festlegen ihrer primären Kontaktmethode. Bei dieser Methode kann es sich um einen Telefonanruf, eine SMS, die mobile App oder ein OATH-Token handeln. |
| Benutzern die Auswahl der Sprache ermöglichen | Ermöglicht Benutzern das Ändern der Sprache, die für den Telefonanruf, die Textnachricht (SMS), die mobile App oder das OATH-Token verwendet wird. |
| Benutzern das Aktivieren der mobilen App ermöglichen | Ermöglicht Benutzern das Generieren eines Aktivierungscodes, um den Aktivierungsprozess der mit dem Server verwendeten mobilen App durchzuführen.  Sie können auch die Anzahl der Geräte festlegen (1 bis 10), auf denen Benutzer die App aktivieren können. |
| Sicherheitsfragen als Alternative verwenden | Ermöglicht die Verwendung von Sicherheitsfragen, falls die Überprüfung in zwei Schritten fehlschlägt. Sie können die Anzahl der Sicherheitsfragen festlegen, die richtig beantwortet werden müssen. |
| Benutzern das Zuordnen von Drittanbieter-OATH-Token ermöglichen | Ermöglicht Benutzern das Angeben eines Drittanbieter-OATH-Tokens. |
| OATH-Token als Alternative verwenden | Ermöglicht die Verwendung eines OATH-Tokens, falls die Überprüfung in zwei Schritten nicht erfolgreich ist. Sie können auch den Sitzungstimeout in Minuten angeben. |
| Aktivieren der Protokollierung | Ermöglicht die Protokollierung im Benutzerportal. Die Protokolldateien befinden sich unter: C:\Programme\Multi-Factor Authentication Server\Logs. |

Diese Einstellungen werden dem Benutzer im Portal angezeigt, nachdem sie aktiviert wurden und der Benutzer sich beim Benutzerportal angemeldet hat.

![Benutzerportal – Einstellungen](./media/multi-factor-authentication-get-started-portal/portalsettings.png)

### <a name="self-service-user-enrollment"></a>Self-Service-Benutzerregistrierung

Wenn Sie möchten, dass Ihre Benutzer sich anmelden und registrieren, müssen Sie auf der Registerkarte „Einstellungen“ die Optionen **Benutzeranmeldung zulassen** und **Benutzerregistrierung zulassen** auswählen. Denken Sie daran, dass die ausgewählten Einstellungen die Benutzeranmeldung beeinflussen.

Wenn sich ein Benutzer beispielsweise zum ersten Mal beim Benutzerportal anmeldet, wird er auf die Seite zur Benutzereinrichtung von Azure Multi-Factor Authentication geleitet. Je nachdem, wie Sie Azure Multi-Factor Authentication konfiguriert haben, kann der Benutzer seine Authentifizierungsmethode auswählen.

Wenn er die Überprüfungsmethode „Sprachanruf“ auswählt oder für die Verwendung dieser Methode vorkonfiguriert wurde, fordert die Seite den Benutzer zur Eingabe seiner primären Rufnummer und ggf. der Durchwahl auf. Möglicherweise kann er auch eine Ersatzrufnummer eingeben.

![Registrieren der primären Rufnummer und der Ersatzrufnummer](./media/multi-factor-authentication-get-started-portal/backupphone.png)

Wenn der Benutzer bei der Authentifizierung eine PIN verwenden muss, fordert ihn die Seite zur Erstellung einer PIN auf. Nach der Eingabe der Telefonnummer(n) und PIN (falls zutreffend) klickt der Benutzer auf die Schaltfläche **Jetzt für Authentifizierung angerufen werden**. Azure Multi-Factor Authentication prüft die primäre Telefonnummer des Benutzers durch einen Anruf. Der Benutzer muss den Anruf annehmen, ggf. seine PIN eingeben und # drücken, um zum nächsten Schritt der Selbstregistrierung zu gelangen.

Wenn der Benutzer die Überprüfung per SMS auswählt oder für die Verwendung dieser Methode vorkonfiguriert wurde, fordert die Seite den Benutzer zur Angabe seiner Mobiltelefonnummer auf. Wenn der Benutzer bei der Authentifizierung eine PIN verwenden muss, fordert ihn die Seite auch zur Eingabe einer PIN auf.  Nach der Eingabe der Telefonnummer und PIN (falls zutreffend) klickt der Benutzer auf die Schaltfläche **Jetzt per SMS authentifizieren**. Azure Multi-Factor Authentication führt eine SMS-Überprüfung über das Mobiltelefon des Benutzers durch. Der Benutzer empfängt die SMS mit einer Einmalkennung (One-Time Passcode, OTP) und sendet dann eine Antwort auf die Nachricht mit der Einmalkennung und seiner PIN (falls zutreffend).

![Benutzerportal-SMS](./media/multi-factor-authentication-get-started-portal/text.png)

Wenn der Benutzer die Überprüfungsmethode per mobiler App wählt, wird er aufgefordert, die Microsoft Authenticator-App auf seinem Gerät zu installieren und einen Aktivierungscode zu generieren. Nach der Installation der App klickt der Benutzer auf die Schaltfläche „Aktivierungscode generieren“.

> [!NOTE]
> Zum Verwenden der Microsoft Authenticator-App muss der Benutzer Pushbenachrichtigungen für sein Gerät aktivieren.

Die Seite zeigt dann einen Aktivierungscode und eine URL zusammen mit einem Barcodebild an. Wenn der Benutzer bei der Authentifizierung eine PIN verwenden muss, fordert ihn die Seite zusätzlich zur Eingabe einer PIN auf. Der Benutzer gibt den Aktivierungscode und die URL in die Microsoft Authenticator-App ein oder verwendet den Barcodescanner, um das Barcodebild zu scannen, und klickt auf die Schaltfläche „Aktivieren“.

Nach Abschluss der Aktivierung klickt der Benutzer auf die Schaltfläche **Jetzt authentifizieren**. Azure Multi-Factor Authentication führt eine Überprüfung über die mobile App des Benutzers durch. Der Benutzer muss seine PIN (falls zutreffend) eingeben die Schaltfläche „Authentifizieren“ in seiner mobilen App drücken, um mit dem nächsten Schritt der Selbstregistrierung fortzufahren.

Wenn die Administratoren den Azure Multi-Factor Authentication-Server für das Erfassen von Sicherheitsfragen und -antworten konfiguriert haben, wird der Benutzer auf die Seite mit den Sicherheitsfragen weitergeleitet. Der Benutzer muss vier Sicherheitsfragen auswählen und die entsprechenden Antworten bereitstellen.

![Benutzerportal-Sicherheitsfragen](./media/multi-factor-authentication-get-started-portal/secq.png)

Die Selbstregistrierung des Benutzers ist jetzt abgeschlossen, und der Benutzer wird am Benutzerportal angemeldet. Benutzer können sich jederzeit erneut beim Benutzerportal anmelden, um ihre Telefonnummern, PINs, Authentifizierungsmethoden und Sicherheitsfragen zu ändern, sofern die Änderung der Methoden von den Administratoren zugelassen wurde.

## <a name="next-steps"></a>Nächste Schritte

- [Aktivieren der Mobile App-Authentifizierung mit dem Azure Multi-Factor Authentication-Server](multi-factor-authentication-get-started-server-webservice.md)
