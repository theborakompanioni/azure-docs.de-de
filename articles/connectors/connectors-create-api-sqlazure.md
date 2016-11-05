---
title: Hinzufügen des Azure SQL-Datenbank-Connectors in Ihren Logik-Apps | Microsoft Docs
description: Übersicht über den Azure SQL-Datenbank-Connector mit REST-API-Parametern
services: ''
documentationcenter: ''
author: MandiOhlinger
manager: erikre
editor: ''
tags: connectors

ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2016
ms.author: mandia

---
# Erste Schritte mit dem Azure SQL-Datenbank-Connector
Mithilfe des Azure SQL-Datenbank-Connectors können Sie für Ihre Organisation Workflows zur Verwaltung von Tabellendaten erstellen.

SQL-Datenbank ermöglicht Folgendes:

* Erstellen Sie Ihren Workflow, indem Sie einer Kundendatenbank einen neuen Kunden hinzufügen oder einen Auftrag in einer Auftragsdatenbank aktualisieren.
* Verwenden Sie Aktionen, um eine Datenzeile abzurufen, eine neue Zeile einzufügen oder Löschvorgänge auszuführen. Wenn also etwa ein Datensatz in Dynamics CRM Online erstellt wird (Trigger), soll eine Zeile in eine Azure SQL-Datenbank eingefügt werden (Aktion).

Dieses Thema beschreibt, wie Sie den SQL-Datenbank-Connector in einer Logik-App verwenden, und enthält eine Liste mit den Aktionen.

> [!NOTE]
> Diese Version des Artikels gilt für die allgemein verfügbare Version von Logic Apps.
> 
> 

Weitere Informationen zu Logic Apps finden Sie unter [Was sind Logik-Apps](../app-service-logic/app-service-logic-what-are-logic-apps.md) sowie unter [Erstellen einer Logik-App zum Verbinden von SaaS-Diensten](../app-service-logic/app-service-logic-create-a-logic-app.md).

## Herstellen einer Verbindung mit Azure SQL-Datenbank
Damit Ihre Logik-App überhaupt auf einen Dienst zugreifen kann, muss zunächst eine *Verbindung* mit dem Dienst hergestellt werden. Eine Verbindung stellt den Kontakt zwischen einer Logik-App und einem anderen Dienst her. Für die Verbindungsherstellung mit SQL-Datenbank erstellen Sie beispielsweise zuerst eine *SQL-Datenbank-Verbindung*. Hierzu geben Sie die Anmeldeinformationen ein, mit denen Sie normalerweise auf den Dienst zugreifen, mit dem Sie eine Verbindung herstellen möchten. Geben Sie in SQL-Datenbank also Ihre SQL-Datenbank-Anmeldeinformationen ein, um die Verbindung zu erstellen.

#### Erstellen der Verbindung
> [!INCLUDE [Erstellen der Verbindung mit SQL Azure](../../includes/connectors-create-api-sqlazure.md)]
> 
> 

## Verwenden eines Triggers
Dieser Connector verfügt über keine Trigger. Verwenden Sie andere Trigger, um die Logik-App zu starten – beispielsweise einen Wiederholungstrigger, einen HTTP-Webhook-Trigger oder einen Trigger eines anderen Connectors. Ein Beispiel finden Sie unter [Erstellen einer Logik-App zum Verbinden von SaaS-Diensten](../app-service-logic/app-service-logic-create-a-logic-app.md).

## Verwenden einer Aktion
Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. Weitere Informationen zu Aktionen finden Sie [hier](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

1. Wählen Sie das Pluszeichen aus. Es stehen mehrere Auswahlmöglichkeiten zur Verfügung: **Aktion hinzufügen**, **Bedingung hinzufügen** oder eine der Optionen unter **Mehr**.
   
    ![](./media/connectors-create-api-sqlazure/add-action.png)
2. Wählen Sie **Aktion hinzufügen** aus.
3. Geben Sie im Textfeld die Zeichenfolge „sql“ ein, um eine Liste mit allen verfügbaren Aktionen zu erhalten.
   
    ![](./media/connectors-create-api-sqlazure/sql-1.png)
4. In unserem Beispiel wählen wir **SQL Server - Get row** (SQL Server – Zeile abrufen) aus. Falls bereits eine Verbindung vorhanden ist, wählen Sie in der Dropdownliste den Tabellennamen aus, und geben Sie die **Zeilen-ID** ein, die zurückgegeben werden soll.
   
    ![](./media/connectors-create-api-sqlazure/sample-table.png)
   
    Wenn Sie zur Eingabe der Verbindungsinformationen aufgefordert werden, geben Sie die Details ein, um die Verbindung zu erstellen. Die Eigenschaften werden in diesem Thema unter [Erstellen der Verbindung](connectors-create-api-sqlazure.md#create-the-connection) beschrieben.
   
   > [!NOTE]
   > In diesem Beispiel geben wir eine Zeile aus einer Tabelle zurück. Fügen Sie zum Anzeigen der Daten aus dieser Zeile eine weitere Aktion hinzu, die eine Datei mit den Feldern aus der Tabelle erstellt. Fügen Sie beispielsweise eine OneDrive-Aktion hinzu, die auf der Grundlage der Felder „FirstName“ und „LastName“ eine neue Datei im Cloudspeicherkonto erstellt.
   > 
   > 
5. Speichern Sie Ihre Änderungen. (Die Option **Speichern** befindet sich links oben auf der Symbolleiste.) Ihre Logik-App wird gespeichert und ggf. automatisch aktiviert.

## Technische Details
## SQL-Datenbank-Aktion
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

### Aktionsdetails
Dieser Abschnitt enthält spezifische Details zu den einzelnen Aktionen. Hierzu zählen unter anderem erforderliche oder optionale Eingabeeigenschaften sowie entsprechende Ausgaben im Zusammenhang mit dem Connector.

#### Gespeicherte Prozedur ausführen
Führt eine gespeicherte Prozedur in SQL aus.

| Eigenschaftenname | Anzeigename | Beschreibung |
| --- | --- | --- |
| procedure* |Name der Prozedur |Der Name der gespeicherten Prozedur, die ausgeführt werden soll. |
| parameters* |Eingabeparameter |Die Parameter sind dynamisch und basieren auf der gewählten gespeicherten Prozedur. <br/><br/> Wählen Sie bei Verwendung der Adventure Works-Beispieldatenbank beispielsweise die gespeicherte Prozedur *ufnGetCustomerInformation* aus. Der Eingabeparameter **Kunden-ID** wird angezeigt. Geben Sie „6“ oder eine der anderen Kunden-IDs ein. |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### Ausgabedetails
ProcedureResult: Enthält das Ergebnis der Ausführung der gespeicherten Prozedur.

| Eigenschaftenname | Datentyp | Beschreibung |
| --- | --- | --- |
| OutputParameters |Objekt |Ausgabeparameterwerte |
| ReturnCode |integer |Rückgabecode einer Prozedur |
| ResultSets |Objekt |Resultsets |

#### Zeile abrufen
Ruft eine einzelne Zeile aus einer SQL-Tabelle ab.

| Eigenschaftenname | Anzeigename | Beschreibung |
| --- | --- | --- |
| table* |Tabellenname |Name der SQL-Tabelle |
| id* |Zeilen-ID |Eindeutiger Bezeichner der abzurufenden Zeile |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### Ausgabedetails
Item

| Eigenschaftenname | Datentyp |
| --- | --- |
| ItemInternalId |string |

#### Zeilen abrufen
Ruft Zeilen aus einer SQL-Tabelle ab.

| Eigenschaftenname | Anzeigename | Beschreibung |
| --- | --- | --- |
| table* |Tabellenname |Name der SQL-Tabelle |
| $skip |Auslassungsanzahl |Anzahl der zu überspringenden Einträge (Standardeinstellung = 0) |
| $top |Maximale Anzahl von Get-Anforderungen |Maximale Anzahl abzurufender Einträge (Standardeinstellung = 256) |
| $filter |Filterabfrage |Eine ODATA-Filterabfrage zum Einschränken der Anzahl der Einträge |
| $orderby |Sortieren nach |Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### Ausgabedetails
ItemsList

| Eigenschaftenname | Datentyp |
| --- | --- |
| value |array |

#### Zeile einfügen
Fügt eine neue Zeile in eine SQL-Tabelle ein.

| Eigenschaftenname | Anzeigename | Beschreibung |
| --- | --- | --- |
| table* |Tabellenname |Name der SQL-Tabelle |
| item* |Zeile |In der angegebenen SQL-Tabelle einzufügende Zeile |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### Ausgabedetails
Item

| Eigenschaftenname | Datentyp |
| --- | --- |
| ItemInternalId |string |

#### Zeile löschen
Löscht eine Zeile aus einer SQL-Tabelle.

| Eigenschaftenname | Anzeigename | Beschreibung |
| --- | --- | --- |
| table* |Tabellenname |Name der SQL-Tabelle |
| id* |Zeilen-ID |Eindeutiger Bezeichner der zu löschenden Zeile |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### Ausgabedetails
Keine.

#### Tabellen abrufen
Ruft Tabellen aus einer SQL-Datenbank ab.

Es gibt keine Parameter für diesen Aufruf.

##### Ausgabedetails
TablesList

| Eigenschaftenname | Datentyp |
| --- | --- |
| value |array |

#### Zeile aktualisieren
Aktualisiert eine vorhandene Zeile in einer SQL-Tabelle.

| Eigenschaftenname | Anzeigename | Beschreibung |
| --- | --- | --- |
| table* |Tabellenname |Name der SQL-Tabelle |
| id* |Zeilen-ID |Eindeutiger Bezeichner der zu aktualisierenden Zeile |
| item* |Zeile |Zeile mit aktualisierten Werten |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### Ausgabedetails
Item

| Eigenschaftenname | Datentyp |
| --- | --- |
| ItemInternalId |string |

### HTTP-Antworten
Das Aufrufen der verschiedenen Aktionen löst unter Umständen bestimmte Antworten aus. Diese Antworten sind in der folgenden Tabelle aufgeführt und beschrieben:

| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 202 |Zulässig |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler ist aufgetreten |
| die Standardeinstellung |Fehler beim Vorgang. |

## Nächste Schritte
[Erstellen Sie eine Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md). Informieren Sie sich in unserer [API-Liste](apis-list.md) über die anderen verfügbaren Connectors für Logic Apps.

<!---HONumber=AcomDC_0727_2016-->