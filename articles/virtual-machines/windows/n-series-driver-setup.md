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
ms.date: 04/03/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 303cb9950f46916fbdd58762acd1608c925c1328
ms.openlocfilehash: 1767a2db05abd3abadfedbef86c38e55c5a57980
ms.lasthandoff: 04/04/2017


---
# <a name="set-up-gpu-drivers-for-n-series-vms-running-windows-server"></a>Einrichten von GPU-Treibern für virtuelle Computer der N-Serie unter Windows Server
Nach der Bereitstellung müssen Sie auf jedem virtuellen Computer NVIDIA-Grafiktreiber installieren, um die GPU-Funktionen von virtuellen Azure-Computern der N-Serie mit Windows Server 2016 oder Windows Server 2012 R2 nutzen zu können. Informationen zur Einrichtung von Treibern stehen auch für [Linux-VMs](../linux/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) zur Verfügung.

Informationen zu grundlegenden Spezifikationen, Speicherkapazitäten und Details zu den Datenträgern finden Sie unter [Größen für virtuelle Computer](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Siehe auch [Allgemeine Überlegungen für virtuelle Computer der N-Serie](#general-considerations-for-n-series-vms).



## <a name="supported-gpu-drivers"></a>Unterstützte GPU-Treiber

Stellen Sie über Remotedesktop eine Verbindung mit den einzelnen virtuellen Computern der N-Serie her. Laden Sie den unterstützten Treiber für Ihr Windows-Betriebssystem herunter, entpacken und installieren Sie ihn. 

### <a name="nvidia-tesla-drivers-for-nc-vms-tesla-k80"></a>NVIDIA Tesla-Treiber für virtuelle NC-Computer (Tesla K80)



| Betriebssystem | Treiberversion |
| -------- |------------- |
| Windows Server 2016 | [376.84](http://us.download.nvidia.com/Windows/Quadro_Certified/376.84/376.84-tesla-desktop-winserver2016-international-whql.exe) (.exe) |
| Windows Server 2012 R2 | [376.84](http://us.download.nvidia.com/Windows/Quadro_Certified/376.84/376.84-tesla-desktop-winserver2008-2012r2-64bit-international-whql.exe) (.exe) |

> [!NOTE]
> Die hier bereitgestellten Links zum Herunterladen der Tesla-Treiber sind zum Zeitpunkt der Veröffentlichung aktuell. Die neuesten Treiber finden Sie auf der [NVIDIA](http://www.nvidia.com/)-Website.
>

### <a name="nvidia-grid-drivers-for-nv-vms-tesla-m60"></a>NVIDIA GRID-Treiber für virtuelle NV-Computer (Tesla M60)

| Betriebssystem | Treiberversion |
| -------- |------------- |
| Windows Server 2016 | [369.95](https://go.microsoft.com/fwlink/?linkid=836843) (ZIP) |
| Windows Server 2012 R2 | [369.95](https://go.microsoft.com/fwlink/?linkid=836844) (ZIP)  |



## <a name="verify-gpu-driver-installation"></a>Überprüfen der GPU-Treiberinstallation

Auf virtuellen Azure NV-Computern ist nach der Treiberinstallation ein Neustart erforderlich. Für virtuelle NC-Computer ist kein Neustart erforderlich.

Sie können die Treiberinstallation im Geräte-Manager überprüfen. Das folgende Beispiel zeigt die erfolgreiche Konfiguration der Tesla K80-Karte auf einem virtuellen Azure-NC-Computer.

![Eigenschaften des GPU-Treibers](./media/n-series-driver-setup/GPU_driver_properties.png)

Führen Sie zum Abfragen des GPU-Gerätestatus das mit dem Treiber installierte Befehlszeilenprogramm [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) aus. 

![NVIDIA-Gerätestatus](./media/n-series-driver-setup/smi.png)  

## <a name="rdma-network-for-nc24r-vms"></a>RDMA-Netzwerk für virtuelle NC24r-Computer

RDMA-Netzwerkkonnektivität kann auf virtuellen NC24r-Computern aktiviert werden, die in der gleichen Verfügbarkeitsgruppe bereitgestellt werden. Die HpcVmDrivers-Erweiterung muss hinzugefügt werden, um Windows-Netzwerkgerätetreiber zu installieren, die für RDMA-Konnektivität benötigt werden. Verwenden Sie [Azure PowerShell](/powershell/azureps-cmdlets-docs)-Cmdlets für Azure Resource Manager, um die VM-Erweiterung einem virtuellen NC24r-Computer hinzuzufügen.

> [!NOTE]
> Derzeit unterstützt nur Windows Server 2012 R2 das RDMA-Netzwerk auf virtuellen NC24r-Computern.
> 

So installieren Sie die neueste Version (1.1) der HpcVMDrivers-Erweiterung auf einem vorhandenen, RDMA-fähigen virtuellen Computer namens myVM in der Region USA, Westen:
  ```PowerShell
  Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  Weitere Informationen finden Sie unter [Erweiterungen und Features für virtuelle Computer für Windows](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Das RDMA-Netzwerk unterstützt Message Passing Interface-Datenverkehr (MPI) für Anwendungen mit [Microsoft MPI](https://msdn.microsoft.com/library/bb524831(v=vs.85).aspx) oder Intel MPI 5.x. 

[!INCLUDE [virtual-machines-n-series-considerations](../../../includes/virtual-machines-n-series-considerations.md)]

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu den NVIDIA GPUs auf der N-Serie VMs finden Sie unter:
    * [NVIDIA Tesla K80](http://www.nvidia.com/object/tesla-k80.html) (für virtuelle Azure NC-Computer)
    * [NVIDIA Tesla M60](http://www.nvidia.com/object/tesla-m60.html) (für virtuelle Azure NV-Computer)

* Entwickler, die GPU-beschleunigte Anwendungen für die NVIDIA Tesla-GPUs erstellen, können auch das CUDA Toolkit 8 für [Windows Server 2016](https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda_8.0.61_win10-exe) oder [Windows Server 2012 R2](https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda_8.0.61_windows-exe) herunterladen und installieren. Weitere Informationen finden Sie im [CUDA-Installationshandbuch](http://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html#axzz4ZcwJvqYi).



