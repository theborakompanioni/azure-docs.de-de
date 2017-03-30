---
title: "Dedizierte Kapazität für Machine Learning-Batchausführungsdienst-Aufträge | Microsoft-Dokumentation"
description: "Übersicht über Azure Batch-Dienste für Machine Learning-Aufträge."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: 
ms.service: machine-learning
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: v-donglo
translationtype: Human Translation
ms.sourcegitcommit: afe143848fae473d08dd33a3df4ab4ed92b731fa
ms.openlocfilehash: e75099e1ca7e3bbfc427883a8c343d773f3923ae
ms.lasthandoff: 03/17/2017


---
# <a name="azure-batch-service-for-machine-learning-jobs"></a>Azure Batch-Dienst für Machine Learning-Aufträge

Machine Learning Batch-Pool-Verarbeitung verwendet den [Azure Batch-Dienst](../batch/batch-technical-overview.md), um kundenverwaltete Skalierung für den Azure Machine Learning-Batchausführungsdienst zu ermöglichen. Klassische Batchverarbeitung erfolgt in einer Umgebung mit mehreren Mandanten, wodurch die Anzahl von gleichzeitigen Aufträgen, die Sie senden können, begrenzt wird, und Aufträge werden auf FIFO-Basis in eine Warteschlange gesetzt. Diese Unsicherheit bedeutet, dass Sie nicht genau vorhersagen können, wann Ihr Auftrag ausgeführt wird.

Batch Pool-Verarbeitung ermöglicht Ihnen das Erstellen von Azure Batch-Pools, an die Sie Batchaufträge senden können. Sie steuern die Größe des Pools, und Sie steuern, an welchen der Pool der Auftrag gesendet wird. Ihr BES-Auftrag wird in seinem eigenen Verarbeitungsbereich ausgeführt, wodurch sich eine vorhersagbare Verarbeitungsleistung und die Möglichkeit ergeben, Ressourcenpools zu erstellen, die an die von Ihnen gesendete Verarbeitungslast angepasst sind.

## <a name="how-to-use-batch-pool-processing"></a>Verwenden von Batch-Pool-Verarbeitung

Um Batch-Pool-Verarbeitung verwenden zu können, benötigen Sie Folgendes:

-   Ein Batch-Pool-Konto, das eine Pool-Dienst-URL und einen Autorisierungsschlüssel hat
-   Einen Neuer Resource Manager-basierten Webdiensttarif und Abrechnungsplan

Um Ihr Konto zu erstellen, rufen Sie Microsoft-Kundendienst und -Support (Microsoft Customer Service and Support, CSS) an, und geben Sie Ihre Abonnement-ID an. CSS bestimmt in Absprache mit Ihnen die geeignete Kapazität für Ihr Szenario. CSS konfiguriert anschließend Ihr Konto mit der maximalen Anzahl von Speicherpools, die Sie erstellen können, und der maximalen Anzahl von virtuellen Computern (VMs), die Sie in jedem Pool platzieren können. Sobald Ihr Konto konfiguriert ist, werden Ihnen Ihre Pool-Dienst-URL und ein Autorisierungsschlüssel bereitgestellt.

Nachdem Ihr Konto erstellt wurde, verwenden Sie die Pool-Dienst-URL und den Autorisierungsschlüssel, um Ihren Batch-Pool zu verwalten.

![Architektur des Batch-Pool-Diensts](media/machine-learning-dedicated-capacity-for-bes-jobs/pool-architecture.png)

Sie erstellen Pools, indem Sie den Vorgang „Pool erstellen“ für die Pool-Dienst-URL aufrufen, die CSS Ihnen bereitgestellt hat. Wenn Sie einen Pool erstellen, geben Sie die Anzahl von VMs und die URL der „swagger.json“-Datei eines Neuer Resource Manager-basierten Machine Learning-Webdiensts an. Dieser Webdienst wird bereitgestellt, um die Abrechnungszuordnung einzurichten. Der Batch-Pool-Dienst verwendet die Datei „swagger.json“ dazu, den Pool einem Abrechnungsplan zuzuordnen. Sie können jeden BES-Webdienst ausführen (sowohl den Neuer Resource Manager-basierten als auch den klassischen), den Sie für den Pool auswählen.

Sie können jeden Neuer Resource Manager-basierten Webdienst verwenden, sollten aber beachten, dass die Abrechnung für die Aufträge entsprechend dem Abrechnungsplan erfolgt, der diesem Dienst zugeordnet ist. Es bietet sich möglicherweise an, dass Sie einen Webdienst und einen neuen Abrechnungsplan speziell für das Ausführen von Batch-Pool-Aufträgen erstellen.

Weitere Informationen zum Erstellen von Webdiensten finden Sie unter [Bereitstellen von Azure Machine Learning-Webdiensten](machine-learning-publish-a-machine-learning-web-service.md).

Nachdem Sie einen Pool erstellt haben, können Sie den BES-Auftrag über die Batchanforderungs-URL für den Webdienst senden. Sie können auswählen, den Auftrag an einen Pool oder an eine klassische Batchverarbeitung zu senden. Um einen Auftrag an Batch-Pool-Verarbeitung zu senden, fügen Sie dem Textkörper der Anforderung, in der der Auftrag gesendet wird, den folgenden Parameter hinzu:

"AzureBatchPoolId": "&lt;Pool-ID&gt;"

Wenn Sie den-Parameter nicht hinzufügen, wird der Auftrag in der klassischen Batchprozessumgebung ausgeführt. Hat der Pool verfügbare Ressourcen, wird der Auftrag sofort gestartet. Hat der Pool keine freien Ressourcen, wird Ihr Auftrag in die Warteschlange gesetzt, bis eine Ressource verfügbar ist.

Wenn Sie feststellen, dass Sie regelmäßig die Kapazität Ihres Pools ausschöpfen und daher mehr Kapazität benötigen, können Sie CSS anrufen und in Zusammenarbeit mit einem Mitarbeiter Ihre Kontingente erhöhen.

Beispiel für eine Anforderung:

https://ussouthcentral.services.azureml.net/subscriptions/80c77c7674ba4c8c82294c3b2957990c/services/9fe659022c9747e3b9b7b923c3830623/jobs?api-version=2.0

```json
{

    "Input":{
    
        "ConnectionString":"DefaultEndpointsProtocol=https;BlobEndpoint=https://sampleaccount.blob.core.windows.net/;TableEndpoint
        =https://sampleaccount.table.core.windows.net/;QueueEndpoint=https://sampleaccount.queue.core.windows.net/;FileEndpoint=https://zhguim
        l.file.core.windows.net/;AccountName=sampleaccount;AccountKey=&lt;Key&gt;;",
        
        "BaseLocation":null,
        
        "RelativeLocation":"testint/AdultCensusIncomeBinaryClassificationDataset.csv",
        
        "SasBlobToken":null
    
    },
    
    "GlobalParameters":{ },
    
    "Outputs":{
    
        "output1":{
        
            "ConnectionString":"DefaultEndpointsProtocol=https;BlobEndpoint=https://sampleaccount.blob.core.windows.net/;TableEndpo
            int=https://sampleaccount.table.core.windows.net/;QueueEndpoint=https://sampleaccount.queue.core.windows.net/;FileEndpoint=https://sampleaccount.file.core.windows.net/;AccountName=sampleaccount;AccountKey=&lt;Key&gt;",
            "BaseLocation":null,
            "RelativeLocation":"testintoutput/testint\_results.csv",
            
            "SasBlobToken":null
        
        }
    
    },
    
    "AzureBatchPoolId":"8dfc151b0d3e446497b845f3b29ef53b"

}
```

## <a name="considerations-when-using-batch-pool-processing"></a>Überlegungen zur Verwendung von Batch-Pool-Verarbeitung

Batch-Pool-Verarbeitung ist ein Dienst mit zeitgenauer Abrechnung, und das erfordert, dass Sie ihn einem Resource Manager-basierten Abrechnungsplan zuordnen. Es wird nur die Anzahl von Computestunden in Rechnung gestellt, die der Pool ausgeführt wird. Die Anzahl von Aufträgen, die während dieser Zeit im Pool ausgeführt werden, spielt keine Rolle. Wenn Sie einen Pool erstellen, werden die Computestunden für jeden virtuellen Computer in dem Pool in Rechnung gestellt, bis der Pool gelöscht wurde. Dies gilt auch dann, wenn keine Batchaufträge im Pool ausgeführt werden. Die Abrechnung für die virtuellen Computer wird gestartet, wenn deren Bereitstellung abgeschlossen ist, und wird beendet, wenn sie gelöscht wurden. Sie können jeden der Pläne verwenden, die auf der Seite [Machine Learning – Preise](https://azure.microsoft.com/pricing/details/machine-learning/) zu finden sind.

Abrechnungsbeispiel:

Wenn Sie einen Batch-Pool mit 2 virtuellen Computern erstellen und diesen nach 24 Stunden löschen, wird Ihr Abrechnungsplan mit 48 Computestunden belastet, und zwar unabhängig davon, wie viele Aufträge innerhalb dieses Zeitraums ausgeführt wurden.

Wenn Sie einen Batch-Pool mit 4 virtuellen Computern erstellen und diesen nach 12 Stunden löschen, wird Ihr Abrechnungsplan ebenfalls mit 48 Computestunden belastet.

Es empfiehlt sich, dass Sie den Auftragsstatus abfragen, um zu ermitteln, wann Aufträge abgeschlossen sind. Wenn Ihre Aufträge abgeschlossen sind, rufen Sie den Vorgang zur Poolgrößenänderung auf, um die Anzahl von virtuellen Computern im Pool auf null festzulegen. Wenn Ihre Poolressourcen nicht mehr ausreichen und Sie einen neuen Pool erstellen müssen, z. B. um nach einem anderen Abrechnungsplan abzurechnen, können Sie den Pool stattdessen löschen, nachdem Ihre Aufträge abgeschlossen sind.


| **Verwenden von Batch-Pool-Verarbeitung**    | **Verwenden von klassischer Batchverarbeitung**  |
|---|---|
|Sie müssen eine große Anzahl von Aufträgen ausführen.<br>Oder<br/>Sie müssen sicher sein, dass Ihre Aufträge sofort ausgeführt werden.<br/>Oder<br/>Sie benötigen einen garantierten Durchsatz. Beispielsweise müssen Sie eine Anzahl von Aufträgen in einem bestimmten Zeitrahmen ausführen und möchten Ihre Compute-Ressourcen entsprechend Ihren Anforderungen skalieren.    | Sie führen nur wenige Aufträge aus.<br/>und<br/> Es ist nicht erforderlich, dass die Aufträge sofort ausgeführt werden. |

