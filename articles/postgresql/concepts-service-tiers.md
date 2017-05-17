---
title: "Diensttarife in Azure-Datenbank für PostgreSQL | Microsoft-Dokumentation"
description: "Diensttarife in Azure-Datenbank für PostgreSQL"
services: postgresql
author: kamathsun
ms.author: sukamat
manager: jhubbard
editor: jasonh
ms.assetid: 
ms.service: postgresql-database
ms.tgt_pltfrm: portal
ms.topic: article
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 23a93060697bc48084658bbbd895d13fd393b07b
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---
# <a name="azure-database-for-postgresql-options-and-performance-understand-whats-available-in-each-service-tier"></a>Optionen und Leistung von Azure-Datenbank für PostgreSQL: Überblick über die verfügbaren Funktionen in den einzelnen Diensttarifen

In der Vorschau bietet Azure-Datenbank für PostgreSQL nur die Diensttarife „Basic“ und „Standard“. „Premium“ ist noch nicht verfügbar.

Jeder Diensttarif enthält mehrere Leistungsstufen für unterschiedliche Arten von Workloadanforderungen. Bei höheren Leistungsstufen stehen zusätzliche Ressourcen zur Verfügung, um einen höheren Durchsatz zu ermöglichen. Sie können die Leistungsstufen innerhalb eines Diensttarifs dynamisch ohne Anwendungsausfallzeiten ändern.

In Zukunft wird es möglich sein, Upgrades oder Downgrades zwischen den Diensttarifen durchzuführen.

> [!IMPORTANT]
> Der Dienst ist derzeit als öffentliche Vorschau verfügbar und bietet daher noch keine Vereinbarung zum Servicelevel (SLA).

Sie können einen einzelnen Azure-Datenbank für PostgreSQL-Server mit dedizierten Ressourcen innerhalb eines Diensttarifs auf einer bestimmten Leistungsstufe erstellen. Anschließend können Sie beliebig viele Datenbanken auf dem Server erstellen, auf dem die Ressourcen gemeinsam von mehreren Datenbanken verwendet werden. Die verfügbaren Ressourcen für einen einzelnen PostgreSQL-Server werden in Form von Compute- und Speichereinheiten ausgedrückt. Weitere Informationen zu Compute- und Speichereinheiten finden Sie unter [Erläuterung von Compute- und Speichereinheiten](concepts-compute-unit-and-storage.md).

## <a name="choosing-a-service-tier"></a>Auswählen einer Dienstebene

In der folgenden Tabelle sind Beispiele für Dienstebenen aufgeführt, die sich für unterschiedliche Anwendungsworkloads am besten eignen.

| Dienstebene | Zielworkloads |
| :----------- | :----------------|
| Basic | Diese Option ist am besten für kleine Workloads geeignet, die skalierbare Compute- und Speichereinheiten ohne IOPS-Garantie erfordern. Beispiele hierfür sind Server, die für die Entwicklung, für Tests oder für kleine, selten verwendete Anwendungen verwendet werden. |
| Standard | Dies ist die optimale Option für Cloudanwendungen, die IOPS-Garantie mit der Möglichkeit, für einen hohen Durchsatz unabhängig auf eine höhere Anzahl von Computing- und Speichereinheiten zu skalieren, erfordern. Hierzu zählen beispielsweise Web- oder Analyseanwendungen. |
| Premium | Diese Option ist am besten für Workloads geeignet, die sehr kurze Latenzen für Transaktionen und E/A sowie hohe E/A und einen hohen Workloaddurchsatz erfordern. Sie bietet optimale Unterstützung für viele gleichzeitige Benutzer. Sie gilt für Datenbanken, die unternehmenskritische Anwendungen unterstützen.<br />Der Diensttarif „Premium“ ist nicht in der Vorschau verfügbar. |
| &nbsp; | &nbsp; |

Um eine Auswahl für einen Diensttarif zu treffen, müssen Sie zunächst bestimmen, ob Ihre Workload eine IOPS-Garantie erfordert. Ermitteln Sie dann die Funktionen, die mindestens erforderlich sind:

| **Dienstebenenfeatures** | **Basic** | **Standard** | **Premium** * |
| :------------------------ | :-------- | :----------- | :------------ |
| Max. Anzahl von Compute-Einheiten | 100 | 2000 | Nicht in der Vorschau verfügbar |
| Max. Gesamtspeicher | 1.050 GB | 10.000 GB | Nicht in der Vorschau verfügbar |
| Garantie für Speicher-IOPS | N/V | Ja | Nicht in der Vorschau verfügbar |
| Max. Speicher-IOPS | N/V | 30.000 | Nicht in der Vorschau verfügbar |
| Aufbewahrungszeitraum von Datenbanksicherungen | 7 Tage | 35 Tage | 35 Tage |
| &nbsp; | &nbsp; | &nbsp; | &nbsp; |

> [!NOTE]
> Der Diensttarif „Standard“ in der Vorschau unterstützt derzeit bis zu 800 Compute-Einheiten und max. 1000 GB Speicher.

Nachdem Sie den Mindestdiensttarif ermittelt haben, können Sie die Leistungsebene für den Azure-Datenbank für PostgreSQL-Server (Anzahl von Compute-Einheiten) bestimmen. Die 200 und 400 Compute-Einheiten im Tarif „Standard“ sind häufig ein guter Ausgangspunkt für Anwendungen, die eine höhere Benutzerparallelität für ihre Web- oder Analyseworkloads erfordern. 

Allerdings können Sie die Compute-Einheiten basierend auf den Workloadanforderungen unabhängig von den Speichereinheiten zentral hoch- bzw. herunterzuskalieren. Wenn die Workload eine Anpassung der Compute-Ressourcen erfordert, können Sie die Compute-Einheiten dynamisch erhöhen oder verringern. Wenn Ihre Workload mehr IOPS oder Speicher benötigt, können Sie auch den Speicher skalieren.

> [!NOTE]
> In der Vorschau unterstützen die Tarife „Basic“ und „Standard“ derzeit keine dynamische Skalierung von Speicher. Diese Funktion soll in Zukunft hinzugefügt werden.

> [!NOTE]
> Im Diensttarif „Standard“ werden IOPS in einem festgelegten Verhältnis von 3:1 proportional zur bereitgestellten Speichergröße skaliert. Der inbegriffene Speicher von 125 GB garantiert 375 bereitgestellte IOPS, jeweils mit einer E/A-Größe von bis zu 256 KB. Wenn Sie 1.000 GB bereitstellen, erhalten Sie 3.000 bereitgestellte IOPS. Sie müssen den Verbrauch der Compute-Einheiten der Server überwachen und zentral hochskalieren, um in vollem Umfang von den verfügbaren bereitgestellten IOPS zu profitieren.

## <a name="service-tiers-and-performance-levels"></a>Tarife und Leistungsebenen

Bei Azure-Datenbank für PostgreSQL gibt es mehrere Leistungsstufen in den einzelnen Diensttarifen. Sie können die Ebene, die Ihren Anforderungen an Workloads am besten entspricht, über eine der folgenden Quellen auswählen:

- [Azure-Portal](/azure/azure-portal-overview) unter [http://portal.azure.com](http://portal.azure.com)
- [Azure-CLI](quickstart-create-server-database-azure-cli.md)

Unabhängig von der Anzahl der gehosteten Datenbanken auf jedem PostgreSQL-Server erhält Ihre Datenbank einen garantierten Ressourcensatz, und die erwarteten Leistungsmerkmale Ihres Servers werden nicht beeinträchtigt.

Diensttarif „Basic“:

| **Leistungsstufe** | **50** | **100** |
| :-------------------- | :----- | :------ |
| Max. Anzahl von Compute-Einheiten | 50 | 100 |
| Inbegriffene Speichergröße | 50 GB | 50 GB |
| Max. Größe des Serverspeichers\* | 1.050 GB | 1.050 GB |
| Maximale Anzahl gleichzeitiger Anmeldungen | &nbsp; | &nbsp; |
| Max. Anzahl von Verbindungen | &nbsp; | &nbsp; |
| &nbsp; | &nbsp; | &nbsp; |

Diensttarif „Standard“:

| **Leistungsstufe** | **100** | **200** | **400** | **800** |
| :-------------------- | :------ | :------ | :------ | :------ |
| Max. Anzahl von Compute-Einheiten | 100 | 200 | 400 | 800 |
| Inbegriffene Speichergröße und bereitgestellte IOPS | 125 GB, 375 IOPS | &nbsp; | &nbsp; | &nbsp; |
| Max. Größe des Serverspeichers\* | 1 TB | &nbsp; | &nbsp; | &nbsp; |
| Max. bereitgestellte Server-IOPS | 3.000 IOPS | &nbsp; | &nbsp; | &nbsp; |
| Max. bereitgestellte Server-IOPS pro GB | 3 IOPS pro GB fest | &nbsp; | &nbsp; | &nbsp; |
| Maximale Anzahl gleichzeitiger Anmeldungen | &nbsp; | &nbsp; | &nbsp; | &nbsp; |
| Max. Anzahl von Verbindungen | &nbsp; | &nbsp; | &nbsp; | &nbsp; |
| &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; |

\* Die max. Größe des Serverspeichers bezieht sich auf die maximale Speichergröße, die für Ihren Server bereitgestellt wird.

## <a name="scaling-up-or-down-a-single-server"></a>Zentrales Hoch- oder Herunterskalieren eines einzigen Servers

Nach der Auswahl eines Diensttarifs und einer Leistungsebene können Sie den Server dynamisch und basierend auf den Workloadanforderungen zentral hoch- oder herunterskalieren. Wenn Sie zentral hoch- oder herunterskalieren müssen, können Sie den Tarif Ihrer Datenbank im Azure-Portal oder über die Azure CLI problemlos ändern.

Wenn Sie die Dienst- und/oder die Leistungsstufe einer Datenbank ändern, wird ein Replikat der ursprünglichen Datenbank mit der neuen Leistungsebene erstellt und anschließend die Verbindung auf dieses Replikat umgestellt. Während dieses Vorgangs gehen keine Daten verloren. Allerdings sind die Verbindungen zur Datenbank inaktiv, während kurz auf das Replikat umgestellt wird. Daher werden möglicherweise einige aktive Transaktionen zurückgesetzt. Dieses Zeitfenster variiert, ist aber durchschnittlich kleiner als vier Sekunden und in mehr als 99 % der Fälle kürzer als 30 Sekunden. Falls im Moment der Verbindungstrennung viele Transaktionen stattfinden, ist das Zeitfenster unter Umständen größer.

Die Dauer des gesamten zentralen Hochskalierungsvorgangs hängt sowohl von der Größe als auch vom Diensttarif des Servers vor und nach der Änderung ab. Beispielsweise sollte ein Server beim Ändern von Compute-Einheiten in den oder aus dem Diensttarif „Standard“ bzw. innerhalb dieses Diensttarifs innerhalb von wenigen Minuten zentral hochskalieren. Die neuen Eigenschaften für den Server werden erst angewendet, wenn die Änderungen abgeschlossen sind.

### <a name="documentation-about-the-steps-for-scaling-up-or-down"></a>Dokumentation zu den Schritten für das zentrale Hoch- oder Herunterskalieren

- [Verwalten eines einzelnen Servers im Azure-Portal](quickstart-create-server-database-portal.md)
- [Verwalten einer einzelnen Datenbank mit der Azure CLI](quickstart-create-server-database-azure-cli.md)

### <a name="details-about-scaling-up-or-down"></a>Details zum zentralen Hoch- oder Herunterskalieren

- Für ein Downgrade eines Servers sollten die Speichereinheiten des Servers kleiner als die im Zieldiensttarif maximal zulässige Größe sein.
- Die Angebote des Wiederherstellungsdienstes variieren für die verschiedenen Dienstebenen. Wenn Sie ein Downgrade durchführen, verlieren Sie eventuell die Möglichkeit einer Zeitpunktwiederherstellung, oder der Aufbewahrungszeitraum für Sicherungen verkürzt sich. Weitere Informationen finden Sie unter [Sichern und Wiederherstellen eines Azure-Datenbank für PostgreSQL-Servers mit dem Azure-Portal](howto-restore-server-portal.md).
- Die neuen Eigenschaften für den Server werden erst angewendet, wenn die Änderungen abgeschlossen sind.

