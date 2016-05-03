<properties
 pageTitle="Hinzufügen von Burstknoten zu einem HPC Pack-Cluster | Microsoft Azure"
 description="Erfahren Sie, wie Sie die HPC Pack-Clusterkapazität bei Bedarf erweitern können, indem Sie Workerrolleninstanzen in einem Clouddienst hinzufügen."
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="04/13/2016"
 ms.author="danlep"/>

# Hinzufügen von bedarfsgesteuerten Burstknoten (Workerrolleninstanzen) als Computeressourcen zu einem HPC Pack-Cluster in Azure



In diesem Artikel wird erläutert, wie Sie Azure-Burstknoten (Workerrolleninstanzen, die in einem Clouddienst ausgeführt werden) bedarfsgesteuert als Computeressourcen zu einem vorhandenen HPC Pack-Hauptknoten in Azure hinzufügen So können Sie die Computekapazität des HPC-Clusters in Azure bedarfsgesteuert zentral hochskalieren, ohne mehrere vorkonfigurierte VMs für Computeknoten verwalten zu müssen.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Ressourcen-Manager-Modell.

![Burstknoten][burst]

>[AZURE.TIP] Bei Verwendung des [HPC Pack IaaS-Bereitstellungsskripts](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) zum Erstellen des Clusters in Azure können Sie Azure-Burstknoten in die automatisierte Bereitstellung integrieren. Sehen Sie sich die Beispiele in diesem Artikel an.

Anhand der Schritte in diesem Artikel können Sie Azure-Knoten auf schnelle Weise zu einem cloudbasierten virtuellen Computer für den HPC Pack-Hauptknoten für Tests oder Machbarkeitsstudien hinzufügen. Der Vorgang entspricht im Wesentlichen dem "Burst-auf-Azure"-Vorgang zum Hinzufügen von Cloudcomputekapazität zu einem lokalen HPC Pack-Cluster. Ein entsprechendes Lernprogramm finden Sie unter [Einrichten eines Hybrid-Rechenclusters mit Microsoft HPC Pack](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Ausführliche Anweisungen und Überlegungen zu Produktionsbereitstellungen finden Sie unter [Burst to Azure with Microsoft HPC Pack](https://technet.microsoft.com/library/gg481749.aspx) (in englischer Sprache).

Wenn Sie die rechenintensive A8- oder A9-Instanzengröße verwenden möchten, finden Sie entsprechende Informationen unter [Informationen zu den rechenintensiven A8-, A9-, A10- und A11-Instanzen](virtual-machines-windows-a8-a9-a10-a11-specs.md).

## Voraussetzungen

* **HPC Pack-Hauptknoten bereitgestellt auf einem virtuellen Azure-Computer** –Anweisungen zum Erstellen eines Clusterhauptknotens im klassischen Bereitstellungsmodell finden Sie unter [Erstellen des Hauptknotens eines HPC Pack-Clusters auf einem virtuellen Azure-Computer mit einem Marketplace-Image](virtual-machines-windows-hpcpack-cluster-headnode.md).

* **Azure-Abonnement**: Zum Hinzufügen von Azure-Knoten können Sie dasselbe Abonnement verwenden, das zum Bereitstellen des virtuellen Computers für den Hauptknoten verwendet wird. Sie können dazu auch andere Abonnements verwenden.

* **Kernkontingent**: Unter Umständen muss das Kontingent für die Kerne erhöht werden. Dies gilt insbesondere, wenn Sie mehrere Azure-Knoten mit Multicore-Größen bereitstellen. Um ein Kontingent zu erhöhen, können Sie kostenlos eine [Anfrage an den Onlinekundensupport richten](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/).

## Schritt 1: Erstellen eines Clouddiensts und eines Speicherkontos zum Hinzufügen von Azure-Knoten

Verwenden Sie das klassische Azure-Portal oder entsprechende Tools, um die folgenden Komponenten zu konfigurieren, die für die Bereitstellung von Azure-Knoten erforderlich sind:

* Einen neuen Azure-Clouddienst
* Ein neues Azure-Speicherkonto

>[AZURE.NOTE] Verwenden Sie in Ihrem Abonnement keinen vorhandenen Clouddienst.

**Überlegungen**

* Konfigurieren Sie einen separaten Clouddienst für jede Azure-Knotenvorlage, die Sie erstellen möchten. Sie können jedoch das gleiche Speicherkonto für mehrere Knotenvorlagen verwenden.

* Im Allgemeinen sollten sich der Clouddienst und das Speicherkonto für die Bereitstellung in der gleichen Azure-Region befinden.




## Schritt 2: Konfigurieren eines Azure-Verwaltungszertifikats

Um Azure-Knoten als Computeressourcen hinzuzufügen, müssen Sie ein Verwaltungszertifikat für den Hauptknoten zuweisen und ein entsprechendes Zertifikat für das für die Bereitstellung verwendete Azure-Abonnement hochladen.

In diesem Szenario können Sie das **Zertifikat "Default HPC Azure Management"** auswählen, das HPC Pack automatisch auf dem Hauptknoten installiert und konfiguriert. Dieses Zertifikat ist nützlich für Testzwecke und Machbarkeitsstudien. Um dieses Zertifikat zu verwenden, laden Sie einfach die Datei "C:\\Programme\\Microsoft HPC Pack 2012\\Bin\\hpccert.cer" vom virtuellen Computer für den Hauptknoten in das Abonnement hoch.

Zusätzliche Optionen zum Konfigurieren des Verwaltungszertifikats finden Sie unter [Options to Configure the Azure Management Certificate for Azure Burst Deployments](http://technet.microsoft.com/library/gg481759.aspx) (in englischer Sprache).

## Schritt 3: Bereitstellen von Azure-Knoten auf dem Cluster



Die Schritte zum Hinzufügen und Starten von Azure-Knoten in diesem Szenario entsprechen i. Allg. der Vorgehensweise bei lokalen Hauptknoten. Weitere Informationen finden Sie in den folgenden Abschnitten unter [Steps to Deploy Azure Nodes with Microsoft HPC Pack](https://technet.microsoft.com/library/gg481758.aspx) (in englischer Sprache):

* Erstellen einer Azure-Knotenvorlage

* Hinzufügen von Azure-Knoten zum Windows HPC-Cluster

* Starten (Bereitstellen) der Azure-Knoten

Nachdem Sie die Knoten hinzugefügt und gestartet haben, können Sie sie zum Ausführen von Clusteraufträgen verwenden.

Wenn bei der Bereitstellung von Azure-Knoten Probleme auftreten, finden Sie weitere Informationen unter [Troubleshoot Deployments of Azure Nodes with Microsoft HPC Pack](http://technet.microsoft.com/library/jj159097.aspx) (Anweisungen zur Problembehandlung bei der Bereitstellung von Azure-Knoten mit Microsoft HPC Pack).

## Nächste Schritte

* Wenn Sie die Azure-Computeressourcen entsprechend der aktuellen Workload der Aufträge und Aufgaben im Cluster automatisch vergrößern oder verkleinern möchten, finden Sie entsprechende Informationen unter [Automatisches Hoch- und Herunterskalieren von Azure-Computeressourcen in einem HPC Pack-Cluster](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md).

<!--Image references-->
[burst]: ./media/virtual-machines-windows-classic-hpcpack-cluster-node-burst/burst.png

<!---HONumber=AcomDC_0420_2016-->