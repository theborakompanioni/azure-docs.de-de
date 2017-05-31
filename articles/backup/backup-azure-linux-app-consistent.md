---
title: 'Azure Backup: Anwendungskonsistente Sicherungen von Linux-VMs | Microsoft-Dokumentation'
description: "Verwenden Sie Skripts, um für Ihre virtuellen Linux-Computer anwendungskonsistente Sicherungen in Azure zu gewährleisten. Die Skripts gelten nur für Linux-VMs in einer Ressourcen-Manager-Bereitstellung. Sie gelten nicht für Windows-VMs oder Dienst-Manager-Bereitstellungen. Dieser Artikel führt Sie durch die Schritte zum Konfigurieren von Skripts, einschließlich zur Problembehandlung."
services: backup
documentationcenter: dev-center-name
author: anuragmehrotra
manager: shivamg
keywords: "App-konsistente Sicherung; anwendungskonsistente Sicherung für Azure-VM; Linux-VM-Sicherung; Azure-Sicherung"
ms.assetid: bbb99cf2-d8c7-4b3d-8b29-eadc0fed3bef
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 4/12/2017
ms.author: anuragm;markgal
ms.translationtype: Human Translation
ms.sourcegitcommit: fc4172b27b93a49c613eb915252895e845b96892
ms.openlocfilehash: 378c65bec8fd1f880ed459e76f5e4b5d85e49d2a
ms.contentlocale: de-de
ms.lasthandoff: 05/12/2017


---

# <a name="application-consistent-backup-of-azure-linux-vms-preview"></a>Anwendungskonsistente Sicherung von Azure-Linux-VMs (Vorschau)

In diesem Artikel finden Sie Informationen zum Pre-Skript- und Post-Skript-Framework von Linux und wie es zum Erstellen anwendungskonsistenter Sicherungen von Azure-Linux-VMs genutzt werden kann.

> [!Note]
> Das Pre-Skript- und Post-Skript-Framework wird nur für virtuelle Linux-Computer unterstützt, die mit Azure Resource Manager bereitgestellt wurden. Skripts für Anwendungskonsistenz werden nicht für mit Service Manager bereitgestellte virtuelle Computer und nicht für Windows-VMs unterstützt.
>

## <a name="how-the-framework-works"></a>Funktionsweise des Frameworks

Das Framework bietet eine Option zum Ausführen benutzerdefinierter Pre-Skripts und Post-Skripts beim Erstellen einer VM-Momentaufnahme. Pre-Skripts werden unmittelbar vor Erstellen der VM-Momentaufnahme und Post-Skripts unmittelbar nach Erstellen der VM-Momentaufnahme ausgeführt. Dies ermöglicht Ihnen, Ihre Anwendung und Umgebung beim Erstellen von VM-Momentaufnahmen flexibel zu steuern.

In diesem Szenario ist das Sicherstellen anwendungskonsistenter VM-Sicherungen wichtig. Das Pre-Skript kann für die Anwendung native APIs aufrufen, um die E/A-Vorgänge stillzulegen und den Arbeitsspeicherinhalt auf den Datenträger zu leeren. Dies stellt sicher, dass die Momentaufnahme anwendungskonsistent ist (d.h., dass die Anwendung gestartet wird, wenn der virtuelle Computer nach der Wiederherstellung hochgefahren wird). Mit dem Post-Skript können die E/A-Vorgänge reaktiviert werden. Dies erfolgt mithilfe von für die Anwendung nativen APIs, sodass die Anwendung im Anschluss an die VM-Momentaufnahmen den normalen Betrieb fortsetzen kann.

## <a name="steps-to-configure-pre-script-and-post-script"></a>Schritte zum Konfigurieren des Pre- und Post-Skripts

1. Melden Sie sich bei der zu sichernden Linux-VM als Root-Benutzer an.

2. Laden Sie die Datei **VMSnapshotScriptPluginConfig.json** von [GitHub](https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig) herunter, und kopieren Sie sie dann auf allen zu sichernden VMs in den Ordner **/etc/azure**. Erstellen Sie das Verzeichnis **/etc/azure**, sofern noch nicht vorhanden.

3. Kopieren Sie das Pre-Skript und Post-Skript für Ihre Anwendung auf alle zu sichernden VMs. Sie können die Skripts in einem beliebigen Speicherort auf der VM kopieren. In der Datei **VMSnapshotScriptPluginConfig.json** müssen Sie den vollständigen Pfad der Skriptdateien aktualisieren.

4. Stellen Sie für diese Dateien die folgenden Berechtigungen sicher:

   - **VMSnapshotScriptPluginConfig.json:** Berechtigung „600“. Beispiel: Nur der Root-Benutzer darf die Berechtigungen „read“ und „write“ für diese Datei haben. Kein Benutzer darf die Berechtigung „execute“ haben.

   - **Pre-Skript-Datei:** Berechtigung „700“.  Beispiel: Nur der Root-Benutzer darf die Berechtigungen „read“, „write“ und „execute“ für diese Datei haben.
  
   - **Post-Skript:** Berechtigung „700“. Beispiel: Nur der Root-Benutzer darf die Berechtigungen „read“, „write“ und „execute“ für diese Datei haben.

   > [!Important]
   > Das Framework bietet Benutzern sehr viele Möglichkeiten. Deshalb ist es wichtig, dass es geschützt ist und dass nur der Root-Benutzer Zugriff auf wichtige JSON- und Skriptdateien hat.
   > Wenn die genannten Voraussetzungen nicht erfüllt sind, wird das Skript nicht ausgeführt. Dies führt zu einer dateisystem- bzw. absturzkonsistenten Sicherung.
   >

5. Konfigurieren Sie **VMSnapshotScriptPluginConfig.json** entsprechend der folgenden Beschreibung:
    - **pluginName:** Lassen Sie dieses Feld unverändert, da die Skripts andernfalls unter Umständen nicht wie erwartet ausgeführt werden.

    - **preScriptLocation:** Geben Sie den vollständigen Pfad des Pre-Skripts auf der zu sichernden VM an.

    - **postScriptLocation**: Geben Sie den vollständigen Pfad des Post-Skripts auf der zu sichernden VM an.

    - **preScriptParams:** Geben Sie die optionalen Parameter an, die an das Pre-Skript übergeben werden müssen. Alle Parameter müssen in Anführungszeichen gesetzt und mehrere Parameter durch ein Komma getrennt werden.

    - **postScriptParams:** Geben Sie die optionalen Parameter an, die an das Post-Skript übergeben werden müssen. Alle Parameter müssen in Anführungszeichen gesetzt und mehrere Parameter durch ein Komma getrennt werden.

    - **preScriptNoOfRetries:** Legen Sie die Häufigkeit fest, mit der das Pre-Skript bei einem Fehler vor seinem Abschluss wiederholt werden soll. Null bedeutet nur einen Versuch und keine Wiederholung bei einem Fehler.

    - **postScriptNoOfRetries:** Legen Sie die Häufigkeit fest, mit der das Post-Skript bei einem Fehler vor seinem Abschluss wiederholt werden soll. Null bedeutet nur einen Versuch und keine Wiederholung bei einem Fehler.
    
    - **timeoutInSeconds:** Geben Sie individuelle Timeouts für das Pre-Skript und das Post-Skript an.

    - **continueBackupOnFailure:** Legen Sie diesen Wert auf **TRUE** fest, wenn Azure Backup bei einem Pre-Skript- oder Post-Skript-Fehler zu einer dateisystem- bzw. absturzkonsistenten Sicherung wechseln soll. Bei Festlegung auf **FALSE** misslingt die Sicherung bei einem Skriptfehler (außer bei einer VM mit einzelnem Datenträger, bei der unabhängig von dieser Einstellung auf eine absturzkonsistente Sicherung zurückgegriffen wird).

    - **fsFreezeEnabled:** Geben Sie an, ob unter Linux „fsfreeze“ aufgerufen werden soll, während die VM-Momentaufnahme erstellt wird, um Dateisystemkonsistenz sicherzustellen. Wir empfehlen die Beibehaltung dieser Einstellung auf **TRUE**, es sei denn, Ihre Anwendung ist von der Deaktivierung von „fsfreeze“ abhängig.

6. Das Skriptframework ist nun konfiguriert. Wenn die VM-Sicherung bereits konfiguriert ist, werden bei der nächsten Sicherung die Skripts aufgerufen und eine anwendungskonsistente Sicherung ausgelöst. Wenn die VM-Sicherung nicht konfiguriert ist, konfigurieren Sie sie entsprechend den Anweisungen unter [Sichern virtueller Azure-Computer in Recovery Services-Tresoren](https://docs.microsoft.com/azure/backup/backup-azure-vms-first-look-arm).

## <a name="troubleshooting"></a>Problembehandlung

Fügen Sie beim Schreiben Ihres Pre-Skripts und Post-Skripts unbedingt eine entsprechende Protokollierung hinzu, und überprüfen Sie die Skriptprotokolle, um etwaige Skriptprobleme zu beheben. Wenn Sie bei der Ausführung von Skripts weiter Probleme haben sollten, konsultieren Sie die folgende Tabelle.

| Error | Fehlermeldung | Empfohlene Maßnahme |
| ------------------------ | -------------- | ------------------ |
| Pre-ScriptExecutionFailed |Das Pre-Skript hat einen Fehler zurückgegeben, sodass die Sicherung gegebenenfalls nicht anwendungskonsistent ist.    | Untersuchen Sie die Fehlerprotokolle des Skripts, um das Problem zu beheben.|  
|    Post-ScriptExecutionFailed |    Das Post-Skript hat einen Fehler zurückgegeben, der sich auf den Anwendungszustand auswirken kann. |    Untersuchen Sie die Fehlerprotokolle des Skripts, um das Problem zu beheben, und überprüfen Sie den Anwendungszustand. |
| Pre-ScriptNotFound |    Das Pre-Skript wurde nicht am in der Konfigurationsdatei **VMSnapshotScriptPluginConfig.json** angegebenen Speicherort gefunden. |    Stellen Sie sicher, dass das Pre-Skript im Pfad vorhanden ist, der in der Konfigurationsdatei angegeben ist, um eine anwendungskonsistente Sicherung zu gewährleisten.|
| Post-ScriptNotFound |    Das Post-Skript wurde nicht am in der Konfigurationsdatei **VMSnapshotScriptPluginConfig.json** angegebenen Speicherort gefunden. |    Stellen Sie sicher, dass das Post-Skript im Pfad vorhanden ist, der in der Konfigurationsdatei angegeben ist, um eine anwendungskonsistente Sicherung zu gewährleisten.|
| IncorrectPluginhostFile |    Die zur Erweiterung „VmSnapshotLinux“ gehörige Datei **Pluginhost** ist beschädigt, sodass Pre-Skript und Post-Skript nicht ausgeführt werden können und die Sicherung nicht anwendungskonsistent sein wird.    | Deinstallieren Sie die Erweiterung **VmSnapshotLinux**. Diese wird dann zum Beheben des Problems bei der nächsten Sicherung automatisch neu installiert. |
| IncorrectJSONConfigFile | Die Datei **VMSnapshotScriptPluginConfig.json** ist falsch, sodass Pre-Skript und Post-Skript nicht ausgeführt werden können und die Sicherung nicht anwendungskonsistent sein wird. | Laden Sie die Kopie von [GitHub](https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig) herunter, und konfigurieren Sie sie erneut. |
| InsufficientPermissionforPre-Script | Zum Ausführen von Skripts muss der Root-Benutzer der Besitzer der Datei sein, und für die Datei muss die Berechtigung „700“ gelten (d.h., dass nur der Besitzer die Berechtigungen „read“, „write“ und „execute“ haben darf). | Stellen Sie sicher, dass der Root-Benutzer der Besitzer der Skriptdatei ist und dass nur der Besitzer die Berechtigungen „read“, „write“ und „execute“ hat. |
| InsufficientPermissionforPost-Skript | Zum Ausführen von Skripts muss der Root-Benutzer der Besitzer der Datei sein, und für die Datei muss die Berechtigung „700“ gelten (d.h., dass nur der Besitzer die Berechtigungen „read“, „write“ und „execute“ haben darf). | Stellen Sie sicher, dass der Root-Benutzer der Besitzer der Skriptdatei ist und dass nur der Besitzer die Berechtigungen „read“, „write“ und „execute“ hat. |
| Pre-ScriptTimeout | Bei Ausführung des Pre-Skripts für eine anwendungskonsistente Sicherung ist ein Timeout aufgetreten. | Überprüfen Sie das Skript, und erhöhen Sie den Timeoutwert in der Datei **VMSnapshotScriptPluginConfig.json** unter **/etc/azure**. |
| Post-ScriptTimeout | Bei Ausführung des Post-Skripts für eine anwendungskonsistente Sicherung ist ein Timeout aufgetreten. | Überprüfen Sie das Skript, und erhöhen Sie den Timeoutwert in der Datei **VMSnapshotScriptPluginConfig.json** unter **/etc/azure**. |

## <a name="next-steps"></a>Nächste Schritte
[Konfigurieren der VM-Sicherung in einem Recovery Services-Tresor](https://docs.microsoft.com/azure/backup/backup-azure-arm-vms)

