---
title: "Bereitstellen von Durchsatz für Azure Cosmos DB | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie bereitgestellten Durchsatz für Ihre Azure Cosmos DB-Sammlung festlegen."
services: cosmosdb
author: mimig1
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: f98def7f-f012-4592-be03-f6fa185e1b1e
ms.service: cosmosdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2017
ms.author: mimig
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 5258ba0bc37442c983d91c5dd7435fd5fbefd56f
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---

# <a name="set-throughput-for-azure-cosmos-db-collections"></a>Festlegen von Durchsatz für Azure Cosmos DB-Sammlungen

Sie können den Durchsatz für Ihre Azure Cosmos DB-Sammlungen im Azure-Portal oder mithilfe der Clients-SDKs festlegen. 

In der folgenden Tabelle sind die für Sammlungen verfügbaren Durchsätze aufgeführt:

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p></p></td>
            <td valign="top"><p><strong>Sammlung mit einer Partition</strong></p></td>
            <td valign="top"><p><strong>Partitionierte Sammlung</strong></p></td>
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

> [!NOTE] 
> Um für partitionierte Sammlungen einen Durchsatzwert zwischen 2.500 RU/s und 10.000 RU/s festzulegen, müssen Sie vorübergehend das Azure-Portal verwenden. Diese Funktion ist in den SDKs noch nicht verfügbar.

## <a name="to-set-the-throughput-by-using-the-azure-portal"></a>Festlegen des Durchsatzes mithilfe des Azure-Portals

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com) in einem neuen Fenster.
2. Klicken Sie in der linken Leiste auf **Azure Cosmos DB**, oder klicken Sie unten auf **Weitere Dienste**, scrollen Sie zu **Datenbanken**, und klicken Sie dann auf **Azure Cosmos DB**.
3. Wählen Sie Ihr Cosmos DB-Konto aus.
4. Klicken Sie wie im folgenden Screenshot gezeigt im neuen Fenster unter **Sammlungen** auf **Scale** (Staffelung).
5. Wählen Sie im neuen Fenster aus der Dropdownliste Ihre Sammlung aus, ändern Sie den Wert **Durchsatz**, und klicken Sie anschließend auf **Speichern**.

    ![Screenshot, der zeigt, wie der Durchsatz für eine Sammlung im Azure-Portal geändert wird, indem Sie zu Ihrem Konto wechseln und auf „Scale“ (Staffelung) klicken](./media/documentdb-set-throughput/azure-documentdb-change-throughput-value.png)

<a id="set-throughput-sdk"></a>

## <a name="to-set-the-throughput-by-using-the-net-sdk"></a>Festlegen des Durchsatzes mithilfe des .NET SDK

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

400 RU/s ist der in Cosmos DB verfügbare minimale Durchsatz für Sammlungen mit nur einer Partition (2500 RU/s ist der minimale Durchsatz für partitionierte Sammlungen). Die Anforderungseinheiten werden in Intervallen von 100 RU/s festgelegt, der Durchsatz kann jedoch nicht auf 100 RU/s oder einen anderen Wert unter 400 RU/s festgelegt werden. Wenn Sie nach einer kostengünstigen Methode zum Entwickeln und Testen von Cosmos DB suchen, können Sie den kostenlosen [Azure Cosmos DB-Emulator](documentdb-nosql-local-emulator.md) verwenden, der lokal kostenfrei bereitgestellt werden kann. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Bereitstellung und zur weltweiten Skalierung mit Cosmos DB finden Sie unter [Partitionieren und Skalieren von Daten in Cosmos DB](documentdb-partition-data.md).

