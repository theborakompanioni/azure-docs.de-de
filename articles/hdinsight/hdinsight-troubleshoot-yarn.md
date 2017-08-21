---
title: "Problembehandlung bei YARN – Azure HDInsight | Microsoft-Dokumentation"
description: "Verwenden Sie die Yarn-FAQ für Antworten auf häufig gestellte Fragen zu Yarn auf der Azure HDInsight-Plattform."
keywords: "Azure HDInsight, Yarn, FAQ, Problembehandlungshandbuch, häufig gestellte Fragen"
services: Azure HDInsight
documentationcenter: na
author: arijitt
manager: 
editor: 
ms.assetid: F76786A9-99AB-4B85-9B15-CA03528FC4CD
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/7/2017
ms.author: arijitt
ms.translationtype: HT
ms.sourcegitcommit: f76de4efe3d4328a37f86f986287092c808ea537
ms.openlocfilehash: 576d6bfe9697d2abd6f84824960dad62e9ed3c11
ms.contentlocale: de-de
ms.lasthandoff: 07/10/2017


---

# <a name="yarn-troubleshooting"></a>Problembehandlung bei YARN

Dieser Artikel beschreibt die wichtigsten Probleme und ihre Lösungen für die Arbeit mit Yarn-Nutzlasten in Apache Ambari.

## <a name="how-do-i-create-a-new-yarn-queue-on-a-cluster"></a>Wie erstelle ich eine neue Yarn-Warteschlange auf einem Cluster?

### <a name="issue"></a>Problem:

Erstellen einer neuen Yarn-Warteschlange mit Kapazitätszuordnung auf einem HDInsight-Cluster.  

### <a name="resolution-steps"></a>Lösungsschritte: 

Erstellen Sie mit den folgenden Schritten über Ambari eine neue Yarn-Warteschlange, und teilen Sie die Kapazitätszuordnung gleichmäßig auf alle Warteschlangen auf. 

In diesem Beispiel wird die Kapazität zweier vorhandener Warteschlangen („default“ und „thriftsvr“) jeweils von 50% in 25% geändert, sodass die neue Warteschlange („spark“) eine Kapazität von 50% hat.
| Warteschlange | Kapazität | Maximal Kapazität |
| --- | --- | --- | --- |
| die Standardeinstellung | 25% | 50% |
| thrftsvr | 25% | 50% |
| spark | 50% | 50% |
1. Klicken Sie auf das Ambari Views-Symbol, das Gittermuster, und wählen Sie **Yarn Queue Manager**.

    ![Klicken Sie auf das Ambari Views-Symbol.](media/hdinsight-troubleshoot-yarn/create-queue-1.png)
1. Wählen Sie die Warteschlange **default** aus.

    ![„default“-Warteschlange auswählen](media/hdinsight-troubleshoot-yarn/create-queue-2.png)
1. Ändern Sie **Capacity** für die Warteschlange **default** von 50% in 25% und für die Warteschlange **thriftsvr** auch in 25%.

    ![Kapazität für die Warteschlangen „default“ und „thriftsvr“ in 25% ändern](media/hdinsight-troubleshoot-yarn/create-queue-3.png)
1. Klicken Sie auf **Add Queue**, um eine neue Warteschlange zu erstellen.

    ![Auf „Add Queue“ klicken](media/hdinsight-troubleshoot-yarn/create-queue-4.png)
1. Benennen Sie die neue Warteschlange.

    ![Neue Warteschlange „spark“ nennen](media/hdinsight-troubleshoot-yarn/create-queue-5.png)  
1. Lassen Sie die **Capacity**-Werte auf 50%, und klicken Sie auf die Schaltfläche **Actions**.

    ![Auf Schaltfläche „Actions“ klicken](media/hdinsight-troubleshoot-yarn/create-queue-6.png)  
1. Wählen Sie **Save and Refresh Queues**.

    ![„Save and Refresh Queues“ wählen](media/hdinsight-troubleshoot-yarn/create-queue-7.png)  

Diese Änderungen werden sofort auf der Yarn-Scheduler-Benutzeroberfläche angezeigt.

### <a name="further-reading"></a>Weitere nützliche Informationen:

- [Yarn Capacity Scheduler](https://hadoop.apache.org/docs/r2.7.2/hadoop-yarn/hadoop-yarn-site/CapacityScheduler.html) (Yarn-Kapazitätsplaner)


## <a name="how-do-i-download-yarn-logs-from-a-cluster"></a>Wie lade ich Yarn-Protokolle aus einem Cluster herunter?

#### <a name="issue"></a>Problem:

Ich muss den Yarn-Anwendungsmaster und andere Containerprotokolle vom HDInsight-Cluster herunterladen.  

#### <a name="resolution-steps"></a>Lösungsschritte: 

1. Stellen Sie mit einem Secure Shell-Client (SSH) eine Verbindung mit dem HDInsight-Cluster her (beachten Sie „Weitere nützliche Informationen“ im Abschnitt unten).
1. Listen Sie alle Anwendungs-IDs der aktuell ausgeführten Yarn-Anwendungen mit dem folgenden Befehl auf:

```apache
yarn top
```
Die IDs werden in der `APPLICATIONID`-Spalte aufgelistet, deren Protokolle heruntergeladen werden müssen.

```apache
YARN top - 18:00:07, up 19d, 0:14, 0 active users, queue(s): root
NodeManager(s): 4 total, 4 active, 0 unhealthy, 0 decommissioned, 0 lost, 0 rebooted
Queue(s) Applications: 2 running, 10 submitted, 0 pending, 8 completed, 0 killed, 0 failed
Queue(s) Mem(GB): 97 available, 3 allocated, 0 pending, 0 reserved
Queue(s) VCores: 58 available, 2 allocated, 0 pending, 0 reserved
Queue(s) Containers: 2 allocated, 0 pending, 0 reserved

                  APPLICATIONID USER             TYPE      QUEUE   #CONT  #RCONT  VCORES RVCORES     MEM    RMEM  VCORESECS    MEMSECS %PROGR       TIME NAME
 application_1490377567345_0007 hive            spark  thriftsvr       1       0       1       0      1G      0G    1628407    2442611  10.00   18:20:20 Thrift JDBC/ODBC Server
 application_1490377567345_0006 hive            spark  thriftsvr       1       0       1       0      1G      0G    1628430    2442645  10.00   18:20:20 Thrift JDBC/ODBC Server
```

Laden Sie Yarn-Containerprotokolle für alle Anwendungsmaster mit folgendem Befehl herunter:
   
```apache
yarn logs -applicationIdn logs -applicationId <application_id> -am ALL > amlogs.txt
```

Dadurch wird die Protokolldatei mit dem Namen `amlogs.txt` im Textformat erstellt. 

Laden Sie Yarn-Containerprotokolle nur für den neuesten Anwendungsmaster mit folgendem Befehl herunter:

```apache
yarn logs -applicationIdn logs -applicationId <application_id> -am -1 > latestamlogs.txt
```

Dadurch wird die Protokolldatei mit dem Namen `latestamlogs.txt` im Textformat erstellt. 

Laden Sie YARN-Containerprotokolle für die ersten beiden Anwendungsmaster mit folgendem Befehl herunter:

```apache
yarn logs -applicationIdn logs -applicationId <application_id> -am 1,2 > first2amlogs.txt 
```

Dadurch wird die Protokolldatei mit dem Namen `first2amlogs.txt` im Textformat erstellt. 

Laden Sie alle Yarn-Containerprotokolle mit dem folgenden Befehl herunter:

```apache
yarn logs -applicationIdn logs -applicationId <application_id> > logs.txt
```

Dadurch wird die Protokolldatei mit dem Namen `logs.txt` im Textformat erstellt. 

Laden Sie ein Yarn-Containerprotokoll für einen bestimmten Container mit folgendem Befehl herunter:

```apache
yarn logs -applicationIdn logs -applicationId <application_id> -containerId <container_id> > containerlogs.txt 
```

Dadurch wird die Protokolldatei mit dem Namen `containerlogs.txt` im Textformat erstellt.

#### <a name="further-readings"></a>Weitere nützliche Informationen:

- [Herstellen einer Verbindung mit HDInsight (Hadoop) per SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix)

- [Apache Hadoop Yarn – Konzepte und -Anwendungen](https://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/)









