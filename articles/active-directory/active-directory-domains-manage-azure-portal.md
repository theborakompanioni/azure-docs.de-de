---
title: Verwalten von benutzerdefinierten Domänennamen in der Azure Active Directory-Vorschau | Microsoft Docs
description: Verwaltungskonzepte und Vorgehensweisen für die Verwaltung eines Domänennamens in Azure Active Directory
services: active-directory
documentationcenter: ''
author: jeffsta
manager: femila
editor: ''

ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2016
ms.author: curtand;jeffsta

---
# Verwalten von benutzerdefinierten Domänennamen in Ihrer Azure Active Directory-Vorschau
Ein Domänenname ist ein wichtiger Bestandteil des Bezeichners für viele Verzeichnisressourcen: Er ist Teil eines Benutzernamens oder einer E-Mail-Adresse für einen Benutzer, Teil der Adresse für eine Gruppe und kann Teil des App-ID-URI für eine Anwendung sein. Eine Ressource in der Azure Active Directory-Vorschau (Azure AD) kann einen Domänennamen enthalten, für den bereits geprüft wurde, ob er zu dem Verzeichnis gehört, das die Ressource enthält. [Was bietet die Vorschauversion?](active-directory-preview-explainer.md) Nur ein globaler Administrator kann Domänenverwaltungsaufgaben in Azure AD ausführen.

## Festlegen des primären Domänennamens für Ihr Azure AD-Verzeichnis
Wenn Ihr Verzeichnis erstellt wird, ist der anfängliche Domänenname (beispielsweise „contoso.onmicrosoft.com“) auch der primäre Domänenname. Die primäre Domäne ist der standardmäßige Domänenname für einen neuen Benutzer, wenn Sie einen neuen Benutzer erstellen. Dies optimiert die Erstellung neuer Benutzer durch einen Administrator im Portal. So ändern Sie den primären Domänennamen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) über ein Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.
2. Wählen Sie **Weitere Dienste** aus, geben Sie **Azure Active Directory** in das Textfeld ein, und drücken Sie die **EINGABETASTE**.
   
   ![Öffnen der Benutzerverwaltung](./media/active-directory-domains-add-azure-portal/user-management.png)
3. Wählen Sie auf dem Blatt ***Verzeichnisname*** die Option **Domänennamen** aus.
4. Wählen Sie auf dem Blatt ***Verzeichnisname* – Domänennamen** den Domänennamen aus, den Sie als primären Domänennamen festlegen möchten.
5. Wählen Sie auf dem Blatt ***Domänenname*** (dem geöffneten Blatt mit Ihrem neuen Domänennamen im Titel) den Befehl **Als primär festlegen** aus. Bestätigen Sie Ihre Auswahl, wenn Sie dazu aufgefordert werden.
   
   ![Festlegen eines Domänennamens als primär](./media/active-directory-domains-manage-azure-portal/make-primary.png)

Sie können den primären Domänennamen für Ihr Verzeichnis in eine beliebige andere überprüfte benutzerdefinierte Domäne (keine Verbunddomäne) ändern. Durch Ändern der primären Domäne für Ihr Verzeichnis werden die Benutzernamen vorhandener Benutzer nicht geändert.

## Hinzufügen von benutzerdefinierten Domänennamen zu Azure AD
Sie können jedem Azure AD-Verzeichnis bis zu 900 benutzerdefinierte Domänennamen hinzufügen. Das Vorgehen zum [Hinzufügen eines weiteren benutzerdefinierten Domänennamens](active-directory-domains-add-azure-portal.md) entspricht dem den ersten benutzerdefinierten Domänennamen.

## Hinzufügen von Unterdomänen einer benutzerdefinierten Domäne
Wenn Sie Ihrem Verzeichnis einen Domänennamen der dritten Ebene wie beispielsweise „europe.contoso.com“ hinzufügen möchten, sollten Sie zuerst die Domäne zweiter Ebene, z. B. „contoso.com“, hinzufügen und überprüfen. Die Unterdomäne wird automatisch von Azure AD überprüft. Um anzuzeigen, ob die gerade hinzugefügte Unterdomäne überprüft wurde, aktualisieren Sie die Seite im Browser, auf der die Domänen aufgelistet sind.

## Vorgehensweise beim Ändern der DNS-Registrierungsstelle für Ihren benutzerdefinierten Domänennamen
Wenn Sie die DNS-Registrierungsstelle für Ihren benutzerdefinierten Domänennamen ändern, können Sie diesen Namen weiterhin ohne Unterbrechung und ohne zusätzlichen Konfigurationsaufwand in Azure AD selbst verwenden. Wenn Sie Ihren benutzerdefinierten Domänennamen mit Office 365, Intune oder anderen Diensten verwenden, die benutzerdefinierte Domänennamen in Azure AD verwenden, lesen Sie in der Dokumentation zu diesen Diensten nach.

## Löschen eines benutzerdefinierten Domänennamens
Sie können einen benutzerdefinierten Domänennamen aus Azure AD löschen, wenn dieser von Ihrem Unternehmen nicht mehr verwendet wird oder wenn Sie den Domänennamen für eine andere Azure AD-Instanz verwenden möchten.

Um einen benutzerdefinierten Domänennamen zu löschen, müssen Sie zunächst sicherstellen, dass dieser Name von keinerlei Ressourcen in Ihrem Verzeichnis verwendet wird. In folgenden Fällen können Sie einen Domänennamen nicht aus Ihrem Verzeichnis löschen:

* Ein Benutzer verwendet einen Benutzernamen, eine E-Mail-Adresse oder eine Proxyadresse mit dem Domänennamen.
* Eine Gruppe verwendet eine E-Mail-Adresse oder Proxyadresse mit dem Domänennamen.
* Eine Anwendung in Ihrem Azure AD-Verzeichnis besitzt eine App-ID-URI mit dem Domänennamen.

Solche Ressourcen müssen Sie in Ihrem Azure AD-Verzeichnis ändern oder löschen, bevor Sie den benutzerdefinierten Domänennamen löschen können.

## Verwenden von PowerShell oder der Graph-API zum Verwalten von Domänennamen
Die meisten Verwaltungsaufgaben für Domänennamen in Azure Active Directory können auch über Microsoft PowerShell oder programmgesteuert mit der Azure AD-Graph-API (in der öffentlichen Vorschau) durchgeführt werden.

* [Verwenden von PowerShell zum Verwalten von Domänennamen in Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
* [Verwenden der Graph-API zum Verwalten von Domänennamen in Azure AD](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## Nächste Schritte
* [Hinzufügen benutzerdefinierter Domänennamen](active-directory-domains-add-azure-portal.md)

<!---HONumber=AcomDC_0914_2016-->