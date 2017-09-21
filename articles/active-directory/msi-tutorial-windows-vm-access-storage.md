---
title: "Verwenden einer Windows-VM-MSI für den Zugriff auf Azure Storage"
description: "Dieses Tutorial erläutert, wie Sie eine Windows-VM-MSI (Managed Service Identity, verwaltete Dienstidentität) verwenden, um auf Azure Storage zuzugreifen."
services: active-directory
documentationcenter: 
author: elkuzmen
manager: mbaldwin
editor: bryanla
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2017
ms.author: elkuzmen
ms.translationtype: HT
ms.sourcegitcommit: 47ba7c7004ecf68f4a112ddf391eb645851ca1fb
ms.openlocfilehash: 86d88e3d44f442171f69d0baea5e7d689b963277
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---

# <a name="use-a-windows-vm-managed-service-identity-msi-to-access-azure-storage"></a>Verwenden einer Windows-VM-MSI (Managed Service Identity, verwaltete Dienstidentität) für den Zugriff auf Azure Storage

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

In diesem Tutorial erfahren Sie, wie Sie eine verwaltete Dienstidentität (Managed Service Identity, MSI) für einen virtuellen Linux-Computer aktivieren und diese Identität anschließend verwenden, um auf Speicherschlüssel zuzugreifen. Sie können Speicherschlüssel wie gewohnt bei Speichervorgängen verwenden, z.B. bei der Verwendung von Storage SDK. Für dieses Tutorial werden Blobs mithilfe der Azure-CLI hoch- und heruntergeladen. Sie lernen Folgendes:


> [!div class="checklist"]
> * Aktivieren von MSI auf einem virtuellen Windows-Computer 
> * Erstellen eines neuen Speicherkontos
> * Gewähren des Speicherzugriffs für den virtuellen Computer 
> * Abrufen eines Zugriffstokens für Ihr Speicherkonto durch Verwendung der VM-Identität 


Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Erstellen eines virtuellen Windows-Computers in einer neuen Ressourcengruppe

In diesem Tutorial wird ein neuer virtueller Windows-Computer erstellt. Sie können MSI auch auf einem vorhandenen virtuellen Computer aktivieren.

1.  Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu**.
2.  Wählen Sie **Compute** und dann **Windows Server 2016 Datacenter**. 
3.  Geben Sie die Informationen zum virtuellen Computer ein. Der **Benutzername** und das **Kennwort**, die hier erstellt werden, sind die Anmeldeinformationen, die Sie für die Anmeldung beim virtuellen Computer verwenden.
4.  Wählen Sie in der Dropdownliste das richtige **Abonnement** für den virtuellen Computer aus.
5.  Um eine neue **Ressourcengruppe** auszuwählen, in der der virtuelle Computer erstellt werden soll, wählen Sie **Neu erstellen** aus. Klicken Sie zum Abschluss auf **OK**.
6.  Wählen Sie eine Größe für den virtuellen Computer. Wählen Sie die Option **Alle anzeigen**, oder ändern Sie den Filter **Supported disk type** (Unterstützter Datenträgertyp), um weitere Größen anzuzeigen. Behalten Sie auf dem Blatt „Einstellungen“ die Standardwerte bei, und klicken Sie auf **OK**.

    ![Alternativer Bildtext](media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>Aktivieren von MSI auf dem virtuellen Computer

Eine VM-MSI ermöglicht es Ihnen, Zugriffstoken aus Azure AD abzurufen, ohne dass Sie Anmeldeinformationen in Ihren Code einfügen müssen. Im Hintergrund werden durch das Aktivieren von MSI zwei Vorgänge ausgelöst: Auf dem virtuellen Computer wird die MSI-VM-Erweiterung installiert, und MSI wird für den virtuellen Computer aktiviert.  

1. Wählen Sie den **virtuellen Computer** aus, auf dem Sie MSI aktivieren möchten.
2. Klicken Sie in der links angezeigten Navigationsleiste auf **Konfiguration**.
3. Die Option **Verwaltete Dienstidenität** wird angezeigt. Wählen Sie zum Registrieren und Aktivieren von MSI die Option **Ja** oder zum Deaktivieren „Nein“.
4. Achten Sie darauf, zum Speichern der Konfiguration auf **Speichern** zu klicken.

    ![Alternativer Bildtext](media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Wenn Sie überprüfen möchten, welche Erweiterungen sich auf diesem **virtuellen Windows-Computer** befinden, klicken Sie auf **Erweiterungen**. Wenn MSI aktiviert ist, wird **ManagedIdentityExtensionforWindows** in der Liste angezeigt.

    ![Alternativer Bildtext](media/msi-tutorial-linux-vm-access-arm/msi-extension-value.png)

## <a name="create-a-new-storage-account"></a>Erstellen eines neuen Speicherkontos 

Sie können Speicherschlüssel wie gewohnt bei Speichervorgängen verwenden. In diesem Beispiel liegt der Schwerpunkt auf dem Hochladen und Herunterladen von Blobs mithilfe der Azure-CLI. 

1. Wechseln Sie in die Seitenleiste, und wählen Sie **Speicher**.  
2. Erstellen Sie ein neues **Speicherkonto**.  
3. Wechseln Sie im **Bereitstellungsmodell** zum **Resource Manager**, und geben Sie als **Kontoart** die Option **Allgemein** an.  
4. Achten Sie darauf, das **Abonnement** und die **Ressourcengruppe** zu verwenden, die Sie beim Erstellen des **virtuellen Linux-Computers** im vorangegangenen Schritt auch verwendet haben.

    ![Alternativer Bildtext](media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="grant-your-vm-identity-access-to-use-storage-keys"></a>Gewähren des Zugriffs für die Verwendung von Speicherschlüsseln für die VM-Identität 

Mithilfe von MSI kann der Code Zugriffstoken zur Authentifizierung von Ressourcen abrufen, die die Azure AD-Authentifizierung unterstützen.   

1. Wechseln Sie zur Registerkarte für **Speicher**.  
2. Wählen Sie das zuvor erstellte **Speicherkonto** aus.   
3. Wechseln Sie im linken Bereich zu **Zugriffssteuerung (IAM)**.  
4. Klicken Sie dann auf **Hinzufügen**, um dem virtuellen Computer eine neue Rollenzuweisung hinzuzufügen, und wählen Sie unter **Rolle** die **Dienstrolle „Speicherkonto-Schlüsseloperator“**.  
5. Wählen Sie in der nächsten Dropdownliste für die Option **Zugriff zuweisen zu** die Ressource **Virtueller Computer** aus.  
6. Stellen Sie dann sicher, dass in der Dropdownliste **Abonnement** das richtige Abonnement aufgeführt wird. Wählen Sie unter **Ressourcengruppe** die Option **Alle Ressourcengruppen**.  
7. Wählen Sie schließlich unter **Auswählen** in der Dropdownliste Ihren virtuellen Windows-Computer aus, und klicken Sie auf **Speichern**. 

    ![Alternativer Bildtext](media/msi-tutorial-linux-vm-access-storage/msi-storage-role.png)

## <a name="get-an-access-token-using-the-vm-identity-and-use-it-to-call-azure-resource-manager"></a>Abrufen eines Zugriffstokens mithilfe der VM-Identität und Verwenden dieses Zugriffstokens zum Aufrufen von Azure Resource Manager 

In diesem Abschnitt müssen Sie **PowerShell** verwenden.  Wenn Sie PowerShell nicht installiert haben, können Sie es [hier](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1) herunterladen. 

1. Navigieren Sie im Portal zu **Virtuelle Computer**, wechseln Sie dann zu Ihrem virtuellen Windows-Computer, und klicken Sie in der **Übersicht** auf **Verbinden**. 
2. Geben Sie Ihren **Benutzernamen** und Ihr **Kennwort** ein, das Sie beim Erstellen des virtuellen Windows-Computers hinzugefügt haben. 
3. Sie haben nun eine **Remotedesktopverbindung** mit dem virtuellen Computer erstellt. Öffnen Sie jetzt **PowerShell** in der Remotesitzung. 
4. Erstellen Sie mithilfe des PowerShell-Befehls „Invoke-WebRequest“ eine Anforderung an den lokalen MSI-Endpunkt, um ein Zugriffstoken für Azure Resource Manager zu erhalten.

    ```powershell
       $response = Invoke-WebRequest -Uri http://localhost/50342/oauth2/token -Method GET -Body @resource="https://management.azure.com/"} -Headers @{Metadata="true"}
    ```
    
    > [!NOTE]
    > Der Wert des „resource“-Parameters muss exakt mit dem von Azure AD erwarteten Wert übereinstimmen. Wenn Sie die Azure Resource Manager-Ressourcen-ID verwenden, müssen Sie den nachgestellten Schrägstrich im URI verwenden.
    
    Extrahieren Sie als Nächstes die vollständige Antwort, die als JSON-formatierte Zeichenfolge (JavaScript Object Notation) im „$response“-Objekt gespeichert ist. 
    
    ```powershell
    $content = $repsonse.Content | ConvertFrom-Json
    ```
    Extrahieren Sie dann das Zugriffstoken aus der Antwort.
    
    ```powershell
    $ArmToken = $content.access_token
    ```
    
    Rufen Sie abschließend Azure Resource Manager mit dem Zugriffstoken auf. In diesem Beispiel wird auch der PowerShell-Befehl „Invoke-WebRequest“ für den Aufruf von Azure Resource Manager und das Einschließen des Zugriffstokens im Autorisierungsheader verwendet.
    
    ```powershell
    (Invoke-WebRequest -Uri https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>?api-version=2016-06-01 -Method GET -ContentType "application/json" -Headers @{ Authorization ="Bearer $ArmToken"}).content
    ```
    > [!NOTE]
    > Bei der URL wird die Groß-/Kleinschreibung beachtet. Achten Sie daher darauf, dieselbe Groß- und Kleinschreibung zu verwenden wie zuvor beim Benennen der Ressourcengruppe und den Großbuchstaben „G“ in „resourceGroup“ zu beachten.
    
## <a name="get-the-storage-keys-from-azure-resource-manager"></a>Abrufen der Speicherschlüssel aus Azure Resource Manager 

```powershell
PS C:\> $keysResponse = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/97f51385-2edc-4b69-bed8-7778dd4cb761/resourceGroups/SKwan_Test/providers/Microsoft.Storage/storageAccounts/skwanteststorage/listKeys/?api-version=2016-12-01 -Method POST$ -Headers @{Authorization="Bearer $ARMToken"}
```

```powershell
PS C:\> $keysContent = $keysResponse.Content | ConvertFrom-Json
```

```powershell
PS C:\> $key = $keysContent.keys[0].value
```

**Erstellen einer Datei, die mit der Azure-CLI hochgeladen werden soll**

```bash
echo "This is a test text file." > test.txt
```

**Hochladen der Datei mithilfe der Azure-CLI und Authentifizieren der Datei mit dem Speicherschlüssel**

> [!NOTE]
> Denken Sie zuerst daran, die Azure Storage-Cmdlets „Install-Module Azure.Storage“ zu installieren. 

PowerShell-Anforderung:


```powershell
PS C:\> $ctx = New-AzureStorageContext -StorageAccountName skwanteststorage -StorageAccountKey $key
PS C:\> Set-AzureStorageBlobContent -File test.txt -Container testcontainer -Blob testblob -Context $ctx
```

Antwort:

```powershell
ICloudBlob        : Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob
BlobType          : BlockBlob
Length            : 56
ContentType       : application/octet-stream
LastModified      : 9/13/2017 6:14:25 PM +00:00
SnapshotTime      :
ContinuationToken :
Context           : Microsoft.WindowsAzure.Commands.Storage.AzureStorageContext
Name              : testblob
```

**Herunterladen der Datei mithilfe der Azure-CLI und Authentifizieren der Datei mit dem Speicherschlüssel**

PowerShell-Anforderung:

```powershell
PS C:\> Get-AzureStorageBlobContent -Blob <blob name> -Container <container name> -Destination <file> -Context $ctx
```

Antwort:

```powershell
ICloudBlob        : Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob
BlobType          : BlockBlob
Length            : 56
ContentType       : application/octet-stream
LastModified      : 9/13/2017 6:14:25 PM +00:00
SnapshotTime      :
ContinuationToken :
Context           : Microsoft.WindowsAzure.Commands.Storage.AzureStorageContext
Name              : testblob
```





