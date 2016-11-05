---
title: Einrichten des Zugriffs auf WinRM für virtuelle Computer in Azure Resource Manager | Microsoft Docs
description: Vorgehensweise zum Einrichten des WinRM-Zugriffs zur Verwendung mit einem virtuellen Azure Resource Manager-Computer
services: virtual-machines-windows
documentationcenter: ''
author: singhkays
manager: timlt
editor: ''
tags: azure-resource-manager

ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/16/2016
ms.author: singhkay

---
# Einrichten des Zugriffs auf WinRM für virtuelle Computer in Azure Resource Manager
## WinRM in Azure Service Management im Vergleich zu Azure Resource Manager
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

Klassisches Bereitstellungsmodell

* Eine Übersicht über Azure Resource Manager finden Sie in diesem [Artikel](../resource-group-overview.md).
* Unterschiede zwischen Azure Service Management und Azure Resource Manager können Sie diesem [Artikel](../resource-manager-deployment-model.md) entnehmen.

Der Hauptunterschied zwischen beiden Stapeln bei der Einrichtung der WinRM-Konfiguration ist die Art und Weise, in der das Zertifikat auf dem virtuellen Computer installiert wird. Im Azure Resource Manager-Stapel sind die Zertifikate als Ressourcen modelliert, die durch den Schlüsseltresor-Ressourcenanbieter verwaltet werden. Daher müssen Benutzer ihr eigenes Zertifikat bereitstellen und in einen Schlüsseltresor hochladen, bevor es in einem virtuellen Computer verwendet werden kann.

Sie müssen folgende Schritte ausführen, um einen virtuellen Computer mit WinRM-Verbindung einzurichten:

1. Erstellen eines Schlüsseltresors
2. Erstellen eines selbstsignierten Zertifikats
3. Hochladen eines selbstsignierten Zertifikats in den Schlüsseltresor
4. Abrufen der URL für das selbstsignierte Zertifikat im Schlüsseltresor
5. Verweisen auf die URL für selbstsignierte Zertifikate beim Erstellen eines virtuellen Computers

## Schritt 1: Erstellen eines Schlüsseltresors
Sie können den folgenden Befehl verwenden, um den Schlüsseltresor zu erstellen.

```
New-AzureRmKeyVault -VaultName "<vault-name>" -ResourceGroupName "<rg-name>" -Location "<vault-location>" -EnabledForDeployment -EnabledForTemplateDeployment
```

## Schritt 2: Erstellen eines selbstsignierten Zertifikats
Sie können mit diesem PowerShell-Skript ein selbstsigniertes Zertifikat erstellen.

```
$certificateName = "somename"

$thumbprint = (New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation Cert:\CurrentUser\My -KeySpec KeyExchange).Thumbprint

$cert = (Get-ChildItem -Path cert:\CurrentUser\My\$thumbprint)

$password = Read-Host -Prompt "Please enter the certificate password." -AsSecureString

Export-PfxCertificate -Cert $cert -FilePath ".\$certificateName.pfx" -Password $password
```

## Schritt 3: Hochladen eines selbstsignierten Zertifikats in den Schlüsseltresor
Vor dem Hochladen des Zertifikats in den in Schritt 1 erstellten Schlüsseltresor muss es in ein Format konvertiert werden, das für den Microsoft.Compute-Ressourcenanbieter verständlich ist. Dies können Sie mit dem unten stehenden PowerShell-Skript tun.

```
$fileName = "<Path to the .pfx file>"
$fileContentBytes = Get-Content $fileName -Encoding Byte
$fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)

$jsonObject = @"
{
  "data": "$filecontentencoded",
  "dataType" :"pfx",
  "password": "<password>"
}
"@

$jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
$jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

$secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText –Force
Set-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>" -SecretValue $secret
```

## Schritt 4: Abrufen der URL für das selbstsignierte Zertifikat im Schlüsseltresor
Der Microsoft.Compute-Ressourcenanbieter benötigt bei der Bereitstellung des virtuellen Computers eine URL für den geheimen Schlüssel im Schlüsseltresor. Dadurch kann der Microsoft.Compute-Ressourcenanbieter den geheimen Schlüssel herunterladen und das entsprechende Zertifikat auf dem virtuellen Computer erstellen.

> [!NOTE]
> Die URL des geheimen Schlüssels muss auch die Version enthalten. Eine Beispiel-URL sieht wie folgt aus: https://contosovault.vault.azure.net:443/secrets/contososecret/01h9db0df2cd4300a20ence585a6s7ve
> 
> 

#### Vorlagen
Sie erhalten den Link zur URL in der Vorlage mit folgendem Code:

    "certificateUrl": "[reference(resourceId(resourceGroup().name, 'Microsoft.KeyVault/vaults/secrets', '<vault-name>', '<secret-name>'), '2015-06-01').secretUriWithVersion]"

#### PowerShell
Sie erhalten diese URL durch den folgenden PowerShell-Befehl:

    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id

## Schritt 5: Verweisen auf die URL für selbstsignierte Zertifikate beim Erstellen eines virtuellen Computers
#### Azure Resource Manager-Vorlagen
Beim Erstellen eines virtuellen Computers mittels Vorlagen wird im Abschnitt mit dem geheimen Schlüssel und im Abschnitt „winRM“ wie folgt auf das Zertifikat verwiesen:

    "osProfile": {
          ...
          "secrets": [
            {
              "sourceVault": {
                "id": "<resource id of the Key Vault containing the secret>"
              },
              "vaultCertificates": [
                {
                  "certificateUrl": "<URL for the certificate you got in Step 4>",
                  "certificateStore": "<Name of the certificate store on the VM>"
                }
              ]
            }
          ],
          "windowsConfiguration": {
            ...
            "winRM": {
              "listeners": [
                {
                  "protocol": "http"
                },
                {
                  "protocol": "https",
                  "certificateUrl": "<URL for the certificate you got in Step 4>"
                }
              ]
            },
            ...
          }
        },

Eine Vorlage zum oben genannten Beispiel finden Sie unter [201-vm-winrm-keyvault-windows](https://azure.microsoft.com/documentation/templates/201-vm-winrm-keyvault-windows).

Den Quellcode für diese Vorlage finden Sie auf [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-winrm-keyvault-windows).

#### PowerShell
    $vm = New-AzureRmVMConfig -VMName "<VM name>" -VMSize "<VM Size>"
    $credential = Get-Credential
    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id
    $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName "<Computer Name>" -Credential $credential -WinRMHttp -WinRMHttps -WinRMCertificateUrl $secretURL
    $sourceVaultId = (Get-AzureRmKeyVault -ResourceGroupName "<Resource Group name>" -VaultName "<Vault Name>").ResourceId
    $CertificateStore = "My"
    $vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore $CertificateStore -CertificateUrl $secretURL

## Schritt 6: Herstellen einer Verbindung mit dem virtuellen Computer
Bevor Sie eine Verbindung mit dem virtuellen Computer herstellen können, müssen Sie sicherstellen, dass Ihr Computer für die WinRM-Remoteverwaltung konfiguriert ist. Starten Sie PowerShell als Administrator, und führen Sie den folgenden Befehl aus, um zu überprüfen, ob das Setup abgeschlossen ist.

    Enable-PSRemoting -Force

> [!NOTE]
> Wenn die oben genannte Methode nicht funktioniert, müssen Sie sicherstellen, dass der WinRM-Dienst ausgeführt wird. Verwenden Sie dazu `Get-Service WinRM`.
> 
> 

Wenn das Setup abgeschlossen ist, können Sie mit dem folgenden Befehl eine Verbindung mit dem virtuellen Computer erstellen:

    Enter-PSSession -ConnectionUri https://<public-ip-dns-of-the-vm>:5986 -Credential $cred -SessionOption (New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck) -Authentication Negotiate

<!---HONumber=AcomDC_0824_2016-->