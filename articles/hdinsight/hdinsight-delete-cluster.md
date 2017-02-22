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
ms.date: 02/08/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 5ec4b964066687b506686709c3dc5ed5b402fbaf
ms.openlocfilehash: 8b1fbc541b3123961d092c63441b804109865f60


---
# <a name="how-to-delete-an-hdinsight-cluster"></a>So löschen Sie einen HDInsight-Cluster

Die Abrechnung für einen HDInsight-Cluster beginnt, sobald der Cluster erstellt wurde, und endet mit dem Löschen des Clusters. Die Gebühren werden anteilig nach Minuten erhoben. Daher sollten Sie Ihren Cluster immer löschen, wenn Sie ihn nicht mehr verwenden. In diesem Artikel erfahren Sie, wie Sie einen Cluster mithilfe des Azure-Portals, Azure PowerShell und der Azure-Befehlszeilenschnittstelle löschen.

> [!IMPORTANT]
> Das Löschen eines HDInsight-Clusters führt nicht zum Löschen der zugeordneten Azure-Speicherkonten. Da die Speicherkonten nicht gelöscht werden, bleiben Ihre Daten erhalten und können wiederverwendet werden.

## <a name="azure-portal"></a>Azure-Portal

1. Melden Sie sich im [Azure-Portal](https://portal.azure.com) an, und wählen Sie Ihren HDInsight-Cluster aus. Wenn Ihr HDInsight-Cluster nicht mit dem Dashboard verknüpft ist, können Sie ihn anhand seines Namens über das Suchfeld suchen.
   
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




<!--HONumber=Feb17_HO2-->


