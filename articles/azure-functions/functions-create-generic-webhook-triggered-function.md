---
title: "Erstellen einer Funktion in Azure, die durch einen generischen Webhook ausgelöst wird | Microsoft-Dokumentation"
description: Verwenden Sie Azure Functions zum Erstellen einer serverlosen Funktion, die durch einen Webhook in Azure aufgerufen wird.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.assetid: fafc10c0-84da-4404-b4fa-eea03c7bf2b1
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/12/2017
ms.author: glenga
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: f283f8d79c5ae5fb6a72c84c9e9edb7bb8de4a83
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---
# <a name="create-a-function-triggered-by-a-generic-webhook"></a>Erstellen einer Funktion, die durch einen generischen Webhook ausgelöst wird

Mit Azure Functions können Sie Code in einer serverlosen Umgebung ausführen, ohne vorher eine VM erstellen oder eine Webanwendung veröffentlichen zu müssen. Sie können beispielsweise eine Funktion generieren, die durch eine Warnung von Azure Monitor ausgelöst wird. In diesem Thema wird gezeigt, wie C#-Code ausgeführt wird, wenn Ihrem Abonnement eine Ressourcengruppe hinzugefügt wird.   

![Eine Funktion im Azure-Portal, die durch einen generischen Webhook ausgelöst wird](./media/functions-create-generic-webhook-triggered-function/function-completed.png)

## <a name="prerequisites"></a>Voraussetzungen 

Für dieses Tutorial benötigen Sie Folgendes:

+ Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Erstellen einer Azure Function-App

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

Erstellen Sie als Nächstes in der neuen Funktionen-App eine Funktion.

## <a name="create-function"></a>Erstellen einer Funktion, die durch einen generischen Webhook ausgelöst wird

1. Erweitern Sie die Funktionen-App, und klicken Sie auf die Schaltfläche **+** neben **Functions**. Wenn dies die erste Funktion in Ihrer Funktions-App ist, wählen Sie **Benutzerdefinierte Funktion** aus. Hiermit wird der vollständige Satz von Funktionsvorlagen angezeigt.

    ![Schnellstartseite für Funktionen im Azure-Portal](./media/functions-create-generic-webhook-triggered-function/add-first-function.png)

2. Wählen Sie die Vorlage für einen **generischen Webhook in C#** aus. Geben Sie einen Namen für die C#-Funktion ein, und wählen Sie dann **Erstellen** aus.

     ![Erstellen einer Funktion im Azure-Portal, die durch einen generischen Webhook ausgelöst wird](./media/functions-create-generic-webhook-triggered-function/functions-create-generic-webhook-trigger.png) 

2. Klicken Sie in der neuen Funktion auf **</> Funktions-URL abrufen**, kopieren Sie den Wert, und speichern Sie ihn. Verwenden Sie diesen Wert zum Konfigurieren des Webhooks. 

    ![Überprüfen des Funktionscodes](./media/functions-create-generic-webhook-triggered-function/functions-copy-function-url.png)
         
Als Nächstes erstellen Sie einen Webhook-Endpunkt in einer Aktivitätsprotokollwarnung in Azure Monitor. 

## <a name="create-an-activity-log-alert"></a>Erstellen einer Aktivitätsprotokollwarnung

1. Navigieren Sie im Azure-Portal zum Dienst **Monitor**, wählen Sie **Warnungen** aus, und klicken Sie dann auf **Aktivitätsprotokollwarnung hinzufügen**.   

    ![Überwachen](./media/functions-create-generic-webhook-triggered-function/functions-monitor-add-alert.png)

2. Verwenden Sie die Proxyeinstellungen aus der Tabelle:

    ![Erstellen einer Aktivitätsprotokollwarnung](./media/functions-create-generic-webhook-triggered-function/functions-monitor-add-alert-settings.png)

    | Einstellung      |  Empfohlener Wert   | Beschreibung                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Der Name der Aktivitätsprotokollwarnung** | resource-group-create-alert | Der Name der Aktivitätsprotokollwarnung |
    | **Abonnement** | Ihr Abonnement | Das Abonnement, das Sie für dieses Tutorial verwenden | 
    |  **Ressourcengruppe** | myResourceGroup | Die Ressourcengruppe, in der die Warnungsressourcen bereitgestellt werden. Wenn Sie die gleiche Ressourcengruppe wie für Ihre Funktions-App verwenden, vereinfacht dies die Bereinigung nach Abschluss des Tutorials. |
    | **Ereigniskategorie** | Administrative | Diese Kategorie umfasst Änderungen an Azure-Ressourcen.  |
    | **Ressourcentyp** | Ressourcengruppen | Filtert Warnungen zu Aktivitäten der Ressourcengruppe |
    | **Ressourcengruppe**<br/>und **Ressource** | Alle | Überwacht alle Ressourcen |
    | **Vorgangsname** | Ressourcengruppe erstellen | Filtert Warnungen, um Vorgänge zu erstellen |
    | **Level** | Information | Schließen Sie Warnungen auf Informationsebene ein. | 
    | **Status** | Erfolgreich | Filtert Warnungen für Aktionen, die erfolgreich abgeschlossen wurden |
    | **Aktionsgruppe** | Neu | Erstellen Sie eine neue Aktionsgruppe, die die Aktion definiert, die bei einer Warnung ausgelöst wird. |
    | **Aktionsgruppenname** | function-webhook | Ein Name zur Identifikation der Aktionsgruppe  | 
    | **Kurzname** | funcwebhook | Ein kurzer Name für die Aktionsgruppe. |  

3. Fügen Sie in **Aktionen** eine Aktion mit den Einstellungen in der Tabelle hinzu: 

    ![Hinzufügen einer Aktionsgruppe](./media/functions-create-generic-webhook-triggered-function/functions-monitor-add-alert-settings-2.png)

    | Einstellung      |  Empfohlener Wert   | Beschreibung                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Name** | CallFunctionWebhook | Der Name für die Aktion |
    | **Aktionstyp** | Webhook | Die Reaktion auf die Warnung ist, dass eine Webhook-URL aufgerufen wird. |
    | **Details** | Funktions-URL | Fügen Sie die Webhook-URL der Funktion ein, die Sie zuvor kopiert haben. |v

4. Klicken Sie auf **OK**, um die Warnung und die Aktionsgruppe zu erstellen.  

Der Webhook wird ab sofort aufgerufen, wenn in Ihrem Abonnement eine Ressourcengruppe erstellt wird. Als Nächstes aktualisieren Sie den Code in Ihrer Funktion, um die JSON-Protokolldaten im Text der Anforderung zu behandeln.   

## <a name="update-the-function-code"></a>Aktualisieren des Funktionscodes

1. Navigieren Sie zurück zu Ihrer Funktions-App im Portal, und erweitern Sie Ihre Funktion. 

2. Ersetzen Sie den C#-Skriptcode in der Funktion im Portal durch den folgenden Code:

    ```csharp
    #r "Newtonsoft.Json"
    
    using System;
    using System.Net;
    using Newtonsoft.Json;
    using Newtonsoft.Json.Linq;
    
    public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
    {
        log.Info($"Webhook was triggered!");
    
        // Get the activityLog object from the JSON in the message body.
        string jsonContent = await req.Content.ReadAsStringAsync();
        JToken activityLog = JObject.Parse(jsonContent.ToString())
            .SelectToken("data.context.activityLog");
    
        // Return an error if the resource in the activity log isn't a resource group. 
        if (activityLog == null || !string.Equals((string)activityLog["resourceType"], 
            "Microsoft.Resources/subscriptions/resourcegroups"))
        {
            log.Error("An error occured");
            return req.CreateResponse(HttpStatusCode.BadRequest, new
            {
                error = "Unexpected message payload or wrong alert received."
            });
        }
    
        // Write information about the created resource group to the streaming log.
        log.Info(string.Format("Resource group '{0}' was {1} on {2}.",
            (string)activityLog["resourceGroupName"],
            ((string)activityLog["subStatus"]).ToLower(), 
            (DateTime)activityLog["submissionTimestamp"]));
    
        return req.CreateResponse(HttpStatusCode.OK);    
    }
    ```

Jetzt können Sie die Funktion testen, indem Sie in Ihrem Abonnement eine neue Ressourcengruppe erstellen.

## <a name="test-the-function"></a>Testen der Funktion

1. Klicken Sie links im Azure-Portal auf das Symbol für die Ressourcengruppe, wählen Sie **+ Hinzufügen** aus, geben Sie einen **Ressourcengruppennamen** ein, und wählen Sie **Erstellen** aus, um eine leere Ressourcengruppe zu erstellen.
    
    ![Erstellen Sie eine Ressourcengruppe.](./media/functions-create-generic-webhook-triggered-function/functions-create-resource-group.png)

2. Wechseln Sie zu Ihrer Funktion zurück, und erweitern Sie das Fenster **Protokolle**. Nachdem die Ressourcengruppe erstellt wurde, löst die Aktivitätsprotokollwarnung den Webhook aus, und die Funktion wird ausgeführt. Daraufhin wird der Name der neuen Ressourcengruppe in die Protokolle geschrieben.  

    ![Fügen Sie eine Testanwendungseinstellung hinzu.](./media/functions-create-generic-webhook-triggered-function/function-view-logs.png)

3. (Optional:) Wechseln Sie zurück, und löschen Sie die Ressourcengruppe, die Sie erstellt haben. Beachten Sie, dass diese Aktivität die Funktion nicht auslöst. Der Grund hierfür ist, dass Löschvorgänge von der Warnung herausgefiltert werden. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie haben eine Funktion erstellt, die ausgeführt wird, wenn eine Anforderung von einem generischen Webhook empfangen wurde. 

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Weitere Informationen zu Webhooktriggern finden Sie unter [HTTP- und Webhookbindungen in Azure Functions](functions-bindings-http-webhook.md). Weitere Informationen zum Entwickeln von Funktionen in C# finden Sie unter [C#-Skriptentwicklerreferenz zu Azure Functions](functions-reference-csharp.md).


