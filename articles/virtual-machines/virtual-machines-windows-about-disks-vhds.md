<properties
	pageTitle="Informationen zu Datenträgern und VHDs für virtuelle Windows-Computer | Microsoft Azure"
	description="Machen Sie sich mit den Grundlagen zu Datenträgern und VHDs für virtuelle Windows-Computer in Azure vertraut."
	services="virtual-machines-windows"
	documentationCenter=""
	authors="cynthn"
	manager="timlt"
	editor="tysonn"
	tags="azure-resource-manager,azure-service-management"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/16/2016"
	ms.author="cynthn"/>

# Informationen zu Datenträgern und VHDs für virtuelle Azure-Computer

Virtuelle Computer in Azure verwenden wie alle anderen Computer auch einen Datenträger, auf dem das Betriebssystem, Anwendungen und Daten gespeichert sind. Alle virtuellen Azure-Computer verfügen über mindestens zwei Datenträger – einen Datenträger mit dem Windows-Betriebssystem (im Fall eines virtuellen Windows-Computers) und einen temporären Datenträger. Der Betriebssystem-Datenträger wird aus einem Image erstellt. Sowohl der Betriebssystem-Datenträger als auch das Image sind virtuelle Festplatten (VHDs), die im Azure-Speicherkonto gespeichert sind. Virtuelle Computer können auch über einen oder mehrere Datenträger verfügen, die ebenfalls als VHDs gespeichert werden. Dieser Artikel ist auch für [virtuelle Linux-Computer](virtual-machines-linux-about-disks-vhds.md) verfügbar.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

[AZURE.INCLUDE [virtual-machines-common-about-disks-vhds](../../includes/virtual-machines-common-about-disks-vhds.md)]

## Nächste Schritte
-  [Erfassen eines virtuellen Windows-Computers](virtual-machines-windows-capture-image.md) zum horizontalen Hochskalieren der VM-Bereitstellung
-  [Hochladen eines Windows-VM-Images in Azure](virtual-machines-windows-upload-image.md), das beim Erstellen eines neuen virtuellen Computers verwendet wird
-  [Ändern des Laufwerkbuchstabens des temporären Windows-Datenträgers](virtual-machines-windows-classic-change-drive-letter.md), damit die Anwendung das Laufwerk „D:“ für Daten verwenden kann

<!---HONumber=AcomDC_0622_2016-->