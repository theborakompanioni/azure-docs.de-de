---
title: Erstellen einer grundlegenden Infrastruktur in Azure mithilfe von Terraform | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure-Ressourcen mit Terraform erstellen.
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
ms.openlocfilehash: 8b8b3b0b46f79058ee69b9a2014581df433f8d3f
ms.contentlocale: de-de
ms.lasthandoff: 06/20/2017

---

# <a name="create-basic-infrastructure-in-azure-using-terraform"></a>Erstellen einer grundlegenden Infrastruktur in Azure mithilfe von Terraform
In diesem Artikel werden die notwendigen Schritte zur Bereitstellung einer virtuellen Maschine zusammen mit der zugrunde liegenden Infrastruktur in Azure dargelegt. Sie erfahren außerdem, wie Sie Terraform-Skripts erstellen und die Änderungen visualisieren, bevor Sie sie in der Cloudinfrastruktur vornehmen. Außerdem lernen Sie, wie Sie mithilfe von Terraform eine Infrastruktur in Azure erstellen.

Erstellen Sie zu Beginn im Texteditor Ihrer Wahl (Visual Studio Code, Sublime, Vim usw.) eine Datei namens _terraform_azure101.tf_. Der genaue Namen der Datei ist nicht wichtig, weil Terraform den Ordnernamen als Parameter akzeptiert. Es werden also alle Skripts im Ordner ausgeführt. Fügen Sie den folgenden Code in die neue Datei ein:

~~~~
# Configure the Microsoft Azure Provider
# NOTE: if you defined these values as environment variables, you do not have to include this block
provider "azurerm" {
  subscription_id = "your_subscription_id_from_script_execution"
  client_id       = "your_client_id_from_script_execution"
  client_secret   = "your_client_secret_from_script_execution"
  tenant_id       = "your_tenant_id_from_script_execution"
}

# create a resource group 
resource "azurerm_resource_group" "helloterraform" {
    name = "terraformtest"
    location = "West US"
}
~~~~
Im Abschnitt „Provider“ des Skripts weisen Sie Terraform an, im Skript einen Azure-Anbieter für die Bereitstellung von Ressourcen zu verwenden. Im [Handbuch zum Installieren und Konfigurieren von Terraform](terraform-install-configure.md) finden Sie die Werte für subscription_id, client_id, client_secret und tenant_id. Beachten Sie, dass wenn Sie Umgebungsvariablen für die Werte in diesem Block erstellt haben, dass Sie diese nicht einbinden müssen. 

Die Ressource „azure_rm_resource_group“ weist Terraform an, eine neue Ressourcengruppe zu erstellen. Nachstehend finden Sie weitere, in Terraform verfügbare Ressourcentypen.

## <a name="executing-the-script"></a>Ausführen des Skripts
Wenn Sie das Skript gespeichert haben, wechseln Sie zur Konsole/Befehlszeile, und geben Sie Folgendes ein:
```
terraform plan terraformscripts
```
Zuvor wurde angenommen, dass „terraformscripts“ der Ordner ist, in dem das Skript gespeichert wurde. Wir haben den Terraform-Befehl „plan“ verwendet, der die in den Skripts definierten Ressourcen prüft, sie mit den von Terraform gespeicherten Zustandsinformationen vergleicht und dann die geplante Ausführung ausgibt, _ohne_ tatsächlich Ressourcen in Azure zu erstellen. 

Nach der Ausführung des Befehls sollte etwa der folgende Bildschirm angezeigt werden:

![Bild des Terraform-Befehls „plan“](linux/media/terraform/tf_plan2.png)

Fahren Sie fort, falls alles stimmt, und stellen Sie diese neue Ressourcengruppe in Azure bereit, indem Sie Folgendes ausführen: 
```
terraform apply terraformscripts
```
Im Azure-Portal sollten Sie nun die neue, leere Ressourcengruppe namens „terraformtest“ sehen. Im folgenden Abschnitt werden Sie dieser Ressourcengruppe einen virtuellen Computer und die unterstützende Infrastruktur für diesen virtuellen Computer hinzufügen.

## <a name="provisioning-ubuntu-vm-with-terraform"></a>Bereitstellen einer Ubuntu-VM mit Terraform
Jetzt erweitern wir das Terraform-Skript, das wir zuvor erstellt haben, mit Details, die für das Bereitstellen eines virtuellen Computers mit Ubuntu erforderlich sind. Die Ressourcen, die Sie in den folgenden Abschnitten bereitstellen, sind ein Netzwerk mit einem einzelnen Subnetz, eine Netzwerkschnittstellenkarte, ein Speicherkonto mit einem Speichercontainer, eine öffentliche IP-Adresse und ein virtueller Computer, der die oben genannten Ressourcen nutzt. Die ausführliche Dokumentation der einzelnen Terraform-Ressourcen für Azure finden Sie in der [Terraform-Dokumentation](https://www.terraform.io/docs/providers/azurerm/index.html).

Die Vollversion des [Bereitstellungsskripts](#complete-terraform-script) wird der Einfachheit halber auch bereitgestellt.

### <a name="extending-the-terraform-script"></a>Erweitern des Terraform-Skripts
Erweitern Sie das oben erstellte Skript mit den folgenden Ressourcen: 
~~~~
# create a virtual network
resource "azurerm_virtual_network" "helloterraformnetwork" {
    name = "acctvn"
    address_space = ["10.0.0.0/16"]
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
}

# create subnet
resource "azurerm_subnet" "helloterraformsubnet" {
    name = "acctsub"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    virtual_network_name = "${azurerm_virtual_network.helloterraformnetwork.name}"
    address_prefix = "10.0.2.0/24"
}
~~~~
Das obige Skript erstellt ein virtuelles Netzwerk und ein Subnetz innerhalb dieses virtuellen Netzwerks. Beachten Sie den Verweis auf die Ressourcengruppe, die Sie bereits über „${azurerm_resource_group.helloterraform.name}“ sowohl im virtuellen Netzwerk als auch in der Subnetzdefinition erstellt haben.

~~~~
# create public IP
resource "azurerm_public_ip" "helloterraformips" {
    name = "terraformtestip"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    public_ip_address_allocation = "dynamic"

    tags {
        environment = "TerraformDemo"
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
}
~~~~
Die Skriptausschnitte oben erstellen eine öffentliche IP-Adresse und eine Netzwerkschnittstelle, die die erstellte öffentliche IP-Adresse verwendet. Beachten Sie die Verweise auf subnet_id und public_ip_address_id. Terraform verfügt über integrierte Intelligenz und erkennt, dass die Netzwerkschnittstelle von den Ressourcen abhängt, die vor der Erstellung der Netzwerkschnittstelle erstellt werden müssen.

~~~~
# create storage account
resource "azurerm_storage_account" "helloterraformstorage" {
    name = "helloterraformstorage"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    location = "westus"
    account_type = "Standard_LRS"

    tags {
        environment = "staging"
    }
}

# create storage container
resource "azurerm_storage_container" "helloterraformstoragestoragecontainer" {
    name = "vhd"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    storage_account_name = "${azurerm_storage_account.helloterraformstorage.name}"
    container_access_type = "private"
    depends_on = ["azurerm_storage_account.helloterraformstorage"]
}
~~~~
Hier haben Sie ein Speicherkonto erstellt und einen Speichercontainer innerhalb dieses Speicherkontos definiert, wo zu erstellende VHDs für den virtuellen Computer gespeichert werden.

~~~~
# create virtual machine
resource "azurerm_virtual_machine" "helloterraformvm" {
    name = "terraformvm"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    network_interface_ids = ["${azurerm_network_interface.helloterraformnic.id}"]
    vm_size = "Standard_A0"

    storage_image_reference {
        publisher = "Canonical"
        offer = "UbuntuServer"
        sku = "14.04.2-LTS"
        version = "latest"
    }

    storage_os_disk {
        name = "myosdisk"
        vhd_uri = "${azurerm_storage_account.helloterraformstorage.primary_blob_endpoint}${azurerm_storage_container.helloterraformstoragestoragecontainer.name}/myosdisk.vhd"
        caching = "ReadWrite"
        create_option = "FromImage"
    }

    os_profile {
        computer_name = "hostname"
        admin_username = "testadmin"
        admin_password = "Password1234!"
    }

    os_profile_linux_config {
        disable_password_authentication = false
    }

    tags {
        environment = "staging"
    }
}
~~~~
In dem obigen Ausschnitt wird schließlich ein virtueller Computer erstellt, der alle bereitgestellten Ressourcen nutzt: das Speicherkonto und den Container für eine virtuelle Festplatte (VHD), die Netzwerkschnittstelle mit einer öffentlichen IP-Adresse und das angegebene Subnetz sowie die bereits erstellte Ressourcengruppe. Beachten Sie die Eigenschaft „vm_size“, in der das Skript eine Azure A0-SKU angibt.

### <a name="executing-the-script"></a>Ausführen des Skripts
Wenn Sie das vollständige Skript gespeichert haben, wechseln Sie zur Konsole/Befehlszeile, und geben Sie Folgendes ein:
```
terraform apply terraformscripts
```
Nach einiger Zeit sollten die Ressourcen einschließlich einem virtuellen Computer in der Ressourcengruppe „terraformtest“ im Azure-Portal angezeigt werden.

## <a name="complete-terraform-script"></a>Vervollständigen des Terraform-Skripts

```
variable "resourcesname" {
  default = "helloterraform"
}

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

# create virtual network
resource "azurerm_virtual_network" "helloterraformnetwork" {
    name = "tfvn"
    address_space = ["10.0.0.0/16"]
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
}

# create subnet
resource "azurerm_subnet" "helloterraformsubnet" {
    name = "tfsub"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    virtual_network_name = "${azurerm_virtual_network.helloterraformnetwork.name}"
    address_prefix = "10.0.2.0/24"
}


# create public IPs
resource "azurerm_public_ip" "helloterraformips" {
    name = "terraformtestip"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    public_ip_address_allocation = "dynamic"

    tags {
        environment = "TerraformDemo"
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
}

# create storage account
resource "azurerm_storage_account" "helloterraformstorage" {
    name = "helloterraformstorage"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    location = "westus"
    account_type = "Standard_LRS"

    tags {
        environment = "staging"
    }
}

# create storage container
resource "azurerm_storage_container" "helloterraformstoragestoragecontainer" {
    name = "vhd"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    storage_account_name = "${azurerm_storage_account.helloterraformstorage.name}"
    container_access_type = "private"
    depends_on = ["azurerm_storage_account.helloterraformstorage"]
}

# create virtual machine
resource "azurerm_virtual_machine" "helloterraformvm" {
    name = "terraformvm"
    location = "West US"
    resource_group_name = "${azurerm_resource_group.helloterraform.name}"
    network_interface_ids = ["${azurerm_network_interface.helloterraformnic.id}"]
    vm_size = "Standard_A0"

    storage_image_reference {
        publisher = "Canonical"
        offer = "UbuntuServer"
        sku = "14.04.2-LTS"
        version = "latest"
    }

    storage_os_disk {
        name = "myosdisk"
        vhd_uri = "${azurerm_storage_account.helloterraformstorage.primary_blob_endpoint}${azurerm_storage_container.helloterraformstoragestoragecontainer.name}/myosdisk.vhd"
        caching = "ReadWrite"
        create_option = "FromImage"
    }

    os_profile {
        computer_name = "hostname"
        admin_username = "testadmin"
        admin_password = "Password1234!"
    }

    os_profile_linux_config {
        disable_password_authentication = false
    }

    tags {
        environment = "staging"
    }
}
```

## <a name="next-steps"></a>Nächste Schritte
Sie haben mit Terraform die grundlegenden Infrastruktur in Azure erstellt. Für komplexere Szenarios einschließlich Beispielen zur Verwendung von Lastenausgleichen und VM-Skalierungsgruppen werfen Sie einen Blick auf die zahlreichen [Terraform Beispiele für Azure](https://github.com/hashicorp/terraform/tree/master/examples). In der [Terraform-Dokumentation](https://www.terraform.io/docs/providers/azurerm/index.html) finden Sie eine vollständige und aktuelle Liste der unterstützten Azure-Anbieter.
