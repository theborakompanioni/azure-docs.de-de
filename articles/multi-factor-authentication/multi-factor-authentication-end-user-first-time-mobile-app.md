<properties 
	pageTitle="Verwenden einer mobilen App als Kontaktmethode mit Azure MFA" 
	description="Auf dieser Seite erfahren Benutzer, wie sie die mobile App als primäre Kontaktmethode für Azure MFA verwenden." 
	services="multi-factor-authentication" 
	documentationCenter="" 
	authors="billmath" 
	manager="stevenp" 
	editor="curtland"/>

<tags 
	ms.service="multi-factor-authentication" 
	ms.workload="identity" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="08/30/2016" 
	ms.author="billmath"/>

# Verwenden einer mobilen App als Kontaktmethode mit Azure Multi-Factor Authentication

Wenn Sie die Microsoft Authenticator-App als primäre Kontaktmethode verwenden möchten, finden Sie weitere Informationen in diesem Artikel. Hier erfahren Sie, wie Sie Multi-Factor Authentication einrichten, um Ihre mobile App als primäre Kontaktmethode zu verwenden.

Die Microsoft Authenticator-App ist für [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072) und [iOS](http://go.microsoft.com/fwlink/?Linkid=825073) verfügbar.

## Verwenden von Microsoft Authenticator als Kontaktmethode


- Wählen Sie auf der Seite „Zusätzliche Sicherheitsüberprüfung“ in der Dropdownliste „Mobile App“ aus.


![Einrichtung](./media/multi-factor-authentication-end-user-first-time-mobile-app/mobileapp.png)

- Wählen Sie "Benachrichtigung" oder "Einmalkennwort" aus, und klicken Sie auf "Einrichten".
- Starten Sie die Microsoft Authenticator-App auf dem Smartphone, auf dem die App installiert ist, und tippen Sie auf „+“, um ein neues Konto hinzuzufügen. Geben Sie anschließend an, dass Sie ein Geschäfts-, Schul- oder Unikonto hinzufügen möchten, über das der QR-Codescanner gestartet wird. Wenn die Kamera nicht ordnungsgemäß funktioniert, können Sie die Informationen zu Ihrem Unternehmen manuell eingeben. [Manuelles Hinzufügen eines Kontos](#adding-an-account-manually).

![Einrichtung](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan4.png)

![Einrichtung](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan.png)

- Scannen Sie das Bild mit dem QR-Code, das auf dem Bildschirm „Mobile App konfigurieren“ angezeigt wird. Klicken Sie auf „Fertig“, um den Bildschirm mit dem QR-Code zu schließen.

![Einrichtung](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan2.png)

- Wenn der QR-Code nicht gescannt werden kann, können Sie die Informationen manuell eingeben.

![Einrichtung](./media/multi-factor-authentication-end-user-first-time-mobile-app/barcode.png)

- Die Aktivierung auf dem Mobiltelefon wird gestartet. Sobald der Vorgang abgeschlossen ist, klicken Sie auf "Nehmen Sie Kontakt mit mir auf". Dadurch wird eine Benachrichtigung oder ein Überprüfungscode an Ihr Mobiltelefon gesendet. Klicken Sie auf "Überprüfen".

![Einrichtung](./media/multi-factor-authentication-end-user-first-time-mobile-app/verify.png)

- In einigen Unternehmen wird bei der Überprüfung eine PIN angefordert.

![Einrichtung](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan3.png)


- Sobald dies abgeschlossen ist, können Sie auf „Schließen“ klicken. Die Überprüfung sollte jetzt erfolgreich abgeschlossen sein.
- Jetzt sollten Sie Ihre Mobiltelefonnummer eingeben, falls Sie einmal keinen Zugang zu Ihrer mobilen App mehr haben.
- Wählen Sie Ihr Land aus der Dropdownliste aus, und geben Sie im Feld neben dem Land Ihre Mobiltelefonnummer ein. Klicken Sie auf Weiter.
- Ihre Kontaktmethode ist nun eingerichtet. Als Nächstes richten Sie App-Kennwörter für Nicht-Browser-Apps wie Outlook 2010 oder früher ein. Wenn Sie solche Apps nicht verwenden, klicken Sie auf **Fertig**. Fahren Sie andernfalls mit dem nächsten Schritt fort.

![Einrichtung](./media/multi-factor-authentication-end-user-first-time-mobile-app/step4.png)

- Wenn Sie solche Apps verwenden, kopieren Sie das angegebene App-Kennwort und fügen es in die Nicht-Browser-Anwendung ein. Die Schritte für bestimmte Anwendungen wie Outlook und Lync finden Sie unter "Ändern des Kennworts in Ihrer E-Mail in das App-Kennwort" und "Ändern des Kennworts in Ihrer Anwendung in das App-Kennwort".
- Klicken Sie auf Done.


## Manuelles Hinzufügen eines Kontos
Wenn Sie ein Konto manuell hinzufügen möchten, wählen Sie die Schaltfläche zum manuellen Eingeben des Kontos aus.

![Einrichtung](./media/multi-factor-authentication-end-user-first-time-mobile-app/addaccount.png)


![Einrichtung](./media/multi-factor-authentication-end-user-first-time-mobile-app/addaccount2.png)

Wenn Sie ein Konto besitzen, das bereits über Azure MFA verfügt, geben Sie den Code und die URL ein, die auf der Seite mit dem Barcode bereitgestellt werden. Diese werden in der mobilen App in den Feldern für Code und URL eingegeben. Dadurch wird die Aktivierung gestartet.

![Einrichtung](./media/multi-factor-authentication-end-user-first-time-mobile-app/barcode2.png)

Sobald der Vorgang abgeschlossen ist, klicken Sie auf die Schaltfläche zur Kontaktaufnahme. Dadurch wird eine Benachrichtigung oder ein Überprüfungscode an Ihr Mobiltelefon gesendet. Klicken Sie auf "Überprüfen".



 

<!---HONumber=AcomDC_0831_2016-->