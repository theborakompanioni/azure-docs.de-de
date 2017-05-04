---
title: "Microsoft Authenticator-App für Mobiltelefone | Microsoft Docs"
description: "Erfahren Sie, wie Sie Azure Authenticator auf die neueste Version aktualisieren können."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: curtland
ms.assetid: 3065a1ee-f253-41f0-a68d-2bd84af5ffba
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2017
ms.author: kgremban
ms.custom: H1Hack27Feb2017, end-user
translationtype: Human Translation
ms.sourcegitcommit: 1cc1ee946d8eb2214fd05701b495bbce6d471a49
ms.openlocfilehash: 6bcf667849a8cb52126f832eed0b99a893182150
ms.lasthandoff: 04/26/2017

---
# <a name="get-started-with-the-microsoft-authenticator-app"></a>Erste Schritte mit der Microsoft Authenticator-App
Die Microsoft Authenticator-App bietet eine zusätzliche Sicherheitsstufe in Ihrem Azure-Konto (z. B. bsimon@contoso.onmicrosoft.com), Ihrem lokalen Geschäftskonto (z. B. bsimon@contoso.com) oder Ihrem Microsoft-Konto (z. B. bsimon@outlook.com).

Die App funktioniert auf zwei Arten:

* **Benachrichtigung**. Die App kann dazu beitragen, unberechtigten Zugriff auf Konten zu verhindern und betrügerische Transaktionen zu beenden, indem eine Benachrichtigung an Ihr Smartphone oder Tablet gesendet wird. Zeigen Sie einfach die Benachrichtigung an, und wählen Sie **Überprüfen**, wenn Sie den Zugriff zulassen möchten. Wählen Sie andernfalls **Verweigern**. Informationen zum Verweigern von Benachrichtigungen finden Sie im Abschnitt über die Verwendung der Funktion zum Ablehnen und Melden von Betrug für Multi-Factor Authentication.
* **Kennwort mit Überprüfungscode**. Die App kann als Softwaretoken zum Generieren eines OATH-Überprüfungscodes verwendet werden. Nachdem Sie Benutzernamen und Kennwort eingegeben haben, geben Sie auf dem Anmeldebildschirm den von der App bereitgestellten Code ein. Der Überprüfungscode kann als zweite Authentifizierungsmethode eingegeben werden.

Die Microsoft Authenticator-App ersetzt die Azure Authenticator-App.  Die Azure Authenticator-App kann weiterhin verwendet werden. Sollten Sie sich jedoch entscheiden, zur neuen Microsoft Authenticator-App zu wechseln, finden Sie in diesem Artikel hilfreiche Informationen.  

## <a name="install-the-app"></a>Installieren der App
Die Microsoft Authenticator-App ist für [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072) und [IOS](http://go.microsoft.com/fwlink/?Linkid=825073) verfügbar.

## <a name="add-accounts-to-the-app"></a>Hinzufügen von Konten zur App
Gehen Sie für jedes Konto, das Sie zur Microsoft Authenticator-App hinzufügen möchten, wie folgt vor.

### <a name="add-a-personal-microsoft-account-to-the-app"></a>Hinzufügen eines persönliches Microsoft-Kontos zur App

Für ein persönliches Microsoft-Konto (das Sie zur Anmeldung bei Outlook.com, Xbox, Skype usw. verwenden) müssen Sie sich lediglich in der Microsoft Authenticator-App bei Ihrem Konto anmelden.

### <a name="add-a-work-or-school-account-to-the-app-using-the-qr-code-scanner"></a>Hinzufügen eines Geschäfts-, Schul- oder Unikontos zur App über den QR-Code-Scanner
1. Rufen Sie die Seite mit Ihren Einstellungen für die Sicherheitsüberprüfung auf.  Informationen dazu, wie Sie auf diese Seite gelangen, finden Sie unter [Ändern der Sicherheitseinstellungen](multi-factor-authentication-end-user-manage-settings.md#where-to-find-the-settings-page).
2. Aktivieren Sie das Kontrollkästchen neben **Authenticator-App**, und wählen Sie dann **Konfigurieren**.

    ![Schaltfläche „Konfigurieren“ auf der Seite mit Ihren Einstellungen für die Sicherheitsüberprüfung](./media/authenticator-app-how-to/azureauthe.png)

    Daraufhin wird ein Bildschirm mit einem QR-Code angezeigt.

    ![Bildschirm, der den QR-Code enthält](./media/authenticator-app-how-to/barcode2.png)
3. Öffnen Sie die Microsoft Authenticator-App. Wählen Sie auf dem Bildschirm **Konten** die Option **+**, und geben Sie dann an, dass Sie ein Arbeits- oder Schulkonto hinzufügen möchten.
4. Verwenden Sie die Kamera, um den QR-Code zu scannen, und wählen Sie **Fertig** , um den Bildschirm mit dem QR-Code zu schließen.

    Wenn die Kamera nicht ordnungsgemäß funktioniert, können Sie den [QR-Code und die URL manuell eingeben](#add-an-account-to-the-app-manually).

5. Wenn die App Ihren Kontonamen mit einem sechsstelligen Code darunter anzeigt, ist der Vorgang abgeschlossen. 

    ![Bildschirm „Konten“](./media/authenticator-app-how-to/accounts.png)

### <a name="add-an-account-to-the-app-manually"></a>Manuelles Hinzufügen eines Kontos zur App
1. Rufen Sie die Seite mit Ihren Einstellungen für die Sicherheitsüberprüfung auf.  Informationen dazu, wie Sie auf diese Seite gelangen, finden Sie unter [Ändern der Sicherheitseinstellungen](multi-factor-authentication-end-user-manage-settings.md).
2. Wählen Sie **Konfigurieren**aus.

    ![Schaltfläche „Konfigurieren“ auf der Seite mit Ihren Einstellungen für die Sicherheitsüberprüfung](./media/authenticator-app-how-to/azureauthe.png)

    Daraufhin wird ein Bildschirm mit einem QR-Code angezeigt.  Notieren Sie den Code und die URL.

    ![Bildschirm, der den QR-Code und die URL enthält](./media/authenticator-app-how-to/barcode2.png)
3. Öffnen Sie die Microsoft Authenticator-App. Wählen Sie auf dem Bildschirm **Konten** die Option **+**, und geben Sie dann an, dass Sie ein Arbeits- oder Schulkonto hinzufügen möchten.

4. Wählen Sie im Scanner **Code manuell eingeben**.

    ![Bildschirm zum Scannen eines QR-Codes](./media/multi-factor-authentication-end-user-first-time/scan2.png)
5. Geben Sie den Code und die URL in die entsprechenden Felder in der App ein, und wählen Sie dann **Fertig stellen**.

    ![Bildschirm für die Eingabe von Code und URL](./media/authenticator-app-how-to/manual.png)

6. Wenn die App Ihren Kontonamen mit einem sechsstelligen Code darunter anzeigt, ist der Vorgang abgeschlossen.

    ![Bildschirm „Konten“](./media/authenticator-app-how-to/accounts.png)

### <a name="add-an-account-to-the-app-using-touch-id"></a>Hinzufügen eines Kontos zur App über Touch ID
Die Microsoft Authenticator-App unter iOS unterstützt Touch ID.  Azure Multi-Factor Authentication ermöglicht Organisationen, für Geräte eine PIN anzufordern. Mit Touch ID müssen iOS-Benutzer keine PIN eingeben. Stattdessen scannen sie ihren Fingerabdruck und wählen **Genehmigen**.

Das Einrichten von Touch ID mit Microsoft Authenticator ist einfach. Sie führen eine normale Überprüfung mit einer PIN durch. Wenn Ihr Gerät Touch ID unterstützt, wird Microsoft Authenticator diese Option automatisch für das Konto einrichten.

![Überprüfung der Touch ID-Einrichtung](./media/authenticator-app-how-to/touchid1.png)

Danach müssen Sie zum Überprüfen Ihrer Anmeldung nur noch die erhaltene Pushbenachrichtigung wählen und Ihren Fingerabdruck scannen lassen, anstatt die PIN einzugeben.

![Pushbenachrichtigung](./media/authenticator-app-how-to/touchid2.png)


