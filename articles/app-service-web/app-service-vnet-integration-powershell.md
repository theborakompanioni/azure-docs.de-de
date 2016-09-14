<properties
	pageTitle="Verbinden Ihrer App mit Ihrem virtuellen Netzwerk mithilfe von PowerShell"
	description="Informationen zum Herstellen einer Verbindung und zum Arbeiten mit virtuellen Netzwerken mithilfe von PowerShell"
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
	ms.date="08/29/2016"
	ms.author="ccompy"/>

# Verbinden Ihrer App mit Ihrem virtuellen Netzwerk mithilfe von PowerShell #

## Übersicht ##

In Azure App Service können Sie Ihre App (Web, mobil oder API) mit einem virtuellen Azure-Netzwerk (VNET) in Ihrem Abonnement verbinden. Dieses Feature heißt „VNET-Integration“. Das Feature „VNET-Integration“ sollte nicht mit dem Feature „App Service-Umgebung“ verwechselt werden, mit dem Sie eine Instanz von Azure App Service in Ihrem virtuellen Netzwerk ausführen können.

Das Feature „VNET-Integration“ bietet im neuen Portal eine Benutzeroberfläche (UI), mit der Sie virtuelle Netzwerke integrieren können, die entweder über das klassische Bereitstellungsmodell oder über das Azure Resource Manager-Bereitstellungsmodell bereitgestellt werden. Weitere Informationen über das Feature finden Sie unter [Integrieren Ihrer App in ein Azure Virtual Network](web-sites-integrate-with-vnet.md).

Dieser Artikel behandelt weniger die Verwendung der Benutzeroberfläche, sondern vielmehr das Ermöglichen der Integration über PowerShell. Da die Befehle für jedes Bereitstellungsmodell unterschiedlich lauten, enthält dieser Artikel einen Abschnitt für jedes Bereitstellungsmodell.

Bevor Sie mit diesem Artikel fortfahren, stellen Sie sicher, dass Sie über Folgendes verfügen:

- Eine Installation des neuesten Azure PowerShell SDK. Sie können es mit dem Webplattform-Installer installieren.
- Eine App in Azure App Service, die in einer Standard- oder Premium-SKU ausgeführt wird.

## Klassische virtuelle Netzwerke ##

Dieser Abschnitt erläutert drei Aufgaben für virtuelle Netzwerke, die das klassischen Bereitstellungsmodell verwenden:

1. Verbinden Ihrer App mit einem bereits vorhandenen virtuellen Netzwerk, das über ein Gateway verfügt und für Punkt-zu-Standort-Verbindungen konfiguriert ist.
1. Aktualisieren Ihrer VNET-Integrationsinformationen für Ihre App.
1. Trennen Ihrer App von Ihrem virtuellen Netzwerk.

### Verbinden einer App mit einem klassischen VNET ###

Um eine App mit einem virtuellen Netzwerk zu verbinden, führen Sie die folgenden drei Schritte:

1. Deklarieren Sie in der Web-App, dass sie mit einem bestimmten virtuellen Netzwerk verknüpft wird. Die App generiert ein Zertifikat, das dem virtuellen Netzwerk übergeben wird, um Punkt-zu-Standort-Verbindungen herzustellen.
1. Laden Sie das Web-App-Zertifikat in das virtuelle Netzwerk hoch, und rufen Sie dann den URI für das Punkt-zu-Standort-VPN-Paket ab.
1. Aktualisieren Sie die virtuelle Netzwerkverbindung der Web-App mit dem Punkt-zu-Standort-Paket-URI.

Der erste und der dritte Schritt können vollständig mit Skripts ausgeführt werden. Der zweite Schritt erfordert jedoch eine einmalige manuelle Aktion über das Portal oder Zugriff zum Ausführen von **PUT**- oder **PATCH**-Aktionen auf dem ARM-Endpunkt (Azure Resource Manager) des virtuellen Netzwerks. Wenden Sie sich an den Azure-Support, um diese Option aktivieren zu lassen. Bevor Sie beginnen, stellen Sie sicher, dass ein klassisches virtuelles Netzwerk mit Punkt-zu-Standort-Verbindung bereits aktiviert ist und ein Gateway bereitgestellt wurde. Zum Erstellen des Gateways und zum Aktivieren der Punkt-zu-Standort-Verbindung müssen Sie das Portal verwenden, wie unter [Erstellen eines VPN-Gateways][createvpngateway] beschrieben.

Das klassische virtuelle Netzwerk muss zum gleichen Abonnement gehören wie der App Service-Plan mit der App, für die Sie die Integration einrichten.

##### Einrichten des Azure PowerShell SDK #####

Öffnen Sie ein PowerShell-Fenster, und richten Sie Ihr Azure-Konto und das Abonnement mit folgendem Befehl ein:

	Login-AzureRmAccount

Dieser Befehl öffnet eine Eingabeaufforderung zur Eingabe Ihrer Azure-Anmeldeinformationen. Nachdem Sie sich angemeldet haben, verwenden Sie einen der folgenden Befehle zur Auswahl des Abonnements, das Sie verwenden möchten. Sie müssen das Abonnement verwenden, zu dem Ihr virtuelles Netzwerk und der App Service-Plan gehören.

	Select-AzureRmSubscription –SubscriptionName [WebAppSubscriptionName]

oder

	Select-AzureRmSubscription –SubscriptionId [WebAppSubscriptionId]

##### In diesem Artikel verwendete Variablen #####

Zur Vereinfachung der Befehle legen wir eine **$Configuration**-PowerShell-Variable mit der spezifischen Konfiguration fest.

Legen Sie eine Variable wie folgt in PowerShell mit den folgenden Parametern fest:

	$Configuration = @{}
	$Configuration.WebAppResourceGroup = "[Your web app resource group]"
	$Configuration.WebAppName = "[Your web app name]"
	$Configuration.VnetSubscriptionId = "[Your vnet subscription id]"
	$Configuration.VnetResourceGroup = "[Your vnet resource group]"
	$Configuration.VnetName = "[Your vnet name]"

Der App-Standort sollte der Standort (in englischer Sprache) ohne Leerzeichen sein. Beispiel: „USA, Westen“ entspricht „West US“, daher „westus“.

	$Configuration.WebAppLocation = "[Your web app Location]"

Das nächste Element ist die Angabe, wohin das Zertifikat geschrieben werden soll. Es sollte ein beschreibbarer Pfad auf Ihrem lokalen Computer sein. Vergessen Sie nicht, am Ende „.cer“ anzufügen.

	$Configuration.GeneratedCertificatePath = "[C:\Path\To\Certificate.cer]"

Um zu sehen, was Sie festgelegt haben, geben Sie **$Configuration** ein.

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

Im Rest dieses Abschnitts wird davon ausgegangen, dass Sie eine Variable wie gerade beschrieben erstellt haben.

##### Deklarieren des virtuellen Netzwerks für die App #####

Verwenden Sie den folgenden Befehl, um für die App anzugeben, dass dieses bestimmte virtuelle Netzwerk verwendet wird. Dadurch generiert die App die erforderlichen Zertifikate:

	$vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -PropertyObject @{"VnetResourceId" = "/subscriptions/$($Configuration.VnetSubscriptionId)/resourceGroups/$($Configuration.VnetResourceGroup)/providers/Microsoft.ClassicNetwork/virtualNetworks/$($Configuration.VnetName)"} -Location $Configuration.WebAppLocation -ApiVersion 2015-07-01

Wenn dieser Befehl erfolgreich ausgeführt wird, sollte **$vnet** eine **Properties**-Variable enthalten. Die **Properties**-Variable sollte sowohl einen Zertifikatfingerabdruck als auch Zertifikatdaten enthalten.

##### Hochladen des Web-App-Zertifikats in das virtuelle Netzwerk #####

Ein einmaliger manueller Schritt ist für jede Kombination aus Abonnement und virtuellem Netzwerk erforderlich. Wenn Sie also Apps im Abonnement A mit dem virtuellen Netzwerk A verbinden, müssen Sie diesen Schritt nur einmal ausführen, unabhängig davon, wie viele Apps Sie konfigurieren. Wenn Sie einem anderen virtuellen Netzwerk eine neue App hinzufügen, müssen Sie diesen Schritt erneut durchführen. Der Grund dafür ist, dass eine Gruppe von Zertifikaten auf Abonnementebene in Azure App Service erstellt wird, und diese Gruppe wird einmal für jedes virtuelle Netzwerk generiert, mit dem die Apps eine Verbindung herstellen.

Die Zertifikate wurden bereits festgelegt, wenn Sie diese Schritte durchgeführt haben oder wenn Sie die Integration mit dem gleichen virtuellen Netzwerk mithilfe des Portals durchgeführt haben.

Der erste Schritt ist das Generieren der CER-Datei. Der zweite Schritt ist das Hochladen der CER-Datei in Ihr virtuelles Netzwerk. Um die CER-Datei aus dem API-Aufruf im vorherigen Schritt zu generieren, führen Sie die folgenden Befehle aus.

	$certBytes = [System.Convert]::FromBase64String($vnet.Properties.certBlob)
	[System.IO.File]::WriteAllBytes("$($Configuration.GeneratedCertificatePath)", $certBytes)

Das Zertifikat befindet sich an dem durch **$Configuration.GeneratedCertificatePath** angegebenen Speicherort.

Verwenden Sie zum manuellen Hochladen des Zertifikats das [Azure-Portal][azureportal], und wechseln Sie zu **Virtuelles Netzwerk (klassisch)** > **VPN-Verbindungen** > **Punkt-zu-Standort** > **Zertifikate verwalten**. Hier können Sie Ihr Zertifikat hochladen.

##### Abrufen des Punkt-zu-Standort-Pakets #####

Der nächste Schritt beim Einrichten einer virtuellen Netzwerkverbindung in einer Web-App ist, das Punkt-zu-Standort-Paket abzurufen und Ihrer Web-App bereitzustellen.

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


Die Variable **$output.Outputs.packageUri** enthält jetzt den Paket-URI, den Ihre Web-App benötigt.

##### Hochladen des Punkt-zu-Standort-Pakets in Ihre App #####

Mit dem letzten Schritt wird der App dieses Paket bereitgestellt. Führen Sie einfach den nächsten Befehl aus:

	$vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)/primary" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-07-01 -PropertyObject @{"VnetName" = $Configuration.VnetName ; "VpnPackageUri" = $($output.Outputs.packageUri).Value } -Location $Configuration.WebAppLocation

Wenn Sie in einer Meldung aufgefordert werden zu bestätigen, dass eine vorhandene Ressource überschrieben wird, müssen Sie dies zulassen.

Nachdem dieser Befehl erfolgreich ausgeführt wurde, sollte Ihre App mit dem virtuellen Netzwerk verbunden sein. Um den Erfolg zu überprüfen, wechseln zu Ihrer App-Konsole, und geben Sie Folgendes ein:

	SET WEBSITE_

Wenn eine Umgebungsvariable namens WEBSITE\_VNETNAME mit einem Wert, der mit dem Namen des virtuellen Zielnetzwerks übereinstimmt, vorhanden ist, wurden alle Konfigurationen erfolgreich abgeschlossen.

### Aktualisieren der Informationen zur klassischen VNET-Integration ###

Um Ihre Informationen zu aktualisieren oder erneut zu synchronisieren, wiederholen Sie einfach die Schritte, die Sie beim ersten Erstellen der Integration ausgeführt haben. Diese Schritte sind:

1. Definieren von Konfigurationsinformationen.
1. Deklarieren des virtuellen Netzwerks für die App.
1. Abrufen des Punkt-zu-Standort-Pakets.
1. Hochladen des Punkt-zu-Standort-Pakets in Ihre App.

### Trennen Ihrer App von einem klassischen VNET. ###

Zum Trennen benötigen Sie die Konfigurationsinformationen, die während der Integration des virtuellen Netzwerks festgelegt wurden. Wenn Sie diese Informationen verwenden, gibt es einen Befehl zum Trennen Ihrer App vom virtuellen Netzwerk.

	$vnet = Remove-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-07-01

## Virtuelle Netzwerke mit Resource Manager ##

Virtuelle Resource Manager-Netzwerke verfügen über Azure Resource Manager-APIs, die im Vergleich mit klassischen virtuellen Netzwerk einige Prozesse vereinfachen. Wir verwenden ein Skript, mit dem Sie die folgenden Aufgaben ausführen können:

- Erstellen eines virtuellen Resource Manager-Netzwerks und Integrieren Ihrer Apps in dieses Netzwerk.
- Erstellen eines Gateways, Konfigurieren der Punkt-zu-Standort-Konnektivität in einem bereits vorhandenen virtuellen Resource Manager-Netzwerk und Integrieren Ihrer App in dieses Netzwerk.
- Integrieren Ihrer App in ein vorhandenes virtuelles Resource Manager-Netzwerk, für das ein Gateway und Punkt-zu-Standort-Verbindungen aktiviert sind.
- Trennen Ihrer App von Ihrem virtuellen Netzwerk.

### Skript für die Integration eines Resource Manager-VNET in App Service ###

Kopieren Sie das folgende Skript, und speichern Sie es in einer Datei. Sie müssen das Skript nicht in der vorliegenden Form verwenden. Sie können es aber nutzen, um zu erfahren, wie Sie ein virtuelles Resource Manager-Netzwerk einrichten können.


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

	    Write-Host "Creating App association to VNET"
	    $propertiesObject = @{
	     "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($resourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
	    }
	    $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

	    CreateVnet $resourceGroupName $vnetName $vnetAddressSpace $vnetGatewayAddressSpace $location

	    CreateVnetGateway $resourceGroupName $vnetName $vnetIpName $location $vnetIpConfigName $vnetGatewayName $virtualNetwork.Properties.CertBlob $vnetPointToSiteAddressSpace

	    Write-Host "Retrieving VPN Package and supplying to App"
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

		    Write-Host "Creating App association to VNET"
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

		    Write-Host "Creating App association to VNET"
		    $propertiesObject = @{
		     "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnet.Name)"
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
	    Write-Host "Retrieving VPN Package and supplying to App"
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

Speichern Sie eine Kopie des Skripts. In diesem Artikel heißt es „V2VnetAllinOne.ps1“, aber Sie können auch einen anderen Namen verwenden. Es gibt keine Argumente für dieses Skript. Sie führen es einfach aus. Als Erstes werden Sie vom Skript aufgefordert, sich anzumelden. Nach der Anmeldung ruft das Skript die Details zu Ihrem Konto ab und gibt eine Liste von Abonnements zurück. Ohne die Abfrage Ihrer Anmeldeinformationen sieht die erste Skriptausführung wie folgt aus:

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

Im Rest dieses Abschnitts wird jede dieser drei Optionen beschrieben.

### Erstellen eines Resource Manager-VNET und Einrichten der Integration ###

Um ein neues virtuelles Netzwerk mit dem Resource Manager-Bereitstellungsmodell zu erstellen und in Ihre App zu integrieren, wählen Sie **1) Add a NEW Virtual Network to an App**. Daraufhin werden Sie aufgefordert, den Namen des virtuellen Netzwerks anzugeben. In diesem Fall sehen Sie in den folgenden Einstellungen, dass der Name „v2pshell“ verwendet wurde.

Das Skript enthält die Details über das virtuelle Netzwerk, das erstellt wird. Bei Bedarf kann ich die Werte ändern. In dieser Beispielausführung habe ich ein virtuelles Netzwerk mit den folgenden Einstellungen erstellt:

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

Wenn Sie die Werte ändern möchten, geben Sie **Y** ein, und nehmen Sie die gewünschten Änderungen vor. Wenn Sie mit den Einstellungen des virtuellen Netzwerks zufrieden sind, geben Sie **N** ein, oder drücken Sie einfach die EINGABETASTE, wenn Sie zum Ändern der Einstellungen aufgefordert werden. Von hier an bis zum Abschluss informiert das Skript Sie über einige der ausgeführten Aktionen, bis das virtuelle Netzwerkgateway erstellt wird. Dieser Schritt kann bis zu einer Stunde dauern. Es gibt in dieser Phase keine Statusanzeige, aber Sie werden entsprechend informiert, sobald das Gateway erstellt wurde.

Bei Abschluss des Skripts wird **Finished** angezeigt. Jetzt verfügen Sie über ein virtuelles Resource Manager-Netzwerk mit dem Namen und den Einstellungen, die Sie ausgewählt haben. Dieses neue virtuelle Netzwerk wird auch in Ihre App integriert.

### Integrieren Ihrer App in ein bereits vorhandenes Resource Manager-VNET ###

Wenn Sie die Integration in ein bereits vorhandenes virtuelles Netzwerk vornehmen und dabei ein virtuelles Resource Manager-Netzwerk bereitstellen, das weder ein Gateway noch eine Punkt-zu-Standort-Verbindung aufweist, werden diese über das Skript eingerichtet. Wenn im VNET bereits beides eingerichtet ist, nimmt das Skript direkt die App-Integration vor. Starten Sie diesen Prozess einfach, indem Sie **2) Add an EXISTING Virtual Network to an App** auswählen.

Diese Option funktioniert nur, wenn Sie über ein vorhandenes virtuelles Resource Manager-Netzwerk verfügen, das sich im selben Abonnement befindet wie Ihre App. Nachdem Sie die Option ausgewählt haben, wird Ihnen eine Liste der virtuellen Resource Manager-Netzwerke angezeigt.

	Select a VNET to integrate with

	1) v2demonetwork
	2) v2pshell
	3) v2vnetintdemo
	4) v2asenetwork
	5) v2pshell2

	Choose an option:
	5

Wählen Sie einfach das virtuelle Netzwerk aus, das Sie für die Integration verwenden möchten. Wenn Sie bereits über ein Gateway mit aktivierter Punkt-zu-Standort-Konnektivität verfügen, integriert das Skript einfach Ihre App in Ihr virtuelles Netzwerk. Wenn Sie nicht über ein Gateway verfügen, müssen Sie das Gatewaysubnetz angeben. Das Gatewaysubnetz muss zum Adressraum Ihres virtuellen Netzwerks gehören, und es darf sich nicht in einem anderen Subnetz befinden. Wenn Sie also ein virtuelles Netzwerk ohne ein Gateway haben und diesen Schritt ausführen, sieht das Ergebnis wie folgt aus:

	This Virtual Network has no gateway. I will need to create one.
	Your VNET is in the address space 172.16.0.0/16, with the following Subnets:
	default: 172.16.0.0/24
	Please choose a GatewaySubnet address space: 172.16.1.0/26

In diesem Beispiel habe ich ein virtuelles Netzwerkgateway mit den folgenden Einstellungen erstellt:

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
	Creating App association to VNET

Sie können diese Einstellungen nach Bedarf ändern. Ansonsten drücken Sie die EINGABETASTE, und das Skript erstellt Ihr Gateway und fügt Ihre App an das virtuelle Netzwerk an. Die Gatewayerstellung dauert aber dennoch eine Stunde. Das sollten Sie berücksichtigen. Wenn alles erledigt ist, zeigt das Skript **Finished** an.

### Trennen Ihrer App von einem Resource Manager-VNET ###

Durch das Trennen Ihrer App von Ihrem virtuellen Netzwerk werden das Gateway und die Punkt-zu-Standort-Verbindung nicht deaktiviert. Sie könnten sie schließlich noch für andere Zwecke verwenden. Die Trennung bezieht sich zudem nur auf die angegebene App. Wählen Sie zum Ausführen dieser Aktion **3) Remove a Virtual Network from an App** aus. Dann sollte etwa Folgendes angezeigt werden:

	Currently connected to VNET v2pshell

	Confirm
	Are you sure you want to delete the following resource:
	/subscriptions/edcc99a4-b7f9-4b5e-a9a1-3034c51db496/resourceGroups/hcdemo-rg/providers/Microsoft.Web/sites/v2vnetpowers
	hell/virtualNetworkConnections/v2pshell
	[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

Auch wenn das Skript „delete“ anzeigt, wird nicht das virtuelle Netzwerk gelöscht. Es wird nur die Integration entfernt. Nachdem Sie bestätigt haben, dass Sie diesen Schritt durchführen möchten, wird der Befehl ziemlich schnell verarbeitet und zeigt abschließend **True** an.

<!--Links-->
[createvpngateway]: http://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/
[azureportal]: http://portal.azure.com

<!---HONumber=AcomDC_0831_2016-->