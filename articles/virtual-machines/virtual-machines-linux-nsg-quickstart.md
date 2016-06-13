<properties
   pageTitle="Zulassen des externen Zugriffs auf einen virtuellen Linux-Computer | Microsoft Azure"
   description="Erfahren Sie, wie Sie mit dem Resource Manager-Bereitstellungsmodell und der Azure-Befehlszeilenschnittstelle einen Port öffnen oder einen Endpunkt erstellen, um externen Zugriff auf Ihren virtuellen Linux-Computer zu ermöglichen."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="05/24/2016"
   ms.author="iainfou"/>

# Zulassen des externen Zugriffs auf einen virtuellen Computer
[AZURE.INCLUDE [virtual-machines-common-nsg-quickstart](../../includes/virtual-machines-common-nsg-quickstart.md)]

## Schnellbefehle
Zum Erstellen einer Netzwerksicherheitsgruppe und der zugehörigen Regeln benötigen Sie [die Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md) im Resource Manager-Modus (`azure config mode arm`).

Erstellen Sie die Netzwerksicherheitsgruppe folgendermaßen, und geben Sie Ihre eigenen Werte für Namen und Speicherort ein.

```
azure network nsg create --resource-group TestRG --name TestNSG --location westus
```

Fügen Sie eine Regel hinzu, um HTTP-Datenverkehr an Ihren Webserver zuzulassen (passen Sie die Regel für Ihr eigenes Szenario an, beispielsweise für SSH-Zugriff oder Datenbankverbindungen):

```
azure network nsg rule create --protocol tcp --direction inbound --priority 1000 \
    --destination-port-range 80 --access allow --resource-group TestRG --nsg-name TestNSG --name AllowHTTP
```

Ordnen Sie die Netzwerksicherheitsgruppe der VM-Netzwerkschnittstelle zu:

```
azure network nic set --resource-group TestRG --name TestNIC --network-security-group-name TestNSG
```

Alternativ dazu können Sie Ihre Netzwerksicherheitsgruppe dem Subnetz Ihres virtuellen Netzwerks zuordnen statt dem Netzwerk (wie im Fall eines einzelnen virtuellen Computers):

```
azure network vnet subnet set --resource-group TestRG --name TestSubnet --network-security-group-name TestNSG
```

## Weitere Informationen zu Netzwerksicherheitsgruppen
Mit diesen Schnellbefehlen können Sie den Datenverkehr zu Ihrem virtuellen Computer einrichten. Netzwerksicherheitsgruppen bieten viele herausragende Funktionen sowie Granularität zum Steuern von Ressourcenzugriffen. Weitere Informationen über das [Erstellen von Netzwerksicherheitsgruppen und ACL-Regeln erhalten Sie hier](../virtual-network/virtual-networks-create-nsg-arm-cli.md).

Netzwerksicherheitsgruppen und ACL-Regeln können auch als Teil von Azure Resource Manager-Vorlagen definiert werden. Erfahren Sie mehr über das [Erstellen von Netzwerksicherheitsgruppen mit Vorlagen](../virtual-network/virtual-networks-create-nsg-arm-template.md).

Wenn eine Anschlussweiterleitung notwendig ist, um einem internen Port des virtuellen Computers einen eindeutigen externen Port zuzuweisen, müssen ein Lastenausgleich sowie NAT-Regeln (Network Address Translation, Netzwerkadressübersetzung) verwendet werden. Sie können beispielsweise TCP-Port 8080 extern verfügbar machen und den Datenverkehr an TCP-Port 80 auf einem virtuellen Computer weiterleiten. Hier finden Sie weitere Informationen zum [Erstellen eines Load Balancers mit Internetzugriff](../load-balancer/load-balancer-get-started-internet-arm-cli.md).

## Nächste Schritte
In diesem Beispiel haben Sie eine einfache Regel erstellt, die HTTP-Datenverkehr zulässt. Weitere Informationen zum Erstellen detaillierterer Umgebungen finden Sie in den folgenden Artikeln:

- [Übersicht über den Azure-Ressourcen-Manager](../resource-group-overview.md)
- [Was ist eine Netzwerksicherheitsgruppe (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Unterstützung von Azure Resource Manager für Load Balancer](../load-balancer2 /load-balancer-arm.md)

<!---HONumber=AcomDC_0601_2016-->