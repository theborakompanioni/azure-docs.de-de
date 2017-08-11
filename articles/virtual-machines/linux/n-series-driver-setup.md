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
ms.date: 07/25/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: 54774252780bd4c7627681d805f498909f171857
ms.openlocfilehash: bdeb4d5ca1d9ff4d7dfd0961690412dd7530572a
ms.contentlocale: de-de
ms.lasthandoff: 07/27/2017

---

# <a name="install-nvidia-gpu-drivers-on-n-series-vms-running-linux"></a>Installieren von NVIDIA GPU-Treibern für virtuelle Computer der Serie N mit Linux

Installieren Sie unterstützte NVIDIA-Grafiktreiber, um die GPU-Funktionen von virtuellen Azure-Computern der N-Serie unter Linux nutzen zu können. In diesem Artikel werden die Treiberinstallationsschritte beschrieben, die Sie nach der Bereitstellung eines virtuellen Computers der N-Serie ausführen müssen. Informationen zur Einrichtung von Treibern stehen auch für [Windows-VMs](../windows/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) zur Verfügung.


Informationen zu Spezifikationen von virtuellen Computern der N-Serie, Speicherkapazitäten und Details zu den Datenträgern finden Sie unter [GPU-optimierte Größen von virtuellen Linux-Computern](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 



[!INCLUDE [virtual-machines-n-series-linux-support](../../../includes/virtual-machines-n-series-linux-support.md)]

## <a name="install-grid-drivers-for-nv-vms"></a>GRID-Treiber für virtuelle NV-Computer

Stellen Sie zum Installieren von NVIDIA GRID-Treibern auf virtuellen NV-Computern eine Verbindung zu jedem virtuellen Computer her, und folgen Sie dann den Schritten für Ihre Linux-Verteilung. 

### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS

1. Führen Sie den Befehl `lspci` aus. Überprüfen Sie, ob die NVIDIA M60-Karte bzw. -Karten als PCI-Geräte angezeigt werden.

2. Installieren Sie die Updates.

  ```bash
  sudo apt-get update

  sudo apt-get upgrade -y

  sudo apt-get dist-upgrade -y

  sudo apt-get install build-essential ubuntu-desktop -y
  ```
3. Deaktivieren Sie den Nouveau-Kerneltreiber, da er nicht mit dem NVIDIA-Treiber kompatibel ist. (Verwenden Sie den NVIDIA-Treiber nur auf virtuellen NV-Computern.) Erstellen Sie zu diesem Zweck eine Datei in `/etc/modprobe.d `, und nennen Sie sie `nouveau.conf`. Die Datei muss den folgenden Inhalt enthalten:

  ```
  blacklist nouveau

  blacklist lbm-nouveau
  ```


4. Starten Sie den virtuellen Computer neu, und stellen Sie erneut eine Verbindung her. Exit X-Server:

  ```bash
  sudo systemctl stop lightdm.service
  ```

5. Laden Sie den GRID-Treiber herunter, und installieren Sie ihn:

  ```bash
  wget -O NVIDIA-Linux-x86_64-367.106-grid.run https://go.microsoft.com/fwlink/?linkid=849941  

  chmod +x NVIDIA-Linux-x86_64-367.106-grid.run

  sudo ./NVIDIA-Linux-x86_64-367.106-grid.run
  ``` 

6. Wenn Sie gefragt werden, ob Sie mit dem Hilfsprogramm „nvidia-xconfig“ Ihre X-Konfigurationsdatei aktualisieren möchten, klicken Sie auf **Ja**.

7. Nachdem die Installation abgeschlossen ist, kopieren Sie „/etc/nvidia/gridd.conf.template“ in eine neue Datei „gridd.conf at location /etc/nvidia/“.

  ```bash
  sudo cp /etc/nvidia/gridd.conf.template /etc/nvidia/gridd.conf
  ```

8. Fügen Sie Folgendes zu `/etc/nvidia/gridd.conf` hinzu:
 
  ```
  IgnoreSP=TRUE
  ```
9. Starten Sie die VM neu, und fahren Sie mit der Überprüfung der Installation fort.


### <a name="centos-based-73-or-red-hat-enterprise-linux-73"></a>CentOS-basiertes 7.3 oder Red Hat Enterprise Linux 7.3


1. Aktualisieren Sie den Kernel und DKMS.
 
  ```bash  
  sudo yum update
 
  sudo yum install kernel-devel
 
  sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
 
  sudo yum install dkms
  ```

2. Deaktivieren Sie den Nouveau-Kerneltreiber, da er nicht mit dem NVIDIA-Treiber kompatibel ist. (Verwenden Sie den NVIDIA-Treiber nur auf virtuellen NV-Computern.) Erstellen Sie zu diesem Zweck eine Datei in `/etc/modprobe.d `, und nennen Sie sie `nouveau.conf`. Die Datei muss den folgenden Inhalt enthalten:

  ```
  blacklist nouveau

  blacklist lbm-nouveau
  ```
 
3. Starten Sie den virtuellen Computer neu, und installieren Sie die aktuellsten Linux-Integrationsdienste für Hyper-V:
 
  ```bash
  wget http://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.2-2.tar.gz
 
  tar xvzf lis-rpms-4.2.2-2.tar.gz
 
  cd LISISO
 
  sudo ./install.sh
 
  sudo reboot
  ```
 
4. Stellen Sie erneut eine Verbindung zum virtuellen Computer her, und führen Sie den Befehl `lspci` aus. Überprüfen Sie, ob die NVIDIA M60-Karte bzw. -Karten als PCI-Geräte angezeigt werden.
 
5. Laden Sie den GRID-Treiber herunter, und installieren Sie ihn:

  ```bash
  wget -O NVIDIA-Linux-x86_64-367.106-grid.run https://go.microsoft.com/fwlink/?linkid=849941  

  chmod +x NVIDIA-Linux-x86_64-367.106-grid.run

  sudo ./NVIDIA-Linux-x86_64-367.106-grid.run
  ``` 
6. Wenn Sie gefragt werden, ob Sie mit dem Hilfsprogramm „nvidia-xconfig“ Ihre X-Konfigurationsdatei aktualisieren möchten, klicken Sie auf **Ja**.

7. Nachdem die Installation abgeschlossen ist, kopieren Sie „/etc/nvidia/gridd.conf.template“ in eine neue Datei „gridd.conf at location /etc/nvidia/“.
  
  ```bash
  sudo cp /etc/nvidia/gridd.conf.template /etc/nvidia/gridd.conf
  ```
  
8. Fügen Sie Folgendes zu `/etc/nvidia/gridd.conf` hinzu:
 
  ```
  IgnoreSP=TRUE
  ```
9. Starten Sie die VM neu, und fahren Sie mit der Überprüfung der Installation fort.

### <a name="verify-driver-installation"></a>Überprüfen der Treiberinstallation


Stellen Sie zum Abfragen des GPU-Gerätestatus eine SSH-Verbindung mit der VM her, und führen Sie das mit dem Treiber installierte Befehlszeilen-Hilfsprogramm [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) aus. 

Daraufhin wird eine Ausgabe angezeigt, die in etwa wie folgt aussieht:

![NVIDIA-Gerätestatus](./media/n-series-driver-setup/smi-nv.png)
 

### <a name="x11-server"></a>X11-Server
Wenn Sie einen X11-Server für Remoteverbindungen an einen virtuellen NV-Computer benötigen, wird [x11vnc](http://www.karlrunge.com/x11vnc/) empfohlen, da es die Grafikhardwarebeschleunigung ermöglicht. Die Bus-ID des M60-Geräts muss manuell zur xconfig-Datei hinzugefügt werden (`etc/X11/xorg.conf` auf Ubuntu 16.04 LTS, `/etc/X11/XF86config` auf CentOS 7.3 und Red Hat Enterprise Server 7.3). Fügen Sie einen `"Device"`-Abschnitt analog zum folgenden Abschnitt hinzu:
 
```
Section "Device"
    Identifier     "Device0"
    Driver         "nvidia"
    VendorName     "NVIDIA Corporation"
    BoardName      "Tesla M60"
    BusID          "your-BusID:0:0:0"
EndSection
```
 
Aktualisieren Sie darüber hinaus Ihren `"Screen"`-Abschnitt, um dieses Gerät zu verwenden.
 
Die Bus-ID ergibt sich durch die Ausführung von

```bash
/usr/bin/nvidia-smi --query-gpu=pci.bus_id --format=csv | tail -1 | cut -d ':' -f 1
```
 
Die Bus-ID kann sich ändern, wenn ein virtueller Computer neu zugewiesen oder neu gestartet wird. Wenn ein virtueller Computer neu gestartet wird, müssen Sie daher in der X11-Konfiguration zum Aktualisieren der Bus-ID ein Skript verwenden. Beispiel:

```bash 
#!/bin/bash
BUSID=$((16#`/usr/bin/nvidia-smi --query-gpu=pci.bus_id --format=csv | tail -1 | cut -d ':' -f 1`))

if grep -Fxq "${BUSID}" /etc/X11/XF86Config; then     echo "BUSID is matching"; else   echo "BUSID changed to ${BUSID}" && sed -i '/BusID/c\    BusID          \"PCI:0@'${BUSID}':0:0:0\"' /etc/X11/XF86Config; fi
```

Diese Datei kann beim Systemstart als Stamm aufgerufen werden, indem in `/etc/rc.d/rc3.d` ein entsprechender Eintrag erstellt wird.


## <a name="install-cuda-drivers-for-nc-vms"></a>Installieren von CUDA-Treibern für virtuelle NC-Computer

Hier werden Schritte zum Installieren von NVIDIA-Treibern auf Linux-NC-VMs vom NVIDIA CUDA Toolkit 8.0 aus beschrieben. 

C- und C++-Entwickler können optional das vollständige Toolkit zum Erstellen GPU-beschleunigter Anwendungen installieren. Weitere Informationen finden Sie im [CUDA-Installationshandbuch](http://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html).


> [!NOTE]
> Die hier bereitgestellten Links zum Herunterladen der CUDA-Treiber sind zum Zeitpunkt der Veröffentlichung aktuell. Die aktuellsten CUDA-Treiber finden Sie auf der [NVIDIA](http://www.nvidia.com/)-Website.
>

Um das CUDA-Toolkit installieren zu können, stellen Sie eine SSH-Verbindung mit jeder VM her. Führen Sie den folgenden Befehl aus, um sicherzustellen, dass das System über eine CUDA-fähige GPU verfügt:

```bash
lspci | grep -i NVIDIA
```
Die Ausgabe sieht in etwa wie das folgende Beispiel aus (zeigt eine NVIDIA Tesla K80-Karte):

![lspci-Befehlsausgabe](./media/n-series-driver-setup/lspci.png)

Führen Sie dann die für Ihre Verteilung spezifischen Installationsbefehle aus.

### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS

1. Laden Sie die CUDA-Treiber herunter, und installieren Sie sie.
  ```bash
  CUDA_REPO_PKG=cuda-repo-ubuntu1604_8.0.61-1_amd64.deb

  wget -O /tmp/${CUDA_REPO_PKG} http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/${CUDA_REPO_PKG} 

  sudo dpkg -i /tmp/${CUDA_REPO_PKG}

  rm -f /tmp/${CUDA_REPO_PKG}

  sudo apt-get update

  sudo apt-get install cuda-drivers

  ```

  Die Installation kann einige Minuten dauern.

2. Um das vollständige CUDA-Toolkit optional zu installieren, geben Sie Folgendes ein:

  ```bash
  sudo apt-get install cuda
  ```

3. Starten Sie die VM neu, und fahren Sie mit der Überprüfung der Installation fort.

### <a name="centos-based-73-or-red-hat-enterprise-linux-73"></a>CentOS-basiertes 7.3 oder Red Hat Enterprise Linux 7.3

1. Rufen Sie die Updates ab. 

  ```bash
  sudo yum update

  sudo reboot
  ```
2. Stellen Sie wieder eine Verbindung mit dem virtuellen Computer her, und installieren Sie die aktuellsten Linux-Integrationsdienste für Hyper-V.

  > [!IMPORTANT]
  > Wenn Sie ein auf CentOS basiertes HPC-Image auf einem virtuellen NC24r-Computer installiert haben, fahren Sie mit Schritt 3 fort. Da Azure RDMA-Treiber und Linux-Integrationsdienste im Image bereits installiert sind, muss LIS nicht aktualisiert werden, und Kernel-Updates sind standardmäßig deaktiviert.
  >

  ```bash
  wget http://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.1.tar.gz
 
  tar xvzf lis-rpms-4.2.1.tar.gz
 
  cd LISISO
 
  sudo ./install.sh
 
  sudo reboot
  ```
 
3. Stellen Sie die Verbindung mit dem virtuellen Computer wieder her, und setzen Sie die Installation mit den folgenden Befehlen fort:

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

  Die Installation kann einige Minuten dauern. 

4. Um das vollständige CUDA-Toolkit optional zu installieren, geben Sie Folgendes ein:

  ```bash
  sudo yum install cuda
  ```

5. Starten Sie die VM neu, und fahren Sie mit der Überprüfung der Installation fort.


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

sudo reboot
```


#### <a name="centos-based-73-or-red-hat-enterprise-linux-73"></a>CentOS-basiertes 7.3 oder Red Hat Enterprise Linux 7.3

```bash
sudo yum update

sudo reboot
```



## <a name="troubleshooting"></a>Problembehandlung

* Es besteht ein bekanntes Problem bei CUDA-Treibern auf virtuellen Azure-Computern der N-Serie, auf denen Linux Kernel 4.4.0-75 unter Ubuntu 16.04 LTS ausgeführt wird. Wenn Sie von einer früheren Kernelversion aktualisieren, aktualisieren Sie mindestens auf die Kernelversion 4.4.0-77. 



## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu den NVIDIA GPUs auf der N-Serie VMs finden Sie unter:
    * [NVIDIA Tesla K80](http://www.nvidia.com/object/tesla-k80.html) (für virtuelle Azure NC-Computer)
    * [NVIDIA Tesla M60](http://www.nvidia.com/object/tesla-m60.html) (für virtuelle Azure NV-Computer)

* Wenn Sie ein Linux-VM-Image mit den installierten NVIDIA-Treibern erfassen möchten, lesen Sie [Generalisieren und Erfassen eines virtuellen Linux-Computers](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

