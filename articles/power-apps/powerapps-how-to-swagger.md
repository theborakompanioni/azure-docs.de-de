<properties
	pageTitle="Gewusst wie: Anpassen der Swagger-Definition für PowerApps und logische Abläufe | Microsoft Azure"
	description="Informieren Sie sich über die Schemaerweiterungen, die für Swagger für die Zusammenarbeit mit PowerApps und logischen Abläufen benötigt werden."
	services=""
    suite="powerapps"
	documentationCenter="" 
	authors="sunaysv"
	manager="erikre"
	editor=""/>

<tags
   ms.service="powerapps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="07/12/2016"
   ms.author="mandia"/>


# Anpassen der Swagger-Definition für PowerApps und logische Abläufe

>[AZURE.IMPORTANT] Dieses Thema finden Sie nun auf „powerapps.microsoft.com“ unter [Customize your Swagger definition for PowerApps and Flows](https://powerapps.microsoft.com/tutorials/customapi-how-to-swagger/) (Anpassen der Swagger-Definition für PowerApps und Flow). Die aktuelle Version finden Sie in PowerApps. Dieser Azure-Link wird archiviert.


## Erstellen der Swagger 2.0-API-Definition für die API

Informationen dazu, wie Sie Swagger Ihrer WebAPI hinzufügen, finden Sie unter [Swashbuckle][1].

## Schemaerweiterungen
Neben den standardmäßigen Swagger-Spezifikationen sind einige zusätzliche Swagger-Erweiterungen verfügbar, wenn Sie eine benutzerdefinierte API für PowerApps und logische Abläufe erstellen. Diese Erweiterungen werden in diesem Abschnitt aufgeführt und beschrieben.

##### x-ms-summary
Eine Zeichenfolge, die die Anzeigenamen für Entitäten beschreibt, für die in der Swagger-Spezifikation kein `summary`-Feld definiert ist. **Parameter names** ist ein Beispiel.

##### x-ms-visibility
Dieser Wert beschreibt, ob die Entität im Logic Flow-Designer angezeigt wird. Folgende Werte sind verfügbar:

- „none“ (Standardwert)
- „advanced“
- „internal“: Diese Vorgänge werden im Logic Flow-Designer nicht angezeigt.

Wenn Vorgänge als „important“ gekennzeichnet sind, ist zu erwarten, dass sie auf dem Logic Flow-Client hervorgehoben werden.

##### x-ms-trigger
Definiert, ob der Vorgang im logischen Ablauf als Trigger verwendet werden kann. Folgende Optionen sind verfügbar:
	
- none (Standardwert): Der Vorgang kann nicht als Trigger verwendet werden.
- single: Der Vorgang kann auch als Trigger verwendet werden.
- batched: Der Vorgang kann als Trigger verwendet werden. Darüber hinaus antwortet der Vorgang mit einem JSON-„Array“ von Objekten, und im logischen Ablauf wird ein Trigger für jedes Element im Array ausgelöst.


##### x-ms-dynamic-values
Dies ist ein Hinweis für den Logic Flow-Designer, dass die API eine Liste mit dynamisch zulässigen Werten für den Parameter bereitstellt. Der Logic Flow-Designer kann einen Vorgang entsprechend der Definition durch den Wert dieses Felds aufrufen und die möglichen Werte aus dem Ergebnis extrahieren. Im Logic Flow-Designer können diese Werte dann als Optionen für den Endbenutzer angezeigt werden.

Der Wert ist ein Objekt, das die folgenden Eigenschaften enthält:
	
- `operationId`: Eine Zeichenfolge, die dem operationId-Wert für den aufgerufenen Vorgang entspricht.
- `parameters`: Ein Objekt, mit dessen Eigenschaften die für den Vorgang erforderlichen Parameter definiert werden.
- `value-collection`: Eine Pfadzeichenfolge, die als Array von Objekten in der Antwortnutzlast ausgewertet wird.
- `value-path`: Eine Pfadzeichenfolge im Objekt innerhalb von „value-collection“, die auf den Wert für den Parameter verweist.
- `value-title`: Eine Pfadzeichenfolge im Objekt innerhalb von „value-collection“, die auf eine Beschreibung für den Wert verweist.


Beispiel:

```javascript
"/api/tables/{table}/items": {
  "post": {
    "operationId": "TableData_CreateItem",
    "summary": "Create an object in {Salesforce}",
    "parameters": [
      {
        "name": "table",
        "x-ms-summary": "Object Type",
        "x-ms-dynamic-values": {
          "operationId": "TableMetadata_ListTables",      // operation that needs to be invoked
          "parameters": { },                              // parameters for the above operation, if any
          "value-collection": "values",                   // field that contains the collection
          "value-path": "Name",                           // field that contains the value
          "value-title": "DisplayName"                    // field that contains a display name for the value
      }
      // ...
    ]
    // ...
  }
  // ...
}
```

In diesem Beispiel wird per Swagger der `TableData_CreateItem`-Vorgang definiert, mit dem ein neues Objekt in Salesforce erstellt wird.

Salesforce verfügt über zahlreiche integrierte Objekte. `x-ms-dynamic-values` wird hier verwendet, damit im Designer die Liste der integrierten Salesforce-Objekte ermittelt werden kann. Die Liste wird durch das Aufrufen von `TableMetadata_ListTables` abgerufen.

##### x-ms-dynamic-schema
Dies ist ein Hinweis für den Logic Flow-Designer, dass das Schema für den Parameter (oder die Antwort) dynamisch ist. Der Designer kann einen Vorgang entsprechend der Definition durch den Wert dieses Felds aufrufen und das Schema dynamisch ermitteln. Dann kann eine entsprechende Benutzeroberfläche angezeigt werden, in der Eingaben des Benutzers erfolgen oder verfügbare Felder angezeigt werden können.

Beispiel:

```javascript
{
  "name": "item",
  "in": "body",
  "required": true,
  "x-ms-dynamic-schema": {
    "operationId": "Metadata_GetTableSchema",
    "parameters": {
      "tablename": "{table}"              // the value that the user has selected from the above parameter
    },
    "value-path": "Schema"                // the field that contains the JSON schema
  }
},
```

Dies ist nützlich in Szenarios, in denen die Eingaben für einen Vorgang dynamisch vorgenommen werden. Betrachten Sie beispielsweise das SQL-Beispiel. Das Schema jeder Tabelle ist unterschiedlich. Wenn ein Benutzer eine bestimmte Tabelle auswählt, muss der Logic Flow-Designer daher die Struktur der Tabelle ermitteln, sodass die Spaltennamen angezeigt werden können. Wenn die Swagger-Definition in diesem Kontext über `x-ms-dynamic-schema` verfügt, wird der entsprechende Vorgang zum Abrufen des Schemas aufgerufen.


<!--Reference links in article-->
[1]: https://github.com/domaindrivendev/Swashbuckle/blob/master/README.md

<!---HONumber=AcomDC_0713_2016-->