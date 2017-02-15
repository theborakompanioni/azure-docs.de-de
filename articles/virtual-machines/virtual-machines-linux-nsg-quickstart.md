---
title: "Öffnen von Ports und Endpunkten für einen virtuellen Linux-Computer in Azure | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie für Ihren virtuellen Linux-Computer mithilfe von Azure Resource Manager-Bereitstellungsmodell und Azure CLI 2.0 (Vorschau) einen Port öffnen oder einen Endpunkt erstellen."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: eef9842b-495a-46cf-99a6-74e49807e74e
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 12/8/2016
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: e4512dd4d818b1c7bea7e858a397728ce48a5362
ms.openlocfilehash: 40f399c339e31d9d008230449d7f559ae01afba3


---
# <a name="opening-ports-and-endpoints-to-a-linux-vm-in-azure"></a>Öffnen von Ports und Endpunkten auf einem virtuellen Linux-Computer in Azure
In Azure öffnen Sie einen Port oder erstellen einen Endpunkt für einen virtuellen Computer (VM), indem Sie einen Netzwerkfilter in einem Subnetz oder einer VM-Netzwerkschnittstelle erstellen. Sie platzieren diese Filter, mit denen sowohl eingehender als auch ausgehender Datenverkehr gesteuert werden kann, in einer Netzwerksicherheitsgruppe, die an die Ressource angefügt ist, die den Datenverkehr empfängt. Wir verwenden ein gängiges Beispiel für Webdatenverkehr über Port 80. In diesem Artikel erfahren Sie, wie Sie mithilfe von Azure CLI 2.0 (Vorschau) einen Port für einen virtuellen Computer öffnen.


## <a name="cli-versions-to-complete-the-task"></a>CLI-Versionen zum Durchführen dieser Aufgabe
Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch:

- [Azure CLI 1.0:](virtual-machines-linux-nsg-quickstart-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Unsere CLI für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell
- [Azure CLI 2.0 (Vorschau):](#quick-commands) Unsere Befehlszeilenschnittstelle der nächsten Generation für das Resource Manager-Bereitstellungsmodell (dieser Artikel)


## <a name="quick-commands"></a>Schnellbefehle
Um eine Netzwerksicherheitsgruppe und Regeln erstellen zu können, muss die neueste Version von [Azure CLI 2.0 (Vorschau)](/cli/azure/install-az-cli2) installiert sein, und Sie müssen sich mithilfe von [az login](/cli/azure/#login) bei einem Azure-Konto anmelden.

Ersetzen Sie in den folgenden Beispielen die Beispielparameternamen durch Ihre eigenen Werte. Als Beispielparameternamen werden `myResourceGroup`, `myNetworkSecurityGroup` und `myVnet` verwendet.

Erstellen Sie die Netzwerksicherheitsgruppe mit [az network nsg create](/cli/azure/network/nsg#create). Im folgenden Beispiel wird am Speicherort `westus` eine Netzwerksicherheitsgruppe namens `myNetworkSecurityGroup` erstellt:

```azurecli
az network nsg create --resource-group myResourceGroup --location westus \
    --name myNetworkSecurityGroup
```

Fügen Sie mit [az network nsg rule create](/cli/azure/network/nsg/rule#create) eine Regel hinzu, um HTTP-Datenverkehr an Ihren Webserver zuzulassen (oder passen Sie die Regel für Ihr eigenes Szenario an, beispielsweise für SSH-Zugriff oder Datenbankverbindungen). Im folgenden Beispiel wird eine Regel namens `myNetworkSecurityGroupRule` erstellt, um TCP-Datenverkehr an Port 80 zuzulassen:

```azurecli
az network nsg rule create --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup --name myNetworkSecurityGroupRule \
    --protocol tcp --direction inbound --priority 1000 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 80 --access allow
```

Ordnen Sie die Netzwerksicherheitsgruppe mit [az network nic update](/cli/azure/network/nic#update) der Netzwerkschnittstelle des virtuellen Computers zu. Im folgenden Beispiel wird die vorhandene Netzwerkschnittstelle `myNic` Ihrer Netzwerksicherheitsgruppe `myNetworkSecurityGroup` zugewiesen:

```azurecli
az network nic update --resource-group myResourceGroup --name myNic \
    --network-security-group myNetworkSecurityGroup
```

Alternativ können Sie Ihre Netzwerksicherheitsgruppe mit [az network vnet subnet update](/cli/azure/network/vnet/subnet#update) dem Subnetz Ihres virtuellen Netzwerks zuordnen anstatt nur der Netzwerkschnittstelle eines einzelnen virtuellen Computers. Im folgenden Beispiel wird das vorhandene Subnetz `mySubnet` im virtuellen Netzwerk `myVnet` Ihrer Netzwerksicherheitsgruppe `myNetworkSecurityGroup` zugewiesen:

```azurecli
az network vnet subnet update --resource-group myResourceGroup \
    --vnet-name myVnet --name mySubnet --network-security-group myNetworkSecurityGroup
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


