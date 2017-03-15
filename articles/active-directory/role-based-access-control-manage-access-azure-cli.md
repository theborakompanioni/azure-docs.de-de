---
title: Verwalten der rollenbasierten Zugriffssteuerung (RBAC) mit der Azure-CLI | Microsoft Docs
description: Erfahren Sie, wie Sie die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) mit der Azure-Befehlszeilenschnittstelle verwalten, indem Sie Rollen und Rollenaktionen auflisten und Rollen im Kontext von Abonnements und Anwendungen zuweisen.
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: 
ms.assetid: 3483ee01-8177-49e7-b337-4d5cb14f5e32
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/22/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 2f03ba60d81e97c7da9a9fe61ecd419096248763
ms.openlocfilehash: d9ef14d1d83d98de5c1d95b237a2e06ea2506766
ms.lasthandoff: 03/04/2017


---
# <a name="manage-role-based-access-control-with-the-azure-command-line-interface"></a>Verwalten der rollenbasierten Zugriffssteuerung mit der Azure-Befehlszeilenschnittstelle
> [!div class="op_single_selector"]
> * [PowerShell](role-based-access-control-manage-access-powershell.md)
> * [Azure-Befehlszeilenschnittstelle](role-based-access-control-manage-access-azure-cli.md)
> * [REST-API](role-based-access-control-manage-access-rest.md)


Mit der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) im Azure-Portal und der Azure Resource Manager-API können Sie den Zugriff auf Ihr Abonnement und Ihre Ressourcen differenziert steuern. Mithilfe dieser Funktion lassen sich Zugriffsberechtigungen für Active Directory-Benutzer, -Gruppen oder -Dienstprinzipale festlegen, indem ihnen bestimmte Rollen für einen bestimmten Bereich zugewiesen werden.

Bevor Sie RBAC mithilfe der Azure-Befehlszeilenschnittstelle verwalten können, benötigen Sie Folgendes:

* Azure-Befehlszeilenschnittstelle, Version 0.8.8 oder höher. Um die neueste Version zu installieren und sie Ihrem Azure-Abonnement zuzuordnen, lesen Sie [Installieren und Konfigurieren der Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md).
* Azure Resource Manager unter der Azure-Befehlszeilenschnittstelle. Weitere Informationen finden Sie unter [Verwenden der Azure-Befehlszeilenschnittstelle mit dem Resource Manager](../xplat-cli-azure-resource-manager.md) .

## <a name="list-roles"></a>Auflisten der Rollen
### <a name="list-all-available-roles"></a>Auflisten aller verfügbaren Rollen
Um alle verfügbaren Rollen aufzulisten, verwenden Sie Folgendes:

        azure role list

Das folgende Beispiel zeigt die Liste *aller verfügbaren Rollen*.

```
azure role list --json | jq '.[] | {"roleName":.properties.roleName, "description":.properties.description}'
```

![Azure-Befehlszeile für die RBAC – Azure-Rollenliste – Screenshot](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-list.png)

### <a name="list-actions-of-a-role"></a>Auflisten der Aktionen einer Rolle
Um die Aktionen einer Rolle aufzulisten, verwenden Sie Folgendes:

    azure role show "<role name>"

Das folgende Beispiel zeigt die Aktionen der Rollen *Contributor* (Mitwirkender) und *Virtual Machine Contributor* (Mitwirkender für virtuelle Computer).

```
azure role show "contributor" --json | jq '.[] | {"Actions":.properties.permissions[0].actions,"NotActions":properties.permissions[0].notActions}'

azure role show "virtual machine contributor" --json | jq '.[] | .properties.permissions[0].actions'
```

![Azure-Befehlszeile für die RBAC – Azure-Rollenanzeige – Screenshot](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-show.png)

## <a name="list-access"></a>Auflisten des Zugriffs
### <a name="list-role-assignments-effective-on-a-resource-group"></a>Auflisten der effektiven Rollenzuweisungen für eine Ressourcengruppe
Verwenden Sie Folgendes, um die in einer Ressourcengruppe enthaltenen Rollenzuweisungen aufzulisten:

    azure role assignment list --resource-group <resource group name>

Das folgende Beispiel zeigt die Rollenzuweisungen für die Gruppe *pharma-sales-projectforecast* .

```
azure role assignment list --resource-group pharma-sales-projecforcast --json | jq '.[] | {"DisplayName":.properties.aADObject.displayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![Azure-Befehlszeile für die RBAC – Azure-Rollenzuweisungsliste nach Gruppe – Screenshot](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-1.png)

### <a name="list-role-assignments-for-a-user"></a>Liste von Rollenzuweisungen für einen Benutzer
Verwenden Sie zum Auflisten der Rollenzuweisungen für einen bestimmten Benutzer und der Zuweisungen der Gruppen eines Benutzers Folgendes:

    azure role assignment list --signInName <user email>

Sie können auch von Gruppen geerbte Rollenzuweisungen anzeigen. Ändern Sie dazu den Befehl folgendermaßen:

    azure role assignment list --expandPrincipalGroups --signInName <user email>

Das folgende Beispiel zeigt die Rollenzuweisungen für den Benutzer *sameert@aaddemo.com* . Darin sind Rollen enthalten, die dem Benutzer direkt zugewiesen werden, und Rollen, die von Gruppen geerbt werden.

```
azure role assignment list --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'

azure role assignment list --expandPrincipalGroups --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![Azure-Befehlszeile für die RBAC – Azure-Rollenzuweisungsliste nach Benutzer – Screenshot](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-2.png)

## <a name="grant-access"></a>Gewähren von Zugriff
So gewähren Sie Zugriff, nachdem Sie die Rolle, die Sie zuweisen möchten, identifiziert haben:

    azure role assignment create

### <a name="assign-a-role-to-group-at-the-subscription-scope"></a>Zuweisen einer Rolle zu einer Gruppe im Abonnementkontext
So weisen Sie eine Rolle einer Gruppe im Abonnementkontext zu:

    azure role assignment create --objectId  <group object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

Das folgende Beispiel weist die Rolle *Reader* dem *Team von Christine Koch* im *Abonnementkontext* zu.

![Azure-Befehlszeile für die RBAC – Azure-Rollenzuweisungserstellung nach Gruppe – Screenshot](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-1.png)

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a>Zuweisen einer Rolle zu einer Anwendung im Abonnementkontext
So weisen Sie eine Rolle einer Anwendung im Abonnementkontext zu:

    azure role assignment create --objectId  <applications object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

Das folgende Beispiel gewährt einer *Azure AD-Anwendung* für das ausgewählte Abonnement die Rolle *Contributor* (Mitwirkender).

 ![Azure-Befehlszeile für die RBAC – Azure-Rollenzuweisungserstellung nach Anwendung](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-2.png)

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a>Zuweisen einer Rolle zu einem Benutzer im Ressourcengruppenkontext
So weisen Sie eine Rolle einem Benutzer im Ressourcengruppenkontext zu:

    azure role assignment create --signInName  <user email address> --roleName "<name of role>" --resourceGroup <resource group name>

Im folgenden Beispiel wird dem Benutzer *samert@aaddemo.com* im Ressourcengruppenkontext *Pharma-Sales-ProjectForcast* die Rolle *Virtual Machine Contributor* (Mitwirkender für virtuelle Computer) gewährt.

![Azure-Befehlszeile für die RBAC – Azure-Rollenzuweisungserstellung nach Benutzer – Screenshot](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-3.png)

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a>Zuweisen einer Rolle zu einer Gruppe im Ressourcenkontext
So weisen Sie einer Gruppe im Ressourcenkontext eine Rolle zu:

    azure role assignment create --objectId <group id> --role "<name of role>" --resource-name <resource group name> --resource-type <resource group type> --parent <resource group parent> --resource-group <resource group>

Das folgende Beispiel gewährt einer *Azure AD-Gruppe* in einem *Subnetz* die Rolle *Virtual Machine Contributor* (Mitwirkender für virtuelle Computer).

![Azure-Befehlszeile für die RBAC – Azure-Rollenzuweisungserstellung nach Gruppe – Screenshot](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-4.png)

## <a name="remove-access"></a>Zugriff entfernen
Um eine Rollenzuordnung zu entfernen, verwenden Sie Folgendes:

    azure role assignment delete --objectId <object id to from which to remove role> --roleName "<role name>"

Im folgenden Beispiel wird die Zuweisung der Rolle *Virtual Machine Contributor* (Mitwirkender für virtuelle Computer) von Benutzer *sammert@aaddemo.com* für die Ressourcengruppe *Pharma-Sales-ProjectForcast* entfernt.
Anschließend wird im Beispiel die Rollenzuweisung aus einer Gruppe im Abonnement entfernt.

![Azure-Befehlszeile für die RBAC – Azure-Rollenzuweisungslöschung nach Benutzer – Screenshot](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-assignment-delete.png)

## <a name="create-a-custom-role"></a>Erstellen einer benutzerdefinierten Rolle
So erstellen Sie eine benutzerdefinierte Rolle:

    azure role create --inputfile <file path>

Im folgenden Beispiel wird die benutzerdefinierte Rolle *Virtual Machine Operator*erstellt. Diese benutzerdefinierte Rolle gewährt Zugriff auf alle Lesevorgänge der Ressourcenanbieter *Microsoft.Compute*, *Microsoft.Storage* und *Microsoft.Network* sowie zum Starten, Neustarten und Überwachen virtueller Computer. Diese benutzerdefinierte Rolle kann in zwei Abonnements verwendet werden. In diesem Beispiel wird eine JSON-Datei als Eingabe genutzt.

![JSON – Definition der benutzerdefinierten Rolle – Screenshot](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-1.png)

![Azure-Befehlszeile für die RBAC – Azure-Rollenerstellung – Screenshot](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-2.png)

## <a name="modify-a-custom-role"></a>Ändern einer benutzerdefinierten Rolle
Um eine benutzerdefinierte Rolle zunächst zu ändern, verwenden Sie den Befehl `azure role show` , um die Rollendefinition abzurufen. Nehmen Sie zweitens die gewünschten Änderungen an der Rollendefinitionsdatei vor. Verwenden Sie abschließend `azure role set` , um die geänderte Rollendefinition zu speichern.

    azure role set --inputfile <file path>

Im folgenden Beispiel wird der Vorgang *Microsoft.Insights/diagnosticSettings/* zu **Actions** und ein Azure-Abonnement zu **AssignableScopes** der benutzerdefinierten Rolle „Virtual Machine Operator“ (Mitwirkender für virtuelle Computer) hinzugefügt.

![JSON – Ändern der Definition der benutzerdefinierten Rolle – Screenshot](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set-1.png)

![Azure-Befehlszeile für RBAC – Azure-Rollenfestlegung – Screenshot](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set2.png)

## <a name="delete-a-custom-role"></a>Löschen einer benutzerdefinierten Rolle
Verwenden Sie zum Löschen einer benutzerdefinierten Rolle zuerst den Befehl `azure role show` , um die **ID** der Rolle zu ermitteln. Führen Sie anschließend den Befehl `azure role delete` aus, um die Rolle unter Angabe der **ID**zu löschen.

Im folgenden Beispiel wird die benutzerdefinierte Rolle *Virtual Machine Operator* entfernt.

![Azure-Befehlszeile für RBAC – Azure-Rollenlöschung – Screenshot](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-delete.png)

## <a name="list-custom-roles"></a>Auflisten benutzerdefinierter Rollen 
Zum Auflisten der Rollen, die in einem Bereich für die Zuweisung verfügbar sind, wählen Sie den Befehl `azure role list` .

Der folgende Befehl führt alle Rollen auf, die für die Zuweisung im ausgewählten Abonnement verfügbar sind.

```
azure role list --json | jq '.[] | {"name":.properties.roleName, type:.properties.type}'
```

![Azure-Befehlszeile für die RBAC – Azure-Rollenliste – Screenshot](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list1.png)

Im folgenden Beispiel ist die benutzerdefinierte Rolle *Virtual Machine Operator* im Abonnement *Production4* nicht verfügbar, da dieses Abonnement nicht in **AssignableScopes** für die Rolle enthalten ist.

```
azure role list --json | jq '.[] | if .properties.type == "CustomRole" then .properties.roleName else empty end'
```

![Azure-Befehlszeile für RBAC – Azure-Rollenliste für benutzerdefinierte Rollen – Screenshot](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list2.png)

## <a name="rbac-topics"></a>RBAC-Themen
[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]


