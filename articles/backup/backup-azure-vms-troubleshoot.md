---
title: Behandeln von Sicherheitsfehlern bei virtuellen Azure-Computern | Microsoft-Dokumentation
description: Fehlerbehandlung bei der Sicherung und Wiederherstellung virtueller Azure-Computer
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: 73214212-57a4-4b57-a2e2-eaf9d7fde67f
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: trinadhk;markgal;jpallavi;
ms.translationtype: Human Translation
ms.sourcegitcommit: bb794ba3b78881c967f0bb8687b1f70e5dd69c71
ms.openlocfilehash: 2c978cf85bd2890e58fafd0b73418133605e4922
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017


---
# <a name="troubleshoot-azure-virtual-machine-backup"></a>Problembehandlung bei der Sicherung virtueller Azure-Computer
> [!div class="op_single_selector"]
> * [Recovery Services-Tresor](backup-azure-vms-troubleshoot.md)
> * [Sicherungstresor](backup-azure-vms-troubleshoot-classic.md)
>
>

Sie können die Problembehandlung für Fehler, die beim Verwenden von Azure Backup auftreten, mit den Informationen in der unten angegebenen Tabelle durchführen.

## <a name="backup"></a>Sicherung
| Fehlerdetails | Problemumgehung |
| --- | --- |
| Der Vorgang konnte nicht ausgeführt werden, da der virtuelle Computer nicht mehr vorhanden ist. - Beenden des Schutzes für virtuelle Computer ohne Löschen der Sicherungsdaten. Weitere Informationen finden Sie unter „http://go.microsoft.com/fwlink/?LinkId=808124“. |Dies tritt auf, wenn der primäre virtuelle Computer gelöscht wurde, die Sicherungsrichtlinie jedoch weiterhin einen virtuellen Computer für die Sicherung sucht. Gehen Sie wie folgt vor, um diesen Fehler zu beheben:  <ol><li> Erstellen Sie den virtuellen Computer mit dem gleichen Namen und dem gleichen Ressourcengruppennamen [Clouddienstname] neu,<br>(ODER)</li><li> Beenden Sie den Schutz für den virtuellen Computer mit oder ohne die Sicherheitsdaten zu löschen. [Weitere Informationen](http://go.microsoft.com/fwlink/?LinkId=808124)</li></ol> |
| Keine Kommunikation mit dem VM-Agent zum Abrufen des Momentaufnahmestatus möglich. - Stellen Sie sicher, dass der virtuelle Computer über Internetzugriff verfügt. Aktualisieren Sie den VM-Agent gemäß den Informationen im Leitfaden zur Problembehandlung unter http://go.microsoft.com/fwlink/?LinkId=800034. |Dieser Fehler wird ausgelöst, wenn ein Problem mit dem VM-Agent besteht oder der Netzwerkzugriff auf die Azure-Infrastruktur blockiert ist. [Erfahren Sie mehr](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md) über das Debuggen von Problemen mit VM-Momentaufnahmen.<br> Wenn der VM-Agent keine Probleme verursacht, starten Sie den virtuellen Computer neu. Gelegentlich kann ein falscher Status des virtuellen Computers Probleme verursachen. Durch einen Neustart des virtuellen Computers wird der Status zurückgesetzt. |
| Der Vorgang kann nicht ausgeführt werden, da der VM-Agent nicht reagiert |Dieser Fehler wird ausgelöst, wenn ein Problem mit dem VM-Agent besteht oder der Netzwerkzugriff auf die Azure-Infrastruktur blockiert ist. Überprüfen Sie für Windows-VMs den VM-Agent-Dienststatus unter Dienste, und sehen Sie nach, ob der Agent in der Systemsteuerung unter Programme angezeigt wird. Versuchen Sie, das Programm aus der Systemsteuerung zu entfernen, und installieren Sie den Agenten erneut, wie [unten](#vm-agent) erwähnt. Lösen Sie nach der erneuten Installation des Agenten eine Ad-hoc-Sicherung zur Überprüfung aus. |
| Fehler beim Vorgang für die Recovery Services-Erweiterung. – Please make sure that latest virtual machine agent is present on the virtual machine and agent service is running. (Stellen Sie sicher, dass der aktuelle VM-Agent auf dem virtuellen Computer vorhanden ist und der Agent-Dienst ausgeführt wird.) Wiederholen Sie den Sicherungsvorgang. Sollte ein Fehler auftreten, wenden Sie sich an den Microsoft-Support. |Dieser Fehler wird ausgelöst, wenn ein VM-Agent nicht mehr aktuell ist. Informationen zum Aktualisieren des VM-Agents finden Sie unter „Aktualisieren des VM-Agents“. |
| Der virtuelle Computer ist nicht vorhanden. - Stellen Sie sicher, dass der virtuelle Computer vorhanden ist, oder wählen Sie einen anderen virtuellen Computer aus. |Dies tritt auf, wenn der primäre virtuelle Computer gelöscht wurde, die Sicherungsrichtlinie jedoch weiterhin einen virtuellen Computer für die Sicherung sucht. Gehen Sie wie folgt vor, um diesen Fehler zu beheben:  <ol><li> Erstellen Sie den virtuellen Computer mit dem gleichen Namen und dem gleichen Ressourcengruppennamen [Clouddienstname] neu,<br>(ODER)<br></li><li>Beenden Sie den Schutz für den virtuellen Computer ohne die Sicherheitsdaten zu löschen. [Weitere Informationen](http://go.microsoft.com/fwlink/?LinkId=808124)</li></ol> |
| Fehler bei der Ausführung des Befehls. - Für dieses Element wird ein anderer Vorgang ausgeführt. Warten Sie, bis der aktuelle Vorgang abgeschlossen ist, und wiederholen Sie anschließend Ihren Vorgang. |Auf dem virtuellen Computer wird ein bereits vorhandener Sicherungsauftrag ausgeführt, und ein neuer Auftrag kann erst gestartet werden, wenn die Ausführung des aktuellen Auftrags abgeschlossen ist. |
| Copying VHDs from the backup vault timed out - Please retry the operation in a few minutes. (Zeitüberschreitung beim Kopieren von VHDs aus dem Sicherungstresor. – Wiederholen Sie den Vorgang in einigen Minuten.) Wenden Sie sich an den Microsoft Support, wenn das Problem weiterhin besteht. | Dieser Fall tritt ein, wenn speicherseitig ein vorübergehender Fehler auftritt oder wenn der Sicherungsdienst nicht genügend IOPS von dem Speicherkonto erhält, das als Host für den virtuellen Computer fungiert, um Daten innerhalb des Zeitlimits an den Tresor zu übertragen. Vergewissern Sie sich, dass Sie beim Einrichten der Sicherung die [bewährten Methoden](backup-azure-vms-introduction.md#best-practices) angewendet haben. Verschieben Sie den virtuellen Computer ggf. in ein anderes, nicht geladenes Speicherkonto, und versuchen Sie erneut, eine Sicherung durchzuführen.|
| Interner Fehler bei der Sicherung – Versuchen Sie, den Vorgang nach einigen Minuten zu wiederholen. Wenden Sie sich an den Microsoft Support, wenn das Problem weiterhin besteht. |Sie können diese Fehlermeldung aus zwei Gründen erhalten:  <ol><li> Es liegt ein vorübergehendes Problem mit dem Zugriff auf den VM-Speicher vor. Überprüfen Sie den [Azure-Status](https://azure.microsoft.com/en-us/status/), um festzustellen, ob ein akutes Problem im Zusammenhang mit Compute/Speicher/Netzwerk in der Region vorliegt. Wiederholen Sie die Sicherung, nachdem das Problem behoben wurde. <li>Der ursprüngliche virtuelle Computer wurde gelöscht, sodass kein geeigneter Wiederherstellungspunkt vorhanden ist. Um die Sicherungsdaten für einen gelöschten virtuellen Computer beizubehalten und gleichzeitig die Sicherungsfehler zu beheben, heben Sie den Schutz für den virtuellen Computer auf und wählen die Option zum Beibehalten der Daten aus. Diese Aktion beendet die geplante Sicherung und die wiederkehrenden Fehlermeldungen. |
| Fehler bei der Installation der Azure Recovery Services-Erweiterung für das ausgewählte Element. – The VM agent is a prerequisite for the Azure Recovery Services Extension. (Der VM-Agent ist eine Voraussetzung für die Azure Recovery Services-Erweiterung.) Install the Azure VM agent and restart the registration operation. (Bitte installieren Sie den Azure-VM-Agent, und starten Sie den Registrierungsvorgang erneut.) |<ol> <li>Überprüfen Sie, ob der VM-Agent richtig installiert wurde. <li>Stellen Sie sicher, dass das Flag für die VM-Konfiguration richtig festgelegt wurde.</ol> [Erfahren Sie mehr](#validating-vm-agent-installation) über die VM-Agent-Installation und die dazugehörige Überprüfung. |
| Die Erweiterungsinstallation ist mit dem Fehler "COM+ konnte keine Daten mit dem Microsoft Distributed Transaction Coordinator austauschen" fehlgeschlagen. |Dies bedeutet i. d. R., dass der COM+-Dienst nicht ausgeführt wird. Wenden Sie sich an den Microsoft Support, um Hilfe beim Beheben dieses Problems zu erhalten. |
| Der Momentaufnahmevorgang ist mit folgendem VSS-Vorgangsfehler fehlgeschlagen: "Dieses Laufwerk ist durch die BitLocker-Laufwerkverschlüsselung gesperrt. You must unlock this drive from the Control Panel.“ (Das Laufwerk muss mithilfe der Systemsteuerung entsperrt werden.) |Deaktivieren Sie BitLocker für alle Laufwerke auf dem virtuellen Computer, und überprüfen Sie, ob der VSS-Fehler behoben ist. |
| Aufgrund des Zustands des virtuellen Computers sind keine Sicherungen möglich. |<ul><li>Überprüfen Sie, ob sich der virtuelle Computer in einem Übergangszustand zwischen „Ausgeführt“ und „Heruntergefahren“ befindet. Ist dies der Fall, warten Sie, bis der virtuelle Computer einen der Zustände erreicht hat, und lösen Sie anschließend erneut eine Sicherung aus. <li> Schließen Sie im Falle eines virtuellen Linux-Computers mit dem Kernel-Modul [Security-Enhanced Linux] den Linux-Agent-Pfad (_/var/lib/waagent_) aus der Sicherheitsrichtlinie aus, damit die Sicherungserweiterung installiert wird.  |
| Der virtuelle Azure-Computer wurde nicht gefunden. |Dies tritt auf, wenn der primäre virtuelle Computer gelöscht wurde, die Sicherungsrichtlinie jedoch weiterhin einen virtuellen Computer für die Sicherung sucht. Gehen Sie wie folgt vor, um diesen Fehler zu beheben:  <ol><li>Erstellen Sie den virtuellen Computer mit dem gleichen Namen und dem gleichen Ressourcengruppennamen [Clouddienstname] neu, <br>(ODER) <li> Deaktivieren Sie den Schutz für diesen virtuellen Computer, damit keine Sicherungsaufträge erstellt werden. </ol> |
| Der virtuelle Computer-Agent ist auf dem virtuellen Computer nicht vorhanden. – Please install any prerequisite and the VM agent, and then restart the operation. (Bitte installieren Sie alle erforderlichen Komponenten und den VM-Agent, und starten Sie den Vorgang erneut.) |[Erfahren Sie mehr](#vm-agent) über die VM-Agent-Installation und die dazugehörige Überprüfung. |
| Snapshot operation failed due to VSS Writers in bad state (Fehler beim Momentaufnahmevorgang aufgrund eines ungültigen Status der VSS-Writer) |Sie müssen die Writer des Volumeschattenkopie-Diensts (Volume Shadow Copy Service, VSS), die einen ungültigen Status aufweisen, neu starten. Führen Sie hierzu an einer Eingabeaufforderung mit erhöhten Rechten den Befehl _vssadmin list writers_ aus. Die Ausgabe enthält alle VSS-Writer und ihren Status. Führen Sie für jeden VSS-Writer, dessen Status nicht „[1] Stable“ lautet, einen Neustart durch, indem Sie die folgenden Befehle an einer Eingabeaufforderung mit erhöhten Rechten ausführen:<br> _net stop serviceName_ <br> _net start serviceName_|
| Snapshot operation failed due to a parsing failure of the configuration (Fehler beim Momentaufnahmevorgang aufgrund eines Fehlers beim Analysieren der Konfiguration) |Der Fehler tritt auf, weil sich die Berechtigungen für das Verzeichnis „MachineKeys“ geändert haben: _%systemdrive%\programdata\microsoft\crypto\rsa\machinekeys_ <br>Führen Sie den folgenden Befehl aus, und stellen Sie sicher, dass für das Verzeichnis „MachineKeys“ die Standardberechtigungen gelten:<br>_icacls %systemdrive%\programdata\microsoft\crypto\rsa\machinekeys_ <br><br> Die Standardberechtigungen lauten:<br>Jeder: (R, W) <br>VORDEFINIERT\Administratoren: (F)<br><br>Falls für das Verzeichnis „MachineKeys“ andere Berechtigungen als die Standardberechtigungen festgelegt sind, sollten Sie die unten angegebenen Schritte ausführen, um die Berechtigungen zu korrigieren, das Zertifikat zu löschen und den Sicherungsvorgang auszulösen.<ol><li>Korrigieren Sie die Berechtigungen für das Verzeichnis „MachineKeys“.<br>Verwenden Sie die Explorer-Sicherheitseigenschaften und die erweiterten Sicherheitseinstellungen für das Verzeichnis, um die Berechtigungen wieder auf die Standardwerte zurückzusetzen, überflüssige (nicht standardmäßige) Benutzerobjekte aus dem Verzeichnis zu entfernen und sicherzustellen, dass für die Berechtigungen für „Jeder“ der folgende Zugriff besteht:<br>- Ordner auflisten/Daten lesen <br>- Attribute lesen <br>- Erweiterte Attribute lesen <br>- Dateien erstellen/Daten schreiben <br>- Ordner erstellen/Daten anhängen<br>- Attribute schreiben<br>- Erweiterte Attribute schreiben<br>- Leseberechtigungen<br><br><li>Löschen Sie alle Zertifikat mit dem Feld „Issued To“ = „Windows Azure Service Management for Extensions“ oder „Windows Azure CRP Certificate Generator“.<ul><li>[Öffnen Sie die Konsole „Zertifikate (lokaler Computer)“](https://msdn.microsoft.com/library/ms788967(v=vs.110).aspx)<li>Löschen Sie alle Zertifikate (unter „Eigene Zertifikate“ > „Zertifikate“) mit dem Feld „Issued To“ = „Windows Azure Service Management for Extensions“ oder „Windows Azure CRP Certificate Generator“.</ul><li>Lösen Sie die VM-Sicherung aus. </ol>|
| Bei der Überprüfung ist ein Fehler aufgetreten, da der virtuelle Computer nur per BEK verschlüsselt wurde. Sicherungen können nur für virtuelle Computer aktiviert werden, die per BEK und KEK verschlüsselt wurden. |Der virtuelle Computer muss sowohl per BitLocker-Verschlüsselungsschlüssel als auch per Schlüsselverschlüsselungsschlüssel verschlüsselt werden. Anschließend sollte die Sicherung aktiviert werden. |
| Der Azure Backup-Dienst weist nicht genügend Berechtigungen für den Schlüsseltresor auf, um verschlüsselte virtuelle Computer zu sichern. |Erteilen Sie dem Backup-Dienst mithilfe von PowerShell die erforderlichen Berechtigungen, indem Sie die Schritte des Abschnitts **Aktivieren der Sicherung** aus der [PowerShell-Dokumentation](backup-azure-vms-automation.md) ausführen. |
|Fehler bei der Momentaufnahmen-Erweiterungsinstallation mit dem Fehler „COM+ konnte keine Daten mit dem Microsoft Distributed Transaction Coordinator austauschen“. | Versuchen Sie, den Windows-Dienst COM+-Systemanwendung zu starten (über eine Eingabeaufforderung mit erhöhten Rechten – _net start COMSysApp_). <br>Wenn der Dienst beim Starten fehlschlägt, führen Sie folgende Schritte aus:<ol><li> Überprüfen Sie, ob das Anmeldekonto des Diensts Distributed Transaction Coordinator „Netzwerkdienst“ lautet. Wenn dies nicht der Fall ist, ändern Sie es in „Netzwerkdienst“, starten Sie den Dienst neu, und versuchen Sie dann, den Dienst COM+-Systemanwendung zu starten.<li>Wenn er immer noch nicht gestartet werden kann, deinstallieren/installieren Sie den Dienst Distributed Transaction Coordinator mithilfe der folgenden Schritte:<br> – Beenden Sie den Dienst MS DTC.<br> – Öffnen Sie eine Eingabeaufforderung (cmd). <br> – Führen Sie den Befehl „msdtc -uninstall“ aus. <br> – Führen Sie den Befehl „msdtc -install“ aus. <br> – Starten Sie den Dienst MS DTC.<li>Starten Sie den Windows-Dienst COM+-Systemanwendung, und nachdem er gestartet wurde, lösen Sie im Portal die Sicherung aus.</ol> |
|  Fehler bei Momentaufnahmevorgang aufgrund eines COM+-Fehlers | Es wird empfohlen, den Windows-Dienst „COM+-Systemanwendung“ neu zu starten (über eine Eingabeaufforderung mit erhöhten Rechten – _net start COMSysApp_). Wenn das Problem weiterhin besteht, starten Sie den virtuellen Computer neu. Wenn der Neustart des VM nicht hilfreich war, versuchen Sie das [Entfernen der VMSnapshot-Erweiterung](https://docs.microsoft.com/en-us/azure/backup/backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout#cause-3-the-backup-extension-fails-to-update-or-load), und lösen Sie die Sicherung manuell aus |
| Fehler beim Einfrieren mindestens eines Bereitstellungspunkts der VM zum Erfassen einer dateisystemkonsistenten Momentaufnahme. | <ol><li>Überprüfen Sie mit dem Befehl _'tune2fs'_ den Dateisystemstatus aller bereitgestellten Geräte.<br> Beispiel: tune2fs -l /dev/sdb1 \| grep "Filesystem state" <li>Heben Sie mit dem Befehl _'umount'_ die Bereitstellung der Geräte auf, deren Dateisystemstatus nicht ordnungsgemäß ist. <li> Überprüfen Sie mit dem Befehl _'fsck'_ die Konsistenz des Dateisystems auf diesen Geräten. <li> Stellen Sie die Geräte erneut bereit, und versuchen Sie, die Sicherung auszuführen.</ol> |
| Fehler bei Momentaufnahmevorgang aufgrund eines Fehlers bei der Erstellung eines sicheren Netzwerkkommunikationskanals. | <ol><Li> Öffnen Sie den Registrierungs-Editor, indem Sie im Modus mit erhöhten Rechten „regedit.exe“ ausführen. <li> Identifizieren Sie alle im System vorhandenen Versionen von .NET Framework. Sie werden unter der Hierarchie des Registrierungsschlüssels „HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft“ aufgeführt. <li> Führen Sie für jede im Registrierungsschlüssel aufgeführte .NET Framework-Version den folgenden Schlüssel hinzu: <br> "SchUseStrongCrypto"=dword:00000001 </ol>|
| Fehler bei Momentaufnahmevorgang aufgrund eines Fehlers bei der Installation von Visual C++ Redistributable für Visual Studio 2012. | Navigieren Sie zu „C:\Packages\Plugins\Microsoft.Azure.RecoveryServices.VMSnapshot\agentVersion“, und installieren Sie „vcredist2012_x64“. Stellen Sie sicher, dass der richtige Registrierungsschlüsselwert zum Zulassen dieser Dienstinstallation festgelegt ist, d.h., der Wert des Registrierungsschlüssels _HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Msiserver_ muss auf 3 und nicht auf 4 festgelegt sein. Wenn immer noch Probleme bei der Installation auftreten, starten Sie den Installationsdienst neu, indem Sie an einer Eingabeaufforderung mit erhöhten Rechten _MSIEXEC /UNREGISTER_ und dann _MSIEXEC /REGISTER_ ausführen.  |


## <a name="jobs"></a>Aufträge
| Fehlerdetails | Problemumgehung |
| --- | --- |
| Der Abbruch wird für diesen Auftragstyp nicht unterstützt. Warten Sie, bis der Auftrag abgeschlossen ist. |Keine |
| Der Auftrag kann in diesem Status nicht abgebrochen werden. Warten Sie, bis der Auftrag abgeschlossen ist. <br>OR<br> The selected job is not in a cancelable state (Der ausgewählte Auftrag kann in diesem Status nicht abgebrochen werden) – Bitte warten Sie, bis der Auftrag abgeschlossen ist. |Der Auftrag wurde mit hoher Wahrscheinlichkeit bereits fast ganz abgeschlossen. Warten Sie, bis der Auftrag vollständig abgeschlossen wurde.|
| Das Abbrechen des Auftrags ist nicht möglich, da sich dieser nicht in Bearbeitung befindet. Der Abbruch wird nur für Aufträge in Bearbeitung unterstützt. Wiederholen Sie den Abbruchversuch für einen Auftrag, der sich in Bearbeitung befindet. |Dies wird durch einen vorübergehenden Zustand verursacht. Warten Sie eine Minute, und wiederholen Sie den Abbruchvorgang. |
| Fehler beim Abbrechen des Auftrags – Warten Sie, bis der Auftrag abgeschlossen ist. |Keine |

## <a name="restore"></a>Wiederherstellen
| Fehlerdetails | Problemumgehung |
| --- | --- |
| Cloudinterner Fehler bei der Wiederherstellung |<ol><li>Der Clouddienst, in dem Sie die Wiederherstellung durchführen möchten, ist mit DNS-Einstellungen konfiguriert. Prüfen Sie  <br>$deployment = Get-AzureDeployment -ServiceName "ServiceName" -Slot "Production"     Get-AzureDns -DnsSettings $deployment.DnsSettings<br>Wenn eine Adresse konfiguriert wurde, bedeutet dies, dass DNS-Einstellungen konfiguriert sind.<br> <li>Der Clouddienst, in dem Sie die Wiederherstellung ausführen möchten, ist mit ReservedIP konfiguriert, und vorhandene virtuelle Computer im Clouddienst haben den Status „Beendet“.<br>Sie können mithilfe des folgenden PowerShell-Cmdlets überprüfen, ob ein Clouddienst eine IP-Adresse reserviert hat:<br>$deployment = Get-AzureDeployment -ServiceName "servicename" -Slot "Production" $dep.ReservedIPName <br><li>Sie versuchen, einen virtuellen Computer mit den folgenden speziellen Netzwerkkonfigurationen im selben Clouddienst wiederherzustellen. <br>– Virtuelle Computer unter Load Balancer-Konfiguration (intern und extern)<br>– Virtuelle Computer mit mehreren reservierten IP-Adressen<br>– Virtuelle Computer mit mehreren NICs<br>Wählen Sie einen neuen Clouddienst auf der Benutzeroberfläche aus, oder lesen Sie die [Überlegungen zu Wiederherstellungen](backup-azure-arm-restore-vms.md#restoring-vms-with-special-network-configurations) für virtuelle Computer mit speziellen Netzwerkkonfigurationen</ol> |
| Der ausgewählte DNS-Name ist bereits vergeben. Geben Sie einen anderen DNS-Namen an, und versuchen Sie es erneut. |Der DNS-Name bezieht sich hier auf den Clouddienstnamen (der i. d. R. auf ".cloudapp.net" endet). Dieser muss eindeutig sein. Wenn der vorliegende Fehler auftritt, müssen Sie während der Wiederherstellung einen anderen Namen für den virtuellen Computer auswählen. <br><br> Dieser Fehler wird nur Benutzern des Azure-Portals angezeigt. Der Wiederherstellungsvorgang über PowerShell ist erfolgreich, da nur die Datenträger wiederhergestellt werden und kein virtueller Computer erstellt wird. Der Fehler tritt auf, wenn der virtuelle Computer nach Abschluss des Wiederherstellungsvorgangs für die Datenträger explizit von Ihnen erstellt wird. |
| Die angegebene Konfiguration des virtuellen Netzwerks ist nicht korrekt. Geben Sie eine andere Konfiguration des virtuellen Netzwerks an, und versuchen Sie es erneut. |Keine |
| Der angegebene Clouddienst verwendet eine reservierte IP, die nicht mit der Konfiguration des virtuellen Computers übereinstimmt, der wiederhergestellt werden soll. Geben Sie einen anderen Clouddienst an, der keine reservierte IP verwendet, oder wählen Sie einen anderen Wiederherstellungspunkt aus. |Keine |
| Der Clouddienst hat den Grenzwert für die Anzahl der Eingabeendpunkte erreicht. Wiederholen Sie den Vorgang, indem Sie einen anderen Clouddienst angeben oder einen vorhandenen Endpunkt verwenden. |Keine |
| Der Sicherungstresor und das Zielspeicherkonto befinden sich in unterschiedlichen Regionen. Stellen Sie sicher, dass sich das für den Wiederherstellungsvorgang angegebene Speicherkonto in derselben Azure-Region wie der Sicherungstresor befindet. |Keine |
| Das für den Wiederherstellungsvorgang angegebene Speicherkonto wird nicht unterstützt. Es werden nur Basic-/Standard-Speicherkonten mit lokal redundanten oder georedundanten Replikationseinstellungen unterstützt. Wählen Sie ein unterstütztes Speicherkonto aus. |Keine |
| Der Typ des für den Wiederherstellungsvorgang angegebenen Speicherkontos ist nicht online. Stellen Sie sicher, dass das für den Wiederherstellungsvorgang angegebene Speicherkonto online ist. |Dies kann durch einen vorübergehenden Fehler in Azure Storage oder einen Ausfall verursacht werden. Wählen Sie ein anderes Speicherkonto aus. |
| Das Kontingent für Ressourcengruppen wurde erreicht. Löschen Sie einige Ressourcengruppen aus dem Azure-Portal, oder wenden Sie sich an den Azure-Support, um die Grenzwerte zu erhöhen. |Keine |
| Das ausgewählte Subnetz besteht nicht. Wählen Sie ein vorhandenes Subnetz aus. |Keine |
| Der Speicherdienst ist nicht zum Zugreifen auf Ressourcen in Ihrem Abonnement autorisiert. |Stellen Sie zum Beheben dieses Problems zunächst Datenträger gemäß den Schritten des Abschnitts **Wiederherstellen von gesicherten Datenträgern** (unter [Auswählen einer Konfiguration für die VM-Wiederherstellung](backup-azure-arm-restore-vms.md#choosing-a-vm-restore-configuration)) wieder her. Führen Sie anschließend die unter [Erstellen eines virtuellen Computers aus wiederhergestellten Datenträgern](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) beschriebenen PowerShell-Schritte aus, um auf der Grundlage der wiederhergestellten Datenträger einen vollständigen virtuellen Computer zu erstellen. |

## <a name="backup-or-restore-taking-time"></a>Lang andauernde Sicherung oder Wiederherstellung
Wenn Sie bemerken, dass Ihre Sicherung (>12 Stunden) oder Ihre Wiederherstellung (>6 Stunden) lange dauert:
* Machen Sie sich mit den [Faktoren, welche die Sicherungsdauer beeinflussen](backup-azure-vms-introduction.md#total-vm-backup-time), und den [Faktoren, die die Wiederherstellungsdauer beeinflussen](backup-azure-vms-introduction.md#total-restore-time), vertraut.
* Stellen Sie sicher, dass Sie die [bewährten Methoden für die Sicherung](backup-azure-vms-introduction.md#best-practices) befolgen. 

## <a name="vm-agent"></a>VM-Agent
### <a name="setting-up-the-vm-agent"></a>Einrichten des VM-Agents
Normalerweise ist der VM-Agent auf virtuellen Computern, die über den Azure-Katalog erstellt werden, bereits vorhanden. Auf virtuellen Computern, die aus lokalen Rechenzentren migriert werden, ist der VM-Agent aber nicht installiert. Für diese virtuellen Computer muss der VM-Agent explizit installiert werden.

Für virtuelle Windows-Computer:

* Laden Sie den [Agent-MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409)herunter, und installieren Sie ihn. Zum Durchführen der Installation benötigen Sie Administratorberechtigungen.
* Für klassische virtuelle Computer [aktualisieren Sie die VM-Eigenschaft](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx), um anzugeben, dass der Agent installiert wurde. Dieser Schritt ist nicht für virtuelle Resource Manager-Computer erforderlich.

Für virtuelle Linux-Computer:

* Installieren Sie den neuesten Agent aus dem Repository der Distribution. Es wird **dringend empfohlen**, den Agent nur über das Repository der Distribution zu installieren. Ausführliche Informationen zum Paketnamen finden Sie unter [Linux agent repository](https://github.com/Azure/WALinuxAgent) (Repository für Linux-Agent). 
* Für klassische virtuelle Computer [aktualisieren Sie die VM-Eigenschaft](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx), um anzugeben, dass der Agent installiert wurde. Dieser Schritt ist nicht für virtuelle Resource Manager-Computer erforderlich.

### <a name="updating-the-vm-agent"></a>Aktualisieren des VM-Agents
Für virtuelle Windows-Computer:

* Das Aktualisieren des VM-Agents ist so einfach wie das Neuinstallieren der [Binärdateien für den VM-Agent](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Allerdings müssen Sie sicherstellen, dass kein Sicherungsvorgang ausgeführt wird, während der VM-Agent aktualisiert wird.

Für virtuelle Linux-Computer:

* Folgen Sie den Anweisungen unter [Aktualisieren des Azure Linux-Agents auf einem virtuellen Computer](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
Es wird **dringend empfohlen**, den Agent nur über das Repository für die Verteilung zu aktualisieren. Wir raten davon ab, den Agent-Code direkt von GitHub herunterzuladen und die Aktualisierung durchzuführen. Wenn der aktuelle Agent nicht für die Verteilung verfügbar ist, können Sie beim für die Verteilung zuständigen Support eine Anleitung zur Installation des aktuellen Agents erhalten. Sie können sich im GitHub-Repository über den aktuellen [Windows Azure Linux-Agent](https://github.com/Azure/WALinuxAgent/releases) informieren.

### <a name="validating-vm-agent-installation"></a>Überprüfen der VM-Agent-Installation
So überprüfen Sie die Version des VM-Agents auf virtuellen Windows-Computern

1. Melden Sie sich beim virtuellen Azure-Computer an, und navigieren Sie zum Ordner *C:\WindowsAzure\Packages*. Dieser Ordner enthält die Datei "WaAppAgent.exe".
2. Klicken Sie mit der rechten Maustaste auf die Datei, wechseln Sie zu **Eigenschaften**, und wählen Sie dann die Registerkarte **Details** aus. Im Feld mit der Produktversion sollte 2.6.1198.718 oder eine höhere Version angegeben sein.

## <a name="troubleshoot-vm-snapshot-issues"></a>Problembehandlung für Probleme mit VM-Momentaufnahmen
Bei der VM-Sicherung werden Momentaufnahmenbefehle an den zugrunde liegenden Speicher ausgegeben. Wenn Sie bei der Ausführung von Momentaufnahmenaufgaben keinen Zugriff auf Speicherung oder Verzögerung haben, kann es beim Sicherungsvorgang unter Umständen zu Fehlern kommen. Es kann die unten angegebenen Gründe haben, wenn für Momentaufnahmenaufgaben ein Fehler auftritt.

1. Netzwerkzugriff auf Storage wird per NSG blockiert<br>
    Lesen Sie weitere Informationen zum [Aktivieren des Netzwerkzugriffs](backup-azure-vms-prepare.md#network-connectivity) auf Storage, indem IPs auf eine Whitelist gesetzt werden, oder per Proxyserver.
2. Virtuelle Computer mit konfigurierter SQL Server-Sicherung können Momentaufnahmeaufgaben verzögern <br>
   Standardmäßig wird bei der Sicherung virtueller Computer eine vollständige VSS-Sicherung auf virtuellen Windows-Computern ausgeführt. Auf VMs, auf denen SQL-Server ausgeführt werden, kann dies – bei konfigurierter SQL Server-Sicherung – eine Verzögerung bei der Ausführung von Momentaufnahmen zur Folge haben. Legen Sie den folgenden Registrierungsschlüssel fest, wenn bei Ihnen aufgrund von Problemen mit Momentaufnahmen Sicherungsfehler auftreten.

   ```
   [HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
   "USEVSSCOPYBACKUP"="TRUE"
   ```
3. Der VM-Status wird falsch gemeldet, da die VM im RDP heruntergefahren ist.  <br>
   Wenn Sie den virtuellen Computer im RDP heruntergefahren haben, sollten Sie im Portal überprüfen, ob der VM-Status richtig wiedergegeben wird. Falls nicht, beenden Sie die VM im Portal, indem Sie im VM-Dashboard die Option „Herunterfahren“ verwenden.
4. Wenn mehr als vier VMs gemeinsam den gleichen Clouddienst nutzen, konfigurieren Sie mehrere Sicherungsrichtlinien zum Bereitstellen der Sicherungszeiten, sodass höchstens vier VM-Sicherungen gleichzeitig gestartet werden. Versuchen Sie, die Sicherungsstartzeiten für verschiedene Richtlinien mit einer Stunde Abstand zu verteilen.
5. Der virtuelle Computer wird bei hoher CPU-/Arbeitsspeicherauslastung ausgeführt.<br>
   Wenn der virtuelle Computer bei hoher CPU-Auslastung (&gt; 90 Prozent) oder Arbeitsspeicherauslastung ausgeführt wird, wird die Momentaufnahmenaufgabe in die Warteschlange eingereiht und verzögert und erreicht schließlich die Zeitüberschreitung. Versuchen Sie es in diesem Fall mit bedarfsgesteuerten Sicherungen.

<br>

## <a name="networking"></a>Netzwerk
Wie bei allen Erweiterungen ist für die Backup-Erweiterung der Zugriff auf das öffentliche Internet erforderlich, damit sie funktioniert. Wenn kein Zugriff auf das öffentliche Internet besteht, kann dies zu unterschiedlichen Ergebnissen führen:

* Beim Installieren der Erweiterung kann ein Fehler auftreten.
* Bei den Sicherungsvorgängen (z. B. Datenträger-Momentaufnahme) kann ein Fehler auftreten.
* Beim Anzeigen des Status für den Sicherungsvorgang kann ein Fehler auftreten.

Die Notwendigkeit zur Auflösung von öffentlichen Internetadressen wird [hier](http://blogs.msdn.com/b/mast/archive/2014/06/18/azure-vm-provisioning-stuck-on-quot-installing-extensions-on-virtual-machine-quot.aspx)beschrieben. Sie müssen die DNS-Konfigurationen für das VNET überprüfen und sicherstellen, dass die Azure-URIs aufgelöst werden können.

Nachdem die Namensauflösung richtig eingerichtet wurde, muss auch der Zugriff auf die Azure-IP-Adressen bereitgestellt werden. Führen Sie einen der folgenden Schritte aus, um die Blockierung des Zugriffs auf die Azure-Infrastruktur aufzuheben:

1. Aufnehmen der IP-Bereiche des Azure-Rechenzentrums in eine Positivliste
   * Beschaffen Sie sich die Liste mit den [IP-Adressen des Azure-Rechenzentrums](https://www.microsoft.com/download/details.aspx?id=41653) , die auf der Positivliste stehen sollen.
   * Heben Sie Blockierung für die IP-Adressen mit dem Cmdlet [New-NetRoute](https://technet.microsoft.com/library/hh826148.aspx) auf. Führen Sie dieses Cmdlet auf dem virtuellen Azure-Computer in einem PowerShell-Fenster mit erhöhten Rechten aus (als Administrator).
   * Fügen Sie der NSG (falls in der Organisation vorhanden) Regeln für den Zugriff auf die IP-Adressen hinzu.
2. Erstellen eines Pfads für HTTP-Datenverkehr
   * Wenn Netzwerkeinschränkung bestehen (beispielsweise eine Netzwerksicherheitsgruppe) sollte ein HTTP-Proxyserver zum Weiterleiten des Datenverkehrs bereitgestellt werden. Schritte zum Bereitstellen eines HTTP-Proxy-Servers finden Sie [hier](backup-azure-vms-prepare.md#network-connectivity).
   * Fügen Sie der NSG (falls in der Organisation vorhanden) Regeln für den Zugriff auf das Internet über den HTTP-Proxy hinzu.

> [!NOTE]
> Für die VM-Sicherung mithilfe von IaaS muss im Gastbetriebssystem die DHCP-Option aktiviert sein.  Wenn Sie eine statische private IP-Adresse benötigen, sollten Sie diese über die Plattform konfigurieren. Die DHCP-Option innerhalb des virtuellen Computers sollte aktiviert bleiben.
> Lesen Sie weitere Informationen zum [Festlegen einer statischen internen privaten IP-Adresse](../virtual-network/virtual-networks-reserved-private-ip.md).
>
>

