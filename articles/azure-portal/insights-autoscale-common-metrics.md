<properties
	pageTitle="Azure Insights: Allgemeine Metriken für die automatische Skalierung in Azure Insights | Microsoft Azure"
	description="Erfahren Sie, welche Metriken häufig für die automatische Skalierung von Cloud Services, Virtual Machines und Web-Apps verwendet werden."
	authors="kamathashwin"
	manager=""
	editor=""
	services="azure-portal"
	documentationCenter="na"/>

<tags
	ms.service="azure-portal"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="03/30/2016"
	ms.author="ashwink"/>

# Allgemeine Metriken für die automatische Skalierung in Azure Insights

Mit der automatischen Skalierung in Azure Insights können Sie die Anzahl der ausgeführten Instanzen nach oben oder unten skalieren, basierend auf Telemetriedaten (Metriken). Dieses Dokument beschreibt allgemeine Metriken, die Sie verwenden möchten. Im Azure-Portal für Cloud Services und Serverfarmen können Sie die Metrik der Ressource für die Skalierung auswählen. Sie können jedoch auch eine Metrik aus einer anderen Ressource für die Skalierung auswählen.

Nachfolgend finden Sie Details dazu, wie Sie die Metriken suchen und auflisten, nach denen Sie skalieren möchten. Folgendes gilt auch für Skalierungsgruppen für virtuelle Computer.

## Berechnen von Metriken
Standardmäßig ist die Diagnoseerweiterung bei Azure VM v2 konfiguriert, wobei die folgenden Metriken aktiviert sind.

- [Gastmetriken für Windows-VM v2](#compute-metrics-for-windows-vm-v2-as-a-guest-os)
- [Gastmetriken für Linux-VM v2](#compute-metrics-for-linux-vm-v2-as-a-guest-os)

Sie können die `Get MetricDefinitions`-API/PoSH/CLI zum Anzeigen der Metriken für Ihre verfügbare VMSS-Ressource verwenden.

Wenn Sie VM-Skalierungsgruppen verwenden und eine bestimmte Metrik nicht aufgeführt ist, ist sie wahrscheinlich in der Diagnoseerweiterung *deaktiviert*.

Wenn eine bestimmte Metrik nicht mit der gewünschten Häufigkeit geprüft oder übertragen wird, können Sie die Diagnosekonfiguration aktualisieren.

Wenn einer der beiden oben genannten Fälle wahr ist, lesen Sie unter [Verwenden von PowerShell zum Aktivieren der Azure-Diagnose auf einem virtuellen Computer unter Windows](../virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md) mehr über PowerShell, um die Azure-VM-Diagnoseerweiterung zur Aktivierung der Metrik zu konfigurieren und zu aktualisieren. Dieser Artikel enthält auch eine beispielhafte Diagnosekonfigurationsdatei.

### Berechnen von Metriken für Windows-VM v2 als Gastbetriebssystem

Wenn Sie einen neuen virtuellen Computer (v2) in Azure erstellen, ist die Diagnose mithilfe der Diagnoseerweiterung aktiviert.

Sie können eine Liste der Metriken mithilfe des folgenden Befehls in PowerShell generieren.


```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Sie können eine Warnung für die folgenden Metriken erstellen.

|Metrikname|	Unit|
|---|---|
|\\Prozessor(\_Gesamt)\\Prozessorzeit (%) | Prozent|
|\\Prozessor(\_Gesamt)\\Privilegierte Zeit (%) | Prozent|
|\\Prozessor(\_Gesamt)\\Benutzerzeit (%) | Prozent|
|\\Prozessorinformationen(\_Gesamt)\\Prozessorfrequenz | Anzahl|
|\\System\\Prozesse | Anzahl|
|\\Prozess(\_Gesamt)\\Threadanzahl | Anzahl|
|\\Prozess(\_Gesamt)\\Handleanzahl | Anzahl|
|\\Speicher\\Zugesicherte verwendete Bytes(%) | Prozent|
|\\Speicher\\Verfügbare Bytes | Byte|
|\\Speicher\\Zugesicherte Bytes | Byte|
|\\Speicher\\Zusicherungslimit | Byte|
|\\Speicher\\Auslagerungsseiten (Bytes)| Byte|
|\\Speicher\\Nichtauslagerungsseiten (Bytes) | Byte|
|\\Physischer Datenträger(\_Gesamt)\\Zeit (%) | Prozent|
|\\Physischer Datenträger(\_Gesamt)\\Lesezeit (%)| Prozent|
|\\Physischer Datenträger(\_Gesamt)\\Schreibzeit (%) | Prozent|
|\\Physischer Datenträger(\_Gesamt)\\Übertragungen/s | Anzahl pro Sekunde|
|\\Physischer Datenträger(\_Gesamt)\\Lesevorgänge/s | Anzahl pro Sekunde|
|\\Physischer Datenträger(\_Gesamt)\\Schreibvorgänge/s | Anzahl pro Sekunde|
|\\Physischer Datenträger(\_Gesamt)\\Bytes/s | Bytes pro Sekunde|
|\\Physischer Datenträger(\_Gesamt)\\Bytes gelesen/s | Bytes pro Sekunde|
|\\Physischer Datenträger(\_Gesamt)\\Bytes geschrieben/s | Bytes pro Sekunde|
|\\Physischer Datenträger(\_Gesamt)\\Durchschnittl. Warteschlangenlänge des Datenträgers| Anzahl|
|\\Physischer Datenträger(\_Gesamt)\\Durchschnittl. Warteschlangenlänge der Datenträger-Lesevorgänge| Anzahl|
|\\Physischer Datenträger(\_Gesamt)\\Durchschnittl. Warteschlangenlänge der Datenträger-Schreibvorgänge| Anzahl|
|\\Logischer Datenträger(\_Gesamt)\\Freier Speicherplatz (%)| Prozent|
|\\Logischer Datenträger(\_Gesamt)\\MB frei| Anzahl|



### Berechnen von Metriken für Linux-VM v2 als Gastbetriebssystem

Wenn Sie einen neuen virtuellen Computer (v2) in Azure erstellen, ist die Diagnose mithilfe der Diagnoseerweiterung standardmäßig aktiviert.

Sie können eine Liste der Metriken mithilfe des folgenden Befehls in PowerShell generieren.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

 Sie können eine Warnung für die folgenden Metriken erstellen.

|Metrikname|	Unit|
|---|---|
|\\Memory\\AvailableMemory |Byte|
|\\Memory\\PercentAvailableMemory|	Prozent|
|\\Memory\\UsedMemory|	Byte|
|\\Memory\\PercentUsedMemory|	Prozent|
|\\Memory\\PercentUsedByCache |Prozent|
|\\Memory\\PagesPerSec|	Anzahl pro Sekunde|
|\\Memory\\PagesReadPerSec|	Anzahl pro Sekunde|
|\\Memory\\PagesWrittenPerSec |Anzahl pro Sekunde|
|\\Memory\\AvailableSwap |Byte|
|\\Memory\\PercentAvailableSwap|	Prozent|
|\\Memory\\UsedSwap |Byte|
|\\Memory\\PercentUsedSwap|	Prozent|
|\\Processor\\PercentIdleTime|	Prozent|
|\\Processor\\PercentUserTime|	Prozent|
|\\Processor\\PercentNiceTime |Prozent|
|\\Processor\\PercentPrivilegedTime |Prozent|
|\\Processor\\PercentInterruptTime|	Prozent|
|\\Processor\\PercentDPCTime|	Prozent|
|\\Processor\\PercentProcessorTime |Prozent|
|\\Processor\\PercentIOWaitTime |Prozent|
|\\PhysicalDisk\\BytesPerSecond|	Bytes pro Sekunde|
|\\PhysicalDisk\\ReadBytesPerSecond|	Bytes pro Sekunde|
|\\PhysicalDisk\\WriteBytesPerSecond|	Bytes pro Sekunde|
|\\PhysicalDisk\\TransfersPerSecond |Anzahl pro Sekunde|
|\\PhysicalDisk\\ReadsPerSecond |Anzahl pro Sekunde|
|\\PhysicalDisk\\WritesPerSecond |Anzahl pro Sekunde|
|\\PhysicalDisk\\AverageReadTime|	Sekunden|
|\\PhysicalDisk\\AverageWriteTime |Sekunden|
|\\PhysicalDisk\\AverageTransferTime|	Sekunden|
|\\PhysicalDisk\\AverageDiskQueueLength|	Count|
|\\NetworkInterface\\BytesTransmitted |Byte|
|\\NetworkInterface\\BytesReceived |Byte|
|\\NetworkInterface\\PacketsTransmitted |Count|
|\\NetworkInterface\\PacketsReceived |Count|
|\\NetworkInterface\\BytesTotal |Byte|
|\\NetworkInterface\\TotalRxErrors|	Count|
|\\NetworkInterface\\TotalTxErrors|	Count|
|\\NetworkInterface\\TotalCollisions|	Count|




## Häufig verwendete Webmetriken (Serverfarm)

Sie können die automatische Skalierung auch basierend auf allgemeinen Webservermetriken wie der HTTP-Warteschlangenlänge ausführen. Der Metrikname ist **HttpQueueLength**. Im folgenden Abschnitt sind die verfügbaren Serverfarmmetriken (Web-Apps) aufgeführt.

### Web-Apps-Metriken

Sie können eine Liste der Web-Apps-Metriken mithilfe des folgenden Befehls in PowerShell generieren.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Anhand dieser Metriken können Sie Warnungen ausgeben oder skalieren.

|Metrikname|	Unit|
|---|---|
|CpuPercentage|	Prozent|
|MemoryPercentage |Prozent|
|DiskQueueLength|	Count|
|HttpQueueLength|	Count|
|BytesReceived|	Byte|
|BytesSent|	Byte|


## Häufig verwendete Speichermetriken
Sie können nach Speicherwarteschlangenlänge skalieren, wobei es sich um die Anzahl der Nachrichten in der Speicherwarteschlange handelt. Die Speicherwarteschlangenlänge ist eine spezielle Metrik und der angewendete Schwellenwert ist die Anzahl der Nachrichten pro Instanz. Dies bedeutet bei zwei Instanzen und bei einem Schwellenwert von 100 kommt es zu einer Skalierung, wenn die Gesamtanzahl der Nachrichten in der Warteschlange 200 beträgt. Beispiel: 100 Nachrichten pro Instanz.

Sie können dies im Azure-Portal auf dem Blatt **Einstellungen** konfigurieren. Für VM-Skalierungsgruppen können Sie die Einstellung für die automatische Skalierung in der ARM-Vorlage aktualisieren, sodass *metricName* als *ApproximateMessageCount* verwendet wird und die ID der Speicherwarteschlange als *metricResourceUri* übergeben wird.


```
"metricName": "ApproximateMessageCount",
 "metricNamespace": "",
 "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ClassicStorage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
```

## Häufig verwendete Service Bus-Metriken

Sie können nach Service Bus-Warteschlangenlänge skalieren, wobei es sich um die Anzahl der Nachrichten in der Service Bus-Warteschlange handelt. Die Service Bus-Warteschlangenlänge ist eine spezielle Metrik, und der angewendete Schwellenwert ist die Anzahl der Nachrichten pro Instanz. Dies bedeutet bei zwei Instanzen und bei einem Schwellenwert von 100 kommt es zu einer Skalierung, wenn die Gesamtanzahl der Nachrichten in der Warteschlange 200 beträgt. Beispiel: 100 Nachrichten pro Instanz.

Für VM-Skalierungsgruppen können Sie die Einstellung für die automatische Skalierung in der ARM-Vorlage aktualisieren, sodass *metricName* als *ApproximateMessageCount* verwendet und die ID der Speicherwarteschlange als *metricResourceUri* übergeben wird.

```
"metricName": "MessageCount",
 "metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue"
```

>[AZURE.NOTE] Für Service Bus gibt es das Konzept einer Ressourcengruppe nicht, Azure Resource Manager erstellt jedoch eine Standardressourcengruppe pro Region. Die Ressourcengruppe hat in der Regel das Format „Default-ServiceBus-[Region]“. Beispiel: „Default-ServiceBus-EastUS“, „Default-ServiceBus-WestUS“, „Default-ServiceBus-AustraliaEast“ usw.

<!---HONumber=AcomDC_0330_2016-->
