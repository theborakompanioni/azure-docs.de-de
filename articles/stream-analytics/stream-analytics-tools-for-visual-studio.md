---
title: "Verwenden des Azure Stream Analytics-Tools für Visual Studio | Microsoft-Dokumentation"
description: "Tutorial mit ersten Schritten für die Azure Stream Analytics-Tools für Visual Studio"
keywords: Visual Studio
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: a473ea0a-3eaa-4e5b-aaa1-fec7e9069f20
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: sujie
translationtype: Human Translation
ms.sourcegitcommit: 503f5151047870aaf87e9bb7ebf2c7e4afa27b83
ms.openlocfilehash: d0125dda4df69279e49a9fad4dc28dcbf6368322
ms.lasthandoff: 03/28/2017


---
# <a name="use-azure-stream-analytics-tool-for-visual-studio"></a>Verwenden des Azure Stream Analytics-Tools für Visual Studio
Azure Stream Analytics-Tools für Visual Studio sind jetzt allgemein verfügbar. Diese Tools bieten umfangreichere Funktionen für Stream Analytics-Benutzer beim Beheben von Problemen sowie beim Verfassen komplexer Abfragen und sogar beim lokalen Schreiben von Abfragen. Sie können einen Stream Analytics-Auftrag zudem in ein Visual Studio-Projekt exportieren.

## <a name="introduction"></a>Einführung
In diesem Tutorial erfahren Sie, wie Sie mit Azure Stream Analytics-Tools für Visual Studio Ihre Azure Stream Analytics-Aufträge erstellen, verfassen, lokal testen, verwalten und debuggen. 

Nach Abschluss dieses Tutorials, werden Sie in der Lage sein, folgende Aufgaben auszuführen:
* Machen Sie sich mit den Azure Stream Analytics-Tools für Visual Studio vertraut.
* Konfigurieren und Bereitstellen eines Stream Analytics-Auftrags
* Lokales Testen Ihres Auftrags mit lokalen Beispieldaten
* Verwenden der Überwachung zur Behebung von Problemen
* Exportieren vorhandener Aufträge in Projekte

## <a name="prerequisites"></a>Voraussetzungen
Zum Durchführen dieses Tutorials benötigen Sie Folgendes:
* Führen Sie die Schritte vor **Erstellen eines Stream Analytics-Auftrags** im Tutorial [Erstellen einer IoT-Lösung mithilfe von Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-build-an-iot-solution-using-stream-analytics) aus. 
* Visual Studio 2015, Visual Studio 2013 Update 4 oder Visual Studio 2012. Die Editionen Enterprise (Ultimate/Premium), Professional und Community werden unterstützt; Express Edition wird nicht unterstützt. Visual Studio 2017 wird derzeit nicht unterstützt. 
* Microsoft Azure SDK für .NET, Version 2.7.1 oder höher.  Führen Sie die Installation mit dem [Webplattform-Installer](http://www.microsoft.com/web/downloads/platform.aspx)durch.
* Installation von [Azure Stream Analytics Tools for Visual Studio](http://aka.ms/asatoolsvs) (Azure Stream Analytics-Tools für Visual Studio).

## <a name="create-a-stream-analytics-project"></a>Erstellen eines Stream Analytics-Projekts
Klicken Sie in Visual Studio auf das Menü **Datei**, und wählen Sie **Neues Projekt**. Wählen Sie in der Vorlagenliste links **Stream Analytics** aus, und klicken Sie dann auf **Azure Stream Analytics Application** (Azure Stream Analytics-Anwendung).
Geben Sie den Projektnamen, Speicherort und Lösungsnamen wie bei anderen Projekten unten ein.

![Erstellen eines Azure Stream Analytics-Projekts](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-create-project-01.png)

Im **Projektmappen-Explorer** wird ein Projekt namens **Maut** erstellt.

![Erstellen eines Azure Stream Analytics-Projekts](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-create-project-02.png)

## <a name="choose-the-correct-subscription"></a>Wahl des richtigen Abonnements
1. Öffnen Sie in Visual Studio im Menü **Ansicht** den **Server-Explorer**.
2. Melden Sie sich bei Ihrem Azure-Konto an. 

## <a name="define-input-sources"></a>Definieren von Eingabequellen
1.    Erweitern Sie im **Projektmappen-Explorer** den Knoten **Eingabe**, und benennen Sie **Input.json** in **EntryStream.json** um. Doppelklicken Sie auf **EntryStream.json**.
2.    Ihr **EINGABEALIAS** sollte jetzt **EntryStream** lauten. Beachten Sie, dass der Eingabealias im Abfrageskript verwendet wird. 
3.    Der Quelltyp ist **Datenstrom**.
4.    Die Quelle lautet **Event Hub**.
5.    Der Service Bus-Namespace sollte der erste **tollData**-Eintrag in der Dropdownliste sein.
6.    Der Event Hub-Name sollte als **Eintrag** festgelegt werden.
7.    Der Event Hub-Richtlinienname lautet **RootManageSharedAccessKey** (Standardwert).
8.    Wählen Sie **JSON** als **Ereignisserialisierungsformat** und **UTF8** als **Codierung** aus.
   
   Ihre Einstellungen sehen nun folgendermaßen aus:
   
   ![Definieren von Eingabequellen](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-01.png)
   
9.    Klicken Sie unten auf der Seite auf **Speichern**, um den Assistenten zu beenden. Sie können jetzt eine andere Eingabequelle zum Erstellen des Ausgangsdatenstroms hinzufügen. Klicken Sie mit der rechten Maustaste auf den Eingangsknoten, und klicken Sie auf **Neues Element**.
   
   ![Definieren von Eingabequellen](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-02.png)
   
10.    Wählen Sie im eingeblendeten Fenster **Azure Stream Analytics Input** (Azure Stream Analytics-Eingabe), und ändern Sie den Namen in **ExitStream.json**. Klicken Sie auf **Hinzufügen**.
   
   ![Definieren von Eingabequellen](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-03.png)
   
11.    Doppelklicken Sie im Projekt auf **ExitStream.json**, und führen Sie dieselben Schritte wie im Eingabedatenstrom aus. Geben Sie die Werte für den Event Hub-Namen wie im folgenden Screenshot ein.
   
   ![Definieren von Eingabequellen](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-04.png)
   
   Sie haben jetzt zwei Eingabedatenströme definiert:
   
   ![Definieren von Eingabequellen](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-05.png)
   
   Als Nächstes fügen Sie die Referenzdateneingabe für die Blobdatei hinzu, in der die Fahrzeugregistrierungsdaten enthalten sind.
   
12.    Klicken Sie mit der rechten Maustaste im Projekt auf den Knoten **Eingaben**, und gehen Sie anschließend genauso wie bei der Datenstromeingabe vor. Wählen Sie jedoch **REFERENZDATEN** anstelle von „Datenstrom“ aus. Wählen Sie als Eingabealias den Eintrag **Registrierung** aus.
   
   ![Definieren von Eingabequellen](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-06.png)
   
13.    Wählen Sie das Storage-Konto aus, das **tolldata** enthält. Der Name des Containers sollte **tolldata** sein, und das **Pfadmuster** sollte **registration.json** lauten. Für diesen Dateinamen wird die Groß-/Kleinschreibung berücksichtigt, und er sollte nur Kleinbuchstaben enthalten.
14.    Klicken Sie auf **Speichern**, um den Assistenten zu beenden.

Jetzt sind alle Eingaben definiert.

## <a name="define-output"></a>Definieren der Ausgabe
1.    Erweitern Sie im **Projektmappen-Explorer** den Knoten **Eingaben**, und doppelklicken Sie auf **Output.json**.
2.    Legen Sie den Ausgabealias auf **Ausgabe** und anschließend den Eintrag für „Senke“ auf „SQL-Datenbank“ fest.
2.    Geben Sie den Datenbanknamen ein: **TollDataDB**.
3.    Geben Sie im Feld **BENUTZERNAME** den Text **tolladmin**, **123toll!** im Feld **KENNWORT** den Text und im Feld **TABELLE** **TollDataRefJoinein**.
4.    Klicken Sie auf **Speichern**.

![Definieren der Ausgabe](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-output-01.png)
 
## <a name="azure-stream-analytics-query"></a>Azure Stream Analytics-Abfragen
In diesem Tutorial versuchen wir, einige geschäftliche Fragen zu beantworten, die im Zusammenhang mit Mautdaten stehen. Außerdem werden Stream Analytics-Abfragen erstellt, die in Azure Stream Analytics verwendet werden können, um eine relevante Antwort zu liefern.
Bevor wir mit dem ersten Stream Analytics-Auftrag beginnen, sehen wir uns einige einfache Szenarien und die Abfragesyntax an.

### <a name="introduction-to-azure-stream-analytics-query-language"></a>Einführung in die Azure Stream Analytics-Abfragesprache
Angenommen, Sie müssen die Fahrzeuge zählen, die ein Mauthäuschen passieren. Da es sich um einen kontinuierlichen Datenstrom von Ereignissen handelt, müssen Sie einen bestimmten Zeitraum definieren. Wir formulieren die Frage also neu: „Wie viele Fahrzeuge passieren ein Mauthäuschen jeweils innerhalb von drei Minuten?“ Dies wird für gewöhnlich als „Rollierende Anzahl“ bezeichnet.

Sehen wir uns die Beantwortung dieser Frage durch die Azure Stream Analytics-Abfrage an:

        SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*) AS Count 
        FROM EntryStream TIMESTAMP BY EntryTime 
        GROUP BY TUMBLINGWINDOW(minute, 3), TollId 

Es ist erkennbar, dass in Azure Stream Analytics eine Abfragesprache verwendet wird, die wie SQL aufgebaut ist und über einige Erweiterungen verfügt, damit die zeitbezogenen Aspekte der Abfrage angegeben werden können.

Weitere Informationen finden Sie in den Artikeln zu [Zeitmanagement](https://msdn.microsoft.com/library/azure/mt582045.aspx)- und [Windowing](https://msdn.microsoft.com/library/azure/dn835019.aspx)-Konstrukten, die in der Abfrage von MSDN verwendet werden.

Da Sie nun Ihre erste Azure Stream Analytics-Abfrage geschrieben haben, können Sie sie mithilfe der Beispieldatendateien testen, die in Ihrem TollApp-Ordner unter dem folgendem Pfad vorhanden sind:

<seg>
  **..\TollApp\TollApp\Data**</seg>

Dieser Ordner enthält die folgenden Dateien: • Entry.json • Exit.json • Registration.json

## <a name="question-number-of-vehicles-entering-a-toll-booth"></a>Frage: Wie viele Fahrzeuge passieren ein Mauthäuschen?
Doppelklicken Sie im Projekt auf „Script.asaql“, um das Skript im Editor zu öffnen, und fügen Sie das Skript im vorherigen Abschnitt in den Editor ein. Der Abfrage-Editor unterstützt Intellisense, farbige Syntaxmarkierung und Fehlermarker.

![Abfrage bearbeiten](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-query-01.png)
 
### <a name="testing-azure-stream-analytics-queries-locally"></a>Lokales Testen von Azure Stream Analytics-Abfragen

1. Sie können zunächst die Abfrage kompilieren, um festzustellen, ob ein Syntaxfehler aufgetreten ist. [TBD]
2. Um diese Abfrage anhand von Beispieldaten zu überprüfen, können Sie lokale Beispieldaten verwenden, indem Sie mit der rechten Maustaste auf die Eingabe klicken und im Kontextmenü **Lokale Eingabe hinzufügen** auswählen.
   
   ![Lokale Eingabe hinzufügen](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-01.png)
   
   Wählen Sie im Popupfenster die Beispieldaten vom lokalen Pfad aus. Klicken Sie auf **Speichern**.
   
   ![Lokale Eingabe hinzufügen](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-02.png)
   
   Eine Datei namens **local_EntryStream.json** wird automatisch zu Ihrem Ordner „Eingaben“ hinzugefügt.
   
   ![Lokale Eingabe hinzufügen](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-03.png)
   
3. Klicken Sie im Abfrage-Editor auf „Lokal ausführen“. Oder drücken Sie F5.
   
   ![Lokaler Testlauf](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-local-run-01.png)
   
   Sie können den Ausgabepfad der Konsolenausgabe suchen und eine beliebige Taste drücken, um den Ergebnisordner zu öffnen.
   
   ![Lokaler Testlauf](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-local-run-02.png)
   
4. Überprüfen Sie die Ergebnisse im lokalen Ordner.
   
   ![Lokaler Testlauf](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-local-run-03.png)
   
   
### <a name="sample-input"></a>Beispieleingabe
Sie können auch Beispieleingabedaten aus Eingabequellen in einer lokalen Datei verwenden. Klicken Sie mit der rechten Maustaste auf die Eingabekonfigurationsdatei, und wählen Sie **Beispieldaten** aus. 

![Beispieldaten](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-01.png)

Bitte beachten Sie, dass Sie derzeit nur Event Hub oder IoT Hub testen können. Andere Eingabequellen werden nicht unterstützt.  Füllen Sie im Popupdialogfeld den lokalen Pfad zum Speichern der Beispieldaten aus. Klicken Sie auf **Beispiel**.

![Beispieldaten](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-02.png)
 
Sie können den Fortschritt im Ausgabefenster anzeigen. 

![Beispieldaten](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-03.png)
 
### <a name="submit-azure-stream-analytics-query-to-azure"></a>Übermitteln einer Azure Stream Analytics-Abfrage an Azure
Klicken Sie im**Abfrage-Editor** auf **Submit To Azure in script editor** (In Skript-Editor an Azure übermitteln).

![Auftrag übermitteln](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-submit-job-01.png)
 
Wählen Sie „Create a New Azure Stream Analytics Job“ (Einen neuen Azure Stream Analytics-Auftrag erstellen). Geben Sie den Auftragsnamen wie unten beschrieben ein. Wählen Sie das richtige Abonnement. Klicken Sie auf Submit.

![Auftrag übermitteln](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-submit-job-02.png)

 
### <a name="start-job"></a>Auftrag starten
Ihr Auftrag wurde gerade erstellt. Die Auftragsansicht wird automatisch geöffnet. Klicken Sie auf die **GRÜNE** Schaltfläche, um den Auftrag zu starten.

![Auftrag starten](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-01.png)
 
Wählen Sie die Standardeinstellung, und klicken Sie auf **Start**.
 
![Auftrag starten](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-02.png)

Sie sehen, dass der Status des Auftrags in **Ausführen** geändert wurde, und Eingabe-/Ausgabeereignisse vorhanden sind.

![Auftrag starten](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-03.png)

## <a name="check-results-in-visual-studio"></a>Überprüfen Sie die Ergebnisse in Visual Studio
1. Öffnen Sie Visual Studio Server-Explorer, und klicken Sie mit der rechten Maustaste auf die Tabelle **TollDataRefJoin** .
2. Klicken Sie auf **Tabellendaten anzeigen** , um die Ausgabe des Auftrags anzuzeigen.
   
   ![Auswahl von „Tabellendaten anzeigen“ im Server-Explorer](media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-check-results.jpg)
   

### <a name="view-job-metrics"></a>Anzeigen von Auftragsmetriken
Einige grundlegende Auftragsstatistiken finden Sie in **Auftragsmetriken**. 

![Auftragsmetriken](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-job-metrics-01.png)

 
## <a name="list-job-in-server-explorer"></a>Auflisten des Auftrags im Server-Explorer
Klicken Sie im **Server-Explorer** auf **Stream Analytics-Aufträge**, und klicken Sie auf **Aktualisieren**. Der Auftrag sollte jetzt unter **Stream Analytics-Aufträge** angezeigt werden.

![Auflisten von Aufträgen](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-list-jobs-01.png)


## <a name="open-job-view"></a>Öffnen der Auftragsansicht
Erweitern Sie den Auftragsknoten, und doppelklicken Sie auf den Knoten **Auftragsansicht**, um die Auftragsansicht zu öffnen.

![Auftragsansicht](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-job-view-01.png)


## <a name="export-an-existing-job-to-a-project"></a>Exportieren eines vorhandenen Auftrags in ein Projekt
Es gibt zwei Möglichkeiten zum Exportieren eines vorhandenen Auftrags in ein Projekt.
1. Klicken Sie in **Server-Explorer** unter **Stream Analytics-Aufträge** mit der rechten Maustaste auf den Auftragsknoten. Klicken Sie im Kontextmenü auf **Export to New Stream Analytics Project** (In neues Stream Analytics-Projekt exportieren).
   
   ![Auftrag exportieren](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-01.png)
   
   Sie sehen das erstellte Projekt im **Projektmappen-Explorer**.
   
   ![Auftrag exportieren](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-02.png)
   
2. Klicken Sie in der Auftragsansicht auf **Projekt generieren**.
   
   ![Auftrag exportieren](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-03.png)
   
## <a name="known-issues-and-limitations"></a>Bekannte Probleme und Einschränkungen
 
1. Lokale Tests funktionieren nicht, wenn Ihre Abfrage geografische Funktionen enthält. 
2. Keine Editor-Unterstützung beim Hinzufügen oder Ändern von JavaScript-UDF.
3. Lokale Tests unterstützen das Speichern der Ausgabe im JSON-Format nicht. 
4. Keine Unterstützung für Power BI-Ausgabe und ADLS-Ausgabe.



## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)



