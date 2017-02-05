---
title: "Allgemeine Metriken für die automatische Skalierung in Azure Monitor | Microsoft Docs"
description: "Erfahren Sie, welche Metriken häufig für die automatische Skalierung von Cloud Services, Virtual Machines und Web-Apps verwendet werden."
author: kamathashwin
manager: carolz
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 189b2a13-01c8-4aca-afd5-90711903ca59
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2016
ms.author: ashwink
translationtype: Human Translation
ms.sourcegitcommit: 5919c477502767a32c535ace4ae4e9dffae4f44b
ms.openlocfilehash: 8d5f8dd454741f5946d6a2c265ce67808abdac9e


---
# <a name="azure-monitor-autoscaling-common-metrics"></a>Allgemeine Metriken für die automatische Skalierung in Azure Monitor
Mit der automatischen Skalierung in Azure Monitor können Sie die Anzahl der ausgeführten Instanzen basierend auf Telemetriedaten (Metriken) zentral hoch- oder herunterskalieren. Dieses Dokument beschreibt allgemeine Metriken, die Sie verwenden möchten. Im Azure-Portal für Cloud Services und Serverfarmen können Sie die Metrik der Ressource für die Skalierung auswählen. Sie können jedoch auch eine Metrik aus einer anderen Ressource für die Skalierung auswählen.

Nachfolgend finden Sie Details dazu, wie Sie die Metriken suchen und auflisten, nach denen Sie skalieren möchten. Folgendes gilt auch für Skalierungsgruppen für virtuelle Computer.

## <a name="compute-metrics"></a>Berechnen von Metriken
Standardmäßig ist die Diagnoseerweiterung bei Azure VM v2 konfiguriert, wobei die folgenden Metriken aktiviert sind.

* [Gastmetriken für Windows-VM v2](#compute-metrics-for-windows-vm-v2-as-a-guest-os)
* [Gastmetriken für Linux-VM v2](#compute-metrics-for-linux-vm-v2-as-a-guest-os)

Sie können die `Get MetricDefinitions`-API/PoSH/CLI zum Anzeigen der Metriken für Ihre verfügbare VMSS-Ressource verwenden. 

Wenn Sie VM-Skalierungsgruppen verwenden und eine bestimmte Metrik nicht aufgeführt wird, ist sie wahrscheinlich in der Diagnoseerweiterung *deaktiviert*.

Wenn eine bestimmte Metrik nicht mit der gewünschten Häufigkeit geprüft oder übertragen wird, können Sie die Diagnosekonfiguration aktualisieren.

Wenn einer der beiden oben genannten Fälle zutrifft, lesen Sie unter [Aktivieren der Azure-Diagnose auf einem virtuellen Azure-Computer unter Windows mithilfe von PowerShell](../virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), um mehr darüber zu erfahren, wie Sie mit PowerShell die Azure-VM-Diagnoseerweiterung zur Aktivierung der Metrik konfigurieren und aktualisieren. Dieser Artikel enthält auch eine beispielhafte Diagnosekonfigurationsdatei.

### <a name="compute-metrics-for-windows-vm-v2-as-a-guest-os"></a>Berechnen von Metriken für Windows-VM v2 als Gastbetriebssystem
Wenn Sie einen neuen virtuellen Computer (v2) in Azure erstellen, ist die Diagnose mithilfe der Diagnoseerweiterung aktiviert.

Sie können eine Liste der Metriken mithilfe des folgenden Befehls in PowerShell generieren.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Sie können eine Warnung für die folgenden Metriken erstellen.

| Metrikname | Einheit |
| --- | --- |
| \Processor(_Total)\%Prozessorzeit |Prozent |
| \Processor(_Total)\%Privilegierte Zeit |Prozent |
| \Processor(_Total)\%Benutzerzeit |Prozent |
| \Processor Informationen(_Total)\Prozessorfrequenz |Count |
| \System\Prozesse |Count |
| \Process(_Total)\Threadanzahl |Count |
| \Process(_Total)\Handleanzahl |Count |
| \Memory\%Verwendete zugesicherte Bytes |Prozent |
| \Memory\Verfügbare Bytes |Byte |
| \Memory\Zugesicherte Bytes |Byte |
| \Memory\Zusagegrenze |Byte |
| \Memory\Auslagerungsseiten (Bytes) |Byte |
| \Memory\Nicht-Auslagerungsseiten (Bytes) |Byte |
| \PhysicalDisk(_Total)\%Zeit |Prozent |
| \PhysicalDisk(_Total)\%Lesezeit |Prozent |
| \PhysicalDisk(_Total)\%Schreibzeit |Prozent |
| \Physikalischer Datenträger(_Total)\Übertragungen/s |Anzahl pro Sekunde |
| \PhysicalDisk(_Total)\Lesevorgänge/s |Anzahl pro Sekunde |
| \PhysicalDisk(_Total)\Schreibvorgänge/s |Anzahl pro Sekunde |
| \PhysicalDisk(_Total)\Bytes/s |Bytes pro Sekunde |
| \PhysicalDisk(_Total)\Byte gelesen/s |Bytes pro Sekunde |
| \PhysicalDisk(_Total)\Byte geschrieben/s |Bytes pro Sekunde |
| \PhysicalDisk(_Total)\Durchschnittl. Warteschlangenlänge des Datenträgers |Count |
| \PhysicalDisk(_Total)\Durchschnittl. Warteschlangenlänge der Datenträger-Lesevorgänge |Count |
| \PhysicalDisk(_Total)\Durchschnittl. Warteschlangenlänge der Datenträger-Schreibvorgänge |Count |
| \LogicalDisk(_Total)\%Freier Speicherplatz |Prozent |
| \LogicalDisk(_Total)\MB frei |Count |

### <a name="compute-metrics-for-linux-vm-v2-as-a-guest-os"></a>Berechnen von Metriken für Linux-VM v2 als Gastbetriebssystem
Wenn Sie einen neuen virtuellen Computer (v2) in Azure erstellen, ist die Diagnose mithilfe der Diagnoseerweiterung standardmäßig aktiviert.

Sie können eine Liste der Metriken mithilfe des folgenden Befehls in PowerShell generieren.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

 Sie können eine Warnung für die folgenden Metriken erstellen.

| Metrikname | Unit |
| --- | --- |
| \Memory\AvailableMemory |Byte |
| \Memory\PercentAvailableMemory |Prozent |
| \Memory\UsedMemory |Byte |
| \Memory\PercentUsedMemory |Prozent |
| \Memory\PercentUsedByCache |Prozent |
| \Memory\PagesPerSec |Anzahl pro Sekunde |
| \Memory\PagesReadPerSec |Anzahl pro Sekunde |
| \Memory\PagesWrittenPerSec |Anzahl pro Sekunde |
| \Memory\AvailableSwap |Byte |
| \Memory\PercentAvailableSwap |Prozent |
| \Memory\UsedSwap |Byte |
| \Memory\PercentUsedSwap |Prozent |
| \Processor\PercentIdleTime |Prozent |
| \Processor\PercentUserTime |Prozent |
| \Processor\PercentNiceTime |Prozent |
| \Processor\PercentPrivilegedTime |Prozent |
| \Processor\PercentInterruptTime |Prozent |
| \Processor\PercentDPCTime |Prozent |
| \Processor\PercentProcessorTime |Prozent |
| \Processor\PercentIOWaitTime |Prozent |
| \PhysicalDisk\BytesPerSecond |Bytes pro Sekunde |
| \PhysicalDisk\ReadBytesPerSecond |Bytes pro Sekunde |
| \PhysicalDisk\WriteBytesPerSecond |Bytes pro Sekunde |
| \PhysicalDisk\TransfersPerSecond |Anzahl pro Sekunde |
| \PhysicalDisk\ReadsPerSecond |Anzahl pro Sekunde |
| \PhysicalDisk\WritesPerSecond |Anzahl pro Sekunde |
| \PhysicalDisk\AverageReadTime |Sekunden |
| \PhysicalDisk\AverageWriteTime |Sekunden |
| \PhysicalDisk\AverageTransferTime |Sekunden |
| \PhysicalDisk\AverageDiskQueueLength |Count |
| \NetworkInterface\BytesTransmitted |Byte |
| \NetworkInterface\BytesReceived |Byte |
| \NetworkInterface\PacketsTransmitted |Count |
| \NetworkInterface\PacketsReceived |Count |
| \NetworkInterface\BytesTotal |Byte |
| \NetworkInterface\TotalRxErrors |Count |
| \NetworkInterface\TotalTxErrors |Count |
| \NetworkInterface\TotalCollisions |Count |

## <a name="commonly-used-web-server-farm-metrics"></a>Häufig verwendete Webmetriken (Serverfarm)
Sie können die automatische Skalierung auch basierend auf allgemeinen Webservermetriken wie der HTTP-Warteschlangenlänge ausführen. Der Metrikname ist **HttpQueueLength**.  Im folgenden Abschnitt sind die verfügbaren Serverfarmmetriken (Web-Apps) aufgeführt.

### <a name="web-apps-metrics"></a>Web-Apps-Metriken
Sie können eine Liste der Web-Apps-Metriken mithilfe des folgenden Befehls in PowerShell generieren.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Anhand dieser Metriken können Sie Warnungen ausgeben oder skalieren.

| Metrikname | Unit |
| --- | --- |
| CpuPercentage |Prozent |
| MemoryPercentage |Prozent |
| DiskQueueLength |Count |
| HttpQueueLength |Count |
| BytesReceived |Byte |
| BytesSent |Byte |

## <a name="commonly-used-storage-metrics"></a>Häufig verwendete Speichermetriken
Sie können nach Speicherwarteschlangenlänge skalieren, wobei es sich um die Anzahl der Nachrichten in der Speicherwarteschlange handelt. Die Speicherwarteschlangenlänge ist eine spezielle Metrik und der angewendete Schwellenwert ist die Anzahl der Nachrichten pro Instanz. Dies bedeutet bei zwei Instanzen und bei einem Schwellenwert von 100 kommt es zu einer Skalierung, wenn die Gesamtanzahl der Nachrichten in der Warteschlange 200 beträgt. Beispiel: 100 Nachrichten pro Instanz.

Sie können dies im Azure-Portal auf dem Blatt **Einstellungen** konfigurieren. Für VM-Skalierungsgruppen können Sie die Einstellung für die automatische Skalierung in der ARM-Vorlage aktualisieren, sodass *metricName* als *ApproximateMessageCount* verwendet und die ID der Speicherwarteschlange als *metricResourceUri* übergeben wird.

Bei einem klassischen Speicherkonto würde die metricTrigger-Einstellung für die automatische Skalierung Folgendes enthalten:

```
"metricName": "ApproximateMessageCount",
 "metricNamespace": "",
 "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ClassicStorage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
 ```

Bei einem (nicht klassischen) Speicherkonto würde metricTrigger Folgendes enthalten:

```
"metricName": "ApproximateMessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Storage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
```

## <a name="commonly-used-service-bus-metrics"></a>Häufig verwendete Service Bus-Metriken
Sie können nach Service Bus-Warteschlangenlänge skalieren, wobei es sich um die Anzahl der Nachrichten in der Service Bus-Warteschlange handelt. Die Service Bus-Warteschlangenlänge ist eine spezielle Metrik, und der angewendete Schwellenwert ist die Anzahl der Nachrichten pro Instanz. Dies bedeutet bei zwei Instanzen und bei einem Schwellenwert von 100 kommt es zu einer Skalierung, wenn die Gesamtanzahl der Nachrichten in der Warteschlange 200 beträgt. Beispiel: 100 Nachrichten pro Instanz.

Für VM-Skalierungsgruppen können Sie die Einstellung für die automatische Skalierung in der ARM-Vorlage aktualisieren, sodass *metricName* als *ApproximateMessageCount* verwendet und die ID der Speicherwarteschlange als *metricResourceUri* übergeben wird.

```
"metricName": "MessageCount",
 "metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue"
```

> [!NOTE]
> Für Service Bus gibt es das Konzept einer Ressourcengruppe nicht, Azure Resource Manager erstellt jedoch eine Standardressourcengruppe pro Region. Die Ressourcengruppe hat in der Regel das Format „Default-ServiceBus-[Region]“. Beispiel: „Default-ServiceBus-EastUS“, „Default-ServiceBus-WestUS“, „Default-ServiceBus-AustraliaEast“ usw.
> 
> 




<!--HONumber=Nov16_HO3-->


