---
title: Wie funktioniert die Replikation von virtuellen Azure-Computern zwischen Azure-Regionen in Azure Site Recovery?  | Microsoft Docs
description: "Dieser Artikel bietet einen Überblick über die Komponenten und die Architektur, die beim Replizieren von Azure-VMs zwischen Azure-Regionen mit dem Azure Site Recovery-Dienst verwendet werden."
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/29/2017
ms.author: sujayt
ms.translationtype: Human Translation
ms.sourcegitcommit: db18dd24a1d10a836d07c3ab1925a8e59371051f
ms.openlocfilehash: ec397eaeda963f257d1bd996f1f57189bcde17ca
ms.contentlocale: de-de
ms.lasthandoff: 06/15/2017


---

# <a name="how-does-azure-vm-replication-work-in-site-recovery"></a>Wie funktioniert die Replikation von Azure-VMs mit Site Recovery?


Dieser Artikel beschreibt die Komponenten und Prozesse bei der Replikation und Wiederherstellung von virtuellen Azure-Computern (VMs) von einer Region in einer anderen mithilfe des [Azure Site Recovery](site-recovery-overview.md)-Diensts.

>[!NOTE]
>Die Replikation von Azure-VMs mit dem Site Recovery-Dienst ist derzeit in der Vorschau verfügbar.

Kommentare können Sie am Ende dieses Artikels senden oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) Fragen stellen.

## <a name="architectural-components"></a>Komponenten der Architektur

Die folgende Abbildung enthält einen allgemeinen Überblick über eine Azure-VM-Umgebung in einer bestimmten Region (in diesem Beispiel wird der Standort „USA, Osten“ verwendet). In einer Azure-VM-Umgebung:
- Apps können auf VMs mit Datenträgern über alle Speicherkonten hinweg ausgeführt werden.
- Die VMs können in einem oder in mehreren Subnetzen innerhalb eines virtuellen Netzwerks enthalten sein.

![Kundenumgebung](./media/site-recovery-azure-to-azure-architecture/source-environment.png)

Weitere Informationen zu den Voraussetzungen und Anforderungen für die Bereitstellung finden Sie in der [Supportmatrix](site-recovery-support-matrix-azure-to-azure.md).

## <a name="replication-process"></a>Replikationsprozess

### <a name="step-1"></a>Schritt 1

Wenn Sie die Replikation von Azure-VMs im Azure-Portal aktivieren, werden automatisch die in der folgenden Abbildung und Tabelle enthaltenen Ressourcen in der Zielregion erstellt. Standardmäßig werden Ressourcen basierend auf den Quellregionseinstellungen erstellt. Bei Bedarf können Sie die Zieleinstellungen anpassen. [Weitere Informationen](site-recovery-replicate-azure-to-azure.md)

![Schritt 1: Aktivieren des Replikationsprozesses](./media/site-recovery-azure-to-azure-architecture/enable-replication-step-1.png)

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

   ![Schritt 2: Aktivieren des Replikationsprozesses](./media/site-recovery-azure-to-azure-architecture/enable-replication-step-2.png)

   >[!IMPORTANT]
   > Site Recovery benötigt nie eingehende Verbindungen mit der VM. Die VM benötigt nur ausgehende Verbindungen mit den URLs/IP-Adressen des Site Recovery-Diensts, den URLs/IP-Adressen der Office 365-Authentifizierung bzw. die IP-Adressen des Cachespeicherkontos. Weitere Informationen finden Sie im Artikel [Netzwerkrichtlinien zum Replizieren von virtuellen Azure-Computern](site-recovery-azure-to-azure-networking-guidance.md).

## <a name="continuous-replication-process"></a>Prozess der fortlaufenden Replikation

Wenn die fortlaufende Replikation funktioniert, werden Schreibvorgänge auf dem Datenträger sofort auf das Cachespeicherkonto übertragen. Site Recovery verarbeitet die Daten und sendet sie an das Zielspeicherkonto. Nachdem die Daten verarbeitet wurden, werden alle paar Minuten Wiederherstellungspunkte im Zielspeicherkonto generiert.

## <a name="failover-process"></a>Failoverprozess

Bei der Initiierung eines Failovers werden die VMs in der Zielressourcengruppe, im virtuellen Zielnetzwerk, im Zielsubnetz und in der Zielverfügbarkeitsgruppe erstellt. Bei einem Failover können Sie einen beliebigen Wiederherstellungspunkt verwenden.

![Failoverprozess](./media/site-recovery-azure-to-azure-architecture/failover.png)

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Netzwerke](site-recovery-azure-to-azure-networking-guidance.md) für die Replikation von Azure-VMs.
- Führen Sie eine exemplarische Vorgehensweise zum [Replizieren von Azure-VMs](site-recovery-azure-to-azure.md) durch.

