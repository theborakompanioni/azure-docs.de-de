---
title: "Erläuterung der Compute- und -Speichereinheiten für Azure-Datenbank für PostgreSQL | Microsoft-Dokumentation"
description: "Azure-Datenbank für PostgreSQL: Erläuterung der Compute- und Speichereinheiten und der Folgen bei Überschreitung der maximalen Anzahl von Compute- oder Speichereinheiten"
services: postgresql
author: 
ms.author: 
manager: jhubbard
editor: jasonh
ms.assetid: 
ms.service: postgresql - database
ms.tgt_pltfrm: portal
ms.topic: article
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 0cf1f0cf3d93dac49e75f558dcba8af002a492da
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---
# <a name="explaining-compute-unit-and-storage-unit"></a>Erläuterung der Compute- und Speichereinheit
In diesem Artikel werden die Konzepte der Compute- und Speichereinheiten und die Folgen bei Überschreitung der maximalen Anzahl von Compute- oder Speichereinheiten erläutert.

## <a name="what-are-compute-units"></a>Was sind Compute-Einheiten?
Compute-Einheiten sind eine Maßeinheit für den CPU-Verarbeitungsdurchsatz, der für einen einzelnen Azure-Datenbank für PostgreSQL-Server garantiert zur Verfügung steht. Eine Compute-Einheit stellt eine kombinierte Maßeinheit für CPU- und Speicherressourcen dar. Im Allgemeinen entsprechen 50 Compute-Einheiten einem halben Kern, 100 Compute-Einheiten einem Kern und 2.000 Compute-Einheiten 20 Kernen an garantiertem Verarbeitungsdurchsatz, der für Ihren Server zur Verfügung steht. 

Die Speichermenge pro Compute-Einheit ist für die Diensttarife „Basic“, „Standard“ und „Premium“ optimiert. Eine Verdoppelung der Compute-Einheiten durch das Erhöhen der Leistungsebene entspricht der Verdoppelung des Ressourcensatzes, der für den jeweiligen Azure-Datenbank für PostgreSQL-Server verfügbar ist. 

2.000 Compute-Einheiten im Tarif „Standard“ stellen beispielsweise 20-mal mehr CPU-Durchsatz und Arbeitsspeicher bereit als eine „Standard“-Konfiguration mit 100 Compute-Einheiten. Während 100 Compute-Einheiten im Tarif „Standard“ denselben CPU-Durchsatz wie 100 Compute-Einheiten im Tarif „Basic“ bieten, beträgt die im „Standard“-Tarif vorkonfigurierte Speichermenge das Doppelte der für „Basic“ konfigurierten Speichermenge und bietet somit eine bessere Workloadleistung und niedrigere Transaktionslatenzen.

>[!IMPORTANT]
>Bei vorhersagbarem Workloadleistungsdurchsatz und hoher Anzahl an gleichzeitigen Benutzern wird ausdrücklich empfohlen, den Diensttarif „Standard“ auszuwählen.

Sie können die [Diensttarife](concepts-service-tiers.md) zu einem beliebigen Zeitpunkt ändern, und zwar mit nahezu keinen Anwendungsausfallzeiten. Für zahlreiche Unternehmen und Apps bietet die Fähigkeit, beliebig viele Datenbanken in jedem einzelnen Azure-Datenbank für PostgreSQL-Server zu erstellen und die Leistung bei Bedarf hoch- oder herunterzuskalieren, die erforderliche Flexibilität bei der Verwaltung der Kosten.

>[!IMPORTANT]
>Derzeit wird das Hoch- bzw. Herunterskalieren der Leistungsstufen innerhalb eines Diensttarifs unterstützt. Beispielsweise können Sie innerhalb des „Standard“-Tarifs 100 auf 400 Compute-Einheiten zentral hochskalieren. Ebenso können Sie innerhalb des „Standard“-Tarifs 400 auf 100 Compute-Einheiten zentral herunterskalieren. Die Funktion zum zentralen Hoch- oder Herunterskalieren zwischen verschiedenen Diensttarifen, z.B. zwischen den Tarifen „Basic“ und „Standard“, ist zukünftig verfügbar.

## <a name="what-are-storage-units"></a>Was sind Speichereinheiten?
Speichereinheiten sind eine Maßeinheit für die bereitgestellte Speicherkapazität, die für einen einzelnen Azure-Datenbank für PostgreSQL-Server garantiert verfügbar ist. Jeder Diensttarif verfügt über eine festgelegte Menge an bereitgestelltem Speicher, der im Preis des ausgewählten Diensttarifs inbegriffen ist.

Optional können Speichereinheiten in Schritten von 125 GB bis zur maximal zulässigen Speichermenge, wie in der folgenden Tabelle beschrieben, unabhängig hochskaliert werden.
| **Dienstebenenfeatures** | **Basic** | **Standard** | **Premium\*** |
|---------------------------|-----------|--------------|---------------|
| Inbegriffene Speichereinheiten | 50 GB | 125 GB | 250 GB |
| Max. Gesamtspeicher | 1.050 GB | 10.000 GB | 4.000 GB |
| Garantie für Speicher-IOPS | N/V | Ja | Ja |
| Max. Speicher-IOPS | N/V | 30.000 | 40.000 |
Die Diensttarife „Standard“ und „Premium“ bieten ebenfalls eine Garantie für bereitgestellte IOPS. Die Menge der verfügbaren bereitgestellten IOPS hängt vom Diensttarif und der konfigurierten Speicherkapazität ab. Bei Servern, die im Tarif „Standard“ bereitgestellt werden, sind die IOPS auf die 3-fache Menge des bereitgestellten Speichers festgelegt. 

Wenn Sie beispielsweise über einen bereitgestellten Speicher von 125 GB verfügen, stehen 375 bereitgestellte IOPS für Ihren Server zur Verfügung. Der Tarif „Premium“ weist sehr niedrige Latenzen und einen hohen IOPS-Speicher auf. Bei Servern, die im Tarif „Premium“ bereitgestellt werden, können die bereitgestellten niedrigen Latenz-IOPS um das 5- bis 10-Fache des bereitgestellten Speichers skaliert werden.
>[!IMPORTANT]
>Wenn bei Ihrer Workload Engpässe durch konfigurierte Compute-Einheiten auftreten, können Sie möglicherweise nicht in vollem Umfang die verfügbaren bereitgestellten IOPS erreichen. Wenn Sie die verfügbaren bereitgestellten IOPS nicht vollständig ausnutzen können, wird empfohlen, Compute-Einheiten zu überwachen und eine Skalierung der Compute-Einheiten in Erwägung zu ziehen.

Die Skalierung der Speichereinheiten durch schrittweises Erhöhen des bereitgestellten Speichers entspricht der proportionalen Erhöhung der bereitgestellten IOPS für die Tarife „Standard“ und „Premium“.

>[!IMPORTANT]
>Der Tarif „Basic“ bietet keine IOPS-Garantie.

## <a name="how-can-i-determine-the-number-of-compute-units-needed-for-my-workload"></a>Wie kann ich die Anzahl der benötigten Compute-Einheiten für meine Workload bestimmen?
Wenn Sie ein vorhandenes lokales oder PostgreSQL-System, das auf einem virtuellen Computer ausgeführt werden soll, migrieren möchten, können Sie die Anzahl der Compute-Einheiten bestimmen, indem Sie die Anzahl der Kerne schätzen, die für den Verarbeitungsdurchsatz Ihrer Workload erforderlich ist. 

Wenn der lokale oder virtuelle Computer derzeit 4 Kerne nutzt (ausgenommen des CPU-Hyperthread), können Sie zunächst 400 Compute-Einheiten für Azure-Datenbank für PostgreSQL konfigurieren. Compute-Einheiten können abhängig von Ihrer Workload dynamisch mit nahezu keinen Anwendungsausfallzeiten zentral hoch- oder herunterskaliert werden. Des Weiteren können Sie den Verbrauch der Compute-Einheiten über das Portal oder die CLI überwachen, um die Ressourcen für den PostgreSQL-Server richtig zu dimensionieren.

## <a name="how-can-i-determine-the-number-of-storage-units-needed-for-my-workload"></a>Wie kann ich die Anzahl der benötigten Speichereinheiten für meine Workload bestimmen?
Sie können die erforderliche Anzahl der Speichereinheiten schätzen, indem Sie zuerst die benötigte Speicherkapazität ermitteln. In den Tarifen „Basic“, „Standard“ und „Premium“ ist Speicher inbegriffen, der in der SKU berücksichtigt wird. 

Der zweitwichtigste Faktor ist die Bestimmung der erforderlichen IOPS. Im Tarif „Basic“ wird variable IOPS ohne Garantie zur Verfügung gestellt. Im Tarif „Standard“ wird ein festgelegtes Verhältnis von drei IOPS pro GB an bereitgestelltem Speicher skaliert und IOPS-Garantie geboten. Den Verbrauch der bereitgestellten IOPS können Sie auch über das Portal oder die CLI überwachen.

>[!IMPORTANT]
>Sobald Speichereinheiten bereitgestellt wurden, können sie nicht dynamisch herunterskaliert werden.

## <a name="what-happens-when-i-hit-my-maximum-compute-units-andor-storage-units"></a>Was geschieht, wenn ich die maximale Anzahl an Compute- und/oder Speichereinheiten erreiche?
Leistungsstufen werden kalibriert und gesteuert, um die erforderlichen Ressourcen zum Ausführen der Workload Ihrer Datenbank bis zu den maximalen Grenzwerten bereitzustellen, die für die ausgewählte Dienstebene/Leistungsstufe zulässig sind. 

Wenn die Workload die Grenzwerte für Compute-Einheiten bzw. bereitgestellte IOPS erreicht, erhalten Sie die Ressourcen auch weiterhin auf der maximal zulässigen Ebene. Es treten aber wahrscheinlich erhöhte Wartezeiten für Ihre Abfragen auf. Diese Höchstgrenzen führen nicht zu Fehlern, sondern nur zu einer Verlangsamung Ihrer Workload. Wenn die Verlangsamung aber zu schwerwiegend ist, tritt ein Timeout für die Abfragen auf. 

Wenn Sie die Grenzwerte für maximal zulässige Verbindungen erreichen, werden explizite Fehler angezeigt. Weitere Informationen zur Beschränkung der Ressourcen finden Sie unter [Ressourcenbeschränkungen für Azure-Datenbank für PostgreSQL](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits). <Need to write about the behavior if a user reaches the storage capacity limits>

## <a name="next-steps"></a>Nächste Schritte
- Informationen zu den verfügbaren Compute- und Speichereinheiten für die einzelnen Server finden Sie unter [Diensttarife in Azure-Datenbank für PostgreSQL](./concepts-service-tiers.md).


