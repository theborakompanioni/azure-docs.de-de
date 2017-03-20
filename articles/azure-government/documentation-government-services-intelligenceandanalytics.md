---
title: Azure Government Intelligence und Analyse | Microsoft-Dokumentation
description: "Dies bietet einen Vergleich der Features und Richtlinien zum Entwickeln von Anwendungen für Azure Government"
services: azure-government
cloud: gov
documentationcenter: 
author: MeganYount
manager: zakramer
ms.assetid: 4b7720c1-699e-432b-9246-6e49fb77f497
ms.service: azure-government
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: azure-government
ms.date: 12/06/2016
ms.author: MeganYount
translationtype: Human Translation
ms.sourcegitcommit: d9dad6cff80c1f6ac206e7fa3184ce037900fc6b
ms.openlocfilehash: 0233aa66bc4f4f135456ec15bd09756e63192b14
ms.lasthandoff: 03/06/2017


---
# <a name="azure-government-intelligence--analytics"></a>Azure Government Intelligence und Analyse
Dieser Artikel beschreibt die Dienste, Varianten und Überlegungen zum Thema Intelligence und Analyse für die Azure Government-Umgebung.

## <a name="hdinsight"></a>HDInsight
HDInsight unter Linux Standard ist in Azure Government allgemein verfügbar. <a href=https://channel9.msdn.com/Blogs/Azure/Cognitive-Services-HDInsight-and-Power-BI-on-Azure-Government/>Hier</a> können Sie sich eine Demo zum Erstellen von datenorientierten Lösungen in Azure Government mithilfe von HDInsight ansehen.

HDInsight unter Linux Premium ist demnächst verfügbar.

### <a name="variations"></a>Variationen
Die folgenden HDInsight-Features sind aktuell nicht in Azure Government verfügbar.

* HDInsight ist unter Windows nicht verfügbar.
* Azure Data Lake Store ist aktuell nicht in Azure Government verfügbar. Azure Blob Storage ist zurzeit die einzige verfügbare Speicheroption.

Die URLs für Log Analytics unterscheiden sich in Azure Government:

| Service Type | Azure – Öffentlich | Azure Government |
| --- | --- | --- |
| HDInsight-Cluster | \*.azurehdinsight.net | \*.azurehdinsight.us |

Weitere Informationen finden Sie in der [öffentlichen Dokumentation zu HDInsight](../hdinsight/hdinsight-hadoop-introduction.md).

## <a name="power-bi"></a>Power BI
Power BI US Government ist als Bestandteil der Office 365 US Government Community-Abonnements allgemein verfügbar. Informationen zu Power BI US Government erhalten Sie <a href=https://powerbi.microsoft.com/en-us/documentation/powerbi-service-govus-overview/>hier</a>. <a href=https://channel9.msdn.com/Blogs/Azure/Cognitive-Services-HDInsight-and-Power-BI-on-Azure-Government/>Hier</a> können Sie sich eine Demo zum Erstellen von datenorientierten Lösungen in Azure Government mithilfe von Power BI ansehen.

Power BI Embedded ist demnächst verfügbar.

### <a name="variations"></a>Variationen

Die URLs für Power BI unterscheiden sich in US Government:

| Service Type | Power BI Commercial | Power BI US Government |
| --- | --- | --- |
| Power BI-URL | app.powerbi.com | app.powerbigov.us |

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen und Updates erhalten Sie, indem Sie den <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government-Blog</a> abonnieren.

