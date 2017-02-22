---
title: Verwalten von Azure CDN-Zwischenspeicherungsrichtlinien in Azure Media Services | Microsoft-Dokumentation
description: "Hier wird erläutert, wie Sie Azure CDN-Zwischenspeicherungsrichtlinien in Azure Media Services verwalten."
services: media-services,cdn
documentationcenter: .NET
author: juliako
manager: erikre
editor: 
ms.assetid: be33aecc-6dbe-43d7-a056-10ba911e0e94
ms.service: media-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/04/2017
ms.author: juliako
translationtype: Human Translation
ms.sourcegitcommit: fba5384acae53ac416d91ae30840c6cfa04e7c68
ms.openlocfilehash: 0c479a58f4158bb1a72dc43432507160f65d2791


---
# <a name="manage-azure-cdn-caching-policy-in-azure-media-services"></a>Verwalten von Azure CDN-Zwischenspeicherungsrichtlinien in Azure Media Services
Azure Media Services bietet HTTP-basiertes adaptives Streaming und progressive Downloads. HTTP-basiertes Streaming ist hochgradig skalierbar und bietet den Vorteil, dass Dateien auf Proxy- und CDN-Ebene sowie clientseitig zwischengespeichert werden können. Streamingendpunkte bieten allgemeine Streamingfunktionen sowie Konfigurationsoptionen für HTTP-Cacheheader. Streamingendpunkte legen die HTTP-Cachesteuerungsheader "max-age" und "Expires" fest. Weitere Informationen zu HTTP-Cacheheadern finden Sie unter [W3.org](http://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html).

## <a name="default-caching-headers"></a>Standardcachingheader
Standardmäßig wenden Streamingendpunkte Cacheheader zum Zwischenspeichern für 3 Tage auf On-Demand-Streamingdaten (tatsächliche Medienfragmente/-segmente) und Manifeste (Wiedergabelisten) an. Beim Livestreaming wenden Streamingendpunkte Cacheheader zum Zwischenspeichern für 3 Tage auf Daten (tatsächliche Medienfragmente/-segmente) und für 2 Sekunden auf Manifeste (Wiedergabelisten) an. Wenn ein Liveprogramm in ein On-Demand-Programm umgewandelt wird (Livearchiv), werden die Cacheheader für On-Demand-Streaming angewendet.

## <a name="azure-cdn-integration"></a>Azure CDN-Integration
Azure Media Services bietet ein [integriertes CDN](https://azure.microsoft.com/updates/azure-media-services-now-fully-integrated-with-azure-cdn/) für Streamingendpunkte. Cachesteuerungsheader werden auf CDN-aktivierte Streamingendpunkte genauso angewendet wie Streamingendpunkte. Azure CDN verwendet für Streamingendpunkte konfigurierte Cachewerte, um die Lebensdauer der intern zwischengespeicherten Objekte zu definieren, und verwendet diesen Wert, um die Übermittlungscacheheader festzulegen. Bei der Verwendung von CDN-aktivierten Streamingendpunkten empfiehlt es sich nicht, kleine Cachewerte festzulegen. Das Festlegen von kleinen Werten senkt die Leistung und reduziert die Vorteile von CDN. Es ist nicht zulässig, Cacheheader für CDN-aktivierte Streamingendpunkte auf Werte unterhalb von 600 Sekunden festzulegen.

> [!IMPORTANT]
>Azure Media Services kann vollständig in Azure CDN integriert werden. Mit nur einem Mausklick können Sie alle verfügbaren Azure CDN-Anbieter (Akamai und Verizon) in Ihren Streamingendpunkt integrieren, einschließlich Standard- und Premium-Produkte von CDN. Weitere Informationen dazu finden Sie in [dieser Ankündigung](https://azure.microsoft.com/blog/standardstreamingendpoint/).
> 
> Datengebühren für die Übertragung von Daten vom Streamingendpunkt an das CDN fallen nur dann nicht an, wenn das CDN über Streamingendpunkt-APIs oder über den Streamingendpunkt-Abschnitt im Azure-Verwaltungsportal aktiviert wird. Bei einer manuellen Integration oder der direkten Erstellung eines CDN-Endpunkts über CDN-APIs oder den Portalabschnitt fallen Datengebühren an.

## <a name="configuring-cache-headers-with-azure-media-services"></a>Konfigurieren von Cacheheadern mit Azure Media Services
Sie können das Azure Verwaltungsportal oder Azure Media Services-APIs verwenden, um Cacheheaderwerte zu konfigurieren.

1. Informationen zum Konfigurieren von Cacheheadern über das Verwaltungsportal finden Sie unter [Verwalten von Streamingendpunkten](../media-services/media-services-portal-manage-streaming-endpoints.md) im Abschnitt "Konfigurieren des Streamingendpunkts".
2. Azure Media Services-REST-API, [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx#StreamingEndpointCacheControl).
3. Azure Media Services .NET SDK, [StreamingEndpointCacheControl-Eigenschaften](http://go.microsoft.com/fwlink/?LinkId=615302).

## <a name="cache-configuration-precedence-order"></a>Rangfolge der Cachekonfiguration
1. In Azure Media Services konfigurierte Cachewerte überschreiben Standardwerte.
2. Wenn keine manuelle Konfiguration vorhanden ist, werden die Standardwerte angewendet.
3. Cacheheader zum Zwischenspeichern für 2 Sekunden werden unabhängig von der Azure Media- oder Azure Storage-Konfiguration auf Livestreamingmanifeste (Wiedergabelisten) angewendet. Dieser Wert kann nicht überschrieben werden.




<!--HONumber=Feb17_HO1-->


