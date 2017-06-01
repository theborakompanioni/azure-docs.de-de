---
title: Erstellen und Hochladen eines VM-Images mit PowerShell | Microsoft Docs
description: Hier finden Sie Informationen Sie zum Erstellen und Hochladen eines generalisierten Windows Server-Image (VHD) mit dem klassischen Bereitstellungsmodell und Azure PowerShell.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 8c4a08fe-7714-4bf0-be87-c728a7806d3f
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: cynthn
ms.translationtype: Human Translation
ms.sourcegitcommit: 67ee6932f417194d6d9ee1e18bb716f02cf7605d
ms.openlocfilehash: 1d2b51f2a8a03a94b7ce92af54f89128bcfc9471
ms.contentlocale: de-de
ms.lasthandoff: 05/26/2017


---
# <a name="create-and-upload-a-windows-server-vhd-to-azure"></a>Erstellen und Hochladen einer Windows Server-VHD nach Azure
Dieser Artikel erläutert, wie Sie Ihr eigenes generalisiertes VM-Image als virtuelle Festplatte (Virtual Hard Disk, VHD) hochladen, um es zum Erstellen virtueller Computer zu nutzen. Weitere Informationen zu Datenträgern und VHDs in Microsoft Azure finden Sie unter [Informationen zu Datenträgern und VHDs für virtuelle Computer](../../../storage/storage-about-disks-and-vhds-windows.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

> [!IMPORTANT]
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager- und klassische Bereitstellung](../../../resource-manager-deployment-model.md). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells. Sie können einen virtuellen Computer auch mit dem Resource Manager-Modell [hochladen](../upload-generalized-managed.md).

## <a name="prerequisites"></a>Voraussetzungen
In diesem Artikel wird davon ausgegangen, dass Sie über Folgendes verfügen:

* **Ein Azure-Abonnement** – wenn Sie keines besitzen, können Sie [ein kostenloses Azure-Konto eröffnen](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
* **[Microsoft Azure PowerShell](/powershell/azure/overview)**: Sie haben das Microsoft Azure PowerShell-Modul installiert und für die Verwendung Ihres Abonnements konfiguriert.
* **Eine VHD-Datei** – ein unterstütztes Windows-Betriebssystem, das in einer VHD-Datei gespeichert und einem virtuellen Computer angefügt ist. Prüfen Sie auch, ob die auf der virtuellen Festplatte ausgeführten Serverrollen von Sysprep unterstützt werden. Weitere Informationen finden Sie unter [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)(Sysprep-Unterstützung für Serverrollen).

    > [!IMPORTANT]
    > Das VHDX-Format wird in Microsoft Azure nicht unterstützt. Sie können den Datenträger mit dem Hyper-V-Manager oder dem [Convert-VHD-Cmdlet](http://technet.microsoft.com/library/hh848454.aspx)in das VHD-Format konvertieren. Ausführliche Informationen finden Sie in diesem [Blogbeitrag](http://blogs.msdn.com/b/virtual_pc_guy/archive/2012/10/03/using-powershell-to-convert-a-vhd-to-a-vhdx.aspx).

## <a name="step-1-prep-the-vhd"></a>Schritt 1: Vorbereiten der VHD-Datei
Bevor Sie die VHD-Datei nach Azure hochladen, muss sie mit dem Sysprep-Tool generalisiert werden. Durch diesen Vorgang wird die VHD-Datei auf eine Verwendung als Image vorbereitet. Weitere Informationen zu Sysprep finden Sie unter [How to Use Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx)(in englischer Sprache). Sichern Sie den virtuellen Computer vor dem Ausführen von Sysprep.

Führen Sie auf dem virtuellen Computer, auf dem das Betriebssystem installiert wurde, die folgende Prozedur aus:

1. Melden Sie sich beim Betriebssystem an.
2. Öffnen Sie ein Eingabeaufforderungsfenster als Administrator. Wechseln Sie in das Verzeichnis **%windir%\system32\sysprep**, und führen Sie anschließend `sysprep.exe` aus.

    ![Öffnen eines Eingabeaufforderungsfensters](./media/createupload-vhd/sysprep_commandprompt.png)
3. Das Dialogfeld **Systemvorbereitungstool** wird angezeigt.

   ![Starten von Sysprep](./media/createupload-vhd/sysprepgeneral.png)
4. Wählen Sie im **Systemvorbereitungstool** die Option **Out-of-Box-Experience (OOBE) für System aktivieren**, und stellen Sie sicher, dass **Verallgemeinern** aktiviert ist.
5. Wählen Sie unter **Optionen für Herunterfahren** die Option **Herunterfahren**.
6. Klicken Sie auf **OK**.

## <a name="step-2-create-a-storage-account-and-a-container"></a>Schritt 2: Erstellen eines Speicherkontos und eines Containers
Sie benötigen ein Speicherkonto in Azure, um über einen Ort zum Hochladen der VHD-Datei zu verfügen. In diesem Schritt wird gezeigt, wie Sie ein Konto erstellen oder Informationen zu einem vorhandenen Konto abrufen. Ersetzen Sie die Variablen in spitzen Klammern („&lsaquo;“ und „&rsaquo;“) durch Ihre eigenen Informationen.

1. Anmeldung

    ```powershell
    Add-AzureAccount
    ```

2. Legen Sie Ihr Azure-Abonnement fest.

    ```powershell
    Select-AzureSubscription -SubscriptionName <SubscriptionName>
    ```

3. Erstellen Sie ein neues Speicherkonto Der Name des Speicherkontos muss eindeutig und 3 bis 24 Zeichen lang sein. Der Name kann eine beliebige Kombination aus Buchstaben und Zahlen sein. Sie müssen auch einen Speicherort angeben, z.B. „USA, Osten“.

    ```powershell
    New-AzureStorageAccount –StorageAccountName <StorageAccountName> -Location <Location>
    ```

4. Legen Sie das neue Speicherkonto als Standard fest.

    ```powershell
    Set-AzureSubscription -CurrentStorageAccountName <StorageAccountName> -SubscriptionName <SubscriptionName>
    ```

5. Erstellen Sie einen neuen Container.

    ```powershell
    New-AzureStorageContainer -Name <ContainerName> -Permission Off
    ```

## <a name="step-3-upload-the-vhd-file"></a>Schritt 3: Hochladen der VHD-Datei
Verwenden Sie [Add-AzureVhd](https://docs.microsoft.com/en-us/powershell/module/azure/add-azurevhd) zum Hochladen der VHD.

Geben Sie in dem Azure PowerShell-Fenster, das Sie im vorherigen Schritt verwendet haben, den folgenden Befehl ein, und ersetzen Sie die Variablen in spitzen Klammern („&lsaquo;“ und „&rsaquo;“) durch Ihre eigenen Informationen.

```powershell
Add-AzureVhd -Destination "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -LocalFilePath <LocalPathtoVHDFile>
```

## <a name="step-4-add-the-image-to-your-list-of-custom-images"></a>Schritt 4: Hinzufügen des Image zur Liste der benutzerdefinierten Images
Fügen Sie das Image mit dem Cmdlet [Add-AzureVMImage](https://docs.microsoft.com/en-us/powershell/module/azure/add-azurevmimage) der Liste Ihrer benutzerdefinierten Images hinzu.

```powershell
Add-AzureVMImage -ImageName <ImageName> -MediaLocation "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -OS "Windows"
```

## <a name="next-steps"></a>Nächste Schritte
Sie können nun mit dem Image, das Sie hochgeladen haben, [einen benutzerdefinierten virtuellen Computer erstellen](createportal.md).

