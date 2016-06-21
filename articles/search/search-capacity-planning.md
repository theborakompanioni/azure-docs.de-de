<properties
	pageTitle="Skalieren von Ressourcenebenen für Abfrage und Indizierung von Workloads in Azure Search | Microsoft Azure"
	description="Die Kapazitätsplanung in Azure Search basiert auf Kombinationen von Partitions- und Replikatcomputerressourcen, wobei jede Ressource in abrechenbaren Sucheinheiten abgerechnet wird."
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
	ms.date="06/03/2016"
	ms.author="heidist"/>

# Skalieren von Ressourcenebenen für Abfrage und Indizierung von Arbeitslasten in Azure Search

Nach dem [Auswählen einer SKU](search-sku-tier.md) und dem [Bereitstellen eines Suchdiensts](search-create-service-portal.md) können Sie optional Dienstressourcen konfigurieren.

In Azure Search wird einem Dienst zunächst eine Mindestmenge von Ressourcen (bestehend aus einer Partition und einem Replikat) zugeordnet. Bei Tarifen, die dies unterstützen, können Sie schrittweise die Verarbeitungsressourcen anpassen, indem Sie die Anzahl von Partitionen erhöhen, um mehr Speicher- und E/A-Kapazität zu erhalten, oder die Anzahl von Replikaten, um ein höheres Abfrageaufkommen zu bewältigen oder die Leistung zu verbessern. Ein einzelner Dienst muss über genügend Ressourcen verfügen, um sämtliche Workloads (Indizierung und Abfragen) bewältigen zu können. Workloads können nicht auf mehrere Dienste aufgeteilt werden.

Skalierungseinstellungen stehen zur Verfügung, wenn Sie einen abrechenbaren Dienst unter dem [Basic-Tarif](http://aka.ms/azuresearchbasic) oder unter einem der [Standard-Tarife](search-limits-quotas-capacity.md) bereitstellen. Bei abrechenbaren SKUs wird die Kapazität in Form von *Sucheinheiten* (Search Units, SUs) erworben, wobei jede Partition und jedes Replikat als einzelne Sucheinheit zählt. Bei Einhaltung der Obergrenzen werden weniger Sucheinheiten verwendet, und die Rechnung fällt entsprechend niedriger aus. Der Dienst wird so lange abgerechnet, wie er bereitgestellt ist. Wenn Sie einen Dienst vorübergehend nicht verwenden und eine Abrechnung vermeiden möchten, müssen Sie den Dienst löschen und später bei Bedarf neu erstellen.

Wir empfehlen, die Replikat- und Partitionszuordnung über das Portal anzupassen. Das Portal erzwingt Grenzwerte für zulässige Kombinationen, um die Obergrenzen nicht zu überschreiten:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, und wählen Sie den Suchdienst aus.
2. Öffnen Sie in den Einstellungen das Blatt „Skalieren“, und passen Sie mithilfe der Schieberegler die Anzahl von Partitionen und Replikaten an.

Allgemein gilt: Suchanwendungen benötigen mehr Replikate als Partitionen – insbesondere, wenn die Dienstvorgänge auf Abfrageworkloads ausgerichtet sind. Warum das so ist, erfahren Sie im [Abschnitt zu hoher Verfügbarkeit](#HA).

> [AZURE.NOTE] Bei bereits bereitgestellten Diensten ist kein direktes Upgrade auf eine höhere SKU möglich. In diesem Fall müssen Sie einen neuen Suchdienst unter dem neuen Tarif erstellen und Ihre Indizes neu laden. Informationen zur Dienstbereitstellung finden Sie unter [Erstellen eines Azure Search-Diensts im Portal](search-create-service-portal.md).

## Terminologie: Partitionen und Replikate

Ein Suchdienst basiert in erster Linie auf Partitionen und Replikaten.

**Partitionen** stellen Indexspeicher und E/A für Lese-/Schreibvorgänge (beispielsweise bei der Neuerstellung oder Aktualisierung eines Index) bereit.

**Replikate** sind Instanzen des Suchdiensts und dienen in erster Linie zum Lastenausgleich bei Abfragevorgängen. Jedes Replikat hostet jeweils eine Kopie eines Index. Wenn Sie über 12 Replikate verfügen, stehen für jeden im Dienst geladenen Index 12 Kopien zur Verfügung.

> [AZURE.NOTE] Welche Indizes auf einem Replikat ausgeführt werden, ist nicht direkt beeinflussbar. Eine Kopie der einzelnen Indizes jedes Replikats ist Teil der Dienstarchitektur.

<a id="HA"></a>
## Hohe Verfügbarkeit

Dienste können problemlos und relativ schnell zentral hochskaliert werden. Daher empfiehlt es sich im Allgemeinen, mit einer einzelnen Partition und bis zu zwei Replikaten zu beginnen und dann bei zunehmendem Abfragevolumen zentral hochzuskalieren, bis die im Rahmen der SKU maximal unterstützte Anzahl von Replikaten und Partitionen erreicht ist. Bei vielen Diensten unter dem Basic- oder S1-Tarif ist die Speicher- und E/A-Kapazität einer einzelnen Partition (mit 15 Millionen Dokumenten pro Partition) ausreichend.

Abfrageworkloads werden in erster Linie auf Replikaten ausgeführt. Wenn Sie einen höheren Durchsatz oder hohe Verfügbarkeit benötigen, sind wahrscheinlich zusätzliche Replikate erforderlich.

Allgemeine Empfehlungen für hohe Verfügbarkeit sind:

- 2 Replikate für hohe Verfügbarkeit von schreibgeschützten Workloads (Abfragen)
- 3 oder mehr Replikate für hohe Verfügbarkeit von Lese-/Schreibworkloads (Abfragen und Indizierung, wenn einzelne Dokumente hinzugefügt, aktualisiert oder gelöscht werden)

Vereinbarungen zum Servicelevel (Service Level Agreements, SLAs) für Azure Search sind auf Abfragevorgänge und auf Indexaktualisierungen (Hinzufügen, Aktualisieren oder Löschen von Dokumenten) ausgerichtet.

**Indexverfügbarkeit während einer Neuerstellung**

Die hohe Verfügbarkeit von Azure Search gilt für Abfragen und Indexaktualisierungen ohne Indexneuerstellung. Falls der Index neu erstellt werden muss (etwa, wenn Sie ein Feld hinzufügen oder löschen, einen Datentyp ändern oder ein Feld umbenennen), gehen Sie wie folgt vor: Löschen Sie den Index, erstellen Sie ihn neu, und laden Sie die Daten erneut.

Um die Verfügbarkeit des Index während einer Neuerstellung zu gewährleisten, muss in der Produktionsumgebung des gleichen Diensts bereits eine zweite Kopie des Index (mit einem anderen Namen) oder ein gleichnamiger Index für einen anderen Dienst vorhanden sein und der Code mit einer Umleitungs- oder Failoverlogik versehen werden.

## Notfallwiederherstellung

Derzeit steht kein integrierter Mechanismus für die Notfallwiederherstellung bereit. Das Hinzufügen von Partitionen oder Replikaten wäre die falsche Strategie, um die Zielsetzungen für eine Notfallwiederherstellung zu erfüllen. Der gängigste Ansatz ist, Redundanz auf Dienstebene durch Bereitstellung eines zweiten Suchdiensts in einer anderen Region hinzuzufügen. Die Umleitungs- oder Failoverlogik muss genau wie bei der Verfügbarkeit während der Indexneuerstellung in Ihrem Code bereitgestellt werden.

## Erhöhen der Abfrageleistung mit Replikaten

Eine Abfragelatenz deutet darauf hin, dass zusätzliche Replikate erforderlich sind. Im Allgemeinen besteht der erste Schritt zum Verbessern der Abfrageleistung im Hinzufügen weiterer Instanzen dieser Ressource. Beim Hinzufügen von Replikaten werden zusätzliche Kopien des Index online geschaltet, um größere Abfrageworkloads zu unterstützen und die Anforderungslast gleichmäßig auf mehrere Replikate zu verteilen.

Beachten Sie, dass wir keine festen Schätzungen für die Abfragen pro Sekunde (Queries Per Second, QPS) abgegeben können: Die Abfrageleistung kann je nach Komplexität der jeweiligen Abfrage und konkurrierenden Workloads stark variieren. Im Schnitt verarbeitet ein Replikat unter der Basic- oder S1-SKU etwa 15 Abfragen pro Sekunde, der Durchsatz kann jedoch abhängig von der Komplexität der Abfrage (Facettenabfragen sind komplexer) und der Netzwerklatenz etwas höher oder niedriger ausfallen. Darüber hinaus ist zu beachten, dass beim Hinzufügen von Replikaten zwar Skalierung und Leistung erhöht werden, das Endergebnis aber nicht streng linear ist: Das Hinzufügen von 3 Replikaten garantiert keinen dreifachen Durchsatz.

Weitere Informationen zu QPS (einschließlich Konzepte zur Schätzung der QPS für Ihre Workloads) finden Sie unter [Verwalten des Search-Dienstes](search-manage.md).

## Erhöhen der Indizierungsleistung mit Partitionen

Suchanwendungen, die Datenaktualisierung nahezu in Echtzeit erfordern, benötigen proportional mehr Partitionen als Replikate. Hinzufügen von Partitionen verteilt Lese-/ Schreibvorgänge über eine größere Anzahl von Computeressourcen. Außerdem erhalten Sie mehr Speicherplatz auf dem Datenträger zum Speichern zusätzlicher Indizes und Dokumente.

Größere Indizes erfordern eine längere Abfragezeit. Daher werden Sie feststellen, dass jede inkrementelle Zunahme an Partitionen einen kleineren, aber proportionalen Anstieg der Replikate erforderlich macht. Wie bereits erwähnt, hat die Komplexität der Abfragen und das Abfragevolumen darauf Einfluss, wie schnell die Abfrage ausgeführt wird.

## Basic-Tarif: Partitions- und Replikatskombinationen

Ein Basic-Dienst kann genau eine Partition und bis zu drei Replikate besitzen. Die Obergrenze liegt bei drei Sucheinheiten. Nur die Replikate können angepasst werden. Für hohe Verfügbarkeit bei Abfragen sind wie bereits erwähnt mindestens zwei Replikate erforderlich.

<a id="chart"></a>
## Standard-Tarife: Partitions- und Replikatskombinationen

Diese Tabelle enthält die Sucheinheiten (Search Units, SUs), die für die Kombinationen aus Replikaten und Partitionen erforderlich sind. Dabei gilt ein Grenzwert von 36 Sucheinheiten (Basic- und S3 HD-Tarife ausgenommen).

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

> [AZURE.NOTE] Die Anzahl der Replikate und Partitionen muss gleichmäßig in 12 unterteilt werden können (d. h. 1, 2, 3, 4, 6, 12). Der Grund: Azure Search unterteilt jeden Index vorab in 12 Shards, damit er gleichmäßig auf alle Partitionen verteilt werden kann. Wenn Ihr Dienst z. B. drei Partitionen aufweist und Sie einen neuen Index erstellen, enthält jede Partition 4 Shards des Indexes. Die Form, in der Azure Search Shards eines Indexes erstellt, ist ein Implementierungsdetail, dass sich bei zukünftigen Versionen ändern kann. Auch wenn die Anzahl heute 12 beträgt, sollten Sie nicht davon ausgehen, das dies auch in Zukunft immer so ist.

## S3 High Density: Partitions- und Replikatskombinationen

S3 HD verfügt über eine einzelne Partition und bis zu 12 Replikate. Die Obergrenze liegt bei 12 Sucheinheiten. Nur die Replikate können angepasst werden.

## Berechnen von Sucheinheiten für bestimmte Ressourcenkombinationen: R X P = SU

Die Formel zum Berechnen der Anzahl der benötigten SUs lautet: Replikate multipliziert mit Partitionen. 3 Replikate multipliziert mit 3 Partitionen werden z. B. als 9 Sucheinheiten abgerechnet.

Beide Tarife beginnen mit einem Replikat und einer Partition, die als eine Sucheinheit (SU) gezählt werden. Dies ist die einzige Instanz, in der ein Replikat und eine Partition als eine einzelne Sucheinheit gerechnet werden. Jede zusätzliche Ressource, sei es ein Replikat oder eine Partition, wird als eigene SU gezählt.

Die Kosten pro SU werden vom Tarif bestimmt. Die Kosten pro SU für den Basic-Tarif sind niedriger als für den Standard-Tarif. Die Preise für die einzelnen Tarife finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/search/).

<!---HONumber=AcomDC_0608_2016-->