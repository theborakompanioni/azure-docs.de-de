---
title: "Hilfe und Support für die Microsoft Authenticator-App | Microsoft-Dokumentation"
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
ms.date: 01/19/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 45358e89561ef2265ca5d8ae522b823c4629b3fc
ms.openlocfilehash: f869e456ef40d04f7dd085d4ce7c3d654cdf2091


---
# <a name="microsoft-authenticator-app-faq"></a>Microsoft Authenticator-App – häufig gestellte Fragen
Die Microsoft Authenticator-App ersetzt die Azure Authenticator-App und ist die empfohlene App, wenn Sie Azure Multi-Factor Authentication verwenden. Diese App ist für Windows Phone, Android und iOS verfügbar.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen
### <a name="what-happened-to-the-azure-authenticator-multi-factor-auth-and-microsoft-account-apps"></a>Was ist mit den Azure Authenticator-, Multi-Factor Authenticator- und Microsoft-Konto-Apps passiert?
Die Microsoft Authenticator-App ersetzt die anderen Apps. Azure Authenticator wurde auf Microsoft Authenticator aktualisiert. Wenn Sie die Multi-Factor Auth- oder Microsoft-Konto-Apps verwenden, installieren Sie Microsoft Authenticator, und fügen Sie Ihre Konten erneut hinzu. Stellen Sie sicher, dass alle Ihre Konten zur neuen App hinzugefügt wurden, bevor Sie die alten Apps löschen.

Die Microsoft Authenticator-App ist für [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072) und [IOS](http://go.microsoft.com/fwlink/?Linkid=825073) verfügbar.

### <a name="im-already-using-the-microsoft-authenticator-application-for-verification-codes-how-do-i-switch-to-one-click-push-notifications"></a>Ich verwende für Sicherheitscodes bereits die Microsoft Authenticator-App. Wie wechsle ich zu One-Click-Pushbenachrichtigungen?
Die Bestätigung einer Anmeldung mittels Pushbenachrichtigungen steht nur für persönliche Microsoft-Konten oder für Geschäfts-, Schul- oder Unikonten von Microsoft zur Verfügung, nicht für Konten von Drittanbietern wie Google oder Facebook. Wenn Sie über ein Geschäfts-, Schul- oder Unikonto von Microsoft verfügen, kann Ihre Organisation diese Option deaktivieren.

Wenn Sie ein Microsoft-Konto als persönliches Konto verwenden, und zu Pushbenachrichtigungen wechseln möchten, müssen Sie Ihr Konto wieder hinzufügen. Registrieren Sie das Gerät erneut mit Ihrem Konto, und richten Sie Pushbenachrichtigungen ein.  

Wenn Sie Microsoft Authenticator für Ihr Geschäfts- oder Schulkonto verwenden, entscheidet Ihre Organisation, ob sie One-Click-Benachrichtigungen zulässt.

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

### <a name="why-does-the-microsoft-authenticator-app-allow-you-to-approve-a-request-without-unlocking-the-device"></a>Warum ermöglicht die Microsoft Authenticator-App die Genehmigung einer Anforderung ohne Entsperrung des Geräts?

Dies ist beabsichtigt. Bei der Überprüfung in zwei Schritten werden zwei Dinge überprüft: etwas, das Sie wissen (das Kennwort), und etwas, das Sie haben (Ihr Smartphone; eingerichtet mit der Microsoft Authenticator-App und registriert als MFA-Nachweis.)  Somit sind die Kriterien des zweiten Authentifizierungsschritts erfüllt, wenn Sie über das Smartphone verfügen und die Anforderung bestätigen. 

## <a name="next-steps"></a>Nächste Schritte

### <a name="contact-us"></a>Kontakt
Falls Ihre Frage hier nicht beantwortet wurde, lassen Sie es uns wissen. Posten Sie Ihre Frage im [Forum der Microsoft Authenticator-App](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp), um Hilfe aus der Community zu erhalten, hinterlassen Sie einen Kommentar auf dieser Seite, oder [wenden Sie sich an den Support](https://support.microsoft.com/contactus), und wir werden uns schnellstmöglich mit Ihrem Problem beschäftigen.


### <a name="related-topics"></a>Verwandte Themen
* [Überprüfung in zwei Schritten](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) für Microsoft-Konten
* [Probleme beim Einrichten der Überprüfung in zwei Schritten](multi-factor-authentication-end-user-troubleshoot.md) für Ihr Geschäfts-, Schul- oder Unikonto?




<!--HONumber=Jan17_HO3-->


