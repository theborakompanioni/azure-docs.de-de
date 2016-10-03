<properties
	pageTitle="Erstmaliges Anmelden mit Azure Multi-Factor Authentication"
	description="Auf dieser Seite werden die bei der erstmaligen Anmeldung erforderlichen Schritte beschrieben."
	services="multi-factor-authentication"
	keywords="Verwenden von Azure Active Directory, Active Directory in der Cloud, Tutorial zu Active Directory"
	documentationCenter=""
	authors="kgremban"
	manager="femila"
	editor="curtland"/>

<tags
	ms.service="multi-factor-authentication"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/22/2016"
	ms.author="kgremban"/>
# Einrichten von Azure Multi-Factor Authentication

 Es werden Einstellungen für die zusätzliche Sicherheitsüberprüfung verwendet, wenn ein Administrator Ihr Konto so konfiguriert hat, dass Ihr Kennwort und eine Antwort von Ihrem Telefon erforderlich sind, um Ihre Identität zu überprüfen. Wenn ein Administrator Ihr Konto für die zusätzliche Sicherheitsüberprüfung konfiguriert hat, **ist eine Anmeldung erst möglich, wenn Sie den automatischen Registrierungsprozess abgeschlossen haben**.

## Überprüfungsmethoden für Multi-Factor Authentication

 Nachdem Ihr Konto konfiguriert wurde, werden Sie bei der ersten Anmeldung aufgefordert, die automatische Registrierung durchzuführen. Klicken Sie zum Starten des Vorgangs auf **Jetzt einrichten**.

![Einrichtung](./media/multi-factor-authentication-end-user-first-time/first.png)

Während des Registrierungsprozesses können Sie Ihre bevorzugte Methode für die Überprüfung angeben. Sie können eine beliebige Methode aus der folgenden Tabelle auswählen. Klicken Sie auf eine der Methoden, um weitere Informationen einschließlich einer Anleitung anzuzeigen.

Methode|Beschreibung
:------------- | :------------- |
[Anruf beim Mobiltelefon](multi-factor-authentication-end-user-first-time-mobile-phone.md)| Startet einen automatisierten Anruf an das Telefon für die Authentifizierung. Der Benutzer nimmt den Anruf an und drückt die #-Taste auf der Telefontastatur, um sich zu authentifizieren. Diese Telefonnummer wird nicht mit dem lokalen Active Directory synchronisiert.
[Textnachricht an Mobiltelefon](multi-factor-authentication-end-user-first-time-mobile-phone.md)|Sendet eine Textnachricht mit einem Überprüfungscode an den Benutzer. Der Benutzer wird aufgefordert, mit dem Überprüfungscode auf die Textnachricht zu antworten oder den Überprüfungscode auf der Anmeldeseite einzugeben.
[Anruf beim Bürotelefon](multi-factor-authentication-end-user-first-time-office-phone.md)|Startet einen automatisierten Anruf an das Telefon des Benutzers. Der Benutzer nimmt den Anruf an und drückt die #-Taste auf der Telefontastatur, um sich zu authentifizieren.
[Mobile App](multi-factor-authentication-end-user-first-time-mobile-app.md)|Es wird eine Benachrichtigung an die Microsoft Authenticator-App auf dem Smartphone oder Tablet des Benutzers gesendet. Der Benutzer tippt in der App auf "Überprüfen", um sich zu authentifizieren. Alternativ kann die App auch als OTP-Token für die Offline-Authentifizierung verwendet werden. Der Benutzer gibt das Token auf der Anmeldeseite ein, um sich zu authentifizieren.<br><p> Die Microsoft Authenticator-App kann in zwei verschiedenen Modi verwendet werden, um die zusätzliche Sicherheit eines Multi-Factor Authentication-Diensts bereitzustellen. Folgende Modi werden unterstützt:<li>**Benachrichtigung**: In diesem Modus verhindert die Microsoft Authenticator-App nicht autorisierte Zugriffe auf Konten sowie betrügerische Transaktionen. Zu diesem Zweck wird eine Pushbenachrichtigung an Ihr Telefon oder registriertes Gerät gesendet. Überprüfen Sie einfach die Benachrichtigung, und wählen Sie "Authentifizieren", wenn Sie den Zugriff zulassen möchten. Bei einem nicht autorisierten Zugriff können Sie den Zugriff verweigern und optional die betrügerische Benachrichtigung melden. Informationen zum Melden von betrügerischen Benachrichtigungen finden Sie unter „Verwenden der Funktion zum Verweigern und Melden für Multi-Factor Authentication“.</li><p><li>**Einmalkennwort**: In diesem Modus kann die Microsoft Authenticator-App als Softwaretoken verwendet werden, um einen OATH-Überprüfungscode zu generieren. Dieser Überprüfungscode kann dann zusammen mit dem Benutzernamen und Kennwort als zweite Authentifizierungsmethode eingegeben werden.</li><br><p> Die Microsoft Authenticator-App ist für [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072) und [iOS](http://go.microsoft.com/fwlink/?Linkid=825073) verfügbar.

<!---HONumber=AcomDC_0921_2016-->