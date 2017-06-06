---
title: Testfailover in Azure in Site Recovery | Microsoft Docs
description: "Erfahren Sie, wie Sie ein Testfailover (lokal zu Azure) durchführen."
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: pratshar
ms.translationtype: Human Translation
ms.sourcegitcommit: 64bd7f356673b385581c8060b17cba721d0cf8e3
ms.openlocfilehash: 0df4b3535449c88f11fa7a58811f68c82549558f
ms.contentlocale: de-de
ms.lasthandoff: 05/02/2017


---
# <a name="test-----failover-to-azure-in-site-recovery"></a>Testfailover in Azure in Site Recovery
> [!div class="op_single_selector"]
> * [Testfailover auf Azure](./site-recovery-test-failover-to-azure.md)
> * [Testfailover (VMM zu VMM)](./site-recovery-test-failover-vmm-to-vmm.md)


Dieser Artikel enthält Informationen und Anweisungen zum Ausführen eines Testfailovers oder eines DR-Drills von virtuellen Computern und physischen Servern, die mit Site Recovery und Azure als Wiederherstellungsstandort geschützt sind.

Kommentare oder Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)veröffentlichen.

Ein Testfailover wird ausgeführt, um Ihre Replikationsstrategie zu überprüfen oder einen Notfallwiederherstellungsdrill ohne Datenverlust oder Ausfallzeiten durchzuführen. Die Ausführung eines Testfailovers hat keinerlei Auswirkungen auf die laufende Replikation oder Ihre Produktionsumgebung. Ein Testfailover kann auf einem virtuellen Computer oder in einem [Wiederherstellungsplan](site-recovery-create-recovery-plans.md) ausgeführt werden. Beim Auslösen eines Testfailovers müssen Sie das Netzwerk angeben, mit dem virtuelle Testcomputer eine Verbindung herstellen. Nach dem Auslösen eines Testfailovers können Sie den Fortschritt auf der Seite **Aufträge** nachverfolgen.  


## <a name="supported-scenarios"></a>Unterstützte Szenarien
Das Testfailover wird in allen Bereitstellungsszenarien außer bei einer [Legacybereitstellung eines VMware-Standorts in Azure](site-recovery-vmware-to-azure-classic-legacy.md) unterstützt. Das Testfailover wird außerdem nicht unterstützt, wenn für den virtuellen Computer ein Failover auf Azure ausgeführt wurde.  


## <a name="run-a-test-failover"></a>Durchführen eines Test-Failovers
Hier erfahren Sie, wie Sie ein Test-Failover für einen Wiederherstellungsplan durchführen. Alternativ können Sie auch ein Testfailover für einen einzelnen Computer mithilfe der entsprechenden Option darauf ausführen.

![Testfailover](./media/site-recovery-test-failover-to-azure/TestFailover.png)


1. Wählen Sie **Wiederherstellungspläne** > *Name des Wiederherstellungsplans* aus. Klicken Sie auf **Testfailover**.
1. Wählen Sie einen **Wiederherstellungspunkt** für ein Failover aus. Sie können eine der folgenden Optionen auswählen:
    1.  **Latest processed** (Zuletzt verarbeitet): Diese Option führt ein Failover für alle virtuellen Computer des Wiederherstellungsplans auf den letzten Wiederherstellungspunkt durch, der bereits vom Site Recovery-Dienst verarbeitet wurde. Wenn Sie das Testfailover für einen virtuellen Computer durchführen, wird zusätzlich der Zeitstempel des zuletzt verarbeiteten Wiederherstellungspunkts angezeigt. Wenn Sie das Failover eines Wiederherstellungsplans durchführen, können Sie zu einzelnen virtuellen Computern wechseln und die Kachel **Neueste Wiederherstellungspunkte** anzeigen, um die entsprechenden Informationen abzurufen. Da keine Zeit mit der Verarbeitung nicht verarbeiteter Daten verbracht wird, bietet diese Option die Möglichkeit zum Failover mit geringem RTO-Wert (Recovery Time Objective, angestrebte Wiederherstellungszeit).
    1.    **Latest app-consistent** (Neueste anwendungskonsistente Elemente): Diese Option führt ein Failover für alle virtuellen Computer des Wiederherstellungsplans auf den letzten anwendungskonsistenten Wiederherstellungspunkt durch, der bereits von Site Recovery verarbeitet wurde. Wenn Sie das Testfailover für einen virtuellen Computer ausführen, wird zusätzlich der Zeitstempel des letzten anwendungskonsistenten Wiederherstellungspunkts angezeigt. Wenn Sie das Failover eines Wiederherstellungsplans durchführen, können Sie zu einzelnen virtuellen Computern wechseln und die Kachel **Neueste Wiederherstellungspunkte** anzeigen, um die entsprechenden Informationen abzurufen.
    1.    **Latest** (Neueste): Diese Option verarbeitet zuerst alle Daten, die an einen Site Recovery-Dienst gesendet wurden, um vor dem Failover einen Wiederherstellungspunkt für jeden virtuellen Computer zu erstellen. Diese Option bietet die niedrigste RPO (Recovery Point Objective), da der nach dem Failover erstellte virtuelle Computer über alle Daten verfügt, die bei Auslösung des Failovers im Site Recovery-Dienst repliziert wurden.
    1.    **Benutzerdefiniert**: Beim Ausführen des Testfailovers für einen virtuellen Computer können Sie diese Option verwenden, um ein Failover auf einen bestimmten Wiederherstellungspunkt durchzuführen.
1. Wählen Sie ein **virtuelles Azure-Netzwerk** aus: Stellen Sie ein virtuelles Azure-Netzwerk bereit, in dem die virtuellen Testcomputer erstellt werden. Site Recovery versucht, virtuelle Testcomputer in einem Subnetz mit demselben Namen und derselben IP zu erstellen, die in den Einstellungen des virtuellen Computers unter **Compute und Netzwerk** angegeben sind. Wenn das Subnetz mit demselben Namen nicht im virtuellen Azure-Netzwerk für das Testfailover verfügbar ist, wird ein virtueller Testcomputer im ersten Subnetz (in alphabetischer Reihenfolge) erstellt. Wenn dieselbe IP-Adresse nicht im Subnetz verfügbar ist, erhält der virtuelle Computer eine andere verfügbare IP-Adresse im Subnetz. Weitere Informationen finden Sie in [diesem](#creating-a-network-for-test-failover) Abschnitt.
1. Wenn Sie ein Failover auf Azure ausführen und die Datenverschlüsselung aktiviert ist, wählen Sie unter **Verschlüsselungsschlüssel** das Zertifikat aus, das beim Aktivieren der Datenverschlüsselung im Rahmen der Anbieterinstallation ausgestellt wurde. Wenn Sie die Verschlüsselung auf dem virtuellen Computer nicht aktiviert haben, können Sie diesen Schritt ignorieren.
1. Verfolgen Sie den Verlauf des Failovers auf der Registerkarte **Aufträge** . Der Test-Replikatcomputer wird im Azure-Portal angezeigt.
1. Zum Initiieren einer RDP-Verbindung auf dem virtuellen Computer müssen Sie auf der Netzwerkschnittstelle des virtuellen Computers, für den ein Failover durchgeführt wurde, [eine öffentliche IP hinzufügen](site-recovery-monitoring-and-troubleshooting.md#adding-a-public-ip-on-a-resource-manager-virtual-machine). Wenn Sie ein Failover auf einen klassischen virtuellen Computer durchführen, müssen Sie an Port 3389 [einen Endpunkt hinzufügen](../virtual-machines/windows/classic/setup-endpoints.md).
1. Klicken Sie anschließend auf dem Wiederherstellungsplan auf **Cleanup-Test-Failover** (Testfailover bereinigen). Erfassen und speichern Sie unter **Notizen** alle Beobachtungen im Zusammenhang mit dem Test-Failover. Dadurch werden die während des Testfailovers erstellten virtuellen Computer gelöscht.


> [!TIP]
> Site Recovery versucht, virtuelle Testcomputer in einem Subnetz mit demselben Namen und derselben IP zu erstellen, die in den Einstellungen des virtuellen Computers unter **Compute und Netzwerk** angegeben sind. Wenn das Subnetz mit demselben Namen nicht im virtuellen Azure-Netzwerk für das Testfailover verfügbar ist, wird ein virtueller Testcomputer im ersten Subnetz (in alphabetischer Reihenfolge) erstellt. Wenn die Ziel-IP Teil des ausgewählten Subnetzes ist, versucht Site Recovery, die Testfailover-VM mit der Ziel-IP zu erstellen. Wenn die Ziel-IP nicht Teil des ausgewählten Subnetzes ist, wird die Testfailover-VM mit einer beliebigen verfügbaren IP im ausgewählten Subnetz erstellt.
>
>

## <a name="test-failover-job"></a>Testfailoverauftrag

![Testfailover](./media/site-recovery-test-failover-to-azure/TestFailoverJob.png)

Die Auslösung eines Testfailovers ist mit den folgenden Schritten verbunden:

1. Prüfung der erforderlichen Komponenten: Mit diesem Schritt wird sichergestellt, dass alle Bedingungen erfüllt sind, die für das Failover erforderlich sind.
1. Failover: In diesem Schritt werden die Daten verarbeitet und vorbereitet, damit daraus ein virtueller Azure-Computer erstellt werden kann. Wenn Sie den Wiederherstellungspunkt **Latest** (Neueste) gewählt haben, wird in diesem Schritt ein Wiederherstellungspunkt aus den Daten erstellt, die an den Dienst gesendet wurden.
1. Start: In diesem Schritt wird ein virtueller Azure-Computer aus den Daten erstellt, die im vorherigen Schritt verarbeitet wurden.

## <a name="time-taken-for-failover"></a>Zeitaufwand für das Failover

In bestimmten Fällen erfordert das Failover virtueller Computer einen zusätzliche Zwischenschritt, der in der Regel ca. 8 bis 10 Minuten dauert. Das gilt für die folgenden Fälle:

* VMware-VMs mit Mobility Service-Version niedriger als 9.8
* Physische Server 
* VMware-Linux-VMs
* Als physische Server geschützte Hyper-V-VMs
* VMware-VMs, bei denen die folgenden Treiber nicht als Starttreiber vorhanden sind 
    * storvsc 
    * vmbus 
    * storflt 
    * intelide 
    * atapi
* VMware-VMs, bei denen der DHCP-Dienst nicht aktiviert ist, unabhängig davon, ob DHCP oder statischen IP-Adressen verwendet werden

In allen anderen Fällen ist dieser Zwischenschritt nicht erforderlich und der Zeitaufwand für das Failover deutlich niedriger. 


## <a name="creating-a-network-for-test-failover"></a>Erstellen eines Netzwerks für Testfailover
Beim Durchführen eines Testfailovers wird empfohlen, ein Netzwerk zu wählen, das vom Site Recovery-Netzwerk für die Produktion isoliert ist, das Sie in den Einstellungen für den virtuellen Computer unter **Compute und Netzwerk** angegeben haben. Standardmäßig ist ein virtuelles Azure-Netzwerk beim Erstellen von anderen Netzwerken isoliert. Dieses Netzwerk sollte Ihr Produktionsnetzwerk imitieren:

1. Das Testnetzwerk muss dieselbe Anzahl von Subnetzen wie in Ihrem Produktionsnetzwerk und dieselben Namen wie die Subnetze in Ihrem Produktionsnetzwerk aufweisen.
1. Das Testnetzwerk muss denselben IP-Adressbereich wie Ihr Produktionsnetzwerk verwenden.
1. Aktualisieren Sie den DNS des Testnetzwerks entsprechend der IP-Adresse, die Sie als Ziel-IP für den virtuellen DNS-Computer in den Einstellungen unter **Compute und Netzwerk** angegeben haben. Unter [Überlegungen zum Testfailover für Active Directory](site-recovery-active-directory.md#test-failover-considerations) finden Sie weitere Details.


## <a name="test-failover-to-a-production-network-on-recovery-site"></a>Testfailover auf ein Produktionsnetzwerk am Wiederherstellungsstandort
Beim Durchführen eines Testfailovers wird empfohlen, ein anderes Netzwerk als das Site Recovery-Netzwerk für die Produktion zu wählen, das Sie in den Einstellungen für den virtuellen Computer unter **Compute und Netzwerk** angegeben haben. Beachten Sie folgende Punkte, wenn Sie wirklich die End-to-End-Netzwerkkonnektivität auf einem virtuellen Computer bewerten möchten, für den ein Failover durchgeführt wurde:

1. Der primäre virtuelle Computer muss heruntergefahren werden, wenn Sie das Testfailover durchführen. Andernfalls werden zwei virtuelle Computer mit derselben Identität gleichzeitig im selben Netzwerk ausgeführt, was unerwünschte Folgen nach sich ziehen kann.
1. Änderungen, die Sie auf virtuellen Computern mit Testfailover vornehmen, würden verloren gehen, wenn Sie die virtuellen Computer mit Testfailover bereinigen. Diese Änderungen werden nicht wieder auf dem primären virtuellen Computer repliziert.
1. Diese Art des Testens von Leads führt zu einem Ausfall Ihrer Produktionsanwendung. Benutzer der Anwendung sollten aufgefordert werden, die Anwendung während des DR-Drills nicht zu verwenden.  



## <a name="prepare-active-directory-and-dns"></a>Vorbereiten von Active Directory und DNS
Wenn Sie ein Test-Failover durchführen möchten, um eine Anwendung zu testen, muss in der Testumgebung eine Kopie der Active Directory-Produktionsumgebung vorhanden sein. Unter [Überlegungen zum Testfailover für Active Directory](site-recovery-active-directory.md#test-failover-considerations) finden Sie weitere Details.

## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Vorbereiten der Verbindungsherstellung mit Azure-VMs nach dem Failover

Wenn Sie nach dem Failover per RDP eine Verbindung mit Azure-VMs herstellen möchten, müssen Sie die in der Tabelle zusammengefassten Aktionen durchführen.

**Failover** | **Standort** | **Aktionen**
--- | --- | ---
**Virtueller Azure-Computer unter Windows** | Lokaler Computer vor dem Failover | Aktivieren Sie für den Zugriff auf den virtuellen Azure-Computer über das Internet RDP. Stellen Sie sicher, dass TCP- und UDP-Regeln für **Öffentlich** hinzugefügt werden und dass RDP unter **Windows-Firewall** > **Zugelassene Apps** für alle Profile zugelassen ist.<br/><br/> Aktivieren Sie für den Zugriff über eine Standort-zu-Standort-Verbindung RDP auf dem Computer, und stellen Sie sicher, dass RDP unter **Windows-Firewall** -> **Zugelassene Apps und Features** für die Netzwerke vom Typ **Domäne und Privat** zugelassen ist.<br/><br/>  Achten Sie darauf, dass die SAN-Richtlinie des Betriebssystems auf **OnlineAll** festgelegt ist. [Weitere Informationen](https://support.microsoft.com/kb/3031135).<br/><br/> Stellen Sie sicher, dass auf dem virtuellen Computer keine ausstehenden Windows-Updates vorhanden sind, wenn Sie ein Failover auslösen. Ansonsten wird Windows Update möglicherweise während des Failovers gestartet, und Sie können sich nicht mehr am virtuellen Computer anmelden, bis das Update abgeschlossen ist. <br/><br/>
**Virtueller Azure-Computer unter Windows** | Auf der Azure-VM nach einem Failover | Für einen klassischen virtuellen Computer[fügen Sie einen öffentlichen Endpunkt](../virtual-machines/windows/classic/setup-endpoints.md) für das RDP-Protokoll (Port 3389) hinzu.<br/><br/>  Auf einem virtuellen Resource Manager-Computer [fügen Sie eine öffentliche IP-Adresse hinzu](site-recovery-monitoring-and-troubleshooting.md#adding-a-public-ip-on-a-resource-manager-virtual-machine).<br/><br/> In den Netzwerksicherheitsgruppen-Regeln auf dem virtuellen Computer nach dem Failover und für das Azure-Subnetz, mit dem der virtuelle Computer verbunden ist, müssen eingehende Verbindungen über den RDP-Port zulässig sein.<br/><br/> Für einen virtuellen Resource Manager-Computer finden Sie unter **Startdiagnose** einen Screenshot des virtuellen Computers.<br/><br/> Wenn Sie keine Verbindung herstellen können, stellen Sie sicher, dass der virtuelle Computer ausgeführt wird, und sehen sich dann diese [Tipps zur Problembehandlung](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) an.<br/><br/>
**Virtueller Azure-Computer unter Linux** | Lokaler Computer vor dem Failover | Stellen Sie sicher, dass der Secure Shell-Dienst auf der Azure-VM so festgelegt ist, dass er beim Systemstart automatisch gestartet wird.<br/><br/> Überprüfen Sie, ob die Firewallregeln eine SSH-Verbindung damit zulassen.
**Virtueller Azure-Computer unter Linux** | Azure-VM nach einem Failover | In den Netzwerksicherheitsgruppen-Regeln auf dem virtuellen Computer nach dem Failover und für das Azure-Subnetz, mit dem der virtuelle Computer verbunden ist, müssen eingehende Verbindungen über den SSH-Port zulässig sein.<br/><br/> Für einen klassischen virtuellen Computer sollte ein [öffentlicher Endpunkt hinzugefügt](../virtual-machines/windows/classic/setup-endpoints.md) werden, um eingehende Verbindungen für den SSH-Port (standardmäßig TCP-Port 22) zuzulassen.<br/><br/> Auf einem virtuellen Resource Manager-Computer [fügen Sie eine öffentliche IP-Adresse hinzu](site-recovery-monitoring-and-troubleshooting.md#adding-a-public-ip-on-a-resource-manager-virtual-machine).<br/><br/> Für einen virtuellen Resource Manager-Computer finden Sie unter **Startdiagnose** einen Screenshot des virtuellen Computers.<br/><br/>



## <a name="next-steps"></a>Nächste Schritte
Sobald Sie ein Testfailover erfolgreich durchgeführt haben, können Sie ein [Failover](site-recovery-failover.md) versuchen.

