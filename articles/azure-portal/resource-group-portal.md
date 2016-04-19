<properties 
	pageTitle="Verwenden des Azure-Portals zum Verwalten von Azure-Ressourcen | Microsoft Azure" 
	description="Verwenden Sie das Azure-Portal und Azure Resource Manager zum Bereitstellen und Verwalten Ihrer Ressourcen. Es wird gezeigt, wie Sie Ressourcen markieren und Überwachungsprotokolle anzeigen." 
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
	ms.topic="get-started-article" 
	ms.date="04/08/2016" 
	ms.author="tomfitz"/>


# Verwenden des Azure-Portals zum Bereitstellen und Verwalten Ihrer Azure-Ressourcen

## Einführung

In diesem Thema wird veranschaulicht, wie Sie das [Azure-Portal](https://portal.azure.com) mit [Azure Resource Manager](../resource-group-overview.md) verwenden, um Ihre Azure-Ressourcen bereitzustellen und zu verwalten.

Das Portal und der Ressourcen-Manager werden derzeit nicht von allen Diensten unterstützt. Verwenden Sie für diese Dienste das [klassische Portal](https://manage.windowsazure.com). Den Status der einzelnen Dienste finden Sie im [Verfügbarkeitsdiagramm für das Azure-Portal](https://azure.microsoft.com/features/azure-portal/availability/).

Sie können Ressourcen auch über Azure PowerShell und die Azure-Befehlszeilenschnittstelle verwalten. Weitere Informationen zur Verwendung dieser Schnittstellen finden Sie unter [Verwenden von Azure PowerShell mit dem Azure-Ressourcen-Manager](../powershell-azure-resource-manager.md) und [Verwenden der plattformübergreifenden Azure-Befehlszeilenschnittstelle mit dem Azure-Ressourcen-Manager](../xplat-cli-azure-resource-manager.md). Weitere Informationen zum Bereitstellen von Lösungen mit Visual Studio finden Sie unter [Erstellen und Bereitstellen von Azure-Ressourcengruppen über Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

## Ressourcengruppe erstellen

Wählen Sie zum Erstellen einer leeren Ressourcengruppe **Neu**, **Verwaltung** und **Ressourcengruppe**.

![Leere Ressourcengruppe erstellen](./media/resource-group-portal/create-empty-group.png)

Geben Sie einen Namen und Speicherort an, und wählen Sie ein Abonnement aus, falls dies erforderlich ist.

![Gruppenwerte festlegen](./media/resource-group-portal/set-group-properties.png)

## Bereitstellen von Ressourcen

Nachdem Sie eine Ressourcengruppe erstellt haben, können Sie dafür Ressourcen bereitstellen. Wählen Sie zum Starten einer Bereitstellung einfach **Neu**, und geben Sie den Typ von Ressource an, den Sie bereitstellen möchten.

![Ressource bereitstellen](./media/resource-group-portal/deploy-resource.png)

Falls der gewünschte Ressourcentyp für die Bereitstellung nicht angezeigt wird, können Sie auf dem Marketplace danach suchen.

![Marketplace durchsuchen](./media/resource-group-portal/search-resource.png)

Je nach ausgewähltem Ressourcentyp müssen Sie einige relevante Eigenschaften festlegen, bevor die Bereitstellung beginnen kann. Diese Optionen werden hier nicht angezeigt, da sie je nach Ressourcentyp variieren. Für alle Typen müssen Sie eine Zielressourcengruppe auswählen. Die folgende Abbildung zeigt, wie Sie eine neue Web-App erstellen und in der gerade erstellten Ressourcengruppe bereitstellen.

![Ressourcengruppe erstellen](./media/resource-group-portal/select-existing-group.png)

Sie können aber auch beim Bereitstellen Ihrer Ressourcen eine neue Ressourcengruppe erstellen. Wählen Sie anstelle einer vorhandenen Ressourcengruppe in Ihrem Abonnement die Option **Neu**, und geben Sie der Ressourcengruppe einen Namen.

![neue Ressourcengruppe erstellen](./media/resource-group-portal/select-new-group.png)

Die Bereitstellung wird gestartet. Dies kann einige Minuten in Anspruch nehmen. Nachdem die Bereitstellung abgeschlossen ist, wird eine Benachrichtigung angezeigt.

![Benachrichtigung anzeigen](./media/resource-group-portal/view-notification.png)

Nach der Bereitstellung von Ressourcen kann es sein, dass Sie der Gruppe weitere Ressourcen hinzufügen müssen. Mit dem Befehl **Hinzufügen** im Blatt „Ressourcengruppe“ können Sie einer Ressourcengruppe Ressourcen hinzufügen.

![Ressource hinzufügen](./media/resource-group-portal/add-resource.png)

## Exportieren der Vorlage

Nach dem Einrichten der Ressourcengruppe kann es sein, dass Sie die Resource Manager-Vorlage für die Ressourcengruppe anzeigen möchten. Das Exportieren der Vorlage hat zwei Vorteile:

1. Sie können zukünftige Bereitstellungen der Lösung leicht automatisieren, da die gesamte Infrastruktur in der Vorlage definiert ist.

2. Sie können sich mit der Vorlagensyntax vertraut machen, indem Sie sich die JavaScript Object Notation (JSON) zu Ihrer Lösung ansehen.

Über das Portal können Sie entweder eine Vorlage generieren, die den aktuellen Status Ihrer Ressourcengruppe darstellt, oder die Vorlage abrufen, die für eine bestimmte Bereitstellung verwendet wurde. Beide Optionen werden in diesem Thema veranschaulicht.

Das Exportieren der Vorlage für eine Ressourcengruppe ist hilfreich, wenn Sie Änderungen an einer Ressourcengruppe vorgenommen haben und die JSON-Darstellung ihres aktuellen Zustands abrufen müssen. Die generierte Vorlage enthält aber nur eine minimale Anzahl von Parametern und keine Variablen. Die meisten Werte in der Vorlage sind hartcodiert. Vor dem Bereitstellen der generierten Vorlage kann es ratsam sein, weitere Werte in Parameter zu konvertieren, damit Sie die Bereitstellung für unterschiedliche Umgebungen anpassen können.

Das Exportieren der Vorlage für eine bestimmte Bereitstellung ist hilfreich, wenn Sie die eigentliche Vorlage anzeigen möchten, die zum Bereitstellen von Ressourcen verwendet wurde. Die Vorlage enthält alle Parameter und Variablen, die für die ursprüngliche Bereitstellung definiert wurden. Wenn eine Person in Ihrer Organisation aber Änderungen an der Ressourcengruppe vorgenommen hat, die über die Definition in der Vorlage hinausgehen, stellt diese Vorlage nicht den aktuellen Zustand der Ressourcengruppe dar.

> [AZURE.NOTE] Die Funktion zum Exportieren von Vorlagen befindet sich in der Vorschau, und das Exportieren einer Vorlage wird derzeit nicht für alle Ressourcentypen unterstützt. Wenn Sie versuchen, eine Vorlage zu exportieren, wird ggf. ein Fehler mit dem Hinweis angezeigt, dass einige Ressourcen nicht exportiert wurden. Bei Bedarf können Sie diese Ressourcen nach dem Herunterladen manuell in der Vorlage definieren.

### Exportieren einer Vorlage für die Ressourcengruppe

Im Blatt „Ressourcengruppe“ können Sie die Vorlage exportieren, die den aktuellen Zustand der Ressource darstellt.

Wählen Sie zum Anzeigen der Vorlage für eine Ressourcengruppe die Option **Vorlage exportieren**.

![Ressourcengruppe exportieren](./media/resource-group-portal/export-resource-group.png)

Der Resource Manager generiert vier Dateien für Sie:

1. Die Vorlage, mit der die Infrastruktur für Ihre Lösung definiert wird

2. Eine Parameterdatei, die Sie zum Übergeben von Werten während der Bereitstellung verwenden können

3. Eine Azure PowerShell-Skriptdatei, die Sie zum Bereitstellen der Vorlage ausführen können

4. Eine Skriptdatei der Azure-Befehlszeilenschnittstelle, die Sie zum Bereitstellen der Vorlage ausführen können

Sehen Sie sich zuerst die Vorlage an, die die aktuelle Ressourcengruppe darstellt.

![Vorlage anzeigen](./media/resource-group-portal/show-rg-template.png)

Im Abschnitt **Ressourcen** werden die Definitionen für die bereitzustellenden Ressourcen angezeigt.

In der Parameterdatei können Sie Parameterwerte speichern, die während der Bereitstellung übergeben werden sollen.

![Parameter anzeigen](./media/resource-group-portal/show-parameters.png)

Es ist eine Skriptdatei zum Bereitstellen der Vorlage über die Azure PowerShell vorhanden.

![Azure PowerShell anzeigen](./media/resource-group-portal/show-powershell.png)

Außerdem ist eine Skriptdatei zum Bereitstellen der Vorlage über die Azure-Befehlszeilenschnittstelle vorhanden.

![Azure-Befehlszeilenschnittstelle anzeigen](./media/resource-group-portal/show-cli.png)

Das Portal enthält drei Optionen zum Verwenden dieser Vorlage. Wählen Sie **Bereitstellen**, um die Vorlage jetzt erneut bereitzustellen. Wählen Sie die Option **Herunterladen**, um alle Dateien lokal herunterzuladen. Wählen Sie **Vorlage speichern**, um die Dateien zur späteren Verwendung über das Portal in Ihrem Azure-Konto zu speichern.

### Herunterladen der Vorlage aus einer Bereitstellung

Im Ressourcengruppen-Blatt können Sie das Datum und den Status der letzten Bereitstellung für diese Ressourcengruppe sehen. Wenn Sie den Link auswählen, wird ein Verlauf der Bereitstellungen für die Gruppe angezeigt.

![Letzte Bereitstellung](./media/resource-group-portal/last-deployment.png)

Wenn Sie eine Bereitstellung aus dem Verlauf auswählen, werden Details zu dieser Bereitstellung angezeigt. Bei jeder Bereitstellung von Ressourcen wird die verwendete Vorlage vom Resource Manager beibehalten. Sie können die tatsächliche Vorlage abrufen, die für die Bereitstellung verwendet wurde, indem Sie die Option **Vorlage anzeigen** wählen.

![Vorlage exportieren](./media/resource-group-portal/export-template.png)

Es wird die Vorlage angezeigt, die für die Bereitstellung verwendet wurde. Sie enthält alle von Ihnen definierten Parameter und Variablen.

![Vorlage anzeigen](./media/resource-group-portal/show-template.png)

Wie bereits erwähnt, muss dies nicht unbedingt eine vollständige Darstellung der Ressourcengruppe sein. Wenn Sie Ressourcen außerhalb dieser Bereitstellung hinzugefügt oder gelöscht haben, werden diese Aktionen nicht in der Vorlage widergespiegelt. Sie können die Vorlage, Parameterdatei und Skriptdateien wie im vorherigen Abschnitt dargestellt anzeigen. Außerdem können Sie die Vorlage wie im vorherigen Abschnitt gezeigt auch neu bereitstellen, herunterladen oder speichern.

## Verwalten einer Ressourcengruppe

Sie können alle Ressourcengruppen durchsuchen, indem Sie auf **Ressourcengruppen** klicken.

![Ressourcengruppen durchsuchen](./media/resource-group-portal/browse-groups.png)

Wenn Sie eine bestimmte Ressourcengruppe auswählen, wird ein Ressourcengruppen-Blatt mit Informationen zur jeweiligen Ressourcengruppe angezeigt. Darin ist auch eine Liste mit allen Ressourcen der Gruppe enthalten.

![Zusammenfassung der Ressourcengruppe](./media/resource-group-portal/group-summary.png)

Sie können dem Blatt mit der Ressourcengruppe weitere Graphen und Tabellen hinzufügen, indem Sie unterhalb der Zusammenfassung die Option **Abschnitt hinzufügen** wählen.

![Abschnitt hinzufügen](./media/resource-group-portal/add-section.png)

In einem Kachelkatalog können Sie die Informationen auswählen, die in das Blatt eingebunden werden sollen. Die Typen der angezeigten Kacheln werden je nach Ressourcentyp gefiltert. Wenn Sie eine andere Ressource auswählen, ändern sich die verfügbaren Kacheln.

![Abschnitt hinzufügen](./media/resource-group-portal/tile-gallery.png)

Ziehen Sie die gewünschte Kachel jeweils an eine der verfügbaren Positionen.

![Kachel ziehen](./media/resource-group-portal/drag-tile.png)

Nachdem Sie oben im Portal **Fertig** gewählt haben, ist die neue Sicht Teil des Blatts.

![Kachel anzeigen](./media/resource-group-portal/show-lens.png)

Um den schnellen Zugriff auf eine Ressourcengruppe zu ermöglichen, können Sie das Blatt in Ihrem Dashboard anheften.

![Ressourcengruppe anheften](./media/resource-group-portal/pin-group.png)

Sie können auch einen Abschnitt des Blatts an das Dashboard anheften, indem Sie auf die Auslassungspunkte (...) oberhalb des Abschnitts klicken. Außerdem können Sie die Größe des Abschnitts im Blatt anpassen oder ihn vollständig entfernen. Die folgende Abbildung veranschaulicht, wie Sie den Abschnitt „CPU und Arbeitsspeicher“ anheften, anpassen oder entfernen.

![Abschnitt anheften](./media/resource-group-portal/pin-cpu-section.png)

Nach dem Anheften des Abschnitts im Dashboard wird die Zusammenfassung im Dashboard angezeigt.

![Dashboard anzeigen](./media/resource-group-portal/view-startboard.png)

Wenn Sie diese auswählen, werden sofort weitere Details zu den Daten eingeblendet.

Da Sie mit Ressourcengruppen den Lebenszyklus aller darin enthaltenen Ressourcen verwalten können, werden beim Löschen einer Ressourcengruppe alle darin enthaltenen Ressourcen gelöscht. Sie können auch einzelne Ressourcen in einer Ressourcengruppe löschen. Gehen Sie beim Löschen einer Ressourcengruppe mit Bedacht vor, da Ressourcen in anderen Ressourcengruppen damit verknüpft sein können. Die verknüpften Ressourcen werden nicht gelöscht, aber sie funktionieren unter Umständen nicht wie erwartet.

![Gruppe löschen](./media/resource-group-portal/delete-group.png)

## Markieren von Ressourcen

Sie können Ressourcengruppen und Ressourcen Tags zuordnen, um sie logisch zu organisieren. Informationen zur Verwendung von Tags über das Portal finden Sie unter [Verwenden von Tags zum Organisieren von Azure-Ressourcen](../resource-group-using-tags.md).

## Bereitstellen der gespeicherten Vorlage

Wenn Sie eine Vorlage unter Ihrem Konto gespeichert haben, können Sie diese später anzeigen, indem Sie **Durchsuchen** und **Vorlagen** auswählen.

![Vorlagen durchsuchen](./media/resource-group-portal/browse-templates.png)

Ihre eigene Sammlung mit den Vorlagen wird angezeigt.

![Vorlagensammlung anzeigen](./media/resource-group-portal/show-template-collection.png)


## Bereitstellen einer benutzerdefinierten Vorlage

Wenn Sie eine Bereitstellung ausführen möchten, ohne eine der Vorlagen des Marketplace zu nutzen, können Sie eine angepasste Vorlage erstellen, mit der die Infrastruktur für Ihre Lösung definiert wird. Weitere Informationen zu Vorlagen finden Sie unter [Erstellen von Azure-Ressourcen-Manager-Vorlagen](../resource-group-authoring-templates.md).

Wählen Sie zum Bereitstellen einer benutzerdefinierten Vorlage über das Portal die Option **Neu**, und starten Sie eine Suche nach **Bereitstellungen von Vorlagen**, bis Sie sie aus den Optionen auswählen können.

![Vorlagenbereitstellung suchen](./media/resource-group-portal/search-template.png)

Wählen Sie **Vorlagenbereitstellung** aus den verfügbaren Ressourcen aus.

![Vorlagenbereitstellung auswählen](./media/resource-group-portal/select-template.png)

Nach dem Starten der Vorlagenbereitstellung können Sie die benutzerdefinierte Vorlage erstellen und Werte für die Bereitstellung festlegen.

![Vorlage erstellen](./media/resource-group-portal/show-custom-template.png)

Sie können auch eine bereits vorhandene Vorlage aus den [Azure-Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates/) auswählen. Diese Vorlagen sind ein Beitrag der Community. Sie decken viele häufig vorkommende Szenarien ab, und unter Umständen wurde bereits eine Vorlage für einen Fall hinzugefügt, der Ihrer Bereitstellung ähnelt. Sie können die Vorlagen nach Übereinstimmungen mit Ihrem Szenario durchsuchen.

![Schnellstartvorlage auswählen](./media/resource-group-portal/select-quickstart-template.png)

Nachdem Sie eine Vorlage ausgewählt haben, wird sie in den Editor geladen.

## Anzeigen Ihres Abonnements und der Kosten

Sie können Informationen zu Ihrem Abonnement und die zusammengefassten Kosten für alle Ihre Ressourcen anzeigen. Wählen Sie **Abonnements** und das Abonnement aus, das Sie anzeigen möchten. Möglicherweise steht nur ein Abonnement zur Auswahl.

![Abonnement](./media/resource-group-portal/select-subscription.png)

Im Blatt „Abonnement“ wird eine Verbrauchsrate angezeigt.

![Verbrauchsrate](./media/resource-group-portal/burn-rate.png)

Eine Aufschlüsselung der Kosten nach Ressourcentyp wird ebenfalls angezeigt.

![Ressourcenkosten](./media/resource-group-portal/cost-by-resource.png)

## Zugriffskontrolle für Azure-Dashboards

Der Zugriff auf die Informationen, die von den meisten Kacheln im Portal angezeigt werden, wird in Azure über die [rollenbasierte Zugriffssteuerung](../active-directory/role-based-access-control-configure.md) geregelt. Um Dashboards nahtlos in das Ökosystem zu integrieren, werden alle veröffentlichten Dashboards als Azure-Ressourcen implementiert. Aus Sicht der Zugriffssteuerung unterscheiden sich Dashboards nicht von einem virtuellen Computer oder Speicherkonto.

Beispiel: Angenommen, Sie verfügen über ein Azure-Abonnement, und verschiedenen Mitgliedern Ihres Teams wurden die Rollen **Besitzer**, **Mitwirkender** oder**Leser** für das Abonnement zugewiesen. Benutzer, die Besitzer oder Mitwirkende sind, können Dashboards im Abonnement auflisten, anzeigen, erstellen, ändern oder löschen. Benutzer, die als Leser festgelegt wurden, können Dashboards auflisten und anzeigen, aber sie können diese nicht ändern oder löschen. Benutzer mit Leserzugriff können lokale Änderungen an einem veröffentlichten Dashboard vornehmen (z.B. beim Behandeln eines Problems), aber sie haben nicht die Möglichkeit, diese Änderungen zurück auf den Server zu veröffentlichen. Diese Benutzer können eine private Kopie des Dashboards zur eigenen Nutzung erstellen.

Beachten Sie, dass die einzelnen Kacheln im Dashboard jeweils eigene Anforderungen an die Zugriffssteuerung durchsetzen. Diese basieren auf den Ressourcen, für die Daten angezeigt werden. Dies bedeutet, dass Sie ein Dashboard entwerfen können, das umfassender genutzt werden kann, während die Daten auf den einzelnen Kacheln trotzdem geschützt sind.

## Nächste Schritte

- Informationen zum Anzeigen von Überwachungsprotokollen finden Sie unter [Überwachen von Vorgängen mit dem Resource Manager](../resource-group-audit.md).
- Informationen zur Problembehandlung von Bereitstellungsfehlern finden Sie unter [Problembehandlung beim Bereitstellen von Ressourcengruppen mit dem Azure-Portal](../resource-manager-troubleshoot-deployments-portal.md).

<!---HONumber=AcomDC_0413_2016-->