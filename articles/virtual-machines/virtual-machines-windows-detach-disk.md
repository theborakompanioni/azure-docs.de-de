<properties
	pageTitle="Trennen eines Datenträgers von einem virtuellen Windows-Computer | Microsoft Azure"
	description="Hier erfahren Sie, wie Sie einen Datenträger unter Verwendung des Resource Manager-Bereitstellungsmodells von einem virtuellen Computer in Azure trennen."
	services="virtual-machines-windows"
	documentationCenter=""
	authors="cynthn"
	manager="timlt"
	editor=""
	tags="azure-service-management"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/02/2016"
	ms.author="cynthn"/>



# Trennen eines Datenträgers von einem virtuellen Windows-Computer


Wenn Sie einen Datenträger, der an einen virtuellen Computer angefügt ist, nicht mehr benötigen, können Sie ihn leicht trennen. Dadurch wird der Datenträger von dem virtuellen Computer entfernt, aber nicht aus dem Speicher.

> [AZURE.WARNING] Beim Trennen eines Datenträgers wird dieser nicht automatisch gelöscht. Wenn Sie Storage Premium abonniert haben, fallen auch weiterhin Speichergebühren für den Datenträger an. Weitere Informationen finden Sie unter [Preisgestaltung und Abrechnung bei der Verwendung von Storage Premium](../storage/storage-premium-storage.md#pricing-and-billing).

Wenn Sie die vorhandenen Daten erneut auf dem Datenträger verwenden möchten, können Sie ihn erneut an denselben virtuellen Computer oder an einen anderen anfügen.


## Trennen eines Datenträgers im Portal

1. Wählen Sie im Portal **Virtuelle Computer** aus.

2. Wählen Sie den virtuellen Computer mit dem zu trennenden Datenträger aus, und klicken Sie auf **Alle Einstellungen**.

3. Wählen Sie auf dem Blatt **Einstellungen** die Option **Datenträger** aus.

4. Wählen Sie im Blatt **Datenträger** den Datenträger aus, den Sie trennen möchten.

5. Klicken Sie im Blatt für diesen Datenträger auf **Trennen**.


	![Screenshot mit der Schaltfläche „Trennen“.](./media/virtual-machines-windows-detach-disk/detach-disk.png)

Der Datenträger verbleibt im Speicher, ist jedoch nicht mehr an einen virtuellen Computer angefügt.


## Trennen eines Datenträgers mit PowerShell

In diesem Beispiel ruft der erste Befehl mit dem Cmdlet „Get-AzureRmVM“ den virtuellen Computer mit dem Namen **MyVM07** aus der Ressourcengruppe **RG11** ab. Der Befehl speichert den virtuellen Computer in der Variablen **$VirtualMachine**.

Der zweite Befehl entfernt den Datenträger DataDisk3 vom virtuellen Computer.

Der letzte Befehl aktualisiert den Status des virtuellen Computers und schließt dadurch das Entfernen des Datenträgers ab.

	$VirtualMachine = Get-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07" 
	Remove-AzureRmVMDataDisk -VM $VirtualMachine -Name "DataDisk3"
	Update-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07" -VM $VirtualMachine


Weitere Informationen finden Sie unter [Remove-AzureRmVMDataDisk](https://msdn.microsoft.com/library/mt603614.aspx).

## Nächste Schritte

Wenn Sie den Datenträger wiederverwenden möchten, können Sie ihn einfach [an einen anderen virtuellen Computer anfügen](virtual-machines-windows-attach-disk-portal.md).

<!---HONumber=AcomDC_0608_2016-->