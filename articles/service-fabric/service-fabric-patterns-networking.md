---
title: "Netzwerkmuster für Azure Service Fabric | Microsoft-Dokumentation"
description: "In diesem Thema werden häufige Netzwerkmuster für Service Fabric sowie die Vorgehensweise zum Erstellen eines Clusters mithilfe der Netzwerkfeatures von Azure beschrieben."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/2017
ms.author: ryanwi
translationtype: Human Translation
ms.sourcegitcommit: 030114fa1ea9b76c0ed48baeffb8859260fc8e52
ms.openlocfilehash: 78012ae1552c01690b0ad5b1285173ef9faf12bc
ms.lasthandoff: 03/01/2017


---
# <a name="service-fabric-networking-patterns"></a>Netzwerkmuster für Service Fabric
Eine der häufigsten Fragen, die beim Erstellen eines Service Fabric-Clusters auftreten, ist, wie man den Cluster mithilfe verschiedener Netzwerkfeatures von Azure integriert.  In diesem Artikel wird gezeigt, wie man Cluster mithilfe der folgenden Features erstellt:

- [Vorhandenes virtuelles Netzwerk / Subnetz](#existingvnet)
- [Statische öffentliche IP-Adresse](#staticpublicip)
- [Rein interner Lastenausgleich](#internallb)
- [Interner + externer Lastenausgleich](#internalexternallb)

Ein zu berücksichtigendes Hauptkonzept ist, dass Service Fabric in einer standardmäßigen VM-Skalierungsgruppe ausgeführt wird. Dadurch können alle Funktionen, die Sie in einer VM-Skalierungsgruppe verwenden können, auch in einem Service Fabric-Cluster verwendet werden. Die Teile des Netzwerks der Resource Manager-Vorlage sind identisch.  Sobald Sie an ein vorhandenes virtuelles Netzwerk (VNET) bereitstellen, können Sie ganz einfach andere Netzwerkfeatures, z.B. ExpressRoute, VPN Gateway, Netzwerksicherheitsgruppen (NSG) und VNET-Peering integrieren.

Nur ein Aspekt ist spezifisch für Service Fabric: Das [Azure-Portal](https://portal.azure.com) verwendet intern den Service Fabric-Ressourcenanbieter (Service Fabric resource provider, SFRP), um zum Abruf von Informationen über Knoten und Anwendungen einen Cluster aufzurufen.  SFRP erfordert öffentlich zugänglichen eingehenden Zugriff auf den Port für das HTTP-Gateway (standardmäßig&19080;) auf dem Verwaltungsendpunkt, der von [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) zur Verwaltung Ihres Clusters verwendet wird. Dieser Port wird außerdem vom Service Fabric-Ressourcenanbieter dazu verwendet, Informationen über Ihren Cluster zur Anzeige im Azure-Portal abzufragen.  Wenn vom SFRP nicht auf diesen Port zugegriffen werden kann, wird im Verwaltungsportal eine Nachricht wie „Nodes Not Found“ (Knoten nicht gefunden) angezeigt, und die Liste der Knoten und Anwendungen ist leer.  Wenn Sie Ihren Cluster über das Azure-Portal anzeigen lassen möchten, muss Ihr Lastenausgleich eine öffentliche IP-Adresse bereitstellen, und die NSG muss über 19080 eingehenden Datenverkehr zulassen.  Wenn diese Anforderungen nicht erfüllt werden, zeigt das Azure-Portal den aktuellen Status Ihres Clusters nicht an.  Ihr Cluster ist andernfalls nicht betroffen, und Sie können [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) verwenden, um den aktuellen Status abzurufen. Je nach Ihren Netzwerkanforderungen stellt dies vermutlich eine akzeptable Einschränkung dar.  Es handelt sich hierbei um eine temporäre Einschränkung, die wir voraussichtlich in einem späteren Update beheben werden. Zu dieser Zeit ist Ihr Cluster nicht ohne Verlust der Funktionalität des Verwaltungsportals öffentlich zugänglich.

## <a name="templates"></a>Vorlagen

Alle Vorlagen finden Sie [hier](https://msdnshared.blob.core.windows.net/media/2016/10/SF_Networking_Templates.zip). Sie sollten in der Lage sein, die Vorlagen mithilfe der folgenden PowerShell-Befehle ohne Änderungen bereitzustellen.  Stellen Sie sicher, dass Sie zuerst die Schritte im Abschnitt [Anfangssetup](#initialsetup) befolgen, wenn Sie die vorhandene Vorlage des VNETs oder der statischen öffentlichen IP-Adresse bereitstellen.

<a id="initialsetup"></a>
## <a name="initial-setup"></a>Anfangssetup

### <a name="existing-virtual-network"></a>Vorhandenes virtuelles Netzwerk

Ich beginne mit einem vorhandenen virtuellen Netzwerk mit dem Namen „ExistingRG-vnet“ in der Ressourcengruppe *ExistingRG* mit einem Subnetz namens „Standard“.  Hierbei handelt es sich um die Standardressourcen, die erstellt werden, wenn mithilfe des Azure-Portals ein standardmäßiger virtueller Computer erstellt wird.  Sie können das VNET und das Subnetz auch ohne den virtuellen Computer erstellen. Das Hauptziel beim Hinzufügen eines Clusters zu einem bestehenden VNET ist es aber, Netzwerkkonnektivität mit anderen VMs herzustellen, daher ist das Erstellen einer VM ein konkretes Beispiel dafür, wie die übliche Verwendung erfolgt.  Wenn Ihr Service Fabric-Cluster nur einen internen Lastenausgleich ohne öffentliche IP-Adresse verwendet, kann die VM mit öffentlicher IP-Adresse als „Jumpbox“ verwendet werden.

### <a name="static-public-ip-address"></a>Statische öffentliche IP-Adressen

Da es sich bei einer statischen öffentlichen IP-Adresse im Allgemeinen um eine dedizierte Ressource handelt, die separat von der VM oder den VMs, denen sie zugewiesen ist, verwaltet wird, wird sie in einer dedizierten Netzwerkressourcengruppe (anstatt innerhalb der Ressourcengruppe des Service Fabric-Clusters selbst) bereitgestellt.  Erstellen Sie entweder über das Azure-Portal oder PowerShell eine statische öffentliche IP-Adresse mit dem Namen „staticIP1“ in der gleichen Ressourcengruppe *ExistingRG*:

```powershell
PS C:\Users\user> New-AzureRmPublicIpAddress -Name staticIP1 -ResourceGroupName ExistingRG -Location westus -AllocationMethod Static -DomainNameLabel sfnetworking

Name                     : staticIP1
ResourceGroupName        : ExistingRG
Location                 : westus
Id                       : /subscriptions/1237f4d2-3dce-1236-ad95-123f764e7123/resourceGroups/ExistingRG/providers/Microsoft.Network/publicIPAddresses/staticIP1
Etag                     : W/"fc8b0c77-1f84-455d-9930-0404ebba1b64"
ResourceGuid             : 77c26c06-c0ae-496c-9231-b1a114e08824
ProvisioningState        : Succeeded
Tags                     :
PublicIpAllocationMethod : Static
IpAddress                : 40.83.182.110
PublicIpAddressVersion   : IPv4
IdleTimeoutInMinutes     : 4
IpConfiguration          : null
DnsSettings              : {
                             "DomainNameLabel": "sfnetworking",
                             "Fqdn": "sfnetworking.westus.cloudapp.azure.com"
                           }
```

### <a name="service-fabric-template"></a>Service Fabric-Vorlage

Ich verwende die Service Fabric-Vorlage template.json, die vor dem Erstellen eines Clusters mit dem Standardassistenten des Portals aus dem Portal heruntergeladen werden kann. Sie können auch eine der Vorlagen im [Vorlagenkatalog](https://azure.microsoft.com/en-us/documentation/templates/?term=service+fabric) verwenden, z.B. [Five node Service Fabric cluster](https://azure.microsoft.com/en-us/documentation/templates/service-fabric-unsecure-cluster-5-node-1-nodetype/) (Service Fabric-Cluster mit fünf Knoten).

<a id="existingvnet"></a>
## <a name="existing-virtual-networksubnet"></a>Vorhandenes virtuelles Netzwerk / Subnetz

[24. Januar 2016: Sie finden ein weiteres Beispiel außerhalb des Service Fabric-Bereichs unter [https://github.com/gbowerman/azure-myriad/tree/master/existing-vnet](https://github.com/gbowerman/azure-myriad/tree/master/existing-vnet)]

1. Ändern Sie den Parameter des Subnetzes in den Namen des vorhandenen Subnetzes, und fügen Sie zwei weitere Parameter hinzu, um auf das vorhandene VNET zu verweisen:

    ```
        "subnet0Name": {
                "type": "string",
                "defaultValue": "default"
            },
            "existingVNetRGName": {
                "type": "string",
                "defaultValue": "ExistingRG"
            },

            "existingVNetName": {
                "type": "string",
                "defaultValue": "ExistingRG-vnet"
            },
            /*
            "subnet0Name": {
                "type": "string",
                "defaultValue": "Subnet-0"
            },
            "subnet0Prefix": {
                "type": "string",
                "defaultValue": "10.0.0.0/24"
            },*/
    ```


2. Ändern Sie die Variable *vnetID* so, dass sie auf das vorhandene VNET zeigt:

    ```
            /*old "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]",*/
            "vnetID": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', parameters('existingVNetRGName'), '/providers/Microsoft.Network/virtualNetworks/', parameters('existingVNetName'))]",
    ```

3. Entfernen Sie *Microsoft.Network/virtualNetworks* aus den Ressourcen, damit Azure kein neues VNET erstellt:

    ```
    /*{
    "apiVersion": "[variables('vNetApiVersion')]",
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[parameters('virtualNetworkName')]",
    "location": "[parameters('computeLocation')]",
    "properities": {
        "addressSpace": {
            "addressPrefixes": [
                "[parameters('addressPrefix')]"
            ]
        },
        "subnets": [
            {
                "name": "[parameters('subnet0Name')]",
                "properties": {
                    "addressPrefix": "[parameters('subnet0Prefix')]"
                }
            }
        ]
    },
    "tags": {
        "resourceType": "Service Fabric",
        "clusterName": "[parameters('clusterName')]"
    }
    },*/
    ```

4. Kommentieren Sie das VNET über das Attribut *dependsOn* von *Microsoft.Compute/virtualMachineScaleSets* aus, damit wir kein neues VNET erstellen müssen:

    ```
    "apiVersion": "[variables('vmssApiVersion')]",
    "type": "Microsoft.Computer/virtualMachineScaleSets",
    "name": "[parameters('vmNodeType0Name')]",
    "location": "[parameters('computeLocation')]",
    "dependsOn": [
        /*"[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]",
        */
        "[Concat('Microsoft.Storage/storageAccounts/', variables('uniqueStringArray0')[0])]",

    ```

5. Stellen Sie die Vorlage bereit:

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkingexistingvnet -Location westus
    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkingexistingvnet -TemplateFile C:\SFSamples\Final\template\_existingvnet.json
    ```

    Nach der Bereitstellung sollte Ihr virtuelles Netzwerk die neuen Skalierungsgruppen-VMs enthalten, und der Knotentyp „VM-Skalierungsgruppe“ sollte das vorhandene VNET und das Subnetz anzeigen.  Sie können auch eine RDP-Verbindung mit der vorhandenen VM herstellen, die sich bereits im VNET befand, und die neuen Skalierungsgruppen-VMs pinnen:

    ```
    C:>\Users\users>ping 10.0.0.5 -n 1
    C:>\Users\users>ping NOde1000000 -n 1
    ```

<a id="staticpublicip"></a>
## <a name="static-public-ip-address"></a>Statische öffentliche IP-Adressen

1. Fügen Sie Parameter für den Namen der Ressourcengruppe der statischen IP-Adresse, den Namen und den vollqualifizierten Domänennamen (FQDN) hinzu:

    ```
    "existingStaticIPResourceGroup": {
                "type": "string"
            },
            "existingStaticIPName": {
                "type": "string"
            },
            "existingStaticIPDnsFQDN": {
                "type": "string"
    }
    ```

2. Löschen Sie den Parameter *dnsName*, da die statische IP-Adresse bereits über einen DNS-Namen verfügt:

    ```
    /*
    "dnsName": {
        "type": "string"
    }, 
    */
    ```

3. Fügen Sie eine Variable hinzu, um auf die vorhandene statische IP-Adresse zu verweisen:

    ```
    "existingStaticIP": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', parameters('existingStaticIPResourceGroup'), '/providers/Microsoft.Network/publicIPAddresses/', parameters('existingStaticIPName'))]",
    ```

4. Entfernen Sie *Microsoft.Network/publicIPAddresses* aus den *Ressourcen*, damit Azure keine neue IP-Adresse erstellt:

    ```
    /*
    {
        "apiVersion": "[variables('publicIPApiVersion')]",
        "type": "Microsoft.Network/publicIPAddresses",
        "name": "[concat(parameters('lbIPName'),)'-', '0')]",
        "location": "[parameters('computeLocation')]",
        "properties": {
            "dnsSettings": {
                "domainNameLabel": "[parameters('dnsName')]"
            },
            "publicIPAllocationMethod": "Dynamic"        
        },
        "tags": {
            "resourceType": "Service Fabric",
            "clusterName": "[parameters('clusterName')]"
        }
    }, */
    ```

5. Kommentieren Sie die IP-Adresse über das Attribut *dependsOn* von *Microsoft.Network/loadBalancers* aus, damit keine neue IP-Adresse erstellt werden muss:

    ```
    "apiVersion": "[variables('lbIPApiVersion')]",
    "type": "Microsoft.Network/loadBalancers",
    "name": "[concat('LB', '-', parameters('clusterName'), '-', parameters('vmNodeType0Name'))]",
    "location": "[parameters('computeLocation')]",
    /*
    "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', concat(parameters('lbIPName'), '-', '0'))]"
    ], */
    "properties": {
    ```

6. Ändern Sie das Element *publicIPAddress* von *frontendIPConfigurations* in der Ressource *Microsoft.Network/loadBalancers*, um auf die vorhandene statische IP-Adresse statt auf eine neu erstellte zu verweisen:

    ```
                "frontendIPConfigurations": [
                        {
                            "name": "LoadBalancerIPConfig",
                            "properties": {
                                "publicIPAddress": {
                                    /*"id": "[resourceId('Microsoft.Network/publicIPAddresses',concat(parameters('lbIPName'),'-','0'))]"*/
                                    "id": "[variables('existingStaticIP')]"
                                }
                            }
                        }
                    ],
    ```

7. Ändern Sie *managementEndpoint* in der Ressource *Microsoft.ServiceFabric/clusters* zu den DNS-FQDN der statischen IP-Adresse.  **Wenn Sie einen sicheren Cluster verwenden, stellen Sie sicher, dass Sie „http://“ zu „https://“ ändern.** (Hinweis: Diese Anweisung gilt nur für Service Fabric-Cluster.  Wenn Sie eine VM-Skalierungsgruppe verwenden, überspringen Sie diesen Schritt):

    ```
                    "fabricSettings": [],
                    /*"managementEndpoint": "[concat('http://',reference(concat(parameters('lbIPName'),'-','0')).dnsSettings.fqdn,':',parameters('nt0fabricHttpGatewayPort'))]",*/
                    "managementEndpoint": "[concat('http://',parameters('existingStaticIPDnsFQDN'),':',parameters('nt0fabricHttpGatewayPort'))]",
    ```

8. Stellen Sie die Vorlage bereit:

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkingstaticip -Location westus

    $staticip = Get-AzureRmPublicIpAddress -Name staticIP1 -ResourceGroupName ExistingRG

    $staticip

    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkingstaticip -TemplateFile C:\SFSamples\Final\template\_staticip.json -existingStaticIPResourceGroup $staticip.ResourceGroupName -existingStaticIPName $staticip.Name -existingStaticIPDnsFQDN $staticip.DnsSettings.Fqdn
    ```

Nach der Bereitstellung sehen Sie, dass Ihr Lastenausgleich an die öffentliche statische IP-Adresse aus der anderen Ressourcengruppe gebunden ist. Der Service Fabric-Clientverbindungsendpunkt und der [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)-Endpunkt zeigen beide auf den DNS-FDQN der statischen IP-Adresse.

<a id="internallb"></a>
## <a name="internal-only-load-balancer"></a>Rein interner Lastenausgleich

Dieses Szenario ersetzt den externen Lastenausgleich in der standardmäßigen Service Fabric-Vorlage durch einen rein internen Lastenausgleich.  Weiter oben finden Sie Auswirkungen auf das Azure-Portal und SFRP.

1. Entfernen Sie den Parameter *dnsName*, da er nicht benötigt wird:

    ```
    /*
    "dnsName": {
        "type": "string"
    },
    */
    ```

2. Fügen Sie optional einen Parameter für die statische IP-Adresse hinzu, wenn Sie die statische Zuordnungsmethode verwenden. Bei der dynamischen Zuordnungsmethode wird Folgendes nicht benötigt:

    ```
            "internalLBAddress": {
                "type": "string",
                "defaultValue": "10.0.0.250"
            }
    ```

3. Entfernen Sie *Microsoft.Network/publicIPAddresses* aus den Ressourcen, damit Azure keine neue IP-Adresse erstellt:

    ```
    /*
    {
        "apiVersion": "[variables('publicIPApiVersion')]",
        "type": "Microsoft.Network/publicIPAddresses",
        "name": "[concat(parameters('lbIPName'),)'-', '0')]",
        "location": "[parameters('computeLocation')]",
        "properties": {
            "dnsSettings": {
                "domainNameLabel": "[parameters('dnsName')]"
            },
            "publicIPAllocationMethod": "Dynamic"        
        },
        "tags": {
            "resourceType": "Service Fabric",
            "clusterName": "[parameters('clusterName')]"
        }
    }, */
    ```

4. Entfernen Sie das Attribut *dependsOn* der IP-Adresse von *Microsoft.Network/loadBalancers*, damit wir keine neue IP-Adresse erstellen müssen.  Fügen Sie das Attribut *dependsOn* des VNETs hinzu, da der Lastenausgleich jetzt vom Subnetz des VNETs abhängt:

    ```
                "apiVersion": "[variables('lbApiVersion')]",
                "type": "Microsoft.Network/loadBalancers",
                "name": "[concat('LB','-', parameters('clusterName'),'-',parameters('vmNodeType0Name'))]",
                "location": "[parameters('computeLocation')]",
                "dependsOn": [
                    /*"[concat('Microsoft.Network/publicIPAddresses/',concat(parameters('lbIPName'),'-','0'))]"*/
                    "[concat('Microsoft.Network/virtualNetworks/',parameters('virtualNetworkName'))]"
                ],
    ```

5. Ändern Sie *frontendIPConfigurations* des Lastenausgleichs von der Verwendung von *publicIPAddress* auf die Verwendung eines Subnetzes und *privateIPAddress*, welche eine festgelegte statische interne IP-Adresse verwendet.  Sie könnten eine dynamische IP-Adresse verwenden, indem Sie das Element *privateIPAdresse* entfernen, und *privateIPAllocationMethod* in „Dynamisch“ ändern.

    ```
                "frontendIPConfigurations": [
                        {
                            "name": "LoadBalancerIPConfig",
                            "properties": {
                                /*
                                "publicIPAddress": {
                                    "id": "[resourceId('Microsoft.Network/publicIPAddresses',concat(parameters('lbIPName'),'-','0'))]"
                                } */
                                "subnet" :{
                                    "id": "[variables('subnet0Ref')]"
                                },
                                "privateIPAddress": "[parameters('internalLBAddress')]",
                                "privateIPAllocationMethod": "Static"
                            }
                        }
                    ],
    ```

6. Ändern Sie in der Ressource *Microsoft.ServiceFabric/clusters* *managementEndpoint*, um auf die Adresse des internen Lastenausgleichs zu zeigen.  **Wenn Sie einen sicheren Cluster verwenden, stellen Sie sicher, dass Sie „http://“ zu „https://“ ändern.** (Hinweis: Diese Anweisung gilt nur für Service Fabric-Cluster.  Wenn Sie eine VM-Skalierungsgruppe verwenden, überspringen Sie diesen Schritt):

    ```
                    "fabricSettings": [],
                    /*"managementEndpoint": "[concat('http://',reference(concat(parameters('lbIPName'),'-','0')).dnsSettings.fqdn,':',parameters('nt0fabricHttpGatewayPort'))]",*/
                    "managementEndpoint": "[concat('http://',reference(variables('lbID0')).frontEndIPConfigurations[0].properties.privateIPAddress,':',parameters('nt0fabricHttpGatewayPort'))]",
    ```

7. Stellen Sie die Vorlage bereit:

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkinginternallb -Location westus

    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkinginternallb -TemplateFile C:\SFSamples\Final\template\_internalonlyLB.json
    ```

Nach der Bereitstellung verwendet Ihr Lastenausgleich die private statische IP-Adresse 10.0.0.250. Wenn sich ein weiterer Computer im gleichen VNET befindet, können Sie auch zum internen [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)-Endpunkt navigieren und sicherstellen, dass er mit einem der Knoten hinter dem Lastenausgleich verbunden ist.

<a id="internalexternallb"></a>
## <a name="internal-and-external-load-balancer"></a>Interner und externer Lastenausgleich

In diesem Szenario wird der vorhandene externe Lastenausgleich mit einem einzigen Knotentyp verwendet und ein zusätzlicher interner Lastenausgleich zum gleichen Knotentyp hinzugefügt.  Ein Back-End-Port, der an Back-End-Adresspools angefügt ist, kann nur einem einzelnen Lastenausgleich zugewiesen werden. Daher müssen Sie entscheiden, welcher Lastenausgleich über Ihre Anwendungsports und welcher Lastenausgleich über Ihre Verwaltungsendpunkte (Port 19000/19080) verfügen soll.  Bedenken Sie die oben aufgeführten Einschränkungen für SFRP, wenn Sie Verwaltungsendpunkte in den internen Lastenausgleich setzen möchten.  In diesem Beispiel bleiben die Verwaltungsendpunkte im externen Lastenausgleich. Außerdem wird ein Port 80-Anwendungsport hinzugefügt und im internen Lastenausgleich platziert.

Wenn Sie einen Cluster mit zwei Knotentypen erstellen möchten, bei dem sich ein Knotentyp auf dem externen Lastenausgleich und der andere auf dem internen Lastenausgleich befindet, verwenden Sie die Vorlage des über das Portal erstellten Knotentyps mit zwei Knoten (die mit 2 Lastenausgleichen geliefert wird), und wandeln Sie den zweiten Lastenausgleich in einen internen Lastenausgleich um, wie im obigen Abschnitt „Rein interner Lastenausgleich“ beschrieben.

1. Fügen Sie einen Parameter „LB“ für die statische interne IP-Adresse hinzu (für eine dynamische IP-Adresse siehe obige Anmerkungen):

    ```
            "internalLBAddress": {
                "type": "string",
                "defaultValue": "10.0.0.250"
            }
    ```

2. Fügen Sie den Parameter für den Anwendungsport 80 hinzu:

3. Fügen Sie mit Kopieren/Einfügen und Anfügen von „-Int“ den Namen interne Versionen von vorhandenen Netzwerkvariablen hinzu:

    ```
    /* Add internal LB networking variables */
            "lbID0-Int": "[resourceId('Microsoft.Network/loadBalancers', concat('LB','-', parameters('clusterName'),'-',parameters('vmNodeType0Name'), '-Internal'))]",
            "lbIPConfig0-Int": "[concat(variables('lbID0-Int'),'/frontendIPConfigurations/LoadBalancerIPConfig')]",
            "lbPoolID0-Int": "[concat(variables('lbID0-Int'),'/backendAddressPools/LoadBalancerBEAddressPool')]",
            "lbProbeID0-Int": "[concat(variables('lbID0-Int'),'/probes/FabricGatewayProbe')]",
            "lbHttpProbeID0-Int": "[concat(variables('lbID0-Int'),'/probes/FabricHttpGatewayProbe')]",
            "lbNatPoolID0-Int": "[concat(variables('lbID0-Int'),'/inboundNatPools/LoadBalancerBEAddressNatPool')]",
            /* internal LB networking variables end */
    ```

4. Wenn Sie mit einer vom Portal generierten Vorlage mit Anwendungsport 80 beginnen, wird von der Standard-Portalvorlage *AppPort1* (Port 80) an den externen Lastenausgleich angefügt.  Entfernen Sie in diesem Fall „AppPort1“ vom externen Lastenausgleich *loadBalancingRules* und von Tests, damit Sie es später zum internen Lastenausgleich hinzufügen können:

    ```
    "loadBalancingRules": [
        {
            "name": "LBHttpRule",
            "properties":{
                "backendAddressPool": {
                    "id": "[variables('lbPoolID0')]"
                },
                "backendPort": "[parameters('nt0fabricHttpGatewayPort')]",
                "enableFloatingIP": "false",
                "frontendIPConfiguration": {
                    "id": "[variables('lbIPConfig0')]"            
                },
                "frontendPort": "[parameters('nt0fabricHttpGatewayPort')]",
                "idleTimeoutInMinutes": "5",
                "probe": {
                    "id": "[variables('lbHttpProbeID0')]"
                },
                "protocol": "tcp"
            }
        } /* remove the AppPort1 from the external LB,
        {
            "name": "AppPortLBRule1",
            "properties": {
                "backendAddressPool": {
                    "id": "[variables('lbPoolID0')]"
                },
                "backendPort": "[parameters('loadBalancedAppPort1')]",
                "enableFloatingIP": "false",
                "frontendIPConfiguration": {
                    "id": "[variables('lbIPConfig0')]"            
                },
                "frontendPort": "[parameters('loadBalancedAppPort1')]",
                "idleTimeoutInMinutes": "5",
                "probe": {
                    "id": "[concate(variables('lbID0'), '/probes/AppPortProbe1')]"
                },
                "protocol": "tcp"
            }
        }*/

    ],
    "probes": [
        {
            "name": "FabricGatewayProbe", 
            "properties": {
                "intervalInSeconds": 5,
                "numberOfProbes": 2,
                "port": "[parameters('nt0fabricTcpGatewayPort')]",
                "protocol": "tcp"
            }
        },
        {
            "name": "FabricHttpGatewayProbe",
            "properties": {
                "intervalInSeconds": 5,
                "numberOfProbes": 2,
                "port": "[parameters('nt0fabricHttpGatewayPort')]",
                "protocol": "tcp"
            }
        } /* remove the AppPort1 from the external LB,,
        {
            "name": "AppPortProbe1",
            "properties": {
                "intervalInSeconds": 5,
                "numberOfProbes": 2,
                "port": "[parameters('loadBalancedAppPort1')]",
                "protocol": "tcp"
            }
        } */

    ], 
    "inboundNatPools": [
    ```

5. Fügen Sie eine zweite Ressource *Microsoft.Network/loadBalancers* hinzu, die dem internen Lastenausgleich sehr ähnlich ist, den wir im Abschnitt [Rein interner Lastenausgleich](#internallb) erstellt haben. Diese Ressource verwendet allerdings nur die Lastenausgleichsvariable „-Int“ und implementiert nur den Anwendungsport 80.  Hierdurch wird auch *inboundNatPools* gelöscht, um RDP-Endpunkte im öffentlichen Lastenausgleich zu halten – wenn Sie RDP im internen Lastenausgleich haben möchten, verschieben Sie *inboundNATPools* aus dem externen Lastenausgleich in diesen internen Lastenausgleich.

    ```
            /* Add a second load balancer, configured with a static privateIPAddress and the "-Int" LB variables */
            {
                "apiVersion": "[variables('lbApiVersion')]",
                "type": "Microsoft.Network/loadBalancers",
                /* Add '-Internal' to name */
                "name": "[concat('LB','-', parameters('clusterName'),'-',parameters('vmNodeType0Name'), '-Internal')]",
                "location": "[parameters('computeLocation')]",
                "dependsOn": [
                    /* Remove public IP dependsOn, add vnet dependsOn
                    "[concat('Microsoft.Network/publicIPAddresses/',concat(parameters('lbIPName'),'-','0'))]"
                    */
                    "[concat('Microsoft.Network/virtualNetworks/',parameters('virtualNetworkName'))]"
                ],
                "properties": {
                    "frontendIPConfigurations": [
                        {
                            "name": "LoadBalancerIPConfig",
                            "properties": {
                                /* Switch from Public to Private IP address
                                */
                                "publicIPAddress": {
                                    "id": "[resourceId('Microsoft.Network/publicIPAddresses',concat(parameters('lbIPName'),'-','0'))]"
                                }
                                */
                                "subnet" :{
                                    "id": "[variables('subnet0Ref')]"
                                },
                                "privateIPAddress": "[parameters('internalLBAddress')]",
                                "privateIPAllocationMethod": "Static"
                            }
                        }
                    ],
                    "backendAddressPools": [
                        {
                            "name": "LoadBalancerBEAddressPool",
                            "properties": {}
                        }
                    ],
                    "loadBalancingRules": [
                        /* Add the AppPort rule, making sure to reference the "-Int" versions of the backendAddressPool, frontendIPConfiguration, and probe variables */
                        {
                            "name": "AppPortLBRule1",
                            "properties": {
                                "backendAddressPool": {
                                    "id": "[variables('lbPoolID0-Int')]"
                                },
                                "backendPort": "[parameters('loadBalancedAppPort1')]",
                                "enableFloatingIP": "false",
                                "frontendIPConfiguration": {
                                    "id": "[variables('lbIPConfig0-Int')]"
                                },
                                "frontendPort": "[parameters('loadBalancedAppPort1')]",
                                "idleTimeoutInMinutes": "5",
                                "probe": {
                                    "id": "[concat(variables('lbID0-Int'),'/probes/AppPortProbe1')]"
                                },
                                "protocol": "tcp"
                            }
                        }
                    ],
                    "probes": [
                    /* Add the probe for the app port */
                    {
                            "name": "AppPortProbe1",
                            "properties": {
                                "intervalInSeconds": 5,
                                "numberOfProbes": 2,
                                "port": "[parameters('loadBalancedAppPort1')]",
                                "protocol": "tcp"
                            }
                        }
                    ],
                    "inboundNatPools": [
                    ]
                },
                "tags": {
                    "resourceType": "Service Fabric",
                    "clusterName": "[parameters('clusterName')]"
                }
            },
    ```

6. Fügen Sie im Netzwerkprofil *networkProfile* der Ressource *Microsoft.Compute/virtualMachineScaleSets* den internen Back-End-Adresspool hinzu:

    ```
    "loadBalancerBackendAddressPools": [
                                                        {
                                                            "id": "[variables('lbPoolID0')]"
                                                        },
                                                        {
                                                            /* Add internal BE pool */
                                                            "id": "[variables('lbPoolID0-Int')]"
                                                        }
    ],
    ```

7. Stellen Sie die Vorlage bereit:

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkinginternalexternallb -Location westus

    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkinginternalexternallb -TemplateFile C:\SFSamples\Final\template\_internalexternalLB.json
    ```

Nach der Bereitstellung befinden sich zwei Lastenausgleiche in der Ressourcengruppe. Wenn Sie in den Lastenausgleichen suchen, sehen Sie die öffentliche IP-Adresse und die Verwaltungsendpunkte (Port 19000/19080) sowie die statische interne IP-Adresse und den Verwaltungsendpunkt (Port 80). Beide Lastenausgleiche verwenden den gleichen Back-End-Pool der VM-Skalierungsgruppe.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie mehr über das Integrieren von Service Fabric Clustern mithilfe der Netzwerkfeatures von Azure erfahren haben, fahren Sie mit dem [Erstellen eines Clusters](service-fabric-cluster-creation-via-arm.md) fort. 

