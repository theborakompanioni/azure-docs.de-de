---
title: "Azure-Abrechnungs-APIs für Unternehmen – PriceSheet | Microsoft-Dokumentation"
description: "Erhalten Sie Informationen zu den Berichterstellungs-APIs, mit denen Azure-Unternehmenskunden Verbrauchsdaten programmgesteuert abrufen können."
services: 
documentationcenter: 
author: aedwin
manager: aedwin
editor: 
tags: billing
ms.assetid: 3e817b43-0696-400c-a02e-47b7817f9b77
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 04/25/2017
ms.author: aedwin
ms.translationtype: Human Translation
ms.sourcegitcommit: f6006d5e83ad74f386ca23fe52879bfbc9394c0f
ms.openlocfilehash: e5deb660ae5cfed5ac64000d070f4dd2e42ec9c0
ms.contentlocale: de-de
ms.lasthandoff: 05/03/2017


---
# <a name="reporting-apis-for-enterprise-customers---pricesheet-preview"></a>Berichterstellungs-APIs für Unternehmenskunden – PriceSheet (Vorschau)

Die Preisblatt-API stellt die zutreffende Rate für jede Verbrauchseinheit für den angegebenen Registrierungs- und Abrechnungszeitraum bereit.

##<a name="request"></a>Anforderung
Allgemeine Headereigenschaften, die hinzugefügt werden müssen, werden [hier](billing-enterprise-api.md) angegeben. Wenn kein Abrechnungszeitraum angegeben wurde, werden die Daten für den aktuellen Abrechnungszeitraum zurückgegeben.

|Methode | Anforderungs-URI|
|-|-|
|GET|https://consumption.azure.com/v1/enrollments/{enrollmentNumber}/pricesheet|
|GET|https://consumption.azure.com/v1/enrollments/{enrollmentNumber}/billingPeriods/{billingPeriod}/pricesheet|


## <a name="response"></a>Antwort

    
          [
            {
                  "id": "enrollments/57354989/billingperiods/201601/products/343/pricesheets",
                  "billingPeriodId": "201704",
                  "meterName": "A1 VM",
                  "unitOfMeasure": "100 Hours",
                  "includedQuantity": 0,
                  "partNumber": "N7H-00015",
                  "unitPrice": 0.00,
                  "currencyCode": "USD"
            },
            {
                  "id": "enrollments/57354989/billingperiods/201601/products/2884/pricesheets",
                  "billingPeriodId": "201404",
                  "meterName": "Locally Redundant Storage Premium Storage - Snapshots - AU East",
                  "unitOfMeasure": "100 GB",
                  "includedQuantity": 0,
                  "partNumber": "N9H-00402",
                  "unitPrice": 0.00,
                  "currencyCode": "USD"
            },
            ...
        ]
    

**Definitionen der Antworteigenschaft**

|Eigenschaftenname| Typ| Beschreibung
|-|-|-|
|id| string| Eindeutige ID, die ein bestimmtes PriceSheet-Element (Verbrauchseinheit nach Abrechnungszeitraum) darstellt|
|billingPeriodId| string| Eindeutige ID, die einen bestimmten Abrechnungszeitraum darstellt|
|meterName| string| Name der Verbrauchseinheit|
|unitOfMeasure| string| Berechnungseinheit zur Messung des Diensts|
|includedQuantity| Decimal| Enthaltene Menge |
|partNumber| string| Teilenummer, die der Verbrauchseinheit zugeordnet ist|
|unitPrice| Decimal| Preis je Einheit für die Verbrauchseinheit|
|currencyCode| string| Währungscode für unitPrice|
<br/>
## <a name="see-also"></a>Weitere Informationen

* [API für Abrechnungszeiträume](billing-enterprise-api-billing-periods.md)

* [API für Verwendungsdetails](billing-enterprise-api-usage-detail.md)

* [API für Bilanz und Zusammenfassung](billing-enterprise-api-balance-summary.md)

* [API für Marketplace Store-Gebühren](billing-enterprise-api-marketplace-storecharge.md)

