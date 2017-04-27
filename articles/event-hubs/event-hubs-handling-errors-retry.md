---
title: "Bewährte Methoden für die Fehlerbehandlung bei Azure Event Hubs | Microsoft-Dokumentation"
description: Behandeln von Fehlern und Wiederholen bei Azure Event Hubs
services: event-hubs
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/09/2017
ms.author: sethm
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 200bc0828574bca72739dea5badb22da129cd896
ms.lasthandoff: 04/12/2017

---

# <a name="-event-hubs-best-practices-for-handling-errors-and-retry"></a>🔧 Bewährte Methoden für Fehlerbehandlung und Wiederholung bei Event Hubs

> [!NOTE]
> 
> Dieses Thema wurde noch nicht verfasst! 
>
> Wir freuen uns über Ihre Beiträge in Bezug auf Umfang und Ausrichtung dieser Inhalte. Sie können den Status dieses [Themas in GitHub](https://github.com/Azure/azure-event-hubs/issues/305) verfolgen und Beiträge einreichen.
> 
> Erfahren Sie mehr darüber, wie Sie Beiträge in [GitHub](https://github.com/Microsoft/azure-docs/blob/master/contributor-guide/contributor-guide-index.md) einreichen können.

In diesem Artikel wird Folgendes behandelt:

- Welche verschiedenen Arten von Fehlern gibt es?
- Was geschieht, wenn ein Fehler ausgegeben wird?
- Bei welchen Arten von Fehlern empfiehlt sich eine Wiederholung? Bei welchen ist dies nicht der Fall?
- Benutzerdefinierte Wiederholungsrichtlinie

