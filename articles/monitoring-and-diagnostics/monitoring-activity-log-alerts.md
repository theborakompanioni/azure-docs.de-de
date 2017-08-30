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
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: 3885469ec0e1fcc31386dd0ad7fe6cb5d03ab28e
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---
# <a name="create-activity-log-alerts"></a>Erstellen von Aktivitätsprotokollwarnungen

## <a name="overview"></a>Übersicht
Aktivitätsprotokollwarnungen sind Warnungen, die aktiviert werden, wenn ein neues Aktivitätsprotokollereignis auftritt, das die in der Warnung angegebenen Bedingungen erfüllt. Als Azure-Ressourcen können sie mit einer Azure Resource Manager-Vorlage erstellt werden. Sie können auch im Azure-Portal erstellt, aktualisiert oder gelöscht werden. In diesem Artikel werden die Konzepte erläutert, auf denen Aktivitätsprotokollwarnungen basieren. Dann erfahren Sie, wie Sie im Azure-Portal Warnungen zu Aktivitätsprotokollereignissen einrichten können.

Normalerweise erstellen Sie Aktivitätsprotokollwarnungen, um Benachrichtigungen zu empfangen, wenn Folgendes geschieht:

* Spezifische Änderungen treten bei Ressourcen in Ihrem Azure-Abonnement auf, die häufig für bestimmte Ressourcengruppen oder Ressourcen gelten. Es kann beispielsweise sein, dass Sie benachrichtigt werden möchten, wenn ein beliebiger virtueller Computer in myProductionResourceGroup gelöscht wird. Unter Umständen möchten Sie auch benachrichtigt werden, wenn einem Benutzer unter Ihrem Abonnement neue Rollen zugewiesen werden.
* Ein Dienstintegritätsereignis tritt auf. Dienstintegritätsereignisse umfassen eine Benachrichtigung bei Vorfällen und Wartungsereignissen, die für Ressourcen Ihres Abonnements gelten.

In beiden Fällen wird mit einer Aktivitätsprotokollwarnung nur eine Überwachung auf Ereignisse in dem Abonnement durchgeführt, unter dem die Warnung erstellt wurde.

Sie können eine Aktivitätsprotokollwarnung basierend auf einer beliebigen Eigenschaft der obersten Ebene im JSON-Objekt für ein Aktivitätsprotokollereignis konfigurieren. Das Portal zeigt jedoch die am häufigsten verwendeten Optionen an:

- **Kategorie**: Verwaltung, Dienstintegrität, Autoskalierung und Empfehlung. Weitere Informationen finden Sie unter [Kategorien im Aktivitätsprotokoll](./monitoring-overview-activity-logs.md#categories-in-the-activity-log). Weitere Informationen zu Dienstintegritätsereignissen finden Sie unter [Erstellen von Aktivitätsprotokollwarnungen zu Dienstbenachrichtigungen](./monitoring-activity-log-alerts-on-service-notifications.md).
- **Ressourcengruppe**
- **Ressource**
- **Ressourcentyp**
- **Vorgangsname**: Der Name des Resource Manager-Vorgangs mit rollenbasierter Zugriffssteuerung.
- **Grad**: Der Schweregrad des Ereignisses („Ausführlich“, „Information“, „Warnung“, „Fehler“ oder „Kritisch“).
- **Status**: Der Status des Ereignisses, z.B. „Gestartet“, „Fehler“ oder „Erfolgreich“.
- **Ereignis initiiert von**: Wird auch als „Aufrufer“ bezeichnet. Die E-Mail-Adresse oder der Azure Active Directory-Bezeichner des Benutzers, der den Vorgang durchgeführt hat.

>[!NOTE]
>Sie müssen in Ihrer Warnung mindestens zwei der obigen Kriterien angeben, wobei eines die Kategorie sein muss. Eine Warnung, die jedes Mal aktiviert wird, wenn ein Ereignis in den Aktivitätsprotokollen erstellt wird, kann nicht erstellt werden.
>
>

Bei Aktivierung einer Aktivitätsprotokollwarnung wird eine Aktionsgruppe verwendet, um Aktionen oder Benachrichtigungen zu generieren. Eine Aktionsgruppe ist ein wiederverwendbarer Satz von Benachrichtigungsempfängern, z.B. E-Mail-Adressen, Webhook-URLs oder SMS-Telefonnummern. Mehrere Warnungen können auf die Empfänger verweisen, um Ihre Benachrichtigungskanäle zu zentralisieren und gruppieren. Wenn Sie Ihre Aktivitätsprotokollwarnung definieren, stehen Ihnen zwei Optionen zur Verfügung. Ihre Möglichkeiten:

* Verwenden Sie eine vorhandene Aktionsgruppe in Ihrer Aktivitätsprotokollwarnung. 
* Erstellen Sie eine neue Aktionsgruppe. 

Weitere Informationen zu Aktionsgruppen finden Sie unter [Erstellen und Verwalten von Aktionsgruppen im Azure-Portal](monitoring-action-groups.md).

Weitere Informationen zu Dienstintegritätsbenachrichtigungen finden Sie unter [Erstellen von Aktivitätsprotokollwarnungen zu Dienstbenachrichtigungen](monitoring-activity-log-alerts-on-service-notifications.md).

## <a name="create-an-alert-on-an-activity-log-event-with-a-new-action-group-by-using-the-azure-portal"></a>Erstellen einer Warnung zu einem Aktivitätsprotokollereignis mit einer neuen Aktionsgruppe im Azure-Portal
1. Wählen Sie im [Portal](https://portal.azure.com) die Option **Überwachen**.

    ![Der Dienst „Überwachen“](./media/monitoring-activity-log-alerts/home-monitor.png)
2. Wählen Sie im Abschnitt **Aktivitätsprotokoll** die Option **Warnungen**.

    ![Die Registerkarte „Warnungen“](./media/monitoring-activity-log-alerts/alerts-blades.png)
3. Wählen Sie **Aktivitätsprotokollwarnung hinzufügen**, und füllen Sie die Felder aus.

4. Geben Sie einen Namen in das Feld **Name der Aktivitätsprotokollwarnung** ein, und wählen Sie eine **Beschreibung**.

    ![Der Befehl „Aktivitätsprotokollwarnung hinzufügen“](./media/monitoring-activity-log-alerts/add-activity-log-alert.png)

5. In das Feld **Abonnement** wird automatisch Ihr aktuelles Abonnement eingetragen. In diesem Abonnement wird die Aktionsgruppe gespeichert. Dies ist das Abonnement, unter dem die Warnungsressource bereitgestellt wurde und die Aktivitätsprotokollereignisse überwacht werden.

    ![Das Dialogfeld „Aktivitätsprotokollwarnung hinzufügen“](./media/monitoring-activity-log-alerts/activity-log-alert-new-action-group.png)

6. Wählen Sie die **Ressourcengruppe** aus, in der die Warnungsressource erstellt wird. Dies ist nicht die Ressourcengruppe, die von der Warnung überwacht wird. Stattdessen ist es die Ressourcengruppe, in der sich die Warnungsressource befindet.

7. Wählen Sie optional eine **Ereigniskategorie** aus, mit der die angezeigten zusätzlichen Filter geändert werden. Für administrative Ereignisse gehören zu den Filtern **Ressourcengruppe**, **Ressource**, **Ressourcentyp**, **Vorgangsname**, **Ebene**, **Status** und **Ereignis initiiert von**. Diese Werte geben an, welche Ereignisse diese Warnung überwachen soll.

    >[!NOTE]
    >Sie müssen mindestens eine der oben genannten Kriterien in der Warnung angeben. Eine Warnung, die jedes Mal aktiviert wird, wenn ein Ereignis in den Aktivitätsprotokollen erstellt wird, kann nicht erstellt werden.
    >
    >

8. Geben Sie jeweils einen Namen in die Felder **Aktionsgruppenname** und **Kurzname** ein. Der Kurzname wird anstelle eines vollständigen Aktionsgruppennamens verwendet, wenn Benachrichtigungen mithilfe dieser Gruppe gesendet werden.

9.  Definieren Sie eine Liste von Aktionen, indem Sie folgende Daten der Aktion angeben:

    a. **Name:** Geben Sie Name, Alias oder Bezeichner der Aktion ein.

    b. **Aktionstyp**: Wählen Sie SMS, E-Mail oder Webhook.

    c. **Details:** Geben Sie basierend auf dem Aktionstyp eine Telefonnummer, eine E-Mail-Adresse oder einen Webhook-URI an.

10. Wählen Sie **OK** , um die Warnung zu erstellen.

Es dauert einige Minuten, bis die Warnung vollständig weitergegeben ist und aktiv wird. Sie wird ausgelöst, wenn neue Ereignisse die Kriterien der Warnung erfüllen.

Weitere Informationen finden Sie unter [Erläuterungen zu dem in Aktivitätsprotokollwarnungen verwendeten Webhookschema](monitoring-activity-log-alerts-webhook.md).

>[!NOTE]
>Die in den folgenden Schritten definierte Aktionsgruppe kann als vorhandene Aktionsgruppe für alle zukünftigen Warnungsdefinitionen wiederverwendet werden.
>
>

## <a name="create-an-alert-on-an-activity-log-event-for-an-existing-action-group-by-using-the-azure-portal"></a>Erstellen einer Warnung zu einem Aktivitätsprotokollereignis für eine vorhandene Aktionsgruppe mit dem Azure-Portal
1. Führen Sie die Schritte 1 bis 7 im vorherigen Abschnitt aus, um Ihre Aktivitätsprotokollwarnung zu erstellen.

2. Wählen Sie unter **Benachrichtigen über** die Aktionsgruppenschaltfläche **Vorhandene**. Wählen Sie in der Liste eine vorhandene Aktionsgruppe aus.

3. Wählen Sie **OK** , um die Warnung zu erstellen.

Es dauert einige Minuten, bis die Warnung vollständig weitergegeben ist und aktiv wird. Sie wird ausgelöst, wenn neue Ereignisse die Kriterien der Warnung erfüllen.

## <a name="manage-your-alerts"></a>Verwalten Ihrer Warnungen

Nachdem Sie eine Warnung erstellt haben, wird sie auf dem Blatt „Überwachen“ im Abschnitt „Warnungen“ angezeigt. Wählen Sie die Warnung, die Sie verwalten möchten, um Folgendes zu tun:

* Sie bearbeiten.
* Sie löschen.
* Deaktivieren oder aktivieren Sie sie, wenn Sie den Empfang von Benachrichtigungen zu dieser Warnung vorübergehend beenden oder fortsetzen möchten.

## <a name="next-steps"></a>Nächste Schritte
- Lesen Sie eine [Übersicht über Warnungen](monitoring-overview-alerts.md).
- Erfahren Sie mehr über [Ratenlimits für SMS, E-Mail-Nachrichten und Webhooks](monitoring-alerts-rate-limiting.md).
- Weitere Informationen zum [Webhookschema für Aktivitätsprotokollwarnungen](monitoring-activity-log-alerts-webhook.md).
- Weitere Informationen zu [Aktionsgruppen](monitoring-action-groups.md).  
- Weitere Informationen zu [Dienstintegritätsbenachrichtigungen](monitoring-service-notifications.md).
- Erstellen Sie eine [Aktivitätsprotokollwarnung, um alle automatischen Modulskalierungsvorgänge für Ihr Abonnement zu überwachen](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert).
- Erstellen Sie eine [Aktivitätsprotokollwarnung, um alle Autoskalierungs-Vorgänge zum horizontalen Herunterskalieren und horizontalen Hochskalieren in Ihrem Abonnement, bei denen Fehler aufgetreten sind, zu überwachen](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).

