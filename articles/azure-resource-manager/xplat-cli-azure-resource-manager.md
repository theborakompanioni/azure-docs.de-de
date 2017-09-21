---
title: Verwalten von Ressourcen mithilfe der Azure CLI | Microsoft Docs
description: Verwalten Sie Azure-Ressourcen und -Gruppen mithilfe der Azure-Befehlszeilenschnittstelle (Command-Line Interface, CLI).
editor: 
manager: timlt
documentationcenter: 
author: tfitzmac
services: azure-resource-manager
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 09/14/2017
ms.author: tomfitz
ms.translationtype: HT
ms.sourcegitcommit: d24c6777cc6922d5d0d9519e720962e1026b1096
ms.openlocfilehash: 2e3fdf06316bbf68abefe06024f63668bdf07b05
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---
# <a name="use-the-azure-cli-to-manage-azure-resources-and-resource-groups"></a>Verwalten von Azure-Ressourcen und -Ressourcengruppen mithilfe der Azure-Befehlszeilenschnittstelle

In diesem Artikel erfahren Sie, wie Sie Ihre Lösungen mit der Azure CLI und Azure Resource Manager verwalten. Wenn Sie nicht mit Resource Manager vertraut sind, finden Sie weitere Informationen unter [Übersicht über Resource Manager](resource-group-overview.md). In diesem Thema geht es um Verwaltungsaufgaben. In diesem Tutorial führen Sie folgende Schritte aus:

1. Erstellen einer Ressourcengruppe
2. Hinzufügen einer Ressource zur Ressourcengruppe
3. Hinzufügen eines Tags zur Ressource
4. Abfragen von Ressourcen basierend auf Namen oder Tagwerten
5. Anwenden und Entfernen einer Sperre für die Ressource
6. Löschen einer Ressourcengruppe

Das Bereitstellen einer Resource Manager-Vorlage für Ihr Abonnement wird in diesem Artikel nicht erläutert. Informationen hierzu finden Sie unter [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure CLI](resource-group-template-deploy-cli.md).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Informationen für die lokale Installation und Verwendung der CLI finden Sie unter [Install Azure CLI 2.0](/cli/azure/install-azure-cli) (Installieren der Azure CLI 2.0).

## <a name="set-subscription"></a>Festlegen des Abonnements

Wenn Sie über mehrere Abonnements verfügen, können Sie zu einem anderen Abonnement wechseln. Zunächst sehen wir uns alle Abonnements für Ihr Konto an.

```azurecli-interactive
az account list
```

Eine Liste der aktivierten und deaktivierten Abonnements wird zurückgegeben.

```json
[
  {
    "cloudName": "AzureCloud",
    "id": "<guid>",
    "isDefault": true,
    "name": "Example Subscription One",
    "registeredProviders": [],
    "state": "Enabled",
    "tenantId": "<guid>",
    "user": {
      "name": "example@contoso.org",
      "type": "user"
    }
  },
  ...
]
```

Beachten Sie, dass ein Abonnement als Standard markiert ist. Dieses Abonnement ist der aktuelle Kontext für Vorgänge. Um zu einem anderen Abonnement zu wechseln, geben Sie den Namen des Abonnements mit dem Befehl **az account set** an.

```azurecli-interactive
az account set -s "Example Subscription Two"
```

Verwenden Sie zum Anzeigen des aktuellen Abonnementkontexts **az account show** ohne Parameter:

```azurecli-interactive
az account show
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Bevor Sie Ressourcen für Ihr Abonnement bereitstellen, müssen Sie eine Ressourcengruppe erstellen, die die Ressourcen enthält.

Verwenden Sie zum Erstellen einer Ressourcengruppe den Befehl **az group create**. Der Befehl verwendet den **name**-Parameter, um einen Namen für die Ressourcengruppe anzugeben, und den **location**-Parameter, um ihren Speicherort anzugeben.

```azurecli-interactive
az group create --name TestRG1 --location "South Central US"
```

Die Ausgabe weist das folgende Format auf:

```json
{
  "id": "/subscriptions/<subscription-id>/resourceGroups/TestRG1",
  "location": "southcentralus",
  "managedBy": null,
  "name": "TestRG1",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

Wenn Sie die Ressourcengruppe später abrufen müssen, verwenden Sie den folgenden Befehl:

```azurecli-interactive
az group show --name TestRG1
```

Rufen Sie alle Ressourcengruppen in Ihrem Abonnement wie folgt ab:

```azurecli-interactive
az group list
```

## <a name="add-resources-to-a-resource-group"></a>Hinzufügen von Ressourcen zu einer Ressourcengruppe
Um eine Ressource zur Ressourcengruppe hinzuzufügen, können Sie den Befehl **az resource create** oder einen Befehl verwenden, der spezifisch für den Typ der Ressource ist, die Sie erstellen (z.B. **az storage account create**). Sie finden es möglicherweise einfacher, einen für einen Ressourcentyp spezifischen Befehl zu verwenden, da er Parameter für die Eigenschaften enthält, die für die neue Ressource erforderlich sind. Für die Verwendung von **az resource create** müssen Sie alle Eigenschaften kennen, die ohne Aufforderung festgelegt werden müssen.

Das Hinzufügen einer Ressource über ein Skript kann jedoch künftig für Verwirrung sorgen, da die neue Ressource nicht in einer Resource Manager-Vorlage vorhanden ist. Vorlagen ermöglichen Ihnen das zuverlässige und wiederholte Bereitstellen Ihrer Lösung.

Mit dem folgenden Befehl wird ein Speicherkonto erstellt. Geben Sie einen eindeutigen Namen für das Speicherkonto anstelle des im Beispiel angezeigten Namens an. Der Name muss zwischen drei und 24 Zeichen lang sein und darf nur Zahlen und Kleinbuchstaben enthalten. Wenn Sie den im Beispiel angezeigten Namen verwenden, erhalten Sie eine Fehlermeldung, da dieser Name bereits verwendet wird.

```azurecli-interactive
az storage account create -n myuniquestorage -g TestRG1 -l westus --sku Standard_LRS
```

Wenn Sie diese Ressource später abrufen müssen, verwenden Sie den folgenden Befehl:

```azurecli-interactive
az storage account show --name myuniquestorage --resource-group TestRG1
```

## <a name="add-a-tag"></a>Hinzufügen eines Tags

Tags ermöglichen das Organisieren Ihrer Ressourcen gemäß unterschiedlichen Eigenschaften. Beispiel: Sie verfügen über verschiedene Ressourcen in verschiedenen Ressourcengruppen, die zur selben Abteilung gehören. Sie können für diese Ressourcen ein Abteilungstag und einen Wert anwenden, um zu markieren, dass sie derselben Kategorie angehören. Alternativ können Sie markieren, ob eine Ressource in einer Produktionsumgebung oder Testumgebung verwendet wird. In diesem Thema weisen Sie nur einer Ressource Tags zu. In Ihrer Umgebung ist es aber wahrscheinlich am sinnvollsten, allen Ressourcen Tags zuzuweisen.

Der folgende Befehl wendet zwei Tags auf Ihr Speicherkonto an:

```azurecli-interactive
az resource tag --tags Dept=IT Environment=Test -g TestRG1 -n myuniquestorage --resource-type "Microsoft.Storage/storageAccounts"
```

Tags werden als einzelnes Objekt aktualisiert. Um ein Tag zu einer Ressource hinzuzufügen, die bereits Tags enthält, rufen Sie zuerst die vorhandenen Tags ab. Fügen Sie das neue Tag dem Objekt hinzu, das die vorhandenen Tags enthält, und wenden Sie sämtliche Tags neu auf die Ressource an.

```azurecli-interactive
jsonrtag=$(az resource show -g TestRG1 -n myuniquestorage --resource-type "Microsoft.Storage/storageAccounts" --query tags)
rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
az resource tag --tags $rt Project=Redesign -g TestRG1 -n myuniquestorage --resource-type "Microsoft.Storage/storageAccounts"
```

## <a name="search-for-resources"></a>Suchen nach Ressourcen

Verwenden Sie den Befehl **az resource list** zum Abrufen von Ressourcen für andere Suchbedingungen.

* Um eine Ressource anhand des Namens abzurufen, geben Sie den **name**-Parameter an:

  ```azurecli-interactive
  az resource list -n myuniquestorage
  ```

* Um alle Ressourcen in einer Ressourcengruppe abzurufen, geben Sie den **resource-group**-Parameter an:

  ```azurecli-interactive
  az resource list --resource-group TestRG1
  ```

* Um alle Ressourcen mit einem Tagnamen und Wert abzurufen, geben Sie den **tag**-Parameter an:

  ```azurecli-interactive
  az resource list --tag Dept=IT
  ```

* Um alle Ressourcen mit einem bestimmten Ressourcentyp abzurufen, geben Sie den **resource-type**-Parameter an:

  ```azurecli-interactive
  az resource list --resource-type "Microsoft.Storage/storageAccounts"
  ```

## <a name="lock-a-resource"></a>Sperren einer Ressource

Wenn Sie sicherstellen müssen, dass eine wichtige Ressource nicht versehentlich gelöscht oder geändert wird, wenden Sie eine Sperre für die Ressource an. Geben Sie entweder **CanNotDelete** oder **ReadOnly** an.

Zum Erstellen oder Löschen von Verwaltungssperren benötigen Sie Zugriff auf `Microsoft.Authorization/*`- oder `Microsoft.Authorization/locks/*`-Aktionen. Unter den integrierten Rollen verfügen nur „Besitzer“ und „Benutzerzugriffsadministrator“ über diese Aktionen.

Verwenden Sie zum Anwenden einer Sperre den folgenden Befehl:

```azurecli-interactive
az lock create --lock-type CanNotDelete --resource-name myuniquestorage --resource-group TestRG1 --resource-type Microsoft.Storage/storageAccounts --name storagelock
```

Die gesperrte Ressource im vorherigen Beispiel kann erst gelöscht werden, wenn die Sperre aufgehoben wird. Eine Sperre entfernen Sie folgendermaßen:

```azurecli-interactive
az lock delete --name storagelock --resource-group TestRG1 --resource-type Microsoft.Storage/storageAccounts --resource-name myuniquestorage
```

Weitere Informationen zum Setzen von Sperren finden Sie unter [Sperren von Ressourcen mit dem Azure Resource Manager](resource-group-lock-resources.md).

## <a name="remove-resources-or-resource-group"></a>Entfernen von Ressourcen oder Ressourcengruppen
Sie können eine Ressource oder Ressourcengruppe entfernen. Wenn Sie eine Ressourcengruppe entfernen, entfernen Sie auch alle Ressourcen in dieser Ressourcengruppe.

* Verwenden Sie zum Löschen einer Ressource aus der Ressourcengruppe den delete-Befehl für den zu löschenden Ressourcentyp. Dieser Befehl löscht die Ressource, aber nicht die Ressourcengruppe.

  ```azurecli-interactive
  az storage account delete -n myuniquestorage -g TestRG1
  ```

* Führen Sie zum Löschen einer Ressourcengruppe und all ihrer Ressourcen den Befehl **az group delete** aus.

  ```azurecli-interactive
  az group delete -n TestRG1
  ```

Bei beiden Befehlen werden Sie aufgefordert zu bestätigen, dass Sie die Ressource oder Ressourcengruppe entfernen möchten.

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum Erstellen von Resource Manager-Vorlagen finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md).
* Informationen zum Bereitstellen von Vorlagen finden Sie unter [Bereitstellen einer Anwendung mit einer Azure Resource Manager-Vorlage](resource-group-template-deploy-cli.md).
* Sie können vorhandene Ressourcen in eine neue Ressourcengruppe verschieben. Beispiele finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](resource-group-move-resources.md).
* Anleitungen dazu, wie Unternehmen Abonnements mit Resource Manager effektiv verwalten können, finden Sie unter [Azure-Unternehmensgerüst - Präskriptive Abonnementgovernance](resource-manager-subscription-governance.md).
