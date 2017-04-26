---
title: 'Batch-Analysen: Azure | Microsoft-Dokumentation'
ms.custom: 
ms.date: 2017-02-01
ms.prod: azure
ms.reviewer: 
ms.service: batch
ms.suite: 
ms.tgt_pltfrm: 
ms.topic: reference
ms.assetid: 2fda4d9c-f782-4088-9320-656b450e3100
caps.latest.revision: 7
author: tamram
ms.author: tamram
manager: timlt
translationtype: Human Translation
ms.sourcegitcommit: 0c4554d6289fb0050998765485d965d1fbc6ab3e
ms.openlocfilehash: 4b73637e0ec6798b701d5a13f96c504bcbb14cc7
ms.lasthandoff: 04/13/2017

---
# <a name="batch-analytics"></a>Batch-Analysen
Die Themen in „Batch-Analysen“ enthalten Referenzinformationen zu den Ereignissen und Warnungen, die für Batch-Dienstressourcen verfügbar sind.

Unter [Diagnoseprotokollierung für Azure Batch](https://azure.microsoft.com/documentation/articles/batch-diagnostics/) finden Sie weitere Informationen zum Aktivieren und Nutzen von Batch-Diagnoseprotokollen.

## <a name="diagnostic-logs"></a>Diagnoseprotokolle

Der Azure Batch-Dienst gibt im Verlauf der Lebensdauer bestimmter Batch-Ressourcen die folgenden Diagnoseprotokollereignisse aus.

**Dienstprotokollereignisse**
* [Erstellung eines Pools](batch-pool-create-event.md)
* [Start des Löschvorgangs eines Pools](batch-pool-delete-start-event.md)
* [Abschluss des Löschvorgangs eines Pools](batch-pool-delete-complete-event.md)
* [Start der Größenänderung eines Pools](batch-pool-resize-start-event.md)
* [Abschluss der Größenänderung eines Pools](batch-pool-resize-complete-event.md)
* [Taskstart](batch-task-start-event.md)
* [Taskabschluss](batch-task-complete-event.md)
* [Taskfehler](batch-task-fail-event.md)
