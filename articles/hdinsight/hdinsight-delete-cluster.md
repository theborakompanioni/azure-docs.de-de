---
title: "Löschen eines HDInsight-Clusters | Microsoft-Dokumentation"
description: "Informationen zu den verschiedenen Möglichkeiten, einen HDInsight-Cluster zu löschen."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 55f7838b-9786-47ff-96db-1b64437bd0bb
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/28/2016
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 67442bf4b04f6f3799d30f7ce26547c8145d9168


---
# <a name="how-to-delete-an-hdinsight-cluster"></a>So löschen Sie einen HDInsight-Cluster
Die HDInsight-Clusterabrechnung beginnt mit der Erstellung eines Clusters und endet mit seiner Löschung. Die Gebühren werden anteilig pro Minute berechnet, daher sollten Sie Cluster immer löschen, wenn sie nicht mehr verwendet werden. In diesem Artikel erfahren Sie, wie Sie einen Cluster mithilfe des Azure-Portals, Azure PowerShell und der Azure-Befehlszeilenschnittstelle löschen.

> [!IMPORTANT]
> Das Löschen eines HDInsight-Clusters führt nicht zum Löschen des zugeordneten Azure-Speicherkontos bzw. der zugeordneten Azure-Speicherkonten. Dadurch können Sie alle vom Cluster gespeicherten Daten erhalten und wiederverwenden.
> 
> 

## <a name="azure-portal"></a>Azure-Portal
1. Melden Sie sich im [Azure-Portal](https://portal.azure.com) an, und wählen Sie Ihren HDInsight-Cluster aus. Wenn Ihr HDInsight-Cluster nicht mit dem Dashboard verknüpft ist, können Sie ihn mithilfe des Suchfelds (Lupensymbol) auf der rechten Seite der Navigationsleiste durch Eingabe des Namens suchen.
   
    ![Portalsuche](./media/hdinsight-delete-cluster/navbar.png)
2. Sobald das Blatt für den Cluster geöffnet ist, wählen Sie das Symbol **Löschen**. Wenn Sie dazu aufgefordert werden, wählen Sie **Ja**, um den Cluster zu löschen.
   
    ![Löschen-Symbol](./media/hdinsight-delete-cluster/deletecluster.png)

## <a name="azure-powershell"></a>Azure PowerShell
Verwenden Sie an einer PowerShell-Eingabeaufforderung den folgenden Befehl zum Löschen des Clusters:

    Remove-AzureRmHDInsightCluster -ClusterName CLUSTERNAME

Ersetzen Sie **CLUSTERNAME** durch den Namen Ihres HDInsight-Clusters.

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle
Verwenden Sie an einer Eingabeaufforderung den folgenden Befehl zum Löschen des Clusters:

    azure hdinsight cluster delete CLUSTERNAME

Ersetzen Sie **CLUSTERNAME** durch den Namen Ihres HDInsight-Clusters.




<!--HONumber=Nov16_HO3-->


