---
title: Entwickeln von Azure Functions mit Media Services
description: In diesem Thema wird gezeigt, wie Sie im Azure-Portal mit dem Entwickeln von Azure Functions mit Media Services beginnen.
services: media-services
documentationcenter: 
author: juliako
manager: erikre
editor: 
ms.assetid: 51bdcb01-1846-4e1f-bd90-70020ab471b0
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/13/2017
ms.author: juliako
translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: b077504e7a289fd4976aa22bab0ad5784ffb491b
ms.lasthandoff: 03/15/2017


---
#<a name="develop-azure-functions-with-media-services"></a>Entwickeln von Azure Functions mit Media Services
[!INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

In diesem Thema wird beschrieben, wie Sie im Azure-Portal mit dem Entwickeln von Azure Functions mit Media Services beginnen. 

Sie können [hier](https://github.com/Azure-Samples/media-services-dotnet-functions-integration) auch vorhandene Media Services Azure Functions bereitstellen, indem Sie die Schaltfläche zum Bereitstellen in Azure (**Deploy to Azure**) verwenden. Dieses Repository enthält Beispiele für Azure Functions, in denen Azure Media Services verwendet werden. Hiermit werden Workflows veranschaulicht, bei denen es um das Erfassen von Inhalten direkt aus dem Blobspeicher, die Codierung und das Rückschreiben von Inhalt in den Blobspeicher geht. Außerdem sind Beispiele dafür vorhanden, wie Sie Auftragsbenachrichtigungen über Webhooks und Azure-Warteschlangen überwachen.

Sie können Ihre Functions basierend auf den Beispielen in [diesem Repository](https://github.com/Azure-Samples/media-services-dotnet-Functions-integration) entwickeln. In diesem Thema wird veranschaulicht, wie Sie mit der Erstellung von Azure Functions beginnen, für die Media Services verwendet werden. 

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie Ihre erste Funktion erstellen können, müssen Sie über ein aktives Azure-Konto verfügen. Wenn Sie noch kein Azure-Konto haben, [erstellen Sie ein kostenloses Konto](https://azure.microsoft.com/free/).

Es ist ratsam, wie [hier](media-services-portal-create-account.md) beschrieben ein AMS-Konto zu erstellen, wenn Sie Azure Functions erstellen möchten, mit denen Aktionen in Ihrem AMS-Konto (Azure Media Services) durchgeführt werden oder auf von Media Services gesendete Ereignisse gelauscht wird.

## <a name="create-a-function-app"></a>Erstellen einer Funktionen-App

Erstellen Sie wie [hier](../azure-functions/functions-create-first-azure-function-azure-portal.md#create-a-function-app) beschrieben eine Funktionen-App.

## <a name="create-a-function"></a>Erstellen einer Funktion

Nachdem die Funktionen-App bereitgestellt wurde, wird sie unter den Azure Functions von **App Services** aufgeführt. 

1. Wählen Sie Ihre Funktionen-App aus, und klicken Sie auf **Neue Funktion**.
3. Wählen Sie die Sprache **C#** und das Szenario **Webhook + API**.
3. Wählen Sie **GenericWebHook-CSharp** (wird jeweils bei Empfang einer Webhookanforderung ausgeführt) oder **HttpTrigger-CSharp** (wird jeweils bei Empfang einer HTTP-Anforderung ausgeführt), und benennen Sie Ihre Funktion.
4. Klicken Sie auf **Erstellen**. 

## <a name="get-function-url"></a>Abrufen der Funktions-URL

Sie benötigen den Wert für die Funktions-URL, um die Ausführung Ihrer Funktionen über ein HTTP-Testtool oder ein anderes Browserfenster auszulösen. 

![Einstellungen](./media/media-services-azure-functions/media-services-azure-functions002.png)

## <a name="files"></a>Dateien

Ihre Azure-Funktion ist Codedateien und anderen Dateien zugeordnet, die in diesem Abschnitt beschrieben werden. Standardmäßig ist eine Funktion den Dateien **function.json** und **run.csx** zugeordnet. Sie müssen die Datei **project.json** hinzufügen. Im restlichen Teil dieses Abschnitts werden die Definitionen für diese Dateien angezeigt.

![files](./media/media-services-azure-functions/media-services-azure-functions003.png)

### <a name="functionjson"></a>function.json

Die Datei „function.json“ definiert die Funktionsbindungen und weitere Konfigurationseinstellungen. Die Laufzeit verwendet diese Datei, um zu ermitteln, welche Ereignisse überwacht werden sollen und wie Daten in die Funktionsausführung übergeben und aus dieser zurückgegeben werden. 

Hier ist ein Beispiel für die Datei **function.json** angegeben.

    {
      "bindings": [
        {
          "type": "httpTrigger",
          "name": "req",
          "direction": "in",
          "methods": [
        "post",
        "get",
        "put",
        "update",
        "patch"
          ]
        },
        {
          "type": "http",
          "name": "res",
          "direction": "out"
        }
      ]
    }
    
### <a name="projectjson"></a>project.json

Die Datei „project.json“ enthält die Abhängigkeiten. Hier ist ein Beispiel für die Datei **function.json** mit AMS-Bibliotheken angegeben.

    {
      "frameworks": {
        "net46":{
          "dependencies": {
        "windowsazure.mediaservices": "3.8.0.5",
        "windowsazure.mediaservices.extensions": "3.8.0.3"
          }
        }
       }
    }
    
### <a name="runcsx"></a>run.csx

Dies ist der C#-Code für Ihre Funktion. Ein Beispiel für eine Webhookfunktion finden Sie in [diesem Thema](media-services-dotnet-check-job-progress-with-webhooks.md). 

Klicken Sie auf **Ausführen**, nachdem Sie die Funktion definiert haben.
    
    ///////////////////////////////////////////////////
    #r "Newtonsoft.Json"
    
    using System;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using System.IO;
    using System.Globalization;
    using Newtonsoft.Json;
    using Microsoft.Azure;
    using System.Net;
    using System.Security.Cryptography;
    

    static string _mediaServicesAccountName = Environment.GetEnvironmentVariable("AMSAccount");
    static string _mediaServicesAccountKey = Environment.GetEnvironmentVariable("AMSKey");
    
    static CloudMediaContext _context = null;
    
    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
    {
        log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");
    
        Task<byte[]> taskForRequestBody = req.Content.ReadAsByteArrayAsync();
        byte[] requestBody = await taskForRequestBody;
    
        string jsonContent = await req.Content.ReadAsStringAsync();
        log.Info($"Request Body = {jsonContent}");
    
        // some valication code ...

        _context = new CloudMediaContext(new MediaServicesCredentials(
        _mediaServicesAccountName,
        _mediaServicesAccountKey));

        // some AMS operations ...
      
        return req.CreateResponse(HttpStatusCode.BadRequest, "Generic Error.");
    }



## <a name="configure-function-app-settings"></a>Konfigurieren von Einstellungen der Funktionen-App

Beim Entwickeln von Media Services-Funktionen ist es nützlich, Parameter, die in Ihren Funktionen verwendet werden, dem Abschnitt mit den **App-Einstellungen** hinzuzufügen. 

Beispiel:

![Einstellungen](./media/media-services-azure-functions/media-services-azure-functions001.png)


## <a name="next-step"></a>Nächster Schritt

Nun können Sie mit der Entwicklung einer Mediendienste-Anwendung beginnen. Weitere Informationen finden Sie unter [Verwenden von Azure-Webhooks zum Überwachen von Media Services-Auftragsbenachrichtigungen mit .NET](media-services-dotnet-check-job-progress-with-webhooks.md).   

## <a name="media-services-learning-paths"></a>Media Services-Lernpfade
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


