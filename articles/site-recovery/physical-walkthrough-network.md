---
title: "Planen von Netzwerken für die Replikation physischer Server in Azure | Microsoft-Dokumentation"
description: "In diesem Artikel wird das erforderliche Planen von Netzwerken beim Replizieren von physischen Servern zu Azure erläutert."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 71db2435-b5ce-4263-83f6-093d10d1d4e1
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/27/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: 138f04f8e9f0a9a4f71e43e73593b03386e7e5a9
ms.openlocfilehash: f8a20b45b50f71631122e574b634818c1912f12e
ms.contentlocale: de-de
ms.lasthandoff: 06/29/2017


---

# <a name="step-4-plan-networking-for-physical-server-replication-to-azure"></a>Schritt 4: Planen von Netzwerken für die Replikation physischer Server in Azure

Dieser Artikel fasst die Überlegungen zur Netzwerkplanung beim Replizieren von lokalen physischen Servern auf Azure mithilfe des [Azure Site Recovery](site-recovery-overview.md)-Diensts zusammen.

Kommentare können Sie am Ende dieses Artikels senden oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) Fragen stellen.


## <a name="connect-to-replica-azure-vms"></a>Herstellen einer Verbindung mit virtuellen Azure-Replikatcomputern

Bei der Planung der Replikation und Failoverstrategie ist eine der wichtigsten Fragen, wie eine Verbindung zum virtuellen Azure-Computer nach einem Failover hergestellt werden kann. Es gibt verschiedene Auswahlmöglichkeiten beim Entwerfen der Netzwerkstrategie für die virtuellen Azure-Replikatcomputer:

- **Verwenden einer anderen IP-Adresse**: Sie können wählen, einen anderen IP-Adressbereich für das replizierte Netzwerk des virtuellen Azure-Computers zu verwenden. In diesem Szenario erhält der Computer nach einem Failover eine neue IP-Adresse. Dazu ist eine DNS-Aktualisierung erforderlich.
- **Verwenden derselben IP-Adresse**: Sie möchten möglicherweise für das Azure-Netzwerk nach dem Failover denselben IP-Adressbereich verwenden wie an Ihrem lokalen Primärstandort. Das Verwenden derselben IP-Adressen vereinfacht die Wiederherstellung, indem Probleme im Zusammenhang mit dem Netzwerk nach dem Failover reduziert werden. Wenn Sie zu Azure replizieren, müssen Sie Routen jedoch mit dem neuen Speicherort der IP-Adressen nach dem Failover aktualisieren.

## <a name="retain-ip-addresses"></a>IP-Adressen beibehalten

Site Recovery bietet die Möglichkeit, feste IP-Adressen beizubehalten, wenn ein Failover zu Azure mit einem Subnetzfailover erfolgt.
Mit einem Subnetzfailover existiert ein bestimmtes Subnetz an Standort 1 oder 2, jedoch nie gleichzeitig an beiden Standorten. Um den IP-Adressraum im Falle eines Failovers beizubehalten, sorgen Sie programmgesteuert dafür, dass die Routerinfrastruktur die Subnetze von einem Standort an einen anderen verschiebt. Die Subnetze verschieben sich während des Failovers mit den zugeordneten geschützten virtuellen Computern. Der größte Nachteil ist, dass Sie im Fall eines Fehlers das gesamte Subnetz verschieben müssen.

### <a name="failover-example"></a>Failover-Beispiel

Sehen wir uns ein Beispiel für das Failover auf Azure an.

- Ein fiktives Unternehmen, Woodgrove Bank, verfügt über eine lokale Infrastruktur, die deren Geschäftsanwendungen hostet. Ihre mobilen Anwendungen werden in Azure gehostet.
- Konnektivität zwischen virtuellen Computern der Woodgrove Bank in Azure und auf lokalen Servern wird durch eine Standort-zu-Standort-Verbindung (VPN) zwischen dem lokalen Umkreisnetzwerk und dem virtuellen Azure-Netzwerk bereitgestellt.
- Dieses VPN bedeutet, dass das virtuelle Netzwerk des Unternehmens in Azure als Erweiterung seines lokalen Netzwerks erscheint.
- Woodgrove möchte Site Recovery verwenden, um die lokalen Workloads zu Azure zu replizieren.
 - Bei Woodgrove sind Anwendungen und Konfigurationen vorhanden, die von hartcodierten IP-Adressen abhängig sind, daher müssen die IP-Adressen der Anwendungen nach einem Failover zu Azure beibehalten werden.
 - Woodgrove hat seinen Ressourcen, die unter Azure ausgeführt werden, IP-Adressen im Bereich 172.16.1.0/24, 172.16.2.0/24 zugewiesen.


Da Woodgrove in der Lage sein muss, seine Server zu Azure zu replizieren und gleichzeitig die IP-Adressen beizubehalten, muss das Unternehmen folgende Schritte ausführen:

1. Erstellen Sie ein virtuelles Azure-Netzwerk. Dies sollte eine Erweiterung des lokalen Netzwerks sein, sodass für Anwendungen ein nahtloses Failover ausgeführt werden kann.
2. Azure ermöglicht Ihnen das Hinzufügen von Standort-zu-Standort-VPN-Konnektivität und Punkt-zu-Standort-Verbindungen mit den virtuellen Netzwerken, die in Azure erstellt werden.
3. Beim Einrichten der Standort-zu-Standort-Verbindung können Sie im Azure-Netzwerk nur dann Datenverkehr an den lokalen Standort („local-network“ oder „lokales Netzwerk“) weiterleiten, wenn sich der IP-Adressbereich vom lokalen IP-Adressbereich unterscheidet.
    - Der Grund dafür ist, dass Azure gestreckte Subnetze nicht unterstützt. Sie können also im Azure-Netzwerk das lokale Netzwerk „192.168.1.0/24“ nicht hinzufügen, wenn Sie über das lokale Subnetz „192.168.1.0/24“ verfügen.
    - Dies ist zu erwarten, da Azure nicht weiß, dass keine aktiven Computer im Subnetz vorhanden sind und dass das Subnetz nur für die Notfallwiederherstellung erstellt wird.
    - Um einen korrekten Netzwerkdatenverkehr aus einem Azure-Netzwerk zu gewährleisten, dürfen keine Konflikte zwischen den Subnetzen im Netzwerk und dem lokalen Netzwerk vorliegen.

![Vor dem Subnetzfailover](./media/physical-walkthrough-network/network-design7.png)

#### <a name="before-failover"></a>Vor dem Failover

1. Erstellen Sie ein zusätzliches Netzwerk (z.B. ein Netzwerk für die Wiederherstellung). Dies ist das Netzwerk, in dem virtuelle Computer mit Failover erstellt werden.
2. Um sicherzustellen, dass die IP-Adresse für einen Computer nach einem Failover beibehalten wird, navigieren Sie in den Computereigenschaften zu **Konfigurieren**, geben die gleiche IP-Adresse an, die für den Server lokal verwendet wird, und klicken Sie dann auf **Speichern**.
3. Wenn das Failover für den Computer erfolgt, weist ihm Azure Site Recovery die bereitgestellte IP-Adresse zu.
4. Nachdem das Failover ausgelöst wurde und die virtuellen Computer in Azure mit der erforderlichen IP-Adresse erstellt wurden, können Sie mithilfe der [VNET-zu-VNET-Verbindung](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) eine Verbindung mit dem Netzwerk herstellen. Für diese Aktion kann ein Skript erstellt werden.
5. Routen müssen entsprechend geändert werden, um widerzuspiegeln, dass „192.168.1.0/24“ jetzt zu Azure verschoben wurde.

    ![Nach dem Subnetzfailover](./media/physical-walkthrough-network/network-design9.png)

#### <a name="after-failover"></a>Nach dem Failover

Wenn Sie kein Azure-Netzwerk wie das oben dargestellte haben, können Sie nach dem Failover eine Standort-zu-Standort-VPN-Verbindung zwischen Ihrem primären Standort und Azure herstellen.

## <a name="change-ip-addresses"></a>Ändern der IP-Adressen

In diesem [Blogbeitrag](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) wird erläutert, wie die Azure-Netzwerkinfrastruktur festgelegt wird, wenn Sie IP-Adressen nach dem Failover nicht beibehalten müssen. Zunächst wird eine Anwendung beschrieben, danach wird die Erstellung eines lokalen Netzwerks in Azure erklärt, und schließlich finden Sie Informationen zum Ausführen von Failoverszenarien.  

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit [Schritt 5: Vorbereiten von Azure](physical-walkthrough-prepare-azure.md) fort.

