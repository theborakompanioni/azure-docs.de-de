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
ms.date: 12/07/2016
ms.author: danlep
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: cea53acc33347b9e6178645f225770936788f807
ms.openlocfilehash: 0d7eba02757fb1b2263cf11c561b374eab837f21
ms.lasthandoff: 03/03/2017


---
# <a name="set-up-gpu-drivers-for-n-series-linux-vms"></a>Einrichten von GPU-Treibern für Linux-VMs der N-Serie
Nach der Bereitstellung müssen Sie auf jedem virtuellen Computer NVIDIA-Grafiktreiber installieren, um die GPU-Funktionen von virtuellen Azure-Computern der N-Serie mit einer unterstützten Linux-Distribution nutzen zu können. Dieser Artikel ist auch für [virtuelle Windows-Computer](virtual-machines-windows-n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)verfügbar.

Informationen zu Spezifikationen von virtuellen Computern der N-Serie, Speicherkapazitäten und Details zu den Datenträgern finden Sie unter [Größen für virtuelle Computer](virtual-machines-linux-sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).



## <a name="supported-gpu-drivers"></a>Unterstützte GPU-Treiber


> [!NOTE]
> Derzeit ist Linux-GPU-Unterstützung nur auf virtuellen Azure NC-Computern verfügbar, die Ubuntu Server 16.04 LTS ausführen.

### <a name="nvidia-tesla-drivers-for-nc-vms"></a>NVIDIA Tesla-Treiber für virtuelle NC-Computer

* [Ubuntu 16.04 LTS](https://go.microsoft.com/fwlink/?linkid=836899) (selbstextrahierende RUN-Installationsdatei ausführen)

## <a name="tesla-driver-installation-on-ubuntu-1604-lts"></a>Installation des Tesla-Treibers unter Ubuntu 16.04 LTS

1. Stellen Sie eine SSH-Verbindung mit dem virtuellen Computer der Azure N-Serie her.

2. Führen Sie den folgenden Befehl aus, um sicherzustellen, dass das System über eine CUDA-fähige GPU verfügt:

    ```bash
    lspci | grep -i NVIDIA
    ```
    Die Ausgabe sieht in etwa wie das folgende Beispiel aus (zeigt eine NVIDIA Tesla K80-Karte):

    ![lspci-Befehlsausgabe](./media/virtual-machines-linux-n-series-driver-setup/lspci.png)

3. Laden Sie die RUN-Datei des Treibers für Ihre Distribution herunter. Mit dem Befehl im folgenden Beispiel wird der Ubuntu 16.04 LTS Tesla-Treiber in das Verzeichnis „/tmp“ heruntergeladen:

    ```bash
    wget -O /tmp/NVIDIA-Linux-x86_64-367.48.run https://go.microsoft.com/fwlink/?linkid=836899
    ```

4. Wenn Sie `gcc` und `make` in Ihrem System installieren müssen (erforderlich für die Tesla-Treiber), geben Sie Folgendes ein:

    ```bash
    sudo apt-get update
    
    sudo apt install gcc

    sudo apt install make
    ```

4. Ändern Sie den Wert in das Verzeichnis mit der Treiberinstallationsdatei, und führen Sie ähnliche Befehle wie die folgenden aus:

    ```bash
    chmod +x NVIDIA-Linux-x86_64-367.48.run
    
    sudo sh ./NVIDIA-Linux-x86_64-367.48.run
    ```

## <a name="verify-driver-installation"></a>Überprüfen der Treiberinstallation


Führen Sie zum Abfragen des GPU-Gerätestatus das mit dem Treiber installierte Befehlszeilenprogramm [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) aus. 

![NVIDIA-Gerätestatus](./media/virtual-machines-linux-n-series-driver-setup/smi.png)

## <a name="optional-installation-of-nvidia-cuda-toolkit-on-ubuntu-1604-lts"></a>Optionale Installation von NVIDIA CUDA Toolkits unter Ubuntu 16.04 LTS

Sie können optional NVIDIA CUDA Toolkit 8.0 auf virtuellen NC-Computern mit Ubuntu 16.04 LTS installieren. Neben GPU-Treibern enthält das Toolkit eine umfangreiche Entwicklungsumgebung für C- und C++-Entwickler, die GPU-beschleunigte Anwendungen erstellen.

Führen Sie zum Installieren von CUDA Toolkit Befehle aus, die etwa folgendermaßen aussehen:

```bash
CUDA_REPO_PKG=cuda-repo-ubuntu1604_8.0.44-1_amd64.deb

wget -O /tmp/${CUDA_REPO_PKG} http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/${CUDA_REPO_PKG} 

sudo dpkg -i /tmp/${CUDA_REPO_PKG}

rm -f /tmp/${CUDA_REPO_PKG}

sudo apt-get update

sudo apt-get install cuda-drivers
```

Die Installation kann einige Minuten dauern.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu den NVIDIA GPUs auf der N-Serie VMs finden Sie unter:
    * [NVIDIA Tesla K80](http://www.nvidia.com/object/tesla-k80.html) (für virtuelle Azure NC-Computer)
    * [NVIDIA Tesla M60](http://www.nvidia.com/object/tesla-m60.html) (für virtuelle Azure NV-Computer)


