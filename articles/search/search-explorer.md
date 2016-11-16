---
title: Abfragen des Azure Search-Indexes mit dem Azure-Portal | Microsoft Docs
description: Eingeben einer Suchabfrage in den Suchexplorer im Azure-Portal.
services: search
manager: jhubbard
documentationcenter: 
author: ashmaka
ms.assetid: 8e524188-73a7-44db-9e64-ae8bf66b05d3
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 08/29/2016
ms.author: ashmaka
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: a23372112e17703a3399e1bdc9eaf73b85a1f80d


---
# <a name="query-your-azure-search-index-using-the-azure-portal"></a>Abfragen des Azure Search-Indexes mit dem Azure-Portal
> [!div class="op_single_selector"]
> * [Übersicht](search-query-overview.md)
> * [Portal](search-explorer.md)
> * [.NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
> 
> 

In diesem Leitfaden erfahren Sie, wie Sie Ihren Azure Search-Index im Azure-Portal abfragen.

[Erstellen Sie einen Azure Search-Index](search-what-is-an-index.md) und [füllen Sie ihn mit Daten](search-what-is-data-import.md), bevor Sie mit dieser exemplarischen Vorgehensweise beginnen.

## <a name="i-go-to-your-azure-search-blade"></a>I. Wechseln Sie zu Ihrem Azure Search-Blatt.
1. Klicken Sie auf  im Menü auf der linken Seite des [Azure-Portals](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices)
2. Wählen Sie Ihren Azure Search-Dienst aus.

## <a name="ii-select-the-index-you-would-like-to-search"></a>II. Wählen Sie den Index aus, den Sie durchsuchen möchten.
1. Wählen Sie in der Kachel „Indizes“ den Index aus, den Sie durchsuchen möchten.

![](./media/search-explorer/pick-index.png)

## <a name="iii-click-on-the-search-explorer-tile"></a>III. Klicken Sie auf die Kachel „Suchexplorer“.
![](./media/search-explorer/search-explorer-tile.png)

## <a name="iii-start-searching"></a>III. Beginnen Sie mit der Suche.
1. Beginnen Sie mit einer Texteingabe in das Feld „*Abfragezeichenfolge*“, und drücken Sie danach auf „**Suchen**“.
   
   * Wenn Sie den Suchexplorer verwenden, können Sie alle beliebigen [Abfrageparameter](https://msdn.microsoft.com/library/dn798927.aspx)
2. Im Abschnitt „*Ergebnisse*“ werden die Abfrageergebnisse im unformatierten JSON-Format angezeigt, das Sie bei Suchanfragen an die Azure Search REST-API in einem HTML-Antworttext erhalten würden.
3. Die Abfragezeichenfolge wird automatisch in die richtige Anforderungs-URL gelesen, um eine HTTP-Anforderung für die Azure Search REST-API zu übermitteln.

![](./media/search-explorer/search-bar.png)




<!--HONumber=Nov16_HO2-->


