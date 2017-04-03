---
title: Streamen von Daten aus Stream Analytics in Data Lake Store | Microsoft Docs
description: Verwenden von Azure Stream Analytics zum Streamen von Daten in Azure Data Lake-Speicher
services: data-lake-store,stream-analytics
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: edb58e0b-311f-44b0-a499-04d7e6c07a90
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/03/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 4ecf4f8594f7a274bec231fb74c4caa22c3cc354
ms.openlocfilehash: b5f2ae124ca3276e15e0d1f75d655ec346bf8ee8
ms.lasthandoff: 01/06/2017


---
# <a name="stream-data-from-azure-storage-blob-into-data-lake-store-using-azure-stream-analytics"></a>Streamen von Daten aus Azure Storage-Blob in Data Lake-Speicher mit Azure Stream Analytics
In diesem Artikel erfahren Sie, wie Sie Azure Data Lake-Speicher als Ausgabe für einen Azure Stream Analytics-Auftrag verwenden. Dieser Artikel beschreibt ein einfaches Szenario, bei dem Daten aus einem Azure Storage-Blob (Eingabe) gelesen und in Data Lake-Speicher (Ausgabe) geschrieben werden.

> [!NOTE]
> Derzeit wird die Erstellung und Konfiguration von Data Lake Store-Ausgaben für Stream Analytics nur im [klassischen Azure-Portal](https://manage.windowsazure.com)unterstützt. Daher wird für einige Teile dieses Tutorials das klassische Azure-Portal verwendet.
>
>

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Tutorial beginnen können, benötigen Sie Folgendes:

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Storage-Konto**. Sie verwenden einen Blobcontainer aus diesem Konto, um Daten für einen Stream Analytics-Auftrag einzugeben. Nehmen Sie für dieses Tutorial an, dass Sie über ein Speicherkonto namens **storageforasa** und in diesem Konto über einen Container namens **storageforasacontainer** verfügen. Nachdem Sie den Container erstellt haben, laden Sie eine Beispieldatendatei in diesen hoch. 
  
* **Azure Data Lake-Speicherkonto**. Führen Sie die Schritte der Anleitung unter [Erste Schritte mit dem Azure Data Lake-Speicher mithilfe des Azure-Portals](data-lake-store-get-started-portal.md)aus. Nehmen wir an, dass Sie über ein Data Lake Store-Konto namens **asadatalakestore** verfügen. 

## <a name="create-a-stream-analytics-job"></a>Erstellen eines Stream Analytics-Auftrags
Erstellen Sie zunächst einen Stream Analytics-Auftrag, der eine Eingabequelle und ein Ausgabeziel enthält. In diesem Tutorial ist die Quelle ein Azure-Blobcontainer, und das Ziel ist der Data Lake-Speicher.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

2. Klicken Sie im linken Bereich auf **Stream Analytics-Aufträge**, und klicken Sie dann auf **Hinzufügen**.

    ![Erstellen eines Stream Analytics-Auftrags](./media/data-lake-store-stream-analytics/create.job.png "Erstellen eines Stream Analytics-Auftrags")

    > [!NOTE]
    > Stellen Sie sicher, dass Sie den Auftrag in der gleichen Region erstellen, in der sich das Speicherkonto befindet, da sonst zusätzliche Kosten für das Verschieben von Daten zwischen Regionen anfallen.
    >

## <a name="create-a-blob-input-for-the-job"></a>Erstellen einer Blobeingabe für den Auftrag

1. Öffnen Sie die Seite für den Stream Analytics-Auftrag, klicken Sie im linken Bereich auf die Registerkarte **Eingaben**, und klicken Sie dann auf **Hinzufügen**.

    ![Hinzufügen einer Eingabe zu Ihrem Auftrag](./media/data-lake-store-stream-analytics/create.input.1.png "Hinzufügen einer Eingabe zu Ihrem Auftrag")

2. Geben Sie auf dem Blatt **Neue Eingabe** die folgenden Werte ein.

    ![Hinzufügen einer Eingabe zu Ihrem Auftrag](./media/data-lake-store-stream-analytics/create.input.2.png "Hinzufügen einer Eingabe zu Ihrem Auftrag")

    * Geben Sie unter **Eingabealias** einen eindeutigen Namen für diese Auftragseingabe ein.
    * Wählen Sie als **Quelltyp** die Option **Datenstrom** aus.
    * Wählen Sie als **Quelle** die Option **Blobspeicher** aus.
    * Wählen Sie unter **Abonnement** die Option **Blobspeicher aus aktuellem Abonnement verwenden** aus.
    * Wählen Sie als **Speicherkonto** das Speicherkonto aus, das Sie unter „Voraussetzungen“ erstellt haben. 
    * Wählen Sie als **Container** den Container aus, den Sie im ausgewählten Speicherkonto erstellt haben.
    * Wählen Sie unter **Ereignisserialisierungsformat** die Option **CSV** aus.
    * Wählen Sie als **Trennzeichen** die Option **Tabstopp** aus.
    * Wählen Sie als **Codierung** die Option **UTF-8** aus.

    Klicken Sie auf **Erstellen**. Das Portal fügt die Eingabe hinzu und testet die Verbindung.


## <a name="create-a-data-lake-store-output-for-the-job"></a>Erstellen einer Data Lake-Speicherausgabe für den Auftrag

1. Öffnen Sie die Seite für den Stream Analytics-Auftrag, klicken Sie auf die Registerkarte **Ausgaben**, und klicken Sie dann auf **Hinzufügen**.

    ![Hinzufügen einer Ausgabe zu Ihrem Auftrag](./media/data-lake-store-stream-analytics/create.output.1.png "Hinzufügen einer Ausgabe zu Ihrem Auftrag")

2. Geben Sie auf dem Blatt **Neue Ausgabe** die folgenden Werte ein.

    ![Hinzufügen einer Ausgabe zu Ihrem Auftrag](./media/data-lake-store-stream-analytics/create.output.2.png "Hinzufügen einer Ausgabe zu Ihrem Auftrag")

    * Geben Sie unter **Ausgabealias** einen eindeutigen Namen für diese Auftragsausgabe ein. Dies ist ein Anzeigename, der in Abfragen verwendet wird, um die Abfrageausgabe an diesen Data Lake-Speicher weiterzuleiten.
    * Wählen Sie unter **Senke** die Option **Data Lake Store** aus.
    * Sie werden aufgefordert, sich für den Zugriff auf das Data Lake Store-Konto zu autorisieren. Klicken Sie auf **Autorisieren**.

3. Geben Sie auf dem Blatt **Neue Ausgabe** die folgenden Werte ein.

    ![Hinzufügen einer Ausgabe zu Ihrem Auftrag](./media/data-lake-store-stream-analytics/create.output.3.png "Hinzufügen einer Ausgabe zu Ihrem Auftrag")

    * Wählen Sie unter **Kontoname** das bereits erstellte Data Lake Store-Konto aus, an das die Auftragsausgabe gesendet werden soll.
    * Geben Sie unter **Pfadpräfixmuster** den Dateipfad ein, in den Ihre Dateien im angegebenen Data Lake Store-Konto geschrieben werden.
    * Wenn Sie ein Datumstoken im Pfadpräfix verwendet wird, können Sie das **Datumsformat** auswählen, mit dem Ihre Dateien sortiert werden.
    * Wenn Sie ein Uhrzeittoken im Pfadpräfix verwendet wird, können Sie das **Uhrzeitformat** auswählen, mit dem Ihre Dateien sortiert werden.
    * Wählen Sie unter **Ereignisserialisierungsformat** die Option **CSV** aus.
    * Wählen Sie als **Trennzeichen** die Option **Tabstopp** aus.
    * Wählen Sie als **Codierung** die Option **UTF-8** aus.
    
    Klicken Sie auf **Erstellen**. Das Portal fügt die Ausgabe hinzu und testet die Verbindung.
    
## <a name="run-the-stream-analytics-job"></a>Ausführen des Stream Analytics-Auftrags

1. Um einen Stream Analytics-Auftrag auszuführen, müssen Sie eine Abfrage auf der Registerkarte **Abfrage** ausführen. Für dieses Tutorial können Sie die Beispielabfrage ausführen, indem Sie die Platzhalter durch die Auftragseingabe- und Auftragsausgabealiase ersetzen, wie in der folgenden Bildschirmaufnahme gezeigt.

    ![Ausführen einer Abfrage](./media/data-lake-store-stream-analytics/run.query.png "Ausführen einer Abfrage")

2. Klicken Sie oben auf dem Bildschirm auf **Speichern**, und klicken Sie dann auf der Registerkarte **Übersicht** auf **Starten**. Wählen Sie im Dialogfeld die Option **Benutzerdefinierte Uhrzeit** aus, und legen Sie das aktuelle Datum und die aktuelle Uhrzeit fest.

    ![Festlegen der Auftragszeit](./media/data-lake-store-stream-analytics/run.query.2.png "Festlegen der Auftragszeit")

    Klicken Sie auf **Starten**, um den Auftrag zu starten. Es kann einige Minuten dauern, bis der Auftrag startet.

3. Um den Auftrag zum Abrufen der Daten aus dem Blob auszulösen, kopieren Sie eine Beispieldatendatei in den Blobcontainer. Eine Beispieldatendatei finden Sie im [Azure Data Lake Git-Repository](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt). Für dieses Tutorial kopieren wir die Datei **vehicle1_09142014.csv**. Sie können verschiedene Clients verwenden, z.B. [Azure-Speicher-Explorer](http://storageexplorer.com/), um Daten in einen Blobcontainer hochzuladen.

4. Auf der Registerkarte **Übersicht** können Sie unter **Überwachung** sehen, wie die Daten verarbeitet wurden.

    ![Überwachen des Auftrags](./media/data-lake-store-stream-analytics/run.query.3.png "Überwachen des Auftrags")

5. Abschließend können Sie überprüfen, ob die Ausgabedaten des Auftrags im Data Lake Store-Konto verfügbar sind. 

    ![Überprüfen der Ausgabe](./media/data-lake-store-stream-analytics/run.query.4.png "Überprüfen der Ausgabe")

    Beachten Sie, dass die Ausgabe im Daten-Explorer-Fenster gemäß der Angabe in den Data Lake Store-Ausgabeeinstellungen (`streamanalytics/job/output/{date}/{time}`) in einen Ordnerpfad geschrieben wird.  

## <a name="see-also"></a>Weitere Informationen
* [Erstellen eines HDInsight-Clusters für die Verwendung von Data Lake-Speicher](data-lake-store-hdinsight-hadoop-use-portal.md)

