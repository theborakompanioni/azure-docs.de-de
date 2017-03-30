---
title: 'Azure Backup: Anwendungskonsistente Sicherung von Azure-Linux-VMs | Microsoft-Dokumentation'
description: Anwendungskonsistente Sicherung von Azure-Linux-VMs
services: backup
documentationcenter: dev-center-name
author: anuragm
manager: shivamg
keywords: "App-konsistente Sicherung; anwendungskonsistente Sicherung für Azure-VM; Linux-VM-Sicherung; Azure-Sicherung"
ms.assetid: bbb99cf2-d8c7-4b3d-8b29-eadc0fed3bef
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 3/20/2017
ms.author: anuragm;markgal
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: 044b5d3834518b44209485d1c1a68f2ac0f8a455
ms.lasthandoff: 03/22/2017


---
# <a name="application-consistent-backup-of-azure-linux-vms-preview"></a>Anwendungskonsistente Sicherung von Azure-Linux-VMs (Vorschau)

In diesem Artikel finden Sie Informationen zum Pre- und Post-Skript-Framework von Linux und wie es zum Erstellen anwendungskonsistenter Sicherungen von Azure-Linux-VMs genutzt werden kann.

> [!Note]
> Das Pre- und Post-Skript-Framework wird nur für mit Resource Manager bereitgestellte virtuelle Linux-Computer und nicht für klassisch bereitgestellte VMs oder virtuelle Windows-Computer unterstützt.
>

## <a name="how-the-framework-works"></a>Funktionsweise des Frameworks

Das Framework bietet eine Option zum Ausführen benutzerdefinierter Pre- und Post-Skripts beim Erstellen einer VM-Momentaufnahme. Das Pre-Skript wird unmittelbar vor Erstellen der VM-Momentaufnahme, das Post-Skript unmittelbar nach Erstellen der VM-Momentaufnahme ausgeführt. Dies ermöglicht Benutzern, ihre Anwendung und Umgebung beim Erstellen der VM-Sicherung flexibel zu steuern.

Ein wichtiger Aspekt dieses Frameworks ist das Sicherstellen anwendungskonsistenter VM-Sicherungen. Das Pre-Skript kann für die Anwendung native APIs aufrufen, um die E/A-Vorgänge stillzulegen und den Arbeitsspeicherinhalt auf den Datenträger zu leeren. Dies stellt sicher, dass die Momentaufnahme anwendungskonsistent ist, was heißt, dass die Anwendung gestartet wird, wenn die VM nach der Wiederherstellung hochgefahren wird. Das Post-Skript dient zum Reaktivieren der E/A-Vorgänge mithilfe für die Anwendung nativer APIs, damit die Anwendung im Anschluss an die VM-Momentaufnahme den normalen Betrieb fortsetzen kann.

## <a name="steps-to-configure-pre-script-and-post-script"></a>Schritte zum Konfigurieren des Pre- und Post-Skripts

1. Melden Sie sich bei der zu sichernden Linux-VM als Benutzer mit der Berechtigung „root“ an.

2. Laden Sie die Datei „VMSnapshotPluginConfig.json“ von [GitHub](https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig) herunter, und kopieren Sie sie auf allen zu sichernden VMs in den Ordner „/etc/azure“. Erstellen Sie das Verzeichnis „/etc/azure“, sofern noch nicht vorhanden.

3. Kopieren Sie das Pre- und Post-Skript für Ihre Anwendung auf alle zu sichernden VMs. Sie können die Skripts an einen beliebigen Speicherort auf der VM kopieren. In der Datei „VMSnapshotPluginConfig.json“ müssen Sie den vollständigen Pfad der Skriptdateien aktualisieren.

4. Stellen Sie für die Dateien folgende Berechtigungen sicher:

   - VMSnapshotPluginConfig.json: Berechtigung „600“, was bedeutet, dass nur der Benutzer „root“ die Berechtigungen „read“ und „write“ für diese Datei haben darf. Kein Benutzer darf die Berechtigung „execute“ haben.
   - Pre-Skript-Datei: Berechtigung „700“, was bedeutet, dass nur der Benutzer „root“ die Berechtigungen „read“, „write“ und „execute“ für diese Datei haben darf.
   - Post-Skript-Datei: Berechtigung „700“, was bedeutet, dass nur der Benutzer „root“ die Berechtigungen „read“, „write“ und „execute“ für diese Datei haben darf.
   
   > [!Note]
   > Das Framework bietet Benutzer sehr viele Möglichkeiten. Deshalb ist es überaus wichtig, dass es vollständig geschützt ist und nur der Benutzer „root“ Zugriff auf wichtige JSON- und Skriptdateien hat.
   > Wenn die genannten Voraussetzungen nicht erfüllt sind, wird das Skript nicht ausgeführt, was zu einer dateisystem- bzw. absturzkonsistenten Sicherung führt.
   >
   
5. Konfigurieren Sie „VMSnapshotPluginConfig.json“ gemäß den folgenden Vorgaben.
    - **pluginName**: Lassen Sie dieses Feld unverändert, da Ihre Skripts andernfalls ggf. nicht wie erwartet funktionieren.
    - **PreScriptLocation**: Geben Sie den vollständigen Pfad des Pre-Skripts auf der zu sichernden VM an.
    - **postScriptLocation**: Geben Sie den vollständigen Pfad des Post-Skripts auf der zu sichernden VM an.
    - **PreScriptParams**: Optionale Parameter, die an das Pre-Skript übergeben werden müssen. Parameter müssen in Anführungszeichen gesetzt und mehrere Parameter durch ein Komma getrennt werden.
    - **postScriptParams**: Optionale Parameter, die an das Post-Skript übergeben werden müssen. Parameter müssen in Anführungszeichen gesetzt und mehrere Parameter durch ein Komma getrennt werden.
    - **PreScriptNoOfRetries**: Häufigkeit, mit der das Pre-Skript bei einem Fehler vor seinem Abschluss wiederholt werden soll. 0 bedeutet nur einen Versuch und keine Wiederholung bei einem Fehler.
    - **postScriptNoOfRetries**: Häufigkeit, mit der das Post-Skript bei einem Fehler vor seinem Abschluss wiederholt werden soll. 0 bedeutet nur einen Versuch und keine Wiederholung bei einem Fehler.
    - **timeoutInSeconds**: Individuelle Timeouts für das Pre- und das Post-Skript.
    - **continueBackupOnFailure**: Legen Sie diesen Wert auf TRUE fest, wenn Azure Backup bei einem Pre-Skript- oder Post-Skript-Fehler zu einer dateisystem- bzw. absturzkonsistenten Sicherung wechseln soll. Bei Festlegung auf FALSE misslingt die Sicherung bei einem Skriptfehler (außer im Fall einer VM mit einzelnem Datenträger, wo unabhängig von dieser Einstellung auf eine absturzkonsistente Sicherung zurückgegriffen wird).
    - **fsFreezeEnabled**: Gibt an, ob unter Linux „fsfreeze“ aufgerufen werden soll, während die VM-Momentaufnahme erstellt wird, um Dateisystemkonsistenz sicherzustellen. Wir empfehlen die Festlegung auf TRUE, es sei denn, Ihre Anwendung ist von der Deaktivierung von „fsfreeze“ abhängig.
    
6. Das Skriptframework ist nun konfiguriert. Wenn die VM-Sicherung bereits konfiguriert ist, ruft Azure Backup die Skripts auf und löst eine anwendungskonsistente Sicherung aus. Wenn die VM-Sicherung nicht konfiguriert ist, konfigurieren Sie sie unter Befolgen von [Sichern virtueller Azure-Computer in Recovery Services-Tresoren](https://docs.microsoft.com/azure/backup/backup-azure-vms-first-look-arm).

## <a name="troubleshooting"></a>Problembehandlung

Fügen Sie unbedingt beim Schreiben Ihres Pre- und Post-Skripts eine entsprechende Protokollierung hinzu, und überprüfen Sie die Skriptprotokolle, um etwaige Skriptprobleme zu beheben. Wenn Sie bei der Ausführung von Skripts weiter Probleme haben sollten, konsultieren Sie die folgende Tabelle.

| Fehler | Fehlermeldung | Empfohlene Maßnahme |
| ------------------------ | -------------- | ------------------ |
| Pre-ScriptExecutionFailed |Das Pre-Skript hat einen Fehler zurückgegeben, sodass die Sicherung ggf. nicht anwendungskonsistent ist.    | Untersuchen Sie die Fehlerprotokolle des Skripts, um das Problem zu beheben.|  
|    Post-ScriptExecutionFailed |    Das Post-Skript hat einen Fehler zurückgegeben, der sich auf den Anwendungszustand auswirken kann. |    Untersuchen Sie die Fehlerprotokolle des Skripts, um das Problem zu beheben, und überprüfen Sie den Anwendungszustand. |
| Pre-ScriptNotFound |    Das Pre-Skript wurde nicht am in der Konfigurationsdatei „VMSnapshotPluginConfig.json“ angegebenen Speicherort gefunden. |    Stellen Sie sicher, dass das Pre-Skript im Pfad vorhanden ist, der in der Konfigurationsdatei angegeben ist, um eine anwendungskonsistente Sicherung zu gewährleisten.|
| Post-ScriptNotFound |    Das Post-Skript wurde nicht am in der Konfigurationsdatei „VMSnapshotPluginConfig.json“ angegebenen Speicherort gefunden. |    Stellen Sie sicher, dass das Post-Skript im Pfad vorhanden ist, der in der Konfigurationsdatei angegeben ist, um eine anwendungskonsistente Sicherung zu gewährleisten.|
| IncorrectPluginhostFile |    Die zur Erweiterung „VmSnapshotLinux“ gehörige Datei „pluginhost“ ist beschädigt, weshalb Pre- und Post-Skript nicht ausgeführt werden können und die Sicherung nicht anwendungskonsistent sein wird.    | Deinstallieren Sie die Erweiterung „VmSnapshotLinux“, die zum Beheben des Problems bei der nächsten Sicherung automatisch neu installiert wird. |
| IncorrectJSONConfigFile | Die Datei „VMSnapshotPluginConfig.json“ ist falsch, weshalb Pre- und Post-Skript nicht ausgeführt werden können und die Sicherung nicht anwendungskonsistent sein wird. | Laden Sie die Kopie von [GitHub](https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig) herunter, und konfigurieren Sie sie erneut. |
| InsufficientPermissionforPre-Script | Zum Ausführen von Skripts muss der Benutzer „root“ der Besitzer der Datei sein, und für die Datei muss die Berechtigung „700“ gelten, was bedeutet, dass nur der Besitzer die Berechtigungen „read“, „write“ und „execute“ haben darf. | Stellen Sie sicher, der Benutzer „root“ der Besitzer der Skriptdatei ist und nur der Besitzer die Berechtigungen „read“, „write“ und „execute“ hat. |
| InsufficientPermissionforPost-Skript | Zum Ausführen von Skripts muss der Benutzer „root“ der Besitzer der Datei sein, und für die Datei muss die Berechtigung „700“ gelten, was bedeutet, dass nur der Besitzer die Berechtigungen „read“, „write“ und „execute“ haben darf. | Stellen Sie sicher, der Benutzer „root“ der Besitzer der Skriptdatei ist und nur der Besitzer die Berechtigungen „read“, „write“ und „execute“ hat. |
| Pre-ScriptTimeout | Bei Ausführung des Pre-Skripts für eine anwendungskonsistente Sicherung ist ein Timeout aufgetreten. | Überprüfen Sie das Skript, und erhöhen Sie den Timeoutwert in der Datei „VMSnapshotPluginConfig.json“ unter „/etc/azure“. |
| Post-ScriptTimeout | Bei Ausführung des Post-Skripts für eine anwendungskonsistente Sicherung ist ein Timeout aufgetreten. | Überprüfen Sie das Skript, und erhöhen Sie den Timeoutwert in der Datei „VMSnapshotPluginConfig.json“ unter „/etc/azure“. |

## <a name="next-steps"></a>Nächste Schritte
[Konfigurieren der VM-Sicherung in einem Recovery Services-Tresor](https://docs.microsoft.com/azure/backup/backup-azure-arm-vms)

