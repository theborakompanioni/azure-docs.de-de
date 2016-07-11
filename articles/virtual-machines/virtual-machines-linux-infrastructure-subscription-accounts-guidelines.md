<properties
	pageTitle="Richtlinien für Abonnements und Konten | Microsoft Azure"
	description="Erfahren Sie mehr über die wichtigsten Entwurfs- und Implementierungsrichtlinien für Abonnements und Konten in Azure."
	documentationCenter=""
	services="virtual-machines-linux"
	authors="iainfoulds"
	manager="timlt"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-linux"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-linux"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/22/2016"
	ms.author="iainfou"/>

# Richtlinien für Abonnements und Konten

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

In diesem Artikel wird erläutert, wie Sie die Abonnement- und-Kontoverwaltung bewältigen können, wenn die Umgebung und die Benutzerbasis wachsen.


## Implementierungsrichtlinien für Konten und Abonnements

Entscheidungen:

- Welche Abonnementgruppen und Konten benötigen Sie, um Ihre IT-Workload oder -Infrastruktur zu hosten?
- Wie unterteilen Sie die Hierarchie passend für Ihre Organisation?

Aufgaben:

- Definieren Sie die logische Organisationshierarchie so, wie Sie sie über die Abonnementebene verwalten möchten.
- Definieren Sie die erforderlichen Konten und die Abonnements in jedem Konto entsprechend dieser logischen Hierarchie.
- Erstellen Sie die Gruppe von Abonnements und Konten mit Ihrer Benennungskonvention.


## Abonnements und Konten

Für die Arbeit mit Azure benötigen Sie mindestens ein Azure-Abonnement. Ressourcen, wie virtuelle Computer (VMs) oder virtuelle Netzwerke, sind in diesen Abonnements vorhanden.

- Unternehmenskunden verwenden i. d. R. eine Unternehmensanmeldung, die die oberste Ressource in der Hierarchie darstellt und einem oder mehreren Konten zugeordnet ist.
- Für Benutzer und Kunden ohne Unternehmensanmeldung ist die oberste Ressource das Konto.
- Abonnements sind Konten zugeordnet, und jedes Konto kann über mehrere Abonnements verfügen. Azure verzeichnet die Abrechnungsinformationen auf Abonnementebene.

Aufgrund der maximal zwei Hierarchieebenen für die Konto-/Abonnement-Beziehung ist es wichtig, die Benennungskonvention für Konten und Abonnements an die Abrechnungsanforderungen anzupassen. Wenn z. B. ein globales Unternehmen Azure verwendet, nutzt dieses pro Region möglicherweise ein Konto, und die Abonnements werden auf Regionsebene verwaltet.

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub01.png)

Sie können beispielsweise diese Struktur verwenden.

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub02.png)

Wenn sich diesem Beispiel folgend eine Region dazu entscheidet, mehr als ein Abonnement einer bestimmten Gruppe zuzuordnen, sollte die Benennungskonvention eine Methode enthalten, mit der das zusätzliche Abonnement entweder im Kontonamen oder im Abonnementnamen codiert wird. Diese Organisation ermöglicht Abrechnungsdaten, mit denen die neuen Hierarchieebenen bei den Abrechnungsberichten generiert werden.

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub03.png)

Die Organisation könnte folgendermaßen aussehen.

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub04.png)

Microsoft stellt ausführliche Abrechnungen in einer herunterladbaren Datei für einzelne Konten oder für alle Konten in einem Enterprise Agreement zur Verfügung. Sie können diese Datei z. B. mit Microsoft Excel verarbeiten. Dieser Prozess würde Daten erfassen, die Ressourcen, die mehr als eine Ebene der Hierarchie codieren, in separate Spalten partitionieren und mithilfe einer PivotTable oder PowerPivot dynamische Berichtsfunktionen bereitstellen.


## Nächste Schritte

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]

<!---HONumber=AcomDC_0629_2016-->