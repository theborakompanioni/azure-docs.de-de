---
title: Optimieren des VM-Netzwerkdurchsatzes | Microsoft-Dokumentation
description: Informationen zum Optimieren des Netzwerkdurchsatzes virtueller Azure-Computer
services: virtual-network
documentationcenter: na
author: steveesp
manager: Gerald DeGrace
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/01/2017
ms.author: steveesp
translationtype: Human Translation
ms.sourcegitcommit: 6af1fdb533e0c5da56232b1aa2d9b8d1c08ad9fd
ms.openlocfilehash: eb3c64fa467a3dcd90b6b240b61816522534cb68


---

# <a name="optimize-network-throughput-for-azure-virtual-machines"></a>Optimieren des Netzwerkdurchsatzes für virtuelle Azure-Computer

Virtuelle Azure-Computer weisen standardmäßige Netzwerkeinstellungen auf, die zur Steigerung des Netzwerkdurchsatzes weiter optimiert werden können. Dieser Artikel beschreibt die Optimierung des Netzwerkdurchsatzes für virtuelle Microsoft Azure-Computer unter Windows und Linux, einschließlich der wichtigsten Versionen, wie etwa Ubuntu, CentOS und Red Hat.

## <a name="windows-vm"></a>Windows-VM

Ein virtueller Computer, der die empfangsseitige Skalierung (Receive Side Scaling; RSS) verwendet, kann einen höheren maximalen Durchsatz als ein virtueller Computer ohne RSS erreichen. RSS kann auf virtuellen Windows-Computern standardmäßig deaktiviert sein. Führen Sie die folgenden Schritte aus, um zu bestimmen, ob RSS aktiviert ist und um sie ggf. zu aktivieren.

1. Geben Sie den PowerShell-Befehl `Get-NetAdapterRss` ein, um anzuzeigen, ob RSS für ein Netzwerk aktiviert ist. In der folgenden Beispielausgabe aus `Get-NetAdapterRss` ist RSS nicht aktiviert.

    ```powershell
    Name                    :Ethernet
    InterfaceDescription            : Microsoft Hyper-V Network Adapter
    Enabled                 : False
    ```
2. Geben Sie den folgenden Befehl zum Aktivieren von RSS ein:

    ```powershell
    Get-NetAdapter | % {Enable-NetAdapterRss -Name \$\_.Name}
    ```
    Der vorherige Befehl weist keine Ausgabe auf. Mit dem Befehl wurden die NIC-Einstellungen geändert und dadurch etwa eine Minute lang vorübergehende Konnektivitätsverluste verursacht. Während des Konnektivitätsverlusts wird das Dialogfeld „Verbindung wird wiederhergestellt“ angezeigt. In der Regel ist die Konnektivität nach dem dritten Versuch wiederhergestellt.
3. Bestätigen Sie, dass RSS auf dem virtuellen Computer aktiviert ist, indem Sie den Befehl `Get-NetAdapterRss` erneut eingeben. Bei Erfolg wird die folgende Beispielausgabe zurückgegeben:

    ```powershell
    Name                    :Ethernet
    InterfaceDescription            : Microsoft Hyper-V Network Adapter
    Enabled                 : True
    ```

## <a name="linux-vm"></a>Linux-VM

RSS ist auf einem virtuellen Azure Linux-Computer immer standardmäßig aktiviert. Linux-Kernel, die seit Januar 2017 veröffentlicht wurden, enthalten neue Netzwerkoptimierungsoptionen, die einem virtuellen Linux-Computer einen höheren Netzwerkdurchsatz ermöglichen.

### <a name="ubuntu"></a>Ubuntu

Zur Optimierung führen Sie zuerst ein Update auf die neueste unterstützte Version aus (Stand Januar 2017):

„Publisher“: „Canonical“, „Offer“: „UbuntuServer“, „Sku“: „16.04.0-LTS“, „Version“: „16.04.201609071“

Nach dem Update geben Sie die folgenden Befehle zum Abrufen des neuesten Kernels ein:

```bash
apt-get -f install
apt-get --fix-missing install
apt-get clean
apt-get -y update
apt-get -y upgrade
```

Optionaler Befehl:

`apt-get -y dist-upgrade`

### <a name="centos"></a>CentOS

Zur Optimierung führen Sie zuerst ein Update auf die neueste unterstützte Version aus (Stand Januar 2017):

„Publisher“: „OpenLogic“, „Offer“: „CentOS“,* „Sku“: „7.3“, „Version“: „latest“

Nach dem Update installieren Sie die neuesten Linux Integration Services (LIS).
Die Optimierung des Durchsatzes erfolgt in LIS, beginnend mit 4.1.3. Geben Sie den folgenden Befehl für die Installation von LIS ein:

```bash
sudo yum install microsoft-hyper-v-4.1.3 kmod-microsoft-hyper-v-4.1.3*
```

### <a name="red-hat"></a>Red Hat

Zur Optimierung führen Sie zuerst ein Update auf die neueste unterstützte Version aus (Stand Januar 2017):

„Publisher“: „RedHat“ „Offer“: „RHEL“ „Sku“: „7.3“ „Version“: „7.3.20161104“

Nach dem Update installieren Sie die neuesten Linux Integration Services (LIS).
Die Optimierung des Durchsatzes erfolgt in LIS, beginnend mit 4.1.3. Geben Sie die folgenden Befehle ein, um LIS herunterzuladen und zu installieren:

```bash
mkdir lis4.1.3
cd lis4.1.3
wget https://download.microsoft.com/download/7/6/B/76BE7A6E-E39F-436C-9353-F4B44EF966E9/lis-rpms-4.1.3.tar.gz
tar xvzf lis-rpms-4.1.3.tar.gz
cd LISISO
install.sh  \#or upgrade.sh if previous LIS was previously installed
```

Weitere Informationen zu Linux Integration Services Version 4.1 für Hyper-V erhalten Sie auf der [Downloadseite](https://www.microsoft.com/download/details.aspx?id=51612).



<!--HONumber=Feb17_HO1-->


