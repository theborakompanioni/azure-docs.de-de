<properties
	pageTitle="Hinzufügen des benutzerdefinierten Domänennamens zur Vereinfachung der Anmeldung mit Azure Active Directory | Microsoft Azure"
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

# Hinzufügen des benutzerdefinierten Domänennamens zur Vereinfachung der Anmeldung mit Azure Active Directory

Wenn Sie Ihr Verzeichnis in Azure Active Directory (Azure AD) erhalten, besteht eine der wichtigsten Aufgaben darin, einen benutzerdefinierten Domänennamen zu überprüfen, den Ihre Organisation verwendet wird, wie etwa „contoso.com“. Dadurch können Sie Benutzernamen zuweisen, die Ihren Benutzern vertraut sind, z.B. „alice@contoso.com“. Vor der Überprüfung des benutzerdefinierten Domänennamens müssen sich die Benutzer mit ihren Benutzernamen wie „alice@contoso.onmicrosoft.com“, bei denen der anfängliche Domänenname genutzt wird, anmelden.

## Hinzufügen eines benutzerdefinierten Domänennamens zu Ihrem Verzeichnis

So fügen Sie Ihrem Verzeichnis einen benutzerdefinierten Domänennamen hinzu

1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com/) mit einem Benutzerkonto an, das ein globaler Administrator Ihres Azure AD-Verzeichnisses ist.

2. Wählen Sie in der linken Navigationsleiste die Option **Active Directory** aus.

4. Wählen Sie die Registerkarte **Domänen** aus.

5. Wählen Sie in der Befehlsleiste die Option **Hinzufügen** aus.

6. Geben Sie den Namen der benutzerdefinierten Domäne ein, wie z.B. „contoso.com“. Achten Sie darauf, die Erweiterung „.com“, „.net“ oder eine andere Erweiterung der obersten Ebene zu verwenden.

7. Aktivieren Sie das Kontrollkästchen, wenn Sie diese Domäne für die [Verbundanmeldung](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect) mit Ihrer lokalen Active Directory-Instanz konfigurieren möchten.

8. Wählen Sie **Hinzufügen**.

Bevor Sie Benutzernamen zuweisen können, die Ihren benutzerdefinierten Domänennamen enthalten, muss Azure AD überprüfen, ob sich der Domänenname im Besitz Ihres Unternehmens befindet. Zum Durchführen dieser Überprüfung müssen Sie in der DNS-Zonendatei für den Domänennamen einen DNS-Eintrag hinzufügen. Diese Aufgabe kann auf der Website für die Registrierung von Domänennamen ausgeführt werden.

## Rufen Sie die DNS-Einträge für den Domänennamen ab

Wenn Sie die Konfiguration der Domäne für einen Verbund ausgewählt haben, werden Sie entsprechend weitergeleitet, um das Azure AD Connect-Tool herunterzuladen. Führen Sie das Azure AD Connect-Tool aus, um [die DNS-Einträge abzurufen, die Sie bei der Domänennamen-Registrierungsstelle hinzufügen müssen](active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation).

Wenn Sie die Konfiguration der Domäne für einen Verbund nicht ausgewählt haben, finden Sie die DNS-Einträge auf der zweiten Seite des Assistenten **Domäne hinzufügen**.

## Hinzufügen des DNS-Eintrags zur DNS-Zonendatei

So fügen Sie den für Azure AD erforderlichen DNS-Eintrag hinzu:

1.  Melden Sie sich an der Domänennamen-Registrierungsstelle für die Domäne an. Wenn Sie nicht über ausreichende Berechtigungen zum Aktualisieren des DNS-Eintrags verfügen, wenden Sie sich an die Personen oder Teams, die über den entsprechenden Zugriff zum Hinzufügen des DNS-Eintrag verfügen.

2.  Aktualisieren Sie die DNS-Zonendatei für die Domäne, indem Sie den DNS-Eintrag hinzufügen, der von Azure AD bereitgestellt wird. Mit diesem DNS-Eintrag kann Azure AD prüfen, ob sich die Domäne in Ihrem Besitz befindet. Der DNS-Eintrag nimmt keine Änderungen in Bezug auf das Verhalten, wie etwa E-Mail-Routing oder Webhosting, vor.

[Anleitung für das Hinzufügen eines DNS-Eintrags bei gängigen DNS-Registrierungsstellen](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## Überprüfen des Domänennamens mit Azure AD

Nachdem Sie den DNS-Eintrag hinzugefügt haben, können Sie den Domänennamen mit Azure AD überprüfen.

Wenn Sie ausgewählt haben, einen Verbund für Ihren benutzerdefinierten Domänennamen einzurichten, führt das Azure AD Connect-Tool die Überprüfung automatisch durch. Führen Sie das Tool aus, nachdem Sie die Voraussetzungen erfüllt haben. Überprüfen Sie die Domäne andernfalls im klassischen Azure-Portal. Wenn der Assistent **Domäne hinzufügen** noch geöffnet ist, können Sie auf der dritten Seite des Assistenten auf **Überprüfen** klicken. Bis die DNS-Einträge verteilt wurden, kann es bis zu einer Stunde dauern.

Wenn der Assistent **Domäne hinzufügen** nicht geöffnet ist, können Sie die Domäne im [klassischen Azure-Portal](https://manage.windowsazure.com/) überprüfen:

1.  Melden Sie sich mit einem Benutzerkonto an, das ein globaler Administrator Ihres Azure AD-Verzeichnisses ist.

2.  Wählen Sie in der linken Navigationsleiste die Option **Active Directory**.

3.  Öffnen Sie Ihr Verzeichnis.

4.  Wählen Sie die Registerkarte **Domänen**.

5.  Wählen Sie in der Liste mit den Domänen die Domäne aus, die Sie überprüfen möchten.

6.  Wählen Sie in der Befehlsleiste die Option **Überprüfen**.

7.  Wählen Sie im Dialogfeld die Option **Überprüfen**.

Nun können Sie [Benutzernamen zuweisen, die Ihren benutzerdefinierten Domänennamen enthalten](active-directory-add-domain-add-users.md).

## Hinzufügen weiterer benutzerdefinierter Domänennamen

Wenn in Ihrem Unternehmen mehrere benutzerdefinierte Domänennamen verwendet werden, z.B. „contoso.com“ und „contosobank.com“, können Sie diese Ihrem Azure AD-Verzeichnis hinzufügen. Maximal können 900 Domänennamen hinzugefügt werden. Führen Sie die gleichen Schritte aus, um weitere Domänennamen hinzuzufügen.

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

-   [Anzeigen des Brandings Ihres Unternehmens bei der Anmeldung der Benutzer](active-directory-add-company-branding.md)

-   [Verwenden von PowerShell zum Verwalten von Domänennamen in Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)

-   [Informationen zu den Konzepten der Domänenverwaltung in Azure AD](active-directory-add-domain-concepts.md)

<!---HONumber=AcomDC_0427_2016-->