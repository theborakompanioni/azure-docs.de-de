<properties
	pageTitle="Problembehandlung für Zuordnungsfehler bei virtuellen Windows-Computern | Microsoft Azure"
	description="Problembehandlung für Zuordnungsfehler beim Erstellen, Neustarten oder Ändern der Größe eines virtuellen Windows-Computers in Azure"
	services="virtual-machines-windows, azure-resource-manager"
	documentationCenter=""
	authors="jiangchen79"
	manager="felixwu"
	editor=""
	tags="top-support-issue,azure-resource-manager,azure-service-management"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="na"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="02/02/2016"
	ms.author="cjiang"/>

# Problembehandlung für Zuordnungsfehler beim Erstellen, Neustarten oder Ändern der Größe von virtuellen Windows-Computern in Azure

Wenn Sie einen virtuellen Computer erstellen, beendete virtuelle Computer (zuordnungsaufgehobene) neu starten oder die Größe eines virtuellen Computers ändern, weist Microsoft Azure Ihrem Abonnement Compute-Ressourcen zu. Unter Umständen erhalten Sie beim Ausführen dieser Schritte auch dann gelegentlich Fehler, wenn Sie die Grenzwerte des Azure-Abonnements noch nicht erreicht haben. In diesem Artikel werden die Ursachen einiger häufig auftretender Zuordnungsfehler erläutert und mögliche Abhilfemaßnahmen vorgeschlagen. Diese Informationen können auch hilfreich sein, wenn Sie die Bereitstellung Ihrer Dienste planen.

Im Abschnitt „Allgemeine Schritte zur Problembehandlung“ finden Sie Schritte für häufig auftretende Probleme. Der Abschnitt „Ausführliche Problembehandlungsschritte“ enthält Lösungsschritte für spezifische Fehlermeldungen. Bevor Sie beginnen, lesen Sie die Hintergrundinformationen zur Funktionsweise der Zuordnung und zu den Ursachen von Zuordnungsfehlern. Sie können auch [Zuordnungsfehler beim Erstellen, Neustarten oder Skalieren der Größe von Linux-VMs in Azure beheben](virtual-machines-linux-allocation-failure.md).

[AZURE.INCLUDE [virtual-machines-common-allocation-failure](../../includes/virtual-machines-common-allocation-failure.md)]

<!---HONumber=AcomDC_0330_2016-->