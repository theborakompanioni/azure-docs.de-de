<properties
	pageTitle="Verwalten von benutzerdefinierten Domänennamen in Azure Active Directory | Microsoft Azure"
	description="Verwaltungskonzepte und Vorgehensweisen für die Verwaltung einer benutzerdefinierten Domäne in Azure Active Directory"
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
	ms.topic="article"
	ms.date="04/19/2016"
	ms.author="curtand;jeffsta"/>

# Verwalten von benutzerdefinierten Domänennamen in Azure Active Directory

Ein Domänenname ist ein wichtiger Bestandteil des Bezeichners für viele Verzeichnisressourcen: Er ist Teil eines Benutzernamens oder einer E-Mail-Adresse für einen Benutzer, Teil der Adresse für eine Gruppe und kann Teil des App-ID-URI für eine Anwendung sein. Eine Ressource in Azure Active Directory (Azure AD) kann einen Domänennamen enthalten, für den bereits geprüft wurde, ob er zu dem Verzeichnis gehört, das die Ressource enthält. Nur ein globaler Administrator kann Domänenverwaltungsaufgaben in Azure AD ausführen.

## Festlegen des primären Domänennamens für Ihr Azure AD-Verzeichnis

Wenn Ihr Verzeichnis erstellt wird, ist der anfängliche Domänenname (beispielsweise „contoso.onmicrosoft.com“) auch der primäre Domänenname für Ihr Verzeichnis. Der primäre Domänenname ist der Standarddomänenname, der verwendet wird, wenn Sie im [klassischen Azure-Portal](https://manage.windowsazure.com/) oder in einem anderen Portal wie beispielsweise dem Office 365-Verwaltungsportal einen neuen Benutzer erstellen. Dies optimiert die Erstellung neuer Benutzer durch einen Administrator im Portal.

So ändern Sie den primären Domänennamen für Ihr Verzeichnis:

1.  Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com/) mit einem Benutzerkonto an, das ein globaler Administrator Ihres Azure AD-Verzeichnisses ist.

2.  Wählen Sie in der linken Navigationsleiste die Option **Active Directory**.

3.  Öffnen Sie Ihr Verzeichnis.

4.  Wählen Sie die Registerkarte **Domänen**.

5.  Wählen Sie auf der Befehlsleiste die Schaltfläche **Primäre Domäne ändern**.

6.  Wählen Sie die Domäne aus, die Sie als neue primäre Domäne für Ihr Verzeichnis einrichten möchten.

Sie können den primären Domänennamen für Ihr Verzeichnis in eine beliebige andere überprüfte benutzerdefinierte Domäne (keine Verbunddomäne) ändern. Durch Ändern der primären Domäne für Ihr Verzeichnis werden die Benutzernamen vorhandener Benutzer nicht geändert.

## Hinzufügen von benutzerdefinierten Domänennamen zu Azure AD

Sie können jedem Azure AD-Verzeichnis bis zu 900 benutzerdefinierte Domänennamen hinzufügen. Das Verfahren zum [Hinzufügen eines weiteren benutzerdefinierten Domänennamens](active-directory-add-domain.md) ist das gleiche wie für den ersten benutzerdefinierten Domänennamen.

## Hinzufügen von Unterdomänen einer benutzerdefinierten Domäne

Wenn Sie Ihrem Verzeichnis einen Domänennamen der dritten Ebene wie beispielsweise „europe.contoso.com“ hinzufügen möchten, sollten Sie zuerst die Domäne zweiter Ebene, z. B. „contoso.com“, hinzufügen und überprüfen. Die Unterdomäne wird automatisch von Azure AD überprüft. Um anzuzeigen, ob die gerade hinzugefügte Unterdomäne überprüft wurde, aktualisieren Sie die Seite im Browser, auf der die Domänen in Ihrem Verzeichnis aufgelistet sind.

## Vorgehensweise beim Ändern der DNS-Registrierungsstelle für Ihren benutzerdefinierten Domänennamen

Wenn Sie die DNS-Registrierungsstelle für Ihren benutzerdefinierten Domänennamen ändern, können Sie diesen Namen weiterhin ohne Unterbrechung und ohne zusätzlichen Konfigurationsaufwand in Azure AD selbst verwenden. Wenn Sie Ihren benutzerdefinierten Domänennamen mit Office 365, Intune oder anderen Diensten verwenden, die benutzerdefinierte Domänennamen in Azure AD verwenden, lesen Sie in der Dokumentation zu diesen Diensten nach.

## Löschen eines benutzerdefinierten Domänennamens

Sie können einen benutzerdefinierten Domänennamen aus Azure AD löschen, wenn dieser von Ihrem Unternehmen nicht mehr verwendet wird oder wenn Sie den Domänennamen für eine andere Azure AD-Instanz verwenden möchten.

Um einen benutzerdefinierten Domänennamen zu löschen, müssen Sie zunächst sicherstellen, dass dieser Name von keinerlei Ressourcen in Ihrem Verzeichnis verwendet wird. In folgenden Fällen können Sie einen Domänennamen nicht aus Ihrem Verzeichnis löschen:

-   Ein Benutzer verwendet einen Benutzernamen, eine E-Mail-Adresse oder eine Proxyadresse mit dem Domänennamen.

-   Eine Gruppe verwendet eine E-Mail-Adresse oder Proxyadresse mit dem Domänennamen.

-   Eine Anwendung in Ihrem Azure AD-Verzeichnis besitzt eine App-ID-URI mit dem Domänennamen.

Solche Ressourcen müssen Sie in Ihrem Azure AD-Verzeichnis ändern oder löschen, bevor Sie den benutzerdefinierten Domänennamen löschen können.

## Verwenden von PowerShell oder der Graph-API zum Verwalten von Domänennamen

Die meisten Verwaltungsaufgaben für Domänennamen in Azure Active Directory können auch über Microsoft PowerShell oder programmgesteuert mit der Azure AD-Graph-API (in der öffentlichen Vorschau) durchgeführt werden.

-   [Verwenden von PowerShell zum Verwalten von Domänennamen in Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)

-   [Verwenden der Graph-API zum Verwalten von Domänennamen in Azure AD](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

**Nächste Schritte**

-   [Informationen zu Domänennamen in Azure AD](active-directory-add-domain-concepts.md)

-   [Verwalten von benutzerdefinierten Domänennamen](active-directory-add-manage-domain-names.md)

<!---HONumber=AcomDC_0420_2016-->