---
title: "Öffnen von Ports für einen virtuellen Linux-Computer mit Azure CLI 2.0 | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie mit dem Azure Resource Manager-Bereitstellungsmodell und Azure CLI 2.0 für Ihren virtuellen Linux-Computer einen Port öffnen oder einen Endpunkt erstellen."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: eef9842b-495a-46cf-99a6-74e49807e74e
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 08/21/2017
ms.author: iainfou
ms.translationtype: HT
ms.sourcegitcommit: cf381b43b174a104e5709ff7ce27d248a0dfdbea
ms.openlocfilehash: d176187fe465264b5f433260de5178b48ca9dd4a
ms.contentlocale: de-de
ms.lasthandoff: 08/23/2017

---
# <a name="open-ports-and-endpoints-to-a-linux-vm-with-the-azure-cli"></a>Öffnen von Ports und Endpunkten für einen virtuellen Linux-Computer mithilfe der Azure-CLI
In Azure öffnen Sie einen Port oder erstellen einen Endpunkt für einen virtuellen Computer (VM), indem Sie einen Netzwerkfilter in einem Subnetz oder einer VM-Netzwerkschnittstelle erstellen. Sie platzieren diese Filter, mit denen sowohl eingehender als auch ausgehender Datenverkehr gesteuert werden kann, in einer Netzwerksicherheitsgruppe, die an die Ressource angefügt ist, die den Datenverkehr empfängt. Wir verwenden ein gängiges Beispiel für Webdatenverkehr über Port 80. In diesem Artikel wird beschrieben, wie Sie mithilfe von Azure CLI 2.0 einen Port für einen virtuellen Computer öffnen. Sie können diese Schritte auch mit [Azure CLI 1.0](nsg-quickstart-nodejs.md) ausführen.


## <a name="quick-commands"></a>Schnellbefehle
Um eine Netzwerksicherheitsgruppe und Regeln erstellen zu können, muss die neueste Version von [Azure CLI 2.0](/cli/azure/install-az-cli2) installiert sein, und Sie müssen mithilfe von [az login](/cli/azure/#login) bei einem Azure-Konto angemeldet sein.

Ersetzen Sie in den folgenden Beispielen die Beispielparameternamen durch Ihre eigenen Werte. Beispielparameternamen sind etwa *myResourceGroup*, *myNetworkSecurityGroup* oder *myVnet*.

Erstellen Sie die Netzwerksicherheitsgruppe mit [az network nsg create](/cli/azure/network/nsg#create). Im folgenden Beispiel wird die Netzwerksicherheitsgruppe *myNetworkSecurityGroup* am Standort *USA, Osten* erstellt:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

Fügen Sie mit [az network nsg rule create](/cli/azure/network/nsg/rule#create) eine Regel hinzu, um HTTP-Datenverkehr an Ihren Webserver zuzulassen (oder passen Sie die Regel für Ihr eigenes Szenario an, beispielsweise für SSH-Zugriff oder Datenbankverbindungen). Im folgenden Beispiel wird die Regel *myNetworkSecurityGroupRule* erstellt, um TCP-Datenverkehr an Port 80 zuzulassen:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --protocol tcp \
    --priority 1000 \
    --destination-port-range 80
```

Ordnen Sie die Netzwerksicherheitsgruppe mit [az network nic update](/cli/azure/network/nic#update) der Netzwerkschnittstelle des virtuellen Computers zu. Im folgenden Beispiel wird die vorhandene Netzwerkschnittstelle *myNic* Ihrer Netzwerksicherheitsgruppe *myNetworkSecurityGroup* zugewiesen:

```azurecli
az network nic update \
    --resource-group myResourceGroup \
    --name myNic \
    --network-security-group myNetworkSecurityGroup
```

Alternativ können Sie Ihre Netzwerksicherheitsgruppe mit [az network vnet subnet update](/cli/azure/network/vnet/subnet#update) dem Subnetz Ihres virtuellen Netzwerks zuordnen anstatt nur der Netzwerkschnittstelle eines einzelnen virtuellen Computers. Im folgenden Beispiel wird das vorhandene Subnetz *mySubnet* im virtuellen Netzwerk *myVnet* Ihrer Netzwerksicherheitsgruppe *myNetworkSecurityGroup* zugewiesen:

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```

## <a name="more-information-on-network-security-groups"></a>Weitere Informationen zu Netzwerksicherheitsgruppen
Mit diesen Schnellbefehlen können Sie den Datenverkehr zu Ihrem virtuellen Computer einrichten. Netzwerksicherheitsgruppen bieten eine Vielzahl erstklassiger Funktionen sowie eine differenzierte Steuerung des Ressourcenzugriffs. Weitere Informationen über die [Erstellung von Netzwerksicherheitsgruppen und ACL-Regeln erhalten Sie hier](tutorial-virtual-network.md#secure-network-traffic).

Bei hoch verfügbaren Webanwendungen sollten Sie die virtuellen Computer hinter einem Azure Load Balancer anordnen. Der Load Balancer verteilt den Datenverkehr auf virtuelle Computer, mit einer Netzwerksicherheitsgruppe, die den Datenverkehr filtert. Weitere Informationen finden Sie unter [Lastenausgleich für virtuelle Linux-Computer in Azure zum Erstellen einer hoch verfügbaren Anwendung](tutorial-load-balancer.md).

## <a name="next-steps"></a>Nächste Schritte
In diesem Beispiel haben Sie eine einfache Regel erstellt, die HTTP-Datenverkehr zulässt. Informationen zum Erstellen von detaillierteren Umgebungen finden Sie in den folgenden Artikeln:

* [Übersicht über den Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md)
* [Was ist eine Netzwerksicherheitsgruppe (NSG)?](../../virtual-network/virtual-networks-nsg.md)

