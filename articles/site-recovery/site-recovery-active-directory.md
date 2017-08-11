---
title: "Schützen von Active Directory und DNS mit Azure Site Recovery | Microsoft-Dokumentation"
description: "Dieser Artikel beschreibt das Implementieren einer Notfallwiederherstellungs-Lösung für Active Directory mit Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: af1d9b26-1956-46ef-bd05-c545980b72dc
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 7/20/2017
ms.author: pratshar
ms.translationtype: HT
ms.sourcegitcommit: 22aa82e5cbce5b00f733f72209318c901079b665
ms.openlocfilehash: 197441fc24c178695d4eada6db59f503b21672ad
ms.contentlocale: de-de
ms.lasthandoff: 07/24/2017

---
# <a name="protect-active-directory-and-dns-with-azure-site-recovery"></a>Schützen von Active Directory und DNS mit Azure Site Recovery
Für Unternehmensanwendungen, z.B. SharePoint, Dynamics AX und SAP, ist eine Active Directory- und DNS-Infrastruktur erforderlich, damit sie richtig funktionieren. Wenn Sie eine Notfallwiederherstellungs-Lösung für Anwendungen erstellen, müssen Sie daran denken, dass Sie Active Directory und DNS vorrangig vor den anderen Anwendungskomponenten schützen und wiederherstellen müssen, um sicherzustellen, dass Vorgänge richtig ablaufen, wenn ein Notfall eintritt.

Site Recovery ist ein Azure-Dienst, der Notfallwiederherstellung durch Koordinierung von Replikation, Failover und Wiederherstellung virtueller Computer bietet. Site Recovery unterstützt eine Reihe von Replikationsszenarien, um für virtuelle Computer und Anwendungen konsistenten Schutz und ein reibungsloses Failover in privaten/öffentlichen Clouds bzw. in Clouds von Hostern zu bieten.

Mit Site Recovery können Sie für Active Directory einen vollständig automatisierten Notfallwiederherstellungs-Plan erstellen. Wenn eine Störung eintritt, können Sie ein Failover von überall aus in Sekundenschnelle einleiten und Active Directory binnen weniger Minuten wieder in Betrieb nehmen. Wenn Sie an Ihrem primären Standort Active Directory für mehrere Anwendungen wie SharePoint und SAP nutzen und für den gesamten Standort ein Failover wünschen, können Sie zunächst für Active Directory ein Failover mit Site Recovery und anschließend für die anderen Anwendungen ein Failover mithilfe anwendungsspezifischer Wiederherstellungspläne ausführen.

In diesem Artikel wird erläutert, wie Sie eine Lösung für die Notfallwiederherstellung für Active Directory erstellen und ein geplantes/ungeplantes oder Testfailover mithilfe eines mit einem Klick aktivierbaren Wiederherstellungsplans, unterstützter Konfigurationen und erfüllter Voraussetzungen ausführen.  Sie sollten mit Active Directory und Azure Site Recovery vertraut sein, bevor Sie beginnen.

## <a name="replicating-domain-controller"></a>Replizieren eines Domänencontrollers

Sie müssen die [Site Recovery-Replikation](#enable-protection-using-site-recovery) auf mindestens einem virtuellen Computer mit einem Domänencontroller und DNS einrichten. Wenn in Ihrer Umgebung [mehrere Domänencontroller](#environment-with-multiple-domain-controllers) vorhanden sind, müssen Sie neben dem Replizieren des virtuellen Domänencontrollercomputers mit Site Recovery noch einen [zusätzlichen Domänencontroller](#protect-active-directory-with-active-directory-replication) am Zielstandort (Azure oder ein sekundäres lokales Rechenzentrum) einrichten. 

### <a name="single-domain-controller-environment"></a>Umgebung mit einem Domänencontroller
Wenn nur wenige Anwendungen und ein einzelner Domänencontroller vorhanden sind und Sie ein Failover für den gesamten Standort ausführen möchten, empfiehlt es sich, den Domänencontroller mit Site Recovery auf den sekundären Standort zu replizieren (dabei spielt es keine Rolle, ob Sie ein Failover zu Azure oder einem sekundären Standort ausführen). Derselbe replizierte Domänencontroller/virtuelle DNS-Computer kann auch für ein [Testfailover](#test-failover-considerations) verwendet werden.

### <a name="environment-with-multiple-domain-controllers"></a>Umgebung mit mehreren Domänencontrollern
Bei vielen Anwendungen und mehreren Domänencontrollern in der Umgebung, oder wenn Sie das gleichzeitige Failover verschiedener Anwendungen planen, empfehlen wir Ihnen, zusätzlich zum Replizieren des virtuellen Computers, auf dem sich der Domänencontroller befindet, mit Site Recovery auch die Einrichtung eines [zusätzlichen Domänencontrollers](#protect-active-directory-with-active-directory-replication) am Zielstandort (Azure oder ein sekundäres lokales Rechenzentrum). Für ein [Testfailover](#test-failover-considerations) verwenden Sie den von Site Recovery replizierten Domänencontroller und für das Failover den zusätzlichen Domänencontroller am Zielstandort. 


In den folgenden Abschnitten wird erläutert, wie der Schutz für einen Domänencontroller in Site Recovery aktiviert und ein Domänencontroller in Azure eingerichtet wird.

## <a name="prerequisites"></a>Voraussetzungen
* Active Directory- und DNS-Server sind lokal bereitgestellt.
* Ein Azure Site Recovery Services-Tresor im Microsoft Azure-Abonnement.
* Falls Sie zu Azure replizieren, führen Sie das Tool „Azure Virtual Machine Readiness Assessment“ auf VMs aus, um sicherzustellen, dass diese mit Azure-VMs und Azure Site Recovery Services kompatibel sind.

## <a name="enable-protection-using-site-recovery"></a>Aktivieren des Schutzes mit Site Recovery
### <a name="protect-the-virtual-machine"></a>Schützen des virtuellen Computers
Aktivieren Sie den Schutz von Domänencontroller/DNS-VM in Site Recovery. Konfigurieren Sie Site Recovery-Einstellungen auf Basis des Typs des virtuellen Computers (Hyper-V oder VMware). Der mithilfe von Site Recovery replizierte Domänencontroller wird für ein [Testfailover](#test-failover-considerations) verwendet. Stellen Sie sicher, dass folgende Anforderungen erfüllt sind:

1. Der Domänencontroller ist ein globaler Katalogserver
2. Der Domänencontroller sollte der FSMO-Rollenbesitzer für Rollen sein, die während eines Testfailovers benötigt werden (andernfalls müssen diese Rollen nach dem Failover [übernommen](http://aka.ms/ad_seize_fsmo) werden)

### <a name="configure-virtual-machine-network-settings"></a>Konfigurieren der Netzwerkeinstellungen virtueller Computer
Konfigurieren Sie für die Domänencontroller- bzw. DNS-VM die Netzwerkeinstellungen in Site Recovery so, dass der virtuelle Computer nach einem Failover dem richtigen Netzwerk zugeordnet wird. 

![VM-Netzwerkeinstellungen](./media/site-recovery-active-directory/DNS-Target-IP.png)

## <a name="protect-active-directory-with-active-directory-replication"></a>Schützen von Active Directory mit Active Directory-Replikation
### <a name="site-to-site-protection"></a>Standort-zu-Standort-Schutz
Erstellen Sie einen Domänencontroller am sekundären Standort. Geben Sie beim Heraufstufen des Servers auf eine Domänencontrollerrolle den Namen der gleichen Domäne an, die am primären Standort verwendet wird. Sie können das Snap-In **Active Directory-Standorte und Dienste** zum Konfigurieren von Einstellungen für das Standortverknüpfungsobjekt nutzen, dem die Standorte hinzugefügt werden. Durch Konfigurieren von Einstellungen für eine Standortverknüpfung können Sie steuern, wann und wie oft eine Replikation zwischen zwei oder mehr Standorten erfolgt. Weitere Informationen finden Sie unter [Planen der Replikation zwischen Standorten](https://technet.microsoft.com/library/cc731862.aspx).

### <a name="site-to-azure-protection"></a>Standort-zu-Azure-Schutz
Befolgen Sie zum [Erstellen eines Domänencontrollers in einem virtuellen Azure-Netzwerk](../active-directory/active-directory-install-replica-active-directory-domain-controller.md)diese Anweisungen. Geben Sie beim Heraufstufen des Servers auf eine Domänencontrollerrolle den gleichen Domänennamen an, der am primären Standort verwendet wird.

Dann [konfigurieren Sie den DNS-Server für das virtuelle Netzwerk neu](../active-directory/active-directory-install-replica-active-directory-domain-controller.md#reconfigure-dns-server-for-the-virtual-network), um den DNS-Server in Azure zu verwenden.

![Azure-Netzwerk](./media/site-recovery-active-directory/azure-network.png)

**DNS im Azure-Produktionsnetzwerk**

## <a name="test-failover-considerations"></a>Überlegungen zum Test-Failover
Das Testfailover erfolgt in einem Netzwerk, das vom Produktionsnetzwerk isoliert ist, damit keine Auswirkungen auf Produktionsworkloads auftreten.

Bei den meisten Anwendungen muss außerdem ein Domänencontroller und ein DNS-Server vorhanden sein, damit das Testfailover funktioniert. Bevor ein Failover für die Anwendung erfolgt, muss ein Domänencontroller im isolierten Netzwerk erstellt werden, der für das Testfailover verwendet werden soll. Die einfachste Möglichkeit hierzu besteht darin, mit Site Recovery eine Domänencontroller- bzw. DNS-VM zu replizieren. Führen Sie dann ein Testfailover des virtuellen Domänencontrollercomputers aus, bevor Sie ein Testfailover des Wiederherstellungsplans für die Anwendung starten. Gehen Sie hierzu wie folgt vor:

1. [Replizieren](site-recovery-replicate-vmware-to-azure.md) Sie die Domänencontroller-/DNS-VM mit Site Recovery.
1. Erstellen Sie ein isoliertes Netzwerk. Jedes in Azure erstellte virtuelle Netzwerk ist standardmäßig von anderen Netzwerken isoliert. Der IP-Adressbereich dieses Netzwerks sollte mit dem Ihres Produktionsnetzwerks identisch sein. Aktivieren Sie nicht die Standort-zu-Standort-Konnektivität in diesem Netzwerk.
1. Stellen Sie eine im Netzwerk erstellte DNS-IP-Adresse als die IP-Adresse bereit, die der virtuelle DNS-Computer abrufen soll. Wenn Sie zu Azure replizieren, geben Sie die IP-Adresse für den virtuellen Computer, der beim Failover verwendet wird, in der Einstellung für die **Ziel-IP-Adresse** in den **Compute und Netzwerk**-Einstellungen an. 

    ![Ziel-IP](./media/site-recovery-active-directory/DNS-Target-IP.png) **Ziel-IP**

    ![Azure-Testnetzwerk](./media/site-recovery-active-directory/azure-test-network.png)

    **DNS im Azure-Testnetzwerk**

> [!TIP]
> Site Recovery versucht, virtuelle Testcomputer in einem Subnetz mit demselben Namen und derselben IP zu erstellen, die in den Einstellungen des virtuellen Computers unter **Compute und Netzwerk** angegeben sind. Wenn das Subnetz mit demselben Namen nicht im virtuellen Azure-Netzwerk für das Testfailover verfügbar ist, wird ein virtueller Testcomputer im ersten Subnetz (in alphabetischer Reihenfolge) erstellt. Wenn die Ziel-IP Teil des ausgewählten Subnetzes ist, versucht Site Recovery, die Testfailover-VM mit der Ziel-IP zu erstellen. Wenn die Ziel-IP nicht Teil des ausgewählten Subnetzes ist, wird die Testfailover-VM mit einer beliebigen verfügbaren IP im ausgewählten Subnetz erstellt. 
>
>


1. Wenn Sie zu einem anderen lokalen Standort replizieren und DHCP verwenden, befolgen Sie die Anweisungen zum [Einrichten von DNS und DHCP für das Testfailover](site-recovery-test-failover-vmm-to-vmm.md#prepare-dhcp)
1. Führen Sie ein Testfailover des virtuellen Computers mit dem Domänencontroller im isolierten Netzwerk aus. Verwenden Sie den neuesten verfügbaren **anwendungskonsistenten** Wiederherstellungspunkt des virtuellen Computers mit dem Domänencontroller, um das Testfailover durchzuführen. 
1. Führen Sie ein Testfailover für den Wiederherstellungsplan aus, der virtuelle Computer der Anwendung enthält. 
1. Nach dem Abschluss der Tests **bereinigen Sie das Testfailover** auf dem virtuellen Domänencontrollercomputer. Mit diesem Schritt wird der für das Testfailover erstellte Domänencontroller gelöscht.


### <a name="removing-reference-to-other-domain-controllers"></a>Entfernen von Verweisen auf andere Domänencontroller
Verwenden Sie bei einem Testfailover nicht alle Domänencontroller im Testnetzwerk. Um die Verweise auf andere in der Produktionsumgebung vorhandene Domänencontroller zu entfernen, müssen Sie für fehlende Domänencontroller ggf. [FSMO-Funktionen von Active Directory-Rollen übernehmen](http://aka.ms/ad_seize_fsmo) und [Metadaten bereinigen](https://technet.microsoft.com/library/cc816907.aspx). 



> [!IMPORTANT]
> Einige der im folgenden Abschnitt beschriebenen Konfigurationen sind nicht die Standardkonfigurationen für Domänencontroller. Wenn Sie diese Änderungen an einem Domänencontroller der Produktionsumgebung nicht vornehmen möchten, können Sie einen dedizierten Domänencontroller für das Site Recovery-Testfailover erstellen und die Änderungen an diesen durchführen.  
>
>

### <a name="issues-because-of-virtualization-safeguards"></a>Probleme aufgrund von Virtualisierungssicherheitsmechanismen 

Ab Windows Server 2012 [sind zusätzliche Sicherheitsmechanismen in Active Directory Domain Services integriert](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100). Mit diesen Sicherheitsmechanismen sind virtualisierte Domänencontroller besser vor USN-Rollbacks geschützt, solange die zugrunde liegende Hypervisorplattform „VM-GenerationID“ unterstützt. Azure unterstützt „VM-GenerationID“, sodass Domänencontroller, die Windows Server 2012 oder höher auf virtuellen Azure-Computern ausführen, über diese zusätzlichen Sicherheitsmechanismen verfügen. 


Wenn die „VM-GenerationID“ zurückgesetzt wird, wird dies auch für die „invocationID“ der AD DS-Datenbank durchgeführt, der RID-Pool wird verworfen, und SYSVOL wird als nicht autoritativ gekennzeichnet. Weitere Informationen finden Sie unter [Einführung in die Virtualisierung der Active Directory Domain Services](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100) und [Sichere Virtualisierung von DFSR](https://blogs.technet.microsoft.com/filecab/2013/04/05/safely-virtualizing-dfsr/).

Durch ein Failover zu Azure kann „VM-GenerationID“ zurückgesetzt werden, und hier kommt der zusätzliche Sicherheitsmechanismus zum Einsatz, wenn der virtuelle Domänencontrollercomputer in Azure gestartet wird. Dies kann zu einer **erheblichen Verzögerung** für Benutzer bei der Anmeldung beim virtuellen Domänencontrollercomputer führen. Da dieser Domänencontroller nur in einem Testfailover verwendet wird, sind keine Virtualisierungsschutzmechanismen erforderlich. Um sicherzustellen, dass „VM-GenerationID“ für den virtuellen Domänencontrollercomputer nicht geändert wird, können Sie auf dem lokalen Domänencontroller den folgenden DWORD-Wert auf „4“ ändern.

        
        HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\gencounter\Start
 

#### <a name="symptoms-of-virtualization-safeguards"></a>Symptome der Virtualisierungssicherheitsmechanismen
 
Wenn Virtualisierungssicherheitsmechanismen nach einem Testfailover ausgelöst wurden, können folgende Symptome auftreten:  

Änderung der Generierungs-ID

![Änderung der Generierungs-ID](./media/site-recovery-active-directory/Event2170.png)

Änderung der Aufrufkennung

![Änderung der Aufrufkennung](./media/site-recovery-active-directory/Event1109.png)

SYSVOL- und NETLOGON-Freigaben sind nicht verfügbar

![SYSVOL-Freigabe](./media/site-recovery-active-directory/sysvolshare.png)

![NTFRS SYSVOL](./media/site-recovery-active-directory/Event13565.png)

Alle DFSR-Datenbanken werden gelöscht

![Löschen der DFSR-DB](./media/site-recovery-active-directory/Event2208.png)


> [!IMPORTANT]
> Einige der im folgenden Abschnitt beschriebenen Konfigurationen sind nicht die Standardkonfigurationen für Domänencontroller. Wenn Sie diese Änderungen an einem Domänencontroller der Produktionsumgebung nicht vornehmen möchten, können Sie einen dedizierten Domänencontroller für das Site Recovery-Testfailover erstellen und die Änderungen an diesen durchführen.  
>
>


### <a name="troubleshooting-domain-controller-issues-during-test-failover"></a>Behandlung von Domänencontrollerproblemen während des Testfailovers


Führen Sie an einer Eingabeaufforderung den folgenden Befehl aus, um zu überprüfen, ob die Ordner „SYSVOL“ und „NETLOGON“ freigegeben sind:

    NET SHARE

Führen Sie an einer Eingabeaufforderung den folgenden Befehl aus, um sicherzustellen, dass der Domänencontroller ordnungsgemäß funktioniert.

    dcdiag /v > dcdiag.txt

Suchen Sie im Ausgabeprotokoll nach folgendem Text, um sicherzustellen, dass der Domänencontroller ordnungsgemäß ausgeführt wird. 

* „passed test Connectivity“
* „passed test Advertising“
* „passed test MachineAccount“

Wenn die oben genannten Bedingungen erfüllt sind, ist es wahrscheinlich, dass der Domänencontroller gut funktioniert. Wenn nicht, probieren Sie die folgenden Schritte aus:


* Führen Sie eine autoritative Wiederherstellung des Domänencontrollers durch.
    * Die [FRS-Replikation wird zwar nicht empfohlen](https://blogs.technet.microsoft.com/filecab/2014/06/25/the-end-is-nigh-for-frs/), wenn Sie sie aber dennoch verwenden, führen Sie [diese](https://support.microsoft.com/kb/290762) Schritte aus, um eine autoritative Wiederherstellung durchzuführen. Weitere Informationen zu den im vorhergehenden Link erwähnten Burflags finden Sie [hier](https://blogs.technet.microsoft.com/janelewis/2006/09/18/d2-and-d4-what-is-it-for/).
    * Wenn Sie DFSR-Replikation verwenden, führen Sie [diese Schritte](https://support.microsoft.com/kb/2218556) aus, um eine autoritative Wiederherstellung durchzuführen. Dazu können Sie auch die unter diesem [Link](https://blogs.technet.microsoft.com/thbouche/2013/08/28/dfsr-sysvol-authoritative-non-authoritative-restore-powershell-functions/) verfügbaren PowerShell-Funktionen verwenden. 
    
* Umgehen Sie die angeforderte Erstsynchronisierung, indem Sie den folgenden Registrierungsschlüssel auf dem lokalen Domänencontroller auf 0 festlegen. Wenn dieses DWORD-Element nicht vorhanden ist, können Sie es unter dem Knoten „Parameter“ erstellen. Weitere Informationen dazu finden Sie [hier](https://support.microsoft.com/kb/2001093).

        HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters\Repl Perform Initial Synchronizations

* Deaktivieren Sie die Anforderung, dass ein globaler Katalogserver zur Überprüfung der Benutzeranmeldung verfügbar sein muss, indem Sie folgenden Registrierungsschlüssel auf dem lokalen Domänencontroller auf 1 festlegen. Wenn dieses DWORD-Element nicht vorhanden ist, können Sie es unter dem Knoten „Lsa“ erstellen. Weitere Informationen dazu finden Sie [hier](http://support.microsoft.com/kb/241789).

        HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\IgnoreGCFailures



### <a name="dns-and-domain-controller-on-different-machines"></a>DNS und Domänencontroller auf unterschiedlichen Computern
Wenn der DNS sich nicht auf dem gleichen virtuellen Computer wie der Domänencontroller befindet, müssen Sie eine DNS-VM für das Testfailover erstellen. Falls sich beide auf dem gleichen virtuellen Computer befinden, können Sie diesen Abschnitt überspringen.

Sie können einen neuen DNS-Server verwenden und alle erforderlichen Zonen erstellen. Wenn Ihre Active Directory-Domäne beispielsweise „contoso.com“ lautet, können Sie eine DNS-Zone mit dem Namen „contoso.com“ erstellen. Die zu Active Directory gehörenden Einträge müssen wie folgt in DNS aktualisiert werden:

1. Stellen Sie sicher, dass diese Einstellungen vorhanden sind, bevor ein anderer virtueller Computer im Wiederherstellungsplan online geschaltet wird:
   
   * Die Zone muss nach dem Stammnamen der Gesamtstruktur benannt werden.
   * Der Zone muss eine Datei zugrunde liegen.
   * Für die Zone müssen sichere und nicht sichere Updates möglich sein.
   * Die Auflösung des virtuellen Computers mit dem Domänencontroller muss auf die IP-Adresse des virtuellen Computers mit DNS zeigen.
2. Führen Sie auf dem virtuellen Computer mit dem Domänencontroller den folgenden Befehl aus:
   
    `nltest /dsregdns`
3. Fügen Sie dem DNS-Server eine Zone hinzu, erlauben Sie nicht sichere Updates, und fügen Sie einen Eintrag dafür dem DNS hinzu:
   
        dnscmd /zoneadd contoso.com  /Primary
        dnscmd /recordadd contoso.com  contoso.com. SOA %computername%.contoso.com. hostmaster. 1 15 10 1 1
        dnscmd /recordadd contoso.com %computername%  A <IP_OF_DNS_VM>
        dnscmd /config contoso.com /allowupdate 1

## <a name="next-steps"></a>Nächste Schritte
Lesen Sie [Welche Workloads können mit Azure Site Recovery geschützt werden?](site-recovery-workload.md) , um weitere Informationen über den Schutz von Unternehmensworkloads mit Azure Site Recovery zu erhalten.


