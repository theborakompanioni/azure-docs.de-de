---
title: "Verwalten von Azure File Storage über das Azure-Portal | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie Azure File Storage über das Azure-Portal verwalten."
services: storage
documentationcenter: 
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/27/2017
ms.author: renash
ms.translationtype: HT
ms.sourcegitcommit: 2ad539c85e01bc132a8171490a27fd807c8823a4
ms.openlocfilehash: 58353a8058c2722788a38d2ca596061d14b68caf
ms.contentlocale: de-de
ms.lasthandoff: 07/12/2017

---

# <a name="how-to-use-azure-file-storage-from-the-azure-portal"></a>Verwenden von Azure File Storage über das Azure-Portal
Das [Azure-Portal](https://portal.azure.com) bietet eine Benutzeroberfläche für die Verwaltung von Azure File Storage. Sie können in Ihrem Webbrowser die folgenden Aktionen ausführen:

* Erstellen einer Dateifreigabe
* Hoch- und Herunterladen von Dateien für die Dateifreigabe
* Überwachen der tatsächlichen Nutzung der einzelnen Dateifreigaben
* Anpassen des Kontingents für die Dateifreigabegröße
* Kopieren der zu verwendenden Einbindebefehle, um die Dateifreigabe über einen Windows- oder Linux-Client einzubinden

## <a name="create-file-share"></a>Erstellen einer Dateifreigabe
1. Melden Sie sich beim Azure-Portal an.
2. Klicken Sie im Navigationsbereich auf **Speicherkonten** oder **Speicherkonten (klassisch)**.
    
    ![Screenshot, der das Erstellen einer Dateifreigabe im Portal veranschaulicht](media/storage-file-how-to-use-files-portal/use-files-portal-create-file-share1.png)

3. Wählen Sie Ihr Speicherkonto aus.

    ![Screenshot, der das Erstellen einer Dateifreigabe im Portal veranschaulicht](media/storage-file-how-to-use-files-portal/use-files-portal-create-file-share2.png)

4. Wählen Sie den Dienst „Dateien“.

    ![Screenshot, der das Erstellen einer Dateifreigabe im Portal veranschaulicht](media/storage-file-how-to-use-files-portal/use-files-portal-create-file-share3.png)

5. Klicken Sie auf „Dateifreigaben“, und folgen Sie dem Link, um Ihre erste Dateifreigabe zu erstellen.

    ![Screenshot, der das Erstellen einer Dateifreigabe im Portal veranschaulicht](media/storage-file-how-to-use-files-portal/use-files-portal-create-file-share4.png)

6. Geben Sie den Namen und die Größe der Dateifreigabe (bis zu 5.120 GB) ein, um Ihre erste Dateifreigabe zu erstellen. Sobald die Dateifreigabe erstellt wurde, können Sie sie von einem beliebigen Dateisystem aus einbinden, das SMB 2.1 oder SMB 3.0 unterstützt. Sie können für die Dateifreigabe auf **Kontingent** klicken und die Größe der Datei auf bis zu 5.120 GB festlegen. Verwenden Sie zum Ermitteln der Speicherkosten, die bei Verwendung von Azure File Storage zu erwarten sind, den [Azure-Preisrechner](https://azure.microsoft.com/pricing/calculator/).

    ![Screenshot, der das Erstellen einer Dateifreigabe im Portal veranschaulicht](media/storage-file-how-to-use-files-portal/use-files-portal-create-file-share5.png)

## <a name="upload-and-download-files"></a>Hochladen und Herunterladen von Dateien
1. Wählen Sie eine bereits erstellte Dateifreigabe aus.

    ![Screenshot, der das Hoch- und Herunterladen von Dateien über das Portal veranschaulicht](media/storage-file-how-to-use-files-portal/use-files-portal-upload-file1.png)

2. Klicken Sie auf **Hochladen** , um die Benutzeroberfläche zum Hochladen von Dateien zu öffnen.

    ![Screenshot, der das Hochladen von Dateien über das Portal veranschaulicht](media/storage-file-how-to-use-files-portal/use-files-portal-upload-file2.png)

## <a name="connect-to-file-share"></a>Herstellen einer Verbindung mit der Dateifreigabe
-  Klicken Sie auf **Verbinden**, um die Befehlszeile zum Bereitstellen der Dateifreigabe aus Windows und Linux zu erhalten. Für Linux-Benutzer stehen unter [Verwenden des Azure-Dateispeichers unter Linux](storage-how-to-use-files-linux.md) weitere Einbindeanweisungen für andere Linux-Distributionen zur Verfügung.

    ![Screenshot, der das Bereitstellen der Dateifreigabe veranschaulicht](media/storage-file-how-to-use-files-portal/use-files-portal-connect.png)
-  Sie können die Befehle zum Einbinden der Dateifreigabe unter Windows oder Linux kopieren und sie über Ihren virtuellen Azure-Computer oder über Ihren lokalen Computer ausführen.

    ![Screenshot mit den Einbindebefehlen für Windows und Linux](media/storage-file-how-to-use-files-portal/use-files-portal-show-mount-commands.png)

**Tipp:**  
Klicken Sie zum Ermitteln des Speicherkonto-Zugriffsschlüssels für die Einbindung am unteren Rand der Verbindungsseite auf **Zugriffsschlüssel für dieses Speicherkonto anzeigen** am unteren Rand der Seite "Verbinden".

## <a name="see-also"></a>Weitere Informationen
Weitere Informationen zum Azure-Dateispeicher erhalten Sie über diese Links.

* [Häufig gestellte Fragen](storage-files-faq.md)
* [Problembehandlung](storage-troubleshoot-file-connection-problems.md)
