<properties
	pageTitle="Migrieren zu Resource Manager mit PowerShell | Microsoft Azure"
	description="In diesem Artikel wird die plattformgestützte Migration von IaaS-Ressourcen vom klassischen Bereitstellungsmodell zu Azure Resource Manager mithilfe von Azure PowerShell-Befehlen erläutert."
	services="virtual-machines-windows"
	documentationCenter=""
	authors="dlepow"
	manager="timlt"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/16/2016"
	ms.author="danlep"/>

# Migrieren von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager mithilfe von Azure PowerShell

Diese Schritte zeigen, wie Sie Azure PowerShell-Befehle zum Migrieren von IaaS-Ressourcen (Infrastructure as a Service) aus dem klassischen Bereitstellungsmodell in das Azure Resource Manager-Bereitstellungsmodell verwenden. Diese Schritte folgen einem Leere-Felder-ausfüllen-Ansatz für die Migration Ihrer benutzerdefinierten Umgebung. Verwenden Sie die Befehle und setzen Sie für die Variablen (Zeilen, die mit „$“ beginnen) Ihre eigenen Werte ein.

Wenn Sie möchten, können Sie Ressourcen auch mithilfe der [Azure-Befehlszeilenschnittstelle](virtual-machines-linux-cli-migration-classic-resource-manager.md) migrieren.

Hintergrundinformationen zu unterstützten Migrationsszenarien finden Sie unter [Plattformgestützte Migration von IaaS-Ressourcen vom klassischen Bereitstellungsmodell zu Azure Resource Manager](virtual-machines-windows-migration-classic-resource-manager.md). Detaillierte Anleitungen und eine exemplarische Vorgehensweise zur Migration finden Sie unter [Ausführliche technische Informationen zur plattformgestützten Migration vom klassischen Bereitstellungsmodell zu Azure Resource Manager](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md).

## Schritt 1: Planen der Migration

Hier finden Sie einige bewährte Methoden, die wir empfehlen, wenn Sie eine Migration von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Resource Manager in Erwägung ziehen:

- Lesen Sie die Informationen zu [unterstützten und nicht unterstützten Funktionen und Konfigurationen](virtual-machines-windows-migration-classic-resource-manager.md). Sollten Sie über virtuelle Computer mit nicht unterstützten Konfigurationen oder Features verfügen, empfiehlt es sich, auf die Ankündigung der Unterstützung der entsprechenden Features/Konfigurationen zu warten. Entfernen Sie alternativ ggf. die betroffenen Features, oder verwenden Sie eine andere Konfiguration, um die Migration zu ermöglichen.
-	Wenn Sie derzeit über automatisierte Skripts zum Bereitstellen Ihrer Infrastruktur und Anwendungen verfügen, versuchen Sie, mithilfe dieser Skripts für die Migration eine ähnliche Testeinrichtung zu erstellen. Alternativ dazu können Sie über das Azure-Portal Beispielumgebungen einrichten.

>[AZURE.IMPORTANT]Virtuelle Netzwerkgateways (Standort-zu-Standort, Azure ExpressRoute, Anwendungsgateway, Punkt-zu-Standort) werden für die Migration vom klassischen Modell zu Resource Manager derzeit nicht unterstützt. Um ein klassisches virtuelles Netzwerk mit einem Gateway zu migrieren, entfernen Sie zuerst das Gateway, bevor Sie einen Commit-Vorgang zum Verschieben des Netzwerks ausführen. (Sie können einen Vorbereitungsschritt ausführen, ohne das Gateway zu löschen.) Stellen Sie nach Abschluss der Migration die Verbindung zum Gateway in Azure Resource Manager wieder her.

## Schritt 2: Installieren der neuesten Version von Azure PowerShell

Es gibt zwei Hauptoptionen für die Installation von Azure PowerShell: [PowerShell-Katalog](https://www.powershellgallery.com/profiles/azure-sdk/) und [Webplattform-Installer (WebPI)](http://aka.ms/webpi-azps). WebPI empfängt monatliche Updates. Der PowerShell-Katalog wird fortlaufend aktualisiert. Dieser Artikel basiert auf Azure PowerShell Version 2.1.0.

Installationsanweisungen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

## Schritt 3: Festlegen Ihres Abonnements und Registrieren für die Migration

Starten Sie zunächst eine PowerShell-Eingabeaufforderung. Für Migrationsszenarien müssen Sie Ihre Umgebung sowohl für das klassische Bereitstellungsmodell als auch für Resource Manager einrichten.

Melden Sie sich an Ihrem Konto für das Resource Manager-Modell an.

	Login-AzureRmAccount

Rufen Sie die verfügbaren Abonnements mit dem folgenden Befehl ab:

	Get-AzureRMSubscription | Sort SubscriptionName | Select SubscriptionName

Legen Sie Ihr Azure-Abonnement für die aktuelle Sitzung fest. Ersetzen Sie den Inhalt der Anführungszeichen (einschließlich der Zeichen „<“ und „>“) durch den korrekten Namen.

	Select-AzureRmSubscription –SubscriptionName "<subscription name>"

>[AZURE.NOTE] Die Registrierung ist ein einmaliger Schritt, der jedoch einmal ausgeführt werden muss, bevor Sie versuchen, die Migration auszuführen. Ohne Registrierung wird die folgende Fehlermeldung angezeigt:

>	*BadRequest : Subscription is not registered for migration.* 

Registrieren Sie sich mithilfe des folgenden Befehls beim Migrationsressourcenanbieter:
	
	Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate

Der Abschluss der Registrierung kann bis zu fünf Minuten dauern. Der Genehmigungsstatus kann mithilfe des folgenden Befehls geprüft werden:

	Get-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate

Stellen Sie sicher, dass der RegistrationState-Wert `Registered` lautet, bevor Sie fortfahren.

Melden Sie sich jetzt an Ihrem Konto für das klassische Modell an.

	Add-AzureAccount

Rufen Sie die verfügbaren Abonnements mit dem folgenden Befehl ab:

	Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName

Legen Sie Ihr Azure-Abonnement für die aktuelle Sitzung fest. Ersetzen Sie den Inhalt der Anführungszeichen (einschließlich der Zeichen „<“ und „>“) durch den korrekten Namen.

	Select-AzureSubscription –SubscriptionName "<subscription name>"

## Schritt 4: Sicherstellen, dass Sie über genügend Kerne in virtuellen Azure Resource Manager-Computern in der Azure-Region Ihrer aktuellen Bereitstellung oder Ihres VNET verfügen

Mit dem folgenden PowerShell-Befehl können Sie die aktuelle Anzahl von Kernen in Azure Resource Manager überprüfen. Weitere Informationen zu Kernkontingenten finden Sie unter [Grenzwerte und Azure Resource Manager](../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager).

```
Get-AzureRmVMUsage -Location "<Your VNET or Deployment's Azure region"
```

## Schritt 5: Ausführen von Befehlen zum Migrieren Ihrer IaaS-Ressourcen

>[AZURE.NOTE] Alle hier beschriebenen Vorgänge sind idempotent. Sollte ein Problem auftreten, das nicht auf ein nicht unterstütztes Feature oder auf einen Konfigurationsfehler zurückzuführen ist, wiederholen Sie den Vorbereitungs-, Abbruch- oder Commitvorgang. Die Plattform versucht dann erneut, die Aktion auszuführen.

### Migrieren virtueller Computer in einem Clouddienst (nicht in einem virtuellen Netzwerk)

Rufen Sie mithilfe des folgenden Befehls die Liste mit den Clouddiensten auf, und wählen Sie anschließend den zu migrierenden Clouddienst aus. Falls sich die virtuellen Computer im Clouddienst in einem virtuellen Netzwerk befinden oder über Web- oder Workerrollen verfügen, gibt der Befehl eine Fehlermeldung zurück.

	Get-AzureService | ft Servicename

Rufen Sie mit folgendem Befehl den Bereitstellungsnamen des Clouddiensts ab:

	$serviceName = "<service name>"
	$deployment = Get-AzureDeployment -ServiceName $serviceName
	$deploymentName = $deployment.DeploymentName

Bereiten Sie die virtuellen Computer des Clouddiensts auf die Migration vor. Dabei stehen Ihnen zwei Optionen zur Auswahl.

* **Option 1. Migrieren der virtuellen Computer in ein von der Plattform erstelltes virtuelles Netzwerk**

	Überprüfen Sie zuerst, ob Sie den Clouddienst mithilfe des folgenden Befehls migrieren können:

		$validate = Move-AzureService -Validate -ServiceName $serviceName -DeploymentName $deploymentName -CreateNewVirtualNetwork
		$validate.ValidationMessages

	Der vorhergehende Befehl zeigt ggf. Warnungen und Fehler an, die die Migration blockieren. Wenn die Überprüfung erfolgreich ist, können Sie mit dem folgenden Vorbereitungsschritt fortfahren:

		Move-AzureService -Prepare -ServiceName $serviceName -DeploymentName $deploymentName -CreateNewVirtualNetwork

* **Option 2. Migrieren zu einem vorhandenen virtuellen Netzwerk im Resource Manager-Bereitstellungsmodell**

		$existingVnetRGName = "<Existing VNET's Resource Group Name>"
		$vnetName = "<Virtual Network Name>"
		$subnetName = "<Subnet name>"

	Überprüfen Sie zuerst, ob Sie den Clouddienst mithilfe des folgenden Befehls migrieren können:

		$validate = Move-AzureService -Validate -ServiceName $serviceName -DeploymentName $deploymentName -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName -VirtualNetworkName $vnetName -SubnetName $subnetName
		$validate.ValidationMessages

	Der vorhergehende Befehl zeigt ggf. Warnungen und Fehler an, die die Migration blockieren. Wenn die Überprüfung erfolgreich ist, können Sie mit dem folgenden Vorbereitungsschritt fortfahren:

		Move-AzureService -Prepare -ServiceName $serviceName -DeploymentName $deploymentName -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName -VirtualNetworkName $vnetName -SubnetName $subnetName

Nachdem der Vorbereitungsvorgang mit einer der oben beschriebenen Optionen erfolgreich abgeschlossen wurde, fragen Sie den Migrationsstatus der virtuellen Computer ab. Stellen Sie sicher, dass sie sich im Status `Prepared` befinden.

	$vmName = "<vm-name>"
	$vm = Get-AzureVM -ServiceName $serviceName -Name $vmName
	$migrationState = $vm.VM.MigrationState

Überprüfen Sie die Konfiguration der vorbereiteten Ressourcen mithilfe von PowerShell oder des Azure-Portals. Wenn Sie noch nicht für die Migration bereit sind und zum alten Zustand zurückkehren möchten, verwenden Sie den folgenden Befehl:

	Move-AzureService -Abort -ServiceName $serviceName -DeploymentName $deploymentName

Wenn die vorbereitete Konfiguration in Ordnung ist, können Sie den Vorgang fortsetzen und mithilfe des folgenden Befehls ein Commit für die Ressourcen ausführen:

	Move-AzureService -Commit -ServiceName $serviceName -DeploymentName $deploymentName

### Migrieren virtueller Computer in einem virtuellen Netzwerk

Um virtuelle Computer in einem virtuellen Netzwerk zu migrieren, migrieren Sie das Netzwerk. Die virtuellen Computer werden automatisch zusammen mit dem Netzwerk migriert. Wählen Sie das virtuelle Netzwerk aus, das Sie migrieren möchten.

	$vnetName = "<Virtual Network Name>"

>[AZURE.NOTE] Wenn das virtuelle Netzwerk Web- oder Workerrollen oder virtuelle Computer mit nicht unterstützten Konfigurationen enthält, tritt ein Überprüfungsfehler auf.

Überprüfen Sie zuerst mithilfe des folgenden Befehls, ob Sie das virtuelle Netzwerk migrieren können:

	Move-AzureVirtualNetwork -Validate -VirtualNetworkName $vnetName

Der vorhergehende Befehl zeigt ggf. Warnungen und Fehler an, die die Migration blockieren. Wenn die Überprüfung erfolgreich ist, können Sie mit dem folgenden Vorbereitungsschritt fortfahren:
	
	Move-AzureVirtualNetwork -Prepare -VirtualNetworkName $vnetName

Überprüfen Sie die Konfiguration der vorbereiteten virtuellen Computer mithilfe von Azure PowerShell oder über das Azure-Portal. Wenn Sie noch nicht für die Migration bereit sind und zum alten Zustand zurückkehren möchten, verwenden Sie den folgenden Befehl:

	Move-AzureVirtualNetwork -Abort -VirtualNetworkName $vnetName

Wenn die vorbereitete Konfiguration in Ordnung ist, können Sie den Vorgang fortsetzen und mithilfe des folgenden Befehls ein Commit für die Ressourcen ausführen:

	Move-AzureVirtualNetwork -Commit -VirtualNetworkName $vnetName

### Migrieren eines Speicherkontos

Sobald Sie mit der Migration der virtuellen Computer fertig sind, sollten Sie die Speicherkonten migrieren.

Bereiten Sie jedes Speicherkonto mithilfe des folgenden Befehls für die Migration vor:

	$storageAccountName = "<storage account name>"
	Move-AzureStorageAccount -Prepare -StorageAccountName $storageAccountName

Überprüfen Sie die Konfiguration des vorbereiteten Speicherkontos entweder mit Azure PowerShell oder im Azure-Portal. Wenn Sie noch nicht für die Migration bereit sind und zum alten Zustand zurückkehren möchten, verwenden Sie den folgenden Befehl:

	Move-AzureStorageAccount -Abort -StorageAccountName $storageAccountName

Wenn die vorbereitete Konfiguration in Ordnung ist, können Sie den Vorgang fortsetzen und mithilfe des folgenden Befehls ein Commit für die Ressourcen ausführen:

	Move-AzureStorageAccount -Commit -StorageAccountName $storageAccountName

## Nächste Schritte

- Weitere Informationen zur Migration finden Sie unter [Plattformgestützte Migration von IaaS-Ressourcen vom klassischen Bereitstellungsmodell zu Azure Resource Manager](virtual-machines-windows-migration-classic-resource-manager.md).
- Um weitere Netzwerkressourcen mithilfe von Azure PowerShell zu Resource Manager zu migrieren, verwenden Sie ähnliche Schritte mit **Move-AzureNetworkSecurityGroup**, **Move-AzureReservedIP** und **Move-AzureRouteTable**.
- Open Source-Skripts für die Migration von Azure-Ressourcen vom klassischen Bereitstellungsmodell zu Resource Manager finden Sie unter [Communitytools für die Migration zu Azure Resource Manager](virtual-machines-windows-migration-scripts.md).

<!---HONumber=AcomDC_0921_2016-->