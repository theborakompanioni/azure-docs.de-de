---
title: "Azure Media Services – Übersicht | Microsoft-Dokumentation"
description: "Dieses Thema bietet eine Übersicht über Azure Media Services."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 7a5e9723-c379-446b-b4d6-d0e41bd7d31f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/04/2017
ms.author: juliako;anilmur
ms.translationtype: Human Translation
ms.sourcegitcommit: bb794ba3b78881c967f0bb8687b1f70e5dd69c71
ms.openlocfilehash: 38c3637cc477e24ac8d056611e7fda9a4d693cdb
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017

---
# <a name="azure-media-services-overview"></a>Azure Media Services – Übersicht 

Microsoft Azure Media Services ist eine erweiterbare, cloudbasierte Plattform, die Entwicklern das Erstellen von skalierbaren Medienverwaltungslösungen und Bereitstellungsanwendungen ermöglicht. Media Services basiert auf REST-APIs, mit denen Sie auf sichere Weise Video- oder Audioinhalte hochladen, speichern, codieren und verpacken können – sowohl für eine bedarfsgesteuerte als auch für eine auf Livestreaming basierende Bereitstellung auf verschiedenen Clients (z.B. TV, PC und mobile Geräte).

Sie können mithilfe von Media Services vollständige End-to-End-Workflows erstellen. Es ist auch möglich, Drittanbieterkomponenten für einige Elemente Ihres Workflows zu nutzen. Beispielsweise können Sie die Codierung mit einem Encoder eines Drittanbieters durchführen. Anschließend sorgen Sie mit Media Services für Upload, Schutz, Paketierung und Bereitstellung.

Sie können Ihre Inhalte live streamen oder sie bei Bedarf bereitstellen. Das Thema enthält außerdem Links zu anderen relevanten Themen.

## <a name="media-services-learning-paths"></a>Media Services-Lernpfade
Sie können sich die AMS-Lernpfade hier ansehen:

* [Media Services - Live Streaming (in englischer Sprache)](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-live/)
* [Media Services – On-Demand-Streaming](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-on-demand/)

## <a name="prerequisites"></a>Voraussetzungen

Um mit Azure Media Services loszulegen, sollten Sie Folgendes haben:

* Ein Azure-Konto. Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Weitere Informationen finden Sie unter [Kostenloses Azure-Testkonto](https://azure.microsoft.com).
* Ein Azure Media Services-Konto. Weitere Informationen finden Sie unter [Erstellen von Konten](media-services-portal-create-account.md).
* (Optional) Eine eingerichtete Entwicklungsumgebung. Wählen Sie .NET oder REST-API für Ihre Entwicklungsumgebung. Weitere Informationen finden Sie unter [Einrichten der Umgebung](media-services-dotnet-how-to-use.md).

    Erfahren Sie außerdem, wie Sie programmgesteuert eine [Verbindung mit AMS-API](media-services-use-aad-auth-to-access-ams-api.md) herstellen können.
* Einen Standard- oder Premium-Streamingendpunkt im Status „Gestartet“.  Weitere Informationen finden Sie unter [Verwalten von Streamingendpunkten](media-services-portal-manage-streaming-endpoints.md).

## <a name="sdks-and-tools"></a>SDKs und Tools

Zum Entwickeln von Media Services-Lösungen können Sie folgende Komponenten verwenden:

* [Media Services-REST-API](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference)
* Eines der verfügbaren Client-SDKs:
    * [Azure Media Services SDK für .NET](https://github.com/Azure/azure-sdk-for-media-services)
    * [Azure SDK für Java](https://github.com/Azure/azure-sdk-for-java)
    * [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php)
    * [Azure Media Services für Node.js](https://github.com/michelle-becker/node-ams-sdk/blob/master/lib/request.js) (Dies ist eine nicht von Microsoft stammende Version des Node.js SDK. Sie wird von einer Community verwaltet und bietet derzeit keine 100 %-ige Abdeckung der AMS-APIs).
* Vorhandene Tools:
    * [Azure-Portal](https://portal.azure.com/)
    * [Azure Media Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (Azure Media Services Explorer [AMSE] ist eine WinForms-/C#-Anwendung für Windows)

## <a name="concepts-and-overview"></a>Konzepte und Übersicht
Azure Media Services-Konzepte finden Sie unter [Konzepte](media-services-concepts.md).

Eine Reihe mit Vorgehensweisen (Gewusst wie), in der die wichtigsten Komponenten von Azure Media Services beschrieben werden, finden Sie unter [Azure Media Services Step-by-Step tutorials](https://docs.com/fukushima-shigeyuki/3439/english-azure-media-services-step-by-step-series)(Schritt-für-Schritt-Tutorials zu Azure Media Services). Diese Serie bietet eine hervorragende Übersicht über die Konzepte und verwendet das AMSE-Tool, um die AMS-Aufgaben zu erläutern. Beim AMSE-Tool handelt es sich um ein Windows-Tool. Dieses Tool unterstützt die meisten Aufgaben, die Sie programmgesteuert mit dem [AMS SDK für .NET](https://github.com/Azure/azure-sdk-for-media-services), dem [Azure SDK für Java](https://github.com/Azure/azure-sdk-for-java) oder dem [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php) durchführen können.

## <a name="supported-scenarios-and-availability-of-media-services-across-data-centers"></a>Unterstützte Szenarien und rechenzentrumsübergreifende Verfügbarkeit von Media Services

Ausführliche Informationen finden Sie unter [AMS scenarios and availability of features and services across data centers](scenarios-and-availability.md) (AMS-Szenarien und rechenzentrumsübergreifende Verfügbarkeit von Funktionen und Diensten).

## <a name="service-level-agreement-sla"></a>Vereinbarung zum Servicelevel (SLA)

* Media Services gewährleistet eine Verfügbarkeit von 99,9 % für die REST-API-Transaktionen zur Media Services-Codierung.
* Das Streaming sorgt für eine erfolgreiche Verarbeitung von Dienstanforderungen mit einer garantierten Verfügbarkeit von 99,9% für vorhandene Medieninhalte, wenn ein Standard- oder Premium-Streamingendpunkt erworben wird.
* Für Livekanäle wird garantiert, dass ausgeführte Kanäle mindestens 99,9 % der Zeit über externe Konnektivität verfügen.
* Für den Schutz von Inhalten garantieren wir, dass Schlüsselanforderungen mindestens 99,9 % der Zeit erfolgreich bearbeitet werden.
* Für Indexer werden Indexer-Aufgabenanforderungen, die mit einer reservierten Einheit für die Codierung verarbeitet werden, 99,9 % der Zeit erfolgreich bearbeitet.

Weitere Informationen finden Sie im [Microsoft Azure-SLA](https://azure.microsoft.com/support/legal/sla/).

Weitere Informationen zur Verfügbarkeit in Rechenzentren finden Sie im Abschnitt [Verfügbarkeit](scenarios-and-availability.md#availability).

## <a name="support"></a>Support

[Azure-Support](https://azure.microsoft.com/support/options/) bietet Supportoptionen für Azure, Media Services eingeschlossen.

## <a name="provide-feedback"></a>Feedback geben

[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

