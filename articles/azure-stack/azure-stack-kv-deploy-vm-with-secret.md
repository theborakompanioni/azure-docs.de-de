---
title: Bereitstellen eines virtuellen Computers mit einem sicher gespeicherten Kennwort in Azure Stack | Microsoft-Dokumentation
description: "Hier erfahren Sie, wie Sie einen virtuellen Computer mithilfe eines Kennworts bereitstellen, das in einem Azure Stack-Schlüsseltresor gespeichert ist."
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: 23322a49-fb7e-4dc2-8d0e-43de8cd41f80
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/08/2017
ms.author: sngun
ms.translationtype: HT
ms.sourcegitcommit: 8351217a29af20a10c64feba8ccd015702ff1b4e
ms.openlocfilehash: 7398cea814a4fd1111bdadbbbd555698076a14f0
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---
# <a name="create-a-virtual-machine-by-retrieving-the-password-stored-in-a-key-vault"></a>Erstellen eines virtuellen Computers durch Abrufen des im Schlüsseltresor gespeicherten Kennworts

Wenn Sie während der Bereitstellung einen sicheren Wert (etwa ein Kennwort) übergeben müssen, können Sie diesen Wert als Geheimnis in einem Azure Stack-Schlüsseltresor speichern und in den Azure Resource Manager-Vorlagen darauf verweisen. Sie müssen nicht bei jeder Bereitstellung der Ressourcen das Geheimnis manuell eingeben, und Sie können angeben, welche Benutzer oder Dienstprinzipale auf das Geheimnis zugreifen können. 

In diesem Artikel werden die Schritte zum Bereitstellen eines virtuellen Windows-Computers in Azure Stack durch Abrufen des Kennworts erläutert, das in einem Schlüsseltresor gespeichert ist. Aus diesem Grund wird das Kennwort nie als Nur-Text in die Vorlagenparameterdatei eingefügt. Führen Sie die Schritte entweder über das Azure Stack Development Kit oder über einen externen Client aus, wenn eine Verbindung über VPN besteht.

## <a name="prerequisites"></a>Voraussetzungen

* Azure Stack-Cloudadministratoren müssen [ein Angebot erstellt](azure-stack-create-offer.md) haben, das den Azure Key Vault-Dienst umfasst.  
* Benutzer müssen [ein Angebot abonnieren](azure-stack-subscribe-plan-provision-vm.md), das den Key Vault-Dienst umfasst.  
* [Installieren Sie PowerShell für Azure Stack.](azure-stack-powershell-install.md)  
* [Konfigurieren Sie die PowerShell-Umgebung des Azure Stack-Benutzers.](azure-stack-powershell-configure-user.md)

Die folgenden Schritte beschreiben das Verfahren zum Erstellen eines virtuellen Computers durch Abrufen des Kennworts, das in einem Schlüsseltresor gespeichert ist:

1. Erstellen eines Geheimnisses im Schlüsseltresor
2. Aktualisieren der Datei „azuredeploy.parameters.json“
3. Stellen Sie die Vorlage bereit.

## <a name="create-a-key-vault-secret"></a>Erstellen eines Geheimnisses im Schlüsseltresor

Das folgende Skript erstellt einen Schlüsseltresor und speichert ein Kennwort als Geheimnis im Schlüsseltresor. Verwenden Sie beim Erstellen des Schlüsseltresors den `-EnabledForDeployment`-Parameter. Dieser Parameter stellt sicher, dass von Azure Resource Manager-Vorlagen auf den Schlüsseltresor verwiesen werden kann.

```powershell

$vaultName = "contosovault"
$resourceGroup = "contosovaultrg"
$location = "local"
$secretName = "MySecret"

New-AzureRmResourceGroup `
  -Name $resourceGroup `
  -Location $location

New-AzureRmKeyVault `
  -VaultName $vaultName `
  -ResourceGroupName $resourceGroup `
  -Location $location
  -EnabledForTemplateDeployment

$secretValue = ConvertTo-SecureString -String '<Password for your virtual machine>' -AsPlainText -Force

Set-AzureKeyVaultSecret `
  -VaultName $vaultName `
  -Name $secretName `
  -SecretValue $secretValue

```

Wenn Sie das vorherige Skript ausführen, enthält die Ausgabe den URI des Geheimnisses. Notieren Sie sich diesen URI. Sie müssen in der [Vorlage zum Bereitstellung eines virtuellen Windows-Computers mit Kennwort in einem Schlüsseltresor](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-vm-windows-create-passwordfromkv) darauf verweisen. Laden Sie den Ordner [101-vm-secure-password](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-vm-windows-create-passwordfromkv) auf den Entwicklungscomputer herunter. Dieser Ordner enthält die Dateien `azuredeploy.json` und `azuredeploy.parameters.json`, die Sie in den nächsten Schritten benötigen.

Ändern Sie die Datei `azuredeploy.parameters.json` gemäß den Werten Ihrer Umgebung. Die Parameter, die hier von besonderem Interesse sind, sind der Tresorname, die Tresorressourcengruppe und der URI des Geheimnisses (vom vorherigen Skript erstellt). Die folgende Datei ist ein Beispiel für eine Parameterdatei:

## <a name="update-the-azuredeployparametersjson-file"></a>Aktualisieren der Datei „azuredeploy.parameters.json“

Aktualisieren Sie die Datei „azuredeploy.parameters.json“ gemäß Ihrer Umgebung mit den Werten für den Schlüsseltresor-URI, den Geheimnisnamen und den Administratorbenutzernamen des virtuellen Computers. Die folgende JSON-Datei zeigt ein Beispiel der Vorlagenparameterdatei: 

```json
{
    "$schema":  "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion":  "1.0.0.0",
    "parameters":  {
       "adminUsername":  {
         "value":  "demouser"
          },
         "adminPassword":  {
           "reference":  {
              "keyVault":  {
                "id":  "/subscriptions/xxxxxx/resourceGroups/RgKvPwd/providers/Microsoft.KeyVault/vaults/KvPwd"
                },
              "secretName":  "MySecret"
           }
         },
       "dnsLabelPrefix":  {
          "value":  "mydns123456"
        },
        "windowsOSVersion":  {
          "value":  "2016-Datacenter"
        }
    }
}

```

## <a name="template-deployment"></a>Bereitstellung von Vorlagen

Stellen Sie nun die Vorlage mithilfe des folgenden PowerShell-Skripts bereit:

```powershell
New-AzureRmResourceGroupDeployment `
  -Name KVPwdDeployment `
  -ResourceGroupName $resourceGroup `
  -TemplateFile "<Fully qualified path to the azuredeploy.json file>" `
  -TemplateParameterFile "<Fully qualified path to the azuredeploy.parameters.json file>"
```
Wenn die Vorlage erfolgreich bereitgestellt wurde, erhalten Sie die folgende Ausgabe:

![Ausgabe der Bereitstellung](media\azure-stack-kv-deploy-vm-with-secret/deployment-output.png)


## <a name="next-steps"></a>Nächste Schritte
[Bereitstellen einer Beispiel-App mit Key Vault](azure-stack-kv-sample-app.md)

[Bereitstellen eines virtuellen Computers mit einem Key Vault-Zertifikat](azure-stack-kv-push-secret-into-vm.md)


