---
title: 'Azure Cosmos DB: Anforderungseinheiten pro Minute (RU/Min.) | Microsoft-Dokumentation'
description: "Erfahren Sie, wie Sie mithilfe von Anforderungseinheiten pro Minute die Kosten senken können."
services: cosmosdb
documentationcenter: 
author: arnomicrosoft
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmosdb
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/10/2017
ms.author: acomet
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 878b7335fb5e09bc8704f7211cc6293ad6ea4bea
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="request-units-per-minute-in-azure-cosmos-db"></a>Anforderungseinheiten pro Minute in Azure Cosmos DB

Azure Cosmos DB wurde entwickelt, damit Sie eine schnelle, vorhersagbare Leistung für Ihre Anwendung erzielen und diese nahtlos entsprechend ihrem Wachstum skalieren können. Sie können Durchsatz für einen Cosmos DB-Container sowohl pro Sekunde als auch pro Minute bereitstellen. Der bereitgestellte Durchsatz mit minutengenauer Granularität dient zur Verarbeitung unerwarteter Lastspitzen in der Workload, für die der Durchsatz mit sekundengenauer Granularität bereitgestellt wurde. 

Dieser Artikel bietet eine Übersicht über die Funktionsweise von Anforderungseinheiten pro Minute (Request Unit per Minute, RU/Min.). Ziel bei der Bereitstellung von Anforderungseinheiten pro Minute ist es, für nicht vorhersagbare Anforderungen (insbesondere, wenn Sie zusätzlich zu Ihren Betriebsdaten Analysen ausführen müssen) und Workloads mit unvorhersehbaren Lastspitzen eine vorhersagbare Leistung bereitzustellen. Unsere Kunden sollen mehr als den von ihnen bereitgestellten Durchsatz nutzen können, damit sie schnell und sorgenfrei skalieren können.

Nach dem Lesen dieses Artikels können Sie die folgenden Fragen beantworten:

* Wie funktionieren Anforderungseinheiten pro Minute?
* Was ist der Unterschied zwischen Anforderungseinheiten pro Minute und Anforderungseinheiten pro Sekunde?
* Wie werden Anforderungseinheiten pro Minute bereitgestellt?
* In welchem Szenario sollte ich die Bereitstellung von Anforderungseinheiten pro Minute in Betracht ziehen?
* Wie verwende ich das Portal, um Kosten und Leistung zu optimieren?
* Für welche Art von Anforderungen kann mein Budget für Anforderungseinheiten pro Minute genutzt werden?

## <a name="provisioning-request-units-per-minute-rum"></a>Bereitstellen von Anforderungseinheiten pro Minute

Wenn Sie Azure Cosmos DB mit sekundengenauer Granularität (Anforderungseinheiten pro Sekunde, RUs/Sek.) bereitstellen, erhalten Sie die Garantie, dass Ihre Anforderung mit geringer Latenz erfolgreich verarbeitet wird, wenn der Durchsatz in dieser Sekunde nicht die bereitgestellte Kapazität überschreitet. Bei der minutengenauen Granularität erhalten Sie die Garantie, dass Ihre Anforderung innerhalb dieser Minute erfolgreich verarbeitet wird. Im Gegensatz zu Burstingsystemen stellen wir sicher, dass Sie eine vorhersagbare Leistung erzielen, mit der Sie planen können.

Die Funktionsweise der Bereitstellung mit minutengenauer Granularität ist einfach:

* RUs/Min. werden stündlich und zusätzlich zu RUs/Sek. in Rechnung gestellt. Weitere Informationen finden Sie in der [Preisübersicht](https://aka.ms/acdbpricing) für Azure Cosmos DB.
* RUs/Min. können auf Ebene der Sammlung aktiviert werden. Hierzu können Sie die SDKs (Node.js, Java oder .NET) oder das Portal verwenden (auch für MongoDB API-Workloads).
* Wenn die Anforderungseinheiten pro Minute aktiviert sind, erhalten Sie für jeweils 100 bereitgestellte RUs/Sek. zusätzlich 1.000 RUs/Min. (das Verhältnis ist 1:10).
* In jeder Sekunde nutzt eine Anforderungseinheit nur dann das Kontingent an RUs/Min., wenn Ihre sekundenbasierte Bereitstellung in dieser Sekunde überschritten wurde.
* Sobald der 60-Sekunden-Zeitraum (UTC-Zeit) verstrichen ist, wird das Kontingent der RUs/Min. wieder aufgefüllt.
* RUs/Min. können nur für Sammlungen mit maximal 5.000 bereitgestellten RUs/Sek. pro Partition aktiviert werden. Wenn Sie Ihre Durchsatzanforderungen skalieren und eine so hohe Bereitstellung pro Partition angeben, wird eine Warnmeldung angezeigt.

Im Folgenden finden Sie ein konkretes Beispiel, in dem ein Kunde 10.000 RUs/Sek mit 100.000 RUs/Min. bereitstellen kann. Dabei werden 73% Kosteneinsparungen erzielt im Vergleich mit einer Bereitstellung für Spitzenlasten (mit 50.000 RUs/Sek.) während eines 90-Sekunden-Zeitraums in einer Sammlung, für die 10.000 RUs/Sek. und 100.000 RUs/Min. bereitgestellt wurden:

* 1. Sekunde: Das Budget an RUs/Min. ist auf 100.000 festgelegt.
* 3. Sekunde: Während dieser Sekunde beträgt der Verbrauch an Anforderungseinheiten 11.010 RUs und damit 1.010 RUs über dem Kontingent der RUs/Sek. Daher werden 1.010 RUs vom Budget der RUs/Min. abgezogen. 98.990 RUs stehen für die nächsten 57 Sekunden im Budget an RUs/Min. zur Verfügung.
* 29. Sekunde: Während dieser Sekunde tritt eine Lastspitze auf (mehr als 4-mal so hoch wie die Bereitstellung pro Sekunde) und der Verbrauch an Anforderungseinheiten beträgt 46.920 RUs. 36.920 RUs werden vom Budget an RUs/Min. abgezogen, das damit von 92.323 RUs (28. Sekunde) auf 55.403 RUs (29. Sekunde) sinkt.
* 61. Sekunde: Das Budget an RUs/Min. wird auf 100.000 RUs zurückgesetzt.
 
![Diagramm mit Verbrauch und Bereitstellung von Azure Cosmos DB](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute.png)

## <a name="specifying-request-unit-capacity-with-rum"></a>Angeben der Kapazität der Anforderungseinheiten mit RUs/Min.

Wenn Sie eine Azure Cosmos DB-Sammlung erstellen, geben Sie die Anzahl von Anforderungseinheiten pro Sekunde an, die für die Sammlung reserviert werden sollen. Sie können auch entscheiden, ob Sie RUs pro Minute hinzufügen möchten. Dies kann über das Portal oder das SDK erfolgen. 

### <a name="through-the-portal"></a>Über das Portal

Die Aktivierung oder Deaktivierung von RUs pro Minute erfordert nur einen Klick beim Bereitstellen einer Sammlung. 

 ![Screenshot: Einrichten von RUs/Min. im Azure-Portal](./media/request-units-per-minute/azure-cosmos-db-request-unit-per-minute-portal.png)

### <a name="through-the-sdk"></a>Über das SDK
Beachten Sie, dass RUs/Min. nur für die folgenden SDKs verfügbar sind:

* .NET 1.14.0
* Java 1.11.0
* Node.js 1.12.0
* Python 2.2.0

Hier sehen Sie einen Codeausschnitt zum Erstellen einer Sammlung mit 3.000 Anforderungseinheiten pro Sekunde und 30.000 Anforderungseinheiten pro Minute mit dem .NET SDK:

```csharp
// Create a collection with RU/m enabled
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

// Set the throughput to 3,000 request units per second which will give you 30,000 request units per minute as the RU/m budget
await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 3000, OfferEnableRUPerMinuteThroughput = true });
```

Hier sehen Sie einen Codeausschnitt zum Ändern des Durchsatzes einer Sammlung auf 5.000 Anforderungseinheiten pro Sekunde ohne Bereitstellung von Anforderungseinheiten pro Minute mit dem.NET SDK:

```csharp
// Get the current offer
Offer offer = client.CreateOfferQuery()
    .Where(r => r.ResourceLink == collection.SelfLink)    
    .AsEnumerable()
    .SingleOrDefault();

// Set the throughput to 5000 request units per second without RU/m enabled (the last parameter to OfferV2 constructor below)
OfferV2 offerV2 = new OfferV2(offer, 5000, false);

// Now persist these changes to the database by replacing the original resource
await client.ReplaceOfferAsync(offerV2);
```

## <a name="good-fit-scenarios"></a>Geeignete Szenarien

In diesem Abschnitt erhalten Sie einen Überblick über Szenarien, die sich für die Aktivierung von Anforderungseinheiten pro Minute eignen.

**Entwicklungs- und Testumgebung:** Gut geeignet. Wenn Sie Ihre Anwendung während der Entwicklungsphase mit verschiedenen Workloads testen, können Sie mit RUs/Min. die notwendige Flexibilität in dieser Phase erzielen. Der [Emulator](../documentdb/documentdb-nosql-local-emulator.md) ist ein gutes kostenloses Tool zum Testen von Azure Cosmos DB. Wenn Sie jedoch in einer Cloudumgebung beginnen möchten, erzielen Sie mit RUs/Min. mehr Flexibilität für Ihre Ad-hoc-Leistungsanforderungen. Sie haben mehr Zeit für die Entwicklung und müssen sich zunächst weniger Gedanken um die Leistungsanforderungen machen. Es empfiehlt sich, mit der Mindestbereitstellung von RUs/Sek. zu beginnen und RUs/Min. zu aktivieren.

**Unvorhersehbare Anforderungen mit Lastspitzen und minutengenauer Granularität:** Gut geeignet – Einsparungen: 25-75%. Unserer Erfahrung nach stellen RUs/Min. hier eine erhebliche Verbesserung dar, und die meisten Produktionsszenarien fallen in diese Gruppe. Wenn Sie über eine IoT-Workload verfügen, in der einige Male innerhalb einer Minute Lastspitzen auftreten, weil Abfragen ausgeführt werden, während das System gleichzeitig eine Masseneinfügung durchführt, benötigen Sie zusätzliche Kapazität zur Verarbeitung der Lastspitzenanforderungen. Es wird empfohlen, Ihre Ressourcenanforderungen mithilfe der folgenden Schritte zu optimieren.

 ![Diagramm mit RU-Verbrauch mit einer Granularität von fünf Minuten](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute-consumption.png)
 
 *Abbildung: Benchmark für RU-Verbrauch*

**Stabile, gleichbleibende Leistung:** Gut geeignet – Einsparungen: 10-20%. In einigen Fällen benötigen Sie einfach ein Leistungsniveau, bei dem Sie sich keine Gedanken um mögliche Lastspitzen und Drosselung machen müssen. Dann ist dieses Feature genau das Richtige für Sie. Hierbei empfiehlt es sich, RUs/Min. zu aktivieren und die Bereitstellung pro Sekunde geringfügig zu senken. Dieser Fall unterscheidet sich von dem oben beschriebenen, da Sie nicht versuchen, Ihre Bereitstellung aggressiv zu optimieren. Es handelt sich hier eher um einen Fall von „keine Drosselung“.

Kritische Vorgänge mit Ad-hoc-Anforderungen: Manchmal empfiehlt es sich, nur kritische Vorgänge auf das Budget an RUs/Min. zugreifen zu lassen, damit das Budget nicht von Ad-hoc-Anforderungen oder weniger wichtigen Vorgängen verbraucht wird. Dies lässt sich problemlos im folgenden Abschnitt definieren.

## <a name="using-the-portal-metrics-to-optimize-cost-and-performance"></a>Verwenden der Metriken im Portal, um Kosten und Leistung zu optimieren

**In den folgenden Wochen werden wir weitere Informationen dazu bereitstellen, wie Sie den Verbrauch von RUs pro Minute überwachen können, um Ihre Durchsatzanforderungen zu optimieren.**

Anhand der Metriken im Portal können Sie sehen, wie viele reguläre RU-Sekunden und wie viele RU-Minuten Sie verbrauchen. Indem Sie diese Metriken überwachen, können Sie Ihre Bereitstellung optimieren. 

Um zu ermitteln, wie Sie die RUs/Min. optimal einsetzen, empfehlen wir, schrittweise vorzugehen. Für jeden Schritt sollten Sie einen Überblick über den RU-Verbrauch in einem vollständigen Zyklus Ihrer Workload haben (hierbei kann es sich um Stunden, Tage oder sogar Wochen handeln), und Sie sollten wissen, wie Ihre RU-Bereitstellung genutzt wird.

Das Prinzip hinter diesem Ansatz: Ihre Durchsatzbereitstellung sollte so nah wie möglich an einem Bereitstellungspunkt liegen, der den unten genannten Leistungskriterien entspricht. 

![Diagramm mit RU-Verbrauch mit einer Granularität von fünf Minuten](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute-adjust-provisioning.png)
 
Um den optimalen Bereitstellungspunkt für Ihre Workload zu ermitteln, müssen Sie über folgende Informationen verfügen:

* Verbrauchsmuster: keine, unregelmäßige oder längere Lastspitzen? Lastspitzen: geringfügig (doppelt so hoch wie der Durchschnitt), mittel oder hoch (mehr als 10-mal so hoch wie der Durchschnitt)?
* Prozentsatz an eingeschränkten Anforderungen: ist ein geringes Maß an Drosselung in Ordnung? Wenn ja, wie viel ist akzeptabel? 

Sobald Sie Ihre Ziele ermittelt haben, können Sie Ihre Bereitstellung so optimal wie möglich gestalten.

Um Sie dabei zu unterstützen, bieten wir Ihnen eine allgemeine Anleitung zum Optimieren Ihrer Bereitstellung basierend auf dem Verbrauch von RUs/Min. Diese Anleitung gilt nicht für alle Arten von Workloads, basiert aber auf den Erkenntnissen aus der privaten Vorschauversion. Wir werden diese Richtwerte möglicherweise anpassen, wenn wir weitere Informationen und Erkenntnisse erhalten:

|Nutzung von RUs/Min. in %|Grad der Nutzung von RUs/Min.|Empfohlene Aktionen für die Bereitstellung|
|---|---|---|
|0-1%|Zu geringe Nutzung|Senken Sie die RUs/Sek., damit mehr RUs/Min. verbraucht werden.|
|1-10%|Ausreichende Nutzung|Behalten Sie die Bereitstellung auf dieser Stufe bei.|
|Über 10%|Zu hohe Nutzung|Erhöhen Sie die RUs/Sek., damit weniger RUs/Min. benötigt werden.|

## <a name="select-which-operations-can-consume-the-rum-budget"></a>Auswählen der Vorgänge, die das Budget an RUs/Min. verbrauchen dürfen

Sie können das Budget an RUs/Min. auch auf Anforderungsebene aktivieren/deaktivieren, um die Anforderung unabhängig vom Vorgangstyp zu verarbeiten. Wenn das reguläre Budget an RUs/Sek. aufgebraucht ist und die Anforderung nicht auf das Budget an RUs/Min. zugreifen kann, wird die Anforderung gedrosselt. Standardmäßig werden alle Anforderungen durch das Durchsatzbudget an RUs/Min. verarbeitet, wenn das Budget aktiviert ist. 

Hier finden Sie einen Codeausschnitt zum Deaktivieren des Budgets an RUs/Min. mithilfe der DocumentDB-API für CRUD- und Abfragevorgänge.

```csharp
// In order to disable any CRUD request for RU/m, set DisableRUPerMinuteUsage to true in RequestOptions
await client.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("db", "container"),
    new Document { Id = "Cosmos DB" },
    new RequestOptions { DisableRUPerMinuteUsage = true });
// In order to disable any query request for RU/m, set DisableRUPerMinuteOnRequest to true in RequestOptions
FeedOptions feedOptions = new FeedOptions();
feedOptions.DisableRUPerMinuteUsage = true;
var query = client.CreateDocumentQuery<Book>(
    UriFactory.CreateDocumentCollectionUri("db", "container"),
    "select * from c",feedOptions).AsDocumentQuery();
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurde beschrieben, wie die Partitionierung in Azure Cosmos DB funktioniert, wie Sie partitionierte Sammlungen erstellen und wie Sie einen sinnvollen Partitionsschlüssel für Ihre Anwendung auswählen.

* Nutzen Sie Azure Cosmos DB, um Skalierungs- und Leistungstests durchzuführen. Ein Beispiel finden Sie unter [Leistungs- und Skalierungstests mit Azure Cosmos DB](../documentdb/documentdb-performance-testing.md).
* Beginnen Sie mit dem Schreiben von Code mit den [SDKs](../documentdb/documentdb-sdk-dotnet.md) oder der [REST-API](https://msdn.microsoft.com/library/azure/dn781481.aspx).
* Erfahren Sie mehr über den [bereitgestellten Durchsatz](../documentdb/documentdb-request-units.md) in Azure Cosmos DB. 


