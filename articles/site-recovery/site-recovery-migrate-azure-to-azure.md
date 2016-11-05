---
title: Migrieren von virtuellen Azure-IaaS-Computern zwischen Azure-Regionen mit Site Recovery | Microsoft Docs
description: Verwenden Sie Azure Site Recovery, um virtuelle Azure-IaaS-Computer aus einer Azure-Region in eine andere zu migrieren.
services: site-recovery
documentationcenter: ''
author: rayne-wiselman
manager: jwhit
editor: tysonn

ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2016
ms.author: raynew

---
# Migrieren von virtuellen Azure-IaaS-Computern zwischen Azure-Regionen mit Azure Site Recovery
## Übersicht
Willkommen bei Azure Site Recovery! Verwenden Sie diesen Artikel, wenn Sie Azure-VMs zwischen Azure-Regionen migrieren möchten. Bevor Sie beginnen, beachten Sie bitte Folgendes:

* Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: Azure Resource Manager- und klassische Bereitstellung. Azure verfügt auch über zwei Portale: das klassische Azure-Portal mit Unterstützung des klassischen Bereitstellungsmodells und das Azure-Portal mit Unterstützung für beide Bereitstellungsmodelle. Die grundlegenden Schritte für die Migration sind bei der Konfiguration von Site Recovery in Resource Manager oder im klassischen Modell identisch. Die UI-Anweisungen und Screenshots in diesem Artikel sind jedoch für das Azure-Portal relevant.
* **Derzeit können Sie nur von einer Region zu einer anderen migrieren. Sie können für virtuelle Computer ein Failover von einer Azure-Region in eine andere durchführen, aber nicht wieder zurück.**
* Die Anweisungen zur Migration in diesem Artikel basieren auf den Anweisungen für die Replikation eines physischen Computers in Azure. Er enthält Links zu den Schritten unter [Replizieren von virtuellen VMware-Computern und physischen Computern nach Azure](site-recovery-vmware-to-azure.md), wo beschrieben wird, wie Sie einen physischen Server im Azure-Portal replizieren.
* Wenn Sie Site Recovery im klassischen Portal einrichten, führen Sie die detaillierten Anweisungen [in diesem Artikel](site-recovery-vmware-to-azure-classic.md) aus. Die Anweisungen in diesem [älteren Artikel](site-recovery-vmware-to-azure-classic-legacy.md) sollten **nicht mehr verwendet werden**.

Kommentare oder Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.

## Voraussetzungen
Für diese Bereitstellung benötigen Sie Folgendes:

* **Konfigurationsserver**: Ein lokaler virtueller Computer unter Windows Server 2012 R2, der als Konfigurationsserver dient. Sie installieren die anderen Site Recovery-Komponenten (einschließlich Prozessserver und Masterzielserver) auch auf diesem virtuellen Computer. Erfahren Sie mehr unter [Szenarioarchitektur](site-recovery-vmware-to-azure.md#scenario-architecture) und [Voraussetzungen für den Konfigurationsserver](site-recovery-vmware-to-azure.md#configuration-server-prerequisites).
* **Virtuelle IaaS-Computer**: die virtuellen Computer, die Sie migrieren möchten. Migrieren Sie diese virtuellen Computer, indem Sie sie wie physische Computer behandeln.

## Bereitstellungsschritte
Dieser Abschnitt beschreibt die Bereitstellungsschritte im neuen Azure-Portal. Wenn Sie diese Bereitstellungsschritte für Site Recovery im klassischen Portal benötigen, lesen Sie [diesen Artikel](site-recovery-vmware-to-azure-classic.md).

1. [Erstellen Sie einen Tresor](site-recovery-vmware-to-azure.md#create-a-recovery-services-vault).
2. [Stellen Sie einen Konfigurationsserver bereit](site-recovery-vmware-to-azure.md#step-2-set-up-the-source-environment).
3. Vergewissern Sie sich nach dem Bereitstellen des Konfigurationsservers, dass er mit den zu migrierenden virtuellen Computern kommunizieren kann.
4. [Richten Sie Replikationseinstellungen ein](site-recovery-vmware-to-azure.md#step-4-set-up-replication-settings). Erstellen Sie eine Replikationsrichtlinie, und weisen Sie sie dem Konfigurationsserver zu.
5. [Installieren Sie den Mobilitätsdienst](site-recovery-vmware-to-azure.md#step-6-replication-application). Auf jedem virtuellen Computer, den Sie schützen möchten, muss der Mobilitätsdienst installiert sein. Dieser Dienst sendet Daten an den Prozessserver. Der Mobilitätsdienst kann manuell installiert oder mittels Push automatisch durch den Prozessserver installiert werden, wenn der Schutz für den virtuellen Computer aktiviert wird. Konfigurieren Sie Firewallregeln für die zu migrierenden virtuellen Computer, um die Pushinstallation des Diensts zu ermöglichen.
6. [Aktivieren Sie die Replikation](site-recovery-vmware-to-azure.md#enable-replication). Aktivieren Sie die Replikation für die virtuellen Computer, die Sie migrieren möchten. Sie können die virtuellen IaaS-Computer, die Sie zu Azure migrieren möchten, mithilfe der privaten IP-Adressen der virtuellen Computer ermitteln. Diese Adresse finden Sie auf dem Dashboard des virtuellen Computers in Azure. Wenn Sie die Replikation aktivieren, legen Sie für die virtuellen Computer den Computertyp als physische Computer fest.
7. [Führen Sie ein nicht geplantes Failover durch](site-recovery-failover.md#run-an-unplanned-failover). Nach Abschluss der ersten Replikation können Sie ein nicht geplantes Failover von einer Azure-Region auf eine andere durchführen. Optional können Sie einen Wiederherstellungsplan erstellen und ein nicht geplantes Failover durchführen, um mehrere virtuelle Computer zwischen Regionen zu migrieren. Weitere Informationen zu Wiederherstellungsplänen finden Sie [hier](site-recovery-create-recovery-plans.md).

## Nächste Schritte
Weitere Informationen zu anderen Replikationsszenarien finden Sie in [Was ist Azure Site Recovery?](site-recovery-overview.md).

<!---HONumber=AcomDC_0824_2016-->