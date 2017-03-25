---
title: Hochladen eines benutzerdefinierten Linux-Images mit der Azure CLI 1.0 | Microsoft-Dokumentation
description: Erstellen Sie eine virtuelle Festplatte (Virtual Hard Disk, VHD) mit einem benutzerdefinierten Linux-Image mithilfe des Resource Manager-Bereitstellungsmodells und der Azure CLI 1.0, und laden Sie sie in Azure hoch.
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: a8c7818f-eb65-409e-aa91-ce5ae975c564
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/10/2016
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 97b5e4c3bda8d66297a43b4e430fec92d9588686
ms.lasthandoff: 03/21/2017


---
# <a name="upload-and-create-a-linux-vm-from-custom-disk-image-by-using-the-azure-cli-10"></a>Hochladen und Erstellen eines virtuellen Linux-Computers aus einem benutzerdefinierten Datenträgerimage mithilfe der Azure CLI 1.0
In diesem Artikel erfahren Sie, wie Sie eine virtuelle Festplatte (Virtual Hard Disk, VHD) mit dem Resource Manager-Bereitstellungsmodell in Azure hochladen und virtuelle Linux-Computer aus diesem benutzerdefinierten Image erstellen. Dadurch können Sie eine Linux-Distribution installieren und konfigurieren und die VHD dann zur schnellen Erstellung virtueller Azure-Computer (Azure-VMs) verwenden.


## <a name="cli-versions-to-complete-the-task"></a>CLI-Versionen zum Durchführen dieser Aufgabe
Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch:

- [Azure-CLI 1.0](#quick-commands): Unsere CLI für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell (in diesem Artikel)
- [Azure CLI 2.0:](virtual-machines-linux-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Unsere CLI der nächsten Generation für das Resource Manager-Bereitstellungsmodell


## <a name="quick-commands"></a>Schnellbefehle
Im folgenden Abschnitt werden die grundlegenden Befehlen zum Hochladen eines virtuellen Computers in Azure beschrieben, falls Sie die Aufgabe schnell durchführen müssen. Ausführlichere Informationen und Kontext für die einzelnen Schritte finden Sie im übrigen Dokument ([ab hier](#requirements)).

Vergewissern Sie sich, ob die [Azure-Befehlszeilenschnittstelle 1.0](../cli-install-nodejs.md) angemeldet ist und den Resource Manager-Modus nutzt:

```azurecli
azure config mode arm
```

Ersetzen Sie in den folgenden Beispielen die Beispielparameternamen durch Ihre eigenen Werte. Zu den Beispielparameternamen zählen `myResourceGroup`, `mystorageaccount` und `myimages`.

Erstellen Sie zunächst eine Ressourcengruppe. Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen `myResourceGroup` am Standort `WestUs`:

```azurecli
azure group create myResourceGroup --location "WestUS"
```

Erstellen Sie ein Speicherkonto für Ihre virtuellen Festplatten. Das folgende Beispiel erstellt ein Speicherkonto namens `mystorageaccount`:

```azurecli
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

Listet die Zugriffsschlüssel für das Speicherkonto auf. Notieren Sie sich `key1`:

```azurecli
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

Erstellen Sie innerhalb des Speicherkontos unter Verwendung des abgerufenen Speicherschlüssels einen Container. Das folgende Beispiel erstellt unter Verwendung des Speicherschlüssels `key1` einen Container namens `myimages`:

```azurecli
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

Laden Sie schließlich Ihre virtuelle Festplatte in den erstellten Container hoch. Geben Sie den lokalen Pfad zur virtuellen Festplatte unter `/path/to/disk/mydisk.vhd` an:

```azurecli
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

Jetzt können Sie aus Ihrer hochgeladenen virtuellen Festplatte [mithilfe einer Resource Manager-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd)einen virtuellen Computer erstellen. Sie können auch die Befehlszeilenschnittstelle verwenden, indem Sie den URI zu Ihrem Datenträger angeben (`--image-urn`). Das folgende Beispiel erstellt einen virtuellen Computer namens `myVM` und verwendet dabei den zuvor hochgeladenen Datenträger:

```azurecli
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
```

Das Zielspeicherkonto muss mit dem Konto identisch sein, in das Sie den virtuellen Datenträger hochgeladen haben. Sie müssen außerdem alle erforderlichen Zusatzparameter für den `azure vm create`-Befehl angeben bzw. entsprechende Eingabeaufforderungen beantworten. Dazu gehören: virtuelles Netzwerk, öffentliche IP-Adresse, Benutzername und SSH-Schlüssel. Erfahren Sie mehr zu den [verfügbaren Resource Manager-Parametern für die Befehlszeilenschnittstelle](azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

## <a name="requirements"></a>Anforderungen
Um die folgenden Schritte ausführen zu können, benötigen Sie Folgendes:

* **In einer VHD-Datei installiertes Linux-Betriebssystem:** Installieren Sie auf einer virtuellen Festplatte im VHD-Format eine [von Azure unterstützte Linux-Distribution](virtual-machines-linux-endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). (Informationen zu nicht unterstützten Distributionen finden Sie [hier](virtual-machines-linux-create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).) Für die Erstellung virtueller Computer und Festplatten stehen verschiedene Tools zur Verfügung:
  * Installieren und konfigurieren Sie [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) oder [KVM](http://www.linux-kvm.org/page/RunningKVM), und verwenden Sie dabei „VHD“ als Imageformat. Bei Bedarf können Sie ein Image mithilfe von `qemu-img convert` [konvertieren](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats).
  * Unter [Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) und [Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx) können Sie auch Hyper-V verwenden.

> [!NOTE]
> Das modernere VHDX-Format wird in Azure noch nicht unterstützt. Wenn Sie einen virtuellen Computer erstellen, geben Sie als Format VHD an. VHDX-Datenträger können bei Bedarf mit [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) oder mit dem PowerShell-Cmdlet [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) in VHD konvertiert werden. Azure unterstützt außerdem das Hochladen von dynamischen VHDs nicht, daher müssen Sie solche Datenträger vor dem Hochladen in statische VHDs konvertieren. Mit Tools wie [Azure VHD Utilities for GO](https://github.com/Microsoft/azure-vhd-utils-for-go) (Azure-VHD-Hilfsprogramme für Go) können Sie dynamische Datenträger im Zuge des Uploads zu Azure konvertieren.
> 
> 

* Virtuelle Computer, die aus dem benutzerdefinierten Image erstellt werden, müssen sich im gleichen Speicherkonto wie das eigentliche Image befinden.
  * Erstellen Sie ein Speicherkonto und einen Container für Ihr benutzerdefiniertes Image und die erstellten virtuellen Computer.
  * Nachdem Sie Ihre virtuellen Computer erstellt haben, können Sie das Image problemlos löschen

Vergewissern Sie sich, ob die [Azure-Befehlszeilenschnittstelle 1.0](../cli-install-nodejs.md) angemeldet ist und den Resource Manager-Modus nutzt:

```azurecli
azure config mode arm
```

Ersetzen Sie in den folgenden Beispielen die Beispielparameternamen durch Ihre eigenen Werte. Zu den Beispielparameternamen zählen `myResourceGroup`, `mystorageaccount` und `myimages`.

<a id="prepimage"> </a>

## <a name="prepare-the-image-to-be-uploaded"></a>Vorbereiten des hochzuladenden Images
Azure unterstützt eine Vielzahl von Linux-Distributionen (siehe [Unterstützte Distributionen](virtual-machines-linux-endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Die folgenden Artikel führen Sie durch die Vorbereitung der verschiedenen Linux-Distributionen, die in Azure unterstützt werden:

* **[CentOS-basierte Verteilungen](virtual-machines-linux-create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Oracle Linux](virtual-machines-linux-oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Red Hat Enterprise Linux](virtual-machines-linux-redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[SLES und openSUSE](virtual-machines-linux-suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Sonstige – nicht unterstützte Distributionen](virtual-machines-linux-create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**

Beachten Sie auch die **[Installationshinweise für Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes)**. Diese enthalten allgemeine Tipps zur Vorbereitung von Linux-Images für Azure.

> [!NOTE]
> Die [Azure Platform-SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) gilt nur dann für virtuelle Computer unter Linux, wenn eine der unterstützten Distributionen mit Konfigurationsdetails verwendet wird, die im Abschnitt mit den unterstützten Versionen unter [Linux auf von Azure unterstützten Verteilungen](virtual-machines-linux-endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) angegeben sind.


## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Ressourcengruppen verknüpfen logisch alle Azure-Ressourcen zur Unterstützung Ihrer virtuellen Computer, z.B. das virtuelle Netzwerk und den Speicher. Erfahren Sie [hier](../azure-resource-manager/resource-group-overview.md) mehr über Azure-Ressourcengruppen. Vor dem Hochladen Ihres benutzerdefinierten Datenträgerimages und dem Erstellen virtueller Computer müssen Sie zuerst eine Ressourcengruppe erstellen. 

Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen `myResourceGroup` am Standort `WestUS`:

```azurecli
azure group create myResourceGroup --location "WestUS"
```

## <a name="create-a-storage-account"></a>Erstellen Sie ein Speicherkonto.
Virtuelle Computer werden als Seitenblobs in einem Speicherkonto gespeichert. Erfahren Sie [hier](../storage/storage-introduction.md#blob-storage)mehr über Azure Blob Storage. Sie erstellen ein Speicherkonto für Ihr benutzerdefiniertes Datenträgerimage und die virtuellen Computer. Alle virtuellen Computer, die Sie aus Ihrem benutzerdefinierten Image erstellen, müssen sich im gleichen Speicherkonto wie das Image befinden.

Das folgende Beispiel erstellt ein Speicherkonto namens `mystorageaccount` in der zuvor erstellten Ressourcengruppe:

```azurecli
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

## <a name="list-storage-account-keys"></a>Auflisten von Speicherkontoschlüsseln
Azure generiert zwei 512-Bit-Zugriffsschlüssel für jedes Speicherkonto. Die Zugriffsschlüssel werden für die Authentifizierung beim Speicherkonto verwendet, um beispielsweise Schreibvorgänge auszuführen. Erfahren Sie [hier](../storage/storage-create-storage-account.md#manage-your-storage-account)mehr über das Verwalten von Speicherzugriff. Sie können Zugriffsschlüssel mit dem Befehl `azure storage account keys list` anzeigen.

Zeigen Sie die Zugriffsschlüssel für das erstellte Speicherkonto an:

```azurecli
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

Die Ausgabe sieht in etwa wie folgt aus:

```azurecli
info:    Executing command storage account keys list
+ Getting storage account keys
data:    Name  Key                                                                                       Permissions
data:    ----  ----------------------------------------------------------------------------------------  -----------
data:    key1  d4XAvZzlGAgWdvhlWfkZ9q4k9bYZkXkuPCJ15NTsQOeDeowCDAdB80r9zA/tUINApdSGQ94H9zkszYyxpe8erw==  Full
data:    key2  Ww0T7g4UyYLaBnLYcxIOTVziGAAHvU+wpwuPvK4ZG0CDFwu/mAxS/YYvAQGHocq1w7/3HcalbnfxtFdqoXOw8g==  Full
info:    storage account keys list command OK

```
Notieren Sie sich `key1` , da Sie ihn in den nächsten Schritten für die Interaktion mit Ihrem Speicherkonto benötigen.

## <a name="create-a-storage-container"></a>Erstellen eines Speichercontainers
Auf die gleiche Weise, in der Sie verschiedene Verzeichnisse erstellen, um Ihr lokales Dateisystem logisch zu organisieren, erstellen Sie Container in einem Speicherkonto, um die virtuellen Datenträger und Images zu organisieren. Ein Speicherkonto kann eine beliebige Anzahl von Containern enthalten. 

Das folgende Beispiel erstellt einen neuen Container namens `myimages`. Dabei wird der im vorherigen Schritt erhaltene Zugriffsschlüssel angegeben (`key1`):

```azurecli
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

## <a name="upload-vhd"></a>Hochladen der VHD
Jetzt können Sie Ihr benutzerdefiniertes Datenträgerimage tatsächlich hochladen. Wie bei allen virtuellen Laufwerken, die von virtuellen Computern verwendet werden, laden Sie Ihr benutzerdefiniertes Datenträgerimage als Seitenblob hoch und speichern es so.

Geben Sie Ihren Zugriffsschlüssel, den Container, den Sie im vorherigen Schritt erstellt haben, und dann den Pfad zum benutzerdefinierten Datenträgerimage auf dem lokalen Computer an:

```azurecli
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

## <a name="create-vm-from-custom-image"></a>Erstellen von virtuellen Computern aus dem benutzerdefinierten Image
Wenn Sie virtuelle Computer aus Ihrem benutzerdefinierten Datenträgerimage erstellen, geben Sie den URI zum Datenträgerimage an. Stellen Sie sicher, dass das Zielspeicherkonto dem Speicherkonto entspricht, in dem Ihr benutzerdefiniertes Datenträgerimage gespeichert ist. Sie können Ihren virtuellen Computer mithilfe der Azure-Befehlszeilenschnittstelle oder einer Resource Manager-JSON-Vorlage erstellen.

### <a name="create-a-vm-using-the-azure-cli"></a>Erstellen eines virtuellen Computers mit der Azure-Befehlszeilenschnittstelle
Sie geben den Parameter `--image-urn` mit dem Befehl `azure vm create` an, um auf Ihr benutzerdefiniertes Datenträgerimage zu verweisen. Stellen Sie sicher, dass `--storage-account-name` dem Speicherkonto entspricht, in dem Ihr benutzerdefiniertes Datenträgerimage gespeichert ist. Sie müssen nicht den gleichen Container verwenden wie das benutzerdefinierte Datenträgerimage, um Ihre virtuellen Computer zu speichern. Stellen Sie sicher, dass Sie jegliche weiteren Container auf die gleiche Weise erstellen, wie in den vorherigen Schritten beschrieben, bevor Sie Ihre benutzerdefinierten Datenträgerimages hochladen.

Das folgende Beispiel erstellt einen virtuellen Computer mit dem Namen `myVM` aus Ihrem benutzerdefinierten Datenträgerimage:

```azurecli
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
    --storage-account-name mystorageaccount
```

Sie müssen alle erforderlichen Zusatzparameter für den `azure vm create`-Befehl angeben bzw. entsprechende Eingabeaufforderungen beantworten. Dazu gehören: virtuelles Netzwerk, öffentliche IP-Adresse, Benutzername und SSH-Schlüssel. Erfahren Sie mehr zu den [verfügbaren Resource Manager-Parametern für die Befehlszeilenschnittstelle](azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

### <a name="create-a-vm-using-a-json-template"></a>Erstellen eines virtuellen Computers mit einer JSON-Vorlage
Azure Resource Manager-Vorlagen sind JSON-Dateien (JavaScript Object Notation), die die Umgebung definieren, die Sie erstellen möchten. Die Vorlagen werden nach unterschiedlichen Ressourcenanbieter unterteilt, z.B. Compute oder Netzwerk. Sie können vorhandene Vorlagen verwenden oder eigene schreiben. Erfahren Sie [Verwendung von Resource Manager und Vorlagen](../azure-resource-manager/resource-group-overview.md)einen virtuellen Computer erstellen.

Im Anbieter `Microsoft.Compute/virtualMachines` der Vorlage ist ein `storageProfile`-Knoten vorhanden, der die Konfigurationsdetails für den virtuellen Computer enthält. Die beiden wichtigsten zu bearbeitenden Parameter sind die `image`- und `vhd`-URIs, die auf Ihr benutzerdefiniertes Datenträgerimage und die neue virtuelle Festplatte Ihres virtuellen Computers verweisen. Im Folgenden finden Sie ein JSON-Beispiel für die Verwendung eines benutzerdefinierten Datenträgerimages:

```json
"storageProfile": {
          "osDisk": {
            "name": "myVM",
            "osType": "Linux",
            "caching": "ReadWrite",
            "createOption": "FromImage",
            "image": {
              "uri": "https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd"
            },
            "vhd": {
              "uri": "https://mystorageaccount.blob.core.windows.net/vhds/newvmname.vhd"
            }
          }
```

Sie können [diese vorhandene Vorlage zum Erstellen eines virtuellen Computers aus einem benutzerdefinierten Image](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) verwenden oder sich mit dem [Erstellen eigener Azure Resource Manager-Vorlagen](../resource-group-authoring-templates.md) beschäftigen. 

Nachdem Sie eine Vorlage konfiguriert haben, erstellen Sie Ihre virtuellen Computer mit dem Befehl `azure group deployment create` . Geben Sie den URI der JSON-Vorlage mit dem Parameter `--template-uri` an:

```azurecli
azure group deployment create --resource-group myResourceGroup
    --template-uri https://uri.to.template/mytemplate.json
```

Wenn Sie eine JSON-Datei lokal auf Ihrem Computer gespeichert haben, können Sie stattdessen den Parameter `--template-file` verwenden:

```azurecli
azure group deployment create --resource-group myResourceGroup
    --template-file /path/to/mytemplate.json
```


## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie den benutzerdefinierten virtuellen Datenträger vorbereitet und hochgeladen haben, können Sie sich mit der [Verwendung von Resource Manager und Vorlagen](../azure-resource-manager/resource-group-overview.md)beschäftigen. Informationen zum Hinzufügen eines Datenträgers zu Ihren neuen virtuellen Computern finden Sie [hier](virtual-machines-linux-add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) . Falls auf Ihren virtuellen Computern Anwendungen ausgeführt werden, auf die Sie zugreifen müssen, müssen Sie [Ports und Endpunkte öffnen](virtual-machines-linux-nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


