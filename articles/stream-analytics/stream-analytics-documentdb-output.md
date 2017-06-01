---
title: "JSON-Ausgabe für Stream Analytics | Microsoft Docs"
description: "Erfahren Sie, wie Stream Analytics für die JSON-Ausgabe auf Azure Cosmos DB ausgerichtet werden kann, um eine Datenarchivierung und Abfragen unstrukturierter JSON-Daten mit geringer Latenz zu ermöglichen."
keywords: JSON-Ausgabe
documentationcenter: 
services: stream-analytics,documentdb
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 5d2a61a6-0dbf-4f1b-80af-60a80eb25dd1
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.translationtype: Human Translation
ms.sourcegitcommit: a643f139be40b9b11f865d528622bafbe7dec939
ms.openlocfilehash: cb85dff7f8bf8a8715aaa9ecd02da59b9108915c
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017


---
# <a name="target-azure-cosmos-db-for-json-output-from-stream-analytics"></a>Ausrichten der JSON-Ausgabe von Stream Analytics auf Azure Cosmos DB
Stream Analytics kann für die JSON-Ausgabe auf [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) ausgerichtet werden, was eine Datenarchivierung und Abfragen unstrukturierter JSON-Daten mit geringer Latenz ermöglicht. In diesem Dokument werden einige bewährte Implementierungsmethoden für diese Konfiguration behandelt.

Falls Sie noch nicht mit Cosmos DB vertraut sind, sehen Sie sich zum Einstieg den [Lernpfad für Azure Cosmos DB](https://azure.microsoft.com/documentation/learning-paths/documentdb/) an.

## <a name="basics-of-cosmos-db-as-an-output-target"></a>Grundlagen von Cosmos DB als Ausgabeziel
Die Azure Cosmos DB-Ausgabe in Stream Analytics ermöglicht das Schreiben der Ergebnisse Ihrer Datenstromverarbeitung als JSON-Ausgabe in Ihre Cosmos DB-Sammlungen. Stream Analytics erstellt keine Sammlungen in Ihrer Datenbank, sondern fordert deren vorherige Erstellung an. Dies ist so, damit die Abrechnungskosten von Cosmos DB-Sammlungen für Sie transparent bleiben und Sie die Leistung, Konsistenz und Kapazität Ihrer Sammlungen direkt mithilfe der [Cosmos DB-APIs](https://msdn.microsoft.com/library/azure/dn781481.aspx)optimieren können. Wir empfehlen den Einsatz einer Cosmos DB-Datenbank pro Streamingauftrag, um Ihre Sammlungen für einen Streamingauftrag logisch zu trennen.

Nachstehen sind einige der Cosmos DB-Sammlungsoptionen beschrieben.

## <a name="tune-consistency-availability-and-latency"></a>Optimieren von Konsistenz, Verfügbarkeit und Latenz
Entsprechend Ihren Anwendungsanforderungen lässt Cosmos DB das Optimieren der Datenbank und Sammlungen zu und wählt Kompromisse zwischen Konsistenz, Verfügbarkeit und Latenz. Abhängig von den Lesekonsistenzebenen, die Ihr Szenario hinsichtlich Lese- und Schreiblatenz benötigt, können Sie in Ihrem Datenbankkonto eine Konsistenzebene wählen. Außerdem aktiviert Cosmos DB für jeden CRUD-Vorgang in Ihrer Sammlung die synchrone Indizierung. Dies ist eine andere nützliche Option zum Steuern der Schreib-/Leseleistung in Cosmos DB. Weitere Informationen zu diesem Thema finden Sie im Artikel [Ändern der Datenbank- und Abfragekonsistenzebenen](../documentdb/documentdb-consistency-levels.md) .

## <a name="upserts-from-stream-analytics"></a>Einfügen/Aktualisieren über Stream Analytics
Die Stream Analytics-Integration mit Cosmos DB ermöglicht das Einfügen oder Aktualisieren von Datensätzen in Ihrer Cosmos DB-Sammlung auf der Grundlage einer angegebenen Dokument-ID-Spalte. Dies wird auch als *Upsert*(Kunstwort aus Update und Insert) bezeichnet.

Stream Analytics nutzt einen optimistischen Upsert-Ansatz, bei dem Aktualisierungen nur erfolgen, wenn eine Einfügung aufgrund eines Dokument-ID-Konflikts keinen Erfolg hat. Diese Aktualisierung wird von Stream Analytics mit einem PATCH-Befehl ausgeführt. Dadurch sind Teilaktualisierungen des Dokuments möglich, was bedeutet, dass das Hinzufügen neuer Eigenschaften oder Austauschen einer vorhandenen Eigenschaft schrittweise erfolgt. Beachten Sie, dass Änderungen der Werte von Arrayeigenschaften in Ihrem JSON-Dokument dazu führen, dass das gesamte Array überschrieben wird, d. h. nicht zusammengeführt wird.

## <a name="data-partitioning-in-cosmos-db"></a>Partitionieren von Daten in CosmosDB
Für die Partitionierung Ihrer Daten werden [partitionierte Cosmos DB-Sammlungen](../cosmos-db/partition-data.md) empfohlen. 

Bei einzelnen Cosmos DB-Sammlungen können Sie Ihre Daten in Stream Analytics weiterhin auf der Grundlage der Abfragemuster und Leistungsanforderungen Ihrer Anwendung partitionieren. Jede Sammlung kann (maximal) bis zu 10 GB Daten enthalten. Derzeit besteht keine Möglichkeit, eine Sammlung zentral hochzuskalieren (oder überlaufen zu lassen). Für eine horizontale Skalierung ermöglicht Stream Analytics das Schreiben mehrerer Sammlungen mit einem gegebenen Präfix (siehe die nachfolgenden Nutzungsdetails). Stream Analytics verwendet die konsistente [HashPartitionResolver](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.partitioning.hashpartitionresolver.aspx)-Strategie basierend auf der vom Benutzer angegebenen „PartitionKey“-Spalte, um die Ausgabedatensätze zu partitionieren. Die Anzahl der Sammlungen mit dem angegebenen Präfix zur Startzeit des Streamingauftrags wird als Anzahl der Ausgabepartitionen verwendet, in der Auftrag parallel schreibt (Cosmos DB-Sammlungen = Ausgabepartitionen). Für eine einzelne Sammlung mit verzögerter Indizierung, in die nur Einfügungen erfolgen, kann ein Schreibdurchsatz von 0,4 MB/s erwartet werden. Mithilfe mehrerer Sammlungen können Sie einen höheren Durchsatz und eine höhere Kapazität ermöglichen.

Wenn Sie beabsichtigen, die Anzahl der Partitionen künftig zu erhöhen, müssen Sie den Auftrag beenden, die Daten in Ihren vorhandenen Sammlungen in neuen Sammlungen neu partitionieren und dann den Stream Analytics-Auftrag neu starten. Weitere Informationen zum Verwenden von „PartitionResolver“ und Neupartitionierung sowie Beispielcode werden in einem Folgeartikel enthalten sein. Der Artikel [Partitionieren und Skalieren von Daten in Cosmos DB](../documentdb/documentdb-partition-data.md) enthält auch entsprechende Details.

## <a name="cosmos-db-settings-for-json-output"></a>Cosmos DB-Einstellungen für die JSON-Ausgabe
Beim Erstellen einer Cosmos DB-Datenbank als Ausgabe in Stream Analytics wird eine Aufforderung zur Eingabe von Informationen eingeblendet. Dieser Abschnitt enthält eine Erklärung der Definition der Eigenschaften.

Partitionierte Sammlung | Mehrere Sammlungen mit einer einzelnen Partition
---|---
![Bildschirm mit DocumentDB als Stream Analytics-Ausgabe](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-1.png) |  ![Bildschirm mit DocumentDB als Stream Analytics-Ausgabe](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-2.png)


  
> [!NOTE]
> Das Szenario **Mehrere Sammlungen mit einer einzelnen Partition** erfordert einen Partitionsschlüssel und wird als Konfiguration unterstützt. 

* **Ausgabealias**: Ein Alias zum Verweisen auf diese Ausgabe in Ihrer ASA-Abfrage.  
* **Kontoname**: Der Name oder Endpunkt-URI des Cosmos DB-Kontos.  
* **Kontoschlüssel**: Der gemeinsame Zugriffsschlüssel für das Cosmos DB-Konto.  
* **Datenbank**: Der Name der Cosmos DB-Datenbank.  
* **Muster für Sammlungsnamen**: Der Sammlungsname oder das Muster für die zu verwendenden Sammlungen. Das Sammlungsnamenformat kann mit dem optionalen Token {partition} gebildet werden, wobei Partitionen bei 0 beginnen. Im Folgenden finden Sie Beispiele gültiger Eingaben:  
  1\) MyCollection – Eine Sammlung mit dem Namen „MyCollection“ muss vorhanden sein.  
  2\) MyCollection{partition} – Solche Auflistungen müssen vorhanden sein: „MyCollection0“, „MyCollection1“, „MyCollection2“ usw.  
* **Partitionsschlüssel**: Optional. Nur erforderlich, wenn Sie im Muster Ihres Sammlungsnamens ein {partition}-Token verwenden. Der Name des Felds in Ausgabeereignissen, das zur Angabe des Schlüssels für die Partitionierung der Ausgabe über Sammlungen hinweg verwendet wird. Für die Ausgabe einer einzelnen Sammlung kann eine beliebige Ausgabespalte wie „PartitionId“ verwendet werden.  
* **Dokument-ID** : Optional. Der Name des Felds in Ausgabeereignissen, das zur Angabe des Primärschlüssels verwendet wird, auf dem Einfüge- und Aktualisierungsvorgänge basieren.  

