<properties
	pageTitle="Verwalten der rollenbasierten Zugriffssteuerung (RBAC) mit Azure PowerShell | Microsoft Azure"
	description="So verwalten Sie RBAC mit Azure PowerShell einschließlich der Auflistung von Rollen, der Zuweisung von Rollen und dem Löschen von Rollenzuweisungen."
	services="active-directory"
	documentationCenter=""
	authors="kgremban"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="na"
	ms.workload="identity"
	ms.date="04/12/2016"
	ms.author="kgremban"/>

# Verwalten der rollenbasierten Zugriffssteuerung mit Azure PowerShell

> [AZURE.SELECTOR]
- [PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure-Befehlszeilenschnittstelle](role-based-access-control-manage-access-azure-cli.md)
- [REST-API](role-based-access-control-manage-access-rest.md)


Mit der rollenbasierten Access Control (RBAC) im Azure-Portal und in der Azure Resource Management-API können Sie den Zugriff auf Ihr Abonnement differenziert steuern. Mithilfe dieser Funktion lassen sich Zugriffsberechtigungen für Active Directory-Benutzer, -Gruppen oder -Dienstprinzipale festlegen, indem ihnen bestimmte Rollen für einen bestimmten Bereich zugewiesen werden.

Bevor Sie PowerShell verwenden können, um die RBAC zu verwalten, benötigen Sie Folgendes:

- Azure PowerShell, Version 0.8.8 oder höher. Um die neueste Version zu installieren und sie Ihrem Azure-Abonnement zuzuordnen, lesen Sie [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

- Azure Resource Manager-Cmdlets Installieren Sie die [Azure Resource Manager-Cmdlets](https://msdn.microsoft.com/library/mt125356.aspx) in PowerShell.

## Auflisten der Rollen

### Auflisten aller verfügbaren Rollen
Zum Auflisten von RBAC-Rollen für die Zuweisung und Überprüfung der Vorgänge, auf die sie Zugriff gewähren, verwenden Sie Folgendes:

		Get-AzureRmRoleDefinition

![RBAC PowerShell – Get-AzureRmRoleDefinition – Screenshot](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition1.png)

### Auflisten der Aktionen einer Rolle
Zum Auflisten der Aktionen für eine bestimmte Rolle verwenden Sie Folgendes:

    Get-AzureRmRoleDefinition <role name>

![RBAC PowerShell – Get-AzureRmRoleDefinition für eine bestimmte Rolle – Screenshot](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition2.png)

## Sehen, wer Zugriff hat
Zum Auflisten der RBAC-Zugriffszuweisungen verwenden Sie:

    Get-AzureRmRoleAssignment

###	Auflistung der Rollenzuweisungen für einen bestimmten Bereich
Sie können alle Zugriffszuweisungen für ein angegebenes Abonnement, eine angegebene Ressourcengruppe oder Ressource sehen. Um beispielsweise alle aktiven Zuweisungen für eine Ressourcengruppe zu sehen, verwenden Sie Folgendes:

    Get-AzureRmRoleAssignment -ResourceGroupName <resource group name>

![RBAC PowerShell – Get-AzureRmRoleAssignment für eine Ressourcengruppe – Screenshot](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment1.png)

### Zugewiesenen Rollen eines Benutzers auflisten
Um alle einem bestimmten Benutzer zugewiesenen Rollen aufzulisten, inklusive der Rollen, die den Gruppen zugewiesen sind, denen er angehört, verwenden Sie Folgendes:

    Get-AzureRmRoleAssignment -SignInName <User email> -ExpandPrincipalGroups

![RBAC PowerShell – Get-AzureRmRoleAssignment für einen Benutzer – Screenshot](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment2.png)

### Auflisten der klassischen Rollenzuweisungen für die Dienstadministrator- und Co-Administrator-Rolle
Zum Auflisten der Zugriffszuweisungen für die klassischen Abonnementadministratoren und -Co-Administratoren verwenden Sie Folgendes:

    Get-AzureRmRoleAssignment -IncludeClassicAdministrators

## Gewähren von Zugriff
### Suchen nach Objekt-IDs
Um eine Rolle zuzuweisen, müssen Sie sowohl das Objekt (Benutzer, Gruppe oder Anwendung) als auch den Bereich identifizieren.

Falls Sie die Abonnement-ID nicht kennen, können Sie diese im Blatt **Abonnements** im Azure-Portal finden. Oder erfahren Sie, wie Sie sie mittels [Get-AzureSubscription](https://msdn.microsoft.com/library/dn495302.aspx) auf MSDN abfragen.

Zum Abrufen der Objekt-ID für eine Azure AD-Gruppe verwenden Sie Folgendes:

    Get-AzureRmADGroup -SearchString <group name in quotes>

Zum Abrufen der Objekt-ID für einen Azure AD-Dienstprinzipal oder eine Anwendung verwenden Sie Folgendes:

    Get-AzureRmADServicePrincipal -SearchString <service name in quotes>

### Zuweisen einer Rolle zu einer Anwendung im Abonnementkontext
Zum Zuweisen des Zugriffs zu einer Anwendung im Abonnementkontext verwenden Sie Folgendes:

    New-AzureRmRoleAssignment -ObjectId <application id> -RoleDefinitionName <role name in quotes> -Scope <subscription id>

![RBAC PowerShell – New-AzureRmRoleAssignment – Screenshot](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment2.png)

### Zuweisen einer Rolle zu einem Benutzer im Ressourcengruppenkontext
Zum Zuweisen des Zugriffs zu einem Benutzer im Ressourcengruppenkontext verwenden Sie Folgendes:

    New-AzureRmRoleAssignment -SignInName <email of user> -RoleDefinitionName <role name in quotes> -ResourceGroupName <resource group name>

![RBAC PowerShell – New-AzureRmRoleAssignment – Screenshot](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment3.png)

### Zuweisen einer Rolle zu einer Gruppe im Ressourcenkontext
Zum Zuweisen des Zugriffs zu einer Gruppe im Ressourcenkontext verwenden Sie Folgendes:

    New-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name in quotes> -ResourceName <resource name> -ResourceType <resource type> -ParentResource <parent resource> -ResourceGroupName <resource group name>

![RBAC PowerShell – New-AzureRmRoleAssignment – Screenshot](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment4.png)

## Zugriff entfernen
Zum Entfernen des Zugriffs für Benutzer, Gruppen und Anwendungen verwenden Sie Folgendes:

    Remove-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name> -Scope <scope such as subscription id>

![RBAC PowerShell – Remove-AzureRmRoleAssignment - screenshot](./media/role-based-access-control-manage-access-powershell/3-remove-azure-rm-role-assignment.png)

## Erstellen einer benutzerdefinierten Rolle
Verwenden Sie zum Erstellen einer benutzerdefinierten Rolle den Befehl `New-AzureRmRoleDefinition`.

Das folgende Beispiel erstellt eine benutzerdefinierte Rolle namens *Virtual Machine Operator*, die Zugriff auf alle Lesevorgänge der Ressourcenanbieter *Microsoft.Compute*, *Microsoft.Storage* und *Microsoft.Network* und zum Starten, Neustarten und Überwachen virtueller Computer gewährt. Die benutzerdefinierte Rolle kann in zwei Abonnements verwendet werden.

![RBAC PowerShell – Get-AzureRmRoleDefinition – Screenshot](./media/role-based-access-control-manage-access-powershell/2-new-azurermroledefinition.png)

## Ändern einer benutzerdefinierten Rolle
Um eine benutzerdefinierte Rolle zunächst zu ändern, verwenden Sie den Befehl `Get-AzureRmRoleDefinition`, um die Rollendefinition abzurufen. Nehmen Sie dann die gewünschten Änderungen an der Rollendefinition vor. Verwenden Sie abschließend den Befehl `Set-AzureRmRoleDefinition`, um die geänderte Rollendefinition zu speichern.

Im folgenden Beispiel wird der Vorgang `Microsoft.Insights/diagnosticSettings/*` zur benutzerdefinierten Rolle *Virtual Machine Operator* hinzugefügt.

![RBAC PowerShell – Set-AzureRmRoleDefinition – Screenshot](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-1.png)

Im folgenden Beispiel wird ein Azure-Abonnement den zuweisbaren Bereichen der benutzerdefinierten Rolle „Virtual Machine Operator“ hinzugefügt.

![RBAC PowerShell – Set-AzureRmRoleDefinition – Screenshot](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-2.png)

## Löschen einer benutzerdefinierten Rolle

Verwenden Sie zum Löschen einer benutzerdefinierten Rolle den Befehl `Remove-AzureRmRoleDefinition`.

Im folgenden Beispiel wird die benutzerdefinierte Rolle *Virtual Machine Operator* entfernt.

![RBAC PowerShell – Remove-AzureRmRoleDefinition – Screenshot](./media/role-based-access-control-manage-access-powershell/4-remove-azurermroledefinition.png)

## Auflisten benutzerdefinierter Rollen
Zum Auflisten der Rollen, die in einem Bereich für die Zuweisung verfügbar sind, wählen Sie den Befehl `Get-AzureRmRoleDefinition`.

Im folgenden Beispiel werden alle Rollen aufgelistet, die für die Zuweisung im ausgewählten Abonnement verfügbar sind.

![RBAC PowerShell – Get-AzureRmRoleDefinition – Screenshot](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition-1.png)

Im folgenden Beispiel ist die benutzerdefinierte Rolle *Virtual Machine Operator* im Abonnement *Production4* nicht verfügbar, da dieses Abonnement nicht in **AssignableScopes** für die Rolle enthalten ist.

![RBAC PowerShell – Get-AzureRmRoleDefinition – Screenshot](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition2.png)

## Weitere Informationen
- [Verwenden von Windows PowerShell mit dem Azure-Ressourcen-Manager](../powershell-azure-resource-manager.md)[AZURE.INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]

<!---HONumber=AcomDC_0413_2016-->