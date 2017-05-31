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
ms.translationtype: Human Translation
ms.sourcegitcommit: 9ae7e129b381d3034433e29ac1f74cb843cb5aa6
ms.openlocfilehash: 0cd0cbb05c465d32a9ef02f9350ebdf9ccea36c5
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017

---

# <a name="searching-blob-storage-with-azure-search"></a>Durchsuchen von Blob Storage mit Azure Search

Das Durchsuchen verschiedenster in Azure Blob Storage gespeicherter Inhalte kann ein schwer zu lösendes Problem sein. Allerdings können Sie den Inhalt Ihrer Blobs mit nur wenigen Klicks indizieren und durchsuchen, und zwar mit Azure Search. Das Durchsuchen von Blob Storage erfordert das Bereitstellen eines Azure Search-Dienstes. Die verschiedenen Dienstgrenzwerte und -tarife von Azure Search finden Sie auf der [Übersicht der Preise](https://aka.ms/azspricing).

## <a name="what-is-azure-search"></a>Was ist Azure Search?
[Azure Search](https://aka.ms/whatisazsearch) ist eine Suchlösung, die es Entwicklern leicht macht, widerstandsfähige Möglichkeiten zur Volltextsuchen in Web- und Smartphone-Anwendungen bereitzustellen. Azure Search macht als Dienst das Verwalten der Suchinfrastruktur überflüssig, während es gleichzeitig eine [Betriebszeit-SLA von 99,9 %](https://aka.ms/azuresearchsla) bietet.

Mit einer erweiterten Unterstützung für 56 Sprachen kann Azure Search Ihre Inhalte analysieren und sprachspezifische Konstrukte auf intelligente Weise behandeln. Azure Search bietet außerdem die Werkzeuge zum Entwickeln einer für Benutzer angenehmen Suchfunktion. Das Hinzufügen von Funktionen wie z.B. die Facettennavigation, automatische Vervollständigungsvorschläge bei der Suche und die Treffermarkierung auf Benutzeroberflächen ist unkompliziert. Lesen Sie die [Dokumentation](https://aka.ms/azsearchdocs) des Dienstes, um mehr über die Funktionen von Azure Search zu erfahren.

## <a name="crack-open-and-search-through-the-content-of-enterprise-document-formats"></a>Erhalten Sie Einblick in die Formatinhalte von Unternehmensdokumenten, und durchsuchen Sie diese
Durch die Unterstützung für die [Dokumentextrahierung](https://aka.ms/azsblobindexer) in Azure Blob Storage kann Azure Search den Inhalt von verschiedensten in Blobs gespeicherten Dateitypen indizieren:
- PDF
- Microsoft Office: DOCX/DOC, XLSX/XLS, PPTX/PPT, MSG (Outlook-E-Mails)
- HTML
- Nur-Text-Dateien

Durch das Extrahieren von Text und Metadaten aus diesen Dateitypen können Sie mit einer einzigen Abfrage formatübergreifend suchen, um die relevantesten Dokumente zu finden, und das unabhängig von deren Typ. Durch das Indizieren des Inhalts und der Metadaten von Dokumenten, PDFs und E-Mails von Microsoft Office ist es möglich, widerstandsfähige Lösungen zur Verwaltung von Unternehmensinhalten mithilfe von Blob Storage und Azure Search zu erstellen.

## <a name="search-through-your-blob-metadata"></a>Durchsuchen Ihrer Blob-Metadaten
Das Indizieren der benutzerdefinierten Blob-Metadaten sowie der Systemeigenschaften für jeden Ihrer Blobs ist ein häufig auftretendes Szenario, mit dem Sie Blobs jedes Inhaltstyps unkompliziert durchsehen können. So werden die Informationen jedes Blobs indiziert, unabhängig vom Dokumententyp im Blob. Dies erleichtert Ihnen das Sortieren und Facettieren des gesamten Inhalts in Blob Storage.

[Weitere Informationen zum Indizieren von Blob-Metadaten.](https://aka.ms/azsblobmetadataindexing)

## <a name="image-search"></a>Bildersuche
Die Volltextsuche, die Facettennavigation und die Sortierungsfunktionen von Azure Search können auch auf die Metadaten von in Blobs gespeicherten Bildern angewendet werden.

Wenn diese Bilder bereits mithilfe der [Maschinelles Sehen-API](https://www.microsoft.com/cognitive-services/computer-vision-api) von Cognitive Services von Microsoft vorverarbeitet wurden, ist es möglich, den visuellen Inhalt jedes Bildes, einschließlich OCR und Handschrifterkennung, zu indizieren. Es wird daran gearbeitet, OCR und andere Bildverarbeitungsfunktionen in Azure Search hinzuzufügen. Wenn Sie such für diese Funktionen interessieren, können Sie uns gerne eine Anfrage über [UserVoice](https://aka.ms/azsuv) schicken, oder [senden Sie uns eine E-Mail](mailto:azscustquestions@microsoft.com).

## <a name="index-and-search-through-json-blobs"></a>Indizieren und Durchsuchen von JSON-Blobs
Azure Search kann so konfiguriert werden, dass es strukturierten Inhalt aus Blobs extrahiert, die eine JSON-Datei enthalten. Azure Search kann aus JSON-Blobs lesen und den strukturierten Inhalt in den entsprechenden Felder in einem Azure Search-Dokument analysieren. Azure Search kann außerdem Blobs annehmen, die ein Array von JSON-Objekten enthalten und jedes Element mit einem separaten Azure Search-Dokument verknüpfen.

Beachten Sie, dass die JSON-Analyse aktuell nicht über das Portal konfiguriert werden kann. [Erfahren Sie mehr zur JSON-Analyse in Azure Search.](https://aka.ms/azsjsonblobindexing)

## <a name="quick-start"></a>Schnellstart
Azure Search kann direkt über das Portalblatt von Blob Storage hinzugefügt werden.

![](./media/search-blob-storage-integration/blob-blade.png)

Wenn Sie auf die Option „Azure Search hinzufügen“ klicken, wird ein Vorgang gestartet, in dem Sie zwischen einem vorhandenen Azure Search-Dienst oder dem Erstellen eines neuen Dienstes wählen können. Wenn Sie einen neuen Dienst erstellen, verlassen Sie den Portalbereich Ihres Speicherkontos. Sie müssen zum Portalblatt des Speichers zurücknavigieren, und dann noch einmal die Option „Azure Search hinzufügen“ auswählen, um den vorhandenen Dienst auswählen zu können.

### <a name="next-steps"></a>Nächste Schritte
In der vollständigen [Dokumentation](https://aka.ms/azsblobindexer) erfahren Sie mehr zum Azure Search-Blobindexer.

