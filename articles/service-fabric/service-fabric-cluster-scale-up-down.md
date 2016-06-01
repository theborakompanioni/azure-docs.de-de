<properties
   pageTitle="Zentrales Hoch- oder Herunterskalieren eines Service Fabric-Clusters | Microsoft Azure"
   description="Skalieren Sie einen Service Fabric-Cluster bedarfsgerecht zentral hoch oder herunter, indem Sie für jeden Knotentyp bzw. VM-Skalierungsgruppe Regeln für die automatische Skalierung festlegen."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/04/2016"
   ms.author="chackdan"/>


# Zentrales Hoch- oder Herunterskalieren eines Service Fabric-Clusters mithilfe von Regeln für die automatische Skalierung

Skalierungsgruppen von virtuellen Computern sind eine Azure-Computeressource, mit der Sie eine Sammlung von virtuellen Computern als Gruppe bereitstellen und verwalten können. Jeder Knotentyp, der in einem Service Fabric-Cluster definiert ist, wird als separate VM-Skalierungsgruppe festgelegt. Jeden Knotentyp kann dann unabhängig zentral hoch- oder herunterskaliert werden, bei jedem Typ können unterschiedliche Portgruppen geöffnet sein, und die Typen können verschiedene Kapazitätsmetriken aufweisen. Weitere Informationen finden Sie in dem Dokument über die [Service Fabric-Knotentypen](service-fabric-cluster-nodetypes.md). Da die Service Fabric-Knotentypen in Ihrem Cluster am Backend aus VM-Skalierungsgruppen bestehen, müssen Sie für jeden Knotentyp bzw. jede VM-Skalierungsgruppe Regeln für die automatische Skalierung einrichten.

>[AZURE.NOTE] Ihr Abonnement muss ausreichend Kerne zum Hinzufügen der neuen virtuellen Computer umfassen, aus denen dieser Cluster bestehen soll. Gegenwärtig erfolgt keine Modellvalidierung. Daher wird für die Bereitstellungszeit ein Fehler ausgegeben, wenn eine der Kontingentgrenzen erreicht wird.

## Auswählen des zu skalierenden Knotentyps bzw. der VM-Skalierungsgruppe

Zurzeit können Sie mithilfe des Portals keine Regeln für die automatische Skalierung für VM-Skalierungsgruppen angeben. Daher verwenden wir Azure PowerShell (1.0 und höher), um die Knotentypen aufzulisten und anschließend automatische Skalierungregeln hinzuzufügen.

Führen Sie die folgenden Cmdlets aus, um eine Liste der VM-Skalierungsgruppen abzurufen, aus denen Ihr Cluster besteht:

```powershell
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Network/VirtualMachineScaleSets

Get-AzureRmVmss -ResourceGroupName <RGname> -VMScaleSetName <VM Scale Set name>
```

## Festlegen von automatischen Skalierungsregeln für Knotentypen bzw. VM-Skalierungsgruppen

Wenn Ihr Cluster mehrere Knotentypen enthält, müssen Sie dies für jeden der Knotentypen bzw. jede der VM-Skalierungsgruppen durchführen, die Sie zentral hoch- oder herunterskalieren möchten. Sie sollten dabei die Anzahl der Knoten berücksichtigen, die Sie benötigen, bevor Sie die automatische Skalierung einrichten. Die minimale Anzahl von Knoten, die für den primären Knotentyp benötigt wird, wird von der Zuverlässigkeitsstufe gesteuert, die Sie ausgewählt haben. Erfahren Sie mehr über [Zuverlässigkeitsstufen](service-fabric-cluster-capacity.md).

>[AZURE.NOTE]  Wenn Sie den primären Knotentyp auf eine geringere Anzahl als das Minimum zentral herunterskalieren, wird der Cluster instabil, oder er schaltet sich ab. Dies könnte für Ihre Anwendung und die Systemdienste zu Datenverlust führen.

Die automatische Skalierungsfunktion wird derzeit nicht durch die Lasten gesteuert, die Ihre Anwendung an Service Fabric meldet. Die bereitgestellte automatische Skalierung wird derzeit ausschließlich durch Leistungsindikatoren gesteuert, die von jeder Instanz der VM-Skalierungsgruppen ausgegeben werden.

Folgen Sie den Anleitungen, um eine [automatische Skalierung für jede VM-Skalierungsgruppe einzurichten](../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview.md).

>[AZURE.NOTE] In einem Szenario, in dem zentral herunterskaliert wird, müssen Sie das Cmdlet [Remove-ServiceFabricNodeState](https://msdn.microsoft.com/library/azure/mt125993.aspx) mit dem Namen des entsprechenden Knotens aufrufen, es sei denn, Ihr Knotentyp besitzt die Dauerhaftigkeitsstufe „Gold“ oder „Silber“.

## Verhaltensweisen von Service Fabric Explorer, die Sie möglicherweise beobachten

Wenn Sie einen Cluster zentral hochskalieren, wird Service Fabric Explorer die Knotenanzahl (Instanzen der VM-Skalierungsgruppen) anzeigen, die zu dem Cluster gehören. Wenn Sie ein Cluster jedoch zentral herunterskalieren, wird Ihnen der entfernte Knoten bzw. die entfernte VM-Instanz weiterhin in einem fehlerhaften Zustand angezeigt, es sei denn, Sie rufen das Cmdlet [Remove-ServiceFabricNodeState](https://msdn.microsoft.com/library/mt125993.aspx) mit dem entsprechenden Knotennamen auf.

Dieses Verhalten erklärt sich wie folgt:

Die in Service Fabric Explorer aufgeführten Knoten sind ein Abbild der Informationen, die die Service Fabric-Systemdienste (besonders FM) über die derzeitige und frühere Anzahl der Knoten des Clusters besitzen. Wenn Sie eine VM-Skalierungsgruppe zentral herunterskalieren, wird der virtuelle Computer gelöscht. Der FM-Systemdienst geht jedoch weiterhin davon aus, dass der Knoten (der dem gelöschten virtuellen Computer zugeordnet war) dem Cluster wieder zur Verfügung stehen wird. Service Fabric Explorer zeigt diesen Knoten daher weiterhin an (der Integritätszustand kann „fehlerhaft“ oder „unbekannt“ lauten).

Sie verfügen über zwei Optionen, um sicherzustellen, dass ein Knoten beim Entfernen eines virtuellen Computers entfernt wird:

1) Wählen Sie die Dauerhaftigkeitsstufe „Gold“ oder „Silber“ (demnächst verfügbar) für die Knotentypen in Ihrem Cluster, die die Infrastrukturintegration für Sie bereitstellt. Wenn Sie zentral herunterskalieren, werden so automatisch die Knoten aus den Systemdiensten (FM) entfernt. Machen Sie sich mit den [Details der Dauerhaftigkeitsstufen](service-fabric-cluster-capacity.md) vertraut.

2) Sobald die VM-Instanz zentral herunterskaliert wurde, müssen Sie das Cmdlet [Remove-ServiceFabricNodeState](https://msdn.microsoft.com/library/mt125993.aspx) aufrufen.

>[AZURE.NOTE] Um Verfügbarkeit sicherzustellen und den Zustand beizubehalten, muss eine bestimmte Anzahl von Knoten in einem Service Fabric-Cluster stets in Betrieb sein. Dies wird auch als „Aufrechterhalten eines Quorums“ bezeichnet. Daher ist es für gewöhnlich nicht sicher, alle Computer innerhalb des Clusters herunterzufahren, sofern Sie nicht zunächst eine [vollständige Sicherung des Zustands](service-fabric-reliable-services-backup-restore.md) durchgeführt haben.

## Nächste Schritte
Lesen Sie die folgenden Artikel, um mehr über die Kapazitätsplanung von Clustern, das Upgraden von Clusters und das Partitionieren von Diensten zu erfahren:

- [Planen Sie Ihre Clusterkapazität](service-fabric-cluster-capacity.md)
- [Clusterupgrades](service-fabric-cluster-upgrade.md)
- [Die Partitionierung statusbehafteter Dienste für maximale Skalierung](service-fabric-concepts-partitioning.md)

<!--Image references-->
[BrowseServiceFabricClusterResource]: ./media/service-fabric-cluster-scale-up-down/BrowseServiceFabricClusterResource.png
[ClusterResources]: ./media/service-fabric-cluster-scale-up-down/ClusterResources.png

<!---HONumber=AcomDC_0518_2016-->