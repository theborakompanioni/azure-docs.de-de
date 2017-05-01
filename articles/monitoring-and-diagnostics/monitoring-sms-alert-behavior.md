---
title: SMS-Warnungsverhalten in Aktionsgruppen | Microsoft-Dokumentation
description: "SMS-Nachrichtenformat und Antworten auf SMS-Nachrichten zum Kündigen des Abonnements, erneuten Abonnieren oder Anfordern von Hilfe."
author: anirudhcavale
manager: carmonm
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
translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: 10f33898fb86bd2449994a153d99cb59dc6078d6
ms.lasthandoff: 03/31/2017


---
# <a name="sms-alert-behavior-in-action-groups"></a>SMS-Warnungsverhalten in Aktionsgruppen
## <a name="overview"></a>Übersicht ##
Mit Aktionsgruppen können Sie eine Empfängerliste konfigurieren. Diese Gruppen können beim Definieren von Aktivitätsprotokollwarnungen genutzt werden, um sicherzustellen, dass eine bestimmte Aktivitätsgruppe benachrichtigt wird, wenn die Aktivitätsprotokollwarnung ausgelöst wird. Einer der unterstützten Warnungsmechanismen ist SMS; die Warnungen unterstützen die bidirektionale Kommunikation. Ein Benutzer kann auf eine der folgenden Weisen auf eine Warnung reagieren:

- **Abonnieren von Warnungen kündigen:** Ein Benutzer kann das Abonnieren aller SMS-Warnungen für alle Aktionsgruppen oder eine einzelne Aktionsgruppe kündigen.  
- **Warnungen neu abonnieren:** Ein Benutzer kann alle SMS-Warnungen für alle Aktionsgruppen oder eine einzelne Aktionsgruppe neu abonnieren.  
- **Hilfe anfordern:** Ein Benutzer kann weitere Informationen per SMS anfordern. Er wird zu diesem Artikel weitergeleitet.

Dieser Artikel behandelt das Verhalten der SMS-Warnungen und die Antwortaktionen, die der Benutzer durchführen kann, basierend auf dem Gebietsschema des Benutzers:

## <a name="usacanada-sms-behavior"></a>SMS-Verhalten in USA/Kanada
### <a name="receiving-an-sms-alert"></a>Empfangen einer SMS-Warnung
Ein SMS-Empfänger, der als Teil einer Aktionsgruppe konfiguriert ist, erhält eine SMS, wenn eine Warnung ausgelöst wird. Die SMS enthält folgende Informationen:
* Kurzname der Aktionsgruppe, an die diese Warnung gesendet wurde.
- Titel der Warnung.

### <a name="unsubscribing-from-sms-alerts-for-one-action-group"></a>Kündigen des Abonnements von SMS-Warnungen für eine Aktionsgruppe
Ein Benutzer kann das Abonnement von SMS-Warnungen für eine Aktionsgruppe kündigen, indem er auf den kurzen Code 20873 mit folgenden Schlüsselwörtern antwortet: „DISABLE &lt;Kurzname der Aktionsgruppe&gt;“.

Beispiel: Ein Benutzer, der das Abonnement von Warnungen für eine Aktionsgruppe mit dem Kurznamen „Azure“ kündigen möchte, sendet eine SMS mit dem Wortlaut „DISABLE Azure“ an den kurzen Code 20873.

### <a name="unsubscribing-from-sms-alerts-for-all-action-groups"></a>Kündigen des Abonnements von SMS-Warnungen für alle Aktionsgruppen
Ein Benutzer kann das Abonnement aller SMS-Warnungen für alle Aktionsgruppen kündigen, indem er auf den kurzen Code 20873 mit einem der folgenden Schlüsselwörter antwortet:
* STOP

Beispiel: Ein Benutzer, der das Abonnement aller SMS-Warnungen für alle Aktionsgruppen kündigen möchte, sendet eine SMS mit dem Wortlaut „STOP“ an den kurzen Code 20873.

>[!NOTE]
>Wenn ein Benutzer das Abonnement aller SMS-Warnungen gekündigt hat, aber dann einer neuen Aktionsgruppe hinzugefügt wird, EMPFÄNGT er SMS-Warnungen für diese neue Aktionsgruppe, doch die Kündigung des Abonnements für alle vorherigen Aktionsgruppen bleibt bestehen.
>
>

### <a name="resubscribing-to-sms-alerts-for-one-action-group"></a>Erneutes Abonnieren von SMS-Warnungen für eine Aktionsgruppe
Ein Benutzer kann SMS-Warnungen für eine Aktionsgruppe erneut abonnieren, indem er auf den kurzen Code 20873 mit folgenden Schlüsselwörtern antwortet: „ENABLE &lt;Kurzname der Aktionsgruppe&gt;“.

Beispiel: Ein Benutzer, der Warnungen für eine Aktionsgruppe mit dem Kurznamen „Azure“ erneut abonnieren möchte, sendet eine SMS mit dem Wortlaut „ENABLE Azure“ an den kurzen Code 20873.

### <a name="resubscribing-to-sms-alerts-for-all-action-groups"></a>Erneutes Abonnieren von SMS-Warnungen für alle Aktionsgruppen
Ein Benutzer kann alle SMS-Warnungen für alle Aktionsgruppen erneut abonnieren, indem er auf den kurzen Code 20873 mit einem der folgenden Schlüsselwörter antwortet:

* START

Beispiel: Ein Benutzer, der alle SMS-Warnungen für alle Aktionsgruppen erneut abonnieren möchte, sendet eine SMS mit dem Wortlaut „START“ an den kurzen Code 20873.

### <a name="requesting-help-via-sms"></a>Anfordern von Hilfe über SMS
Ein Benutzer kann weitere Informationen zu der empfangenen SMS anfordern, indem er mit einem der folgenden Schlüsselwörter auf den kurzen Code 20873 antwortet:
* HELP

Der Benutzer erhält eine Antwort mit einem Link zu diesem Artikel.

## <a name="next-steps"></a>Nächste Schritte
Verschaffen Sie sich einen [Überblick über Aktivitätsprotokollwarnungen](monitoring-overview-alerts.md), und erfahren Sie, wie Sie gewarnt werden können.  
Erfahren Sie mehr über die [SMS-Rateneinschränkung](monitoring-alerts-rate-limiting.md).  
Erfahren Sie mehr über [Aktionsgruppen](monitoring-action-groups.md).

