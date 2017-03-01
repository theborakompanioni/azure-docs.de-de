---
title: WebJobs in Azure App Service
description: "Erfahren Sie, wie Sie WebJobs erstellen, um Hintergrundtests auszuführen, mit Diensten wie Speicher und Service Bus zu interagieren und geplante Tasks zu erstellen."
services: app-service
documentationcenter: 
author: christopheranderson
manager: erikre
editor: mollybos
ms.assetid: 85975432-04c9-4b83-b937-b30c082d52a1
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/10/2015
ms.author: chrande
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 45016e09e981bab6e4413048838c66f1108dc663
ms.lasthandoff: 11/17/2016


---
# <a name="using-webjobs-in-azure-app-service"></a>Verwenden von WebJobs in Azure App Service
Dieser Artikel enthält Links zu Dokumentationsressourcen zur Verwendung von Azure WebJobs und dem Azure WebJobs SDK. Azure WebJobs bietet eine einfache Möglichkeit zum Ausführen von Skripts oder Programmen als Hintergrundprozesse in [App Service-Web-Apps](http://go.microsoft.com/fwlink/?LinkId=529714). Sie können eine ausführbare Datei, z. B. eine CMD-, BAT-, EXE (.NET)-, PS1-, SH-, PHP, PY-, JS- und JAR-Datei, hochladen und ausführen. Diese Programme werden als Webaufträge nach einem Zeitplan (Cron) oder laufend ausgeführt.

Das WebJobs-SDK vereinfacht die Verwendung des Azure-Speichers. Das WebJobs-SDK verfügt über ein Bindungs- und Auslösersystem, das Microsoft Azure-Speicherblobs, Warteschlangen und Tabellen sowie Service Bus-Warteschlangen verwendet.

Das Erstellen, Bereitstellen und Verwalten von WebJobs erfolgt dank der integrierten Tools in Visual Studio nahtlos. Sie können Webaufträge aus Vorlagen erstellen, veröffentlichen und verwalten (Ausführen/Anhalten/Überwachen/Debuggen).

Das WebJobs-Dashboard im Azure-Portal bietet leistungsstarke Verwaltungsfunktionen, mit denen Sie vollständige Kontrolle über die Ausführung von Webaufträgen haben, einschließlich der Möglichkeit, einzelne Funktionen innerhalb von WebJobs aufzurufen. Das Dashboard zeigt auch Funktionslaufzeiten und Protokollausgaben an.

[!INCLUDE [app-service-blueprint-webjobs](../../includes/app-service-blueprint-webjobs.md)]


