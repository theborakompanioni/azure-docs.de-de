---
title: Installieren von Azure Backup Server v2 | Microsoft-Dokumentation
description: "Azure Backup Server v2 bietet Ihnen erweiterte Sicherungsfunktionen für den Schutz von u.a. virtuellen Computern, Dateien, Ordner und Workloads. Erfahren Sie, wie Sie Azure Backup Server v2 installieren oder ein Upgrade auf diese Version ausführen."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: masaran;markgal
ms.translationtype: HT
ms.sourcegitcommit: 9633e79929329470c2def2b1d06d95994ab66e38
ms.openlocfilehash: 1bbb16afef7940933b4c3ae23873f212770137e0
ms.contentlocale: de-de
ms.lasthandoff: 08/04/2017

---

# <a name="install-azure-backup-server-v2"></a>Installieren von Azure Backup Server v2

Azure Backup Server schützt Ihre virtuellen Computer (VMs), Workloads, Dateien und Ordner und vieles mehr. Azure Backup Server v2 baut auf Azure Backup Server v1 auf und bietet neue Funktionen, die in v1 nicht verfügbar sind. Einen Vergleich der Funktionen von v1 und v2 finden Sie unter [Azure Backup Server-Schutzmatrix](backup-mabs-protection-matrix.md). 

Die zusätzlichen Funktionen in Backup Server v2 sind ein Upgrade von Backup Server v1. Allerdings ist Backup Server v1 keine Voraussetzung für die Installation von Backup Server v2. Wenn Sie Backup Server v1 auf Backup Server v2 aktualisieren, installieren Sie Backup Server v2 auf dem Backup Server-Schutzserver. Ihre vorhandenen Backup Server-Einstellungen bleiben intakt.

Sie können Backup Server v2 unter Windows Server 2012 R2 oder Windows Server 2016 installieren. Um neue System Center 2016 Data Protection Manager-Funktionen wie Modern Backup Storage nutzen zu können, müssen Sie Backup Server v2 unter Windows Server 2016 installieren. Machen Sie sich vor der Installation von oder einem Upgrade auf Backup Server v2 mit den [Installationsvoraussetzungen](https://docs.microsoft.com/system-center/dpm/install-dpm#setup-prerequisites) vertraut.

> [!NOTE]
> Azure Backup Server und System Center Data Protection Manager haben dieselbe Codebasis. Backup Server v1 ist identisch mit Data Protection Manager 2012 R2, Backup Server v2 mit Data Protection Manager 2016. In diesem Artikel wird gelegentlich auf die Data Protection Manager-Dokumentation verwiesen.
>
>

## <a name="upgrade-backup-server-to-v2"></a>Upgrade von Azure Backup Server auf v2
Stellen Sie für ein Upgrade von Backup Server v1 auf Backup Server v2 sicher, dass die Installation über die erforderlichen Updates verfügt:

- [Aktualisieren Sie die Schutz-Agents](backup-mabs-upgrade-to-v2.md#update-the-dpm-protection-agent) auf den geschützten Servern.
- Aktualisieren Sie Windows Server 2012 R2 auf Windows Server 2016.
- Aktualisieren Sie den Azure Backup Server-Remoteadministrator auf allen Produktionsservern.
- Stellen Sie sicher, dass Sicherungen auf Fortsetzen ohne Neustarten Ihres Produktionsservers festgelegt sind.


### <a name="upgrade-steps-for-backup-server-v2"></a>Upgradeschritte für Backup Server v2

1. Sie müssen im Download Center [das Installationsprogramm des Upgrades herunterladen](https://go.microsoft.com/fwlink/?LinkId=626082).

2. Nachdem Sie den Setup-Assistenten extrahiert haben, stellen sicher, dass **setup.exe ausführen** ausgewählt ist, und wählen Sie dann **Fertig stellen**.

  ![Installationsprogramm für Setup: Setup ausführen](./media/backup-mabs-upgrade-to-v2/run-setup.png)

3. Wählen Sie im Microsoft Azure Backup Server-Assistenten unter **Installieren** die Option **Microsoft Azure Backup Server** aus.

  ![Installationsprogramm für Setup: „Installieren“ auswählen](./media/backup-mabs-upgrade-to-v2/mabs-installer-s1.png)

4. Überprüfen Sie auf der **Startseite** die Warnungen, und klicken Sie dann auf **Weiter**.

  ![Installationsprogramm für Setup: Startseite](./media/backup-mabs-upgrade-to-v2/mabs-installer-s2.png)

5. Der Setup-Assistent führt Voraussetzungsprüfungen aus, um sicherzustellen, dass ein Upgrade Ihrer Umgebung erfolgen kann. Wählen Sie auf der Seite **Überprüfungen der Voraussetzungen** die Option **Prüfen** aus.

  ![Installationsprogramm für Setup: Seite „Überprüfungen der Voraussetzungen“](./media/backup-mabs-upgrade-to-v2/mabs-installer-s3-perform-checks.png)

6. Ihre Umgebung muss die Voraussetzungsprüfungen bestehen. Falls Ihre Umgebung die Prüfungen nicht besteht, bestimmen Sie die Probleme, und korrigieren Sie sie. Wählen Sie dann **Erneut prüfen** aus. Sobald Sie die Überprüfungen der Voraussetzungen bestanden haben, klicken Sie auf **Weiter**.

  ![Installationsprogramm für Setup: Schaltfläche „Erneut prüfen“](./media/backup-mabs-upgrade-to-v2/mabs-installer-s4-pass-checks.png)

7. Wählen Sie auf der Seite **SQL-Einstellungen** die Ihrer SQL-Installation entsprechende Option aus, und klicken Sie dann auf **Prüfen und installieren**.

  ![Installationsprogramm für Setup: Seite „SQL-Einstellungen“](./media/backup-mabs-upgrade-to-v2/mabs-installer-s5-sql-settings.png)

  Die Prüfungen können einige Minuten dauern. Wenn die Prüfungen abgeschlossen sind, klicken Sie auf **Weiter**.

  ![Installationsprogramm für Setup: Schaltfläche „Prüfen und installieren“ auf der Seite „SQL-Einstellungen“](./media/backup-mabs-upgrade-to-v2/mabs-installer-s5a-check-and fix-settings.png)

8. Ändern Sie auf der Seite **Installationseinstellungen** den Speicherort in den Installationspfad von Backup Server oder in das Scratchverzeichnis. Klicken Sie auf **Weiter**.

  ![Installationsprogramm für Setup: Seite „Installationseinstellungen“](./media/backup-mabs-upgrade-to-v2/mabs-installer-s6-installation-settings.png)

9. Klicken Sie zum Beenden des Setup-Assistenten auf **Fertig stellen**.

  ![Installationsprogramm für Setup: Fertig stellen](./media/backup-mabs-upgrade-to-v2/run-setup.png)



## <a name="add-storage-for-modern-backup-storage"></a>Hinzufügen von Speicher für Modern Backup Storage

Um die Effizienz der Speicherung von Sicherungen zu verbessern, unterstützt Backup Server v2 nun Volumes. Wie Backup Server v1 unterstützt Backup Server v2-Datenträger.

### <a name="add-volumes-and-disks"></a>Hinzufügen von Volumes und Datenträgern
Wenn Sie Backup Server v2 unter Windows Server 2016 ausführen, können Sie zum Speichern von Sicherungsdaten Volumes nutzen. Volumes ermöglichen Speichereinsparungen und schnellere Sicherungen. Da Volumes in Backup Server neu sind, müssen Sie sie hinzufügen. 

Wenn Sie Backup Server ein Volume hinzufügen, können Sie dem Volume einen Anzeigenamen geben. Klicken Sie auf die Spalte **Anzeigename** des Volumes, um es zu benennen. Sie können den Namen später bei Bedarf ändern. Sie können auch mit PowerShell Anzeigenamen für Volumes hinzufügen oder ändern.

So fügen Sie in der Administratorkonsole ein Volume hinzu

1. Wählen Sie in der Azure Backup Server-Administratorkonsole **Verwaltung** > **Datenspeicher** > **Hinzufügen** aus.

    ![Öffnen des Assistenten „Datenspeicher hinzufügen“](./media//backup-mabs-upgrade-to-v2/open-add-disk-storage-wizard.png)

    Der Assistent „Datenspeicher hinzufügen“ wird geöffnet.

2. Wählen Sie auf der Seite **Datenspeicher hinzufügen** im Feld **Verfügbare Volumes** ein Volume aus, und klicken Sie dann auf **Hinzufügen**.
3. Geben Sie in das Feld **Ausgewählte Volumes** einen Anzeigenamen für das Volume ein, und klicken Sie dann auf **OK**.

      ![Assistent „Datenspeicher hinzufügen“: Volume hinzufügen](./media/backup-mabs-upgrade-to-v2/add-volume.png)

  Wenn Sie einen Datenträger hinzufügen möchten, muss der Datenträger zu einer Schutzgruppe gehören, die über Legacyspeicher verfügt. Diese Datenträger können nur für diese Schutzgruppen verwendet werden. Falls Backup Server keine Quellen mit Legacyschutz hat, wird der Datenträger nicht aufgeführt.

  Weitere Informationen zum Hinzufügen von Datenträgern finden Sie unter [Hinzufügen von Datenträgern zum Vergrößern des Legacyspeichers](http://docs.microsoft.com/system-center/dpm/upgrade-to-dpm-2016#adding-disks-to-increase-legacy-storage). Ein Datenträger kann nicht mit einem Anzeigenamen versehen werden.


### <a name="assign-workloads-to-volumes"></a>Zuweisen von Workloads zu Volumes

Geben Sie Backup Server an, welche Workloads welchen Volumes zugewiesen werden. Sie können beispielsweise kostenintensive Volumes festlegen, die eine hohe Anzahl von Ein- und Ausgabevorgängen pro Sekunde (IOPS) unterstützen, um nur Workloads zu speichern, die häufige Sicherungen mit großem Volumen erfordern. Ein Beispiel ist SQL Server mit Transaktionsprotokollen.

#### <a name="update-dpmdiskstorage"></a>Update-DPMDiskStorage

Um die Eigenschaften eines Volumes im Speicherpool in Backup Server zu aktualisieren, verwenden Sie das PowerShell-Cmdlet „Update-DPMDiskStorage“.

Syntax:

`Parameter Set: Volume`

```
Update-DPMDiskStorage [-Volume] <Volume> [[-FriendlyName] <String> ] [[-DatasourceType] <VolumeTag[]> ] [-Confirm] [-WhatIf] [ <CommonParameters>]
```

Alle Änderungen, die Sie mithilfe von PowerShell vornehmen, werden auf der Benutzeroberfläche übernommen.


## <a name="protect-data-sources"></a>Schützen von Datenquellen
Erstellen Sie eine Schutzgruppe, um mit dem Schützen von Datenquellen zu beginnen. In den folgenden Schritten werden Änderungen und Ergänzungen am Assistenten für neue Schutzgruppen vorgestellt.

So erstellen Sie eine Schutzgruppe

1. Wählen Sie in der Backup Server-Verwaltungskonsole **Schutz** aus.

2. Wählen Sie auf dem Menüband **Neu** aus.

    Der Assistent zum Erstellen einer neuen Schutzgruppe wird geöffnet.

  ![Assistent zum Erstellen einer neuen Schutzgruppe](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-1.png)

3. Wählen Sie auf der Seite **Willkommen** die Option **Weiter** aus.
4. Wählen Sie auf der Seite **Schutzgruppentyp auswählen** den Typ der zu erstellenden Schutzgruppe aus, und klicken Sie dann auf **Weiter**.

  ![Seite „Schutzgruppentyp auswählen“](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-2.png)

5. Auf der Seite **Gruppenmitglieder auswählen** werden im Bereich **Verfügbare Mitglieder** die Mitglieder mit Schutz-Agents aufgelistet. Wählen Sie in diesem Beispiel die Volumes „D:\“ und „E:\“ aus, und fügen Sie beide dem Bereich **Ausgewählte Mitglieder** hinzu. Klicken Sie auf **Weiter**.

  ![Seite „Gruppenmitglieder auswählen“](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-3.png)

6. Geben Sie auf der Seite **Datenschutzmethode auswählen** den **Namen der Schutzgruppe** ein, wählen Sie die Schutzmethode aus, und klicken Sie dann auf **Weiter**. Wenn Sie kurzfristigen Schutz wünschen, müssen Sie die Sicherungsmethode **Datenträger** auswählen.

  ![Seite „Datenschutzmethode auswählen“](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-4.png)

7. Wählen Sie auf der Seite **Kurzfristige Ziele angeben** die Details für **Beibehaltungsdauer** und **Synchronisierungsfrequenz** aus. Klicken Sie anschließend auf **Weiter**. Um den Zeitplan für das Erstellen von Wiederherstellungspunkten zu ändern, wählen Sie optional **Ändern** aus.

  ![Seite „Kurzfristige Ziele angeben“](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-5.png)

8. Überprüfen Sie auf der Seite **Datenspeicherzuordnung überprüfen** die Details der Datenquellen, die Sie ausgewählt haben, ihre Größe, die Werte für den Speicherplatz, der bereitgestellt werden soll, und das Zielspeichervolume.

  ![Seite „Datenspeicherzuordnung überprüfen“](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-6.png)

  Speichervolumes basieren auf der (mithilfe von PowerShell) festgelegten Zuordnung von Volumes für Workloads und dem verfügbaren Speicherplatz. Sie können die Speichervolumes ändern, indem Sie im Dropdownmenü andere Volumes auswählen. Wenn Sie den Wert für **Zielspeicher** ändern, wird der Wert von **Verfügbarer Datenspeicher** dynamisch zum Wiedergeben der Werte unter **Freier Speicherplatz** und **Underprovisioned Space** (Nicht ausreichend bereitgestellter Speicherplatz) geändert.

  Wenn die Datenquellen wie geplant anwachsen, gibt der Wert in der Spalte **Underprovisioned Space** (Nicht ausreichend bereitgestellter Speicherplatz) unter **Verfügbarer Datenspeicher** die Menge an zusätzlichem Speicher an, die erforderlich ist. Planen Sie mithilfe dieses Werts Ihre Speicheranforderungen für reibungslose Sicherungen. Wenn der Wert 0 (null) ist, sind in naher Zukunft keine potenziellen Probleme mit Speicher zu erwarten. Wenn der Wert ungleich 0 (null) ist, haben Sie (basierend auf Ihrer Schutzrichtlinie und der Datengröße Ihre geschützten Mitglieder) nicht genügend Speicher zugeteilt.

  ![Nicht ausreichend zugeordneter Datenträgerspeicher](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-7.png)

   Schließen Sie den Assistenten ab, um das Erstellen Ihrer Schutzgruppe fertig zu stellen.

## <a name="migrate-legacy-storage-to-modern-backup-storage"></a>Migrieren von Legacyspeicher zu Modern Backup Storage
Nach dem Upgrade auf oder der Installation von Backup Server v2 und dem Upgrade des Betriebssystems auf Windows Server 2016 müssen Sie Ihre Schutzgruppen für die Verwendung von Modern Backup Storage aktualisieren. Standardmäßig werden Schutzgruppen nicht geändert. Sie funktionieren weiterhin, wie sie ursprünglich eingerichtet wurden. 

Das Aktualisieren von Schutzgruppen zur Verwendung von Modern Backup Storage ist optional. Beenden Sie zum Aktualisieren der Schutzgruppe den Schutz aller Datenquellen mithilfe der Option „Daten beibehalten“. Fügen Sie dann die Datenquellen einer neuen Schutzgruppe hinzu.

1. Wählen Sie in der Backup Server-Administratorkonsole **Schutz** aus. Klicken Sie in der Liste **Schutzgruppenmitglied** mit der rechten Maustaste auf das Mitglied, und wählen Sie dann **Schutz des Mitglieds beenden** aus.

  ![Schutz des Mitglieds beenden](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-stop-protection1.png)

2. Überprüfen Sie im Dialogfeld **Aus Gruppe entfernen** den belegten Speicherplatz und den verfügbaren freien Speicherplatz für den Speicherpool. Die Standardeinstellung ist das Belassen der Wiederherstellungspunkte auf dem Datenträger und sie gemäß der zugehörigen Beibehaltungsrichtlinie ablaufen zu lassen. Klicken Sie auf **OK**.

  Wenn der belegte Speicherplatz sofort an den Pool mit freiem Speicherplatz zurückgegeben werden soll, aktivieren Sie das Kontrollkästchen **Replikat auf Datenträger löschen**, um die Sicherungsdaten (und Wiederherstellungspunkte) zu löschen, die zu diesem Mitglied gehören.

  ![Dialogfeld „Aus Gruppe entfernen“](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-retain-data.png)

3. Erstellen Sie eine Schutzgruppe, die Modern Backup Storage verwendet. Fügen Sie die ungeschützten Datenquellen hinzu.


## <a name="add-disks-to-increase-legacy-storage"></a>Hinzufügen von Datenträgern zum Vergrößern des Legacyspeichers

Wenn Sie Legacyspeicher mit Backup Server verwenden möchten, müssen Sie möglicherweise Datenträger hinzufügen, um den Legacyspeicher zu vergrößern. 

So fügen Sie Datenspeicher hinzu

1. Wählen Sie in der Verwaltungskonsole **Verwaltung** > **Datenspeicher** > **Hinzufügen** aus.

    ![Dialogfeld „Datenspeicher hinzufügen“](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-add-disk-storage.png)

4. Wählen Sie im Dialogfeld **Datenspeicher hinzufügen** den Befehl **Datenträger hinzufügen** aus.

5. Wählen Sie in der Liste der verfügbaren Datenträger die Datenträger aus, die Sie hinzufügen möchten. Klicken Sie auf **Hinzufügen** und dann auf **OK**.

## <a name="update-the-data-protection-manager-protection-agent"></a>Aktualisieren des Data Protection Manager-Schutz-Agents

Backup Server verwendet für Updates den System Center Data Protection Manager-Schutz-Agent. Wenn Sie einen Schutz-Agent aktualisieren, der nicht mit dem Netzwerk verbunden ist, können Sie ein Upgrade des Agents nicht mithilfe der Data Protection Manager-Administratorkonsole durchführen. Sie müssen den Schutz-Agent in einer nicht aktiven Domänenumgebung aktualisieren. Bis zum Verbinden des Clientcomputers mit dem Netzwerk wird in der Data Protection Manager-Administratorkonsole angezeigt, dass das Update des Schutz-Agents aussteht.

In den folgenden Abschnitten wird beschrieben, wie Schutz-Agents für Clientcomputer aktualisiert werden, die mit dem Netzwerk verbunden bzw. nicht verbunden sind.

### <a name="update-a-protection-agent-for-a-connected-client-computer"></a>Aktualisieren eines Schutz-Agents auf einem mit dem Netzwerk verbundenen Clientcomputer

1. Wählen Sie in der Backup Server-Administratorkonsole **Verwaltung** > **Agents** aus.

2. Wählen Sie im Anzeigebereich die Clientcomputer aus, für die Sie den Schutz-Agent aktualisieren möchten.

  > [!NOTE]
  > Die Spalte **Agent-Updates** gibt an, wann ein Update des Schutz-Agents für jeden geschützten Computer verfügbar ist. Im Bereich **Aktionen** ist die Aktion **Aktualisieren** nur verfügbar, wenn ein geschützter Computer ausgewählt ist und Updates verfügbar sind.
  >
  >

3. Zum Installieren aktualisierter Schutz-Agents auf den ausgewählten Computern wählen Sie im Bereich **Aktionen** den Befehl **Aktualisieren** aus.

### <a name="update-a-protection-agent-on-a-client-computer-that-is-not-connected"></a>Aktualisieren eines Schutz-Agents auf einem Clientcomputer, der nicht mit dem Netzwerk verbunden ist

1. Wählen Sie in der Backup Server-Administratorkonsole **Verwaltung** > **Agents** aus.

2. Wählen Sie im Anzeigebereich die Clientcomputer aus, für die Sie den Schutz-Agent aktualisieren möchten.

  > [!NOTE]
   > Die Spalte **Agent-Updates** gibt an, wann ein Update des Schutz-Agents für jeden geschützten Computer verfügbar ist. Im Bereich **Aktionen** ist die Aktion **Aktualisieren** nicht verfügbar, wenn ein geschützter Computer ausgewählt ist, es sei denn, Updates sind verfügbar.
  >
  >

3. Zum Installieren aktualisierter Schutz-Agents auf den ausgewählten Computern wählen Sie **Aktualisieren** aus.

4. Für einen Clientcomputer, der nicht mit dem Netzwerk verbunden ist, enthält die Spalte **Agent-Status** so lange den Status **Update ausstehend**, bis der Computer mit dem Netzwerk verbunden wird.

  Sobald ein Clientcomputer mit dem Netzwerk verbunden ist, enthält die Spalte **Agent-Updates** für den Clientcomputer den Status **Wird aktualisiert**.
  
### <a name="move-legacy-protection-groups-from-old-version-and-sync-the-new-version-with-azure"></a>Verschieben von älteren Schutzgruppen aus der alten Version und Synchronisieren der neuen Version mit Azure

Nachdem sowohl Azure Backup Server als auch das Betriebssystem aktualisiert wurden, können Sie neue Datenquellen per Modern Backup Storage schützen. Für bereits geschützte Datenquellen gilt aber weiterhin der Legacyschutz wie unter Azure Backup Server, während für alle neuen Schutzvorgänge Modern Backup Storage genutzt wird.

Die unten angegebenen Schritte dienen zum Migrieren von Datenquellen aus dem Legacyschutzmodus zu Modern Backup Storage.

• Fügen Sie die neuen Volumes dem DPM-Speicherpool hinzu, und weisen Sie Anzeigenamen und Datenquellentags hinzu, falls erforderlich.
• Beenden Sie für alle Datenquellen, die sich im Legacymodus befinden, den Schutz der Datenquellen, und wählen Sie „Geschützte Daten beibehalten“.  Dies ermöglicht die Wiederherstellung alter Wiederherstellungspunkte nach der Migration.

• Erstellen Sie eine neue Schutzgruppe, und wählen Sie die Datenquellen aus, die im neuen Format gespeichert werden sollen.
• DPM führt lokal einen Replikatkopiervorgang aus dem Legacysicherungsspeicher auf das Modern Backup Storage-Volume durch.
Hinweis: Dies wird als Auftrag nach dem Wiederherstellungsvorgang angesehen. • Alle neuen Synchronisierungs- und Wiederherstellungspunkte werden dann in Modern Backup Storage gespeichert.
• Alte Wiederherstellungspunkte werden ausgesondert, wenn sie ablaufen, damit Speicherplatz auf dem Datenträger freigegeben wird.
• Nachdem alle Legacyvolumes aus dem alten Speicher gelöscht wurden, kann der Datenträger aus Azure Backup und vom System entfernt werden.
• Erstellen Sie eine Sicherung der Azure DPMDB.

Teil 2: - Wichtige Elemente: Der neue Server muss den gleichen Namen wie der ursprüngliche Azure Backup-Server erhalten. Sie können den Namen des neuen Azure Backup-Servers nicht ändern, wenn Sie den alten Speicherpool und DPMDB zum Beibehalten von Wiederherstellungspunkten verwenden möchten. - Es muss eine Sicherung von DPMDB vorhanden sein, da eine Wiederherstellung durchgeführt werden muss.

1) Fahren Sie den ursprünglichen Azure Backup-Server herunter, oder stellen Sie ihn außer Dienst.
2) Setzen Sie das Computerkonto in Active Directory zurück.
3) Installieren Sie Server 2016 auf dem neuen Computer, und vergeben Sie den gleichen Namen wie für den ursprünglichen Azure Backup-Server.
4) Führen Sie den Domänenbeitritt durch.
5) Installieren Sie Azure Backup Server V2. (Verschieben Sie den DPM-Speicherpool vom alten Server, und führen Sie den Import durch.)
6) Stellen Sie die DPMDB vom Ende von Teil 2 wieder her.
7) Fügen Sie den Speicher vom ursprünglichen Sicherungsserver an den neuen Server an.
8) Stellen Sie die DPMDB über SQL wieder her.
9) Wechseln Sie in der Administrator-Befehlszeile auf dem neuen Server per „cd“ zum Installationsspeicherort von Microsoft Azure Backup und in den Ordner „bin“.

Pfadbeispiel: C:\windows\system32>cd „c:\Programme\Microsoft Azure Backup\DPM\DPM\bin\“
Zu Azure Backup: DPMSYNC -SYNC ausführen

10) Führen Sie DPMSYNC -SYNC aus. Hinweis: Wenn Sie dem DPM-Speicherpool NEUE Datenträger hinzugefügt haben, anstatt alte zu verschieben, müssen Sie Folgendes ausführen: „DPMSYNC -Reallocatereplica“.

## <a name="new-powershell-cmdlets-in-v2"></a>Neue PowerShell-Cmdlets in v2

Nach der Installation von Azure Backup Server v2 stehen zwei neue Cmdlets zur Verfügung: 
* [Mount-DPMRecoveryPoint](https://technet.microsoft.com/library/mt787159.aspx)
* [Dismount-DPMRecoveryPoint](https://technet.microsoft.com/library/mt787158.aspx)

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie Ihren Server vorbereiten oder mit dem Schutz einer Workload beginnen:
- [Vorbereiten von Backup Server-Workloads](backup-azure-microsoft-azure-backup.md)
- [Sichern eines VMware-Servers mit Backup Server](backup-azure-backup-server-vmware.md)
- [Sichern von SQL Server mit Backup Server](backup-azure-sql-mabs.md)
- [Verwenden von Modern Backup Storage mit Backup Server](backup-mabs-add-storage.md)


