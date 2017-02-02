---
title: Erstellen eines IoT Hubs mit der Azure CLI (az.py) | Microsoft-Dokumentation
description: "So erstellen Sie einen Azure IoT Hub mithilfe der plattformübergreifenden Azure CLI 2.0 (Vorschau) (az.py)."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/02/2016
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: 39c8c4944ef19379dc04e04a717ab60d305593c4
ms.openlocfilehash: c52d9a5fadf494cc066bee773543c9d67bb8334b


---
# <a name="create-an-iot-hub-using-the-azure-cli-20-preview"></a>Erstellen eines IoT Hubs mit der Azure CLI 2.0 (Vorschau)

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Einführung

Sie können die Azure CLI 2.0 (Vorschau) (az.py) verwenden, um Azure IoT Hubs programmgesteuert zu erstellen und zu verwalten. In diesem Artikel wird beschrieben, wie Sie über die Azure CLI 2.0 (Vorschau) (az.py) einen IoT Hub erstellen.

Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch:

* [Azure CLI (azure.js)](iot-hub-create-using-cli-nodejs.md): die Befehlszeilenschnittstelle für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell
* Azure CLI 2.0 (Vorschau) (az.py): die Befehlszeilenschnittstelle der nächsten Generation für das Resource Manager-Bereitstellungsmodell wie in diesem Artikel beschrieben

Für dieses Tutorial benötigen Sie Folgendes:

* Ein aktives Azure-Konto. Wenn Sie kein Konto besitzen, können Sie in nur wenigen Minuten ein [kostenloses Konto][lnk-free-trial] erstellen.
* [Azure CLI 2.0 (Vorschau)][lnk-CLI-install]

## <a name="sign-in-and-set-your-azure-account"></a>Anmelden und Festlegen Ihres Azure-Kontos

Melden Sie sich bei Ihrem Azure-Konto an, und konfigurieren Sie die Azure CLI für IoT Hub-Ressourcen.

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

3. Bevor Sie IoT-Ressourcen bereitstellen können, müssen Sie den IoT-Anbieter registrieren. Führen Sie den folgenden [Befehl aus, um den IoT-Anbieter zu registrieren][lnk-az-register-command]:
    
    ```azurecli
    az provider register -namespace "Microsoft.Devices"
    ```

4. Möglicherweise müssen Sie die _IoT-Komponente_ der Azure CLI installieren. Führen Sie den folgenden [Befehl aus, um die IoT-Komponente hinzuzufügen][lnk-az-addcomponent-command]:
    
    ```azurecli
    az component update --add iot
    ```

## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs

Verwenden Sie die Azure CLI zum Erstellen einer Ressourcengruppe, und fügen Sie dann einen IoT Hub hinzu.

1. Wenn Sie einen IoT Hub erstellen, müssen Sie ihn in einer Ressourcengruppe erstellen. Verwenden Sie entweder eine vorhandene Ressourcengruppe, oder führen Sie den folgenden [Befehl zum Erstellen einer Ressourcengruppe aus][lnk-az-resource-command]:
    
    ```azurecli
     az resource group create --name {your resource group name} --location westus
    ```

    > [!TIP]
    > Im vorherigen Beispiel wird die Ressourcengruppe am Standort „USA, Westen“ erstellt. Mit dem Befehl `az account list-locations -o table` können Sie eine Liste der verfügbaren Standorte anzeigen.
    >
    >

2. Führen Sie den folgenden [Befehl aus, um einen IoT Hub][lnk-az-iot-command] in Ihrer Ressourcengruppe zu erstellen:
    
    ```azurecli
    az iot hub create --name {your iot hub name} --resource-group {your resource group name} --sku S1
    ```

> [!NOTE]
> Der Name des IoT Hub muss global eindeutig sein. Mit dem vorherigen Befehl wird ein kostenpflichtiger IoT Hub im S1-Tarif erstellt. Weitere Informationen finden Sie unter [Azure IoT Hub – Preise][lnk-iot-pricing].
>
>

## <a name="remove-an-iot-hub"></a>Entfernen eines IoT Hubs

Mit der Azure CLI können Sie [eine einzelne Ressource][lnk-az-resource-command] wie einen IoT Hub oder eine Ressourcengruppe und alle zugehörigen Ressourcen einschließlich IoT Hubs löschen.

Führen Sie den folgenden Befehl aus, um einen IoT Hub zu löschen:

```azurecli
az resource delete --name {your iot hub name} --resource-group {your resource group name} --resource-type Microsoft.Devices/IotHubs
```

Führen Sie den folgenden Befehl aus, um eine Ressourcengruppe und alle zugehörigen Ressourcen zu löschen:

```azurecli
az resource group delete --name {your resource group name}
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Entwickeln für IoT Hub finden Sie unter:

* [IoT Hub-Entwicklerhandbuch][lnk-devguide]

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [Verwenden des Azure-Portals zur Verwaltung von IoT Hub][lnk-portal]

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



<!--HONumber=Dec16_HO2-->


