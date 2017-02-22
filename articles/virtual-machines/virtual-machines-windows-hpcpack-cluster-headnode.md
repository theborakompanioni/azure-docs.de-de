---
title: Erstellen eines HPC Pack-Hauptknotens in einem virtuellen Azure-Computer | Microsoft Docs
description: Erfahren Sie, wie Sie das Azure-Portal und das Resource Manager-Bereitstellungsmodell verwenden, um einen Microsoft HPC Pack 2012 R2-Hauptknoten auf einem virtuellen Azure-Computer zu erstellen.
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager,hpc-pack
ms.assetid: e6a13eaf-9124-47b4-8d75-2bc4672b8f21
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 12/29/2016
ms.author: danlep
translationtype: Human Translation
ms.sourcegitcommit: 3d8300bbb54bd88e6ff3844208ec5d5fa25c5e8d
ms.openlocfilehash: d935f45f87558dd7f9838ad3b370de0d9a7870a1


---
# <a name="create-the-head-node-of-an-hpc-pack-cluster-in-an-azure-vm-with-a-marketplace-image"></a>Erstellen des Hauptknotens eines HPC Pack-Clusters auf einem virtuellen Azure-Computer mit einem Marketplace-Image
Verwenden Sie ein [Microsoft HPC Pack 2012 R2-Image für einen virtuellen Computer](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) aus Azure Marketplace und das Azure-Portal, um den Hauptknoten eines HPC-Clusters zu erstellen. Das HPC Pack-VM-Image basiert auf Windows Server 2012 R2 Datacenter mit vorinstalliertem HPC Pack 2012 R2 Update 3. Verwenden Sie diesen Hauptknoten für eine Proof of Concept-Bereitstellung von HPC Pack in Azure. Sie können dem Cluster dann Computeknoten zum Ausführen von HPC-Workloads hinzufügen.

> [!TIP]
> Um einen vollständigen HPC Pack 2012 R2-Cluster in Azure bereitzustellen, der den Hauptknoten und Serverknoten enthält, sollten Sie eine automatisierte Methode verwenden. Zu den Optionen gehören das [HPC Pack-IaaS-Bereitstellungsskript](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) und Resource Manager-Vorlagen wie der [HPC Pack-Cluster für Windows-Workloads](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/). Resource Manager-Vorlagen stehen auch für [Microsoft HPC Pack 2016-Cluster](https://github.com/MsHpcPack/HPCPack2016/tree/master/newcluster-templates) zur Verfügung. 
> 
> 

## <a name="planning-considerations"></a>Überlegungen zur Planung
Wie in der folgenden Abbildung gezeigt, stellen Sie den HPC Pack-Hauptknoten in einer Active Directory-Domäne in einem virtuellen Azure-Netzwerk bereit.

![HPC Pack-Hauptknoten][headnode]

* **Active Directory-Domäne:** Der HPC Pack 2012 R2-Hauptknoten muss einer Active Directory-Domäne in Azure beigetreten sein, bevor Sie die HPC-Dienste auf dem virtuellen Computer starten. Wie in diesem Artikel gezeigt, können Sie die VM, die Sie für den Hauptknoten als Domänencontroller erstellen, für eine Proof of Concept-Bereitstellung höher stufen, bevor Sie die HPC-Dienste starten. Eine weitere Möglichkeit ist das Bereitstellen eines separaten Domänencontrollers und einer Gesamtstruktur in Azure, der der virtuelle Hauptknotencomputer beitritt.

* **Bereitstellungsmodell:** Für die meisten neuen Bereitstellungen empfiehlt Microsoft die Verwendung des Resource Manager-Bereitstellungsmodells. In diesem Artikel wird davon ausgegangen, dass Sie dieses Bereitstellungsmodell verwenden.

* **Virtuelles Azure-Netzwerk:** Wenn Sie den Hauptknoten mithilfe des Resource Manager-Bereitstellungsmodells bereitstellen, geben Sie ein virtuelles Azure-Netzwerk an bzw. erstellen es. Sie verwenden das virtuelle Netzwerk, um den Beitritt des Hauptknotens zu einer vorhandenen Active Directory-Domäne auszuführen. Damit fügen Sie dem Cluster auch später Serverknoten-VMs hinzu.

## <a name="steps-to-create-the-head-node"></a>Schritte zum Erstellen des Hauptknotens
Es folgen allgemeine Schritte zur Verwendung des Azure-Portals zum Erstellen einer Azure-VM für den HPC Pack-Hauptknoten mit dem Ressourcen-Manager-Bereitstellungsmodell. 

1. Wenn Sie eine neue Active Directory-Gesamtstruktur in Azure mit separaten Domänencontroller-VMs erstellen möchten, ist die Verwendung einer [Resource Manager-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/active-directory-new-domain-ha-2-dc)eine Möglichkeit. Für eine einfache Proof of Concept-Bereitstellung können Sie diesen Schritt auslassen und die Hauptknoten-VM selbst als Domänencontroller konfigurieren. Diese Option wird weiter unten in diesem Artikel beschrieben.
2. Klicken Sie im Azure Marketplace auf der Seite [HPC Pack 2012 R2 unter Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) auf **Virtuellen Computer erstellen**. 
3. Wählen Sie im Portal auf der Seite **HPC Pack 2012 R2 unter Windows Server 2012 R2** das Bereitstellungsmodell **Resource Manage**, und klicken Sie dann auf **Erstellen**.
   
    ![HPC Pack-Image][marketplace]
4. Verwenden Sie das Portal, um die Einstellungen zu konfigurieren und die VM zu erstellen. Wenn Sie mit Azure noch nicht vertraut sind, folgen Sie den Schritten im Tutorial [Erstellen Ihres ersten virtuellen Windows-Computers im Azure-Portal](virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Für eine Proof of Concept-Bereitstellung können Sie in der Regel die Standardeinstellungen oder empfohlenen Einstellungen übernehmen.
   
   > [!NOTE]
   > Wenn Sie den Beitritt des Hauptknotens in Azure zu einer vorhandenen Active Directory-Domäne ausführen möchten, stellen Sie sicher, dass Sie beim Erstellen der VM das virtuelle Netzwerk dieser Domäne angeben.
   > 
   > 
5. Wenn der virtuelle Computer, den Sie erstellt haben, ausgeführt wird, [stellen Sie über Remotedesktop die Verbindung mit dem virtuellen Computer her](virtual-machines-windows-connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 
6. Verbinden Sie den virtuellen Computer mit einer Active Directory-Domänengesamtstruktur durch Auswählen einer der folgenden Optionen:
   
   * Wenn Sie den virtuellen Computer in einem virtuellen Azure-Netzwerk mit einer vorhandenen Domänengesamtstruktur erstellt haben, verwenden Sie den standardmäßigen Server-Manager oder Windows PowerShell-Tools, um seinen Beitritt zur Gesamtstruktur auszuführen. Führen Sie anschließend einen Neustart durch.
   * Wenn Sie die VM in einem neuen virtuellen Netzwerk (ohne vorhandene Domänengesamtstruktur) erstellt haben, stufen Sie die VM zum Domänencontroller hoch. Installieren und konfigurieren Sie die Active Directory-Domänendienste-Rolle in standardmäßigen Schritten auf dem Hauptknoten. Ausführliche Schritte finden Sie unter [Installieren einer neuen Windows Server 2012 Active Directory-Gesamtstruktur](https://technet.microsoft.com/library/jj574166.aspx).
7. Wenn der virtuelle Computer ausgeführt wird und Mitglied einer Active Directory-Gesamtstruktur ist, starten Sie die HPC Pack-Dienste wie folgt:
   
    a. Stellen Sie eine Verbindung mit dem virtuellen Hauptknotencomputer her, indem Sie ein Domänenkonto verwenden, das Mitglied der lokalen Administratorgruppe ist. Nutzen Sie beispielsweise das Administratorkonto, das Sie beim Erstellen des virtuellen Hauptknotencomputers eingerichtet haben.
   
    b. Starten Sie zur Durchführung einer Standardkonfiguration des Hauptknotens Windows PowerShell als Administrator, und geben Sie Folgendes ein:
   
    ```PowerShell
    & $env:CCP_HOME\bin\HPCHNPrepare.ps1 –DBServerInstance ".\ComputeCluster"
    ```
   
    Es kann mehrere Minuten dauern, bis die HPC Pack-Dienste starten.
   
    Geben Sie `get-help HPCHNPrepare.ps1`ein, um weitere Konfigurationsoptionen für den Hauptknoten anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte
* Sie können jetzt mit dem Hauptknoten Ihres HPC Pack-Clusters arbeiten. Starten Sie z.B. den HPC-Cluster-Manager, und führen Sie die [Aufgabenliste für die Bereitstellung](https://technet.microsoft.com/library/jj884141.aspx) aus.
* Wenn Sie die Clustercomputekapazität bedarfsgesteuert heraufsetzen möchten, fügen Sie [Azure-Burstknoten](virtual-machines-windows-classic-hpcpack-cluster-node-burst.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) in einem Clouddienst hinzu. 
* Führen Sie eine Test-Workload auf dem Cluster aus. Ein Beispiel hierzu finden Sie im [Leitfaden für die ersten Schritte](https://technet.microsoft.com/library/jj884144)von HPC Pack.

<!--Image references-->
[headnode]: ./media/virtual-machines-windows-hpcpack-cluster-headnode/headnode.png
[marketplace]: ./media/virtual-machines-windows-hpcpack-cluster-headnode/marketplace.png



<!--HONumber=Jan17_HO1-->


