<properties
   pageTitle="Bereitstellen von Ressourcen mit REST-API und Vorlagen | Microsoft Azure"
   description="Verwenden Sie Azure Resource Manager und Resource Manager-REST-API, um Ressourcen in Azure bereitzustellen. Die Ressourcen werden in einer Resource Manager-Vorlage definiert."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/30/2016"
   ms.author="tomfitz"/>

# Bereitstellen von Ressourcen mit Resource Manager-Vorlagen und Resource Manager-REST-API

> [AZURE.SELECTOR]
- [PowerShell](resource-group-template-deploy.md)
- [Azure-Befehlszeilenschnittstelle](resource-group-template-deploy-cli.md)
- [Portal](resource-group-template-deploy-portal.md)
- [REST-API](resource-group-template-deploy-rest.md)
- [Java](https://azure.microsoft.com/documentation/samples/resources-java-deploy-using-arm-template/)
- [Knoten](https://azure.microsoft.com/documentation/samples/resource-manager-node-template-deployment/)
- [Python](https://azure.microsoft.com/documentation/samples/resource-manager-python-template-deployment/)
- [Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-template-deployment/)

In diesem Artikel wird erläutert, wie Ihre Ressourcen mithilfe der Resource Manager-REST-API und von Azure Resource Manager-Vorlagen in Azure bereitgestellt werden.

> [AZURE.TIP] Hilfe zum Beheben eines Fehlers während der Bereitstellung finden Sie unter:
>
> - [Anzeigen von Bereitstellungsvorgängen mit der REST-API](resource-manager-troubleshoot-deployments-rest.md). Hier erfahren Sie, wie Sie Informationen zum Beheben von Fehlern abrufen.
> - [Beheben von häufigen Fehlern beim Bereitstellen von Ressourcen in Azure mit Azure Resource Manager](resource-manager-common-deployment-errors.md). Hier erfahren Sie, wie Sie häufig auftretende Bereitstellungsfehler beheben.

Ihre Vorlage kann entweder eine lokale Datei oder eine externe Datei sein, die über einen URI verfügbar ist. Wenn sich Ihre Vorlage in einem Speicherkonto befindet, können Sie den Zugriffs auf die Vorlage beschränken und ein SAS-Token (Shared Access Signature) während der Bereitstellung angeben.

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## Bereitstellen mit der REST-API
1. Legen Sie [allgemeine Parameter und Header](https://msdn.microsoft.com/library/azure/8d088ecc-26eb-42e9-8acc-fe929ed33563#bk_common) fest, einschließlich Authentifizierungstoken.
2. Erstellen Sie eine neue Ressourcengruppe, wenn noch keine vorhanden ist. Geben Sie Ihre Abonnement-ID, den Namen der neuen Ressourcengruppe und den Speicherort für Ihre Lösung an. Weitere Informationen finden Sie unter [Erstellen einer Ressourcengruppe](https://msdn.microsoft.com/library/azure/dn790525.aspx).

        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2015-01-01
          <common headers>
          {
            "location": "West US",
            "tags": {
               "tagname1": "tagvalue1"
            }
          }
   
3. Überprüfen Sie die Bereitstellung vor der Implementierung, indem Sie den Vorgang unter [Überprüfen einer Vorlagenbereitstellung](https://msdn.microsoft.com/library/azure/dn790547.aspx) ausführen. Geben Sie die Parameter beim Testen der Bereitstellung genauso an wie beim Ausführen der Bereitstellung (wie im nächsten Schritt zu sehen).

3. Erstellen Sie eine neue Bereitstellung. Geben Sie Ihre Abonnement-ID, den Namen der bereitzustellenden Ressourcengruppe, den Namen der Bereitstellung und einen Link zu Ihrer Vorlage an. Informationen über die Vorlagendatei finden Sie unter [Parameterdatei](./#parameter-file). Weitere Informationen über die REST-API zum Erstellen einer Ressourcengruppe finden Sie unter [Erstellen einer Vorlagenbereitstellung](https://msdn.microsoft.com/library/azure/dn790564.aspx). Beachten Sie, dass **Mode** auf **Incremental** festgelegt ist. Legen Sie zum Ausführen einer vollständigen Bereitstellung **Mode** auf **Complete** fest. Seien Sie bei Wahl des Modus „Complete“ vorsichtig, da Sie versehentlich Ressourcen löschen können, die nicht in Ihrer Vorlage enthalten sind.
    
        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
          <common headers>
          {
            "properties": {
              "templateLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
                "contentVersion": "1.0.0.0",
              },
              "mode": "Incremental",
              "parametersLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
                "contentVersion": "1.0.0.0",
              }
            }
          }
   
      Wenn Sie den Antwortinhalt und/oder den Anforderungsinhalt protokollieren möchten, fügen Sie **debugSetting** in die Anforderung ein.

        "debugSetting": {
          "detailLevel": "requestContent, responseContent"
        }

      Sie können das Speicherkonto so einrichten, das ein SAS-Token (Shared Access Signature) verwendet wird. Weitere Informationen finden Sie unter [Delegieren des Zugriffs mit einer SAS (Shared Access Signature)](https://msdn.microsoft.com/library/ee395415.aspx).

4. Rufen Sie den Status der Vorlagenbereitstellung ab. Weitere Informationen finden Sie unter [Abrufen von Informationen zu einer Vorlagenbereitstellung](https://msdn.microsoft.com/library/azure/dn790565.aspx).

          GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
           <common headers>

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## Nächste Schritte
- Ein Beispiel für die Bereitstellung von Ressourcen über die .NET-Clientbibliothek finden Sie unter [Bereitstellen von Ressourcen mithilfe von .NET-Bibliotheken und einer Vorlage](virtual-machines/virtual-machines-windows-csharp-template.md).
- Informationen zum Definieren von Parametern in der Vorlage finden Sie unter [Erstellen von Vorlagen](resource-group-authoring-templates.md#parameters).
- Informationen zum Bereitstellen der Lösung in andere Umgebungen finden Sie unter [Entwicklungs- und Testumgebungen in Microsoft Azure](solution-dev-test-environments.md).
- Weitere Informationen zum Verwenden eines KeyVault-Verweises zum Übergeben sicherer Werte finden Sie unter [Übergeben sicherer Werte während der Bereitstellung](resource-manager-keyvault-parameter.md).

<!---HONumber=AcomDC_0706_2016-->