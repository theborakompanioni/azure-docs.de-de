---
title: DNS-Dienst in Azure Service Fabric | Microsoft-Dokumentation
description: Verwenden Sie den DNS-Dienst von Azure Service Fabric zum Ermitteln von Microservices aus dem Cluster heraus.
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: vturecek
ms.assetid: 47f5c1c1-8fc8-4b80-a081-bc308f3655d3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 5/9/2017
ms.author: msfussell
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: e0f6a3a91207b73320d60a498d635262ef730d89
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="dns-service-in-azure-service-fabric"></a>DNS-Dienst in Azure Service Fabric
Der DNS-Dienst ist ein optionaler Systemdienst, den Sie in Ihrem Cluster aktivieren können, um andere Dienste mithilfe des DNS-Protokolls zu ermitteln.

Viele Dienste, insbesondere in Containern gepackte Dienste, können über einen vorhandenen URL-Namen verfügen. Diese über das standardmäßige DNS-Protokoll (statt über das Naming Service-Protokoll) auflösen zu können, ist sehr praktisch, besonders in Szenarien mit „Lift & Shift“-Anwendungen. Mit dem DNS-Dienst können Sie einem Dienstnamen DNS-Namen zuordnen und so Endpunkt-IP-Adressen auflösen. 

Wie im folgenden Diagramm dargestellt, ordnet der im Service Fabric-Cluster ausgeführte DNS-Dienst den Dienstnamen DNS-Namen zu, die dann durch den Naming Service aufgelöst werden und die Endpunktadressen zurückgeben, mit denen die Verbindung hergestellt werden soll. Der DNS-Name für den Dienst wird zum Zeitpunkt der Erstellung bereitgestellt. 

![Dienstendpunkte][0]

## <a name="enabling-the-dns-service"></a>Aktivieren des DNS-Diensts
Zuerst müssen Sie den DNS-Dienst in Ihrem Cluster aktivieren. Rufen Sie die Vorlage für den Cluster ab, den Sie bereitstellen möchten. Sie können entweder die [Beispielvorlagen](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) verwenden oder eine Resource Manager-Vorlage erstellen. Sie aktivieren den DNS-Dienst mit den folgenden Schritten:

1. Überprüfen Sie zunächst, ob `apiversion` für die `Microsoft.ServiceFabric/clusters`-Ressource auf `2017-07-01-preview` festgelegt ist, wie im folgenden Codeausschnitt gezeigt. Liegt eine andere Einstellung vor, müssen Sie `apiVersion` auf den Wert `2017-07-01-preview` aktualisieren.

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. Jetzt aktivieren Sie den DNS-Dienst, wie unten abgebildet, indem Sie folgenden `addonFeatures`-Abschnitt nach dem Abschnitt `fabricSettings` hinzufügen.

    ```json
        "fabricSettings": [
        ...      
        ],
        "addonFeatures": [
            "DnsService"
        ],
    ```

3. Nachdem Sie die Clustervorlage aktualisiert haben diese ändern, wenden Sie sie an, und lassen Sie das Upgrade fortfahren. Sobald der Vorgang abgeschlossen ist, wird der in Ihrem Cluster ausgeführte DNS-Systemdienst mit dem Namen `fabric:/System/DnsService` im Abschnitt der Systemdienste Service Fabric-Explorer angezeigt. 

## <a name="setting-the-dns-name-for-your-service"></a>Festlegen des DNS-Namens für den Dienst
Nachdem der DNS-Dienst nun in Ihrem Cluster ausgeführt wird, können Sie einen DNS-Namen für Ihre Dienste entweder deklarativ für Standarddienste in `ApplicationManifest.xml` oder über Powershell festlegen.

### <a name="setting-the-dns-name-for-a-default-service-in-the-applicationmanifestxml"></a>Festlegen des DNS-Namens für einen Standarddienst in der Datei „ApplicationManifest.xml“
Öffnen Sie das Projekt in Visual Studio oder Ihrem bevorzugten Editor, und öffnen Sie die Datei `ApplicationManifest.xml`. Navigieren Sie zum Abschnitt der Standarddienste, und fügen Sie für jeden Dienst das Attribut `ServiceDnsName` hinzu. Im folgenden Beispiel wird gezeigt, wie Sie den DNS-Namen des Diensts auf `service1.application1` festlegen.

```xml
    <Service Name="Stateless1" ServiceDnsName="service1.application1">
    <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
        <SingletonPartition />
    </StatelessService>
    </Service>
```
Stellen Sie die Anwendung jetzt bereit. Nachdem die Anwendung bereitgestellt wurde, navigieren Sie im Service Fabric-Explorer zur Dienstinstanz, wo der DNS-Name für diese Instanz wie im Folgenden angezeigt wird. 

![Dienstendpunkte][1]

### <a name="setting-the-dns-name-for-a-service-using-powershell"></a>Festlegen des DNS-Namens für einen Dienst mithilfe von Powershell
Sie können den DNS-Namen für einen Dienst bei dessen Erstellung mit dem Powershell-Befehl `New-ServiceFabricService` festlegen. Im folgenden Beispiel wird ein neuer zustandsloser Dienst namens mit dem DNS-Namen `service1.application1` erstellt.

```powershell
    New-ServiceFabricService `
    -Stateless `
    -PartitionSchemeSingleton `
    -ApplicationName `fabric:/application1 `
    -ServiceName fabric:/application1/service1 `
    -ServiceTypeName Service1Type `
    -InstanceCount 1 `
    -ServiceDnsName service1.application1
```

## <a name="using-dns-in-your-services"></a>Verwenden von DNS in Ihren Diensten
Wenn Sie mehr als einen Dienst bereitstellen, finden Sie die Endpunkte der Dienste, mit denen Sie kommunizieren möchten, mithilfe eines DNS-Namens. Dies gilt nur für zustandslose Dienste, da das DNS Protokoll bei zustandsbehafteten Diensten nicht weiß, mit wem die Kommunikation erfolgen soll. Bei zustandsbehafteten Diensten können Sie den integrierten Reverseproxy für HTTP-Aufrufe verwenden, um eine bestimmte Dienstpartition aufzurufen.

Der folgende Code zeigt, wie Sie einen anderen Dienst aufrufen. Dabei handelt es sich einfach um einen regulären HTTP-Aufruf. Beachten Sie, dass Sie den Port und ggf. den optionalen Pfad als Teil der URL angeben müssen.

```csharp
public class ValuesController : Controller
{
    // GET api
    [HttpGet]
    public async Task<string> Get()
    {
        string result = "";
        try
        {
            Uri uri = new Uri("http://service1.application1:8080/api/values");
            HttpClient client = new HttpClient();
            var response = await client.GetAsync(uri);
            result = await response.Content.ReadAsStringAsync();
            
        }
        catch (Exception e)
        {
            Console.Write(e.Message);
        }

        return result;
    }
}
```

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über Dienstkommunikation innerhalb des Clusters unter [Herstellung einer Verbindung mit Diensten und Kommunikation mit diesen Diensten](service-fabric-connect-and-communicate-with-services.md).

[0]: ./media/service-fabric-connect-and-communicate-with-services/dns.png
[1]: ./media/service-fabric-dnsservice/servicefabric-explorer-dns.PNG

