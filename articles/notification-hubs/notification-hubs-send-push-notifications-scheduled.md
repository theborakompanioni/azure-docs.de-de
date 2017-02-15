---
title: 'Gewusst wie: Senden geplanter Benachrichtigungen | Microsoft Docs'
description: In diesem Thema wird beschrieben, wie Sie geplante Benachrichtigungen mit Azure Notification Hubs verwenden.
services: notification-hubs
documentationcenter: .net
keywords: Pushbenachrichtigungen,Pushbenachrichtigung,Planen von Pushbenachrichtigungen
author: ysxu
manager: erikre
editor: 
ms.assetid: 6b718c75-75dd-4c99-aee3-db1288235c1a
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: efac6e1ecc00359f1622d380333140bc055c83e0


---
# <a name="how-to-send-scheduled-notifications"></a>Gewusst wie: Senden von geplanten Benachrichtigungen
## <a name="overview"></a>Übersicht
Stellen Sie sich ein Szenario vor, bei dem Sie zu einem späteren Zeitpunkt eine Benachrichtigung senden möchten, aber keine einfache Möglichkeit zur Aktivierung des Back-End-Codes zum Senden der Benachrichtigung besteht. Notification Hubs des Standard-Tarifs unterstützen ein Feature, mit dem Sie Benachrichtigungen bis zu sieben Tage im Voraus planen können.

Verwenden Sie beim Senden einer Benachrichtigung einfach die [ScheduledNotification](https://msdn.microsoft.com/library/microsoft.azure.notificationhubs.schedulednotification.aspx)-Klasse im Notification Hubs SDK, wie im folgenden Beispiel veranschaulicht:

    Notification notification = new AppleNotification("{\"aps\":{\"alert\":\"Happy birthday!\"}}");
    var scheduled = await hub.ScheduleNotificationAsync(notification, new DateTime(2014, 7, 19, 0, 0, 0));

Sie können eine zuvor geplante Benachrichtigung auch abbrechen, indem Sie die entsprechende notificationId verwenden:

    await hub.CancelNotificationAsync(scheduled.ScheduledNotificationId);

Es gibt keine Beschränkung der Anzahl von geplanten Benachrichtigungen, die Sie senden können.




<!--HONumber=Nov16_HO3-->


