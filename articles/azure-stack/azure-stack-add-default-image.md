---
title: "Hinzufügen des VM-Standardimages zum Azure Stack-Marketplace | Microsoft-Dokumentation"
description: "Fügen Sie das VM-Standardimage für Windows Server 2016 zum Azure Stack-Marketplace hinzu."
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: sngun
ms.translationtype: HT
ms.sourcegitcommit: 1c730c65194e169121e3ad1d1423963ee3ced8da
ms.openlocfilehash: 2cbdca8c795346864b6e39e42858f3dc46def199
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---
# <a name="add-the-windows-server-2016-vm-image-to-the-azure-stack-marketplace"></a>Hinzufügen des VM-Images für Windows Server 2016 zum Azure Stack-Marketplace

Im Azure Stack-Marketplace sind standardmäßig keine VM-Images verfügbar. Der Azure Stack-Betreiber muss ein Image zum Marketplace hinzufügen, damit Benutzer es verwenden können. Sie können mit einer der folgenden Methoden das Windows Server 2016-Image zum Azure Stack-Marketplace hinzufügen:

* [Hinzufügen des Images durch Herunterladen aus dem Azure Marketplace](#add-the-image-by-downloading-it-from-the-Azure-marketplace): Verwenden Sie diese Option, wenn Sie ein verbundenes Szenario nutzen und Ihre Azure Stack-Instanz bei Azure registriert haben.

* [Hinzufügen des Images mithilfe von PowerShell](#add-the-image-by-using-powershell): Verwenden Sie diese Option, wenn Sie Azure Stack in einem nicht verbundenen Szenario oder in Szenarien mit eingeschränkter Konnektivität bereitgestellt haben.

## <a name="add-the-image-by-downloading-it-from-the-azure-marketplace"></a>Hinzufügen des Images durch Herunterladen aus dem Azure Marketplace

1. Melden Sie sich nach der Bereitstellung von Azure Stack bei Ihrem Azure Stack Development Kit an.

2. Klicken Sie auf **Weitere Dienste** > **Marketplace Management** (Marketplace-Verwaltung) > **Add from Azure** (Aus Azure hinzufügen). 

3. Suchen Sie nach dem Image **Windows Server 2016 Datacenter – Eval**, und klicken Sie auf **Herunterladen**.

   ![Herunterladen eines Images von Azure](media/azure-stack-add-default-image/download-image.png)

Nachdem der Download abgeschlossen ist, wird das Image auf dem Blatt **Marketplace Management** (Marketplace-Verwaltung) hinzugefügt und auch auf dem Blatt **Virtuelle Computer** zur Verfügung gestellt.

## <a name="add-the-image-by-using-powershell"></a>Hinzufügen des Images mithilfe von PowerShell

### <a name="prerequisites"></a>Voraussetzungen 

Führen Sie die folgenden erforderlichen Schritte entweder über das [Development Kit](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop) oder auf einem Windows-basierten externen Client aus, wenn Sie [über VPN verbunden](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) sind:

* Installieren Sie [mit Azure Stack kompatible Azure PowerShell-Module](azure-stack-powershell-install.md).  

* Laden Sie die [Tools herunter, die zum Arbeiten mit Azure Stack erforderlich sind](azure-stack-powershell-download.md).  

* Rufen Sie https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2016 auf, und laden Sie die Windows Server 2016-Evaluierung herunter. Wählen Sie bei entsprechender Aufforderung die **ISO**-Version des Downloads aus. Notieren Sie sich den Pfad zum Downloadspeicherort, der später in dieser Anleitung verwendet wird. Für diesen Schritt ist eine Internetverbindung erforderlich.  

Führen Sie nun die folgenden Schritte aus, um das Image dem Azure Stack-Marketplace hinzuzufügen:
   
1. Importieren Sie nun mit den folgenden Befehlen das Connect- und das ComputeAdmin-Modul von Azure Stack:

   ```powershell
   Set-ExecutionPolicy RemoteSigned

   # import the Connect and ComputeAdmin modules   
   Import-Module .\Connect\AzureStack.Connect.psm1
   Import-Module .\ComputeAdmin\AzureStack.ComputeAdmin.psm1

   ```

2. Melden Sie sich bei Ihrer Azure Stack-Umgebung an. Führen Sie abhängig davon, ob die Azure Stack-Umgebung mit AAD oder AD FS bereitgestellt wird, das entsprechende Skript aus, und ersetzen Sie dabei den AAD-Mandantennamen:  

   a. Verwenden Sie für **Azure Active Directory** das folgende Cmdlet:

   ```PowerShell
   # Create the Azure Stack operator's AzureRM environment by using the following cmdlet:
   Add-AzureRMEnvironment `
     -Name "AzureStackAdmin" `
     -ArmEndpoint "https://adminmanagement.local.azurestack.external" 

   Set-AzureRmEnvironment `
    -Name "AzureStackAdmin" `
    -GraphAudience "https://graph.windows.net/"

   $TenantID = Get-AzsDirectoryTenantId `
     -AADTenantName "<myDirectoryTenantName>.onmicrosoft.com" `
     -EnvironmentName AzureStackAdmin

   Login-AzureRmAccount `
     -EnvironmentName "AzureStackAdmin" `
     -TenantId $TenantID 
   ```

   b. Verwenden Sie für **Active Directory-Verbunddienste (AD FS)** das folgende Cmdlet:
    
   ```PowerShell
   # Create the Azure Stack operator's AzureRM environment by using the following cmdlet:
   Add-AzureRMEnvironment `
     -Name "AzureStackAdmin" `
     -ArmEndpoint "https://adminmanagement.local.azurestack.external"

   Set-AzureRmEnvironment `
     -Name "AzureStackAdmin" `
     -GraphAudience "https://graph.local.azurestack.external/" `
     -EnableAdfsAuthentication:$true

   $TenantID = Get-AzsDirectoryTenantId `
     -ADFS 
     -EnvironmentName AzureStackAdmin 

   Login-AzureRmAccount `
     -EnvironmentName "AzureStackAdmin" `
     -TenantId $TenantID 
   ```
   
3. Fügen Sie das Windows Server 2016-Image zum Azure Stack-Marketplace hinzu. (Ersetzen Sie dabei *Path_to_ISO* durch den Pfad zur heruntergeladenen WS2016-ISO-Datei.)

   ```PowerShell
   $ISOPath = "<Fully_Qualified_Path_to_ISO>"

   # Add a Windows Server 2016 Evaluation VM Image.
   New-AzsServer2016VMImage `
     -ISOPath $ISOPath

   ```

Um sicherzustellen, dass das VM-Image für Windows Server 2016 das aktuelle kumulative Update enthält, nehmen Sie beim Ausführen des `New-AzsServer2016VMImage`-Cmdlets den `IncludeLatestCU`-Parameter mit auf. Informationen zu den zulässigen Parametern für das `New-AzsServer2016VMImage`-Cmdlet finden Sie im Abschnitt [Parameter](#parameters). Die Veröffentlichung des Images im Azure Stack-Marketplace dauert etwa eine Stunde. 

## <a name="parameters"></a>Parameter

|New-AzsServer2016VMImage-Parameter|Erforderlich|Beschreibung|
|-----|-----|------|
|ISOPath|Ja|Der vollqualifizierte Pfad zur heruntergeladenen ISO-Datei für Windows Server 2016|
|Net35|Nein|Dieser Parameter ermöglicht die Installation der .NET 3.5-Runtime im Windows Server 2016-Image. Standardmäßig ist dieser Wert auf „true“ festgelegt. Das Image muss die .NET 3.5-Runtime zum Installieren der SQL- und MYSQL-Ressourcenanbieter enthalten. |
|Version|Nein|Mit diesem Parameter können Sie auswählen, welches Windows Server 2016-Image hinzugefügt werden soll: **Core**, **Voll** oder **Beide**. Standardmäßig ist dieser Wert auf „Voll“ festgelegt.|
|VHDSizeInMB|Nein|Legt die Größe (in MB) des VHD-Images fest, das zu Ihrer Azure Stack-Umgebung hinzugefügt werden soll. Standardmäßig ist dieser Wert auf 40.960 MB festgelegt.|
|CreateGalleryItem|Nein|Gibt an, ob ein Marketplace-Element für das Windows Server 2016-Image erstellt werden soll. Standardmäßig ist dieser Wert auf „true“ festgelegt.|
|location |Nein |Gibt den Ort an, an dem das Windows Server 2016-Image veröffentlicht werden soll.|
|IncludeLatestCU|Nein|Legen Sie diesen Switch fest, um das aktuelle kumulative Windows Server 2016-Update auf die neue VHD anzuwenden.|
|CUUri |Nein |Legen Sie diesen Wert fest, um das kumulative Update für Windows Server 2016 aus einem bestimmten URI auszuwählen. |
|CUPath |Nein |Legen Sie diesen Wert fest, um das kumulative Update für Windows Server 2016 aus einem lokalen Pfad auszuwählen. Diese Option ist nützlich, wenn Sie die Azure Stack-Instanz in einer nicht verbundenen Umgebung bereitgestellt haben.|

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen von virtuellen Computern](azure-stack-provision-vm.md)

