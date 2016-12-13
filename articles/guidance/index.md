---
title: Azure-Anleitungen | Microsoft-Dokumentation
description: "Bewährte Methoden und Anleitungen für Azure"
services: 
documentationcenter: na
author: bennage
manager: marksou
editor: 
tags: 
ms.assetid: de94c74a-fea7-4815-8484-553e421a7490
ms.service: guidance
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2016
ms.author: christb
translationtype: Human Translation
ms.sourcegitcommit: 5f3ced657cf3d6587a63789b3dd3ca41cd2856f0
ms.openlocfilehash: 0061e1ff2ae2d6b8ed7b7c3bb60405e76d4cc91b


---
# <a name="azure-guidance"></a>Azure-Anleitungen
[!INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Das Microsoft Patterns & Practices-Team ist Teil des Azure Customer Advisory Teams. Unser Ziel ist es, Entwicklern, Architekten und IT-Experten auf der Microsoft Azure Platform zum Erfolg zu verhelfen. Wir entwickeln Anleitungen mit bewährten Methoden zum Erstellen von Cloudlösungen in Azure.

## <a name="checklists"></a>Checklists
Diese Listen sind eine Kurzübersicht zum Überprüfen der grundlegenden Aspekte der Verfügbarkeit und Skalierbarkeit. 

* [Checkliste für die Verfügbarkeit][AvailabilityChecklist] 
  
    Eine Zusammenfassung der empfohlenen Methoden zum Sicherstellen der Verfügbarkeit.
* [Checkliste für die Skalierbarkeit][ScalabilityChecklist]
  
    Eine Zusammenfassung der empfohlenen Methoden für das Entwerfen und Implementieren skalierbarer Dienste sowie für die Datenverwaltung.

## <a name="best-practices-articles"></a>Artikel zu bewährten Methoden
Diese Artikel bieten eine ausführliche Erläuterung wichtiger Konzepte im allgemeinen Zusammenhang mit Cloud Computing. 

* [API-Entwurf][APIDesign] 
  
    Eine Erläuterung von Designproblemen, die beim Entwerfen einer Web-API zu berücksichtigen sind.
* [API-Implementierung][APIImplementation] 
  
    Eine Reihe von empfohlenen Methoden zum Implementieren und Veröffentlichen einer Web-API.
* [API-Sicherheitsleitfaden](https://github.com/mspnp/azure-guidance/blob/master/API-security.md) 
  
    Eine Erläuterung von Authentifizierungs- und Autorisierungsaspekten (z.B. Tokentypen, Autorisierungsprotokolle Autorisierungsabläufe und Bedrohungsminderung).
* [Anleitungen für die automatische Skalierung][AutoscalingGuidance] 
  
    Eine Zusammenfassung der Überlegungen für die Skalierung von Lösungen ohne die Notwendigkeit eines manuellen Eingriffs.
* [Anleitungen für Hintergrundaufträge][BackgroundJobsGuidance] 
  
    Eine Beschreibung der verfügbaren Optionen und empfohlenen Methoden für das Implementieren von Aufgaben, die im Hintergrund und unabhängig von Vordergrundvorgängen oder interaktiven Vorgängen ausgeführt werden sollen.
* [Anleitungen zum Content Delivery Network (CDN)][CDNGuidance] 
  
    Allgemeine Richtlinien und empfohlene Methoden für die Verwendung von CDN zum Reduzieren der Belastung Ihrer Anwendungen und zum Optimieren der Verfügbarkeit und Leistung.
* [Anleitungen zum Caching][CachingGuidance] 
  
    Eine Zusammenfassung zur Verwendung der Zwischenspeicherung zur Verbesserung der Leistung und Skalierbarkeit eines Systems.
* [Leitfaden zur Datenpartitionierung][DataPartitioningGuidance]
  
    Strategien, die Sie zur Partitionierung von Daten verwenden können, um die Skalierbarkeit zu verbessern, Konflikte zu reduzieren und die Leistung zu optimieren.
* [Anleitung zur Überwachung und Diagnose][MonitoringandDiagnosticsGuidance] 
  
    Anleitungen zur Nachverfolgung, auf welche Weise Benutzer Ihr System verwenden, sowie zur Nachverfolgung der Ressourcennutzung und allgemeinen Überwachung der Integrität und Leistung des Systems.
* [Empfohlene Benennungskonventionen][naming-conventions] 
  
    Empfohlene Benennungskonventionen für Azure-Ressourcen
* [Allgemeiner Leitfaden zum Wiederholen von Vorgängen][RetryGeneralGuidance] 
  
    Erläuterung der allgemeinen Konzepte für die Behandlung vorübergehender Fehler.
* [Anleitung zu dienstspezifischen Wiederholungsmechanismen][RetryServiceSpecificGuidance]
  
    Eine Zusammenfassung der Wiederholungsfunktionen für zahlreiche Azure-Dienste, einschließlich Informationen, mit denen Sie einen Wiederholungsmechanismus für einen bestimmten Dienst verwenden, anpassen oder erweitern können.

## <a name="scenario-guides"></a>Anleitungen zu Szenarien
* [Ausführen von Elasticsearch in Azure][elasticsearch] 
  
    Elasticsearch ist ein hochgradig skalierbares Open-Source-Suchmodul mit Datenbank. Es ist für Situationen geeignet, in denen eine schnelle Analyse und Ermittlung von Informationen in großen Datasets erforderlich ist. In dieser Anleitung werden einige wichtige Aspekte behandelt, die beim Entwerfen eines Elasticsearch-Clusters zu berücksichtigen sind.
* [Identitätsverwaltung für mehrinstanzenfähige Anwendungen][identity-multitenant] 
  
    Mehrinstanzenfähigkeit ist eine Architektur, bei der mehrere Mandanten eine Anwendung gemeinsam verwenden, jedoch voneinander isoliert sind. In dieser Anleitung wird gezeigt, wie Benutzeridentitäten in einer mehrinstanzenfähigen Anwendung verwaltet werden, wobei [Azure Active Directory][AzureAD] für die Anmeldung und Authentifizierung zuständig ist.
* [Entwickeln von Big Data-Lösungen](https://msdn.microsoft.com/library/dn749874.aspx)
  
    In diesem Handbuch wird die Verwendung von HDInsight für Szenarien wie iteratives Durchsuchen behandelt, als Datawarehouse, für ETL-Prozesse sowie die Integration in vorhandene BI-Systeme. Außerdem enthält es Erläuterungen zu den Konzepten von Big Data sowie Anleitungen für Planung und Entwurf von Big Data-Lösungen und die Implementierung dieser Lösungen.

## <a name="patterns"></a>Muster
* [Cloudentwurfsmuster: Präskriptive Anleitung zur Architektur für Cloudanwendungen](https://msdn.microsoft.com/library/dn568099.aspx)
  
    Cloudentwurfsmuster ist eine Bibliothek mit Entwurfsmustern und entsprechenden Anleitungsthemen. Darin wird der Vorteil der Anwendung von Mustern hervorgehoben, indem gezeigt wird, wie jedes Element in Cloudanwendungsarchitekturen eingepasst werden kann.
* [Optimieren der Leistung für Cloudanwendungen](https://github.com/mspnp/performance-optimization)
  
    Diese Anleitung ist eine Untersuchung allgemeiner Anti-Muster, die das Skalieren von Apps unter Last verhindern. Sie enthält Beispiele, die acht Anti-Muster veranschaulichen, [Grundlagen zur Leistungsanalyse](https://github.com/mspnp/performance-optimization/blob/master/Performance-Analysis-Primer.md) und eine Anleitung zum [Bewerten der Leistung anhand von Schlüsselmetriken](https://github.com/mspnp/performance-optimization/blob/master/Assessing-System-Performance-Against-KPI.md).

## <a name="reference-architectures"></a>Referenzarchitekturen
Unsere Referenzarchitekturen sind nach Szenario sortiert.
Jede Architektur bietet empfohlene Vorgehensweisen, Anweisungen und eine ausführbare Komponente, die die Empfehlungen verkörpert.

Die aktuelle Bibliothek der Referenzarchitekturen finden Sie unter [http://aka.ms/architecture](http://aka.ms/architecture).

## <a name="resiliency-guidance"></a>Leitfaden zu Resilienz
In diesen Themen wird das Entwerfen von Anwendungen beschrieben, die in einer verteilten Cloudumgebung robust gegenüber Fehlern sind.   

* [Resilienz (Übersicht)][ResiliencyOvervew]
  
     Informationen zur Erstellung von Anwendungen auf der Azure-Plattform, die wiederhergestellt und weiterhin ausgeführt werden können. Beschreibt einen strukturierten Ansatz zum Erzielen von Resilienz vom Entwurf bis zur Implementierung, Bereitstellung und Vorgängen.
* [Checkliste für Resilienz][resiliency-checklist]
  
    Eine Checkliste mit Empfehlungen, die Ihnen beim Planen für eine Vielzahl von Fehlerzuständen helfen, die auftreten können.
* [Fehlermodusanalyse][resiliency-fma] 
  
    Die Fehlermodusanalyse (Failure Mode Analysis, FMA) ist ein Prozess zum Erzielen von Resilienz in einem System durch das Identifizieren möglicher Schwachstellen. Dieser Artikel enthält als Ausgangspunkt für den FMA-Prozess einen Katalog mit möglichen Fehlerzuständen und den entsprechenden Gegenmaßnahmen. 

<!-- links -->

[AzureAD]: https://azure.microsoft.com/documentation/services/active-directory/

[PerformanceOptimization]: https://github.com/mspnp/performance-optimization

[APIDesign]: ../best-practices-api-design.md
[APIImplementation]: ../best-practices-api-implementation.md
[AutoscalingGuidance]: ../best-practices-auto-scaling.md
[BackgroundJobsGuidance]: ../best-practices-background-jobs.md
[CDNGuidance]: ../best-practices-cdn.md
[CachingGuidance]: ../best-practices-caching.md
[DataPartitioningGuidance]: ../best-practices-data-partitioning.md
[MonitoringandDiagnosticsGuidance]: ../best-practices-monitoring.md
[RetryGeneralGuidance]: ../best-practices-retry-general.md
[RetryServiceSpecificGuidance]: ../best-practices-retry-service-specific.md
[RetryPolicies]: Retry-Policies.md
[ScalabilityChecklist]: ../best-practices-scalability-checklist.md
[AvailabilityChecklist]: ../best-practices-availability-checklist.md
[naming-conventions]: guidance-naming-conventions.md

<!-- guidance projects -->
[elasticsearch]: guidance-elasticsearch.md
[identity-multitenant]: guidance-multitenant-identity.md

<!-- reference architectures -->
[ref-arch-single-vm-windows]: guidance-compute-single-vm.md
[ref-arch-single-vm-linux]: guidance-compute-single-vm-linux.md
[ref-arch-multi-vm]: guidance-compute-multi-vm.md
[ref-arch-3-tier]: guidance-compute-3-tier-vm.md
[ref-arch-n-tier-windows]: guidance-compute-n-tier-vm.md
[ref-arch-n-tier-linux]: guidance-compute-n-tier-vm-linux.md
[ref-arch-multi-dc-windows]: guidance-compute-multiple-datacenters.md
[ref-arch-multi-dc-linux]: guidance-compute-multiple-datacenters-linux.md

<!-- resiliency -->
[resiliency-fma]: guidance-resiliency-failure-mode-analysis.md
[resiliency-checklist]: guidance-resiliency-checklist.md
[ResiliencyOvervew]: guidance-resiliency-overview.md




<!--HONumber=Dec16_HO1-->


