<properties
	pageTitle="Verschiedene Möglichkeiten zum Erstellen eines virtuellen Linux-Computers | Microsoft Azure"
	description="Hier finden Sie Informationen zu den verschiedenen Möglichkeiten zum Erstellen eines virtuellen Linux-Computers unter Azure sowie Links zu Tools und Tutorials für die einzelnen Methoden."
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
	ms.date="09/27/2016"
	ms.author="iainfou"/>

# Verschiedene Möglichkeiten zum Erstellen eines virtuellen Linux-Computers in Azure

In Azure können Sie einen virtuellen Linux-Computer mit Ihren bevorzugten Tools und Workflows erstellen. Dieser Artikel fasst die Unterschiede und Beispiele für die Erstellung virtueller Linux-Computer zusammen.


## Azure-Befehlszeilenschnittstelle 

Die Azure-Befehlszeilenschnittstelle ist plattformübergreifend über ein npm-Paket, per Distributionspaket oder per Docker-Container verfügbar. Weitere Informationen finden Sie unter [How to install and configure the Azure CLI](../xplat-cli-install.md) (Installieren und Konfigurieren der Azure-Befehlszeilenschnittstelle). In den folgenden Tutorials wird die Verwendung der Azure-Befehlszeilenschnittstelle anhand von Beispielen veranschaulicht. Jeder Artikel enthält ausführliche Informationen zu den CLI-Schnellstartbefehlen:

- [Erstellen eines virtuellen Linux-Computers in Azure mithilfe der Befehlszeilenschnittstelle](virtual-machines-linux-quick-create-cli.md)
	- Das folgende Beispiel erstellt einen virtuellen CoreOS-Computer mit einem öffentlichen Schlüssel namens `azure_id_rsa.pub`:

	```bash
	azure vm quick-create -ssh-publickey-file ~/.ssh/azure_id_rsa.pub \
		--image-urn CoreOS
	```

- [Erstellen einer geschützten Linux-VM mit einer Azure-Vorlage](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
	- Das folgende Beispiel erstellt einen virtuellen Computer unter Verwendung einer auf GitHub gespeicherten Vorlage:

	```bash
	azure group create --name TestRG --location WestUS 
		--template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
	```

- [Erstellen einer vollständigen Linux-Umgebung über die Azure-Befehlszeilenschnittstelle](virtual-machines-linux-create-cli-complete.md)
	- Beinhaltet das Erstellen eines Lastenausgleichs und mehrerer virtueller Computer in einer Verfügbarkeitsgruppe.

- [Hinzufügen eines Datenträgers zu einem virtuellen Linux-Computer](virtual-machines-linux-add-disk.md)
	- Das folgende Beispiel fügt einem vorhandenen virtuellen Computer namens `TestVM` einen 5-GB-Datenträger hinzu:

	```bash
	azure vm disk attach-new --resource-group TestRG --vm-name TestVM \
		--size-in-GB 5
	```

## Azure-Portal

Über das [Azure-Portal](https://portal.azure.com) können Sie schnell einen virtuellen Computer erstellen, da bei dieser Variante nichts auf Ihrem System installiert werden muss. Verwenden Sie das Azure-Portal, um den virtuellen Computer zu erstellen:

- [Erstellen einer Linux-VM mit dem Azure-Portal](virtual-machines-linux-quick-create-portal.md)
- [Anfügen eines Datenträgers mit dem Azure-Portal](virtual-machines-linux-attach-disk-portal.md)


## Betriebssystem und Image-Optionen
Beim Erstellen einer VM muss ein für das auszuführende Betriebssystem geeignetes Image gewählt werden. Azure und seine Partner bieten zahlreiche Images an – einige mit bereits vorinstallierten Anwendungen und Tools. Alternativ können Sie auch Ihre eigenen Images hochladen (wie [im folgenden Abschnitt](#use-your-own-image) beschrieben).

### Azure-Images
Mithilfe der CLI-Befehle vom Typ `azure vm image` können Sie verfügbare Elemente nach Herausgeber, Distributionsversion und Build anzeigen.

So listen Sie die verfügbaren Herausgeber auf:

```bash
azure vm image list-publishers --location WestUS
```

So listen Sie die verfügbaren Produkte (Angebote) für einen Herausgeber auf:

```bash
azure vm image list-offers --location WestUS --publisher Canonical
```

So listen Sie die verfügbaren SKUs (Distributionsversionen) eines Angebots auf:

```bash
azure vm image list-skus --location WestUS --publisher Canonical --offer UbuntuServer
```

So listen Sie alle verfügbaren Images für eine bestimmte Version auf:

```bash
azure vm image list --location WestUS --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS
```

Weitere Beispiele für das Durchsuchen und Verwenden von verfügbaren Images finden Sie unter [Auswählen von Linux-VM-Images mit der Azure-CLI](virtual-machines-linux-cli-ps-findimage.md).

Die Befehle `azure vm quick-create` und `azure vm create` verfügen über Aliase, die Sie zum schnellen Zugreifen auf die gängigeren Distributionen und ihre neuesten Versionen verwenden können. Das geht häufig schneller als bei jeder Erstellung eines virtuellen Computers den Herausgeber, das Angebot, die SKU und die Version anzugeben:

| Alias | Herausgeber | Angebot | SKU | Version |
|:----------|:----------|:-------------|:------------|:--------|
| CentOS | OpenLogic | Centos | 7\.2 | neueste |
| CoreOS | CoreOS | CoreOS | Stable | neueste |
| Debian | credativ | Debian | 8 | neueste |
| openSUSE | SUSE | openSUSE | 13\.2 | neueste |
| RHEL | Redhat | RHEL | 7\.2 | neueste |
| SLES | SLES | SLES | 12-SP1 | neueste |
| UbuntuLTS | Canonical | UbuntuServer | 14\.04.4-LTS | neueste |

### Verwenden eines eigenen Image

Sollten Sie spezielle Anpassungen benötigen, verwenden Sie ein Image, das auf einem vorhandenen virtuellen Azure-Computer basiert. Hierzu können Sie diesen virtuellen Computer *erfassen*. Sie können auch ein lokal erstelltes Image hochladen. Weitere Informationen zu unterstützten Distributionen und zur Verwendung eigener Images finden Sie in den folgenden Artikeln:

- [Von Azure unterstützte Distributionen](virtual-machines-linux-endorsed-distros.md)

- [Informationen zu nicht unterstützten Distributionen](virtual-machines-linux-create-upload-generic.md)

- [Erfassen eines virtuellen Linux-Computers zur Verwendung als Resource Manager-Vorlage](virtual-machines-linux-capture-image.md)
	- Beispielbefehle zum Erfassen eines vorhandenen virtuellen Computers:

	```bash
	azure vm deallocate --resource-group TestRG --vm-name TestVM
	azure vm generalize --resource-group TestRG --vm-name TestVM
	azure vm capture --resource-group TestRG --vm-name TestVM --vhd-name-prefix CapturedVM
	```

## Nächste Schritte

- Erstellen Sie einen virtuellen Linux-Computer über das [Portal](virtual-machines-linux-quick-create-portal.md), mit der [Befehlszeilenschnittstelle](virtual-machines-linux-quick-create-cli.md) oder mithilfe einer [Azure Resource Manager-Vorlage](virtual-machines-linux-cli-deploy-templates.md).

- Nach dem Erstellen eines virtuellen Linux-Computers können Sie [einen Datenträger hinzufügen](virtual-machines-linux-add-disk.md).

- Schritte zum schnellen Zurücksetzen von Kennwörtern oder SSH-Schlüsseln sowie zum Verwalten von Benutzern finden Sie [hier](virtual-machines-linux-using-vmaccess-extension.md).

<!---HONumber=AcomDC_0928_2016-->