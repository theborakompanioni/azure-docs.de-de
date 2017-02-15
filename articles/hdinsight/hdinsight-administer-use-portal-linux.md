---
title: Verwalten von Linux-basierten Hadoop-Clustern in HDInsight mit dem Azure-Portal | Microsoft Docs
description: Hier erhalten Sie Informationen zum Erstellen und Verwalten von Linux-basierten HDInsight-Clustern mithilfe des Azure-Portals.
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 5a76f897-02e8-4437-8f2b-4fb12225854a
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2016
ms.author: jgao
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 0ea08d08e058db661b0b13b2002da240e8a2e63f


---
# <a name="manage-hadoop-clusters-in-hdinsight-by-using-the-azure-portal"></a>Verwalten von Hadoop-Clustern in HDInsight mit dem Azure-Portal
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Mithilfe des [Azure-Portals][azure-portal] können Sie Linux-basierte Cluster in Azure HDInsight verwalten. Rufen Sie über die Registerkartenauswahl Informationen zum Erstellen von Hadoop-Clustern in HDInsight mit anderen Tools ab. 

**Voraussetzungen**

Bevor Sie mit diesem Artikel beginnen können, benötigen Sie Folgendes:

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="open-the-portal"></a>Öffnen des Portals
1. Melden Sie sich bei [https://portal.azure.com](https://portal.azure.com)an.
2. Nachdem Sie das Portal geöffnet haben, können Sie die folgenden Schritte ausführen:
   
   * Klicken Sie im linken Menü auf **Neu** , um einen neuen Clusters zu erstellen:
     
       ![Schaltfläche „Neuer HDInsight-Cluster“](./media/hdinsight-administer-use-portal-linux/azure-portal-new-button.png)
   * Klicken Sie im linken Menü auf **HDInsight-Cluster** , um die bestehenden Cluster aufzulisten.
     
       ![Azure-Portal – Schaltfläche „HDInsight-Cluster“](./media/hdinsight-administer-use-portal-linux/azure-portal-hdinsight-button.png)
     
       Wenn **HDInsight** nicht im linken Menü angezeigt wird, klicken Sie auf **Durchsuchen** und anschließend auf **HDInsight-Cluster**.
     
       ![Azure-Portal – Schaltfläche „Cluster durchsuchen“](./media/hdinsight-administer-use-portal-linux/azure-portal-browse-button.png)

## <a name="create-clusters"></a>Erstellen von Clustern
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

In HDInsight kann eine Vielzahl von Hadoop-Komponenten verwendet werden. Eine Liste der überprüften und unterstützten Komponenten finden Sie unter [Neuheiten in den von HDInsight bereitgestellten Clusterversionen](hdinsight-component-versioning.md). Weitere Informationen finden Sie unter [Erstellen von Linux-basierten Hadoop-Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md). 

## <a name="list-and-show-clusters"></a>Auflisten und Anzeigen von Clustern
1. Melden Sie sich bei [https://portal.azure.com](https://portal.azure.com)an.
2. Klicken Sie im linken Menü auf **HDInsight-Cluster** , um die vorhandenen Cluster aufzulisten.
3. Klicken Sie auf den Clusternamen. Wenn die Clusterliste lang ist, können Sie oben auf der Seite filtern.
4. Doppelklicken Sie auf einen Cluster aus der Liste, um Details anzuzeigen.
   
    **Menü und Zusammenfassung**:
   
    ![Azure-Portal – HDInsight-Cluster-Zusammenfassung](./media/hdinsight-administer-use-portal-linux/hdinsight-essentials.png)
   
   * **Einstellungen** und **Alle Einstellungen**: Zeigt das Blatt **Einstellungen** für den Cluster an, über das Sie auf detaillierte Konfigurationsinformationen für den Cluster zugreifen können.
   * **Dashboard**, **Cluster-Dashboard** und **URL: Über diese Optionen können Sie auf das Cluster-Dashboard zugreifen (Ambari Web für Linux-basierte Cluster).
   * **Secure Shell**: Zeigt die Anweisungen für die Verbindung mit dem Cluster über eine Secure Shell-Verbindung (SSH) an.
   * **Cluster skalieren**: Dient zum Ändern der Anzahl von Workerknoten für den Cluster.
   * **Löschen**: Löscht den Cluster.
   * **Schnellstart (![Wolken- und Blitzsymbol = Schnellstart](./media/hdinsight-administer-use-portal-linux/quickstart.png))**: Zeigt hilfreiche Informationen zu den ersten Schritten mit HDInsight an.
   * **Benutzer (![Benutzersymbol](./media/hdinsight-administer-use-portal-linux/users.png))**: Dient zum Festlegen der Berechtigungen für die *Portalverwaltung* dieses Clusters für andere Benutzer in Ihrem Azure-Abonnement.
     
     > [!IMPORTANT]
     > Die hier vorgenommenen Einstellungen betreffen *nur* den Zugriff und die Berechtigungen für diesen Cluster im Azure-Portal und haben keine Auswirkung darauf, wer eine Verbindung mit dem HDInsight-Cluster herstellen oder Aufträge übermitteln kann.
     > 
     > 
   * **Tags (![Tag-Symbol](./media/hdinsight-administer-use-portal-linux/tags.png))**: Mithilfe von Tags können Sie Schlüssel-Wert-Paare festlegen, um eine benutzerdefinierte Taxonomie für Ihre Clouddienste zu definieren. Sie können z. B. einen Schlüssel mit dem Namen **Projekt** erstellen und dann einen gemeinsamen Wert für alle mit einem bestimmten Projekt verknüpften Dienste verwenden.
   * **Ambari-Ansichten**: Verknüpfung mit Ambari Web.
     
     > [!IMPORTANT]
     > Um die vom HDInsight-Cluster bereitgestellten Dienste zu verwalten, müssen Sie Ambari Web oder die Ambari-REST-API verwenden. Weitere Informationen zur Verwendung von Ambari finden Sie unter [Verwalten von HDInsight-Clustern mit Ambari](hdinsight-hadoop-manage-ambari.md).
     > 
     > 
     
     **Verwendung**:
     
     ![Azure-Portal – HDInsight-Clusternutzung](./media/hdinsight-administer-use-portal-linux/hdinsight-portal-cluster-usage.png)
5. Klicken Sie auf **Einstellungen**.
   
    ![Azure-Portal – HDInsight-Clusternutzung](./media/hdinsight-administer-use-portal-linux/hdinsight.portal.cluster.settings.png)
   
   * **Überwachungsprotokolle**:
   * **Schnellstart**: Zeigt hilfreiche Informationen für die ersten Schritte mit HDInsight an.
   * **Cluster skalieren**: Erhöhen und Verringern der Anzahl der Cluster-Workerknoten
   * **Secure Shell**: Zeigt die Anweisungen für die Verbindung mit dem Cluster über eine Secure Shell-Verbindung (SSH) an.
   * **HDInsight-Partner**: Hinzufügen/Entfernen des aktuellen HDInsight-Partners.
   * **Externe Metastores**: Anzeigen der Hive- und Oozie-Metastores. Die Metastores können nur während der Clustererstellung konfiguriert werden.
   * **Skriptaktionen**: Anwenden von Bashskripts auf den Cluster.
   * **Eigenschaften**: Anzeigen der Clustereigenschaften
   * **Azure-Speicherschlüssel**: Anzeigen des Standardspeicherkontos und des dazugehörigen Schlüssels. Das Speicherkonto wird während der Clustererstellung konfiguriert.
   * **Cluster-AAD-Identität**: 
   * **Benutzer**: Dient zum Festlegen der Berechtigungen für die *Portalverwaltung* dieses Clusters für andere Benutzer in Ihrem Azure-Abonnement.
   * **Tags**: Mithilfe von Tags können Sie Schlüssel-Wert-Paare festlegen, um eine benutzerdefinierte Taxonomie für Ihre Clouddienste zu definieren. Sie können z. B. einen Schlüssel mit dem Namen **Projekt** erstellen und dann einen gemeinsamen Wert für alle mit einem bestimmten Projekt verknüpften Dienste verwenden.
     
     > [!NOTE]
     > Dies ist eine generische Liste der verfügbaren Einstellungen – nicht alle stehen für alle Cluster zur Verfügung.
     > 
     > 
6. Klicken Sie auf **Eigenschaften**.
   
    Die Eigenschaften sind:
   
   * **Hostname**: Der Clustername
   * **Cluster-URL**
   * **Status**: Umfasst Aborted, Accepted, ClusterStorageProvisioned, AzureVMConfiguration, HDInsightConfiguration, Operational, Running, Error, Deleting, Deleted, Timedout, DeleteQueued, DeleteTimedout, DeleteError, PatchQueued, CertRolloverQueued, ResizeQueued, ClusterCustomization
   * **Region**: Der Azure-Standort. Eine Liste der unterstützten Azure-Standorte, finden Sie im Dropdown-Listenfeld **Region** unter [HDInsight Preise](https://azure.microsoft.com/pricing/details/hdinsight/).
   * **Erstellte Daten**
   * **Betriebssystem**: Entweder **Windows** oder **Linux**
   * **Typ**: Hadoop, HBase, Storm, Spark 
   * **Version**. Siehe [HDInsight-Versionen](hdinsight-component-versioning.md)
   * **Abonnement**: Name des Abonnements.
   * **Abonnement-ID**
   * **Standarddatenquelle**: Das Standardcluster-Dateisystem.
   * **Workerknoten – Tarif**
   * **Hauptknoten – Tarif**

## <a name="delete-clusters"></a>Löschen von Clustern
Wenn Sie einen Cluster löschen, werden das Standardspeicherkonto oder die verknüpften Speicherkonten dadurch nicht gelöscht. Sie können den Cluster mit den gleichen Speicherkonten und den gleichen Metastores neu erstellen. Sie sollten einen neuen Standardblobcontainer verwenden, wenn Sie den Cluster neu erstellen.

1. Melden Sie sich beim [Portal][azure-portal] an.
2. Klicken Sie im linken Menü auf **Alle durchsuchen**, klicken Sie auf **HDInsight-Cluster** und anschließend auf den Clusternamen.
3. Klicken Sie im oberen Menü auf **Löschen** , und befolgen Sie dann die Anweisungen.

Siehe auch [Anhalten/Herunterfahren von Clustern](#pauseshut-down-clusters).

## <a name="scale-clusters"></a>Skalieren von Clustern
Mithilfe der Clusterskalierung können Sie die Anzahl der von einem in Azure HDInsight ausgeführten Cluster verwendeten Workerknoten ändern, ohne den Cluster neu erstellen zu müssen.

> [!NOTE]
> Es werden nur Cluster mit HDInsight-Versionen ab 3.1.3 unterstützt. Überprüfen Sie ggf. auf der Seite „Eigenschaften“ die Version Ihres Clusters.  Informationen hierzu finden Sie unter [Auflisten und Anzeigen von Clustern](#list-and-show-clusters).
> 
> 

Auswirkungen der Änderung der Anzahl von Datenknoten für die von HDInsight unterstützten Clustertypen:

* Hadoop
  
    Sie können die Anzahl der Workerknoten in einem aktiven Hadoop-Cluster problemlos ohne Auswirkungen auf ausstehende oder aktive Aufträge erhöhen. Neue Aufträge können auch während des Vorgangs gesendet werden. Fehler bei einer Skalierung werden ordnungsgemäß behandelt, sodass der Cluster immer in einem funktionsfähigen Zustand verbleibt.
  
    Wenn ein Hadoop-Cluster durch Verringern der Anzahl der Datenknoten zentral herunterskaliert wird, werden einige der Dienste im Cluster neu gestartet. Dies führt beim Abschluss des Skalierungsvorgangs bei allen aktiven und ausstehenden Aufträgen zu einem Fehler. Sie können die Aufträge jedoch nach Abschluss des Vorgangs erneut senden.
* HBase
  
    Sie können Knoten reibungslos Ihrem HBase-Cluster hinzufügen oder aus diesem entfernen, während er aktiv ist. Regionale Server werden innerhalb weniger Minuten nach Abschluss des Skalierungsvorgangs automatisch ausgeglichen. Allerdings können Sie die regionalen Server auch manuell ausgleichen, indem Sie sich am Hauptknoten des Clusters anmelden und in einem Eingabeaufforderungsfenster die folgenden Befehle ausführen:
  
        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer
  
    Weitere Informationen zur Verwendung der HBase-Shell finden Sie unter []
* Storm
  
    Sie können Datenknoten übergangslos zum Storm-Cluster hinzufügen oder aus diesem entfernen, während er aktiv ist. Nach erfolgreichen Abschluss des Skalierungsvorgangs müssen Sie die Topologie neu ausgleichen.
  
    Es stehen zwei Methoden für den erneuten Ausgleich zur Verfügung:
  
  * Storm-Webbenutzeroberfläche
  * Befehlszeilenschnittstelle (CLI)
    
    Weitere Informationen finden Sie in der Dokumentation zu [Apache Storm](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) .
    
    Die Storm-Webbenutzeroberfläche ist für den HDInsight-Cluster verfügbar:
    
    ![HDInsight Storm-Skalierung ausgleichen](./media/hdinsight-administer-use-portal-linux/hdinsight.portal.scale.cluster.storm.rebalance.png)
    
    Es folgt ein Beispiel, wie die Storm-Topologie mithilfe des CLI-Befehls neu ausgeglichen werden kann:
    
    ## <a name="reconfigure-the-topology-mytopology-to-use-5-worker-processes"></a>Konfigurieren Sie die Topologie „mytopology“ so neu, dass sie fünf Workerprozesse verwendet,
    ## <a name="the-spout-blue-spout-to-use-3-executors-and"></a>das Spout „blue-spout“ so, dass es drei Executors verwendet, und
    ## <a name="the-bolt-yellow-bolt-to-use-10-executors"></a>das Bolt „yellow-bolt“ so, dass es zehn Executors verwendet.
      $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

**So skalieren Sie Cluster**

1. Melden Sie sich beim [Portal][azure-portal] an.
2. Klicken Sie im linken Menü auf **Alle durchsuchen**, klicken Sie auf **HDInsight-Cluster** und anschließend auf den Clusternamen.
3. Klicken Sie im oberen Menü auf **Einstellungen** und anschließend auf **Cluster skalieren**.
4. Geben Sie die **Anzahl von Workerknoten**ein. Die maximale Anzahl von Clusterknoten variiert abhängig vom Azure-Abonnement. Sie können sich an den Azure-Abrechnungssupport wenden, falls Sie diese Begrenzung erhöhen möchten.  Die Änderungen, die Sie an der Anzahl von Knoten vorgenommen haben, werden in den Kosteninformationen widergespiegelt.
   
    ![HDInsight Hadoop Hbase Storm Spark skalieren](./media/hdinsight-administer-use-portal-linux/hdinsight.portal.scale.cluster.png)

## <a name="pauseshut-down-clusters"></a>Anhalten/Herunterfahren von Clustern
Die meisten Hadoop-Aufträge sind Batch-Aufträge, die nur gelegentlich ausgeführt werden. Bei den meisten Hadoop-Clustern wird der Cluster immer wieder für einen längeren Zeitraum nicht zur Verarbeitung verwendet. Mit HDInsight werden Ihre Daten im Azure-Speicher gespeichert, sodass Sie einen Cluster problemlos löschen können, wenn er nicht verwendet wird.
Für einen HDInsight-Cluster fallen auch dann Gebühren an, wenn er nicht verwendet wird. Da die Gebühren für den Cluster erheblich höher sind als die Kosten für den Speicher, ist es sinnvoll, nicht verwendete Cluster zu löschen.

Es gibt viele Methoden, mit denen Sie den Prozess programmieren können:

* Verwenden Sie Azure Data Factory. Siehe [Erstellen von bedarfsgesteuerten Linux-basierten Hadoop-Clustern in HDInsight mit Azure Data Factory](hdinsight-hadoop-create-linux-clusters-adf.md) zum Erstellen bedarfsgesteuerter verknüpfter HDInsight-Dienste.
* Verwenden Sie Azure PowerShell.  Weitere Informationen hierzu finden Sie unter [Analysieren von Flugverspätungsdaten mit Hive in HDInsight](hdinsight-analyze-flight-delay-data.md).
* Verwenden Sie die Azure-Befehlszeilenschnittstelle. Weitere Informationen hierzu finden Sie unter [Verwalten von Hadoop-Clustern in HDInsight mit der Azure-Befehlszeilenschnittstelle](hdinsight-administer-use-command-line.md).
* Verwenden das Sie HDInsight .NET-SDK. Weitere Informationen hierzu finden Sie unter [Übermitteln von Hadoop-Aufträgen in HDInsight](hdinsight-submit-hadoop-jobs-programmatically.md).

Die Preisinformationen finden Sie unter [HDInsight Preise](https://azure.microsoft.com/pricing/details/hdinsight/). Informationen zum Löschen eines Clusters aus dem Portal finden Sie unter [Löschen von Clustern](#delete-clusters)

## <a name="change-passwords"></a>Ändern von Kennwörtern
Ein HDInsight-Cluster kann über zwei Benutzerkonten verfügen. Das HDInsight-Clusterbenutzerkonto (auch HTTP-Benutzerkonto genannt) und das SSH-Benutzerkonto werden während des Erstellungsprozesses erstellt. Sie können die Ambari-Webbenutzeroberfläche verwenden, um Benutzername und Kennwort für das Clusterbenutzerkonto zu ändern, und Skriptaktionen, um das SSH-Benutzerkonto zu ändern.

### <a name="change-the-cluster-user-password"></a>Ändern des Clusterbenutzerkennworts
Sie können die Ambari-Webbenutzeroberfläche verwenden, um das Clusterbenutzerkennwort zu ändern. Zum Anmelden bei Ambari müssen Sie den vorhandenen Benutzernamen und das vorhandene Kennwort verwenden.

> [!NOTE]
> Wenn Sie das Kennwort für den Clusterbenutzer (Admin) ändern, können Skriptaktionen, die für diesen Cluster ausgeführt werden, möglicherweise fehlschlagen. Wenn Sie Skriptaktionen beibehalten möchten, deren Ziel Workerknoten sind, können sie fehlschlagen, wenn Sie dem Cluster über eine Größenänderung Knoten hinzufügen. Weitere Informationen zu Skriptaktionen finden Sie unter [Anpassen von HDInsight-Clustern mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md).
> 
> 

1. Melden Sie sich bei der Ambari-Webbenutzeroberfläche mit den HDInsight-Clusterbenutzer-Anmeldeinformationen an. Der Standard-Benutzername lautet **admin**. Die URL lautet **https://&lt;HDInsight-Clustername>azurehdinsight.net**.
2. Klicken Sie im oberen Menü auf **Administrator** und anschließend auf „Ambari verwalten“. 
3. Klicken Sie im linken Menü auf **Benutzer**.
4. Klicken Sie auf **Administrator**.
5. Klicken Sie auf **Kennwort ändern**.

Ambari ändert anschließend das Kennwort auf allen Knoten im Cluster.

### <a name="change-the-ssh-user-password"></a>Ändern des SSH-Benutzerkennworts
1. Speichern Sie mit einem Texteditor den folgenden Text als Datei namens **changepassword.sh**.
   
   > [!IMPORTANT]
   > Sie müssen einen Editor verwenden, der „LF“ als Zeilenende verwendet. Wenn der Editor „CRLF“ verwendet, funktioniert das Skript nicht.
   > 
   > 
   
        #! /bin/bash
        USER=$1
        PASS=$2
   
        usermod --password $(echo $PASS | openssl passwd -1 -stdin) $USER
2. Laden Sie die Datei an einen Speicherort hoch, auf den von HDInsight über eine HTTP- oder HTTPS-Adresse zugegriffen werden kann. Verwenden Sie z.B. einen öffentlichen Dateispeicher wie OneDrive oder Azure Blob Storage. Speichern Sie den URI (HTTP- oder HTTPS-Adresse) in der Datei, da dieser im nächsten Schritt benötigt wird.
3. Wählen Sie im Azure-Portal den HDInsight-Cluster und dann **Alle Einstellungen** aus. Wählen Sie auf dem Blatt **Einstellungen** die Option **Skriptaktionen** aus.
4. Wählen Sie auf dem Blatt **Skriptaktionen** die Option **Neue übermitteln** aus. Geben Sie auf dem Blatt **Skriptaktion übermitteln** die folgenden Informationen ein.
   
   | Feld | Wert |
   | --- | --- |
   | Name |Ändern des SSH-Kennworts |
   | Bash-Skript-URI |Der URI zur Datei „changepassword.sh“ |
   | Knoten (Hauptknoten, Worker, Nimbus, Supervisor, Zookeeper usw.) |✓ für alle aufgeführten Knotentypen |
   | Parameter |Geben Sie den SSH-Benutzernamen und dann das neue Kennwort ein. Es muss ein Leerzeichen zwischen den Benutzernamen und das Kennwort eingefügt werden. |
   | Speichern Sie diese Skriptaktion ... |Lassen Sie dieses Feld deaktiviert. |
5. Wählen Sie **Erstellen** aus, um das Skript anzuwenden. Nachdem das Skript ausgeführt wurde, können Sie per SSH mit dem neuen Kennwort eine Verbindung mit dem Cluster herstellen.

## <a name="grantrevoke-access"></a>Gewähren/Entziehen des Zugriffs
In HDInsight-Clustern stehen die folgenden HTTP-Webdienste zur Verfügung (alle mit RESTful-Endpunkten):

* ODBC
* JDBC
* Ambari
* Oozie
* Templeton

Der Zugriff auf diese Dienste wird standardmäßig gewährt. Sie können den Zugriff mit der [Azure-CLI](hdinsight-administer-use-command-line.md#enabledisable-http-access-for-a-cluster) und [Azure PowerShell](hdinsight-administer-use-powershell.md#grantrevoke-access) widerrufen/gewähren.

## <a name="find-the-subscription-id"></a>Ermitteln der Abonnement-ID
**So ermitteln Sie Ihre Azure-Abonnement-IDs**

1. Melden Sie sich beim [Portal][azure-portal] an.
2. Klicken Sie im linken Menü auf **Alle durchsuchen**, und klicken Sie dann auf **Abonnements**. Jedes Abonnement verfügt über einen Namen und eine ID.

Jeder Cluster ist mit einem Azure-Abonnement verknüpft. Die Abonnement-ID wird auf der Clusterkachel **Zusammenfassung** angezeigt. Informationen hierzu finden Sie unter [Auflisten und Anzeigen von Clustern](#list-and-show-clusters).

## <a name="find-the-resource-group"></a>Suchen der Ressourcengruppe
Im ARM-Modus wird jeder HDInsight-Cluster mit einer Azure-Ressourcengruppe erstellt. Die Azure-Ressourcengruppe, zu der ein Cluster gehört, wird hier angezeigt:

* Die Liste der Cluster verfügt über eine Spalte **Ressourcengruppe** .
* Die Kachel **Zusammenfassung** des Clusters.  

Informationen hierzu finden Sie unter [Auflisten und Anzeigen von Clustern](#list-and-show-clusters).

## <a name="find-the-default-storage-account"></a>Suchen des Standardspeicherkontos
Jeder HDInsight-Cluster verfügt über eine Standardspeicherkonto. Das Standardspeicherkonto und die zugehörigen Schlüssel für einen Cluster werden unter **Einstellungen**/**Eigenschaften**/**Azure-Speicherschlüssel**. Informationen hierzu finden Sie unter [Auflisten und Anzeigen von Clustern](#list-and-show-clusters).

## <a name="run-hive-queries"></a>Ausführen von Hive-Abfragen
Hive-Aufträge können Sie nicht direkt vom Azure-Portal aus ausführen, aber Sie können die Hive-Ansicht auf der Ambari-Webbenutzeroberfläche verwenden.

**So führen Sie Hive-Abfragen mithilfe der Ambari-Hive-Ansicht aus**

1. Melden Sie sich bei der Ambari-Webbenutzeroberfläche mit den HDInsight-Clusterbenutzer-Anmeldeinformationen an. Der Standardbenutzername lautet **admin**. Die URL lautet **https://&lt;HDInsight-Clustername>azurehdinsight.net**.
2. Öffnen Sie die Hive-Ansicht wie im folgenden Screenshot dargestellt:  
   
    ![HDInsight-Hive-Ansicht](./media/hdinsight-administer-use-portal-linux/hdinsight-hive-view.png)
3. Klicken Sie im Menü oben auf **Abfrage** .
4. Geben Sie eine Hive-Abfrage im **Abfrage-Editor** ein, und klicken Sie dann auf **Ausführen**.

## <a name="monitor-jobs"></a>Überwachen von Aufträgen
Siehe [Verwalten von HDInsight-Clustern mithilfe der Ambari-Webbenutzeroberfläche](hdinsight-hadoop-manage-ambari.md#monitoring).

## <a name="browse-files"></a>Dateien durchsuchen
Mit dem Azure-Portal können Sie den Inhalt des Standardcontainers durchsuchen.

1. Melden Sie sich bei [https://portal.azure.com](https://portal.azure.com)an.
2. Klicken Sie im linken Menü auf **HDInsight-Cluster** , um die vorhandenen Cluster aufzulisten.
3. Klicken Sie auf den Clusternamen. Wenn die Clusterliste lang ist, können Sie oben auf der Seite filtern.
4. Klicken Sie auf **Einstellungen**.
5. Klicken Sie auf dem Blatt **Einstellungen** auf **Azure-Speicherschlüssel**.
6. Klicken Sie auf den Standardspeicherkonto-Namen.
7. Klicken Sie auf die Kachel **Blobs** .
8. Klicken Sie auf den Standardcontainernamen.

## <a name="monitor-cluster-usage"></a>Überwachen der Clusternutzung
Der Abschnitt **Nutzung** auf dem Blatt für den HDInsight-Cluster enthält Informationen zur Anzahl von Kernen, die für Ihr Abonnement zur Verwendung mit HDInsight verfügbar sind. Außerdem ist hier die Anzahl von Kernen angegeben, die diesem Cluster zugeordnet sind. Weiterhin wird angezeigt, wie die Kerne für die Knoten innerhalb dieses Clusters zugeordnet sind. Informationen hierzu finden Sie unter [Auflisten und Anzeigen von Clustern](#list-and-show-clusters).

> [!IMPORTANT]
> Um die vom HDInsight-Cluster bereitgestellten Dienste zu überwachen, müssen Sie Ambari Web oder die Ambari-REST-API verwenden. Weitere Informationen zur Verwendung von Ambari finden Sie unter [Verwalten von HDInsight-Clustern mit Ambari](hdinsight-hadoop-manage-ambari.md)
> 
> 

## <a name="connect-to-a-cluster"></a>Herstellen der Verbindung zu einem Cluster
Siehe [Verwenden von Hive mit Hadoop in HDInsight über SSH](hdinsight-hadoop-use-hive-ssh.md#ssh).

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie erfahren, wie ein HDInsight-Cluster mit dem Azure-Portal erstellt und wie das Hadoop-Befehlszeilentool geöffnet wird. Weitere Informationen finden Sie in den folgenden Artikeln:

* [Verwalten von HDInsight mit Azure PowerShell](hdinsight-administer-use-powershell.md)
* [Verwalten von HDInsight mit der Azure-CLI](hdinsight-administer-use-command-line.md)
* [Erstellen von Hadoop-Clustern in HDInsight](hdinsight-provision-clusters.md)
* [Verwenden von Hive in HDInsight](hdinsight-use-hive.md)
* [Verwenden von Pig in HDInsight](hdinsight-use-pig.md)
* [Verwenden von Sqoop in HDInsight](hdinsight-use-sqoop.md)
* [Erste Schritte mit Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
* [Welche Hadoop-Version wird in Azure HDInsight verwendet?](hdinsight-component-versioning.md)

[azure-portal]: https://portal.azure.com
[image-hadoopcommandline]: ./media/hdinsight-administer-use-portal-linux/hdinsight-hadoop-command-line.png "Hadoop-Befehlszeile"



<!--HONumber=Nov16_HO3-->


