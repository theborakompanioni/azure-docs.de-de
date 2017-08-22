---
title: Verwalten von Hadoop-Clustern mit der Azure-CLI - Azure HDInsight| Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit der Azure-Befehlszeilenschnittstelle Hadoop-Cluster in Azure HDInsight verwalten. Die Azure-Befehlszeilenschnittstelle funktioniert unter Windows, Mac und Linux.
services: hdinsight
editor: cgronlun
manager: jhubbard
author: mumian
tags: azure-portal
documentationcenter: 
ms.assetid: 4f26c79f-8540-44bd-a470-84722a9e4eca
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ms.translationtype: Human Translation
ms.sourcegitcommit: 245ce9261332a3d36a36968f7c9dbc4611a019b2
ms.openlocfilehash: 5102d44ebd447b638ab6e26fa1826bad224ed6c3
ms.contentlocale: de-de
ms.lasthandoff: 06/09/2017

---
# <a name="manage-hadoop-clusters-in-hdinsight-using-the-azure-cli"></a>Verwalten von Hadoop-Clustern in HDInsight mit der Azure-Befehlszeilenschnittstelle
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Hier erfahren Sie, wie Sie mit der [Azure-Befehlszeilenschnittstelle](../cli-install-nodejs.md) Hadoop-Cluster in Azure HDInsight verwalten. Die Azure-CLI ist in Node.js implementiert. Sie kann auf allen Plattformen verwendet werden, die Node.js unterstützen, inklusive Windows, Mac und Linux.

In diesem Artikel wird lediglich die Verwendung der Azure-CLI mit HDInsight behandelt. Eine allgemeine Anleitung zur Verwendung der Azure-Befehlszeilenschnittstelle finden Sie unter [Installieren und Konfigurieren der Azure-Befehlszeilenschnittstelle][azure-command-line-tools].

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Artikel beginnen können, benötigen Sie Folgendes:

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Azure-CLI** – Informationen zur Installation und Konfiguration finden Sie unter [Installieren und Konfigurieren der Azure-CLI](../cli-install-nodejs.md) .
* Stellen Sie über den folgenden Befehl **eine Verbindung mit Azure** her:
  
        azure login
  
    Weitere Informationen zur Authentifizierung mit einem Geschäfts- oder Schulkonto finden Sie unter [Herstellen einer Verbindung mit einem Azure-Abonnement über die Azure-Befehlszeilenschnittstelle](../xplat-cli-connect.md).
* Wechseln Sie mit dem folgenden Befehl in den **Azure-Ressourcen-Manager-Modus**:
  
        azure config mode arm

Um Hilfe zu erhalten, verwenden Sie die Option **-h** .  Beispiel:

    azure hdinsight cluster create -h

## <a name="create-clusters-with-the-cli"></a>Erstellen von Clustern mit der Befehlszeilenschnittstelle
Weitere Informationen finden Sie unter [Erstellen von Clustern in HDInsight mit der Azure-Befehlszeilenschnittstelle](hdinsight-hadoop-create-linux-clusters-azure-cli.md).

## <a name="list-and-show-cluster-details"></a>Auflisten und Anzeigen von Clusterdetails
Mit den folgenden Befehlen können Sie Clusterdetails auflisten und anzeigen:

    azure hdinsight cluster list
    azure hdinsight cluster show <Cluster Name>

![Befehlszeilenansicht der Clusterliste][image-cli-clusterlisting]

## <a name="delete-clusters"></a>Löschen von Clustern
Mit dem folgenden Befehl können Sie ein Cluster löschen:

    azure hdinsight cluster delete <Cluster Name>

Sie können einen Cluster auch löschen, indem Sie die Ressourcengruppe löschen, die den Cluster enthält. Beachten Sie, dass damit alle Ressourcen in der Gruppe, einschließlich des Standardspeicherkontos, gelöscht werden.

    azure group delete <Resource Group Name>

## <a name="scale-clusters"></a>Skalieren von Clustern
So ändern Sie die Hadoop-Clustergröße:

    azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>


## <a name="enabledisable-http-access-for-a-cluster"></a>Aktivieren/Deaktivieren des HTTP-Zugriffs für einen Cluster
    azure hdinsight cluster enable-http-access [options] <Cluster Name> <userName> <password>
    azure hdinsight cluster disable-http-access [options] <Cluster Name>

## <a name="enabledisable-rdp-access-for-a-cluster"></a>Aktivieren/Deaktivieren des RDP-Zugriffs für einen Cluster
      azure hdinsight cluster enable-rdp-access [options] <Cluster Name> <rdpUserName> <rdpPassword> <rdpExpiryDate>
      azure hdinsight cluster disable-rdp-access [options] <Cluster Name>


## <a name="next-steps"></a>Nächste Schritte
Sie sind nun in der Lage, verschiedene Verwaltungsaufgaben für HDInsight-Cluster auszuführen. Weitere Informationen finden Sie in den folgenden Artikeln:

* [Verwalten von HDInsight mit dem Azure-Portal][hdinsight-admin-portal]
* [Verwalten von HDInsight mit Azure PowerShell][hdinsight-admin-powershell]
* [Erste Schritte mit Azure HDInsight][hdinsight-get-started]
* [Verwenden der Azure-Befehlszeilenschnittstelle][azure-command-line-tools]

[azure-command-line-tools]: ../cli-install-nodejs.md
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[image-cli-account-download-import]: ./media/hdinsight-administer-use-command-line/HDI.CLIAccountDownloadImport.png
[image-cli-clustercreation]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreation.png
[image-cli-clustercreation-config]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreationConfig.png
[image-cli-clusterlisting]: ./media/hdinsight-administer-use-command-line/command-line-list-of-clusters.png "Auflisten und Anzeigen von Clustern"

