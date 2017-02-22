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
ms.date: 02/08/2017
ms.author: mimig
translationtype: Human Translation
ms.sourcegitcommit: 9919f9430626b02635272599e7d1f3f09e0068cd
ms.openlocfilehash: ae83dc01113e72576a35aa57acbac3b71d87e9f8


---

# <a name="set-throughput-for-azure-documentdb-collections"></a>Festlegen von Durchsatz für Azure DocumentDB-Sammlungen

Sie können den Durchsatz für Ihre DocumentDB-Sammlungen im Azure-Portal oder mithilfe der Clients-SDKs festlegen. 

## <a name="to-set-the-throughput-by-using-the-azure-portal"></a>Festlegen des Durchsatzes mithilfe des Azure-Portals

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com) in einem neuen Fenster.
2. Klicken Sie in der linken Leiste auf **NoSQL (DocumentDB)**, oder klicken Sie unten auf **Weitere Dienste**, scrollen Sie zu **Datenbanken**, und klicken Sie dann auf **NoSQL (DocumentDB)**.
3. Wählen Sie Ihr DocumentDB-Konto aus.
4. Klicken Sie wie im folgenden Screenshot gezeigt im neuen Fenster unter **Sammlungen** auf **Scale** (Staffelung).
5. Wählen Sie im neuen Fenster aus der Dropdownliste Ihre Sammlung aus, ändern Sie den Wert **Durchsatz**, und klicken Sie anschließend auf **Speichern**.

    ![Screenshot, der zeigt, wie der Durchsatz für eine Sammlung im Azure-Portal geändert wird, indem Sie zu Ihrem Konto wechseln und auf „Scale“ (Staffelung) klicken](./media/documentdb-set-throughput/azure-documentdb-change-throughput-value.png)

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


