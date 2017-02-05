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
ms.date: 07/07/2016
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 73d3e5577d0702a93b7f4edf3bf4e29f55a053ed
ms.openlocfilehash: 2864fbf1fc4f070cb4d88d1bb3efbaaf408c68ce


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
* **Aktivieren Sie Ihr Azure-Abonnement** für die öffentliche Vorschauversion von Data Lake Store. Weitere Informationen finden Sie in den [Anweisungen](data-lake-store-get-started-portal.md).
* **Azure Storage-Konto**. Sie verwenden einen Blobcontainer aus diesem Konto, um Daten für einen Stream Analytics-Auftrag einzugeben. Gehen Sie für dieses Tutorial davon aus, dass Sie ein Speicherkonto namens **datalakestoreasa** und einen Container innerhalb des Kontos namens **datalakestoreasacontainer** erstellen. Nachdem Sie den Container erstellt haben, laden Sie eine Beispieldatendatei in diesen hoch. Eine Beispieldatendatei finden Sie im [Azure Data Lake Git-Repository](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt). Sie können verschiedene Clients verwenden, z.B. [Azure-Speicher-Explorer](http://storageexplorer.com/), um Daten in einen Blobcontainer hochzuladen.

  > [!NOTE]
  > Wenn Sie das Konto aus dem Azure-Portal heraus erstellen, stellen Sie sicher, dass Sie es mit dem **klassischen** Bereitstellungsmodell erstellen. Dadurch wird sichergestellt, dass aus dem klassischen Azure-Portal heraus auf das Speicherkonto zugegriffen werden kann, denn dies verwenden wir zum Erstellen eines Stream Analytics-Auftrags. Eine Anleitung zum Erstellen eines Speicherkontos über das Azure-Portal mithilfe der klassischen Bereitstellung finden Sie unter [Erstellen von Azure-Speicherkonten](../storage/storage-create-storage-account.md#create-a-storage-account).
  >
  > Sie können ein Speicherkonto auch über das klassische Azure-Portal erstellen.
  >
  >
* **Azure Data Lake-Speicherkonto**. Führen Sie die Schritte der Anleitung unter [Erste Schritte mit dem Azure Data Lake-Speicher mithilfe des Azure-Portals](data-lake-store-get-started-portal.md)aus.  

## <a name="create-a-stream-analytics-job"></a>Erstellen eines Stream Analytics-Auftrags
Erstellen Sie zunächst einen Stream Analytics-Auftrag, der eine Eingabequelle und ein Ausgabeziel enthält. In diesem Tutorial ist die Quelle ein Azure-Blobcontainer, und das Ziel ist der Data Lake-Speicher.

1. Melden Sie sich am [klassischen Azure-Portal](https://manage.windowsazure.com)an.
2. Klicken Sie unten links im Bildschirm auf **Neu** > **Data Services** > **Stream Analytics** > **Schnellerfassung**. Geben Sie die folgenden Werte an, und klicken Sie dann auf **Stream Analytics-Auftrag erstellen**.

    ![Erstellen eines Stream Analytics-Auftrags](./media/data-lake-store-stream-analytics/create.job.png "Create a Stream Analytics job")

## <a name="create-a-blob-input-for-the-job"></a>Erstellen einer Blobeingabe für den Auftrag
1. Öffnen Sie die Seite für den Stream Analytics-Auftrag, klicken Sie auf die Registerkarte **Eingaben**, und klicken Sie dann auf **Eingabe hinzufügen**, um einen Assistenten zu starten.
2. Wählen Sie auf der Seite **Ihrem Auftrag eine Eingabe hinzufügen** **Datenstrom** aus, und klicken Sie dann auf den Vorwärtspfeil.

    ![Hinzufügen einer Eingabe zu Ihrem Auftrag](./media/data-lake-store-stream-analytics/create.input.1.png "Add an input to your job")
3. Wählen Sie auf der Seite **Ihrem Auftrag einen Datenstrom** hinzufügen **Blobspeicher** aus, und klicken Sie dann auf den Vorwärtspfeil.

    ![Hinzufügen eines Datenstroms zu Ihrem Auftrag](./media/data-lake-store-stream-analytics/create.input.2.png "Add a data stream to the job")
4. Geben Sie auf der Seite **Einstellungen des Blobspeichers** Details für den Blobspeicher ein, den Sie als Eingabedatenquelle verwenden.

    ![Angeben der Blobspeichereinstellungen](./media/data-lake-store-stream-analytics/create.input.3.png "Provide the blob storage settings")

   * **Geben Sie einen Eingabealias ein**. Dies ist ein eindeutiger Namen, den Sie für die Auftragseingabe angeben.
   * **Wählen Sie ein Speicherkonto aus**. Stellen Sie sicher, dass sich das Speicherkonto in der gleichen Region wie der Stream Analytics-Auftrag befindet, anderenfalls fallen zusätzliche Kosten für das Verschieben von Daten zwischen Regionen an.
   * **Geben Sie einen Speichercontainer an**. Sie können auch einen neuen Container erstellen oder einen vorhandenen Container auswählen.

     Klicken Sie auf den Vorwärtspfeil.
5. Legen Sie auf der Seite **Serialisierungseinstellungen** **CSV** als Serialisierungsformat, **Tabulator** als Trennzeichen und **UTF8** als Codierung fest, und klicken Sie dann auf das Häkchen.

    ![Angeben der Serialisierungseinstellungen](./media/data-lake-store-stream-analytics/create.input.4.png "Provide the serialization settings")
6. Nachdem Sie den Assistenten abgeschlossen haben, wird die Blobeingabe unter der Registerkarte **Eingaben** hinzugefügt. Daraufhin sollte in der Spalte **Diagnose** **OK** angezeigt werden. Sie können die Verbindung auch mithilfe der Schaltfläche **Verbindung testen** unten explizit auf die Eingabe hin testen.

## <a name="create-a-data-lake-store-output-for-the-job"></a>Erstellen einer Data Lake-Speicherausgabe für den Auftrag
1. Öffnen Sie die Seite für den Stream Analytics-Auftrag, klicken Sie auf die Registerkarte **Ausgaben**, und klicken Sie dann auf **Ausgabe hinzufügen**, um einen Assistenten zu starten.
2. Wählen Sie auf der Seite **Ihrem Auftrag eine Ausgabe hinzufügen** **Data Lake Store** aus, und klicken Sie dann auf den Vorwärtspfeil.

    ![Hinzufügen einer Ausgabe zu Ihrem Auftrag](./media/data-lake-store-stream-analytics/create.output.1.png "Add an output to your job")
3. Klicken Sie auf der Seite **Verbindung autorisieren** auf **Jetzt autorisieren**, wenn Sie bereits ein Data Lake Store-Konto erstellt haben. Klicken Sie andernfalls auf **Jetzt anmelden** , um ein neues Konto zu erstellen. Für dieses Tutorial gehen wir davon aus, dass Sie bereits ein Data Lake-Speicherkonto erstellt haben (wie in den Voraussetzungen angegeben). Sie werden automatisch mit den Anmeldeinformationen autorisiert, mit denen Sie sich am klassischen Azure-Portal angemeldet haben.

    ![Autorisieren von Data Lake-Speicher](./media/data-lake-store-stream-analytics/create.output.2.png "Authorize Data Lake Store")
4. Geben Sie auf der Seite **Data Lake-Speichereinstellungen** die Informationen ein, wie in der folgenden Bildschirmaufnahme gezeigt.

    ![Festlegen der Data Lake-Speichereinstellungen](./media/data-lake-store-stream-analytics/create.output.3.png "Specify Data Lake Store settings")

   * **Geben Sie einen Ausgabealias ein**. Dies ist ein eindeutiger Namen, den Sie für die Auftragsausgabe angeben.
   * **Geben Sie ein Data Lake-Speicherkonto an**. Sie sollten es bereits erstellt haben, wie in den Voraussetzungen angegeben.
   * **Geben Sie ein Präfixmuster des Pfads an**. Dies ist erforderlich, um die Ausgabedateien zu identifizieren, die vom Stream Analytics-Auftrag in den Data Lake-Speicher geschrieben werden. Da die Titel der Ausgaben, die vom Auftrag geschrieben werden, ein GUID-Format haben, hilft die Angabe eines Präfixes bei der Identifizierung der geschriebenen Ausgabe. Wenn Sie ein Datum und einen Zeitstempel als Teil des Präfixes einschließen möchten, stellen Sie sicher, dass Sie `{date}/{time}` im Präfixmuster angeben. Wenn Sie dies einschließen, werden die Felder **Datumsformat** und **Zeitformat** aktiviert, sodass Sie das bevorzugte Format auswählen können.

     Klicken Sie auf den Vorwärtspfeil.
5. Legen Sie auf der Seite **Serialisierungseinstellungen** **CSV** als Serialisierungsformat, **Tabulator** als Trennzeichen und **UTF8** als Codierung fest, und klicken Sie dann auf das Häkchen.

    ![Festlegen des Ausgabeformats](./media/data-lake-store-stream-analytics/create.output.4.png "Specify the output format")
6. Nachdem Sie den Assistenten abgeschlossen haben, wird die Data Lake Store-Ausgabe unter der Registerkarte **Ausgaben** hinzugefügt. Daraufhin sollte in der Spalte **Diagnose** **OK** angezeigt werden. Sie können die Verbindung auch mithilfe der Schaltfläche **Verbindung testen** unten explizit auf die Ausgabe hin testen.

## <a name="run-the-stream-analytics-job"></a>Ausführen des Stream Analytics-Auftrags
Um einen Stream Analytics-Auftrag auszuführen, müssen Sie eine Abfrage auf der Registerkarte „Abfrage“ ausführen. Für dieses Tutorial können Sie die Beispielabfrage ausführen, indem Sie die Platzhalter durch die Auftragseingabe- und Auftragsausgabealiase ersetzen, wie in der folgenden Bildschirmaufnahme gezeigt.

![Abfrage ausführen](./media/data-lake-store-stream-analytics/run.query.png "Run query")

Klicken Sie am unteren Bildschirmrand auf **Speichern** und dann auf **Starten**. Wählen Sie im Dialogfeld **Benutzerdefinierte Uhrzeit** und dann ein Datum aus der Vergangenheit aus, z.B. **1/1/2016**. Klicken Sie auf das Häkchen, um den Auftrag zu starten. Es kann einige Minuten dauern, bis der Auftrag startet.

![Festlegen der Auftragszeit](./media/data-lake-store-stream-analytics/run.query.2.png "Set job time")

Klicken Sie, nachdem der Auftrag gestartet wurde, auf die Registerkarte **Überwachen** , um anzuzeigen, wie die Daten verarbeitet wurden.

![Überwachen des Auftrags](./media/data-lake-store-stream-analytics/run.query.3.png "Monitor job")

Abschließend können Sie das Data Lake Store-Konto mit dem [Azure-Portal](https://portal.azure.com) öffnen und überprüfen, ob die Daten erfolgreich in das Konto geschrieben wurden.

![Überprüfen der Ausgabe](./media/data-lake-store-stream-analytics/run.query.4.png "Verify output")

Beachten Sie, dass die Ausgabe im Daten-Explorer-Fenster gemäß Angabe in den Data Lake Store-Ausgabeeinstellungen (`streamanalytics/job/output/{date}/{time}`) in einen Ordner geschrieben wird.  

## <a name="see-also"></a>Siehe auch
* [Erstellen eines HDInsight-Clusters für die Verwendung von Data Lake-Speicher](data-lake-store-hdinsight-hadoop-use-portal.md)



<!--HONumber=Nov16_HO3-->


