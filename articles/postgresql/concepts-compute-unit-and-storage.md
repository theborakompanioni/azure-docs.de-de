---
title: "Erläuterungen zu Computeeinheiten in Azure-Datenbank für PostgreSQL | Microsoft-Dokumentation"
description: "Azure-Datenbank für PostgreSQL: In diesem Artikel werden das Konzept der Computeeinheiten und die Folgen bei Überschreitung der maximalen Anzahl von Computeeinheiten durch eine Workload erläutert."
services: postgresql
author: kamathsun
ms.author: sukamat
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.topic: article
ms.date: 05/23/2017
ms.translationtype: HT
ms.sourcegitcommit: f76de4efe3d4328a37f86f986287092c808ea537
ms.openlocfilehash: e480671e6550b305c49442becaed3c0b42ce5cb3
ms.contentlocale: de-de
ms.lasthandoff: 07/10/2017

---
# <a name="explaining-compute-units-in-azure-database-for-postgresql"></a>Erläuterungen zu Computeeinheiten in Azure-Datenbank für PostgreSQL
In diesem Artikel werden das Konzept der Computeeinheiten und die Folgen bei Überschreitung der maximalen Anzahl von Computeeinheiten durch eine Workload erläutert.

## <a name="what-are-compute-units"></a>Was sind Compute-Einheiten?
Compute-Einheiten sind eine Maßeinheit für den CPU-Verarbeitungsdurchsatz, der für einen einzelnen Azure-Datenbank für PostgreSQL-Server garantiert zur Verfügung steht. Eine Compute-Einheit stellt eine kombinierte Maßeinheit für CPU- und Speicherressourcen dar. Im Allgemeinen entsprechen 50 Computeeinheiten einem halben Kern. 100 Computeeinheiten entsprechen einem Kern. 2000 Computeeinheiten entsprechen zwanzig Kernen, die als garantierter Verarbeitungsdurchsatz für Ihre Server zur Verfügung stehen.

Die Speichermenge pro Computeeinheit ist für die Tarife Basic und Standard optimiert. Eine Verdoppelung der Compute-Einheiten durch das Erhöhen der Leistungsebene entspricht der Verdoppelung des Ressourcensatzes, der für den jeweiligen Azure-Datenbank für PostgreSQL-Server verfügbar ist.

800 Computeeinheiten im Tarif Standard stellen beispielsweise 8-mal mehr CPU-Durchsatz und Arbeitsspeicher bereit als eine Konfiguration im Tarif Standard mit 100 Computeeinheiten. Während 100 Computeeinheiten im Tarif Standard denselben CPU-Durchsatz wie 100 Computeeinheiten im Tarif Basic bieten, beträgt die im Tarif Standard vorkonfigurierte Speichermenge das Doppelte der im Tarif Basic konfigurierten Speichermenge. Aus diesem Grund bietet der Tarif Standard eine bessere Workloadleistung und niedrigere Transaktionslatenzen als der Tarif Basic mit denselben Computeeinheiten.

## <a name="how-can-i-determine-the-number-of-compute-units-needed-for-my-workload"></a>Wie kann ich die Anzahl der benötigten Compute-Einheiten für meine Workload bestimmen?
Wenn Sie einen vorhandenen lokalen PostgreSQL-Server oder einen PostgreSQL-Server, der auf einem virtuellen Computer ausgeführt wird, migrieren möchten, können Sie die Anzahl der Computeeinheiten bestimmen, indem Sie die Anzahl der Kerne schätzen, die für den Verarbeitungsdurchsatz Ihrer Workload erforderlich ist. 

Wenn der vorhandene lokale oder virtuelle Server derzeit 4 Kerne nutzt (ohne CPU-Hyperthread), können Sie zunächst bei 400 Computeeinheiten für Ihren Azure-Datenbank für PostgreSQL-Server beginnen. Computeeinheiten können abhängig von Ihrer Workload dynamisch mit nahezu keinen Anwendungsausfallzeiten zentral hoch- oder herunterskaliert werden. 

Überwachen Sie entweder den Metrikgraph im Azure-Portal, oder schreiben Sie Befehle für die Azure-Befehlszeilenschnittstelle zum Messen der Computeeinheiten. Wichtige zu überwachende Metriken sind der Prozentsatz von Computeeinheiten und der Grenzwert für Computeeinheiten.

>[!IMPORTANT]
> Wenn die Speicher-IOPS nicht voll ausgelastet sind, sollten Sie auch die Nutzung der Computeeinheiten überwachen. Durch das Erhöhen der Computeeinheiten ist eventuell ein höherer E/A-Durchsatz möglich, da der Leistungsengpass aufgrund der begrenzten CPU- oder Arbeitsspeicherressourcen verringert wird.

## <a name="what-happens-when-i-hit-my-maximum-compute-units"></a>Was passiert, wenn die Obergrenze von Computeeinheiten erreicht ist?
Leistungsebenen werden kalibriert und gesteuert, um die erforderlichen Ressourcen zum Ausführen der Workload Ihrer Datenbank bis zu den maximalen Grenzwerten für den ausgewählten Tarif und die Leistungsebene bereitzustellen. 

Wenn die Workload die Grenzwerte für Computeeinheiten bzw. bereitgestellte IOPS erreicht, können Sie die Ressourcen auch weiterhin auf der maximal zulässigen Ebene nutzen. Es treten aber wahrscheinlich erhöhte Wartezeiten für Ihre Abfragen auf. Diese Grenzwerte führen nicht zu Fehlern, sondern nur zu einer Verlangsamung Ihrer Workload. Wenn die Verlangsamung aber zu schwerwiegend ist, tritt ein Timeout für die Abfragen auf. 

Wenn die Workload die Grenzwerte für die Anzahl von Verbindungen erreicht, werden explizite Fehler ausgelöst. Weitere Informationen zu Grenzwerten für Ressourcen finden Sie unter [Einschränkungen in Azure-Datenbank für PostgreSQL](concepts-limits.md).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Tarifen finden Sie unter [Azure-Datenbank für PostgreSQL – Tarife](./concepts-service-tiers.md).

