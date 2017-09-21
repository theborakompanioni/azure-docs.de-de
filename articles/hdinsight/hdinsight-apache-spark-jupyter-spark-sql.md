---
title: Erstellen eines Apache Spark-Clusters in Azure HDInsight | Microsoft-Dokumentation
description: HDInsight Spark-Schnellstartanleitung zum Erstellen eines Apache Spark-Clusters in HDInsight.
keywords: Spark Schnellstart,interaktiv Spark,interaktive Abfrage,HDInsight Spark,Azure Spark
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 91f41e6a-d463-4eb4-83ef-7bbb1f4556cc
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/07/2017
ms.author: nitinme
ms.translationtype: HT
ms.sourcegitcommit: 2c6cf0eff812b12ad852e1434e7adf42c5eb7422
ms.openlocfilehash: 78051b9df15c62d4caf56d800c9a5f4421ea2254
ms.contentlocale: de-de
ms.lasthandoff: 09/13/2017

---
# <a name="create-an-apache-spark-cluster-in-azure-hdinsight"></a>Erstellen eines Apache Spark-Clusters in Azure HDInsight

In diesem Artikel wird beschrieben, wie Sie einen Apache Spark-Cluster in Azure HDInsight erstellen und anschließend eine Spark SQL-Abfrage in einer Hive-Tabelle ausführen. Informationen zu Spark in HDInsight finden Sie unter [Übersicht: Apache Spark in Azure HDInsight](hdinsight-apache-spark-overview.md).

   ![Schnellstartdiagramm mit einer Beschreibung der Schritte zum Erstellen eines Apache Spark-Clusters unter Azure HDInsight](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-quickstart-interactive-spark-query-flow.png "Spark-Schnellstart mit Apache Spark in HDInsight. Dargestellte Schritte: Erstellen eines Clusters; Ausführen einer interaktiven Spark-Abfrage")

## <a name="prerequisites"></a>Voraussetzungen

* **Ein Azure-Abonnement**. Für dieses Tutorial wird ein Azure-Abonnement benötigt. Weitere Informationen finden Sie unter [Erstellen Sie noch heute Ihr kostenloses Azure-Konto](https://azure.microsoft.com/free).

## <a name="create-hdinsight-spark-cluster"></a>Erstellen eines HDInsight Spark-Clusters

In diesem Abschnitt erstellen Sie einen HDInsight Spark-Cluster mit einer [Azure Resource Manager-Vorlage](https://azure.microsoft.com/resources/templates/101-hdinsight-spark-linux/). Andere Methoden zur Erstellung von Clustern finden Sie unter [Erstellen von Linux-basierten Hadoop-Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

1. Klicken Sie auf die folgende Abbildung, um die Vorlage im Azure-Portal zu öffnen:         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-spark-linux%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apache-spark-jupyter-spark-sql/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Geben Sie die folgenden Werte ein:

    ![Erstellen eines HDInsight Spark-Clusters mit einer Azure Resource Manager-Vorlage](./media/hdinsight-apache-spark-jupyter-spark-sql/create-spark-cluster-in-hdinsight-using-azure-resource-manager-template.png "Erstellen eines Spark-Clusters in HDInsight mit einer Azure Resource Manager-Vorlage")

    * **Abonnement**: Wählen Sie Ihr Azure-Abonnement für diesen Cluster aus.
    * **Ressourcengruppe**: Erstellen Sie eine Ressourcengruppe, oder wählen Sie eine vorhandene aus. Die Ressourcengruppe wird verwendet, um Azure-Ressourcen für Ihre Projekte zu verwalten.
    * **Standort**: Wählen Sie einen Standort für die Ressourcengruppe aus. Die Vorlage verwendet diesen Standort sowohl für die Erstellung des Clusters als auch für den Standardclusterspeicher.
    * **Clustername**: Geben Sie einen Namen für den HDInsight-Cluster ein, den Sie erstellen möchten.
    * **Spark-Version**: Wählen Sie als Version, die Sie für den Cluster installieren möchten, die Option **2.0** aus.
    * **Cluster-Benutzername und -Kennwort**: Der Standardname für die Anmeldung lautet „admin“.
    * **SSH-Benutzername und -Kennwort**.

   Notieren Sie diese Werte.  Sie werden später im Lernprogramm benötigt.

3. Aktivieren Sie die Optionen **Ich stimme den oben genannten Geschäftsbedingungen zu** und **An Dashboard anheften**, und klicken Sie anschließend auf **Kaufen**. Daraufhin wird eine neue Kachel mit der Bezeichnung „Bereitstellung für Vorlagenbereitstellung wird gesendet“ angezeigt. Das Erstellen des Clusters dauert ca. 20 Minuten.

Sollte bei der HDInsight-Clustererstellung ein Problem auftreten, verfügen Sie unter Umständen nicht über die erforderlichen Berechtigungen. Weitere Informationen finden Sie unter [Voraussetzungen für die Zugriffssteuerung](hdinsight-administer-use-portal-linux.md#create-clusters).

> [!NOTE]
> In diesem Artikel wird ein Spark-Cluster erstellt, der [Azure Storage-Blobs als Clusterspeicher](hdinsight-hadoop-use-blob-storage.md) einsetzt. Sie können auch einen Spark-Cluster erstellen, der [Azure Data Lake Store](hdinsight-hadoop-use-data-lake-store.md) als Standardspeicher verwendet. Anweisungen hierzu finden Sie unter [Erstellen eines HDInsight-Clusters mit Data Lake-Speicher](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).
>
>

## <a name="run-spark-sql-statements-on-a-hive-table"></a>Ausführen von Spark SQL-Anweisungen in einer Hive-Tabelle

SQL (Structured Query Language) ist die gängigste und am häufigsten verwendete Sprache zum Abfragen und Definieren von Daten. Ziel der Gründer von Spark war es, dieses Wissen nutzbar zu machen und die bekannte Sprache für Datenabfragen für eine größere Gruppe von Analysten zu öffnen, die mit Daten in Hadoop Distributed File System (HDFS) arbeiten möchten. Dieses Angebot ist Spark SQL. Es fungiert als Erweiterung von Apache Spark zur Verarbeitung von strukturierten Daten mit der vertrauten SQL-Syntax.

Spark SQL unterstützt sowohl SQL als auch HiveQL als Abfragesprache. Die Funktionen umfassen auch die Einbindung in Python, Scala und Java. Hiermit können Sie Daten an unterschiedlichen Orten abfragen, z.B. externe Datenbanken, strukturierte Datendateien (z.B. JSON) und Hive-Tabellen.

### <a name="running-spark-sql-on-an-hdinsight-cluster"></a>Ausführen von Spark SQL in einem HDInsight-Cluster

Wenn Sie ein für Ihren HDInsight Spark-Cluster konfiguriertes Jupyter Notebook verwenden, erhalten Sie einen vordefinierten `sqlContext`, den Sie zum Ausführen von Hive-Abfragen mit Spark SQL verwenden können. In diesem Abschnitt erfahren Sie, wie Sie ein Jupyter Notebook starten und anschließend eine einfache Spark SQL-Abfrage für eine vorhandene Hive-Tabelle (**hivesampletable**) ausführen, die in allen HDInsight-Clustern verfügbar ist.

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com/).

2. Wenn Sie den Cluster am Dashboard angeheftet haben, klicken Sie im Dashboard auf die Clusterkachel , um das Clusterblatt aufzurufen.

    Wenn Sie den Cluster nicht am Dashboard angeheftet haben, klicken Sie im linken Bereich auf **HDInsight-Cluster** und dann auf den von Ihnen erstellten Cluster.

3. Klicken Sie unter **Quicklinks** auf **Clusterdashboards** und dann auf **Jupyter Notebook**. Geben Sie die Administratoranmeldeinformationen für den Cluster ein, wenn Sie dazu aufgefordert werden.

   ![Öffnen von Jupyter Notebook zum Ausführen einer interaktiven Spark SQL-Abfrage](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "Öffnen von Jupyter Notebook zum Ausführen einer interaktiven Spark SQL-Abfrage")

   > [!NOTE]
   > Sie können auch auf das Jupyter Notebook für Ihren Cluster zugreifen, indem Sie in Ihrem Browser die folgende URL öffnen. Ersetzen Sie **CLUSTERNAME** durch den Namen Ihres Clusters:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. Erstellen Sie ein Notebook. Klicken Sie auf **Neu** und dann auf **PySpark**.

   ![Erstellen eines Jupyter Notebooks zum Ausführen einer interaktiven Spark SQL-Abfrage](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-create-jupyter-interactive-spark-sql-query.png "Erstellen eines Jupyter Notebooks zum Ausführen einer interaktiven Spark SQL-Abfrage")

   Ein neues Notebook mit dem Namen „Untitled“ (Untitled.pynb) wird erstellt und geöffnet.

4. Klicken Sie oben auf den Namen des Notebooks, und geben Sie bei Bedarf einen Anzeigenamen ein.

    ![Eingeben eines Namens für das Jupyter Notebook, aus dem eine interaktive Spark-Abfrage ausgeführt werden soll](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-jupyter-notebook-name.png "Eingeben eines Namens für das Jupyter Notebook, aus dem eine interaktive Spark-Abfrage ausgeführt werden soll")

5.  Fügen Sie den folgenden Code in eine leere Zelle ein, und drücken Sie **UMSCHALT+EINGABE** , um den Code auszuführen. Im folgenden Code wird das Jupyter Notebook mit `%%sql` (SQL Magic) angewiesen, den vordefinierten `sqlContext` für die Ausführung der Hive-Abfrage zu verwenden. Die Abfrage ruft die ersten zehn Zeilen aus einer Hive-Tabelle (**hivesampletable**) ab, die standardmäßig in allen HDInsight-Clustern zur Verfügung steht.

        %%sql
        SELECT * FROM hivesampletable LIMIT 10

    ![Hive-Abfrage in HDInsight Spark](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query.png "Hive-Abfrage in HDInsight Spark")

    Weitere Informationen zu `%%sql` und den vordefinierten Kontexten finden Sie unter [Kernel für Jupyter-Notebook in Spark-Clustern in Azure HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md).

    > [!NOTE]
    > Bei jeder Ausführung einer Abfrage in Jupyter wird auf der Titelleiste Ihres Webbrowserfensters neben dem Notebooktitel der Status **(Beschäftigt)** angezeigt. Außerdem sehen Sie in der rechten oberen Ecke einen ausgefüllten Kreis neben dem Text **PySpark**. Wenn der Auftrag abgeschlossen ist, wird ein Kreis ohne Füllung angezeigt.
    >
    >
    
6. Der Bildschirm wird aktualisiert, um die Ausgabe der Abfrage wird angezeigt.

    ![Hive-Abfrageausgabe in HDInsight Spark](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query-output.png "Hive-Abfrageausgabe in HDInsight Spark")

7. Fahren Sie das Notebook nach dem Ausführen der Anwendung herunter, um die Clusterressourcen freizugeben. Klicken Sie hierzu im Menü **Datei** des Notebooks auf die Option zum **Schließen und Anhalten**.

8. Falls Sie die nächsten Schritte erst zu einem späteren Zeitpunkt ausführen möchten, löschen Sie den in diesem Artikel erstellten HDInsight-Cluster wieder. 

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-step"></a>Nächster Schritt 

In diesem Artikel haben Sie gelernt, wie Sie einen HDInsight Spark-Cluster erstellen und eine einfache Spark SQL-Abfrage ausführen. Im nächsten Artikel erfahren Sie, wie Sie mithilfe eines HDInsight Spark-Clusters interaktive Abfragen für Beispieldaten ausführen.

> [!div class="nextstepaction"]
>[Ausführen interaktiver Abfragen in einem HDInsight Spark-Cluster](hdinsight-apache-spark-load-data-run-query.md)




