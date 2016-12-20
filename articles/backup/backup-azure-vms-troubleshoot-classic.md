---
title: Fehlerbehandlung bei der Sicherung virtueller Azure-Computer | Microsoft Docs
description: Fehlerbehandlung bei der Sicherung und Wiederherstellung virtueller Azure-Computer
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: 117201fb-c0cd-4be4-b47f-abd88fe914cf
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: trinadhk;jimpark;
translationtype: Human Translation
ms.sourcegitcommit: 63cf1a5476a205da2f804fb2f408f4d35860835f
ms.openlocfilehash: e2c22d2833a7905ba6001178be8709ec81c26000


---
# <a name="troubleshoot-azure-virtual-machine-backup"></a>Problembehandlung bei der Sicherung virtueller Azure-Computer
> [!div class="op_single_selector"]
> * [Recovery Services-Tresor](backup-azure-vms-troubleshoot.md)
> * [Sicherungstresor](backup-azure-vms-troubleshoot-classic.md)
>
>

Sie können die Problembehandlung für Fehler, die beim Verwenden von Azure Backup auftreten, mit den Informationen in der unten angegebenen Tabelle durchführen.

## <a name="discovery"></a>Ermittlung
| Sicherungsvorgang | Fehlerdetails | Problemumgehung |
| --- | --- | --- |
| Ermittlung |Fehler beim Entdecken neuer Elemente – Microsoft Azure Backup hat einen internen Fehler festgestellt. Warten Sie einige Minuten, und versuchen Sie es dann erneut. |Wiederholen Sie den Ermittlungsvorgang nach 15 Minuten. |
| Ermittlung |Fehler beim Entdecken neuer Elemente – Ein anderer Ermittlungsvorgang wird bereits ausgeführt. Bitte warten Sie, bis der aktuelle Ermittlungsvorgang abgeschlossen ist. |Keine |

## <a name="register"></a>Registrieren
| Sicherungsvorgang | Fehlerdetails | Problemumgehung |
| --- | --- | --- |
| Registrieren |Für die Anzahl von Datenträgern für Daten, die dem virtuellen Computer zugeordnet sind, wurde die unterstützte Obergrenze überschritten. Trennen Sie einige Datenträger für Daten von diesem virtuellen Computer, und wiederholen Sie den Vorgang. Azure Backup unterstützt bis zu 16 Datenträger für Daten, die für die Sicherung an einen virtuellen Azure-Computer angeschlossen sind. |Keine |
| Registrieren |Für Microsoft Azure Backup ist ein interner Fehler aufgetreten. Warten Sie einige Minuten, und wiederholen Sie anschließend den Vorgang. Wenden Sie sich an den Microsoft Support, wenn das Problem weiterhin besteht. |Dieser Fehler kann aufgrund einer der folgenden nicht unterstützten Konfigurationen eines virtuellen Computers für Premium LRS auftreten. <br>  Storage Premium-VMs können mithilfe des Recovery Services-Tresors gesichert werden. [Weitere Informationen](backup-introduction-to-azure-backup.md#using-premium-storage-vms-with-azure-backup) |
| Registrieren |Fehler bei der Registrierung aufgrund einer Zeitüberschreitung beim Installieren des Agents |Überprüfen Sie, ob die Betriebssystemversion des virtuellen Computers unterstützt wird. |
| Registrieren |Fehler beim Ausführen des Befehls – Für dieses Element wird ein anderer Vorgang ausgeführt. Warten Sie, bis der aktuelle Vorgang abgeschlossen ist. |Keine |
| Registrieren |Virtuelle Computer mit virtuellen Festplatten, die in Storage Premium gespeichert sind, werden für die Sicherung nicht unterstützt. |Keine |
| Registrieren |Der Agent für virtuelle Computer ist nicht auf dem virtuellen Computer vorhanden. Installieren Sie den VM-Agent (erforderliche Voraussetzung), und starten Sie den Vorgang erneut. |[Erfahren Sie mehr](#vm-agent) über die VM-Agent-Installation und die dazugehörige Überprüfung. |

## <a name="backup"></a>Sicherung
| Sicherungsvorgang | Fehlerdetails | Problemumgehung |
| --- | --- | --- |
| Sicherung |Keine Kommunikation mit dem VM-Agent zum Abrufen des Momentaufnahmestatus möglich. Zeitüberschreitung bei Momentaufnahme für VM-Unteraufgabe. – Weitere Informationen zur Lösung dieses Problems finden Sie im Handbuch zur Problembehandlung. |Dieser Fehler wird ausgelöst, wenn ein Problem mit dem VM-Agent besteht oder der Netzwerkzugriff auf die Azure-Infrastruktur blockiert ist. Erfahren Sie mehr über das [Debuggen von Problemen mit VM-Momentaufnahmen](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md). <br> Wenn der VM-Agent keine Probleme verursacht, starten Sie den virtuellen Computer neu. Gelegentlich kann ein falscher Status des virtuellen Computers Probleme verursachen. Durch einen Neustart des virtuellen Computers wird der Status zurückgesetzt. |
| Sicherung |Interner Fehler bei der Sicherung – Versuchen Sie, den Vorgang nach einigen Minuten zu wiederholen. Wenden Sie sich an den Microsoft Support, wenn das Problem weiterhin besteht. |Überprüfen Sie, ob ein vorübergehendes Problem beim Zugriff auf den VM-Speicher vorliegt. Überprüfen Sie den [Azure-Status](https://azure.microsoft.com/en-us/status/), um festzustellen, ob ein akutes Problem im Zusammenhang mit Compute/Speicher/Netzwerk in der Region vorliegt. Versuchen Sie nach der Behebung des Problems erneut, das Backup durchzuführen. |
| Sicherung |Der Vorgang konnte nicht ausgeführt werden, weil die VM nicht mehr vorhanden ist. |Die Sicherung kann nicht ausgeführt werden, weil die für die Sicherung konfigurierte VM gelöscht wurde. Beenden Sie weitere Sicherungen, indem Sie die Ansicht mit geschützten Elementen aufrufen, das geschützte Element auswählen und auf „Schutz beenden“ klicken. Sie können Daten beibehalten, indem Sie die Option „Sicherung beibehalten“ wählen. Sie können den Schutz für diesen virtuellen Computer später fortsetzen, indem Sie in der Ansicht „Registrierte Elemente“ auf „Schutz konfigurieren“ klicken. |
| Sicherung |Fehler beim Installieren der Azure Recovery Services-Erweiterung auf dem ausgewählten Element – Der VM-Agent ist eine Voraussetzung für die Azure Recovery Services-Erweiterung. Installieren Sie den Azure-VM-Agent, und starten Sie den Registrierungsvorgang erneut. |<ol> <li>Überprüfen Sie, ob der VM-Agent richtig installiert wurde. <li>Stellen Sie sicher, dass das Flag für die VM-Konfiguration richtig festgelegt wurde.</ol> [Erfahren Sie mehr](#validating-vm-agent-installation) über die VM-Agent-Installation und die dazugehörige Überprüfung. |
| Sicherung |Fehler bei der Ausführung des Befehls – Für dieses Element wird ein anderer Vorgang ausgeführt. Warten Sie, bis der aktuelle Vorgang abgeschlossen ist, und wiederholen Sie anschließend Ihren Vorgang. |Ein vorhandener Sicherungs- oder Wiederherstellungsauftrag wird für den virtuellen Computer ausgeführt. Ein neuer Auftrag kann erst gestartet werden, wenn die Ausführung des aktuellen Auftrags abgeschlossen ist. |
| Sicherung |Die Erweiterungsinstallation ist mit dem Fehler "COM+ konnte keine Daten mit dem Microsoft Distributed Transaction Coordinator austauschen" fehlgeschlagen. |Dies bedeutet i. d. R., dass der COM+-Dienst nicht ausgeführt wird. Wenden Sie sich an den Microsoft Support, um Hilfe beim Beheben dieses Problems zu erhalten. |
| Sicherung |Der Momentaufnahmevorgang ist mit folgendem VSS-Vorgangsfehler fehlgeschlagen: "Dieses Laufwerk ist durch die BitLocker-Laufwerkverschlüsselung gesperrt. Das Laufwerk muss mithilfe der Systemsteuerung entsperrt werden." |Deaktivieren Sie BitLocker für alle Laufwerke auf dem virtuellen Computer, und überprüfen Sie, ob der VSS-Fehler behoben ist. |
| Sicherung |Virtuelle Computer mit virtuellen Festplatten, die in Storage Premium gespeichert sind, werden für die Sicherung nicht unterstützt. |Keine |
| Sicherung |Der virtuelle Azure-Computer wurde nicht gefunden. |Dies tritt auf, wenn der primäre virtuelle Computer gelöscht wurde, die Sicherungsrichtlinie jedoch weiterhin einen virtuellen Computer für die Sicherung sucht. Gehen Sie wie folgt vor, um diesen Fehler zu beheben:  <ol><li>Erstellen Sie den virtuellen Computer mit dem gleichen Namen und dem gleichen Ressourcengruppennamen [Clouddienstname] neu. <br>(ODER) <li> Deaktivieren Sie den Schutz für diesen virtuellen Computer, damit nachfolgende Sicherungen nicht ausgelöst werden. </ol> |
| Sicherung |Der Agent für virtuelle Computer ist nicht auf dem virtuellen Computer vorhanden. Installieren Sie den VM-Agent (erforderliche Voraussetzung), und starten Sie den Vorgang erneut. |[Erfahren Sie mehr](#vm-agent) über die VM-Agent-Installation und die dazugehörige Überprüfung. |

## <a name="jobs"></a>Aufträge
| Vorgang | Fehlerdetails | Problemumgehung |
| --- | --- | --- |
| Auftrag abbrechen |Der Abbruch wird für diesen Auftragstyp nicht unterstützt. Warten Sie, bis der Auftrag abgeschlossen ist. |Keine |
| Auftrag abbrechen |Der Auftrag kann in diesem Status nicht abgebrochen werden. Warten Sie, bis der Auftrag abgeschlossen ist. <br>OR<br> Der ausgewählte Auftrag kann in diesem Status nicht abgebrochen werden. Warten Sie, bis der Auftrag abgeschlossen ist. |Der Auftrag ist aller Wahrscheinlichkeit nach fast abgeschlossen. Warten Sie, bis der Auftrag abgeschlossen ist. |
| Auftrag abbrechen |Das Abbrechen des Auftrags ist nicht möglich, da sich dieser nicht in Bearbeitung befindet. Der Abbruch wird nur für Aufträge in Bearbeitung unterstützt. Wiederholen Sie den Abbruchversuch für einen Auftrag, der sich in Bearbeitung befindet. |Dies wird durch einen vorübergehenden Zustand verursacht. Warten Sie eine Minute, und wiederholen Sie den Abbruchvorgang. |
| Auftrag abbrechen |Fehler beim Abbrechen des Auftrags – Warten Sie, bis der Auftrag abgeschlossen ist. |Keine |

## <a name="restore"></a>Wiederherstellen
| Vorgang | Fehlerdetails | Problemumgehung |
| --- | --- | --- |
| Wiederherstellen |Cloudinterner Fehler bei der Wiederherstellung |<ol><li>Der Clouddienst, in dem Sie die Wiederherstellung durchführen möchten, ist mit DNS-Einstellungen konfiguriert. Prüfen Sie  <br>$deployment = Get-AzureDeployment -ServiceName "ServiceName" -Slot "Production"     Get-AzureDns -DnsSettings $deployment.DnsSettings<br>Wenn eine Adresse konfiguriert wurde, bedeutet dies, dass DNS-Einstellungen konfiguriert sind.<br> <li>Der Clouddienst, in dem Sie die Wiederherstellung ausführen möchten, ist mit ReservedIP konfiguriert, und vorhandene virtuelle Computer im Clouddienst haben den Status „Beendet“.<br>Sie können mithilfe des folgenden PowerShell-Cmdlets überprüfen, ob ein Clouddienst eine IP-Adresse reserviert hat:<br>$deployment = Get-AzureDeployment -ServiceName "servicename" -Slot "Production" $dep.ReservedIPName <br><li>Sie versuchen, einen virtuellen Computer mit den folgenden speziellen Netzwerkkonfigurationen im selben Clouddienst wiederherzustellen. <br>– Virtuelle Computer unter Load Balancer-Konfiguration (intern und extern)<br>– Virtuelle Computer mit mehreren reservierten IP-Adressen<br>– Virtuelle Computer mit mehreren NICs<br>Wählen Sie einen neuen Clouddienst auf der Benutzeroberfläche aus, oder lesen Sie die [Überlegungen zu Wiederherstellungen](backup-azure-restore-vms.md#restoring-vms-with-special-network-configurations) für virtuelle Computer mit speziellen Netzwerkkonfigurationen.</ol> |
| Wiederherstellen |Der ausgewählte DNS-Name ist bereits vergeben. Geben Sie einen anderen DNS-Namen an, und versuchen Sie es erneut. |Der DNS-Name bezieht sich hier auf den Clouddienstnamen (der i. d. R. auf ".cloudapp.net" endet). Dieser muss eindeutig sein. Wenn der vorliegende Fehler auftritt, müssen Sie während der Wiederherstellung einen anderen Namen für den virtuellen Computer auswählen. <br><br> Dieser Fehler wird nur Benutzern des Azure-Portals angezeigt. Der Wiederherstellungsvorgang über PowerShell ist erfolgreich, da nur die Datenträger wiederhergestellt werden und kein virtueller Computer erstellt wird. Der Fehler tritt auf, wenn der virtuelle Computer nach Abschluss des Wiederherstellungsvorgangs für die Datenträger explizit von Ihnen erstellt wird. |
| Wiederherstellen |Die angegebene Konfiguration des virtuellen Netzwerks ist nicht korrekt. Geben Sie eine andere Konfiguration des virtuellen Netzwerks an, und versuchen Sie es erneut. |Keine |
| Wiederherstellen |Der angegebene Clouddienst verwendet eine reservierte IP, die nicht mit der Konfiguration des virtuellen Computers übereinstimmt, der wiederhergestellt werden soll. Geben Sie einen anderen Clouddienst an, der keine reservierte IP verwendet, oder wählen Sie einen anderen Wiederherstellungspunkt aus. |Keine |
| Wiederherstellen |Der Clouddienst hat den Grenzwert für die Anzahl der Eingabeendpunkte erreicht. Wiederholen Sie den Vorgang, indem Sie einen anderen Clouddienst angeben oder einen vorhandenen Endpunkt verwenden. |Keine |
| Wiederherstellen |Der Sicherungstresor und das Zielspeicherkonto befinden sich in unterschiedlichen Regionen. Stellen Sie sicher, dass sich das für den Wiederherstellungsvorgang angegebene Speicherkonto in derselben Azure-Region wie der Sicherungstresor befindet. |Keine |
| Wiederherstellen |Das für den Wiederherstellungsvorgang angegebene Speicherkonto wird nicht unterstützt. Es werden nur Basic-/Standard-Speicherkonten mit lokal redundanten oder georedundanten Replikationseinstellungen unterstützt. Wählen Sie ein unterstütztes Speicherkonto aus. |Keine |
| Wiederherstellen |Der Typ des für den Wiederherstellungsvorgang angegebenen Speicherkontos ist nicht online. Stellen Sie sicher, dass das für den Wiederherstellungsvorgang angegebene Speicherkonto online ist. |Dies kann durch einen vorübergehenden Fehler in Azure Storage oder einen Ausfall verursacht werden. Wählen Sie ein anderes Speicherkonto aus. |
| Wiederherstellen |Das Kontingent für Ressourcengruppen wurde erreicht. Löschen Sie einige Ressourcengruppen aus dem Azure-Portal, oder wenden Sie sich an den Azure-Support, um die Grenzwerte zu erhöhen. |Keine |
| Wiederherstellen |Das ausgewählte Subnetz besteht nicht. Wählen Sie ein vorhandenes Subnetz aus. |Keine |

## <a name="policy"></a>Richtlinie
| Vorgang | Fehlerdetails | Problemumgehung |
| --- | --- | --- |
| Richtlinie erstellen |Fehler beim Erstellen der Richtlinie – Verringern Sie die Aufbewahrungsdauer, um mit der Richtlinienkonfiguration fortzufahren. |Keine |

## <a name="vm-agent"></a>VM-Agent
### <a name="setting-up-the-vm-agent"></a>Einrichten des VM-Agents
Normalerweise ist der VM-Agent auf virtuellen Computern, die über den Azure-Katalog erstellt werden, bereits vorhanden. Auf virtuellen Computern, die aus lokalen Rechenzentren migriert werden, ist der VM-Agent aber nicht installiert. Für diese virtuellen Computer muss der VM-Agent explizit installiert werden. Erfahren Sie mehr über das [Installieren des VM-Agents auf einem vorhandenen virtuellen Computer](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx).

Für virtuelle Windows-Computer:

* Laden Sie den [Agent-MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409)herunter, und installieren Sie ihn. Zum Durchführen der Installation benötigen Sie Administratorberechtigungen.
* [Aktualisieren Sie die VM-Eigenschaft](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) , um anzugeben, dass der Agent installiert wurde.

Für virtuelle Linux-Computer:

* Installieren Sie den neuesten [Linux-Agent](https://github.com/Azure/WALinuxAgent) aus GitHub.
* [Aktualisieren Sie die VM-Eigenschaft](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) , um anzugeben, dass der Agent installiert wurde.

### <a name="updating-the-vm-agent"></a>Aktualisieren des VM-Agents
Für virtuelle Windows-Computer:

* Das Aktualisieren des VM-Agents ist so einfach wie das Neuinstallieren der [Binärdateien für den VM-Agent](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Allerdings müssen Sie sicherstellen, dass kein Sicherungsvorgang ausgeführt wird, während der VM-Agent aktualisiert wird.

Für virtuelle Linux-Computer:

* Folgen Sie den Anweisungen unter [Aktualisieren des Azure Linux-Agents auf einem virtuellen Computer](../virtual-machines/virtual-machines-linux-update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="validating-vm-agent-installation"></a>Überprüfen der VM-Agent-Installation
So überprüfen Sie die Version des VM-Agents auf virtuellen Windows-Computern

1. Melden Sie sich beim virtuellen Azure-Computer an, und navigieren Sie zum Ordner *C:\WindowsAzure\Packages*. Dieser Ordner enthält die Datei "WaAppAgent.exe".
2. Klicken Sie mit der rechten Maustaste auf die Datei, wechseln Sie zu **Eigenschaften**, und wählen Sie dann die Registerkarte **Details** aus. Im Feld mit der Produktversion sollte 2.6.1198.718 oder eine höhere Version angegeben sein.



<!--HONumber=Nov16_HO3-->


