---
title: Erstellen einer Azure IoT Hub-Instanz mithilfe der Ressourcenanbieter-REST-API | Microsoft Docs
description: Verwenden der Ressourcenanbieter-REST-API zum Erstellen einer IoT Hub-Instanz
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 52814ee5-bc10-4abe-9eb2-f8973096c2d8
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/04/2017
ms.author: dobett
ms.translationtype: Human Translation
ms.sourcegitcommit: e7da3c6d4cfad588e8cc6850143112989ff3e481
ms.openlocfilehash: 61ff425266ddd6fe6f9dbe03890257bcb9c84376
ms.contentlocale: de-de
ms.lasthandoff: 05/16/2017


---
# <a name="create-an-iot-hub-using-the-resource-provider-rest-api-net"></a>Erstellen einer IoT Hub-Instanz mithilfe der Ressourcenanbieter-REST-API (.NET)
[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Einführung
Sie können die [IoT Hub-Ressourcenanbieter-REST-API][lnk-rest-api] verwenden, um Azure IoT Hubs programmgesteuert zu erstellen und zu verwalten. In diesem Tutorial erfahren Sie, wie Sie mithilfe des REST-API für IoT Hub-Ressourcenanbieter mit einem C#-Programm einen IoT Hub erstellen.

> [!NOTE]
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Azure Resource Manager-Bereitstellung und klassische Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md).  Dieser Artikel behandelt die Verwendung des Azure Resource Manager-Bereitstellungsmodells.
> 
> 

Für dieses Tutorial benötigen Sie Folgendes:

* Visual Studio 2015 oder Visual Studio 2017
* Ein aktives Azure-Konto. <br/>Wenn Sie kein Konto besitzen, können Sie in nur wenigen Minuten ein [kostenloses Konto][lnk-free-trial] erstellen.
* [Azure PowerShell 1.0][lnk-powershell-install] oder höher.

[!INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>Vorbereiten des Visual Studio-Projekts
1. Erstellen Sie in Visual Studio mithilfe der Projektvorlage **Konsolen-App (.NET Framework)** ein Visual C#-Projekt für den klassischen Windows-Desktop. Geben Sie dem Projekt den Namen **CreateIoTHubREST**.
2. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und klicken Sie dann auf **NuGet-Pakete verwalten**.
3. Markieren Sie im NuGet-Paket-Manager die Option **Vorabversion einbeziehen**, und suchen Sie auf der Seite **Durchsuchen** nach **Microsoft.Azure.Management.ResourceManager**. Wählen Sie das Paket aus, klicken Sie auf **Installieren** und dann unter **Änderungen überprüfen** auf **OK**. Klicken Sie anschließend auf **Ich akzeptiere**, um die Lizenzen zu akzeptieren.
4. Suchen Sie im NuGet-Paket-Manager nach **Microsoft.IdentityModel.Clients.ActiveDirectory**.  Klicken Sie auf **Installieren**, dann unter **Änderungen überprüfen** auf **OK**. Klicken Sie anschließend auf **Ich akzeptiere**, um die Lizenz zu akzeptieren.
5. Ersetzen Sie in „Program.cs“ die vorhandenen **using**-Anweisungen durch folgenden Code:
   
    ```
    using System;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Text;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Newtonsoft.Json;
    using Microsoft.Rest;
    using System.Linq;
    using System.Threading;
    ```
6. Fügen Sie in „Program.cs“ die folgenden statischen Variablen ein, mit denen die Platzhalterwerte ersetzt werden. Weiter oben in diesem Tutorial haben Sie sich die Werte für **ApplicationId**, **SubscriptionId**, **TenantId** und **Password** notiert. **Ressourcengruppenname** ist der Name der Ressourcengruppe, die beim Erstellen des IoT Hubs verwendet wird – diese Gruppe kann neu oder bereits vorhanden sein. **IoT Hub-Name** ist der Name des erstellten IoT Hubs, beispielsweise **MyIoTHub** (dieser Name muss global eindeutig sein und sollte daher Ihren Namen oder Ihre Initialen enthalten). **Bereitstellungsname** ist ein Name für die Bereitstellung, wie z.B. **Deployment_01**.
   
    ```
    static string applicationId = "{Your ApplicationId}";
    static string subscriptionId = "{Your SubscriptionId}";
    static string tenantId = "{Your TenantId}";
    static string password = "{Your application Password}";
   
    static string rgName = "{Resource group name}";
    static string iotHubName = "{IoT Hub name including your initials}";
    ```

[!INCLUDE [iot-hub-get-access-token](../../includes/iot-hub-get-access-token.md)]

## <a name="use-the-resource-provider-rest-api-to-create-an-iot-hub"></a>Verwenden der Ressourcenanbieter-REST-API zum Erstellen eines IoT-Hubs
Verwenden Sie die [IoT-Ressourcenanbieter-REST-API][lnk-rest-api], um einen IoT-Hub in der Ressourcengruppe zu erstellen. Sie können die Ressourcenanbieter-REST-API auch verwenden, um Änderungen an einem vorhandenen IoT-Hub vorzunehmen.

1. Fügen Sie "Program.cs" die folgende Methode hinzu:
   
    ```
    static void CreateIoTHub(string token)
    {
   
    }
    ```
2. Fügen Sie den folgenden Code in die Methode **CreateIoTHub** ein. Dieser Code erstellt ein **HttpClient**-Objekt mit dem Authentifizierungstoken in den Headern:
   
    ```
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
    ```
3. Fügen Sie den folgenden Code in die Methode **CreateIoTHub** ein. Dieser Code beschreibt den zu erstellenden IoT Hub und generiert eine JSON-Darstellung. Die aktuelle Liste der Standorte, die IoT Hub unterstützen, finden Sie unter [Azure-Status][lnk-status].
   
    ```
    var description = new
    {
      name = iotHubName,
      location = "East US",
      sku = new
      {
        name = "S1",
        tier = "Standard",
        capacity = 1
      }
    };
   
    var json = JsonConvert.SerializeObject(description, Formatting.Indented);
    ```
4. Fügen Sie den folgenden Code in die Methode **CreateIoTHub** ein. Dieser Code übermittelt die REST-Anforderung an Azure, überprüft die Antwort und ruft die URL ab, die Sie zum Überwachen des Status der Bereitstellungsaufgabe verwenden können:
   
    ```
    var content = new StringContent(JsonConvert.SerializeObject(description), Encoding.UTF8, "application/json");
    var requestUri = string.Format("https://management.azure.com/subscriptions/{0}/resourcegroups/{1}/providers/Microsoft.devices/IotHubs/{2}?api-version=2016-02-03", subscriptionId, rgName, iotHubName);
    var result = client.PutAsync(requestUri, content).Result;
   
    if (!result.IsSuccessStatusCode)
    {
      Console.WriteLine("Failed {0}", result.Content.ReadAsStringAsync().Result);
      return;
    }
   
    var asyncStatusUri = result.Headers.GetValues("Azure-AsyncOperation").First();
    ```
5. Fügen Sie den folgenden Code am Ende der **CreateIoTHub**-Methode hinzu. Dieser Code verwendet die **asyncStatusUri**-Adresse, die im vorherigen Schritt zum Warten auf den Abschluss der Bereitstellung abgerufen wurde:
   
    ```
    string body;
    do
    {
      Thread.Sleep(10000);
      HttpResponseMessage deploymentstatus = client.GetAsync(asyncStatusUri).Result;
      body = deploymentstatus.Content.ReadAsStringAsync().Result;
    } while (body == "{\"status\":\"Running\"}");
    ```
6. Fügen Sie den folgenden Code am Ende der **CreateIoTHub**-Methode hinzu. Dieser Code ruft die Schlüssel des IoT Hub ab, den Sie erstellt haben, und gibt sie an der Konsole aus:
   
    ```
    var listKeysUri = string.Format("https://management.azure.com/subscriptions/{0}/resourceGroups/{1}/providers/Microsoft.Devices/IotHubs/{2}/IoTHubKeys/listkeys?api-version=2016-02-03", subscriptionId, rgName, iotHubName);
    var keysresults = client.PostAsync(listKeysUri, null).Result;
   
    Console.WriteLine("Keys: {0}", keysresults.Content.ReadAsStringAsync().Result);
    ```

## <a name="complete-and-run-the-application"></a>Fertigstellen und Ausführen der Anwendung
Sie können die Anwendung jetzt durch Aufrufen der Methode **CreateIoTHub** fertig stellen und das Projekt dann erstellen und ausführen.

1. Fügen Sie den folgenden Code am Ende der **Main** -Methode hinzu:
   
    ```
    CreateIoTHub(token.AccessToken);
    Console.ReadLine();
    ```
2. Klicken Sie auf **Erstellen** und auf **Projektmappe erstellen**. Beheben Sie alle Fehler.
3. Klicken Sie auf **Debuggen** und dann auf **Debuggen starten**, um die Anwendung auszuführen. Es kann mehrere Minuten dauern, bis die Bereitstellung abgeschlossen ist.
4. Das Hinzufügen des neuen IoT-Hubs durch die Anwendung lässt sich durch Anzeigen der Ressourcenliste im [Azure-Portal][lnk-azure-portal] oder mithilfe des PowerShell-Cmdlets **Get-AzureRmResource** überprüfen.

> [!NOTE]
> Diese Beispielanwendung fügt einen für Sie kostenpflichtigen S1-Standard-IoT Hub hinzu. Sie können den IoT-Hub nach Abschluss des Beispiels über das [Azure-Portal][lnk-azure-portal] oder mithilfe des PowerShell-Cmdlets **Remove-AzureRmResource** löschen.
> 
> 

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie nun einen IoT-Hub mit der Ressourcenanbieter-REST-API bereitgestellt haben, möchten Sie vielleicht mehr wissen:

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
[lnk-powershell-install]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md

