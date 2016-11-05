---
title: Hinzufügen von Burstknoten zu einem HPC Pack-Cluster | Microsoft Docs
description: Erfahren Sie, wie Sie die HPC Pack-Clusterkapazität bei Bedarf erweitern können, indem Sie Workerrolleninstanzen in einem Clouddienst hinzufügen.
services: virtual-machines-windows
documentationcenter: ''
author: dlepow
manager: timlt
editor: ''
tags: azure-service-management,hpc-pack

ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 07/15/2016
ms.author: danlep

---
# <a name="add-on-demand-"burst"-nodes-to-an-hpc-pack-cluster-in-azure"></a>Hinzufügen von bedarfsgesteuerten „Burstknoten“ zu einem HPC Pack-Cluster in Azure
In diesem Artikel wird erläutert, wie Sie Azure-Burstknoten (Workerrolleninstanzen, die in einem Clouddienst ausgeführt werden) als Computeressourcen zu einem vorhandenen HPC Pack-Hauptknoten in Azure hinzufügen Mit einem Burstknoten können Sie die Computekapazität des HPC-Clusters in Azure bedarfsgesteuert zentral hoch- und herunterskalieren, ohne mehrere vorkonfigurierte VMs für Computeknoten verwalten zu müssen.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

![Burstknoten][burst]

Anhand der Schritte in diesem Artikel können Sie Azure-Knoten auf schnelle Weise zu einem cloudbasierten virtuellen Computer für den HPC Pack-Hauptknoten für Tests oder Machbarkeitsstudien hinzufügen. Der Vorgang entspricht im Wesentlichen dem "Burst-auf-Azure"-Vorgang zum Hinzufügen von Cloudcomputekapazität zu einem lokalen HPC Pack-Cluster. Ein entsprechendes Lernprogramm finden Sie unter [Einrichten eines Hybrid-Rechenclusters mit Microsoft HPC Pack](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Ausführliche Anweisungen und Überlegungen zu Produktionsbereitstellungen finden Sie unter [Burst to Azure with Microsoft HPC Pack](https://technet.microsoft.com/library/gg481749.aspx)(in englischer Sprache).

Überlegungen zur Verwendung einer rechenintensiven Instanzengröße für die Burstknoten finden Sie unter [Informationen zu den rechenintensiven A8-, A9-, A10- und A11-Instanzen](virtual-machines-windows-a8-a9-a10-a11-specs.md).

## <a name="prerequisites"></a>Voraussetzungen
* **HPC Pack-Hauptknoten, der in einer Azure-VM bereitgestellt ist** : Sie können eine eigenständige Hauptknoten-VM verwenden, oder eine, die Teil eines größeren Clusters ist. Informationen zum Erstellen eines eigenständigen Hauptknotens finden Sie unter [Erstellen des Hauptknotens eines HPC Pack-Clusters auf einem virtuellen Azure-Computer mit einem Marketplace-Image](virtual-machines-windows-hpcpack-cluster-headnode.md). Informationen zu automatisierten Optionen für die HPC Pack-Clusterbereitstellung finden Sie unter [Optionen zum Erstellen und Verwalten eines Windows HPC-Clusters (High Performance Computing) in Azure mit Microsoft HPC Pack](virtual-machines-windows-hpcpack-cluster-options.md).
  
  > [!TIP]
  > Bei Verwendung des [HPC Pack IaaS-Bereitstellungsskripts](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) zum Erstellen des Clusters in Azure können Sie Azure-Burstknoten in die automatisierte Bereitstellung integrieren. Sehen Sie sich die Beispiele in diesem Artikel an.
  > 
  > 
* **Azure-Abonnement** – Zum Hinzufügen von Azure-Knoten können Sie dasselbe Abonnement verwenden, das zum Bereitstellen des virtuellen Computers für den Hauptknoten verwendet wird. Sie können dazu auch andere Abonnements verwenden.
* **Kernkontingent** – Unter Umständen muss das Kontingent für die Kerne erhöht werden. Dies gilt insbesondere, wenn Sie mehrere Azure-Knoten mit Multicore-Größen bereitstellen. Um ein Kontingent zu erhöhen, können Sie kostenlos [eine Anfrage an den Onlinekundensupport richten](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) .

## <a name="step-1:-create-a-cloud-service-and-a-storage-account-for-the-azure-nodes"></a>Schritt 1: Erstellen eines Clouddiensts und eines Speicherkontos für die Azure-Knoten
Verwenden Sie das klassische Azure-Portal oder entsprechende Tools, um die folgenden Ressourcen zu konfigurieren, die für die Bereitstellung von Azure-Knoten erforderlich sind:

* Einen neuen Azure-Clouddienst
* Ein neues Azure-Speicherkonto

> [!NOTE]
> Verwenden Sie in Ihrem Abonnement keinen vorhandenen Clouddienst. 
> 
> 

**Überlegungen**

* Konfigurieren Sie einen separaten Clouddienst für jede Azure-Knotenvorlage, die Sie erstellen möchten. Sie können jedoch das gleiche Speicherkonto für mehrere Knotenvorlagen verwenden.
* Der Clouddienst und das Speicherkonto für die Bereitstellung sollten sich in der gleichen Azure-Region befinden.

## <a name="step-2:-configure-an-azure-management-certificate"></a>Schritt 2: Konfigurieren eines Azure-Verwaltungszertifikats
Um Azure-Knoten als Computeressourcen hinzuzufügen, benötigen Sie ein Verwaltungszertifikat für den Hauptknoten und müssen ein entsprechendes Zertifikat für das für die Bereitstellung verwendete Azure-Abonnement hochladen.

In diesem Szenario können Sie das **Zertifikat "Default HPC Azure Management"** auswählen, das HPC Pack automatisch auf dem Hauptknoten installiert und konfiguriert. Dieses Zertifikat ist nützlich für Testzwecke und Machbarkeitsstudien. Um dieses Zertifikat zu verwenden, laden Sie die Datei „C:\Programme\Microsoft HPC Pack 2012\Bin\hpccert.cer“ vom virtuellen Computer für den Hauptknoten in das Abonnement hoch. Klicken Sie zum Hochladen des Zertifikats im [klassischen Azure-Portal](https://manage.windowsazure.com) auf **Einstellungen** > **Verwaltungszertifikate**.

Zusätzliche Optionen zum Konfigurieren des Verwaltungszertifikats finden Sie unter [Options to Configure the Azure Management Certificate for Azure Burst Deployments](http://technet.microsoft.com/library/gg481759.aspx)(in englischer Sprache).

## <a name="step-3:-deploy-azure-nodes-to-the-cluster"></a>Schritt 3: Bereitstellen von Azure-Knoten auf dem Cluster
Die Schritte zum Hinzufügen und Starten von Azure-Knoten in diesem Szenario entsprechen im Allgemeinen der Vorgehensweise bei lokalen Hauptknoten. Weitere Informationen finden Sie in den folgenden Abschnitten unter [Steps to Deploy Azure Nodes with Microsoft HPC Pack](https://technet.microsoft.com/library/gg481758.aspx)(in englischer Sprache):

* Erstellen einer Azure-Knotenvorlage
* Hinzufügen von Azure-Knoten zum Windows HPC-Cluster
* Starten (Bereitstellen) der Azure-Knoten

Nachdem Sie die Knoten hinzugefügt und gestartet haben, können Sie sie zum Ausführen von Clusteraufträgen verwenden.

Wenn bei der Bereitstellung von Azure-Knoten Probleme auftreten, finden Sie weitere Informationen unter [Troubleshoot Deployments of Azure Nodes with Microsoft HPC Pack](http://technet.microsoft.com/library/jj159097.aspx)(Anweisungen zur Problembehandlung bei der Bereitstellung von Azure-Knoten mit Microsoft HPC Pack).

## <a name="next-steps"></a>Nächste Schritte
* Wenn Sie die Azure-Computeressourcen entsprechend der aktuellen Clusterworkload automatisch vergrößern oder verkleinern möchten, finden Sie entsprechende Informationen unter [Automatisches Vergrößern oder Verkleinern der HPC Pack-Clusterressourcen in Azure gemäß der Clusterworkload](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md).

<!--Image references-->
[Burst]: ./media/virtual-machines-windows-classic-hpcpack-cluster-node-burst/burst.png



<!--HONumber=Oct16_HO2-->


