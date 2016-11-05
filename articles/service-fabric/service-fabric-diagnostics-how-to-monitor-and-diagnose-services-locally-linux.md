---
title: Lokales Überwachen und Diagnostizieren von mit Azure Service Fabric geschriebenen Diensten| Microsoft Docs
description: Erfahren Sie, wie Sie mit Microsoft Azure Service Fabric erstellte Dienste auf einem lokalen Entwicklungscomputer überwachen und diagnostizieren.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: ''

ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/24/2016
ms.author: subramar

---
# <a name="monitor-and-diagnose-services-in-a-local-machine-development-setup"></a>Überwachen und Diagnostizieren von Diensten in einer Entwicklungsumgebung auf einem lokalen Computer
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
> * [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
> 
> 

Überwachung, Erkennung, Diagnose und Problembehandlung ermöglichen das Ausführen von Diensten mit minimalen Unterbrechungen für Benutzer. Überwachung und Diagnose sind in einer tatsächlichen bereitgestellten Produktionsumgebung wichtig. Die Anwendung eines ähnlichen Modells während der Entwicklung von Diensten stellt sicher, dass die Diagnosepipeline beim Wechsel zu einer Produktionsumgebung funktioniert. Service Fabric erleichtert Dienstentwicklern das Implementieren von Diagnosen, die sowohl in einer lokalen Umgebung auf einem einzelnen Computer als auch in der tatsächlichen Konfiguration in einem Produktionscluster nahtlos verwendet werden können.

## <a name="debugging-service-fabric-java-applications"></a>Debuggen von Service Fabric Java-Anwendungen
Für Java-Anwendungen sind [mehrere Protokollierungsframeworks](http://en.wikipedia.org/wiki/Java_logging_framework) verfügbar. Da `java.util.logging` die Standardoption bei JRE ist, wird dies auch für die [Codebeispiele in Github](http://github.com/Azure-Samples/service-fabric-java-getting-started)verwendet.  Im Folgenden wir die Konfiguration des `java.util.logging` -Frameworks erklärt. 

Mithilfe von „java.util.logging“ können Sie Ihre Anwendungsprotokolle zum Arbeitsspeicher, zu Ausgabedatenströmen, zu Konsolendateien oder zu Sockets umleiten. Für jede dieser Optionen gibt es Standardhandler, die im Framework bereits bereitgestellt wurden. Sie können eine `app.properties` -Datei erstellen, um den Dateihandler für Ihre Anwendung zu konfigurieren, damit alle Protokolle zu einer lokalen Datei umgeleitet werden. 

Der folgende Codeausschnitt enthält eine Beispielkonfiguration: 

```java 
handlers = java.util.logging.FileHandler

java.util.logging.FileHandler.level = ALL
java.util.logging.FileHandler.formatter = java.util.logging.SimpleFormatter
java.util.logging.FileHandler.limit = 1024000
java.util.logging.FileHandler.count = 10
java.util.logging.FileHandler.pattern = /tmp/servicefabric/logs/mysfapp%u.%g.log             
```

Der Ordner, auf den die `app.properties` -Datei verweist, muss vorhanden sein. Nachdem die `app.properties`-Datei erstellt wurde, müssen Sie auch Ihr Skript für den Einstiegspunkt, `entrypoint.sh` im `<applicationfolder>/<servicePkg>/Code/`-Ordner, ändern, um die Eigenschaft `java.util.logging.config.file` auf die `app.propertes`-Datei festzulegen. Der Eintrag sollte wie der folgende Ausschnitt aussehen:

```sh 
java -Djava.library.path=$LD_LIBRARY_PATH -Djava.util.logging.config.file=<path to app.properties> -jar <service name>.jar
```


Diese Konfiguration führt zu Protokollen, die auf rotierende Weise unter `/tmp/servicefabric/logs/`erfasst werden. **%u** und **%g** ermöglichen das Erstellen weiterer Dateien mit den Dateinamen „mysfapp0.log“, „mysfapp1.log“ usw. Wenn kein Handler explizit konfiguriert ist, wird standardmäßig der Konsolen-Handler registriert. Die Protokolle können in syslog unter „/var/log/syslog“ angezeigt werden.

Weitere Informationen finden Sie unter der [Codebeispielen in Github](http://github.com/Azure-Samples/service-fabric-java-getting-started).  

## <a name="next-steps"></a>Nächste Schritte
Der Ablaufverfolgungscode, der Ihrer Anwendung hinzugefügt wurde, gilt auch für die Diagnose der Anwendung auf einem Azure-Cluster. Sehen Sie sich diese Artikel an, in denen die verschiedenen Optionen für die Tools erläutert werden und beschrieben wird, wie Sie sie einrichten können.

* [Sammeln von Protokollen mit Azure-Diagnose](service-fabric-diagnostics-how-to-setup-lad.md)

<!--HONumber=Oct16_HO2-->


