---
title: "Replizieren von virtuellen Hyper-V-Computern an einem sekundären VMM-Standort mit Azure Site Recovery | Microsoft-Dokumentation"
description: "Eine Übersicht über die Replikation von virtuellen Hyper-V-Computern an einem sekundären VMM-Standort über das Azure-Portal."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 476ca82a-8f5c-4498-9dcf-e1011d60ed59
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: fff84ee45818e4699df380e1536f71b2a4003c71
ms.openlocfilehash: b422dd2cf23426de2f154a553b38509082536309
ms.contentlocale: de-de
ms.lasthandoff: 08/01/2017

---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site"></a>Replizieren von virtuellen Hyper-V-Maschinen in VMM-Clouds an einen sekundären VMM-Standort

> [!div class="op_single_selector"]
> * [Azure-Portal](site-recovery-vmm-to-vmm.md)
> * [Klassisches Portal](site-recovery-vmm-to-vmm-classic.md)
> * [PowerShell – Resource Manager](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
>
>

Dieser Artikel bietet eine Übersicht über die erforderlichen Schritte, um lokale virtuelle Hyper-V-Computer, die in System Center Virtual Machine Manager-Clouds (VMM) verwaltet werden, mithilfe von [Azure Site Recovery](site-recovery-overview.md) im Azure-Portal an einem sekundären VMM-Standort zu replizieren.

Kommentare können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) hinterlassen.


## <a name="step-1-review-the-scenario-architecture"></a>Schritt 1: Überprüfen der Architektur für das Szenario

Überprüfen Sie die Szenarioarchitektur, bevor Sie mit der Bereitstellung beginnen, und stellen Sie sicher, dass Sie mit allen Komponenten vertraut sind, die Sie bereitstellen möchten.

Wechseln Sie zu [Schritt 1: Überprüfen der Architektur](vmm-to-vmm-walkthrough-architecture.md).

## <a name="step-2-review-prerequisites-and-limitations"></a>Schritt 2: Überprüfen der Voraussetzungen und Einschränkungen

Stellen Sie sicher, dass Sie alle Voraussetzungen und Einschränkungen der Bereitstellung kennen.

**Voraussetzungen für Azure:** Sie benötigen ein Microsoft Azure-Abonnement und einen Azure Recovery Services-Tresor zum Orchestrieren und Verwalten der Replikation.
**Lokale VMM-Server und Hyper-V-Hosts:** Stellen Sie sicher, dass die VMM-Server und Hyper-V-Hosts konform und für die Site Recovery-Bereitstellung vorbereitet sind.

Wechseln Sie zu [Schritt 2: Überprüfen der Voraussetzungen und Einschränkungen](vmm-to-vmm-walkthrough-prerequisites.md).

## <a name="step-3-plan-networking"></a>Schritt 3: Planen von Netzwerken

Sie müssen einige Planungen für das Netzwerk durchführen, um sicherzustellen, dass Sie die Netzwerkzuordnung zwischen VMM-VM-Netzwerken beim Bereitstellen des Szenarios konfigurieren können.

Wechseln Sie zu [Schritt 3: Planen von Netzwerken](vmm-to-vmm-walkthrough-network.md).


## <a name="step-4-prepare-vmm-and-hyper-v"></a>Schritt 4: Vorbereiten von VMM und Hyper-V

Bereiten Sie die VMM-Server und Hyper-V-Hosts für die Site Recovery-Bereitstellung vor.

Wechseln Sie zu [Schritt 4: Vorbereiten von lokalen Servern](vmm-to-vmm-walkthrough-vmm-hyper-v.md).

## <a name="step-5-set-up-a-vault"></a>Schritt 5: Einrichten eines Tresors

Richten Sie einen Recovery Services-Tresor ein. Der Tresor enthält Konfigurationseinstellungen und dient zum Orchestrieren der Replikation.

[Schritt 5: Einrichten eines Tresors](vmm-to-vmm-walkthrough-create-vault.md).

## <a name="step-6-set-up-source-and-target-settings"></a>Schritt 6: Einrichten der Quell- und Zieleinstellungen

Richten Sie die Quell- und Zieleinstellungen für die Replikation auf VMM-Standorten ein. Fügen Sie die VMM-Server dem Tresor hinzu, und laden Sie die Installationsdateien für Site Recovery-Komponenten herunter. Führen Sie das Setup für den Azure Site Recovery-Anbieter auf dem VMM-Server aus. Mit dem Setup wird der Azure Site Recovery-Anbieter auf dem VMM-Server installiert und der Server im Tresor registriert. Sie installieren den Microsoft Recovery Services-Agent auf jedem Hyper-V-Host.

Wechseln Sie zu [Schritt 6: Einrichten der Quell- und Zieleinstellungen](vmm-to-vmm-walkthrough-source-target.md).

## <a name="step-7-configure-network-mapping"></a>Schritt 7: Konfigurieren der Netzwerkzuordnung

Ordnen Sie VMM-VM-Netzwerke in den Quell- und Zielstandorten zu. Nach einem Failover werden virtuelle Computer in dem Zielnetzwerk erstellt, das dem Quell-VM-Netzwerk zugeordnet ist, in dem sich der virtuelle Hyper-V-Quellcomputer befindet.

Wechseln Sie zu [Schritt 7: Konfigurieren der Netzwerkzuordnung](vmm-to-vmm-walkthrough-network-mapping.md).


## <a name="step-8-set-up-a-replication-policy"></a>Schritt 8: Einrichten einer Replikationsrichtlinie

Geben Sie an, wie virtuelle Computer zwischen VMM-Standorten repliziert werden.

Wechseln Sie zu [Schritt 8: Einrichten einer Replikationsrichtlinie](vmm-to-vmm-walkthrough-replication.md).


## <a name="step-9-enable-replication-for-vms"></a>Schritt 9: Aktivieren der Replikation für virtuelle Computer

Wählen Sie die virtuellen Computer aus, die Sie replizieren möchten. Durch Aktivieren eines virtuellen Computers für die Replikation wird die erste Replikation am sekundären Standort ausgelöst. Danach erfolgt eine fortlaufende Deltareplikation.

Wechseln Sie zu [Schritt 9: Aktivieren der Replikation](vmm-to-vmm-walkthrough-enable-replication.md).


## <a name="step-10-run-a-test-failover"></a>Schritt 10: Ausführen eines Testfailovers

Führen Sie ein Testfailover aus, um sicherzustellen, dass alles wie erwartet funktioniert.

Wechseln Sie zu [Schritt 10: Ausführen eines Testfailovers](vmm-to-vmm-walkthrough-test-failover.md).

