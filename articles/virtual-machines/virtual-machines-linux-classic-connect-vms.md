<properties
	pageTitle="Verbinden von Linux-VMs in einem Clouddienst | Microsoft Azure"
	description="Verbinden von virtuellen Linux-Computern, die mit dem klassischen Bereitstellungsmodell erstellt wurden, mit einem Azure-Clouddienst oder einem virtuellen Netzwerk."
	services="virtual-machines-linux"
	documentationCenter=""
	authors="cynthn"
	manager="timlt"
	editor=""
	tags="azure-service-management"/>

<tags
	ms.service="virtual-machines-linux"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-linux"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/06/2016"
	ms.author="cynthn"/>

# Verbinden virtueller Linux-Computer, die mit dem klassischen Bereitstellungsmodell erstellt wurden, mit einem virtuellen Netzwerk oder einem Clouddienst

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Virtuelle Linux-Computer, die mit dem klassischen Bereitstellungsmodell erstellt werden, befinden sich immer in einem Clouddienst. Der Clouddienst fungiert als Container und stellt einen eindeutigen öffentlichen DNS-Namen, eine öffentliche IP-Adresse und einen Satz von Endpunkten bereit, damit auf den virtuellen Computer über das Internet zugegriffen werden kann. Der Clouddienst kann sich in einem virtuellen Netzwerk befinden, was jedoch nicht zwingend erforderlich ist. Sie können auch [virtuelle Windows-Computer mit einem virtuellen Netzwerk oder einem Clouddienst verbinden](virtual-machines-windows-classic-connect-vms.md).

Befindet sich ein Clouddienst nicht in einem virtuellen Netzwerk, handelt es sich um einen *eigenständigen* Clouddienst. Die virtuellen Computer in einem eigenständigen Clouddienst können mit anderen virtuellen Computern nur über deren öffentliche DNS-Namen kommunizieren. Dieser Datenverkehr läuft über das Internet. Wenn ein Clouddienst mit einem virtuellen Netzwerk verbunden ist, können die virtuellen Computer in diesem Clouddienst mit allen anderen virtuellen Computern in dem virtuellen Netzwerk kommunizieren, ohne dass Daten über das Internet gesendet werden.

Wenn Sie Ihre virtuellen Computer im gleichen eigenständigen Clouddienst platzieren, können Sie Lastenausgleich und Verfügbarkeitsgruppen immer noch nutzen. Weitere Informationen finden Sie unter [Lastenausgleich virtueller Computer](virtual-machines-linux-load-balance.md) und [Verwalten der Verfügbarkeit virtueller Computer](virtual-machines-linux-manage-availability.md). Sie können die virtuellen Computer jedoch nicht in Subnetzen organisieren oder einen eigenständigen Clouddienst mit Ihrem lokalen Netzwerk verbinden. Hier sehen Sie ein Beispiel:

[AZURE.INCLUDE [virtual-machines-common-classic-connect-vms](../../includes/virtual-machines-common-classic-connect-vms.md)]

## Nächste Schritte

Nachdem Sie einen virtuellen Computer erstellt haben, empfiehlt es sich, [einen Datenträger hinzuzufügen](virtual-machines-linux-classic-attach-disk.md), damit ein Speicherort für die Daten der Dienste und Workloads verfügbar ist.

<!---HONumber=AcomDC_0706_2016-->