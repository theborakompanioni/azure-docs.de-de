---
title: "Verwenden einer Linux-VM-MSI für den Zugriff auf Azure Storage"
description: "Dieses Tutorial erläutert, wie Sie eine Linux-VM-MSI (Managed Service Identity, verwaltete Dienstidentität) verwenden, um auf Azure Storage zuzugreifen."
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
ms.openlocfilehash: 9fec51528d1203a7107558408ced42fa2b2b884a
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---


# <a name="use-managed-service-identity-msi-with-a-linux-vm-to-access-storage-credentials"></a>Verwenden der verwalteten Dienstidentität (Managed Service Identity, MSI) mit einer Linux-VM für den Zugriff auf Speicheranmeldeinformationen

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

In diesem Tutorial erfahren Sie, wie Sie eine verwaltete Dienstidentität (Managed Service Identity, MSI) für einen virtuellen Linux-Computer aktivieren und diese Identität anschließend verwenden, um auf Speicherschlüssel zuzugreifen. Sie können Speicherschlüssel wie gewohnt bei Speichervorgängen verwenden, z.B. bei der Verwendung von Storage SDK. Für dieses Tutorial werden Blobs mithilfe der Azure-CLI hoch- und heruntergeladen. Sie lernen Folgendes:


> [!div class="checklist"]
> * Aktivieren von MSI auf einem virtuellen Linux-Computer 
> * Erstellen eines neuen Speicherkontos
> * Gewähren des Speicherzugriffs für den virtuellen Computer
> * Abrufen eines Zugriffstokens für Ihr Speicherkonto durch Verwendung der VM-Identität zum Zugriff auf Speicherschlüssel 


Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure
Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.


## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Erstellen eines virtuellen Linux-Computers in einer neuen Ressourcengruppe

In diesem Tutorial wird ein neuer virtueller Linux-Computer erstellt. Sie können MSI auch auf einem vorhandenen virtuellen Computer aktivieren.

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu**.
2. Wählen Sie **Compute** und dann **Ubuntu Server 16.04 LTS**.
3. Geben Sie die Informationen zum virtuellen Computer ein. Wählen Sie unter **Authentifizierungstyp** die Option **Öffentlicher SSH-Schlüssel** oder **Kennwort**. Mit den erstellten Anmeldeinformationen können Sie sich auf dem virtuellen Computer anmelden.

    ![Alternativer Bildtext](media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Wählen Sie in der Dropdownliste ein **Abonnement** für den virtuellen Computer aus.
5. Um eine neue **Ressourcengruppe** auszuwählen, in der der virtuelle Computer erstellt werden soll, wählen Sie **Neu erstellen** aus. Klicken Sie zum Abschluss auf **OK**.
6. Wählen Sie eine Größe für den virtuellen Computer. Wählen Sie die Option **Alle anzeigen**, oder ändern Sie den Filter für den unterstützten Datenträgertyp, um weitere Größen anzuzeigen. Behalten Sie auf dem Blatt „Einstellungen“ die Standardwerte bei, und klicken Sie auf **OK**.

## <a name="enable-msi-on-your-vm"></a>Aktivieren von MSI auf dem virtuellen Computer

Eine VM-MSI ermöglicht es Ihnen, Zugriffstoken aus Azure AD abzurufen, ohne dass Sie Anmeldeinformationen in Ihren Code einfügen müssen. Im Hintergrund werden durch das Aktivieren von MSI zwei Vorgänge ausgelöst: Auf dem virtuellen Computer wird die MSI-VM-Erweiterung installiert, und MSI wird für den virtuellen Computer aktiviert.  

1. Wählen Sie den **virtuellen Computer** aus, auf dem Sie MSI aktivieren möchten.
2. Klicken Sie in der links angezeigten Navigationsleiste auf **Konfiguration**.
3. Die Option **Verwaltete Dienstidenität** wird angezeigt. Wählen Sie zum Registrieren und Aktivieren von MSI die Option **Ja** oder zum Deaktivieren „Nein“.
4. Achten Sie darauf, zum Speichern der Konfiguration auf **Speichern** zu klicken.

    ![Alternativer Bildtext](media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Wenn Sie überprüfen möchten, welche Erweiterungen sich auf diesem **virtuellen Linux-Computer** befinden, klicken Sie auf **Erweiterungen**. Wenn MSI aktiviert ist, wird **ManagedIdentityExtensionforLinux** in der Liste angezeigt.

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
3. Wechseln Sie linken Bereich zu **Zugriffssteuerung (IAM)**.  
4. Klicken Sie dann auf **Hinzufügen**, um dem virtuellen Computer eine neue Rollenzuweisung hinzuzufügen, und wählen Sie unter **Rolle** die **Dienstrolle „Speicherkonto-Schlüsseloperator“**.  
5. Wählen Sie in der nächsten Dropdownliste für die Option **Zugriff zuweisen zu** die Ressource **Virtueller Computer** aus.  
6. Stellen Sie dann sicher, dass in der Dropdownliste **Abonnement** das richtige Abonnement aufgeführt wird. Wählen Sie unter **Ressourcengruppe** die Option **Alle Ressourcengruppen**.  
7. Wählen Sie schließlich unter **Auswählen** in der Dropdownliste Ihren virtuellen Linux-Computer aus, und klicken Sie auf **Speichern**. 
    ![Alternativer Bildtext](media/msi-tutorial-linux-vm-access-storage/msi-storage-role.png)

## <a name="get-an-access-token-using-the-vm-identity-and-use-it-to-call-azure-resource-manager"></a>Abrufen eines Zugriffstokens mithilfe der VM-Identität und Verwenden dieses Zugriffstokens zum Aufrufen von Azure Resource Manager

Hierfür benötigen Sie das Bash-Terminal. [Hier](https://msdn.microsoft.com/commandline/wsl/install_guide) können Sie eine Linux-Distribution auswählen und herunterladen.


1. Navigieren Sie im Portal zu Ihrem virtuellen Linux-Computer, und klicken Sie in der **Übersicht** auf **Verbinden**. Sie werden aufgefordert, Bash zu verwenden. Notieren Sie sich die in der Benachrichtigung angegebene SSH und VM-IP. 
2. Öffnen Sie Bash, und stellen Sie eine Verbindung her.  
3. Geben Sie im Terminal Ihre **SSH** und den **virtuellen Computer** ein, mit dem Sie eine Verbindung herstellen möchten, zum Beispiel: „ssh admin@12.61.219.35“.  
4. Als Nächstes werden Sie aufgefordert, Ihr **Kennwort** einzugeben, das Sie hinzugefügt haben, als Sie den **virtuellen Linux-Computer** erstellt haben. Anschließend sollten Sie erfolgreich angemeldet werden.  
5. Dann können Sie mit CURL eine Anforderung erstellen, um das Autorisierungstoken für den virtuellen Linux-Computer abzurufen, bei dem Sie angemeldet sind. Der **Azure Resource Manager**-Endpunkt lautet https://management.azure.com.  

    Die CURL-Anforderung für das Zugriffstoken finden Sie weiter unten:
    
    ```bash
    curl --data "authority= https://login.microsoftonline.com/<TENANT ID>&&resource=https://management.azure.com/"  -H Metadata:true http://localhost:50432/oauth2/token   
    ```
    
    > [!NOTE]
    > Stellen Sie sicher, dass die URL der Ressource, für die Sie den Zugriff anfordern, ordnungsgemäß mit einem Schrägstrich am Ende formatiert ist. Beispiel: https:<RESOURCE>/
    
    ```powershell
    {"access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IkhIQnlLVS0wRHFBcU1aaDZaRlBkMlZXYU90ZyIsImtpZCI6IkhIQnlLVS0wRHFBcU1aaDZaRlBkMlZXYU90ZyJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvNzJmOTg4YmYtODZmMS00MWFmLTkxYWItMmQ3Y2QwMTFkYjQ3LyIsImlhdCI6MTUwNDEyNjYyNywibmJmIjoxNTA0MTI2NjI3LCJleHAiOjE1MDQxMzA1MjcsImFpbyI6IlkyRmdZTGg2dENWSzRkSDlGWGtuZzgyQ21ZNVdBZ0E9IiwiYXBwaWQiOiI2ZjJmNmU2OS04MGExLTQ3NmEtOGRjZi1mOTgzZDZkMjUxYjgiLCJhcHBpZGFjciI6IjIiLCJpZHAiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDcvIiwib2lkIjoiMTEyODJiZDgtMDNlMi00NGVhLTlmYjctZTQ1YjVmM2JmNzJlIiwic3ViIjoiMTEyODJiZDgtMDNlMi00NGVhLTlmYjctZTQ1YjVmM2JmNzJlIiwidGlkIjoiNzJmOTg4YmYtODZmMS00MWFmLTkxYWItMmQ3Y2QwMTFkYjQ3IiwidXRpIjoib0U5T3JVZFJMMHVKSEw4UFdvOEJBQSIsInZlciI6IjEuMCJ9.J6KS7b9kFgDkegJ-Vfff19LMnu3Cfps4dL2uNGucb5M76rgDM5f73VO-19wZSRhQPxWmZLETzN3SljnIMQMkYWncp79MVdBud_xqXYyLdQpGkNinpKVJhTo1j1dY27U_Cjl4yvvpBTrtH3OX9gG0GtQs7PBFTTLznqcH3JR9f-bTSEN4wUhalaIPHPciVDtJI9I24_vvMfVqxkXOo6gkL0mEP"}
     ```
    
## <a name="the-curl-request-to-get-storage-keys-from-azure-resource-manager"></a>Die CURL-Anforderung zum Abrufen von Speicherschlüsseln aus Azure Resource Manager  

> [!NOTE]
> Bei dem Text in der URL wird die Groß-/Kleinschreibung beachtet. Wenn Sie also für Ihre Ressourcengruppen Groß- und Kleinschreibung verwenden, achten Sie darauf, diese Schreibung richtig widerzuspiegeln. Wichtig ist darüber hinaus die Tatsache, dass es sich um eine POST-Anforderung und nicht um eine GET-Anforderung handelt. Stellen Sie sicher, dass Sie mit -d einen Wert zum Erfassen einer Längenbeschränkung übergeben, der NULL sein kann.  

```bash 
curl https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE ACCOUNT NAME>/listKeys?api-version=2016-12-01 –request POST -d"" -H "Authorization: Bearer <ACCESS TOKEN>" 
```

Die CURL-Antwort stellt die Liste der Schlüssel bereit:  

```bash 
{"keys":[{"keyName":"key1","permissions":"Full","value":"iqDPNtXcluBiFSV2Fs+JTj6c+VK5wWB4RWUVYw2Y5BkRQu21cRpAztt3S19bAowhGPqJrAfMr3gn89Ui9VPbtA=="},{"keyName":"key2","permissions":"Full","value":"U+uI0B1Iygw1bDK6Ic1VWKMMY77ac0VV3kqYaCW7FcUFiuw4UkLfEKbnF2sZpIfFR9zUWniLBhlt+eUn/TeSJg=="}]} 
```


Erstellen Sie eine Datei, die hochgeladen werden soll. Dies ist eine Blob-Beispieldatei, die Sie in Ihrem Speicherkonto im erstellten Container mit Ihren Speicherschlüsseln hochladen können. 

Dies ist auf einem virtuellen Linux-Computer mit dem folgenden Befehl möglich. 

```bash
echo "This is a test file." > test.txt
```
 Dann können Sie die Datei mithilfe der Azure-CLI hochladen und mit dem Speicherschlüssel authentifizieren.
 

```azurecli-interactive
 az storage blob upload --container-name 
                        --file 
                        --name 
                        [--account-name] 
                        [--account-key] 
```

Antwort: 

```JSON
Finished[#############################################################]  100.0000%
{
  "etag": "\"0x8D4F9929765C139\"",
  "lastModified": "2017-09-12T03:58:56+00:00"
}
```

Zudem können Sie die Datei mithilfe der Azure-CLI herunterladen und mit dem Speicherschlüssel authentifizieren. 

Anforderung: 

```azurecli-interactive
az storage blob download --container-name
                         --file 
                         --name 
                         [--account-name]
                         [--account-key]  
```

Antwort: 

```JSON
{
  "content": null,
  "metadata": {},
  "name": "testblob",
  "properties": {
    "appendBlobCommittedBlockCount": null,
    "blobType": "BlockBlob",
    "contentLength": 16,
    "contentRange": "bytes 0-15/16",
    "contentSettings": {
      "cacheControl": null,
      "contentDisposition": null,
      "contentEncoding": null,
      "contentLanguage": null,
      "contentMd5": "Aryr///Rb+D8JQ8IytleDA==",
      "contentType": "text/plain"
    },
    "copy": {
      "completionTime": null,
      "id": null,
      "progress": null,
      "source": null,
      "status": null,
      "statusDescription": null
    },
    "etag": "\"0x8D4F9929765C139\"",
    "lastModified": "2017-09-12T03:58:56+00:00",
    "lease": {
      "duration": null,
      "state": "available",
      "status": "unlocked"
    },
    "pageBlobSequenceNumber": null,
    "serverEncrypted": false
  },
  "snapshot": null
}
```

## <a name="related-content"></a>Verwandte Inhalte

Einen Überblick über MSI finden Sie unter [Übersicht über verwaltete Dienstidentitäten](../active-directory/msi-overview.md).

Verwenden Sie den folgenden Kommentarabschnitt, um uns Feedback zu senden und uns bei der Verbesserung unserer Inhalte zu unterstützen.


