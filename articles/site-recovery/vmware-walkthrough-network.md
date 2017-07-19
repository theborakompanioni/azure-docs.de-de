---
title: "Planen von Netzwerken für die Replikation von VMware in Azure | Microsoft-Dokumentation"
description: "In diesem Artikel wird das erforderliche Planen von Netzwerken beim Replizieren von virtuellen VMware-Computern in Azure erläutert"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 5578a0b4-0352-41c3-9cce-56beb17a4d97
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/27/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: 138f04f8e9f0a9a4f71e43e73593b03386e7e5a9
ms.openlocfilehash: f164ac68ba6ec650bb3996b4aa870e1b98533a23
ms.contentlocale: de-de
ms.lasthandoff: 06/29/2017


---

# <a name="step-4-plan-networking-for-vmware-to-azure-replication"></a>Schritt 4: Planen von Netzwerken für die Replikation von VMware in Azure

Dieser Artikel fasst Überlegungen zur Netzwerkplanung beim Replizieren von virtuellen VMware-Computern in Azure mithilfe des [Azure Site Recovery](site-recovery-overview.md)-Diensts zusammen.

Kommentare können Sie am Ende dieses Artikels senden oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) Fragen stellen.


## <a name="connect-to-replica-vms"></a>Herstellen einer Verbindung mit Replikat-VMs

Bei der Planung der Replikation und Failoverstrategie ist eine der wichtigsten Fragen, wie eine Verbindung zum virtuellen Azure-Computer nach einem Failover hergestellt werden kann. Es gibt verschiedene Auswahlmöglichkeiten beim Entwerfen der Netzwerkstrategie für die virtuellen Azure-Replikatcomputer.

- **Verwenden einer anderen IP-Adresse**: Sie können sich entscheiden, einen anderen IP-Adressbereich für das replizierte Netzwerk des virtuellen Azure-Computers zu verwenden. In diesem Szenario erhält der virtuelle Computer eine neue IP-Adresse nach einem Failover. Dazu ist eine DNS-Aktualisierung erforderlich.
- **Behalten derselben IP-Adresse**: Sie möchten möglicherweise für das Azure-Netzwerk nach dem Failover denselben IP-Adressbereich verwenden, der sich an Ihrem lokalen Primärstandort befindet. Das Verwenden derselben IP-Adressen vereinfacht die Wiederherstellung, indem Probleme im Zusammenhang mit dem Netzwerk nach dem Failover reduziert werden. Wenn Sie in Azure replizieren, müssen Sie Routen mit dem neuen Speicherort der IP-Adressen nach dem Failover aktualisieren. 


## <a name="retain-ip-addresses"></a>IP-Adressen beibehalten

Site Recovery bietet die Möglichkeit, feste IP-Adressen beizubehalten, wenn ein Failover in Azure mit einem Subnetzfailover erfolgt.

Mit einem Subnetzfailover existiert ein bestimmtes Subnetz an Standort 1 oder 2, jedoch nie gleichzeitig an beiden Standorten. Um den IP-Adressraum im Falle eines Failovers beizubehalten, sorgen Sie programmgesteuert, dass die Routerinfrastruktur die Subnetze von einem Standort an einen anderen verschiebt. Die Subnetze verschieben sich während des Failovers mit den zugeordneten geschützten VMs. Der größte Nachteil ist, dass Sie im Fall eines Fehlers das gesamte Subnetz verschieben müssen.


### <a name="failover-example"></a>Beispiel für ein Failover

Sehen wir uns ein Beispiel für ein Failover auf Azure an.

- Ein fiktives Unternehmen, Woodgrove Bank, verfügt über eine lokale Infrastruktur, die ihre Geschäftsanwendungen hostet. Ihre mobilen Anwendungen werden in Azure gehostet.
- Konnektivität zwischen der Woodgrove Bank-VMs in Azure und auf lokalen Servern wird durch eine Standort-zu-Standort-Verbindung (VPN) zwischen dem lokalen Umkreisnetzwerk und dem virtuellen Azure-Netzwerk bereitgestellt.
- Dieses VPN bedeutet, dass das virtuelle Netzwerk der Firma in Azure als Erweiterung des lokalen Netzwerks des Unternehmens erscheint.
- Die Woodgrove Bank möchte Site Recovery verwenden, um die lokalen Workloads zu Azure zu replizieren.
 - Bei der Woodgrove Bank sind Anwendungen und Konfigurationen vorhanden, die von hartcodierten IP-Adressen abhängig sind, daher müssen die IP-Adressen der Anwendungen nach einem Failover zu Azure beibehalten werden.
 - Woodgrove hat IP-Adressen vom Bereich 172.16.1.0/24, 172.16.2.0/24 seinen Ressourcen zugewiesen, die unter Azure ausgeführt werden.


Da die Woodgrove Bank in der Lage sein muss, die virtuellen Computer zu Azure zu replizieren, während gleichzeitig die IP-Adressen beibehalten werden, muss das Unternehmen Folgendes tun:

1. Erstellen Sie ein virtuelles Azure-Netzwerk. Dies sollte eine Erweiterung des lokalen Netzwerks sein, so dass für Anwendungen ein nahtloses Failover ausgeführt werden kann.
2. Azure ermöglicht Ihnen das Hinzufügen von Standort-zu-Standort-VPN-Konnektivität und Punkt-zu-Standort-Verbindungen mit den virtuellen Netzwerken, die in Azure erstellt werden.
3. Beim Einrichten der Standort-zu-Standort-Verbindung können Sie im Azure-Netzwerk nur dann Datenverkehr an den lokalen Standort („local-network“ oder „lokales Netzwerk“) weiterleiten, wenn der IP-Adressbereich sich vom lokalen IP-Adressbereich unterscheidet.
    - Der Grund dafür ist, dass Azure gestreckte Subnetze nicht unterstützt. Sie können also im Azure-Netzwerk kein lokales Netzwerk 192.168.1.0/24 hinzufügen, wenn Sie über ein lokales Subnetz 192.168.1.0/24 verfügen.
    - Der Grund hierfür liegt darin, dass Azure nicht weiß, dass keine aktiven virtuellen Computer im Subnetz vorhanden sind und dass das Subnetz nur für die Notfallwiederherstellung erstellt wird.
    - Um einen korrekten Netzwerkdatenverkehr aus einem Azure-Netzwerk zu gewährleisten, dürfen keine Konflikte zwischen den Subnetzen im Netzwerk und dem lokalen Netzwerk vorliegen.

![Vor dem Subnetzfailover](./media/site-recovery-network-design/network-design7.png)

### <a name="before-failover"></a>Vor dem Failover

1. Erstellen Sie ein zusätzliches Netzwerk (z.B. ein Netzwerk für die Wiederherstellung). Dies ist das Netzwerk, in dem virtuelle Computer mit Failover erstellt werden.
2. Um sicherzustellen, dass die IP-Adresse für einen virtuellen Computer nach einem Failover beibehalten wird, navigieren Sie in den VM-Eigenschaften zu **Konfigurieren**, geben Sie die gleiche IP-Adresse an, die für den virtuellen Computer lokal verwendet wird, und klicken Sie dann auf **Speichern**.
3. Wenn das Failover für den virtuellen Computer erfolgt, weist Azure Site Recovery die bereitgestellte IP-Adresse diesem zu.

    ![Netzwerkeigenschaften](./media/site-recovery-network-design/network-design8.png)

4. Nachdem das Failover ausgelöst wurde, und die virtuellen Computer in Azure mithilfe der erforderlichen IP-Adresse erstellt wurden, können Sie mithilfe der [VNet-zu-VNet-Verbindung](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) eine Verbindung mit dem Netzwerk herstellen. Dadurch kann ein Skript erstellt werden.
5. Routen müssen entsprechend geändert werden, um widerzuspiegeln, dass 192.168.1.0/24 jetzt zu Azure verschoben wurde.

    ![Nach dem Subnetzfailover](./media/site-recovery-network-design/network-design9.png)

### <a name="after-failover"></a>Nach dem Failover

Wenn Sie kein Azure-Netzwerk wie oben haben, können Sie nach dem Failover eine Standort-zu-Standort-VPN-Verbindung zwischen Ihrem primären Standort und Azure herstellen.

## <a name="change-ip-addresses"></a>Ändern der IP-Adressen

In diesem [Blogbeitrag](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) wird erläutert, wie die Azure-Netzwerkinfrastruktur festgelegt wird, wenn Sie IP-Adressen nach dem Failover nicht beibehalten müssen. Es wird mit der Beschreibung einer Anwendung begonnen, danach wird die Erstellung eines lokalen Netzwerks in Azure erklärt, und schließlich finden Sie Informationen zum Ausführen von Failoverszenarios.  

## <a name="next-steps"></a>Nächste Schritte

Gehen Sie zu [Schritt 5: Vorbereiten von Azure](vmware-walkthrough-prepare-azure.md).

