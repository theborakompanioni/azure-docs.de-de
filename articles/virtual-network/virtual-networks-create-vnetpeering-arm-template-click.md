---
title: Erstellen des VNet-Peerings mit Resource Manager-Vorlagen | Microsoft Docs
description: Es wird beschrieben, wie Sie das VNet-Peering mit den Vorlagen in Resource Manager erstellen.
services: virtual-network
documentationcenter: ''
author: narayanannamalai
manager: jefco
editor: ''
tags: azure-resource-manager

ms.service: virtual-network
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2016
ms.author: narayanannamalai;annahar

---
# Erstellen des VNet-Peerings mit Resource Manager-Vorlagen
[!INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[!INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Führen Sie die folgenden Schritte aus, um ein VNet-Peering mit Resource Manager-Vorlagen zu erstellen:

1. Wenn Sie Azure PowerShell zuvor noch nicht verwendet haben, lesen Sie [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md), und befolgen Sie die komplette Anleitung, um sich bei Azure anzumelden und Ihr Abonnement auszuwählen.
   
   > [!NOTE]
   > Das PowerShell-Cmdlet zum Verwalten des VNet-Peerings ist in [Azure PowerShell 1.6](http://www.powershellgallery.com/packages/Azure/1.6.0) enthalten.
   > 
   > 
2. Im Text weiter unten finden Sie die Definition eines VNet-Peeringlinks für „VNet1 zu VNet2“ (auf der Grundlage des obigen Szenarios). Kopieren Sie den Inhalt weiter unten, und speichern Sie ihn in einer Datei namens „VNetPeeringVNet1.json“.
   
        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet1/LinkToVNet2",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet2')]"       
        }
            }
            }
        ]
        }
3. Der Abschnitt weiter unten enthält die Definition eines VNet-Peeringlinks für „VNet2 zu VNet1“ (auf der Grundlage des obigen Szenarios). Kopieren Sie den Inhalt weiter unten, und speichern Sie ihn in einer Datei namens „VNetPeeringVNet2.json“.
   
        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet2/LinkToVNet1",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet1')]"       
                }
            }
            }
        ]
        }
   
    Wie in der obigen Vorlage zu sehen, können für das VNet-Peering einige Eigenschaften konfiguriert werden:
   
   | Option | Beschreibung | Standard |
   |:--- |:--- |:--- |
   | AllowVirtualNetworkAccess |Gibt an, ob der Adressraum eines mittels Peering verknüpften VNets als Teil des Tags „virtual\_network“ einbezogen wird. |Ja |
   | AllowForwardedTraffic |Gibt an, ob Datenverkehr, der nicht aus dem mittels Peering verknüpften VNet stammt, akzeptiert oder verworfen wird. |Nein |
   | AllowGatewayTransit |Ermöglicht dem mittels Peering verknüpften VNet die Verwendung des VNet-Gateways. |Nein |
   | UseRemoteGateways |Gibt an, dass das VNet-Gateway des per Peering verknüpften VNet verwendet wird. Für das verknüpfte VNet muss ein Gateway konfiguriert sein, und „AllowGatewayTransit“ muss ausgewählt sein. Diese Option kann nicht verwendet werden, wenn Sie ein Gateway konfiguriert haben. |Nein |
   
    Jeder Link in einem VNet-Peering verfügt über die oben angegebenen Eigenschaften. So können Sie beispielsweise die Eigenschaft „AllowVirtualNetworkAccess“ beim VNet-Peeringlink für „VNet1 zu VNet2“ auf „true“ und beim VNet-Peeringlink für die Gegenrichtung auf „false“ festlegen.
4. Zum Bereitstellen der Vorlagendatei können Sie das Cmdlet „New-AzureRmResourceGroupDeployment“ ausführen und die Bereitstellung erstellen oder aktualisieren. Weitere Informationen zur Verwendung von Resource Manager-Vorlagen finden Sie in [diesem Artikel](../resource-group-template-deploy.md).
   
        New-AzureRmResourceGroupDeployment -ResourceGroupName <resource group name> -TemplateFile <template file path> -DeploymentDebugLogLevel all
   
   > [!NOTE]
   > Ersetzen Sie den Ressourcengruppennamen und die Vorlagendatei gemäß Ihren Anforderungen.
   > 
   > 
   
    Unten ist ein Beispiel angegeben, das auf dem obigen Szenario basiert:
   
        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet1.json -DeploymentDebugLogLevel all
   
    Ausgabe:
   
        DeploymentName        : VNetPeeringVNet1
        ResourceGroupName    : VNet101
        ProvisioningState        : Succeeded
        Timestamp            : 7/26/2016 9:05:03 AM
        Mode            : Incremental
        TemplateLink        :
        Parameters            :
        Outputs            :
        DeploymentDebugLogLevel : RequestContent, ResponseContent
   
        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet2.json -DeploymentDebugLogLevel all
   
    Ausgabe:
   
        DeploymentName        : VNetPeeringVNet2
        ResourceGroupName    : VNet101
        ProvisioningState        : Succeeded
        Timestamp            : 7/26/2016 9:07:22 AM
        Mode            : Incremental
        TemplateLink        :
        Parameters            :
        Outputs            :
        DeploymentDebugLogLevel : RequestContent, ResponseContent
5. Nach Abschluss der Bereitstellung können Sie das unten angegebene Cmdlet ausführen, um den Peeringstatus anzuzeigen:
   
        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName VNet1 -ResourceGroupName VNet101 -Name linktoVNet2
   
    Ausgabe:
   
        Name            : LinkToVNet2
        Id                : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VNet101/providers/Microsoft.Network/virtualNetworks/VNet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName    : VNet101
        VirtualNetworkName    : VNet1
        ProvisioningState        : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VNet101/providers/Microsoft.Network/virtualNetworks/VNet2"
                                        }
        AllowVirtualNetworkAccess    : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null
   
    Nachdem das Peering in diesem Szenario eingerichtet wurde, können Sie die Verbindung zwischen beliebigen virtuellen Computern in beiden VNets initiieren. Standardmäßig ist „AllowVirtualNetworkAccess“ auf „true“ festgelegt, und das VNet-Peering stellt die passenden ACLs bereit, um die Kommunikation zwischen VNets zu ermöglichen. Sie können aber trotzdem Netzwerksicherheitsgruppen-Regeln (NSG-Regeln) anwenden, um die Konnektivität zwischen bestimmten Subnetzen oder virtuellen Computern zu blockieren und so eine Feinsteuerung des Zugriffs zwischen den beiden virtuellen Netzwerken zu erreichen. Weitere Informationen zum Erstellen von NSG-Regeln finden Sie in [diesem Artikel](virtual-networks-create-nsg-arm-ps.md).

[!INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

Führen Sie die folgenden Schritte aus, um ein abonnementübergreifendes VNet-Peering zu erstellen:

1. Melden Sie sich bei Azure unter Abonnement A mit einem privilegierten Benutzerkonto für Benutzer A an, und führen Sie das folgende Cmdlet aus:
   
        New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet5
   
    Dies ist nicht zwingend erforderlich. Das Peering kann auch dann eingerichtet werden, wenn Benutzer individuell Peeringanforderungen für ihre jeweiligen VNets auslösen, solange die Anforderungen übereinstimmen. Das Hinzufügen von privilegierten Benutzern des anderen VNets als Benutzer im lokalen VNet vereinfacht aber die Einrichtung.
2. Melden Sie sich bei Azure unter dem Abonnement B mit einem privilegierten Benutzerkonto für Benutzer B an, und führen Sie das folgende Cmdlet aus:
   
        New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet3
3. Führen Sie in der Anmeldesitzung von Benutzer A das folgende Cmdlet aus:
   
        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet3.json -DeploymentDebugLogLevel all
   
    Hier ist angegeben, wie die JSON-Datei definiert wird.
   
        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet3/LinkToVNet5",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/<Subscription-B-ID>/resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/VNet5"
                }
            }
            }
        ]
        }
4. Führen Sie in der Anmeldesitzung von Benutzer B das folgende Cmdlet aus:
   
        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet5.json -DeploymentDebugLogLevel all
   
    Hier ist angegeben, wie die JSON-Datei definiert wird:
   
        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet5/LinkToVNet3",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/Subscription-A-ID /resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/VNet3"
                }
            }
            }
        ]
        }
   
     Nachdem das Peering in diesem Szenario eingerichtet wurde, können Sie die Verbindungen von jedem virtuellen Computer mit jedem virtuellen Computer in beiden VNets über verschiedene Abonnements hinweg initiieren.

[!INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. In diesem Szenario können Sie die weiter unten angegebene Beispielvorlage bereitstellen, um das VNet-Peering einzurichten. Die AllowForwardedTraffic-Eigenschaft muss auf „true“ festgelegt werden, damit das virtuelle Netzwerkgerät im mittels Peering verknüpften VNet Datenverkehr senden und empfangen kann.
   
    Im Anschluss finden Sie die Vorlage zum Erstellen eines VNet-Peerings für „HubVNet zu VNet1“. Beachten Sie, dass „AllowForwardedTraffic“ auf „false“ festgelegt ist.
   
        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "HubVNet/LinkToVNet1",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet1')]"       
                }
            }
            }
            }
        ]
        }
2. Im Anschluss finden Sie die Vorlage zum Erstellen eines VNet-Peerings für „VNet1 zu HubVNet“. Beachten Sie, dass „AllowForwardedTraffic“ auf „true“ festgelegt ist.
   
        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet1/LinkToHubVNet",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": true,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'HubVnet')]"       
                }
            }
            }
        ]
        }
3. Nachdem das Peering eingerichtet wurde, können Sie [diesen Artikel](virtual-network-create-udr-arm-ps.md) lesen und eine benutzerdefinierte Route (User-Defined Route, UDR) festlegen, um VNet1-Datenverkehr über ein virtuelles Gerät zu leiten und dessen Funktionen zu nutzen. Wenn Sie die Adresse des nächsten Hops der Route angeben, können Sie diese auf die IP-Adresse des virtuellen Geräts im mittels Peering verknüpften VNet (HubVNet) festlegen.

[!INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]

Führen Sie die folgenden Schritte aus, um ein Peering zwischen virtuellen Netzwerken aus unterschiedlichen Bereitstellungsmodellen zu erstellen:

1. Im Text weiter unten ist die Definition eines VNet-Peeringlinks zwischen VNET1 und VNET2 für dieses Szenario enthalten. Es ist nur ein Link erforderlich, um ein klassisches virtuelles Netzwerk mit einem virtuellen Azure Resource Manager-Netzwerk zu verknüpfen.
   
    Fügen Sie Ihre Abonnement-ID ein, um anzugeben, wo sich das klassische virtuelle Netzwerk bzw. VNET2 befindet, und ändern Sie MyResourceGroup in den entsprechenden Ressourcengruppennamen.
   
    { "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#", "contentVersion": "1.0.0.0", "parameters": { }, "variables": { }, "resources": [ { "apiVersion": "2016-06-01", "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings", "name": "VNET1/LinkToVNET2", "location": "[resourceGroup().location]", "properties": { "allowVirtualNetworkAccess": true, "allowForwardedTraffic": false, "allowGatewayTransit": false, "useRemoteGateways": false, "remoteVirtualNetwork": { "id": "[resourceId('Microsoft.ClassicNetwork/virtualNetworks', 'VNET2')]" } } } ] }
2. Führen Sie zum Bereitstellen der Vorlagendatei das folgende Cmdlet aus, um die Bereitstellung zu erstellen oder zu aktualisieren.
   
        New-AzureRmResourceGroupDeployment -ResourceGroupName MyResourceGroup -TemplateFile .\VnetPeering.json -DeploymentDebugLogLevel all
   
        Output shows:
   
        DeploymentName          : VnetPeering
        ResourceGroupName       : MyResourceGroup
        ProvisioningState       : Succeeded
        Timestamp               : XX/XX/YYYY 5:42:33 PM
        Mode                    : Incremental
        TemplateLink            :
        Parameters              :
        Outputs                 :
        DeploymentDebugLogLevel : RequestContent, ResponseContent
3. Nachdem die Bereitstellung erfolgreich abgeschlossen wurde, können Sie das folgende Cmdlet ausführen, um den Peeringstatus anzuzeigen:
   
        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName VNET1 -ResourceGroupName MyResourceGroup -Name LinkToVNET2
   
        Output shows:
   
        Name                             : LinkToVNET2
        Id                               : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResource
                                   Group/providers/Microsoft.Network/virtualNetworks/VNET1/virtualNetworkPeering
                                   s/LinkToVNET2
        Etag                             : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName                : MyResourceGroup
        VirtualNetworkName               : VNET1
        PeeringState                     : Connected
        ProvisioningState                : Succeeded
        RemoteVirtualNetwork             : {
                                     "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/M
                                   yResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2"
                                   }
        AllowVirtualNetworkAccess        : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

Nach dem Peering zwischen einem klassischen VNet und einem Resource Manager-VNet sollte es möglich sein, Verbindungen von beliebigen virtuellen Computern in VNET1 mit virtuellen Computern in VNET2 zu initiieren (und umgekehrt).

<!---HONumber=AcomDC_0921_2016-->