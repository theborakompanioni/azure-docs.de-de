---
title: Migrieren virtueller Computer von AWS zu Azure | Microsoft-Dokumentation
description: "Dieser Artikel beschreibt, wie Sie in Amazon Web Services (AWS) ausgeführte virtuelle Computer mithilfe von Azure Site Recovery zu Azure migrieren."
services: site-recovery
documentationcenter: 
author: bsiva
manager: jwhit
editor: 
ms.assetid: ddb412fd-32a8-4afa-9e39-738b11b91118
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 02/12/2017
ms.author: bsiva
translationtype: Human Translation
ms.sourcegitcommit: 5031f64ffcd34b6287a3ecd87dd027c2bc7c716f
ms.openlocfilehash: 9d0d0ba4ca5966b39ce62ea8296d48e5930c9782


---
# <a name="migrate-virtual-machines-in-amazon-web-services-aws-to-azure-with-azure-site-recovery"></a>Migrieren virtueller Computer von Amazon Web Services (AWS) zu Azure mit Azure Site Recovery

Dieser Artikel beschreibt, wie Sie in AWS Windows-Instanzen mithilfe von [Azure Site Recovery](site-recovery-overview.md) zu virtuellen Computern in Azure migrieren.

Die Migration ist eigentlich ein Failover von AWS zu Azure. Ein Failback der Computer ist nicht möglich, und es gibt keine laufende Replikation. Dieser Artikel beschreibt die Schritte für die Migration im Azure-Portal, und er basiert auf den Anweisungen für die [Replikation eines physischen Computers in Azure](site-recovery-vmware-to-azure.md).

Kommentare oder Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)

## <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme

Site Recovery kann zum Migrieren von EC2-Instanzen verwendet werden, die unter einem der folgenden Betriebssysteme ausgeführt werden:

- Windows (nur&64; Bit)
    - Windows Server 2008 R2 SP1+ (nur Citrix PV- oder AWS PV-Treiber. **Instanzen mit RedHat PV-Treibern werden nicht unterstützt.**) Windows Server 2012, Windows Server 2012 R2
- Linux (nur&64; Bit)
    - Red Hat Enterprise Linux 6.7 (nur virtualisierte HVM-Instanzen)

## <a name="prerequisites"></a>Voraussetzungen

Für diese Bereitstellung benötigen Sie Folgendes:

* **Konfigurationsserver**: Ein lokaler virtueller Computer unter Windows Server 2012 R2 wird als Konfigurationsserver bereitgestellt. Standardmäßig werden die anderen Site Recovery-Komponenten (Prozessserver und Masterzielserver) beim Bereitstellen des Konfigurationsservers installiert. [Weitere Informationen](site-recovery-components.md#replicate-vmware-vmsphysical-servers-to-azure)
* **EC2-Instanzen**: Die EC2 Instanzen von virtuellen Computern, die Sie migrieren möchten.

## <a name="deployment-steps"></a>Bereitstellungsschritte

1. Erstellen eines Tresors
2. Bereitstellen des Konfigurationsservers
3. Vergewissern Sie sich nach dem Bereitstellen des Konfigurationsservers, dass er mit den zu migrierenden virtuellen Computern kommunizieren kann.
4. Richten Sie Replikationseinstellungen ein.
5. Installieren Sie den Mobilitätsdienst. Auf jedem virtuellen Computer, den Sie schützen möchten, muss der Mobilitätsdienst installiert sein. Dieser Dienst sendet Daten an den Prozessserver. Der Mobilitätsdienst kann manuell installiert oder mittels Push automatisch durch den Prozessserver installiert werden, wenn der Schutz für den virtuellen Computer aktiviert wird. Konfigurieren Sie Firewallregeln für die zu migrierenden EC2-Instanzen, um die Push-Installation des Diensts zu ermöglichen. Die Sicherheitsgruppe für EC2-Instanzen muss über folgende Regeln verfügen:

    ![Firewallregeln](./media/site-recovery-migrate-aws-to-azure/migrate-firewall.png)
6. Aktivieren Sie die Replikation. Aktivieren Sie die Replikation für die virtuellen Computer, die Sie migrieren möchten. Die EC2-Instanzen können anhand der privaten IP-Adressen ermittelt werden, die Sie über die EC2-Konsole abrufen können.
7. Führen Sie ein ungeplantes Failover durch. Nach Abschluss der ersten Replikation können Sie für jeden virtuellen Computer ein nicht geplantes Failover von AWS auf Azure durchführen. Optional können Sie einen Wiederherstellungsplan erstellen und ein nicht geplantes Failover durchführen, um mehrere virtuelle Computer von AWS zu Azure zu migrieren. [hier](site-recovery-create-recovery-plans.md) .

Sehen Sie sich detaillierte Anweisungen für die [Bereitstellungsschritte](site-recovery-vmware-to-azure.md) und für die Ausführung eines [ungeplanten Failovers](site-recovery-failover.md#run-an-unplanned-failover) an.



<!--HONumber=Feb17_HO2-->


