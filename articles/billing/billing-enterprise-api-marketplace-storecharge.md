---
title: "Azure-Abrechnungs-APIs für Unternehmen – Marketplace-Gebühren | Microsoft-Dokumentation"
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
ms.translationtype: HT
ms.sourcegitcommit: 6e76ac40e9da2754de1d1aa50af3cd4e04c067fe
ms.openlocfilehash: 5539623f7ae35e14b6dafe6fdf9efe4bcaba4fd3
ms.contentlocale: de-de
ms.lasthandoff: 07/31/2017

---
# <a name="reporting-apis-for-enterprise-customers---marketplace-store-charge"></a>Berichterstellungs-APIs für Unternehmenskunden – Marketplace Store-Gebühren

Die API für Marketplace Store-Gebühren gibt die Aufschlüsselung der nutzungsbasierten Marketplace-Gebühren nach Tag für den angegebenen Abrechnungszeitraum oder die Start- und Enddaten zurück (einmalige Gebühren sind nicht enthalten).

##<a name="request"></a>Anforderung 
Allgemeine Headereigenschaften, die hinzugefügt werden müssen, werden [hier](billing-enterprise-api.md) angegeben. Wenn kein Abrechnungszeitraum angegeben wurde, werden die Daten für den aktuellen Abrechnungszeitraum zurückgegeben. Benutzerdefinierte Zeitabschnitte können mit den Parameter für Start- und Enddatum im Format JJJJ-MM-TT angegeben werden, wobei für den Zeitbereich maximal 36 Monate unterstützt werden.  

|Methode | Anforderungs-URI|
|-|-|
|GET|https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/marketplacecharges|
|GET|https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/billingPeriods/{billingPeriod}/marketplacecharges|
|GET|https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/marketplacechargesbycustomdate?startTime=2017-01-01&endTime=2017-01-10|

> [!Note]
> Ersetzen Sie in der obigen URL v2 durch v1, um die Vorschauversion der API zu verwenden.
>

## <a name="response"></a>Antwort
 
    
        [
            {
                "id": "id",
                "subscriptionGuid": "00000000-0000-0000-0000-000000000000",
                "subscriptionName": "subName",
                "meterId": "2core",
                "usageStartDate": "2015-09-17T00:00:00Z",
                "usageEndDate": "2015-09-17T23:59:59Z",
                "offerName": "Virtual LoadMaster™ (VLM) for Azure",
                "resourceGroup": "Res group",
                "instanceId": "id",
                "additionalInfo": "{\"ImageType\":null,\"ServiceType\":\"Medium\"}",
                "tags": "",
                "orderNumber": "order",
                "unitOfMeasure": "",
                "costCenter": "100",
                "accountId": 100,
                "accountName": "Account Name",
                "accountOwnerId": "account@live.com",
                "departmentId": 101,
                "departmentName": "Department 1",
                "publisherName": "Publisher 1",
                "planName": "Plan name",
                "consumedQuantity": 1.15,
                "resourceRate": 0.1,
                "extendedCost": 1.11
            },
            ...
        ]
    

**Definitionen der Antworteigenschaft**

|Eigenschaftenname| Typ| Beschreibung
|-|-|-|
|id|string|Eindeutige ID für das Marketplace-Gebührenelement|
|subscriptionGuid|Guid|GUID für das Abonnement|
|subscriptionName|string|Bezeichnung für das Abonnement|
|meterId|string|ID für die ausgegebene Verbrauchseinheit|
|usageStartDate|DateTime|Startzeit für den Verwendungseintrag|
|usageEndDate|DateTime|Endzeit für den Verwendungseintrag|
|offerName|string|Angebotsname|
|Ressourcengruppe|string|Ressourcengruppe|
|instanceId|string|Instanzen-ID|
|additionalInfo|string|Zusätzliche Informationen zur JSON-Zeichenfolge|
|tags|string|JSON-Zeichenfolge für Tag|
|orderNumber|string|Bestellnummer|
|unitOfMeasure|string|Berechnungseinheit für die Verbrauchseinheit|
|costCenter|string|Kostenstelle|
|accountId|int|Konto-ID|
|accountName|string |Kontoname|
|accountOwnerId|string|ID des Kontobesitzers|
|departmentId|int|Abteilungs-ID|
|departmentName|string|Abteilungsname|
|publisherName|string|Herausgebername|
|planName|string|Planname|
|consumedQuantity|Decimal|Während dieses Zeitraums verbrauchte Menge|
|resourceRate|Decimal|Preis je Einheit für die Verbrauchseinheit|
|extendedCost|Decimal|Geschätzte Gebühren auf Basis der verbrauchten Menge und den erweiterten Kosten|
<br/>
## <a name="see-also"></a>Weitere Informationen

* [API für Abrechnungszeiträume](billing-enterprise-api-billing-periods.md)

* [API für Verwendungsdetails](billing-enterprise-api-usage-detail.md) 

* [API für Bilanz und Zusammenfassung](billing-enterprise-api-balance-summary.md)

* [Preisblatt-API](billing-enterprise-api-pricesheet.md)
