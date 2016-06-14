<properties
	pageTitle="Skalieren von Ressourcenebenen für Abfrage und Indizierung von Workloads in Azure Search | Microsoft Azure"
	description="Kapazitätsplanung in Azure Search basiert auf Kombinationen von Partitions- und Replikatscomputerressourcen, wobei jede Ressource in abrechenbaren Sucheinheiten abgerechnet wird."
	services="search"
	documentationCenter=""
	authors="HeidiSteen"
	manager="paulettm"
	editor=""
    tags="azure-portal"/>

<tags
	ms.service="search"
	ms.devlang="NA"
	ms.workload="search"
	ms.topic="article"
	ms.tgt_pltfrm="na"
	ms.date="05/25/2016"
	ms.author="heidist"/>

# Skalieren von Ressourcenebenen für Abfrage und Indizierung von Arbeitslasten in Azure Search

In Azure Search können Sie inkrementell die Kapazität der spezifischen Verarbeitungsressourcen anpassen, indem Sie Partitionen erhöhen, wenn Sie mehr Speicher- und E/A-Kapazität oder Replikate für verbesserte Abfrage- und Indizierungsleistung benötigen.

Skalierbarkeit wird verfügbar, wenn Sie einen Dienst zum [Basic-Tarif](http://aka.ms/azuresearchbasic) oder zu einem der [Standard-Tarife](search-limits-quotas-capacity.md) bereitstellen.

Bei allen abrechenbaren Tarifen wird die Kapazität in *Sucheinheit* (SU)-Schritten gekauft, wobei jede Partition und jedes Replikat als jeweils eine „SU“ zählen.

- Basic bietet bis zu 3 SU pro Dienst.
- Standard bietet bis zu 36 SU pro Dienst.

Sie müssen eine Kombination aus Partitionen und Replikaten auswählen, die unterhalb des Grenzwerts für den Tarif bleibt. Wenn Sie das Portal verwenden, um zentral hochzuskalieren, erzwingt das Portal Beschränkungen für zulässige Kombinationen.

Im Allgemeinen gilt, dass Suchanwendungen mehr Replikate benötigen als Partitionen. Im nächsten Abschnitt, [Ressourcenbereitstellung für hohe Verfügbarkeit](#HA), wird erklärt, warum.

<a id="HA"></a>
## Ressourcenbereitstellung für hohe Verfügbarkeit

Da ein Hochskalieren einfach und relativ schnell durchzuführen ist, wird im Allgemeinen empfohlen, mit einer Partition und einem oder zwei Replikaten zu beginnen und dann bei steigenden Abfragevolumen hochzuskalieren. Bei vielen Bereitstellungen bietet eine Partition ausreichend Speicher und E/A (bei 15 Millionen Dokumenten pro Partition).

Abfrageworkloads werden jedoch primär auf Replikaten ausgeführt. Wenn Sie einen höheren Durchsatz oder hohe Verfügbarkeit benötigen, können zusätzliche Replikate erforderlich sein.

Allgemeine Empfehlungen für hohe Verfügbarkeit sind:

- 2 Replikate für hohe Verfügbarkeit von schreibgeschützten Workloads (Abfragen)
- 3 oder mehr Replikate für hohe Verfügbarkeit von Lese-/ Schreibworkloads (Abfragen und Indizierung)

## Notfallwiederherstellung

Derzeit steht kein integrierter Mechanismus für die Notfallwiederherstellung bereit. Das Hinzufügen von Partitionen oder Replikaten wäre die falsche Strategie, um die Zielsetzungen für eine Notfallwiederherstellung zu erfüllen. Der gängigste Ansatz ist, Redundanz auf Dienstebene durch Bereitstellung eines zweiten Suchdiensts in einer anderen Region hinzuzufügen.

> [AZURE.NOTE] Denken Sie daran, dass Vereinbarungen zu Servicelevel und Skalierbarkeit Features des Basis- und Standarddiensts sind. Der kostenlose Dienst wird auf fester Ressourcenebene angeboten, wobei Replikate und Partitionen von mehreren Abonnenten gemeinsam genutzt werden. Wenn Sie mit dem kostenlosen Dienst begonnen haben und jetzt ein Upgrade durchführen möchten, müssen Sie einen neuen Azure Search-Dienst auf Basis- oder Standardebene erstellen und dann Indizes und Daten erneut in den neuen Dienst laden. Anweisungen zur Dienstbereitstellung finden Sie unter [Erstellen eines Azure Search-Diensts im Portal](search-create-service-portal.md).

## Terminologie: Partitionen und Replikate

**Partitionen** bieten Speicher und E/A. Ein einzelner Search-Dienst kann maximal 12 Partitionen umfassen. Jede Partition weist einen festen Grenzwert von 15 Millionen Dokumenten oder 25 GB Speicher auf, je nachdem, was zuerst erreicht wird. Wenn Sie Partitionen hinzufügen, können mehr Dokumente im Search-Dienst geladen werden. Ein Dienst mit einer einzelnen Partition, in dem anfänglich bis zu 25 GB Daten gespeichert werden, kann beispielsweise 50 GB speichern, wenn Sie eine zweite Partition zum Dienst hinzufügen.

**Replikate** sind Kopien der Suchmaschine. Ein einzelner Search-Dienst kann maximal zwölf Replikate aufweisen. Sie benötigen mindestens 2 Replikate für Leseverfügbarkeit (Abfrage) und mindestens 3 Replikate für Lese-/Schreibverfügbarkeit (Abfrage, Indizierung).

## Erhöhen der Abfrageleistung mit Replikaten

Die Abfragewartezeit ist ein Indikator, dass möglicherweise zusätzliche Replikate erforderlich sind. Im Allgemeinen besteht der erste Schritt zum Verbessern der Abfrageleistung im Hinzufügen weiterer Replikate.

Für jedes Replikat wird eine Kopie jedes Indexes ausgeführt. Beim Hinzufügen von Replikaten werden zusätzliche Kopien des Indexes online gestellt, um höhere Abfrageworkloads zu unterstützen und die Anforderungslast gleichmäßig auf mehrere Replikate zu verteilen. Wenn Sie über mehrere Indizes (z. B. 6) und 3 Replikate verfügen, weist jedes Replikat eine Kopie aller 6 Indizes auf.

Beachten Sie, dass keine festen Schätzungen für die Abfragen pro Sekunde (QPS) gegeben werden: Abfrageleistung kann je nach Komplexität der jeweiligen Abfrage und konkurrierenden Workloads stark variieren. Durchschnittlich kann ein Replikat etwa 15 QPS bedienen, doch ist der Durchsatz je nach Komplexität der Abfrage (Facettenabfragen sind komplexer) und Netzwerklatenz etwas höher oder niedriger. Darüber hinaus ist zu beachten, dass beim Hinzufügen von Replikaten zwar Skalierung und Leistung erhöht werden, das Endergebnis aber nicht streng linear ist: Das Hinzufügen von 3 Replikaten garantiert keinen dreifachen Durchsatz.

Weitere Informationen zu QPS, einschließlich der Ansätze für die Schätzung der QPS für Ihre Workloads, finden Sie unter [Verwalten des Search-Dienstes](search-manage.md).

## Erhöhen der Indizierungsleistung mit Partitionen

Suchanwendungen, die Datenaktualisierung nahezu in Echtzeit erfordern, benötigen proportional mehr Partitionen als Replikate. Hinzufügen von Partitionen verteilt Lese-/ Schreibvorgänge über eine größere Anzahl von Computeressourcen. Außerdem erhalten Sie mehr Speicherplatz auf dem Datenträger zum Speichern zusätzlicher Indizes und Dokumente.

Größere Indizes erfordern eine längere Abfragezeit. Daher werden Sie feststellen, dass jede inkrementelle Zunahme an Partitionen einen kleineren, aber proportionalen Anstieg der Replikate erforderlich macht. Wie bereits erwähnt, hat die Komplexität der Abfragen und das Abfragevolumen darauf Einfluss, wie schnell die Abfrage ausgeführt wird.

## Basic-Tarif: Partitions- und Replikatskombinationen

Ein Basic-Dienst kann aus 1 Partition und bis zu 3 Replikaten bestehen, für einen maximalen Grenzwert von 3 SUs.

<a id="chart"></a>
## Standard-Tarif: Partitions- und Replikatskombinationen

Diese Tabelle enthält die Sucheinheiten, die für eine Unterstützung der Kombinationen aus Replikaten und Partitionen erforderlich sind. Dabei gilt ein Grenzwert von 36 Sucheinheiten (SU).

- |- |- |- |- |- |- |
---|----|---|---|---|---|---|
**12 Replikate**|12 SU|24 SU|36 SU|N/V|–|N/V|
**6 Replikate**|6 SU|12 SU|18 SU|24 SU|36 SU|N/V|
**5 Replikate**|5 SU|10 SU|15 SU|20 SU|30 SU|N/V|
**4 Replikate**|4 SU|8 SU<|12 SU|16 SU|24 SU|N/|
**3 Replikate**|3 SU|6 SU|9 SU|12 SU|18 SU|36 SU|
**2 Replikate**|2 SU|4 SU|6 SU|8 SU|12 SU|24 SU|
**1 Replikat:**|1 SU|2 SU|3 SU|4 SU|6 SU|12 SU|
N/V|**1 Partition**|**2 Partitionen**|**3 Partitionen**<|**4 Partitionen**|**6 Partitionen**|**12 Partitionen**|


Sucheinheiten, Preise und Kapazität werden auf der Azure-Website ausführlich erläutert. Weitere Informationen finden Sie unter [Preise](https://azure.microsoft.com/pricing/details/search/).

> [AZURE.NOTE] Die Anzahl der Replikate und Partitionen muss gleichmäßig in 12 unterteilt werden können (d. h. 1, 2, 3, 4, 6, 12). Der Grund dafür ist, dass Azure Search jeden Index vorab in 12 Shards unterteilt, damit er auf Partitionen verteilt werden kann. Wenn Ihr Dienst z. B. drei Partitionen aufweist und Sie einen neuen Index erstellen, enthält jede Partition 4 Shards des Indexes. Die Form, in der Azure Search Shards eines Indexes erstellt, ist ein Implementierungsdetail, dass sich bei zukünftigen Versionen ändern kann. Auch wenn die Anzahl heute 12 beträgt, sollten Sie nicht davon ausgehen, das dies auch in Zukunft immer so ist.

## Berechnen von Sucheinheiten für bestimmte Ressourcenkombinationen: R X P = SU

Die Formel zum Berechnen der Anzahl der benötigten SUs lautet: Replikate multipliziert mit Partitionen. 3 Replikate multipliziert mit 3 Partitionen werden z. B. als 9 Sucheinheiten abgerechnet.

Beide Tarife beginnen mit einem Replikat und einer Partition, die als eine Sucheinheit (SU) gezählt werden. Dies ist die einzige Instanz, in der ein Replikat und eine Partition als eine einzelne Sucheinheit gerechnet werden. Jede zusätzliche Ressource, sei es ein Replikat oder eine Partition, wird als eigene SU gezählt.

Die Kosten pro SU werden vom Tarif bestimmt. Die Kosten pro SU für den Basic-Tarif sind niedriger als für den Standard-Tarif. Die Preise für jeden Tarif finden Sie in den [Preisdetails](https://azure.microsoft.com/pricing/details/search/).

<!---HONumber=AcomDC_0601_2016-->