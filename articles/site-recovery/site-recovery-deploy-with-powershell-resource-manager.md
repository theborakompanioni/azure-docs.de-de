<properties
	pageTitle="Schützen von Servern in Azure mithilfe von Azure PowerShell mit Azure Resource Manager | Microsoft Azure"
	description="Automatisieren Sie den Schutz von Servern in Azure mit Azure Site Recovery mithilfe von PowerShell und Azure Resource Manager."
	services="site-recovery"
	documentationCenter=""
	authors="bsiva"
	manager="abhiag"
	editor=""/>

<tags
	ms.service="site-recovery"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="na"
	ms.workload="backup-recovery"
	ms.date="06/13/2016"
	ms.author="bsiva"/>

# Replizieren zwischen lokalen virtuellen Hyper-V-Computern und Azure mithilfe von PowerShell und Azure Resource Manager

> [AZURE.SELECTOR]
- [Azure-Portal](site-recovery-hyper-v-site-to-azure.md)
- [PowerShell ARM](site-recovery-deploy-with-powershell-resource-manager.md)
- [Klassisches Portal](site-recovery-hyper-v-site-to-azure-classic.md)



## Übersicht

Azure Site Recovery unterstützt Ihre Strategie für Geschäftskontinuität und Notfallwiederherstellung, indem Replikation, Failover und Wiederherstellung virtueller Computer in einer Vielzahl von Bereitstellungsszenarien aufeinander abgestimmt werden. Eine vollständige Liste mit Bereitstellungsszenarien finden Sie unter [Übersicht über Azure Site Recovery](site-recovery-overview.md).

Azure PowerShell ist ein Modul, das Cmdlets zum Verwalten von Azure durch Windows PowerShell enthält. Sie können zwei Arten von Modulen verwenden: das Azure-Profil oder den Azure Resource Manager.

Mit PowerShell-Cmdlets für Site Recovery, die mit Azure PowerShell für den Azure Resource Manager verfügbar sind, können Sie Ihre Server in Azure schützen und wiederherstellen.

Dieser Artikel beschreibt, wie Sie Site Recovery mithilfe von Windows PowerShell und Azure Resource Manager bereitstellen, um den Serverschutz in Azure zu konfigurieren und zu koordinieren. Das Beispiel in diesem Artikel zeigt Ihnen, wie Sie virtuelle Computer auf einem Hyper-V-Host mithilfe von Azure PowerShell in Verbindung mit Azure Resource Manager in Azure schützen, für sie ein Failover ausführen und sie wiederherstellen.

> [AZURE.NOTE] Die PowerShell-Cmdlets für Site Recovery bieten Ihnen gegenwärtig die folgenden Konfigurationsmöglichkeiten: von einem Virtual Machine Manager-Standort zu einem anderen, von einem Virtual Machine Manager-Standort zu Azure und von einem Hyper-V-Standort zu Azure.

Sie müssen kein PowerShell-Experte sein, um diesen Artikel verwenden zu können. Sie müssen jedoch mit den grundlegenden Konzepten wie Modulen, Cmdlets und Sitzungen vertraut sein. Weitere Informationen zu Windows PowerShell finden Sie unter [Erste Schritte mit Windows PowerShell](http://technet.microsoft.com/library/hh857337.aspx).

Weitere Informationen finden Sie unter [Verwenden von Azure PowerShell mit Azure Resource Manager](../powershell-azure-resource-manager.md).

> [AZURE.NOTE] Microsoft-Partner, die Teil des Cloudlösungsanbieter-Programms (Cloud Solution Provider, CSP) sind, können den Schutz der Server ihrer Kunden im entsprechenden CSP-Abonnement (Mandantenabonnement) ihrer Kunden konfigurieren und verwalten.

## Vorbereitung

Stellen Sie sicher, dass diese Voraussetzungen erfüllt werden:

- Ein [Microsoft Azure](https://azure.microsoft.com/)-Konto. Für den Einstieg steht eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/) zur Verfügung. Darüber hinaus können Sie sich über die [Preisgestaltung für Azure Site Recovery-Manager](https://azure.microsoft.com/pricing/details/site-recovery/) informieren.
- Azure PowerShell 1.0. Informationen zu dieser Version, und wie Sie diese installieren, finden Sie unter [Azure PowerShell 1.0](https://azure.microsoft.com/).
- Die Module [AzureRM.SiteRecovery](https://www.powershellgallery.com/packages/AzureRM.SiteRecovery/) und [AzureRM.RecoveryServices](https://www.powershellgallery.com/packages/AzureRM.RecoveryServices/). Sie erhalten die neuesten Versionen dieser Module im [PowerShell-Katalog](https://www.powershellgallery.com/).

In diesem Artikel wird das Verwenden von Azure PowerShell mit Azure Resource Manager zum Konfigurieren und Verwalten des Schutzes Ihrer Server veranschaulicht. Das Beispiel in diesem Artikel zeigt, wie Sie einen auf einem Hyper-V-Host ausgeführten virtuellen Computer in Azure schützen. Die folgenden Anforderungen sind für dieses Beispiel spezifisch. Eine umfassendere Aufstellung von Voraussetzungen für die verschiedenen Site Recovery-Szenarien finden Sie in der Dokumentation zum jeweiligen Szenario.

- Ein Hyper-V-Host unter Windows Server 2012 R2 mit einem oder mehreren virtuellen Computern.
- Direkt oder über einen Proxy mit dem Internet verbundene Hyper-V-Server.
- Die zu schützenden virtuellen Computer müssen die [Voraussetzungen für virtuelle Computer](site-recovery-best-practices.md#virtual-machines) erfüllen.


## Schritt 1: Anmelden bei Ihrem Azure-Konto


1. Öffnen Sie eine PowerShell-Konsole, und führen Sie den folgenden Befehl aus, um sich bei Ihrem Azure-Konto anzumelden. Das Cmdlet ruft eine Webseite auf, die Sie zur Eingabe Ihrer Kontoanmeldeinformationen auffordert.

    	Login-AzureRmAccount

	Alternativ können Sie Ihre Kontoanmeldeinformationen auch als Parameter des `Login-AzureRmAccount`-Cmdlets mithilfe des `-Credential`-Parameters einbeziehen.

	Wenn Sie als CSP-Partner für einen Mandanten tätig sind, geben Sie den Kunden mit dessen Mandanten-ID oder primärem Mandantendomänennamen als Mandanten an.

		Login-AzureRmAccount -Tenant "fabrikam.com"

2. Da ein Konto mehrere Abonnements enthalten kann, müssen Sie das Abonnement, das Sie verwenden möchten, dem Konto zuordnen.

    	Select-AzureRmSubscription -SubscriptionName $SubscriptionName

3.  Stellen Sie mit den folgenden Befehlen sicher, dass Ihr Abonnement zur Verwendung der Azure-Anbieter für Recovery Services und Site Recovery registriert ist:

	- `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices`
	-  `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`

	Wenn **RegistrationState** in der Ausgabe dieser beiden Befehle auf **Registered** festgelegt ist, können Sie mit Schritt 2 fortfahren. Andernfalls müssen Sie den fehlenden Anbieter in Ihrem Abonnement registrieren.

	Führen Sie den folgenden Befehl aus, um den Azure-Anbieter für Site Recovery zu registrieren:

    	Register-AzureRmResourceProvider -ProviderNamespace Microsoft.SiteRecovery

	Wenn Sie die Recovery Services-Cmdlets zum ersten Mal in Ihrem Abonnement verwenden, müssen Sie den Azure-Anbieter für Recovery Services registrieren. Bevor Sie dies tun können, aktivieren Sie zunächst den Zugriff auf den Recovery Services-Anbieter in Ihrem Abonnement mit folgendem Befehl:

		Register-AzureRmProviderFeature -FeatureName betaAccess -ProviderNamespace Microsoft.RecoveryServices

	>[AZURE.TIP] Es kann nach erfolgreicher Ausführung des obigen Befehls bis zu einer Stunde dauern, bis der Zugriff auf den Recovery Services-Anbieter in Ihrem Abonnement aktiviert ist. In der Zwischenzeit kann bei Versuchen, den Recovery Services-Anbieter in Ihrem Abonnement mit dem Befehl `Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices` zu registrieren, ein Fehler auftreten. Warten Sie in diesem Fall eine Stunde, und wiederholen Sie den Versuch.

	Sobald Sie den Zugriff auf den Recovery Services-Anbieter in Ihrem Abonnement aktiviert haben, registrieren Sie den Anbieter in Ihrem Abonnement mit folgendem Befehl:

		Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices

	Überprüfen Sie mit den Befehlen `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` und `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`, ob die Anbieter erfolgreich registriert wurden.



## Schritt 2: Einrichten des Recovery Services-Tresors

1. Erstellen Sie eine Azure Resource Manager-Ressourcengruppe, in der Sie den Tresor erstellen, oder verwenden Sie eine vorhandene Ressourcengruppe. Sie können mithilfe des folgenden Befehls eine neue Ressourcengruppe erstellen:

		New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Geo  

	Dabei enthält die Variable „$ResourceGroupName“ den Namen der zu erstellenden Ressourcengruppe und die Variable „$Geo“ die Azure-Region, in der die Ressourcengruppe erstellt wird (z.B. „Brasilien, Süden“).

	Sie können mit dem `Get-AzureRmResourceGroup`-Cmdlet eine Liste der Ressourcengruppen in Ihrem Abonnement abrufen.

2. Erstellen Sie einen neuen Azure Recovery Services-Tresor wie folgt:

		$vault = New-AzureRmRecoveryServicesVault -Name <string> -ResourceGroupName <string> -Location <string>

	Sie können eine Liste der vorhandenen Tresore mithilfe des Cmdlets `Get-AzureRmRecoveryServicesVault` abrufen.

> [AZURE.NOTE] Wenn Sie Vorgänge in Site Recovery-Tresoren ausführen möchten, die mit dem klassischen Portal oder dem PowerShell-Modul für die Azure-Dienstverwaltung erstellt wurden, können Sie eine Liste mit solchen Tresoren mit dem Cmdlet `Get-AzureRmSiteRecoveryVault` abrufen. Sie sollten für alle neuen Vorgänge einen neuen Recovery Services-Tresor erstellen. Die zuvor erstellten Site Recovery-Tresore werden weiterhin unterstützt, bieten aber nicht die neuesten Features.

## Schritt 3: Festlegen des Kontexts des Recovery Services-Tresors

1.  Legen Sie den Tresorkontext durch Ausführen des folgenden Befehls fest:

		Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## Schritt 4: Erstellen eines Hyper-V-Standorts und Generieren eines neuen Tresorregistrierungsschlüssels für den Standort

1. Erstellen Sie einen neuen Hyper-V-Standort wie folgt:

		$sitename = "MySite"                #Specify site friendly name
		New-AzureRmSiteRecoverySite -Name $sitename

	Dieses Cmdlet startet einen Site Recovery-Auftrag, um den Standort zu erstellen, und gibt ein Site Recovery-Auftragsobjekt zurück. Warten Sie, bis der Auftrag abgeschlossen ist, und überprüfen Sie, ob der Auftrag erfolgreich abgeschlossen wurde.

	Sie können das Auftragsobjekt abrufen und damit den aktuellen Status des Auftrags mit dem Cmdlet „Get-AzureRmSiteRecoveryJob“ überprüfen.
2. Generieren Sie wie folgt einen Registrierungsschlüssel für den Standort, und laden Sie ihn herunter:

		$SiteIdentifier = Get-AzureRmSiteRecoverySite -Name $sitename | Select -ExpandProperty SiteIdentifier
		Get-AzureRmRecoveryServicesVaultSettingsFile -Vault $vault -SiteIdentifier $SiteIdentifier -SiteFriendlyName $sitename -Path $Path

	Kopieren Sie den heruntergeladenen Schlüssel in den Hyper-V-Host. Sie benötigen den Schlüssel, um den Hyper-V-Host am Standort zu registrieren.

## Schritt 5: Installieren des Azure Site Recovery-Anbieters und Azure Recovery Services-Agents auf dem Hyper-V-Host

1. Laden Sie bei [Microsoft](https://aka.ms/downloaddra) das Installationsprogramm für die neueste Version des Anbieters herunter.

2. Führen Sie das Installationsprogramm auf Ihrem Hyper-V-Host aus, und fahren Sie nach Beendigung des Installationsvorgangs mit der Registrierung fort.

3. Geben Sie den heruntergeladenen Standortregistrierungsschlüssel an, wenn Sie dazu aufgefordert werden, und schließen Sie die Registrierung des Hyper-V-Hosts am Standort ab.

4. Überprüfen Sie mit dem folgenden Befehl, ob der Hyper-V-Host am Standort registriert wurde:

		$server =  Get-AzureRmSiteRecoveryServer -FriendlyName $server-friendlyname

## Schritt 6: Erstellen einer Replikationsrichtlinie und deren Zuordnung zum Schutzcontainer

1. Erstellen Sie eine Replikationsrichtlinie wie folgt:

		$ReplicationFrequencyInSeconds = "300";    	#options are 30,300,900
		$PolicyName = “replicapolicy”
		$Recoverypoints = 6            		#specify the number of recovery points
		$storageaccountID = Get-AzureRmStorageAccount -Name "mystorea" -ResourceGroupName "MyRG" | Select -ExpandProperty Id

		$PolicyResult = New-AzureRmSiteRecoveryPolicy -Name $PolicyName -ReplicationProvider “HyperVReplicaAzure” -ReplicationFrequencyInSeconds $ReplicationFrequencyInSeconds  -RecoveryPoints $Recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId $storageaccountID

	Überprüfen Sie den zurückgegebenen Auftrag, um sicherzustellen, dass die Replikationsrichtlinie erfolgreich erstellt wurde.

	>[AZURE.IMPORTANT] Das angegebene Speicherkonto muss sich in der gleichen Azure-Region wie Ihr Recovery Services-Tresor befinden, und die Georeplikation muss hierfür aktiviert sein.
	>
	> - Wenn es sich bei dem angegebenen Speicherkonto für die Wiederherstellung um den Typ „Azure Storage (klassisch)“ handelt, werden die geschützten Computer bei einem Failover auf „Azure IaaS (klassisch)“ wiederhergestellt.
	> - Wenn es sich bei dem angegebenen Speicherkonto für die Wiederherstellung um den Typ „Azure Storage (Azure Resource Manager)“ handelt, werden die geschützten Computer bei einem Failover auf „Azure IaaS (Azure Resource Manager)“ wiederhergestellt.

2. Rufen Sie den entsprechenden Schutzcontainer des Standorts wie folgt ab:

		$protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer
3.	Starten Sie die Zuordnung des Schutzcontainers zu der Replikationsrichtlinie wie folgt:

		$Policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $PolicyName
		$associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy $Policy -PrimaryProtectionContainer $protectionContainer

	Warten Sie, bis der Zuordnungsauftrag abgeschlossen ist, und stellen Sie sicher, dass er erfolgreich abgeschlossen wurde.

##Schritt 7: Aktivieren des Schutzes für virtuelle Computer

1. Rufen Sie die Schutzentität, die der zu schützenden VM entspricht, folgendermaßen ab:

		$VMFriendlyName = "Fabrikam-app"                    #Name of the VM
		$protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

2. Starten Sie den Schutz des virtuellen Computers folgendermaßen:

		$Ostype = "Windows"                                 # "Windows" or "Linux"
		$DRjob = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity -Policy $Policy -Protection Enable -RecoveryAzureStorageAccountId $storageaccountID  -OS $OStype -OSDiskName $protectionEntity.Disks[0].Name

	>[AZURE.IMPORTANT] Das angegebene Speicherkonto muss sich in der gleichen Azure-Region wie Ihr Recovery Services-Tresor befinden, und die Georeplikation muss hierfür aktiviert sein.
	>
	> - Wenn es sich bei dem angegebenen Speicherkonto für die Wiederherstellung um den Typ „Azure Storage (klassisch)“ handelt, werden die geschützten Computer bei einem Failover auf „Azure IaaS (klassisch)“ wiederhergestellt.
	> - Wenn es sich bei dem angegebenen Speicherkonto für die Wiederherstellung um den Typ „Azure Storage (Azure Resource Manager)“ handelt, werden die geschützten Computer bei einem Failover auf „Azure IaaS (Azure Resource Manager)“ wiederhergestellt.

	> Wenn dem geschützten virtuellen Computer mehrere Datenträger angefügt sind, geben Sie den Betriebssystemdatenträger mit dem *OSDiskName*-Parameter an.

3. Warten Sie, bis die virtuellen Computer nach der ersten Replikation einen geschützten Zustand erreicht haben. Abhängig von Faktoren wie der Menge der zu replizierenden Daten und der verfügbare Upstreambandbreite zu Azure kann dies länger dauern. Die Aufträge „State“ und „StateDescription“ werden wie folgt aktualisiert, sobald die VM einen geschützten Zustand erreicht.

		PS C:\> $DRjob = Get-AzureRmSiteRecoveryJob -Job $DRjob

		PS C:\> $DRjob | Select-Object -ExpandProperty State
		Succeeded

		PS C:\> $DRjob | Select-Object -ExpandProperty StateDescription
		Completed

4. Aktualisieren Sie die Wiederherstellungseigenschaften, z.B. die Größe der VM-Rolle, und das Azure-Netzwerk, dem die Netzwerkschnittstellenkarten der virtuellen Computer beim Failover angefügt werden sollen.

		PS C:\> $nw1 = Get-AzureRmVirtualNetwork -Name "FailoverNw" -ResourceGroupName "MyRG"

		PS C:\> $VMFriendlyName = "Fabrikam-App"

		PS C:\> $VM = Get-AzureRmSiteRecoveryVM -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

		PS C:\> $UpdateJob = Set-AzureRmSiteRecoveryVM -VirtualMachine $VM -PrimaryNic $VM.NicDetailsList[0].NicId -RecoveryNetworkId $nw1.Id -RecoveryNicSubnetName $nw1.Subnets[0].Name

		PS C:\> $UpdateJob = Get-AzureRmSiteRecoveryJob -Job $UpdateJob

		PS C:\> $UpdateJob


		Name             : b8a647e0-2cb9-40d1-84c4-d0169919e2c5
		ID               : /Subscriptions/a731825f-4bf2-4f81-a611-c331b272206e/resourceGroups/MyRG/providers/Microsoft.RecoveryServices/vault
		                   s/MyVault/replicationJobs/b8a647e0-2cb9-40d1-84c4-d0169919e2c5
		Type             : Microsoft.RecoveryServices/vaults/replicationJobs
		JobType          : UpdateVmProperties
		DisplayName      : Update the virtual machine
		ClientRequestId  : 805a22a3-be86-441c-9da8-f32685673112-2015-12-10 17:55:51Z-P
		State            : Succeeded
		StateDescription : Completed
		StartTime        : 10-12-2015 17:55:53 +00:00
		EndTime          : 10-12-2015 17:55:54 +00:00
		TargetObjectId   : 289682c6-c5e6-42dc-a1d2-5f9621f78ae6
		TargetObjectType : ProtectionEntity
		TargetObjectName : Fabrikam-App
		AllowedActions   : {Restart}
		Tasks            : {UpdateVmPropertiesTask}
		Errors           : {}



## Schritt 8: Ausführen eines Testfailovers

1. Führen Sie einen Testfailoverauftrag folgendermaßen aus:

		$nw = Get-AzureRmVirtualNetwork -Name "TestFailoverNw" -ResourceGroupName "MyRG" #Specify Azure vnet name and resource group

		$protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMFriendlyName -ProtectionContainer $protectionContainer

		$TFjob = Start-AzureRmSiteRecoveryTestFailoverJob -ProtectionEntity $protectionEntity -Direction PrimaryToRecovery -AzureVMNetworkId $nw.Id

2. Stellen Sie sicher, dass die Test-VM in Azure erstellt wird. (Der Testfailoverauftrag wird nach dem Erstellen der Test-VM in Azure angehalten. Der Auftrag schließt mit der Bereinigung der erstellten Artefakte beim Fortsetzen des Auftrags ab, wie im nächsten Schritt dargestellt.)

3. Schließen Sie das Testfailover wie folgt ab:

    	$TFjob = Resume-AzureRmSiteRecoveryJob -Job $TFjob


##Nächste Schritte

[Erfahren Sie mehr](https://msdn.microsoft.com/library/azure/mt637930.aspx) über Azure Site Recovery mit PowerShell-Cmdlets für Azure Resource Manager.

<!---HONumber=AcomDC_0615_2016-->