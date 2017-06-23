---
title: Replizieren virtueller Hyper-V-Computer in Azure | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie die Replikation, das Failover und die Wiederherstellung von lokalen virtuellen Hyper-V-Computern zu Azure orchestrieren.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 1777e0eb-accb-42b5-a747-11272e131a52
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 04/05/2017
ms.author: raynew
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: hyper-v-site-walkthrough-overview
ms.translationtype: Human Translation
ms.sourcegitcommit: c308183ffe6a01f4d4bf6f5817945629cbcedc92
ms.openlocfilehash: 0ef782a7bb7a98da2ec63c91732b3d5ddd959848
ms.contentlocale: de-de
ms.lasthandoff: 05/17/2017

---


# <a name="replicate-hyper-v-virtual-machines-without-vmm-to-azure-using-azure-site-recovery-with-the-azure-portal"></a>Replizieren von virtuellen Hyper-V-Computern (ohne VMM) nach Azure per Azure Site Recovery über das Azure-Portal

> [!div class="op_single_selector"]
> * [Azure-Portal](site-recovery-hyper-v-site-to-azure.md)
> * [Klassisches Azure](site-recovery-hyper-v-site-to-azure-classic.md)
> * [PowerShell – Resource Manager](site-recovery-deploy-with-powershell-resource-manager.md)
>
>

In diesem Artikel erfahren Sie, wie Sie lokale virtuelle Hyper-V-Computer mithilfe von [Azure Site Recovery](site-recovery-overview.md) über das Azure-Portal zu Azure replizieren können. In dieser Bereitstellung werden Hyper-V-VMs nicht von System Center Virtual Machine Manager (VMM) verwaltet.

Nach der Lektüre dieses Artikels können Sie Kommentare am Ende des Artikels oder technische Fragen im [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.

Wenn Sie Computer zu Azure migrieren möchten (ohne Failback), erfahren Sie [in diesem Artikel](site-recovery-migrate-to-azure.md) mehr darüber.



## <a name="deployment-steps"></a>Bereitstellungsschritte

Führen Sie anhand des Artikels diese Bereitstellungsschritte aus:

1. [Erfahren Sie mehr](site-recovery-components.md#hyper-v-to-azure) über die Architektur für diese Bereitstellung. [Informieren Sie sich](site-recovery-hyper-v-azure-architecture.md) darüber hinaus über die Funktionsweise der Hyper-V-Replikation in Site Recovery.
2. Überprüfen Sie die Voraussetzungen und Einschränkungen.
3. Richten Sie Azure-Netzwerk- und -Speicherkonten ein.
4. Bereiten Sie die Hyper-V-Hosts vor.
5. Erstellen Sie einen Recovery Services-Tresor. Der Tresor enthält Konfigurationseinstellungen und dient zum Orchestrieren der Replikation.
6. Geben Sie die Einstellungen für die Datenquelle an. Erstellen Sie eine Hyper-V-Site, die Hyper-V-Hosts enthält, und registrieren Sie die Site im Tresor. Installieren Sie den Azure Site Recovery-Anbieter und den Microsoft Recovery Services-Agent auf den Hyper-V-Hosts.
7. Geben Sie die Ziel- und Replikationseinstellungen an.
8. Aktivieren Sie die Replikation für die VMs.
9. Führen Sie ein Testfailover aus, um sicherzustellen, dass alles wie erwartet funktioniert.



## <a name="prerequisites"></a>Voraussetzungen


**Anforderung** | **Details** |
--- | --- |
**Azure** | Informieren Sie sich über die [Azure-Anforderungen](site-recovery-prereq.md#azure-requirements).
**Lokale Server** | [Erfahren Sie mehr](site-recovery-prereq.md#disaster-recovery-of-hyper-v-virtual-machines-to-azure-no-virtual-machine-manager) zu den Anforderungen für den lokalen VMM-Server und die Hyper-V-Hosts.
**Lokale Hyper-V-VMs** | Auf VMs, die Sie replizieren möchten, sollte ein [unterstütztes Betriebssystem](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) ausgeführt werden, das die [Azure-Voraussetzungen](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) erfüllt.
**Azure-URLs** | Hyper-V-Hosts benötigen Zugriff auf die folgenden URLs:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> Stellen Sie bei Verwendung von auf IP-Adressen basierenden Firewallregeln sicher, dass hierfür die Kommunikation mit Azure möglich ist.<br/></br> Lassen Sie die [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653) (IP-Bereiche für Azure-Rechenzentren) und den HTTPS-Port (443) zu.<br/></br> Lassen Sie IP-Adressbereiche für die Azure-Region Ihres Abonnements und für die Region „USA, Westen“ (wird für Zugriffsteuerung und Identitätsverwaltung verwendet) zu.



## <a name="prepare-for-deployment"></a>Vorbereiten der Bereitstellung

Für die Vorbereitung der Bereitstellung benötigen Sie Folgendes:

1. [Richten Sie ein Azure-Netzwerk ein](#set-up-an-azure-network) , in dem sich Azure-VMs befinden, wenn sie nach dem Failover erstellt werden.
2. [Richten Sie ein Azure-Speicherkonto ein](#set-up-an-azure-storage-account) , das für replizierte Daten verwendet werden kann.
3. [Bereiten Sie die Hyper-V-Hosts vor](#prepare-the-hyper-v-hosts), um sicherzustellen, dass diese auf die erforderlichen URLs zugreifen können.

### <a name="set-up-an-azure-network"></a>Richten Sie ein Azure-Netzwerk ein

Richten Sie ein Azure-Netzwerk ein. Sie benötigen dies, damit die nach dem Failover erstellten Azure-VMs mit einem Netzwerk verbunden sind.

* Das Netzwerk muss sich in der gleichen Region befinden wie der Recovery Services-Tresor.
* Richten Sie das Azure-Netzwerk im [Resource Manager-Modus](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) oder im [klassischen Modus](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) ein (je nachdem, welches Ressourcenmodell Sie für virtuelle Azure-Computer nach dem Failover verwenden möchten).
* Wir empfehlen Ihnen, ein Netzwerk einzurichten, bevor Sie beginnen. Falls Sie es nicht tun, müssen Sie diesen Schritt während der Site Recovery-Bereitstellung ausführen.
- Von Site Recovery verwendete Azure-Speicherkonten können nicht innerhalb eines Abonnements oder über mehrere Abonnements hinweg [verschoben](../azure-resource-manager/resource-group-move-resources.md) werden.


### <a name="set-up-an-azure-storage-account"></a>Richten Sie ein Azure-Speicherkonto ein

- Sie benötigen ein Standard-/Premium-Azure-Speicherkonto zum Speichern nach Azure replizierter Daten. [Storage Premium](../storage/storage-premium-storage.md) wird normalerweise für virtuelle Computer verwendet, die eine konsistent hohe E/A-Leistung und geringe Latenz zum Hosten von E/A-intensiven Workloads erfordern.
- Wenn Sie ein Premiumkonto zum Speichern replizierter Daten verwenden möchten, benötigen Sie außerdem ein Standard-Speicherkonto zum Speichern von Replikationsprotokollen, in denen laufende Änderungen lokaler Daten erfasst werden.
- Richten Sie ein Konto im [Resource Manager-Modus](../storage/storage-create-storage-account.md) oder im [klassischen Modus](../storage/storage-create-storage-account-classic-portal.md) ein (je nachdem, welches Ressourcenmodell Sie für virtuelle Azure-Computer nach dem Failover verwenden möchten).
- Es wird empfohlen, ein Speicherkonto einzurichten, bevor Sie beginnen. Falls Sie es nicht tun, müssen Sie diesen Schritt während der Site Recovery-Bereitstellung ausführen. Die Konten müssen sich in der gleichen Region wie der Recovery Services-Tresor befinden.
- Speicherkonten, die von Site Recovery verwendet werden, können nicht über Ressourcengruppen hinweg im gleichen Abonnement oder über verschiedene Abonnements hinweg verschoben werden.


### <a name="prepare-the-hyper-v-hosts"></a>Bereiten Sie die Hyper-V-Hosts vor

* Stellen Sie sicher, dass die Hyper-V-Hosts die [Voraussetzungen](site-recovery-prereq.md#disaster-recovery-of-hyper-v-virtual-machines-to-azure-no-virtual-machine-manager) erfüllen.
- Stellen Sie sicher, dass die Hosts auf die erforderlichen URLs zugreifen können.


## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie auf **Neu** > **Überwachung und Verwaltung** > **Backup und Site Recovery (OMS)**.  

    ![Neuer Tresor](./media/site-recovery-hyper-v-site-to-azure/new-vault.png)

3. Geben Sie unter **Name**einen Anzeigenamen an, mit dem der Tresor identifiziert wird. Wenn Sie mehrere Abonnements haben, müssen Sie ein Abonnement auswählen.

4. [Erstellen Sie eine neue Ressourcengruppe](../azure-resource-manager/resource-group-template-deploy-portal.md) , oder wählen Sie eine vorhandene Ressourcengruppe aus, und geben Sie eine Azure-Region an. Computer werden in dieser Region repliziert. Eine Liste mit den unterstützten Regionen finden Sie in den [Preisdetails zu Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/) unter „Geografische Verfügbarkeit“.

5. Wenn Sie über das Dashboard schnell auf den Tresor zugreifen möchten, klicken Sie auf **An Dashboard anheften** und anschließend auf **Erstellen**.

    ![Neuer Tresor](./media/site-recovery-hyper-v-site-to-azure/new-vault-settings.png)

Der neue Tresor wird unter **Dashboard** > **Alle Ressourcen** und auf dem Hauptblatt **Recovery Services-Tresore** angezeigt.



## <a name="select-the-protection-goal"></a>Auswählen des Schutzziels

Wählen Sie aus, was Sie replizieren möchten und wohin die Daten repliziert werden sollen.

1. Wählen Sie unter **Recovery Services-Tresore** den Tresor aus.
2. Klicken Sie unter **Erste Schritte** auf **Site Recovery** > **Bereiten Sie die Infrastruktur vor** > **Schutzziel**.

    ![Ziele wählen](./media/site-recovery-hyper-v-site-to-azure/choose-goals.png)
3. Wählen Sie unter **Schutzziel** die Option **To Azure** (Zu Azure) und dann **Yes, with Hyper-V** (Ja, mit Hyper-V) aus. Wählen Sie **Nein** aus, um zu bestätigen, dass Sie VMM nicht verwenden. Klicken Sie dann auf **OK**.

    ![Ziele wählen](./media/site-recovery-hyper-v-site-to-azure/choose-goals2.png)

## <a name="set-up-the-source-environment"></a>Einrichten der Quellumgebung

Richten Sie die Hyper-V-Site ein, installieren Sie den Azure Site Recovery-Anbieter und den Azure Recovery Services-Agent auf Hyper-V-Hosts, und registrieren Sie die Site im Tresor.

1. Klicken Sie unter **Bereiten Sie die Infrastruktur vor** auf **Quelle**. Klicken Sie auf **+Hyper-V-Standort**, um einen neuen Hyper-V-Standort als Container für Ihre Hyper-V-Hosts oder -Cluster hinzuzufügen.

    ![Quelle einrichten](./media/site-recovery-hyper-v-site-to-azure/set-source1.png)
2. Geben Sie auf dem Blatt **Hyper-V-Site erstellen** einen Namen für die Site an. Klicken Sie dann auf **OK**. Wählen Sie jetzt die erstellte Site aus, und klicken Sie auf **+Hyper-V Server**, um der Site einen Server hinzuzufügen.

    ![Quelle einrichten](./media/site-recovery-hyper-v-site-to-azure/set-source2.png)

3. Vergewissern Sie sich, dass unter **Server hinzufügen** > **Servertyp** der Typ **Hyper-V Server** angezeigt wird.

    - Stellen Sie sicher, dass der Hyper-V-Server, den Sie hinzufügen möchten, die [Voraussetzungen](#on-premises-prerequisites) erfüllt und auf die angegebenen URLs zugreifen kann.
    - Laden Sie die Installationsdatei für den Azure Site Recovery-Anbieter herunter. Führen Sie diese Datei aus, um sowohl den Anbieter als auch den Recovery Services-Agent auf jedem Hyper-V-Host zu installieren.

    ![Quelle einrichten](./media/site-recovery-hyper-v-site-to-azure/set-source3.png)


## <a name="install-the-provider-and-agent"></a>Installieren Sie den Anbieter und den Agent.

1. Führen Sie die Setupdatei für den Anbieter auf jedem Host aus, den Sie zum Hyper-V-Standort hinzugefügt haben. Wenn Sie auf einem Hyper-V-Cluster installieren, führen Sie das Setup auf jedem Clusterknoten aus. Durch das Installieren und Registrieren jedes Hyper-V-Clusterknotens wird sichergestellt, dass virtuelle Computer geschützt sind, selbst wenn sie über Knoten hinweg migrieren.
2. Unter **Microsoft Update** können Sie Updates aktivieren, damit Updates für den Anbieter gemäß Ihrer Microsoft Update-Richtlinie installiert werden.
3. Ändern Sie unter **Installation** den Speicherort für die Anbieterinstallation, oder akzeptieren Sie den Standardspeicherort, und klicken Sie auf **Installieren**.
4. Klicken in **Tresoreinstellungen** auf **Durchsuchen**, um die Tresorschlüsseldatei auszuwählen, die Sie heruntergeladen haben. Geben Sie das Azure Site Recovery-Abonnement, den Tresornamen und den Hyper-V-Standort an, zu dem der Hyper-V-Server gehört.

    ![Serverregistrierung](./media/site-recovery-hyper-v-site-to-azure/provider3.png)

5. Geben Sie unter **Proxyeinstellungen** an, wie der auf Hyper-V-Hosts ausgeführte Anbieter eine Internetverbindung mit Azure Site Recovery herstellen soll.

    * Wenn der Anbieter eine direkte Verbindung herstellen soll, wählen Sie **Direkt mit Azure Site Recovery verbinden (ohne Proxyserver)**.
    * Wenn für den vorhandenen Proxy eine Authentifizierung erforderlich ist, oder wenn Sie einen benutzerdefinierten Proxy für die Anbieterverbindung verwenden möchten, wählen Sie **Unter Verwendung eines Proxyservers mit Azure Site Recovery verbinden** aus.
    * Wenn Sie einen Proxy verwenden:
        - Geben Sie Adresse, Port und Anmeldeinformationen ein.
        - Stellen Sie sicher, dass die in den [Voraussetzungen](#prerequisites) beschriebenen URLs vom Proxy zugelassen werden.

    ![Internet](./media/site-recovery-hyper-v-site-to-azure/provider7.PNG)

6. Klicken Sie nach Abschluss der Installation auf **Registrieren**, um den Server im Tresor zu registrieren.

    ![Installationsspeicherort](./media/site-recovery-hyper-v-site-to-azure/provider2.png)

7. Nach der Registrierung ruft Azure Site Recovery Metadaten vom Hyper-V-Server ab, und der Server wird in **Site Recovery-Infrastruktur** > **Hyper-V-Hosts** angezeigt.


## <a name="set-up-the-target-environment"></a>Einrichten der Zielumgebung

Geben Sie das Azure-Speicherkonto für die Replikation und das Azure-Netzwerk an, mit dem Azure-VMs nach dem Failover eine Verbindung herstellen.

1. Klicken Sie auf **Infrastruktur vorbereiten** > **Ziel**.
2. Wählen Sie das Abonnement und die Ressourcengruppe aus, in dem bzw. der Sie die Azure-VMs erstellen möchten, für die ein Failover durchgeführt wurde. Wählen Sie das Bereitstellungsmodell aus, das in Azure für die VMs verwendet werden soll (klassisch oder Resource Manager).

3. Site Recovery prüft, ob Sie über ein oder mehrere kompatible Azure-Speicherkonten und -Netzwerke verfügen.

    - Wenn Sie kein Speicherkonto haben, klicken Sie auf **+Speicher**, um ein Konto auf Resource Manager-Basis inline zu erstellen. Lesen Sie diese Informationen zu [Speicheranforderungen](site-recovery-prereq.md#azure-requirements).
    - Wenn Sie nicht über ein Azure-Netzwerk verfügen, klicken Sie auf **+Netzwerk**, um ein Netzwerk auf Resource Manager-Basis inline zu erstellen.

    ![Speicher](./media/site-recovery-vmware-to-azure/enable-rep3.png)




## <a name="configure-replication-settings"></a>Konfigurieren der Replikationseinstellungen

1. Klicken Sie zum Erstellen einer neuen Replikationsrichtlinie auf **Infrastruktur vorbereiten** > **Replikationseinstellungen** > **+Erstellen und zuordnen**.

    ![Netzwerk](./media/site-recovery-hyper-v-site-to-azure/gs-replication.png)
2. Geben Sie unter **Richtlinie erstellen und zuordnen**einen Richtliniennamen an.
3. Geben Sie unter **Kopierhäufigkeit**an, wie oft Sie Deltadaten nach der ersten Replikation replizieren möchten (alle 30 Sekunden, nach 5 Minuten oder nach 15 Minuten).

    > [!NOTE]
    > Eine Häufigkeit von 30 Sekunden wird bei der Replikation nach Storage Premium nicht unterstützt. Die Einschränkung richtet sich nach der Anzahl von Momentaufnahmen pro Blob (100), die von Storage Premium unterstützt wird. [Weitere Informationen](../storage/storage-premium-storage.md#snapshots-and-copy-blob).

4. Geben Sie unter **Aufbewahrungszeitraum des Wiederherstellungspunkts** das Aufbewahrungszeitfenster für die einzelnen Wiederherstellungspunkte in Stunden an. VMs können innerhalb eines Zeitfensters an einem beliebigen Punkt wiederhergestellt werden.
5. Geben Sie unter **App-konsistente Momentaufnahmehäufigkeit** an, wie häufig (1 bis 12 Stunden) Wiederherstellungspunkte erstellt werden sollen, die anwendungskonsistente Momentaufnahmen enthalten.

    - Hyper-V verwendet zwei Momentaufnahmen: eine Standard-Momentaufnahme, die eine inkrementelle Momentaufnahme des gesamten virtuellen Computers bereitstellt, und eine anwendungskonsistente Momentaufnahme, die eine Zeitpunkt-Momentaufnahme der Anwendungsdaten innerhalb des virtuellen Computers erfasst.
    - Anwendungskonsistente Momentaufnahmen verwenden den Volumeschattenkopie-Dienst (Volume Shadow Copy Service, VSS), um sicherzustellen, dass Anwendungen sich bei der Erstellung der Momentaufnahme in einem konsistenten Zustand befinden.
    - Die Leistung von Anwendungen auf virtuellen Quellcomputern wird durch die Aktivierung anwendungskonsistenter Momentaufnahmen beeinträchtigt. Stellen Sie sicher, dass der festgelegte Wert kleiner als die konfigurierte Anzahl der zusätzlichen Wiederherstellungspunkte ist.

6. Geben Sie unter **Startzeit der ersten Replikation** an, wann die erste Replikation starten soll. Da die Replikation über Ihre Internetbandbreite durchgeführt wird, ist es ratsam, sie außerhalb der Zeiten mit der höchsten Arbeitsbelastung einzuplanen. Klicken Sie dann auf **OK**.

    ![Replikationsrichtlinie](./media/site-recovery-hyper-v-site-to-azure/gs-replication2.png)

Wenn Sie eine neue Richtlinie erstellen, wird sie der Hyper-V-Site automatisch zugeordnet. Sie können die Hyper-V-Site (und die darin enthaltenen VMs) über **Replikation** > Richtlinienname > **Associate Hyper-V Site** (Hyper-V-Site zuordnen) mehreren Replikationsrichtlinien zuordnen.

## <a name="capacity-planning"></a>Kapazitätsplanung

Nachdem Sie nun Ihre grundlegende Infrastruktur eingerichtet haben, können Sie sich mit der Kapazitätsplanung beschäftigen und ermitteln, ob Sie zusätzliche Ressourcen benötigen.

Site Recovery verfügt über einen Kapazitätsplaner, der Sie dabei unterstützt, die richtigen Ressourcen für Compute, Netzwerk und Speicher zuzuordnen. Sie können den Planer im Schnellmodus für Schätzungen, die auf einer durchschnittlichen Anzahl von VMs, Datenträgern und Speicher basieren, oder im ausführlichen Modus mit benutzerdefinierten Zahlen auf Workloadebene ausführen. Führen Sie Folgendes aus, bevor Sie beginnen:

* Sammeln Sie Informationen zu Ihrer Replikationsumgebung, z.B. VMs, Datenträger pro VM und Speicher pro Datenträger.
* Schätzen Sie die tägliche Änderungsrate für Ihre replizierten Daten. Zur Unterstützung können Sie den [Capacity Planner für Hyper-V-Replikat](https://www.microsoft.com/download/details.aspx?id=39057) verwenden.

1. Klicken Sie auf **Herunterladen**, um das Tool herunterzuladen und es anschließend auszuführen. [entsprechenden Artikel](site-recovery-capacity-planner.md) durch.
2. Wählen Sie anschließend unter **Haben Sie den Capacity Planner ausgeführt?** die Option **Ja** aus.

   ![Kapazitätsplanung](./media/site-recovery-hyper-v-site-to-azure/gs-capacity-planning.png)

Erfahren Sie mehr über das [Steuern der Netzwerkbandbreite](#network-bandwidth-considerations).



## <a name="enable-replication"></a>Replikation aktivieren

Bevor Sie starten, stellen Sie sicher, dass Ihr Azure-Benutzerkonto die erforderlichen [Berechtigungen](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) zum Aktivieren der Replikation eines neuen virtuellen Computers in Azure besitzt.

Aktivieren Sie die Replikation für VMs wie folgt:          

1. Klicken Sie auf **Replicate application (Anwendung replizieren)** > **Quelle**. Klicken Sie nach der erstmaligen Einrichtung der Replikation auf **+Replizieren**, um die Replikation für weitere Computer zu aktivieren.

    ![Replikation aktivieren](./media/site-recovery-hyper-v-site-to-azure/enable-replication.png)
2. Wählen Sie in **Quelle** die Hyper-V-Site aus. Klicken Sie dann auf **OK**.
3. Wählen Sie unter **Ziel** das Tresorabonnement und das Failovermodell aus, das Sie in Azure (klassisches oder Resource Manager-Modell) nach einem Failover verwenden möchten.
4. Wählen Sie das Speicherkonto aus, das Sie verwenden möchten. Wenn Sie über kein Konto verfügen, das Sie verwenden möchten, können Sie [eines erstellen](#set-up-an-azure-storage-account). Klicken Sie dann auf **OK**.
5. Wählen Sie das Azure-Netzwerk und das Subnetz aus, mit dem virtuelle Azure-Computer bei einem Failover eine Verbindung herstellen.

    - Um die Netzwerkeinstellungen auf alle Computer anzuwenden, die Sie für die Replikation aktivieren, wählen Sie **Jetzt für die ausgewählten Computer konfigurieren** aus.
    - Wählen Sie **Später konfigurieren** aus, um das Azure-Netzwerk pro Computer auszuwählen.
    - Wenn Sie über kein Netzwerk verfügen, das Sie verwenden möchten, können Sie [eines erstellen](#set-up-an-azure-network). Wählen Sie, falls zutreffend, ein Subnetz aus. Klicken Sie dann auf **OK**.

   ![Replikation aktivieren](./media/site-recovery-hyper-v-site-to-azure/enable-replication11.png)

6. Klicken Sie auf **Virtuelle Computer** > **Virtuelle Computer auswählen**, und wählen Sie die Computer aus, die Sie replizieren möchten. Sie können nur Computer auswählen, für die die Replikation aktiviert werden kann. Klicken Sie dann auf **OK**.

    ![Replikation aktivieren](./media/site-recovery-hyper-v-site-to-azure/enable-replication5-for-exclude-disk.png)

7. Geben Sie unter **Eigenschaften** > **Eigenschaften konfigurieren**das Betriebssystem für die ausgewählten VMs und den Betriebssystem-Datenträger aus.
8. Stellen Sie sicher, dass der Name des virtuellen Azure-Computers (Zielname) den [Anforderungen für virtuelle Azure-Computer](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) entspricht.
9. Standardmäßig werden alle Datenträger des virtuellen Computers für die Replikation ausgewählt, doch Sie können Datenträger löschen, um sie auszuschließen.
    - Möglicherweise möchten Sie Datenträger ausschließen, um die Replikationsbandbreite zu verringern. Beispielsweise möchten Sie vielleicht nicht, dass Datenträger mit temporären Daten oder Daten, die bei jedem Neustart eines Computers oder einer App aktualisiert werden (etwa „pagefile.sys“ oder Microsoft SQL Server tempdb), repliziert werden. Durch Aufheben der Auswahl eines Datenträgers können Sie ihn von der Replikation ausschließen.
    - Nur Basisdatenträger können ausgeschlossen werden. Betriebssystemdatenträger können Sie nicht ausschließen.
    - Es wird empfohlen, keine dynamischen Datenträger auszuschließen. Site Recovery kann nicht ermitteln, ob eine virtuelle Festplatte in einer Gast-VM einfach oder dynamisch ist. Wenn keiner der abhängigen dynamischen Volumendatenträger ausgeschlossen wird, werden geschützte dynamische Datenträger bei einem VM-Failover als fehlerhaft angesehen, und auf die Daten des Datenträgers kann nicht zugegriffen werden.
        - Nach Aktivierung der Replikation können Sie keine Datenträger für die Replikation hinzufügen oder entfernen. Wenn Sie einen Datenträger hinzufügen oder entfernen möchten, müssen Sie den Schutz für den virtuellen Computer deaktivieren und anschließend wieder aktivieren.
        - Für Datenträger, die Sie manuell in Azure erstellen, wird kein Failback durchgeführt. Wenn Sie also beispielsweise ein Failover für drei Datenträger durchführen und zwei Datenträger direkt auf dem virtuellen Azure-Computer erstellen, wird nur für die drei Datenträger, für die das Failover ausgeführt wurde, ein Failback von Azure zu Hyper-V durchgeführt. Manuell erstellte Datenträger können nicht in das Failback oder in die umgekehrte Replikation von Hyper-V zu Azure einbezogen werden.
        - Wenn Sie einen Datenträger ausschließen, der für den Betrieb einer Anwendung erforderlich ist, müssen Sie ihn nach dem Failover zu Azure manuell in Azure erstellen, damit die replizierte Anwendung ausgeführt werden kann. Alternativ können Sie Azure Automation in einen Wiederherstellungsplan integrieren, um den Datenträger während des Failovers des Computers zu erstellen.

10. Klicken Sie zum Speichern der Änderungen auf **OK**. Sie können später weitere Eigenschaften festlegen.

    ![Replikation aktivieren](./media/site-recovery-hyper-v-site-to-azure/enable-replication6-with-exclude-disk.png)

11. Wählen Sie unter **Replikationseinstellungen** > **Replikationseinstellungen konfigurieren** die Replikationsrichtlinie aus, die Sie für die geschützten VMs anwenden möchten. Klicken Sie dann auf **OK**. Sie können die Replikationsrichtlinie unter **Replikationsrichtlinien** > Richtlinienname > **Einstellungen bearbeiten** ändern. Von Ihnen vorgenommene Änderungen werden für Computer, die bereits repliziert werden, und für neue Computer verwendet.


   ![Replikation aktivieren](./media/site-recovery-hyper-v-site-to-azure/enable-replication7.png)

Sie können den Fortschritt des Auftrags **Schutz aktivieren** unter **Aufträge** > **Site Recovery-Aufträge** verfolgen. Nachdem der Auftrag **Schutz abschließen** ausgeführt wurde, ist der Computer bereit für das Failover.

### <a name="view-and-manage-vm-properties"></a>Anzeigen und Verwalten von VM-Eigenschaften

Es wird empfohlen, dass Sie die Eigenschaften des Quellcomputers überprüfen.

1. Klicken Sie unter **Geschützte Elemente** auf **Replizierte Elemente**, und wählen Sie den Computer aus.

    ![Replikation aktivieren](./media/site-recovery-hyper-v-site-to-azure/test-failover1.png)
2. Unter **Eigenschaften** können Sie die Informationen zur Replikation und zum Failover für den virtuellen Computer anzeigen.

    ![Replikation aktivieren](./media/site-recovery-hyper-v-site-to-azure/test-failover2.png)
3. Unter **Compute und Netzwerk** > **Compute-Eigenschaften** können Sie den Namen und die Zielgröße des virtuellen Azure-Computers angeben. Ändern Sie ggf. den Namen, damit er die Azure-Anforderungen erfüllt. Sie können auch Informationen zum Zielnetzwerk, zum Subnetz sowie zur IP-Adresse anzeigen und ändern, die der Azure-VM zugewiesen wird. Beachten Sie Folgendes:

   * Sie können die Ziel-IP-Adresse festlegen. Wenn Sie keine Adresse angeben, wird für den Computer, für den das Failover durchgeführt wurde, DHCP verwendet. Wenn Sie eine Adresse festlegen, die beim Failover nicht verfügbar ist, tritt beim Failover ein Fehler auf. Dieselbe Ziel-IP-Adresse kann für das Testfailover verwendet werden, wenn die Adresse im Testfailover-Netzwerk verfügbar ist.
   * Die Anzahl der Netzwerkkarten hängt von der Größe ab, die Sie für den virtuellen Zielcomputer angeben. Hierbei gilt Folgendes:

     * Wenn die Anzahl der Netzwerkkarten des Quellcomputers maximal der Anzahl der Netzwerkkarten entspricht, die für die Größe des Zielcomputers zulässig ist, hat der Zielcomputer die gleiche Anzahl von Netzwerkkarten wie der Quellcomputer.
     * Wenn die Anzahl der Netzwerkadapter für den virtuellen Quellcomputer die maximal zulässige Anzahl für die Größe des Zielcomputers übersteigt, wird die Anzahl verwendet, die maximal für die Größe des Zielcomputers zulässig ist.
     * Ein Beispiel: Wenn ein Quellcomputer zwei Netzwerkkarten besitzt und der Zielcomputer aufgrund seiner Größe vier Netzwerkkarten unterstützt, erhält der Zielcomputer zwei Netzwerkkarten. Wenn der Quellcomputer dagegen zwei Netzwerkadapter besitzt und der Zielcomputer aufgrund seiner Größe nur einen Adapter unterstützt, erhält der Zielcomputer nur einen Adapter.     
     * Wenn die VM über mehrere Netzwerkkarten verfügt, werden alle mit dem gleichen Netzwerk verbunden.
     * Wenn der virtuelle Computer über mehrere Netzwerkadapter verfügt, wird der erste, der in der Liste angezeigt wird, zum *Standard*-Netzwerkadapter im virtuellen Azure-Computer.

     ![Replikation aktivieren](./media/site-recovery-hyper-v-site-to-azure/test-failover4.png)

4. Unter **Datenträger** werden das Betriebssystem und die Datenträger auf der VM angezeigt, die repliziert werden.

#### <a name="managed-disks"></a>Verwaltete Datenträger

Unter **Compute und Netzwerk** > **Compute-Eigenschaften** können Sie die Einstellung „Verwaltete Datenträger verwenden“ für den virtuellen Computer auf „Ja“ festlegen, wenn Sie Ihrem Computer bei der Migration zu Azure verwaltete Datenträger anfügen möchten. Managed Disks vereinfacht die Datenträgerverwaltung für Azure-IaaS-VMs durch die Verwaltung der Speicherkonten, die den VM-Datenträgern zugeordnet sind. [Weitere Informationen zu Managed Disks](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview).

   - Verwaltete Datenträger werden erstellt und nur beim Failover zu Azure an den virtuellen Computer angefügt. Beim Aktivieren des Schutzes werden Daten von lokalen Computern weiterhin auf Speicherkonten repliziert.
   Verwaltete Datenträger können nur für virtuelle Computer erstellt werden, die über das Resource Manager-Bereitstellungsmodell bereitgestellt werden.

  > [!NOTE]
  > Ein Failback von Azure zur lokalen Hyper-V-Umgebung wird für Computer mit verwalteten Datenträgern derzeit nicht unterstützt. Legen Sie „Verwaltete Datenträger verwenden“ nur dann auf „Ja“ fest, wenn Sie beabsichtigen, diesen Computer zu Azure zu migrieren.

   - Wenn Sie „Verwaltete Datenträger verwenden“ auf „Ja“ festlegen, stehen nur Verfügbarkeitsgruppen in der Ressourcengruppe zur Auswahl, bei denen „Verwaltete Datenträger verwenden“ auf „Ja“ festgelegt ist. Der Grund hierfür ist, dass virtuelle Computer mit verwalteten Datenträgern nur in Verfügbarkeitsgruppen enthalten sein können, deren Eigenschaft „Verwaltete Datenträger verwenden“ auf „Ja“ festgelegt ist. Stellen Sie sicher, dass die Eigenschaft „Verwaltete Datenträgern verwenden“ der erstellten Verfügbarkeitsgruppen Ihrer Absicht entspricht, verwaltete Datenträger beim Failover zu verwenden. In gleicher Weise gilt: Wenn Sie „Verwaltete Datenträger verwenden“ auf „Nein“ festlegen, stehen nur Verfügbarkeitsgruppen in der Ressourcengruppe zur Auswahl, deren Eigenschaft „Verwaltete Datenträger verwenden“ auf „Nein“ festgelegt ist. [Weitere Informationen zu verwalteten Datenträgern und Verfügbarkeitsgruppen](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability#use-managed-disks-for-vms-in-an-availability-set).

  > [!NOTE]
  > Wenn das für die Replikation verwendete Speicherkonto zu einem beliebigen Zeitpunkt mit der Speicherdienstverschlüsselung verschlüsselt wurde, tritt bei der Erstellung verwalteter Datenträger während des Failovers ein Fehler auf. Sie können entweder „Verwaltete Datenträger verwenden“ auf „Nein“ festlegen und den Failoverversuch wiederholen oder den Schutz für den virtuellen Computer deaktivieren und diesen in einem Speicherkonto schützen, für das die Speicherdienstverschlüsselung zu keinem Zeitpunkt aktiviert war.
  > [Weitere Informationen zu Speicherdienstverschlüsselung und verwalteten Datenträgern](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview#managed-disks-and-encryption).


## <a name="test-the-deployment"></a>Testen der Bereitstellung

Zum Testen der Bereitstellung können Sie ein Testfailover für einen einzelnen virtuellen Computer durchführen. Alternativ können Sie einen Wiederherstellungsplan erstellen, der mehrere virtuelle Computer enthält.

### <a name="before-you-start"></a>Vorbereitung

 - Wenn Sie die Verbindung mit Azure-VMs nach dem Failover per RDP herstellen möchten, informieren Sie sich über die [Vorbereitung auf das Herstellen der Verbindung](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover).
 - Für den vollständigen Test müssen Sie Active Directory und DNS in Ihre Testumgebung kopieren. [Weitere Informationen](site-recovery-active-directory.md#test-failover-considerations).

### <a name="run-a-test-failover"></a>Durchführen eines Test-Failovers

1. Klicken Sie zum Durchführen eines Failovers für einen einzelnen Computer unter **Replizierte Elemente** auf den virtuellen Computer und dann auf das Symbol **+Testfailover**.
2. Klicken Sie zum Ausführen eines Failovers für einen Wiederherstellungsplan unter **Wiederherstellungspläne** mit der rechten Maustaste auf den Plan, und klicken Sie anschließend auf **Testfailover**. Eine Anleitung zum Erstellen eines Wiederherstellungsplans finden Sie [hier](site-recovery-create-recovery-plans.md).
3. Wählen Sie unter **Testfailover** das Azure-Netzwerk aus, mit dem Azure-VMs nach dem Failover verbunden werden.
4. Klicken Sie auf **OK**, um den Failovervorgang zu starten. Sie können den Fortschritt verfolgen, indem Sie auf den virtuellen Computer klicken, um die Eigenschaften zu öffnen. Alternativ können Sie unter „Tresorname“ > > **Aufträge** > **Site Recovery-Aufträge** auf den Auftrag **Testfailover** klicken.
5. Nach Abschluss des Failovers sollte der Azure-Replikatcomputer im Azure-Portal unter **Virtuelle Computer** angezeigt werden. Stellen Sie sicher, dass die VM die richtige Größe hat, mit dem richtigen Netzwerk verbunden ist und ausgeführt wird.
6. Wenn Sie die Vorbereitung für Verbindungen nach dem Failover durchgeführt haben, sollten Sie eine Verbindung mit dem virtuellen Azure-Computer herstellen können.
7. Klicken Sie anschließend auf dem Wiederherstellungsplan auf **Cleanup-Test-Failover** (Testfailover bereinigen). Erfassen und speichern Sie unter **Notizen** alle Beobachtungen im Zusammenhang mit dem Test-Failover. Dadurch werden die während des Testfailovers erstellten virtuellen Computer gelöscht.

Weitere Informationen finden Sie im Artikel [Testfailover in Azure](site-recovery-test-failover-to-azure.md).



## <a name="monitor-the-deployment"></a>Überwachen der Bereitstellung

Überwachen Sie Konfigurationseinstellungen, Status und Integrität für die Site Recovery-Bereitstellung:

1. Klicken Sie auf den Tresornamen, um auf das Dashboard **Zusammenfassung** zuzugreifen. In diesem Dashboard können Sie Site Recovery-Aufträge, Replikationsstatus, Wiederherstellungspläne, Serverzustand und Ereignisse nachverfolgen.  

    ![Zusammenfassung](./media/site-recovery-hyper-v-site-to-azure/essentials.png)
2. Auf der Kachel **Integrität** können Sie Standortserver, bei denen Probleme auftreten, sowie die von Site Recovery in den letzten 24 Stunden ausgelösten Ereignisse überwachen. Sie können die Zusammenfassung so anpassen, dass die nützlichsten Kacheln und Layouts angezeigt werden, z.B. auch der Status anderer Site Recovery- und Backup-Tresore.
3. Sie können die Replikation über die Kacheln **Replizierte Elemente**, **Wiederherstellungspläne** und **Site Recovery-Aufträge** verwalten und überwachen. Ausführlichere Informationen zu Aufträgen finden Sie unter **Aufträge** > **Site Recovery-Aufträge**.

## <a name="command-line-provider-and-agent-installation"></a>Befehlszeileninstallation von Anbieter und Agent

Der Azure Site Recovery-Anbieter und der Azure Site Recovery-Agent können auch mithilfe der folgenden Befehlszeile installiert werden. Mit dieser Methode kann der Anbieter in Server Core für Windows Server 2012 R2 installiert werden.

1. Laden Sie die Installationsdatei und den Registrierungsschlüssel des Anbieters in einen Ordner herunter. Beispiel: C:\ASR.
2. Extrahieren Sie über eine Eingabeaufforderung mit erhöhten Rechten das Installationsprogramm des Anbieters, indem Sie diese Befehle ausführen:

            C:\Windows\System32> CD C:\ASR
            C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Führen Sie diesen Befehl aus, um die Komponenten zu installieren:

            C:\ASR> setupdr.exe /i
4. Führen Sie anschließend diese Befehle aus, um den Server im Tresor zu registrieren:

            CD C:\Program Files\Microsoft Azure Site Recovery Provider\
            C:\Program Files\Microsoft Azure Site Recovery Provider\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file>

<br/>
Hierbei gilt:

* **/Credentials**: erforderlicher Parameter zum Angeben des Speicherorts der Registrierungsschlüsseldatei.  
* **/FriendlyName**: erforderlicher Parameter für den Namen des Hyper-V-Hostservers, der im Azure Site Recovery-Portal angezeigt wird.
* **/proxyAddress**: Optionaler Parameter, der die Adresse des Proxyservers angibt.
* **/proxyport** : optionaler Parameter, der den Port des Proxyservers angibt.
* **/proxyUsername**: optionaler Parameter, der den Proxybenutzernamen angibt (sofern für den Proxy eine Authentifizierung erforderlich ist).
* **/proxyPassword**: optionaler Parameter, der das Kennwort für die Authentifizierung beim Proxyserver angibt (sofern der Proxy eine Authentifizierung erfordert).


## <a name="network-bandwidth-considerations"></a>Aspekte der Netzwerkbandbreite
Sie können den [Hyper-V-Kapazitätsplaner](site-recovery-capacity-planner.md) verwenden, um die Bandbreite zu berechnen, die Sie für die Replikation benötigen (erste Replikation und dann Deltareplikation). Sie haben verschiedene Möglichkeiten, um die verwendete Bandbreite für die Replikation zu steuern:

* **Bandbreite drosseln**: Hyper-V-Datenverkehr, der nach Azure repliziert wird, wird über einen speziellen Hyper-V-Host geleitet. Sie können die Bandbreite auf dem Hostserver drosseln.
* **Bandbreite optimieren**: Sie können beeinflussen, wie viel Bandbreite für die Replikation verwendet wird, indem Sie einige Registrierungsschlüssel nutzen.

### <a name="throttle-bandwidth"></a>Bandbreite einschränken
1. Öffnen Sie das Microsoft Azure Backup-MMC-Snap-In auf dem Hyper-V-Hostserver. Standardmäßig ist auf dem Desktop oder unter „C:\Programme\Microsoft Azure Recovery Services Agent\bin\wabadmin“ eine Verknüpfung für Microsoft Azure Backup verfügbar.
2. Klicken Sie im Snap-In auf **Eigenschaften ändern**.
3. Wählen Sie auf der Registerkarte **Drosselung** die Option **Internet-Bandbreiteneinschränkung für Sicherungsvorgänge aktivieren** aus, und legen Sie die Grenzwerte für Geschäftszeiten und arbeitsfreie Zeiten fest. Der gültige Bereich reicht von 512 KBit/s bis 102 MBit/s.

    ![Bandbreite drosseln](./media/site-recovery-hyper-v-site-to-azure/throttle2.png)

Sie können auch das Cmdlet [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) verwenden, um die Drosselung festzulegen. Hier ein Beispiel:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting -NoThrottle** gibt an, dass keine Drosselung erforderlich ist.

### <a name="influence-network-bandwidth"></a>Netzwerkbandbreite beeinflussen
1. Navigieren Sie in der Registrierung zu **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
   * Um die Bandbreite für den Datenverkehr auf einem replizierenden Datenträger zu beeinflussen, ändern Sie den Wert für **UploadThreadsPerVM**, oder erstellen Sie den Schlüssel, falls dieser nicht vorhanden ist.
   * Um die Bandbreite für den Failbackdatenverkehr von Azure zu beeinflussen, ändern Sie den Wert für **DownloadThreadsPerVM**.
2. Der Standardwert ist 4. In einem absichtlich mit großen Reserven ausgestatteten Netzwerk müssen die Standardwerte dieser Registrierungsschlüssel geändert werden. Der maximale Wert beträgt 32. Überwachen Sie den Datenverkehr, um den Wert zu optimieren.

## <a name="next-steps"></a>Nächste Schritte

Nach Abschluss der ersten Replikation, und wenn Sie die Bereitstellung getestet haben, können Sie bei Bedarf Failover auslösen. [Informieren Sie sich](site-recovery-failover.md) über die unterschiedlichen Failoverarten und deren Durchführung.

