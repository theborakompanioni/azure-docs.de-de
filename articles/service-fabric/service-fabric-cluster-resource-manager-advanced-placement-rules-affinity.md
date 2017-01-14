---
title: "Clusterressourcen-Manager von Service Fabric – Affinität | Microsoft Docs"
description: "Übersicht über die Konfiguration der Affinität für Service Fabric-Dienste"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 678073e1-d08d-46c4-a811-826e70aba6c4
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/19/2016
ms.author: masnider
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 70c7209c0971c7015f585a5b5afcc8881c9f9f1a


---
# <a name="configuring-and-using-service-affinity-in-service-fabric"></a>Konfigurieren und Verwenden der Dienstaffinität in Service Fabric
Affinität ist ein Steuerelement, das hauptsächlich bereitgestellt wird, um den Übergang von größeren monolithischen Anwendungen in die Welt der Cloud und Microservices zu vereinfachen. Sie kann in bestimmten Fällen auch als legitime Optimierung zum Verbessern der Leistung von Diensten verwendet werden, obwohl dabei Nebeneffekte auftreten können.

Angenommen, Sie überführen eine größere App bzw. eine, die nicht mit Blick auf Microservices entwickelt wurde, in Service Fabric. Dieser Vorgang ist tatsächlich ziemlich üblich, und wir hatten einige Kunden (intern und extern) in dieser Situation. Sie beginnen mit dem Überführen der gesamten App in die Umgebung, verpacken sie und führen sie aus. Anschließend unterteilen Sie sie in verschiedene kleinere Dienste, die alle miteinander kommunizieren.

Doch dann ertönt ein „Huch“. Das „Huch“ gehört meist zu einer dieser Kategorien:

1. Eine Komponente X in der monolithischen App verfügt über eine nicht dokumentierte Abhängigkeit von Komponente Y, und die beiden Komponenten wurden soeben in separate Dienste umgewandelt. Die beiden Komponenten wurden voneinander getrennt und werden jetzt auf unterschiedlichen Knoten innerhalb des Clusters ausgeführt.
2. Diese Elemente kommunizieren über (lokale Named Pipes | freigegebenen Speicher | Dateien auf dem Datenträger), doch ich muss unbedingt in der Lage sein, sie unabhängig voneinander zu aktualisieren, um die Sache ein wenig zu beschleunigen. Ich entferne die harte Abhängigkeit später.
3. Alles ist in Ordnung, doch es stellt sich heraus, dass diese beiden Komponenten sehr kommunikativ bzw. leistungsabhängig sein. Beim Verschieben der Komponenten in unterschiedliche Dienste hat die Leistung der Anwendung rapide abgenommen, oder die Latenz ist gestiegen. Folglich wird die erwartete Leistung der Anwendung nicht mehr erreicht.

Nun muss eine Lösung gefunden werden, bei der der Umgestaltungsaufwand nicht umsonst war und nicht erneut auf die monolithische Anwendung zurückgegriffen wird. Allerdings muss die Positionierung der Komponenten berücksichtigt werden. Diese Lösung muss umgesetzt werden, bis die Komponenten so neu entworfen werden können, dass sie selbst als Dienste ausgeführt werden, oder bis die erwartete Leistung auf andere Weise erreicht wird.

Vorgehensweise Wir könnten also versuchen, mit Affinität zu arbeiten.

## <a name="how-to-configure-affinity"></a>Konfigurieren von Affinität
Um für Affinität zu sorgen, definieren Sie eine Affinitätsbeziehung zwischen zwei Diensten. Sie können sich das so vorstellen, dass ein Dienst auf einen anderen „zeigt“ und angibt, dass dieser Dienst nur dort ausgeführt werden kann, wo auch der andere ausgeführt wird. Mitunter wird Affinität als Beziehung über- und untergeordneter Dienste beschrieben (wobei der untergeordnete auf den übergeordneten Dienst zeigt). Mithilfe von Affinität wird sichergestellt, dass die Replikate oder Instanzen eines Diensts auf denselben Knoten platziert werden wie die Replikate oder Instanzen eines anderen Diensts.

``` csharp
ServiceCorrelationDescription affinityDescription = new ServiceCorrelationDescription();
affinityDescription.Scheme = ServiceCorrelationScheme.Affinity;
affinityDescription.ServiceName = new Uri("fabric:/otherApplication/parentService");
serviceDescription.Correlations.Add(affinityDescription);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

## <a name="different-affinity-options"></a>Verschiedene Affinitätsoptionen
Affinität wird mithilfe eines von mehreren Korrelationschemen dargestellt und weist zwei verschiedene Modi auf. Der gängigste Affinitätsmodus ist, was wir als „NonAlignedAffinity“ bezeichnen. Bei „NonAlignedAffinity“ werden die Replikate oder Instanzen der anderen Dienste auf denselben Knoten platziert. Der andere Modus heißt „AlignedAffinity“. AlignedAffinity wird ausschließlich für zustandsbehaftete Dienste verwendet. Durch die Konfiguration von zwei zustandsbehafteten Diensten mit AlignedAffinity wird sichergestellt, dass die primären Replikate dieser Dienste auf denselben Knoten platziert werden wie die des anderen Diensts. Außerdem wird jedes Paar aus sekundären Replikaten für diese Dienste auf denselben Knoten platziert. Es ist auch möglich, (jedoch nicht so üblich) „NonAlignedAffinity“ für zustandsbehaftete Dienste zu konfigurieren. Bei NonAlignedAffinity werden die verschiedenen Replikate der beiden zustandsbehafteten Dienste auf denselben Knoten platziert, es wird jedoch nicht versucht, die primären oder sekundären Replikate auszurichten.

![Affinitätsmodi und ihre Auswirkungen][Image1]

### <a name="best-effort-desired-state"></a>Anstreben des gewünschten Zustands
Es gibt einige Unterschiede zwischen Affinität und monolithischen Architekturen. Viele dieser Unterschiede sind dadurch begründet, dass mit einer Affinitätsbeziehung ein gewünschter Zustand angestrebt wird. Die Dienste in einer Affinitätsbeziehung sind unterschiedliche Entitäten, die unabhängig voneinander ausfallen und verschoben werden können. Es gibt auch Gründe, weshalb eine Affinitätsbeziehung nicht aufrechterhalten werden kann. Ein Beispiel sind Kapazitätseinschränkungen, aufgrund derer nur einige Dienstobjekte der Affinitätsbeziehung auf einem bestimmten Knoten platziert werden können. In diesen Fällen besteht zwar eine Affinitätsbeziehung, aufgrund der anderen Einschränkungen kann sie jedoch nicht erzwungen werden. Wenn es zu einem späteren Zeitpunkt möglich ist, alle anderen Einschränkungen und die Affinität zu erzwingen, wird die Verletzung der Affinitätseinschränkung automatisch korrigiert.  

### <a name="chains-vs-stars"></a>Ketten im Vergleich zu Sternen
Derzeit können wir Ketten von Affinitätsbeziehungen nicht modellieren. Dies bedeutet, dass ein Dienst, der ein untergeordneter Dienst einer Affinitätsbeziehung ist, kein übergeordneter Dienst in einer anderen Affinitätsbeziehung sein kann. Wenn Sie diesen Typ von Beziehung modellieren möchten, müssen Sie sie als Stern und nicht als Kette modellieren. Dabei würde das übergeordnete Element des „mittleren“ untergeordneten Elements als übergeordnetes Element für das unterste untergeordnete Element verwendet. Abhängig von der Anordnung der Dienste muss möglicherweise ein Dienst als „Platzhalter“ erstellt werden, um für mehrere untergeordnete Elemente als übergeordnetes Element zu dienen.

![Ketten im Vergleich zu Sternen im Kontext von Affinitätsbeziehungen ][Image2]

Eine weitere Eigenschaft derzeitiger Affinitätsbeziehungen ist, dass sie gerichtet sind. Mit dieser Affinitätsregel wird also lediglich erzwungen, dass das untergeordnete Element dort positioniert werden muss, wo sich das übergeordnete Element befindet. Wenn für das übergeordnete Element z.B. plötzlich ein Failover auf einen anderen Knoten durchgeführt wird, bemerkt der Clusterressourcen-Manager das Problem erst, wenn er feststellt, dass das untergeordnete Element nicht gemeinsam mit dem übergeordneten Element platziert ist. Die Beziehung wird nicht umgehend erzwungen.

### <a name="partitioning-support"></a>Unterstützung der Partitionierung
Der letzte wichtige Affinitätsaspekt ist, dass Affinitätsbeziehungen nicht unterstützt werden, wenn das übergeordnete Element partitioniert ist. Dies wird ggf. künftig unterstützt werden, ist derzeit aber nicht zulässig.

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zu den anderen Optionen, die für die Konfiguration von Diensten zur Verfügung stehen, finden Sie im Thema zu den anderen verfügbaren Clusterressourcen-Manager-Konfigurationen unter [Konfigurieren von Diensten](service-fabric-cluster-resource-manager-configure-services.md)
* Viele Gründe, weshalb Benutzer Affinität verwenden, z.B. das Einschränken von Diensten auf eine kleine Gruppe von Computern, und der Versuch, die Last einer Sammlung von Diensten zu aggregieren, werden besser durch Anwendungsgruppen unterstützt. Weitere Informationen finden Sie unter [Anwendungsgruppen](service-fabric-cluster-resource-manager-application-groups.md).

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resrouce-manager-affinity-modes.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resource-manager-chains-vs-stars.png



<!--HONumber=Dec16_HO2-->


