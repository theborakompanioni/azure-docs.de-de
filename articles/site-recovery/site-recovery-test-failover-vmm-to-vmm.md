---
title: Testfailover (VMM auf VMM) in Site Recovery (Azure) | Microsoft-Dokumentation
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
ms.sourcegitcommit: 6b1a5b2879a7b98ec4ad3e8ebbc9e95c0740d89f
ms.openlocfilehash: 3aaa005319b1ce2a10cd913c63b31860d31b797e
ms.contentlocale: de-de
ms.lasthandoff: 02/23/2017


---
# <a name="test-failover-vmm-to-vmm-in-site-recovery"></a>Testfailover (VMM auf VMM) in Site Recovery
> [!div class="op_single_selector"]
> * [Testfailover auf Azure](./site-recovery-test-failover-to-azure.md)
> * [Testfailover (VMM zu VMM)](./site-recovery-test-failover-vmm-to-vmm.md)


Dieser Artikel enthält Informationen und Anweisungen zum Ausführen eines Testfailovers oder eines DR-Drills von virtuellen Computern und physischen Servern, die mit Site Recovery und einem mithilfe von VMM verwalteten lokalen Speicherort als Wiederherstellungsstandort geschützt sind.

Kommentare oder Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)veröffentlichen.

Ein Testfailover wird ausgeführt, um Ihre Replikationsstrategie zu überprüfen oder einen Notfallwiederherstellungsdrill ohne Datenverlust oder Ausfallzeiten durchzuführen. Die Ausführung eines Testfailovers hat keinerlei Auswirkungen auf die laufende Replikation oder Ihre Produktionsumgebung. Ein Testfailover kann auf einem virtuellen Computer oder in einem [Wiederherstellungsplan](site-recovery-create-recovery-plans.md) ausgeführt werden. Beim Auslösen eines Testfailovers müssen Sie das Netzwerk angeben, mit dem virtuelle Testcomputer eine Verbindung herstellen. Nach dem Auslösen eines Testfailovers können Sie den Fortschritt auf der Seite **Aufträge** nachverfolgen.  


## <a name="preparing-infrastructure-for-test-failover"></a>Vorbereiten der Infrastruktur für Testfailover
* Wenn Sie ein Test-Failover mit einem vorhandenen Netzwerk durchführen möchten, bereiten Sie Active Directory, DHCP und DNS in diesem Netzwerk vor.
* Wenn Sie ein Test-Failover mit der Option für die automatische Erstellung von VM-Netzwerken durchführen möchten, fügen Sie vor der ersten Gruppe des Wiederherstellungsplans, den Sie für das Test-Failover verwenden möchten, einen manuellen Schritt ein, und fügen Sie dem automatisch erstellten Netzwerk dann vor der Durchführung des Test-Failovers die Infrastrukturressourcen hinzu.

### <a name="things-to-note"></a>Was Sie beachten sollten
* Bei einer Replikation an einen sekundären Standort muss der Typ des vom Replikatcomputer verwendeten Netzwerks nicht dem Typ des logischen Netzwerks für das Test-Failover entsprechen. Einige Kombinationen funktionieren aber unter Umständen nicht. Wenn das Replikat DHCP und eine VLAN-basierte Isolation verwendet, benötigt das VM-Netzwerk für das Replikat keinen statischen IP-Adresspool. In diesem Fall könnte für das Test-Failover also keine Windows-Netzwerkvirtualisierung verwendet werden, da keine Adresspools verfügbar sind. Das Test-Failover funktioniert auch nicht, wenn das Replikatnetzwerk nicht isoliert ist und im Testnetzwerk die Windows-Netzwerkvirtualisierung verwendet wird. Der Grund: Das nicht isolierte Netzwerk verfügt nicht über die Subnetze, die zum Erstellen eines Netzwerks mit Windows-Netzwerkvirtualisierung erforderlich sind.
* Welche Verbindung nach dem Failover zwischen virtuellen Replikatcomputern und zugeordneten VM-Netzwerken besteht, hängt davon ab, wie das VM-Netzwerk in der VMM-Konsole konfiguriert ist:
  * **VM-Netzwerk ohne Isolation oder VLAN-Isolation**: Bei definiertem DHCP für das VM-Netzwerk wird der virtuelle Replikatcomputer mit der VLAN-ID verbunden. Hierbei kommen die Einstellungen zur Anwendung, die für den Netzwerkstandort im zugeordneten logischen Netzwerk angegeben sind. Der virtuelle Computer erhält seine IP-Adresse vom verfügbaren DHCP-Server. Für das VM-Zielnetzwerk muss kein statischer IP-Adresspool definiert sein. Bei Verwendung eines statischen IP-Adresspools für das VM-Netzwerk wird der virtuelle Replikatcomputer mit der VLAN-ID verbunden. Hierbei kommen die Einstellungen zur Anwendung, die für den Netzwerkstandort im zugeordneten logischen Netzwerk angegeben sind. Der virtuelle Computer erhält seine IP-Adresse aus dem für das VM-Netzwerk definierten Adresspool. Ist für das VM-Zielnetzwerk kein statischer IP-Adresspool definiert, ist die IP-Adresszuweisung nicht erfolgreich. Der IP-Adresspool muss sowohl auf dem Quell- als auch auf dem Ziel-VMM-Server erstellt werden, den Sie für den Schutz und die Wiederherstellung verwenden möchten.
  * **VM-Netzwerk mit Windows-Netzwerkvirtualisierung**: Wenn ein VM-Netzwerk mit dieser Einstellung konfiguriert ist, muss für das VM-Zielnetzwerk ein statischer Adresspool definiert werden, und zwar unabhängig davon, ob für das VM-Quellnetzwerk die Verwendung von DHCP oder die Verwendung eines statischen IP-Adresspools konfiguriert ist. Bei Verwendung von DHCP fungiert der VMM-Zielserver als DHCP-Server und stellt eine IP-Adresse aus dem für das VM-Zielnetzwerk definierten Pool bereit. Ist für den Quellserver die Verwendung eines statischen IP-Adresspools definiert, weist der VMM-Zielserver eine IP-Adresse aus dem Pool zu. In beiden Fällen ist die IP-Adresszuweisung nicht erfolgreich, wenn kein statischer IP-Adresspool definiert ist.


### <a name="prepare-dhcp"></a>Vorbereiten von DHCP
Wenn die am Test-Failover beteiligten virtuellen Computer DHCP nutzen, muss innerhalb des für das Test-Failover erstellten isolierten Netzwerks ein DHCP-Testserver erstellt werden.

### <a name="prepare-active-directory"></a>Vorbereiten von Active Directory
Wenn Sie ein Test-Failover durchführen möchten, um eine Anwendung zu testen, muss in der Testumgebung eine Kopie der Active Directory-Produktionsumgebung vorhanden sein. Unter [Überlegungen zum Testfailover für Active Directory](site-recovery-active-directory.md#test-failover-considerations) finden Sie weitere Details.

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
Hier erfahren Sie, wie Sie ein Test-Failover für einen Wiederherstellungsplan durchführen. Alternativ können Sie das Failover über die Registerkarte **Virtuelle Computer** auch für einen einzelnen virtuellen Computer oder physischen Server durchführen.

![Testfailover](./media/site-recovery-test-failover-vmm-to-vmm/TestFailover.png)

1. Wählen Sie **Wiederherstellungspläne** > *Name des Wiederherstellungsplans* aus. Klicken Sie auf **Failover** > **Test Failover**veröffentlichen.
1. Geben Sie auf dem Blatt **Testfailover** an, wie die virtuellen Computer nach dem Testfailover mit Netzwerken verbunden werden sollen. Weitere Details finden Sie in den [Netzwerkoptionen](#network-options-in-site-recovery).
1. Verfolgen Sie den Verlauf des Failovers auf der Registerkarte **Aufträge** .
1. Vergewissern Sie sich anschließend, dass die virtuellen Computer erfolgreich starten.
1. Klicken Sie anschließend im Wiederherstellungsplan auf **Cleanup test failover** (Testfailover bereinigen). Erfassen und speichern Sie unter **Notizen** alle Beobachtungen im Zusammenhang mit dem Test-Failover. Dadurch werden die während des Testfailovers erstellten virtuellen Computer und Netzwerke gelöscht.


## <a name="network-options-in-site-recovery"></a>Netzwerkoptionen in Site Recovery

Beim einem Test-Failover werden Sie zum Auswählen von Netzwerkeinstellungen für die Testreplikatcomputer aufgefordert. Hierbei stehen mehrere Optionen zur Verfügung:  

| **Option für das Test-Failover** | **Beschreibung** | **Failoverprüfung** | **Details** |
| --- | --- | --- | --- |
| **Failover zu einem sekundären VMM-Standort (ohne Netzwerk)** |Wählen Sie kein VM-Netzwerk aus. |Beim Failover wird überprüft, ob die Testcomputer erstellt werden.<br/><br/>Der virtuelle Testcomputer wird auf dem Host erstellt, auf dem sich auch der virtuelle Replikatcomputer befindet. Er wird nicht der Cloud hinzugefügt, in der sich der replizierte virtuelle Computer befindet. |<p>Der Computer, für den das Failover ausgeführt wurde, ist mit keinem Netzwerk verbunden.<br/><br/>Der virtuelle Computer kann nach der Erstellung mit einem VM-Netzwerk verbunden werden. |
| **Failover zu einem sekundären VMM-Standort (mit Netzwerk)** |Wählen Sie ein vorhandenes VM-Netzwerk aus. |Beim Failover wird geprüft, ob virtuelle Computer erstellt werden. |Der virtuelle Testcomputer wird auf dem Host erstellt, auf dem sich auch der virtuelle Replikatcomputer befindet. Er wird nicht der Cloud hinzugefügt, in der sich der replizierte virtuelle Computer befindet.<br/><br/>Erstellen Sie ein von Ihrem Produktionsnetzwerk isoliertes VM-Netzwerk.<br/><br/>Bei Verwendung eines VLAN-basierten Netzwerks empfiehlt es sich, hierzu in VMM ein separates logisches Netzwerk zu erstellen, das nicht in der Produktionsumgebung verwendet wird. Dieses logische Netzwerk dient zum Erstellen von VM-Netzwerken für das Testfailover.<br/><br/>Das logische Netzwerk muss mindestens einem der Netzwerkadapter aller Hyper-V-Server zugeordnet werden, die virtuelle Computer hosten.<br/><br/>Bei logischen VLAN-Netzwerken müssen die Netzwerkstandorte, die Sie dem logischen Netzwerk hinzufügen, isoliert sein.<br/><br/>Bei Verwendung eines logischen Netzwerks, das auf der Windows-Netzwerkvirtualisierung basiert, erstellt Azure Site Recovery automatisch isolierte VM-Netzwerke. |
| **Failover zu einem sekundären VMM-Standort (mit Netzwerkerstellung)** |Auf der Grundlage der Einstellung, die Sie unter **Logisches Netzwerk** angeben, und den zugehörigen Netzwerkstandorten wird automatisch ein temporäres Testnetzwerk erstellt. |Beim Failover wird geprüft, ob virtuelle Computer erstellt werden. |Verwenden Sie diese Option, wenn für den Wiederherstellungsplan mehr als ein VM-Netzwerk verwendet wird. Im Falle von Netzwerken, die auf der Windows-Netzwerkvirtualisierung basieren, kann diese Option im Netzwerk des virtuellen Replikatcomputers automatisch VM-Netzwerke mit den gleichen Einstellungen (Subnetze und IP-Adresspools) erstellen. Diese VM-Netzwerke werden nach dem Test-Failover automatisch bereinigt.</p><p>Der virtuelle Testcomputer wird auf dem Host erstellt, auf dem sich auch der virtuelle Replikatcomputer befindet. Er wird nicht der Cloud hinzugefügt, in der sich der replizierte virtuelle Computer befindet. |

> [!TIP]
> Die einem virtuellen Computer bei einem Testfailover zugewiesene IP-Adresse ist mit der IP-Adresse identisch, die ihm bei einem geplanten oder nicht geplanten Failover zugewiesen wird (sofern die IP-Adresse im Testfailovernetzwerk verfügbar ist). Wenn im Testfailovernetzwerk nicht dieselbe IP-Adresse verfügbar ist, wird dem virtuellen Computer eine andere IP-Adresse zugewiesen, die im Testfailovernetzwerk verfügbar ist.
>
>


## <a name="test-failover-to-a-production-network-on-recovery-site"></a>Testfailover auf ein Produktionsnetzwerk am Wiederherstellungsstandort
Beim Durchführen eines Testfailovers wird empfohlen, ein anderes Netzwerk als das Site Recovery-Netzwerk für die Produktion zu wählen, das Sie in **Netzwerkzuordnung** angegeben haben. Beachten Sie folgende Punkte, wenn Sie wirklich die End-to-End-Netzwerkkonnektivität auf einem virtuellen Computer bewerten möchten, für den ein Failover durchgeführt wurde:

1. Der primäre virtuelle Computer muss heruntergefahren werden, wenn Sie das Testfailover durchführen. Andernfalls werden zwei virtuelle Computer mit derselben Identität gleichzeitig im selben Netzwerk ausgeführt, was unerwünschte Folgen nach sich ziehen kann.
1. Änderungen, die Sie auf virtuellen Computern mit Testfailover vornehmen, würden verloren gehen, wenn Sie die virtuellen Computer mit Testfailover bereinigen. Diese Änderungen werden nicht wieder auf dem primären virtuellen Computer repliziert.
1. Diese Art des Testens von Leads führt zu einem Ausfall Ihrer Produktionsanwendung. Benutzer der Anwendung sollten aufgefordert werden, die Anwendung während des DR-Drills nicht zu verwenden.  


## <a name="next-steps"></a>Nächste Schritte
Sobald Sie ein Testfailover erfolgreich durchgeführt haben, können Sie ein [Failover](site-recovery-failover.md) versuchen.

