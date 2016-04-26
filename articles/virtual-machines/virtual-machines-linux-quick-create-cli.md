<properties
   pageTitle="Erstellen einer Linux-VM in Azure mithilfe der Befehlszeilenschnittstelle (CLI) | Microsoft Azure"
   description="Erstellen Sie einen virtuellen Linux-Computer in Azure mithilfe der Befehlszeilenschnittstelle."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="vlivech"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="04/08/2016"
   ms.author="v-livech"/>


# Erstellen eines virtuellen Linux-Computers in Azure mithilfe der Befehlszeilenschnittstelle

Dieser Artikel zeigt, wie Sie mit dem Befehl `azure vm quick-create` der Azure-Befehlszeilenschnittstelle schnell einen virtuellen Linux-Computer erstellen können. Der Befehl `quick-create` erstellt einen virtuellen Computer mit einer Basisinfrastruktur, mit dem Sie rasch einen Prototyp erstellen oder ein Konzept testen können. Dies ist der schnellste Weg zu einer Linux-Bash-Shell. Für die Schritte in diesem Artikel ist ein Azure-Konto ([kostenlose Testversion herunterladen](https://azure.microsoft.com/pricing/free-trial/)) und die [Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md) im Resource Manager-Modus (`azure config mode arm`) erforderlich.

## Kurze Zusammenfassung der Befehle

```
# One command to quickly the VM that prompts for arguments
ahmet@fedora$ azure vm quick-create -M ~/.ssh/azure_id_rsa.pub
```

## Ausführliche exemplarische Vorgehensweise

## Erstellen der Linux-VM

Im folgenden Befehl können Sie ein beliebiges Image verwenden, aber in diesem Beispiel wird `canonical:ubuntuserver:14.04.2-LTS:latest` verwendet, um auf schnelle Weise einen virtuellen Computer zu erstellen. (Um ein Image im Marketplace zu finden, können Sie [ein Image suchen](virtual-machines-linux-cli-ps-findimage.md) oder [Ihr eigenes benutzerdefiniertes Image hochladen](virtual-machines-linux-create-upload-generic.md).) Etwa Folgendes sollte angezeigt werden:

Ersetzen Sie in der folgenden exemplarischen Vorgehensweise für einen Befehl die Aufforderungen durch die Werte aus Ihrer eigenen Umgebung. Für diesen Artikel werden Beispielwerte verwendet.

```bash
# Create the Linux VM using prompts
ahmet@fedora$ azure vm quick-create -M ~/.ssh/azure_id_rsa.pub
info:    Executing command vm quick-create
Resource group name: exampleRGname
Virtual machine name: exampleVMname
Location name: westus
Operating system Type [Windows, Linux]: Linux
ImageURN (in the format of "publisherName:offer:skus:version") or a VHD link to the user image: Canonical:UbuntuServer:14.04.4-LTS:latest
User name: ahmet
Password: ************************************************
Confirm password: ************************************************
+ Looking up the VM "exampleVMname"
info:    Verifying the public key SSH file: /home/ahmet/.ssh/azure_id_rsa.pub
info:    Using the VM Size "Standard_D1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Looking up the storage account cli38948918364134011018
info:    Could not find the storage account "cli38948918364134011018", trying to create new one
+ Creating storage account "cli38948918364134011018" in "westus"
+ Looking up the storage account cli38948918364134011018
+ Looking up the NIC "examp-westu-3894891836-nic"
info:    An nic with given name "examp-westu-3894891836-nic" not found, creating a new one
+ Looking up the virtual network "examp-westu-3894891836-vnet"
info:    Preparing to create new virtual network and subnet
| Creating a new virtual network "examp-westu-3894891836-vnet" [address prefix: "10.0.0.0/16"] with subnet "examp-westu-3894891836-snet" [address prefix: "10.+.1.0/24"]
+ Looking up the virtual network "examp-westu-3894891836-vnet"
+ Looking up the subnet "examp-westu-3894891836-snet" under the virtual network "examp-westu-3894891836-vnet"
info:    Found public ip parameters, trying to setup PublicIP profile
+ Looking up the public ip "examp-westu-3894891836-pip"
info:    PublicIP with given name "examp-westu-3894891836-pip" not found, creating a new one
+ Creating public ip "examp-westu-3894891836-pip"
+ Looking up the public ip "examp-westu-3894891836-pip"
+ Creating NIC "examp-westu-3894891836-nic"
+ Looking up the NIC "examp-westu-3894891836-nic"
+ Creating VM "exampleVMname"
+ Looking up the VM "exampleVMname"
+ Looking up the NIC "examp-westu-3894891836-nic"
+ Looking up the public ip "examp-westu-3894891836-pip"
data:    Id                              :/subscriptions/<**subscriptionsID**>/resourceGroups/exampleRGname/providers/Microsoft.Compute/virtualMachines/exampleVMname
data:    ProvisioningState               :Succeeded
data:    Name                            :exampleVMname
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_D1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :Canonical
data:        Offer                       :UbuntuServer
data:        Sku                         :14.04.4-LTS
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clife36db80ae0539d2-os-1460152163612
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://<**subscriptionsID**>.blob.core.windows.net/vhds/clife36db80ae0539d2-os-1460152163612.vhd
data:
data:    OS Profile:
data:      Computer Name                 :exampleVMname
data:      User Name                     :ahmet
data:      Linux Configuration:
data:        Disable Password Auth       :false
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-33-4C-B2
data:          Provisioning State        :Succeeded
data:          Name                      :examp-westu-3894891836-nic
data:          Location                  :westus
data:            Public IP address       :13.88.22.244
data:            FQDN                    :examp-westu-3894891836-pip.westus.cloudapp.azure.com
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://<**subscriptionsID**>.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm quick-create command OK
```

Sie können jetzt eine SSH-Verbindung mit Ihrem virtuellen Computer auf dem SSH-Standardport 22 und der öffentlichen IP-Adresse (s. Ausgabe oben) herstellen.

```
ahmet@fedora$ ssh -i ~/.ssh/azure_id_rsa ubuntu@13.88.22.244
```

`azure vm quick-create` ist eine Möglichkeit, schnell einen virtuellen Computer zu erstellen, damit Sie sich bei einer Bash-Shell anmelden und arbeiten können. Durch die Verwendung von `vm quick-create` erhalten Sie jedoch nicht die Vorteiler einer komplexen Umgebung. Wenn Sie Ihre Umgebung anpassen möchten, können Sie eine [Azure Resource Manager-Vorlage verwenden, um schnell eine bestimmte Bereitstellung zu erstellen](virtual-machines-linux-cli-deploy-templates.md), oder Sie können [Ihre eigene benutzerdefinierte Umgebung für einen virtuellen Linux-Computer direkt mit Befehlen der Azure-Befehlszeilenschnittstelle erstellen](virtual-machines-linux-cli-deploy-templates.md).

Das obige Beispiel erstellt:

- eine Azure-Ressourcengruppe, in der die VM bereitgestellt wird
- ein Azure Storage-Konto zum Speichern der VHD-Datei, die das VM-Image ist
- ein virtuelles Azure-Netzwerk und Subnetz für die Verbindung mit der VM
- eine virtuelle Netzwerkschnittstellen-Karte (Network Interface Card, NIC), um die VM dem Netzwerk zuzuordnen
- eine öffentliche IP-Adresse und ein Unterdomänenpräfix, um eine Internetadresse für die externe Verwendung bereitzustellen, und anschließend wird die Linux-VM in dieser Umgebung erstellt.

## Nächste Schritte

Sie haben nun schnell eine Linux-VM zum Testen und für Demonstrationszwecke erstellt. Lesen Sie zum Erstellen eines virtuellen Linux-Computers, der an Ihre Infrastruktur angepasst ist, einen der folgenden Artikel.

- [Bereitstellen und Verwalten von virtuellen Computern mit Azure-Ressourcen-Manager-Vorlagen und der Azure-CLI](virtual-machines-linux-cli-deploy-templates.md)
- [Erstellen einer geschützten Linux-VM mit einer Azure-Vorlage](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
- [Erstellen einer Linux-VM von Grund auf mit der Azure-Befehlszeilenschnittstelle](virtual-machines-linux-create-cli-complete.md)

Diese Artikel beschreiben die ersten Schritte beim Erstellen einer Azure-Infrastruktur sowie einer beliebigen Anzahl von proprietären und Open Source-Tools zur Infrastrukturbereitstellung, Konfiguration und Orchestrierung.

<!---HONumber=AcomDC_0420_2016-->