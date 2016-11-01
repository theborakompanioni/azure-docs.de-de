<properties
	pageTitle="Konfigurieren von Azure Marketplace-Image-Einstellungen in Azure DevTest Labs | Microsoft Azure"
	description="Konfigurieren Sie, welche Azure Marketplace-Images beim Erstellen eines virtuellen Computers in Azure DevTest Labs verwendet werden können."
	services="devtest-lab,virtual-machines"
	documentationCenter="na"
	authors="tomarcher"
	manager="douge"
	editor=""/>

<tags
	ms.service="devtest-lab"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/06/2016"
	ms.author="tarcher"/>

# Konfigurieren von Azure Marketplace-Image-Einstellungen in Azure DevTest Labs

DevTest Labs unterstützt die Erstellung von virtuellen Computern (VMs, Virtual Machines) auf Basis von Azure Marketplace-Images, abhängig davon, wie Sie Azure Marketplace-Images zur Verwendung in Ihrem Lab konfiguriert haben. In diesem Artikel erfahren Sie, wie Sie ggf. angeben, welche Azure Marketplace-Images zum Erstellen virtueller Computer in einem Lab verwendet werden können.

## Auswählen, welche Azure Marketplace-Images beim Erstellen eines virtuellen Computers zulässig sind

1. Melden Sie sich auf dem [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) an.

1. Wählen Sie **Weitere Dienste** und dann in der Liste **DevTest Labs**.

1. Wählen Sie in der Liste der Labs das gewünschte Lab aus.

1. Wählen Sie auf dem Blatt des Labs **Configuration** (Konfiguration) aus.
	
1. Wählen Sie auf dem Blatt **Configuration** (Konfiguration) die Option **Marketplace images** (Marketplace-Images) aus.

1. Geben Sie an, ob alle qualifizierten Azure Marketplace-Images für die Verwendung als Basis einer neuen VM verfügbar sein sollen. Bei Auswahl von **Ja** sind alle Azure Marketplace-Images im Lab zulässig, die alle folgenden Kriterien erfüllen:

	- Das Image erstellt einen einzelnen virtuellen Computer, **und**
	- Das Image verwendet den Azure-Resource-Manager zur Bereitstellung von VMs, **und**
	- Das Image erfordert nicht den Erwerb eines zusätzlichen Lizenzplans.
	
	Wenn keine Images zulässig sein sollen, oder Sie angeben möchten, welche Bilder verwendet werden können, wählen Sie **Nein**.
 
	![Option, die Verwendung aller Marketplace-Images als Basis-Images für virtuelle Computer zuzulassen](./media/devtest-lab-configure-marketplace-images/allow-all-marketplace-images.png)
 
1. Bei Auswahl von **Nein** im vorherigen Schritt ist das Kontrollkästchen **Allowed images/Select all** (Zulässige Images/Alle auswählen) aktiviert. Sie können diese Option zusammen mit dem Suchfeld verwenden, um schnell alle in der Liste angezeigten Elemente auszuwählen oder ihre Auswahl aufzuheben. Sie können auch die Azure Marketplace-Images einzeln auswählen, die Sie für die Erstellung des virtuellen Computers zulassen möchten, indem Sie das Kontrollkästchen jedes entsprechenden Images aktivieren. Wählen Sie nichts aus der Liste, wenn keine Azure Marketplace-Images im Lab verwendet werden sollen.

	![Sie können angeben, welche Azure Marketplace-Images als Basis-Images für virtuelle Computer verwendet werden können.](./media/devtest-lab-configure-marketplace-images/select-marketplace-images.png)

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## Nächste Schritte

Nachdem Sie konfiguriert haben, auf welche Weise Azure Marketplace-Images beim Erstellen eines virtuellen Computers zulässig sind, ist der nächste Schritt das [Hinzufügen eines virtuellen Computers zu Ihrem Lab](./devtest-lab-add-vm-with-artifacts.md).

<!---HONumber=AcomDC_0907_2016-->