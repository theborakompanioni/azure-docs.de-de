---
title: "Hinzufügen des Dynamics CRM Online-Connectors zu Ihren Logik-Apps | Microsoft Docs"
description: "Erstellen Sie Logik-Apps mit Azure App Service. Der Dynamics CRM Online-Verbindungsanbieter stellt eine API für die Verwendung von Entitäten unter Dynamics CRM Online bereit."
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 0dc2abef-7d2c-4a2d-87ca-fad21367d135
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 11/07/2016
ms.author: mandia
translationtype: Human Translation
ms.sourcegitcommit: 66fc8f7e1da55dbe6bb1dd8b8d6a535c498c1cf7
ms.openlocfilehash: a1ea6c09621aeeb1e98bbbf5edf1d5deb5e4b721


---
# <a name="get-started-with-the-dynamics-crm-online-connector"></a>Erste Schritte mit dem Dynamics CRM Online-Connector
Verbinden Sie sich mit Dynamics CRM Online, um z. B. einen neuen Datensatz zu erstellen oder ein Element zu aktualisieren. CRM Online ermöglicht Folgendes:

* Erstellen eines Geschäftsworkflows basierend auf den Daten, die aus CRM Online abgerufen werden. 
* Verwenden von Aktionen, die z. B. einen Datensatz löschen oder Entitäten abrufen. Diese Aktionen erhalten eine Antwort und stellen anschließend die Ausgabe anderen Aktionen zur Verfügung. Wenn ein Element in CRM aktualisiert wird, können Sie z. B. über Office 365 eine E-Mail senden.

Dieses Thema beschreibt, wie Sie den Dynamics CRM Online-Connector in einer Logik-App verwenden, und enthält eine Liste mit den Triggern und Aktionen.

> [!NOTE]
> Diese Version des Artikels gilt für die allgemein verfügbare Version von Logic Apps.
> 
> 

Weitere Informationen zu Logik-Apps finden Sie unter [Was sind Logik-Apps](../logic-apps/logic-apps-what-are-logic-apps.md) sowie unter [Erstellen einer Logik-App zum Verbinden von SaaS-Diensten](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-to-dynamics-crm-online"></a>Herstellen einer Verbindung mit Dynamics CRM Online
Damit Ihre Logik-App überhaupt auf einen Dienst zugreifen kann, muss zunächst eine *Verbindung* mit dem Dienst hergestellt werden. Eine Verbindung stellt den Kontakt zwischen einer Logik-App und einem anderen Dienst her. Wenn Sie also beispielsweise eine Verbindung mit Dynamics herstellen möchten, müssen Sie zunächst eine Dynamics CRM Online-*Verbindung* erstellen. Geben Sie zum Erstellen einer Verbindung die Anmeldeinformationen ein, mit denen Sie normalerweise auf den Dienst zugreifen, mit dem Sie eine Verbindung herstellen möchten. Geben Sie für Dynamics also die Anmeldeinformationen Ihres Dynamics CRM Online-Kontos ein, um die Verbindung zu erstellen.

### <a name="create-the-connection"></a>Erstellen der Verbindung
> [!INCLUDE [Steps to create a connection to Dynamics CRM Online Connection Provider](../../includes/connectors-create-api-crmonline.md)]
> 
> 

## <a name="use-a-trigger"></a>Verwenden eines Triggers
Ein Trigger ist ein Ereignis, mit dem ein in einer Logik-App definierter Workflow gestartet werden kann. Trigger fragen den Dienst im gewünschten Intervall und mit der gewünschten Häufigkeit ab. Weitere Informationen zu Triggern finden Sie [hier](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

1. Geben Sie in der Logik-App „dynamics“ ein, um eine Liste mit den Triggern abzurufen:  
   
    ![](./media/connectors-create-api-crmonline/dynamics-triggers.png)
2. Wählen Sie **Dynamics CRM Online – When a record is created** (Dynamics CRM Online – wenn ein Datensatz erstellt wird) aus. Falls bereits eine Verbindung vorhanden ist, wählen Sie in der Dropdownliste eine Organisation und eine Entität aus.
   
    ![](./media/connectors-create-api-crmonline/select-organization.png)
   
    Wenn Sie zur Anmeldung aufgefordert werden, geben Sie die Anmeldeinformationen ein, um die Verbindung zu erstellen. Die erforderlichen Schritte sind in diesem Thema unter [Erstellen der Verbindung](connectors-create-api-crmonline.md#create-the-connection) aufgeführt. 
   
   > [!NOTE]
   > In diesem Beispiel wird die Logik-App ausgeführt, wenn ein Datensatz erstellt wird. Fügen Sie zum Anzeigen der Ergebnisse dieses Triggers eine weitere Aktion hinzu, die Ihnen eine E-Mail sendet. Fügen Sie beispielsweise die Office 365-Aktion *E-Mail senden* hinzu, um eine E-Mail zu erhalten, wenn der neue Datensatz hinzugefügt wird. 
   > 
   > 
3. Wählen Sie die Schaltfläche **Bearbeiten** aus, und legen Sie Werte für **Häufigkeit** und **Intervall** fest. Falls die Abfrage durch den Trigger also beispielsweise alle 15 Minuten erfolgen soll, legen Sie **Häufigkeit** auf **Minute** und **Intervall** auf **15** fest. 
   
    ![](./media/connectors-create-api-crmonline/edit-properties.png)
4. Speichern Sie Ihre Änderungen. (Die Option **Speichern** befindet sich links oben auf der Symbolleiste.) Ihre Logik-App wird gespeichert und ggf. automatisch aktiviert.

## <a name="use-an-action"></a>Verwenden einer Aktion
Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. Weitere Informationen zu Aktionen finden Sie [hier](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

1. Wählen Sie das Pluszeichen aus. Es stehen mehrere Auswahlmöglichkeiten zur Verfügung: **Aktion hinzufügen**, **Bedingung hinzufügen** oder eine der Optionen unter **Mehr**.
   
    ![](./media/connectors-create-api-crmonline/add-action.png)
2. Wählen Sie **Aktion hinzufügen**aus.
3. Geben Sie im Textfeld die Zeichenfolge „dynamics“ ein, um eine Liste mit allen verfügbaren Aktionen zu erhalten.
   
    ![](./media/connectors-create-api-crmonline/dynamics-actions.png)
4. Wählen Sie in unserem Beispiel die Option **Dynamics CRM Online – Update a record** (Dynamics CRM Online – Datensatz aktualisieren) aus. Falls bereits eine Verbindung vorhanden ist, wählen Sie **Organisationsname**, **Entitätsname** und andere Eigenschaften aus:  
   
    ![](./media/connectors-create-api-crmonline/sample-action.png)
   
    Wenn Sie zur Eingabe der Verbindungsinformationen aufgefordert werden, geben Sie die Details ein, um die Verbindung zu erstellen. Die Eigenschaften werden in diesem Thema unter [Erstellen der Verbindung](connectors-create-api-crmonline.md#create-the-connection) beschrieben. 
   
   > [!NOTE]
   > In diesem Beispiel aktualisieren wir einen vorhandenen Datensatz in CRM Online. Für die Aktualisierung des Datensatzes kann die Ausgabe eines anderen Triggers verwendet werden. Fügen Sie beispielsweise den SharePoint-Trigger *When an existing item is modified* (Wenn ein vorhandenes Element geändert wird) hinzu. Fügen Sie dann die CRM Online-Aktion *Update a record* (Datensatz aktualisieren) hinzu, die den vorhandenen Datensatz in CRM Online mithilfe der SharePoint-Felder aktualisiert. 
   > 
   > 
5. Speichern Sie Ihre Änderungen. (Die Option **Speichern** befindet sich links oben auf der Symbolleiste.) Ihre Logik-App wird gespeichert und ggf. automatisch aktiviert.

## <a name="technical-details"></a>Technische Details
## <a name="triggers"></a>Trigger
| Trigger | Beschreibung |
| --- | --- |
| [When a record is created](connectors-create-api-crmonline.md#when-a-record-is-created) (Wenn ein Datensatz erstellt wird) |Löst einen Ablauf aus, wenn ein Objekt in CRM erstellt wird. |
| [When a record is updated](connectors-create-api-crmonline.md#when-a-record-is-updated) (Wenn ein Datensatz aktualisiert wird) |Löst einen Ablauf aus, wenn ein Objekt in CRM geändert wird. |
| [When a record is deleted](connectors-create-api-crmonline.md#when-a-record-is-deleted) (Wenn ein Datensatz gelöscht wird) |Löst einen Ablauf aus, wenn ein Objekt in CRM gelöscht wird. |

## <a name="actions"></a>Aktionen
| Aktion | Beschreibung |
| --- | --- |
| [List records](connectors-create-api-crmonline.md#list-records) (Datensätze auflisten) |Dieser Vorgang ruft die Datensätze für eine Entität ab. |
| [Create a new record](connectors-create-api-crmonline.md#create-a-new-record) (Neuen Datensatz erstellen) |Dieser Vorgang erstellt einen neuen Datensatz einer Entität. |
| [Get record](connectors-create-api-crmonline.md#get-record) (Datensatz abrufen) |Dieser Vorgang ruft den angegebenen Datensatz für eine Entität ab. |
| [Delete a record](connectors-create-api-crmonline.md#delete-a-record) (Datensatz löschen) |Dieser Vorgang löscht einen Datensatz aus einer Entitätssammlung. |
| [Update a record](connectors-create-api-crmonline.md#update-a-record) (Datensatz aktualisieren) |Dieser Vorgang aktualisiert einen vorhandenen Datensatz für eine Entität. |

### <a name="trigger-and-action-details"></a>Trigger- und Aktionsdetails
Dieser Abschnitt enthält spezifische Details zu den einzelnen Triggern und Aktionen. Hierzu zählen unter anderem erforderliche oder optionale Eingabeeigenschaften sowie entsprechende Ausgaben im Zusammenhang mit dem Connector.

#### <a name="when-a-record-is-created"></a>When a record is created (Wenn ein Datensatz erstellt wird)
Löst einen Ablauf aus, wenn ein Objekt in CRM erstellt wird. 

| Eigenschaftenname | Anzeigename | Beschreibung |
| --- | --- | --- |
| dataset* |Name der Organisation |Name der CRM-Organisation (beispielsweise Contoso) |
| table* |Entitätsname |Name der Entität |
| $skip |Auslassungsanzahl |Anzahl der zu überspringenden Einträge (Standardeinstellung = 0) |
| $top |Maximale Anzahl von Get-Anforderungen |Maximale Anzahl abzurufender Einträge (Standardwert: 256) |
| $filter |Filterabfrage |Eine ODATA-Filterabfrage zum Einschränken der Anzahl zurückgegebener Einträge |
| $orderby |Sortieren nach |Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
ItemsList

| Eigenschaftenname | Datentyp |
| --- | --- |
| value |array |

#### <a name="when-a-record-is-updated"></a>When a record is updated (Wenn ein Datensatz aktualisiert wird)
Löst einen Ablauf aus, wenn ein Objekt in CRM geändert wird. 

| Eigenschaftenname | Anzeigename | Beschreibung |
| --- | --- | --- |
| dataset* |Name der Organisation |Name der CRM-Organisation (beispielsweise Contoso) |
| table* |Entitätsname |Name der Entität |
| $skip |Auslassungsanzahl |Anzahl der zu überspringenden Einträge (Standardeinstellung = 0) |
| $top |Maximale Anzahl von Get-Anforderungen |Maximale Anzahl abzurufender Einträge (Standardwert: 256) |
| $filter |Filterabfrage |Eine ODATA-Filterabfrage zum Einschränken der Anzahl zurückgegebener Einträge |
| $orderby |Sortieren nach |Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
ItemsList

| Eigenschaftenname | Datentyp |
| --- | --- |
| value |array |

#### <a name="when-a-record-is-deleted"></a>When a record is deleted (Wenn ein Datensatz gelöscht wird)
Löst einen Ablauf aus, wenn ein Objekt in CRM gelöscht wird. 

| Eigenschaftenname | Anzeigename | Beschreibung |
| --- | --- | --- |
| dataset* |Name der Organisation |Name der CRM-Organisation (beispielsweise Contoso) |
| table* |Entitätsname |Name der Entität |
| $skip |Auslassungsanzahl |Anzahl der zu überspringenden Einträge (Standardeinstellung = 0) |
| $top |Maximale Anzahl von Get-Anforderungen |Maximale Anzahl abzurufender Einträge (Standardwert: 256) |
| $filter |Filterabfrage |Eine ODATA-Filterabfrage zum Einschränken der Anzahl zurückgegebener Einträge |
| $orderby |Sortieren nach |Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
ItemsList

| Eigenschaftenname | Datentyp |
| --- | --- |
| value |Array |

#### <a name="list-records"></a>List records (Datensätze auflisten)
Dieser Vorgang ruft die Datensätze für eine Entität ab. 

| Eigenschaftenname | Anzeigename | Beschreibung |
| --- | --- | --- |
| dataset* |Name der Organisation |Name der CRM-Organisation (beispielsweise Contoso) |
| table* |Entitätsname |Name der Entität |
| $skip |Auslassungsanzahl |Anzahl der zu überspringenden Einträge (Standardeinstellung = 0) |
| $top |Maximale Anzahl von Get-Anforderungen |Maximale Anzahl abzurufender Einträge (Standardwert: 256) |
| $filter |Filterabfrage |Eine ODATA-Filterabfrage zum Einschränken der Anzahl zurückgegebener Einträge |
| $orderby |Sortieren nach |Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
ItemsList

| Eigenschaftenname | Datentyp |
| --- | --- |
| value |Array |

#### <a name="create-a-new-record"></a>Create a new record (Neuen Datensatz erstellen)
Dieser Vorgang erstellt einen neuen Datensatz einer Entität. 

| Eigenschaftenname | Anzeigename | Beschreibung |
| --- | --- | --- |
| dataset* |Name der Organisation |Name der CRM-Organisation (beispielsweise Contoso) |
| table* |Entitätsname |Name der Entität |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Keine

#### <a name="get-record"></a>Get record (Datensatz abrufen)
Dieser Vorgang ruft den angegebenen Datensatz für eine Entität ab. 

| Eigenschaftenname | Anzeigename | Beschreibung |
| --- | --- | --- |
| dataset* |Name der Organisation |Name der CRM-Organisation (beispielsweise Contoso) |
| table* |Entitätsname |Name der Entität |
| id* |Item identifier (Elementbezeichner) |Bezeichner für den Datensatz |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Keine

#### <a name="delete-a-record"></a>Delete a record (Datensatz löschen)
Dieser Vorgang löscht einen Datensatz aus einer Entitätssammlung. 

| Eigenschaftenname | Anzeigename | Beschreibung |
| --- | --- | --- |
| dataset* |Name der Organisation |Name der CRM-Organisation (beispielsweise Contoso) |
| table* |Entitätsname |Name der Entität |
| id* |Item identifier (Elementbezeichner) |Bezeichner für den Datensatz |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

#### <a name="update-a-record"></a>Update a record (Datensatz aktualisieren)
Dieser Vorgang aktualisiert einen vorhandenen Datensatz für eine Entität. 

| Eigenschaftenname | Anzeigename | Beschreibung |
| --- | --- | --- |
| dataset* |Name der Organisation |Name der CRM-Organisation (beispielsweise Contoso) |
| table* |Entitätsname |Name der Entität |
| id* |Record identifier (Datensatzbezeichner) |Bezeichner für den Datensatz |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Keine

## <a name="http-responses"></a>HTTP-Antworten
Von den angegebenen Aktionen und Triggern können folgende HTTP-Statuscodes zurückgegeben werden: 

| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 202 |Zulässig |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler. |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="next-steps"></a>Nächste Schritte
[Erstellen Sie eine Logik-App](../logic-apps/logic-apps-create-a-logic-app.md). Informieren Sie sich in unserer [API-Liste](apis-list.md)über die anderen verfügbaren Connectors für Logik-Apps.




<!--HONumber=Jan17_HO3-->


