---
title: Erstellen eines Azure Search-Diensts im Portal | Microsoft-Dokumentation
description: "Stellen Sie einen Azure Search-Dienst über das Portal bereit."
services: search
manager: jhubbard
author: HeidiSteen
documentationcenter: 
ms.assetid: c8c88922-69aa-4099-b817-60f7b54e62df
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 02/16/2017
ms.author: heidist
translationtype: Human Translation
ms.sourcegitcommit: 4d7c0afe5e43e0c119f534cdedaaa03a16ed5303
ms.openlocfilehash: 0d5198c74fa81358dcdfec514e1fe422d17f368f
ms.lasthandoff: 02/17/2017


---
# <a name="create-an-azure-search-service-in-the-portal"></a>Erstellen eines Azure Search-Diensts im Portal

In diesem Artikel wird die Erstellung bzw. Bereitstellung eines Azure Search-Diensts im Portal erläutert. Anweisungen für PowerShell finden Sie unter [Verwalten des Azure Search-Diensts mit PowerShell](search-manage-powershell.md).

## <a name="subscribe-free-or-paid"></a>Abonnieren (kostenlos oder kostenpflichtig)

[Erstellen Sie ein kostenloses Azure-Konto](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F), und verwenden Sie das kostenlose Guthaben, um unsere kostenpflichtigen Azure-Dienste zu testen. Wenn das Guthaben aufgebraucht ist, können Sie das Konto behalten und weiterhin kostenlose Azure-Dienste wie z.B. Websites verwenden. Ihre Kreditkarte wird nur dann belastet, wenn Sie Ihre Einstellungen explizit ändern und mit der Berechnung von Gebühren einverstanden sind.

Alternativ dazu können Sie Ihre [Vorteile für MSDN-Abonnenten aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). Ihr MSDN-Abonnement beinhaltet ein monatliches Guthaben, das Sie für kostenpflichtige Azure-Dienste verwenden können. 

## <a name="find-azure-search"></a>Aufrufen von Azure Search
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Klicken Sie in der oberen linken Ecke auf das Pluszeichen („+“).
3. Wählen Sie **Web + Mobile** > **Azure Search** aus.

![](./media/search-create-service-portal/find-search2.png)

## <a name="name-the-service-and-url-endpoint"></a>Benennen des Diensts und des URL-Endpunkts

Ein Dienstname ist Teil eines URL-Endpunkts, für den API-Aufrufe ausgegeben werden. Geben Sie den Dienstnamen im Feld **URL** ein. 

Anforderungen an Dienstnamen:
   * Zwischen 2 und 60 Zeichen lang
   * Kleinbuchstaben, Ziffern oder Bindestriche („-“)
   * Kein Bindestrich in den ersten beiden und im letzten Zeichen
   * keine aufeinander folgenden Bindestriche („--“)

## <a name="select-a-subscription"></a>Auswählen eines Abonnements
Wenn Sie über mehrere Abonnements verfügen, wählen Sie ein Abonnement aus, in dem sich Daten- oder Dateispeicherdienste befinden. Azure Search kann die Dienste Azure Table Storage, Blob Storage, SQL-Datenbank und DocumentDB automatisch erkennen, um sie über *Indexer* zu indizieren. Dies gilt jedoch nur für Dienste im gleichen Abonnement.

## <a name="select-a-resource-group"></a>Auswählen einer Ressourcengruppe
Eine Ressourcengruppe ist eine Sammlung von Azure-Diensten und -Ressourcen, die zusammen verwendet werden. Wenn Sie beispielsweise mit Azure Search eine SQL-Datenbank indizieren, müssen beide Dienste der gleichen Ressourcengruppe angehören.

> [!TIP]
> Wenn eine Ressourcengruppe gelöscht wird, werden auch die darin befindlichen Dienste gelöscht. Bei Prototypprojekten, die mehrere Dienste verwenden, sollten Sie all diese Dienste in die gleiche Ressourcengruppe platzieren, um das Bereinigen nach Abschluss des Projekts zu vereinfachen. 

## <a name="select-a-hosting-location"></a>Auswählen eines Hostingstandorts 
Als Azure-Dienst kann Azure Search in Rechenzentren auf der ganzen Welt gehostet werden. Beachten Sie, dass sich die [Preise je nach geografischer Lage unterscheiden können](https://azure.microsoft.com/pricing/details/search/).

## <a name="select-a-pricing-tier-sku"></a>Auswählen eines Tarifs (SKU)
[Azure Search wird derzeit in mehreren Tarifen angeboten](https://azure.microsoft.com/pricing/details/search/): Free, Basic oder Standard. Jeder Tarif verfügt über eigene [Kapazitäten und Grenzwerte](search-limits-quotas-capacity.md). Anleitungen finden Sie unter [Auswählen einer SKU oder eines Tarifs für Azure Search](search-sku-tier.md) .

In dieser exemplarischen Vorgehensweise haben wir den Standard-Tarif für unseren Dienst gewählt.

## <a name="create-your-service"></a>Erstellen des Diensts

Denken Sie daran, Ihren Dienst an das Dashboard anzuheften, damit Sie nach jeder Anmeldung sofort darauf zugreifen können.

![](./media/search-create-service-portal/new-service2.png)

## <a name="scale-your-service"></a>Skalieren des Diensts
Die Erstellung eines Diensts kann einige Minuten dauern (je nach Tarif&15; Minuten oder länger). Nach der Bereitstellung Ihres Diensts können Sie ihn Ihren Anforderungen entsprechend skalieren. Da Sie für Ihren Azure Search-Dienst den Standard-Tarif ausgewählt haben, können Sie den Dienst in zwei Dimensionen skalieren: Replikate und Partitionen. Wenn Sie den Basic-Tarif auswählen, können Sie nur Replikate hinzufügen. Wenn Sie den kostenlosen Dienst bereitstellen, ist keine Skalierung verfügbar.

***Partitionen*** ermöglichen Ihrem Dienst das Speichern und Durchsuchen weiterer Dokumente.

***Replikate*** ermöglichen Ihrem Dienst, eine größere Menge von Suchabfragen zu verarbeiten.

> [!Important]
> Ein Dienst benötigt [2 Replikate für schreibgeschützte SLAs und 3 Replikate für SLAs mit Lese-/Schreibzugriff](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

1. Wechseln Sie im Azure-Portal zum Blatt Ihres Suchdiensts.
2. Wählen Sie im linken Navigationsbereich die Optionen **Einstellungen** > **Skalierung** aus.
3. Verwenden Sie den Schiebregler, um Replikate oder Partitionen hinzuzufügen.

![](./media/search-create-service-portal/settings-scale.png)

> [!Note] 
> Jeder Tarif bietet unterschiedliche [Grenzwerte](search-limits-quotas-capacity.md) für die Gesamtzahl der in einem einzelnen Dienst zulässigen Sucheinheiten (Replikate × Partitionen = Sucheinheiten gesamt).

## <a name="next-steps"></a>Nächste Schritte
Nach der Bereitstellung eines Azure Search-Diensts können Sie einen [Index definieren](search-what-is-an-index.md), damit Sie Ihre Daten hochladen und durchsuchen können.

Um per Code oder Skript auf den Dienst zuzugreifen, stellen Sie die URL (*Dienstname*.search.windows.net) und einen Schlüssel bereit. Administratorschlüssel gewähren Vollzugriff, Abfrageschlüssel gewähren schreibgeschützten Zugriff. Informationen zum Einstieg finden Sie unter [Verwenden von Azure Search aus einer .NET-Anwendung](search-howto-dotnet-sdk.md).

Ein kurzes Tutorial für die Verwendung des Portals finden Sie unter [Erste Schritte mit Azure Search im Portal](search-get-started-portal.md).


