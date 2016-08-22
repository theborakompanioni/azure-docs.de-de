<properties 
   pageTitle="Konfigurieren der Tunnelerzwingung für Standort-zu-Standort-Verbindungen mit dem klassischen Bereitstellungsmodell | Microsoft Azure"
   description="Gewusst wie: „Erzwingen“ der Umleitung des gesamten Internetdatenverkehrs an Ihren lokalen Standort."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/10/2016"
   ms.author="cherylmc" />

# Konfigurieren der Tunnelerzwingung mit dem klassischen Bereitstellungsmodell

> [AZURE.SELECTOR]
- [PowerShell – klassisch](vpn-gateway-about-forced-tunneling.md)
- [PowerShell – Resource Manager](vpn-gateway-forced-tunneling-rm.md)

Über die Tunnelerzwingung können Sie die Umleitung des gesamten Internetdatenverkehrs an Ihren lokalen Standort „erzwingen“. Sie verwenden dazu einen Standort-zu-Standort-VPN-Tunnel für die Kontrolle und Überwachung. Dies ist eine wichtige Sicherheitsvoraussetzung der IT-Richtlinien für die meisten Unternehmen.

Ohne die Tunnelerzwingung wird der Internetdatenverkehr Ihrer virtuellen Computer in Azure immer direkt von der Azure-Netzwerkinfrastruktur an das Internet geleitet, ohne dass Sie die Möglichkeit haben, diesen zu überprüfen oder zu überwachen. Nicht autorisierter Zugriff auf das Internet kann potenziell zur Offenlegung von Informationen oder anderen Arten von Sicherheitsverletzungen führen.

Dieser Artikel beschreibt die Konfiguration der Tunnelerzwingung für virtuelle Netzwerke, die mit dem klassischen Bereitstellungsmodell erstellt wurden.

**Informationen zu Azure-Bereitstellungsmodellen**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

**Bereitstellungsmodelle und Tools für die Tunnelerzwingung**

Eine Verbindung mit erzwungenem Tunneln kann sowohl für das klassische Bereitstellungsmodell als auch für das Resource Manager-Bereitstellungsmodell konfiguriert werden. Ausführlichere Informationen finden Sie in der Tabelle weiter unten. Wir aktualisieren diese Tabelle, wenn neue Artikel, neue Bereitstellungsmodelle und weitere Tools für diese Konfiguration verfügbar werden. Wenn ein Artikel verfügbar ist, fügen wir in der Tabelle einen direkten Link dazu ein.

[AZURE.INCLUDE [vpn-gateway-forcedtunnel](../../includes/vpn-gateway-table-forcedtunnel-include.md)]


## Anforderungen und Überlegungen

Die Tunnelerzwingung in Azure wird über benutzerdefinierte Routen im virtuellen Netzwerk konfiguriert. Das Umleiten von Datenverkehr an einen lokalen Standort wird als eine Standardroute zum Azure-VPN-Gateway umgesetzt. Im folgende Abschnitt werden die aktuellen Einschränkung für die Routingtabelle und die Routen in Azure Virtual Network aufgeführt:


-  Jedes Subnetz des virtuellen Netzwerks verfügt über eine integrierte Systemroutingtabelle. Die Systemroutingtabelle verfügt über die folgenden drei Gruppen von Routen:

	- **Lokale VNET-Routen:** direkt zu den virtuelle Zielcomputern im gleichen virtuellen Netzwerk
	
	- **Lokale Routen:** zum Azure-VPN-Gateway
	
	- **Standardroute:** direkt zum Internet. Pakete an private IP-Adressen, die nicht durch die vorherigen beiden Routen abgedeckt sind, werden verworfen.


-  Sie können mit der Veröffentlichung von benutzerdefinierten Routen eine Routingtabelle erstellen, um eine Standardroute hinzuzufügen. Anschließend verknüpfen Sie dann die Routingtabelle mit den VNET-Subnetzen, um die Tunnelerzwingung in diesen Subnetzen zu aktivieren.

- Sie müssen einen "Standardstandort" unter den standortübergreifenden lokalen Standorten auswählen, der mit dem virtuellen Netzwerk verbunden ist.

- Die Tunnelerzwingung muss einem VNet zugeordnet werden, das über ein VPN-Gateway für dynamisches Routing (kein statisches Gateway) verfügt.
 
- Die ExpressRoute-Tunnelerzwingung kann über diesen Mechanismus nicht konfiguriert werden. Sie wird stattdessen durch Anfordern einer Standardroute über die ExpressRoute-BGP-Peeringsitzungen aktiviert. Weitere Informationen finden Sie unter der [ExpressRoute-Dokumentation](https://azure.microsoft.com/documentation/services/expressroute/).



## Konfigurationsübersicht

Im folgenden Beispiel wird für das Front-End-Subnetz kein Tunneln erzwungen. Die Workloads im Frontend-Subnetz können weiterhin Kundenanfragen direkt aus dem Internet akzeptieren und darauf reagieren. Für die Subnetze "Midtier" und "Backend" wird das Tunneln erzwungen. Bei allen ausgehenden Verbindungen mit dem Internet aus diesen zwei Subnetzen wird eine Umleitung an einen lokalen Standort über einen der S2S-VPN-Tunnel erzwungen.

Dadurch können Sie den Internetzugriff über die virtuellen Computer oder Clouddienste in Azure einschränken und überprüfen, während die erforderliche mehrschichtige Dienstarchitektur weiterhin in Betrieb bleibt. Sie können das erzwungene Tunneln auch auf alle virtuellen Netzwerke anwenden, wenn in diesen keine Workloads mit Internetverbindung erforderlich sind.


![Tunnelerzwingung](./media/vpn-gateway-about-forced-tunneling/forced-tunnel.png)



## Voraussetzungen

Vergewissern Sie sich vor Beginn der Konfiguration, dass Sie über Folgendes verfügen:

- Ein Azure-Abonnement. Wenn Sie noch kein Abonnement haben, können Sie Ihre [MSDN-Abonnentenvorteile](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) aktivieren oder sich für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) registrieren.

- Ein konfiguriertes virtuelles Netzwerk.

- Die neueste Version der Azure PowerShell-Cmdlets. Weitere Informationen zur Installation der PowerShell-Cmdlets finden Sie unter [Gewusst wie: Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).


## Konfigurieren der Tunnelerzwingung

Das folgende Verfahren hilft Ihnen bei der Angabe der Tunnelerzwingung für ein virtuelles Netzwerk. Die Konfigurationsschritte entsprechen der VNet-Netzwerkkonfigurationsdatei.



	<VirtualNetworkSite name="MultiTier-VNet" Location="North Europe">
     <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="DefaultSiteHQ">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch1">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch2">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch3">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
        </Gateway>
      </VirtualNetworkSite>
	</VirtualNetworkSite>

In diesem Beispiel verfügt das virtuelle Netzwerk „MultiTier-VNet“ über drei Subnetze: *Frontend*, *Midtier* und *Backend* mit vier standortübergreifenden Verbindungen: *DefaultSiteHQ* und drei *Verzweigungen*.

Mithilfe der Schritte wird festgelegt, dass *DefaultSiteHQ* die Standard-Standortverbindung für die Tunnelerzwingung ist und die Subnetze „Midtier“ und „Backend“ die Tunnelerzwingung verwenden müssen.


1. Erstellen Sie eine Routingtabelle. Verwenden Sie das folgende Cmdlet zum Erstellen der Routingtabelle.

		New-AzureRouteTable –Name "MyRouteTable" –Label "Routing Table for Forced Tunneling" –Location "North Europe"

2. Fügen Sie der Routingtabelle eine Standardroute hinzu.

	Das folgende Beispiel fügt der in Schritt 1 erstellten Routingtabelle eine Standardroute hinzu. Beachten Sie, dass die einzige unterstützte Route das Zielpräfix von 0.0.0.0/0 zum nächsten Hop „VPNGateway“ ist.
 
		Set-AzureRoute –RouteTable "MyRouteTable" –RouteName "DefaultRoute" –AddressPrefix "0.0.0.0/0" –NextHopType VPNGateway

3. Ordnen Sie die Routingtabelle den Subnetzen zu.

	Nachdem eine Routingtabelle erstellt und eine Route hinzugefügt wurde, verwenden Sie das folgende Beispiel, um die Routingtabelle einem VNet-Subnetz hinzuzufügen oder zuzuordnen. Das Beispiel fügt die Routingtabelle „MyRouteTable“ den Subnetzen „Midtier“ und „Backend“ des virtuellen Netzwerks „MultiTier-VNet“ hinzu.

		Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Midtier" -RouteTableName "MyRouteTable"

		Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Backend" -RouteTableName "MyRouteTable"

4. Weisen Sie einen Standardstandort für die Tunnelerzwingung zu.

	Im vorherigen Schritt wurde mit dem Cmdlet-Beispielskript die Routingtabelle erstellt und zwei VNet-Subnetzen zugeordnet. Im letzten Schritt wird ein lokaler Standort aus den Verbindungen mit mehreren Standorten des virtuellen Netzwerks als Standardstandort oder -tunnel ausgewählt.

		$DefaultSite = @("DefaultSiteHQ")
		Set-AzureVNetGatewayDefaultSite –VNetName "MultiTier-VNet" –DefaultSite "DefaultSiteHQ"

## Weitere PowerShell-Cmdlets

### So löschen Sie eine Routingtabelle

	Remove-AzureRouteTable -Name <routeTableName>

### So listen Sie eine Routingtabelle auf

	Get-AzureRouteTable [-Name <routeTableName> [-DetailLevel <detailLevel>]]

### So löschen Sie eine Route aus einer Routingtabelle

	Remove-AzureRouteTable –Name <routeTableName>

### So entfernen Sie eine Route aus einem Subnetz

	Remove-AzureSubnetRouteTable –VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>

### So listen Sie die einem Subnetz zugeordnete Routingtabelle auf
	
	Get-AzureSubnetRouteTable -VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>

### So entfernen Sie einen Standardstandort aus einem VNET-VPN-Gateway

	Remove-AzureVnetGatewayDefaultSite -VNetName <virtualNetworkName>

<!---HONumber=AcomDC_0810_2016-->