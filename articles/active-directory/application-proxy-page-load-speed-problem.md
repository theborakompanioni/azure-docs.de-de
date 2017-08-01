---
title: Zu lange Ladezeiten bei einer Anwendungsproxyanwendung | Microsoft-Dokumentation
description: Behandlung von Leistungsproblemen beim Laden von Seiten mit dem Azure AD-Anwendungsproxy
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.translationtype: Human Translation
ms.sourcegitcommit: 0d6f6fb24f1f01d703104f925dcd03ee1ff46062
ms.openlocfilehash: e172f55d45bccfd126928225eb9f78da61a670d3
ms.contentlocale: de-de
ms.lasthandoff: 04/17/2017

---

# <a name="an-application-proxy-application-takes-too-long-to-load"></a>Zu lange Ladezeiten bei einer Anwendungsproxyanwendung

Dieser Artikel enthält Informationen zu möglicherweise auftretenden Leistungsproblemen beim Laden einer Azure AD-Anwendungsproxyanwendung und Schritte zur Behebung der Probleme.

## <a name="overview"></a>Übersicht
Wenn Ihre Anwendungen funktionieren, beim Laden jedoch lange Wartezeiten auftreten, können Sie möglicherweise einige kleine Anpassungen an der Netzwerktopologie vornehmen, um die Geschwindigkeit zu erhöhen. Eine Bewertung der unterschiedlichen Topologien finden Sie unter [Aspekte der Netzwerktopologie bei Verwendung des Azure Active Directory-Anwendungsproxys](https://docs.microsoft.com/azure/active-directory/application-proxy-network-topology-considerations).

Wenn diese Hinweise nicht hilfreich sind, liegen derzeit leider keine weiteren Empfehlungen zum Optimieren der Leistung vor. Wenn der Anwendungsproxydienst sich über mehrere Rechenzentren erstreckt, die möglicherweise größere räumliche Nähe zu Ihnen aufweisen, kann sich die Latenz direkt verbessern. Eine vollständige Liste der Azure-Rechenzentren finden Sie auf der [Latenztestseite](http://www.azurespeed.com/Azure/Latency). 

Rechenzentren mit Webanwendungsproxy-Dienst können Sie mit dem [Connectorports-Testtool](https://aadap-portcheck.connectorporttest.msappproxy.net/) suchen. 

## <a name="feedback-on-application-proxy-data-center-locations"></a>Feedback zu Anwendungsproxy-Rechenzentrumsstandorten 
Möglicherweise gibt es Azure-Rechenzentren, die noch keinen Anwendungsproxy haben, aber zu einer starken Leistungssteigerung bei der Latenz führen können. Senden Sie uns unter <aadapfeedback@microsoft.com> eine E-Mail zu Rechenzentrumsstandorten, sodass wir Ihr Feedback zur Planung von Erweiterungen nutzen können.

Wir arbeiten an einigen zusätzlichen Funktionen, mit deren Hilfe die Latenz für Mandanten verbessert wird, bei denen derzeit lange Wartezeiten auftreten. Wir werden die Dokumentation freigeben, sobald sie verfügbar ist.

## <a name="next-steps"></a>Nächste Schritte
[Verwenden von vorhandenen lokalen Proxyservern](application-proxy-working-with-proxy-servers.md)

