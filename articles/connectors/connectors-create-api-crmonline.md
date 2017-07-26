---
title: "Verbinden mit Dynamics 365 (online) über Azure Logic Apps | Microsoft-Dokumentation"
description: "Erstellen Sie Logik-App-Workflows, die Dynamics 365 (online)-Entitäten über die vom Dynamics 365-Connector bereitgestellten API verwalten."
services: logic-apps
cloud: Azure Stack
author: Mattp123
manager: anneta
documentationcenter: 
tags: connectors
ms.assetid: 0dc2abef-7d2c-4a2d-87ca-fad21367d135
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: matp; LADocs
ms.translationtype: Human Translation
ms.sourcegitcommit: c785ad8dbfa427d69501f5f142ef40a2d3530f9e
ms.openlocfilehash: d35647921ff540167a3a591fb489d3bab031a5c1
ms.contentlocale: de-de
ms.lasthandoff: 05/26/2017

---

# <a name="connect-to-dynamics-365-from-logic-app-workflows"></a>Verbinden mit Dynamics 365 über Logik-App-Workflows

Mit Logik-Apps können Sie eine Verbindung zu Dynamics 365 (online) herstellen und praktische Geschäftsworkflows erstellen, die Datensätze erstellen, Elemente aktualisieren oder eine Liste mit Datensätzen zurückgeben. Der Dynamics 365-Connector bietet folgende Möglichkeiten:

* Erstellen eines Geschäftsflows auf der Grundlage der Daten, die Sie von Dynamics 365 (online) erhalten.
* Verwenden von Aktionen, die eine Antwort generieren, und anschließendes Verfügbarmachen der Ausgabe für andere Aktionen. Wenn also beispielsweise ein Element in Dynamics 365 (online) aktualisiert wird, können Sie über Office 365 eine E-Mail senden.

In diesem Thema erfahren Sie, wie Sie eine Logik-App erstellen, die eine Aufgabe in Dynamics 365 erstellt, sobald in Dynamics 365 ein neuer Lead erstellt wird.

## <a name="prerequisites"></a>Voraussetzungen
* Ein Azure-Konto.
* Eine Dynamics 365 (online)-Konto.

## <a name="create-a-task-when-a-new-lead-is-created-in-dynamics-365"></a>Erstellen einer Aufgabe bei der Erstellung eines neuen Leads in Dynamics 365

1.    [Melden Sie sich bei Azure an.](https://portal.azure.com)

2.    Geben Sie im Azure-Suchfeld `Logic apps` ein und drücken Sie die EINGABETASTE.

      ![Suchen nach Logik-Apps](./media/connectors-create-api-crmonline/find-logic-apps.png)

3.    Klicken Sie unter **Logik-Apps** auf **Hinzufügen**.

      ![Hinzufügen einer Logik-App](./media/connectors-create-api-crmonline/add-logic-app.png)

4.    Füllen Sie zum Erstellen der Logik-App die Felder **Name**, **Abonnement**, **Ressourcengruppe** und **Standort** aus, und klicken Sie anschließend auf **Erstellen**.

5.    Wählen Sie die neue Logik-App aus. Klicken Sie auf **Aktualisieren**, wenn Sie die Benachrichtigung **Bereitstellung erfolgreich** erhalten.

6.    Klicken Sie unter **Entwicklungstools** auf **Logik-App-Designer**. Klicken Sie in der Vorlagenliste auf **Leere Logik-App**.

7.    Geben Sie in das Suchfeld `Dynamics 365` ein. Wählen Sie aus der Dynamics 365-Triggerliste **Dynamics 365 – Wenn ein Datensatz erstellt wird** aus.

8.    Melden Sie sich bei Dynamics 365 an, falls Sie dazu aufgefordert werden.

9.    Geben Sie in den Triggerdetails folgende Informationen ein:

  * **Name der Organisation**: Wählen Sie die Dynamics 365-Instanz aus, die die Logik-App überwachen soll.

  * **Entitätsname**: Wählen Sie die Entität aus, die überwacht werden soll. Dieses Ereignis dient als Trigger, um die Logik-App zu starten. 
  In dieser exemplarischen Vorgehensweise wird **Leads** ausgewählt.

  * **Wie oft möchten Sie auf Elemente prüfen?**: Mit diesen Werten legen Sie fest, wie oft die Logik-App nach Trigger-bezogenen Aktualisierungen suchen soll. Standardmäßig findet alle drei Minuten eine Prüfung auf Aktualisierungen statt.

    * **Häufigkeit**: Wählen Sie Sekunden, Minuten, Stunden oder Tage aus.

    * **Intervall**: Geben Sie die Anzahl von Sekunden, Minuten, Stunden oder Tagen ein, die bis zur nächsten Überprüfung verstreichen sollen.

      ![Triggerdetails der Logik-App](./media/connectors-create-api-crmonline/trigger-details.png)

10. Klicken Sie auf **Neuer Schritt** und anschließend auf **Aktion hinzufügen**.

11. Geben Sie in das Suchfeld `Dynamics 365` ein. Wählen Sie aus der Aktionsliste **Dynamics 365 – Neuen Datensatz erstellen** aus.

12. Geben Sie Folgendes ein:

    * **Name der Organisation**: Wählen Sie die Dynamics 365-Instanz aus, in der der Workflow den Datensatz erstellen soll. 
    Dabei muss es sich nicht um dieselbe Instanz handeln, durch die das Ereignis ausgelöst wird.

    * **Entitätsname**: Wählen Sie die Entität aus, die einen Datensatz erstellen soll, wenn das Ereignis ausgelöst wird. 
    In dieser exemplarischen Vorgehensweise wird **Aufgaben** ausgewählt.

13. Klicken Sie auf das angezeigte Feld **Betreff**. In der dynamischen Inhaltsliste, die angezeigt wird, können Sie eines der folgenden Felder auswählen:

    * **Nachname**: Wenn Sie dieses Feld auswählen, wird bei der Erstellung des Aufgabendatensatzes der Nachname des Leads in das Betrefffeld der Aufgabe eingefügt.
    * **Thema**: Wenn Sie dieses Feld auswählen, wird bei der Erstellung des Aufgabendatensatzes das Feld „Thema“ des Leads in das Betrefffeld der Aufgabe eingefügt. 
    Klicken Sie auf **Thema**, um es dem Feld **Betreff** hinzuzufügen.

      ![Logik-App – Details zum Erstellen eines neuen Datensatzes](./media/connectors-create-api-crmonline/create-record-details.png)

14. Klicken Sie auf der Symbolleiste des Logik-App-Designers auf **Speichern**.

    ![„Speichern“ auf der Symbolleiste des Logik-App-Designers](./media/connectors-create-api-crmonline/designer-toolbar-save.png)

15. Klicken Sie zum Starten der Logik-App auf **Ausführen**.

    ![„Speichern“ auf der Symbolleiste des Logik-App-Designers](./media/connectors-create-api-crmonline/designer-toolbar-run.png)

16. Erstellen Sie nun in Dynamics 365 for Sales einen Leaddatensatz, und beobachten Sie Ihren Flow in Aktion.

## <a name="set-advanced-options-for-a-logic-app-step"></a>Festlegen von erweiterten Optionen für einen Schritt der Logik-App

Um festzulegen, wie Daten in einem Schritt der Logik-App gefiltert werden, klicken Sie bei diesem Schritt auf **Erweiterte Optionen anzeigen** und fügen Sie dann einen Filter hinzu oder sortieren Sie die Daten nach Abfrage.

So können Sie beispielsweise eine Filterabfrage verwenden, um nur aktive Konten abzurufen, und die Daten nach Kontoname sortieren. Geben Sie zur Durchführung dieser Aufgabe die OData-Filterabfrage `statuscode eq 1` ein und wählen Sie in der dynamischen Inhaltsliste die Option **Kontoname** aus. Weitere Informationen finden Sie unter [MSDN: $filter](https://msdn.microsoft.com/library/gg309461.aspx#Anchor_1) und [$orderby](https://msdn.microsoft.com/library/gg309461.aspx#Anchor_2).

![Erweiterte Optionen für Logik-Apps](./media/connectors-create-api-crmonline/advanced-options.png)

### <a name="best-practices-when-using-advanced-options"></a>Bewährte Methoden bei der Verwendung erweiterter Optionen

Wenn Sie einem Feld einen Wert hinzufügen, muss dieser dem Feldtyp entsprechen. Dabei spielt es keine Rolle, ob Sie einen Wert eingeben oder ihn aus der dynamischen Inhaltsliste auswählen.

Feldtyp  |Verwendung  |Ort  |Name  |Datentyp  
---------|---------|---------|---------|---------
Textfelder|Textfelder erfordern eine einzelne Textzeile oder dynamischen Inhalt, bei dem es sich um ein Textfeld handelt. Beispiele: „Kategorie“ und „Unterkategorie“|Einstellungen > Anpassungen > System anpassen > Entitäten > Aufgabe > Felder |category |Einzelne Textzeile        
Ganzzahlige Felder | Einige Felder erfordern eine ganze Zahl oder dynamischen Inhalt, bei dem es sich um ein ganzzahliges Feld handelt. Beispiele: „Prozent abgeschlossen“ und „Dauer“ |Einstellungen > Anpassungen > System anpassen > Entitäten > Aufgabe > Felder |percentcomplete |Ganze Zahl         
Datumsfelder | Einige Felder erfordern ein Datum im Format MM/TT/JJJJ oder dynamischen Inhalt, bei dem es sich um ein Datumsfeld handelt. Beispiele: „Erstellt am“, „Startdatum“, „Tatsächlicher Beginn“, „Letzte Zeit der Zurückstellung“, „Tatsächliches Ende“ und „Fälligkeitsdatum“ | Einstellungen > Anpassungen > System anpassen > Entitäten > Aufgabe > Felder |createdon |Datum und Uhrzeit
Felder, die sowohl eine Datensatz-ID als auch einen Suchtyp erfordern |Einige Felder, die auf einen anderen Entitätsdatensatz verweisen, erfordern sowohl die Datensatz-ID als auch den Suchtyp. |Einstellungen > Anpassungen > System anpassen > Entitäten > Konto > Felder  | accountid  | Primärschlüssel

### <a name="more-examples-of-fields-that-require-both-a-record-id-and-lookup-type"></a>Weitere Beispiele für Felder, die sowohl eine Datensatz-ID als auch einen Suchtyp erfordern
Ergänzend zur obigen Tabelle finden Sie hier weitere Beispiele für Felder, für die keine Werte aus der Liste dynamischer Inhalte verwendet werden können. Für diese Felder muss stattdessen sowohl eine Datensatz-ID als auch ein Suchtyp in die Felder in PowerApps eingegeben werden.  
* Besitzer und Besitzertyp: Bei dem Feld „Besitzer“ muss es sich um eine gültige Benutzer- oder Teamdatensatz-ID handeln. Der Besitzertyp muss entweder **systemusers** oder **teams** lauten.
* Kunde und Kundentyp: Bei dem Feld „Kunde“ muss es sich um eine gültige Konto- oder Kontaktdatensatz-ID handeln. Der Kundentyp muss entweder **accounts** oder **contacts** lauten.
* Betreff und Betrefftyp: Bei dem Feld „Betreff“ muss es sich um eine gültige Datensatz-ID handeln (beispielsweise eine Konto- oder Kontaktdatensatz-ID). Beim Betrefftyp muss es sich um den Suchtyp für den Datensatz handeln (beispielsweise **accounts** oder **contacts**).

Im folgenden Aufgabenerstellungsbeispiel wird ein der Datensatz-ID entsprechender Kontodatensatz dem Betrefffeld der Aufgabe hinzugefügt.

![Flow mit Datensatz-ID und Konto](./media/connectors-create-api-crmonline/recordid-type-account.png)

Außerdem wird die Aufgabe in diesem Beispiel auf der Grundlage der Datensatz-ID des Benutzers einem bestimmten Benutzer zugewiesen.

![Flow mit Datensatz-ID und Konto](./media/connectors-create-api-crmonline/recordid-type-user.png)

Informationen zum Ermitteln einer Datensatz-ID finden Sie im folgenden Abschnitt: *Ermitteln der Datensatz-ID*.

## <a name="find-the-record-id"></a>Ermitteln der Datensatz-ID

1. Öffnen Sie einen Datensatz (beispielsweise einen Kontodatensatz).

2. Klicken Sie auf der Aktionssymbolleiste auf **Aufklappen** ![Datensatz aufklappen](./media/connectors-create-api-crmonline/popout-record.png).
Klicken Sie alternativ auf der Aktionssymbolleiste auf **LINK PER E-MAIL SENDEN**, um die vollständige URL in Ihr Standard-E-Mail-Programm zu kopieren.

   Die Datensatz-ID befindet sich zwischen den Codierungszeichen „%7b“ und „%7d“ der URL.

   ![Flow mit Datensatz-ID und Konto](./media/connectors-create-api-crmonline/recordid.png)

## <a name="troubleshooting"></a>Problembehandlung
Sehen Sie sich zur Problembehandlung im Falle eines nicht erfolgreichen Schritts in einer Logik-App die Statusdetails des Ereignisses an.

1. Klicken Sie im Bereich **Logik-Apps** auf Ihre Logik-App und anschließend auf **Übersicht**. 

   Der daraufhin angezeigte Zusammenfassungsbereich enthält den Ausführungsstatus der Logik-App. 

   ![Ausführungsstatus der Logik-App](./media/connectors-create-api-crmonline/tshoot1.png)

2. Um weitere Informationen zu fehlgeschlagenen Ausführungen anzuzeigen, klicken Sie auf das fehlerhafte Ereignis. Klicken Sie auf diesen Schritt, um einen fehlerhaften Schritt zu erweitern.

   ![Erweitern eines fehlerhaften Schritts](./media/connectors-create-api-crmonline/tshoot2.png)

   Die angezeigten Schrittdetails können zur Behebung der Fehlerursache herangezogen werden.

   ![Details zu fehlerhaften Schritten](./media/connectors-create-api-crmonline/tshoot3.png)

Weitere Informationen zum Behandeln von Problemen mit Logik-Apps finden Sie unter [Diagnostizieren von Fehlern bei Logik-Apps](../logic-apps/logic-apps-diagnosing-failures.md).

## <a name="connector-specific-details"></a>Connectorspezifische Details

Zeigen Sie die in Swagger definierten Trigger und Aktionen sowie mögliche Beschränkungen in den [Connectordetails](/connectors/crm/) an. 

## <a name="next-steps"></a>Nächste Schritte
Informieren Sie sich in unserer [API-Liste](apis-list.md)über die anderen verfügbaren Connectors für Logik-Apps.

