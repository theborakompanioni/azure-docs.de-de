<properties
	pageTitle="Gewusst wie: Senden geplanter Benachrichtigungen | Microsoft Azure"
	description="In diesem Thema wird beschrieben, wie Sie geplante Benachrichtigungen mit Azure Notification Hubs verwenden."
	services="notification-hubs"
	documentationCenter=".net"
	keywords="Pushbenachrichtigungen,Pushbenachrichtigung,Planen von Pushbenachrichtigungen"
	authors="wesmc7777"
	manager="erikre"
	editor=""/>
<tags
	ms.service="notification-hubs"
	ms.workload="mobile"
	ms.tgt_pltfrm="mobile-android"
	ms.devlang="dotnet"
	ms.topic="article"
	ms.date="06/29/2016"
	ms.author="wesmc"/>

# Gewusst wie: Senden von geplanten Benachrichtigungen


##Übersicht

Stellen Sie sich ein Szenario vor, bei dem Sie zu einem späteren Zeitpunkt eine Benachrichtigung senden möchten, aber keine einfache Möglichkeit zur Aktivierung des Back-End-Codes zum Senden der Benachrichtigung besteht. Notification Hubs des Standard-Tarifs unterstützen ein Feature, mit dem Sie Benachrichtigungen bis zu sieben Tage im Voraus planen können.

Verwenden Sie beim Senden einer Benachrichtigung einfach die [ScheduledNotification](https://msdn.microsoft.com/library/microsoft.azure.notificationhubs.schedulednotification.aspx)-Klasse im Notification Hubs-SDK. Dies ist im folgenden Beispiel dargestellt:

	Notification notification = new AppleNotification("{"aps":{"alert":"Happy birthday!"}}");
	var scheduled = await hub.ScheduleNotificationAsync(notification, new DateTime(2014, 7, 19, 0, 0, 0));

Sie können eine zuvor geplante Benachrichtigung auch abbrechen, indem Sie die entsprechende notificationId verwenden:

	await hub.CancelNotificationAsync(scheduled.ScheduledNotificationId);

Es gibt keine Beschränkung der Anzahl von geplanten Benachrichtigungen, die Sie senden können.

<!---HONumber=AcomDC_0706_2016-->