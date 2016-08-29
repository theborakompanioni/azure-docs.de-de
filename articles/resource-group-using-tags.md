<properties
	pageTitle="Verwenden von Tags zum Organisieren von Azure-Ressourcen | Microsoft Azure"
	description="Zeigt, wie Sie Tags zum Organisieren von Ressourcen für die Abrechnung und Verwaltung anwenden können."
	services="azure-resource-manager"
	documentationCenter=""
	authors="tfitzmac"
	manager="timlt"
	editor="tysonn"/>

<tags
	ms.service="azure-resource-manager"
	ms.workload="multiple"
	ms.tgt_pltfrm="AzurePortal"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/16/2016"
	ms.author="tomfitz"/>


# Verwenden von Tags zum Organisieren von Azure-Ressourcen

Mit dem Resource Manager können Sie Ressourcen durch Anwenden von Tags logisch organisieren. Tags bestehen aus Schlüssel-Wert-Paaren, die Ressourcen mit den Eigenschaften identifizieren, die Sie definieren. Um Ressourcen so zu kennzeichnen, dass sie zur selben Kategorie gehören, wenden Sie das gleiche Tag auf diese Ressourcen an.

Wenn Sie Ressourcen mit einem bestimmten Tag anzeigen, sehen Sie Ressourcen aus allen Ressourcengruppen. Sie sind nicht auf Ressourcen in der gleichen Ressourcengruppe beschränkt, weshalb Sie Ihre Ressourcen unabhängig von Bereitstellungsbeziehungen organisieren können. Tags können besonders hilfreich sein, wenn Sie Ressourcen zur Abrechnung oder Verwaltung organisieren müssen.

Jedes Tag, das Sie einer Ressource oder Ressourcengruppe hinzufügen, wird automatisch der Taxonomie für das Abonnement hinzugefügt. Sie können die Taxonomie für Ihr Abonnement auch vorab mit Tagnamen und -werten füllen, die Sie zukünftig zum Markieren von Ressourcen verwenden möchten.

Jede Ressource oder Ressourcengruppe kann maximal 15 Tags haben. Der Tagname ist auf 512 Zeichen beschränkt und der Tagwert auf 256 Zeichen.

> [AZURE.NOTE] Sie können Tags nur auf Ressourcen anwenden, die Resource Manager-Vorgänge unterstützen. Wenn Sie einen virtuellen Computer, ein virtuelles Netzwerk (VNET) oder Speicher über das klassische Bereitstellungsmodell erstellt haben (z.B. über das klassische Azure-Portal), können Sie auf diese Ressource kein Tag anwenden. Damit Tags unterstützt werden, müssen Sie diese Ressourcen über den Resource Manager erneut bereitstellen. Alle anderen Ressourcen unterstützen die Markierung durch Tags.

## Vorlagen

Um eine Ressource während der Bereitstellung zu markieren, fügen Sie der Ressource, die Sie bereitstellen, einfach das **tags**-Element hinzu, und geben Sie den Namen und den Wert des Tags an. Der Tagname und der Wert müssen nicht bereits in Ihrem Abonnement vorhanden sein. Sie können für jede Ressource bis zu 15 Tags bereitstellen.

Das folgende Beispiel zeigt ein Speicherkonto mit einem Tag.

    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2015-06-15",
            "name": "[concat('storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "tags": {
                "dept": "Finance"
            },
            "properties": 
            {
                "accountType": "Standard_LRS"
            }
        }
    ]

Derzeit wird die Verarbeitung eines Objekts für die Tagnamen und -werte von Resource Manager nicht unterstützt. Übergeben Sie stattdessen ein Objekt für die Tagwerte, Sie müssen jedoch dennoch die Tagnamen angeben, wie unten dargestellt.

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "tagvalues": {
          "type": "object",
          "defaultValue": {
            "dept": "Finance",
            "project": "Test"
          }
        }
      },
      "resources": [
      {
        "apiVersion": "2015-06-15",
        "type": "Microsoft.Storage/storageAccounts",
        "name": "examplestorage",
        "tags": {
          "dept": "[parameters('tagvalues').dept]",
          "project": "[parameters('tagvalues').project]"
        },
        "location": "[resourceGroup().location]",
        "properties": {
          "accountType": "Standard_LRS"
        }
      }]
    }


## Portal

[AZURE.INCLUDE [resource-manager-tag-resource](../includes/resource-manager-tag-resources.md)]

## PowerShell

[AZURE.INCLUDE [resource-manager-tag-resources](../includes/resource-manager-tag-resources-powershell.md)]

## Azure-Befehlszeilenschnittstelle

Tags sind direkt in Ressourcen und Ressourcengruppen vorhanden. Um die vorhandenen Tags anzuzeigen, rufen Sie einfach eine Ressourcengruppe und ihre Ressourcen mit **azure group show** auf.

    azure group show -n tag-demo-group
    
So werden Metadaten für die Ressourcengruppe zurückgegeben, einschließlich aller Tags, die darauf angewendet werden.
    
    info:    Executing command group show
    + Listing resource groups
    + Listing resources for the group
    data:    Id:                  /subscriptions/{guid}/resourceGroups/tag-demo-group
    data:    Name:                tag-demo-group
    data:    Location:            westus
    data:    Provisioning State:  Succeeded
    data:    Tags: Dept=Finance;Environment=Production
    data:    Resources:
    data:
    data:      Id      : /subscriptions/{guid}/resourceGroups/tag-demo-group/providers/Microsoft.Sql/servers/tfsqlserver
    data:      Name    : tfsqlserver
    data:      Type    : servers
    data:      Location: eastus2
    data:      Tags    : Dept=Finance;Environment=Production
    ...

Um nur die Tags für die Ressourcengruppe abzurufen, verwenden Sie ein JSON-Dienstprogramm, z.B. [jq](http://stedolan.github.io/jq/download/).

    azure group show -n tag-demo-group --json | jq ".tags"
    
So werden die Tags für diese Ressourcengruppe zurückgegeben.
    
    {
      "Dept": "Finance",
      "Environment": "Production" 
    }

Mit **azure resource show** zeigen Sie die Tags für eine bestimmte Ressource an.

    azure resource show -g tag-demo-group -n tfsqlserver -r Microsoft.Sql/servers -o 2014-04-01-preview --json | jq ".tags"
    
So werden die Tags für diese Ressource zurückgegeben.
    
    {
      "Dept": "Finance",
      "Environment": "Production"
    }
    
Das folgende Beispiel zeigt das Abrufen aller Ressourcen, die einen Tagnamen und -Wert aufweisen.

    azure resource list --json | jq ".[] | select(.tags.Dept == "Finance") | .name"
    
So werden die Namen von Ressourcen mit diesem Tag zurückgegeben.
    
    "tfsqlserver"
    "tfsqlserver/tfsqldata"

Tags werden als Ganzes aktualisiert. Um ein Tag einer Ressource hinzuzufügen, die bereits über Tags verfügt, rufen Sie alle vorhandenen Tags ab, die Sie behalten möchten. Verwenden Sie **azure group set** zum Festlegen von Tagwerten für eine Ressourcengruppe, und geben Sie alle Tags für die Ressourcengruppe an.

    azure group set -n tag-demo-group -t Dept=Finance;Environment=Production;Project=Upgrade
    
Es wird eine Zusammenfassung der Ressourcengruppe mit den neuen Tags zurückgegeben.
    
    info:    Executing command group set
    ...
    data:    Name:                tag-demo-group
    data:    Location:            westus
    data:    Provisioning State:  Succeeded
    data:    Tags: Dept=Finance;Environment=Production;Project=Upgrade
    ...
    
Sie können die vorhandenen Tags in Ihrem Abonnement mit **azure tag list** auflisten und mit **azure tag create** ein Tag hinzufügen. Um ein Tag aus der Taxonomie für Ihr Abonnement zu entfernen, entfernen Sie das Tag zuerst von allen Ressourcen. Entfernen Sie dann das Tag mit **azure tag delete**.

## REST-API

Das Vorschauportal und PowerShell verwenden im Hintergrund die [Ressourcen-Manager-REST-API](https://msdn.microsoft.com/library/azure/dn848368.aspx). Wenn Sie das Tagging in eine andere Umgebung integrieren müssen, können Sie Tags mit GET für die Ressourcen-ID abrufen und die Tags mit einem PATCH-Aufruf aktualisieren.


## Tags und Abrechnung

Abrechnungsdaten können für unterstützte Dienste mithilfe von Tags gruppiert werden. So können Sie beispielsweise mithilfe von [in den Azure-Ressourcen-Manager integrierten virtuellen Computern](./virtual-machines/virtual-machines-windows-compare-deployment-models.md) Tags definieren und anwenden, um die Abrechnung für virtuelle Computer zu organisieren. Wenn Sie mehrere virtuelle Computer für verschiedene Organisationen betreiben, können Sie die Nutzung mithilfe von Tags nach Kostenstelle gruppieren. Mit Tags können Sie auch Kosten nach Laufzeitumgebung kategorisieren (beispielsweise zur Abrechnung der Nutzung virtueller Computer in der Produktionsumgebung).

Informationen zu Tags können Sie über die [Azure-Ressourcennutzungs- und RateCard-APIs](billing-usage-rate-card-overview.md) oder aus der Nutzungsdatei im CSV-Format abrufen. Sie laden die Nutzungsdatei aus dem [Azure-Kontenportal](https://account.windowsazure.com/) oder dem [EA-Portal](https://ea.azure.com) herunter. Weitere Informationen zum programmgesteuerten Zugriff auf Abrechnungsinformationen finden Sie unter [Gewinnen von Einblicken in den Ressourcenverbrauch unter Microsoft Azure](billing-usage-rate-card-overview.md). Hinweise zu REST-API-Vorgängen finden Sie unter [Azure Billing REST API Reference (Preview)](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) (in englischer Sprache).

Wenn Sie die CSV-Nutzungsdatei für Dienste herunterladen, die die Verwendung von Tags für die Abrechnung unterstützen, sind die Tags in der Spalte **Tags** enthalten. Ausführlichere Informationen finden Sie unter [Informationen zu Ihrer Rechnung für Microsoft Azure](billing-understand-your-bill.md).

![Anzeigen von Tags in der Abrechnung](./media/resource-group-using-tags/billing_csv.png)

## Nächste Schritte

- Sie können mithilfe benutzerdefinierter Richtlinien Einschränkungen und Konventionen für Ihr Abonnement festlegen. Die Richtlinie, die Sie definieren, kann erfordern, dass alle Ressourcen über einen Wert für ein bestimmtes Tag verfügen. Weitere Informationen finden Sie unter [Verwenden von Richtlinien für Ressourcenverwaltung und Zugriffssteuerung](resource-manager-policy.md).
- Eine Einführung zur Verwendung von Azure PowerShell für das Bereitstellen von Ressourcen finden Sie unter [Verwenden von Windows PowerShell mit dem Azure-Ressourcen-Manager](./powershell-azure-resource-manager.md).
- Eine Einführung zur Verwendung der Azure-Befehlszeilenschnittstelle für das Bereitstellen von Ressourcen finden Sie unter [Verwenden der Azure-Befehlszeilenschnittstelle für Mac, Linux und Windows mit der Azure-Ressourcenverwaltung](./xplat-cli-azure-resource-manager.md).
- Eine Einführung zum Verwenden des Portals finden Sie unter [Verwenden des Azure-Portals zum Verwalten Ihrer Azure-Ressourcen](./azure-portal/resource-group-portal.md).

<!---HONumber=AcomDC_0817_2016-->