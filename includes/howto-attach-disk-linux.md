
Weitere Informationen zu Datenträgern finden Sie unter [Informationen zu Datenträgern und VHDs für virtuelle Computer](../articles/virtual-machines/virtual-machines-linux-about-disks-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

<a id="attachempty"></a>

## <a name="attach-an-empty-disk"></a>Anfügen eines leeren Datenträgers
1. Öffnen Sie Azure CLI 1.0, und [stellen Sie eine Verbindung mit Ihrem Azure-Abonnement her](../articles/xplat-cli-connect.md). Stellen Sie sicher, dass Sie sich im Azure Service Management-Modus (`azure config mode asm`) befinden.
2. Geben Sie wie im folgenden Beispiel gezeigt den Befehl `azure vm disk attach-new` ein, um einen neuen Datenträger zu erstellen und anzufügen. Ersetzen Sie dabei *myVM* durch den Namen Ihres virtuellen Linux-Computers, und geben Sie die Größe des Datenträgers in GB ein. In diesem Beispiel sind es *100 GB*:

    ```azurecli
    azure vm disk attach-new myVM 100
    ```

3. Nachdem der Datenträger erstellt und angefügt wurde, wird er in der Ausgabe von `azure vm disk list <virtual-machine-name>` wie im folgenden Beispiel aufgeführt:
   
    ```azurecli
    azure vm disk list TestVM
    ```

    Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

    ```bash
    info:    Executing command vm disk list
   
   * Fetching disk images
   * Getting virtual machines
   * Getting VM disks
     data:    Lun  Size(GB)  Blob-Name                         OS
     data:    ---  --------  --------------------------------  -----
     data:         30        myVM-2645b8030676c8f8.vhd  Linux
     data:    0    100       myVM-76f7ee1ef0f6dddc.vhd
     info:    vm disk list command OK
    ```

<a id="attachexisting"></a>

## <a name="attach-an-existing-disk"></a>Anfügen eines vorhandenen Datenträgers
Zum Anfügen eines vorhandenen Datenträgers wird eine VHD-Datei im Speicherkonto benötigt.

1. Öffnen Sie Azure CLI 1.0, und [stellen Sie eine Verbindung mit Ihrem Azure-Abonnement her](../articles/xplat-cli-connect.md). Stellen Sie sicher, dass Sie sich im Azure Service Management-Modus (`azure config mode asm`) befinden.
2. Überprüfen Sie, ob die VHD, die Sie anfügen möchten, bereits in Ihr Azure-Abonnement hochgeladen wurde:
   
    ```azurecli
    azure vm disk list
    ```

    Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

    ```azurecli
     info:    Executing command vm disk list
   
   * Fetching disk images
     data:    Name                                          OS
     data:    --------------------------------------------  -----
     data:    myTestVhd                                     Linux
     data:    TestVM-ubuntuVMasm-0-201508060029150744  Linux
     data:    TestVM-ubuntuVMasm-0-201508060040530369
     info:    vm disk list command OK
    ```

3. Wenn Sie den gewünschten Datenträger nicht finden, können Sie eine lokale VHD in Ihr Abonnement hochladen, indem Sie `azure vm disk create` oder `azure vm disk upload` verwenden. Ein Beispiel für `disk create` sieht wie folgt aus:
   
    ```azurecli
    azure vm disk create myVhd .\TempDisk\test.VHD -l "East US" -o Linux
    ```

    Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

    ```azurecli
    info:    Executing command vm disk create
    + Retrieving storage accounts
    info:    VHD size : 10 GB
    info:    Uploading 10485760.5 KB
    Requested:100.0% Completed:100.0% Running:   0 Time:   25s Speed:    82 KB/s
    info:    Finishing computing MD5 hash, 16% is complete.
    info:    https://mystorageaccount.blob.core.windows.net/disks/myVHD.vhd was
    uploaded successfully
    info:    vm disk create command OK
    ```
   
   Sie können auch den Befehl `azure vm disk upload` zum Hochladen einer VHD in ein bestimmtes Speicherkonto verwenden. Weitere Informationen zu den Befehlen zum Verwalten der Datenträger Ihrer virtuellen Azure-Computer finden Sie [hier](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).

4. Nun fügen Sie die gewünschte VHD an Ihren virtuellen Computer an:
   
    ```azurecli
    azure vm disk attach myVM myVhd
    ```
   
   Denken Sie daran, *myVM* durch den Namen Ihres virtuellen Computers und *myVHD* durch die gewünschte VHD zu ersetzen.

5. Sie können mit dem Befehl `azure vm disk list <virtual-machine-name>`überprüfen, ob der Datenträger an den virtuellen Computer angefügt ist:
   
    ```azurecli
    azure vm disk list myVM
    ```

    Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

    ```azurecli
     info:    Executing command vm disk list
   
   * Fetching disk images
   * Getting virtual machines
   * Getting VM disks
     data:    Lun  Size(GB)  Blob-Name                         OS
     data:    ---  --------  --------------------------------  -----
     data:         30        TestVM-2645b8030676c8f8.vhd  Linux
     data:    1    10        test.VHD
     data:    0    100        TestVM-76f7ee1ef0f6dddc.vhd
     info:    vm disk list command OK
    ```

> [!NOTE]
> Nachdem Sie einen Datenträger angefügt haben, müssen Sie sich auf dem virtuellen Computer anmelden und den Datenträger initialisieren, damit der virtuelle Computer ihn zur Speicherung verwenden kann (in den folgenden Schritten wird das Initialisieren des Datenträgers erläutert).
> 
> 



<!--HONumber=Feb17_HO2-->


