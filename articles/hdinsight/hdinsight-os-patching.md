---
title: "Konfigurieren des Zeitplans für das Patchen des Betriebssystems für Linux-basierte HDInsight-Cluster –Azure | Microsoft-Dokumentation"
description: "Erfahren Sie, wie der Zeitplan für das Patchen des Betriebssystems für Linux-basierte HDInsight-Cluster konfiguriert wird."
services: hdinsight
documentationcenter: 
author: bprakash
manager: asadk
editor: bprakash
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/21/2017
ms.author: bhanupr
ms.translationtype: Human Translation
ms.sourcegitcommit: 3bbc9e9a22d962a6ee20ead05f728a2b706aee19
ms.openlocfilehash: af3c5a19ae8e2e606e4b0506f9f6dddb41192e40
ms.contentlocale: de-de
ms.lasthandoff: 06/10/2017


---

# <a name="os-patching-for-hdinsight"></a>Patchen des Betriebssystems für HDInsight 
Als verwalteter Hadoop-Dienst erledigt HDInsight das Patchen des Betriebssystems für die zugrundeliegenden virtuellen Computer, die von HDInsight-Clustern verwendet werden. Mit Wirkung ab dem 1. August 2016 haben wir die Richtlinie für das Patchen von Gastbetriebssystemen für Linux-basierte HDInsight-Cluster (Version 3.4 oder höher) geändert. Ziel der neuen Richtlinie ist es, die aufgrund von Patching erforderlichen Neustarts erheblich zu reduzieren. Im Rahmen der neuen Richtlinie werden Patches für virtuelle Computer in Linux-Clustern weiterhin jeden Montag oder Donnerstag ab 0:00 Uhr UTC schrittweise auf allen Knoten in jedem Cluster aufgespielt. Die einzelnen virtuellen Computer werden jedoch nur noch maximal alle 30 Tage aufgrund von Patchingvorgängen für das Gastbetriebssystem neu gestartet. Auch der erste Neustart eines neu erstellten Clusters erfolgt nicht früher als 30 Tage nach Erstellungsdatum des Clusters. Patches werden wirksam, sobald die virtuellen Computer neu gestartet werden.

## <a name="how-to-configure-the-os-patching-schedule-for-linux-based-hdinsight-clusters"></a>Konfigurieren des Zeitplans für das Patchen des Betriebssystems für Linux-basierte HDInsight-Cluster
Die virtuellen Computer in einem HDInsight-Cluster müssen gelegentlich neu gestartet werden, damit wichtige Sicherheitspatches installiert werden können. Seit dem 1. August 2016 werden neue Linux-basierte HDInsight-Cluster (Version 3.4 oder höher) gemäß dem folgenden Zeitplan neu gestartet:

1. Ein virtueller Computer im Cluster kann zum Patchen höchstens einmal in einem Zeitraum von 30 Tagen neu gestartet werden.
2. Der Neustart erfolgt beginnend um 00:00 Uhr UTC.
3. Der Neustartvorgang erfolgt für die virtuellen Computer im Cluster gestaffelt, sodass der Cluster während des Neustartvorgangs weiterhin verfügbar ist.
4. Der erste Neustart für einen neu erstellten Cluster erfolgt nicht innerhalb der ersten 30 Tage nach dem Erstellungsdatum des Clusters.

Mithilfe der in diesem Artikel beschriebenen Skriptaktion können Sie den Zeitplan für Patches des Betriebssystems in folgender Weise ändern:
1. Aktivieren oder Deaktivieren des automatischen Neustarts
2. Festlegen der Häufigkeit von Neustarts (in Tagen zwischen Neustarts)
3. Festlegen des Wochentags, an dem Neustarts vorgenommen werden

> [!NOTE]
> Diese Skriptaktion funktioniert nur mit Linux-basierten HDInsight-Clustern, die nach dem 1. August 2016 erstellt wurden. Patches werden nur wirksam, wenn die virtuellen Computer neu gestartet werden. 
>

## <a name="how-to-use-the-script"></a>Verwendung des Skripts 

Zur Verwendung des Skripts benötigen Sie die folgenden Informationen:
1. Den Skriptspeicherort: https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv01/os-patching-reboot-config.sh.
    HDInsight verwendet diesen URI zum Suchen und Ausführen des Skripts auf allen virtuellen Computern im Cluster.
  
2. Clusterknotentypen, auf die das Skript angewendet wird: Hauptknoten, Arbeitsknoten, Zookeeper. Dieses Skript muss auf alle Knotentypen im Cluster angewendet werden. Wenn es auf einen Knotentyp nicht angewendet wird, verwenden die virtuellen Computer für den betreffenden Knotentyp weiterhin den vorherigen Patchzeitplan.


3.  Parameter: Dieses Skript akzeptiert drei numerische Parameter:

    | -Parameter enthalten. | Definition |
    | --- | --- |
    | Aktivieren/Deaktivieren des automatischen Neustarts |0 oder 1. Der Wert 0 deaktiviert den automatischen Neustart, 1 aktiviert ihn. |
    | Frequency |7 bis 90 (einschließlich). Die Anzahl der Tage, die vor dem Neustart zum Patchen von virtuellen Computern, für die ein Neustart erforderlich ist, gewartet wird. |
    | Wochentag |1 bis 7 (einschließlich). Der Wert 1 gibt an, dass der Neustart an einem Montag stattfinden soll, 7 gibt den Sonntag an. Die Parameter 1 60 2 bewirken beispielsweise einen automatischen Neustart alle 60 Tage (höchstens) am Dienstag. |
    | Persistenz |Wenn Sie eine Skriptaktion auf einen vorhandenen Cluster anwenden, können Sie das Skript als permanent kennzeichnen. Permanente Skripts werden angewendet, wenn dem Cluster durch Skalierungsvorgänge neue Arbeitsknoten hinzugefügt werden. |

> [!NOTE]
> Sie müssen dieses Skript als permanent kennzeichnen, wenn Sie es auf einen vorhandenen Cluster anwenden. Andernfalls verwenden durch Skalierungsvorgänge erstellte neue Knoten den Standardpatchzeitplan.
Wenn Sie das Skript im Rahmen der Clustererstellung anwenden, ist es automatisch permanent.
>

## <a name="next-steps"></a>Nächste Schritte

Spezifische Schritte zur Verwendung der Skriptaktion finden Sie in den folgenden Abschnitten in [Anpassen von Linux-basierten HDInsight-Clustern mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md):

* [Verwenden einer Skriptaktion während der Clustererstellung](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation)
* [Anwenden einer Skriptaktion auf einen ausgeführten Cluster](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster)

