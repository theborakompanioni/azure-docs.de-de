<properties 
	pageTitle="Verknüpfen von Ressourcen in Azure Resource Manager | Microsoft Azure" 
	description="Erstellen Sie in Azure Resource Manager eine Verknüpfung zwischen verwandten Ressourcen in verschiedenen Ressourcengruppen." 
	services="azure-resource-manager" 
	documentationCenter="" 
	authors="tfitzmac" 
	manager="timlt" 
	editor="tysonn"/>

<tags 
	ms.service="azure-resource-manager" 
	ms.workload="multiple" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="05/16/2016" 
	ms.author="tomfitz"/>

# Verknüpfen von Ressourcen im Azure-Ressourcen-Manager

Während der Bereitstellung können Sie eine Ressource als von einer anderen Ressource abhängig markieren, aber dieser Lebenszyklus endet bei der Bereitstellung. Nach der Bereitstellung gibt es keine identifizierte Beziehung zwischen abhängigen Ressourcen. Stattdessen bietet Resource Manager ein neues Feature namens Ressourcenverknüpfungen, um dauerhafte Beziehungen zwischen Ressourcen herzustellen.

Mithilfe von Ressourcenverknüpfungen können Sie Beziehungen ressourcengruppenübergreifend dokumentieren. Beispielsweise ist es üblich, dass eine Datenbank mit einem eigenen Lebenszyklus sich in einer Ressourcengruppe und eine App mit einem anderen Lebenszyklus in einer anderen Ressourcengruppe befindet. Die App verbindet sich mit der Datenbank, weshalb Sie eine Verknüpfung zwischen der App und der Datenbank markieren sollten.

Alle verknüpften Ressourcen müssen zum selben Abonnement gehören. Jede Ressource kann mit 50 anderen Ressourcen verknüpft werden. Die einzige Möglichkeit, verwandte Ressourcen abzufragen, ist über die REST-API. Wenn verknüpfte Ressourcen gelöscht oder verschoben werden, muss der Besitzer des Links den verbleibenden Link bereinigen. Sie werden **nicht** gewarnt, wenn Sie eine Ressource löschen, die mit anderen Ressourcen verknüpft ist.

## Verknüpfen in Vorlagen

Um eine Verknüpfung in einer Vorlage zu definieren, schließen Sie einen Ressourcentyp ein, der den Namespace des Ressourcenanbieters kombiniert, und geben die Quellressource mit **/providers/links** ein. Der Name muss den Namen der Quellressource enthalten. Sie geben die Ressourcen-ID der Zielressource an. Im folgenden Beispiel wird eine Verknüpfung zwischen einer Website und einem Speicherkonto eingerichtet.

    {
      "type": "Microsoft.Web/sites/providers/links",
      "apiVersion": "2015-01-01",
      "name": "[concat(variables('siteName'),'/Microsoft.Resources/SiteToStorage')]",
      "dependsOn": [ "[variables('siteName')]" ],
      "properties": {
        "targetId": "[resourceId('Microsoft.Storage/storageAccounts','storagecontoso')]",
        "notes": "This web site uses the storage account to store user information."
      }
    }


Eine vollständige Beschreibung der Vorlage finden Sie unter [Ressourcenverknüpfungen – Vorlagenschema](resource-manager-template-links.md).

## Verknüpfen mit der REST-API

Um einen Link zwischen bereitgestellten Ressourcen zu definieren, führen Sie die folgenden Schritte aus:

    PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/{provider-namespace}/{resource-type}/{resource-name}/providers/Microsoft.Resources/links/{link-name}?api-version={api-version}

Ersetzen Sie {subscription-id} durch Ihre Abonnement-ID. Ersetzen Sie {resource-group}, {provider-namespace}, {resource-type} und {resource-name} durch die Werte, die die erste Ressource im Link identifizieren. Ersetzen Sie {link-name} durch den Namen des zu erstellenden Links. Verwenden Sie "api-version" 2015-01-01.

Schließen Sie in die Anforderung ein Objekt ein, das die zweite Ressource im Link definiert:

    {
        "name": "{new-link-name}",
        "properties": {
            "targetId": "subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/{provider-namespace}/{resource-type}/{resource-name}/",
            "notes": "{link-description}",
        }
    }

Das properties-Element enthält den Bezeichner für die zweite Ressource.

Sie können Verknüpfungen in Ihrem Abonnement wie folgt abfragen:

    https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Resources/links?api-version={api-version}

Weitere Beispiele, wie z. B. das Abrufen von Informationen zu Links, finden Sie unter [Verknüpfte Ressourcen](https://msdn.microsoft.com/library/azure/mt238499.aspx).

## Nächste Schritte

- Sie können Ihre Ressourcen auch mithilfe von Tags organisieren. Informationen zum Markieren von Ressourcen mit Tags finden Sie unter [Verwenden von Tags zum Organisieren von Azure-Ressourcen](resource-group-using-tags.md).
- Eine Beschreibung der Vorgehensweise zum Erstellen von Vorlagen und Definieren der bereitzustellenden Ressourcen finden Sie unter [Erstellen von Azure-Ressourcen-Manager-Vorlagen](resource-group-authoring-templates.md).

<!---HONumber=AcomDC_0518_2016-->