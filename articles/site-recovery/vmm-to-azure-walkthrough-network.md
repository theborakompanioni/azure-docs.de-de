---
title: "Planen von Netzwerken für die Hyper-V-Replikation (mit System Center VMM) in Azure mithilfe von Azure Site Recovery | Microsoft-Dokumentation"
description: "In diesem Artikel wird das erforderliche Planen von Netzwerken beim Replizieren von virtuellen Hyper-V-Computern (mit VMM) in Azure erläutert."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 41c3c83e-6b1a-496a-8179-498c552ef0c7
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/23/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: 74b75232b4b1c14dbb81151cdab5856a1e4da28c
ms.openlocfilehash: ef87cd82b021e40f0da05142878daff245cd9c62
ms.contentlocale: de-de
ms.lasthandoff: 07/26/2017

---

# <a name="step-4-plan-networking-for-hyper-v-with-vmm-to-azure-replication"></a>Schritt 4: Planen von Netzwerken für die Hyper-V-Replikation (mit VMM) in Azure

Nach dem Durchführen der [Kapazitätsplanung](vmm-to-azure-walkthrough-capacity.md) (bei einer vollständigen Bereitstellung) finden Sie in diesem Artikel Überlegungen zur Netzwerkplanung beim Replizieren von lokalen virtuellen Hyper-V-Computern in System Center Virtual Machine Manager-Clouds (VMM) in Azure mithilfe des [Azure Site Recovery](site-recovery-overview.md)-Diensts.

Kommentare können Sie am Ende dieses Artikels senden oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) Fragen stellen.


## <a name="network-mapping-for-replication-to-azure"></a>Netzwerkzuordnung für die Replikation nach Azure

Die Netzwerkzuordnung wird bei der Replikation von Hyper-V-VMs (die in VMM verwaltet werden) nach Azure verwendet. Die Netzwerkzuordnung erfolgt zwischen VM-Netzwerken auf einem VMM-Quellserver und Azure-Zielnetzwerken. Die Zuordnung ermöglicht Folgendes:

- **Netzwerkverbindung:** Nach einem Failover sind alle replizierten virtuellen Azure-Computer mit dem zugeordneten Azure-Netzwerk verbunden. Alle Computer, auf denen ein Failover im selben Netzwerk ausgeführt wird, können untereinander eine Verbindung herstellen, selbst wenn das Failover in verschiedenen Wiederherstellungsplänen stattgefunden hat.
- **Netzwerkgateway:** Wenn im Azure-Zielnetzwerk ein Netzwerkgateway eingerichtet ist, können die virtuellen Computer eine Verbindung mit anderen lokalen virtuellen Computern im zugeordneten lokalen Netzwerk herstellen.

### <a name="prepare-vmm-for-network-mapping"></a>Vorbereiten von VMM für die Netzwerkzuordnung

Bereiten Sie VMM wie folgt für die Netzwerkzuordnung vor:

1. Sofern dies noch nicht erfolgt ist, bereiten Sie ein [logisches VMM-Netzwerk](https://docs.microsoft.com/system-center/vmm/network-logical) vor, das der Cloud zugeordnet ist, in der sich die Hyper-V-Hosts befinden.
2. Sofern nicht vorhanden, erstellen Sie ein [VM-Netzwerk](https://docs.microsoft.com/system-center/vmm/network-virtual), das mit dem oben vorbereiteten logischen Netzwerk verknüpft ist.
3. Stellen Sie eine Verbindung für die virtuellen Computer auf dem Hyper-V-Hostserver/Cluster in der VMM-Cloud mit dem VM-Netzwerk her.

 
Beachten Sie Folgendes: 
- Neue virtuelle Computer, die dem VM-Quellnetzwerk hinzugefügt werden, werden bei der Replikation mit dem zugeordneten Azure-Netzwerk verbunden.
- Fall das Zielnetzwerk mehrere Subnetze enthält und eines dieser Subnetze den gleichen Namen besitzt wie das Subnetz des virtuellen Quellcomputers, stellt der virtuelle Replikatcomputer nach dem Failover eine Verbindung mit diesem Zielsubnetz her.
- Ist kein Zielsubnetz mit einem übereinstimmenden Namen vorhanden, stellt der virtuelle Computer eine Verbindung mit dem ersten Subnetz im Netzwerk her.
- Beim Bereitstellen des Szenarios bereiten Sie ein Azure-Netzwerk vor und richten Netzwerkzuordnungen ein.

## <a name="connecting-to-azure-vms-after-failover"></a>Herstellen der Verbindung mit virtuellen Azure-Computern nach einem Failover

Bei der Planung der Replikation und Failoverstrategie ist eine der wichtigsten Fragen, wie eine Verbindung zum virtuellen Azure-Computer nach einem Failover hergestellt werden kann. Es gibt verschiedene Auswahlmöglichkeiten beim Entwerfen der Netzwerkstrategie für die virtuellen Azure-Replikatcomputer:

- **Verwenden einer anderen IP-Adresse**: Sie können wählen, einen anderen IP-Adressbereich für das replizierte Netzwerk des virtuellen Azure-Computers zu verwenden. In diesem Szenario erhält der virtuelle Computer eine neue IP-Adresse nach einem Failover. Dazu ist ein DNS-Update erforderlich.
- **Beibehalten derselben IP-Adresse:** Sie möchten möglicherweise für das Azure-Netzwerk nach dem Failover denselben IP-Adressbereich verwenden wie in Ihrem lokalen primären Netzwerk.  Das Verwenden derselben IP-Adressen vereinfacht die Wiederherstellung, indem Probleme im Zusammenhang mit dem Netzwerk nach dem Failover reduziert werden. Wenn Sie in Azure replizieren, müssen Sie Routen mit dem neuen Speicherort der IP-Adressen nach dem Failover aktualisieren.


## <a name="retain-ip-addresses"></a>IP-Adressen beibehalten

Site Recovery bietet die Möglichkeit, feste IP-Adressen beizubehalten, wenn ein Failover in Azure mit einem Subnetzfailover erfolgt.

Mit einem Subnetzfailover existiert ein bestimmtes Subnetz an Standort 1 oder 2, jedoch nie gleichzeitig an beiden Standorten. Um den IP-Adressraum im Falle eines Failovers beizubehalten, sorgen Sie programmgesteuert dafür, dass die Routerinfrastruktur die Subnetze von einem Standort an einen anderen verschiebt. Die Subnetze verschieben sich während des Failovers mit den zugeordneten geschützten virtuellen Computern. Der größte Nachteil ist, dass Sie im Fall eines Fehlers das gesamte Subnetz verschieben müssen.



### <a name="failover-example"></a>Beispiel für ein Failover

Sehen wir uns ein Beispiel für ein Failover auf Azure an.

- Ein fiktives Unternehmen, Woodgrove Bank, verfügt über eine lokale Infrastruktur, die ihre Geschäftsanwendungen hostet. Ihre mobilen Anwendungen werden in Azure gehostet.
- Konnektivität zwischen virtuellen Computern der Woodgrove Bank in Azure und auf lokalen Servern wird durch eine Standort-zu-Standort-Verbindung (VPN) zwischen dem lokalen Umkreisnetzwerk und dem virtuellen Azure-Netzwerk bereitgestellt.
- Dieses VPN bedeutet, dass das virtuelle Netzwerk des Unternehmens in Azure als Erweiterung seines lokalen Netzwerks erscheint.
- Woodgrove möchte Site Recovery verwenden, um die lokalen Workloads zu Azure zu replizieren.
 - Bei Woodgrove sind Anwendungen und Konfigurationen vorhanden, die von hartcodierten IP-Adressen abhängig sind, daher müssen die IP-Adressen der Anwendungen nach einem Failover zu Azure beibehalten werden.
 - Woodgrove hat seinen Ressourcen, die unter Azure ausgeführt werden, IP-Adressen im Bereich 172.16.1.0/24, 172.16.2.0/24 zugewiesen.


Da Woodgrove in der Lage sein muss, seine virtuellen Computer zu Azure zu replizieren und gleichzeitig die IP-Adressen beizubehalten, muss das Unternehmen folgende Schritte ausführen:

1. Erstellen Sie ein virtuelles Azure-Netzwerk. Dies sollte eine Erweiterung des lokalen Netzwerks sein, sodass für Anwendungen ein nahtloses Failover ausgeführt werden kann.
2. Azure ermöglicht Ihnen das Hinzufügen von Standort-zu-Standort-VPN-Konnektivität und Punkt-zu-Standort-Verbindungen mit den virtuellen Netzwerken, die in Azure erstellt werden.
3. Beim Einrichten der Standort-zu-Standort-Verbindung können Sie im Azure-Netzwerk nur dann Datenverkehr an den lokalen Standort („local-network“ oder „lokales Netzwerk“) weiterleiten, wenn sich der IP-Adressbereich vom lokalen IP-Adressbereich unterscheidet.
    - Der Grund dafür ist, dass Azure gestreckte Subnetze nicht unterstützt. Sie können also im Azure-Netzwerk das lokale Netzwerk „192.168.1.0/24“ nicht hinzufügen, wenn Sie über das lokale Subnetz „192.168.1.0/24“ verfügen.
    - Dies ist zu erwarten, da Azure nicht weiß, dass keine aktiven Computer im Subnetz vorhanden sind und dass das Subnetz nur für die Notfallwiederherstellung erstellt wird.
    - Um einen korrekten Netzwerkdatenverkehr aus einem Azure-Netzwerk zu gewährleisten, dürfen keine Konflikte zwischen den Subnetzen im Netzwerk und dem lokalen Netzwerk vorliegen.

![Vor dem Subnetzfailover](./media/vmm-to-azure-walkthrough-network/network-design7.png)

### <a name="before-failover"></a>Vor dem Failover

1. Erstellen Sie ein zusätzliches Netzwerk (z.B. ein Netzwerk für die Wiederherstellung). Dies ist das Netzwerk, in dem virtuelle Computer mit Failover erstellt werden.
2. Um sicherzustellen, dass die IP-Adresse für einen virtuellen Computer nach einem Failover beibehalten wird, navigieren Sie in den Eigenschaften des virtuellen Computers zu **Konfigurieren**, geben die gleiche IP-Adresse an, die für den virtuellen Computer lokal verwendet wird, und klicken Sie dann auf **Speichern**.
3. Wenn das Failover für den virtuellen Computer erfolgt, weist ihm Azure Site Recovery die bereitgestellte IP-Adresse zu.

    ![Netzwerkeigenschaften](./media/vmm-to-azure-walkthrough-network/network-design8.png)

4. Nachdem das Failover ausgelöst wurde und die virtuellen Computer in Azure mit der erforderlichen IP-Adresse erstellt wurden, können Sie mithilfe der [VNET-zu-VNET-Verbindung](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) eine Verbindung mit dem Netzwerk herstellen. Für diese Aktion kann ein Skript erstellt werden.
5. Routen müssen entsprechend geändert werden, um widerzuspiegeln, dass „192.168.1.0/24“ jetzt zu Azure verschoben wurde.

    ![Nach dem Subnetzfailover](./media/vmm-to-azure-walkthrough-network/network-design9.png)

### <a name="after-failover"></a>Nach dem Failover

Wenn Sie kein Azure-Netzwerk wie das oben dargestellte haben, können Sie nach dem Failover eine Standort-zu-Standort-VPN-Verbindung zwischen Ihrem primären Standort und Azure herstellen.

## <a name="change-ip-addresses"></a>Ändern der IP-Adressen

In diesem [Blogbeitrag](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) wird erläutert, wie die Azure-Netzwerkinfrastruktur festgelegt wird, wenn Sie IP-Adressen nach dem Failover nicht beibehalten müssen. Es wird mit der Beschreibung einer Anwendung begonnen, danach wird die Erstellung eines lokalen Netzwerks in Azure erklärt, und schließlich finden Sie Informationen zum Ausführen von Failoverszenarios.  

## <a name="next-steps"></a>Nächste Schritte

Gehen Sie zu [Schritt 5: Vorbereiten von Azure](vmm-to-azure-walkthrough-prepare-azure.md).

