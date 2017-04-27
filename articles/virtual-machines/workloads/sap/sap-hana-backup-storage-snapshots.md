---
title: SAP HANA-Azure-Sicherung auf der Grundlage von Speichermomentaufnahmen | Microsoft-Dokumentation
description: "Für SAP HANA auf virtuellen Azure-Computern stehen grundsätzlich zwei Sicherungsoptionen zur Verfügung. In diesem Artikel wird die SAP HANA-Sicherung basierend auf Speichermomentaufnahmen behandelt."
services: virtual-machines-linux
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ums.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 3/13/2017
ms.author: rclaus
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 5d395da0779d84e414341d6d73151fd7e77e3e8e
ms.lasthandoff: 04/03/2017


---

# <a name="sap-hana-backup-based-on-storage-snapshots"></a>SAP HANA-Sicherung auf der Grundlage von Speichermomentaufnahmen

## <a name="introduction"></a>Einführung

Dieser Artikel ist Teil einer dreiteiligen Reihe zur SAP HANA-Sicherung. [Sicherungsanleitung für SAP HANA in Azure Virtual Machines](sap-hana-backup-guide.md) enthält eine Übersicht und Informationen zu den ersten Schritten, und unter [SAP HANA-Sicherung mit Azure Backup auf Dateiebene](sap-hana-backup-file-level.md) wird die dateibasierte Sicherung behandelt.

Bei Verwendung einer VM-Sicherungsfunktion für ein All-in-One-Einzelinstanz-Demosystem ist es ratsam, eine VM-Sicherung durchzuführen, anstatt HANA-Sicherungen auf Betriebssystemebene zu verwalten. Eine Alternative hierzu ist die Erstellung von Azure-Blob-Momentaufnahmen zum Erstellen von Kopien einzelner virtueller Datenträger, die an einen virtuellen Computer angefügt sind, und die Beibehaltung der HANA-Datendateien. Ein kritischer Punkt ist die App-Konsistenz beim Erstellen einer VM-Sicherung oder Datenträger-Momentaufnahme, während das System aktiv ist. Informationen hierzu finden Sie unter _Konsistenz von SAP HANA-Daten beim Erstellen von Speichermomentaufnahmen_ im Artikel [Sicherungsanleitung für SAP HANA in Azure Virtual Machines](sap-hana-backup-guide.md) dieser Reihe. SAP HANA verfügt über ein Feature, das diese Arten von Speichermomentaufnahmen unterstützt.

## <a name="sap-hana-snapshots"></a>SAP HANA-Momentaufnahmen

In SAP HANA gibt es ein Feature, bei dem das Erstellen von Speichermomentaufnahmen unterstützt wird. Ab Dezember 2016 gilt für Systeme mit nur einem Container aber eine Einschränkung. Mehrinstanzenfähige Containerkonfigurationen unterstützen diese Art von Datenbankmomentaufnahme nicht (siehe [Create a Storage Snapshot (SAP HANA Studio)](https://help.sap.com/saphelp_hanaplatform/helpdata/en/a0/3f8f08501e44d89115db3c5aa08e3f/content.htm) (Erstellen einer Speichermomentaufnahme (SAP HANA Studio)).

Dies funktioniert wie folgt:

- Vorbereiten auf eine Speichermomentaufnahme durch die Initiierung der SAP HANA-Momentaufnahme
- Ausführen der Speichermomentaufnahme (z.B. Azure-Blob-Momentaufnahme)
- Bestätigen der SAP HANA-Momentaufnahme

![Screenshot: Erstellung einer SAP HANA-Datenmomentaufnahme per SQL-Anweisung](media/sap-hana-backup-storage-snapshots/image011.png)

In diesem Screenshot ist dargestellt, dass eine SAP HANA-Datenmomentaufnahme per SQL-Anweisung erstellt werden kann.

![Die Momentaufnahme wird in SAP HANA Studio auch im Sicherungskatalog angezeigt](media/sap-hana-backup-storage-snapshots/image012.png)

Die Momentaufnahme wird in SAP HANA Studio auch im Sicherungskatalog angezeigt.

![Auf dem Datenträger wird die Momentaufnahme im SAP HANA-Datenverzeichnis angezeigt](media/sap-hana-backup-storage-snapshots/image013.png)

Auf dem Datenträger wird die Momentaufnahme im SAP HANA-Datenverzeichnis angezeigt.

Es muss auch sichergestellt werden, dass die Konsistenz des Dateisystems gewährleistet ist, bevor die Speichermomentaufnahme ausgeführt wird, während sich SAP HANA im Vorbereitungsmodus für Momentaufnahmen befindet. Informationen hierzu finden Sie unter _Konsistenz von SAP HANA-Daten beim Erstellen von Speichermomentaufnahmen_ im Artikel [Sicherungsanleitung für SAP HANA in Azure Virtual Machines](sap-hana-backup-guide.md) dieser Reihe.

Nachdem die Speichermomentaufnahme erstellt wurde, ist es wichtig, dass die SAP HANA-Momentaufnahme bestätigt wird. Hierzu muss eine entsprechende SQL-Anweisung ausgeführt werden: BACKUP DATA CLOSE SNAPSHOT (siehe [BACKUP DATA CLOSE SNAPSHOT Statement (Backup and Recovery)](https://help.sap.com/saphelp_hanaplatform/helpdata/en/c3/9739966f7f4bd5818769ad4ce6a7f8/content.htm) (BACKUP DATA CLOSE SNAPSHOT-Anweisung (Sicherung und Wiederherstellung)).

> [!IMPORTANT]
> Bestätigen Sie die HANA-Momentaufnahme. Aufgrund des &quot;Copy-on-Write&quot;-Vorgangs benötigt SAP HANA im Vorbereitungsmodus für Momentaufnahmen ggf. zusätzlichen Datenträger-Speicherplatz. Es ist erst möglich, neue Sicherungen zu starten, nachdem die SAP HANA-Momentaufnahme bestätigt wurde.

## <a name="hana-vm-backup-via-azure-backup-service"></a>HANA-VM-Sicherung per Azure Backup-Dienst

Ab Dezember 2016 ist der Backup-Agent des Azure Backup-Diensts für Linux-VMs nicht mehr verfügbar. Zur Verwendung der Azure-Sicherung auf Datei- bzw. Verzeichnisebene werden SAP HANA-Sicherungsdateien auf eine Windows-VM kopiert, und anschließend wird der Backup-Agent verwendet. Andernfalls ist mit dem Azure Backup-Dienst nur eine vollständige Linux-VM-Sicherung möglich. Weitere Informationen finden Sie unter [Übersicht über die Funktionen in Azure Backup](../../../backup/backup-introduction-to-azure-backup.md).

Der Azure Backup-Dienst verfügt über eine Option zum Sichern und Wiederherstellen einer VM. Weitere Informationen zu diesem Dienst und zur Funktionsweise finden Sie im Artikel [Planen der Sicherungsinfrastruktur für virtuelle Computer in Azure](../../../backup/backup-azure-vms-introduction.md).

In diesem Artikel wird auf zwei wichtige Aspekte hingewiesen, die berücksichtigt werden sollten:

_&quot;Für virtuelle Linux-Computer sind nur dateikonsistente Sicherungen möglich, da Linux keine dem VSS entsprechende Plattform umfasst.&quot;_

_&quot;Anwendungen müssen einen eigenen &quot;Reparaturmechanismus&quot; für die wiederhergestellten Daten implementieren.&quot;_

Aus diesem Grund muss sichergestellt werden, dass sich SAP HANA beim Starten der Sicherung in einem konsistenten Zustand befindet. Informationen hierzu finden Sie unter _SAP HANA-Momentaufnahmen_ weiter oben im Dokument. Unter Umständen kann aber ein Problem auftreten, wenn SAP HANA in diesem Vorbereitungsmodus für Momentaufnahmen verbleibt. Weitere Informationen finden Sie unter [Create a Storage Snapshot (SAP HANA Studio)](https://help.sap.com/saphelp_hanaplatform/helpdata/en/a0/3f8f08501e44d89115db3c5aa08e3f/content.htm) (Erstellen einer Speichermomentaufnahme (SAP HANA Studio)).

Im Artikel wird auf Folgendes hingewiesen:

_&quot;Es wird dringend empfohlen, eine Speichermomentaufnahme nach ihrer Erstellung so schnell wie möglich zu bestätigen oder zu verwerfen. Während die Speichermomentaufnahme vorbereitet oder erstellt wird, werden die für die Momentaufnahme relevanten Daten eingefroren. Während des Einfrierzustands der für die Momentaufnahme relevanten Daten können in der Datenbank trotzdem Änderungen vorgenommen werden. Diese Änderungen führen nicht dazu, dass sich die eingefrorenen Daten für die Momentaufnahme ändern. Stattdessen werden die Änderungen im Datenbereich an Speicherorte geschrieben, die vom Bereich für die Speichermomentaufnahme getrennt sind. Außerdem werden die Änderungen in ein Protokoll geschrieben. Je länger die für die Momentaufnahme relevanten Daten aber eingefroren bleiben, desto stärker kann die Größe des Datenvolume zunehmen.&quot;_

Azure Backup stellt die Konsistenz des Dateisystems mithilfe von Azure-VM-Erweiterungen sicher. Diese Erweiterungen sind nicht als eigenständige Komponenten verfügbar und funktionieren nur in Verbindung mit dem Azure Backup-Dienst. Trotzdem ist es erforderlich, eine SAP HANA-Momentaufnahme zu verwalten, um die App-Konsistenz zu gewährleisten.

Azure Backup umfasst zwei Hauptphasen:

- Erstellen der Momentaufnahme
- Übertragen von Daten in einen Tresor

Sie können die SAP HANA-Momentaufnahme also bestätigen, nachdem die Phase des Azure Backup-Diensts zum Erstellen einer Momentaufnahme abgeschlossen ist. Es kann einige Minuten dauern, bis dies im Azure-Portal angezeigt wird.

![Abbildung: Teil der Sicherungsauftragsliste eines Azure Backup-Diensts](media/sap-hana-backup-storage-snapshots/image014.png)

In dieser Abbildung ist ein Teil der Sicherungsauftragsliste eines Azure Backup-Diensts dargestellt, der zum Sichern der HANA-Test-VM verwendet wurde.

![Klicken auf den Sicherungsauftrag im Azure-Portal zum Anzeigen der Auftragsdetails](media/sap-hana-backup-storage-snapshots/image015.png)

Klicken Sie im Azure-Portal auf den Sicherungsauftrag, um die Auftragsdetails anzuzeigen. Hier sind die beiden Phasen dargestellt. Es kann einige Minuten dauern, bis angezeigt wird, dass die Phase für die Momentaufnahme abgeschlossen ist. Die meiste Zeit fällt für die Phase der Datenübertragung an.

## <a name="hana-vm-backup-automation-via-azure-backup-service"></a>Automatisierung von HANA-VM-Sicherungen mit dem Azure Backup-Dienst

Wie bereits beschrieben wurde, kann die SAP HANA-Momentaufnahme manuell bestätigt werden, nachdem die Phase für die Erstellung der Azure Backup-Momentaufnahme abgeschlossen ist. Die Nutzung einer Automatisierung kann aber hilfreich sein, da die Sicherungsauftragsliste im Azure-Portal unter Umständen nicht von einem Administrator überwacht wird.

Hier wird beschrieben, wie dies mit Azure PowerShell-Cmdlets erreicht werden kann.

![Erstellen eines Azure Backup-Diensts mit dem Namen „hana-backup-vault“](media/sap-hana-backup-storage-snapshots/image016.png)

Es wurde ein Azure Backup-Dienst mit dem Namen &quot;hana-backup-vault&quot; erstellt. Mit dem PS-Befehl **Get-AzureRmRecoveryServicesVault -Name hana-backup-vault** wird das entsprechende Objekt abgerufen. Dieses Objekt wird dann verwendet, um den Sicherungskontext festzulegen. Dies ist in der nächsten Abbildung dargestellt.

![Überprüfen des aktiven Sicherungsauftrags](media/sap-hana-backup-storage-snapshots/image017.png)

Nach dem Festlegen des richtigen Kontexts können Sie den derzeit aktiven Sicherungsauftrag überprüfen und die dazugehörigen Auftragsdetails anzeigen. In der Liste mit den Unteraufgaben wird angezeigt, ob die Momentaufnahmephase des Azure-Sicherungsauftrags bereits abgeschlossen ist:

```
$ars = Get-AzureRmRecoveryServicesVault -Name hana-backup-vault
Set-AzureRmRecoveryServicesVaultContext -Vault $ars
$jid = Get-AzureRmRecoveryServicesBackupJob -Status InProgress | select -ExpandProperty jobid
Get-AzureRmRecoveryServicesBackupJobDetails -Jobid $jid | select -ExpandProperty subtasks
```

![Abfragen des Werts in einer Schleife, bis er „Completed“ lautet](media/sap-hana-backup-storage-snapshots/image018.png)

Nachdem die Auftragsdetails in einer Variablen gespeichert wurden, wird einfach PS-Syntax verwendet, um den ersten Arrayeintrag abzurufen und den Statuswert zu erhalten. Fragen Sie den Wert in einer Schleife ab, bis er &quot;Completed&quot; lautet, um das Automatisierungsskript fertig zu stellen.

```
$st = Get-AzureRmRecoveryServicesBackupJobDetails -Jobid $jid | select -ExpandProperty subtasks
$st[0] | select -ExpandProperty status
```

## <a name="hana-license-key-and-vm-restore-via-azure-backup-service"></a>HANA-Lizenzschlüssel und VM-Wiederherstellung per Azure Backup-Dienst

Der Azure Backup-Dienst ist dafür ausgelegt, während der Wiederherstellung eine neue VM zu erstellen. Derzeit ist nicht geplant, eine &quot;direkte&quot; Wiederherstellung einer vorhandenen Azure-VM durchzuführen.

![Abbildung: Wiederherstellungsoption des Azure-Diensts im Azure-Portal](media/sap-hana-backup-storage-snapshots/image019.png)

In dieser Abbildung ist die Wiederherstellungsoption des Azure-Diensts im Azure-Portal dargestellt. Sie können zwischen der Erstellung einer VM während der Wiederherstellung und der Wiederherstellung der Datenträger wählen. Nach dem Wiederherstellen der Datenträger ist es zusätzlich erforderlich, eine neue VM zu erstellen. Wenn in Azure eine neue VM erstellt wird, ändert sich die eindeutige VM-ID (siehe [Accessing and Using Azure VM Unique ID](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) (Zugreifen auf und Verwenden einer eindeutigen Azure-VM-ID)).

![Abbildung: Eindeutige Azure-VM-ID vor und nach der Wiederherstellung mit dem Azure Backup-Dienst](media/sap-hana-backup-storage-snapshots/image020.png)

In dieser Abbildung ist die eindeutige Azure-VM-ID vor und nach der Wiederherstellung mit dem Azure Backup-Dienst dargestellt. Diese eindeutige VM-ID wird für den SAP-Hardwareschlüssel verwendet, der für die SAP-Lizenzierung genutzt wird. Aus diesem Grund muss nach einer VM-Wiederherstellung eine neue SAP-Lizenz installiert werden.

Während der Erstellung dieser Sicherungsanleitung wurde die Vorschauversion eines neuen Azure Backup-Features vorgestellt. Mit diesem Feature ist basierend auf der VM-Momentaufnahme, die für die VM-Sicherung erstellt wurde, eine Wiederherstellung auf Dateiebene möglich. So entfällt die Anforderung, eine neue VM bereitstellen zu müssen. Die eindeutige VM-ID bleibt unverändert, und es ist kein neuer SAP HANA-Lizenzschlüssel erforderlich. Weitere Informationen und eine Dokumentation zu diesem Feature werden bereitgestellt, nachdem es vollständig getestet wurde.

Für Azure Backup ist dann zu einem späteren Zeitpunkt die Sicherung von einzelnen virtuellen Azure-Datenträgern möglich, einschließlich der Dateien und Verzeichnisse der VM. Ein großer Vorteil von Azure Backup ist die Verwaltung aller eigenen Sicherungen, sodass dies nicht vom Kunden durchgeführt werden muss. Wenn eine Wiederherstellung erforderlich ist, wählt Azure Backup die richtige Sicherung aus.

## <a name="sap-hana-vm-backup-via-manual-disk-snapshot"></a>SAP HANA-VM-Sicherung per manueller Datenträger-Momentaufnahme

Anstatt den Azure Backup-Dienst zu verwenden, können Sie auch eine individuelle Sicherungslösung konfigurieren, indem Sie Blob-Momentaufnahmen von Azure-VHDs manuell mit PowerShell erstellen. Eine Beschreibung dieser Schritte finden Sie unter [Using blob snapshots with PowerShell](https://blogs.msdn.microsoft.com/cie/2016/05/17/using-blob-snapshots-with-powershell/) (Verwenden von Blob-Momentaufnahmen mit PowerShell).

Dies ermöglicht zwar eine höhere Flexibilität, aber die weiter oben in diesem Dokument erwähnten Probleme werden hierdurch nicht gelöst:

- Es muss weiterhin sichergestellt werden, dass sich SAP HANA in einem konsistenten Zustand befindet.
- Der Betriebssystemdatenträger kann auch dann nicht überschrieben werden, wenn die Zuordnung der VM aufgehoben wird, da in einem Fehler angegeben wird, dass ein Lease vorhanden ist. Dies funktioniert nur nach dem Löschen der VM, wonach dann aber wieder eine neue eindeutige VM-ID benötigt wird und eine neue SAP-Lizenz installiert werden muss.

![Für eine Azure-VM können auch nur die Datenträger für Daten wiederhergestellt werden](media/sap-hana-backup-storage-snapshots/image021.png)

Für eine Azure-VM können auch nur die Datenträger für Daten wiederhergestellt werden. So wird vermieden, dass eine neue eindeutige VM-ID erforderlich ist und die SAP-Lizenz ungültig wird:

- Für den Test wurden zwei Azure-Datenträger an eine VM angefügt, und zusätzlich wurde eine Software-RAID-Komponente definiert. 
- Mit dem SAP HANA-Feature für Momentaufnahmen wurde bestätigt, dass sich SAP HANA in einem konsistenten Zustand befindet.
- Das Dateisystem wurde eingefroren. (Informationen hierzu finden Sie unter _Konsistenz von SAP HANA-Daten beim Erstellen von Speichermomentaufnahmen_ im Artikel [Sicherungsanleitung für SAP HANA in Azure Virtual Machines](sap-hana-backup-guide.md) dieser Reihe.)
- Für beide Datenträger wurden Blob-Momentaufnahmen erstellt.
- Der Einfrierzustand des Dateisystems wurde aufgehoben.
- Die SAP HANA-Momentaufnahme wurde bestätigt.
- Zum Wiederherstellen der Datenträger wurde die VM heruntergefahren, und beide Datenträger wurden getrennt.
- Nach dem Trennen der Datenträger wurden diese durch die vorherigen Blob-Momentaufnahmen überschrieben.
- Anschließend wurden die wiederhergestellten virtuellen Datenträger wieder an die VM angefügt.
- Nach dem Starten der VM funktionierte die Software-RAID-Komponente fehlerfrei, und die Zurücksetzung auf die Zeit der Blob-Momentaufnahme wurde durchgeführt.
- HANA wurde auf die HANA-Momentaufnahme zurückgesetzt.

Wenn es möglich wäre, SAP HANA vor der Erstellung der Blob-Momentaufnahmen herunterzufahren, wäre das Verfahren weniger komplex. In diesem Fall könnte die HANA-Momentaufnahme übersprungen werden, und falls im System keine anderen Vorgänge ablaufen, könnte auch das Einfrieren des Dateisystems entfallen. Die Komplexität erhöht sich, wenn Momentaufnahmen bei vollem Onlinebetrieb erstellt werden müssen. Informationen hierzu finden Sie unter _Konsistenz von SAP HANA-Daten beim Erstellen von Speichermomentaufnahmen_ im Artikel [Sicherungsanleitung für SAP HANA in Azure Virtual Machines](sap-hana-backup-guide.md) dieser Reihe.

## <a name="next-steps"></a>Nächste Schritte
* [Sicherungsanleitung für SAP HANA in Azure Virtual Machines](sap-hana-backup-guide.md) enthält eine Übersicht und Informationen zu den ersten Schritten.
* Unter [SAP HANA-Sicherung mit Azure Backup auf Dateiebene](sap-hana-backup-file-level.md) wird die dateibasierte Sicherung behandelt.
* Informationen zur Erzielung von hoher Verfügbarkeit und zur Planung der Notfallwiederherstellung für SAP HANA in Azure (große Instanzen) finden Sie unter [Hohe Verfügbarkeit und Notfallwiederherstellung für SAP HANA in Azure (große Instanzen)](hana-overview-high-availability-disaster-recovery.md).

