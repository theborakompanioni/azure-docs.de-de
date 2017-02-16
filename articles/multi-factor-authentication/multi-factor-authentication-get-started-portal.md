---
title: "Benutzerportal für Azure MFA-Server | Microsoft-Dokumentation"
description: "Dies ist die Seite für Azure Multi-Factor Authentication, auf der die ersten Schritte mit Azure MFA und dem Benutzerportal beschrieben werden."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 06b419fa-3507-4980-96a4-d2e3960e1772
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/04/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 1222223f8c45249402bfdd04c8754074f877e132
ms.openlocfilehash: 1236489212b2a9c421972599a12511d5bc42efdf


---
# <a name="deploy-the-user-portal-for-the-azure-multi-factor-authentication-server"></a>Bereitstellen des Benutzerportals für den Azure Multi-Factor Authentication-Server
Im Benutzerportal kann der Administrator das Azure Multi-Factor Authentication-Benutzerportal installieren und konfigurieren. Das Benutzerportal ist eine IIS-Website, die es Benutzern ermöglicht, sich für Azure Multi-Factor Authentication zu registrieren und ihre Konten zu verwalten. Die Benutzer können ihre Telefonnummer oder PIN ändern oder die Überprüfung in zwei Schritten bei der nächsten Anmeldung umgehen.

Die Benutzer melden sich mit ihrem normalen Benutzernamen und Kennwort beim Benutzerportal an und rufen dann entweder die Überprüfung in zwei Schritten auf oder beantworten Sicherheitsfragen, um die Authentifizierung abzuschließen. Wenn die Benutzerregistrierung zulässig ist, konfigurieren Benutzer ihre Telefonnummer und PIN, wenn sie sich zum ersten Mal beim Benutzerportal anmelden.

Es können Benutzerportaladministratoren eingerichtet werden, und ihnen kann die Berechtigung zum Hinzufügen neuer Benutzer und Aktualisieren vorhandener Benutzer erteilt werden.

<center>![Einrichtung](./media/multi-factor-authentication-get-started-portal/install.png)</center>

## <a name="deploy-the-user-portal-on-the-same-server-as-the-azure-multi-factor-authentication-server"></a>Bereitstellen des Benutzerportals auf demselben Server wie für den Azure Multi-Factor Authentication-Server
Die folgenden Voraussetzungen müssen erfüllt sein, um das Benutzerportal auf demselben Server wie für den Azure Multi-Factor Authentication-Server zu installieren:

* IIS, einschließlich ASP.NET und IIS 6-Metabasiskompatibilität (für IIS 7 oder höher)
* Der angemeldete Benutzer muss über Administratorrechte für den Computer und ggf. die Domäne verfügen.  Der Grund dafür ist, dass das Konto Berechtigungen zum Erstellen von Active Directory-Sicherheitsgruppen benötigt.

### <a name="to-deploy-the-user-portal"></a>So stellen Sie das Benutzerportal bereit
1. Klicken Sie auf dem Azure Multi-Factor Authentication-Server im linken Menü auf das Symbol für das **Benutzerportal** und dann auf **Benutzerportal installieren**.
2. Klicken Sie auf **Weiter**.
3. Klicken Sie auf **Weiter**.
4. Wenn der Computer in die Domäne eingebunden ist und Active Directory nicht für den Schutz der Kommunikation zwischen dem Benutzerportal und dem Azure Multi-Factor Authentication-Dienst konfiguriert ist, wird der Active Directory-Schritt angezeigt. Klicken Sie auf die Schaltfläche **Weiter**, um diese Konfiguration automatisch abzuschließen.
5. Klicken Sie auf **Weiter**.
6. Klicken Sie auf **Weiter**.
7. Klicken Sie auf **Schließen**.
8. Öffnen Sie auf einem beliebigen Computer einen Webbrowser, und navigieren Sie zu der URL, unter der das Benutzerportal installiert wurde (z.B. „https://www.publicwebsite.com/MultiFactorAuth“). Stellen Sie sicher, dass keine Zertifikatswarnungen oder -fehler angezeigt werden.

<center>![Einrichtung](./media/multi-factor-authentication-get-started-portal/portal.png)</center>

## <a name="deploy-the-azure-multi-factor-authentication-server-user-portal-on-a-separate-server"></a>Bereitstellen des Azure Multi-Factor Authentication-Server-Benutzerportals auf einem anderen Server
Erfüllen Sie die folgenden Voraussetzungen, damit die Microsoft Authenticator-App mit dem Benutzerportal kommunizieren kann: 

* Sie müssen Version 6.0 oder höher des Azure Multi-Factor Authentication-Servers verwenden.
* Das Benutzerportal muss auf einem Webserver mit Internetzugriff installiert sein, auf dem Microsoft® Internetinformationsdienste (IIS) 6.x, 7.x oder höher ausgeführt wird.
* Stellen Sie bei Verwendung von IIS 6.x sicher, dass ASP.NET v2.0.50727 installiert, registriert und auf **Zulässig** festgelegt ist.
* Bei Verwendung von IIS 7.x  oder höher erforderliche Rollendienste umfassen ASP.NET und IIS 6-Metabasiskompatibilität.
* Das Benutzerportal sollte per SSL-Zertifikat geschützt sein.
* In IIS 6.x, IIS 7.x oder höher muss das Azure Multi-Factor Authentication-Webdienst-SDK auf dem Server installiert sein, auf dem der Azure Multi-Factor Authentication-Server installiert ist.
* Das Azure Multi-Factor Authentication-Webdienst-SDK muss mit einem SSL-Zertifikat gesichert sein.
* Das Benutzerportal muss eine SSL-Verbindung mit dem Azure Multi-Factor Authentication-Webdienst-SDK herstellen können.
* Das Benutzerportal muss sich beim Azure Multi-Factor Authentication-Webdienst-SDK mit den Anmeldeinformationen eines Dienstkontos der Sicherheitsgruppe „PhoneFactor Admins“ authentifizieren können. Dieses Dienstkonto und diese Gruppe sind in Active Directory vorhanden, wenn der Azure Multi-Factor Authentication-Server auf einem in die Domäne eingebundenen Server ausgeführt wird. Dieses Dienstkonto und diese Gruppe sind lokal auf dem Azure Multi-Factor Authentication-Server vorhanden, wenn dieser nicht in eine Domäne eingebunden ist.

Zum Installieren des Benutzerportals auf einem anderen Server als dem Azure Multi-Factor Authentication-Server sind die folgenden drei Schritte erforderlich:

1. Installieren des Webdienst-SDK
2. Installieren des Benutzerportals
3. Konfigurieren der Benutzerportal-Einstellungen im Azure Multi-Factor Authentication-Server

### <a name="step-1-install-the-web-service-sdk"></a>Schritt 1: Installieren des Webdienst-SDK
Wenn das Azure Multi-Factor Authentication-Webdienst-SDK nicht bereits auf dem Azure Multi-Factor Authentication-Server installiert ist, wechseln Sie zu diesem Server, und öffnen Sie den Azure Multi-Factor Authentication-Server. Klicken Sie auf das Symbol des **Webdienst-SDK** und dann auf **Webdienst-SDK installieren**. Befolgen Sie die angezeigten Anweisungen. 

Ein selbstsigniertes Zertifikat ist für diesen Zweck zulässig. Ein selbstsigniertes Zertifikat ist für diese Zwecke geeignet, aber es muss in den Speicher „Vertrauenswürdige Stammzertifizierungsstellen“ des Kontos „Lokaler Computer“ auf dem Server importiert werden. Dieses Zertifikat wird vom Webdienst-SDK jetzt als vertrauenswürdig angesehen, wenn die SSL-Verbindung initiiert wird.

<center>![Einrichtung](./media/multi-factor-authentication-get-started-portal/sdk.png)</center>

### <a name="step-2-install-the-user-portal"></a>Schritt 2: Installieren des Benutzerportals
Beachten Sie die folgenden bewährten Methoden, bevor Sie das Benutzerportal auf einem anderen Server installieren:

* Es ist hilfreich, auf dem mit dem Internet verbundenen Webserver einen Webbrowser zu öffnen und die URL des Webdienst-SDK aufzurufen, die in die Datei "Web.config" eingegeben wurde. Wenn der Webdienst erfolgreich im Browser aufgerufen werden kann, werden Sie zur Eingabe von Anmeldeinformationen aufgefordert. Geben Sie den Benutzernamen und das Kennwort, die in die Datei "Web.config" eingegeben wurden, genau wie in der Datei angezeigt ein. Stellen Sie sicher, dass keine Zertifikatswarnungen oder -fehler angezeigt werden.
* Wenn sich vor dem Benutzerportal-Webserver ein Reverseproxy oder eine Firewall befindet und die SSL-Abladung durchführt, können Sie die Datei „web.config“ für das Benutzerportal bearbeiten und dem `<appSettings>`-Abschnitt den folgenden Schlüssel hinzufügen, damit das Benutzerportal HTTP anstelle von HTTPS verwenden kann:

    `<add key="SSL_REQUIRED" value="false"/>`

#### <a name="to-install-the-user-portal"></a>So installieren Sie das Benutzerportal
1. Öffnen Sie auf dem Azure Multi-Factor Authentication-Server den Windows-Explorer, und navigieren Sie zu dem Ordner, in dem der Azure Multi-Factor Authentication-Server installiert ist (z. B. C:\Programme\Multi-Factor Authentication Server). Wählen Sie entsprechend dem Server, auf dem das Benutzerportal installiert wird, die 32-Bit- oder 64-Bit-Version der Datei "MultiFactorAuthenticationUserPortalSetup" aus. Kopieren Sie die Installationsdatei auf den mit dem Internet verbundenen Server.
2. Die Setupdatei muss auf dem mit dem Internet verbundenen Webserver mit Administratorrechten ausgeführt werden. Dies lässt sich am einfachsten erreichen, indem Sie eine Eingabeaufforderung als Administrator öffnen und zu dem Verzeichnis navigieren, in das die Installationsdatei kopiert wurde.
3. Führen Sie die Installationsdatei "MultiFactorAuthenticationUserPortalSetup64" aus, und ändern Sie ggf. den Namen der Website und des virtuellen Verzeichnisses.
4. Navigieren Sie nach Abschluss der Installation des Benutzerportals zu "C:\inetpub\wwwroot\MultiFactorAuth" (oder zum entsprechenden Verzeichnis basierend auf dem Namen des virtuellen Verzeichnisses), und bearbeiten Sie die Datei "Web.config".
5. Suchen Sie den Schlüssel USE_WEB_SERVICE_SDK, und ändern Sie den Wert von "false" in "true". Suchen Sie die Schlüssel WEB_SERVICE_SDK_AUTHENTICATION_USERNAME und WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD, und legen Sie die Werte auf den Benutzernamen und das Kennwort des Dienstkontos der Sicherheitsgruppe „PhoneFactor Admins“ fest (siehe Abschnitt zu den Voraussetzungen). Geben Sie den Benutzernamen und das Kennwort zwischen den Anführungszeichen am Ende der Zeile ein (value=""/>). Es ist ratsam, einen qualifizierten Benutzernamen (z.B. „Domäne\Benutzername“ oder „Computer\Benutzername“) zu verwenden.
6. Suchen Sie die Einstellung "pfup_pfwssdk_PfWsSdk", und ändern Sie den Wert "http://localhost:4898/PfWsSdk.asmx" in die URL des Webdienst-SDK, das auf dem Azure Multi-Factor Authentication-Server ausgeführt wird (z. B. "https://computer1.domain.local/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx"). Da für diese Verbindung SSL verwendet wird, sollten Sie anhand des Servernamens auf das Webdienst-SDK verweisen, nicht anhand der IP-Adresse. Das SSL-Zertifikat wurde für den Servernamen ausgestellt. Wenn der Servername nicht als IP-Adresse des Servers mit Internetzugriff aufgelöst wird, fügen Sie der HOSTS-Datei auf diesem Server einen Eintrag hinzu, um den Namen des Azure Multi-Factor Authentication-Servers seiner IP-Adresse zuzuordnen. Speichern Sie die Datei "Web.config", nachdem Änderungen vorgenommen wurden.

    Weitere Informationen zur Bearbeitung der config-Datei finden Sie unter [Schützen von Cloudressourcen und lokalen Ressourcen mithilfe von Azure Multi-Factor Authentication-Server mit AD FS unter Windows Server 2012 R2](multi-factor-authentication-get-started-adfs-w2k12.md#edit-the-multifactorauthenticationadfsadapterconfig-file).

7. Wenn die Website, unter der das Benutzerportal installiert wurde (z.B. die Standardwebsite), noch nicht mit einem öffentlich signierten Zertifikat gebunden wurde, installieren Sie das Zertifikat auf dem Server. Öffnen Sie dann den IIS-Manager, und binden Sie das Zertifikat an die Website.
8. Öffnen Sie auf einem beliebigen Computer einen Webbrowser, und navigieren Sie zu der URL, unter der das Benutzerportal installiert wurde (z.B. „https://www.publicwebsite.com/MultiFactorAuth“). Stellen Sie sicher, dass keine Zertifikatswarnungen oder -fehler angezeigt werden.

### <a name="step-3-configure-the-user-portal-settings-in-the-azure-multi-factor-authentication-server"></a>Schritt 3: Konfigurieren der Benutzerportal-Einstellungen im Azure Multi-Factor Authentication-Server
Nachdem das Benutzerportal installiert wurde, müssen Sie den Azure Multi-Factor Authentication-Server für die Verwendung des Portals konfigurieren.

Der Azure Multi-Factor Authentication-Server stellt mehrere Optionen für das Benutzerportal bereit.  Die folgende Tabelle enthält eine Liste dieser Optionen sowie eine Erläuterung zu deren Verwendung.

| Benutzerportal – Einstellungen | Beschreibung |
|:--- |:--- |
| Benutzerportal-URL | Geben Sie die URL ein, unter der das Portal gehostet wird. |
| Primäre Authentifizierung | Geben Sie den Typ der Authentifizierung für die Anmeldung beim Portal an.  Mögliche Optionen: Windows-, Radius- oder LDAP-Authentifizierung. |
| Benutzeranmeldung zulassen | Ermöglicht Benutzern die Eingabe von Benutzername und Kennwort auf der Anmeldeseite des Benutzerportals.  Wenn diese Option nicht ausgewählt ist, sind die Felder abgeblendet. |
| Benutzerregistrierung zulassen | Ermöglicht dem Benutzer die Registrierung bei der Multi-Factor Authentication. Dazu wird der Benutzer auf einem Setupbildschirm zur Eingabe zusätzlicher Informationen wie beispielsweise einer Telefonnummer aufgefordert. Die Eingabeaufforderung für die Ersatzrufnummer ermöglicht dem Benutzer die Angabe einer sekundären Telefonnummer. Die Eingabeaufforderung für das Drittanbieter-OATH-Token ermöglicht dem Benutzer die Angabe eines entsprechenden Tokens. |
| Benutzern das Initiieren einer Einmalumgehung ermöglichen | Ermöglicht Benutzern das Initiieren einer Einmalumgehung.  Wenn ein Benutzer diese Einstellung festlegt, wird sie bei seiner nächsten Anmeldung übernommen. Die Eingabeaufforderung für die Gültigkeit der Umgehung in Sekunden zeigt dem Benutzer ein Feld an, in dem er den Standardwert von 300 Sekunden ändern kann. Andernfalls gilt die Einmalumgehung nur für 300 Sekunden. |
| Benutzern die Auswahl der Methode ermöglichen | Ermöglicht Benutzern das Festlegen ihrer primären Kontaktmethode.  Dabei kann es sich um einen Telefonanruf, eine Textnachricht, die mobile App oder ein OATH-Token handeln. |
| Benutzern die Auswahl der Sprache ermöglichen | Ermöglicht Benutzern das Ändern der Sprache, die für den Telefonanruf, die Textnachricht (SMS), die mobile App oder das OATH-Token verwendet wird. |
| Benutzern das Aktivieren der mobilen App ermöglichen | Ermöglicht Benutzern das Generieren eines Aktivierungscodes, um den Aktivierungsprozess der mit dem Server verwendeten mobilen App durchzuführen.  Sie können auch die Anzahl der Geräte festlegen (1 bis 10), auf denen Benutzer die App aktivieren können. |
| Sicherheitsfragen als Alternative verwenden | Ermöglicht die Verwendung von Sicherheitsfragen, falls die Überprüfung in zwei Schritten fehlschlägt.  Sie können die Anzahl der Sicherheitsfragen festlegen, die richtig beantwortet werden müssen. |
| Benutzern das Zuordnen von Drittanbieter-OATH-Token ermöglichen | Ermöglicht Benutzern das Angeben eines Drittanbieter-OATH-Tokens. |
| OATH-Token als Alternative verwenden | Ermöglicht die Verwendung eines OATH-Tokens, falls die Überprüfung in zwei Schritten nicht erfolgreich ist. Sie können auch den Sitzungstimeout in Minuten angeben. |
| Aktivieren der Protokollierung | Ermöglicht die Protokollierung im Benutzerportal. Die Protokolldateien befinden sich unter: C:\Programme\Multi-Factor Authentication Server\Logs. |

Die meisten dieser Einstellungen sind für den Benutzer sichtbar, sobald sie aktiviert wurden und der Benutzer sich beim Benutzerportal angemeldet hat.

![Benutzerportal – Einstellungen](./media/multi-factor-authentication-get-started-portal/portalsettings.png)

### <a name="to-configure-the-user-portal-settings-in-the-azure-multi-factor-authentication-server"></a>So konfigurieren Sie die Benutzerportal-Einstellungen im Azure Multi-Factor Authentication-Server
1. Klicken Sie auf dem Azure Multi-Factor Authentication-Server auf das Symbol **Benutzerportal**. Geben Sie auf der Registerkarte „Einstellungen“ im Textfeld **Benutzerportal-URL** die URL des Benutzerportals ein. Sofern die E-Mail-Funktion aktiviert wurde, wird diese URL in E-Mails eingefügt, die an Benutzer gesendet werden, wenn sie in den Azure Multi-Factor Authentication-Server importiert werden.
2. Wählen Sie die Einstellungen aus, die Sie im Benutzerportal verwenden möchten. Wenn beispielsweise Benutzer ihre Authentifizierungsmethoden steuern dürfen, sollten Sie sicherstellen, dass die Option **Methodenauswahl durch Benutzer zulassen** zusammen mit den zur Auswahl stehenden Methoden aktiviert ist.
3. Klicken Sie auf den Link **Hilfe** in der rechten oberen Ecke, um Erläuterungen zu den angezeigten Einstellungen zu erhalten.

<center>![Einrichtung](./media/multi-factor-authentication-get-started-portal/config.png)</center>


## <a name="administrators-tab"></a>Registerkarte „Administratoren“
Auf dieser Registerkarte können Sie problemlos Benutzer hinzufügen, die über Administratorrechte verfügen sollen.  Beim Hinzufügen eines Administrators können Sie seine Berechtigungen präzise abstimmen. Klicken Sie auf die Schaltfläche **Hinzufügen**, wählen Sie einen Benutzer und die entsprechenden Berechtigungen aus, und klicken Sie dann auf **Hinzufügen**.

![Benutzerportaladministratoren](./media/multi-factor-authentication-get-started-portal/admin.png)

## <a name="security-questions"></a>Sicherheitsfragen
Auf dieser Registerkarte können Sie die Sicherheitsfragen angeben, die Benutzer beantworten müssen, wenn die Option **Sicherheitsfragen als Alternative verwenden** ausgewählt ist.  Der Azure Multi-Factor Authentication-Server verfügt über Standardfragen, die Sie verwenden können. Sie können die Reihenfolge ändern oder eigene Fragen hinzufügen.  Beim Hinzufügen eigener Fragen können Sie die Sprache angeben, in der diese angezeigt werden sollen.

![Benutzerportal-Sicherheitsfragen](./media/multi-factor-authentication-get-started-portal/secquestion.png)

## <a name="saml"></a>SAML
Verwenden Sie diese Registerkarte, um das Benutzerportal für das Akzeptieren von Ansprüchen von einem Identitätsanbieter per SAML zu konfigurieren.  Sie können die Timeout-Sitzung, das Verifizierungszertifikat und die URL für die Umleitung zur Abmeldung angeben.

![SAML](./media/multi-factor-authentication-get-started-portal/saml.png)

## <a name="trusted-ips"></a>Vertrauenswürdige IP-Adressen
Auf dieser Registerkarte können Sie entweder einzelne IP-Adressen oder IP-Adressbereiche angeben, die hinzugefügt werden können, damit Benutzer, die sich über eine dieser Adressen anmelden, nicht die Überprüfung in zwei Schritten durchführen müssen.

![Vertrauenswürdige IPs des Benutzerportals](./media/multi-factor-authentication-get-started-portal/trusted.png)

## <a name="self-service-user-enrollment"></a>Self-Service-Benutzerregistrierung
Wenn Sie möchten, dass Ihre Benutzer sich anmelden und registrieren, müssen Sie auf der Registerkarte „Einstellungen“ die Optionen **Benutzeranmeldung zulassen** und **Benutzerregistrierung zulassen** auswählen. Denken Sie daran, dass die ausgewählten Einstellungen die Benutzeranmeldung beeinflussen.

Wenn sich ein Benutzer beispielsweise zum ersten Mal beim Benutzerportal anmeldet, wird er auf die Seite zur Benutzereinrichtung von Azure Multi-Factor Authentication geleitet.  Je nachdem, wie Sie Azure Multi-Factor Authentication konfiguriert haben, kann der Benutzer seine Authentifizierungsmethode auswählen.  

Wenn er die Überprüfungsmethode „Sprachanruf“ auswählt oder für die Verwendung dieser Methode vorkonfiguriert wurde, fordert die Seite den Benutzer zur Eingabe seiner primären Rufnummer und ggf. der Durchwahl auf.  Möglicherweise kann er auch eine Ersatzrufnummer eingeben.  

![Vertrauenswürdige IPs des Benutzerportals](./media/multi-factor-authentication-get-started-portal/backupphone.png)

Wenn der Benutzer bei der Authentifizierung eine PIN verwenden muss, fordert ihn die Seite auch zur Erstellung einer PIN auf.  Nach der Eingabe der Telefonnummer(n) und PIN (falls zutreffend) klickt der Benutzer auf die Schaltfläche **Jetzt für Authentifizierung angerufen werden**.  Azure Multi-Factor Authentication führt eine Überprüfung per Telefonanruf an die primäre Telefonnummer des Benutzers durch.  Der Benutzer muss den Anruf annehmen, ggf. seine PIN eingeben und # drücken, um zum nächsten Schritt der Selbstregistrierung zu gelangen.   

Wenn der Benutzer die Überprüfung per SMS auswählt oder für die Verwendung dieser Methode vorkonfiguriert wurde, fordert die Seite den Benutzer auf, seine Mobiltelefonnummer anzugeben.  Wenn der Benutzer bei der Authentifizierung eine PIN verwenden muss, fordert ihn die Seite auch zur Eingabe einer PIN auf.  Nach der Eingabe der Telefonnummer und PIN (falls zutreffend) klickt der Benutzer auf die Schaltfläche **Jetzt per SMS authentifizieren**.  Azure Multi-Factor Authentication führt eine SMS-Überprüfung über das Mobiltelefon des Benutzers durch.  Der Benutzer empfängt die SMS mit einer Einmalkennung (One-Time Passcode, OTP) und sendet dann eine Antwort auf die Nachricht mit der Einmalkennung und seiner PIN (falls zutreffend).

![Benutzerportal-SMS](./media/multi-factor-authentication-get-started-portal/text.png)   

Wenn der Benutzer die Überprüfungsmethode per mobiler App wählt, wird er aufgefordert, die Microsoft Authenticator-App auf seinem Gerät zu installieren und einen Aktivierungscode zu generieren.  Nach der Installation der App klickt der Benutzer auf die Schaltfläche „Aktivierungscode generieren“.    

> [!NOTE]
> Zum Verwenden der Microsoft Authenticator-App muss der Benutzer Pushbenachrichtigungen für sein Gerät aktivieren.

Die Seite zeigt dann einen Aktivierungscode und eine URL zusammen mit einem Barcodebild an.  Wenn der Benutzer bei der Authentifizierung eine PIN verwenden muss, fordert ihn die Seite auch zur Eingabe einer PIN auf.  Der Benutzer gibt den Aktivierungscode und die URL in die Microsoft Authenticator-App ein oder verwendet den Barcodescanner, um das Barcodebild zu scannen, und klickt auf die Schaltfläche „Aktivieren“.    

Nach Abschluss der Aktivierung klickt der Benutzer auf die Schaltfläche **Jetzt authentifizieren**.  Azure Multi-Factor Authentication führt eine Überprüfung über die mobile App des Benutzers durch.  Der Benutzer muss seine PIN (falls zutreffend) eingeben die Schaltfläche „Authentifizieren“ in seiner mobilen App drücken, um mit dem nächsten Schritt der Selbstregistrierung fortzufahren.  

Wenn die Administratoren den Azure Multi-Factor Authentication-Server für das Erfassen von Sicherheitsfragen und -antworten konfiguriert haben, wird der Benutzer auf die Seite mit den Sicherheitsfragen weitergeleitet.  Der Benutzer muss vier Sicherheitsfragen auswählen und die entsprechenden Antworten bereitstellen.    

![Benutzerportal-Sicherheitsfragen](./media/multi-factor-authentication-get-started-portal/secq.png)  

Die Selbstregistrierung des Benutzers ist jetzt abgeschlossen, und der Benutzer wird am Benutzerportal angemeldet.  Benutzer können sich jederzeit erneut am Benutzerportal anmelden, um ihre Telefonnummern, PINs, Authentifizierungsmethoden und Sicherheitsfragen zu ändern, wenn die Administratoren dies zulassen.




<!--HONumber=Feb17_HO3-->


