---
title: Verwalten von Hadoop-Clustern in HDInsight mit dem Azure-Portal | Microsoft-Dokumentation
description: Hier erhalten Sie Informationen zum Erstellen und Verwalten von HDInsight-Clustern mithilfe des Azure-Portals.
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 5a76f897-02e8-4437-8f2b-4fb12225854a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: jgao
ms.translationtype: HT
ms.sourcegitcommit: 368589509b163cacf495fd0be893a8953fe2066e
ms.openlocfilehash: 72c02eac9d627ad642d3e66492c314a2276e9c0a
ms.contentlocale: de-de
ms.lasthandoff: 08/17/2017

---
# <a name="manage-hadoop-clusters-in-hdinsight-by-using-the-azure-portal"></a>Verwalten von Hadoop-Clustern in HDInsight mit dem Azure-Portal
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Mithilfe des [Azure-Portals][azure-portal] können Sie Hadoop-Cluster in Azure HDInsight verwalten. Rufen Sie über die Registerkartenauswahl Informationen zum Verwalten von Hadoop-Clustern in HDInsight mit anderen Tools ab.

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

       Wenn Sie den HDInsight-Cluster nicht sehen, klicken Sie unten in der Liste auf **Weitere Dienste** und anschließend im Abschnitt **Daten und Analyse** auf **HDInsight-Cluster**.


## <a name="create-clusters"></a>Erstellen von Clustern
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

In HDInsight kann eine Vielzahl von Hadoop-Komponenten verwendet werden. Eine Liste der überprüften und unterstützten Komponenten finden Sie unter [Neuheiten in den von HDInsight bereitgestellten Clusterversionen](hdinsight-component-versioning.md). Weitere Informationen finden Sie unter [Erstellen von Linux-basierten Hadoop-Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

### <a name="access-control-requirements"></a>Voraussetzungen für die Zugriffssteuerung

Wenn Sie einen HDInsight-Cluster erstellen, müssen Sie ein Azure-Abonnement angeben. Dieser Cluster kann in einer neuen Azure-Ressourcengruppe oder in einer vorhandenen Ressourcengruppe erstellt werden. Sie können die folgenden Schritte ausführen, um Ihre Berechtigungen zum Erstellen von HDInsight-Clustern zu überprüfen:

- So verwenden Sie eine vorhandene Ressourcengruppe

    1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
    2. Klicken Sie im linken Menü auf **Ressourcengruppen**, um eine Liste der Ressourcengruppen anzuzeigen.
    3. Klicken Sie auf die Ressourcengruppe, die Sie zum Erstellen Ihres HDInsight-Clusters verwenden möchten.
    4. Klicken Sie auf **Zugriffssteuerung (IAM)**, und vergewissern Sie sich, dass Sie oder eine Gruppe, der Sie angehören, mindestens über den Zugriffstyp „Mitwirkender“ für die Ressourcengruppe verfügen/verfügt.

- So erstellen Sie eine neue Ressourcengruppe

    1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
    2. Klicken Sie im linken Menü auf **Abonnement**. Es weist ein gelbes Schlüsselsymbol auf. Daraufhin sollte eine Liste der Abonnements angezeigt werden.
    3. Klicken Sie auf das Abonnement, das Sie zum Erstellen von Clustern verwenden. 
    4. Klicken Sie auf **Meine Berechtigungen**.  Ihre [Rolle](../active-directory/role-based-access-control-what-is.md#built-in-roles) für das Abonnement wird angezeigt. Um HDInsight-Cluster erstellen zu können, müssen Sie mindestens über den Zugriffstyp „Mitwirkender“ verfügen.

Wenn der Fehler „NoRegisteredProviderFound“ oder „MissingSubscriptionRegistration“ angezeigt wird, lesen Sie [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](../azure-resource-manager/resource-manager-common-deployment-errors.md).

## <a name="list-and-show-clusters"></a>Auflisten und Anzeigen von Clustern
1. Melden Sie sich bei [https://portal.azure.com](https://portal.azure.com)an.
2. Klicken Sie im linken Menü auf **HDInsight-Cluster** , um die vorhandenen Cluster aufzulisten.
3. Klicken Sie auf den Clusternamen. Wenn die Clusterliste lang ist, können Sie oben auf der Seite filtern.
4. Klicken Sie in der Liste auf einen Cluster, um die Übersichtsseite anzuzeigen:

    ![Azure-Portal – HDInsight-Cluster-Zusammenfassung](./media/hdinsight-administer-use-portal-linux/hdinsight-essentials.png)

    * **Dashboard**: Öffnet das Clusterdashboard (für Linux-basierte Cluster: Ambari Web).
    * **Secure Shell**: Zeigt die Anweisungen für die Verbindung mit dem Cluster über eine Secure Shell-Verbindung (SSH) an.
    * **Cluster skalieren**: Dient zum Ändern der Anzahl von Workerknoten für den Cluster.
    * **Löschen**: Löscht den Cluster.
    * **Aktivitätsprotokoll**: Dient zum Anzeigen und Abfragen von Aktivitätsprotokollen.
    * **Zugriffssteuerung (IAM)**: Dient zum Verwenden von Rollenzuweisungen.  Weitere Informationen finden Sie unter [Verwenden von Rollenzuweisungen zum Verwalten Ihrer Azure-Abonnementressourcen](../active-directory/role-based-access-control-configure.md).
    * **Tags**: Dient zum Festlegen von Schlüssel-Wert-Paaren, um eine benutzerdefinierte Taxonomie für Ihre Clouddienste zu definieren. Sie können z. B. einen Schlüssel mit dem Namen **Projekt** erstellen und dann einen gemeinsamen Wert für alle mit einem bestimmten Projekt verknüpften Dienste verwenden.
    * **Diagnose und Problembehandlung**: Dient zum Anzeigen von Informationen zur Problembehandlung.
    * **Sperren**: Dient zum Hinzufügen einer Sperre, um zu verhindern, dass der Cluster geändert oder gelöscht wird.
    * **Automatisierungsskript**: Dient zum Anzeigen und Exportieren der Azure Resource Manager-Vorlage für den Cluster. Derzeit können Sie nur das abhängige Azure-Speicherkonto exportieren. Weitere Informationen finden Sie unter [Erstellen Linux-basierter Hadoop-Cluster in HDInsight mithilfe von Azure Resource Manager-Vorlagen](hdinsight-hadoop-create-linux-clusters-arm-templates.md).
    * **Schnellstart**: Zeigt hilfreiche Informationen für die ersten Schritte mit HDInsight an.
    * **Tools für HDInsight**: Hilfeinformationen für HDInsight-bezogene Tools.
    * **Clusteranmeldung**: Dient zum Anzeigen von Informationen zur Clusteranmeldung.
    * **Subscription Core Usage** (Nutzung des Abonnementkernspeichers): Dient zum Anzeigen der verwendeten und verfügbaren Kernspeicher für Ihr Abonnement.
    * **Cluster skalieren**: Erhöhen und Verringern der Anzahl der Cluster-Workerknoten Weitere Informationen finden Sie unter [Skalieren von Clustern](hdinsight-administer-use-management-portal.md#scale-clusters).
    * **Secure Shell**: Zeigt die Anweisungen für die Verbindung mit dem Cluster über eine Secure Shell-Verbindung (SSH) an. Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md).
    * **HDInsight-Partner**: Hinzufügen/Entfernen des aktuellen HDInsight-Partners.
    * **Externe Metastores**: Anzeigen der Hive- und Oozie-Metastores. Die Metastores können nur während der Clustererstellung konfiguriert werden. Weitere Informationen finden Sie unter [Verwenden des Hive/Oozie-Metastores](hdinsight-hadoop-provision-linux-clusters.md#use-hiveoozie-metastore).
    * **Skriptaktionen**: Anwenden von Bashskripts auf den Cluster. Weitere Informationen finden Sie unter [Anpassen Linux-basierter HDInsight-Cluster mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md).
    * **Anwendungen**: Dient zum Hinzufügen und Entfernen von HDInsight-Anwendungen.  Weitere Informationen finden Sie unter [Installieren benutzerdefinierter HDInsight-Anwendungen](hdinsight-apps-install-custom-applications.md).
    * **Eigenschaften**: Anzeigen der Clustereigenschaften
    * **Speicherkonten**: Dient zum Anzeigen der Speicherkonten und Schlüssel. Die Speicherkonten werden während der Clustererstellung konfiguriert.
    * **Cluster-AAD-Identität**:
    * **Neue Supportanfrage**: Ermöglicht die Erstellung eines Supporttickets für den Microsoft-Support.
    
6. Klicken Sie auf **Eigenschaften**.

    Die Eigenschaften sind:

   * **Hostname**: Der Clustername
   * **Cluster-URL** Dies ist die URL für die Ambari-Weboberfläche.
   * **Status**: Umfasst Aborted, Accepted, ClusterStorageProvisioned, AzureVMConfiguration, HDInsightConfiguration, Operational, Running, Error, Deleting, Deleted, Timedout, DeleteQueued, DeleteTimedout, DeleteError, PatchQueued, CertRolloverQueued, ResizeQueued, ClusterCustomization
   * **Region**: Der Azure-Standort. Eine Liste der unterstützten Azure-Standorte, finden Sie im Dropdown-Listenfeld **Region** unter [HDInsight Preise](https://azure.microsoft.com/pricing/details/hdinsight/).
   * **Erstellungsdatum**.
   * **Betriebssystem**: Entweder **Windows** oder **Linux**
   * **Typ**: Hadoop, HBase, Storm, Spark
   * **Version**. Siehe [HDInsight-Versionen](hdinsight-component-versioning.md)
   * **Abonnement**: Name des Abonnements.
   * **Standarddatenquelle**: Das Standardcluster-Dateisystem.
   * **Workerknotengröße**.
   * **Hauptknotengröße**.

## <a name="delete-clusters"></a>Löschen von Clustern
Wenn Sie einen Cluster löschen, werden das Standardspeicherkonto oder die verknüpften Speicherkonten dadurch nicht gelöscht. Sie können den Cluster mit den gleichen Speicherkonten und den gleichen Metastores neu erstellen. Sie sollten einen neuen Standardblobcontainer verwenden, wenn Sie den Cluster neu erstellen.

1. Melden Sie sich beim [Portal][azure-portal] an.
2. Klicken Sie im linken Menü auf **HDInsight-Cluster** . Klicken Sie zuerst auf **Weitere Dienste**, falls **HDInsight-Cluster** nicht angezeigt wird.
3. Klicken Sie auf den Cluster, den Sie löschen möchten.
4. Klicken Sie im oberen Menü auf **Löschen** , und befolgen Sie dann die Anweisungen.

Siehe auch [Anhalten/Herunterfahren von Clustern](#pauseshut-down-clusters).

## <a name="add-additional-storage-accounts"></a>Hinzufügen weiterer Speicherkonten

Nachdem ein Cluster erstellt wurde, können Sie weitere Azure Storage- und Azure Data Lake Store-Speicherkonten hinzufügen. Weitere Informationen finden Sie unter [Hinzufügen zusätzlicher Speicherkonten zu HDInsight](./hdinsight-hadoop-add-storage.md).

## <a name="scale-clusters"></a>Skalieren von Clustern
Mithilfe der Clusterskalierung können Sie die Anzahl der von einem in Azure HDInsight ausgeführten Cluster verwendeten Workerknoten ändern, ohne den Cluster neu erstellen zu müssen.

> [!NOTE]
> Es werden nur Cluster mit HDInsight-Versionen ab 3.1.3 unterstützt. Überprüfen Sie ggf. auf der Seite „Eigenschaften“ die Version Ihres Clusters.  Informationen hierzu finden Sie unter [Auflisten und Anzeigen von Clustern](#list-and-show-clusters).
>
>

Auswirkungen der Änderung der Anzahl von Datenknoten für die von HDInsight unterstützten Clustertypen:

* Hadoop

    Sie können die Anzahl der Workerknoten in einem aktiven Hadoop-Cluster problemlos ohne Auswirkungen auf ausstehende oder aktive Aufträge erhöhen. Neue Aufträge können auch während des Vorgangs gesendet werden. Fehler bei einer Skalierung werden ordnungsgemäß behandelt, sodass der Cluster immer in einem funktionsfähigen Zustand verbleibt.

    Wenn ein Hadoop-Cluster durch Verringern der Anzahl der Datenknoten zentral herunterskaliert wird, werden einige der Dienste im Cluster neu gestartet. Dieses Verhalten führt beim Abschluss des Skalierungsvorgangs bei allen aktiven und ausstehenden Aufträgen zu einem Fehler. Sie können die Aufträge jedoch nach Abschluss des Vorgangs erneut senden.
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

    Weitere Informationen finden Sie in der [Apache Storm-Dokumentation](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).

    Die Storm-Webbenutzeroberfläche ist für den HDInsight-Cluster verfügbar:

    ![Ausgleichen der HDInsight Storm-Skalierung](./media/hdinsight-administer-use-portal-linux/hdinsight-portal-scale-cluster-storm-rebalance.png)

    Es folgt ein Beispiel, wie die Storm-Topologie mithilfe des CLI-Befehls neu ausgeglichen werden kann:

        ## Reconfigure the topology "mytopology" to use 5 worker processes,
        ## the spout "blue-spout" to use 3 executors, and
        ## the bolt "yellow-bolt" to use 10 executors
        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

**So skalieren Sie Cluster**

1. Melden Sie sich beim [Portal][azure-portal] an.
2. Klicken Sie im linken Menü auf **HDInsight-Cluster** .
3. Klicken Sie auf den Cluster, den Sie skalieren möchten.
3. Klicken Sie auf **Cluster skalieren**.
4. Geben Sie die **Anzahl von Workerknoten**ein. Die maximale Anzahl von Clusterknoten variiert abhängig vom Azure-Abonnement. Sie können sich an den Azure-Abrechnungssupport wenden, falls Sie diese Begrenzung erhöhen möchten.  Die Änderungen, die Sie an der Anzahl von Knoten vorgenommen haben, werden in den Kosteninformationen widergespiegelt.

    ![HDInsight Hadoop HBase Storm Spark skalieren](./media/hdinsight-administer-use-portal-linux/hdinsight-portal-scale-cluster.png)

## <a name="pauseshut-down-clusters"></a>Anhalten/Herunterfahren von Clustern

Die meisten Hadoop-Aufträge sind Batchaufträge, die nur gelegentlich ausgeführt werden. Bei den meisten Hadoop-Clustern wird der Cluster immer wieder für einen längeren Zeitraum nicht zur Verarbeitung verwendet. Mit HDInsight werden Ihre Daten im Azure-Speicher gespeichert, sodass Sie einen Cluster problemlos löschen können, wenn er nicht verwendet wird.
Für einen HDInsight-Cluster fallen auch dann Gebühren an, wenn er nicht verwendet wird. Da die Gebühren für den Cluster erheblich höher sind als die Kosten für den Speicher, ist es sinnvoll, nicht verwendete Cluster zu löschen.

Es gibt viele Methoden, mit denen Sie den Prozess programmieren können:

* Verwenden Sie Azure Data Factory. Siehe [Erstellen von bedarfsgesteuerten Linux-basierten Hadoop-Clustern in HDInsight mit Azure Data Factory](hdinsight-hadoop-create-linux-clusters-adf.md) zum Erstellen bedarfsgesteuerter verknüpfter HDInsight-Dienste.
* Verwenden Sie Azure PowerShell.  Weitere Informationen hierzu finden Sie unter [Analysieren von Flugverspätungsdaten mit Hive in HDInsight](hdinsight-analyze-flight-delay-data.md).
* Verwenden Sie die Azure-Befehlszeilenschnittstelle. Weitere Informationen hierzu finden Sie unter [Verwalten von Hadoop-Clustern in HDInsight mit der Azure-Befehlszeilenschnittstelle](hdinsight-administer-use-command-line.md).
* Verwenden das Sie HDInsight .NET-SDK. Weitere Informationen hierzu finden Sie unter [Übermitteln von Hadoop-Aufträgen in HDInsight](hdinsight-submit-hadoop-jobs-programmatically.md).

Die Preisinformationen finden Sie unter [HDInsight Preise](https://azure.microsoft.com/pricing/details/hdinsight/). Informationen zum Löschen eines Clusters aus dem Portal finden Sie unter [Löschen von Clustern](#delete-clusters)

## <a name="change-passwords"></a>Ändern von Kennwörtern
Ein HDInsight-Cluster kann über zwei Benutzerkonten verfügen. Das HDInsight-Clusterbenutzerkonto (auch HTTP-Benutzerkonto genannt) und das SSH-Benutzerkonto werden während des Erstellungsprozesses erstellt. Sie können die Ambari-Webbenutzeroberfläche verwenden, um den Benutzernamen und das Kennwort für das Clusterbenutzerkonto zu ändern, und Skriptaktionen, um das SSH-Benutzerkonto zu ändern.

### <a name="change-the-cluster-user-password"></a>Ändern des Clusterbenutzerkennworts
Sie können die Ambari-Webbenutzeroberfläche verwenden, um das Clusterbenutzerkennwort zu ändern. Zum Anmelden bei Ambari müssen Sie den vorhandenen Benutzernamen und das vorhandene Kennwort verwenden.

> [!NOTE]
> Wenn Sie das Kennwort für den Clusterbenutzer (Admin) ändern, können Skriptaktionen, die für diesen Cluster ausgeführt werden, möglicherweise fehlschlagen. Wenn Sie Skriptaktionen beibehalten möchten, deren Ziel Workerknoten sind, können diese Skripts fehlschlagen, sobald Sie dem Cluster über eine Größenänderung Knoten hinzufügen. Weitere Informationen zu Skriptaktionen finden Sie unter [Anpassen von HDInsight-Clustern mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md).
>
>

1. Melden Sie sich bei der Ambari-Webbenutzeroberfläche mit den HDInsight-Clusterbenutzer-Anmeldeinformationen an. Der Standard-Benutzername lautet **admin**. Die URL lautet **https://&lt;HDInsight-Clustername>azurehdinsight.net**.
2. Klicken Sie im oberen Menü auf **Administrator** und anschließend auf „Ambari verwalten“.
3. Klicken Sie im linken Menü auf **Benutzer**.
4. Klicken Sie auf **Administrator**.
5. Klicken Sie auf **Kennwort ändern**.

Ambari ändert anschließend das Kennwort auf allen Knoten im Cluster.

### <a name="change-the-ssh-user-password"></a>Ändern des SSH-Benutzerkennworts
1. Speichern Sie den folgenden Text mit einem Texteditor als Datei mit dem Namen **changepassword.sh**.

   > [!IMPORTANT]
   > Sie müssen einen Editor verwenden, der „LF“ als Zeilenende verwendet. Wenn der Editor „CRLF“ verwendet, funktioniert das Skript nicht.
   >
   >

        #! /bin/bash
        USER=$1
        PASS=$2

        usermod --password $(echo $PASS | openssl passwd -1 -stdin) $USER
2. Laden Sie die Datei an einen Speicherort hoch, auf den von HDInsight über eine HTTP- oder HTTPS-Adresse zugegriffen werden kann. Verwenden Sie z.B. einen öffentlichen Dateispeicher wie OneDrive oder Azure Blob Storage. Speichern Sie den URI (HTTP- oder HTTPS-Adresse) in der Datei, da dieser URI im nächsten Schritt benötigt wird.
3. Klicken Sie im Azure-Portal auf **HDInsight-Cluster**.
4. Klicken Sie auf Ihren HDInsight-Cluster.
4. Klicken Sie auf **Skriptaktionen**.
4. Wählen Sie auf dem Blatt **Skriptaktionen** die Option **Neue übermitteln** aus. Geben Sie auf dem Blatt **Skriptaktion übermitteln** die folgenden Informationen ein:

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
2. Klicken Sie auf **Abonnements**. Jedes Abonnement verfügt über einen Namen und eine ID.

Jeder Cluster ist mit einem Azure-Abonnement verknüpft. Die Abonnement-ID wird auf der Clusterkachel **Zusammenfassung** angezeigt. Informationen hierzu finden Sie unter [Auflisten und Anzeigen von Clustern](#list-and-show-clusters).

## <a name="find-the-resource-group"></a>Suchen der Ressourcengruppe
Im Azure Resource Manager-Modus wird jeder HDInsight-Cluster mit einer Azure Resource Manager-Gruppe erstellt. Die Resource Manager-Gruppe, zu der ein Cluster gehört, wird hier angezeigt:

* Die Liste der Cluster verfügt über eine Spalte **Ressourcengruppe** .
* Die Kachel **Zusammenfassung** des Clusters.  

Informationen hierzu finden Sie unter [Auflisten und Anzeigen von Clustern](#list-and-show-clusters).

## <a name="find-the-default-storage-account"></a>Suchen des Standardspeicherkontos
Jeder HDInsight-Cluster verfügt über eine Standardspeicherkonto. Das Standardspeicherkonto und die dazugehörigen Schlüssel für einen Cluster werden unter **Speicherkonten** angezeigt. Informationen hierzu finden Sie unter [Auflisten und Anzeigen von Clustern](#list-and-show-clusters).

## <a name="run-hive-queries"></a>Ausführen von Hive-Abfragen
Hive-Aufträge können Sie nicht direkt vom Azure-Portal aus ausführen, aber Sie können die Hive-Ansicht auf der Ambari-Webbenutzeroberfläche verwenden.

**So führen Sie Hive-Abfragen mithilfe der Ambari-Hive-Ansicht aus**

1. Melden Sie sich bei der Ambari-Webbenutzeroberfläche mit den HDInsight-Clusterbenutzer-Anmeldeinformationen an. Der Standard-Benutzername lautet **admin**. Die URL lautet **https://&lt;HDInsight-Clustername>azurehdinsight.net**.
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
4. Klicken Sie im linken Menü des Clusters auf **Speicherkonten**.
5. Klicken Sie auf ein Speicherkonto.
7. Klicken Sie auf die Kachel **Blobs** .
8. Klicken Sie auf den Standardcontainernamen.

## <a name="monitor-cluster-usage"></a>Überwachen der Clusternutzung
Der Abschnitt **Nutzung** auf dem Blatt für den HDInsight-Cluster enthält Informationen zur Anzahl von Kernen, die für Ihr Abonnement zur Verwendung mit HDInsight verfügbar sind. Außerdem ist hier die Anzahl von Kernen angegeben, die diesem Cluster zugeordnet sind. Weiterhin wird angezeigt, wie die Kerne für die Knoten innerhalb dieses Clusters zugeordnet sind. Informationen hierzu finden Sie unter [Auflisten und Anzeigen von Clustern](#list-and-show-clusters).

> [!IMPORTANT]
> Um die vom HDInsight-Cluster bereitgestellten Dienste zu überwachen, müssen Sie Ambari Web oder die Ambari-REST-API verwenden. Weitere Informationen zur Verwendung von Ambari finden Sie unter [Verwalten von HDInsight-Clustern mit Ambari](hdinsight-hadoop-manage-ambari.md)
>
>

## <a name="connect-to-a-cluster"></a>Herstellen der Verbindung zu einem Cluster

* [Verwenden von Hive mit HDInsight](hdinsight-hadoop-use-hive-ambari-view.md)
* [Verwenden von SSH mit HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie einige grundlegende administrative Funktionen kennengelernt. Weitere Informationen finden Sie in den folgenden Artikeln:

* [Verwalten von HDInsight mit Azure PowerShell](hdinsight-administer-use-powershell.md)
* [Verwalten von HDInsight mit der Azure-CLI](hdinsight-administer-use-command-line.md)
* [Erstellen von Hadoop-Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md)
* [Verwenden von Hive in HDInsight](hdinsight-use-hive.md)
* [Verwenden von Pig in HDInsight](hdinsight-use-pig.md)
* [Verwenden von Sqoop in HDInsight](hdinsight-use-sqoop.md)
* [Erste Schritte mit Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
* [Welche Hadoop-Version wird in Azure HDInsight verwendet?](hdinsight-component-versioning.md)

[azure-portal]: https://portal.azure.com
[image-hadoopcommandline]: ./media/hdinsight-administer-use-portal-linux/hdinsight-hadoop-command-line.png "Hadoop-Befehlszeile"

