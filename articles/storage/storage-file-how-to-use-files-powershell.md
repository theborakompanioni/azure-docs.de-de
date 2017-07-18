---
title: Verwenden von PowerShell zum Verwalten des Azure-Dateispeichers | Microsoft-Dokumentation
description: "Erfahren Sie hier, wie Sie den Azure-Dateispeicher mithilfe von PowerShell verwalten können."
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
ms.sourcegitcommit: f76de4efe3d4328a37f86f986287092c808ea537
ms.openlocfilehash: 5d825c1139d49fb8d37c719d8e0ab8ba2da2ed8d
ms.contentlocale: de-de
ms.lasthandoff: 07/10/2017

---
# <a name="how-to-use-powershell-to-manage-azure-file-storage"></a>Verwenden von PowerShell zum Verwalten des Azure-Dateispeichers
Sie können Azure PowerShell zum Erstellen und Verwalten von Dateifreigaben verwenden.

## <a name="install-the-powershell-cmdlets-for-azure-storage"></a>Installieren der PowerShell-Cmdlets für den Azure-Speicher
Laden Sie die Azure PowerShell-Cmdlets herunter und installieren Sie diese anschließend, um die Verwendung von PowerShell vorzubereiten. Informationen zum Installationspunkt und zu Installationsanweisungen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs) .

> [!NOTE]
> Sie sollten daher das neueste Azure PowerShell-Modul herunterladen und installieren bzw. ein Upgrade durchführen.
> 
> 

Öffnen Sie ein Azure PowerShell-Fenster, indem Sie auf **Start** klicken und **Windows PowerShell** eingeben. Im PowerShell-Fenster wird das Azure Powershell-Modul geladen.

## <a name="create-a-context-for-your-storage-account-and-key"></a>Erstellen von Kontexten für Speicherkonten und -schlüssel
Erstellen Sie den Speicherkontokontext. Der Kontext kapselt den Speicherkontonamen und den Kontoschlüssel. Anweisungen zum Kopieren Ihres Kontoschlüssels aus dem [Azure-Portal](https://portal.azure.com) finden Sie unter [Anzeigen und Kopieren von Speicherzugriffsschlüsseln](storage-create-storage-account.md#view-and-copy-storage-access-keys).

Ersetzen Sie im folgenden Beispiel `storage-account-name` und `storage-account-key` durch Ihren Speicherkontonamen und -schlüssel.

```powershell
# create a context for account and key
$ctx=New-AzureStorageContext storage-account-name storage-account-key
```

## <a name="create-a-new-file-share"></a>Erstellen einer neuen Dateifreigabe
Erstellen Sie die neue Freigabe mit dem Namen `logs`.

```powershell
# create a new share
$s = New-AzureStorageShare logs -Context $ctx
```

Nun haben Sie eine Dateifreigabe im Dateispeicher. Als Nächstes fügen Sie ein Verzeichnis und eine Datei hinzu.

> [!IMPORTANT]
> Der Name der Dateifreigabe darf nur Kleinbuchstaben enthalten. Ausführliche Informationen zur Benennung von Dateifreigaben und Dateien finden Sie unter [Benennen und Referenzieren von Freigaben, Verzeichnissen, Dateien und Metadaten](https://msdn.microsoft.com/library/azure/dn167011.aspx).
> 
> 

## <a name="create-a-directory-in-the-file-share"></a>Erstellen eines Verzeichnisses in der Dateifreigabe
Erstellen Sie ein Verzeichnis in der Freigabe. Im folgenden Beispiel lautet der Verzeichnisname `CustomLogs`.

```powershell
# create a directory in the share
New-AzureStorageDirectory -Share $s -Path CustomLogs
```

## <a name="upload-a-local-file-to-the-directory"></a>Hochladen einer lokalen Datei in das Verzeichnis
Laden Sie nun eine lokale Datei in das Verzeichnis hoch. Im folgenden Beispiel wird eine Datei aus `C:\temp\Log1.txt` hochgeladen. Bearbeiten Sie den Dateipfad so, dass er auf eine gültige Datei auf Ihrem lokalen Computer verweist.

```powershell
# upload a local file to the new directory
Set-AzureStorageFileContent -Share $s -Source C:\temp\Log1.txt -Path CustomLogs
```

## <a name="list-the-files-in-the-directory"></a>Auflisten der Dateien im Verzeichnis
Sie können die Dateien im Verzeichnis auflisten, um alle im Verzeichnis enthaltenen Dateien anzuzeigen. Mit diesem Befehl werden die Dateien und Unterverzeichnisse (falls vorhanden) im Verzeichnis CustomLogs zurückgegeben.

```powershell
# list files in the new directory
Get-AzureStorageFile -Share $s -Path CustomLogs | Get-AzureStorageFile
```

Mit Get-AzureStorageFile wird eine Liste mit Dateien und Verzeichnissen für das jeweils übergebene Verzeichnisobjekt zurückgegeben. Mit „Get-AzureStorageFile -Share $s“ wird eine Liste mit Dateien und Verzeichnissen im Stammverzeichnis zurückgegeben. Um eine Liste mit den Dateien eines Unterverzeichnisses zu erhalten, müssen Sie das Unterverzeichnis an Get-AzureStorageFile übergeben. Folgendes wird durchgeführt: Mit dem ersten Teil des Befehls bis zum Pipe-Zeichen wird eine Verzeichnisinstanz des Unterverzeichnisses CustomLogs zurückgegeben. Diese wird dann an Get-AzureStorageFile übergeben, und die Dateien und Verzeichnisse in CustomLogs werden zurückgegeben.

## <a name="copy-files"></a>Kopieren von Dateien
Ab Version 0.9.7 von Azure PowerShell können Sie eine Datei in eine andere Datei, eine Datei in ein Blob oder ein Blob in eine Datei kopieren. Im Folgenden wird demonstriert, wie diese Kopiervorgänge mit PowerShell-Cmdlets ausgeführt werden.

```powershell
# copy a file to the new directory
Start-AzureStorageFileCopy -SrcShareName srcshare -SrcFilePath srcdir/hello.txt -DestShareName destshare -DestFilePath destdir/hellocopy.txt -Context $srcCtx -DestContext $destCtx

# copy a blob to a file directory
Start-AzureStorageFileCopy -SrcContainerName srcctn -SrcBlobName hello2.txt -DestShareName hello -DestFilePath hellodir/hello2copy.txt -DestContext $ctx -Context $ctx
```
## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Azure-Dateispeicher erhalten Sie über diese Links.

* [Häufig gestellte Fragen](storage-files-faq.md)
* [Problembehandlung](storage-troubleshoot-file-connection-problems.md)
