<properties
	pageTitle="Hinzufügen eines benutzerdefinierten Domänennamens zu Azure Active Directory | Microsoft Azure"
	description="Erfahren Sie, wie Sie Azure Active Directory die Domänennamen Ihres Unternehmens hinzufügen und den Domänennamen überprüfen."
	services="active-directory"
	documentationCenter=""
	authors="jeffsta"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="04/20/2016"
	ms.author="curtand;jeffsta"/>

# Hinzufügen eines benutzerdefinierten Domänennamens zu Azure Active Directory

Sie verfügen über mindestens einen Domänennamen, den Ihre Organisation zu geschäftlichen Zwecken verwendet, und Ihre Benutzer melden sich unter Verwendung Ihres geschäftlichen Domänennamens bei Ihrem Unternehmensnetzwerk an. Mit Azure Active Directory (Azure AD) können Sie den Namen Ihrer Unternehmensdomäne nun auch zu Azure AD hinzufügen. Dadurch können Sie in dem Verzeichnis Benutzernamen zuweisen, die Ihren Benutzern vertraut sind (etwa „alice@contoso.com“). Der Prozess ist ganz einfach:

- Fügen Sie Ihren Domänennamen im klassischen Azure-Portal mithilfe des Assistenten **Domäne hinzufügen** hinzu.

- Rufen Sie im klassischen Azure AD-Portal oder mit dem Azure AD Connect-Tool den DNS-Eintrag ab.

- Fügen Sie auf der Website für die DNS-Registrierung den DNS-Eintrag für den Domänennamen der DNS-Zonendatei hinzu.

- Überprüfen Sie den Domänennamen im klassischen Azure AD-Portal oder mit dem Azure AD Connect-Tool.


Bis zur Überprüfung des benutzerdefinierten Domänennamens müssen sich die Benutzer mit Benutzernamen wie „alice@contoso.onmicrosoft.com“ anmelden und dabei den anfänglichen Domänennamen verwenden. Bei Bedarf können bis zu 900 benutzerdefinierte Domänennamen hinzugefügt werden (etwa „contoso.com“ und „contosobank.com“). Verwenden Sie zum Hinzufügen der einzelnen Domänennamen jeweils die Schritte aus diesem Artikel.

## Hinzufügen eines benutzerdefinierten Domänennamens zu Ihrem Verzeichnis

1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com/) mit dem Benutzerkonto eines globalen Administrators Ihres Azure AD-Verzeichnisses an.

2. Öffnen Sie in Active Directory Ihr Verzeichnis, und wählen Sie die Registerkarte **Domänen** aus.

3. Wählen Sie auf der Befehlsleiste **Hinzufügen** aus, und geben Sie den Namen Ihrer benutzerdefinierten Domäne ein (beispielsweise „contoso.com“). Achten Sie darauf, die Erweiterung „.com“, „.net“ oder eine andere Erweiterung der obersten Ebene zu verwenden.

4. Aktivieren Sie das Kontrollkästchen, wenn Sie diese Domäne für eine [Verbundanmeldung](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect) mit Ihrer lokalen Active Directory-Instanz konfigurieren möchten.

5. Wählen Sie **Hinzufügen**.

Nach dem Hinzufügen des Domänennamens muss Azure AD überprüfen, ob der Domänenname Ihrer Organisation gehört. Dazu muss allerdings zuerst der DNS-Zonendatei ein DNS-Eintrag für den Domänennamen hinzugefügt werden. Dieser Schritt wird auf der Website für die Registrierung von Domänennamen ausgeführt.

## Rufen Sie die DNS-Einträge für den Domänennamen ab

Die DNS-Einträge befinden sich auf der zweiten Seite des Assistenten **Domäne hinzufügen** – es sei denn, Sie richten einen Verbund mit einem lokalen Windows Server Active Directory ein.

Wenn Sie die Domäne für einen Verbund konfigurieren, werden Sie zum Download des Azure AD Connect-Tools weitergeleitet. Führen Sie das Azure AD Connect-Tool aus, um [die DNS-Einträge abzurufen, die Sie bei der Domänennamen-Registrierungsstelle hinzufügen müssen](active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation). Das Azure AD Connect-Tool überprüft auch den Domänennamen für Azure AD.

## Hinzufügen des DNS-Eintrags zur DNS-Zonendatei

1.  Melden Sie sich an der Domänennamen-Registrierungsstelle für die Domäne an. Wenn Sie nicht über ausreichende Berechtigungen zum Aktualisieren des DNS-Eintrags verfügen, wenden Sie sich an die Personen oder Teams, die über den entsprechenden Zugriff zum Hinzufügen des DNS-Eintrag verfügen.

2.  Aktualisieren Sie die DNS-Zonendatei für die Domäne, indem Sie den DNS-Eintrag hinzufügen, der von Azure AD bereitgestellt wird. Mit diesem DNS-Eintrag kann Azure AD prüfen, ob sich die Domäne in Ihrem Besitz befindet. Der DNS-Eintrag nimmt keine Änderungen in Bezug auf das Verhalten, wie etwa E-Mail-Routing oder Webhosting, vor. Bis die DNS-Einträge verteilt wurden, kann es bis zu einer Stunde dauern.

[Anleitung für das Hinzufügen eines DNS-Eintrags bei gängigen DNS-Registrierungsstellen](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## Überprüfen des Domänennamens mit Azure AD

Nach dem Hinzufügen des DNS-Eintrags muss der Domänenname von Azure AD überprüft werden. Das ist der letzte Schritt zum Erfolg.

Falls der Assistent **Domäne hinzufügen** noch geöffnet ist, klicken Sie auf der dritten Seite des Assistenten auf **Überprüfen**. Es kann bis zu einer Stunde dauern, bis der DNS-Eintrag registriert wurde. Warten Sie daher etwas, bevor Sie die Überprüfung durchführen.

Ist der Assistent **Domäne hinzufügen** nicht mehr geöffnet, können Sie die Domäne im [klassischen Azure-Portal](https://manage.windowsazure.com/) überprüfen:

1.  Melden Sie sich mit einem Benutzerkonto an, das ein globaler Administrator Ihres Azure AD-Verzeichnisses ist.

2.  Öffnen Sie Ihr Verzeichnis, und wählen Sie die Registerkarte **Domänen** aus.

3.  Wählen Sie die Domäne aus, die Sie überprüfen möchten.

4.  Wählen Sie auf der Befehlsleiste den Befehl **Überprüfen** und anschließend im Dialogfeld **Überprüfen** aus.

Geschafft! Nun können Sie [Benutzernamen zuweisen, die Ihren benutzerdefinierten Domänennamen enthalten](active-directory-add-domain-add-users.md). Informationen zu möglichen Problemen bei der Überprüfung des Domänennamens finden Sie im Abschnitt [Problembehandlung](#troubleshooting).

## Problembehandlung
Wenn Sie einen benutzerdefinierten Domänennamen nicht überprüfen können, gibt es einige mögliche Ursachen dafür. Wir beginnen mit den häufigsten Ursachen und fahren mit den weniger häufigen fort.

- Sie haben versucht, den Domänennamen zu überprüfen, bevor der DNS-Eintrag verteilt werden konnte. Warten Sie einen Augenblick, und versuchen Sie es anschließend erneut.

- Der DNS-Eintrag wurde nicht eingegeben. Überprüfen Sie den DNS-Eintrag, und warten Sie, bis er verteilt wurde. Versuchen Sie es anschließend erneut.

- Der Domänenname wurde bereits in einem anderen Verzeichnis überprüft. Suchen Sie den Domänennamen, und entfernen Sie ihn aus dem anderen Verzeichnis. Versuchen Sie es anschließend erneut.

- Der DNS-Eintrag enthält einen Fehler. Korrigieren Sie den Fehler, und wiederholen Sie den Vorgang.

- Sie haben keine ausreichende Berechtigung zum Aktualisieren von DNS-Datensätzen. Wenden Sie sich an die Personen oder Teams in Ihrer Organisation, die über einen solchen Zugriff verfügt, und bitten Sie sie, den DNS-Eintrag hinzuzufügen.


## Nächste Schritte

-   [Zuweisen von Benutzernamen, die Ihren benutzerdefinierten Domänennamen enthalten](active-directory-add-domain-add-users.md)
-   [Verwalten von benutzerdefinierten Domänennamen](active-directory-add-manage-domain-names.md)
-   [Informationen zu den Konzepten der Domänenverwaltung in Azure AD](active-directory-add-domain-concepts.md)
-   [Anzeigen des Brandings Ihres Unternehmens bei der Anmeldung der Benutzer](active-directory-add-company-branding.md)
-   [Verwenden von PowerShell zum Verwalten von Domänennamen in Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)

<!---HONumber=AcomDC_0504_2016-->