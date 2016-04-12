<properties
	pageTitle="Einrichten von Endpunkten auf einer klassischen Windows-VM | Microsoft Azure"
	description="Erfahren Sie, wie Endpunkte im klassischen Azure-Portal eingerichtet werden, um die Kommunikation mit einem virtuellen Windows-Computer in Azure zu ermöglichen."
	services="virtual-machines-windows"
	documentationCenter=""
	authors="cynthn"
	manager="timlt"
	editor=""
	tags="azure-service-management"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="01/06/2016"
	ms.author="cynthn"/>

# Gewusst wie: Einrichten von Endpunkten auf einem klassischen virtuellen Windows-Computer in Azure



Alle virtuellen Windows-Computer, die Sie mit dem klassischen Bereitstellungsmodell in Azure erstellen, können automatisch über einen privaten Netzwerkkanal mit anderen virtuellen Computern im gleichen Clouddienst oder virtuellen Netzwerk kommunizieren. Computer im Internet oder anderen virtuellen Netzwerken erfordern jedoch von den Endpunkten, dass sie den eingehenden Netzwerkdatenverkehr auf einen virtuellen Computer leiten. Informationen zu Endpunkten auf Linux-VMs finden Sie unter [Gewusst wie: Einrichten von Endpunkten auf einem klassischen virtuellen Linux-Computer in Azure](virtual-machines-linux-classic-setup-endpoints.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Ressourcen-Manager-Modell.

Beim Erstellen eines virtuellen Windows-Computers im klassischen Azure-Portal werden allgemeine Endpunkte, z.B. für Remotedesktop und Windows PowerShell-Remoting, in der Regel automatisch erstellt – je nachdem, welches Betriebssystem Sie auswählen. Sie können zusätzliche Endpunkte beim Erstellen des virtuellen Computers oder später nach Bedarf konfigurieren.



[AZURE.INCLUDE [virtual-machines-common-classic-setup-endpoints](../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## Nächste Schritte

* Ein Azure PowerShell-Cmdlet zum Einrichten eines VM-Endpunkts finden Sie unter [Add-AzureEndpoint](https://msdn.microsoft.com/library/azure/dn495300.aspx).

* Informationen dazu, wie Sie ein Azure PowerShell-Cmdlet zum Verwalten einer ACL für einen Endpunkt verwenden, finden Sie unter [Verwalten von Zugriffssteuerungslisten (ACLs) für Endpunkte mithilfe von PowerShell](../virtual-network/virtual-networks-acl-powershell.md).

* Wenn Sie einen virtuellen Computer im Resource Manager-Bereitstellungsmodell erstellt haben, können Sie auch Azure PowerShell zum [Erstellen eines Load Balancers mit Internetzugriff](../load-balancer/load-balancer-get-started-internet-arm-ps.md) verwenden.

<!---HONumber=AcomDC_0330_2016-->