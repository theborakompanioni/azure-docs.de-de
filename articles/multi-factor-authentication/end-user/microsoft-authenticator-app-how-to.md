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
ms.date: 12/15/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 4ff028f88e984f28bc0f4a228aabed1fabc90560
ms.openlocfilehash: b20dd0fa6812c3125c3e2afe1dc9266613191baf


---
# <a name="microsoft-authenticator"></a>Microsoft Authenticator
Die Microsoft Authenticator-App bietet eine zusätzliche Sicherheitsstufe in Ihrem Azure-Konto (z.B. bsimon@contoso.onmicrosoft.com), Ihrem lokalen Geschäftskonto (z.B. bsimon@contoso.com), oder Ihrem Microsoft-Konto (z.B. bsimon@outlook.com).

Die App funktioniert auf zwei Arten:

* **Benachrichtigung**. Die App kann dazu beitragen, unberechtigten Zugriff auf Konten zu verhindern und betrügerische Transaktionen zu beenden, indem eine Benachrichtigung an Ihr Smartphone oder Tablet gesendet wird. Zeigen Sie einfach die Benachrichtigung an, und wählen Sie **Überprüfen**, wenn Sie den Zugriff zulassen möchten. Wählen Sie andernfalls **Verweigern**. Informationen zum Verweigern von Benachrichtigungen finden Sie im Abschnitt über die Verwendung der Funktion zum Ablehnen und Melden von Betrug für Multi-Factor Authentication.
* **Kennwort mit Überprüfungscode**. Die App kann als Softwaretoken zum Generieren eines OATH-Überprüfungscodes verwendet werden. Geben Sie auf dem Anmeldebildschirm den von der App bereitgestellten Code sowie Benutzername und Kennwort ein, wenn Sie dazu aufgefordert werden. Der Überprüfungscode kann als zweite Authentifizierungsmethode eingegeben werden.

Die Azure Authenticator-App wird durch die neue Microsoft Authenticator-App ersetzt.  Die Azure Authenticator-App kann weiterhin verwendet werden. Sollten Sie sich jedoch entscheiden, zur neuen Microsoft Authenticator-App zu wechseln, finden Sie in diesem Artikel hilfreiche Informationen.  

## <a name="install-the-app"></a>Installieren der App
Die Microsoft Authenticator-App ist für [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072) und [IOS](http://go.microsoft.com/fwlink/?Linkid=825073) verfügbar.

## <a name="add-accounts-to-the-app"></a>Hinzufügen von Konten zur App
Gehen Sie für jedes Konto, das Sie zur Microsoft Authenticator-App hinzufügen möchten, wie folgt vor.

### <a name="add-an-account-to-the-app-by-using-the-qr-code-scanner"></a>Hinzufügen eines Kontos zur App mit dem QR-Code-Scanner
1. Rufen Sie die Seite mit Ihren Einstellungen für die Sicherheitsüberprüfung auf.  Informationen dazu, wie Sie auf diese Seite gelangen, finden Sie unter [Ändern der Sicherheitseinstellungen](multi-factor-authentication-end-user-manage-settings.md).
2. Wählen Sie **Konfigurieren**aus.

    ![Schaltfläche „Konfigurieren“ auf der Seite mit Ihren Einstellungen für die Sicherheitsüberprüfung](./media/authenticator-app-how-to/azureauthe.png)

    Daraufhin wird ein Bildschirm mit einem QR-Code angezeigt.

    ![Bildschirm, der den QR-Code enthält](./media/authenticator-app-how-to/barcode2.png)
3. Öffnen Sie die Microsoft Authenticator-App. Wählen Sie auf dem Bildschirm **Konten** die Option **+**, und geben Sie dann an, dass Sie ein Arbeits- oder Schulkonto hinzufügen möchten.

    ![Bildschirm „Konten“ mit Pluszeichen](./media/authenticator-app-how-to/addaccount3.png)

    ![Bildschirm zum Festlegen von Geschäfts-oder Schulkonto](./media/authenticator-app-how-to/scan.png)
4. Verwenden Sie die Kamera, um den QR-Code zu scannen, und wählen Sie **Fertig** , um den Bildschirm mit dem QR-Code zu schließen.

    ![Bildschirm zum Scannen eines QR-Codes](./media/multi-factor-authentication-end-user-first-time/scan2.png)

    Wenn die Kamera nicht ordnungsgemäß funktioniert, können Sie den QR-Code und die URL manuell eingeben. Weitere Informationen finden Sie unter [Manuelles Hinzufügen eines Kontos zur App](#add-an-account-to-the-app-manually).
5. Warten Sie, während das Konto aktiviert wird. Wenn die Aktivierung abgeschlossen ist, wählen Sie **Nehmen Sie Kontakt mit mir auf**.  Dadurch wird eine Benachrichtigung oder ein Überprüfungscode an Ihr Mobiltelefon gesendet.  Wählen Sie **Überprüfen**aus.

    ![Bildschirm, auf dem Sie „Überprüfen“ auswählen, um sich anzumelden](./media/authenticator-app-how-to/verify.png)
6. Wenn Ihr Unternehmen eine PIN anfordert, um die Überprüfung der Anmeldung zu genehmigen, gegen Sie sie hier ein.

    ![Feld für die Eingabe einer PIN](./media/multi-factor-authentication-end-user-first-time/scan3.png)
7. Tippen Sie nach der PIN-Eingabe auf **Schließen**. Die Überprüfung sollte jetzt erfolgreich abgeschlossen sein.
8. Sie sollten Ihre Mobiltelefonnummer eingeben, falls Sie einmal keinen Zugang zu Ihrer App mehr haben. Wählen Sie Ihr Land in der Dropdownliste aus, und geben Sie im Feld neben dem Land Ihre Mobiltelefonnummer ein. Wählen Sie **Weiter**.
9. Die Kontaktmethode ist nun eingerichtet. Nun müssen App-Kennwörter für Nicht-Browser-Apps wie Outlook 2010 oder früher eingerichtet werden. Wenn Sie diese Apps nicht nutzen, wählen Sie **Fertig**aus. Fahren Sie andernfalls mit dem nächsten Schritt fort.

10. Wenn Sie Nicht-Browser-Apps verwenden, kopieren Sie das angegebene App-Kennwort und fügen es in Ihre Apps ein. Die Schritte für bestimmte Anwendungen wie Outlook und Lync finden Sie unter „Ändern des Kennworts in Ihrer E-Mail in das App-Kennwort“ und „Ändern des Kennworts in Ihrer Anwendung in das App-Kennwort“.
11. Wählen Sie **Fertig**aus.

Das neue Konto sollte jetzt auf der Seite **Konten** angezeigt werden.

![Bildschirm „Konten“](./media/authenticator-app-how-to/accounts.png)

### <a name="add-an-account-to-the-app-manually"></a>Manuelles Hinzufügen eines Kontos zur App
1. Rufen Sie die Seite mit Ihren Einstellungen für die Sicherheitsüberprüfung auf.  Informationen dazu, wie Sie auf diese Seite gelangen, finden Sie unter [Ändern der Sicherheitseinstellungen](multi-factor-authentication-end-user-manage-settings.md).
2. Wählen Sie **Konfigurieren**aus.

    ![Schaltfläche „Konfigurieren“ auf der Seite mit Ihren Einstellungen für die Sicherheitsüberprüfung](./media/authenticator-app-how-to/azureauthe.png)

    Daraufhin wird ein Bildschirm mit einem QR-Code angezeigt.  Notieren Sie den Code und die URL.

    ![Bildschirm, der den QR-Code und die URL enthält](./media/authenticator-app-how-to/barcode2.png)
3. Öffnen Sie die Microsoft Authenticator-App. Wählen Sie auf dem Bildschirm **Konten** die Option **+**, und geben Sie dann an, dass Sie ein Arbeits- oder Schulkonto hinzufügen möchten.

    ![Bildschirm „Konten“ mit Pluszeichen](./media/authenticator-app-how-to/addaccount3.png)

    ![Bildschirm zum Festlegen von Geschäfts-oder Schulkonto](./media/authenticator-app-how-to/scan.png)
4. Wählen Sie im Scanner **Code manuell eingeben**.

    ![Bildschirm zum Scannen eines QR-Codes](./media/multi-factor-authentication-end-user-first-time/scan2.png)
5. Geben Sie den Code und die URL in die entsprechenden Felder in der App ein.

    ![Bildschirm für die Eingabe von Code und URL](./media/authenticator-app-how-to/manual.png)

    ![Bildschirm für die Eingabe von Code und URL](./media/authenticator-app-how-to/addaccount2.png)
6. Warten Sie, während das Konto aktiviert wird. Wenn die Aktivierung abgeschlossen ist, wählen Sie **Nehmen Sie Kontakt mit mir auf**. Dadurch wird eine Benachrichtigung oder ein Überprüfungscode an Ihr Mobiltelefon gesendet. Wählen Sie **Überprüfen**aus.

Das neue Konto sollte jetzt auf der Seite **Konten** angezeigt werden.

![Bildschirm „Konten“](./media/authenticator-app-how-to/accounts.png)

### <a name="add-an-account-to-the-app-by-using-touch-id"></a>Hinzufügen eines Kontos zur App mit Touch ID
Die Microsoft Authenticator-App unter iOS unterstützt Touch ID.  Azure Multi-Factor Authentication ermöglicht Organisationen, für Geräte eine PIN anzufordern. Mit Touch ID müssen iOS-Benutzer keine PIN eingeben. Stattdessen scannen sie ihren Fingerabdruck und wählen **Genehmigen**.

Das Einrichten von Touch ID mit Microsoft Authenticator ist einfach. Sie führen eine normale Überprüfung mit einer PIN durch. Wenn Ihr Gerät Touch ID unterstützt, wird Microsoft Authenticator diese Option automatisch für das Konto einrichten.

![Überprüfung der Touch ID-Einrichtung](./media/authenticator-app-how-to/touchid1.png)

Danach müssen Sie zum Überprüfen Ihrer Anmeldung nur noch die erhaltene Pushbenachrichtigung wählen und Ihren Fingerabdruck scannen lassen, anstatt die PIN einzugeben.

![Pushbenachrichtigung](./media/authenticator-app-how-to/touchid2.png)

## <a name="uninstall-the-old-azure-authentication-app"></a>Deinstallieren der alten Azure Authentication-App
Sobald Sie der neuen App alle Konten hinzugefügt haben, können Sie die alte Anwendung auf Ihrem Telefon deinstallieren.

## <a name="delete-an-account"></a>Löschen eines Kontos
Um ein Konto aus der Microsoft-Authenticator-App zu entfernen, wählen Sie das Konto aus, und wählen Sie dann **Löschen**.

![Schaltfläche „Löschen“](./media/authenticator-app-how-to/remove.png)



<!--HONumber=Dec16_HO4-->


