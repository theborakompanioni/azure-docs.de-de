Wenn Sie einen Datenträger, der einem virtuellen Computer (VM) angefügt ist, nicht mehr benötigen, können Sie ihn leicht trennen. Dadurch wird der Datenträger von der VM entfernt, aber nicht aus dem Speicher. Wenn Sie die vorhandenen Daten erneut auf dem Datenträger verwenden möchten, können Sie ihn erneut an dieselbe VM oder eine andere anfügen.

> [AZURE.NOTE] Eine VM in Azure verwendet verschiedene Arten von Datenträgern wie Betriebssystemfestplatten, lokale temporäre Festplatten und optionale Datenträger. Weitere Informationen finden Sie unter [Datenträgern und VHDs für virtuelle Computer](../articles/virtual-machines/virtual-machines-linux-about-disks-vhds.md). Es ist nicht möglich, einen Betriebssystem-Datenträger zu trennen, es sei denn, Sie löschen auch die VM.

## Suchen des Datenträgers

Bevor Sie einen Datenträger von einer VM trennen können, müssen Sie die LUN-Nummer herausfinden, die als Bezeichner des zu trennenden Datenträgers fungiert. Gehen Sie dazu folgendermaßen vor:

1. 	Öffnen Sie die Azure-Befehlszeilenschnittstelle, und [stellen Sie eine Verbindung mit Ihrem Azure-Abonnement her](../articles/xplat-cli-connect.md). Stellen Sie sicher, dass Sie sich im Azure Service Management-Modus (`azure config mode asm`) befinden.

2. 	Bestimmen Sie die Ihrer VM angefügten Datenträger, indem Sie `azure vm disk list
	<virtual-machine-name>` aufrufen:

		$azure vm disk list UbuntuVM
		info:    Executing command vm disk list
		+ Fetching disk images
		+ Getting virtual machines
		+ Getting VM disks
		data:    Lun  Size(GB)  Blob-Name                         OS
		data:    ---  --------  --------------------------------  -----
		data:         30        ubuntuVM-2645b8030676c8f8.vhd  Linux
		data:    1    10        test.VHD
		data:    0    30        ubuntuVM-76f7ee1ef0f6dddc.vhd
		info:    vm disk list command OK

3. 	Notieren Sie sich die LUN **Logical Unit Number (logische Gerätenummer)** des Datenträgers, den Sie trennen möchten.


## Trennen des Datenträgers

Nachdem Sie die LUN des Datenträgers gefunden haben, sind Sie bereit, ihn zu trennen:

1. 	Trennen Sie den ausgewählten Datenträger vom virtuellen Computer mithilfe des folgenden Befehls `azure vm disk detach
 	<virtual-machine-name> <LUN>`:

		$azure vm disk detach UbuntuVM 0
		info:    Executing command vm disk detach
		+ Getting virtual machines
		+ Removing Data-Disk
		info:    vm disk detach command OK

2. 	Sie können überprüfen, ob der Datenträger getrennt wurde, indem Sie diesen Befehl ausführen:

		$azure vm disk list UbuntuVM
		info:    Executing command vm disk list
		+ Fetching disk images
		+ Getting virtual machines
		+ Getting VM disks
		data:    Lun  Size(GB)  Blob-Name                         OS
		data:    ---  --------  --------------------------------  -----
		data:         30        ubuntuVM-2645b8030676c8f8.vhd  Linux
		data:    1    10        test.VHD
		info:    vm disk list command OK

Der getrennte Datenträger verbleibt im Speicher, ist jedoch nicht mehr an einen virtuellen Computer angefügt.

<!---HONumber=AcomDC_0608_2016-->