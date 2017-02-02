---
title: Authentifizieren des Clientzugriffs auf ein Cluster | Microsoft Docs
description: Beschreibt das Authentifizieren des Clientzugriffs auf einen Service Fabric-Cluster und das Sichern der Kommunikation zwischen Clients und einem Cluster
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 759a539e-e5e6-4055-bff5-d38804656e10
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/11/2016
ms.author: ryanwi
translationtype: Human Translation
ms.sourcegitcommit: 65775053918e12ef8881f417dacc0a63f080d093
ms.openlocfilehash: 6de98012e768abc7f8450e97648444a74474b5e9


---
# <a name="connect-to-a-secure-cluster"></a>Herstellen einer Verbindung mit einem sicheren Cluster
Wenn ein Client eine Verbindung mit einem Service Fabric-Clusterknoten herstellt, kann der Client mit der Zertifikatssicherheit oder mit Azure Active Directory (AAD) authentifiziert und eine sichere Kommunikation eingerichtet werden. Mit dieser Authentifizierung wird sichergestellt, dass nur autorisierte Benutzer auf den Cluster zugreifen, Anwendungen bereitstellen und Verwaltungsaufgaben ausführen können.  Zertifikat- oder AAD-Sicherheit muss zuvor beim Erstellen des Clusters auf dem Cluster aktiviert worden sein.  Weitere Informationen zu Clustersicherheitsszenarien finden Sie unter [Schützen von Service Fabric-Clustern](service-fabric-cluster-security.md). Wenn Sie eine Verbindung mit einem mit Zertifikaten gesicherten Cluster herstellen, [richten Sie das Clientzertifikat](service-fabric-connect-to-secure-cluster.md#connectsecureclustersetupclientcert) auf dem Computer ein, der mit dem Cluster verbunden wird. 

<a id="connectsecureclustercli"></a> 

## <a name="connect-to-a-secure-cluster-using-azure-cli"></a>Herstellen einer Verbindung mit einem sicheren Cluster mithilfe der Azure-Befehlszeilenschnittstelle
Mit den folgenden Befehlen der Azure-Befehlszeilenschnittstelle können Sie eine Verbindung mit einem sicheren Cluster herstellen. 

### <a name="connect-to-a-secure-cluster-using-a-client-certificate"></a>Herstellen einer Verbindung mit einem sicheren Cluster mithilfe eines Clientzertifikats
Die Details des Zertifikats müssen mit einem Zertifikat auf den Clusterknoten übereinstimmen. 

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

Nach dem Herstellen der Verbindung können Sie durch [Ausführen anderer Befehle über die Befehlszeilenschnittstelle](service-fabric-azure-cli.md) mit dem Cluster interagieren. 

<a id="connectsecurecluster"></a>

## <a name="connect-to-a-secure-cluster-using-powershell"></a>Herstellen einer Verbindung mit einem sicheren Cluster mit PowerShell
Stellen Sie zunächst eine Verbindung mit dem Cluster her, bevor Sie mithilfe von PowerShell Vorgänge auf einem Cluster durchführen. Die Clusterverbindung wird für alle nachfolgenden Befehle in der angegebenen PowerShell-Sitzung verwendet.

### <a name="connect-to-an-unsecure-cluster"></a>Herstellen einer Verbindung mit einem unsicheren Cluster

Geben Sie im Befehl **Connect-ServiceFabricCluster** die Adresse des Clusterendpunkts an, um eine Verbindung mit einem unsicheren Cluster herzustellen:

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 
```

### <a name="connect-to-a-secure-cluster-using-azure-active-directory"></a>Herstellen einer Verbindung mit einem sicheren Cluster mit Azure Active Directory

Stellen Sie zum Herstellen einer Verbindung mit einem sicheren Cluster, das Azure Active Directory für die Autorisierung des Clusteradministratorzugriffs verwendet, den Zertifikatfingerabdruck für das Cluster bereit, und verwenden Sie das *AzureActiveDirectory*-Flag.  

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
-ServerCertThumbprint <Server Certificate Thumbprint> `
-AzureActiveDirectory
```

### <a name="connect-to-a-secure-cluster-using-a-client-certificate"></a>Herstellen einer Verbindung mit einem sicheren Cluster mithilfe eines Clientzertifikats
Führen Sie den folgenden PowerShell-Befehl zur Verbindungsherstellung mit einem sicheren Cluster aus, das Clientzertifikate verwendet, um den Administratorzugriff zu autorisieren. Stellen Sie den Zertifikatfingerabdruck für das Cluster und den Fingerabdruck des Clientzertifikats bereit, das Berechtigungen für die Clusterverwaltung erhalten hat. Die Details des Zertifikats müssen mit einem Zertifikat auf den Clusterknoten übereinstimmen.

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


<a id="connectsecureclusterfabricclient"></a>

## <a name="connect-to-a-secure-cluster-using-the-fabricclient-apis"></a>Herstellen einer Verbindung mit einem sicheren Cluster mit den FabricClient-APIs
Das Service Fabric SDK enthält die [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx)-Klasse für die Clusterverwaltung. 

### <a name="connect-to-an-unsecure-cluster"></a>Herstellen einer Verbindung mit einem unsicheren Cluster

Erstellen Sie zur Verbindungsherstellung mit einem ungesicherten Remotecluster eine FabricClient-Instanz, und geben die Clusteradresse an:

```csharp
FabricClient fabricClient = new FabricClient("clustername.westus.cloudapp.azure.com:19000");
```

Für Code, der innerhalb eines Clusters ausgeführt wird, z.B. in einem Reliable Services-Dienst, erstellen Sie einen FabricClient, *ohne* die Adresse des Clusters anzugeben. FabricClient stellt eine Verbindung mit der lokalen Gatewayverwaltung auf dem Knoten her, auf dem der Code derzeit ausgeführt wird und vermeidet einen zusätzlichen Netzwerkhop.

```csharp
FabricClient fabricClient = new FabricClient();
```

### <a name="connect-to-a-secure-cluster-using-a-client-certificate"></a>Herstellen einer Verbindung mit einem sicheren Cluster mithilfe eines Clientzertifikats

Die Knoten im Cluster müssen über gültige Zertifikate verfügen, deren allgemeiner Name oder DNS-Name im SAN in der [RemoteCommonNames](https://msdn.microsoft.com/library/azure/system.fabric.x509credentials.remotecommonnames.aspx)-Eigenschaft angezeigt wird, die auf [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) festgelegt ist. Durch dieses Verfahren wird die gegenseitige Authentifizierung zwischen dem Client und dem Clusterknoten ermöglicht.

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

### <a name="connect-to-a-secure-cluster-using-azure-active-directory"></a>Herstellen einer Verbindung mit einem sicheren Cluster mit Azure Active Directory

Durch dieses Verfahren wird Azure Active Directory für die Clientidentität und das Serverzertifikat für die Serveridentität aktiviert.

So verwenden Sie den interaktiven Modus, der das AAD-Dialogfeld für die interaktive Anmeldung anzeigt

```csharp
string serverCertThumb = "A8136758F4AB8962AF2BF3F27921BE1DF67F4326";
string connection = "clustername.westus.cloudapp.azure.com:19000";

ClaimsCredentials claimsCredentials = new ClaimsCredentials();
claimsCredentials.ServerThumbprints.Add(serverCertThumb);

FabricClient fc = new FabricClient(
    claimsCredentials,
    connection);

try
{
    var ret = fc.ClusterManager.GetClusterManifestAsync().Result;
    Console.WriteLine(ret.ToString());
}
catch (AggregateException ae)
{
    Console.WriteLine("Connect failed: {0}", ae.InnerException.Message);
}
catch (Exception e)
{
    Console.WriteLine("Connect failed: {0}", e.Message);
}
```

So verwenden Sie den unbeaufsichtigten Modus ohne menschliche Interaktion:

(Dieses Beispiel basiert auf Microsoft.IdentityModel.Clients.ActiveDirectory, Version: 2.19.208020213

Informationen über das Abrufen eines Token und weitere Informationen finden Sie unter [Microsoft.IdentityModel.Clients.ActiveDirectory Namespace](https://msdn.microsoft.com/library/microsoft.identitymodel.clients.activedirectory.aspx).)

```csharp
string tenantId = "c15cfcea-02c1-40dc-8466-fbd0ee0b05d2";
string clientApplicationId = "118473c2-7619-46e3-a8e4-6da8d5f56e12";
string webApplicationId = "53E6948C-0897-4DA6-B26A-EE2A38A690B4";

string token = GetAccessToken(
    tenantId,
    webApplicationId,
    clientApplicationId,
    "urn:ietf:wg:oauth:2.0:oob"
    );

string serverCertThumb = "A8136758F4AB8962AF2BF3F27921BE1DF67F4326";
string connection = "clustername.westus.cloudapp.azure.com:19000";
ClaimsCredentials claimsCredentials = new ClaimsCredentials();
claimsCredentials.ServerThumbprints.Add(serverCertThumb);
claimsCredentials.LocalClaims = token;

FabricClient fc = new FabricClient(
   claimsCredentials,
   connection);

try
{
    var ret = fc.ClusterManager.GetClusterManifestAsync().Result;
    Console.WriteLine(ret.ToString());
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

static string GetAccessToken(
    string tenantId,
    string resource,
    string clientId,
    string redirectUri)
{
    string authorityFormat = @"https://login.microsoftonline.com/{0}";
    string authority = string.Format(CultureInfo.InvariantCulture, authorityFormat, tenantId);
    AuthenticationContext authContext = new AuthenticationContext(authority);

    string token = "";
    try
    {
        var authResult = authContext.AcquireToken(
            resource,
            clientId,
            new UserCredential("TestAdmin@clustenametenant.onmicrosoft.com", "TestPassword"));
        token = authResult.AccessToken;
    }
    catch (AdalException ex)
    {
        Console.WriteLine("Get AccessToken failed: {0}", ex.Message);
    }

    return token;
}

```

<a id="connectsecureclustersfx"></a>

## <a name="connect-to-a-secure-cluster-using-service-fabric-explorer"></a>Herstellen einer Verbindung mit einem sicheren Cluster mit Service Fabric Explorer
Um [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) für einen bestimmten Cluster zu erreichen, navigieren Sie mit Ihrem Browser zu:

`http://<your-cluster-endpoint>:19080/Explorer`

Die vollständige URL steht auch im Cluster Essentials-Bereich des Azure-Portal zur Verfügung.

### <a name="connect-to-a-secure-cluster-using-azure-active-directory"></a>Herstellen einer Verbindung mit einem sicheren Cluster mit Azure Active Directory

Um eine Verbindung zu einem Cluster herzustellen, das mit AAD gesichert ist, verweisen Sie mit Ihrem Browser auf:

`https://<your-cluster-endpoint>:19080/Explorer`

Sie werden automatisch aufgefordert, sich bei AAD anzumelden.

### <a name="connect-to-a-secure-cluster-using-a-client-certificate"></a>Herstellen einer Verbindung mit einem sicheren Cluster mithilfe eines Clientzertifikats

Um eine Verbindung zu einem Cluster herzustellen, das mit Zertifikaten gesichert ist, verweisen Sie mit Ihrem Browser auf:

`https://<your-cluster-endpoint>:19080/Explorer`

Sie werden automatisch aufgefordert, ein Clientzertifikat auszuwählen.

<a id="connectsecureclustersetupclientcert"></a>
## <a name="set-up-a-client-certificate-on-the-remote-computer"></a>Einrichten eines Clientzertifikats auf dem Remotecomputer
Zum Schutz des Clusters müssen mindestens zwei Zertifikate verwendet werden: eines für den Cluster- und Serverzugriff und ein zweites für den Clientzugriff.  Wir empfehlen zudem, dass Sie zusätzliche sekundäre Zertifikate und Clientzugriffszertifikate nutzen.  Um die Kommunikation zwischen einem Client und einem Clusterknoten mit Zertifikatssicherheit zu schützen, müssen Sie zuerst das Clientzertifikat beziehen und installieren. Das Zertifikat kann im persönlichen Speicher (My) auf dem lokalen Computer oder im persönlichen Speicher des aktuellen Benutzers installiert werden.  Sie benötigen außerdem den Fingerabdruck des Serverzertifikats, damit der Cluster vom Client authentifiziert werden kann.

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

## <a name="next-steps"></a>Nächste Schritte
* [Service Fabric-Cluster-Upgradeprozess und Erwartungen](service-fabric-cluster-upgrade.md)
* [Verwalten von Service Fabric-Anwendungen in Visual Studio](service-fabric-manage-application-in-visual-studio.md)
* [Einführung in das Service Fabric-Integritätsmodell](service-fabric-health-introduction.md)
* [Anwendungssicherheit und RunAs](service-fabric-application-runas-security.md)




<!--HONumber=Dec16_HO2-->


