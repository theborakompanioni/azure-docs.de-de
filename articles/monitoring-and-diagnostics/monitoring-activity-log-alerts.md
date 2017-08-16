---
title: "Erstellen von Aktivitätsprotokollwarnungen | Microsoft-Dokumentation"
description: "Lassen Sie sich per SMS, Webhook und E-Mail benachrichtigen, wenn bestimmte Ereignisse im Aktivitätsprotokoll auftreten."
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
ms.date: 08/03/2017
ms.author: johnkem
ms.translationtype: HT
ms.sourcegitcommit: 8b857b4a629618d84f66da28d46f79c2b74171df
ms.openlocfilehash: c44ecc0ee61c58ab7590e9a21a5fe002068c6b83
ms.contentlocale: de-de
ms.lasthandoff: 08/04/2017

---
# <a name="create-activity-log-alerts"></a>Erstellen von Aktivitätsprotokollwarnungen

## <a name="overview"></a>Übersicht
Aktivitätsprotokollwarnungen sind Warnungen, die aktiviert werden, wenn ein neues Aktivitätsprotokollereignis auftritt, das die in der Warnung angegebenen Bedingungen erfüllt. Da es sich um Azure-Ressourcen handelt, können sie mit einer Resource Manager-Vorlage erstellt werden und im Azure-Portal erstellt, aktualisiert oder gelöscht werden. In diesem Artikel werden die Konzepte für Aktivitätsprotokollwarnungen beschrieben, und anschließend wird gezeigt, wie Sie das Azure-Portal zum Einrichten einer Warnung für Aktivitätsprotokollereignisse verwenden.

Aktivitätsprotokollwarnungen werden normalerweise aus einem der folgenden beiden Gründe erstellt:
1. Um eine Benachrichtigung zu erhalten, wenn es für Ressourcen in Ihrem Azure-Abonnement zu spezifischen Änderungen kommt, die häufig für bestimmte Ressourcengruppen oder Ressourcen gelten. Es kann beispielsweise sein, dass Sie benachrichtigt werden möchten, wenn ein beliebiger virtueller Computer in „myProductionResourceGroup“ gelöscht wird. Unter Umständen möchten Sie auch benachrichtigt werden, wenn einem Benutzer unter Ihrem Abonnement neue Rollen zugewiesen werden.
2. Um eine Benachrichtigung zu erhalten, wenn ein Service Health-Ereignis eintritt. Service Health-Ereignisse (also Ereignisse zur Dienstintegrität) umfassen eine Benachrichtigung bei Vorfällen und Wartungsereignissen, die für Ressourcen Ihres Abonnements gelten.

In beiden Fällen wird mit der Aktivitätsprotokollwarnung nur eine Überwachung auf Ereignisse in dem Abonnement durchgeführt, unter dem die Warnung erstellt wurde.

Sie können eine Aktivitätsprotokollwarnung basierend auf einer beliebigen Eigenschaft der obersten Ebene im JSON-Objekt für ein Aktivitätsprotokollereignis konfigurieren. Im Portal werden aber die am häufigsten verwendeten Optionen angezeigt:
- **Kategorie**: Verwaltung, Dienstintegrität, Autoskalierung oder Empfehlung. [Dieser Artikel](./monitoring-overview-activity-logs.md#categories-in-the-activity-log) enthält Informationen zu Aktivitätsprotokollkategorien, und [hier](./monitoring-activity-log-alerts-on-service-notifications.md) finden Sie weitere Informationen zu Dienstintegritätsereignissen.
- **Ressourcengruppe**
- **Ressource**
- **Ressourcentyp**
- **Vorgangsname**: Der Name des Resource Manager-RBAC-Vorgangs.
- **Level** (Grad): Der Schweregrad des Ereignisses (Ausführlich, Information, Warnung, Fehler, Kritisch).
- **Status**: Der Status des Ereignisses, z.B. „Gestartet“, „Fehler“ oder „Erfolgreich“.
- **Ereignis initiiert von**: Wird auch als „Aufrufer“ bezeichnet. Die E-Mail-Adresse oder der Active Directory-Bezeichner des Benutzers, der den Vorgang durchgeführt hat.

>[!NOTE]
>Sie müssen in Ihrer Warnung mindestens zwei der obigen Kriterien angeben, wobei eines die Kategorie sein muss. Eine Warnung, die jedes Mal aktiviert wird, wenn ein Ereignis in den Aktivitätsprotokollen erstellt wird, kann nicht erstellt werden.
>
>

Bei Aktivierung einer Aktivitätsprotokollwarnung wird eine Aktionsgruppe verwendet, um Aktionen oder Benachrichtigungen zu generieren. Eine Aktionsgruppe ist ein wiederverwendbarer Satz von Benachrichtigungsempfängern (E-Mail-Adressen, Webhook-URLs oder Telefonnummern für SMS), auf die von mehreren Warnungen verwiesen werden kann, um Ihre Benachrichtigungskanäle zu zentralisieren und zu gruppieren. Sie können entweder eine vorhandene Aktionsgruppe in Ihrer Aktivitätsprotokollwarnung verwenden oder eine neue Aktionsgruppe erstellen, während Sie Ihre Aktivitätsprotokollwarnung definieren. [Hier](monitoring-action-groups.md) erfahren Sie mehr über Aktionsgruppen.

Sie können [hier](monitoring-activity-log-alerts-on-service-notifications.md) mehr über Aktivitätsprotokollwarnungen für Dienstintegritätsbenachrichtigungen erfahren.

## <a name="create-an-alert-on-an-activity-log-event-with-a-new-action-group-with-the-azure-portal"></a>Erstellen einer Warnung zu einem Aktivitätsprotokollereignis mit einer neuen Aktionsgruppe im Azure-Portal
1.  Navigieren Sie im [Portal](https://portal.azure.com) zum Blatt **Überwachen**.

    ![Überwachen](./media/monitoring-activity-log-alerts/home-monitor.png)
2.  Klicken Sie auf die Option **Überwachen**, um das Blatt „Überwachen“ zu öffnen. Zunächst wird das Blatt **Aktivitätsprotokoll** geöffnet.

3.  Klicken Sie nun auf das Blatt **Warnungen**.

    ![Warnungen](./media/monitoring-activity-log-alerts/alerts-blades.png)
4.  Wählen Sie den Befehl **Aktivitätsprotokollwarnung hinzufügen**, und füllen Sie die Felder aus.

5.  Geben Sie Ihrer Aktivitätsprotokollwarnung einen **Namen**, und wählen Sie eine **Beschreibung**.

    ![Hinzufügen einer Warnung](./media/monitoring-activity-log-alerts/add-activity-log-alert.png)

6.  Die Aktivitätsprotokollwarnung wird im ausgewählten **Abonnement** gespeichert. Es ist automatisch das Abonnement angegeben, in dem Sie gerade arbeiten. Dies ist das Abonnement, unter dem die Warnungsressource bereitgestellt wurde und die Aktivitätsprotokollereignisse überwacht werden.

    ![Add-Alert-New-Action-Group](./media/monitoring-activity-log-alerts/activity-log-alert-new-action-group.png)

7.  Wählen Sie die **Ressourcengruppe** aus, in der diese Warnungsressource erstellt wird. Beachten Sie, dass dies nicht die Ressourcengruppe ist, die *von* der Warnung überwacht wird, sondern die Ressourcengruppe, in der *die Warnungsressource selbst bereitgestellt wird*.

8.  Wählen Sie optional eine **Ereigniskategorie** aus, mit der die angezeigten zusätzlichen Filter geändert werden. Für Verwaltungsereignisse umfasst dies Optionen zum Filtern nach den Werten für **Ressourcengruppe**, **Ressource**, **Ressourcentyp**, **Vorgangsname**, **Level** (Grad), **Status** und **Ereignis initiiert von**, um zu ermitteln, welche Ereignisse von dieser Warnung überwacht werden sollten.

>[!NOTE]
>Sie müssen mindestens eine der oben genannten Kriterien in der Warnung angeben. Eine Warnung, die jedes Mal aktiviert wird, wenn ein Ereignis in den Aktivitätsprotokollen erstellt wird, kann nicht erstellt werden.
>
>

9.  Erstellen Sie eine neue Aktionsgruppe (Option **Neu**), indem Sie ihren **Namen** und **Kurznamen** angeben. Auf den Kurznamen verweisen die Benachrichtigungen, die gesendet werden, wenn diese Warnung aktiviert ist.

10. Definieren Sie anschließend eine Liste von Aktionen, indem Sie folgende Daten der Aktion angeben:

    a. **Name:** Name, Alias oder Bezeichner der Aktion.

    b. **Aktionstyp:** Wählen Sie den Aktionstyp aus: SMS, E-Mails oder Webhook

    c. **Details:** Geben Sie basierend auf dem ausgewählten Aktionstyp eine Telefonnummer, eine E-Mail-Adresse oder einen Webhook-URI an.

11. Wählen Sie **OK** aus, wenn das Erstellen der Warnung abgeschlossen ist.

Es dauert einige Minuten, bis die Warnung vollständig weitergegeben wurde und dann aktiviert und ausgelöst wird, wenn neue Ereignisse die Kriterien der Warnung erfüllen.

[Dieses Dokument enthält ausführliche Informationen zum Webhookschema für Aktivitätsprotokollwarnungen](monitoring-activity-log-alerts-webhook.md).

>[!NOTE]
>Die in den folgenden Schritten definierte Aktionsgruppe kann als vorhandene Aktionsgruppe für alle zukünftigen Warnungsdefinitionen wiederverwendet werden.
>
>

## <a name="create-an-alert-on-an-activity-log-event-for-an-existing-action-group-with-the-azure-portal"></a>Erstellen einer Warnung zu einem Aktivitätsprotokollereignis für eine vorhandene Aktionsgruppe mit dem Azure-Portal
1.  Navigieren Sie im [Portal](https://portal.azure.com) zum Blatt **Überwachen**.

    ![Überwachen](./media/monitoring-activity-log-alerts/home-monitor.png)
2.  Klicken Sie auf die Option **Überwachen**, um das Blatt „Überwachen“ zu öffnen. Zunächst wird der Abschnitt **Aktivitätsprotokoll** geöffnet.

3.  Klicken Sie nun auf den Abschnitt **Warnungen**.

    ![Warnungen](./media/monitoring-activity-log-alerts/alerts-blades.png)
4.  Wählen Sie den Befehl **Aktivitätsprotokollwarnung hinzufügen**, und füllen Sie die Felder aus.

5.  Geben Sie Ihrer Aktivitätsprotokollwarnung einen **Namen**, und wählen Sie eine **Beschreibung**. Diese werden in den Benachrichtigungen angezeigt, die gesendet werden, wenn diese Warnung ausgelöst wird.

    ![Hinzufügen einer Warnung](./media/monitoring-activity-log-alerts/add-activity-log-alert.png)
6.  Die Aktivitätsprotokollwarnung wird im ausgewählten **Abonnement** gespeichert. Es ist automatisch das Abonnement angegeben, in dem Sie gerade arbeiten. Dies ist das Abonnement, unter dem die Warnungsressource bereitgestellt wurde und die Aktivitätsprotokollereignisse überwacht werden.

    ![Hinzufügen einer Warnung für eine vorhandene Aktionsgruppe](./media/monitoring-activity-log-alerts/activity-log-alert-existing-action-group.png)
7.  Wählen Sie die **Ressourcengruppe** aus, in der diese Warnungsressource erstellt wird. Beachten Sie, dass dies nicht die Ressourcengruppe ist, die *von* der Warnung überwacht wird, sondern die Ressourcengruppe, in der *die Warnungsressource selbst bereitgestellt wird*.

8.  Wählen Sie optional eine **Ereigniskategorie** aus, mit der die angezeigten zusätzlichen Filter geändert werden. Für Verwaltungsereignisse umfasst dies Optionen zum Filtern nach den Werten für **Ressourcengruppe**, **Ressource**, **Ressourcentyp**, **Vorgangsname**, **Level** (Grad), **Status** und **Ereignis initiiert von**, um zu ermitteln, welche Ereignisse von dieser Warnung überwacht werden sollten.

9.  Wählen Sie für **Benachrichtigen über** eine **Vorhandene Aktionsgruppe**. Wählen Sie in der Liste eine vorhandene Aktionsgruppe aus.

10. Wählen Sie **OK** aus, wenn das Erstellen der Warnung abgeschlossen ist.

Es dauert einige Minuten, bis die Warnung vollständig weitergegeben wurde und dann aktiviert und ausgelöst wird, wenn neue Ereignisse die Kriterien der Warnung erfüllen.

## <a name="managing-your-alerts"></a>Verwalten von Warnungen

Nachdem Sie eine Warnung erstellt haben, wird sie auf dem Blatt „Überwachen“ im Abschnitt „Warnungen“ angezeigt. Wählen Sie die Warnung aus, die Sie verwalten möchten. Sie haben folgende Möglichkeiten:
* **Bearbeiten**
* **Löschen**
* **Deaktivieren** oder **Aktivieren** – wenn Sie den Empfang von Benachrichtigungen zu dieser Warnung vorübergehend beenden oder fortsetzen möchten.

## <a name="next-steps"></a>Nächste Schritte
- Lesen Sie eine [Übersicht über Warnungen](monitoring-overview-alerts.md).
- Erfahren Sie mehr über [Notification Rate Limiting (Ratenbegrenzung für Benachrichtigungen)](monitoring-alerts-rate-limiting.md).
- Weitere Informationen zum [Webhookschema für Aktivitätsprotokollwarnungen](monitoring-activity-log-alerts-webhook.md)
- Weitere Informationen zu [Aktionsgruppen](monitoring-action-groups.md)  
- Weitere Informationen zu [Dienstintegritätsbenachrichtigungen](monitoring-service-notifications.md)
- [Erstellen Sie eine Aktivitätsprotokollwarnung, um alle automatischen Modulskalierungsvorgänge für Ihr Abonnement zu überwachen.](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert)
- [Erstellen Sie eine Aktivitätsprotokollwarnung, um alle Autoskalierungs-Vorgänge zum horizontalen Herunterskalieren und horizontalen Hochskalieren in Ihrem Abonnement, bei denen Fehler aufgetreten sind, zu überwachen.](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert)

