---
title: "Tarife in Azure-Datenbank für MySQL | Microsoft-Dokumentation"
description: "Tarife in Azure-Datenbank für MySQL"
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/23/2017
ms.translationtype: HT
ms.sourcegitcommit: 349fe8129b0f98b3ed43da5114b9d8882989c3b2
ms.openlocfilehash: d9ec4556d57ff1975a93d806237ad0c7416b9988
ms.contentlocale: de-de
ms.lasthandoff: 07/26/2017

---
# <a name="azure-database-for-mysql-options-and-performance-understand-whats-available-in-each-pricing-tier"></a>Optionen und Leistung von Azure-Datenbank für MySQL: Übersicht über die verfügbaren Funktionen in den einzelnen Tarifen
Wenn Sie einen Azure-Datenbank für MySQL-Server erstellen, treffen Sie drei Hauptentscheidungen zur Konfiguration der Ressourcen, die diesem Server zugeordnet werden. Diese Entscheidungen beeinflussen die Leistung und Skalierung des Servers.
- Tarif
- Compute-Einheiten
- Speicher (GB)

Für jeden Tarif stehen je nach Anforderungen der Workload verschiedene Leistungsebenen (Compute-Einheiten) zur Auswahl. Bei höheren Leistungsebenen stehen für den Server zusätzliche Ressourcen zur Verfügung, um einen höheren Durchsatz zu ermöglichen. Sie können die Leistungsebene des Servers innerhalb eines Tarifs praktisch ohne Ausfallzeiten der Anwendung ändern.

> [!IMPORTANT]
> Solange der Dienst als Public Preview verfügbar ist, besteht keine garantierte Vereinbarung zum Servicelevel (SLA).

Sie können in einer Instanz von Azure-Datenbank für MySQL-Server eine oder mehrere Datenbanken verwenden. Sie können eine Einzeldatenbank pro Server erstellen, die alle Ressourcen nutzt, oder Sie können mehrere Datenbanken erstellen, um die Ressourcen gemeinsam zu verwenden. 

## <a name="choose-a-pricing-tier"></a>Auswählen eines Tarifs
In der Vorschauversion gibt es für Azure-Datenbank für MySQL zwei Tarife: Basic und Standard. Der Tarif Premium ist noch nicht verfügbar, wird aber bald bereitgestellt. 

In der folgenden Tabelle sind Beispiele für Tarife aufgeführt, die sich für unterschiedliche Anwendungsworkloads am besten eignen.

| Tarif | Zielworkloads |
| :----------- | :----------------|
| Basic | Diese Option ist am besten für kleine Workloads geeignet, die skalierbare Compute- und Speichereinheiten ohne IOPS-Garantie erfordern. Beispiele hierfür sind Server, die für die Entwicklung, für Tests oder für kleine, selten verwendete Anwendungen verwendet werden. |
| Standard | Die ideale Wahl für Cloudanwendungen, die eine IOPS-Garantie mit hohem Durchsatz benötigen. Hierzu zählen beispielsweise Web- oder Analyseanwendungen. |
| Premium | Am besten geeignet für Workloads, die für Transaktionen und E/A-Vorgänge kurze Wartezeiten benötigen. Sie bietet optimale Unterstützung für viele gleichzeitige Benutzer. Er gilt für Datenbanken, die unternehmenskritische Anwendungen unterstützen.<br />Der Tarif Premium ist in der Vorschauversion nicht verfügbar. |

Um eine Auswahl für einen Tarif zu treffen, müssen Sie zunächst festlegen, ob Ihre Workload eine IOPS-Garantie erfordert. Wenn dies der Fall ist, verwenden Sie den Tarif Standard.

| **Merkmale der Tarife** | **Basic** | **Standard** |
| :------------------------ | :-------- | :----------- |
| Max. Anzahl von Compute-Einheiten | 100 | 800 | 
| Max. Gesamtspeicher | 1 TB | 1 TB | 
| Garantie für Speicher-IOPS | N/V | Ja | 
| Max. Speicher-IOPS | N/V | 3,000 | 
| Aufbewahrungszeitraum von Datenbanksicherungen | 7 Tage | 35 Tage | 

Sie können den Tarif während der Vorschauphase nicht wechseln, nachdem der Server erstellt wurde. In Zukunft wird es jedoch möglich sein, Upgrades oder Downgrades zwischen den Tarifen für Server durchzuführen.

## <a name="understand-the-price"></a>Informationen zum Preis
Klicken Sie beim Erstellen einer neuen Azure-Datenbank für MySQL im [Azure-Portal](https://portal.azure.com/#create/Microsoft.MySQLServer) auf das Blatt **Tarif**, dann werden die monatlichen Kosten angezeigt, basierend auf den ausgewählten Optionen. Wenn Sie nicht über ein Azure-Abonnement verfügen, verwenden Sie den Azure-Preisrechner, um einen geschätzten Preis zu erhalten. Besuchen Sie die Website des [Azure-Preisrechners](https://azure.microsoft.com/pricing/calculator/), klicken Sie auf **Elemente hinzufügen**, erweitern Sie die Kategorie **Datenbanken**, und wählen Sie **Azure-Datenbank für MySQL** aus, um die Optionen anzupassen.

## <a name="choose-a-performance-level-compute-units"></a>Auswählen einer Leistungsebene (Computeeinheiten)
Nachdem Sie den Tarif für Ihren Azure-Datenbank für MySQL-Server ermittelt haben, können Sie die Leistungsebene durch Auswählen der Anzahl von Computeeinheiten festlegen. Die 200 und 400 Computeeinheiten im Tarif Standard sind häufig ein guter Ausgangspunkt für Anwendungen, die eine höhere Benutzerparallelität für ihre Web- oder Analyseworkloads erfordern. Sie werden bei Bedarf inkrementell angepasst. 

Computeeinheiten sind eine Maßeinheit für den CPU-Verarbeitungsdurchsatz, der für einen einzelnen Azure-Datenbank für MySQL-Server garantiert zur Verfügung steht. Eine Compute-Einheit stellt eine kombinierte Maßeinheit für CPU- und Speicherressourcen dar.  Weitere Informationen finden Sie unter [Erläuterungen zu Computeeinheiten](concepts-compute-unit-and-storage.md).

### <a name="basic-pricing-tier-performance-levels"></a>Leistungsebenen im Tarif Basic:

| **Leistungsstufe** | **50** | **100** |
| :-------------------- | :----- | :------ |
| Max. Anzahl von Compute-Einheiten | 50 | 100 |
| Inbegriffene Speichergröße | 50 GB | 50 GB |
| Max. Größe des Serverspeichers\* | 1 TB | 1 TB |

### <a name="standard-pricing-tier-performance-levels"></a>Leistungsebenen im Tarif Standard:

| **Leistungsstufe** | **100** | **200** | **400** | **800** |
| :-------------------- | :------ | :------ | :------ | :------ |
| Max. Anzahl von Compute-Einheiten | 100 | 200 | 400 | 800 |
| Inbegriffene Speichergröße und bereitgestellte IOPS | 125 GB,<br/> 375 IOPS | 125 GB,<br/> 375 IOPS | 125 GB,<br/> 375 IOPS | 125 GB,<br/> 375 IOPS |
| Max. Größe des Serverspeichers\* | 1 TB | 1 TB | 1 TB | 1 TB |
| Max. bereitgestellte Server-IOPS | 3.000 IOPS | 3.000 IOPS | 3.000 IOPS | 3.000 IOPS |
| Max. bereitgestellte Server-IOPS pro GB | 3 IOPS pro GB fest | 3 IOPS pro GB fest | 3 IOPS pro GB fest | 3 IOPS pro GB fest |

\* Die max. Größe des Serverspeichers bezieht sich auf die maximale Speichergröße, die für Ihren Server bereitgestellt wird.

## <a name="storage"></a>Speicher 
Die Speicherkonfiguration definiert die Speicherkapazität, die einer Azure-Datenbank für MySQL-Server zur Verfügung steht. Der vom Dienst verwendete Speicher umfasst die Datenbankdateien, Transaktionsprotokolle und die MySQL-Serverprotokolle. Berücksichtigen Sie die erforderliche Speichergröße für das Hosten Ihrer Datenbanken und die Leistungsanforderungen (IOPS), wenn Sie die Speicherkonfiguration auswählen.

In einigen Fällen ist bereits eine Mindestspeicherkapazität im Tarif enthalten. Dies wird in der Tabelle oben als „Inbegriffene Speichergröße“ bezeichnet. Zusätzliche Speicherkapazität kann beim Erstellen des Servers in Schritten von 125 GB bis zum maximal zulässigen Speicher hinzugefügt werden. Die zusätzliche Speicherkapazität kann unabhängig von den Computeeinheiten konfiguriert werden. Die Preise variieren basierend auf der Menge des ausgewählten Speichers.

Die IOPS-Konfiguration in jeder Leistungsebene bezieht sich auf den Tarif und die ausgewählte Speichergröße. Der Tarif Basic bietet keine IOPS-Garantie. Innerhalb des Tarifs Standard skalieren die IOPS proportional zur maximalen Speichergröße in einem festen Verhältnis von 3:1. Der inbegriffene Speicher von 125 GB garantiert 375 bereitgestellte IOPS, jeweils mit einer E/A-Größe von bis zu 256 KB. Sie können zusätzlichen Speicher bis 1 TB auswählen, um 3.000 bereitgestellte IOPS zu garantieren.

Überwachen Sie entweder den Metrikgraph im Azure-Portal, oder schreiben Sie Befehle für die Azure-Befehlszeilenschnittstelle zum Messen des Verbrauchs von Speicher und IOPS. Wichtige zu überwachende Metriken sind das Speicherlimit, der Speicherprozentsatz, der genutzte Speicher und der E/A-Prozentsatz.

>[!IMPORTANT]
> In der Vorschauversion wählen Sie die Menge des Speichers beim Erstellen des Servers aus. Das Ändern der Speichergröße auf einem vorhandenen Server wird noch nicht unterstützt. 

## <a name="scaling-a-server-up-or-down"></a>Zentrales Hoch- oder Herunterskalieren eines Servers
Sie wählen den Tarif und die Leistungsebene anfänglich beim Erstellen Ihrer Instanz von Azure-Datenbank für MySQL aus. Später können Sie die Computeeinheiten innerhalb des Tarifbereichs dynamisch zentral hoch- oder herunterskalieren. Schieben Sie im Azure-Portal die Computeeinheiten auf dem Tarifblatt des Servers, oder erstellen Sie ein Skript anhand des folgenden Beispiels: [Überwachen und Skalieren eines Azure-Datenbank für MySQL-Servers mithilfe der Azure-Befehlszeilenschnittstelle](scripts/sample-scale-server.md).

Das Skalieren der Computeeinheiten erfolgt unabhängig von der maximalen Speichergröße, die Sie ausgewählt haben.

Wenn Sie die Leistungsebene einer Datenbank ändern, wird im Hintergrund ein Replikat der ursprünglichen Datenbank mit der neuen Leistungsebene erstellt, und anschließend werden die Verbindungen auf dieses Replikat umgestellt. Während dieses Vorgangs gehen keine Daten verloren. Allerdings sind die Verbindungen zur Datenbank inaktiv, während kurz auf das Replikat umgestellt wird. Daher werden möglicherweise einige aktive Transaktionen zurückgesetzt. Dieses Zeitfenster variiert, ist aber durchschnittlich kleiner als vier Sekunden und in mehr als 99 % der Fälle kürzer als 30 Sekunden. Falls im Moment der Verbindungstrennung viele Transaktionen stattfinden, ist das Zeitfenster unter Umständen größer.

Die Dauer des gesamten zentralen Hochskalierungsvorgangs hängt sowohl von der Größe als auch vom Tarif des Servers vor und nach der Änderung ab. Beispielsweise sollte ein Server beim Ändern von Computeeinheiten in den oder aus dem Tarif Standard bzw. innerhalb dieses Tarifs in wenigen Minuten zentral hochskalieren. Die neuen Eigenschaften für den Server werden erst angewendet, wenn die Änderungen abgeschlossen sind.

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zu Computeeinheiten finden Sie unter [Erläuterungen zu Computeeinheiten](concepts-compute-unit-and-storage.md).
- Erfahren Sie etwas über das [Überwachen und Skalieren eines Azure-Datenbank für MySQL-Servers mithilfe der Azure-Befehlszeilenschnittstelle](scripts/sample-scale-server.md).

