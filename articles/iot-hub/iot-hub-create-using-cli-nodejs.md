---
title: Erstellen eines IoT Hubs mit der Azure-Befehlszeilenschnittstelle (azure.js) | Microsoft Docs
description: "Erstellen eines Azure IoT Hubs mithilfe der plattformübergreifenden Azure-Befehlszeilenschnittstelle (azure.js)."
services: iot-hub
documentationcenter: .net
author: BeatriceOltean
manager: timlt
editor: 
ms.assetid: 46a17831-650c-41d9-b228-445c5bb423d3
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/04/2017
ms.author: boltean
translationtype: Human Translation
ms.sourcegitcommit: 5ea7095e12b6194556d3cd0baa43ccfed1e087ee
ms.openlocfilehash: 1be7dc038f1bc864e7c6461b616449d75ecda439
ms.lasthandoff: 02/27/2017


---
# <a name="create-an-iot-hub-using-the-azure-cli"></a>Erstellen eines IoT Hubs mit der Azure-Befehlszeilenschnittstelle
[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Einführung
Sie können die Azure-Befehlszeilenschnittstelle (azure.js) verwenden, um Azure IoT Hubs programmgesteuert zu erstellen und zu verwalten. In diesem Artikel wird beschrieben, wie Sie mithilfe der Azure-Befehlszeilenschnittstelle (azure.js) einen IoT Hub erstellen.

Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch:

* Azure-Befehlszeilenschnittstelle (azure.js): Die Befehlszeilenschnittstelle für das klassische und das Resource Manager-Bereitstellungsmodell (wie in diesem Artikel beschrieben)
* [Azure CLI 2.0 (az.py):](iot-hub-create-using-cli.md) die Befehlszeilenschnittstelle der nächsten Generation für das Resource Manager-Bereitstellungsmodell

Für dieses Tutorial benötigen Sie Folgendes:

* Ein aktives Azure-Konto. Wenn Sie nicht über ein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto][lnk-free-trial] erstellen.
* [Azure-CLI 0.10.4][lnk-CLI-install] oder höher. Wenn Sie die Azure-Befehlszeilenschnittstelle bereits installiert haben, können Sie die aktuelle Version mit dem folgenden Befehl an der Eingabeaufforderung überprüfen:
  ```
    azure --version
  ```

> [!NOTE]
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Azure Resource Manager-Bereitstellung und klassische Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md). Die Azure-Befehlszeilenschnittstelle muss im Azure Resource Manager-Modus ausgeführt werden:
> 
> ```
> azure config mode arm
> ```
> 
> 

## <a name="set-your-azure-account-and-subscription"></a>Festlegen Ihres Azure-Kontos und -Abonnements
1. Geben Sie an der Eingabeaufforderung den folgenden Befehl ein, um sich anzumelden:
   
   ```
    azure login
   ```
   Verwenden Sie den vorgeschlagenen Webbrowser und Code zum Authentifizieren.
2. Wenn Sie über mehrere Azure-Abonnements verfügen, erhalten Sie durch Herstellen einer Verbindung mit Azure Zugriff auf alle Azure-Abonnements, die mit Ihren Anmeldeinformationen verknüpft sind. Mit dem folgenden Befehl können Sie die Azure-Abonnements anzeigen und herausfinden, welches Abonnement das Standardabonnement ist:
   
   ```
    azure account list 
   ```

   Legen Sie den Abonnementkontext fest, unter dem alle weiteren Befehle ausgeführt werden sollen. Geben Sie dazu Folgendes ein:

   ```
    azure account set <subscription name>
   ```

3. Sie können die Ressourcengruppe **exampleResourceGroup** erstellen, wenn Sie über keine Ressourcengruppe verfügen:
   ```
    azure group create -n exampleResourceGroup -l westus
   ```

> [!TIP]
> Der Artikel [Verwalten von Azure-Ressourcen und -Ressourcengruppen mithilfe der Azure-Befehlszeilenschnittstelle][lnk-CLI-arm] enthält weitere Informationen zur Verwendung der Azure-Befehlszeilenschnittstelle zum Verwalten von Azure-Ressourcen. 
> 
> 

## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs
Erforderliche Parameter:

```
 azure iothub create -g <resource-group> -n <name> -l <location> -s <sku-name> -u <units>  
    - <resourceGroup> The resource group name (case insensitive alphanumeric, underscore and hyphen, 1-64 length)
    - <name> (The name of the IoT hub to be created. The format is case insensitive alphanumeric, underscore and hyphen, 3-50 length )
    - <location> (The location (azure region/datacenter) where the IoT hub will be provisioned.
    - <sku-name> (The name of the sku, one of: [F1, S1, S2, S3] etc. For the latest full list refer to the pricing page for IoT Hub.
    - <units> (The number of provisioned units. Range : F1 [1-1] : S1, S2 [1-200] : S3 [1-10]. IoT Hub units are based on your total message count and the number of devices you want to connect.)
```
Um alle für die Erstellung verfügbaren Parameter anzuzeigen, können Sie an der Eingabeaufforderung den Hilfebefehl verwenden:

```
    azure iothub create -h 
```
Kurzes Beispiel:

 Zum Erstellen des IoT Hubs **exampleIoTHubName** in der Ressourcengruppe **exampleResourceGroup** führen Sie den folgenden Befehl aus:

```
    azure iothub create -g exampleResourceGroup -n exampleIoTHubName -l westus -k s1 -u 1
```

> [!NOTE]
> Mit diesem Azure-CLI-Befehl wird ein für Sie kostenpflichtiger S1-Standard-IoT Hub hinzugefügt. Sie können den IoT Hub **exampleIoTHubName** mit dem folgenden Befehl löschen:
> 
> ```
> azure iothub delete -g exampleResourceGroup -n exampleIoTHubName
> ```
> 
> 
> 

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Entwickeln für IoT Hub finden Sie unter:

* [IoT-SDKs][lnk-sdks]

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [Verwenden des Azure-Portals zum Verwalten von IoT Hub][lnk-portal]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-CLI-install]:../cli-install-nodejs.md
[lnk-rest-api]: https://msdn.microsoft.com/library/mt589014.aspx
[lnk-CLI-arm]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md

[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-portal]: iot-hub-create-through-portal.md 

