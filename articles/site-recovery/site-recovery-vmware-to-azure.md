---
title: Replizieren von VMware-VMs in Azure | Microsoft-Dokumentation
description: "Enthält eine Zusammenfassung der Schritte zum Replizieren von Workloads, die auf VMware-VMs ausgeführt werden, im Azure-Speicher."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: dab98aa5-9c41-4475-b7dc-2e07ab1cfd18
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/12/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: c308183ffe6a01f4d4bf6f5817945629cbcedc92
ms.openlocfilehash: b7b7347fef8ea6f6bf643e98bbcc0a6292c083ed
ms.contentlocale: de-de
ms.lasthandoff: 05/17/2017


---
# <a name="replicate-vmware-virtual-machines-to-azure-with-site-recovery"></a>Replizieren von virtuellen VMware-Computern in Azure mithilfe von Site Recovery

> [!div class="op_single_selector"]
> * [Azure-Portal](site-recovery-vmware-to-azure.md)
> * [Klassisches Azure](site-recovery-vmware-to-azure-classic.md)


In diesem Artikel erfahren Sie, wie Sie lokale virtuelle VMware-Computer mit dem [Azure Site Recovery](site-recovery-overview.md)-Dienst über das Azure-Portal in Azure replizieren.

Wenn Sie virtuelle VMware-Computer zu Azure migrieren möchten (nur Failover), lesen Sie [diesen Artikel](site-recovery-migrate-to-azure.md), um mehr zu erfahren.

Kommentare und Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.

## <a name="deployment-steps"></a>Bereitstellungsschritte

Das müssen Sie tun:

1. Überprüfen Sie die Voraussetzungen und Einschränkungen.
2. Richten Sie Azure-Netzwerk- und -Speicherkonten ein.
3. Bereiten Sie den lokalen Computer vor, den Sie als Konfigurationsserver bereitstellen möchten.
4. Bereiten Sie VMware-Konten für die automatische Ermittlung von VMs und optional für die Pushinstallation des Mobilitätsdiensts vor.
4. Erstellen Sie einen Recovery Services-Tresor. Der Tresor enthält Konfigurationseinstellungen und dient zum Orchestrieren der Replikation.
5. Geben Sie Quelle, Ziel und Replikationseinstellungen an.
6. Stellen Sie den Mobilitätsdienst auf VMs bereit, die Sie replizieren möchten.
7. Aktivieren Sie die Replikation für die VMs.
7. Führen Sie ein Testfailover aus, um sicherzustellen, dass alles wie erwartet funktioniert.

## <a name="prerequisites"></a>Voraussetzungen

**Supportbedarf** | **Details**
--- | ---
**Azure** | Informieren Sie sich über die [Azure-Anforderungen](site-recovery-prereq.md#azure-requirements).
**Lokaler Konfigurationsserver** | Sie benötigen eine VMware-VM mit Windows Server 2012 R2 oder höher. Sie richten diesen Server während der Site Recovery-Bereitstellung ein.<br/><br/> Standardmäßig werden auch der Prozessserver und der Masterzielserver auf dieser VM installiert. Wenn Sie zentral hochskalieren, benötigen Sie möglicherweise einen separaten Prozessserver. Es gelten die gleichen Voraussetzungen wie für den Konfigurationsserver.<br/><br/> Weitere Informationen zu diesen Komponenten finden Sie [hier](site-recovery-set-up-vmware-to-azure.md#configuration-server-minimum-requirements).
**Lokale VMware-Server** | Mindestens einen VMware vSphere-Server mit 6.0, 5.5 oder 5.1 mit den neuesten Updates. Die Server sollten sich in demselben Netzwerk wie der Konfigurationsserver (oder der separate Prozessserver) befinden.<br/><br/> Wir empfehlen Ihnen die Verwendung eines vCenter-Servers zum Verwalten von Hosts, auf dem Version 6.0 oder 5.5 mit den neuesten Updates ausgeführt wird. Bei der Bereitstellung von Version 6.0 werden nur Features unterstützt, die in 5.5 verfügbar sind.
**Lokale VMs** | Auf VMs, die Sie replizieren möchten, sollte ein [unterstütztes Betriebssystem](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) ausgeführt werden, das die [Azure-Voraussetzungen](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) erfüllt. Auf der VM sollten die VMware-Tools ausgeführt werden.
**URLs** | Der Konfigurationsserver benötigt Zugriff auf diese URLs:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> Stellen Sie bei Verwendung von auf IP-Adressen basierenden Firewallregeln sicher, dass hierfür die Kommunikation mit Azure möglich ist.<br/></br> Lassen Sie die [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653) (IP-Bereiche für Azure-Rechenzentren) und den HTTPS-Port (443) zu.<br/></br> Lassen Sie IP-Adressbereiche für die Azure-Region Ihres Abonnements und für die Region „USA, Westen“ (wird für Zugriffsteuerung und Identitätsverwaltung verwendet) zu.<br/><br/> Lassen Sie zum Herunterladen von MySQL den Zugriff auf die URL „http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi“ zu.
**Mobilitätsdienst** | Ist auf jeder replizierten VM installiert.




## <a name="limitations"></a>Einschränkungen

**Einschränkung** | **Details**
--- | ---
**Azure** | Speicher- und Netzwerkkonten müssen sich in derselben Region wie der Tresor befinden.<br/><br/> Bei Verwendung eines Storage Premium-Kontos benötigen Sie auch ein Standardspeicherkonto zum Speichern von Replikationsprotokollen.<br/><br/> Die Replikation in Premium-Konten in den Regionen „Indien, Mitte“ und „Indien, Süden“ ist nicht möglich.
**Lokaler Konfigurationsserver** | Der VMware-VM-Adaptertyp sollte „VMXNET3“ lauten. [Installieren Sie dieses Update](https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2110245&sliceId=1&docTypeID=DT_KB_1_1&dialogID=26228401&stateId=1), falls dies nicht der Fall ist.<br/><br/> vSphere PowerCLI 6.0 sollte installiert sein.<br/><br> Der Computer sollte kein Domänencontroller sein. Der Computer sollte über eine statische IP-Adresse verfügen.<br/><br/> Der Hostname sollte maximal 15 Zeichen lang sein, und es sollte ein Betriebssystem in englischer Sprache verwendet werden.
**VMware** | Nur Features der Version 5.5 werden in vCenter 6.0 unterstützt. Site Recovery unterstützt neue Features von vCenter und vSphere 6.0 nicht. Hierzu zählen etwa Cross vCenter vMotion, Virtual Volumes und Storage DRS.
**VMs** | Überprüfen Sie die [Einschränkungen für Azure-VMs](site-recovery-prereq.md#azure-requirements).<br/><br/> Es ist nicht möglich, VMs mit verschlüsselten Datenträgern oder VMs mit UEFI/EFI-Startvorgang zu replizieren.<br/><br> Cluster mit freigegebenen Datenträgern werden nicht unterstützt. Wenn auf dem virtuellen Quellcomputer NIC-Teamvorgänge ausgeführt wurden, wird er nach dem Failover in eine einzelne NIC konvertiert.<br/><br/> Falls VMs über einen iSCSI-Datenträger verfügen, wird er von Site Recovery nach dem Failover in eine VHD-Datei konvertiert. Wenn das iSCSI-Ziel für die Azure-VM erreichbar ist, wird eine Verbindung damit hergestellt, und sowohl das Ziel als auch die VHD sind sichtbar. Trennen Sie die Verbindung mit dem iSCSI-Ziel in diesem Fall.<br/><br/> Öffnen Sie Port 20004 auf der VM, falls Sie die Multi-VM-Konsistenz aktivieren möchten. Hierbei können VMs, auf denen die gleiche Workload ausgeführt wird, gemeinsam an einem konsistenten Datenpunkt wiederhergestellt werden.<br/><br/> Windows muss auf dem Laufwerk C installiert sein. Der Betriebssystemdatenträger sollte ein einfacher Datenträger sein (kein dynamischer). Der Datenträger für Daten kann dynamisch sein.<br/><br/> Linux-Dateien vom Typ „/etc/hosts“ auf VMs müssen Einträge enthalten, mit denen der Name des lokalen Hosts den IP-Adressen aller Netzwerkkarten zugeordnet wird. Der Hostname, Bereitstellungspunkte, Gerätename, Systempfade und Dateinamen (z.B. „/etc/“, „/usr“) sollten nur englische Zeichen umfassen.<br/><br/> Bestimmte Arten von [Linux-Speicher](site-recovery-support-matrix-to-azure.md#support-for-storage) werden unterstützt.<br/><br/>Erstellen Sie **disk.enableUUID=true** in den VM-Einstellungen, bzw. legen Sie dies fest. So wird für das VMDK eine konsistente UUID bereitgestellt, damit die Bereitstellung richtig durchgeführt wird. Außerdem wird sichergestellt, dass während des Failbacks nur Deltaänderungen zurück an den lokalen Standort übertragen werden und keine vollständige Replikation durchgeführt wird.

## <a name="set-up-azure"></a>Einrichten von Azure

1. [Richten Sie ein Azure-Netzwerk ein](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).
    - Azure-VMs werden in diesem Netzwerk angeordnet, wenn sie nach dem Failover erstellt werden.
    - Sie können ein Netzwerk im [Resource Manager](../resource-manager-deployment-model.md)-Modus oder im klassischen Modus einrichten.

2. [Richten Sie ein Azure-Speicherkonto ein](../storage/storage-create-storage-account.md#create-a-storage-account), das für replizierte Daten verwendet werden kann.
    - Dies kann ein Standard- oder [Premium](../storage/storage-premium-storage.md)-Konto sein.
    - Sie können ein Konto im Resource Manager-Modus oder im klassischen Modus einrichten.

3. [Vorbereiten eines Kontos](#prepare-for-automatic-discovery-and-push-installation) auf dem vCenter-Server oder den vSphere-Hosts, damit VMware-VMs von Site Recovery automatisch erkannt werden können.

## <a name="prepare-the-configuration-server"></a>Bereiten Sie den Konfigurationsserver vor

1. Installieren Sie Windows Server 2012 R2 oder höher auf einer VMware-VM.
2. Stellen Sie sicher, dass für die VM Zugriff auf die URLs besteht, die unter den [Voraussetzungen](#prerequisites) aufgeführt sind.
3. Installieren Sie [VMware vSphere PowerCLI 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1).


## <a name="prepare-for-automatic-discovery-and-push-installation"></a>Vorbereiten für die automatische Ermittlung und Pushinstallation

- **Vorbereiten eines Kontos für die automatische Ermittlung**: Der Site Recovery-Prozessserver ermittelt VMs automatisch. Hierfür benötigt Site Recovery Anmeldeinformationen, mit denen auf vCenter-Server und vSphere ESXi-Hosts zugegriffen werden kann.

    1. Erstellen Sie zum Verwenden eines dedizierten Kontos eine Rolle (auf vCenter-Ebene mit diesen [Berechtigungen](#vmware-account-permissions)). Geben Sie ihr einen Namen wie **Azure_Site_Recovery**.
    2. Erstellen Sie anschließend einen Benutzer auf dem vSphere-Host bzw. dem vCenter-Server, und weisen Sie die Rolle dem Benutzer zu. Sie geben dieses Benutzerkonto während der Site Recovery-Bereitstellung an.

- **Vorbereiten eines Kontos für die Pushinstallation des Mobilitätsdiensts**: Wenn Sie den Mobilitätsdienst per Pushvorgang auf den VMs installieren möchten, benötigen Sie ein Konto, das vom Prozessserver für den Zugriff auf die VM verwendet werden kann. Das Konto wird nur für die Pushinstallation verwendet. Sie können ein Domänenkonto oder ein lokales Konto verwenden:

    - Wenn Sie kein Domänenkonto verwenden, müssen Sie für Windows die Remote-Benutzerzugriffssteuerung auf dem lokalen Computer deaktivieren. Zu diesem Zweck fügen Sie in der Registrierung unter **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System** den DWORD-Eintrag **LocalAccountTokenFilterPolicy** mit dem Wert 1 hinzu.
    - Geben Sie Folgendes ein, wenn Sie den Registrierungseintrag für Windows über eine CLI hinzufügen möchten:   ``REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.``
    - Bei Linux muss auf dem Linux-Quellserver das root-Konto verwendet werden.

## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="select-the-protection-goal"></a>Auswählen des Schutzziels

Wählen Sie aus, was Sie replizieren möchten und wohin die Daten repliziert werden sollen.

1. Klicken Sie auf **Recovery Services-Tresore** > „Tresor“.
2. Klicken Sie im Ressourcenmenü auf **Site Recovery** > **Schritt 1: Bereiten Sie die Infrastruktur vor** > **Schutzziel**.

    ![Ziele wählen](./media/site-recovery-vmware-to-azure/choose-goals.png)
3. Wählen Sie unter **Schutzziel** die Option **To Azure** (Zu Azure) > **Yes, with VMware vSphere Hypervisor** (Ja, mit VMware vSphere-Hypervisor) aus.

    ![Ziele wählen](./media/site-recovery-vmware-to-azure/choose-goals2.png)

## <a name="set-up-the-source-environment"></a>Einrichten der Quellumgebung

Richten Sie den Konfigurationsserver ein, registrieren Sie ihn im Tresor, und ermitteln Sie die VMs.

1. Klicken Sie auf **Site Recovery** > **Schritt 1: Bereiten Sie die Infrastruktur vor** > **Quelle**.
2. Wenn Sie keinen Konfigurationsserver verwenden, klicken Sie auf **+Konfigurationsserver**.

    ![Quelle einrichten](./media/site-recovery-vmware-to-azure/set-source1.png)
3. Überprüfen Sie unter **Server hinzufügen**, ob unter **Servertyp** die Option **Konfigurationsserver** angezeigt wird.
4. Laden Sie die Installationsdatei für das einheitliche Setup von Site Recovery herunter.
5. Laden Sie den Tresorregistrierungsschlüssel herunter. Sie benötigen diesen beim Ausführen des einheitlichen Setups. Der Schlüssel ist nach der Erstellung fünf Tage lang gültig.

   ![Quelle einrichten](./media/site-recovery-vmware-to-azure/set-source2.png)


## <a name="run-site-recovery-unified-setup"></a>Ausführen des einheitlichen Setups von Site Recovery

Erledigen Sie das Folgende, bevor Sie beginnen, und führen Sie dann das einheitliche Setup aus, um den Konfigurationsserver, den Prozessserver und den Masterzielserver zu installieren.
    - Verschaffen Sie sich schnell einen Überblick per Video

        > [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video1-Source-Infrastructure-Setup/player]

    - Stellen Sie auf der Konfigurationsserver-VM sicher, dass die Systemuhr mit einem [Zeitserver](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) synchronisiert ist. Die Zeiten sollten übereinstimmen. Falls der Unterschied 15 Minuten beträgt, ist das Setup unter Umständen nicht erfolgreich.
    - Führen Sie das Setup als lokaler Administrator auf der Konfigurationsserver-VM aus.
    - Stellen Sie sicher, dass TLS 1.0 auf der VM aktiviert ist.


[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> Der Konfigurationsserver kann auch [über die Befehlszeile](http://aka.ms/installconfigsrv) installiert werden.



### <a name="add-the-account-for-automatic-discovery"></a>Hinzufügen des Kontos für die automatische Ermittlung

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

### <a name="connect-to-vmware-servers"></a>Herstellen der Verbindung mit VMware-Servern

Stellen Sie eine Verbindung mit vSphere ESXi-Hosts oder vCenter-Servern her, um VMware-VMs zu ermitteln.

- Wenn Sie den vCenter-Server oder vSphere-Hosts mit einem Konto ohne Administratorrechte auf dem Server hinzufügen, müssen für das Konto die folgenden Berechtigungen aktiviert sein:
    - Datencenter, Datenspeicher, Ordner, Host, Netzwerk, Ressource, Virtueller Computer, vSphere Distributed Switch.
    - Der vCenter-Server erfordert Anzeigeberechtigungen für den Speicher.
- Beim Hinzufügen von VMware-Servern kann es 15 Minuten oder auch länger dauern, bis sie im Portal angezeigt werden.
Damit Azure Site Recovery virtuelle Computern in Ihrer lokalen Umgebung ermitteln kann, müssen Sie den VMware vCenter Server- oder vSphere ESXi-Host mit Site Recovery verbinden.

Klicken Sie auf **+vCenter**, um eine Verbindung mit einem VMware vCenter Server- oder VMware vSphere ESXi-Host herzustellen.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]

Site Recovery stellt mithilfe der angegebenen Einstellungen eine Verbindung mit VMware-Servern her und ermittelt VMs.

## <a name="set-up-the-target"></a>Einrichten des Ziels

Überprüfen Sie vor dem Einrichten der Zielumgebung, ob Sie über ein [Azure-Speicherkonto und -Netzwerk](#set-up-azure) verfügen.

1. Klicken Sie auf **Infrastruktur vorbereiten** > **Ziel**, und wählen Sie das gewünschte Azure-Abonnement aus.
2. Geben Sie an, ob Ihr Zielbereitstellungsmodell vom Typ „Resource Manager“ oder „klassisch“ ist.
3. Site Recovery prüft, ob Sie über ein oder mehrere kompatible Azure-Speicherkonten und -Netzwerke verfügen.

   ![Ziel](./media/site-recovery-vmware-to-azure/gs-target.png)
4. Gehen Sie wie folgt vor, falls Sie kein Speicherkonto oder Netzwerk erstellt haben: Klicken Sie auf **+Speicherkonto** oder **+Netzwerk**, um „inline“ ein Resource Manager-Konto oder Netzwerk zu erstellen.

## <a name="set-up-replication-settings"></a>Einrichten der Replikationseinstellungen

Bevor Sie beginnen, verschaffen Sie sich schnell einen Überblick per Video:
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video2-vCenter-Server-Discovery-and-Replication-Policy/player]

1. Klicken Sie zum Erstellen einer neuen Replikationsrichtlinie auf **Site Recovery-Infrastruktur** > **Replikationsrichtlinien** > **+Replikationsrichtlinie**.
2. Geben Sie unter **Replikationsrichtlinie erstellen** einen Richtliniennamen an.
3. Geben Sie unter **RPO-Schwellenwert** den RPO-Grenzwert an. Mit diesem Wert wird angegeben, wie oft Datenwiederherstellungspunkte erstellt werden. Wenn dieser Grenzwert bei der fortlaufenden Replikation überschritten wird, wird eine Warnung generiert.
4. Geben Sie unter **Aufbewahrungszeitraum des Wiederherstellungspunkts** das Aufbewahrungszeitfenster für die einzelnen Wiederherstellungspunkte (in Stunden) an. Replizierte VMs können für jeden Punkt eines Zeitfensters wiederhergestellt werden. Für nach Storage Premium replizierte Computer wird eine Aufbewahrungsdauer von bis zu 24 Stunden unterstützt (72 Stunden für Standardspeicher).
5. Geben Sie unter **App-konsistente Momentaufnahmehäufigkeit**an, wie häufig (in Minuten) Wiederherstellungspunkte erstellt werden sollen, die anwendungskonsistente Momentaufnahmen enthalten. Klicken Sie auf **OK**, um die Richtlinie zu erstellen.

    ![Replikationsrichtlinie](./media/site-recovery-vmware-to-azure/gs-replication2.png)
8. Wenn Sie eine neue Richtlinie erstellen, wird sie dem Konfigurationsserver automatisch zugeordnet. Standardmäßig wird für das Failback automatisch eine passende Richtlinie erstellt. Bei Verwendung der Replikationsrichtlinie **rep-policy** lautet die Failbackrichtlinie beispielsweise **rep-policy-failback**. Diese Richtlinie wird erst verwendet, wenn Sie ein Failback über Azure initiieren.  



## <a name="plan-capacity"></a>Planen der Kapazität

1. Nachdem Sie nun Ihre grundlegende Infrastruktur eingerichtet haben, können Sie sich mit der Kapazitätsplanung beschäftigen und ermitteln, ob Sie zusätzliche Ressourcen benötigen. [Weitere Informationen](site-recovery-plan-capacity-vmware.md).
2. Wählen Sie nach Abschluss der Kapazitätsplanung unter **Haben Sie die Kapazitätsplanung abgeschlossen?** die Option **Ja**.

   ![Kapazitätsplanung](./media/site-recovery-vmware-to-azure/gs-capacity-planning.png)


## <a name="prepare-vms-for-replication"></a>Vorbereiten von VMs für die Replikation

Der Mobilitätsdienst muss auf jeder VMware-VM installiert sein, die Sie replizieren möchten. Sie können den Mobilitätsdienst auf verschiedene Arten installieren:

1. Installation per Pushvorgang vom Prozessserver. Sie müssen VMs vorbereiten, um diese Methode zu verwenden.
2. Installation mit Bereitstellungstools, z.B. System Center Configuration Manager oder Azure Automation DSC.
3.  Manuelle Installation.

[Weitere Informationen](site-recovery-vmware-to-azure-install-mob-svc.md)


## <a name="enable-replication"></a>Replikation aktivieren

Vorbereitung:

- Ihr Azure-Benutzerkonto benötigt bestimmte [Berechtigungen](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) zum Aktivieren der Replikation eines neuen virtuellen Computers in Azure.
- Beim Hinzufügen oder Ändern von VMs kann es 15 Minuten oder auch länger dauern, bis Änderungen wirksam werden und im Portal sichtbar sind.
- Sie können den letzten Zeitpunkt der Ermittlung für VMs unter **Konfigurationsserver** > **Letzter Kontakt um** überprüfen.
- Wenn Sie VMs hinzufügen möchten, ohne auf die planmäßige Ermittlung zu warten, markieren Sie den Konfigurationsserver (nicht darauf klicken) und klicken auf **Refresh** (Aktualisieren).
- Wenn eine VM für die Pushinstallation vorbereitet wurde, installiert der Prozessserver automatisch den Mobilitätsdienst, sobald Sie die Replikation aktivieren.


### <a name="exclude-disks-from-replication"></a>Ausschließen von Datenträgern von der Replikation

Standardmäßig werden alle Datenträger auf einem Computer repliziert. Sie können Datenträger von der Replikation ausschließen. Beispielsweise möchten Sie möglicherweise nicht, dass Datenträger mit temporären Daten oder Daten, die bei jedem Neustart des Computers oder der Anwendung aktualisiert werden (z.B. pagefile.sys oder SQL Server tempdb), repliziert werden.

### <a name="replicate-vms"></a>Replizieren von VMs

Bevor Sie beginnen, verschaffen Sie sich schnell einen Überblick per Video

>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video3-Protect-VMware-Virtual-Machines/player]

1. Klicken Sie auf **Schritt 2: Replizieren Sie die Anwendung** > **Quelle**.
2. Wählen Sie unter **Quelle** den Konfigurationsserver aus.
3. Wählen Sie unter **Computertyp** die Option **Virtuelle Computer**.
4. Wählen Sie unter **vCenter-/vSphere-Hypervisor** den vCenter-Server aus, der den vSphere-Host verwaltet, oder wählen Sie den Host aus.
5. Wählen Sie den Prozessserver aus. Wenn Sie keine zusätzlichen Prozessserver erstellt haben, ist dies der Konfigurationsserver. Klicken Sie dann auf **OK**.

    ![Replikation aktivieren](./media/site-recovery-vmware-to-azure/enable-replication2.png)

6. Wählen Sie unter **Ziel** das Abonnement und die Ressourcengruppe aus, in dem bzw. der Sie die VMs erstellen möchten, für die ein Failover durchgeführt wurde. Wählen Sie das Bereitstellungsmodell aus, das in Azure für die VMs verwendet werden soll, für die ein Failover durchgeführt wurde (klassisch oder Resource Manager).


7. Wählen Sie das Azure-Speicherkonto aus, das Sie für die Replikation von Daten verwenden möchten. Falls Sie kein bereits eingerichtetes Konto verwenden möchten, können Sie ein neues Konto erstellen.

8. Wählen Sie das Azure-Netzwerk und das Subnetz aus, mit dem virtuelle Azure-Computer, die nach einem Failover erstellt werden, eine Verbindung herstellen. Wählen Sie die Option **Jetzt für die ausgewählten Computer konfigurieren** aus, um die Netzwerkeinstellung auf alle Computer anzuwenden, die geschützt werden sollen. Wählen Sie **Später konfigurieren** aus, um das Azure-Netzwerk pro Computer auszuwählen. Falls Sie kein vorhandenes Netzwerk verwenden möchten, können Sie ein Netzwerk erstellen.

    ![Replikation aktivieren](./media/site-recovery-vmware-to-azure/enable-rep3.png)
9. Klicken Sie auf **Virtuelle Computer** > **Virtuelle Computer auswählen**, und wählen Sie die Computer aus, die Sie replizieren möchten. Sie können nur Computer auswählen, für die die Replikation aktiviert werden kann. Klicken Sie dann auf **OK**.

    ![Replikation aktivieren](./media/site-recovery-vmware-to-azure/enable-replication5.png)
10. Geben Sie unter **Eigenschaften** > **Eigenschaften konfigurieren**das Konto aus, das der Prozessserver zum automatischen Installieren des Mobilitätsdiensts auf dem Computer verwenden soll.
11. Standardmäßig werden alle Datenträger repliziert. Klicken Sie auf **Alle Datenträger** , und entfernen Sie alle Datenträger, die Sie nicht replizieren möchten. Klicken Sie dann auf **OK**. Sie können später weitere VM-Eigenschaften festlegen.

    ![Replikation aktivieren](./media/site-recovery-vmware-to-azure/enable-replication6.png)
11. Überprüfen Sie unter **Replikationseinstellungen** > **Replikationseinstellungen konfigurieren**, ob die richtige Replikationsrichtlinie ausgewählt ist. Wenn Sie eine Richtlinie ändern, werden die Änderungen auf den replizierenden Computer und neue Computer angewendet.
12. Aktivieren Sie **Multi-VM-Konsistenz** , wenn Sie Computer in einer Replikationsgruppe zusammenfassen möchten, und geben Sie einen Namen für die Gruppe an. Klicken Sie dann auf **OK**. Beachten Sie Folgendes:

    * Computer in Replikationsgruppen werden gemeinsam repliziert und verfügen beim Failover über gemeinsame ausfallsichere und anwendungskonsistente Wiederherstellungspunkte.
    * Es wird empfohlen, dass Sie virtuelle Computer und physische Server zusammenfassen, damit sie Ihre Workloads widerspiegeln. Das Aktivieren von Multi-VM-Konsistenz kann sich auf die Leistung der Workload auswirken und sollte nur verwendet werden, wenn Computer die gleiche Workload ausführen und Sie Konsistenz benötigen.

    ![Replikation aktivieren](./media/site-recovery-vmware-to-azure/enable-replication7.png)
13. Klicken Sie auf **Replikation aktivieren**. Sie können den Fortschritt des Auftrags **Schutz aktivieren** unter **Einstellungen** > **Aufträge** > **Site Recovery-Aufträge** verfolgen. Nachdem der Auftrag **Schutz abschließen** ausgeführt wurde, ist der Computer bereit für das Failover.

Nach dem Aktivieren der Replikation wird der Mobilitätsdienst installiert, wenn Sie die Pushinstallation einrichten. Nachdem die Pushinstallation für den Mobilitätsdienst auf einer VM installiert wurde, wird ein Schutzauftrag gestartet und verursacht einen Fehler. Nach dem Fehler müssen Sie jeden Computer manuell neu starten. Anschließend wird der Schutzauftrag erneut gestartet, und die erste Replikation wird ausgeführt.



### <a name="view-and-manage-vm-properties"></a>Anzeigen und Verwalten von VM-Eigenschaften

Wir empfehlen Ihnen, die VM-Eigenschaften zu überprüfen und die erforderlichen Änderungen daran vorzunehmen.

1. Klicken Sie auf **Replizierte Elemente**, und wählen Sie den Computer aus. Das Blatt **Zusammenfassung** enthält Informationen zu den Einstellungen und zum Status der Computer.
2. Unter **Eigenschaften** können Sie die Informationen zur Replikation und zum Failover für den virtuellen Computer anzeigen.
3. Unter **Compute und Netzwerk** > **Compute-Eigenschaften** können Sie den Namen und die Zielgröße des virtuellen Azure-Computers angeben. Ändern Sie ggf. den Namen, damit er die [Azure-Anforderungen](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) erfüllt.
4. Ändern Sie die Einstellungen für Zielnetzwerk, Subnetz und IP-Adresse, die der Azure-VM zugewiesen werden:

   - Sie können die Ziel-IP-Adresse festlegen.

    - Wenn Sie keine Adresse angeben, wird für den Computer, für den das Failover durchgeführt wurde, DHCP verwendet.
    - Wenn Sie eine Adresse festlegen, die beim Failover nicht verfügbar ist, funktioniert das Failover nicht.
    - Die gleiche Ziel-IP-Adresse kann für das Testfailover verwendet werden, wenn die Adresse im Testfailover-Netzwerk verfügbar ist.

   - Die Anzahl der Netzwerkkarten hängt von der Größe ab, die Sie für den virtuellen Zielcomputer angeben:

     - Wenn die Anzahl der Netzwerkkarten des Quellcomputers maximal der Anzahl der Netzwerkkarten entspricht, die für die Größe des Zielcomputers zulässig ist, hat der Zielcomputer die gleiche Anzahl von Netzwerkkarten wie der Quellcomputer.
     - Wenn die Anzahl der Netzwerkadapter für den virtuellen Quellcomputer die maximal zulässige Anzahl für die Größe des Zielcomputers übersteigt, wird die Anzahl verwendet, die maximal für die Größe des Zielcomputers zulässig ist.
     - Wenn beispielsweise ein Quellcomputer zwei Netzwerkadapter besitzt und der Zielcomputer aufgrund seiner Größe vier Netzwerkadapter unterstützt, erhält der Zielcomputer zwei Netzwerkadapter. Wenn der Quellcomputer dagegen zwei Netzwerkadapter besitzt und der Zielcomputer aufgrund seiner Größe nur einen Adapter unterstützt, erhält der Zielcomputer nur einen Adapter.     
   - Wenn der virtuelle Computer über mehrere Netzwerkadapter verfügt, werden alle mit dem gleichen Netzwerk verbunden.
   - Wenn der virtuelle Computer über mehrere Netzwerkadapter verfügt, wird der erste, der in der Liste angezeigt wird, zum *Standard*-Netzwerkadapter im virtuellen Azure-Computer.
4. Unter **Datenträger** werden das VM-Betriebssystem und die Datenträger angezeigt, die repliziert werden.

#### <a name="managed-disks"></a>Verwaltete Datenträger

Unter **Compute und Netzwerk** > **Compute-Eigenschaften** können Sie die Einstellung „Verwaltete Datenträger verwenden“ für den virtuellen Computer auf „Ja“ festlegen, wenn Sie Ihrem Computer beim Failover zu Azure verwaltete Datenträger anfügen möchten. Managed Disks vereinfacht die Datenträgerverwaltung für Azure-IaaS-VMs durch die Verwaltung der Speicherkonten, die den VM-Datenträgern zugeordnet sind. Weitere Informationen zu [Managed Disks](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview).

   - Verwaltete Datenträger werden erstellt und nur beim Failover zu Azure an den virtuellen Computer angefügt. Beim Aktivieren des Schutzes werden Daten von lokalen Computern weiterhin auf Speicherkonten repliziert.  Verwaltete Datenträger können nur für virtuelle Computer erstellt werden, die über das Resource Manager-Bereitstellungsmodell bereitgestellt werden.  

   - Wenn Sie „Verwaltete Datenträger verwenden“ auf „Ja“ festlegen, stehen nur Verfügbarkeitsgruppen in der Ressourcengruppe zur Auswahl, bei denen „Verwaltete Datenträger verwenden“ auf „Ja“ festgelegt ist. Der Grund hierfür ist, dass virtuelle Computer mit verwalteten Datenträgern nur in Verfügbarkeitsgruppen enthalten sein können, deren Eigenschaft „Verwaltete Datenträger verwenden“ auf „Ja“ festgelegt ist. Stellen Sie sicher, dass die Eigenschaft „Verwaltete Datenträgern verwenden“ der erstellten Verfügbarkeitsgruppen Ihrer Absicht entspricht, verwaltete Datenträger beim Failover zu verwenden.  In gleicher Weise gilt: Wenn Sie „Verwaltete Datenträger verwenden“ auf „Nein“ festlegen, stehen nur Verfügbarkeitsgruppen in der Ressourcengruppe zur Auswahl, deren Eigenschaft „Verwaltete Datenträger verwenden“ auf „Nein“ festgelegt ist. [Weitere Informationen zu verwalteten Datenträgern und Verfügbarkeitsgruppen](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability#use-managed-disks-for-vms-in-an-availability-set).

  > [!NOTE]
  > Wenn das für die Replikation verwendete Speicherkonto zu einem beliebigen Zeitpunkt mit der Speicherdienstverschlüsselung verschlüsselt wurde, tritt bei der Erstellung verwalteter Datenträger während des Failovers ein Fehler auf. Sie können entweder „Verwaltete Datenträger verwenden“ auf „Nein“ festlegen und den Failoverversuch wiederholen oder den Schutz für den virtuellen Computer deaktivieren und diesen in einem Speicherkonto schützen, für das die Speicherdienstverschlüsselung zu keinem Zeitpunkt aktiviert war.
  > [Weitere Informationen zu Speicherdienstverschlüsselung und verwalteten Datenträgern](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview#managed-disks-and-encryption).


## <a name="run-a-test-failover"></a>Durchführen eines Test-Failovers


Führen Sie nach Abschluss der Einrichtung ein Testfailover durch, um sicherzustellen, dass alles wie erwartet funktioniert. Bevor Sie beginnen, verschaffen Sie sich schnell einen Überblick per Video
>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video4-Recovery-Plan-DR-Drill-and-Failover/player]


1. Klicken Sie zum Durchführen eines Failovers für einen einzelnen Computer unter **Einstellungen** > **Replizierte Elemente** auf den virtuellen Computer und dann auf das Symbol **+Testfailover**.

    ![Testfailover](./media/site-recovery-vmware-to-azure/TestFailover.png)

1. Klicken Sie zum Durchführen eines Failovers für einen Wiederherstellungsplan unter **Einstellungen** > **Wiederherstellungspläne** mit der rechten Maustaste auf den Plan, und klicken Sie dann auf **Testfailover**. Eine Anleitung zum Erstellen eines Wiederherstellungsplans finden Sie [hier](site-recovery-create-recovery-plans.md).  

1. Wählen Sie unter **Testfailover** das Azure-Netzwerk aus, mit dem Azure-VMs nach dem Failover verbunden werden.

1. Klicken Sie auf **OK**, um den Failovervorgang zu starten. Sie können den Fortschritt verfolgen, indem Sie auf den virtuellen Computer klicken, um die Eigenschaften zu öffnen. Alternativ können Sie unter „Tresorname“ > **Einstellungen** > **Aufträge** > **Site Recovery-Aufträge** auf den Auftrag **Testfailover** klicken.

1. Nach Abschluss des Failovers sollte der Azure-Replikatcomputer im Azure-Portal unter **Virtuelle Computer** angezeigt werden. Stellen Sie sicher, dass die VM die richtige Größe hat, mit dem richtigen Netzwerk verbunden ist und ausgeführt wird.

1. Wenn Sie die [Vorbereitung für Verbindungen nach dem Failover](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover) durchgeführt haben, sollten Sie eine Verbindung mit dem virtuellen Azure-Computer herstellen können.

1. Klicken Sie anschließend auf dem Wiederherstellungsplan auf **Cleanup-Test-Failover** (Testfailover bereinigen). Erfassen und speichern Sie unter **Notizen** alle Beobachtungen im Zusammenhang mit dem Testfailover. Dadurch werden die während des Testfailovers erstellten virtuellen Computer gelöscht.

[Erfahren Sie mehr](site-recovery-test-failover-to-azure.md) über Testfailover.


## <a name="vmware-account-permissions"></a>VMware-Kontoberechtigungen

Für Site Recovery ist der Zugriff auf VMware erforderlich, damit der Prozessserver VMs automatisch ermitteln und das Failover und Failback von VMs durchgeführt werden kann.

- **Migrieren**: Wenn Sie nur VMware-VMs zu Azure migrieren möchten, ohne jemals ein Failback durchzuführen, können Sie ein VMware-Konto mit einer schreibgeschützten Rolle verwenden. Mit einer Rolle dieser Art kann ein Failover durchgeführt werden, aber es ist nicht möglich, geschützte Quellcomputer herunterzufahren. Dies ist für die Migration nicht erforderlich.
- **Replizieren/Wiederherstellen**: Wenn Sie die vollständige Replikation (Replizieren, Failover, Failback) bereitstellen möchten, müssen mit dem Konto bestimmte Vorgänge möglich sein, z.B. das Erstellen und Entfernen von Datenträgern, das Aktivieren von VMs usw.
- **Automatische Ermittlung**: Mindestens ein schreibgeschütztes Konto ist erforderlich.


**Aufgabe** | **Erforderliches Konto bzw. Rolle** | **Berechtigungen** | **Details**
--- | --- | --- | ---
**Prozessserver ermittelt VMware-VMs automatisch** | Sie benötigen mindestens einen Benutzer mit Lesezugriff. | Data Center object (Rechenzentrenobjekt) –> Propagate to Child Object (An untergeordnetes Objekt weitergeben), role=Read-only (Rolle=schreibgeschützt) | Der Benutzer wird auf Datencenterebene zugewiesen und hat Zugriff auf alle Objekte im Datencenter.<br/><br/> Um den Zugriff einzuschränken, weisen Sie den untergeordneten Objekten (vSphere-Hosts, Datenspeicher, VMs und Netzwerke) die Rolle **No access** (Kein Zugriff) mit **Propagate to child object** (Auf untergeordnetes Objekt übertragen) zu.
**Failover** | Sie benötigen mindestens einen Benutzer mit Lesezugriff. | Data Center object (Rechenzentrenobjekt) –> Propagate to Child Object (An untergeordnetes Objekt weitergeben), role=Read-only (Rolle=schreibgeschützt) | Der Benutzer wird auf Datencenterebene zugewiesen und hat Zugriff auf alle Objekte im Datencenter.<br/><br/> Um den Zugriff einzuschränken, weisen Sie den untergeordneten Objekten (vSphere-Hosts, Datenspeicher, VMs und Netzwerke) die Rolle **No access** (Kein Zugriff) mit **Propagate to child object** (Auf untergeordnetes Objekt übertragen) zu.<br/><br/> Ist für Migrationszwecke geeignet, aber nicht für die vollständige Replikation, Failover oder Failback.
**Failover und Failback** | Wir empfehlen, dass Sie eine Rolle (Azure_Site_Recovery) mit den erforderlichen Berechtigungen erstellen und sie dann einem VMware-Benutzer oder einer VMware-Gruppe zuweisen | Rechenzentrumsobjekt -> An untergeordnetes Objekt weitergeben, Rolle=Azure_Site_Recovery<br/><br/> Datenspeicher -> Speicherplatz zuordnen, Datenspeicher durchsuchen, Low-Level-Dateivorgänge, Datei entfernen, Dateien virtueller Computer aktualisieren<br/><br/> Netzwerk -> Netzwerk zuweisen<br/><br/> Ressource -> Zuweisen der VM zu einem Ressourcenpool, ausgeschaltete VM migrieren, eingeschaltete VM migrieren<br/><br/> Tasks (Aufgaben) -> Create task (Aufgabe erstellen), update task (Aufgabe aktualisieren)<br/><br/> Virtueller Computer -> Konfiguration<br/><br/> Virtueller Computer -> Interagieren -> Frage beantworten, Geräteverbindung, CD-Medien konfigurieren, Diskettenmedien konfigurieren, Ausschalten, Einschalten, VMware-Tools installieren<br/><br/> Virtueller Computer -> Inventar -> Erstellen, Registrieren, Registrierung aufheben<br/><br/> Virtueller Computer -> Bereitstellung -> Download virtueller Computer zulassen, Upload von Dateien virtueller Computer zulassen<br/><br/> Virtual machine -> Snapshots -> Remove snapshots | Der Benutzer wird auf Datencenterebene zugewiesen und hat Zugriff auf alle Objekte im Datencenter.<br/><br/> Um den Zugriff einzuschränken, weisen Sie den untergeordneten Objekten (vSphere-Hosts, Datenspeicher, VMs und Netzwerke) die Rolle **No access** (Kein Zugriff) mit **Propagate to child object** (Auf untergeordnetes Objekt übertragen) zu.


## <a name="next-steps"></a>Nächste Schritte

Nach der vollständigen Einrichtung der Replikation führen Sie bei einem Ausfall ein Failover zu Azure durch, und Azure-VMs werden aus den replizierten Daten erstellt. Sie können dann auf Workloads und Apps in Azure zugreifen, bis das Failback an Ihren primären Standort erfolgt, sobald wieder Normalbetrieb herrscht.

- [Informieren Sie sich](site-recovery-failover.md) über die unterschiedlichen Failoverarten und deren Durchführung.
- Wenn Sie keine Replikation und kein Failback durchführen, sondern Computer migrieren möchten, [helfen Ihnen diese Informationen weiter](site-recovery-migrate-to-azure.md#migrate-on-premises-vms-and-physical-servers).
- [Informieren Sie sich über Failbacks](site-recovery-failback-azure-to-vmware.md), um für Azure-VMs Failbacks und Replikationen zurück an den primären lokalen Standort durchzuführen.

## <a name="third-party-software-notices-and-information"></a>Hinweise und Informationen zu Drittanbietersoftware
Do Not Translate or Localize

The software and firmware running in the Microsoft product or service is based on or incorporates material from the projects listed below (collectively, “Third Party Code”).  Microsoft is the not original author of the Third Party Code.  The original copyright notice and license, under which Microsoft received such Third Party Code, are set forth below.

The information in Section A is regarding Third Party Code components from the projects listed below. Such licenses and information are provided for informational purposes only.  This Third Party Code is being relicensed to you by Microsoft under Microsoft's software licensing terms for the Microsoft product or service.  

The information in Section B is regarding Third Party Code components that are being made available to you by Microsoft under the original licensing terms.

The complete file may be found on the [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=529428). Microsoft reserves all rights not expressly granted herein, whether by implication, estoppel or otherwise.

