---
title: "Hinzufügen des Azure SQL-Datenbank-Connectors in Ihren Logik-Apps | Microsoft Docs"
description: "Übersicht über den Azure SQL-Datenbank-Connector mit REST-API-Parametern"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: d8a319d0-e4df-40cf-88f0-29a6158c898c
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/18/2016
ms.author: mandia
translationtype: Human Translation
ms.sourcegitcommit: 66fc8f7e1da55dbe6bb1dd8b8d6a535c498c1cf7
ms.openlocfilehash: ce3a622db8667df8b3f1d1391c2aa0d7e1e012a5


---
# <a name="get-started-with-the-azure-sql-database-connector"></a>Erste Schritte mit dem Azure SQL-Datenbank-Connector
Mithilfe des Azure SQL-Datenbank-Connectors können Sie für Ihre Organisation Workflows zur Verwaltung von Tabellendaten erstellen. 

SQL-Datenbank ermöglicht Folgendes:

* Erstellen Sie Ihren Workflow, indem Sie einer Kundendatenbank einen neuen Kunden hinzufügen oder einen Auftrag in einer Auftragsdatenbank aktualisieren.
* Verwenden Sie Aktionen, um eine Datenzeile abzurufen, eine neue Zeile einzufügen oder Löschvorgänge auszuführen. Wenn also etwa ein Datensatz in Dynamics CRM Online erstellt wird (Trigger), soll eine Zeile in eine Azure SQL-Datenbank eingefügt werden (Aktion). 

Dieses Thema beschreibt, wie Sie den SQL-Datenbank-Connector in einer Logik-App verwenden, und enthält eine Liste mit Aktionen.

> [!NOTE]
> Diese Version des Artikels gilt für die allgemein verfügbare Version von Logic Apps. 
> 
> 

Weitere Informationen zu Logik-Apps finden Sie unter [Was sind Logik-Apps](../logic-apps/logic-apps-what-are-logic-apps.md) sowie unter [Erstellen einer Logik-App zum Verbinden von SaaS-Diensten](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-to-azure-sql-database"></a>Herstellen einer Verbindung mit Azure SQL-Datenbank
Damit Ihre Logik-App überhaupt auf einen Dienst zugreifen kann, muss zunächst eine *Verbindung* mit dem Dienst hergestellt werden. Eine Verbindung stellt den Kontakt zwischen einer Logik-App und einem anderen Dienst her. Für die Verbindungsherstellung mit SQL-Datenbank erstellen Sie beispielsweise zuerst eine SQL-Datenbank-*Verbindung*. Hierzu geben Sie die Anmeldeinformationen ein, mit denen Sie normalerweise auf den Dienst zugreifen, mit dem Sie eine Verbindung herstellen möchten. Geben Sie in SQL-Datenbank also Ihre SQL-Datenbank-Anmeldeinformationen ein, um die Verbindung zu erstellen. 

#### <a name="create-the-connection"></a>Erstellen der Verbindung
> [!INCLUDE [Create the connection to SQL Azure](../../includes/connectors-create-api-sqlazure.md)]
> 
> 

## <a name="use-a-trigger"></a>Verwenden eines Triggers
Dieser Connector verfügt über keine Trigger. Verwenden Sie andere Trigger, um die Logik-App zu starten – beispielsweise einen Wiederholungstrigger, einen HTTP-Webhook-Trigger oder einen Trigger eines anderen Connectors. Unter [Erstellen einer Logik-App zum Verbinden von SaaS-Diensten](../logic-apps/logic-apps-create-a-logic-app.md) finden Sie ein Beispiel.

## <a name="use-an-action"></a>Verwenden einer Aktion
Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. Weitere Informationen zu Aktionen finden Sie [hier](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

1. Wählen Sie das Pluszeichen aus. Es stehen mehrere Auswahlmöglichkeiten zur Verfügung: **Aktion hinzufügen**, **Bedingung hinzufügen** oder eine der Optionen unter **Mehr**.
   
    ![](./media/connectors-create-api-sqlazure/add-action.png)
2. Wählen Sie **Aktion hinzufügen**aus.
3. Geben Sie im Textfeld die Zeichenfolge „sql“ ein, um eine Liste mit allen verfügbaren Aktionen zu erhalten.
   
    ![](./media/connectors-create-api-sqlazure/sql-1.png) 
4. In unserem Beispiel wählen wir **SQL Server - Get row** (SQL Server – Zeile abrufen) aus. Falls bereits eine Verbindung vorhanden ist, wählen Sie in der Dropdownliste den **Tabellennamen** aus, und geben Sie die **Zeilen-ID** ein, die zurückgegeben werden soll.
   
    ![](./media/connectors-create-api-sqlazure/sample-table.png)
   
    Wenn Sie zur Eingabe der Verbindungsinformationen aufgefordert werden, geben Sie die Details ein, um die Verbindung zu erstellen. Die Eigenschaften werden in diesem Thema unter [Erstellen der Verbindung](connectors-create-api-sqlazure.md#create-the-connection) beschrieben. 
   
   > [!NOTE]
   > In diesem Beispiel geben wir eine Zeile aus einer Tabelle zurück. Fügen Sie zum Anzeigen der Daten aus dieser Zeile eine weitere Aktion hinzu, die eine Datei mit den Feldern aus der Tabelle erstellt. Fügen Sie beispielsweise eine OneDrive-Aktion hinzu, die auf der Grundlage der Felder „FirstName“ und „LastName“ eine neue Datei im Cloudspeicherkonto erstellt. 
   > 
   > 
5. Speichern Sie Ihre Änderungen. (Die Option **Speichern** befindet sich links oben auf der Symbolleiste.) Ihre Logik-App wird gespeichert und ggf. automatisch aktiviert.

## <a name="technical-details"></a>Technische Details
## <a name="sql-database-actions"></a>SQL-Datenbank-Aktion
Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. Der SQL-Datenbank-Connector beinhaltet die folgenden Aktionen. 

| Aktion | Beschreibung |
| --- | --- |
| [ExecuteProcedure](connectors-create-api-sqlazure.md#execute-stored-procedure) |Führt eine gespeicherte Prozedur in SQL aus. |
| [GetRow](connectors-create-api-sqlazure.md#get-row) |Ruft eine einzelne Zeile aus einer SQL-Tabelle ab. |
| [GetRows](connectors-create-api-sqlazure.md#get-rows) |Ruft Zeilen aus einer SQL-Tabelle ab. |
| [InsertRow](connectors-create-api-sqlazure.md#insert-row) |Fügt eine neue Zeile in eine SQL-Tabelle ein. |
| [DeleteRow](connectors-create-api-sqlazure.md#delete-row) |Löscht eine Zeile aus einer SQL-Tabelle. |
| [GetTables](connectors-create-api-sqlazure.md#get-tables) |Ruft Tabellen aus einer SQL-Datenbank ab. |
| [UpdateRow](connectors-create-api-sqlazure.md#update-row) |Aktualisiert eine vorhandene Zeile in einer SQL-Tabelle. |

### <a name="action-details"></a>Aktionsdetails
Dieser Abschnitt enthält spezifische Details zu den einzelnen Aktionen. Hierzu zählen unter anderem erforderliche oder optionale Eingabeeigenschaften sowie entsprechende Ausgaben im Zusammenhang mit dem Connector.

#### <a name="execute-stored-procedure"></a>Gespeicherte Prozedur ausführen
Führt eine gespeicherte Prozedur in SQL aus.  

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| procedure* |Name der Prozedur |Der Name der gespeicherten Prozedur, die ausgeführt werden soll. |
| parameters* |Eingabeparameter |Die Parameter sind dynamisch und basieren auf der gewählten gespeicherten Prozedur. <br/><br/> Wählen Sie bei Verwendung der Adventure Works-Beispieldatenbank beispielsweise die gespeicherte Prozedur *ufnGetCustomerInformation* aus. Der Eingabeparameter **Kunden-ID** wird angezeigt. Geben Sie „6“ oder eine der anderen Kunden-IDs ein. |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
ProcedureResult: Enthält das Ergebnis der Ausführung der gespeicherten Prozedur.

| Eigenschaftenname | Datentyp | Beschreibung |
| --- | --- | --- |
| OutputParameters |objekt |Ausgabeparameterwerte |
| ReturnCode |ganze Zahl |Rückgabecode einer Prozedur |
| ResultSets |objekt |Resultsets |

#### <a name="get-row"></a>Zeile abrufen
Ruft eine einzelne Zeile aus einer SQL-Tabelle ab.  

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| table* |Tabellenname |Name der SQL-Tabelle |
| id* |Zeilen-ID |Eindeutiger Bezeichner der abzurufenden Zeile |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Item

| Eigenschaftenname | Datentyp |
| --- | --- |
| ItemInternalId |string |

#### <a name="get-rows"></a>Zeilen abrufen
Ruft Zeilen aus einer SQL-Tabelle ab.  

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| table* |Tabellenname |Name der SQL-Tabelle |
| $skip |Auslassungsanzahl |Anzahl der zu überspringenden Einträge (Standardeinstellung = 0) |
| $top |Maximale Anzahl von Get-Anforderungen |Maximale Anzahl abzurufender Einträge (Standardeinstellung = 256) |
| $filter |Filterabfrage |Eine ODATA-Filterabfrage zum Einschränken der Anzahl der Einträge |
| $orderby |Sortieren nach |Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
ItemsList

| Eigenschaftenname | Datentyp |
| --- | --- |
| value |array |

#### <a name="insert-row"></a>Zeile einfügen
Fügt eine neue Zeile in eine SQL-Tabelle ein.  

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| table* |Tabellenname |Name der SQL-Tabelle |
| item* |Zeile |In der angegebenen SQL-Tabelle einzufügende Zeile |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Item

| Eigenschaftenname | Datentyp |
| --- | --- |
| ItemInternalId |string |

#### <a name="delete-row"></a>Zeile löschen
Löscht eine Zeile aus einer SQL-Tabelle.  

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| table* |Tabellenname |Name der SQL-Tabelle |
| id* |Zeilen-ID |Eindeutiger Bezeichner der zu löschenden Zeile |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Keine

#### <a name="get-tables"></a>Tabellen abrufen
Ruft Tabellen aus einer SQL-Datenbank ab.  

Es gibt keine Parameter für diesen Aufruf. 

##### <a name="output-details"></a>Ausgabedetails
TablesList

| Eigenschaftenname | Datentyp |
| --- | --- |
| value |array |

#### <a name="update-row"></a>Zeile aktualisieren
Aktualisiert eine vorhandene Zeile in einer SQL-Tabelle.  

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| table* |Tabellenname |Name der SQL-Tabelle |
| id* |Zeilen-ID |Eindeutiger Bezeichner der zu aktualisierenden Zeile |
| item* |Zeile |Zeile mit aktualisierten Werten |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Item

| Eigenschaftenname | Datentyp |
| --- | --- |
| ItemInternalId |string |

### <a name="http-responses"></a>HTTP-Antworten
Das Aufrufen der verschiedenen Aktionen löst unter Umständen bestimmte Antworten aus. Diese Antworten sind in der folgenden Tabelle aufgeführt und beschrieben:  

| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 202 |Zulässig |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler ist aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="next-steps"></a>Nächste Schritte
[Erstellen Sie eine Logik-App](../logic-apps/logic-apps-create-a-logic-app.md). Informieren Sie sich in unserer [API-Liste](apis-list.md)über die anderen verfügbaren Connectors für Logik-Apps.




<!--HONumber=Feb17_HO2-->


