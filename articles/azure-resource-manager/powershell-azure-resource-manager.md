---
title: "Verwalten von Azure-Lösungen mit PowerShell | Microsoft-Dokumentation"
description: Verwenden Sie Azure PowerShell und Resource Manager zum Verwalten von Ressourcen.
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: b33b7303-3330-4af8-8329-c80ac7e9bc7f
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: powershell
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: tomfitz
ms.translationtype: HT
ms.sourcegitcommit: 8021f8641ff3f009104082093143ec8eb087279e
ms.openlocfilehash: c201ac12d06ffc8097615517ae09422b037eba6b
ms.contentlocale: de-de
ms.lasthandoff: 07/21/2017

---
# <a name="manage-resources-with-azure-powershell-and-resource-manager"></a>Verwalten von Ressourcen mit Azure PowerShell und Resource Manager
> [!div class="op_single_selector"]
> * [Portal](resource-group-portal.md)
> * [Azure-Befehlszeilenschnittstelle](xplat-cli-azure-resource-manager.md)
> * [Azure PowerShell](powershell-azure-resource-manager.md)
> * [REST-API](resource-manager-rest-api.md)
>
>

In diesem Artikel erfahren Sie, wie Sie Ihre Lösungen mit Azure PowerShell und Azure Resource Manager verwalten. Wenn Sie nicht mit Resource Manager vertraut sind, finden Sie weitere Informationen unter [Übersicht über Resource Manager](resource-group-overview.md). In diesem Thema geht es um Verwaltungsaufgaben. In diesem Tutorial führen Sie folgende Schritte aus:

1. Erstellen einer Ressourcengruppe
2. Hinzufügen einer Ressource zur Ressourcengruppe
3. Hinzufügen eines Tags zur Ressource
4. Abfragen von Ressourcen basierend auf Namen oder Tagwerten
5. Anwenden und Entfernen einer Sperre für die Ressource
6. Löschen einer Ressourcengruppe

Das Bereitstellen einer Resource Manager-Vorlage für Ihr Abonnement wird in diesem Artikel nicht erläutert. Informationen darüber finden Sie unter [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure PowerShell](resource-group-template-deploy.md).

## <a name="get-started-with-azure-powershell"></a>Erste Schritte mit Azure PowerShell

Wenn Azure PowerShell nicht installiert ist, lesen Sie [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview).

Wenn Sie Azure PowerShell in der Vergangenheit installiert, aber nicht vor kurzem aktualisiert haben, sollten Sie die neueste Version installieren. Sie können die Version über dieselbe Methode aktualisieren, die Sie für die Installation verwendet haben. Beispiel: Wenn Sie den Webplattform-Installer verwendet haben, starten Sie ihn erneut, und suchen Sie nach einem Update.

Zur Prüfung Ihrer Version des Azure-Ressourcenmodells verwenden Sie das folgende Cmdlet:

```powershell
Get-Module -ListAvailable -Name AzureRm.Resources | Select Version
```

Dieses Thema wurde für Version 3.3.0 aktualisiert. Wenn Sie über eine frühere Version verfügen, stimmt Ihre Umgebung möglicherweise nicht mit den in diesem Thema gezeigten Schritten überein. Eine Dokumentation zu den Cmdlets in dieser Version finden Sie unter [AzureRM.Resources Module](/powershell/module/azurerm.resources) (AzureRM.Resources-Modul).

## <a name="log-in-to-your-azure-account"></a>Melden Sie sich bei Ihrem Azure-Konto an.
Bevor Sie an Ihrer Lösung arbeiten, müssen Sie sich bei Ihrem Konto anmelden.

Um sich bei Ihrem Azure-Konto anzumelden, verwenden Sie das Cmdlet **Login-AzureRmAccount**.

```powershell
Login-AzureRmAccount
```

Sie werden vom Cmdlet zur Eingabe der Anmeldeinformationen für Ihr Azure-Konto aufgefordert. Nach dem Anmelden werden Ihre Kontoeinstellungen heruntergeladen, damit sie Azure PowerShell zur Verfügung stehen.

Das Cmdlet gibt Informationen über Ihr Konto und das Abonnement zurück, die für die Aufgaben verwendet werden sollen.

```powershell
Environment           : AzureCloud
Account               : example@contoso.com
TenantId              : {guid}
SubscriptionId        : {guid}
SubscriptionName      : Example Subscription One
CurrentStorageAccount :

```

Wenn Sie über mehrere Abonnements verfügen, können Sie zu einem anderen Abonnement wechseln. Zunächst sehen wir uns alle Abonnements für Ihr Konto an.

```powershell
Get-AzureRmSubscription
```

Aktivierte und deaktivierte Abonnements werden zurückgegeben.

```powershell
SubscriptionName : Example Subscription One
SubscriptionId   : {guid}
TenantId         : {guid}
State            : Enabled

SubscriptionName : Example Subscription Two
SubscriptionId   : {guid}
TenantId         : {guid}
State            : Enabled

SubscriptionName : Example Subscription Three
SubscriptionId   : {guid}
TenantId         : {guid}
State            : Disabled
```

Um zu einem anderen Abonnement zu wechseln, geben Sie den Namen des Abonnements mit dem **Set-AzureRmContext**-Cmdlet an.

```powershell
Set-AzureRmContext -SubscriptionName "Example Subscription Two"
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Bevor Sie Ressourcen für Ihr Abonnement bereitstellen, müssen Sie eine Ressourcengruppe erstellen, die die Ressourcen enthält.

Um eine Ressourcengruppe zu erstellen, verwenden Sie das Cmdlet **New-AzureRmResourceGroup** . Der Befehl verwendet den **Name**-Parameter, um einen Namen für die Ressourcengruppe anzugeben, und den **Location**-Parameter, um ihren Speicherort anzugeben.

```powershell
New-AzureRmResourceGroup -Name TestRG1 -Location "South Central US"
```

Die Ausgabe weist das folgende Format auf:

```powershell
ResourceGroupName : TestRG1
Location          : southcentralus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1
```

Wenn Sie die Ressourcengruppe später abrufen müssen, verwenden Sie das folgende Cmdlet:

```powershell
Get-AzureRmResourceGroup -ResourceGroupName TestRG1
```

Um alle Ressourcengruppen in Ihrem Abonnement zu erhalten, geben Sie keinen Namen an:

```powershell
Get-AzureRmResourceGroup
```

## <a name="add-resources-to-a-resource-group"></a>Hinzufügen von Ressourcen zu einer Ressourcengruppe
Um eine Ressource zur Ressourcengruppe hinzuzufügen, können Sie das **New-AzureRmResource**-Cmdlet oder ein Cmdlet verwenden, das spezifisch für den Typ der Ressource ist, die Sie erstellen (z.B. **New-AzureRmStorageAccount**). Sie finden es möglicherweise einfacher, ein für einen Ressourcentyp spezifisches Cmdlet zu verwenden, da es Parameter für die Eigenschaften enthält, die für die neue Ressource erforderlich sind. Für die Verwendung von **New-AzureRmResource** müssen Sie alle Eigenschaften kennen, die ohne Aufforderung festgelegt werden müssen.

Das Hinzufügen einer Ressource über Cmdlets kann jedoch künftig für Verwirrung sorgen, da die neue Ressource nicht in einer Resource Manager-Vorlage vorhanden ist. Microsoft empfiehlt die Definition der Infrastruktur für Ihre Azure-Lösung in einer Resource Manager-Vorlage. Vorlagen ermöglichen Ihnen das zuverlässige und wiederholte Bereitstellen Ihrer Lösung. Für dieses Thema erstellen Sie ein Speicherkonto mit einem PowerShell-Cmdlet. Später erstellen Sie jedoch eine Vorlage aus Ihrer Ressourcengruppe.

Das folgende Cmdlet erstellt ein Speicherkonto. Geben Sie einen eindeutigen Namen für das Speicherkonto anstelle des im Beispiel angezeigten Namens an. Der Name muss zwischen drei und 24 Zeichen lang sein und darf nur Zahlen und Kleinbuchstaben enthalten. Wenn Sie den im Beispiel angezeigten Namen verwenden, erhalten Sie eine Fehlermeldung, da dieser Name bereits verwendet wird.

```powershell
New-AzureRmStorageAccount -ResourceGroupName TestRG1 -AccountName mystoragename -Type "Standard_LRS" -Location "South Central US"
```

Wenn Sie diese Ressource später abrufen müssen, verwenden Sie das folgende Cmdlet:

```powershell
Get-AzureRmResource -ResourceName mystoragename -ResourceGroupName TestRG1
```

## <a name="add-a-tag"></a>Hinzufügen eines Tags

Tags ermöglichen das Organisieren Ihrer Ressourcen gemäß unterschiedlichen Eigenschaften. Beispiel: Sie verfügen über verschiedene Ressourcen in verschiedenen Ressourcengruppen, die zur selben Abteilung gehören. Sie können für diese Ressourcen ein Abteilungstag und einen Wert anwenden, um zu markieren, dass sie derselben Kategorie angehören. Alternativ können Sie markieren, ob eine Ressource in einer Produktionsumgebung oder Testumgebung verwendet wird. In diesem Thema weisen Sie nur einer Ressource Tags zu. In Ihrer Umgebung ist es aber wahrscheinlich am sinnvollsten, allen Ressourcen Tags zuzuweisen.

Das folgende Cmdlet wendet zwei Tags auf Ihr Speicherkonto an:

```powershell
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceName mystoragename -ResourceGroupName TestRG1 -ResourceType Microsoft.Storage/storageAccounts
 ```

Tags werden als einzelnes Objekt aktualisiert. Um ein Tag zu einer Ressource hinzuzufügen, die bereits Tags enthält, rufen Sie zuerst die vorhandenen Tags ab. Fügen Sie das neue Tag dem Objekt hinzu, das die vorhandenen Tags enthält, und wenden Sie sämtliche Tags neu auf die Ressource an.

```powershell
$tags = (Get-AzureRmResource -ResourceName mystoragename -ResourceGroupName TestRG1).Tags
$tags += @{Status="Approved"}
Set-AzureRmResource -Tag $tags -ResourceName mystoragename -ResourceGroupName TestRG1 -ResourceType Microsoft.Storage/storageAccounts
```

## <a name="search-for-resources"></a>Suchen nach Ressourcen

Verwenden Sie das **Find-AzureRmResource**-Cmdlet zum Abrufen von Ressourcen für andere Suchbedingungen.

* Um eine Ressource anhand des Namens abzurufen, geben Sie den **ResourceNameContains**-Parameter an:

  ```powershell
  Find-AzureRmResource -ResourceNameContains mystoragename
  ```

* Um alle Ressourcen in einer Ressourcengruppe abzurufen, geben Sie den **ResourceGroupNameContains**-Parameter an:

  ```powershell
  Find-AzureRmResource -ResourceGroupNameContains TestRG1
  ```

* Um alle Ressourcen mit einem Tagnamen und Wert abzurufen, geben Sie den **TagName**- und den **TagValue**-Parameter an:

  ```powershell
  Find-AzureRmResource -TagName Dept -TagValue IT
  ```

* Um alle Ressourcen mit einem bestimmten Ressourcentyp abzurufen, geben Sie den **ResourceType**-Parameter an:

  ```powershell
  Find-AzureRmResource -ResourceType Microsoft.Storage/storageAccounts
  ```

## <a name="lock-a-resource"></a>Sperren einer Ressource

Wenn Sie sicherstellen müssen, dass eine wichtige Ressource nicht versehentlich gelöscht oder geändert wird, wenden Sie eine Sperre für die Ressource an. Geben Sie entweder **CanNotDelete** oder **ReadOnly** an.

Zum Erstellen oder Löschen von Verwaltungssperren benötigen Sie Zugriff auf `Microsoft.Authorization/*`- oder `Microsoft.Authorization/locks/*`-Aktionen. Unter den integrierten Rollen verfügen nur „Besitzer“ und „Benutzerzugriffsadministrator“ über diese Aktionen.

Verwenden Sie zum Anwenden einer Sperre das folgende Cmdlet:

```powershell
New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockStorage -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
```

Die gesperrte Ressource im vorherigen Beispiel kann erst gelöscht werden, wenn die Sperre aufgehoben wird. Eine Sperre entfernen Sie folgendermaßen:

```powershell
Remove-AzureRmResourceLock -LockName LockStorage -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
```

Weitere Informationen zum Setzen von Sperren finden Sie unter [Sperren von Ressourcen mit dem Azure Resource Manager](resource-group-lock-resources.md).

## <a name="remove-resources-or-resource-group"></a>Entfernen von Ressourcen oder Ressourcengruppen
Sie können eine Ressource oder Ressourcengruppe entfernen. Wenn Sie eine Ressourcengruppe entfernen, entfernen Sie auch alle Ressourcen in dieser Ressourcengruppe.

* Um eine Ressource aus der Ressourcengruppe zu löschen, verwenden Sie das Cmdlet **Remove-AzureRmResource** . Dieses Cmdlet löscht die Ressource, aber nicht die Ressourcengruppe.

  ```powershell
  Remove-AzureRmResource -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
  ```

* Um eine Ressourcengruppe und alle ihre Ressourcen zu löschen, verwenden Sie das **Remove-AzureRmResourceGroup**-Cmdlet.

  ```powershell
  Remove-AzureRmResourceGroup -Name TestRG1
  ```

Bei beiden Cmdlets werden Sie aufgefordert zu bestätigen, dass Sie die Ressource oder Ressourcengruppe entfernen möchten. Wenn der Vorgang die Ressource oder Ressourcengruppe löscht, wird **true** zurückgegeben.

## <a name="run-resource-manager-scripts-with-azure-automation"></a>Ausführen von Resource Manager-Skripts mit Azure Automation

In diesem Thema wird gezeigt, wie grundlegende Vorgänge für Ihre Ressourcen mit Azure PowerShell ausgeführt werden. Bei fortgeschritteneren Verwaltungsszenarios können Sie ein Skript erstellen und es je nach Bedarf oder basierend auf einem Zeitplan wiederverwenden. [Azure Automation](../automation/automation-intro.md) bietet eine Möglichkeit zum Automatisieren häufig verwendeter Skripts, die Ihre Azure-Lösungen verwalten.

In den folgenden Themen wird gezeigt, wie Sie mit Azure Automation, Resource Manager und PowerShell Verwaltungsaufgaben effektiv ausführen:

- Informationen zum Erstellen eines Runbooks finden Sie unter [Mein erstes PowerShell-Runbook](../automation/automation-first-runbook-textual-powershell.md).
- Informationen zum Arbeiten mit Scriptkatalogen finden Sie unter [Runbook und Modulkataloge für Azure Automation](../automation/automation-runbook-gallery.md).
- Informationen zu Runbooks, die virtuelle Computer starten und beenden, finden Sie unter [Azure Automation-Szenario: Erstellen eines Zeitplans für das Starten und Herunterfahren virtueller Azure-Computer mithilfe von Tags im JSON-Format](../automation/automation-scenario-start-stop-vm-wjson-tags.md).
- Informationen zu Runbooks, die virtuelle Computer außerhalb der Geschäftszeiten starten und beenden, finden Sie unter [Lösung zum Starten/Beenden von VMs außerhalb der Geschäftszeiten in Automation](../automation/automation-solution-vm-management.md).

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum Erstellen von Resource Manager-Vorlagen finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md).
* Informationen zum Bereitstellen von Vorlagen finden Sie unter [Bereitstellen einer Anwendung mit einer Azure Resource Manager-Vorlage](resource-group-template-deploy.md).
* Sie können vorhandene Ressourcen in eine neue Ressourcengruppe verschieben. Beispiele finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](resource-group-move-resources.md).
* Anleitungen dazu, wie Unternehmen Abonnements mit Resource Manager effektiv verwalten können, finden Sie unter [Azure-Unternehmensgerüst - Präskriptive Abonnementgovernance](resource-manager-subscription-governance.md).


