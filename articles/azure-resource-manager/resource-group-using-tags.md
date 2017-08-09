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
ms.date: 07/17/2017
ms.author: tomfitz
ms.translationtype: HT
ms.sourcegitcommit: 141270c353d3fe7341dfad890162ed74495d48ac
ms.openlocfilehash: 4f52c30614ad39da8a34ff6ecfb707b75400517f
ms.contentlocale: de-de
ms.lasthandoff: 07/25/2017

---
# <a name="use-tags-to-organize-your-azure-resources"></a>Verwenden von Tags zum Organisieren von Azure-Ressourcen
[!INCLUDE [resource-manager-tag-introduction](../../includes/resource-manager-tag-introduction.md)]

> [!NOTE]
> Sie können Tags nur auf Ressourcen anwenden, die Azure Resource Manager-Vorgänge unterstützen. Wenn Sie einen virtuellen Computer, ein virtuelles Netzwerk oder ein Speicherkonto über das klassische Bereitstellungsmodell erstellt haben (z.B. über das klassische Azure-Portal), können Sie auf diese Ressource kein Tag anwenden. Damit Tags unterstützt werden, müssen Sie diese Ressourcen über den Resource Manager erneut bereitstellen. Alle anderen Ressourcen unterstützen die Markierung durch Tags.
> 
> 

## <a name="policies-for-tag-consistency"></a>Richtlinien für die Tagkonsistenz

Mithilfe von Ressourcenrichtlinien können Sie Standardregeln für Ihre Organisation erstellen. Sie können Richtlinien erstellen, mit denen sichergestellt wird, dass Ressourcen durch die entsprechenden Werte gekennzeichnet werden. Weitere Informationen finden Sie unter [Anwenden von Ressourcenrichtlinien für Tags](resource-manager-policy-tags.md).

## <a name="powershell"></a>PowerShell
[!INCLUDE [resource-manager-tag-resources-powershell](../../includes/resource-manager-tag-resources-powershell.md)]

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Verwenden Sie Folgendes, um die vorhandenen Tags für eine *Ressourcengruppe* anzuzeigen:

```azurecli
az group show -n examplegroup --query tags
```

Das Skript gibt das folgende Format zurück:

```json
{
  "Dept"        : "IT",
  "Environment" : "Test"
}
```

Verwenden Sie Folgendes, um die vorhandenen Tags für eine *Ressource mit einer angegebenen Ressourcen-ID* anzuzeigen:

```azurecli
az resource show --id {resource-id} --query tags
```

Oder verwenden Sie Folgendes, um die vorhandenen Tags für eine *Ressource mit einem angegebenen Namen, Typen und einer Ressourcengruppe* anzuzeigen:

```azurecli
az resource show -n examplevnet -g examplegroup --resource-type "Microsoft.Network/virtualNetworks" --query tags
```

Verwenden Sie `az group list`, um Ressourcengruppen mit einem bestimmten Tag abzurufen:

```azurecli
az group list --tag Dept=IT
```

Zum Abrufen aller Ressourcen mit einem bestimmten Tag und Wert verwenden Sie `az resource list`:

```azurecli
az resource list --tag Dept=Finance
```

Wenn Sie Tags auf eine Ressource oder Ressourcengruppe anwenden, werden die bereits vorhandenen Tags für diese Ressource oder Ressourcengruppe überschrieben. Daher müssen Sie Ihre Vorgehensweise darauf abstimmen, ob für die Ressource oder Ressourcengruppe bereits Tags vorhanden sind. 

Verwenden Sie Folgendes, um einer *Ressourcengruppe ohne vorhandene Tags* Tags hinzuzufügen:

```azurecli
az group update -n examplegroup --set tags.Environment=Test tags.Dept=IT
```

Verwenden Sie Folgendes, um einer *Ressource ohne vorhandene Tags* Tags hinzuzufügen:

```azurecli
az resource tag --tags Dept=IT Environment=Test -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
``` 

Zum Hinzufügen von Tags zu einer Ressource, die bereits Tags aufweist, rufen Sie die vorhandenen Tags ab, formatieren den Wert neu und wenden die vorhandenen und neuen Tags erneut an: 

```azurecli
jsonrtag=$(az resource show -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks" --query tags)
rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
az resource tag --tags $rt Project=Redesign -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
```

Verwenden Sie das folgende Skript, um alle Tags einer Ressourcengruppe auf die darin enthaltenen Ressourcen anzuwenden und *vorhandene Tags für die Ressourcen nicht beizubehalten*:

```azurecli
groups=$(az group list --query [].name --output tsv)
for rg in $groups 
do 
  jsontag=$(az group show -n $rg --query tags)
  t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')
  r=$(az resource list -g $rg --query [].id --output tsv) 
  for resid in $r 
  do 
    az resource tag --tags $t --id $resid
  done 
done
```

Verwenden Sie das folgende Skript, um alle Tags einer Ressourcengruppe auf die darin enthaltenen Ressourcen anzuwenden und *vorhandene Tags für Ressourcen beizubehalten*:

```azurecli
groups=$(az group list --query [].name --output tsv)
for rg in $groups 
do 
  jsontag=$(az group show -n $rg --query tags)
  t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')
  r=$(az resource list -g $rg --query [].id --output tsv) 
  for resid in $r 
  do 
    jsonrtag=$(az resource show --id $resid --query tags)
    rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
    az resource tag --tags $t$rt --id $resid
  done 
done
```


## <a name="templates"></a>Vorlagen

[!INCLUDE [resource-manager-tags-in-templates](../../includes/resource-manager-tags-in-templates.md)]

## <a name="portal"></a>Portal
[!INCLUDE [resource-manager-tag-resource](../../includes/resource-manager-tag-resources.md)]


## <a name="rest-api"></a>REST-API
Das Azure-Portal und PowerShell verwenden im Hintergrund die [Resource Manager-REST-API](https://docs.microsoft.com/rest/api/resources/). Wenn Sie das Tagging in eine andere Umgebung integrieren müssen, können Sie Tags mit **GET** für die Ressourcen-ID abrufen und die Tags mit einem **PATCH**-Aufruf aktualisieren.

## <a name="tags-and-billing"></a>Tags und Abrechnung
Abrechnungsdaten können mithilfe von Tags gruppiert werden. Beispiel: Wenn Sie mehrere virtuelle Computer für verschiedene Organisationen betreiben, gruppieren Sie die Nutzung mithilfe von Tags nach Kostenstelle. Mit Tags können Sie auch Kosten nach Runtimeumgebung kategorisieren, beispielsweise zur Abrechnung der Nutzung virtueller Computer in der Produktionsumgebung.


Informationen zu Tags können Sie über die [Azure-Ressourcennutzungs- und RateCard-APIs](../billing/billing-usage-rate-card-overview.md) oder aus der Nutzungsdatei im CSV-Format abrufen. Sie laden die Nutzungsdatei aus dem [Azure-Kontoportal](https://account.windowsazure.com/) oder dem [EA-Portal](https://ea.azure.com) herunter. Weitere Informationen zum programmgesteuerten Zugriff auf Abrechnungsinformationen finden Sie unter [Gewinnen von Einblicken in den Ressourcenverbrauch unter Microsoft Azure](../billing/billing-usage-rate-card-overview.md). Hinweise zu REST-API-Vorgängen finden Sie unter [Azure Billing REST API Reference (Preview)](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c)(in englischer Sprache).


Wenn Sie die CSV-Nutzungsdatei für Dienste herunterladen, die die Verwendung von Tags für die Abrechnung unterstützen, sind die Tags in der Spalte **Tags** enthalten. Weitere Informationen finden Sie unter [Informationen zu Ihrer Rechnung für Microsoft Azure](../billing/billing-understand-your-bill.md).

![Anzeigen von Tags in der Abrechnung](./media/resource-group-using-tags/billing_csv.png)

## <a name="next-steps"></a>Nächste Schritte
* Sie können mithilfe benutzerdefinierter Richtlinien Einschränkungen und Konventionen für Ihr Abonnement festlegen. Bei einer von Ihnen definierten Richtlinie kann es erforderlich sein, dass alle Ressourcen über einen Wert für ein bestimmtes Tag verfügen. Weitere Informationen finden Sie unter [Verwenden von Richtlinien für Ressourcenverwaltung und Zugriffssteuerung](resource-manager-policy.md).
* Eine Einführung zur Verwendung von Azure PowerShell beim Bereitstellen von Ressourcen finden Sie unter [Verwenden von Windows PowerShell mit dem Azure Resource Manager](powershell-azure-resource-manager.md).
* Eine Einführung zur Verwendung der Azure CLI beim Bereitstellen von Ressourcen finden Sie unter [Verwenden der Azure-Befehlszeilenschnittstelle für Mac, Linux und Windows mit dem Azure Resource Manager](xplat-cli-azure-resource-manager.md).
* Eine Einführung zum Verwenden des Portals finden Sie unter [Verwenden des Azure-Portals zum Verwalten Ihrer Azure-Ressourcen](resource-group-portal.md).  
* Anleitungen dazu, wie Unternehmen Abonnements mit Resource Manager effektiv verwalten können, finden Sie unter [Azure-Unternehmensgerüst - Präskriptive Abonnementgovernance](resource-manager-subscription-governance.md).


