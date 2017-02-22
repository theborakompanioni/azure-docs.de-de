---
title: Erstellen Windows-basierter Hadoop-Cluster in HDInsight | Microsoft-Dokumentation
description: "Hier erfahren Sie, wie Sie über das Azure-Portal Windows-basierte Hadoop-Cluster in HDInsight erstellen."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: c8689ef3-f56f-4708-8a3a-cc00abc54e8d
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/18/2017
ms.author: jgao
translationtype: Human Translation
ms.sourcegitcommit: ede2e4ec5f3414d1c8a17f4c120011eba0d9a6ca
ms.openlocfilehash: a9d7a4372693ad021a898fae9818f2d037d42547


---
# <a name="create-windows-based-hadoop-clusters-in-hdinsight-using-the-azure-portal"></a>Erstellen Windows-basierter Hadoop-Cluster in HDInsight über das Azure-Portal

[!INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Hier erfahren Sie, wie Sie über das Azure-Portal einen Windows-basierten Hadoop-Cluster in HDInsight erstellen. 

Die Informationen in diesem Artikel gelten nur für Windows-basierte HDInsight-Cluster. Informationen zum Erstellen von Linux-basieren Clustern finden Sie unter [Erstellen von Linux-basierten Clustern in HDInsight mithilfe des Azure-Portals](hdinsight-hadoop-create-linux-clusters-portal.md).

> [!IMPORTANT]
> Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight unter Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date).

## <a name="prerequisites"></a>Voraussetzungen:
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Bevor Sie die Anweisungen in diesem Artikel ausführen können, benötigen Sie Folgendes:

* Ein Azure-Abonnement. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

### <a name="access-control-requirements"></a>Voraussetzungen für die Zugriffssteuerung
[!INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-clusters"></a>Erstellen von Clustern
**So erstellen Sie einen HDInsight-Cluster**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie auf **NEU**, auf **Intelligence + Analyse** und anschließend auf **HDInsight**.
3. Geben Sie folgende Werte ein bzw. wählen diese aus:
   
   * **Clustername**: Benennen Sie Ihren Cluster.
   * **Abonnement**: Wählen Sie ein für diesen Cluster verwendetes Azure-Abonnement aus.
   * **Clusterkonfiguration**:

     * **Clustertyp**: Wählen Sie für dieses Tutorial die Option **Hadoop** aus.
     * **Betriebssystem**: Wählen Sie für dieses Tutorial die Option **Windows** aus.
     * **Version**: Wählen Sie **Hadoop 2.7.0 (HDI 3.3)** aus.  Dies ist die neueste Version für Windows-basierte Hadoop-Cluster.
     * **Clustertarif**: Wählen Sie für dieses Tutorial die Option **STANDARD** aus.

   * **Anmeldeinformationen**:

     * **Benutzername für Clusteranmeldung**: Der Standardname lautet **admin**. 
     * **Kennwort für Clusteranmeldung**: Geben Sie ein Kennwort ein.
     * **Remotedesktop aktivieren**: Remotedesktop wird für dieses Tutorial nicht benötigt.

   * **Datenquelle**:

     * **Auswahlmethode**: Wählen Sie **Aus allen Abonnements** aus.
     * **Speicherkonto auswählen**: Klicken Sie auf **Neu erstellen**, und geben Sie einen Namen für das Standard-Speicherkonto ein.
     * **Standardcontainer**: Für den Standardcontainer wird standardmäßig der gleiche Namen verwendet wie für den Cluster.
     * **Standort**: Wählen Sie einen Standort in Ihrer Nähe aus.  Dieser Standort wird sowohl vom Cluster als auch vom Standard-Speicherkonto verwendet.
   * **Clustergröße**:

     * **Anzahl von Workerknoten**: In diesem Tutorial wird lediglich ein einzelner Knoten benötigt.
   * **Erweiterte Konfigurationen**: Diesen Teil können Sie für dieses Tutorial überspringen.
   * **Ressourcengruppe**: Wählen Sie **Neu erstellen** aus, und geben Sie einen Namen ein.

4. Wählen Sie **An Dashboard anheften** aus, und klicken Sie dann auf **Erstellen**. Wenn Sie **An Startmenü anheften** auswählen, wird dem Startmenü Ihres Portals eine Kachel für den Cluster hinzugefügt. Die Clustererstellung dauert zwischen 15 und 20 Minuten. Sie können den Status des Bereitstellungsprozesses auf der Kachel im Startmenü oder im linken Bereich der Seite unter **Benachrichtigungen** überprüfen.
5. Klicken Sie nach Abschluss der Erstellung im Startmenü auf die Kachel für den Cluster, um das Clusterblatt zu öffnen. Auf dem Clusterblatt werden grundlegende Informationen zum Cluster angezeigt, z. B. der Name, die zugehörige Ressourcengruppe, der Standort, das Betriebssystem, die URL für das Cluster-Dashboard usw.

## <a name="customize-clusters"></a>Anpassen von Clustern
* Weitere Informationen finden Sie unter [Anpassen von HDInsight-Clustern mithilfe von Bootstrap](hdinsight-hadoop-customize-cluster-bootstrap.md).
* Weitere Informationen finden Sie unter [Anpassen Windows-basierter HDInsight-Cluster mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster.md).

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie mehrere Möglichkeiten zum Erstellen von HDInsight-Clustern kennengelernt. Weitere Informationen finden Sie in den folgenden Artikeln:

* [Erste Schritte mit Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) – Erfahren Sie, wie Sie die Arbeit mit Ihrem HDInsight-Cluster aufnehmen können.
* [Programmgesteuerte Übermittlung von Hadoop-Jobs](hdinsight-submit-hadoop-jobs-programmatically.md) – Erfahren Sie, wie Sie Aufträge programmgesteuert an HDInsight übermitteln können.
* [Verwalten von Hadoop-Clustern in HDInsight mit dem Azure-Portal](hdinsight-administer-use-management-portal.md)




<!--HONumber=Jan17_HO3-->


