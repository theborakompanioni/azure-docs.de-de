---
title: Visualisieren von Sensordaten in Azure IoT Hub in Echtzeit mit Power BI | Microsoft-Dokumentation
description: Verwenden Sie Power BI, um Temperatur- und Luftfeuchtigkeitsdaten visuell darzustellen, die der Sensor gesammelt und an Ihren Azure IoT Hub gesendet hat.
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: Datenvisualisierung in Echtzeit, Visualisierung von Livedaten, Visualisierung von Sensordaten
ms.assetid: e67c9c09-6219-4f0f-ad42-58edaaa74f61
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/29/2017
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 6a99749a96a6239428e5b018a26a6e8fd440c9d2
ms.lasthandoff: 04/12/2017


---
# <a name="visualize-real-time-sensor-data-from-azure-iot-hub-using-power-bi"></a>Verwenden von Power BI zum Visualisieren von Sensordaten in Azure IoT Hub in Echtzeit

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Lerninhalt

Sie erfahren, wie Sie mit Power BI von Azure IoT Hub empfangene Sensordaten in Echtzeit visualisieren. Wenn Sie versuchen möchten, die Daten in Ihrem IoT Hub mit Web-Apps visuell darzustellen, siehe [Visualisieren von Sensordaten in Azure IoT Hub in Echtzeit mit Azure-Web-Apps](iot-hub-live-data-visualization-in-web-apps.md).

## <a name="what-you-do"></a>Aufgaben

- Bereiten Sie Ihren IoT Hub für den Datenzugriff vor, indem Sie eine Consumergruppe hinzufügen.
- Erstellen und konfigurieren Sie einen Stream Analytics-Auftrag zum Übertragen von Daten von Ihrem IoT Hub zu Ihrem Power BI-Konto, und führen Sie ihn aus.
- Erstellen und veröffentlichen Sie einen Power BI-Bericht zum Visualisieren der Daten.

## <a name="what-you-need"></a>Voraussetzungen

- Sie müssen das Tutorial [Verbinden von ESP8266 mit Azure IoT Hub](iot-hub-arduino-huzzah-esp8266-get-started.md) abgeschlossen haben, in dem die folgenden Voraussetzungen behandelt werden:
  - Ein aktives Azure-Abonnement.
  - Ein Azure IoT Hub in Ihrem Abonnement.
  - Eine Clientanwendung, die Nachrichten an Ihren Azure IoT Hub sendet.
- Ein Power BI-Konto. ([Power BI kostenlos testen](https://powerbi.microsoft.com/))

## <a name="add-a-consumer-group-to-your-iot-hub"></a>Hinzufügen einer Consumergruppe zu Ihrem IoT Hub

Consumergruppen werden von Anwendungen verwendet, um Daten aus Azure IoT Hub abzurufen. In dieser Lektion erstellen Sie eine Consumergruppe, die von einem Stream Analytics-Auftrag zum Lesen von Daten in Ihrem IoT Hub verwendet wird.

Gehen Sie folgendermaßen vor, um Ihrem IoT Hub eine Consumergruppe hinzuzufügen:

1. Öffnen Sie im [Azure-Portal](https://ms.portal.azure.com/) Ihren IoT-Hub.
1. Klicken Sie im linken Bereich auf **Endpunkte**. Wählen Sie im mittleren Bereich **Ereignisse** aus. Geben Sie im rechten Bereich unter **Consumergruppen** einen Namen ein, und klicken Sie dann auf **Speichern**.

   ![Erstellen einer Consumergruppe in Azure IoT Hub](media/iot-hub-live-data-visualization-in-power-bi/1_iot-hub-create-consumer-group-azure.png)

## <a name="create-configure-and-run-a-stream-analytics-job"></a>Erstellen, Konfigurieren und Ausführen eines Stream Analytics-Auftrags

### <a name="create-a-stream-analytics-job"></a>Erstellen eines Stream Analytics-Auftrags

1. Klicken Sie im Azure-Portal auf „Neu > Internet der Dinge (IoT) > Stream Analytics-Auftrag“.
1. Geben Sie die folgenden Informationen für den Auftrag ein.

   **Auftragsname**: Der Name des Auftrags. Der Name muss global eindeutig sein.

   **Ressourcengruppe**: Verwenden Sie dieselbe Ressourcengruppe wie für Ihren IoT Hub.

   **Speicherort**: Verwenden Sie denselben Speicherort wie für Ihre Ressourcengruppe.

   **An Dashboard anheften**: Aktivieren Sie diese Option für den leichteren Zugriff auf Ihren IoT Hub über das Dashboard.

   ![Erstellen eines Stream Analytics-Auftrags in Azure](media/iot-hub-live-data-visualization-in-power-bi/2_create-stream-analytics-job-azure.png)

1. Klicken Sie auf **Erstellen**.

### <a name="add-an-input-to-the-stream-analytics-job"></a>Hinzufügen einer Eingabe zum Stream Analytics-Auftrag

1. Öffnen Sie den Stream Analytics-Auftrag.
1. Klicken Sie unter **Auftragstopologie** auf **Eingaben**.
1. Klicken Sie im Bereich **Eingaben** auf **Hinzufügen**, und geben Sie die folgenden Informationen ein:

   **Eingabealias**: Der eindeutige Alias für die Eingabe.

   **Quelle**: Wählen Sie **IoT Hub** aus.

   **Consumergruppe**: Wählen Sie die Consumergruppe aus, die Sie zuvor erstellt haben.
1. Klicken Sie auf **Erstellen**.

   ![Hinzufügen einer Eingabe zum Stream Analytics-Auftrag in Azure](media/iot-hub-live-data-visualization-in-power-bi/3_add-input-to-stream-analytics-job-azure.png)

### <a name="add-an-output-to-the-stream-analytics-job"></a>Hinzufügen einer Ausgabe zum Stream Analytics-Auftrag

1. Klicken Sie unter **Auftragstopologie** auf **Ausgaben**.
1. Klicken Sie im Bereich **Ausgaben** auf **Hinzufügen**, und geben Sie die folgenden Informationen ein:

   **Ausgabealias**: Der eindeutige Alias für die Ausgabe.

   **Senke**: Wählen Sie **Power BI** aus.
1. Klicken Sie auf **Autorisieren**, und melden Sie sich dann bei Ihrem Power BI-Konto an.
1. Geben Sie nach der Autorisierung Folgendes ein:

   **Gruppenarbeitsbereich**: Wählen Sie den Arbeitsbereich der Zielgruppe aus.

   **Datasetname**: Geben Sie einen Datasetnamen ein.

   **Tabellenname**: Geben Sie einen Tabellennamen ein.
1. Klicken Sie auf **Erstellen**.

   ![Hinzufügen einer Ausgabe zum Stream Analytics-Auftrag in Azure](media/iot-hub-live-data-visualization-in-power-bi/4_add-output-to-stream-analytics-job-azure.png)

### <a name="configure-the-query-of-the-stream-analytics-job"></a>Konfigurieren der Abfrage des Stream Analytics-Auftrags

1. Klicken Sie unter **Auftragstopologie** auf **Abfrage**.
1. Ersetzen Sie `[YourInputAlias]` durch den Eingabealias des Auftrags.
1. Ersetzen Sie `[YourOutputAlias]` durch den Ausgabealias des Auftrags.
1. Klicken Sie auf **Speichern**.

   ![Hinzufügen einer Abfrage zum Stream Analytics-Auftrag in Azure](media/iot-hub-live-data-visualization-in-power-bi/5_add-query-stream-analytics-job-azure.png)

### <a name="run-the-stream-analytics-job"></a>Ausführen des Stream Analytics-Auftrags

Klicken Sie im Stream Analytics-Auftrag auf **Starten** > **Jetzt** > **Starten**. Sobald der Auftrag erfolgreich gestartet wurde, ändert sich der Status des Auftrags von **Beendet** in **Wird ausgeführt**.

![Ausführen eines Stream Analytics-Auftrags in Azure](media/iot-hub-live-data-visualization-in-power-bi/6_run-stream-analytics-job-azure.png)

## <a name="create-and-publish-a-power-bi-report-to-visualize-the-data"></a>Erstellen und Veröffentlichen eines Power BI-Berichts zum Visualisieren der Daten

1. Stellen Sie sicher, dass die Beispielanwendung ausgeführt wird. Falls nicht, führen Sie den folgenden Befehl aus, um die Anwendung auf dem Pi-Gerät auszuführen:

   ```bash
   gulp run
   ```
1. Melden Sie sich bei Ihrem [Power BI](https://powerbi.microsoft.com/en-us/)-Konto an.
1. Wechseln Sie zum Gruppenarbeitsbereich, den Sie festgelegt haben, als Sie die Ausgabe für den Stream Analytics-Auftrag erstellt haben.
1. Klicken Sie auf **Streamingdatasets**.

   Es sollte das Dataset aufgelistet sein, das Sie beim Erstellen der Ausgabe für den Stream Analytics-Auftrag angegeben haben.
1. Klicken Sie unter **AKTIONEN** auf das erste Symbol, um einen Bericht zu erstellen.

   ![Erstellen eines Microsoft Power BI-Berichts](media/iot-hub-live-data-visualization-in-power-bi/7_create-power-bi-report-microsoft.png)

1. Erstellen Sie ein Liniendiagramm, um die Temperatur in Echtzeit im Zeitverlauf anzuzeigen.
   1. Fügen Sie auf der Seite zur Berichterstellung ein Liniendiagramm hinzu.
   1. Erweitern Sie im Bereich **Felder** die Tabelle, die Sie beim Erstellen der Ausgabe für den Stream Analytics-Auftrag angegeben haben.
   1. Ziehen Sie **EventEnqueuedUtcTime** im Bereich **Visualisierungen** auf **Achse**.
   1. Ziehen Sie **Temperatur** auf **Werte**.

      Jetzt wird ein Liniendiagramm erstellt. Die x-Achse des Diagramms zeigt Datum und Uhrzeit in der Zeitzone UTC. Die y-Achse zeigt vom Sensor empfangene Temperatur an.

      ![Hinzufügen eines Liniendiagramms für Temperatur zu einem Microsoft Power BI-Bericht](media/iot-hub-live-data-visualization-in-power-bi/8_add-line-chart-for-temperature-to-power-bi-report-microsoft.png)

1. Erstellen Sie ein weiteres Liniendiagramm, um die Luftfeuchtigkeit in Echtzeit im Zeitverlauf anzuzeigen. Gehen Sie dazu wie zuvor beschrieben vor, und platzieren Sie **EventEnqueuedUtcTime** auf der y-Achse und **humidity** auf der y-Achse.

   ![Hinzufügen eines Liniendiagramms für Luftfeuchtigkeit zu einem Microsoft Power BI-Bericht](media/iot-hub-live-data-visualization-in-power-bi/9_add-line-chart-for-humidity-to-power-bi-report-microsoft.png)

1. Klicken Sie auf **Speichern**, um den Bericht zu speichern.
1. Klicken Sie auf **Datei** > **Im Web veröffentlichen**.
1. Klicken Sie auf **Einbindungscode erstellen** und dann auf **Veröffentlichen**.

Sie erhalten einen Berichtslink, den Sie für den Zugriff auf den Bericht freigeben können, und einen Codeausschnitt, um den Bericht in Ihren Blog oder Ihre Website zu integrieren.

![Veröffentlichen eines Microsoft Power BI-Berichts](media/iot-hub-live-data-visualization-in-power-bi/10_publish-power-bi-report-microsoft.png)

Microsoft bietet auch die [mobilen Power BI-Apps](https://powerbi.microsoft.com/en-us/documentation/powerbi-power-bi-apps-for-mobile-devices/) für die Anzeige von und Interaktion mit Ihren Power BI-Dashboards und -Berichten auf Ihrem mobilen Gerät.

## <a name="next-steps"></a>Nächste Schritte

Sie haben Power BI erfolgreich eingesetzt, um Sensordaten in Ihrem Azure IoT Hub in Echtzeit zu visualisieren.
Es gibt eine alternative Möglichkeit zum Anzeigen von Daten in Ihrem Azure IoT Hub. Siehe [Visualisieren von Sensordaten in Azure IoT Hub in Echtzeit mit Azure-Web-Apps](iot-hub-live-data-visualization-in-web-apps.md).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
