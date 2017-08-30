---
title: "Verwenden des Azure Stream Analytics-Tools für Visual Studio | Microsoft-Dokumentation"
description: "Tutorial mit ersten Schritten für die Azure Stream Analytics-Tools für Visual Studio"
keywords: Visual Studio
documentationcenter: 
services: stream-analytics
author: samacha
manager: 
editor: 
ms.assetid: a473ea0a-3eaa-4e5b-aaa1-fec7e9069f20
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 
ms.author: samacha
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: 618c1055795a75e0ed71dacddba3e076f81f4946
ms.contentlocale: de-de
ms.lasthandoff: 07/04/2017

---
# <a name="use-azure-stream-analytics-tools-for-visual-studio"></a>Verwenden der Azure Stream Analytics-Tools für Visual Studio
## <a name="introduction"></a>Einführung
In diesem Tutorial erfahren Sie, wie Sie mit Azure Stream Analytics-Tools für Visual Studio Ihre Stream Analytics-Aufträge erstellen, verfassen, lokal testen, verwalten und debuggen. 

Nach Abschluss dieses Tutorials, werden Sie in der Lage sein, folgende Aufgaben auszuführen:
* Machen Sie sich mit den Stream Analytics-Tools für Visual Studio vertraut.
* Konfigurieren und Bereitstellen eines Stream Analytics-Auftrags
* Lokales Testen Ihres Auftrags mit lokalen Beispieldaten
* Verwenden der Überwachung zum Beheben von Problemen
* Exportieren vorhandener Aufträge in Projekte

## <a name="prerequisites"></a>Voraussetzungen
Zum Durchführen dieses Tutorials benötigen Sie Folgendes:
* Führen Sie die Schritte vor „Erstellen eines Stream Analytics-Auftrags“ im Tutorial [Erstellen einer IoT-Lösung mithilfe von Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-build-an-iot-solution-using-stream-analytics) aus. 
* Verwenden Sie Visual Studio 2015, Visual Studio 2013 Update 4 oder Visual Studio 2012. Die Editionen Enterprise (Ultimate/Premium), Professional und Community werden unterstützt. Die Express Edition wird nicht unterstützt. Visual Studio 2017 wird nicht unterstützt. 
* Verwenden Sie das Azure SDK für .NET, Version 2.7.1 oder höher. Führen Sie die Installation mit dem [Webplattform-Installer](http://www.microsoft.com/web/downloads/platform.aspx)durch.
* Installieren Sie die [Stream Analytics-Tools für Visual Studio](http://aka.ms/asatoolsvs).

## <a name="create-a-stream-analytics-project"></a>Erstellen eines Stream Analytics-Projekts
1. Klicken Sie in Visual Studio auf das Menü **Datei**, und wählen Sie **Neues Projekt**. 

2. Wählen Sie in der Vorlagenliste links **Stream Analytics** aus, und klicken Sie dann auf **Azure Stream Analytics Application** (Azure Stream Analytics-Anwendung).

3. Geben Sie für das Projekt **Name**, **Speicherort** und **Lösungsname** ein, wie Sie dies von anderen Projekten kennen.

    ![Fenster „Neues Projekt“](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-create-project-01.png)

    Im **Projektmappen-Explorer** wird das Projekt **Toll** (Maut) generiert.

    ![Generiertes Projekt „Toll“ im Projektmappen-Explorer](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-create-project-02.png)

## <a name="choose-the-correct-subscription"></a>Auswählen des richtigen Abonnements
1. Klicken Sie in Visual Studio auf das Menü **Ansicht**, und öffnen Sie den **Server-Explorer**.

2. Melden Sie sich mit Ihrem Azure-Konto an. 

## <a name="define-the-input-sources"></a>Definieren der Eingabequellen
1.  Erweitern Sie im **Projektmappen-Explorer** den Knoten **Eingaben**, und benennen Sie **Input.json** in **EntryStream.json** um. Doppelklicken Sie auf **EntryStream.json**.
2.  Der **Eingabealias** lautet jetzt **EntryStream**. Der Eingabealias wird im Abfrageskript verwendet. 
3.  Wählen Sie unter **Quelltyp** die Option **Datenstrom**.
4.  Wählen Sie unter **Quelle** die Option **Event Hub**.
5.  Wählen Sie unter **Service Bus-Namespace** die Option **TollData**.
6.  Wählen Sie unter **Event Hub-Name** die Option **entry**.
7.  Wählen Sie unter **Event Hub-Richtlinienname** die Option **RootManageSharedAccessKey** (Standardwert).
8.  Wählen Sie unter **Ereignisserialisierungsformat** die Option **Json**. 
9.  Wählen Sie unter **Codierung** die Option **UTF8**. Ihre Einstellungen sollten wie im folgenden Screenshot aussehen:

    ![Fenster „Eingabe“](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-01.png)
 
10. Klicken Sie zum Beenden des Assistenten auf **Speichern**. Sie können jetzt eine andere Eingabequelle zum Erstellen des Ausgangsdatenstroms hinzufügen. Klicken Sie mit der rechten Maustaste auf den Knoten **Eingaben**, und wählen Sie **Neues Element**.

    ![Neues Element](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-02.png)
 
11. Wählen Sie im Fenster die Option **Azure Stream Analytics Input** (Azure Stream Analytics-Eingabe), und ändern Sie den **Namen** in **ExitStream.json**. Klicken Sie auf **Hinzufügen**.

    ![Fenster „Neues Element hinzufügen“](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-03.png)
 
12. Doppelklicken Sie im Projekt auf **ExitStream.json**, und führen Sie dieselben Schritte wie für den Eingabedatenstrom aus. Achten Sie darauf, dass Sie unter **Event Hub-Name** den Text **exit** eingeben. Dies ist im folgenden Screenshot dargestellt:

    ![Fenster „ExitStream“](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-04.png)

    Sie haben jetzt zwei Eingabedatenströme definiert:

    ![Eingabedatenströme „Entry“ und „Exit“](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-05.png)
 
    Als Nächstes fügen Sie die Referenzdateneingabe für die Blobdatei hinzu, in der die Fahrzeugregistrierungsdaten enthalten sind.

13. Klicken Sie mit der rechten Maustaste auf den Knoten **Eingaben** des Projekts, und führen Sie dann die gleichen Schritte wie für die Datenstromeingaben aus. Geben Sie unter **Eingabealias** den Text **Registration** ein, und wählen Sie unter **Quelltyp** die Option **Referenzdaten**.

    ![Fenster „Registrierung“](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-06.png)

14. Wählen Sie unter **Speicherkonto** die Option **tolldata**. Wählen Sie unter **Container** die Option **tolldata**, und geben Sie unter **Pfadmuster** den Text **registration.json** ein. Für diesen Dateinamen wird die Groß-/Kleinschreibung berücksichtigt, und er sollte nur Kleinbuchstaben enthalten.
15. Klicken Sie zum Beenden des Assistenten auf **Speichern**.

Jetzt sind alle Eingaben definiert.

## <a name="define-the-output"></a>Definieren der Ausgabe
1.  Erweitern Sie im **Projektmappen-Explorer** den Knoten **Eingaben**, und doppelklicken Sie auf **Output.json**.

2.  Geben Sie unter **Ausgabealias** den Text **output** ein. 
3.  Wählen Sie unter **Senke** die Option **SQL-Datenbank**.
4.  Wählen Sie unter **Datenbank** die Option **TollDataDB**.
5.  Geben Sie unter **Benutzername** den Text **tolladmin** ein. 
6.  Geben Sie unter **Kennwort** den Text **123toll!** ein.
7.  Geben Sie unter **Tabelle** den Text **TollDataRefJoin** ein.
8.  Klicken Sie auf **Speichern**.

    ![Fenster „Ausgabe“](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-output-01.png)
 
## <a name="create-a-stream-analytics-query"></a>Erstellen einer Stream Analytics-Abfrage
In diesem Tutorial wird versucht, einige geschäftliche Fragen in Bezug auf Mautdaten zu beantworten. Außerdem werden Stream Analytics-Abfragen erstellt, die in Stream Analytics verwendet werden können, um relevante Antworten zu liefern.
Bevor wir mit dem ersten Stream Analytics-Auftrag beginnen, sehen wir uns ein einfaches Szenario und die Abfragesyntax an.

### <a name="introduction-to-the-stream-analytics-query-language"></a>Einführung in die Stream Analytics-Abfragesprache
Angenommen, Sie müssen die Fahrzeuge zählen, die ein Mauthäuschen passieren. Da es sich bei diesem Beispiel um einen kontinuierlichen Datenstrom von Ereignissen handelt, müssen Sie einen bestimmten Zeitraum definieren. Die Frage wird neu formuliert: „Wie viele Fahrzeuge passieren ein Mauthäuschen jeweils innerhalb von drei Minuten?“ Diese Art der Erfassung von Zähldaten wird normalerweise als „Rollierende Anzahl“ bezeichnet.

Sehen Sie sich die Beantwortung dieser Frage durch die Stream Analytics-Abfrage an:

        SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*) AS Count 
        FROM EntryStream TIMESTAMP BY EntryTime 
        GROUP BY TUMBLINGWINDOW(minute, 3), TollId 

In Stream Analytics wird eine Abfragesprache verwendet, die wie SQL aufgebaut ist und über einige Erweiterungen verfügt, damit die zeitbezogenen Aspekte der Abfrage angegeben werden können.

Weitere Informationen finden Sie in den Artikeln zu [Zeitmanagement](https://msdn.microsoft.com/library/azure/mt582045.aspx)- und [Windowing](https://msdn.microsoft.com/library/azure/dn835019.aspx)-Konstrukten, die in der Abfrage von MSDN verwendet werden.

Nachdem Sie nun Ihre erste Stream Analytics-Abfrage geschrieben haben, können Sie sie testen. Verwenden Sie die Beispieldatendateien, die sich im Ordner „TollApp“ unter dem folgenden Pfad befinden:

..\TollApp\TollApp\Data

Dieser Ordner enthält die folgenden Dateien:
*   Entry.json
*   Exit.json
*   registration.json

## <a name="count-the-number-of-vehicles-entering-a-toll-booth"></a>Zählen der Anzahl von Fahrzeugen, die ein Mauthäuschen passieren
Doppelklicken Sie im Projekt auf **Script.asaql**, um das Skript im **Abfrage-Editor** zu öffnen. Kopieren Sie das Skript aus dem vorherigen Abschnitt, und fügen Sie es in den Editor ein. Der Abfrage-Editor unterstützt IntelliSense, farbige Syntaxmarkierung und den Fehlermarker.

![Abfrage-Editor](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-query-01.png)
 
### <a name="test-stream-analytics-queries-locally"></a>Lokales Testen von Stream Analytics-Abfragen

1. Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **Erstellen**, um die Abfrage zu kompilieren und zu ermitteln, ob ein Syntaxfehler vorliegt. 

2. Zum Überprüfen dieser Abfrage anhand von Beispieldaten können Sie lokale Beispieldaten verwenden. Klicken Sie mit der rechten Maustaste auf die Eingabe, und wählen Sie **Lokale Eingabe hinzufügen**.

    ![Lokale Eingabe hinzufügen](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-01.png)
 
3. Wählen Sie im Popupfenster die Beispieldaten in Ihrem lokalen Pfad aus. Klicken Sie auf **Speichern**.

    ![Fenster „Lokale Eingabe hinzufügen“](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-02.png)
 
    Eine Datei namens **local_EntryStream.json** wird Ihrem Ordner „Eingaben“ automatisch hinzugefügt.

    ![Hinzugefügte Datei im Ordner „Eingaben“](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-03.png)
 
4. Klicken Sie im **Abfrage-Editor** auf **Lokal ausführen**. Sie können auch die Taste F5 drücken.

    ![Lokal ausführen](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-local-run-01.png)

    ![Ausgabe der lokalen Ausführung](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-local-run-02.png)

    Drücken Sie eine beliebige Taste, um die Ausgabe in Visual Studio im Fenster **Ergebnis der lokalen ASA-Ausführung** anzuzeigen. 

    ![Fenster „Ergebnis der lokalen ASA-Ausführung“](./media/stream-analytics-tools-for-vs/local-testing-output.png)

5. Klicken Sie auf **Ergebnisordner öffnen**, um die Ausgabedateien sowohl im CSV- als auch im JSON-Format zu überprüfen.

    ![Ausgabe von „Ergebnisordner öffnen“](./media/stream-analytics-tools-for-vs/local-testing-files.png)
 

### <a name="sample-the-input-data"></a>Nehmen von Stichproben der Eingabedaten
Sie können auch Stichproben von Eingabedaten aus Eingabequellen nehmen und in einer lokalen Datei speichern. 
1. Klicken Sie mit der rechten Maustaste auf die Eingabekonfigurationsdatei, und wählen Sie **Beispieldaten**. 

   ![Beispieldaten](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-01.png)

    Derzeit können Sie Stichproben nur für Event Hubs oder IoT Hubs nehmen. Andere Eingabequellen werden nicht unterstützt.

2. Geben Sie im Popupfenster den lokalen Pfad ein, der zum Speichern von Beispieldaten verwendet wird. Klicken Sie auf **Beispiel**.

    ![Fenster „Beispieldaten“](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-02.png)
 
    Sie können den Status im Fenster **Ausgabe** anzeigen. 

    ![Fenster „Ausgabe“](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-03.png)
 
### <a name="submit-a-stream-analytics-query-to-azure"></a>Übermitteln einer Stream Analytics-Abfrage an Azure
1. Klicken Sie im **Abfrage-Editor** im Skript-Editor auf **An Azure übermitteln**.

    ![An Azure übermitteln](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-submit-job-01.png)
 
2. Wählen Sie die Option **Neuen Azure Stream Analytics-Auftrag erstellen**. Geben Sie unter **Auftragsname** einen Namen ein, und wählen Sie das richtige **Abonnement** aus. Klicken Sie auf **Senden**.

    ![Fenster „Auftrag übermitteln“](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-submit-job-02.png)

 
### <a name="start-a-job"></a>Starten eines Auftrags
Nach der Erstellung Ihres Auftrags wird automatisch die Auftragsansicht geöffnet. 
1. Klicken Sie zum Starten des Auftrags auf die Schaltfläche mit dem **grünen Pfeil**.

    ![Starten eines Auftrags](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-01.png)
 
2. Wählen Sie die Standardeinstellung, und klicken Sie auf **Start**.
 
    ![Fenster „Auftrag starten“](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-02.png)

    Der **Status** des Auftrags ändert sich in **Wird ausgeführt**, und die **Eingabeereignisse** und **Ausgabeereignisse** werden angezeigt.

    ![Status „Wird ausgeführt“ in der Auftragszusammenfassung](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-03.png)

## <a name="check-the-results-in-visual-studio"></a>Überprüfen der Ergebnisse in Visual Studio
1. Öffnen Sie in Visual Studio den **Server-Explorer**, und klicken Sie mit der rechten Maustaste auf die Tabelle **TollDataRefJoin**.
2. Wählen Sie die Option **Tabellendaten anzeigen**, um die Ausgabe des Auftrags anzuzeigen.
   
    ![Auswahl von „Tabellendaten anzeigen“ im Server-Explorer](media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-check-results.jpg)


### <a name="view-the-job-metrics"></a>Anzeigen der Auftragsmetriken
Einige grundlegende Auftragsstatistiken finden Sie in **Auftragsmetriken**. 

![Fenster „Auftragsmetriken“](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-job-metrics-01.png)

 
## <a name="list-the-job-in-server-explorer"></a>Auflisten des Auftrags im Server-Explorer
Klicken Sie im **Server-Explorer** auf **Stream Analytics-Aufträge** und dann auf **Aktualisieren**. Der Auftrag wird unter **Stream Analytics-Aufträge** angezeigt.

![Im Server-Explorer aufgeführte Stream Analytics-Aufträge](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-list-jobs-01.png)


## <a name="open-the-job-view"></a>Öffnen der Auftragsansicht
Erweitern Sie den Auftragsknoten, und doppelklicken Sie auf den Knoten **Auftragsansicht**, um die Auftragsansicht zu öffnen.

![Knoten „Auftragsansicht“](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-job-view-01.png)


## <a name="export-an-existing-job-to-a-project"></a>Exportieren eines vorhandenen Auftrags in ein Projekt
Es gibt zwei Möglichkeiten zum Exportieren eines vorhandenen Auftrags in ein Projekt.

Klicken Sie im **Server-Explorer** mit der rechten Maustaste im Knoten **Stream Analytics-Aufträge** auf den Auftragsknoten, und wählen Sie **In neues Stream Analytics-Projekt exportieren**.

![In neues Stream Analytics-Projekt exportieren](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-01.png)

Das Projekt wird im **Projektmappen-Explorer** generiert.

![Im Projektmappen-Explorer generiertes Projekt](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-02.png)
 
Sie können auch die Auftragsansicht verwenden und auf **Projekt generieren** klicken.

![Projekt generieren](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-03.png)

## <a name="known-issues-and-limitations"></a>Bekannte Probleme und Einschränkungen
 
- Die Power BI-Ausgabe und die Azure Date Lake Store-Ausgabe werden nicht unterstützt.
- Es ist keine Editorunterstützung für das Hinzufügen oder Ändern von benutzerdefinierten JavaScript-Funktionen vorhanden.

## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics: Betrugserkennung in Echtzeit](stream-analytics-real-time-fraud-detection.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

