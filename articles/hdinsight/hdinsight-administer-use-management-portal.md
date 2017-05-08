---
title: Verwalten von Windows-basierten Hadoop-Clustern in HDInsight mit dem Azure-Portal | Microsoft-Dokumentation
description: "Hier erfahren Sie, wie Sie den HDInsight-Dienst verwalten. Erstellen Sie ein HDInsight-Cluster, öffnen Sie die interaktive JavaScript-Konsole, und öffnen Sie die Hadoop-Befehlszeile."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 9295a988-bd88-453a-8c8b-55fa103bf39c
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/17/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.translationtype: Human Translation
ms.sourcegitcommit: f6006d5e83ad74f386ca23fe52879bfbc9394c0f
ms.openlocfilehash: c9c45305d75be59f7840941b1e626e62e93f1758
ms.contentlocale: de-de
ms.lasthandoff: 05/03/2017


---
# <a name="manage-windows-based-hadoop-clusters-in-hdinsight-by-using-the-azure-portal"></a>Verwalten von Windows-basierten Hadoop-Clustern in HDInsight mit dem Azure-Portal

Mit dem [Azure-Portal][azure-portal] können Sie Windows-basierte Hadoop-Cluster in Azure HDInsight erstellen, das Hadoop-Benutzerkennwort ändern und das Remotedesktopprotokoll (RDP) aktivieren, sodass Sie auf die Hadoop-Befehlskonsole im Cluster zugreifen können.

Die Informationen in diesem Artikel gelten nur für Windows-basierte HDInsight-Cluster. Informationen zur Verwaltung von Linux-basierten Clustern finden Sie unter [Verwalten von Hadoop-Clustern in HDInsight mit dem Azure-Portal](hdinsight-administer-use-portal-linux.md).

> [!IMPORTANT]
> Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight unter Windows](hdinsight-component-versioning.md#hdi-version-33-nearing-deprecation-date).


## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit diesem Artikel beginnen können, benötigen Sie Folgendes:

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Azure-Speicherkonto** – Ein HDInsight-Cluster verwendet einen Azure-Blobspeichercontainer als Standarddateisystem. Weitere Informationen über Azure-Blobspeicher und dessen nahtlose Integration in HDInsight-Cluster finden Sie unter [Verwenden von Azure-Blobspeicher mit HDInsight](hdinsight-hadoop-use-blob-storage.md). Weitere Informationen zum Erstellen eines Azure-Speicherkontos finden Sie unter [Erstellen eines Speicherkontos](../storage/storage-create-storage-account.md).

## <a name="open-the-portal"></a>Öffnen des Portals
1. Melden Sie sich bei [https://portal.azure.com](https://portal.azure.com)an.
2. Nachdem Sie das Portal geöffnet haben, können Sie die folgenden Schritte ausführen:

   * Klicken Sie im linken Menü auf **Neu** , um einen neuen Clusters zu erstellen:

       ![Schaltfläche „Neuer HDInsight-Cluster“](./media/hdinsight-administer-use-management-portal/azure-portal-new-button.png)
   * Klicken Sie im linken Menü auf **HDInsight-Cluster** .

       ![Azure-Portal – Schaltfläche „HDInsight-Cluster“](./media/hdinsight-administer-use-management-portal/azure-portal-hdinsight-button.png)

     Wenn **HDInsight** im linken Menü nicht angezeigt wird, klicken Sie auf **Durchsuchen**.

     ![Azure-Portal – Schaltfläche „Cluster durchsuchen“](./media/hdinsight-administer-use-management-portal/azure-portal-browse-button.png)

## <a name="create-clusters"></a>Erstellen von Clustern
Informationen zur Erstellung mithilfe des Portals finden Sie unter [Erstellen von HDInsight-Clustern](hdinsight-hadoop-provision-linux-clusters.md).

In HDInsight kann eine Vielzahl von Hadoop-Komponenten verwendet werden. Eine Liste der überprüften und unterstützten Komponenten finden Sie unter [Neuheiten in den von HDInsight bereitgestellten Clusterversionen](hdinsight-component-versioning.md). Sie können HDInsight mit einer der folgenden Optionen anpassen:

* Verwenden Sie Skriptaktion, um benutzerdefinierte Skripts auszuführen, mit denen ein Cluster angepasst werden kann, um eine Clusterkonfiguration zu ändern oder um benutzerdefinierte Komponenten wie Giraph oder Solr zu installieren. Weitere Informationen finden Sie unter [Anpassen eines HDInsight-Clusters mithilfe von Skriptaktion](hdinsight-hadoop-customize-cluster.md).
* Verwenden Sie die Clusteranpassungsparameter im HDInsight .NET-SDK oder in Azure PowerShell während der Clustererstellung. Diese Konfigurationsänderungen bleiben während der gesamten Lebensdauer des Clusters erhalten und werden nicht vom Reimaging von Clusterknoten beeinträchtigt, das die Azure-Plattform regelmäßig zu Wartungszwecken durchführt. Weitere Informationen zur Verwendung der Clusteranpassungsparameter finden Sie unter [Erstellen von HDInsight-Clustern](hdinsight-hadoop-provision-linux-clusters.md).
* Einige systemeigene Java-Komponenten wie Mahout und Cascading können auf dem Cluster als JAR-Dateien ausgeführt werden. Diese JAR-Dateien können an Azure-Blobspeicher verteilt und mit den Verfahren zur Übermittlung von Hadoop-Aufträgen an HDInsight-Cluster gesendet werden. Weitere Informationen finden Sie unter [Programmgesteuerte Übermittlung von Hadoop-Aufträgen](hdinsight-submit-hadoop-jobs-programmatically.md).

  > [!NOTE]
  > Wenn bei der Bereitstellung von JAR-Dateien für HDInsight-Cluster oder beim Aufrufen von JAR-Dateien für HDInsight-Cluster Probleme auftreten, wenden Sie sich an den [Microsoft-Support](https://azure.microsoft.com/support/options/).
  >
  > Cascading wird von HDInsight nicht unterstützt, und es steht kein Microsoft-Support dafür zur Verfügung. Listen der unterstützten Komponenten finden Sie unter [Neuheiten in den von HDInsight bereitgestellten Clusterversionen](hdinsight-component-versioning.md).
  >
  >

Die Installation von benutzerdefinierter Software auf dem Cluster über eine Remotedesktopverbindung wird nicht unterstützt. Speichern Sie nach Möglichkeit keine Dateien auf den Laufwerken des Hauptknotens, da diese verloren gehen, wenn Sie die Cluster neu erstellen müssen. Wir empfehlen die Speicherung von Dateien im Azure-Blobspeicher. Blobspeicher ist persistent.

## <a name="list-and-show-clusters"></a>Auflisten und Anzeigen von Clustern
1. Melden Sie sich bei [https://portal.azure.com](https://portal.azure.com)an.
2. Klicken Sie im linken Menü auf **HDInsight-Cluster** .
3. Klicken Sie auf den Clusternamen. Wenn die Clusterliste lang ist, können Sie oben auf der Seite filtern.
4. Doppelklicken Sie auf einen Cluster aus der Liste, um Details anzuzeigen.

    **Menü und Zusammenfassung**:

    ![Azure-Portal – HDInsight-Cluster-Zusammenfassung](./media/hdinsight-administer-use-management-portal/hdinsight-essentials.png)

   * Klicken Sie zum Anpassen des Menüs mit der rechten Maustaste auf eine beliebige Stelle im Menü, und klicken Sie dann auf **Anpassen**.
   * **Einstellungen** und **Alle Einstellungen**: Zeigt das Blatt **Einstellungen** für den Cluster an, über das Sie auf detaillierte Konfigurationsinformationen für den Cluster zugreifen können.
   * **Dashboard**, **Cluster-Dashboard** und **URL: Über diese Optionen können Sie auf das Cluster-Dashboard zugreifen (Ambari Web für Linux-basierte Cluster). -**Secure Shell**: Zeigt die Anweisungen für die Verbindung mit dem Cluster über eine Secure Shell-Verbindung (SSH) an.
   * **Cluster skalieren**: Dient zum Ändern der Anzahl von Workerknoten für den Cluster.
   * **Löschen**: Löscht den Cluster.
   * **Schnellstart**: Zeigt hilfreiche Informationen für die ersten Schritte mit HDInsight an.
   * **Benutzer: Dient zum Festlegen der Berechtigungen für die *Portalverwaltung* dieses Clusters für andere Benutzer in Ihrem Azure-Abonnement.

     > [!IMPORTANT]
     > Die hier vorgenommenen Einstellungen betreffen *nur* den Zugriff und die Berechtigungen für diesen Cluster im Azure-Portal und haben keine Auswirkung darauf, wer eine Verbindung mit dem HDInsight-Cluster herstellen oder Aufträge übermitteln kann.
     >
     >
   * **Tags**: Mithilfe von Tags können Sie Schlüssel-Wert-Paare festlegen, um eine benutzerdefinierte Taxonomie für Ihre Clouddienste zu definieren. Sie können z. B. einen Schlüssel mit dem Namen **Projekt** erstellen und dann einen gemeinsamen Wert für alle mit einem bestimmten Projekt verknüpften Dienste verwenden.
   * **Ambari-Ansichten**: Verknüpfung mit Ambari Web.

     > [!IMPORTANT]
     > Um die vom HDInsight-Cluster bereitgestellten Dienste zu verwalten, müssen Sie Ambari Web oder die Ambari-REST-API verwenden. Weitere Informationen zur Verwendung von Ambari finden Sie unter [Verwalten von HDInsight-Clustern mit Ambari](hdinsight-hadoop-manage-ambari.md).
     >
     >

     **Verwendung**:

     ![Azure-Portal – HDInsight-Clusternutzung](./media/hdinsight-administer-use-management-portal/hdinsight-portal-cluster-usage.png)
5. Klicken Sie auf **Einstellungen**.

    ![Azure-Portal – HDInsight-Clusternutzung](./media/hdinsight-administer-use-management-portal/hdinsight.portal.cluster.settings.png)

   * **Eigenschaften**: Anzeigen der Clustereigenschaften
   * **Cluster-AAD-Identität**:
   * **Azure-Speicherschlüssel**: Anzeigen des Standardspeicherkontos und des dazugehörigen Schlüssels. Das Speicherkonto wird während der Clustererstellung konfiguriert.
   * **Clusteranmeldung**: Ändern des Cluster-HTTP-Benutzernamens und des Kennworts
   * **Externe Metastores**: Anzeigen der Hive- und Oozie-Metastores. Die Metastores können nur während der Clustererstellung konfiguriert werden.
   * **Cluster skalieren**: Erhöhen und Verringern der Anzahl der Cluster-Workerknoten
   * **Remotedesktop**: Aktivieren und Deaktivieren von Remotedesktop (RDP)-Zugriff und Konfigurieren des RDP-Benutzernamens.  Der RDP-Benutzername muss sich vom HTTP-Benutzernamen unterscheiden.
   * **Registrierter Partner**:

     > [!NOTE]
     > Dies ist eine generische Liste der verfügbaren Einstellungen – nicht alle stehen für alle Cluster zur Verfügung.
     >
     >
6. Klicken Sie auf **Eigenschaften**.

    Der Abschnitt „Eigenschaften“ umfasst Folgendes:

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
   * **Primäre Datenquelle** Das Azure Blob-Speicherkonto, das als Standard-Hadoop-Dateisystem verwendet wird.
   * **Workerknoten – Tarif**
   * **Hauptknoten – Tarif**

## <a name="delete-clusters"></a>Löschen von Clustern
Wenn Sie einen Cluster löschen, werden das Standardspeicherkonto oder die verknüpften Speicherkonten dadurch nicht gelöscht. Sie können den Cluster mit den gleichen Speicherkonten und den gleichen Metastores neu erstellen.

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

    ![HDInsight Storm-Skalierung ausgleichen](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.storm.rebalance.png)

    Es folgt ein Beispiel, wie die Storm-Topologie mithilfe des CLI-Befehls neu ausgeglichen werden kann:

        ## Reconfigure the topology "mytopology" to use 5 worker processes,
        ## the spout "blue-spout" to use 3 executors, and
        ## the bolt "yellow-bolt" to use 10 executors
        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

**So skalieren Sie Cluster**

1. Melden Sie sich beim [Portal][azure-portal] an.
2. Klicken Sie im linken Menü auf **Alle durchsuchen**, klicken Sie auf **HDInsight-Cluster** und anschließend auf den Clusternamen.
3. Klicken Sie im oberen Menü auf **Einstellungen** und anschließend auf **Cluster skalieren**.
4. Geben Sie die **Anzahl von Workerknoten**ein. Die maximale Anzahl von Clusterknoten variiert abhängig vom Azure-Abonnement. Sie können sich an den Azure-Abrechnungssupport wenden, falls Sie diese Begrenzung erhöhen möchten.  Die Änderungen, die Sie an der Anzahl von Knoten vorgenommen haben, werden in den Kosteninformationen widergespiegelt.

    ![HDInsight Hadoop HBase Storm Spark skalieren](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.png)

## <a name="pauseshut-down-clusters"></a>Anhalten/Herunterfahren von Clustern
Die meisten Hadoop-Aufträge sind Batch-Aufträge, die nur gelegentlich ausgeführt werden. Bei den meisten Hadoop-Clustern wird der Cluster immer wieder für einen längeren Zeitraum nicht zur Verarbeitung verwendet. Mit HDInsight werden Ihre Daten im Azure-Speicher gespeichert, sodass Sie einen Cluster problemlos löschen können, wenn er nicht verwendet wird.
Für einen HDInsight-Cluster fallen auch dann Gebühren an, wenn er nicht verwendet wird. Da die Gebühren für den Cluster erheblich höher sind als die Kosten für den Speicher, ist es sinnvoll, nicht verwendete Cluster zu löschen.

Es gibt viele Methoden, mit denen Sie den Prozess programmieren können:

* Verwenden Sie Azure Data Factory. Unter [Verknüpfter Azure HDInsight-Dienst](../data-factory/data-factory-compute-linked-services.md) und [Transformation und Analyse mit Azure Data Factory](../data-factory/data-factory-data-transformation-activities.md) finden Sie Informationen über bedarfsgesteuerte und selbstdefinierte mit HDInsight verknüpfte Dienste.
* Verwenden Sie Azure PowerShell.  Weitere Informationen hierzu finden Sie unter [Analysieren von Flugverspätungsdaten mit Hive in HDInsight](hdinsight-analyze-flight-delay-data.md).
* Verwenden Sie die Azure-Befehlszeilenschnittstelle. Weitere Informationen hierzu finden Sie unter [Verwalten von Hadoop-Clustern in HDInsight mit der Azure-Befehlszeilenschnittstelle](hdinsight-administer-use-command-line.md).
* Verwenden das Sie HDInsight .NET-SDK. Weitere Informationen hierzu finden Sie unter [Übermitteln von Hadoop-Aufträgen in HDInsight](hdinsight-submit-hadoop-jobs-programmatically.md).

Die Preisinformationen finden Sie unter [HDInsight Preise](https://azure.microsoft.com/pricing/details/hdinsight/). Informationen zum Löschen eines Clusters aus dem Portal finden Sie unter [Löschen von Clustern](#delete-clusters)

## <a name="change-cluster-username"></a>Ändern des Clusterbenutzernamens
Ein HDInsight-Cluster kann über zwei Benutzerkonten verfügen. Das Benutzerkonto des HDInsight-Clusters wird während des Erstellungsprozesses erstellt. Sie können auch ein RDP-Benutzerkonto erstellen, um über das RDP auf den Cluster zuzugreifen. Weitere Informationen finden Sie unter [So aktivieren Sie den Remotedesktopzugriff](#connect-to-hdinsight-clusters-by-using-rdp).

**So ändern Sie den Benutzernamen und das Kennwort für einen HDInsight-Cluster**

1. Melden Sie sich beim [Portal][azure-portal] an.
2. Klicken Sie im linken Menü auf **Alle durchsuchen**, klicken Sie auf **HDInsight-Cluster** und anschließend auf den Clusternamen.
3. Klicken Sie im oberen Menü auf **Einstellungen** und anschließend auf **Clusteranmeldung**.
4. Wenn **Clusteranmeldung** aktiviert ist, klicken Sie auf **Deaktivieren** und dann auf **Aktivieren**, bevor Sie den Benutzernamen und das Kennwort ändern.
5. Ändern Sie den **Clusterbenutzernamen** und/oder das **Clusteranmeldekennwort**, und klicken Sie dann auf **Speichern**.

    ![HDInsight Clusterbenutzernamen ändern Clusterkennwort ändern HTTP-Benutzer](./media/hdinsight-administer-use-management-portal/hdinsight.portal.change.username.password.png)

## <a name="grantrevoke-access"></a>Gewähren/Entziehen des Zugriffs
In HDInsight-Clustern stehen die folgenden HTTP-Webdienste zur Verfügung (alle mit RESTful-Endpunkten):

* ODBC
* JDBC
* Ambari
* Oozie
* Templeton

Der Zugriff auf diese Dienste wird standardmäßig gewährt. Über das Azure-Portal können Sie den Zugriff widerrufen/gewähren.

> [!NOTE]
> Durch Gewähren/Widerrufen des Zugriffs werden der Benutzername und das Kennwort des Clusterbenutzers zurückgesetzt.
>
>

**So gewähren/widerrufen Sie den Zugriff auf HTTP-Webdienste**

1. Melden Sie sich beim [Portal][azure-portal] an.
2. Klicken Sie im linken Menü auf **Alle durchsuchen**, klicken Sie auf **HDInsight-Cluster** und anschließend auf den Clusternamen.
3. Klicken Sie im oberen Menü auf **Einstellungen** und anschließend auf **Clusteranmeldung**.
4. Wenn **Clusteranmeldung** aktiviert ist, klicken Sie auf **Deaktivieren** und dann auf **Aktivieren**, bevor Sie den Benutzernamen und das Kennwort ändern.
5. Geben Sie für **Clusterbenutzername** und **Clusteranmeldekennwort** den neuen Benutzernamen bzw. das Kennwort für den Cluster ein.
6. Klicken Sie auf **SPEICHERN**.

    ![HDInsight HTTP-Webdienstzugriff gewähren widerrufen](./media/hdinsight-administer-use-management-portal/hdinsight.portal.change.username.password.png)

## <a name="find-the-default-storage-account"></a>Suchen des Standardspeicherkontos
Jeder HDInsight-Cluster verfügt über eine Standardspeicherkonto. Das Standardspeicherkonto und die zugehörigen Schlüssel für einen Cluster werden unter **Einstellungen**/**Eigenschaften**/**Azure-Speicherschlüssel**. Informationen hierzu finden Sie unter [Auflisten und Anzeigen von Clustern](#list-and-show-clusters).

## <a name="find-the-resource-group"></a>Suchen der Ressourcengruppe
Im Azure Resource Manager-Modus wird jeder HDInsight-Cluster mit einer Azure-Ressourcengruppe erstellt. Die Azure-Ressourcengruppe, zu der ein Cluster gehört, wird hier angezeigt:

* Die Liste der Cluster verfügt über eine Spalte **Ressourcengruppe** .
* Die Kachel **Zusammenfassung** des Clusters.  

Informationen hierzu finden Sie unter [Auflisten und Anzeigen von Clustern](#list-and-show-clusters).

## <a name="open-hdinsight-query-console"></a>Öffnen der HDInsight-Abfragekonsole
Die HDInsight-Abfragekonsole umfasst die folgenden Features:

* **Hive-Editor**: Eine grafische Weboberfläche zum Übermitteln von Hive-Aufträgen.  Weitere Informationen hierzu finden Sie unter [Ausführen von Hive-Abfragen mithilfe der Abfragekonsole](hdinsight-hadoop-use-hive-query-console.md).

    ![HDInsight-Portal – Hive-Editor](./media/hdinsight-administer-use-management-portal/hdinsight-hive-editor.png)
* **Auftragsverlauf**: Überwachen von Hadoop-Aufträgen.  

    ![HDInsight-Portal – Auftragsverlauf](./media/hdinsight-administer-use-management-portal/hdinsight-job-history.png)

    Klicken Sie auf **Abfragename**, um die Details einschließlich Auftragseigenschaften, **Auftragsabfrage**, und **Auftragsausgabe anzuzeigen. Sie können auch die Abfrage und die Ausgabe auf Ihre Arbeitsstation herunterladen.
* **Dateibrowser**: Durchsuchen Sie das Standardspeicherkonto und die verknüpften Speicherkonten.

    ![HDInsight Portal – Dateibrowser durchsuchen](./media/hdinsight-administer-use-management-portal/hdinsight-file-browser.png)

    Im Screenshot gibt der Typ **<Account>** an, dass das Element ein Azure-Speicherkonto ist.  Klicken Sie auf den Kontonamen, um die Dateien zu durchsuchen.
* **Hadoop UI**.

    ![HDInsight-Portal – Hadoop UI](./media/hdinsight-administer-use-management-portal/hdinsight-hadoop-ui.png)

    Über **Hadoop UI*können Sie Dateien durchsuchen und Protokolle prüfen.
* **Yarn UI**.

    ![HDInsight-Portal – YARN UI](./media/hdinsight-administer-use-management-portal/hdinsight-yarn-ui.png)

## <a name="run-hive-queries"></a>Ausführen von Hive-Abfragen
Klicken Sie in der HDInsight-Abfragekonsole auf **Hive-Editor** , um Hive-Aufträge aus dem Portal auszuführen. Weitere Informationen hierzu finden Sie unter [Öffnen der HDInsight-Abfragekonsole](#open-hdinsight-query-console).

## <a name="monitor-jobs"></a>Überwachen von Aufträgen
Klicken Sie in der HDInsight-Abfragekonsole auf **Auftragsverlauf** , um Aufträge aus dem Portal zu überwachen. Weitere Informationen hierzu finden Sie unter [Öffnen der HDInsight-Abfragekonsole](#open-hdinsight-query-console).

## <a name="browse-files"></a>Dateien durchsuchen
Klicken Sie in der HDInsight-Abfragekonsole auf **Dateibrowser** , um im Standardspeicherkonto gespeicherte Dateien und die dazugehörigen Speicherkonten zu durchsuchen. Weitere Informationen hierzu finden Sie unter [Öffnen der HDInsight-Abfragekonsole](#open-hdinsight-query-console).

Sie können auch das Hilfsprogramm **Dateisystem durchsuchen** unter **Hadoop UI** in der HDInsight-Konsole verwenden.  Weitere Informationen hierzu finden Sie unter [Öffnen der HDInsight-Abfragekonsole](#open-hdinsight-query-console).

## <a name="monitor-cluster-usage"></a>Überwachen der Clusternutzung
Der Abschnitt **Nutzung** auf dem Blatt für den HDInsight-Cluster enthält Informationen zur Anzahl von Kernen, die für Ihr Abonnement zur Verwendung mit HDInsight verfügbar sind. Außerdem ist hier die Anzahl von Kernen angegeben, die diesem Cluster zugeordnet sind. Weiterhin wird angezeigt, wie die Kerne für die Knoten innerhalb dieses Clusters zugeordnet sind. Informationen hierzu finden Sie unter [Auflisten und Anzeigen von Clustern](#list-and-show-clusters).

> [!IMPORTANT]
> Um die vom HDInsight-Cluster bereitgestellten Dienste zu überwachen, müssen Sie Ambari Web oder die Ambari-REST-API verwenden. Weitere Informationen zur Verwendung von Ambari finden Sie unter [Verwalten von HDInsight-Clustern mit Ambari](hdinsight-hadoop-manage-ambari.md)
>
>

## <a name="open-hadoop-ui"></a>Öffnen der Hadoop-Benutzeroberfläche
Klicken Sie in der HDInsight-Abfragekonsole auf **Hadoop UI** , um den Cluster zu überwachen, das Dateisystem zu durchsuchen und Protokolle zu prüfen. Weitere Informationen hierzu finden Sie unter [Öffnen der HDInsight-Abfragekonsole](#open-hdinsight-query-console).

## <a name="open-yarn-ui"></a>Öffnen der Yarn-Benutzeroberfläche
Klicken Sie in der HDInsight-Abfragekonsole auf **Yarn UI** , um die Yarn-Benutzeroberfläche zu verwenden. Weitere Informationen hierzu finden Sie unter [Öffnen der HDInsight-Abfragekonsole](#open-hdinsight-query-console).

## <a name="connect-to-clusters-using-rdp"></a>Herstellen einer Verbindung zu Clustern mit RDP
Mit den Anmeldeinformationen, die Sie bei der Erstellung des Clusters für dieses festgelegt haben, können Sie auf die Dienste im Cluster zugreifen, nicht jedoch über eine Remotedesktopverbindung auf den Cluster selbst. Sie können beim oder nach dem Bereitstellen eines Clusters den Remotedesktopzugriff aktivieren. Eine Anleitung zum Aktivieren des Remotedesktops bei der Erstellung finden Sie unter [Erstellen von Hadoop-Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

**So aktivieren Sie den Remotedesktopzugriff**

1. Melden Sie sich beim [Portal][azure-portal] an.
2. Klicken Sie im linken Menü auf **Alle durchsuchen**, klicken Sie auf **HDInsight-Cluster** und anschließend auf den Clusternamen.
3. Klicken Sie im oberen Menü auf **Einstellungen** und anschließend auf **Remotedesktop**.
4. Füllen Sie die Felder **Gültig bis**, **Remotedesktopbenutzername** und **Remotedesktopkennwort** aus, und klicken Sie dann auf **Aktivieren**.

    ![HDInsight Remotedesktop aktivieren deaktivieren konfigurieren](./media/hdinsight-administer-use-management-portal/hdinsight.portal.remote.desktop.png)

    Der Standardwert für „Gültig bis“ ist eine Woche.

   > [!NOTE]
   > Mit dem HDInsight .NET SDK können Sie auch Remotedesktop auf einem Cluster aktivieren. Verwenden Sie die **EnableRdp**-Methode folgendermaßen für das HDInsight-Client-Objekt:**client.EnableRdp(clustername, location, "rdpuser", "rdppassword", DateTime.Now.AddDays(6))**. Um Remotedesktop auf dem Cluster zu deaktivieren, können Sie auch **client.DisableRdp(clustername, location)**verwenden. Weitere Informationen zu diesen Methoden finden Sie unter [HDInsight .NET SDK-Referenz](http://go.microsoft.com/fwlink/?LinkId=529017). Dies gilt nur für HDInsight-Cluster unter Windows.
   >
   >

**So stellen Sie eine Verbindung über RDP mit dem Cluster her**

1. Melden Sie sich beim [Portal][azure-portal] an.
2. Klicken Sie im linken Menü auf **Alle durchsuchen**, klicken Sie auf **HDInsight-Cluster** und anschließend auf den Clusternamen.
3. Klicken Sie im oberen Menü auf **Einstellungen** und anschließend auf **Remotedesktop**.
4. Klicken Sie auf **Verbinden** , und folgen Sie den Anweisungen. Falls „Verbinden“ deaktiviert ist, müssen Sie diese Option zunächst aktivieren. Achten Sie darauf, dass Sie den Benutzernamen und das Kennwort für Remotedesktop verwenden.  Es ist nicht möglich die Benutzeranmeldeinformationen des Clusters zu verwenden.

## <a name="open-hadoop-command-line"></a>Öffnen der Hadoop-Befehlszeile
Wenn Sie die Verbindung zum Cluster über den Remotedesktop herstellen und die Hadoop-Befehlszeile verwenden möchten, müssen Sie zuerst den Remotedesktopzugriff auf dem Cluster aktivieren, wie im vorherigen Abschnitt beschrieben.

**So öffnen Sie eine Hadoop-Befehlszeile**

1. Stellen Sie per Remotedesktop eine Verbindung mit dem Cluster her.
2. Doppelklicken Sie auf dem Desktop auf **Hadoop Command Line**.

    ![HDI.HadoopCommandLine][image-hadoopcommandline]

    Weitere Informationen zu Hadoop-Befehlen finden Sie in der [Hadoop-Befehlsreferenz](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/CommandsManual.html)(in englischer Sprache).

Im vorherigen Screenshot ist die Hadoop-Versionsnummer im Ordnernamen enthalten. Die Versionsnummer kann sich je nach den im Cluster installierten Hadoop-Komponenten ändern. Sie können mithilfe von Hadoop-Umgebungsvariablen auf diese Ordner verweisen. Beispiel:

    cd %hadoop_home%
    cd %hive_home%
    cd %hbase_home%
    cd %pig_home%
    cd %sqoop_home%
    cd %hcatalog_home%

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie erfahren, wie ein HDInsight-Cluster mit dem Azure-Portal erstellt und wie das Hadoop-Befehlszeilentool geöffnet wird. Weitere Informationen finden Sie in den folgenden Artikeln:

* [Verwalten von HDInsight mit Azure PowerShell](hdinsight-administer-use-powershell.md)
* [Verwalten von HDInsight mit der Azure-CLI](hdinsight-administer-use-command-line.md)
* [Erstellen von Hadoop-Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md)
* [Programmgesteuerte Übermittlung von Hadoop-Aufträgen](hdinsight-submit-hadoop-jobs-programmatically.md)
* [Erste Schritte mit Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
* [Welche Hadoop-Version wird in Azure HDInsight verwendet?](hdinsight-component-versioning.md)

[azure-portal]: https://portal.azure.com
[image-hadoopcommandline]: ./media/hdinsight-administer-use-management-portal/hdinsight-hadoop-command-line.png "Hadoop-Befehlszeile"

