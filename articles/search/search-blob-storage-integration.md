---
title: "Hinzufügen von Azure Search in Blob Storage | Microsoft-Dokumentation"
description: Erstellen Sie einen Index im Code mithilfe der Azure Search HTTP-REST-API.
services: search
documentationcenter: 
author: ashmaka
manager: jhubbard
ms.service: search
ms.topic: article
ms.date: 05/04/2017
ms.author: ashmaka
ms.translationtype: HT
ms.sourcegitcommit: d24c6777cc6922d5d0d9519e720962e1026b1096
ms.openlocfilehash: 15469e8a2d28bdf00d6e8d8c9f823c51975ee90e
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---

# <a name="searching-blob-storage-with-azure-search"></a>Durchsuchen von Blob Storage mit Azure Search

Das Durchsuchen verschiedenster in Azure Blob Storage gespeicherter Inhalte kann ein schwer zu lösendes Problem sein. Allerdings können Sie den Inhalt Ihrer Blobs mit nur wenigen Klicks indizieren und durchsuchen, und zwar mit Azure Search. Das Durchsuchen von Blob Storage erfordert das Bereitstellen eines Azure Search-Dienstes. Die verschiedenen Dienstgrenzwerte und -tarife von Azure Search finden Sie auf der [Übersicht der Preise](https://aka.ms/azspricing).

## <a name="what-is-azure-search"></a>Was ist Azure Search?
[Azure Search](https://aka.ms/whatisazsearch) ist ein Suchdienst, der es Entwicklern leicht macht, stabile Möglichkeiten zur Volltextsuche in Web- und Mobilanwendungen bereitzustellen. Azure Search macht als Dienst das Verwalten einer Suchinfrastruktur überflüssig und bietet gleichzeitig eine [Betriebszeit-SLA von 99,9 %](https://aka.ms/azuresearchsla).

## <a name="index-and-search-enterprise-document-formats"></a>Unternehmensdokumentformate für Indizierung und Suche
Mit der Unterstützung für die [Dokumentextrahierung](https://aka.ms/azsblobindexer) in Azure Blob Storage können Sie folgende Inhalte indizieren:

[!INCLUDE [search-blob-data-sources](../../includes/search-blob-data-sources.md)]

Durch Extrahieren von Text und Metadaten aus diesen Dateitypen können Sie mehrere Dateiformate mit einer einzelnen Abfrage durchsuchen. 

## <a name="search-through-your-blob-metadata"></a>Durchsuchen Ihrer Blob-Metadaten
Das Indizieren von benutzerdefinierten Metadaten sowie von Systemeigenschaften für jedes Blob ist ein häufig auftretendes Szenario, mit dem Sie Blobs jedes Inhaltstyps unkompliziert durchsehen können. Auf diese Weise werden die Informationen für alle Blobs unabhängig vom Dokumenttyp indiziert. Sie können dann den gesamten Blob Storage-Inhalt sortieren und filtern sowie Facets bilden.

[Weitere Informationen zum Indizieren von Blob-Metadaten.](https://aka.ms/azsblobmetadataindexing)

## <a name="image-search"></a>Bildersuche
Die Volltextsuche, die Facettennavigation und die Sortierungsfunktionen von Azure Search können auch auf die Metadaten von in Blobs gespeicherten Bildern angewendet werden.

Wenn diese Bilder bereits mithilfe der [Maschinelles Sehen-API](https://www.microsoft.com/cognitive-services/computer-vision-api) von Cognitive Services von Microsoft vorverarbeitet wurden, ist es möglich, den visuellen Inhalt jedes Bildes, einschließlich OCR und Handschrifterkennung, zu indizieren. Es wird daran gearbeitet, OCR und andere Bildverarbeitungsfunktionen direkt in Azure Search hinzuzufügen. Wenn Sie sich für diese Funktionen interessieren, können Sie uns eine Anfrage über [UserVoice](https://aka.ms/azsuv) oder per [E-Mail](mailto:azscustquestions@microsoft.com) übermitteln.

## <a name="index-and-search-through-json-blobs"></a>Indizieren und Durchsuchen von JSON-Blobs
Azure Search kann so konfiguriert werden, dass es strukturierten Inhalt aus Blobs extrahiert, die eine JSON-Datei enthalten. Azure Search kann aus JSON-Blobs lesen und den strukturierten Inhalt in den entsprechenden Felder in einem Azure Search-Dokument analysieren. Azure Search kann außerdem Blobs annehmen, die ein Array von JSON-Objekten enthalten und jedes Element mit einem separaten Azure Search-Dokument verknüpfen.

Die JSON-Analyse kann derzeit nicht über das Portal konfiguriert werden. [Erfahren Sie mehr zur JSON-Analyse in Azure Search.](https://aka.ms/azsjsonblobindexing)

## <a name="quick-start"></a>Schnellstart
Azure Search kann Blobs direkt über die Blob Storage-Portalseite hinzugefügt werden.

![](./media/search-blob-storage-integration/blob-blade.png)

Klicken Sie auf **Azure Search hinzufügen**, um einen Vorgang zu starten, bei dem Sie einen vorhandenen Azure Search-Dienst auswählen oder einen neuen Dienst erstellen können. Wenn Sie einen neuen Dienst erstellen, verlassen Sie den Portalbereich Ihres Speicherkontos. Sie können zurück zur Storage-Portalseite navigieren, und erneut die Option **Azure Search hinzufügen** auswählen, um den vorhandenen Dienst auszuwählen.

### <a name="next-steps"></a>Nächste Schritte
In der vollständigen [Dokumentation](https://aka.ms/azsblobindexer) erfahren Sie mehr zum Azure Search-Blobindexer.

