<properties
	pageTitle="Geplante Wartung für virtuelle Windows-Computer | Microsoft Azure"
	description="Grundlagen zur geplanten Wartung in Azure – was sie ist und wie sie sich auf die Verfügbarkeit Ihrer virtuellen in Azure ausgeführten Windows-Computer auswirkt."
	services="virtual-machines-windows"
	documentationCenter=""
	authors="drewm"
	manager="timlt"
	editor=""
	tags="azure-service-management,azure-resource-manager"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="01/05/2016"
	ms.author="drewm"/>

# Geplante Wartung für virtuelle Windows-Computer in Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Grundlagen zur geplanten Wartung in Azure – was sie ist und wie sie sich auf die Verfügbarkeit Ihrer virtuellen Windows-Computer auswirken kann. Informationen zur geplanten Wartung für virtuelle Linux-Computer finden Sie unter [planned maintenance for Linux virtual machines](virtual-machines-linux-planned-maintenance.md) (Geplante Wartung für virtuelle Linux-Computer).

## Gründe für das Ausführen einer geplanten Wartung durch Azure

Microsoft Azure führt regelmäßig weltweit Updates aus, um die Zuverlässigkeit, Leistung und Sicherheit der Hostinfrastruktur zu verbessern, die virtuellen Computern unterliegt. Viele dieser Updates werden ohne Beeinträchtigung der virtuellen Computer oder Clouddienste ausgeführt. Dazu zählen auch speichererhaltende Updates.

Bei bestimmten Updates ist allerdings ein Neustart der virtuellen Computer erforderlich, damit die erforderlichen Aktualisierungen auf die Infrastruktur angewendet werden können. Die virtuellen Computer werden heruntergefahren, während der Patch für die Infrastruktur vorgenommen wird. Danach werden die virtuellen Computer neu gestartet.

Beachten Sie, dass es zwei Wartungsarten gibt, die die Verfügbarkeit Ihrer virtuellen Computer beeinträchtigen können: geplante und ungeplante. Diese Seite beschreibt, wie Microsoft Azure die geplante Wartung ausführt. Weitere Informationen über ungeplante Wartung finden Sie unter [Unterschied zwischen geplanter und ungeplanter Wartung](virtual-machines-windows-manage-availability.md).

[AZURE.INCLUDE [virtual-machines-common-planned-maintenance](../../includes/virtual-machines-common-planned-maintenance.md)]

<!---HONumber=AcomDC_0330_2016-->