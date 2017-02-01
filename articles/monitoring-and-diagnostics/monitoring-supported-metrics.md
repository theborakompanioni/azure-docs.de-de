---
title: "Azure Monitor-Metriken: Unterstützte Metriken pro Ressourcentyp | Microsoft Docs"
description: "Liste der Metriken, die mit Azure Monitor für jeden Ressourcentyp zur Verfügung stehen."
author: johnkemnetz
manager: rboucher
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 63d4ac65-1688-40d1-85c8-7cd408285b0f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/12/2016
ms.author: johnkem
translationtype: Human Translation
ms.sourcegitcommit: 5aa823259a86d361d13af9f6c2df062362c7e47b
ms.openlocfilehash: 2faa881f146cc1b0a8c0523f2fb0f20b0136441e


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
|qpu_metric|QPU|Count|Durchschnitt|QPU. Bereich: 0–100 für S1, 0–200 für S2 und 0–400 für S4|
|memory_metric|Arbeitsspeicher|Byte|Durchschnitt|Arbeitsspeicher. Bereich: 0–25 GB für S1, 0–50 GB für S2 und 0–100 GB für S4|

## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|TotalRequests|Total Gateway Requests (Gatewayanforderungen gesamt)|Count|Summe|Anzahl von Gatewayanforderungen|
|TotalSuccessfulRequests|Successful Gateway Requests (Erfolgreiche Gatewayanforderungen)|Count|Summe|Anzahl verarbeiteter Gatewayanforderungen|
|TotalFailedRequests|Failed Gateway Requests (Fehlgeschlagene Gatewayanforderungen)|Count|Summe|Anzahl von Fehlern bei Gatewayanforderungen|

## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|CoreCount|Anzahl von Kernen|Count|Gesamt|Gesamtzahl der Kerne im Batch-Konto|
|TotalNodeCount|Anzahl von Knoten|Count|Gesamt|Gesamtzahl der Knoten im Batch-Konto|
|CreatingNodeCount|Anzahl erstellter Knoten|Count|Gesamt|Anzahl von Knoten, die gerade erstellt werden|
|StartingNodeCount|Anzahl gestarteter Knoten|Count|Gesamt|Anzahl von Knoten, die gerade gestartet werden|
|WaitingForStartTaskNodeCount|Anzahl von Knoten, die auf den Starttask warten|Count|Gesamt|Anzahl von Knoten, die auf den Abschluss des Starttasks warten|
|StartTaskFailedNodeCount|Anzahl von Knoten mit Starttaskfehlern|Count|Gesamt|Anzahl von Knoten, bei denen der Starttask nicht durchgeführt werden konnte|
|IdleNodeCount|Anzahl von Knoten im Leerlauf|Count|Gesamt|Anzahl von Knoten im Leerlauf|
|OfflineNodeCount|Anzahl von Offlineknoten|Count|Gesamt|Anzahl offline geschalteter Knoten|
|RebootingNodeCount|Anzahl neu gestarteter Knoten|Count|Gesamt|Anzahl von Knoten, die neu gestartet werden|
|ReimagingNodeCount|Anzahl von Knoten mit Reimaging|Count|Gesamt|Anzahl von Knoten, für die ein Reimaging durchgeführt wird|
|RunningNodeCount|Anzahl ausgeführter Knoten|Count|Gesamt|Anzahl ausgeführter Knoten|
|LeavingPoolNodeCount|Anzahl von Knoten, die den Pool verlassen|Count|Gesamt|Anzahl von Knoten, die den Pool verlassen|
|UnusableNodeCount|Anzahl nicht verwendbarer Knoten|Count|Gesamt|Anzahl nicht verwendbarer Knoten|
|TaskStartEvent|Taskstartereignisse|Count|Gesamt|Gesamtanzahl gestarteter Tasks|
|TaskCompleteEvent|Taskabschlussereignisse|Count|Gesamt|Gesamtanzahl abgeschlossener Tasks|
|TaskFailEvent|Taskfehlerereignisse|Count|Gesamt|Gesamtanzahl von Tasks, die mit Fehlerstatus abgeschlossen wurden|
|PoolCreateEvent|Poolerstellungsereignisse|Count|Gesamt|Gesamtanzahl erstellter Pools|
|PoolResizeStartEvent|Ereignisse zum Start der Größenänderung von Pools|Count|Gesamt|Gesamtanzahl gestarteter Größenänderungen von Pools|
|PoolResizeCompleteEvent|Ereignisse zum Abschluss der Größenänderung von Pools|Count|Gesamt|Gesamtanzahl abgeschlossener Größenänderungen von Pools|
|PoolDeleteStartEvent|Ereignisse zum Starten des Löschvorgangs von Pools|Count|Gesamt|Gesamtanzahl gestarteter Poollöschvorgänge|
|PoolDeleteCompleteEvent|Ereignisse zum Abschluss des Löschvorgangs von Pools|Count|Gesamt|Gesamtanzahl abgeschlossener Poollöschvorgänge|

## <a name="microsoftcacheredis"></a>Microsoft.Cache/redis

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|connectedclients|Verbundene Clients|Count|Maximum||
|totalcommandsprocessed|Vorgänge gesamt|Count|Gesamt||
|cachehits|Cachetreffer|Count|Gesamt||
|cachemisses|Cachefehler|Count|Gesamt||
|getcommands|get-Vorgänge|Count|Gesamt||
|setcommands|set-Vorgänge|Count|Gesamt||
|evictedkeys|Entfernte Schlüssel|Count|Gesamt||
|totalkeys|Schlüssel insgesamt|Count|Maximum||
|expiredkeys|Abgelaufene Schlüssel|Count|Gesamt||
|usedmemory|Verwendeter Arbeitsspeicher|Byte|Maximum||
|usedmemoryRss|Verwendeter Arbeitsspeicher (RSS)|Byte|Maximum||
|serverLoad|Serverlast|Prozent|Maximum||
|cacheWrite|Cache-Schreibvorgänge|Bytes pro Sekunde|Maximum||
|cacheRead|Cache-Lesevorgänge|Bytes pro Sekunde|Maximum||
|percentProcessorTime|CPU|Prozent|Maximum||
|connectedclients0|Verbundene Clients (Shard 0)|Count|Maximum||
|totalcommandsprocessed0|Vorgänge gesamt (Shard 0)|Count|Gesamt||
|cachehits0|Cachetreffer (Shard 0)|Count|Gesamt||
|cachemisses0|Cachefehler (Shard 0)|Count|Gesamt||
|getcommands0|Get-Vorgänge (Shard 0)|Count|Gesamt||
|setcommands0|Set-Vorgänge (Shard 0)|Count|Gesamt||
|evictedkeys0|Entfernte Schlüssel (Shard 0)|Count|Gesamt||
|totalkeys0|Schlüssel gesamt (Shard 0)|Count|Maximum||
|expiredkeys0|Abgelaufene Schlüssel (Shard 0)|Count|Gesamt||
|usedmemory0|Verwendeter Arbeitsspeicher (Shard 0)|Byte|Maximum||
|usedmemoryRss0|Verwendeter Arbeitsspeicher (RSS, Shard 0)|Byte|Maximum||
|serverLoad0|Serverauslastung (Shard 0)|Prozent|Maximum||
|cacheWrite0|Cacheschreibvorgänge (Shard 0)|Bytes pro Sekunde|Maximum||
|cacheRead0|Cachelesevorgänge (Shard 0)|Bytes pro Sekunde|Maximum||
|percentProcessorTime0|CPU (Shard 0)|Prozent|Maximum||
|connectedclients1|Verbundene Clients (Shard 1)|Count|Maximum||
|totalcommandsprocessed1|Vorgänge gesamt (Shard 1)|Count|Gesamt||
|cachehits1|Cachetreffer (Shard 1)|Count|Gesamt||
|cachemisses1|Cachefehler (Shard 1)|Count|Gesamt||
|getcommands1|Get-Vorgänge (Shard 1)|Count|Gesamt||
|setcommands1|Set-Vorgänge (Shard 1)|Count|Gesamt||
|evictedkeys1|Entfernte Schlüssel (Shard 1)|Count|Gesamt||
|totalkeys1|Schlüssel gesamt (Shard 1)|Count|Maximum||
|expiredkeys1|Abgelaufene Schlüssel (Shard 1)|Count|Gesamt||
|usedmemory1|Verwendeter Arbeitsspeicher (Shard 1)|Byte|Maximum||
|usedmemoryRss1|Verwendeter Arbeitsspeicher (RSS, Shard 1)|Byte|Maximum||
|serverLoad1|Serverauslastung (Shard 1)|Prozent|Maximum||
|cacheWrite1|Cacheschreibvorgänge (Shard 1)|Bytes pro Sekunde|Maximum||
|cacheRead1|Cachelesevorgänge (Shard 1)|Bytes pro Sekunde|Maximum||
|percentProcessorTime1|CPU (Shard 1)|Prozent|Maximum||
|connectedclients2|Verbundene Clients (Shard 2)|Count|Maximum||
|totalcommandsprocessed2|Vorgänge gesamt (Shard 2)|Count|Gesamt||
|cachehits2|Cachetreffer (Shard 2)|Count|Gesamt||
|cachemisses2|Cachefehler (Shard 2)|Count|Gesamt||
|getcommands2|Get-Vorgänge (Shard 2)|Count|Gesamt||
|setcommands2|Set-Vorgänge (Shard 2)|Count|Gesamt||
|evictedkeys2|Entfernte Schlüssel (Shard 2)|Count|Gesamt||
|totalkeys2|Schlüssel gesamt (Shard 2)|Count|Maximum||
|expiredkeys2|Abgelaufene Schlüssel (Shard 2)|Count|Gesamt||
|usedmemory2|Verwendeter Arbeitsspeicher (Shard 2)|Byte|Maximum||
|usedmemoryRss2|Verwendeter Arbeitsspeicher (RSS, Shard 2)|Byte|Maximum||
|serverLoad2|Serverauslastung (Shard 2)|Prozent|Maximum||
|cacheWrite2|Cacheschreibvorgänge (Shard 2)|Bytes pro Sekunde|Maximum||
|cacheRead2|Cachelesevorgänge (Shard 2)|Bytes pro Sekunde|Maximum||
|percentProcessorTime2|CPU (Shard 2)|Prozent|Maximum||
|connectedclients3|Verbundene Clients (Shard 3)|Count|Maximum||
|totalcommandsprocessed3|Vorgänge gesamt (Shard 3)|Count|Gesamt||
|cachehits3|Cachetreffer (Shard 3)|Count|Gesamt||
|cachemisses3|Cachefehler (Shard 3)|Count|Gesamt||
|getcommands3|Get-Vorgänge (Shard 3)|Count|Gesamt||
|setcommands3|Set-Vorgänge (Shard 3)|Count|Gesamt||
|evictedkeys3|Entfernte Schlüssel (Shard 3)|Count|Gesamt||
|totalkeys3|Schlüssel gesamt (Shard 3)|Count|Maximum||
|expiredkeys3|Abgelaufene Schlüssel (Shard 3)|Count|Gesamt||
|usedmemory3|Verwendeter Arbeitsspeicher (Shard 3)|Byte|Maximum||
|usedmemoryRss3|Verwendeter Arbeitsspeicher (RSS, Shard 3)|Byte|Maximum||
|serverLoad3|Serverauslastung (Shard 3)|Prozent|Maximum||
|cacheWrite3|Cacheschreibvorgänge (Shard 3)|Bytes pro Sekunde|Maximum||
|cacheRead3|Cachelesevorgänge (Shard 3)|Bytes pro Sekunde|Maximum||
|percentProcessorTime3|CPU (Shard 3)|Prozent|Maximum||
|connectedclients4|Verbundene Clients (Shard 4)|Count|Maximum||
|totalcommandsprocessed4|Vorgänge gesamt (Shard 4)|Count|Gesamt||
|cachehits4|Cachetreffer (Shard 4)|Count|Gesamt||
|cachemisses4|Cachefehler (Shard 4)|Count|Gesamt||
|getcommands4|Get-Vorgänge (Shard 4)|Count|Gesamt||
|setcommands4|Set-Vorgänge (Shard 4)|Count|Gesamt||
|evictedkeys4|Entfernte Schlüssel (Shard 4)|Count|Gesamt||
|totalkeys4|Schlüssel gesamt (Shard 4)|Count|Maximum||
|expiredkeys4|Abgelaufene Schlüssel (Shard 4)|Count|Gesamt||
|usedmemory4|Verwendeter Arbeitsspeicher (Shard 4)|Byte|Maximum||
|usedmemoryRss4|Verwendeter Arbeitsspeicher (RSS, Shard 4)|Byte|Maximum||
|serverLoad4|Serverauslastung (Shard 4)|Prozent|Maximum||
|cacheWrite4|Cacheschreibvorgänge (Shard 4)|Bytes pro Sekunde|Maximum||
|cacheRead4|Cachelesevorgänge (Shard 4)|Bytes pro Sekunde|Maximum||
|percentProcessorTime4|CPU (Shard 4)|Prozent|Maximum||
|connectedclients5|Verbundene Clients (Shard 5)|Count|Maximum||
|totalcommandsprocessed5|Vorgänge gesamt (Shard 5)|Count|Gesamt||
|cachehits5|Cachetreffer (Shard 5)|Count|Gesamt||
|cachemisses5|Cachefehler (Shard 5)|Count|Gesamt||
|getcommands5|Get-Vorgänge (Shard 5)|Count|Gesamt||
|setcommands5|Set-Vorgänge (Shard 5)|Count|Gesamt||
|evictedkeys5|Entfernte Schlüssel (Shard 5)|Count|Gesamt||
|totalkeys5|Schlüssel gesamt (Shard 5)|Count|Maximum||
|expiredkeys5|Abgelaufene Schlüssel (Shard 5)|Count|Gesamt||
|usedmemory5|Verwendeter Arbeitsspeicher (Shard 5)|Byte|Maximum||
|usedmemoryRss5|Verwendeter Arbeitsspeicher (RSS, Shard 5)|Byte|Maximum||
|serverLoad5|Serverauslastung (Shard 5)|Prozent|Maximum||
|cacheWrite5|Cacheschreibvorgänge (Shard 5)|Bytes pro Sekunde|Maximum||
|cacheRead5|Cachelesevorgänge (Shard 5)|Bytes pro Sekunde|Maximum||
|percentProcessorTime5|CPU (Shard 5)|Prozent|Maximum||
|connectedclients6|Verbundene Clients (Shard 6)|Count|Maximum||
|totalcommandsprocessed6|Vorgänge gesamt (Shard 6)|Count|Gesamt||
|cachehits6|Cachetreffer (Shard 6)|Count|Gesamt||
|cachemisses6|Cachefehler (Shard 6)|Count|Gesamt||
|getcommands6|Get-Vorgänge (Shard 6)|Count|Gesamt||
|setcommands6|Set-Vorgänge (Shard 6)|Count|Gesamt||
|evictedkeys6|Entfernte Schlüssel (Shard 6)|Count|Gesamt||
|totalkeys6|Schlüssel gesamt (Shard 6)|Count|Maximum||
|expiredkeys6|Abgelaufene Schlüssel (Shard 6)|Count|Gesamt||
|usedmemory6|Verwendeter Arbeitsspeicher (Shard 6)|Byte|Maximum||
|usedmemoryRss6|Verwendeter Arbeitsspeicher (RSS, Shard 6)|Byte|Maximum||
|serverLoad6|Serverauslastung (Shard 6)|Prozent|Maximum||
|cacheWrite6|Cacheschreibvorgänge (Shard 6)|Bytes pro Sekunde|Maximum||
|cacheRead6|Cachelesevorgänge (Shard 6)|Bytes pro Sekunde|Maximum||
|percentProcessorTime6|CPU (Shard 6)|Prozent|Maximum||
|connectedclients7|Verbundene Clients (Shard 7)|Count|Maximum||
|totalcommandsprocessed7|Vorgänge gesamt (Shard 7)|Count|Gesamt||
|cachehits7|Cachetreffer (Shard 7)|Count|Gesamt||
|cachemisses7|Cachefehler (Shard 7)|Count|Gesamt||
|getcommands7|Get-Vorgänge (Shard 7)|Count|Gesamt||
|setcommands7|Set-Vorgänge (Shard 7)|Count|Gesamt||
|evictedkeys7|Entfernte Schlüssel (Shard 7)|Count|Gesamt||
|totalkeys7|Schlüssel gesamt (Shard 7)|Count|Maximum||
|expiredkeys7|Abgelaufene Schlüssel (Shard 7)|Count|Gesamt||
|usedmemory7|Verwendeter Arbeitsspeicher (Shard 7)|Byte|Maximum||
|usedmemoryRss7|Verwendeter Arbeitsspeicher (RSS, Shard 7)|Byte|Maximum||
|serverLoad7|Serverauslastung (Shard 7)|Prozent|Maximum||
|cacheWrite7|Cacheschreibvorgänge (Shard 7)|Bytes pro Sekunde|Maximum||
|cacheRead7|Cachelesevorgänge (Shard 7)|Bytes pro Sekunde|Maximum||
|percentProcessorTime7|CPU (Shard 7)|Prozent|Maximum||
|connectedclients8|Verbundene Clients (Shard 8)|Count|Maximum||
|totalcommandsprocessed8|Vorgänge gesamt (Shard 8)|Count|Gesamt||
|cachehits8|Cachetreffer (Shard 8)|Count|Gesamt||
|cachemisses8|Cachefehler (Shard 8)|Count|Gesamt||
|getcommands8|Get-Vorgänge (Shard 8)|Count|Gesamt||
|setcommands8|Set-Vorgänge (Shard 8)|Count|Gesamt||
|evictedkeys8|Entfernte Schlüssel (Shard 8)|Count|Gesamt||
|totalkeys8|Schlüssel gesamt (Shard 8)|Count|Maximum||
|expiredkeys8|Abgelaufene Schlüssel (Shard 8)|Count|Gesamt||
|usedmemory8|Verwendeter Arbeitsspeicher (Shard 8)|Byte|Maximum||
|usedmemoryRss8|Verwendeter Arbeitsspeicher (RSS, Shard 8)|Byte|Maximum||
|serverLoad8|Serverauslastung (Shard 8)|Prozent|Maximum||
|cacheWrite8|Cacheschreibvorgänge (Shard 8)|Bytes pro Sekunde|Maximum||
|cacheRead8|Cachelesevorgänge (Shard 8)|Bytes pro Sekunde|Maximum||
|percentProcessorTime8|CPU (Shard 8)|Prozent|Maximum||
|connectedclients9|Verbundene Clients (Shard 9)|Count|Maximum||
|totalcommandsprocessed9|Vorgänge gesamt (Shard 9)|Count|Gesamt||
|cachehits9|Cachetreffer (Shard 9)|Count|Gesamt||
|cachemisses9|Cachefehler (Shard 9)|Count|Gesamt||
|getcommands9|Get-Vorgänge (Shard 9)|Count|Gesamt||
|setcommands9|Set-Vorgänge (Shard 9)|Count|Gesamt||
|evictedkeys9|Entfernte Schlüssel (Shard 9)|Count|Gesamt||
|totalkeys9|Schlüssel gesamt (Shard 9)|Count|Maximum||
|expiredkeys9|Abgelaufene Schlüssel (Shard 9)|Count|Gesamt||
|usedmemory9|Verwendeter Arbeitsspeicher (Shard 9)|Byte|Maximum||
|usedmemoryRss9|Verwendeter Arbeitsspeicher (RSS, Shard 9)|Byte|Maximum||
|serverLoad9|Serverauslastung (Shard 9)|Prozent|Maximum||
|cacheWrite9|Cacheschreibvorgänge (Shard 9)|Bytes pro Sekunde|Maximum||
|cacheRead9|Cachelesevorgänge (Shard 9)|Bytes pro Sekunde|Maximum||
|percentProcessorTime9|CPU (Shard 9)|Prozent|Maximum||

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|TotalCalls|Aufrufe gesamt|Count|Gesamt|Gesamtanzahl von Aufrufen|
|SuccessfulCalls|Erfolgreiche Aufrufe|Count|Gesamt|Anzahl erfolgreicher Aufrufe|
|TotalErrors|Fehler insgesamt|Count|Gesamt|Gesamtzahl von Aufrufen mit Fehlerantwort (HTTP-Antwortcode 4xx oder 5xx)|
|BlockedCalls|Blockierte Aufrufe|Count|Gesamt|Anzahl von Aufrufen, die das Raten- oder Kontingentlimit überschritten haben|
|ServerErrors|Serverfehler|Count|Gesamt|Anzahl von Aufrufen mit internem Dienstfehler (HTTP-Antwortcode 5xx)|
|ClientErrors|Clientfehler|Count|Gesamt|Anzahl von Aufrufen mit Fehler auf Clientseite (HTTP-Antwortcode 4xx)|
|DataIn|Eingehende Daten|Byte|Gesamt|Menge eingehender Daten in Byte|
|DataOut|Datenausgabe|Byte|Gesamt|Menge ausgehender Daten in Byte|
|Latenz|Latenz|Millisekunden|Durchschnitt|Latenz in Millisekunden|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|Prozentsatz CPU|Prozentsatz CPU|Prozent|Durchschnitt|Der Prozentsatz der zugewiesenen Computeeinheiten, die derzeit von den virtuellen Computern verwendet werden|
|Eingehender Netzwerkdatenverkehr|Eingehender Netzwerkdatenverkehr|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen empfangen werden (eingehender Datenverkehr)|
|Ausgehender Netzwerkdatenverkehr|Ausgehender Netzwerkdatenverkehr|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen gesendet werden (ausgehender Datenverkehr)|
|Datenträgerlesevorgänge in Bytes|Datenträgerlesevorgänge in Bytes|Byte|Gesamt|Gesamtzahl von Bytes, die während des Überwachungszeitraums vom Datenträger gelesen werden|
|Datenträgerschreibvorgänge in Bytes|Datenträgerschreibvorgänge in Bytes|Byte|Gesamt|Gesamtzahl von Bytes, die während des Überwachungszeitraums auf den Datenträger geschrieben werden|
|Datenträgerlesevorgänge/Sek.|Datenträgerlesevorgänge/Sek.|Anzahl pro Sekunde|Durchschnitt|Datenträgerlesevorgänge in IOPS|
|Datenträgerschreibvorgänge/Sek.|Datenträgerschreibvorgänge/Sek.|Anzahl pro Sekunde|Durchschnitt|Datenträgerschreibvorgänge in IOPS|

## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|Prozentsatz CPU|Prozentsatz CPU|Prozent|Durchschnitt|Der Prozentsatz der zugewiesenen Computeeinheiten, die derzeit von den virtuellen Computern verwendet werden|
|Eingehender Netzwerkdatenverkehr|Eingehender Netzwerkdatenverkehr|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen empfangen werden (eingehender Datenverkehr)|
|Ausgehender Netzwerkdatenverkehr|Ausgehender Netzwerkdatenverkehr|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen gesendet werden (ausgehender Datenverkehr)|
|Datenträgerlesevorgänge in Bytes|Datenträgerlesevorgänge in Bytes|Byte|Gesamt|Gesamtzahl von Bytes, die während des Überwachungszeitraums vom Datenträger gelesen werden|
|Datenträgerschreibvorgänge in Bytes|Datenträgerschreibvorgänge in Bytes|Byte|Gesamt|Gesamtzahl von Bytes, die während des Überwachungszeitraums auf den Datenträger geschrieben werden|
|Datenträgerlesevorgänge/Sek.|Datenträgerlesevorgänge/Sek.|Anzahl pro Sekunde|Durchschnitt|Datenträgerlesevorgänge in IOPS|
|Datenträgerschreibvorgänge/Sek.|Datenträgerschreibvorgänge/Sek.|Anzahl pro Sekunde|Durchschnitt|Datenträgerschreibvorgänge in IOPS|

## <a name="microsoftcomputevirtualmachinescalesetsvirtualmachines"></a>Microsoft.Compute/virtualMachineScaleSets/virtualMachines

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|Prozentsatz CPU|Prozentsatz CPU|Prozent|Durchschnitt|Der Prozentsatz der zugewiesenen Computeeinheiten, die derzeit von den virtuellen Computern verwendet werden|
|Eingehender Netzwerkdatenverkehr|Eingehender Netzwerkdatenverkehr|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen empfangen werden (eingehender Datenverkehr)|
|Ausgehender Netzwerkdatenverkehr|Ausgehender Netzwerkdatenverkehr|Byte|Gesamt|Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen gesendet werden (ausgehender Datenverkehr)|
|Datenträgerlesevorgänge in Bytes|Datenträgerlesevorgänge in Bytes|Byte|Gesamt|Gesamtzahl von Bytes, die während des Überwachungszeitraums vom Datenträger gelesen werden|
|Datenträgerschreibvorgänge in Bytes|Datenträgerschreibvorgänge in Bytes|Byte|Gesamt|Gesamtzahl von Bytes, die während des Überwachungszeitraums auf den Datenträger geschrieben werden|
|Datenträgerlesevorgänge/Sek.|Datenträgerlesevorgänge/Sek.|Anzahl pro Sekunde|Durchschnitt|Datenträgerlesevorgänge in IOPS|
|Datenträgerschreibvorgänge/Sek.|Datenträgerschreibvorgänge/Sek.|Anzahl pro Sekunde|Durchschnitt|Datenträgerschreibvorgänge in IOPS|

## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/IotHubs
| Metrik | Metrikanzeigename | Einheit | Aggregationstyp | Beschreibung |
| --- | --- | --- | --- | --- |
| d2c.telemetry.Ingress.allProtocol |Versuche zum Senden von Telemetrienachrichten |Count |Gesamt |Anzahl von Telemetrienachrichten vom Gerät an die Cloud, die an Ihren IoT Hub gesendet werden sollten |
| d2c.telemetry.ingress.success |Gesendete Telemetrienachrichten |Count |Gesamt |Anzahl von Telemetrienachrichten vom Gerät an die Cloud, die erfolgreich an Ihren IoT Hub gesendet wurden |
| d2c.telemetry.egress.success | Telemetry messages delivered (Übermittelte Telemetrienachrichten) | Count | Gesamt | Anzahl aller erfolgreichen Schreibvorgänge an einen Endpunkt |
| d2c.telemetry.egress.invalid | Invalid telemetry message delivery attempts (Ungültige Versuche zur Übermittlung von Telemetrienachrichten) | Count | Gesamt | Anzahl von Nachrichten, die aufgrund von Inkompatibilität mit dem Endpunkt nicht übermittelt wurden |
| d2c.telemetry.egress.dropped | Dropped telemetry messages (Verworfene Telemetrienachrichten) | Count | Gesamt | Anzahl von Nachrichten, die verworfen wurden, weil ein Endpunkt fehlerhaft war |
| d2c.telemetry.egress.fallback | Telemetry messages matching fallback condition (Telemetrienachrichten, die die Fallbackbedingung erfüllen) | Count | Gesamt | Anzahl von Nachrichten, die der Fallbackroute entsprechen |
| d2c.telemetry.egress.orphaned | Orphaned telemetry messages (Verwaiste Telemetrienachrichten) | Count | Gesamt | Anzahl von Nachrichten, die keiner Route entsprechen (einschließlich der Fallbackroute) |
| d2c.endpoints.latency.eventHubs | Message latency for Event Hub endpoints (Nachrichtenwartezeit für Event Hub-Endpunkte) | Millisekunden | Durchschnitt | Durchschnittliche, minimale und maximale Wartezeit zwischen dem Eingang der Nachricht an den IoT Hub und dem Eingang der Nachricht in einem Event Hub-Endpunkt in Millisekunden |
| d2c.endpoints.latency.serviceBusQueues | Message latency for Service Bus Queue endpoints (Nachrichtenwartezeit für Service Bus-Warteschlangenendpunkte) | Millisekunden | Durchschnitt | Durchschnittliche, minimale und maximale Wartezeit zwischen dem Eingang der Nachricht an den IoT Hub und dem Eingang der Nachricht in einem Service Bus-Warteschlangenendpunkt in Millisekunden |
| d2c.endpoints.latency.serviceBusTopics | Message latency for Service Bus Topic endpoints (Nachrichtenwartezeit für Service Bus-Themenendpunkte) | Millisekunden | Durchschnitt | Durchschnittliche, minimale und maximale Wartezeit zwischen dem Eingang der Nachricht an den IoT Hub und dem Eingang der Nachricht in einem Service Bus-Themenendpunkt in Millisekunden |
| c2d.commands.egress.complete.success |Abgeschlossene Befehle |Count |Gesamt |Anzahl von Befehlen von der Cloud an das Gerät, die vom Gerät erfolgreich abgeschlossen wurden |
| c2d.commands.egress.abandon.success |Abgebrochene Befehle |Count |Gesamt |Anzahl von Befehlen von der Cloud an das Gerät, die vom Gerät abgebrochen wurden |
| c2d.commands.egress.reject.success |Abgelehnte Befehle |Count |Gesamt |Anzahl von Befehlen von der Cloud an das Gerät, die vom Gerät abgelehnt wurden |
| devices.totalDevices |Gesamtanzahl von Geräten |Count |Gesamt |Die Anzahl von Geräten, die beim IoT Hub registriert sind |
| devices.connectedDevices.allProtocol |Verbundene Geräte |Count |Gesamt |Die Anzahl von Geräten, die mit dem IoT Hub verbunden sind |

## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|INREQS|Eingehende Anforderungen|Count|Gesamt|Event Hub-Durchsatz eingehender Nachrichten für einen Namespace|
|SUCCREQ|Erfolgreiche Anforderungen|Count|Gesamt|Gesamtzahl der erfolgreichen Anforderungen für einen Namespace|
|FAILREQ|Anforderungsfehler|Count|Gesamt|Gesamtzahl der fehlgeschlagenen Anforderungen für einen Namespace|
|SVRBSY|Fehler durch ausgelasteten Server|Count|Gesamt|Gesamtzahl der Fehler durch ausgelasteten Server für einen Namespace|
|INTERR|Interne Serverfehler|Count|Gesamt|Gesamtzahl der internen Serverfehler für einen Namespace|
|MISCERR|Andere Fehler|Count|Gesamt|Gesamtzahl der fehlgeschlagenen Anforderungen für einen Namespace|
|INMSGS|Eingehende Nachrichten|Count|Gesamt|Gesamtzahl der eingehenden Nachrichten für einen Namespace|
|OUTMSGS|Ausgehende Nachrichten|Count|Gesamt|Gesamtzahl der ausgehenden Nachrichten für einen Namespace|
|EHINMBS|Eingehende Bytes pro Sekunde|Bytes pro Sekunde|Gesamt|Event Hub-Durchsatz eingehender Nachrichten für einen Namespace|
|EHOUTMBS|Ausgehende Bytes pro Sekunde|Bytes pro Sekunde|Gesamt|Gesamtzahl der ausgehenden Nachrichten für einen Namespace|
|EHABL|Archivierte Backlognachrichten|Count|Gesamt|Event Hub-Archivnachrichten im Backlog für einen Namespace|
|EHAMSGS|Archivnachrichten|Count|Gesamt|Event Hub-Archivnachrichten in einem Namespace|
|EHAMBS|Durchsatz von Archivnachrichten|Bytes pro Sekunde|Gesamt|Event Hub-Durchsatz archivierter Nachrichten in einem Namespace|

## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|RunsStarted|Gestartete Ausführungen|Count|Gesamt|Anzahl gestarteter Workflowausführungen.|
|RunsCompleted|Abgeschlossene Ausführungen|Count|Gesamt|Anzahl abgeschlossener Workflowausführungen.|
|RunsSucceeded|Erfolgreiche Ausführungen|Count|Gesamt|Anzahl erfolgreicher Workflowausführungen.|
|RunsFailed|Ausführungsfehler|Count|Gesamt|Anzahl fehlerhafter Workflowausführungen.|
|RunsCancelled|Abgebrochene Ausführungen|Count|Gesamt|Anzahl abgebrochener Workflowausführungen.|
|RunLatency|Ausführungslatenz|Sekunden|Durchschnitt|Latenz abgeschlossener Workflowausführungen.|
|RunSuccessLatency|Latenz erfolgreicher Ausführungen|Sekunden|Durchschnitt|Latenz erfolgreicher Workflowausführungen.|
|RunThrottledEvents|Ereignisse zu gedrosselten Ausführungen|Count|Gesamt|Anzahl von Ereignissen zu gedrosselten Workflowaktionen oder Triggern.|
|RunFailurePercentage|Prozentsatz der Ausführungsfehler|Prozent|Gesamt|Prozentsatz fehlerhafter Workflowausführungen|
|ActionsStarted|Gestartete Aktionen |Count|Gesamt|Anzahl gestarteter Workflowaktionen.|
|ActionsCompleted|Abgeschlossene Aktionen |Count|Gesamt|Anzahl abgeschlossener Workflowaktionen.|
|ActionsSucceeded|Erfolgreiche Aktionen |Count|Gesamt|Anzahl erfolgreicher Workflowaktionen.|
|ActionsFailed|Aktionsfehler|Count|Gesamt|Anzahl fehlerhafter Workflowaktionen.|
|ActionsSkipped|Übersprungene Aktionen |Count|Gesamt|Anzahl übersprungener Workflowaktionen.|
|ActionLatency|Aktionslatenz |Sekunden|Durchschnitt|Latenz abgeschlossener Workflowaktionen.|
|ActionSuccessLatency|Latenz erfolgreicher Aktionen |Sekunden|Durchschnitt|Latenz erfolgreicher Workflowaktionen.|
|ActionThrottledEvents|Ereignisse zu gedrosselten Aktionen|Count|Gesamt|Anzahl von Ereignissen zu gedrosselten Workflowaktionen.|
|TriggersStarted|Gestartete Trigger |Count|Gesamt|Anzahl gestarteter Workflowtrigger.|
|TriggersCompleted|Abgeschlossene Trigger |Count|Gesamt|Anzahl abgeschlossener Workflowtrigger.|
|TriggersSucceeded|Erfolgreiche Trigger |Count|Gesamt|Anzahl erfolgreicher Workflowtrigger.|
|TriggersFailed|Triggerfehler |Count|Gesamt|Anzahl fehlerhafter Workflowtrigger.|
|TriggersSkipped|Übersprungene Trigger|Count|Gesamt|Anzahl übersprungener Workflowtrigger.|
|TriggersFired|Ausgelöste Trigger |Count|Gesamt|Anzahl ausgelöster Workflowtrigger.|
|TriggerLatency|Triggerlatenz |Sekunden|Durchschnitt|Latenz abgeschlossener Workflowtrigger.|
|TriggerFireLatency|Latenz beim Auslösen von Triggern |Sekunden|Durchschnitt|Latenz ausgelöster Workflowtrigger.|
|TriggerSuccessLatency|Latenz erfolgreicher Trigger |Sekunden|Durchschnitt|Latenz erfolgreicher Workflowtrigger.|
|TriggerThrottledEvents|Ereignisse zu gedrosselten Triggern|Count|Gesamt|Anzahl von Ereignissen zu gedrosselten Workflowtriggern.|
|BillableActionExecutions|Ausführungen abrechenbarer Aktionen|Count|Gesamt|Anzahl der Workflowaktionsausführungen, die in Rechnung gestellt werden|
|BillableTriggerExecutions|Ausführungen abrechenbarer Trigger|Count|Gesamt|Anzahl der Workflowtriggerausführungen, die in Rechnung gestellt werden|
|TotalBillableExecutions|Gesamtzahl der abrechenbaren Ausführungen|Count|Gesamt|Anzahl der Workflowausführungen, die in Rechnung gestellt werden|

## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|Durchsatz|Durchsatz|Bytes pro Sekunde|Durchschnitt||

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
|connection_successful|Erfolgreiche Verbindungen|Count|Gesamt|Erfolgreiche Verbindungen|
|connection_failed|Verbindungsfehler|Count|Gesamt|Verbindungsfehler|
|blocked_by_firewall|Von der Firewall blockiert|Count|Gesamt|Von der Firewall blockiert|
|deadlock|Deadlocks|Count|Gesamt|Deadlocks|
|storage_percent|Datenbankgröße als Prozentsatz|Prozent|Maximum|Datenbankgröße als Prozentsatz|
|xtp_storage_percent|In-Memory-OLTP-Speicher in Prozent (Vorschau)|Prozent|Durchschnitt|In-Memory-OLTP-Speicher in Prozent (Vorschau)|
|workers_percent|Worker in Prozent|Prozent|Durchschnitt|Worker in Prozent|
|sessions_percent|Sitzungen in Prozent|Prozent|Durchschnitt|Sitzungen in Prozent|
|dtu_limit|DTU-Grenzwert|Count|Durchschnitt|DTU-Grenzwert|
|dtu_used|DTU-Verbrauch|Count|Durchschnitt|DTU-Verbrauch|
|service_level_objective|SLO der Datenbank|Count|Gesamt|SLO der Datenbank|
|dwu_limit|DWU-Grenzwert|Count|Maximum|DWU-Grenzwert|
|dwu_consumption_percent|DWU in Prozent|Prozent|Durchschnitt|DWU in Prozent|
|dwu_used|DWU-Verbrauch|Count|Durchschnitt|DWU-Verbrauch|

## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|cpu_percent|CPU-Prozentsatz|Prozent|Durchschnitt|CPU-Prozentsatz|
|physical_data_read_percent|E/A-Prozentsatz für Daten|Prozent|Durchschnitt|E/A-Prozentsatz für Daten|
|log_write_percent|E/A-Prozentsatz für Protokoll|Prozent|Durchschnitt|E/A-Prozentsatz für Protokoll|
|dtu_consumption_percent|DTU-Prozentsatz|Prozent|Durchschnitt|DTU-Prozentsatz|
|storage_percent|Speicher in Prozent|Prozent|Durchschnitt|Speicher in Prozent|
|workers_percent|Worker in Prozent|Prozent|Durchschnitt|Worker in Prozent|
|sessions_percent|Sitzungen in Prozent|Prozent|Durchschnitt|Sitzungen in Prozent|
|eDTU_limit|eDTU-Grenzwert|Count|Durchschnitt|eDTU-Grenzwert|
|storage_limit|Speicherbegrenzung|Byte|Durchschnitt|Speicherbegrenzung|
|eDTU_used|eDTU-Verbrauch|Count|Durchschnitt|eDTU-Verbrauch|
|storage_used|Verwendeter Speicher|Byte|Durchschnitt|Verwendeter Speicher|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|ResourceUtilization|Speichereinheitnutzung in %|Prozent|Maximum|Speichereinheitnutzung in %|
|InputEvents|Eingabeereignisse|Count|Gesamt|Eingabeereignisse|
|InputEventBytes|Eingabeereignisbytes|Byte|Gesamt|Eingabeereignisbytes|
|LateInputEvents|Ereignisse bei verspäteter Eingabe|Count|Gesamt|Ereignisse bei verspäteter Eingabe|
|OutputEvents|Ausgabeereignisse|Count|Gesamt|Ausgabeereignisse|
|ConversionErrors|Konvertierungsfehler|Count|Gesamt|Konvertierungsfehler|
|Fehler|Laufzeitfehler|Count|Gesamt|Laufzeitfehler|
|DroppedOrAdjustedEvents|Ereignisse für falsche Reihenfolge|Count|Gesamt|Ereignisse für falsche Reihenfolge|
|AMLCalloutRequests|Funktionsanforderungen|Count|Gesamt|Funktionsanforderungen|
|AMLCalloutFailedRequests|Fehlerhafte Funktionsanforderungen|Count|Gesamt|Fehlerhafte Funktionsanforderungen|
|AMLCalloutInputEvents|Funktionsereignisse|Count|Gesamt|Funktionsereignisse|

## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|CpuPercentage|CPU-Prozentsatz|Prozent|Durchschnitt|CPU-Prozentsatz|
|MemoryPercentage|Arbeitsspeicherprozentsatz|Prozent|Durchschnitt|Arbeitsspeicherprozentsatz|
|DiskQueueLength|Warteschlangenlänge des Datenträgers|Count|Gesamt|Warteschlangenlänge des Datenträgers|
|HttpQueueLength|Länge der HTTP-Warteschlange|Count|Gesamt|Länge der HTTP-Warteschlange|
|BytesReceived|Eingehende Daten|Byte|Gesamt|Eingehende Daten|
|BytesSent|Datenausgabe|Byte|Gesamt|Datenausgabe|

## <a name="microsoftwebsites-including-azure-functions"></a>Microsoft.Web/sites (einschließlich Azure Functions)

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|CpuTime|CPU-Zeit|Sekunden|Gesamt|CPU-Zeit|
|Anforderungen|Anforderungen|Count|Gesamt|Anforderungen|
|BytesReceived|Eingehende Daten|Byte|Gesamt|Eingehende Daten|
|BytesSent|Datenausgabe|Byte|Gesamt|Datenausgabe|
|Http101|HTTP 101|Count|Gesamt|HTTP 101|
|Http2xx|HTTP 2xx|Count|Gesamt|HTTP 2xx|
|Http3xx|HTTP 3xx|Count|Gesamt|HTTP 3xx|
|Http401|HTTP 401|Count|Gesamt|HTTP 401|
|Http403|HTTP 403|Count|Gesamt|HTTP 403|
|Http404|HTTP 404|Count|Gesamt|HTTP 404|
|Http406|HTTP 406|Count|Gesamt|HTTP 406|
|Http4xx|HTTP 4xx|Count|Gesamt|HTTP 4xx|
|Http5xx|HTTP-Serverfehler|Count|Gesamt|HTTP-Serverfehler|
|MemoryWorkingSet|Arbeitssatz für Arbeitsspeicher|Byte|Gesamt|Arbeitssatz für Arbeitsspeicher|
|AverageMemoryWorkingSet|Durchschnittlicher Arbeitssatz für Arbeitsspeicher|Byte|Gesamt|Durchschnittlicher Arbeitssatz für Arbeitsspeicher|
|AverageResponseTime|Durchschnittliche Antwortzeit|Sekunden|Durchschnitt|Durchschnittliche Antwortzeit|
|FunctionExecutionUnits|Ausführungseinheiten für Funktion|Count|Durchschnitt|Ausführungseinheiten für Funktion|
|FunctionExecutionCount|Ausführungsanzahl für Funktion|Count|Durchschnitt|Ausführungsanzahl für Funktion|

## <a name="microsoftwebsitesslots"></a>Microsoft.Web/sites/slots

|Metrik|Metrikanzeigename|Einheit|Aggregationstyp|Beschreibung|
|---|---|---|---|---|
|CpuTime|CPU-Zeit|Sekunden|Gesamt|CPU-Zeit|
|Anforderungen|Anforderungen|Count|Gesamt|Anforderungen|
|BytesReceived|Eingehende Daten|Byte|Gesamt|Eingehende Daten|
|BytesSent|Datenausgabe|Byte|Gesamt|Datenausgabe|
|Http101|HTTP 101|Count|Gesamt|HTTP 101|
|Http2xx|HTTP 2xx|Count|Gesamt|HTTP 2xx|
|Http3xx|HTTP 3xx|Count|Gesamt|HTTP 3xx|
|Http401|HTTP 401|Count|Gesamt|HTTP 401|
|Http403|HTTP 403|Count|Gesamt|HTTP 403|
|Http404|HTTP 404|Count|Gesamt|HTTP 404|
|Http406|HTTP 406|Count|Gesamt|HTTP 406|
|Http4xx|HTTP 4xx|Count|Gesamt|HTTP 4xx|
|Http5xx|HTTP-Serverfehler|Count|Gesamt|HTTP-Serverfehler|
|MemoryWorkingSet|Arbeitssatz für Arbeitsspeicher|Byte|Gesamt|Arbeitssatz für Arbeitsspeicher|
|AverageMemoryWorkingSet|Durchschnittlicher Arbeitssatz für Arbeitsspeicher|Byte|Gesamt|Durchschnittlicher Arbeitssatz für Arbeitsspeicher|
|AverageResponseTime|Durchschnittliche Antwortzeit|Sekunden|Durchschnitt|Durchschnittliche Antwortzeit|
|FunctionExecutionUnits|Ausführungseinheiten für Funktion|Count|Durchschnitt|Ausführungseinheiten für Funktion|
|FunctionExecutionCount|Ausführungsanzahl für Funktion|Count|Durchschnitt|Ausführungsanzahl für Funktion|

## <a name="next-steps"></a>Nächste Schritte
* [Informationen zu Metriken in Azure Monitor](monitoring-overview.md#monitoring-sources)
* [Erstellen von Warnungen zu Metriken](insights-receive-alert-notifications.md)
* [Exportieren von Metriken in Storage, Event Hub oder Log Analytics](monitoring-overview-of-diagnostic-logs.md)




<!--HONumber=Dec16_HO2-->


