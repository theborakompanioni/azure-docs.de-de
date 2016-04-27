<properties
	pageTitle="Was sind Skalierungsgruppen für VMs? | Microsoft Azure"
	description="Erfahren Sie mehr über VM-Skalierungsgruppen."
	keywords="virtueller Linux-Computer, Skalierungsgruppen für virtuelle Computer" 
	services="virtual-machines-linux"
	documentationCenter=""
	authors="gatneil"
	manager="madhana"
	editor="tysonn"
	tags="azure-resource-manager" />

<tags
	ms.service="virtual-machine-linux"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-linux"
	ms.devlang="na"
	ms.topic="article"
	ms.date="03/24/2016"
	ms.author="gatneil"/>

# Was sind Skalierungsgruppen für virtuelle Computer?

Skalierungsgruppen virtueller Computer ermöglichen es Ihnen, mehrere VMs als Satz zu verwalten. Auf hoher Ebene haben Skalierungsgruppen die folgenden Vor- und Nachteile:

Vorteile:

1. Hohe Verfügbarkeit. Jede Skalierungsgruppe platziert ihre virtuellen Computer in eine Verfügbarkeitsgruppe mit fünf Fehlerdomänen (FD) und fünf Updatedomänen (UD), um die Verfügbarkeit sicherzustellen (weitere Informationen zu FDs und UDs finden Sie unter [VM-Verfügbarkeit](./virtual-machines-linux-manage-availability.md)). 
2. Einfache Integration mit dem Azure Load Balancer und App Gateway
3. Leichtere Integration mit Azure Autoscale
4. Vereinfachte Bereitstellung, Verwaltung und Bereinigung von virtuellen Computern
5. Unterstützung häufiger Windows- und Linux-Varianten sowie benutzerdefinierter Images

Nachteile:

1. Kann in einer Skalierungsgruppe keine Datenträger an VM-Instanzen anfügen. Muss stattdessen Azure Blob Storage, Azure Files Storage, Azure Table Storage oder andere Speicherlösungen verwenden.

## Schnellerfassung mithilfe der Azure-Befehlszeilenschnittstelle

[AZURE.INCLUDE [cli-vmss-quick-create](../../includes/virtual-machines-linux-cli-vmss-quick-create-include.md)]

## Nächste Schritte

Allgemeine Informationen finden Sie auf der [Hauptseite für Skalierungsgruppen](https://azure.microsoft.com/services/virtual-machine-scale-sets/).

Weitere Dokumente finden Sie auf der [Hauptdokumentationsseite für Skalierungsgruppen](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

Wenn Sie Beispiele für Resource Manager-Vorlagen ansehen möchten, die Skalierungsgruppen verwenden, suchen Sie im [Github-Repository für Azure-Schnellstartvorlagen](https://github.com/Azure/azure-quickstart-templates) nach „vmss“.

<!---HONumber=AcomDC_0413_2016-->