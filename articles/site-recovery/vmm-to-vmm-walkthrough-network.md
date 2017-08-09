---
title: "Planen des Netzwerks für die Hyper-V-Replikation an einem sekundären VMM-Standort mit Azure Site Recovery | Microsoft-Dokumentation"
description: "Dieser Artikel erläutert, wie Sie das Netzwerk planen, wenn Sie virtuelle Hyper-V-Computer mit Azure Site Recovery an einem sekundären System Center VMM-Standort replizieren."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 095f2d73-994e-4fc0-96f2-e03a7d72e492
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: fff84ee45818e4699df380e1536f71b2a4003c71
ms.openlocfilehash: a1f3f6e6cba074647195e2b0cbcdc7b4f3dec475
ms.contentlocale: de-de
ms.lasthandoff: 08/01/2017

---
# <a name="step-3-plan-networking-for-hyper-v-vm-replication-to-a-secondary-vmm-site"></a>Schritt 3: Planen des Netzwerks für die Replikation virtueller Hyper-V-Computer an einem sekundären VMM-Standort

Nachdem Sie die Voraussetzungen für die Bereitstellung überprüft haben, lesen Sie diesen Artikel, um das Netzwerk zu planen, das zum Replizieren von in System Center Virtual Machine Manager-Clouds (VMM) verwalteten Hyper-V-VMs an einem sekundären Standort mithilfe von [Azure Site Recovery](site-recovery-overview.md) im Azure-Portal erforderlich ist. 

Kommentare können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) hinterlassen.

## <a name="network-mapping-overview"></a>Übersicht über die Netzwerkzuordnung

Die Netzwerkzuordnung wird bei der Replikation von Hyper-V-VMs (die in VMM verwaltet werden) in einem sekundären Rechenzentrum verwendet. Die Netzwerkzuordnung erfolgt zwischen VM-Netzwerken auf einem VMM-Quellserver und VM-Netzwerken auf einem VMM-Zielserver. Die Zuordnung ermöglicht Folgendes:

- **Netzwerkverbindung** – Verbindet VMs nach einem Failover mit den entsprechenden Netzwerken. Die Replikat-VM wird mit dem Zielnetzwerk verbunden, das dem Quellnetzwerk zugeordnet ist.
- **Optimale Platzierung** – Platziert die Replikat-VMs optimal auf Hyper-V-Hostservern. Replikat-VMs werden auf Hosts platziert, die auf die zugeordneten VM-Netzwerke zugreifen können.
- **Keine Netzwerkzuordnung** – Wenn Sie keine Netzwerkzuordnung konfigurieren, werden Replikat-VMs nach dem Failover nicht mit VM-Netzwerken verbunden.


### <a name="example"></a>Beispiel

Im Anschluss folgt ein Beispiel zur Veranschaulichung dieses Mechanismus. Für das Beispiel verwenden wir eine Organisation mit zwei Standorten in New York und Chicago.

**Standort** | **VMM-Server** | **VM-Netzwerke** | **Zuordnung**
---|---|---|---
New York | VMM-NewYork| VMNetwork1-NewYork | Zugeordnet zu „VMNetwork1-Chicago“
 |  | VMNetwork2-NewYork | Nicht zugeordnet
Chicago | VMM-Chicago| VMNetwork1-Chicago | Zugeordnet zu „VMNetwork1-NewYork“
 | | VMNetwork1-Chicago | Nicht zugeordnet

In diesem Beispiel:

- Wenn für einen virtuellen, mit „VMNetwork1-NewYork“ verbundenen Computer ein virtueller Replikatcomputer erstellt wird, wird dieser mit „VMNetwork1-Chicago“ verbunden.
- Ein für „VMNetwork2-NewYork“ oder „VMNetwork2-Chicago“ erstellter virtueller Replikatcomputer wird mit keinem Netzwerk verbunden.

Im Anschluss finden Sie Informationen zur Einrichtung der VMM-Clouds für unser Beispielunternehmen sowie zu den logischen Netzwerken, die den Clouds zugeordnet sind.

#### <a name="cloud-protection-settings"></a>Cloudschutzeinstellungen

**Geschützte Cloud** | **Schützende Cloud** | **Logisches Netzwerk (New York)**  
---|---|---
GoldCloud1 | GoldCloud2 |
SilverCloud1| SilverCloud2 |
GoldCloud2 | <p>NA</p><p></p> | <p>LogicalNetwork1-NewYork</p><p>LogicalNetwork1-Chicago</p>
SilverCloud2 | <p>NA</p><p></p> | <p>LogicalNetwork1-NewYork</p><p>LogicalNetwork1-Chicago</p>

#### <a name="logical-and-vm-network-settings"></a>Einstellungen für das logische und das VM-basierte Netzwerk

**Standort** | **Logisches Netzwerk** | **Zugeordnetes VM-Netzwerk**
---|---|---
New York | LogicalNetwork1-NewYork | VMNetwork1-NewYork
Chicago | LogicalNetwork1-Chicago | VMNetwork1-Chicago
 | LogicalNetwork2-Chicago | VMNetwork2-Chicago

#### <a name="target-network-settings"></a>Zielnetzwerkeinstellungen

Die folgende Tabelle zeigt die Optionen, die zur Verfügung stehen, wenn Sie bei Verwendung dieser Einstellungen das VM-Zielnetzwerk auswählen:

**Auswahl** | **Geschützte Cloud** | **Schützende Cloud** | **Verfügbarkeit des Zielnetzwerks**
---|---|---|---
VMNetwork1-Chicago | SilverCloud1 | SilverCloud2 | Verfügbar
 | GoldCloud1 | GoldCloud2 | Verfügbar
VMNetwork2-Chicago | SilverCloud1 | SilverCloud2 | Nicht verfügbar
 | GoldCloud1 | GoldCloud2 | Verfügbar


Wenn das Zielnetzwerk mehrere Subnetze enthält und eines dieser Subnetze den gleichen Namen besitzt wie das Subnetz des virtuellen Quellcomputers, wird der virtuelle Replikatcomputer nach dem Failover mit diesem Zielsubnetz verbunden. Gibt es kein Zielsubnetz mit einem übereinstimmenden Namen, wird der virtuelle Computer mit dem ersten Subnetz im Netzwerk verbunden.


#### <a name="failback-behavior"></a>Failbackverhalten

Zur Veranschaulichung der Vorgänge bei einem Failback (umgekehrte Replikation) nehmen wir an, dass zwischen „VMNetwork1-NewYork“ und „VMNetwork1-Chicago“ eine Zuordnung mit den folgenden Einstellungen besteht:


**Virtueller Computer** | **Verbundenes VM-Netzwerk**
---|---
VM1 | VMNetwork1-NewYork
VM2 (Replikat von VM1) | VMNetwork1-Chicago

Einige mögliche Szenarien mit diesen Einstellungen:

**Szenario** | **Ergebnis**
---|---
Keine Änderung der Netzwerkeigenschaften von VM-2 nach dem Failover | VM-1 bleibt mit dem Quellnetzwerk verbunden.
Änderung der Netzwerkeigenschaften von VM-2 nach dem Failover und Trennung der Verbindung | VM-1 wird getrennt.
Änderung der Netzwerkeigenschaften von VM-2 nach dem Failover und Verbindung mit "VMNetwork2-Chicago" | Wenn "VMNetwork2-Chicago" nicht zugeordnet ist, wird VM-1 getrennt.
Änderung der Netzwerkzuordnung von "VMNetwork1-Chicago" | VM-1 wird mit dem Netzwerk verbunden, das jetzt "VMNetwork1-Chicago" zugeordnet ist.



## <a name="prepare-for-network-mapping"></a>Bereiten Sie die Netzwerkzuordnung vor

1. Auf den VMM-Quell- und -Zielservern ist ein logisches Netzwerk erforderlich, das der Quell- und der Zielcloud zugeordnet ist. 
2. Auf den Quell- und Zielservern muss ein VM-Netzwerk vorhanden sein, das mit dem logischen Netzwerk verknüpft ist.
3. VMs auf Hyper-V-Hosts im Quellstandort müssen mit dem VM-Quellnetzwerk verknüpft sein. Wenn Sie nur einen einzigen VMM-Server verwenden, können Sie die Zuordnung zwischen den VM-Netzwerken auf dem gleichen Server konfigurieren.

Wenn Sie die Netzwerkzuordnung während der Site Recovery-Bereitstellung einrichten, geschieht Folgendes:

- Wenn Sie die Netzwerkzuordnung einrichten und ein VM-Zielnetzwerk auswählen, werden die VMM-Quellclouds, die das VM-Quellnetzwerk verwenden, ebenso angezeigt wie die verfügbaren VM-Zielnetzwerke in den Zielclouds.
- - Bei richtig konfigurierter Zuordnung und aktivierter Replikation wird eine Quell-VM mit ihrem VM-Quellnetzwerk verbunden, und das entsprechende Replikat am Zielstandort wird mit dem zugeordneten VM-Netzwerk verbunden.
- Wenn das Zielnetzwerk mehrere Subnetze enthält und eines dieser Subnetze den gleichen Namen besitzt wie das Subnetz des virtuellen Quellcomputers, wird der virtuelle Replikatcomputer nach dem Failover mit diesem Zielsubnetz verbunden. Gibt es kein Zielsubnetz mit einem übereinstimmenden Namen, wird der virtuelle Computer mit dem ersten Subnetz im Netzwerk verbunden.

## <a name="connect-to-vms-after-failover"></a>Herstellen einer Verbindung mit VMs nach einem Failover

Bei der Planung der Replikations- und Failoverstrategie lautet eine der wichtigsten Fragen, wie nach einem Failover die Verbindung zum Replikat hergestellt wird. Es gibt verschiedene Optionen: 

- **Verwenden einer anderen IP-Adresse**: Sie können eine andere IP-Adresse für den replizierten virtuellen Computer verwenden. In diesem Szenario erhält der virtuelle Computer eine neue IP-Adresse nach einem Failover. Dazu ist ein DNS-Update erforderlich.
- **Beibehalten der gleichen IP-Adresse**: Möglicherweise möchten Sie für die Replikat-VM die gleiche IP-Adresse verwenden. Das Verwenden derselben IP-Adressen vereinfacht die Wiederherstellung, indem Probleme im Zusammenhang mit dem Netzwerk nach dem Failover reduziert werden. 

## <a name="retain-ip-addresses"></a>IP-Adressen beibehalten

Wenn Sie nach dem Failover an den sekundären Standort die IP-Adressen aus dem primären Standort beibehalten möchten, können Sie ein vollständiges Subnetzfailover durchführen. Anschließend aktualisieren Sie die Routen, um den neuen Standort der IP-Adressen anzugeben, oder stellen alternativ dazu ein gestrecktes Subnetz zwischen dem primären und dem Wiederherstellungsstandort bereit.

### <a name="stretched-subnet"></a>Gestrecktes Subnetz

In einem gestreckten Subnetz ist das Subnetz gleichzeitig am primären und am sekundären Standort verfügbar. Wenn Sie einen Server und dessen IP-Konfiguration (Layer 3) an den sekundären Standort verschieben, leitet das Netzwerk den Datenverkehr automatisch an den neuen Standort weiter. 

Aus Layer-2-Perspektive (Sicherungsschicht) benötigen Sie Netzwerkkomponenten, die ein gestrecktes VLAN verarbeiten können. Darüber hinaus weitet sich die potenzielle Fehlerdomäne durch Strecken des VLANs auf beide Standorte aus und wird im Grunde zu einer einzigen Fehlerquelle. Es ist zwar unwahrscheinlich, aber es könnte passieren, dass ein Broadcast-Storm gestartet wird und nicht isoliert werden kann. 


### <a name="subnet-failover"></a>Subnetzfailover

Sie können ein Subnetzfailover durchführen, um von den Vorteilen eines gestreckten Subnetzes zu profitieren, ohne das Subnetz tatsächlich zu strecken. In dieser Lösung ist ein Subnetz am Quell- oder Zielstandort verfügbar, aber nicht an beiden Standorten gleichzeitig. Um den IP-Adressraum im Falle eines Failovers beizubehalten, können Sie programmgesteuert dafür sorgen, dass die Routerinfrastruktur die Subnetze von einem Standort an einen anderen verschiebt. Nach dem Failover werden die Subnetze mit den zugehörigen virtuellen Computern verschoben. Der größte Nachteil ist, dass Sie im Fall eines Fehlers das gesamte Subnetz verschieben müssen.

### <a name="example"></a>Beispiel

Hier finden Sie ein Beispiel für ein vollständiges Subnetzfailover. Der primäre Standort verfügt über Anwendungen, die im Subnetz 192.168.1.0/24 ausgeführt werden. Beim Failover werden alle VMs in diesem Subnetz an den sekundären Standort verlagert und behalten Ihre IP-Adressen. Die Routen müssen geändert werden, um die Tatsache widerzuspiegeln, dass alle virtuellen Computer, die zum Subnetz 192.168.1.0/24 gehören, jetzt an den sekundären Standort verschoben wurden.

Die folgende Abbildung zeigt die Subnetze vor und nach dem Failover:

- Subnetz 192.168.0.1/24 ist vor dem Failover am Quellstandort aktiv und wird nach dem Failover am sekundären Standort aktiv.
- Die Routen zwischen primärem Standort und Wiederherstellungsstandort, drittem Standort und primärem Standort sowie drittem Standort und Wiederherstellungsstandort müssen entsprechend geändert werden.

**Vor dem Failover**

![Vor dem Failover](./media/vmm-to-vmm-walkthrough-network/network-design2.png)

**Nach dem Failover**

![Nach dem Failover](./media/vmm-to-vmm-walkthrough-network/network-design3.png)

Nach dem Failover geschieht Folgendes:

- Site Recovery weist jeder Netzwerkschnittstelle auf dem virtuellen Computer eine IP-Adresse aus dem statischen IP-Adresspool zu, der im relevanten Netzwerk für jede VMM-Instanz definiert ist.
- Wenn der IP-Adresspool am sekundären Standort der gleiche Adresspool ist wie am Quellstandort, weist Site Recovery der Replikat-VM die gleiche IP-Adresse (der Quell-VM) zu. Die IP-Adresse ist in VMM reserviert, aber auf dem Hyper-V-Host nicht als Failover IP-Adresse festgelegt. Die Failover-IP-Adresse auf einem Hyper-V-Host wird erst unmittelbar vor dem Failover festgelegt.
- Wenn die gleiche IP-Adresse nicht verfügbar ist, ordnet Site Recovery eine andere verfügbare Adresse aus dem Pool zu.
- Wenn virtuelle Computer DHCP verwenden, verwaltet Site Recovery die IP-Adressen nicht. Sie müssen überprüfen, ob der DHCP-Server am sekundären Standort Adressen aus dem gleichen Bereich zuordnen kann wie am Quellstandort.

### <a name="validate-the-ip-address"></a>Überprüfen der IP-Adresse

Nachdem Sie den Schutz für einen virtuellen Computer aktiviert haben, können Sie das folgende Beispielskript verwenden, um die Adresse zu überprüfen, die dem virtuellen Computer zugewiesen wurde. Die gleiche IP-Adresse wird als Failover-IP-Adresse festgelegt und dem virtuellen Computer zum Zeitpunkt der Failovers zugewiesen:

    ```
    $vm = Get-SCVirtualMachine -Name <VM_NAME>
    $na = $vm[0].VirtualNetworkAdapters>
    $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
    $ip.address 
    ```

## <a name="changing-ip-addresses"></a>Ändern von IP-Adressen

In diesem Szenario werden die IP-Adressen von VMs, für die ein Failover durchgeführt wird, geändert. Der Nachteil dieser Lösung liegt im erforderlichen Wartungsaufwand. In der Regel wird DNS nach dem Start von Replikat-VMs aktualisiert. Um Verwirrungen im Netzwerk zu vermeiden, müssen möglicherweise DNS-Einträge geändert und zwischengespeicherten Einträge aktualisiert werden. Dies kann zu Ausfallzeiten führen. Solche Ausfallzeiten lassen sich folgendermaßen minimieren:

- Verwenden Sie niedrige TTL-Werte für Intranetanwendungen.
- Verwenden Sie das folgende Skript in einem Site Recovery-Wiederherstellungsplan, um den DNS-Server rechtzeitig zu aktualisieren. Wenn Sie die dynamische DNS-Registrierung verwenden, benötigen Sie das Skript nicht.

    ```
    param(
    string]$Zone,
    [string]$name,
    [string]$IP
    )
    $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
    $newrecord = $record.clone()
    $newrecord.RecordData[0].IPv4Address  =  $IP
    Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord
    ```
    
### <a name="example"></a>Beispiel 

Sehen wir uns ein Szenario an, in dem Sie verschiedene IP-Adressen für den primären und den Wiederherstellungsstandort verwenden möchten. In diesem Beispiel sind am primären und am sekundären Standort verschiedene IP-Adressen vorhanden, und es gibt einen dritten Standort, von dem aus auf Anwendungen zugegriffen werden kann, die am primären oder am Wiederherstellungsstandort gehostet werden.

- Vor dem Failover werden Apps am primären Standort im Subnetz 192.168.1.0/24 gehostet und sind so konfiguriert, dass sie sich nach einem Failover am sekundären Standort im Subnetz 172.16.1.0/24 befinden.
- VPN-Verbindungen/Netzwerkrouten wurden entsprechend konfiguriert, damit alle drei Standorte aufeinander zugreifen können.
- Nach dem Failover werden Apps im Wiederherstellungssubnetz wiederhergestellt. In diesem Szenario ist kein Failover des gesamten Subnetzes erforderlich, und es müssen keine Änderungen vorgenommen werden, um VPN- oder Netzwerkrouten neu zu konfigurieren. Mit einem Failover und einigen DNS-Updates wird sichergestellt, dass der Zugriff auf Anwendungen nicht unterbrochen wird.
- Wenn der DNS so konfiguriert ist, dass er dynamische Updates zulässt, registrieren die virtuellen Computer sich unter Verwendung der neuen IP-Adresse selbst, sobald sie nach einem Failover gestartet werden.

**Vor dem Failover**

![Andere IP-Adresse – vor dem Failover](./media/vmm-to-vmm-walkthrough-network/network-design10.png)

**Nach dem Failover**

![Andere IP-Adresse – nach dem Failover](./media/vmm-to-vmm-walkthrough-network/network-design11.png)



## <a name="next-steps"></a>Nächste Schritte

Gehen Sie zu [Schritt 4: Vorbereiten von VMM und Hyper-V](vmm-to-vmm-walkthrough-vmm-hyper-v.md).



