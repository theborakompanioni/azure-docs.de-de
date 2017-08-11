---
title: "Ausführen von Azure Batch-Workloads auf kostengünstigen VMs mit niedriger Priorität (Vorschau) | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie VMs mit niedriger Priorität bereitstellen, um die Kosten von Azure Batch-Workloads zu verringern."
services: batch
author: mscurrell
manager: timlt
ms.assetid: dc6ba151-1718-468a-b455-2da549225ab2
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.workload: na
ms.date: 07/21/2017
ms.author: markscu
ms.translationtype: HT
ms.sourcegitcommit: 22aa82e5cbce5b00f733f72209318c901079b665
ms.openlocfilehash: 9bf0ac322020d8a8453011c3207c1930175db6d3
ms.contentlocale: de-de
ms.lasthandoff: 07/24/2017

---

# <a name="use-low-priority-vms-with-batch-preview"></a>Verwenden von VMs mit niedriger Priorität mit Batch (Vorschau)

Azure Batch bietet virtuelle Computer (VMs) mit niedriger Priorität, um die Kosten für Batch-Workloads zu verringern. Durch VMs mit niedriger Priorität werden neue Typen von Batch-Workloads möglich, indem eine große Menge an Computeleistung bereitgestellt wird, die gleichzeitig wirtschaftlich ist.

VMs mit niedriger Priorität nutzen überschüssige Kapazitäten in Azure. Wenn Sie VMs mit niedriger Priorität in Pools angeben, kann Azure Batch diesen Überschuss automatisch nutzen, sobald er zur Verfügung steht.

Der Nachteil der Verwendung von VMs mit niedriger Priorität ist, dass diese VMs vorzeitig entfernt werden können, wenn keine überschüssigen Kapazitäten in Azure verfügbar sind. Aus diesem Grund sind VMs mit niedriger Priorität am besten für bestimmte Arten von Workloads geeignet. Verwenden Sie VMs mit niedriger Priorität für Batch und asynchrone Verarbeitungsworkloads, bei denen der Zeitpunkt des Auftragsabschlusses flexibel ist und die Arbeit über viele VMs verteilt wird.

VMs mit niedriger Priorität sind deutlich weniger kostenaufwendig als dedizierte VMs. Ausführliche Preisinformationen finden Sie unter [Preise für Batch](https://azure.microsoft.com/pricing/details/batch/).

Zusätzliche Erläuterungen zu VMs mit niedriger Priorität finden Sie in der Blogbeitragsankündigung: [Batchcomputing zu einem Bruchteil des Preises](https://azure.microsoft.com/blog/announcing-public-preview-of-azure-batch-low-priority-vms/).

> [!IMPORTANT]
> VMs mit niedriger Priorität sind derzeit in der Vorschau verfügbar und stehen nur für in Batch ausgeführte Workloads zur Verfügung. 
>
>

## <a name="use-cases-for-low-priority-vms"></a>Anwendungsfälle für VMs mit niedriger Priorität

Welche Workloads sind aufgrund der besonderen Merkmale von VMs mit niedriger Priorität für diese geeignet? Im Allgemeinen sind Batch-Workloads gut geeignet, da die Aufträge in viele parallele Aufgaben aufgeteilt werden oder viele Aufträge vorhanden sind, die horizontal hochskaliert und auf viele VMs verteilt werden.

-   Zur Maximierung der Verwendung von überschüssigen Kapazitäten in Azure können geeignete Aufträge horizontal hochskaliert werden.

-   Gelegentlich sind die VMs möglicherweise nicht verfügbar oder werden vorzeitig entfernt. Dies führt zu einer verringerten Kapazität für Aufträge und damit möglicherweise zu Unterbrechungen und einem erneutem Ausführen von Aufgaben. Aufträge müssen daher flexibel in der zeitlichen Dauer ihrer Ausführung sein.

-   Aufträge mit längeren Aufgaben werden möglicherweise durch Unterbrechungen stärker beeinträchtigt. Wenn für lang andauernde Aufgaben Prüfpunkte zum Speichern von Fortschritten bei der Ausführung implementiert werden, wirken sich Unterbrechungen erheblich weniger stark aus. Aufgaben mit kürzeren Ausführungszeiten funktionieren im Allgemeinen am besten mit VMs mit niedriger Priorität, da Unterbrechungen deutlich geringere Auswirkungen haben.

-   Lang andauernde MPI-Aufträge, die mehrere VMs nutzen, sind für die Verwendung von VMs mit niedriger Priorität nicht gut geeignet, da eine vorzeitig entfernte VM wahrscheinlich dazu führt, dass der gesamte Auftrag erneut ausgeführt werden muss.

Einige Beispiele für Anwendungsfälle mit Batchverarbeitung, die gut für VMs mit niedriger Priorität geeignet sind:

-   **Entwicklung und Tests:** Insbesondere bei der Entwicklung von umfangreichen Lösungen können beträchtliche Einsparungen realisiert werden. Alle Arten von Tests können profitieren, aber umfangreiche Auslastungstests und Regressionstests sind besonders gut geeignete Fälle.

-   **Ergänzen der bedarfsgesteuerten Kapazität:** VMs mit niedriger Priorität können zum Ergänzen normaler dedizierter VMs verwendet werden – bei Verfügbarkeit können Aufträge skaliert und daher schneller und zu niedrigeren Kosten abgeschlossen werden; bei Nichtverfügbarkeit stehen die normalerweise bereitgestellten dedizierten VMs zur Verfügung.

-   **Flexible Ausführungszeit:** Wenn die Zeit, in der Aufträge abgeschlossen werden müssen, flexibel ist, können potenzielle Rückgänge in der Kapazität toleriert werden. Durch das Hinzufügen von VMs mit niedriger Priorität werden Aufträge werden jedoch häufig schneller und kostengünstiger ausgeführt.

Batchpools können auf verschiedene Weise für VMs mit niedriger Priorität konfiguriert werden, je nachdem, wie flexibel die Ausführungszeit des Auftrags ist:

-   VMs mit niedriger Priorität können ausschließlich in Pools verwendet werden, und Batch stellt vorzeitig entfernte Kapazitäten einfach wieder bereit, wenn sie verfügbar werden. Dies ist die kostengünstigste Möglichkeit, Aufträge auszuführen, da ausschließlich VMs mit niedriger Priorität verwendet werden.

-   VMs mit niedriger Priorität können in Verbindung mit einer festen Grundausstattung von dedizierten VMs verwendet werden. Die feste Anzahl von dedizierten VMs stellt sicher, dass immer eine gewisse Kapazität zum Fortsetzen der Verarbeitung eines Auftrags verbleibt.

-   Dedizierte VMs und VMs mit niedriger Priorität können dynamisch gemischt werden, sodass ausschließlich die kostengünstigeren VMs mit niedriger Priorität verwendet werden, wenn sie verfügbar sind, bei Bedarf jedoch die dedizierten VMs zum vollen Preis zentral hochskaliert werden, damit eine Mindestmenge an Kapazität für die fortlaufende Verarbeitung der Aufträge verfügbar bleibt.

## <a name="batch-support-for-low-priority-vms"></a>Unterstützung von VMs mit niedriger Priorität in Batch

Azure Batch stellt verschiedene Funktionen bereit, die die vorteilhafte Nutzung von VMs mit niedriger Priorität erleichtern:

-   Batchpools können sowohl dedizierte VMs als auch VMs mit niedriger Priorität enthalten. Die Anzahl der einzelnen VM-Typen kann beim Erstellen eines Pools angegeben und für einen bestehenden Pool mithilfe der ausdrücklichen Größenänderung oder der automatischen Skalierung jederzeit geändert werden. Die Auftrags- und Aufgabenübermittlung kann unverändert bleiben und muss nicht an die VM-Typen im Pool angepasst werden. Es ist auch möglich, in einem Speicherpool ausschließlich VMs mit niedriger Priorität zu verwenden, um Aufträge möglichst kostengünstig ausführen zu können, und die Anzahl der dedizierten VMs zu erhöhen, wenn die Kapazität unter einen Mindestschwellenwert fällt, damit die Aufträge weiterhin ausgeführt werden.

-   In Batchpools wird automatisch die Zielanzahl von VMs mit niedriger Priorität angestrebt. Wenn VMs vorzeitig entfernt werden, versucht Batch, die verlorene Kapazität auszugleichen und das Ziel wieder zu erreichen.

-   Wenn Aufgaben unterbrochen werden, erkennt Batch dies automatisch und stellt Aufgaben für die erneute Ausführung wieder in die Warteschlange.

-   VMs mit niedriger Priorität verfügen über ein Kernkontingent, das von dem dedizierter VMs abweicht. 
    Das Kontingent für VMs mit niedriger Priorität ist höher als bei dedizierten VMs, da VMs mit niedriger Priorität weniger kosten. Weitere Informationen finden Sie im Artikel [Batch-Dienst – Kontingente und Grenzwerte](batch-quota-limit.md#resource-quotas).    

> [!NOTE]
> VMs mit niedriger Priorität werden derzeit für Batch-Konten, in denen der Poolzuteilungsmodus auf [Benutzerabonnement](batch-account-create-portal.md#user-subscription-mode) festgelegt ist, nicht unterstützt.
>
>

## <a name="create-and-update-pools"></a>Erstellen und Aktualisieren von Pools

Ein Batchpool kann sowohl dedizierte VMs als auch VMs mit niedriger Priorität (auch als Serverknoten bezeichnet) enthalten. Sie können die Zielanzahl der Serverknoten für dedizierte VMs und VMs mit niedriger Priorität festlegen. Die Zielanzahl der Knoten gibt die Anzahl von VMs an, die im Pool vorhanden sein sollen.

Um beispielsweise einen Pool von Azure-Clouddienst-VMs mit einem Ziel von 5 dedizierten VMs und 20 VMs mit niedriger Priorität zu erstellen, gehen Sie folgendermaßen vor:

```csharp
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: "cspool",
    targetDedicatedComputeNodes: 5,
    targetLowPriorityComputeNodes: 20,
    virtualMachineSize: "Standard_D2_v2",
    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4") // WS 2012 R2
);
```

Um einen Pool von Azure-VMs (in diesem Fall virtuelle Linux-Computer) mit einem Ziel von 5 dedizierten VMs und 20 VMs mit niedriger Priorität zu erstellen, gehen Sie folgendermaßen vor:

```csharp
ImageReference imageRef = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    sku: "16.04.0-LTS",
    version: "latest");

// Create the pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration("batch.node.ubuntu 16.04", imageRef);

pool = batchClient.PoolOperations.CreatePool(
    poolId: "vmpool",
    targetDedicatedComputeNodes: 5,
    targetLowPriorityComputeNodes: 20,
    virtualMachineSize: "Standard\_D2\_v2",
    virtualMachineConfiguration: virtualMachineConfiguration);
```

Sie können die aktuelle Anzahl der Knoten für dedizierte VMs und VMs mit niedriger Priorität abrufen:

```csharp
int? numDedicated = pool1.CurrentDedicatedComputeNodes;
int? numLowPri = pool1.CurrentLowPriorityComputeNodes;
```

Poolknoten weisen eine Eigenschaft auf, die angibt, ob es sich bei einem Knoten um eine dedizierte VM oder eine VM mit niedriger Priorität handelt:

```csharp
bool? isNodeDedicated = poolNode.IsDedicated;
```

Wenn ein oder mehrere Knoten in einem Pool vorzeitig entfernt werden, gibt ein Auflisten der Knoten für den Pool diese Knoten weiterhin zurück, die aktuelle Anzahl von Knoten mit niedriger Priorität bleibt unverändert, aber der Status dieser Knoten ist auf den Status **Vorzeitig entfernt** festgelegt. Batch versucht, Ersatz-VMs zu finden, und bei Erfolg durchlaufen die Knoten die Zustände **Erstellen** und dann **Starten**, bevor sie für die Aufgabenausführung verfügbar sind – genau wie neue Knoten.

## <a name="scale-a-pool-containing-low-priority-vms"></a>Skalieren eines Pools mit VMs mit niedriger Priorität

Wie bei Pools, die ausschließlich aus dedizierten VMs bestehen, kann auch ein Pool mit VMs mit niedriger Priorität durch Aufrufen der Methode zum Ändern der Größe oder mithilfe der automatischen Skalierung skaliert werden.

Der Vorgang zum Ändern der Poolgröße nimmt einen zweiten optionalen Parameter entgegen, der den Wert von **targetLowPriorityNodes** aktualisiert:

```csharp
pool.Resize(targetDedicatedComputeNodes: 0, targetLowPriorityComputeNodes: 25);
```

Die Formel für die automatische Skalierung des Pools unterstützt VMs mit niedriger Priorität wie folgt:

-   Den Wert der vom Dienst definierten Variable **$TargetLowPriorityNodes** können Sie abrufen oder festlegen.

-   Den Wert der vom Dienst definierten Variable **$CurrentLowPriorityNodes** können Sie abrufen.

-   Den Wert der vom Dienst definierten Variable **$PreemptedNodeCount** können Sie abrufen. 
    Diese Variable gibt die Anzahl der Knoten mit dem Status „Vorzeitig entfernt“ zurück und ermöglicht Ihnen, die Anzahl der dedizierten Knoten abhängig von der Anzahl der nicht verfügbaren vorzeitig entfernten Knoten zentral hoch oder herunter zu skalieren.

## <a name="jobs-and-tasks"></a>Aufträge und Aufgaben

Aufträge und Aufgaben erfordern nur geringe Unterstützung für Knoten mit niedriger Priorität – die einzige Unterstützung ist die folgende:

-   Die JobManagerTask-Eigenschaft eines Auftrags hat eine neue Eigenschaft **AllowLowPriorityNode**. 
    Wenn diese Eigenschaft auf TRUE festgelegt ist, kann die Auftrags-Manager-Aufgabe auf einem dedizierten oder einem Knoten mit niedriger Priorität geplant werden. Wenn diese Eigenschaft auf FALSE festgelegt ist, wird die Auftrags-Manager-Aufgabe ausschließlich auf einen dedizierten Knoten geplant.

-   Für eine Aufgabenanwendung steht eine [Umgebungsvariable](batch-compute-node-environment-variables.md) zur Verfügung, sodass ermittelt werden kann, ob die Anwendung auf einem Knoten mit niedriger Priorität oder einem dedizierten Knoten ausgeführt wird. Die Umgebungsvariable lautet AZ_BATCH_NODE_IS_DEDICATED.

## <a name="handling-preemption"></a>Behandeln der vorzeitigen Entfernung

VMs werden gelegentlich vorzeitig entfernt. In diesem Fall geschieht in Batch Folgendes:

-   Der Status der vorzeitig entfernten VMs wird in **Vorzeitig entfernt** geändert.
-   Wenn auf den VMs der vorzeitig entfernten Knoten Aufgaben ausgeführt wurden, werden diese Aufgaben wieder in die Warteschlange gestellt und erneut ausgeführt.
-   Die VM wird effektiv gelöscht, sodass alle auf der VM lokal gespeicherten Daten verloren gehen.
-   Der Pool versucht fortlaufend, die Zielanzahl der verfügbaren Knoten mit niedriger Priorität zu erreichen. Wenn Ersatzkapazitäten gefunden werden, behalten die Knoten ihre IDs bei, werden jedoch erneut initialisiert und durchlaufen wieder die Zustände **Erstellen** und **Starten**, bevor sie zum Planen von Aufgaben verfügbar sind.
-   Die Anzahl der vorzeitigen Entfernungen ist als Metrik im Azure-Portal verfügbar.

## <a name="metrics"></a>Metriken

Im [Azure-Portal](https://portal.azure.com) sind neue Metriken für Knoten mit niedriger Priorität verfügbar. Diese Metriken sind:

- Anzahl der Knoten mit niedriger Priorität
- Anzahl von Kernen mit niedriger Priorität 
- Anzahl der vorzeitig entfernten Knoten

Metriken im Azure-Portal anzeigen:

1. Navigieren Sie zu Ihrem Batch-Konto im Verwaltungsportal, und zeigen Sie die Einstellungen für Ihr Batch-Konto an.
2. Wählen Sie **Metriken** im Abschnitt **Überwachung** aus.
3. Wählen Sie die Metriken, die Sie erhalten möchten, aus der Liste **Verfügbare Metriken** aus.

![Metriken für Knoten mit niedriger Priorität](media/batch-low-pri-vms/low-pri-metrics.png)

## <a name="next-steps"></a>Nächste Schritte

* Sehen Sie sich die [Übersicht über Batch-Features für Entwickler](batch-api-basics.md) an. Diese enthält wichtige Informationen für Entwickler, die sich auf die Verwendung von Batch vorbereiten möchten. Der Artikel enthält ausführliche Informationen zu Batch-Dienstressourcen wie Pools, Knoten, Aufträgen und Aufgaben sowie zu den zahlreichen API-Funktionen, die beim Erstellen der Batch-Anwendung verwendet werden können.
* Informieren Sie sich über die [Batch-APIs und Tools](batch-apis-tools.md), die für die Erstellung von Batch-Lösungen verfügbar sind.

