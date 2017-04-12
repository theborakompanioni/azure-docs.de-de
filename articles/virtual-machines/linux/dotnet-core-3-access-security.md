---
title: "Zugriff und Sicherheit in Azure-Vorlagen für Linux-VMs | Microsoft-Dokumentation"
description: ".NET Core-Tutorial für virtuelle Azure-Computer"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 07e47189-680e-4102-a8d4-5a8eb9c00213
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 11/21/2016
ms.author: nepeters
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 8dae248b5ac4059da4684336b22030c63546da15
ms.lasthandoff: 04/03/2017

---
# <a name="access-and-security-in-azure-resource-manager-templates-for-linux-vms"></a>Zugriff und Sicherheit in Azure Resource Manager-Vorlagen für Linux-VMs

Auf in Azure gehostete Anwendungen muss in der Regel über das Internet oder über eine VPN-/ExpressRoute-Verbindung mit Azure zugegriffen werden. Im Rahmen des Music Store-Anwendungsbeispiels wird die Website im Internet mit einer öffentlichen IP-Adresse verfügbar gemacht. Nach Einrichtung des Zugriffs müssen die Verbindungen mit der Anwendung sowie der Zugriff auf die Ressourcen des virtuellen Computers geschützt werden. Diese Zugriffssicherheit wird mithilfe einer Netzwerksicherheitsgruppe erreicht. 

In diesem Dokument erfahren Sie, wie die Music Store-Anwendung in der Azure Resource Manager-Beispielvorlage geschützt wird. Alle Abhängigkeiten und individuellen Konfigurationen werden hervorgehoben. Stellen Sie am besten vorab eine Instanz der Lösung in Ihrem Azure-Abonnement bereit, und orientieren Sie sich an der Azure Resource Manager-Vorlage. Die vollständige Vorlage finden Sie [hier](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux). 

## <a name="public-ip-address"></a>Öffentliche IP-Adresse
Eine Azure-Ressource kann mithilfe einer öffentlichen IP-Adressressource öffentlich zugänglich gemacht werden. Eine öffentliche IP-Adresse kann mit einer statischen oder mit einer dynamischen IP-Adresse konfiguriert werden. Dynamische Adressen werden entfernt, wenn der virtuelle Computer beendet und seine Zuordnung aufgehoben wird. Wenn der Computer wieder gestartet wird, erhält er unter Umständen eine andere öffentliche IP-Adresse. Um die Änderung einer IP-Adresse zu verhindern, können Sie eine reservierte IP-Adresse verwenden. 

Eine öffentliche IP-Adresse kann einer Azure Resource Manager-Vorlage mithilfe des Visual Studio-Assistenten zum Hinzufügen neuer Ressourcen oder durch Einfügen von gültigem JSON-Code in eine Vorlage hinzugefügt werden. 

Das entsprechende JSON-Beispiel in der Resource Manager-Vorlage finden Sie [hier](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L121).

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[variables('publicipaddressName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "public-ip-front"
  },
  "properties": {
    "publicIPAllocationMethod": "Dynamic",
    "dnsSettings": {
      "domainNameLabel": "[parameters('publicipaddressDnsName')]"
    }
  }
}
```

Eine öffentliche IP-Adresse kann einem virtuellen Netzwerkadapter oder einem Lastenausgleich zugeordnet werden. Da bei der Music Store-Website ein Lastenausgleich mit mehreren virtuellen Computern verwendet wird, ist die öffentliche IP-Adresse in diesem Beispiel mit dem Lastenausgleich verknüpft.

Das entsprechende JSON-Beispiel in der Resource Manager-Vorlage finden Sie [hier](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L208).

```json
"frontendIPConfigurations": [
  {
    "properties": {
      "publicIPAddress": {
        "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicipaddressName'))]"
      }
    },
    "name": "LoadBalancerFrontend"
  }
]
```

Hier sehen Sie die öffentliche IP-Adresse im Azure-Portal. Beachten Sie, dass die öffentliche IP-Adresse einem Lastenausgleich (und keinem virtuellen Computer) zugeordnet ist. Module für den Netzwerklastenausgleich werden im nächsten Dokument dieser Reihe behandelt.

![Öffentliche IP-Adresse](./media/dotnet-core-3-access-security/pubip.png)

Weitere Informationen zu öffentlichen Azure-IP-Adressen finden Sie unter [IP-Adressen in Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).

## <a name="network-security-group"></a>Netzwerksicherheitsgruppen (NSG)
Es empfiehlt sich, nach der Einrichtung den Zugriff auf Azure-Ressourcen einzuschränken. Bei virtuellen Azure-Computern wird ein sicherer Zugriff mithilfe einer Netzwerksicherheitsgruppe erreicht. Im Music Store-Anwendungsbeispiel ist mit Ausnahme von Port 80 (HTTP-Zugriff) und Port 22 (SSH-Zugriff) jeglicher Zugriff auf den virtuellen Computer eingeschränkt. Eine Netzwerksicherheitsgruppe kann einer Azure Resource Manager-Vorlage mithilfe des Visual Studio-Assistenten zum Hinzufügen neuer Ressourcen oder durch Einfügen von gültigem JSON-Code in eine Vorlage hinzugefügt werden.

Das entsprechende JSON-Beispiel in der Resource Manager-Vorlage finden Sie [hier](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L68).

```json
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Network/networkSecurityGroups",
  "name": "[variables('nsgfront')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "nsg-front"
  },
  "properties": {
    "securityRules": [
      {
        "name": "http",
        "properties": {
          "description": "http endpoint",
          "protocol": "Tcp",
          "sourcePortRange": "*",
          "destinationPortRange": "80",
          "sourceAddressPrefix": "*",
          "destinationAddressPrefix": "*",
          "access": "Allow",
          "priority": 100,
          "direction": "Inbound"
        }
      },
      ........<truncated> 
    ]
  }
}
```

In diesem Beispiel ist die Netzwerksicherheitsgruppe dem in der Virtual Network-Ressource deklarierten Subnetz zugeordnet. 

Das entsprechende JSON-Beispiel in der Resource Manager-Vorlage finden Sie [hier](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L158).

```json
"subnets": [
  {
    "name": "[variables('subnetName')]",
    "properties": {
      "addressPrefix": "10.0.0.0/24",
      "networkSecurityGroup": {
        "id": "[resourceId('Microsoft.Network/networkSecurityGroups', variables('networkSecurityGroup'))]"
      }
    }
  }
```

Hier sehen Sie die Netzwerksicherheitsgruppe im Azure-Portal. Eine NSG kann einem Subnetz und/oder einer Netzwerkschnittstelle zugeordnet werden. In diesem Fall ist die NSG einem Subnetz zugeordnet. In dieser Konfiguration gelten die eingehenden Regeln für alle virtuellen Computer, die mit dem Subnetz verbunden sind.

![Netzwerksicherheitsgruppen (NSG)](./media/dotnet-core-3-access-security/nsg.png)

Ausführliche Informationen zu Netzwerksicherheitsgruppen finden Sie unter [Was ist eine Netzwerksicherheitsgruppe (NSG)?](https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/).

## <a name="next-step"></a>Nächster Schritt
<hr>

[Schritt 3: Verfügbarkeit und Skalierung in Azure Resource Manager-Vorlagen](dotnet-core-4-availability-scale.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)


