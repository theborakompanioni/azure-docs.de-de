---
title: Erstellen eines virtuellen Netzwerks mit einer Resource Manager-Vorlage | Microsoft Docs
description: Erfahren Sie, wie Sie ein virtuelles Netzwerk mit einer Vorlage erstellen | Ressourcen-Manager.
services: virtual-network
documentationcenter: 
author: jimdial
manager: carmonm
editor: tysonn
tags: azure-resource-manager
ms.assetid: 69530861-2f97-4a6e-b336-a7baf2690044
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
translationtype: Human Translation
ms.sourcegitcommit: c934f78e514230958fad8b2aa9be4d2e56a3a835
ms.openlocfilehash: be3fbc2c14be06cac2b9213ee2da58545e34fc0e


---
# <a name="create-a-virtual-network-using-a-template"></a>Erstellen eines virtuellen Netzwerks mithilfe einer Vorlage

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

Azure verfügt über zwei Bereitstellungsmodelle: Azure Resource Manager und klassisch. Microsoft empfiehlt, Ressourcen mit dem Resource Manager-Bereitstellungsmodell zu erstellen. Weitere Informationen zu den Unterschieden zwischen den beiden Modellen finden Sie im Artikel zum Thema [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md) (Grundlegendes zu Azure-Bereitstellungsmodellen).
 
In diesem Artikel wird beschrieben, wie Sie mit dem Resource Manager-Bereitstellungsmodell mithilfe einer Azure Resource Manager-Vorlage ein VNET erstellen. Sie können ein VNet per Resource Manager auch mit anderen Tools erstellen oder dafür das klassische Bereitstellungsmodell verwenden, indem Sie in der folgenden Liste eine andere Option wählen:

> [!div class="op_single_selector"]
- [Portal](virtual-networks-create-vnet-arm-pportal.md)
- [PowerShell](virtual-networks-create-vnet-arm-ps.md)
- [BEFEHLSZEILENSCHNITTSTELLE (CLI)](virtual-networks-create-vnet-arm-cli.md)
- [Vorlage](virtual-networks-create-vnet-arm-template-click.md)
- [Portal (klassisch)](virtual-networks-create-vnet-classic-pportal.md)
- [PowerShell (klassisch)](virtual-networks-create-vnet-classic-netcfg-ps.md)
- [CLI (klassisch)](virtual-networks-create-vnet-classic-cli.md)

Sie erfahren, wie Sie eine vorhandene ARM-Vorlage von GitHub herunterladen und ändern sowie die Vorlage aus GitHub, PowerShell und der Azure-Befehlszeilenschnittstelle bereitstellen.

Wenn Sie die ARM-Vorlage, ohne sie zu ändern, einfach direkt aus GitHub bereitstellen, springen Sie zu [Bereitstellen einer Vorlage aus Github](#deploy-the-arm-template-by-using-click-to-deploy).

[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

## <a name="download-and-understand-the-azure-resource-manager-template"></a>Herunterladen und Verstehen der Azure-Ressourcen-Manager-Vorlage
Sie können die vorhandene Vorlage zum Erstellen von einem VNET und zwei Subnetzen über GitHub herunterladen, die gewünschten Änderungen vornehmen und die Vorlage anschließend wiederverwenden. Dazu müssen Sie die folgenden Schritte ausführen:

1. Rufen Sie die [Seite mit den Beispielvorlagen](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets)auf.
2. Klicken Sie auf **azuredeploy.json**, und klicken Sie dann auf **RAW**.
3. Speichern Sie die Datei in einem lokalen Ordner auf Ihrem Computer.
4. Wenn Sie mit Vorlagen vertraut sind, fahren Sie mit Schritt 7 fort.
5. Öffnen Sie die Datei, die Sie gerade gespeichert haben, und sehen Sie sich den Inhalt unter **parameters** in Zeile 5 an. ARM-Vorlagenparameter enthalten Platzhalter für Werte, die während der Bereitstellung ausgefüllt werden können.
   
   | Parameter | Beschreibung |
   | --- | --- |
   | **location** |Azure-Region, in der das VNet erstellt wird |
   | **vnetName** |Name für das neue VNet |
   | **addressPrefix** |Adressraum für das VNet im CIDR-Format |
   | **subnet1Name** |Name für das erste VNet |
   | **subnet1Prefix** |CIDR-Block für das erste Subnetz |
   | **subnet2Name** |Name für das zweite VNet |
   | **subnet2Prefix** |CIDR-Block für das zweite Subnetz |
   
   > [!IMPORTANT]
   > Azure-Ressourcen-Manager-Vorlagen, die in GitHub verwaltet werden, können sich im Laufe der Zeit ändern. Überprüfen Sie die Vorlage stets, bevor Sie sie verwenden.
   > 
   > 
6. Überprüfen Sie den Inhalt unter **resources** , und beachten Sie Folgendes:
   
   * **type**. Typ der Ressource, die von der Vorlage erstellt wird. In diesem Fall **Microsoft.Network/virtualNetworks**(ein VNet).
   * **name**. Name der Ressource. Beachten Sie die Verwendung von **[parameters('vnetName')]**, die bedeutet, dass der Name während der Bereitstellung als Eingabe des Benutzers oder durch eine Parameterdatei bereitgestellt wird.
   * **properties**. Liste der Eigenschaften für die Ressource. Diese Vorlage verwendet bei der Erstellung des VNet die Adressraum- und Subnetzeigenschaften.
7. Kehren Sie zur [Seite mit den Beispielvorlagen](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets)zurück.
8. Klicken Sie auf **azuredeploy-paremeters.json** und anschließend auf **RAW**.
9. Speichern Sie die Datei in einem lokalen Ordner auf Ihrem Computer.
10. Öffnen Sie die Datei, die Sie gerade gespeichert haben, und bearbeiten Sie die Parameterwerte. Verwenden Sie die folgenden Werte, um das VNET wie in dem Szenario beschrieben bereitzustellen:

    ```json
        {
          "location": {
            "value": "Central US"
          },
          "vnetName": {
              "value": "TestVNet"
          },
          "addressPrefix": {
              "value": "192.168.0.0/16"
          },
          "subnet1Name": {
              "value": "FrontEnd"
          },
          "subnet1Prefix": {
            "value": "192.168.1.0/24"
          },
          "subnet2Name": {
              "value": "BackEnd"
          },
          "subnet2Prefix": {
              "value": "192.168.2.0/24"
          }
        }
    ```

11. Speichern Sie die Datei .


## <a name="deploy-the-template-using-powershell"></a>Bereitstellen der Vorlage mithilfe von PowerShell

Führen Sie zum Bereitstellen der mithilfe von PowerShell heruntergeladenen Vorlage die unten beschriebenen Schritte aus:

1. Installieren und konfigurieren Sie Azure PowerShell mithilfe der Anweisungen unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs).
2. Führen Sie den folgenden Befehl aus, um eine neue Ressourcengruppe zu erstellen:

    ```powershell
    New-AzureRmResourceGroup -Name TestRG -Location centralus
    ```

    Mit dem Befehl wird eine Ressourcengruppe mit dem Namen *TestRG* in der Azure-Region *USA, Mitte* erstellt. Weitere Informationen zu Ressourcengruppen finden Sie unter [Übersicht über den Azure-Ressourcen-Manager](../azure-resource-manager/resource-group-overview.md).

    Erwartete Ausgabe:

        ResourceGroupName : TestRG
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *
        ResourceId        : /subscriptions/[Id]/resourceGroups/TestRG

3. Führen Sie das folgende Cmdlet aus, um das neue VNET mithilfe der heruntergeladenen und oben geänderten Vorlage und Parameterdateien bereitzustellen:

    ```powershell
    New-AzureRmResourceGroupDeployment -Name TestVNetDeployment -ResourceGroupName TestRG `
    -TemplateFile C:\ARM\azuredeploy.json -TemplateParameterFile C:\ARM\azuredeploy-parameters.json
    ```

    Erwartete Ausgabe:
   
        DeploymentName    : TestVNetDeployment
        ResourceGroupName : TestRG
        ProvisioningState : Succeeded
        Timestamp         : [Date and time]
        Mode              : Incremental
        TemplateLink      :
        Parameters        :
                            Name             Type                       Value
                            ===============  =========================  ==========
                            location         String                     Central US
                            vnetName         String                     TestVNet
                            addressPrefix    String                     192.168.0.0/16
                            subnet1Prefix    String                     192.168.1.0/24
                            subnet1Name      String                     FrontEnd
                            subnet2Prefix    String                     192.168.2.0/24
                            subnet2Name      String                     BackEnd
   
        Outputs           :
4. Führen Sie den folgenden Befehl aus, um die Eigenschaften des neuen VNET anzuzeigen:

    ```powershell
    Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    ```

    Erwartete Ausgabe:

        Name              : TestVNet
        ResourceGroupName : TestRG
        Location          : centralus
        Id                : /subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag              : W/"[Id]"
        ProvisioningState : Succeeded
        Tags              :
        AddressSpace      : {
                              "AddressPrefixes": [
                                "192.168.0.0/16"
                              ]
                            }
        DhcpOptions       : {
                              "DnsServers": null
                            }
        NetworkInterfaces : null
        Subnets           : [
                              {
                                "Name": "FrontEnd",
                                "Etag": "W/\"[Id]\"",
                                "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                "AddressPrefix": "192.168.1.0/24",
                                "IpConfigurations": [],
                                "NetworkSecurityGroup": null,
                                "RouteTable": null,
                                "ProvisioningState": "Succeeded"
                              },
                              {
                                "Name": "BackEnd",
                                "Etag": "W/\"[Id]\"",
                                "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
                                "AddressPrefix": "192.168.2.0/24",
                                "IpConfigurations": [],
                                "NetworkSecurityGroup": null,
                                "RouteTable": null,
                                "ProvisioningState": "Succeeded"
                              }
                            ]

## <a name="deploy-the-template-using-click-to-deploy"></a>Bereitstellen der Vorlage per Click-to-Deploy

Sie können vordefinierte Azure Resource Manager-Vorlagen wiederverwenden, die in ein von Microsoft verwaltetes und für die Community freigegebenes GitHub-Repository hochgeladen wurden. Diese Vorlagen können direkt aus GitHub heraus bereitgestellt oder heruntergeladen und an Ihre Bedürfnisse angepasst werden. Zum Bereitstellen einer Vorlage, die ein VNET mit zwei Subnetzen erstellt, müssen Sie die folgenden Schritte ausführen:

1. Navigieren Sie in einem Browser zu [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).
2. Führen Sie in der Liste der Vorlagen einen Bildlauf nach unten aus, und klicken Sie auf **101-vnet-two-subnets**. Überprüfen Sie die Datei **Readme.MD** , wie unten dargestellt.

    ![READEME.md-Datei in GitHub](./media/virtual-networks-create-vnet-arm-template-click-include/figure1.png)

3. Klicken Sie auf Schaltfläche zum **Bereitstellen in Azure**. Falls erforderlich, geben Sie Ihre Anmeldeinformationen für Azure ein. 
4. Geben Sie auf dem Blatt **Parameter** die Werte ein, die Sie zum Erstellen Ihres neuen VNet verwenden möchten, und klicken Sie dann auf **OK**. Die folgende Abbildung zeigt die Werte für das Szenario:
   
    ![ARM-Vorlagenparameter](./media/virtual-networks-create-vnet-arm-template-click-include/figure2.png)

5. Klicken Sie auf **Ressourcengruppe**, und wählen Sie eine Ressourcengruppe aus, zu der Sie das VNet hinzufügen möchten, oder klicken Sie auf **Neu erstellen**, um das VNet zu einer neuen Ressourcengruppe hinzuzufügen. Die folgende Abbildung zeigt die Ressourcengruppeneinstellungen für eine neue Ressourcengruppe namens **TestRG**:

    ![Ressourcengruppe](./media/virtual-networks-create-vnet-arm-template-click-include/figure3.png)

6. Ändern Sie ggf. die **Abonnement**- und **Speicherort**-Einstellungen für das VNet.
7. Wenn das VNet nicht als Kachel im **Startmenü** angezeigt werden soll, deaktivieren Sie **An Startmenü anheften**.
8. Klicken Sie auf **Rechtliche Bedingungen**, lesen Sie die Bedingungen, und klicken Sie auf **Kaufen**, wenn Sie diesen zustimmen. 
9. Klicken Sie auf **Erstellen**, um das VNet zu erstellen.
   
    ![Senden der Bereitstellungskachel im Vorschauportal](./media/virtual-networks-create-vnet-arm-template-click-include/figure4.png)

10. Nachdem die Bereitstellung abgeschlossen ist, klicken Sie im Azure-Portal auf **Weitere Dienste**, geben Sie *virtuelle Netzwerke* in das Filterfeld ein, das angezeigt wird, und klicken Sie dann auf „virtuelle Netzwerke“, um das Blatt „virtuelle Netzwerke“ anzuzeigen. Klicken Sie auf dem Blatt auf *TestVNet*. Klicken Sie auf dem Blatt *TestVNet* auf **Subnetze**, um die erstellten Subnetze anzuzeigen, wie in der folgenden Abbildung dargestellt:
    
     ![Erstellen von VNet im Vorschauportal](./media/virtual-networks-create-vnet-arm-template-click-include/figure5.png)

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich, wie folgende Verbindungen hergestellt werden:

- Eine Verbindung eines virtuellen Computers (VM) mit einem virtuellen Netzwerk. Lesen Sie hierzu den Artikel [Erstellen eines virtuellen Windows-Computers](../virtual-machines/virtual-machines-windows-hero-tutorial.md) oder [Erstellen eines virtuellen Linux-Computers](../virtual-machines/virtual-machines-linux-quick-create-portal.md). Anstatt gemäß den Schritten in den Artikeln ein VNet und ein Subnetz zu erstellen, können Sie für die Verbindung mit der VM auch ein vorhandenes VNet und Subnetz auswählen.
- Eine Verbindung des virtuellen Netzwerks mit anderen virtuellen Netzwerken. Lesen Sie hierzu den Artikel [Konfigurieren einer VNet-zu-VNet-Verbindung über das Azure-Portal](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).
- Eine Verbindung des virtuellen Netzwerks mit einem lokalen Netzwerk. Verwenden Sie hierfür ein Site-to-Site-VPN oder eine ExpressRoute-Verbindung. Informationen zur Vorgehensweise finden Sie in den Artikeln [Add a Site-to-Site connection to a VNet with an existing VPN gateway connection](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) (Hinzufügen einer Site-to-Site-Verbindung zu einem VNet mit einer vorhandenen VPN-Gatewayverbindung) und [Verknüpfen eines VNet mit einer ExpressRoute-Verbindung](../expressroute/expressroute-howto-linkvnet-arm.md).



<!--HONumber=Jan17_HO1-->


