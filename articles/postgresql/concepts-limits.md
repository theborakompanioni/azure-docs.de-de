---
title: "Beschränkungen in Azure-Datenbank für PostgreSQL | Microsoft-Dokumentation"
description: "Beschreibung der Einschränkungen in Azure-Datenbank für PostgreSQL"
services: postgresql
author: kamathsun
ms.author: sukamat
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.custom: mvc
ms.topic: article
ms.date: 06/01/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: ef1e603ea7759af76db595d95171cdbe1c995598
ms.openlocfilehash: a4ddb9d4f672de6cf1ebe137bf06e5e2c8b7796f
ms.contentlocale: de-de
ms.lasthandoff: 06/16/2017

---
# <a name="limitations-in-azure-database-for-postgresql"></a>Einschränkungen in Azure-Datenbank für PostgreSQL
Der Azure-Datenbank für PostgreSQL-Dienst ist in der öffentlichen Vorschau verfügbar. In den folgenden Abschnitten werden die Kapazitäts- und funktionalen Beschränkungen im Datenbankdienst beschrieben.

## <a name="service-tier-maximums"></a>Maximalwerte der Diensttarife
Azure-Datenbank für PostgreSQL weist mehrere Diensttarife auf, aus denen Sie bei der Erstellung eines Servers wählen können. Weitere Informationen finden Sie unter [Überblick über die verfügbaren Funktionen in jedem Diensttarif](concepts-service-tiers.md).  

In jedem Diensttarif ist eine maximale Anzahl von Verbindungen, Compute-Einheiten und Speicherkapazitäten während der Dienstvorschau verfügbar. Es gelten folgende Maximalwerte: 

|                            |                   |
| :------------------------- | :---------------- |
| **Max. Anzahl von Verbindungen**        |                   |
| 50 Compute-Einheiten (Basic)     | 50 Verbindungen    |
| 100 Compute-Einheiten (Basic)    | 100 Verbindungen   |
| 100 Compute-Einheiten (Standard) | 200 Verbindungen   |
| 200 Compute-Einheiten (Standard) | 300 Verbindungen   |
| 400 Compute-Einheiten (Standard) | 400 Verbindungen   |
| 800 Compute-Einheiten (Standard) | 500 Verbindungen   |
| **Max. Anzahl von Compute-Einheiten**      |                   |
| Basic-Dienstebene         | 100 Compute-Einheiten |
| Standard-Dienstebene      | 800 Compute-Einheiten |
| **Max. Speicherkapazität**            |                   |
| Basic-Dienstebene         | 1 TB              |
| Standard-Dienstebene      | 1 TB              |

Wenn die max. Anzahl von Verbindungen erreicht wird, wird möglicherweise folgende Fehlermeldung angezeigt:
> SCHWERWIEGEND: Es sind bereits zu viele Clients vorhanden.

## <a name="preview-functional-limitations"></a>Funktionale Beschränkungen der Vorschau
### <a name="scale-operations"></a>Skalierungsvorgänge
1.  Die dynamische Skalierung von Servern über verschiedene Diensttarife hinweg, d.h. ein Wechsel zwischen den Diensttarifen „Basic“ und „Standard“, wird derzeit nicht unterstützt.
2.  Die dynamische bedarfsgesteuerte Steigerung des Speichers auf vorab erstellten Servern wird derzeit nicht unterstützt.
3.  Die Verringerung der Größe des Serverspeichers wird nicht unterstützt.

### <a name="server-version-upgrades"></a>Upgrades von Serverversionen
- Die automatisierte Migration zwischen Hauptversionen von Datenbankmodulen wird derzeit nicht unterstützt.

### <a name="subscription-management"></a>Abonnementverwaltung
- Die dynamische Verschiebung von vorab erstellten Servern zwischen Abonnement- und Ressourcengruppen wird derzeit nicht unterstützt.

### <a name="point-in-time-restore"></a>Point-in-Time-Wiederherstellung
1.  Die Wiederherstellung in anderen Diensttarifen und/oder Compute-Einheiten und Speichergrößen ist nicht zulässig.
2.  Die Wiederherstellung eines gelöschten Servers wird nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte
- Thema [Informationen zu den verfügbaren Funktionen in jedem Tarif](concepts-service-tiers.md) lesen
- Thema [Unterstützte PostgreSQL-Datenbankversionen ](concepts-supported-versions.md) lesen
- Thema [Sichern und Wiederherstellen eines Servers in Azure-Datenbank für PostgreSQL mithilfe des Azure-Portals](howto-restore-server-portal.md) durchgehen

