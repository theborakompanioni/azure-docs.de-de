---
title: 'Gewusst wie: Skalieren der Azure Time Series Insights-Umgebung | Microsoft-Dokumentation'
description: "In diesem Tutorial erfahren Sie, wie Sie Ihre Azure Time Series Insights-Umgebung skalieren können."
keywords: 
services: time-series-insights
documentationcenter: 
author: sandshadow
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: tsi
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/19/2017
ms.author: edett
ms.translationtype: Human Translation
ms.sourcegitcommit: 7f8b63c22a3f5a6916264acd22a80649ac7cd12f
ms.openlocfilehash: 8f6c66ea2173c98179ec899d6626c2ab6f7ec4b6
ms.contentlocale: de-de
ms.lasthandoff: 05/01/2017

---
# <a name="how-to-scale-your-time-series-insights-environment"></a>Gewusst wie: Skalieren der Azure Time Series Insights-Umgebung

In diesem Tutorial erfahren Sie, wie Sie Ihre Time Series Insights-Umgebung skalieren können.

> [!NOTE]
> Ein zentrales Hochskalieren zwischen verschiedenen SKU-Typen ist nicht zulässig. Eine Umgebung mit der SKU „S1“ kann nicht in eine S2-Umgebung konvertiert werden.

## <a name="s1-sku-ingress-rates-and-capacities"></a>SKU „S1“: Erfassungsraten und Kapazitäten

| Kapazität der SKU „S1“ | Erfassungsrate | Maximale Speicherkapazität
| --- | --- | --- |
| 1 | 1 GB (1 Mio. Ereignisse) | 30 GB (30 Mio. Ereignisse) pro Monat |
| 10 | 10 GB (10 Mio. Ereignisse) | 300 GB (300 Mio. Ereignisse) pro Monat |

## <a name="s2-sku-ingress-rates-and-capacities"></a>SKU „S2“: Erfassungsraten und Kapazitäten

| Kapazität der SKU „S2“ | Erfassungsrate | Maximale Speicherkapazität
| --- | --- | --- |
| 1 | 10 GB (10 Mio. Ereignisse) | 300 GB (300 Mio. Ereignisse) pro Monat |
| 10 | 100 GB (100 Mio. Ereignisse) | 3 TB (3 Mrd. Ereignisse) pro Monat |

Kapazitäten werden linear skaliert, sodass eine SKU des Typs S1 mit Kapazität 2 als Erfassungsrate 2 GB (2 Mio.) Ereignisse pro Tag und 60 GB (60 Mio. Ereignisse) pro Monat unterstützt.

## <a name="changing-the-capacity-of-your-environment"></a>Ändern der Kapazität Ihrer Umgebung

1. Wählen Sie im Azure-Portal die Umgebung, deren Kapazität Sie ändern möchten.
1. Klicken Sie unter „Einstellungen“ auf „Konfigurieren“.
1. Bewegen Sie den Schieberegler „Kapazität“, um die Kapazität auszuwählen, die die Anforderungen an Erfassungsrate und Speicherkapazität erfüllt.

## <a name="next-steps"></a>Nächste Schritte

* Stellen Sie sicher, dass die neue Kapazität ausreichend ist, um eine Drosselung zu verhindern. Weitere Informationen finden Sie [hier](time-series-insights-diagnose-and-solve-problems.md) im Abschnitt *Ihre Umgebung wird möglicherweise gedrosselt*.
