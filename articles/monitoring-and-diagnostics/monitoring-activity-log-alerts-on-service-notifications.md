---
title: "Empfangen von Aktivitätsprotokollwarnungen zu Dienstbenachrichtigungen | Microsoft-Dokumentation"
description: Lassen Sie sich per SMS, E-Mail oder Webhook benachrichtigen, wenn Ereignisse beim Azure-Dienst eintreten.
author: johnkemnetz
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
ms.author: johnkem
ms.translationtype: HT
ms.sourcegitcommit: cddb80997d29267db6873373e0a8609d54dd1576
ms.openlocfilehash: 202e7ec116e5e49beaad8c2d570a3659236e9b0f
ms.contentlocale: de-de
ms.lasthandoff: 07/18/2017

---
# <a name="create-activity-log-alerts-on-service-notifications"></a>Erstellen von Aktivitätsprotokollwarnungen zu Dienstbenachrichtigungen
## <a name="overview"></a>Übersicht
Dieser Artikel zeigt, wie Sie Aktivitätsprotokollwarnungen zur Dienstintegrität im Azure-Portal einrichten.  

Sie können eine Warnung erhalten, wenn Azure Benachrichtigungen zur Dienstintegrität an Ihr Azure-Abonnement sendet.  
Sie können die Warnung basierend auf Folgendem konfigurieren:
- Der Klasse der Dienstintegritätsbenachrichtigung (Vorfälle, Wartung, Informationen usw.)
- Die betroffenen Dienste
- Die betroffenen Regionen
- Dem Status der Benachrichtigung (aktiv bzw. gelöst)
- Der Benachrichtigungsstufe (Information, Warnung, Fehler)

Sie können auch konfigurieren, an wen die Warnung gesendet werden soll:
- Wählen Sie eine vorhandene Aktionsgruppe aus.
- Erstellen Sie eine neue Aktionsgruppe (die für zukünftige Warnungen verwendet werden kann).

[Hier](monitoring-action-groups.md) erfahren Sie mehr über Aktionsgruppen.

Informationen zum Konfigurieren von Dienstintegritätsbenachrichtigungen mithilfe von Azure Resource Manager-Vorlagen: [Resource Manager-Vorlagen](monitoring-create-activity-log-alerts-with-resource-manager-template.md)

## <a name="create-an-alert-on-a-service-health-notification-for-a-new-action-group-with-the-azure-portal"></a>Erstellen einer Warnung zu einer Dienstintegritätsbenachrichtigung für eine neue Aktionsgruppe mit dem Azure-Portal
1.  Navigieren Sie im [Portal](https://portal.azure.com) zum Dienst **Überwachen**.

    ![Überwachen](./media/monitoring-activity-log-alerts-on-service-notifications/home-monitor.png)

2.  Klicken Sie auf die Option **Überwachen**, um das Blatt „Überwachen“ zu öffnen. Zunächst wird der Abschnitt **Aktivitätsprotokoll** geöffnet.

3.  Klicken Sie nun auf den Abschnitt **Warnungen**.

    ![Warnungen](./media/monitoring-activity-log-alerts-on-service-notifications/alerts-blades.png)

4.  Wählen Sie den Befehl **Aktivitätsprotokollwarnung hinzufügen** aus, und füllen Sie die Felder aus.

    ![Hinzufügen einer Warnung](./media/monitoring-activity-log-alerts-on-service-notifications/add-activity-log-alert.png)

5.  Geben Sie Ihrer Aktivitätsprotokollwarnung einen **Namen**, und geben Sie eine **Beschreibung** an.

    ![Hinzufügen einer Warnung für eine neue Aktionsgruppe](./media/monitoring-activity-log-alerts-on-service-notifications/activity-log-alert-service-notification-new-action-group.png)

6.  Das **Abonnement** gibt den Speicherort für die Aktivitätsprotokollwarnung an. Es wird automatisch das Abonnement eingetragen, mit dem Sie gerade arbeiten. Dies ist das Abonnement, für das die Warnungsressource bereitgestellt wird, und das sie überwacht.

7.  Wählen Sie die **Ressourcengruppe** aus, die dieser Warnung im **Abonnement** zugeordnet wird.

8.  Wählen Sie unter **Ereigniskategorie** die Option „Dienstintegrität“ aus. Wählen Sie **Dienst, Region, Typ Status** und **Ebene** der Dienstintegritätsbenachrichtigungen aus, über die Sie benachrichtigt werden möchten.

9.  Erstellen Sie eine **Neue** Aktionsgruppe, indem Sie ihren **Namen** und **Kurznamen** angeben. Auf den Kurznamen wird in den Benachrichtigungen verwiesen, die beim Auslösen dieser Warnung gesendet werden.

10. Definieren Sie anschließend eine Liste von Empfängern, indem Sie folgende Daten der Empfänger angeben:

    a. **Name:** Name, Alias oder Bezeichner des Empfängers

    b. **Aktionstyp:** Wählen Sie als Art der Kontaktaufnahme mit dem Empfänger per SMS, E-Mail oder Webhook aus.

    c. **Details:** Geben Sie basierend auf dem ausgewählten Aktionstyp eine Telefonnummer, eine E-Mail-Adresse oder einen Webhook-URI an.

11. Wählen Sie **OK** aus, wenn das Erstellen der Warnung abgeschlossen ist.

Innerhalb weniger Minuten wird die Warnung aktiv und wie oben beschrieben ausgelöst.

Weitere Informationen zum Webhookschema für Aktivitätsprotokollwarnungen finden Sie [hier](monitoring-activity-log-alerts-webhook.md).

>[!NOTE]
>Die in den folgenden Schritten definierte Aktionsgruppe kann als vorhandene Aktionsgruppe für alle zukünftigen Warnungsdefinitionen wiederverwendet werden.
>
>

## <a name="create-an-alert-on-a-service-health-notification-using-an-existing-action-group-with-the-azure-portal"></a>Erstellen einer Warnung zu einer Dienstintegritätsbenachrichtigung mit einer vorhandenen Aktionsgruppe mit dem Azure-Portal
1.  Navigieren Sie im [Portal](https://portal.azure.com) zum Dienst **Überwachen**.

    ![Überwachen](./media/monitoring-activity-log-alerts-on-service-notifications/home-monitor.png)
2.  Klicken Sie auf die Option **Überwachen**, um das Blatt „Überwachen“ zu öffnen. Zunächst wird der Abschnitt **Aktivitätsprotokoll** geöffnet.

3.  Klicken Sie nun auf den Abschnitt **Warnungen**.

    ![Warnungen](./media/monitoring-activity-log-alerts-on-service-notifications/alerts-blades.png)
4.  Wählen Sie den Befehl **Aktivitätsprotokollwarnung hinzufügen** aus, und füllen Sie die Felder aus.

    ![Hinzufügen einer Warnung](./media/monitoring-activity-log-alerts-on-service-notifications/add-activity-log-alert.png)
5.  Geben Sie Ihrer Aktivitätsprotokollwarnung einen **Namen**, und wählen Sie eine **Beschreibung** aus.

    ![Hinzufügen einer Warnung für eine vorhandene Aktionsgruppe](./media/monitoring-activity-log-alerts-on-service-notifications/activity-log-alert-service-notification-existing-action-group.png)
6.  Das **Abonnement** gibt den Speicherort für die Aktivitätsprotokollwarnung an. Es wird automatisch das Abonnement eingetragen, mit dem Sie gerade arbeiten. Dies ist das Abonnement, für das die Warnungsressource bereitgestellt wird, und das sie überwacht.

7.  Wählen Sie die **Ressourcengruppe** aus, die dieser Warnung im **Abonnement** zugeordnet wird.

8.  Wählen Sie unter **Ereigniskategorie** die Option „Dienstintegrität“ aus. Wählen Sie **Dienst, Region, Typ Status** und **Ebene** der Dienstintegritätsbenachrichtigungen aus, über die Sie benachrichtigt werden möchten.

9.  Wählen Sie für **Benachrichtigen über** die Option **Vorhandene Aktionsgruppe** aus. Wählen Sie die entsprechende Aktionsgruppe aus.

10. Wählen Sie **OK** aus, wenn das Erstellen der Warnung abgeschlossen ist.

Innerhalb weniger Minuten wird die Warnung aktiv und wie oben beschrieben ausgelöst.

## <a name="managing-your-alerts"></a>Verwalten von Warnungen

Nachdem Sie eine Warnung erstellt haben, wird sie im Abschnitt „Warnungen“ des Diensts „Überwachen“ angezeigt. Wählen Sie die Warnung aus, die Sie verwalten möchten. Sie haben folgende Möglichkeiten:
* **Bearbeiten**
* **Löschen**
* **Deaktivieren** oder **Aktivieren** – wenn Sie den Empfang von Benachrichtigungen zu dieser Warnung vorübergehend beenden oder fortsetzen möchten.

## <a name="next-steps"></a>Nächste Schritte:
- Weitere Informationen zu [Dienstintegritätsbenachrichtigungen](monitoring-service-notifications.md)
- Erfahren Sie mehr über [Notification Rate Limiting (Ratenbegrenzung für Benachrichtigungen)](monitoring-alerts-rate-limiting.md).
- Weitere Informationen zum [Webhookschema für Aktivitätsprotokollwarnungen](monitoring-activity-log-alerts-webhook.md)
- Verschaffen Sie sich einen [Übersicht über Aktivitätsprotokollwarnungen](monitoring-overview-alerts.md), und erfahren Sie, wie Sie gewarnt werden können.  
- Weitere Informationen zu [Aktionsgruppen](monitoring-action-groups.md)

