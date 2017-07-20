---
title: "Bereitstellen von Durchsatz für Azure Cosmos DB | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie bereitgestellten Durchsatz für Ihre Azure Cosmos DB-Container, -Sammlungen, -Grafiken und -Tabellen festlegen."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: f98def7f-f012-4592-be03-f6fa185e1b1e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2017
ms.author: mimig
ms.translationtype: Human Translation
ms.sourcegitcommit: 5bbeb9d4516c2b1be4f5e076a7f63c35e4176b36
ms.openlocfilehash: d541bb19ba7e5ecb44c9fe91b1e232d4d9c2170e
ms.contentlocale: de-de
ms.lasthandoff: 06/13/2017


---

# <a name="set-throughput-for-azure-cosmos-db-containers"></a>Festlegen von Durchsatz für Azure Cosmos DB-Container

Sie können den Durchsatz für Ihre Azure Cosmos DB-Container im Azure-Portal oder mithilfe der Clients-SDKs festlegen. 

In der folgenden Tabelle sind die für Container verfügbaren Durchsätze aufgeführt:

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p></p></td>
            <td valign="top"><p><strong>Container mit nur einer Partition</strong></p></td>
            <td valign="top"><p><strong>Partitionierte Container</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Minimaler Durchsatz</p></td>
            <td valign="top"><p>400 Anforderungseinheiten pro Sekunde</p></td>
            <td valign="top"><p>2.500 Anforderungseinheiten pro Sekunde</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Maximaler Durchsatz</p></td>
            <td valign="top"><p>10.000 Anforderungseinheiten pro Sekunde</p></td>
            <td valign="top"><p>Unbegrenzt</p></td>
        </tr>
    </tbody>
</table>

## <a name="to-set-the-throughput-by-using-the-azure-portal"></a>Festlegen des Durchsatzes mithilfe des Azure-Portals

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com) in einem neuen Fenster.
2. Klicken Sie in der linken Leiste auf **Azure Cosmos DB**, oder klicken Sie unten auf **Weitere Dienste**, scrollen Sie zu **Datenbanken**, und klicken Sie dann auf **Azure Cosmos DB**.
3. Wählen Sie Ihr Cosmos DB-Konto aus.
4. Klicken Sie im neuen Fenster im Navigationsmenü auf **Daten-Explorer (Vorschau)**.
5. Erweitern Sie im neuen Fenster die Datenbank und den Container, und klicken Sie dann auf **Skalierungseinstellungen**.
6. Geben Sie im neuen Fenster den neuen Durchsatzwert im Feld **Durchsatz** ein, und klicken Sie dann auf **Speichern**.

<a id="set-throughput-sdk"></a>

## <a name="to-set-the-throughput-by-using-the-documentdb-api-for-net"></a>So legen Sie den Durchsatz mithilfe der DocumentDB-API für .NET fest

```C#
//Fetch the resource to be updated
Offer offer = client.CreateOfferQuery()
    .Where(r => r.ResourceLink == collection.SelfLink)    
    .AsEnumerable()
    .SingleOrDefault();

// Set the throughput to the new value, for example 12,000 request units per second
offer = new OfferV2(offer, 12000);

//Now persist these changes to the database by replacing the original resource
await client.ReplaceOfferAsync(offer);
```

## <a name="throughput-faq"></a>Häufig gestellte Fragen zum Durchsatz

**Kann der Durchsatz auf unter 400 RU/s festgelegt werden?**

400 RU/s ist der in Cosmos DB verfügbare minimale Durchsatz für Sammlungen mit nur einer Partition (2500 RU/s ist der minimale Durchsatz für partitionierte Sammlungen). Die Anforderungseinheiten werden in Intervallen von 100 RU/s festgelegt, der Durchsatz kann jedoch nicht auf 100 RU/s oder einen anderen Wert unter 400 RU/s festgelegt werden. Wenn Sie nach einer kostengünstigen Methode zum Entwickeln und Testen von Cosmos DB suchen, können Sie den kostenlosen [Azure Cosmos DB-Emulator](local-emulator.md) verwenden, der lokal kostenfrei bereitgestellt werden kann. 

**Wie kann ich den Durchsatz mit der MongoDB-API festlegen?**

Es gibt keine MongoDB-API-Erweiterung zum Festlegen des Durchsatzes. Es wird empfohlen, die DocumentDB-API zu verwenden, wie unter [So legen Sie den Durchsatz mithilfe der DocumentDB-API für .NET fest](#set-throughput-sdk) beschrieben.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Bereitstellung und zur weltweiten Skalierung mit Cosmos DB finden Sie unter [Partitionieren und Skalieren von Daten in Cosmos DB](partition-data.md).

