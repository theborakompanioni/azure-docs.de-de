---
title: 'Verbinden von klassischen virtuellen Netzwerken mit Azure Resource Manager-VNETs: Portal | Microsoft Docs'
description: Hier erfahren Sie, wie Sie mithilfe von VPN Gateway und des Portals eine VPN-Verbindung zwischen klassischen VNets und Resource Manager-VNets erstellen.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 5a90498c-4520-4bd3-a833-ad85924ecaf9
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/04/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 73ee330c276263a21931a7b9a16cc33f86c58a26
ms.openlocfilehash: 3d12aaf19326c45cba1a214f1d68f41f0db8ccf6
ms.lasthandoff: 04/05/2017


---
# <a name="connect-virtual-networks-from-different-deployment-models-using-the-portal"></a>Verbinden von virtuellen Netzwerken aus unterschiedlichen Bereitstellungsmodellen über das Portal
> [!div class="op_single_selector"]
> * [Azure-Portal](vpn-gateway-connect-different-deployment-models-portal.md)
> * [PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
> 
> 

In Azure sind zurzeit zwei Verwaltungsmodelle verfügbar: das klassische Modell und das Resource Manager-Modell. Wenn Sie Azure seit einiger Zeit verwenden, verfügen Sie wahrscheinlich über Azure-VMs und Instanzrollen, die in einem klassischen VNet ausgeführt werden. Ihre neueren VMs und Rolleninstanzen werden möglicherweise in einem in Resource Manager erstellten VNet ausgeführt. In diesem Artikel erfahren Sie, wie Sie eine VPN-Gatewayverbindung über einen IPsec/IKE-VPN-Tunnel zwischen den virtuellen Netzwerken erstellen.

Weitere Informationen zu VNet-zu-VNet-Verbindungen finden Sie am Ende dieses Artikels unter [Informationen zu VNet-zu-VNet-Verbindungen](#faq). Darüber hinaus sollten Sie auch den Artikel [VNET-Peering](../virtual-network/virtual-network-peering-overview.md) lesen. VNET-Peering stellt eine weitere Möglichkeit dar, um eine Verbindung zwischen Ihren VNETs zu erstellen, und bietet einige Vorteile in Bezug auf Verbindungen bei bestimmten Konfigurationen.

### <a name="prerequisites"></a>Voraussetzungen

* Bei den folgenden Schritten wird davon ausgegangen, dass beide VNETs bereits erstellt wurden. Wenn Sie diesen Artikel als Übung verwenden und Sie keine VNETs besitzen, finden Sie über die in den Schritten angegebenen Links Informationen zu ihrer Erstellung.
* Stellen Sie sicher, dass sich die Adressbereiche für die VNETs weder einander überlappen noch die Bereiche für andere Verbindungen, mit denen die Gateways verbunden werden können.
* Installieren Sie die neuesten PowerShell-Cmdlets für den Resource Manager und die Dienstverwaltung (klassisch). In diesem Artikel verwenden wir das Azure-Portal und PowerShell. Für die Verbindung zwischen dem klassischen VNET und dem Resource Manager-VNET ist PowerShell erforderlich. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs). 

### <a name="values"></a>Beispieleinstellungen
Sie können die Beispieleinstellungen als Referenz oder zum Erstellen einer Testkonfiguration verwenden.

**Klassisches VNET**

VNET-Name = ClassicVNet <br>
Adressraum = 10.0.0.0/24 <br>
Subnet-1 = 10.0.0.0/27 <br>
Ressourcengruppe = ClassicRG <br>
Standort = USA, Westen <br>
GatewaySubnet = 10.0.0.32/28 <br>
Lokaler Standort = RMVNetLocal <br>

**Resource Manager-VNET**

VNET-Name = RMVNet <br>
Adressraum = 192.168.0.0/16 <br>
Subnet-1 = 192.168.1.0/24 <br>
GatewaySubnet = 192.168.0.0/26 <br>
Ressourcengruppe = RG1 <br>
Standort = USA, Osten <br>
Name des Gateways für virtuelle Netzwerke = RMGateway <br>
Gatewaytyp = VPN <br>
VPN-Typ = Routenbasiert <br>
Öffentliche IP-Adresse des Gateways = rmgwpip <br>
Lokales Netzwerkgateway = ClassicVNetLocal <br>
Verbindungsname = RMtoClassic

### <a name="connection-overview"></a>Verbindung – Überblick

Für diese Konfiguration erstellen Sie eine VPN-Gatewayverbindung über einen IPsec/IKE-VPN-Tunnel zwischen den virtuellen Netzwerken. Stellen Sie sicher, dass sich die VNET-Bereiche weder untereinander überlappen noch mit lokalen Netzwerken, mit denen sie eine Verbindung herstellen.

Die folgende Tabelle enthält ein Beispiel dafür, wie die Beispiel-VNETs und lokalen Standorte definiert werden:

| Virtuelles Netzwerk | Adressraum | Region | Verbindung mit dem lokalen Netzwerkstandort |
|:--- |:--- |:--- |:--- |
| ClassicVNet |(10.0.0.0/24) |USA (Westen) | RMVNetLocal (192.168.0.0/16) |
| RMVNet | (192.168.0.0/16) |USA (Ost) |ClassicVNetLocal (10.0.0.0/24) |

## <a name="classicvnet"></a>1. Konfigurieren der Einstellungen für klassische VNETs
In diesem Abschnitt erstellen Sie das lokale Netzwerk (den lokalen Standort) und das Gateway des virtuellen Netzwerks für das klassische VNET. Wenn Sie nicht über ein klassisches VNET verfügen und diese Schritte als Übung ausführen, können Sie mithilfe der Informationen [dieses Artikels](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) und den oben genannten [Beispieleinstellungswerten](#values) ein VNET erstellen. Wenn Sie bereits ein VNET mit einem VPN-Gateway besitzen, stellen Sie sicher, dass das Gateway dynamisch ist. Falls es sich um ein statisches Gateway handelt, müssen Sie das VPN-Gateway zuerst löschen. Anschließend können Sie fortfahren.

Die Screenshots dienen lediglich zur Veranschaulichung. Achten Sie darauf, dass Sie die Werte durch Ihre eigenen Werte ersetzen oder die [Beispielwerte](#values) verwenden.

### <a name="part-1---configure-the-local-site"></a>Teil 1 – Konfigurieren des lokalen Standorts

Öffnen Sie das [Azure-Portal](https://ms.portal.azure.com), und melden Sie sich mit Ihrem Azure-Konto an.

1. Navigieren Sie zu **Alle Ressourcen** und suchen Sie in der Liste nach **ClassicVNet**.
2. Klicken Sie auf dem Blatt **Übersicht** im Abschnitt **VPN-Verbindungen** auf die Grafik **Gateway**, um ein Gateway zu erstellen.

    ![Konfigurieren von VPN-Gateways](./media/vpn-gateway-connect-different-deployment-models-portal/gatewaygraphic.png "Konfigurieren von VPN-Gateways")
3. Wählen Sie auf dem Blatt **Neue VPN-Verbindung** unter **Verbindungstyp** die Option **Standort-zu-Standort** aus.
4. Klicken Sie unter **Lokaler Standort** auf **Erforderliche Einstellungen konfigurieren**. Daraufhin wird das Blatt **Lokaler Standort** geöffnet.
5. Erstellen Sie auf dem Blatt **Lokaler Standort** einen Namen, um auf das Resource Manager-VNET zu verweisen. Beispiel: „RMVNetLocal“.
6. Wenn das VPN-Gateway für das Resource Manager-VNET bereits eine öffentliche IP-Adresse aufweist, verwenden Sie diesen Wert für das Feld **IP-Adresse des VPN-Gateways**. Wenn Sie diese Schritte als Übung ausführen oder Sie noch kein Gateway für virtuelle Netzwerke für Ihr Resource Manager-VNET besitzen, können Sie eine Platzhalter-IP-Adresse erstellen. Stellen Sie sicher, dass die Platzhalter-IP-Adresse ein gültiges Format aufweist. Später ersetzen Sie die Platzhalter-IP-Adresse durch die öffentliche IP-Adresse des Gateways für das virtuelle Resource Manager-Netzwerk.
7. Verwenden Sie für **Clientadressraum** die Werte für die IP-Adressräume des virtuellen Netzwerks für das Resource Manager-VNET. Diese Einstellung wird verwendet, um die Adressräume für die Weiterleitung an das virtuelle Resource Manager-Netzwerk anzugeben.
8. Klicken Sie auf **OK**, um die Werte zu speichern und zum Blatt **Neue VPN-Verbindung** zurückzukehren.

### <a name="part-2---create-the-virtual-network-gateway"></a>Teil 2 – Erstellen des Gateways für das virtuelle Netzwerk
1. Aktivieren Sie auf dem Blatt **Neue VPN-Verbindung** das Kontrollkästchen **Gateway sofort erstellen**, und klicken Sie auf **Optionale Gatewaykonfiguration**, um das Blatt **Gatewaykonfiguration** zu öffnen. 

    ![Öffnen des Blatts „Gatewaykonfiguration“](./media/vpn-gateway-connect-different-deployment-models-portal/optionalgatewayconfiguration.png "Öffnen des Blatts „Gatewaykonfiguration“")
2. Klicken Sie auf **Subnetz – Erforderliche Einstellungen konfigurieren**, um das Blatt **Subnetz hinzufügen** zu öffnen. Der **Name** ist bereits mit dem erforderlichen Wert **GatewaySubnet** konfiguriert.
3. **Adressbereich** bezieht sich auf den Bereich für das Gatewaysubnetz. Obwohl Sie ein Gatewaysubnetz mit einem /29-Adressbereich (3 Adressen) erstellen können, wird empfohlen, ein Gatewaysubnetz mit einer höheren Anzahl von IP-Adressen zu erstellen. Dadurch werden zukünftige Konfigurationen unterstützt, die möglicherweise eine höhere Anzahl von verfügbaren IP-Adressen erfordern. Verwenden Sie möglichst /27 oder /28. Sie können die [Beispielwerte](#values) nutzen, wenn Sie diese Schritte als Übung verwenden. Klicken Sie auf **OK**, um das Gatewaysubnetz zu erstellen.
4. Auf dem Blatt **Gatewaykonfiguration** bezieht sich **Größe** auf die Gateway-SKU. Wählen Sie die Gateway-SKU für das VPN-Gateway aus.
5. Vergewissern Sie sich, dass für **Routingtyp** der Wert **Dynamisch** festgelegt wurde, und klicken Sie auf **OK**, um zum Blatt **Neue VPN-Verbindung** zurückzukehren.
6. Klicken Sie auf dem Blatt **Neue VPN-Verbindung** auf **OK**, um mit dem Erstellen des VPN-Gateways zu beginnen. Die Erstellung eines VPN-Gateways kann bis zu 45 Minuten dauern.

### <a name="ip"></a>Teil 3 – Kopieren der öffentlichen IP-Adresse für das Gateway des virtuellen Netzwerks
Nachdem das Gateway für das virtuelle Netzwerk erstellt wurde, können Sie die Gateway-IP-Adresse anzeigen. 

1. Navigieren Sie zum klassischen VNET, und klicken Sie auf **Übersicht**.
2. Klicken Sie auf **VPN-Verbindungen**, um das Blatt „VPN-Verbindungen“ zu öffnen. Auf dem Blatt „VPN-Verbindungen“ können Sie die öffentliche IP-Adresse anzeigen. Dies ist die öffentliche IP-Adresse, die dem Gateway Ihres virtuellen Netzwerks zugewiesen ist. 
3. Notieren Sie die IP-Adresse, oder kopieren Sie sie. Sie verwenden sie in späteren Schritten bei der Arbeit mit den Konfigurationseinstellungen für das Gateway für das lokale Resource Manager-Netzwerk. Sie können auch den Status der Gatewayverbindungen anzeigen. Beachten Sie, dass der lokale Netzwerkstandort als „Verbindung wird hergestellt“ aufgeführt wird. Der Status ändert sich nach dem Erstellen der Verbindungen.
4. Schließen Sie das Blatt, nachdem Sie die IP-Adresse des Gateways kopiert haben.

## <a name="rmvnet"></a>2. Konfigurieren der Einstellungen für das Resource Manager-VNET

In diesem Abschnitt erstellen Sie das Gateway für das virtuelle Netzwerk und das lokale Netzwerkgateway für Ihr Resource Manager-VNET. Wenn Sie nicht über ein Resource Manager-VNET verfügen und diese Schritte als Übung ausführen, können Sie mithilfe der Informationen [dieses Artikels](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) und den oben genannten [Beispieleinstellungswerten](#values) ein VNET erstellen.

Die Screenshots dienen lediglich zur Veranschaulichung. Achten Sie darauf, dass Sie die Werte durch Ihre eigenen Werte ersetzen oder die [Beispielwerte](#values) verwenden.

### <a name="part-1---create-a-gateway-subnet"></a>Teil 1 – Erstellen eines Gatewaysubnetzes
Bevor Sie ein Gateway für ein virtuelles Netzwerk erstellen, müssen Sie das Gatewaysubnetz erstellen. Erstellen Sie ein Gatewaysubnetz mit einem CIDR-Wert von /28 oder einem größeren Wert (/27, /26 usw.).

[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]


[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

### <a name="part-2---create-a-virtual-network-gateway"></a>Teil 2 – Erstellen eines virtuellen Netzwerkgateways
[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

### <a name="createlng"></a>Teil 3 – Erstellen eines lokalen Netzwerkgateways

Das lokale Netzwerkgateway legt den Adressbereich und die öffentliche IP-Adresse fest, die Ihrem klassischen VNET und dem Gateway des zugehörigen virtuellen Netzwerks zugeordnet sind.

Wenn Sie diese Schritte als Übung durchführen, verwenden Sie folgende Einstellungen:

| Virtuelles Netzwerk | Adressraum | Region | Verbindung mit dem lokalen Netzwerkstandort |Öffentliche IP-Adresse des Gateways|
|:--- |:--- |:--- |:--- |:--- |
| ClassicVNet |(10.0.0.0/24) |USA (Westen) | RMVNetLocal (192.168.0.0/16) |Die öffentliche IP-Adresse, die dem Gateway ClassicVNet zugewiesen ist|
| RMVNet | (192.168.0.0/16) |USA (Ost) |ClassicVNetLocal (10.0.0.0/24) |Die öffentliche IP-Adresse, die dem Gateway RMVNet zugewiesen ist|

[!INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]

## <a name="modifylng"></a>3. Ändern der Einstellungen für den lokalen Standort des klassischen VNET

In diesem Abschnitt ersetzen Sie die Platzhalter-IP-Adresse, die Sie beim Festlegen der Einstellungen für den lokalen Standort verwendet haben, durch die IP-Adresse des Resource Manager-VPN-Gateways. In diesem Abschnitt werden die klassischen PowerShell-Cmdlets (SM) verwendet.

1. Navigieren Sie im Azure-Portal zum klassischen virtuellen Netzwerk.
2. Klicken Sie auf dem Blatt für Ihr virtuelles Netzwerk auf **Übersicht**.
3. Klicken Sie unter **VPN-Verbindungen** auf den Namen des lokalen Standorts in der Abbildung.

    ![VPN-Verbindungen](./media/vpn-gateway-connect-different-deployment-models-portal/vpnconnections.png "VPN-Verbindungen")
4. Klicken Sie auf dem Blatt **Standort-zu-Standort-VPN-Verbindungen** auf den Namen des Standorts.

    ![Standortname](./media/vpn-gateway-connect-different-deployment-models-portal/sitetosite3.png "Lokaler Standortname")
5. Klicken Sie auf dem Blatt „Verbindung“ für den lokalen Standort auf den Namen des lokalen Standorts, um das Blatt **Lokaler Standort** zu öffnen.

    ![Öffnen von „Lokaler Standort“](./media/vpn-gateway-connect-different-deployment-models-portal/openlocal.png "Öffnen von „Lokaler Standort“")
6. Ersetzen Sie auf dem Blatt **Lokaler Standort** die **IP-Adresse des VPN-Gateways** durch die IP-Adresse des Resource Manager-Gateways.

    ![Gateway-IP-Adresse](./media/vpn-gateway-connect-different-deployment-models-portal/gwipaddress.png "Gateway-IP-Adresse")
7. Klicken Sie auf **OK**, um die IP-Adresse zu aktualisieren.

## <a name="RMtoclassic"></a>4. Erstellen der Verbindung zwischen dem Resource Manager-VNET und dem klassischen VNET

In den folgenden Schritten konfigurieren Sie mithilfe des Azure-Portals die Verbindung zwischen dem Resource Manager-VNET und dem klassischen VNET.

1. Suchen Sie unter **Alle Ressourcen** nach dem lokalen Netzwerkgateway. In unserem Beispiel handelt es sich beim lokalen Netzwerkgateway um **ClassicVNetLocal**.
2. Klicken Sie auf **Konfiguration** und stellen Sie sicher, dass es sich beim IP-Adresswert um das VPN-Gateway für das klassische VNET handelt. Aktualisieren Sie diesen bei Bedarf und klicken Sie dann auf **Speichern**. Schließen Sie das Blatt.
3. Klicken Sie unter **Alle Ressourcen** auf das lokale Netzwerkgateway.
4. Klicken Sie auf **Verbindungen**, um das Blatt „Verbindungen“ zu öffnen.
5. Klicken Sie auf dem Blatt **Verbindungen** auf **+**, um eine Verbindung hinzuzufügen.
6. Geben Sie auf dem Blatt **Verbindung hinzufügen** einen Namen für die Verbindung ein. Beispiel: „RMtoClassic“.
7. **Standort-zu-Standort-** ist bereits auf diesem Blatt ausgewählt.
8. Wählen Sie das Gateway des virtuellen Netzwerks aus, das Sie diesem Standort zuweisen möchten.
9. Erstellen Sie einen **gemeinsam verwendeten Schlüssel**. Dieser Schlüssel wird auch bei der Verbindung verwendet, die Sie zwischen dem klassischen VNET und dem Resource Manager-VNET erstellen. Sie können den Schlüssel generieren oder einen solchen erstellen. In unserem Beispiel wird „abc123“ verwendet, aber Sie können (und sollten) einen komplexeren Wert verwenden.
10. Klicken Sie auf **OK**, um die Verbindung zu erstellen.

##<a name="classictoRM"></a>5. Erstellen der Verbindung zwischen dem klassischen VNET und dem Resource Manager-VNET

In den folgenden Schritten konfigurieren Sie die Verbindung zwischen dem klassischen VNET und dem Resource Manager-VNET. Für diese Schritte ist PowerShell erforderlich. Diese Verbindung kann nicht im Portal erstellt werden. Stellen Sie sicher, dass Sie sowohl die SM-PowerShell-Cmdlets (klassisch) als auch die RM-PowerShell-Cmdlets (Resource Manager) heruntergeladen und installiert haben.

### <a name="1-connect-to-your-azure-account"></a>1. Herstellen einer Verbindung mit Ihrem Azure-Konto

Öffnen Sie die PowerShell-Konsole mit erhöhten Rechten, und melden Sie sich bei Ihrem Azure-Konto an. Das folgende Cmdlet fordert Sie zur Eingabe der Anmeldeinformationen für Ihr Azure-Konto auf. Nachdem Sie sich angemeldet haben, werden Ihre Kontoeinstellungen heruntergeladen, sodass sie in Azure PowerShell verfügbar sind.
   
    Login-AzureRmAccount 
   
Falls Sie mehrere Abonnements haben, benötigen Sie eine Liste Ihrer Azure-Abonnements.
   
    Get-AzureRmSubscription
   
Geben Sie das Abonnement an, das Sie verwenden möchten. 
   
    Select-AzureRmSubscription -SubscriptionName "Name of subscription"

Fügen Sie Ihr Azure-Konto hinzu, um die klassischen PowerShell-Cmdlets zu verwenden. Dazu können Sie den folgenden Befehl verwenden:
   
    Add-AzureAccount

### <a name="2-view-the-network-configuration-file-values"></a>2. Anzeigen der Werte der Netzwerkkonfigurationsdatei

Wenn Sie im Azure-Portal ein VNet erstellen, ist der vollständige von Azure verwendet Name im Azure-Portal nicht zu sehen. Beispielsweise kann ein VNET, das im Azure-Portal als „ClassicVNet“ angezeigt wird, in der Netzwerkkonfigurationsdatei einen viel längeren Namen haben. Der Name kann etwa wie folgt aussehen: „Group ClassicRG ClassicVNet“. In den folgenden Schritten laden Sie die Netzwerkkonfigurationsdatei herunter und zeigen die Werte an.

Erstellen Sie auf Ihrem Computer ein Verzeichnis, und exportieren Sie die Netzwerkkonfigurationsdatei in das Verzeichnis. In diesem Beispiel wird die Netzwerkkonfigurationsdatei in das Verzeichnis „C:\AzureNet“ exportiert.

    Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml

Öffnen Sie die Datei in einem Text-Editor, und zeigen Sie den Namen für Ihr klassisches VNet an. Verwenden Sie zum Ausführen von PowerShell-Cmdlets die Namen in der Netzwerkkonfigurationsdatei.

- VNET-Namen werden unter **VirtualNetworkSite name =** aufgelistet.
- Standortnamen werden unter **LocalNetworkSite name=** aufgelistet.

### <a name="3-create-the-connection"></a>3. Erstellen der Verbindung

Legen Sie den gemeinsam verwendeten Schlüssel fest und erstellen Sie die Verbindung zwischen dem klassischen VNET und dem Resource Manager-VNET.

- In diesem Beispiel ist **-VNetName** der Name des klassischen VNET, der in Ihrer Netzwerkkonfigurationsdatei enthalten ist. 
- **-LocalNetworkSiteName** ist der Name, den Sie für den lokalen Standort festgelegt haben und der in Ihrer Netzwerkkonfigurationsdatei enthalten ist.
- **-SharedKey** ist ein Wert, den Sie generieren und festlegen. In diesem Beispiel wurde *abc123* verwendet, aber Sie können etwas Komplexeres generieren. Entscheidend ist Folgendes: Der Wert, den Sie hier angeben, muss demselben Wert entsprechen, den Sie beim Erstellen der Verbindung zwischen Ihrem Resource Manager-VNET und dem klassischen VNET angegeben haben.

        Set-AzureVNetGatewayKey -VNetName "Group ClassicRG ClassicVNet" `
        -LocalNetworkSiteName "172B9E16_RMVNetLocal" -SharedKey abc123

##<a name="verify"></a>6. Überprüfen der Verbindungen
Sie können die Verbindung im Azure-Portal oder mit PowerShell überprüfen. Beim Überprüfen müssen Sie möglicherweise eine Minute warten, während die Verbindung erstellt wird. Wenn eine Verbindung erfolgreich erstellt wurde, wechselt der Verbindungsstatus von „Verbindung wird hergestellt“ zu „Verbunden“.

### <a name="to-verify-the-connection-from-your-classic-vnet-to-your-resource-manager-vnet"></a>So überprüfen Sie die Verbindung des klassischen VNet mit dem Resource Manager-VNet

[!INCLUDE [vpn-gateway-verify-connection-azureportal-classic](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

###<a name="to-verify-the-connection-from-your-resource-manager-vnet-to-your-classic-vnet"></a>So überprüfen Sie die Verbindung des Resource Manager-VNet mit dem klassischen VNet

[!INCLUDE [vpn-gateway-verify-connection-portal-rm](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="faq"></a>Informationen zu VNet-zu-VNet-Verbindungen

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]
