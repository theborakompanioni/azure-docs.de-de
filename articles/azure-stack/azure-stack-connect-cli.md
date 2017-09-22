---
title: Herstellen einer Verbindung mit Azure Stack per CLI | Microsoft-Dokumentation
description: "Es wird beschrieben, wie Sie die plattformübergreifende Befehlszeilenschnittstelle (Command-Line Interface, CLI) verwenden, um Ressourcen in Azure Stack zu verwalten und bereitzustellen."
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: f576079c-5384-4c23-b5a4-9ae165d1e3c3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/29/2017
ms.author: sngun
ms.translationtype: HT
ms.sourcegitcommit: 1c730c65194e169121e3ad1d1423963ee3ced8da
ms.openlocfilehash: ece02b5132dcaaf229847542c5b634c9b28855ae
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---
# <a name="install-and-configure-cli-for-the-azure-stack-users-environment"></a>Installieren und Konfigurieren der CLI für die Umgebung des Azure Stack-Benutzers

In diesem Dokument wird die Verwendung der Azure-Befehlszeilenschnittstelle (CLI) zum Verwalten von Azure Stack Development Kit-Ressourcen über Linux- und Mac-Clientplattformen schrittweise beschrieben. 

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie die CLI zum Verwalten von Azure Stack-Ressourcen verwenden können, sollten Sie sicherstellen, dass Sie die beiden folgenden Schritte ausführen:

* Beschaffen Sie das [Zertifizierungsstellen-Stammzertifikat für Azure Stack](azure-stack-cli-admin.md#export-the-azure-stack-ca-root-certificate) von Ihrem Azure Stack-Betreiber. 
* Beschaffen Sie den [VM-Aliasendpunkt](azure-stack-cli-admin.md#set-up-the-virtual-machine-aliases-endpoint) von Ihrem Azure Stack-Betreiber.
 
## <a name="install-cli"></a>Installieren der CLI

Als Nächstes sollten Sie sich an Ihrer Entwicklungsarbeitsstation anmelden und die CLI installieren. Für Azure Stack ist Version 2.0 der Azure CLI erforderlich. Sie können diese Version installieren, indem Sie die Schritte im Artikel [Install Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) (Installieren von Azure CLI 2.0) ausführen. Öffnen Sie ein Terminal oder ein Eingabeaufforderungsfenster, und führen Sie den folgenden Befehl aus, um zu überprüfen, ob die Installation erfolgreich war:

```azurecli
az --version
```

Daraufhin sollten die Version der Azure-CLI und die anderen abhängigen Bibliotheken angezeigt werden, die auf Ihrem Computer installiert sind.

## <a name="trust-the-azure-stack-ca-root-certificate"></a>Einstufen des Zertifizierungsstellen-Stammzertifikats für Azure Stack als vertrauenswürdig

Zum Einstufen des Zertifizierungsstellen-Stammzertifikats für Azure Stack als vertrauenswürdig sollten Sie es an das vorhandene Python-Zertifikat anfügen. Wenn Sie die CLI über einen Linux-Computer ausführen, der in der Azure Stack-Umgebung erstellt wird, ist das Zertifikat bereits auf dem virtuellen Computer verfügbar. Sie müssen also den folgenden Bash-Befehl ausführen, um das Zertifikat an das Python-Zertifikat anzufügen:

```bash
sudo cat /var/lib/waagent/Certificates.pem >> ~/lib/azure-cli/lib/python2.7/site-packages/certifi/cacert.pem
```

Wenn Sie die CLI über einen Computer außerhalb der Azure Stack-Umgebung ausführen, müssen Sie zuerst die [VPN-Konnektivität für Azure Stack](azure-stack-connect-azure-stack.md) einrichten. Kopieren Sie jetzt das PEM-Zertifikat, das Sie von Ihrem Azure Stack-Betreiber erhalten haben, auf Ihre Entwicklungsarbeitsstation, und führen Sie je nach Betriebssystem einen der folgenden Befehle aus:

### <a name="linux"></a>Linux

```bash
sudo cat PATH_TO_PEM_FILE >> ~/lib/azure-cli/lib/python2.7/site-packages/certifi/cacert.pem
```

### <a name="macos"></a>MacOS

```bash
sudo cat PATH_TO_PEM_FILE >> ~/lib/azure-cli/lib/python2.7/site-packages/certifi/cacert.pem
```

### <a name="windows"></a>Windows

```powershell
$pemFile = "<Fully qualified path to the PEM certificate Ex: C:\Users\user1\Downloads\root.pem>"

$root = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2
$root.Import($pemFile)

Write-Host "Extracting needed information from the cert file"
$md5Hash=(Get-FileHash -Path $pemFile -Algorithm MD5).Hash.ToLower()
$sha1Hash=(Get-FileHash -Path $pemFile -Algorithm SHA1).Hash.ToLower()
$sha256Hash=(Get-FileHash -Path $pemFile -Algorithm SHA256).Hash.ToLower()

$issuerEntry = [string]::Format("# Issuer: {0}", $root.Issuer)
$subjectEntry = [string]::Format("# Subject: {0}", $root.Subject)
$labelEntry = [string]::Format("# Label: {0}", $root.Subject.Split('=')[-1])
$serialEntry = [string]::Format("# Serial: {0}", $root.GetSerialNumberString().ToLower())
$md5Entry = [string]::Format("# MD5 Fingerprint: {0}", $md5Hash)
$sha1Entry  = [string]::Format("# SHA1 Finterprint: {0}", $sha1Hash)
$sha256Entry = [string]::Format("# SHA256 Fingerprint: {0}", $sha256Hash)
$certText = (Get-Content -Path root.pem -Raw).ToString().Replace("`r`n","`n")

$rootCertEntry = "`n" + $issuerEntry + "`n" + $subjectEntry + "`n" + $labelEntry + "`n" + `
$serialEntry + "`n" + $md5Entry + "`n" + $sha1Entry + "`n" + $sha256Entry + "`n" + $certText

Write-Host "Adding the certificate content to Python Cert store"
Add-Content "${env:ProgramFiles(x86)}\Microsoft SDKs\Azure\CLI2\Lib\site-packages\certifi\cacert.pem" $rootCertEntry

Write-Host "Python Cert store was updated for allowing the azure stack CA root certificate"
```

## <a name="connect-to-azure-stack"></a>Herstellen einer Verbindung mit Azure Stack

Führen Sie die folgenden Schritte aus, um eine Verbindung mit Azure Stack herzustellen:

1. Registrieren Sie die Azure Stack-Benutzerumgebung, indem Sie den Befehl „az cloud register“ ausführen. Rufen Sie den URI für das Dokument mit den VM-Imagealiasen ab, und führen Sie die Registrierung für die Cloud durch. Der Parameter `endpoint-vm-image-alias-doc` im Befehl `az cloud register` wird für diesen Zweck genutzt.

   ```azurecli
   az cloud register \ 
     -n AzureStackUser \ 
     --endpoint-resource-manager "https://management.local.azurestack.external" \ 
     --suffix-storage-endpoint "local.azurestack.external" \ 
     --suffix-keyvault-dns ".vault.local.azurestack.external" \ 
     --endpoint-active-directory-graph-resource-id "https://graph.windows.net/" \
     --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases>
   ```

2. Legen Sie die aktive Umgebung fest, und aktualisieren Sie Ihre Umgebungskonfiguration so, dass das spezifische API-Versionsprofil für Azure Stack verwendet wird:

   ```azurecli
   az cloud set \
     -n AzureStackUser

   az cloud update \
     --profile 2017-03-09-profile
   ```

3. Melden Sie sich an Ihrer Azure Stack-Umgebung an, indem Sie den Befehl **az login** verwenden. Sie können sich entweder als Benutzer oder als [Dienstprinzipal](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-application-objects) an der Azure Stack-Umgebung anmelden. 

   * Anmeldung als **Benutzer**: Sie können entweder den Benutzernamen und das Kennwort direkt im Befehl „az login“ eingeben oder die Authentifizierung über einen Browser durchführen. Sie müssen das letztgenannte Verfahren wählen, wenn für Ihr Konto die mehrstufige Authentifizierung (Multi-Factor Authentication) aktiviert ist.

   ```azurecli
   az login \
     -u <Active directory user account. For example: username@<aadtenant>.onmicrosoft.com> \
     --tenant <Azure Active Directory Tenant name. For example: myazurestack.onmicrosoft.com>
   ```

   > [!NOTE]
   > Wenn für Ihr Benutzerkonto die mehrstufige Authentifizierung aktiviert ist, können Sie den Befehl „az login“ verwenden, ohne den Parameter „-u“ anzugeben. Durch die Ausführung des Befehls erhalten Sie eine URL und einen Code für die Authentifizierung.
   
   * Anmeldung als **Dienstprinzipal**: [Erstellen Sie einen Dienstprinzipal über das Azure-Portal](azure-stack-create-service-principals.md) oder die CLI, und weisen Sie ihm eine Rolle zu, bevor Sie sich anmelden. Melden Sie sich anschließend mit dem folgenden Befehl an:

   ```azurecli
   az login \
     --tenant <Azure Active Directory Tenant name. For example: myazurestack.onmicrosoft.com> \
     --service-principal \
     -u <Application Id of the Service Principal> \
     -p <Key generated for the Service Principal>
   ```

## <a name="test-the-connectivity"></a>Testen der Konnektivität

Nachdem nun alles eingerichtet ist, können wir mit der CLI Ressourcen in Azure Stack erstellen. Sie können beispielsweise eine Ressourcengruppe für eine Anwendung erstellen und einen virtuellen Computer hinzufügen. Verwenden Sie den folgenden Befehl, um eine Ressourcengruppe mit dem Namen „MyResourceGroup“ zu erstellen:

```azurecli
az group create \
  -n MyResourceGroup -l local
```

Wenn die Ressourcengruppe erfolgreich erstellt wurde, werden mit dem vorherigen Befehl die folgenden Eigenschaften der neu erstellten Ressource ausgegeben:

![Ausgabe der Ressourcengruppenerstellung](media/azure-stack-connect-cli/image1.png)

Für die Verwendung von CLI 2.0 in Azure Stack bestehen einige bekannte Probleme. Informationen zu diesen Problemen finden Sie im Abschnitt zu den [bekannten Problemen in der Azure Stack-CLI](azure-stack-troubleshooting.md#cli). 

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen von Vorlagen mit der Azure CLI](azure-stack-deploy-template-command-line.md)

[Verbinden mit PowerShell](azure-stack-connect-powershell.md)

[Verwalten von Benutzerberechtigungen](azure-stack-manage-permissions.md)


