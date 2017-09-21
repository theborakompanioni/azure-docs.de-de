---
title: Integration von Azure Event Grid und Event Hubs
description: "Hier wird erläutert, wie Daten mithilfe von Azure Event Grid und Event Hubs zu einem SQL Data Warehouse migriert werden."
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 09/14/2017
ms.author: tomfitz
ms.translationtype: HT
ms.sourcegitcommit: d24c6777cc6922d5d0d9519e720962e1026b1096
ms.openlocfilehash: 5f35a3bdd97b72acfe06b6bec54d41814e560b1c
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---
# <a name="stream-big-data-into-a-data-warehouse"></a>Streamen von Big Data in ein Data Warehouse

Bei Azure [Event Grid](overview.md) handelt es sich um einen intelligenten Ereignisroutingdienst, der es Ihnen ermöglicht, auf Benachrichtigungen von Apps und Diensten zu reagieren. Das [Beispiel für Event Hubs Capture und Event Grid](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo) veranschaulicht, wie Sie Azure Event Hubs Capture mit Azure Event Grid für die nahtlose Migration von Daten von einem Event Hub zu einem SQL Data Warehouse verwenden.

![Anwendungsübersicht](media/event-grid-event-hubs-integration/overview.png)

Wenn Daten an den Event Hub gesendet werden, ruft Capture per Pull Daten aus dem Stream ab und generiert Speicherblobs mit den Daten im Avro-Format. Beim Erstellen des Blobs durch Capture wird ein Ereignis ausgelöst. Event Grid verteilt Daten zum Ereignis an Abonnenten. In diesem Fall werden die Ereignisdaten an den Azure Functions-Endpunkt gesendet. Die Ereignisdaten enthalten den Pfad des erstellten Blobs. Die Funktion ruft die Datei mithilfe dieser URL ab und sendet sie an das Data Warehouse.

In diesem Artikel führen Sie folgende Schritte aus:

* Bereitstellen der folgenden Infrastruktur:
  * Event Hub, für den Capture aktiviert ist
  * Speicherkonto für die Dateien aus Capture
  * Azure App Service-Tarif zum Hosten der Funktions-App
  * Funktions-App zum Verarbeiten des Ereignisses
  * SQL Server zum Hosten des Data Warehouse
  * SQL Data Warehouse zum Speichern der migrierten Daten
* Erstellen einer Tabelle im Data Warehouse
* Hinzufügen von Code zur Funktions-App
* Abonnieren des Ereignisses
* Ausführen der App, die Daten an den Event Hub sendet
* Anzeigen von migrierten Daten im Data Warehouse

## <a name="about-the-event-data"></a>Informationen zu den Ereignisdaten

Event Grid verteilt Ereignisdaten an die Abonnenten. Das folgende Beispiel zeigt Ereignisdaten zum Erstellen einer Capture-Datei. Beachten Sie insbesondere die `fileUrl`-Eigenschaft im `data`-Objekt. Die Funktions-App ruft diesen Wert ab und verwendet ihn zum Abrufen der Capture-Datei.

```json
[
    {
        "topic": "/subscriptions/<guid>/resourcegroups/rgDataMigrationSample/providers/Microsoft.EventHub/namespaces/tfdatamigratens",
        "subject": "eventhubs/hubdatamigration",
        "eventType": "Microsoft.EventHub.CaptureFileCreated",
        "eventTime": "2017-08-31T19:12:46.0498024Z",
        "id": "14e87d03-6fbf-4bb2-9a21-92bd1281f247",
        "data": {
            "fileUrl": "https://tf0831datamigrate.blob.core.windows.net/windturbinecapture/tfdatamigratens/hubdatamigration/1/2017/08/31/19/11/45.avro",
            "fileType": "AzureBlockBlob",
            "partitionId": "1",
            "sizeInBytes": 249168,
            "eventCount": 1500,
            "firstSequenceNumber": 2400,
            "lastSequenceNumber": 3899,
            "firstEnqueueTime": "2017-08-31T19:12:14.674Z",
            "lastEnqueueTime": "2017-08-31T19:12:44.309Z"
        }
    }
]
```

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.
* [Visual Studio 2017 Version 15.3.2 oder höher](https://www.visualstudio.com/vs/) mit Workloads für .NET-Desktopentwicklung, Azure-Entwicklung, ASP.NET- und Webentwicklung, Node.js-Entwicklung und Python-Entwicklung
* Das auf Ihren Computer heruntergeladene [EventHubsCaptureEventGridDemo-Beispielprojekt](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo)

## <a name="deploy-the-infrastructure"></a>Bereitstellen der Infrastruktur

Zur Vereinfachung dieses Artikels stellen Sie die erforderliche Infrastruktur mit einer Resource Manager-Vorlage bereit. Sehen Sie sich die [Vorlage](https://github.com/Azure/azure-docs-json-samples/blob/master/event-grid/EventHubsDataMigration.json) an, um zu ermitteln, welche Ressourcen bereitgestellt werden.

Verwenden Sie für die Azure-Befehlszeilenschnittstelle den folgenden Befehl:

```azurecli-interactive
az group create -l westcentralus -n rgDataMigrationSample

az group deployment create \
  --resource-group rgDataMigrationSample \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/EventHubsDataMigration.json \
  --parameters eventHubNamespaceName=<event-hub-namespace> eventHubName=hubdatamigration sqlServerName=<sql-server-name> sqlServerUserName=<user-name> sqlServerPassword=<password> sqlServerDatabaseName=<database-name> storageName=<unique-storage-name> functionAppName=<app-name>
```

Verwenden Sie für PowerShell Folgendes:

```powershell
New-AzureRmResourceGroup -Name rgDataMigration -Location westcentralus

New-AzureRmResourceGroupDeployment -ResourceGroupName rgDataMigration -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/EventHubsDataMigration.json -eventHubNamespaceName <event-hub-namespace> -eventHubName hubdatamigration -sqlServerName <sql-server-name> -sqlServerUserName <user-name> -sqlServerDatabaseName <database-name> -storageName tf08202storage -functionAppName <app-name>
```

Geben Sie bei entsprechender Aufforderung einen Kennwortwert ein.

## <a name="create-a-table-in-sql-data-warehouse"></a>Erstellen einer Tabelle in SQL Data Warehouse

Fügen Sie dem Data Warehouse durch Ausführen des Skripts [CreateDataWarehouseTable.sql](https://github.com/Azure/azure-event-hubs/blob/master/samples/e2e/EventHubsCaptureEventGridDemo/scripts/CreateDataWarehouseTable.sql) eine Tabelle hinzu. Verwenden Sie zum Ausführen des Skripts Visual Studio oder den Abfrage-Editor im Portal.

Das auszuführende Skript lautet wie folgt:

```sql
CREATE TABLE [dbo].[Fact_WindTurbineMetrics] (
    [DeviceId] nvarchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL, 
    [MeasureTime] datetime NULL, 
    [GeneratedPower] float NULL, 
    [WindSpeed] float NULL, 
    [TurbineSpeed] float NULL
)
WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = ROUND_ROBIN);
```

## <a name="publish-the-azure-functions-app"></a>Veröffentlichen der Azure Functions-App

1. Öffnen Sie das [EventHubsCaptureEventGridDemo-Beispielprojekt](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo) in Visual Studio 2017 (15.3.2 oder höher).

2. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf **FunctionDWDumper**, und wählen Sie **Veröffentlichen** aus.

   ![Veröffentlichen einer Funktions-App](media/event-grid-event-hubs-integration/publish-function-app.png)

3. Wählen Sie **Azure-Funktionen-App** und **Vorhandenes Element auswählen**. Klicken Sie auf **OK**.

   ![Ziel-Funktions-App](media/event-grid-event-hubs-integration/pick-target.png)

4. Wählen Sie die Funktions-App aus, die Sie über die Vorlage bereitgestellt haben. Klicken Sie auf **OK**.

   ![Auswählen der Funktions-App](media/event-grid-event-hubs-integration/select-function-app.png)

5. Wenn Visual Studio das Profil konfiguriert hat, wählen Sie **Veröffentlichen**.

   ![Wählen Sie "Veröffentlichen"](media/event-grid-event-hubs-integration/select-publish.png)

6. Rufen Sie nach dem Veröffentlichen der Funktion das [Azure-Portal](https://portal.azure.com/) auf. Wählen Sie die Ressourcengruppe und die Funktions-App aus.

   ![Anzeigen der Funktions-App](media/event-grid-event-hubs-integration/view-function-app.png)

7. Wählen Sie die Funktion aus.

   ![Auswählen der Funktion](media/event-grid-event-hubs-integration/select-function.png)

8. Rufen Sie die URL für die Funktion ab. Sie benötigen diese URL beim Erstellen des Ereignisabonnements.

   ![Abrufen der Funktions-URL](media/event-grid-event-hubs-integration/get-function-url.png)

9. Kopieren Sie den Wert.

   ![Kopieren der URL](media/event-grid-event-hubs-integration/copy-url.png)

## <a name="subscribe-to-the-event"></a>Abonnieren des Ereignisses

Sie können zum Abonnieren des Ereignisses die Azure CLI oder das Portal verwenden. In diesem Artikel werden beide Ansätze beschrieben.

### <a name="portal"></a>Portal

1. Wählen Sie im Event Hubs-Namespace links **Event Grid** aus.

   ![Auswählen von Event Grid](media/event-grid-event-hubs-integration/select-event-grid.png)

2. Fügen Sie ein Ereignisabonnement hinzu.

   ![Hinzufügen eines Ereignisabonnements](media/event-grid-event-hubs-integration/add-event-subscription.png)

3. Geben Sie Werte für das Ereignisabonnement ein. Verwenden Sie die kopierte Azure Functions-URL. Klicken Sie auf **Erstellen**.

   ![Angeben der Abonnementwerte](media/event-grid-event-hubs-integration/provide-values.png)

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Führen Sie zum Abonnieren des Ereignisses den folgenden Befehl aus:

```azurecli-interactive
az eventgrid resource event-subscription create -g rgDataMigrationSample --provider-namespace Microsoft.EventHub --resource-type namespaces --resource-name <your-EventHubs-namespace> --name captureEventSub --endpoint <your-function-endpoint>
```

## <a name="run-the-app-to-generate-data"></a>Ausführen der App zum Generieren von Daten

Sie haben den Event Hub, SQL Data Warehouse, die Azure-Funktions-App und das Ereignisabonnement eingerichtet. Die Projektmappe kann nun Daten vom Event Hub zum Data Warehouse migrieren. Vor dem Ausführen einer Anwendung, die Daten für Event Hubs generiert, müssen Sie einige Werte konfigurieren.

1. Wählen Sie im Portal den Event Hub-Namespace aus. Wählen Sie **Verbindungszeichenfolgen**.

   ![Auswählen von „Verbindungszeichenfolgen“](media/event-grid-event-hubs-integration/event-hub-connection.png)

2. Wählen Sie **RootManageSharedAccessKey**.

   ![Auswählen des Schlüssels](media/event-grid-event-hubs-integration/show-root-key.png)

3. Kopieren Sie den Wert für **Verbindungszeichenfolge – Primärschlüssel**.

   ![Kopieren des Schlüssels](media/event-grid-event-hubs-integration/copy-key.png)

4. Kehren Sie zum Visual Studio-Projekt zurück. Öffnen Sie **program.cs** im Projekt „WindTurbineDataGenerator“.

5. Ersetzen Sie die zwei konstanten Werte. Verwenden Sie für **EventHubConnectionString** den kopierten Wert. Verwenden Sie für **EventHubName** den Event Hub-Namen.

   ```cs
   private const string EventHubConnectionString = "Endpoint=sb://tfdatamigratens.servicebus.windows.net/...";
   private const string EventHubName = "hubdatamigration";
   ```

6. Erstellen Sie die Projektmappe. Führen Sie die Anwendung „WindTurbineGenerator.exe“ aus. Fragen Sie nach ein paar Minuten in der Tabelle im Data Warehouse die migrierten Daten ab.

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in Event Grid finden Sie unter [Informationen zu Event Grid](overview.md).
* Eine Einführung zu Event Hubs Capture finden Sie unter [Aktivieren von Event Hubs Capture über das Azure-Portal](../event-hubs/event-hubs-capture-enable-through-portal.md).
* Weitere Informationen zum Einrichten und Ausführen des Beispiels finden Sie unter [Event Hubs Capture and Event Grid sample](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo) (Beispiel für Events Hubs Capture und Event Grid).
