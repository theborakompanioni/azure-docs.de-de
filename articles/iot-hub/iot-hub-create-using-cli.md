---
title: Erstellen eines IoT Hubs mit der Azure CLI (az.py) | Microsoft-Dokumentation
description: "Erstellen eines Azure IoT Hubs mithilfe der plattformübergreifenden Azure CLI 2.0 (az.py)"
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-hub
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: dobett
ms.translationtype: HT
ms.sourcegitcommit: 26c07d30f9166e0e52cb396cdd0576530939e442
ms.openlocfilehash: 161089159999a4a63a39b059e69a08b7a9297445
ms.contentlocale: de-de
ms.lasthandoff: 07/19/2017

---
# <a name="create-an-iot-hub-using-the-azure-cli-20"></a>Erstellen eines IoT Hubs mit der Azure CLI 2.0

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Einführung

Sie können die Azure CLI 2.0 (az.py) verwenden, um Azure IoT Hubs programmgesteuert zu erstellen und zu verwalten. In diesem Artikel wird beschrieben, wie Sie über die Azure CLI 2.0 (az.py) einen IoT Hub erstellen.

Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch:

* [Azure CLI (azure.js)](iot-hub-create-using-cli-nodejs.md): die Befehlszeilenschnittstelle für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell
* Azure CLI 2.0 (az.py): die Befehlszeilenschnittstelle der nächsten Generation für das Resource Manager-Bereitstellungsmodell, wie in diesem Artikel beschrieben

Für dieses Tutorial benötigen Sie Folgendes:

* Ein aktives Azure-Konto. Wenn Sie nicht über ein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto][lnk-free-trial] erstellen.
* [Azure CLI 2.0][lnk-CLI-install].

## <a name="sign-in-and-set-your-azure-account"></a>Anmelden und Festlegen Ihres Azure-Kontos

Melden Sie sich bei Ihrem Azure-Konto an, und wählen Sie Ihr Abonnement aus.

1. Führen Sie an der Eingabeaufforderung den [Anmeldebefehl][lnk-login-command] aus:
    
    ```azurecli
    az login
    ```

    Befolgen Sie die Anweisungen zur Authentifizierung mit dem Code, und melden Sie sich in einem Webbrowser bei Ihrem Azure-Konto an.

2. Wenn Sie über mehrere Azure-Abonnements verfügen, erhalten Sie durch die Anmeldung bei Azure Zugriff auf alle Azure-Konten, die mit Ihren Anmeldeinformationen verknüpft sind. Führen Sie den folgenden [Befehl aus, um eine Liste der Azure-Konten anzuzeigen][lnk-az-account-command], die Sie verwenden können:
    
    ```azurecli
    az account list 
    ```

    Führen Sie den folgenden Befehl aus, um das Abonnement auszuwählen, das Sie zum Ausführen der Befehle zum Erstellen Ihres IoT Hubs verwenden möchten. Sie können entweder den Abonnementnamen oder die ID aus der Ausgabe des vorherigen Befehls verwenden:

    ```azurecli
    az account set --subscription {your subscription name or id}
    ```

## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs

Verwenden Sie die Azure CLI zum Erstellen einer Ressourcengruppe, und fügen Sie dann einen IoT Hub hinzu.

1. Wenn Sie einen IoT Hub erstellen, müssen Sie ihn in einer Ressourcengruppe erstellen. Verwenden Sie entweder eine vorhandene Ressourcengruppe, oder führen Sie den folgenden [Befehl zum Erstellen einer Ressourcengruppe aus][lnk-az-resource-command]:
    
    ```azurecli
     az group create --name {your resource group name} --location westus
    ```

    > [!TIP]
    > Im vorherigen Beispiel wird die Ressourcengruppe am Standort „USA, Westen“ erstellt. Mit dem Befehl `az account list-locations -o table` können Sie eine Liste der verfügbaren Standorte anzeigen.
    >
    >

2. Führen Sie den folgenden [Befehl zum Erstellen eines IoT Hubs][lnk-az-iot-command] in Ihrer Ressourcengruppe mithilfe eines global eindeutigen Namens für Ihren IoT Hub aus:
    
    ```azurecli
    az iot hub create --name {your iot hub name} --resource-group {your resource group name} --sku S1
    ```

   [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]


> [!NOTE]
> Mit dem vorherigen Befehl wird ein kostenpflichtiger IoT Hub im S1-Tarif erstellt. Weitere Informationen finden Sie unter [Azure IoT Hub – Preise][lnk-iot-pricing].
>
>

## <a name="remove-an-iot-hub"></a>Entfernen eines IoT Hubs

Mit der Azure CLI können Sie [eine einzelne Ressource][lnk-az-resource-command] wie einen IoT Hub oder eine Ressourcengruppe und alle zugehörigen Ressourcen einschließlich IoT Hubs löschen.

Führen Sie den folgenden Befehl aus, um einen IoT Hub zu löschen:

```azurecli
az iot hub delete --name {your iot hub name} --resource-group {your resource group name}
```

Führen Sie den folgenden Befehl aus, um eine Ressourcengruppe und alle zugehörigen Ressourcen zu löschen:

```azurecli
az group delete --name {your resource group name}
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Entwickeln für IoT Hub finden Sie in folgenden Artikeln:

* [IoT Hub-Entwicklerhandbuch][lnk-devguide]

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [Verwenden des Azure-Portals zum Verwalten von IoT Hub][lnk-portal]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-CLI-install]: https://docs.microsoft.com/cli/azure/install-az-cli2
[lnk-login-command]: https://docs.microsoft.com/cli/azure/get-started-with-az-cli2
[lnk-az-account-command]: https://docs.microsoft.com/cli/azure/account
[lnk-az-register-command]: https://docs.microsoft.com/cli/azure/provider
[lnk-az-addcomponent-command]: https://docs.microsoft.com/cli/azure/component
[lnk-az-resource-command]: https://docs.microsoft.com/cli/azure/resource
[lnk-az-iot-command]: https://docs.microsoft.com/cli/azure/iot
[lnk-iot-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-devguide]: iot-hub-devguide.md
[lnk-portal]: iot-hub-create-through-portal.md 

