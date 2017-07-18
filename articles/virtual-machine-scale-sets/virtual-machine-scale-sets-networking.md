---
title: "Netzwerk für Azure-VM-Skalierungsgruppen | Microsoft-Dokumentation"
description: "Konfigurationsnetzwerkeigenschaften für Azure-VM-Skalierungsgruppen."
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/06/2017
ms.author: guybo
ms.translationtype: HT
ms.sourcegitcommit: f76de4efe3d4328a37f86f986287092c808ea537
ms.openlocfilehash: 1c9487be5415d05a8699f458259d872591280d3d
ms.contentlocale: de-de
ms.lasthandoff: 07/10/2017


---
# <a name="networking-for-azure-virtual-machine-scale-sets"></a>Netzwerk für Azure-VM-Skalierungsgruppen

Wenn Sie eine Azure-VM-Skalierungsgruppe über das Portal bereitstellen, werden bestimmte Standardnetzwerkeigenschaften verwendet (beispielsweise ein Azure Load Balancer mit NAT-Eingangsregeln). Dieser Artikel beschreibt die Verwendung einiger der erweiterten Netzwerkfeatures, die Sie mit Skalierungsgruppen konfigurieren können.

Alle in diesem Artikel behandelten Features können mithilfe von Azure Resource Manager-Vorlagen konfiguriert werden. Für einige Features stehen auch Beispiele für die Azure-Befehlszeilenschnittstelle zur Verfügung. Die Version der Befehlszeilenschnittstelle muss mindestens vom Juli 2017 sein. Weitere Beispiele für die Befehlszeilenschnittstelle und für PowerShell folgen in Kürze.

## <a name="accelerated-networking"></a>Beschleunigter Netzwerkbetrieb
Der [beschleunigte Netzwerkbetrieb](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-create-vm-accelerated-networking) von Azure ermöglicht die E/A-Virtualisierung mit Einzelstamm (Single Root I/O Virtualization, SR-IOV) für einen virtuellen Computer und somit die Verbesserung der Netzwerkleistung. Wenn Sie den beschleunigten Netzwerkbetrieb mit Skalierungsgruppen verwenden möchten, legen Sie in den networkInterfaceConfigurations-Einstellungen Ihrer Skalierungsgruppe die Option „EnableAcceleratedNetworking“ auf _true_ fest. Beispiel:
```json
"networkProfile": {
    "networkInterfaceConfigurations": [
    {
      "name": "niconfig1",
      "properties": {
        "primary": true,
        "enableAcceleratedNetworking" : true,
        "ipConfigurations": [
          ...
        ]
      }
    }
   ]
}
```

## <a name="create-a-scale-set-that-references-an-existing-azure-load-balancer"></a>Erstellen einer Skalierungsgruppe, die auf einen vorhandenen Azure Load Balancer verweist
Wenn eine Skalierungsgruppe über das Azure-Portal erstellt wird, wird für die meisten Konfigurationsoptionen ein neuer Load Balancer erstellt. Wenn Sie eine Skalierungsgruppe erstellen möchten, die auf eine einen vorhandenen Load Balancer verweisen soll, können Sie dazu die Befehlszeilenschnittstelle verwenden. Das folgende Beispielskript erstellt einen Load Balancer und anschließend eine Skalierungsgruppe, die darauf verweist:
```bash
az network lb create -g lbtest -n mylb --vnet-name myvnet --subnet mysubnet --public-ip-address-allocation Static --backend-pool-name mybackendpool

az vmss create -g lbtest -n myvmss --image Canonical:UbuntuServer:16.04-LTS:latest --admin-username negat --ssh-key-value /home/myuser/.ssh/id_rsa.pub --upgrade-policy-mode Automatic --instance-count 3 --vnet-name myvnet --subnet mysubnet --lb mylb --backend-pool-name mybackendpool

```

## <a name="configurable-dns-settings"></a>Konfigurierbare DNS-Einstellungen
Standardmäßig werden für Skalierungsgruppen die spezifischen DNS-Einstellungen des VNETs und des Subnetzes verwendet, in dem sie erstellt wurden. Sie können die DNS-Einstellungen für eine Skalierungsgruppe allerdings direkt konfigurieren.

### <a name="creating-a-scale-set-with-configurable-dns-servers"></a>Erstellen einer Skalierungsgruppe mit konfigurierbaren DNS-Servern
Um mithilfe von CLI 2.0 eine Skalierungsgruppe mit einer benutzerdefinierten DNS-Konfiguration zu erstellen, fügen Sie dem Befehl _vmss create_ das Argument „--dns-servers“ und eine durch Leerzeichen getrennte Liste mit Server-IP-Adressen hinzu. Beispiel:
```bash
--dns-servers 10.0.0.6 10.0.0.5
```
Um benutzerdefinierte DNS-Server in einer Azure-Vorlage zu konfigurieren, fügen Sie dem Abschnitt „networkInterfaceConfigurations“ der Skalierungsgruppe eine Eigenschaft vom Typ „dnsSettings“ hinzu. Beispiel:
```json
"dnsSettings":{
    "dnsServers":["10.0.0.6", "10.0.0.5"]
}
```

### <a name="creating-a-scale-set-with-configurable-virtual-machine-domain-names"></a>Erstellen einer Skalierungsgruppe mit konfigurierbaren Domänennamen für virtuelle Computer
Um mithilfe von CLI 2.0 eine Skalierungsgruppe mit einem benutzerdefinierten DNS-Namen für virtuelle Computer zu erstellen, fügen Sie dem Befehl _vmss create_ das Argument _--vm-domain-name_ mit einer Zeichenfolgendarstellung des Domänennamens hinzu.

Um den Domänennamen in einer Azure-Vorlage zu konfigurieren, fügen Sie dem Abschnitt „networkInterfaceConfigurations“ der Skalierungsgruppe eine Eigenschaft vom Typ „dnsSettings“ hinzu. Beispiel:

```json
"networkProfile": {
  "networkInterfaceConfigurations": [
    {
    "name": "nic1",
    "properties": {
      "primary": "true",
      "ipConfigurations": [
      {
        "name": "ip1",
        "properties": {
          "subnet": {
            "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
          },
          "publicIPAddressconfiguration": {
            "name": "publicip",
            "properties": {
            "idleTimeoutInMinutes": 10,
              "dnsSettings": {
                "domainNameLabel": "[parameters('vmssDnsName')]"
              }
            }
          }
        }
      }
    ]
    }
}
```

Die Ausgabe für einen bestimmten VM-DNS-Namen hat das folgende Format: 
```
<vmname><vmindex>.<specifiedVmssDomainNameLabel>
```

## <a name="ipv6-preview-for-public-ips-and-load-balancer-pools"></a>IPv6-Vorschau für öffentliche IP-Adressen und Load Balancer-Pools
Sie können öffentliche IPv6-IP-Adressen für einen Azure Load Balancer konfigurieren und Verbindungen an Back-End-Pools für VM-Skalierungsgruppen weiterleiten. Zur Verwendung von IPv6 (derzeit noch in der Vorschauphase) müssen Sie zunächst eine öffentliche IPv6-Adressressource erstellen. Beispiel:
```json
{
    "apiVersion": "2016-03-30",
    "type": "Microsoft.Network/publicIPAddresses",
    "name": "[parameters('ipv6PublicIPAddressName')]",
    "location": "[parameters('location')]",
    "properties": {
        "publicIPAddressVersion": "IPv6",
        "publicIPAllocationMethod": "Dynamic",
        "dnsSettings": {
            "domainNameLabel": "[parameters('dnsNameforIPv6LbIP')]"
        }
    }
}
```
Konfigurieren Sie dann nach Bedarf Ihre Load Balancer-Front-End-IP-Konfigurationen für IPv4 und IPv6:

```json
"frontendIPConfigurations": [
    {
        "name": "LoadBalancerFrontEndIPv6",
        "properties": {
            "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('ipv6PublicIPAddressName'))]"
            }
        }
    }
]
```
Definieren Sie die erforderlichen Back-End-Pools:
```json
"backendAddressPools": [
    {
        "name": "BackendPoolIPv4"
    },
    {
        "name": "BackendPoolIPv6"
    }
]
```
Definieren Sie Lastenausgleichsregeln nach Bedarf:
```json
{
    "name": "LBRuleIPv6-46000",
    "properties": {
        "frontendIPConfiguration": {
            "id": "[variables('ipv6FrontEndIPConfigID')]"
        },
        "backendAddressPool": {
            "id": "[variables('ipv6LbBackendPoolID')]"
        },
        "protocol": "tcp",
        "frontendPort": 46000,
        "backendPort": 60001,
        "probe": {
            "id": "[variables('ipv4ipv6lbProbeID')]"
        }
    }
}
```
Verweisen Sie abschließend im Abschnitt „IPConfigurations“ der Skalierungsgruppen-Netzwerkeigenschaften auf den IPv6-Adresspool:
```json
{
    "name": "ipv6IPConfig",
    "properties": {
        "privateIPAddressVersion": "IPv6",
        "loadBalancerBackendAddressPools": [
            {
                "id": "[variables('ipv6LbBackendPoolID')]"
            }
        ]
    }
}
```

## <a name="public-ipv4-per-virtual-machine"></a>Öffentliche IPv4-Adresse pro virtuellem Computer
Virtuelle Computer in Azure-Skalierungsgruppen benötigen im Allgemeinen keine eigene IP-Adresse. In den meisten Szenarien ist es wirtschaftlicher und sicherer, eine öffentliche IP-Adresse einem Load Balancer oder einem einzelnen virtuellen Computer (auch Jumpbox genannt) zuzuweisen, der dann eingehende Verbindungen nach Bedarf an virtuelle Computer in der Skalierungsgruppe weiterleitet (beispielsweise über NAT-Eingangsregeln).

In einigen Szenarien müssen virtuelle Computer in Skalierungsgruppen jedoch über eine eigene öffentliche IP-Adresse verfügen. Ein Beispiel sind etwa Spiele, bei denen eine Konsole eine direkte Verbindung mit einem virtuellen Cloudcomputer herstellen muss, der die Spielphysik verarbeitet. Ein weiteres Beispiel, bei dem virtuelle Computer regionsübergreifend gegenseitige externe Verbindungen herstellen müssen, sind verteilte Datenbanken.

### <a name="creating-a-scale-set-with-public-ip-per-virtual-machine"></a>Erstellen einer Skalierungsgruppe mit öffentlicher IP-Adresse pro virtuellem Computer
Um mithilfe von CLI 2.0 eine Skalierungsgruppe zu erstellen, die jedem virtuellen Computer eine öffentliche IP-Adresse zuweist, fügen Sie dem Befehl _vmss create_ den Parameter _--public-ip-per-vm_ hinzu. 

Wenn Sie eine Skalierungsgruppe mithilfe einer Azure-Vorlage erstellen möchten, vergewissern Sie sich, dass die API-Version der Ressource „Microsoft.Compute/virtualMachineScaleSets“ mindestens den Wert „2017-03-30“ besitzt, und fügen Sie dem Abschnitt „ipConfigurations“ für die Skalierungsgruppe eine JSON-Eigenschaft vom Typ _publicIpAddressConfiguration_ hinzu. Beispiel:

```json
"publicIpAddressConfiguration": {
    "name": "pub1",
    "properties": {
      "idleTimeoutInMinutes": 15
    }
}
```
Beispielvorlage: [201-vmss-public-ip-linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-public-ip-linux)

### <a name="querying-the-public-ip-addresses-of-the-virtual-machines-in-a-scale-set"></a>Abfragen der öffentlichen IP-Adressen der virtuellen Computer in einer Skalierungsgruppe
Um mithilfe von CLI 2.0 die öffentlichen IP-Adressen aufzulisten, die den virtuellen Computern in einer Skalierungsgruppe zugewiesen sind, verwenden Sie den Befehl _az vmss list-instance-public-ips_.

Die öffentlichen IP-Adressen, die virtuellen Computern in einer Skalierungsgruppe zugewiesen sind, können auch über den [Azure-Ressourcen-Explorer](https://resources.azure.com) oder mithilfe der Azure-REST-API (ab Version _2017-03-30_) abgefragt werden.

Wenn Sie die öffentlichen IP-Adressen für eine Skalierungsgruppe mithilfe des Ressourcen-Explorers ermitteln möchten, sehen Sie sich den Abschnitt _publicipaddresses_ unter Ihrer Skalierungsgruppe an. Beispiel: https://resources.azure.com/subscriptions/_Ihre Abonnement-ID_/resourceGroups/_Ihre Ressourcengruppe_/providers/Microsoft.Compute/virtualMachineScaleSets/_Ihre VM-Skalierungsgruppe_/publicipaddresses

```
GET https://management.azure.com/subscriptions/{your sub ID}/resourceGroups/{RG name}/providers/Microsoft.Compute/virtualMachineScaleSets/{scale set name}/publicipaddresses?api-version=2017-03-30
```

Beispielausgabe:
```json
{
  "value": [
    {
      "name": "pub1",
      "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/pipvmss/virtualMachines/0/networkInterfaces/pipvmssnic/ipConfigurations/yourvmssipconfig/publicIPAddresses/pub1",
      "etag": "W/\"a64060d5-4dea-4379-a11d-b23cd49a3c8d\"",
      "properties": {
        "provisioningState": "Succeeded",
        "resourceGuid": "ee8cb20f-af8e-4cd6-892f-441ae2bf701f",
        "ipAddress": "13.84.190.11",
        "publicIPAddressVersion": "IPv4",
        "publicIPAllocationMethod": "Dynamic",
        "idleTimeoutInMinutes": 15,
        "ipConfiguration": {
          "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmss/virtualMachines/0/networkInterfaces/yourvmssnic/ipConfigurations/yourvmssipconfig"
        }
      }
    },
    {
      "name": "pub1",
      "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmss/virtualMachines/3/networkInterfaces/yourvmssnic/ipConfigurations/yourvmssipconfig/publicIPAddresses/pub1",
      "etag": "W/\"5f6ff30c-a24c-4818-883c-61ebd5f9eee8\"",
      "properties": {
        "provisioningState": "Succeeded",
        "resourceGuid": "036ce266-403f-41bd-8578-d446d7397c2f",
        "ipAddress": "13.84.159.176",
        "publicIPAddressVersion": "IPv4",
        "publicIPAllocationMethod": "Dynamic",
        "idleTimeoutInMinutes": 15,
        "ipConfiguration": {
          "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmss/virtualMachines/3/networkInterfaces/yourvmssnic/ipConfigurations/yourvmssipconfig"
        }
      }
    }
```

## <a name="multiple-ip-addresses-per-nic"></a>Mehrere IP-Adressen pro NIC
Jeder NIC, die an einen virtuellen Computer in einer Skalierungsgruppe angefügt ist, kann mindestens eine IP-Konfiguration zugeordnet sein. Jeder Konfiguration wird eine private IP-Adresse zugewiesen. Außerdem kann jeder Konfiguration eine öffentliche IP-Adressressource zugeordnet sein. Informationen dazu, wie viele IP-Adressen einer NIC zugewiesen werden und wie viele öffentliche IP-Adressen Sie in einem Azure-Abonnement verwenden können, finden Sie unter [Netzwerkgrenzwerte – Azure Resource Manager](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

## <a name="multiple-nics-per-virtual-machine"></a>Mehrere NICs vor virtuellem Computer
Ein virtueller Computer kann je nach Größe über bis zu acht NICs verfügen. Informationen zur maximalen Anzahl von NICs pro Computer finden Sie im Artikel [Größen für virtuelle Windows-Computer in Azure](../virtual-machines/windows/sizes.md). Das folgende Beispiel ist ein Skalierungsgruppen-Netzwerkprofil mit mehreren NIC-Einträgen und mehreren öffentlichen IP-Adressen pro virtuellem Computer:
```json
"networkProfile": {
    "networkInterfaceConfigurations": [
        {
        "name": "nic1",
        "properties": {
            "primary": "true",
            "ipConfigurations": [
            {
                "name": "ip1",
                "properties": {
                "subnet": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                },
                "publicipaddressconfiguration": {
                    "name": "pub1",
                    "properties": {
                    "idleTimeoutInMinutes": 15
                    }
                },
                "loadBalancerInboundNatPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                    }
                ],
                "loadBalancerBackendAddressPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                    }
                ]
                }
            }
            ]
        }
        },
        {
        "name": "nic2",
        "properties": {
            "primary": "false",
            "ipConfigurations": [
            {
                "name": "ip1",
                "properties": {
                "subnet": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                },
                "publicipaddressconfiguration": {
                    "name": "pub1",
                    "properties": {
                    "idleTimeoutInMinutes": 15
                    }
                },
                "loadBalancerInboundNatPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                    }
                ],
                "loadBalancerBackendAddressPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                    }
                ]
                }
            }
            ]
        }
        }
    ]
}
```

## <a name="nsg-per-scale-set"></a>NSGs pro Skalierungsgruppe
Netzwerksicherheitsgruppen können direkt auf eine Skalierungsgruppe angewendet werden. Hierzu muss dem Abschnitt mit den Netzwerkschnittstellenkonfigurationen der Skalierungsgruppen-VM-Eigenschaften ein Verweis hinzugefügt werden.

Beispiel: 
```
"networkProfile": {
    "networkInterfaceConfigurations": [
        {
            "name": "nic1",
            "properties": {
                "primary": "true",
                "ipConfigurations": [
                    {
                        "name": "ip1",
                        "properties": {
                            "subnet": {
                                "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                            }
                "loadBalancerInboundNatPools": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                                }
                            ],
                            "loadBalancerBackendAddressPools": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                                }
                            ]
                        }
                    }
                ],
                "networkSecurityGroup": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/networkSecurityGroups/', variables('nsgName'))]"
                }
            }
        }
    ]
}
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Azure-Netzwerken finden Sie unter [Virtuelle Netzwerke im Überblick](../virtual-network/virtual-networks-overview.md).

