---
title: Azure Analysis Services-Datenbanksicherung und -wiederherstellung | Microsoft-Dokumentation
description: Beschreibt, wie eine Azure Analysis Services-Datenbank gesichert und wiederhergestellt wird.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2017
ms.author: owend
ms.translationtype: Human Translation
ms.sourcegitcommit: e72275ffc91559a30720a2b125fbd3d7703484f0
ms.openlocfilehash: ba842028ecda47b8011cba948d7c92417214c2d0
ms.contentlocale: de-de
ms.lasthandoff: 05/05/2017


---

# <a name="backup-and-restore"></a>Sichern und Wiederherstellen

Das Sichern von Datenbanken für tabellarische Modelle in Azure Analysis Services ähnelt dem Sichern von lokalem Analysis Services. Der Hauptunterschied besteht im Speicherort der Sicherungsdateien. Sicherungsdateien müssen in einem Container in einem [Azure Storage-Konto](../storage/storage-create-storage-account.md) gespeichert werden. Sie können ein bereits vorhandenes Speicherkonto und einen vorhandenen Container verwenden, oder beim Konfigurieren der Speichereinstellungen für den Server können ein Speicherkonto und ein Container erstellt werden.

> [!NOTE]
> Wenn Sie ein Speicherkonto erstellen, wird unter Umständen auch ein neuer abrechenbarer Dienst erstellt. Weitere Informationen finden Sie unter [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/blobs/).
> 
> 

Sicherungen werden mit der Erweiterung ABF gespeichert. Für tabellarische In-Memory-Modelle werden sowohl die Modelldaten als auch die Metadaten gespeichert. Für tabellarische DirectQuery-Modelle werden nur die Modellmetadaten gespeichert. Sicherungen können in Abhängigkeit von den gewählten Optionen komprimiert und verschlüsselt werden. 



## <a name="configure-storage-settings"></a>Konfigurieren von Speichereinstellungen
Bevor Sie eine Sicherung durchführen, müssen Sie die Speichereinstellungen für den Server konfigurieren.


### <a name="to-configure-storage-settings"></a>So konfigurieren Sie Speichereinstellungen
1.  Klicken Sie im Azure-Portal auf **Einstellungen** > **Sicherung**.

    ![Sicherungen in den Einstellungen](./media/analysis-services-backup/aas-backup-backups.png)

2.  Klicken Sie auf **Aktiviert** und dann auf **Speichereinstellungen**.

    ![Aktivieren](./media/analysis-services-backup/aas-backup-enable.png)

3. Wählen Sie Ihr Speicherkonto aus, oder erstellen Sie ein neues.

4. Wählen Sie einen Container aus, oder erstellen Sie einen neuen.

    ![Container auswählen](./media/analysis-services-backup/aas-backup-container.png)

5. Speichern Sie die Sicherungseinstellungen. Bei jeder Änderung der Speichereinstellungen und beim Aktivieren oder Deaktivieren der Sicherung müssen Sie die Änderungen speichern.

    ![Sicherungseinstellungen speichern](./media/analysis-services-backup/aas-backup-save.png)

## <a name="backup"></a>Sicherung

### <a name="to-backup-by-using-ssms"></a>So führen Sie eine Sicherung mithilfe von SSMS durch

1. Klicken Sie in SSMS mit der rechten Maustaste auf eine Datenbank > **Sichern**.

2. Klicken Sie unter **Datenbank sichern** > **Sicherungsdatei** auf **Durchsuchen**.

3. Überprüfen Sie im Dialogfeld **Datei speichern unter** den Ordnerpfad, und geben Sie einen Namen für die Sicherungsdatei ein. Standardmäßig wird eine ABF-Erweiterung an den Dateinamen angefügt. 

4. Wählen Sie im Dialogfeld **Datenbank sichern** Optionen aus.

    **Überschreiben von Dateien zulassen**: Wählen Sie diese Option aus, um Sicherungsdateien mit identischem Namen zu überschreiben. Wenn diese Option nicht ausgewählt ist, darf die Datei, die Sie speichern, nicht denselben Namen wie eine Datei haben, die an diesem Speicherort bereits vorhanden ist.

    **Komprimierung anwenden**: Wählen Sie diese Option aus, um die Sicherungsdatei zu komprimieren. Mit komprimierten Sicherungsdateien sparen Sie Speicherplatz, allerdings wird die CPU-Auslastung geringfügig erhöht. 

    **Sicherungsdatei verschlüsseln**: Wählen Sie diese Option aus, um die Sicherungsdatei zu verschlüsseln. Für diese Option muss vom Benutzer ein Kennwort angegeben werden, um die Sicherungsdatei zu sichern. Mit dem Kennwort wird verhindert, dass die Sicherungsdaten außer bei einem Wiederherstellungsvorgang gelesen werden. Wenn Sie Sicherungen verschlüsseln, speichern Sie das Kennwort an einem sicheren Ort.

5. Klicken Sie auf **OK**, um die Sicherungsdatei zu erstellen und zu speichern.


### <a name="powershell"></a>PowerShell
Verwenden Sie das Cmdlet [Backup-ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/backup-asdatabase-cmdlet).

## <a name="restore"></a>Wiederherstellen
Bei der Wiederherstellung muss sich die Sicherungsdatei im Speicherkonto befinden, das Sie für den Server konfiguriert haben. Wenn Sie eine Sicherungsdatei aus einem lokalen Speicherort in Ihr Speicherkonto verschieben müssen, verwenden Sie den [Microsoft Azure-Speicher-Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer) oder das Befehlszeilenprogramm [AzCopy](../storage/storage-use-azcopy.md). 



> [!NOTE]
> Wenn Sie eine Datenbank für ein tabellarisches Modell von einem lokalen SQL Server Analysis Services-Server wiederherstellen, müssen Sie zuerst alle Domänenbenutzer aus den Rollen des Modells entfernen und sie als Azure Active Directory-Benutzer den Rollen wieder hinzufügen. Die Rollen werden identisch sein.
> 
> 

### <a name="to-restore-by-using-ssms"></a>So führen Sie eine Wiederherstellung mithilfe von SSMS durch

1. Klicken Sie in SSMS mit der rechten Maustaste auf eine Datenbank > **Wiederherstellen**.

2. Klicken Sie im Dialogfeld **Datenbank sichern** unter **Sicherungsdatei** auf **Durchsuchen**.

3. Wählen Sie im Dialogfeld **Datenbankdateien suchen** die Datei aus, die Sie wiederherstellen möchten.

4. Wählen Sie die Datenbank unter **Datenbank wiederherstellen** aus.

5. Geben Sie Optionen an. Die Sicherheitsoptionen müssen mit den Sicherungsoptionen übereinstimmen, die Sie beim Sichern verwendet haben.


### <a name="powershell"></a>PowerShell

Verwenden Sie das Cmdlet [Restore-ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/restore-asdatabase-cmdlet).


## <a name="related-information"></a>Verwandte Informationen

[Azure-Speicherkonten](../storage/storage-create-storage-account.md)  
[Hohe Verfügbarkeit](analysis-services-bcdr.md)     
[Verwalten von Azure Analysis Services](analysis-services-manage.md)

