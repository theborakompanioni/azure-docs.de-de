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
ms.date: 05/04/2017
ms.author: boltean
ms.translationtype: Human Translation
ms.sourcegitcommit: 5e6ffbb8f1373f7170f87ad0e345a63cc20f08dd
ms.openlocfilehash: 8ac82da36b2edb71fcd0599dac12a3ed18e33b6f
ms.contentlocale: de-de
ms.lasthandoff: 03/24/2017

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

```azurecli
azure --version
```

> [!NOTE]
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Azure Resource Manager-Bereitstellung und klassische Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md). Die Azure-Befehlszeilenschnittstelle muss im Azure Resource Manager-Modus ausgeführt werden:
>
> ```azurecli
> azure config mode arm
> ```

## <a name="set-your-azure-account-and-subscription"></a>Festlegen Ihres Azure-Kontos und -Abonnements

1. Geben Sie an der Eingabeaufforderung den folgenden Befehl ein, um sich anzumelden:

   ```azurecli
    azure login
   ```

   Verwenden Sie den vorgeschlagenen Webbrowser und Code zum Authentifizieren.
1. Wenn Sie über mehrere Azure-Abonnements verfügen, erhalten Sie durch Herstellen einer Verbindung mit Azure Zugriff auf alle Azure-Abonnements, die mit Ihren Anmeldeinformationen verknüpft sind. Mit dem folgenden Befehl können Sie die Azure-Abonnements anzeigen und herausfinden, welches Abonnement das Standardabonnement ist:

   ```azurecli
    azure account list
   ```

   Legen Sie den Abonnementkontext fest, unter dem alle weiteren Befehle ausgeführt werden sollen. Geben Sie dazu Folgendes ein:

   ```azurecli
    azure account set <subscription name>
   ```

1. Sie können die Ressourcengruppe **exampleResourceGroup** erstellen, wenn Sie über keine Ressourcengruppe verfügen:

   ```azurecli
    azure group create -n exampleResourceGroup -l westus
   ```

> [!TIP]
> Der Artikel [Verwalten von Azure-Ressourcen und -Ressourcengruppen mithilfe der Azure-Befehlszeilenschnittstelle][lnk-CLI-arm] enthält weitere Informationen zur Verwendung der Azure-Befehlszeilenschnittstelle zum Verwalten von Azure-Ressourcen.

## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs

Erforderliche Parameter:

```azurecli
azure iothub create -g <resource-group> -n <name> -l <location> -s <sku-name> -u <units>
```

* **resource-group**. Der Name der Ressourcengruppe. Beim Format wird die Groß-/Kleinschreibung beachtet. Es können alphanumerische Zeichen, Unterstriche und Bindestriche verwendet werden. Die Länge ist auf 1 bis 64 Zeichen beschränkt.
* **name**. Der Name der zu erstellenden IoT Hub-Instanz. Beim Format wird die Groß-/Kleinschreibung beachtet. Es können alphanumerische Zeichen, Unterstriche und Bindestriche verwendet werden. Die Länge ist auf 3 bis 50 Zeichen beschränkt.
* **location**. Der Standort (Azure-Region/Datencenter) für die Bereitstellung der IoT Hub-Instanz.
* **sku-name**. Der Name der SKU, einer der folgenden: [F1, S1, S2, S3]. Die aktuelle vollständige Liste finden Sie in der Preisübersicht für IoT Hub.
* **units**. Die Anzahl der bereitgestellten Einheiten. Bereich: F1 [1-1]; S1, S2 [1-200]; S3 [1-10]. Die IoT Hub-Einheiten basieren auf der Gesamtzahl von Nachrichten und der Anzahl von Geräten, die Sie verbinden möchten.

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

Um alle für die Erstellung verfügbaren Parameter anzuzeigen, können Sie an der Eingabeaufforderung den Hilfebefehl verwenden:

```azurecli
azure iothub create -h
```

Kurzes Beispiel: Zum Erstellen des IoT Hubs **exampleIoTHubName** in der Ressourcengruppe **exampleResourceGroup** führen Sie den folgenden Befehl aus:

```azurecli
azure iothub create -g exampleResourceGroup -n exampleIoTHubName -l westus -k s1 -u 1
```

> [!NOTE]
> Mit diesem Azure-CLI-Befehl wird ein für Sie kostenpflichtiger S1-Standard-IoT Hub hinzugefügt. Sie können den IoT Hub **exampleIoTHubName** mit dem folgenden Befehl löschen:
>
> ```azurecli
> azure iothub delete -g exampleResourceGroup -n exampleIoTHubName
> ```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Entwickeln für IoT Hub finden Sie im folgenden Artikel:

* [IoT-SDKs][lnk-sdks]

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [Verwenden des Azure-Portals zum Verwalten von IoT Hub][lnk-portal]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-CLI-install]:../cli-install-nodejs.md
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-CLI-arm]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md

[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-portal]: iot-hub-create-through-portal.md 

