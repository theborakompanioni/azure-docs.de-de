<properties
	pageTitle="Richtlinien für Ressourcengruppen | Microsoft Azure"
	description="Erfahren Sie mehr über die wichtigsten Entwurfs- und Implementierungsrichtlinien für die Bereitstellung von Ressourcengruppen in Azure-Infrastrukturdiensten."
	documentationCenter=""
	services="virtual-machines-windows"
	authors="iainfoulds"
	manager="timlt"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/30/2016"
	ms.author="iainfou"/>

# Richtlinien für Azure-Ressourcengruppen

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

In diesem Artikel erfahren Sie, wie Sie Ihre Umgebung logisch aufbauen und alle Komponenten in Ressourcengruppen zusammenfassen.


## Implementierungsrichtlinien für Ressourcengruppen

Entscheidungen:

- Werden Sie Ressourcengruppen anhand der zentralen Infrastrukturkomponenten oder für eine vollständige Anwendungsbereitstellung aufbauen?
- Müssen Sie den Zugriff auf Ressourcengruppen mithilfe der rollenbasierten Zugriffssteuerung beschränken?

Aufgaben:

- Legen Sie fest, welche zentralen Infrastrukturkomponenten Sie benötigen und ob Sie dedizierte Ressourcengruppen verwenden.
- Prüfen Sie, wie Sie Resource Manager-Vorlagen für konsistente, reproduzierbare Bereitstellungen implementieren können.
- Legen Sie fest, welche Benutzerzugriffsrollen Sie zur Steuerung des Zugriffs auf Ressourcengruppen benötigen.
- Erstellen Sie den Satz von Ressourcengruppen anhand Ihrer Benennungskonvention. Sie können Azure PowerShell oder das Portal verwenden.


## Ressourcengruppen

In Azure können Sie zusammengehörige Ressourcen wie Speicherkonten, virtuelle Netzwerke und virtuelle Computer (VMs) logisch zusammenfassen, um sie als eine Einheit bereitzustellen, zu verwalten und zu warten. Auf diese Weise können Sie Anwendungen leichter bereitstellen und gleichzeitig alle zugehörigen Ressourcen verwaltungstechnisch zusammenhalten oder anderen Benutzern Zugriff auf die Ressourcengruppe gewähren. Ausführlichere Informationen zu Ressourcengruppen finden Sie in der [Übersicht über den Azure Resource Manager](../resource-group-overview.md).

Ein wichtiges Feature von Ressourcengruppen ist die Möglichkeit, zum Erstellen Ihrer Umgebung mithilfe von Vorlagen. Eine Vorlage ist einfach eine JSON-Datei, die Speicher-, Netzwerk- und Computeressourcen ebenso deklariert wie sämtliche anzuwendende zugehörige benutzerdefinierte Skripts oder Konfigurationen. Mithilfe dieser Vorlagen können Sie konsistente, reproduzierbare Bereitstellungen für Ihre Anwendungen erstellen. Dies erleichtert die Erstellung einer Umgebung für die Entwicklung und die anschließende Verwendung derselben Vorlage für die Erstellung einer Produktionsbereitstellung, oder umgekehrt. Um die Verwendung von Vorlagen besser zu verstehen, können Sie die [exemplarische Vorgehensweise zu Vorlagen](../resource-manager-template-walkthrough.md) lesen, die Sie schrittweise durch die Erstellung einer Vorlage führt.

Sie können beim Entwerfen Ihrer Umgebung mit Ressourcengruppen zwischen zwei verschiedenen Ansätzen wählen:

- Ressourcengruppen für jede Anwendungsbereitstellung, die Speicherkonten, virtuelle Netzwerke und Subnetze, virtuelle Computer, Lastenausgleichsmodule usw. kombinieren.
- Zentralisierte Ressourcengruppen, die Ihre wesentlichen virtuellen Netzwerke und Subnetze oder Speicherkonten enthalten. Hierbei befinden sich Ihre Anwendungen in ihren eigenen Ressourcengruppen, die ausschließlich virtuelle Computer, Lastenausgleichsmodule, Netzwerkschnittstellen usw. enthalten.

Wenn Sie Ihre Umgebung horizontal hochskalieren möchten, vereinfachen zentralisierte Ressourcengruppen für Ihre virtuellen Netzwerke und Subnetze die Erstellung von standortübergreifenden Netzwerkverbindungen für hybride Verbindungsoptionen. Im Gegensatz dazu ist der Konfigurations-und Wartungsaufwand wesentlich höher, wenn jede einzelne Anwendung über ein eigenes virtuelles Netzwerk verfügt. [Rollenbasierte Zugriffssteuerungen](../active-directory/role-based-access-control-what-is.md) bieten eine differenzierte Möglichkeit, den Zugriff auf Ressourcengruppen zu steuern. Bei Produktionsanwendungen können Sie steuern, welche Benutzer auf diese Ressourcen zugreifen dürfen. Für die zentralen Infrastrukturressourcen können Sie festlegen, dass ausschließlich Infrastrukturtechniker mit Infrastrukturressourcen arbeiten dürfen. Ihre Anwendungsbesitzer haben nur Zugriff auf die Anwendungskomponenten innerhalb ihrer Ressourcengruppe, nicht aber auf die zentrale Azure-Infrastruktur Ihrer Umgebung. Berücksichtigen Sie beim Entwerfen Ihrer Umgebung die Benutzer, die auf die Ressourcen zugreifen müssen, und entwerfen Sie Ihre Ressourcengruppen entsprechend.


## Nächste Schritte

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]

<!---HONumber=AcomDC_0706_2016-->