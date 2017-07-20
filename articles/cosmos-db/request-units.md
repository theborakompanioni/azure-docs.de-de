---
title: "Anforderungseinheiten und Schätzen des Durchsatzes – Azure Cosmos DB | Microsoft-Dokumentation"
description: "Enthält Informationen zu den Grundlagen von Anforderungseinheiten in Azure Cosmos DB sowie zur Angabe und zur Schätzung des Bedarfs an Anforderungseinheiten."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: d0a3c310-eb63-4e45-8122-b7724095c32f
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: mimig
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: 95adddc01ee2814515c20f36e8503de30454a8f4
ms.contentlocale: de-de
ms.lasthandoff: 07/04/2017

---
# <a name="request-units-in-azure-cosmos-db"></a>Anforderungseinheiten in Azure Cosmos DB
Jetzt verfügbar: [Rechner für Anforderungseinheiten](https://www.documentdb.com/capacityplanner) in Azure Cosmos DB. Erfahren Sie mehr unter [Schätzen der Durchsatzanforderungen](request-units.md#estimating-throughput-needs).

![Durchsatzrechner][5]

## <a name="introduction"></a>Einführung
[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) ist eine global verteilte Datenbank von Microsoft mit mehreren Modellen. Mit Azure Cosmos DB müssen Sie keine virtuellen Computer mieten, Software bereitstellen oder Datenbanken überwachen. Azure Cosmos DB wird von Microsoft-Entwicklern betrieben und ständig überwacht, um erstklassige Verfügbarkeit, Leistung und Datensicherheit zu gewährleisten. Sie können über die API Ihrer Wahl auf Ihre Daten zugreifen – [DocumentDB SQL](documentdb-sql-query.md) (Dokument), MongoDB (Dokument), [Azure Table Storage](https://azure.microsoft.com/services/storage/tables/) (Schlüssel-Wert-Paar) und [Gremlin](https://tinkerpop.apache.org/gremlin.html) (Diagramm) werden nativ unterstützt. Die Währung von Azure Cosmos DB ist die Anforderungseinheit (Request Unit, RU). Dank der Anforderungseinheiten ist es nicht erforderlich, Kapazitäten für Lese- und Schreibvorgänge zu reservieren oder CPU, Arbeitsspeicher und IOPS bereitzustellen.

Azure Cosmos DB unterstützt eine Reihe von APIs mit anderen Vorgängen, die von Lese- und Schreibvorgängen bis hin zu komplexen Graph-Abfragen reichen. Da nicht alle Anforderungen gleich sind, wird ihnen eine normalisierte Menge von **Anforderungseinheiten** zugewiesen, die auf dem für das Bedienen der Anforderung erforderlichen Rechenaufwand basiert. Die Anzahl der Anforderungseinheiten für einen Vorgang ist deterministisch, und Sie können die Anzahl der von den einzelnen Vorgängen genutzten Anforderungseinheiten in Azure Cosmos DB über einen Antwortheader verfolgen. 

Sie müssen einen Durchsatz von 100 RU/Sekunde reservieren, um eine vorhersagbare Leistung bereitstellen zu können. Für jeden Block von 100 RU/Sekunde können Sie einen Block von 1.000 RU/Minute anfügen. Die Kombination von Bereitstellung pro Sekunde und pro Minute ist äußerst leistungsstark, da keine Bereitstellung für Spitzenlasten erforderlich ist und Sie im Vergleich zu Diensten, deren Bereitstellungen ausschließlich pro Sekunde erfolgen, bis zu 75 % der Kosten einsparen können.

Nach Lesen dieses Artikels können Sie die folgenden Fragen beantworten:  

* Was sind Anforderungseinheiten und Anforderungsgebühren?
* Wie gebe ich die Kapazität der Anforderungseinheiten für eine Sammlung an?
* Wie schätze ich die benötigten Anforderungseinheiten für meine Anwendung?
* Was geschieht, wenn ich die Kapazität der Anforderungseinheiten für eine Sammlung überschreite?

Da es sich bei Azure Cosmos DB um eine Datenbank mit mehreren Modellen handelt, ist es wichtig zu beachten, dass wir für eine Dokument-API auf eine Sammlung bzw. auf ein Dokument, für eine Graph-API auf einen Graph/Knoten, und für eine Tabellen-API auf eine Tabelle/Entität verweisen. In diesem Dokument werden allgemein die Konzepte von Container/Element verwendet.

## <a name="request-units-and-request-charges"></a>Anforderungseinheiten und Anforderungsgebühren
Azure Cosmos DB bietet eine schnelle, vorhersagbare Leistung durch die *Reservierung* von Ressourcen, die dem benötigten Durchsatz für Ihre Anwendung entsprechen.  Da sich Anwendungsauslastung und Zugriffsmuster mit der Zeit ändern, können Sie mit Azure Cosmos DB den Umfang des reservierten Durchsatzes, der Ihrer Anwendung zur Verfügung steht, ganz einfach erhöhen oder verringern.

Bei Azure Cosmos DB wird der reservierte Durchsatz in Anforderungseinheiten pro Sekunde oder pro Minute (Add-On) angegeben.  Sie können sich Anforderungseinheiten als Währung für den Durchsatz vorstellen, wobei Sie eine Anzahl garantierter Anforderungseinheiten auf Sekunden- oder Minutenbasis für Ihre Anwendung *reservieren*.  Jeder Vorgang in Azure Cosmos DB – das Schreiben eines Dokuments, das Durchführen einer Abfrage, das Aktualisieren eines Dokuments – beansprucht CPU, Arbeitsspeicher und IOPS.  Mit anderen Worten: Für jeden Vorgang fällt eine *Anforderungsgebühr* an, die in *Anforderungseinheiten* ausgedrückt wird.  Wenn Sie die Faktoren, die sich auf die berechneten Anforderungseinheiten auswirken, sowie die Durchsatzanforderungen Ihrer Anwendung genau kennen, können Sie die Kosten für Ihre Anwendung optimieren. Der Abfrage-Explorer ist auch ein hervorragendes Tool zum Testen des Kerns einer Abfrage.

Wir empfehlen Ihnen, sich zunächst das folgende Video anzusehen, in dem Aravind Ramachandran Anforderungseinheiten erklärt und die vorhersagbare Leistung mit Azure Cosmos DB erläutert.

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Predictable-Performance-with-DocumentDB/player]
> 
> 

## <a name="specifying-request-unit-capacity-in-azure-cosmos-db"></a>Angeben der Kapazität der Anforderungseinheiten in Azure Cosmos DB
Wenn Sie eine neue Sammlung, eine Tabelle oder einen Graph beginnen, geben Sie die Anzahl von Anforderungseinheiten (Request Units, RUs) pro Sekunde an, die reserviert werden sollen. Sie können auch entscheiden, ob Sie Anforderungseinheiten pro Minute aktivieren möchten. Wenn Sie diese Option aktivieren, erhalten Sie das Zehnfache von dem, was Sie pro Sekunde erhalten (jedoch pro Minute). Basierend auf dem bereitgestellten Durchsatz ordnet Azure Cosmos DB physische Partitionen zum Hosten Ihrer Sammlung zu, und Daten werden gemäß ihres Wachstums zwischen Partitionen aufgeteilt/neu verteilt.

Azure Cosmos DB erfordert die Angabe eines Partitionsschlüssels, wenn eine Sammlung mit 2.500 oder mehr Anforderungseinheiten bereitgestellt wird. Ein Partitionsschlüssel ist auch erforderlich, um den Durchsatz Ihrer Sammlung künftig auf über 2.500 Anforderungseinheiten zu skalieren. Das Konfigurieren eines [Partitionsschlüssels](partition-data.md) beim Erstellen eines Containers wird daher unabhängig von Ihrem ursprünglichen Durchsatz dringend empfohlen. Da Ihre Daten möglicherweise auf mehrere Partitionen aufgeteilt werden müssen, ist es notwendig, einen Partitionsschlüssel mit hoher Kardinalität (Hunderte bis Millionen von unterschiedlichen Werten) auszuwählen, damit Ihre Sammlung (bzw. Tabelle oder Graph) und die Anforderungen von Azure Cosmos DB gleichmäßig skaliert werden können. 

> [!NOTE]
> Ein Partitionsschlüssel ist eine logische Grenze, keine physische. Daher müssen Sie die Anzahl der unterschiedlichen Partitionsschlüsselwerte nicht beschränken. Es ist in der Tat besser, mehr unterschiedliche Partitionschlüsselwerte zu haben, da Azure Cosmos DB dann mehr Optionen für den Lastenausgleich zur Verfügung stehen.

Hier sehen Sie einen Codeausschnitt zum Erstellen einer Sammlung mit 3.000 Anforderungseinheiten pro Sekunde mit .NET SDK:

```csharp
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 3000 });
```

Azure Cosmos DB wird mit einem Reservierungsmodell für den Durchsatz ausgeführt. Ihnen wird also der *reservierte* Durchsatz berechnet, unabhängig davon, wie viel von diesem Durchsatz aktiv *verwendet* wird. Sie können die Menge reservierter RUs über SDKs oder über das [Azure-Portal](https://portal.azure.com) ganz leicht zentral hoch- oder herunterskalieren, wenn sich die Auslastung, die Daten und die Nutzungsmuster Ihrer Anwendung verändern.

Jede Sammlung, jede Tabelle bzw. jeder Graph ist einer `Offer`-Ressource in Azure Cosmos DB zugeordnet, die bzw. der Metadaten zu dem bereitgestellten Durchsatz enthält. Sie können den reservierten Durchsatz ändern, indem Sie die entsprechende Angebotsressource für einen Container suchen und mit dem neuen Durchsatzwert aktualisieren. Hier sehen Sie einen Codeausschnitt zum Ändern des Durchsatzes einer Sammlung auf 5.000 Anforderungseinheiten pro Sekunde mithilfe von .NET SDK:

```csharp
// Fetch the resource to be updated
Offer offer = client.CreateOfferQuery()
                .Where(r => r.ResourceLink == collection.SelfLink)    
                .AsEnumerable()
                .SingleOrDefault();

// Set the throughput to 5000 request units per second
offer = new OfferV2(offer, 5000);

// Now persist these changes to the database by replacing the original resource
await client.ReplaceOfferAsync(offer);
```

Die Durchsatzänderung hat keine Auswirkungen auf die Verfügbarkeit Ihres Containers. In der Regel wird der neue reservierte Durchsatz innerhalb von Sekunden nach der Anwendung des neuen Durchsatzes wirksam.

## <a name="request-unit-considerations"></a>Aspekte zu Anforderungseinheiten
Beim Abschätzen der Anzahl von Anforderungseinheiten, die für Ihren Azure Cosmos DB-Container reserviert werden soll, sollten Sie unbedingt die folgenden Variablen berücksichtigen:

* **Elementgröße**. Mit zunehmender Größe werden auch mehr Einheiten für das Lesen und Schreiben der Daten genutzt.
* **Anzahl der Elementeigenschaften**. Eine standardmäßige Indizierung aller Eigenschaften vorausgesetzt, werden mehr Einheiten für das Schreiben eines Dokuments, eines Knotens oder einer Entität genutzt, wenn die Anzahl der Eigenschaften steigt.
* **Datenkonsistenz**. Bei Verwendung der Datenkonsistenzebenen „Strong“ oder „Bounded Staleness“ werden zusätzliche Einheiten zum Lesen von Elementen genutzt.
* **Indizierte Eigenschaften**. Eine Indexrichtlinie für jeden Container gibt an, welche Eigenschaften standardmäßig indiziert werden. Sie können die Nutzung von Anforderungseinheiten reduzieren, indem Sie die Anzahl der indizierten Eigenschaften begrenzen oder die verzögerte Indizierung aktivieren.
* **Dokumentindizierung**. Standardmäßig wird jedes Element automatisch indiziert. Sie nutzen weniger Anforderungseinheiten, wenn Sie einige Elemente nicht indizieren.
* **Abfragemuster**. Die Komplexität einer Abfrage wirkt sich darauf aus, wie viele Anforderungseinheiten für einen Vorgang verbraucht werden. Die Anzahl von Prädikaten, die Art der Prädikate, Projektionen, die Anzahl von UDFs und die Größe des Quelldatasets beeinflussen die Kosten von Abfragevorgängen.
* **Skriptnutzung**.  Wie bei Abfragen beanspruchen gespeicherte Prozeduren und Trigger Anforderungseinheiten basierend auf der Komplexität des ausgeführten Vorgangs. Untersuchen Sie während der Entwicklung Ihrer Anwendung den "x-ms-request-charge"-Header, um herauszufinden, wie viel Anforderungseinheiten die einzelnen Vorgänge verbrauchen.

## <a name="estimating-throughput-needs"></a>Schätzen der Durchsatzanforderungen
Eine Anforderungseinheit ist eine normalisierte Kennzahl für die Anforderungsverarbeitungskosten. Eine einzelne Anforderungseinheit stellt die Verarbeitungskapazität dar, die erforderlich ist, um ein einzelnes, aus 10 eindeutigen Eigenschaftswerten (außer Systemeigenschaften) bestehendes Element von 1 KB zu lesen (per „self link“ oder ID). Eine Anforderung zum Erstellen (Einfügen), Ersetzen oder Löschen des gleichen Elements verbraucht mehr Verarbeitungsleistung des Diensts und daher mehr Anforderungseinheiten.   

> [!NOTE]
> Die Baseline einer Anforderungseinheit für ein Element von 1 KB entspricht einem einfachen GET-Vorgang per „self link“ oder ID des Elements.
> 
> 

In der Tabelle unten ist beispielsweise angegeben, wie viele Anforderungseinheiten bei drei unterschiedlichen Elementgrößen (1 KB, 4 KB und 64 KB) und bei zwei unterschiedlichen Leistungsebenen (500 Lesevorgänge/Sekunde + 100 Schreibvorgänge/Sekunde und 500 Lesevorgänge/Sekunde + 500 Schreibvorgänge/Sekunde) bereitgestellt werden sollten. Für die Datenkonsistenz wurde „Session“ konfiguriert, und die Indizierungsrichtlinie wurde auf „None“ festgelegt.

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Elementgröße</strong></p></td>
            <td valign="top"><p><strong>Lesevorgänge/Sekunde</strong></p></td>
            <td valign="top"><p><strong>Schreibvorgänge/Sekunde</strong></p></td>
            <td valign="top"><p><strong>Anforderungseinheiten</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 * 1) + (100 * 5) = 1.000 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 * 1) + (500 * 5) = 3.000 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 * 1,3) + (100 * 7) = 1.350 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 * 1,3) + (500 * 7) = 4.150 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 * 10) + (100 * 48) = 9.800 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 * 10) + (500 * 48) = 29.000 RU/s</p></td>
        </tr>
    </tbody>
</table>

### <a name="use-the-request-unit-calculator"></a>Verwenden des Rechners für Anforderungseinheiten
Damit Kunden ihre Durchsatzschätzungen optimieren können, gibt es einen webbasierten [Rechner für Anforderungseinheiten](https://www.documentdb.com/capacityplanner) , um den Bedarf an Anforderungseinheiten für normale Vorgänge zu schätzen, einschließlich:

* Erstellen von Elementen (Schreiben)
* Lesen von Elementen
* Löschen von Elementen
* Aktualisieren von Elementen

Das Tool unterstützt auch die Schätzung des Datenspeicherbedarfs auf der Grundlage der bereitgestellten Beispielelemente.

Die Verwendung des Tools ist einfach:

1. Laden Sie mindestens ein repräsentatives Element hoch.
   
    ![Hochladen von Elementen in den Rechner für Anforderungseinheiten][2]
2. Geben Sie zum Schätzen des Speicherplatzbedarfs die Gesamtanzahl von Elementen ein, die Sie voraussichtlich speichern werden.
3. Geben Sie die pro Sekunde benötige Anzahl von Erstellungs-, Lese-, Aktualisierungs- und Löschvorgängen für Elemente an. Laden Sie zum Ermitteln der bei Elementaktualisierungen voraussichtlich anfallenden Gebühren für Anforderungseinheiten eine Kopie des Beispielelements aus Schritt 1 mit typischen Feldaktualisierungen hoch.  Wenn bei Elementaktualisierungen also üblicherweise die beiden Eigenschaften „lastLogin“ und „userVisits“ geändert werden, kopieren Sie einfach das Beispielelement, aktualisieren Sie die Werte für diese beiden Eigenschaften, und laden Sie das kopierte Element anschließend hoch.
   
    ![Eingeben der Durchsatzanforderungen in den Rechner für Anforderungseinheiten][3]
4. Klicken Sie auf „Berechnen“, und prüfen Sie die Ergebnisse.
   
    ![Ergebnisse des Rechners für Anforderungseinheiten][4]

> [!NOTE]
> Wenn sich die Elementtypen im Hinblick auf Größe und Anzahl indizierter Eigenschaften erheblich voneinander unterscheiden, laden Sie ein Beispiel für jeden *Typ* eines normalen Elements an das Tools hoch, und berechnen Sie dann die Ergebnisse.
> 
> 

### <a name="use-the-azure-cosmos-db-request-charge-response-header"></a>Verwenden des Azure Cosmos DB-Antwortheaders „request-charge“
Jede Antwort des Azure Cosmos DB-Diensts enthält einen benutzerdefinierten Header (`x-ms-request-charge`), der die für die Anforderung verbrauchten Anforderungseinheiten enthält. Auf diesen Header kann auch über die DocumentDB-SDKs zugegriffen werden. Im .Net SDK ist „RequestCharge“ eine Eigenschaft des ResourceResponse-Objekts.  Für Abfragen stellt der Azure Cosmos DB-Abfrage-Explorer im Azure-Portal Informationen zu Anforderungsgebühren für ausgeführten Abfragen bereit.

![Untersuchen der berechneten RUs im Abfrage-Explorer][1]

Vor diesem Hintergrund besteht eine Methode zum Abschätzen des von der Anwendung benötigten Durchsatzes darin, typische Vorgänge mit einem repräsentativen, von Ihrer Anwendung verwendeten Element auszuführen, dabei die berechneten Anforderungseinheiten zu notieren und anschließend die Anzahl von Vorgängen zu schätzen, die erwartungsgemäß pro Sekunde ausgeführt werden.  Stellen Sie sicher, dass auch typische Abfragen und die Nutzung von Azure Cosmos DB-Skripts gemessen und berücksichtigt werden.

> [!NOTE]
> Wenn sich die Elementtypen im Hinblick auf Größe und Anzahl indizierter Eigenschaften erheblich voneinander unterscheiden, erfassen Sie für jeden typischen *Elementtyp* jeweils die berechneten Anforderungseinheiten des jeweiligen Vorgangs.
> 
> 

Beispiel:

1. Notieren Sie die berechneten Anforderungseinheiten für das Erstellen (Einfügen) eines typischen Elements. 
2. Notieren Sie die berechneten Anforderungseinheiten für das Lesen eines typischen Elements.
3. Notieren Sie die berechneten Anforderungseinheiten für das Aktualisieren eines typischen Elements.
4. Notieren Sie die berechneten Anforderungseinheiten für typische, häufig ausgeführte Elementabfragen.
5. Notieren Sie die berechneten Anforderungseinheiten für benutzerdefinierte Skripts (gespeicherte Prozeduren, Trigger, benutzerdefinierte Funktionen), die von der Anwendung genutzt werden.
6. Berechnen Sie die erforderlichen Anforderungseinheiten anhand der geschätzten Anzahl von Vorgängen, die erwartungsgemäß pro Sekunde ausgeführt werden.

### <a id="GetLastRequestStatistics"></a>Verwenden des GetLastRequestStatistics-Befehls von API für MongoDB
API für MongoDB unterstützt den benutzerdefinierten Befehl *getLastRequestStatistics*, um die Anforderungsgebühr für bestimmte Vorgänge abzurufen.

Führen Sie z.B. in der Mongo Shell den Vorgang aus, für den Sie die Anforderungsgebühr überprüfen möchten.
```
> db.sample.find()
```

Anschließend führen Sie den Befehl *getLastRequestStatistics* aus.
```
> db.runCommand({getLastRequestStatistics: 1})
{
    "_t": "GetRequestStatisticsResponse",
    "ok": 1,
    "CommandName": "OP_QUERY",
    "RequestCharge": 2.48,
    "RequestDurationInMilliSeconds" : 4.0048
}
```

Vor diesem Hintergrund besteht eine Methode zum Abschätzen des von der Anwendung benötigten Durchsatzes darin, typische Vorgänge mit einem repräsentativen, von Ihrer Anwendung verwendeten Element auszuführen, dabei die berechneten Anforderungseinheiten zu notieren und anschließend die Anzahl von Vorgängen zu schätzen, die erwartungsgemäß pro Sekunde ausgeführt werden.

> [!NOTE]
> Wenn sich die Elementtypen im Hinblick auf Größe und Anzahl indizierter Eigenschaften erheblich voneinander unterscheiden, erfassen Sie für jeden typischen *Elementtyp* jeweils die berechneten Anforderungseinheiten des jeweiligen Vorgangs.
> 
> 

## <a name="use-api-for-mongodbs-portal-metrics"></a>Verwenden von Portalmetriken von API für MongoDB
Die einfachste Möglichkeit, eine recht genaue Schätzung der Gebühren für Anforderungseinheiten für Ihre API für MongoDB-Datenbank zu erhalten, ist die Verwendung der Metriken im [Azure-Portal](https://portal.azure.com). Mit den Diagrammen *Anzahl von Anforderungen* und *Anforderungsgebühr* können Sie abschätzen, wie viele Anforderungseinheiten von jedem Vorgang verbraucht werden und wie viele Einheiten in Relation der Vorgänge zueinander verbraucht werden.

![Portalmetriken von API für MongoDB][6]

## <a name="a-request-unit-estimation-example"></a>Beispiel für die Schätzung von Anforderungseinheiten
Betrachten Sie das folgende Dokument von 1 KB:

```json
{
 "id": "08259",
  "description": "Cereals ready-to-eat, KELLOGG, KELLOGG'S CRISPIX",
  "tags": [
    {
      "name": "cereals ready-to-eat"
    },
    {
      "name": "kellogg"
    },
    {
      "name": "kellogg's crispix"
    }
  ],
  "version": 1,
  "commonName": "Includes USDA Commodity B855",
  "manufacturerName": "Kellogg, Co.",
  "isFromSurvey": false,
  "foodGroup": "Breakfast Cereals",
  "nutrients": [
    {
      "id": "262",
      "description": "Caffeine",
      "nutritionValue": 0,
      "units": "mg"
    },
    {
      "id": "307",
      "description": "Sodium, Na",
      "nutritionValue": 611,
      "units": "mg"
    },
    {
      "id": "309",
      "description": "Zinc, Zn",
      "nutritionValue": 5.2,
      "units": "mg"
    }
  ],
  "servings": [
    {
      "amount": 1,
      "description": "cup (1 NLEA serving)",
      "weightInGrams": 29
    }
  ]
}
```

> [!NOTE]
> Dokumente werden in Azure Cosmos DB minimiert, daher beträgt die vom System berechnete Größe des obigen Dokuments etwas weniger als 1 KB.
> 
> 

Die folgende Tabelle zeigt die ungefähre Anzahl berechneter Anforderungseinheiten für normale Vorgänge für dieses Element (bei der ungefähren Anzahl berechneter Anforderungseinheiten wird angenommen, dass die Kontokonsistenzebene auf „Session“ festgelegt ist und dass alle Elemente automatisch indiziert werden):

| Vorgang | Berechnete Anforderungseinheiten |
| --- | --- |
| Create item (Element erstellen) |~15 RUs |
| Read item (Element lesen) |~1 RU |
| Element abfragen nach ID |~2,5 RUs |

Diese Tabelle zeigt darüber hinaus die ungefähre Anzahl berechneter Anforderungseinheiten für normale Abfragen, die in der Anwendung verwendet werden:

| Abfrage | Berechnete Anforderungseinheiten | Anzahl zurückgegebener Elemente |
| --- | --- | --- |
| Nahrungsmittel nach ID auswählen |~2,5 RUs |1 |
| Nahrungsmittel nach Hersteller auswählen |~7 RUs |7 |
| Nach Nahrungsmittelgruppe auswählen und nach Gewicht bestellen |~70 RUs |100 |
| Die 10 beliebtesten Nahrungsmittel in einer Nahrungsmittelgruppe auswählen |~10 RUs |10 |

> [!NOTE]
> RU-Gebühren richten sich nach der Anzahl zurückgegebener Elemente.
> 
> 

Mit diesen Informationen können wir den RU-Bedarf für diese Anwendung angesichts der Anzahl erwarteter Vorgänge und Abfragen pro Sekunde abschätzen:

| Vorgang/Abfrage | Geschätzte Anzahl pro Sekunde | Erforderliche RUs |
| --- | --- | --- |
| Create item (Element erstellen) |10 |150 |
| Read item (Element lesen) |100 |100 |
| Nahrungsmittel nach Hersteller auswählen |25 |175 |
| Nach Nahrungsmittelgruppe auswählen |10 |700 |
| 10 beliebteste auswählen |15 |150 insgesamt |

In diesem Fall erwarten wir einen durchschnittlichen Durchsatzbedarf von 1,275 RU/s.  Wir runden auf den nächsten Hunderter auf und würden für die Sammlung dieser Anwendung 1.300 RU/s bereitstellen.

## <a id="RequestRateTooLarge"></a> Überschreiten von Grenzwerten für den reservierten Durchsatz in Azure Cosmos DB
Bedenken Sie, dass der Verbrauch von Anforderungseinheiten als Rate pro Sekunde ausgewertet wird, wenn die Anforderungseinheit pro Minute deaktiviert oder das Budget aufgebraucht ist. Für Anwendungen, die die bereitgestellte Anforderungseinheitsrate für einen Container überschreiten, werden Anforderungen an die Sammlung gedrosselt, bis die Rate unter das reservierte Niveau fällt. Bei einer Drosselung beendet der Server die Anforderung präemptiv mit „RequestRateTooLargeException“ (HTTP-Statuscode 429) und gibt den x-ms-retry-after-ms-Header zurück. Darin ist die Zeitspanne in Millisekunden angegeben, die der Benutzer abwarten muss, bevor ein neuer Anforderungsversuch unternommen werden kann.

    HTTP Status 429
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100

Wenn Sie das .NET Client SDK und LINQ-Abfragen verwenden, werden Sie sich normalerweise nicht mit dieser Ausnahme beschäftigen müssen, da die aktuelle Version des .NET Client SDK diese Antwort implizit abfängt, den vom Server angegebenen Retry-After-Header beachtet und die Anforderung wiederholt. Wenn nicht mehrere Clients gleichzeitig auf Ihr Konto zugreifen, wird die nächste Wiederholung erfolgreich ausgeführt.

Wenn mehrere Clients kumulativ oberhalb der Anforderungsrate arbeiten, reicht das Standard-Wiederholungsverhalten möglicherweise nicht aus, und der Client löst für die Anwendung eine DocumentClientException mit dem Statuscode 429 aus. In diesen Fällen sollten Sie in Betracht ziehen, das Wiederholungsverhalten und die zugehörige Logik in die Anwendungsroutinen zur Fehlerbehandlung aufzunehmen oder den reservierten Durchsatz für den Container zu erhöhen.

## <a id="RequestRateTooLargeAPIforMongoDB"></a> Überschreiten von Grenzwerten für den reservierten Durchsatz in API für MongoDB
Anwendungen, die die bereitgestellten Anforderungseinheiten für eine Sammlung überschreiten, werden gedrosselt, bis die Rate unter das reservierte Niveau fällt. Wenn eine Drosselung eintritt, beendet das Back-End die Anforderung präventiv mit einem *16500*-Fehlercode – *Zu viele Anforderungen*. API für MongoDB versucht standardmäßig automatisch bis zu 10-mal, eine Anforderung erneut zu senden, bevor der Fehlercode *Zu viele Anforderungen* zurückgegeben wird. Wenn Sie zu viele Fehlercodes *Zu viele Anforderungen* erhalten, sollten Sie in Betracht ziehen, das Wiederholungsverhalten in die Fehlerbehandlungsroutinen Ihrer Anwendung aufzunehmen oder den [reservierten Durchsatz für die Sammlung zu erhöhen](set-throughput.md).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum reservierten Durchsatz mit Azure Cosmos DB-Datenbanken finden Sie in folgenden Ressourcen:

* [Azure Cosmos DB-Preise](https://azure.microsoft.com/pricing/details/cosmos-db/)
* [Partitionierung von Daten in Azure Cosmos DB](partition-data.md)

Weitere Informationen zu Azure Cosmos DB finden Sie in der [Dokumentation](https://azure.microsoft.com/documentation/services/cosmos-db/) zu Azure Cosmos DB. 

Im Artikel [Leistungs- und Skalierungstests mit Azure Cosmos DB](performance-testing.md) finden Sie eine Einführung in Leistungs- und Skalierungstests mit Azure Cosmos DB.

[1]: ./media/request-units/queryexplorer.png 
[2]: ./media/request-units/RUEstimatorUpload.png
[3]: ./media/request-units/RUEstimatorDocuments.png
[4]: ./media/request-units/RUEstimatorResults.png
[5]: ./media/request-units/RUCalculator2.png
[6]: ./media/request-units/api-for-mongodb-metrics.png

