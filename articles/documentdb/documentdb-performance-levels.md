---
title: Leistungsebenen in DocumentDB | Microsoft Docs
description: "Erfahren Sie, wie Sie mithilfe von Leistungsebenen in DocumentDB den Durchsatz pro Sammlung reservieren können."
services: documentdb
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 7dc21c71-47e2-4e06-aa21-e84af52866f4
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: mimig
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 094729399070a64abc1aa05a9f585a0782142cbf
ms.openlocfilehash: 659d6bd63ea996af7b7b172f998884354e5d5858
ms.lasthandoff: 03/07/2017


---
# <a name="retiring-the-s1-s2-and-s3-performance-levels-in-documentdb"></a>Aussondern der Leistungsebenen S1, S2 und S3 in DocumentDB

> [!IMPORTANT] 
> Die in diesem Artikel beschriebenen Leistungsebenen S1, S2 und S3 werden ausgesondert und sind für neue DocumentDB-Sammlungen nicht mehr verfügbar.
>

Dieser Artikel enthält eine Übersicht über die Leistungsebenen S1, S2 und S3. Es wird beschrieben, wie die Sammlungen, für die diese Leistungsebenen verwendet werden, am 1. August 2017 zu Sammlungen mit nur einer Partition migriert werden. Nach Lesen dieses Artikels können Sie die folgenden Fragen beantworten:

- [Warum werden die Leistungsebenen S1, S2 und S3 ausgesondert?](#why-retired)
- [Welche Unterschiede weisen Sammlungen mit nur einer Partition und partitionierte Sammlungen gegenüber den Leistungsebenen S1, S2 und S3 auf?](#compare)
- [Was muss ich tun, um den unterbrechungsfreien Zugriff auf meine Daten sicherzustellen?](#uninterrupted-access)
- [Wie verändert sich meine Sammlung nach der Migration?](#collection-change)
- [Wie verändert sich meine Abrechnung nach der Migration zu Sammlungen mit nur einer Partition?](#billing-change)
- [Welche Möglichkeiten habe ich, wenn ich mehr als 10 GB Speicher benötige?](#more-storage-needed)
- [Kann ich vor dem 1. August 2017 zwischen den Leistungsebenen S1, S2 und S3 wechseln?](#change-before)
- [Woran kann ich erkennen, dass meine Sammlung migriert wurde?](#when-migrated)
- [Wie kann ich die Migration von den Leistungsebenen S1, S2 und S3 zu Sammlungen mit nur einer Partition selbst durchführen?](#migrate-diy)
- [Inwieweit bin ich betroffen, wenn ich EA-Kunde bin?](#ea-customer)

<a name="why-retired"></a>

## <a name="why-are-the-s1-s2-and-s3-performance-levels-being-retired"></a>Warum werden die Leistungsebenen S1, S2 und S3 ausgesondert?

Die Leistungsebenen S1, S2 und S3 bieten nicht die Flexibilität wie DocumentDB-Sammlungen mit nur einer Partition. Bei den Leistungsebenen S1, S2 und S3 waren sowohl der Durchsatz als auch die Speicherkapazität voreingestellt. Mit DocumentDB können Sie den Durchsatz und den Speicher jetzt anpassen, sodass Sie viel flexibler skalieren können, wenn sich Ihre Anforderungen ändern.

<a name="compare"></a>

## <a name="how-do-single-partition-collections-and-partitioned-collections-compare-to-the-s1-s2-s3-performance-levels"></a>Welche Unterschiede weisen Sammlungen mit nur einer Partition und partitionierte Sammlungen gegenüber den Leistungsebenen S1, S2 und S3 auf?

Die folgende Tabelle enthält einen Vergleich der Durchsatz- und Speicheroptionen von Sammlungen mit nur einer Partition, partitionierten Sammlungen und den Leistungsebenen S1, S2 und S3. Hier ist ein Beispiel für die Region „USA, Osten 2“ angegeben:

|   |Partitionierte Sammlung|Sammlung mit nur einer Partition|S1|S2|S3|
|---|---|---|---|---|---|
|Maximaler Durchsatz|Unbegrenzt|10.000 RU/s|250 RU/s|1.000 RU/s|2.500 RU/s|
|Minimaler Durchsatz|2.500 RU/s|400 RU/s|250 RU/s|1.000 RU/s|2.500 RU/s|
|Maximale Speichergröße|Unbegrenzt|10 GB|10 GB|10 GB|10 GB|
|Preis|Durchsatz: 6 USD/100 RU/s<br><br>Speicher:&0;,25 USD/GB|Durchsatz: 6 USD/100 RU/s<br><br>Speicher:&0;,25 USD/GB|25 USD|50 USD|100 USD|

Sind Sie EA-Kunde? Wenn ja, helfen Ihnen die Informationen unter [Inwieweit bin ich betroffen, wenn ich EA-Kunde bin?](#ea-customer) weiter.

<a name="uninterrupted-access"></a>

## <a name="what-do-i-need-to-do-to-ensure-uninterrupted-access-to-my-data"></a>Was muss ich tun, um den unterbrechungsfreien Zugriff auf meine Daten sicherzustellen?

Nichts. Die Migration wird von DocumentDB für Sie durchgeführt. Wenn Sie eine S1-, S2- oder S3-Sammlung verwenden, wird Ihre aktuelle Sammlung am 31. Juli 2017 zu einer Sammlung mit nur einer Partition migriert. 

<a name="collection-change"></a>

## <a name="how-will-my-collection-change-after-the-migration"></a>Wie verändert sich meine Sammlung nach der Migration?

Bei Verwendung einer S1-Sammlung wird die Migration zu einer Sammlung mit nur einer Partition und einem Durchsatz von 400 RU/s durchgeführt. 400 RU/s ist der niedrigste Durchsatz, der für Sammlungen mit nur einer Partition verfügbar ist. Die Kosten für 400 RU/s bei einer Sammlung mit nur einer Partition entsprechen ungefähr den Kosten für eine S1-Sammlung mit 250 RU/s. Sie erhalten die zusätzlichen 150 RU/s also ohne weitere Kosten.

Bei Verwendung einer S2-Sammlung wird die Migration zu einer Sammlung mit nur einer Partition und 1.000 RU/s durchgeführt. Die Durchsatzstufe ändert sich für Sie nicht.

Bei Verwendung einer S3-Sammlung wird die Migration zu einer Sammlung mit nur einer Partition und 2.500 RU/s durchgeführt. Die Durchsatzstufe ändert sich für Sie nicht.

In jedem dieser Fälle können Sie nach der Migration Ihrer Sammlung die Durchsatzstufe anpassen oder je nach Bedarf zentral hoch- oder herunterskalieren, um für Ihre Benutzer den Zugriff mit geringer Wartezeit zu ermöglichen. Um die Durchsatzstufe nach der Migration Ihrer Sammlung zu ändern, öffnen Sie im Azure-Portal einfach Ihr DocumentDB-Konto, klicken auf „Skalieren“, wählen Ihre Sammlung aus und passen dann die Durchsatzstufe an. Dies ist im folgenden Screenshot dargestellt:

![Skalieren des Durchsatzes im Azure-Portal](./media/documentdb-performance-levels/azure-documentdb-portal-scale-throughput.png)

<a name="billing-change"></a>

## <a name="how-will-my-billing-change-after-im-migrated-to-the-single-partition-collections"></a>Wie verändert sich meine Abrechnung nach der Migration zu Sammlungen mit nur einer Partition?

Angenommen, Sie verfügen über zehn S1-Sammlungen mit jeweils 1 GB Speicher in der Region „USA, Osten“ und migrieren diese zehn S1-Sammlungen zu zehn Sammlungen mit nur einer Partition und 400 RU/s (Minimum). Ihre Rechnung sieht dann wie folgt aus, wenn Sie die zehn Sammlungen mit nur einer Partition einen ganzen Monat lang beibehalten:

![Vergleich der S1-Preise für zehn Sammlungen mit dem Preis für zehn Sammlungen mit nur einer Partition](./media/documentdb-performance-levels/documentdb-s1-vs-standard-pricing.png)

<a name="more-storage-needed"></a>

## <a name="what-if-i-need-more-than-10-gb-of-storage"></a>Welche Möglichkeiten habe ich, wenn ich mehr als 10 GB Speicher benötige?

Es spielt keine Rolle, ob Sie eine Sammlung mit der Leistungsebene S1, S2 oder S3 oder eine Sammlung mit nur einer Partition verwenden, für die jeweils 10 GB Speicher verfügbar sind: Sie können das DocumentDB-Datenmigrationstool verwenden, um Ihre Daten zu einer partitionierten Sammlung mit praktisch unbegrenztem Speicher zu migrieren. Informationen zu den Vorteilen einer partitionierten Sammlung finden Sie unter [Partitionieren und Skalieren von Daten in DocumentDB](documentdb-partition-data.md). Informationen dazu, wie Sie eine S1-, S2- oder S3-Sammlung oder eine Sammlung mit nur einer Partition zu einer partitionierten Sammlung migrieren, finden Sie unter [Migrieren von Sammlungen mit nur einer Partition zu partitionierten Sammlungen](documentdb-partition-data.md#migrating-from-single-partition). 

<a name="change-before"></a>

## <a name="can-i-change-between-the-s1-s2-and-s3-performance-levels-before-august-1-2017"></a>Kann ich vor dem 1. August 2017 zwischen den Leistungsebenen S1, S2 und S3 wechseln?

Nur für vorhandene Konten mit S1-, S2- und S3-Leistung können die Leitungsebenentarife über das Portal oder programmgesteuert geändert werden. Ab dem 1. August 2017 sind die Leistungsebenen S1, S2 und S3 nicht mehr verfügbar. Wenn Sie von S1, S2 oder S3 zu einer Sammlung mit nur einer Partition wechseln, ist es nicht möglich, zu den Leistungsebenen S1, S2 oder S3 zurückzukehren.

<a name="when-migrated"></a>

## <a name="how-will-i-know-when-my-collection-has-migrated"></a>Woran kann ich erkennen, dass meine Sammlung migriert wurde?

Die Migration wird am 31. Juli 2017 durchgeführt. Wenn Sie eine Sammlung mit der Leistungsebene S1, S2 oder S3 verwenden, erhalten Sie vom DocumentDB-Team vor der Durchführung der Migration eine E-Mail. Nach Abschluss der Migration wird am 1. August 2017 im Azure-Portal angezeigt, dass für Ihre Sammlung Standard-Preise verwendet werden.

![Sicherstellen, dass Ihre Sammlung zum Standard-Tarif migriert wurde](./media/documentdb-performance-levels/documentdb-portal-standard-pricing-applied.png)

<a name="migrate-diy"></a>

## <a name="how-do-i-migrate-from-the-s1-s2-s3-performance-levels-to-single-partition-collections-on-my-own"></a>Wie kann ich die Migration von den Leistungsebenen S1, S2 und S3 zu Sammlungen mit nur einer Partition selbst durchführen?

Sie können von den Leistungsebenen S1, S2 und S3 die Migration zu Sammlungen mit nur einer Partition über das Azure-Portal oder programmgesteuert durchführen. Bis zum 1. August können Sie diesen Schritt selbst ausführen, um von den flexiblen Durchsatzoptionen für Sammlungen mit nur einer Partition zu profitieren. Andernfalls migrieren wir die Sammlungen für Sie am 31. Juli 2017.

**So migrieren Sie Sammlungen mit nur einer Partition über das Azure-Portal**

1. Klicken Sie im [**Azure-Portal**](https://portal.azure.com) auf **NoSQL (DocumentDB)**, und wählen Sie das zu ändernde DocumentDB-Konto aus. 
 
    Wenn **NoSQL (DocumentDB)** in der Navigationsleiste nicht angezeigt wird, klicken Sie auf „>“, scrollen Sie zu **Datenbanken**, wählen Sie **NoSQL (DocumentDB)** und dann das DocumentDB-Konto aus.  

2. Klicken Sie im Ressourcenmenü unter **Sammlungen** auf **Skalieren**, wählen Sie in der Dropdownliste die zu ändernde Sammlung aus, und klicken Sie dann auf **Tarif**. Für Konten mit vordefiniertem Durchsatz gilt der Tarif S1, S2 oder S3.  Klicken Sie auf dem Blatt **Preisstufe auswählen** auf **Standard**, um den benutzerdefinierten Durchsatz zu ändern, und anschließend auf **Auswählen**, um die Änderung zu speichern.

    ![Screenshot des Blatts „Einstellungen“, der veranschaulicht, wo Sie den Durchsatzwert ändern können](./media/documentdb-performance-levels/documentdb-change-performance-set-thoughput.png)

3. Auf dem Blatt **Skalieren** wurde der **Tarif** in **Standard** geändert, und im Feld **Durchsatz (RU/s)** wird als Standardwert „400“ angezeigt. Legen Sie den Durchsatz auf einen Wert zwischen 400 und 10.000 [Anforderungseinheiten](documentdb-request-units.md)/Sekunde (RU/s) fest. Die **Geschätzte monatliche Rechnung** am unteren Rand der Seite wird automatisch aktualisiert, um eine Schätzung der monatlichen Kosten anzugeben. 

    >[!IMPORTANT] 
    > Nachdem Sie Ihre Änderungen gespeichert haben und zum Standard-Tarif gewechselt sind, ist kein Rollback zu den Leistungsebenen S1, S2 oder S3 mehr möglich.

4. Klicken Sie zum Speichern der Änderungen auf **Speichern**.

    Wenn Sie feststellen, dass Sie einen höheren Durchsatz (größer als 10.000 RU/s) oder mehr Speicher (größer als 10 GB) benötigen, können Sie eine partitionierte Sammlung erstellen. Informationen zur Migration einer Sammlung mit nur einer Partition zu einer partitionierten Sammlung finden Sie unter [Migrieren von Sammlungen mit nur einer Partitionen zu partitionierten Sammlungen](documentdb-partition-data.md#migrating-from-single-partition).

    > [!NOTE]
    > Die Umstellung von S1, S2 oder S3 auf Standard kann bis zu zwei Minuten dauern.
    > 
    > 

**So migrieren Sie Sammlungen mit nur einer Partition über das .NET SDK**

Eine weitere Möglichkeit zum Ändern der Leistungsstufen Ihrer Sammlungen stellen unsere SDK dar. In diesem Abschnitt wird nur das Ändern einer Sammlung mit unserer [.NET SDK](https://msdn.microsoft.com/library/azure/dn948556.aspx) behandelt. Die Vorgehensweise bei unseren anderen [SDKs](https://msdn.microsoft.com/library/azure/dn781482.aspx) ist jedoch ähnlich. Falls Sie noch keine Erfahrung mit unserem .NET SDK haben, hilft Ihnen das [Tutorial zu den ersten Schritten](documentdb-get-started.md) weiter.

Hier ist ein Codeausschnitt zum Ändern des Sammlungsdurchsatzes auf 5.000 Anforderungseinheiten pro Sekunde angegeben:
    
```C#
    //Fetch the resource to be updated
    Offer offer = client.CreateOfferQuery()
                      .Where(r => r.ResourceLink == collection.SelfLink)    
                      .AsEnumerable()
                      .SingleOrDefault();

    // Set the throughput to 5000 request units per second
    offer = new OfferV2(offer, 5000);

    //Now persist these changes to the database by replacing the original resource
    await client.ReplaceOfferAsync(offer);
```

Besuchen Sie [MSDN](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.aspx) , um weitere Beispiele anzuzeigen und mehr über unsere Angebotsmethoden zu erfahren:

* [**ReadOfferAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readofferasync.aspx)
* [**ReadOffersFeedAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readoffersfeedasync.aspx)
* [**ReplaceOfferAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.replaceofferasync.aspx)
* [**CreateOfferQuery**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createofferquery.aspx)

<a name="ea-customer"></a>

## <a name="how-am-i-impacted-if-im-an-ea-customer"></a>Inwieweit bin ich betroffen, wenn ich EA-Kunde bin?

EA-Kunden genießen bis zum Ablauf ihres aktuellen Vertrags Preissicherheit.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Preisen und der Datenverwaltung mit Azure DocumentDB finden Sie in folgenden Ressourcen:

1.    [Partitionieren von Daten in DocumentDB](documentdb-partition-data.md): Es werden die Unterschiede zwischen Sammlungen mit nur einer Partition und partitionierten Sammlungen beschrieben, und Sie erhalten Tipps zur Implementierung einer Partitionierungsstrategie für die nahtlose Skalierung.
2.    [DocumentDB-Preise](https://azure.microsoft.com/pricing/details/documentdb/): Enthält Informationen zu den Kosten für den Bereitstellungsdurchsatz und für den Speicherverbrauch.
3.    [Anforderungseinheiten](documentdb-request-units.md): Enthält Informationen zum Verbrauch des Durchsatzes für verschiedene Vorgangstypen, z.B. Lesen, Schreiben, Abfragen.
4.    [Modellieren von Daten in DocumentDB](documentdb-modeling-data.md): Es wird beschrieben, wie Sie Ihre Daten für DocumentDB modellieren.
