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
ms.date: 05/02/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: be3ac7755934bca00190db6e21b6527c91a77ec2
ms.openlocfilehash: 181428e5302c5c8f5b72f06d6c54b0f87802690a
ms.contentlocale: de-de
ms.lasthandoff: 05/03/2017


---

# <a name="set-up-gpu-drivers-for-n-series-vms-running-linux"></a>Einrichten von GPU-Treibern für virtuelle Computer der N-Serie unter Linux

Installieren Sie auf jedem virtuellen Computer NVIDIA-Grafiktreiber, um die GPU-Funktionen von virtuellen Azure-Computern der N-Serie unter Linux nutzen zu können. In diesem Artikel werden die Treiberinstallationsschritte beschrieben, die Sie nach der Bereitstellung eines virtuellen Computers der N-Serie ausführen müssen. Informationen zur Einrichtung von Treibern stehen auch für [Windows-VMs](../windows/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) zur Verfügung.


Informationen zu Spezifikationen von virtuellen Computern der N-Serie, Speicherkapazitäten und Details zu den Datenträgern finden Sie unter [GPU-optimierte Größen von virtuellen Linux-Computern](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 



## <a name="supported-distributions-and-drivers"></a>Unterstützte Verteilungen und Treiber

> [!IMPORTANT]
> Derzeit ist eine Linux-GPU-Treiberunterstützung nur auf virtuellen Azure NC-Computern verfügbar. 

Die folgenden Verteilungen aus dem Azure Marketplace werden zur Ausführung von NVIDIA-Grafiktreibern auf virtuellen Linux-Computern der N-Serie unterstützt.

### <a name="nc-vms-tesla-k80-card"></a>Virtuelle NC-Computer (Tesla K80-Karte)
* Ubuntu 16.04 LTS 
* Red Hat Enterprise Linux 7.3 
* 7.3 (CentOS-basiert) 

**Unterstützte Treiber**: NVIDIA CUDA 8.0, Branch R375. [Installationsschritte](#install-CUDA-drivers-for-NC-VMs)




> [!WARNING] 
> Die Installation von Drittanbietersoftware auf Red Hat-Produkten kann Auswirkungen auf die Red Hat-Supportbedingungen haben. Weitere Informationen hierzu finden Sie im [Red Hat-Knowledgebase-Artikel](https://access.redhat.com/articles/1067).
>




## <a name="install-cuda-drivers-for-nc-vms"></a>Installieren von CUDA-Treibern für virtuelle NC-Computer

Hier werden Schritte zum Installieren von NVIDIA-Treibern auf Linux-NC-VMs vom NVIDIA CUDA Toolkit 8.0 aus beschrieben. 

C- und C++-Entwickler können optional das vollständige Toolkit zum Erstellen GPU-beschleunigter Anwendungen installieren. Weitere Informationen finden Sie im [CUDA-Installationshandbuch](http://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html).


> [!NOTE]
> Die hier bereitgestellten Links zum Herunterladen der CUDA-Treiber sind zum Zeitpunkt der Veröffentlichung aktuell. Die neuesten Treiber finden Sie auf der [NVIDIA](http://www.nvidia.com/)-Website.
>

Um das CUDA-Toolkit installieren zu können, stellen Sie eine SSH-Verbindung mit jeder VM her. Führen Sie den folgenden Befehl aus, um sicherzustellen, dass das System über eine CUDA-fähige GPU verfügt:

```bash
lspci | grep -i NVIDIA
```
Die Ausgabe sieht in etwa wie das folgende Beispiel aus (zeigt eine NVIDIA Tesla K80-Karte):

![lspci-Befehlsausgabe](./media/n-series-driver-setup/lspci.png)

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

### <a name="centos-73-or-red-hat-enterprise-linux-73"></a>CentOS 7.3 oder Red Hat Enterprise Linux 7.3

> [!IMPORTANT] 
> NVIDIA CUDA-Treiber können auf virtuellen NC24r-Computern unter CentOS 7.3 oder Red Hat Enterprise Linux 7.3 aufgrund eines bekannten Problems nicht installiert werden.
>

Rufen Sie zuerst Updates ab. 

```bash
sudo yum update

sudo reboot
```

Stellen Sie die Verbindung mit dem virtuellen Computer wieder her, und setzen Sie die Installation mit den folgenden Befehlen fort:

```bash
sudo yum install kernel-devel

sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

sudo yum install dkms

CUDA_REPO_PKG=cuda-repo-rhel7-8.0.61-1.x86_64.rpm

wget http://developer.download.nvidia.com/compute/cuda/repos/rhel7/x86_64/${CUDA_REPO_PKG} -O /tmp/${CUDA_REPO_PKG}

sudo rpm -ivh /tmp/${CUDA_REPO_PKG}

rm -f /tmp/${CUDA_REPO_PKG}

sudo yum install cuda-drivers
```

Die Installation kann einige Minuten dauern. Um das vollständige CUDA-Toolkit optional zu installieren, geben Sie Folgendes ein:

```bash
sudo yum install cuda
```

Starten Sie die VM neu, und fahren Sie mit der Überprüfung der Installation fort.


### <a name="verify-driver-installation"></a>Überprüfen der Treiberinstallation


Stellen Sie zum Abfragen des GPU-Gerätestatus eine SSH-Verbindung mit der VM her, und führen Sie das mit dem Treiber installierte Befehlszeilen-Hilfsprogramm [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) aus. 

Daraufhin wird eine Ausgabe angezeigt, die in etwa wie folgt aussieht:

![NVIDIA-Gerätestatus](./media/n-series-driver-setup/smi.png)


### <a name="cuda-driver-updates"></a>CUDA-Treiberupdates

Sie sollten CUDA-Treiber nach der Bereitstellung in regelmäßigen Abständen aktualisieren.

#### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS

```bash
sudo apt-get update

sudo apt-get upgrade -y

sudo apt-get dist-upgrade -y

sudo apt-get install cuda-drivers
```

Nachdem das Update abgeschlossen ist, starten Sie die VM neu.

#### <a name="centos-73-or-red-hat-enterprise-linux-73"></a>CentOS 7.3 oder Red Hat Enterprise Linux 7.3

```bash
sudo yum update
```

Nachdem das Update abgeschlossen ist, starten Sie die VM neu.



## <a name="troubleshooting"></a>Problembehandlung

* Es besteht ein bekanntes Problem bei CUDA-Treibern auf virtuellen Azure-Computern der N-Serie, auf denen Linux Kernel 4.4.0-75 unter Ubuntu 16.04 LTS ausgeführt wird. Um die Treiberfunktion bei einem Upgrade des Kernels aufrechtzuerhalten, müssen Sie mindestens auf Kernelversion 4.4.0-77 aktualisieren. 



## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu den NVIDIA GPUs auf der N-Serie VMs finden Sie unter:
    * [NVIDIA Tesla K80](http://www.nvidia.com/object/tesla-k80.html) (für virtuelle Azure NC-Computer)
    * [NVIDIA Tesla M60](http://www.nvidia.com/object/tesla-m60.html) (für virtuelle Azure NV-Computer)

* Wenn Sie ein Linux-VM-Image mit den installierten NVIDIA-Treibern erfassen möchten, lesen Sie [Generalisieren und Erfassen eines virtuellen Linux-Computers](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
