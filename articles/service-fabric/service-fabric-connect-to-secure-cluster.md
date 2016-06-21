<properties
   pageTitle="Authentifizieren des Clientzugriffs auf einen Cluster | Microsoft Azure"
   description="Beschreibt das Authentifizieren des Clientzugriffs auf einen Service Fabric-Cluster mithilfe von Zertifikaten und das Sichern der Kommunikation zwischen Clients und einem Cluster."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/01/2016"
   ms.author="ryanwi"/>

# Herstellen einer Verbindung mit einem sicheren Cluster
Wenn ein Client eine Verbindung mit einem Service Fabric-Clusterknoten herstellt, kann mit der Zertifikatssicherheit der Client authentifiziert und sichere Kommunikation eingerichtet werden. So wird sichergestellt, dass nur autorisierte Benutzer auf den Cluster zugreifen, Anwendungen bereitstellen und Verwaltungsaufgaben ausführen können. Zertifikatssicherheit muss zuvor beim Erstellen des Clusters auf dem Cluster aktiviert worden sein. Weitere Informationen zu Clustersicherheitsszenarien finden Sie unter [Schützen von Service Fabric-Clustern](service-fabric-cluster-security.md).

Um die Kommunikation zwischen einem Client und einem Clusterknoten mit Zertifikatssicherheit zu schützen, müssen Sie zuerst das Clientzertifikat beziehen und im persönlichen Speicher (My) auf dem lokalen Computer oder im Personal-Speicher des aktuellen Benutzers installieren.

Führen Sie das folgende PowerShell-Cmdlet aus, um das Zertifikat auf dem Computer einrichten, von dem aus Sie auf den Cluster zugreifen werden.

```powershell
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\docDemo\certs\DocDemoClusterCert.pfx `
        -Password (ConvertTo-SecureString -String test -AsPlainText -Force)
```

Falls es sich um ein selbstsigniertes Zertifikat handelt, müssen Sie es in den Speicher „Vertrauenswürdige Personen“ Ihres Computers importieren, bevor Sie es zum Herstellen einer Verbindung mit einem sicheren Cluster verwenden können.

```powershell
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\TrustedPeople `
-FilePath C:\docDemo\certs\DocDemoClusterCert.pfx `
-Password (ConvertTo-SecureString -String test -AsPlainText -Force)
```

## Herstellen einer Verbindung mit einem sicheren Cluster mit PowerShell

Führen Sie den folgenden PowerShell-Befehl aus, um eine Verbindung mit einem sicheren Cluster herzustellen. Die Details des Zertifikats müssen mit einem Zertifikat auf den Clusterknoten übereinstimmen.

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint <Certificate Thumbprint> `
          -FindType FindByThumbprint -FindValue <Certificate Thumbprint> `
          -StoreLocation CurrentUser -StoreName My
```

Beispielsweise sollte der obige PowerShell-Befehl ähnlich wie der Folgende aussehen:

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint clustername.westus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C179E609BBF0B227844342535142306F3913D6ED `
          -FindType FindByThumbprint -FindValue C179E609BBF0B227844342535142306F3913D6ED `
          -StoreLocation CurrentUser -StoreName My
```

## Herstellen einer Verbindung mit einem sicheren Cluster mit den FabricClient-APIs
Der folgende [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx). Die Knoten im Cluster müssen über gültige Zertifikate verfügen, deren gemeinsamer Name oder DNS-Name im SAN in der [RemoteCommonNames-Eigenschaft](https://msdn.microsoft.com/library/azure/system.fabric.x509credentials.remotecommonnames.aspx) angezeigt wird, die auf [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) festgelegt ist. Dies ermöglicht die gegenseitige Authentifizierung zwischen dem Client und dem Clusterknoten.

```csharp
string thumb = "C179E609BBF0B227844342535142306F3913D6ED";
string CommonName = "www.clustername.westus.azure.com";
string connection = "clustername.westus.cloudapp.azure.com:19000";

X509Credentials xc = GetCredentials(thumb, CommonName);
FabricClient fc = new FabricClient(xc, connection);
Task<bool> t = fc.PropertyManager.NameExistsAsync(new Uri("fabric:/any"));
try
{
    bool result = t.Result;
    Console.WriteLine("Cluster is connected");
}
catch (AggregateException ae)
{
    Console.WriteLine("Connect failed: {0}", ae.InnerException.Message);
}
catch (Exception e)
{
    Console.WriteLine("Connect failed: {0}", e.Message);
}

...

static X509Credentials GetCredentials(string thumb, string name)
{
    X509Credentials xc = new X509Credentials();
    xc.StoreLocation = StoreLocation.CurrentUser;
    xc.StoreName = "MY";
    xc.FindType = X509FindType.FindByThumbprint;
    xc.FindValue = thumb;
    xc.RemoteCertThumbprints.Add(thumb);
    xc.RemoteCommonNames.Add(name);
    xc.ProtectionLevel = ProtectionLevel.EncryptAndSign;
    return xc;
}
```


## Nächste Schritte

- [Service Fabric-Cluster-Upgradeprozess und Erwartungen](service-fabric-cluster-upgrade.md)
- [Verwalten von Service Fabric-Anwendungen in Visual Studio](service-fabric-manage-application-in-visual-studio.md)
- [Einführung in das Service Fabric-Integritätsmodell](service-fabric-health-introduction.md)
- [Anwendungssicherheit und RunAs](service-fabric-application-runas-security.md)

<!---HONumber=AcomDC_0608_2016-->