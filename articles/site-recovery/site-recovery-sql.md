---
title: Replizieren von Apps mit SQL Server und Azure Site Recovery | Microsoft-Dokumentation
description: "In diesem Artikel wird beschrieben, wie Sie SQL Server mit Azure Site Recovery bzw. SQL Server-Funktionen für die Notfallwiederherstellung replizieren."
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: 9126f5e8-e9ed-4c31-b6b4-bf969c12c184
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2017
ms.author: pratshar
translationtype: Human Translation
ms.sourcegitcommit: 9ea73dd91c9637692bbc3d6d2aa97fbed7ae500d
ms.openlocfilehash: 79c110031a47f1bdb78f4acfcadd7bff1e909807
ms.lasthandoff: 02/23/2017


---
# <a name="protect-sql-server-using-sql-server-disaster-recovery-and-azure-site-recovery"></a>Schützen von SQL Server mit der Notfallwiederherstellung von SQL Server und Azure Site Recovery

In diesem Artikel erfahren Sie, wie Sie das SQL Server-Back-End einer Anwendung mit einer Kombination aus SQL Server-Technologien für Geschäftskontinuität und Notfallwiederherstellung (Business Continuity and Disaster Recovery, BCDR) und [Azure Site Recovery](site-recovery-overview.md) schützen.

Informieren Sie sich zunächst unbedingt über die SQL Server-Funktionen zur Notfallwiederherstellung, einschließlich Failoverclustering, AlwaysOn-Verfügbarkeitsgruppen, Datenbankspiegelung und Protokollversand.


## <a name="sql-server-deployments"></a>SQL Server-Bereitstellungen

Viele Workloads nutzen SQL Server als Grundlage, und die Plattform kann zum Implementieren von Datendiensten in Apps wie SharePoint, Dynamics und SAP integriert werden.  SQL Server kann auf verschiedene Arten bereitgestellt werden:

* **Eigenständiger SQL-Server:**SQL Server und alle Datenbanken werden auf einem einzelnen Computer gehostet (physisch oder virtuell). Bei einer Virtualisierung wird Hostclustering verwendet, um die lokale hohe Verfügbarkeit sicherzustellen. Es wird keine hohe Verfügbarkeit für die Gastebene implementiert.
* **SQL Server-Failoverclustering-Instanzen (AlwaysOn FCI):** Zwei oder mehr Knoten mit SQL Server-Instanzen und freigegebenen Datenträgern werden in einem Windows-Failovercluster konfiguriert. Wenn ein Knoten heruntergefahren ist, kann der Cluster ein Failover von SQL Server auf eine andere Instanz ausführen. Dieses Setup wird in der Regel zum Implementieren von hoher Verfügbarkeit an einem primären Standort verwendet. Diese Bereitstellung stellt keinen Schutz vor Fehlern oder einem Ausfall auf der Ebene des freigegebenen Speichers dar. Ein freigegebener Datenträger kann per iSCSI, Fibre Channel oder freigegebener VHDX implementiert werden.
* **SQL AlwaysOn-Verfügbarkeitsgruppen:** Zwei oder mehr Knoten werden in einem Cluster ohne Freigabe mit SQL Server-Datenbanken eingerichtet, die in einer Verfügbarkeitsgruppe mit synchroner Replikation und automatischem Failover konfiguriert sind.

 In diesem Artikel werden die folgenden nativen SQL-Notfallwiederherstellungstechnologien zum Wiederherstellen von Datenbanken an einem Remotestandort verwendet:

* SQL AlwaysOn-Verfügbarkeitsgruppen zur Notfallwiederherstellung für SQL Server 2012 oder 2014 Enterprise-Editionen
* SQL-Datenbankspiegelung im Hochsicherheitsmodus für SQL Server Standard Edition (beliebige Version) oder SQL Server 2008 R2

## <a name="site-recovery-support"></a>Site Recovery-Unterstützung

### <a name="supported-scenarios"></a>Unterstützte Szenarien
Site Recovery kann zum Schützen von SQL Server verwendet werden. Dies ist in der Tabelle unten zusammengefasst.

**Szenario** | **Am sekundären Standort** | **In Azure**
--- | --- | ---
**Hyper-V** | Ja | Ja
**VMware** | Ja | Ja
**Physischer Server** | Ja | Ja

### <a name="supported-sql-server-versions"></a>Unterstützte SQL Server-Versionen
Diese SQL Server-Versionen werden für die unterstützten Szenarien unterstützt:

* SQL Server 2014 Enterprise und Standard
* SQL Server 2012 Enterprise und Standard
* SQL Server 2008 R2 Enterprise und Standard

### <a name="supported-sql-server-integration"></a>Unterstützte SQL Server-Integration

Site Recovery kann zur Bereitstellung einer Notfallwiederherstellungslösung mit nativen BCDR-Technologien von SQL Server kombiniert werden. Dies ist in der folgenden Tabelle zusammengefasst:

**Feature** | **Details** | **SQL Server** |
--- | --- | ---
**AlwaysOn-Verfügbarkeitsgruppe** | Mehrere eigenständige Instanzen von SQL Server werden jeweils in einem Failovercluster mit mehreren Knoten ausgeführt.<br/><br/>Datenbanken können in Failovergruppen zusammengefasst werden, die in SQL Server-Instanzen kopiert (gespiegelt) werden können, sodass kein freigegebener Speicher erforderlich ist.<br/><br/>Ermöglicht die Notfallwiederherstellung zwischen einem primären und einem oder mehreren sekundären Standorten. Zwei Knoten können in einem Shared-Nothing-Cluster mit SQL Server-Datenbanken in einer Verfügbarkeitsgruppe mit synchroner Replikation und automatischem Failover eingerichtet werden. | SQL Server 2014 und 2012 Enterprise Edition
**Failoverclustering (AlwaysOn-FCI)** | SQL Server nutzt das Windows-Failoverclustering für hohe Verfügbarkeit der lokalen SQL Server-Workloads.<br/><br/>Knoten, auf denen Instanzen von SQL Server mit freigegebenen Datenträgern ausgeführt werden, werden in einem Failovercluster konfiguriert. Fällt eine Instanz aus, wird für den Cluster ein Failover zu einer anderen Instanz durchgeführt.<br/><br/>Der Cluster schützt nicht vor Fehlern oder Ausfällen im freigegebenen Speicher. Der freigegebene Datenträger kann mit iSCSI, Fibre Channel oder freigegebenen VHDX-Dateien implementiert werden. | SQL Server Enterprise Editions<br/><br/>SQL Server Standard Edition (auf zwei Knoten beschränkt)
**Datenbankspiegelung (Modus für hohe Sicherheit)** | Schützt eine einzelne Datenbank mittels einer sekundären Kopie. Replikationsmodi mit hoher Sicherheit (synchron) oder mit hoher Leistung (asynchron) verfügbar. Kein Failovercluster erforderlich. | SQL Server 2008 R2<br/><br/>SQL Server Enterprise (alle Editionen)
**Eigenständige SQL Server-Instanz** | SQL Server und die Datenbank werden auf einem einzelnen Server (physisch oder virtuell) gehostet. Bei einem virtuellen Server wird Hostclustering verwendet, um eine hohe Verfügbarkeit zu gewährleisten. Keine hohe Verfügbarkeit auf Gastebene. | Enterprise oder Standard

## <a name="deployment-recommendations"></a>Bereitstellungsempfehlungen

Diese Tabelle enthält unsere Empfehlungen für die Integration von SQL Server-BCDR-Technologien in Site Recovery:

| **Version** | **Edition** | **Bereitstellung** | **Lokal zu lokal** | **Lokal zu Azure** |
| --- | --- | --- | --- | --- |
| SQL Server 2014 oder 2012 |Enterprise |Failoverclusterinstanz |AlwaysOn-Verfügbarkeitsgruppen |AlwaysOn-Verfügbarkeitsgruppen |
|| Enterprise |AlwaysOn-Verfügbarkeitsgruppen für hohe Verfügbarkeit |AlwaysOn-Verfügbarkeitsgruppen |AlwaysOn-Verfügbarkeitsgruppen | |
|| Standard |Failoverclusterinstanz (FCI) |Site Recovery-Replikation mit lokaler Spiegelung |Site Recovery-Replikation mit lokaler Spiegelung | |
|| Enterprise oder Standard |Eigenständig |Site Recovery-Replikation |Site Recovery-Replikation | |
| SQL Server 2008 R2 |Enterprise oder Standard |Failoverclusterinstanz (FCI) |Site Recovery-Replikation mit lokaler Spiegelung |Site Recovery-Replikation mit lokaler Spiegelung |
|| Enterprise oder Standard |Eigenständig |Site Recovery-Replikation |Site Recovery-Replikation | |
| SQL Server (beliebige Version) |Enterprise oder Standard |Failoverclusterinstanz – DTC-Anwendung |Site Recovery-Replikation |Nicht unterstützt |

## <a name="deployment-prerequisites"></a>Voraussetzungen für die Bereitstellung

* Eine lokale SQL Server-Bereitstellung mit einer unterstützten SQL Server-Version. In der Regel benötigen Sie auch Active Directory für SQL Server.
* Die erforderlichen Komponenten für das gewünschte Bereitstellungsszenario. Erfahren Sie mehr über die Supportanforderungen für die [Replikation in Azure](site-recovery-support-matrix-to-azure.md) und die [lokale Replikation](site-recovery-support-matrix.md) sowie die [Bereitstellungsvoraussetzungen](site-recovery-prereq.md).
* Führen Sie zum Einrichten der Wiederherstellung in Azure auf Ihren virtuellen SQL Server-Computern das [Tool zur Ermittlung der Bereitschaft virtueller Azure-Computer](http://www.microsoft.com/download/details.aspx?id=40898) aus, um sicherzustellen, dass diese mit Azure und Site Recovery kompatibel sind.

## <a name="set-up-active-directory"></a>Einrichten von Active Directory

Richten Sie für den ordnungsgemäßen Betrieb von SQL Server Active Directory am sekundären Wiederherstellungsstandort ein.

* **Kleine Unternehmen**: Wenn nur eine geringe Anzahl von Anwendungen und ein einzelner Domänencontroller für den lokalen Standort vorhanden sind und Sie ein Failover für den gesamten Standort durchführen möchten, empfiehlt es sich, den Domänencontroller mithilfe der Site Recovery-Replikation im sekundären Datencenter oder in Azure zu replizieren.
* **Mittlere bis große Unternehmen**: Wenn Sie über eine hohe Anzahl von Anwendung verfügen, eine Active Directory-Gesamtstruktur verwenden und ein anwendungs- oder workloadspezifisches Failover durchführen möchten, empfiehlt sich die Einrichtung eines zusätzlichen Domänencontrollers im sekundären Datencenter oder in Azure. Wenn Sie für die Wiederherstellung an einem Remotestandort AlwaysOn-Verfügbarkeitsgruppen verwenden, empfiehlt es sich, einen weiteren zusätzlichen Domänencontroller für den sekundären Standort oder für Azure einzurichten, der für die wiederhergestellte SQL Server-Instanz verwendet werden kann.

Bei den Anweisungen in diesem Artikel wird davon ausgegangen, dass am sekundären Standort ein Domänencontroller verfügbar ist. [weiteren Informationen](site-recovery-active-directory.md) zum Schützen von Active Directory mit Site Recovery.

## <a name="integrate-with-sql-server-alwayson-for-replication-to-azure-classic-portal-with-a-vmmconfiguration-server"></a>Integration in SQL Server AlwaysOn für die Replikation in Azure (klassisches Portal mit einem VMM-/Konfigurationsserver)


Site Recovery bietet native Unterstützung von SQL AlwaysOn. Wenn Sie eine SQL-Verfügbarkeitsgruppe mit einem virtuellen Azure-Computer erstellt haben, die als sekundärer Standort eingerichtet ist, können Sie anschließend mithilfe von Site Recovery das Failover der Verfügbarkeitsgruppen verwalten.

> [!NOTE]
> Diese Funktion befindet sich derzeit in der Vorschauphase. Es steht zur Verfügung, wenn am primären Standort in System Center VMM-Clouds verwaltete Hyper-V-Hostserver vorhanden sind oder wenn Sie [VMware-Replikation](site-recovery-vmware-to-azure.md) eingerichtet haben. Die Funktion ist derzeit im neuen Azure-Portal nicht verfügbar. Führen Sie die Schritte in [diesem Abschnitt](site-recovery-sql.md#integrate-with-sql-server-alwayson-for-replication-to-azure-azure-portalclassic-portal-with-no-vmmconfiguration-server) durch, wenn Sie das neue Azure-Portal verwenden.
>
>


#### <a name="before-you-start"></a>Vorbereitung

Für die Integration von SQL Always On in Site Recovery benötigen Sie Folgendes:

* Lokale SQL Server-Instanz (eigenständiger Server oder Failovercluster).
* Mindestens einen virtuellen Azure-Computer, auf dem SQL Server installiert ist
* Eine SQL Server-Verfügbarkeitsgruppe, die zwischen der lokalen SQL Server-Instanz und der unter Azure ausgeführten SQL Server-Instanz eingerichtet ist
* PowerShell-Remoting muss für die lokale SQL Server-Instanz aktiviert sein. Der VMM-Server bzw. der Konfigurationsserver muss PowerShell-Remoteaufrufe an den SQL Server-Computer richten können.
* Dem lokalen SQL Server-Computer muss ein Benutzerkonto hinzugefügt werden. Fügen Sie es in einer SQL Server-Gruppe mit mindestens folgenden Berechtigungen hinzu:
  * ALTER AVAILABILITY GROUP: Berechtigungen [hier](https://msdn.microsoft.com/library/hh231018.aspx) und [hier](https://msdn.microsoft.com/library/ff878601.aspx#Anchor_3)
  * ALTER DATABASE: Berechtigungen [hier](https://msdn.microsoft.com/library/ff877956.aspx#Security)
* Bei der Ausführung von VMM muss ein ausführendes Konto auf dem VMM-Server erstellt werden.
- Wenn Sie VMware ausführen, muss mithilfe von „CSPSConfigtool.exe“ ein Konto auf dem Konfigurationsserver erstellt werden.
* Das SQL PS-Modul muss für die lokal ausgeführten SQL Server-Instanzen und auf virtuellen Azure-Computern installiert werden.
* Der VM-Agent muss auf virtuellen Azure-Computern installiert werden.
* „NTAUTHORITY\System“ benötigt folgende Berechtigungen für eine auf virtuellen Azure-Computern ausgeführte SQL Server-Instanz.
  * ALTER AVAILABILITY GROUP: Berechtigungen [hier](https://msdn.microsoft.com/library/hh231018.aspx) und [hier](https://msdn.microsoft.com/library/ff878601.aspx#Anchor_3)
  * ALTER DATABASE: Berechtigungen [hier](https://msdn.microsoft.com/library/ff877956.aspx#Security)

### <a name="add-a-sql-server"></a>Hinzufügen einer SQL Server-Instanz
1. Klicken Sie auf **SQL hinzufügen** , um eine neue SQL Server-Instanz hinzuzufügen.

    ![SQL hinzufügen](./media/site-recovery-sql/add-sql.png)
2. Geben Sie unter **SQL Server-Einstellungen konfigurieren** > **Name** einen Anzeigenamen für die SQL Server-Instanz ein.
3. Geben Sie unter **SQL Server (FQDN)** den FQDN der SQL Server-Quellinstanz an, die Sie hinzufügen möchten. Falls SQL Server in einem Failovercluster installiert ist, geben Sie den FQDN des Clusters und nicht den der Clusterknoten an.  
4. Wählen Sie unter **SQL Server-Instanz** die Standardinstanz aus, oder geben Sie den benutzerdefinierten Namen an.
5. Wählen Sie unter **Verwaltungsserver** einen VMM-Server oder Konfigurationsserver aus, der im Tresor registriert ist. Site Recovery nutzt diesen Server für die Kommunikation mit der SQL Server-Instanz.
6. Geben Sie unter **Ausführendes Konto** den Namen des ausführenden VMM-Kontos oder das Konfigurationsserverkonto an. Dieses Konto wird verwendet, um auf die SQL Server-Instanz zuzugreifen, und muss über die Berechtigungen „Lesen“ und „Failover“ für Verfügbarkeitsgruppen auf dem SQL Server-Computer verfügen.

    ![Dialogfeld "SQL hinzufügen"](./media/site-recovery-sql/add-sql-dialog.png)

Nachdem Sie die SQL Server-Instanz hinzugefügt haben, wird sie auf der Registerkarte **Server mit SQL Server** angezeigt.

![SQL Server-Liste](./media/site-recovery-sql/sql-server-list.png)

### <a name="add-a-sql-availability-group"></a>Hinzufügen einer SQL-Verfügbarkeitsgruppe

1. Klicken Sie für die hinzugefügte SQL Server-Instanz auf **SQL-Verfügbarkeitsgruppe hinzufügen**.

    ![SQL-Verfügbarkeitsgruppe hinzufügen](./media/site-recovery-sql/add-sqlag.png)
2. Die Verfügbarkeitsgruppe kann auf einem oder mehreren virtuellen Azure-Computern repliziert werden. Wenn Sie die Gruppe hinzufügen, müssen Sie den Namen und das Abonnement des virtuellen Azure-Computers angeben, auf dem durch Site Recovery ein Failover der Gruppe ausgeführt werden soll.

    ![Dialogfeld "SQL-Verfügbarkeitsgruppe hinzufügen"](./media/site-recovery-sql/add-sqlag-dialog.png)
3. Bei einem Failover wird in diesem Beispiel die Verfügbarkeitsgruppe DB1-AG zur primären Verfügbarkeitsgruppe auf dem virtuellen Computer SQLAGVM2 im Abonnement DevTesting2.

> [!NOTE]
> Nur die Verfügbarkeitsgruppen, die in der hinzugefügten SQL Server-Instanz primär sind, können zu Site Recovery hinzugefügt werden. Wenn Sie in der SQL Server-Instanz eine Verfügbarkeitsgruppe als primär eingestuft haben oder der SQL Server-Instanz nach deren Hinzufügen mehrere Verfügbarkeitsgruppen hinzugefügt haben, aktualisieren Sie die Gruppe in der SQL Server-Instanz.
>
>

### <a name="create-a-recovery-plan"></a>Erstellen eines Wiederherstellungsplans

Erstellen Sie einen Wiederherstellungsplan mit virtuellen Computern und Verfügbarkeitsgruppen. Wählen Sie den VMM-Server oder Konfigurationsserver als Quelle und Azure als Ziel.

![Wiederherstellungsplan erstellen](./media/site-recovery-sql/create-rp1.png)

![Wiederherstellungsplan erstellen](./media/site-recovery-sql/create-rp2.png)

In diesem Beispiel besteht die SharePoint-App aus drei virtuellen Computern, die eine SQL-Verfügbarkeitsgruppe als Back-End verwenden. In diesem Wiederherstellungsplan können Sie sowohl die Verfügbarkeitsgruppe als auch die virtuellen App-Computer ausgewählt werden. Sie können den Wiederherstellungsplan weiter anpassen, indem Sie virtuelle Computer in verschiedene Failovergruppen verschieben, um die Reihenfolge des Failovers festzulegen. Für die Verfügbarkeitsgruppe erfolgt das Failover immer zuerst, da sie als App-Back-End verwendet wird.

![Wiederherstellungsplan anpassen](./media/site-recovery-sql/customize-rp.png)

### <a name="failover"></a>Failover

Nachdem die Verfügbarkeitsgruppe einem Wiederherstellungsplan hinzugefügt wurde, sind andere Failoveroptionen verfügbar.

**Failover** | **Details**
--- | ---
**Geplantes Failover** | Ein geplantes Failover impliziert ein Failover ohne Datenverlust. Hierfür wird der Modus der SQL-Verfügbarkeitsgruppe auf „Synchron“ festgelegt. Dann wird ein Failover ausgelöst, um die Verfügbarkeitsgruppe auf dem bereitgestellten virtuellen Computer als „Primär“ festzulegen, während die Gruppe Site Recovery hinzugefügt wird. Nachdem das Failover abgeschlossen ist, wird der Verfügbarkeitsmodus auf den gleichen Wert wie vor dem Auslösen des geplanten Failovers festgelegt.
**Nicht geplantes Failover** | Ein ungeplantes Failover kann zum Verlust von Daten führen. Bei der Auslösung eines ungeplanten Failovers wird der Verfügbarkeitsmodus der Gruppe nicht geändert. Er wird auf dem bereitgestellten virtuellen Computer als „Primär“ festgelegt, während die Verfügbarkeitsgruppe Site Recovery hinzugefügt wird. Nachdem ein ungeplantes Failover abgeschlossen ist und der lokale Server mit SQL Server wieder zur Verfügung steht, muss für die Verfügbarkeitsgruppe die umgekehrte Replikation ausgelöst werden. Diese Aktion ist unter **Server mit SQL Server** > **SQL-Verfügbarkeitsgruppe** und nicht im Wiederherstellungsplan verfügbar.
**Test-Failover** |Ein Testfailover für SQL-Verfügbarkeitsgruppen wird nicht unterstützt. Wenn Sie ein Testfailover auslösen, wird das Failover für die Verfügbarkeitsgruppe übersprungen.

Probieren Sie diese Failoveroptionen aus.

**Option** | **Details**
--- | ---
**Option 1:** | 1. Führen Sie ein Test-Failover für die Anwendungs- und die Front-End-Ebene durch.<br/><br/>2. Aktualisieren Sie die Anwendungsebene für den Zugriff auf die Replikatkopie im schreibgeschützten Modus, und führen Sie einen schreibgeschützten Test der Anwendung durch.
**Option 2** | 1. Erstellen Sie eine Kopie der virtuellen SQL Server-Replikatcomputerinstanz (mit VMM-Klon bei Site-to-Site oder mit Azure Backup), und machen Sie sie in einem Testnetzwerk verfügbar.<br/><br/> 2. Führen Sie das Test-Failover mit dem Wiederherstellungsplan durch.

### <a name="fail-back"></a>Failback

Wenn Sie die Verfügbarkeitsgruppe für die lokale SQL Server-Instanz wieder als „Primär“ festlegen möchten, können Sie ein geplantes Failover für den Wiederherstellungsplan auslösen und die Richtung von Microsoft Azure zum lokalen Server wählen.

> [!NOTE]
> Nach einem ungeplanten Failover muss die umgekehrte Replikation für die Verfügbarkeitsgruppe ausgelöst werden, um die Replikation fortzusetzen.  Bis dahin bleibt die Replikation unterbrochen.
>
>

## <a name="integrate-with-sql-server-alwayson-for-replication-to-azure-azure-portalclassic-portal-with-no-vmmconfiguration-server"></a>Integration in SQL Server AlwaysOn für die Replikation in Azure (Azure-Portal/klassisches Portal ohne VMM-/Konfigurationsserver)

Diese Anweisungen sind relevant, wenn Sie eine Integration in SQL Server-Verfügbarkeitsgruppen im neuen Azure-Portal oder im klassischen Portal ausführen, wenn Sie keinen VMM-Server oder Konfigurationsserver verwenden. In diesem Szenario können Azure Automation-Runbooks verwendet werden, um ein skriptgesteuertes Failover von SQL-Verfügbarkeitsgruppen zu konfigurieren.

Das müssen Sie tun:

1. Erstellen Sie eine lokale Datei für ein Skript, das ein Failover einer Verfügbarkeitsgruppe ausführt. Dieses Beispielskript gibt einen Pfad zur Verfügbarkeitsgruppe im Azure-Replikat an und führt ein Failover der Gruppe auf diese Replikatinstanz durch. Dieses Skript wird auf dem virtuellen SQL Server-Replikatcomputer ausgeführt, indem es zusammen mit der Erweiterung des benutzerdefinierten Skripts übergeben wird.

        ``Param(
           [string]$SQLAvailabilityGroupPath
        )
        import-module sqlps
        Switch-SqlAvailabilityGroup -Path $SQLAvailabilityGroupPath -AllowDataLoss -force``

2. Laden Sie das Skript an ein Blob im Azure-Speicherkonto hoch. Zu verwendendes Beispiel:

        ``$context = New-AzureStorageContext -StorageAccountName "Account" -StorageAccountKey "Key"
        Set-AzureStorageBlobContent -Blob "AGFailover.ps1" -Container "script-container" -File "ScriptLocalFilePath" -context $context``

3. Erstellen Sie ein Azure Automation-Runbook, um die Skripts auf dem virtuellen SQL Server-Replikatcomputer in Azure aufzurufen. Verwenden Sie dazu dieses Beispielskript. [hier](site-recovery-runbook-automation.md) .

4. Fügen Sie beim Erstellen eines Wiederherstellungsplans für die Anwendung einen geskripteten Startschritt vor der ersten Gruppe ein, der das Automation-Runbook für das Failover der Verfügbarkeitsgruppe aufruft.


5. SQL AlwaysOn bietet keine native Unterstützung für ein Testfailover. Daher empfehlen wir Folgendes:
    1. Richten Sie [Azure Backup](../backup/backup-azure-vms.md) auf dem virtuellen Computer ein, auf dem das Replikat der Verfügbarkeitsgruppe in Azure gehostet wird.
    1. Vor dem Auslösen des Testfailovers des Wiederherstellungsplans stellen Sie den virtuellen Computer aus der im vorherigen Schritt erstellten Sicherung wieder her.
    1. Führen Sie ein Testfailover des Wiederherstellungsplans durch.


> [!NOTE]
> Für das folgende Skript wird davon ausgegangen, dass die SQL-Verfügbarkeitsgruppe auf einem klassischen virtuellen Azure-Computer wird und der Name des wiederhergestellten virtuellen Computers in Schritt&2; „SQLAzureVM-Test“ lautet. Ändern Sie das Skript basierend auf dem Namen, den Sie für den wiederhergestellten virtuellen Computer verwenden.
>
>


     ``workflow SQLAvailabilityGroupFailover
     {

         param (
             [Object]$RecoveryPlanContext
         )

         $Cred = Get-AutomationPSCredential -name 'AzureCredential'

         #Connect to Azure
         $AzureAccount = Add-AzureAccount -Credential $Cred
         $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
         Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

         InLineScript
         {
          #Update the script with name of your storage account, key and blob name
          $context = New-AzureStorageContext -StorageAccountName "Account" -StorageAccountKey "Key";
          $sasuri = New-AzureStorageBlobSASToken -Container "script-container"- Blob "AGFailover.ps1" -Permission r -FullUri -Context $context;

          Write-output "failovertype " + $Using:RecoveryPlanContext.FailoverType;

          if ($Using:RecoveryPlanContext.FailoverType -eq "Test")
                {
                    Write-output "tfo"

                    Write-Output "Creating ILB"
                    Add-AzureInternalLoadBalancer -InternalLoadBalancerName SQLAGILB -SubnetName Subnet-1 -ServiceName SQLAzureVM-Test -StaticVNetIPAddress #IP
                    Write-Output "ILB Created"

                    #Update the script with name of the virtual machine recovered using Azure Backup
                    Write-Output "Adding SQL AG Endpoint"
                    Get-AzureVM -ServiceName "SQLAzureVM-Test" -Name "SQLAzureVM-Test"| Add-AzureEndpoint -Name sqlag -LBSetName sqlagset -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName SQLAGILB | Update-AzureVM

                    Write-Output "Added Endpoint"

                    $VM = Get-AzureVM -Name "SQLAzureVM-Test" -ServiceName "SQLAzureVM-Test"

                    Write-Output "UnInstalling custom script extension"
                    Set-AzureVMCustomScriptExtension -Uninstall -ReferenceName CustomScriptExtension -VM $VM |Update-AzureVM
                    Write-Output "Installing custom script extension"
                    Set-AzureVMExtension -ExtensionName CustomScriptExtension -VM $vm -Publisher Microsoft.Compute -Version 1.*| Update-AzureVM   

                    Write-output "Starting AG Failover"
                    Set-AzureVMCustomScriptExtension -VM $VM -FileUri $sasuri -Run "AGFailover.ps1" -Argument "-Path sqlserver:\sql\sqlazureVM\default\availabilitygroups\testag"  | Update-AzureVM
                    Write-output "Completed AG Failover"
                }
          else
                {
                Write-output "pfo/ufo";
                #Get the SQL Azure Replica VM.
                #Update the script to use the name of your VM and Cloud Service
                $VM = Get-AzureVM -Name "SQLAzureVM" -ServiceName "SQLAzureReplica";     

                Write-Output "Installing custom script extension"
                #Install the Custom Script Extension on teh SQL Replica VM
                Set-AzureVMExtension -ExtensionName CustomScriptExtension -VM $VM -Publisher Microsoft.Compute -Version 1.*| Update-AzureVM;

                Write-output "Starting AG Failover";
                #Execute the SQL Failover script
                #Pass the SQL AG path as the argument.

                $AGArgs="-SQLAvailabilityGroupPath sqlserver:\sql\sqlazureVM\default\availabilitygroups\testag";

                Set-AzureVMCustomScriptExtension -VM $VM -FileUri $sasuri -Run "AGFailover.ps1" -Argument $AGArgs | Update-AzureVM;

                Write-output "Completed AG Failover";

                }

         }
     }``

## <a name="integrate-with-sql-server-alwayson-for-replication-to-a-secondary-on-premises-site"></a>Integration in SQL Server AlwaysOn für die Replikation an einem sekundären lokalen Standort

Wenn die SQL Server-Instanz Verfügbarkeitsgruppen für hohe Verfügbarkeit (oder eine FCI) verwendet, empfiehlt es sich, auch am Wiederherstellungsstandort Verfügbarkeitsgruppen zu verwenden. Diese Empfehlung gilt für Apps, die keine verteilten Transaktionen verwenden.

1. [Konfigurieren Sie Datenbanken](https://msdn.microsoft.com/library/hh213078.aspx) in Verfügbarkeitsgruppen.
2. Erstellen Sie ein virtuelles Netzwerk am sekundären Standort.
3. Richten Sie eine Site-to-Site-VPN-Verbindung zwischen dem virtuellen Netzwerk und dem primären Standort ein.
4. Erstellen Sie einen virtuellen Computer am Wiederherstellungsstandort, und installieren Sie SQL Server.
5. Erweitern Sie die vorhandenen AlwaysOn-Verfügbarkeitsgruppen auf die neue SQL Server-VM. Konfigurieren Sie diese SQL Server-Instanz als asynchrone Replikatkopie.
6. Erstellen Sie einen Verfügbarkeitsgruppenlistener, oder aktualisieren Sie den vorhandenen Listener, sodass er den asynchronen virtuellen Replikatcomputer enthält.
7. Stellen Sie sicher, dass die Anwendungsfarm mit dem Listener konfiguriert ist. Sollte die Farm mit dem Namen des Datenbankservers konfiguriert sein, aktualisieren Sie sie so, dass sie den Listener verwendet. Andernfalls muss dieser Schritt nach dem Failover durchgeführt werden.

Für Anwendungen, die verteilte Transaktionen verwenden, empfiehlt sich die Bereitstellung von [Site Recovery mit SAN-Replikation](site-recovery-vmm-san.md) oder [Site-to-Site-Replikation für VMware-Server oder physische Server](site-recovery-vmware-to-vmware.md).

### <a name="recovery-plan-considerations"></a>Überlegungen zum Wiederherstellungsplan
1. Fügen Sie dieses Beispielskript der VMM-Bibliothek am primären und sekundären Standort hinzu.

        ``Param(
        [string]$SQLAvailabilityGroupPath
        )
        import-module sqlps
        Switch-SqlAvailabilityGroup -Path $SQLAvailabilityGroupPath -AllowDataLoss -force``
2. Fügen Sie beim Erstellen eines Wiederherstellungsplans für die Anwendung einen geskripteten Startschritt vor der ersten Gruppe ein, der das Skript für das Failover von Verfügbarkeitsgruppen aufruft.

## <a name="protect-a-standalone-sql-server"></a>Schützen einer eigenständigen SQL Server-Instanz

In diesem Szenario empfiehlt es sich, den SQL Server-Computer mithilfe der Site Recovery-Replikation zu schützen. Welche Schritte dafür konkret erforderlich sind, hängt davon ab, ob die SQL Server-Instanz ein virtueller Computer oder ein physischer Server ist und ob Sie Azure oder einen sekundären lokalen Standort als Replikationsziel verwenden möchten. Weitere Informationen zu [Site Recovery-Szenarien](site-recovery-overview.md).

## <a name="protect-a-sql-server-cluster-standard-editionwindows-server-2008-r2"></a>Schützen eines SQL Server-Clusters (Standard Edition/Windows Server 2008 R2)

Bei einem Cluster mit SQL Server Standard Edition oder SQL Server 2008 R2 empfiehlt es sich, SQL Server mit der Site Recovery-Replikation zu schützen.

### <a name="on-premises-to-on-premises"></a>Lokal zu lokal

* Für eine App, die verteilte Transaktionen verwendet, empfiehlt sich die Bereitstellung von [Site Recovery mit SAN-Replikation](site-recovery-vmm-san.md) für eine Hyper-V-Umgebung bzw. von [VMware-Server/physischem Server in VMware](site-recovery-vmware-to-vmware.md) für eine VMware-Umgebung.
* Nutzen Sie für Nicht-DTC-Anwendungen den obigen Ansatz, um den Cluster als eigenständigen Server wiederherzustellen, indem Sie eine lokale DB-Hochsicherheitsspiegelung verwenden.

### <a name="on-premises-to-azure"></a>Lokal zu Azure

Site Recovery unterstützt bei der Replikation in Azure keine Gastcluster. Für die Standard-Edition von SQL Server steht zudem keine kostengünstige Notfallwiederherstellungslösung zur Verfügung. In diesem Szenario empfiehlt es sich, den lokalen SQL Server-Cluster mit einer eigenständigen SQL Server-Instanz zu schützen und die Wiederherstellung in Azure vorzunehmen.

1. Konfigurieren Sie eine zusätzliche eigenständige SQL Server-Instanz am lokalen Standort.
2. Konfigurieren Sie diese Instanz so, dass sie als Spiegelung für die zu schützenden Datenbanken fungiert. Konfigurieren Sie die Spiegelung im Modus für hohe Sicherheit.
3. Konfigurieren Sie Site Recovery am lokalen Standort für [Hyper-V](site-recovery-hyper-v-site-to-azure.md) oder [virtuelle VMware-Computer/physische Server](site-recovery-vmware-to-azure-classic.md).
4. Verwenden Sie die Site Recovery-Replikation, um die neue SQL Server-Instanz zu Azure zu replizieren. Da es sich hierbei um eine Spiegelkopie mit hoher Sicherheit handelt, wird sie mit dem primären Cluster synchronisiert. Bei der Replikation in Azure wird allerdings die Site Recovery-Replikation verwendet.


![Standardcluster](./media/site-recovery-sql/BCDRStandaloneClusterLocal.png)

### <a name="failback-considerations"></a>Überlegungen zum Failback

Bei SQL Server Standard-Clustern ist für das Failback nach einem nicht geplanten Failover eine SQL-Serversicherung und -wiederherstellung der Spiegelinstanz im ursprünglichen Cluster mit Wiederherstellung der Spiegelung erforderlich.

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über die [Site Recovery-Architektur](site-recovery-components.md).

