---
title: Installieren und Konfigurieren von Terraform zum Bereitstellen von VMs und sonstiger Infrastruktur in Azure | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Terraform zum Erstellen von Azure-Ressourcen installieren und konfigurieren
services: virtual-machines-linux
documentationcenter: virtual-machines
author: echuvyrov
manager: jtalkar
editor: na
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/14/2017
ms.author: echuvyrov
ms.translationtype: HT
ms.sourcegitcommit: 8727e15448a74b68277c5bdd82c573e817254f80
ms.openlocfilehash: da567097be38ac649c6bf1de1508de24d21cb877
ms.contentlocale: de-de
ms.lasthandoff: 08/04/2017

---

# <a name="install-and-configure-terraform-to-provision-vms-and-other-infrastructure-into-azure"></a>Installieren und Konfigurieren von Terraform zum Bereitstellen von VMs und sonstiger Infrastruktur in Azure 
In diesem Artikel werden die erforderlichen Schritte zum Installieren und Konfigurieren von Terraform zum Bereitstellen von Ressourcen (z.B. virtuellen Computern) in Azure erläutert. Sie erfahren, wie Sie Azure-Anmeldeinformationen erstellen und verwenden, um Cloudressourcen auf sichere Weise bereitzustellen.

HashiCorp Terraform bietet eine einfache Möglichkeit, Cloudinfrastruktur mit der benutzerdefinierten Vorlagensprache HashiCorp (HCL) zu definieren und bereitzustellen. Diese benutzerdefinierte Sprache ist [einfach zu schreiben und leicht verständlich](terraform-create-complete-vm.md). Darüber hinaus ermöglicht Ihnen Terraform über den Befehl `terraform plan` das Visualisieren von Änderungen an der Infrastruktur, bevor Sie einen Commit der Änderungen durchführen. Führen Sie die folgenden Schritte aus, um in die Verwendung von Terraform in Azure einzusteigen.

## <a name="install-terraform"></a>Installieren von Terraform
[Laden](https://www.terraform.io/downloads.html) Sie zum Installieren von Terraform das entsprechende Paket für Ihr Betriebssystem in ein separates Installationsverzeichnis herunter. Der Download enthält eine einzelne ausführbare Datei, für die Sie auch einen globalen Pfad definieren müssen. [Auf dieser Webseite](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux) finden Sie Anweisungen für das Einrichten des Pfads unter Linux und Macintosh. [Auf dieser Webseite](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows) finden Sie Anweisungen für das Einrichten des Pfads unter Windows. Um Ihre Installation zu überprüfen, führen Sie den Befehl `terraform` aus. Normalerweise wird dann die Liste der verfügbaren Terraform-Optionen angezeigt.

Anschließend müssen Sie Terraform den Zugriff auf Ihr Azure-Abonnement gewähren, um Infrastruktur-Bereitstellungsvorgänge auszuführen.

## <a name="set-up-terraform-access-to-azure"></a>Einrichten des Terraform-Zugriffs auf Azure
Um Terraform das Bereitstellen von Ressourcen in Azure zu ermöglichen, müssen Sie zwei Entitäten in Azure Active Directory (Azure AD) erstellen: eine Azure AD-Anwendung und einen Azure AD-Dienstprinzipal. Sie verwenden die Bezeichner dieser Entitäten in den Terraform-Skripts. Der Dienstprinzipal ist eine lokale Instanz einer globalen Azure AD-Anwendung. Ein Dienstprinzipal ermöglicht eine präzisere Steuerung des lokalen Zugriffs auf globale Ressourcen.

Es gibt mehrere Möglichkeiten, eine Azure AD-Anwendung und einen Azure AD-Dienstprinzipal zu erstellen. Am einfachsten und schnellsten ist es, die Azure CLI 2.0 zu verwenden, die Sie [für Windows, Linux oder Mac herunterladen und installieren können](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli). Sie können auch mithilfe von PowerShell oder der Azure CLI 1.0 die nötige Sicherheitsinfrastruktur erstellen. In den folgenden Anweisungen wird erläutert, wie Sie Terraform für Azure konfigurieren. Dabei wird auf alle diese Ansätze eingegangen.

### <a name="use-azure-cli-20-for-windows-linux-or-mac-users"></a>Verwenden der Azure CLI 2.0 (für Benutzer von Windows, Linux oder Mac) 
Melden Sie sich nach dem Herunterladen und Installieren der [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) für die Verwaltung Ihres Azure-Abonnements an. Geben Sie hierzu den folgenden Befehl aus:

```
az login
```

>[!NOTE]
>Wenn Sie die Clouds von Azure China, Azure Deutschland oder Azure Government verwenden, müssen Sie zuerst die Azure-CLI für die Arbeit mit der betreffenden Cloud konfigurieren. Verwenden Sie dazu den folgenden Code:

```
az cloud set --name AzureChinaCloud|AzureGermanCloud|AzureUSGovernment
```

Wenn Sie über mehrere Azure-Abonnements verfügen, werden deren Details vom Befehl `az login` zurückgegeben. Legen Sie die `SUBSCRIPTION_ID`-Umgebungsvariable so fest, dass sie den Wert des zurückgegebenen Feldes `id` für das gewünschte Abonnement enthält. 

Legen Sie das Abonnement fest, das Sie für diese Sitzung verwenden möchten.

```
az account set --subscription="${SUBSCRIPTION_ID}"
```

Fragen Sie das Konto ab, um die Werte für Abonnement- und Mandanten-ID abzurufen.

```
az account show --query "{subscriptionId:id, tenantId:tenantId}"
```

Erstellen Sie anschließend separate Anmeldeinformationen für Terraform.

```
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/${SUBSCRIPTION_ID}"
```

Es werden „appId“, „password“, „sp_name“ sowie „tenenat“ zurückgegeben. Notieren Sie sich appId und Kennwort.

Überprüfen Sie in einer neuen Shell Ihre Anmeldeinformationen (Dienstprinzipal), öffnen Sie eine neue Shell, und führen Sie die folgenden Befehle aus. Ersetzen Sie die zurückgegebenen Werte für „sp_name“, „password“ und „tenant“:

```
az login --service-principal -u SP_NAME -p PASSWORD --tenant TENANT
az vm list-sizes --location westus
```

### <a name="use-powershell-for-windows-users"></a>Verwenden der PowerShell (für Windows-Benutzer) 
Um auf einem Windows-Computer Terraform-Skripts zu schreiben und auszuführen und Powershell für Konfigurationsaufgaben zu verwenden, konfigurieren Sie den Computer mit den entsprechenden Powershell-Tools. 

1. Installieren Sie die PowerShell-Tools mithilfe der Schritte in [Installieren und Konfigurieren von Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps). 

2. Laden Sie das Skript [azure-setup.ps1](https://github.com/echuvyrov/terraform101/blob/master/azureSetup.ps1) herunter, und führen Sie es mit der PowerShell-Konsole aus.

3. Um das Skript „azure-setup.ps1“ auszuführen, laden Sie es herunter, und führen Sie mit der PowerShell-Konsole den Befehl `./azure-setup.ps1 setup` aus. Melden Sie sich dann mit Administratorrechten bei Ihrem Azure-Abonnement an.

4. Geben Sie bei einer entsprechenden Aufforderung einen Anwendungsnamen an (beliebige Zeichenfolge, erforderlich). Sie haben die Option, ein sicheres Kennwort bereitzustellen, sobald Sie dazu aufgefordert werden. Wenn Sie kein Kennwort angeben, wird mit den .NET-Sicherheitsbibliotheken automatisch ein sicheres Kennwort erstellt.

### <a name="use-azure-cli-10-for-linux-or-mac-users"></a>Verwenden der Azure-CLI 1.0 (für Linux- und Mac-Benutzer)
Um Terraform auf Linux-Computern oder Macs mit der Azure CLI 1.0 verwenden zu können, müssen Sie die richtigen Bibliotheken auf dem Computer installieren.  

1. Mit den Schritten in [Installieren der Azure-CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) installieren Sie die Azure xPlat CLI-Tools. 

2. Laden Sie gemäß den Anweisungen in [Herunterladen](https://stedolan.github.io/jq/download/) einen JSON-Prozessor herunter, und installieren Sie ihn.

3. Laden Sie das Bash-Skript [azure-setup.sh](https://github.com/mitchellh/packer/blob/master/contrib/azure-setup.sh) von der Konsole herunter, und führen Sie es aus.

4. Um das azure-setup.sh-Skript auszuführen, laden Sie es herunter, und führen Sie auf der Konsole den Befehl `./azure-setup setup` aus. Melden Sie sich dann mit Administratorrechten bei Ihrem Azure-Abonnement an.
 
5. Geben Sie bei einer entsprechenden Aufforderung einen Anwendungsnamen an (beliebige Zeichenfolge, erforderlich). Sie haben die Option, ein sicheres Kennwort bereitzustellen, sobald Sie dazu aufgefordert werden. Wenn Sie kein Kennwort angeben, wird mit den .NET-Sicherheitsbibliotheken automatisch ein sicheres Kennwort erstellt.

Mit allen vorherigen Skripts wird eine Azure AD-Anwendung und ein Dienstprinzipal erstellt. Der Dienstprinzipal kann auf das Abonnement als Mitwirkender oder als Besitzer zugreifen. Aufgrund der gewährten hohen Zugriffsebene sollten Sie stets die von diesen Skripts generierten Sicherheitsinformationen schützen. Notieren Sie sich alle vier Teile der Sicherheitsinformationen, die von den Skripts bereitgestellt werden: „appId“, „password“, „subscription_id“ und „tenant_id“.

## <a name="set-environment-variables"></a>Festlegen von Umgebungsvariablen
Nachdem der Azure AD-Dienstprinzipal erstellt und konfiguriert wurde, müssen Sie Terraform die Mandanten-ID, die Abonnement-ID, die Client-ID und das Clientgeheimnis mitteilen, die verwendet werden sollen. Dazu können Sie diese Werte in die Terraform-Skripts einbetten (die Vorgehensweise wird in [Erstellen einer grundlegenden Infrastruktur in Azure mithilfe von Terraform](terraform-create-complete-vm.md) beschrieben). Sie können auch die folgenden Umgebungsvariablen festlegen (und damit das versehentliche Einchecken oder Freigeben Ihrer Anmeldeinformationen vermeiden):

- ARM_SUBSCRIPTION_ID
- ARM_CLIENT_ID
- ARM_CLIENT_SECRET
- ARM_TENANT_ID

Dieses Beispiel-Shell-Skript können Sie dazu verwenden, diese Variablen festzulegen:

```
#!/bin/sh
echo "Setting environment variables for Terraform"
export ARM_SUBSCRIPTION_ID=your_subscription_id
export ARM_CLIENT_ID=your_appId
export ARM_CLIENT_SECRET=your_password
export ARM_TENANT_ID=your_tenant_id
```

Wenn Sie Terraform mit Azure China, Azure Government oder Azure Deutschland verwenden, müssen Sie zudem die Umgebungsvariable ENVIRONMENT entsprechend festlegen.

## <a name="next-steps"></a>Nächste Schritte
Sie haben nun Terraform installiert und Azure-Anmeldeinformationen konfiguriert und können mit dem Bereitstellen einer Infrastruktur in Ihrem Azure-Abonnement beginnen. Nun erfahren Sie, wie Sie eine [Infrastruktur mit Terraform erstellen](terraform-create-complete-vm.md).

