---
title: "Anforderungseinheiten und Schätzen des Durchsatzes – Azure DocumentDB | Microsoft-Dokumentation"
description: "Enthält Informationen zu den Grundlagen von Anforderungseinheiten in DocumentDB sowie zur Angabe und zur Schätzung des Bedarfs an Anforderungseinheiten."
services: documentdb
author: syamkmsft
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: d0a3c310-eb63-4e45-8122-b7724095c32f
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: syamk
translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: b098e3087cb08528c5fbdc2d0d768ce40e7ffe0d
ms.lasthandoff: 03/15/2017


---
# <a name="request-units-in-documentdb"></a>Anforderungseinheiten in DocumentDB
Jetzt verfügbar: [Rechner für Anforderungseinheiten](https://www.documentdb.com/capacityplanner)in DocumentDB. Erfahren Sie mehr unter [Schätzen der Durchsatzanforderungen](documentdb-request-units.md#estimating-throughput-needs).

![Durchsatzrechner][5]

## <a name="introduction"></a>Einführung
[Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) ist ein vollständig verwalteter, skalierbarer NoSQL-Datenbankdienst für JSON-Dokumente. Mit DocumentDB müssen Sie keine virtuellen Computer mieten, Software bereitstellen oder Datenbanken überwachen. DocumentDB wird von Microsoft-Entwicklern betrieben und ständig überwacht, um erstklassige Verfügbarkeit, Leistung und Datensicherheit zu gewährleisten. Daten in DocumentDB werden innerhalb von Sammlungen gespeichert. Dabei handelt es sich um flexible, hochverfügbare Container. Anstatt auf Hardwareressourcen wie CPU, Arbeitsspeicher und IOPs für eine Sammlung zu achten und zu verwalten, können Sie Durchsatz im Hinblick auf Anforderungen pro Sekunde reservieren. DocumentDB verwaltet automatisch die Bereitstellung, transparente Partitionierung und Skalierung Ihrer Sammlung, um die bereitgestellte Anzahl von Anforderungen zu bedienen. 

DocumentDB unterstützt eine Reihe von APIs für Lese- und Schreibvorgänge, Abfragen und Ausführungen von gespeicherten Prozeduren. Da nicht alle Anforderungen gleich sind, wird ihnen eine normalisierte Menge von **Anforderungseinheiten** zugewiesen, die auf dem für das Bedienen der Anforderung erforderlichen Rechenaufwand basiert. Die Anzahl der Anforderungseinheiten für einen Vorgang ist deterministisch, und Sie können die Anzahl der von den einzelnen Vorgängen genutzten Anforderungseinheiten in DocumentDB über einen Antwortheader verfolgen.

Für jede Sammlung in DocumentDB kann Durchsatz reserviert werden, der auch in Form von Anforderungseinheiten ausgedrückt wird. Dieser Durchsatz wird in Blöcken von 100 Anforderungseinheiten pro Sekunde ausgedrückt und kann von Hunderten bis zu Millionen von Anforderungseinheiten pro Sekunde reichen. Der bereitgestellte Durchsatz kann während der gesamten Lebensdauer einer Sammlung angepasst werden, um sie auf die veränderten Verarbeitungsanforderungen und Zugriffsmuster der Anwendung abzustimmen. 

Nach Lesen dieses Artikels können Sie die folgenden Fragen beantworten:  

* Was sind Anforderungseinheiten und Anforderungsgebühren?
* Wie gebe ich die Kapazität der Anforderungseinheiten für eine Sammlung an?
* Wie schätze ich die benötigten Anforderungseinheiten für meine Anwendung?
* Was geschieht, wenn ich die Kapazität der Anforderungseinheiten für eine Sammlung überschreite?

## <a name="request-units-and-request-charges"></a>Anforderungseinheiten und Anforderungsgebühren
DocumentDB und API für MongoDB bieten eine schnelle, vorhersagbare Leistung durch die *Reservierung* von Ressourcen, die dem benötigten Durchsatz für Ihre Anwendung entsprechen.  Da sich Anwendungsauslastung und Zugriffsmuster mit der Zeit ändern, können Sie mit DocumentDB den Umfang des reservierten Durchsatzes, der Ihrer Anwendung zur Verfügung steht, ganz einfach erhöhen oder verringern.

Bei DocumentDB wird der reservierte Durchsatz in Anforderungseinheiten pro Sekunde angegeben.  Sie können sich Anforderungseinheiten als Währung für den Durchsatz vorstellen, wobei Sie eine Anzahl garantierter Anforderungseinheiten auf Sekundenbasis für Ihre Anwendung *reservieren* .  Jeder Vorgang in DocumentDB – das Schreiben eines Dokuments, das Durchführen einer Abfrage, das Aktualisieren eines Dokuments – beansprucht CPU, Arbeitsspeicher und IOPS.  Mit anderen Worten: Für jeden Vorgang fällt eine *Anforderungsgebühr* an, die in *Anforderungseinheiten* ausgedrückt wird.  Wenn Sie die Faktoren, die sich auf die berechneten Anforderungseinheiten auswirken, sowie die Durchsatzanforderungen Ihrer Anwendung genau kennen, können Sie die Kosten für Ihre Anwendung optimieren. 

Wir empfehlen Ihnen, sich zunächst das folgende Video anzusehen, in dem Aravind Ramachandran Anforderungseinheiten erklärt und die vorhersagbare Leistung mit DocumentDB erläutert.

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Predictable-Performance-with-DocumentDB/player]
> 
> 

## <a name="specifying-request-unit-capacity-in-documentdb"></a>Angeben der Kapazität der Anforderungseinheiten in DocumentDB
Wenn Sie eine DocumentDB-Sammlung erstellen, geben Sie die Anzahl von Anforderungseinheiten (Request Units, RUs) pro Sekunde an, die für die Sammlung reserviert werden sollen. Basierend auf dem bereitgestellten Durchsatz ordnet DocumentDB physische Partitionen zum Hosten Ihrer Sammlung zu, und Daten werden gemäß ihres Wachstums zwischen Partitionen aufgeteilt/neu verteilt.

DocumentDB erfordert die Angabe eines Partitionsschlüssels, wenn eine Sammlung mit 10.000 oder mehr Anforderungseinheiten bereitgestellt wird. Ein Partitionsschlüssel ist auch erforderlich, um den Durchsatz Ihrer Sammlung künftig auf über 10.000 Anforderungseinheiten zu skalieren. Das Konfigurieren eines [Partitionsschlüssels](documentdb-partition-data.md) beim Erstellen einer Sammlung wird daher unabhängig von Ihrem ursprünglichen Durchsatz dringend empfohlen. Da Ihre Daten möglicherweise auf mehrere Partitionen aufgeteilt werden müssen, ist es notwendig, einen Partitionsschlüssel mit hoher Kardinalität (Hunderte bis Millionen von unterschiedlichen Werten) auszuwählen, damit Ihre Sammlung und die Anforderungen von DocumentDB gleichmäßig skaliert werden können. 

> [!NOTE]
> Ein Partitionsschlüssel ist eine logische Grenze, keine physische. Daher müssen Sie die Anzahl der unterschiedlichen Partitionsschlüsselwerte nicht beschränken. Es ist in der Tat besser, mehr unterschiedliche Partitionschlüsselwerte zu haben, da DocumentDB dann mehr Optionen für den Lastenausgleich zur Verfügung stehen.

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

DocumentDB wird mit einem Reservierungsmodell für den Durchsatz ausgeführt. Ihnen wird also der für die Sammlung *reservierte* Durchsatz berechnet, unabhängig davon, wie viel von diesem Durchsatz aktiv *verwendet* wird. Sie können die Menge reservierter RUs über DocumentDB-SDKs oder über das [Azure-Portal](https://portal.azure.com) ganz leicht zentral hoch- oder herunterskalieren, wenn sich die Auslastung, die Daten und die Nutzungsmuster Ihrer Anwendung verändern.

Jede Sammlung ist einer `Offer`-Ressource in DocumentDB zugeordnet, die Metadaten zu dem von der Sammlung bereitgestellten Durchsatz enthält. Sie können den reservierten Durchsatz ändern, indem Sie die entsprechende Angebotsressource für eine Sammlung suchen und mit dem neuen Durchsatzwert aktualisieren. Hier sehen Sie einen Codeausschnitt zum Ändern des Durchsatzes einer Sammlung auf 5.000 Anforderungseinheiten pro Sekunde mithilfe von .NET SDK:

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

Die Durchsatzänderung hat keine Auswirkungen auf die Verfügbarkeit Ihrer Sammlung. In der Regel wird der neue reservierte Durchsatz innerhalb von Sekunden nach der Anwendung des neuen Durchsatzes wirksam.

## <a name="specifying-request-unit-capacity-in-api-for-mongodb"></a>Angeben der Kapazität der Anforderungseinheiten in API für MongoDB
In API für MongoDB können Sie die Anzahl von Anforderungseinheiten (Request Units, RUs) pro Sekunde angeben, die für die Sammlung reserviert werden sollen.

API für MongoDB arbeitet mit dem gleichen auf Durchsatz basierenden Reservierungsmodell wie DocumentDB. Ihnen wird also der für die Sammlung *reservierte* Durchsatz berechnet, unabhängig davon, wie viel von diesem Durchsatz aktiv *verwendet* wird. Sie können die Menge reservierter RUs ganz einfach über das [Azure-Portal](https://portal.azure.com) zentral hoch- oder herunterskalieren, wenn sich die Auslastung, die Daten und die Nutzungsmuster Ihrer Anwendung verändern.

Die Durchsatzänderung hat keine Auswirkungen auf die Verfügbarkeit Ihrer Sammlung. In der Regel wird der neue reservierte Durchsatz innerhalb von Sekunden nach der Anwendung des neuen Durchsatzes wirksam.

## <a name="request-unit-considerations"></a>Aspekte zu Anforderungseinheiten
Beim Abschätzen der Anzahl von Anforderungseinheiten, die für Ihre DocumentDB-Sammlung reserviert werden soll, sollten Sie unbedingt die folgenden Variablen berücksichtigen:

* **Dokumentgröße**. Je größer ein Dokument, desto mehr Einheiten werden für das Lesen und Schreiben der Daten genutzt.
* **Anzahl der Dokumenteigenschaften**. Eine standardmäßige Indizierung aller Eigenschaften vorausgesetzt, werden mehr Einheiten für das Schreiben eines Dokuments genutzt, wenn die Anzahl der Eigenschaften steigt.
* **Datenkonsistenz**. Bei Verwendung der Datenkonsistenzebenen "Strong" oder "Bounded Staleness" werden zusätzliche Einheiten zum Lesen von Dokumenten genutzt.
* **Indizierte Eigenschaften**. Eine Indexrichtlinie für jede Sammlung gibt an, welche Eigenschaften standardmäßig indiziert werden. Sie können die Nutzung von Anforderungseinheiten reduzieren, indem Sie die Anzahl der indizierten Eigenschaften begrenzen oder die verzögerte Indizierung aktivieren.
* **Dokumentindizierung**. Standardmäßig wird jedes Dokument automatisch indiziert. Sie nutzen weniger Anforderungseinheiten, wenn Sie einige Dokumente nicht indizieren.
* **Abfragemuster**. Die Komplexität einer Abfrage wirkt sich darauf aus, wie viele Anforderungseinheiten für einen Vorgang verbraucht werden. Die Anzahl von Prädikaten, die Art der Prädikate, Projektionen, die Anzahl von UDFs und die Größe des Quelldatasets beeinflussen die Kosten von Abfragevorgängen.
* **Skriptnutzung**.  Wie bei Abfragen beanspruchen gespeicherte Prozeduren und Trigger Anforderungseinheiten basierend auf der Komplexität des ausgeführten Vorgangs. Untersuchen Sie während der Entwicklung Ihrer Anwendung den "x-ms-request-charge"-Header, um herauszufinden, wie viel Anforderungseinheiten die einzelnen Vorgänge verbrauchen.

## <a name="estimating-throughput-needs"></a>Schätzen der Durchsatzanforderungen
Eine Anforderungseinheit ist eine normalisierte Kennzahl für die Anforderungsverarbeitungskosten. Eine einzelne Anforderungseinheit stellt die Verarbeitungskapazität dar, die erforderlich ist, um ein einzelnes, aus 10 eindeutigen Eigenschaftswerten (außer Systemeigenschaften) bestehendes JSON-Dokument von 1 KB zu lesen (per „self link“ oder ID). Eine Anforderung zum Erstellen (Einfügen), Ersetzen oder Löschen des gleichen Dokuments verbraucht mehr Verarbeitungsleistung des Diensts und daher mehr Anforderungseinheiten.   

> [!NOTE]
> Die Baseline einer Anforderungseinheit für ein Dokument von 1 KB entspricht einem einfachen GET-Vorgang per „self link“ oder ID des Dokuments.
> 
> 

In der Tabelle unten ist beispielsweise angegeben, wie viele Anforderungseinheiten bei drei unterschiedlichen Dokumentgrößen (1 KB, 4 KB und 64 KB) und bei zwei unterschiedlichen Leistungsebenen (500 Lesevorgänge/Sekunde + 100 Schreibvorgänge/Sekunde und 500 Lesevorgänge/Sekunde + 500 Schreibvorgänge/Sekunde) bereitgestellt werden sollten. Für die Datenkonsistenz wurde „Session“ konfiguriert, und die Indizierungsrichtlinie wurde auf „None“ festgelegt.

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Dokumentgröße</strong></p></td>
            <td valign="top"><p><strong>Lesevorgänge/Sekunde</strong></p></td>
            <td valign="top"><p><strong>Schreibvorgänge/Sekunde</strong></p></td>
            <td valign="top"><p><strong>Anforderungseinheiten</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 *1) + (100* 5) = 1.000 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 *5) + (100* 5) = 3.000 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 *1.3) + (100* 7) = 1.350 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 *1.3) + (500* 7) = 4.150 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 *10) + (100* 48) = 9.800 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 *10) + (500* 48) = 29.000 RU/s</p></td>
        </tr>
    </tbody>
</table>

### <a name="use-the-request-unit-calculator"></a>Verwenden des Rechners für Anforderungseinheiten
Damit Kunden ihre Durchsatzschätzungen optimieren können, gibt es einen webbasierten [Rechner für Anforderungseinheiten](https://www.documentdb.com/capacityplanner) , um den Bedarf an Anforderungseinheiten für normale Vorgänge zu schätzen, einschließlich:

* Erstellen von Dokumenten (Schreiben)
* Lesen von Dokumenten
* Löschen von Dokumenten
* Aktualisieren von Dokumenten

Das Tool unterstützt auch die Schätzung des Datenspeicherbedarfs auf der Grundlage der bereitgestellten Beispieldokumente.

Die Verwendung des Tools ist einfach:

1. Laden Sie mindestens ein repräsentatives JSON-Dokument hoch.
   
    ![Hochladen von Dokumenten in den Rechner für Anforderungseinheiten][2]
2. Geben Sie zum Schätzen des Speicherplatzbedarfs die Gesamtanzahl von Dokumenten ein, die Sie voraussichtlich speichern werden.
3. Geben Sie die pro Sekunde benötige Anzahl von Erstellungs-, Lese-, Aktualisierungs- und Löschvorgängen für Dokumente an. Laden Sie zum Ermitteln der bei Dokumentaktualisierungen voraussichtlich anfallenden Gebühren für Anforderungseinheiten eine Kopie des Beispieldokuments aus Schritt 1 mit typischen Feldaktualisierungen hoch.  Wenn bei Dokumentaktualisierungen also üblicherweise die beiden Eigenschaften „lastLogin“ und „userVisits“ geändert werden, kopieren Sie einfach das Beispieldokument, aktualisieren Sie die Werte für diese beiden Eigenschaften, und laden Sie das kopierte Dokument anschließend hoch.
   
    ![Eingeben der Durchsatzanforderungen in den Rechner für Anforderungseinheiten][3]
4. Klicken Sie auf „Berechnen“, und prüfen Sie die Ergebnisse.
   
    ![Ergebnisse des Rechners für Anforderungseinheiten][4]

> [!NOTE]
> Wenn sich die Dokumenttypen im Hinblick auf Größe und Anzahl indizierter Eigenschaften erheblich voneinander unterscheiden, laden Sie ein Beispiel für jeden *Typ* eines normalen Dokuments an das Tools hoch, und berechnen Sie dann die Ergebnisse.
> 
> 

### <a name="use-the-documentdb-request-charge-response-header"></a>Verwenden des DocumentDB-Antwortheaders „request-charge“
Jede Antwort des DocumentDB-Diensts enthält einen benutzerdefinierten Header (`x-ms-request-charge`), der die für die Anforderung verbrauchten Anforderungseinheiten enthält. Auf diesen Header kann auch über die DocumentDB-SDKs zugegriffen werden. Im .Net SDK ist „RequestCharge“ eine Eigenschaft des ResourceResponse-Objekts.  Für Abfragen stellt der DocumentDB-Abfrage-Explorer im Azure-Portal Informationen zu Anforderungsgebühren für ausgeführten Abfragen bereit.

![Untersuchen der berechneten RUs im Abfrage-Explorer][1]

Vor diesem Hintergrund besteht eine Methode zum Abschätzen des von der Anwendung benötigten Durchsatzes darin, typische Vorgänge mit einem repräsentativen, von Ihrer Anwendung verwendeten Dokument auszuführen, dabei die berechneten Anforderungseinheiten zu notieren und anschließend die Anzahl von Vorgängen zu schätzen, die erwartungsgemäß pro Sekunde ausgeführt werden.  Stellen Sie sicher, dass auch typische Abfragen und die Nutzung von DocumentDB-Skripts gemessen und berücksichtigt werden.

> [!NOTE]
> Wenn sich die Dokumenttypen im Hinblick auf Größe und Anzahl indizierter Eigenschaften erheblich voneinander unterscheiden, erfassen Sie für jeden typischen Dokumenttyp** jeweils die berechneten Anforderungseinheiten des jeweiligen Vorgangs.
> 
> 

Beispiel:

1. Notieren Sie die berechneten Anforderungseinheiten für das Erstellen (Einfügen) eines typischen Dokuments. 
2. Notieren Sie die berechneten Anforderungseinheiten für das Lesen eines typischen Dokuments.
3. Notieren Sie die berechneten Anforderungseinheiten für das Aktualisieren eines typischen Dokuments.
4. Notieren Sie die berechneten Anforderungseinheiten für typische, häufig ausgeführte Dokumentabfragen.
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

Vor diesem Hintergrund besteht eine Methode zum Abschätzen des von der Anwendung benötigten Durchsatzes darin, typische Vorgänge mit einem repräsentativen, von Ihrer Anwendung verwendeten Dokument auszuführen, dabei die berechneten Anforderungseinheiten zu notieren und anschließend die Anzahl von Vorgängen zu schätzen, die erwartungsgemäß pro Sekunde ausgeführt werden.

> [!NOTE]
> Wenn sich die Dokumenttypen im Hinblick auf Größe und Anzahl indizierter Eigenschaften erheblich voneinander unterscheiden, erfassen Sie für jeden typischen Dokumenttyp** jeweils die berechneten Anforderungseinheiten des jeweiligen Vorgangs.
> 
> 

## <a name="use-api-for-mongodbs-portal-metrics"></a>Verwenden von Portalmetriken von API für MongoDB
Die einfachste Möglichkeit, eine recht genaue Schätzung der Gebühren für Anforderungseinheiten für Ihre API für MongoDB-Datenbank zu erhalten, ist die Verwendung der Metriken im [Azure-Portal](https://portal.azure.com). Mit den Diagrammen *Anzahl von Anforderungen* und *Anforderungsgebühr* können Sie abschätzen, wie viele Anforderungseinheiten von jedem Vorgang verbraucht werden und wie viele Einheiten in Relation der Vorgänge zueinander verbraucht werden.

![Portalmetriken von API für MongoDB][6]

## <a name="a-request-unit-estimation-example"></a>Beispiel für die Schätzung von Anforderungseinheiten
Betrachten Sie das folgende Dokument von&1; KB:

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
> Dokumente werden in DocumentDB minimiert, daher beträgt die vom System berechnete Größe des obigen Dokuments etwas weniger als 1 KB.
> 
> 

Die folgende Tabelle zeigt die ungefähre Anzahl berechneter Anforderungseinheiten für normale Vorgänge für dieses Dokument (bei der ungefähren Anzahl berechneter Anforderungseinheiten wird angenommen, dass die Kontokonsistenzebene auf „Session“ festgelegt ist und dass alle Dokumente automatisch indiziert werden):

| Vorgang | Berechnete Anforderungseinheiten |
| --- | --- |
| Dokument erstellen |~15 RUs |
| Dokument lesen |~1 RU |
| Dokument abfragen nach ID |~2,5 RUs |

Diese Tabelle zeigt darüber hinaus die ungefähre Anzahl berechneter Anforderungseinheiten für normale Abfragen, die in der Anwendung verwendet werden:

| Abfrage | Berechnete Anforderungseinheiten | # zurückgegebener Dokumente |
| --- | --- | --- |
| Nahrungsmittel nach ID auswählen |~2,5 RUs |1 |
| Nahrungsmittel nach Hersteller auswählen |~7 RUs |7 |
| Nach Nahrungsmittelgruppe auswählen und nach Gewicht bestellen |~70 RUs |100 |
| Die 10 beliebtesten Nahrungsmittel in einer Nahrungsmittelgruppe auswählen |~10 RUs |10 |

> [!NOTE]
> RU-Gebühren richten sich nach der Anzahl zurückgegebener Dokumente.
> 
> 

Mit diesen Informationen können wir den RU-Bedarf für diese Anwendung angesichts der Anzahl erwarteter Vorgänge und Abfragen pro Sekunde abschätzen:

| Vorgang/Abfrage | Geschätzte Anzahl pro Sekunde | Erforderliche RUs |
| --- | --- | --- |
| Dokument erstellen |10 |150 |
| Dokument lesen |100 |100 |
| Nahrungsmittel nach Hersteller auswählen |25 |175 |
| Nach Nahrungsmittelgruppe auswählen |10 |700 |
| 10 beliebteste auswählen |15 |150 insgesamt |

In diesem Fall erwarten wir einen durchschnittlichen Durchsatzbedarf von 1,275 RU/s.  Wir runden auf den nächsten Hunderter auf und würden für die Sammlung dieser Anwendung 1.300 RU/s bereitstellen.

## <a id="RequestRateTooLarge"></a> Überschreiten von Grenzwerten für den reservierten Durchsatz in DocumentDB
Der Verbrauch von Anforderungseinheiten wird als Rate pro Sekunde bemessen. Für Anwendungen, die die bereitgestellte Anforderungseinheitsrate für eine Sammlung überschreiten, werden Anforderungen an die Sammlung gedrosselt, bis die Rate unter das reservierte Niveau fällt. Bei einer Drosselung beendet der Server die Anforderung präemptiv mit „RequestRateTooLargeException“ (HTTP-Statuscode 429) und gibt den x-ms-retry-after-ms-Header zurück. Darin ist die Zeitspanne in Millisekunden angegeben, die der Benutzer abwarten muss, bevor ein neuer Anforderungsversuch unternommen werden kann.

    HTTP Status 429
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100

Wenn Sie das .NET Client SDK und LINQ-Abfragen verwenden, werden Sie sich normalerweise nicht mit dieser Ausnahme beschäftigen müssen, da die aktuelle Version des .NET Client SDK diese Antwort implizit abfängt, den vom Server angegebenen Retry-After-Header beachtet und die Anforderung wiederholt. Wenn nicht mehrere Clients gleichzeitig auf Ihr Konto zugreifen, wird die nächste Wiederholung erfolgreich ausgeführt.

Wenn mehrere Clients kumulativ oberhalb der Anforderungsrate arbeiten, reicht das Standard-Wiederholungsverhalten möglicherweise nicht aus, und der Client löst für die Anwendung eine DocumentClientException mit dem Statuscode 429 aus. In diesen Fällen sollten Sie in Betracht ziehen, das Wiederholungsverhalten und die zugehörige Logik in die Anwendungsroutinen zur Fehlerbehandlung aufzunehmen oder den reservierten Durchsatz für die Sammlung zu erhöhen.

## <a id="RequestRateTooLargeAPIforMongoDB"></a> Überschreiten von Grenzwerten für den reservierten Durchsatz in API für MongoDB
Anwendungen, die die bereitgestellten Anforderungseinheiten für eine Sammlung überschreiten, werden gedrosselt, bis die Rate unter das reservierte Niveau fällt. Wenn eine Drosselung eintritt, beendet das Back-End die Anforderung präventiv mit einem *16500*-Fehlercode – *Zu viele Anforderungen*. API für MongoDB versucht standardmäßig automatisch bis zu 10-mal, eine Anforderung erneut zu senden, bevor der Fehlercode *Zu viele Anforderungen* zurückgegeben wird. Wenn Sie zu viele Fehlercodes *Zu viele Anforderungen* erhalten, sollten Sie in Betracht ziehen, das Wiederholungsverhalten in die Fehlerbehandlungsroutinen Ihrer Anwendung aufzunehmen oder den [reservierten Durchsatz für die Sammlung zu erhöhen](documentdb-set-throughput.md).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum reservierten Durchsatz mit Azure DocumentDB-Datenbanken finden Sie in folgenden Ressourcen:

* [DocumentDB-Preise](https://azure.microsoft.com/pricing/details/documentdb/)
* [Modellieren von Daten in DocumentDB](documentdb-modeling-data.md)
* [Leistungsebenen in DocumentDB](documentdb-partition-data.md)

Weitere Informationen zu DocumentDB finden Sie in der Azure DocumentDB- [Dokumentation](https://azure.microsoft.com/documentation/services/documentdb/). 

Im Artikel [Leistungs- und Skalierungstests mit Azure DocumentDB](documentdb-performance-testing.md)finden Sie eine Einführung in Leistungs- und Skalierungstests mit DocumentDB.

[1]: ./media/documentdb-request-units/queryexplorer.png 
[2]: ./media/documentdb-request-units/RUEstimatorUpload.png
[3]: ./media/documentdb-request-units/RUEstimatorDocuments.png
[4]: ./media/documentdb-request-units/RUEstimatorResults.png
[5]: ./media/documentdb-request-units/RUCalculator2.png
[6]: ./media/documentdb-request-units/api-for-mongodb-metrics.png

