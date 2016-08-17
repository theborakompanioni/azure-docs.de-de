<properties
	pageTitle="Informationen zu Datenträgern und VHDs für virtuelle Linux-Computer | Microsoft Azure"
	description="Machen Sie sich mit den Grundlagen von Datenträgern und VHDs für virtuelle Linux-Computer vertraut."
	services="virtual-machines-linux"
	documentationCenter=""
	authors="cynthn"
	manager="timlt"
	editor="tysonn"
	tags="azure-resource-manager,azure-service-management"/>

<tags
	ms.service="virtual-machines-linux"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-linux"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/16/2016"
	ms.author="cynthn"/>

# Informationen zu Datenträgern und VHDs für virtuelle Azure-Computer

Virtuelle Computer in Azure verwenden wie alle anderen Computer auch einen Datenträger, auf dem das Betriebssystem, Anwendungen und Daten gespeichert sind. Alle virtuellen Azure-Computer verfügen über mindestens zwei Datenträger – einen Datenträger mit dem Linux-Betriebssystem (im Fall eines virtuellen Linux-Computers) und einen temporären Datenträger. Der Betriebssystem-Datenträger wird aus einem Image erstellt. Sowohl der Betriebssystem-Datenträger als auch das Image sind virtuelle Festplatten (VHDs), die im Azure-Speicherkonto gespeichert sind. Virtuelle Computer können auch über einen oder mehrere Datenträger verfügen, die ebenfalls als VHDs gespeichert werden. Dieser Artikel ist auch für [virtuelle Windows-Computer](virtual-machines-windows-about-disks-vhds.md) verfügbar.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

[AZURE.INCLUDE [virtual-machines-common-about-disks-vhds](../../includes/virtual-machines-common-about-disks-vhds.md)]

## Problembehandlung
[AZURE.INCLUDE [virtual-machines-linux-lunzero](../../includes/virtual-machines-linux-lunzero.md)]

## Nächste Schritte

-  [Anfügen eines Datenträgers](virtual-machines-linux-attach-disk-portal.md), um zusätzlichen Speicherplatz für den virtuellen Computer hinzuzufügen
-  [Konfigurieren von Software-RAID](virtual-machines-linux-configure-raid.md) für Redundanz
-  [Erfassen eines virtuellen Linux-Computers](virtual-machines-linux-classic-capture-image.md) für die schnelle Bereitstellung zusätzlicher virtueller Computer

<!---HONumber=AcomDC_0803_2016-->