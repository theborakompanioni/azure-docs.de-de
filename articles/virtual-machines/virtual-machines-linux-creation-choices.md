<properties
	pageTitle="Verschiedene Möglichkeiten zum Erstellen eines virtuellen Linux-Computers | Microsoft Azure"
	description="Enthält eine Liste mit den verschiedenen Möglichkeiten zum Erstellen eines virtuellen Linux-Computers unter Azure und für jede Methode Links zu Tools und Tutorials."
	services="virtual-machines-linux"
	documentationCenter=""
	authors="iainfoulds"
	manager="timlt"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-linux"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.tgt_pltfrm="vm-linux"
	ms.workload="infrastructure-services"
	ms.date="07/06/2016"
	ms.author="iainfou"/>

# Verschiedene Möglichkeiten zum Erstellen eines virtuellen Linux-Computers mit Resource Manager

Virtuelle Computer können mit Azure und dem Resource Manager-Bereitstellungsmodell auf unterschiedliche Arten erstellt werden, um den Anforderungen unterschiedlicher Benutzer und Szenarien gerecht zu werden. Dieser Artikel fasst die Unterschiede und die Entscheidungen zusammen, die Sie beim Erstellen Ihres virtuellen Linux-Computers treffen können.

## Azure-Befehlszeilenschnittstelle 

Die Azure-Befehlszeilenschnittstelle ist plattformübergreifend über ein npm-Paket, per Distributionspaket oder per Docker-Container verfügbar. Weitere Informationen finden Sie unter [How to install and configure the Azure CLI](../xplat-cli-install.md) (Installieren und Konfigurieren der Azure-Befehlszeilenschnittstelle). In den folgenden Tutorials wird die Verwendung der Azure-Befehlszeilenschnittstelle anhand von Beispielen veranschaulicht. Jeder Artikel enthält ausführliche Informationen zu den CLI-Schnellstartbefehlen:

* [Erstellen eines virtuellen Linux-Computers in Azure mithilfe der Befehlszeilenschnittstelle](virtual-machines-linux-quick-create-cli.md)

	```bash
	azure vm quick-create -M ~/.ssh/azure_id_rsa.pub -Q CoreOS
	```

* [Erstellen einer geschützten Linux-VM mit einer Azure-Vorlage](virtual-machines-linux-create-ssh-secured-vm-from-template.md)

	```bash
	azure group create --name TestRG --location WestUS 
		--template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
	```

* [Erstellen einer Linux-VM von Grund auf mit der Azure-Befehlszeilenschnittstelle](virtual-machines-linux-create-cli-complete.md)

* [Hinzufügen eines Datenträgers zu einem virtuellen Linux-Computer](virtual-machines-linux-add-disk.md)

	```bash
	azure vm disk attach-new --resource-group TestRG --vm-name TestVM <size-in-GB>
	```

## Azure-Portal

Die grafische Benutzeroberfläche des [Azure-Portals](https://portal.azure.com) bietet eine einfache Möglichkeit zum Testen eines virtuellen Computers. Dies gilt insbesondere dann, wenn Sie noch nicht viel Erfahrung mit Azure haben, da hierzu keine Installation auf Ihrem System erforderlich ist. Verwenden Sie das Azure-Portal, um den virtuellen Computer zu erstellen:

* [Erstellen einer Linux-VM mit dem Azure-Portal](virtual-machines-linux-quick-create-portal.md)
* [Anfügen eines Datenträgers mit dem Azure-Portal](virtual-machines-linux-attach-disk-portal.md)

## Betriebssystem und Image-Optionen
Beim Erstellen einer VM muss ein für das auszuführende Betriebssystem geeignetes Image gewählt werden. Azure und seine Partner bieten zahlreiche Images an – einige mit bereits vorinstallierten Anwendungen und Tools. Alternativ können Sie auch ein eigenes Image hochladen (siehe unten).

### Azure-Images
Sie können die `azure vm image`-CLI-Befehle verwenden, um anzuzeigen, was verfügbar ist, und zwar nach Herausgeber, Distributionsversion und Build.

Liste mit den verfügbaren Herausgebern:

```bash
azure vm image list-publishers --location WestUS
```

Liste mit den verfügbaren Produkten (Angeboten) für einen Herausgeber:

```bash
azure vm image list-offers --location WestUS --publisher Canonical
```

Liste mit den verfügbaren SKUs (Distributionsversionen) eines Angebots:

```bash
azure vm image list-skus --location WestUS --publisher Canonical --offer UbuntuServer
```

Liste mit allen verfügbaren Images für eine bestimmte Version:

```bash
azure vm image list --location WestUS --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS
```

Weitere Beispiele für das Navigieren zu und Verwenden von verfügbaren Images finden Sie unter [Navigieren zu und Auswählen von Images virtueller Linux-Computer in Azure mithilfe der Befehlszeilenschnittstelle oder PowerShell](virtual-machines-linux-cli-ps-findimage.md).

Die Befehle `azure vm quick-create` und `azure vm create` verfügen auch über Aliase, die Sie zum schnellen Zugreifen auf die gängigeren Distributionen und ihre neuesten Versionen verwenden können. Dies ist einfacher, als bei jeder Erstellung einer VM den Herausgeber, das Angebot, die SKU und die Version angeben zu müssen:

| Alias | Herausgeber | Angebot | SKU | Version |
|:----------|:----------|:-------------|:------------|:--------|
| CentOS | OpenLogic | Centos | 7,2 | neueste |
| CoreOS | CoreOS | CoreOS | Stable | neueste |
| Debian | credativ | Debian | 8 | neueste |
| openSUSE | SUSE | openSUSE | 13\.2 | neueste |
| RHEL | Redhat | RHEL | 7,2 | neueste |
| SLES | SLES | SLES | 12-SP1 | neueste |
| UbuntuLTS | Canonical | UbuntuServer | 14\.04.4-LTS | neueste |

### Verwenden eines eigenen Image

Sollten Sie spezielle Anpassungen benötigen, verwenden Sie ein Image, das auf einem vorhandenen virtuellen Azure-Computer basiert. Hierzu können Sie diesen virtuellen Computer *erfassen* oder ein eigenes, auf einer virtuellen Festplatte (Virtual Hard Disk, VHD) gespeichertes Image hochladen. Weitere Informationen zu unterstützten Distributionen und zur Verwendung eigener Images finden Sie in den folgenden Artikeln:

* [Von Azure unterstützte Distributionen](virtual-machines-linux-endorsed-distros.md)

* [Informationen zu nicht unterstützten Distributionen](virtual-machines-linux-create-upload-generic.md)

* [Erfassen eines virtuellen Linux-Computers zur Verwendung als Resource Manager-Vorlage](virtual-machines-linux-capture-image.md) Schnellstartbefehle:

	```bash
	azure vm deallocate --resource-group TestRG --vm-name TestVM
	azure vm generalize --resource-group TestRG --vm-name TestVM
	azure vm capture --resource-group TestRG --vm-name TestVM --vhd-name-prefix CapturedVM
	```

## Nächste Schritte

* Probieren Sie eines der Tutorials aus, um einen virtuellen Linux-Computer über das [Portal](virtual-machines-linux-quick-create-portal.md), mit der [CLI](virtual-machines-linux-quick-create-cli.md) oder mit einer Azure Resource Manager-[Vorlage](virtual-machines-linux-cli-deploy-templates.md) zu erstellen.

* Nach dem Erstellen eines virtuellen Linux-Computers können Sie problemlos [einen Datenträger hinzufügen](virtual-machines-linux-add-disk.md).

* Schritte zum schnellen Zurücksetzen von Kennwörtern oder SSH-Schlüsseln sowie zum Verwalten von Benutzern finden Sie [hier](virtual-machines-linux-using-vmaccess-extension.md).

<!---HONumber=AcomDC_0706_2016-->