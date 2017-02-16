---
title: Hochladen von VHD-Dateien in Azure DevTest Labs mithilfe von PowerShell | Microsoft-Dokumentation
description: Hochladen von VHD-Dateien in das Speicherkonto des Labs mithilfe von PowerShell
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 38d70ed302eeab912ce4fe33272f85e96f1b1eda
ms.openlocfilehash: 36edca0969eb2895777a85dc4ee826abeb42601f


---

# <a name="upload-vhd-file-to-labs-storage-account-using-powershell"></a>Hochladen von VHD-Dateien in das Speicherkonto des Labs mithilfe von PowerShell

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

In Azure DevTest Labs können VHD-Dateien verwendet werden, um benutzerdefinierte Images zu erstellen, mit denen virtuelle Computer bereitgestellt werden. Die folgenden Schritte führen Sie durch die Verwendung von PowerShell zum Hochladen einer VHD-Datei in das Speicherkonto eines Labs. Nachdem Sie die VHD-Datei hochgeladen haben, finden Sie im Abschnitt [Nächste Schritte](#next-steps) einige Artikel, die veranschaulichen, wie aus der hochgeladenen VHD-Datei ein benutzerdefiniertes Image erstellt wird. Weitere Informationen zu Datenträgern und VHDs in Azure finden Sie unter [Informationen zu Datenträgern und VHDs für virtuelle Computer](../virtual-machines/virtual-machines-linux-about-disks-vhds.md).

## <a name="step-by-step-instructions"></a>Schrittweise Anweisungen

Die folgenden Schritte führen Sie durch das Hochladen einer VHD-Datei in Azure DevTest Labs mithilfe von PowerShell. 

1. Melden Sie sich beim [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)an.

1. Wählen Sie **Weitere Dienste** und dann in der Liste **DevTest Labs**.

1. Wählen Sie in der Liste der Labs das gewünschte Lab aus.  

1. Wählen Sie auf dem Blatt des Labs **Konfiguration**aus. 

1. Wählen Sie auf dem Blatt **Konfiguration** für das Lab die Option **Benutzerdefinierte Images (VHDs)** aus.

1. Wählen Sie auf dem Blatt **Benutzerdefinierte Images** die Option **+ Hinzufügen** aus. 

1. Wählen Sie auf dem Blatt **Benutzerdefinierte Images** die Option **VHD** aus.

1. Wählen Sie auf dem Blatt **VHD** die Option **Hochladen einer VHD mithilfe von PowerShell** aus.

    ![Hochladen der VHD mithilfe von PowerShell](./media/devtest-lab-upload-vhd-using-powershell/upload-image-using-psh.png)

1. Kopieren Sie auf dem Blatt **Image mithilfe von PowerShell hochladen** das generierte PowerShell-Skript in einen Text-Editor.

1. Ändern Sie den **LocalFilePath**-Parameter des **Add-AzureVhd**-Cmdlets so, dass er auf den Speicherort der VHD-Datei zeigt, die Sie hochladen möchten.

1. Führen Sie das **Add-AzureVhd**-Cmdlet (mit dem geänderten **LocalFilePath**-Parameter) an einer PowerShell-Eingabeaufforderung aus.

> [!WARNING] 
> 
> Das Hochladen einer VHD-Datei kann abhängig von der Größe der VHD-Datei und der Geschwindigkeit Ihrer Verbindung lange dauern.

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen eines benutzerdefinierten Images in Azure DevTest Labs aus einer VHD-Datei mit dem Azure-Portal](devtest-lab-create-template.md)
- [Erstellen eines benutzerdefinierten Azure DevTest Labs-Images aus einer VHD-Datei mithilfe von PowerShell](devtest-lab-create-custom-image-from-vhd-using-powershell.md)



<!--HONumber=Jan17_HO2-->


