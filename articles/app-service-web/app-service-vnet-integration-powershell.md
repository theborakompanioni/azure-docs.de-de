<properties 
	pageTitle="Verbinden einer App mit einem VNET mithilfe von PowerShell" 
	description="Informationen zum Verbinden von und Arbeiten mit V1- oder V2-VNETs mithilfe von PowerShell" 
	services="app-service" 
	documentationCenter="" 
	authors="ccompy" 
	manager="wpickett" 
	editor="cephalin"/>

<tags 
	ms.service="app-service" 
	ms.workload="na" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="04/07/2016" 
	ms.author="ccompy"/>

# Verbinden Ihrer App mit Ihrem VNET über PowerShell #

## Übersicht ##

In Azure App Service können Sie Ihre App (Web, mobil oder API) mit einem VNET in Ihrem Abonnement verbinden. Dieses Feature heißt „VNET-Integration“. Das Feature „VNET-Integration“ sollte nicht mit dem Feature „App Service-Umgebung“ verwechselt werden, mit dem Sie eine Instanz von Azure App Service in Ihrem VNET ausführen können.

„VNET-Integration“ verfügt über eine Benutzeroberfläche im neuen Portal und ermöglicht Ihnen die Integration in V1- oder V2-VNETs. Weitere Informationen über das Feature finden Sie hier: „Integrieren Ihrer App in ein Azure Virtual Network“.

In diesem Dokument geht es nicht um die Verwendung der Benutzeroberfläche, sondern um das Aktivieren der Integration mithilfe von PowerShell. Die Befehle für V1-VNETs unterscheiden sich von denen für V2-VNETs, deshalb gibt es zwei Abschnitte.

Bevor Sie mit diesem Dokument fortfahren, stellen Sie sicher, dass Sie über Folgendes verfügen:

1. Eine Installation des neuesten Azure PowerShell SDK. Sie können es mit dem Webplattform-Installer installieren.
1. Eine App in Azure App Service, die in einer Standard- oder Premium-SKU ausgeführt wird.

## V1-VNETs (klassisch) ##

Dieses Dokument beschreibt drei Elemente für V1-VNETs:

- Verbinden Ihrer App mit einem bereits vorhandenen V1-VNET, das über ein Gateway verfügt und eine Punkt-zu-Standort-Konfiguration aufweist
- Aktualisieren Ihrer VNET-Integrationsinformationen für Ihre App
- Trennen Ihrer App vom V1-VNET

### Verbinden einer App mit einem V1-VNET (klassisch) ###

Das Verbinden einer App mit einem virtuellen Netzwerk (VNET) erfolgt in drei Schritten:

1. Deklarieren Sie in der Web-App, dass sie mit einem bestimmten VNET verknüpft wird. Die App generiert ein Zertifikat für die Punkt-zu-Standort-Verbindung für das VNET.
1. Laden Sie das Web-App-Zertifikat in das VNET hoch, und rufen Sie dann den URI für das Punkt-zu-Standort-VPN-Paket ab.
1. Aktualisieren Sie die Web-App-VNET-Verbindung mit dem Punkt-zu-Standort-Paket-URI.

Die Schritte 1) und 3) können vollständig mit Skripts ausgeführt werden. Aber Schritt 2) erfordert eine einmalige manuelle Aktion über das Portal oder Zugriff zum Ausführen von PUT- oder PATCH-Aktionen auf dem ARM-Endpunkt des virtuellen Netzwerks (wenden Sie sich an den Azure-Support, um diese Option aktivieren zu lassen). Bevor Sie beginnen, stellen Sie sicher, dass ein klassische virtuelles Netzwerk mit Punkt-zu-Standort-Konfiguration bereits aktiviert ist und ein Gateway erstellt/bereitgestellt wurde. Zum Erstellen des Gateways und Aktivieren der Punkt-zu-Standort-Konfiguration müssen Sie das Portal verwenden, wie unter [Erstellen eines VPN-Gateways][createvpngateway] beschrieben.

Das V1-VNET muss zum gleichen Abonnement wie der App Service-Plan gehören, der die App enthält, für die Sie die Integration einrichten.

#####Einrichten des Azure PowerShell SDK#####

Öffnen Sie ein PowerShell-Fenster, und richten Sie Ihr Azure-Konto und das Abonnement mit folgendem Befehl ein:

	Login-AzureRmAccount

Dieser Befehl öffnet eine Eingabeaufforderung zur Eingabe Ihrer Azure-Anmeldeinformationen. Wählen Sie nach der Anmeldung mit

	Select-AzureRmSubscription –SubscriptionName [WebAppSubscriptionName]

oder

	Select-AzureRmSubscription –SubscriptionId [WebAppSubscriptionId]

das Abonnement aus, das Sie verwenden möchten. Sie müssen das Abonnement verwenden, zum dem Ihr VNET und der App Service-Plan gehören.

#####In diesem Dokument verwendete Variablen#####

Zur Vereinfachung der weiter unten verwendeten Befehle legen wir eine $Configuration-PowerShell-Variable mit der spezifischen Konfiguration fest.

Legen Sie eine Variable wie folgt in PowerShell mit den folgenden Parametern fest:

	$Configuration = @{} 
	$Configuration.WebAppResourceGroup = "[Your web app resource group]"
	$Configuration.WebAppName = "[Your web app name]"
	$Configuration.VnetSubscriptionId = "[Your vnet subscription id]" 
	$Configuration.VnetResourceGroup = "[Your vnet resource group]" 
	$Configuration.VnetName = "[Your vnet name]"
 
Der App-Ort sollte der Ort ohne Leerzeichen sein, z. B. „USA, Westen“ ist „westus“.

	$Configuration.WebAppLocation = "[Your web app Location]" 

Das nächste Element ist die Angabe, wohin das Zertifikat geschrieben werden soll. Es sollte ein beschreibbarer Pfad im lokalen System sein. Vergessen Sie nicht, am Ende „.cer“ anzufügen.

	$Configuration.GeneratedCertificatePath = "[C:\Path\To\Certificate.cer]" 

Um zu sehen, was Sie festgelegt haben, geben Sie „$Configuration“ ein.

	> $Configuration
	
	Name                           Value
	----                           -----
	GeneratedCertificatePath       C:\vnetcert.cer
	VnetSubscriptionId             efc239a4-88f9-2c5e-a9a1-3034c21ad496
	WebAppResourceGroup            vnetdemo-rg
	VnetResourceGroup              testase1-rg
	VnetName                       TestNetwork
	WebAppName                     vnetintdemoapp
	WebAppLocation                 centralus

Im Rest des Dokuments wird davon ausgegangen, dass Sie eine Variable wie gerade beschrieben erstellt haben.

#####Deklarieren des VNET für die App#####

Verwenden Sie den folgenden Befehl aus, um für die App anzugeben, dass dieses bestimmte VNET verwendet wird. Dadurch generiert die App die erforderlichen Zertifikate:

	$vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -PropertyObject @{"VnetResourceId" = "/subscriptions/$($Configuration.VnetSubscriptionId)/resourceGroups/$($Configuration.VnetResourceGroup)/providers/Microsoft.ClassicNetwork/virtualNetworks/$($Configuration.VnetName)"} -Location $Configuration.WebAppLocation -ApiVersion 2015-07-01

Wenn dieser Befehl erfolgreich ausgeführt wird, sollte $vnet eine Properties-Variable enthalten. Die Properties-Variable sollte sowohl einen Zertifikatfingerabdruck als auch Zertifikatdaten enthalten.

#####Hochladen des Web-App-Zertifikats in das virtuelle Netzwerk#####

Ein einmaliger manueller Schritt ist für jede Kombination aus Abonnement und VNET erforderlich. Wenn Sie also Apps im Abonnement A mit VNET A verbinden, müssen Sie diesen Schritt nur einmal ausführen, unabhängig davon, wie viele Apps Sie konfigurieren. Wenn Sie eine neue App zu einem anderen VNET hinzufügen, muss dieser Schritt erneut ausgeführt werden. Der Grund dafür ist, dass eine Gruppe von Zertifikaten auf Abonnementebene in Azure App Service erstellt wird, und sie wird einmal für jedes VNET generiert, mit dem die Apps eine Verbindung herstellen.

Die Zertifikate wurden bereits festgelegt, wenn Sie diese Schritte durchgeführt haben oder wenn Sie die Integration mit dem gleichen VNET mithilfe des Portals durchgeführt haben.

Der erste Schritt ist das Generieren der CER-Datei. Der zweite Schritt ist das Hochladen der CER-Datei in Ihr VNET. Um die CER-Datei aus dem API-Aufruf im vorherigen Schritt zu generieren, führen Sie die folgenden Befehle aus.

	$certBytes = [System.Convert]::FromBase64String($vnet.Properties.certBlob)
	[System.IO.File]::WriteAllBytes("$($Configuration.GeneratedCertificatePath)", $certBytes)

Das Zertifikat befindet sich am mit „$Configuration.GeneratedCertificatePath“ angegebenen Speicherort.

Verwenden Sie zum manuellen Hochladen des Zertifikats das neue Portal über das [Azure-Portal][azureportal], und wechseln Sie zu „Virtuelles Netzwerk (klassisch) > VPN Verbindungen > Punkt-zu-Standort > Zertifikate verwalten“. Hier können Sie Ihr Zertifikat hochladen.

#####Abrufen des Punkt-zu-Standort-Pakets#####

Der nächste Schritt beim Einrichten einer VNET-Verbindung in einer Web-App ist, das Punkt-zu-Standort-Paket abzurufen und Ihrer Web-App bereitzustellen.

Speichern Sie die folgende Vorlage in einer Datei namens „GetNetworkPackageUri.json“ an einer beliebigen Stelle auf dem Computer, z. B. „C:\\Azure\\Templates\\GetNetworkPackageUri.json“.

	{ 
		"$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#", 
		"contentVersion": "1.0.0.0", 
		"parameters": { 
			"certData": { 
				"type": "string" 
			}, 
			"certThumbprint": { 
				"type": "string" 
			}, 
			"networkName": { 
				"type": "string" 
			} 
		}, 
		"variables": { 
			"legacyVnetName": "[concat('Group ', resourceGroup().name, ' ', parameters('networkName'))]" 
			}, 
			"resources": [ 
			], 
		"outputs" : { 
			"PackageUri" : 
			{ 
			"value" : "[listPackage(resourceId('Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates', parameters('networkName'), 'primary', parameters('certThumbprint')), '2014-06-01').packageUri]", "type" : "string" 
			} 
		} 
	} 


Legen Sie Eingabeparameter fest:

	$parameters = @{"certData" = $vnet.Properties.certBlob ; 
	certThumbprint = $vnet.Properties.certThumbprint ; 
	"networkName" = $Configuration.VnetName } 

Rufen Sie das Skript auf:

	$output = New-AzureRmResourceGroupDeployment -Name unused -ResourceGroupName $Configuration.VnetResourceGroup -TemplateParameterObject $parameters -TemplateFile C:\PATH\TO\GetNetworkPackageUri.json 


Die Variable „$output.Outputs.packageUri“ enthält jetzt den Paket-URI, den Ihre Web-App benötigt.

#####Hochladen des Punkt-zu-Standort-Pakets in Ihre App#####

Mit dem letzten Schritt wird der App dieses Paket bereitgestellt. Führen Sie einfach den nächsten Befehl aus:

	$vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)/primary" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-07-01 -PropertyObject @{"VnetName" = $Configuration.VnetName ; "VpnPackageUri" = $($output.Outputs.packageUri).Value } -Location $Configuration.WebAppLocation 

Sie könnten mit einer Meldung aufgefordert werden zu bestätigen, dass Sie eine vorhandene Ressource überschreiben. Sie müssen dies zulassen.

Wenn dieser Befehl erfolgreich ausgeführt wird, sollte Ihre App mit dem VNET verbunden sein. Sie können dies überprüfen, indem Sie zur App-Konsole wechseln und Folgendes eingeben:

	SET WEBSITE_

Wenn eine Umgebungsvariable namens WEBSITE\_VNETNAME mit einem Wert, der mit dem Namen des Ziel-VNET übereinstimmt, vorhanden ist, wurden alle Konfigurationen erfolgreich abgeschlossen.

###Aktualisieren der V1-VNET-Integrationsinformationen (klassisch)###

Zum Aktualisieren oder Neusynchronisieren Ihrer Daten müssen Sie einfach die Schritte wiederholen, die Sie beim Einrichten der Integration ausgeführt haben. Diese Schritte sind:

1. Definieren von Konfigurationsinformationen
1. Deklarieren des VNET für die App
1. Abrufen des Punkt-zu-Standort-Pakets
1. Hochladen des Punkt-zu-Standort-Pakets in Ihre App

###Trennen Ihrer App von einem V1-VNET (klassisch)###

Zum Trennen benötigen Sie die Konfigurationsinformationen, die während der VNET-Integration festgelegt wurden. Wenn Sie diese Informationen verwenden, ist ein Befehl erforderlich, um Ihre App vom VNET zu trennen.

	$vnet = Remove-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-07-01

## V2-VNETS (Ressourcen-Manager) ##

Die V2- oder Ressourcen-Manager-VNETs verfügen über ARM-APIs, und es gibt einige Dinge, die wir tun können, um die Verwendung im Vergleich mit V1-VNETs zu erleichtern. Wir haben ein Skript bereitgestellt, das Folgendes ermöglicht:

- Erstellen eines V2-VNET und Integrieren Ihrer App
- Erstellen eines Gateways, Konfigurieren einer Punkt-zu-Standort-Verbindung in einem bereits vorhandenen V2-VNET und anschließendes Integrieren Ihrer App in das VNET
- Integrieren Ihrer App in ein bereits vorhandenes V2-VNET, in dem ein Gateway und eine Punkt-zu-Standort-Konfiguration aktiviert wurden 
- Trennen Ihrer App vom V2-VNET

###Skript für die Integration eines V2-VNET und App Service###

Kopieren Sie das folgende Skript, und speichern Sie es in einer Datei. Sie müssen das Skript nicht in der vorliegenden Form verwenden. Sie können es aber nutzen, um zu erfahren, wie Sie alles mit einem V2-VNET einrichten können.


    function ReadHostWithDefault($message, $default)
    {
    	$result = Read-Host "$message [$default]"
    	if($result -eq "")
	    {
			$result = $default
	    }
		    return $result
    	}
    
	function PromptCustom($title, $optionValues, $optionDescriptions)
	{
	    Write-Host $title
	    Write-Host
	    $a = @()
	    for($i = 0; $i -lt $optionValues.Length; $i++){
		    Write-Host "$($i+1))" $optionDescriptions[$i]
	    }
	    Write-Host
    
	    while($true)
	    {
		    Write-Host "Choose an option: "
		    $option = Read-Host
		    $option = $option -as [int]
		    
		    if($option -ge 1 -and $option -le $optionValues.Length)
		    {
			    return $optionValues[$option-1]
		    }
	    }
    }
    
    function PromptYesNo($title, $message, $default = 0)
    {
	    $yes = New-Object System.Management.Automation.Host.ChoiceDescription "&Yes", ""
	    $no = New-Object System.Management.Automation.Host.ChoiceDescription "&No", ""
	    $options = [System.Management.Automation.Host.ChoiceDescription[]]($yes, $no)
	    $result = $host.ui.PromptForChoice($title, $message, $options, $default)
	    return $result
    }
    
    function CreateVnet($resourceGroupName, $vnetName, $vnetAddressSpace, $vnetGatewayAddressSpace, $location)
    {
	    Write-Host "Creating a new VNET"
	    $gatewaySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix $vnetGatewayAddressSpace
	    New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName -Location $location -AddressPrefix $vnetAddressSpace -Subnet $gatewaySubnet
    }
    
    function CreateVnetGateway($resourceGroupName, $vnetName, $vnetIpName, $location, $vnetIpConfigName, $vnetGatewayName, $certificateData, $vnetPointToSiteAddressSpace)
    {
	    $vnet = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName
	    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
	    
	    Write-Host "Creating a public IP address for this VNET"
	    $pip = New-AzureRmPublicIpAddress -Name $vnetIpName -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
	    $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $vnetIpConfigName -Subnet $subnet -PublicIpAddress $pip
	    
	    Write-Host "Adding a root certificate to this VNET"
	    $root = New-AzureRmVpnClientRootCertificate -Name "AppServiceCertificate.cer" -PublicCertData $certificateData
	    
	    Write-Host "Creating Azure VNET Gateway. This may take up to an hour."
	    New-AzureRmVirtualNetworkGateway -Name $vnetGatewayName -ResourceGroupName $resourceGroupName -Location $location -IpConfigurations $ipconf -GatewayType Vpn -VpnType RouteBased -EnableBgp $false -GatewaySku Basic -VpnClientAddressPool $vnetPointToSiteAddressSpace -VpnClientRootCertificates $root
    }
    
    function AddNewVnet($subscriptionId, $webAppResourceGroup, $webAppName)
    {
	    Write-Host "Adding a new Vnet"
	    Write-Host
	    $vnetName = Read-Host "Specify a name for this Virtual Network"
	    
	    $vnetGatewayName="$($vnetName)-gateway"
	    $vnetIpName="$($vnetName)-ip"
	    $vnetIpConfigName="$($vnetName)-ipconf"
	    
	    # Virtual Network settings
	    $vnetAddressSpace="10.0.0.0/8"
	    $vnetGatewayAddressSpace="10.5.0.0/16"
	    $vnetPointToSiteAddressSpace="172.16.0.0/16"
	    
	    $changeRequested = 0
	    $resourceGroupName = $webAppResourceGroup
	    
	    while($changeRequested -eq 0)
	    {
		    Write-Host
		    Write-Host "Currently, I will create a VNET with the following settings:"
		    Write-Host
		    Write-Host "Virtual Network Name: $vnetName"
		    Write-Host "Resource Group Name:  $resourceGroupName"
		    Write-Host "Gateway Name: $vnetGatewayName"
		    Write-Host "Vnet IP name: $vnetIpName"
		    Write-Host "Vnet IP config name:  $vnetIpConfigName"
		    Write-Host "Address Space:$vnetAddressSpace"
		    Write-Host "Gateway Address Space:$vnetGatewayAddressSpace"
		    Write-Host "Point-To-Site Address Space:  $vnetPointToSiteAddressSpace"
		    Write-Host
		    $changeRequested = PromptYesNo "" "Do you wish to change these settings?" 1
		    
		    if($changeRequested -eq 0)
		    {
			    $vnetName = ReadHostWithDefault "Virtual Network Name" $vnetName
			    $resourceGroupName = ReadHostWithDefault "Resource Group Name" $resourceGroupName
			    $vnetGatewayName = ReadHostWithDefault "Vnet Gateway Name" $vnetGatewayName
			    $vnetIpName = ReadHostWithDefault "Vnet IP name" $vnetIpName
			    $vnetIpConfigName = ReadHostWithDefault "Vnet IP configuration name" $vnetIpConfigName
			    $vnetAddressSpace = ReadHostWithDefault "Vnet Address Space" $vnetAddressSpace
			    $vnetGatewayAddressSpace = ReadHostWithDefault "Vnet Gateway Address Space" $vnetGatewayAddressSpace
			    $vnetPointToSiteAddressSpace = ReadHostWithDefault "Vnet Point-to-site Address Space" $vnetPointToSiteAddressSpace
		    }
	    }
    
	    $ErrorActionPreference = "Stop";
	    
	    # We create the virtual network and add it here. The way this works is:
	    # 1) Add the VNET association to the App. This allows the App to generate certificates, etc. for the VNET.
	    # 2) Create the VNET and VNET gateway, add the certificates, create the public IP, etc., required for the gateway
	    # 3) Get the VPN package from the gateway and pass it back to the App.
	    
	    $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup
	    $location = $webApp.Location
	    
	    Write-Host "Creating App assocation to VNET"
	    $propertiesObject = @{
	     "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($resourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
	    }
	    $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force
	       
	    CreateVnet $resourceGroupName $vnetName $vnetAddressSpace $vnetGatewayAddressSpace $location
	    
	    CreateVnetGateway $resourceGroupName $vnetName $vnetIpName $location $vnetIpConfigName $vnetGatewayName $virtualNetwork.Properties.CertBlob $vnetPointToSiteAddressSpace
	    
	    Write-Host "Retreiving VPN Package and supplying to App"
	    $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $resourceGroupName -VirtualNetworkGatewayName $vnetGatewayName -ProcessorArchitecture Amd64
	    
	    # Put the VPN client configuration package onto the App
	    $PropertiesObject = @{
	    "vnetName" = $VirtualNetworkName; "vpnPackageUri" = $packageUri
	    }
	    
	    New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)/primary" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force
	    
	    Write-Host "Finished!"
    }
    
    function AddExistingVnet($subscriptionId, $resourceGroupName, $webAppName)
    {
		$ErrorActionPreference = "Stop";
		
		# At this point, the gateway should be able to be joined to an App, but may require some minor tweaking. We will declare to the App now to use this VNET
		Write-Host "Getting App information"
		$webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
		$location = $webApp.Location
		   
		$webAppConfig = Get-AzureRmResource -ResourceName "$($webAppName)/web" -ResourceType "Microsoft.Web/sites/config" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
		$currentVnet = $webAppConfig.Properties.VnetName
		if($currentVnet -ne $null -and $currentVnet -ne "")
		{
			Write-Host "Currently connected to VNET $currentVnet"
		}
		
		# Display existing vnets
		$vnets = Get-AzureRmVirtualNetwork
		$vnetNames = @()
		foreach($vnet in $vnets)
		{
			$vnetNames += $vnet.Name
	    }
	    
	    Write-Host
	    $vnet = PromptCustom "Select a VNET to integrate with" $vnets $vnetNames
	    
	    # We need to check if this VNET is able to be joined to a App, based on following criteria
		    # If there is no gateway, we can create one.
		    # If there is a gateway:
			    # It must be of type Vpn
			    # It must be of VpnType RouteBased
			    # If it doesn't have the right certificate, we will need to add it.
			    # If it doesn't have a point-to-site range, we will need to add it.
	       
	    $gatewaySubnet = $vnet.Subnets | Where-Object { $_.Name -eq "GatewaySubnet" }
	       
	    if($gatewaySubnet -eq $null -or $gatewaySubnet.IpConfigurations -eq $null -or $gatewaySubnet.IpConfigurations.Count -eq 0)
	    {
			$ErrorActionPreference = "Continue";
		    # There is no gateway. We need to create one.
		    Write-Host "This Virtual Network has no gateway. I will need to create one."
		    
		    $vnetName = $vnet.Name
		    $vnetGatewayName="$($vnetName)-gateway"
		    $vnetIpName="$($vnetName)-ip"
		    $vnetIpConfigName="$($vnetName)-ipconf"
		    
		    # Virtual Network settings
		    $vnetAddressSpace="10.0.0.0/8"
		    $vnetGatewayAddressSpace="10.5.0.0/16"
		    $vnetPointToSiteAddressSpace="172.16.0.0/16"
		    
		    $changeRequested = 0
		    
		    Write-Host "Your VNET is in the address space $($vnet.AddressSpace.AddressPrefixes), with the following Subnets:"
		    foreach($subnet in $vnet.Subnets)
		    {
			    Write-Host "$($subnet.Name): $($subnet.AddressPrefix)"
		    }
		    
		    $vnetGatewayAddressSpace = Read-Host "Please choose a GatewaySubnet address space"
		    
		    while($changeRequested -eq 0)
		    {
			    Write-Host
			    Write-Host "Currently, I will create a VNET gateway with the following settings:"
			    Write-Host
			    Write-Host "Virtual Network Name: $vnetName"
			    Write-Host "Resource Group Name:  $($vnet.ResourceGroupName)"
			    Write-Host "Gateway Name: $vnetGatewayName"
			    Write-Host "Vnet IP name: $vnetIpName"
			    Write-Host "Vnet IP config name:  $vnetIpConfigName"
			    Write-Host "Address Space:$($vnet.AddressSpace.AddressPrefixes)"
			    Write-Host "Gateway Address Space:$vnetGatewayAddressSpace"
			    Write-Host "Point-To-Site Address Space:  $vnetPointToSiteAddressSpace"
			    Write-Host
			    $changeRequested = PromptYesNo "" "Do you wish to change these settings?" 1
			    
			    if($changeRequested -eq 0)
			    {
				    $vnetGatewayName = ReadHostWithDefault "Vnet Gateway Name" $vnetGatewayName
				    $vnetIpName = ReadHostWithDefault "Vnet IP name" $vnetIpName
				    $vnetIpConfigName = ReadHostWithDefault "Vnet IP configuration name" $vnetIpConfigName
				    $vnetGatewayAddressSpace = ReadHostWithDefault "Vnet Gateway Address Space" $vnetGatewayAddressSpace
				    $vnetPointToSiteAddressSpace = ReadHostWithDefault "Vnet Point-to-site Address Space" $vnetPointToSiteAddressSpace
			    }
		    }
		    
		    $ErrorActionPreference = "Stop";
		    
		    Write-Host "Creating App assocation to VNET"
		    $propertiesObject = @{
		     "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
		    }
		    
		    $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force
		    
		    # If there is no gateway subnet, we need to create one.
		    if($gatewaySubnet -eq $null)
		    {
			    $gatewaySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix $vnetGatewayAddressSpace
			    $vnet.Subnets.Add($gatewaySubnet);
			    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
		    }
		    
		    CreateVnetGateway $vnet.ResourceGroupName $vnetName $vnetIpName $location $vnetIpConfigName $vnetGatewayName $virtualNetwork.Properties.CertBlob $vnetPointToSiteAddressSpace
		    
		    $gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $vnet.ResourceGroupName -Name $vnetGatewayName
	    }
	    else
	    {
		    $uriParts = $gatewaySubnet.IpConfigurations[0].Id.Split('/')
		    $gatewayResourceGroup = $uriParts[4]
		    $gatewayName = $uriParts[8]
		       
		    $gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $vnet.ResourceGroupName -Name $gatewayName
		    
		    # validate gateway types, etc.
		    if($gateway.GatewayType -ne "Vpn")
		    {
			    Write-Error "This gateway is not of the Vpn type. It cannot be joined to an App."
			    return
		    }
		    
		    if($gateway.VpnType -ne "RouteBased")
		    {
			    Write-Error "This gateways Vpn type is not RouteBased. It cannot be joined to an App."
			    return
		    }
		    
		    if($gateway.VpnClientConfiguration -eq $null -or $gateway.VpnClientConfiguration.VpnClientAddressPool -eq $null)
		    {
			    Write-Host "This gateway does not have a Point-to-site Address Range. Please specify one in CIDR notation, e.g. 10.0.0.0/8"
			    $pointToSiteAddress = Read-Host "Point-To-Site Address Space"
			    Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $gateway.Name -VpnClientAddressPool $pointToSiteAddress
		    }
		    
		    Write-Host "Creating App assocation to VNET"
		    $propertiesObject = @{
		     "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
		    }
		    
		    $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force
		    
		    # We need to check if the certificate here exists in the gateway.
		    $certificates = $gateway.VpnClientConfiguration.VpnClientRootCertificates
		    
		    $certFound = $false
		    foreach($certificate in $certificates)
		    {
			    if($certificate.PublicCertData -eq $virtualNetwork.Properties.CertBlob)
			    {
				    $certFound = $true
				    break
			    }
		    }
		    
		    if(-not $certFound)
		    {
			    Write-Host "Adding certificate"
			    Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName "AppServiceCertificate.cer" -PublicCertData $virtualNetwork.Properties.CertBlob -VirtualNetworkGatewayName $gateway.Name
		    }
	    }
	    
	    # Now finish joining by getting the VPN package and giving it to the App
	    Write-Host "Retreiving VPN Package and supplying to App"
	    $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $vnet.ResourceGroupName -VirtualNetworkGatewayName $gateway.Name -ProcessorArchitecture Amd64
	    
	    # Put the VPN client configuration package onto the App
	    $PropertiesObject = @{
	    "vnetName" = $vnet.Name; "vpnPackageUri" = $packageUri
	    }
	    
	    New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)/primary" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force
	    
	    Write-Host "Finished!"
    }
       
    function RemoveVnet($subscriptionId, $resourceGroupName, $webAppName)
    {
	    $webAppConfig = Get-AzureRmResource -ResourceName "$($webAppName)/web" -ResourceType "Microsoft.Web/sites/config" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
	    $currentVnet = $webAppConfig.Properties.VnetName
	    if($currentVnet -ne $null -and $currentVnet -ne "")
	    {
		    Write-Host "Currently connected to VNET $currentVnet"
		    
		    Remove-AzureRmResource -ResourceName "$($webAppName)/$($currentVnet)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
	    }
	  	  else
	    {
	  	  Write-Host "Not connected to a VNET."
	    }
    }
    
    Write-Host "Please Login"
    Login-AzureRmAccount
    
    # Choose subscription. If there's only one we will choose automatically
    
    $subs = Get-AzureRmSubscription
    $subscriptionId = ""
    
    if($subs.Length -eq 0)
    {
	    Write-Error "No subscriptions bound to this account."
	    return
    }
    
    if($subs.Length -eq 1)
    {
	    $subscriptionId = $subs[0].SubscriptionId
    }
    else
    {
	    $subscriptionChoices = @()
	    $subscriptionValues = @()
	       
	    foreach($subscription in $subs){
	    $subscriptionChoices += "$($subscription.SubscriptionName) ($($subscription.SubscriptionId))";
	    $subscriptionValues += ($subscription.SubscriptionId);
	    }
	    
	    $subscriptionId = PromptCustom "Choose a subscription" $subscriptionValues $subscriptionChoices
    }
    
    Select-AzureRmSubscription -SubscriptionId $subscriptionId
    
    $resourceGroup = Read-Host "Please enter the Resource Group of your App"
    
    $appName = Read-Host "Please enter the Name of your App"
    
    $options = @("Add a NEW Virtual Network to an App", "Add an EXISTING Virtual Network to an App", "Remove a Virtual Network from an App");
    $optionValues = @(0, 1, 2)
    $option = PromptCustom "What do you want to do?" $optionValues $options
    
    if($option -eq 0)
    {
		AddNewVnet $subscriptionId $resourceGroup $appName
    }
    if($option -eq 1)
    {
	    AddExistingVnet $subscriptionId $resourceGroup $appName
    }
	if($option -eq 2)
    {
	    RemoveVnet $subscriptionId $resourceGroup $appName
    }
    
Speichern Sie eine Kopie des Skripts. In diesem Dokument wurde es „V2VnetAllinOne.ps1“ genannt, aber Sie können einen beliebigen Namen verwenden. Es gibt keine Argumente für dieses Skript. Sie führen es einfach aus. Als Erstes werden Sie vom Skript aufgefordert, sich anzumelden. Nach der Anmeldung ruft das Skript die Details zu Ihrem Konto ab und gibt eine Liste von Abonnements zurück, zwischen denen Sie wählen können. Ohne das Popupfenster zur Eingabe Ihrer Anmeldeinformationen sieht die erste Skriptausführung wie folgt aus:

	PS C:\Users\ccompy\Documents\VNET> .\V2VnetAllInOne.ps1
	Please Login
	
	Environment           : AzureCloud
	Account               : ccompy@microsoft.com
	TenantId              : 722278f-fef1-499f-91ab-2323d011db47
	SubscriptionId        : af5358e1-acac-2c90-a9eb-722190abf47a
	CurrentStorageAccount :
	
	Choose a subscription
	
	1) Demo Subscription (af5358e1-acac-2c90-a9eb-722190abf47a)
	2) MS Test (a5350f55-dd5a-41ec-2ddb-ff7b911bb2ef)
	3) Purple Demo Subscription (2d4c99a4-57f9-4d5e-a0a1-0034c52db59d)
	
	Choose an option:
	3
	
	Account      : ccompy@microsoft.com
	Environment  : AzureCloud
	Subscription : 2d4c99a4-57f9-4d5e-a0a1-0034c52db59d
	Tenant       : 722278f-fef1-499f-91ab-2323d011db47
	
	Please enter the Resource Group of your App: hcdemo-rg
	Please enter the Name of your App: v2vnetpowershell
	What do you want to do?
	
	1) Add a NEW Virtual Network to an App
	2) Add an EXISTING Virtual Network to an App
	3) Remove a Virtual Network from an App

Im Rest dieses Dokuments werden die drei einzelnen Optionen erläutert.

###Erstellen eines V2-VNET (Ressourcen-Manager) und Einrichten der Integration###

Wählen Sie zum Erstellen eines neuen V2-VNET und zum Integrieren Ihrer App „1) Add a NEW Virtual Network to an App“ aus. Daraufhin werden Sie aufgefordert, den Namen des VNET anzugeben. Ich habe unten den Namen „v2pshell“ verwendet.

Das Skript enthält die Details über das VNET, das erstellt wird. Bei Bedarf kann ich die Werte ändern. In dieser Beispielausführung habe ich ein VNET mit den folgenden Einstellungen erstellt:

	Virtual Network Name:         v2pshell
	Resource Group Name:          hcdemo-rg
	Gateway Name:                 v2pshell-gateway
	Vnet IP name:                 v2pshell-ip
	Vnet IP config name:          v2pshell-ipconf
	Address Space:                10.0.0.0/8
	Gateway Address Space:        10.5.0.0/16
	Point-To-Site Address Space:  172.16.0.0/12
	
	Do you wish to change these settings?
	[Y] Yes  [N] No  [?] Help (default is "N"):

Wenn Sie die Werte ändern möchten, geben Sie „Y“ ein, und nehmen Sie die gewünschten Änderungen vor. Wenn Sie mit den VNET-Einstellungen zufrieden sind, geben Sie „N“ ein, oder drücken Sie einfach die EINGABETASTE, wenn Sie zum Ändern der Einstellungen aufgefordert werden. Von hier an bis zum Abschluss informiert das Skript Sie über einige der ausgeführten Aktionen, bis das VNET-Gateway erstellt wird. Dieser Schritt kann bis zu einer Stunde dauern. Es gibt in dieser Phase keine Statusanzeige, aber Sie werden entsprechend informiert, sobald das Gateway erstellt wurde.

Bei Abschluss des Skripts wird **Finished** angezeigt. An diesem Punkt verfügen Sie über ein V2-VNET, das mit dem Namen und den Einstellungen, die Sie ausgewählt haben, erstellt wurde. In dieses neue VNET kann Ihre App integriert werden.

###Integrieren Ihrer App in ein bereits vorhandenes V2-VNET###

Wenn Sie bei der Integration in ein bereits vorhandenes VNET ein V2-VNET bereitstellen, das nicht über ein Gateway oder eine Punkt-zu-Standort-Konfiguration verfügt, wird beides vom Skript eingerichtet. Wenn im VNET beides bereits eingerichtet ist, beginnt direkt die App-Integration. Starten Sie diesen Prozess einfach, indem Sie die Option „2) Add an EXISTING Virtual Network to an App“ auswählen.

Diese Option funktioniert nur, wenn Sie über ein bereits vorhandenes V2-VNET verfügen, das zum selben Abonnement wie Ihre App gehört. Nach Auswahl der Option 2 wird eine Liste von V2-VNETs angezeigt.

	Select a VNET to integrate with
	
	1) v2demonetwork
	2) v2pshell
	3) v2vnetintdemo
	4) v2asenetwork
	5) v2pshell2
	
	Choose an option:
	5

Wählen Sie einfach das VNET aus, das Sie für die Integration verwenden möchten. Wenn Sie bereits über ein Gateway mit aktivierter Punkt-zu-Standort-Konfiguration verfügen, dann integriert das Skript einfach Ihre App in Ihr VNET. Wenn Sie nicht über ein Gateway verfügen, müssen Sie das Gatewaysubnetz angeben. Das Gatewaysubnetz muss zu Ihrem VNET-Adressraum gehören, und es darf sich nicht in einem anderen Subnetz befinden. Wenn Sie also ein VNET ohne ein Gateway haben und diesen Schritt ausführen, sieht das Ergebnis wie folgt aus:

	This Virtual Network has no gateway. I will need to create one.
	Your VNET is in the address space 172.16.0.0/16, with the following Subnets:
	default: 172.16.0.0/24
	Please choose a GatewaySubnet address space: 172.16.1.0/26

In diesem Beispiel habe ich ein VNET-Gateway mit den folgenden Einstellungen erstellt:

	Virtual Network Name:         v2pshell2
	Resource Group Name:          vnetdemo-rg
	Gateway Name:                 v2pshell2-gateway
	Vnet IP name:                 v2pshell2-ip
	Vnet IP config name:          v2pshell2-ipconf
	Address Space:                172.16.0.0/16
	Gateway Address Space:        172.16.1.0/26
	Point-To-Site Address Space:  172.16.0.0/12
	
	Do you wish to change these settings?
	[Y] Yes  [N] No  [?] Help (default is "N"):
	Creating App assocation to VNET

Sie können diese Einstellungen bei Bedarf ändern. Drücken Sie andernfalls die EINGABETASTE, und die Skriptausführung wird fortgesetzt, Ihr Gateway wird erstellt, und Ihre App wird Ihrem VNET angefügt. Die Dauer der Gatewayerstellung beträgt noch immer eine Stunde. Das sollten Sie im Auge behalten. Wenn alles erledigt ist, zeigt das Skript „Finished“ an.

###Trennen Ihrer App von einem V2-VNET###

Durch das Trennen Ihrer App von Ihrem V2-VNET werden das Gateway und die Punkt-zu-Standort-Verbindung nicht deaktiviert. Sie könnten sie schließlich noch für andere Zwecke verwenden. Die Trennung bezieht sich zudem nur auf die angegebene App. Wählen Sie zum Ausführen dieser Aktion „3) Remove a Virtual Network from an App“ aus. Dann sollte etwa Folgendes angezeigt werden:

	Currently connected to VNET v2pshell
	
	Confirm
	Are you sure you want to delete the following resource:
	/subscriptions/edcc99a4-b7f9-4b5e-a9a1-3034c51db496/resourceGroups/hcdemo-rg/providers/Microsoft.Web/sites/v2vnetpowers
	hell/virtualNetworkConnections/v2pshell
	[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

Im Skript findet sich das Wort „delete“ (löschen), aber das VNET wird nicht gelöscht. Es wird nur die Integration entfernt. Nachdem Sie bestätigt haben, dass Sie dies tun möchten, wird der Befehl ziemlich schnell verarbeitet und zeigt abschließend „True“ an.

<!--Links-->
[createvpngateway]: http://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/
[azureportal]: http://portal.azure.com

<!---HONumber=AcomDC_0406_2016-->