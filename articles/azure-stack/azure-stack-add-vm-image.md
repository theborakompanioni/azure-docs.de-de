---
title: "Hinzufügen eines VM-Images zu Azure Stack | Microsoft-Dokumentation"
description: "Hinzufügen des benutzerdefinierten Windows- oder Linux-VM-Images Ihrer Organisation für die Verwendung durch Mandanten"
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: e5a4236b-1b32-4ee6-9aaa-fcde297a020f
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: sngun
ms.translationtype: HT
ms.sourcegitcommit: 1c730c65194e169121e3ad1d1423963ee3ced8da
ms.openlocfilehash: e726ef05632c7983a45fae191bb0a2ad18fc2553
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---
# <a name="make-a-custom-virtual-machine-image-available-in-azure-stack"></a>Verfügbarmachen eines benutzerdefinierten VM-Images in Azure Stack

Azure Stack ermöglicht Betreibern die Bereitstellung benutzerdefinierter VM-Images für ihre Benutzer. Auf diese Images kann von Azure Resource Manager-Vorlagen verwiesen werden, oder die Images können der Azure Marketplace-Benutzeroberfläche bei der Erstellung eines Marketplace-Elements hinzugefügt werden. 

## <a name="add-a-vm-image-to-marketplace-with-powershell"></a>Hinzufügen eines VM-Images zu Marketplace mithilfe von PowerShell

Führen Sie die folgenden erforderlichen Schritte entweder über das [Entwicklungskit](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop) oder auf einem Windows-basierten externen Client aus, wenn Sie [über VPN verbunden](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) sind.

* [Installieren Sie PowerShell für Azure Stack.](azure-stack-powershell-install.md)  

* Laden Sie die [Tools, die für die Arbeit mit Azure Stack benötigt werden](azure-stack-powershell-download.md), herunter.  

* Bereiten Sie ein virtuelles Festplattenimage mit einem Windows- oder Linux-Betriebssystem im VHD-Format (nicht VHDX) vor:
   
   * Anweisungen zur Vorbereitung von Windows-Images finden Sie im Artikel [Hochladen einer generalisierten VHD und Verwendung dieser zum Erstellen neuer VMs in Azure](../virtual-machines/windows/upload-generalized-managed.md) im Abschnitt **Vorbereiten der VHD für den Upload**.
   * Führen Sie bei Linux-Images die Schritte zum Vorbereiten des Images aus, oder verwenden Sie ein vorhandenes Azure Stack-Linux-Image, wie im Artikel [Deploy Linux virtual machines on Azure Stack](azure-stack-linux.md) (Bereitstellen virtueller Linux-Computer in Azure Stack) beschrieben.  

Führen Sie nun die folgenden Schritte aus, um das Image dem Azure Stack-Marketplace hinzuzufügen:

1. Importieren Sie das Connect- und das ComputeAdmin-Modul:
   
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
    
3. Fügen Sie das VM-Image durch Aufruf des `Add-AzsVMImage`-Cmdlets hinzu. Geben Sie im Add-AzsVMImage-Cmdlet für „osType“ entweder „Windows“ oder „Linux“ an. Fügen Sie den Herausgeber, das Angebot, die SKU und die Version für das VM-Image ein. Informationen zu den zulässigen Parametern finden Sie im Abschnitt [Parameter](#parameters). Diese Parameter werden von Azure Resource Manager-Vorlagen zum Verweisen auf das VM-Image verwendet. Nachfolgend sehen Sie einen Beispielaufruf des Skripts:
     
     ```powershell
     Add-AzsVMImage `
       -publisher "Canonical" `
       -offer "UbuntuServer" `
       -sku "14.04.3-LTS" `
       -version "1.0.0" `
       -osType Linux `
       -osDiskLocalPath 'C:\Users\AzureStackAdmin\Desktop\UbuntuServer.vhd' `
     ```

Der Befehl bewirkt Folgendes:

* Authentifizieren bei der Azure Stack-Umgebung
* Hochladen der lokalen VHD in ein neu erstelltes temporäres Speicherkonto
* Hinzufügen des VM-Images zum VM-Image-Repository
* Erstellen eines Marketplace-Elements

Um sich zu vergewissern, dass der Befehl ausgeführt wurde, navigieren Sie im Portal zum Marketplace, und überprüfen Sie, ob das VM-Image in der Kategorie **Virtuelle Computer** verfügbar ist.

![Hinzugefügtes VM-Image](./media/azure-stack-add-vm-image/image5.PNG) 

## <a name="remove-a-vm-image-with-powershell"></a>Entfernen eines VM-Images mit PowerShell

Wenn Sie das zuvor hochgeladene VM-Image nicht mehr benötigen, können Sie es im Marketplace mit dem folgenden Cmdlet entfernen:

```powershell
Remove-AzsVMImage `
  -publisher "Canonical" `
  -offer "UbuntuServer" `
  -sku "14.04.3-LTS" `
  -version "1.0.0" `
```

## <a name="parameters"></a>Parameter

| Parameter | Beschreibung |
| --- | --- |
| **publisher** |Das von Benutzern bei der Bereitstellung des Images verwendete Segment mit dem Herausgebernamen des VM-Images. Beispiel: Microsoft. Dieses Feld darf kein Leerzeichen und kein anderes Sonderzeichen enthalten. |
| **offer** |Das von Benutzern bei der Bereitstellung des Images verwendete Segment mit dem Angebotsnamen des VM-Images. Beispiel: WindowsServer. Dieses Feld darf kein Leerzeichen und kein anderes Sonderzeichen enthalten. |
| **sku** |Das von Benutzern bei der Bereitstellung des Images verwendete Segment mit dem SKU-Namen des VM-Images. Beispiel: Datacenter2016. Dieses Feld darf kein Leerzeichen und kein anderes Sonderzeichen enthalten. |
| **version** |Die von Benutzern bei der Bereitstellung des Images verwendete Version des VM-Images. Diese Version wird im Format *\#.\#.\#* angegeben. Beispiel: 1.0.0. Dieses Feld darf kein Leerzeichen und kein anderes Sonderzeichen enthalten. |
| **osType** |Als Betriebssystemtyp des Images muss „Windows“ oder „Linux“ angegeben werden. |
| **osDiskLocalPath** |Der lokale Pfad zur VHD mit dem Betriebssystem-Datenträger, die Sie als VM-Image in Azure Stack hochladen |
| **dataDiskLocalPaths** |Ein optionales Array der lokalen Pfade für Datenträger, die als Teil des VM-Images hochgeladen werden können |
| **CreateGalleryItem** |Ein boolesches Flag, das angibt, ob ein Element im Marketplace erstellt werden soll. Standardmäßig ist „true“ festgelegt. |
| **title** |Der Anzeigename des Marketplace-Elements. Standardmäßig ist die Herausgeberangebots-SKU des VM-Images angegeben. |
| **description** |Die Beschreibung des Marketplace-Elements |
| **location** |Der Ort, an dem das VM-Image veröffentlicht werden soll. Standardmäßig ist dieser Wert auf „lokal“ festgelegt.|
| **osDiskBlobURI** |Dieses Skript akzeptiert optional auch einen Blobspeicher-URI für „osDisk“. |
| **dataDiskBlobURIs** |Dieses Skript akzeptiert optional auch ein Array mit Blobspeicher-URIs zum Hinzufügen von Datenträgern zum Image. |

## <a name="add-a-vm-image-through-the-portal"></a>Hinzufügen eines VM-Images über das Portal

> [!NOTE]
> Bei dieser Methode muss das Marketplace-Element separat erstellt werden.

Eine Anforderung von Images besteht darin, dass auf sie durch einen Blobspeicher-URI verwiesen werden kann. Bereiten Sie ein Image mit Windows- oder Linux-Betriebssystem im VHD-Format (nicht VHDX) vor, und laden Sie es anschließend in ein Speicherkonto in Azure oder Azure Stack hoch. Falls Ihr Image bereits in den Blobspeicher in Azure oder Azure Stack hochgeladen wurde, können Sie Schritt 1 überspringen.

1. [Laden Sie ein Windows-VM-Image in Azure für Resource Manager-Bereitstellungen](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/) hoch. Befolgen Sie bei einem Linux-Image die Anweisungen im Artikel [Deploy Linux virtual machines on Azure Stack](azure-stack-linux.md) (Bereitstellen von virtuellen Linux-Computern in Azure Stack). Beachten Sie vor dem Hochladen des Images die folgenden Überlegungen:

   * Es ist effizienter, ein Image in den Azure Stack-Blobspeicher als in Azure Blob Storage hochzuladen, da die Übertragung des Images per Push in das Azure Stack-Image-Repository schneller geht. 
   
   * Stellen Sie beim Hochladen des [Windows-VM-Images](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/) sicher, dass der Schritt **Anmelden bei Azure** durch den Schritt [Konfigurieren der PowerShell-Umgebung des Azure Stack-Betreibers](azure-stack-powershell-configure-admin.md) ersetzt wird.  

   * Notieren Sie sich den Blobspeicher-URI, zu dem Sie das Image hochladen. Er hat folgendes Format: *&lt;storageAccount&gt;/&lt;blobContainer&gt;/&lt;targetVHDName&gt;*.vhd.

   * Damit auf das Blob anonym zugegriffen werden kann, navigieren Sie zum Speicherkonto-Blobcontainer, in dem die VM-Image-VHD in **Blob** hochgeladen wurde. Klicken Sie dann auf **Zugriffsrichtlinie**. Sie können stattdessen auch eine Shared Access Signature für den Container erstellen und als Teil des Blob-URIs aufnehmen.

   ![Navigieren zu Speicherkontoblobs](./media/azure-stack-add-vm-image/image1.png)

   ![Festlegen des öffentlichen Blobzugriffs](./media/azure-stack-add-vm-image/image2.png)

2. Melden Sie sich bei Azure Stack als Betreiber an. Klicken Sie im Menü auf **Weitere Dienste** > **Ressourcenanbieter**, und wählen Sie **Compute** > **VM-Images** > **Hinzufügen**.

3. Geben Sie auf dem Blatt **Add a VM Image** (VM-Image hinzufügen) den Herausgeber, das Angebot, die SKU und die Version des VM-Images ein. Diese Namenssegmente verweisen in Resource Manager-Vorlagen auf das VM-Image. Stellen Sie sicher, dass für **osType** der richtige Typ ausgewählt ist. Geben Sie unter **OS Disk Blob URI** (Blob-URI des Betriebssystemdatenträgers) den Blob-URI ein, zu dem das Image hochgeladen wurde. Klicken Sie anschließend auf **Erstellen**, um mit der Erstellung des VM-Images zu beginnen.
   
   ![Starten der Imageerstellung](./media/azure-stack-add-vm-image/image4.png)

   Wenn das Image erstellt wurde, ändert sich der VM-Imagestatus in „Erfolgreich“.

4. Damit über die Benutzeroberfläche leichter auf das VM-Image zugegriffen werden kann, wird die [Erstellung eines Marketplace-Elements](azure-stack-create-and-publish-marketplace-item.md) empfohlen.

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen von virtuellen Computern](azure-stack-provision-vm.md)
