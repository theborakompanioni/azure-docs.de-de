---
title: "Bereitstellen von Durchsatz für Azure DocumentDB | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie bereitgestellten Durchsatz für Ihre DocumentDB-Sammlung festlegen."
services: documentdb
author: mimig1
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: f98def7f-f012-4592-be03-f6fa185e1b1e
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: mimig
translationtype: Human Translation
ms.sourcegitcommit: c65c4021f5d0beda8005abdd375ac51729acf0a8
ms.openlocfilehash: 8ff246ab7d70bd00ce989d3049e016d692722d18


---

# <a name="set-throughput-for-azure-documentdb-collections"></a>Festlegen von Durchsatz für Azure DocumentDB-Sammlungen

Sie können den Durchsatz für Ihre DocumentDB-Sammlungen im Azure-Portal oder mithilfe der Clients-SDKs festlegen. 

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
2. Klicken Sie in der linken Leiste auf **NoSQL (DocumentDB)**, oder klicken Sie unten auf **Weitere Dienste**, scrollen Sie zu **Datenbanken**, und klicken Sie dann auf **NoSQL (DocumentDB)**.
3. Wählen Sie Ihr DocumentDB-Konto aus.
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

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Bereitstellung und zur weltweiten Skalierung mit DocumentDB finden Sie unter [Partitionieren und Skalieren von Daten in DocumentDB](documentdb-partition-data.md).


<!--HONumber=Feb17_HO2-->


