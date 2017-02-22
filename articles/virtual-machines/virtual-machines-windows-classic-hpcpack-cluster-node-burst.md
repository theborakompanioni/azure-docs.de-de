---
title: "Hinzufügen von Burstknoten zu einem HPC Pack-Cluster | Microsoft Docs"
description: "Erfahren Sie, wie Sie einen HPC Pack-Cluster bei Bedarf erweitern können, indem Sie Workerrolleninstanzen in einem Clouddienst hinzufügen."
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,hpc-pack
ms.assetid: 24b79a8a-24ad-4002-ae76-75abc9b28c83
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 10/14/2016
ms.author: danlep
translationtype: Human Translation
ms.sourcegitcommit: f6537e4ebac76b9f3328223ee30647885ee15d3e
ms.openlocfilehash: e206d161665e46a7c1c4a111a3642ece844b7b64


---
# <a name="add-on-demand-burst-nodes-to-an-hpc-pack-cluster-in-azure"></a>Hinzufügen von bedarfsgesteuerten „Burstknoten“ zu einem HPC Pack-Cluster in Azure
Wenn Sie einen [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029)-Cluster in Azure einrichten, benötigen Sie wahrscheinlich eine Möglichkeit, die Clusterkapazität schnell hoch- oder herunterzuskalieren, ohne einen Satz vorkonfigurierter Computeknoten-VMs vorhalten zu müssen. In diesem Artikel wird erläutert, wie Sie bedarfsgesteuerte Burstknoten (Workerrolleninstanzen, die in einem Clouddienst ausgeführt werden) als Computeressourcen zu einem Hauptknoten in Azure hinzufügen. 

> [!IMPORTANT] 
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager- und klassische Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells.

![Burstknoten][burst]

Anhand der Schritte in diesem Artikel können Sie Azure-Knoten auf schnelle Weise zu einem cloudbasierten virtuellen Computer für den HPC Pack-Hauptknoten für Tests oder Machbarkeitsstudien hinzufügen. Die Schritte entsprechen im Wesentlichen den „Burst-auf-Azure“-Schritten zum Hinzufügen von Cloudcomputekapazität zu einem lokalen HPC Pack-Cluster. Ein entsprechendes Lernprogramm finden Sie unter [Einrichten eines Hybrid-Rechenclusters mit Microsoft HPC Pack](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Ausführliche Anweisungen und Überlegungen zu Produktionsbereitstellungen finden Sie unter [Burst to Azure with Microsoft HPC Pack](https://technet.microsoft.com/library/gg481749.aspx)(in englischer Sprache).

## <a name="prerequisites"></a>Voraussetzungen
* **HPC Pack-Hauptknoten, der in einer Azure-VM bereitgestellt ist** : Sie können eine eigenständige Hauptknoten-VM verwenden, oder eine, die Teil eines größeren Clusters ist. Informationen zum Erstellen eines eigenständigen Hauptknotens finden Sie unter [Erstellen des Hauptknotens eines HPC Pack-Clusters auf einem virtuellen Azure-Computer mit einem Marketplace-Image](virtual-machines-windows-hpcpack-cluster-headnode.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Informationen zu automatisierten Optionen für die HPC Pack-Clusterbereitstellung finden Sie unter [Optionen zum Erstellen und Verwalten eines Windows HPC-Clusters (High Performance Computing) in Azure mit Microsoft HPC Pack](virtual-machines-windows-hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
  
  > [!TIP]
  > Bei Verwendung des [HPC Pack IaaS-Bereitstellungsskripts](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) zum Erstellen des Clusters in Azure können Sie Azure-Burstknoten in die automatisierte Bereitstellung integrieren. Sehen Sie sich die Beispiele in diesem Artikel an.
  > 
  > 
* **Azure-Abonnement** – Zum Hinzufügen von Azure-Knoten können Sie dasselbe Abonnement verwenden, das zum Bereitstellen des virtuellen Computers für den Hauptknoten verwendet wird. Sie können dazu auch andere Abonnements verwenden.
* **Kernkontingent** – Unter Umständen muss das Kontingent für die Kerne erhöht werden. Dies gilt insbesondere, wenn Sie mehrere Azure-Knoten mit Multicore-Größen bereitstellen. Um ein Kontingent zu erhöhen, können Sie kostenlos [eine Anfrage an den Onlinekundensupport richten](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) .

## <a name="step-1-create-a-cloud-service-and-a-storage-account-for-the-azure-nodes"></a>Schritt 1: Erstellen eines Clouddiensts und eines Speicherkontos für die Azure-Knoten
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

## <a name="step-2-configure-an-azure-management-certificate"></a>Schritt 2: Konfigurieren eines Azure-Verwaltungszertifikats
Um Azure-Knoten als Computeressourcen hinzuzufügen, benötigen Sie ein Verwaltungszertifikat für den Hauptknoten und müssen ein entsprechendes Zertifikat für das für die Bereitstellung verwendete Azure-Abonnement hochladen.

In diesem Szenario können Sie das **Zertifikat "Default HPC Azure Management"** auswählen, das HPC Pack automatisch auf dem Hauptknoten installiert und konfiguriert. Dieses Zertifikat ist nützlich für Testzwecke und Machbarkeitsstudien. Um dieses Zertifikat zu verwenden, laden Sie die Datei „C:\Programme\Microsoft HPC Pack 2012\Bin\hpccert.cer“ vom virtuellen Computer für den Hauptknoten in das Abonnement hoch. Klicken Sie zum Hochladen des Zertifikats im [klassischen Azure-Portal](https://manage.windowsazure.com) auf **Einstellungen** > **Verwaltungszertifikate**.

Zusätzliche Optionen zum Konfigurieren des Verwaltungszertifikats finden Sie unter [Options to Configure the Azure Management Certificate for Azure Burst Deployments](http://technet.microsoft.com/library/gg481759.aspx)(in englischer Sprache).

## <a name="step-3-deploy-azure-nodes-to-the-cluster"></a>Schritt 3: Bereitstellen von Azure-Knoten auf dem Cluster
Die Schritte zum Hinzufügen und Starten von Azure-Knoten in diesem Szenario entsprechen im Allgemeinen der Vorgehensweise bei lokalen Hauptknoten. Weitere Informationen finden Sie in den folgenden Abschnitten unter [Steps to Deploy Azure Nodes with Microsoft HPC Pack](https://technet.microsoft.com/library/gg481758.aspx)(in englischer Sprache):

* Erstellen einer Azure-Knotenvorlage
* Hinzufügen von Azure-Knoten zum Windows HPC-Cluster
* Starten (Bereitstellen) der Azure-Knoten

Nachdem Sie die Knoten hinzugefügt und gestartet haben, können Sie sie zum Ausführen von Clusteraufträgen verwenden.

Wenn bei der Bereitstellung von Azure-Knoten Probleme auftreten, finden Sie weitere Informationen unter [Troubleshoot Deployments of Azure Nodes with Microsoft HPC Pack](http://technet.microsoft.com/library/jj159097.aspx)(Anweisungen zur Problembehandlung bei der Bereitstellung von Azure-Knoten mit Microsoft HPC Pack).

## <a name="next-steps"></a>Nächste Schritte
* Überlegungen zur Verwendung einer rechenintensiven Instanzengröße für die Burstknoten finden Sie unter [Informationen zu virtuellen Computern der H-Reihe und der rechenintensiven A-Reihe](virtual-machines-windows-a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Wenn Sie die Azure-Computeressourcen entsprechend der aktuellen Clusterworkload automatisch vergrößern oder verkleinern möchten, finden Sie entsprechende Informationen unter [Automatisches Vergrößern oder Verkleinern der HPC Pack-Clusterressourcen in Azure gemäß der Clusterworkload](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

<!--Image references-->
[burst]: ./media/virtual-machines-windows-classic-hpcpack-cluster-node-burst/burst.png



<!--HONumber=Feb17_HO3-->


