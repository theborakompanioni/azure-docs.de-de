<properties
   pageTitle="Sammeln von Protokollen mit der Linux-Azure-Diagnose | Microsoft Azure"
   description="In diesem Artikel erfahren Sie, wie Sie die Azure-Diagnose so konfigurieren, dass Protokolle aus einem Linux-basierten Service Fabric-Cluster unter Azure gesammelt werden."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/28/2016"
   ms.author="subramar"/>


# Sammeln von Protokollen mit Azure-Diagnose

> [AZURE.SELECTOR]
- [Windows](service-fabric-diagnostics-how-to-setup-wad.md)
- [Linux](service-fabric-diagnostics-how-to-setup-lad.md)

Bei Verwendung eines Azure Service Fabric-Clusters empfiehlt es sich, die Protokolle aller Knoten an einem zentralen Ort zu sammeln. Das Sammeln der Protokolle an einem zentralen Ort vereinfacht die Analyse und Behandlung von Problemen, die ggf. in Ihren Diensten, in Ihrer Anwendung oder im Cluster auftreten. Eine Möglichkeit zum Hochladen und Sammeln von Protokollen ist die Verwendung der Erweiterung „Azure-Diagnose“, mit der Protokolle an Azure Storage hochgeladen werden. Die Ereignisse können aus dem Speicher gelesen und in einem Produkt wie [Elastic Search](service-fabric-diagnostic-how-to-use-elasticsearch.md) oder in einer anderen Protokollanalyselösung verwendet werden.

## Andere Protokollquellen, die gesammelt werden können
1. **Service Fabric-Protokolle:** Werden von der Plattform mittels LTTng ausgegeben und in Ihr Speicherkonto hochgeladen. Bei Protokollen kann es sich um von der Plattform ausgegebene Betriebs- oder Laufzeitereignisse handeln. Diese Protokolle werden an dem Ort gespeichert, auf den in Ihrem Clustermanifest verwiesen wird. (Details zum Speicherkonto finden Sie unter dem Tag „AzureTableWinFabETWQueryable“ unter „StoreConnectionString“.)
2. **Anwendungsereignisse:** Von Ihrem Dienstcode ausgegebene Ereignisse. Sie können eine beliebige Protokollierungslösung verwenden, die textbasierte Protokolldateien schreibt (beispielsweise [LTTng](http://lttng.org)). Informationen zur Durchführung einer Ablaufverfolgung für Ihre Anwendung finden Sie in der LTTng-Dokumentation.


## Bereitstellen der Diagnoseerweiterungen
Zum Sammeln von Protokollen muss zunächst die Diagnoseerweiterung auf allen VMs des Service Fabric-Clusters bereitgestellt werden. Die Diagnoseerweiterung sammelt Protokolle auf allen VMs und lädt sie an das angegebene Speicherkonto hoch. Die auszuführenden Schritte hängen davon ab, ob Sie das Azure-Portal oder Azure Resource Manager verwenden.

### Bereitstellen der Diagnoseerweiterung im Rahmen der Clustererstellung 
Wenn Sie die Diagnoseerweiterung im Rahmen der Clustererstellung für die im Cluster enthaltenen virtuellen Computer bereitstellen möchten, legen Sie „Diagnose“ auf **Ein** fest. Nach der Erstellung des Clusters kann diese Einstellung nicht mehr über das Portal geändert werden.

Konfigurieren Sie die Linux-Azure-Diagnose (LAD) so, dass sie die Dateien erfasst und im Kundenspeicherkonto ablegt. Dieser Prozess wird im Artikel [Verwenden der Linux-Diagnoseerweiterung zur Überwachung der Leistungs- und Diagnosedaten eines Linux-VM](../virtual-machines/virtual-machines-linux-classic-diagnostic-extension.md) als drittes Szenario („Hochladen eigener Protokolldateien“) beschrieben. Durch dieses Verfahren erhalten Sie Zugriff auf die Ablaufverfolgungen, die dann in eine Visualisierungslösung Ihrer Wahl hochgeladen werden können.


Die Diagnoseerweiterung kann auch mithilfe von Azure Resource Manager bereitgestellt werden. Die Dokumentation für Windows-Cluster finden Sie unter [Sammeln von Protokollen mit Azure-Diagnose](service-fabric-diagnostics-how-to-setup-wad.md). Für Linux wird ein ähnlicher Prozess verwendet.

Sie können auch OMS verwenden. Eine entsprechende Beschreibung finden Sie unter [Operations Management Suite Log Analytics with Linux](https://blogs.technet.microsoft.com/hybridcloud/2016/01/28/operations-management-suite-log-analytics-with-linux/) (Operations Management Suite-Protokollanalyse mit Linux).

Nach Abschluss der Konfiguration überwacht der LAD-Agent die angegebenen Protokolldateien. Sobald bei einer Datei eine neue Zeile hinzukommt, erstellt der Agent einen Syslog-Eintrag, der an den vom Kunden angegebenen Speicher gesendet wird.


## Nächste Schritte
Ausführlichere Informationen zu relevanten Ereignissen für die Problembehandlung finden Sie in der [LTTng-Dokumentation](http://lttng.org/docs) sowie unter [Verwenden der Linux-Diagnoseerweiterung zur Überwachung der Leistungs- und Diagnosedaten eines Linux-VM](../virtual-machines/virtual-machines-linux-classic-diagnostic-extension.md).

<!---HONumber=AcomDC_0928_2016-->