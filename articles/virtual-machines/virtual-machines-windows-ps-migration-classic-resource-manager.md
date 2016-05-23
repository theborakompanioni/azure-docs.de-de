<properties
	pageTitle="Migrieren von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager mithilfe von Azure PowerShell"
	description="Dieser Artikel beschreibt die plattformgestützten Migrationsdienstfunktionen der Dienstverwaltung für die Migration zu Azure Resource Manager mithilfe von PowerShell-Skripts."
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

Hier erfahren Sie, wie Sie IaaS-Ressourcen mithilfe von Azure PowerShell-Befehlen aus dem klassischen Bereitstellungsmodell zu Resource Manager migrieren. Diese Schritte folgen einem Leere-Felder-ausfüllen-Ansatz für die Migration Ihrer benutzerdefinierten Umgebung. Sie müssen lediglich die Befehle verwenden und sie durch eigene Werte für die Variablen ersetzen (Zeilen, die mit "$" beginnen).

## Schritt 1: Vorbereiten der Migration

Hier finden Sie einige bewährte Methoden für den Fall, dass Sie eine Migration von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Resource Manager in Erwägung ziehen.

- Eine Liste mit nicht unterstützten Konfigurationen und Features finden Sie [hier](virtual-machines-windows-migration-classic-resource-manager.md). Sollten Sie über virtuelle Computer mit nicht unterstützten Konfigurationen oder Features verfügen, empfiehlt es sich, auf die Ankündigung der Unterstützung der entsprechenden Features/Konfigurationen zu warten. Alternativ können Sie ggf. die betroffenen Features entfernen oder eine andere Konfiguration verwenden, um die Migration zu ermöglichen.
-	Falls Sie über automatisierte Skripts zur Bereitstellung Ihrer Infrastruktur und Anwendungen verfügen, erstellen Sie eine ähnliche Testumgebung mit diesen Skripts für die Migration. Alternativ können Sie auch Beispielumgebungen über das Azure-Portal einrichten.
- Da es sich bei dem Dienst um eine öffentliche Vorschauversion handelt, muss die Testumgebung für die Migration von der Produktionsumgebung isoliert sein. Speicherkonten, VNETs und andere Ressourcen der Test- und Produktionsumgebung dürfen nicht gemischt werden.

## Schritt 2: Installieren der aktuellen Azure PowerShell

Es gibt zwei Hauptoptionen für die Installation: [PowerShell-Katalog](https://www.powershellgallery.com/profiles/azure-sdk/) und [WebPI](http://aka.ms/webpi-azps). WebPI wird monatlich aktualisiert. Der PowerShell-Katalog wird fortlaufend aktualisiert.

Weitere Informationen finden Sie unter [Azure PowerShell 1.0](https://azure.microsoft.com//blog/azps-1-0/) (in englischer Sprache).

## Schritt 3: Festlegen Ihres Abonnements und Registrieren für die öffentliche Vorschauversion der Migration

Starten Sie zunächst eine PowerShell-Eingabeaufforderung. Für Migrationsszenarien müssen Sie Ihre Umgebung sowohl für das klassische Bereitstellungsmodell als auch für Resource Manager einrichten.

Melden Sie sich an Ihrem Konto für das Resource Manager-Modell an.

	Login-AzureRmAccount

Rufen Sie die verfügbaren Abonnements mit dem folgenden Befehl ab.

	Get-AzureRMSubscription | Sort SubscriptionName | Select SubscriptionName

Legen Sie Ihr Azure-Abonnement für die aktuelle Sitzung fest. Ersetzen Sie alles in den Anführungszeichen, einschließlich der Zeichen < and >, durch die korrekten Namen.

	$subscr="<subscription name>"
	Get-AzureRmSubscription –SubscriptionName $subscr | Select-AzureRmSubscription

Registrieren Sie sich mithilfe des folgenden Befehls für die öffentliche Vorschau.

	Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate

Der Abschluss der Registrierung kann bis zu fünf Minuten dauern. Warten Sie daher etwas. Der Genehmigungsstatus kann mithilfe des folgenden Befehls geprüft werden. Vergewissern Sie sich, dass „RegistrationState“ den Wert „Registered“ besitzt, bevor Sie den Vorgang fortsetzen.

	Get-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate

Melden Sie sich jetzt an Ihrem Konto für das klassische Modell an.

	Add-AzureAccount

Rufen Sie die verfügbaren Abonnements mit dem folgenden Befehl ab.

	Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName

Legen Sie Ihr Azure-Abonnement für die aktuelle Sitzung fest. Ersetzen Sie alles in den Anführungszeichen, einschließlich der Zeichen < and >, durch die korrekten Namen.

	$subscr="<subscription name>"
	Get-AzureSubscription –SubscriptionName $subscr | Select-AzureSubscription

## Schritt 4: Befehle zum Migrieren von IaaS-Ressourcen

>[AZURE.NOTE] Alle im Anschluss beschriebenen Vorgänge sind idempotent. Sollte ein Fehler auftreten, der nicht auf ein nicht unterstütztes Feature oder auf eine nicht unterstützte Konfiguration zurückzuführen ist, wiederholen Sie den prepare-, abort- oder commit-Vorgang. Die Plattform versucht dann erneut, die Aktion auszuführen.

### Migrieren virtueller Computer in einem Clouddienst (nicht in einem virtuellen Netzwerk)

Rufen Sie mithilfe des folgenden Befehls die Liste mit den Clouddiensten auf, und wählen Sie anschließend den zu migrierenden Clouddienst aus. Falls sich die virtuellen Computer in dem Clouddienst in einem VNET befinden oder über Web-/Workerrollen verfügen, wird eine Fehlermeldung zurückgegeben.

	Get-AzureService | ft Servicename

Rufen Sie mit folgendem Befehl den Bereitstellungsnamen für den Clouddienst ab.

	$serviceName = "<service name>"
	$deployment = Get-AzureDeployment -ServiceName $serviceName
	$deploymentName = $deployment.DeploymentName

Bereiten Sie die virtuellen Computer des Clouddiensts für die Migration vor. Dabei stehen Ihnen zwei Optionen zur Verfügung.

Wenn Sie die virtuellen Computer in ein von der Plattform erstelltes virtuelles Netzwerk migrieren möchten, verwenden Sie den folgenden Befehl:

	Move-AzureService -Prepare -ServiceName $serviceName -DeploymentName $deploymentName -CreateNewVirtualNetwork

Wenn Sie als Migrationsziel ein vorhandenes virtuelles Netzwerk unter dem Resource Manager-Bereitstellungsmodell verwenden möchten, führen Sie den folgenden Befehl aus:

	$existingVnetRGName = "<Existing VNET's Resource Group Name>"
	$vnetName = "<Virtual Network Name>"
	$subnetName = "<Subnet name>"
	Move-AzureService -Prepare -ServiceName $serviceName -DeploymentName $deploymentName -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName 		-VirtualNetworkName $vnetName -SubnetName $subnetName

Nach der Vorbereitung können Sie den Migrationszustand der virtuellen Computer abrufen und sich vergewissern, dass der Zustand der virtuellen Computer "Prepared" lautet.

	$vmName = "<vm-name>"
	$vm = Get-AzureVM -ServiceName $serviceName -Name $vmName
	$migrationState = $vm.VM.MigrationState

Überprüfen Sie die Konfiguration der vorbereiteten Ressourcen mithilfe von PowerShell oder über das Azure-Portal. Falls Sie noch nicht für die Migration bereit sind und zum alten Zustand zurückkehren möchten, führen Sie den folgenden Befehl aus:

	Move-AzureService -Abort -ServiceName $serviceName -DeploymentName $deploymentName

Wenn die vorbereitete Konfiguration in Ordnung ist, können Sie den Vorgang fortsetzen und mithilfe des folgenden Befehls ein Commit für die Ressourcen ausführen:

	Move-AzureService -Commit -ServiceName docmigtest1 -DeploymentName docmigtest1

### Migrieren virtueller Computer in einem virtuellen Netzwerk

Wählen Sie das virtuelle Netzwerk aus, das Sie migrieren möchten. Falls das virtuelle Netzwerk Web-/Workerrollen oder virtuelle Computer mit nicht unterstützten Konfigurationen enthält, tritt ein Validierungsfehler auf.

Bereiten Sie das virtuelle Netzwerk mithilfe des folgenden Befehls für die Migration vor:

	$vnetName = "VNET-Name"
	Move-AzureVirtualNetwork -Prepare -VirtualNetworkName $vnetName

Überprüfen Sie die Konfiguration der vorbereiteten virtuellen Computer mithilfe von PowerShell oder über das Azure-Portal. Falls Sie noch nicht für die Migration bereit sind und zum alten Zustand zurückkehren möchten, führen Sie den folgenden Befehl aus:

	Move-AzureVirtualNetwork -Abort -VirtualNetworkName $vnetName

Wenn die vorbereitete Konfiguration in Ordnung ist, können Sie den Vorgang fortsetzen und mithilfe des folgenden Befehls ein Commit für die Ressourcen ausführen:

	Move-AzureVirtualNetwork -Commit -VirtualNetworkName $vnetName

## Referenzen

- [Platform supported migration of IaaS resources from Classic to Resource Manager (Plattformgestützte Migration von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Resource Manager)](virtual-machines-windows-migration-classic-resource-manager.md)
- [Ausführliche technische Informationen zur plattformgestützten Migration vom klassischen Bereitstellungsmodell zu Azure Resource Manager](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
- [Clone a classic Virtual Machine to Azure Resource Manager using Community PowerShell Scripts (Klonen eines klassischen virtuellen Computers für Azure Resource Manager mithilfe von PowerShell-Skripts aus der Community)](virtual-machines-windows-migration-scripts.md)

<!---HONumber=AcomDC_0511_2016-->