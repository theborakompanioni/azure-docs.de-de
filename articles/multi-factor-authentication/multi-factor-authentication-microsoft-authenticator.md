<properties 
	pageTitle="Microsoft Authenticator-App für Mobiltelefone" 
	description="Erfahren Sie, wie Sie Azure Authenticator auf die neueste Version aktualisieren können." 
	services="multi-factor-authentication" 
	documentationCenter="" 
	authors="billmath" 
	manager="femila" 
	editor="curtland"/>

<tags 
	ms.service="multi-factor-authentication" 
	ms.workload="identity" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="08/22/2016" 
	ms.author="billmath"/>

# Microsoft Authenticator

Die Microsoft Authenticator-App bietet eine zusätzliche Sicherheitsstufe, die entweder in Ihrem Azure-Konto (z.B. bsimon@contoso.onmicrosoft.com), Ihrem lokalen Geschäftskonto (z.B. bsimon@contoso.com) oder Ihrem Microsoft-Konto (z.B. bsimon@outlook.com) genutzt werden kann.

## Herunterladen der Microsoft Authenticator-App

Die Microsoft Authenticator-App ist für [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072) und [iOS](http://go.microsoft.com/fwlink/?Linkid=825073) verfügbar.

## Funktionsweise der Microsoft Authenticator-App
Die App sendet eine Pushbenachrichtigung an die Microsoft Authenticator-App auf Ihrem Smartphone oder Tablet. Tippen Sie in der App auf „Überprüfen“, um sich zu authentifizieren.

Alternativ kann die App auch mit einem Überprüfungscode verwendet werden. Geben Sie einfach auf dem Anmeldebildschirm den von der App bereitgestellten Code ein, wenn Sie dazu aufgefordert werden.

Die zwei verschiedenen Modi sind:

**Benachrichtigung**: In diesem Modus verhindert die Microsoft Authenticator-App nicht autorisierte Zugriffe auf Konten sowie betrügerische Transaktionen. Zu diesem Zweck wird eine Pushbenachrichtigung an Ihr Telefon oder registriertes Gerät gesendet. Überprüfen Sie einfach die Benachrichtigung, und wählen Sie "Authentifizieren", wenn Sie den Zugriff zulassen möchten. Bei einem nicht autorisierten Zugriff können Sie den Zugriff verweigern und optional die betrügerische Benachrichtigung melden. Informationen zum Senden von Berichten zu betrügerischen Benachrichtigungen finden Sie im Abschnitt über die Verwendung der Funktion zum Ablehnen und Melden von Betrug für Multi-Factor Authentication.

**Einmalkennwort**: In diesem Modus kann die Microsoft Authenticator-App als Softwaretoken zum Generieren eines OATH-Überprüfungscodes verwendet werden. Dieser Überprüfungscode kann dann zusammen mit dem Benutzernamen und Kennwort als zweite Authentifizierungsmethode eingegeben werden.

## Hinzufügen eines Kontos zur Microsoft Authenticator-App mithilfe eines QR-Codescanners

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


## Manuelles Hinzufügen eines Kontos zur Microsoft Authenticator-App
Wenn Sie ein Konto manuell hinzufügen möchten, wählen Sie die Schaltfläche zum manuellen Eingeben des Kontos aus.

![Einrichtung](./media/multi-factor-authentication-end-user-first-time-mobile-app/addaccount.png)


![Einrichtung](./media/multi-factor-authentication-end-user-first-time-mobile-app/addaccount2.png)

Wenn Sie ein Konto besitzen, das bereits über Azure MFA verfügt, geben Sie den Code und die URL ein, die auf der Seite mit dem Barcode bereitgestellt werden. Diese werden in der mobilen App in den Feldern für Code und URL eingegeben. Dadurch wird die Aktivierung gestartet.

![Einrichtung](./media/multi-factor-authentication-end-user-first-time-mobile-app/barcode2.png)

Sobald der Vorgang abgeschlossen ist, klicken Sie auf die Schaltfläche zur Kontaktaufnahme. Dadurch wird eine Benachrichtigung oder ein Überprüfungscode an Ihr Mobiltelefon gesendet. Klicken Sie auf "Überprüfen".

## Wechseln zur neuen Microsoft Authenticator-App

Die Azure Authenticator-App wird durch die neue Microsoft Authenticator-App ersetzt. Die Azure Authenticator-App kann weiterhin verwendet werden. Sollten Sie sich jedoch entscheiden, zur neuen Microsoft Authenticator-App zu wechseln, finden Sie in diesem Artikel hilfreiche Informationen.


### Gewusst wie: Wechseln zur neuen Microsoft Authenticator-App 

**Schritt 1:** Installieren Sie die Microsoft Authenticator-App.


**Schritt 2:** Aktivieren Sie Ihre Konten in der neuen App.

Stellen Sie zunächst sicher, dass Sie für das hinzuzufügende Konto den QR-Code der den Code und die URL für die manuelle Eingabe zur Hand haben.

> [AZURE.NOTE] Sie wissen nicht, wie Sie den QR-Code abrufen können? Für Hilfe wenden Sie sich an den Helpdesk.
> 
> Das Aktivieren Ihres Kontos in der neuen App funktioniert nicht? Wenden Sie sich an den Helpdesk.
>


Wenn Sie den QR-Code zur Hand haben, starten Sie die App. Klicken Sie auf "+".

Geben Sie dann an, dass Sie ein Geschäfts-, Schul- oder Unikonto hinzufügen möchten. Hierdurch wird die Kamera zum Scannen des QR-Codes gestartet. Wenn Sie den QR-Code nicht scannen können, können Sie die Daten auch manuell eingeben.

![Einrichtung](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan2.png)

Überprüfen Sie die Seiten für Konten. Wenn das neue Konto angezeigt wird, war die Aktivierung erfolgreich.


Führen Sie diesen Schritt für alle Konten durch, die Sie zur neuen App migrieren möchten.



**Schritt 3:** Deinstallieren Sie die alte Multi-Factor Authentication-App von Ihrem Telefon.

Nachdem Sie der neuen App alle Konten hinzugefügt haben, deinstallieren Sie die alte Anwendung von Ihrem Telefon.



## Hinzufügen eines Kontos mit dem Barcodescanner



- Navigieren Sie zuerst auf die Seite mit Ihren Einstellungen für die Sicherheitsüberprüfung. Informationen dazu, wie Sie auf diese Seite gelangen, finden Sie unter [Ändern der Sicherheitseinstellungen](multi-factor-authentication-end-user-manage-settings.md).

- Klicken Sie auf die Schaltfläche „Konfigurieren“.
 
![Konto hinzufügen](./media/multi-factor-authentication-azure-authenticator/azureauthe.png)

- Ein Bildschirm mit einem Barcode wird angezeigt.
  
![Barcode scannen](./media/multi-factor-authentication-azure-authenticator/barcode2.png)

- Öffnen Sie nun die Microsoft Authenticator-App, um auf die Kontoseite zu gelangen. Es wird eine Liste mit den Konten angezeigt, die Sie eingerichtet haben. Wenn Sie ein neues Konto hinzufügen möchten, klicken Sie auf das Pluszeichen (+). Geben Sie dann an, dass Sie ein Geschäfts-, Schul- oder Unikonto hinzufügen möchten. Der Scanner wird geöffnet.

- Scannen Sie den Barcode.

![Einrichtung](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan2.png)

- Warten Sie, während das Konto aktiviert wird.

- Das ist schon alles. Das neue Konto sollte jetzt auf der Seite mit den Konten angezeigt werden.

![Konto hinzufügen](./media/multi-factor-authentication-azure-authenticator/addaccount2.png)

![Konto hinzufügen](./media/multi-factor-authentication-azure-authenticator/accounts.png)

## Manuelles Hinzufügen eines Azure-Kontos

Wenn Sie ein Konto manuell hinzufügen möchten, können Sie wie folgt vorgehen:

- Navigieren Sie zuerst auf die Seite mit Ihren Einstellungen für die Sicherheitsüberprüfung. Informationen dazu, wie Sie auf diese Seite gelangen, finden Sie unter [Ändern der Sicherheitseinstellungen](multi-factor-authentication-end-user-manage-settings.md).

- Klicken Sie auf die Schaltfläche „Konfigurieren“.
 
![Konto hinzufügen](./media/multi-factor-authentication-azure-authenticator/azureauthe.png)

- Ein Bildschirm mit einem Barcode wird angezeigt. Beachten Sie den Code und die URL unterhalb des Barcodes.
  
![Barcode scannen](./media/multi-factor-authentication-azure-authenticator/barcode2.png)

- Öffnen Sie nun die Microsoft Authenticator-App, um auf die Kontoseite zu gelangen. Es wird eine Liste mit den Konten angezeigt, die Sie eingerichtet haben. Wenn Sie ein neues Konto hinzufügen möchten, klicken Sie auf das Pluszeichen (+). Geben Sie dann an, dass Sie ein Geschäfts-, Schul- oder Unikonto hinzufügen möchten. Der Scanner wird geöffnet. Der Scanner wird geöffnet.

![Konto hinzufügen](./media/multi-factor-authentication-azure-authenticator/addaccount3.png)

- Klicken Sie unten auf „Code manuell eingeben“.

![Einrichtung](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan2.png)

- Geben Sie den Code und die URL ein, der bzw. die auf der Seite mit dem Barcode angegeben ist. Diese werden in der mobilen App in den Feldern für Code und URL eingegeben. Dadurch wird die Aktivierung gestartet.

![Konto hinzufügen](./media/multi-factor-authentication-azure-authenticator/manual.png)

![Einrichtung](./media/multi-factor-authentication-end-user-first-time-mobile-app/addaccount2.png)

- Warten Sie, während das Konto aktiviert wird.

- Das ist schon alles. Das neue Konto sollte jetzt auf der Seite mit den Konten angezeigt werden.

![Konto hinzufügen](./media/multi-factor-authentication-azure-authenticator/addaccount2.png)

![Konto hinzufügen](./media/multi-factor-authentication-azure-authenticator/accounts.png)


## Hinzufügen eines Kontos per Touch ID
Die mobile Microsoft Authenticator-App unter iOS unterstützt die Touch ID. Bei der Azure Multi-Factor Authentication können Unternehmen zusätzlich zum Nachweis des Besitzes eines registrierten Geräts die Eingabe einer PIN fordern. Mit diesem neuen Feature ist es für iOS-Benutzer mit Touch ID-fähigen Geräten nicht mehr nötig, die PIN einzugeben. Nach der Einrichtung scannen die Benutzer einfach ihren Fingerabdruck ein, anstatt die PIN einzugeben, und tippen auf „Genehmigen“.

Das Einrichten von Touch-ID mit Microsoft Authenticator ist sehr einfach. Sie führen einfach einen normalen Überprüfungsvorgang mit PIN durch, und wenn Ihr Gerät die Touch ID unterstützt, übernehmen wir die Einrichtung für Sie automatisch.

![Touch ID](./media/multi-factor-authentication-azure-authenticator/touchid1.png)

Danach müssen Sie zum Verifizieren Ihrer Anmeldung nur noch auf die erhaltene Pushbenachrichtigung tippen und Ihren Fingerabdruck scannen lassen, anstatt die PIN einzugeben.

![Touch ID](./media/multi-factor-authentication-azure-authenticator/touchid2.png)

## Löschen eines Kontos

Um einzelne Konten aus der Microsoft Authenticator-App zu entfernen, tippen Sie einfach auf das entsprechende Konto. Daraufhin wird eine Option zum Löschen angezeigt.

![Konto entfernen](./media/multi-factor-authentication-azure-authenticator/remove.png)

<!---HONumber=AcomDC_0831_2016-->