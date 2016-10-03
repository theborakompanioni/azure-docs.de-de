<properties
	pageTitle="Stimmungsanalysen mit Azure Stream Analytics und Azure Machine Learning | Microsoft Azure"
	description="Verwenden von Machine Learning und einer benutzerdefinierten Funktion in einem Stream Analytics-Auftrag"
	keywords=""
	documentationCenter=""
	services="stream-analytics"
	authors="jeffstokes72"
	manager="jhubbard"
	editor="cgronlun"
/>


<tags 
	ms.service="stream-analytics" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.tgt_pltfrm="na" 
	ms.workload="data-services" 
	ms.date="08/08/2016" 
	ms.author="jeffstok"
/>

# Stimmungsanalysen mit Azure Stream Analytics und Azure Machine Learning #

Dieser Artikel unterstützt Sie dabei, schnell einen einfachen Azure Stream Analytics-Auftrags mit Azure Machine Learning-Integration einzurichten. Wir nutzen ein Stimmungsanalysemodell für Machine Learning aus dem Cortana Intelligence-Katalog, um Streamingtextdaten zu analysieren und den Stimmungswert in Echtzeit zu ermitteln. Die Informationen in diesem Artikel helfen dabei, folgende Szenarien zu verstehen: Echtzeit-Stimmungsanalysen für Twitter-Streamingdaten, Analysieren der Aufzeichnungen von Kundenchats mit Supportmitarbeitern, Auswerten von Kommentaren zu Foren/Blogs/Videos sowie viele weitere Szenarien für das Predictive Scoring in Echtzeit.

Dieser Artikel bietet eine CSV-Beispieldatei mit Text als Eingabe für den Azure-Blobspeicher, wie in der folgenden Abbildung gezeigt. Der Auftrag wendet das Stimmungsanalysemodell als benutzerdefinierte Funktion (User-Defined Function, UDF) auf die Beispieltextdaten aus dem Blobspeicher an. Das Endergebnis wird in den gleichen Blobspeicher in einer anderen CSV-Datei eingefügt.

![Stream Analytics und Machine Learning](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-2.png)

Die folgende Abbildung veranschaulicht diese Konfiguration. Um ein realistischeres Szenario zu erhalten, können Sie den Blobspeicher durch Twitter-Streamingdaten aus einer Azure Event Hubs-Eingabe ersetzen. Außerdem können Sie eine [Microsoft Power BI](https://powerbi.microsoft.com/)-Echtzeitvisualisierung der zusammengefassten Stimmung erstellen.

![Stream Analytics und Machine Learning](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-1.png)

## Voraussetzungen

Folgende Voraussetzungen müssen erfüllt sein, damit Sie die in diesem Artikel beschriebenen Aufgaben ausführen können:

-	Ein aktives Azure-Abonnement.
-	Eine CSV-Datei mit einigen Daten. Sie können die in Abbildung 1 gezeigte Datei aus [GitHub](https://github.com/jeffstokes72/azure-stream-analytics-repository/blob/master/sampleinputs.csv) herunterladen oder selbst eine Datei erstellen. In diesem Artikel wird davon ausgegangen, dass Sie die Datei aus GitHub verwenden.

Im Allgemeinen führen Sie mit den Aufgaben in diesem Artikel folgende Aktionen aus:

1.	Sie laden die CSV-Eingabedatei in den Azure-Blobspeicher hoch.
2.	Sie fügen ein Stimmungsanalysemodell aus dem Cortana Intelligence-Katalog zu Ihrem Azure Machine Learning-Arbeitsbereich hinzu.
3.	Sie stellen dieses Modell als Webdienst im Machine Learning-Arbeitsbereich bereit.
4.	Sie erstellen einen Stream Analytics-Auftrag, der diesen Webdienst als Funktion aufruft, um die Stimmung für die Texteingabe zu ermitteln.
5.	Sie starten den Stream Analytics-Auftrags und beobachten die Ausgabe.

## Hochladen der CSV-Eingabedatei in den Blobspeicher

Für diesen Schritt können Sie eine beliebige CSV-Datei verwenden, beispielsweise die Datei, die für diesen Zweck aus GitHub heruntergeladen werden kann. Zum Hochladen der Datei können Sie den [Azure-Speicher-Explorer](http://storageexplorer.com/), Visual Studio oder auch benutzerdefinierten Code verwenden. Wir verwenden in diesem Beispiel Visual Studio.

1.	Klicken Sie in Visual Studio auf **Azure** > **Speicher** > **Externen Speicher anfügen**. Geben Sie einen **Kontonamen** und einen **Kontoschlüssel** ein.

    ![Stream Analytics und Machine Learning, Server-Explorer](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-server-explorer.png)

2.	Erweitern Sie den in Schritt 1 angefügten Speicher, klicken Sie auf **Blobcontainer erstellen**, und geben Sie einen logischen Namen an. Öffnen Sie den erstellten Container, um seinen Inhalt anzuzeigen. (Zu diesem Zeitpunkt ist der Container leer.)

    ![Stream Analytics und Machine Learning, Blob erstellen](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-create-blob.png)

3.	Um die CSV-Datei hochzuladen, klicken Sie auf **Blob hochladen**, und klicken Sie dann auf **Datei vom lokalen Datenträger**.

## Hinzufügen des Stimmungsanalysemodells aus dem Cortana Analytics-Katalog

1.	Laden Sie das [Predictive Experiment – Mini Twitter sentiment analysis](https://gallery.cortanaintelligence.com/Experiment/Predictive-Mini-Twitter-sentiment-analysis-Experiment-1) aus dem Cortana Intelligence-Katalog herunter.
2.	Klicken Sie in Machine Learning Studio auf **In Studio öffnen**.

    ![Stream Analytics und Machine Learning, Machine Learning Studio öffnen](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-open-ml-studio.png)

3.	Melden Sie sich an, um zum Arbeitsbereich zu gelangen. Wählen Sie den Speicherort aus, der für Sie am besten geeignet ist.
4.	Klicken Sie unten auf der Seite auf **Ausführen**.
5.	Klicken Sie nach erfolgreicher Ausführung des Prozesses auf **Webdienst bereitstellen**.
6.	Das Stimmungsanalysemodell ist jetzt zur Verwendung bereit. Klicken Sie zum Überprüfen auf die Schaltfläche **Test**, und geben Sie Text ein, z.B. „I love Microsoft“. Daraufhin sollte ein Ergebnis ähnlich dem folgenden zurückgegeben werden:

`'Predictive Mini Twitter sentiment analysis Experiment' test returned ["4","0.715057671070099"]...`

![Stream Analytics und Machine Learning, Analysedaten](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-analysis-data.png)

Klicken Sie in der Spalte **Apps** auf den Link zu Ihrer **Excel-Arbeitsmappe (Excel 2010 oder früher)**, um Ihren API-Schlüssel und die URL abzurufen, die Sie später zum Einrichten des Stream Analytics-Auftrags benötigen. (Dieser Schritt ist nur erforderlich, um ein Machine Learning-Modell aus einem anderen Arbeitsbereich des Azure-Kontos zu nutzen. In diesem Artikel wird davon ausgegangen, dass dies der Fall ist, um dieses Szenario zu ermöglichen.)

Notieren Sie sich die Webdienst-URL und den Zugriffsschlüssel aus der heruntergeladenen Excel-Datei wie unten gezeigt:

![Stream Analytics und Machine Learning auf einen Blick](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-quick-glance.png)

## Erstellen eines Stream Analytics-Auftrags unter Verwendung des Machine Learning-Modells

1.	Öffnen Sie das [Azure-Portal](https://manage.windowsazure.com).
2.	Klicken Sie auf **Neu** > **Datendienste** > **Stream Analytics** > **Schnellerfassung**. Geben Sie unter **Auftragsname** einen Namen für Ihren Auftrag ein, geben Sie unter **Region** eine geeignete Region für den Auftrag ein, und wählen Sie dann das Konto in **Speicherkonto für regionale Überwachung** aus.
3.	Klicken Sie nach dem Erstellen des Auftrags auf der Registerkarte **Eingaben** auf **Eingabe hinzufügen**.

    ![Stream Analytics und Machine Learning, Machine Learning-Eingabe hinzufügen](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-add-input-screen.png)

4.	Klicken Sie auf der ersten Seite des Assistenten **Eingabe hinzufügen** auf **Datenstrom**, und klicken Sie auf **Weiter**. Wählen Sie auf der nächsten Seite **Blobspeicher** als Eingabe aus, und klicken Sie auf **Weiter**.
5.	Geben Sie auf der Seite **Einstellungen des Blobspeichers** den Namen des Speicherkonto-Blobcontainers an, den Sie zuvor beim Hochladen der Daten definiert haben. Klicken Sie auf **Weiter**. Klicken Sie unter **Ereignisserialisierungsformat** auf **CSV**. Übernehmen Sie für die restlichen **Serialisierungseinstellungen** die Standardwerte. Klicken Sie auf **OK**.
6.	Klicken Sie auf der Registerkarte **Ausgaben** auf **Ausgabe hinzufügen**.

    ![Stream Analytics und Machine Learning, Ausgabe hinzufügen](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-add-output-screen.png)

7.	Klicken Sie auf **Blobspeicher**, und geben Sie die gleichen Parameter an, außer für den Container. Der Wert für die **Eingabe** wird per Konfiguration aus dem Container mit dem Namen „test“ gelesen, in den die **CSV**-Datei hochgeladen wurde. Geben Sie unter **Ausgabe** die Zeichenfolge „testoutput“ ein. Die Containernamen müssen unterschiedlich sein. Überprüfen Sie, ob dieser Container vorhanden ist.
8.	Klicken Sie auf **Weiter**, um die **Serialisierungseinstellungen** der Ausgabe zu konfigurieren. Wählen Sie wie bei der **Eingabe** die Option **CSV**, und klicken Sie auf die Schaltfläche **OK**.
9.	Klicken Sie auf der Registerkarte **Funktionen** auf **Machine Learning-Funktion hinzufügen**.

    ![Stream Analytics und Machine Learning, Machine Learning-Funktion hinzufügen](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-add-ml-function.png)

10.	Suchen Sie auf der Seite **Machine Learning-Webdiensteinstellungen** nach dem Arbeitsbereich, dem Webdienst und dem Standardendpunkt für Machine Learning. Um sich mit der Konfiguration eines Webdiensts für einen Arbeitsbereich vertraut zu machen, wenden Sie die Einstellungen für diesen Artikel manuell an, sofern Ihnen die URL und der API-Schlüssel bekannt sind. Geben Sie die **URL** und den **API-Schlüssel** für den Endpunkt an. Klicken Sie auf **OK**.

    ![Stream Analytics und Machine Learning, Machine Learning-Webdienst](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-ml-web-service.png)

11.	Ändern Sie die Abfrage auf der Registerkarte **Abfrage** wie folgt:

 ```
 	WITH subquery AS (  
 		SELECT text, sentiment(text) as result from input  
  	)  
 
 	Select text, result.[Scored Labels]  
 	Into output  
 	From subquery  
 ```    
12.	Klicken Sie auf **Speichern**, um die Abfrage zu speichern.

## Starten des Stream Analytics-Auftrags und Verfolgen der Ausgabe

1.	Klicken Sie unten auf der Seite des Auftrags auf **Starten**.
2.	Wählen Sie im Dialogfeld **Abfrage starten** die Option **Benutzerdefinierte Uhrzeit**, und wählen Sie einen Zeitpunkt aus, der vor dem Zeitpunkt des Hochladens der CSV-Datei in den Blobspeicher liegt. Klicken Sie auf **OK**.

    ![Stream Analytics und Machine Learning, benutzerdefinierter Zeitpunkt](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-custom-time.png)

3.	Wechseln Sie mit dem gleichen Tool, das Sie zum Hochladen der CSV-Datei verwendet haben (z.B. Visual Studio) zum Blobspeicher.
4.	Einige Minuten nach dem Starten des Auftrags wird der Ausgabecontainer erstellt und eine CSV-Datei in den Container hochgeladen.
5.	Öffnen Sie die Datei im Ihrem Standardeditor für CSV-Dateien. Es sollte eine Tabelle ähnlich der folgenden angezeigt werden:

    ![Stream Analytics und Machine Learning, CSV-Ansicht](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-csv-view.png)

## Zusammenfassung

Dieser Artikel veranschaulicht, wie Sie einen Stream Analytics-Auftrag erstellen, der Streamingtextdaten liest und in Echtzeit Stimmungsanalysen auf die Daten anwendet. All dies ist möglich, ohne dass Sie sich mit der komplexen Erstellung eines Stimmungsanalysemodells beschäftigen müssen. Dies ist einer der Vorteile der Cortana Intelligence Suite.

Sie können auch auf Azure Machine Learning-Funktionen bezogene Metriken anzeigen. Klicken Sie zu diesem Zweck auf die Registerkarte **Überwachen**. Es werden drei funktionsbezogene Metriken angezeigt.

- **Funktionsanforderungen** gibt die Anzahl von Anforderungen an, die an den Machine Learning-Webdienst gesendet werden.
- **Funktionsereignisse** gibt die Anzahl von Ereignissen in der Anforderung an. Standardmäßig enthält jede Anforderung an einen Machine Learning-Webdienst bis zu 1000 Ereignisse.
- **Fehler bei Funktionsanforderungen** gibt die Anzahl von Anforderungen an den Machine Learning-Webdienst an, bei denen Fehler aufgetreten sind.

    ![Stream Analytics und Machine Learning, Machine Learning-Überwachungsansicht](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-ml-monitor-view.png)

<!---HONumber=AcomDC_0921_2016-->