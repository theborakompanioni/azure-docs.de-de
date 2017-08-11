---
title: "Planen von Netzwerken für die Hyper-V-Replikation (ohne System Center VMM) in Azure mithilfe von Azure Site Recovery | Microsoft-Dokumentation"
description: "In diesem Artikel wird das erforderliche Planen von Netzwerken beim Replizieren von virtuellen Hyper-V-Computern zu Azure erläutert."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 5ca12254-975d-48e8-a84d-422825dac9b2
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: 74b75232b4b1c14dbb81151cdab5856a1e4da28c
ms.openlocfilehash: 100b9d8a55c2c163e7a04680f0f7d7963315ee73
ms.contentlocale: de-de
ms.lasthandoff: 07/26/2017

---

# <a name="step-4-plan-networking-for-hyper-v-to-azure-replication"></a>Schritt 4: Planen von Netzwerken für die Replikation von Hyper-V zu Azure

Dieser Artikel fasst die Überlegungen zur Netzwerkplanung beim Replizieren von lokalen virtuellen Hyper-V-Computern (ohne System Center VMM) zu Azure mithilfe des [Azure Site Recovery](site-recovery-overview.md)-Diensts zusammen.

Kommentare können Sie am Ende dieses Artikels senden oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) Fragen stellen.


## <a name="connecting-to-replica-vms-after-failover"></a>Herstellen der Verbindung mit Replikat-VMs nach einem Failover

Bei der Planung der Replikation und Failoverstrategie ist eine der wichtigsten Fragen, wie eine Verbindung zum virtuellen Azure-Computer nach einem Failover hergestellt werden kann. Es gibt verschiedene Auswahlmöglichkeiten beim Entwerfen der Netzwerkstrategie für die virtuellen Azure-Replikatcomputer:

- **Andere IP-Adresse**: Sie können wählen, einen anderen IP-Adressbereich für das replizierte Netzwerk des virtuellen Azure-Computers zu verwenden. In diesem Szenario erhält der virtuelle Computer eine neue IP-Adresse nach einem Failover. Dazu ist ein DNS-Update erforderlich. [Weitere Informationen](site-recovery-test-failover-vmm-to-vmm.md#prepare-the-infrastructure-for-test-failover)
- **Dieselbe IP-Adresse:** Es kann ratsam sein, für das Azure-Netzwerk nach dem Failover denselben IP-Adressbereich wie in Ihrem lokalen primären Netzwerk zu verwenden.  Das Verwenden derselben IP-Adressen vereinfacht die Wiederherstellung, indem Probleme im Zusammenhang mit dem Netzwerk nach dem Failover reduziert werden. Wenn Sie in Azure replizieren, müssen Sie Routen mit dem neuen Speicherort der IP-Adressen nach dem Failover aktualisieren.


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

![Vor dem Subnetzfailover](./media/hyper-v-site-walkthrough-network/network-design7.png)

### <a name="before-failover"></a>Vor dem Failover

1. Erstellen Sie ein zusätzliches Netzwerk (z.B. ein Netzwerk für die Wiederherstellung). Dies ist das Netzwerk, in dem virtuelle Computer mit Failover erstellt werden.
2. Um sicherzustellen, dass die IP-Adresse für einen virtuellen Computer nach einem Failover beibehalten wird, navigieren Sie in den Eigenschaften des virtuellen Computers zu **Konfigurieren**, geben die gleiche IP-Adresse an, die für den virtuellen Computer lokal verwendet wird, und klicken Sie dann auf **Speichern**.
3. Wenn das Failover für den virtuellen Computer erfolgt, weist ihm Azure Site Recovery die bereitgestellte IP-Adresse zu.

    ![Netzwerkeigenschaften](./media/hyper-v-site-walkthrough-network/network-design8.png)

4. Nachdem das Failover ausgelöst wurde und die virtuellen Computer in Azure mit der erforderlichen IP-Adresse erstellt wurden, können Sie mithilfe der [VNET-zu-VNET-Verbindung](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) eine Verbindung mit dem Netzwerk herstellen. Für diese Aktion kann ein Skript erstellt werden.
5. Routen müssen entsprechend geändert werden, um widerzuspiegeln, dass „192.168.1.0/24“ jetzt zu Azure verschoben wurde.

    ![Nach dem Subnetzfailover](./media/hyper-v-site-walkthrough-network/network-design9.png)

### <a name="after-failover"></a>Nach dem Failover

Wenn Sie kein Azure-Netzwerk wie das oben dargestellte haben, können Sie nach dem Failover eine Standort-zu-Standort-VPN-Verbindung zwischen Ihrem primären Standort und Azure herstellen.

## <a name="change-ip-addresses"></a>Ändern der IP-Adressen

In diesem [Blogbeitrag](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) wird erläutert, wie die Azure-Netzwerkinfrastruktur festgelegt wird, wenn Sie IP-Adressen nach dem Failover nicht beibehalten müssen. Es wird mit der Beschreibung einer Anwendung begonnen, danach wird die Erstellung eines lokalen Netzwerks in Azure erklärt, und schließlich finden Sie Informationen zum Ausführen von Failoverszenarios.  

## <a name="next-steps"></a>Nächste Schritte

Gehen Sie zu [Schritt 5: Vorbereiten von Azure](hyper-v-site-walkthrough-prepare-azure.md).

