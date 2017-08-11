---
title: Verwenden rechenintensiver Azure-VMs mit Batch | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie die Vorteile RDMA-fähiger oder GPU-fähiger VM-Größen in Azure Batch-Pools nutzen."
services: batch
documentationcenter: 
author: dlepow
manager: timlt
editor: 
ms.assetid: 
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: danlep
ms.translationtype: HT
ms.sourcegitcommit: 137671152878e6e1ee5ba398dd5267feefc435b7
ms.openlocfilehash: c52a054e4fc8f61f871acd9f35b9a3e6247e48ef
ms.contentlocale: de-de
ms.lasthandoff: 07/28/2017

---
# <a name="use-rdma-capable-or-gpu-enabled-instances-in-batch-pools"></a>Verwenden RDMA-fähiger oder GPU-fähiger Instanzen in Batch-Pools

Für die Ausführung bestimmter Batch-Aufträge können Sie die Vorteile der Azure-VM-Größen nutzen, die speziell für umfangreiche Berechnungen entwickelt wurden. Wenn Sie beispielsweise [MPI-Workloads](batch-mpi.md) mit mehreren Instanzen ausführen, können Sie die Größen A8, A9 oder die H-Serie auswählen, die jeweils über eine Netzwerkschnittstelle für RDMA (Remote Direct Memory Access) verfügen. Diese Größen stellen für die knotenübergreifende Kommunikation eine Verbindung mit einem InfiniBand-Netzwerk her, um so MPI-Anwendungen zu beschleunigen. Bei CUDA-Anwendungen können Sie außerdem Größen der N-Serie auswählen, die NVIDIA Tesla-GPU-Karten (Graphics Processing Unit) enthalten.

Dieser Artikel enthält Anweisungen und Anwendungsbeispiele für einige spezielle Größen in Azure für Batch-Pools. Technische Daten und Hintergrundinformationen finden Sie unter:

* Größen von virtuellen HPC-Computern (High Performance Computing) – [Linux](../virtual-machines/linux/sizes-hpc.md), [Windows](../virtual-machines/windows/sizes-hpc.md) 

* GPU-fähige VM-Größen ([Linux](../virtual-machines/linux/sizes-gpu.md), [Windows](../virtual-machines/windows/sizes-gpu.md)) 


## <a name="subscription-and-account-limits"></a>Abonnements und Kontoeinschränkungen

* **Kontingente:** Die Anzahl der Knoten, die Sie einem Batch-Pool hinzufügen können, kann durch ein oder mehrere Azure-Kontingenten eingeschränkt werden. Besonders wahrscheinlich sind solche Einschränkungen bei einer Auswahl von RDMA-fähigen, GPU-fähigen oder sonstigen Größen für virtuelle Computer mit mehreren Kernen. Die Kontingente können je nach Art des erstellten Batch-Kontos für das Konto selbst oder für Ihr Abonnement gelten.

    * Wenn Sie Ihr Batch-Konto in der Konfiguration **Batch-Dienst** erstellt haben, gilt als Einschränkung das [dedizierte Kernekontingent pro Batch-Konto](batch-quota-limit.md#resource-quotas). Dieses Kontingent beträgt standardmäßig 20 Kerne. Ein eigenes Kontingent gilt für [VMs mit niedriger Priorität](batch-low-pri-vms.md), sofern Sie diese verwenden. 

    * Wenn Sie das Konto in der Konfiguration **Benutzerabonnement** erstellt haben, beschränkt Ihr Abonnement die Anzahl der VM-Kerne pro Region. Weitere Informationen finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md). Für Ihr Abonnement gilt auch ein regionsbezogenes Kontingent für bestimmte VM-Größen, einschließlich HPC- und GPU-Instanzen. In der Konfiguration „Benutzerabonnement“ gelten keine zusätzlichen Kontingente für das Batch-Konto. 

  Sie müssen u.U. Kontingente erhöhen, wenn Sie in Batch eine sehr spezialisierte VM-Größe verwenden. In diesem Fall können Sie kostenlos [eine Anfrage an den Onlinekundensupport richten](../azure-supportability/how-to-create-azure-support-request.md) und eine Erhöhung des Kontingents anfordern.

* **Regionale Verfügbarkeit:** Rechenintensive virtuelle Computer sind möglicherweise nicht in den Regionen verfügbar, in denen Sie die Batch-Konten erstellen. Informationen dazu, welche Größen verfügbar sind, finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/regions/services/).


## <a name="dependencies"></a>Abhängigkeiten

Die RDMA- und GPU-Funktionen rechenintensiver Größen werden nur unter bestimmten Betriebssystemen unterstützt. Je nach Betriebssystem müssen Sie eventuell zusätzliche Treiber und andere Software installieren oder konfigurieren. In den folgenden Tabellen werden diese Abhängigkeiten zusammengefasst. Einzelheiten finden Sie in den verlinkten Artikeln. Optionen zum Konfigurieren von Batch-Pools finden Sie weiter unten in diesem Artikel.


### <a name="linux-pools---virtual-machine-configuration"></a>Linux-Pools – Konfiguration „Virtueller Computer“

| Größe | Funktion | Betriebssysteme | Erforderliche Software | Pooleinstellungen |
| -------- | -------- | ----- |  -------- | ----- |
| [H16r, H16mr, A8, A9](../virtual-machines/linux/sizes-hpc.md#rdma-capable-instances) | RDMA | SUSE Linux Enterprise Server 12 HPC oder<br/>CentOS-basierter HPC<br/>(Azure Marketplace) | Intel MPI 5 | Knotenübergreifende Kommunikation aktivieren, parallele Taskausführung deaktivieren |
| [NC-Serie*](../virtual-machines/linux/n-series-driver-setup.md#install-cuda-drivers-for-nc-vms) | NVIDIA Tesla K80 GPU | Ubuntu 16.04 LTS.<br/>Red Hat Enterprise Linux 7.3 oder<br/>7.3 (CentOS-basiert)<br/>(Azure Marketplace) | NVIDIA CUDA Toolkit 8.0-Treiber | N/V | 
| [NV-Serie](../virtual-machines/linux/n-series-driver-setup.md#install-grid-drivers-for-nv-vms) | NVIDIA Tesla M60 GPU | Ubuntu 16.04 LTS<br/>Red Hat Enterprise Linux 7.3<br/>7.3 (CentOS-basiert)<br/>(Azure Marketplace) | NVIDIA GRID 4.3-Treiber | N/V |

* RDMA-Konnektivität auf NC24r-VMs wird auf CentOS-basierten 7.3 HPC mit Intel MPI unterstützt.



### <a name="windows-pools---virtual-machine-configuration"></a>Windows-Pools – Konfiguration „Virtueller Computer“

| Größe | Funktion | Betriebssysteme | Erforderliche Software | Pooleinstellungen |
| -------- | ------ | -------- | -------- | ----- |
| [H16r, H16mr, A8, A9](../virtual-machines/windows/sizes-hpc.md#rdma-capable-instances) | RDMA | Windows Server 2012 R2 oder<br/>Windows Server 2012 (Azure Marketplace) | Microsoft MPI 2012 R2 oder höher oder<br/> Intel MPI 5<br/><br/>Azure-VM-Erweiterung HpcVMDrivers | Knotenübergreifende Kommunikation aktivieren, parallele Taskausführung deaktivieren |
| [NC-Serie*](../virtual-machines/windows/n-series-driver-setup.md) | NVIDIA Tesla K80 GPU | Windows Server 2016 oder <br/>Windows Server 2012 R2 (Azure Marketplace) | NVIDIA Tesla-Treiber oder CUDA Toolkit 8.0-Treiber| N/V | 
| [NV-Serie](../virtual-machines/windows/n-series-driver-setup.md) | NVIDIA Tesla M60 GPU | Windows Server 2016 oder<br/>Windows Server 2012 R2 (Azure Marketplace) | NVIDIA GRID 4.3-Treiber | N/V |

* RDMA-Konnektivität auf NC24r-VMs wird unter Windows Server 2012 R2 mit der Erweiterung HpcVMDrivers und Microsoft MPI oder Intel MPI unterstützt.

### <a name="windows-pools---cloud-services-configuration"></a>Windows-Pools – Konfiguration „Clouddienst“

> [!NOTE]
> Größen der N-Serie werden in Batch-Pools mit der Clouddienstkonfiguration nicht unterstützt.
>

| Größe | Funktion | Betriebssysteme | Erforderliche Software | Pooleinstellungen |
| -------- | ------- | -------- | -------- | ----- |
| [H16r, H16mr, A8, A9](../virtual-machines/windows/sizes-hpc.md#rdma-capable-instances) | RDMA | Windows Server 2012 R2,<br/>Windows Server 2012 oder<br/>Windows Server 2008 R2 (Gastbetriebssystemfamilie) | Microsoft MPI 2012 R2 oder höher oder<br/>Intel MPI 5<br/><br/>Azure-VM-Erweiterung HpcVMDrivers | Kommunikation zwischen Knoten aktivieren,<br/> parallele Taskausführung deaktivieren |





## <a name="pool-configuration-options"></a>Poolkonfigurationsoptionen

Die Batch-APIs und -Tools bietet eine Reihe von Optionen für die Installation der erforderlichen Software oder Treiber für das Konfigurieren einer speziellen VM-Größe für den Batch-Pool, einschließlich:

* [Startaufgabe:](batch-api-basics.md#start-task) Laden Sie ein Installationspaket als eine Ressourcendatei in ein Azure Storage-Konto in derselben Region wie das Batch-Konto hoch. Erstellen Sie eine Befehlszeile für die Startaufgabe, um die Ressourcendatei im Hintergrund zu installieren, wenn der Pool gestartet wird. Weitere Informationen finden Sie in der [REST-API-Dokumentation](/rest/api/batchservice/add-a-pool-to-an-account#bk_starttask).

  > [!NOTE] 
  > Die Startaufgabe muss mit erhöhten Rechten (Administrator) ausgeführt werden und die erfolgreiche Ausführung abwarten.
  >

* [Anwendungspaket:](batch-application-packages.md) Fügen Sie ein ZIP-Installationspaket für das Batch-Konto hinzu, und konfigurieren Sie einen Paketverweis im Pool. Diese Einstellung lädt das Paket auf alle Knoten im Pool hoc und entzippt es. Wenn das Paket ein Installationsprogramm ist, erstellen Sie eine Befehlszeile für die Startaufgabe, mit der die App im Hintergrund auf allen Knoten im Pool installiert wird. Installieren Sie optional das Paket, wenn eine Aufgabe für die Ausführung auf einem Knoten geplant ist.

* [Benutzerdefiniertes Poolimage:](batch-api-basics.md#pool) Erstellen Sie ein benutzerdefiniertes Windows- oder Linux-VM-Image, das Treiber, Software und andere erforderliche Einstellungen für die Größe des virtuellen Computers enthält. Wenn Sie das Batch-Konto in der Konfiguration „Benutzerabonnement“ erstellt haben, geben Sie das benutzerdefinierte Image für den Batch-Pool an. (Benutzerdefinierte Images werden in Konten mit der Konfiguration „Batch-Dienst“ nicht unterstützt.) Benutzerdefinierte Images können nur für Pools mit der Konfiguration „Virtueller Computer“ verwendet werden.

  > [!IMPORTANT]
  > In Batch-Pools können Sie derzeit keine benutzerdefinierten Images verwenden, die mit Managed Disks oder mit Storage Premium erstellt wurden.
  >



* [Batch Shipyard](https://github.com/Azure/batch-shipyard) konfiguriert automatisch GPU und RDMA für die transparente Arbeit mit Containerworkloads in Azure Batch. Batch Shipyard wird vollständig über Konfigurationsdateien gesteuert. Es gibt eine Vielzahl von Beispielkonfigurationen, die GPU- und RDMA-Workloads ermöglichen, z.B. das [CNTK GPU Recipe](https://github.com/Azure/batch-shipyard/tree/master/recipes/CNTK-GPU-OpenMPI), das GPU-Treiber auf virtuellen Computern der N-Serie vorkonfiguriert und die Software des Microsoft Cognitive Toolkit als Docker-Image lädt.


## <a name="example-microsoft-mpi-on-an-a8-vm-pool"></a>Beispiel: Microsoft MPI in einem A8-VM-Pool

Um Windows-MPI-Anwendungen in einem Pool von Azure A8-Knoten auszuführen, müssen Sie eine unterstützte MPI-Implementierung installieren. Im Folgenden finden Sie eine exemplarische Vorgehensweise für das Installieren von [Microsoft MPI](https://msdn.microsoft.com/library/bb524831(v=vs.85).aspx) in einem Windows-Pool mit einem Batch-Anwendungspaket.

1. Laden Sie das [Installationspaket](http://go.microsoft.com/FWLink/p/?LinkID=389556) („MSMpiSetup.exe“) für die neueste Version von Microsoft MPI herunter.
2. Erstellen Sie eine ZIP-Datei des Pakets.
3. Laden Sie das Paket in Ihr Batch-Konto hoch. Anweisungen finden Sie in der Anleitung zum [Anwendungspaket](batch-application-packages.md). Geben Sie eine Anwendungs-ID (z.B. *MSMPI*) und eine Version (z.B. *8.1*) an. 
4. Erstellen Sie mit den Batch-APIs oder dem Azure-Portal einen Pool in der Konfiguration „Clouddienst“ mit der gewünschten Anzahl von Knoten und der gewünschten Skalierung. Die folgende Tabelle zeigt Beispieleinstellungen zum Einrichten von MPI im unbeaufsichtigten Modus mithilfe einer Startaufgabe:

| Einstellung | Wert |
| ---- | ----- | 
| **Imagetyp** | Cloud Services |
| **Betriebssystemfamilie** | Windows Server 2012 R2 (Betriebssystemfamilie 4) |
| **Knotengröße** | A8 Standard |
| **Kommunikation zwischen Knoten aktiviert** | true |
| **Max. Aufgaben pro Knoten** | 1 |
| **Anwendungspaketverweise** | MSMPI |
| **Startaufgabe aktiviert** | true<br>**Befehlszeile** - `"cmd /c %AZ_BATCH_APP_PACKAGE_MSMPI#8.1%\\MSMpiSetup.exe -unattend -force"`<br/>**Benutzeridentität:** autouser, admin für den Pool<br/>**Erfolg abwarten:** TRUE

## <a name="example-nvidia-tesla-drivers-on-nc-vm-pool"></a>Beispiel: NVIDIA Tesla-Treiber in NC-VM-Pool

Um CUDA Anwendungen in einem Pool von Linux-NC-Knoten auszuführen, müssen Sie das CUDA Toolkit 8.0 auf den Knoten installieren. Das Toolkit installiert die erforderlichen NVIDIA Tesla-GPU-Treiber. Im Folgenden finden Sie Beispielschritte zum Bereitstellen eines benutzerdefinierten Images mit Ubuntu 16.04 LTS mit GPU-Treibern:

1. Stellen Sie eine Azure-NC6-VM mit Ubuntu 16.04 LTS bereit. Sie können den virtuellen Computer beispielsweise in der Region „USA, Süden-Mitte“ erstellen. Erstellen Sie den virtuellen Computer unbedingt mit Storage Standard und *ohne* Managed Disks.
2. Befolgen Sie die Schritte zum Herstellen der Verbindung mit dem virtuellen Computer und zum [Installieren der CUDA-Treiber](../virtual-machines/linux/n-series-driver-setup.md#install-cuda-drivers-for-nc-vms).
3. Heben Sie die Bereitstellung des Linux-Agents auf, und erfassen Sie dann das Linux-VM-Image mithilfe von Befehlen der Azure CLI 1.0. Eine Anleitung finden Sie unter [Erfassen eines virtuellen Linux-Computers, der in Azure ausgeführt wird](../virtual-machines/linux/capture-image-nodejs.md). Notieren Sie sich den Image-URI.
  > [!IMPORTANT]
  > Verwenden Sie zum Erfassen des Images für Azure Batch keine Befehle von Azure CLI 2.0. Die CLI-2.0-Befehle erfassen derzeit nur virtuelle Computer, die mit Managed Disks erstellt wurden.
  >
4. Erstellen Sie ein Batch-Konto mit der Konfiguration „Benutzerabonnement“ in einer Region, die NC-VMs unterstützt.
5. Erstellen Sie mit den Batch-APIs oder dem Azure-Portal einen Pool mit dem benutzerdefinierten Image mit der gewünschten Anzahl von Knoten und der gewünschten Skalierung. Die folgende Tabelle enthält Beispielpooleinstellungen für das Image:

| Einstellung | Wert |
| ---- | ---- |
| **Imagetyp** | Benutzerdefiniertes Image |
| **Benutzerdefiniertes Image** | Image-URI der Form `https://yourstorageaccountdisks.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd` |
| **Knoten-Agent-SKU** | batch.node.ubuntu 16.04 |
| **Knotengröße** | NC6 Standard |



## <a name="next-steps"></a>Nächste Schritte

* Beispiele zur Ausführung von MPI-Aufträgen in einem Azure Batch-Pool stehen für [Windows](batch-mpi.md) und [Linux](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/) zur Verfügung.

* Beispiele für GPU-Workloads in Batch finden Sie in [Batch Shipyard](https://github.com/Azure/batch-shipyard/).
