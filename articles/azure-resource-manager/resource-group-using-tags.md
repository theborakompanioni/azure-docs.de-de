---
title: "Kennzeichnen von Azure-Ressourcen für die logische Organisation | Microsoft-Dokumentation"
description: "Zeigt, wie Sie Tags zum Organisieren von Azure-Ressourcen für die Abrechnung und Verwaltung anwenden können."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 003a78e5-2ff8-4685-93b4-e94d6fb8ed5b
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: AzurePortal
ms.devlang: na
ms.topic: article
ms.date: 02/03/2017
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: ec80273fd388a435bc0aee9cb2dd49df12535923
ms.openlocfilehash: c5270c56ab6d0a4f200b0707554705d3b7855d32


---
# <a name="use-tags-to-organize-your-azure-resources"></a>Verwenden von Tags zum Organisieren von Azure-Ressourcen
[!INCLUDE [resource-manager-tag-introduction](../../includes/resource-manager-tag-introduction.md)]

> [!NOTE]
> Sie können Tags nur auf Ressourcen anwenden, die Resource Manager-Vorgänge unterstützen. Wenn Sie einen virtuellen Computer, ein virtuelles Netzwerk (VNET) oder Speicher über das klassische Bereitstellungsmodell erstellt haben (z.B. über das klassische Azure-Portal), können Sie auf diese Ressource kein Tag anwenden. Damit Tags unterstützt werden, müssen Sie diese Ressourcen über den Resource Manager erneut bereitstellen. Alle anderen Ressourcen unterstützen die Markierung durch Tags.
> 
> 

## <a name="templates"></a>Vorlagen

[!INCLUDE [resource-manager-tags-in-templates](../../includes/resource-manager-tags-in-templates.md)]

## <a name="portal"></a>Portal
[!INCLUDE [resource-manager-tag-resource](../../includes/resource-manager-tag-resources.md)]

## <a name="powershell"></a>PowerShell
[!INCLUDE [resource-manager-tag-resources-powershell](../../includes/resource-manager-tag-resources-powershell.md)]

## <a name="azure-cli-20-preview"></a>Azure CLI 2.0 (Vorschau)

Mit Azure CLI 2.0 (Vorschau) können Sie Ressourcen und Ressourcengruppen Tags hinzufügen und Ressourcen anhand von Tag-Werten abfragen.

Wenn Sie Tags auf eine Ressource oder Ressourcengruppe anwenden, werden die bereits vorhandenen Tags für diese Ressource oder Ressourcengruppe überschrieben. Daher müssen Sie Ihre Vorgehensweise darauf abstimmen, ob für die Ressource oder Ressourcengruppe bereits Tags vorhanden sind, die Sie beibehalten möchten. Mögliche Szenarien:

* Hinzufügen von Tags zu einer Ressourcengruppe ohne vorhandene Tags

  ```azurecli
  az group update -n TagTestGroup --set tags.Environment=Test tags.Dept=IT
  ```

* Hinzufügen von Tags zu einer Ressource ohne vorhandene Tags

  ```azurecli
  az resource tag --tags Dept=IT Environment=Test -g TagTestGroup -n storageexample --resource-type "Microsoft.Storage/storageAccounts"
  ``` 

Um einer Ressource ein Tag hinzuzufügen, die bereits Tags enthält, rufen Sie zuerst die vorhandenen Tags ab: 

```azurecli
az resource show --query tags --output list -g TagTestGroup -n storageexample --resource-type "Microsoft.Storage/storageAccounts"
```

Hiermit wird das folgende Format zurückgegeben:

```
Dept        : Finance
Environment : Test
```

Wenden Sie die vorhandenen Tags erneut auf die Ressource an, und fügen Sie die neuen Tags hinzu.

```azurecli
az resource tag --tags Dept=Finance Environment=Test CostCenter=IT -g TagTestGroup -n storageexample --resource-type "Microsoft.Storage/storageAccounts"
``` 

Zum Abrufen von Ressourcengruppen mit einem bestimmten Tag verwenden Sie `az group list`.

```azurecli
az group list --tag Dept=IT
```

Zum Abrufen aller Ressourcen mit einem bestimmten Tag und Wert verwenden Sie `az resource list`.

```azurecli
az resource list --tag Dept=Finance
```

## <a name="azure-cli-10"></a>Azure-Befehlszeilenschnittstelle 1.0
[!INCLUDE [resource-manager-tag-resources-cli](../../includes/resource-manager-tag-resources-cli.md)]

## <a name="rest-api"></a>REST-API
Das Vorschauportal und PowerShell verwenden im Hintergrund die [Ressourcen-Manager-REST-API](https://docs.microsoft.com/rest/api/resources/) . Wenn Sie das Tagging in eine andere Umgebung integrieren müssen, können Sie Tags mit GET für die Ressourcen-ID abrufen und die Tags mit einem PATCH-Aufruf aktualisieren.

## <a name="tags-and-billing"></a>Tags und Abrechnung
Mithilfe von Tags können Sie Ihre Abrechnungsdaten gruppieren. Beispiel: Wenn Sie mehrere virtuelle Computer für verschiedene Organisationen betreiben, gruppieren Sie die Nutzung mithilfe von Tags nach Kostenstelle. Mit Tags können Sie auch Kosten nach Laufzeitumgebung kategorisieren (beispielsweise zur Abrechnung der Nutzung virtueller Computer in der Produktionsumgebung).


Informationen zu Tags können Sie über die [Azure-Ressourcennutzungs- und RateCard-APIs](../billing/billing-usage-rate-card-overview.md) oder aus der Nutzungsdatei im CSV-Format abrufen. Sie laden die Nutzungsdatei aus dem [Azure-Kontenportal](https://account.windowsazure.com/) oder dem [EA-Portal](https://ea.azure.com) herunter. Weitere Informationen zum programmgesteuerten Zugriff auf Abrechnungsinformationen finden Sie unter [Gewinnen von Einblicken in den Ressourcenverbrauch unter Microsoft Azure](../billing/billing-usage-rate-card-overview.md). Hinweise zu REST-API-Vorgängen finden Sie unter [Azure Billing REST API Reference (Preview)](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c)(in englischer Sprache).


Wenn Sie die CSV-Nutzungsdatei für Dienste herunterladen, die die Verwendung von Tags für die Abrechnung unterstützen, sind die Tags in der Spalte **Tags** enthalten. Weitere Informationen finden Sie unter [Informationen zu Ihrer Rechnung für Microsoft Azure](../billing/billing-understand-your-bill.md).

![Anzeigen von Tags in der Abrechnung](./media/resource-group-using-tags/billing_csv.png)

## <a name="next-steps"></a>Nächste Schritte
* Sie können mithilfe benutzerdefinierter Richtlinien Einschränkungen und Konventionen für Ihr Abonnement festlegen. Die Richtlinie, die Sie definieren, kann erfordern, dass alle Ressourcen über einen Wert für ein bestimmtes Tag verfügen. Weitere Informationen finden Sie unter [Verwenden von Richtlinien für Ressourcenverwaltung und Zugriffssteuerung](resource-manager-policy.md).
* Eine Einführung zur Verwendung von Azure PowerShell für das Bereitstellen von Ressourcen finden Sie unter [Verwenden von Windows PowerShell mit dem Azure-Ressourcen-Manager](powershell-azure-resource-manager.md).
* Eine Einführung zur Verwendung der Azure-Befehlszeilenschnittstelle für das Bereitstellen von Ressourcen finden Sie unter [Verwenden der Azure-Befehlszeilenschnittstelle für Mac, Linux und Windows mit der Azure-Ressourcenverwaltung](xplat-cli-azure-resource-manager.md).
* Eine Einführung zum Verwenden des Portals finden Sie unter [Verwenden des Azure-Portals zum Verwalten Ihrer Azure-Ressourcen](resource-group-portal.md)  
* Anleitungen dazu, wie Unternehmen Abonnements mit Resource Manager effektiv verwalten können, finden Sie unter [Azure-Unternehmensgerüst - Präskriptive Abonnementgovernance](resource-manager-subscription-governance.md).




<!--HONumber=Feb17_HO2-->


