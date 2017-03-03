---
title: "Hinzufügen des Dynamics 365 (online)-Connectors zu Azure Logic Apps | Microsoft-Dokumentation"
description: "Erstellen Sie Logik-Apps mit Azure App Service. Der Dynamics 365 (online)-Verbindungsanbieter stellt eine API für die Verwendung von Entitäten unter Dynamics 365 (online) bereit."
services: logic-apps
cloud: Azure Stack
documentationcenter: 
author: Mattp123
manager: anneta
ms.assetid: 0dc2abef-7d2c-4a2d-87ca-fad21367d135
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: matp
translationtype: Human Translation
ms.sourcegitcommit: fa426f474f4efd4023da5dfd9954dacf96f635ab
ms.openlocfilehash: 99d5379ad1e6965dd9ed88de456cc850d7e40d5a
ms.lasthandoff: 02/14/2017


---
# <a name="create-a-logic-app-with-the-dynamics-365-connector"></a>Erstellen einer Logik-App mit dem Dynamics 365-Connector

Mit Logic Apps können Sie eine Verbindung mit Dynamics 365 (online) herstellen und praktische Geschäftsflows erstellen, die neue Datensätze erstellen, Elemente aktualisieren oder eine Liste mit Datensätzen zurückgeben. Der Dynamics 365-Connector bietet folgende Möglichkeiten:

* Erstellen eines Geschäftsflows auf der Grundlage der Daten, die Sie von Dynamics 365 (online) erhalten.
* Verwenden von Aktionen, die eine Antwort generieren, und anschließendes Verfügbarmachen der Ausgabe für andere Aktionen. Wenn also beispielsweise ein Element in Dynamics 365 (online) aktualisiert wird, können Sie über Office 365 eine E-Mail senden.

In diesem Thema erfahren Sie, wie Sie eine Logik-App erstellen, die eine Aufgabe in Dynamics 365 erstellt, sobald in Dynamics 365 ein neuer Lead erstellt wird.

## <a name="prerequisites"></a>Voraussetzungen
* Ein Azure-Konto.
* Eine Dynamics 365 (online)-Konto.

## <a name="walkthrough-create-a-task-whenever-a-new-lead-is-created-in-dynamics-365"></a>Exemplarische Vorgehensweise: Erstellen einer Aufgabe, wenn in Dynamics 365 ein neuer Lead erstellt wird
1.    [Melden Sie sich bei Azure an.](https://portal.azure.com)
2.    Geben Sie *Logic Apps* in das Feld **Suche** ein, und drücken Sie anschließend die EINGABETASTE.
3.    Klicken Sie im Bereich des Logic Apps-Diensts auf **Hinzufügen**.

  ![Hinzufügen einer Logik-App](./media/connectors-create-api-crmonline/add-logic-app.png)

4.    Füllen Sie zum Erstellen des Logik-App-Objekts die Felder **Name**, **Abonnement**, **Ressourcengruppe** und **Standort** aus, und klicken Sie anschließend auf **Erstellen**.

5.    Wählen Sie die neue Logik-App aus. Klicken Sie auf **Aktualisieren**, wenn Sie die Benachrichtigung **Bereitstellung erfolgreich** erhalten.

6.    Klicken Sie unter den Entwicklungstools auf **Logik-App-Designer** und anschließend in der Liste mit den verfügbaren Vorlagen auf **Leere LogicApp**.

7.    Geben Sie *Dynamics 365* ein. Die Liste enthält mehrere Dynamics 365-Trigger. Klicken Sie auf **Dynamics 365 – Wenn ein Datensatz erstellt wird**.

8.    Melden Sie sich bei Dynamics 365 an, falls Sie dazu aufgefordert werden.

9.    Geben Sie unter den Triggerdetails folgende Informationen ein:

  * **Name der Organisation**: Wählen Sie die Dynamics 365-Instanz aus, die die Logik-App überwachen soll.

  * **Entitätsname**: Wählen Sie die zu überwachende Entität aus. Diese fungiert als Trigger für die Initiierung der Logik-App. In dieser exemplarischen Vorgehensweise wird **Leads** ausgewählt.

  * **Wie oft möchten Sie auf Elemente prüfen?**: Mit diesen Werten legen Sie fest, wie oft die Logik-App nach Trigger-bezogenen Aktualisierungen suchen soll. Standardmäßig findet alle drei Minuten eine Prüfung auf Aktualisierungen statt.

    * **Häufigkeit**: Wählen Sie Sekunden, Minuten, Stunden oder Tage aus.

    * **Intervall**: Geben Sie die Anzahl von Sekunden, Minuten, Stunden oder Tagen bis zur nächsten Überprüfung ein.

    ![Triggerdetails der Logik-App](./media/connectors-create-api-crmonline/trigger-details.png)

10.    Klicken Sie auf **Neuer Schritt** und anschließend auf **Aktion hinzufügen**.

11.    Geben Sie *Dynamics 365* ein, und klicken Sie in der Liste auf **Dynamics 365 – Neuen Datensatz erstellen**.

12.    Geben Sie Folgendes ein:
  * **Name der Organisation**: Wählen Sie die Dynamics 365-Instanz aus, in der der Flow den Datensatz erstellen soll. Dabei muss es sich nicht um die gleiche Instanz handeln, durch die das Ereignis ausgelöst wird.
  * **Entitätsname**: Wählen Sie die Entität aus, die einen Datensatz erstellen soll, wenn das Ereignis ausgelöst wird. In dieser exemplarischen Vorgehensweise wird **Aufgaben** ausgewählt.

13.    Ein Betrefffeld wird angezeigt. Wenn Sie auf das Feld klicken, erscheint ein dynamischer Inhaltsbereich, in dem folgende Felder zur Auswahl stehen:
  * **Nachname**: Wenn Sie dieses Feld auswählen, wird bei der Erstellung des Aufgabendatensatzes der Nachname des Leads in das Betrefffeld der Aufgabe eingefügt.
  * **Thema**: Wenn Sie dieses Feld auswählen, wird bei der Erstellung des Aufgabendatensatzes das Themenfeld für den Lead in das Betrefffeld der Aufgabe eingefügt.
Klicken Sie auf **Thema**, um es dem Feld **Betreff** hinzuzufügen.

  ![Logik-App – Details zum Erstellen eines neuen Datensatzes](./media/connectors-create-api-crmonline/create-record-details.png)

14.    Klicken Sie auf der Symbolleiste des Logik-App-Designers auf **Speichern**.

  ![„Speichern“ auf der Symbolleiste des Logik-App-Designers](./media/connectors-create-api-crmonline/designer-toolbar-save.png)

15.    Klicken Sie zum Starten der Logik-App auf **Ausführen**.

  ![„Speichern“ auf der Symbolleiste des Logik-App-Designers](./media/connectors-create-api-crmonline/designer-toolbar-run.png)

16. Erstellen Sie nun in Dynamics 365 for Sales einen Leaddatensatz, und beobachten Sie Ihren Flow in Aktion.

## <a name="using-advanced-options"></a>Verwenden erweiterter Optionen
Wenn Sie einer Logik-App einen Schritt hinzufügen, können Sie auf **Erweiterte Optionen anzeigen** klicken und einen Filter oder eine Sortierung hinzufügen, um die Filterung der Daten in dem Schritt zu steuern.

So können Sie beispielsweise eine Filterabfrage verwenden, um nur aktive Konten abzurufen, und die Daten nach Kontoname sortieren. Geben Sie hierzu die OData-Filterabfrage **statuscode eq 1** ein, und wählen Sie im dynamischen Inhaltsbereich die Option **Kontoname** aus. Weitere Informationen finden Sie unter [MSDN: $filter](https://msdn.microsoft.com/library/gg309461.aspx#Anchor_1) und [$orderby](https://msdn.microsoft.com/library/gg309461.aspx#Anchor_2).

  ![Erweiterte Optionen für Logik-Apps](./media/connectors-create-api-crmonline/advanced-options.png)

### <a name="best-practices-when-using-advanced-options"></a>Bewährte Methoden bei der Verwendung erweiterter Optionen
Wenn Sie einem Feld einen Wert hinzufügen, muss dieser auf die Art des Felds abgestimmt sein. Dabei spielt es keine Rolle, ob Sie einen Wert eingeben oder ihn aus dem angezeigten dynamischen Inhalt auswählen.

Feldtyp  |Verwendung  |Ort  |Name  |Datentyp  
---------|---------|---------|---------|---------
Textfelder|Textfelder erfordern eine einzelne Textzeile oder dynamischen Inhalt, bei dem es sich um ein Textfeld handelt. Beispiele: „Kategorie“ und „Unterkategorie“|Einstellungen > Anpassungen > System anpassen > Entitäten > Aufgabe > Felder |category |Einzelne Textzeile       
Ganzzahlige Felder | Einige Felder erfordern eine ganze Zahl oder dynamischen Inhalt, bei dem es sich um ein ganzzahliges Feld handelt. Beispiele: „Prozent abgeschlossen“ und „Dauer“ |Einstellungen > Anpassungen > System anpassen > Entitäten > Aufgabe > Felder |percentcomplete |Ganze Zahl         
Datumsfelder | Einige Felder erfordern ein Datum im Format MM/TT/JJJJ oder dynamischen Inhalt, bei dem es sich um ein Datumsfeld handelt. Beispiele: „Erstellt am“, „Startdatum“, „Tatsächlicher Beginn“, „Letzte Zeit der Zurückstellung“, „Tatsächliches Ende“ und „Fälligkeitsdatum“ | Einstellungen > Anpassungen > System anpassen > Entitäten > Aufgabe > Felder |createdon |Datum und Uhrzeit         
Felder, die sowohl eine Datensatz-ID als auch einen Suchtyp erfordern |Einige Felder, die auf einen anderen Entitätsdatensatz verweisen, erfordern sowohl die Datensatz-ID als auch den Suchtyp. |Einstellungen > Anpassungen > System anpassen > Entitäten > Konto > Felder  | accountid   | Primärschlüssel

### <a name="more-examples-of-fields-that-require-both-a-record-id-and-lookup-type"></a>Weitere Beispiele für Felder, die sowohl eine Datensatz-ID als auch einen Suchtyp erfordern
Ergänzend zur obigen Tabelle finden Sie hier weitere Beispiele für Felder, für die keine Werte aus der Liste dynamischer Inhalte verwendet werden können. Für diese Felder muss stattdessen sowohl eine Datensatz-ID als auch ein Suchtyp in die Felder in PowerApps eingegeben werden.  
*  Besitzer und Besitzertyp: Bei dem Feld „Besitzer“ muss es sich um eine gültige Benutzer- oder Teamdatensatz-ID handeln. Der Besitzertyp muss entweder **systemusers** oder **teams** lauten.
* Kunde und Kundentyp: Bei dem Feld „Kunde“ muss es sich um eine gültige Konto- oder Kontaktdatensatz-ID handeln. Der Kundentyp muss entweder **accounts** oder **contacts** lauten.
* Betreff und Betrefftyp: Bei dem Feld „Betreff“ muss es sich um eine gültige Datensatz-ID handeln (beispielsweise eine Konto- oder Kontaktdatensatz-ID). Beim Betrefftyp muss es sich um den Suchtyp für den Datensatz handeln (beispielsweise **accounts** oder **contacts**).

Im folgenden Aufgabenerstellungsbeispiel wird ein der Datensatz-ID entsprechender Kontodatensatz dem Betrefffeld der Aufgabe hinzugefügt.

  ![Flow mit Datensatz-ID und Konto](./media/connectors-create-api-crmonline/recordid-type-account.png)

Außerdem wird die Aufgabe in diesem Beispiel auf der Grundlage der Datensatz-ID des Benutzers einem bestimmten Benutzer zugewiesen.
  ![Flow mit Datensatz-ID und Konto](./media/connectors-create-api-crmonline/recordid-type-user.png)

Informationen zum Ermitteln einer Datensatz-ID finden Sie weiter unten im Abschnitt *Ermitteln der Datensatz-ID*.

## <a name="find-the-record-id"></a>Ermitteln der Datensatz-ID
1. Öffnen Sie einen Datensatz (beispielsweise einen Kontodatensatz).

2. Klicken Sie auf der Aktionssymbolleiste auf **Aufklappen** ![Datensatz aufklappen](./media/connectors-create-api-crmonline/popout-record.png).
Oder: Klicken Sie auf der Aktionssymbolleiste auf **LINK PER E-MAIL SENDEN**, um die vollständige URL in Ihr Standard-E-Mail-Programm zu kopieren.

3. Die Datensatz-ID befindet sich zwischen den Codierungszeichen „%7b“ und „%7d“ der URL.

  ![Flow mit Datensatz-ID und Konto](./media/connectors-create-api-crmonline/recordid.png)

## <a name="troubleshooting"></a>Problembehandlung
Sehen Sie sich zur Problembehandlung im Falle eines nicht erfolgreichen Schritts in einer Logik-App die Statusdetails des Ereignisses an.

1. Klicken Sie im Logic Apps-Bereich auf Ihre Logik-App und anschließend auf **Übersicht**. Der daraufhin angezeigte Zusammenfassungsbereich enthält den Ausführungsstatus der Logik-App. Sollten nicht erfolgreiche Ausführungen vorhanden sein, klicken Sie auf das fehlerhafte Ereignis, für das Sie weitere Informationen anzeigen möchten.

  ![Logik-App – Problembehandlungsschritt 1](./media/connectors-create-api-crmonline/tshoot1.png)

2. Klicken Sie auf den nicht erfolgreichen Schritt, um ihn zu erweitern.

  ![Logik-App – Problembehandlungsschritt 2](./media/connectors-create-api-crmonline/tshoot2.png)

3. Die angezeigten Schrittdetails können zum Behandeln der Fehlerursache herangezogen werden.

    ![Logik-App – Problembehandlungsschritt 2](./media/connectors-create-api-crmonline/tshoot3.png)

Weitere Informationen zum Behandeln von Problemen mit Logik-Apps finden Sie unter [Diagnostizieren von Fehlern bei Logik-Apps](../logic-apps/logic-apps-diagnosing-failures.md).

## <a name="technical-details"></a>Technische Details
## <a name="triggers"></a>Trigger
| Trigger | Beschreibung |
| --- | --- |
| When a record is created (Wenn ein Datensatz erstellt wird) |Löst einen Flow aus, wenn ein Objekt in Dynamics 365 erstellt wird. |
| When a record is updated (Wenn ein Datensatz aktualisiert wird) |Löst einen Flow aus, wenn ein Objekt in Dynamics 365 aktualisiert wird. |
| When a record is deleted (Wenn ein Datensatz gelöscht wird) |Löst einen Flow aus, wenn ein Objekt in Dynamics 365 gelöscht wird. |

## <a name="actions"></a>Actions
| Aktion | Beschreibung |
| --- | --- |
| List records (Datensätze auflisten) |Dieser Vorgang ruft die Datensätze für eine Entität ab. |
| Create a new record (Neuen Datensatz erstellen) |Dieser Vorgang erstellt einen neuen Datensatz einer Entität. |
| Get record (Datensatz abrufen) |Dieser Vorgang ruft den angegebenen Datensatz für eine Entität ab. |
| Delete a record (Datensatz löschen) |Dieser Vorgang löscht einen Datensatz aus einer Entitätssammlung. |
| Aktualisieren eines Eintrags |Dieser Vorgang aktualisiert einen vorhandenen Datensatz für eine Entität. |

### <a name="trigger-and-action-details"></a>Trigger- und Aktionsdetails
Dieser Abschnitt enthält spezifische Details zu den einzelnen Triggern und Aktionen. Hierzu zählen unter anderem erforderliche oder optionale Eingabeeigenschaften sowie entsprechende Ausgaben im Zusammenhang mit dem Connector.

#### <a name="when-a-record-is-created"></a>When a record is created (Wenn ein Datensatz erstellt wird)
Löst einen Flow aus, wenn ein Objekt in Dynamics 365 erstellt wird.

| Eigenschaftenname | Anzeigename | Beschreibung |
| --- | --- | --- |
| dataset* |Name der Organisation |Name der Dynamics 365-Organisation (beispielsweise Contoso) |
| table* |Entitätsname |Name der Entität |
| $filter |Filterabfrage |Eine ODATA-Filterabfrage zum Einschränken der Anzahl zurückgegebener Einträge |
| $orderby |Sortieren nach |Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
ItemsList

| Eigenschaftenname | Datentyp |
| --- | --- |
| value |array |

#### <a name="when-a-record-is-updated"></a>When a record is updated (Wenn ein Datensatz aktualisiert wird)
Löst einen Flow aus, wenn ein Objekt in Dynamics 365 aktualisiert wird.

| Eigenschaftenname | Anzeigename | Beschreibung |
| --- | --- | --- |
| dataset* |Name der Organisation |Name der Dynamics 365-Organisation (beispielsweise Contoso) |
| table* |Entitätsname |Name der Entität |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
ItemsList

| Eigenschaftenname | Datentyp |
| --- | --- |
| value |array |

#### <a name="when-a-record-is-deleted"></a>When a record is deleted (Wenn ein Datensatz gelöscht wird)
Löst einen Flow aus, wenn ein Objekt in Dynamics 365 gelöscht wird.

| Eigenschaftenname | Anzeigename | Beschreibung |
| --- | --- | --- |
| dataset* |Name der Organisation |Name der Dynamics 365-Organisation (beispielsweise Contoso) |
| table* |Entitätsname |Name der Entität |


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
| dataset* |Name der Organisation |Name der Dynamics 365-Organisation (beispielsweise Contoso) |
| table* |Entitätsname |Name der Entität |
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
| dataset* |Name der Organisation |Name der Dynamics 365-Organisation (beispielsweise Contoso) |
| table* |Entitätsname |Name der Entität |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Keine

#### <a name="get-record"></a>Get record (Datensatz abrufen)
Dieser Vorgang ruft den angegebenen Datensatz für eine Entität ab.

| Eigenschaftenname | Anzeigename | Beschreibung |
| --- | --- | --- |
| dataset* |Name der Organisation |Name der Dynamics 365-Organisation (beispielsweise Contoso) |
| table* |Entitätsname |Name der Entität |
| id* |Item identifier (Elementbezeichner) |Bezeichner für den Datensatz |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Keine

#### <a name="delete-a-record"></a>Delete a record (Datensatz löschen)
Dieser Vorgang löscht einen Datensatz aus einer Entitätssammlung.

| Eigenschaftenname | Anzeigename | Beschreibung |
| --- | --- | --- |
| dataset* |Name der Organisation |Name der Dynamics 365-Organisation (beispielsweise Contoso) |
| table* |Entitätsname |Name der Entität |
| id* |Item identifier (Elementbezeichner) |Bezeichner für den Datensatz |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

#### <a name="update-a-record"></a>Update a record (Datensatz aktualisieren)
Dieser Vorgang aktualisiert einen vorhandenen Datensatz für eine Entität.

| Eigenschaftenname | Anzeigename | Beschreibung |
| --- | --- | --- |
| dataset* |Name der Organisation |Name der Dynamics 365-Organisation (beispielsweise Contoso) |
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
Informieren Sie sich in unserer [API-Liste](apis-list.md)über die anderen verfügbaren Connectors für Logik-Apps.

