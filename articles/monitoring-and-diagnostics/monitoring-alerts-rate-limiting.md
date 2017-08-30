---
title: "Ratenlimits für SMS, E-Mail-Nachrichten und Webhooks | Microsoft-Dokumentation"
description: "Es wird erläutert, wie Azure die Anzahl der möglichen SMS-, E-Mail- oder Webhookbenachrichtigungen aus einer Aktionsgruppe beschränkt."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: ancav
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: bde645624ab1860d19ba18470f55845855a7d1fb
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---

# <a name="rate-limiting-for-sms-messages-emails-and-webhook-posts"></a>Ratenlimits für SMS-Nachrichten, E-Mail-Nachrichten und Webhooks
Das Ratenlimit führt zu einer Unterbrechung der Benachrichtigungen, wenn zu viele Benachrichtigungen an eine bestimmte Telefonnummer oder E-Mail-Adresse gesendet werden. Das Ratenlimit stellt sicher, dass Warnungen verwaltbar und verfolgbar sind.

Die Regeln sind für SMS und E-Mail identisch. Folgende Ratenlimits gelten:

 - **SMS**: 10 Nachrichten pro Stunde.
 - **E-Mail**: 100 Nachrichten pro Stunde.

## <a name="rate-limit-rules"></a>Regeln für Ratenlimits
- Für eine bestimmte Telefonnummer oder E-Mail-Adresse wird die Rate begrenzt, wenn sie mehr Nachrichten erhält, als der Schwellenwert zulässt.
- Eine Telefonnummer oder E-Mail-Adresse kann Mitglied von Aktionsgruppen mehrerer Abonnements sein. Das Ratenlimit gilt für alle Abonnements. Es wird wirksam, sobald der Schwellenwert erreicht wird, auch wenn Nachrichten aus mehreren Abonnements gesendet werden.  
- Wenn für eine Telefonnummer oder E-Mail-Adresse ein Ratenlimit gilt, wird eine weitere Benachrichtigung gesendet, um über das Ratenlimit zu informieren. In der Benachrichtigung wird angegeben, wann die Ratenbegrenzung abläuft.

## <a name="rate-limit-of-webhooks"></a>Ratenlimit von Webhooks ##
Es gibt kein Ratenlimit für Webhooks.

## <a name="next-steps"></a>Nächste Schritte ##
* Erfahren Sie mehr über das [SMS-Warnungsverhalten in Aktionsgruppen](monitoring-sms-alert-behavior.md).
* Verschaffen Sie sich eine [Übersicht über Aktivitätsprotokollwarnungen](monitoring-overview-alerts.md), und erfahren Sie, wie Sie Warnungen empfangen können.  
* Erfahren Sie, wie Sie [Warnungen konfigurieren, wenn eine Dienstintegritätsbenachrichtigung gesendet wird](monitoring-activity-log-alerts-on-service-notifications.md).

