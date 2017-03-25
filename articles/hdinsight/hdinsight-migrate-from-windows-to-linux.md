---
title: Migrieren von Windows-basierten HDInsight-Clustern zu Linux-basierten HDInsight-Clustern | Microsoft Docs
description: Erfahren Sie, wie sie von einem Windows-basierten HDInsight-Cluster zu einem Linux-basierten HDInsight-Cluster migrieren.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: ff35be59-bae3-42fd-9edc-77f0041bab93
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/13/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 1a2a08a844f1502d970cb7686d512387263d1d13
ms.lasthandoff: 03/21/2017


---
# <a name="migrate-from-a-windows-based-hdinsight-cluster-to-a-linux-based-cluster"></a>Migrieren von einem Windows-basierten HDInsight-Cluster zu einem Linux-basierten Cluster
Auch wenn Windows-basiertes HDInsight eine einfache Möglichkeit zur Verwendung von Hadoop in der Cloud darstellt, müssen Sie möglicherweise eine Migration zu einem Linux-basierten Cluster durchführen. Dies ist beispielsweise erforderlich, um Linux-basierte Tools und Technologien zu nutzen, die für Ihre Lösung erforderlich sind. Vieles im Hadoop-Ökosystem wird in Linux-basierten Systemen entwickelt. Einiges ist für die Nutzung mit HDInsight (Windows-basiert) möglicherweise nicht verfügbar. Darüber hinaus wird in vielen Büchern, Videos und anderem Trainingsmaterial angenommen, dass Sie beim Arbeiten mit Hadoop ein Linuxsystem verwenden.

Dieses Dokument enthält Informationen zu den Unterschieden zwischen HDInsight unter Windows und Linux sowie eine Anleitung zum Migrieren vorhandener Workloads zu einem Linux-basierten Cluster.

> [!NOTE]
> HDInsight-Cluster verwenden Ubuntu LTS-Versionen (Long-Term Support) als Betriebssystem für die Knoten im Cluster. Informationen zur Version von Ubuntu, die für HDInsight verfügbar ist, sowie weitere Informationen zu Versionen von Komponenten finden Sie unter [HDInsight-Komponentenversionen](hdinsight-component-versioning.md).

## <a name="migration-tasks"></a>Migrationsaufgaben
Der allgemeine Workflow für die Migration sieht folgendermaßen aus:

![Migrations-Workflow-Diagramm](./media/hdinsight-migrate-from-windows-to-linux/workflow.png)

1. Lesen Sie jeden Abschnitt dieses Dokuments, um zu verstehen, welche Änderungen möglicherweise erforderlich sind, wenn Sie Ihren vorhandenen Workflow, Aufträge etc. zu einem Linux-basierten Cluster migrieren.
2. Erstellen Sie einen Linux-basierten Cluster als Test-/Qualitätssicherungsumgebung. Weitere Informationen zur Erstellung eines Linux-basierten Clusters finden Sie unter [Erstellen von Linux-basierten Hadoop-Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).
3. Kopieren Sie vorhandene Aufträge, Datenquellen und Senken in die neue Umgebung.
4. Führen Sie Überprüfungstests durch, um sicherzustellen, dass Ihre Aufträge im neuen Cluster wie erwartet funktionieren.

Nachdem Sie überprüft haben, dass alles wie erwartet funktioniert, planen Sie für die Migration eine Downtime. Führen Sie während dieser Downtime die folgenden Aktionen durch:

1. Sichern Sie alle Daten, die vorübergehend lokal auf dem Clusterknoten gespeichert sind. zum Beispiel direkt auf einem Hauptknoten gespeicherte Daten.
2. Löschen Sie den Windows-basierten Cluster.
3. Erstellen Sie einen Linux-basierten Cluster mit demselben Standarddatenspeicher, der vom Windows-basierten Cluster verwendet wurde. Der neue Cluster kann so weiterhin mit Ihren vorhandenen Produktionsdaten arbeiten.
4. Importieren Sie alle vorübergehenden Daten, die Sie gesichert haben.
5. Starten Sie Aufträge/Verarbeiten Sie weiterhin mithilfe des neuen Clusters.

### <a name="copy-data-to-the-test-environment"></a>Kopieren von Daten in die Testumgebung
Es existieren viele Methoden zum Kopieren von Daten und Aufträgen. Die in diesem Abschnitt beschriebenen sind jedoch die einfachsten Methoden, um Dateien direkt in einen Testcluster zu verschieben.

#### <a name="hdfs-dfs-copy"></a>Kopieren mit HDFS DFS

Kopieren Sie anhand folgender Schritte Daten aus dem Produktionscluster in das Testcluster. In diesen Schritten wird das in HDInsight enthaltene `hdfs dfs`-Hilfsprogramm verwendet.

1. Suchen Sie die Informationen zum Speicherkonto und Standardcontainer für Ihren vorhandenen Cluster. Im folgenden Beispiel wird PowerShell zum Abrufen dieser Informationen verwendet:

    ```powershell
    $clusterName="Your existing HDInsight cluster name"
    $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    write-host "Storage account name: $clusterInfo.DefaultStorageAccount.split('.')[0]"
    write-host "Default container: $clusterInfo.DefaultStorageContainer"
    ```

2. Folgen Sie der Anleitung im Dokument „Erstellen von Linux-basierten Hadoop-Clustern in HDInsight“, um eine neue Testumgebung zu erstellen. Bevor Sie den Cluster erstellen, wählen Sie stattdessen **Optionale Konfiguration**aus.
3. Wählen Sie auf dem Blatt „Optionale Konfiguration“ **Verknüpfte Speicherkonten**aus.
4. Wählen Sie **Einen Speicherschlüssel hinzufügen**aus, und wählen Sie, wenn Sie dazu aufgefordert werden, das Speicherkonto aus, das in Schritt 1 vom PowerShell-Skript zurückgegeben wurde. Klicken Sie auf jedem Blatt auf **Auswahl**. Erstellen Sie schließlich den Cluster.
5. Nachdem der Cluster erstellt wurde, verbinden Sie sich mithilfe von **SSH** mit dem Cluster. Wenn Sie mit dem Verwenden von SSH mit HDInsight nicht vertraut sind, finden Sie weitere Informationen in einem der folgenden Dokumente:

   * [Verwenden von SSH (PuTTY) mit Linux-basiertem HDInsight unter Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
   * [Verwenden von SSH mit Linux-basiertem HDInsight unter Linux, Unix, OS X und Bash unter Windows](hdinsight-hadoop-linux-use-ssh-unix.md)

6. Verwenden Sie über die SSH-Sitzung den folgenden Befehl, um Daten aus dem verknüpften Speicherkonto in das neue Standardspeicherkonto zu kopieren. Ersetzen Sie CONTAINER durch die von PowerShell zurückgegebenen Containerinformationen. Ersetzen Sie __KONTO__ durch den Kontonamen. Ersetzen Sie den Datenpfad mit dem Datendateipfad.

        hdfs dfs -cp wasbs://CONTAINER@ACCOUNT.blob.core.windows.net/path/to/old/data /path/to/new/location

    > [!NOTE]
    > Wenn die Verzeichnisstruktur, die die Daten enthält, in der Testumgebung nicht existiert, können Sie diese mithilfe des folgenden Befehls erstellen:

        hdfs dfs -mkdir -p /new/path/to/create

    Der Schalter `-p` ermöglicht die Erstellung aller Verzeichnisse im Pfad.

#### <a name="direct-copy-between-azure-storage-blobs"></a>Direktes Kopieren zwischen Azure Storage-Blobs
Alternativ können Sie das Azure PowerShell-Cmdlet `Start-AzureStorageBlobCopy` verwenden, um Blobs zwischen Speicherkonten außerhalb von HDInsight zu kopieren. Weitere Informationen finden Sie im Abschnitt „Verwalten von Azure-Blobs“ im Artikel „Verwenden von Azure PowerShell mit Azure Storage“.

## <a name="client-side-technologies"></a>Clientseitige Technologien
Im Allgemeinen funktionieren clientseitige Technologien wie [Azure PowerShell-Cmdlets](/powershell/azureps-cmdlets-docs), die [Azure-Befehlszeilenschnittstelle](../cli-install-nodejs.md) oder das [.NET SDK für Hadoop](https://hadoopsdk.codeplex.com/) mit Linux-basierten Clustern weiterhin genauso wie bisher, da sie auf REST-APIs basieren, die bei beiden Cluster-Betriebssystemtypen gleich sind.

## <a name="server-side-technologies"></a>Serverseitige Technologien
Die folgende Tabelle enthält eine Hilfestellung zum Migrieren von serverseitigen Komponenten, die Windows-spezifisch sind.

| Wenn Sie folgende Technologie verwenden ... | Führen Sie diese Aktion durch... |
| --- | --- |
| **PowerShell** (serverseitige Skripts, einschließlich Skriptaktionen, die während der Clustererstellung verwendet werden) |Umschreiben zu Bash-Skripts. Informationen zu Skriptaktionen finden Sie unter [Anpassen Linux-basierter HDInsight-Cluster mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md) und [Entwickeln von Skriptaktionen mit HDInsight](hdinsight-hadoop-script-actions-linux.md). |
| **Azure-Befehlszeilenschnittstelle** (serverseitige Skripts) |Die Azure-Befehlszeilenschnittstelle ist für Linux verfügbar, auf den Hauptknoten des HDInsight-Clusters jedoch nicht vorinstalliert. Wenn Sie sie zum Erstellen serverseitiger Skripts benötigen, finden Sie unter [Installieren der Azure-Befehlszeilenschnittstelle](../cli-install-nodejs.md) Informationen zur Installation auf Linux-basierten Plattformen. |
| **.NET-Komponenten** |.NET wird derzeit auf Linux-basierten HDInsight-Clustertypen nicht vollständig unterstützt. Linux-basierte Cluster mit Storm in HDInsight, die nach dem 28.10.2016 erstellt wurden, unterstützen C#-Storm-Topologien mithilfe des SCP.NET-Frameworks. Zusätzliche Unterstützung für .NET wird in zukünftigen Updates hinzugefügt. |
| **Win32-Komponenten oder andere reine Windows-Technologie** |Die Anleitung hängt von der Komponente oder der Technologie ab. Möglicherweise finden Sie eine mit Linux kompatible Version, möglicherweise brauchen Sie eine alternative Lösung oder müssen diese Komponente umschreiben. |

## <a name="cluster-creation"></a>Clustererstellung
Dieser Abschnitt enthält Informationen zu Unterschieden beim Erstellen von Clustern.

### <a name="ssh-user"></a>SSH-Benutzer
Linux-basierte HDInsight-Cluster nutzen **Secure Shell (SSH)** als Protokoll, um Clusterknoten Remotezugriff zu gewähren. Im Gegensatz zu Remotedesktop für Windows-basierte Cluster bieten die meisten SSH-Clients keine grafische Benutzeroberfläche. Stattdessen bieten sie eine Befehlszeile, mithilfe derer Sie Befehle im Cluster ausführen können. Einige Clients (z.B. [MobaXterm](http://mobaxterm.mobatek.net/)) bieten zusätzlich zu einer Remotebefehlszeile einen grafischen Dateisystembrowser.

Während der Clustererstellung müssen Sie einen SSH-Benutzer und entweder ein **Kennwort** oder ein **öffentliches Schlüsselzertifikat** für die Authentifizierung bereitstellen.

Es empfiehlt sich, ein Public Key-Zertifikat zu verwenden, da dies sicherer ist als ein Kennwort. Die Zertifikatsauthentifizierung funktioniert, indem ein signiertes öffentliches/privates Schlüsselpaar generiert und anschließend beim Erstellen des Clusters der öffentliche Schlüssel bereitgestellt wird. Beim Verbinden zum Server mithilfe von SSH erfolgt die Authentifizierung für die Verbindung durch den privaten Schlüssel auf dem Client.

Weitere Informationen zur Verwendung von SSH mit HDInsight finden Sie in den folgenden Artikeln:

* [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
* [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix und Mac OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

### <a name="cluster-customization"></a>Clusteranpassung
Mit Linux-basierten Clustern verwendete **Skriptaktionen** müssen in Bash-Skripts geschrieben sein. Skriptaktionen können während der Clustererstellung verwendet werden. Für Linux-basierte Cluster können sie außerdem zum Anpassen verwendet werden, sobald ein Cluster eingerichtet wurde und ausgeführt wird. Weitere Informationen finden Sie unter [Anpassen Linux-basierter HDInsight-Cluster mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md) und [Entwickeln von Skriptaktionen mit HDInsight](hdinsight-hadoop-script-actions-linux.md).

Eine weitere Anpassungsfunktion ist **bootstrap**. Diese Funktion ermöglicht Ihnen, bei Windows-Clustern den Speicherort zusätzlicher Bibliotheken zum Verwenden mit Hive anzugeben. Diese Bibliotheken sind nach der Clustererstellung automatisch für die Verwendung mit Hive-Abfragen verfügbar, ohne dass `ADD JAR`verwendet werden muss.

Die Bootstrap-Funktion für Linux-basierte Cluster bietet diese Funktionalität nicht. Verwenden Sie stattdessen eine Skriptaktion, wie in [Hinzufügen von Hive-Bibliotheken während der Erstellung des HDInsight-Clusters](hdinsight-hadoop-add-hive-libraries.md)beschrieben.

### <a name="virtual-networks"></a>Virtuelle Netzwerke
Windows-basierte HDInsight-Cluster funktionieren nur mit klassischen virtuellen Netzwerken, während für Linux-basierte HDInsight-Cluster virtuelle Resource Manager-Netzwerke erforderlich sind. Wenn Sie über Ressourcen in einem klassischen virtuellen Netzwerk verfügen, mit dem sich der Linux-basierte HDInsight-Cluster verbinden muss, finden Sie weitere Informationen dazu unter [Verbinden von virtuellen Netzwerken aus unterschiedlichen Bereitstellungsmodellen im Portal](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

Weitere Informationen zu den Konfigurationsanforderungen für das Verwenden von Azure Virtual Networks mit HDInsight finden Sie unter [Erweitern der HDInsight-Funktionen mit Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md).

## <a name="management-and-monitoring"></a>Verwaltung und Überwachung
Viele der Web-Benutzeroberflächen, die Sie möglicherweise mit Windows-basiertem HDInsight verwendet haben, wie Job History (Auftragsverlauf) oder die Yarn-Benutzeroberfläche, sind über Ambari verfügbar. Darüber hinaus bietet die Ambari-Hive-Ansicht eine Möglichkeit zum Ausführen von Hive-Abfragen mithilfe Ihres Webbrowsers. Die Ambari-Webbenutzeroberfläche ist auf Linux-basierten Clustern verfügbar (unter „https://CLUSTERNAME.azurehdinsight.net“).

Weitere Informationen zum Arbeiten mit Ambari finden Sie in den folgenden Dokumenten:

* [Ambari-Web](hdinsight-hadoop-manage-ambari.md)
* [Ambari-REST-API](hdinsight-hadoop-manage-ambari-rest-api.md)

### <a name="ambari-alerts"></a>Ambari-Warnungen
Ambari verfügt über ein Warnsystem, das Sie über potenzielle Probleme mit dem Cluster informiert. Warnungen werden als rote oder gelbe Einträge in der Ambari-Webbenutzeroberfläche angezeigt, Sie können sie aber auch über die REST-API abrufen.

> [!IMPORTANT]
> Ambari-Warnungen weisen darauf hin, dass *möglicherweise* ein Problem besteht, nicht, dass ein Problem *tatsächlich* besteht. Zum Beispiel erhalten Sie möglicherweise eine Warnung, dass der Zugriff auf HiveServer2 nicht möglich ist, obwohl Sie normal darauf zugreifen können.
>
> Viele Warnungen werden als intervallbasierte Abfragen bei einem Dienst implementiert und erwarten innerhalb eines bestimmten Zeitrahmens eine Antwort. Die Warnung bedeutet also nicht zwingend, dass der Dienst nicht verfügbar ist, sondern nur, dass im erwarteten Zeitrahmen keine Ergebnisse zurückgegeben wurden.

Im Allgemeinen sollten Sie vor einer Reaktion auf eine Warnung überprüfen, ob diese schon seit längerer Zeit besteht oder ob sie bereits gemeldete Benutzerprobleme widerspiegelt.

## <a name="file-system-locations"></a>Dateisystem-Speicherorte
Das Linux-Cluster-Dateisystem ist anders angeordnet als Windows-basierte HDInsight-Cluster. Verwenden Sie die folgende Tabelle, um häufig verwendete Dateien zu suchen.

| Ich muss ... finden | Sie befinden/befindet sich unter ... |
| --- | --- |
| Konfiguration |`/etc`. Beispiel: `/etc/hadoop/conf/core-site.xml` |
| Protokolldateien |`/var/logs` |
| Hortonworks Data Platform (HDP) |`/usr/hdp`. Hier befinden sich zwei Verzeichnisse, die aktuelle HDP-Version und `current`. Das `current`-Verzeichnis enthält symbolische Verknüpfungen zu Dateien und Verzeichnissen im Versionsnummernverzeichnis. Das `current`-Verzeichnis wird als einfache Möglichkeit des Zugriffs auf HDP-Dateien bereitgestellt, da sich die Versionsnummer beim Aktualisieren der HDP-Version ändert. |
| hadoop-streaming.jar |`/usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar` |

Im Allgemeinen sollten Sie, falls Sie den Namen der Datei kennen, in einer SSH-Sitzung den folgenden Befehl verwenden, um den Dateipfad zu suchen:

    find / -name FILENAME 2>/dev/null

Sie können auch Platzhalter mit dem Dateinamen verwenden. Beispiel: `find / -name *streaming*.jar 2>/dev/null` gibt den Pfad für alle JAR-Dateien zurück, die als Teil des Dateinamens das Wort „Streaming“ enthalten.

## <a name="hive-pig-and-mapreduce"></a>Hive, Pig und MapReduce.

Pig- und MapReduce-Workloads sind auf Linux-basierten Clustern sehr ähnlich. Der einzige Unterschied ist die Art der Verbindungsherstellung zu den Clusterhauptknoten. Weitere Informationen finden Sie in den folgenden Dokumenten:

* [Verwenden von Pig mit SSH](hdinsight-hadoop-use-pig-ssh.md)
* [Verwenden von MapReduce mit SSH](hdinsight-hadoop-use-mapreduce-ssh.md)

### <a name="hive"></a>Hive
Das folgende Diagramm enthält Hilfestellungen zum Migrieren Ihrer Hive-Workloads:

| Auf Windows-basierten Clustern verwende ich ... | Auf Linux-basierten Clustern verwende ich ... |
| --- | --- |
| **Hive-Editor** |[Hive-Ansicht in Ambari](hdinsight-hadoop-use-hive-ambari-view.md) |
| `set hive.execution.engine=tez;` zum Aktivieren von Tez |Tez ist das Standardausführungsmodul für Linux-basierte Cluster. Die SET-Anweisung wird also nicht mehr benötigt. |
| CMD-Dateien oder -Skripts auf dem Server, die als Teil eines Hive-Auftrags aufgerufen wurden |Verwenden von Bash-Skripts |
| `hive` -Befehl von Remotedesktop |Verwenden von [Beeline](hdinsight-hadoop-use-hive-beeline.md) oder [Hive in einer SSH-Sitzung](hdinsight-hadoop-use-hive-ssh.md) |

## <a name="storm"></a>Storm
| Auf Windows-basierten Clustern verwende ich ... | Auf Linux-basierten Clustern verwende ich ... |
| --- | --- |
| Storm-Dashboard |Das Storm-Dashboard ist nicht verfügbar. Methoden zum Übermitteln von Topologien finden Sie unter [Bereitstellen und Verwalten von Apache Storm-Topologien in HDInsight unter Linux](hdinsight-storm-deploy-monitor-topology-linux.md) . |
| Storm-Benutzeroberfläche |Die Storm-Benutzeroberfläche ist unter „https://CLUSTERNAME.azurehdinsight.net/stormui“ verfügbar. |
| Visual Studio zum Erstellen, Bereitstellen und Verwalten von C#- oder Hybridtopologien |Visual Studio kann verwendet werden, um C#-Topologien (SCP.NET) oder Hybridtopologien auf Linux-basierten Clustern mit Storm in HDInsight zu erstellen, bereitzustellen und zu verwalten, die nach dem 28.10.2016 erstellt wurden. |

## <a name="hbase"></a>HBase
Auf Linux-basierten Clustern ist `/hbase-unsecure`der übergeordnete ZNode für HBase. Legen Sie dies in der Konfiguration für alle Java-Clientanwendungen fest, die native HBase-Java-API verwenden.

Einen Beispiel-Client, der diesen Wert festlegt, finden Sie unter [Verwenden von Maven zur Entwicklung von Java-Anwendungen, die HBase mit HDInsight (Hadoop) nutzen](hdinsight-hbase-build-java-maven.md) .

## <a name="spark"></a>Spark
Spark-Cluster waren während der Vorschau auf Windows-Clustern verfügbar. Spark GA ist nur mit Linux-basierten Clustern verfügbar. Es existiert kein Migrationspfad von einer Vorabversion eines Windows-basierten Spark-Clusters zu einem veröffentlichten Linux-basierten Spark-Cluster.

## <a name="known-issues"></a>Bekannte Probleme
### <a name="azure-data-factory-custom-net-activities"></a>Benutzerdefinierte Azure Data Factory-.NET-Aktivitäten
Benutzerdefinierte Azure Data Factory-.NET-Aktivitäten werden auf Linux-basierten HDInsight-Clustern derzeit nicht unterstützt. Stattdessen sollten Sie eine der folgenden Methoden zum Implementieren von benutzerdefinierten Aktivitäten als Teil Ihrer ADF-Pipeline verwenden.

* Führen Sie .NET-Aktivitäten im Azure Batch-Pool aus. Informationen hierzu finden Sie im Abschnitt „Verwenden des mit Azure Batch verknüpften Dienstes“ im Artikel [Verwenden von benutzerdefinierten Aktivitäten in einer Azure Data Factory-Pipeline](../data-factory/data-factory-use-custom-activities.md)
* Implementieren Sie die Aktivität als MapReduce-Aktivität. Weitere Informationen finden Sie unter [Aufrufen von MapReduce-Programmen über Data Factory](../data-factory/data-factory-map-reduce.md).

### <a name="line-endings"></a>Zeilenenden
Im Allgemeinen verwenden Zeilenenden in Windows-basierten Systemen CRLF und in Linux-basierten Systemen LF. Wenn Sie Daten mit CRLF-Zeilenenden erzeugen oder erwarten, müssen Sie die Producer oder Consumer möglicherweise dahingehend ändern, dass sie mit dem LF-Zeilenende arbeiten.

Zum Beispiel werden Daten beim Verwenden von Azure PowerShell zum Abfragen von HDInsight auf einem Windows-basierten Cluster Daten mit CRLF zurückgeben. Dieselbe Abfrage gibt bei einem Linux-basierten Cluster LF zurück. Führen Sie vor der Migration auf einen Linux-basierten Cluster einen Test durch, um zu sehen, ob dies ein Problem mit Ihrer Lösung verursacht.

Wenn Sie über Skripts verfügen, die direkt auf den Linux-Knoten ausgeführt werden, sollten Sie LF stets als unterste Zeile verwenden. Wenn Sie CRLF verwenden, entdecken Sie möglicherweise Fehler beim Ausführen der Skripts auf einem Linux-basierten Cluster.

Wenn Sie wissen, dass die Skripts keine Zeichenfolgen mit eingebetteten CR-Zeichen enthalten, können Sie die Zeilenenden massenweise mithilfe einer der folgenden Methoden ändern:

* **Wenn Sie Skripts haben, die Sie in den Cluster hochladen möchten**, verwenden Sie die folgenden PowerShell-Anweisungen zum Ändern der Zeilenenden von CRLF zu LF, bevor Sie das Skript in den Cluster hochladen.

      $original_file ='c:\path\to\script.py'
      $text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
      [IO.File]::WriteAllText($original_file, $text)
* **Wenn Sie Skripts haben, die sich bereits im vom Cluster verwendeten Speicher befinden**, können Sie den folgenden Befehl aus einer SSH-Sitzung für den Linux-basierten Cluster zum Ändern des Skripts verwenden.

      hdfs dfs -get wasbs:///path/to/script.py oldscript.py
      tr -d '\r' < oldscript.py > script.py
      hdfs dfs -put -f script.py wasbs:///path/to/script.py

## <a name="next-steps"></a>Nächste Schritte
* [Erfahren Sie, wie Sie Linux-basierte HDInsight-Cluster erstellen](hdinsight-hadoop-provision-linux-clusters.md)
* [Verbinden zu einem Linux-basierten Cluster mithilfe von SSH auf einem Windows-Client](hdinsight-hadoop-linux-use-ssh-windows.md)
* [Verbinden zu einem Linux-basierten Cluster mithilfe von SSH auf einem Linux-, Unix- oder Mac-Client](hdinsight-hadoop-linux-use-ssh-unix.md)
* [Verwalten eines Linux-basierten Clusters mithilfe von Ambari](hdinsight-hadoop-manage-ambari.md)

