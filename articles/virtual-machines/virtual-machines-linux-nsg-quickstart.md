---
title: Öffnen von Ports für einen virtuellen Linux-Computer | Microsoft Docs
description: Erfahren Sie, wie Sie mit dem Azure Resource Manager-Bereitstellungsmodell und der Azure-Befehlszeilenschnittstelle für Ihren virtuellen Linux-Computer einen Port öffnen oder einen Endpunkt erstellen.
services: virtual-machines-linux
documentationcenter: ''
author: iainfoulds
manager: timlt
editor: ''

ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 08/08/2016
ms.author: iainfou

---
# Öffnen von Ports für eine Linux-VM in Azure
In Azure öffnen Sie einen Port oder erstellen einen Endpunkt für einen virtuellen Computer (VM), indem Sie einen Netzwerkfilter in einem Subnetz oder einer VM-Netzwerkschnittstelle erstellen. Sie platzieren diese Filter, mit denen sowohl eingehender als auch ausgehender Datenverkehr gesteuert werden kann, in einer Netzwerksicherheitsgruppe, die an die Ressource angefügt ist, die den Datenverkehr empfängt. Wir verwenden ein gängiges Beispiel für Webdatenverkehr über Port 80.

## Schnellbefehle
Zum Erstellen einer Netzwerksicherheitsgruppe und der zugehörigen Regeln benötigen Sie [die Azure-CLI](../xplat-cli-install.md) im Resource Manager-Modus (`azure config mode arm`).

Erstellen Sie die Netzwerksicherheitsgruppe, und geben Sie Ihre eigenen Werte für Namen und Speicherort ein:

```
azure network nsg create --resource-group TestRG --name TestNSG --location westus
```

Fügen Sie eine Regel hinzu, um HTTP-Datenverkehr an Ihren Webserver zuzulassen (oder passen Sie die Regel für Ihr eigenes Szenario an, beispielsweise für SSH-Zugriff oder Datenbankverbindungen):

```
azure network nsg rule create --protocol tcp --direction inbound --priority 1000 \
    --destination-port-range 80 --access allow --resource-group TestRG --nsg-name TestNSG --name AllowHTTP
```

Ordnen Sie die Netzwerksicherheitsgruppe der VM-Netzwerkschnittstelle zu:

```
azure network nic set --resource-group TestRG --name TestNIC --network-security-group-name TestNSG
```

Alternativ dazu können Sie Ihre Netzwerksicherheitsgruppe dem Subnetz Ihres virtuellen Netzwerks zuordnen statt der Netzwerkschnittstelle eines einzelnen virtuellen Computers:

```
azure network vnet subnet set --resource-group TestRG --name TestSubnet --network-security-group-name TestNSG
```

## Weitere Informationen zu Netzwerksicherheitsgruppen
Mit diesen Schnellbefehlen können Sie den Datenverkehr zu Ihrem virtuellen Computer einrichten. Netzwerksicherheitsgruppen bieten eine Vielzahl erstklassiger Funktionen sowie eine differenzierte Steuerung des Ressourcenzugriffs. Weitere Informationen über die [Erstellung von Netzwerksicherheitsgruppen und ACL-Regeln erhalten Sie hier](../virtual-network/virtual-networks-create-nsg-arm-cli.md).

Sie können Netzwerksicherheitsgruppen und ACL-Regeln auch als Teil von Azure Resource Manager-Vorlagen definieren. Erfahren Sie mehr über [das Erstellen von Netzwerksicherheitsgruppen mit Vorlagen](../virtual-network/virtual-networks-create-nsg-arm-template.md).

Wenn eine Portweiterleitung notwendig ist, um einem internen Port des virtuellen Computers einen eindeutigen externen Port zuzuweisen, verwenden Sie einen Lastenausgleich sowie NAT-Regeln (Network Address Translation, Netzwerkadressübersetzung). Sie können beispielsweise TCP-Port 8080 extern verfügbar machen und den Datenverkehr an TCP-Port 80 auf einem virtuellen Computer weiterleiten. Hier finden Sie weitere Informationen zum [Erstellen eines Load Balancers mit Internetzugriff](../load-balancer/load-balancer-get-started-internet-arm-cli.md).

## Nächste Schritte
In diesem Beispiel haben Sie eine einfache Regel erstellt, die HTTP-Datenverkehr zulässt. Informationen zum Erstellen von detaillierteren Umgebungen finden Sie in den folgenden Artikeln:

* [Übersicht über den Azure-Ressourcen-Manager](../resource-group-overview.md)
* [Was ist eine Netzwerksicherheitsgruppe (NSG)?](../virtual-network/virtual-networks-nsg.md)
* [Unterstützung von Azure Resource Manager für Load Balancer](../load-balancer/load-balancer-arm.md)

<!---HONumber=AcomDC_0907_2016-->