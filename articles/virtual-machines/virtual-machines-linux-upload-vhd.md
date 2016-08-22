<properties
	pageTitle="Erstellen und Hochladen eines benutzerdefinierten Linux-Images | Microsoft Azure"
	description="Erstellen Sie eine virtuelle Festplatte (Virtual Hard Disk, VHD) mit einem benutzerdefinierten Linux-Image mithilfe des Resource Manager-Bereitstellungsmodells, und laden Sie sie in Azure hoch."
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
	ms.date="07/15/2016"
	ms.author="iainfou"/>

# Hochladen und Erstellen eines virtuellen Computers aus einem benutzerdefinierten Datenträgerimage

In diesem Artikel erfahren Sie, wie Sie eine virtuelle Festplatte (Virtual Hard Disk, VHD) mit dem Resource Manager-Bereitstellungsmodell hochladen und virtuelle Computer aus diesem benutzerdefinierten Image erstellen. Dadurch können Sie eine Linux-Distribution installieren und konfigurieren und die VHD dann zur schnellen Erstellung virtueller Azure-Computer (Azure-VMs) verwenden.

## Schnellbefehle
Vergewissern Sie sich, dass die [Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md) angemeldet ist und den Resource Manager-Modus (`azure config mode arm`) nutzt.

Erstellen Sie zunächst eine Ressourcengruppe:

```bash
azure group create TestRG --location "WestUS"
```

Erstellen Sie ein Speicherkonto für Ihre virtuellen Festplatten:

```bash
azure storage account create testuploadedstorage --resource-group TestRG \
	--location "WestUS" --kind Storage --sku-name PLRS
```

Führen Sie die Zugriffsschlüssel für das soeben erstellte Speicherkonto auf, und notieren Sie sich `key1`:

```bash
azure storage account keys list testuploadedstorage --resource-group TestRG
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

Nun können Sie auf der Grundlage der hochgeladenen virtuellen Festplatte [mithilfe einer Resource Manager-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) oder über die Befehlszeilenschnittstelle einen virtuellen Computer erstellen, indem Sie den URI zu Ihrem Datenträger angeben:

```bash
azure vm create TestVM -l "WestUS" --resource-group TestRG \
	-Q https://testuploadedstorage.blob.core.windows.net/vm-images/yourdisk.vhd
```

Beachten Sie, dass das Zielspeicherkonto mit dem Konto identisch sein muss, in das Sie den virtuellen Datenträger hochgeladen haben. Sie müssen außerdem alle erforderlichen Zusatzparameter für den `azure vm create`-Befehl angeben bzw. entsprechende Eingabeaufforderungen beantworten. Dazu gehören: virtuelles Netzwerk, öffentliche IP-Adresse, Benutzername, SSH-Schlüssel usw. Weitere Informationen zu den verfügbaren Resource Manager-Parametern für die Befehlszeilenschnittstelle finden Sie [hier](azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).


## Ausführliche Schritte
Sie müssen zum Vorbereiten Ihres benutzerdefinierten Linux-Images und zum Hochladen in Azure eine Reihe von Schritten ausführen. Der Rest dieses Artikels bietet ausführlichere Informationen zu den einzelnen Schritten, die weiter oben unter „Schnellbefehle“ aufgeführt sind.


## Anforderungen
Für die oben angegebenen Schritte benötigen Sie Folgendes:

- **In einer VHD-Datei installiertes Linux-Betriebssystem**: Installieren Sie auf einer virtuellen Festplatte im VHD-Format eine [von Azure unterstützte Linux-Distribution](virtual-machines-linux-endorsed-distros.md). (Informationen zu nicht unterstützten Distributionen finden Sie [hier](virtual-machines-linux-create-upload-generic.md).) Für die Erstellung virtueller Computer und Festplatten stehen verschiedene Tools zur Verfügung:
	- Installieren und konfigurieren Sie [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) oder [KVM](http://www.linux-kvm.org/page/RunningKVM), und verwenden Sie dabei „VHD“ als Imageformat. Bei Bedarf können Sie ein Image mithilfe von `qemu-img convert` [konvertieren](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats).
	- Unter [Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) und [Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx) können Sie auch Hyper-V verwenden.

> [AZURE.NOTE] Das modernere VHDX-Format wird in Azure noch nicht unterstützt. Wenn Sie einen virtuellen Computer erstellen, geben Sie als Format VHD an. VHDX-Datenträger können bei Bedarf mit [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) oder mit dem PowerShell-Cmdlet [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) in VHD konvertiert werden. Azure unterstützt außerdem das Hochladen von dynamischen VHDs nicht, daher müssen Sie solche Datenträger vor dem Hochladen in statische VHDs konvertieren. Mit Tools wie [Azure VHD Utilities for GO](https://github.com/Microsoft/azure-vhd-utils-for-go) (Azure-VHD-Hilfsprogramme für Go) können Sie dynamische Datenträger im Zuge des Uploads zu Azure konvertieren.

- Virtuelle Computer, die aus dem benutzerdefinierten Image erstellt werden, müssen sich im gleichen Speicherkonto wie das eigentliche Image befinden.
	- Erstellen Sie ein Speicherkonto und einen Container für Ihr benutzerdefiniertes Image und die erstellten virtuellen Computer.
	- Nachdem Sie Ihre virtuellen Computer erstellt haben, können Sie das Image problemlos löschen


<a id="prepimage"> </a>
## Vorbereiten des hochzuladenden Images

Microsoft Azure unterstützt eine Vielzahl von Linux-Distributionen (siehe [Unterstützte Distributionen](virtual-machines-linux-endorsed-distros.md)). Die folgenden Artikel führen Sie durch die Vorbereitung der verschiedenen Linux-Distributionen, die in Azure unterstützt werden:

- **[CentOS-basierte Verteilungen](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Oracle Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Red Hat Enterprise Linux](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES und openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**
- **[Sonstige – nicht unterstützte Distributionen](virtual-machines-linux-create-upload-generic.md)**

Beachten Sie auch die **[Installationshinweise für Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes)**. Diese enthalten allgemeine Tipps zur Vorbereitung von Linux-Images für Azure.

> [AZURE.NOTE] Die [Azure Platform-SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) gilt nur dann für virtuelle Computer unter Linux, wenn eine der unterstützten Distributionen mit Konfigurationsdetails verwendet wird, die im Abschnitt mit den unterstützten Versionen unter [Linux auf von Azure unterstützten Verteilungen](virtual-machines-linux-endorsed-distros.md) angegeben sind.


## Erstellen einer Ressourcengruppe
Ressourcengruppen verknüpfen logisch alle Azure-Ressourcen zur Unterstützung Ihrer virtuellen Computer, z.B. das virtuelle Netzwerk und den Speicher. Erfahren Sie [hier](../resource-group-overview.md) mehr über Azure-Ressourcengruppen. Vor dem Hochladen Ihres benutzerdefinierten Datenträgerimages und dem Erstellen virtueller Computer müssen Sie zuerst eine Ressourcengruppe erstellen:

```bash
azure group create TestRG --location "WestUS"
```

## Erstellen Sie ein Speicherkonto.
Virtuelle Computer werden als Seitenblobs in einem Speicherkonto gespeichert. Erfahren Sie [hier](../storage/storage-introduction.md#blob-storage) mehr über Azure Blob Storage. Sie müssen ein Speicherkonto für Ihr benutzerdefiniertes Datenträgerimage und die virtuellen Computer erstellen. Alle virtuellen Computer, die Sie aus Ihrem benutzerdefinierten Image erstellen, müssen sich im gleichen Speicherkonto wie das Image befinden.

Erstellen Sie ein Speicherkonto in der Ressourcengruppe, die Sie gerade erstellt haben:

```bash
azure storage account create testuploadedstorage --resource-group TestRG \
	--location "WestUS" --kind Storage --sku-name PLRS
```

## Auflisten von Speicherkontoschlüsseln
Azure generiert zwei 512-Bit-Zugriffsschlüssel für jedes Speicherkonto. Die Zugriffsschlüssel werden für die Authentifizierung beim Speicherkonto verwendet, um beispielsweise Schreibvorgänge auszuführen. Erfahren Sie [hier](../storage/storage-create-storage-account.md#manage-your-storage-account) mehr über das Verwalten von Speicherzugriff. Sie können Zugriffsschlüssel mit dem Befehl `azure storage account keys list` anzeigen.

Zeigen Sie die Zugriffsschlüssel für das soeben erstellte Speicherkonto an:

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
Notieren Sie sich `key1`, da Sie ihn in den nächsten Schritten für die Interaktion mit Ihrem Speicherkonto benötigen.

## Erstellen eines Speichercontainers
So wie Sie verschiedene Verzeichnisse zum logischen Organisieren Ihres lokalen Dateisystems erstellen, erstellen Sie Container in einem Speicherkonto, um die virtuellen Datenträger und Datenträgerimages zu organisieren. Ein Speicherkonto kann eine beliebige Anzahl von Containern enthalten.

Erstellen Sie einen neuen Container, und geben Sie dabei den Zugriffsschlüssel an, den Sie im vorherigen Schritt erhalten haben:

```bash
azure storage container create --account-name testuploadedstorage \
	--account-key <key1> --container vm-images
```

## Hochladen der VHD
Jetzt können Sie Ihr benutzerdefiniertes Datenträgerimage tatsächlich hochladen. Wie bei allen virtuellen Laufwerken, die von virtuellen Computern verwendet werden, laden Sie Ihr benutzerdefiniertes Datenträgerimage als Seitenblob hoch und speichern es so.

Sie müssen Ihren Zugriffsschlüssel, den Container, den Sie im vorherigen Schritt erstellt haben, und dann den Pfad zum benutzerdefinierten Datenträgerimage auf dem lokalen Computer angeben:

```bash
azure storage blob upload --blobtype page --account-name testuploadedstorage \
	--account-key <key1> --container vm-images /path/to/disk/yourdisk.vhd
```

## Erstellen von virtuellen Computern aus dem benutzerdefinierten Image
Wenn Sie virtuelle Computer aus Ihrem benutzerdefinierten Image erstellen, müssen Sie den URI für das Datenträgerimage angeben und sicherstellen, dass das Zielspeicherkonto dem Konto entspricht, in dem Ihr benutzerdefiniertes Datenträgerimage gespeichert ist. Sie können Ihren virtuellen Computer mithilfe der Azure-Befehlszeilenschnittstelle oder einer Resource Manager-JSON-Vorlage erstellen.


### Erstellen eines virtuellen Computers mit der Azure-Befehlszeilenschnittstelle
Sie geben den Parameter `--image-urn` (oder einfach `-Q`) mit dem Befehl `azure vm create` an, um auf Ihr benutzerdefiniertes Datenträgerimage zu verweisen. Stellen Sie sicher, dass `--storage-account-name` (oder `-o`) dem Speicherkonto entspricht, in dem Ihr benutzerdefiniertes Datenträgerimage gespeichert ist. Sie müssen nicht den gleichen Container wie für das benutzerdefinierte Datenträgerimage verwenden, um Ihren virtuellen Computer zu speichern. Achten Sie nur darauf, alle weiteren Container auf die gleiche Weise zu erstellen, wie in den Schritten weiter oben beschrieben, bevor Sie Ihre benutzerdefinierten Datenträgerimages hochladen.

Erstellen Sie einen virtuellen Computer aus Ihrem benutzerdefinierten Datenträgerimage:

```bash
azure vm create TestVM -l "WestUS" --resource-group TestRG \
	-Q https://testuploadedstorage.blob.core.windows.net/vm-images/yourdisk.vhd
	-o testuploadedstorage
```

Beachten Sie, dass Sie außerdem alle erforderlichen Zusatzparameter für den `azure vm create`-Befehl angeben bzw. entsprechende Eingabeaufforderungen beantworten müssen. Dazu gehören: virtuelles Netzwerk, öffentliche IP-Adresse, Benutzername, SSH-Schlüssel usw. Weitere Informationen zu den verfügbaren Resource Manager-Parametern für die Befehlszeilenschnittstelle finden Sie [hier](azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

### Erstellen eines virtuellen Computers mit einer JSON-Vorlage
Azure Resource Manager-Vorlagen sind JSON-Dateien (JavaScript Object Notation), die die Umgebung definieren, die Sie erstellen möchten. Die Vorlagen werden nach unterschiedlichen Ressourcenanbieter unterteilt, z.B. Compute oder Netzwerk. Sie können vorhandene Vorlagen verwenden oder eigene schreiben. Erfahren Sie [hier](../resource-group-overview.md) mehr über die Verwendung von Resource Manager und Vorlagen.

Im Anbieter `Microsoft.Compute/virtualMachines` der Vorlage muss ein `storageProfile`-Knoten vorhanden sein, der die Konfigurationsdetails für den virtuellen Computer enthält. Die beiden wichtigsten zu bearbeitenden Parameter sind die `image`- und `vhd`-URIs, die auf Ihr benutzerdefiniertes Datenträgerimage und die neue virtuelle Festplatte Ihres virtuellen Computers verweisen. Im Folgenden finden Sie ein JSON-Beispiel für die Verwendung eines benutzerdefinierten Datenträgerimages:

```bash
"storageProfile": {
          "osDisk": {
            "name": "TestVM",
            "osType": "Linux",
            "caching": "ReadWrite",
			"createOption": "FromImage",
            "image": {
              "uri": "https://testuploadedstorage.blob.core.windows.net/vm-images/yourdisk.vhd"
            },
            "vhd": {
              "uri": "https://testuploadedstorage.blob.core.windows.net/vhds/newvmname.vhd"
            }
          }
```

Sie können [diese vorhandene Vorlage zum Erstellen eines virtuellen Computers aus einem benutzerdefinierten Image](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) verwenden oder sich mit dem [Erstellen eigener Azure Resource Manager-Vorlagen](../resource-group-authoring-templates.md) beschäftigen.

Nachdem Sie eine Vorlage konfiguriert haben, erstellen Sie Ihre virtuellen Computer mit dem Befehl `azure group deployment create`. Geben Sie den URI der JSON-Vorlage mit dem Parameter `--template-uri` an:

```bash
azure group deployment create --resource-group TestTemplateRG
	--template-uri https://uri.to.template/yourtemplate.json
```

Wenn Sie eine JSON-Datei lokal auf Ihrem Computer gespeichert haben, können Sie stattdessen den Parameter `--template-file` verwenden:

```bash
azure group deployment create --resource-group TestTemplateRG
	--template-file /path/to/yourtemplate.json
```


## Nächste Schritte
Nachdem Sie die benutzerdefinierte virtuelle Festplatte vorbereitet und hochgeladen haben, können Sie sich mit der [Verwendung von Resource Manager und Vorlagen](../resource-group-overview.md) beschäftigen. Informationen zum Hinzufügen eines Datenträgers zu Ihren neuen virtuellen Computern finden Sie [hier](virtual-machines-linux-add-disk.md). Falls auf Ihren virtuellen Computern Anwendungen ausgeführt werden, auf die Sie zugreifen müssen, müssen Sie [Ports und Endpunkte öffnen](virtual-machines-linux-nsg-quickstart.md).

<!---HONumber=AcomDC_0810_2016-->