---
title: Verwenden von Modern Backup Storage mit Azure Backup Server v2 | Microsoft-Dokumentation
description: Informationen zu den neuen Funktionen in Azure Backup Server v2. Dieser Artikel beschreibt, wie Sie Ihre Backup-Server-Installation aktualisieren.
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
ms.translationtype: Human Translation
ms.sourcegitcommit: a1ba750d2be1969bfcd4085a24b0469f72a357ad
ms.openlocfilehash: 751b9b495fd368dff1f72429707f5f33a0ccb569
ms.contentlocale: de-de
ms.lasthandoff: 06/20/2017

---

# <a name="add-storage-to-azure-backup-server-v2"></a>Hinzufügen von Speicher zu Azure Backup Server v2

Azure Backup Server v2 bietet die System Center 2016 Data Protection Manager-Funktion Modern Backup Storage. Modern Backup Storage ermöglicht Speichereinsparungen von bis zu 50 %, dreimal schnellere Sicherungen und eine effizientere Speicherung. Zudem wird workloadorientierter Speicher geboten. 

> [!NOTE]
> Zum Verwenden von Modern Backup Storage müssen Sie Backup Server v2 unter Windows Server 2016 ausführen. Wenn Sie Backup Server v2 unter einer früheren Version von Windows Server ausführen, kann Modern Backup Storage nicht von Azure Backup Server genutzt werden. Workloads werden dann wie mit Backup Server v1 geschützt. Weitere Informationen finden Sie in der [Schutzmatrix](backup-mabs-protection-matrix.md) der Backup Server-Versionen.

## <a name="volumes-in-backup-server-v2"></a>Volumes in Backup Server v2

Backup Server v2 unterstützt Speichervolumes. Wenn Sie ein Volume hinzufügen, formatiert Azure Backup das Volume mit ReFS (Resilient File System), was für Modern Backup Storage erforderlich ist. Um ein Volume hinzuzufügen und es zu einem späteren Zeitpunkt zu erweitern, sollten Sie diesen Workflow befolgen:

1.  Richten Sie Azure Backup v2 auf einem virtuellen Computer ein.
2.  Erstellen Sie ein Volume auf einem virtuellen Datenträger in einem Speicherpool:
    1.  Fügen Sie einen Datenträger zu einem Speicherpool hinzu, und erstellen Sie einen virtuellen Datenträger mit einfachem Layout.
    2.  Fügen Sie zusätzliche Datenträger hinzu, und erweitern Sie den virtuellen Datenträger.
    3.  Erstellen Sie Volumes auf dem virtuellen Datenträger.
3.  Fügen Sie Backup Server die Volumes hinzu.
4.  Konfigurieren Sie workloadorientierten Speicher.

## <a name="create-a-volume-for-modern-backup-storage"></a>Erstellen eines Volumes für Modern Backup Storage

Durch Verwenden von Backup Server v2 mit Volumes als Datenträgerspeicher können Sie besser die Kontrolle über die Speicherung behalten. Ein Volume kann ein einzelner Datenträger sein. Wenn Sie jedoch den Speicher in Zukunft erweitern möchten, erstellen Sie ein Volume aus einem mit dem Feature „Speicherplätze“ erstellten Datenträger. Dies kann hilfreich sein, wenn Sie das Volume für Sicherungsspeicher erweitern möchten. Dieser Abschnitt bietet bewährte Methoden zum Erstellen eines Volumes mit diesem Setup.

1. Wählen Sie in Server-Manager nacheinander **Datei- und Speicherdienste** > **Volumes** > **Speicherpools** aus. Klicken Sie unter **PHYSISCHE DATENTRÄGER** auf **Neuer Speicherpool**. 

    ![Erstellen eines neuen Speicherpools](./media/backup-mabs-add-storage/mabs-add-storage-1.png)

2. Klicken Sie in der Dropdownliste **AUFGABEN** auf **Neuer virtueller Datenträger**.

    ![Hinzufügen eines virtuellen Datenträgers](./media/backup-mabs-add-storage/mabs-add-storage-2.png)

3. Wählen Sie den Speicherpool aus, und klicken Sie dann auf **Physischen Datenträger hinzufügen**.

    ![Hinzufügen eines physischen Datenträgers](./media/backup-mabs-add-storage/mabs-add-storage-3.png)

4. Wählen Sie den physischen Datenträger aus, und klicken Sie dann auf **Virtuellen Datenträger erweitern**.

    ![Erweitern des virtuellen Datenträgers](./media/backup-mabs-add-storage/mabs-add-storage-4.png)

5. Wählen Sie den virtuellen Datenträger aus, und klicken Sie dann auf **Neues Volume**.

    ![Erstellen eines neuen Volumes](./media/backup-mabs-add-storage/mabs-add-storage-5.png)

6. Wählen Sie im Dialogfeld **Server und Datenträger auswählen** den Server und den neuen Datenträger aus. Klicken Sie anschließend auf **Weiter**.

    ![Auswählen des Servers und Datenträgers](./media/backup-mabs-add-storage/mabs-add-storage-6.png)

## <a name="add-volumes-to-backup-server-disk-storage"></a>Hinzufügen von Volumes zu Azure Backup-Datenträgerspeicher

Um Backup Server ein Volume hinzuzufügen, durchsuchen Sie im Bereich **Verwaltung** erneut den Speicher, und klicken Sie dann auf **Hinzufügen**. Eine Liste aller Volumes wird angezeigt, die dem Azure Backup-Speicher hinzugefügt werden können. Nachdem verfügbare Volumes der Liste ausgewählter Volumes hinzugefügt wurden, können Sie sie mit einem Anzeigenamen versehen, damit sie besser verwaltet werden können. Um diese Volumes mit ReFS zu formatieren, damit Backup Server die Vorteile von Modern Backup Storage nutzen kann, klicken Sie auf **OK**.

![Hinzufügen verfügbarer Volumes](./media/backup-mabs-add-storage/mabs-add-storage-7.png)

## <a name="set-up-workload-aware-storage"></a>Einrichten von workloadorientiertem Speicher

Mithilfe von workloadorientiertem Speicher können Sie die Volumes auswählen, auf denen bestimmte Arten von Workloads vorzugsweise gespeichert werden sollen. Sie können beispielsweise kostenintensive Volumes festlegen, die eine hohe Anzahl von Ein- und Ausgabevorgängen pro Sekunde (IOPS) unterstützen, um nur die Workloads zu speichern, die häufige Sicherungen mit großem Volumen erfordern. Ein Beispiel ist SQL Server mit Transaktionsprotokollen. Andere Workloads, die weniger häufig gesichert werden, wie z.B. VMs, können auf kostengünstigen Volumes gesichert werden.

### <a name="update-dpmdiskstorage"></a>Update-DPMDiskStorage

Sie können workloadorientierten Speicher mithilfe des PowerShell-Cmdlets „Update-DPMDiskStorage“ einrichten, das die Eigenschaften eines Volumes im Speicherpool auf einem Data Protection Manager-Server aktualisiert.

Syntax:

`Parameter Set: Volume`

```
Update-DPMDiskStorage [-Volume] <Volume> [[-FriendlyName] <String> ] [[-DatasourceType] <VolumeTag[]> ] [-Confirm] [-WhatIf] [ <CommonParameters>]
```
Der folgende Screenshot zeigt das Cmdlet „Update-DPMDiskStorage“ im PowerShell-Fenster.

![Das Cmdlet „Update DPMDiskStorage“ im PowerShell-Fenster](./media/backup-mabs-add-storage/mabs-add-storage-8.png)

Die Änderungen, die Sie mithilfe von PowerShell vornehmen, werden in der Backup-Server-Administratorkonsole angezeigt.

![Datenträger und Volumes in der Administratorkonsole](./media/backup-mabs-add-storage/mabs-add-storage-9.png)

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie, wie Sie nach der Installation von Backup Server Ihren Server vorbereiten oder mit dem Schutz einer Workload beginnen.

- [Vorbereiten von Backup Server-Workloads](backup-azure-microsoft-azure-backup.md)
- [Sichern eines VMware-Servers mit Backup Server](backup-azure-backup-server-vmware.md)
- [Sichern von SQL Server mit Backup Server](backup-azure-sql-mabs.md)


