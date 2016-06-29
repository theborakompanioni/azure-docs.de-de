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
	ms.date="06/10/2016" 
	ms.author="tomfitz"/>


# Verwalten von Azure-Ressourcen über das Portal

In diesem Thema wird veranschaulicht, wie Sie das [Azure-Portal](https://portal.azure.com) mit [Azure Resource Manager](../resource-group-overview.md) verwenden, um Ihre Azure-Ressourcen zu verwalten. Informationen zum Bereitstellen von Ressourcen über das Portal finden Sie unter [Bereitstellen von Ressourcen mit Resource Manager-Vorlagen und Azure-Portal](../resource-group-template-deploy-portal.md).

Das Portal und der Ressourcen-Manager werden derzeit nicht von allen Diensten unterstützt. Verwenden Sie für diese Dienste das [klassische Portal](https://manage.windowsazure.com). Den Status der einzelnen Dienste finden Sie im [Verfügbarkeitsdiagramm für das Azure-Portal](https://azure.microsoft.com/features/azure-portal/availability/).

<a id="access-control-for-azure-dashboards" />
## Anpassen des Dashboards zum Überwachen von Ressourcen

Das Portal bietet ein Dashboard, das Sie zur Überwachung und Verwaltung Ihrer Ressourcen verwenden können. Das Dashboard ist vollständig anpassbar. Sie können zudem mehrere Dashboards erstellen, um mehrere Sichten auf das Abonnement zu ermöglichen.

![Dashboard](./media/resource-group-portal/dashboard.png)

> [AZURE.TIP] Die beste Möglichkeit, sich mit dem Arbeiten mit Dashboards vertraut zu machen, bietet das Video zum [Erstellen benutzerdefinierter Dashboards im Microsoft Azure-Portal](https://channel9.msdn.com/Blogs/trevor-cloud/azure-portal-dashboards).

### Freigeben von Azure-Dashboards und Zugriffssteuerung
Nach der Konfiguration eines Dashboards können Sie es veröffentlichen und für andere Benutzer in Ihrer Organisation freigeben. Der Zugriff auf die Informationen, die auf Kacheln im Portal angezeigt werden, wird in Azure über die [rollenbasierte Zugriffssteuerung](../active-directory/role-based-access-control-configure.md) geregelt. Alle veröffentlichten Dashboards werden als Azure-Ressourcen implementiert. Aus Sicht der Zugriffssteuerung unterscheiden sich Dashboards nicht von einem virtuellen Computer oder Speicherkonto.

Beispiel: Angenommen, Sie verfügen über ein Azure-Abonnement und verschiedenen Mitgliedern Ihres Teams wurden die Rollen **Besitzer**, **Mitwirkender** oder**Leser** für das Abonnement zugewiesen. Benutzer, die Besitzer oder Mitwirkende sind, können Dashboards im Abonnement auflisten, anzeigen, erstellen, ändern oder löschen. Benutzer, die als Leser festgelegt wurden, können Dashboards auflisten und anzeigen, aber sie können diese nicht ändern oder löschen. Benutzer mit Lesezugriff können lokale Änderungen an einem veröffentlichten Dashboard vornehmen (z.B. beim Behandeln eines Problems), haben aber nicht die Möglichkeit, diese Änderungen zur Veröffentlichung zurück an den Server zu übergeben. Diese Benutzer können eine private Kopie des Dashboards zur eigenen Nutzung erstellen.

Einzelne Kacheln im Dashboard erzwingen basierend auf den von ihnen gezeigten Ressourcen eigene Anforderungen an die Zugriffssteuerung. Sie können deshalb ein Dashboard entwerfen, das umfassend freigegeben werden kann, während die Daten auf den einzelnen Kacheln trotzdem geschützt sind.

## Verwalten von Ressourcengruppen

1. Wählen Sie zum Anzeigen aller Ressourcengruppen in Ihrem Abonnement **Ressourcengruppen** aus.

    ![Ressourcengruppen durchsuchen](./media/resource-group-portal/browse-groups.png)

2. Wählen Sie die Ressourcengruppe aus, die Sie verwalten möchten. Es wird das Blatt „Ressourcengruppe“ mit Informationen zur jeweiligen Ressourcengruppe einschließlich aller Ressourcen in der Gruppe angezeigt.

    ![Zusammenfassung der Ressourcengruppe](./media/resource-group-portal/group-summary.png)

    Wenn Sie eine der Ressourcen auswählen, werden Details dazu angezeigt.

3. Auf dem Blatt einer Ressource können Sie weitere Graphen und Tabellen hinzufügen, indem Sie unter der Zusammenfassung **Abschnitt hinzufügen** auswählen.

    ![Abschnitt hinzufügen](./media/resource-group-portal/add-section.png)

4. In einem Kachelkatalog können Sie die Informationen auswählen, die in das Blatt eingebunden werden sollen. Die Typen der angezeigten Kacheln werden je nach Ressourcentyp gefiltert. Wenn Sie eine andere Ressource auswählen, ändern sich die verfügbaren Kacheln.

    ![Abschnitt hinzufügen](./media/resource-group-portal/tile-gallery.png)

5. Ziehen Sie die gewünschte Kachel jeweils an eine der verfügbaren Positionen.

    ![Kachel ziehen](./media/resource-group-portal/drag-tile.png)

6. Nachdem Sie oben im Portal **Fertig** ausgewählt haben, ist die neue Sicht Teil des Blatts.

    ![Kachel anzeigen](./media/resource-group-portal/show-lens.png)

7. Um den schnellen Zugriff auf eine Ressourcengruppe zu ermöglichen, können Sie das Blatt in Ihrem Dashboard anheften.

    ![Ressourcengruppe anheften](./media/resource-group-portal/pin-group.png)

    Sie können auch einen Abschnitt des Blatts an das Dashboard anheften, indem Sie auf die Auslassungspunkte (...) oberhalb des Abschnitts klicken. Außerdem können Sie die Größe des Abschnitts im Blatt anpassen oder ihn vollständig entfernen. Die folgende Abbildung veranschaulicht, wie Sie den Abschnitt „CPU und Arbeitsspeicher“ anheften, anpassen oder entfernen.

    ![Abschnitt anheften](./media/resource-group-portal/pin-cpu-section.png)

8. Nach dem Anheften des Abschnitts im Dashboard wird die Zusammenfassung im Dashboard angezeigt.

    ![Dashboard anzeigen](./media/resource-group-portal/view-startboard.png)

Wenn Sie diese auswählen, werden sofort weitere Details zu den Daten eingeblendet.

## Markieren von Ressourcen

Sie können Ressourcengruppen und Ressourcen Tags zuordnen, um sie logisch zu organisieren. Informationen zur Verwendung von Tags über das Portal finden Sie unter [Verwenden von Tags zum Organisieren von Azure-Ressourcen](../resource-group-using-tags.md).

## Anzeigen Ihres Abonnements und der Kosten

Sie können Informationen zu Ihrem Abonnement und die zusammengefassten Kosten für alle Ihre Ressourcen anzeigen. Wählen Sie **Abonnements** und das Abonnement aus, das Sie anzeigen möchten. Möglicherweise steht nur ein Abonnement zur Auswahl.

![Abonnement](./media/resource-group-portal/select-subscription.png)

Im Blatt „Abonnement“ wird eine Verbrauchsrate angezeigt.

![Verbrauchsrate](./media/resource-group-portal/burn-rate.png)

Eine Aufschlüsselung der Kosten nach Ressourcentyp wird ebenfalls angezeigt.

![Ressourcenkosten](./media/resource-group-portal/cost-by-resource.png)

## Exportieren der Vorlage

Nach dem Einrichten der Ressourcengruppe kann es sein, dass Sie die Resource Manager-Vorlage für die Ressourcengruppe anzeigen möchten. Das Exportieren der Vorlage hat zwei Vorteile:

1. Sie können zukünftige Bereitstellungen der Lösung leicht automatisieren, da die gesamte Infrastruktur in der Vorlage definiert ist.

2. Sie können sich mit der Vorlagensyntax vertraut machen, indem Sie sich die JavaScript Object Notation (JSON) zu Ihrer Lösung ansehen.

Eine detaillierte Anleitung finden Sie unter [Exportieren einer Azure Resource Manager-Vorlage aus vorhandenen Ressourcen](../resource-manager-export-template.md).

## Löschen von Ressourcen oder Ressourcengruppen

Beim Löschen einer Ressourcengruppe werden alle darin enthaltenen Ressourcen gelöscht. Sie können auch einzelne Ressourcen in einer Ressourcengruppe löschen. Gehen Sie beim Löschen einer Ressourcengruppe mit Bedacht vor, da Ressourcen in anderen Ressourcengruppen damit verknüpft sein können. Die verknüpften Ressourcen werden nicht gelöscht, aber sie funktionieren unter Umständen nicht wie erwartet.

![Gruppe löschen](./media/resource-group-portal/delete-group.png)


## Nächste Schritte

- Informationen zum Anzeigen von Überwachungsprotokollen finden Sie unter [Überwachen von Vorgängen mit Resource Manager](../resource-group-audit.md).
- Informationen zur Behebung von Bereitstellungsfehlern finden Sie unter [Problembehandlung beim Bereitstellen von Ressourcengruppen mit dem Azure-Portal](../resource-manager-troubleshoot-deployments-portal.md).
- Informationen zum Bereitstellen von Ressourcen über das Portal finden Sie unter [Bereitstellen von Ressourcen mit Resource Manager-Vorlagen und Azure-Portal](../resource-group-template-deploy-portal.md).

<!---HONumber=AcomDC_0615_2016-->