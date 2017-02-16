---
title: Entfernen von Servern und Deaktivieren des Schutzes | Microsoft Docs
description: "In diesem Artikel wird erläutert, wie Sie die Registrierung von Servern im Site Recovery-Tresor aufheben und den Schutz für virtuelle Computer und physische Server deaktivieren."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: cfreeman
editor: 
ms.assetid: ef1f31d5-285b-4a0f-89b5-0123cd422d80
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 12/28/2016
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: f57c88cbace41af233f542880c6199b3e278700e
ms.openlocfilehash: c8d893dbac1a4f6cb3f05f857e186bca155e5865


---

# <a name="remove-servers-and-disable-protection"></a>Entfernen von Servern und Deaktivieren des Schutzes

Der Azure Site Recovery-Dienst unterstützt Ihre Strategie für Geschäftskontinuität und Notfallwiederherstellung (Business Continuity and Disaster Recovery, BCDR). Dieser Dienst koordiniert Replikation, Failover und Wiederherstellung von virtuellen Computern und physischen Servern. Computer können in Azure oder in einem sekundären lokalen Rechenzentrum repliziert werden. Eine kurze Übersicht über das Gesamtthema finden Sie unter [Was ist Azure Site Recovery?](site-recovery-overview.md)

In diesem Artikel erfahren Sie, wie Sie die Registrierung von Servern in einem Recovery Services-Tresor im Azure-Portal aufheben und wie Sie den Schutz für mit Site Recovery geschützte Computer deaktivieren.

Kommentare oder Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)veröffentlichen.

## <a name="unregister-a-connected-configuration-server"></a>Aufheben der Registrierung eines verbundenen Konfigurationsservers

Wenn Sie VMware-VMs oder physische Windows-/Linux-Server in Azure replizieren, können Sie die Registrierung des verbundenen Konfigurationsservers bei einem Tresor wie folgt aufheben:

1. Deaktivieren Sie den Computerschutz. Klicken Sie unter **Geschützte Elemente** > **Replizierte Elemente** mit der rechten Maustaste auf den Computer, und klicken Sie dann auf **Löschen**.
2. Trennen Sie ggf. alle Richtlinien. Doppelklicken Sie unter **Site Recovery-Infrastruktur** > **Für VMware und physische Computer** > **Replikationsrichtlinien** auf die zugehörige Richtlinie. Klicken Sie mit der rechten Maustaste auf den Konfigurationsserver, und klicken Sie dann auf **Trennen**.
3. Entfernen Sie alle zusätzlichen lokalen Prozess- oder Masterzielserver. Klicken Sie unter **Site Recovery-Infrastruktur** > **Für VMware und physische Computer** > **Konfigurationsserver** mit der rechten Maustaste auf den Server, und klicken Sie dann auf **Löschen**.
4. Löschen Sie den Konfigurationsserver.
5. Deinstallieren Sie den Mobilitätsdienst manuell, der auf dem Masterzielserver ausgeführt wird. (Dabei handelt es sich entweder um einen separaten Server, oder er wird auf dem Konfigurationsserver ausgeführt.)
6. Deinstallieren Sie ggf. weitere Prozessserver.
7. Deinstallieren Sie den Konfigurationsserver.
8. Deinstallieren Sie auf dem Konfigurationsserver die Instanz von MySQL, die von Site Recovery installiert wurde.
9. Löschen Sie in der Registrierung des Konfigurationsservers den Schlüssel ``HKEY_LOCAL_MACHINE\Software\Microsoft\Azure Site Recovery``.

## <a name="unregister-a-unconnected-configuration-server"></a>Aufheben der Registrierung eines nicht verbundenen Konfigurationsservers

Wenn Sie VMware-VMs oder physische Windows-/Linux-Server in Azure replizieren, können Sie die Registrierung des nicht verbundenen Konfigurationsservers bei einem Tresor wie folgt aufheben:

1. Deaktivieren Sie den Computerschutz. Klicken Sie unter **Geschützte Elemente** > **Replizierte Elemente** mit der rechten Maustaste auf den Computer, und klicken Sie dann auf **Löschen**. Wählen Sie **Verwaltung des Computers beenden** aus.
2. Entfernen Sie alle zusätzlichen lokalen Prozess- oder Masterzielserver. Klicken Sie unter **Site Recovery-Infrastruktur** > **Für VMware und physische Computer** > **Konfigurationsserver** mit der rechten Maustaste auf den Server, und klicken Sie dann auf **Löschen**.
3. Löschen Sie den Konfigurationsserver.
4. Deinstallieren Sie den Mobilitätsdienst manuell, der auf dem Masterzielserver ausgeführt wird. (Dabei handelt es sich entweder um einen separaten Server, oder er wird auf dem Konfigurationsserver ausgeführt.)
5. Deinstallieren Sie ggf. weitere Prozessserver.
6. Deinstallieren Sie den Konfigurationsserver.
7. Deinstallieren Sie auf dem Konfigurationsserver die Instanz von MySQL, die von Site Recovery installiert wurde.
8. Löschen Sie in der Registrierung des Konfigurationsservers den Schlüssel ``HKEY_LOCAL_MACHINE\Software\Microsoft\Azure Site Recovery``.

## <a name="unregister-a-connected-vmm-server"></a>Aufheben der Registrierung eines verbundenen VMM-Servers

Es empfiehlt sich, die Registrierung des VMM-Servers aufzuheben, während dieser mit Azure verbunden ist. Dadurch wird sichergestellt, dass Einstellungen auf den VMM-Servern (und auf anderen VMM-Servern mit gekoppelten Clouds) ordnungsgemäß bereinigt werden. Nicht verbundene Server sollten nur im Falle eines dauerhaften Konnektivitätsproblems entfernt werden. Wenn der VMM-Server nicht verbunden ist, müssen Sie manuell ein Skript zum Bereinigen der Einstellungen ausführen.

1. Beenden Sie die Replikation virtueller Computer in Clouds auf dem VMM-Server, den Sie entfernen möchten.
2. Löschen Sie Netzwerkzuordnungen, die von Clouds auf dem VMM-Server verwendet werden, den Sie löschen möchten. Klicken Sie unter **Site Recovery-Infrastruktur** > **Für System Center VMM** > **Netzwerkzuordnung** mit der rechten Maustaste auf die Netzwerkzuordnung, und klicken Sie dann auf **Löschen**.
3. Heben Sie die Zuordnung zwischen Replikationsrichtlinien und Clouds auf dem VMM-Server auf, den Sie entfernen möchten.  Doppelklicken Sie unter **Site Recovery-Infrastruktur** > **Für System Center VMM** >  **Replikationsrichtlinien** auf die zugehörige Richtlinie. Klicken Sie mit der rechten Maustaste auf die Cloud, und klicken Sie dann auf **Zuordnung aufheben**.
4. Löschen Sie den VMM-Server oder den aktiven VMM-Knoten. Klicken Sie unter **Site Recovery-Infrastruktur** > **Für System Center VMM** > **VMM-Server** mit der rechten Maustaste auf den Server, und klicken Sie dann auf **Löschen**.
5. Deinstallieren Sie manuell den Anbieter auf dem VMM-Server. Wenn Sie einen Cluster verwenden, entfernen Sie ihn von allen Knoten.
6. Wenn Sie eine Replikation nach Azure durchführen, müssen Sie den Microsoft Recovery Services-Agent manuell von Hyper-V-Hosts in den gelöschten Clouds entfernen.



### <a name="unregister-an-unconnected-vmm-server"></a>Aufheben der Registrierung eines nicht verbundenen VMM-Servers

1. Beenden Sie die Replikation virtueller Computer in Clouds auf dem VMM-Server, den Sie entfernen möchten.
2. Löschen Sie Netzwerkzuordnungen, die von Clouds auf dem VMM-Server verwendet werden, den Sie löschen möchten. Klicken Sie unter **Site Recovery-Infrastruktur** > **Für System Center VMM** > **Netzwerkzuordnung** mit der rechten Maustaste auf die Netzwerkzuordnung, und klicken Sie dann auf **Löschen**.
3. Notieren Sie die ID des VMM-Servers.
4. Heben Sie die Zuordnung zwischen Replikationsrichtlinien und Clouds auf dem VMM-Server auf, den Sie entfernen möchten.  Doppelklicken Sie unter **Site Recovery-Infrastruktur** > **Für System Center VMM** >  **Replikationsrichtlinien** auf die zugehörige Richtlinie. Klicken Sie mit der rechten Maustaste auf die Cloud, und klicken Sie dann auf **Zuordnung aufheben**.
5. Löschen Sie den VMM-Server oder den aktiven Knoten. Klicken Sie unter **Site Recovery-Infrastruktur** > **Für System Center VMM** > **VMM-Server** mit der rechten Maustaste auf den Server, und klicken Sie dann auf **Löschen**.
6. Laden Sie auf dem VMM-Server das [Bereinigungsskript](http://aka.ms/asr-cleanup-script-vmm) herunter, und führen Sie es aus. Öffnen Sie PowerShell mit der Option **Als Administrator ausführen**, um die Ausführungsrichtlinie für den Standardbereich (LocalMachine) zu ändern. Geben Sie im Skript die ID des VMM-Servers an, den Sie entfernen möchten. Das Skript entfernt die Informationen zu Registrierung und Cloudkopplung vom Server.
5. Führen Sie das Bereinigungsskript auf allen anderen VMM-Servern aus, die Clouds enthalten, welche mit Clouds auf dem zu entfernenden VMM-Server gekoppelt sind.
6. Führen Sie das Bereinigungsskript auf allen anderen passiven VMM-Clusterknoten aus, auf denen der Anbieter installiert ist.
7. Deinstallieren Sie manuell den Anbieter auf dem VMM-Server. Wenn Sie einen Cluster verwenden, entfernen Sie ihn von allen Knoten.
8. Wenn Sie eine Replikation nach Azure durchführen, können Sie den Microsoft Recovery Services-Agent von Hyper-V-Hosts in den gelöschten Clouds entfernen.

## <a name="unregister-a-hyper-v-host-in-a-hyper-v-site"></a>Aufheben der Registrierung eines Hyper-V-Hosts an einem Hyper-V-Standort

Hyper-V-Hosts, die nicht von VMM verwaltet werden, werden an einem Hyper-V-Standort gesammelt. Entfernen Sie einen Host an einem Hyper-V-Standort wie folgt:

1. Deaktivieren Sie die Replikation für Hyper-V-VMs, die sich auf dem Host befinden.
2. Heben Sie die Zuordnung von Richtlinien für den Hyper-V-Standort auf. Doppelklicken Sie unter **Site Recovery-Infrastruktur** > **Für Hyper-V-Standorte** >  **Replikationsrichtlinien** auf die zugehörige Richtlinie. Klicken Sie mit der rechten Maustaste auf den Standort, und klicken Sie dann auf **Zuordnung aufheben**.
3. Löschen Sie Hyper-V-Hosts. Klicken Sie unter **Site Recovery-Infrastruktur** > **Für System Center VMM** > **Hyper-V-Hosts** mit der rechten Maustaste auf den Server, und klicken Sie dann auf **Löschen**.
4. Löschen Sie den Hyper-V-Standort, nachdem alle Hosts daraus entfernt wurden. Klicken Sie unter **Site Recovery-Infrastruktur** > **Für System Center VMM** > **Hyper-V-Standorte** mit der rechten Maustaste auf den Standort, und klicken Sie dann auf **Löschen**.
5. Führen Sie das folgende Skript auf jedem Hyper-V-Host aus, den Sie entfernt haben. Das Skript bereinigt die Einstellungen auf dem Server und hebt die Registrierung beim Tresor auf.


        `` pushd .
        try
        {
             $windowsIdentity=[System.Security.Principal.WindowsIdentity]::GetCurrent()
             $principal=new-object System.Security.Principal.WindowsPrincipal($windowsIdentity)
             $administrators=[System.Security.Principal.WindowsBuiltInRole]::Administrator
             $isAdmin=$principal.IsInRole($administrators)
             if (!$isAdmin)
             {
                "Please run the script as an administrator in elevated mode."
                $choice = Read-Host
                return;       
             }

            $error.Clear()    
            "This script will remove the old Azure Site Recovery Provider related properties. Do you want to continue (Y/N) ?"
            $choice =  Read-Host

            if (!($choice -eq 'Y' -or $choice -eq 'y'))
            {
            "Stopping cleanup."
            return;
            }

            $serviceName = "dra"
            $service = Get-Service -Name $serviceName
            if ($service.Status -eq "Running")
            {
                "Stopping the Azure Site Recovery service..."
                net stop $serviceName
            }

            $asrHivePath = "HKLM:\SOFTWARE\Microsoft\Azure Site Recovery"
            $registrationPath = $asrHivePath + '\Registration'
            $proxySettingsPath = $asrHivePath + '\ProxySettings'
            $draIdvalue = 'DraID'

            if (Test-Path $asrHivePath)
            {
                if (Test-Path $registrationPath)
                {
                    "Removing registration related registry keys."    
                    Remove-Item -Recurse -Path $registrationPath
                }

                if (Test-Path $proxySettingsPath)
            {
                    "Removing proxy settings"
                    Remove-Item -Recurse -Path $proxySettingsPath
                }

                $regNode = Get-ItemProperty -Path $asrHivePath
                if($regNode.DraID -ne $null)
                {            
                    "Removing DraId"
                    Remove-ItemProperty -Path $asrHivePath -Name $draIdValue
                }
                "Registry keys removed."
            }

            # First retrive all the certificates to be deleted
            $ASRcerts = Get-ChildItem -Path cert:\localmachine\my | where-object {$_.friendlyname.startswith('ASR_SRSAUTH_CERT_KEY_CONTAINER') -or $_.friendlyname.startswith('ASR_HYPER_V_HOST_CERT_KEY_CONTAINER')}
            # Open a cert store object
            $store = New-Object System.Security.Cryptography.X509Certificates.X509Store("My","LocalMachine")
            $store.Open('ReadWrite')
            # Delete the certs
            "Removing all related certificates"
            foreach ($cert in $ASRcerts)
            {
                $store.Remove($cert)
            }
        }catch
        {    
            [system.exception]
            Write-Host "Error occured" -ForegroundColor "Red"
            $error[0]
            Write-Host "FAILED" -ForegroundColor "Red"
        }
        popd``



## <a name="disable-protection-for-a-vmware-vm-or-physical-server"></a>Deaktivieren des Schutzes für eine VMware-VM oder einen physischen Server

1. Klicken Sie unter **Geschützte Elemente** > **Replizierte Elemente** mit der rechten Maustaste auf den Computer, und klicken Sie dann auf **Löschen**.
2. Wählen Sie unter **Computer entfernen** eine der folgenden Optionen aus:
    - **Schutz für Computer deaktivieren (empfohlen)**. Verwenden Sie diese Option zum Beenden der Replikation des Computers. Site Recovery-Einstellungen werden automatisch bereinigt. Diese Option wird Ihnen nur unter folgenden Umständen angezeigt:
        - **Sie haben die Größe des VM-Volumes geändert**: Wenn Sie die Größe eines Volumes ändern, wechselt der virtuelle Computer in einen kritischen Zustand. Wählen Sie diese Option, um den Schutz unter Beibehaltung der Wiederherstellungspunkte in Azure zu deaktivieren. Wenn Sie den Schutz für den Computer wieder aktivieren, werden die Daten für das angepasste Volume an Azure übertragen.
        - **Sie haben vor kurzem ein Failover ausgeführt**: Nach dem Ausführen eines Failovers zum Testen der Umgebung wählen Sie diese Option aus, um den Schutz für lokale Computer erneut zu starten. Mit dieser Option werden die einzelnen virtuellen Computer deaktiviert, sodass Sie den Schutz für diese wieder aktivieren müssen. Das Deaktivieren des Computers mit dieser Einstellung wirkt sich nicht auf den replizierten virtuellen Computer in Azure aus. Deinstallieren Sie nicht den Mobilitätsdienst vom Computer.
    - **Verwaltung des Computers beenden**. Wenn Sie diese Option auswählen, wird der Computer nur aus dem Tresor entfernt. Lokale Sicherheitseinstellungen für den Computer sind nicht betroffen. Um Einstellungen vom Computer und den Computer aus dem Azure-Abonnement zu entfernen, müssen Sie die Einstellungen bereinigen, indem Sie den Mobilitätsdienst deinstallieren.

## <a name="disable-protection-for-a-hyper-v-vm-in-a-vmm-cloud"></a>Deaktivieren des Schutzes für eine Hyper-V-VM in einer VMM-Cloud

1. Klicken Sie unter **Geschützte Elemente** > **Replizierte Elemente** mit der rechten Maustaste auf den Computer, und klicken Sie dann auf **Löschen**.
2. Wählen Sie unter **Computer entfernen** eine der folgenden Optionen aus:

    - **Schutz für Computer deaktivieren (empfohlen)**. Verwenden Sie diese Option zum Beenden der Replikation des Computers. Site Recovery-Einstellungen werden automatisch bereinigt.
    - **Verwaltung des Computers beenden**. Wenn Sie diese Option auswählen, wird der Computer nur aus dem Tresor entfernt. Lokale Sicherheitseinstellungen für den Computer sind nicht betroffen. Um Einstellungen vom Computer und den Computer aus dem Azure-Abonnement zu entfernen, müssen Sie die Einstellungen manuell mithilfe der folgenden Anweisungen bereinigen. Wenn Sie den virtuellen Computer und dessen Festplatten löschen, beachten Sie, dass diese am Zielspeicherort entfernt werden.

### <a name="clean-up-protection-settings---replication-to-a-secondary-vmm-site"></a>Bereinigen von Schutzeinstellungen – Replikation an einen sekundären VMM-Standort

Wenn Sie **Verwaltung des Computers beenden** ausgewählt haben und eine Replikation an einen sekundären Standort durchführen, führen Sie dieses Skript auf dem primären Server aus, um die Einstellungen für den primären virtuellen Computer zu bereinigen. Klicken Sie in der VMM-Konsole auf die Schaltfläche „PowerShell“, um die VMM-PowerShell-Konsole zu öffnen. Ersetzen Sie „SQLVM1“ durch den Namen des virtuellen Computers.

         ``$vm = get-scvirtualmachine -Name "SQLVM1"
         Set-SCVirtualMachine -VM $vm -ClearDRProtection``
2. Führen Sie auf dem sekundären VMM-Server das folgende Skript aus, um die Einstellungen für den sekundären virtuellen Computer zu bereinigen:

        ``$vm = get-scvirtualmachine -Name "SQLVM1"
        Remove-SCVirtualMachine -VM $vm -Force``
3. Aktualisieren Sie auf dem sekundären VMM-Server die virtuellen Computer auf dem Hyper-V-Hostserver, damit der sekundäre virtuelle Computer in der VMM-Konsole neu erkannt wird.
4. Mit den obigen Schritten werden die Replikationseinstellungen auf dem VMM-Server gelöscht. Wenn Sie die Replikation für den virtuelle Computer beenden möchten, führen Sie das folgende Skript auf dem primären und sekundären virtuellen Computer aus. Ersetzen Sie „SQLVM1“ durch den Namen des virtuellen Computers.

        ``Remove-VMReplication –VMName “SQLVM1”``

### <a name="clean-up-protection-settings---replication-to-azure"></a>Bereinigen von Schutzeinstellungen – Replikation nach Azure

1. Wenn Sie **Verwaltung des Computers beenden** ausgewählt haben und die Replikation nach Azure durchführen, führen Sie dieses Skript mithilfe von PowerShell in der VMM-Konsole auf dem VMM-Quellserver aus.
        ``$vm = get-scvirtualmachine -Name "SQLVM1"
        Set-SCVirtualMachine -VM $vm -ClearDRProtection``
2. Mit den obigen Schritten werden die Replikationseinstellungen auf dem VMM-Server gelöscht. Um die Replikation für den virtuellen Computer zu beenden, der auf dem Hyper-V-Hostserver ausgeführt wird, führen Sie dieses Skript aus. Ersetzen Sie „SQLVM1“ durch den Namen Ihres virtuellen Computers und „host01.contoso.com“ durch den Namen des Hyper-V-Hostservers.

        ``$vmName = "SQLVM1"
        $hostName  = "host01.contoso.com"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'" -computername $hostName
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"  -computername $hostName
        $replicationService.RemoveReplicationRelationship($vm.__PATH)``


## <a name="disable-protection-for-a-hyper-v-vm-in-a-hyper-v-site"></a>Deaktivieren des Schutzes für eine Hyper-V-VM an einem Hyper-V-Standort

Gehen Sie folgendermaßen vor, wenn Sie Hyper-V-VMs ohne einen VMM-Server nach Azure replizieren.

1. Klicken Sie unter **Geschützte Elemente** > **Replizierte Elemente** mit der rechten Maustaste auf den Computer, und klicken Sie dann auf **Löschen**.
2. Unter **Computer entfernen** können Sie folgende Optionen auswählen:

   - **Schutz für Computer deaktivieren (empfohlen)**. Verwenden Sie diese Option zum Beenden der Replikation des Computers. Site Recovery-Einstellungen werden automatisch bereinigt.
   - **Verwaltung des Computers beenden**. Wenn Sie diese Option auswählen, wird der Computer nur aus dem Tresor entfernt. Lokale Sicherheitseinstellungen für den Computer sind nicht betroffen. Um Einstellungen vom Computer und den virtuellen Computer aus dem Azure-Abonnement zu entfernen, müssen Sie die Einstellungen manuell bereinigen. Wenn Sie den virtuellen Computer und dessen Festplatten löschen, werden sie am Zielspeicherort entfernt.
3. Wenn Sie **Verwaltung des Computers beenden** gewählt haben, führen Sie auf dem Hyper-V-Hostquellserver das folgende Skript aus, um die Replikation für den virtuellen Computer zu entfernen. Ersetzen Sie „SQLVM1“ durch den Namen des virtuellen Computers.

        $vmName = "SQLVM1"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'"
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"
        $replicationService.RemoveReplicationRelationship($vm.__PATH)



<!--HONumber=Jan17_HO1-->


