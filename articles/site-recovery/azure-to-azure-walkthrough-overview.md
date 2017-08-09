---
title: Replizieren von Azure-VMs zwischen Azure-Regionen | Microsoft-Dokumentation
description: "In diesem Artikel werden die erforderlichen Schritte für die Replikation von Azure-VMs zwischen Azure-Regionen mit dem Azure Site Recovery-Dienst im Azure-Portal zusammengefasst"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: 6dd36239-4363-4538-bf80-a18e71b8ec67
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: c30998a77071242d985737e55a7dc2c0bf70b947
ms.openlocfilehash: 9258613161a61e36b1d0c5796d5763c916d66859
ms.contentlocale: de-de
ms.lasthandoff: 08/02/2017

---

# <a name="replicate-azure-vms-between-regions-with-azure-site-recovery"></a>Replizieren von Azure-VMs zwischen Regionen mit Azure Site Recovery

>Dieser Artikel enthält eine Übersicht über die erforderlichen Schritte zum Replizieren von virtuellen Azure-Computern (VMs) in einer Azure-Region auf Azure-VMs in einer anderen Region. 

>[!NOTE]
>
> Die Azure-VM-Replikation ist derzeit als Vorschau verfügbar.

Kommentare und Fragen können Sie am Ende dieses Artikels oder im [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.

## <a name="step-1-review-architecture"></a>Schritt 1: Überprüfen der Architektur

Bevor Sie mit der Bereitstellung beginnen, überprüfen Sie die Szenarioarchitektur und die Komponenten, die Sie bereitstellen müssen.

Gehen Sie zu [Schritt 1: Überprüfen der Architektur](azure-to-azure-walkthrough-architecture.md).


## <a name="step-2-review-prerequisites"></a>Schritt 2: Überprüfen der Voraussetzungen

Überprüfen Sie, ob die Azure-Voraussetzungen erfüllt sind. Dazu gehören ein Abonnement, virtuelle Netzwerke, Speicherkonten und VM-Anforderungen.

Gehen Sie zu [Schritt 2: Überprüfen der Voraussetzungen und Einschränkungen](azure-to-azure-walkthrough-prerequisites.md).


## <a name="step-3-plan-networking"></a>Schritt 3: Planen von Netzwerken

Überprüfen Sie, ob die ausgehende Konnektivität auf Azure-VMs eingerichtet ist, die Sie replizieren möchten, und ob entsprechende lokale Verbindungen eingerichtet sind.

Gehen Sie zu [Schritt 4: Planen von Netzwerken](azure-to-azure-walkthrough-network.md).



## <a name="step-4-create-a-vault"></a>Schritt 4: Erstellen eines Tresors 

Sie müssen einen Tresor für Recovery Services einrichten, der die Replikation orchestrieren und verwalten kann, und die Quellregion angeben.

Gehen Sie zu [Schritt 4: Erstellen eines Tresors](azure-to-azure-walkthrough-vault.md).


## <a name="step-5-enable-replication"></a>Schritt 5: Aktivieren der Replikation


Zum Aktivieren der Replikation konfigurieren Sie die Einstellungen des Zielstandorts, richten eine Replikationsrichtlinie ein und wählen die Azure-VMs aus, die Sie replizieren möchten. Nach der Aktivierung erfolgt eine erste Replikation des virtuellen Computers.

Gehen Sie zu [Schritt 5: Aktivieren der Replikation](azure-to-azure-walkthrough-enable-replication.md).


## <a name="step-6-run-a-test-failover"></a>Schritt 6: Ausführen eines Testfailovers

Nachdem die erste Replikation abgeschlossen wurde und die Deltareplikation ausgeführt wird, können Sie ein Testfailover ausführen, um sicherzustellen, dass alles wie erwartet funktioniert.

Gehen Sie zu [Schritt 6: Ausführen eines Testfailovers](azure-to-azure-walkthrough-test-failover.md).



