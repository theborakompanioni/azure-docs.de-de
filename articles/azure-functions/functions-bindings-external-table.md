---
title: "Azure Functions – Bindungen zu externen Tabellen (Vorschau) | Microsoft-Dokumentation"
description: Verwenden von Bindungen zu externen Tabellen in Azure Functions
services: functions
documentationcenter: 
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/12/2017
ms.author: alkarche
ms.translationtype: Human Translation
ms.sourcegitcommit: 64bd7f356673b385581c8060b17cba721d0cf8e3
ms.openlocfilehash: 716438e5ea490f6716999813112305499dbe61a8
ms.contentlocale: de-de
ms.lasthandoff: 05/02/2017


---
# <a name="azure-functions-external-table-binding-preview"></a>Azure Functions – Bindungen zu externen Tabellen (Vorschau)
In diesem Artikel wird gezeigt, wie Sie in einer Funktion integrierte Bindungen verwenden können, um tabellarisch gespeicherte Daten in SaaS-Anbietern (z. B. Sharepoint, Dynamics) zu verarbeiten. Azure Functions unterstützt Eingabe- und Ausgabebindungen für externe Tabellen.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="api-connections"></a>API-Verbindungen

Für Tabellenbindungen werden externe API-Verbindungen genutzt, um Authentifizierungen bei SaaS-Anbietern anderer Hersteller vorzunehmen. 

Wenn Sie eine Bindung zuweisen, können Sie entweder eine neue API-Verbindung erstellen oder eine vorhandene API-Verbindung in derselben Ressourcengruppe verwenden.

### <a name="supported-api-connections-tables"></a>Unterstützte API-Verbindungen (Tabellen)

|Connector|Trigger|Eingabe|Ausgabe|
|:-----|:---:|:---:|:---:|
|[DB2](https://docs.microsoft.com/azure/connectors/connectors-create-api-db2)||x|x
|[Dynamics 365 for Operations](https://ax.help.dynamics.com/wiki/install-and-configure-dynamics-365-for-operations-warehousing/)||x|x
|[Dynamics 365](https://docs.microsoft.com/azure/connectors/connectors-create-api-crmonline)||x|x
|[Dynamics NAV](https://msdn.microsoft.com/library/gg481835.aspx)||x|x
|[Google Sheets](https://docs.microsoft.com/azure/connectors/connectors-create-api-googledrive)||x|x
|[Informix](https://docs.microsoft.com/azure/connectors/connectors-create-api-informix)||x|x
|[Dynamics 365 for Financials](https://docs.microsoft.com/azure/connectors/connectors-create-api-crmonline)||x|x
|[MySQL](https://docs.microsoft.com/azure/store-php-create-mysql-database)||x|x
|[Oracle-Datenbank](https://docs.microsoft.com/azure/connectors/connectors-create-api-oracledatabase)||x|x
|[Common Data Service](https://docs.microsoft.com/common-data-service/entity-reference/introduction)||x|x
|[Salesforce](https://docs.microsoft.com/azure/connectors/connectors-create-api-salesforce)||x|x
|[SharePoint](https://docs.microsoft.com/azure/connectors/connectors-create-api-sharepointonline)||x|x
|[SQL Server](https://docs.microsoft.com/azure/connectors/connectors-create-api-sqlazure)||x|x
|[Teradata](http://www.teradata.com/products-and-services/azure/products/)||x|x
|UserVoice||x|x
|Zendesk||x|x


> [!NOTE]
> Verbindungen mit externen Tabellen können auch in [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list) verwendet werden.

### <a name="creating-an-api-connection-step-by-step"></a>Erstellen eine API-Verbindung: Schritt für Schritt

1. Erstellen einer Funktion > benutzerdefinierte Funktion ![Erstellen einer benutzerdefinierten Funktion](./media/functions-bindings-storage-table/create-custom-function.jpg)
1. Szenario `Experimental`  >  `ExternalTable-CSharp` Vorlage > Erstellen einer neuen `External Table connection` 
 ![Vorlage für Tabelleneingabe auswählen](./media/functions-bindings-storage-table/create-template-table.jpg)
1. Auswählen des SaaS-Anbieters > Auswählen/Erstellen einer Verbindung ![Konfigurieren der SaaS-Verbindung](./media/functions-bindings-storage-table/authorize-API-connection.jpg)
1. Auswählen der API-Verbindung > Erstellen der Funktion ![Tabellenfunktion erstellen](./media/functions-bindings-storage-table/table-template-options.jpg)
1. Auswählen von `Integrate` > `External Table`
    1. Konfigurieren Sie die Verbindung, die für die Zieltabelle verwendet werden soll. Diese Einstellungen sind je nach SaaS-Anbieter unterschiedlich. Sie sind weiter unten unter [Einstellungen für die Datenquelle](#datasourcesettings)
 erläutert.![Tabelle konfigurieren](./media/functions-bindings-storage-table/configure-API-connection.jpg)

## <a name="usage"></a>Verwendung

In diesem Beispiel wird eine Verbindung mit einer Tabelle namens „Contact“ hergestellt, die die Spalten „Id“, „LastName“ und „FirstName“ hat. Im Code werden die in der Tabelle enthaltenen Kontaktelemente aufgelistet sowie die Vor- und Nachnamen protokolliert.

### <a name="bindings"></a>Bindungen
```json
{
  "bindings": [
    {
      "type": "manualTrigger",
      "direction": "in",
      "name": "input"
    },
    {
      "type": "apiHubTable",
      "direction": "in",
      "name": "table",
      "connection": "ConnectionAppSettingsKey",
      "dataSetName": "default",
      "tableName": "Contact",
      "entityId": "",
    }
  ],
  "disabled": false
}
```
`entityId` muss für Tabellenbindungen leer sein.

`ConnectionAppSettingsKey` kennzeichnet die App-Einstellung, in der die API-Verbindungszeichenfolge gespeichert wird. Die App-Einstellung wird automatisch erstellt, wenn Sie in der Benutzeroberfläche für „Integrieren“ eine API-Verbindung hinzufügen.

Ein tabellarischer Connector stellt Datasets bereit, und jedes Dataset enthält Tabellen. Der Name des Standarddatasets ist „default“. Die Titel für ein Dataset und eine Tabelle in verschiedene SaaS-Anbietern sind nachstehend aufgeführt:

|Connector|Datensatz|Table|
|:-----|:---|:---| 
|**SharePoint**|Website|SharePoint-Liste
|**SQL**|Datenbank|Table 
|**Google Sheet**|Spreadsheet|Worksheet 
|**Excel**|Excel-Datei|Tabelle 

<!--
See the language-specific sample that copies the input file to the output file.

* [C#](#incsharp)
* [Node.js](#innodejs)

-->
<a name="incsharp"></a>

### <a name="usage-in-c"></a>Verwendung in C# #

```cs
#r "Microsoft.Azure.ApiHub.Sdk"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.ApiHub;

//Variable name must match column type
//Variable type is dynamically bound to the incoming data
public class Contact
{
    public string Id { get; set; }
    public string LastName { get; set; }
    public string FirstName { get; set; }
}

public static async Task Run(string input, ITable<Contact> table, TraceWriter log)
{
    //Iterate over every value in the source table
    ContinuationToken continuationToken = null;
    do
    {   
        //retreive table values
        var contactsSegment = await table.ListEntitiesAsync(
            continuationToken: continuationToken);

        foreach (var contact in contactsSegment.Items)
        {   
            log.Info(string.Format("{0} {1}", contact.FirstName, contact.LastName));
        }

        continuationToken = contactsSegment.ContinuationToken;
    }
    while (continuationToken != null);
}
```

<!--
<a name="innodejs"></a>

### Usage in Node.js

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputFile = context.bindings.myInputFile;
    context.done();
};
```
-->


<a name="datasourcesettings"></a>
## Einstellungen für die Datenquelle

### <a name="sql-server"></a>SQL Server

Es folgt das Skript zum Erstellen und Auffüllen der Tabelle „Contact“. dataSetName (Name des Datasets) ist „default“.

```sql
CREATE TABLE Contact
(
    Id int NOT NULL,
    LastName varchar(20) NOT NULL,
    FirstName varchar(20) NOT NULL,
    CONSTRAINT PK_Contact_Id PRIMARY KEY (Id)
)
GO
INSERT INTO Contact(Id, LastName, FirstName)
     VALUES (1, 'Bitt', 'Prad') 
GO
INSERT INTO Contact(Id, LastName, FirstName)
     VALUES (2, 'Glooney', 'Ceorge') 
GO
```

### <a name="google-sheets"></a>Google Tabellen
Erstellen Sie in Google Docs ein Arbeitsblatt mit einer Tabelle namens `Contact`. Der Connector kann den Anzeigenamen der Tabelle nicht verwenden. Der interne Name (in Fettschrift) muss als dataSetName verwendet werden. Beispiel: `docs.google.com/spreadsheets/d/`  **`1UIz545JF_cx6Chm_5HpSPVOenU4DZh4bDxbFgJOSMz0`**  Fügen Sie die Spaltennamen `Id`, `LastName`, `FirstName` zur erste Zeile hinzu, und füllen Sie dann die nachfolgenden Zeilen mit Daten.

### <a name="salesforce"></a>Salesforce
dataSetName (Name des Datasets) ist „default“.

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

