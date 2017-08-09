---
title: Replizieren von Hyper-V-VMs in VMM-Clouds nach Azure mit Azure Site Recovery | Microsoft-Dokumentation
description: "Eine Übersicht über die Replikation von Hyper-V VMs in VMM-Clouds nach Azure mithilfe des Azure Site Recovery-Diensts."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: 74b75232b4b1c14dbb81151cdab5856a1e4da28c
ms.openlocfilehash: af68d21184c137b2b0f1bb4c1afb9bf507e8332d
ms.contentlocale: de-de
ms.lasthandoff: 07/26/2017

---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-site-recovery-in-the-azure-portal"></a>Replizieren von virtuellen Hyper-V-Computern in VMM-Clouds in Azure per Site Recovery im Azure-Portal
> [!div class="op_single_selector"]
> * [Azure-Portal](site-recovery-vmm-to-azure.md)
> * [Klassisches Azure](site-recovery-vmm-to-azure-classic.md)
> * [PowerShell – Resource Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [PowerShell (klassisch)](site-recovery-deploy-with-powershell.md)


Dieser Artikel bietet eine Übersicht über die erforderlichen Schritte, um lokale virtuelle Hyper-V-Computer, die in System Center Virtual Machine Manager-Clouds (VMM) verwaltet werden, mithilfe des [Azure Site Recovery](site-recovery-overview.md)-Diensts im Azure-Portal nach Azure zu replizieren.

Kommentare können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) hinterlassen.


## <a name="step-1-review-the-scenario-architecture"></a>Schritt 1: Überprüfen der Architektur für das Szenario

Überprüfen Sie die Szenarioarchitektur, bevor Sie mit der Bereitstellung beginnen, und stellen Sie sicher, dass Sie mit allen Komponenten vertraut sind, die Sie bereitstellen müssen.

Gehen Sie zu [Schritt 1: Überprüfen der Architektur](vmm-to-azure-walkthrough-architecture.md).

## <a name="step-2-review-prerequisites-and-limitations"></a>Schritt 2: Überprüfen der Voraussetzungen und Einschränkungen

Stellen Sie sicher, dass Sie alle Voraussetzungen und Einschränkungen der Bereitstellung kennen.

**Azure-Voraussetzungen**: Sie benötigen ein Microsoft Azure-Konto, Azure-Netzwerke und Speicherkonten.
**Lokale VMM-Server und Hyper-V-Hosts**: Stellen Sie sicher, dass die VMM-Server und Hyper-V-Hosts kompatibel und auf die Site Recovery-Bereitstellung vorbereitet sind.
**Replizierte virtuelle Computer**: Überprüfen Sie, ob die virtuellen Computer, die Sie replizieren möchten, die Azure-Anforderungen erfüllen.

Gehen Sie zu [Schritt 2: Überprüfen der Voraussetzungen und Einschränkungen](vmm-to-azure-walkthrough-prerequisites.md).

## <a name="step-3-plan-capacity"></a>Schritt 3: Planen der Kapazität

Wenn Sie eine vollständige Bereitstellung durchführen, müssen Sie herausfinden, welche Replikationsressourcen benötigt werden. Es gibt eine Reihe von Tools, die Sie dabei unterstützt. Wenn Sie zum Testen der Umgebung ein schnelles Setup durchführen können Sie diesen Schritt überspringen.

Gehen Sie zu [Schritt 3: Planen der Kapazität](vmm-to-azure-walkthrough-capacity.md).

## <a name="step-4-plan-networking"></a>Schritt 4: Planen von Netzwerken

Sie müssen einige Planungen für das Netzwerk durchführen, um sicherzustellen, dass Sie die Netzwerkzuordnung beim Bereitstellen des Szenarios konfigurieren können, dass die virtuellen Azure-Computer nach einem Failover mit virtuellen Azure-Netzwerken verbunden sein werden und dass den VMs geeignete IP-Adressen zugewiesen werden.

Gehen Sie zu [Schritt 4: Planen von Netzwerken](vmm-to-azure-walkthrough-network.md).


## <a name="step-5-prepare-azure-resources"></a>Schritt 5: Vorbereiten der Azure-Ressourcen

Richten Sie ein Azure-Konto mit Netzwerk und Speicher ein. Dies können Sie während der Bereitstellung durchführen. Es wird jedoch empfohlen, dass sie diesen Schritt vorher vornehmen.

Gehen Sie zu [Schritt 5: Vorbereiten von Azure](vmm-to-azure-walkthrough-prepare-azure.md).

## <a name="step-6-prepare-vmm-and-hyper-v"></a>Schritt 6: Vorbereiten von VMM und Hyper-V

Bereiten Sie die lokalen VMM-Server und Hyper-V-Hosts auf die Site Recovery-Bereitstellung vor.

Gehen Sie zu [Schritt 6: Vorbereiten von lokalen Servern](vmm-to-azure-walkthrough-vmm-hyper-v.md).

## <a name="step-7-set-up-a-vault"></a>Schritt 7: Einrichten eines Tresors

Richten Sie einen Recovery Services-Tresor ein. Der Tresor enthält Konfigurationseinstellungen und dient zum Orchestrieren der Replikation.

[Schritt 7: Einrichten eines Tresors](vmm-to-azure-walkthrough-create-vault.md)

## <a name="step-8-configure-source-and-target-settings"></a>Schritt 8: Konfigurieren von Quell- und Zieleinstellungen

Richten Sie die Quell- und Zielstandorte für die Replikation ein. Fügen Sie den VMM-Server zum Tresor hinzu, und laden Sie die Installationsdateien für Site Recovery-Komponenten herunter. Führen Sie das Setup für den Azure Site Recovery-Anbieter auf dem VMM-Server aus. Setup installiert den Azure Site Recovery-Anbieter auf dem VMM-Server installiert und registriert den Server im Tresor. Sie installieren den Microsoft Recovery Services-Agent auf jedem Hyper-V-Host.

Gehen Sie zu [Schritt 8: Konfigurieren von Quell- und Zieleinstellungen](vmm-to-azure-walkthrough-source-target.md).

## <a name="step-9-configure-network-mapping"></a>Schritt 9: Konfigurieren der Netzwerkzuordnung

Ordnen Sie lokale VMM-VM-Netzwerke zu virtuellen Azure-Netzwerken zu. Nach einem Failover werden Azure-VMs in dem Azure-Netzwerk erstellt, das dem lokalen VM-Netzwerk zugeordnet ist, in dem sich die Hyper-V-Quelle befindet.

Gehen Sie zu [Schritt 9: Konfigurieren der Netzwerkzuordnung](vmm-to-azure-walkthrough-network-mapping.md).


## <a name="step-10-set-up-a-replication-policy"></a>Schritt 10: Einrichten einer Replikationsrichtlinie

Geben Sie an, wie lokale VMs nach Azure repliziert werden sollen.

Gehen Sie zu [Schritt 10: Einrichten einer Replikationsrichtlinie](vmm-to-azure-walkthrough-replication.md).


## <a name="step-11-enable-replication-for-vms"></a>Schritt 11: Aktivieren der Replikation für VMs

Wählen Sie die VMs aus, die Sie replizieren möchten. Durch Aktivieren einer VM für die Replikation wird die erste Replikation nach Azure ausgelöst. Danach erfolgt eine fortlaufende Deltareplikation.

Gehen Sie zu [Schritt 11: Aktivieren der Replikation](vmm-to-azure-walkthrough-enable-replication.md).


## <a name="step-12-run-a-test-failover"></a>Schritt 12: Ausführen eines Testfailovers

Führen Sie ein Testfailover aus, um sicherzustellen, dass alles wie erwartet funktioniert.

Gehen Sie zu [Schritt 12: Ausführen eines Testfailovers](vmm-to-azure-walkthrough-test-failover.md).



