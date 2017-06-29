---
title: Verwenden von Data Lake Store mit Hadoop in Azure HDInsight | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Daten in Azure Data Lake Store abfragen und die Ergebnisse Ihrer Analyse speichern.
keywords: Blobspeicher,HDFS,strukturierte Daten,unstrukturierte Daten,Data Lake Store
services: hdinsight,storage
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/10/2017
ms.author: jgao
ms.translationtype: Human Translation
ms.sourcegitcommit: 5bbeb9d4516c2b1be4f5e076a7f63c35e4176b36
ms.openlocfilehash: b60a0d1668e7c1010663ee2b11bff26b0bb70cf0
ms.contentlocale: de-de
ms.lasthandoff: 06/13/2017


---
# <a name="use-data-lake-store-with-azure-hdinsight-clusters"></a>Verwenden von Data Lake Store mit Azure HDInsight-Clustern

Zum Analysieren von Daten im HDInsight-Cluster können Sie die Daten in [Azure Storage](../storage/storage-introduction.md), [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md) oder beidem speichern. Beide Speichervarianten ermöglichen das sichere Löschen der HDInsight-Cluster, die für Berechnungen verwendet werden, ohne Benutzerdaten zu verlieren.

In diesem Artikel erfahren Sie, wie Data Lake Store mit HDInsight-Clustern funktioniert. Weitere Informationen zur Funktionsweise von Azure Store mit HDInsight-Clustern finden Sie unter [Verwenden von Azure Storage mit Azure HDInsight-Clustern](hdinsight-hadoop-use-blob-storage.md). Weitere Informationen zum Erstellen eines HDInsight-Clusters erhalten Sie unter [Erstellen von Hadoop-Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

> [!NOTE]
> Da auf Data Lake Store immer über einen sicheren Kanal zugegriffen wird, ist kein `adls`-Dateisystem-Schemaname vorhanden. Sie verwenden immer `adl`.
> 
> 

## <a name="availabilities-for-hdinsight-clusters"></a>Verfügbarkeit für HDInsight-Cluster

Hadoop unterstützt eine Variante des Standarddateisystems. Das Standarddateisystem gibt ein Standardschema und eine Standardautorität vor. Es kann auch zur Auflösung relativer Pfade verwendet werden. Bei der Erstellung des HDInsight-Clusters können Sie einen Blobcontainer in Azure Storage als Standarddateisystem angeben. Mit HDInsight 3.5 oder einer neueren Version können Sie mit einigen wenigen Ausnahmen Azure Storage oder Azure Data Lake Store als Standarddateisystem auswählen. 

Für HDInsight-Cluster kann Data Lake Store auf zwei Arten verwendet werden:

* Als Standardspeicher
* Als zusätzlicher Speicher mit Azure Storage Blob als Standardspeicher

Zum aktuellen Zeitpunkt unterstützen nur einige der HDInsight-Clustertypen/-versionen Data Lake Store als Konten für Standardspeicher und Zusatzspeicher:

| HDInsight-Clustertyp | Data Lake Store als Standardspeicher | Data Lake Store als Zusatzspeicher| Hinweise |
|------------------------|------------------------------------|---------------------------------------|------|
| HDInsight-Version 3.6 | Ja | Ja | |
| HDInsight-Version 3.5 | Ja | Ja | Mit Ausnahme von HBase|
| HDInsight-Version 3.4 | Nein | Ja | |
| HDInsight, Version 3.3 | Nein | Nein | |
| HDInsight, Version 3.2 | Nein | Ja | |
| HDInsight Premium (Tarif)| Nein | Nein | |
| Storm | | |Sie können Data Lake Store verwenden, um dort Daten aus einer Storm-Topologie zu schreiben. Sie können Data Lake Store auch zum Speichern von Verweisdaten verwenden, die anschließend von einer Storm-Topologie gelesen werden.|

Das Verwenden von Data Lake Store als zusätzliches Speicherkonto wirkt sich nicht auf die Leistung oder die Fähigkeit aus, Daten aus dem Cluster in Azure Storage zu lesen bzw. zu schreiben.


## <a name="use-data-lake-store-as-default-storage"></a>Verwenden von Data Lake Store als Standardspeicher

Wenn HDInsight mit Data Lake Store als Standardspeicher bereitgestellt wird, werden die clusterbezogenen Dateien in Data Lake Store am folgenden Ort gespeichert:

    adl://mydatalakestore/<cluster_root_path>/

Hierbei ist `<cluster_root_path>` der Name eines Ordners, den Sie in Data Lake Store erstellen. Indem Sie einen Stammpfad für jeden Cluster angeben, können Sie dasselbe Data Lake Store-Konto für mehrere Cluster verwenden. Beispielsweise können Sie das folgende Setup verwenden:

* Cluster1 kann den Pfad `adl://mydatalakestore/cluster1storage` nutzen.
* Cluster2 kann den Pfad `adl://mydatalakestore/cluster2storage` nutzen.

Beachten Sie, dass für beide Cluster dasselbe Data Lake Store-Konto **mydatalakestore** verwendet wird. Jeder Cluster hat in Data Lake Store Zugriff auf sein eigenes Stammdateisystem. Bei der Bereitstellung im Azure-Portal werden Sie aufgefordert, für den Stammpfad einen Ordnernamen wie **/clusters/\<Clustername>** zu verwenden.

Um Data Lake Store als Standardspeicher verwenden zu können, müssen Sie dem Dienstprinzipal Zugriff auf die folgenden Pfade gewähren:

- Das Stammverzeichnis für Data Lake Store.  Beispiel: adl://mydatalakestore/.
- Den Ordner für alle Clusterordner.  Beispiel: adl://mydatalakestore/clusters.
- Den Ordner für den Cluster.  Beispiel: adl://mydatalakestore/clusters/cluster1storage.

Weitere Informationen zum Erstellen von Dienstprinzipalen und zum Gewähren des Zugriffs für diese finden Sie unter [Konfigurieren des Zugriffs auf Data Lake Store](#configure-data-lake-store-access).


## <a name="use-data-lake-store-as-additional-storage"></a>Verwenden von Data Lake Store als Zusatzspeicher

Sie können Data Lake Store zudem als zusätzlichen Speicher für den Cluster verwenden. In solchen Fällen kann der Clusterstandardspeicher ein Azure Storage Blob- oder ein Data Lake Store-Konto sein. Wenn Sie HDInsight-Aufträge mit den in Data Lake Store gespeicherten Daten als zusätzlichem Speicher ausführen, müssen Sie den vollqualifizierten Pfad zu den Dateien verwenden. Beispiel:

    adl://mydatalakestore.azuredatalakestore.net/<file_path>

Beachten Sie, dass die URL jetzt keinen **cluster_root_path** enthält. Dies liegt daran, dass Data Lake Store hier kein Standardspeicher ist. Sie müssen also nur den Pfad zu den Dateien angeben.

Um Data Lake Store als zusätzlichen Speicher verwenden zu können, müssen Sie lediglich dem Dienstprinzipal Zugriff auf die Pfade gewähren, in denen Ihre Dateien gespeichert sind.  Beispiel:

    adl://mydatalakestore.azuredatalakestore.net/<file_path>

Weitere Informationen zum Erstellen von Dienstprinzipalen und zum Gewähren des Zugriffs für diese finden Sie unter [Konfigurieren des Zugriffs auf Data Lake Store](#configure-data-lake-store-access).


## <a name="use-more-than-one-data-lake-store-accounts"></a>Verwenden von mehreren Data Lake Store-Konten

Das Hinzufügen eines Data Lake Store-Kontos als Zusatz und das Hinzufügen von mehreren Data Lake Store-Konten wird erreicht, indem Sie dem HDInsight-Cluster den Zugriff auf Daten in einem oder mehreren Data Lake Store-Konten gewähren. Weitere Informationen finden Sie unter [Konfigurieren des Zugriffs auf Data Lake Store](#configure-data-lake-store-access).

## <a name="configure-data-lake-store-access"></a>Konfigurieren des Zugriffs auf Data Lake Store

Um den Zugriff auf Data Lake Store von Ihrem HDInsight-Cluster aus zu konfigurieren, benötigen Sie einen Azure Active Directory-Dienstprinzipal (Azure AD). Nur ein Azure AD-Administrator kann einen Dienstprinzipal erstellen. Der Dienstprinzipal muss mit einem Zertifikat erstellt werden. Weitere Informationen finden Sie unter [Konfigurieren des Zugriffs auf Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md#configure-data-lake-store-access) und [Erstellen eines Dienstprinzipals mit selbst signiertem Zertifikat](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-self-signed-certificate).

## <a name="access-files-from-the-cluster"></a>Zugreifen auf Dateien aus dem Cluster

Es gibt mehrere Möglichkeiten, wie Sie auf die Dateien in Data Lake Store über einen HDInsight-Cluster zugreifen können.

* **Verwenden des vollqualifizierten Namens** Bei diesem Ansatz geben Sie den vollständigen Pfad zu der Datei an, auf die Sie zugreifen möchten.

        adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/<file_path>

* **Verwenden des verkürzten Pfadformats** Bei diesem Ansatz ersetzen Sie den Pfad bis zum Clusterstamm durch „adl:///“. Im obigen Beispiel können Sie also `adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/` durch `adl:///` ersetzen.

        adl:///<file path>

* **Verwenden des relativen Pfads** Bei diesem Ansatz geben Sie nur den relativen Pfad zu der Datei an, auf die Sie zugreifen möchten. Der vollständige Pfad zur Datei lautet beispielsweise:

        adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/example/data/sample.log

    Sie können auf die gleiche Datei „sample.log“ zugreifen, indem Sie stattdessen diesen relativen Pfad verwenden:

        /example/data/sample.log

## <a name="create-hdinsight-clusters-with-access-to-data-lake-store"></a>Erstellen von HDInsight-Clustern mit Zugriff auf Data Lake Store

Unter den folgenden Links finden Sie eine ausführliche Anleitung, wie Sie HDInsight-Cluster mit Zugriff auf Data Lake Store erstellen.

* [Verwenden des Portals](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md)
* [Use Azure PowerShell to create an HDInsight cluster with Data Lake Store (as default storage)](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md) (Verwenden von Azure PowerShell zum Erstellen eines HDInsight-Clusters mit Data Lake Store (als Standardspeicher))
* [Erstellen eines HDInsight-Clusters mit Data Lake Store mithilfe von Azure PowerShell](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md)
* [Erstellen eines HDInsight-Clusters mit Data Lake Store mithilfe einer Azure Resource Manager-Vorlage](../data-lake-store/data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)


## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel wurde beschrieben, wie Sie Azure Data Lake Store HDFS-kompatibel mit HDInsight verwenden. Dadurch können Sie skalierbare Datenerfassungslösungen mit langfristiger Archivierung aufbauen und HDInsight verwenden, um die Informationen innerhalb der gespeicherten strukturierten und unstrukturierten Daten zu entsperren.

Weitere Informationen finden Sie unter:

* [Erste Schritte mit Azure HDInsight][hdinsight-get-started]
* [Erste Schritte mit Azure Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md)
* [Hochladen von Daten in HDInsight][hdinsight-upload-data]
* [Verwenden von Hive mit HDInsight][hdinsight-use-hive]
* [Verwenden von Pig mit HDInsight][hdinsight-use-pig]
* [Verwenden von Azure Storage Shared Access Signatures zum Einschränken des Zugriffs auf Daten mit HDInsight][hdinsight-use-sas]

[hdinsight-use-sas]: hdinsight-storage-sharedaccesssignature-permissions.md
[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-creation]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md

[blob-storage-restAPI]: http://msdn.microsoft.com/library/windowsazure/dd135733.aspx
[azure-storage-create]: ../storage/storage-create-storage-account.md

[img-hdi-powershell-blobcommands]: ./media/hdinsight-hadoop-use-blob-storage/HDI.PowerShell.BlobCommands.png
[img-hdi-quick-create]: ./media/hdinsight-hadoop-use-blob-storage/HDI.QuickCreateCluster.png
[img-hdi-custom-create-storage-account]: ./media/hdinsight-hadoop-use-blob-storage/HDI.CustomCreateStorageAccount.png  

