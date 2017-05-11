---
title: Erstellen eines Azure Event Hubs | Microsoft-Dokumentation
description: Erstellen eines Azure Event Hubs-Namespace und eines Event Hubs mithilfe des Azure-Portals
services: event-hubs
documentationcenter: na
author: jtaubensee
manager: timlt
editor: 
ms.assetid: ff99e327-c8db-4354-9040-9c60c51a2191
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/03/2017
ms.author: jotaub
ms.translationtype: Human Translation
ms.sourcegitcommit: 7c4d5e161c9f7af33609be53e7b82f156bb0e33f
ms.openlocfilehash: 2ef14fccda5c38ebdbf9d64b4766b45503755af0
ms.contentlocale: de-de
ms.lasthandoff: 05/04/2017

---

# <a name="create-an-event-hubs-namespace-and-an-event-hub-using-the-azure-portal"></a>Erstellen eines Event Hubs-Namespace und eines Event Hubs mithilfe des Azure-Portals

## <a name="create-an-event-hubs-namespace"></a>Erstellen eines Event Hubs-Namespace
1. Melden Sie sich beim [Azure-Portal][Azure portal] an, und klicken Sie oben links auf dem Bildschirm auf **Neu**.
1. Klicken Sie auf **Internet der Dinge**, und klicken Sie dann auf **Event Hubs**.
   
    ![](./media/event-hubs-create/create-event-hub9.png)
1. Geben Sie auf dem Blatt **Namespace erstellen** einen Namen für den Namespace ein. Das System überprüft sofort, ob dieser Name verfügbar ist.
   
    ![](./media/event-hubs-create/create-event-hub1.png)
1. Ist der Name verfügbar, wählen Sie den Tarif („Basic“ oder „Standard“) aus. Wählen Sie außerdem ein Azure-Abonnement, eine Ressourcengruppe und einen Standort, an dem die Ressource erstellt werden soll. 
1. Klicken Sie auf **Erstellen** , um den Namespace zu erstellen.
1. Klicken Sie in der Liste der Event Hubs-Namespaces auf den neu erstellten Namespace.      
   
    ![](./media/event-hubs-create/create-event-hub2.png)
1. Klicken Sie auf dem Blatt mit den Namespaces auf **Event Hubs**.
   
    ![](./media/event-hubs-create/create-event-hub3.png)

## <a name="create-an-event-hub"></a>Erstellen eines Ereignis-Hubs

1. Klicken Sie oben auf dem Blatt auf **Event Hub hinzufügen**.
   
    ![](./media/event-hubs-create/create-event-hub4.png)
1. Geben Sie einen Namen für den Event Hub ein, und klicken Sie auf **Erstellen**.
   
    ![](./media/event-hubs-create/create-event-hub5.png)
1. Klicken Sie in der Liste der Event Hubs auf den Namen des neu erstellten Event Hubs. 
    
    ![](./media/event-hubs-create/create-event-hub6.png)
1. Klicken Sie auf dem Blatt mit Namespaces (nicht auf dem Blatt für den bestimmten Event Hub) auf **SAS-Richtlinien**, und klicken Sie dann auf **RootManageSharedAccessKey**.
    
    ![](./media/event-hubs-create/create-event-hub7.png)
1. Klicken Sie auf die Kopierschaltfläche, um die Verbindungszeichenfolge **RootManageSharedAccessKey** in die Zwischenablage zu kopieren. Speichern Sie diese Verbindungszeichenfolge für die spätere Verwendung in diesem Tutorial.
    
    ![](./media/event-hubs-create/create-event-hub8.png)

Ihr Event Hub wird jetzt erstellt, und Sie verfügen über die zum Senden und Empfangen von Ereignissen erforderlichen Verbindungszeichenfolgen.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Event Hubs finden Sie unter diesen Links:

* [Übersicht über Event Hubs](event-hubs-what-is-event-hubs.md)
* [Übersicht über die Event Hubs-API](event-hubs-api-overview.md)

[Azure portal]: https://portal.azure.com/
