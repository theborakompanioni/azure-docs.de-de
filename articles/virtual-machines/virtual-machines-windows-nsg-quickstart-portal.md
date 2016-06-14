<properties
   pageTitle="Ermöglichen des externen Zugriffs auf einen virtuellen Computer über das Portal | Microsoft Azure"
   description="Erfahren Sie, wie Sie mit dem Resource Manager-Bereitstellungsmodell im Azure-Portal einen Port öffnen oder einen Endpunkt erstellen können, der einen Zugriff auf Ihren virtuellen Computer ermöglicht."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="05/24/2016"
   ms.author="iainfou"/>

# Ermöglichen des externen Zugriffs auf einen virtuellen Computer über das Azure-Portal
[AZURE.INCLUDE [virtual-machines-common-nsg-quickstart](../../includes/virtual-machines-common-nsg-quickstart.md)]

## Schnellbefehle
Sie können [diese Schritte auch über Azure PowerShell ausführen](virtual-machines-windows-nsg-quickstart-powershell.md).

Erstellen Sie zuerst Ihre Netzwerksicherheitsgruppe. Wählen Sie im Portal eine Ressourcengruppe aus, klicken Sie auf „Hinzufügen“, und suchen Sie dann nach einer bestimmten Netzwerksicherheitsgruppe:

![Hinzufügen einer Netzwerksicherheitsgruppe](./media/virtual-machines-windows-nsg-quickstart-portal/add-nsg.png)

Geben Sie einen Namen für die Netzwerksicherheitsgruppe ein, und wählen Sie einen Speicherort aus:

![Erstellen einer Netzwerksicherheitsgruppe](./media/virtual-machines-windows-nsg-quickstart-portal/create-nsg.png)

Wählen Sie die neue Netzwerksicherheitsgruppe aus. Nun erstellen Sie eine eingehende Regel:

![Hinzufügen einer eingehenden Regel](./media/virtual-machines-windows-nsg-quickstart-portal/add-inbound-rule.png)

Geben Sie einen Namen für die neue Regel an. Beachten Sie, dass standardmäßig Port 80 eingetragen ist. Hier können beim Hinzufügen von zusätzlichen Regeln zur Netzwerksicherheitsgruppe die Quelle, das Protokoll und das Ziel geändert werden.

![Erstellen einer eingehenden Regel](./media/virtual-machines-windows-nsg-quickstart-portal/create-inbound-rule.png)

Im letzten Schritt muss die Netzwerksicherheitsgruppe einem Subnetz oder einer bestimmten Netzwerkschnittstelle zugeordnet werden. So ordnen Sie die Netzwerksicherheitsgruppe einem Subnetz zu:

![Zuordnen einer Netzwerksicherheitsgruppe zu einem Subnetz](./media/virtual-machines-windows-nsg-quickstart-portal/associate-subnet.png)

Wählen Sie das virtuelle Netzwerk und dann das gewünschte Subnetz aus:

![Zuordnen einer Netzwerksicherheitsgruppe zu einem virtuellen Netzwerk](./media/virtual-machines-windows-nsg-quickstart-portal/select-vnet-subnet.png)

Sie haben nun eine Netzwerksicherheitsgruppe sowie eine eingehende Regel erstellt, die Datenverkehr über Port 80 ermöglicht, und haben diese einem Subnetz zugeordnet. Alle virtuellen Computer, die Sie mit diesem Subnetz verbinden, sind über Port 80 erreichbar.


## Weitere Informationen zu Netzwerksicherheitsgruppen
Mit diesen Schnellbefehlen können Sie den Datenverkehr zu Ihrem virtuellen Computer einrichten. Netzwerksicherheitsgruppen bieten eine Vielzahl erstklassiger Funktionen sowie eine differenzierte Steuerung des Ressourcenzugriffs. Weitere Informationen über die [Erstellung von Netzwerksicherheitsgruppen und ACL-Regeln finden Sie hier](../virtual-network/virtual-networks-create-nsg-arm-ps.md).

Netzwerksicherheitsgruppen und ACL-Regeln können auch als Teil von Azure Resource Manager-Vorlagen definiert werden. Erfahren Sie mehr über das [Erstellen von Netzwerksicherheitsgruppen mit Vorlagen](../virtual-network/virtual-networks-create-nsg-arm-template.md).

Wenn eine Portweiterleitung notwendig ist, um einem internen Port Ihres virtuellen Computers einen eindeutigen externen Port zuzuordnen, müssen Load Balancer- und NAT-Regeln (Netzwerkadressenübersetzung) verwendet werden. Sie können beispielsweise TCP-Port 8080 extern verfügbar machen und den Datenverkehr an TCP-Port 80 auf einem virtuellen Computer weiterleiten. Hier finden Sie weitere Informationen zum [Erstellen eines Load Balancers mit Internetzugriff](../load-balancer/load-balancer-get-started-internet-arm-ps.md).

## Nächste Schritte
In diesem Beispiel haben Sie eine einfache Regel erstellt, die den HTTP-Verkehr ermöglicht. Informationen zum Erstellen von detaillierteren Umgebungen finden Sie in den folgenden Artikeln:

- [Übersicht über Azure Resource Manager](../resource-group-overview.md)
- [Was ist eine Netzwerksicherheitsgruppe (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Übersicht über Azure Resource Manager für Load Balancer](../load-balancer/load-balancer-arm.md)

<!---HONumber=AcomDC_0601_2016-->