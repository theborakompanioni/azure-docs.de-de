---
title: "Verwalten von Azure-Datenträgern mit der Azure-CLI | Microsoft-Dokumentation"
description: "Tutorial: Verwalten von Azure-Datenträgern mit der Azure-CLI"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/25/2017
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: 1cc1ee946d8eb2214fd05701b495bbce6d471a49
ms.openlocfilehash: 3e47c917774245f8b321b5cd94def24b7f523a94
ms.lasthandoff: 04/26/2017

---

# <a name="manage-azure-disks-with-the-azure-cli"></a>Verwalten von Azure-Datenträgern mit der Azure-CLI

Dieses Tutorial behandelt die verschiedenen Typen von VM-Datenträgern, das Auswählen einer Datenträgerkonfiguration sowie das Erstellen von Datenträgern und deren Anfügen an virtuelle Azure-Computer. Darüber hinaus wird in diesem Tutorial erläutert, wie Datenträger-Momentaufnahmen erstellt werden. 

Die Schritte in diesem Tutorial können mit der neuesten Version von [Azure CLI 2.0](/cli/azure/install-azure-cli) ausgeführt werden.

## <a name="default-azure-disks"></a>Azure-Standarddatenträger

Beim Erstellen eines virtuellen Azure-Computers werden dem virtuellen Computer automatisch zwei Datenträger angefügt. 

**Betriebssystem-Datenträger**: Betriebssystem-Datenträger können in der Größe auf bis zu 1TB angepasst werden und hosten das Betriebssystem des virtuellen Computers. Der Betriebssystem-Datenträger wird standardmäßig mit `/dev/sda` bezeichnet. Die Konfiguration der Datenträgerzwischenspeicherung des Betriebssystem-Datenträgers ist für die Leistung des Betriebssystems optimiert. Aufgrund dieser Konfiguration sollte der Betriebssystem-Datenträger **nicht** Anwendungen oder Daten hosten. Verwenden Sie für Anwendungen und Daten einen Datenträger. Dies wird weiter unten in diesem Artikel ausführlich erläutert. 

**Temporärer Datenträger**: Temporäre Datenträger verwenden ein Solid State Drive, das sich auf dem gleichen Azure-Host wie der virtuelle Computer befindet. Temporäre Datenträger sind äußerst leistungsfähig und können für Vorgänge wie die temporäre Datenverarbeitung verwendet werden. Wenn der virtuelle Computer jedoch auf einen neuen Host verschoben wird, werden alle auf einem temporären Datenträger gespeicherten Daten entfernt. Die Größe des temporären Datenträgers richtet sich nach der Größe des virtuellen Computers. Temporäre Datenträger werden mit bezeichnet `/dev/sdb` und haben einen Bereitstellungspunkt `/mnt`.

### <a name="temporary-disk-sizes"></a>Größe von temporären Datenträgern

| Typ | Größe des virtuellen Computers | Max. Größe des temporären Datenträgers |
|----|----|----|
| [Allgemeiner Zweck](sizes-general.md) | A- und D-Serie | 800 |
| [Computeoptimiert](sizes-compute.md) | F-Serie | 800 |
| [Arbeitsspeicheroptimiert](../virtual-machines-windows-sizes-memory.md) | D- und G-Serie | 6.144 |
| [Speicheroptimiert](../virtual-machines-windows-sizes-storage.md) | L-Serie | 5.630 |
| [GPU](sizes-gpu.md) | N-Serie | 1.440 |
| [Hohe Leistung](sizes-hpc.md) | A- und H-Serie | 2000 |

## <a name="azure-data-disks"></a>Azure-Datenträger

Zum Installieren von Anwendungen und zum Speichern von Daten können weitere Datenträger hinzugefügt werden. Datenträger sollten in allen Fällen verwendet werden, in denen eine dauerhafte und dynamische Datenspeicherung erwünscht ist. Jeder Datenträger hat eine maximale Kapazität von 1TB. Die Größe eines virtuellen Computers bestimmt die Anzahl der Datenträger, die dem virtuellen Computer angefügt werden können. Für jeden Kernspeicher eines virtuellen Computers können zwei Datenträger angefügt werden. 

### <a name="max-data-disks-per-vm"></a>Max. Anzahl der Datenträger pro virtuellem Computer

| Typ | Größe des virtuellen Computers | Max. Anzahl der Datenträger pro virtuellem Computer |
|----|----|----|
| [Allgemeiner Zweck](sizes-general.md) | A- und D-Serie | 32 |
| [Computeoptimiert](sizes-compute.md) | F-Serie | 32 |
| [Arbeitsspeicheroptimiert](../virtual-machines-windows-sizes-memory.md) | D- und G-Serie | 64 |
| [Speicheroptimiert](../virtual-machines-windows-sizes-storage.md) | L-Serie | 64 |
| [GPU](sizes-gpu.md) | N-Serie | 48 |
| [Hohe Leistung](sizes-hpc.md) | A- und H-Serie | 32 |

## <a name="vm-disk-types"></a>VM-Datenträgertypen

In Azure stehen zwei verschiedene Datenträgertypen zur Verfügung.

### <a name="standard-disk"></a>Standarddatenträger

Standardspeicher basiert auf Festplatten und stellt eine kostengünstige, performante Speicherlösung dar. Standarddatenträger sind ideal für eine kostengünstige Entwicklungs- und Testworkload.

### <a name="premium-disk"></a>Premium-Datenträger

Premium-Datenträger zeichnen sich durch SSD-basierte hohe Leistung und geringe Wartezeit aus. Sie eignen sich hervorragend für virtuelle Computer, auf denen die Produktionsworkload ausgeführt wird. Storage Premium unterstützt virtuelle Computer der DS-, DSv2-, GS- und FS-Serie. Premium-Datenträger gibt es in drei Varianten (P10 P20, P30). Der Datenträgertyp wird durch die Größe des Datenträgers vorgegeben. Bei der Auswahl einer Datenträgergröße wird der Wert auf den nächsten Datenträgertyp aufgerundet. Liegt die Größe des Datenträgers z.B. unter 128GB, ist der Datenträgertyp P10. Wenn die Größe des Datenträgers zwischen 129 und 512GB liegt, ist die Größe P20. Bei allem über 512GB ist die Größe P30.

### <a name="premium-disk-performance"></a>Leistung von Premium-Datenträgern

|Datenträgertyp des Premium-Speichers | P10 | P20 | P30 |
| --- | --- | --- | --- |
| Datenträgergröße (aufgerundet) | 128 GB | 512 GB | 1.024GB (1TB) |
| Max. IOPS pro Datenträger | 500 | 2.300 | 5.000 |
Durchsatz pro Datenträger | 100 MB/s | 150 MB/s | 200 MB/s |

In dieser Tabelle ist zwar die maximale IOPS-Anzahl pro Datenträger angegeben, eine höhere Leistung kann aber durch Striping mehrerer Datenträger erreicht werden. Eine Standard_GS5-VM kann z.B. ein Maximum von 80.000IOPS erreichen. Ausführliche Informationen zur maximalen IOPS-Anzahl pro virtuellem Computer finden Sie unter [Größen für virtuelle Linux-Computer](sizes.md).

## <a name="create-and-attach-disks"></a>Erstellen und Anfügen von Datenträgern

Datenträger für Daten können zum Zeitpunkt der VM-Erstellung erstellt und angefügt oder später erstellt und einer vorhandenen VM angefügt werden.

### <a name="attach-disk-at-vm-creation"></a>Anfügen eines Datenträgers bei der VM-Erstellung

Erstellen Sie mit dem Befehl [az group create](https://docs.microsoft.com/cli/azure/group#create) eine Ressourcengruppe. 

```azurecli
az group create --name myResourceGroupDisk --location westus
```

Erstellen Sie mit dem Befehl [az vm create]( /cli/azure/vm#create) einen virtuellen Computer. Das `--datadisk-sizes-gb`-Argument gibt an, dass ein weiterer Datenträger erstellt und dem virtuellen Computer angefügt werden sollte. Verwenden Sie zum Erstellen und Anfügen mehrerer Datenträger eine durch Leerzeichen getrennte Liste der Datenträger-Größenwerte. Im folgenden Beispiel wird ein virtueller Computer mit zwei Datenträgern von jeweils 128GB erstellt. Da die Größe der Datenträger jeweils 128GB beträgt, werden beide Datenträger als P10 konfiguriert, was maximal 500IOPS pro Datenträger bereitstellt.

```azurecli
az vm create \
  --resource-group myResourceGroupDisk \
  --name myVM \
  --image UbuntuLTS \
  --size Standard_DS2_v2 \
  --data-disk-sizes-gb 128 128 \
  --generate-ssh-keys
```

### <a name="attach-disk-to-existing-vm"></a>Anfügen eines Datenträgers an einen vorhandenen virtuellen Computer

Verwenden Sie zum Erstellen eines neuen Datenträgers und dessen Anfügen an einen vorhandenen virtuellen Computer den [az vm disk attach](/cli/azure/vm/disk#attach)-Befehl. Im folgenden Beispiel wird ein Premium-Datenträger von 128GB erstellt und dem im letzten Schritt erstellten virtuellen Computer angefügt.

```azurecli
az vm disk attach --vm-name myVM --resource-group myResourceGroupDisk --disk myDataDisk --size-gb 128 --sku Premium_LRS --new 
```

## <a name="prepare-data-disks"></a>Vorbereiten von Datenträgern

Nach dem Anfügen eines Datenträgers an den virtuellen Computer muss das Betriebssystem zur Verwendung des Datenträgers konfiguriert werden. Das folgende Beispiel zeigt das manuelle Konfigurieren eines Datenträgers. Dieser Prozess kann auch mit cloud-init automatisiert werden, was in einem [späteren Tutorial](./tutorial-automate-vm-deployment.md) veranschaulicht wird.

### <a name="manual-configuration"></a>Manuelle Konfiguration

Stellen Sie eine SSH-Verbindung mit dem virtuellen Computer her. Ersetzen Sie die IP-Beispieladresse durch die öffentliche IP-Adresse des virtuellen Computers.

```azurecli
ssh 52.174.34.95
```

Partitionieren Sie den Datenträger mit `fdisk`.

```bash
(echo n; echo p; echo 1; echo ; echo ; echo w) | sudo fdisk /dev/sdc
```

Schreiben Sie mit dem Befehl `mkfs` ein Dateisystem auf die Partition:

```bash
sudo mkfs -t ext4 /dev/sdc1
```

Binden Sie den neuen Datenträger ein, damit im Betriebssystem darauf zugegriffen werden kann.

```bash
sudo mkdir /datadrive && sudo mount /dev/sdc1 /datadrive
```

Auf den Datenträger kann jetzt über den Bereitstellungspunkt `datadrive` zugegriffen werden. Dies kann mit Ausführung des `df -h`-Befehls überprüft werden. 

```bash
df -h
```

Die Ausgabe zeigt das neue, auf `/datadrive` bereitgestellte Laufwerk.

```bash
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        30G  1.4G   28G   5% /
/dev/sdb1       6.8G   16M  6.4G   1% /mnt
/dev/sdc1        50G   52M   47G   1% /datadrive
```

Um sicherzustellen, dass das Laufwerk nach einem Neustart automatisch wieder eingebunden wird, muss es der Datei `/stc/fstab` hinzugefügt werden. Rufen Sie hierzu die UUID des Datenträgers mit dem `blkid`-Hilfsprogramm ab.

```bash
sudo -i blkid
```

Die Ausgabe zeigt die UUID des Laufwerks an, in diesem Fall `/dev/sdc1`.

```bash
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

Fügen Sie der `/etc/fstab`-Datei eine Zeile ähnlich der folgenden hinzu. Beachten Sie außerdem, dass Schreibbarrieren mit `barrier=0` deaktiviert werden können – diese Konfiguration kann die Datenträgerleistung verbessern. 

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive  ext4    defaults,nofail,barrier=0   1  2
```

Jetzt ist der Datenträger konfiguriert, und Sie können die SSH-Sitzung schließen.

```bash
exit
```

## <a name="resize-vm-disk"></a>Vergrößern des VM-Datenträgers

Sobald ein virtueller Computer bereitgestellt ist, können der Betriebssystem-Datenträger bzw. sämtliche angefügten Datenträger vergrößert werden. Die Vergrößerung eines Datenträgers ist nützlich, wenn mehr Speicherplatz oder ein höheres Maß an Leistung (P10, P20, P30) benötigt wird. Beachten Sie, dass Datenträger nicht verkleinert werden können.

Zum Vergrößern eines Datenträgers wird die ID oder der Name des Datenträgers benötigt. Verwenden Sie den [az disk list](/cli/azure/vm/disk#list)-Befehl, um alle Datenträger in einer Ressourcengruppe zurückzugeben. Notieren Sie den Namen des Datenträgers, den Sie vergrößern möchten.

```azurecli
 az disk list -g myResourceGroupDisk --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' --output table
```

Der virtuelle Computer muss auch freigegeben werden. Verwenden Sie den Befehl [az vm deallocate]( /cli/azure/vm#deallocate), um den virtuellen Computer zu beenden und ihn freizugeben.

```azurecli
az vm deallocate --resource-group myResourceGroupDisk --name myVM
```

Verwenden Sie den [az disk update](/cli/azure/vm/disk#update)-Befehl, um den Datenträger zu vergrößern. In diesem Beispiel wird ein Datenträger mit dem Namen `myDataDisk` auf 1Terabyte vergrößert.

```azurecli
az disk update --name myDataDisk --resource-group myResourceGroupDisk --size-gb 1023
```

Starten Sie den virtuellen Computer nach Abschluss der Vergrößerung.

```azurecli
az vm start --resource-group myResourceGroupDisk --name myVM
```

Wenn Sie den Betriebssystem-Datenträger vergrößert haben, wird die Partition automatisch erweitert. Wenn Sie einen Datenträger vergrößert haben, müssen alle aktuellen Partitionen im Betriebssystem der VM erweitert werden.

## <a name="snapshot-azure-disks"></a>Momentaufnahme von Azure-Datenträgern

Bei einer Momentaufnahme des Datenträger wird eine schreibgeschützte Point-in-Time-Kopie des Datenträgers erstellt. Mit Azure-VM-Momentaufnahmen können Sie schnell den Status eines virtuellen Computers speichern, bevor Sie Änderungen an der Konfiguration vornehmen. Falls sich die Konfigurationsänderungen als unvorteilhaft herausstellen, kann der Status des virtuellen Computers mit der Momentaufnahme wiederhergestellt werden. Wenn ein virtueller Computer über mehrere Datenträger verfügt, wird von jedem einzelnen Datenträger eine separate Momentaufnahme erstellt. Um anwendungskonsistente Sicherungen zu erstellen, erwägen Sie, den virtuellen Computer vor dem Erstellen von Momentaufnahmen des Datenträgers zu beenden. Verwenden Sie alternativ den [Azure Backup-Dienst](/azure/backup/), mit dem Sie automatisierte Sicherungen ausführen können, während die VM ausgeführt wird.

### <a name="create-snapshot"></a>Erstellen der Momentaufnahme

Vor dem Erstellen der Momentaufnahme einer VM wird die ID oder der Name des Datenträgers benötigt. Rufen Sie die Datenträger-ID mit dem Befehl [az vm show](https://docs.microsoft.com/en-us/cli/azure/vm#show) ab. In diesem Beispiel wird die Datenträger-ID in einer Variablen gespeichert und kann in einem späteren Schritt verwendet werden.

```azurecli
osdiskid=$(az vm show -g myResourceGroupDisk -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
```

Nachdem Sie jetzt die ID des VM-Datenträgers haben, wird mit dem folgenden Befehl dessen Momentaufnahme erstellt.

```azurcli
az snapshot create -g myResourceGroupDisk --source "$osdiskid" --name osDisk-backup
```

### <a name="create-disk-from-snapshot"></a>Erstellen eines Datenträgers aus der Momentaufnahme

Diese Momentaufnahme kann dann in einen Datenträger konvertiert werden, mit dem wiederum der virtuelle Computer neu erstellt werden kann.

```azurecli
az disk create --resource-group myResourceGroupDisk --name mySnapshotDisk --source osDisk-backup
```

### <a name="restore-virtual-machine-from-snapshot"></a>Wiederherstellen des virtuellen Computers aus der Momentaufnahme

Löschen Sie den vorhandenen virtuellen Computer, um die Wiederherstellung des virtuellen Computers durchzuführen. 

```azurecli
az vm delete --resource-group myResourceGroupDisk --name myVM
```

Erstellen Sie einen neuen virtuellen Computer aus dem Momentaufnahmendatenträger.

```azurecli
az vm create --resource-group myResourceGroupDisk --name myVM --attach-os-disk mySnapshotDisk --os-type linux
```

### <a name="reattach-data-disk"></a>Erneutes Anfügen des Datenträgers

Alle Datenträger müssen dem virtuellen Computer erneut angefügt werden.

Finden Sie zuerst mithilfe des Befehls [az disk list](https://docs.microsoft.com/cli/azure/disk#list) den Namen des Datenträgers. In diesem Beispiel wird der Name des Datenträgers in eine Variable namens `datadisk` eingefügt, die im nächsten Schritt verwendet wird.

```azurecli
datadisk=$(az disk list -g myResourceGroupDisk --query "[?contains(name,'myVM')].[name]" -o tsv)
```

Verwenden Sie den Befehl [az vm disk attach](https://docs.microsoft.com/cli/azure/vm/disk#attach), um den Datenträger anzufügen.

```azurecli
az vm disk attach –g myResourceGroupDisk –-vm-name myVM –-disk $datadisk
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Informationen zu VM-Datenträgern erhalten. Im nächsten Tutorial erfahren Sie, wie die VM-Konfiguration automatisiert werden kann.

[Automatisieren der VM-Konfiguration](./tutorial-automate-vm-deployment.md)
