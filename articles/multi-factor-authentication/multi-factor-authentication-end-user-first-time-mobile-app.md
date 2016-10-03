<properties
	pageTitle="Verwenden einer mobilen App als Kontaktmethode mit Azure Multi-Factor Authentication | Microsoft Azure"
	description="In diesem Artikel erfahren Sie, wie eine mobile App als primäre Kontaktmethode für Azure Multi-Factor Authentication verwendet wird."
	services="multi-factor-authentication"
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
	ms.date="08/30/2016"
	ms.author="kgremban"/>

# Verwenden einer mobilen App als Kontaktmethode mit Azure Multi-Factor Authentication

Wenn Sie die Microsoft Authenticator-App als primäre Kontaktmethode verwenden möchten, finden Sie weitere Informationen in diesem Artikel. Hier erfahren Sie, wie Sie Azure Multi-Factor Authentication einrichten, um Ihre mobile App als primäre Kontaktmethode zu verwenden.

Die Microsoft Authenticator-App ist für [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072) und [iOS](http://go.microsoft.com/fwlink/?Linkid=825073) verfügbar.

## Verwenden von Microsoft Authenticator als Kontaktmethode


1. Wählen Sie auf der Seite **Zusätzliche Sicherheitsüberprüfung** in der Dropdownliste **Mobile App** aus.
2. Wählen Sie **Benachrichtigung** oder **Einmalkennwort** aus, und klicken Sie auf **Einrichten**.

	![Zusätzliche Sicherheitsüberprüfung, Bildschirm](./media/multi-factor-authentication-end-user-first-time-mobile-app/mobileapp.png)

3. Starten Sie die Microsoft Authenticator-App auf dem Smartphone, auf dem die App installiert ist, und tippen Sie auf **+**, um ein neues Konto hinzuzufügen.
4. Geben Sie dann an, dass Sie ein Geschäfts-, Schul- oder Unikonto hinzufügen möchten. Der QR-Codescanner wird geöffnet. Wenn die Kamera nicht ordnungsgemäß funktioniert, können Sie die Informationen zu Ihrem Unternehmen manuell eingeben. Weitere Informationen finden Sie unter [Manuelles Hinzufügen eines Kontos](#add-an-account-manually).

	![Bildschirm zum Auswählen eines Kontos](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan.png)

	![Bildschirm für die Auswahl, ob einen QR-Code gescannt oder ein Konto manuell eingegeben werden soll](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan4.png)

5. Scannen Sie das Bild mit dem QR-Code, das auf dem Bildschirm zum Konfigurieren der mobilen App angezeigt wird. Klicken Sie auf **Fertig**, um den Bildschirm mit dem QR-Code zu schließen.

	![QR-Code, Bildschirm](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan2.png)

	Wenn der QR-Code nicht gescannt werden kann, können Sie die Informationen manuell eingeben.

	![Bildschirm für die manuelle Eingabe von Informationen](./media/multi-factor-authentication-end-user-first-time-mobile-app/barcode.png)

6. Wenn die Aktivierung auf dem Telefon abgeschlossen ist, wählen Sie **Nehmen Sie Kontakt mit mir auf**. Dadurch wird eine Benachrichtigung oder ein Überprüfungscode an Ihr Mobiltelefon gesendet. Wählen Sie **Überprüfen** aus.

	![Bildschirm, auf dem Sie „Überprüfen“ auswählen, um sich anzumelden](./media/multi-factor-authentication-end-user-first-time-mobile-app/verify.png)

7. Wenn Ihr Unternehmen eine PIN anfordert, um die Überprüfung der Anmeldung zu genehmigen, gegen Sie sie hier ein.

	![Feld für die Eingabe einer PIN](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan3.png)

8. Tippen Sie nach der PIN-Eingabe auf **Schließen**. Die Überprüfung sollte jetzt erfolgreich abgeschlossen sein.
9. Sie sollten Ihre Mobiltelefonnummer eingeben, falls Sie einmal keinen Zugang zu Ihrer mobilen App mehr haben. Wählen Sie in der Dropdownliste Ihr Land aus, und geben Sie im Feld neben dem Land Ihre Mobiltelefonnummer ein. Wählen Sie **Weiter**.
10. Die Kontaktmethode ist nun eingerichtet. Nun müssen App-Kennwörter für Nicht-Browser-Apps wie Outlook 2010 oder früher eingerichtet werden. Wenn Sie diese Apps nicht nutzen, wählen Sie **Fertig** aus. Fahren Sie andernfalls mit dem nächsten Schritt fort.

	![Bildschirm zum Erstellen eines App-Kennworts](./media/multi-factor-authentication-end-user-first-time-mobile-app/step4.png)

11. Wenn Sie Nicht-Browser-Apps verwenden, kopieren Sie das angegebene App-Kennwort und fügen es in Ihre Apps ein. Die Schritte für bestimmte Anwendungen wie Outlook und Lync finden Sie unter „Ändern des Kennworts in Ihrer E-Mail in das App-Kennwort“ und „Ändern des Kennworts in Ihrer Anwendung in das App-Kennwort“.
12. Wählen Sie **Fertig** aus.


## Manuelles Hinzufügen eines Kontos
So fügen Sie ein Konto manuell hinzu:

1. Wählen Sie die Schaltfläche **Konto manuell eingeben** aus.

	![Bildschirm für die Eingabe von Code und URL](./media/multi-factor-authentication-end-user-first-time-mobile-app/addaccount.png)

	![Bildschirm für die Eingabe von Code und URL](./media/multi-factor-authentication-end-user-first-time-mobile-app/addaccount2.png)

2. Wenn Sie ein Konto haben, das bereits über Azure Multi-Factor Authentication verfügt, geben Sie den Code und die URL ein, die auf der Seite mit dem Barcode bereitgestellt werden. Diese Informationen werden in der mobilen App in die Felder **Code** und **URL** eingegeben.

	![Einrichtung](./media/multi-factor-authentication-end-user-first-time-mobile-app/barcode2.png)

3. Wenn die Aktivierung abgeschlossen ist, wählen Sie **Nehmen Sie Kontakt mit mir auf** aus. Dadurch wird eine Benachrichtigung oder ein Überprüfungscode an Ihr Mobiltelefon gesendet. Wählen Sie **Überprüfen** aus.

<!---HONumber=AcomDC_0921_2016-->