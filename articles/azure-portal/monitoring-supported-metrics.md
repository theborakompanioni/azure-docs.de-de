---
title: 'Azure Monitor-Metriken: unterstützte Metriken pro Ressourcentyp | Microsoft Docs'
description: Liste der Metriken, die mit Azure Monitor für jeden Ressourcentyp zur Verfügung stehen.
author: johnkemnetz
manager: rboucher
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics

ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2016
ms.author: johnkem

---
# Unterstützte Metriken von Azure Monitor
Azure Monitor bietet verschiedene Methoden für die Interaktion mit Metriken, z.B. die Diagrammdarstellung im Portal, den Zugriff über die REST-API oder die Abfrage über PowerShell oder CLI. Unten ist eine vollständige Liste aller Metriken aufgeführt, die derzeit mit der Metrikpipeline von Azure Monitor verfügbar sind.

> [!NOTE]
> Weitere Metriken stehen möglicherweise im Portal oder über Legacy-APIs zur Verfügung. Diese Liste enthält nur Metriken der öffentlichen Vorschau, die über die öffentliche Vorschau der konsolidierten Azure Monitor-Metrikpipeline verfügbar sind.
> 
> 

## Microsoft.Batch/batchAccounts
| Metrik | Metrikanzeigename | Einheit | Aggregationstyp | Beschreibung |
| --- | --- | --- | --- | --- |
| CoreCount |Anzahl von Kernen |Prozent |Durchschnitt |Durchschnittliche Anzahl von Kernen im Batch-Konto |
| TotalNodeCount |Anzahl von Knoten |Prozent |Durchschnitt |Durchschnittliche Anzahl von Knoten im Batch-Konto |
| CreatingNodeCount |Anzahl erstellter Knoten |Prozent |Durchschnitt |Anzahl von Knoten, die gerade erstellt werden |
| StartingNodeCount |Anzahl gestarteter Knoten |Prozent |Durchschnitt |Anzahl von Knoten, die gerade gestartet werden |
| WaitingForStartTaskNodeCount |Anzahl von Knoten, die auf den Starttask warten |Prozent |Durchschnitt |Anzahl von Knoten, die auf den Abschluss des Starttasks warten |
| StartTaskFailedNodeCount |Anzahl von Knoten mit Starttaskfehlern |Prozent |Durchschnitt |Anzahl von Knoten, bei denen der Starttask nicht durchgeführt werden konnte |
| IdleNodeCount |Anzahl von Knoten im Leerlauf |Prozent |Durchschnitt |Anzahl von Knoten im Leerlauf |
| OfflineNodeCount |Anzahl von Offlineknoten |Prozent |Durchschnitt |Anzahl offline geschalteter Knoten |
| RebootingNodeCount |Anzahl neu gestarteter Knoten |Prozent |Durchschnitt |Anzahl von Knoten, die neu gestartet werden |
| ReimagingNodeCount |Anzahl von Knoten mit Reimaging |Prozent |Durchschnitt |Anzahl von Knoten, für die ein Reimaging durchgeführt wird |
| RunningNodeCount |Anzahl ausgeführter Knoten |Prozent |Durchschnitt |Anzahl ausgeführter Knoten |
| LeavingPoolNodeCount |Anzahl von Knoten, die den Pool verlassen |Prozent |Durchschnitt |Anzahl von Knoten, die den Pool verlassen |
| UnusableNodeCount |Anzahl nicht verwendbarer Knoten |Prozent |Durchschnitt |Anzahl nicht verwendbarer Knoten |
| TaskStartEvent |Taskstartereignisse |Count |Gesamt |Gesamtanzahl gestarteter Tasks |
| TaskCompleteEvent |Taskabschlussereignisse |Count |Gesamt |Gesamtanzahl abgeschlossener Tasks |
| TaskFailEvent |Taskfehlerereignisse |Count |Gesamt |Gesamtanzahl von Tasks, die mit Fehlerstatus abgeschlossen wurden |
| PoolCreateEvent |Poolerstellungsereignisse |Count |Gesamt |Gesamtanzahl erstellter Pools |
| PoolResizeStartEvent |Ereignisse zum Start der Größenänderung von Pools |Count |Gesamt |Gesamtanzahl gestarteter Größenänderungen von Pools |
| PoolResizeCompleteEvent |Ereignisse zum Abschluss der Größenänderung von Pools |Count |Gesamt |Gesamtanzahl abgeschlossener Größenänderungen von Pools |
| PoolDeleteStartEvent |Ereignisse zum Starten des Löschvorgangs von Pools |Count |Gesamt |Gesamtanzahl gestarteter Poollöschvorgänge |
| PoolDeleteCompleteEvent |Ereignisse zum Abschluss des Löschvorgangs von Pools |Count |Gesamt |Gesamtanzahl abgeschlossener Poollöschvorgänge |

## Microsoft.Cache/redis
| Metrik | Metrikanzeigename | Unit | Aggregationstyp | Beschreibung |
| --- | --- | --- | --- | --- |
| connectedclients |Verbundene Clients |Count |Maximum | |
| totalcommandsprocessed |Vorgänge gesamt |Count |Gesamt | |
| cachehits |Cachetreffer |Count |Gesamt | |
| cachemisses |Cachefehler |Count |Gesamt | |
| getcommands |get-Vorgänge |Count |Gesamt | |
| setcommands |set-Vorgänge |Count |Gesamt | |
| evictedkeys |Entfernte Schlüssel |Count |Gesamt | |
| totalkeys |Schlüssel insgesamt |Count |Maximum | |
| expiredkeys |Abgelaufene Schlüssel |Count |Gesamt | |
| usedmemory |Verwendeter Arbeitsspeicher |Byte |Maximum | |
| usedmemoryRss |Verwendeter Arbeitsspeicher (RSS) |Byte |Maximum | |
| serverLoad |Serverlast |Prozent |Maximum | |
| cacheWrite |Cache-Schreibvorgänge |Bytes pro Sekunde |Maximum | |
| cacheRead |Cache-Lesevorgänge |Bytes pro Sekunde |Maximum | |
| percentProcessorTime |CPU |Prozent |Maximum | |
| connectedclients0 |Verbundene Clients (Shard 0) |Count |Maximum | |
| totalcommandsprocessed0 |Vorgänge gesamt (Shard 0) |Count |Gesamt | |
| cachehits0 |Cachetreffer (Shard 0) |Count |Gesamt | |
| cachemisses0 |Cachefehler (Shard 0) |Count |Gesamt | |
| getcommands0 |Get-Vorgänge (Shard 0) |Count |Gesamt | |
| setcommands0 |Set-Vorgänge (Shard 0) |Count |Gesamt | |
| evictedkeys0 |Entfernte Schlüssel (Shard 0) |Count |Gesamt | |
| totalkeys0 |Schlüssel gesamt (Knoten 0) |Count |Maximum | |
| expiredkeys0 |Abgelaufene Schlüssel (Shard 0) |Count |Gesamt | |
| usedmemory0 |Verwendeter Arbeitsspeicher (Shard 0) |Byte |Maximum | |
| usedmemoryRss0 |Verwendeter Arbeitsspeicher (RSS, Shard 0) |Byte |Maximum | |
| serverLoad0 |Serverauslastung (Shard 0) |Prozent |Maximum | |
| cacheWrite0 |Cacheschreibvorgänge (Shard 0) |Bytes pro Sekunde |Maximum | |
| cacheRead0 |Cachelesevorgänge (Shard 0) |Bytes pro Sekunde |Maximum | |
| percentProcessorTime0 |CPU (Shard 0) |Prozent |Maximum | |
| connectedclients1 |Verbundene Clients (Shard 1) |Count |Maximum | |
| totalcommandsprocessed1 |Vorgänge gesamt (Shard 1) |Count |Gesamt | |
| cachehits1 |Cachetreffer (Shard 1) |Count |Gesamt | |
| cachemisses1 |Cachefehler (Shard 1) |Count |Gesamt | |
| getcommands1 |Get-Vorgänge (Shard 1) |Count |Gesamt | |
| setcommands1 |Set-Vorgänge (Shard 1) |Count |Gesamt | |
| evictedkeys1 |Entfernte Schlüssel (Shard 1) |Count |Gesamt | |
| totalkeys1 |Schlüssel gesamt (Knoten 1) |Count |Maximum | |
| expiredkeys1 |Abgelaufene Schlüssel (Shard 1) |Count |Gesamt | |
| usedmemory1 |Verwendeter Arbeitsspeicher (Shard 1) |Byte |Maximum | |
| usedmemoryRss1 |Verwendeter Arbeitsspeicher (RSS, Shard 1) |Byte |Maximum | |
| serverLoad1 |Serverauslastung (Shard 1) |Prozent |Maximum | |
| cacheWrite1 |Cacheschreibvorgänge (Shard 1) |Bytes pro Sekunde |Maximum | |
| cacheRead1 |Cachelesevorgänge (Shard 1) |Bytes pro Sekunde |Maximum | |
| percentProcessorTime1 |CPU (Shard 1) |Prozent |Maximum | |
| connectedclients2 |Verbundene Clients (Shard 2) |Count |Maximum | |
| totalcommandsprocessed2 |Vorgänge gesamt (Shard 2) |Count |Gesamt | |
| cachehits2 |Cachetreffer (Shard 2) |Count |Gesamt | |
| cachemisses2 |Cachefehler (Shard 2) |Count |Gesamt | |
| getcommands2 |Get-Vorgänge (Shard 2) |Count |Gesamt | |
| setcommands2 |Set-Vorgänge (Shard 2) |Count |Gesamt | |
| evictedkeys2 |Entfernte Schlüssel (Shard 2) |Count |Gesamt | |
| totalkeys2 |Schlüssel gesamt (Knoten 2) |Count |Maximum | |
| expiredkeys2 |Abgelaufene Schlüssel (Shard 2) |Count |Gesamt | |
| usedmemory2 |Verwendeter Arbeitsspeicher (Shard 2) |Byte |Maximum | |
| usedmemoryRss2 |Verwendeter Arbeitsspeicher (RSS, Shard 2) |Byte |Maximum | |
| serverLoad2 |Serverauslastung (Shard 2) |Prozent |Maximum | |
| cacheWrite2 |Cacheschreibvorgänge (Shard 2) |Bytes pro Sekunde |Maximum | |
| cacheRead2 |Cachelesevorgänge (Shard 2) |Bytes pro Sekunde |Maximum | |
| percentProcessorTime2 |CPU (Shard 2) |Prozent |Maximum | |
| connectedclients3 |Verbundene Clients (Shard 3) |Count |Maximum | |
| totalcommandsprocessed3 |Vorgänge gesamt (Shard 3) |Count |Gesamt | |
| cachehits3 |Cachetreffer (Shard 3) |Count |Gesamt | |
| cachemisses3 |Cachefehler (Shard 3) |Count |Gesamt | |
| getcommands3 |Get-Vorgänge (Shard 3) |Count |Gesamt | |
| setcommands3 |Set-Vorgänge (Shard 3) |Count |Gesamt | |
| evictedkeys3 |Entfernte Schlüssel (Shard 3) |Count |Gesamt | |
| totalkeys3 |Schlüssel gesamt (Knoten 3) |Count |Maximum | |
| expiredkeys3 |Abgelaufene Schlüssel (Shard 3) |Count |Gesamt | |
| usedmemory3 |Verwendeter Arbeitsspeicher (Shard 3) |Byte |Maximum | |
| usedmemoryRss3 |Verwendeter Arbeitsspeicher (RSS, Shard 3) |Byte |Maximum | |
| serverLoad3 |Serverauslastung (Shard 3) |Prozent |Maximum | |
| cacheWrite3 |Cacheschreibvorgänge (Shard 3) |Bytes pro Sekunde |Maximum | |
| cacheRead3 |Cachelesevorgänge (Shard 3) |Bytes pro Sekunde |Maximum | |
| percentProcessorTime3 |CPU (Shard 3) |Prozent |Maximum | |
| connectedclients4 |Verbundene Clients (Shard 4) |Count |Maximum | |
| totalcommandsprocessed4 |Vorgänge gesamt (Shard 4) |Count |Gesamt | |
| cachehits4 |Cachetreffer (Shard 4) |Count |Gesamt | |
| cachemisses4 |Cachefehler (Shard 4) |Count |Gesamt | |
| getcommands4 |Get-Vorgänge (Shard 4) |Count |Gesamt | |
| setcommands4 |Set-Vorgänge (Shard 4) |Count |Gesamt | |
| evictedkeys4 |Entfernte Schlüssel (Shard 4) |Count |Gesamt | |
| totalkeys4 |Schlüssel gesamt (Knoten 4) |Count |Maximum | |
| expiredkeys4 |Abgelaufene Schlüssel (Shard 4) |Count |Gesamt | |
| usedmemory4 |Verwendeter Arbeitsspeicher (Shard 4) |Byte |Maximum | |
| usedmemoryRss4 |Verwendeter Arbeitsspeicher (RSS, Shard 4) |Byte |Maximum | |
| serverLoad4 |Serverauslastung (Shard 4) |Prozent |Maximum | |
| cacheWrite4 |Cacheschreibvorgänge (Shard 4) |Bytes pro Sekunde |Maximum | |
| cacheRead4 |Cachelesevorgänge (Shard 4) |Bytes pro Sekunde |Maximum | |
| percentProcessorTime4 |CPU (Shard 4) |Prozent |Maximum | |
| connectedclients5 |Verbundene Clients (Shard 5) |Count |Maximum | |
| totalcommandsprocessed5 |Vorgänge gesamt (Shard 5) |Count |Gesamt | |
| cachehits5 |Cachetreffer (Shard 5) |Count |Gesamt | |
| cachemisses5 |Cachefehler (Shard 5) |Count |Gesamt | |
| getcommands5 |Get-Vorgänge (Shard 5) |Count |Gesamt | |
| setcommands5 |Set-Vorgänge (Shard 5) |Count |Gesamt | |
| evictedkeys5 |Entfernte Schlüssel (Shard 5) |Count |Gesamt | |
| totalkeys5 |Schlüssel gesamt (Knoten 5) |Count |Maximum | |
| expiredkeys5 |Abgelaufene Schlüssel (Shard 5) |Count |Gesamt | |
| usedmemory5 |Verwendeter Arbeitsspeicher (Shard 5) |Byte |Maximum | |
| usedmemoryRss5 |Verwendeter Arbeitsspeicher (RSS, Shard 5) |Byte |Maximum | |
| serverLoad5 |Serverauslastung (Shard 5) |Prozent |Maximum | |
| cacheWrite5 |Cacheschreibvorgänge (Shard 5) |Bytes pro Sekunde |Maximum | |
| cacheRead5 |Cachelesevorgänge (Shard 5) |Bytes pro Sekunde |Maximum | |
| percentProcessorTime5 |CPU (Shard 5) |Prozent |Maximum | |
| connectedclients6 |Verbundene Clients (Shard 6) |Count |Maximum | |
| totalcommandsprocessed6 |Vorgänge gesamt (Shard 6) |Count |Gesamt | |
| cachehits6 |Cachetreffer (Shard 6) |Count |Gesamt | |
| cachemisses6 |Cachefehler (Shard 6) |Count |Gesamt | |
| getcommands6 |Get-Vorgänge (Shard 6) |Count |Gesamt | |
| setcommands6 |Set-Vorgänge (Shard 6) |Count |Gesamt | |
| evictedkeys6 |Entfernte Schlüssel (Shard 6) |Count |Gesamt | |
| totalkeys6 |Schlüssel gesamt (Knoten 6) |Count |Maximum | |
| expiredkeys6 |Abgelaufene Schlüssel (Shard 6) |Count |Gesamt | |
| usedmemory6 |Verwendeter Arbeitsspeicher (Shard 6) |Byte |Maximum | |
| usedmemoryRss6 |Verwendeter Arbeitsspeicher (RSS, Shard 6) |Byte |Maximum | |
| serverLoad6 |Serverauslastung (Shard 6) |Prozent |Maximum | |
| cacheWrite6 |Cacheschreibvorgänge (Shard 6) |Bytes pro Sekunde |Maximum | |
| cacheRead6 |Cachelesevorgänge (Shard 6) |Bytes pro Sekunde |Maximum | |
| percentProcessorTime6 |CPU (Shard 6) |Prozent |Maximum | |
| connectedclients7 |Verbundene Clients (Shard 7) |Count |Maximum | |
| totalcommandsprocessed7 |Vorgänge gesamt (Shard 7) |Count |Gesamt | |
| cachehits7 |Cachetreffer (Shard 7) |Count |Gesamt | |
| cachemisses7 |Cachefehler (Shard 7) |Count |Gesamt | |
| getcommands7 |Get-Vorgänge (Shard 7) |Count |Gesamt | |
| setcommands7 |Set-Vorgänge (Shard 7) |Count |Gesamt | |
| evictedkeys7 |Entfernte Schlüssel (Shard 7) |Count |Gesamt | |
| totalkeys7 |Schlüssel gesamt (Knoten 7) |Count |Maximum | |
| expiredkeys7 |Abgelaufene Schlüssel (Shard 7) |Count |Gesamt | |
| usedmemory7 |Verwendeter Arbeitsspeicher (Shard 7) |Byte |Maximum | |
| usedmemoryRss7 |Verwendeter Arbeitsspeicher (RSS, Shard 7) |Byte |Maximum | |
| serverLoad7 |Serverauslastung (Shard 7) |Prozent |Maximum | |
| cacheWrite7 |Cacheschreibvorgänge (Shard 7) |Bytes pro Sekunde |Maximum | |
| cacheRead7 |Cachelesevorgänge (Shard 7) |Bytes pro Sekunde |Maximum | |
| percentProcessorTime7 |CPU (Shard 7) |Prozent |Maximum | |
| connectedclients8 |Verbundene Clients (Shard 8) |Count |Maximum | |
| totalcommandsprocessed8 |Vorgänge gesamt (Shard 8) |Count |Gesamt | |
| cachehits8 |Cachetreffer (Shard 8) |Count |Gesamt | |
| cachemisses8 |Cachefehler (Shard 8) |Count |Gesamt | |
| getcommands8 |Get-Vorgänge (Shard 8) |Count |Gesamt | |
| setcommands8 |Set-Vorgänge (Shard 8) |Count |Gesamt | |
| evictedkeys8 |Entfernte Schlüssel (Shard 8) |Count |Gesamt | |
| totalkeys8 |Schlüssel gesamt (Knoten 8) |Count |Maximum | |
| expiredkeys8 |Abgelaufene Schlüssel (Shard 8) |Count |Gesamt | |
| usedmemory8 |Verwendeter Arbeitsspeicher (Shard 8) |Byte |Maximum | |
| usedmemoryRss8 |Verwendeter Arbeitsspeicher (RSS, Shard 8) |Byte |Maximum | |
| serverLoad8 |Serverauslastung (Shard 8) |Prozent |Maximum | |
| cacheWrite8 |Cacheschreibvorgänge (Shard 8) |Bytes pro Sekunde |Maximum | |
| cacheRead8 |Cachelesevorgänge (Shard 8) |Bytes pro Sekunde |Maximum | |
| percentProcessorTime8 |CPU (Shard 8) |Prozent |Maximum | |
| connectedclients9 |Verbundene Clients (Shard 9) |Count |Maximum | |
| totalcommandsprocessed9 |Vorgänge gesamt (Shard 9) |Count |Gesamt | |
| cachehits9 |Cachetreffer (Shard 9) |Count |Gesamt | |
| cachemisses9 |Cachefehler (Shard 9) |Count |Gesamt | |
| getcommands9 |Get-Vorgänge (Shard 9) |Count |Gesamt | |
| setcommands9 |Set-Vorgänge (Shard 9) |Count |Gesamt | |
| evictedkeys9 |Entfernte Schlüssel (Shard 9) |Count |Gesamt | |
| totalkeys9 |Schlüssel gesamt (Knoten 9) |Count |Maximum | |
| expiredkeys9 |Abgelaufene Schlüssel (Shard 9) |Count |Gesamt | |
| usedmemory9 |Verwendeter Arbeitsspeicher (Shard 9) |Byte |Maximum | |
| usedmemoryRss9 |Verwendeter Arbeitsspeicher (RSS, Shard 9) |Byte |Maximum | |
| serverLoad9 |Serverauslastung (Shard 9) |Prozent |Maximum | |
| cacheWrite9 |Cacheschreibvorgänge (Shard 9) |Bytes pro Sekunde |Maximum | |
| cacheRead9 |Cachelesevorgänge (Shard 9) |Bytes pro Sekunde |Maximum | |
| percentProcessorTime9 |CPU (Shard 9) |Prozent |Maximum | |

## Microsoft.Compute/virtualMachines
| Metrik | Metrikanzeigename | Unit | Aggregationstyp | Beschreibung |
| --- | --- | --- | --- | --- |
| Prozentsatz CPU |Prozentsatz CPU |Prozent |Durchschnitt |Der Prozentsatz der zugewiesenen Computeeinheiten, die derzeit von den virtuellen Computern verwendet werden |
| Eingehender Netzwerkdatenverkehr |Eingehender Netzwerkdatenverkehr |Byte |Gesamt |Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen empfangen werden (eingehender Datenverkehr) |
| Ausgehender Netzwerkdatenverkehr |Ausgehender Netzwerkdatenverkehr |Byte |Gesamt |Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen gesendet werden (ausgehender Datenverkehr) |
| Datenträgerlesevorgänge in Bytes |Datenträgerlesevorgänge in Bytes |Byte |Gesamt |Gesamtzahl von Bytes, die während des Überwachungszeitraums vom Datenträger gelesen werden |
| Datenträgerschreibvorgänge in Bytes |Datenträgerschreibvorgänge in Bytes |Byte |Gesamt |Gesamtzahl von Bytes, die während des Überwachungszeitraums auf den Datenträger geschrieben werden |
| Datenträgerlesevorgänge/Sek. |Datenträgerlesevorgänge/Sek. |Anzahl pro Sekunde |Durchschnitt |Datenträgerlesevorgänge in IOPS |
| Datenträgerschreibvorgänge/Sek. |Datenträgerschreibvorgänge/Sek. |Anzahl pro Sekunde |Durchschnitt |Datenträgerschreibvorgänge in IOPS |

## Microsoft.Compute/virtualMachineScaleSets
| Metrik | Metrikanzeigename | Unit | Aggregationstyp | Beschreibung |
| --- | --- | --- | --- | --- |
| Prozentsatz CPU |Prozentsatz CPU |Prozent |Durchschnitt |Der Prozentsatz der zugewiesenen Computeeinheiten, die derzeit von den virtuellen Computern verwendet werden |
| Eingehender Netzwerkdatenverkehr |Eingehender Netzwerkdatenverkehr |Byte |Gesamt |Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen empfangen werden (eingehender Datenverkehr) |
| Ausgehender Netzwerkdatenverkehr |Ausgehender Netzwerkdatenverkehr |Byte |Gesamt |Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen gesendet werden (ausgehender Datenverkehr) |
| Datenträgerlesevorgänge in Bytes |Datenträgerlesevorgänge in Bytes |Byte |Gesamt |Gesamtzahl von Bytes, die während des Überwachungszeitraums vom Datenträger gelesen werden |
| Datenträgerschreibvorgänge in Bytes |Datenträgerschreibvorgänge in Bytes |Byte |Gesamt |Gesamtzahl von Bytes, die während des Überwachungszeitraums auf den Datenträger geschrieben werden |
| Datenträgerlesevorgänge/Sek. |Datenträgerlesevorgänge/Sek. |Anzahl pro Sekunde |Durchschnitt |Datenträgerlesevorgänge in IOPS |
| Datenträgerschreibvorgänge/Sek. |Datenträgerschreibvorgänge/Sek. |Anzahl pro Sekunde |Durchschnitt |Datenträgerschreibvorgänge in IOPS |

## Microsoft.Compute/virtualMachineScaleSets/virtualMachines
| Metrik | Metrikanzeigename | Unit | Aggregationstyp | Beschreibung |
| --- | --- | --- | --- | --- |
| Prozentsatz CPU |Prozentsatz CPU |Prozent |Durchschnitt |Der Prozentsatz der zugewiesenen Computeeinheiten, die derzeit von den virtuellen Computern verwendet werden |
| Eingehender Netzwerkdatenverkehr |Eingehender Netzwerkdatenverkehr |Byte |Gesamt |Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen empfangen werden (eingehender Datenverkehr) |
| Ausgehender Netzwerkdatenverkehr |Ausgehender Netzwerkdatenverkehr |Byte |Gesamt |Die Anzahl von Bytes, die von den virtuellen Computern an allen Netzwerkschnittstellen gesendet werden (ausgehender Datenverkehr) |
| Datenträgerlesevorgänge in Bytes |Datenträgerlesevorgänge in Bytes |Byte |Gesamt |Gesamtzahl von Bytes, die während des Überwachungszeitraums vom Datenträger gelesen werden |
| Datenträgerschreibvorgänge in Bytes |Datenträgerschreibvorgänge in Bytes |Byte |Gesamt |Gesamtzahl von Bytes, die während des Überwachungszeitraums auf den Datenträger geschrieben werden |
| Datenträgerlesevorgänge/Sek. |Datenträgerlesevorgänge/Sek. |Anzahl pro Sekunde |Durchschnitt |Datenträgerlesevorgänge in IOPS |
| Datenträgerschreibvorgänge/Sek. |Datenträgerschreibvorgänge/Sek. |Anzahl pro Sekunde |Durchschnitt |Datenträgerschreibvorgänge in IOPS |

## Microsoft.Devices/IotHubs
| Metrik | Metrikanzeigename | Unit | Aggregationstyp | Beschreibung |
| --- | --- | --- | --- | --- |
| d2c.telemetry.Ingress.allProtocol |Versuche zum Senden von Telemetrienachrichten |Count |Gesamt |Anzahl von Telemetrienachrichten vom Gerät an die Cloud, die an Ihren IoT Hub gesendet werden sollten |
| d2c.telemetry.ingress.success |Gesendete Telemetrienachrichten |Count |Gesamt |Anzahl von Telemetrienachrichten vom Gerät an die Cloud, die erfolgreich an Ihren IoT Hub gesendet wurden |
| c2d.commands.egress.complete.success |Abgeschlossene Befehle |Count |Gesamt |Anzahl von Befehlen von der Cloud an das Gerät, die vom Gerät erfolgreich abgeschlossen wurden |
| c2d.commands.egress.abandon.success |Abgebrochene Befehle |Count |Gesamt |Anzahl von Befehlen von der Cloud an das Gerät, die vom Gerät abgebrochen wurden |
| c2d.commands.egress.reject.success |Abgelehnte Befehle |Count |Gesamt |Anzahl von Befehlen von der Cloud an das Gerät, die vom Gerät abgelehnt wurden |
| devices.totalDevices |Gesamtanzahl von Geräten |Count |Gesamt |Die Anzahl von Geräten, die beim IoT Hub registriert sind |
| devices.connectedDevices.allProtocol |Verbundene Geräte |Count |Gesamt |Die Anzahl von Geräten, die mit dem IoT Hub verbunden sind |

## Microsoft.EventHub/namespaces
| Metrik | Metrikanzeigename | Unit | Aggregationstyp | Beschreibung |
| --- | --- | --- | --- | --- |
| INREQS |Eingehende Anforderungen |Count |Gesamt | |
| SUCCREQ |Erfolgreiche Anforderungen |Count |Gesamt | |
| FAILREQ |Anforderungsfehler |Count |Gesamt | |
| SVRBSY |Fehler durch ausgelasteten Server |Count |Gesamt | |
| INTERR |Interne Serverfehler |Count |Gesamt | |
| MISCERR |Andere Fehler |Count |Gesamt | |
| INMSGS |Eingehende Nachrichten |Count |Gesamt | |
| OUTMSGS |Ausgehende Nachrichten |Count |Gesamt | |
| EHINMBS |Eingehende Bytes pro Sekunde |Bytes pro Sekunde |Gesamt | |
| EHOUTMBS |Ausgehende Bytes pro Sekunde |Bytes pro Sekunde |Gesamt | |
| EHABL |Archivierte Backlognachrichten |Count |Gesamt | |
| EHAMSGS |Archivnachrichten |Count |Gesamt | |
| EHAMBS |Durchsatz von Archivnachrichten |Bytes pro Sekunde |Gesamt | |

## Microsoft.Logic/workflows
| Metrik | Metrikanzeigename | Unit | Aggregationstyp | Beschreibung |
| --- | --- | --- | --- | --- |
| RunsStarted |Gestartete Ausführungen |Count |Gesamt |Anzahl gestarteter Workflowausführungen. |
| RunsCompleted |Abgeschlossene Ausführungen |Count |Gesamt |Anzahl abgeschlossener Workflowausführungen. |
| RunsSucceeded |Erfolgreiche Ausführungen |Count |Gesamt |Anzahl erfolgreicher Workflowausführungen. |
| RunsFailed |Ausführungsfehler |Count |Gesamt |Anzahl fehlerhafter Workflowausführungen. |
| RunsCancelled |Abgebrochene Ausführungen |Count |Gesamt |Anzahl abgebrochener Workflowausführungen. |
| RunLatency |Ausführungslatenz |Sekunden |Durchschnitt |Latenz abgeschlossener Workflowausführungen. |
| RunSuccessLatency |Latenz erfolgreicher Ausführungen |Sekunden |Durchschnitt |Latenz erfolgreicher Workflowausführungen. |
| RunThrottledEvents |Ereignisse zu gedrosselten Ausführungen |Count |Gesamt |Anzahl von Ereignissen zu gedrosselten Workflowaktionen oder Triggern. |
| ActionsStarted |Gestartete Aktionen |Count |Gesamt |Anzahl gestarteter Workflowaktionen. |
| ActionsCompleted |Abgeschlossene Aktionen |Count |Gesamt |Anzahl abgeschlossener Workflowaktionen. |
| ActionsSucceeded |Erfolgreiche Aktionen |Count |Gesamt |Anzahl erfolgreicher Workflowaktionen. |
| ActionsFailed |Aktionsfehler |Count |Gesamt |Anzahl fehlerhafter Workflowaktionen. |
| ActionsSkipped |Übersprungene Aktionen |Count |Gesamt |Anzahl übersprungener Workflowaktionen. |
| ActionLatency |Aktionslatenz |Sekunden |Durchschnitt |Latenz abgeschlossener Workflowaktionen. |
| ActionSuccessLatency |Latenz erfolgreicher Aktionen |Sekunden |Durchschnitt |Latenz erfolgreicher Workflowaktionen. |
| ActionThrottledEvents |Ereignisse zu gedrosselten Aktionen |Count |Gesamt |Anzahl von Ereignissen zu gedrosselten Workflowaktionen. |
| TriggersStarted |Gestartete Trigger |Count |Gesamt |Anzahl gestarteter Workflowtrigger. |
| TriggersCompleted |Abgeschlossene Trigger |Count |Gesamt |Anzahl abgeschlossener Workflowtrigger. |
| TriggersSucceeded |Erfolgreiche Trigger |Count |Gesamt |Anzahl erfolgreicher Workflowtrigger. |
| TriggersFailed |Triggerfehler |Count |Gesamt |Anzahl fehlerhafter Workflowtrigger. |
| TriggersSkipped |Übersprungene Trigger |Count |Gesamt |Anzahl übersprungener Workflowtrigger. |
| TriggersFired |Ausgelöste Trigger |Count |Gesamt |Anzahl ausgelöster Workflowtrigger. |
| TriggerLatency |Triggerlatenz |Sekunden |Durchschnitt |Latenz abgeschlossener Workflowtrigger. |
| TriggerFireLatency |Latenz beim Auslösen von Triggern |Sekunden |Durchschnitt |Latenz ausgelöster Workflowtrigger. |
| TriggerSuccessLatency |Latenz erfolgreicher Trigger |Sekunden |Durchschnitt |Latenz erfolgreicher Workflowtrigger. |
| TriggerThrottledEvents |Ereignisse zu gedrosselten Triggern |Count |Gesamt |Anzahl von Ereignissen zu gedrosselten Workflowtriggern. |

## Microsoft.Network/applicationGateways
| Metrik | Metrikanzeigename | Unit | Aggregationstyp | Beschreibung |
| --- | --- | --- | --- | --- |
| Durchsatz |Durchsatz |Bytes pro Sekunde |Durchschnitt | |

## Microsoft.Search/searchServices
| Metrik | Metrikanzeigename | Unit | Aggregationstyp | Beschreibung |
| --- | --- | --- | --- | --- |
| Latenz |Latenz |Sekunden |Durchschnitt |Durchschnittliche Latenz für den Suchdienst |
| SearchLatency |Suchlatenz |Sekunden |Durchschnitt |Durchschnittliche Suchlatenz für den Suchdienst |
| SearchQueriesPerSecond |Suchabfragen pro Sekunde |Anzahl pro Sekunde |Durchschnitt |Suchabfragen pro Sekunde für den Suchdienst |
| ThrottledSearchQueriesPercentage |Gedrosselte Suchabfragen in Prozent |Prozent |Durchschnitt |Prozentsatz der Suchabfragen, die für den Suchdienst gedrosselt wurden |

## Microsoft.ServiceBus/namespaces
| Metrik | Metrikanzeigename | Unit | Aggregationstyp | Beschreibung |
| --- | --- | --- | --- | --- |
| CPUXNS |CPU-Auslastung pro Namespace |Prozent |Maximum | |
| WSXNS |Auslastung der Arbeitsspeichergröße pro Namespace |Prozent |Maximum | |

## Microsoft.Sql/servers/databases
| Metrik | Metrikanzeigename | Unit | Aggregationstyp | Beschreibung |
| --- | --- | --- | --- | --- |
| cpu\_percent |CPU-Prozentsatz |Prozent |Maximum |CPU-Prozentsatz |
| physical\_data\_read\_percent |E/A-Prozentsatz für Daten |Prozent |Maximum |E/A-Prozentsatz für Daten |
| log\_write\_percent |E/A-Prozentsatz für Protokoll |Prozent |Maximum |E/A-Prozentsatz für Protokoll |
| dtu\_consumption\_percent |DTU-Prozentsatz |Prozent |Maximum |DTU-Prozentsatz |
| storage |Datenbankgröße gesamt |Byte |Maximum |Datenbankgröße gesamt |
| connection\_successful |Erfolgreiche Verbindungen |Count |Gesamt |Erfolgreiche Verbindungen |
| connection\_failed |Verbindungsfehler |Count |Gesamt |Verbindungsfehler |
| blocked\_by\_firewall |Von der Firewall blockiert |Count |Gesamt |Von der Firewall blockiert |
| deadlock |Deadlocks |Count |Gesamt |Deadlocks |
| storage\_percent |Datenbankgröße als Prozentsatz |Prozent |Maximum |Datenbankgröße als Prozentsatz |
| xtp\_storage\_percent |In-Memory-OLTP-Speicher in Prozent (Vorschau) |Prozent |Maximum |In-Memory-OLTP-Speicher in Prozent (Vorschau) |
| workers\_percent |Worker in Prozent |Prozent |Maximum |Worker in Prozent |
| sessions\_percent |Sitzungen in Prozent |Prozent |Maximum |Sitzungen in Prozent |
| dtu\_limit |DTU-Grenzwert |Count |Maximum |DTU-Grenzwert |
| dtu\_used |DTU-Verbrauch |Count |Maximum |DTU-Verbrauch |
| service\_level\_objective |SLO der Datenbank |Count |Gesamt |SLO der Datenbank |
| dwu\_limit |DWU-Grenzwert |Count |Maximum |DWU-Grenzwert |
| dwu\_consumption\_percent |DWU in Prozent |Prozent |Maximum |DWU in Prozent |
| dwu\_used |DWU-Verbrauch |Count |Maximum |DWU-Verbrauch |

## Microsoft.Sql/servers/elasticPools
| Metrik | Metrikanzeigename | Unit | Aggregationstyp | Beschreibung |
| --- | --- | --- | --- | --- |
| cpu\_percent |CPU-Prozentsatz |Prozent |Maximum |CPU-Prozentsatz |
| physical\_data\_read\_percent |E/A-Prozentsatz für Daten |Prozent |Maximum |E/A-Prozentsatz für Daten |
| log\_write\_percent |E/A-Prozentsatz für Protokoll |Prozent |Maximum |E/A-Prozentsatz für Protokoll |
| dtu\_consumption\_percent |DTU-Prozentsatz |Prozent |Maximum |DTU-Prozentsatz |
| storage\_percent |Speicher in Prozent |Prozent |Maximum |Speicher in Prozent |
| workers\_percent |Worker in Prozent |Prozent |Maximum |Worker in Prozent |
| sessions\_percent |Sitzungen in Prozent |Prozent |Maximum |Sitzungen in Prozent |
| eDTU\_limit |eDTU-Grenzwert |Count |Maximum |eDTU-Grenzwert |
| storage\_limit |Speicherbegrenzung |Byte |Maximum |Speicherbegrenzung |
| eDTU\_used |eDTU-Verbrauch |Count |Maximum |eDTU-Verbrauch |
| storage\_used |Verwendeter Speicher |Byte |Maximum |Verwendeter Speicher |

## Microsoft.StreamAnalytics/streamingjobs
| Metrik | Metrikanzeigename | Unit | Aggregationstyp | Beschreibung |
| --- | --- | --- | --- | --- |
| ResourceUtilization |Speichereinheitnutzung in % |Prozent |Maximum |Speichereinheitnutzung in % |
| InputEvents |Eingabeereignisse |Byte |Gesamt |Eingabeereignisse |
| InputEventBytes |Eingabeereignisbytes |Byte |Gesamt |Eingabeereignisbytes |
| LateInputEvents |Ereignisse bei verspäteter Eingabe |Byte |Gesamt |Ereignisse bei verspäteter Eingabe |
| OutputEvents |Ausgabeereignisse |Byte |Gesamt |Ausgabeereignisse |
| ConversionErrors |Konvertierungsfehler |Byte |Gesamt |Konvertierungsfehler |
| Fehler |Laufzeitfehler |Byte |Gesamt |Laufzeitfehler |
| DroppedOrAdjustedEvents |Ereignisse für falsche Reihenfolge |Byte |Gesamt |Ereignisse für falsche Reihenfolge |
| AMLCalloutRequests |Funktionsanforderungen |Byte |Gesamt |Funktionsanforderungen |
| AMLCalloutFailedRequests |Fehlerhafte Funktionsanforderungen |Byte |Gesamt |Fehlerhafte Funktionsanforderungen |
| AMLCalloutInputEvents |Funktionsereignisse |Byte |Gesamt |Funktionsereignisse |

## Microsoft.Web/serverfarms
| Metrik | Metrikanzeigename | Unit | Aggregationstyp | Beschreibung |
| --- | --- | --- | --- | --- |
| CpuPercentage |CPU-Prozentsatz |Prozent |Durchschnitt |CPU-Prozentsatz |
| MemoryPercentage |Arbeitsspeicherprozentsatz |Prozent |Durchschnitt |Arbeitsspeicherprozentsatz |
| DiskQueueLength |Warteschlangenlänge des Datenträgers |Count |Gesamt |Warteschlangenlänge des Datenträgers |
| HttpQueueLength |Länge der HTTP-Warteschlange |Count |Gesamt |Länge der HTTP-Warteschlange |
| BytesReceived |Eingehende Daten |Byte |Gesamt |Eingehende Daten |
| BytesSent |Datenausgabe |Byte |Gesamt |Datenausgabe |

## Microsoft.Web/sites
| Metrik | Metrikanzeigename | Unit | Aggregationstyp | Beschreibung |
| --- | --- | --- | --- | --- |
| CpuTime |CPU-Zeit |Sekunden |Gesamt |CPU-Zeit |
| Anforderungen |Anforderungen |Count |Gesamt |Anforderungen |
| BytesReceived |Eingehende Daten |Byte |Gesamt |Eingehende Daten |
| BytesSent |Datenausgabe |Byte |Gesamt |Datenausgabe |
| Http2xx |HTTP 2xx |Count |Gesamt |HTTP 2xx |
| Http3xx |HTTP 3xx |Count |Gesamt |HTTP 3xx |
| Http401 |HTTP 401 |Count |Gesamt |HTTP 401 |
| Http403 |HTTP 403 |Count |Gesamt |HTTP 403 |
| Http404 |HTTP 404 |Count |Gesamt |HTTP 404 |
| Http406 |HTTP 406 |Count |Gesamt |HTTP 406 |
| Http4xx |HTTP 4xx |Count |Gesamt |HTTP 4xx |
| Http5xx |HTTP-Serverfehler |Count |Gesamt |HTTP-Serverfehler |
| MemoryWorkingSet |Arbeitssatz für Arbeitsspeicher |Byte |Gesamt |Arbeitssatz für Arbeitsspeicher |
| AverageMemoryWorkingSet |Durchschnittlicher Arbeitssatz für Arbeitsspeicher |Byte |Durchschnitt |Durchschnittlicher Arbeitssatz für Arbeitsspeicher |
| AverageResponseTime |Durchschnittliche Antwortzeit |Sekunden |Durchschnitt |Durchschnittliche Antwortzeit |

## Microsoft.Web/sites/slots
| Metrik | Metrikanzeigename | Unit | Aggregationstyp | Beschreibung |
| --- | --- | --- | --- | --- |
| CpuTime |CPU-Zeit |Sekunden |Gesamt |CPU-Zeit |
| Anforderungen |Anforderungen |Count |Gesamt |Anforderungen |
| BytesReceived |Eingehende Daten |Byte |Gesamt |Eingehende Daten |
| BytesSent |Datenausgabe |Byte |Gesamt |Datenausgabe |
| Http2xx |HTTP 2xx |Count |Gesamt |HTTP 2xx |
| Http3xx |HTTP 3xx |Count |Gesamt |HTTP 3xx |
| Http401 |HTTP 401 |Count |Gesamt |HTTP 401 |
| Http403 |HTTP 403 |Count |Gesamt |HTTP 403 |
| Http404 |HTTP 404 |Count |Gesamt |HTTP 404 |
| Http406 |HTTP 406 |Count |Gesamt |HTTP 406 |
| Http4xx |HTTP 4xx |Count |Gesamt |HTTP 4xx |
| Http5xx |HTTP-Serverfehler |Count |Gesamt |HTTP-Serverfehler |
| MemoryWorkingSet |Arbeitssatz für Arbeitsspeicher |Byte |Gesamt |Arbeitssatz für Arbeitsspeicher |
| AverageMemoryWorkingSet |Durchschnittlicher Arbeitssatz für Arbeitsspeicher |Byte |Durchschnitt |Durchschnittlicher Arbeitssatz für Arbeitsspeicher |
| AverageResponseTime |Durchschnittliche Antwortzeit |Sekunden |Durchschnitt |Durchschnittliche Antwortzeit |

## Nächste Schritte
* [Informationen zu Metriken in Azure Monitor](monitoring-overview.md#monitoring-sources)
* [Erstellen von Warnungen zu Metriken](insights-receive-alert-notifications.md)
* [Exportieren von Metriken in Storage, Event Hub oder Log Analytics](monitoring-overview-of-diagnostic-logs.md)

<!---HONumber=AcomDC_0928_2016-->