Wenn Sie einen Datenträger, der einem virtuellen Computer (VM) angefügt ist, nicht mehr benötigen, können Sie ihn leicht trennen. Wenn Sie die Verbindung zwischen einem Datenträger und dem virtuellen Computer trennen, wird der Datenträger nicht aus dem Speicher entfernt. Wenn Sie die vorhandenen Daten erneut auf dem Datenträger verwenden möchten, können Sie ihn erneut an dieselbe VM oder eine andere anfügen.  

> [!NOTE]
> Eine VM in Azure verwendet verschiedene Arten von Datenträgern wie Betriebssystemfestplatten, lokale temporäre Festplatten und optionale Datenträger. Weitere Informationen finden Sie unter [Informationen zu Datenträgern und VHDs für virtuelle Computer](../articles/virtual-machines/linux/about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Es ist nicht möglich, einen Betriebssystem-Datenträger zu trennen, ohne auch die VM zu löschen.

## <a name="find-the-disk"></a>Suchen des Datenträgers
Bevor Sie einen Datenträger von einer VM trennen können, müssen Sie die LUN-Nummer herausfinden, die als Bezeichner des zu trennenden Datenträgers fungiert. Gehen Sie dazu folgendermaßen vor:

1. Öffnen Sie die Azure-Befehlszeilenschnittstelle, und [stellen Sie eine Verbindung mit Ihrem Azure-Abonnement her](../articles/xplat-cli-connect.md). Stellen Sie sicher, dass Sie sich im Azure Service Management-Modus (`azure config mode asm`) befinden.
2. Bestimmen Sie die an Ihren virtuellen Computer angefügten Datenträger. Im folgenden Beispiel werden Datenträger für die VM mit dem Namen `myVM` aufgeführt:

    ```azurecli
    azure vm disk list myVM
    ```

    Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

    ```azurecli
    * Fetching disk images
    * Getting virtual machines
    * Getting VM disks
      data:    Lun  Size(GB)  Blob-Name                         OS
      data:    ---  --------  --------------------------------  -----
      data:         30        ubuntuVM-2645b8030676c8f8.vhd  Linux
      data:    0    30        myDataDisk.vhd
      info:    vm disk list command OK
    ```

3. Notieren Sie sich die LUN **Logical Unit Number (logische Gerätenummer)** des Datenträgers, den Sie trennen möchten.

## <a name="remove-operating-system-references-to-the-disk"></a>Entfernen von Betriebssystemverweisen auf den Datenträger
Bevor Sie den Datenträger vom Linux-Gast trennen, müssen Sie sicherstellen, dass keine Partition auf dem Datenträger verwendet wird. Stellen Sie sicher, dass das Betriebssystem nicht versucht, die Datenträger nach dem Neustart erneut einzubinden. Mit diesen Schritten wird die Konfiguration rückgängig gemacht, die Sie beim [Anfügen](../articles/virtual-machines/linux/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) des Datenträgers vermutlich vorgenommen haben.

1. Verwenden Sie den Befehl `lsscsi`, um die Datenträger-ID zu ermitteln. `lsscsi` kann entweder über `yum install lsscsi` (für auf Red Hat basierende Distributionen) oder über `apt-get install lsscsi` (für auf Debian basierende Distributionen) installiert werden. Sie finden die gesuchte Datenträger-ID, indem Sie die LUN (Logical Unit Number, logische Gerätenummer) verwenden. Die letzte Zahl im Tupel in jeder Zeile ist die LUN. Im folgenden Beispiel zu `lsscsi` wird die Zuordnung über LUN 0 mit */dev/sdc* durchgeführt.

    ```bash
    [1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0
    [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
    [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
    [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc
    ```

2. Verwenden Sie `fdisk -l <disk>`, um die Partitionen zu ermitteln, die dem zu trennenden Datenträger zugeordnet sind. Das folgende Beispiel zeigt die Ausgabe für `/dev/sdc`:

    ```bash
    Disk /dev/sdc: 1098.4 GB, 1098437885952 bytes, 2145386496 sectors
    Units = sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disk label type: dos
    Disk identifier: 0x5a1d2a1a
    
        Device Boot      Start         End      Blocks   Id  System
    /dev/sdc1            2048  2145386495  1072692224   83  Linux
    ```

3. Heben Sie die Bereitstellung für jede Partition auf, die für den Datenträger aufgelistet ist. Im folgenden Beispiel wird die Bereitstellung für `/dev/sdc1` aufgehoben:

    ```bash
    sudo umount /dev/sdc1
    ```

4. Verwenden Sie den Befehl `blkid`, um die UUIDs für alle Partitionen zu ermitteln. Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

    ```bash
    /dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
    /dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
    /dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
    ```

5. Entfernen Sie die Einträge in der Datei **/etc/fstab** , die entweder den Gerätepfaden oder UUIDs zugeordnet sind, für alle Partitionen des zu trennenden Datenträgers.  Einträge für dieses Beispiel können wie folgt lauten:

    ```sh  
   UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults   1   2
   ```

    oder
   
   ```sh   
   /dev/sdc1   /datadrive   ext4   defaults   1   2
   ```

## <a name="detach-the-disk"></a>Trennen des Datenträgers
Nachdem Sie die LUN-Nummer des Datenträgers ermittelt und die Betriebssystemverweise entfernt haben, ist alles für den Trennvorgang vorbereitet:

1. Trennen Sie den ausgewählten Datenträger mithilfe des Befehls `azure vm disk detach
   <virtual-machine-name> <LUN>` vom virtuellen Computer. Im folgenden Beispiel wird LUN `0` von der VM mit dem Namen `myVM` getrennt:
   
    ```azurecli
    azure vm disk detach myVM 0
    ```

2. Sie können überprüfen, ob der Datenträger getrennt wurde, indem Sie `azure vm disk list` erneut ausführen. Im folgenden Beispiel wird die VM mit dem Namen `myVM` überprüft:
   
    ```azurecli
    azure vm disk list myVM
    ```

    Die Ausgabe ähnelt dem folgenden Beispiel, in dem veranschaulicht wird, dass der Datenträger nicht mehr angefügt ist:

    ```azurecli
    info:    Executing command vm disk list
   
   * Fetching disk images
   * Getting virtual machines
   * Getting VM disks
     data:    Lun  Size(GB)  Blob-Name                         OS
     data:    ---  --------  --------------------------------  -----
     data:         30        ubuntuVM-2645b8030676c8f8.vhd  Linux
     info:    vm disk list command OK
    ```

Der getrennte Datenträger verbleibt im Speicher, ist jedoch nicht mehr an einen virtuellen Computer angefügt.

