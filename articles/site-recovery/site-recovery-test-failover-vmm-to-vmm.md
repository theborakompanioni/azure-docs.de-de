---
title: Testfailover (VMM zu VMM) in Azure Site Recovery | Microsoft-Dokumentation
description: "Azure Site Recovery koordiniert Replikation, Failover und Wiederherstellung virtueller Computer und physischer Server. Informieren Sie sich über das Failover zu Azure oder zu einem sekundären Rechenzentrum."
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: pratshar
ms.translationtype: Human Translation
ms.sourcegitcommit: a1ba750d2be1969bfcd4085a24b0469f72a357ad
ms.openlocfilehash: afc4790d5714ce7145c8f4291a05acc2e9882a9b
ms.contentlocale: de-de
ms.lasthandoff: 06/20/2017


---
# <a name="test-failover-vmm-to-vmm-in-site-recovery"></a>Testfailover (VMM zu VMM) in Site Recovery


Dieser Artikel enthält Informationen und Anweisungen zum Ausführen eines Testfailovers oder eines DR-Drills (desaster recovery, Notfallwiederherstellung) von virtuellen Computern (virtual machines, VMs) und physischen Servern, die mit Azure Site Recovery geschützt sind. Sie müssen einen durch System Center Virtual Machine Manager (VMM) verwalteten lokalen Standort als Wiederherstellungsstandort verwenden.

Ein Testfailover wird ausgeführt, um Ihre Replikationsstrategie zu überprüfen oder einen DR-Drill ohne Datenverlust oder Ausfallzeiten durchzuführen. Ein Testfailover hat keinerlei Auswirkungen auf die laufende Replikation oder Ihre Produktionsumgebung. Sie können es entweder auf einem virtuellen Computer oder einem [Wiederherstellungsplan](site-recovery-create-recovery-plans.md) ausführen. Beim Auslösen eines Testfailovers müssen Sie das Netzwerk angeben, mit dem die virtuellen Testcomputer eine Verbindung herstellen. Sie können den Status des Testfailovers auf der Seite **Aufträge** verfolgen.  

Wenn Sie Kommentare oder Fragen haben, können Sie diese am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.


## <a name="prepare-the-infrastructure-for-test-failover"></a>Vorbereiten der Infrastruktur für Testfailover
Wenn Sie ein Testfailover mit einem vorhandenen Netzwerk durchführen möchten, bereiten Sie Active Directory, DHCP und DNS in diesem Netzwerk vor.

Wenn Sie ein Testfailover mit der Option zum automatischen Erstellen von VM-Netzwerken ausführen möchten, fügen Sie einen manuellen Schritt vor Gruppe 1 im Wiederherstellungsplan ein, der für das Testfailover verwendet werden soll. Fügen Sie anschließend die Infrastrukturressourcen zum automatisch erstellten Netzwerk hinzu, bevor Sie das Testfailover durchführen.

### <a name="things-to-note"></a>Was Sie beachten sollten
Bei einer Replikation zu einem sekundären Standort muss der Typ des Netzwerks, das vom Replikatcomputer verwendet wird, nicht dem Typ des logischen Netzwerks für das Testfailover entsprechen. Einige Kombinationen funktionieren aber unter Umständen nicht. Wenn das Replikat DHCP und eine VLAN-basierte Isolation verwendet, benötigt das VM-Netzwerk für das Replikat keinen statischen IP-Adresspool. In diesem Fall kann für das Testfailover also keine Windows-Netzwerkvirtualisierung verwendet werden, da keine Adresspools verfügbar sind. 

Das Testfailover funktioniert ebenfalls nicht, wenn das Replikatnetzwerk keine Isolation verwendet und im Testnetzwerk die Windows-Netzwerkvirtualisierung verwendet wird. Das liegt daran, dass das nicht isolierte Netzwerk nicht über die Subnetze verfügt, die zum Erstellen eines Netzwerks mit Windows-Netzwerkvirtualisierung erforderlich sind.

Auf welche Weise nach dem Failover eine Verbindung zwischen virtuellen Replikatcomputern und zugeordneten VM-Netzwerken besteht, hängt davon ab, wie das VM-Netzwerk in der VMM-Konsole konfiguriert ist.

#### <a name="vm-network-configured-with-no-isolation-or-vlan-isolation"></a>Ohne Isolation oder VLAN-Isolation konfiguriertes VM-Netzwerk
Wenn DHCP für das VM-Netzwerk definiert ist, wird der virtuelle Replikatcomputer mit der VLAN-ID verbunden. Hierbei kommen die Einstellungen zur Anwendung, die für den Netzwerkstandort im zugeordneten logischen Netzwerk angegeben sind. Der virtuelle Computer erhält seine IP-Adresse vom verfügbaren DHCP-Server. 

Sie müssen keinen statischen IP-Adresspool für das VM-Zielnetzwerk definieren. Bei Verwendung eines statischen IP-Adresspools für das VM-Netzwerk wird der virtuelle Replikatcomputer mit der VLAN-ID verbunden. Hierbei kommen die Einstellungen zur Anwendung, die für den Netzwerkstandort im zugeordneten logischen Netzwerk angegeben sind.

Der virtuelle Computer erhält seine IP-Adresse aus dem für das VM-Netzwerk definierten Adresspool. Ist für das VM-Zielnetzwerk kein statischer IP-Adresspool definiert, ist die IP-Adresszuweisung nicht erfolgreich. Erstellen Sie den IP-Adresspool sowohl auf dem Quell- als auch auf dem Ziel-VMM-Server, den Sie für den Schutz und die Wiederherstellung verwenden.

#### <a name="vm-network-with-windows-network-virtualization"></a>VM-Netzwerk mit Windows-Netzwerkvirtualisierung
Wenn ein VM-Netzwerk mit der Windows-Netzwerkvirtualisierung konfiguriert ist, müssen Sie für das VM-Zielnetzwerk einen statischen Adresspool definieren. Dies ist unabhängig davon, ob für das VM-Quellnetzwerk die Verwendung von DHCP oder die Verwendung eines statischen IP-Adresspools konfiguriert ist. 

Bei Verwendung von DHCP fungiert der VMM-Zielserver als DHCP-Server und stellt eine IP-Adresse aus dem für das VM-Zielnetzwerk definierten Pool bereit. Ist für den Quellserver die Verwendung eines statischen IP-Adresspools definiert, weist der VMM-Zielserver eine IP-Adresse aus dem Pool zu. In beiden Fällen ist die IP-Adresszuweisung nicht erfolgreich, wenn kein statischer IP-Adresspool definiert ist.


### <a name="prepare-dhcp"></a>Vorbereiten von DHCP
Wenn die am Testfailover beteiligten virtuellen Computer DHCP nutzen, muss innerhalb des isolierten Netzwerks, das für das Testfailover erstellt wurde, ein DHCP-Testserver erstellt werden.

### <a name="prepare-active-directory"></a>Vorbereiten von Active Directory
Wenn Sie ein Test-Failover durchführen möchten, um eine Anwendung zu testen, muss in der Testumgebung eine Kopie der Active Directory-Produktionsumgebung vorhanden sein. Weitere Informationen finden Sie unter [Überlegungen zum Testfailover für Active Directory](site-recovery-active-directory.md#test-failover-considerations).

### <a name="prepare-dns"></a>Vorbereiten von DNS
Gehen Sie wie folgt vor, um einen DNS-Server für das Test-Failover vorzubereiten:

* **DHCP**: Wenn virtuelle Computer DHCP nutzen, muss die IP-Adresse des Test-DNS auf dem DHCP-Testserver aktualisiert werden. Bei Verwendung der Windows-Netzwerkvirtualisierung fungiert der VMM-Server als DHCP-Server. Daher sollte die IP-Adresse des DNS im Testfailovernetzwerk aktualisiert werden. In diesem Fall werden die virtuellen Computer automatisch beim relevanten DNS-Server registriert.
* **Statische Adresse**: Wenn virtuelle Computer eine statische IP-Adresse verwenden, muss die IP-Adresse des DNS-Testservers im Testfailovernetzwerk aktualisiert werden. Das DNS muss eventuell mit der IP-Adresse der virtuellen Testcomputer aktualisiert werden. Hierzu können Sie das folgende Beispielskript verwenden:

        Param(
        [string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord



## <a name="run-a-test-failover"></a>Durchführen eines Test-Failovers
Hier erfahren Sie, wie Sie ein Test-Failover für einen Wiederherstellungsplan durchführen. Alternativ können Sie das Failover über die Registerkarte **Virtuelle Computer** auch für einen einzelnen virtuellen Computer durchführen.

![Blatt „Testfailover“](./media/site-recovery-test-failover-vmm-to-vmm/TestFailover.png)

1. Wählen Sie **Wiederherstellungspläne** > *Name des Wiederherstellungsplans* aus. Klicken Sie auf **Failover** > **Test Failover**veröffentlichen.
1. Geben Sie auf dem Blatt **Testfailover** an, wie die virtuellen Computer nach dem Testfailover mit Netzwerken verbunden werden sollen. Weitere Informationen finden Sie unter [Netzwerkoptionen](#network-options-in-site-recovery).
1. Verfolgen Sie den Verlauf des Failovers auf der Registerkarte **Aufträge** .
1. Vergewissern Sie sich nach Abschluss des Failovers, dass die virtuellen Computer erfolgreich starten.
1. Klicken Sie anschließend auf dem Wiederherstellungsplan auf **Cleanup test failover** (Testfailover bereinigen). Erfassen und speichern Sie unter **Notizen** alle Beobachtungen im Zusammenhang mit dem Testfailover. Durch diesen Schritt werden die während des Testfailovers erstellten virtuellen Computer und Netzwerke gelöscht.


## <a name="network-options-in-site-recovery"></a>Netzwerkoptionen in Site Recovery

Bei einem Testfailover werden Sie zum Auswählen von Netzwerkeinstellungen für die Testreplikatcomputer aufgefordert. Sie haben mehrere Optionen.  

| **Option für das Test-Failover** | **Beschreibung** | **Failoverprüfung** | **Details** |
| --- | --- | --- | --- |
| **Failover zu einem sekundären VMM-Standort (ohne Netzwerk)** |Wählen Sie kein VM-Netzwerk aus. |Beim Failover wird überprüft, ob die Testcomputer erstellt werden.<br/><br/>Der virtuelle Testcomputer wird auf dem Host erstellt, auf dem der virtuelle Replikatcomputer vorhanden ist. Er wird nicht der Cloud hinzugefügt, in der sich der replizierte virtuelle Computer befindet. |<p>Der Computer, für den das Failover ausgeführt wurde, ist mit keinem Netzwerk verbunden.<br/><br/>Der Computer kann nach der Erstellung mit einem VM-Netzwerk verbunden werden. |
| **Failover zu einem sekundären VMM-Standort (mit Netzwerk)** |Wählen Sie ein vorhandenes VM-Netzwerk aus. |Beim Failover wird geprüft, ob virtuelle Computer erstellt werden. |Der virtuelle Testcomputer wird auf dem Host erstellt, auf dem der virtuelle Replikatcomputer vorhanden ist. Er wird nicht der Cloud hinzugefügt, in der sich der replizierte virtuelle Computer befindet.<br/><br/>Erstellen Sie ein von Ihrem Produktionsnetzwerk isoliertes VM-Netzwerk.<br/><br/>Bei Verwendung eines VLAN-basierten Netzwerks empfiehlt es sich, hierzu in VMM ein separates logisches Netzwerk zu erstellen, das nicht in der Produktionsumgebung verwendet wird. Dieses logische Netzwerk dient zum Erstellen von VM-Netzwerken für Testfailover.<br/><br/>Das logische Netzwerk muss mindestens einem der Netzwerkadapter aller Hyper-V-Server zugeordnet werden, die virtuelle Computer hosten.<br/><br/>Bei logischen VLAN-Netzwerken müssen die Netzwerkstandorte, die Sie dem logischen Netzwerk hinzufügen, isoliert sein.<br/><br/>Bei Verwendung eines logischen Netzwerks, das auf der Windows-Netzwerkvirtualisierung basiert, erstellt Azure Site Recovery automatisch isolierte VM-Netzwerke. |
| **Failover zu einem sekundären VMM-Standort (mit Netzwerkerstellung)** |Auf der Grundlage der Einstellung, die Sie unter **Logisches Netzwerk** angeben und den zugehörigen Netzwerkstandorten wird automatisch ein temporäres Testnetzwerk erstellt. |Beim Failover wird geprüft, ob virtuelle Computer erstellt werden. |Verwenden Sie diese Option, wenn für den Wiederherstellungsplan mehr als ein VM-Netzwerk verwendet wird. Im Falle von Netzwerken, die auf der Windows-Netzwerkvirtualisierung basieren, kann diese Option im Netzwerk des virtuellen Replikatcomputers automatisch VM-Netzwerke mit den gleichen Einstellungen (Subnetze und IP-Adresspools) erstellen. Diese VM-Netzwerke werden nach dem Test-Failover automatisch bereinigt.</p><p>Der virtuelle Testcomputer wird auf dem Host erstellt, auf dem der virtuelle Replikatcomputer vorhanden ist. Er wird nicht der Cloud hinzugefügt, in der sich der replizierte virtuelle Computer befindet. |

> [!TIP]
> Die IP-Adresse, die einem virtuellen Computer bei einem Testfailover zugewiesen wird, ist mit der IP-Adresse identisch, die dem virtuellen Computer bei einem geplanten oder nicht geplanten Failover zugewiesen wird (sofern die IP-Adresse im Testfailovernetzwerk verfügbar ist). Wenn dieselbe IP-Adresse nicht im Testfailovernetzwerk verfügbar ist, wird dem virtuellen Computer eine andere IP-Adresse zugewiesen, die im Testfailovernetzwerk verfügbar ist.
>
>


## <a name="test-failover-to-a-production-network-on-a-recovery-site"></a>Testfailover zu einem Produktionsnetzwerk an einem Wiederherstellungsstandort
Beim Durchführen eines Testfailovers wird empfohlen, ein anderes Netzwerk als das Site Recovery-Netzwerk für die Produktion zu wählen, das Sie in [Netzwerkzuordnung](https://docs.microsoft.com/azure/site-recovery/site-recovery-network-mapping) angegeben haben. Beachten Sie folgende Punkte, wenn Sie wirklich die End-to-End-Netzwerkkonnektivität auf einem virtuellen Computer bewerten möchten, für den ein Failover durchgeführt wurde:

* Der primäre virtuelle Computer muss heruntergefahren werden, wenn Sie das Testfailover durchführen. Wenn Sie dies nicht tun, werden zwei virtuelle Computer mit derselben Identität gleichzeitig im selben Netzwerk ausgeführt. Diese Situation kann unerwünschte Folgen haben.
* Änderungen, die Sie auf virtuellen Computern mit Testfailover vornehmen, gehen verloren, wenn Sie die virtuellen Computer mit Testfailover bereinigen. Diese Änderungen werden nicht wieder auf dem primären virtuellen Computer repliziert.
* Diese Art, Leads zu testen, führt zu einem Ausfall Ihrer Produktionsanwendung. Fordern Sie Benutzer der Anwendung auf, die Anwendung während des DR-Drills nicht zu verwenden.  


## <a name="next-steps"></a>Nächste Schritte
Sobald Sie erfolgreich ein Testfailover durchgeführt haben, können Sie ein [Failover](site-recovery-failover.md) versuchen.

