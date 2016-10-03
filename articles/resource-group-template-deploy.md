<properties
   pageTitle="Bereitstellen von Ressourcen mit PowerShell und Vorlagen | Microsoft Azure"
   description="Verwenden Sie Azure Resource Manager und Azure PowerShell, um Ressourcen in Azure bereitzustellen. Die Ressourcen werden in einer Resource Manager-Vorlage definiert."
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
   ms.date="08/15/2016"
   ms.author="tomfitz"/>

# Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure PowerShell

> [AZURE.SELECTOR]
- [PowerShell](resource-group-template-deploy.md)
- [Azure-Befehlszeilenschnittstelle](resource-group-template-deploy-cli.md)
- [Portal](resource-group-template-deploy-portal.md)
- [REST-API](resource-group-template-deploy-rest.md)

In diesem Thema wird erläutert, wie Ihre Anwendung mit Azure PowerShell und Resource Manager-Vorlagen in Azure bereitgestellt wird.

> [AZURE.TIP] Hilfe zum Beheben eines Fehlers während der Bereitstellung finden Sie unter:
>
> - [Anzeigen von Bereitstellungsvorgängen mit Azure PowerShell](resource-manager-troubleshoot-deployments-powershell.md). Hier erfahren Sie, wie Sie Informationen zum Beheben von Fehlern abrufen.
> - [Beheben von häufigen Fehlern beim Bereitstellen von Ressourcen in Azure mit Azure Resource Manager](resource-manager-common-deployment-errors.md). Hier erfahren Sie, wie Sie häufig auftretende Bereitstellungsfehler beheben.

Ihre Vorlage kann entweder eine lokale Datei oder eine externe Datei sein, die über einen URI verfügbar ist. Wenn sich Ihre Vorlage in einem Speicherkonto befindet, können Sie den Zugriffs auf die Vorlage beschränken und ein SAS-Token (Shared Access Signature) während der Bereitstellung angeben.

## Schnelle Schritte zur Bereitstellung

Dieser Artikel beschreibt alle verschiedenen Optionen, die Ihnen während der Bereitstellung zur Verfügung stehen. Allerdings benötigen Sie häufig nur zwei einfache Befehle. Verwenden Sie die folgenden Befehle, um schnell mit der Bereitstellung zu beginnen:

    New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West US"
    New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterFile <PathToParameterFile>

Weitere Informationen zu Optionen für die Bereitstellung, die für Ihr Szenario besser geeignet sein könnten, erhalten Sie im weiteren Verlauf dieses Artikels.

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## Bereitstellen mit PowerShell

1. Melden Sie sich beim Azure-Konto an.

        Add-AzureRmAccount

     Es wird eine Zusammenfassung Ihres Kontos zurückgegeben.

        Environment : AzureCloud
        Account     : someone@example.com
        ...

2. Wenn Sie über mehrere Abonnements verfügen, geben Sie die Abonnement-ID ein, die Sie mit dem Befehl **Set-AzureRmContext** für die Bereitstellung verwenden möchten.

        Set-AzureRmContext -SubscriptionID <YourSubscriptionId>

3. Wenn Sie eine neue Vorlage bereitstellen, müssen Sie in der Regel zunächst eine Ressourcengruppe erstellen, die die Ressourcen enthalten soll. Wenn es bereits eine Ressourcengruppe gibt, in der die Bereitstellung erfolgen soll, können Sie diesen Schritt überspringen und diese Ressourcengruppe nutzen.

     Geben Sie zum Erstellen einer Ressourcengruppe einen Namen und Speicherort für diese an. Sie müssen einen Standort für die Ressourcengruppe angeben, da diese Metadaten zu den Ressourcen speichert. Aus Compliance-Gründen sollten Sie angeben, wo diese Metadaten gespeichert werden. Im Allgemeinen wird die Angabe eines Standorts empfohlen, an dem sich der Großteil Ihrer Ressourcen befindet. Durch die Verwendung des gleichen Standorts können Sie die Vorlage vereinfachen.

        New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West US"
   
     Es wird eine Zusammenfassung der neuen Ressourcengruppe zurückgegeben.
   
        ResourceGroupName : ExampleResourceGroup
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
             Actions  NotActions
             =======  ==========
             *
        ResourceId        : /subscriptions/######/resourceGroups/ExampleResourceGroup

4. Vor dem Ausführen der Bereitstellung können Sie Ihre Bereitstellungseinstellungen überprüfen. Das Cmdlet **Test-AzureRmResourceGroupDeployment** ermöglicht Ihnen, Probleme zu finden, bevor tatsächlich Ressourcen erstellt werden. Im folgenden Beispiel wird veranschaulicht, wie eine Bereitstellung überprüft wird.

        Test-AzureRmResourceGroupDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate>

5. Führen Sie zum Bereitstellen in der Ressourcengruppe den Befehl **New-AzureRmResourceGroupDeployment** aus, und geben Sie die erforderlichen Parameter ein. Die Parameter enthalten den Namen der Bereitstellung, den Namen der Ressourcengruppe, den Pfad oder die URL der erstellten Vorlage und alle anderen für Ihr Szenario erforderlichen Parameter. Wenn der Parameter **Mode** nicht angegeben wurde, wird der Standardwert **Incremental** verwendet. Legen Sie zum Ausführen einer vollständigen Bereitstellung **Mode** auf **Complete** fest. Seien Sie bei Wahl des Modus „Complete“ vorsichtig, da Sie versehentlich Ressourcen löschen können, die nicht in Ihrer Vorlage enthalten sind.

     Verwenden Sie zum Bereitstellen einer lokalen Vorlage den **TemplateFile**-Parameter:

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate>

     Verwenden Sie zum Bereitstellen einer externen Vorlage den **TemplateUri**-Parameter:

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri <LinkToTemplate>
   
     Sie haben die folgenden Möglichkeiten zum Angeben der Parameterwerte:
   
     1. Verwenden Sie Inlineparameter.

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -myParameterName "parameterValue"

     2. Verwenden Sie ein Parameterobjekt.

            $parameters = @{"<ParameterName>"="<Parameter Value>"}
            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterObject $parameters

     3. Verwenden Sie eine lokale Parameterdatei. Informationen über die Vorlagendatei finden Sie unter [Parameterdatei](#parameter-file).

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterFile <PathToParameterFile>

     4. Verwenden Sie eine externe Parameterdatei. Informationen über die Vorlagendatei finden Sie unter [Parameterdatei](#parameter-file).

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri <LinkToTemplate> -TemplateParameterUri <LinkToParameterFile>

        Bei Verwendung einer externen Parameterdatei können Sie keine anderen Werte (weder inline noch aus einer lokalen Datei) übergeben. Weitere Informationen finden Sie im Thema zur [Parameterrangfolge](#parameter-precendence).

     Nachdem die Ressourcen bereitgestellt wurden, wird eine Zusammenfassung der Bereitstellung angezeigt.

        DeploymentName    : ExampleDeployment
        ResourceGroupName : ExampleResourceGroup
        ProvisioningState : Succeeded
        Timestamp         : 4/14/2015 7:00:27 PM
        Mode              : Incremental
        ...

     Sollte Ihre Vorlage einen Parameter enthalten, der den gleichen Namen besitzt wie einer der Parameter des PowerShell-Befehls, werden Sie zur Eingabe eines Werts für diesen Parameter aufgefordert. Der Parameter aus der Vorlage wird mit dem Postfix **FromTemplate** versehen. Beispiel: Ein Parameter namens **ResourceGroupName** in Ihrer Vorlage verursacht einen Konflikt mit dem Parameter **ResourceGroupName** im [New-AzureRmResourceGroupDeployment](https://msdn.microsoft.com/library/azure/mt679003.aspx)-Cmdlet. Sie werden zur Eingabe eines Werts für **ResourceGroupNameFromTemplate** aufgefordert. Im Allgemeinen sollten Sie diese Verwirrung vermeiden, indem Sie Parametern nicht dieselben Namen wie Parametern für Bereitstellungsvorgänge geben.

6. Wenn Sie zusätzliche Informationen zur Bereitstellung protokollieren möchten, die Ihnen möglicherweise bei der Behebung von Bereitstellungsfehlern helfen können, verwenden Sie den Parameter **DeploymentDebugLogLevel**. Sie können angeben, dass der Anforderungsinhalt, der Antwortinhalt oder beide beim Bereitstellungsvorgang protokolliert werden.

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -DeploymentDebugLogLevel All -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate>
        
     Weitere Informationen zur Verwendung dieses Debuginhalts zur Behebung von Problemen in der Bereitstellung finden Sie unter [Anzeigen von Bereitstellungsvorgängen mit Azure PowerShell](resource-manager-troubleshoot-deployments-powershell.md).

## Bereitstellen einer Vorlage aus Speicher mithilfe eines SAS-Tokens

Sie können Ihre Vorlagen einem Speicherkonto hinzufügen und sie während der Bereitstellung mit einem SAS-Token verknüpfen.

> [AZURE.IMPORTANT] Wenn Sie die nachstehenden Schritte befolgen, hat nur der Kontobesitzer Zugriff auf das Blob mit der Vorlage. Wenn Sie jedoch ein SAS-Token für das Blob erstellen, können andere Benutzer über diesen URI auf das Blob zugreifen. Wenn ein anderer Benutzer den URI abfängt, hat dieser Benutzer Zugriff auf die Vorlage. Ein SAS-Token ist eine gute Möglichkeit zum Einschränken des Zugriffs auf Ihre Vorlagen. Sie sollten allerdings Kennwörter auf keinen Fall direkt in die Vorlage einschließen.

### Hinzufügen einer privaten Vorlage zum Speicherkonto

Führen Sie die folgenden Schritte aus, um ein Speicherkonto für Vorlagen einzurichten:

1. Erstellen Sie eine Ressourcengruppe.

        New-AzureRmResourceGroup -Name ManageGroup -Location "West US"

2. Erstellen Sie ein Speicherkonto. Der Name des Speicherkontos muss innerhalb von Azure eindeutig sein. Geben Sie deshalb Ihren eigenen Namen für das Konto an.

        New-AzureRmStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates -Type Standard_LRS -Location "West US"

3. Legen Sie das aktuelle Speicherkonto fest.

        Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates

4. Erstellen Sie einen Container. Die Berechtigung ist auf **Off** festgelegt, was bedeutet, dass nur der Besitzer Zugriff auf den Container hat.

        New-AzureStorageContainer -Name templates -Permission Off
        
5. Fügen Sie Ihre Vorlage dem Container hinzu.

        Set-AzureStorageBlobContent -Container templates -File c:\Azure\Templates\azuredeploy.json
        
### Bereitstellen eines SAS-Tokens während der Bereitstellung

Rufen Sie zum Bereitstellen einer privaten Vorlage in einem Speicherkonto ein SAS-Token ab, und fügen Sie es dem URI für die Vorlage hinzu.

1. Wenn Sie das aktuelle Speicherkonto geändert haben, legen Sie das aktuelle Speicherkonto auf dasjenige mit Ihren Vorlagen fest.

        Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates

2. Erstellen Sie ein SAS-Token mit Leseberechtigungen und Ablaufzeit, um den Zugriff einzuschränken. Rufen Sie den vollständigen URI der Vorlage einschließlich SAS-Token ab.

        $templateuri = New-AzureStorageBlobSASToken -Container templates -Blob azuredeploy.json -Permission r -ExpiryTime (Get-Date).AddHours(2.0) -FullUri

3. Stellen Sie die Vorlage durch Angabe des URI bereit, der das SAS-Token enthält.

        New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri $templateuri

Ein Beispiel der Verwendung eines SAS-Tokens mit verknüpften Vorlagen finden Sie unter [Verwenden von verknüpften Vorlagen mit Azure Resource Manager](resource-group-linked-templates.md).

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## Parameterrangfolge

Sie können Inlineparameter und eine lokale Parameterdatei im selben Bereitstellungsvorgang verwenden. Sie können beispielsweise einige Werte in der lokalen Parameterdatei angeben und weitere Werte während der Bereitstellung inline hinzufügen. Wenn Sie Werte für einen Parameter sowohl in der lokalen Parameterdatei als auch inline bereitstellen, haben die Inlinewerte Vorrang.

Inlineparameter können jedoch nicht mit einer externen Parameterdatei verwendet werden. Wenn Sie eine Parameterdatei im Parameter **TemplateParameterUri** angeben, werden alle Inlineparameter ignoriert. Sie müssen alle Parameterwerte in der externen Datei bereitstellen. Enthält Ihre Vorlage einen vertraulichen Wert, der nicht in die Parameterdatei aufgenommen werden kann, fügen Sie diesen Wert zu einem Schlüsseltresor hinzu, und verweisen Sie in der externen Parameterdatei auf den Schlüsseltresor. Alternativ können Sie alle Parameterwerte dynamisch inline bereitstellen.

Weitere Informationen zum Verwenden eines KeyVault-Verweises zum Übergeben sicherer Werte finden Sie unter [Übergeben sicherer Werte während der Bereitstellung](resource-manager-keyvault-parameter.md).

## Nächste Schritte
- Ein Beispiel für die Bereitstellung von Ressourcen über die .NET-Clientbibliothek finden Sie unter [Bereitstellen von Ressourcen mithilfe von .NET-Bibliotheken und einer Vorlage](virtual-machines/virtual-machines-windows-csharp-template.md).
- Informationen zum Definieren von Parametern in der Vorlage finden Sie unter [Erstellen von Vorlagen](resource-group-authoring-templates.md#parameters).
- Informationen zum Bereitstellen der Lösung in andere Umgebungen finden Sie unter [Entwicklungs- und Testumgebungen in Microsoft Azure](solution-dev-test-environments.md).

<!---HONumber=AcomDC_0921_2016-->