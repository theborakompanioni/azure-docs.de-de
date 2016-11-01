<properties 
	pageTitle="Verwenden des Azure-Portals zum Verwalten von Azure-Ressourcen | Microsoft Azure" 
	description="Verwenden Sie das Azure-Portal und Azure Resource Manager zum Verwalten Ihrer Ressourcen. Veranschaulicht das Arbeiten mit Dashboards, um Ressourcen zu überwachen." 
	services="azure-resource-manager,azure-portal" 
	documentationCenter="" 
	authors="tfitzmac" 
	manager="timlt" 
	editor="tysonn"/>

<tags 
	ms.service="azure-resource-manager" 
	ms.workload="multiple" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="09/12/2016" 
	ms.author="tomfitz"/>

# Verwalten von Azure-Ressourcen über das Portal

> [AZURE.SELECTOR]
- [Azure PowerShell](../powershell-azure-resource-manager.md)
- [Azure-Befehlszeilenschnittstelle](../xplat-cli-azure-resource-manager.md)
- [Portal](resource-group-portal.md)
- [REST-API](../resource-manager-rest-api.md)

In diesem Thema wird veranschaulicht, wie Sie das [Azure-Portal](https://portal.azure.com) mit [Azure Resource Manager](../resource-group-overview.md) verwenden, um Ihre Azure-Ressourcen zu verwalten. Informationen zum Bereitstellen von Ressourcen über das Portal finden Sie unter [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure-Portal](../resource-group-template-deploy-portal.md).

Das Portal und der Ressourcen-Manager werden derzeit nicht von allen Diensten unterstützt. Für diese Dienste müssen Sie das [klassische Portal](https://manage.windowsazure.com) verwenden. Den Status der einzelnen Dienste finden Sie im [Verfügbarkeitsdiagramm der Azure-Portale](https://azure.microsoft.com/features/azure-portal/availability/).

## Verwalten von Ressourcengruppen

1. Wählen Sie zum Anzeigen aller Ressourcengruppen in Ihrem Abonnement die Option **Ressourcengruppen**.

    ![Ressourcengruppen durchsuchen](./media/resource-group-portal/browse-groups.png)

1. Wählen Sie zum Erstellen einer leeren Ressourcengruppe die Option **Hinzufügen**.

    ![Ressourcengruppe hinzufügen](./media/resource-group-portal/add-resource-group.png)

1. Geben Sie einen Namen und Speicherort für die neue Ressourcengruppe an. Klicken Sie auf **Erstellen**.

    ![Ressourcengruppe erstellen](./media/resource-group-portal/create-empty-group.png)

1. Unter Umständen müssen Sie **Aktualisieren** auswählen, damit die zuletzt erstellte Ressourcengruppe angezeigt wird.

    ![Ressourcengruppe aktualisieren](./media/resource-group-portal/refresh-resource-groups.png)

1. Wählen Sie zum Anpassen der für die Ressourcengruppe angezeigten Informationen die Option **Spalten**.

    ![Spalten anpassen](./media/resource-group-portal/select-columns.png)

1. Wählen Sie die hinzuzufügenden Spalten und dann die Option **Aktualisieren** aus.

    ![Spalten hinzufügen](./media/resource-group-portal/add-columns.png)

1. Informationen zum Bereitstellen von Ressourcen für die neue Ressourcengruppe finden Sie unter [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure-Portal](../resource-group-template-deploy-portal.md).

1. Um den schnellen Zugriff auf eine Ressourcengruppe zu ermöglichen, können Sie das Blatt in Ihrem Dashboard anheften.

    ![Ressourcengruppe anheften](./media/resource-group-portal/pin-group.png)

1. Im Dashboard wird die Ressourcengruppe mit den zugehörigen Ressourcen angezeigt. Sie können entweder die Ressourcengruppen oder eine der Ressourcen auswählen, um zum jeweiligen Element zu navigieren.

    ![Ressourcengruppe anheften](./media/resource-group-portal/show-resource-group-dashboard.png)

## Markieren von Ressourcen

Sie können Ressourcengruppen und Ressourcen Tags zuordnen, um sie logisch zu organisieren. Informationen zur Verwendung von Tags finden Sie unter [Verwenden von Tags zum Organisieren von Azure-Ressourcen](../resource-group-using-tags.md).

[AZURE.INCLUDE [resource-manager-tag-resource](../../includes/resource-manager-tag-resources.md)]

## Überwachen von Ressourcen

Wenn Sie eine Ressource auswählen, werden auf dem Ressourcenblatt Standarddiagramme und -tabellen zur Überwachung des Ressourcentyps angezeigt.

1. Wählen Sie eine Ressource aus, und beachten Sie den Abschnitt **Überwachen**. Dieser enthält Diagramme, die für den Ressourcentyp relevant sind. Die folgende Abbildung zeigt die standardmäßigen Überwachungsdaten für ein Speicherkonto.

    ![Überwachung anzeigen](./media/resource-group-portal/show-monitoring.png)

1. Sie können einen Abschnitt des Blatts an das Dashboard anheften, indem Sie auf die Auslassungspunkte (...) oberhalb des Abschnitts klicken. Außerdem können Sie die Größe des Abschnitts im Blatt anpassen oder ihn vollständig entfernen. Die folgende Abbildung veranschaulicht, wie Sie den Abschnitt „CPU und Arbeitsspeicher“ anheften, anpassen oder entfernen.

    ![Abschnitt anheften](./media/resource-group-portal/pin-cpu-section.png)

1. Nach dem Anheften des Abschnitts im Dashboard wird die Zusammenfassung im Dashboard angezeigt. Wenn Sie diese auswählen, werden sofort weitere Details zu den Daten eingeblendet.

    ![Dashboard anzeigen](./media/resource-group-portal/view-startboard.png)

1. Um die Daten, die Sie über das Portal überwachen, vollständig anzupassen, navigieren Sie zu Ihrem Standarddashboard, und wählen Sie **Neues Dashboard** aus.

    ![Dashboard](./media/resource-group-portal/dashboard.png)

1. Benennen Sie das neue Dashboard, und ziehen Sie Kacheln darauf. Die Kacheln werden anhand verschiedener Optionen gefiltert.

    ![Dashboard](./media/resource-group-portal/create-dashboard.png)

     Um sich mit der Verwendung von Dashboards vertraut zu machen, lesen Sie [Erstellen und Freigeben von Dashboards im Azure-Portal](azure-portal-dashboards.md).

## Verwalten von Ressourcen

Auf dem Blatt für eine Ressource sehen Sie die Optionen, die Ihnen zum Verwalten der Ressource zur Verfügung stehen. Das Portal zeigt Verwaltungsoptionen für diesen speziellen Ressourcentyp an. Im oberen Bereich und auf der linken Seite des Ressourcenblatts sehen Sie die verfügbaren Verwaltungsbefehle.

![Ressourcen verwalten](./media/resource-group-portal/manage-resources.png)

Mit diesen Optionen können Sie Vorgänge wie das Starten und Anhalten eines virtuellen Computers oder das Neukonfigurieren der Eigenschaften eines virtuellen Computers durchführen.

## Verschieben von Ressourcen

Wenn Sie Ressourcen in eine andere Ressourcengruppe oder ein anderes Abonnement verschieben möchten, helfen Ihnen die Informationen unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../resource-group-move-resources.md) weiter.

## Sperren von Ressourcen

Sie können ein Abonnement, eine Ressourcengruppe oder eine Ressource sperren, um zu verhindern, dass andere Benutzer in Ihrer Organisation versehentlich wichtige Ressourcen löschen oder ändern. Weitere Informationen finden Sie unter [Sperren von Ressourcen mit dem Azure-Ressourcen-Manager](../resource-group-lock-resources.md).

[AZURE.INCLUDE [resource-manager-lock-resources](../../includes/resource-manager-lock-resources.md)]

## Anzeigen Ihres Abonnements und der Kosten

Sie können Informationen zu Ihrem Abonnement und die zusammengefassten Kosten für Ihre gesamten Ressourcen anzeigen. Wählen Sie **Abonnements** und dann das Abonnement aus, das Sie anzeigen möchten. Möglicherweise steht nur ein Abonnement zur Auswahl.

![Abonnement](./media/resource-group-portal/select-subscription.png)

Auf dem Blatt „Abonnement“ wird eine Verbrauchsrate angezeigt.

![Verbrauchsrate](./media/resource-group-portal/burn-rate.png)

Eine Aufschlüsselung der Kosten nach Ressourcentyp wird ebenfalls angezeigt.

![Ressourcenkosten](./media/resource-group-portal/cost-by-resource.png)

## Exportieren der Vorlage

Nach dem Einrichten der Ressourcengruppe kann es sein, dass Sie die Resource Manager-Vorlage für die Ressourcengruppe anzeigen möchten. Das Exportieren der Vorlage hat zwei Vorteile:

1. Sie können zukünftige Bereitstellungen der Lösung leicht automatisieren, da die Vorlage die gesamte Infrastruktur enthält.

2. Sie können sich mit der Vorlagensyntax vertraut machen, indem Sie sich die JavaScript Object Notation (JSON) zu Ihrer Lösung ansehen.

Eine Schrittanleitung finden Sie unter [Exportieren einer Azure Resource Manager-Vorlage aus vorhandenen Ressourcen](../resource-manager-export-template.md).

## Löschen von Ressourcen oder Ressourcengruppen

Beim Löschen einer Ressourcengruppe werden alle darin enthaltenen Ressourcen gelöscht. Sie können auch einzelne Ressourcen in einer Ressourcengruppe löschen. Gehen Sie beim Löschen einer Ressourcengruppe mit Bedacht vor, da Ressourcen in anderen Ressourcengruppen damit verknüpft sein können. Resource Manager löscht verknüpfte Ressourcen nicht, aber ohne die erwarteten Ressourcen funktionieren sie unter Umständen nicht richtig.

![Gruppe löschen](./media/resource-group-portal/delete-group.png)

## Nächste Schritte

- Informationen zum Anzeigen von Überwachungsprotokollen finden Sie unter [Überwachen von Vorgängen mit Resource Manager](../resource-group-audit.md).
- Informationen zur Behebung von Bereitstellungsfehlern finden Sie unter [Problembehandlung beim Bereitstellen von Ressourcengruppen mit dem Azure-Portal](../resource-manager-troubleshoot-deployments-portal.md).
- Informationen zum Bereitstellen von Ressourcen über das Portal finden Sie unter [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure-Portal](../resource-group-template-deploy-portal.md).
- Informationen zum Verwalten des Zugriffs auf Ressourcen finden Sie unter [Verwenden von Rollenzuweisungen zum Verwalten Ihrer Azure-Abonnementressourcen](../active-directory/role-based-access-control-configure.md).

<!---HONumber=AcomDC_0914_2016-->