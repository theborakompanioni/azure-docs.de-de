---
title: Anleitung zum Verwenden des Azure Service Bus-Connectors in Ihren Logik-Apps | Microsoft Docs
description: "Erstellen Sie Logik-Apps mit Azure App Service. Stellen Sie eine Verbindung mit Azure Service Bus her, um Nachrichten zu senden und zu empfangen. Sie können Aktionen ausführen wie Senden an eine Warteschlange, Senden an ein Thema, Empfangen aus der Warteschlange und Empfangen aus Abonnements usw."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: d6d14f5f-2126-4e33-808e-41de08e6721f
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/02/2016
ms.author: mandia; ladocs
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: e5d3301c90adf993b1cba35969dfe4dbeaeab499
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="get-started-with-the-azure-service-bus-connector"></a>Erste Schritte mit dem Azure Service Bus-Connector
Stellen Sie eine Verbindung mit Azure Service Bus her, um Nachrichten zu senden und zu empfangen. Sie können Aktionen ausführen wie Senden an eine Warteschlange, Senden an ein Thema, Empfangen aus der Warteschlange und Empfangen aus Abonnements usw.

Wenn Sie [einen Connector](apis-list.md) verwenden möchten, müssen Sie zuerst eine Logik-App erstellen. Erstellen Sie daher erst einmal eine Logik-App, wie [hier](../logic-apps/logic-apps-create-a-logic-app.md) beschrieben.

## <a name="connect-to-service-bus"></a>Herstellen einer Verbindung mit Service Bus
Damit Ihre Logik-App überhaupt auf einen Dienst zugreifen kann, müssen Sie zunächst eine Verbindung mit dem Dienst herstellen. Eine [Verbindung](connectors-overview.md) stellt den Kontakt zwischen einer Logik-App und einem anderen Dienst her.  

> [!INCLUDE [Steps to create a connection to Azure Service Bus](../../includes/connectors-create-api-servicebus.md)]
> 
> 

## <a name="use-a-service-bus-trigger"></a>Verwenden eines Service Bus-Triggers
Ein Trigger ist ein Ereignis, mit dem ein in einer Logik-App definierter Workflow gestartet werden kann. Weitere Informationen zu Triggern finden Sie [hier](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).  

> [!INCLUDE [Steps to create a Service Bus trigger](../../includes/connectors-create-api-servicebus-trigger.md)]
> 
> 

## <a name="use-a-service-bus-action"></a>Verwenden einer Service Bus-Aktion
Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. Weitere Informationen zu Aktionen finden Sie [hier](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

[!INCLUDE [Steps to create a Service Bus action](../../includes/connectors-create-api-servicebus-action.md)]

## <a name="view-the-swagger"></a>Anzeigen von Swagger
Weitere Informationen finden Sie unter [Details zu Swagger](/connectors/servicebus/). 

## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Logik-App](../logic-apps/logic-apps-create-a-logic-app.md)


