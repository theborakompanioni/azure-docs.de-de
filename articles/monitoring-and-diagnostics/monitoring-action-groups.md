---
title: Erstellen und Verwalten von Aktionsgruppen im Azure-Portal | Microsoft-Dokumentation
description: 
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
ms.openlocfilehash: 9a290422bf897397942fccf2e3733d0709701e91
ms.lasthandoff: 03/31/2017


---
# <a name="create-and-manage-action-groups-in-azure-portal"></a>Erstellen und Verwalten von Aktionsgruppen im Azure-Portal
## <a name="overview"></a>Übersicht ##
In diesem Artikel wird beschrieben, wie Sie Aktionsgruppen im Azure-Portal erstellen und verwalten.

Mit Aktionsgruppen können Sie eine Empfängerliste konfigurieren. Diese Gruppen können beim Definieren von Aktivitätsprotokollwarnungen genutzt werden, um sicherzustellen, dass eine bestimmte Aktivitätsgruppe benachrichtigt wird, wenn die Aktivitätsprotokollwarnung ausgelöst wird.

Eine Aktivitätsgruppe kann bis zu zehn Aktionen jedes Aktionstyps enthalten. Eine Aktion wird durch die Kombination von folgenden Informationen definiert:

**Name:** ein eindeutiger Bezeichner innerhalb der Aktionsgruppe  
**Aktionstyp:** definiert die ausgeführte Aktion. Optionen sind das Senden einer SMS oder E-Mail oder das Aufrufen eines Webhooks.  
**Details:** Geben Sie basierend auf dem Aktionstyp eine Telefonnummer, eine E-Mail-Adresse oder einen Webhook-URI an.

Für das Konfigurieren und Abrufen von Informationen zu Dienstintegritäts-Benachrichtigungswarnungen können Sie Folgendes verwenden:
* [Azure-Portal](monitoring-action-groups.md)
- [Resource Manager-Vorlagen](monitoring-create-action-group-with-resource-manager-template.md)

## <a name="creating-an-action-group-for-the-azure-portal"></a>Erstellen einer Aktionsgruppe für das Azure-Portal ##
1.    Navigieren Sie im [Portal](https://portal.azure.com) zum Dienst **Überwachen**.

    ![Überwachen](./media/monitoring-action-groups/home-monitor.png)
2.    Klicken Sie auf die Option **Überwachen**, um das Blatt „Überwachen“ zu öffnen. Auf diesem Blatt sind alle Überwachungseinstellungen und -daten in einer konsolidierten Ansicht zusammengefasst. Zunächst wird der Abschnitt **Aktivitätsprotokoll** geöffnet.

3.    Klicken Sie jetzt auf den Abschnitt **Aktionsgruppen**.

    ![Aktionsgruppe](./media/monitoring-action-groups/action-groups-blade.png)
4.    Klicken Sie auf den Befehl zum **Hinzufügen** einer Aktionsgruppe, und füllen Sie die Felder aus.

    ![Hinzufügen einer Aktionsgruppe](./media/monitoring-action-groups/add-action-group.png)
5.    Geben Sie einen **Namen** und **Kurznamen** für die Aktionsgruppe ein. In Benachrichtigungen an diese Gruppe wird auf den Kurznamen verwiesen.

      ![Definieren von Aktionsgruppen](./media/monitoring-action-groups/action-group-define.png)

6.    Das **Abonnement** gibt den Speicherort für die Aktionsgruppe an. Es wird automatisch das Abonnement eingetragen, mit dem Sie gerade arbeiten.

7.    Wählen Sie die **Ressourcengruppe** für diese Aktionsgruppe aus.

8.    Definieren Sie anschließend eine Liste von Aktionen über eine Kombination aus:
  1. **Name:** ein eindeutiger Bezeichner innerhalb der Aktionsgruppe
  2. **Aktionstyp:** definiert die ausgeführte Aktion. Optionen sind das Senden einer SMS oder E-Mail oder das Aufrufen eines Webhooks.
  3. **Details:** Geben Sie basierend auf dem Aktionstyp eine Telefonnummer, eine E-Mail-Adresse oder einen Webhook-URI an.

9.    Wählen Sie **OK** aus, wenn Sie mit dem Erstellen der Warnung fertig sind.

## <a name="managing-your-action-groups"></a>Verwalten von Aktionsgruppen ##
Nachdem Sie eine Aktionsgruppe erstellt haben, wird diese im Abschnitt „Aktionsgruppen“ des Diensts „Überwachen“ angezeigt. Wählen Sie die Aktionsgruppe aus, die Sie verwalten möchten. Sie haben folgende Möglichkeiten:
* Hinzufügen, Bearbeiten oder Entfernen von Empfängern.
-    Löschen der Aktionsgruppe.

## <a name="next-steps"></a>Nächste Schritte: ##
Weitere Informationen zum [Verhalten von SMS-Warnungen](monitoring-sms-alert-behavior.md)  
Weitere Informationen zum [Verständnis des Webhookschemas für Aktivitätsprotokollwarnungen](monitoring-activity-log-alerts-webhook.md)  
Weitere Informationen zu [Rateneinschränkungen](monitoring-alerts-rate-limiting.md) für Warnungen  
Verschaffen Sie sich einen [Übersicht über Aktivitätsprotokollwarnungen](monitoring-overview-alerts.md), und erfahren Sie, wie Sie gewarnt werden können.  
[Konfigurieren von Warnungen, wenn eine Dienstintegritätsbenachrichtigung gesendet wird](monitoring-activity-log-alerts-on-service-notifications.md)

