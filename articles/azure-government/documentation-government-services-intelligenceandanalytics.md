---
title: Azure Government Intelligence und Analyse | Microsoft-Dokumentation
description: "Dies bietet einen Vergleich der Features und Richtlinien zum Entwickeln von Anwendungen für Azure Government"
services: Azure-Government
cloud: gov
documentationcenter: 
author: MeganYount
manager: zakramer
editor: 
ms.assetid: 4b7720c1-699e-432b-9246-6e49fb77f497
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: azure-government
ms.date: 12/06/2016
ms.author: MeganYount
translationtype: Human Translation
ms.sourcegitcommit: 0302a44bb42200c07a313bf2862479c1722af8c4
ms.openlocfilehash: 2a8769f7a611fb22e39db166d3416728f57cbbf4


---
# <a name="azure-government-intelligence--analytics"></a>Azure Government Intelligence und Analyse
Dieser Artikel beschreibt die Dienste, Varianten und Überlegungen zum Thema Intelligence und Analyse für die Azure Government-Umgebung.

## <a name="azure-hdinsight"></a>Azure HDInsight
HDInsight ist in Azure Government allgemein verfügbar.

### <a name="variations"></a>Variationen
Die folgenden HDInsight-Features sind aktuell nicht in Azure Government verfügbar.

* HDInsight ist unter Windows nicht verfügbar.
* Azure Data Lake Store ist aktuell nicht in Azure Government verfügbar. Azure Blob Storage ist zurzeit die einzige verfügbare Speicheroption.
* Die Azure Active Directory Domain Services-Integration ist noch nicht verfügbar. Um sichere Hadoop-Cluster ohne Active Directory Domain Services zu erstellen, wählen Sie eines der folgenden Setupszenarien aus:

1. HDInsight, integriert in Active Directory, das in Azure IaaS ausgeführt wird

  Dies ist die bei Weitem einfachste Architektur für die Integration von HDInsight in Active Directory. Das Architekturdiagramm finden Sie nachfolgend. In dieser Architektur wird Ihr Active Directory-Domänencontroller auf einem oder mehreren virtuellen Computern in Azure ausgeführt. Diese virtuellen Computer befinden sich üblicherweise innerhalb eines virtuellen Netzwerks. Sie können ein neues virtuelles Netzwerk einrichten, in dem Sie Ihren HDInsight-Cluster platzieren können. Damit HDInsight auf das Active Directory zugreifen kann, müssen Sie diese virtuellen Netzwerke per [VNET-zu-VNET-Peering] verbinden (https://docs.microsoft.com/de-de/azure/virtual-network/virtual-networks-create-vnetpeering-arm-portal). 
  
  Voraussetzungen, die im Active Directory eingerichtet werden müssen
     * Es muss eine Organisationseinheit erstellt werden, in der Sie die HDInsight-Cluster-VMs und die vom Cluster verwendeten Dienstprinzipale platzieren möchten. 
     * Zur Kommunikation mit dem Active Directory muss LDAPS eingerichtet werden. Das Zertifikat zum Einrichten von LDAPS muss ein reales Zertifikat (kein selbst signiertes Zertifikat) sein.
     * In der Domäne müssen Reverse-DNS-Zonen für den IP-Adressbereich des HDI-Subnetzes erstellt werden.
     * Es wird ein Dienst- oder ein Benutzerkonto benötigt, das zum Erstellen des HDInsight-Clusters verwendet wird. Dieses Konto muss die folgenden Berechtigungen besitzen:
        * Berechtigungen zum Erstellen von Dienstprinzipalobjekten und Computerobjekten innerhalb der Organisationseinheit
        * Berechtigungen zum Erstellen von Reverse-DNS-Proxyregeln
        * Berechtigungen zum Einbinden von Computern in die Active Directory-Domäne
        
2. HDInsight, über VPN-Setup in ein lokales Active Directory integriert
  
  Diese Architektur entspricht der Architektur im ersten Szenario. Der einzige Unterschied besteht darin, dass sich in diesem Fall Ihr Active Directory in Ihrem lokalen System befindet und der Zugriff von HDInsight auf das Active Directory über eine [VPN-Verbindung von Azure mit dem lokalen Netzwerk] erfolgt (https://docs.microsoft.com/de-de/azure/expressroute/expressroute-introduction). Das Architekturdiagramm für dieses Setup ist unten dargestellt. 
  
  Voraussetzungen, die im lokalen Active Directory eingerichtet werden müssen
     * Es muss eine Organisationseinheit erstellt werden, in der Sie die HDInsight-Cluster-VMs und die vom Cluster verwendeten Dienstprinzipale platzieren möchten.
     * Zur Kommunikation mit dem Active Directory muss LDAPS eingerichtet werden. Das Zertifikat zum Einrichten von LDAPS muss ein reales Zertifikat (kein selbst signiertes Zertifikat) sein.
     * In der Domäne müssen Reverse-DNS-Zonen für den IP-Adressbereich des HDI-Subnetzes erstellt werden.
     * Es wird ein Dienst- oder ein Benutzerkonto benötigt, das zum Erstellen des HDInsight-Clusters verwendet wird. Dieses Konto muss die folgenden Berechtigungen besitzen:
        * Berechtigungen zum Erstellen von Dienstprinzipalobjekten und Computerobjekten innerhalb der Organisationseinheit
        * Berechtigungen zum Erstellen von Reverse-DNS-Proxyregeln
        * Berechtigungen zum Einbinden von Computern in die Active Directory-Domäne


Die URLs für Log Analytics unterscheiden sich in Azure Government:

| Service Type | Azure – Öffentlich | Azure Government |
| --- | --- | --- |
| HDInsight-Cluster | \*.azurehdinsight.net | \*.azurehdinsight.us |

Weitere Informationen finden Sie in der [öffentlichen Dokumentation zu Azure HDInsight](../hdinsight/hdinsight-hadoop-introduction.md).


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen und Updates erhalten Sie, indem Sie den <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government-Blog</a> abonnieren.



<!--HONumber=Jan17_HO2-->


