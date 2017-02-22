---
title: Failover in Site Recovery | Microsoft Docs
description: "Azure Site Recovery koordiniert Replikation, Failover und Wiederherstellung virtueller Computer und physischer Server. Informieren Sie sich über das Failover zu Azure oder zu einem sekundären Rechenzentrum."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 1/04/2017
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: b70d2d709d0940964c4220b798207adaf3551d36
ms.openlocfilehash: d31e987e636b178b5aa05722ff08707a6e53c3cf


---
# <a name="failover-in-site-recovery"></a>Failover in Site Recovery
Der Dienst Azure Site Recovery unterstützt Ihre Strategie für Geschäftskontinuität und Notfallwiederherstellung, indem Replikation, Failover und Wiederherstellung virtueller Computer und physischer Server aufeinander abgestimmt werden. Computer können in Azure oder in einem sekundären lokalen Rechenzentrum repliziert werden. Eine kurze Übersicht über das Gesamtthema finden Sie unter [Was ist Azure Site Recovery?](site-recovery-overview.md)

## <a name="overview"></a>Übersicht
Dieser Artikel enthält Informationen und Anweisungen zur Wiederherstellung (Failover und Failback) virtueller Computer und physischer Server, die mit Site Recovery geschützt sind.

Kommentare oder Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)veröffentlichen.

## <a name="types-of-failover"></a>Failover-Arten
Nachdem der Schutz für virtuelle Computer und physische Server eingerichtet wurde und diese repliziert werden, können Sie bei Bedarf Failover durchführen. Site Recovery unterstützt eine Reihe von Failover-Arten:

| **Failover** | **Zweck** | **Details** | **Prozess** |
| --- | --- | --- | --- |
| **Test-Failover** |Überprüfung der Replikationsstrategie oder Durchführung einer Notfallwiederherstellungsübung |Kein Datenverlust und keine Ausfallzeiten<br/><br/>Keine Auswirkungen auf die Replikation<br/><br/>Keine Auswirkungen auf die Produktionsumgebung |Starten Sie das Failover.<br/><br/>Geben Sie an, wie Testcomputer nach einem Failover Netzwerkverbindungen herstellen sollen.<br/><br/>Überwachen Sie den Fortschritt auf der Registerkarte **Aufträge** . Testcomputer werden erstellt und am sekundären Standort gestartet.<br/><br/>Azure: Stellen Sie über das Azure-Portal eine Verbindung mit dem Computer her.<br/><br/>Sekundärer Standort: Greifen Sie auf den Computer über den gleichen Host und die gleiche Cloud zu.<br/><br/>Schließen Sie den Test ab, und führen Sie eine automatische Bereinigung der Einstellungen für das Test-Failover durch. |
| **Geplantes Failover** |Erfüllung von Compliance-Anforderungen<br/><br/>Geplante Wartung<br/><br/>Datenfailover, um die Ausführung von Workloads bei bekannten Ausfällen weiter zu gewährleisten (beispielsweise im Falle eines erwartenden Stromausfalls oder bei Unwetterwarnungen)<br/><br/>Failback nach einem Failover vom primären zum sekundären Standort |Kein Datenverlust<br/><br/>Downtime entsteht, während der virtuelle Computer am primären Standort heruntergefahren und am sekundären Standort wieder verfügbar gemacht wird.<br/><br/>Auswirkungen auf virtuelle Computer: Zielcomputer werden nach dem Failover zu Quellcomputern. |Starten Sie das Failover.<br/><br/>Überwachen Sie den Fortschritt auf der Registerkarte **Aufträge** . Die Quellcomputer werden heruntergefahren.<br/><br/>Am sekundären Standort werden Replikatcomputer gestartet.<br/><br/>Azure: Stellen Sie über das Azure-Portal eine Verbindung mit dem Replikatcomputer her.<br/><br/>Sekundärer Standort: Greifen Sie auf den Computer über den gleichen Host und in der gleichen Cloud zu.<br/><br/>Führen Sie ein Commit für das Failover aus. |
| **Nicht geplantes Failover** |Reaktives Failover, falls aufgrund eines unerwarteten Vorfalls (beispielsweise ein Stromausfall oder ein Virusangriff) nicht mehr auf einen primären Standort zugegriffen werden kann <br/><br/> Ein nicht geplantes Failover kann auch durchgeführt werden, wenn der primäre Standort nicht verfügbar ist. |Datenverluste abhängig von der konfigurierten Replikationshäufigkeit<br/><br/>Daten sind jeweils auf dem Stand der letzten Synchronisierung |Starten Sie das Failover.<br/><br/>Überwachen Sie den Fortschritt auf der Registerkarte **Aufträge** . Optional: Fahren Sie die virtuellen Computer herunter, und synchronisieren Sie die aktuellen Daten.<br/><br/>Am sekundären Standort werden Replikatcomputer gestartet.<br/><br/>Azure: Stellen Sie über das Azure-Portal eine Verbindung mit dem Replikatcomputer her.<br/><br/>Sekundärer Standort: Greifen Sie auf den Computer über den gleichen Host und in der gleichen Cloud zu.<br/><br/>Führen Sie ein Commit für das Failover aus. |

Welche Failover konkret unterstützt werden, hängt von Ihrem Bereitstellungsszenario ab.

| **Failover-Richtung** | **Test-Failover** | **Geplantes Failover** | **Nicht geplantes Failover** |
| --- | --- | --- | --- |
| Primärer VMM-Standort zu sekundärem VMM-Standort |Unterstützt |Unterstützt |Unterstützt |
| Sekundärer VMM-Standort zu primärem VMM-Standort |Unterstützt |Unterstützt |Unterstützt |
| Cloudintern (einzelner VMM-Server) |Unterstützt |Unterstützt |Unterstützt |
| VMM-Standort zu Azure |Unterstützt |Unterstützt |Unterstützt |
| Azure zu VMM-Standort |Nicht unterstützt |Unterstützt |Nicht unterstützt |
| Hyper-V-Standort zu Azure |Unterstützt |Unterstützt |Unterstützt |
| Azure zu Hyper-V-Standort |Nicht unterstützt |Unterstützt |Nicht unterstützt |
| VMware-Standort zu Azure |Unterstützt (erweitertes Szenario)<br/><br/> Nicht unterstützt (älteres Szenario) |Nicht unterstützt |Unterstützt |
| Physischer Server zu Azure |Unterstützt (erweitertes Szenario)<br/><br/> Nicht unterstützt (älteres Szenario) |Nicht unterstützt |Unterstützt |

## <a name="failover-and-failback"></a>Failover und Failback
Als Failoverziel für virtuelle Computer wird abhängig von der verwendeten Bereitstellung entweder ein lokaler sekundärer Standort oder Azure verwendet. Ein Computer, für den ein Failover zu Azure durchgeführt wird, wird als virtueller Azure-Computer erstellt. Failover können für einen einzelnen virtuellen Computer oder physischen Server oder für einen Wiederherstellungsplan durchgeführt werden. Wiederherstellungspläne enthalten mindestens eine sortierte Gruppe mit geschützten virtuellen Computern oder physischen Servern. Sie dienen zur Orchestrierung des Failovers mehrerer Computer, deren Failover dann gemäß der jeweiligen Gruppe durchgeführt wird. [hier](site-recovery-create-recovery-plans.md) .

Wenn das Failover abgeschlossen ist und die Computer an einem sekundären Standort in Betrieb sind, gilt Folgendes:

* Nach einem Failover zu Azure sind die Computer am primären oder sekundären Standort nicht geschützt und werden auch nicht repliziert. In Azure werden die virtuellen Computer in geografisch repliziertem Speicher gespeichert. Dies sorgt zwar für Resilienz, nicht aber für Replikation.
* Nach einem nicht geplanten Failover zu einem sekundären Standort sind die Computer am sekundären Standort nicht geschützt und werden nicht repliziert.
* Nach einem geplanten Failover zu einem sekundären Standort sind die Computer am sekundären Standort geschützt.

### <a name="failback-from-secondary-site"></a>Failback vom sekundären Standort
Beim Failback von einem sekundären zu einem primären Standort wird der gleiche Prozess verwendet wie beim umgekehrten Failover. Wenn für das Failover vom primären zum sekundären Rechenzentrum ein Commit ausgeführt wurde und das Failover abgeschlossen ist, können Sie die umgekehrte Replikation initiieren, sobald Ihr primärer Standort wieder verfügbar ist. Bei der umgekehrten Replikation wird die Replikation zwischen dem sekundären und dem primären Standort initiiert, indem die geänderten Daten synchronisiert werden. Die umgekehrte Replikation versetzt die virtuellen Computer in einen geschützten Zustand, das sekundäre Rechenzentrum bleibt aber weiterhin aktiv. Initiieren Sie ein geplantes Failover vom sekundären zum primären Standort, und führen Sie anschließend eine erneute umgekehrte Replikation durch, um den primären Standort zum aktiven Standort zu machen.

### <a name="failback-from-azure"></a>Failback von Azure
Wenn Sie ein Failover zu Azure durchgeführt haben, werden Ihre virtuellen Computer durch die Azure-Resilienzfeatures für virtuelle Computer geschützt. Führen Sie ein geplantes Failover durch, um den ursprünglichen primären Standort zum aktiven Standort zu machen. Als Ziel für das Failback können Sie den ursprünglichen Standort oder auch einen anderen Standort verwenden, falls der ursprüngliche Standort nicht verfügbar ist. Initiieren Sie eine umgekehrte Replikation, um nach dem Failback die Replikation zum primären Standort zu reaktivieren.

### <a name="failover-considerations"></a>Überlegungen zum Failover
* **IP-Adresse nach dem Failover**: Standardmäßig besitzt ein Computer nach einem Failover eine andere IP-Adresse als der Quellcomputer. So können Sie die IP-Adresse beibehalten:
  * **Sekundärer Standort**: Wenn Sie ein Failover zu einem sekundären Standort durchführen und eine IP-Adresse beibehalten möchten, lesen Sie [diesen Artikel](http://blogs.technet.com/b/scvmm/archive/2014/04/04/retaining-ip-address-after-failover-using-hyper-v-recovery-manager.aspx) . Eine öffentliche IP-Adresse können Sie beibehalten, wenn Ihr Internet-Service Provider dies unterstützt.
  * **Azure**: Im Falle eines Failovers zu Azure können Sie in den Eigenschaften des virtuellen Computers auf der Registerkarte **Konfigurieren** die IP-Adresse angeben, die Sie zuweisen möchten. Öffentliche IP-Adressen können nach einem Failover zu Azure nicht beibehalten werden. Als interne Adressen verwendete RFC 1918-fremde Adressräume können beibehalten werden.
* **Partielles Failover**: Wenn ein Failover nur für einen Teil des Standorts durchgeführt werden soll, gilt Folgendes:

  * **Sekundärer Standort**: Wenn Sie für einen Teil eines primären Standorts ein Failover zu einem sekundären Standort durchführen und wieder eine Verbindung mit dem primären Standort herstellen möchten, verwenden Sie eine VPN-Verbindung zwischen den beiden Standorten, um eine Verbindung zwischen Failover-Anwendungen am sekundären Standort und Infrastrukturkomponenten am primären Standort herzustellen. Beim Failover eines gesamten Subnetzes können Sie die IP-Adressen der virtuellen Computer beibehalten. Beim partiellen Failover eines Subnetzes kann die IP-Adresse des virtuellen Computers nicht beibehalten werden, da Subnetze nicht aufgeteilt werden können.
  * **Azure**: Wenn Sie für einen Teil eines Standorts ein Failover zu Azure durchführen und wieder eine Verbindung mit dem primären Standort herstellen möchten, können Sie eine VPN-Verbindung zwischen den Standorten verwenden, um eine Verbindung zwischen einer Failover-Anwendung in Azure und Infrastrukturkomponenten am primären Standort herzustellen. Bei einem Failover des gesamten Subnetzes können Sie die IP-Adresse des virtuellen Computers beibehalten. Beim partiellen Failover eines Subnetzes kann die IP-Adresse des virtuellen Computers nicht beibehalten werden, da Subnetze nicht aufgeteilt werden können.
* **Laufwerkbuchstabe**: Wenn Sie nach dem Failover den Laufwerkbuchstaben des virtuellen Computers beibehalten möchten, können Sie die SAN-Richtlinie für den virtuellen Computer** **aktivieren. Die Datenträger von virtuellen Computern werden automatisch online geschaltet. [Weitere Informationen](https://technet.microsoft.com/library/gg252636.aspx)
* **Weiterleiten von Clientanforderungen**: Site Recovery verwendet Azure Traffic Manager, um Clientanforderungen nach dem Failover an Ihre Anwendung weiterzuleiten.

## <a name="run-a-test-failover"></a>Ausführen eines Testfailovers
Im Dokument [Testfailover in Azure](site-recovery-test-failover-to-azure.md) finden Sie Informationen zur Durchführung eines Testfailovers in Azure. Wenn Sie das Testfailover zu einem anderen lokal verwalteten VMM-Server durchführen möchten, lesen Sie das Dokument [Testfailover zu VMM](site-recovery-test-failover-vmm-to-vmm.md).  

## <a name="run-a-planned-failover-primary-to-secondary"></a>Durchführen eines geplanten Failovers (primär zu sekundär)
 Hier erfahren Sie, wie Sie ein geplantes Failover für einen Wiederherstellungsplan durchführen. Alternativ können Sie das Failover über die Registerkarte **Virtuelle Computer** auch für einen einzelnen virtuellen Computer durchführen.

1. Vergewissern Sie sich zunächst, dass bei allen virtuellen Computern, für die ein Failover durchgeführt werden soll, die erste Replikation abgeschlossen ist.
2. Wählen Sie **Wiederherstellungspläne** > *Name des Wiederherstellungsplans* aus. Klicken Sie auf **Failover** > **Planned Failover**veröffentlichen.
3. Wählen Sie auf der Seite **Geplantes Failover bestätigen **den Quell- und Zielort aus. Beachten Sie die Failover-Richtung.

   * Wenn bisherige Failover wie erwartet funktioniert haben und sich alle virtuellen Servercomputer entweder am Quell- oder am Zielort befinden, dienen die Angaben zur Failover-Richtung nur zu Informationszwecken.
   * Wenn virtuelle Computer sowohl am Quell- als auch am Zielort aktiv sind, erscheint die Schaltfläche **Richtung ändern** . Mit dieser Schaltfläche können Sie die Richtung für das Failover ändern.
4. Wenn Sie ein Failover zu Azure durchführen und die Datenverschlüsselung für die Cloud aktiviert ist, wählen Sie unter **Verschlüsselungsschlüssel** das Zertifikat aus, das beim Aktivieren der Datenverschlüsselung im Rahmen der Anbieterinstallation auf dem VMM-Server ausgestellt wurde.
5. Zu Beginn eines geplantes Failovers wird zur Vermeidung von Datenverlusten zunächst der virtuelle Computer heruntergefahren. Der Fortschritt des Failovers wird auf der Registerkarte **Aufträge** angezeigt. Tritt während des Failovers ein Fehler auf (entweder auf einem virtuellen Computer oder in einem Skript aus dem Wiederherstellungsplan), wird das geplante Failover des Wiederherstellungsplans beendet. Sie können das Failover erneut initiieren.
6. Nach der Erstellung der virtuellen Replikatcomputer weist deren Status darauf hin, dass ein Commit aussteht. Klicken Sie auf **Commit** , um ein Commit für das Failover auszuführen.
7. Nach Abschluss der Replikation werden die virtuellen Computer am sekundären Standort gestartet.

## <a name="run-an-unplanned-failover"></a>Durchführen eines nicht geplanten Failovers
Hier erfahren Sie, wie Sie ein nicht geplantes Failover für einen Wiederherstellungsplan durchführen. Alternativ können Sie das Failover über die Registerkarte **Virtuelle Computer** auch für einen einzelnen virtuellen Computer oder physischen Server durchführen.

1. Wählen Sie **Wiederherstellungspläne** > *Name des Wiederherstellungsplans* aus. Klicken Sie auf **Failover** > **Unplanned Failover**veröffentlichen.
2. Wählen Sie auf der Seite **Nicht geplantes Failover bestätigen **den Quell- und Zielort aus. Beachten Sie die Failover-Richtung.

   * Wenn bisherige Failover wie erwartet funktioniert haben und sich alle virtuellen Servercomputer entweder am Quell- oder am Zielort befinden, dienen die Angaben zur Failover-Richtung nur zu Informationszwecken.
   * Wenn virtuelle Computer sowohl am Quell- als auch am Zielort aktiv sind, erscheint die Schaltfläche **Richtung ändern** . Mit dieser Schaltfläche können Sie die Richtung für das Failover ändern.
3. Wenn Sie ein Failover zu Azure durchführen und die Datenverschlüsselung für die Cloud aktiviert ist, wählen Sie unter **Verschlüsselungsschlüssel** das Zertifikat aus, das beim Aktivieren der Datenverschlüsselung im Rahmen der Anbieterinstallation auf dem VMM-Server ausgestellt wurde.
4. Wählen Sie **Virtuelle Computer herunterfahren und die aktuellen Daten synchronisieren** aus, damit Site Recovery die geschützten virtuellen Computer herunterfährt und die Daten synchronisiert, sodass das Failover mit der neuesten Version der Daten erfolgt. Wenn Sie diese Option nicht auswählen oder der Vorgang nicht erfolgreich ausgeführt werden kann, wird das Failover für den neuesten verfügbaren Wiederherstellungspunkt des virtuellen Computers durchgeführt.
5. Der Fortschritt des Failovers wird auf der Registerkarte **Aufträge** angezeigt. Beachten Sie, dass der Wiederherstellungsplan bei einem nicht geplanten Failover auch dann bis zum Abschluss ausgeführt wird, wenn Fehler auftreten.
6. Nach dem Failover weist der Status der virtuellen Computer darauf hin, dass ein Commit** **aussteht. Klicken Sie auf **Commit** , um ein Commit für das Failover auszuführen.
7. Wenn Sie für die Replikation die Verwendung mehrerer Wiederherstellungspunkte konfigurieren, können Sie unter „Wiederherstellungspunkt ändern“ einen Wiederherstellungspunkt auswählen, bei dem es sich nicht um den neuesten (standardmäßig verwendeten) Wiederherstellungspunkt handelt. Nach dem Commit werden die zusätzlichen Wiederherstellungspunkte entfernt.
8. Nach Abschluss der Replikation werden die virtuellen Computer am sekundären Standort gestartet und ausgeführt. Sie sind jedoch nicht geschützt und werden auch nicht repliziert. Wenn der primäre Standort mit der gleichen zugrunde liegenden Infrastruktur wieder verfügbar ist, klicken Sie auf **Umgekehrt replizieren** , um die umgekehrte Replikation zu starten. Dadurch wird sichergestellt, dass alle Daten wieder an den primären Standort repliziert werden und der virtuelle Computer für ein erneutes Failover bereit ist. Bei der umgekehrten Replikation nach einem nicht geplanten Failover fällt ein gewisser zusätzlicher Datenübertragungsaufwand an. Bei der Übertragung wird die gleiche Methode verwendet, die auch in den Einstellungen der ersten Replikation für die Cloud konfiguriert ist.

## <a name="failback-from-secondary-to-primary"></a>Failback (sekundär zu primär)
 Nach dem Failover vom primären zum sekundären Standort sind die virtuellen Replikatcomputer nicht durch Site Recovery geschützt, und der sekundäre Standort fungiert nun als primärer Standort. Gehen Sie wie folgt vor, um ein Failback zum ursprünglichen primären Standort durchzuführen. Hier erfahren Sie, wie Sie ein geplantes Failover für einen Wiederherstellungsplan durchführen. Alternativ können Sie das Failover über die Registerkarte **Virtuelle Computer** auch für einen einzelnen virtuellen Computer durchführen.

1. Wählen Sie **Wiederherstellungspläne** > *Name des Wiederherstellungsplans* aus. Klicken Sie auf **Failover** > **Planned Failover**veröffentlichen.
2. Wählen Sie auf der Seite **Geplantes Failover bestätigen **den Quell- und Zielort aus. Beachten Sie die Failover-Richtung. Wenn das Failover vom primären Standort erwartungsgemäß funktioniert hat und sich alle virtuellen Computer am sekundären Standort befinden, dient diese Angabe nur zu Informationszwecken.
3. Wählen Sie bei einem Failback von Azure Einstellungen unter **Datensynchronisierung**aus:

   * **Daten vor dem Failover synchronisieren (nur Deltaänderungen synchronisieren)**: Diese Option minimiert die Ausfallzeiten der virtuellen Computer, da diese für die Synchronisierung nicht heruntergefahren werden. Die Option bewirkt Folgendes:
     * Phase 1: Sie erstellt eine Momentaufnahme des virtuellen Computers in Azure und kopiert sie auf den lokalen Hyper-V-Host. Der Computer wird weiterhin in Azure ausgeführt.
     * Phase 2: Sie fährt den virtuellen Computer in Azure herunter, damit keine neuen Änderungen vorgenommen werden. Die letzten Änderungen werden an den lokalen Server übertragen, und der lokale virtuelle Computer wird gestartet.

    - **Daten nur während Failover synchronisieren (vollständiger Download)**: Verwenden Sie diese Option, wenn Sie über einen längeren Zeitraum Azure verwendet haben. Diese Option ist schneller, da erwartet wird, dass die meisten Daten auf dem Datenträger geändert wurden, und keine Zeit mit der Prüfsummenberechnung verschwendet werden soll. Mit der Option wird ein Download des Datenträgers durchgeführt. Sie ist auch nützlich, wenn der lokale virtuelle Computer gelöscht wurde.

    > [!NOTE] 
    > Es wird empfohlen, diese Option zu verwenden, wenn Sie Azure seit einer Weile nutzen (einen Monat oder länger) oder wenn der lokale virtuelle Computer gelöscht wurde. Mit dieser Option werden keine Prüfsummenberechnungen durchgeführt.
    >
    >


1. Wenn Sie ein Failover zu Azure durchführen und die Datenverschlüsselung für die Cloud aktiviert ist, wählen Sie unter **Verschlüsselungsschlüssel** das Zertifikat aus, das beim Aktivieren der Datenverschlüsselung im Rahmen der Anbieterinstallation auf dem VMM-Server ausgestellt wurde.
2. Standardmäßig wird der letzte Wiederherstellungspunkt verwendet. Unter **Wiederherstellungspunkt ändern** können Sie bei Bedarf einen anderen Wiederherstellungspunkt angeben.
3. Klicken Sie auf das Häkchen, um das Failback zu starten.  Der Fortschritt des Failovers wird auf der Registerkarte **Aufträge** angezeigt.
4. Falls Sie die Option ausgewählt haben, mit der die Daten vor dem Failover synchronisiert werden, klicken Sie auf **Aufträge** > <planned failover job name>**Failover abschließen**veröffentlichen. Dadurch wird der Azure-Computer heruntergefahren, die neuesten Änderungen werden an den lokalen virtuellen Computer übertragen, und der lokale virtuelle Computer wird gestartet.
5. Anschließend können Sie sich bei dem virtuellen Computer anmelden und sich vergewissern, dass er wie erwartet verfügbar ist.
6. Der Status des virtuellen Computers weist darauf hin, dass ein Commit aussteht. Klicken Sie auf **Commit** , um ein Commit für das Failover auszuführen.
7. Klicken Sie anschließend auf **Umgekehrt replizieren** , um das Failback abzuschließen und den virtuellen Computer am primären Standort zu schützen.

## <a name="failback-to-an-alternate-location"></a>Failback zu einem alternativen Standort
Wenn Sie den Schutz zwischen einem [Hyper-V-Standort und Azure](site-recovery-hyper-v-site-to-azure.md) bereitgestellt haben, können Sie ein Failback von Azure zu einem alternativen lokalen Standort durchführen. Dies ist hilfreich, wenn Sie neue lokale Hardware einrichten müssen. Gehen Sie hierzu wie folgt vor:

1. Wenn Sie neue Hardware einrichten möchten, installieren Sie Windows Server 2012 R2 und die Hyper-V-Rolle auf dem Server.
2. Erstellen Sie einen virtuellen Netzwerkswitch mit dem gleichen Namen wie auf dem ursprünglichen Server.
3. Wählen Sie **Geschützte Elemente** -> **Schutzgruppe** -> <ProtectionGroupName> -> <VirtualMachineName> für das Failback und anschließend die Option **Geplantes Failover** aus.
4. Erfassen und speichern Sie unter **Geplantes Failover bestätigen** select ****veröffentlichen.
5. Wählen Sie unter **Hostname** den neuen Hyper-V-Hostserver aus, auf dem sich der virtuelle Computer befinden soll.
6. Für die Datensynchronisierung empfiehlt sich die Option **Daten vor dem Failover synchronisieren**. Diese Option minimiert die Ausfallzeiten der virtuellen Computer, da diese für die Synchronisierung nicht heruntergefahren werden. Die Option bewirkt Folgendes:

   * Phase 1: Sie erstellt eine Momentaufnahme des virtuellen Computers in Azure und kopiert sie auf den lokalen Hyper-V-Host. Der Computer wird weiterhin in Azure ausgeführt.
   * Phase 2: Sie fährt den virtuellen Computer in Azure herunter, damit keine neuen Änderungen vorgenommen werden. Die letzten Änderungen werden an den lokalen Server übertragen, und der lokale virtuelle Computer wird gestartet.
7. Klicken Sie auf das Häkchen, um das Failover (Failback) zu starten.
8. Klicken Sie auf **Aufträge** > <planned failover job> > **Failover abschließen**veröffentlichen. Dadurch wird der Azure-Computer heruntergefahren, die neuesten Änderungen werden an den lokalen virtuellen Computer übertragen, und der lokale virtuelle Computer wird gestartet.
9. Sie können sich bei dem lokalen virtuellen Computer anmelden und sich vergewissern, das alles wie erwartet funktioniert. Klicken Sie anschließend auf **Commit** , um das Failover abzuschließen.
10. Klicken Sie auf **Umgekehrt replizieren** , um den Schutz des lokalen virtuellen Computers zu starten.

    > [!NOTE]
    > Wenn Sie den Failbackauftrag während der Datensynchronisierung abbrechen, wird der lokale virtuelle Computer als beschädigt angezeigt. Das liegt daran, dass bei der Datensynchronisierung die aktuellen Daten von den Datenträgern virtueller Azure-Computer auf die lokalen Datenträger kopiert werden. Bis zum Abschluss der Synchronisierung befinden sich die Datenträgerdaten unter Umständen nicht in einem konsistenten Zustand. Wenn der lokale virtuelle Computer nach dem Abbruch der Datensynchronisierung gestartet wird, ist das Starten unter Umständen nicht möglich. Lösen Sie zum Abschließen der Datensynchronisierung das Failover erneut aus.
    >
    >



<!--HONumber=Jan17_HO2-->


