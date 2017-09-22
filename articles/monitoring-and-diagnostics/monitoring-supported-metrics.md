---
title: "Azure Monitor-Metriken: Unterstützte Metriken pro Ressourcentyp | Microsoft Docs"
description: "Liste der Metriken, die mit Azure Monitor für jeden Ressourcentyp zur Verfügung stehen."
author: anirudhcavale
manager: ashwink
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 63d4ac65-1688-40d1-85c8-7cd408285b0f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 9/6/2017
ms.author: ancav
ms.translationtype: HT
ms.sourcegitcommit: 9b7316a5bffbd689bdb26e9524129ceed06606d5
ms.openlocfilehash: 6f40f0cdb998bda9d936ecf87089e85713ed9e1b
ms.contentlocale: de-de
ms.lasthandoff: 09/08/2017

---
# <a name="supported-metrics-with-azure-monitor"></a>Unterstützte Metriken von Azure Monitor
Azure Monitor bietet verschiedene Methoden für die Interaktion mit Metriken, z.B. die Diagrammdarstellung im Portal, den Zugriff über die REST-API oder die Abfrage über PowerShell oder CLI. Unten ist eine vollständige Liste aller Metriken aufgeführt, die derzeit mit der Metrikpipeline von Azure Monitor verfügbar sind.

> [!NOTE]
> Weitere Metriken stehen möglicherweise im Portal oder über Legacy-APIs zur Verfügung. Diese Liste enthält nur Metriken der öffentlichen Vorschau, die über die öffentliche Vorschau der konsolidierten Azure Monitor-Metrikpipeline verfügbar sind.
>
>

## <a name="microsoftanalysisservicesservers"></a>Microsoft.AnalysisServices/servers

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|qpu_metric|QPU|Anzahl|Durchschnitt|QPU. Bereich: 0–100 für S1, 0–200 für S2 und 0–400 für S4|
|memory_metric|Arbeitsspeicher|Byte|Durchschnitt|Arbeitsspeicher. Bereich: 0–25 GB für S1, 0–50 GB für S2 und 0–100 GB für S4|
|TotalConnectionRequests|Total Connection Requests (Verbindungsanforderungen gesamt)|Anzahl|Durchschnitt|Gesamtanzahl von Verbindungsanforderungen. Dies sind erhaltene Anforderungen.|
|SuccessfullConnectionsPerSec|Erfolgreiche Verbindungen pro Sekunde|Anzahl pro Sekunde|Durchschnitt|Rate der erfolgreichen Verbindungsabschlüsse|
|TotalConnectionFailures|Verbindungsfehler gesamt|Anzahl|Durchschnitt|Gesamtanzahl von fehlerhaften Verbindungsversuchen|
|CurrentUserSessions|Aktuelle Benutzersitzungen|Anzahl|Durchschnitt|Aktuelle Anzahl von eingerichteten Benutzersitzungen|
|QueryPoolBusyThreads|Ausgelastete Abfragepoolthreads|Anzahl|Durchschnitt|Anzahl von ausgelasteten Threads im Abfragethreadpool|
|CommandPoolJobQueueLength|Warteschlangenlänge für Aufträge im Befehlspool|Anzahl|Durchschnitt|Gibt die Anzahl von Aufträgen in der Warteschlange des Befehlsthreadpools an.|
|ProcessingPoolJobQueueLength|Warteschlangenlänge für Verarbeitungspoolaufträge|Anzahl|Durchschnitt|Anzahl von Nicht-E/A-Aufträgen in der Warteschlange des Verarbeitungsthreadpools|
|CurrentConnections|Verbindung: Aktuelle Verbindungen|Anzahl|Durchschnitt|Aktuelle Anzahl hergestellter Clientverbindungen.|
|CleanerCurrentPrice|Arbeitsspeicher: Bereinigung – aktueller Preis|Anzahl|Durchschnitt|Aktueller Preis des Arbeitsspeichers, $/Byte/Zeit, normalisiert auf 1000.|
|CleanerMemoryShrinkable|Arbeitsspeicher: Bereinigung – verkleinerbarer Arbeitsspeicher|Byte|Durchschnitt|Die Menge des Arbeitsspeichers in Byte, die durch den Hintergrundbereinigungsprozess bereinigt wird.|
|CleanerMemoryNonshrinkable|Arbeitsspeicher: Bereinigung – nicht verkleinerbarer Arbeitsspeicher|Byte|Durchschnitt|Die Menge des Arbeitsspeichers in Byte, die nicht durch den Hintergrundbereinigungsprozess bereinigt wird.|
|MemoryUsage|Arbeitsspeicher: Speicherauslastung|Byte|Durchschnitt|Speicherauslastung des Serverprozesses, wie bei der Berechnung des Arbeitsspeicherpreises für die Bereinigung verwendet. Entspricht dem Indikator „Process\PrivateBytes“ zuzüglich der Größe der im Speicher abgebildeten Daten. Vom xVelocity-In-Memory-Analysemodul (VertiPaq) abgebildeter oder belegter Arbeitsspeicher, der über die Arbeitsspeichergrenze des xVelocity-Moduls hinausgeht, wird dabei ignoriert.|
|MemoryLimitHard|Arbeitsspeicher: Grenzwert für den festen Speicher|Byte|Durchschnitt|Grenzwert für den festen Speicher gemäß Konfigurationsdatei.|
|MemoryLimitHigh|Arbeitsspeicher: Obere Arbeitsspeichergrenze|Byte|Durchschnitt|Oberer Grenzwert für den Arbeitsspeicher gemäß Konfigurationsdatei.|
|MemoryLimitLow|Arbeitsspeicher: Untere Arbeitsspeichergrenze|Byte|Durchschnitt|Unterer Grenzwert für den Arbeitsspeicher gemäß Konfigurationsdatei.|
|MemoryLimitVertiPaq|Arbeitsspeicher: VertiPaq-Arbeitsspeichergrenze|Byte|Durchschnitt|In-Memory-Grenzwert gemäß Konfigurationsdatei.|
|Kontingent|Arbeitsspeicher: Kontingent|Byte|Durchschnitt|Aktuelles Arbeitsspeicherkontingent in Byte. Das Arbeitsspeicherkontingent wird auch als Speicherzuweisung oder Speicherreservierung bezeichnet.|
|QuotaBlocked|Arbeitsspeicher: Kontingent blockiert|Anzahl|Durchschnitt|Aktuelle Anzahl von Kontingentanforderungen, die blockiert werden, bis andere Arbeitsspeicherkontingente freigegeben werden.|
|VertiPaqNonpaged|Arbeitsspeicher: Nicht ausgelagerte VertiPaq-Daten|Byte|Durchschnitt|Bytes von Arbeitsspeicher, die im Arbeitssatz zur Verwendung durch das In-Memory-Modul gesperrt sind.|
|VertiPaqPaged|Arbeitsspeicher: Ausgelagerte VertiPaq-Daten|Byte|Durchschnitt|Bytes von ausgelagertem Arbeitsspeicher, die für In-Memory-Daten verwendet werden.|
|RowsReadPerSec|Verarbeitung: Gelesene Zeilen pro Sekunde|Anzahl pro Sekunde|Durchschnitt|Rate der aus allen relationalen Datenbanken gelesenen Zeilen.|
|RowsConvertedPerSec|Verarbeitung: Konvertierte Zeilen pro Sekunde|Anzahl pro Sekunde|Durchschnitt|Rate der Zeilen, die bei der Verarbeitung konvertiert werden.|
|RowsWrittenPerSec|Verarbeitung: Geschriebene Zeilen pro Sekunde|Anzahl pro Sekunde|Durchschnitt|Rate der Zeilen, die bei der Verarbeitung geschrieben werden.|
|CommandPoolBusyThreads|Threads: Ausgelastete Threads im Befehlspool|Anzahl|Durchschnitt|Anzahl ausgelasteter Threads im Befehlsthreadpool.|
|CommandPoolIdleThreads|Threads: Leerlaufthreads im Befehlspool|Anzahl|Durchschnitt|Anzahl von Leerlaufthreads im Befehlsthreadpool.|
|LongParsingBusyThreads|Threads: Ausgelastete lange Analysethreads|Anzahl|Durchschnitt|Anzahl ausgelasteter Threads im Pool für lange Analysethreads.|
|LongParsingIdleThreads|Threads: Lange Analysethreads im Leerlauf|Anzahl|Durchschnitt|Anzahl von Leerlaufthreads im Pool für lange Analysethreads.|
|LongParsingJobQueueLength|Threads: Warteschlangenlänge für lange Analyseaufträge|Anzahl|Durchschnitt|Anzahl von Aufträgen in der Warteschlange des Pools für lange Analysethreads.|
|ProcessingPoolBusyIOJobThreads|Threads: Ausgelastete Threads für E/A-Aufträge im Verarbeitungspool|Anzahl|Durchschnitt|Anzahl von Threads, die E/A-Aufträge im Verarbeitungsthreadpool ausführen.|
|ProcessingPoolBusyNonIOThreads|Threads: Ausgelastete Nicht-E/A-Threads im Verarbeitungspool|Anzahl|Durchschnitt|Anzahl von Threads, die Nicht-E/A-Aufträge im Verarbeitungsthreadpool ausführen.|
|ProcessingPoolIOJobQueueLength|Threads: Warteschlangenlänge für E/A-Aufträge im Verarbeitungspool|Anzahl|Durchschnitt|Anzahl von E/A-Aufträgen in der Warteschlange des Verarbeitungsthreadpools.|
|ProcessingPoolIdleIOJobThreads|Threads: Leerlaufthreads für E/A-Aufträge im Verarbeitungspool|Anzahl|Durchschnitt|Anzahl von Leerlaufthreads für E/A-Aufträge im Verarbeitungsthreadpool.|
|ProcessingPoolIdleNonIOThreads|Threads: Nicht-E/A-Leerlaufthreads im Verarbeitungspool|Anzahl|Durchschnitt|Anzahl von Leerlaufthreads im Verarbeitungsthreadpool, die für Nicht-E/A-Aufträge vorgesehen sind.|
|QueryPoolIdleThreads|Threads: Abfragepoolthreads im Leerlauf|Anzahl|Durchschnitt|Anzahl von Leerlaufthreads für E/A-Aufträge im Verarbeitungsthreadpool.|
|QueryPoolJobQueueLength|Threads: Auftragswarteschlangenlänge für Abfragepool|Anzahl|Durchschnitt|Anzahl von Aufträgen in der Warteschlange des Abfragethreadpools.|
|ShortParsingBusyThreads|Threads: Ausgelastete kurze Analysethreads|Anzahl|Durchschnitt|Anzahl ausgelasteter Threads im Pool für kurze Analysethreads.|
|ShortParsingIdleThreads|Threads: Kurze Analysethreads im Leerlauf|Anzahl|Durchschnitt|Anzahl von Leerlaufthreads im Pool für kurze Analysethreads.|
|ShortParsingJobQueueLength|Threads: Warteschlangenlänge für kurze Analyseaufträge|Anzahl|Durchschnitt|Anzahl von Aufträgen in der Warteschlange des Pools für kurze Analysethreads.|
|memory_thrashing_metric|Arbeitsspeicherüberlastung|Prozent|Durchschnitt|Durchschnittliche Arbeitsspeicherüberlastung.|
|mashup_engine_qpu_metric|M-Modul – QPU|Anzahl|Durchschnitt|QPU-Nutzung durch Mashup-Modulprozesse|
|mashup_engine_memory_metric|M-Modul – Arbeitsspeicher|Byte|Durchschnitt|Arbeitsspeichernutzung durch Mashup-Modulprozesse|

## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|TotalRequests|Total Gateway Requests (Gatewayanforderungen gesamt)|Anzahl|Gesamt|Anzahl von Gatewayanforderungen|
|SuccessfulRequests|Successful Gateway Requests (Erfolgreiche Gatewayanforderungen)|Anzahl|Gesamt|Anzahl von erfolgreichen Gatewayanforderungen|
|UnauthorizedRequests|Unauthorized Gateway Requests (Nicht autorisierte Gatewayanforderungen)|Anzahl|Gesamt|Anzahl von nicht autorisierten Gatewayanforderungen|
|FailedRequests|Failed Gateway Requests (Fehlgeschlagene Gatewayanforderungen)|Anzahl|Gesamt|Anzahl von Fehlern bei Gatewayanforderungen|
|OtherRequests|Other Gateway Requests (Sonstige Gatewayanforderungen)|Anzahl|Gesamt|Anzahl von anderen Gatewayanforderungen|
|Duration|Gesamtdauer von Gatewayanforderungen|Millisekunden|Durchschnitt, Maximum|Gesamtdauer von Gatewayanforderungen in Millisekunden|
|Capacity|Kapazität (Vorschau)|Prozent|Durchschnitt, Maximum|Auslastungsmetrik für ApiManagement-Dienst|

## <a name="microsoftautomationautomationaccounts"></a>Microsoft.Automation/automationAccounts

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|TotalJob|Gesamtzahl an Einzelvorgängen (Jobs)|Anzahl|Gesamt|Die Gesamtzahl an Einzelvorgängen (Jobs)|

## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|CoreCount|Dedizierte Anzahl von Kernen|Anzahl|Gesamt|Gesamtzahl der dedizierten Kerne im Batch-Konto|
|TotalNodeCount|Dedizierte Knotenanzahl|Anzahl|Gesamt|Gesamtzahl der dedizierten Knoten im Batch-Konto|
|LowPriorityCoreCount|Anzahl von LowPriority-Kernen|Anzahl|Gesamt|Gesamtzahl der Kerne mit niedriger Priorität im Batch-Konto|
|TotalLowPriorityNodeCount|Anzahl der Knoten mit niedriger Priorität|Anzahl|Gesamt|Gesamtzahl der Knoten mit niedriger Priorität im Batchkonto|
|CreatingNodeCount|Anzahl erstellter Knoten|Anzahl|Gesamt|Anzahl von Knoten, die gerade erstellt werden|
|StartingNodeCount|Anzahl gestarteter Knoten|Anzahl|Gesamt|Anzahl von Knoten, die gerade gestartet werden|
|WaitingForStartTaskNodeCount|Anzahl von Knoten, die auf den Starttask warten|Anzahl|Gesamt|Anzahl von Knoten, die auf den Abschluss des Starttasks warten|
|StartTaskFailedNodeCount|Anzahl von Knoten mit Starttaskfehlern|Anzahl|Gesamt|Anzahl von Knoten, bei denen der Starttask nicht durchgeführt werden konnte|
|IdleNodeCount|Anzahl von Knoten im Leerlauf|Anzahl|Gesamt|Anzahl von Knoten im Leerlauf|
|OfflineNodeCount|Anzahl von Offlineknoten|Anzahl|Gesamt|Anzahl offline geschalteter Knoten|
|RebootingNodeCount|Anzahl neu gestarteter Knoten|Anzahl|Gesamt|Anzahl von Knoten, die neu gestartet werden|
|ReimagingNodeCount|Anzahl von Knoten mit Reimaging|Anzahl|Gesamt|Anzahl von Knoten, für die ein Reimaging durchgeführt wird|
|RunningNodeCount|Anzahl ausgeführter Knoten|Anzahl|Gesamt|Anzahl ausgeführter Knoten|
|LeavingPoolNodeCount|Anzahl von Knoten, die den Pool verlassen|Anzahl|Gesamt|Anzahl von Knoten, die den Pool verlassen|
|UnusableNodeCount|Anzahl nicht verwendbarer Knoten|Anzahl|Gesamt|Anzahl nicht verwendbarer Knoten|
|PreemptedNodeCount|Anzahl der vorzeitig entfernten Knoten|Anzahl|Gesamt|Die Anzahl der vorzeitig entfernten Knoten|
|TaskStartEvent|Taskstartereignisse|Anzahl|Gesamt|Gesamtanzahl gestarteter Tasks|
|TaskCompleteEvent|Taskabschlussereignisse|Anzahl|Gesamt|Gesamtanzahl abgeschlossener Tasks|
|TaskFailEvent|Taskfehlerereignisse|Anzahl|Gesamt|Gesamtanzahl von Tasks, die mit Fehlerstatus abgeschlossen wurden|
|PoolCreateEvent|Poolerstellungsereignisse|Anzahl|Gesamt|Gesamtanzahl erstellter Pools|
|PoolResizeStartEvent|Ereignisse zum Start der Größenänderung von Pools|Anzahl|Gesamt|Gesamtanzahl gestarteter Größenänderungen von Pools|
|PoolResizeCompleteEvent|Ereignisse zum Abschluss der Größenänderung von Pools|Anzahl|Gesamt|Gesamtanzahl abgeschlossener Größenänderungen von Pools|
|PoolDeleteStartEvent|Ereignisse zum Starten des Löschvorgangs von Pools|Anzahl|Gesamt|Gesamtanzahl gestarteter Poollöschvorgänge|
|PoolDeleteCompleteEvent|Ereignisse zum Abschluss des Löschvorgangs von Pools|Anzahl|Gesamt|Gesamtanzahl abgeschlossener Poollöschvorgänge|

## <a name="microsoftcacheredis"></a>Microsoft.Cache/redis

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|connectedclients|Verbundene Clients|Anzahl|Maximum||
|totalcommandsprocessed|Vorgänge gesamt|Anzahl|Gesamt||
|cachehits|Cachetreffer|Anzahl|Gesamt||
|cachemisses|Cachefehler|Anzahl|Gesamt||
|getcommands|get-Vorgänge|Anzahl|Gesamt||
|setcommands|set-Vorgänge|Anzahl|Gesamt||
|evictedkeys|Entfernte Schlüssel|Anzahl|Gesamt||
|totalkeys|Schlüssel insgesamt|Anzahl|Maximum||
|expiredkeys|Abgelaufene Schlüssel|Anzahl|Gesamt||
|usedmemory|Verwendeter Arbeitsspeicher|Byte|Maximum||
|usedmemoryRss|Verwendeter Arbeitsspeicher (RSS)|Byte|Maximum||
|serverLoad|Serverlast|Prozent|Maximum||
|cacheWrite|Cache-Schreibvorgänge|Bytes pro Sekunde|Maximum||
|cacheRead|Cache-Lesevorgänge|Bytes pro Sekunde|Maximum||
|percentProcessorTime|CPU|Prozent|Maximum||
|connectedclients0|Verbundene Clients (Shard 0)|Anzahl|Maximum||
|totalcommandsprocessed0|Vorgänge gesamt (Shard 0)|Anzahl|Gesamt||
|cachehits0|Cachetreffer (Shard 0)|Anzahl|Gesamt||
|cachemisses0|Cachefehler (Shard 0)|Anzahl|Gesamt||
|getcommands0|Get-Vorgänge (Shard 0)|Anzahl|Gesamt||
|setcommands0|Set-Vorgänge (Shard 0)|Anzahl|Gesamt||
|evictedkeys0|Entfernte Schlüssel (Shard 0)|Anzahl|Gesamt||
|totalkeys0|Schlüssel gesamt (Shard 0)|Anzahl|Maximum||
|expiredkeys0|Abgelaufene Schlüssel (Shard 0)|Anzahl|Gesamt||
|usedmemory0|Verwendeter Arbeitsspeicher (Shard 0)|Byte|Maximum||
|usedmemoryRss0|Verwendeter Arbeitsspeicher (RSS, Shard 0)|Byte|Maximum||
|serverLoad0|Serverauslastung (Shard 0)|Prozent|Maximum||
|cacheWrite0|Cacheschreibvorgänge (Shard 0)|Bytes pro Sekunde|Maximum||
|cacheRead0|Cachelesevorgänge (Shard 0)|Bytes pro Sekunde|Maximum||
|percentProcessorTime0|CPU (Shard 0)|Prozent|Maximum||
|connectedclients1|Verbundene Clients (Shard 1)|Anzahl|Maximum||
|totalcommandsprocessed1|Vorgänge gesamt (Shard 1)|Anzahl|Gesamt||
|cachehits1|Cachetreffer (Shard 1)|Anzahl|Gesamt||
|cachemisses1|Cachefehler (Shard 1)|Anzahl|Gesamt||
|getcommands1|Get-Vorgänge (Shard 1)|Anzahl|Gesamt||
|setcommands1|Set-Vorgänge (Shard 1)|Anzahl|Gesamt||
|evictedkeys1|Entfernte Schlüssel (Shard 1)|Anzahl|Gesamt||
|totalkeys1|Schlüssel gesamt (Shard 1)|Anzahl|Maximum||
|expiredkeys1|Abgelaufene Schlüssel (Shard 1)|Anzahl|Gesamt||
|usedmemory1|Verwendeter Arbeitsspeicher (Shard 1)|Byte|Maximum||
|usedmemoryRss1|Verwendeter Arbeitsspeicher (RSS, Shard 1)|Byte|Maximum||
|serverLoad1|Serverauslastung (Shard 1)|Prozent|Maximum||
|cacheWrite1|Cacheschreibvorgänge (Shard 1)|Bytes pro Sekunde|Maximum||
|cacheRead1|Cachelesevorgänge (Shard 1)|Bytes pro Sekunde|Maximum||
|percentProcessorTime1|CPU (Shard 1)|Prozent|Maximum||
|connectedclients2|Verbundene Clients (Shard 2)|Anzahl|Maximum||
|totalcommandsprocessed2|Vorgänge gesamt (Shard 2)|Anzahl|Gesamt||
|cachehits2|Cachetreffer (Shard 2)|Anzahl|Gesamt||
|cachemisses2|Cachefehler (Shard 2)|Anzahl|Gesamt||
|getcommands2|Get-Vorgänge (Shard 2)|Anzahl|Gesamt||
|setcommands2|Set-Vorgänge (Shard 2)|Anzahl|Gesamt||
|evictedkeys2|Entfernte Schlüssel (Shard 2)|Anzahl|Gesamt||
|totalkeys2|Schlüssel gesamt (Shard 2)|Anzahl|Maximum||
|expiredkeys2|Abgelaufene Schlüssel (Shard 2)|Anzahl|Gesamt||
|usedmemory2|Verwendeter Arbeitsspeicher (Shard 2)|Byte|Maximum||
|usedmemoryRss2|Verwendeter Arbeitsspeicher (RSS, Shard 2)|Byte|Maximum||
|serverLoad2|Serverauslastung (Shard 2)|Prozent|Maximum||
|cacheWrite2|Cacheschreibvorgänge (Shard 2)|Bytes pro Sekunde|Maximum||
|cacheRead2|Cachelesevorgänge (Shard 2)|Bytes pro Sekunde|Maximum||
|percentProcessorTime2|CPU (Shard 2)|Prozent|Maximum||
|connectedclients3|Verbundene Clients (Shard 3)|Anzahl|Maximum||
|totalcommandsprocessed3|Vorgänge gesamt (Shard 3)|Anzahl|Gesamt||
|cachehits3|Cachetreffer (Shard 3)|Anzahl|Gesamt||
|cachemisses3|Cachefehler (Shard 3)|Anzahl|Gesamt||
|getcommands3|Get-Vorgänge (Shard 3)|Anzahl|Gesamt||
|setcommands3|Set-Vorgänge (Shard 3)|Anzahl|Gesamt||
|evictedkeys3|Entfernte Schlüssel (Shard 3)|Anzahl|Gesamt||
|totalkeys3|Schlüssel gesamt (Shard 3)|Anzahl|Maximum||
|expiredkeys3|Abgelaufene Schlüssel (Shard 3)|Anzahl|Gesamt||
|usedmemory3|Verwendeter Arbeitsspeicher (Shard 3)|Byte|Maximum||
|usedmemoryRss3|Verwendeter Arbeitsspeicher (RSS, Shard 3)|Byte|Maximum||
|serverLoad3|Serverauslastung (Shard 3)|Prozent|Maximum||
|cacheWrite3|Cacheschreibvorgänge (Shard 3)|Bytes pro Sekunde|Maximum||
|cacheRead3|Cachelesevorgänge (Shard 3)|Bytes pro Sekunde|Maximum||
|percentProcessorTime3|CPU (Shard 3)|Prozent|Maximum||
|connectedclients4|Verbundene Clients (Shard 4)|Anzahl|Maximum||
|totalcommandsprocessed4|Vorgänge gesamt (Shard 4)|Anzahl|Gesamt||
|cachehits4|Cachetreffer (Shard 4)|Anzahl|Gesamt||
|cachemisses4|Cachefehler (Shard 4)|Anzahl|Gesamt||
|getcommands4|Get-Vorgänge (Shard 4)|Anzahl|Gesamt||
|setcommands4|Set-Vorgänge (Shard 4)|Anzahl|Gesamt||
|evictedkeys4|Entfernte Schlüssel (Shard 4)|Anzahl|Gesamt||
|totalkeys4|Schlüssel gesamt (Shard 4)|Anzahl|Maximum||
|expiredkeys4|Abgelaufene Schlüssel (Shard 4)|Anzahl|Gesamt||
|usedmemory4|Verwendeter Arbeitsspeicher (Shard 4)|Byte|Maximum||
|usedmemoryRss4|Verwendeter Arbeitsspeicher (RSS, Shard 4)|Byte|Maximum||
|serverLoad4|Serverauslastung (Shard 4)|Prozent|Maximum||
|cacheWrite4|Cacheschreibvorgänge (Shard 4)|Bytes pro Sekunde|Maximum||
|cacheRead4|Cachelesevorgänge (Shard 4)|Bytes pro Sekunde|Maximum||
|percentProcessorTime4|CPU (Shard 4)|Prozent|Maximum||
|connectedclients5|Verbundene Clients (Shard 5)|Anzahl|Maximum||
|totalcommandsprocessed5|Vorgänge gesamt (Shard 5)|Anzahl|Gesamt||
|cachehits5|Cachetreffer (Shard 5)|Anzahl|Gesamt||
|cachemisses5|Cachefehler (Shard 5)|Anzahl|Gesamt||
|getcommands5|Get-Vorgänge (Shard 5)|Anzahl|Gesamt||
|setcommands5|Set-Vorgänge (Shard 5)|Anzahl|Gesamt||
|evictedkeys5|Entfernte Schlüssel (Shard 5)|Anzahl|Gesamt||
|totalkeys5|Schlüssel gesamt (Shard 5)|Anzahl|Maximum||
|expiredkeys5|Abgelaufene Schlüssel (Shard 5)|Anzahl|Gesamt||
|usedmemory5|Verwendeter Arbeitsspeicher (Shard 5)|Byte|Maximum||
|usedmemoryRss5|Verwendeter Arbeitsspeicher (RSS, Shard 5)|Byte|Maximum||
|serverLoad5|Serverauslastung (Shard 5)|Prozent|Maximum||
|cacheWrite5|Cacheschreibvorgänge (Shard 5)|Bytes pro Sekunde|Maximum||
|cacheRead5|Cachelesevorgänge (Shard 5)|Bytes pro Sekunde|Maximum||
|percentProcessorTime5|CPU (Shard 5)|Prozent|Maximum||
|connectedclients6|Verbundene Clients (Shard 6)|Anzahl|Maximum||
|totalcommandsprocessed6|Vorgänge gesamt (Shard 6)|Anzahl|Gesamt||
|cachehits6|Cachetreffer (Shard 6)|Anzahl|Gesamt||
|cachemisses6|Cachefehler (Shard 6)|Anzahl|Gesamt||
|getcommands6|Get-Vorgänge (Shard 6)|Anzahl|Gesamt||
|setcommands6|Set-Vorgänge (Shard 6)|Anzahl|Gesamt||
|evictedkeys6|Entfernte Schlüssel (Shard 6)|Anzahl|Gesamt||
|totalkeys6|Schlüssel gesamt (Shard 6)|Anzahl|Maximum||
|expiredkeys6|Abgelaufene Schlüssel (Shard 6)|Anzahl|Gesamt||
|usedmemory6|Verwendeter Arbeitsspeicher (Shard 6)|Byte|Maximum||
|usedmemoryRss6|Verwendeter Arbeitsspeicher (RSS, Shard 6)|Byte|Maximum||
|serverLoad6|Serverauslastung (Shard 6)|Prozent|Maximum||
|cacheWrite6|Cacheschreibvorgänge (Shard 6)|Bytes pro Sekunde|Maximum||
|cacheRead6|Cachelesevorgänge (Shard 6)|Bytes pro Sekunde|Maximum||
|percentProcessorTime6|CPU (Shard 6)|Prozent|Maximum||
|connectedclients7|Verbundene Clients (Shard 7)|Anzahl|Maximum||
|totalcommandsprocessed7|Vorgänge gesamt (Shard 7)|Anzahl|Gesamt||
|cachehits7|Cachetreffer (Shard 7)|Anzahl|Gesamt||
|cachemisses7|Cachefehler (Shard 7)|Anzahl|Gesamt||
|getcommands7|Get-Vorgänge (Shard 7)|Anzahl|Gesamt||
|setcommands7|Set-Vorgänge (Shard 7)|Anzahl|Gesamt||
|evictedkeys7|Entfernte Schlüssel (Shard 7)|Anzahl|Gesamt||
|totalkeys7|Schlüssel gesamt (Shard 7)|Anzahl|Maximum||
|expiredkeys7|Abgelaufene Schlüssel (Shard 7)|Anzahl|Gesamt||
|usedmemory7|Verwendeter Arbeitsspeicher (Shard 7)|Byte|Maximum||
|usedmemoryRss7|Verwendeter Arbeitsspeicher (RSS, Shard 7)|Byte|Maximum||
|serverLoad7|Serverauslastung (Shard 7)|Prozent|Maximum||
|cacheWrite7|Cacheschreibvorgänge (Shard 7)|Bytes pro Sekunde|Maximum||
|cacheRead7|Cachelesevorgänge (Shard 7)|Bytes pro Sekunde|Maximum||
|percentProcessorTime7|CPU (Shard 7)|Prozent|Maximum||
|connectedclients8|Verbundene Clients (Shard 8)|Anzahl|Maximum||
|totalcommandsprocessed8|Vorgänge gesamt (Shard 8)|Anzahl|Gesamt||
|cachehits8|Cachetreffer (Shard 8)|Anzahl|Gesamt||
|cachemisses8|Cachefehler (Shard 8)|Anzahl|Gesamt||
|getcommands8|Get-Vorgänge (Shard 8)|Anzahl|Gesamt||
|setcommands8|Set-Vorgänge (Shard 8)|Anzahl|Gesamt||
|evictedkeys8|Entfernte Schlüssel (Shard 8)|Anzahl|Gesamt||
|totalkeys8|Schlüssel gesamt (Shard 8)|Anzahl|Maximum||
|expiredkeys8|Abgelaufene Schlüssel (Shard 8)|Anzahl|Gesamt||
|usedmemory8|Verwendeter Arbeitsspeicher (Shard 8)|Byte|Maximum||
|usedmemoryRss8|Verwendeter Arbeitsspeicher (RSS, Shard 8)|Byte|Maximum||
|serverLoad8|Serverauslastung (Shard 8)|Prozent|Maximum||
|cacheWrite8|Cacheschreibvorgänge (Shard 8)|Bytes pro Sekunde|Maximum||
|cacheRead8|Cachelesevorgänge (Shard 8)|Bytes pro Sekunde|Maximum||
|percentProcessorTime8|CPU (Shard 8)|Prozent|Maximum||
|connectedclients9|Verbundene Clients (Shard 9)|Anzahl|Maximum||
|totalcommandsprocessed9|Vorgänge gesamt (Shard 9)|Anzahl|Gesamt||
|cachehits9|Cachetreffer (Shard 9)|Anzahl|Gesamt||
|cachemisses9|Cachefehler (Shard 9)|Anzahl|Gesamt||
|getcommands9|Get-Vorgänge (Shard 9)|Anzahl|Gesamt||
|setcommands9|Set-Vorgänge (Shard 9)|Anzahl|Gesamt||
|evictedkeys9|Entfernte Schlüssel (Shard 9)|Anzahl|Gesamt||
|totalkeys9|Schlüssel gesamt (Shard 9)|Anzahl|Maximum||
|expiredkeys9|Abgelaufene Schlüssel (Shard 9)|Anzahl|Gesamt||
|usedmemory9|Verwendeter Arbeitsspeicher (Shard 9)|Byte|Maximum||
|usedmemoryRss9|Verwendeter Arbeitsspeicher (RSS, Shard 9)|Byte|Maximum||
|serverLoad9|Serverauslastung (Shard 9)|Prozent|Maximum||
|cacheWrite9|Cacheschreibvorgänge (Shard 9)|Bytes pro Sekunde|Maximum||
|cacheRead9|Cachelesevorgänge (Shard 9)|Bytes pro Sekunde|Maximum||
|percentProcessorTime9|CPU (Shard 9)|Prozent|Maximum||

## <a name="microsoftclassiccomputevirtualmachines"></a>Microsoft.ClassicCompute/virtualMachines

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|Percentage CPU|CPU in Prozent|Prozent|Durchschnitt|Der Prozentsatz der zugewiesenen Computeeinheiten, die derzeit von den virtuellen Computern verwendet werden|
|Network In|Netzwerk eingehend|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen empfangen werden (eingehender Datenverkehr)|
|Network Out|Netzwerk ausgehend|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen gesendet werden (ausgehender Datenverkehr)|
|Disk Read Bytes/Sec|Datenträgerlesevorgänge|Bytes pro Sekunde|Durchschnitt|Durchschnitt an Bytes, die während des Überwachungszeitraums vom Datenträger gelesen werden|
|Disk Write Bytes/Sec|Datenträgerschreibvorgänge|Bytes pro Sekunde|Durchschnitt|Durchschnitt an Bytes, die während des Überwachungszeitraums auf den Datenträger geschrieben werden|
|Disk Read Operations/Sec|Datenträgerlesevorgänge/Sek.|Anzahl pro Sekunde|Durchschnitt|Datenträgerlesevorgänge in IOPS|
|Disk Write Operations/Sec|Datenträgerschreibvorgänge/Sek.|Anzahl pro Sekunde|Durchschnitt|Datenträgerschreibvorgänge in IOPS|

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|TotalCalls|Aufrufe gesamt|Anzahl|Gesamt|Gesamtanzahl von Aufrufen|
|SuccessfulCalls|Erfolgreiche Aufrufe|Anzahl|Gesamt|Anzahl erfolgreicher Aufrufe|
|TotalErrors|Fehler insgesamt|Anzahl|Gesamt|Gesamtzahl von Aufrufen mit Fehlerantwort (HTTP-Antwortcode 4xx oder 5xx)|
|BlockedCalls|Blockierte Aufrufe|Anzahl|Gesamt|Anzahl von Aufrufen, die das Raten- oder Kontingentlimit überschritten haben|
|ServerErrors|Serverfehler|Anzahl|Gesamt|Anzahl von Aufrufen mit internem Dienstfehler (HTTP-Antwortcode 5xx)|
|ClientErrors|Clientfehler|Anzahl|Gesamt|Anzahl von Aufrufen mit Fehler auf Clientseite (HTTP-Antwortcode 4xx)|
|DataIn|Eingehende Daten|Byte|Gesamt|Menge eingehender Daten in Byte|
|DataOut|Datenausgabe|Byte|Gesamt|Menge ausgehender Daten in Byte|
|Latency|Latenz|Millisekunden|Durchschnitt|Latenz in Millisekunden|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|Percentage CPU|CPU in Prozent|Prozent|Durchschnitt|Der Prozentsatz der zugewiesenen Computeeinheiten, die derzeit von den virtuellen Computern verwendet werden|
|Network In|Netzwerk eingehend|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen empfangen werden (eingehender Datenverkehr)|
|Network Out|Netzwerk ausgehend|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen gesendet werden (ausgehender Datenverkehr)|
|Disk Read Bytes|Datenträgerlesevorgänge in Bytes|Byte|Gesamt|Gesamtzahl von Bytes, die während des Überwachungszeitraums vom Datenträger gelesen werden|
|Disk Write Bytes|Datenträgerschreibvorgänge in Bytes|Byte|Gesamt|Gesamtzahl von Bytes, die während des Überwachungszeitraums auf den Datenträger geschrieben werden|
|Disk Read Operations/Sec|Datenträgerlesevorgänge/Sek.|Anzahl pro Sekunde|Durchschnitt|Datenträgerlesevorgänge in IOPS|
|Disk Write Operations/Sec|Datenträgerschreibvorgänge/Sek.|Anzahl pro Sekunde|Durchschnitt|Datenträgerschreibvorgänge in IOPS|

## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|Percentage CPU|CPU in Prozent|Prozent|Durchschnitt|Der Prozentsatz der zugewiesenen Computeeinheiten, die derzeit von den virtuellen Computern verwendet werden|
|Network In|Netzwerk eingehend|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen empfangen werden (eingehender Datenverkehr)|
|Network Out|Netzwerk ausgehend|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen gesendet werden (ausgehender Datenverkehr)|
|Disk Read Bytes|Datenträgerlesevorgänge in Bytes|Byte|Gesamt|Gesamtzahl von Bytes, die während des Überwachungszeitraums vom Datenträger gelesen werden|
|Disk Write Bytes|Datenträgerschreibvorgänge in Bytes|Byte|Gesamt|Gesamtzahl von Bytes, die während des Überwachungszeitraums auf den Datenträger geschrieben werden|
|Disk Read Operations/Sec|Datenträgerlesevorgänge/Sek.|Anzahl pro Sekunde|Durchschnitt|Datenträgerlesevorgänge in IOPS|
|Disk Write Operations/Sec|Datenträgerschreibvorgänge/Sek.|Anzahl pro Sekunde|Durchschnitt|Datenträgerschreibvorgänge in IOPS|

## <a name="microsoftcomputevirtualmachinescalesetsvirtualmachines"></a>Microsoft.Compute/virtualMachineScaleSets/virtualMachines

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|Percentage CPU|CPU in Prozent|Prozent|Durchschnitt|Der Prozentsatz der zugewiesenen Computeeinheiten, die derzeit von den virtuellen Computern verwendet werden|
|Network In|Netzwerk eingehend|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen empfangen werden (eingehender Datenverkehr)|
|Network Out|Netzwerk ausgehend|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen gesendet werden (ausgehender Datenverkehr)|
|Disk Read Bytes|Datenträgerlesevorgänge in Bytes|Byte|Gesamt|Gesamtzahl von Bytes, die während des Überwachungszeitraums vom Datenträger gelesen werden|
|Disk Write Bytes|Datenträgerschreibvorgänge in Bytes|Byte|Gesamt|Gesamtzahl von Bytes, die während des Überwachungszeitraums auf den Datenträger geschrieben werden|
|Disk Read Operations/Sec|Datenträgerlesevorgänge/Sek.|Anzahl pro Sekunde|Durchschnitt|Datenträgerlesevorgänge in IOPS|
|Disk Write Operations/Sec|Datenträgerschreibvorgänge/Sek.|Anzahl pro Sekunde|Durchschnitt|Datenträgerschreibvorgänge in IOPS|

## <a name="microsoftcustomerinsightshubs"></a>Microsoft.CustomerInsights/hubs

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|DCIApiCalls|Customer Insights-API-Aufrufe|Anzahl|Gesamt||
|DCIMappingImportOperationSuccessfulLines|Erfolgreiche Zeilen beim Zuordnen von Importvorgängen|Anzahl|Gesamt||
|DCIMappingImportOperationFailedLines|Fehlgeschlagene Zeilen beim Zuordnen von Importvorgängen|Anzahl|Gesamt||
|DCIMappingImportOperationTotalLines|Gesamte Zeilen beim Zuordnen von Importvorgängen|Anzahl|Gesamt||
|DCIMappingImportOperationRuntimeInSeconds|Laufzeit in Sekunden beim Zuordnen von Importvorgängen|Sekunden|Gesamt||
|DCIOutboundProfileExportSucceeded|Ausgehender Profilexport erfolgreich|Anzahl|Gesamt||
|DCIOutboundProfileExportFailed|Fehler beim ausgehenden Profilexport|Anzahl|Gesamt||
|DCIOutboundProfileExportDuration|Dauer des ausgehenden Profilexports|Sekunden|Gesamt||
|DCIOutboundKpiExportSucceeded|Ausgehender KPI-Export erfolgreich|Anzahl|Gesamt||
|DCIOutboundKpiExportFailed|Fehler beim ausgehenden KPI-Export|Anzahl|Gesamt||
|DCIOutboundKpiExportDuration|Dauer des ausgehenden KPI-Exports|Sekunden|Gesamt||
|DCIOutboundKpiExportStarted|Ausgehender KPI-Export gestartet|Sekunden|Gesamt||
|DCIOutboundKpiRecordCount|Anzahl der ausgehenden KPI-Datensätze|Sekunden|Gesamt||
|DCIOutboundProfileExportCount|Anzahl der ausgehenden Profilexporte|Sekunden|Gesamt||
|DCIOutboundInitialProfileExportFailed|Fehler beim ausgehenden ursprünglichen Profilexport|Sekunden|Gesamt||
|DCIOutboundInitialProfileExportSucceeded|Ausgehender ursprünglicher Profilexport erfolgreich|Sekunden|Gesamt||
|DCIOutboundInitialKpiExportFailed|Fehler beim ausgehenden ursprünglichen KPI-Export|Sekunden|Gesamt||
|DCIOutboundInitialKpiExportSucceeded|Ausgehender ursprünglicher KPI-Export erfolgreich|Sekunden|Gesamt||
|DCIOutboundInitialProfileExportDurationInSeconds|Dauer des ausgehenden ursprünglichen Profilexports in Sekunden|Sekunden|Gesamt||
|AdlaJobForStandardKpiFailed|Fehler beim ADLA-Vorgang für Standard-KPI in Sekunden|Sekunden|Gesamt||
|AdlaJobForStandardKpiTimeOut|Timeout bei ADLA-Vorgang für Standard-KPI in Sekunden|Sekunden|Gesamt||
|AdlaJobForStandardKpiCompleted|ADLA-Vorgang für Standard-KPI abgeschlossen in Sekunden|Sekunden|Gesamt||
|ImportASAValuesFailed|Anzahl der fehlgeschlagenen Importe von ASA-Werten|Anzahl|Gesamt||
|ImportASAValuesSucceeded|Anzahl der erfolgreichen Importe von ASA-Werten|Anzahl|Gesamt||

## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.DataLakeAnalytics/accounts

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|JobEndedSuccess|Erfolgreiche Vorgänge|Anzahl|Gesamt|Die Anzahl der erfolgreichen Vorgänge|
|JobEndedFailure|Fehlgeschlagene Vorgänge|Anzahl|Gesamt|Die Anzahl der fehlgeschlagenen Vorgänge|
|JobEndedCancelled|Abgebrochene Vorgänge|Anzahl|Gesamt|Die Anzahl der abgebrochenen Vorgänge|
|JobAUEndedSuccess|Erfolgreiche AU-Zeit|Sekunden|Gesamt|Die Gesamt-AU-Zeit für erfolgreiche Vorgänge|
|JobAUEndedFailure|Fehlgeschlagene AU-Zeit|Sekunden|Gesamt|Die Gesamt-AU-Zeit für fehlgeschlagene Vorgänge|
|JobAUEndedCancelled|Abgebrochene AU-Zeit|Sekunden|Gesamt|Die Gesamt-AU-Zeit für abgebrochene Vorgänge|

## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.DataLakeStore/accounts

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|TotalStorage|Gesamtspeicher|Byte|Maximum|Gesamtmenge der im Konto gespeicherten Daten|
|DataWritten|Geschriebene Daten|Byte|Gesamt|Gesamtmenge der in das Konto geschriebenen Daten|
|DataRead|Gelesene Daten|Byte|Gesamt|Gesamtmenge der im Konto gelesenen Daten|
|WriteRequests|Schreibanforderungen|Anzahl|Gesamt|Anzahl der Datenschreibanforderungen für das Konto|
|ReadRequests|Leseanforderungen|Anzahl|Gesamt|Anzahl der Datenleseanforderungen für das Konto|

## <a name="microsoftdbformysqlservers"></a>Microsoft.DBforMySQL/servers

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|cpu_percent|CPU in Prozent|Prozent|Durchschnitt|CPU in Prozent|
|compute_limit|Grenzwert für Computeeinheit|Anzahl|Durchschnitt|Grenzwert für Computeeinheit|
|compute_consumption_percent|Prozentsatz von Computeeinheit|Prozent|Durchschnitt|Prozentsatz von Computeeinheit|
|memory_percent|Arbeitsspeicher in Prozent|Prozent|Durchschnitt|Arbeitsspeicher in Prozent|
|io_consumption_percent|E/A in Prozent|Prozent|Durchschnitt|E/A in Prozent|
|storage_percent|Speicher in Prozent|Prozent|Durchschnitt|Speicher in Prozent|
|storage_used|Verwendeter Speicher|Byte|Durchschnitt|Verwendeter Speicher|
|storage_limit|Speicherbegrenzung|Byte|Durchschnitt|Speicherbegrenzung|
|active_connections|Aktive Verbindungen gesamt|Anzahl|Durchschnitt|Aktive Verbindungen gesamt|
|connections_failed|Fehlerhafte Verbindungen gesamt|Anzahl|Durchschnitt|Fehlerhafte Verbindungen gesamt|

## <a name="microsoftdbforpostgresqlservers"></a>Microsoft.DBforPostgreSQL/servers

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|cpu_percent|CPU in Prozent|Prozent|Durchschnitt|CPU in Prozent|
|compute_limit|Grenzwert für Computeeinheit|Anzahl|Durchschnitt|Grenzwert für Computeeinheit|
|compute_consumption_percent|Prozentsatz von Computeeinheit|Prozent|Durchschnitt|Prozentsatz von Computeeinheit|
|memory_percent|Arbeitsspeicher in Prozent|Prozent|Durchschnitt|Arbeitsspeicher in Prozent|
|io_consumption_percent|E/A in Prozent|Prozent|Durchschnitt|E/A in Prozent|
|storage_percent|Speicher in Prozent|Prozent|Durchschnitt|Speicher in Prozent|
|storage_used|Verwendeter Speicher|Byte|Durchschnitt|Verwendeter Speicher|
|storage_limit|Speicherbegrenzung|Byte|Durchschnitt|Speicherbegrenzung|
|active_connections|Aktive Verbindungen gesamt|Anzahl|Durchschnitt|Aktive Verbindungen gesamt|
|connections_failed|Fehlerhafte Verbindungen gesamt|Anzahl|Durchschnitt|Fehlerhafte Verbindungen gesamt|

## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/IotHubs

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|d2c.telemetry.Ingress.allProtocol|Telemetry message send attempts (Sendeversuche für Telemetrienachrichten)|Anzahl|Gesamt|Anzahl von Telemetrienachrichten vom Gerät an die Cloud, die an Ihren IoT Hub gesendet werden sollten|
|d2c.telemetry.ingress.success|Telemetry messages sent (Gesendete Telemetrienachrichten)|Anzahl|Gesamt|Anzahl von Telemetrienachrichten vom Gerät an die Cloud, die erfolgreich an Ihren IoT Hub gesendet wurden|
|c2d.commands.egress.complete.success|Commands completed (Abgeschlossene Befehle)|Anzahl|Gesamt|Anzahl von Befehlen von der Cloud an das Gerät, die vom Gerät erfolgreich abgeschlossen wurden|
|c2d.commands.egress.abandon.success|Commands abandoned (Abgebrochene Befehle)|Anzahl|Gesamt|Anzahl von Befehlen von der Cloud an das Gerät, die vom Gerät abgebrochen wurden|
|c2d.commands.egress.reject.success|Commands rejected (Abgelehnte Befehle)|Anzahl|Gesamt|Anzahl von Befehlen von der Cloud an das Gerät, die vom Gerät abgelehnt wurden|
|devices.totalDevices|Total devices (Geräte gesamt)|Anzahl|Gesamt|Die Anzahl von Geräten, die beim IoT Hub registriert sind|
|devices.connectedDevices.allProtocol|Verbundene Geräte|Anzahl|Gesamt|Die Anzahl von Geräten, die mit dem IoT Hub verbunden sind|
|d2c.telemetry.egress.success|Telemetry messages delivered (Übermittelte Telemetrienachrichten)|Anzahl|Gesamt|Erfolgreich auf Endpunkte geschriebene Nachrichten (Häufigkeit gesamt)|
|d2c.telemetry.egress.dropped|Dropped messages (Gelöschte Nachrichten)|Anzahl|Gesamt|Anzahl der Nachrichten, die aufgrund eines inaktiven Zustellungsendpunkts gelöscht wurden|
|d2c.telemetry.egress.orphaned|Verwaiste Nachrichten|Anzahl|Gesamt|Anzahl von Nachrichten, die keiner Route entsprechen (einschließlich der Fallbackroute)|
|d2c.telemetry.egress.invalid|Invalid messages (Ungültige Nachrichten)|Anzahl|Gesamt|Anzahl von Nachrichten, die aufgrund von Inkompatibilität mit dem Endpunkt nicht übermittelt wurden|
|d2c.telemetry.egress.fallback|Messages matching fallback condition (Nachrichten, die die Fallbackbedingung erfüllen)|Anzahl|Gesamt|Gibt die Anzahl von Nachrichten an, die auf den Fallbackendpunkt geschrieben werden.|
|d2c.endpoints.egress.eventHubs|Messages delivered to Event Hub endpoints (An Event Hub-Endpunkte übermittelte Nachrichten)|Anzahl|Gesamt|Gibt an, wie oft Nachrichten erfolgreich auf Event Hub-Endpunkte geschrieben wurden.Gibt an, wie oft Nachrichten erfolgreich auf Event Hub-Endpunkte geschrieben wurden.|
|d2c.endpoints.latency.eventHubs|Message latency for Event Hub endpoints (Nachrichtenwartezeit für Event Hub-Endpunkte)|Millisekunden|Durchschnitt|Durchschnittliche Wartezeit zwischen dem Eingang der Nachricht beim IoT-Hub und dem Eingang der Nachricht bei einem Event Hub-Endpunkt in Millisekunden|
|d2c.endpoints.egress.serviceBusQueues|Messages delivered to Service Bus Queue endpoints (An Service Bus-Warteschlangenendpunkte übermittelte Nachrichten)|Anzahl|Gesamt|Gibt an, wie oft Nachrichten erfolgreich auf Service Bus-Warteschlangenendpunkte geschrieben wurden.|
|d2c.endpoints.latency.serviceBusQueues|Message latency for Service Bus Queue endpoints (Nachrichtenwartezeit für Service Bus-Warteschlangenendpunkte)|Millisekunden|Durchschnitt|Durchschnittliche Wartezeit zwischen dem Eingang der Nachricht beim IoT-Hub und dem Eingang der Nachricht bei einem Service Bus-Warteschlangenendpunkt in Millisekunden|
|d2c.endpoints.egress.serviceBusTopics|Messages delivered to Service Bus Topic endpoints (An Service Bus-Themenendpunkte übermittelte Nachrichten)|Anzahl|Gesamt|Gibt an, wie oft Nachrichten erfolgreich auf Service Bus-Themenendpunkte geschrieben wurden.|
|d2c.endpoints.latency.serviceBusTopics|Message latency for Service Bus Topic endpoints (Nachrichtenwartezeit für Service Bus-Themenendpunkte)|Millisekunden|Durchschnitt|Durchschnittliche Wartezeit zwischen dem Eingang der Nachricht beim IoT-Hub und dem Eingang der Nachricht bei einem Service Bus-Themenendpunkt in Millisekunden|
|d2c.endpoints.egress.builtIn.events|Messages delivered to the built-in endpoint (messages/events) (An den integrierten Endpunkt (messages/events) übermittelte Nachrichten)|Anzahl|Gesamt|Gibt an, wie oft Nachrichten erfolgreich auf den integrierten Endpunkt (messages/events) geschrieben wurden.|
|d2c.endpoints.latency.builtIn.events|Message latency for the built-in endpoint (messages/events) (Nachrichtenwartezeit für den integrierten Endpunkt (messages/events))|Millisekunden|Durchschnitt|Durchschnittliche Wartezeit zwischen dem Eingang der Nachricht beim IoT-Hub und dem Eingang der Nachricht beim integrierten Endpunkt (Nachrichten/Ereignisse) in Millisekunden |
|d2c.endpoints.egress.storage|An Storage-Endpunkte übermittelte Nachrichten|Anzahl|Gesamt|Anzahl der erfolgreichen Schreibvorgänge von Nachrichten auf Storage-Endpunkten|
|d2c.endpoints.latency.storage|Nachrichtenwartezeit für Storage-Endpunkte|Millisekunden|Durchschnitt|Durchschnittliche Wartezeit zwischen dem Eingang der Nachricht beim IoT-Hub und dem Eingang der Nachricht auf einem Storage-Endpunkt in Millisekunden|
|d2c.endpoints.egress.storage.bytes|In den Speicher geschriebene Daten|Byte|Gesamt|Menge der Daten in Bytes, die auf Storage-Endpunkten geschrieben wurden|
|d2c.endpoints.egress.storage.blobs|In den Speicher geschriebene Blobs|Anzahl|Gesamt|Anzahl von Blobs, die auf Storage-Endpunkten geschrieben werden|
|d2c.twin.read.success|Successful twin reads from devices (Erfolgreiche Zwillingslesevorgänge von Geräten)|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Zwillingslesevorgängen an, die vom Gerät initiiert wurden.|
|d2c.twin.read.failure|Failed twin reads from devices (Nicht erfolgreiche Zwillingslesevorgänge von Geräten)|Anzahl|Gesamt|Gibt die Anzahl von nicht erfolgreichen Zwillingslesevorgängen an, die vom Gerät initiiert wurden.|
|d2c.twin.read.size|Response size of twin reads from devices (Antwortgröße von Zwillingslesevorgängen von Geräten)|Byte|Durchschnitt|Durchschnitts-, Minimal- und Maximalwert für alle erfolgreichen Zwillingslesevorgänge, die vom Gerät initiiert wurden.|
|d2c.twin.update.success|Successful twin updates from devices (Erfolgreiche Zwillingsaktualisierungen von Geräten)|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Zwillingsaktualisierungen an, die vom Gerät initiiert wurden.|
|d2c.twin.update.failure|Failed twin updates from devices (Nicht erfolgreiche Zwillingsaktualisierungen von Geräten)|Anzahl|Gesamt|Gibt die Anzahl von nicht erfolgreichen Zwillingsaktualisierungen an, die vom Gerät initiiert wurden.|
|d2c.twin.update.size|Size of twin updates from devices (Größe der Zwillingsaktualisierungen von Geräten)|Byte|Durchschnitt|Durchschnitts-, Minimal- und Maximalgröße für alle erfolgreichen Zwillingsaktualisierungen, die vom Gerät initiiert wurden.|
|c2d.methods.success|Successful direct method invocations (Erfolgreiche direkte Methodenaufrufvorgänge)|Anzahl|Gesamt|Gibt an, wie viele direkte Methodenaufrufe erfolgreich durchgeführt wurden.|
|c2d.methods.failure|Failed direct method invocations (Nicht erfolgreiche direkte Methodenaufrufe)|Anzahl|Gesamt|Gibt an, wie viele direkte Methodenaufrufe nicht erfolgreich waren.|
|c2d.methods.requestSize|Request size of direct method invocations (Anforderungsgröße von direkten Methodenaufrufen)|Byte|Durchschnitt|Gibt den Durchschnitts-, Minimal- und Maximalwert für alle erfolgreichen direkten Methodenaufrufe an.|
|c2d.methods.responseSize|Response size of direct method invocations (Antwortgröße von direkten Methodenaufrufen)|Byte|Durchschnitt|Gibt den Durchschnitts-, Minimal- und Maximalwert für alle erfolgreichen Antworten für die direkte Methode an.|
|c2d.twin.read.success|Successful twin reads from back end (Erfolgreiche Zwillingslesevorgänge vom Back-End)|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Zwillingslesevorgängen an, die vom Back-End initiiert wurden.|
|c2d.twin.read.failure|Failed twin reads from back end (Nicht erfolgreiche Zwillingslesevorgänge vom Back-End)|Anzahl|Gesamt|Gibt die Anzahl von nicht erfolgreichen Zwillingslesevorgängen an, die vom Back-End initiiert wurden.|
|c2d.twin.read.size|Response size of twin reads from back end (Antwortgröße von Zwillingslesevorgängen vom Back-End)|Byte|Durchschnitt|Durchschnitts-, Minimal- und Maximalwert für alle erfolgreichen Zwillingslesevorgänge, die vom Back-End initiiert wurden.|
|c2d.twin.update.success|Successful twin updates from back end (Erfolgreiche Zwillingsaktualisierungen vom Back-End)|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Zwillingsaktualisierungen an, die vom Back-End initiiert wurden.|
|c2d.twin.update.failure|Failed twin updates from back end (Nicht erfolgreiche Zwillingsaktualisierungen vom Back-End)|Anzahl|Gesamt|Gibt die Anzahl von nicht erfolgreichen Zwillingsaktualisierungen an, die vom Back-End initiiert wurden.|
|c2d.twin.update.size|Size of twin updates from back end (Größe der Zwillingsaktualisierungen vom Back-End)|Byte|Durchschnitt|Durchschnitts-, Minimal- und Maximalgröße für alle erfolgreichen Zwillingsaktualisierungen, die vom Back-End initiiert wurden.|
|twinQueries.success|Successful twin queries (Erfolgreiche Zwillingsabfragen)|Anzahl|Gesamt|Gibt an, wie viele erfolgreiche Zwillingsabfragen durchgeführt wurden.|
|twinQueries.failure|Failed twin queries (Nicht erfolgreiche Zwillingsabfragen)|Anzahl|Gesamt|Gibt an, wie viele nicht erfolgreiche Zwillingsabfragen durchgeführt wurden.|
|twinQueries.resultSize|Twin queries result size (Ergebnisgröße von Zwillingsabfragen)|Byte|Durchschnitt|Durchschnitts-, Minimal- und Maximalwert der Ergebnisgröße aller erfolgreichen Zwillingsabfragen.|
|jobs.createTwinUpdateJob.success|Successful creations of twin update jobs (Erfolgreiche Erstellungen von Zwillingsaktualisierungsaufträgen)|Anzahl|Gesamt|Gibt die Anzahl von allen erfolgreichen Erstellungen von Zwillingsaktualisierungsaufträgen an.|
|jobs.createTwinUpdateJob.failure|Failed creations of twin update jobs (Nicht erfolgreiche Erstellungen von Zwillingsaktualisierungsaufträgen)|Anzahl|Gesamt|Gibt die Anzahl von allen nicht erfolgreichen Erstellungen von Zwillingsaktualisierungsaufträgen an.|
|jobs.createDirectMethodJob.success|Successful creations of method invocation jobs (Erfolgreiche Erstellungen von Methodenaufrufaufträgen)|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Erstellungen von Aufträgen für direkte Methodenaufrufe an.|
|jobs.createDirectMethodJob.failure|Failed creations of method invocation jobs (Nicht erfolgreiche Erstellungen von Methodenaufrufaufträgen)|Anzahl|Gesamt|Gibt die Anzahl von nicht erfolgreichen Erstellungen von Aufträgen für direkte Methodenaufrufe an.|
|jobs.listJobs.success|Successful calls to list jobs (Erfolgreiche Aufrufe von Auflistungsaufträgen)|Anzahl|Gesamt|Gibt an, wie viele erfolgreiche Aufrufe von Auflistungsaufträgen durchgeführt wurden.|
|jobs.listJobs.failure|Failed calls to list jobs (Nicht erfolgreiche Aufrufe von Auflistungsaufträgen)|Anzahl|Gesamt|Gibt an, wie viele nicht erfolgreiche Aufrufe von Auflistungsaufträgen durchgeführt wurden.|
|jobs.cancelJob.success|Successful job cancellations (Erfolgreiche Auftragsabbrüche)|Anzahl|Gesamt|Gibt an, wie viele erfolgreiche Aufrufe von Auftragsabbrüchen durchgeführt wurden.|
|jobs.cancelJob.failure|Failed job cancellations (Nicht erfolgreiche Auftragsabbrüche)|Anzahl|Gesamt|Gibt an, wie viele nicht erfolgreiche Aufrufe von Auftragsabbrüchen durchgeführt wurden.|
|jobs.queryJobs.success|Successful job queries (Erfolgreiche Auftragsabfragen)|Anzahl|Gesamt|Gibt an, wie viele erfolgreiche Aufrufe von Abfrageaufträgen durchgeführt wurden.|
|jobs.queryJobs.failure|Failed job queries (Nicht erfolgreiche Auftragsabfragen)|Anzahl|Gesamt|Gibt an, wie viele nicht erfolgreiche Aufrufe von Abfrageaufträgen durchgeführt wurden.|
|jobs.completed|Abgeschlossene Aufträge|Anzahl|Gesamt|Gibt die Anzahl von abgeschlossenen Aufträgen an.|
|jobs.failed|Fehlerhafte Aufträge|Anzahl|Gesamt|Gibt die Anzahl aller fehlerhaften Aufträge an.|
|d2c.telemetry.ingress.sendThrottle|Anzahl von Drosselungsfehlern|Anzahl|Gesamt|Anzahl von Drosselungsfehlern aufgrund von Drosselungen des Gerätedurchsatzes|
|dailyMessageQuotaUsed|Gesamtzahl verwendeter Nachrichten|Anzahl|Durchschnitt|Gesamtzahl der heute verwendeten Nachrichten|

## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|INREQS|Eingehende Sendeanforderungen|Anzahl|Gesamt|Gesamtzahl eingehender Sendeanforderungen für einen Notification Hub|
|SUCCREQ|Erfolgreiche Anforderungen|Anzahl|Gesamt|Gesamtzahl der erfolgreichen Anforderungen für einen Namespace|
|FAILREQ|Anforderungsfehler|Anzahl|Gesamt|Gesamtzahl der fehlgeschlagenen Anforderungen für einen Namespace|
|SVRBSY|Fehler durch ausgelasteten Server|Anzahl|Gesamt|Gesamtzahl der Fehler durch ausgelasteten Server für einen Namespace|
|INTERR|Interne Serverfehler|Anzahl|Gesamt|Gesamtzahl der internen Serverfehler für einen Namespace|
|MISCERR|Andere Fehler|Anzahl|Gesamt|Gesamtzahl der fehlgeschlagenen Anforderungen für einen Namespace|
|INMSGS|Eingehende Nachrichten|Anzahl|Gesamt|Gesamtzahl der eingehenden Nachrichten für einen Namespace|
|OUTMSGS|Ausgehende Nachrichten|Anzahl|Gesamt|Gesamtzahl der ausgehenden Nachrichten für einen Namespace|
|EHINMBS|Eingehende Bytes|Byte|Gesamt|Event Hub-Durchsatz eingehender Nachrichten für einen Namespace|
|EHOUTMBS|Ausgehende Bytes|Byte|Gesamt|Gesamtzahl der ausgehenden Nachrichten für einen Namespace|
|EHABL|Archivierte Backlognachrichten|Anzahl|Gesamt|Event Hub-Archivnachrichten im Backlog für einen Namespace|
|EHAMSGS|Archivnachrichten|Anzahl|Gesamt|Event Hub-Archivnachrichten in einem Namespace|
|EHAMBS|Durchsatz von Archivnachrichten|Byte|Gesamt|Event Hub-Durchsatz archivierter Nachrichten in einem Namespace|

## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|RunsStarted|Gestartete Ausführungen|Anzahl|Gesamt|Anzahl gestarteter Workflowausführungen.|
|RunsCompleted|Abgeschlossene Ausführungen|Anzahl|Gesamt|Anzahl abgeschlossener Workflowausführungen.|
|RunsSucceeded|Erfolgreiche Ausführungen|Anzahl|Gesamt|Anzahl erfolgreicher Workflowausführungen.|
|RunsFailed|Ausführungsfehler|Anzahl|Gesamt|Anzahl fehlerhafter Workflowausführungen.|
|RunsCancelled|Abgebrochene Ausführungen|Anzahl|Gesamt|Anzahl abgebrochener Workflowausführungen.|
|RunLatency|Ausführungslatenz|Sekunden|Durchschnitt|Latenz abgeschlossener Workflowausführungen.|
|RunSuccessLatency|Latenz erfolgreicher Ausführungen|Sekunden|Durchschnitt|Latenz erfolgreicher Workflowausführungen.|
|RunThrottledEvents|Ereignisse zu gedrosselten Ausführungen|Anzahl|Gesamt|Anzahl von Ereignissen zu gedrosselten Workflowaktionen oder Triggern.|
|RunFailurePercentage|Prozentsatz der Ausführungsfehler|Prozent|Gesamt|Prozentsatz fehlerhafter Workflowausführungen|
|ActionsStarted|Gestartete Aktionen |Anzahl|Gesamt|Anzahl gestarteter Workflowaktionen.|
|ActionsCompleted|Abgeschlossene Aktionen |Anzahl|Gesamt|Anzahl abgeschlossener Workflowaktionen.|
|ActionsSucceeded|Erfolgreiche Aktionen |Anzahl|Gesamt|Anzahl erfolgreicher Workflowaktionen.|
|ActionsFailed|Aktionsfehler|Anzahl|Gesamt|Anzahl fehlerhafter Workflowaktionen.|
|ActionsSkipped|Übersprungene Aktionen |Anzahl|Gesamt|Anzahl übersprungener Workflowaktionen.|
|ActionLatency|Aktionslatenz |Sekunden|Durchschnitt|Latenz abgeschlossener Workflowaktionen.|
|ActionSuccessLatency|Latenz erfolgreicher Aktionen |Sekunden|Durchschnitt|Latenz erfolgreicher Workflowaktionen.|
|ActionThrottledEvents|Ereignisse zu gedrosselten Aktionen|Anzahl|Gesamt|Anzahl von Ereignissen zu gedrosselten Workflowaktionen.|
|TriggersStarted|Gestartete Trigger |Anzahl|Gesamt|Anzahl gestarteter Workflowtrigger.|
|TriggersCompleted|Abgeschlossene Trigger |Anzahl|Gesamt|Anzahl abgeschlossener Workflowtrigger.|
|TriggersSucceeded|Erfolgreiche Trigger |Anzahl|Gesamt|Anzahl erfolgreicher Workflowtrigger.|
|TriggersFailed|Triggerfehler |Anzahl|Gesamt|Anzahl fehlerhafter Workflowtrigger.|
|TriggersSkipped|Übersprungene Trigger|Anzahl|Gesamt|Anzahl übersprungener Workflowtrigger.|
|TriggersFired|Ausgelöste Trigger |Anzahl|Gesamt|Anzahl ausgelöster Workflowtrigger.|
|TriggerLatency|Triggerlatenz |Sekunden|Durchschnitt|Latenz abgeschlossener Workflowtrigger.|
|TriggerFireLatency|Latenz beim Auslösen von Triggern |Sekunden|Durchschnitt|Latenz ausgelöster Workflowtrigger.|
|TriggerSuccessLatency|Latenz erfolgreicher Trigger |Sekunden|Durchschnitt|Latenz erfolgreicher Workflowtrigger.|
|TriggerThrottledEvents|Ereignisse zu gedrosselten Triggern|Anzahl|Gesamt|Anzahl von Ereignissen zu gedrosselten Workflowtriggern.|
|BillableActionExecutions|Ausführungen abrechenbarer Aktionen|Anzahl|Gesamt|Anzahl der Workflowaktionsausführungen, die in Rechnung gestellt werden|
|BillableTriggerExecutions|Ausführungen abrechenbarer Trigger|Anzahl|Gesamt|Anzahl der Workflowtriggerausführungen, die in Rechnung gestellt werden|
|TotalBillableExecutions|Gesamtzahl der abrechenbaren Ausführungen|Anzahl|Gesamt|Anzahl der Workflowausführungen, die in Rechnung gestellt werden|

## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|Throughput|Durchsatz|Bytes pro Sekunde|Durchschnitt|Anzahl von Bytes pro Sekunde, die das Application Gateway bereitgestellt hat|

## <a name="microsoftnetworkexpressroutecircuits"></a>Microsoft.Network/expressRouteCircuits

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|BytesIn|BytesIn|Anzahl|Gesamt|In Azure eingehende Bytes|
|BytesOut|BytesOut|Anzahl|Gesamt|Aus Azure ausgehende Bytes|

## <a name="microsoftnotificationhubsnamespacesnotificationhubs"></a>Microsoft.NotificationHubs/Namespaces/NotificationHubs

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|registration.all|Registration Operation (Registrierungsvorgang)|Anzahl|Gesamt|Gibt die Anzahl von allen erfolgreichen Registrierungsvorgängen an (Erstellungen, Aktualisierungen, Abfragen und Löschungen). |
|registration.create|Registrierungserstellungsvorgänge|Anzahl|Gesamt|Gibt die Anzahl von allen erfolgreichen Registrierungserstellungen an.|
|registration.update|Registrierungsaktualisierungsvorgänge|Anzahl|Gesamt|Gibt die Anzahl von allen erfolgreichen Registrierungsaktualisierungen an.|
|registration.get|Registrierungslesevorgänge|Anzahl|Gesamt|Gibt die Anzahl von allen erfolgreichen Registrierungsabfragen an.|
|registration.delete|Registrierungslöschvorgänge|Anzahl|Gesamt|Gibt die Anzahl von allen erfolgreichen Registrierungslöschungen an.|
|incoming|Eingehende Nachrichten|Anzahl|Gesamt|Gibt die Anzahl von allen erfolgreichen Übermittlungs-API-Aufrufen an. |
|incoming.scheduled|Geplante Pushbenachrichtigungen (gesendet)|Anzahl|Gesamt|Geplante Pushbenachrichtigungen (abgebrochen)|
|incoming.scheduled.cancel|Geplante Pushbenachrichtigungen (abgebrochen)|Anzahl|Gesamt|Geplante Pushbenachrichtigungen (abgebrochen)|
|scheduled.pending|Ausstehende Pushbenachrichtigungen|Anzahl|Gesamt|Ausstehende Pushbenachrichtigungen|
|installation.all|Installationsverwaltungsvorgänge|Anzahl|Gesamt|Installationsverwaltungsvorgänge|
|installation.get|Installationsvorgänge abrufen|Anzahl|Gesamt|Installationsvorgänge abrufen|
|installation.upsert|Installationsvorgänge erstellen oder aktualisieren|Anzahl|Gesamt|Installationsvorgänge erstellen oder aktualisieren|
|installation.patch|Patchinstallationsvorgänge|Anzahl|Gesamt|Patchinstallationsvorgänge|
|installation.delete|Installationsvorgänge löschen|Anzahl|Gesamt|Installationsvorgänge löschen|
|outgoing.allpns.success|Erfolgreiche Benachrichtigungen|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Benachrichtigungen an.|
|outgoing.allpns.invalidpayload|Nutzlastfehler|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil vom PNS ein Nutzlastfehler zurückgegeben wurde.|
|outgoing.allpns.pnserror|Fehler des externen Benachrichtigungssystems|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil ein Problem bei der Kommunikation mit dem PNS vorlag (ausschließlich Authentifizierungsprobleme).|
|outgoing.allpns.channelerror|Kanalfehler|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil der Kanal ungültig, nicht der richtigen App zugeordnet, gedrosselt oder abgelaufen war.|
|outgoing.allpns.badorexpiredchannel|Kanalfehler (unzulässig oder abgelaufen)|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil der Kanal, das Token oder die Registrierungs-ID in der Registrierung abgelaufen oder ungültig war.|
|outgoing.wns.success|Erfolgreiche WNS-Benachrichtigungen|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Benachrichtigungen an.|
|outgoing.wns.invalidcredentials|WNS-Autorisierungsfehler (ungültige Anmeldeinformationen)|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil das PNS die angegebenen Anmeldeinformationen nicht akzeptiert hat oder die Anmeldeinformationen blockiert wurden. (Windows Live erkennt die Anmeldeinformationen nicht.)|
|outgoing.wns.badchannel|WNS-Fehler durch fehlerhaften Kanal|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil der ChannelURI in der Registrierung nicht erkannt wurde (WNS-Status: 404 – Nicht gefunden).|
|outgoing.wns.expiredchannel|WNS-Fehler durch abgelaufenen Kanal|Anzahl|Gesamt|Die Anzahl von nicht erfolgreichen Pushvorgängen, weil der ChannelURI abgelaufen ist (WNS-Status: 410 – Fehlend).|
|outgoing.wns.throttled|Gedrosselte WNS-Benachrichtigungen|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil diese App vom WNS gedrosselt wird (WNS-Status: 406 – Nicht annehmbar).|
|outgoing.wns.tokenproviderunreachable|WNS-Autorisierungsfehler (nicht erreichbar)|Anzahl|Gesamt|Windows Live ist nicht erreichbar.|
|outgoing.wns.invalidtoken|WNS-Autorisierungsfehler (ungültiges Token)|Anzahl|Gesamt|Das für WNS bereitgestellte Token ist nicht gültig (WNS-Status: 401 – Nicht autorisiert).|
|outgoing.wns.wrongtoken|WNS-Autorisierungsfehler (falsches Token)|Anzahl|Gesamt|Das für WNS bereitgestellte Token ist gültig, gilt aber für eine andere Anwendung (WNS-Status: 403 – Verboten). Dieser Fall kann eintreten, wenn der ChannelURI in der Registrierung einer anderen App zugeordnet wird. Stellen Sie sicher, dass die Client-App der App zugeordnet ist, deren Anmeldeinformationen sich im Notification Hub befinden.|
|outgoing.wns.invalidnotificationformat|Ungültiges WNS-Benachrichtigungsformat|Anzahl|Gesamt|Das Format der Benachrichtigung ist ungültig (WNS-Status: 400). Beachten Sie, dass von WNS nicht alle ungültigen Nutzlasten abgelehnt werden.|
|outgoing.wns.invalidnotificationsize|WNS-Fehler durch ungültige Benachrichtigungsgröße|Anzahl|Gesamt|Die Benachrichtigungsnutzlast ist zu groß (WNS-Status: 413).|
|outgoing.wns.channelthrottled|WNS-Kanal gedrosselt|Anzahl|Gesamt|Die Benachrichtigung wurde verworfen, weil der ChannelURI in der Registrierung gedrosselt wurde (WNS-Antwortheader: X-WNS-NotificationStatus:channelThrottled).|
|outgoing.wns.channeldisconnected|WNS-Kanal nicht verbunden|Anzahl|Gesamt|Die Benachrichtigung wurde verworfen, weil der ChannelURI in der Registrierung gedrosselt wurde (WNS-Antwortheader: X-WNS-DeviceConnectionStatus: disconnected).|
|outgoing.wns.dropped|Verworfene WNS-Benachrichtigungen|Anzahl|Gesamt|Die Benachrichtigung wurde verworfen, weil der ChannelURI in der Registrierung gedrosselt wurde („X-WNS-NotificationStatus: dropped“, aber nicht „X-WNS-DeviceConnectionStatus: disconnected“).|
|outgoing.wns.pnserror|WNS-Fehler|Anzahl|Gesamt|Die Benachrichtigung wurde aufgrund von Fehlern bei der Kommunikation mit WNS nicht übermittelt.|
|outgoing.wns.authenticationerror|WNS-Authentifizierungsfehler|Anzahl|Gesamt|Die Benachrichtigung wurde nicht übermittelt, weil Fehler bei der Kommunikation mit Windows Live aufgetreten sind, die Anmeldeinformationen ungültig waren oder das falsche Token verwendet wurde.|
|outgoing.apns.success|Erfolgreiche APNS-Benachrichtigungen|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Benachrichtigungen an.|
|outgoing.apns.invalidcredentials|APNS-Autorisierungsfehler|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil das PNS die angegebenen Anmeldeinformationen nicht akzeptiert hat oder die Anmeldeinformationen blockiert wurden.|
|outgoing.apns.badchannel|APNS-Fehler durch fehlerhaften Kanal|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil das Token ungültig ist (APNS-Statuscode: 8).|
|outgoing.apns.expiredchannel|APNS-Fehler durch abgelaufenen Kanal|Anzahl|Gesamt|Die Anzahl von Token, die aufgrund des APNS-Feedbackkanals ungültig gemacht wurden.|
|outgoing.apns.invalidnotificationsize|APNS-Fehler durch ungültige Benachrichtigungsgröße|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil die Nutzlast zu groß war (APNS-Statuscode: 7).|
|outgoing.apns.pnserror|APNS-Fehler|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil Fehler bei der Kommunikation mit APNS aufgetreten sind.|
|outgoing.gcm.success|Erfolgreiche GCM-Benachrichtigungen|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Benachrichtigungen an.|
|outgoing.gcm.invalidcredentials|GCM-Autorisierungsfehler (ungültige Anmeldeinformationen)|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil das PNS die angegebenen Anmeldeinformationen nicht akzeptiert hat oder die Anmeldeinformationen blockiert wurden.|
|outgoing.gcm.badchannel|GCM-Fehler durch fehlerhaften Kanal|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil die registrationId in der Registrierung nicht erkannt wurde (GCM-Ergebnis: Ungültige Registrierung).|
|outgoing.gcm.expiredchannel|GCM-Fehler durch abgelaufenen Kanal|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil die registrationId in der Registrierung abgelaufen war (GCM-Ergebnis: NotRegistered).|
|outgoing.gcm.throttled|Gedrosselte GCM-Benachrichtigungen|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil diese App von GCM gedrosselt wurde (GCM-Statuscode: 501 - 599 oder result:Unavailable).|
|outgoing.gcm.invalidnotificationformat|Ungültiges GCM-Benachrichtigungsformat|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil die Nutzlast nicht richtig formatiert war (GCM-Ergebnis: InvalidDataKey oder InvalidTtl).|
|outgoing.gcm.invalidnotificationsize|GCM-Fehler durch ungültige Benachrichtigungsgröße|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil die Nutzlast zu groß war (GCM-Ergebnis: MessageTooBig).|
|outgoing.gcm.wrongchannel|GCM-Fehler durch falschen Kanal|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil die registrationId in der Registrierung nicht der aktuellen App zugeordnet war (GCM-Ergebnis: InvalidPackageName).|
|outgoing.gcm.pnserror|GCM-Fehler|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil Fehler bei der Kommunikation mit GCM aufgetreten sind.|
|outgoing.gcm.authenticationerror|GCM-Authentifizierungsfehler|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil die angegebenen Anmeldeinformationen vom PNS nicht akzeptiert wurden, die Anmeldeinformationen blockiert wurden oder die SenderId in der App nicht richtig konfiguriert wurde (GCM-Ergebnis: MismatchedSenderId).|
|outgoing.mpns.success|Erfolgreiche MPNS-Benachrichtigungen|Anzahl|Gesamt|Gibt die Anzahl von erfolgreichen Benachrichtigungen an.|
|outgoing.mpns.invalidcredentials|Ungültige MPNS-Anmeldeinformationen|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil das PNS die angegebenen Anmeldeinformationen nicht akzeptiert hat oder die Anmeldeinformationen blockiert wurden.|
|outgoing.mpns.badchannel|MPNS-Fehler durch fehlerhaften Kanal|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil der ChannelURI in der Registrierung nicht erkannt wurde (MPNS-Status: 404 – Nicht gefunden).|
|outgoing.mpns.throttled|Gedrosselte MPNS-Benachrichtigungen|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil diese App vom MPNS gedrosselt wird (WNS-MPNS: 406 – Nicht annehmbar).|
|outgoing.mpns.invalidnotificationformat|Ungültiges MPNS-Benachrichtigungsformat|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil die Nutzlast der Benachrichtigung zu groß war.|
|outgoing.mpns.channeldisconnected|MPNS-Kanal nicht verbunden|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil die Verbindung für den ChannelURI in der Registrierung getrennt wurde (MPNS-Status: 412 – Nicht gefunden).|
|outgoing.mpns.dropped|Verworfene MPNS-Benachrichtigungen|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die von MPNS verworfen wurden (MPNS-Antwortheader: X-NotificationStatus: QueueFull or Suppressed).|
|outgoing.mpns.pnserror|MPNS-Fehler|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil Fehler bei der Kommunikation mit MPNS aufgetreten sind.|
|outgoing.mpns.authenticationerror|MPNS-Authentifizierungsfehler|Anzahl|Gesamt|Gibt die Anzahl von Pushvorgängen an, die nicht erfolgreich waren, weil das PNS die angegebenen Anmeldeinformationen nicht akzeptiert hat oder die Anmeldeinformationen blockiert wurden.|
|notificationhub.pushes|Alle ausgehenden Benachrichtigungen|Anzahl|Gesamt|Alle ausgehenden Benachrichtigungen des Benachrichtigungs-Hub|
|incoming.all.requests|Alle eingehenden Anforderungen|Anzahl|Gesamt|Gesamtzahl eingehender Anforderungen für einen Notification Hub|
|incoming.all.failedrequests|Alle eingehenden fehlerhaften Anforderungen|Anzahl|Gesamt|Gesamtzahl eingehender fehlerhafter Anforderungen für einen Notification Hub|

## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|SearchLatency|Suchlatenz|Sekunden|Durchschnitt|Durchschnittliche Suchlatenz für den Suchdienst|
|SearchQueriesPerSecond|Suchabfragen pro Sekunde|Anzahl pro Sekunde|Durchschnitt|Suchabfragen pro Sekunde für den Suchdienst|
|ThrottledSearchQueriesPercentage|Gedrosselte Suchabfragen in Prozent|Prozent|Durchschnitt|Prozentsatz der Suchabfragen, die für den Suchdienst gedrosselt wurden|

## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|CPUXNS|CPU-Auslastung pro Namespace|Prozent|Maximum|CPU-Auslastungsmetrik für Service Bus-Premium-Namespace|
|WSXNS|Auslastung der Arbeitsspeichergröße pro Namespace|Prozent|Maximum|Speicherauslastungsmetrik für Service Bus-Premium-Namespace|

## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|cpu_percent|CPU-Prozentsatz|Prozent|Durchschnitt|CPU-Prozentsatz|
|physical_data_read_percent|E/A-Prozentsatz für Daten|Prozent|Durchschnitt|E/A-Prozentsatz für Daten|
|log_write_percent|E/A-Prozentsatz für Protokoll|Prozent|Durchschnitt|E/A-Prozentsatz für Protokoll|
|dtu_consumption_percent|DTU-Prozentsatz|Prozent|Durchschnitt|DTU-Prozentsatz|
|storage|Datenbankgröße gesamt|Byte|Maximum|Datenbankgröße gesamt|
|connection_successful|Erfolgreiche Verbindungen|Anzahl|Gesamt|Erfolgreiche Verbindungen|
|connection_failed|Verbindungsfehler|Anzahl|Gesamt|Verbindungsfehler|
|blocked_by_firewall|Von der Firewall blockiert|Anzahl|Gesamt|Von der Firewall blockiert|
|deadlock|Deadlocks|Anzahl|Gesamt|Deadlocks|
|storage_percent|Datenbankgröße als Prozentsatz|Prozent|Maximum|Datenbankgröße als Prozentsatz|
|xtp_storage_percent|In-Memory-OLTP-Speicher in Prozent|Prozent|Durchschnitt|In-Memory-OLTP-Speicher in Prozent|
|workers_percent|Worker in Prozent|Prozent|Durchschnitt|Worker in Prozent|
|sessions_percent|Sitzungen in Prozent|Prozent|Durchschnitt|Sitzungen in Prozent|
|dtu_limit|DTU-Grenzwert|Anzahl|Durchschnitt|DTU-Grenzwert|
|dtu_used|DTU-Verbrauch|Anzahl|Durchschnitt|DTU-Verbrauch|
|dwu_limit|DWU-Grenzwert|Anzahl|Maximum|DWU-Grenzwert|
|dwu_consumption_percent|DWU in Prozent|Prozent|Maximum|DWU in Prozent|
|dwu_used|DWU-Verbrauch|Anzahl|Maximum|DWU-Verbrauch|

## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|cpu_percent|CPU-Prozentsatz|Prozent|Durchschnitt|CPU-Prozentsatz|
|database_cpu_percent|CPU-Prozentsatz|Prozent|Durchschnitt|CPU-Prozentsatz|
|physical_data_read_percent|E/A-Prozentsatz für Daten|Prozent|Durchschnitt|E/A-Prozentsatz für Daten|
|database_physical_data_read_percent|E/A-Prozentsatz für Daten|Prozent|Durchschnitt|E/A-Prozentsatz für Daten|
|log_write_percent|E/A-Prozentsatz für Protokoll|Prozent|Durchschnitt|E/A-Prozentsatz für Protokoll|
|database_log_write_percent|E/A-Prozentsatz für Protokoll|Prozent|Durchschnitt|E/A-Prozentsatz für Protokoll|
|dtu_consumption_percent|DTU-Prozentsatz|Prozent|Durchschnitt|DTU-Prozentsatz|
|database_dtu_consumption_percent|DTU-Prozentsatz|Prozent|Durchschnitt|DTU-Prozentsatz|
|storage_percent|Speicher in Prozent|Prozent|Durchschnitt|Speicher in Prozent|
|workers_percent|Worker in Prozent|Prozent|Durchschnitt|Worker in Prozent|
|database_workers_percent|Worker in Prozent|Prozent|Durchschnitt|Worker in Prozent|
|sessions_percent|Sitzungen in Prozent|Prozent|Durchschnitt|Sitzungen in Prozent|
|database_sessions_percent|Sitzungen in Prozent|Prozent|Durchschnitt|Sitzungen in Prozent|
|eDTU_limit|eDTU-Grenzwert|Anzahl|Durchschnitt|eDTU-Grenzwert|
|storage_limit|Speicherbegrenzung|Byte|Durchschnitt|Speicherbegrenzung|
|eDTU_used|eDTU-Verbrauch|Anzahl|Durchschnitt|eDTU-Verbrauch|
|storage_used|Verwendeter Speicher|Byte|Durchschnitt|Verwendeter Speicher|
|database_storage_used|Verwendeter Speicher|Byte|Durchschnitt|Verwendeter Speicher|
|xtp_storage_percent|In-Memory-OLTP-Speicher in Prozent|Prozent|Durchschnitt|In-Memory-OLTP-Speicher in Prozent|

## <a name="microsoftsqlservers"></a>Microsoft.Sql/servers

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|dtu_consumption_percent|DTU-Prozentsatz|Prozent|Durchschnitt|DTU-Prozentsatz|
|database_dtu_consumption_percent|DTU-Prozentsatz|Prozent|Durchschnitt|DTU-Prozentsatz|
|storage_used|Verwendeter Speicher|Byte|Durchschnitt|Verwendeter Speicher|
|database_storage_used|Verwendeter Speicher|Byte|Durchschnitt|Verwendeter Speicher|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|ResourceUtilization|Speichereinheitnutzung in %|Prozent|Maximum|Speichereinheitnutzung in %|
|InputEvents|Eingabeereignisse|Anzahl|Gesamt|Eingabeereignisse|
|InputEventBytes|Eingabeereignisbytes|Byte|Gesamt|Eingabeereignisbytes|
|LateInputEvents|Ereignisse bei verspäteter Eingabe|Anzahl|Gesamt|Ereignisse bei verspäteter Eingabe|
|OutputEvents|Ausgabeereignisse|Anzahl|Gesamt|Ausgabeereignisse|
|ConversionErrors|Konvertierungsfehler|Anzahl|Gesamt|Konvertierungsfehler|
|Errors|Laufzeitfehler|Anzahl|Gesamt|Laufzeitfehler|
|DroppedOrAdjustedEvents|Ereignisse für falsche Reihenfolge|Anzahl|Gesamt|Ereignisse für falsche Reihenfolge|
|AMLCalloutRequests|Funktionsanforderungen|Anzahl|Gesamt|Funktionsanforderungen|
|AMLCalloutFailedRequests|Fehlerhafte Funktionsanforderungen|Anzahl|Gesamt|Fehlerhafte Funktionsanforderungen|
|AMLCalloutInputEvents|Funktionsereignisse|Anzahl|Gesamt|Funktionsereignisse|

## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|CpuPercentage|CPU-Prozentsatz|Prozent|Durchschnitt|CPU-Prozentsatz|
|MemoryPercentage|Arbeitsspeicherprozentsatz|Prozent|Durchschnitt|Arbeitsspeicherprozentsatz|
|DiskQueueLength|Warteschlangenlänge des Datenträgers|Anzahl|Gesamt|Warteschlangenlänge des Datenträgers|
|HttpQueueLength|Länge der HTTP-Warteschlange|Anzahl|Gesamt|Länge der HTTP-Warteschlange|
|BytesReceived|Eingehende Daten|Byte|Gesamt|Eingehende Daten|
|BytesSent|Datenausgabe|Byte|Gesamt|Datenausgabe|

## <a name="microsoftwebsites-excluding-functions"></a>Microsoft.Web/sites (Funktionen ausgenommen)

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|CpuTime|CPU-Zeit|Sekunden|Gesamt|CPU-Zeit|
|Anforderungen|Anforderungen|Anzahl|Gesamt|Anforderungen|
|BytesReceived|Eingehende Daten|Byte|Gesamt|Eingehende Daten|
|BytesSent|Datenausgabe|Byte|Gesamt|Datenausgabe|
|Http101|HTTP 101|Anzahl|Gesamt|HTTP 101|
|Http2xx|HTTP 2xx|Anzahl|Gesamt|HTTP 2xx|
|Http3xx|HTTP 3xx|Anzahl|Gesamt|HTTP 3xx|
|Http401|HTTP 401|Anzahl|Gesamt|HTTP 401|
|Http403|HTTP 403|Anzahl|Gesamt|HTTP 403|
|Http404|HTTP 404|Anzahl|Gesamt|HTTP 404|
|Http406|HTTP 406|Anzahl|Gesamt|HTTP 406|
|Http4xx|HTTP 4xx|Anzahl|Gesamt|HTTP 4xx|
|Http5xx|HTTP-Serverfehler|Anzahl|Gesamt|HTTP-Serverfehler|
|MemoryWorkingSet|Arbeitssatz für Arbeitsspeicher|Byte|Durchschnitt|Arbeitssatz für Arbeitsspeicher|
|AverageMemoryWorkingSet|Durchschnittlicher Arbeitssatz für Arbeitsspeicher|Byte|Durchschnitt|Durchschnittlicher Arbeitssatz für Arbeitsspeicher|
|AverageResponseTime|Durchschnittliche Antwortzeit|Sekunden|Durchschnitt|Durchschnittliche Antwortzeit|

## <a name="microsoftwebsites-functions"></a>Microsoft.Web/sites (Funktionen)

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|BytesReceived|Eingehende Daten|Byte|Gesamt|Eingehende Daten|
|BytesSent|Datenausgabe|Byte|Gesamt|Datenausgabe|
|Http5xx|HTTP-Serverfehler|Anzahl|Gesamt|HTTP-Serverfehler|
|MemoryWorkingSet|Arbeitssatz für Arbeitsspeicher|Byte|Durchschnitt|Arbeitssatz für Arbeitsspeicher|
|AverageMemoryWorkingSet|Durchschnittlicher Arbeitssatz für Arbeitsspeicher|Byte|Durchschnitt|Durchschnittlicher Arbeitssatz für Arbeitsspeicher|
|FunctionExecutionUnits|Ausführungseinheiten für Funktion|Anzahl|Durchschnitt|Ausführungseinheiten für Funktion|
|FunctionExecutionCount|Ausführungsanzahl für Funktion|Anzahl|Durchschnitt|Ausführungsanzahl für Funktion|

## <a name="microsoftwebsitesslots"></a>Microsoft.Web/sites/slots

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|CpuTime|CPU-Zeit|Sekunden|Gesamt|CPU-Zeit|
|Anforderungen|Anforderungen|Anzahl|Gesamt|Anforderungen|
|BytesReceived|Eingehende Daten|Byte|Gesamt|Eingehende Daten|
|BytesSent|Datenausgabe|Byte|Gesamt|Datenausgabe|
|Http101|HTTP 101|Anzahl|Gesamt|HTTP 101|
|Http2xx|HTTP 2xx|Anzahl|Gesamt|HTTP 2xx|
|Http3xx|HTTP 3xx|Anzahl|Gesamt|HTTP 3xx|
|Http401|HTTP 401|Anzahl|Gesamt|HTTP 401|
|Http403|HTTP 403|Anzahl|Gesamt|HTTP 403|
|Http404|HTTP 404|Anzahl|Gesamt|HTTP 404|
|Http406|HTTP 406|Anzahl|Gesamt|HTTP 406|
|Http4xx|HTTP 4xx|Anzahl|Gesamt|HTTP 4xx|
|Http5xx|HTTP-Serverfehler|Anzahl|Gesamt|HTTP-Serverfehler|
|MemoryWorkingSet|Arbeitssatz für Arbeitsspeicher|Byte|Durchschnitt|Arbeitssatz für Arbeitsspeicher|
|AverageMemoryWorkingSet|Durchschnittlicher Arbeitssatz für Arbeitsspeicher|Byte|Durchschnitt|Durchschnittlicher Arbeitssatz für Arbeitsspeicher|
|AverageResponseTime|Durchschnittliche Antwortzeit|Sekunden|Durchschnitt|Durchschnittliche Antwortzeit|
|FunctionExecutionUnits|Ausführungseinheiten für Funktion|Anzahl|Durchschnitt|Ausführungseinheiten für Funktion|
|FunctionExecutionCount|Ausführungsanzahl für Funktion|Anzahl|Durchschnitt|Ausführungsanzahl für Funktion|

## <a name="microsoftwebhostingenvironmentsmultirolepools"></a>Microsoft.Web/hostingEnvironments/multiRolePools

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|Requests|Anforderungen|Anzahl|Gesamt|Anforderungen|
|BytesReceived|Eingehende Daten|Byte|Gesamt|Eingehende Daten|
|BytesSent|Datenausgabe|Byte|Gesamt|Datenausgabe|
|Http101|HTTP 101|Anzahl|Gesamt|HTTP 101|
|Http2xx|HTTP 2xx|Anzahl|Gesamt|HTTP 2xx|
|Http3xx|HTTP 3xx|Anzahl|Gesamt|HTTP 3xx|
|Http401|HTTP 401|Anzahl|Gesamt|HTTP 401|
|Http403|HTTP 403|Anzahl|Gesamt|HTTP 403|
|Http404|HTTP 404|Anzahl|Gesamt|HTTP 404|
|Http406|HTTP 406|Anzahl|Gesamt|HTTP 406|
|Http4xx|HTTP 4xx|Anzahl|Gesamt|HTTP 4xx|
|Http5xx|HTTP-Serverfehler|Anzahl|Gesamt|HTTP-Serverfehler|
|AverageResponseTime|Durchschnittliche Antwortzeit|Sekunden|Durchschnitt|Durchschnittliche Antwortzeit|
|CpuPercentage|CPU-Prozentsatz|Prozent|Durchschnitt|CPU-Prozentsatz|
|MemoryPercentage|Arbeitsspeicherprozentsatz|Prozent|Durchschnitt|Arbeitsspeicherprozentsatz|
|DiskQueueLength|Warteschlangenlänge des Datenträgers|Anzahl|Gesamt|Warteschlangenlänge des Datenträgers|
|HttpQueueLength|Länge der HTTP-Warteschlange|Anzahl|Gesamt|Länge der HTTP-Warteschlange|
|ActiveRequests|Aktive Anforderungen|Anzahl|Gesamt|Aktive Anforderungen|
|TotalFrontEnds|Gesamtanzahl an Front-Ends|Anzahl|Durchschnitt|Gesamtanzahl an Front-Ends|
|SmallAppServicePlanInstances|Kleine Worker im App Service-Plan|Anzahl|Durchschnitt|Kleine Worker im App Service-Plan|
|MediumAppServicePlanInstances|Mittlere Worker im App Service-Plan|Anzahl|Durchschnitt|Mittlere Worker im App Service-Plan|
|LargeAppServicePlanInstances|Große Worker im App Service-Plan|Anzahl|Durchschnitt|Große Worker im App Service-Plan|

## <a name="microsoftwebhostingenvironmentsworkerpools"></a>Microsoft.Web/hostingEnvironments/workerPools

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|WorkersTotal|Gesamtanzahl an Workern|Anzahl|Durchschnitt|Gesamtanzahl an Workern|
|WorkersAvailable|Verfügbare Worker|Anzahl|Durchschnitt|Verfügbare Worker|
|WorkersUsed|Verwendete Worker|Anzahl|Durchschnitt|Verwendete Worker|

## <a name="next-steps"></a>Nächste Schritte
* [Informationen zu Metriken in Azure Monitor](monitoring-overview-metrics.md)
* [Erstellen von Warnungen zu Metriken](insights-receive-alert-notifications.md)
* [Exportieren von Metriken in Storage, Event Hub oder Log Analytics](monitoring-overview-of-diagnostic-logs.md)

