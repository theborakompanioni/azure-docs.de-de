---
title: "Azure Service Fabric – programmgesteuertes Skalieren| Microsoft-Dokumentation"
description: "Programmgesteuertes horizontales Herunter- oder Hochskalieren eines Azure Service Fabric-Clusters gemäß benutzerdefinierten Triggern"
services: service-fabric
documentationcenter: .net
author: mjrousos
manager: jonjung
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/10/2017
ms.author: mikerou
translationtype: Human Translation
ms.sourcegitcommit: afe143848fae473d08dd33a3df4ab4ed92b731fa
ms.openlocfilehash: 8d7052fabeb348b4bba744b43d9af78f058175a8
ms.lasthandoff: 03/17/2017


---

# <a name="scale-a-service-fabric-cluster-programmatically"></a>Programmgesteuertes Skalieren eines Service Fabric-Clusters 

Grundlagen zum Skalieren eines Service Fabric-Clusters in Azure finden Sie in der Dokumentation zu [Clusterskalierung](./service-fabric-cluster-scale-up-down.md). Der Artikel beschreibt, wie Service Fabric-Cluster auf Skalierungsgruppen für virtuelle Computer aufgebaut werden und entweder manuell oder mit automatischen Skalierungsregeln skaliert werden können. In diesem Dokument werden programmgesteuerte Methoden für die Koordination von Azure-Skalierungsvorgängen für komplexere Szenarien behandelt. 

## <a name="reasons-for-programmatic-scaling"></a>Gründe für die programmgesteuerte Skalierung
In vielen Szenarien ist die manuelle oder automatisch geregelte Skalierung eine gute Lösung. In anderen Szenarien ist sie jedoch möglicherweise nicht geeignet. Mögliche Nachteile dieser Ansätze:

- Das manuelle Skalieren erfordert, dass Sie sich anmelden und Skalierungsvorgänge explizit anfordern. Wenn Skalierungsvorgänge häufig oder zu unvorhersehbaren Zeiten erforderlich sind, ist dieser Ansatz u.U. keine geeignete Lösung.
- Wenn Regeln für automatische Skalierung eine Instanz aus einer VM-Skalierungsgruppe entfernen, entfernen sie nicht automatisch Informationen dieses Knotens aus dem zugeordneten Service Fabric-Cluster, es sei denn, der Knotentyp weist die Dauerhaftigkeitsstufe „Gold“ oder „Silber“ auf. Da Regeln für automatische Skalierung auf Skalierungsgruppenebene gelten (anstatt auf Service Fabric-Ebene), können sie Service Fabric-Knoten entfernen, ohne diese ordnungsgemäß heruntergefahren. Diese grobe Knotenentfernung hinterlässt Service Fabric-Knoten nach Vorgängen zum horizontalen Herunterskalieren in verwaistem Zustand. Eine Person (oder ein Dienst) müsste den Zustand entfernter Knoten im Service Fabric-Cluster in regelmäßigen Abständen bereinigen.
  - Hinweis: Ein Knotentyp mit Dauerhaftigkeitsstufe „Gold“ oder „Silber“ bereinigt entfernte Knoten automatisch.  
- Zwar werden [viele Metriken](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md) von Regeln für automatische Skalierung unterstützt, es handelt sich jedoch immer noch um einen begrenzten Satz. Wenn Ihr Szenario die Skalierung basierend auf einer Metrik erfordert, die in diesem Satz nicht enthalten ist, sind Regeln für automatische Skalierung möglicherweise keine gute Wahl.

Aufgrund dieser Einschränkungen möchten Sie möglicherweise stärker benutzerdefinierte Modelle für die automatische Skalierung implementieren. 

## <a name="scaling-apis"></a>Skalieren von APIs
Azure-APIs sind vorhanden, sodass Anwendungen programmgesteuert mit den VM-Skalierungsgruppen und Service Fabric-Clustern arbeiten können. Wenn vorhandene Optionen zur automatischen Skalierung für Ihr Szenario nicht funktionieren, ermöglichen diese APIs das Implementieren einer benutzerdefinierten Skalierungslogik. 

Ein Verfahren zur Implementierung dieser selbst erstellen Funktionalität für automatische Skalierung ist das Hinzufügen eines neuen zustandslosen Diensts zur Service Fabric-Anwendung, um Skalierungsvorgänge zu verwalten. Innerhalb der `RunAsync`-Methode des Dienstes kann ein Satz von Triggern ermitteln, ob eine Skalierung erforderlich ist (dabei werden auch Parameter wie z.B. die maximale Clustergröße überprüft und cooldown-Einstellungen skaliert).   

Die für Interaktionen mit der VM-Skalierungsgruppe (sowohl zum Überprüfen als auch zum Ändern der aktuellen Anzahl von virtuellen Computerinstanzen) verwendete API ist die Fluent-Bibliothek [Azure.Management.Compute](https://www.nuget.org/packages/Microsoft.Azure.Management.Compute.Fluent/1.0.0-beta50). Die Fluent-Compute-Bibliothek stellt eine benutzerfreundliche API für die Interaktion mit VM-Skalierungsgruppen bereit.

Verwenden Sie für die Interaktion mit dem Service Fabric-Cluster selbst [System.Fabric.FabricClient](/dotnet/api/system.fabric.fabricclient).

Natürlich muss der Skalierungscode nicht als Dienst im Cluster ausgeführt werden, um skaliert zu werden. Sowohl `IAzure` als auch `FabricClient` können eine Remoteverbindung mit ihren jeweils zugeordneten Azure-Ressoucen herstellen, damit der Skalierungsdienst einfach eine Konsolenanwendung oder ein Windows-Dienst sein kann, die bzw. der außerhalb der Service Fabric-Anwendung ausgeführt werden kann. 

## <a name="credential-management"></a>Verwaltung von Anmeldeinformationen
Eine Herausforderung beim Schreiben von einem Dienst für die Skalierung besteht darin, dass der Dienst ohne eine interaktive Anmeldung Zugriff auf VM-Skalierungsgruppenressourcen haben muss. Der Zugriff auf den Service Fabric-Cluster ist einfach, wenn der Skalierungsdienst seine eigene Service Fabric-Anwendung ändert, für den Zugriff auf die Skalierungsgruppe jedoch Anmeldeinformationen erforderlich sind. Für die Anmeldung können Sie einen mit [Azure CLI 2.0](https://github.com/azure/azure-cli) erstellen [Dienstprinzipal](https://github.com/Azure/azure-sdk-for-net/blob/Fluent/AUTH.md#creating-a-service-principal-in-azure) verwenden.

Ein Dienstprinzipal kann mit den folgenden Schritten erstellt werden:

1. Melden Sie sich bei der Azure CLI (`az login`) als Benutzer mit Zugriff auf die VM-Skalierungsgruppe an.
2. Erstellen Sie den Dienstprinzipal mit `az ad sp create-for-rbac`.
    1. Notieren Sie sich App-ID (an anderer Stelle als „Client-ID“ bezeichnet), Name, Kennwort und Mandant für die spätere Verwendung.
    2. Sie benötigen auch Ihre Abonnement-ID, die mit `az account list` angezeigt werden kann.

Die Fluent-Compute-Bibliothek kann sich mit diesen Anmeldeinformationen wie folgt anmelden:

```C#
var credentials = AzureCredentials.FromServicePrincipal(AzureClientId, AzureClientKey, AzureTenantId, AzureEnvironment.AzureGlobalCloud);
IAzure AzureClient = Azure.Authenticate(credentials).WithSubscription(AzureSubscriptionId);

if (AzureClient?.SubscriptionId == AzureSubscriptionId)
{
    ServiceEventSource.Current.ServiceMessage(Context, "Successfully logged into Azure");
}
else
{
    ServiceEventSource.Current.ServiceMessage(Context, "ERROR: Failed to login to Azure");
}
```

Nach der Anmeldung kann die Anzahl von Skalierungsgruppeninstanzen über `AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId).Capacity` abgefragt werden.

## <a name="scaling-out"></a>Horizontales Skalieren
Mithilfe des Azure Fluent-Compute-SDKs können der VM-Skalierungsgruppe mit nur wenigen Aufrufe Instanzen hinzugefügt werden:

```C#
var scaleSet = AzureClient?.VirtualMachineScaleSets.GetById(ScaleSetId);
var newCapacity = Math.Min(MaximumNodeCount, NodeCount.Value + 1);
scaleSet.Update().WithCapacity(newCapacity).Apply(); 
``` 

**Zurzeit verhindert [ein Fehler](https://github.com/Azure/azure-sdk-for-net/issues/2716), dass dieser Code funktioniert**, es wurde jedoch eine Korrektur zusammengeführt, sodass das Problem in veröffentlichten Versionen von Microsoft.Azure.Management.Compute.Fluent in Kürze gelöst werden sollte. Der Fehler bewirkt, dass beim Ändern der Eigenschaften (z.B Kapazität) der VM-Skalierungsgruppe mit der Fluent-Compute-API geschützte Einstellungen aus der Resource Manager-Vorlage der Skalierungsgruppe verloren gehen. Diese fehlenden Einstellungen führen u. a. dazu, dass Service Fabric-Dienste auf neuen virtuellen Computerinstanzen nicht ordnungsgemäß eingerichtet werden.

Als vorübergehende Problemumgehung können im Skalierungsdienst PowerShell-Cmdlets aufgerufen werden, welche die gleiche Änderung umsetzen (dazu müssen jedoch PowerShell-Tools vorhanden sein):

```C#
using (var psInstance = PowerShell.Create())
{
    psInstance.AddScript($@"
        $clientId = ""{AzureClientId}""
        $clientKey = ConvertTo-SecureString -String ""{AzureClientKey}"" -AsPlainText -Force
        $Credential = New-Object -TypeName ""System.Management.Automation.PSCredential"" -ArgumentList $clientId, $clientKey
        Login-AzureRmAccount -Credential $Credential -ServicePrincipal -TenantId {AzureTenantId}
        
        $vmss = Get-AzureRmVmss -ResourceGroupName {ResourceGroup} -VMScaleSetName {NodeTypeToScale}
        $vmss.sku.capacity = {newCapacity}
        Update-AzureRmVmss -ResourceGroupName {ResourceGroup} -Name {NodeTypeToScale} -VirtualMachineScaleSet $vmss
    ");

    psInstance.Invoke();

    if (psInstance.HadErrors)
    {
        foreach (var error in psInstance.Streams.Error)
        {
            ServiceEventSource.Current.ServiceMessage(Context, $"ERROR adding node: {error.ToString()}");
        }
    }                
}
```

Wie beim manuellen Hinzufügen eines Knotens sollte zum Starten eines neuen Service Fabric-Knotens nur das Hinzufügen einer Skalierungsgruppe erforderlich sein, da die Skalierungsgruppenvorlage Erweiterungen beinhaltet, um dem Service Fabric-Cluster automatisch neue Instanzen hinzuzufügen. 

## <a name="scaling-in"></a>Horizontales Herunterskalieren

Das horizontale Herunterskalieren ähnelt dem horizontalen Hochskalieren. Die tatsächlichen Änderungen an der VM-Skalierungsgruppe sind praktisch identisch. Aber wie zuvor erläutert bereinigt Service Fabric nur entfernte Knoten mit Dauerhaftigkeitsstufe „Gold“ oder „Silber“ automatisch. Im Falle einer horizontalen Herunterskalierung bei Dauerhaftigkeitsstufe „Bronze“ ist daher eine Interaktion mit dem Service Fabric-Cluster erforderlich, um den Knoten herunterzufahren, ihn zu entfernen und dann seinen Zustand zu entfernen.

Die Vorbereitung des Knotens auf das Herunterfahren umfasst das Suchen und Deaktivieren des zu entfernenden Knotens (der zuletzt hinzugefügte Knoten). Für Knoten ohne Ausgangswert können neuere Knoten durch Vergleichen von `NodeInstanceId` gefunden werden. 

```C#
using (var client = new FabricClient())
{
    var mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync())
        .Where(n => n.NodeType.Equals(NodeTypeToScale, StringComparison.OrdinalIgnoreCase))
        .Where(n => n.NodeStatus == System.Fabric.Query.NodeStatus.Up)
        .OrderByDescending(n => n.NodeInstanceId)
        .FirstOrDefault();
```

Beachten Sie, dass Knoten mit *Ausgangswert* scheinbar nicht immer der Konvention folgen, dass die IDs größerer Instanzen zuerst entfernt werden.

Sobald der zu entfernende Knoten gefunden wurde, kann er mit denselben `FabricClient`- und `IAzure`-Instanzen wie zuvor deaktiviert und entfernt werden.

```C#
var scaleSet = AzureClient?.VirtualMachineScaleSets.GetById(ScaleSetId);

// Remove the node from the Service Fabric cluster
ServiceEventSource.Current.ServiceMessage(Context, $"Disabling node {mostRecentLiveNode.NodeName}");
await client.ClusterManager.DeactivateNodeAsync(mostRecentLiveNode.NodeName, NodeDeactivationIntent.RemoveNode);

// Wait (up to a timeout) for the node to gracefully shutdown
var timeout = TimeSpan.FromMinutes(5);
var waitStart = DateTime.Now;
while ((mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Up || mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Disabling) &&
        DateTime.Now - waitStart < timeout)
{
    mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync()).FirstOrDefault(n => n.NodeName == mostRecentLiveNode.NodeName);
    await Task.Delay(10 * 1000);
}

// Decrement VMSS capacity
var newCapacity = Math.Max(MinimumNodeCount, NodeCount.Value - 1); // Check min count 

scaleSet.Update().WithCapacity(newCapacity).Apply(); 
```

Sobald die virtuelle Computerinstanz entfernt wurde, kann der Zustand des Service Fabric-Knotens entfernt werden.

```C#
await client.ClusterManager.RemoveNodeStateAsync(mostRecentLiveNode.NodeName);
```

Wie zuvor müssen Sie `IVirtualMachineScaleSet.Update()` umgehen, das erst funktioniert, wenn das Problem [Azure/azure-sdk-for-net#2716](https://github.com/Azure/azure-sdk-for-net/issues/2716) behoben wurde.

## <a name="potential-drawbacks"></a>Mögliche Nachteile

Wie in den vorhergehenden Codeausschnitten veranschaulicht, bietet das Erstellen eines eigenen Skalierungsdiensts das höchsten Maß an Kontrolle und Individualisierbarkeit für das Skalierungsverhalten Ihrer Anwendung. Dies kann für Szenarien hilfreich sein, die eine genaue Kontrolle darüber erfordern, wann oder wie eine Anwendung horizontal herunter- oder hochskaliert wird. Diese Kontrolle bringt jedoch den Nachteil von komplexem Code mit sich. Für diesen Ansatz benötigen Sie anspruchsvollen eigenen Skalierungscode.

Der geeignete Ansatz für die Service Fabric-Skalierung hängt von Ihrem Szenario ab. Wenn selten skaliert wird, ist die Möglichkeit zum manuellen Hinzufügen oder Entfernen von Knoten wahrscheinlich ausreichend. Bei komplexeren Szenarien stellen Regeln für automatische Skalierung und SDKs, die eine programmgesteuerte Skalierung ermöglichen, leistungsfähige Alternativen dar.

## <a name="next-steps"></a>Nächste Schritte

Für die ersten Schritte mit der Implementierung Ihrer eigenen Logik für automatische Skalierung machen Sie sich mit den folgenden Konzepten und hilfreichen APIs vertraut:

- [Manuelles Skalieren oder mit automatischen Skalierungsregeln](./service-fabric-cluster-scale-up-down.md)
- [Azure-Fluent-Verwaltungsbibliotheken für .NET](https://github.com/Azure/azure-sdk-for-net/tree/Fluent) (nützlich für die Interaktion mit den einem Service Fabric-Cluster zugrunde liegenden VM-Skalierungsgruppen)
- [System.Fabric.FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) (nützlich für die Interaktion mit einem Service Fabric-Cluster und dessen Knoten)
