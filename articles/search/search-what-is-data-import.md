---
title: Hochladen von Daten in Azure Search | Microsoft Docs
description: "Erfahren Sie, wie Sie Daten in einen Index in Azure Search hochladen können."
services: search
documentationcenter: 
author: ashmaka
manager: jhubbard
editor: 
tags: 
ms.assetid: aa8d47c1-4ae6-4209-a8ce-48d5a9474707
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 01/11/2017
ms.author: ashmaka
translationtype: Human Translation
ms.sourcegitcommit: 292c9150822363aba3336b1efce579dc5362cb14
ms.openlocfilehash: e522d608e8ff51e00b3c1a461bf9ba909b0105af


---
# <a name="upload-data-to-azure-search"></a>Hochladen von Daten in Azure Search
> [!div class="op_single_selector"]
> * [Übersicht](search-what-is-data-import.md)
> * [.NET](search-import-data-dotnet.md)
> * [REST](search-import-data-rest-api.md)
> 
> 

Es gibt zwei Möglichkeiten, einen Index mit Ihren Daten zu füllen. Die erste Option ist die manuelle Push-Übertragung Ihrer Daten mithilfe der Azure Search [REST-API](search-import-data-rest-api.md) oder [.NET SDK](search-import-data-dotnet.md) in den Index. Die zweite Option ist, [für eine unterstützte Datenquelle auf Ihren Index zu verweisen](search-indexer-overview.md) und die Daten automatisch durch Azure Search beschaffen zu lassen.

## <a name="push-data-to-an-index"></a>Push-Übertragung von Daten in einen Index
Dieser Ansatz bezieht sich auf die programmgesteuerte Übertragung Ihrer Daten an Azure Search, damit diese für die Suche verfügbar sind. Bei Anwendungen mit sehr niedrigen Latenzanforderungen (wenn z.B. Suchvorgänge mit dynamischen Inventardatenbanken synchronisiert werden müssen) ist das Push-Modell Ihre einzige Option.

Sie können die [REST-API](https://docs.microsoft.com/rest/api/searchservice/AddUpdate-or-Delete-Documents) oder [.NET SDK](search-import-data-dotnet.md) zur Push-Übertragung von Daten in einen Index verwenden. Es gibt derzeit keine Toolunterstützung für die Push-Übertragung von Daten über das Portal.

Dieser Ansatz ist flexibler als ein Pull-Modell, da Sie Dokumente einzeln oder in Batches hochladen können (bis zu 1000 pro Batch oder 16 MB, je nachdem, welches Limit zuerst erreicht wird). Das Push-Modell ermöglicht Ihnen auch das Hochladen von Dokumenten in Azure Search, unabhängig davon, wo sich Ihre Daten befinden.

Das für Azure Search geeignete Datenformat ist JSON, und alle Dokumente im Dataset müssen über Felder verfügen, die im Indexschema definierten Feldern zugeordnet sind. 

## <a name="pull-data-into-an-index"></a>Abrufen von Daten in einen Index mithilfe von Pull
Beim Pull-Modell wird eine unterstützte Datenquelle durchsucht, und die Daten werden für Sie automatisch in Ihren Index hochgeladen. Diese Funktion wird in Azure Search über *Indexer* implementiert, die derzeit für [Blobspeicher](search-howto-indexing-azure-blob-storage.md), [Tabellenspeicher](search-howto-indexing-azure-tables.md), [DocumentDB](http://aka.ms/documentdb-search-indexer), [Azure SQL-Datenbank und SQL Server auf virtuellen Azure-Computern](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md) verfügbar sind. 

Mit Indexern wird für einen Index eine Verbindung mit einer Datenquelle (normalerweise eine Tabelle, Sicht oder ähnliche Struktur) hergestellt, und Quellfelder werden gleichwertigen Feldern im Index zugeordnet. Während der Ausführung wird das Rowset automatisch in JSON transformiert und in den angegebenen Index geladen. Alle Indexer unterstützen die Verwendung von Zeitplänen, sodass Sie angeben können, wie häufig die Daten aktualisiert werden sollen. Die meisten Indexer verfügen über eine Änderungsnachverfolgung, wenn dies von der Datenquelle unterstützt wird. Da Änderungen und Löschvorgänge in vorhandenen Dokumenten nachverfolgt und neue Dokumente erkannt werden, ist es mithilfe von Indexern nicht mehr erforderlich, die Daten in Ihrem Index aktiv zu verwalten. 

Die Indexer-Funktion steht im [Azure-Portal](search-import-data-portal.md) und für die [REST-API](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations) und das [.NET SDK](https://docs.microsoft.com/otnet/api/microsoft.azure.search.iindexersoperations?redirectedfrom=MSDN#microsoft_azure_search_iindexersoperations) zur Verfügung. 

Ein Vorteil der Verwendung des Portals ist, dass Azure Search normalerweise ein Indexstandardschema für Sie generieren kann, indem die Metadaten des Quelldatasets gelesen werden. Sie können den generierten Index ändern, bis er verarbeitet wird. Nach der Verarbeitung sind nur noch Schemabearbeitungen zulässig, bei denen keine erneute Indizierung erforderlich ist. Wenn die gewünschten Änderungen eine direkte Auswirkung auf das Schema haben, müssen Sie den Index neu erstellen. 

Nach dem Auffüllen des Index können Sie den **Suchexplorer** in der Befehlsleiste des Portals verwenden, um einen Überprüfungsschritt auszuführen.

## <a name="query-an-index-using-search-explorer"></a>Abfragen eines Index mit dem Suchexplorer

Eine schnelle Möglichkeit zum Durchführen einer vorläufigen Prüfung für den Dokumentupload ist die Verwendung des **Suchexplorers** im Portal. Mit dem Explorer können Sie einen Index abfragen, ohne dass Sie dafür Code schreiben müssen. Die Suche basiert auf den Standardeinstellungen, z.B. der [einfachen Syntax](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) und dem [searchMode-Abfrageparameter](https://docs.microsoft.com/rest/api/searchservice/search-documents), der standardmäßig verwendet wird. Ergebnisse werden im JSON-Format zurückgegeben, damit Sie das gesamte Dokument untersuchen können.

> [!TIP]
> Viele [Azure Search-Codebeispiele](https://github.com/Azure-Samples/?utf8=%E2%9C%93&query=search) enthalten eingebettete bzw. sofort verfügbare Datasets, damit Sie schnell starten können. Außerdem enthält das Portal auch einen Beispielindexer und eine Datenquelle mit einem kleinen Immobiliendataset („realestate-us-sample“). Wenn Sie den vorkonfigurierten Indexer in der Beispieldatenquelle ausführen, wird ein Index erstellt und mit Dokumenten geladen, die dann im Suchexplorer oder mit von Ihnen geschriebenem Code abgefragt werden können.


<!--HONumber=Feb17_HO3-->


