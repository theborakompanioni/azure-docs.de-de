---
title: "Was sind Dienstintegritätsbenachrichtigungen? | Microsoft-Dokumentation"
description: "Mit Dienstintegritätsbenachrichtigungen können Sie Dienstintegritätsmeldungen von Microsoft Azure anzeigen."
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
ms.translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: c7ea129346c5fddcff9f824dc69478cf6fb1c67a
ms.contentlocale: de-de
ms.lasthandoff: 03/31/2017

---
# <a name="service-health-notifications"></a>Dienstintegritätsbenachrichtigungen
## <a name="overview"></a>Übersicht

Dieser Artikel zeigt Ihnen, wie Sie Dienstintegritätsbenachrichtigungen im Azure-Portal anzeigen.

Mit Dienstintegritätsbenachrichtigungen können Sie Dienstintegritätsmeldungen des Azure-Teams anzeigen, die sich auf die Ressourcen unter Ihrem Abonnement auswirken könnten. Diese Benachrichtigungen sind eine Unterklasse von Aktivitätsprotokollereignissen und auch auf dem Aktivitätsprotokollblatt zu finden. Dienstintegritätsbenachrichtigungen können je nach Klasse informativ oder handlungsrelevant sein.

Es gibt fünf Klassen von Dienstintegritätsbenachrichtigungen:  

- **Aktion erforderlich:** Von Zeit zu Zeit stellen wir möglicherweise fest, dass auf Ihrem Konto etwas Ungewöhnliches stattfindet. Dann müssen wir dies vielleicht mit Ihnen gemeinsam klären. Wir werden Ihnen dann eine Benachrichtigung senden, in der wir Ihnen ausführlich mitteilen, welche Maßnahmen Sie ergreifen müssen, oder wie Sie sich an Azure-Engineering oder -Support wenden.  
- **Unterstützte Wiederherstellung:** Ein Ereignis ist aufgetreten, und Entwickler haben sich davon überzeugt, dass Sie weiterhin Beeinträchtigungen unterliegen. Das Engineeringteam muss direkt mit Ihnen zusammenarbeiten, um Ihre Dienste wiederherzustellen.  
- **Incident:** Ein Ereignis, das Dienste beeinträchtigt, beeinflusst derzeit eine oder mehrere Ressourcen in Ihrem Abonnement.  
- **Wartung:** Diese Benachrichtigung informiert Sie über eine geplante Wartungsaktivität, die eine oder mehrere Ressourcen in Ihrem Abonnement beeinträchtigen kann.  
- **Informationen:** Von Zeit für die Zeit senden wir Ihnen möglicherweise Benachrichtigungen, um Sie über potenzielle Optimierungen zu informieren, die Ihre Ressourcenverwendung verbessern könnten.  
- **Sicherheit:** Dringende sicherheitsbezogene Informationen bezüglich Ihrer auf Azure ausgeführten Lösung(en).

Jede Dienstintegritätsbenachrichtigung übermittelt Details zum Umfang und zu den Auswirkungen auf Ihre Ressourcen. Zu den Details gehören:

Eigenschaftenname | Beschreibung
-------- | -----------
channels | Einer der folgenden Werte: „Admin“, „Operation“
correlationId | Normalerweise eine GUID im Zeichenfolgenformat. Ereignisse, die zur gleichen Uber-Aktion gehören, haben normalerweise die gleiche correlationId.
eventDataId | Eindeutiger Bezeichner eines Ereignisses.
eventName | Der Titel des Ereignisses.
Einstellung | Ebene des Ereignisses. Einer der folgenden Werte: Critical, Error, Warning, Informational, Verbose
resourceProviderName | Name des Ressourcenanbieters für die betroffene Ressource.
resourceType| Der Ressourcentyp der betroffenen Ressource.
subStatus | Normalerweise der HTTP-Statuscode des entsprechenden REST-Aufrufs, kann aber auch andere Zeichenfolgen zum Beschreiben eines Unterstatus enthalten, z.B. diese häufigen Werte: OK (HTTP-Statuscode: 200), Erstellt (HTTP-Statuscode: 201), Akzeptiert (HTTP-Statuscode: 202), Kein Inhalt (HTTP-Statuscode: 204), Ungültige Anforderung (HTTP-Statuscode: 400), Nicht gefunden (HTTP-Statuscode: 404), Konflikt (HTTP-Statuscode: 409), Interner Serverfehler (HTTP-Statuscode: 500), Dienst nicht verfügbar (HTTP-Statuscode: 503), Gatewaytimeout (HTTP-Statuscode: 504).
eventTimestamp | Zeitstempel der Ereignisgenerierung durch den Azure-Dienst, der die zum Ereignis gehörende Anforderung verarbeitet hat.
submissionTimestamp |   Zeitstempel des Zeitpunkts, ab dem das Ereignis für Abfragen verfügbar war.
subscriptionId | Das Azure-Abonnement, in dem dieses Ereignis protokolliert wurde.
status | Zeichenfolge, die den Status des Vorgangs beschreibt. Gängige Werte sind z.B.: Started, In Progress, Succeeded, Failed, Active, Resolved.
operationName | Name des Vorgangs.
category | „ServiceHealth“
resourceId | Ressourcen-ID der betroffenen Ressource.
Properties.title | Der lokalisierte Titel für diese Kommunikation. Englisch ist die Standardsprache.
Properties.communication | Die lokalisierten Details der Kommunikation mit HTML-Markup. Englisch ist der Standard.
Properties.incidentType | Mögliche Werte: AssistedRecovery, ActionRequired, Information, Incident, Maintenance, Security
Properties.trackingId | Identifiziert den Incident, dem dieses Ereignis zugeordnet ist. Verwenden Sie diese Option, um die Ereignisse im Zusammenhang mit einem Vorfall zu korrelieren.
Properties.impactedServices | Ein JSON-Blob mit Escapezeichen, das die Dienste und Regionen beschreibt, die vom Incident betroffen sind. Eine Liste der Dienste, von denen jeder über einen ServiceName und eine Liste von ImpactedRegions verfügt, von denen jede einen RegionName hat.
Properties.defaultLanguageTitle | Die Kommunikation in Englisch.
Properties.defaultLanguageContent | Die Kommunikation in Englisch, entweder als HTML-Markup oder Nur-Text.
Properties.stage | Mögliche Werte für AssistedRecovery, ActionRequired, Information, Incident, Security: Active, Resolved. Für die Wartung lauten sie: Active, Planned, InProgress, Canceled, Rescheduled, Resolved, Complete.
Properties.communicationId | Die Kommunikation dieses Ereignisses ist zugeordnet.


## <a name="viewing-your-service-health-notifications-in-the-azure-portal"></a>Anzeigen Ihrer Dienstintegritätsbenachrichtigungen im Azure-Portal
1.  Navigieren Sie im [Portal](https://portal.azure.com) zum Dienst **Überwachen**.

    ![Überwachen](./media/monitoring-service-notifications/home-monitor.png)
2.  Klicken Sie auf die Option **Überwachen**, um das Blatt „Überwachen“ zu öffnen. Auf diesem Blatt sind alle Überwachungseinstellungen und -daten in einer konsolidierten Ansicht zusammengefasst. Zunächst wird der Abschnitt **Aktivitätsprotokoll** geöffnet.

3.  Klicken Sie jetzt auf den Abschnitt **Dienstbenachrichtigungen**.

    ![Überwachen](./media/monitoring-service-notifications/service-health-summary.png)
4.  Klicken Sie auf eine beliebige Position, um weitere Details anzuzeigen.

5. Klicken Sie auf den Vorgang **+Aktivitätsprotokollwarnung hinzufügen**, um Benachrichtigungen zu empfangen und so sicherzustellen, dass Sie zukünftige Dienstbenachrichtigungen dieses Typs erhalten werden. Um mehr zum Konfigurieren von Warnungen zu Dienstbenachrichtigungen zu erfahren, [klicken Sie hier](monitoring-activity-log-alerts-on-service-notifications.md).

## <a name="next-steps"></a>Nächste Schritte:
Empfangen von [Warnungsbenachrichtigungen, wenn eine Dienstintegritätsbenachrichtigung](monitoring-activity-log-alerts-on-service-notifications.md) gesendet wird  
Erfahren Sie mehr über [Aktivitätsprotokollwarnungen](monitoring-activity-log-alerts.md).

