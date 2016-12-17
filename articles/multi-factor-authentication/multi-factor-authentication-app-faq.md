---
title: "Microsoft Authenticator-App – häufig gestellte Fragen"
description: "Enthält eine Liste mit häufig gestellten Fragen und Antworten im Zusammenhang mit Microsoft Authenticator-App und Azure Multi-Factor Authentication (MFA)."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: pblachar, librown
ms.assetid: f04d5bce-e99e-4f75-82d1-ef6369be3402
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/02/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: dbe949dac15b45ba038ed675873f3da1fb1ff86a


---
# <a name="microsoft-authenticator-application-faq"></a>Microsoft Authenticator-App – häufig gestellte Fragen
Die Microsoft Authenticator-App ersetzt die Azure Authenticator-App und ist die empfohlene App, wenn Sie Azure Multi-Factor Authentication verwenden. Diese App ist für Windows Phone, Android und iOS verfügbar.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen
### <a name="what-happened-to-the-azure-authenticator-multi-factor-auth-and-microsoft-account-apps"></a>Was ist mit den Azure Authenticator-, Multi-Factor Authenticator- und Microsoft-Konto-Apps passiert?
Die Microsoft Authenticator-App ersetzt die anderen Apps. Azure Authenticator wurde auf Microsoft Authenticator aktualisiert. Wenn Sie die Multi-Factor Auth- oder Microsoft-Konto-Apps verwenden, installieren Sie Microsoft Authenticator, und fügen Sie Ihre Konten erneut hinzu. Stellen Sie sicher, dass alle Ihre Konten zur neuen App hinzugefügt wurden, bevor Sie die alten Apps löschen.

### <a name="im-already-using-the-microsoft-authenticator-application-for-verification-codes-how-do-i-switch-to-one-click-push-notifications"></a>Ich verwende für Sicherheitscodes bereits die Microsoft Authenticator-App. Wie wechsle ich zu One-Click-Pushbenachrichtigungen?
Das Genehmigen einer Anmeldung durch Pushbenachrichtigungen steht nur für Microsoft-Konten zur Verfügung, nicht für Konten von Drittanbietern wie Google oder Facebook. Bei Geschäfts- oder Schulkonten bei Microsoft kann Ihre Organisation allerdings entscheiden, diese Option zu deaktivieren.

Wenn Sie ein Microsoft-Konto als persönliches Konto verwenden, und zu Pushbenachrichtigungen wechseln möchten, müssen Sie Ihr Konto wieder hinzufügen. Registrieren Sie das Gerät erneut mit Ihrem Konto, und richten Sie Pushbenachrichtigungen ein.  

Wenn Sie Microsoft Authenticator für Ihr Geschäfts- oder Schulkonto verwenden, entscheidet Ihre Organisation, ob sie One-Click-Benachrichtigungen zulässt.

### <a name="when-will-i-be-able-to-use-one-click-push-notifications-on-iphone-or-ipad"></a>Wann kann ich One-Click-Pushbenachrichtigungen auf dem iPhone oder iPad verwenden?
Diese Funktion ist bis Ende August in der Betaversion enthalten. Ab dann wird sie allgemein für Microsoft-Konten verfügbar sein. Wenn Sie an unserem Betaprogramm teilnehmen möchten, senden Sie eine E-Mail an msauthenticator@microsoft.com.. Ihre Nachricht muss Ihren Vornamen, Nachnamen und Ihre Apple-ID enthalten.  

### <a name="do-one-click-push-notifications-work-for-non-microsoft-accounts"></a>Funktionieren One-Click-Pushbenachrichtigungen für Nicht-Microsoft-Konten?
Nein, Pushbenachrichtigungen funktionieren nur mit Microsoft-Konten und Azure Active Directory-Konten. Wenn Ihre Organisation oder Schule Azure AD-Konten verwendet, kann sie diese Funktion deaktivieren.  

### <a name="i-restored-my-device-from-a-backup-and-my-account-codes-are-missing-or-not-working-what-happened"></a>Ich habe mein Gerät aus einer Sicherung wiederhergestellt, doch meine Kontocodes fehlen oder funktionieren nicht. Was ist passiert?
Aus Sicherheitsgründen werden keine Konten aus App-Sicherungen wiederhergestellt. Wenn Sie die iOS-App aus einer Sicherung wiederherstellen, werden Ihre Konten zwar weiterhin angezeigt, ohne allerdings Anmeldeüberprüfungen zu empfangen oder Sicherheitscodes zu generieren. Nach der Wiederherstellung der App müssen Sie Ihre Konten löschen und dann erneut hinzufügen.

### <a name="i-got-a-new-device-how-do-i-remove-the-microsoft-authenticator-app-from-my-old-device-and-move-to-the-new-one"></a>Ich habe ein neues Gerät. Wie kann ich Microsoft Authenticator von meinem alten Gerät entfernen, und zum neuen Gerät wechseln?
Das Installieren der Microsoft Authenticator-App auf einem neuen Gerät entfernt die App nicht automatisch von anderen Geräten. Wenn Sie verwalten möchten, welche Geräte für Ihr Konto konfiguriert werden, besuchen Sie die gleiche Website, mit der Sie die Überprüfung in zwei Schritten verwalten, und wählen Sie aus, alte Apps zu entfernen.

Bei persönlichen Microsoft-Konten ist dies die Website [Kontosicherheit](https://account.microsoft.com/security). Bei Geschäfts- oder Schulkonten bei Microsoft ist dies entweder die Website [MyApps](https://myapps.microsoft.com), oder ein benutzerdefiniertes Portal, dass Ihre Organisation eingerichtet hat.

### <a name="how-do-i-remove-an-account-from-the-app"></a>Wie entferne ich ein Konto aus der App?
* iOS: Wischen Sie im Hauptbildschirm nach links zu einer Kontokachel. Klicken Sie auf **Löschen**.
* Windows Phone: Wählen Sie im Hauptbildschirm die Menütaste und anschließend **Konten bearbeiten** aus. Tippen Sie auf das **X** neben dem Kontonamen.
* Android: Wählen Sie im Hauptbildschirm die Menütaste und anschließend **Konten bearbeiten** aus. Tippen Sie auf das **X** neben dem Kontonamen.

Wenn Sie ein Android-Gerät besitzen, das bei Ihrer Organisation registriert ist, müssen Sie möglicherweise einen weiteren Schritt befolgen, um Ihr Konto zu entfernen. Auf diesen Geräten wird die Microsoft Authenticator-App automatisch als Geräteadministrator registriert. Wenn Sie die App vollständig deinstallieren möchten, müssen Sie die Registrierung der App zuerst in den App-Einstellungen aufheben.

### <a name="why-does-the-app-request-so-many-permissions"></a>Warum fordert die App so viele Berechtigungen an?
Hier finden Sie eine vollständige Liste der Berechtigungen, die wir erfragen, und wie diese in der App verwendet werden:

* **Kamera**: Wir verwenden Ihre Kamera, um QR-Codes zu scannen, wenn Sie ein Geschäfts-, Schulkonto oder ein nicht-Microsoft-Konto hinzufügen.
* **Kontakte und Telefon**: Wenn Sie sich mit Ihrem persönlichen Microsoft-Konto anmelden, versuchen wir, den Vorgang zu vereinfachen, indem wir bereits vorhandene Konten suchen, die Sie auf Ihrem Telefon verwenden.
* **SMS**: Wenn Sie sich zum ersten Mal mit Ihrem persönlichen Konto anmelden, müssen wir sicherstellen, dass Ihre Telefonnummer mit der bei uns registrierten Nummer übereinstimmt. Sie erhalten eine Textnachricht auf das Telefon, mit dem Sie die App heruntergeladen haben. Die Nachricht enthält einen Überprüfungscode mit 6 bis 8 Ziffern. Sie werden nicht aufgefordert, diesen Code zu suchen und in der App einzugeben, stattdessen suchen wir den Code für Sie in der Textnachricht.
* **Andere Apps in den Hintergrund rücken**: Wenn Sie eine Benachrichtigung erhalten, dass Sie Ihre Identität bestätigen müssen, wird diese Benachrichtigung im Vordergrund angezeigt, vor allen anderen Apps, die möglicherweise ausgeführt werden.
* **Daten aus dem Internet empfangen**: Diese Berechtigung ist für das Senden von Benachrichtigungen erforderlich.
* **Standby-Modus verhindern**: Wenn Sie Ihr Gerät bei Ihrer Organisation registrieren, können sie diese Richtlinie auf Ihrem Telefon ändern.
* **Vibrationsfunktion steuern**: Sie haben die Möglichkeit auszuwählen, ob Ihr Telefon vibrieren soll, wenn Sie eine Benachrichtigung erhalten, dass Sie Ihre Identität bestätigen müssen.
* **Verwendung der Fingerprint-Hardware**: Bei einigen Geschäfts- und Schulkonten wird eine zusätzliche PIN benötigt, wenn Sie Ihre Identität bestätigen. Sie können Ihren Fingerabdruck verwenden, um den Prozess zu vereinfachen, anstatt die PIN einzugeben.
* **Anzeigen von Netzwerkverbindungen**: Wenn Sie über ein Microsoft-Konto verfügen, benötigt die App eine Netzwerk-/Internetverbindung.
* **Lesen Ihres Speichers**: Diese Berechtigung ist nur notwendig, wenn Sie ein technisches Problem über die App-Einstellungen melden. Einige Informationen aus Ihrem Speicherkonto werden gesammelt, um das Problem zu diagnostizieren.
* **Vollständiger Netzwerkzugriff**: Diese Berechtigung ist erforderlich, um Benachrichtigungen senden zu können, mit denen Sie Ihre Identität bestätigen.
* **Bei Start automatisch starten**: Wenn Sie Ihr Telefon neu starten, stellt diese Berechtigung sicher, dass Sie weiterhin Benachrichtigungen erhalten, um Ihre Identität zu bestätigen.

## <a name="contact-us"></a>Kontakt
Wenn Ihre Frage hier nicht beantwortet wurde, hinterlassen Sie einen Kommentar unten auf der Seite. Andernfalls können Sie sich auch [an den Support wenden](https://support.microsoft.com/contactus), und wir werden so schnell wie möglich auf Ihr Problem antworten.

Wenn Sie sich an den Support wenden, halten Sie so viele der folgenden Informationen wie möglich bereit:

* **Benutzer-ID** – Mit welcher E-Mail-Adresse haben Sie versucht sich anzumelden?
* **Allgemeine Beschreibung des Fehlers** – Welche Fehlermeldung wurde Ihnen angezeigt?  Wenn keine Fehlermeldung angezeigt wird, beschreiben Sie das beobachtete unerwartete Verhalten so genau wie möglich.
* **Seite** – Auf welcher Seite haben Sie sich befunden, als der Fehler aufgetreten ist (einschließlich URL)?
* **ErrorCode** : Der Fehlercode, den Sie erhalten.
* **SessionId** : Die spezifische Sitzungs-ID, die Sie erhalten.
* **Korrelations-ID** : Der generierte Code der Korrelations-ID bei Anzeige des Fehlers.
* **Zeitstempel** : Genaues Datum samt Uhrzeit und Zeitzone des Auftretens des Fehlers.

Viele dieser Informationen finden Sie auf Ihrer Anmeldeseite. Wenn Sie Ihre Anmeldung nicht rechtzeitig bestätigen, wählen Sie **Details anzeigen** aus.

![Details zu Anmeldefehlern](./media/multi-factor-authentication-end-user-troubleshoot/view_details.png)

Diese Informationen helfen uns dabei, Ihr Problem so schnell wie möglich zu beheben.

## <a name="related-topics"></a>Verwandte Themen
* [Azure Multi-Factor Authentication – Häufig gestellte Fragen](multi-factor-authentication-faq.md)  
* [Überprüfung in zwei Schritten](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) für Microsoft-Konten
* [Probleme bei der Überprüfung in zwei Schritten?](multi-factor-authentication-end-user-troubleshoot.md)




<!--HONumber=Nov16_HO3-->


