---
title: Erstellen eines Service Bus-Namespace im Azure-Portal | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie einen Service Bus-Namespace mithilfe des Azure-Portals erstellen.
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
ms.date: 03/23/2017
ms.author: sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: 0bec803e4b49f3ae53f2cc3be6b9cb2d256fe5ea
ms.openlocfilehash: 5de92033eb7eb4fef8d27a215b3284ab80594065
ms.contentlocale: de-de
ms.lasthandoff: 03/24/2017


---
# <a name="create-a-service-bus-namespace-using-the-azure-portal"></a>Erstellen eines Service Bus-Namespace mithilfe des Azure-Portals
Ein Namespace ist ein allgemeiner Container für alle Messagingkomponenten. Ein einzelner Namespace kann mehrere Warteschlangen und Themen enthalten, und Namespaces fungieren häufig als Anwendungscontainer. Es gibt zwei verschiedene Verfahren zum Erstellen von Service Bus-Namespaces:

1. Azure-Portal (dieser Artikel)
2. [Resource Manager-Vorlagen][create-namespace-using-arm]

## <a name="create-a-namespace-in-the-azure-portal"></a>Erstellen eines Namespace im Azure-Portal
[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Glückwunsch! Sie haben nun einen Service Bus Messaging-Namespace erstellt.

## <a name="next-steps"></a>Nächste Schritte
Sehen Sie sich die [GitHub-Beispiele][github-samples] an, die einige erweiterte Features von Azure Service Bus Messaging veranschaulichen.

[create-namespace-using-arm]: service-bus-resource-manager-overview.md
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples

