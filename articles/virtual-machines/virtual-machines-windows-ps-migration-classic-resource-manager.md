<properties
	pageTitle="Migrieren von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager mithilfe von Azure PowerShell | Microsoft Azure"
	description="In diesem Artikel wird die plattformgestützte Migration von Ressourcen vom klassischen Bereitstellungsmodell zu Azure Resource Manager mithilfe von PowerShell-Skripts erläutert."
	services="virtual-machines-windows"
	documentationCenter=""
	authors="mahthi"
	manager="drewm"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/04/2016"
	ms.author="mahthi"/>

# Migrieren von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager mithilfe von Azure PowerShell

Diese Schritte zeigen, wie Sie Azure PowerShell-Befehle zum Migrieren von IaaS-Ressourcen (Infrastructure as a Service) aus dem klassischen Bereitstellungsmodell in das Azure Resource Manager-Bereitstellungsmodell verwenden. Diese Schritte folgen einem Leere-Felder-ausfüllen-Ansatz für die Migration Ihrer benutzerdefinierten Umgebung. Sie müssen lediglich die Befehle verwenden und sie durch eigene Werte für die Variablen ersetzen (Zeilen, die mit „$“ beginnen).

## Schritt 1: Vorbereiten der Migration

Hier finden Sie einige bewährte Methoden, die wir empfehlen, wenn Sie eine Migration von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Resource Manager in Erwägung ziehen:

- Sehen Sie sich die [Liste mit nicht unterstützten Konfigurationen und Features](virtual-machines-windows-migration-classic-resource-manager.md) an. Sollten Sie über virtuelle Computer mit nicht unterstützten Konfigurationen oder Features verfügen, empfiehlt es sich, auf die Ankündigung der Unterstützung der entsprechenden Features/Konfigurationen zu warten. Alternativ können Sie ggf. die betroffenen Features entfernen oder eine andere Konfiguration verwenden, um die Migration zu ermöglichen.
-	Wenn Sie derzeit über automatisierte Skripts zum Bereitstellen Ihrer Infrastruktur und Anwendungen verfügen, versuchen Sie, mithilfe dieser Skripts für die Migration eine ähnliche Testeinrichtung zu erstellen. Alternativ dazu können Sie über das Azure-Portal Beispielumgebungen einrichten.

## Schritt 2: Installieren der neuesten Version von Azure PowerShell

Es gibt zwei Hauptoptionen für die Installation: [PowerShell-Katalog](https://www.powershellgallery.com/profiles/azure-sdk/) und [Webplattform-Installer (WPI)](http://aka.ms/webpi-azps). WebPI wird monatlich aktualisiert. Der PowerShell-Katalog wird fortlaufend aktualisiert.

Weitere Informationen finden Sie unter [Azure PowerShell 1.0](https://azure.microsoft.com//blog/azps-1-0/) (in englischer Sprache).

## Schritt 3: Festlegen Ihres Abonnements und Registrieren für die Migration

Starten Sie zunächst eine PowerShell-Eingabeaufforderung. Für Migrationsszenarien müssen Sie Ihre Umgebung sowohl für das klassische Bereitstellungsmodell als auch für Resource Manager einrichten.

Melden Sie sich an Ihrem Konto für das Resource Manager-Modell an.

	Login-AzureRmAccount

Rufen Sie die verfügbaren Abonnements mit dem folgenden Befehl ab.

	Get-AzureRMSubscription | Sort SubscriptionName | Select SubscriptionName

Legen Sie Ihr Azure-Abonnement für die aktuelle Sitzung fest. Ersetzen Sie alles in den Anführungszeichen, einschließlich der Zeichen < und >, durch die korrekten Namen.

	$subscr="<subscription name>"
	Get-AzureRmSubscription –SubscriptionName $subscr | Select-AzureRmSubscription

>[AZURE.NOTE] Die Registrierung ist ein einmaliger Schritt, der jedoch einmal ausgeführt werden muss, bevor Sie versuchen, die Migration auszuführen. Ohne Registrierung wird die folgende Fehlermeldung angezeigt:

>	*BadRequest : Subscription is not registered for migration.* 

Registrieren Sie sich mithilfe des folgenden Befehls beim Migrationsressourcenanbieter.
	
	Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate

Der Abschluss der Registrierung kann bis zu fünf Minuten dauern. Der Genehmigungsstatus kann mithilfe des folgenden Befehls geprüft werden. Stellen Sie sicher, dass der RegistrationState-Wert `Registered` lautet, bevor Sie fortfahren.

	Get-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate

Melden Sie sich jetzt an Ihrem Konto für das klassische Modell an.

	Add-AzureAccount

Rufen Sie die verfügbaren Abonnements mit dem folgenden Befehl ab.

	Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName

Legen Sie Ihr Azure-Abonnement für die aktuelle Sitzung fest. Ersetzen Sie alles in den Anführungszeichen, einschließlich der Zeichen < und >, durch die korrekten Namen.

	$subscr="<subscription name>"
	Get-AzureSubscription –SubscriptionName $subscr | Select-AzureSubscription

## Schritt 4: Ausführen von Befehlen zum Migrieren Ihrer IaaS-Ressourcen

>[AZURE.NOTE] Alle hier beschriebenen Vorgänge sind idempotent. Sollte ein Problem auftreten, das nicht auf ein nicht unterstütztes Feature oder auf einen Konfigurationsfehler zurückzuführen ist, wiederholen Sie den Vorbereitungs-, Abbruch- oder Commitvorgang. Die Plattform versucht dann erneut, die Aktion auszuführen.

### Migrieren virtueller Computer in einem Clouddienst (nicht in einem virtuellen Netzwerk)

Rufen Sie mithilfe des folgenden Befehls die Liste mit den Clouddiensten auf, und wählen Sie anschließend den zu migrierenden Clouddienst aus. Beachten Sie: Falls sich die virtuellen Computer im Clouddienst in einem virtuellen Netzwerk befinden oder über Web-/Workerrollen verfügen, wird eine Fehlermeldung zurückgegeben.

	Get-AzureService | ft Servicename

Rufen Sie mit folgendem Befehl den Bereitstellungsnamen des Clouddiensts ab.

	$serviceName = "<service name>"
	$deployment = Get-AzureDeployment -ServiceName $serviceName
	$deploymentName = $deployment.DeploymentName

Bereiten Sie die virtuellen Computer des Clouddiensts auf die Migration vor. Dabei stehen Ihnen zwei Optionen zur Auswahl.

Wenn Sie die virtuellen Computer in ein von der Plattform erstelltes virtuelles Netzwerk migrieren möchten, verwenden Sie den folgenden Befehl.

	Move-AzureService -Prepare -ServiceName $serviceName -DeploymentName $deploymentName -CreateNewVirtualNetwork

Wenn Sie als Migrationsziel ein vorhandenes virtuelles Netzwerk im Resource Manager-Bereitstellungsmodell verwenden möchten, führen Sie den folgenden Befehl aus.

	$existingVnetRGName = "<Existing VNET's Resource Group Name>"
	$vnetName = "<Virtual Network Name>"
	$subnetName = "<Subnet name>"
	Move-AzureService -Prepare -ServiceName $serviceName -DeploymentName $deploymentName -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName 		-VirtualNetworkName $vnetName -SubnetName $subnetName

Nach der Vorbereitung können Sie den Migrationszustand der virtuellen Computer abrufen und sich vergewissern, dass der Zustand der virtuellen Computer `Prepared` lautet.

	$vmName = "<vm-name>"
	$vm = Get-AzureVM -ServiceName $serviceName -Name $vmName
	$migrationState = $vm.VM.MigrationState

Überprüfen Sie die Konfiguration der vorbereiteten Ressourcen mithilfe von PowerShell oder des Azure-Portals. Wenn Sie noch nicht für die Migration bereit sind und zum alten Zustand zurückkehren möchten, verwenden Sie den folgenden Befehl.

	Move-AzureService -Abort -ServiceName $serviceName -DeploymentName $deploymentName

Wenn die vorbereitete Konfiguration in Ordnung ist, können Sie den Vorgang fortsetzen und mithilfe des folgenden Befehls ein Commit für die Ressourcen ausführen.

	Move-AzureService -Commit -ServiceName docmigtest1 -DeploymentName docmigtest1

### Migrieren virtueller Computer in einem virtuellen Netzwerk

Wählen Sie das virtuelle Netzwerk aus, das Sie migrieren möchten. Beachten Sie: Falls das virtuelle Netzwerk Web-/Workerrollen oder virtuelle Computer mit nicht unterstützten Konfigurationen enthält, tritt ein Validierungsfehler auf.

Bereiten Sie das virtuelle Netzwerk mithilfe des folgenden Befehls für die Migration vor.

	$vnetName = "VNET-Name"
	Move-AzureVirtualNetwork -Prepare -VirtualNetworkName $vnetName

Überprüfen Sie die Konfiguration der vorbereiteten virtuellen Computer mithilfe von PowerShell oder des Azure-Portals. Wenn Sie noch nicht für die Migration bereit sind und zum alten Zustand zurückkehren möchten, verwenden Sie den folgenden Befehl.

	Move-AzureVirtualNetwork -Abort -VirtualNetworkName $vnetName

Wenn die vorbereitete Konfiguration in Ordnung ist, können Sie den Vorgang fortsetzen und mithilfe des folgenden Befehls ein Commit für die Ressourcen ausführen.

	Move-AzureVirtualNetwork -Commit -VirtualNetworkName $vnetName

### Migrieren eines Speicherkontos

Sobald Sie mit der Migration der virtuellen Computer fertig sind, sollten Sie das Speicherkonto migrieren.

Bereiten Sie das Speicherkonto Netzwerk mithilfe des folgenden Befehls für die Migration vor.

	$storageAccountName = "storagename"
	Move-AzureStorageAccount -Prepare -StorageAccountName $storageAccountName

Überprüfen Sie die Konfiguration des vorbereiteten Speicherkontos entweder mit PowerShell oder im Azure-Portal. Wenn Sie noch nicht für die Migration bereit sind und zum alten Zustand zurückkehren möchten, verwenden Sie den folgenden Befehl.

	Move-AzureStorageAccount -Abort -StorageAccountName $storageAccountName

Wenn die vorbereitete Konfiguration in Ordnung ist, können Sie den Vorgang fortsetzen und mithilfe des folgenden Befehls ein Commit für die Ressourcen ausführen.

	Move-AzureStorageAccount -Commit -StorageAccountName $storageAccountName

## Nächste Schritte

- [Plattformgestützte Migration von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Resource Manager](virtual-machines-windows-migration-classic-resource-manager.md)
- [Ausführliche technische Informationen zur plattformgestützten Migration vom klassischen Bereitstellungsmodell zu Azure Resource Manager](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
- [Klonen eines klassischen virtuellen Computers für Azure Resource Manager mithilfe von PowerShell-Skripts aus der Community](virtual-machines-windows-migration-scripts.md)

<!---HONumber=AcomDC_0706_2016-->