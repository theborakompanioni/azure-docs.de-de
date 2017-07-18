---
title: Erstellen eines Service Bus-Namespace im Azure-Portal | Microsoft-Dokumentation
description: Erstellen eines Service Bus-Namespace mithilfe des Azure-Portals
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: fbb10e62-b133-4851-9d27-40bd844db3ba
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 06/27/2017
ms.author: sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: ffb134085c8555b22a317213622ca6c9490497d8
ms.contentlocale: de-de
ms.lasthandoff: 06/28/2017

---
# Erstellen eines Service Bus-Namespace mithilfe des Azure-Portals
<a id="create-a-service-bus-namespace-using-the-azure-portal" class="xliff"></a>

Ein Namespace ist ein Bereichscontainer für alle Messagingkomponenten. Ein einzelner Namespace kann mehrere Warteschlangen und Themen enthalten, und Namespaces fungieren häufig als Anwendungscontainer. Es gibt zwei verschiedene Verfahren zum Erstellen von Service Bus-Namespaces:

1. Azure-Portal (dieser Artikel)
2. [Resource Manager-Vorlagen][create-namespace-using-arm]

## Erstellen eines Namespace im Azure-Portal
<a id="create-a-namespace-in-the-azure-portal" class="xliff"></a>

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Glückwunsch! Sie haben nun einen Service Bus Messaging-Namespace erstellt.

## Nächste Schritte
<a id="next-steps" class="xliff"></a>

Sehen Sie sich die [GitHub-Beispiele][github-samples] an, die einige erweiterte Features von Azure Service Bus Messaging veranschaulichen.

[create-namespace-using-arm]: service-bus-resource-manager-overview.md
[github-samples]: https://github.com/Azure/azure-service-bus/tree/master/samples

