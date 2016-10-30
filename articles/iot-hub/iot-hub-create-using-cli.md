<properties
    pageTitle="Erstellen eines IoT Hubs über die Befehlszeilenschnittstelle | Microsoft Azure"
    description="Befolgen Sie die Anweisungen in diesem Artikel, um einen IoT Hub über die Azure-Befehlszeilenschnittstelle zu erstellen."
    services="iot-hub"
    documentationCenter=".net"
    authors="BeatriceOltean"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="multiple"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/21/2016"
     ms.author="boltean"/>


# <a name="create-an-iot-hub-using-cli"></a>Erstellen eines IoT Hubs über die Befehlszeilenschnittstelle

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Einführung

Über die Azure-Befehlszeilenschnittstelle können Sie Azure IoT Hubs programmgesteuert erstellen und verwalten. In diesem Artikel wird beschrieben, wie Sie über die Azure-Befehlszeilenschnittstelle einen IoT Hub erstellen.

Für dieses Tutorial benötigen Sie Folgendes:

- Ein aktives Azure-Konto. Ein [kostenloses Azure-Testkonto][lnk-free-trial] können Sie in wenigen Minuten erstellen.
- [Azure-Befehlszeilenschnittstelle 0.10.4][lnk-CLI-install] oder höher. Wenn Sie die Azure-Befehlszeilenschnittstelle bereits installiert haben, können Sie die aktuelle Version mit dem folgenden Befehl an der Eingabeaufforderung überprüfen:
```
    azure --version
```

> [AZURE.NOTE] Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager-Bereitstellung und klassische Bereitstellung](../resource-manager-deployment-model.md). Die Azure-Befehlszeilenschnittstelle muss im Azure Resource Manager-Modus ausgeführt werden:
```
    azure config mode arm
```

## <a name="set-your-azure-account-and-subscription"></a>Festlegen Ihres Azure-Kontos und -Abonnements 

1. Geben Sie an der Eingabeaufforderung den folgenden Befehl ein, um sich anzumelden:
```
    azure login
```
Verwenden Sie den vorgeschlagenen Webbrowser und Code zum Authentifizieren.

2. Wenn Sie mehrere Azure-Abonnements besitzen, erhalten Sie durch das Herstellen einer Verbindung mit Azure Zugriff auf alle Abonnements, die mit Ihren Anmeldeinformationen verknüpft sind. Mit dem folgenden Befehl können Sie die Abonnements anzeigen und herausfinden, welches Abonnement das Standardabonnement ist:
```
    azure account list 
```

Legen Sie den Abonnementkontext fest, unter dem alle weiteren Befehle ausgeführt werden sollen. Geben Sie dazu Folgendes ein:

```
    azure account set <subscription name>
```

3. Sie können die Ressourcengruppe **exampleResourceGroup** erstellen, wenn Sie über keine verfügen. 
```
    azure group create -n exampleResourceGroup -l westus
```

> [AZURE.TIP] Der Artikel [Verwalten von Azure-Ressourcen und -Ressourcengruppen mithilfe der Azure-Befehlszeilenschnittstelle][lnk-CLI-arm] enthält weitere Informationen zur Verwendung der Azure-Befehlszeilenschnittstelle zum Verwalten von Azure-Ressourcen. 


## <a name="create-and-iot-hub"></a>Erstellen eines IoT Hubs

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

 Zum Erstellen des IoT Hubs **exampleIoTHubName** in der Ressourcengruppe **exampleResourceGroup** führen Sie einfach den folgenden Befehl aus:
```
    azure iothub create -g exampleResourceGroup -n exampleIoTHubName -l westus -k s1 -u 1
```

> [AZURE.NOTE] Mit diesem CLI-Befehl wird ein für Sie kostenpflichtiger S1-Standard-IoT Hub hinzugefügt. Sie können den IoT Hub **exampleIoTHubName** mit dem folgenden Befehl löschen: 
```
    azure iothub delete -g exampleResourceGroup -n exampleIoTHubName
```


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Entwickeln für IoT Hub finden Sie unter:
- [IoT Hub-SDKs][lnk-sdks]

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

- [Verwenden des Azure-Portals zur Verwaltung von IoT Hub][lnk-portal]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-CLI-install]: ../xplat-cli-install.md
[lnk-rest-api]: https://msdn.microsoft.com/library/mt589014.aspx
[lnk-azure-rm-overview]: ../resource-group-overview.md
[lnk-CLI-arm]: ../xplat-cli-azure-resource-manager.md

[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-portal]: iot-hub-create-through-portal.md 



<!--HONumber=Oct16_HO2-->


