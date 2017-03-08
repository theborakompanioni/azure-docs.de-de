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
ms.sourcegitcommit: c33b69dbdccd2a894b494175dec2bd423af478b1
ms.openlocfilehash: 61909f588077f3bbdbaf368db1a9138a3b28b117
ms.lasthandoff: 03/02/2017


---
# <a name="migrate-virtual-machines-in-amazon-web-services-aws-to-azure-with-azure-site-recovery"></a>Migrieren virtueller Computer von Amazon Web Services (AWS) zu Azure mit Azure Site Recovery

Dieser Artikel beschreibt, wie Sie in AWS Windows-Instanzen mithilfe von [Azure Site Recovery](site-recovery-overview.md) zu virtuellen Computern in Azure migrieren.

Die Migration ist eigentlich ein Failover von AWS zu Azure. Ein Failback der Computer in AWS ist nicht möglich, und es gibt keine laufende Replikation. Dieser Artikel beschreibt die Schritte für die Migration im Azure-Portal, und er basiert auf den Anweisungen für die [Replikation eines physischen Computers in Azure](site-recovery-vmware-to-azure.md).

Kommentare oder Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)

## <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme

Site Recovery kann zum Migrieren von EC2-Instanzen verwendet werden, die unter einem der folgenden Betriebssysteme ausgeführt werden:

- Windows (nur&64; Bit)
    - Windows Server 2008 R2 SP1+ (nur Citrix PV- oder AWS PV-Treiber. **Instanzen mit RedHat PV-Treibern werden nicht unterstützt.**) Windows Server 2012, Windows Server 2012 R2
- Linux (nur&64; Bit)
    - Red Hat Enterprise Linux 6.7 (nur virtualisierte HVM-Instanzen)

## <a name="prerequisites"></a>Voraussetzungen

Für diese Bereitstellung benötigen Sie Folgendes:

* **Konfigurationsserver:** Ein virtueller Amazon EC2-Computer unter Windows Server 2012 R2 wird als Konfigurationsserver bereitgestellt. Standardmäßig werden die anderen Azure Site Recovery-Komponenten (Prozessserver und Masterzielserver) beim Bereitstellen des Konfigurationsservers installiert. Dieser Artikel beschreibt die Schritte für die Migration im Azure-Portal. Er basiert auf den Anweisungen unter [Weitere Informationen](site-recovery-components.md#vmware-replication-to-azure).

* **EC2-Instanzen:** Die Amazon EC2-Instanzen von virtuellen Computern, die Sie migrieren möchten.

## <a name="deployment-steps"></a>Bereitstellungsschritte

1. Erstellen eines Recovery Services-Tresors

2. Für die Sicherheitsgruppe Ihrer EC2-Instanzen müssen folgende Regeln konfiguriert sein: ![Regeln](./media/site-recovery-migrate-aws-to-azure/migration_pic1.png)

3. Stellen Sie in der gleichen virtuellen privaten Amazon-Cloud wie Ihre EC2-Instanzen einen ASR-Konfigurationsserver bereit. Lesen Sie auch die Informationen zu den Voraussetzungen für Azure bei VMware oder physischen Computern für die Bereitstellung von Konfigurationsservern unter ![DeployCS](./media/site-recovery-migrate-aws-to-azure/migration_pic2.png).

4.    Nachdem Ihr Konfigurationsserver in AWS bereitgestellt und bei Ihrem Recovery Services-Tresor registriert wurde, sollten der Konfigurationsserver und der Prozessserver wie unten dargestellt in der Site Recovery-Infrastruktur angezeigt werden: ![CSinVault](./media/site-recovery-migrate-aws-to-azure/migration_pic3.png)
  >[!NOTE]
  >Es kann bis zu 15 Minuten dauern, bis Konfigurationsserver und Prozessserver angezeigt werden.
  >

5. Vergewissern Sie sich nach dem Bereitstellen des Konfigurationsservers, dass er mit den zu migrierenden virtuellen Computern kommunizieren kann.

6. [Einrichten der Replikationseinstellungen](site-recovery-setup-replication-settings-vmware.md)

7. Aktivieren der Replikation: Aktivieren Sie die Replikation für die virtuellen Computer, die Sie migrieren möchten. Die EC2-Instanzen können anhand der privaten IP-Adressen ermittelt werden, die Sie über die EC2-Konsole abrufen können.
![SelectVM](./media/site-recovery-migrate-aws-to-azure/migration_pic4.png)
8. Sobald die EC2-Instanzen geschützt wurden und die Replikation in Azure abgeschlossen ist, [führen Sie ein Testfailover](site-recovery-test-failover-to-azure.md) durch, um die Leistung Ihrer Anwendung in Azure zu überprüfen. ![TFI](./media/site-recovery-migrate-aws-to-azure/migration_pic5.png)

9. Führen Sie ein Failover von AWS nach Azure für jeden virtuellen Computer durch. Optional können Sie einen Wiederherstellungsplan erstellen und ein Failover durchführen, um mehrere virtuelle Computer von AWS zu Azure zu migrieren. [hier](site-recovery-create-recovery-plans.md) .

10. Für die Migration müssen Sie kein Commit für ein Failover durchführen. Wählen Sie stattdessen die Option „Migration abschließen“ für jeden Computer aus, den Sie migrieren möchten. Mit der Aktion „Migration abschließen“ wird der Migrationsprozess abgeschlossen, die Replikation für den Computer wird entfernt, und die Site Recovery-Berechnung von Kosten für den Computer wird beendet.![Migrieren](./media/site-recovery-migrate-aws-to-azure/migration_pic6.png)

