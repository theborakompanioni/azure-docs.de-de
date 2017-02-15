---
title: "Öffnen von Ports und Endpunkten auf einem virtuellen Linux-Computer in Azure | Microsoft Docs"
description: "Erfahren Sie, wie Sie mit dem Azure Resource Manager-Bereitstellungsmodell und der Azure CLI 1.0 für Ihren virtuellen Linux-Computer einen Port öffnen oder einen Endpunkt erstellen."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 10/27/2016
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 2ca514ec1bbfb5404070de904effd1522fcac892
ms.openlocfilehash: fd43b3a16607ccd383fbf185176d94af5aa97d89


---
# <a name="opening-ports-and-endpoints-to-a-linux-vm-in-azure"></a>Öffnen von Ports und Endpunkten auf einem virtuellen Linux-Computer in Azure
In Azure öffnen Sie einen Port oder erstellen einen Endpunkt für einen virtuellen Computer (VM), indem Sie einen Netzwerkfilter in einem Subnetz oder einer VM-Netzwerkschnittstelle erstellen. Sie platzieren diese Filter, mit denen sowohl eingehender als auch ausgehender Datenverkehr gesteuert werden kann, in einer Netzwerksicherheitsgruppe, die an die Ressource angefügt ist, die den Datenverkehr empfängt. Wir verwenden ein gängiges Beispiel für Webdatenverkehr über Port 80. In diesem Artikel wird beschrieben, wie Sie mithilfe der Azure CLI 1.0 einen Port für einen virtuellen Computer öffnen.


## <a name="cli-versions-to-complete-the-task"></a>CLI-Versionen zum Durchführen dieser Aufgabe
Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch:

- [Azure-CLI 1.0](#quick-commands): Unsere CLI für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell (in diesem Artikel)
- [Azure CLI 2.0 (Vorschau):](virtual-machines-linux-nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Unsere CLI der nächsten Generation für das Resource Manager-Bereitstellungsmodell


## <a name="quick-commands"></a>Schnellbefehle
Zum Erstellen einer Netzwerksicherheitsgruppe und der zugehörigen Regeln müssen Sie [die Azure CLI 1.0](../xplat-cli-install.md) im Resource Manager-Modus installieren und verwenden:

```azurecli
azure config mode arm
```

Ersetzen Sie in den folgenden Beispielen die Beispielparameternamen durch Ihre eigenen Werte. Zu den Beispielparameternamen zählen `myResourceGroup`, `myNetworkSecurityGroup` und `myVnet`.

Erstellen Sie die Netzwerksicherheitsgruppe, und geben Sie Ihre eigenen Werte für Namen und Speicherort ein. Im folgenden Beispiel wird eine Netzwerksicherheitsgruppe namens `myNetworkSecurityGroup` im Speicherort `WestUS` erstellt:

```azurecli
azure network nsg create --resource-group myResourceGroup --location westus \
    --name myNetworkSecurityGroup
```

Fügen Sie eine Regel hinzu, um HTTP-Datenverkehr an Ihren Webserver zuzulassen (oder passen Sie die Regel für Ihr eigenes Szenario an, beispielsweise für SSH-Zugriff oder Datenbankverbindungen). Im folgenden Beispiel wird eine Regel namens `myNetworkSecurityGroupRule` erstellt, um TCP-Datenverkehr an Port 80 zuzulassen:

```azurecli
azure network nsg rule create --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup --name myNetworkSecurityGroupRule \
    --protocol tcp --direction inbound --priority 1000 \
    --destination-port-range 80 --access allow
```

Ordnen Sie die Netzwerksicherheitsgruppe der VM-Netzwerkschnittstelle zu. Im folgenden Beispiel wird die vorhandene Netzwerkschnittstelle `myNic` Ihrer Netzwerksicherheitsgruppe `myNetworkSecurityGroup` zugewiesen:

```azurecli
azure network nic set --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup --name myNic
```

Alternativ dazu können Sie Ihre Netzwerksicherheitsgruppe dem Subnetz Ihres virtuellen Netzwerks zuordnen statt der Netzwerkschnittstelle eines einzelnen virtuellen Computers. Im folgenden Beispiel wird das vorhandene Subnetz `mySubnet` im virtuellen Netzwerk `myVnet` Ihrer Netzwerksicherheitsgruppe `myNetworkSecurityGroup` zugewiesen:

```azurecli
azure network vnet subnet set --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup \
    --vnet-name myVnet --name mySubnet
```

## <a name="more-information-on-network-security-groups"></a>Weitere Informationen zu Netzwerksicherheitsgruppen
Mit diesen Schnellbefehlen können Sie den Datenverkehr zu Ihrem virtuellen Computer einrichten. Netzwerksicherheitsgruppen bieten eine Vielzahl erstklassiger Funktionen sowie eine differenzierte Steuerung des Ressourcenzugriffs. Weitere Informationen über die [Erstellung von Netzwerksicherheitsgruppen und ACL-Regeln erhalten Sie hier](../virtual-network/virtual-networks-create-nsg-arm-cli.md).

Sie können Netzwerksicherheitsgruppen und ACL-Regeln auch als Teil von Azure Resource Manager-Vorlagen definieren. Erfahren Sie mehr über [das Erstellen von Netzwerksicherheitsgruppen mit Vorlagen](../virtual-network/virtual-networks-create-nsg-arm-template.md).

Wenn eine Portweiterleitung notwendig ist, um einem internen Port des virtuellen Computers einen eindeutigen externen Port zuzuweisen, verwenden Sie einen Lastenausgleich sowie NAT-Regeln (Network Address Translation, Netzwerkadressübersetzung). Sie können beispielsweise TCP-Port 8080 extern verfügbar machen und den Datenverkehr an TCP-Port 80 auf einem virtuellen Computer weiterleiten. Hier finden Sie weitere Informationen zum [Erstellen eines Load Balancers mit Internetzugriff](../load-balancer/load-balancer-get-started-internet-arm-cli.md).

## <a name="next-steps"></a>Nächste Schritte
In diesem Beispiel haben Sie eine einfache Regel erstellt, die HTTP-Datenverkehr zulässt. Informationen zum Erstellen von detaillierteren Umgebungen finden Sie in den folgenden Artikeln:

* [Übersicht über den Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)
* [Was ist eine Netzwerksicherheitsgruppe (NSG)?](../virtual-network/virtual-networks-nsg.md)
* [Übersicht über Azure Resource Manager für Load Balancer](../load-balancer/load-balancer-arm.md)




<!--HONumber=Dec16_HO2-->


