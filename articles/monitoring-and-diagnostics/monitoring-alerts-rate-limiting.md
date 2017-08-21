---
title: "Ratenlimits für SMS, E-Mail-Nachrichten und Webhooks | Microsoft-Dokumentation"
description: "Lassen Sie sich per SMS, Webhook und E-Mail benachrichtigen, wenn bestimmte Ereignisse im Aktivitätsprotokoll eintreten."
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
ms.sourcegitcommit: cddb80997d29267db6873373e0a8609d54dd1576
ms.openlocfilehash: b2e954405500921c0c1e9c7cd71ce57130c98d64
ms.contentlocale: de-de
ms.lasthandoff: 07/18/2017

---

# <a name="rate-limiting-for-sms-emails-and-webhooks"></a>Ratenlimits für SMS, E-Mail-Nachrichten und Webhooks
Das Ratenlimit führt zu einer Unterbrechung der Benachrichtigungen, wenn zu viele Benachrichtigungen an eine bestimmte Telefonnummer oder E-Mail-Adresse gesendet werden. Die Begrenzung der Übertragungsrate stellt sicher, dass Warnungen verwaltbar und verfolgbar sind.

Die Regeln sind für SMS und E-Mail identisch. Folgende Ratenlimits gelten:
 - SMS: 10 Nachrichten pro Stunde
 - E-Mail: 100 Nachrichten pro Stunde

## <a name="rate-limit-rules"></a>Regeln für Ratenlimits
- Für eine bestimmte Telefonnummer oder E-Mail-Adresse wird die Rate begrenzt, wenn sie mehr als den Schwellenwert an Nachrichten erhält.
- Eine Telefonnummer oder E-Mail-Adresse kann Mitglied von Aktionsgruppen mehrerer Abonnements sein. Die Ratenlimits gelten für alle Abonnements, d.h., sie gelten, sobald der Schwellenwert erreicht wird, auch wenn von mehreren Abonnements gesendet wurde.  
- Wenn für eine Telefonnummer oder E-Mail-Adresse ein Ratenlimit gilt, wird eine weitere Benachrichtigung gesendet, um über das Ratenlimit zu informieren. In der Benachrichtigung wird angegeben, wann die Ratenbegrenzung abläuft.

## <a name="rate-limit-of-webhooks"></a>Ratenlimit von Webhooks ##
Es gibt derzeit kein Ratenlimit für Webhooks.

## <a name="next-steps"></a>Nächste Schritte ##
Weitere Informationen zum [Verhalten von SMS-Warnungen](monitoring-sms-alert-behavior.md)  
Verschaffen Sie sich einen [Übersicht über Aktivitätsprotokollwarnungen](monitoring-overview-alerts.md), und erfahren Sie, wie Sie gewarnt werden können.  
[Konfigurieren von Warnungen, wenn eine Dienstintegritätsbenachrichtigung gesendet wird](monitoring-activity-log-alerts-on-service-notifications.md)

