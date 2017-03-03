---
title: Wiederherstellen von Daten von einer Azure Backup Server-Instanz | Microsoft-Dokumentation
description: Stellen Sie Daten wiederher, die Sie in einem Azure-Sicherungstresor auf einem beliebigen, bei diesem Tresor registrierten Server von Azure Backup Server gesichert haben.
services: backup
documentationcenter: 
author: nkolli1
manager: shreeshd
editor: 
ms.assetid: a55f8c6b-3627-42e1-9d25-ed3e4ab17b1f
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: adigan;giridham;trinadhk;markgal
translationtype: Human Translation
ms.sourcegitcommit: 14cc190a7d1cde1181a6f26ef83095bc601f7fbb
ms.openlocfilehash: 36c4e1865c99dd1c55be798e1d310a02f2af0864
ms.lasthandoff: 01/28/2017


---
# <a name="recovering-data-from-another-azure-backup-server-in-the-backup-vault"></a>Wiederherstellen von Daten von einem anderen Server von Azure Backup Server im Sicherungstresor
Sie können nun Daten wiederherstellen, die Sie in einem Azure-Sicherungstresor auf einem beliebigen, bei diesem Tresor registrierten Server von Azure Backup Server gesichert haben. Das entsprechende Verfahren ist vollständig in die ABS-Verwaltungskonsole integriert und ähnelt dem anderer Wiederherstellungsworkflows.

> [!NOTE]
> Dieser Artikel und die folgenden Schritte gelten auch für [System Center Data Protection Manager UR7] (https://support.microsoft.com/en-us/kb/3065246) mit dem [neuesten Azure Backup-Agent](http://aka.ms/azurebackup_agent).
>
>

## <a name="recover-data-from-another-azure-backup-server"></a>Wiederherstellen von Daten von einem anderen Server von Azure Backup Server
Zum Wiederherstellen von Daten von einem anderen Server von Azure Backup Server:

1. Klicken Sie auf der Registerkarte **Wiederherstellung** der ABS-Verwaltungskonsole auf **Externen DPM hinzufügen** (links oben auf dem Bildschirm).   
    ![Externen DPM hinzufügen](./media/backup-azure-alternate-dpm-server/add-external-dpm.png)
2. Laden Sie die neuen **Tresoranmeldeinformationen** des mit dem Server von **Azure Backup Server** verknüpften Tresors herunter, wählen Sie den Server von Azure Backup Server aus der Liste der beim Sicherungstresor registrierten DPM-Server aus, und geben Sie die **Verschlüsselungspassphrase** für den Server von Azure Backup Server an, dessen Daten wiederhergestellt werden.

    ![Anmeldeinformationen des externen DPM](./media/backup-azure-alternate-dpm-server/external-dpm-credentials.png)

   > [!NOTE]
   > Sie können nur Daten zwischen Servern von Azure Backup Servern wiederherstellen, die bei demselben Sicherungstresor registriert sind.
   >
   >

    Sobald der externe Server von Azure Backup Server erfolgreich hinzugefügt wurde, können Sie die Daten vom externen Server von Azure Backup Server und vom lokalen Server von Azure Backup Server auf der Registerkarte **Wiederherstellung** durchsuchen.
3. Durchsuchen Sie die Liste der Produktionsserver, die durch den externen Server von Azure Backup Server geschützt werden, und wählen Sie die entsprechende Datenquelle aus.

    ![Durchsuchen des externen DPM-Servers](./media/backup-azure-alternate-dpm-server/browse-external-dpm.png)
4. Wählen Sie nacheinander **Monat und Jahr** aus der Dropdownliste **Wiederherstellungspunkte**, das erforderliche **Wiederherstellungsdatum** der Erstellung des Wiederherstellungspunkts sowie die **Wiederherstellungszeit** aus.

    Im unteren Bereich wird eine Liste von Dateien und Ordnern angezeigt, die Sie durchsuchen und an einem beliebigen Ort wiederherstellen können.

    ![Wiederherstellungspunkte des externen DPM-Servers](./media/backup-azure-alternate-dpm-server/external-dpm-recoverypoint.png)
5. Klicken Sie mit der rechten Maustaste auf das entsprechende Element, und klicken Sie dann auf **Wiederherstellen**.

    ![Externe DPM-Wiederherstellung](./media/backup-azure-alternate-dpm-server/recover.png)
6. Überprüfen Sie die **Wiederherstellungsauswahl**. Überprüfen Sie Datum und Uhrzeit der wiederhergestellten Sicherungskopie sowie die Quelle, aus der die Sicherungskopie erstellt wurde. Wenn die Auswahl fehlerhaft ist, klicken Sie auf **Abbrechen** , und navigieren Sie zur Registerkarte "Wiederherstellung", um dort den richtigen Wiederherstellungspunkt auszuwählen. Wenn die Auswahl richtig ist, klicken Sie auf **Weiter**.

    ![Zusammenfassung zur externen DPM-Wiederherstellung](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-summary.png)
7. Wählen Sie **An anderem Speicherort wiederherstellen**aus. **Durchsuchen** den richtigen Speicherort für die Wiederherstellung aus.

    ![Alternativer Speicherort für externe DPM-Wiederherstellung](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-alternate-location.png)
8. Wählen Sie die gewünschte Option aus: **Kopie erstellen**, **Überspringen** oder **Überschreiben**.

   * **Kopie erstellen** erstellt eine Kopie der Datei, falls ein Namenskonflikt besteht.
   * **Überspringen** überspringt die Wiederherstellung der Datei, falls ein Namenskonflikt besteht.
   * **Überschreiben** überschreibt die vorhandene Kopie am angegebenen Speicherort, falls ein Namenskonflikt besteht.

     Wählen Sie die entsprechende Option für **Sicherheit wiederherstellen**aus. Sie können die Sicherheitseinstellungen des Zielcomputers anwenden, auf dem die Daten wiederhergestellt werden, oder die Sicherheitseinstellungen, die für das Produkt zum Zeitpunkt der Erstellung des Wiederherstellungspunkts galten.

     Geben Sie an, ob eine **Benachrichtigung** gesendet werden soll, sobald die Wiederherstellung erfolgreich abgeschlossen wurde.

     ![Benachrichtigungen für externe DPM-Wiederherstellung](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-notifications.png)
9. Auf dem Bildschirm **Zusammenfassung** werden die bisher ausgewählten Optionen aufgelistet. Nach dem Klicken auf **Wiederherstellen**werden die Daten am entsprechenden lokalen Speicherort wiederhergestellt.

    ![Zusammenfassung der Optionen für die externe DPM-Wiederherstellung](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-options-summary.png)

   > [!NOTE]
   > Der Wiederherstellungsauftrag kann auf der Registerkarte **Überwachung** des Servers von Azure Backup Server überwacht werden.
   >
   >

    ![Überwachen der Wiederherstellung](./media/backup-azure-alternate-dpm-server/monitoring-recovery.png)
10. Klicken Sie auf der Registerkarte **Wiederherstellung** des DPM-Servers auf **Externen DPM löschen**, um die Ansicht des externen DPM-Servers zu entfernen.

    ![Löschen des externen DPM](./media/backup-azure-alternate-dpm-server/clear-external-dpm.png)

## <a name="troubleshooting-error-messages"></a>Problembehandlung bei Fehlermeldungen
| Nein. | Fehlermeldung | Schritte zur Problembehandlung |
|:---:|:--- |:--- |
| 1. |Dieser Server ist nicht bei dem Tresor registriert, der durch die Tresoranmeldeinformationen angegeben ist. |**Ursache**: Dieser Fehler wird angezeigt, wenn die ausgewählte Datei mit den Tresoranmeldeinformationen nicht zu dem Sicherungstresor gehört, dem der Server von Azure Backup Server zugeordnet ist, auf dem die Wiederherstellung versucht wird. <br> **Lösung**: Laden Sie die Anmeldeinformationsdatei für den Tresor aus dem Sicherungstresor herunter, bei dem der Server von Azure Backup Server registriert ist. |
| 2. |Die wiederherzustellenden Daten sind nicht verfügbar, oder der ausgewählte Server ist kein DPM-Server. |**Ursache**: Es sind keine anderen Server von Azure Backup Server bei dem Sicherungstresor registriert, oder die Server von Azure Backup Server haben ihre Metadaten noch nicht hochgeladen, oder der ausgewählte Server ist kein Server von Azure Backup Server (also kein Windows-Server oder Windows-Client). <br> **Lösung**: Wenn noch andere Server von Azure Backup Server bei dem Sicherungstresor registriert sind, müssen Sie sicherstellen, dass der neueste Azure Backup-Agent installiert ist. <br>Sind noch andere Server von Azure Backup Server bei dem Sicherungstresor registriert, warten Sie nach der Installation einen Tag, um den Wiederherstellungsprozess zu starten. Während der Nacht werden die Metadaten für geschützte Sicherungen in die Cloud hochgeladen. Die Daten sind dann für die Wiederherstellung verfügbar. |
| 3. |Es sind keine anderen DPM-Server bei diesem Tresor registriert. |**Ursache:** Bei dem Tresor, von dem aus die Wiederherstellung versucht wird, sind keine weiteren Server von Azure Backup Server registriert.<br>**Lösung**: Wenn noch andere Server von Azure Backup Server bei dem Sicherungstresor registriert sind, müssen Sie sicherstellen, dass der neueste Azure Backup-Agent installiert ist.<br>Sind noch andere Server von Azure Backup Server bei dem Sicherungstresor registriert, warten Sie nach der Installation einen Tag, um den Wiederherstellungsprozess zu starten. Während der Nacht werden die Metadaten für geschützte Sicherungen in die Cloud hochgeladen. Die Daten sind dann für die Wiederherstellung verfügbar. |
| 4. |Die angegebene Verschlüsselungspassphrase stimmt nicht mit der Passphrase überein, die dem folgenden Server zugeordnet ist: **<server name>** |**Ursache:** Die beim Verschlüsseln der wiederherzustellenden Daten auf dem Server von Azure Backup Server verwendete Verschlüsselungspassphrase stimmt nicht mit der angegebenen Verschlüsselungspassphrase überein. Der Agent kann die Daten nicht entschlüsseln. Daher schlägt die Wiederherstellung fehl.<br>**Lösung:** Geben Sie genau dieselbe Verschlüsselungspassphrase an, die dem Server von Azure Backup Server zugeordnet ist, dessen Daten wiederhergestellt werden sollen. |

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen:
1. **Warum kann ich nach der Installation von UR7 und dem neuesten Azure Backup-Agent keinen externen DPM-Server von einem anderen DPM-Server hinzufügen? (Gilt für den Fall, dass Sie SC DPM 2012 R2 verwenden)**

    A) Für die vorhandene DPM-Server mit Datenquellen, die in der Cloud (mithilfe eines Updaterollups vor Update Rollup 7) geschützt sind, müssen Sie mindestens einen Tag nach der Installation von UR7 und des neuesten Azure Backup-Agents warten, bevor Sie *externe DPM-Server hinzufügen*. Diese Zeit ist erforderlich, um die Metadaten der DPM-Schutzgruppen in Azure hochzuladen. Dies erfolgt das erste Mal während der Nacht.
2. **Welches ist die erforderliche Mindestversion des Azure Backup-Agents?**

    A) Dieses Feature ist ab der Version 2.0.8719.0 des Azure Backup-Agents verfügbar.  Sie können die Version des Azure Backup-Agents überprüfen, indem Sie zu „Systemsteuerung“ **>** „Alle Systemsteuerungselemente“ **>** „Programme und Funktionen“ **>** „Microsoft Azure Recovery Services-Agent“ navigieren. Wenn die Version kleiner als 2.0.8719.0 ist, laden Sie den [neuesten Azure Backup-Agent](https://go.microsoft.com/fwLink/?LinkID=288905) herunter und installieren ihn.

    ![Löschen des externen DPM](./media/backup-azure-alternate-dpm-server/external-dpm-azurebackupagentversion.png)

## <a name="next-steps"></a>Nächste Schritte:
•    [Azure Backup – Häufig gestellte Fragen](backup-azure-backup-faq.md)

