---
title: Gewähren von Benutzerberechtigungen für bestimmte Labrichtlinien | Microsoft Docs
description: Erfahren Sie, wie Sie Berechtigungen für bestimmte Labrichtlinien in DevTest Labs basierend auf den Anforderungen der Benutzer gewähren.
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: tomarcher
manager: douge
editor: ''

ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2016
ms.author: tarcher

---
# Gewähren von Benutzerberechtigungen für bestimmte Labrichtlinien
## Übersicht
Dieser Artikel veranschaulicht, wie Sie mit PowerShell Benutzerberechtigungen für eine bestimmte Labrichtlinie gewähren. Auf diese Weise können Berechtigungen basierend auf den Anforderungen der einzelnen Benutzer gewährt werden. Sie möchten beispielsweise einem bestimmten Benutzer die Möglichkeit zum Ändern der Richtlinieneinstellungen der virtuellen Computer gewähren, jedoch nicht für die Kostenrichtlinien.

## Richtlinien als Ressourcen
Wie im Artikel [Rollenbasierte Access Control in Azure](../active-directory/role-based-access-control-configure.md) verdeutlicht, ermöglicht RBAC eine differenzierte Zugriffsverwaltung für Azure-Ressourcen. Mithilfe von RBAC können Sie Aufgaben in Ihrem DevOps-Team verteilen und Benutzern nur den Zugriff gewähren, den sie zur Ausführung ihrer Aufgaben benötigen.

In DevTest Labs ist eine Richtlinie ein Ressourcentyp, der die RBAC-Aktion **Microsoft.DevTestLab/labs/policySets/policies/** ermöglicht. Jede Labrichtlinie ist eine Ressource im Richtlinienressourcentyp und kann als Bereich einer RBAC-Rolle zugewiesen werden.

Um Benutzern beispielsweise Lese-/Schreibberechtigungen für die Richtlinie** Zulässige VM-Größen** zu erteilen, erstellen Sie eine benutzerdefinierte Rolle, die mit der Aktion **Microsoft.DevTestLab/labs/policySets/policies/*** arbeitet, und weisen dieser benutzerdefinierten Rolle anschließend im Bereich von **Microsoft.DevTestLab/labs/policySets/policies/AllowedVmSizesInLab** die entsprechenden Benutzer zu.

Weitere Informationen zu benutzerdefinierten Rollen in RBAC finden Sie im Abschnitt [Benutzerdefinierte Rollen in Azure RBAC](../active-directory/role-based-access-control-configure.md#custom-roles-in-azure-rbac) im Artikel [Rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-control-configure.md).

## Erstellen einer benutzerdefinierten Labrolle mithilfe von PowerShell
Lesen Sie zuerst den folgenden Artikel, der erklärt, wie Sie die Azure PowerShell-Cmdlets installieren und konfigurieren: [https://azure.microsoft.com/blog/azps-1-0-pre](https://azure.microsoft.com/blog/azps-1-0-pre).

Nachdem Sie Azure PowerShell-Cmdlets eingerichtet haben, können Sie die folgenden Aufgaben ausführen:

* Auflisten aller Vorgänge/Aktionen für einen Ressourcenanbieter
* Auflisten der Aktionen in einer bestimmten Rolle
* Erstellen einer benutzerdefinierten Rolle

Im folgenden PowerShell-Skript sind Beispiele für diese Aufgaben veranschaulicht:

    ‘List all the operations/actions for a resource provider.
    Get-AzureRmProviderOperation -OperationSearchString "Microsoft.DevTestLab/*"

    ‘List actions in a particular role.
    (Get-AzureRmRoleDefinition "DevTest Labs User").Actions

    ‘Create custom role.
    $policyRoleDef = (Get-AzureRmRoleDefinition "DevTest Labs User")
    $policyRoleDef.Id = $null
    $policyRoleDef.Name = "Policy Contributor"
    $policyRoleDef.IsCustom = $true
    $policyRoleDef.AssignableScopes.Clear()
    $policyRoleDef.AssignableScopes.Add("/subscriptions/<SubscriptionID> ")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/policySets/policies/*")
    $policyRoleDef = (New-AzureRmRoleDefinition -Role $policyRoleDef)

## Zuweisen von Benutzerberechtigungen für eine bestimmte Richtlinie mithilfe von benutzerdefinierten Rollen
Nachdem Sie Ihre benutzerdefinierten Rollen definiert haben, können Sie diese Benutzern zuweisen. Um einem Benutzer eine benutzerdefinierte Rolle zuzuweisen, benötigen Sie zunächst die **ObjectId** dieses Benutzers. Verwenden Sie dazu das **Get-AzureRmADUser**-Cmdlet.

Im folgenden Beispiel lautet die **ObjectId** des Benutzers *SomeUser* 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3.

    PS C:\>Get-AzureRmADUser -SearchString "SomeUser"

    DisplayName                    Type                           ObjectId
    -----------                    ----                           --------
    someuser@hotmail.com                                          05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3

Sobald Sie über die **ObjectId** für den Benutzer und den Namen einer benutzerdefinierte Rolle verfügen, können Sie dem Benutzer diese Rolle mit dem **New-AzureRmRoleAssignment**-Cmdlet zuweisen:

    PS C:\>New-AzureRmRoleAssignment -ObjectId 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 -RoleDefinitionName "Policy Contributor" -Scope /subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.DevTestLab/labs/<LabName>/policySets/policies/AllowedVmSizesInLab

Im vorherigen Beispiel wird die **AllowedVmSizesInLab**-Richtlinie verwendet. Sie können auch eine der folgenden Richtlinien verwenden:

* MaxVmsAllowedPerUser
* MaxVmsAllowedPerLab
* AllowedVmSizesInLab
* LabVmsShutdown

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## Nächste Schritte
Nachdem Sie Benutzerberechtigungen für bestimmte Labrichtlinien gewährt haben, finden Sie im Folgenden Informationen zu den nächsten möglichen Schritten:

* [Sicherer Zugriff auf ein Lab](devtest-lab-add-devtest-user.md).
* [Festlegen von Labrichtlinien](devtest-lab-set-lab-policy.md).
* [Erstellen einer Labvorlage](devtest-lab-create-template.md).
* [Erstellen benutzerdefinierter Artefakte für Ihre VMs](devtest-lab-artifact-author.md).
* [Hinzufügen einer VM mit Artefakten zu einem Lab](devtest-lab-add-vm-with-artifacts.md).

<!---HONumber=AcomDC_0831_2016-->