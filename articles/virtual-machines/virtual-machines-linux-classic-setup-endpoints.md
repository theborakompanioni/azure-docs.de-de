<properties
	pageTitle="Einrichten von Endpunkten auf einer klassischen Linux-VM | Microsoft Azure"
	description="Erfahren Sie, wie Endpunkte im klassischen Azure-Portal eingerichtet werden, um die Kommunikation mit einem virtuellen Linux-Computer in Azure zu ermöglichen."
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
	ms.date="01/06/2016"
	ms.author="cynthn"/>

# Gewusst wie: Einrichten von Endpunkten auf einem klassischen virtuellen Computer in Azure

Alle virtuellen Linux-Computer, die Sie mit dem klassischen Bereitstellungsmodell in Azure erstellen, können automatisch über einen privaten Netzwerkkanal mit anderen virtuellen Computern im gleichen Clouddienst oder virtuellen Netzwerk kommunizieren. Computer im Internet oder anderen virtuellen Netzwerken erfordern jedoch von den Endpunkten, dass sie den eingehenden Netzwerkdatenverkehr auf einen virtuellen Computer leiten. Dieser Artikel ist auch für [virtuelle Windows-Computer](virtual-machines-windows-classic-setup-endpoints.md) verfügbar.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Ressourcen-Manager-Modell.

Wenn Sie einen virtuellen Linux-Computer im klassischen Azure-Portal erstellen, werden allgemeine Endpunkte, z.B. für Secure Shell (SSH), in der Regel automatisch erstellt. Sie können zusätzliche Endpunkte beim Erstellen des virtuellen Computers oder später nach Bedarf konfigurieren.
 

[AZURE.INCLUDE [virtual-machines-common-classic-setup-endpoints](../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## Nächste Schritte

* Sie können auch einen VM-Endpunkt erstellen, indem Sie die Azure-Befehlszeilenschnittstelle im [Dienstverwaltungsmodus](../virtual-machines-command-line-tools.md) verwenden. Führen Sie den Befehl **azure vm endpoint create** aus.

<!---HONumber=AcomDC_0413_2016-->