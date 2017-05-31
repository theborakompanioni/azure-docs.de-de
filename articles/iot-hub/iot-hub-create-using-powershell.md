---
title: Erstellen einer Azure IoT Hub-Instanz mithilfe eines PowerShell-Cmdlets | Microsoft Docs
description: Verwenden eines PowerShell-Cmdlets zum Erstellen einer IoT Hub-Instanz
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/04/2017
ms.author: dobett
ms.translationtype: Human Translation
ms.sourcegitcommit: e7da3c6d4cfad588e8cc6850143112989ff3e481
ms.openlocfilehash: 8271c10cceb7a98879b06704b65a716cd9ac6822
ms.contentlocale: de-de
ms.lasthandoff: 05/16/2017


---
# <a name="create-an-iot-hub-using-the-new-azurermiothub-cmdlet"></a>Erstellen einer IoT Hub-Instanz mithilfe des Cmdlets New-AzureRmIotHub

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Einführung

Mit Azure PowerShell-Cmdlets können Sie Azure IoT Hubs erstellen und verwalten. In diesem Tutorial erfahren Sie, wie Sie eine IoT Hub-Instanz mit PowerShell erstellen.

> [!NOTE]
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Azure Resource Manager-Bereitstellung und klassische Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md).  Dieser Artikel behandelt die Verwendung des Azure Resource Manager-Bereitstellungsmodells.

Für dieses Tutorial benötigen Sie Folgendes:

* Ein aktives Azure-Konto. <br/>Wenn Sie kein Konto besitzen, können Sie in nur wenigen Minuten ein [kostenloses Konto][lnk-free-trial] erstellen.
* [Azure PowerShell-Cmdlets][lnk-powershell-install].

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

## <a name="create-resource-group"></a>Ressourcengruppe erstellen

Für das Bereitstellen einer IoT Hub-Instanz benötigen Sie eine Ressourcengruppe. Sie können eine vorhandene Ressourcengruppe verwenden oder eine neue erstellen.

Mit dem folgenden Befehl können Sie die Standorte ermitteln, an denen Sie eine IoT Hub-Instanz bereitstellen können:

```powershell
((Get-AzureRmResourceProvider `
  -ProviderNamespace Microsoft.Devices).ResourceTypes `
  | Where-Object ResourceTypeName -eq IoTHubs).Locations
```

Mit dem folgenden Befehl können Sie eine Ressourcengruppe für die IoT Hub-Instanz an einem der unterstützten Standorte für IoT Hub erstellen. Mit diesem Beispiel wird die Ressourcengruppe **MyIoTRG1** in der Region **USA, Osten** erstellt:

```powershell
New-AzureRmResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs

Führen Sie den folgenden Befehl aus, um eine IoT Hub-Instanz in der Ressourcengruppe zu erstellen, die Sie im vorherigen Schritt erstellt haben. Mit diesem Beispiel wird ein **S1**-Hub mit dem Namen **MyTestIoTHub** in der Region **USA, Osten** erstellt:

```powershell
New-AzureRmIotHub `
    -ResourceGroupName MyIoTRG1 `
    -Name MyTestIoTHub `
    -SkuName S1 -Units 1 `
    -Location "East US"
```

> [!NOTE]
> Der Name der IoT Hub-Instanz muss eindeutig sein.

Mit dem folgenden Befehl können Sie alle IoT Hubs in Ihrem Abonnement aufführen:

```powershell
Get-AzureRmIotHub
```

Mit dem vorherigen Beispiel wird eine für Sie kostenpflichtige S1-Standard-IoT Hub-Instanz hinzugefügt. Sie können die IoT Hub-Instanz mit dem folgenden Befehl löschen:

```powershell
Remove-AzureRmIotHub `
    -ResourceGroupName MyIoTRG1 `
    -Name MyTestIoTHub
```

Alternativ können Sie eine Ressourcengruppe und alle darin enthaltenen Ressourcen mit dem folgenden Befehl entfernen:

```powershell
Remove-AzureRmResourceGroup -Name MyIoTRG1
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun eine IoT Hub-Instanz mit einem PowerShell-Cmdlet bereitgestellt haben, möchten Sie vielleicht mehr wissen:

* Testen Sie andere [PowerShell-Cmdlets zur Verwendung mit Ihrer IoT Hub-Instanz][lnk-iothub-cmdlets].
* Informieren Sie sich über die Funktionen der [IoT Hub-Ressourcenanbieter-REST-API][lnk-rest-api].

Weitere Informationen zum Entwickeln für IoT Hub finden Sie in folgenden Artikeln:

* [Einführung in das C SDK][lnk-c-sdk]
* [Azure IoT SDKs][lnk-sdks]

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [Simulieren eines Geräts mit IoT Edge][lnk-iotedge]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-powershell-install]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[lnk-iothub-cmdlets]: https://docs.microsoft.com/powershell/module/azurerm.iothub/
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md

