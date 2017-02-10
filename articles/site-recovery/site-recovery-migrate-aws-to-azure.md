---
title: Migrieren virtueller Computer von Amazon Web Services zu Azure mit Site Recovery | Microsoft Docs
description: "Dieser Artikel beschreibt, wie Sie in Amazon Web Services (AWS) ausgeführte virtuelle Computer mithilfe von Azure Site Recovery zu Azure migrieren."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: ddb412fd-32a8-4afa-9e39-738b11b91118
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 11/01/2016
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: 5614c39d914d5ae6fde2de9c0d9941e7b93fc10f
ms.openlocfilehash: c7238d677b315390bc5f53e54ab7dbded2871c5a


---
# <a name="migrate-virtual-machines-in-amazon-web-services-aws-to-azure-with-azure-site-recovery"></a>Migrieren virtueller Computer von Amazon Web Services (AWS) zu Azure mit Azure Site Recovery
## <a name="overview"></a>Übersicht
Willkommen bei Azure Site Recovery. Verwenden Sie die Informationen in diesem Artikel, um in AWS ausgeführte EC2-Instanzen mit Site Recovery zu Azure zu migrieren. Bevor Sie beginnen, beachten Sie bitte Folgendes:

* Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: Azure Resource Manager- und klassische Bereitstellung. Azure verfügt auch über zwei Portale: das klassische Azure-Portal mit Unterstützung des klassischen Bereitstellungsmodells und das Azure-Portal mit Unterstützung für beide Bereitstellungsmodelle. Die grundlegenden Schritte für die Migration sind bei der Konfiguration von Site Recovery in Resource Manager oder im klassischen Modell identisch. Die UI-Anweisungen und Screenshots in diesem Artikel sind jedoch für das Azure-Portal relevant.
* **Derzeit können Sie nur von AWS zu Azure migrieren. Sie können ein Failover virtueller Computer von AWS zu Azure durchführen, aber nicht wieder zurück. Es gibt keine fortlaufende Replikation.**
* Die Anweisungen zur Migration in diesem Artikel basieren auf den Anweisungen für die Replikation eines physischen Computers in Azure. Er enthält Links zu den Schritten unter [Replizieren von virtuellen VMware-Computern und physischen Computern nach Azure](site-recovery-vmware-to-azure.md), wo beschrieben wird, wie Sie einen physischen Server im Azure-Portal replizieren.
* Wenn Sie Site Recovery im klassischen Portal einrichten, führen Sie die detaillierten Anweisungen [in diesem Artikel](site-recovery-vmware-to-azure-classic.md)aus. **älteren Artikel** sollten [nicht mehr verwendet werden](site-recovery-vmware-to-azure-classic-legacy.md)aus.

Kommentare oder Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)

## <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme
Site Recovery kann zum Migrieren von EC2-Instanzen verwendet werden, die unter einem der folgenden Betriebssysteme ausgeführt werden:

### <a name="windows64-bit-only"></a>Windows (nur 64 Bit)
* Windows Server 2008 R2 SP1+ (nur Citrix PV-Treiber oder AWS PV-Treiber; **Instanzen mit RedHat PV-Treibern werden nicht unterstützt**)
* Windows Server 2012
* Windows Server 2012 R2

### <a name="linux-64-bit-only"></a>Linux (nur 64 Bit)
* Red Hat Enterprise Linux 6.7 (nur virtualisierte HVM-Instanzen)

## <a name="prerequisites"></a>Voraussetzungen
Für diese Bereitstellung benötigen Sie Folgendes:

* **Konfigurationsserver**: Ein lokaler virtueller Computer unter Windows Server 2012 R2, der als Konfigurationsserver dient. Sie installieren die anderen Site Recovery-Komponenten (einschließlich Prozessserver und Masterzielserver) auch auf diesem virtuellen Computer. Erfahren Sie mehr unter [Szenarioarchitektur](site-recovery-vmware-to-azure.md#scenario-architecture) und [Voraussetzungen für den Konfigurationsserver](site-recovery-vmware-to-azure.md#configuration-server-or-additional-process-server-prerequisites).
* **EC2-VM-Instanzen**: Die EC2-Instanzen, die Sie migrieren möchten.

## <a name="deployment-steps"></a>Bereitstellungsschritte
Dieser Abschnitt beschreibt die Bereitstellungsschritte im neuen Azure-Portal. Wenn Sie diese Bereitstellungsschritte für Site Recovery im klassischen Portal benötigen, lesen Sie [diesen Artikel](site-recovery-vmware-to-azure-classic.md).

1. [Erstellen Sie einen Tresor](site-recovery-vmware-to-azure.md#create-a-recovery-services-vault).
2. [Stellen Sie einen Konfigurationsserver bereit](site-recovery-vmware-to-azure.md#step-2-set-up-the-source-environment).
3. Vergewissern Sie sich nach dem Bereitstellen des Konfigurationsservers, dass er mit den zu migrierenden virtuellen Computern kommunizieren kann.
4. [Richten Sie Replikationseinstellungen ein](site-recovery-vmware-to-azure.md#step-4-set-up-replication-settings). Erstellen Sie eine Replikationsrichtlinie, und weisen Sie sie dem Konfigurationsserver zu.
5. [Installieren Sie den Mobilitätsdienst](site-recovery-vmware-to-azure.md#step-6-replicate-applications). Auf jedem virtuellen Computer, den Sie schützen möchten, muss der Mobilitätsdienst installiert sein. Dieser Dienst sendet Daten an den Prozessserver. Der Mobilitätsdienst kann manuell installiert oder mittels Push automatisch durch den Prozessserver installiert werden, wenn der Schutz für den virtuellen Computer aktiviert wird. Konfigurieren Sie Firewallregeln für die zu migrierenden EC2-Instanzen, um die Push-Installation des Diensts zu ermöglichen. Die Sicherheitsgruppe für EC2-Instanzen muss über folgende Regeln verfügen:

    ![Firewallregeln](./media/site-recovery-migrate-aws-to-azure/migrate-firewall.png)
6. [Aktivieren Sie die Replikation](site-recovery-vmware-to-azure.md#enable-replication). Aktivieren Sie die Replikation für die virtuellen Computer, die Sie migrieren möchten. Die EC2-Instanzen können anhand der privaten IP-Adressen ermittelt werden, die Sie über die EC2-Konsole abrufen können.
7. [ Führen Sie ein nicht geplantes Failover durch](site-recovery-failover.md#run-an-unplanned-failover). Nach Abschluss der ersten Replikation können Sie für jeden virtuellen Computer ein nicht geplantes Failover von AWS auf Azure durchführen. Optional können Sie einen Wiederherstellungsplan erstellen und ein nicht geplantes Failover durchführen, um mehrere virtuelle Computer von AWS zu Azure zu migrieren. [hier](site-recovery-create-recovery-plans.md) .

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu anderen Replikationsszenarien finden Sie in [Was ist Azure Site Recovery?](site-recovery-overview.md)



<!--HONumber=Nov16_HO3-->


