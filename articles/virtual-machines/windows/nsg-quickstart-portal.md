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
ms.date: 08/21/2017
ms.author: iainfou
ms.translationtype: HT
ms.sourcegitcommit: cf381b43b174a104e5709ff7ce27d248a0dfdbea
ms.openlocfilehash: 33bc0be0aeae6d0276fd8999b9ac0a010e3067ba
ms.contentlocale: de-de
ms.lasthandoff: 08/23/2017

---
# <a name="how-to-open-ports-to-a-virtual-machine-with-the-azure-portal"></a>Öffnen von Ports zu einem virtuellen Computer mit dem Azure-Portal
[!INCLUDE [virtual-machines-common-nsg-quickstart](../../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Schnellbefehle
Sie können [diese Schritte auch über Azure PowerShell ausführen](nsg-quickstart-powershell.md).

Erstellen Sie zuerst Ihre Netzwerksicherheitsgruppe. Wählen Sie im Portal eine Ressourcengruppe aus, wählen Sie **Hinzufügen**, suchen Sie nach **Netzwerksicherheitsgruppe**, und wählen Sie diese Option aus:

![Hinzufügen einer Netzwerksicherheitsgruppe](./media/nsg-quickstart-portal/add-nsg.png)

Geben Sie einen Namen für die Netzwerksicherheitsgruppe ein, wählen oder erstellen Sie eine Ressourcengruppe, und wählen Sie einen Speicherort aus: Wenn Sie fertig sind, klicken Sie auf **Erstellen**:

![Erstellen einer Netzwerksicherheitsgruppe](./media/nsg-quickstart-portal/create-nsg.png)

Wählen Sie die neue Netzwerksicherheitsgruppe aus. Wählen Sie „Eingangssicherheitsregeln“, und wählen Sie dann zum Erstellen einer Regel die Schaltfläche **Hinzufügen** aus:

![Hinzufügen einer eingehenden Regel](./media/nsg-quickstart-portal/add-inbound-rule.png)

Wählen Sie im Dropdownmenü einen allgemeinen **Dienst**, z.B. *HTTP*, aus. Sie können auch *Benutzerdefiniert* auswählen, um einen bestimmten zu verwendenden Port anzugeben. Sie können die Priorität oder den Namen ggf. ändern. Die Priorität wirkt sich auf die Reihenfolge aus, in der Regeln angewendet werden – je niedriger der numerische Wert, desto früher wird die Regel angewendet. Sie können auch am oberen Rand dieses Bildschirms **Erweitert** auswählen, um z.B. einen bestimmten Quell-IP-Block oder Portbereich einzugeben. Wenn Sie fertig sind, wählen Sie **OK** aus, um die Regel zu erstellen:

![Erstellen einer eingehenden Regel](./media/nsg-quickstart-portal/create-inbound-rule.png)

Im letzten Schritt muss die Netzwerksicherheitsgruppe einem Subnetz oder einer bestimmten Netzwerkschnittstelle zugeordnet werden. So ordnen Sie die Netzwerksicherheitsgruppe einem Subnetz zu: Wählen Sie **Subnetze** und dann **Zuordnen** aus:

![Zuordnen einer Netzwerksicherheitsgruppe zu einem Subnetz](./media/nsg-quickstart-portal/associate-subnet.png)

Wählen Sie das virtuelle Netzwerk und dann das gewünschte Subnetz aus:

![Zuordnen einer Netzwerksicherheitsgruppe zu einem virtuellen Netzwerk](./media/nsg-quickstart-portal/select-vnet-subnet.png)

Sie haben nun eine Netzwerksicherheitsgruppe sowie eine eingehende Regel erstellt, die Datenverkehr über Port 80 ermöglicht, und haben diese einem Subnetz zugeordnet. Alle virtuellen Computer, die Sie mit diesem Subnetz verbinden, sind über Port 80 erreichbar.

## <a name="more-information-on-network-security-groups"></a>Weitere Informationen zu Netzwerksicherheitsgruppen
Mit diesen Schnellbefehlen können Sie den Datenverkehr zu Ihrem virtuellen Computer einrichten. Netzwerksicherheitsgruppen bieten eine Vielzahl erstklassiger Funktionen sowie eine differenzierte Steuerung des Ressourcenzugriffs. Weitere Informationen über die [Erstellung von Netzwerksicherheitsgruppen und ACL-Regeln erhalten Sie hier](../../virtual-network/virtual-networks-create-nsg-arm-ps.md).

Bei hoch verfügbaren Webanwendungen sollten Sie die virtuellen Computer hinter einem Azure Load Balancer anordnen. Der Load Balancer verteilt den Datenverkehr auf virtuelle Computer, mit einer Netzwerksicherheitsgruppe, die den Datenverkehr filtert. Weitere Informationen finden Sie unter [Lastenausgleich für virtuelle Linux-Computer in Azure zum Erstellen einer hoch verfügbaren Anwendung](tutorial-load-balancer.md).

## <a name="next-steps"></a>Nächste Schritte
In diesem Beispiel haben Sie eine einfache Regel erstellt, die HTTP-Datenverkehr zulässt. Informationen zum Erstellen von detaillierteren Umgebungen finden Sie in den folgenden Artikeln:

* [Übersicht über den Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md)
* [Was ist eine Netzwerksicherheitsgruppe (NSG)?](../../virtual-network/virtual-networks-nsg.md)
