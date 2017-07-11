---
title: "Hinzufügen eines Verweisdatasets zu Ihrer Azure Time Series Insights-Umgebung | Microsoft-Dokumentation"
description: "In diesem Tutorial fügen Sie Ihrer Time Series Insights-Umgebung ein Verweisdataset hinzu."
keywords: 
services: time-series-insights
documentationcenter: 
author: venkatgct
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: venkatja
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: 23444297b5231e6a026bcd9ce3ee9f943bf05867
ms.contentlocale: de-de
ms.lasthandoff: 07/04/2017

---

<a id="create-a-reference-data-set-for-your-time-series-insights-environment-using-the-ibiza-portal" class="xliff"></a>

# Erstellen eines Verweisdatasets für Ihre Time Series Insights-Umgebung über das Ibiza-Portal

Ein Verweisdataset ist eine Sammlung von Elementen, die mit den Ereignissen aus Ihrer Ereignisquelle ergänzt werden. Das Time Series Insights-Erfassungsmodul verknüpft ein Ereignis aus Ihrer Ereignisquelle mit einem Element in Ihrem Verweisdataset. Dieses ergänzte Ereignis ist dann für Abfragen verfügbar. Diese Verknüpfung basiert auf den in Ihrem Verweisdataset definierten Schlüsseln.

<a id="steps-to-add-a-reference-data-set-to-your-environment" class="xliff"></a>

## Schritte zum Hinzufügen eines Verweisdatasets zu Ihrer Umgebung

1. Melden Sie sich beim [Ibiza-Portal](https://portal.azure.com) an.
2. Klicken Sie im Ibiza-Portal im Menü auf der linken Seite auf „Alle Ressourcen“.
3. Wählen Sie Ihre Time Series Insights-Umgebung aus.

    ![Erstellen des Time Series Insights-Verweisdatasets](media/add-reference-data-set/getstarted-create-reference-data-set-1.png)

4. Wählen Sie „Verweisdatasets“, und klicken Sie auf „+ Hinzufügen“.

    ![Erstellen des Time Series Insights-Verweisdatasets – Details](media/add-reference-data-set/getstarted-create-reference-data-set-2.png)

5. Geben Sie den Namen des Verweisdatasets an.
6. Geben Sie den Namen des Schlüssels und dessen Typ an. Dieser Name und Typ wird verwendet, um die richtige Eigenschaft aus dem Ereignis in der Ereignisquelle auszuwählen. Wenn Sie beispielsweise den Schlüsselnamen „DeviceId“ und den Typ „Zeichenfolge“ angeben, sucht das Time Series Insights-Erfassungsmodul im eingehenden Ereignis nach einer Eigenschaft mit dem Namen „DeviceId“ vom Typ „String“. Sie können mehrere Schlüssel zur Verknüpfung mit dem Ereignis angeben. Beim Vergleichen der Eigenschaftsnamen wird die Groß-/Kleinschreibung beachtet.

     ![Erstellen des Time Series Insights-Verweisdatasets – Details](media/add-reference-data-set/getstarted-create-reference-data-set-3.png)

7. Klicken Sie auf „Erstellen“.

<a id="next-steps" class="xliff"></a>

## Nächste Schritte

* [Verwalten Sie Verweisdaten](time-series-insights-manage-reference-data-csharp.md) programmgesteuert.
* Die vollständige API-Referenz finden Sie im Dokument zur [Verweisdaten-API](/rest/api/time-series-insights/time-series-insights-reference-reference-data-api).
