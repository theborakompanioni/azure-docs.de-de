---
title: "Replizieren virtueller Hyper-V-Computer in einem sekundären Standort mit Azure Site Recovery | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie virtuelle Hyper-V-Computer in VMM-Clouds mithilfe des Azure-Portals in einem sekundären VMM-Standort replizieren."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: b33a1922-aed6-4916-9209-0e257620fded
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: a2bc32193ad539053984794a411ae7307b8d8532
ms.contentlocale: de-de
ms.lasthandoff: 03/15/2017


---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site-using-the-azure-portal"></a>Replizieren von virtuellen Hyper-V-Computern in VMM-Clouds an einen sekundären VMM-Standort mithilfe des Azure-Portals
> [!div class="op_single_selector"]
> * [Azure-Portal](site-recovery-vmm-to-vmm.md)
> * [Klassisches Portal](site-recovery-vmm-to-vmm-classic.md)
> * [PowerShell – Resource Manager](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
>
>

Dieser Artikel beschreibt, wie Sie lokale virtuelle Hyper-V-Computer, die in System Center Virtual Machine Manager-Clouds (VMM-Clouds) verwaltet werden, mithilfe von [Azure Site Recovery](site-recovery-overview.md) über das Azure-Portal in sekundären Standorten replizieren. Weitere Informationen zu dieser Szenarioarchitektur finden Sie [hier](site-recovery-components.md#hyper-v-to-a-secondary-site).

Kommentare können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) hinterlassen.


## <a name="prerequisites"></a>Voraussetzungen

**Voraussetzung** | **Details**
--- | ---
**Azure** | Sie benötigen ein [Microsoft Azure](http://azure.microsoft.com/) -Konto. Für den Einstieg steht eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)zur Verfügung. [hier](https://azure.microsoft.com/pricing/details/site-recovery/) .
**VMM lokal** | Wir empfehlen die Verwendung von zwei VMM-Servern: einer am primären und einer am sekundären Standort.<br/><br/> Replikation zwischen Clouds auf einem einzelnen VMM-Server möglich.<br/><br/> Auf den VMM-Servern muss mindestens System Center 2012 SP1 mit den neuesten Updates ausgeführt werden.<br/><br/> VMM-Server benötigen Zugriff auf das Internet.
**VMM-Clouds** | Jeder VMM-Server muss über mindestens eine Cloud verfügen, und für alle Clouds muss das Hyper-V-Kapazitätsprofil festgelegt sein. <br/><br/>Die Clouds müssen mindestens eine VMM-Hostgruppe enthalten.<br/><br/> Wenn Sie nur über einen einzelnen VMM-Server verfügen, benötigt dieser mindestens zwei Clouds (primär und sekundär).
**Hyper-V** | Auf den Hyper-V-Servern muss mindestens Windows Server 2012 mit der Hyper-V-Rolle ausgeführt werden, und die neuesten Updates müssen installiert sein.<br/><br/> Ein Hyper-V-Server muss mindestens einen virtuellen Computer enthalten.<br/><br/>  Hyper-V-Hostserver müssen sich in Hostgruppen in der primären und in der sekundären VMM-Cloud befinden.<br/><br/> Wenn Sie Hyper-V in einem Cluster unter Windows Server 2012 R2 ausführen, installieren Sie das [Update 2961977](https://support.microsoft.com/kb/2961977).<br/><br/> Wenn Sie Hyper-V in einem Cluster unter Windows Server 2012 ausführen und über einen auf statischen IP-Adressen basierenden Cluster verfügen, wird der Clusterbroker nicht automatisch erstellt. Konfigurieren Sie den Clusterbroker manuell. [Weitere Informationen](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx)<br/><br/> Hyper-V-Server benötigen Zugriff auf das Internet.
**URLs** | Für VMM-Server und Hyper-V-Hosts müssen folgende URLs erreichbar sein:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]

## <a name="deployment-steps"></a>Bereitstellungsschritte

Gehen Sie wie folgt vor:

1. Vergewissern Sie sich, dass die Voraussetzungen erfüllt sind.
2. Bereiten Sie den VMM-Server und die Hyper-V-Hosts vor.
3. Erstellen Sie einen Recovery Services-Tresor. Der Tresor enthält Konfigurationseinstellungen und dient zum Orchestrieren der Replikation.
4. Geben Sie Quelle, Ziel und Replikationseinstellungen an.
5. Stellen Sie den Mobilitätsdienst auf virtuellen Computern bereit, die Sie replizieren möchten.
6. Bereiten Sie die Replikation vor, und aktivieren Sie sie für virtuelle Hyper-V-Computer.
7. Führen Sie ein Testfailover aus, um sicherzustellen, dass alles wie erwartet funktioniert.

## <a name="prepare-vmm-servers-and-hyper-v-hosts"></a>Vorbereiten von VMM-Servern und Hyper-V-Hosts

So bereiten Sie die Bereitstellung vor:

1. Vergewissern Sie sich, dass der VMM-Server und die Hyper-V-Hosts die oben beschriebenen Voraussetzungen erfüllen und die erforderlichen URLs erreichen können.
2. Richten Sie VMM-Netzwerke ein, damit Sie die [Netzwerkzuordnung](#network-mapping-overview) konfigurieren können.

    - Stellen Sie sicher, dass VMs auf dem Hyper-V-Quellhostserver mit einem VMM-VM-Netzwerk verbunden sind. Dieses Netzwerk sollte mit einem logischen Netzwerk verbunden sein, das der Cloud zugeordnet ist.
    Stellen Sie sicher, dass für die sekundäre Cloud, die Sie für die Wiederherstellung verwenden, ein entsprechendes VM-Netzwerk konfiguriert ist. Dieses VM-Netzwerk sollte mit einem logischen Netzwerk verbunden sein, das der sekundären Cloud zugeordnet ist.

3. Bereiten Sie eine [einzelne Serverbereitstellung](#single-vmm-server-deployment) vor, wenn Sie virtuelle Computer zwischen Clouds auf dem gleichen VMM-Server replizieren möchten.

## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors
1. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie auf **Neu** > **Verwaltung** > **Recovery Services**.
3. Geben Sie unter **Name**einen Anzeigenamen an, mit dem der Tresor identifiziert wird. Wenn Sie mehrere Abonnements haben, müssen Sie ein Abonnement auswählen.
4. [Erstellen Sie eine Ressourcengruppe](../azure-resource-manager/resource-group-template-deploy-portal.md), oder wählen Sie eine vorhandene Ressourcengruppe aus. Geben Sie eine Azure-Region an. Computer werden in dieser Region repliziert. Sie finden eine Liste der unterstützten Regionen unter Geografische Verfügbarkeit auf der Seite [Azure Site Recovery – Preisübersicht](https://azure.microsoft.com/pricing/details/site-recovery/)
5. Wenn Sie schnell über das Dashboard auf den Tresor zugreifen möchten, klicken Sie auf **An Dashboard anheften** > **Tresor erstellen**.

    ![Neuer Tresor](./media/site-recovery-vmm-to-vmm/new-vault-settings.png)

Der neue Tresor wird unter **Dashboard** > **Alle Ressourcen** und auf dem Hauptblatt **Recovery Services-Tresore** angezeigt.


## <a name="choose-a-protection-goal"></a>Wählen eines Schutzziels

Wählen Sie aus, was Sie replizieren möchten und wohin die Daten repliziert werden sollen.

2. Klicken Sie auf **Site Recovery** > **Schritt 1: Bereiten Sie die Infrastruktur vor** > **Schutzziel**.
3. Wählen Sie **Auf Wiederherstellungsstandort** und anschließend **Yes, with Hyper-V** (Ja, mit Hyper-V) aus.
4. Wählen Sie **Ja** aus, um anzugeben, dass Sie die Hyper-V-Hosts mithilfe von VMM verwalten.
5. Wählen Sie **Ja** aus, falls Sie über einen sekundären VMM-Server verfügen. Wenn Sie die Replikation zwischen Clouds auf einem einzelnen VMM-Server bereitstellen, klicken Sie auf **Nein**. Klicken Sie dann auf **OK**.

    ![Ziele wählen](./media/site-recovery-vmm-to-vmm/choose-goals.png)

## <a name="set-up-the-source-environment"></a>Einrichten der Quellumgebung

Installieren Sie den Azure Site Recovery-Anbieter auf den VMM-Servern, und ermitteln und registrieren Sie die Server im Tresor.

1. Klicken Sie auf **Schritt 1: Bereiten Sie die Infrastruktur vor** > **Quelle**.

    ![Quelle einrichten](./media/site-recovery-vmm-to-vmm/goals-source.png)
2. Klicken Sie unter **Quelle vorbereiten** auf **+ VMM**, um einen VMM-Server hinzuzufügen.

    ![Quelle einrichten](./media/site-recovery-vmm-to-vmm/set-source1.png)
3. Vergewissern Sie sich unter **Server hinzufügen**, dass unter **Servertyp** der Eintrag **System Center-VMM-Server** angezeigt wird und dass der VMM-Server die [Voraussetzungen](#prerequisites) erfüllt.
4. Laden Sie die Installationsdatei für den Azure Site Recovery-Anbieter herunter.
5. Laden Sie den Registrierungsschlüssel herunter. Sie benötigen diese Angaben beim Ausführen des Setups. Der Schlüssel ist nach der Erstellung fünf Tage lang gültig.

    ![Quelle einrichten](./media/site-recovery-vmm-to-vmm/set-source3.png)
6. Installieren Sie den Azure Site Recovery-Anbieter auf dem VMM-Server. Sie müssen nicht explizit etwas auf Hyper-V-Hostservern installieren.


### <a name="install-the-azure-site-recovery-provider"></a>Installieren des Azure Site Recovery-Anbieters

1. Führen Sie die Anbietersetupdatei auf jedem VMM-Server aus. Falls VMM in einem Cluster bereitgestellt wird, gehen Sie bei der erstmaligen Installation wie folgt vor:
    -  Installieren Sie den Anbieter auf einem aktiven Knoten, und schließen Sie die Installation ab, um den VMM-Server im Tresor zu registrieren.
    - Installieren Sie den Anbieter anschließend auf den anderen Knoten. Auf allen Clusterknoten sollte die gleiche Version des Anbieters ausgeführt werden.
2. Das Setup führt einige Voraussetzungsüberprüfungen durch und fordert die Berechtigung zum Beenden des VMM-Diensts an. Der VMM-Dienst wird nach Abschluss des Setups automatisch neu gestartet. Bei der Installation in einem VMM-Cluster werden Sie aufgefordert, die Clusterrolle zu beenden.
3. Unter **Microsoft Update** können Sie Updates aktivieren, damit Updates für den Anbieter gemäß Ihrer Microsoft Update-Richtlinie installiert werden.
4. Ändern Sie unter **Installation** den Speicherort für die Installation, oder akzeptieren Sie den Standardspeicherort, und klicken Sie auf **Installieren**.

    ![Installationsspeicherort](./media/site-recovery-vmm-to-vmm/provider-location.png)
5. Klicken Sie nach Abschluss der Installation auf **Registrieren**, um den Server im Tresor zu registrieren.

    ![Installationsspeicherort](./media/site-recovery-vmm-to-vmm/provider-register.png)
6. Prüfen Sie unter **Tresorname**den Namen des Tresors, in dem der Server registriert wird. Klicken Sie auf *Weiter*.

    ![Serverregistrierung](./media/site-recovery-vmm-to-vmm-classic/vaultcred.PNG)
7. Geben Sie unter **Internetverbindung** an, wie der auf dem VMM-Server ausgeführte Anbieter eine Verbindung mit Azure herstellen soll.

    ![Interneteinstellungen](./media/site-recovery-vmm-to-vmm-classic/proxydetails.PNG)

   - Der Anbieter kann entweder eine direkte oder eine proxybasierte Internetverbindung herstellen.
   - Geben Sie ggf. Proxyeinstellungen an.
   - Bei Verwendung eines Proxys wird automatisch ein ausführendes VMM-Konto (DRAProxyAccount) mit den angegebenen Proxyanmeldeinformationen erstellt. Konfigurieren Sie den Proxyserver so, dass dieses Konto erfolgreich authentifiziert werden kann. Die Einstellungen für ausführende Konten können über die VMM-Konsole unter **Einstellungen** > **Sicherheit** > **Ausführende Konten** geändert werden. Starten Sie den VMM-Dienst neu, um die Änderungen zu übernehmen.
8. Wählen Sie unter **Registrierungsschlüssel**den Schlüssel aus, den Sie von Azure Site Recovery heruntergeladen und auf den VMM-Server kopiert haben.
9. Die Verschlüsselungseinstellung wird nur verwendet, wenn Sie Hyper-V-VMs in VMM-Clouds in Azure replizieren. Bei der Replikation an einem sekundären Standort wird sie nicht verwendet.
10. Geben Sie unter **Servername**einen Anzeigenamen ein, um den VMM-Server im Tresor zu identifizieren. Geben Sie in einer Clusterkonfiguration den Rollennamen des VMM-Clusters an.
11. Wählen Sie unter **Cloudmetadaten synchronisieren** aus, ob Sie Metadaten für alle Clouds auf dem VMM-Server mit dem Tresor synchronisieren möchten. Diese Aktion muss für jeden VMM-Server nur einmal ausgeführt werden. Wenn Sie nicht alle Clouds synchronisieren möchten, können Sie diese Einstellung deaktiviert lassen und in den Cloudeigenschaften in der VMM-Konsole jede Cloud einzeln synchronisieren.
12. Klicken Sie auf **Weiter** , um den Prozess abzuschließen. Nach der Registrierung werden die Metadaten vom VMM-Server von Azure Site Recovery abgerufen. Der Server wird im Tresor auf der Registerkarte **VMM-Server** der Seite **Server** angezeigt.

    ![Server](./media/site-recovery-vmm-to-vmm-classic/provider13.PNG)
13. Sobald der Server in der Site Recovery-Konsole verfügbar ist, wählen Sie unter **Quelle** > **Quelle vorbereiten** den VMM-Server sowie die Cloud aus, in der sich der Hyper-V-Host befindet. Klicken Sie dann auf **OK**.

Der Anbieter kann auch über die Befehlszeile installiert werden:

[!INCLUDE [site-recovery-rw-provider-command-line](../../includes/site-recovery-rw-provider-command-line.md)]


## <a name="set-up-the-target-environment"></a>Einrichten der Zielumgebung

Wählen Sie VMM-Server und Cloud aus.

1. Klicken Sie auf **Infrastruktur vorbereiten** > **Ziel**, und wählen Sie den gewünschten VMM-Zielserver aus.
2. Clouds auf dem Server, die mit Site Recovery synchronisiert werden, werden angezeigt. Wählen Sie die Zielcloud aus.

   ![Ziel](./media/site-recovery-vmm-to-vmm/target-vmm.png)

## <a name="set-up-replication-settings"></a>Einrichten der Replikationseinstellungen

- Wenn Sie eine Replikationsrichtlinie erstellen, müssen alle Hosts, die die Richtlinie verwenden, über das gleiche Betriebssystem verfügen. Die VMM-Cloud kann zwar Hyper-V-Hosts mit unterschiedlichen Windows Server-Versionen enthalten, in diesem Fall benötigen Sie jedoch mehrere Replikationsrichtlinien.
- Die anfängliche Replikation kann offline durchgeführt werden. [Weitere Informationen](#prepare-for-initial-offline-replication)

1. Klicken Sie zum Erstellen einer neuen Replikationsrichtlinie auf **Infrastruktur vorbereiten** > **Replikationseinstellungen** > **+Erstellen und zuordnen**.

    ![Netzwerk](./media/site-recovery-vmm-to-vmm/gs-replication.png)
2. Geben Sie unter **Richtlinie erstellen und zuordnen**einen Richtliniennamen an. Quell- und Zieltyp müssen jeweils **Hyper-V**lauten.
3. Wählen Sie unter **Hyper-V-Hostversion** aus, welches Betriebssystem auf dem Host ausgeführt wird.
4. Geben Sie unter **Authentifizierungstyp** und **Authentifizierungsport** an, wie der Datenverkehr zwischen dem primären und dem Hyper-V-Wiederherstellungs-Hostserver authentifiziert wird. Wählen Sie **Zertifikat** aus, sofern Sie nicht über eine funktionsfähige Kerberos-Umgebung verfügen. Azure Site Recovery konfiguriert automatisch Zertifikate für die HTTPS-Authentifizierung. Sie müssen gar nichts manuell durchführen. Standardmäßig werden die Ports 8083 und 8084 (für Zertifikate) in der Windows-Firewall auf den Hyper-V-Hostservern geöffnet. Wenn Sie **Kerberos**auswählen, wird ein Kerberos-Ticket für die gegenseitige Authentifizierung der Hostserver verwendet. Bitte beachten Sie, dass diese Einstellung nur für Hyper-V-Hostserver relevant ist, die unter Windows Server 2012 R2 laufen.
5. Geben Sie unter **Kopierhäufigkeit**an, wie oft Sie Deltadaten nach der ersten Replikation replizieren möchten (alle 30 Sekunden, nach 5 Minuten oder nach 15 Minuten).
6. Geben Sie unter **Aufbewahrungszeitraum des Wiederherstellungspunkts**das Aufbewahrungszeitfenster für die einzelnen Wiederherstellungspunkte in Stunden an. Geschützte Computer können innerhalb eines Zeitfensters an einem beliebigen Punkt wiederhergestellt werden.
7. Geben Sie unter **App-konsistente Momentaufnahmehäufigkeit** an, wie häufig (1 bis 12 Stunden) Wiederherstellungspunkte erstellt werden sollen, die anwendungskonsistente Momentaufnahmen enthalten. Hyper-V verwendet zwei Momentaufnahmen: eine Standard-Momentaufnahme, die eine inkrementelle Momentaufnahme des gesamten virtuellen Computers bereitstellt, und eine anwendungskonsistente Momentaufnahme, die eine Zeitpunkt-Momentaufnahme der Anwendungsdaten innerhalb des virtuellen Computers erfasst. Anwendungskonsistente Momentaufnahmen verwenden den Volumeschattenkopie-Dienst (Volume Shadow Copy Service, VSS), um sicherzustellen, dass Anwendungen sich bei der Erstellung der Momentaufnahme in einem konsistenten Zustand befinden. Die Leistung von Anwendungen auf virtuellen Quellcomputern wird durch die Aktivierung anwendungskonsistenter Momentaufnahmen beeinträchtigt. Stellen Sie sicher, dass der festgelegte Wert kleiner als die konfigurierte Anzahl der zusätzlichen Wiederherstellungspunkte ist.
8. Geben Sie unter **Datenübertragungskomprimierung**an, ob replizierte Daten, die übertragen werden, komprimiert werden sollen.
9. Wählen Sie **Replikat-VM löschen** aus, um anzugeben, dass die Replikat-VM gelöscht werden sollte, wenn Sie den Schutz für die Quell-VM deaktivieren. Wenn Sie diese Einstellung aktivieren, und Sie deaktivieren den Schutz für die Quell-VMs, wird der virtuelle Computer aus der Site Recovery-Konsole entfernt, die Site Recovery-Einstellungen für die VMM werden aus der VMM-Konsole entfernt, und das Replikat wird gelöscht.
10. Wenn Sie über das Netzwerk replizieren, geben Sie unter **Methode für anfängliche Replikation** an, ob die erste Replikation sofort gestartet werden soll oder ob Sie sie planen möchten. Um Netzwerkbandbreite zu sparen, sollte der Zeitraum außerhalb Ihrer Spitzenzeiten liegen. Klicken Sie dann auf **OK**.

     ![Replikationsrichtlinie](./media/site-recovery-vmm-to-vmm/gs-replication2.png)
11. Wenn Sie eine neue Richtlinie erstellen, wird sie der VMM-Cloud automatisch zugeordnet. Klicken Sie unter **Replikationsrichtlinie** auf **OK**. Sie können dieser Replikationsrichtlinie unter **Replikation** > „Richtlinienname“ > **VMM-Cloud zuordnen** zusätzliche VMM-Clouds (und die darin enthaltenen VMs) zuordnen.

     ![Replikationsrichtlinie](./media/site-recovery-vmm-to-vmm/policy-associate.png)


### <a name="configure-network-mapping"></a>Konfigurieren der Netzwerkzuordnung

- Machen Sie sich zunächst mit der [Netzwerkzuordnung](#prepare-for-network-mapping) vertraut.
- Stellen Sie sicher, dass die virtuellen Computer auf den VMM-Servern mit einem VM-Netzwerk verbunden sind.


1. Klicken Sie unter **Site Recovery-Infrastruktur** > **Netzwerkzuordnung** > **Netzwerkzuordnungen** auf **+Netzwerkzuordnung**.

    ![Netzwerkzuordnung](./media/site-recovery-vmm-to-azure/network-mapping1.png)
2. Wählen Sie auf der Registerkarte **Netzwerkzuordnung hinzufügen** die Quell- und Ziel-VMM-Server aus. Die VM-Netzwerke, die den VMM-Servern zugeordnet sind, werden abgerufen.
3. Wählen Sie unter **Quellnetzwerk**in der Liste mit den VM-Netzwerken, die dem primären VMM-Server zugeordnet sind, das gewünschte Netzwerk aus.
4. Wählen Sie unter **Zielnetzwerk** das Netzwerk aus, das Sie auf dem sekundären VMM-Server verwenden möchten. Klicken Sie dann auf **OK**.

    ![Netzwerkzuordnung](./media/site-recovery-vmm-to-vmm/network-mapping2.png)

Wenn die Netzwerkzuordnung beginnt, passiert Folgendes:

* Vorhandene virtuelle Replikatcomputer, die dem Quell-VM-Netzwerk entsprechen, werden mit dem Ziel-VM-Netzwerk verbunden.
* Neue virtuelle Computer, die mit dem Quell-VM-Netzwerk verbunden sind, werden nach der Replikation mit dem zugeordneten Zielnetzwerk verbunden.
* Wenn Sie eine vorhandene Zuordnung mit einem neuen Netzwerk ändern, werden virtuelle Replikatcomputer anhand der neuen Einstellungen verbunden.
* Wenn das Zielnetzwerk mehrere Subnetze enthält und eines dieser Subnetze den gleichen Namen besitzt wie das Subnetz des virtuellen Quellcomputers, wird der virtuelle Replikatcomputer nach dem Failover mit diesem Zielsubnetz verbunden. Gibt es kein Zielsubnetz mit einem übereinstimmenden Namen, wird der virtuelle Computer mit dem ersten Subnetz im Netzwerk verbunden.

### <a name="configure-storage-mapping"></a>Konfigurieren der Speicherzuordnung

Die [Speicherzuordnung](#prepare-for-storage-mapping) wird im neuen Azure-Portal nicht unterstützt. Sie kann aber über Powershell aktiviert werden. [detaillierte Kapazitätsplanung](site-recovery-vmm-to-vmm-powershell-resource-manager.md#step-7-configure-storage-mapping)

## <a name="step-5-capacity-planning"></a>Schritt 5: Kapazitätsplanung

Nachdem Sie nun Ihre grundlegende Infrastruktur eingerichtet haben, können Sie sich mit der Kapazitätsplanung beschäftigen und ermitteln, ob Sie zusätzliche Ressourcen benötigen.

- Laden Sie den [Azure Site Recovery Capacity Planner](site-recovery-capacity-planner.md) herunter, und führen Sie ihn aus, um Informationen zu Ihrer Replikationsumgebung (etwa zu virtuellen Computern, zu den Datenträgern pro virtuellem Computer und zum Speicher pro Datenträger) zu sammeln.
- Nach dem Sammeln von Echtzeit-Replikationsinformationen können Sie die NetQos-Richtlinie anpassen, um die Replikationsbandbreite für virtuelle Computer zu steuern. Weitere Informationen zur Drosselung von Hyper-V-Replikat-Datenverkehr finden Sie im [Blog von Thomas Maurer](http://www.thomasmaurer.ch/2013/12/throttling-hyper-v-replica-traffic/). Weitere Informationen zum Cmdlet „New-NetQosPolicy“ finden Sie [hier](https://technet.microsoft.com/library/hh967468.aspx.).

## <a name="enable-replication"></a>Replikation aktivieren

1. Klicken Sie auf **Schritt 2: Replizieren Sie die Anwendung** > **Quelle**. Klicken Sie nach der erstmaligen Aktivierung der Replikation im Tresor auf **+Replizieren** , um die Replikation für weitere Computer zu aktivieren.

    ![Replikation aktivieren](./media/site-recovery-vmm-to-vmm/enable-replication1.png)
2. Wählen Sie auf dem Blatt **Quelle** den VMM-Server und die Cloud aus, in denen sich die Hyper-V-Hosts befinden, die Sie replizieren möchten. Klicken Sie dann auf **OK**.

    ![Replikation aktivieren](./media/site-recovery-vmm-to-vmm/enable-replication2.png)
3. Überprüfen Sie in **Ziel** den sekundären VMM-Server und die Cloud.
4. Wählen Sie unter **Virtuelle Computer** in der Liste die VMs aus, die Sie schützen möchten.

    ![Schutz für virtuelle Computer aktivieren](./media/site-recovery-vmm-to-vmm/enable-replication5.png)

Sie können den Fortschritt der Aktion **Schutz aktivieren** unter **Aufträge** > **Site Recovery-Aufträge** verfolgen. Nach Ausführung des Auftrags **Schutz abschließen** ist der virtuelle Computer bereit zum Failover.

Beachten Sie Folgendes:

- Sie können auch Schutz für virtuelle Maschinen in der VMM-Konsole aktivieren. Klicken Sie in den Eigenschaften des virtuellen Computers auf der Symbolleiste auf **Schutz aktivieren** und anschließend auf die Registerkarte **Azure Site Recovery**.
- Nachdem Sie die Replikation aktiviert haben, können Sie unter **Replizierte Elemente** die Eigenschaften für die VM anzeigen. Das Dashboard **Zusammenfassung** enthält Informationen zur Replikationsrichtlinie der VM und ihrem Status. Klicken Sie auf **Eigenschaften** , um weitere Details anzuzeigen.

### <a name="onboard-existing-virtual-machines"></a>Integrieren vorhandener virtueller Computer
Wenn in VMM virtuelle Computer vorhanden sind, die über Hyper-V-Replikate repliziert werden, können Sie diese für die Replikation durch Azure Site Recovery wie folgt integrieren:

1. Stellen Sie sicher, dass sich der Hyper-V-Hostserver der vorhandenen VM in der primären Cloud und der Hyper-V-Hostserver der Replikat-VM in der sekundären Cloud befindet.
2. Stellen Sie sicher, dass eine Replikationsrichtlinie für die primäre VMM-Cloud konfiguriert ist.
3. Aktivieren Sie die Replikation für den primären virtuellen Computer. Azure Site Recovery und VMM stellen sicher, dass der gleiche Replikathost und virtuelle Computer erkannt werden. Azure Site Recovery verwendet die Replikation wieder und stellt sie anhand der festgelegten Einstellungen erneut her.

## <a name="test-your-deployment"></a>Testen der Bereitstellung

Zum Testen Ihrer Bereitstellung können Sie ein [Testfailover](site-recovery-test-failover-vmm-to-vmm.md) für einen einzelnen virtuellen Computer durchführen oder [einen Wiederherstellungsplan erstellen](site-recovery-create-recovery-plans.md), der mindestens einen virtuellen Computer enthält.



## <a name="prepare-for-offline-initial-replication"></a>Vorbereiten der ersten Offlinereplikation

Sie können eine Offlinereplikation für die erstmalige Datenkopie durchführen. Sie können dies wie folgt vorbereiten:

* Geben Sie auf dem Quellserver einen Pfad an, von dem der Datenexport stattfinden soll. Weisen Sie Vollzugriff für NTFS und Freigabeberechtigungen für den VMM-Dienst auf dem Exportpfad zu. Auf dem Zielserver geben Sie einen Pfad an, von dem der Datenimport stattfinden soll. Weisen Sie für diesen Importpfad die gleichen Berechtigungen zu.
* Wenn der Import- oder Exportpfad freigegeben ist, weisen Sie Mitgliedschaften in den Gruppen "Administratoren", "Hauptbenutzer", "Druck-Operatoren" oder "Server-Operatoren" für das VMM-Dienstkonto auf dem Remotecomputer zu, auf dem sich die Freigabe befindet.
* Wenn Sie zum Hinzufügen von Hosts ausführende Konten verwenden, weisen Sie auf dem Import- und Exportpfad Lese- und Schreibberechtigungen für die ausführenden Konten in VMM zu.
* Die Import- und Exportfreigaben sollten sich nicht auf Computern befinden, die als Hyper-V-Hostserver eingesetzt werden, da die Loopbackkonfiguration von Hyper-V nicht unterstützt wird.
* Aktivieren und konfigurieren Sie die eingeschränkte Delegierung in Active Directory auf jedem Hyper-V-Hostserver mit virtuellen Computern, die Sie schützen möchten, um den Remotecomputern zu vertrauen, auf denen sich der Import- und der Exportpfad befinden. Gehen Sie dabei wie folgt vor:
  1. Öffnen Sie auf dem Domänencontroller **Active Directory-Benutzer und-Computer**.
  2. Klicken Sie in der Konsolenstruktur auf **Domänenname** > **Computer**.
  3. Klicken Sie mit der rechten Maustaste auf den Namen des Hyper-V-Hostservers, und klicken Sie anschließend auf **Eigenschaften**.
  4. Klicken Sie auf der Registerkarte **Delegierung** auf **Computer nur bei Delegierungen angegebener Dienste vertrauen**.
  5. Klicken Sie auf **Beliebiges Authentifizierungsprotokoll verwenden**.
  6. Klicken Sie auf **Hinzufügen** > **Benutzer und Computer**können Sie technische Fragen stellen.
  7. Geben Sie den Namen des Computers an, der den Importpfad hostet, und klicken Sie auf **OK**. Halten Sie in der Liste der verfügbaren Dienste die STRG-Taste gedrückt, und klicken Sie auf **CIFS** > **OK**. Wiederholen Sie diesen Vorgang für den Namen des Computers, der den Importpfad hostet. Wiederholen Sie den Vorgang bei Bedarf für zusätzliche Hyper-V-Hostserver.



## <a name="prepare-for-network-mapping"></a>Bereiten Sie die Netzwerkzuordnung vor
Die Netzwerkzuordnung zwischen VMM-VM-Netzwerken auf dem primären und sekundären VMM-Server dient folgenden Zwecken:

* Optimales Platzieren von Replikat-VMs auf sekundären Hyper-V-Hosts nach dem Failover
* Verbinden von Replikat-VMs mit geeigneten VM-Netzwerken nach dem Failover

Beachten Sie Folgendes:
- Die Netzwerkzuordnung kann zwischen VM-Netzwerken auf zwei VMM-Servern konfiguriert werden (oder auf einem einzelnen VMM-Server, wenn zwei Standorte vom gleichen Server verwaltet werden).
- Bei korrekt konfigurierter Zuordnung und aktivierter Replikation ist ein virtueller Computer am primären Standort mit einem Netzwerk verbunden, und sein Replikat am Zielort wird mit dem zugeordneten Netzwerk verbunden.
- Wenn die Netzwerke in VMM ordnungsgemäß eingerichtet wurden, gilt Folgendes: Wenn Sie bei der Netzwerkzuordnung ein VM-Zielnetzwerk auswählen, werden die VMM-Quellclouds, die das VM-Quellnetzwerk verwenden, sowie die verfügbaren VM-Zielnetzwerke in den Zielclouds angezeigt, die für den Schutz verwendet werden.
- Wenn das Zielnetzwerk mehrere Subnetze enthält und eines dieser Subnetze den gleichen Namen besitzt wie das Subnetz des virtuellen Quellcomputers, wird der virtuelle Replikatcomputer nach dem Failover mit diesem Zielsubnetz verbunden. Gibt es kein Zielsubnetz mit einem übereinstimmenden Namen, wird der virtuelle Computer mit dem ersten Subnetz im Netzwerk verbunden.


Im Anschluss folgt ein Beispiel zur Veranschaulichung dieses Mechanismus. Für das Beispiel verwenden wir eine Organisation mit zwei Standorten in New York und Chicago.

| **Standort** | **VMM-Server** | **VM-Netzwerke** | **Zuordnung** |
| --- | --- | --- | --- |
| New York |VMM-NewYork |VMNetwork1-NewYork |Zugeordnet zu „VMNetwork1-Chicago“ |
| VMNetwork2-NewYork |Nicht zugeordnet | | |
| Chicago |VMM-Chicago |VMNetwork1-Chicago |Zugeordnet zu „VMNetwork1-NewYork“ |
| VMNetwork2-Chicago |Nicht zugeordnet | | |

In diesem Beispiel gilt Folgendes:

* Wenn für einen virtuellen, mit „VMNetwork1-NewYork“ verbundenen Computer ein virtueller Replikatcomputer erstellt wird, wird dieser mit „VMNetwork1-Chicago“ verbunden.
* Ein für „VMNetwork2-NewYork“ oder „VMNetwork2-Chicago“ erstellter virtueller Replikatcomputer wird mit keinem Netzwerk verbunden.

Im Anschluss finden Sie Informationen zur Einrichtung der VMM-Clouds für unser Beispielunternehmen sowie zu den logischen Netzwerken, die den Clouds zugeordnet sind.

### <a name="cloud-protection-settings"></a>Cloudschutzeinstellungen
| **Geschützte Cloud** | **Schützende Cloud** | **Logisches Netzwerk (New York)** |
| --- | --- | --- |
| GoldCloud1 |GoldCloud2 | |
| SilverCloud1 |SilverCloud2 | |
| GoldCloud2 |<p>NA</p><p></p> |<p>LogicalNetwork1-NewYork</p><p>LogicalNetwork1-Chicago</p> |
| SilverCloud2 |<p>NA</p><p></p> |<p>LogicalNetwork1-NewYork</p><p>LogicalNetwork1-Chicago</p> |

### <a name="logical-and-vm-network-settings"></a>Einstellungen für das logische und das VM-basierte Netzwerk
| **Standort** | **Logisches Netzwerk** | **Zugeordnetes VM-Netzwerk** |
| --- | --- | --- |
| New York |LogicalNetwork1-NewYork |VMNetwork1-NewYork |
| Chicago |LogicalNetwork1-Chicago |VMNetwork1-Chicago |
| LogicalNetwork2-Chicago |VMNetwork2-Chicago | |

### <a name="target-networks"></a>Zielnetzwerke
Die folgende Tabelle zeigt die Optionen, die zur Verfügung stehen, wenn Sie bei Verwendung dieser Einstellungen das VM-Zielnetzwerk auswählen:

| **Auswahl** | **Geschützte Cloud** | **Schützende Cloud** | **Verfügbarkeit des Zielnetzwerks** |
| --- | --- | --- | --- |
| VMNetwork1-Chicago |SilverCloud1 |SilverCloud2 |Verfügbar |
| GoldCloud1 |GoldCloud2 |Verfügbar | |
| VMNetwork2-Chicago |SilverCloud1 |SilverCloud2 |Nicht verfügbar |
| GoldCloud1 |GoldCloud2 |Verfügbar | |


### <a name="failback"></a>Failback
Zur Veranschaulichung der Vorgänge bei einem Failback (umgekehrte Replikation) nehmen wir an, dass zwischen „VMNetwork1-NewYork“ und „VMNetwork1-Chicago“ eine Zuordnung mit den folgenden Einstellungen besteht:

| **Virtueller Computer** | **Verbundenes VM-Netzwerk** |
| --- | --- |
| VM1 |VMNetwork1-NewYork |
| VM2 (Replikat von VM1) |VMNetwork1-Chicago |

Einige mögliche Szenarien mit diesen Einstellungen:

| **Szenario** | **Ergebnis** |
| --- | --- |
| Keine Änderung der Netzwerkeigenschaften von VM-2 nach dem Failover |VM-1 bleibt mit dem Quellnetzwerk verbunden. |
| Änderung der Netzwerkeigenschaften von VM-2 nach dem Failover und Trennung der Verbindung |VM-1 wird getrennt. |
| Änderung der Netzwerkeigenschaften von VM-2 nach dem Failover und Verbindung mit "VMNetwork2-Chicago" |Wenn "VMNetwork2-Chicago" nicht zugeordnet ist, wird VM-1 getrennt. |
| Änderung der Netzwerkzuordnung von "VMNetwork1-Chicago" |VM-1 wird mit dem Netzwerk verbunden, das jetzt "VMNetwork1-Chicago" zugeordnet ist. |


## <a name="prepare-for-single-server-deployment"></a>Vorbereiten einer einzelnen Serverbereitstellung


Wenn Sie nur über einen einzelnen VMM-Server verfügen, können Sie VMs in Hyper-V-Hosts in der VMM-Cloud zu [Azure](site-recovery-vmm-to-azure.md) oder in eine sekundäre VMM-Cloud replizieren. Wir empfehlen die erste Option, da die Replikation zwischen Clouds nicht übergangslos erfolgt. Wenn Sie eine Replikation zwischen Clouds durchführen möchten, können Sie entweder einen einzelnen eigenständigen VMM-Server oder einen einzelnen VMM-Server verwenden, der in einem gestreckten Windows-Cluster bereitgestellt ist.

### <a name="standalone-vmm-server"></a>Eigenständiger VMM-Server

In diesem Szenario stellen Sie den einzelnen VMM-Server als virtuellen Computer an einem primären Standort bereit und replizieren diese VM mithilfe von Site Recovery und Hyper-V-Replikat zu einem sekundären Standort.

1. **Richten Sie VMM auf einem virtuellen Hyper-V-Computer ein**. Sie sollten die von VMM verwendete SQL Server-Instanz auf der gleichen VM installieren. Dies spart Zeit, da nur ein virtueller Computer erstellt werden muss. Wenn Sie eine Remoteinstanz von SQL Server verwenden möchten und ein Ausfall auftritt, müssen Sie diese Instanz vor dem Wiederherstellen von VMM wiederherstellen.
2. **Stellen Sie sicher, dass für den VMM-Server mindestens zwei Clouds konfiguriert sind**. Eine Cloud enthält die virtuellen Computer, die repliziert werden sollen, und die andere Cloud dient als sekundärer Standort. Die Cloud mit den virtuellen Computern, die Sie schützen möchten, muss bestimmte [Voraussetzungen](#prerequisites)erfüllen.
3. Richten Sie Site Recovery wie in diesem Artikel beschrieben ein. Erstellen und registrieren Sie den VMM-Server in einem Tresor, richten Sie eine Replikationsrichtlinie ein, und aktivieren Sie die Replikation. Die Quell- und Ziel-VMM-Namen sind identisch. Geben Sie an, dass die anfängliche Replikation über das Netzwerk erfolgt.
4. Beim Einrichten der Netzwerkzuordnung ordnen Sie das VM-Netzwerk für die primäre Cloud dem VM-Netzwerk für die sekundäre Cloud zu.
5. Aktivieren Sie in der Hyper-V-Manager-Konsole Hyper-V-Replikat auf dem Hyper-V-Host, der die VMM-VM enthält, und aktivieren Sie die Replikation auf dem virtuellen Computer. Fügen Sie die VMM-VM nicht zu Clouds hinzu, die von Site Recovery geschützt werden, um sicherzustellen, dass Einstellungen von Hyper-V Replikat nicht durch Site Recovery außer Kraft gesetzt werden.
6. Wenn Sie Wiederherstellungspläne für das Failover erstellen, verwenden Sie den gleichen VMM-Server als Quelle und Ziel.
7. Bei einem vollständigen Ausfall führen Sie Failover und Wiederherstellung wie folgt aus:

   1. Führen Sie in der Hyper-V-Manager-Konsole am sekundären Standort ein ungeplantes Failover durch, um ein Failover des primären virtuellen VMM-Computers auf den sekundären Standort vorzunehmen.
   2. Vergewissern Sie sich, dass der virtuelle VMM-Computer ausgeführt wird, und führen Sie im Tresor ein ungeplantes Failover durch, um für die virtuellen Computer ein Failover von primären auf sekundäre Clouds vorzunehmen. Committen Sie das Failover, und wählen Sie bei Bedarf einen alternativen Wiederherstellungspunkt aus.
   3. Nach Abschluss des nicht geplanten Failovers können die Benutzer wieder vom primären Standort aus auf sämtliche Ressourcen zugreifen.
   4. Wenn der primäre Standort wieder verfügbar ist, aktivieren Sie in der Hyper-V-Manager-Konsole am sekundären Standort die umgekehrte Replikation für den virtuellen VMM-Computer. Dadurch wird für den virtuellen Computer die Replikation vom sekundären an den primären Standort gestartet.
   5. Führen Sie in der Hyper-V-Manager-Konsole am sekundären Standort ein geplantes Failover durch, um ein Failover des virtuellen VMM-Computers auf den primären Standort vorzunehmen. Committen Sie das Failover. Aktivieren Sie anschließend die umgekehrte Replikation, damit der virtuelle VMM-Computer wieder vom primären an den sekundären Standort repliziert wird.
   6. Aktivieren Sie im Recovery Services-Tresor die umgekehrte Replikation für die Workload-VMs, um mit deren Replikation vom sekundären zum primären Standort zu beginnen.
   7. Führen Sie im Recovery Services-Tresor ein geplantes Failover aus, um das Failback der Workload-VMs zum primären Standort vorzunehmen. Führen Sie ein Commit für das Failover aus, um es abzuschließen. Aktivieren Sie anschließend die umgekehrte Replikation, um mit der Replikation von Workload-VMs vom primären zum sekundären Standort zu beginnen.

### <a name="stretched-vmm-cluster"></a>Gestreckte VMM-Cluster

Anstatt einen eigenständigen VMM-Server als virtuellen Computer bereitzustellen, der zu einem sekundären Standort repliziert wird, können Sie VMM als virtuellen Computer in einem Windows-Failovercluster bereitstellen und so hohe Verfügbarkeit erreichen. Dies bietet Workloadstabilität und Schutz vor Hardwarefehlern. Zum Bereitstellen mit Site Recovery sollte die VMM-VM in einem gestreckten Cluster über geografisch getrennte Standorte bereitgestellt werden. Gehen Sie dazu folgendermaßen vor:

1. Installieren Sie VMM auf einem virtuellen Computer in einem Windows-Failovercluster, und wählen Sie während der Installation die Option aus, den Server mit hoher Verfügbarkeit auszuführen.
2. Die von VMM verwendete SQL Server-Instanz sollte mithilfe von SQL Server Always On-Verfügbarkeitsgruppen repliziert werden, damit ein Replikat der Datenbank am sekundären Standort verfügbar ist.
3. Führen Sie die Schritte in diesem Artikel aus, um einen Tresor zu erstellen, den Server zu registrieren und den Schutz einzurichten. Sie müssen jeden VMM-Server im Cluster im Recovery Services-Tresor registrieren. Installieren Sie dazu den Anbieter auf einem aktiven Knoten, und registrieren Sie den VMM-Server. Dann installieren Sie den Anbieter auf den anderen Knoten.
4. Bei einem Ausfall wird für den VMM-Server und die entsprechende SQL Server-Datenbank ein Failover durchgeführt, und der Zugriff erfolgt vom sekundären Standort aus.



## <a name="prepare-for-storage-mapping"></a>Vorbereiten der Speicherzuordnung


Sie richten die Speicherzuordnung ein, indem Sie auf einem Quell- und Ziel-VMM-Server Speicherklassifizierungen für folgende Zwecke zuordnen:

  * **Angabe des Zielspeichers für virtuelle Replikatcomputer**: Eine Festplatte eines virtuellen Quellcomputers wird im angegebenen Speicher (SMB-Freigabe oder freigegebene Clustervolumes (Cluster Shared Volumes, CSVs)) am Zielspeicherort repliziert.
  * **Platzierung virtueller Replikatcomputer**: Virtuelle Replikatcomputer werden durch die Speicherzuordnung optimal auf Hyper-V-Hostservern platziert. Virtuelle Replikatcomputer werden auf Hosts platziert, die auf die zugeordnete Speicherklassifizierung zugreifen können.
  * **Keine Speicherzuordnung**: Wenn Sie keine Speicherzuordnung konfigurieren, werden virtuelle Computer zum Standardspeicherort auf dem Hyper-V-Hostserver repliziert, der dem virtuellen Replikatcomputer zugeordnet ist.

Beachten Sie Folgendes:
- Sie können die Zuordnung zwischen zwei VMM-Clouds auf einem einzelnen Server einrichten.
- Speicherklassifizierungen müssen den Hostgruppen in der Quell- und Zielcloud zur Verfügung stehen.
- Klassifizierungen müssen nicht den gleichen Speichertyp aufweisen. So können Sie beispielsweise eine Quellklassifizierung mit SMB-Freigaben einer Zielklassifizierung mit CSVs zuordnen.

### <a name="example"></a>Beispiel
Sind die Klassifizierungen in VMM ordnungsgemäß konfiguriert, werden die Quell- und Zielklassifizierungen angezeigt, wenn Sie bei der Speicherzuordnung den VMM-Quell- und -Zielserver auswählen. Im Anschluss folgt ein Beispiel für die Speicherdateifreigaben und -klassifizierungen für eine Organisation mit zwei Standorten in New York und Chicago:

| **Standort** | **VMM-Server** | **Dateifreigabe (Quelle)** | **Klassifizierung (Quelle)** | **Zuordnungsziel** | **Dateifreigabe (Ziel)** |
| --- | --- | --- | --- | --- | --- |
| New York |VMM_Source |SourceShare1 |GOLD |GOLD_TARGET |TargetShare1 |
| SourceShare2 |SILVER |SILVER_TARGET |TargetShare2 | | |
| SourceShare3 |BRONZE |BRONZE_TARGET |TargetShare3 | | |
| Chicago |VMM_Target | |GOLD_TARGET |Nicht zugeordnet | |
|  |SILVER_TARGET |Nicht zugeordnet | | | |
|  |BRONZE_TARGET |Nicht zugeordnet | | | |

In diesem Beispiel gilt Folgendes:

* Wenn für einen virtuellen Computer im GOLD-Speicher (SourceShare1) ein virtueller Replikatcomputer erstellt wird, wird dieser in einem GOLD_TARGET-Speicher (TargetShare1) repliziert.
* Wenn für einen virtuellen Computer im SILVER-Speicher (SourceShare2) ein virtueller Replikatcomputer erstellt wird, wird er zu einem SILVER_TARGET-Speicher (TargetShare2) repliziert, usw.

### <a name="multiple-storage-locations"></a>Mehrere Speicherorte
Wenn die Zielklassifizierung mehreren SMB-Freigaben oder freigegebenen Clustervolumes zugewiesen und der virtuelle Computer geschützt ist, wird der optimale Speicherort automatisch ausgewählt. Ist kein passender Zielspeicher mit der angegebenen Klassifizierung verfügbar, wird der Standardspeicherort auf dem Hyper-V-Host für die Platzierung der virtuellen Replikatfestplatten verwendet.

Die folgende Tabelle zeigt die Einrichtung der Speicherklassifizierung und der freigegebenen Clustervolumes in unserem Beispiel:

| **Standort** | **Klassifizierung** | **Zugeordneter Speicher** |
| --- | --- | --- |
| New York |GOLD |<p>C:\ClusterStorage\SourceVolume1</p><p>\\FileServer\SourceShare1</p> |
| SILVER |<p>C:\ClusterStorage\SourceVolume2</p><p>\\FileServer\SourceShare2</p> | |
| Chicago |GOLD_TARGET |<p>C:\ClusterStorage\TargetVolume1</p><p>\\FileServer\TargetShare1</p> |
| SILVER_TARGET |<p>C:\ClusterStorage\TargetVolume2</p><p>\\FileServer\TargetShare2</p> | |

Die folgende Tabelle zeigt, was passiert, wenn Sie den Schutz für virtuelle Computer (VM1 - VM5) in dieser Beispielumgebung aktivieren:

| **Virtueller Computer** | **Quellspeicher** | **Quellklassifizierung** | **Zugeordneter Zielspeicher** |
| --- | --- | --- | --- |
| VM1 |C:\ClusterStorage\SourceVolume1 |GOLD |<p>C:\ClusterStorage\SourceVolume1</p><p>\\\FileServer\SourceShare1</p><p>Beides GOLD_TARGET</p> |
| VM2 |\\FileServer\SourceShare1 |GOLD |<p>C:\ClusterStorage\SourceVolume1</p><p>\\FileServer\SourceShare1</p> <p>Beides GOLD_TARGET</p> |
| VM3 |C:\ClusterStorage\SourceVolume2 |SILVER |<p>C:\ClusterStorage\SourceVolume2</p><p>\FileServer\SourceShare2</p> |
| VM4 |\FileServer\SourceShare2 |SILVER |<p>C:\ClusterStorage\SourceVolume2</p><p>\\FileServer\SourceShare2</p><p>Beides SILVER_TARGET</p> |
| VM5 |C:\ClusterStorage\SourceVolume3 |N/V |Keine Zuordnung, daher Verwendung des Standardspeicherorts des Hyper-V-Hosts |



### <a name="data-privacy-overview"></a>Übersicht zum Datenschutz

In dieser Tabelle ist zusammengefasst, wie Daten in diesem Szenario gespeichert werden:

- - -
| Aktion | **Details** | **Gesammelte Daten** | **Verwenden Sie** | **Erforderlich** |
| --- | --- | --- | --- | --- |
| **Registrierung** | Sie erstellen und registrieren einen VMM-Server in einem Recovery Services-Tresor. Wenn Sie die Registrierung eines Servers später aufheben möchten, können Sie dafür die Serverinformationen aus dem Azure-Portal löschen. | Nach der Registrierung eines VMM-Servers erfasst Site Recovery Metadaten über den VMM-Server und die Namen der von Site Recovery erkannten VMM-Clouds, verarbeitet und überträgt sie. | Die Daten dienen zum Identifizieren und zum Kommunizieren mit dem entsprechenden VMM-Server sowie zum Konfigurieren der Einstellungen für die entsprechenden VMM-Clouds. | Dieses Feature ist erforderlich. Wenn Sie diese Informationen nicht an Site Recovery senden möchten, sollten Sie den Site Recovery-Dienst nicht verwenden. |
| **Replikation aktivieren** | Der auf dem VMM-Server installierte Azure Site Recovery-Anbieter ist für die Kommunikation mit dem Site Recovery-Dienst zuständig. Der Anbieter ist eine Dynamic Link Library (DLL), die im VMM-Prozess gehostet wird. Nachdem der Anbieter installiert ist, wird die Funktion „Datacenter Recovery“ in der VMM-Administratorkonsole aktiviert. Neue und vorhandene virtuelle Computer können dieses Feature aktivieren, um den Schutz für einen virtuellen Computer zu aktivieren. |Wenn diese Eigenschaft festgelegt ist, sendet der Anbieter den Namen und die ID des virtuellen Computers an Site Recovery.  Die Replikation wird vom Windows Server 2012- oder Windows Server 2012 R2-Hyper-V-Replikat aktiviert. Die Daten des virtuellen Computers werden von einem Hyper-V-Host auf einen anderen repliziert (der sich in der Regel in einem anderen "Recovery"-Rechenzentrum befindet). |Site Recovery verwendet die Metadaten zum Auffüllen der VM-Informationen im Azure-Portal. | Dieses Feature ist ein wesentlicher Bestandteil des Diensts und kann nicht deaktiviert werden. Wenn Sie diese Informationen nicht senden möchten, aktivieren Sie den Site Recovery-Schutz für virtuelle Computer nicht. Beachten Sie, dass alle Daten über HTTPS vom Anbieter an Site Recovery gesendet werden. |
| **Plan für die Wiederherstellung** | Pläne für die Wiederherstellung unterstützen Sie beim Erstellen eines Orchestrierungsplans für das Wiederherstellungs-Rechenzentrum. Sie können die Reihenfolge definieren, in der VMs oder eine Gruppe virtueller Computer am Wiederherstellungsstandort gestartet werden sollen. Außerdem können Sie die Ausführung beliebiger automatisierter Skripts oder manueller Aktionen angeben, die zum Zeitpunkt der Wiederherstellung für jede VM ausgeführt werden. Ein Failover wird in der Regel auf der Ebene des Wiederherstellungsplans ausgelöst, um eine koordinierte Wiederherstellung einzuleiten. | Site Recovery sammelt, verarbeitet und überträgt Metadaten für den Wiederherstellungsplan, einschließlich der Metadaten für virtuelle Computer sowie der Metadaten von Automatisierungsskripts und Hinweisen für manuelle Aktionen. |Die Metadaten werden verwendet, um den Wiederherstellungsplan im Azure-Portal zu erstellen. |Dieses Feature ist ein wesentlicher Bestandteil des Diensts und kann nicht deaktiviert werden. Wenn Sie diese Informationen nicht an Site Recovery senden möchten, erstellen Sie keine Wiederherstellungspläne. |
| **Netzwerkzuordnung** | Netzwerkinformationen aus dem primären Rechenzentrum werden dem Rechenzentrum für die Wiederherstellung zugeordnet. Wenn VMs am Wiederherstellungsstandort wiederhergestellt werden, hilft die Netzwerkzuordnung beim Herstellen der Netzwerkkonnektivität. |Site Recovery erfasst, verarbeitet und überträgt die Metadaten der logischen Netzwerke für die einzelnen Standorte (primär und Rechenzentrum). |Die Metadaten werden verwendet, um Netzwerkeinstellungen aufzufüllen, damit Sie die Netzwerkinformationen zuordnen können. | Dieses Feature ist ein wesentlicher Bestandteil des Diensts und kann nicht deaktiviert werden. Wenn Sie diese Informationen nicht an Site Recovery senden möchten, verwenden Sie die Netzwerkzuordnung nicht. |
| **Failover (geplant/ungeplant/Test)** | Bei einem Failover werden VMs in einem VMM-verwalteten Rechenzentrum offline genommen und dafür in einem anderen online geschaltet. Die Failoveraktion wird im Azure-Portal manuell ausgelöst. |Der Anbieter auf dem VMM-Server wird von Site Recovery über das Failoverereignis benachrichtigt und führt über VMM-Schnittstellen eine Failoveraktion auf dem Hyper-V-Host aus. Das tatsächliche Failover einer VM wird von einem Hyper-V-Host zu einem anderen ausgeführt und von Windows Server 2012 oder Windows Server 2012 R2 Hyper-V-Replikat abgewickelt. Site Recovery füllt anhand der gesendeten Informationen den Status der Informationen zur Failoveraktion im Azure-Portal auf. | Dieses Feature ist ein wesentlicher Bestandteil des Diensts und kann nicht deaktiviert werden. Wenn Sie diese Informationen nicht an Site Recovery senden möchten, verwenden Sie das Failover nicht. |

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie die Bereitstellung getestet haben, können Sie sich [hier](site-recovery-failover.md) ausführlicher über andere Failoverarten informieren.

