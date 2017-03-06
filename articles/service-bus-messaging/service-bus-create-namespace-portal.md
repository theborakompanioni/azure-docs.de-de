---
title: Erstellen eines Service Bus-Namespace im Azure-Portal | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie einen Service Bus-Namespace mithilfe des Azure-Portals erstellen.
services: service-bus-messaging
documentationcenter: .net
author: jtaubensee
manager: timlt
editor: 
ms.assetid: fbb10e62-b133-4851-9d27-40bd844db3ba
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 11/30/2016
ms.author: jotaub
translationtype: Human Translation
ms.sourcegitcommit: 4a972b9b8b52a90f27afda98d8bdc661016d1fe1
ms.openlocfilehash: ba7093f8a2c06ab4cecf11207174a4871435ae64
ms.lasthandoff: 02/28/2017


---
# <a name="create-a-service-bus-namespace-using-the-azure-portal"></a>Erstellen eines Service Bus-Namespace mithilfe des Azure-Portals
Ein Namespace ist ein allgemeiner Container für alle Messagingkomponenten. Ein einzelner Namespace kann mehrere Warteschlangen und Themen enthalten, und Namespaces fungieren häufig als Anwendungscontainer. Derzeit gibt es zwei verschiedene Verfahren zum Erstellen von Service Bus-Namespaces.

1. Azure-Portal (dieser Artikel)
2. [Resource Manager-Vorlagen][create-namespace-using-arm]

## <a name="create-a-namespace-in-the-azure-portal"></a>Erstellen eines Namespace im Azure-Portal
[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Glückwunsch! Sie haben nun einen Service Bus Messaging-Namespace erstellt.

## <a name="next-steps"></a>Nächste Schritte
Sehen Sie sich die [GitHub-Beispiele][github-samples] an, die einige erweiterte Features von Azure Service Bus Messaging veranschaulichen.

[create-namespace-using-arm]: service-bus-resource-manager-overview.md
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples

