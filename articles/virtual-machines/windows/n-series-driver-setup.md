---
title: "Einrichtung von Treibern für die Azure N-Serie unter Windows | Microsoft-Dokumentation"
description: "Einrichtung von NVIDIA-GPU-Treibern für virtuelle Computer der N-Serie mit Windows in Azure"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f3950c34-9406-48ae-bcd9-c0418607b37d
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/07/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: 2ad539c85e01bc132a8171490a27fd807c8823a4
ms.openlocfilehash: aa2d4f671bab46929ccc4444f8fe9de98a3e0eb2
ms.contentlocale: de-de
ms.lasthandoff: 07/12/2017

---
# <a name="set-up-gpu-drivers-for-n-series-vms-running-windows-server"></a>Einrichten von GPU-Treibern für virtuelle Computer der N-Serie unter Windows Server
Installieren Sie unterstützte NVIDIA-Grafiktreiber, um die GPU-Funktionen von virtuellen Azure-Computern der N-Serie mit Windows Server 2016 oder Windows Server 2012 R2 nutzen zu können. In diesem Artikel werden die Treiberinstallationsschritte beschrieben, die Sie nach der Bereitstellung eines virtuellen Computers der N-Serie ausführen müssen. Informationen zur Einrichtung von Treibern stehen auch für [Linux-VMs](../linux/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) zur Verfügung.

Informationen zu grundlegenden Spezifikationen, Speicherkapazitäten und Details zu den Datenträgern finden Sie unter [GPU-optimierte Größen von virtuellen Windows-Computern](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 


[!INCLUDE [virtual-machines-n-series-windows-support](../../../includes/virtual-machines-n-series-windows-support.md)]



## <a name="driver-installation"></a>Treiberinstallation

1. Stellen Sie über Remotedesktop eine Verbindung mit den einzelnen virtuellen Computern der N-Serie her.

2. Laden Sie den unterstützten Treiber für Ihr Windows-Betriebssystem herunter, entpacken und installieren Sie ihn.

Auf virtuellen Azure NV-Computern ist nach der Treiberinstallation ein Neustart erforderlich. Für virtuelle NC-Computer ist kein Neustart erforderlich.

## <a name="verify-driver-installation"></a>Überprüfen der Treiberinstallation

Sie können die Treiberinstallation im Geräte-Manager überprüfen. Das folgende Beispiel zeigt die erfolgreiche Konfiguration der Tesla K80-Karte auf einem virtuellen Azure-NC-Computer.

![Eigenschaften des GPU-Treibers](./media/n-series-driver-setup/GPU_driver_properties.png)

Führen Sie zum Abfragen des GPU-Gerätestatus das mit dem Treiber installierte Befehlszeilenprogramm [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) aus.

1. Öffnen Sie eine Eingabeaufforderung, und wechseln Sie in das Verzeichnis **C:\Programme\NVIDIA Corporation\NVSMI**.

2. Führen Sie **nvidia-smi** aus. Wenn der Treiber installiert wurde, wird eine Ausgabe ähnlich wie unten angezeigt. Beachten Sie, dass eine **GPU-Auslastung** von **0 %** angezeigt wird, sofern gerade keine GPU-Auslastung auf dem virtuellen Computer ausgeführt wird.

![NVIDIA-Gerätestatus](./media/n-series-driver-setup/smi.png)  

## <a name="rdma-network-for-nc24r-vms"></a>RDMA-Netzwerk für virtuelle NC24r-Computer

RDMA-Netzwerkkonnektivität kann auf virtuellen NC24r-Computern aktiviert werden, die in der gleichen Verfügbarkeitsgruppe bereitgestellt werden. Die HpcVmDrivers-Erweiterung muss hinzugefügt werden, um Windows-Netzwerkgerätetreiber zu installieren, die für RDMA-Konnektivität benötigt werden. Verwenden Sie [Azure PowerShell](/powershell/azure/overview)-Cmdlets für Azure Resource Manager, um die VM-Erweiterung einem virtuellen NC24r-Computer hinzuzufügen.

> [!NOTE]
> Derzeit unterstützt nur Windows Server 2012 R2 das RDMA-Netzwerk auf virtuellen NC24r-Computern.
> 

So installieren Sie die neueste Version (1.1) der HpcVMDrivers-Erweiterung auf einem vorhandenen, RDMA-fähigen virtuellen Computer namens myVM in der Region USA, Westen:
  ```PowerShell
  Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  Weitere Informationen finden Sie unter [Erweiterungen und Features für virtuelle Computer für Windows](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Das RDMA-Netzwerk unterstützt Message Passing Interface-Datenverkehr (MPI) für Anwendungen mit [Microsoft MPI](https://msdn.microsoft.com/library/bb524831(v=vs.85).aspx) oder Intel MPI 5.x. 


## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu den NVIDIA GPUs auf der N-Serie VMs finden Sie unter:
    * [NVIDIA Tesla K80](http://www.nvidia.com/object/tesla-k80.html) (für virtuelle Azure NC-Computer)
    * [NVIDIA Tesla M60](http://www.nvidia.com/object/tesla-m60.html) (für virtuelle Azure NV-Computer)

* Entwickler, die GPU-beschleunigte Anwendungen für die NVIDIA Tesla-GPUs erstellen, können auch das CUDA Toolkit 8 für [Windows Server 2016](https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda_8.0.61_win10-exe) oder [Windows Server 2012 R2](https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda_8.0.61_windows-exe) herunterladen und installieren. Weitere Informationen finden Sie im [CUDA-Installationshandbuch](http://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html#axzz4ZcwJvqYi).



