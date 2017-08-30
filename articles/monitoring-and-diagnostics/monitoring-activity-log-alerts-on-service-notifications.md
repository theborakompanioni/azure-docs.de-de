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
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: bf6a98fd7e7e11764bef174f9efd0635fa7efe9a
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---
# <a name="create-activity-log-alerts-on-service-notifications"></a>Erstellen von Aktivitätsprotokollwarnungen zu Dienstbenachrichtigungen
## <a name="overview"></a>Übersicht
Dieser Artikel zeigt, wie Sie Aktivitätsprotokollwarnungen für Dienstintegritätsbenachrichtigungen im Azure-Portal einrichten.  

Sie können eine Warnung erhalten, wenn Azure Benachrichtigungen zur Dienstintegrität an Ihr Azure-Abonnement sendet. Sie können die Warnung konfigurieren auf der Grundlage von:

- Der Klasse der Dienstintegritätsbenachrichtigung (Vorfälle, Wartung, Informationen usw.).
- Den betroffenen Diensten.
- Den betroffenen Regionen.
- Dem Status der Benachrichtigung (aktiv bzw. aufgelöst).
- Der Benachrichtigungsstufe (Information, Warnung, Fehler).

Sie können auch konfigurieren, an wen die Warnung gesendet werden soll:

- Wählen Sie eine vorhandene Aktionsgruppe aus.
- Erstellen Sie eine neue Aktionsgruppe (die für zukünftige Warnungen verwendet werden kann).

Weitere Informationen zu Aktionsgruppen finden Sie unter [Erstellen und Verwalten von Aktionsgruppen im Azure-Portal](monitoring-action-groups.md).

Informationen zum Konfigurieren von Dienstintegritätsbenachrichtigungs-Warnungen mithilfe von Azure Resource Manager-Vorlagen siehe [Resource Manager-Vorlagen](monitoring-create-activity-log-alerts-with-resource-manager-template.md).

## <a name="create-an-alert-on-a-service-health-notification-for-a-new-action-group-by-using-the-azure-portal"></a>Erstellen einer Warnung zu einer Dienstintegritätsbenachrichtigung für eine neue Aktionsgruppe mit dem Azure-Portal
1. Wählen Sie im [Portal](https://portal.azure.com) die Option **Überwachen**.

    ![Der Dienst „Überwachen“](./media/monitoring-activity-log-alerts-on-service-notifications/home-monitor.png)

2. Wählen Sie im Abschnitt **Aktivitätsprotokoll** die Option **Warnungen**.

    ![Die Registerkarte „Warnungen“](./media/monitoring-activity-log-alerts-on-service-notifications/alerts-blades.png)

3. Wählen Sie **Aktivitätsprotokollwarnung hinzufügen**, und füllen Sie die Felder aus.

    ![Der Befehl „Aktivitätsprotokollwarnung hinzufügen“](./media/monitoring-activity-log-alerts-on-service-notifications/add-activity-log-alert.png)

4. Geben Sie einen Namen in das Feld **Name der Aktivitätsprotokollwarnung** ein, und geben Sie eine **Beschreibung** ein.

    ![Das Dialogfeld „Aktivitätsprotokollwarnung hinzufügen“](./media/monitoring-activity-log-alerts-on-service-notifications/activity-log-alert-service-notification-new-action-group.png)

5. In das Feld **Abonnement** wird automatisch Ihr aktuelles Abonnement eingetragen. Dieses Abonnement wird verwendet, um die Aktivitätsprotokollwarnung zu speichern. Die Warnungsressource wird für dieses Abonnement bereitgestellt und überwacht hierfür Ereignisse im Aktivitätsprotokoll.

6. Wählen Sie die **Ressourcengruppe** aus, in der die Warnungsressource erstellt wird. Dies ist nicht die Ressourcengruppe, die von der Warnung überwacht wird. Stattdessen ist es die Ressourcengruppe, in der sich die Warnungsressource befindet.

7. Wählen Sie im Feld **Ereigniskategorie** die Option **Dienstintegrität**. Wählen Sie optional **Dienst**, **Region**, **Typ**, **Status** und **Ebene** der Dienstintegritätsbenachrichtigungen aus, die Sie empfangen möchten.

8. Wählen Sie unter **Warnung über** die Aktionsgruppenschaltfläche **Neu**. Geben Sie jeweils einen Namen in die Felder **Aktionsgruppenname** und **Kurzname** ein. Auf den kurzen Namen wird in den Benachrichtigungen verwiesen, die gesendet werden, wenn diese Warnung ausgelöst wird.

9. Definieren Sie eine Liste von Empfängern, indem Sie folgende Daten der Empfänger angeben:

    a. **Name:** Geben Sie Name, Alias oder Bezeichner des Empfängers ein.

    b. **Aktionstyp**: Wählen Sie SMS, E-Mail oder Webhook.

    c. **Details:** Geben Sie basierend auf dem ausgewählten Aktionstyp eine Telefonnummer, eine E-Mail-Adresse oder einen Webhook-URI ein.

10. Wählen Sie **OK** , um die Warnung zu erstellen.

Innerhalb weniger Minuten wird die Warnung aktiv und entsprechend den Bedingungen ausgelöst, die Sie während der Erstellung angegeben haben.

Informationen über das Webhookschema für Aktivitätsprotokollwarnungen finden Sie unter [Webhook für Azure-Aktivitätsprotokollwarnungen](monitoring-activity-log-alerts-webhook.md).

>[!NOTE]
>Die in den folgenden Schritten definierte Aktionsgruppe kann als vorhandene Aktionsgruppe für alle zukünftigen Warnungsdefinitionen wiederverwendet werden.
>
>

## <a name="create-an-alert-on-a-service-health-notification-for-an-existing-action-group-by-using-the-azure-portal"></a>Erstellen einer Warnung zu einer Dienstintegritätsbenachrichtigung für eine vorhandene Aktionsgruppe mit dem Azure-Portal

1. Führen Sie die Schritte 1 bis 7 im vorherigen Abschnitt aus, um Ihre Dienstintegritätsbenachrichtigung zu erstellen. 

2. Wählen Sie unter **Warnung über** die Aktionsgruppenschaltfläche **Vorhandene**. Wählen Sie die entsprechende Aktionsgruppe aus.

3. Wählen Sie **OK** , um die Warnung zu erstellen.

Innerhalb weniger Minuten wird die Warnung aktiv und entsprechend den Bedingungen ausgelöst, die Sie während der Erstellung angegeben haben.

## <a name="manage-your-alerts"></a>Verwalten Ihrer Warnungen

Nachdem Sie eine Warnung erstellt haben, wird sie auf dem Blatt **Überwachen** im Abschnitt **Warnungen** angezeigt. Wählen Sie die Warnung, die Sie verwalten möchten, um Folgendes zu tun:

* Sie bearbeiten.
* Sie löschen.
* Deaktivieren oder aktivieren Sie sie, wenn Sie den Empfang von Benachrichtigungen zu dieser Warnung vorübergehend beenden oder fortsetzen möchten.

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zu [Dienstintegritätsbenachrichtigungen](monitoring-service-notifications.md).
- Erfahren Sie mehr über [Ratenlimits für SMS, E-Mail-Nachrichten und Webhooks](monitoring-alerts-rate-limiting.md).
- Weitere Informationen zum [Webhookschema für Aktivitätsprotokollwarnungen](monitoring-activity-log-alerts-webhook.md).
- Verschaffen Sie sich eine [Übersicht über Aktivitätsprotokollwarnungen](monitoring-overview-alerts.md), und erfahren Sie, wie Sie Warnungen empfangen können. 
- Weitere Informationen zu [Aktionsgruppen](monitoring-action-groups.md).

