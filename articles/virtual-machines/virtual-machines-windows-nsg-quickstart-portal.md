---
title: "Öffnen von Ports für einen virtuellen Computer mithilfe des Azure-Portals | Microsoft Docs"
description: "Erfahren Sie, wie Sie mit dem Resource Manager-Bereitstellungsmodell im Azure-Portal für Ihren virtuellen Windows-Computer einen Port öffnen oder einen Endpunkt erstellen."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: f7cf0319-5ee7-435e-8f94-c484bf5ee6f1
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 02/09/2017
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 5919c477502767a32c535ace4ae4e9dffae4f44b
ms.openlocfilehash: 979260cdb1ce7f3eaa4b7e05be4c34fce06d0399


---
# <a name="opening-ports-to-a-vm-in-azure-using-the-azure-portal"></a>Öffnen von Ports für einen virtuellen Computer in Azure mithilfe des Azure-Portals
[!INCLUDE [virtual-machines-common-nsg-quickstart](../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Schnellbefehle
Sie können [diese Schritte auch über Azure PowerShell ausführen](virtual-machines-windows-nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Erstellen Sie zuerst Ihre Netzwerksicherheitsgruppe. Wählen Sie im Portal eine Ressourcengruppe aus, klicken Sie auf **Hinzufügen**, suchen Sie nach „Netzwerksicherheitsgruppe“, und wählen Sie diese Option aus:

![Hinzufügen einer Netzwerksicherheitsgruppe](./media/virtual-machines-windows-nsg-quickstart-portal/add-nsg.png)

Geben Sie einen Namen für die Netzwerksicherheitsgruppe ein, wählen oder erstellen Sie eine Ressourcengruppe, und wählen Sie einen Speicherort aus: Wenn Sie fertig sind, klicken Sie auf **Erstellen**.

![Erstellen einer Netzwerksicherheitsgruppe](./media/virtual-machines-windows-nsg-quickstart-portal/create-nsg.png)

Wählen Sie die neue Netzwerksicherheitsgruppe aus. Wählen Sie „Inbound security rules“ (Eingehende Sicherheitsregeln), und klicken Sie dann zum Erstellen einer Regel auf die Schaltfläche **Hinzufügen**:

![Hinzufügen einer eingehenden Regel](./media/virtual-machines-windows-nsg-quickstart-portal/add-inbound-rule.png)

Geben Sie einen Namen für die neue Regel an. Standardmäßig ist Port 80 bereits eingetragen. Auf diesem Blatt können beim Hinzufügen von zusätzlichen Regeln zur Netzwerksicherheitsgruppe die Quelle, das Protokoll und das Ziel geändert werden. Klicken Sie auf **OK** , um die Regel zu erstellen:

![Erstellen einer eingehenden Regel](./media/virtual-machines-windows-nsg-quickstart-portal/create-inbound-rule.png)

Im letzten Schritt muss die Netzwerksicherheitsgruppe einem Subnetz oder einer bestimmten Netzwerkschnittstelle zugeordnet werden. So ordnen Sie die Netzwerksicherheitsgruppe einem Subnetz zu: Wählen Sie „Subnetze“, und klicken Sie dann auf **Zuordnen**:

![Zuordnen einer Netzwerksicherheitsgruppe zu einem Subnetz](./media/virtual-machines-windows-nsg-quickstart-portal/associate-subnet.png)

Wählen Sie das virtuelle Netzwerk und dann das gewünschte Subnetz aus:

![Zuordnen einer Netzwerksicherheitsgruppe zu einem virtuellen Netzwerk](./media/virtual-machines-windows-nsg-quickstart-portal/select-vnet-subnet.png)

Sie haben nun eine Netzwerksicherheitsgruppe sowie eine eingehende Regel erstellt, die Datenverkehr über Port 80 ermöglicht, und haben diese einem Subnetz zugeordnet. Alle virtuellen Computer, die Sie mit diesem Subnetz verbinden, sind über Port 80 erreichbar.

## <a name="more-information-on-network-security-groups"></a>Weitere Informationen zu Netzwerksicherheitsgruppen
Mit diesen Schnellbefehlen können Sie den Datenverkehr zu Ihrem virtuellen Computer einrichten. Netzwerksicherheitsgruppen bieten eine Vielzahl erstklassiger Funktionen sowie eine differenzierte Steuerung des Ressourcenzugriffs. Weitere Informationen über die [Erstellung von Netzwerksicherheitsgruppen und ACL-Regeln erhalten Sie hier](../virtual-network/virtual-networks-create-nsg-arm-ps.md).

Sie können Netzwerksicherheitsgruppen und ACL-Regeln auch als Teil von Azure Resource Manager-Vorlagen definieren. Erfahren Sie mehr über [das Erstellen von Netzwerksicherheitsgruppen mit Vorlagen](../virtual-network/virtual-networks-create-nsg-arm-template.md).

Wenn eine Portweiterleitung notwendig ist, um einem internen Port des virtuellen Computers einen eindeutigen externen Port zuzuweisen, verwenden Sie einen Lastenausgleich sowie NAT-Regeln (Network Address Translation, Netzwerkadressübersetzung). Sie können beispielsweise TCP-Port 8080 extern verfügbar machen und den Datenverkehr an TCP-Port 80 auf einem virtuellen Computer weiterleiten. Hier finden Sie weitere Informationen zum [Erstellen eines Load Balancers mit Internetzugriff](../load-balancer/load-balancer-get-started-internet-arm-ps.md).

## <a name="next-steps"></a>Nächste Schritte
In diesem Beispiel haben Sie eine einfache Regel erstellt, die HTTP-Datenverkehr zulässt. Informationen zum Erstellen von detaillierteren Umgebungen finden Sie in den folgenden Artikeln:

* [Übersicht über den Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)
* [Was ist eine Netzwerksicherheitsgruppe (NSG)?](../virtual-network/virtual-networks-nsg.md)
* [Übersicht über Azure Resource Manager für Load Balancer](../load-balancer/load-balancer-arm.md)




<!--HONumber=Nov16_HO3-->


