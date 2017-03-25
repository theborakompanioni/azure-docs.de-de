---
title: "Einrichtung von Treibern für die Azure N-Serie unter Linux | Microsoft-Dokumentation"
description: "Einrichtung von NVIDIA-GPU-Treibern für virtuelle Computer der N-Serie mit Linux in Azure"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d91695d0-64b9-4e6b-84bd-18401eaecdde
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/10/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: c1cd1450d5921cf51f720017b746ff9498e85537
ms.openlocfilehash: 1701646ce8cb9494561c37d46cd435b4e7608fe8
ms.lasthandoff: 03/14/2017


---

# <a name="set-up-gpu-drivers-for-n-series-vms-running-linux"></a>Einrichten von GPU-Treibern für virtuelle Computer der N-Serie unter Linux

Nach der Bereitstellung müssen Sie auf jedem virtuellen Computer NVIDIA-Grafiktreiber installieren, um die GPU-Funktionen von virtuellen Azure-Computern der N-Serie mit einer unterstützten Linux-Distribution nutzen zu können. Informationen zur Einrichtung von Treibern stehen auch für [Windows-VMs](virtual-machines-windows-n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) zur Verfügung.


> [!IMPORTANT]
> Derzeit ist Linux-GPU-Unterstützung nur auf virtuellen Azure NC-Computern verfügbar, die Ubuntu Server 16.04 LTS ausführen.
> 

Informationen zu Spezifikationen von virtuellen Computern der N-Serie, Speicherkapazitäten und Details zu den Datenträgern finden Sie unter [Größen für virtuelle Computer](virtual-machines-linux-sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Siehe auch [Allgemeine Überlegungen für virtuelle Computer der N-Serie](#general-considerations-for-n-series-vms).



## <a name="install-nvidia-cuda-drivers"></a>Installieren von NVIDIA CUDA-Treibern

Hier werden Schritte zum Installieren von NVIDIA-Treibern auf Linux-NC-VMs vom NVIDIA CUDA Toolkit 8.0 aus beschrieben. C- und C++-Entwickler können optional das vollständige Toolkit zum Erstellen GPU-beschleunigter Anwendungen installieren. Weitere Informationen finden Sie im [CUDA-Installationshandbuch](http://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html).


> [!NOTE]
> Die hier angebotenen Links zum Herunterladen von Treibern sind zum Zeitpunkt der Veröffentlichung aktuell. Die neuesten Treiber finden Sie auf der [NVIDIA](http://www.nvidia.com/)-Website.

Um das CUDA-Toolkit installieren zu können, stellen Sie eine SSH-Verbindung mit jeder VM her. Führen Sie den folgenden Befehl aus, um sicherzustellen, dass das System über eine CUDA-fähige GPU verfügt:

```bash
lspci | grep -i NVIDIA
```
Die Ausgabe sieht in etwa wie das folgende Beispiel aus (zeigt eine NVIDIA Tesla K80-Karte):

![lspci-Befehlsausgabe](./media/virtual-machines-linux-n-series-driver-setup/lspci.png)

Führen Sie dann die für Ihre Verteilung spezifischen Befehle aus.

### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS

```bash
CUDA_REPO_PKG=cuda-repo-ubuntu1604_8.0.61-1_amd64.deb

wget -O /tmp/${CUDA_REPO_PKG} http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/${CUDA_REPO_PKG} 

sudo dpkg -i /tmp/${CUDA_REPO_PKG}

rm -f /tmp/${CUDA_REPO_PKG}

sudo apt-get update

sudo apt-get install cuda-drivers

```
Die Installation kann einige Minuten dauern.

Um das vollständige CUDA-Toolkit optional zu installieren, geben Sie Folgendes ein:

```bash
sudo apt-get install cuda
```

Starten Sie die VM neu, und fahren Sie mit der Überprüfung der Installation fort.

## <a name="verify-driver-installation"></a>Überprüfen der Treiberinstallation


Stellen Sie zum Abfragen des GPU-Gerätestatus eine SSH-Verbindung mit der VM her, und führen Sie das mit dem Treiber installierte Befehlszeilen-Hilfsprogramm [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) aus. 

![NVIDIA-Gerätestatus](./media/virtual-machines-linux-n-series-driver-setup/smi.png)

## <a name="cuda-driver-updates"></a>CUDA-Treiberupdates

Sie sollten CUDA-Treiber nach der Bereitstellung in regelmäßigen Abständen aktualisieren.

### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS

```bash
sudo apt-get update

sudo apt-get upgrade -y

sudo apt-get dist-upgrade -y

sudo apt-get install cuda-drivers
```

Nachdem das Update abgeschlossen ist, starten Sie die VM neu.


[!INCLUDE [virtual-machines-n-series-considerations](../../includes/virtual-machines-n-series-considerations.md)]

* Sie sollten nicht X-Server oder andere Systeme installieren, die den Nouveau-Treiber auf Ubuntu-NC-VMs verwenden. Vor der Installation von NVIDIA-GPU-Treibern müssen Sie den Nouveau-Treiber deaktivieren.  

* Wenn Sie ein Image einer Linux-VM erfassen möchten, auf der Sie NVIDIA-Treiber installiert haben, informieren Sie sich unter [Generalisieren und Erfassen eines virtuellen Linux-Computers](virtual-machines-linux-capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu den NVIDIA GPUs auf der N-Serie VMs finden Sie unter:
    * [NVIDIA Tesla K80](http://www.nvidia.com/object/tesla-k80.html) (für virtuelle Azure NC-Computer)
    * [NVIDIA Tesla M60](http://www.nvidia.com/object/tesla-m60.html) (für virtuelle Azure NV-Computer)


