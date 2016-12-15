---
title: Erstellen eines Service Bus-Namespace mithilfe des Azure-Portals | Microsoft Docs
description: "Für den Einstieg in Service Bus benötigen Sie einen Namespace. Hier wird erläutert, wie Sie einen Namespace mithilfe des Azure-Portals erstellen."
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
ms.sourcegitcommit: 0b1f6f7ec47e47f39407cdbfd5efef2a18944ecc
ms.openlocfilehash: 89610512c264cef4bd52728240ddc0a34091edea


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



<!--HONumber=Dec16_HO1-->


