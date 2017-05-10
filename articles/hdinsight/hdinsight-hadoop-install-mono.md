---
title: Installieren oder Aktualisieren von Mono in HDInsight | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie eine bestimmte Version von Mono im HDInsight-Cluster verwenden. Mono dient zum Ausführen von .NET-Anwendungen in Linux-basierten HDInsight-Clustern."
services: hdinsight
documentationCenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/01/2017
ms.author: larryfr
ms.custom: hdinsightactive
ms.translationtype: Human Translation
ms.sourcegitcommit: 64bd7f356673b385581c8060b17cba721d0cf8e3
ms.openlocfilehash: dc303b5386d48cd7daad1630a35c8b8366a9c19f
ms.contentlocale: de-de
ms.lasthandoff: 05/02/2017


---

# <a name="install-or-update-mono-on-hdinsight"></a>Installieren oder Aktualisieren von Mono in HDInsight

Erfahren Sie, wie Sie eine bestimmte Version von [Mono](https://www.mono-project.com) in HDInsight 3.4 oder höher installieren.

Mono wird in HDInsight 3.4 oder höher installiert und zum Ausführen von .NET-Anwendungen verwendet. Informationen zur mit jeder HDInsight-Version bereitgestellten Version von Mono finden Sie unter [HDInsight-Komponentenversionen](hdinsight-component-versioning.md). Um eine andere Version im Cluster zu installieren, verwenden Sie die Skriptaktion in diesem Dokument. 

## <a name="how-it-works"></a>So funktioniert's

Dieses Skript akzeptiert die folgenden Parameter:

* __Mono-Versionsnummer__: Die zu installierende Version von Mono. Die Version muss unter [https://download.mono-project.com/repo/debian/dists/wheezy/snapshots/](https://download.mono-project.com/repo/debian/dists/wheezy/snapshots/) verfügbar sein.

Mit dem Skript werden die folgenden Mono-Pakete installiert:

* __mono-complete__

* __ca-certificates-mono__

## <a name="the-script"></a>Das Skript

__Speicherort des Skripts__: [https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash](https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash)

__Anforderungen__:

* Das Skript muss auf die __Hauptknoten__ und __Workerknoten__ angewendet werden.

## <a name="to-use-the-script"></a>So verwenden Sie das Skript

Informationen zum Verwenden dieses Skripts mit HDInsight finden Sie im Dokument [Anpassen Linux-basierter HDInsight-Cluster mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster). Sie können das Skript über das Azure-Portal, Azure PowerShell oder die Azure CLI ausführen.

Verwenden Sie beim Befolgen des Skriptaktionsdokuments den folgenden URI:

    https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash

> [!NOTE]
> Wenn Sie HDInsight mit diesem Skript konfigurieren, markieren Sie das Skript als __Persisted__. Mit dieser Einstellung kann HDInsight das Skript auf Workerknoten anwenden, die über Skalierungsvorgänge hinzugefügt wurden.


## <a name="next-steps"></a>Nächste Schritte

Sie haben erfahren, wie Sie eine bestimmte Version von Mono in HDInsight aktualisieren oder installieren. Weitere Informationen zur Verwendung von .NET-Anwendungen mit Mono in HDInsight finden Sie in den folgenden Dokumenten:

* [Verwenden von .NET für Streaming-MapReduce in HDInsight](hdinsight-hadoop-dotnet-csharp-mapreduce-streaming.md)
* [Verwenden von .NET mit Hive und Pig in HDInsight](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)
* [Entwickeln von C#-Lösungen mit Storm in HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md)
* [Migrieren von .NET-Lösungen zu Linux-basiertem HDInsight](hdinsight-hadoop-migrate-dotnet-to-linux.md)

Weitere Informationen zu Skriptaktionen finden Sie unter [Anpassen Linux-basierter HDInsight-Cluster mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md).
