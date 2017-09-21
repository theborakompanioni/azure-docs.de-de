---
title: "Verwenden einer Windows-VM-MSI für den Zugriff auf Azure Key Vault"
description: "Dieses Tutorial erläutert, wie Sie eine Windows-VM-MSI (Managed Service Identity, verwaltete Dienstidentität) verwenden, um auf Azure Key Vault zuzugreifen."
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
ms.openlocfilehash: 1785b4da8c54354dbc48c514dbb8f969a1f997ca
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---

# <a name="use-managed-service-identity-msi-with-a-windows-vm-to-access-azure-key-vault"></a>Verwenden der verwalteten Dienstidentität (Managed Service Identity, MSI) mit einer Windows-VM für den Zugriff auf Azure Key Vault 

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

In diesem Tutorial erfahren Sie, wie Sie eine verwaltete Dienstidentität (Managed Service Identity, MSI) für einen virtuellen Windows-Computer aktivieren und diese Identität anschließend verwenden, um auf Azure Key Vault zuzugreifen. Verwaltete Dienstidentitäten werden von Azure automatisch verwaltet und ermöglichen Ihnen die Authentifizierung für Dienste, die die Azure AD-Authentifizierung unterstützen, ohne dass Sie Anmeldeinformationen in Ihren Code einfügen müssen. Sie lernen Folgendes:


> [!div class="checklist"]
> * Aktivieren einer verwalteten Dienstidentität auf einem virtuellen Windows-Computer 
> * Gewähren des Zugriffs auf ein in einer Key Vault gespeicherten Geheimnisses für den virtuellen Computer 
> * Abrufen eines Zugriffstokens mithilfe der VM-Identität und Verwenden dieses Zugriffstokens zum Abrufen des Geheimnisses aus der Key Vault 


Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Erstellen eines virtuellen Windows-Computers in einer neuen Ressourcengruppe

In diesem Tutorial wird ein neuer virtueller Windows-Computer erstellt. Sie können MSI auch auf einem vorhandenen virtuellen Computer aktivieren.

1.  Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu**.
2.  Wählen Sie **Compute** und dann **Windows Server 2016 Datacenter**. 
3.  Geben Sie die Informationen zum virtuellen Computer ein. Der **Benutzername** und das **Kennwort**, die hier erstellt werden, sind die Anmeldeinformationen, die Sie für die Anmeldung beim virtuellen Computer verwenden.
4.  Wählen Sie in der Dropdownliste das geeignete **Abonnement** für den virtuellen Computer aus.
5.  Um eine neue **Ressourcengruppe** auszuwählen, in der der virtuelle Computer erstellt werden soll, wählen Sie **Neu erstellen** aus. Klicken Sie zum Abschluss auf **OK**.
6.  Wählen Sie eine Größe für den virtuellen Computer. Wählen Sie die Option **Alle anzeigen**, oder ändern Sie den Filter **Supported disk type** (Unterstützter Datenträgertyp), um weitere Größen anzuzeigen. Behalten Sie auf dem Blatt „Einstellungen“ die Standardwerte bei, und klicken Sie auf **OK**.

    ![Alternativer Bildtext](media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>Aktivieren von MSI auf dem virtuellen Computer 

Eine VM-MSI ermöglicht es Ihnen, Zugriffstoken aus Azure AD abzurufen, ohne dass Sie Anmeldeinformationen in Ihren Code einfügen müssen. Durch das Aktivieren von MSI weisen Sie Azure an, eine verwaltete Identität für den virtuellen Computer zu erstellen. Im Hintergrund werden durch das Aktivieren von MSI zwei Vorgänge ausgelöst: Auf dem virtuellen Computer wird die MSI-VM-Erweiterung installiert, und MSI wird im Azure Resource Manager aktiviert.

1.  Wählen Sie den **virtuellen Computer** aus, auf dem Sie MSI aktivieren möchten.  
2.  Klicken Sie in der links angezeigten Navigationsleiste auf **Konfiguration**. 
3.  Die Option **Verwaltete Dienstidenität** wird angezeigt. Wählen Sie zum Registrieren und Aktivieren von MSI die Option **Ja** oder zum Deaktivieren „Nein“. 
4.  Achten Sie darauf, zum Speichern der Konfiguration auf **Speichern** zu klicken.  

    ![Alternativer Bildtext](media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Wenn Sie überprüfen möchten, welche Erweiterungen sich auf diesem virtuellen Computer befinden, klicken Sie auf **Erweiterungen**. Wenn MSI aktiviert ist, wird **ManagedIdentityExtensionforWindows** in der Liste angezeigt.

    ![Alternativer Bildtext](media/msi-tutorial-windows-vm-access-arm/msi-windows-extension.png)

## <a name="grant-your-vm-access-to-a-secret-stored-in-a-key-vault"></a>Gewähren des Zugriffs auf ein in einer Key Vault gespeicherten Geheimnisses für den virtuellen Computer 
 
Mithilfe von MSI kann der Code Zugriffstoken zur Authentifizierung von Ressourcen abrufen, die die Azure AD-Authentifizierung unterstützen.  Allerdings unterstützen nicht alle Azure-Dienste die Azure AD-Authentifizierung. Um MSI mit Diensten zu verwenden, die die Azure AD-Authentifizierung nicht unterstützen, können Sie die für diese Dienste erforderlichen Anmeldeinformationen in Azure Key Vault speichern und MSI für die Authentifizierung bei Azure Key Vault verwenden, um die Anmeldeinformationen abzurufen. 

Zunächst müssen Sie eine Key Vault erstellen und der Identität des virtuellen Computers den Zugriff darauf erteilen.   

1. Wählen Sie oben auf der linken Navigationsleiste **+ Neu**, dann **Sicherheit und Identität** und anschließend **Key Vault**.  
2. Geben Sie einen Namen für die neue **Key Vault** an. 
3. Suchen Sie die Key Vault in dem Abonnement und in der Ressourcengruppe, in der sich auch der zuvor erstellte virtuelle Computer befindet. 
4. Wählen Sie **Zugriffsrichtlinien** aus, und klicken Sie auf **Neue hinzufügen**. 
5. Wählen Sie unter „Anhand einer Vorlage konfigurieren“ die Option **Verwaltung von Geheimnissen** aus. 
6. Wählen Sie **Prinzipal auswählen**, und geben Sie im Suchfeld den Namen des zuvor erstellten virtuellen Computers ein.  Wählen Sie in der Ergebnisliste den virtuellen Computer aus, und klicken Sie auf **Auswählen**. 
7. Klicken Sie auf **OK**, um die neue Zugriffsrichtlinie hinzuzufügen. Klicken Sie dann erneut auf **OK**, um die Auswahl der Zugriffsrichtlinie abzuschließen. 
8. Klicken Sie auf **Erstellen**, um die Key Vault zu erstellen. 

    ![Alternativer Bildtext](media/msi-tutorial-windows-vm-access-nonaad/msi-blade.png)


Fügen Sie der Key Vault nun ein Geheimnis hinzu, das Sie später mithilfe von Code abrufen können, der auf dem virtuellen Computer ausgeführt wird: 

1. Wählen Sie **Alle Ressourcen** aus, suchen Sie die soeben erstellte Key Vault, und wählen Sie diese aus. 
2. Wählen Sie **Geheimnisse** aus, und klicken Sie auf **Hinzufügen**. 
3. Wählen Sie unter den **Uploadoptionen** die Option **Manuell** aus. 
4. Geben Sie einen Namen und einen Wert für das Geheimnis ein.  Dabei kann es sich um einen beliebigen Wert handeln. 
5. Lassen Sie das Aktivierungs- und das Ablaufdatum leer, und übernehmen Sie für **Aktiviert** die ausgewählte Option **Ja**. 
6. Klicken Sie auf **Erstellen**, um das Geheimnis zu erstellen. 
 
## <a name="get-an-access-token-using-the-vm-identity-and-use-it-retrieve-the-secret-from-the-key-vault"></a>Abrufen eines Zugriffstokens mithilfe der VM-Identität und Verwenden dieses Zugriffstokens zum Abrufen des Geheimnisses aus der Key Vault  

Nachdem Sie ein Geheimnis erstellt, dieses in einer Key Vault gespeichert und der VM-MSI den Zugriff auf die Key Vault gewährt haben, können Sie nun Code schreiben, um das Geheimnis zur Laufzeit abzurufen.  Zur Vereinfachung dieses Beispiels werden hier einfache REST-Aufrufe mit PowerShell verwendet.  Wenn Sie PowerShell nicht installiert haben, können Sie es [hier](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1) herunterladen.

Zunächst wird die VM-MSI verwendet, um ein Zugriffstoken für die Authentifizierung bei der Key Vault abzurufen:
 
1. Navigieren Sie im Portal zu **Virtuelle Computer**, wechseln Sie dann zu Ihrem virtuellen Windows-Computer, und klicken Sie in der **Übersicht** auf **Verbinden**.
2. Geben Sie Ihren **Benutzernamen** und Ihr **Kennwort** ein, das Sie beim Erstellen des **rs** hinzugefügt haben.  
3. Sie haben nun eine **Remotedesktopverbindung** mit dem virtuellen Computer erstellt. Öffnen Sie jetzt PowerShell in der Remotesitzung.  
4. Rufen Sie in PowerShell die Webanforderung für den Mandanten auf, um das Token für den lokalen Host am spezifischen Port des virtuellen Computers abzurufen.  

    Die PowerShell-Anforderung:
    
    ```powershell
    PS C:\> $response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token -Method GET -Body @{resource="https://vault.azure.net"} -Headers @{Metadata="true"} 
    ```
    
    Extrahieren Sie als Nächstes die vollständige Antwort, die als JSON-formatierte Zeichenfolge (JavaScript Object Notation) im „$response“-Objekt gespeichert ist.  
    
    ```powershell
    PS C:\> $content = $response.Content | ConvertFrom-Json 
    ```
    
    Extrahieren Sie dann das Zugriffstoken aus der Antwort.  
    
    ```powershell
    PS C:\> $KeyVaultToken = $content.access_token 
    ```
    
    Verwenden Sie abschließend den PowerShell-Befehl „Invoke-WebRequest“, um das zuvor in der Key Vault erstellte Geheimnis abzurufen. Übergeben Sie dabei das Zugriffstoken im Autorisierungsheader.  Sie benötigen die URL Ihrer Key Vaults. Diese befindet sich im Abschnitt **Zusammenfassung** der Seite **Übersicht** der Key Vault.  
    
    ```powershell
    PS C:\> (Invoke-WebRequest -Uri https://<your-key-vault-URL>/secrets/<secret-name>?api-version=2016-10-01 -Method GET -Headers @{Authorization="Bearer $KeyVaultToken"}).content 
    ```
    
    Die Antwort sieht so aus: 
    
    ```powershell
    {"value":"p@ssw0rd!","id":"https://mytestkeyvault.vault.azure.net/secrets/MyTestSecret/7c2204c6093c4d859bc5b9eff8f29050","attributes":{"enabled":true,"created":1505088747,"updated":1505088747,"recoveryLevel":"Purgeable"}} 
    ```
    
Nachdem Sie das Geheimnis aus der Key Vault abgerufen haben, können Sie es für die Authentifizierung bei einem Dienst verwenden, für den ein Name und ein Kennwort angegeben werden müssen. 

## <a name="related-content"></a>Verwandte Inhalte

- Einen Überblick über MSI finden Sie unter [Übersicht über verwaltete Dienstidentitäten](../active-directory/msi-overview.md).

Verwenden Sie den folgenden Kommentarabschnitt, um uns Feedback zu senden und uns bei der Verbesserung unserer Inhalte zu unterstützen.

