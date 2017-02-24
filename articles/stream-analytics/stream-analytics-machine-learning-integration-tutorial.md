---
title: Azure Stream Analytics und Machine Learning-Integration | Microsoft-Dokumentation
description: Verwenden von Machine Learning und einer benutzerdefinierten Funktion in einem Stream Analytics-Auftrag
keywords: 
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: cfced01f-ccaa-4bc6-81e2-c03d1470a7a2
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 02/14/2017
ms.author: jeffstok
translationtype: Human Translation
ms.sourcegitcommit: d1ffb9aba0eb1e17d1efd913f536f6f3997fccbb
ms.openlocfilehash: 7b635b1810536f5b3eb1371d687e9c355e604e41

---

# <a name="sentiment-analysis-by-using-azure-stream-analytics-and-azure-machine-learning"></a>Stimmungsanalysen mit Azure Stream Analytics und Azure Machine Learning
Dieser Artikel unterstützt Sie dabei, schnell einen einfachen Azure Stream Analytics-Auftrags mit Azure Machine Learning-Integration einzurichten. Wir nutzen ein Stimmungsanalysemodell für Machine Learning aus dem Cortana Intelligence-Katalog, um Streamingtextdaten zu analysieren und den Stimmungswert in Echtzeit zu ermitteln. Die Informationen in diesem Artikel helfen dabei, folgende Szenarien zu verstehen: Echtzeit-Stimmungsanalysen für Twitter-Streamingdaten, Analysieren der Aufzeichnungen von Kundenchats mit Supportmitarbeitern, Auswerten von Kommentaren zu Foren/Blogs/Videos sowie viele weitere Szenarien für das Predictive Scoring in Echtzeit.

Dieser Artikel bietet eine CSV-Beispieldatei mit Text als Eingabe für den Azure-Blobspeicher, wie in der folgenden Abbildung gezeigt. Der Auftrag wendet das Stimmungsanalysemodell als benutzerdefinierte Funktion (User-Defined Function, UDF) auf die Beispieltextdaten aus dem Blobspeicher an. Das Endergebnis wird in den gleichen Blobspeicher in einer anderen CSV-Datei eingefügt. 

![Stream Analytics und Machine Learning](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-2.png)  

Die folgende Abbildung veranschaulicht diese Konfiguration. Um ein realistischeres Szenario zu erhalten, können Sie den Blobspeicher durch Twitter-Streamingdaten aus einer Azure Event Hubs-Eingabe ersetzen. Außerdem können Sie eine [Microsoft Power BI](https://powerbi.microsoft.com/) -Echtzeitvisualisierung der zusammengefassten Stimmung erstellen.    

![Stream Analytics und Machine Learning](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-1.png)  

## <a name="prerequisites"></a>Voraussetzungen
Folgende Voraussetzungen müssen erfüllt sein, damit Sie die in diesem Artikel beschriebenen Aufgaben ausführen können:

* Ein aktives Azure-Abonnement.
* Eine CSV-Datei mit einigen Daten. Sie können die in Abbildung 1 gezeigte Datei aus [GitHub](https://github.com/Azure/azure-stream-analytics/blob/master/Sample Data/sampleinput.csv) herunterladen oder selbst eine Datei erstellen. In diesem Artikel wird davon ausgegangen, dass Sie die Datei aus GitHub verwenden.

Im Allgemeinen führen Sie mit den Aufgaben in diesem Artikel folgende Aktionen aus:

1. Sie laden die CSV-Eingabedatei in den Azure-Blobspeicher hoch.
2. Sie fügen ein Stimmungsanalysemodell aus dem Cortana Intelligence-Katalog zu Ihrem Azure Machine Learning-Arbeitsbereich hinzu.
3. Sie stellen dieses Modell als Webdienst im Machine Learning-Arbeitsbereich bereit.
4. Sie erstellen einen Stream Analytics-Auftrag, der diesen Webdienst als Funktion aufruft, um die Stimmung für die Texteingabe zu ermitteln.
5. Sie starten den Stream Analytics-Auftrags und beobachten die Ausgabe.

## <a name="create-a-storage-blob-and-upload-the-csv-input-file"></a>Erstellen eines Speicherblobs und Hochladen der CSV-Eingabedatei
Für diesen Schritt können Sie eine beliebige CSV-Datei verwenden, beispielsweise die Datei, die für diesen Zweck aus GitHub heruntergeladen werden kann. Das Hochladen der CSV-Datei ist einfach, da es sich um eine Option bei der Erstellung eines Speicherblobs handelt.

Erstellen Sie für unser Tutorial ein neues Speicherkonto, indem Sie auf **Neu** klicken und dann nach „Speicherkonto“ suchen. Klicken Sie anschließend auf das resultierende Symbol des Speicherkontos, und geben Sie Details für die Erstellung des Kontos an. Geben Sie einen **Namen** („azuresamldemosa“ in diesem Beispiel) ein. Erstellen Sie eine neue oder nutzen Sie eine vorhandene **Ressourcengruppe**, und geben Sie einen **Standort** an (hierbei ist es wichtig, dass alle in dieser Demo erstellten Ressourcen nach Möglichkeit denselben Standort verwenden).

![Erstellen eines Speicherkontos](./media/stream-analytics-machine-learning-integration-tutorial/create-sa.png)

Im Anschluss können Sie auf den Blobdienst klicken und einen Blobcontainer erstellen.

![Erstellen eines Blobcontainers](./media/stream-analytics-machine-learning-integration-tutorial/create-sa2.png)

Geben Sie dann einen **Namen** für den Container („azuresamldemoblob“ in diesem Beispiel) ein, und vergewissern Sie sich, dass **Zugriffstyp** auf „Blob“ festgelegt ist.

![Festlegen des Zugriffstyps auf „Blob“](./media/stream-analytics-machine-learning-integration-tutorial/create-sa3.png)

Nun können wir das Blob mit unseren Daten auffüllen. Wählen Sie **Dateien** und dann die Datei auf dem lokalen Laufwerk aus, die Sie von GitHub heruntergeladen haben. Ich habe „Blockblob“ und „4 MB“ als Größe ausgewählt. Diese Angaben sollten für diese Demo in Ordnung sein. Wählen Sie dann **Hochladen** aus, woraufhin im Portal ein Blob mit dem Textbeispiel erstellt wird.

![Erstellen der Datei für „Blob hochladen“](./media/stream-analytics-machine-learning-integration-tutorial/create-sa4.png)

Nun da sich die Beispieldaten in einem Blob befinden, ist es Zeit, das Standpunktanalysemodell im Cortana Intelligence-Katalog zu aktivieren.

## <a name="add-the-sentiment-analytics-model-from-the-cortana-intelligence-gallery"></a>Hinzufügen des Stimmungsanalysemodells aus dem Cortana Analytics-Katalog
1. Laden Sie das [Predictive Experiment – Mini Twitter sentiment analysis](https://gallery.cortanaintelligence.com/Experiment/Predictive-Mini-Twitter-sentiment-analysis-Experiment-1) aus dem Cortana Intelligence-Katalog herunter.  
2. Klicken Sie in Machine Learning Studio auf **Open in Studio**.  
   
   ![Stream Analytics und Machine Learning, Machine Learning Studio öffnen](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-open-ml-studio.png)  

3. Melden Sie sich an, um zum Arbeitsbereich zu gelangen. Wählen Sie den Speicherort aus, der für Sie am besten geeignet ist.
4. Klicken Sie unten auf der Seite auf **Ausführen** .  
5. Klicken Sie nach erfolgreicher Ausführung des Prozesses auf **Deploy Web Service**.
6. Das Stimmungsanalysemodell ist jetzt zur Verwendung bereit. Klicken Sie zum Überprüfen auf die Schaltfläche **Test**, und geben Sie Text ein, z.B. „I love Microsoft“. Daraufhin sollte ein Ergebnis ähnlich dem folgenden zurückgegeben werden:

`'Predictive Mini Twitter sentiment analysis Experiment' test returned ["4","0.715057671070099"]...`  

![Stream Analytics und Machine Learning, Analysedaten](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-analysis-data.png)  

Klicken Sie in der Spalte **Apps** auf den Link zu Ihrer **Excel-Arbeitsmappe (Excel 2010 oder früher)**, um Ihren API-Schlüssel und die URL abzurufen, die Sie später zum Einrichten des Stream Analytics-Auftrags benötigen. (Dieser Schritt ist nur erforderlich, um ein Machine Learning-Modell aus einem anderen Arbeitsbereich des Azure-Kontos zu nutzen. In diesem Artikel wird davon ausgegangen, dass dies der Fall ist, um dieses Szenario zu ermöglichen.)  

Notieren Sie sich die Webdienst-URL und den Zugriffsschlüssel aus der heruntergeladenen Excel-Datei wie unten gezeigt:  

![Stream Analytics und Machine Learning auf einen Blick](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-quick-glance.png)  

## <a name="create-a-stream-analytics-job-that-uses-the-machine-learning-model"></a>Erstellen eines Stream Analytics-Auftrags unter Verwendung des Machine Learning-Modells
1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).  
2. Klicken Sie auf **Neu** > **Intelligence + Analyse** > **Stream Analytics**. Geben Sie in **Auftragsname** einen Namen für Ihren Auftrag ein. Geben Sie entweder eine vorhandene Ressourcengruppe an, oder erstellen Sie eine neue. Geben Sie dann in das Feld **Standort** den dem Auftrag entsprechenden Standort an.    
   
   ![Erstellen des Auftrags](./media/stream-analytics-machine-learning-integration-tutorial/create-job-1.png)
   
3. Klicken Sie nach Erstellen des Auftrags auf der Registerkarte **Eingaben** auf **Eingabe hinzufügen**.  
   
   ![Stream Analytics und Machine Learning, Machine Learning-Eingabe hinzufügen](./media/stream-analytics-machine-learning-integration-tutorial/create-job-add-input.png)  

4. Wählen Sie **Hinzufügen** aus, und geben Sie dann einen **Eingabealias** an. Wählen Sie **Datenstrom**, **Blobspeicher** als Eingabe aus, und klicken Sie dann auf **Weiter**.  
5. Geben Sie auf der Seite **Einstellungen des Blobspeichers** den Namen des Speicherkonto-Blobcontainers an, den Sie zuvor beim Hochladen der Daten definiert haben. Klicken Sie auf **Weiter**. Wählen Sie unter **Ereignisserialisierungsformat** die Option **CSV** aus. Übernehmen Sie für die restlichen **Serialisierungseinstellungen** die Standardwerte. Klicken Sie auf **OK**.  
   
   ![Hinzufügen des Eingabeblobcontainers](./media/stream-analytics-machine-learning-integration-tutorial/create-job-add-input-blob.png)

6. Klicken Sie auf der Registerkarte **Ausgaben** auf **Ausgabe hinzufügen**.  
   
   ![Stream Analytics und Machine Learning, Ausgabe hinzufügen](./media/stream-analytics-machine-learning-integration-tutorial/create-output.png)  

7. Klicken Sie auf **Blobspeicher**, und geben Sie die gleichen Parameter an, außer für den Container. Der Wert für die **Eingabe** wird per Konfiguration aus dem Container mit dem Namen „test“ gelesen, in den die **CSV-Datei** hochgeladen wurde. Geben Sie unter **Ausgabe**die Zeichenfolge „testoutput“ ein.
8. Prüfen Sie, ob die **Serialisierungseinstellungen** der Ausgabe auf **CSV** festgelegt sind, und klicken Sie dann auf die Schaltfläche **OK**.
   
   ![Stream Analytics und Machine Learning, Ausgabe hinzufügen](./media/stream-analytics-machine-learning-integration-tutorial/create-output2.png) 

9. Klicken Sie auf der Registerkarte **Funktionen** auf **Machine Learning-Funktion hinzufügen**.  
   
   ![Stream Analytics und Machine Learning, Machine Learning-Funktion hinzufügen](./media/stream-analytics-machine-learning-integration-tutorial/add-function.png)  

10. Suchen Sie auf der Seite **Machine Learning-Webdiensteinstellungen** nach dem Arbeitsbereich, dem Webdienst und dem Standardendpunkt für Machine Learning. Um sich mit der Konfiguration eines Webdiensts für einen Arbeitsbereich vertraut zu machen, wenden Sie die Einstellungen für diesen Artikel manuell an, sofern Ihnen die URL und der API-Schlüssel bekannt sind. Geben Sie die **URL** und den **API-Schlüssel** für den Endpunkt an. Klicken Sie auf **OK**. Beachten Sie, dass der **Funktionsalias** „Standpunkt“ ist.  
    
    ![Stream Analytics und Machine Learning, Machine Learning-Webdienst](./media/stream-analytics-machine-learning-integration-tutorial/add-function-endpoints.png)    

11. Ändern Sie die Abfrage auf der Registerkarte **Abfrage** wie folgt:    
    
    ```
    WITH sentiment AS (  
      SELECT text, sentiment(text) as result from datainput  
    )  
    
    Select text, result.[Scored Labels]  
    Into testoutput  
    From sentiment  
    ```    

12. Klicken Sie auf **Speichern** , um die Abfrage zu speichern.

## <a name="start-the-stream-analytics-job-and-observe-the-output"></a>Starten des Stream Analytics-Auftrags und Verfolgen der Ausgabe
1. Klicken Sie oben auf der Seite des Auftrags auf **Starten**.
2. Klicken Sie im Dialogfeld **Abfrage starten** auf die Option **Benutzerdefinierte Uhrzeit**, und wählen Sie einen Zeitpunkt aus, der vor dem Zeitpunkt des Hochladens der CSV-Datei in den Blobspeicher liegt. Klicken Sie auf **OK**.  
3. Wechseln Sie mit dem gleichen Tool, das Sie zum Hochladen der CSV-Datei verwendet haben (z.B. Visual Studio) zum Blobspeicher.
4. Einige Minuten nach dem Starten des Auftrags wird der Ausgabecontainer erstellt und eine CSV-Datei in den Container hochgeladen.  
5. Öffnen Sie die Datei im Ihrem Standardeditor für CSV-Dateien. Es sollte eine Tabelle ähnlich der folgenden angezeigt werden:  
   
   ![Stream Analytics und Machine Learning, CSV-Ansicht](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-csv-view.png)  

## <a name="conclusion"></a>Zusammenfassung
Dieser Artikel veranschaulicht, wie Sie einen Stream Analytics-Auftrag erstellen, der Streamingtextdaten liest und in Echtzeit Stimmungsanalysen auf die Daten anwendet. All dies ist möglich, ohne dass Sie sich mit der komplexen Erstellung eines Stimmungsanalysemodells beschäftigen müssen. Dies ist einer der Vorteile der Cortana Intelligence Suite.

Sie können auch auf Azure Machine Learning-Funktionen bezogene Metriken anzeigen. Klicken Sie zu diesem Zweck auf die Registerkarte **Überwachen**. Es werden drei funktionsbezogene Metriken angezeigt.  

* **Funktionsanforderungen** gibt die Anzahl von Anforderungen an, die an den Machine Learning-Webdienst gesendet werden.  
* **Funktionsereignisse** gibt die Anzahl von Ereignissen in der Anforderung an. Standardmäßig enthält jede Anforderung an einen Machine Learning-Webdienst bis zu 1000 Ereignisse.  
  
    ![Stream Analytics und Machine Learning, Machine Learning-Überwachungsansicht](./media/stream-analytics-machine-learning-integration-tutorial/job-monitor.png)  




<!--HONumber=Feb17_HO2-->


