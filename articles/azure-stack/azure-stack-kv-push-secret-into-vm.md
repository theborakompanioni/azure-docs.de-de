---
title: Bereitstellen eines virtuellen Computers mit einem sicher gespeicherten Zertifikat in Azure Stack | Microsoft-Dokumentation
description: "Hier erfahren Sie, wie Sie einen virtuellen Computer bereitstellen und mithilfe eines Schlüsseltresors in Azure Stack ein Zertifikat per Push auf ihn übertragen."
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: 46590eb1-1746-4ecf-a9e5-41609fde8e89
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/03/2017
ms.author: sngun
ms.translationtype: HT
ms.sourcegitcommit: 1c730c65194e169121e3ad1d1423963ee3ced8da
ms.openlocfilehash: 4b4bbf6aa612fe3eca5b860cbddaff8500e21f15
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---
# <a name="create-a-virtual-machine-and-include-certificate-retrieved-from-a-key-vault"></a>Erstellen eines virtuellen Computers und Aufnehmen eines aus einem Schlüsseltresor abgerufenen Zertifikats

Dieser Artikel unterstützt Sie beim Erstellen eines virtuellen Computers in Azure Stack und beim Übertragen von Zertifikaten per Push auf diesen Computer. 

## <a name="prerequisites"></a>Voraussetzungen

* Azure Stack-Betreiber müssen [ein Angebot erstellt](azure-stack-create-offer.md) haben, das den Azure Key Vault-Dienst umfasst.  
* Benutzer müssen [ein Angebot abonnieren](azure-stack-subscribe-plan-provision-vm.md), das den Key Vault-Dienst umfasst.  
* [Installieren Sie PowerShell für Azure Stack.](azure-stack-powershell-install.md)  
* [Konfigurieren Sie die PowerShell-Umgebung des Azure Stack-Benutzers.](azure-stack-powershell-configure-user.md)

Ein Schlüsseltresor in Azure Stack wird zum Speichern von Zertifikaten verwendet. Zertifikate sind in vielen verschiedenen Szenarios nützlich. Stellen Sie sich beispielsweise ein Szenario mit einem virtuellen Computer in Azure Stack vor, auf dem eine Anwendung ausgeführt wird, die ein Zertifikat erfordert. Dieses Zertifikat kann zur Verschlüsselung, zur Authentifizierung bei Active Directory oder für SSL auf einer Website verwendet werden. Die Speicherung des Zertifikats in einem Schlüsseltresor trägt dazu bei, dass es sicher ist.

In diesem Artikel werden die Schritte zum Übertragen eines Zertifikats per Push auf einen virtuellen Windows-Computer in Azure Stack erläutert. Führen Sie die Schritte entweder über das Azure Stack Development Kit oder über einen Windows-basierten externen Client aus, wenn eine Verbindung über VPN besteht.

Die folgenden Schritte beschreiben den Prozess, der zum Übertragen eines Zertifikats per Push auf den virtuellen Computer erforderlich ist:

1. Erstellen eines Geheimnisses im Schlüsseltresor
2. Aktualisieren der Datei „azuredeploy.parameters.json“
3. Bereitstellen der Vorlage

## <a name="create-a-key-vault-secret"></a>Erstellen eines Geheimnisses im Schlüsseltresor

Das folgende Skript erstellt ein Zertifikat im PFX-Format sowie einen Schlüsseltresor und speichert das Zertifikat als Geheimnis im Schlüsseltresor. Beim Erstellen des Schlüsseltresors muss der `-EnabledForDeployment`-Parameter verwendet werden. Dieser Parameter stellt sicher, dass von Azure Resource Manager-Vorlagen auf den Schlüsseltresor verwiesen werden kann.

```powershell

# Create a certificate in the .pfx format
New-SelfSignedCertificate `
  -certstorelocation cert:\LocalMachine\My `
  -dnsname contoso.microsoft.com

$pwd = ConvertTo-SecureString `
  -String "<Password used to export the certificate>" `
  -Force `
  -AsPlainText

Export-PfxCertificate `
  -cert "cert:\localMachine\my\<Certificate Thumbprint that was created in the previous step>" `
  -FilePath "<Fully qualified path where the exported certificate can be stored>" `
  -Password $pwd

# Create a key vault and upload the certificate into the key vault as a secret
$vaultName = "contosovault"
$resourceGroup = "contosovaultrg"
$location = "local"
$secretName = "servicecert"
$fileName = "<Fully qualified path where the exported certificate can be stored>"
$certPassword = "<Password used to export the certificate>"

$fileContentBytes = get-content $fileName `
  -Encoding Byte

$fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
$jsonObject = @"
{
"data": "$filecontentencoded",
"dataType" :"pfx",
"password": "$certPassword"
}
"@
$jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
$jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

New-AzureRmResourceGroup `
  -Name $resourceGroup `
  -Location $location

New-AzureRmKeyVault `
  -VaultName $vaultName `
  -ResourceGroupName $resourceGroup `
  -Location $location `
  -sku standard `
  -EnabledForDeployment

$secret = ConvertTo-SecureString `
  -String $jsonEncoded `
  -AsPlainText -Force

Set-AzureKeyVaultSecret `
  -VaultName $vaultName `
  -Name $secretName `
   -SecretValue $secret

```

Wenn Sie das vorherige Skript ausführen, enthält die Ausgabe den URI des Geheimnisses. Notieren Sie sich diesen URI. Sie müssen in der [Vorlage zum Übertragen des Zertifikats per Push an Windows Resource Manager](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/201-vm-windows-pushcertificate) auf ihn verweisen. Laden Sie den Ordner für die [Vorlage „vm-push-certificate-windows“](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/201-vm-windows-pushcertificate) auf Ihren Entwicklungscomputer herunter. Dieser Ordner enthält die Dateien `azuredeploy.json` und `azuredeploy.parameters.json`, die Sie in den nächsten Schritten benötigen.

Ändern Sie die Datei `azuredeploy.parameters.json` gemäß den Werten Ihrer Umgebung. Die Parameter, die hier von besonderem Interesse sind, sind der Tresorname, die Tresorressourcengruppe und der URI des Geheimnisses (vom vorherigen Skript erstellt). Die folgende Datei ist ein Beispiel für eine Parameterdatei:

## <a name="update-the-azuredeployparametersjson-file"></a>Aktualisieren der Datei „azuredeploy.parameters.json“

Aktualisieren Sie die Datei „azuredeploy.parameters.json“ gemäß Ihrer Umgebung mit dem Tresornamen, dem URI des Geheimnisses, dem Namen des virtuellen Computers und anderen Werten. Die folgende JSON-Datei zeigt ein Beispiel der Vorlagenparameterdatei: 

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "newStorageAccountName": {
      "value": "kvstorage01"
    },
    "vmName": {
      "value": "VM1"
    },
    "vmSize": {
      "value": "Standard_D1_v2"
    },
    "adminUserName": {
      "value": "demouser"
    },
    "adminPassword": {
      "value": "demouser@123"
    },
    "vaultName": {
      "value": "contosovault"
    },
    "vaultResourceGroup": {
      "value": "contosovaultrg"
    },
    "secretUrlWithVersion": {
      "value": "https://testkv001.vault.local.azurestack.external/secrets/testcert002/82afeeb84f4442329ce06593502e7840"
    }
  }
}
```

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Stellen Sie nun die Vorlage mithilfe des folgenden PowerShell-Skripts bereit:

```powershell
# Deploy a Resource Manager template to create a VM and push the secret onto it
New-AzureRmResourceGroupDeployment `
  -Name KVDeployment `
  -ResourceGroupName $resourceGroup `
  -TemplateFile "<Fully qualified path to the azuredeploy.json file>" `
  -TemplateParameterFile "<Fully qualified path to the azuredeploy.parameters.json file>"
```

Wenn die Vorlage erfolgreich bereitgestellt wurde, erhalten Sie die folgende Ausgabe:

![Ausgabe der Bereitstellung](media\azure-stack-kv-push-secret-into-vm/deployment-output.png)

Wenn dieser virtuelle Computer bereitgestellt wird, überträgt Azure Stack das Zertifikat per Push auf den virtuellen Computer. Unter Windows wird das Zertifikat zum Zertifikatspeicherort „LocalMachine“ mit dem vom Benutzer angegebenen Zertifikatspeicher hinzugefügt. Unter Linux wird das Zertifikat im Verzeichnis „/var/lib/waagent“ abgelegt. Dabei wird für die X509-Zertifikatdatei der Dateiname „&lt;UppercaseThumbprint&gt;.crt“ und für den privaten Schlüssel der Dateiname „&lt;UppercaseThumbprint&gt;.prv“ verwendet.

## <a name="retire-certificates"></a>Zurückziehen von Zertifikaten

Im vorherigen Abschnitt wurde gezeigt, wird Sie ein neues Zertifikat per Push auf einen virtuellen Computer übertragen. Das alte Zertifikat befindet sich noch auf dem virtuellen Computer und kann nicht entfernt werden. Sie können die ältere Version des Geheimnisses jedoch mit dem `Set-AzureKeyVaultSecretAttribute`-Cmdlet deaktivieren. Nachfolgend sehen Sie ein Beispiel für die Verwendung dieses Cmdlets. Ersetzen Sie die Werte für den Tresornamen, den Geheimnisnamen und die Version gemäß Ihrer Umgebung:

```powershell
Set-AzureKeyVaultSecretAttribute -VaultName contosovault -Name servicecert -Version e3391a126b65414f93f6f9806743a1f7 -Enable 0
```

## <a name="next-steps"></a>Nächste Schritte

* [Bereitstellen eines virtuellen Computers mit einem Key Vault-Kennwort](azure-stack-kv-deploy-vm-with-secret.md)
* [Gewähren des Zugriffs einer Anwendung auf Key Vault](azure-stack-kv-sample-app.md)



