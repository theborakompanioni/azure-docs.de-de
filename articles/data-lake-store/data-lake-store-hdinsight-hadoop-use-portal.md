---
title: Verwenden Sie das Azure-Portal zum Erstellen von Azure HDInsight-Clustern mit Data Lake Store | Microsoft-Dokumentation
description: Verwenden des Azure-Portals zum Erstellen und Verwenden von HDInsight-Clustern mit Azure Data Lake Store
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
ms.date: 03/02/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 7c28fda22a08ea40b15cf69351e1b0aff6bd0a95
ms.openlocfilehash: d1d780eb2c635f60409396804c0b8b706e59127b
ms.lasthandoff: 03/07/2017


---
# <a name="create-hdinsight-clusters-with-data-lake-store-by-using-the-azure-portal"></a>Erstellen von HDInsight-Clustern mit Azure Data Lake Store mithilfe des Azure-Portals
> [!div class="op_single_selector"]
> * [Verwenden des Azure-Portals](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [Verwenden von PowerShell (für Standardspeicher)](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [Verwenden von PowerShell (für zusätzlichen Speicher)](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Verwenden des Ressourcen-Managers](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

In diesem Artikel erfahren Sie, wie Sie das Azure-Portal zum Erstellen eines HDInsight-Clusters mit Zugriff auf Azure Data Lake Store verwenden. Für unterstützte Clustertypen können Sie Data Lake Store als Standardspeicherkonto oder als zusätzliches Speicherkonto verwenden. 

Wenn Sie Data Lake Store als zusätzlichen Speicher verwenden, ist das Standardspeicherkonto der Cluster nach wie vor Azure Blob Storage, und die mit dem Cluster in Verbindung stehenden Dateien (wie etwa Protokolle) werden immer noch in den Standardspeicher geschrieben. Allerdings können die Daten, die Sie verarbeiten möchten, in ein Data Lake Store-Konto gespeichert werden. Das Verwenden von Data Lake Store als zusätzliches Speicherkonto wirkt sich nicht auf die Leistung oder die Fähigkeit aus, Daten aus dem Cluster in den Speicher zu lesen bzw. zu schreiben.

Hier finden Sie einige wichtige Überlegungen zur Verwendung von HDInsight mit Data Lake Store:

* Die Option zum Erstellen von HDInsight-Clustern mit Zugriff auf Data Lake Store als Standardspeicher ist für HDInsight Version 3.5 verfügbar.

* Die Option zum Erstellen von HDInsight-Clustern mit Zugriff auf Data Lake Store als Standardspeicher ist für HDInsight Premium-Cluster *nicht verfügbar*.

* Die Option zum Erstellen von HDInsight-Clustern mit Zugriff auf Data Lake Store als zusätzlichen Speicher ist für die HDInsight-Versionen 3.2, 3.4 und 3.5 verfügbar.

* Für HBase-Cluster (Windows und Linux) wird Data Lake Store *nicht* als Speicheroption unterstützt, weder für Standardspeicher noch für zusätzlichen Speicher.

* Bei Storm-Clustern (Windows und Linux) können Sie Data Lake Store zum Schreiben von Daten aus einer Storm-Topologie verwenden. Sie können Data Lake Store auch zum Speichern von Verweisdaten verwenden, die anschließend von einer Storm-Topologie gelesen werden. Weitere Informationen finden Sie unter [Verwenden des Data Lake-Speichers in einer Storm-Topologie](#use-data-lake-store-in-a-storm-topology).


## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Tutorial beginnen, stellen Sie sicher, dass Sie die folgenden Anforderungen erfüllen:

* **Ein Azure-Abonnement**. Navigieren Sie zu [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

* **Ein Azure Data Lake-Speicherkonto**. Führen Sie die Schritte der Anleitung unter [Erste Schritte mit Data Lake Store mithilfe des Azure-Portals](data-lake-store-get-started-portal.md) aus.

* **Azure Active Directory-Dienstprinzipal**. Dieses Tutorial enthält Anweisungen zum Erstellen eines Dienstprinzipals in Azure Active Directory (Azure AD). Sie müssen jedoch Azure AD-Administrator sein, um einen Dienstprinzipal erstellen zu können. Wenn Sie Administrator sind, können Sie diese Voraussetzung ignorieren und mit dem Tutorial fortfahren.

 >[!NOTE]
 >Sie können einen Dienstprinzipal nur dann erstellen, wenn Sie ein Azure AD-Administrator sind. Ihr Azure AD-Administrator muss zunächst einen Dienstprinzipal erstellen, bevor Sie einen HDInsight-Cluster mit Data Lake Store erstellen können. Zudem muss der Dienstprinzipal mit einem Zertifikat erstellt werden, wie unter [Create a service principal with certificate (Erstellen eines Dienstprinzipals mit einem Zertifikat)](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate) beschrieben.

## <a name="create-an-hdinsight-cluster-with-access-to-a-data-lake-store"></a>Erstellen eines HDInsight-Clusters mit Zugriff auf einen Data Lake Store-Speicher
In diesem Abschnitt erstellen Sie einen HDInsight Hadoop-Cluster, für den ein Data Lake Store-Speicher als zusätzlicher Speicher verwendet wird. In dieser Version kann Data Lake Store für einen Hadoop-Cluster nur als zusätzlicher Speicher für den Cluster verwendet werden. Der Standardspeicher ist nach wie vor Azure Blob Storage. Sie erstellen daher zuerst das Speicherkonto und die Speichercontainer, die für den Cluster erforderlich sind.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

2. Führen Sie die Schritte unter [Erstellen von Hadoop-Clustern in HDInsight](../hdinsight/hdinsight-provision-clusters.md) aus, um mit der Bereitstellung eines HDInsight-Clusters zu beginnen.

3. Geben Sie auf dem Blatt **Speicher** an, ob Sie den Blob-Speicher oder Data Lake Store als Standardspeicher verwenden möchten; dann haben Sie mehrere Optionen, mit denen Sie fortfahren können:

 * Um Data Lake Store als Standardspeicher zu verwenden, fahren Sie mit Schritt 4 fort.

 * Um den Blob-Speicher als Standardspeicher zu verwenden, haben Sie folgende Optionen:

    a. Wählen Sie unter **Primärer Speichertyp** **Azure Storage** aus.

    b. Entscheiden Sie sich unter **Auswahlmethode**für eine der folgenden Optionen:      *Um ein Speicherkonto anzugeben, das Teil Ihres Azure-Abonnements ist, wählen Sie **Meine Abonnements** und dann ein Speicherkonto aus.      *Um ein Speicherkonto anzugeben, das außerhalb Ihres Azure-Abonnements liegt, wählen Sie **Zugriffsschlüssel**, und geben Sie dann die Informationen des externen Speicherkontos an.

    c. Geben Sie unter **Standardcontainer** den Standardcontainernamen an, der vom Portal vorgeschlagen wird, oder geben Sie Ihren eigenen an.

 Bei der Verwendung eines Blob-Speichers als Standardspeicher können Sie trotzdem Data Lake Store als zusätzlichen Speicher für den Cluster verwenden. Klicken Sie dazu auf **Data Lake Store-Zugriff**, und fahren Sie mit Schritt 5 fort.

 ![Dienstprinzipal für HDInsight-Cluster hinzufügen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.1.png "Dienstprinzipal für HDInsight-Cluster hinzufügen")

4. Um Data Lake Store als Standardspeicher zu verwenden, haben Sie folgende Optionen:

 a. Klicken Sie unter **Primärer Speichertyp** auf **Data Lake Store**.

 b. Wählen Sie ein bereits vorhandenes Data Lake Store-Konto aus, geben Sie einen Stammordnerpfad an, in dem die clusterspezifischen Dateien gespeichert werden sollen, geben Sie den **Standort** als **USA, Osten 2** an, und klicken Sie dann auf **Data Lake Store-Zugriff**.

 >[!NOTE]
 >Sie können diese Option nur mit HDInsight 3.5-Clustern (Standard-Edition) verwenden. In HDInsight 3.5-Clustern ist diese Option für den HBase-Clustertyp nicht verfügbar.

 Im folgenden Screenshot ist der Stammordnerpfad „/clusters/myhdiadlcluster“, wobei *myhdiadlcluster* der Name des Clusters ist, der erstellt wird. Stellen Sie in einem solchen Fall sicher, dass der Ordner */clusters* im Data Lake Store-Konto vorhanden ist. Der Ordner *myhdiadlcluster* wird während der Erstellung des Clusters erstellt. Wenn der Stammpfad auf */hdinsight/clusters/data/myhdiadlcluter* festgelegt wurde, stellen Sie sicher, dass */hdinsight/clusters/data/* im Data Lake Store-Konto vorhanden ist.

 ![Dienstprinzipal für HDInsight-Cluster hinzufügen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.1.adls.storage.png "Dienstprinzipal für HDInsight-Cluster hinzufügen")

5. Führen Sie auf dem Blatt **Data Lake Store-Zugriff** eine der folgenden Optionen aus:

 * Um einen vorhandenen Dienstprinzipal zu verwenden, fahren Sie mit Schritt 6 fort.
 * Um einen Dienstprinzipal zu erstellen, haben Sie folgende Optionen:

    a. Klicken Sie auf dem Blatt **Data Lake Store-Zugriff** auf **Neu erstellen**, und klicken Sie dann auf **Dienstprinzipal**.

    b. Geben Sie auf dem Blatt **Erstellen eines Dienstprinzipals** die erforderlichen Werte ein.  

    c. Klicken Sie auf **Erstellen**.  
    Ein Zertifikat und eine Azure AD-Anwendung werden automatisch erstellt.

    ![Dienstprinzipal für HDInsight-Cluster hinzufügen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.2.png "Dienstprinzipal für HDInsight-Cluster hinzufügen")

    Sie können auch auf **Zertifikat herunterladen** klicken, um das Zertifikat herunterzuladen, das dem erstellten Dienstprinzipal zugeordnet ist. Das Herunterladen des Zertifikats ist nützlich, wenn Sie den gleichen Dienstprinzipal beim Erstellen eines zusätzlichen HDInsight-Clusters verwenden möchten. Klicken Sie auf **Auswählen**.

6. Um ein vorhandenes Dienstprinzipal zu verwenden, haben Sie folgende Optionen:

 a. Klicken Sie auf dem Blatt **Data Lake Store-Zugriff** auf **Use existing** (Vorhandenes verwenden), und klicken Sie dann auf **Dienstprinzipal**.

 b. Suchen Sie auf dem Blatt **AD-Dienstprinzipal auswählen** nach einem vorhandenen Dienstprinzipal. Klicken Sie auf den Dienstprinzipal, den Sie verwenden möchten, und klicken Sie dann auf **Auswählen**.

 c. Laden Sie auf dem Blatt **Data Lake Store-Zugriff** das Zertifikat (.pfx-Datei) hoch, das dem ausgewählten Dienstprinzipal zugeordnet ist, und geben Sie dann das Zertifikatkennwort ein.

 ![Dienstprinzipal für HDInsight-Cluster hinzufügen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.5.png "Dienstprinzipal für HDInsight-Cluster hinzufügen")

7. Klicken Sie auf dem Blatt **Data Lake Store-Zugriff** auf **Zugriff**.  
Das Blatt **Dateiberechtigungen auswählen** ist standardmäßig geöffnet. Hier sind alle Data Lake Store-Konten in Ihrem Abonnement aufgeführt.

8. Wählen Sie das Data Lake Store-Konto, das dem Cluster zugeordnet werden soll.
 Hier sind alle Dateien und Ordner in diesem Konto aufgeführt. Sie können dann Berechtigungen auf Datei- oder Ordnerebene zuweisen. Wenn Sie die Berechtigungen auf der Stammebene des Kontos zuweisen möchten, aktivieren Sie das Kontrollkästchen neben dem Kontonamen.

 ![Dienstprinzipal für HDInsight-Cluster hinzufügen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.3.png "Dienstprinzipal für HDInsight-Cluster hinzufügen")

 > [!NOTE]
 > Wenn Sie das Data Lake Store-Konto als Standardspeicher für einen Cluster verwenden, weisen Sie dem Dienstprinzipal die Berechtigungen auf Stammebene des Data Lake Store-Kontos zu.

9. Um Berechtigungen für Dateien oder Ordner innerhalb eines Kontos zuzuweisen, wählen Sie im Fenster **Dateiberechtigungen auswählen** das Data Lake Store-Konto aus.

10. Wählen Sie im rechten Fensterbereich die Dateien und Ordner aus, denen Sie Berechtigungen zuweisen möchten; wählen Sie die Berechtigungen (zum Lesen, Schreiben oder Ausführen) aus, geben Sie an, ob die Berechtigungen auch für untergeordnete Elemente gelten, und klicken Sie dann auf **Auswählen**.

    ![Assign service principal permissions to the HDInsight cluster (Dem HDInsight-Cluster Dienstberechtigungen zuweisen)](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.3-1.png "Assign service principal permissions to the HDInsight cluster (Dem HDInsight-Cluster Dienstberechtigungen zuweisen)")

11. Klicken Sie im Fenster **Assign selected permissions** (Ausgewählte Berechtigungen auswählen) auf **Ausführen**, um die Berechtigungen für den Azure AD-Dienstprinzipal dem Konto und den Dateien und Ordnern zuzuweisen, die Sie ausgewählt haben.

    ![Assign service principal permissions to the HDInsight cluster (Dem HDInsight-Cluster Dienstberechtigungen zuweisen)](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.3-2.png "(Assign service principal permissions to the HDInsight cluster (Dem HDInsight-Cluster Dienstberechtigungen zuweisen)")

12. Nachdem die Berechtigungen erfolgreich zugewiesen wurden, klicken Sie auf jedem geöffneten Blatt auf **Fertig**, um zum Blatt **Data Lake Store-Zugriff** zurückzukehren.

13. Klicken Sie auf dem Blatt **Data Lake Store-Zugriff** auf **Auswählen**, und fahren Sie mit der Clustererstellung wie unter [Erstellen von Hadoop-Clustern in HDInsight](../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md) beschrieben fort.

14. Nachdem das Clustersetup abgeschlossen ist, überprüfen Sie auf dem Blatt „Cluster“ Ihre Ergebnisse, indem Sie eine oder beide der folgenden Optionen durchführen:

 * Um sicherzustellen, dass der zugeordnete Speicher für das Cluster das Data Lake Store-Konto ist, das Sie angegeben haben, klicken Sie auf **Speicherkonten** im linken Fensterbereich.

        ![Add service principal to HDInsight cluster](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.6-1.png "Add service principal to HDInsight cluster")

 * Um sicherzustellen, dass der Dienstprinzipal dem HDInsight-Cluster ordnungsgemäß zugeordnet wurde, klicken Sie auf **Data Lake Store-Zugriff** im linken Fensterbereich.

       ![Add service principal to HDInsight cluster](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.6.png "Add service principal to HDInsight cluster")

## <a name="examples"></a>Beispiele

Nachdem Sie den Cluster mit Data Lake Store als Speicher eingerichtet haben, können Sie sich diese Beispiele ansehen, um zu erfahren, wie Sie HDInsight-Cluster verwenden können, um Daten zu analysieren, die in Data Lake Store gespeichert sind.

### <a name="run-a-hive-query-against-data-in-a-data-lake-store-as-primary-storage"></a>Ausführen einer Hive-Abfrage für in Data Lake Store gespeicherte Daten (primärer Speicher)

Verwenden Sie zum Ausführen einer Hive-Abfrage die Hive-Ansichten-Schnittstelle im Ambari-Portal. Anweisungen zum Verwenden der Hive-Ansichten mit Ambari finden Sie unter [Verwenden der Hive-Ansicht mit Hadoop in HDInsight](../hdinsight/hdinsight-hadoop-use-hive-ambari-view.md).

Wenn Sie mit Daten in Data Lake Store arbeiten, müssen einige Zeichenfolgen geändert werden.

Wenn Sie beispielsweise das Cluster verwenden, das Sie mit Data Lake Store als primären Speicher erstellt haben, ist der Datenpfad folgender: *adl://<data_lake_store_account_name>/azuredatalakestore.net/path/to/file*. Eine Hive-Abfrage zum Erstellen einer Tabelle aus Beispieldaten, die im Data Lake Store-Konto gespeichert sind, sieht wie folgt aus:

    CREATE EXTERNAL TABLE websitelog (str string) LOCATION 'adl://hdiadlsstorage.azuredatalakestore.net/clusters/myhdiadlcluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/'

Beschreibungen:
* `adl://hdiadlstorage.azuredatalakestore.net/` ist der Stamm des Data Lake Store-Kontos.
* `/clusters/myhdiadlcluster` ist der Stamm der Clusterdaten, die Sie beim Erstellen des Clusters angegeben haben.
* `/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/` ist der Speicherort der Beispieldatei, die Sie in der Abfrage verwenden.

### <a name="run-a-hive-query-against-data-in-a-data-lake-store-as-additional-storage"></a>Ausführen einer Hive-Abfrage für in Data Lake Store gespeicherte Daten (zusätzlicher Speicher)

Wenn der von Ihnen erstellte Cluster einen Blob-Speicher als Standardspeicher verwendet, befinden sich die Beispieldaten nicht im Azure Data Lake Store-Konto, das als zusätzlicher Speicher verwendet wird. Übertragen Sie in einem derartigen Fall die Daten aus dem Blob-Speicher auf Data Lake Store, und führen Sie dann die Abfragen wie in obigem Beispiel aus.

Informationen zum Kopieren von Daten aus dem Blob-Speicher in Data Lake Store finden Sie in folgenden Artikeln:

* [Kopieren von Daten zwischen Azure Storage-Blobs und Data Lake Store mithilfe von Distcp](data-lake-store-copy-data-wasb-distcp.md)
* [Kopieren von Daten aus Azure Storage-Blobs in Data Lake Store mithilfe von AdlCopy](data-lake-store-copy-data-azure-storage-blob.md)

### <a name="use-data-lake-store-with-a-spark-cluster"></a>Verwenden von Data Lake Store mit einem Spark-Cluster
Sie können einen Spark-Cluster zum Ausführen von Spark-Aufträgen für Daten verwenden, die in Data Lake Store gespeichert sind. Weitere Informationen finden Sie unter [Verwenden des HDInsight Spark-Clusters, um Daten in Data Lake Store zu analysieren](../hdinsight/hdinsight-apache-spark-use-with-data-lake-store.md).


### <a name="use-data-lake-store-in-a-storm-topology"></a>Verwenden des Data Lake-Speichers in einer Storm-Topologie
Sie können den Data Lake-Speicher verwenden, um dort Daten aus einer Storm-Topologie zu schreiben. Informationen zum Umsetzen dieses Szenarios finden Sie unter [Verwenden von Azure Data Lake Store mit Apache Storm und HDInsight](../hdinsight/hdinsight-storm-write-data-lake-store.md).

## <a name="see-also"></a>Siehe auch
* [PowerShell: Erstellen eines HDInsight-Clusters für die Verwendung des Data Lake-Speichers](data-lake-store-hdinsight-hadoop-use-powershell.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx

