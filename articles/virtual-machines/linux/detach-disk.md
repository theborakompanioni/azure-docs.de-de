---
title: "Trennen eines Datenträgers für Daten von einem virtuellen Linux-Computer – Azure | Microsoft Docs"
description: "Erfahren Sie, wie Sie einen Datenträger unter Verwendung von Azure CLI 2.0 oder des Azure-Portals von einem virtuellen Computer in Azure trennen."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: azurecli
ms.topic: article
ms.date: 03/21/2017
ms.author: cynthn
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 3f29547e1da6028b1e4b91d9e29fd3bcdfe08d50
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---
# <a name="how-to-detach-a-data-disk-from-a-linux-virtual-machine"></a>Trennen eines Datenträgers von einem virtuellen Linux-Computer

Wenn Sie einen Datenträger, der an einen virtuellen Computer angefügt ist, nicht mehr benötigen, können Sie ihn leicht trennen. Dadurch wird der Datenträger von dem virtuellen Computer entfernt, aber nicht aus dem Speicher. 

> [!WARNING]
> Beim Trennen eines Datenträgers wird dieser nicht automatisch gelöscht. Wenn Sie Storage Premium abonniert haben, fallen auch weiterhin Speichergebühren für den Datenträger an. Weitere Informationen finden Sie unter [Preisgestaltung und Abrechnung bei der Verwendung von Storage Premium](../../storage/common/storage-premium-storage.md#pricing-and-billing). 
> 
> 

Wenn Sie die vorhandenen Daten erneut auf dem Datenträger verwenden möchten, können Sie ihn erneut an denselben virtuellen Computer oder an einen anderen anfügen.  

## <a name="detach-a-data-disk-using-cli-20"></a>Trennen eines Datenträgers mit CLI 2.0

```azurecli
az vm disk detach -g myResourceGroup --vm-name myVm -n myDataDisk
```

Der Datenträger verbleibt im Speicher, ist jedoch nicht mehr an einen virtuellen Computer angefügt.


## <a name="detach-a-data-disk-using-the-portal"></a>Trennen eines Datenträgers im Portal
1. Wählen Sie im Portal **Virtuelle Computer**aus.
2. Wählen Sie den virtuellen Computer mit dem zu trennenden Datenträger aus, und klicken Sie auf **Beenden**, um die Zuordnung der VM aufzuheben.
3. Klicken Sie auf dem Blatt für den virtuellen Computer auf **Datenträger**.
4. Wählen Sie oben auf dem Blatt **Datenträger** die Option **Bearbeiten** aus.
5. Klicken Sie auf dem Blatt **Datenträger** ganz rechts neben dem Datenträger, den Sie trennen möchten, auf die Schaltfläche „Trennen“ (![Schaltfläche „Trennen“](./media/detach-disk/detach.png)).
5. Klicken Sie, nachdem der Datenträger entfernt wurde, oben auf dem Blatt auf „Speichern“.
6. Klicken Sie auf dem Blatt für den virtuellen Computer auf **Übersicht** und dann oben auf dem Blatt auf die Schaltfläche **Starten**, um den virtuellen Computer neu zu starten.

Der Datenträger verbleibt im Speicher, ist jedoch nicht mehr an einen virtuellen Computer angefügt.








## <a name="next-steps"></a>Nächste Schritte
Wenn Sie den Datenträger wiederverwenden möchten, können Sie ihn einfach [an einen anderen virtuellen Computer anfügen](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


