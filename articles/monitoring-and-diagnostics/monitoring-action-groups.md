---
title: Erstellen und Verwalten von Aktionsgruppen im Azure-Portal | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Aktionsgruppen im Azure-Portal erstellen und verwalten.
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
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: ea15705bf02d9773507c6cb59f2da4c1dd0f9d77
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---
# <a name="create-and-manage-action-groups-in-the-azure-portal"></a>Erstellen und Verwalten von Aktionsgruppen im Azure-Portal
## <a name="overview"></a>Übersicht ##
In diesem Artikel wird beschrieben, wie Sie Aktionsgruppen im Azure-Portal erstellen und verwalten.

Sie können eine Liste von Aktionen mit Aktionsgruppen konfigurieren. Diese Gruppen können Sie dann verwenden, wenn Sie Aktivitätsprotokollwarnungen definieren. Diese Gruppen können Sie dann bei jeder Aktivitätsprotokollwarnung wiederverwenden, die Sie definieren, um sicherzustellen, dass jedes Mal, wenn die Aktivitätsprotokollwarnung ausgelöst wird, die gleichen Aktionen ausgeführt werden.

Eine Aktivitätsgruppe kann bis zu zehn Aktionen jedes Aktionstyps enthalten. Jede Aktion besteht aus den folgenden Eigenschaften:

* **Name:** ein eindeutiger Bezeichner innerhalb der Aktionsgruppe.  
* **Aktionstyp**: Senden einer SMS, einer E-Mail oder Aufrufen eines Webhooks.  
* **Details**: entsprechende Telefonnummer, E-Mail-Adresse oder entsprechender Webhook-URI.

Weitere Informationen zum Verwenden von Azure Resource Manager-Vorlagen zur Konfigurierung von Aktionsgruppen finden Sie unter [Aktionsgruppen-Resource Manager-Vorlagen](monitoring-create-action-group-with-resource-manager-template.md).

## <a name="create-an-action-group-by-using-the-azure-portal"></a>Erstellen einer Aktionsgruppe mit dem Azure-Portal ##
1. Wählen Sie im [Portal](https://portal.azure.com) die Option **Überwachen**. Das Blatt **Überwachen** konsolidiert alle Ihre Überwachungseinstellungen und Daten in einer Ansicht.

    ![Der Dienst „Überwachen“](./media/monitoring-action-groups/home-monitor.png)
2. Wählen Sie im Abschnitt **Aktivitätsprotokoll** die Option **Aktionsgruppen**.

    ![Registerkarte „Aktionsgruppen“](./media/monitoring-action-groups/action-groups-blade.png)
3. Wählen Sie **Aktionsgruppe hinzufügen**, und füllen Sie die Felder aus.

    ![Der Befehl „Aktionsgruppe hinzufügen“](./media/monitoring-action-groups/add-action-group.png)
4. Geben Sie jeweils einen Namen in die Felder **Aktionsgruppenname** und **Kurzname** ein. Der Kurzname wird anstelle eines vollständigen Aktionsgruppennamens verwendet, wenn Benachrichtigungen mithilfe dieser Gruppe gesendet werden.

      ![Das Dialogfeld „Aktionsgruppe hinzufügen“](./media/monitoring-action-groups/action-group-define.png)

5. In das Feld **Abonnement** wird automatisch Ihr aktuelles Abonnement eingetragen. In diesem Abonnement wird die Aktionsgruppe gespeichert.

6. Wählen Sie die **Ressourcengruppe**, in der die Aktionsgruppe gespeichert wird.

7. Definieren Sie eine Liste von Aktionen, indem Sie für jede Aktion Folgendes angeben:

    a. **Name**: Geben Sie einen eindeutigen Bezeichner für diese Aktion an.

    b. **Aktionstyp**: Wählen Sie SMS, E-Mail oder Webhook.

    c. **Details:** Geben Sie basierend auf dem Aktionstyp eine Telefonnummer, eine E-Mail-Adresse oder einen Webhook-URI an.

8. Wählen Sie **OK**, um die Aktionsgruppe zu erstellen.

## <a name="manage-your-action-groups"></a>Verwalten von Aktionsgruppen ##
Nachdem Sie eine Aktionsgruppe erstellt haben, wird diese im Abschnitt **Aktionsgruppen** des Diensts **Überwachen** angezeigt. Wählen Sie die Aktionsgruppe, die Sie verwalten möchten, um Folgendes zu tun:

* Fügen Sie Aktionen hinzu, bearbeiten oder entfernen Sie diese.
* Löschen der Aktionsgruppe.

## <a name="next-steps"></a>Nächste Schritte ##
* Weitere Informationen zum [Verhalten von SMS-Warnungen](monitoring-sms-alert-behavior.md).  
* Erweitern Sie Ihr [Verständnis des Webhookschemas für Aktivitätsprotokollwarnungen](monitoring-activity-log-alerts-webhook.md).  
* Weitere Informationen zu [Ratenlimits](monitoring-alerts-rate-limiting.md) für Warnungen. 
* Verschaffen Sie sich eine [Übersicht über Aktivitätsprotokollwarnungen](monitoring-overview-alerts.md), und erfahren Sie, wie Sie Warnungen empfangen können.  
* Erfahren Sie, wie Sie [Warnungen konfigurieren, wenn eine Dienstintegritätsbenachrichtigung gesendet wird](monitoring-activity-log-alerts-on-service-notifications.md).

