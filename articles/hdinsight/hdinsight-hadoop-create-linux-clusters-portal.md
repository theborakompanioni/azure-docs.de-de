---
title: Bereitstellen von Hadoop-, HBase-, Storm- oder Spark-Clustern unter Linux in HDInsight mithilfe des Portals | Microsoft-Dokumentation
description: "Erfahren Sie, wie Hadoop-, HBase-, Storm- oder Spark-Cluster unter Linux für HDInsight mithilfe eines Webbrowsers und des Azure-Vorschauportals erstellt werden."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 697278cf-0032-4f7c-b9b2-a84c4347659e
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/05/2016
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: fe4577516a3cd34cce8fd78e22074065209035ad


---
# <a name="create-linux-based-clusters-in-hdinsight-using-the-azure-portal"></a>Erstellen von Linux-basierten Clustern in HDInsight mithilfe des Azure-Portals
[!INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Das Azure-Portal ist ein webbasiertes Verwaltungstool für Dienste und Ressourcen, die in der Microsoft Azure-Cloud gehostet werden. In diesem Artikel erfahren Sie, wie Sie mit dem Portal Linux-basierte HDInsight-Cluster erstellen.

## <a name="prerequisites"></a>Voraussetzungen
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Ein zeitgemäßer Webbrowser**. Das Azure-Portal verwendet HTML5 und JavaScript und funktioniert in älteren Webbrowsern unter Umständen nicht richtig.

### <a name="access-control-requirements"></a>Voraussetzungen für die Zugriffssteuerung
[!INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-clusters"></a>Erstellen von Clustern
Das Azure-Portal macht die meisten Clustereigenschaften verfügbar. Mithilfe der Azure Resource Manager-Vorlage können Sie viele Details ausblenden. Weitere Informationen finden Sie unter [Erstellen Linux-basierter Hadoop-Cluster in HDInsight mithilfe von Azure Resource Manager-Vorlagen](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie auf **NEU**, auf **Datenanalyse** und anschließend auf **HDInsight**.
   
    ![Erstellen eines neuen Clusters im Azure-Portal](./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.1.png "Creating a new cluster in the Azure portal")
3. Geben Sie den **Clusternamen**ein: Dieser Name muss global eindeutig sein.
4. Klicken Sie auf **Clustertyp auswählen**, und wählen Sie dann aus:
   
   * **Clustertyp**: Wenn Sie nicht sicher sind, welche Angabe hier richtig ist, können Sie **Hadoop** auswählen. Das ist der am häufigsten verwendete Clustertyp.
     
     > [!IMPORTANT]
     > HDInsight-Cluster gibt es in vielen verschiedenen Typen, die der Workload oder Technologie entsprechen, für die der Cluster optimiert ist. Es ist keine unterstützte Methode zum Erstellen eines Clusters vorhanden, bei der mehrere Typen kombiniert werden, z.B. Storm und HBase in einem Cluster. 
     > 
     > 
   * **Betriebssystem**: Wählen Sie **Linux** aus.
   * **Version:**Verwenden Sie die Standardversion, wenn Sie nicht wissen, was Sie auswählen sollen. Weitere Informationen finden Sie unter [HDInsight-Clusterversionen](hdinsight-component-versioning.md).
   * **Clustertarif:**Azure HDInsight bietet die Cloudlösungen für Big Data in zwei Kategorien an: Standard und Premium. Weitere Informationen finden Sie unter [Clustertarife](hdinsight-hadoop-provision-linux-clusters.md#cluster-tiers).
     
     ![Konfiguration des HDInsight Premium-Tarifs](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-cluster-type-configuration.png)
5. Klicken Sie auf **Abonnement** , um das Azure-Abonnement auszuwählen, das für den Cluster verwendet werden soll.
6. Klicken Sie auf **Ressourcengruppe**, um eine vorhandene Ressourcengruppe auszuwählen, oder klicken Sie auf **Neu**, um eine neue Ressourcengruppe zu erstellen.
   
   > [!NOTE]
   > Dieser Eintrag ist standardmäßig auf eine Ihrer vorhandenen Ressourcengruppen festgelegt (sofern verfügbar).
   > 
   > 
7. Klicken Sie auf **Anmeldeinformationen** , und geben Sie ein Kennwort für den Administrator ein. Außerdem müssen Sie einen **SSH-Benutzernamen** und entweder ein **KENNWORT** oder einen **ÖFFENTLICHEN SCHLÜSSEL** zum Authentifizieren des SSH-Benutzers eingeben. Es wird empfohlen, einen öffentlichen Schlüssel zu verwenden. Klicken Sie unten auf **Auswählen** , um die Konfiguration der Anmeldeinformationen zu speichern.
   
    ![Clusteranmeldeinformationen eingeben](./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.3.png "Provide cluster credentials")
   
    Weitere Informationen zur Verwendung von SSH mit HDInsight finden Sie in den folgenden Artikeln:
   
   * [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
   * [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
8. Klicken Sie auf **Datenquelle** , um eine vorhandene Datenquelle für den Cluster auszuwählen, oder erstellen Sie eine neue Datenquelle.
   
    ![Blatt „Datenquelle“](./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.4.png "Provide data source configuration")
   
    Zurzeit kann ein Azure-Speicherkonto als Datenquelle für einen HDInsight-Cluster ausgewählt werden. Die Einträge auf dem Blatt **Datenquelle** werden im Folgenden erläutert.
   
   * **Auswahlmethode**: Wählen Sie **Aus allen Abonnements** aus, damit Speicherkonten aus all Ihren Abonnements durchsucht werden können. Wählen Sie **Zugriffsschlüssel** aus, wenn Sie den **Speichernamen** und **Zugriffsschlüssel** eines vorhandenen Speicherkontos eingeben möchten.
   * **Speicherkonto auswählen/Neu:** Klicken Sie auf **Speicherkonto auswählen**, um ein vorhandenes Speicherkonto auszuwählen, das Sie dem Cluster zuordnen möchten. Alternativ hierzu können Sie auf **Neu** klicken, um ein neues Speicherkonto zu erstellen. Geben Sie den Namen des Speicherkontos in das angezeigte Feld ein. Wenn der Name verfügbar ist, wird ein grünes Häkchen angezeigt.
   * **Standardcontainer auswählen**: Geben Sie den Namen des Standardcontainers für den Cluster ein. Sie können einen beliebigen Namen eingeben, es wird jedoch empfohlen, den Namen des Clusters zu verwenden. So können Sie leichter erkennen, dass es sich um den Container für diesen spezifischen Cluster handelt.
   * **Standort**: Die geografische Region, in der sich das Speicherkonto befindet bzw. in der es erstellt wird.
     
     > [!IMPORTANT]
     > Durch die Auswahl des Standorts für die Standarddatenquelle wird auch der Standort des HDInsight-Clusters festgelegt. Der Cluster und die Standarddatenquelle müssen sich in der gleichen Region befinden.
     > 
     > 
   * **Azure Active Directory-Identität für den Cluster:**Durch die Konfiguration machen Sie den Cluster basierend auf der AAD-Konfiguration für die Azure Data Lake-Speicher zugänglich.
     
     Klicken Sie auf **Auswählen** , um die Datenquellenkonfiguration zu speichern.
9. Klicken Sie auf **Knotenpreistarife** , um Informationen zu den Knoten anzuzeigen, die für diesen Cluster erstellt werden. Legen Sie die Anzahl von Workerknoten fest, die Sie für den Cluster benötigen. Die vorkalkulierten Kosten für den Cluster werden auf dem Blatt angezeigt.
   
    ![Blatt „Knotenpreistarife“](./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.5.png "Specify number of cluster nodes")
   
   > [!IMPORTANT]
   > Wenn Sie mehr als 32 Workerknoten planen, entweder bei Erstellung des Clusters oder durch eine Skalierung des Clusters nach der Erstellung, müssen Sie eine Hauptknotengröße von mindestens 8 Kernen und 14 GB Arbeitsspeicher (RAM) auswählen.
   > 
   > Weitere Informationen zu Knotengrößen und den damit verbundenen Kosten finden Sie unter [HDInsight – Preise](https://azure.microsoft.com/pricing/details/hdinsight/).
   > 
   > 
   
    Klicken Sie auf **Auswählen** , um die Konfiguration der Knotenpreise zu speichern.
10. Klicken Sie auf **Optionale Konfiguration**, um die Clusterversion auszuwählen und andere optionale Einstellungen zu konfigurieren. Sie können den Cluster z.B. einem **virtuellen Netzwerk** hinzufügen, einen **externen Metastore** zum Speichern von Daten für Hive und Oozie einrichten, einen Cluster mithilfe von Skriptaktionen zum Installieren von benutzerdefinierten Komponenten anpassen oder zusätzliche Speicherkonten für einen Cluster angeben.
    
    * **Virtuelles Netzwerk**: Wählen Sie ein virtuelles Azure-Netzwerk und das Subnetz aus, wenn Sie den Cluster in einem virtuellen Netzwerk platzieren möchten.  
      
        ![Blatt „Virtuelles Netzwerk“](./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.6.png "Specify virtual network details")
      
        Informationen zur Verwendung von HDInsight mit Virtual Network, einschließlich spezifischer Konfigurationsanforderungen für Virtual Network, finden Sie unter [Erweitern der HDInsight-Funktionen mit Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md).
    * Klicken Sie auf **Externe Metastores** , um die SQL-Datenbank anzugeben, die zum Speichern von Hive- und Oozie-Metadaten für den Cluster verwendet werden soll.
      
      > [!NOTE]
      > Für HBase-Clustertypen ist keine Metastore-Konfiguration verfügbar.
      > 
      > 
      
        ![Blatt „Benutzerdefinierte Metastores“](./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.7.png "Specify external metastores")
      
        Klicken Sie zum **Verwenden einer vorhandenen SQL-Datenbank für Hive**-Metadaten auf **Ja**, wählen Sie eine SQL-­Datenbank, und geben Sie dann den Benutzernamen und das Kennwort für die Datenbank ein. Wiederholen Sie diese Schritte ggf. für **Vorhandene SQL-Datenbank für Oozie-Metadaten verwenden**. Klicken Sie auf **Auswählen**, bis wieder das Blatt **Optionale Konfiguration** angezeigt wird.
      
      > [!NOTE]
      > Die als Metastore verwendete Azure SQL-Datenbank muss für die Konnektivität mit anderen Azure-Diensten konfiguriert sein, inklusive Azure HDInsight. Klicken Sie im Dashboard der Azure SQL-Datenbank mit der rechten Maustaste auf den Servernamen. Dies ist der Server, auf dem die SQL-Datenbankinstanz läuft. Öffnen Sie die Serveransicht, klicken Sie auf **Konfigurieren**, wählen Sie unter **Azure Services** den Wert **Ja** aus, und klicken Sie auf **Speichern**.
      > 
      > 
      
        &nbsp;
      
      > [!IMPORTANT]
      > Verwenden Sie beim Erstellen eines Metastores keinen Datenbanknamen, der Gedankenstriche oder Bindestriche enthält, da dadurch der Clustererstellungsprozess misslingen kann.
      > 
      > 
    * **Skriptaktionen** , wenn Sie einen Cluster bei seiner Erstellung mit einem benutzerdefinierten Skript anpassen möchten. Weitere Informationen zu Skriptaktionen finden Sie unter [Anpassen von HDInsight-Clustern mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md). Geben Sie auf dem Blatt „Skriptaktionen“ die Details wie im Screenshot dargestellt ein.
      
        ![Blatt „Skriptaktionen“](./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.8.png "Specify script action")
    * Klicken Sie auf **Verknüpfte Speicherkonten** , um dem Cluster weitere Speicherkonten zuzuordnen. Klicken Sie auf dem Blatt **Azure-Speicherschlüssel** auf **Speicherschlüssel hinzufügen**, und wählen Sie ein vorhandenes Speicherkonto aus, oder erstellen Sie ein neues Konto.
      
        ![Blatt „Zusätzlicher Speicher“](./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.9.png "Specify additional storage accounts")
      
        Nach dem Erstellen eines Clusters können Sie auch zusätzliche Speicherkonten hinzufügen.  Weitere Informationen finden Sie unter [Anpassen Linux-basierter HDInsight-Cluster mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md).
      
        Klicken Sie auf **Auswählen**, bis wieder das Blatt **Neuer HDInsight-Cluster** angezeigt wird.
      
        Zusätzlich zum Blob-Speicherkonto können Sie auch Azure Data Lake-Speicher verknüpfen. Sie können die Konfiguration durchführen, indem Sie AAD über die Datenquelle konfigurieren, für die Sie das Standardspeicherkonto und den Standardcontainer konfiguriert haben.
11. Vergewissern Sie sich auf dem Blatt **Neuer HDInsight-Cluster**, dass **An Startmenü anheften** ausgewählt ist, und klicken Sie dann auf **Erstellen**. Der Cluster wird erstellt, und dem Startmenü Ihres Azure-Portals wird eine Kachel für den Cluster hinzugefügt. Das Symbol zeigt an, dass der Cluster bereitgestellt wird. Sobald die Bereitstellung abgeschlossen ist, ändert es sich in das HDInsight-Symbol.
    
    | Während der Bereitstellung | Nach Abschluss der Bereitstellung |
    | --- | --- |
    | ![Bereitstellungsanzeige im Startmenü](./media/hdinsight-hadoop-create-linux-cluster-portal/provisioning.png) |![Kachel für einen bereitgestellten Cluster](./media/hdinsight-hadoop-create-linux-cluster-portal/provisioned.png) |
    
    > [!NOTE]
    > Die Erstellung des Clusters dauert in der Regel ca. 15 Minuten. Sie können den Status des Bereitstellungsprozesses auf der Kachel im Startmenü oder im linken Bereich der Seite unter **Benachrichtigungen** überprüfen.
    > 
    > 
12. Klicken Sie nach Abschluss des Erstellungsprozesses im Startmenü auf die Kachel für den Cluster, um das Clusterblatt zu öffnen. Auf dem Clusterblatt werden grundlegende Informationen zum Cluster angezeigt, z. B. der Name, die zugehörige Ressourcengruppe, der Standort, das Betriebssystem, die URL für das Cluster-Dashboard usw.
    
    ![Clusterblatt](./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.Cluster.Blade.png "Cluster properties")
    
    Im Folgenden werden die Symbole oben auf diesem Blatt und im Abschnitt **Zusammenfassung** erläutert:
    
    * **Einstellungen** und **Alle Einstellungen**: Zeigt das Blatt **Einstellungen** für den Cluster an, über das Sie auf detaillierte Konfigurationsinformationen für den Cluster zugreifen können.
    * **Dashboard**, **Cluster-Dashboard** und **URL**: Über diese Optionen können Sie auf das Cluster-Dashboard zugreifen. Dies ist ein Webportal für die Ausführung von Aufträgen im Cluster.
    * **Secure Shell**: Für den Zugriff auf den Cluster über SSH erforderliche Informationen.
    * **Löscht**: Löscht den HDInsight-Cluster.
    * **Schnellstart** (![cloud and thunderbolt icon = quickstart](./media/hdinsight-hadoop-create-linux-cluster-portal/quickstart.png)): Zeigt hilfreiche Informationen zu den ersten Schritten mit HDInsight an.
    * **Benutzer** (![Symbol „Benutzer“](./media/hdinsight-hadoop-create-linux-cluster-portal/users.png)): Dient zum Festlegen der Berechtigungen für die *Portalverwaltung* dieses Clusters für andere Benutzer in Ihrem Azure-Abonnement.
      
      > [!IMPORTANT]
      > Die hier vorgenommenen Einstellungen betreffen *nur* den Zugriff und die Berechtigungen für diesen Cluster im Azure-Portal und haben keine Auswirkung darauf, wer eine Verbindung mit dem HDInsight-Cluster herstellen oder Aufträge übermitteln kann.
      > 
      > 
    * **Tags** (![Symbol „Tag“](./media/hdinsight-hadoop-create-linux-cluster-portal/tags.png)): Mithilfe von Tags können Sie Schlüssel-Wert-Paare festlegen, um eine benutzerdefinierte Taxonomie für Ihre Clouddienste zu definieren. Sie können z. B. einen Schlüssel mit dem Namen **Projekt** erstellen und dann einen gemeinsamen Wert für alle mit einem bestimmten Projekt verknüpften Dienste verwenden.

## <a name="customize-clusters"></a>Anpassen von Clustern
* Weitere Informationen finden Sie unter [Anpassen von HDInsight-Clustern mithilfe von Bootstrap](hdinsight-hadoop-customize-cluster-bootstrap.md).
* Weitere Informationen finden Sie unter [Anpassen Linux-basierter HDInsight-Cluster mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="delete-the-cluster"></a>Löschen des Clusters
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie einen HDInsight-Cluster erfolgreich erstellt haben, nutzen Sie die folgenden Informationen, um zu erfahren, wie Sie mit Ihrem Cluster arbeiten:

### <a name="hadoop-clusters"></a>Hadoop-Cluster
* [Verwenden von Hive mit HDInsight](hdinsight-use-hive.md)
* [Verwenden von Pig mit HDInsight](hdinsight-use-pig.md)
* [Verwenden von MapReduce mit HDInsight](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>HBase-Cluster
* [Erste Schritte mit HBase in HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Entwickeln von Java-Anwendungen für HBase in HDInsight](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Storm-Cluster
* [Entwickeln von Java-Topologien für Storm in HDInsight](hdinsight-storm-develop-java-topology.md)
* [Verwenden von Python-Komponenten in Storm in HDInsight](hdinsight-storm-develop-python-topology.md)
* [Bereitstellen und Überwachen von Topologien mit Storm in HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="spark-clusters"></a>Spark-Cluster
* [Erstellen einer eigenständigen Anwendung mit Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Remoteausführung von Aufträgen in einem Spark-Cluster mithilfe von Livy](hdinsight-apache-spark-livy-rest-interface.md)
* [Spark mit BI: Durchführen interaktiver Datenanalysen mithilfe von Spark in HDInsight mit BI-Tools](hdinsight-apache-spark-use-bi-tools.md)
* [Spark mit Machine Learning: Vorhersage von Lebensmittelkontrollergebnissen mithilfe von Spark in HDInsight](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark-Streaming: Erstellen von Echtzeit-Streaminganwendungen mithilfe von Spark in HDInsight](hdinsight-apache-spark-eventhub-streaming.md)




<!--HONumber=Nov16_HO3-->


