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
   ms.date="08/25/2016"
   ms.author="ryanwi"/>


# <a name="connect-to-a-secure-cluster-without-aad"></a>Herstellen einer Verbindung mit einem sicheren Cluster ohne AAD
Wenn ein Client eine Verbindung mit einem Service Fabric-Clusterknoten herstellt, kann mit der Zertifikatssicherheit der Client authentifiziert und sichere Kommunikation eingerichtet werden. Mit dieser Authentifizierung wird sichergestellt, dass nur autorisierte Benutzer auf den Cluster zugreifen, Anwendungen bereitstellen und Verwaltungsaufgaben ausführen können.  Zertifikatssicherheit muss zuvor beim Erstellen des Clusters auf dem Cluster aktiviert worden sein.  Zum Schutz des Clusters müssen mindestens zwei Zertifikate verwendet werden: eines für den Cluster- und Serverzugriff und ein zweites für den Clientzugriff.  Wir empfehlen zudem, dass Sie zusätzliche sekundäre Zertifikate und Clientzugriffszertifikate nutzen.  Weitere Informationen zu Clustersicherheitsszenarien finden Sie unter [Schützen von Service Fabric-Clustern](service-fabric-cluster-security.md).

Um die Kommunikation zwischen einem Client und einem Clusterknoten mit Zertifikatssicherheit zu schützen, müssen Sie zuerst das Clientzertifikat beziehen und installieren. Das Zertifikat kann im persönlichen Speicher (My) auf dem lokalen Computer oder im persönlichen Speicher des aktuellen Benutzers installiert werden.  Sie benötigen außerdem den Fingerabdruck des Serverzertifikats, damit der Cluster vom Client authentifiziert werden kann.


Führen Sie das folgende PowerShell-Cmdlet aus, um das Clientzertifikat auf dem Computer einzurichten, auf dem Sie auf den Cluster zugreifen.

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
<a id="connectsecureclustercli"></a> 
## <a name="connect-to-a-secure-cluster-using-azure-cli-without-aad"></a>Herstellen einer Verbindung mit einem sicheren Cluster mit der Azure-CLI ohne AAD

Mit den folgenden Befehlen der Azure-Befehlszeilenschnittstelle können Sie eine Verbindung mit einem sicheren Cluster herstellen. Die Details des Zertifikats müssen mit einem Zertifikat auf den Clusterknoten übereinstimmen. 
 
Falls Ihr Zertifikat über Zertifizierungsstellen (Certificate Authorities, CAs) verfügt, müssen Sie den Parameter `--ca-cert-path` hinzufügen, wie im folgenden Beispiel zu sehen: 

```
 azure servicefabric cluster connect --connection-endpoint https://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --ca-cert-path /tmp/ca1,/tmp/ca2 
```
Mehrere Zertifizierungsstellen können als kommagetrennte Liste angegeben werden. 

 
Falls der allgemeine Name im Zertifikat nicht dem Verbindungsendpunkt entspricht, können Sie die Überprüfung mithilfe des Parameters `--strict-ssl-false` umgehen. 

```
azure servicefabric cluster connect --connection-endpoint https://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --ca-cert-path /tmp/ca1,/tmp/ca2 --strict-ssl-false 
```
 
Wenn Sie die Überprüfung der Zertifizierungsstelle überspringen möchten, können Sie den Parameter ``--reject-unauthorized-false`` hinzufügen, wie im folgenden Befehl zu sehen:

```
azure servicefabric cluster connect --connection-endpoint https://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --reject-unauthorized-false 
```
 
Verwenden Sie zum Herstellen einer Verbindung mit einem Cluster, der durch ein selbstsigniertes Zertifikat geschützt ist, den folgenden Befehl. Dieser entfernt die Überprüfung der Zertifizierungsstelle und die Überprüfung des allgemeinen Namens:

```
azure servicefabric cluster connect --connection-endpoint https://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --strict-ssl-false --reject-unauthorized-false
```

Nach dem Herstellen der Verbindung können Sie durch Ausführen anderer Befehle über die Befehlszeilenschnittstelle mit dem Cluster interagieren. 

<a id="connectsecurecluster"></a>
## <a name="connect-to-a-secure-cluster-using-powershell-without-aad"></a>Herstellen einer Verbindung mit einem sicheren Cluster mit PowerShell ohne AAD

Führen Sie den folgenden PowerShell-Befehl aus, um eine Verbindung mit einem sicheren Cluster herzustellen. Die Details des Zertifikats müssen mit einem Zertifikat auf den Clusterknoten übereinstimmen.

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint <Certificate Thumbprint> `
          -FindType FindByThumbprint -FindValue <Certificate Thumbprint> `
          -StoreLocation CurrentUser -StoreName My
```

*ServerCertThumbprint* ist der Fingerabdruck des Serverzertifikats, das auf den Clusterknoten installiert ist. *FindValue* ist der Fingerabdruck des Clientzertifikats des Administrators.
Beim Auffüllen der Parameter sieht der Befehl wie im folgenden Beispiel aus: 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint clustername.westus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint A8136758F4AB8962AF2BF3F27921BE1DF67F4326 `
          -FindType FindByThumbprint -FindValue 71DE04467C9ED0544D021098BCD44C71E183414E `
          -StoreLocation CurrentUser -StoreName My
```




## <a name="connect-to-a-secure-cluster-using-the-fabricclient-apis"></a>Herstellen einer Verbindung mit einem sicheren Cluster mit den FabricClient-APIs

Weitere Informationen zu FabricClient-APIs finden Sie unter [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx). Die Knoten im Cluster müssen über gültige Zertifikate verfügen, deren allgemeiner Name oder DNS-Name im SAN in der [RemoteCommonNames](https://msdn.microsoft.com/library/azure/system.fabric.x509credentials.remotecommonnames.aspx)-Eigenschaft angezeigt wird, die auf [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) festgelegt ist. Durch dieses Verfahren wird die gegenseitige Authentifizierung zwischen dem Client und dem Clusterknoten ermöglicht.

```csharp
string clientCertThumb = "71DE04467C9ED0544D021098BCD44C71E183414E";
string serverCertThumb = "A8136758F4AB8962AF2BF3F27921BE1DF67F4326";
string CommonName = "www.clustername.westus.azure.com";
string connection = "clustername.westus.cloudapp.azure.com:19000";

X509Credentials xc = GetCredentials(clientCertThumb, serverCertThumb, CommonName);
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

static X509Credentials GetCredentials(string clientCertThumb, string serverCertThumb, string name)
{
    X509Credentials xc = new X509Credentials();

    // Client certificate
    xc.StoreLocation = StoreLocation.CurrentUser;
    xc.StoreName = "MY";
    xc.FindType = X509FindType.FindByThumbprint;
    xc.FindValue = thumb;

    // Server certificate
    xc.RemoteCertThumbprints.Add(thumb);
    xc.RemoteCommonNames.Add(name);

    xc.ProtectionLevel = ProtectionLevel.EncryptAndSign;
    return xc;
}
```


## <a name="next-steps"></a>Nächste Schritte

- [Service Fabric-Cluster-Upgradeprozess und Erwartungen](service-fabric-cluster-upgrade.md)
- [Verwalten von Service Fabric-Anwendungen in Visual Studio](service-fabric-manage-application-in-visual-studio.md)
- [Einführung in das Service Fabric-Integritätsmodell](service-fabric-health-introduction.md)
- [Anwendungssicherheit und RunAs](service-fabric-application-runas-security.md)



<!--HONumber=Oct16_HO2-->


