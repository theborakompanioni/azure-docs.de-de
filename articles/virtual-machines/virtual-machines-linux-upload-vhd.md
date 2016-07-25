<properties
	pageTitle="Erstellen und Hochladen einer Linux-VHD | Microsoft Azure"
	description="Erstellen Sie eine virtuelle Festplatte (Virtual Hard Disk, VHD) mit dem Resource Manager-Bereitstellungsmodell, und laden Sie sie an Azure hoch."
	services="virtual-machines-linux"
	documentationCenter=""
	authors="iainfoulds"
	manager="timlt"
	editor="tysonn"
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-linux"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-linux"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/07/2016"
	ms.author="iainfou"/>

# Hochladen einer virtuellen Festplatte

In diesem Artikel erfahren Sie, wie Sie eine virtuelle Festplatte (Virtual Hard Disk, VHD) mit dem Resource Manager-Bereitstellungsmodell hochladen. Dadurch können Sie eine Linux-Distribution installieren und konfigurieren und die VHD dann zur schnellen Erstellung virtueller Azure-Computer (Azure-VMs) verwenden.

## Schnellbefehle
Vergewissern Sie sich, dass die [Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md) angemeldet ist und den Resource Manager-Modus (`azure config mode arm`) verwendet.

Erstellen Sie zunächst eine Ressourcengruppe:

```bash
azure group create TestRG --location "WestUS"
```

Erstellen Sie ein Speicherkonto für Ihre virtuellen Festplatten:

```bash
azure storage account create testuploadedstorage --resource-group TestRG \
	--location "WestUS" --kind Storage --sku-name LRS
```

Führen Sie die Speicherschlüssel für das soeben erstellte Speicherkonto auf, und notieren Sie sich `key1`:

```bash
azure storage account keys list testuploadedstorage --resource-group TestRG
```

Die Ausgabe ähnelt der folgenden:

```
info:    Executing command storage account keys list
+ Getting storage account keys
data:    Name  Key                                                                                       Permissions
data:    ----  ----------------------------------------------------------------------------------------  -----------
data:    key1  d4XAvZzlGAgWdvhlWfkZ9q4k9bYZkXkuPCJ15NTsQOeDeowCDAdB80r9zA/tUINApdSGQ94H9zkszYyxpe8erw==  Full
data:    key2  Ww0T7g4UyYLaBnLYcxIOTVziGAAHvU+wpwuPvK4ZG0CDFwu/mAxS/YYvAQGHocq1w7/3HcalbnfxtFdqoXOw8g==  Full
info:    storage account keys list command OK
```

Erstellen Sie innerhalb des Speicherkontos unter Verwendung des soeben abgerufenen Speicherschlüssels einen Container:

```bash
azure storage container create --account-name testuploadedstorage \
	--account-key <key1> --container vm-images
```

Laden Sie schließlich Ihre virtuelle Festplatte in den soeben erstellten Container hoch:

```bash
azure storage blob upload --blobtype page --account-name testuploadedstorage \
	--account-key <key1> --container vm-images /path/to/disk/yourdisk.vhd
```

Nun können Sie auf der Grundlage der hochgeladenen virtuellen Festplatte [mithilfe einer Resource Manager-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) einen virtuellen Computer erstellen. Alternativ können Sie auch die Befehlszeilenschnittstelle verwenden, indem Sie den URI zu Ihrem Datenträger angeben:

```bash
azure vm create TestVM -l "WestUS" --resource-group TestRG \
	-Q https://testuploadedstorage.blob.core.windows.net/vm-images/yourdisk.vhd
```

Hinweis: Sie benötigen weiterhin alle erforderlichen Zusatzparameter für den `azure vm create`-Befehl (virtuelles Netzwerk, öffentliche IP-Adresse, Benutzername, SSH-Schlüssel usw.). Weitere Informationen zu den verfügbaren Resource Manager-Parametern für die Befehlszeilenschnittstelle finden Sie [hier](azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

## Anforderungen
Für die oben angegebenen Schritte benötigen Sie Folgendes:

- **In einer VHD-Datei installiertes Linux-Betriebssystem**: Installieren Sie auf einer virtuellen Festplatte im VHD-Format eine [von Azure unterstützte Linux-Distribution](virtual-machines-linux-endorsed-distros.md). (Informationen zu nicht unterstützten Distributionen finden Sie [hier](virtual-machines-linux-create-upload-generic.md).) Für die Erstellung virtueller Computer und Festplatten stehen verschiedene Tools zur Verfügung:
	- Installieren und konfigurieren Sie [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) oder [KVM](http://www.linux-kvm.org/page/RunningKVM), und verwenden Sie dabei „VHD“ als Imageformat. Bei Bedarf können Sie ein Image mithilfe von `qemu-img convert` [konvertieren](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats).
	- Unter [Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) und [Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx) können Sie auch Hyper-V verwenden.

> [AZURE.NOTE] Das modernere VHDX-Format wird in Azure noch nicht unterstützt. Geben Sie beim Erstellen eines virtuellen Computers das ursprüngliche VHD-Format an. VHDX-Datenträger können bei Bedarf mit [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) oder mit dem PowerShell-Cmdlet [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) in das VHD-Format konvertiert werden. Azure unterstützt außerdem das Hochladen von dynamischen VHDs nicht, daher müssen Sie solche Datenträger vor dem Hochladen in statische VHDs konvertieren. Mit Tools wie [Azure VHD Utilities for GO](https://github.com/Microsoft/azure-vhd-utils-for-go) (Azure-VHD-Hilfsprogramme für Go) können Sie dynamische Datenträger im Zuge des Uploads zu Azure konvertieren.


<a id="prepimage"> </a>
## Vorbereiten des hochzuladenden Images

Microsoft Azure unterstützt eine Vielzahl von Linux-Distributionen (siehe [Unterstützte Distributionen](virtual-machines-linux-endorsed-distros.md)). Die folgenden Artikel führen Sie durch die Vorbereitung der verschiedenen Linux-Distributionen, die in Azure unterstützt werden. Der virtuelle Datenträger muss vor dem Hochladen entsprechend vorbereitet werden:

- **[CentOS-basierte Verteilungen](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Oracle Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Red Hat Enterprise Linux](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES und openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**
- **[Sonstige – nicht unterstützte Distributionen](virtual-machines-linux-create-upload-generic.md)**

Beachten Sie auch die **[Installationshinweise für Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes)**. Diese enthalten allgemeine Tipps zur Vorbereitung von Linux-Images für Azure.

> [AZURE.NOTE] Das [Azure-Plattform-SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) gilt nur dann für virtuelle Computer unter Linux, wenn eine unterstützte Distribution mit Konfigurationsdetails verwendet wird, die unter [Linux auf von Azure unterstützten Verteilungen](virtual-machines-linux-endorsed-distros.md) angegeben sind.

## Nächste Schritte
Nachdem Sie die benutzerdefinierte virtuelle Festplatte vorbereitet und hochgeladen haben, können Sie sich mit der [Verwendung von Resource Manager und Vorlagen](../resource-group-overview.md) beschäftigen. Informationen zum Hinzufügen eines Datenträgers zu Ihren neuen virtuellen Computern finden Sie [hier](virtual-machines-linux-add-disk.md). Falls auf Ihren virtuellen Computern Anwendungen ausgeführt werden, auf die Sie zugreifen müssen, informieren Sie sich über das [Öffnen von Ports und Endpunkten](virtual-machines-linux-nsg-quickstart.md).

<!---HONumber=AcomDC_0713_2016-->