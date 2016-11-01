<properties
   pageTitle="Erstellen einer Linux-VM in Azure mithilfe der CLI| Microsoft Azure"
   description="Erstellen Sie mithilfe der CLI eine Linux-VM in Azure."
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
   ms.date="09/08/2016"
   ms.author="v-livech"/>


# Erstellen einer Linux-VM in Azure mithilfe der CLI

In diesem Artikel wird erläutert, wie Sie mit dem Befehl `azure vm quick-create` in der Azure-Befehlszeilenschnittstelle (CLI) schnell einen virtuellen Linux-Computer (VM) bereitstellen können. Der Befehl `quick-create` stellt eine VM in einer sicheren Basisinfrastruktur bereit, mit der Sie rasch einen Prototyp erstellen oder ein Konzept testen können. Zum Ausführen der Schritte in diesem Artikel ist Folgendes erforderlich:

- Azure-Konto ([kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/))

- [Azure-CLI](../xplat-cli-install.md), angemeldet mit `azure login`

- Azure-CLI _muss im_ Azure Resource Manager-Modus `azure config mode arm` ausgeführt werden

Sie können eine Linux-VM auch schnell über das [Azure-Portal](virtual-machines-linux-quick-create-portal.md) bereitstellen.

## Schnellbefehle

Das folgende Beispiel zeigt, wie Sie eine CoreOS-VM bereitstellen und Ihren SSH-Schlüssel (Secure Shell) anfügen (Ihre Argumente können sich von den hier verwendeten Argumenten unterscheiden):

```bash
azure vm quick-create -M ~/.ssh/azure_id_rsa.pub -Q CoreOS
```

In den folgenden Abschnitten wird zur Erläuterung des Befehls und der Anforderungen Ubuntu Server 14.04 LTS als Linux-Distribution verwendet.

## Aliase für die Schnellerstellung mit „vm quick-create“

Eine schnelle Möglichkeit zum Auswählen einer Distribution ist die Verwendung der entsprechenden Azure-CLI-Aliase für die am häufigsten verwendeten OS-Distributionen. Die folgende Tabelle enthält eine Liste der Aliase (ab Azure-CLI Version 0.10). Bei allen Bereitstellungen mit `quick-create` werden standardmäßig VMs mit SSD-Speicher (Solid State Drive) erstellt, die eine schnellere Bereitstellung und hohe Leistung für den Datenträgerzugriff bieten. (Diese Aliase stellen einen kleinen Teil der in Azure verfügbaren Distributionen dar. Sie können auf dem Azure Marketplace auf weitere Images zugreifen, indem Sie [nach einem Image suchen](virtual-machines-linux-cli-ps-findimage.md), oder [ein eigenes benutzerdefiniertes Image hochladen](virtual-machines-linux-create-upload-generic.md).)

| Alias | Herausgeber | Angebot | SKU | Version |
|:----------|:----------|:-------------|:------------|:--------|
| CentOS | OpenLogic | CentOS | 7,2 | neueste |
| CoreOS | CoreOS | CoreOS | Stable | neueste |
| Debian | credativ | Debian | 8 | neueste |
| openSUSE | SUSE | openSUSE | 13\.2 | neueste |
| RHEL | Red Hat | RHEL | 7\.2 | neueste |
| UbuntuLTS | Canonical | Ubuntu Server | 14\.04.4-LTS | neueste |

In den folgenden Abschnitten wird der Alias `UbuntuLTS` für die Option **ImageURN** (`-Q`) verwendet, um einen Ubuntu 14.04.4 LTS-Server bereitzustellen.

## Ausführliche exemplarische Vorgehensweise

Im vorherigen Beispiel für `quick-create` wurde nur das Flag `-M` aufgerufen, um den hochzuladenden öffentlichen SSH-Schlüssel zu identifizieren, während SSH-Kennwörter deaktiviert wurden. Sie werden daher zum Angeben der folgenden Argumente aufgefordert:

- Ressourcengruppenname (für Ihre erste Azure-Ressourcengruppe ist in der Regel eine beliebige Zeichenfolge ausreichend)
- Name des virtuellen Computers
- Standort (`westus` oder `westeurope` sind gute Standardwerte)
- Linux (um Azure das zu verwendende Betriebssystem mitzuteilen)
- username

Da im folgenden Beispiel alle Werte angegeben werden, sind keine weiteren Eingabeaufforderungen erforderlich. Sofern Sie über eine Datei `~/.ssh/id_rsa.pub` als öffentliche Schlüsseldatei im ssh-rsa-Format verfügen, sind keine Änderungen erforderlich:

```bash
azure vm quick-create \
-g exampleResourceGroup \
-n exampleVMName \
-l westus \
-y Linux \
-u exampleAdminUser \
-M ~/.ssh/id_rsa.pub \
-Q UbuntuLTS
```

Die Ausgabe sollte dem folgenden Ausgabeblock ähneln:

```bash
info:    Executing command vm quick-create
+ Listing virtual machine sizes available in the location "westus"
+ Looking up the VM "exampleVMName"
info:    Verifying the public key SSH file: /Users/ahmet/.ssh/id_rsa.pub
info:    Using the VM Size "Standard_DS1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Looking up the storage account cli16330708391032639673
+ Looking up the NIC "examp-westu-1633070839-nic"
info:    An nic with given name "examp-westu-1633070839-nic" not found, creating a new one
+ Looking up the virtual network "examp-westu-1633070839-vnet"
info:    Preparing to create new virtual network and subnet
/ Creating a new virtual network "examp-westu-1633070839-vnet" [address prefix: "10.0.0.0/16"] with subnet "examp-westu-1633070839-snet" [address prefix: "10.+.1.0/24"]
+ Looking up the virtual network "examp-westu-1633070839-vnet"
+ Looking up the subnet "examp-westu-1633070839-snet" under the virtual network "examp-westu-1633070839-vnet"
info:    Found public ip parameters, trying to setup PublicIP profile
+ Looking up the public ip "examp-westu-1633070839-pip"
info:    PublicIP with given name "examp-westu-1633070839-pip" not found, creating a new one
+ Creating public ip "examp-westu-1633070839-pip"
+ Looking up the public ip "examp-westu-1633070839-pip"
+ Creating NIC "examp-westu-1633070839-nic"
+ Looking up the NIC "examp-westu-1633070839-nic"
+ Looking up the storage account clisto1710997031examplev
+ Creating VM "exampleVMName"
+ Looking up the VM "exampleVMName"
+ Looking up the NIC "examp-westu-1633070839-nic"
+ Looking up the public ip "examp-westu-1633070839-pip"
data:    Id                              :/subscriptions/2<--snip-->d/resourceGroups/exampleResourceGroup/providers/Microsoft.Compute/virtualMachines/exampleVMName
data:    ProvisioningState               :Succeeded
data:    Name                            :exampleVMName
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
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
data:        Name                        :clic7fadb847357e9cf-os-1473374894359
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://cli16330708391032639673.blob.core.windows.net/vhds/clic7fadb847357e9cf-os-1473374894359.vhd
data:
data:    OS Profile:
data:      Computer Name                 :exampleVMName
data:      User Name                     :exampleAdminUser
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-33-42-FB
data:          Provisioning State        :Succeeded
data:          Name                      :examp-westu-1633070839-nic
data:          Location                  :westus
data:            Public IP address       :138.91.247.29
data:            FQDN                    :examp-westu-1633070839-pip.westus.cloudapp.azure.com
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://clisto1710997031examplev.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm quick-create command OK
```

Melden Sie sich mit der in der Ausgabe aufgeführten öffentlichen IP-Adresse bei Ihrer VM an. Sie können auch den vollständig qualifizierten Domänennamen (FQDN) verwenden, der in der Ausgabe angegeben ist:

```bash
ssh -i ~/.ssh/id_rsa.pub exampleAdminUser@138.91.247.29
```

Der Anmeldeprozess sollte etwa wie der folgende Ausgabeblock aussehen:

```bash
Warning: Permanently added '138.91.247.29' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 14.04.4 LTS (GNU/Linux 3.19.0-65-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

  System information as of Thu Sep  8 22:50:57 UTC 2016

  System load: 0.63              Memory usage: 2%   Processes:       81
  Usage of /:  39.6% of 1.94GB   Swap usage:   0%   Users logged in: 0

  Graph this data and manage this system at:
    https://landscape.canonical.com/

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.



The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

exampleAdminUser@exampleVMName:~$
```

## Nächste Schritte

Der Befehl `azure vm quick-create` ermöglicht die schnelle Bereitstellung einer VM – Sie können sich bei einer Bash-Shell anmelden und mit der Arbeit beginnen. Bei der Verwendung von `vm quick-create` haben Sie jedoch weder umfassende Kontrolle, noch können Sie eine komplexere Umgebung erstellen. Informationen zum Bereitstellen einer für Ihre Infrastruktur angepassten Linux-VM finden Sie in den Artikeln zu folgenden Themen:

- [Bereitstellen und Verwalten von virtuellen Computern mit Azure-Ressourcen-Manager-Vorlagen und der Azure-CLI](virtual-machines-linux-cli-deploy-templates.md)
- [Direktes Erstellen einer benutzerdefinierten Umgebung für einen virtuellen Linux-Computer über Azure-CLI-Befehle](virtual-machines-linux-create-cli-complete.md)
- [Erstellen einer SSH-geschützten Linux-VM mit einer Azure-Vorlage](virtual-machines-linux-create-ssh-secured-vm-from-template.md)

Sie können auch [den Azure-Treiber `docker-machine` mit verschiedenen Befehlen verwenden, um eine Linux-VM schnell als Docker-Host zu erstellen](virtual-machines-linux-docker-machine.md).

<!---HONumber=AcomDC_1005_2016-->