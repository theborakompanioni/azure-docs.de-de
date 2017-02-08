---
title: Erstellen eines Medienprozessors | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie eine Medienprozessorkomponente erstellen können, um Medieninhalte für Azure Media Services zu codieren, zu ver- oder entschlüsseln, und um Formate zu konvertieren. Die Codebeispiele sind in C# geschrieben und verwenden das Media Services SDK für .NET."
services: media-services
documentationcenter: 
author: juliako
manager: erikre
editor: 
ms.assetid: dbf9496f-c6f0-42a7-aa36-70f89dcb8ea2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2016
ms.author: juliako
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: cc3c3bff6179b634b82884dfcb3f8f8395f6bb89


---
# <a name="how-to-get-a-media-processor-instance"></a>Gewusst wie: Abrufen einer Media Processor-Instanz
> [!div class="op_single_selector"]
> * [.NET](media-services-get-media-processor.md)
> * [REST](media-services-rest-get-media-processor.md)
> 
> 

## <a name="overview"></a>Übersicht
Der Medienprozessor in Media Services ist für bestimmte Verarbeitungsaufgaben wie z. B. Codierung, Formatumwandlungen, Verschlüsselung oder Entschlüsselung von Medieninhalten zuständig. Normalerweise erstellen Sie einen Medienprozessor, wenn Sie eine Aufgabe zur Codierung, Verschlüsselung oder Formatumwandlung von Medieninhalten erstellen.

Die folgende Tabelle enthält Name und Beschreibung der verfügbaren Medienprozessoren.

| Medienprozessorname | Beschreibung | Weitere Informationen |
| --- | --- | --- |
| Media Encoder Standard |Umfasst Standardfunktionen für bedarfsgesteuerte Codierung. |[Azure On-Demand Media Encoder – Überblick und Vergleich](media-services-encode-asset.md) |
| Media Encoder Premium Workflow |Zur Ausführung von Codieraufgaben mit dem Media Encoder Premium Workflow. |[Azure On-Demand Media Encoder – Überblick und Vergleich](media-services-encode-asset.md) |
| Azure Media Indexer |Macht Mediendateien und Inhalte durchsuchbar und generiert Untertitelspuren und Schlüsselwörter. |[Azure Media Indexer](media-services-index-content.md) |
| Azure Media Hyperlapse (Vorschau) |Ermöglicht es Ihnen, die "Unregelmäßigkeiten" in Ihrem Video mithilfe von Videostabilisierung auszugleichen. Darüber hinaus können Sie Ihre Inhalte beschleunigen, um einen verwendbaren Clip zu erhalten. |[Azure Media Hyperlapse](media-services-hyperlapse-content.md) |
| Azure Media Encoder |Veraltet | |
| Storage Decryption |Veraltet | |
| Azure Media Packager |Veraltet | |
| Azure Media Encryptor |Veraltet | |

## <a name="get-media-processor"></a>Abrufen von Medienprozessoren
Das folgende Beispiel zeigt, wie Sie eine Medienprozessor-Instanz erstellen. Das folgende Codebeispiel setzt eine Variable auf Modulebene mit dem Namen **_context** voraus, die Zugriff auf den Serverkontext bietet. Siehe [Programmgesteuertes Herstellen einer Verbindung mit Media Services](media-services-dotnet-connect-programmatically.md).

    private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
        ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

        if (processor == null)
        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

        return processor;
    }


## <a name="media-services-learning-paths"></a>Media Services-Lernpfade
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Nächste Schritte
Sie sind nun in der Lage, eine Medienprozessorinstanz zu erstellen, und können mit dem Thema [Codieren von Medienobjekten](media-services-dotnet-encode-with-media-encoder-standard.md) fortfahren. Dort lernen Sie, wie Sie Medienobjekte mit dem Media Encoder Standard codieren können.




<!--HONumber=Nov16_HO3-->


