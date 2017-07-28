---
title: Einbinden einer Azure-Dateifreigabe und Zugreifen auf die Freigabe unter Windows | Microsoft-Dokumentation
description: Binden Sie eine Azure-Dateifreigabe unter Windows ein, und greifen Sie darauf zu.
services: storage
documentationcenter: na
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
ms.sourcegitcommit: 2812039649f7d2fb0705220854e4d8d0a031d31e
ms.openlocfilehash: e911e787cd1e29b2bbeaa648869c50245f2dd9ba
ms.contentlocale: de-de
ms.lasthandoff: 07/22/2017

---

# <a name="mount-an-azure-file-share-and-access-the-share-in-windows"></a>Einbinden einer Azure-Dateifreigabe und Zugreifen auf die Freigabe unter Windows
[Azure File Storage](storage-dotnet-how-to-use-files.md) ist das benutzerfreundliche Clouddateisystem von Microsoft. Azure-Dateifreigaben können in Windows und in Windows Server eingebunden werden. Dieser Artikel zeigt drei verschiedene Methoden zum Einbinden einer Azure-Dateifreigabe unter Windows: über die Benutzeroberfläche des Explorers, mithilfe von PowerShell und über die Eingabeaufforderung. 

Wenn Sie eine Azure-Dateifreigabe außerhalb der Azure-Region einbinden möchten, in der sie gehostet wird (beispielsweise lokal oder in einer anderen Azure-Region), muss das Betriebssystem SMB 3.0 unterstützen. 

Die Azure-Dateifreigabe kann auf einem Windows-Computer je nach Betriebssystemversion entweder lokal oder in einen virtuellen Azure-Computer eingebunden werden. Dies wird in der folgenden Tabelle veranschaulicht: 

| Windows-Version        | SMB-Version |In virtuellen Azure-Computer einbindbar|Lokal einbindbar|
|------------------------|-------------|---------------------|---------------------|
| Windows 7              | SMB 2.1     | Ja                 | Nein                  |
| Windows Server 2008 R2 | SMB 2.1     | Ja                 | Nein                  |
| Windows 8              | SMB 3.0     | Ja                 | Ja                 |
| Windows Server 2012    | SMB 3.0     | Ja                 | Ja                 |
| Windows Server 2012 R2 | SMB 3.0     | Ja                 | Ja                 |
| Windows 10             | SMB 3.0     | Ja                 | Ja                 |

> [!Note]  
> Es empfiehlt sich, immer die neueste KB für Ihre Windows-Version zu verwenden.

## <a name="aprerequisites-for-mounting-azure-file-share-with-windows"></a></a>Voraussetzungen für das Einbinden von Azure-Dateifreigaben mit Windows 
* **Name des Speicherkontos:** Zum Einbinden einer Azure-Dateifreigabe benötigen Sie den Namen des Speicherkontos.

* **Speicherkontoschlüssel:** Zum Einbinden einer Azure-Dateifreigabe benötigen Sie den primären (oder sekundären) Speicherschlüssel. SAS-Schlüssel können derzeit nicht zum Einbinden verwendet werden.

* **Sicherstellen, dass Port 445 geöffnet ist:** Azure File Storage verwendet das SMB-Protokoll. SMB kommuniziert über den TCP-Port 445. Vergewissern Sie sich, dass der TCP-Port 445 des Clientcomputers nicht durch die Firewall blockiert wird.

## <a name="mount-the-azure-file-share-with-file-explorer"></a>Einbinden der Azure-Dateifreigabe über den Explorer
> [!Note]  
> Beachten Sie, dass sich die folgenden Anweisungen auf Windows 10 beziehen und bei älteren Versionen unter Umständen geringfügig unterscheiden können. 

1. **Öffnen Sie den Explorer:** Verwenden Sie dazu entweder das Startmenü, oder drücken Sie WINDOWS-TASTE+E.

2. **Navigieren Sie im linken Fensterbereich zu „Dieser PC“. Dadurch ändern sich die verfügbaren Menüs auf dem Menüband. Wählen Sie im Menü „Computer“ die Option „Netzlaufwerk verbinden“ aus.**
    
    ![Screenshot des Dropdownmenüs „Netzlaufwerk verbinden“](media/storage-file-how-to-use-files-windows/1_MountOnWindows10.png)

3. **Kopieren Sie im Azure-Portal den UNC-Pfad aus dem Bereich „Verbinden“:** Falls Sie nicht genau wissen, wie Sie diese Information finden, lesen Sie [hier](storage-file-how-to-use-files-portal.md#connect-to-file-share) weiter.

    ![UNC-Pfad aus dem Verbindungsbereich von Azure File Storage](media/storage-file-how-to-use-files-windows/portal_netuse_connect.png)

4. **Wählen Sie den Laufwerkbuchstaben aus, und geben Sie den UNC-Pfad ein.** 
    
    ![Screenshot des Dialogfelds „Netzlaufwerk verbinden“](media/storage-file-how-to-use-files-windows/2_MountOnWindows10.png)

5. **Verwenden Sie den Speicherkontonamen mit vorangestelltem `Azure\` als Benutzername und einen Speicherkontoschlüssel als Kennwort.**
    
    ![Screenshot des Dialogfelds für die Netzwerkanmeldeinformationen](media/storage-file-how-to-use-files-windows/3_MountOnWindows10.png)

6. **Verwenden Sie die Azure-Dateifreigabe wie gewünscht.**
    
    ![Die Azure-Dateifreigabe ist nun eingebunden.](media/storage-file-how-to-use-files-windows/4_MountOnWindows10.png)

7. **Wenn Sie die Einbindung der Azure-Dateifreigabe wieder aufheben (also die Verbindung trennen) möchten, klicken Sie im Explorer unter „Netzwerk“ mit der rechten Maustaste auf den Eintrag für die Freigabe, und wählen Sie „Trennen“ aus.**

## <a name="mount-the-azure-file-share-with-powershell"></a>Einbinden der Azure-Dateifreigabe mithilfe von PowerShell
1. **Verwenden Sie zum Einbinden der Azure-Dateifreigabe den folgenden Befehl.** Ersetzen Sie dabei `<storage-account-name>`, `<share-name>`, `<storage-account-key>` und `<desired-drive-letter>` durch die entsprechenden Angaben.

    ```PowerShell
    $acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
    New-PSDrive -Name <desired-drive-letter> -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.windows.net\<share-name>" -Credential $credential
    ```

2. **Verwenden Sie die Azure-Dateifreigabe wie gewünscht.**

3. **Wenn Sie die Azure-Dateifreigabe nicht mehr benötigen, heben Sie die Einbindung mithilfe des folgenden Befehls wieder auf:**

    ```PowerShell
    Remove-PSDrive -Name <desired-drive-letter>
    ```

> [!Note]  
> Mit dem Parameter `-Persist` für `New-PSDrive` können Sie die eingebundene Azure-Dateifreigabe für den Rest des Betriebssystems sichtbar machen.

## <a name="mount-the-azure-file-share-with-command-prompt"></a>Einbinden der Azure-Dateifreigabe über die Eingabeaufforderung
1. **Verwenden Sie zum Einbinden der Azure-Dateifreigabe den folgenden Befehl.** Ersetzen Sie dabei `<storage-account-name>`, `<share-name>`, `<storage-account-key>` und `<desired-drive-letter>` durch die entsprechenden Angaben.

    ```
    net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> <storage-account-key> /user:Azure\<storage-account-name>
    ```

2. **Verwenden Sie die Azure-Dateifreigabe wie gewünscht.**

3. **Wenn Sie die Azure-Dateifreigabe nicht mehr benötigen, heben Sie die Einbindung mithilfe des folgenden Befehls wieder auf:**

    ```
    net use <desired-drive-letter>: /delete
    ```

> [!Note]  
> Sie können die Azure-Dateifreigabe so konfigurieren, dass die Verbindung nach einem Neustart des Systems automatisch wiederhergestellt wird, indem Sie die Anmeldeinformationen in Windows speichern. Verwenden Sie zum Speichern der Anmeldeinformationen den folgenden Befehl:
>   ```
>   cmdkey /add:<storage-account-name>.file.core.windows.net /user:AZURE\<storage-account-name> /pass:<storage-account-key>
>   ```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Azure-Dateispeicher erhalten Sie über diese Links.

* [Häufig gestellte Fragen](storage-files-faq.md)
* [Problembehandlung](storage-troubleshoot-file-connection-problems.md)

### <a name="conceptual-articles-and-videos"></a>Konzeptionelle Artikel und Videos
* [Azure File Storage: ein reibungsloses Cloud-SMB-Dateisystem für Windows und Linux](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
* [Verwenden des Azure-Dateispeichers unter Linux](storage-how-to-use-files-linux.md)

### <a name="tooling-support-for-azure-file-storage"></a>Toolunterstützung für Azure File Storage
* [Verwenden von Azure PowerShell mit Azure Storage](storage-powershell-guide-full.md)
* [Verwenden von AzCopy mit Microsoft Azure Storage](storage-use-azcopy.md)
* [Verwenden der Azure-Befehlszeilenschnittstelle mit Azure-Speicher](storage-azure-cli.md#create-and-manage-file-shares)
* [Beheben von Problemen mit Azure File Storage](https://docs.microsoft.com/azure/storage/storage-troubleshoot-file-connection-problems)

### <a name="blog-posts"></a>Blogbeiträge
* [Azure-Dateispeicher ist jetzt allgemein verfügbar](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [Inside Azure File Storage](https://azure.microsoft.com/blog/inside-azure-file-storage/) (Informationen zu Azure File Storage)
* [Einführung in den Microsoft Azure-Dateidienst](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [Migrating Data to Microsoft Azure Files](https://azure.microsoft.com/blog/migrating-data-to-microsoft-azure-files/) (Migrieren von Daten zu Microsoft Azure Files)

### <a name="reference"></a>Referenz
* [Referenz zur Storage-Clientbibliothek für .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [Referenz zur REST-API des Dateidiensts](http://msdn.microsoft.com/library/azure/dn167006.aspx)

