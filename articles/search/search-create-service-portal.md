---
title: "Erstellen eines Azure Search-Diensts über das Azure-Portal | Microsoft Docs"
description: "Erlernen Sie das Bereitstellen eines Azure Search-Diensts über das Azure-Portal."
services: search
manager: jhubbard
author: ashmaka
documentationcenter: 
ms.assetid: c8c88922-69aa-4099-b817-60f7b54e62df
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 08/29/2016
ms.author: ashmaka
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 5975bd5b2a2e7fe8799155ab47d96c3ecd0093ee


---
# <a name="create-an-azure-search-service-using-the-azure-portal"></a>Erstellen eines Azure Search-Diensts über das Azure-Portal
In diesem Leitfaden lernen Sie, wie Sie einen Azure Search-Dienst mithilfe des [Azure-Portals](https://portal.azure.com/)erstellen (oder bereitstellen).

In diesem Leitfaden wird davon ausgegangen, dass Sie bereits über ein Azure-Abonnement verfügen und sich beim Azure-Portal anmelden können.

## <a name="find-azure-search-in-the-azure-portal"></a>Suchen Sie Azure Search im Azure-Portal.
1. Wechseln Sie zum [Azure-Portal](https://portal.azure.com/) , und melden Sie sich an.
2. Klicken Sie in der oberen linken Ecke auf das Pluszeichen („+“).
3. Wählen Sie **Daten und Speicher**.
4. Wählen Sie **Azure Search**aus.

![](./media/search-create-service-portal/find-search.png)

## <a name="pick-a-service-name-and-url-endpoint-for-your-service"></a>Wählen Sie einen Dienstnamen und einen URL-Endpunkt für den Dienst.
1. Ihr Dienstname wird Teil der Endpunkt-URL für den Azure Search-Dienst. Anhand dieser URL führen Sie Ihre API-Aufrufe zum Verwalten und Verwenden des Search-Diensts durch.
2. Geben Sie den Dienstnamen im Feld **URL** ein. Der Dienstname:
   * darf nur Kleinbuchstaben, Ziffern oder Bindestriche („-“) enthalten.
   * darf in den ersten beiden und als letztes Zeichen keinen Bindestrich („-“) enthalten.
   * darf keine aufeinander folgenden Bindestriche enthalten („--“).
   * ist auf eine Länge zwischen 2 und 60 Zeichen beschränkt.

## <a name="select-a-subscription-where-you-will-keep-your-service"></a>Wählen Sie ein Abonnement zum Verwalten Ihres Diensts aus.
Wenn Sie über mehrere Abonnements verfügen, können Sie auswählen, welches davon diesen Azure Search-Dienst enthalten soll.

## <a name="select-a-resource-group-for-your-service"></a>Erstellen Sie eine Ressourcengruppe für Ihren Dienst.
Erstellen Sie eine neue Ressourcengruppe, oder wählen Sie eine vorhandene Ressourcengruppe aus. Eine Ressourcengruppe ist eine Sammlung von Azure-Diensten und -Ressourcen, die zusammen verwendet werden. Wenn Sie beispielsweise mit Azure Search eine SQL-Datenbank indizieren, sollten diese beiden Dienste der gleichen Ressourcengruppe angehören.

## <a name="select-the-location-where-your-service-will-be-hosted"></a>Wählen Sie den Standort aus, an dem Ihr Dienst gehostet wird.
Als Azure-Dienst kann Azure Search in Rechenzentren auf der ganzen Welt gehostet werden. Beachten Sie, dass die [Preise sich je nach geografischer Lage unterscheiden können](https://azure.microsoft.com/pricing/details/search/) .

## <a name="select-your-pricing-tier"></a>Wählen Sie Ihren Tarif aus.
[Azure Search wird derzeit in mehreren Tarifen angeboten](https://azure.microsoft.com/pricing/details/search/): Free, Basic oder Standard. Jeder Tarif verfügt über eigene [Kapazitäten und Grenzwerte](search-limits-quotas-capacity.md). Anleitungen finden Sie unter [Auswählen einer SKU oder eines Tarifs für Azure Search](search-sku-tier.md) .

In diesem Fall haben wir den Standard-Tarif für unseren Dienst gewählt.

## <a name="select-the-create-button-to-provision-your-service"></a>Wählen Sie die Schaltfläche „Erstellen“, um Ihren Dienst bereitzustellen.
![](./media/search-create-service-portal/create-service.png)

## <a name="scale-your-service"></a>Skalieren Sie den Dienst.
Nach der Bereitstellung Ihres Diensts können Sie ihn Ihren Anforderungen entsprechend skalieren. Wenn Sie den Standard-Tarif für Ihren Azure Search-Dienst ausgewählt haben, können Sie Ihren Dienst in zwei Dimensionen skalieren: Replikate und Partitionen. Wenn Sie den Basic-Tarif ausgewählt haben, können Sie nur Replikate hinzufügen.

***Partitionen*** ermöglichen Ihrem Dienst das Speichern und Durchsuchen weiterer Dokumente.

Mit ***Replikaten*** kann Ihr Dienst eine höhere Auslastung von Suchabfragen verarbeiten. [Ein Dienst benötigt zwei Replikate, um eine schreibgeschützte SLA zu erreichen, und drei Replikate zum Erreichen einer SLA mit Lese-/Schreibzugriff](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

1. Öffnen Sie im Azure-Portal das Verwaltungsblatt für Ihren Azure Search-Dienst.
2. Wählen Sie auf dem Blatt **Einstellungen** die Option **Skalieren** aus.
3. Sie können den Dienst skalieren, indem Sie Replikate oder Partitionen hinzufügen.
   * Der Dienst kann nicht über 36 Sucheinheiten hinaus skaliert werden. Die Gesamtzahl von Sucheinheiten entspricht dem Produkt der Replikate und Partitionen (Replikate * Partitionen = Sucheinheiten insgesamt).
   * Wenn Sie den Basic-Tarif ausgewählt haben, können Sie nur auf drei Replikate skalieren. Basic-Dienste sind an eine einzelne Partition gebunden.

![](./media/search-create-service-portal/scale-service.png)

## <a name="next"></a>Weiter
Nach der Bereitstellung eines Azure Search-Diensts können Sie einen [Azure Search-Index definieren](search-what-is-an-index.md) , damit Sie Ihre Daten hochladen und durchsuchen können.

Unter [Erste Schritte mit Azure Search im Portal](search-get-started-portal.md) finden Sie ein kurzes Tutorial.




<!--HONumber=Nov16_HO3-->


