<properties
   pageTitle="Dashboards im Azure-Portal | Microsoft Azure"
   description="In diesem Artikel wird das Erstellen und Bearbeiten von Dashboards im Azure-Portal beschrieben."
   services="azure-portal"
   documentationCenter=""
   authors="sewatson"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="multiple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="09/06/2016"
   ms.author="sewatson"/>

# Erstellen und Freigeben von Dashboards im Azure-Portal

Sie können mehrere Dashboards erstellen und für andere Benutzer freigeben, die Zugriff auf Ihre Azure-Abonnements haben. In diesem Beitrag werden die Grundlagen der Erstellung/Bearbeitung, Veröffentlichung und Verwaltung des Zugriffs auf Dashboards beschrieben.

## Anpassung von Dashboards und Blättern

Seit dem Start der Dashboards vor einigen Monaten waren ein stetiger Rückgang von Blattanpassungen und eine rasante Zunahme von Dashboardanpassungen zu beobachten. Dieser starke Trend aus der Praxis weist darauf hin, dass Sie die Anpassung von Dashboards der Anpassung von Blättern vorziehen. Zur Unterstützung dieses Trends wird die Option zum Anpassen von Blättern eingestellt, und wir konzentrieren uns stattdessen auf die Verbesserung der Dashboardfunktionen. Wenn Sie ein Blatt angepasst haben, wird Ihre Anpassung in Kürze entfernt werden. Um die Anpassung beizubehalten, können Sie die angepassten Kacheln an ein Dashboard anheften. Klicken Sie einfach mit der rechten Maustaste auf die Kachel, und wählen Sie die Option **An Dashboard anheften**. Dies ist in der folgenden Abbildung dargestellt.

![Angepasste Kachel speichern](./media/azure-portal-dashboards/save-customization.png)

## Erstellen eines Dashboards

Wählen Sie zum Erstellen eines Dashboards die Schaltfläche **Neues Dashboard** neben dem Namen des aktuellen Dashboards aus.

![Dashboard erstellen](./media/azure-portal-dashboards/new-dashboard.png)

Bei dieser Aktion wird ein neues, leeres, privates Dashboard erstellt. Außerdem wird der Anpassungsmodus aktiviert, in dem Sie dem Dashboard einen Namen geben und Kacheln hinzufügen oder neu anordnen können. In diesem Modus ersetzt der ausblendbare Kachelkatalog das Navigationsmenü auf der linken Seite. Im Kachelkatalog können Sie auf verschiedene Arten nach Kacheln für Ihre Azure-Ressourcen suchen: Sie können nach [Ressourcengruppe](../resource-group-overview.md#resource-groups), Ressourcentyp, [Tag](../resource-group-using-tags.md) oder nach dem Namen der Ressource suchen.

![Dashboard anpassen](./media/azure-portal-dashboards/customize-dashboard.png)

Fügen Sie Kacheln hinzu, indem Sie sie an eine beliebige Position auf der Dashboardoberfläche ziehen und dort ablegen.

Es ist eine neue Kategorie mit dem Namen **Allgemein** für Kacheln vorhanden, die nicht einer bestimmten Ressource zugeordnet sind. In diesem Beispiel heften wir die Kachel „Markdown“ an. Sie verwenden diese Kachel, um dem Dashboard benutzerdefinierte Inhalte hinzuzufügen. Die Kachel unterstützt Klartext, [Markdown-Syntax](https://daringfireball.net/projects/markdown/syntax) und eine begrenzte Gruppe von HTML-Elementen. (Aus Sicherheitsgründen ist es nicht möglich, `<script>`-Tags zu injizieren oder bestimmte Formatelemente von CSS zu verwenden, die unter Umständen die Funktionsweise des Portals beeinträchtigen.)

![Markdown hinzufügen](./media/azure-portal-dashboards/add-markdown.png)

## Bearbeiten eines Dashboards

Nach der Erstellung des Dashboards können Sie Kacheln aus dem Kachelkatalog oder der Kacheldarstellung von Blättern anheften. Hier heften wir die Darstellung unserer Ressourcengruppe an. Sie können das Anheften entweder beim Durchsuchen des Elements oder über das Ressourcengruppenblatt durchführen. Beide Ansätze führen zum Anheften der Kacheldarstellung der Ressourcengruppe.

![An Dashboard anheften](./media/azure-portal-dashboards/pin-to-dashboard.png)

Nach dem Anheften des Elements wird es in Ihrem Dashboard angezeigt.

![Dashboard anzeigen](./media/azure-portal-dashboards/view-dashboard.png)

Nachdem wir nun die Kachel „Markdown“ und eine Ressourcengruppe im Dashboard angeheftet haben, können wir die Größe der Kacheln anpassen und die Kacheln wie gewünscht anordnen.

Wenn Sie mit der Maus auf „…“ zeigen oder mit der rechten Maustaste auf eine Kachel klicken, werden alle Kontextbefehle für die jeweilige Kachel angezeigt. Standardmäßig sind zwei Elemente vorhanden:

1. **Von Dashboard lösen**: Entfernt die Kachel aus dem Dashboard.
2.	**Anpassen**: Aktiviert den Anpassungsmodus.

![Kachel anpassen](./media/azure-portal-dashboards/customize-tile.png)

Wenn Sie „Anpassen“ auswählen, können Sie die Größe von Kacheln ändern und diese neu anordnen. Wählen Sie zum Ändern der Größe einer Kachel die neue Größe im Kontextmenü aus. Dies ist in der folgenden Abbildung dargestellt.

![Größe der Kachel ändern](./media/azure-portal-dashboards/resize-tile.png)

Falls für die Kachel alle Größen unterstützt werden, können Sie durch Ziehen an der unteren rechten Ecke die gewünschte Größe festlegen.

![Größe der Kachel ändern](./media/azure-portal-dashboards/resize-corner.png)

Sehen Sie sich nach dem Ändern der Größe von Kacheln das Dashboard an.

![Kachel anzeigen](./media/azure-portal-dashboards/view-tile.png)

Wählen Sie nach dem Anpassen eines Dashboards einfach die Option **Anpassung abgeschlossen**, um den Anpassungsmodus zu beenden. Sie können auch mit der rechten Maustaste klicken und im Kontextmenü die Option **Anpassung abgeschlossen** wählen.

## Veröffentlichen eines Dashboards und Verwalten der Zugriffssteuerung

Wenn Sie ein Dashboard erstellen, ist es standardmäßig ein privates Dashboard. Dies bedeutet, dass Sie die einzige Person sind, die es anzeigen kann. Verwenden Sie die Schaltfläche **Freigeben**, die neben den anderen Dashboardbefehlen angezeigt wird, um es auch für andere Benutzer sichtbar zu machen.

![Dashboard freigeben](./media/azure-portal-dashboards/share-dashboard.png)

Sie werden aufgefordert, für die Veröffentlichung des Dashboards ein Abonnement und eine Ressourcengruppe auszuwählen. Damit Dashboards nahtlos in das Ökosystem integriert werden können, haben wir freigegebene Dashboards als Azure-Ressourcen implementiert (das Freigeben durch die Eingabe einer E-Mail-Adresse ist also nicht möglich). Der Zugriff auf die Informationen, die von den meisten Kacheln im Portal angezeigt werden, wird in Azure über die [rollenbasierte Zugriffssteuerung](../active-directory/role-based-access-control-configure.md) geregelt. Aus Sicht der Zugriffssteuerung unterscheiden sich freigegebene Dashboards nicht von einem virtuellen Computer oder Speicherkonto.

Angenommen, Sie verfügen über ein Azure-Abonnement, und Mitgliedern Ihres Teams wurden die Rollen **Besitzer**, **Mitwirkender** oder **Leser** für das Abonnement zugewiesen. Benutzer, die Besitzer oder Mitwirkende sind, können Dashboards im Abonnement auflisten, anzeigen, erstellen, ändern oder löschen. Benutzer, die als Leser festgelegt wurden, können Dashboards auflisten und anzeigen, aber sie können diese nicht ändern oder löschen. Benutzer mit Lesezugriff können lokale Änderungen an einem freigegebenen Dashboard vornehmen, haben aber nicht die Möglichkeit, diese Änderungen zur Veröffentlichung zurück an den Server zu übergeben. Sie können aber eine private Kopie des Dashboards zur eigenen Verwendung erstellen. Einzelne Kacheln im Dashboard erzwingen basierend auf den jeweiligen Ressourcen wie immer eigene Regeln für die Zugriffssteuerung.

Während der Veröffentlichung über das Portal werden Sie der Einfachheit halber zu einem Muster geführt, bei dem Sie Dashboards in einer Ressourcengruppe mit dem Namen **Dashboards** anordnen.

![Dashboard veröffentlichen](./media/azure-portal-dashboards/publish-dashboard.png)

Sie können sich auch dafür entscheiden, ein Dashboard unter einer bestimmten Ressourcengruppe zu veröffentlichen. Die Zugriffssteuerung für dieses Dashboard stimmt mit der Zugriffssteuerung für die Ressourcengruppe überein. Benutzer, die die Ressourcen in dieser Ressourcengruppe verwalten können, haben auch Zugriff auf die Dashboards.

![Dashboard in Ressourcengruppe veröffentlichen](./media/azure-portal-dashboards/publish-to-resource-group.png)

Nach der Veröffentlichung des Dashboards wird der Steuerungsbereich für **Freigabe und Zugriff** aktualisiert, und es werden Informationen zum veröffentlichten Dashboard angezeigt, z.B. ein Link zur Verwaltung des Benutzerzugriffs auf das Dashboard. Mit diesem Link wird das Standardblatt für die rollenbasierte Zugriffssteuerung gestartet, über das der Zugriff für alle Azure-Ressourcen verwaltet wird. Sie können auf diese Ansicht immer zugreifen, indem Sie die Option **Freigeben** wählen.

![Zugriffssteuerung verwalten](./media/azure-portal-dashboards/manage-access.png)

## Nächste Schritte

- Informationen zum Verwalten von Ressourcen finden Sie unter [Verwalten von Azure-Ressourcen über das Portal](resource-group-portal.md).
- Informationen zum Bereitstellen von Ressourcen finden Sie unter [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure-Portal](../resource-group-template-deploy-portal.md).

<!---HONumber=AcomDC_0907_2016-->