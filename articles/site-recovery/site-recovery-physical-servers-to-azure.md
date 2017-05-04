---
title: Replizieren von physischen Servern zu Azure | Microsoft-Dokumentation
description: Beschreibt, wie Azure Site Recovery bereitgestellt wird, um die Replikation, das Failover und die Wiederherstellung von lokalen physischen Windows-/Linux-Servern auf Azure mithilfe des Azure-Portals zu orchestrieren.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: b0c27ca561567ff002bbb864846b7a3ea95d7fa3
ms.openlocfilehash: e093b78980f5f1aacfdf7be278ef58906eedc634
ms.lasthandoff: 04/25/2017


---
# <a name="replicate-physical-machines-to-azure-with-site-recovery"></a>Replizieren von physischen Computern in Azure mithilfe von Site Recovery


In diesem Artikel erfahren Sie, wie Sie lokale physische Computer mit dem Azure Site Recovery-Dienst über das Azure-Portal in Azure replizieren.

Wenn Sie physische Computer zu Azure migrieren möchten (nur Failover), lesen Sie [diesen Artikel](site-recovery-migrate-to-azure.md), um mehr zu erfahren.

Kommentare und Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.


## <a name="deployment-summary"></a>Zusammenfassung der Bereitstellungen

Das müssen Sie tun:

1. Überprüfen Sie die Voraussetzungen und Einschränkungen.
2. Richten Sie Azure-Netzwerk- und -Speicherkonten ein.
3. Bereiten Sie einen lokalen Computer als Konfigurationsserver vor.
4. Erstellen Sie einen Recovery Services-Tresor. Der Tresor enthält Konfigurationseinstellungen und dient zum Orchestrieren der Replikation.
5. Geben Sie Quelle, Ziel und Replikationseinstellungen an.
6. Stellen Sie den Mobilitätsdienst auf Computern bereit, die Sie replizieren möchten.
7. Aktivieren Sie die Replikation für die Computer.
7. Führen Sie ein Testfailover aus, um sicherzustellen, dass alles wie erwartet funktioniert.

## <a name="prerequisites"></a>Voraussetzungen

**Supportbedarf** | **Details**
--- | ---
**Azure** | Informieren Sie sich über die [Azure-Anforderungen](site-recovery-prereq.md#azure-requirements).
**Lokaler Konfigurationsserver** | Lokaler Computer (physischer oder virtueller VMware-Computer), auf dem Windows Server 2012 R2 oder höher ausgeführt wird. Sie richten den Konfigurationsserver während der Site Recovery-Bereitstellung ein.<br/><br/> Standardmäßig werden auch der Prozessserver und der Masterzielserver auf diesem Computer installiert. Wenn Sie zentral hochskalieren, benötigen Sie möglicherweise einen separaten Prozessserver. Es gelten die gleichen Voraussetzungen wie für den Konfigurationsserver.<br/><br/> Weitere Informationen zu diesen Komponenten finden Sie [hier](site-recovery-set-up-vmware-to-azure.md#configuration-server-minimum-requirements).
**Lokale VMs** | Auf Computern, die Sie replizieren möchten, sollte ein [unterstütztes Betriebssystem](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) ausgeführt werden, das die [Azure-Voraussetzungen](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) erfüllt.
**URLs** | Der Konfigurationsserver benötigt Zugriff auf diese URLs:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> Stellen Sie bei Verwendung von auf IP-Adressen basierenden Firewallregeln sicher, dass hierfür die Kommunikation mit Azure möglich ist.<br/></br> Lassen Sie die [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653) (IP-Bereiche für Azure-Rechenzentren) und den HTTPS-Port (443) zu.<br/></br> Lassen Sie IP-Adressbereiche für die Azure-Region Ihres Abonnements und für die Region „USA, Westen“ (wird für Zugriffsteuerung und Identitätsverwaltung verwendet) zu.<br/><br/> Lassen Sie zum Herunterladen von MySQL den Zugriff auf die URL „http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi“ zu.
**Mobilitätsdienst** | Dieser wird auf jedem Computer installiert, den Sie replizieren möchten.

## <a name="limitations"></a>Einschränkungen

**Einschränkung** | **Details**
--- | ---
**Azure** | Speicher- und Netzwerkkonten müssen sich in derselben Region wie der Tresor befinden.<br/><br/> Bei Verwendung eines Storage Premium-Kontos benötigen Sie auch ein Standardspeicherkonto zum Speichern von Replikationsprotokollen.<br/><br/> Die Replikation in Premium-Konten in den Regionen „Indien, Mitte“ und „Indien, Süden“ ist nicht möglich.
**Lokaler Konfigurationsserver** | Wenn Sie den Konfigurationsserver auf einem virtuellen VMware-Computer installieren, muss der VM-Adaptertyp „VMXNET3“ lauten. [Installieren Sie dieses Update](https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2110245&sliceId=1&docTypeID=DT_KB_1_1&dialogID=26228401&stateId=1), falls dies nicht der Fall ist.<br/><br/> Wenn Sie einen virtuellen VMware-Computer verwenden, sollte vSphere PowerCLI 6.0 darauf installiert sein.<br/><br> Der Computer sollte kein Domänencontroller sein.<br/><br/> Der Computer sollte über eine statische IP-Adresse verfügen.<br/><br/> Der Hostname sollte maximal 15 Zeichen lang sein, und es sollte ein Betriebssystem in englischer Sprache verwendet werden.
**Replizierte Computer** | Überprüfen Sie die [Einschränkungen für Azure-VMs](site-recovery-prereq.md#azure-requirements).<br/><br/> Öffnen Sie Port 20004 auf dem Computer, wenn Sie die Multi-VM-Konsistenz aktivieren möchten. Hierbei können Computer, auf denen die gleiche Workload ausgeführt wird, zusammen an einem konsistenten Datenpunkt wiederhergestellt werden.<br/><br/> Bestimmte Arten von [Linux-Speicher](site-recovery-support-matrix-to-azure.md#support-for-storage) werden unterstützt.
**Failback** | Ein Failback von Azure zu einem physischen Computer ist nicht möglich. Wenn nach einem Failover ein lokales Failback möglich sein soll, benötigen Sie eine VMware-Umgebung, sodass ein Failback zu einem virtuellen VMware-Computer durchgeführt werden kann.


## <a name="set-up-azure"></a>Einrichten von Azure

1. [Richten Sie ein Azure-Netzwerk ein](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).
    - Azure-VMs werden in diesem Netzwerk angeordnet, wenn sie nach dem Failover erstellt werden.
    - Sie können ein Netzwerk im [Resource Manager](../resource-manager-deployment-model.md)-Modus oder im klassischen Modus einrichten.

2. [Richten Sie ein Azure-Speicherkonto ein](../storage/storage-create-storage-account.md#create-a-storage-account), das für replizierte Daten verwendet werden kann.
    - Dies kann ein Standard- oder [Premium](../storage/storage-premium-storage.md)-Konto sein.
    - Sie können ein Konto im Resource Manager-Modus oder im klassischen Modus einrichten.

## <a name="prepare-the-configuration-server"></a>Bereiten Sie den Konfigurationsserver vor

1. Installieren Sie Windows Server 2012 R2 oder höher auf einem lokalen physischen Server oder auf einem virtuellen VMware-Computer.
2. Stellen Sie sicher, dass für den Computer Zugriff auf die URLs besteht, die unter den [Voraussetzungen](#prerequisites) aufgeführt sind.

## <a name="prepare-for-mobility-service-installation"></a>Vorbereitung für die Installation des Mobilitätsdiensts

Wenn Sie den Mobilitätsdienst per Pushvorgang auf dem physischen Computer installieren möchten, benötigen Sie ein Konto, das vom Prozessserver für den Zugriff auf die Computer verwendet werden kann. Das Konto wird nur für die Pushinstallation verwendet. Sie können ein Domänenkonto oder ein lokales Konto verwenden:

  - Wenn Sie kein Domänenkonto verwenden, müssen Sie für Windows die Remote-Benutzerzugriffssteuerung auf dem lokalen Computer deaktivieren. Zu diesem Zweck fügen Sie in der Registrierung unter **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System** den DWORD-Eintrag **LocalAccountTokenFilterPolicy** mit dem Wert 1 hinzu.
    - Geben Sie Folgendes ein, wenn Sie den Registrierungseintrag für Windows über eine CLI hinzufügen möchten:   ``REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.``
  - Für Linux muss das Konto einem Root-Benutzer auf dem Linux-Quellserver entsprechen.


## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]


## <a name="select-the-protection-goal"></a>Auswählen des Schutzziels

Wählen Sie aus, was Sie replizieren möchten und wohin die Daten repliziert werden sollen.

1. Klicken Sie auf **Recovery Services-Tresore** > „Tresor“.
2. Klicken Sie im Ressourcenmenü auf **Site Recovery** > **Infrastruktur vorbereiten** > **Schutzziel**.

    ![Ziele wählen](./media/site-recovery-vmware-to-azure/choose-goal-physical.PNG)
3. Wählen Sie unter **Schutzziel** die Option **In Azure** und dann **Nicht virtualisiert/Andere** aus.


## <a name="set-up-the-source-environment"></a>Einrichten der Quellumgebung

Richten Sie den Konfigurationsserver ein, registrieren Sie ihn im Tresor, und ermitteln Sie die VMs.

1. Klicken Sie auf **Site Recovery** > **Infrastruktur vorbereiten** > **Quelle**.
2. Wenn Sie keinen Konfigurationsserver verwenden, klicken Sie auf **+Konfigurationsserver**.

    ![Quelle einrichten](./media/site-recovery-vmware-to-azure/set-source1.png)
3. Überprüfen Sie unter **Server hinzufügen**, ob unter **Servertyp** die Option **Konfigurationsserver** angezeigt wird.
4. Laden Sie die Installationsdatei für das einheitliche Setup von Site Recovery herunter.
5. Laden Sie den Tresorregistrierungsschlüssel herunter. Sie benötigen diesen beim Ausführen des einheitlichen Setups. Der Schlüssel ist nach der Erstellung fünf Tage lang gültig.

   ![Quelle einrichten](./media/site-recovery-vmware-to-azure/set-source2.png)


## <a name="run-site-recovery-unified-setup"></a>Ausführen des einheitlichen Setups von Site Recovery

Führen Sie zunächst folgende Schritte aus:

- Verschaffen Sie sich schnell eine Übersicht per Video (im Video wird beschrieben, wie virtuelle VMware-Computer repliziert werden. Der Vorgang ähnelt jedoch der Replikation von physischen Computern).

    > [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video1-Source-Infrastructure-Setup/player]

- Stellen Sie auf dem Konfigurationsservercomputer sicher, dass die Systemuhr mit einem [Zeitserver](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) synchronisiert ist. Falls der Unterschied 15 Minuten beträgt, ist das Setup unter Umständen nicht erfolgreich.
- Führen Sie das Setup als lokaler Administrator auf dem Konfigurationsservercomputer aus.
- Stellen Sie sicher, dass TLS 1.0 auf dem Computer aktiviert ist.

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> Der Konfigurationsserver kann auch [über die Befehlszeile](http://aka.ms/installconfigsrv) installiert werden.


## <a name="set-up-the-target-environment"></a>Einrichten der Zielumgebung

Überprüfen Sie vor dem Einrichten der Zielumgebung, ob Sie über ein [Azure-Speicherkonto und -Netzwerk](#set-up-azure) verfügen.

1. Klicken Sie auf **Infrastruktur vorbereiten** > **Ziel**, und wählen Sie das gewünschte Azure-Abonnement aus.
2. Geben Sie an, ob Ihr Zielbereitstellungsmodell vom Typ „Resource Manager“ oder „klassisch“ ist.
3. Site Recovery prüft, ob Sie über ein oder mehrere kompatible Azure-Speicherkonten und -Netzwerke verfügen.

   ![Ziel](./media/site-recovery-vmware-to-azure/gs-target.png)
4. Gehen Sie wie folgt vor, falls Sie kein Speicherkonto oder Netzwerk erstellt haben: Klicken Sie auf **+Speicherkonto** oder **+Netzwerk**, um „inline“ ein Resource Manager-Konto oder Netzwerk zu erstellen.

## <a name="set-up-replication-settings"></a>Einrichten der Replikationseinstellungen

Verschaffen Sie sich zunächst schnell eine Übersicht per Video (im Video wird beschrieben, wie virtuelle VMware-Computer repliziert werden. Der Vorgang ähnelt jedoch der Replikation von physischen Computern).

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

Für alle Computer, die Sie replizieren möchten, muss der Mobilitätsdienst installiert sein. Sie können den Mobilitätsdienst auf verschiedene Arten installieren:

1. Installation per Pushvorgang vom Prozessserver. Für diese Methode müssen Sie Computer im Voraus vorbereiten.
2. Installation mit Bereitstellungstools, z.B. System Center Configuration Manager oder Azure Automation DSC.
3.  Manuelle Installation.

[Weitere Informationen](site-recovery-vmware-to-azure-install-mob-svc.md)


## <a name="enable-replication"></a>Replikation aktivieren

Vorbereitung:

- Beim Hinzufügen oder Ändern von VMs kann es 15 Minuten oder auch länger dauern, bis Änderungen wirksam werden und im Portal sichtbar sind.
- Sie können den letzten Zeitpunkt der Ermittlung für VMs unter **Konfigurationsserver** > **Letzter Kontakt um** überprüfen.
- Wenn Sie VMs hinzufügen möchten, ohne auf die planmäßige Ermittlung zu warten, markieren Sie den Konfigurationsserver (nicht darauf klicken) und klicken auf **Refresh** (Aktualisieren).
* Wenn eine VM für die Pushinstallation vorbereitet wurde, installiert der Prozessserver automatisch den Mobilitätsdienst, sobald Sie die Replikation aktivieren.
- Verschaffen Sie sich zunächst schnell eine Übersicht per Video (im Video wird beschrieben, wie virtuelle VMware-Computer repliziert werden. Der Vorgang ähnelt jedoch der Replikation von physischen Computern).

    >[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video3-Protect-VMware-Virtual-Machines/player]


### <a name="exclude-disks-from-replication"></a>Ausschließen von Datenträgern von der Replikation

Standardmäßig werden alle Datenträger auf einem Computer repliziert. Sie können Datenträger von der Replikation ausschließen. Beispielsweise möchten Sie möglicherweise nicht, dass Datenträger mit temporären Daten oder Daten, die bei jedem Neustart des Computers oder der Anwendung aktualisiert werden (z.B. pagefile.sys oder SQL Server tempdb), repliziert werden.

### <a name="replicate-vms"></a>Replizieren von VMs

1. Klicken Sie auf **Replicate application (Anwendung replizieren)** > **Quelle**.
2. Wählen Sie in **Quelle** die Option „Lokal“.
3. Wählen Sie unter **Quellspeicherort** den Namen des Konfigurationsservers aus.
4. Wählen Sie unter **Computertyp** die Option **Physische Computer**.
5. Wählen Sie unter **Prozessserver** den Prozessserver aus. Wenn Sie keine zusätzlichen Prozessserver erstellt haben, ist dies der Konfigurationsserver. Klicken Sie dann auf **OK**.

    ![Replikation aktivieren](./media/site-recovery-physical-to-azure/chooseVM.png)

6. Wählen Sie unter **Ziel** das Abonnement und die Ressourcengruppe aus, in dem bzw. der Sie die virtuellen Azure-Computer erstellen möchten, für die ein Failover durchgeführt wurde. Wählen Sie das Bereitstellungsmodell aus, das in Azure für die VMs verwendet werden soll, für die ein Failover durchgeführt wurde (klassisch oder Resource Manager).

7. Wählen Sie das Azure-Speicherkonto aus, das Sie für die Replikation von Daten verwenden möchten. Falls Sie kein bereits eingerichtetes Konto verwenden möchten, können Sie ein neues Konto erstellen.

8. Wählen Sie das Azure-Netzwerk und das Subnetz aus, mit dem virtuelle Azure-Computer nach dem Failover eine Verbindung herstellen. Wählen Sie die Option **Jetzt für die ausgewählten Computer konfigurieren** aus, um die Netzwerkeinstellung auf alle Computer anzuwenden, die geschützt werden sollen. Wählen Sie **Später konfigurieren** aus, um das Azure-Netzwerk pro Computer auszuwählen. Falls Sie kein vorhandenes Netzwerk verwenden möchten, können Sie ein Netzwerk erstellen.

    ![Replikation aktivieren](./media/site-recovery-physical-to-azure/targetsettings.png)
9. Klicken Sie in **Physische Computer** auf die Schaltfläche „+“, geben Sie **Name** und **IP-Adresse** ein, und wählen Sie das Betriebssystem des Computers aus, den Sie replizieren möchten.
  ![Aktivieren der Replikation](./media/site-recovery-physical-to-azure/machineselect.png) Es dauert einige Minuten, bis die Computer ermittelt und in der Liste angezeigt werden.

10. Geben Sie unter **Eigenschaften** > **Eigenschaften konfigurieren**das Konto aus, das der Prozessserver zum automatischen Installieren des Mobilitätsdiensts auf dem Computer verwenden soll.
11. Standardmäßig werden alle Datenträger repliziert. Klicken Sie auf **Alle Datenträger** , und entfernen Sie alle Datenträger, die Sie nicht replizieren möchten. Klicken Sie dann auf **OK**. Sie können später weitere VM-Eigenschaften festlegen.

    ![Replikation aktivieren](./media/site-recovery-physical-to-azure/configprop.png)
11. Überprüfen Sie unter **Replikationseinstellungen** > **Replikationseinstellungen konfigurieren**, ob die richtige Replikationsrichtlinie ausgewählt ist. Wenn Sie eine Richtlinie ändern, werden die Änderungen auf den replizierenden Computer und neue Computer angewendet.
12. Aktivieren Sie **Multi-VM-Konsistenz** , wenn Sie Computer in einer Replikationsgruppe zusammenfassen möchten, und geben Sie einen Namen für die Gruppe an. Klicken Sie dann auf **OK**. Beachten Sie Folgendes:

    * Computer in Replikationsgruppen werden gemeinsam repliziert und verfügen beim Failover über gemeinsame ausfallsichere und anwendungskonsistente Wiederherstellungspunkte.
    * Es wird empfohlen, dass Sie virtuelle Computer und physische Server zusammenfassen, damit sie Ihre Workloads widerspiegeln. Das Aktivieren von Multi-VM-Konsistenz kann sich auf die Leistung der Workload auswirken und sollte nur verwendet werden, wenn Computer die gleiche Workload ausführen und Sie Konsistenz benötigen.
    ![Replikation aktivieren](./media/site-recovery-physical-to-azure/policy.png)

13. Klicken Sie auf **Replikation aktivieren**. Sie können den Fortschritt des Auftrags **Schutz aktivieren** unter **Einstellungen** > **Aufträge** > **Site Recovery-Aufträge** verfolgen. Nachdem der Auftrag **Schutz abschließen** ausgeführt wurde, ist der Computer bereit für das Failover.

Nach dem Aktivieren der Replikation wird der Mobilitätsdienst installiert, wenn Sie die Pushinstallation einrichten. Nachdem die Pushinstallation für den Mobilitätsdienst auf einem Computer installiert wurde, wird ein Schutzauftrag gestartet und mit einem Fehler abgebrochen. Nach dem Fehler müssen Sie jeden Computer manuell neu starten. Anschließend wird der Schutzauftrag erneut gestartet, und die erste Replikation wird ausgeführt.


### <a name="view-and-manage-azure-vm-properties"></a>Anzeigen und Verwalten von Azure-VM-Eigenschaften

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
5. Unter **Datenträger** werden das VM-Betriebssystem und die Datenträger angezeigt, die repliziert werden.

## <a name="run-a-test-failover"></a>Ausführen eines Testfailovers

Führen Sie nach Abschluss der Einrichtung ein Testfailover durch, um sicherzustellen, dass alles wie erwartet funktioniert. Verschaffen Sie sich zunächst schnell eine Übersicht per Video.

>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video4-Recovery-Plan-DR-Drill-and-Failover/player]


1. Klicken Sie zum Durchführen eines Failovers für einen einzelnen Computer unter **Einstellungen** > **Replizierte Elemente** auf den virtuellen Computer und dann auf das Symbol **+Testfailover**.

    ![Testfailover](./media/site-recovery-vmware-to-azure/TestFailover.png)

2. Klicken Sie zum Durchführen eines Failovers für einen Wiederherstellungsplan unter **Einstellungen** > **Wiederherstellungspläne** mit der rechten Maustaste auf den Plan, und klicken Sie dann auf **Testfailover**. Eine Anleitung zum Erstellen eines Wiederherstellungsplans finden Sie [hier](site-recovery-create-recovery-plans.md).  
3. Wählen Sie unter **Testfailover** das Azure-Netzwerk aus, mit dem Azure-VMs nach dem Failover verbunden werden.
4. Klicken Sie auf **OK**, um den Failovervorgang zu starten. Sie können den Fortschritt verfolgen, indem Sie auf den virtuellen Computer klicken, um die Eigenschaften zu öffnen. Alternativ können Sie unter „Tresorname“ > **Einstellungen** > **Aufträge** > **Site Recovery-Aufträge** auf den Auftrag **Testfailover** klicken.
5. Nach Abschluss des Failovers sollte der Azure-Replikatcomputer im Azure-Portal unter **Virtuelle Computer** angezeigt werden. Stellen Sie sicher, dass die VM die richtige Größe hat, mit dem richtigen Netzwerk verbunden ist und ausgeführt wird.
6. Wenn Sie die [Vorbereitung für Verbindungen nach dem Failover](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover) durchgeführt haben, sollten Sie eine Verbindung mit dem virtuellen Azure-Computer herstellen können.
7. Klicken Sie anschließend auf dem Wiederherstellungsplan auf **Cleanup-Test-Failover** (Testfailover bereinigen). Erfassen und speichern Sie unter **Notizen** alle Beobachtungen im Zusammenhang mit dem Test-Failover. Dadurch werden die während des Testfailovers erstellten virtuellen Computer gelöscht.

Weitere Informationen finden Sie im Dokument [Testfailover in Azure](site-recovery-test-failover-to-azure.md).

## <a name="next-steps"></a>Nächste Schritte

Nach der vollständigen Einrichtung der Replikation führen Sie bei einem Ausfall ein Failover zu Azure durch, und Azure-VMs werden aus den replizierten Daten erstellt. Sie können dann auf Workloads und Apps in Azure zugreifen, bis das Failback an Ihren primären Standort erfolgt, sobald wieder Normalbetrieb herrscht.

- [Informieren Sie sich](site-recovery-failover.md) über die unterschiedlichen Failoverarten und deren Durchführung.
- Wenn Sie keine Replikation und kein Failback durchführen, sondern Computer migrieren möchten, [helfen Ihnen diese Informationen weiter](site-recovery-migrate-to-azure.md#migrate-on-premises-vms-and-physical-servers).
- Beim Replizieren physischer Computer können Sie nur ein Failback auf eine VMWare-Umgebung durchführen. [Hier finden Sie Informationen zum Failback](site-recovery-failback-azure-to-vmware.md).

## <a name="third-party-software-notices-and-information"></a>Hinweise und Informationen zu Drittanbietersoftware
Do Not Translate or Localize

The software and firmware running in the Microsoft product or service is based on or incorporates material from the projects listed below (collectively, “Third Party Code”).  Microsoft is the not original author of the Third Party Code.  The original copyright notice and license, under which Microsoft received such Third Party Code, are set forth below.

The information in Section A is regarding Third Party Code components from the projects listed below. Such licenses and information are provided for informational purposes only.  This Third Party Code is being relicensed to you by Microsoft under Microsoft's software licensing terms for the Microsoft product or service.  

The information in Section B is regarding Third Party Code components that are being made available to you by Microsoft under the original licensing terms.

The complete file may be found on the [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=529428). Microsoft reserves all rights not expressly granted herein, whether by implication, estoppel or otherwise.

