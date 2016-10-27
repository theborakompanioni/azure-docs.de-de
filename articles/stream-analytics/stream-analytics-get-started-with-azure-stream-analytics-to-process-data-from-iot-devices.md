<properties
    pageTitle="Erste Schritte mit Azure Stream Analytics zum Verarbeiten der Daten von IoT-Geräten | Microsoft Azure"
    description="IoT-Sensortags und -Datenströme mit Stream Analytics und Echtzeit-Datenverarbeitung"
    keywords="IoT-Lösung, erste Schritte mit IoT"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>


# <a name="get-started-with-azure-stream-analytics-to-process-data-from-iot-devices"></a>Erste Schritte mit Azure Stream Analytics zum Verarbeiten der Daten von IoT-Geräten

In diesem Tutorial erfahren Sie, wie Sie Datenstrom-Verarbeitungslogik erstellen, um Daten von IoT-Geräten (Internet of Things) zu erfassen. Wir verwenden einen echten IoT-Anwendungsfall aus der Praxis, um zu zeigen, wie Sie Ihre Lösung schnell und wirtschaftlich erstellen.

## <a name="prerequisites"></a>Voraussetzungen

-   [Azure-Abonnement](https://azure.microsoft.com/pricing/free-trial/)
-   Beispielabfrage und Datendateien (bei [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot)

## <a name="scenario"></a>Szenario

Contoso, ein Unternehmen im Bereich der industriellen Automation, hat seinen Fertigungsprozess vollständig automatisiert. Die Maschinen im Werk verfügen über Sensoren, die Datenströme in Echtzeit ausgeben können. In diesem Szenario möchte ein Production Floor Manager in Echtzeit Einblick in die Sensordaten erhalten, um darin nach Mustern zu suchen und entsprechend darauf zu reagieren. Wir verwenden die Stream Analytics Query Language (SAQL) für die Sensordaten, um für den eingehenden Datenstrom interessante Muster zu ermitteln.

In diesem Fall werden die Daten mit einem SensorTag-Gerät von Texas Instruments generiert.

![Texas Instruments SensorTag](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-01.jpg)

Die Nutzlast der Daten liegt im JSON-Format vor und sieht wie folgt aus:


    {
        "time": "2016-01-26T20:47:53.0000000",  
        "dspl": "sensorE",  
        "temp": 123,  
        "hmdt": 34  
    }  

In der Praxis können Hunderte dieser Sensoren einen Datenstrom mit Ereignissen generieren. Idealerweise führt ein Gatewaygerät hierbei Code aus, um diese Ereignisse mittels Push an [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) oder [Azure IoT Hubs](https://azure.microsoft.com/services/iot-hub/) zu übertragen. Ihr Stream Analytics-Auftrag erfasst diese Ereignisse über Event Hubs und führt in Echtzeit Analyseabfragen für die Datenströme aus. Die Ergebnisse können dann an eine der [unterstützten Ausgaben](stream-analytics-define-outputs.md) gesendet werden.

Zur einfacheren Verwendung enthält dieser Leitfaden zu den ersten Schritten eine Datei mit Beispieldaten, die mit echten SensorTag-Geräten erfasst wurden. Sie können Abfragen für die Beispieldaten ausführen und Ergebnisse anzeigen. In den nachfolgenden Tutorials erfahren Sie, wie Sie Ihren Auftrag mit Eingaben und Ausgaben verbinden und für den Azure-Dienst bereitstellen.

## <a name="create-a-stream-analytics-job"></a>Erstellen eines Stream Analytics-Auftrags

1. Klicken Sie im [Azure-Portal](http://manage.windowsazure.com) auf **STREAM ANALYTICS** und dann in der unteren linken Ecke der Seite auf **NEU**, um einen neuen Analytics-Auftrag zu erstellen.

    ![Neuen Stream Analytics-Auftrag erstellen](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-02.png)

2. Klicken Sie auf **SCHNELLERFASSUNG**.

3. Klicken Sie für die Einstellung **SPEICHERKONTO FÜR REGIONALE ÜBERWACHUNG** auf die Option **NEUES SPEICHERKONTO ERSTELLEN**, und versehen Sie das Konto mit einem eindeutigen Namen. Azure Stream Analytics verwendet dieses Konto, um Überwachungsinformationen für alle Ihre zukünftigen Aufträge zu speichern.

    > [AZURE.NOTE] Sie sollten dieses Speicherkonto nur einmal pro Region erstellen. Dieser Speicher wird über alle Stream Analytics-Aufträge hinweg gemeinsam genutzt, die in dieser Region erstellt werden.

4. Klicken Sie am Ende der Seite auf **STREAM ANALYTICS-AUFTRAG ERSTELLEN** .

    ![Speicherkontokonfiguration](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03.jpg)

## <a name="azure-stream-analytics-query"></a>Azure Stream Analytics-Abfrage

Klicken Sie auf die Registerkarte **ABFRAGE**, um zum Abfrage-Editor zu wechseln. Die Registerkarte **ABFRAGE** enthält eine T-SQL-Abfrage, die die Transformation der eingehenden Ereignisdaten ausführt.

## <a name="archive-your-raw-data"></a>Archivieren von Rohdaten

Die einfachste Form einer Abfrage ist eine Passthrough-Abfrage, bei der alle Eingabedaten unter dem angegebenen Ausgabeort archiviert werden.

![Auftragsabfrage archivieren](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-04.png)

Laden Sie nun die Beispieldatendatei von [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot) an einen Speicherort auf Ihrem Computer herunter. Fügen Sie die Abfrage aus der Datei „PassThrough.txt“ ein. Klicken Sie auf die Schaltfläche **Test**, und wählen Sie dann die Datendatei „HelloWorldASA-InputStream.json“ am Downloadort aus.

![Schaltfläche „Test“ in Stream Analytics](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-05.png)

![Eingabedatenstrom testen](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06.png)

Die Ergebnisse der Abfrage werden im Browser wie in der folgenden Abbildung angezeigt.

![Testergebnisse](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-07.png)

## <a name="filter-the-data-based-on-a-condition"></a>Filtern der Daten basierend auf einer Bedingung

Wir versuchen nun, die Ergebnisse anhand einer Bedingung zu filtern. Wir möchten nur Ergebnisse für die Ereignisse anzeigen, die von „sensorA“ stammen. Die Abfrage ist in der Datei „Filtering.txt“ enthalten.

![Filtern eines Datenstroms](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-08.png)

Beachten Sie, dass mit der Abfrage, bei der die Groß-/Kleinschreibung berücksichtigt wird, ein Zeichenfolgenwert verglichen wird. Klicken Sie auf die Schaltfläche **Erneut ausführen** um die Abfrage auszuführen. Mit der Abfrage sollten 389 Zeilen für 1860 Ereignisse zurückgegeben werden.

![Zweite Ausgabeergebnisse des Abfragetests](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-09.png)

## <a name="alert-to-trigger-a-business-workflow"></a>Warnung zur Auslösung eines Geschäftsworkflows

Wir fügen unserer Abfrage nun weitere Details hinzu. Für jeden Typ von Sensor möchten wir die Durchschnittstemperatur pro 30-Sekunden-Fenster überwachen und die Ergebnisse nur anzeigen, wenn die Durchschnittstemperatur über 100 Grad liegt. Wir schreiben die folgende Abfrage und klicken dann auf **Erneut ausführen**, um die Ergebnisse anzuzeigen. Die Abfrage befindet sich in der Datei „ThresholdAlerting.txt“.

![Filterabfrage (30 Sekunden)](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-10.png)

Die Ergebnisse umfassen jetzt nur noch 245 Zeilen, und es werden die Namen der Sensoren aufgeführt, bei denen die Durchschnittstemperatur höher als 100 ist. Bei dieser Abfrage wird der Datenstrom mit den Ereignissen nach **dspl** (Sensorname) gruppiert, und es wird ein **rollierendes Fenster** von 30 Sekunden verwendet. Bei zeitlichen Abfragen muss angegeben werden, wie der Zeitablauf erfolgen soll. Mit der **TIMESTAMP BY**-Klausel haben wir die Spalte **OUTPUTTIME** angegeben, um Zeiten allen Zeitberechnungen zuzuordnen. Ausführliche Informationen finden Sie in den MSDN-Artikeln zu den Funktionen [Time Management](https://msdn.microsoft.com/library/azure/mt582045.aspx) (Zeitverwaltung) und [Windowing](https://msdn.microsoft.com/library/azure/dn835019.aspx).

![Temperatur über 100 Grad](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-11.png)

## <a name="detect-absence-of-events"></a>Abwesenheit von Ereignissen erkennen

Wie können wir eine Abfrage schreiben, die einen Mangel an Eingabeereignissen findet? Hierzu ermitteln wir, wann ein Sensor zum letzten Mal Daten gesendet und danach eine Minute lang keine weiteren Ereignisse gesendet hat. Die Abfrage ist in der Datei „AbsenseOfEvent.txt“ enthalten.

![Abwesenheit von Ereignissen erkennen](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-12.png)

Hier verwenden wir den Verknüpfungsvorgang **LEFT OUTER** für den gleichen Datenstrom (Selbstverknüpfung). Für eine innere Verknüpfung (**INNER**) wird nur dann ein Ergebnis zurückgegeben, wenn eine Übereinstimmung gefunden wird.  Wenn ein Ereignis von der linken Seite der Verknüpfung keine Übereinstimmung besitzt, wird bei **LEFT OUTER** für alle Spalten der rechten Seite eine Zeile mit NULL zurückgegeben. Dieses Verfahren ist äußerst hilfreich, um die Abwesenheit von Ereignissen zu ermitteln. Weitere Informationen zu [JOIN](https://msdn.microsoft.com/library/azure/dn835026.aspx) finden Sie in der MSDN-Dokumentation.

![JOIN-Ergebnisse](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-13.png)

## <a name="conclusion"></a>Zusammenfassung

In diesem Tutorial erfahren Sie, wie Sie verschiedene Abfragen in der Stream Analytics-Abfragesprache schreiben und die Ergebnisse im Browser anzeigen. Dies ist aber erst der Anfang. Mit Stream Analytics haben Sie noch viele weitere Möglichkeiten. Stream Analytics unterstützt verschiedenste Ein- und Ausgaben und kann sogar Funktionen in Azure Machine Learning nutzen. Dies macht Stream Analytics zu einem zuverlässigen Tool für die Datenstromanalyse. Weitere Informationen zu Stream Analytics finden Sie in unserem [Lernpfad](https://azure.microsoft.com/documentation/learning-paths/stream-analytics/). Weitere Informationen zum Schreiben von Abfragen finden Sie im Artikel zu [gängigen Abfragemustern](./stream-analytics-stream-analytics-query-patterns.md).



<!--HONumber=Oct16_HO2-->


