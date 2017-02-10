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
ms.date: 11/30/2016
ms.author: danlep
translationtype: Human Translation
ms.sourcegitcommit: 4bf470737f961219250c37d890ed81eb62409626
ms.openlocfilehash: feb9370f0241fd860749d32f5db5842cb18463f1


---
# <a name="set-up-gpu-drivers-for-n-series-vms"></a>Einrichten von GPU-Treibern für virtuelle Computer der N-Serie
Nach der Bereitstellung müssen Sie auf jedem virtuellen Computer NVIDIA-Grafiktreiber installieren, um die GPU-Funktionen von virtuellen Azure-Computern der N-Serie mit Windows Server nutzen zu können. Dieser Artikel ist auch für [virtuelle Linux-Computer](virtual-machines-linux-n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) verfügbar.

Informationen zu grundlegenden Spezifikationen, Speicherkapazitäten und Details zu den Datenträgern finden Sie unter [Größen für virtuelle Computer](virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).




## <a name="supported-gpu-drivers"></a>Unterstützte GPU-Treiber

Stellen Sie über Remotedesktop eine Verbindung mit den einzelnen virtuellen Computern der N-Serie her. Laden Sie den unterstützten Treiber für Ihr Windows-Betriebssystem herunter, entpacken und installieren Sie ihn. 

### <a name="nvidia-grid-drivers-for-nv-vms"></a>NVIDIA GRID-Treiber für virtuelle NV-Computer

* [Windows Server 2016](https://go.microsoft.com/fwlink/?linkid=836843) (.zip)

* [Windows Server 2012 R2](https://go.microsoft.com/fwlink/?linkid=836844) (.zip)

### <a name="nvidia-tesla-drivers-for-nc-vms"></a>NVIDIA Tesla-Treiber für virtuelle NC-Computer

* [Windows Server 2016](https://go.microsoft.com/fwlink/?linkid=836841) (.zip)

* [Windows Server 2012 R2](https://go.microsoft.com/fwlink/?linkid=836842) (.zip)



## <a name="verify-driver-installation"></a>Überprüfen der Treiberinstallation

Auf virtuellen Azure NV-Computern ist nach der Treiberinstallation ein Neustart erforderlich. Für virtuelle NC-Computer ist kein Neustart erforderlich.

Sie können die Treiberinstallation im Geräte-Manager überprüfen. Das folgende Beispiel zeigt die erfolgreiche Konfiguration der K80-Karte auf einem virtuellen Azure-NC-Computer.

![Eigenschaften des GPU-Treibers](./media/virtual-machines-windows-n-series-driver-setup/GPU_driver_properties.png)

Führen Sie zum Abfragen des GPU-Gerätestatus das mit dem Treiber installierte Befehlszeilenprogramm [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) aus. 

![NVIDIA-Gerätestatus](./media/virtual-machines-windows-n-series-driver-setup/smi.png)  

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu den NVIDIA-GPUs auf virtuellen Computern der N-Serie finden Sie unter:
    * [NVIDIA Tesla K80](http://www.nvidia.com/object/tesla-k80.html) (für virtuelle Azure NC-Computer)
    * [NVIDIA Tesla M60](http://www.nvidia.com/object/tesla-m60.html) (für virtuelle Azure NV-Computer)

* Entwickler, die GPU-beschleunigte Anwendungen für die NVIDIA Tesla-GPUs erstellen, können auch das [CUDA Toolkit 8](https://developer.nvidia.com/cuda-downloads) herunterladen und installieren.





<!--HONumber=Dec16_HO2-->


