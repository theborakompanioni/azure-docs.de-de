---
title: "Einrichten der Überprüfung in zwei Schritten für mein Geschäfts-, Schul- oder Unikonto | Microsoft Docs"
description: "Wenn Ihr Unternehmen Azure Multi-Factor Authentication konfiguriert, werden Sie aufgefordert, sich für die Überprüfung in zwei Schritten anzumelden. Hier erfahren Sie, wie Sie das Verfahren einrichten. "
services: multi-factor-authentication
keywords: Verwenden von Azure Active Directory, Active Directory in der Cloud, Tutorial zu Active Directory
documentationcenter: 
author: kgremban
manager: femila
editor: pblachar
ms.assetid: 46f83a6a-dbdd-4375-8dc4-e7ea77c16357
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2017
ms.author: kgremban
ms.custom: end-user
translationtype: Human Translation
ms.sourcegitcommit: 4ff028f88e984f28bc0f4a228aabed1fabc90560
ms.openlocfilehash: 6dafec65b2e8091954cae39994d0013d9fd06558
ms.lasthandoff: 12/22/2016

---
# <a name="set-up-my-account-for-two-step-verification"></a>Einrichten meines Kontos für die Überprüfung in zwei Schritten
Die Überprüfung in zwei Schritten ist eine zusätzliche Sicherheitsmaßnahme, die zum Schutz Ihres Kontos beiträgt, indem sie den Zugriff durch Unbefugte erschwert. Wenn Sie diesen Artikel lesen, haben Sie wahrscheinlich vom Administrator Ihres Geschäfts-, Schul- oder Unikontos eine E-Mail zum Thema Multi-Factor Authentication erhalten. Oder Sie haben eventuell versucht, sich anzumelden, und eine Nachricht mit der Aufforderung empfangen, eine zusätzliche Sicherheitsüberprüfung einzurichten. Wenn dies der Fall ist, **können Sie sich erst anmelden, nachdem Sie die automatische Registrierung abgeschlossen haben**.

Dieser Artikel unterstützt Sie beim Einrichten Ihres **Geschäfts-, Schul- oder Unikontos**. Wenn Sie die Überprüfung in zwei Schritten für Ihr eigenes persönliches Microsoft-Konto aktivieren möchten, finden Sie entsprechende Informationen unter [Überprüfung in zwei Schritten](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).

## <a name="determine-how-you-will-use-multi-factor-authentication"></a>Überprüfungsmethoden für Multi-Factor Authentication
Bei der Überprüfung in zwei Schritten werden Sie zum Ausführen von zwei Identifizierungsschritten aufgefordert, wenn Sie sich anmelden. Zuerst fordern wir Sie wie üblich zur Angabe Ihres Benutzernamens und Kennworts auf. Dann übermitteln wir einen Anruf oder eine Nachricht an ein Telefon, von dem wir wissen, dass es Ihnen gehört, und Sie bestätigen, dass es sich um einen rechtmäßigen Anmeldeversuch gehandelt hat.  

Um mit dem Setup zu beginnen, versuchen Sie sich bei Ihrem Konto auf die gleiche Weise anzumelden, wie Sie sich bisher angemeldet haben. Wenn der Administrator Ihr Konto für die Überprüfung in zwei Schritten konfiguriert hat, werden Sie aufgefordert, mit der automatischen Registrierung zu beginnen. Klicken Sie zum Starten dieses Vorgangs auf **Jetzt einrichten**.

![Einrichtung](./media/multi-factor-authentication-end-user-first-time/first.png)

Die erste Frage bei der Registrierung betrifft die Methode, mit der wir Kontakt zu Ihnen aufnehmen sollen. Überprüfen Sie die Optionen in der Tabelle, und wechseln Sie mithilfe der Links zu den Einrichtungsschritten für jede Methode.

| Kontaktmethode | Beschreibung |
| --- | --- |
| [Mobile App](#use-a-mobile-app-as-the-contact-method) |- **Benachrichtigungen zur Überprüfung empfangen** Mit dieser Option wird eine Benachrichtigung an die Authenticator-App auf Ihrem Smartphone oder Tablet gesendet. Überprüfen Sie die Benachrichtigung, und wählen Sie in der App **Authentifizieren** aus, wenn Sie den Zugriff zulassen möchten. Ihr Unternehmen oder Ihre Bildungseinrichtung erfordert möglicherweise die Eingabe einer PIN, bevor Sie sich authentifizieren.<br>- **Überprüfungscode verwenden** In diesem Modus generiert die Authenticator-App einen Überprüfungscode, der alle 30 Sekunden aktualisiert wird. Geben Sie im Anmeldungsfenster den aktuellen Überprüfungscode ein.<br>Die Microsoft Authenticator-App ist für [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072) und [IOS](http://go.microsoft.com/fwlink/?Linkid=825073) verfügbar. |
| [Anruf beim Mobiltelefon oder Senden einer SMS](#use-your-mobile-phone-as-the-contact-method) |- **Telefonanruf**: Das Telefon mit der von Ihnen angegebenen Telefonnummer empfängt einen automatisierten Anruf. Nehmen Sie den Anruf an, und drücken Sie die #-TASTE auf der Telefontastatur, um sich zu authentifizieren.<br>- **SMS**: Es wird eine SMS gesendet, die einen Überprüfungscode enthält. Je nachdem, wozu Sie in der SMS aufgefordert werden, antworten Sie auf die SMS, oder geben Sie im Anmeldungsfenster den angegebenen Überprüfungscode ein. |
| [Anruf beim Bürotelefon](#use-your-office-phone-as-the-contact-method) |Das Telefon mit der von Ihnen angegebenen Telefonnummer empfängt einen automatisierten Anruf. Nehmen Sie den Anruf an, und drücken Sie die #-TASTE auf der Telefontastatur, um sich zu authentifizieren. |

## <a name="use-a-mobile-app-as-the-contact-method"></a>Verwenden einer mobilen App als Kontaktmethode
Für diese Methode müssen Sie auf Ihrem Smartphone oder Tablet eine Authenticator-App installieren. Die Schritte in diesem Artikel basieren auf der Microsoft Authenticator-App, die für [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072) und [IOS](http://go.microsoft.com/fwlink/?Linkid=825073) verfügbar ist.

1. Wählen Sie in der Dropdownliste **Mobile App** aus.
2. Wählen Sie **Benachrichtigungen zur Überprüfung empfangen** oder **Überprüfungscode verwenden** aus, und wählen Sie dann **Einrichten** aus.

    ![Zusätzliche Sicherheitsüberprüfung, Bildschirm](./media/multi-factor-authentication-end-user-first-time/mobileapp.png)
3. Öffnen Sie auf dem Smartphone oder Tablet die App, und tippen Sie auf **+**, um ein Konto hinzuzufügen. (Wählen Sie auf Android-Geräten die drei Punkte und dann **Konto hinzufügen** aus.)
4. Geben Sie dann an, dass Sie ein Geschäfts-, Schul- oder Unikonto hinzufügen möchten. Der QR-Codescanner auf Ihrem Smartphone wird geöffnet. Wenn die Kamera nicht ordnungsgemäß funktioniert, können Sie die Informationen zu Ihrem Unternehmen manuell eingeben. Weitere Informationen finden Sie unter [Manuelles Hinzufügen eines Kontos](#add-an-account-manually).  
5. Scannen Sie das Bild mit dem QR-Code, das auf dem Bildschirm zum Konfigurieren der mobilen App angezeigt wird.  Klicken Sie auf **Fertig** , um den Bildschirm mit dem QR-Code zu schließen.  

    ![QR-Code, Bildschirm](./media/multi-factor-authentication-end-user-first-time/scan2.png)
6. Wenn die Aktivierung auf dem Telefon abgeschlossen ist, wählen Sie **Nehmen Sie Kontakt mit mir auf**.  Dadurch wird eine Benachrichtigung oder ein Überprüfungscode an Ihr Mobiltelefon gesendet. Wählen Sie **Überprüfen**aus.  
7. Wenn Ihr Unternehmen eine PIN anfordert, um die Überprüfung der Anmeldung zu genehmigen, gegen Sie sie hier ein.

    ![Feld für die Eingabe einer PIN](./media/multi-factor-authentication-end-user-first-time/scan3.png)
8. Tippen Sie nach der PIN-Eingabe auf **Schließen**. Die Überprüfung sollte jetzt erfolgreich abgeschlossen sein.
9. Sie sollten Ihre Mobiltelefonnummer eingeben, falls Sie einmal keinen Zugang zu Ihrer mobilen App mehr haben. Wählen Sie in der Dropdownliste Ihr Land aus, und geben Sie im Feld neben dem Land Ihre Mobiltelefonnummer ein. Wählen Sie **Weiter**.
10. Jetzt werden Sie aufgefordert, App-Kennwörter für Nicht-Browser-Apps, z. B. Outlook 2010 oder früher, oder die native E-Mail-App auf Apple-Geräten einzurichten. Der Grund dafür ist, dass einige Apps die Überprüfung in zwei Schritten nicht unterstützen. Wenn Sie solche Apps nicht verwenden, klicken Sie auf **Fertig**, und überspringen Sie die restlichen Schritte.
11. Wenn Sie solche Apps verwenden, kopieren Sie das angegebene App-Kennwort, und fügen Sie es anstelle des regulären Kennworts in die App ein. Sie können das gleiche App-Kennwort für mehrere Apps verwenden. Weitere Informationen finden Sie unter [Hilfe bei App-Kennwörtern].
12. Klicken Sie auf **Done**.

### <a name="add-an-account-manually"></a>Manuelles Hinzufügen eines Kontos
Gehen Sie folgendermaßen vor, wenn Sie der mobilen Anwendung manuell ein Konto hinzufügen möchten, statt den QR-Reader zu verwenden.

1. Wählen Sie die Schaltfläche **Konto manuell eingeben** aus.  
2. Geben Sie den Code und die URL ein, der bzw. die auf der Seite mit dem Barcode angegeben ist. Diese Informationen werden in der mobilen App in die Felder **Code** und **URL** eingegeben.

    ![Einrichtung](./media/multi-factor-authentication-end-user-first-time/barcode2.png)
3. Wenn die Aktivierung abgeschlossen ist, wählen Sie **Nehmen Sie Kontakt mit mir auf**aus. Dadurch wird eine Benachrichtigung oder ein Überprüfungscode an Ihr Mobiltelefon gesendet. Wählen Sie **Überprüfen**aus.

## <a name="use-your-mobile-phone-as-the-contact-method"></a>Verwenden des Mobiltelefons als Kontaktmethode
1. Wählen Sie in der Dropdownliste **Telefon für Authentifizierung** aus.  

    ![Einrichtung](./media/multi-factor-authentication-end-user-first-time/phone.png)  
2. Wählen Sie Ihr Land aus der Dropdownliste aus, und geben Sie Ihre Mobiltelefonnummer ein.
3. Wählen Sie die gewünschte Methode für das Mobiltelefon aus: SMS oder Anruf.
4. Wählen Sie **Kontaktformular** aus, um Ihre Telefonnummer überprüfen zu lassen. Abhängig vom ausgewählten Modus senden wir Ihnen eine SMS oder rufen Sie an. Befolgen Sie die Anweisungen auf dem Bildschirm, und wählen Sie dann **Überprüfen** aus.
5. Jetzt werden Sie aufgefordert, App-Kennwörter für Nicht-Browser-Apps, z. B. Outlook 2010 oder früher, oder die native E-Mail-App auf Apple-Geräten einzurichten. Der Grund dafür ist, dass einige Apps die Überprüfung in zwei Schritten nicht unterstützen. Wenn Sie solche Apps nicht verwenden, klicken Sie auf **Fertig**, und überspringen Sie die restlichen Schritte.
6. Wenn Sie solche Apps verwenden, kopieren Sie das angegebene App-Kennwort, und fügen Sie es anstelle des regulären Kennworts in die App ein. Sie können das gleiche App-Kennwort für mehrere Apps verwenden. Weitere Informationen finden Sie unter [Hilfe bei App-Kennwörtern].
7. Klicken Sie auf **Done**.

## <a name="use-your-office-phone-as-the-contact-method"></a>Verwenden des Bürotelefons als Kontaktmethode
1. Wählen Sie in der Dropdownliste **Bürotelefon** aus.  

    ![Einrichtung](./media/multi-factor-authentication-end-user-first-time/office.png)  
2. Das Feld für die Telefonnummer wird automatisch mit den Kontaktinformationen Ihres Unternehmens ausgefüllt. Wenn die Nummer falsch oder nicht vorhanden ist, bitten Sie den Administrator, Änderungen vorzunehmen.
3. Wählen Sie **Kontaktformular** aus, um Ihre Telefonnummer überprüfen zu lassen, und wir senden einen Anruf an diese Nummer. Befolgen Sie die Anweisungen auf dem Bildschirm, und wählen Sie dann **Überprüfen** aus.
4. Jetzt werden Sie aufgefordert, App-Kennwörter für Nicht-Browser-Apps, z. B. Outlook 2010 oder früher, oder die native E-Mail-App auf Apple-Geräten einzurichten. Der Grund dafür ist, dass einige Apps die Überprüfung in zwei Schritten nicht unterstützen. Wenn Sie solche Apps nicht verwenden, klicken Sie auf **Fertig**, und überspringen Sie die restlichen Schritte.
5. Wenn Sie solche Apps verwenden, kopieren Sie das angegebene App-Kennwort, und fügen Sie es anstelle des regulären Kennworts in die App ein. Sie können das gleiche App-Kennwort für mehrere Apps verwenden. Weitere Informationen finden Sie unter [Welchen Zweck erfüllen App-Kennwörter bei Azure Multi-Factor Authentication?](multi-factor-authentication-end-user-app-passwords.md).
6. Klicken Sie auf **Done**.

## <a name="next-steps"></a>Nächste Schritte
* Ändern Sie Ihre bevorzugten Optionen und [verwalten Sie Ihre Einstellungen für die Überprüfung in zwei Schritten](multi-factor-authentication-end-user-manage-settings.md).
* Richten Sie [App-Kennwörter](multi-factor-authentication-end-user-app-passwords.md) für native Geräte-Apps ein, die die Überprüfung in zwei Schritten nicht unterstützen.
* Testen Sie die [Microsoft Authenticator-App](microsoft-authenticator-app-how-to.md) für die schnelle, sichere Authentifizierung, auch wenn kein Mobilfunkempfang möglich ist.

