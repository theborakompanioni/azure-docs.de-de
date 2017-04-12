---
title: Bereitstellen eines virtuellen Azure-Computers mit C# | Microsoft-Dokumentation
description: "Verwenden Sie C# und Azure Resource Manager, um einen virtuellen Computer und alle unterstützenden Ressourcen bereitzustellen."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 87524373-5f52-4f4b-94af-50bf7b65c277
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/01/2017
ms.author: davidmu
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: f4c63af2d873fb11c8503a30b104b9b7db7f74f0
ms.lasthandoff: 04/03/2017


---
# <a name="deploy-an-azure-virtual-machine-using-c"></a>Bereitstellen eines virtuellen Azure-Computers mit C## #

In diesem Artikel wird gezeigt, wie Sie C# zum Erstellen eines virtuellen Azure-Computers und unterstützender Ressourcen verwenden.

Die Ausführung dieser Schritte dauert etwa 20 Minuten.

## <a name="step-1-create-a-visual-studio-project"></a>Schritt 1: Erstellen eines Visual Studio-Projekts

In diesem Schritt stellen Sie sicher, dass Visual Studio installiert ist, und erstellen eine Konsolenanwendung, die zum Erstellen der Ressourcen verwendet wird.

1. Wenn Sie dies noch nicht getan haben, installieren Sie [Visual Studio](https://www.visualstudio.com/).
2. Klicken Sie in Visual Studio auf **Datei** > **Neu** > **Projekt**.
3. Wählen Sie unter **Vorlagen** > **Visual C#** die Option **Konsolenanwendung** aus, geben Sie Name und Speicherort des Projekts ein, und klicken Sie dann auf **OK**.

## <a name="step-2-install-libraries"></a>Schritt 2: Installieren von Bibliotheken

NuGet-Pakete sind die einfachste Möglichkeit, um die Bibliotheken zu installieren, die Sie zum Ausführen dieser Schritte benötigen. Gehen Sie wie folgt vor, um die in Visual Studio benötigten Bibliotheken zu erhalten:


1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen, klicken Sie anschließend auf **NuGet-Pakete verwalten** und dann auf **Durchsuchen**.
2. Geben Sie *Microsoft.IdentityModel.Clients.ActiveDirectory* in das Suchfeld ein, klicken Sie auf **Installieren**, und befolgen Sie dann die Anweisungen zum Installieren des Pakets.
3. Wählen Sie am oberen Rand der Seite **Vorabversion einschließen**aus. Geben Sie *Microsoft.Azure.Management.Compute* in das Suchfeld ein, klicken Sie auf **Installieren**, und befolgen Sie dann die Anweisungen zum Installieren des Pakets.
4. Geben Sie *Microsoft.Azure.Management.Network* in das Suchfeld ein, klicken Sie auf **Installieren**, und befolgen Sie dann die Anweisungen zum Installieren des Pakets.
5. Geben Sie *Microsoft.Azure.Management.Storage* in das Suchfeld ein, klicken Sie auf **Installieren**, und befolgen Sie dann die Anweisungen zum Installieren des Pakets.
6. Geben Sie *Microsoft.Azure.Management.ResourceManager* in das Suchfeld ein, klicken Sie auf **Installieren**, und befolgen Sie dann die Anweisungen zum Installieren des Pakets.

Sie können nun die Bibliotheken verwenden, um Ihre Anwendung zu erstellen.

## <a name="step-3-create-the-credentials-used-to-authenticate-requests"></a>Schritt 3: Erstellen der Anmeldeinformationen zum Authentifizieren von Anforderungen

Bevor Sie mit diesem Schritt beginnen, stellen Sie sicher, dass Sie Zugriff auf einen [Active Directory-Dienstprinzipal](../../resource-group-authenticate-service-principal.md) haben. Vom Dienstprinzipal erhalten Sie ein Token zum Authentifizieren von Anforderungen an Azure Resource Manager.

1. Öffnen Sie die Datei „Program.cs“ für das von Ihnen erstellte Projekt, und fügen Sie dann die folgenden using-Anweisungen am Anfang der Datei hinzu:
   
    ```
    using Microsoft.Azure;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.Azure.Management.Storage;
    using Microsoft.Azure.Management.Storage.Models;
    using Microsoft.Azure.Management.Network;
    using Microsoft.Azure.Management.Network.Models;
    using Microsoft.Azure.Management.Compute;
    using Microsoft.Azure.Management.Compute.Models;
    using Microsoft.Rest;
    ```

2. Fügen Sie der Program-Klasse die folgende Methode hinzu, um das Token abzurufen, das zum Erstellen der Anmeldeinformationen benötigt wird:
   
    ```    
    private static async Task<AuthenticationResult> GetAccessTokenAsync()
    {
      var cc = new ClientCredential("{client-id}", "{client-secret}");
      var context = new AuthenticationContext("https://login.windows.net/{tenant-id}");
      var token = await context.AcquireTokenAsync("https://management.azure.com/", cc);
      if (token == null)
      {
        throw new InvalidOperationException("Could not get the token");
      }
      return token;
    }
    ```

    Ersetzen Sie die folgenden Werte:
    
    - *{client-id}* durch den Bezeichner der Azure Active Directory-Anwendung. Diesen Bezeichner finden Sie auf dem Blatt „Eigenschaften“ der AD-Anwendung. Um die AD-Anwendung im Azure-Portal zu suchen, klicken Sie im Ressourcenmenü auf **Azure Active Directory**, und klicken Sie dann auf **App-Registrierungen**.
    - *{client-secret}* durch den Zugriffsschlüssel der AD-Anwendung. Diesen Bezeichner finden Sie auf dem Blatt „Eigenschaften“ der AD-Anwendung.
    - *{tenant-id}* durch die Mandanten-ID Ihres Abonnements. Die Mandanten-ID finden Sie auf dem Blatt „Eigenschaften“ für Azure Active Directory im Azure-Portal. Sie ist als *Verzeichnis-ID* bezeichnet.

3. Fügen Sie der Main-Methode in der Datei „Program.cs“ den folgenden Code hinzu, um die zuvor hinzugefügte Methode aufzurufen:
   
    ```
    var token = GetAccessTokenAsync();
    var credential = new TokenCredentials(token.Result.AccessToken);
    ```

4. Speichern Sie die Datei "Program.cs".

## <a name="step-3-create-the-resources"></a>Schritt 3: Erstellen der Ressourcen

### <a name="register-the-providers-and-create-a-resource-group"></a>Registrieren Sie die Anbieter, und erstellen Sie eine Ressourcengruppe.

Alle Ressourcen müssen in einer Ressourcengruppe enthalten sein. Bevor Sie Ressourcen zu einer Gruppe hinzufügen können, muss Ihr Abonnement bei Ressourcenanbietern registriert werden.

1. Fügen Sie der Main-Methode der Program-Klasse Variablen hinzu, um die Namen anzugeben, die Sie für die Ressourcen verwenden möchten:

    ```   
    var groupName = "myResourceGroup";
    var subscriptionId = "subsciptionId";
    var storageName = "myStorageAccount";
    var ipName = "myPublicIP";
    var subnetName = "mySubnet";
    var vnetName = "myVnet";
    var nicName = "myNIC";
    var avSetName = "myAVSet";
    var vmName = "myVM";  
    var location = "location";
    var adminName = "adminName";
    var adminPassword = "adminPassword";
    ```

    Ersetzen Sie die folgenden Werte:

    - Alle Ressourcennamen, die mit *my* beginnen, durch sinnvolle Namen für Ihre Umgebung
    - *subscriptionId* mit Ihrer Abonnement-ID. Die Abonnement-ID finden Sie auf dem Blatt „Abonnements“ im Azure-Portal.
    - *location* durch die [Azure-Region](https://azure.microsoft.com/regions/), in der Sie die Ressourcen erstellen möchten
    - *adminName* durch den Namen und das Kennwort des Administratorkontos auf dem virtuellen Computer
    - *adminPassword* durch das Kennwort des Administratorkontos

2. Fügen Sie der Program-Klasse die folgende Methode hinzu, um die Ressourcengruppe zu erstellen und die Anbieter zu registrieren:
   
    ```
    public static async Task<ResourceGroup> CreateResourceGroupAsync(
      TokenCredentials credential,
      string groupName,
      string subscriptionId,
      string location)
    {
      var resourceManagementClient = new ResourceManagementClient(credential)
        { SubscriptionId = subscriptionId };
   
      Console.WriteLine("Registering the providers...");
      var rpResult = resourceManagementClient.Providers.Register("Microsoft.Storage");
      Console.WriteLine(rpResult.RegistrationState);
      rpResult = resourceManagementClient.Providers.Register("Microsoft.Network");
      Console.WriteLine(rpResult.RegistrationState);
      rpResult = resourceManagementClient.Providers.Register("Microsoft.Compute");
      Console.WriteLine(rpResult.RegistrationState);
   
      Console.WriteLine("Creating the resource group...");
      var resourceGroup = new ResourceGroup { Location = location };
      return await resourceManagementClient.ResourceGroups.CreateOrUpdateAsync(
        groupName, 
        resourceGroup);
    }
    ```

3. Fügen Sie der Main-Methode den folgenden Code hinzu, um die zuvor hinzugefügte Methode aufzurufen:
   
    ```   
    var rgResult = CreateResourceGroupAsync(
      credential,
      groupName,
      subscriptionId,
      location);
    Console.WriteLine(rgResult.Result.Properties.ProvisioningState);
    Console.ReadLine();
    ```

### <a name="create-a-storage-account"></a>Erstellen Sie ein Speicherkonto.

Bei Verwendung eines nicht verwalteten Datenträgers wird ein [Speicherkonto](../../storage/storage-create-storage-account.md) benötigt, um die für den virtuellen Computer erstellte virtuelle Festplattendatei zu speichern.

1. Fügen Sie der Program-Klasse die folgende Methode hinzu, um das Speicherkonto zu erstellen:

    ```   
    public static async Task<StorageAccount> CreateStorageAccountAsync(
      TokenCredentials credential,       
      string groupName,
      string subscriptionId,
      string location,
      string storageName)
    {
      var storageManagementClient = new StorageManagementClient(credential)
        { SubscriptionId = subscriptionId };
      
      Console.WriteLine("Creating the storage account...");

      return await storageManagementClient.StorageAccounts.CreateAsync(
        groupName,
        storageName,
        new StorageAccountCreateParameters()
          {
            Sku = new Microsoft.Azure.Management.Storage.Models.Sku() 
              { Name = SkuName.StandardLRS},
            Kind = Kind.Storage,
            Location = location
          }
      );
    }
    ```

2. Fügen Sie der Main-Methode den folgenden Code hinzu, um die zuvor hinzugefügte Methode aufzurufen:
   
    ```
    var stResult = CreateStorageAccountAsync(
      credential,
      groupName,
      subscriptionId,
      location,
      storageName);
    Console.WriteLine(stResult.Result.ProvisioningState);  
    Console.ReadLine();
    ```

### <a name="create-a-public-ip-address"></a>Erstellen einer öffentlichen IP-Adresse

Eine öffentliche IP-Adresse ist für die Kommunikation mit der virtuellen Maschine erforderlich.

1. Fügen Sie der Program-Klasse die folgende Methode hinzu, um die öffentliche IP-Adresse des virtuellen Computers zu erstellen:
   
    ```
    public static async Task<PublicIPAddress> CreatePublicIPAddressAsync(
      TokenCredentials credential,  
      string groupName,
      string subscriptionId,
      string location,
      string ipName)
    {
      var networkManagementClient = new NetworkManagementClient(credential)
        { SubscriptionId = subscriptionId };
      
      Console.WriteLine("Creating the public ip...");

      return await networkManagementClient.PublicIPAddresses.CreateOrUpdateAsync(
        groupName,
        ipName,
        new PublicIPAddress
          {
            Location = location,
            PublicIPAllocationMethod = "Dynamic"
          }
      );
    }
    ```

2. Fügen Sie der Main-Methode den folgenden Code hinzu, um die zuvor hinzugefügte Methode aufzurufen:
   
    ```   
    var ipResult = CreatePublicIPAddressAsync(
      credential,
      groupName,
      subscriptionId,
      location,
      ipName);
    Console.WriteLine(ipResult.Result.ProvisioningState);  
    Console.ReadLine();
    ```

### <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

Eine virtuelle Maschine, die mit dem Ressourcen-Manager-Bereitstellungsmodell erstellt wurde, muss sich in einem virtuellen Netzwerk befinden.

1. Fügen Sie der Program-Klasse die folgende Methode hinzu, um ein Subnetz und ein virtuelles Netzwerk zu erstellen:

    ```   
    public static async Task<VirtualNetwork> CreateVirtualNetworkAsync(
      TokenCredentials credential,
      string groupName,
      string subscriptionId,
      string location,
      string vnetName,
      string subnetName)
    {
      var networkManagementClient = new NetworkManagementClient(credential)
        { SubscriptionId = subscriptionId };
   
      var subnet = new Subnet
        {
          Name = subnetName,
          AddressPrefix = "10.0.0.0/24"
        };
   
      var address = new AddressSpace 
        {
          AddressPrefixes = new List<string> { "10.0.0.0/16" }
        };
         
      Console.WriteLine("Creating the virtual network...");
      return await networkManagementClient.VirtualNetworks.CreateOrUpdateAsync(
        groupName,
        vnetName,
        new VirtualNetwork
          {
            Location = location,
            AddressSpace = address,
            Subnets = new List<Subnet> { subnet }
          }
      );
    }
    ```

2. Fügen Sie der Main-Methode den folgenden Code hinzu, um die zuvor hinzugefügte Methode aufzurufen:

    ```   
    var vnResult = CreateVirtualNetworkAsync(
      credential,
      groupName,
      subscriptionId,
      location,
      vnetName,
      subnetName);
    Console.WriteLine(vnResult.Result.ProvisioningState);  
    Console.ReadLine();
    ```

### <a name="create-a-network-interface"></a>Erstellen einer Netzwerkschnittstelle

Ein virtueller Computer benötigt eine Netzwerkschnittstelle, um in dem virtuellen Netzwerk kommunizieren zu können.

1. Fügen Sie der Program-Klasse die folgende Methode hinzu, um eine Netzwerkschnittstelle zu erstellen:

    ```   
    public static async Task<NetworkInterface> CreateNetworkInterfaceAsync(
      TokenCredentials credential,
      string groupName,
      string subscriptionId,
      string location,
      string subnetName,
      string vnetName,
      string ipName,
      string nicName)
    {
      var networkManagementClient = new NetworkManagementClient(credential)
        { SubscriptionId = subscriptionId };
      var subnet = await networkManagementClient.Subnets.GetAsync(
        groupName,
        vnetName,
        subnetName
      );
      var publicIP = await networkManagementClient.PublicIPAddresses.GetAsync(
        groupName, 
        ipName
      );
   
      Console.WriteLine("Creating the network interface...");
      return await networkManagementClient.NetworkInterfaces.CreateOrUpdateAsync(
        groupName,
        nicName,
        new NetworkInterface
          {
            Location = location,
            IpConfigurations = new List<NetworkInterfaceIPConfiguration>
              {
                new NetworkInterfaceIPConfiguration
                  {
                    Name = nicName,
                    PublicIPAddress = pubipResponse,
                    Subnet = subnetResponse
                  }
              }
          }
      );
    }
    ```

2. Fügen Sie der Main-Methode den folgenden Code hinzu, um die zuvor hinzugefügte Methode aufzurufen:
   
    ```
    var ncResult = CreateNetworkInterfaceAsync(
      credential,
      groupName,
      subscriptionId,
      location,
      subnetName,
      vnetName,
      ipName,
      nicName);
    Console.WriteLine(ncResult.Result.ProvisioningState);  
    Console.ReadLine();
    ```

### <a name="create-an-availability-set"></a>Verfügbarkeitsgruppe erstellen

Verfügbarkeitsgruppen erleichtern die Verwaltung der Wartung der virtuellen Maschinen, die von Ihrer Anwendung verwendet werden.

1. Fügen Sie der Program-Klasse die folgende Methode hinzu, um die Verfügbarkeitsgruppe zu erstellen:

    ```   
    public static async Task<AvailabilitySet> CreateAvailabilitySetAsync(
      TokenCredentials credential,
      string groupName,
      string subscriptionId,
      string location,
      string avsetName)
    {
      var computeManagementClient = new ComputeManagementClient(credential)
        { SubscriptionId = subscriptionId };
        
      Console.WriteLine("Creating the availability set...");
      return await computeManagementClient.AvailabilitySets.CreateOrUpdateAsync(
        groupName,
        avsetName,
        new AvailabilitySet()
          { Location = location }
      );
    }
    ```

2. Fügen Sie der Main-Methode den folgenden Code hinzu, um die zuvor hinzugefügte Methode aufzurufen:
   
    ```
    var avResult = CreateAvailabilitySetAsync(
      credential,  
      groupName,
      subscriptionId,
      location,
      avSetName);
    Console.ReadLine();
    ```

### <a name="create-a-virtual-machine"></a>Erstellen eines virtuellen Computers

Nachdem Sie nun alle benötigten Ressourcen erstellt haben, können Sie einen virtuellen Computer erstellen.

1. Fügen Sie der Program-Klasse die folgende Methode hinzu, um den virtuellen Computer zu erstellen:

    ```   
    public static async Task<VirtualMachine> CreateVirtualMachineAsync(
      TokenCredentials credential, 
      string groupName,
      string subscriptionId,
      string location,
      string nicName,
      string avsetName,
      string storageName,
      string adminName,
      string adminPassword,
      string vmName)
    {
      var networkManagementClient = new NetworkManagementClient(credential)
        { SubscriptionId = subscriptionId };
      var computeManagementClient = new ComputeManagementClient(credential)
        { SubscriptionId = subscriptionId };  
      var nic = await networkManagementClient.NetworkInterfaces.GetAsync(
        groupName, 
        nicName);
      var avSet = await computeManagementClient.AvailabilitySets.GetAsync(
        groupName, 
        avsetName);
   
      Console.WriteLine("Creating the virtual machine...");
      return await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(
        groupName,
        vmName,
        new VirtualMachine
          {
            Location = location,
            AvailabilitySet = new Microsoft.Azure.Management.Compute.Models.SubResource
              {
                Id = avSet.Id
              },
            HardwareProfile = new HardwareProfile
              {
                VmSize = "Standard_A0"
              },
            OsProfile = new OSProfile
              {
                AdminUsername = adminName,
                AdminPassword = adminPassword,
                ComputerName = vmName,
                WindowsConfiguration = new WindowsConfiguration
                  {
                    ProvisionVMAgent = true
                  }
              },
            NetworkProfile = new NetworkProfile
              {
                NetworkInterfaces = new List<NetworkInterfaceReference>
                  {
                    new NetworkInterfaceReference { Id = nic.Id }
                  }
              },
            StorageProfile = new StorageProfile
              {
                ImageReference = new ImageReference
                  {
                    Publisher = "MicrosoftWindowsServer",
                    Offer = "WindowsServer",
                    Sku = "2012-R2-Datacenter",
                    Version = "latest"
                  },
                OsDisk = new OSDisk
                  {
                    Name = "mytestod1",
                    CreateOption = DiskCreateOptionTypes.FromImage,
                    Vhd = new VirtualHardDisk
                      {
                        Uri = "http://" + storageName + ".blob.core.windows.net/vhds/mytestod1.vhd"
                      }
                  }
              }
          }
      );
    }
    ```

    > [!NOTE]
    > In diesem Tutorial wird eine virtuelle Maschine erstellt, auf dem eine Version des Windows Server-Betriebssystems ausgeführt wird. Weitere Informationen zur Auswahl von anderen Images finden Sie unter [Navigieren zu und Auswählen von Images virtueller Linux-Computer in Azure mithilfe der Befehlszeilenschnittstelle oder PowerShell](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
    > 
    >

2. Fügen Sie der Main-Methode den folgenden Code hinzu, um die zuvor hinzugefügte Methode aufzurufen:
   
    ```
    var vmResult = CreateVirtualMachineAsync(
      credential,
      groupName,
      subscriptionId,
      location,
      nicName,
      avSetName,
      storageName,
      adminName,
      adminPassword,
      vmName);
    Console.WriteLine(vmResult.Result.ProvisioningState);
    Console.ReadLine();
    ```

## <a name="step-4-delete-the-resources"></a>Schritt 4: Löschen der Ressourcen

Da in Azure die genutzten Ressourcen in Rechnung gestellt werden, empfiehlt es sich grundsätzlich, nicht mehr benötigte Ressourcen zu löschen. Um den virtuellen Computer und alle unterstützenden Ressourcen zu löschen, müssen Sie lediglich die Ressourcengruppe löschen.

1. Fügen Sie der Program-Klasse die folgende Methode hinzu, um die Ressourcengruppe zu löschen:
   
    ```
    public static async void DeleteResourceGroupAsync(
      TokenCredentials credential,
      string groupName,
      string subscriptionId)
    {
      Console.WriteLine("Deleting resource group...");
      var resourceManagementClient = new ResourceManagementClient(credential)
        { SubscriptionId = subscriptionId };
      await resourceManagementClient.ResourceGroups.DeleteAsync(groupName);
    }
    ```

2. Fügen Sie der Main-Methode den folgenden Code hinzu, um die zuvor hinzugefügte Methode aufzurufen:
   
    ```   
    DeleteResourceGroupAsync(
      credential,
      groupName,
      subscriptionId);
    Console.ReadLine();
    ```

## <a name="step-5-run-the-console-application"></a>Schritt 5: Ausführen der Konsolenanwendung

1. Klicken Sie zum Ausführen der Konsolenanwendung in Visual Studio auf **Starten** , und melden Sie sich dann bei Azure AD mit demselben Benutzernamen und Kennwort an, die Sie für Ihr Abonnement verwenden.

2. Wenn der Status *Erfolgreich* angezeigt wird, drücken Sie die **EINGABETASTE**. 
   
3. Nach dem Erstellen des virtuellen Computers und vor dem Drücken der **EINGABETASTE** zum Löschen der Ressourcen können Sie sich ein paar Minuten Zeit nehmen, die Ressourcen im Azure-Portal zu überprüfen.

## <a name="next-steps"></a>Nächste Schritte
* Nutzen Sie die Vorteile der Erstellung eines virtuellen Computers per Vorlage, indem Sie sich die Informationen unter [Bereitstellen eines virtuellen Azure-Computers mit C# und einer Resource Manager-Vorlage](csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)durchlesen.
* Informationen zur Verwaltung des erstellten virtuellen Computers finden Sie unter [Verwalten von virtuellen Azure-Computern mit Azure Resource Manager und C#](csharp-manage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


