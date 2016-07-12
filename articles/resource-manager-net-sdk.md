<properties
   pageTitle="Resource Manager SDK für .NET| Microsoft Azure"
   description="Enthält eine Übersicht über die Resource Manager .NET SDK-Authentifizierung und Beispiele zur Verwendung."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="navalev"
   manager=""
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/21/2016"
   ms.author="navale;tomfitz;"/>

# Azure Resource Manager SDK für .NET  
Azure Resource Manager SDKs sind für mehrere Sprachen und Plattformen verfügbar. Jede dieser Sprachimplementierungen ist jeweils über die entsprechenden Ökosystem-Paket-Manager und GitHub verfügbar.

Der Code in diesen SDKs wird jeweils über [Azure RESTful-API-Spezifikationen](https://github.com/azure/azure-rest-api-specs) generiert. Dies sind Open-Source-Spezifikationen, die auf der Swagger 2.0-Spezifikation basieren. Der SDK-Code wird über ein Open-Source-Projekt mit dem Namen [AutoRest](https://github.com/azure/autorest) generiert. AutoRest transformiert diese RESTful-API-Spezifikationen in Clientbibliotheken in mehreren Sprachen. Falls Sie Aspekte des generierten Codes in den SDKs verbessern möchten, können Sie den gesamten Satz der Tools zum Erstellen der SDKs verwenden. Sie sind frei zugänglich und verfügbar und basieren auf dem gängigen API-Spezifikationsformat.

Das [Azure SDK für .NET](https://azure.microsoft.com/downloads/) ist eine Gruppe von NuGet-Paketen, mit denen Sie die meisten Azure Resource Manager-APIs aufrufen können. Wenn das SDK die erforderliche Funktionalität nicht bereitstellt, können Sie das SDK leicht mit normalen Aufrufen der Resource Manager-REST-API im Hintergrund kombinieren.

In diesem Artikel sollen nicht alle Aspekte des Azure SDK für .NET, von Azure Resource Manager-APIs oder von Visual Studio beschrieben werden. Er soll Ihnen lediglich als schnelle Einstiegshilfe dienen.

Alle folgenden Codeausschnitte stammen aus dem [Beispielprojekt, das Sie herunterladen können](https://github.com/dx-ted-emea/Azure-Resource-Manager-Documentation/tree/master/ARM/SDKs/Samples/Net).

## Installieren von NuGet-Paketen

Für die Beispiele in diesem Artikel sind zwei NuGet-Pakete erforderlich (zusätzlich zum Azure SDK für .NET). Gehen Sie bei der Installation wie folgt vor:

1. Klicken Sie in Visual Studio mit der rechten Maustaste auf Ihr Projekt, und wählen Sie **NuGet-Pakete verwalten** aus.
2. Suchen Sie nach **Microsoft.IdentityModel.Clients.ActiveDirectory**, und installieren Sie die neueste stabile Version des Pakets.
3. Suchen Sie nach **Microsoft.Azure.Management.ResourceManager**, und wählen Sie **Vorabversion einschließen**. Installieren Sie die aktuelle Vorschauversion (z.B. 1.1.2-preview).

## Authentifizieren
Azure Active Directory (Azure AD) führt die Authentifizierung für Resource Manager durch. Zum Herstellen einer Verbindung mit einer API müssen Sie sich zuerst gegenüber Azure AD authentifizieren, um ein Zugriffstoken zu erhalten, das Sie für jede Anforderung übergeben können. Zum Beschaffen dieses Tokens müssen Sie eine Azure AD-Anwendung und einen Dienstprinzipal für die Anmeldung erstellen. Schritt-für-Schritt-Anleitungen finden Sie in diesen Artikeln:

- [Verwenden von Azure PowerShell zum Erstellen einer Active Directory-Anwendung zum Zugreifen auf Ressourcen](resource-group-authenticate-service-principal.md)
- [Verwenden der Azure-Befehlszeilenschnittstelle zum Erstellen einer Active Directory-Anwendung für den Zugriff auf Ressourcen](resource-group-authenticate-service-principal-cli.md)
- [Verwenden des Portals zum Erstellen einer Active Directory-Anwendung, die auf Ressourcen zugreifen kann](resource-group-create-service-principal-portal.md)

Nach dem Erstellen des Dienstprinzipals sollten Sie über Folgendes verfügen:

- Client-ID oder Anwendungs-ID (GUID)
- Geheimer Clientschlüssel oder Kennwort (Zeichenfolge)
- Mandanten-ID (GUID) oder Domänenname (Zeichenfolge)

### Empfangen des Zugriffstokens aus dem Code
Sie können das Zugriffstoken mit den folgenden Codezeilen abrufen, indem Sie nur Ihre Azure AD-Mandanten-ID, Ihre Azure AD-Anwendungsclient-ID und den geheimen Clientschlüssel der Azure AD-Anwendung übergeben. Speichern Sie das Token für mehrere Anforderungen, da es standardmäßig eine Stunde lang gültig ist.

```csharp
private static async Task<AuthenticationResult> GetAccessTokenAsync(string tenantId, string clientId, string clientSecret)
{
    Console.WriteLine("Acquiring Access Token from Azure AD");
    AuthenticationContext authContext = new AuthenticationContext
        ("https://login.windows.net/" /* Azure AD URI */
            + $"{tenantId}" /* Tenant ID */);

    var credential = new ClientCredential(clientId, clientSecret);

    var token = await authContext.AcquireTokenAsync("https://management.azure.com/", credential);

    Console.WriteLine($"Token: {token.AccessToken}");
    return token;
}
```

Anstelle der Mandanten-ID für die Anmeldung können Sie auch die Azure AD-Domäne verwenden. Dies wird im folgenden Code veranschaulicht. Bei diesem Ansatz müssen Sie die Methodensignatur so ändern, dass sie anstelle der Mandanten-ID den Domänennamen enthält.

```csharp
AuthenticationContext authContext = new AuthenticationContext
    ("https://login.windows.net/" /* Azure AD URI */
    + $"{domain}.onmicrosoft.com");
```

Sie können das Zugriffstoken für eine Azure AD-Anwendung abrufen, für die zur Authentifizierung ein Zertifikat genutzt wird, indem Sie den folgenden Code verwenden:

```csharp
private static async Task<AuthenticationResult> GetAccessTokenFromCertAsync(string tenantId, string clientId, string certName)
{
    Console.WriteLine("Acquiring Access Token from Azure AD");
    AuthenticationContext authContext = new AuthenticationContext
        ("https://login.windows.net/" /* Azure AD URI */
        + $"{tenantId}" /* Tenant ID or Azure AD domain */);

    X509Certificate2 cert = null;
    X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);

    try
    {
        store.Open(OpenFlags.ReadOnly);
        var certCollection = store.Certificates;
        var certs = certCollection.Find(X509FindType.FindBySubjectName, certName, false);
        cert = certs[0];
    }
    finally
    {
        store.Close();
    }

    var certCredential = new ClientAssertionCertificate(clientId, cert);

    var token = await authContext.AcquireTokenAsync("https://management.azure.com/", certCredential);

    Console.WriteLine($"Token: {token.AccessToken}");
    return token;
}
```

### Abfragen von Azure-Abonnements, die an die authentifizierte Anwendung angefügt sind
Es kann vorkommen, dass Sie die Azure-Abonnements abfragen möchten, die der gerade authentifizierten Anwendung zugeordnet sind. Ab jetzt ist es obligatorisch, die Abonnement-ID für das gewünschte Abonnement für jeden durchgeführten API-Aufruf zu übergeben.

Im folgenden Beispielcode werden Azure-APIs direkt mit der REST-API abgefragt. Es werden also keine Features im Azure SDK für .NET verwendet.

```csharp
async private static Task<List<string>> GetSubscriptionsAsync(string token)
{
    Console.WriteLine("Querying for subscriptions");
    const string apiVersion = "2015-01-01";

    var client = new HttpClient();
    client.BaseAddress = new Uri("https://management.azure.com/");
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
    client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

    var response = await client.GetAsync($"subscriptions?api-version={apiVersion}");

    var jsonResponse = response.Content.AsString();

    var subscriptionIds = new List<string>();
    dynamic json = JsonConvert.DeserializeObject(jsonResponse);

    for (int i = 0; i < json.value.Count; i++)
    {
        subscriptionIds.Add(json.value[i].subscriptionId.Value);
    }

    Console.WriteLine($"Found {subscriptionIds.Count} subscription(s)");
    return subscriptionIds;
}
```

Beachten Sie, dass Sie von Azure eine JSON-Antwort erhalten. Anschließend extrahieren Sie die Abonnement-IDs aus dieser Antwort, um eine Liste mit IDs zurückzugeben. Für alle nachfolgenden Aufrufe von Azure Resource Manager-APIs in diesem Artikel wird nur eine einzelne Azure-Abonnement-ID verwendet. Wenn Ihre Anwendung also mehreren Abonnements zugeordnet ist, müssen Sie nur das richtige Abonnement auswählen und im weiteren Verlauf als Parameter übergeben.

Ab jetzt wird für jeden Aufruf, den Sie für die Azure-APIs durchführen, das Azure SDK für .NET genutzt. Der Code sieht daher etwas anders aus.

### Umschließen des Tokens als TokenCredentials-Objekt
Für alle nachfolgenden API-Aufrufe ist das Token erforderlich, das Sie von Azure AD im Format eines TokenCredentials-Objekts erhalten haben. Sie können ein Objekt dieser Art leicht erstellen, indem Sie einfach das unformatierte Token als Parameter an den Konstruktor der Klasse übergeben.

```csharp
var credentials = new TokenCredentials(token);
```

Wenn Sie eine frühere Version des Resource Manager-NuGet-Pakets (Microsoft.Azure.Management.Resources) besitzen, müssen Sie den folgenden Code verwenden:

```csharp
var credentials = new TokenCloudCredentials(subscriptionId, token.AccessToken);
```

## Erstellen einer Ressourcengruppe
In Azure dreht sich alles um die Ressourcengruppen. Deshalb erstellen wir zunächst eine Ressourcengruppe. Mit *ResourceManagementClient* werden allgemeine Ressourcen und Ressourcengruppen behandelt. Für alle folgenden spezielleren Verwaltungsclients, die Sie verwenden, müssen Sie Ihre Anmeldeinformationen und eine Abonnement-ID angeben, um das gewünschte Abonnement zu identifizieren.

```csharp
private static async Task<ResourceGroup> CreateResourceGroupAsync(TokenCredentials credentials, string subscriptionId, string resourceGroup, string location)
{
    Console.WriteLine($"Creating Resource Group {resourceGroup}");
    var resourceClient = new ResourceManagementClient(credentials) { SubscriptionId = subscriptionId };
    return await resourceClient.ResourceGroups.CreateOrUpdateAsync(resourceGroup,
        new ResourceGroup
        {
            Location = location
        });
}
```

## Manuelles Erstellen von Ressourcen oder Erstellen von Ressourcen mit Vorlagen
Es gibt verschiedene Möglichkeiten, mit den Azure Resource Manager-APIs zu interagieren, aber die beiden Hauptverfahren sind:

* Manuelles Interagieren durch das Aufrufen bestimmter Ressourcenanbieter
* Verwenden einer Azure Resource Manager-Vorlage

Die Verwendung einer Resource Manager-Vorlage hat die folgenden Vorteile:

* Sie geben deklarativ an, wie das Ergebnis aussehen soll, und nicht, wie es erreicht werden soll.
* Sie müssen die parallele Ausführung Ihrer Bereitstellungen nicht manuell durchführen. Diese Aufgabe übernimmt Resource Manager für Sie.
* Sie müssen nicht C# oder eine andere Sprache erlernen, um eine Resource Manager-Vorlage bereitzustellen (obwohl Sie jede Sprache verwenden können, um eine Bereitstellung mit Vorlage zu starten).
* Die domänenspezifische Sprache (DSL), die in den Vorlagen genutzt wird, wird über JSON erstellt. Für Benutzer, die bereits mit JSON gearbeitet haben, sollte alles leicht verständlich sein.

Trotz dieser vielen Vorteile der Vorlagen zeigen wir Ihnen zuerst, wie Sie die APIs manuell aufrufen.

### Erstellen eines virtuellen Computers – Stück für Stück
Sie verfügen über Ihr Abonnement und die Ressourcengruppe. Wenn Sie einen virtuellen Computer (VM) bereitstellen möchten, müssen Sie wissen, woraus er besteht:

* Mindestens ein Speicherkonto zum Speichern persistenter Datenträger
* Mindestens eine öffentliche IP-Adresse, damit Ihre Ressourcen in Azure über das Internet erreichbar sind (mit einem DNS-Namen)
* Mindestens ein virtuelles Netzwerk für die interne Kommunikation zwischen Ressourcen
* Mindestens eine Netzwerkschnittstellenkarte (NIC) für die Kommunikation der VMs
* Mindestens ein virtueller Computer zum Ausführen von Software

Einige dieser Ressourcen können parallel erstellt werden, andere dagegen nicht. Zum Beispiel:

* NICs sind von öffentlichen IP-Adressen und virtuellen Netzwerken abhängig.
* VMs sind von NICs und Speicherkonten abhängig.

Versuchen Sie nicht, Ressourcen zu instanziieren, bevor die erforderlichen Abhängigkeiten erstellt wurden. Das vollständige [Beispiel](https://github.com/dx-ted-emea/Azure-Resource-Manager-Documentation/tree/master/ARM/SDKs/Samples/Net) verdeutlicht, wie Sie Ihre Ressourcen auf effiziente Weise parallel erstellen und trotzdem nachverfolgen können, was genau erstellt wurde.

#### Erstellen des Speicherkontos
Sie benötigen ein Speicherkonto zum Speichern der virtuellen Festplatten für den virtuellen Computer. Wenn Sie über ein vorhandenes Speicherkonto verfügen, können Sie es für mehrere virtuelle Computer verwenden. Beachten Sie aber, dass Sie die Last auf mehrere Speicherkonten verteilen sollten, damit es nicht zu Engpässen kommt. Bedenken Sie auch, dass der Typ des Speicherkontos und dessen Standort eine Einschränkung der auswählbaren VM-Größe ergeben. Der Grund ist, dass nicht alle VM-Größen in allen Regionen bzw. für alle Speicherkontotypen verfügbar sind.

```csharp
private static async Task<StorageAccount> CreateStorageAccountAsync(TokenCredentials credentials, string subscriptionId, string resourceGroup, string location, string storageAccountName, AccountType accountType = AccountType.StandardLRS)
{
    Console.WriteLine("Creating Storage Account");
    var storageClient = new StorageManagementClient(credentials) { SubscriptionId = subscriptionId };
    return await storageClient.StorageAccounts.CreateAsync(resourceGroup, storageAccountName,
        new StorageAccountCreateParameters
        {
            Location = location,
            AccountType = accountType,
        });
}
```

#### Erstellen der öffentlichen IP-Adresse
Über die öffentliche IP-Adresse sind Ihre Ressourcen in Azure über das Internet zugänglich. Zusammen mit der IP-Adresse wird Ihnen ein vollqualifizierter Domänenname (FQDN) zugewiesen, der Ihnen den Zugriff erleichtert.

```csharp
private static Task<PublicIPAddress> CreatePublicIPAddressAsync(TokenCredentials credentials, string subscriptionId, string resourceGroup, string location, string pipAddressName, string pipDnsName)
{
    Console.WriteLine("Creating Public IP");
    var networkClient = new NetworkManagementClient(credentials) { SubscriptionId = subscriptionId };
    var createPipTask = networkClient.PublicIPAddresses.CreateOrUpdateAsync(resourceGroup, pipAddressName,
        new PublicIPAddress
        {
            Location = location,
            DnsSettings = new PublicIPAddressDnsSettings { DomainNameLabel = pipDnsName },
            PublicIPAllocationMethod = "Dynamic" // This sample doesn't support static IP addresses but can be extended to do so
        });

    return createPipTask;
}
```

#### Erstellen des virtuellen Netzwerks
Alle virtuellen Computer, die mit den Resource Manager-APIs erstellt werden, müssen Teil eines virtuellen Netzwerks sein. Dies gilt auch, wenn der virtuelle Computer der einzige darin enthaltene Computer ist. Das virtuelle Netzwerk muss mindestens ein Subnetz enthalten, aber Sie können auch viele Subnetze verwenden, um Ihre Ressourcen aufzuteilen und besser zu schützen.

```csharp
private static Task<VirtualNetwork> CreateVirtualNetworkAsync(TokenCredentials credentials, string subscriptionId, string resourceGroup, string location, string vNetName, string vNetAddressPrefix, Subnet[] subnets)
{
    Console.WriteLine("Creating Virtual Network");
    var networkClient = new NetworkManagementClient(credentials) { SubscriptionId = subscriptionId };
    var createVNetTask = networkClient.VirtualNetworks.CreateOrUpdateAsync(resourceGroup, vNetName,
        new VirtualNetwork
        {
            Location = location,
            AddressSpace = new AddressSpace(new[] { vNetAddressPrefix }),
            Subnets = subnets
        });

    return createVNetTask;
}
```

#### Erstellen der Netzwerkschnittstellenkarte
Über die Netzwerkschnittstellenkarte (NIC) wird Ihre VM mit dem virtuellen Netzwerk verbunden, in dem sie sich befindet. Eine VM kann über viele NICs verfügen und somit mehreren virtuellen Netzwerken zugeordnet werden. In diesem Beispiel wird davon ausgegangen, dass Sie Ihre VMs nur einem virtuellen Netzwerk zuordnen.

```csharp
private static Task<NetworkInterface> CreateNetworkInterfaceAsync(TokenCredentials credentials, string subscriptionId, string resourceGroup, string location, string nicName, string nicIPConfigName, PublicIPAddress pip, Subnet subnet)
{
    Console.WriteLine("Creating Network Interface");
    var networkClient = new NetworkManagementClient(credentials) { SubscriptionId = subscriptionId };
    var createNicTask = networkClient.NetworkInterfaces.CreateOrUpdateAsync(resourceGroup, nicName,
        new NetworkInterface()
        {
            Location = location,
            IpConfigurations = new[] {
                new NetworkInterfaceIPConfiguration
                {
                    Name = nicIPConfigName,
                    PrivateIPAllocationMethod = "Dynamic",
                    PublicIPAddress = pip,
                    Subnet = subnet
                }
            }
        });

    return createNicTask;
}
```

#### Erstellen des virtuellen Computers
Jetzt können wir den virtuellen Computer erstellen. Die VM ist (direkt oder indirekt) von allen zuvor erstellten Ressourcen abhängig. Sie müssen also auf die Fertigstellung bzw. Betriebsbereitschaft aller Ressourcen warten, bevor Sie versuchen, eine VM bereitzustellen. Die Bereitstellung einer VM dauert länger als die Erstellung der anderen Ressourcen. Es kann also sein, dass es für die Anwendung zu einer Wartezeit kommt.

```csharp
private static async Task<VirtualMachine> CreateVirtualMachineAsync(TokenCredentials credentials, string subscriptionId, string resourceGroup, string location, string storageAccountName, string vmName, string vmSize, string vmAdminUsername, string vmAdminPassword, string vmImagePublisher, string vmImageOffer, string vmImageSku, string vmImageVersion, string vmOSDiskName, string nicId)
{
    Console.WriteLine("Creating Virtual Machine (this may take a while)");
    var computeClient = new ComputeManagementClient(credentials) { SubscriptionId = subscriptionId };
    var vm = await computeClient.VirtualMachines.CreateOrUpdateAsync(resourceGroup, vmName,
        new VirtualMachine
        {
            Location = location,
            HardwareProfile = new HardwareProfile(vmSize),
            OsProfile = new OSProfile(vmName, vmAdminUsername, vmAdminPassword),
            StorageProfile = new StorageProfile(
                new ImageReference
                {
                    Publisher = vmImagePublisher,
                    Offer = vmImageOffer,
                    Sku = vmImageSku,
                    Version = vmImageVersion
                },
                new OSDisk
                {
                    Name = vmOSDiskName,
                    Vhd = new VirtualHardDisk($"http://{storageAccountName}.blob.core.windows.net/vhds/{vmOSDiskName}.vhd"),
                    Caching = "ReadWrite",
                    CreateOption = "FromImage"
                }),
            NetworkProfile = new NetworkProfile(
                new[] { new NetworkInterfaceReference { Id = nicId } }),
            DiagnosticsProfile = new DiagnosticsProfile(
                new BootDiagnostics
                {
                    Enabled = true,
                    StorageUri = $"http://{storageAccountName}.blob.core.windows.net"
                })
        });

    return vm;
}
```

### Bereitstellen einer Vorlage
Eine ausführliche Anleitung zur Bereitstellung einer Vorlage finden Sie im Artikel [Bereitstellen eines virtuellen Azure-Computers mit C# und einer Resource Manager-Vorlage](./virtual-machines/virtual-machines-windows-csharp-template.md).

Kurz gesagt: Das Bereitstellen einer Vorlage ist viel einfacher als die manuelle Bereitstellung von Ressourcen. Der folgende Code veranschaulicht, wie Sie hierbei vorgehen, indem Sie auf die URIs der Vorlage und einer Parameterdatei verweisen.

```csharp
private static async Task<DeploymentExtended> CreateTemplatedDeployment(TokenCredentials credentials, string subscriptionId, string resourceGroup, string templateUri, string parametersUri)
{
    var resourceClient = new ResourceManagementClient(credentials) { SubscriptionId = subscriptionId };

    return await resourceClient.Deployments.BeginCreateOrUpdateAsync(resourceGroup, "mytemplateddeployment", new Deployment(
        new DeploymentProperties()
        {
            Mode = DeploymentMode.Incremental,
            TemplateLink = new TemplateLink(templateUri),
            ParametersLink = new ParametersLink(parametersUri)
        }));

}
```

<!---HONumber=AcomDC_0629_2016-->