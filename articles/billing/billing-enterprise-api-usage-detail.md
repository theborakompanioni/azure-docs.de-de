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
ms.translationtype: HT
ms.sourcegitcommit: 6e76ac40e9da2754de1d1aa50af3cd4e04c067fe
ms.openlocfilehash: 0f751063510707d53ac76a287aa420818a31b04b
ms.contentlocale: de-de
ms.lasthandoff: 07/31/2017

---
# <a name="reporting-apis-for-enterprise-customers---usage-details"></a>Berichterstellungs-APIs für Unternehmenskunden – Verwendungsdetails

API für Verwendungsdetails bietet eine tägliche Aufschlüsselung der verbrauchten Mengen und durch eine Registrierung anfallenden geschätzten Kosten. Das Ergebnis umfasst auch Informationen zu Instanzen, Verbrauchseinheiten und Abteilungen. Die API kann nach Abrechnungszeitraum oder einem angegebenen Start- und Enddatum abgefragt werden. 
## <a name="consumption-apis"></a>APIs zur Nutzung


##<a name="request"></a>Anforderung 
Allgemeine Headereigenschaften, die hinzugefügt werden müssen, werden [hier](billing-enterprise-api.md) angegeben. Wenn kein Abrechnungszeitraum angegeben wurde, werden die Daten für den aktuellen Abrechnungszeitraum zurückgegeben. Benutzerdefinierte Zeitabschnitte können mit den Parameter für Start- und Enddatum im Format JJJJ-MM-TT angegeben werden. Der maximal unterstützte Zeitraum beträgt 36 Monate.  

|Methode | Anforderungs-URI|
|-|-|
|GET|https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/usagedetails 
|GET|https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/billingPeriods/{billingPeriod}/usagedetails|
|GET|https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/usagedetailsbycustomdate?startTime=2017-01-01&endTime=2017-01-10|

> [!Note]
> Um die Vorschauversion der API zu verwenden, ersetzen Sie in der obigen URL v2 durch v1.
>

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
            "Cost": 0,
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
        ],
        "nextLink": "string"
    }

<br/>
**Definitionen der Antworteigenschaft**

|Eigenschaftenname| Typ| Beschreibung
|-|-|-|
|id| string| Eindeutige ID für den API-Aufruf |
|data| JSON-Array| Array der täglichen Verwendungsdetails für jede Instanz/Verbrauchseinheit.|
|nextLink| string| Wenn weitere Seiten mit Daten vorhanden sind, verweist nextLink auf die URL, mit der die nächste Seite zurückgegeben wird. |
|accountId| int| Veraltetes Feld. Aus Gründen der Abwärtskompatibilität vorhanden. |
|productId| int| Veraltetes Feld. Aus Gründen der Abwärtskompatibilität vorhanden. |
|resourceLocationId| int| Veraltetes Feld. Aus Gründen der Abwärtskompatibilität vorhanden. |
|consumedServiceID| int| Veraltetes Feld. Aus Gründen der Abwärtskompatibilität vorhanden. |
|departmentId| int| Veraltetes Feld. Aus Gründen der Abwärtskompatibilität vorhanden. |
|accountOwnerEmail| string| E-Mail-Konto des Kontoinhabers. |
|accountName| string| Vom Kunden eingegebener Name des Kontos. |
|serviceAdministratorId| string| E-Mail-Adresse des Dienstadministrators. |
|subscriptionId| int| Veraltetes Feld. Aus Gründen der Abwärtskompatibilität vorhanden. |
|subscriptionGuid| string| Globally Unique Identifier für das Abonnement. |
|subscriptionName| string| Name des Abonnements. |
|date| string| Datum, an dem die Nutzung aufgetreten ist. |
|product| string| Weitere Details zur Verbrauchseinheit. Beispiel: A1(VM)Windows – Asien-Pazifik, Osten|
|meterId| string| Bezeichner für die Verbrauchseinheit, die die Nutzung ausgegeben hat. |
|meterCategory| string| Verwendeter Azure-Plattformdienst. |
|meterSubCategory| string| Definiert den Typ des Azure-Diensts, der sich auf den Tarif auswirken kann. Beispiel: A1 VM (Nicht-Windows)|
|meterRegion| string| Gibt den Standort des Rechenzentrums für bestimmte Dienste an, die basierend auf dem Standort des Rechenzentrums berechnet werden. |
|meterName| string| Name der Verbrauchseinheit. |
|consumedQuantity| double| Menge der Verbrauchseinheit, die verbraucht wurde. |
|resourceRate| double| Zutreffende Rate pro abrechenbare Einheit. |
|cost| double| Gebühr, die für die Verbrauchseinheit aufgekommen ist. |
|resourceLocation| string| Gibt das Rechenzentrum an, in dem die Verbrauchseinheit ausgeführt wird. |
|consumedService| string| Verwendeter Azure-Plattformdienst. |
|instanceId| string| Dieser Bezeichner ist entweder der Name der Ressource oder die vollqualifizierte Ressourcen-ID. Weitere Informationen finden Sie unter [Azure Resource Manager-API](https://docs.microsoft.com/rest/api/resources/resources). |
|serviceInfo1| string| Interne Azure-Dienstmetadaten. |
|serviceInfo2| string| Beispielsweise ein Imagetyp für einen virtuellen Computer und ein ISP-Name für ExpressRoute. |
|additionalInfo| string| Dienstspezifische Metadaten. Dies kann beispielsweise ein Imagetyp für einen virtuellen Computer sein. |
|tags| string| Vom Kunden hinzugefügte Tags. Weitere Informationen finden Sie unter [Organisieren von Azure-Ressourcen mit Tags](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-using-tags). |
|storeServiceIdentifier| string| Diese Spalte wird nicht verwendet. Aus Gründen der Abwärtskompatibilität vorhanden. |
|departmentName| string| Name der Abteilung. |
|costCenter| string| Kostenstelle, der die Nutzung zugeordnet ist. |
|unitOfMeasure| string| Gibt die Einheit an, in der der Dienst in Rechnung gestellt wird. Beispiel: GB, Stunden, 10.000 s. |
|Ressourcengruppe| string| Die Ressourcengruppe, in der die bereitgestellte Verbrauchseinheit ausgeführt wird. Weitere Informationen finden Sie unter [Übersicht über den Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview). |
<br/>
## <a name="see-also"></a>Weitere Informationen

* [API für Abrechnungszeiträume](billing-enterprise-api-billing-periods.md)

* [API für Bilanz und Zusammenfassung](billing-enterprise-api-balance-summary.md)

* [API für Marketplace Store-Gebühren](billing-enterprise-api-marketplace-storecharge.md) 

* [Preisblatt-API](billing-enterprise-api-pricesheet.md)

