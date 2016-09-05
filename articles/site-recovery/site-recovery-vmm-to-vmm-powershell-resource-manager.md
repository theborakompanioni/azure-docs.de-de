<properties
	pageTitle="Replizieren von virtuellen Hyper-V-Computern in VMM-Clouds an einen sekundären VMM-Standort von PowerShell (Resource Manager) | Microsoft Azure"
	description="Es wird beschrieben, wie Sie Azure Site Recovery bereitstellen, um Replikation, Failover und Wiederherstellung von Hyper-V-VMs in VMM-Clouds an einem sekundären VMM-Standort mithilfe von PowerShell (Resource Manager) zu orchestrieren."
	services="site-recovery"
	documentationCenter=""
	authors="sujaytalasila"
	manager="rochakm"
	editor="raynew"/>

<tags
	ms.service="site-recovery"
	ms.workload="backup-recovery"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/19/2016"
	ms.author="sutalasi"/>

# Replizieren von virtuellen Hyper-V-Computern in VMM-Clouds an einen sekundären VMM-Standort von PowerShell (Resource Manager)

> [AZURE.SELECTOR]
- [Azure-Portal](site-recovery-vmm-to-vmm.md)
- [Klassisches Portal](site-recovery-vmm-to-vmm-classic.md)
- [PowerShell – Resource Manager](site-recovery-vmm-to-vmm-powershell-resource-manager.md)

Willkommen bei Azure Site Recovery! Verwenden Sie die Informationen in diesem Artikel, wenn Sie lokale virtuelle Hyper-V-Computer an einen sekundären Standort replizieren möchten, die in System Center Virtual Machine Manager-Clouds (VMM) verwaltet werden.

In diesem Artikel erfahren Sie, wie Sie PowerShell zur Automatisierung häufiger Aufgaben verwenden, die Sie ausführen müssen, wenn Sie Azure Site Recovery zum Replizieren virtueller Hyper-V-Computer in System Center VMM-Clouds an einem sekundären Standort einrichten.

Der Artikel enthält Voraussetzungen für das Szenario und zeigt Ihnen folgende Vorgänge:

- Einrichten eines Recovery Services-Tresors
- Installieren des Azure Site Recovery-Anbieters auf dem VMM-Quellserver und dem VMM-Zielserver
- Registrieren der VMM-Server im Tresor
- Konfigurieren der Replikationsrichtlinie für die VMM-Cloud; die Replikationseinstellungen in der Richtlinie werden auf alle geschützten virtuellen Computer angewendet
- Aktivieren des Schutzes für diese virtuellen Computer
- Testen des Failovers für virtuelle Computer als Einzelvorgang oder im Rahmen eines Wiederherstellungsplans, um sicherzustellen, dass alle Komponenten erwartungsgemäß funktionieren
- Durchführen eines geplanten oder ungeplanten Failovers für virtuelle Computer als Einzelvorgang oder im Rahmen eines Wiederherstellungsplans, um sicherzustellen, dass alle Komponenten erwartungsgemäß funktionieren

Sollten beim Einrichten dieses Szenarios Probleme auftreten, besuchen Sie das [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

> [AZURE.NOTE] Azure verfügt über zwei verschiedene [Bereitstellungsmodelle](../resource-manager-deployment-model.md) für das Erstellen und Verwenden von Ressourcen: Azure Resource Manager- und klassische Bereitstellung. Azure verfügt auch über zwei Portale: das klassische Azure-Portal mit Unterstützung des klassischen Bereitstellungsmodells und das Azure-Portal mit Unterstützung für beide Bereitstellungsmodelle. Dieser Artikel gilt für das Ressourcen-Manager-Bereitstellungsmodell.



## Lokale Voraussetzungen

Folgendes benötigen Sie am primären und sekundären lokalen Standort, um dieses Szenario bereitzustellen:

**Voraussetzungen** | **Details** 
--- | ---
**VMM** | Wir empfehlen Ihnen, am primären und am sekundären Standort je einen VMM-Server bereitzustellen.<br/><br/> Eine [Replikation zwischen Clouds auf einem einzelnen VMM-Server](site-recovery-single-vmm.md) ist auch möglich. Zu diesem Zweck benötigen Sie mindestens zwei auf dem VMM-Server konfigurierte Clouds.<br/><br/> Auf den VMM-Servern sollte mindestens System Center 2012 SP1 mit den neuesten Updates ausgeführt werden.<br/><br/> Für jeden VMM-Server muss mindestens eine Cloud konfiguriert sein, und für alle Clouds muss das Hyper-V-Kapazitätsprofil festgelegt sein. <br/><br/>Clouds müssen eine oder mehrere VMM-Hostgruppen enthalten.<br/><br/>Weitere Informationen zum Einrichten von VMM-Clouds finden Sie in [Vorbereiten der Azure Site Recovery-Bereitstellung](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric) und [Walkthrough: Creating Private Clouds with System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx) (Exemplarische Vorgehensweise: Erstellen von privaten Clouds mit System Center 2012 SP1 VMM).<br/><br/> VMM-Server sollten über Internetzugriff verfügen. 
**Hyper-V** | Auf den Hyper-V-Servern muss mindestens Windows Server 2012 mit der Hyper-V-Rolle ausgeführt werden, und die neuesten Updates müssen installiert sein.<br/><br/> Ein Hyper-V-Server muss mindestens eine VM enthalten.<br/><br/> Hyper-V-Hostserver sollten in Hostgruppen in der primären sowie in der sekundären VMM-Cloud angeordnet sein.<br/><br/> Wenn Sie Hyper-V unter Windows Server 2012 R2 in einem Cluster ausführen, sollten Sie [Update 2961977](https://support.microsoft.com/kb/2961977) installieren.<br/><br/> Wenn Sie Hyper-V unter Windows Server 2012 in einem Cluster ausführen, beachten Sie, dass der Clusterbroker nicht automatisch erstellt wird, falls Sie einen auf statischen IP-Adressen basierenden Cluster haben. Sie müssen den Clusterbroker manuell konfigurieren. [Weitere Informationen](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx)
**Anbieter** | Während der Site Recovery-Bereitstellung installieren Sie den Azure Site Recovery-Anbieter auf VMM-Servern. Der Anbieter kommuniziert mit Azure Site Recovery über HTTPS 443, um die Replikation zu orchestrieren. Die Datenreplikation erfolgt über das LAN oder eine VPN-Verbindung zwischen dem primären und sekundären Hyper-V-Server.<br/><br/> Der auf dem VMM-Server ausgeführte Anbieter benötigt Zugriff auf folgende URLs: *.hypervrecoverymanager.windowsazure.com; *.accesscontrol.windows.net; *.backup.windowsazure.com; *.blob.core.windows.net; *.store.core.windows.net.<br/><br/> Ermöglichen Sie darüber hinaus die Firewallkommunikation der VMM-Server mit den [IP-Bereichen des Azure-Rechenzentrums](https://www.microsoft.com/download/confirmation.aspx?id=41653), und lassen Sie das HTTPS-Protokoll (443) zu.

### Voraussetzungen für die Netzwerkzuordnung
Die Netzwerkzuordnung zwischen VMM-VM-Netzwerken auf dem primären und sekundären VMM-Server dient folgenden Zwecken:

- Optimales Platzieren von Replikat-VMs auf sekundären Hyper-V-Hosts nach dem Failover.
- Verbinden von Replikat-VMs mit entsprechenden VM-Netzwerken.
- Wenn Sie die Netzwerkzuordnung nicht konfigurieren, werden Replikat-VMs nach einem Failover nicht mit einem Netzwerk verbunden.
- Wenn Sie die Netzwerkzuordnung während der Site Recovery-Bereitstellung einrichten möchten, benötigen Sie Folgendes:

	- Stellen Sie sicher, dass VMs auf dem Hyper-V-Quellhostserver mit einem VMM-VM-Netzwerk verbunden sind. Dieses Netzwerk sollte mit einem logischen Netzwerk verbunden sein, das der Cloud zugeordnet ist.
	- Stellen Sie sicher, dass für die sekundäre Cloud, die Sie für die Wiederherstellung verwenden, ein entsprechendes VM-Netzwerk konfiguriert ist. Dieses VM-Netzwerk sollte mit einem logischen Netzwerk verbunden sein, das der sekundären Cloud zugeordnet ist.


Weitere Informationen zur Konfiguration von VMM-Netzwerken finden Sie in unten stehenden Artikeln.

- [Konfigurieren von logischen Netzwerken in VMM](http://go.microsoft.com/fwlink/p/?LinkId=386307)
- [Konfiguration von VM-Netzwerken und Gateways in VMM](http://go.microsoft.com/fwlink/p/?LinkId=386308)

Weitere Informationen zur Funktionsweise der Netzwerkzuordnung finden Sie [hier](site-recovery-network-mapping.md).

###PowerShell-Voraussetzungen
Stellen Sie sicher, dass Azure PowerShell einsatzbereit ist. Wenn Sie PowerShell bereits verwenden, müssen Sie auf Version 0.8.10 oder höher aktualisieren. Informationen zum Einrichten von PowerShell finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md). Nach dem Einrichten und Konfigurieren von PowerShell können Sie alle verfügbaren Cmdlets für den Dienst [hier](https://msdn.microsoft.com/library/dn850420.aspx) anzeigen.

Tipps für die Verwendung von Cmdlets, z.B. wie Parameterwerte, Eingaben und Ausgaben in der Regel in Azure PowerShell behandelt werden, finden Sie unter [Erste Schritte mit Azure-Cmdlets](https://msdn.microsoft.com/library/azure/jj554332.aspx).

## Schritt 1: Festlegen des Abonnements 

1. Melden Sie sich über Azure PowerShell mithilfe der folgenden Cmdlets beim Azure-Konto an.
 
		$UserName = "<user@live.com>"
		$Password = "<password>"
		$SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
		$Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
		Login-AzureRmAccount #-Credential $Cred 
	

2. Rufen Sie eine Liste Ihrer Abonnements ab. Dadurch werden auch die subscriptionIDs für die einzelnen Abonnements aufgeführt. Notieren Sie sich die subscriptionID des Abonnements, in dem Sie den Recovery Services-Tresor erstellen möchten.

		Get-AzureRmSubscription 

3. Legen Sie das Abonnement fest, in dem der Recovery Services-Tresor erstellt werden soll, indem Sie die Abonnement-ID angeben.

		Set-AzureRmContext –SubscriptionID <subscriptionId>


## Schritt 2: Erstellen eines Recovery Services-Tresors 

1. Erstellen Sie eine Azure Resource Manager-Ressourcengruppe, falls noch keine vorhanden ist.

		New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location

2. Erstellen Sie einen neuen Recovery Services-Tresor, und speichern Sie das erstellte ASR-Tresorobjekt in einer Variablen (die später verwendet wird). Sie können das ASR-Tresorobjekt auch nach der Erstellung abrufen, indem Sie das Cmdlet „Get-AzureRMRecoveryServicesVault“ verwenden:

		$vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location 

## Schritt 3: Festlegen des Kontexts des Recovery Services-Tresors

1.  Wenn bereits ein Tresor erstellt wurde, führen Sie unten stehenden Befehl aus, um den Tresor abzurufen.

		$vault = Get-AzureRmRecoveryServicesVault -Name #vaultname

2.  Legen Sie den Tresorkontext durch Ausführen des nachstehenden Befehls fest.

		Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault



## Schritt 4: Installieren des Azure Site Recovery-Anbieters

1.	Erstellen Sie ein Verzeichnis auf dem VMM-Computer durch Ausführen des folgenden Befehls:
	
		New-Item c:\ASR -type directory
		
2.	Extrahieren Sie die Dateien mithilfe des heruntergeladenen Anbieters durch Ausführen des folgenden Befehls.
	
		pushd C:\ASR\
		.\AzureSiteRecoveryProvider.exe /x:. /q

	
3.	Installieren Sie den Anbieter mithilfe der folgenden Befehls:
	
		.\SetupDr.exe /i
		$installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
		do
		{
		                $isNotInstalled = $true;
		                if(Test-Path $installationRegPath)
		                {
		                                $isNotInstalled = $false;
		                }
		}While($isNotInstalled)

    Warten Sie, bis die Installation abgeschlossen ist.
	
4.	Registrieren Sie den Server beim Tresor mithilfe des folgenden Befehls:
	
		$BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
		pushd $BinPath
		$encryptionFilePath = "C:\temp".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice


## Schritt 5: Erstellen und Zuordnen einer Replikationsrichtlinie

1.	Erstellen Sie eine Hyper-V 2012 R2-Replikationsrichtlinie, indem Sie folgenden Befehl ausführen:

	
		$ReplicationFrequencyInSeconds = "300";    	#options are 30,300,900
		$PolicyName = “replicapolicy”
		$RepProvider = HyperVReplica2012R2
		$Recoverypoints = 24            		#specify the number of hours to retain recovery pints
		$AppConsistentSnapshotFrequency = 4 #specify the frequency (in hours) at which app consistent snapshots are taken
		$AuthMode = "Kerberos"  #options are "Kerberos" or "Certificate"
		$AuthPort = "8083"  #specify the port number that will be used for replication traffic on Hyper-V hosts
		$InitialRepMethod = "Online" #options are "Online" or "Offline"

		$policyresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider $RepProvider -ReplicationFrequencyInSeconds $Replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours $AppConsistentSnapshotFrequency -Authentication $AuthMode -ReplicationPort $AuthPort -ReplicationMethod $InitialRepMethod 

	> [AZURE.NOTE] Die VMM-Cloud kann Hyper-V-Hosts enthalten, auf denen verschiedene Versionen von Windows Server ausgeführt werden (wie in den Voraussetzungen für Hyper-V beschrieben), aber die Replikationsrichtlinie ist betriebssystemspezifisch. Wenn Sie über verschiedene Hosts verfügen, die unter verschiedenen Betriebssystemversionen ausgeführt werden, erstellen Sie getrennte Replikationsrichtlinien für jede Betriebssystemversion. Beispiel: Sie haben fünf Hosts unter Windows Server 2012 und drei Hosts unter Windows Server 2012 R2. In diesem Fall erstellen Sie zwei Replikationsrichtlinien – eine für jede Betriebssystemversion.

2.	Rufen Sie den primären Schutzcontainer (primäre VMM-Cloud) und den Schutzcontainer für die Wiederherstellung (VMM-Cloud für die Wiederherstellung) ab, indem Sie folgende Befehle ausführen:
	
		$PrimaryCloud = "testprimarycloud"
		$primaryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

		$RecoveryCloud = "testrecoverycloud"
		$recoveryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $RecoveryCloud;  
  
3.	Rufen Sie die in Schritt 1 erstellte Richtlinie ab, und verwenden Sie dabei den Anzeigenamen der Richtlinie:

		$policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname

4.	Starten Sie die Zuordnung des Schutzcontainers (VMM-Cloud) zur Replikationsrichtlinie:

		$associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer

5.	Warten Sie, bis der Auftrag der Richtlinienzuordnung abgeschlossen ist. Mithilfe des folgenden PowerShell-Codeausschnitts können Sie prüfen, ob der Auftrag abgeschlossen ist.
   
		$job = Get-AzureRmSiteRecoveryJob -Job $associationJob
   		if($job -eq $null -or $job.StateDescription -ne "Completed")
   		 {
        	$isJobLeftForProcessing = $true;
    	}

 	Nachdem die Verarbeitung des Auftrags abgeschlossen ist, führen Sie den folgenden Befehl aus:

		if($isJobLeftForProcessing)
    	{
    	Start-Sleep -Seconds 60
    	}
        }While($isJobLeftForProcessing)


Um den Abschluss des Vorgangs zu überprüfen, führen Sie die Schritte in [Überwachen der Aktivität](#monitor) aus.

## Schritt 5: Konfigurieren der Netzwerkzuordnung

1. Der erste Befehl ruft Server für den aktuellen Azure Site Recovery-Tresor ab. Der Befehl speichert die Microsoft Azure Site Recovery-Server in der "$Servers"-Arrayvariablen.

		$Servers = Get-AzureRmSiteRecoveryServer

2. Mit den unten stehenden Befehlen rufen Sie das Site Recovery-Netzwerk für den VMM-Quellserver und den VMM-Zielserver ab.

    	$PrimaryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]        

		$RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

	
	> [AZURE.NOTE] Der VMM-Quellserver kann der erste oder zweite Server im Serverarray sein. Überprüfen Sie die Namen der VMM-Server, und rufen Sie die entsprechenden Netzwerke ab.


4. Das letzte Cmdlet erstellt eine Zuordnung zwischen dem primären Netzwerk und dem Wiederherstellungsnetzwerk. Das Cmdlet legt das primäre Netzwerk als erstes Element von $PrimaryNetworks und das Wiederherstellungsnetzwerk als erstes Element von $RecoveryNetworks fest.

		New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]

## Schritt 6: Konfigurieren der Speicherzuordnung

1. Mit dem folgenden Befehl wird die Liste der Speicherklassifizierungen in der $storageclassifications-Variablen abgerufen.

		$storageclassifications = Get-AzureRmSiteRecoveryStorageClassification


2. Mit den folgenden Befehlen werden die Quellklassifizierung in der $SourceClassificaion-Variablen und die Zielklassifizierung in $TargetClassification-Variablen abgerufen.

    	$SourceClassificaion = $storageclassifications[0]

		$TargetClassification = $storageclassifications[1]

	
	> [AZURE.NOTE] Die Quell- und Zielklassifizierungen können ein beliebiges Element im Array sein. Über die Ausgabe des folgenden Befehls können Sie den Index der Quell- und Zielklassifizierungen im $storageclassifications-Array ermitteln.
	
	> Get-AzureRmSiteRecoveryStorageClassification | Select-Object -Property FriendlyName, Id | Format-Table


3. Mit dem folgenden Cmdlet wird eine Zuordnung zwischen der Quellklassifizierung und der Zielklassifizierung erstellt.

		New-AzureRmSiteRecoveryStorageClassificationMapping -PrimaryStorageClassification $SourceClassificaion -RecoveryStorageClassification $TargetClassification

## Schritt 7: Aktivieren des Schutzes für virtuelle Computer

Nach der korrekten Konfiguration der Server, Clouds und Netzwerke können Sie den Schutz für die virtuellen Computer in der Cloud aktivieren.


  1. Führen Sie den folgenden Befehl aus, um den Schutzcontainer abzurufen und den Schutz zu aktivieren:
	
			$PrimaryProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloudName
	
  2. Rufen Sie die Schutzentität (VM) durch Ausführen des folgenden Befehls ab:
	
	 		$protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
		
  3. Aktivieren Sie die Replikation für die VM, indem Sie den folgenden Befehl ausführen:

			$jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable -Policy $policy


## Testen der Bereitstellung

Um Ihre Bereitstellung zu testen, können Sie ein Testfailover für einen einzelnen virtuellen Computer durchführen oder einen Wiederherstellungsplan erstellen, der mehrere virtuelle Computer umfasst, und ein Testfailover für diesen Plan durchführen. Das Test-Failover simuliert Ihre Failover- und Wiederherstellungsmechanismen in einem isolierten Netzwerk.

> [AZURE.NOTE] Sie können im Azure-Portal einen Wiederherstellungsplan für Ihre Anwendung erstellen.

Um den Abschluss des Vorgangs zu überprüfen, führen Sie die Schritte in [Überwachen der Aktivität](#monitor) aus.


### Ausführen eines Testfailovers

1.	Führen Sie die unten stehenden Cmdlets aus, um das VM-Netzwerk abzurufen, mit dem Sie ein Testfailover für Ihre VMs durchführen möchten.

		$Servers = Get-AzureRmSiteRecoveryServer
		$RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

2.	Führen Sie ein Testfailover einer VM durch, indem Sie folgende Befehle ausführen:
	
		$protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMName -ProtectionContainer $PrimaryprotectionContainer

		$jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -VMNetwork $RecoveryNetworks[1] 

2.	Führen Sie ein Testfailover eines Wiederherstellungsplans durch, indem Sie folgende Befehle ausführen:
	
		$recoveryplanname = "test-recovery-plan"

		$recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

		$jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan -VMNetwork $RecoveryNetworks[1] 

### Ausführen eines geplanten Failovers

1. Führen Sie ein geplantes Failover einer VM durch, indem Sie folgende Befehle ausführen:
	
		$protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

		$jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

2. Führen Sie ein geplantes Failover eines Wiederherstellungsplans durch, indem Sie folgende Befehle ausführen:
	
		$recoveryplanname = "test-recovery-plan"

		$recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

		$jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan

### Ausführen eines ungeplanten Failovers

1. Führen Sie ein ungeplantes Failover einer VM durch, indem Sie folgende Befehle ausführen:
		
		$protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

		$jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity 

2\. Führen Sie ein ungeplantes Failover eines Wiederherstellungsplans durch, indem Sie folgende Befehle ausführen:
		
		$recoveryplanname = "test-recovery-plan"

		$recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

		$jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity 
	
## <a name=monitor></a> Überwachen der Aktivität

Verwenden Sie die folgenden Befehle zum Überwachen der Aktivität. Beachten Sie, dass Sie zwischen den Aufträgen auf den Abschluss der Verarbeitung warten müssen.

	Do
	{
        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
        	$isJobLeftForProcessing = $true;
        }

	if($isJobLeftForProcessing)
        {
        	Start-Sleep -Seconds 60
        }
	}While($isJobLeftForProcessing)



## Nächste Schritte

[Erfahren Sie mehr](https://msdn.microsoft.com/library/azure/mt637930.aspx) über Azure Site Recovery mit PowerShell-Cmdlets für Azure Resource Manager.

<!---HONumber=AcomDC_0824_2016-->