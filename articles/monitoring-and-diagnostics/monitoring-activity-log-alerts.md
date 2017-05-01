---
title: "Erstellen von Aktivitätsprotokollwarnungen | Microsoft-Dokumentation"
description: "Lassen Sie sich per SMS, Webhook und E-Mail benachrichtigen, wenn bestimmte Ereignisse im Aktivitätsprotokoll auftreten."
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
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: cbdb1f866c51f2a2622d14e1987660e34cb1b5ff
ms.lasthandoff: 04/03/2017


---
# <a name="create-activity-log-alerts"></a>Erstellen von Aktivitätsprotokollwarnungen

## <a name="overview"></a>Übersicht
Aktivitätsprotokollwarnungen sind Azure-Ressourcen, sodass sie im Azure-Portal oder mit dem Azure Resource Manager (ARM) verwaltet werden können. In diesem Artikel erfahren Sie, wie Sie im Azure-Portal Warnungen zu Aktivitätsprotokollereignissen einrichten können.

Sie können Warnungen zu Vorgängen erhalten, die auf Ressourcen in Ihrem Abonnement ausgeführt wurden, oder über Dienstintegritätsereignisse, die sich auf die Integrität Ihrer Ressourcen auswirken können. Eine Aktivitätsprotokollwarnung überwacht die Aktivitätsprotokollereignisse für das bestimmte Abonnement, für das die Warnung bereitgestellt wird.

Sie können die Warnung konfigurieren auf der Grundlage von:
* Ereigniskategorie (für [Dienstintegritätsereignisse hier klicken](monitoring-activity-log-alerts-on-service-notifications.md))
- Ressourcengruppe
- Ressource
- Ressourcentyp
- Vorgangsname
- Benachrichtigungsstufe (Ausführlich, Information, Warnung, Fehler, Kritisch)
- Status
- Ereignis initiiert von

Sie können auch konfigurieren, an wen die Warnung gesendet werden soll:
* Wählen Sie eine vorhandene Aktionsgruppe aus
- Erstellen Sie eine neue Aktionsgruppe (die für zukünftige Warnungen verwendet werden kann)

[Hier](monitoring-action-groups.md) erfahren Sie mehr über Aktionsgruppen.

Informationen zu Dienstintegritäts-Benachrichtigungswarnungen können Sie konfigurieren und abrufen unter Verwendung von
* [Zugriffsrichtlinien](monitoring-activity-log-alerts.md)
- [Resource Manager-Vorlagen](monitoring-create-activity-log-alerts-with-resource-manager-template.md)

## <a name="create-an-alert-on-an-activity-log-event-with-a-new-action-group-with-the-azure-portal"></a>Erstellen einer Warnung zu einem Aktivitätsprotokollereignis mit einer neuen Aktionsgruppe mit dem Azure-Portal
1.    Navigieren Sie im [Portal](https://portal.azure.com) zum Dienst **Überwachen**.

    ![Überwachen](./media/monitoring-activity-log-alerts/home-monitor.png)
2.    Klicken Sie auf die Option **Überwachen**, um das Blatt „Überwachen“ zu öffnen. Zunächst wird der Abschnitt **Aktivitätsprotokoll** geöffnet.

3.    Klicken Sie nun auf den Abschnitt **Warnungen**.

    ![Warnungen](./media/monitoring-activity-log-alerts/alerts-blades.png)
4.    Wählen Sie den Befehl **Aktivitätsprotokollwarnung hinzufügen** aus, und füllen Sie die Felder aus.

5.    Geben Sie Ihrer Aktivitätsprotokollwarnung einen **Namen**, und wählen Sie eine **Beschreibung**. Diese werden angezeigt in den Benachrichtigungen angezeigt, die gesendet werden, wenn diese Warnung ausgelöst wird.

    ![Add-Alert](./media/monitoring-activity-log-alerts/add-activity-log-alert.png)

6.    Für **Abonnement** sollte automatisch das Abonnement eingetragen werden, mit dem Sie gerade arbeiten. Dies ist das Abonnement, für das die Warnungsressource bereitgestellt wird, und das sie überwacht.

    ![Add-Alert-New-Action-Group](./media/monitoring-activity-log-alerts/activity-log-alert-new-action-group.png)

7.    Wählen Sie die **Ressourcengruppe** aus, die dieser Warnung im **Abonnement** zugeordnet wird.

8.    Geben Sie die Werte für **Ereigniskategorie**, **Ressourcengruppe**, **Ressource**, **Ressourcentyp**, **Vorgangsname**, **Ebene**, **Status** und **Ereignis initiiert von** an, um zu bestimmen, welche Ereignisse diese Warnung überwachen muss.

9.    Erstellen Sie eine **Neue** Aktionsgruppe, indem Sie ihren **Namen** und **Kurznamen** angeben; auf den Kurznamen verweisen die Benachrichtigungen, die gesendet werden, wenn diese Warnung aktiviert ist.

10.    Definieren Sie anschließend eine Liste von Empfängern, indem Sie folgende Daten der Empfänger angeben:

    a. **Name:** Name, Alias oder Bezeichner des Empfängers.

    b. **Aktionstyp:** Wählen Sie die Kontaktaufnahme mit dem Empfänger per SMS, E-Mail oder Webhook.

    c. **Details:** Geben Sie basierend auf dem ausgewählten Aktionstyp eine Telefonnummer, E-Mail-Adresse oder einen Webhook-URI an.

11.    Wählen Sie **OK** aus, wenn das Erstellen der Warnung abgeschlossen ist.

Innerhalb weniger Minuten wird die Warnung aktiv und wie oben beschrieben ausgelöst.

Um weitere Informationen über das Webhookschema für Aktivitätsprotokollwarnungen zu erhalten, [klicken Sie hier](monitoring-activity-log-alerts-webhook.md).

>[!NOTE]
>Die in den folgenden Schritten definierte Aktionsgruppe kann als vorhandene Aktionsgruppe für alle zukünftigen Warnungsdefinitionen wiederverwendet werden.
>
>

## <a name="create-an-alert-on-an-activity-log-event-for-an-existing-action-group-with-the-azure-portal"></a>Erstellen einer Warnung zu einem Aktivitätsprotokollereignis für eine vorhandene Aktionsgruppe mit dem Azure-Portal
1.    Navigieren Sie im [Portal](https://portal.azure.com) zum Dienst **Überwachen**.

    ![Überwachen](./media/monitoring-activity-log-alerts/home-monitor.png)
2.    Klicken Sie auf die Option **Überwachen**, um das Blatt „Überwachen“ zu öffnen. Zunächst wird der Abschnitt **Aktivitätsprotokoll** geöffnet.

3.    Klicken Sie nun auf den Abschnitt **Warnungen**.

    ![Warnungen](./media/monitoring-activity-log-alerts/alerts-blades.png)
4.    Wählen Sie den Befehl **Aktivitätsprotokollwarnung hinzufügen** aus, und füllen Sie die Felder aus.

5.    Geben Sie Ihrer Aktivitätsprotokollwarnung einen **Namen**, und wählen Sie eine **Beschreibung**. Diese werden angezeigt in den Benachrichtigungen angezeigt, die gesendet werden, wenn diese Warnung ausgelöst wird.

    ![Add-Alert](./media/monitoring-activity-log-alerts/add-activity-log-alert.png)
6.    Für **Abonnement** sollte automatisch das Abonnement eingetragen werden, mit dem Sie gerade arbeiten.

    ![Add-Alert-Existing-Action-Group](./media/monitoring-activity-log-alerts/activity-log-alert-existing-action-group.png)
7.    Wählen Sie die **Ressourcengruppe** für diese Warnung.

8.    Geben Sie die Werte für **Ereigniskategorie**, **Ressourcengruppe**, **Ressource**, **Ressourcentyp**, **Vorgangsname**, **Ebene**, **Status** und **Ereignis initiiert von** an, um zu bestimmen, für welche Ereignisse diese Warnung gelten soll.

9.    Wählen Sie für **Benachrichtigen über** eine **Vorhandene Aktionsgruppe**. Wählen Sie die entsprechende Aktionsgruppe aus.

10.    Wählen Sie **OK** aus, wenn das Erstellen der Warnung abgeschlossen ist.

Innerhalb weniger Minuten wird die Warnung aktiv und wie oben beschrieben ausgelöst.

## <a name="managing-your-alerts"></a>Verwalten von Warnungen

Nachdem Sie eine Warnung erstellt haben, ist sie im Abschnitt „Warnungen“ des Diensts „Überwachen“ sichtbar. Wählen Sie die Warnung aus, die Sie verwalten möchten. Sie haben folgende Möglichkeiten:
* **Bearbeiten** Sie sie.
* **Löschen** Sie sie.
* **Deaktivieren** oder **Aktivieren** Sie sie, wenn Sie den Empfang von Benachrichtigungen zu dieser Warnung vorübergehend beenden oder fortsetzen möchten.

## <a name="next-steps"></a>Nächste Schritte:
Lesen Sie eine [Übersicht über Warnungen](monitoring-overview-alerts.md).  
Machen Sie sich mit dem [Webhookschema zu Aktivitätsprotokollwarnungen](monitoring-activity-log-alerts-webhook.md) vertraut. Erfahren Sie mehr über [Aktionsgruppen](monitoring-action-groups.md).  
Erfahren Sie mehr über [Dienstintegritätsbenachrichtigungen](monitoring-service-notifications.md).

