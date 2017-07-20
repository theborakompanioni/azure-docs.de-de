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
ms.workload: storage-backup-recovery
ms.date: 05/31/2017
ms.author: bsiva
ms.translationtype: Human Translation
ms.sourcegitcommit: 31ecec607c78da2253fcf16b3638cc716ba3ab89
ms.openlocfilehash: b2420da03b83a355215d7beeffd5b4cff10da75b
ms.contentlocale: de-de
ms.lasthandoff: 06/23/2017


---
# <a name="migrate-virtual-machines-in-amazon-web-services-aws-to-azure-with-azure-site-recovery"></a>Migrieren virtueller Computer von Amazon Web Services (AWS) zu Azure mit Azure Site Recovery

Dieser Artikel beschreibt, wie Sie in AWS Windows-Instanzen mithilfe von [Azure Site Recovery](site-recovery-overview.md) zu virtuellen Computern in Azure migrieren.

Die Migration ist eigentlich ein Failover von AWS zu Azure. Ein Failback der Computer in AWS ist nicht möglich, und es gibt keine laufende Replikation. Dieser Artikel beschreibt die Schritte für die Migration im Azure-Portal, und er basiert auf den Anweisungen für die [Replikation eines physischen Computers in Azure](site-recovery-vmware-to-azure.md).

Kommentare oder Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)

## <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme

Site Recovery kann zum Migrieren von EC2-Instanzen verwendet werden, die unter einem der folgenden Betriebssysteme ausgeführt werden:

- Windows (nur 64 Bit)
    - Windows Server 2008 R2 SP1+ (nur Citrix PV- oder AWS PV-Treiber. **Instanzen mit RedHat PV-Treibern werden nicht unterstützt.**) Windows Server 2012, Windows Server 2012 R2
- Linux (nur 64 Bit)
    - Red Hat Enterprise Linux 6.7 (nur virtualisierte HVM-Instanzen)

## <a name="prerequisites"></a>Voraussetzungen

Für diese Bereitstellung benötigen Sie Folgendes:

* **Konfigurationsserver:** Ein virtueller Amazon EC2-Computer unter Windows Server 2012 R2 wird als Konfigurationsserver bereitgestellt. Standardmäßig werden die anderen Azure Site Recovery-Komponenten (Prozessserver und Masterzielserver) beim Bereitstellen des Konfigurationsservers installiert. Dieser Artikel beschreibt die Schritte für die Migration im Azure-Portal. Er basiert auf den Anweisungen unter [Weitere Informationen](site-recovery-components.md#vmware-to-azure).

* **EC2-Instanzen:** Die Amazon EC2-Instanzen von virtuellen Computern, die Sie migrieren möchten.

## <a name="deployment-steps"></a>Bereitstellungsschritte

1. Erstellen Sie einen Recovery Services-Tresor.
2. Die Sicherheitsgruppe Ihrer EC2-Instanzen muss über Regeln verfügen, deren Konfiguration eine Kommunikation zwischen der zu migrierenden EC2-Instanz und der Instanz ermöglicht, für die die Bereitstellung auf dem Konfigurationsserver vorgesehen ist.

3. Stellen Sie in der gleichen virtuellen privaten Amazon-Cloud wie Ihre EC2-Instanzen einen ASR-Konfigurationsserver bereit. Lesen Sie die Informationen zu den Voraussetzungen für Azure bei VMware oder physischen Computern für die Bereitstellung von Konfigurationsservern.

    ![DeployCS](./media/site-recovery-migrate-aws-to-azure/migration_pic2.png)

4.  Nachdem Ihr Konfigurationsserver in AWS bereitgestellt und bei Ihrem Recovery Services-Tresor registriert wurde, sollten der Konfigurationsserver und der Prozessserver wie unten dargestellt in der Site Recovery-Infrastruktur angezeigt werden:

    ![CSinVault](./media/site-recovery-migrate-aws-to-azure/migration_pic3.png)

5. Vergewissern Sie sich nach dem Bereitstellen des Konfigurationsservers, dass er mit den zu migrierenden VMs kommunizieren kann. (Es kann bis zu 15 Minuten dauern, bis der Konfigurationsserver angezeigt wird.)

6. [Richten Sie Replikationseinstellungen ein](site-recovery-setup-replication-settings-vmware.md).

7. Aktivieren der Replikation: Aktivieren Sie die Replikation für die virtuellen Computer, die Sie migrieren möchten. Die EC2-Instanzen können anhand der privaten IP-Adressen ermittelt werden, die Sie über die EC2-Konsole abrufen können.

    ![SelectVM](./media/site-recovery-migrate-aws-to-azure/migration_pic4.png)

8. Sobald die EC2-Instanzen geschützt wurden und die Replikation in Azure abgeschlossen ist, [führen Sie ein Testfailover aus](site-recovery-test-failover-to-azure.md), um die Leistung Ihrer Anwendung in Azure zu überprüfen.

    ![TFI](./media/site-recovery-migrate-aws-to-azure/migration_pic5.png)

9. Führen Sie ein Failover von AWS nach Azure für jeden virtuellen Computer durch. Optional können Sie einen Wiederherstellungsplan erstellen und ein Failover durchführen, um mehrere virtuelle Computer von AWS zu Azure zu migrieren. [hier](site-recovery-create-recovery-plans.md) .

10. Für die Migration müssen Sie kein Commit für ein Failover durchführen. Wählen Sie stattdessen die Option „Migration abschließen“ für jeden Computer aus, den Sie migrieren möchten. Mit der Aktion „Migration abschließen“ wird der Migrationsprozess abgeschlossen, die Replikation für den Computer wird entfernt, und die Site Recovery-Berechnung von Kosten für den Computer wird beendet.

    ![Migrieren](./media/site-recovery-migrate-aws-to-azure/migration_pic6.png)

## <a name="next-steps"></a>Nächste Schritte

- [Vorbereiten der migrierten Computer zum Ermöglichen der Replikation](site-recovery-azure-to-azure-after-migration.md) in einer anderen Region für die Notfallwiederherstellung
- Schützen von Workloads durch die [Replikation virtueller Azure-Computer](site-recovery-azure-to-azure.md)

