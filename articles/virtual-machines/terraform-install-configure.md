---
title: Installieren und Konfigurieren von Terraform zum Bereitstellen von VMs und sonstiger Infrastruktur in Azure | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Terraform zum Erstellen von Azure-Ressourcen installieren und konfigurieren.
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
ms.translationtype: Human Translation
ms.sourcegitcommit: 4f68f90c3aea337d7b61b43e637bcfda3c98f3ea
ms.openlocfilehash: aa82dda778af927db0a204eb88bae445c086e309
ms.contentlocale: de-de
ms.lasthandoff: 06/20/2017

---

# <a name="install-and-configure-terraform-to-provision-vms-and-other-infrastructure-into-azure"></a>Installieren und Konfigurieren von Terraform zum Bereitstellen von VMs und sonstiger Infrastruktur in Azure 
In diesem Artikel werden die erforderlichen Schritte zum Installieren und Konfigurieren von Terraform zum Bereitstellen von Ressourcen (z.B. virtuellen Computern) in Azure erläutert. Sie erfahren, wie Sie Azure-Anmeldeinformationen erstellen und verwenden, um Cloudressourcen auf sichere Weise bereitzustellen.

HashiCorp Terraform bietet eine einfache Möglichkeit, Cloudinfrastruktur mit der benutzerdefinierten Vorlagensprache HCL zu definieren und bereitzustellen. Diese benutzerdefinierte Sprache ist [sowohl einfach zu schreiben als auch leicht verständlich](terraform-create-complete-vm.md). Darüber hinaus ermöglicht Ihnen Terraform über den Befehl „terraform plan“ das Visualisieren von Änderungen an der Infrastruktur, bevor Sie einen Commit der Änderungen durchführen. Führen Sie die untenstehenden Schritte aus, um in die Arbeit mit Terraform in Verbindung mit Azure einzusteigen.

## <a name="installing-terraform"></a>Installieren von Terraform
[Laden](https://www.terraform.io/downloads.html) Sie zum Installieren von Terraform das entsprechende Paket für Ihr Betriebssystem in ein separates Installationsverzeichnis herunter. Der Download enthält eine einzelne ausführbare Datei, für die Sie auch einen globalen PATH definieren müssen. Anweisungen zum Festlegen des PATH für Linux und Mac finden Sie auf [dieser Seite](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux), während [diese Seite](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows) Anweisungen zum Festlegen des Pfads für Windows enthält. Überprüfen Sie die Installation durch Ausführen des Befehls „terraform“. Als Ausgabe wird eine Liste der verfügbaren Terraform-Optionen angezeigt.

Anschließend müssen Sie Terraform den Zugriff auf Ihr Azure-Abonnement gewähren, um Infrastruktur-Bereitstellungsvorgänge auszuführen.

## <a name="setting-up-terraform-access-to-azure"></a>Einrichten des Terraform-Zugriffs auf Azure
Um Terraform das Bereitstellen von Ressourcen in Azure zu ermöglichen, müssen Sie zwei Entitäten in Azure Active Directory (AAD) erstellen: AAD-Anwendung und AAD-Dienstprinzipal. Sie verwenden die Bezeichner dieser Entitäten in den Terraform-Skripts. Der Dienstprinzipal ist eine lokale Instanz einer globalen AAD-Anwendung. Ein vorhandener Dienstprinzipal ermöglicht eine präzisere Steuerung des lokalen Zugriffs auf globale Ressourcen.

Es gibt mehrere Möglichkeiten, AAD-Anwendung und AAD-Dienstprinzipal zu erstellen. Am einfachsten und schnellsten ist es, Azure CLI 2.0 zu verwenden, die Sie [für Windows/Linux/Mac herunterladen und installieren können](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli). Sie können auch mithilfe von Powershell oder Azure CLI 1.0 die nötige Sicherheitsinfrastruktur erstellen. In den untenstehenden Anweisungen wird erläutert, wie Sie Terraform für Azure konfigurieren; dabei wird auf alle diese Ansätze eingegangen.

### <a name="windowslinuxmac-users-using-azure-cli-20"></a>Windows/Linux/Mac-Benutzer, die Azure CLI 2.0 verwenden
Melden Sie sich nach dem Herunterladen und Installieren der [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) für die Verwaltung Ihres Azure-Abonnements an. Geben Sie hierzu den folgenden Befehl aus:

```
az login
```

>[!NOTE]
>Wenn Sie die Clouds von Azure China, Azure Deutschland oder Azure Government verwenden, müssen Sie zuerst die Azure-CLI für die Arbeit mit der betreffenden Cloud konfigurieren. Führen Sie dazu Folgendes aus:

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

Dadurch werden Ihre Angaben für „client_id“, „client_secret“ (Kennwort), „sp_name“ und „tenant“ ausgegeben. Notieren Sie sich die Werte für **client_id** und **client_secret**.

Sie können Ihre Anmeldeinformationen (Dienstprinzipal) bestätigen, indem Sie eine neue Shell öffnen und die folgenden Befehle ausführen, in die Sie die zurückgegebenen Werte für **sp_name**, **client_secret** und **tenant** eingefügt haben:

```
az login --service-principal -u SP_NAME -p CLIENT_SECRET --tenant TENANT
az vm list-sizes --location westus
```

### <a name="windows-users-using-powershell"></a>Windows-Benutzer, die Powershell verwenden
Wenn Sie Ihre Terraform-Skripts auf einem Windows-Computer schreiben und ausführen und Powershell für Konfigurationsaufgaben verwenden möchten, müssen Sie den Computer mit den entsprechenden Powershell-Tools konfigurieren. Gehen Sie dazu wie folgt vor: (1) [Installieren Sie Azure PowerShell-Tools](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps) und (2) laden Sie das Skript [azure-setup.ps1](https://github.com/echuvyrov/terraform101/blob/master/azureSetup.ps1) aus der Powershell-Konsole herunter, und führen Sie es aus. Um das Skript „azure-setup.ps1“ auszuführen, laden Sie es herunter, führen Sie den Befehl „./azure-setup.ps1 setup“ an der Powershell-Konsole aus, und melden Sie sich mit Administratorrechten bei Ihrem Azure-Abonnement an. Geben Sie anschließend bei einer entsprechenden Aufforderung einen Anwendungsnamen an (beliebige Zeichenfolge, erforderlich), und geben Sie (optional) ein starkes Kennwort an. Wenn Sie kein Kennwort angeben, wird das starke Kennwort automatisch anhand der .Net-Sicherheitsbibliotheken erstellt.

### <a name="linuxmac-users-using-azure-cli-10"></a>Linux/Mac-Benutzer, die Azure CLI 1.0 verwenden
Um Terraform auf Linux-Computern oder Macs mit Azure CLI 1.0 verwenden zu können, müssen Sie sicherstellen, dass die richtigen Bibliotheken auf dem Computer installiert sind. Gehen Sie dazu wie folgt vor: (1) [Installieren Sie die Azure xPlat CLI-Tools](https://docs.microsoft.com/cli/azure/install-azure-cli), (2) Sie müssen den JSON-Prozessor [jq herunterladen und installieren](https://stedolan.github.io/jq/download/) und (3) das Bash-Skript [azure-setup.sh](https://github.com/mitchellh/packer/blob/master/contrib/azure-setup.sh) von der Konsole herunterladen und ausführen. Um das Skript „azure-setup.sh“ auszuführen, laden Sie es herunter, führen Sie den Befehl „./azure-setup setup“ an der Konsole aus, und melden Sie sich mit Administratorrechten bei Ihrem Azure-Abonnement an. Geben Sie anschließend bei einer entsprechenden Aufforderung einen Anwendungsnamen an (beliebige Zeichenfolge, erforderlich), und geben Sie bei einer entsprechenden Aufforderung (optional) ein starkes Kennwort an. Wenn Sie kein Kennwort angeben, wird das starke Kennwort automatisch anhand der .Net-Sicherheitsbibliotheken erstellt.

Alle obigen Skripts erstellen eine AAD-Anwendung und einen Dienstprinzipal, wobei dem Dienstprinzipal der Abonnementzugriff auf Mitwirkenden- oder Besitzerebene gewährt wird. Aufgrund der gewährten hohen Zugriffsebene sollten Sie stets die von diesen Skripts generierten Sicherheitsinformationen schützen. Notieren Sie sich alle vier Teile der Sicherheitsinformationen, die von den Skripts bereitgestellt werden: „client_id“, „client_secret“, „subscription_id“ und „tenant_id“. 

## <a name="setting-environment-variables"></a>Festlegen von Umgebungsvariablen
Wenn der AAD-Dienstprinzipal erstellt und konfiguriert wurde, müssen Sie Terraform die Mandanten-ID, die Abonnement-ID, die Client-ID und das Clientgeheimnis mitteilen, die verwendet werden sollen. Dazu können Sie diese Werte in die Terraform-Skripts einbetten (die Vorgehensweise wird im [nächsten Abschnitt](terraform-create-complete-vm.md) beschrieben). Sie können jedoch auch die folgenden Umgebungsvariablen festlegen (und damit das versehentliche Einchecken/Freigeben Ihrer Anmeldeinformationen vermeiden):

- ARM_SUBSCRIPTION_ID
- ARM_CLIENT_ID
- ARM_CLIENT_SECRET
- ARM_TENANT_ID

Unten finden Sie ein Beispiel-Shell-Skript, anhand dessen Sie diese Variablen festlegen können:

```
#!/bin/sh
echo "Setting environment variables for Terraform"
export ARM_SUBSCRIPTION_ID=your_subscription_id
export ARM_CLIENT_ID=your_client_id
export ARM_CLIENT_SECRET=your_client_secret
export ARM_TENANT_ID=your_tenant_id
```

Wenn Sie Terraform mit Azure Government, Azure Deutschland oder Azure China verwenden, müssen Sie zudem die Variable ENVIRONMENT entsprechend festlegen.

## <a name="next-steps"></a>Nächste Schritte
Sie haben nun Terraform installiert und Azure-Anmeldeinformationen konfiguriert, um mit dem Bereitstellen einer Infrastruktur in Ihrem Azure-Abonnement zu beginnen. Nun erfahren Sie, wie Sie eine [Infrastruktur mit Terraform erstellen](terraform-create-complete-vm.md).
