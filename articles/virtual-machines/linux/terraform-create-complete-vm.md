---
title: Erstellen einer grundlegenden Infrastruktur in Azure mithilfe von Terraform | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure-Ressourcen mithilfe von Terraform erstellen.
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
ms.sourcegitcommit: 5b6c261c3439e33f4d16750e73618c72db4bcd7d
ms.openlocfilehash: aa0926de32dd85f4460bbfa84b7a6007e2199d51
ms.contentlocale: de-de
ms.lasthandoff: 08/28/2017

---

# <a name="create-basic-infrastructure-in-azure-by-using-terraform"></a>Erstellen einer grundlegenden Infrastruktur in Azure mithilfe von Terraform
In diesem Artikel werden die notwendigen Schritte zur Bereitstellung eines virtuellen Computers zusammen mit der zugrunde liegenden Infrastruktur in Azure beschrieben. Sie erfahren außerdem, wie Sie Terraform-Skripts schreiben und die Änderungen visualisieren, bevor Sie sie in der Cloudinfrastruktur vornehmen. Außerdem lernen Sie, wie Sie mithilfe von Terraform eine Infrastruktur in Azure erstellen.

Erstellen Sie zu Beginn im Text-Editor Ihrer Wahl (Visual Studio Code, Sublime, Vim usw.) eine Datei mit dem Namen \terraform_azure101.tf. Der genaue Namen der Datei ist nicht wichtig, da Terraform den Ordnernamen als Parameter akzeptiert. Es werden also alle Skripts im Ordner ausgeführt. Fügen Sie den folgenden Code in die neue Datei ein:

~~~~
# Configure the Microsoft Azure Provider
# NOTE: if you defined these values as environment variables, you do not have to include this block
provider "azurerm" {
  subscription_id = "your_subscription_id_from_script_execution"
  client_id       = "your_appId_from_script_execution"
  client_secret   = "your_password_from_script_execution"
  tenant_id       = "your_tenant_id_from_script_execution"
}

# create a resource group if it doesn't exist
resource "azurerm_resource_group" "helloterraform" {
    name = "terraformtest"
    location = "West US"
}
~~~~
Im Abschnitt `provider` des Skripts weisen Sie Terraform an, im Skript einen Azure-Anbieter für die Bereitstellung von Ressourcen zu verwenden. Werte für subscription_id, appId, password, and tenant_id finden Sie im [Handbuch zum Installieren und Konfigurieren von Terraform](terraform-install-configure.md). Wenn Sie Umgebungsvariablen für die Werte in diesem Block erstellt haben, müssen Sie diese nicht einbinden. 

Die Ressource `azurerm_resource_group` weist Terraform an, eine neue Ressourcengruppe zu erstellen. Mehr Ressourcentypen, die in Terraform verfügbar sind, finden Sie weiter unten in diesem Artikel.

## <a name="execute-the-script"></a>Ausführen des Skripts
Nachdem Sie das Skript gespeichert haben, wechseln Sie zur Konsole/Befehlszeile, und geben Sie Folgendes ein:
```
terraform init
```
 zum Initialisieren des Terraform-Anbieters für Azure. Ändern Sie anschließend das Verzeichnis in **terraformscripts**, und geben Sie folgenden Befehl ein:
```
terraform plan
```
Wir haben den Terraform-Befehl `plan` verwendet, der die im Skript definierten Ressourcen betrachtet. Er vergleicht diese mit den von Terraform gespeicherten Zustandsinformationen und gibt dann die geplante Ausführung aus, _ohne_ tatsächlich Ressourcen in Azure zu erstellen. 

Nach der Ausführung des vorherigen Befehls sollte etwa der folgende Bildschirm angezeigt werden:

![Terraform-Plan](./media/terraform/tf_plan2.png)

Falls alles stimmt, stellen Sie diese neue Ressourcengruppe in Azure bereit, indem Sie Folgendes ausführen: 
```
terraform apply
```
Im Azure-Portal sollten Sie die neue, leere Ressourcengruppe namens `terraformtest` sehen. Im folgenden Abschnitt fügen Sie dieser Ressourcengruppe einen virtuellen Computer und die unterstützende Infrastruktur für diesen virtuellen Computer hinzu.

## <a name="provision-an-ubuntu-vm-with-terraform"></a>Bereitstellen einer Ubuntu-VM mit Terraform
Jetzt erweitern wir das Terraform-Skript, das wir zuvor erstellt haben, mit den Details, die für das Bereitstellen eines virtuellen Computers mit Ubuntu erforderlich sind. Die Ressourcen, die Sie in den folgenden Abschnitten bereitstellen, sind:

* Ein Netzwerk mit einzelnem Subnetz
* Eine Netzwerkschnittstellenkarte 
* Ein Speicherkonto für die VM-Diagnose
* Eine öffentliche IP-Adresse
* Ein virtueller Computer, der alle vorherigen Ressourcen nutzt 

Die ausführliche Dokumentation der einzelnen Azure Terraform-Ressourcen finden Sie in der [Terraform-Dokumentation](https://www.terraform.io/docs/providers/azurerm/index.html).

Die Vollversion des [Bereitstellungsskripts](#complete-terraform-script) wird der Einfachheit halber auch bereitgestellt.

### <a name="extend-the-terraform-script"></a>Erweitern des Terraform-Skripts
Erweitern Sie das erstellte Skript mit den folgenden Ressourcen: 
~~~~
# create a virtual network
resource "azurerm_virtual_network" "helloterraformnetwork" {
    name = "acctvn"
    address_space = ["10.0.0.0/16"]
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"

    tags {
        environment = "Terraform Demo"
    }
}

# create subnet
resource "azurerm_subnet" "helloterraformsubnet" {
    name = "acctsub"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    virtual_network_name = "${azurerm_virtual_network.helloterraformnetwork.name}"
    address_prefix = "10.0.2.0/24"
}
~~~~
Das vorherige Skript erstellt ein virtuelles Netzwerk und ein Subnetz innerhalb dieses virtuellen Netzwerks. Beachten Sie den Verweis auf die Ressourcengruppe, die Sie bereits über „${azurerm_resource_group.helloterraform.name}“ sowohl im virtuellen Netzwerk als auch in der Subnetzdefinition erstellt haben.

~~~~
# create public IP
resource "azurerm_public_ip" "helloterraformips" {
    name = "terraformtestip"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    public_ip_address_allocation = "dynamic"

    tags {
        environment = "Terraform Demo"
    }
}

# create network interface
resource "azurerm_network_interface" "helloterraformnic" {
    name = "tfni"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"

    ip_configuration {
        name = "testconfiguration1"
        subnet_id = "${azurerm_subnet.helloterraformsubnet.id}"
        private_ip_address_allocation = "static"
        private_ip_address = "10.0.2.5"
        public_ip_address_id = "${azurerm_public_ip.helloterraformips.id}"
    }

    tags {
        environment = "Terraform Demo"
    }
}
~~~~
Die vorherigen Skriptausschnitte erstellen eine öffentliche IP-Adresse und eine Netzwerkschnittstelle, die die erstellte öffentliche IP-Adresse verwendet. Beachten Sie die Verweise auf subnet_id und public_ip_address_id. Terraform verfügt über integrierte Intelligenz und erkennt, dass die Netzwerkschnittstelle von den Ressourcen abhängt, die vor der Erstellung der Netzwerkschnittstelle erstellt werden müssen.

~~~~
# create a random id
resource "random_id" "randomId" {
  keepers = {
    # Generate a new id only when a new resource group is defined
    resource_group = "${azurerm_resource_group.helloterraform.name}"
  }

  byte_length = 8
}

# create storage account
resource "azurerm_storage_account" "helloterraformstorage" {
    name                = "diag${random_id.randomId.hex}"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    location = "West US"
    account_type = "Standard_LRS"

    tags {
        environment = "Terraform Demo"
    }
}
~~~~
Hier haben Sie ein Speicherkonto erstellt. Dieses Speicherkonto ist dort, wo der virtuelle Computer seine Diagnosedetails speichert.

~~~~
# create virtual machine
resource "azurerm_virtual_machine" "helloterraformvm" {
    name = "terraformvm"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    network_interface_ids = ["${azurerm_network_interface.helloterraformnic.id}"]
    vm_size = "Standard_DS1_v2"

    os_profile {
        computer_name = "hostname"
        admin_username = "azureuser"
        admin_password = ""
    }

    os_profile_linux_config {
        disable_password_authentication = true

        ssh_keys {
            path = "/home/azureuser/.ssh/authorized_keys"
            key_data = "... INSERT OPENSSH PUBLIC KEY HERE ..."
        }
    }

    storage_image_reference {
        publisher = "Canonical"
        offer = "UbuntuServer"
        sku = "16.04.0-LTS"
        version = "latest"
    }

    storage_os_disk {
        name = "myosdisk"
        managed_disk_type = "Premium_LRS"
        create_option = "FromImage"
    } 

    boot_diagnostics {
        enabled = "true"
        storage_uri = "${azurerm_storage_account.helloterraformstorage.primary_blob_endpoint}"
    }

    tags {
        environment = "Terraform Demo"
    }
}
~~~~
Schließlich erstellt der vorherige Codeausschnitt einen virtuellen Computer, der alle zuvor bereitgestellten Ressourcen nutzt. Dies umfasst ein Speicherkonto für die Diagnose des virtuellen Computers, eine Netzwerkschnittstelle mit öffentlicher IP-Adresse und angegebenem Subnetz sowie die Ressourcengruppe, die Sie bereits erstellt haben. Beachten Sie die Eigenschaft „vm_size“, in der das Skript eine Azure Standard DS1v2-SKU angibt.

Sie müssen einen öffentlichen SSH-Schlüssel angeben. Platzieren Sie den Wert des öffentlichen Schlüssels im Abschnitt **... INSERT OPENSSH PUBLIC KEY HERE ...** oben. Sie können ein vorhandenes SSH-Schlüsselpaar verwenden oder gemäß der [Windows](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/ssh-from-windows)- oder [Linux/MacOS](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/mac-create-ssh-keys)-Dokumentation ein Schlüsselpaar generieren.

### <a name="execute-the-script"></a>Ausführen des Skripts
Wenn Sie das vollständige Skript gespeichert haben, wechseln Sie zur Konsole/Befehlszeile, und geben Sie Folgendes ein:
```
terraform apply
```
Nach einiger Zeit werden die Ressourcen einschließlich einem virtuellen Computer in der Ressourcengruppe `terraformtest` im Azure-Portal angezeigt.

## <a name="complete-terraform-script"></a>Vervollständigen des Terraform-Skripts

Unten wird das vollständige Terraform-Skript angezeigt, dass die gesamte in diesem Artikel behandelte Infrastruktur bestimmt.

```
# Configure the Microsoft Azure Provider
provider "azurerm" {
  subscription_id = "XXX"
  client_id       = "XXX"
  client_secret   = "XXX"
  tenant_id       = "XXX"
}

# create a resource group if it doesn't exist
resource "azurerm_resource_group" "helloterraform" {
    name = "terraformtest"
    location = "West US"
}

# create a virtual network
resource "azurerm_virtual_network" "helloterraformnetwork" {
    name = "acctvn"
    address_space = ["10.0.0.0/16"]
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"

    tags {
        environment = "Terraform Demo"
    }
}

# create subnet
resource "azurerm_subnet" "helloterraformsubnet" {
    name = "acctsub"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    virtual_network_name = "${azurerm_virtual_network.helloterraformnetwork.name}"
    address_prefix = "10.0.2.0/24"
}

# create public IP
resource "azurerm_public_ip" "helloterraformips" {
    name = "terraformtestip"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    public_ip_address_allocation = "dynamic"

    tags {
        environment = "Terraform Demo"
    }
}

# create network interface
resource "azurerm_network_interface" "helloterraformnic" {
    name = "tfni"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"

    ip_configuration {
        name = "testconfiguration1"
        subnet_id = "${azurerm_subnet.helloterraformsubnet.id}"
        private_ip_address_allocation = "static"
        private_ip_address = "10.0.2.5"
        public_ip_address_id = "${azurerm_public_ip.helloterraformips.id}"
    }

    tags {
        environment = "Terraform Demo"
    }
}

# create a random id
resource "random_id" "randomId" {
  keepers = {
    # Generate a new id only when a new resource group is defined
    resource_group = "${azurerm_resource_group.helloterraform.name}"
  }

  byte_length = 8
}

# create storage account
resource "azurerm_storage_account" "helloterraformstorage" {
    name                = "diag${random_id.randomId.hex}"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    location = "West US"
    account_type = "Standard_LRS"

    tags {
        environment = "Terraform Demo"
    }
}

# create virtual machine
resource "azurerm_virtual_machine" "helloterraformvm" {
    name = "terraformvm"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    network_interface_ids = ["${azurerm_network_interface.helloterraformnic.id}"]
    vm_size = "Standard_DS1_v2"

    os_profile {
        computer_name = "hostname"
        admin_username = "azureuser"
        admin_password = ""
    }

    os_profile_linux_config {
        disable_password_authentication = true

        ssh_keys {
            path = "/home/azureuser/.ssh/authorized_keys"
            key_data = "... INSERT OPENSSH PUBLIC KEY HERE ..."
        }
    }

    storage_image_reference {
        publisher = "Canonical"
        offer = "UbuntuServer"
        sku = "16.04.0-LTS"
        version = "latest"
    }

    storage_os_disk {
        name = "myosdisk"
        managed_disk_type = "Premium_LRS"
        create_option = "FromImage"
    } 

    boot_diagnostics {
        enabled = "true"
        storage_uri = "${azurerm_storage_account.helloterraformstorage.primary_blob_endpoint}"
    }

    tags {
        environment = "Terraform Demo"
    }
}
```

## <a name="next-steps"></a>Nächste Schritte
Sie haben die grundlegende Infrastruktur in Azure mithilfe von Terraform erstellt. Komplexere Szenarios einschließlich Beispielen zur Verwendung von Lastenausgleichen und VM-Skalierungsgruppen finden Sie in den zahlreichen [Terraform-Beispielen für Azure](https://github.com/hashicorp/terraform/tree/master/examples). Eine vollständige und aktuelle Liste der unterstützten Azure-Anbieter finden Sie in der [Terraform-Dokumentation](https://www.terraform.io/docs/providers/azurerm/index.html).

