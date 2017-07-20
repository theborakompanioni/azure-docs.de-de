---
title: 'Replizieren von Azure-VMs zwischen Azure-Regionen zur Notfallwiederherstellung: von Azure nach Azure | Microsoft-Dokumentation'
description: In diesem Artikel werden die Schritte zusammengefasst, um zur Notfallwiederherstellung Azure-VMs zwischen Azure-Regionen (von Azure nach Azure) mit dem Azure Site Recovery-Dienst zu replizieren.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: dab98aa5-9c41-4475-b7dc-2e07ab1cfd18
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: a1ba750d2be1969bfcd4085a24b0469f72a357ad
ms.openlocfilehash: 9ca33057f6030fdcc233f6053fdc392d62f8f9f4
ms.contentlocale: de-de
ms.lasthandoff: 06/20/2017


---

# <a name="replicate-azure-vms-between-regions-with-azure-site-recovery"></a>Replizieren von Azure-VMs zwischen Regionen mit Azure Site Recovery

>[!NOTE]
>
> Die Azure Site Recovery-Replikation für virtuelle Azure-Computer (VMs) ist derzeit in der Vorschau verfügbar.

In diesem Artikel erfahren Sie, wie Sie mit dem [Site Recovery](site-recovery-overview.md)-Dienst Azure-VMs zwischen Azure-Regionen über das Azure-Portal replizieren.

Kommentare und Fragen können Sie am Ende dieses Artikels oder im [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.

## <a name="disaster-recovery-in-azure"></a>Notfallwiederherstellung in Azure

Die integrierten Funktionen der Azure-Infrastruktur fördern eine zuverlässige und stabile Verfügbarkeitsstrategie für Workloads, die auf Azure-VMs ausgeführt werden. Es gibt jedoch zahlreiche Gründe, warum Sie die Notfallwiederherstellung zwischen Azure-Regionen selbst planen müssen:

- Sie müssen Konformitätsrichtlinien für bestimmte Apps und Workloads erfüllen, die eine Strategie für Geschäftskontinuität und Notfallwiederherstellung (Business Continuity and Disaster Recovery, BCDR) erfordern.
- Sie möchten die Möglichkeit haben, Azure-VMs basierend auf Ihren geschäftlichen Entscheidungen zu schützen und wiederherzustellen, nicht nur basierend auf den integrierten Azure-Funktionen.
- Sie müssen Failover- und Wiederherstellungsvorgänge gemäß Ihren Geschäfts- und Konformitätsanforderungen ohne Auswirkungen auf die Produktion testen.
- Sie müssen im Falle eines Notfalls ein nahtloses Failover auf den Bereich für die Wiederherstellung und ein Failback auf die ursprüngliche Quellregion ausführen.

Verwenden Sie Site Recovery für die VM-Replikation mit Azure als Quelle und Ziel, um all diese Aufgaben durchzuführen.


## <a name="why-use-site-recovery"></a>Gründe für Azure Site Recovery      

Site Recovery bietet eine einfache Möglichkeit, um Azure-VMs zwischen Regionen zu replizieren:

- **Automatische Bereitstellung**. Im Gegensatz zu einem Aktiv/Aktiv-Replikationsmodell ist keine aufwändige und komplexe Infrastruktur in der sekundären Region erforderlich. Wenn Sie die Replikation aktivieren, erstellt Site Recovery basierend auf den Quellregionseinstellungen automatisch die erforderlichen Ressourcen in der Zielregion.
- **Steuern von Regionen**. Mit Site Recovery können Sie eine beliebige Region in einer beliebigen Region auf einem Kontinent replizieren. Vergleichen Sie dies mit georedundanten Speichern mit Lesezugriff, die lediglich eine asynchrone Replikation zwischen standardmäßig [gekoppelten Regionen](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) durchführen. Georedundante Speicher mit Lesezugriff bieten einen schreibgeschützten Zugriff auf die Daten in der Zielregion.
- **Automatisierte Replikation**. Site Recovery bietet eine automatisierte fortlaufende Replikation. Failover und Failback können durch einen einzigen Mausklick ausgelöst werden.
- **RTO und RPO**. Site Recovery macht sich die Funktion zur Verbindung von Regionen der Azure-Netzwerkinfrastruktur zunutze, um die RTO und RPO möglichst gering zu halten.
- **Testen**. Bei Bedarf können Sie jederzeit durch bedarfsgesteuerte Testfailovers ein Drilldown auf Notfallwiederherstellungen ausführen, ohne dass Ihre Produktionsworkloads oder die laufende Replikation beeinträchtigt wird.
- **Wiederherstellungspläne**. Mithilfe von Wiederherstellungsplänen können Sie das Failover und Failback der gesamten Anwendung orchestrieren, die auf mehreren VMs ausgeführt wird. Die Funktion des Wiederherstellungsplans bietet eine umfangreiche erstklassige Integration in Azure Automation-Runbooks.


## <a name="deployment-summary"></a>Zusammenfassung der Bereitstellungen

Im Folgenden wird zusammengefasst, wie Sie die Replikation von VMs zwischen Azure-Regionen einrichten:

1. Erstellen Sie einen Recovery Services-Tresor. Der Tresor enthält Konfigurationseinstellungen und dient zum Orchestrieren der Replikation.
2. Aktivieren Sie die Replikation für die Azure-VMs.
3. Führen Sie ein Testfailover aus, um sicherzustellen, dass alles wie erwartet funktioniert.

>[!IMPORTANT]
>
> Sie können sich die [Supportmatrix für die Azure-VM-Replikation](./site-recovery-support-matrix-azure-to-azure.md) ansehen.

>[!IMPORTANT]
>
> Informationen zum Konfigurieren der erforderlichen ausgehenden Netzwerkverbindungen für Azure-VMs für die Site Recovery-Replikation finden Sie im [Dokument zum Leitfaden für Netzwerke](./site-recovery-azure-to-azure-networking-guidance.md).

### <a name="before-you-start"></a>Vorbereitung

* Ihrem Azure-Benutzerkonto müssen bestimmte [Berechtigungen](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) zum Aktivieren der Replikation eines virtuellen Azure-Computers zugewiesen sein.
* Ihr Azure-Abonnement sollte zum Erstellen von VMs am Zielstandort aktiviert sein, den Sie als Region für die Notfallwiederherstellung verwenden möchten. Wenden Sie sich an den Support, um das erforderliche Kontingent zu aktivieren.

## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

>[!NOTE]
>
> Es wird empfohlen, dass Sie den Recovery Services-Tresor an dem Standort erstellen, an dem Ihre VMs repliziert werden sollen. Wenn Ihr Zielstandort beispielsweise „USA, Mitte“ ist, erstellen Sie den Tresor **USA, Mitte**.

## <a name="enable-replication"></a>Replikation aktivieren

Klicken Sie unter **Recovery Services-Tresore** auf den Tresornamen. Klicken Sie im Tresor auf die Schaltfläche **+Replizieren**.

### <a name="step-1-configure-the-source"></a>Schritt 1: Konfigurieren der Quelle
1. Wählen Sie unter **Quelle** die Option **Azure – VORSCHAU**.
2. Wählen Sie unter **Quellstandort** die Azure-Quellregion aus, in der Ihre VMs derzeit ausgeführt werden.
3. Wählen Sie das Bereitstellungsmodell Ihrer VMs: **Resource Manager** oder **Klassisch**.
4. Wählen Sie für Resource Manager-VMs die **Quellressourcengruppe** oder für klassische VMs den **Clouddienst**.

    ![Konfigurieren der Quelle](./media/site-recovery-azure-to-azure/source.png)

### <a name="step-2-select-virtual-machines"></a>Schritt 2: Virtuelle Computer auswählen

* Wählen Sie die zu replizierenden VMs aus, und klicken Sie dann auf **OK**.

    ![Auswählen von VMs](./media/site-recovery-azure-to-azure/vms.png)

### <a name="step-3-configure-settings"></a>Schritt 3: Konfigurieren von Einstellungen

1. Um die standardmäßigen Zieleinstellungen außer Kraft zu setzen und die Einstellungen Ihrer Wahl festzulegen, klicken Sie auf **Anpassen**. Weitere Informationen finden Sie unter [Anpassen von Zielressourcen](site-recovery-replicate-azure-to-azure.md##customize-target-resources).

    ![Konfigurieren von Einstellungen](./media/site-recovery-azure-to-azure/settings.png)


2. Site Recovery erstellt standardmäßig eine Replikationsrichtlinie, die alle 4 Stunden App-konsistente Momentaufnahmen erstellt und Wiederherstellungspunkte 24 Stunden lang behält. Klicken Sie zum Erstellen einer Richtlinie mit unterschiedlichen Einstellungen neben **Replikationsrichtlinie** auf **Anpassen**.

    ![Anpassen der Richtlinie](./media/site-recovery-azure-to-azure/customize-policy.png)

3. Um mit der Bereitstellung der Zielressourcen zu beginnen, klicken Sie auf **Zielressourcen erstellen**. Die Bereitstellung dauert ungefähr einige Minuten. Schließen Sie nicht das Blatt während der Bereitstellung. Anderenfalls müssen Sie den Vorgang wiederholen.

4. Um die Replikation der ausgewählten VM auszulösen, klicken Sie auf **Replikation aktivieren**.

5. Sie können den Fortschritt des Auftrags **Schutz aktivieren** unter **Einstellungen** > **Aufträge** > **Site Recovery-Aufträge** verfolgen.

6. Unter **Einstellungen** > **Replizierte Elemente** können Sie den Status der VMs und den der ersten Replikation einsehen. Klicken Sie auf die VM, um ein Drilldown auf die zugehörigen Einstellungen auszuführen.

## <a name="run-a-test-failover"></a>Durchführen eines Test-Failovers

Führen Sie nach Abschluss der Einrichtung ein Testfailover durch, um sicherzustellen, dass alles wie erwartet funktioniert:

1. Klicken Sie zum Ausführen eines Failovers für einen einzelnen Computer unter **Einstellungen** > **Replizierte Elemente** auf die VM und dann auf das Symbol **+Testfailover**.

2. Klicken Sie zum Ausführen eines Failovers für einen Wiederherstellungsplan unter **Einstellungen** > **Wiederherstellungspläne** mit der rechten Maustaste auf den Plan **Testfailover**. Eine Anleitung zum Erstellen eines Wiederherstellungsplans finden Sie [hier](site-recovery-create-recovery-plans.md). 

3. Wählen Sie unter **Testfailover** das virtuelle Azure-Zielnetzwerk aus, mit dem Azure-VMs nach dem Failover verbunden werden.

4. Um das Failover zu starten, klicken Sie auf **OK**. Klicken Sie zum Überwachen des Fortschritts auf die VM, um die Eigenschaften aufzurufen. Alternativ können Sie auf den Auftrag **Testfailover** unter dem Tresornamen auf **Einstellungen** > **Aufträge** > **Site Recovery-Aufträge** klicken.

5. Nach Abschluss des Failovers wird der Azure-Replikatcomputer im Azure-Portal unter **Virtuelle Computer** angezeigt. Stellen Sie sicher, dass die VM die richtige Größe hat, mit dem richtigen Netzwerk verbunden ist und ausgeführt wird.

6. Um die VMs zu löschen, die beim Testfailover erstellt wurden, klicken Sie im replizierten Element oder im Wiederherstellungsplan auf **Testfailover bereinigen**. Erfassen und speichern Sie unter **Notizen** alle Beobachtungen im Zusammenhang mit dem Testfailover. 

[Erfahren Sie mehr](site-recovery-test-failover-to-azure.md) über Testfailover.


## <a name="next-steps"></a>Nächste Schritte

Nach dem Testen der Bereitstellung:

- [Informieren Sie sich](site-recovery-failover.md) über die unterschiedlichen Failoverarten und deren Durchführung.
- Erfahren Sie mehr über die [Verwendung von Wiederherstellungsplänen](site-recovery-create-recovery-plans.md) zur Verringerung der RTO.

