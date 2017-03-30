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
ms.date: 03/21/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 1429bf0d06843da4743bd299e65ed2e818be199d
ms.openlocfilehash: f7c977dc2e385819dada976afa9497e9a20fd90c
ms.lasthandoff: 03/22/2017


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

* Eine Option zum Erstellen von HDInsight-Clustern mit Zugriff auf Data Lake Store als Standardspeicher ist für HDInsight-Version 3.5 verfügbar.

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
    >

## <a name="create-an-hdinsight-cluster-with-access-to-a-data-lake-store"></a>Erstellen eines HDInsight-Clusters mit Zugriff auf einen Data Lake Store-Speicher
In diesem Abschnitt erstellen Sie einen HDInsight Hadoop-Cluster, für den ein Data Lake-Speicher als Standardspeicher oder zusätzlicher Speicher verwendet wird.

### <a name="create-a-cluster-with-data-lake-store-as-default-storage"></a>Erstellen eines Clusters mit Data Lake-Speicher als Standardspeicher

>[!NOTE]
>Sie können diese Option nur mit HDInsight 3.5-Clustern (Standard-Edition) verwenden. In HDInsight 3.5-Clustern ist diese Option für den HBase-Clustertyp nicht verfügbar.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

2. Führen Sie die Schritte unter [Erstellen von Hadoop-Clustern in HDInsight](../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md) aus, um mit der Bereitstellung eines HDInsight-Clusters zu beginnen.

3. Klicken Sie auf dem Blatt **Speicher** unter **Primärer Speichertyp** auf **Data Lake-Speicher**.

4. Wählen Sie ein bereits vorhandenes Data Lake Store-Konto aus, und geben Sie einen Stammordnerpfad ein, in dem die clusterspezifischen Dateien gespeichert werden sollen.

    ![Dienstprinzipal für HDInsight-Cluster hinzufügen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.1.adls.storage.png "Dienstprinzipal für HDInsight-Cluster hinzufügen")

    
    In diesem Screenshot ist der Stammordnerpfad „/clusters/myhdiadlcluster“, wobei *myhdiadlcluster* der Name des Clusters ist, der erstellt wird. Stellen Sie in einem solchen Fall sicher, dass der Ordner */clusters* im Data Lake Store-Konto vorhanden ist. Der Ordner *myhdiadlcluster* wird während der Erstellung des Clusters erstellt. Wenn der Stammpfad auf */hdinsight/clusters/data/myhdiadlcluter* festgelegt wurde, stellen Sie sicher, dass */hdinsight/clusters/data/* im Data Lake Store-Konto vorhanden ist.

5. Klicken Sie auf **Data Lake Store-Zugriff**, um den Zugriff zwischen dem Data Lake Store-Konto und dem HDInsight-Cluster zu konfigurieren. Anweisungen hierzu finden Sie unter [Konfigurieren des Zugriffs zwischen HDInsight-Cluster und Data Lake Store](#configure-access-between-hdinsight-cluster-and-data-lake-store). 


### <a name="create-a-cluster-with-data-lake-store-as-additional-storage"></a>Erstellen eines Clusters mit Data Lake Store als zusätzlichem Speicher 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

2. Führen Sie die Schritte unter [Erstellen von Hadoop-Clustern in HDInsight](../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md) aus, um mit der Bereitstellung eines HDInsight-Clusters zu beginnen.

3. Wählen Sie auf dem Blatt **Speicher** unter **Primärer Speichertyp** die Option **Azure Storage** aus.
 
4. Aktivieren Sie unter **Auswahlmethode** die gewünschte Methode:

    * Um ein Speicherkonto anzugeben, das Teil Ihres Azure-Abonnements ist, wählen Sie **Meine Abonnements** und dann ein Speicherkonto aus.

    * Um ein Speicherkonto anzugeben, das außerhalb Ihres Azure-Abonnements liegt, wählen Sie **Zugriffsschlüssel** aus, und geben Sie dann die Informationen des externen Speicherkontos an.

5. Übernehmen Sie unter **Standardcontainer** den Standardcontainernamen, der vom Portal vorgeschlagen wird, oder geben Sie Ihren eigenen Containernamen an.

    ![Dienstprinzipal für HDInsight-Cluster hinzufügen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.1.png "Dienstprinzipal für HDInsight-Cluster hinzufügen")

6. Bei der Verwendung eines Blob-Speichers als Standardspeicher können Sie trotzdem Data Lake Store als zusätzlichen Speicher für den Cluster verwenden. Klicken Sie dazu auf **Data Lake Store-Zugriff**, um den Zugriff zwischen dem Data Lake Store-Konto und dem HDInsight-Cluster zu konfigurieren. Anweisungen hierzu finden Sie unter [Konfigurieren des Zugriffs zwischen HDInsight-Cluster und Data Lake Store](#configure-access-between-hdinsight-cluster-and-data-lake-store).

## <a name="configure-access-between-hdinsight-cluster-and-data-lake-store"></a>Konfigurieren des Zugriffs zwischen HDInsight-Cluster und Data Lake Store

In diesem Abschnitt konfigurieren Sie den Zugriff zwischen HDInsight-Clustern und Data Lake Store mithilfe eines Azure Active Directory-Dienstprinzipals.

1. Geben Sie auf dem Blatt **Data Lake Store-Zugriff** an, ob ein neuer Dienstprinzipal oder ein vorhandener Dienstprinzipal verwendet werden soll. In diesem Schritt wird ein neuer Dienstprinzipal erstellt. Möchten Sie einen vorhandenen Dienstprinzipal verwenden, wechseln Sie zum nächsten Schritt.

    a. Klicken Sie auf dem Blatt **Data Lake Store-Zugriff** auf **Neu erstellen**, und klicken Sie dann auf **Dienstprinzipal**.

    b. Geben Sie auf dem Blatt **Erstellen eines Dienstprinzipals** die erforderlichen Werte ein.  

    c. Klicken Sie auf **Erstellen**. Ein Zertifikat und eine Azure AD-Anwendung werden automatisch erstellt.

    ![Dienstprinzipal für HDInsight-Cluster hinzufügen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.2.png "Dienstprinzipal für HDInsight-Cluster hinzufügen")

    Sie können auch auf **Zertifikat herunterladen** klicken, um das Zertifikat herunterzuladen, das dem erstellten Dienstprinzipal zugeordnet ist. Das Herunterladen des Zertifikats ist nützlich, wenn Sie den gleichen Dienstprinzipal beim Erstellen eines zusätzlichen HDInsight-Clusters verwenden möchten. Klicken Sie auf **Auswählen**.

2. Um einen vorhandenen Dienstprinzipal zu verwenden, führen Sie die folgenden Schritte aus.

    a. Klicken Sie auf dem Blatt **Data Lake Store-Zugriff** auf **Use existing** (Vorhandenes verwenden), und klicken Sie dann auf **Dienstprinzipal**.

    b. Suchen Sie auf dem Blatt **AD-Dienstprinzipal auswählen** nach einem vorhandenen Dienstprinzipal. Klicken Sie auf den Dienstprinzipal, den Sie verwenden möchten, und klicken Sie dann auf **Auswählen**.

    c. Laden Sie auf dem Blatt **Data Lake Store-Zugriff** das Zertifikat (.pfx-Datei) hoch, das dem ausgewählten Dienstprinzipal zugeordnet ist, und geben Sie dann das Zertifikatkennwort ein.

    ![Dienstprinzipal für HDInsight-Cluster hinzufügen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.5.png "Dienstprinzipal für HDInsight-Cluster hinzufügen")

7. Klicken Sie auf dem Blatt **Data Lake Store-Zugriff** auf **Zugriff**. Das Blatt **Dateiberechtigungen auswählen** ist standardmäßig geöffnet. Hier sind alle Data Lake Store-Konten in Ihrem Abonnement aufgeführt.

8. Wählen Sie das Data Lake Store-Konto, das dem Cluster zugeordnet werden soll. 

    **Wenn Sie Data Lake Store als Standardspeicher verwenden**, müssen Sie Berechtigungen auf zwei Ebenen zuweisen.

    a. Zuerst müssen Sie die Berechtigung auf der Stammebene des Data Lake Store-Kontos zuweisen. Zeigen Sie dazu mit dem Mauszeiger auf den Namen des Data Lake Store-Kontos, um das Kontrollkästchen sichtbar zu machen. Aktivieren Sie das Kontrollkästchen.

    ![Dienstprinzipal für HDInsight-Cluster hinzufügen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.3.png "Dienstprinzipal für HDInsight-Cluster hinzufügen")

    b. Anschließend müssen Sie die Berechtigung für den Stammordner des HDInsight-Clusterspeichers zuweisen. Entsprechend dem früheren Screenshot ist der Ordner **/clusters**, den Sie beim Auswählen von Data Lake Store als Standardspeicher angegeben haben, der Stammordner des Clusterspeichers.

    ![Dienstprinzipal für HDInsight-Cluster hinzufügen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.3.add.png "Dienstprinzipal für HDInsight-Cluster hinzufügen")

    **Wenn Sie Data Lake Store als zusätzlichen Speicher verwenden**, müssen Sie die Berechtigung nur für den Ordner zuweisen, auf den Sie aus dem HDInsight-Cluster zugreifen möchten. Im folgenden Screenshot wird z. B. nur Zugriff auf den Ordner **hdiaddonstorage** in einem Data Lake Store-Konto gewährt.

    ![Assign service principal permissions to the HDInsight cluster (Dem HDInsight-Cluster Dienstberechtigungen zuweisen)](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.3-1.png "(Assign service principal permissions to the HDInsight cluster (Dem HDInsight-Cluster Dienstberechtigungen zuweisen)")

9. Wählen Sie für alle Konten und Pfade, die Sie ausgewählt haben, die Berechtigungen (Lesen, Schreiben oder Ausführen) aus, und geben Sie an, ob die Berechtigungen auch rekursiv für die untergeordneten Elemente gelten sollen. Klicken Sie dann auf **Auswählen**.

11. Klicken Sie im Fenster **Assign selected permissions** (Ausgewählte Berechtigungen auswählen) auf **Ausführen**, um die Berechtigungen für den Azure AD-Dienstprinzipal dem Konto und den Dateien und Ordnern zuzuweisen, die Sie ausgewählt haben.

    ![Assign service principal permissions to the HDInsight cluster (Dem HDInsight-Cluster Dienstberechtigungen zuweisen)](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.3-2.png "(Assign service principal permissions to the HDInsight cluster (Dem HDInsight-Cluster Dienstberechtigungen zuweisen)")

12. Nachdem die Berechtigungen erfolgreich zugewiesen wurden, klicken Sie auf jedem geöffneten Blatt auf **Fertig**, um zum Blatt **Data Lake Store-Zugriff** zurückzukehren.

13. Klicken Sie auf dem Blatt **Data Lake Store-Zugriff** auf **Auswählen**, und fahren Sie mit der Clustererstellung wie unter [Erstellen von Hadoop-Clustern in HDInsight](../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md) beschrieben fort.

## <a name="verify-cluster-set-up"></a>Überprüfen des Clustersetups

Nachdem das Clustersetup abgeschlossen ist, überprüfen Sie auf dem Blatt „Cluster“ Ihre Ergebnisse, indem Sie eine oder beide der folgenden Optionen durchführen:

* Um sicherzustellen, dass der zugeordnete Speicher für das Cluster das Data Lake Store-Konto ist, das Sie angegeben haben, klicken Sie auf **Speicherkonten** im linken Fensterbereich.

    ![Dienstprinzipal für HDInsight-Cluster hinzufügen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.6-1.png "Dienstprinzipal für HDInsight-Cluster hinzufügen")

* Um sicherzustellen, dass der Dienstprinzipal dem HDInsight-Cluster ordnungsgemäß zugeordnet wurde, klicken Sie auf **Data Lake Store-Zugriff** im linken Fensterbereich.

    ![Dienstprinzipal für HDInsight-Cluster hinzufügen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.6.png "Dienstprinzipal für HDInsight-Cluster hinzufügen")


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

