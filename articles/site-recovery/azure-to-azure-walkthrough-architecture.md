---
title: "Überprüfen der Architektur für die Replikation von Azure-VMs zwischen Azure-Regionen | Microsoft-Dokumentation"
description: "Dieser Artikel bietet einen Überblick über die Komponenten und die Architektur, die beim Replizieren von Azure-VMs zwischen Azure-Regionen mit dem Azure Site Recovery-Dienst verwendet werden."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/12/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: c30998a77071242d985737e55a7dc2c0bf70b947
ms.openlocfilehash: f471add4f4dee26482e2820fb06d010d6c0c0e88
ms.contentlocale: de-de
ms.lasthandoff: 08/02/2017

---

# <a name="step-1-review-the-architecture-for-azure-vm-replication-between-azure-regions"></a>Schritt 1: Überprüfen der Architektur für die Azure-VM-Replikation zwischen Azure-Regionen


Nach dem Überprüfen der [Übersicht über die Schritte](azure-to-azure-walkthrough-overview.md) für diese Bereitstellung lesen Sie diesen Artikel, um die Komponenten und Prozesse kennen zu lernen, die beim Replizieren und Wiederherstellen von virtuellen Azure-Computern (VMs) zwischen Azure-Regionen mit [Azure Site Recovery](site-recovery-overview.md) verwendet werden.

- Wenn Sie diesen Artikel gelesen haben, wissen Sie, wie die Replikation von Azure-VMs in einer anderen Region funktioniert.
- Kommentare können Sie am Ende dieses Artikels senden oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) Fragen stellen.

>[!NOTE]
>Die Replikation von Azure-VMs mit dem Site Recovery-Dienst ist derzeit in der Vorschau verfügbar.



## <a name="architectural-components"></a>Komponenten der Architektur

Die folgende Abbildung enthält einen allgemeinen Überblick über eine Azure-VM-Umgebung in einer bestimmten Region (in diesem Beispiel wird der Standort „USA, Osten“ verwendet). In einer Azure-VM-Umgebung:
- Apps können auf VMs mit Datenträgern über alle Speicherkonten hinweg ausgeführt werden.
- Die VMs können in einem oder in mehreren Subnetzen innerhalb eines virtuellen Netzwerks enthalten sein.

![Kundenumgebung](./media/azure-to-azure-walkthrough-architecture/source-environment.png)

## <a name="replication-process"></a>Replikationsprozess

### <a name="step-1"></a>Schritt 1

Wenn Sie die Replikation von Azure-VMs im Azure-Portal aktivieren, werden automatisch die in der folgenden Abbildung und Tabelle enthaltenen Ressourcen in der Zielregion erstellt. Standardmäßig werden Ressourcen basierend auf den Quellregionseinstellungen erstellt. Bei Bedarf können Sie die Zieleinstellungen anpassen. [Weitere Informationen](site-recovery-replicate-azure-to-azure.md)

![Schritt 1: Aktivieren des Replikationsprozesses](./media/azure-to-azure-walkthrough-architecture/enable-replication-step-1.png)

**Ressource** | **Details**
--- | ---
**Zielressourcengruppe** | Die Ressourcengruppe, zu denen replizierte VMs nach einem Failover gehören.
**Virtuelles Zielnetzwerk** | Das virtuelle Netzwerk, in dem sich nach einem Failover replizierte VMs befinden. Eine Netzwerkzuordnung von virtuellen Quell- zu Zielnetzwerken (und umgekehrt) wird erstellt.
**Cachespeicherkonten** | Bevor Änderungen an Quell-VMs im Zielspeicherkonto repliziert werden, werden sie nachverfolgt und an das Cachespeicherkonto am Zielstandort gesendet. Dadurch wird sichergestellt, dass die Auswirkungen auf Produktion-Apps, die auf der VM ausgeführt werden, so gering wie möglich sind.
**Zielspeicherkonten**  | Speicherkonten am Zielstandort, an dem die Daten repliziert werden.
**Zielverfügbarkeitsgruppen**  | Verfügbarkeitsgruppen, in denen sich nach einem Failover die replizierten VMs befinden.

### <a name="step-2"></a>Schritt 2

Da die Replikation aktiviert ist, wird automatisch die Site Recovery-Erweiterung Mobility Service auf der VM installiert. Folgendes passiert:

1. Die VM wird bei Site Recovery registriert.

2. Für die VM wird die fortlaufende Replikation konfiguriert. Datenschreibvorgänge auf VM-Datenträgern werden kontinuierlich auf das Cachespeicherkonto am Quellstandort übertragen.

   ![Schritt 2: Aktivieren des Replikationsprozesses](./media/azure-to-azure-walkthrough-architecture/enable-replication-step-2.png)

  
  Beachten Sie, dass Site Recovery niemals eingehende Konnektivität mit der VM benötigt. Es ist nur ausgehende Konnektivität mit den URLs oder IP-Adressen des Site Recovery-Diensts, den URLs oder IP-Adressen der Office 365-Authentifizierung und den IP-Adressen des Cachespeicherkontos erforderlich. 

## <a name="continuous-replication-process"></a>Prozess der fortlaufenden Replikation

Wenn die fortlaufende Replikation funktioniert, werden Schreibvorgänge auf dem Datenträger sofort auf das Cachespeicherkonto übertragen. Site Recovery verarbeitet die Daten und sendet sie an das Zielspeicherkonto. Nachdem die Daten verarbeitet wurden, werden alle paar Minuten Wiederherstellungspunkte im Zielspeicherkonto generiert.

## <a name="failover-process"></a>Failoverprozess

Bei der Initiierung eines Failovers werden die VMs in der Zielressourcengruppe, im virtuellen Zielnetzwerk, im Zielsubnetz und in der Zielverfügbarkeitsgruppe erstellt. Bei einem Failover können Sie einen beliebigen Wiederherstellungspunkt verwenden.

![Failoverprozess](./media/azure-to-azure-walkthrough-architecture/failover.png)

## <a name="next-steps"></a>Nächste Schritte

Gehen Sie zu [Schritt 2: Überprüfen der Voraussetzungen und Einschränkungen](azure-to-azure-walkthrough-prerequisites.md).

