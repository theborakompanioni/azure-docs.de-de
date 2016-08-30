<properties
	pageTitle="Problembehandlung: Zugriff nicht möglich | Microsoft Azure"
	description="Mit den Informationen in diesem Thema können Sie ermitteln, ob es Korrekturschritte gibt, um Zugriff auf die gewünschte Anwendung zu erhalten."
	services="active-directory"
	keywords="gerätebasierter bedingter Zugriff, Geräteregistrierung, Geräteregistrierung aktivieren, Geräteregistrierung und MDM"
	documentationCenter=""
	authors="markusvi"
	manager="femila"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="08/23/2016"
	ms.author="markvi"/>


# Problembehandlung: Zugriff nicht möglich

Sie haben beim Zugreifen auf eine Anwendung wie Office 365 SharePoint Online eine Seite mit der Meldung „Zugriff verweigert“ erhalten. Was können Sie tun?

Mithilfe dieses Leitfadens können Sie ermitteln, ob es Korrekturschritte gibt, die Sie ausführen können, um Zugriff auf die gewünschte Anwendung zu erhalten.



Auf welcher Geräteplattform wird Ihr Gerät ausgeführt? Die Antwort auf diese Frage bestimmt den passenden Abschnitt in diesem Thema:
 

-	Windows-Gerät

-	iOS-Gerät (iPhone oder iPad)

-	Android-Gerät

## Zugriff mit einem Windows-Gerät

Wenn auf Ihrem Gerät Windows 10, Windows 8.1, Windows 8.0, Windows 7, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 oder Windows Server 2008 R2 ausgeführt wird, können Sie die jeweilige Ursache auswählen, indem Sie die Seite identifizieren, die beim versuchten Zugriff auf die Anwendung angezeigt wurde.

### Das Gerät ist nicht registriert

Wenn das Gerät nicht bei Azure Active Directory registriert und die Anwendung durch eine gerätebasierte Richtlinie geschützt ist, wird ggf. eine Seite mit folgendem Inhalt angezeigt:

![Szenario](./media/active-directory-conditional-access-device-remediation/01.png "Szenario")

 

Wenn das Gerät einer Domäne der Active Directory-Instanz Ihrer Organisation angehört, können Sie Folgendes versuchen:

1.	Stellen Sie sicher, dass Sie an Windows mit Ihrem Geschäftskonto (Active Directory-Konto) angemeldet sind.

2.	Stellen Sie per VPN oder Direktzugriff eine Verbindung mit Ihrem Unternehmensnetzwerk her.

3.	Sperren Sie Ihre Windows-Sitzung nach der Verbindungsherstellung, indem Sie WINDOWS-TASTE+L drücken.

4.	Entsperren Sie die Windows-Sitzung, indem Sie die Anmeldeinformationen Ihres Geschäftskontos eingeben.

5.	Warten Sie eine Minute, und versuchen Sie erneut, auf die Anwendung zuzugreifen.

6.	Wenn Sie wieder die gleiche Seite erhalten, sollten Sie sich an Ihren Administrator wenden und nach dem Klicken auf den Link „Weitere Details“ die Details angeben.

Falls das Gerät keiner Domäne angehört und über Windows 10 verfügt, haben Sie zwei Optionen:

1. Ausführen von Azure AD Join
2. Fügen Sie Windows Ihr Geschäfts- oder Schulkonto hinzu.

Informationen zu den Unterschieden zwischen den beiden Konten finden Sie unter [Verwenden von Windows 10-Geräten an Ihrem Arbeitsplatz](active-directory-azureadjoin-windows10-devices.md).

Gehen Sie wie folgt vor, um Azure AD Join auszuführen (in Windows Phone nicht verfügbar):

**Windows 10 Anniversary Update**

1.	Starten Sie die App „Einstellungen“.

2.	Navigieren Sie zu „Konten“ und dann zu „Access work or school“ (Auf Geschäfts-/Schulkonto zugreifen).

3.	Klicken Sie auf „Verbinden“.

4.	Wählen Sie unten auf der Seite die Option „Join this device to Azure AD“ (Gerät mit Azure AD verknüpfen).

5.	Führen Sie die Authentifizierung für Ihre Organisation durch, geben Sie bei Bedarf die MFA-Daten an, und führen Sie alle Schritte bis zum Ende aus.

6.	Melden Sie sich ab, und melden Sie sich dann mit Ihrem Geschäftskonto an.

7.	Versuchen Sie erneut, auf die Anwendung zuzugreifen.




**Windows 10 November 2015 Update**


1.	Starten Sie die App „Einstellungen“.

2.	Navigieren Sie zu „System“ und dann zu „About“ (Info).
	
3.	Klicken Sie auf „Azure AD beitreten“.

4.	Führen Sie die Authentifizierung für Ihre Organisation durch, geben Sie bei Bedarf die MFA-Daten an, und führen Sie alle Schritte bis zum Ende aus.

5.	Melden Sie sich ab, und melden Sie sich dann mit Ihrem Geschäftskonto (Azure AD-Konto) an.

6.	Versuchen Sie erneut, auf die Anwendung zuzugreifen.


Führen Sie die folgenden Schritte aus, um das Geschäfts- oder Schulkonto hinzuzufügen:

**Windows 10 Anniversary Update**

1.	Starten Sie die App „Einstellungen“.

2.	Navigieren Sie zu „Konten“ und dann zu „Access work or school“ (Auf Geschäfts-/Schulkonto zugreifen).

3.	Klicken Sie auf „Verbinden“.

4.	Führen Sie die Authentifizierung für Ihre Organisation durch, geben Sie bei Bedarf die MFA-Daten an, und führen Sie alle Schritte bis zum Ende aus.

5.	Versuchen Sie erneut, auf die Anwendung zuzugreifen.


**Windows 10 November 2015 Update**
	
1.	Starten Sie die App „Einstellungen“.
2.	Navigieren Sie zu „Konten“ und dann zu „Your accounts“ (Ihre Konten).
3.	Klicken Sie auf „Geschäfts- oder Schulkonto hinzufügen“.
4.	Führen Sie die Authentifizierung für Ihre Organisation durch, geben Sie bei Bedarf die MFA-Daten an, und führen Sie alle Schritte bis zum Ende aus.
5.	Versuchen Sie erneut, auf die Anwendung zuzugreifen.

Wenn das Gerät keiner Domäne angehört und über Windows 8.1 verfügt, können Sie wie folgt eine Arbeitsbereichverknüpfung und die Registrierung bei Microsoft Intune durchführen:

1.	Starten Sie die PC-Einstellungen.

2.	Navigieren Sie zu „Netzwerk“ und dann zu „Arbeitsplatz“.

3.	Klicken Sie auf „Beitreten“.

4.	Führen Sie die Authentifizierung für Ihre Organisation durch, geben Sie bei Bedarf die MFA-Daten an, und führen Sie alle Schritte bis zum Ende aus.

5.	Klicken Sie auf „Aktivieren“.

6.	Warten Sie, bis der Vorgang abgeschlossen ist.

7.	Versuchen Sie erneut, auf die Anwendung zuzugreifen.


## Nicht unterstützte Browser

Wenn Sie auf die Anwendung mit den folgenden Browsern zugreifen, wird eine Seite wie in der folgenden Abbildung angezeigt:

1.	Chrome, Firefox oder andere Browser, die nicht Microsoft Edge oder Microsoft Internet Explorer unter Windows 10 oder Windows Server 2016 entsprechen.

2.	Firefox unter Windows 8.1, Windows 7, Windows Server 2012 R2, Windows Server 2012 oder Windows Server 2008 R2.
 

![Szenario](./media/active-directory-conditional-access-device-remediation/02.png "Szenario")


Die einzige Lösung ist die Verwendung eines Browsers, der für Ihre Geräteplattform unterstützt wird.

## Zugriff mit einem iOS-Gerät

In Kürze finden Sie hier eine Anleitung für iPhones bzw. iPads.

## Zugriff mit einem Android-Gerät

In Kürze finden Sie hier eine Anleitung für Android-Smartphones bzw. -Tablets.


## Nächste Schritte

[Bedingter Zugriff mit Azure Active Directory](active-directory-conditional-access.md)

<!---HONumber=AcomDC_0824_2016-->