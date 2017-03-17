---
title: "Überwachung und Problembehandlung für den Schutz von virtuellen Computern und physischen Servern | Microsoft-Dokumentation"
description: "Azure Site Recovery koordiniert Replikation, Failover und Wiederherstellung virtueller Computer auf lokalen Servern in Azure oder in einem sekundären Datencenter. Nutzen Sie diesen Artikel zum Überwachen und Durchführen der Problembehandlung für Virtual Machine Manager und den Schutz von Hyper-V-Standorten."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: mkjain
editor: 
ms.assetid: 0fc8e368-0c0e-4bb1-9d50-cffd5ad0853f
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 01/19/2017
ms.author: rajanaki
translationtype: Human Translation
ms.sourcegitcommit: d9dad6cff80c1f6ac206e7fa3184ce037900fc6b
ms.openlocfilehash: 664a1bd9df9ace4993a8389dbeb049e721932082
ms.lasthandoff: 03/06/2017


---
# <a name="monitor-and-troubleshoot-protection-for-virtual-machines-and-physical-servers"></a>Überwachung und Problembehandlung für den Schutz von virtuellen Computern und physischen Servern
In diesem Leitfaden zur Überwachung und Problembehandlung wird beschrieben, wie Sie die Replikationsintegrität nachverfolgen und die Problembehandlung für Azure Site Recovery durchführen.

## <a name="understand-the-components"></a>Grundlegendes zu den Komponenten
### <a name="vmware-virtual-machine-or-physical-server-site-deployment-for-replication-between-on-premises-and-azure"></a>Bereitstellung von virtuellen VMware-Computern oder physischen Servern für die Replikation zwischen der lokalen Umgebung und Azure
Zum Einrichten der Datenbankwiederherstellung zwischen einem lokalen virtuellen VMware-Computer oder physischen Server und Azure müssen Sie den Konfigurationsserver, Masterzielserver und die Prozessserverkomponenten auf Ihrem virtuellen Computer oder Server einrichten. Wenn Sie den Schutz für den Quellserver aktivieren, installiert Azure Site Recovery das Mobile Apps-Feature von Microsoft Azure App Service. Nach einem lokalen Ausfall und dem Failover des Quellservers zu Azure müssen Kunden einen Prozessserver in Azure und einen Masterzielserver lokal einrichten, um den Quellserver lokal wiederherzustellen.

![Bereitstellung per VMware bzw. am physischen Standort für die Replikation zwischen der lokalen Umgebung und Azure](media/site-recovery-monitoring-and-troubleshooting/image18.png)

### <a name="virtual-machine-manager-site-deployment-for-replication-between-on-premises-sites"></a>Virtual Machine Manager-Standortbereitstellung für die Replikation zwischen lokalen Standorten
Zum Einrichten der Datenbankwiederherstellung zwischen zwei lokalen Standorten müssen Sie den Azure Site Recovery-Anbieter herunterladen und auf dem Virtual Machine Manager-Server installieren. Der Anbieter benötigt eine Internetverbindung, damit sichergestellt werden kann, dass alle Vorgänge, die über das Azure-Portal ausgelöst werden, in lokale Vorgänge übersetzt werden.

![Virtual Machine Manager-Standortbereitstellung für die Replikation zwischen lokalen Standorten](media/site-recovery-monitoring-and-troubleshooting/image1.png)

### <a name="virtual-machine-manager-site-deployment-for-replication-between-on-premises-locations-and-azure"></a>Virtual Machine Manager-Standortbereitstellung für die Replikation zwischen der lokalen Umgebung und Azure
Zum Einrichten der Datenbankwiederherstellung zwischen lokalen Standorten und Azure müssen Sie den Azure Site Recovery-Anbieter herunterladen und auf dem Virtual Machine Manager-Server installieren. Außerdem müssen Sie den Azure Recovery Services-Agent installieren, der auf jedem Hyper-V-Host vorhanden sein muss. Klicken Sie [hier](site-recovery-hyper-v-azure-architecture.md), um weitere Informationen zu erhalten.

![Virtual Machine Manager-Standortbereitstellung für die Replikation zwischen der lokalen Umgebung und Azure](media/site-recovery-monitoring-and-troubleshooting/image2.png)

### <a name="hyper-v-site-deployment-for-replication-between-on-premises-locations-and-azure"></a>Hyper-V-Standortbereitstellung für die Replikation zwischen lokalen Standorten und Azure
Dieser Prozess ähnelt der Bereitstellung von Virtual Machine Manager. Der einzige Unterschied besteht darin, dass der Azure Site Recovery-Anbieter und der Azure Recovery Services-Agent auf dem Hyper-V-Host selbst installiert werden. [Weitere Informationen](site-recovery-hyper-v-azure-architecture.md). verfügbar.

## <a name="monitor-configuration-protection-and-recovery-operations"></a>Überwachen von Konfigurations-, Schutz- und Wiederherstellungsvorgängen
Jeder Vorgang in Azure Site Recovery wird auf der Registerkarte **AUFTRÄGE** überwacht und nachverfolgt. Greifen Sie bei allen Fehlern, die sich auf die Konfiguration, den Schutz oder die Wiederherstellung beziehen, auf die Registerkarte **AUFTRÄGE** zu, und suchen Sie nach Fehlern.

![Filter „Fehler“ auf der Registerkarte „AUFTRÄGE“](media/site-recovery-monitoring-and-troubleshooting/image3.png)

Wenn auf der Registerkarte **AUFTRÄGE** Fehler angezeigt werden, können Sie auf den Auftrag und dann auf die Option **FEHLERDETAILS** für den Auftrag klicken.

![Schaltfläche „FEHLERDETAILS“](media/site-recovery-monitoring-and-troubleshooting/image4.png)

Anhand der Fehlerdetails können Sie eine mögliche Ursache ermitteln und erhalten Empfehlungen zum Beheben des Problems.

![Dialogfeld zum Anzeigen von Fehlerdetails für einen bestimmten Auftrag](media/site-recovery-monitoring-and-troubleshooting/image5.png)

Im vorherigen Beispiel bewirkt scheinbar ein anderer aktiver Vorgang, dass für die Schutzkonfiguration ein Fehler auftritt. Beheben Sie das Problem gemäß der Empfehlung, und klicken Sie dann auf **NEU STARTEN**, um den Vorgang erneut zu initiieren.

![Schaltfläche „NEU STARTEN“ auf der Registerkarte „AUFTRÄGE“](media/site-recovery-monitoring-and-troubleshooting/image6.png)

Die Option **NEU STARTEN** ist nicht für alle Vorgänge verfügbar. Falls die Option **NEU STARTEN** für einen Vorgang nicht vorhanden ist, können Sie zum Objekt zurückkehren und den Vorgang erneut durchführen. Sie können alle in Bearbeitung befindlichen Aufträge abbrechen, indem Sie die Schaltfläche **ABBRECHEN** verwenden.

![Schaltfläche „ABBRECHEN“](media/site-recovery-monitoring-and-troubleshooting/image7.png)

## <a name="monitor-replication-health-for-virtual-machines"></a>Überwachen der Replikationsintegrität für virtuelle Computer
Mit dem Azure-Portal können Sie die Remoteüberwachung von Azure Site Recovery-Anbietern für alle geschützten Entitäten durchführen. Klicken Sie auf **GESCHÜTZTE ELEMENTE** und dann auf **VMM-CLOUDS** oder **SCHUTZGRUPPEN**. Die Registerkarte **VMM-CLOUDS** ist nur für Bereitstellungen verfügbar, die auf Virtual Machine Manager basieren. Für andere Szenarien befinden sich die geschützten Entitäten auf der Registerkarte **SCHUTZGRUPPEN**.

![Optionen „VMM-CLOUDS“ und „SCHUTZGRUPPEN“](media/site-recovery-monitoring-and-troubleshooting/image8.png)

Klicken Sie unter der entsprechenden Cloud oder Schutzgruppe auf eine geschützte Entität, um alle verfügbaren Vorgänge im unteren Bereich anzuzeigen.

![Verfügbare Optionen für eine geschützte Entität](media/site-recovery-monitoring-and-troubleshooting/image9.png)

Wie im vorherigen Screenshot dargestellt, lautet die Integrität des virtuellen Computers **Kritisch**. Sie können unten auf die Schaltfläche **FEHLERDETAILS** klicken, um den Fehler anzuzeigen. Beheben Sie das Problem basierend auf den **möglichen Ursachen** und der **Empfehlung**.

![Schaltfläche „Fehlerdetails“](media/site-recovery-monitoring-and-troubleshooting/image10.png)

![Fehler und Empfehlungen im Dialogfeld „Fehlerdetails“](media/site-recovery-monitoring-and-troubleshooting/image11.png)

> [!NOTE]
> Wenn aktive Vorgänge in Bearbeitung oder fehlgeschlagen sind, können Sie unter **AUFTRÄGE** wie oben beschrieben den Fehler zu einem bestimmten Auftrag anzeigen.
>
>

## <a name="troubleshoot-on-premises-hyper-v-issues"></a>Behandlung von lokalen Hyper-V-Problemen
Stellen Sie eine Verbindung mit der lokalen Hyper-V-Manager-Konsole her, wählen Sie den virtuellen Computer aus, und prüfen Sie die Replikationsintegrität.

![Option zum Anzeigen der Replikationsintegrität in der Hyper-V-Manager-Konsole](media/site-recovery-monitoring-and-troubleshooting/image12.png)

In diesem Fall ist **Replikationsintegrität** auf **Kritisch** festgelegt. Klicken Sie mit der rechten Maustaste auf den virtuellen Computer, und klicken Sie dann auf **Replikation** > **Replikationsstatus anzeigen**, um die Details einzublenden.

![Replikationsintegrität für einen bestimmten virtuellen Computer](media/site-recovery-monitoring-and-troubleshooting/image13.png)

Wenn die Replikation für den virtuellen Computer angehalten wird, klicken Sie mit der rechten Maustaste auf den virtuellen Computer, und klicken Sie anschließend auf **Replikation** > **Replikation fortsetzen**.

![Option zum Fortsetzen der Replikation in der Hyper-V-Manager-Konsole](media/site-recovery-monitoring-and-troubleshooting/image19.png)

Wenn ein virtueller Computer einen neuen Hyper-V-Host im Cluster oder auf einem eigenständigen Computer migriert und der Hyper-V-Host per Azure Site Recovery konfiguriert wurde, ist die Replikation des virtuellen Computers nicht betroffen. Stellen Sie sicher, dass der neue Hyper-V-Host alle Voraussetzungen erfüllt und mit Azure Site Recovery konfiguriert wurde.

### <a name="event-log"></a>Ereignisprotokoll
| Ereignisquellen | Details |
| --- |:--- |
| **Anwendungs- und Dienstprotokolle/Microsoft/VirtualMachineManager/Server/Admin** (Virtual Machine Manager-Server) |Ermöglicht eine nützliche Protokollierung für die Problembehandlung vieler unterschiedlicher Virtual Machine Manager-Probleme. |
| **Anwendungs- und Dienstprotokolle/MicrosoftAzureRecoveryServices/Replikation** (Hyper-V-Host) |Ermöglicht eine nützliche Protokollierung für die Problembehandlung vieler Probleme, die beim Microsoft Azure Recovery Services-Agent auftreten können. <br/> ![Speicherort der Ereignisquelle „Replikation“ für Hyper-V-Host](media/site-recovery-monitoring-and-troubleshooting/eventviewer03.png) |
| **Anwendungs- und Dienstprotokolle/Microsoft/Azure Site Recovery/Anbieter/Betriebsbereit** (Hyper-V-Host) |Ermöglicht eine nützliche Protokollierung für die Problembehandlung vieler Probleme mit dem Microsoft Azure Site Recovery-Dienst. <br/> ![Speicherort der Ereignisquelle „Betriebsbereit“ für Hyper-V-Host](media/site-recovery-monitoring-and-troubleshooting/eventviewer02.png) |
| **Anwendungs- und Dienstprotokolle/Microsoft/Windows/Hyper-V-VMMS/Admin** (Hyper-V-Host) |Ermöglicht eine nützliche Protokollierung für die Problembehandlung vieler Probleme, die bei der Verwaltung eines virtuellen Hyper-V-Computers auftreten können. <br/> ![Speicherort der Ereignisquelle „Virtual Machine Manager“ für Hyper-V-Host](media/site-recovery-monitoring-and-troubleshooting/eventviewer01.png) |

### <a name="hyper-v-replication-logging-options"></a>Protokollierungsoptionen für die Hyper-V-Replikation
Alle Ereignisse für die Hyper-V-Replikation werden im Protokoll „Hyper-V-VMMS\\Admin“ unter „Anwendungs- und Dienstprotokolle\\Microsoft\\Windows“ protokolliert. Außerdem können Sie ein analytisches Protokoll für den Hyper-V-Verwaltungsdienst für virtuelle Computer aktivieren. Um dieses Protokoll zu aktivieren, müssen Sie zunächst die analytischen und Debugprotokolle in der Ereignisanzeige einblenden. Öffnen Sie die Ereignisanzeige, und klicken Sie auf **Ansicht** > **Analytische und Debugprotokolle einblenden**.

![Option „Analytische und Debugprotokolle einblenden“](media/site-recovery-monitoring-and-troubleshooting/image14.png)

Unter **Hyper-V-VMMS** wird ein analytisches Protokoll angezeigt.

![Analytisches Protokoll in der Struktur der Ereignisanzeige](media/site-recovery-monitoring-and-troubleshooting/image15.png)

Klicken Sie im Bereich **Aktionen** auf **Protokoll aktivieren**. Nach der Aktivierung wird das Protokoll in **Systemmonitor** als **Ereignisablaufverfolgungs-Sitzung** unter **Datensammlersätze** angezeigt.

![Ereignisablaufverfolgungssitzung in der Struktur des Systemmonitors](media/site-recovery-monitoring-and-troubleshooting/image16.png)

Zum Anzeigen der gesammelten Daten müssen Sie zuerst die Ablaufverfolgungssitzung beenden, um das Protokoll zu deaktivieren. Speichern Sie das Protokoll, und öffnen Sie es in der Ereignisanzeige, oder konvertieren Sie es mit anderen Tools wie gewünscht.

## <a name="reach-out-for-microsoft-support"></a>Microsoft-Support
### <a name="log-collection"></a>Protokollsammlung
Unter [ASR Data Collection and Analysis Using the VMM Support Diagnostics Platform (SDP) Tool](http://social.technet.microsoft.com/wiki/contents/articles/28198.asr-data-collection-and-analysis-using-the-vmm-support-diagnostics-platform-sdp-tool.aspx) (ASR-Datensammlung und Analyse mit dem Tool VMM Support Diagnostics Platform (SDP)) finden Sie Informationen zum Virtual Machine Manager-Standortschutz und zur Erfassung der erforderlichen Protokolle.

Laden Sie für den Schutz von Hyper-V-Standorten dieses [Tool](https://dcupload.microsoft.com/tools/win7files/DIAG_ASRHyperV_global.DiagCab) herunter, und führen Sie es auf dem Hyper-V-Host aus, um die Protokolle zu sammeln.

Informationen zum Sammeln der erforderlichen Protokolle für physische oder VMware-Szenarien finden Sie unter [Azure Site Recovery Log Collection for VMware and Physical site protection](http://social.technet.microsoft.com/wiki/contents/articles/30677.azure-site-recovery-log-collection-for-vmware-and-physical-site-protection.aspx) (Azure Site Recovery-Protokollsammlung für den VMware-Schutz und den Schutz von physischen Standorten).

Das Tool sammelt die Protokolle lokal in einem zufällig benannten Unterordner unter „%LocalAppData%\ElevatedDiagnostics“.

![Beispielschritte aus dem Hyper-V-Standortschutz](media/site-recovery-monitoring-and-troubleshooting/animate01.gif)

### <a name="open-a-support-ticket"></a>Öffnen eines Supporttickets
Um ein Supportticket für Azure Site Recovery zu öffnen, wenden Sie sich unter <http://aka.ms/getazuresupport> an den Azure-Support.

## <a name="kb-articles"></a>KB-Artikel
* [Beibehalten des Laufwerkbuchstabens für geschützte virtuelle Computer, für die ein Failover ausgeführt oder die zu Azure migriert werden](http://support.microsoft.com/kb/3031135)
* [Verwalten der Netzwerkbandbreitennutzung für den Schutz lokaler Elemente in Azure](https://support.microsoft.com/kb/3056159)
* [Azure Site Recovery: Der Fehler „Die Clusterressource wurde nicht gefunden.“ wird angezeigt, wenn Sie den Schutz für einen virtuellen Computer aktivieren möchten.](http://support.microsoft.com/kb/3010979)
* [Hyper-V Replica Troubleshooting Guide](http://social.technet.microsoft.com/wiki/contents/articles/21948.hyper-v-replica-troubleshooting-guide.aspx) (Hyper-V-Replikate – Leitfaden zur Problembehandlung)

## <a name="common-azure-site-recovery-errors-and-their-resolutions"></a>Häufige Fehler bei der automatischen Systemwiederherstellung und deren Lösungen
Im Folgenden sind häufige Fehler und die dazugehörigen Lösungen aufgeführt. Jeder Fehler wird auf einer separaten Wikiseite dokumentiert.

### <a name="general"></a>Allgemein
* <span style="color:green;">NEU</span> [Aufträge mit dem Fehler „Ein Vorgang wird ausgeführt.“ Fehler 505, 514, 532.](http://social.technet.microsoft.com/wiki/contents/articles/32190.azure-site-recovery-jobs-failing-with-error-an-operation-is-in-progress-error-505-514-532.aspx)
* <span style="color:green;">NEU</span> [Aufträge mit dem Fehler „Server ist nicht mit dem Internet verbunden“. Fehler 25018.](http://social.technet.microsoft.com/wiki/contents/articles/32192.azure-site-recovery-jobs-failing-with-error-server-isn-t-connected-to-the-internet-error-25018.aspx)

### <a name="setup"></a>Einrichtung
* [Der Virtual Machine Manager-Server kann aufgrund eines internen Fehlers nicht registriert werden. In der Auftragsansicht im Site Recovery-Portal finden Sie weitere Details zu diesem Fehler. Führen Sie das Setup erneut aus, um den Server zu registrieren.](http://social.technet.microsoft.com/wiki/contents/articles/25570.the-vmm-server-cannot-be-registered-due-to-an-internal-error-please-refer-to-the-jobs-view-in-the-site-recovery-portal-for-more-details-on-the-error-run-setup-again-to-register-the-server.aspx)
* [Mit dem Hyper-V-Wiederherstellungs-Manager-Tresor kann keine Verbindung hergestellt werden. Überprüfen Sie die Proxyeinstellungen oder versuchen Sie es später erneut.](http://social.technet.microsoft.com/wiki/contents/articles/25571.a-connection-cant-be-established-to-the-hyper-v-recovery-manager-vault-verify-the-proxy-settings-or-try-again-later.aspx)

### <a name="configuration"></a>Konfiguration
* [Die Schutzgruppe konnte nicht erstellt werden: Fehler beim Abrufen der Serverliste.](http://blogs.technet.com/b/somaning/archive/2015/08/12/unable-to-create-the-protection-group-in-azure-site-recovery-portal.aspx)
* [Der Hyper-V-Hostcluster enthält mindestens einen statischen Netzwerkadapter, oder keine verbundenen Adapter sind für die Verwendung von DHCP konfiguriert.](http://social.technet.microsoft.com/wiki/contents/articles/25498.hyper-v-host-cluster-contains-at-least-one-static-network-adapter-or-no-connected-adapters-are-configured-to-use-dhcp.aspx)
* [Virtual Machine Manager verfügt nicht über Berechtigungen zum Durchführen einer Aktion.](http://social.technet.microsoft.com/wiki/contents/articles/31110.vmm-does-not-have-permissions-to-complete-an-action.aspx)
* [Das Speicherkonto im Abonnement kann beim Konfigurieren des Schutzes nicht ausgewählt werden.](http://social.technet.microsoft.com/wiki/contents/articles/32027.can-t-select-the-storage-account-within-the-subscription-while-configuring-protection.aspx)

### <a name="protection"></a>Schutz
* <span style="color:green;">NEU</span> [„Schutz aktivieren“ führt zu dem Fehler „Der Schutz für den virtuellen Computer konnte nicht konfiguriert werden“. Fehler 60007, 40003.](http://social.technet.microsoft.com/wiki/contents/articles/32194.azure-site-recovery-enable-protection-failing-with-error-protection-couldn-t-be-configured-for-the-virtual-machine-error-60007-40003.aspx)
* <span style="color:green;">NEU</span> [„Schutz aktivieren“ führt zu dem Fehler „Der Schutz für den virtuellen Computer konnte nicht aktiviert werden“. Fehler 70094.](http://social.technet.microsoft.com/wiki/contents/articles/32195.azure-site-recovery-enable-protection-failing-with-error-protection-couldn-t-be-enabled-for-the-virtual-machine-error-70094.aspx)
* <span style="color:green;">NEU</span> [Livemigrationsfehler 23848: Der virtuelle Computer wird mithilfe von Typ „Live“ verschoben. Dadurch kann der Wiederherstellungsschutzstatus des virtuellen Computers unterbrochen werden.](http://social.technet.microsoft.com/wiki/contents/articles/32021.live-migration-error-23848-the-virtual-machine-is-going-to-be-moved-using-type-live-this-could-break-the-recovery-protection-status-of-the-virtual-machine.aspx)
* [Aktivieren des Schutzes fehlgeschlagen, da der Agent nicht auf dem Hostcomputer installiert ist.](http://social.technet.microsoft.com/wiki/contents/articles/31105.enable-protection-failed-since-agent-not-installed-on-host-machine.aspx)
* [Ein geeigneter Host für den replizierten virtuellen Computer wurde nicht gefunden. Grund: geringe Computeressourcen.](http://social.technet.microsoft.com/wiki/contents/articles/25501.a-suitable-host-for-the-replica-virtual-machine-can-t-be-found-due-to-low-compute-resources.aspx)
* [Ein geeigneter Host für den replizierten virtuellen Computer wurde nicht gefunden. Grund: Es ist kein logisches Netzwerk angeschlossen.](http://social.technet.microsoft.com/wiki/contents/articles/25502.a-suitable-host-for-the-replica-virtual-machine-can-t-be-found-due-to-no-logical-network-attached.aspx)
* [Keine Verbindung mit dem replizierten Hostcomputer – Verbindung konnte nicht hergestellt werden.](http://social.technet.microsoft.com/wiki/contents/articles/31106.cannot-connect-to-the-replica-host-machine-connection-could-not-be-established.aspx)

### <a name="recovery"></a>Wiederherstellen
* Der Hostvorgang kann von Virtual Machine Manager nicht abgeschlossen werden:
  * [Ein Failover auf den ausgewählten Wiederherstellungspunkt für den virtuellen Computer: Allgemeiner Zugriffsfehler.](http://social.technet.microsoft.com/wiki/contents/articles/25504.fail-over-to-the-selected-recovery-point-for-virtual-machine-general-access-denied-error.aspx)
  * [Hyper-V konnte kein Failover zum ausgewählten Wiederherstellungspunkt für virtuelle Computer ausführen: Der Vorgang wurde abgebrochen.  Versuchen Sie es mit einem neueren Wiederherstellungspunkt. (0x80004004).](http://social.technet.microsoft.com/wiki/contents/articles/25503.hyper-v-failed-to-fail-over-to-the-selected-recovery-point-for-virtual-machine-operation-aborted-try-a-more-recent-recovery-point-0x80004004.aspx)
  * Eine Verbindung mit dem Server konnte nicht hergestellt werden (0x00002EFD).
    * [Fehler von Hyper-V beim Aktivieren der umgekehrten Replikation für den virtuellen Computer.](http://social.technet.microsoft.com/wiki/contents/articles/25505.a-connection-with-the-server-could-not-be-established-0x00002efd-hyper-v-failed-to-enable-reverse-replication-for-virtual-machine.aspx)
    * [Fehler von Hyper-V beim Aktivieren der Replikation für den virtuellen Computer.](http://social.technet.microsoft.com/wiki/contents/articles/25506.a-connection-with-the-server-could-not-be-established-0x00002efd-hyper-v-failed-to-enable-replication-for-virtual-machine-virtual-machine.aspx)
  * [Failover für virtuellen Computer konnte nicht übergeben werden.](http://social.technet.microsoft.com/wiki/contents/articles/25508.could-not-commit-failover-for-virtual-machine.aspx)
* [Der Wiederherstellungsplan enthält virtuelle Computer, die für ein geplantes Failover nicht bereit sind.](http://social.technet.microsoft.com/wiki/contents/articles/25509.the-recovery-plan-contains-virtual-machines-which-are-not-ready-for-planned-failover.aspx)
* [Der virtuelle Computer ist nicht bereit für ein geplantes Failover.](http://social.technet.microsoft.com/wiki/contents/articles/25507.the-virtual-machine-isn-t-ready-for-planned-failover.aspx)
* [Der virtuelle Computer wird nicht ausgeführt und ist nicht ausgeschaltet.](http://social.technet.microsoft.com/wiki/contents/articles/25510.virtual-machine-is-not-running-and-is-not-powered-off.aspx)
* [Out-of-Band-Vorgang auf einem virtuellen Computer und Fehler bei Übergabe des Failovers.](http://social.technet.microsoft.com/wiki/contents/articles/25507.the-virtual-machine-isn-t-ready-for-planned-failover.aspx)
* Testfailover
  * [Failover konnte nicht initiiert werden, da das Testfailover ausgeführt wird.](http://social.technet.microsoft.com/wiki/contents/articles/31111.failover-could-not-be-initiated-since-test-failover-is-in-progress.aspx)
* <span style="color:green;">NEU</span> Beim Failover tritt aufgrund der Konfigurationseigenschaften der Netzwerksicherheitsgruppe, die dem virtuellen Computer oder dem Subnetz für den virtuellen Computer zugeordnet ist, ein Timeout mit der Meldung „PreFailoverWorkflow task WaitForScriptExecutionTaskTimeout“ (Timeout bei der PreFailoverWorkflow-Aufgabe „WaitForScriptExecutionTask“) auf. Ausführlichere Informationen finden Sie unter [PreFailoverWorkflow task WaitForScriptExecutionTaskTimeout](https://aka.ms/troubleshoot-nsg-issue-azure-site-recovery) (Timeout bei der PreFailoverWorkflow-Aufgabe „WaitForScriptExecutionTask“).

### <a name="configuration-server-process-server-master-target"></a>Konfigurationsserver, Prozessserver, Masterziel
* [Der ESXi-Host, auf dem der PS/CS als virtueller Computer gehostet wird, schlägt mit einem violetten Bildschirm fehl.](http://social.technet.microsoft.com/wiki/contents/articles/31107.vmware-esxi-host-experiences-a-purple-screen-of-death.aspx)

### <a name="remote-desktop-troubleshooting-after-failover"></a>Remotedesktop-Problembehandlung nach einem Failover
* Viele Kunden haben Probleme beim Herstellen einer Verbindung mit dem virtuellen Computer in Azure festgestellt, für den ein Failover ausgeführt wurde. [Stellen Sie anhand des Dokuments für die Problembehandlung eine RDP-Verbindung mit dem virtuellen Computer her.](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx)

#### <a name="adding-a-public-ip-on-a-resource-manager-virtual-machine"></a>Hinzufügen einer öffentlichen IP-Adresse für einen virtuellen Computer mit Ressourcen-Manager
Wenn die Schaltfläche **Verbinden** im Portal ausgeblendet ist und keine ExpressRoute- oder Site-to-Site-VPN-Verbindung mit Azure besteht, müssen Sie eine öffentliche IP-Adresse erstellen und dem virtuellen Computer zuweisen, damit Sie Remotedesktop/Shared Shell nutzen können. Anschließend können Sie der Netzwerkschnittstelle des virtuellen Computers eine öffentliche IP hinzufügen.  

![Hinzufügen einer öffentlichen IP-Adresse an der Netzwerkschnittstelle des virtuellen Computers, für den ein Failover ausgeführt wurde](media/site-recovery-monitoring-and-troubleshooting/createpublicip.gif)

