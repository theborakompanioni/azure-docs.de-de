---
title: "Azure-Abrechnungs-APIs für Unternehmen – Verwendungsdetails| Microsoft-Dokumentation"
description: "Erfahren Sie mehr über die Azure-Abrechnungs-APIs für Nutzung und Gebührenkarte, mit denen Sie Einblicke in den Azure-Ressourcenverbrauch und damit verbundene Trends erlangen können."
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
ms.openlocfilehash: d6fe7a020100e3372c1fa0e244838d4c40839dbf
ms.contentlocale: de-de
ms.lasthandoff: 05/03/2017


---
# <a name="reporting-apis-for-enterprise-customers---usage-details-preview"></a>Berichterstellungs-APIs für Unternehmenskunden – Verwendungsdetails (Vorschau)

API für Verwendungsdetails bietet eine tägliche Aufschlüsselung der verbrauchten Mengen und durch eine Registrierung anfallenden geschätzten Kosten. Das Ergebnis umfasst auch Informationen zu Instanzen, Verbrauchseinheiten und Abteilungen. Die API kann nach Abrechnungszeitraum oder einem angegebenen Start- und Enddatum abgefragt werden. 
## <a name="consumption-apis"></a>APIs zur Nutzung


##<a name="request"></a>Anforderung 
Allgemeine Headereigenschaften, die hinzugefügt werden müssen, werden [hier](billing-enterprise-api.md) angegeben. Wenn kein Abrechnungszeitraum angegeben wurde, werden die Daten für den aktuellen Abrechnungszeitraum zurückgegeben. Benutzerdefinierte Zeitabschnitte können mit den Parameter für Start- und Enddatum im Format JJJJ-MM-TT angegeben werden. Der maximal unterstützte Zeitraum beträgt 36 Monate.  

|Methode | Anforderungs-URI|
|-|-|
|GET|https://consumption.azure.com/v1/enrollments/{enrollmentNumber}/usagedetails 
|GET|https://consumption.azure.com/v1/enrollments/{enrollmentNumber}/billingPeriods/{billingPeriod}/usagedetails|
|GET|https://consumption.azure.com/v1/enrollments/{enrollmentNumber}/usagedetailsbycustomdate?startTime=2017-01-01&endTime=2017-01-10|

## <a name="response"></a>Antwort

> Aufgrund der potenziell großen Datenmenge ist das Resultset ausgelagert. Die Eigenschaft „nextLink“ (sofern vorhanden) gibt den Link für die nächste Seite der Daten an. Wenn der Link leer ist, weist dies darauf hin, dass es sich um die letzte Seite handelt. 
<br/>

    {
        "id": "string",
        "data": [
            {                        
            "accountId": 0,
            "productId": 0,
            "resourceLocationId": 0,
            "consumedServiceId": 0,
            "departmentId": 0,
            "accountOwnerEmail": "string",
            "accountName": "string",
            "serviceAdministratorId": "string",
            "subscriptionId": 0,
            "subscriptionGuid": "string",
            "subscriptionName": "string",
            "date": "2017-04-27T23:01:43.799Z",
            "product": "string",
            "meterId": "string",
            "meterCategory": "string",
            "meterSubCategory": "string",
            "meterRegion": "string",
            "meterName": "string",
            "consumedQuantity": 0,
            "resourceRate": 0,
            "extendedCost": 0,
            "resourceLocation": "string",
            "consumedService": "string",
            "instanceId": "string",
            "serviceInfo1": "string",
            "serviceInfo2": "string",
            "additionalInfo": "string",
            "tags": "string",
            "storeServiceIdentifier": "string",
            "departmentName": "string",
            "costCenter": "string",
            "unitOfMeasure": "string",
            "resourceGroup": "string"
            }
        ]
        "nextLink": "string"
    }

<br/>

**Definitionen der Antworteigenschaft**

|Eigenschaftenname| Typ| Beschreibung
|-|-|-|
|id| string| Eindeutige ID für den API-Aufruf |
|data| JSON-Array| Array der täglichen Verwendungsdetails für jede Instanz\Verbrauchseinheit|
|nextLink| string| Wenn weitere Seiten mit Daten vorhanden sind, verweist „nextLink“ auf die URL, mit der die nächste Seite zurückgegeben wird |

## <a name="see-also"></a>Weitere Informationen
* [API für Abrechnungszeiträume](billing-enterprise-api-billing-periods.md)

* [API für Bilanz und Zusammenfassung](billing-enterprise-api-balance-summary.md)

* [API für Marketplace Store-Gebühren](billing-enterprise-api-marketplace-storecharge.md) 

* [Preisblatt-API](billing-enterprise-api-pricesheet.md)
