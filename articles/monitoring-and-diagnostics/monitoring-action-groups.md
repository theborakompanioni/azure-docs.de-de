---
title: Erstellen und Verwalten von Aktionsgruppen im Azure-Portal | Microsoft-Dokumentation
description: 
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
ms.date: 05/15/2017
ms.author: ancav
ms.translationtype: HT
ms.sourcegitcommit: cddb80997d29267db6873373e0a8609d54dd1576
ms.openlocfilehash: 2b2fa2126b9c3f8598ec8fe686846920a4b7c422
ms.contentlocale: de-de
ms.lasthandoff: 07/18/2017

---
# <a name="create-and-manage-action-groups-in-azure-portal"></a>Erstellen und Verwalten von Aktionsgruppen im Azure-Portal
## <a name="overview"></a>Übersicht ##
In diesem Artikel wird beschrieben, wie Sie Aktionsgruppen im Azure-Portal erstellen und verwalten.

Mit Aktionsgruppen können Sie eine Aktionsliste konfigurieren. Diese Gruppen können beim Definieren von Aktivitätsprotokollwarnungen genutzt werden, um sicherzustellen, dass eine bestimmte Aktivitätsgruppe aufgerufen wird, wenn die Aktivitätsprotokollwarnung ausgelöst wird.

Eine Aktivitätsgruppe kann bis zu zehn Aktionen jedes Aktionstyps enthalten. Eine Aktion wird durch die Kombination von folgenden Informationen definiert:

**Name:** ein eindeutiger Bezeichner innerhalb der Aktionsgruppe  
**Aktionstyp:** definiert die ausgeführte Aktion. Optionen sind das Senden einer SMS oder E-Mail oder das Aufrufen eines Webhooks.  
**Details:** Geben Sie basierend auf dem Aktionstyp eine Telefonnummer, eine E-Mail-Adresse oder einen Webhook-URI an.

Weitere Informationen zum Verwenden von Azure Resource Manager-Vorlagen zur Konfigurierung von Aktionsgruppen: [Aktionsgruppen-Resource Manager-Vorlagen](monitoring-create-action-group-with-resource-manager-template.md)

## <a name="creating-an-action-group-using-the-azure-portal"></a>Erstellen einer Aktionsgruppe mit dem Azure-Portal ##
1.  Navigieren Sie im [Portal](https://portal.azure.com) zum Dienst **Überwachen**.

    ![Überwachen](./media/monitoring-action-groups/home-monitor.png)
2.  Klicken Sie auf die Option **Überwachen**, um das Blatt „Überwachen“ zu öffnen. Auf diesem Blatt sind alle Überwachungseinstellungen und -daten in einer konsolidierten Ansicht zusammengefasst. Zunächst wird der Abschnitt **Aktivitätsprotokoll** geöffnet.

3.  Klicken Sie jetzt auf den Abschnitt **Aktionsgruppen**.

    ![Aktionsgruppe](./media/monitoring-action-groups/action-groups-blade.png)
4.  Klicken Sie auf den Befehl zum **Hinzufügen** einer Aktionsgruppe, und füllen Sie die Felder aus.

    ![Hinzufügen einer Aktionsgruppe](./media/monitoring-action-groups/add-action-group.png)
5.  Geben Sie einen **Namen** und **Kurznamen** für die Aktionsgruppe ein. In Benachrichtigungen an diese Gruppe wird auf den Kurznamen verwiesen.

      ![Definieren von Aktionsgruppen](./media/monitoring-action-groups/action-group-define.png)

6.  Das **Abonnement** gibt den Speicherort für die Aktionsgruppe an. Es wird automatisch das Abonnement eingetragen, mit dem Sie gerade arbeiten.

7.  Wählen Sie die **Ressourcengruppe** aus, die dieser Warnung im **Abonnement** zugeordnet wird.

8.  Definieren Sie anschließend eine Liste von Aktionen über eine Kombination aus:
  1. **Name:** ein eindeutiger Bezeichner innerhalb der Aktionsgruppe
  2. **Aktionstyp:** definiert die ausgeführte Aktion. Optionen sind das Senden einer SMS oder E-Mail oder das Aufrufen eines Webhooks.
  3. **Details:** Geben Sie basierend auf dem Aktionstyp eine Telefonnummer, eine E-Mail-Adresse oder einen Webhook-URI an.

9.  Wählen Sie **OK** aus, wenn Sie mit dem Erstellen der Warnung fertig sind.

## <a name="managing-your-action-groups"></a>Verwalten von Aktionsgruppen ##
Nachdem Sie eine Aktionsgruppe erstellt haben, wird diese im Abschnitt „Aktionsgruppen“ des Diensts „Überwachen“ angezeigt. Wählen Sie die Aktionsgruppe aus, die Sie verwalten möchten. Sie haben folgende Möglichkeiten:
* Fügen Sie Aktionen hinzu, bearbeiten oder entfernen Sie diese.
-   Löschen der Aktionsgruppe.

## <a name="next-steps"></a>Nächste Schritte: ##
Weitere Informationen zum [Verhalten von SMS-Warnungen](monitoring-sms-alert-behavior.md)  
Weitere Informationen zum [Verständnis des Webhookschemas für Aktivitätsprotokollwarnungen](monitoring-activity-log-alerts-webhook.md)  
Weitere Informationen zu [Rateneinschränkungen](monitoring-alerts-rate-limiting.md) für Warnungen  
Verschaffen Sie sich einen [Übersicht über Aktivitätsprotokollwarnungen](monitoring-overview-alerts.md), und erfahren Sie, wie Sie gewarnt werden können.  
[Konfigurieren von Warnungen, wenn eine Dienstintegritätsbenachrichtigung gesendet wird](monitoring-activity-log-alerts-on-service-notifications.md)

