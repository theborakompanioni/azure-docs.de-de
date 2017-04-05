---
title: Zugreifen auf YARN-Anwendungsprotokolle unter Linux-basiertem HDInsight | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie auf einem Linux-basierten HDInsight (Hadoop)-Cluster mithilfe der Befehlszeile und eines Webbrowsers auf YARN-Anwendungsprotokolle zugreifen.
services: hdinsight
documentationcenter: 
tags: azure-portal
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 3ec08d20-4f19-4a8e-ac86-639c04d2f12e
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 1d568bc6ab8f2801d575d726352f4c68e1f9277a
ms.lasthandoff: 03/25/2017


---
# <a name="access-yarn-application-logs-on-linux-based-hdinsight"></a>Zugriff auf YARN-Anwendungsprotokolle unter Linux-basiertem HDInsight
Dieses Dokument erläutert den Zugriff auf Protokolle für YARN (Yet Another Resource Negotiator)-Anwendungen, die auf einem Hadoop-Cluster in Azure HDInsight abgeschlossen wurden.

> [!IMPORTANT]
> Die Schritte in diesem Dokument erfordern einen HDInsight-Cluster mit Linux. Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight unter Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date).

## <a name="prerequisites"></a>Voraussetzungen
* Ein Linux-basierter HDInsight-Cluster.
* Sie müssen einen [SSH-Tunnel erstellen](hdinsight-linux-ambari-ssh-tunnel.md) , bevor Sie auf die Webbenutzeroberfläche für ResourceManager-Protokolle zugreifen können.

## <a name="YARNTimelineServer"></a>YARN Timeline Server
Der [YARN Timeline Server](http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html) bietet über zwei Schnittstellen allgemeine Informationen zu abgeschlossenen Anwendungen sowie frameworkspezifische Anwendungsinformationen. Dies bedeutet Folgendes:

* Speicherung und Abruf allgemeiner Anwendungsinformationen zu HDInsight-Clustern sind für Versionen ab 3.1.1.374 aktiviert.
* Die frameworkspezifische Anwendungsinformationskomponente des Timeline Servers steht für HDInsight-Cluster derzeit nicht zur Verfügung.

Zu den allgemeinen Informationen zu Anwendungen zählen die folgenden Datenarten:

* Die Anwendungs-ID, ein eindeutiger Bezeichner einer Anwendung
* Der Benutzer, der die Anwendung gestartet hat
* Informationen zu den erfolgten Versuchen, die Anwendung abzuschließen
* Die bei Anwendungsversuchen verwendeten Container

## <a name="YARNAppsAndLogs"></a>YARN-Anwendungen und -Protokolle

YARN unterstützt mehrere Programmierungsmodelle (u. a. MapReduce), indem die Ressourcenverwaltung von der Zeitplanung/Überwachung von Anwendungen getrennt wird. Dies erfolgt über einen globalen *ResourceManager* (RM), workerknotenbezogene *NodeManager* (NMs) und anwendungsbezogene *ApplicationMaster* (AMs). Der anwendungsbezogene AM handelt Ressourcen (CPU, Arbeitsspeicher, Datenträger, Netzwerk) für die Ausführung Ihrer Anwendung mit dem RM aus. Der RM arbeitet mit NMs zusammen, um diese Ressourcen zu gewähren, die als *Container* zugewiesen werden. Der AM ist zuständig für die Nachverfolgung des Status der Container, die ihm vom RM zugewiesen wurden. Je nach Art der Anwendung kann diese viele Container benötigen.

Jede Anwendung kann aus mehreren *Anwendungsversuchen* bestehen. Dies ermöglicht einer Anwendung das Wiederholen eines Vorgangs, wenn es zu einem Absturz oder einem Verlust der Kommunikation zwischen einem AM und einem RM kommt. Jeder Versuch wird in einem Container ausgeführt. In gewisser Weise stellt ein Container den Kontext für die Standardeinheit für Aufgaben, die von einer YARN-Anwendung ausgeführt werden. Alle Aufgaben, die im Kontext eines Containers erledigt werden, erfolgen auf dem einzelnen Workerknoten, auf dem der Container zugeordnet wurde. Weitere Informationen finden Sie unter [YARN-Konzepte][YARN-concepts].

Anwendungsprotokolle (und dazugehörige Containerprotokolle) sind für das Beheben von Problemen bei Hadoop-Anwendungen besonders wichtig. YARN bietet mit seinem Feature [Log Aggregation][log-aggregation] ein nützliches Gerüst für das Sammeln, Zusammenführen und Speichern von Anwendungsprotokollen. Durch die Protokollaggregationsfunktion wird der Zugriff auf Anwendungsprotokolle deterministischer. Sie aggregiert Protokolle in allen Containern auf einem Workerknoten und speichert sie als eine aggregierte Protokolldatei pro Workerknoten. Das Protokoll wird, nachdem eine Anwendung beendet wurde, im Standarddateisystem gespeichert. Ihre Anwendung mag Hunderte oder Tausende von Containern verwenden, doch Protokolle für alle auf einem einzelnen Arbeitsknoten vorhandenen Container werden zu einer zentralen Datei zusammengeführt. Dies mündet in einer Protokolldatei pro Workerknoten, die von Ihrer Anwendung genutzt wird. Log Aggregation ist für HDInsight-Cluster (ab Version 3.0) standardmäßig aktiviert. Zusammengeführte Protokolle befinden sich im Standardcontainer Ihres Clusters am folgenden Speicherort:

    wasbs:///app-logs/<user>/logs/<applicationId>

An diesem Speicherort ist *user* der Name des Benutzers, der die Anwendung gestartet hat. *applicationId* ist der eindeutige Bezeichner einer Anwendung gemäß der Zuweisung durch den YARN Resource Manager (RM).

Die zusammengeführten Protokolle sind nicht unmittelbar lesbar, da sie in [TFile][T-file] geschrieben werden, einem nach Container indiziertem [Binärformat][binary-format]. Sie müssen die YARN-ResourceManager-Protokolle oder CLI-Tools verwenden, um diese Protokolle für relevante Anwendungen oder Container im Nur-Text-Format anzuzeigen. 

## <a name="yarn-cli-tools"></a>YARN-CLI-Tools

Zur Verwendung der YARN CLI-Tools müssen Sie zuerst über SSH eine Verbindung mit dem HDInsight-Cluster herstellen. Informationen hierzu finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md).

Sie können diese Protokolle im Nur-Text-Format anzeigen, indem Sie einen der folgenden Befehle ausführen:

    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>

Beim Ausführen dieser Befehle müssen Sie &lt;AnwendungsID>, den &lt;Benutzer,-der-die-Anwendung-gestartet-hat>, die &lt;ContainerID> und die &lt;Workerknotenadresse> angeben.

## <a name="yarn-resourcemanager-ui"></a>YARN-ResourceManager-Benutzeroberfläche
Die YARN-ResourceManager-Benutzeroberfläche wird im Hauptknoten des Clusters ausgeführt, und der Zugriff erfolgt über die Ambari-Webbenutzeroberfläche; Sie müssen jedoch erst einen [SSH-Tunnel erstellen](hdinsight-linux-ambari-ssh-tunnel.md) , bevor Sie auf die ResourceManager-Benutzeroberfläche zugreifen können.

Nachdem Sie einen SSH-Tunnel erstellt haben, zeigen Sie die YARN-Protokolle mithilfe folgender Schritte an:

1. Navigieren Sie in Ihrem Webbrowser zu „https://CLUSTERNAME.azurehdinsight.net“. Ersetzen Sie CLUSTERNAME durch den Namen Ihres HDInsight-Clusters.
2. Wählen Sie aus der Liste der Dienste auf der linken Seite den Dienst **YARN**aus.
   
    ![Ausgewählter YARN-Dienst](./media/hdinsight-hadoop-access-yarn-app-logs-linux/yarnservice.png)
3. Wählen Sie aus der Dropdownliste **QuickLinks** einen der Clusterhauptknoten und dann **ResourceManager-Protokoll** aus.
   
    ![YARN-Direktlinks](./media/hdinsight-hadoop-access-yarn-app-logs-linux/yarnquicklinks.png)
   
    Eine Liste mit Links zu YARN-Protokollen wird angezeigt.

[YARN-timeline-server]:http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html
[log-aggregation]:http://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/
[T-file]:https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf
[binary-format]:https://issues.apache.org/jira/browse/HADOOP-3315
[YARN-concepts]:http://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/

