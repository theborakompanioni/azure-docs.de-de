---
title: "Problembehandlung bei HBase – Azure HDInsight | Microsoft-Dokumentation"
description: "Problembehandlung zur Ermittlung der Ursache von Lücken in der Kette der Regionen."
services: hdinsight
documentationcenter: 
author: nitinver
manager: ashitg
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 7/7/2017
ms.author: nitinver
ms.translationtype: HT
ms.sourcegitcommit: f76de4efe3d4328a37f86f986287092c808ea537
ms.openlocfilehash: 29b1776d6b0c456515738aae3c5fd836c9a0295d
ms.contentlocale: de-de
ms.lasthandoff: 07/10/2017

---

# <a name="hbase-troubleshooting"></a>Problembehandlung bei HBASE

Dieser Artikel beschreibt die wichtigsten Probleme und ihre Lösungen für die Arbeit mit HBASE-Nutzlasten in Apache Ambari.

## <a name="how-do-i-run-hbck-command-reports-with-multiple-unassigned-regions"></a>Wie führe ich hbck-Befehlsberichte mit mehreren nicht zugewiesenen Regionen aus?

Es ist ein häufig auftretendes Problem, dass „mehrere Regionen nicht zugewiesen werden oder Lücken in der Kette der Regionen auftreten“, wenn der HBase-Benutzer den Befehl „hbase hbck“ ausführt.

Der Benutzer sieht in der HBase-Masterbenutzeroberfläche, dass die Anzahl der Regionen auf allen Regionsservern unausgewogen ist. Danach kann der Benutzer den Befehl „hbase hbck“ ausführen und erkennt Lücken in der Regionskette.

Der Benutzer sollte zunächst die Zuweisungen beheben, da Lücken darauf zurückzuführen sein können, dass diese Regionen offline sind. 

Bringen Sie die nicht zugewiesenen Regionen mit folgenden Schritten wieder in den normalen Zustand:

1. Melden Sie sich mit SSH beim HDInsight HBase-Cluster an.
1. Stellen Sie mit dem Befehl „hbase zkcli“ eine Verbindung mit der Zookeeper-Shell her.
1. Führen Sie den Befehl „rmr /hbase/regions-in-transition“ oder „rmr /hbase-unsecure/regions-in-transition“ aus.
1. Verlassen Sie die „hbase zkcli“-Shell mit dem Befehl „exit“.
1. Öffnen Sie die Ambari-Benutzeroberfläche, und starten Sie den Active HBase Master-Dienst von Ambari aus neu.
1. Führen Sie den Befehl „hbase hbck“ erneut aus (ohne weitere Optionen).

Überprüfen Sie die Ausgabe des Befehls in Schritt 6, und stellen Sie sicher, dass alle Regionen zugewiesen wurden.

---

## <a name="how-do-i-fix-timeout-issues-with-hbck-commands-for-region-assignments"></a>Wie behebe ich Timeoutprobleme mit hbck-Befehlen für Regionszuweisungen?

### <a name="probable-cause"></a>Wahrscheinliche Ursache

Die potenzielle Ursache hierfür ist möglicherweise, dass mehrere Regionen sich für einen langen Zeitraum im Status „im Übergang“ befinden. Diese Regionen werden in der HBase Master-Benutzeroberfläche als offline angezeigt. Aufgrund der großen Anzahl von Regionen, die den Übergang versuchen, könnte ein HBase Master-Timeout aufgetreten sein, sodass HBase Master diese Regionen nicht wieder in den Onlinezustand versetzen kann.

### <a name="resolution-steps"></a>Lösungsschritte

Mit folgenden Schritten beheben Sie das hbck-Timeoutproblem:

1. Melden Sie sich mit SSH beim HDInsight HBase-Cluster an.
1. Stellen Sie mit dem Befehl „hbase zkcli“ eine Verbindung mit der Zookeeper-Shell her.
1. Führen Sie den Befehl „rmr /hbase/regions-in-transition“ oder „rmr /hbase-unsecure/regions-in-transition“ aus.
1. Verlassen Sie die „hbase zkcli“-Shell mit dem Befehl „exit“.
1. Öffnen Sie die Ambari-Benutzeroberfläche, und starten Sie den Active HBase Master-Dienst von Ambari aus neu.
1. Führen Sie den Befehl „hbase hbck -fixAssignments“ erneut aus, und dabei sollte kein Timeout mehr auftreten.

## <a name="how-do-i-force-disable-hdfs-safe-mode-in-an-cluster"></a>Wie erzwinge ich das Deaktivieren des abgesicherten HDFS-Modus in einem Cluster?

### <a name="issue"></a>Problem:

Lokales HDFS ist im abgesicherten Modus auf einem HDInsight-Cluster hängen geblieben.   

### <a name="detailed-description"></a>Detaillierte Beschreibung:

Fehler beim Ausführen des einfachen HDFS-Befehls wie folgt:

```apache
hdfs dfs -D "fs.default.name=hdfs://mycluster/" -mkdir /temp
```

Bei dem Versuch, den obigen Befehl auszuführen, ist folgender Fehler aufgetreten:

```apache
hdiuser@hn0-spark2:~$ hdfs dfs -D "fs.default.name=hdfs://mycluster/" -mkdir /temp
17/04/05 16:20:52 WARN retry.RetryInvocationHandler: Exception while invoking ClientNamenodeProtocolTranslatorPB.mkdirs over hn0-spark2.2oyzcdm4sfjuzjmj5dnmvscjpg.dx.internal.cloudapp.net/10.0.0.22:8020. Not retrying because try once and fail.
org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.hdfs.server.namenode.SafeModeException): Cannot create directory /temp. Name node is in safe mode.
It was turned on manually. Use "hdfs dfsadmin -safemode leave" to turn safe mode off.
        at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.checkNameNodeSafeMode(FSNamesystem.java:1359)
        at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.mkdirs(FSNamesystem.java:4010)
        at org.apache.hadoop.hdfs.server.namenode.NameNodeRpcServer.mkdirs(NameNodeRpcServer.java:1102)
        at org.apache.hadoop.hdfs.protocolPB.ClientNamenodeProtocolServerSideTranslatorPB.mkdirs(ClientNamenodeProtocolServerSideTranslatorPB.java:630)
        at org.apache.hadoop.hdfs.protocol.proto.ClientNamenodeProtocolProtos$ClientNamenodeProtocol$2.callBlockingMethod(ClientNamenodeProtocolProtos.java)
        at org.apache.hadoop.ipc.ProtobufRpcEngine$Server$ProtoBufRpcInvoker.call(ProtobufRpcEngine.java:640)
        at org.apache.hadoop.ipc.RPC$Server.call(RPC.java:982)
        at org.apache.hadoop.ipc.Server$Handler$1.run(Server.java:2313)
        at org.apache.hadoop.ipc.Server$Handler$1.run(Server.java:2309)
        at java.security.AccessController.doPrivileged(Native Method)
        at javax.security.auth.Subject.doAs(Subject.java:422)
        at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1724)
        at org.apache.hadoop.ipc.Server$Handler.run(Server.java:2307)
        at org.apache.hadoop.ipc.Client.getRpcResponse(Client.java:1552)
        at org.apache.hadoop.ipc.Client.call(Client.java:1496)
        at org.apache.hadoop.ipc.Client.call(Client.java:1396)
        at org.apache.hadoop.ipc.ProtobufRpcEngine$Invoker.invoke(ProtobufRpcEngine.java:233)
        at com.sun.proxy.$Proxy10.mkdirs(Unknown Source)
        at org.apache.hadoop.hdfs.protocolPB.ClientNamenodeProtocolTranslatorPB.mkdirs(ClientNamenodeProtocolTranslatorPB.java:603)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:498)
        at org.apache.hadoop.io.retry.RetryInvocationHandler.invokeMethod(RetryInvocationHandler.java:278)
        at org.apache.hadoop.io.retry.RetryInvocationHandler.invoke(RetryInvocationHandler.java:194)
        at org.apache.hadoop.io.retry.RetryInvocationHandler.invoke(RetryInvocationHandler.java:176)
        at com.sun.proxy.$Proxy11.mkdirs(Unknown Source)
        at org.apache.hadoop.hdfs.DFSClient.primitiveMkdir(DFSClient.java:3061)
        at org.apache.hadoop.hdfs.DFSClient.mkdirs(DFSClient.java:3031)
        at org.apache.hadoop.hdfs.DistributedFileSystem$24.doCall(DistributedFileSystem.java:1162)
        at org.apache.hadoop.hdfs.DistributedFileSystem$24.doCall(DistributedFileSystem.java:1158)
        at org.apache.hadoop.fs.FileSystemLinkResolver.resolve(FileSystemLinkResolver.java:81)
        at org.apache.hadoop.hdfs.DistributedFileSystem.mkdirsInternal(DistributedFileSystem.java:1158)
        at org.apache.hadoop.hdfs.DistributedFileSystem.mkdirs(DistributedFileSystem.java:1150)
        at org.apache.hadoop.fs.FileSystem.mkdirs(FileSystem.java:1898)
        at org.apache.hadoop.fs.shell.Mkdir.processNonexistentPath(Mkdir.java:76)
        at org.apache.hadoop.fs.shell.Command.processArgument(Command.java:273)
        at org.apache.hadoop.fs.shell.Command.processArguments(Command.java:255)
        at org.apache.hadoop.fs.shell.FsCommand.processRawArguments(FsCommand.java:119)
        at org.apache.hadoop.fs.shell.Command.run(Command.java:165)
        at org.apache.hadoop.fs.FsShell.run(FsShell.java:297)
        at org.apache.hadoop.util.ToolRunner.run(ToolRunner.java:76)
        at org.apache.hadoop.util.ToolRunner.run(ToolRunner.java:90)
        at org.apache.hadoop.fs.FsShell.main(FsShell.java:350)
mkdir: Cannot create directory /temp. Name node is in safe mode.
```

### <a name="probable-cause"></a>Wahrscheinliche Ursache:

Der HDInsight-Cluster wurde auf sehr wenige Knoten unterhalb oder in der Nähe des HDFS-Replikationsfaktors zentral herunterskaliert.

### <a name="resolution-steps"></a>Lösungsschritte: 

- Melden Sie den HDFS-Status auf dem HDInsight-Cluster mithilfe der folgenden Befehle:

```apache
hdfs dfsadmin -D "fs.default.name=hdfs://mycluster/" -report
```

```apache
hdiuser@hn0-spark2:~$ hdfs dfsadmin -D "fs.default.name=hdfs://mycluster/" -report
Safe mode is ON
Configured Capacity: 3372381241344 (3.07 TB)
Present Capacity: 3138625077248 (2.85 TB)
DFS Remaining: 3102710317056 (2.82 TB)
DFS Used: 35914760192 (33.45 GB)
DFS Used%: 1.14%
Under replicated blocks: 0
Blocks with corrupt replicas: 0
Missing blocks: 0
Missing blocks (with replication factor 1): 0

-------------------------------------------------
Live datanodes (8):

Name: 10.0.0.17:30010 (10.0.0.17)
Hostname: 10.0.0.17
Decommission Status : Normal
Configured Capacity: 421547655168 (392.60 GB)
DFS Used: 5288128512 (4.92 GB)
Non DFS Used: 29087272960 (27.09 GB)
DFS Remaining: 387172253696 (360.58 GB)
DFS Used%: 1.25%
DFS Remaining%: 91.85%
Configured Cache Capacity: 0 (0 B)
Cache Used: 0 (0 B)
Cache Remaining: 0 (0 B)
Cache Used%: 100.00%
Cache Remaining%: 0.00%
Xceivers: 2
Last contact: Wed Apr 05 16:22:00 UTC 2017
...

```
- Sie können auch die HDFS-Integrität auf dem HDInsight-Cluster mithilfe der folgenden Befehle prüfen:

```apache
hdiuser@hn0-spark2:~$ hdfs fsck -D "fs.default.name=hdfs://mycluster/" /
```

```apache
Connecting to namenode via http://hn0-spark2.2oyzcdm4sfjuzjmj5dnmvscjpg.dx.internal.cloudapp.net:30070/fsck?ugi=hdiuser&path=%2F
FSCK started by hdiuser (auth:SIMPLE) from /10.0.0.22 for path / at Wed Apr 05 16:40:28 UTC 2017
....................................................................................................

....................................................................................................
..................Status: HEALTHY
 Total size:    9330539472 B
 Total dirs:    37
 Total files:   2618
 Total symlinks:                0 (Files currently being written: 2)
 Total blocks (validated):      2535 (avg. block size 3680686 B)
 Minimally replicated blocks:   2535 (100.0 %)
 Over-replicated blocks:        0 (0.0 %)
 Under-replicated blocks:       0 (0.0 %)
 Mis-replicated blocks:         0 (0.0 %)
 Default replication factor:    3
 Average block replication:     3.0
 Corrupt blocks:                0
 Missing replicas:              0 (0.0 %)
 Number of data-nodes:          8
 Number of racks:               1
FSCK ended at Wed Apr 05 16:40:28 UTC 2017 in 187 milliseconds


The filesystem under path '/' is HEALTHY
```

- Wenn festgestellt wird, dass keine Blöcke fehlen, beschädigt oder unterrepliziert sind, bzw. diese Blöcke ignoriert werden können, führen Sie den folgenden Befehl aus, um den Namensknoten aus dem abgesicherten Modus herauszunehmen:

```apache
hdfs dfsadmin -D "fs.default.name=hdfs://mycluster/" -safemode leave
```


## <a name="how-do-i-fix-jdbc-or-sqlline-connectivity-issues-with-apache-phoenix"></a>Wie behebe ich JDBC- oder sqlline-Konnektivitätsprobleme mit Apache Phoenix?

### <a name="resolution-steps"></a>Lösungsschritte:

Um eine Verbindung mit Apache Phoenix herzustellen, müssen Sie die IP-Adresse des aktiven Zookeeper-Knotens bereitstellen. Stellen Sie sicher, dass der Zookeeper-Dienst, mit dem „sqlline.py“ eine Verbindung herzustellen versucht, ausgeführt wird.
1. Führen Sie die SSH-Anmeldung bei dem HDInsight-Cluster durch.
1. Versuchen Sie, folgenden Befehl auszuführen:
        
```apache
        "/usr/hdp/current/phoenix-client/bin/sqlline.py <IP of machine where Active Zookeeper is running"
```     
    Note: The IP of Active Zooker node can be identified from Ambari UI, by following the links to HBase -> "Quick Links" -> "ZK* (Active)" -> "Zookeeper Info". 

Wenn „sqlline.py“ eine Verbindung mit Apache Phoenix herzustellen versucht, und kein Timeout auftritt, führen Sie den folgenden Befehl aus, um die Verfügbarkeit und Integrität von Apache Phoenix zu überprüfen:

```apache
        !tables
        !quit
```      
- Wenn die oben aufgeführten Befehle funktionieren, liegt kein Problem vor. Die vom Benutzer bereitgestellte IP-Adresse könnte falsch sein.
   
    Wenn jedoch der Befehl zu lange anhält und dann den unten erwähnten Fehler meldet, befolgen Sie die folgenden Anweisungen zur Problembehandlung:

```apache
        Error while connecting to sqlline.py (Hbase - phoenix) Setting property: [isolation, TRANSACTION_READ_COMMITTED] issuing: !connect jdbc:phoenix:10.2.0.7 none none org.apache.phoenix.jdbc.PhoenixDriver Connecting to jdbc:phoenix:10.2.0.7 SLF4J: Class path contains multiple SLF4J bindings. 
```

- Führen Sie die folgenden Befehle vom Hauptknoten (hn0) aus aus, um die Bedingung der Phoenix-Tabelle SYSTEM.CATALOG zu diagnostizieren:

```apache
        hbase shell
        
        count 'SYSTEM.CATALOG'
```        
- Der Befehl sollte eine Fehlermeldung ähnlich der folgenden zurückgeben: 

```apache
        ERROR: org.apache.hadoop.hbase.NotServingRegionException: Region SYSTEM.CATALOG,,1485464083256.c0568c94033870c517ed36c45da98129. is not online on 10.2.0.5,16020,1489466172189) 
```
- Starten Sie den HMaster-Dienst auf allen Zookeeper-Knoten von der Ambari-Benutzeroberfläche aus mit folgenden Schritten neu:

    a. Wechseln Sie im zusammenfassenden Abschnitt von HBase zum Link „HBase -> Active HBase-Master“. 
    a. Starten Sie im Abschnitt „Components“ den HBase-Master-Dienst neu.
    a. Wiederholen Sie die oben genannten Schritte für die verbleibenden „Standby HBase Master“-Dienste. 

Es kann bis zu fünf Minuten dauern, bis der HBase-Master-Dienst stabilisiert und die Wiederherstellung beendet ist. Wiederholen Sie nach einigen Minuten Wartezeit die sqlline.py-Befehle, um zu bestätigen, dass die Systemkatalogtabelle aufgerufen ist und abgefragt werden kann. 

Sobald die Tabelle „SYSTEM.CATALOG“ wieder in normalem Zustand ist, sollte das Problem der Konnektivität mit Apache Phoenix automatisch gelöst werden.


## <a name="what-causes-a-master-server-to-fail-to-start"></a>Was ist die Ursache dafür, dass ein Masterserver nicht startet?

### <a name="error"></a>Fehler 

Fehler bei der atomischen Umbenennung

### <a name="detailed-description"></a>Detaillierte Beschreibung:

Während des Startprozesses führt HMaster viele Initialisierungsschritte einschließlich des Verschiebens von Daten aus dem von Grund auf neuen Ordner (.tmp) in den Datenordner durch. Außerdem prüft HMaster, ob sich im WALs-Ordner (Write Ahead Logs, Write-Ahead-Protokollen) inaktive Regionsserver usw. befinden. 

In allen diesen Fällen wird ein grundlegender „list“-Befehl für diese Ordner ausgeführt. Wenn zu beliebigem Zeitpunkt eine unerwartete Datei in einem dieser Ordner gefunden wird, wird eine Ausnahme ausgelöst und also kein Start ausgeführt.  

### <a name="probable-cause"></a>Wahrscheinliche Ursache:

Versuchen Sie, die Zeitachse der Dateierstellung zu identifizieren, und zu sehen, ob in diesem Zeitraum ein Prozessabsturz in Regionsserverprotokollen verzeichnet ist (bitten Sie hierzu das HBase-Team um Hilfe). So können wir robustere Mechanismen bereitstellen, um diesen Fehler zu vermeiden und das ordnungsgemäße Herunterfahren des Prozesses sicherzustellen.

### <a name="resolution-steps"></a>Lösungsschritte:

Versuchen Sie in einer solchen Situation, die Aufrufliste zu überprüfen, und zu sehen, welcher Ordner das Problem verursachen könnte (Ist es z.B. der WALs- oder TMP-Ordner?). Versuchen Sie dann, mittels Cloud-Explorer oder hdfs-Befehlen die Problemdatei zu finden – in der Regel ist dies eine *-renamePending.json-Datei (eine zum Implementieren des Atomic Rename-Vorgangs in WASB-Treiber verwendete Journaldatei; aufgrund von Fehlern in dieser Implementierung können solche Dateien bei Prozessabstürzen usw. übrig bleiben). Erzwingen Sie deren Löschen über den Cloud-Explorer. 

Darüber hinaus könnte sich in einigen Fällen möglicherweise auch eine temporäre Datei der Art „$$$. $$$“ an diesem Speicherort befinden. Dies ist nicht im Cloud-Explorer, sondern nur mit dem hdfs ls-Befehl zu erkennen. Sie können den hdfs-Befehl „hdfs dfs -rm /<the path>/\$\$\$.\$\$\$“ zum Löschen dieser Datei verwenden.  

Sobald dies geschehen ist, sollte HMaster sofort starten. 

### <a name="error-no-server-address-listed-in"></a>Fehler: Keine Serveradresse aufgelistet in 

Keine Serveradresse in „hbase:meta“ aufgelistet für Region xxx

### <a name="detailed-description"></a>Detaillierte Beschreibung:

Auf dem Linux-Cluster des Kunden ist das Problem aufgetreten, dass hbase:meta-Tabelle nicht online war. Der ausgeführte hbck-Befehl meldete, dass „hbase:meta-Tabelle replicaId 0 in keiner Region gefunden wurde“. Nach dem HBase-Neustart trat das Symptom auf, dass HMaster nicht initialisiert werden konnte. In den HMaster-Protokollen wurde aufgezeichnet, dass „keine Serveradresse in hbase:meta für die Region hbase:backup <region name> aufgelistet ist“.  

### <a name="resolution-steps"></a>Lösungsschritte:

- Geben Sie Folgendes in der HBase-Shell ein (mit entsprechend geänderten Istwerten).  

```apache
> scan 'hbase:meta'  
```

```apache
> delete 'hbase:meta','hbase:backup <region name>','<column name>'  
```

- Löschen Sie den Eintrag „hbase:namespace“, da der gleiche Fehler bei der Überprüfung der „hbase:namespace“-Tabelle gemeldet werden könnte.

- Starten Sie den aktiven HMaster von der Ambari-Benutzeroberfläche aus neu, um HBase hochzufahren.  

- Führen Sie den folgenden Befehl in der HBase-Shell aus, um alle Tabellen hochzufahren, die offline sind:

```apache 
hbase hbck -ignorePreCheckPermission -fixAssignments 
```

### <a name="further-reading"></a>Weitere nützliche Informationen:

[HBase-Tabelle kann nicht verarbeitet werden](http://stackoverflow.com/questions/4794092/unable-to-access-hbase-table)


### <a name="error"></a>Fehler

Bei HMaster tritt ein Timeout mit schwerwiegendem Ausnahmefehler auf, Beispiel: java.io.IOException: Timeout nach 300.000 ms Warten auf die Zuweisung der Namespace-Tabelle.

### <a name="detailed-description"></a>Detaillierte Beschreibung:

Dieses Problem trat bei Kunden auf, die offensichtlich viele Tabellen und Regionen besaßen und nicht geleert hatten, als sie ihre HMaster-Dienste neu starteten. Beim Neustart trat mit obiger Meldung ein Fehler auf. Kein anderer Fehler wurde gefunden.  

### <a name="probable-cause"></a>Wahrscheinliche Ursache:

Dies ist ein bekannter „Defekt“ bei HMaster – allgemeine Clusterstartaufgaben können sehr lange dauern, und HMaster fährt herunter, weil die Namespace-Tabelle noch nicht zugewiesen ist – was nur in diesem Szenario auftritt, wo große Mengen nicht geleerter Daten vorhanden sind und ein Timeout von fünf Minuten nicht ausreicht.
  
### <a name="resolution-steps"></a>Lösungsschritte:

- Greifen Sie auf die Ambari-Benutzeroberfläche zu, wechseln Sie zu „HBase -> Configs“, und fügen Sie in der benutzerdefinierten Datei „hbase-site.xml“ folgende Einstellung hinzu: 

```apache
Key: hbase.master.namespace.init.timeout Value: 2400000  
```

- Starten Sie die erforderlichen Dienste neu (hauptsächlich HMaster und möglicherweise andere HBase-Dienste).  



## <a name="what-causes-a-restart-failure-on-a-region-server"></a>Was verursacht einen Neustartfehler bei einem Regionsserver?

### <a name="probable-cause"></a>Wahrscheinliche Ursache:

Erstens kann diese Situation mit den folgenden bewährten Methoden verhindert werden. Sie sollten die hohe Workload-Aktivität anhalten, wenn Sie planen, die HBase-Regionsserver neu zu starten. Wenn die Anwendung während des Herunterfahrens weiterhin versucht, Verbindungen mit Regionsservern herzustellen, wird der Regionsserver-Neustart um einige Minuten verlangsamt. Außerdem sollte der Benutzer alle Tabellen wie in [HDInsight HBase: How to Improve HBase cluster restart time by Flushing tables](https://blogs.msdn.microsoft.com/azuredatalake/2016/09/19/hdinsight-hbase-how-to-improve-hbase-cluster-restart-time-by-flushing-tables/) (HDInsight HBase: Verbesserung der HBase-Clusterneustart-Zeit durch Leeren von Tabellen) beschrieben leeren.

Wenn ein Benutzer den Neustartvorgang auf HBase-Regionsservern von der Ambari-Benutzeroberfläche aus initiiert. Er würde sofort sehen, dass die Regionsserver heruntergefahren sind, aber zu lange nicht wieder hochfahren. 

Im folgenden sehen Sie, was hinter den Kulissen geschieht: 

1. Der Ambari-Agent sendet eine Beendigungsanforderung an den Regionsserver.
1. Der Ambari-Agent wartet dann 30 Sekunden auf das ordnungsgemäße Herunterfahren des Regionsservers. 
1. Wenn die Anwendung des Kunden weiterhin versucht, eine Verbindung mit dem Regionsserver herzustellen, fährt er nicht sofort herunter, und der 30-Sekunden-Timeout läuft also früher ab. 
1. Nach Ablauf von 30 Sekunden sendet der Ambari-Agent einen Befehl zum erzwungenen Beenden (kill -9) an den Regionsserver. Dies können Sie in „ambari-agent.log“ (im Verzeichnis „/var/log/“ des jeweiligen Workerknotens) wie folgt beobachten:

```apache
         2017-03-21 13:22:09,171 - Execute['/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh --config /usr/hdp/current/hbase-regionserver/conf stop regionserver'] {'only_if': 'ambari-sudo.sh  -H -E t
         est -f /var/run/hbase/hbase-hbase-regionserver.pid && ps -p `ambari-sudo.sh  -H -E cat /var/run/hbase/hbase-hbase-regionserver.pid` >/dev/null 2>&1', 'on_timeout': '! ( ambari-sudo.sh  -H -E test -
         f /var/run/hbase/hbase-hbase-regionserver.pid && ps -p `ambari-sudo.sh  -H -E cat /var/run/hbase/hbase-hbase-regionserver.pid` >/dev/null 2>&1 ) || ambari-sudo.sh -H -E kill -9 `ambari-sudo.sh  -H 
         -E cat /var/run/hbase/hbase-hbase-regionserver.pid`', 'timeout': 30, 'user': 'hbase'}
         2017-03-21 13:22:40,268 - Executing '! ( ambari-sudo.sh  -H -E test -f /var/run/hbase/hbase-hbase-regionserver.pid && ps -p `ambari-sudo.sh  -H -E cat /var/run/hbase/hbase-hbase-regionserver.pid` >
         /dev/null 2>&1 ) || ambari-sudo.sh -H -E kill -9 `ambari-sudo.sh  -H -E cat /var/run/hbase/hbase-hbase-regionserver.pid`'. Reason: Execution of 'ambari-sudo.sh su hbase -l -s /bin/bash -c 'export  
         PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/var/lib/ambari-agent ; /usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh --config /usr/hdp/curre
         nt/hbase-regionserver/conf stop regionserver was killed due timeout after 30 seconds
         2017-03-21 13:22:40,285 - File['/var/run/hbase/hbase-hbase-regionserver.pid'] {'action': ['delete']}
         2017-03-21 13:22:40,285 - Deleting File['/var/run/hbase/hbase-hbase-regionserver.pid']
```
Aufgrund dieses abrupten Herunterfahrens wird der Port, der dem Prozess zugeordnet ist, möglicherweise nicht freigegeben, obwohl der Regionsserverprozess beendet wird. Diese Situation kann, wie in den Protokollen unten dargestellt, beim Starten des Regionsservers zu der AddressBindException-Ausnahme führen. Dies können Sie in „region-server.log“ im Verzeichnis „/var/log/hbase“ des Workerknotens überprüfen, wo beim Start des Regionsservers ein Fehler auftritt. 

```apache

       2017-03-21 13:25:47,061 ERROR [main] regionserver.HRegionServerCommandLine: Region server exiting
       java.lang.RuntimeException: Failed construction of Regionserver: class org.apache.hadoop.hbase.regionserver.HRegionServer
       at org.apache.hadoop.hbase.regionserver.HRegionServer.constructRegionServer(HRegionServer.java:2636)
       at org.apache.hadoop.hbase.regionserver.HRegionServerCommandLine.start(HRegionServerCommandLine.java:64)
       at org.apache.hadoop.hbase.regionserver.HRegionServerCommandLine.run(HRegionServerCommandLine.java:87)
       at org.apache.hadoop.util.ToolRunner.run(ToolRunner.java:76)
       at org.apache.hadoop.hbase.util.ServerCommandLine.doMain(ServerCommandLine.java:126)
       at org.apache.hadoop.hbase.regionserver.HRegionServer.main(HRegionServer.java:2651)
       
       Caused by: java.lang.reflect.InvocationTargetException
       at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
       at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:57)
       at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
       at java.lang.reflect.Constructor.newInstance(Constructor.java:526)
       at org.apache.hadoop.hbase.regionserver.HRegionServer.constructRegionServer(HRegionServer.java:2634)
       ... 5 more
       
       Caused by: java.net.BindException: Problem binding to /10.2.0.4:16020 : Address already in use
       at org.apache.hadoop.hbase.ipc.RpcServer.bind(RpcServer.java:2497)
       at org.apache.hadoop.hbase.ipc.RpcServer$Listener.<init>(RpcServer.java:580)
       at org.apache.hadoop.hbase.ipc.RpcServer.<init>(RpcServer.java:1982)
       at org.apache.hadoop.hbase.regionserver.RSRpcServices.<init>(RSRpcServices.java:863)
       at org.apache.hadoop.hbase.regionserver.HRegionServer.createRpcServices(HRegionServer.java:632)
       at org.apache.hadoop.hbase.regionserver.HRegionServer.<init>(HRegionServer.java:532)
       ... 10 more
       
       Caused by: java.net.BindException: Address already in use
       at sun.nio.ch.Net.bind0(Native Method)
       at sun.nio.ch.Net.bind(Net.java:463)
       at sun.nio.ch.Net.bind(Net.java:455)
       at sun.nio.ch.ServerSocketChannelImpl.bind(ServerSocketChannelImpl.java:223)
       at sun.nio.ch.ServerSocketAdaptor.bind(ServerSocketAdaptor.java:74)
       at org.apache.hadoop.hbase.ipc.RpcServer.bind(RpcServer.java:2495)
       ... 15 more
```

### <a name="resolution-steps"></a>Lösungsschritte:

Versuchen Sie in solchen Fällen die folgende Problemumgehung: 

- Versuchen Sie, die Last auf den HBase-Regionsservern zu reduzieren, bevor Sie einen Neustart initiieren. 

- Alternativ (wenn die vorherigen Schritte nicht hilfreich waren) können Sie versuchen, die Regionsserver auf den Workerknoten mithilfe folgender Befehle manuell neu zu starten:

```apache
      sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh stop regionserver"
      sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh start regionserver"   
```



