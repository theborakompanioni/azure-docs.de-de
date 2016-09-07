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

## Hinzufügen eines Kontos zur Microsoft Authenticator-App

## Wechseln zur neuen Microsoft Authenticator-App

Die Azure Authenticator-App wird durch die neue Version ersetzt. Die Azure Authenticator-App kann weiterhin verwendet werden. Sollten Sie sich jedoch entscheiden, zur neuen Microsoft Authenticator-App zu wechseln, finden Sie in diesem Artikel hilfreiche Informationen.


## Gewusst wie: Wechseln zur neuen Microsoft Authenticator-App 

**Schritt 1:** Installieren Sie die Microsoft Authenticator-App.

![Cloud](./media/multi-factor-authentication-azure-authenticator/home.png)

**Schritt 2:** Aktivieren Sie Ihre Konten in der neuen App.

Stellen Sie zunächst sicher, dass Sie für das hinzuzufügende Konto den QR-Code der den Code und die URL für die manuelle Eingabe zur Hand haben.

> [AZURE.NOTE] Sie wissen nicht, wie Sie den QR-Code abrufen können? Für Hilfe wenden Sie sich an den Helpdesk.
> 
> Das Aktivieren Ihres Kontos in der neuen App funktioniert nicht? Wenden Sie sich an den Helpdesk.
>


Wenn Sie den QR-Code zur Hand haben, starten Sie die App. Klicken Sie auf "+".


![Konto hinzufügen](./media/multi-factor-authentication-azure-authenticator/addaccount.png)

Hierdurch wird die Kamera zum Scannen des QR-Codes gestartet. Wenn Sie den QR-Code nicht scannen können, können Sie die Daten auch manuell eingeben.

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

- Öffnen Sie nun die Microsoft Authenticator-App, um auf die Kontoseite zu gelangen. Es wird eine Liste mit den Konten angezeigt, die Sie eingerichtet haben. Klicken Sie auf das Pluszeichen (+), wenn Sie ein neues Konto hinzufügen möchten. Der Scanner wird geöffnet.

![Konto hinzufügen](./media/multi-factor-authentication-azure-authenticator/addaccount3.png)

- Scannen Sie den Barcode.

![Konto hinzufügen](./media/multi-factor-authentication-azure-authenticator/scan.png)

- Warten Sie, während das Konto aktiviert wird.

- Das ist schon alles. Das neue Konto sollte jetzt auf der Seite mit den Konten angezeigt werden.

![Konto hinzufügen](./media/multi-factor-authentication-azure-authenticator/addaccount2.png)


## Manuelles Hinzufügen eines Azure-Kontos

Wenn Sie ein Konto manuell hinzufügen möchten, können Sie wie folgt vorgehen:

- Navigieren Sie zuerst auf die Seite mit Ihren Einstellungen für die Sicherheitsüberprüfung. Informationen dazu, wie Sie auf diese Seite gelangen, finden Sie unter [Ändern der Sicherheitseinstellungen](multi-factor-authentication-end-user-manage-settings.md).

- Klicken Sie auf die Schaltfläche „Konfigurieren“.
 
![Konto hinzufügen](./media/multi-factor-authentication-azure-authenticator/azureauthe.png)

- Ein Bildschirm mit einem Barcode wird angezeigt. Beachten Sie den Code und die URL unterhalb des Barcodes.
  
![Barcode scannen](./media/multi-factor-authentication-azure-authenticator/barcode2.png)

- Öffnen Sie nun die Microsoft Authenticator-App, um auf die Kontoseite zu gelangen. Es wird eine Liste mit den Konten angezeigt, die Sie eingerichtet haben. Klicken Sie auf das Pluszeichen (+), wenn Sie ein neues Konto hinzufügen möchten. Der Scanner wird geöffnet.

![Konto hinzufügen](./media/multi-factor-authentication-azure-authenticator/addaccount3.png)

- Klicken Sie unten auf die Schaltfläche „Manuell eingeben“.

![Konto hinzufügen](./media/multi-factor-authentication-azure-authenticator/scan.png)

- Geben Sie den Code und die URL ein, der bzw. die auf der Seite mit dem Barcode angegeben ist. Diese werden in der mobilen App in den Feldern für Code und URL eingegeben. Dadurch wird die Aktivierung gestartet.

![Konto hinzufügen](./media/multi-factor-authentication-azure-authenticator/manual.png)

- Warten Sie, während das Konto aktiviert wird.

- Das ist schon alles. Das neue Konto sollte jetzt auf der Seite mit den Konten angezeigt werden.

![Konto hinzufügen](./media/multi-factor-authentication-azure-authenticator/addaccount2.png)

## Manuelles Hinzufügen eines Nicht-Azure-Kontos

Wenn Sie andere Konten als ein Azure-Konto manuell hinzufügen möchten, z. B. Ihr Microsoft-Konto, können Sie wie folgt vorgehen:


- Ein Nicht-Azure-Konto lässt sich entweder durch das Scannen des QR-Codes oder durch Eingabe des geheimen Schlüssels hinzufügen.
- Wenn Sie den geheimen Schlüssel manuell eingeben möchten, müssen Sie diesen Schlüssel auf der Website anfordern, bei der das betreffende Konto besteht. In Outlook.com greifen Sie beispielsweise auf Ihre Kontoeinstellungen und dann auf die Sicherheitseinstellungen zu und wählen die Einrichtung einer Authentifizierungs-App aus. Wählen Sie „Ich kann den Barcode nicht scannen“ aus, um den geheimen Schlüssel zu erhalten.
- 

![Konto hinzufügen](./media/multi-factor-authentication-azure-authenticator/secretkey.png)

- Öffnen Sie die Microsoft Authenticator-App, um auf die Kontoseite zu gelangen. Es wird eine Liste mit den Konten angezeigt, die Sie eingerichtet haben. Klicken Sie auf das Pluszeichen (+), wenn Sie ein neues Konto hinzufügen möchten. Der Scanner wird geöffnet.

![Konto hinzufügen](./media/multi-factor-authentication-azure-authenticator/addaccount3.png)

- Sie können entweder den QR-Code scannen oder unten auf die Schaltfläche „Manuell eingeben“ klicken. Beim Scannen des QR-Codes entfällt der nachfolgende Schritt und die Aktivierung startet sofort.

![Konto hinzufügen](./media/multi-factor-authentication-azure-authenticator/scan.png)

- Bei der manuellen Eingabe des geheimen Schlüssels sind der Kontoname und der geheime Schlüssel zusammen auf der Seite mit dem Barcode anzugeben. Diese werden in der mobilen App in den Feldern für Code und URL eingegeben. Dadurch wird die Aktivierung gestartet.

![Konto hinzufügen](./media/multi-factor-authentication-azure-authenticator/manual.png)

- Warten Sie, während das Konto aktiviert wird. Das neu hinzugefügte Konto sollte jetzt angezeigt werden.

![Konto hinzufügen](./media/multi-factor-authentication-azure-authenticator/msaccount.png)

- Schließen Sie den Prozess ab, indem Sie den Code eingeben, der unter Ihrem Konto angezeigt wird (hier: 875 756). Geben Sie ihn in das Feld auf der Seite ein, über die Sie den geheimen Schlüssel erhalten haben, und klicken Sie auf „Weiter“.

![Konto hinzufügen](./media/multi-factor-authentication-azure-authenticator/verify.png)

## Hinzufügen eines Kontos per Touch ID
Die mobile Microsoft Authenticator-App unter iOS unterstützt die Touch ID. Bei der Azure Multi-Factor Authentication können Unternehmen zusätzlich zum Nachweis des Besitzes eines registrierten Geräts die Eingabe einer PIN fordern. Mit diesem neuen Feature ist es für iOS-Benutzer mit Touch ID-fähigen Geräten nicht mehr nötig, die PIN einzugeben. Nach der Einrichtung scannen die Benutzer einfach ihren Fingerabdruck ein, anstatt die PIN einzugeben, und tippen auf „Genehmigen“.

Das Einrichten von Touch-ID mit Microsoft Authenticator ist sehr einfach. Sie führen einfach einen normalen Überprüfungsvorgang mit PIN durch, und wenn Ihr Gerät die Touch ID unterstützt, übernehmen wir die Einrichtung für Sie automatisch.

![Touch ID](./media/multi-factor-authentication-azure-authenticator/touchid1.png)

Danach müssen Sie zum Verifizieren Ihrer Anmeldung nur noch auf die erhaltene Pushbenachrichtigung tippen und Ihren Fingerabdruck scannen lassen, anstatt die PIN einzugeben.

![Touch ID](./media/multi-factor-authentication-azure-authenticator/touchid2.png)

## Löschen eines Kontos

Um einzelne Konten aus der Microsoft Authenticator-App zu entfernen, tippen Sie einfach auf das entsprechende Konto. Daraufhin wird eine Option zum Löschen angezeigt.

![Konto entfernen](./media/multi-factor-authentication-azure-authenticator/remove.png)

<!---HONumber=AcomDC_0824_2016-->