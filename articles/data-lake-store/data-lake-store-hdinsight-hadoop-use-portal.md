---
title: Verwenden eines Webbrowsers zum Erstellen von Azure HDInsight-Clustern und Data Lake Store | Microsoft-Dokumentation
description: Verwenden des Azure-Portals zum Erstellen und Verwenden von HDInsight-Clustern mit Azure Data Lake-Speicher
services: data-lake-store,hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: a8c45a83-a8e3-4227-8b02-1bc1e1de6767
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/16/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 8876a37b78fa8a80eba7af133d661c3d7ed425d7
ms.openlocfilehash: 76e098525951d122799f11bdcd9ee5451c9a3777
ms.lasthandoff: 03/01/2017


---
# <a name="create-an-hdinsight-cluster-with-data-lake-store-using-azure-portal"></a>Erstellen eines HDInsight-Clusters mit Data Lake-Speicher mithilfe des Azure-Portals
> [!div class="op_single_selector"]
> * [Verwenden des Portals](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [Verwenden von PowerShell (für Standardspeicher)](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [Verwenden von PowerShell (für zusätzlichen Speicher)](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Verwenden von Resource Manager](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

Erfahren Sie, wie Sie das Azure-Portal zum Erstellen eines HDInsight-Clusters mit Zugriff auf Azure Data Lake Store verwenden. Für unterstützte Clustertypen kann Data Lake Store als Standardspeicherkonto oder als zusätzliches Speicherkonto verwendet werden. Wenn Data Lake Store als zusätzlicher Speicher verwendet wird, ist das Standardspeicherkonto für den Cluster weiterhin Azure Storage Blob (WASB), und clusterbezogene Dateien (z.B. Protokolle usw.) werden weiterhin in den Standardspeicher geschrieben, während die Daten, die Sie verarbeiten möchten, in einem Data Lake Store-Konto gespeichert werden können. Das Verwenden von Data Lake-Speicher als zusätzliches Speicherkonto wirkt sich nicht auf Leistung oder die Fähigkeit aus, Daten aus dem Cluster in den Speicher zu lesen bzw. zu schreiben.

## <a name="using-data-lake-store-for-hdinsight-cluster-storage"></a>Verwenden von Data Lake Store für die Speicherung von HDInsight-Clustern

Hier finden Sie einige wichtige Überlegungen zur Verwendung von HDInsight mit Data Lake Store:

* Eine Option zum Erstellen von HDInsight-Clustern mit Zugriff auf Data Lake Store als Standardspeicher ist für die HDInsight-Version 3.5 verfügbar.

* Eine Option zum Erstellen von HDInsight-Clustern mit Zugriff auf Data Lake Store als zusätzlichen Speicher ist für die HDInsight-Versionen 3.2, 3.4 und 3.5 verfügbar.

* Für HBase-Cluster (Windows und Linux) wird Data Lake Store **nicht** als Speicheroption unterstützt, weder für Standardspeicher noch für zusätzlichen Speicher.

* Bei Storm-Clustern (Windows und Linux) kann Data Lake Store zum Schreiben von Daten aus einer Storm-Topologie verwendet werden. Der Data Lake-Speicher kann auch zum Speichern von Verweisdaten verwendet werden, die anschließend von einer Storm-Topologie gelesen werden. Weitere Informationen finden Sie unter [Verwenden des Data Lake-Speichers in einer Storm-Topologie](#use-data-lake-store-in-a-storm-topology).


## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Tutorial beginnen können, benötigen Sie Folgendes:

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Data Lake-Speicherkonto**. Führen Sie die Schritte der Anleitung unter [Erste Schritte mit dem Azure Data Lake-Speicher mithilfe des Azure-Portals](data-lake-store-get-started-portal.md)aus.

* **Azure Active Directory-Dienstprinzipal**. Die Schritte in diesem Tutorial enthalten Anweisungen zum Erstellen eines Dienstprinzipals in Azure AD. Sie müssen jedoch Azure AD-Administrator sein, um einen Dienstprinzipal erstellen zu können. Wenn Sie Azure AD-Administrator sind, können Sie diese Voraussetzung ignorieren und mit dem Tutorial fortfahren.

    **Wenn Sie kein Azure AD-Administrator sind**, können Sie die erforderlichen Schritte zum Erstellen eines Dienstprinzipals nicht ausführen. In diesem Fall muss Ihr Azure AD-Administrator zunächst einen Dienstprinzipal erstellen, bevor Sie einen HDInsight-Cluster mit Data Lake Store erstellen können. Zudem muss der Dienstprinzipal mit einem Zertifikat erstellt werden, wie unter [Erstellen eines Dienstprinzipals mit Zertifikat](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate) beschrieben.

## <a name="create-an-hdinsight-cluster-with-access-to-azure-data-lake-store"></a>Erstellen eines HDInsight-Clusters mit Zugriff auf den Azure Data Lake-Speicher
In diesem Abschnitt erstellen Sie einen HDInsight Hadoop-Cluster, für den der Data Lake-Speicher als zusätzlicher Speicher verwendet wird. In dieser Version kann der Data Lake-Speicher für einen Hadoop-Cluster nur als zusätzlicher Speicher für den Cluster verwendet werden. Als Standardspeicher werden weiterhin Azure Storage-BLOBS (WASB) verwendet. Wir erstellen daher zuerst das Speicherkonto und die Speichercontainer, die für den Cluster erforderlich sind.

1. Melden Sie sich beim neuen [Azure-Portal](https://portal.azure.com)an.

2. Führen Sie die Schritte unter [Erstellen von Hadoop-Clustern in HDInsight](../hdinsight/hdinsight-provision-clusters.md) aus, um mit der Bereitstellung eines HDInsight-Clusters zu beginnen.

3. Geben Sie auf dem Blatt **Speicher** an, ob Sie Azure Storage (WASB) oder Data Lake Store als Standardspeicher verwenden möchten. Wenn Sie Azure Data Lake Store als Standardspeicher verwenden möchten, fahren Sie mit dem nächsten Schritt fort.

    Wenn Sie Azure Storage-Blobs als Standardspeicher verwenden möchten, klicken Sie für **Primärer Speichertyp** auf **Azure Storage**. Anschließend können Sie für **Auswahlmethode** die Option **Meine Abonnements** auswählen, wenn Sie ein Speicherkonto angeben möchten, das Bestandteil Ihres Azure-Abonnements ist. Wählen Sie dann das Speicherkonto aus. Klicken Sie andernfalls auf **Zugriffsschlüssel**, und geben Sie die Informationen für das nicht in Ihrem Azure-Abonnement enthaltene Speicherkonto an, das Sie auswählen möchten. Als **Standardcontainer** können Sie den vom Portal vorgeschlagenen Standardcontainernamen verwenden oder einen eigenen angeben. 

    Bei Verwendung von Azure Storage-Blobs als Standardspeicher können Sie trotzdem Azure Data Lake Store als zusätzlichen Speicher für den Cluster verwenden. Klicken Sie dazu auf **Data Lake Store-Zugriff**, und fahren Sie mit Schritt 5 fort.

    ![Dienstprinzipal für HDInsight-Cluster hinzufügen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.1.png "Dienstprinzipal für HDInsight-Cluster hinzufügen")


4. Wenn Sie Azure Data Lake Store als Standardspeicher verwenden möchten, klicken Sie für **Primärer Speichertyp** auf **Data Lake Store**. Wählen Sie ein bereits vorhandenes Data Lake Store-Konto aus, geben Sie einen Stammordnerpfad an, in dem die clusterspezifischen-Dateien gespeichert werden, geben Sie den **Standort** als **USA, Osten 2** an, und klicken Sie dann auf **Data Lake Store-Zugriff**. Sie können diese Option nur mit HDInsight 3.5-Clustern (Standard-Edition) verwenden. In HDInsight 3.5-Clustern ist diese Option für den HBase-Clustertyp nicht verfügbar.

    In der folgenden Bildschirmaufnahme ist der Stammordnerpfad „/clusters/myhdiadlcluster“, wobei **myhdiadlcluster** der Name des Clusters ist, der erstellt wird. Stellen Sie in einem solchen Fall sicher, dass der Ordner **/clusters** im Data Lake Store-Konto bereits vorhanden ist. Der Ordner **myhdiadlcluster** wird während der Erstellung des Clusters erstellt. Wenn der Stammpfad auf „/hdinsight/clusters/data/myhdiadlcluter“ festgelegt wurde, müssen Sie sicherstellen, dass **/hdinsight/clusters/data/** im Data Lake Store-Konto bereits vorhanden ist.

    ![Dienstprinzipal für HDInsight-Cluster hinzufügen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.1.adls.storage.png "Dienstprinzipal für HDInsight-Cluster hinzufügen")

5. Auf dem Blatt **Data Lake Store-Zugriff** können Sie einen vorhandenen Dienstprinzipal auswählen oder einen neuen erstellen. Wenn Sie einen vorhandenen Dienstprinzipal verwenden möchten, fahren Sie mit dem nächsten Schritt fort.

    Wenn Sie einen neuen Dienstprinzipal erstellen möchten, klicken Sie auf dem Blatt **Data Lake Store-Zugriff** auf **Neu erstellen** und anschließend auf **Dienstprinzipal**. Geben Sie dann auf dem Blatt **Dienstprinzipal erstellen** Werte zum Erstellen eines neuen Dienstprinzipals an. Hierbei werden auch ein Zertifikat und eine Azure Active Directory-Anwendung erstellt. Klicken Sie auf **Create**.

    ![Dienstprinzipal für HDInsight-Cluster hinzufügen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.2.png "Dienstprinzipal für HDInsight-Cluster hinzufügen")

    Sie können auch auf **Zertifikat herunterladen** klicken, um das Zertifikat herunterzuladen, das dem erstellten Dienstprinzipal zugeordnet ist. Dies ist hilfreich, wenn Sie denselben Dienstprinzipal später beim Erstellen weiterer HDInsight-Cluster verwenden möchten. Klicken Sie auf **Auswählen**.

6. Wenn Sie einen vorhandenen Dienstprinzipal verwenden möchten, klicken Sie auf dem Blatt **Data Lake Store-Zugriff** auf **Vorhandene verwenden** und dann auf **Dienstprinzipal**. Suchen Sie anschließend auf dem Blatt **AD-Dienstprinzipal auswählen** nach einem vorhandenen Dienstprinzipal. Klicken Sie auf den Namen eines Dienstprinzipals und dann auf **Auswählen**.

    ![Dienstprinzipal für HDInsight-Cluster hinzufügen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.5.png "Dienstprinzipal für HDInsight-Cluster hinzufügen")

    Laden Sie auf dem Blatt **Data Lake Store-Zugriff** das Zertifikat (PFX) hoch, das dem ausgewählten Dienstprinzipal zugeordnet ist, und geben Sie dann das Zertifikatkennwort an.

6. Klicken Sie auf dem Blatt **Data Lake Store-Zugriff** auf **Zugriff**. Im nächsten Bereich ist **Dateiberechtigungen auswählen** standardmäßig ausgewählt und listet alle Data Lake Store-Konten in Ihrem Abonnement auf. Klicken Sie auf das Data Lake Store-Konto, das Sie dem Cluster zuordnen möchten, um die Dateien und Ordner in diesem Konto aufzulisten. Sie können dann Berechtigungen auf Datei- oder Ordnerebene zuweisen. Wenn Sie die Berechtigungen auf der Stammebene des Kontos zuordnen möchten, aktivieren Sie das Kontrollkästchen neben dem Kontonamen.

    ![Dienstprinzipal für HDInsight-Cluster hinzufügen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.3.png "Dienstprinzipal für HDInsight-Cluster hinzufügen")

    > [!NOTE]
    > Wenn Sie das Data Lake Store-Konto als Standardspeicher für einen Cluster verwenden, **müssen** Sie dem Dienstprinzipal die Berechtigungen auf Stammebene des Data Lake Store-Kontos zuweisen.

7. Wenn Sie Berechtigungen für Dateien oder Ordnern innerhalb eines Kontos zuweisen möchten, wählen Sie das Data Lake Store-Konto aus, um die Dateien bzw. Ordner im nächsten Bereich anzuzeigen. Wählen Sie die Dateien bzw. Ordner aus, wählen Sie die Berechtigungen (LESEN/SCHREIBEN/AUSFÜHREN) aus, die Sie zuweisen möchten, geben Sie an, ob die Berechtigungen auch rekursiv für die untergeordneten Elemente gelten sollen, und klicken Sie dann auf **Auswählen**.

    ![Dienstprinzipal für HDInsight-Cluster hinzufügen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.3-1.png "Dienstprinzipal für HDInsight-Cluster hinzufügen")

8. Klicken Sie auf dem nächsten Bildschirm auf **Ausführen**, um die Berechtigungen für den Azure Active Directory-Dienstprinzipal dem Konto und den Dateien und Ordnern zuzuweisen, die Sie ausgewählt haben.

    ![Dienstprinzipal für HDInsight-Cluster hinzufügen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.3-2.png "Dienstprinzipal für HDInsight-Cluster hinzufügen")

9. Nachdem die Berechtigungen erfolgreich zugewiesen wurden, klicken Sie auf allen Blättern auf **Fertig**, bis wieder das Blatt **Data Lake Store-Zugriff** angezeigt wird.

4. Klicken Sie auf dem Blatt **Data Lake Store-Zugriff** auf **Auswählen**, und fahren Sie mit der Clustererstellung wie unter [Erstellen von Hadoop-Clustern in HDInsight](../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md) beschrieben fort.

10. Nach der Bereitstellung des Clusters können Sie überprüfen, ob der zugeordnete Speicher für den Cluster das von Ihnen angegebene Data Lake Store-Konto ist. Klicken Sie dazu auf dem Blatt „Cluster“ auf die Registerkarte **Speicherkonten**. 

    ![Dienstprinzipal für HDInsight-Cluster hinzufügen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.6-1.png "Dienstprinzipal für HDInsight-Cluster hinzufügen")

    Sie können auch überprüfen, ob der Dienstprinzipal dem HDInsight-Cluster zugeordnet wurde. Klicken Sie auf dem Clusterblatt auf **Data Lake Store-Zugriff**, um den zugeordneten Dienstprinzipal anzuzeigen.

    ![Dienstprinzipal für HDInsight-Cluster hinzufügen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.6.png "Dienstprinzipal für HDInsight-Cluster hinzufügen")

## <a name="show-me-some-examples"></a>Beispiele

Nachdem Sie den Cluster mit Data Lake Store als Speicher bereitgestellt haben, finden Sie im Folgenden einige Beispiele zur Verwendung von HDInsight-Clustern zum Analysieren der in Data Lake Store gespeicherten Daten.

### <a name="run-a-hive-query-against-data-stored-in-data-lake-store-as-primary-storage"></a>Ausführen einer Hive-Abfrage für in Data Lake Store gespeicherte Daten (primärer Speicher)

Zum Ausführen einer Hive-Abfrage können Sie die Hive-Ansichten verwenden, die im Ambari-Portal verfügbar sind. Anweisungen zum Verwenden der Ambari Hive-Ansichten finden Sie unter [Verwenden der Hive-Ansicht mit Hadoop in HDInsight](../hdinsight/hdinsight-hadoop-use-hive-ambari-view.md). Sie müssen einige Dinge ändern, wenn Sie Daten in Data Lake Store verwenden.

* Basierend auf dem Beispielcluster, den wir mit Data Lake Store als primären Speicher erstellt haben, lautet der Pfad zu den Daten `adl://<data_lake_store_account_name>/azuredatalakestore.net/path/to/file`. Eine Hive-Abfrage zum Erstellen einer Tabelle aus Beispieldaten, die im Data Lake Store-Konto gespeichert sind, sieht wie folgt aus:

        CREATE EXTERNAL TABLE websitelog (str string) LOCATION 'adl://hdiadlsstorage.azuredatalakestore.net/clusters/myhdiadlcluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/'

In der obigen Abfrage gilt:

* `adl://hdiadlstorage.azuredatalakestore.net/` ist der Stamm des Data Lake Store-Kontos.
* `/clusters/myhdiadlcluster` ist der Stamm für die Clusterdaten, die Sie beim Erstellen des Clusters angegeben haben.
* `/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/` ist der Speicherort der Beispieldatei, die Sie in der Abfrage verwenden.

### <a name="run-a-hive-query-against-data-stored-in-data-lake-store-as-additional-storage"></a>Ausführen einer Hive-Abfrage für in Data Lake Store gespeicherte Daten (zusätzlicher Speicher)

Wenn der von Ihnen erstellte Cluster Azure Storage (WASB) als Standardspeicher verwendet, befinden sich die Beispieldaten nicht im Azure Data Lake Store-Konto, das als zusätzlicher Speicher verwendet wird. In diesen Fällen müssen Sie die Daten zunächst von WASB in Azure Data Lake Store übertragen und die Abfragen dann wie oben gezeigt ausführen.

Informationen zum Kopieren von Daten aus WASB in Azure Data Lake Store finden hier:

* [Kopieren von Daten zwischen Azure Storage-Blobs und Data Lake Store mithilfe von Distcp](data-lake-store-copy-data-wasb-distcp.md)
* [Verwenden des AdlCopy-Tools zum Kopieren von Daten aus Azure Storage-Blobs in Data Lake Store](data-lake-store-copy-data-azure-storage-blob.md) 

### <a name="use-data-lake-store-with-spark-cluster"></a>Verwenden eines Data Lake-Speichers mit Spark-Clustern
Sie können einen Spark-Cluster zum Ausführen von Spark-Aufträgen für Daten verwenden, die in Data Lake Store gespeichert sind. Anweisungen dazu finden Sie unter [Verwenden des HDInsight Spark-Clusters, um Daten in Data Lake Store zu analysieren](../hdinsight/hdinsight-apache-spark-use-with-data-lake-store.md).


### <a name="use-data-lake-store-in-a-storm-topology"></a>Verwenden des Data Lake-Speichers in einer Storm-Topologie
Sie können den Data Lake-Speicher verwenden, um dort Daten aus einer Storm-Topologie zu schreiben. Informationen zum Umsetzen dieses Szenarios finden Sie unter [Verwenden von Azure Data Lake Store mit Apache Storm und HDInsight](../hdinsight/hdinsight-storm-write-data-lake-store.md).

## <a name="see-also"></a>Siehe auch
* [PowerShell: Erstellen eines HDInsight-Clusters für die Verwendung des Data Lake-Speichers](data-lake-store-hdinsight-hadoop-use-powershell.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx

