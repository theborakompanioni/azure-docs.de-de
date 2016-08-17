<properties 
	pageTitle="Verwenden des Azure-Portals zum Verwalten von Azure-Ressourcen | Microsoft Azure" 
	description="Verwenden Sie das Azure-Portal und Azure Resource Manager zum Verwalten Ihrer Ressourcen. Veranschaulicht das Arbeiten mit Dashboards und Kacheln, um Ressourcen zu überwachen." 
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
	ms.date="08/03/2016" 
	ms.author="tomfitz"/>


# Verwalten von Azure-Ressourcen über das Portal

> [AZURE.SELECTOR]
- [Portal](azure-portal/resource-group-portal.md)
- [Azure-Befehlszeilenschnittstelle](xplat-cli-azure-resource-manager.md)
- [Azure PowerShell](powershell-azure-resource-manager.md)
- [.NET](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-resources-and-groups/)
- [Java](https://azure.microsoft.com/documentation/samples/resources-java-manage-resource-group/)
- [Knoten](https://azure.microsoft.com/documentation/samples/resource-manager-node-resources-and-groups/)
- [Python](https://azure.microsoft.com/documentation/samples/resource-manager-python-resources-and-groups/)
- [Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

In diesem Thema wird veranschaulicht, wie Sie das [Azure-Portal](https://portal.azure.com) mit [Azure Resource Manager](../resource-group-overview.md) verwenden, um Ihre Azure-Ressourcen zu verwalten. Das Portal und der Ressourcen-Manager werden derzeit nicht von allen Diensten unterstützt. Verwenden Sie für diese Dienste das [klassische Portal](https://manage.windowsazure.com). Den Status der einzelnen Dienste finden Sie im [Verfügbarkeitsdiagramm für das Azure-Portal](https://azure.microsoft.com/features/azure-portal/availability/).

## Verwalten von Ressourcengruppen

1. Wählen Sie zum Anzeigen aller Ressourcengruppen in Ihrem Abonnement die Option **Ressourcengruppen**.

    ![Ressourcengruppen durchsuchen](./media/resource-group-portal/browse-groups.png)

1. Wählen Sie zum Erstellen einer leeren Ressourcengruppe die Option **Hinzufügen**.

    ![Ressourcengruppe hinzufügen](./media/resource-group-portal/add-resource-group.png)

1. Geben Sie einen Namen und Speicherort für die neue Ressourcengruppe an. Klicken Sie auf **Erstellen**.

    ![Ressourcengruppe erstellen](./media/resource-group-portal/create-empty-group.png)

1. Unter Umständen müssen Sie **Aktualisieren** wählen, damit die zuletzt erstellte Ressourcengruppe angezeigt wird.

    ![Ressourcengruppe aktualisieren](./media/resource-group-portal/refresh-resource-groups.png)

1. Wählen Sie zum Anpassen der für die Ressourcengruppe angezeigten Informationen die Option **Spalten**.

    ![Spalten anpassen](./media/resource-group-portal/select-columns.png)

1. Wählen Sie die hinzuzufügenden Spalten und dann die Option **Aktualisieren** aus.

    ![Spalten hinzufügen](./media/resource-group-portal/add-columns.png)

1. Informationen zum Bereitstellen von Ressourcen für die neue Ressourcengruppe finden Sie unter [Bereitstellen von Ressourcen mit Resource Manager-Vorlagen und Azure-Portal](../resource-group-template-deploy-portal.md).

1. Um den schnellen Zugriff auf eine Ressourcengruppe zu ermöglichen, können Sie das Blatt in Ihrem Dashboard anheften.

    ![Ressourcengruppe anheften](./media/resource-group-portal/pin-group.png)

1. Im Dashboard wird die Ressourcengruppe mit den zugehörigen Ressourcen angezeigt. Sie können entweder die Ressourcengruppen oder eine der Ressourcen auswählen, um zum jeweiligen Element zu navigieren.

    ![Ressourcengruppe anheften](./media/resource-group-portal/show-resource-group-dashboard.png)

## Markieren von Ressourcen

Sie können Ressourcengruppen und Ressourcen Tags zuordnen, um sie logisch zu organisieren. Informationen zur Verwendung von Tags finden Sie unter [Verwenden von Tags zum Organisieren von Azure-Ressourcen](../resource-group-using-tags.md).

[AZURE.INCLUDE [resource-manager-tag-resource](../../includes/resource-manager-tag-resources.md)]

## Überwachen von Ressourcen

Wenn Sie eine Ressource auswählen, werden auf dem Ressourcenblatt Standarddiagramme und -tabellen zur Überwachung des Ressourcentyps angezeigt. Sie können anpassen, wie Sie die Ressource überwachen, indem Sie die angezeigten Diagramme und Tabellen ändern.

1. Fügen Sie auf dem Blatt einer Ressource weitere Diagramme und Tabellen hinzu, indem Sie unter der Zusammenfassung **Abschnitt hinzufügen** auswählen.

    ![Abschnitt hinzufügen](./media/resource-group-portal/add-section.png)

1. Wählen Sie im Kachelkatalog die Informationen aus, die auf dem Blatt angezeigt werden sollen. Der Editor filtert die Kacheln nach dem Ressourcentyp. Wenn Sie eine andere Ressource auswählen, ändern sich die verfügbaren Kacheln.

    ![Abschnitt hinzufügen](./media/resource-group-portal/tile-gallery.png)

1. Ziehen Sie die gewünschte Kachel jeweils an eine der verfügbaren Positionen.

    ![Kachel ziehen](./media/resource-group-portal/drag-tile.png)

1. Nachdem Sie oben im Portal **Fertig** ausgewählt haben, ist die neue Ansicht Teil des Blatts.

    ![Kachel anzeigen](./media/resource-group-portal/show-lens.png)

1. Sie können einen Abschnitt des Blatts an das Dashboard anheften, indem Sie auf die Auslassungspunkte (...) oberhalb des Abschnitts klicken. Außerdem können Sie die Größe des Abschnitts im Blatt anpassen oder ihn vollständig entfernen. Die folgende Abbildung veranschaulicht, wie Sie den Abschnitt „CPU und Arbeitsspeicher“ anheften, anpassen oder entfernen.

    ![Abschnitt anheften](./media/resource-group-portal/pin-cpu-section.png)

1. Nach dem Anheften des Abschnitts im Dashboard wird die Zusammenfassung im Dashboard angezeigt. Wenn Sie diese auswählen, werden sofort weitere Details zu den Daten eingeblendet.

    ![Dashboard anzeigen](./media/resource-group-portal/view-startboard.png)

1. Außerdem können Sie mehrere Dashboards zum Überwachen und Verwalten Ihrer Ressourcen erstellen und diese Dashboards für andere Benutzer in Ihrer Organisation freigeben. Wählen Sie **Neues Dashboard**.

    ![Dashboard](./media/resource-group-portal/dashboard.png)

     Sie können sich mit der Verwendung von Dashboards vertraut machen, indem Sie sich das Video zum [Erstellen benutzerdefinierter Dashboards im Microsoft Azure-Portal](https://channel9.msdn.com/Blogs/trevor-cloud/azure-portal-dashboards) ansehen. Informationen zum Freigeben des Zugriffs auf ein veröffentlichtes Dashboard finden Sie unter [Sharing Azure dashboards](azure-portal-dashboard-share-access.md) (Freigeben von Azure-Dashboards).

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

Eine detaillierte Anleitung finden Sie unter [Exportieren einer Azure Resource Manager-Vorlage aus vorhandenen Ressourcen](../resource-manager-export-template.md).

## Löschen von Ressourcen oder Ressourcengruppen

Beim Löschen einer Ressourcengruppe werden alle darin enthaltenen Ressourcen gelöscht. Sie können auch einzelne Ressourcen in einer Ressourcengruppe löschen. Gehen Sie beim Löschen einer Ressourcengruppe mit Bedacht vor, da Ressourcen in anderen Ressourcengruppen damit verknüpft sein können. Resource Manager löscht verknüpfte Ressourcen nicht, aber ohne die erwarteten Ressourcen funktionieren sie unter Umständen nicht richtig.

![Gruppe löschen](./media/resource-group-portal/delete-group.png)

## Nächste Schritte

- Informationen zum Anzeigen von Überwachungsprotokollen finden Sie unter [Überwachen von Vorgängen mit Resource Manager](../resource-group-audit.md).
- Informationen zur Behebung von Bereitstellungsfehlern finden Sie unter [Problembehandlung beim Bereitstellen von Ressourcengruppen mit dem Azure-Portal](../resource-manager-troubleshoot-deployments-portal.md).
- Informationen zum Bereitstellen von Ressourcen über das Portal finden Sie unter [Bereitstellen von Ressourcen mit Resource Manager-Vorlagen und Azure-Portal](../resource-group-template-deploy-portal.md).
- Informationen zum Verwalten des Zugriffs auf Ressourcen finden Sie unter [Verwenden von Rollenzuweisungen zum Verwalten Ihrer Azure-Abonnementressourcen](../active-directory/role-based-access-control-configure.md).

<!---HONumber=AcomDC_0803_2016-->