---
title: Erstellen einer Azure IoT Hub-Instanz mithilfe einer Vorlage (PowerShell) | Microsoft Docs
description: Erfahren Sie, wie Sie mithilfe einer Azure Resource Manager-Vorlage in PowerShell eine IoT Hub-Instanz erstellen.
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 7eade855-c289-4ffb-b5ef-02be8c5f670f
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.translationtype: HT
ms.sourcegitcommit: f9003c65d1818952c6a019f81080d595791f63bf
ms.openlocfilehash: f83fac6cffc9e58582417324a4348ca3b6220f0c
ms.contentlocale: de-de
ms.lasthandoff: 08/09/2017

---
# <a name="create-an-iot-hub-using-azure-resource-manager-template-powershell"></a>Erstellen einer IoT Hub-Instanz mithilfe einer Azure Resource Manager-Vorlage (PowerShell)

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Sie können den Azure-Ressourcen-Manager verwenden, um Azure IoT Hubs programmgesteuert zu erstellen und zu verwalten. In diesem Tutorial erfahren Sie, wie Sie mithilfe einer Azure Resource Manager-Vorlage und PowerShell einen IoT Hub erstellen.

> [!NOTE]
> Azure bietet zwei Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Azure Resource Manager](../azure-resource-manager/resource-manager-deployment-model.md) und klassisch. Dieser Artikel behandelt die Verwendung des Azure Resource Manager-Bereitstellungsmodells.

Für dieses Tutorial benötigen Sie Folgendes:

* Ein aktives Azure-Konto. <br/>Wenn Sie kein Konto besitzen, können Sie in nur wenigen Minuten ein [kostenloses Konto][lnk-free-trial] erstellen.
* [Azure PowerShell 1.0][lnk-powershell-install] oder höher.

> [!TIP]
> Der Artikel [Verwenden von Azure PowerShell mit dem Azure Resource Manager][lnk-powershell-arm] enthält weitere Informationen zum Verwenden von PowerShell und Azure Resource Manager-Vorlagen zum Erstellen von Azure-Ressourcen.

## <a name="connect-to-your-azure-subscription"></a>Verbinden mit Ihrem Azure-Abonnement

Geben Sie in einer PowerShell-Befehlszeile den folgenden Befehl zur Anmeldung bei Ihrem Azure-Abonnement ein:

```powershell
Login-AzureRmAccount
```

Wenn Sie über mehrere Azure-Abonnements verfügen, erhalten Sie durch die Anmeldung bei Azure Zugriff auf alle Azure-Abonnements, die mit Ihren Anmeldeinformationen verknüpft sind. Führen Sie den folgenden Befehl aus, um eine Liste der Azure-Abonnements anzuzeigen, die Sie verwenden können:

```powershell
Get-AzureRMSubscription
```

Führen Sie den folgenden Befehl aus, um das Abonnement auszuwählen, das Sie zum Ausführen der Befehle zum Erstellen Ihres IoT Hubs verwenden möchten. Sie können entweder den Abonnementnamen oder die ID aus der Ausgabe des vorherigen Befehls verwenden:

```powershell
Select-AzureRMSubscription `
    -SubscriptionName "{your subscription name}"
```

Mit den folgenden Befehlen können Sie ermitteln, wo Sie einen IoT-Hub und die derzeit unterstützten API-Versionen bereitstellen können:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).Locations
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).ApiVersions
```

Erstellen Sie mit dem folgenden Befehl an einem Speicherort, der IoT Hub unterstützt, eine Ressourcengruppe, die Ihren IoT-Hub enthalten soll. Dieses Beispiel erstellt eine Ressourcengruppe namens **MyIoTRG1**:

```powershell
New-AzureRmResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="submit-a-template-to-create-an-iot-hub"></a>Einsenden einer Vorlage zum Erstellen eines IoT Hubs

Verwenden Sie eine JSON-Vorlage, um einen IoT Hub in der Ressourcengruppe zu erstellen. Sie können auch eine Azure Resource Manager-Vorlage verwenden, um Änderungen an einem vorhandenen IoT Hub vorzunehmen.

1. Erstellen Sie mithilfe eines Texteditors eine Azure Resource Manager-Vorlage namens **template.json** mit der folgenden Ressourcendefinition, um einen neuen Standard-IoT Hub zu erstellen. In diesem Beispiel wird der IoT Hub in der Region **USA, Osten** hinzugefügt, es werden zwei Consumergruppen (**cg1** und **cg2**) auf dem Event Hub-kompatiblen Endpunkt erstellt, und es wird die API-Version **2016-02-03** verwendet. Diese Vorlage erwartet auch, dass Sie den IoT Hub-Namen als Parameter **hubName**übergeben. Die aktuelle Liste der Standorte, die IoT Hub unterstützen, finden Sie unter [Azure-Status][lnk-status].

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": {
          "type": "string"
        }
      },
      "resources": [
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs",
        "name": "[parameters('hubName')]",
        "location": "East US",
        "sku": {
          "name": "S1",
          "tier": "Standard",
          "capacity": 1
        },
        "properties": {
          "location": "East US"
        }
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg1')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg2')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      }
      ],
      "outputs": {
        "hubKeys": {
          "value": "[listKeys(resourceId('Microsoft.Devices/IotHubs', parameters('hubName')), '2016-02-03')]",
          "type": "object"
        }
      }
    }
    ```

2. Speichern Sie die Azure Resource Manager-Vorlagendatei auf Ihrem lokalen Computer. In diesem Beispiel wird davon ausgegangen, dass Sie sie im Ordner **c:\templates** speichern.

3. Führen Sie den folgenden Befehl zum Bereitstellen Ihres neuen IoT-Hubs aus, der den Namen des IoT-Hubs als Parameter übergibt. In diesem Beispiel lautet der Name des IoT-Hubs `abcmyiothub`. Der Name des IoT-Hubs muss global eindeutig sein:

    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName MyIoTRG1 -TemplateFile C:\templates\template.json -hubName abcmyiothub
    ```
  [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

4. Die Ausgabe zeigt die Schlüssel für den IoT-Hub an, den Sie erstellt haben.

5. Um zu überprüfen, ob Ihre Anwendung den neuen IoT-Hub hinzugefügt hat, besuchen Sie das [Azure-Portal][lnk-azure-portal], und zeigen Sie die Liste der Ressourcen an. Verwenden Sie alternativ das PowerShell-Cmdlet **Get-AzureRmResource**.

> [!NOTE]
> Diese Beispielanwendung fügt einen für Sie kostenpflichtigen S1-Standard-IoT Hub hinzu. Sie können den IoT-Hub nach Abschluss des Beispiels über das [Azure-Portal][lnk-azure-portal] oder mithilfe des PowerShell-Cmdlets **Remove-AzureRmResource** löschen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun einen IoT Hub mithilfe einer Azure Resource Manager-Vorlage mit PowerShell bereitgestellt haben, möchten Sie vielleicht mehr wissen:

* Informieren Sie sich über die Funktionen der [IoT Hub-Ressourcenanbieter-REST-API][lnk-rest-api].
* Weitere Informationen zu den Funktionen des Azure Resource Manager finden Sie unter [Übersicht über Azure Resource Manager][lnk-azure-rm-overview].

Weitere Informationen zum Entwickeln für IoT Hub finden Sie in folgenden Artikeln:

* [Einführung in das C SDK][lnk-c-sdk]
* [Azure IoT SDKs][lnk-sdks]

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [Simulieren eines Geräts mit Azure IoT Edge][lnk-iotedge]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-powershell-install]: /powershell/azure/install-azurerm-ps
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md
[lnk-powershell-arm]: ../azure-resource-manager/powershell-azure-resource-manager.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md

