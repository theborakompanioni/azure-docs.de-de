---
title: "Zuordnen von Speicher in Azure Site Recovery für die Replikation virtueller Hyper-V-Computer zwischen lokalen Rechenzentren | Microsoft Docs"
description: "Vorbereiten der Speicherzuordnung für die Replikation virtueller Hyper-V-Computer zwischen zwei lokalen Rechenzentren mithilfe von Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 20e14e9c-87c4-4ad3-aa95-441697b1fc43
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/01/2016
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 1f21c41374e2c09a6b9706df2e084123de035f1a


---
# <a name="prepare-storage-mapping-for-hyper-v-virtual-machine-replication-between-two-on-premises-datacenters-with-azure-site-recovery"></a>Vorbereiten der Speicherzuordnung für die Replikation virtueller Hyper-V-Computer zwischen zwei lokalen Rechenzentren mithilfe von Azure Site Recovery
Azure Site Recovery unterstützt Ihre Strategie für Geschäftskontinuität und Notfallwiederherstellung, indem Replikation, Failover und Wiederherstellung virtueller Computer und physischer Server aufeinander abgestimmt werden. In diesem Artikel wird die Netzwerkzuordnung beschrieben, die zur optimalen Nutzung von Speicher diente, wenn Sie Site Recovery zum Replizieren virtueller Hyper-V-Computer zwischen zwei lokalen VMM-Rechenzentren verwenden.

Kommentare oder Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)veröffentlichen.

## <a name="overview"></a>Übersicht
Die Speicherzuordnung ist nur relevant, wenn Sie virtuelle Hyper-V-Computer in VMM-Clouds mithilfe der Hyper-V-Replikat- oder SAN-Replikation aus einem primären Rechenzentrum in ein sekundäres Rechenzentrum wie folgt replizieren:

* **Lokal zu lokal (Replikation mit Hyper-V-Replikat)**: Sie richten die Speicherzuordnung ein, indem Sie einem Quell- und Ziel-VMM-Server Speicherklassifizierungen für die folgenden Zwecke hinzufügen:
  
  * **Angabe des Zielspeichers für virtuelle Replikatcomputer**: Virtuelle Computer werden zu einem Speicherziel Ihrer Wahl repliziert (SMB-Freigabe oder freigegebene Clustervolumes (CSVs)).
  * **Platzierung virtueller Replikatcomputer**: Virtuelle Replikatcomputer werden durch die Speicherzuordnung optimal auf Hyper-V-Hostservern platziert. Virtuelle Replikatcomputer werden auf Hosts platziert, die auf die zugeordnete Speicherklassifizierung zugreifen können.
  * **Keine Speicherzuordnung**: Wenn Sie keine Speicherzuordnung konfigurieren, werden virtuelle Computer zum Standardspeicherort auf dem Hyper-V-Hostserver repliziert, der dem virtuellen Replikatcomputer zugeordnet ist.
* **Lokal zu lokal (Replikation mit SAN)**: Sie richten die Speicherzuordnung ein, indem Sie einem Quell- und Ziel-VMM-Server Speicherarraypools hinzufügen.
  
  * **Angabe von Pools**: Dient zum Angeben, welcher sekundäre Speicherpool Replikationsdaten aus dem primären Pool empfängt.
  * **Angabe von Zielspeicherpools**: Stellt sicher, dass LUNs in einer Replikationsgruppe in den zugeordneten Zielspeicherpool Ihrer Wahl repliziert werden.

## <a name="set-up-storage-classifications-for-hyper-v-replication"></a>Einrichten von Speicherklassifizierungen für die Hyper-V-Replikation
Wenn Sie Hyper-V-Replikate für die Replikation mit Site Recovery verwenden, erfolgt die Zuordnung zwischen Speicherklassifizierungen auf dem VMM-Quell- und -Zielserver (oder auf einem einzelnen VMM-Server, wenn zwei Standorte vom gleichen VMM-Server verwaltet werden). Beachten Sie Folgendes:

* Bei ordnungsgemäß konfigurierter Zuordnung und aktivierter Replikation wird die virtuelle Festplatte eines virtuellen Computers am primären Standort zum Speicher am zugeordneten Zielspeicherort repliziert.
* Speicherklassifizierungen müssen den Hostgruppen in der Quell- und Zielcloud zur Verfügung stehen.
* Klassifizierungen müssen nicht den gleichen Speichertyp aufweisen. So können Sie beispielsweise eine Quellklassifizierung mit SMB-Freigaben einer Zielklassifizierung mit CSVs zuordnen.
* Weitere Informationen finden Sie unter [Erstellen von Speicher-Klassifizierungen in VMM](https://technet.microsoft.com/library/gg610685.aspx).

## <a name="example"></a>Beispiel
Sind die Klassifizierungen in VMM ordnungsgemäß konfiguriert, werden die Quell- und Zielklassifizierungen angezeigt, wenn Sie bei der Speicherzuordnung den VMM-Quell- und -Zielserver auswählen. Im Anschluss folgt ein Beispiel für die Speicherdateifreigaben und -klassifizierungen für eine Organisation mit zwei Standorten in New York und Chicago:

| **Standort** | **VMM-Server** | **Dateifreigabe (Quelle)** | **Klassifizierung (Quelle)** | **Zuordnungsziel** | **Dateifreigabe (Ziel)** |
| --- | --- | --- | --- | --- | --- |
| New York |VMM_Source |SourceShare1 |GOLD |GOLD_TARGET |TargetShare1 |
| SourceShare2 |SILVER |SILVER_TARGET |TargetShare2 | | |
| SourceShare3 |BRONZE |BRONZE_TARGET |TargetShare3 | | |
| Chicago |VMM_Target | |GOLD_TARGET |Nicht zugeordnet | |
|  |SILVER_TARGET |Nicht zugeordnet | | | |
|  |BRONZE_TARGET |Nicht zugeordnet | | | |

Diese Einstellungen werden im Site Recovery-Portal auf der Registerkarte **Serverspeicher** der Seite **Ressourcen** konfiguriert.

![Konfigurieren der Speicherzuordnung](./media/site-recovery-storage-mapping/storage-mapping1.png)

In diesem Beispiel gilt Folgendes:

* Wenn für einen virtuellen Computer im GOLD-Speicher (SourceShare1) ein virtueller Replikatcomputer erstellt wird, wird dieser zu einem GOLD_TARGET-Speicher (TargetShare1) repliziert.
* Wenn für einen virtuellen Computer im SILVER-Speicher (SourceShare2) ein virtueller Replikatcomputer erstellt wird, wird er zu einem SILVER_TARGET-Speicher (TargetShare2) repliziert, usw.

Die tatsächlichen Dateifreigaben und deren zugewiesene Klassifizierungen in VMM werden im nächsten Screenshot dargestellt.

![Speicherklassifizierungen in VMM](./media/site-recovery-storage-mapping/storage-mapping2.png)

## <a name="multiple-storage-locations"></a>Mehrere Speicherorte
Wenn die Zielklassifizierung mehreren SMB-Freigaben oder freigegebenen Clustervolumes zugewiesen und der virtuelle Computer geschützt ist, wird der optimale Speicherort automatisch ausgewählt. Ist kein passender Zielspeicher mit der angegebenen Klassifizierung verfügbar, wird der Standardspeicherort auf dem Hyper-V-Host für die Platzierung der virtuellen Replikatfestplatten verwendet.

Die folgende Tabelle zeigt die Einrichtung der Speicherklassifizierung und der freigegebenen Clustervolumes in unserem Beispiel:

| **Standort** | **Klassifizierung** | **Zugeordneter Speicher** |
| --- | --- | --- |
| New York |GOLD |<p>C:\ClusterStorage\SourceVolume1</p><p>\\FileServer\SourceShare1</p> |
| SILVER |<p>C:\ClusterStorage\SourceVolume2</p><p>\\FileServer\SourceShare2</p> | |
| Chicago |GOLD_TARGET |<p>C:\ClusterStorage\TargetVolume1</p><p>\\FileServer\TargetShare1</p> |
| SILVER_TARGET |<p>C:\ClusterStorage\TargetVolume2</p><p>\\FileServer\TargetShare2</p> | |

Die folgende Tabelle zeigt, was passiert, wenn Sie den Schutz für virtuelle Computer (VM1 - VM5) in dieser Beispielumgebung aktivieren:

| **Virtueller Computer** | **Quellspeicher** | **Quellklassifizierung** | **Zugeordneter Zielspeicher** |
| --- | --- | --- | --- |
| VM1 |C:\ClusterStorage\SourceVolume1 |GOLD |<p>C:\ClusterStorage\SourceVolume1</p><p>\\\FileServer\SourceShare1</p><p>Beides GOLD_TARGET</p> |
| VM2 |\\FileServer\SourceShare1 |GOLD |<p>C:\ClusterStorage\SourceVolume1</p><p>\\FileServer\SourceShare1</p> <p>Beides GOLD_TARGET</p> |
| VM3 |C:\ClusterStorage\SourceVolume2 |SILVER |<p>C:\ClusterStorage\SourceVolume2</p><p>\FileServer\SourceShare2</p> |
| VM4 |\FileServer\SourceShare2 |SILVER |<p>C:\ClusterStorage\SourceVolume2</p><p>\\FileServer\SourceShare2</p><p>Beides SILVER_TARGET</p> |
| VM5 |C:\ClusterStorage\SourceVolume3 |N/V |Keine Zuordnung, daher Verwendung des Standardspeicherorts des Hyper-V-Hosts |

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie sich mit der Speicherzuordnung vertraut gemacht haben, können Sie mit der [Azure Site Recovery-Bereitstellung beginnen](site-recovery-best-practices.md).




<!--HONumber=Nov16_HO3-->


