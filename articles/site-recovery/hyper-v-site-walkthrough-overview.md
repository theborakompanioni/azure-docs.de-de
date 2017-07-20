---
title: Replizieren von Hyper-V-VMs in Azure mit Azure Site Recovery | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie die Replikation, das Failover und die Wiederherstellung von lokalen virtuellen Hyper-V-Computern zu Azure orchestrieren.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: efddd986-bc13-4a1d-932d-5484cdc7ad8d
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: 31ecec607c78da2253fcf16b3638cc716ba3ab89
ms.openlocfilehash: da10b213bc2543942b5ac77cf5c5d8547c00220c
ms.contentlocale: de-de
ms.lasthandoff: 06/23/2017

---

# <a name="replicate-hyper-v-virtual-machines-without-vmm-to-azure"></a>Replizieren von Hyper-V-VMs (ohne VMM) in Azure 

> [!div class="op_single_selector"]
> * [Azure-Portal](site-recovery-hyper-v-site-to-azure.md)
> * [Klassisches Azure](site-recovery-hyper-v-site-to-azure-classic.md)
> * [PowerShell – Resource Manager](site-recovery-deploy-with-powershell-resource-manager.md)
>
>

In diesem Artikel erfahren Sie, wie Sie lokale virtuelle Hyper-V-Computer mit [Azure Site Recovery](site-recovery-overview.md) über das Azure-Portal in Azure replizieren können. In dieser Bereitstellung werden Hyper-V-VMs nicht von System Center Virtual Machine Manager (VMM) verwaltet.


Nach der Lektüre dieses Artikels können Sie Kommentare am Ende des Artikels oder technische Fragen im [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.


## <a name="step-1-review-architecture-and-prerequisites"></a>Schritt 1: Überprüfen der Architektur und der Voraussetzungen

Überprüfen Sie die Szenarioarchitektur, bevor Sie mit der Bereitstellung beginnen, und stellen Sie sicher, dass Sie mit allen Komponenten vertraut sind, die Sie bereitstellen möchten.

Gehen Sie zu [Schritt 1: Überprüfen der Architektur](hyper-v-site-walkthrough-architecture.md).


## <a name="step-2-review-prerequisites"></a>Schritt 2: Überprüfen der Voraussetzungen

Stellen Sie sicher, dass Sie über die Voraussetzungen für jede Bereitstellungskomponente verfügen:

- **Voraussetzungen für Azure**: Sie benötigen ein Microsoft Azure-Konto, Azure-Netzwerke und Speicherkonten.
- **Voraussetzungen für lokales Hyper-V**: Vergewissern Sie sich, dass Hyper-V-Hosts für die Site Recovery-Bereitstellung vorbereitet sind.
- **Replizierte virtuelle Computer**: Virtuelle Computer, die Sie replizieren möchten, müssen die Azure-Anforderungen zu erfüllen.

Gehen Sie zu [Schritt 2: Überprüfen der Voraussetzungen und Einschränkungen](hyper-v-site-walkthrough-prerequisites.md).

## <a name="step-3-plan-capacity"></a>Schritt 3: Planen der Kapazität

Wenn Sie eine vollständige Bereitstellung durchführen, müssen Sie herausfinden, welche Replikationsressourcen benötigt werden. Es gibt eine Reihe von Tools, die Sie dabei unterstützt. Fahren Sie mit Schritt 2 fort. Wenn Sie eine schnelle Einrichtung vornehmen, um die Umgebung zu testen, können Sie diesen Schritt überspringen.

Gehen Sie zu [Schritt 3: Planen der Kapazität](hyper-v-site-walkthrough-capacity.md).

## <a name="step-4-plan-networking"></a>Schritt 4: Planen von Netzwerken

Sie müssen einige Netzwerke planen, um sicherzustellen, dass Azure-VMs nach dem Auftreten von Failover mit Netzwerken verbunden sind und die richtigen IP-Adressen besitzen.

Gehen Sie zu [Schritt 4: Planen von Netzwerken](hyper-v-site-walkthrough-network.md).

##  <a name="step-5-prepare-azure-resources"></a>Schritt 5: Vorbereiten der Azure-Ressourcen

Richten Sie Azure-Netzwerke und -Speicher ein, bevor Sie beginnen. Dies können Sie während der Bereitstellung durchführen. Es wird jedoch empfohlen, dass sie diesen Schritt vorher vornehmen.

Gehen Sie zu [Schritt 5: Vorbereiten von Azure](hyper-v-site-walkthrough-prepare-azure.md).


## <a name="step-6-prepare-hyper-v"></a>Schritt 6: Vorbereiten von Hyper-V

Stellen Sie sicher, dass die Hyper-V-Server die Anforderungen für die Site Recovery-Bereitstellung erfüllen.

Gehen Sie zu [Schritt 6: Vorbereiten von Hyper-V](hyper-v-site-walkthrough-prepare-hyper-v.md).

## <a name="step-7-set-up-a-vault"></a>Schritt 7: Einrichten eines Tresors

Sie müssen einen Tresor für Recovery Services einrichten, der die Replikation orchestrieren und verwalten kann. Bei der Einrichtung des Tresors geben Sie an, was Sie replizieren möchten und wo es repliziert werden soll.

Gehen Sie zu [Schritt 7: Erstellen eines Tresors](hyper-v-site-walkthrough-create-vault.md).

## <a name="step-8-configure-source-and-target-settings"></a>Schritt 8: Konfigurieren von Quell- und Zieleinstellungen

Richten Sie die Quelle und das Ziel ein, die für die Replikation verwendet werden. Das Einrichten der Quelleinstellungen umfasst Folgendes: Hinzufügen von Hyper-V-Hosts zu einer Hyper-V-Site, Installieren des Site Recovery-Anbieters und des Recovery Services-Agents auf jedem Hyper-V-Host und Registrieren der Site im Recovery Services-Tresor.

Gehen Sie zu [Schritt 8: Einrichten der Quelle und des Ziels](hyper-v-site-walkthrough-source-target.md).

## <a name="step-9-set-up-a-replication-policy"></a>Schritt 9: Einrichten einer Replikationsrichtlinie

Sie richten eine Richtlinie ein, um die Replikationseinstellungen für Hyper-V-VMs im Tresor anzugeben.

Gehen Sie zu [Schritt 9: Einrichten einer Replikationsrichtlinie](hyper-v-site-walkthrough-replication.md).


## <a name="step-10-enable-replication"></a>Schritt 10: Aktivieren der Replikation

Nachdem eine Replikationsrichtlinie eingerichtet und aktiviert wurde, erfolgt die erste Replikation des virtuellen Computers.

Gehen Sie zu [Schritt 10: Aktivieren der Replikation](hyper-v-site-walkthrough-enable-replication.md).

## <a name="step-11-run-a-test-failover"></a>Schritt 11: Ausführen eines Testfailovers

Nachdem die erste Replikation abgeschlossen wurde und die Deltareplikation ausgeführt wird, können Sie ein Testfailover ausführen, um sicherzustellen, dass alles wie erwartet funktioniert.

Gehen Sie zu [Schritt 11: Ausführen eines Testfailovers](hyper-v-site-walkthrough-test-failover.md).

