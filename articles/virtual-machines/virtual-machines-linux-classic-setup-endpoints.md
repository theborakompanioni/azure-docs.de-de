---
title: Einrichten von Endpunkten auf einer klassischen Linux-VM | Microsoft Docs
description: Erfahren Sie, wie Endpunkte für virtuelle Linux-Computer im klassischen Azure-Portal eingerichtet werden, um die Kommunikation mit einem virtuellen Linux-Computer in Azure zu ermöglichen.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: timlt
editor: ''
tags: azure-service-management

ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/13/2016
ms.author: cynthn

---
# Gewusst wie: Einrichten von Endpunkten auf einem klassischen virtuellen Linux-Computer in Azure
Alle virtuellen Linux-Computer, die Sie mit dem klassischen Bereitstellungsmodell in Azure erstellen, können automatisch über einen privaten Netzwerkkanal mit anderen virtuellen Computern im gleichen Clouddienst oder virtuellen Netzwerk kommunizieren. Computer im Internet oder anderen virtuellen Netzwerken erfordern jedoch von den Endpunkten, dass sie den eingehenden Netzwerkdatenverkehr auf einen virtuellen Computer leiten. Dieser Artikel ist auch für [virtuelle Windows-Computer](virtual-machines-windows-classic-setup-endpoints.md) verfügbar.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Im **Resource Manager**-Bereitstellungsmodell werden Endpunkte mithilfe von **Netzwerksicherheitsgruppen (NSGs)** konfiguriert. Weitere Informationen finden Sie unter [Öffnen von Ports und Endpunkten](virtual-machines-linux-nsg-quickstart.md).

Wenn Sie einen virtuellen Linux-Computer im klassischen Azure-Portal erstellen, wird normalerweise automatisch ein Endpunkt für Secure Shell (SSH) erstellt. Sie können zusätzliche Endpunkte beim Erstellen des virtuellen Computers oder später nach Bedarf konfigurieren.

[!INCLUDE [virtual-machines-common-classic-setup-endpoints](../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## Nächste Schritte
* Sie können einen VM-Endpunkt auch über die [Azure-Befehlszeilenschnittstelle](../virtual-machines-command-line-tools.md) erstellen. Führen Sie den Befehl **azure vm endpoint create** aus.
* Wenn Sie einen virtuellen Computer im Resource Manager-Bereitstellungsmodell erstellt haben, können Sie die Azure-CLI im Resource Manager-Modus verwenden, um [Netzwerksicherheitsgruppen](../virtual-network/virtual-networks-create-nsg-arm-cli.md) zum Steuern des Datenverkehrs an den virtuellen Computer zu erstellen.

<!---HONumber=AcomDC_0713_2016-->