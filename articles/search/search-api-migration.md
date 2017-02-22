---
title: "Aktualisieren der REST-API für den Azure Search-Dienst auf Version 2016-09-01 | Microsoft Docs"
description: "Aktualisieren der REST-API für den Azure Search-Dienst auf Version 2016-09-01"
services: search
documentationcenter: 
author: brjohnstmsft
manager: pablocas
editor: 
ms.assetid: 6183fa6c-48bb-4af7-adae-4be3bc43c3ed
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/27/2016
ms.author: brjohnst
translationtype: Human Translation
ms.sourcegitcommit: 7d45759915f38ba4337b745eb2b28dcbc72dbbe0
ms.openlocfilehash: f6a189c2e314b91c490583a86d8bacca8ec78a0f

---
# <a name="upgrading-to-the-azure-search-service-rest-api-version-2016-09-01"></a>Aktualisieren der REST-API für den Azure Search-Dienst auf Version 2016-09-01
Wenn Sie die Version 2015-02-28 oder 2015-02-28-Preview der [REST-API für den Azure Search-Dienst](https://msdn.microsoft.com/library/azure/dn798935.aspx) verwenden, unterstützt dieser Artikel Sie beim Upgrade Ihrer Anwendung auf die erste allgemein verfügbare API-Version 2016-09-01.

Version 2016-09-01 der REST-API enthält einige Änderungen im Vergleich mit früheren Versionen. Sie sind größtenteils abwärtskompatibel, daher sollte das Ändern von Code nur minimalen Aufwand bedeuten, abhängig davon, welche Version Sie zuvor verwendet haben. Anweisungen zum Ändern Ihres Codes, um die neue API-Version zu verwenden, finden Sie unter [Schritte zum Upgrade](#UpgradeSteps).

> [!NOTE]
> Ihre Azure Search-Dienstinstanz unterstützt mehrere REST-API-Versionen, einschließlich der neuesten. Sie können auch ältere Versionen weiterhin verwenden, aber es wird empfohlen, den Code zur neuesten Version zu migrieren.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-2016-09-01"></a>Neuerungen in Version 2016-09-01
Version 2016-09-01 ist die zweite allgemein verfügbare Version der REST-API für den Azure Search-Dienst. Neue Funktionen in dieser API-Version:

* [Benutzerdefinierte Analysen](https://aka.ms/customanalyzers), die es Ihnen ermöglichen, die Kontrolle über den Prozess der Konvertierung von Text in indizierbare und durchsuchbare Token zu übernehmen.
* [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md)- und [Azure Table Storage](search-howto-indexing-azure-tables.md)-Indexer, die es Ihnen ermöglichen, problemlos Daten aus Azure Storage nach einem Zeitplan oder bei Bedarf in Azure Search zu importieren.
* [Feldzuordnungen](search-indexer-field-mappings.md), mit denen Sie anpassen können, wie Indexer Daten in Azure Search importieren.
* ETags, mit denen Sie die Definitionen von Indizes, Indexern und Datenquellen in einer parallelitätssicheren Weise aktualisieren können. 

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Schritte zum Upgrade
Wenn Sie das Upgrade von Version 2015-02-28 ausführen, müssen Sie wahrscheinlich keine Änderungen am Code vornehmen, außer die Versionsnummer zu ändern. Nur in folgenden Situationen müssen Sie möglicherweise Code ändern:

* Der Code erzeugt Fehler, wenn nicht erkannte Eigenschaften in einer API-Antwort zurückgegeben werden. Standardmäßig sollte die Anwendung Eigenschaften ignorieren, die sie nicht versteht.
* Ihr Code behält API-Anforderungen bei und versucht, sie erneut an die neue API-Version zu senden. Dies kann beispielsweise vorkommen, wenn Ihre Anwendung Fortsetzungstoken beibehält, die von der Search-API zurückgegeben wurden (weitere Informationen finden Sie, indem Sie in der [Search-API-Referenz](https://msdn.microsoft.com/library/azure/dn798927.aspx#Anchor_1) nach `@search.nextPageParameters` suchen).

Wenn diese Situationen für Sie zutreffen, müssen Sie den Code möglicherweise entsprechend ändern. Ansonsten sollten keine Änderungen nötig sein, es sei denn, Sie möchten die [neuen Features](#WhatsNew) der Version 2016-09-01 verwenden.

Wenn Sie das Upgrade von Version 2015-02-28-Preview ausführen, gilt das oben Genannte ebenfalls. Sie müssen jedoch außerdem beachten, dass einige Features der Vorschauversion in der Version 2016-09-01 nicht verfügbar sind:

* Unterstützung für CSV-Dateien und Blobs, die JSON-Arrays enthalten, durch Azure Blob Storage-Indexer
* Synonyme
* MLT-Abfragen (More Like This)

Wenn der Code diese Features verwendet, können Sie erst auf 2016-09-01 aktualisieren, sobald Sie sie nicht mehr nutzen.

> [!IMPORTANT]
> Beachten Sie hierbei, dass Vorschau-APIs für Tests und Evaluierungen bestimmt sind und nicht in Produktionsumgebungen eingesetzt werden sollten.
> 
> 

## <a name="conclusion"></a>Zusammenfassung
Weitere Informationen zur Verwendung der REST-API für den Azure Search-Dienst finden Sie in der kürzlich aktualisierten [API-Referenz](https://msdn.microsoft.com/library/azure/dn798935.aspx) auf MSDN.

Wir freuen uns auf Ihr Feedback zu Azure Search. Wenn Probleme auftreten, können Sie sich gerne über das [Azure Search-MSDN-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch) oder [StackOverflow](http://stackoverflow.com/) an uns wenden. Wenn Sie eine Frage zu Azure Search auf StackOverflow stellen, müssen Sie sie mit `azure-search` kennzeichnen.

Vielen Dank für die Nutzung von Azure Search!




<!--HONumber=Feb17_HO3-->


