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
ms.date: 06/20/2017
ms.author: davidmu
ms.translationtype: Human Translation
ms.sourcegitcommit: 61fd58063063d69e891d294e627ae40cb878d65b
ms.openlocfilehash: f04783896eb36e9a8c3e5cc8422a1fb054b36dd2
ms.contentlocale: de-de
ms.lasthandoff: 06/23/2017

---
# <a name="create-and-manage-windows-vms-in-azure-using-c"></a>Erstellen und Verwalten von virtuellen Windows-Computern in Azure mithilfe von C# #

Für einen [virtuellen Azure-Computer](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) sind mehrere unterstützende Azure-Ressourcen erforderlich. In diesem Artikel wird das Erstellen, Verwalten und Löschen virtueller Computerressourcen mithilfe von C# behandelt. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Erstellen eines Visual Studio-Projekts
> * Installieren von Bibliotheken
> * Erstellen von Anmeldeinformationen
> * Erstellen von Ressourcen
> * Ausführen von Verwaltungsaufgaben
> * Löschen von Ressourcen
> * Ausführen der Anwendung

Die Ausführung dieser Schritte dauert etwa 20 Minuten.

## <a name="create-a-visual-studio-project"></a>Erstellen eines Visual Studio-Projekts

1. Wenn Sie dies noch nicht getan haben, installieren Sie [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio). Wählen Sie auf der Seite „Workloads“  **	.NET-Desktopentwicklung** aus, und klicken Sie dann auf **Installieren**. In der Zusammenfassung sehen Sie, dass **Entwicklungstools für .NET Framework 4 – 4.6** automatisch für Sie ausgewählt ist. Wenn Sie Visual Studio bereits installiert haben, können Sie die .NET-Workload mit dem Visual Studio-Startprogramm hinzufügen.
2. Klicken Sie in Visual Studio auf **Datei** > **Neu** > **Projekt**.
3. Wählen Sie unter **Vorlagen** > **Visual C#** die Option **Konsolen-App (.NET Framework)** aus, geben Sie *myDotnetProject* als Name des Projekts ein, wählen Sie den Projektspeicherort, und klicken Sie dann auf **OK**.

## <a name="install-libraries"></a>Installieren von Bibliotheken

NuGet-Pakete sind die einfachste Möglichkeit, um die Bibliotheken zu installieren, die Sie zum Ausführen dieser Schritte benötigen. Gehen Sie wie folgt vor, um die in Visual Studio benötigten Bibliotheken zu erhalten:

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf *myDotnetProject*, und klicken Sie auf **NuGet-Pakete für Projektmappe verwalten...** und dann auf **Durchsuchen**.
2. Geben Sie *Microsoft.IdentityModel.Clients.ActiveDirectory* in das Suchfeld ein, klicken Sie auf **Installieren**, und befolgen Sie dann die Anweisungen zum Installieren des Pakets.
3. Geben Sie *Microsoft.Azure.Management.Compute* in das Suchfeld ein, klicken Sie auf **Installieren**, und befolgen Sie dann die Anweisungen zum Installieren des Pakets.
4. Geben Sie *Microsoft.Azure.Management.Network* in das Suchfeld ein, klicken Sie auf **Installieren**, und befolgen Sie dann die Anweisungen zum Installieren des Pakets.
5. Geben Sie *Microsoft.Azure.Management.ResourceManager* in das Suchfeld ein, klicken Sie auf **Installieren**, und befolgen Sie dann die Anweisungen zum Installieren des Pakets.

Sie können nun die Bibliotheken verwenden, um Ihre Anwendung zu erstellen.

## <a name="create-credentials"></a>Erstellen von Anmeldeinformationen

Bevor Sie mit diesem Schritt beginnen, stellen Sie sicher, dass Sie Zugriff auf einen [Active Directory-Dienstprinzipal](../../azure-resource-manager/resource-group-create-service-principal-portal.md) haben. Sie sollten auch die Anwendungs-ID, den Authentifizierungsschlüssel und die Mandanten-ID aufzeichnen. Sie benötigen diese Angaben in einem späteren Schritt.

1. Öffnen Sie die Datei „Program.cs“, die erstellt wurde, und fügen Sie am Anfang der Datei den vorhandenen Anweisungen die folgenden using-Anweisungen hinzu:
   
    ```
    using Microsoft.Azure;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.Azure.Management.Network;
    using Microsoft.Azure.Management.Network.Models;
    using Microsoft.Azure.Management.Compute;
    using Microsoft.Azure.Management.Compute.Models;
    using Microsoft.Rest;
    ```

2. Fügen Sie als Nächstes in der Main-Methode der Datei „Program.cs“ Variablen hinzu, um die im Code verwendeten allgemeinen Werten anzugeben:

    ```   
    var groupName = "myResourceGroup";
    var subscriptionId = "subsciption-id";
    var location = "westus";
    var vmName = "myVM";
    ```

    Ersetzen Sie **subscription-id** durch Ihren Abonnementbezeichner.

3. Um die Active Directory-Anmeldeinformationen zu erstellen, die Sie benötigen, um Anforderungen zu erstellen, fügen Sie der Program-Klasse diese Methode hinzu:
   
    ```    
    private static async Task<AuthenticationResult> GetAccessTokenAsync()
    {
      var cc = new ClientCredential("application-id", "authentication-key");
      var context = new AuthenticationContext("https://login.microsoftonline.com/tenant-id");
      var token = await context.AcquireTokenAsync("https://management.azure.com/", cc);
      if (token == null)
      {
        throw new InvalidOperationException("Could not get the token");
      }
      return token;
    }
    ```

    Ersetzen Sie **application-id**, **authentication-key** und **tenant-id** mit den Werten, die Sie zuvor beim Erstellen Ihres Azure Active Directory-Dienstprinzipals erfasst haben.

4. Fügen Sie der Main-Methode in der Datei „Program.cs“ den folgenden Code hinzu, um die zuvor hinzugefügte Methode aufzurufen:
   
    ```
    var token = GetAccessTokenAsync();
    var credential = new TokenCredentials(token.Result.AccessToken);
    ```

## <a name="create-resources"></a>Erstellen von Ressourcen

### <a name="initialize-management-clients"></a>Initialisieren von Verwaltungsclients

Verwaltungsclients sind zum Erstellen und Verwalten von Ressourcen mithilfe des .NET SDK in Azure erforderlich. Fügen Sie der Main-Methode in der Datei „Program.cs“ den folgenden Code hinzu, um damit die Verwaltungsclients zu erstellen:

```
var resourceManagementClient = new ResourceManagementClient(credential)
    { SubscriptionId = subscriptionId };
var networkManagementClient = new NetworkManagementClient(credential)
    { SubscriptionId = subscriptionId };
var computeManagementClient = new ComputeManagementClient(credential)
    { SubscriptionId = subscriptionId };
```

### <a name="create-the-vm-and-supporting-resources"></a>Erstellen des virtuellen Computers und der unterstützenden Ressourcen

Alle Ressourcen müssen in einer [Ressourcengruppe](../../azure-resource-manager/resource-group-overview.md) enthalten sein.

1. Fügen Sie der Program-Klasse die folgende Methode hinzu, um die Ressourcengruppe zu erstellen:
   
    ```
    public static async Task<ResourceGroup> CreateResourceGroupAsync(
      ResourceManagementClient resourceManagementClient,
      string groupName,
      string location)
    {   
      var resourceGroup = new ResourceGroup { Location = location };
      return await resourceManagementClient.ResourceGroups.CreateOrUpdateAsync(
        groupName, 
        resourceGroup);
    }
    ```

2. Fügen Sie der Main-Methode den folgenden Code hinzu, um die zuvor hinzugefügte Methode aufzurufen:
   
    ```   
    var rgResult = CreateResourceGroupAsync(
      resourceManagementClient,
      groupName,
      location);
    Console.WriteLine("Resource group creation: " + 
      rgResult.Result.Properties.ProvisioningState + 
      ". Press enter to continue...");
    Console.ReadLine();
    ```

[Verfügbarkeitsgruppen](tutorial-availability-sets.md) erleichtern die Verwaltung der virtuellen Computer, die von Ihrer Anwendung verwendet werden.

1. Fügen Sie der Program-Klasse die folgende Methode hinzu, um die Verfügbarkeitsgruppe zu erstellen:

    ```   
    public static async Task<AvailabilitySet> CreateAvailabilitySetAsync(
      ComputeManagementClient computeManagementClient,
      string groupName,
      string location)
    {
      return await computeManagementClient.AvailabilitySets.CreateOrUpdateAsync(
        groupName,
        "myAVSet",
        new AvailabilitySet()
        { 
          Sku = new Microsoft.Azure.Management.Compute.Models.Sku("Aligned"),
          PlatformFaultDomainCount = 3,
          Location = location 
        });
    }
    ```

2. Fügen Sie der Main-Methode den folgenden Code hinzu, um die zuvor hinzugefügte Methode aufzurufen:
   
    ```
    var avResult = CreateAvailabilitySetAsync(
      computeManagementClient,
      groupName,
      location);
    Console.WriteLine("Availability set created. Press enter to continue...");
    Console.ReadLine();
    ```

Eine [öffentliche IP-Adresse](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) ist für die Kommunikation mit dem virtuellen Computer erforderlich.

1. Fügen Sie der Program-Klasse die folgende Methode hinzu, um die öffentliche IP-Adresse des virtuellen Computers zu erstellen:
   
    ```
    public static async Task<PublicIPAddress> CreatePublicIPAddressAsync( 
      NetworkManagementClient networkManagementClient,
      string groupName,
      string location)
    {
      return await networkManagementClient.PublicIPAddresses.CreateOrUpdateAsync(
        groupName,
        "myIpAddress",
        new PublicIPAddress
        {
          Location = location,
          PublicIPAllocationMethod = "Dynamic"
        });
    }
    ```

2. Fügen Sie der Main-Methode den folgenden Code hinzu, um die zuvor hinzugefügte Methode aufzurufen:
   
    ```   
    var ipResult = CreatePublicIPAddressAsync(
      networkManagementClient,
      groupName,
      location);
    Console.WriteLine("IP address creation: " + 
      ipResult.Result.ProvisioningState + 
      ". Press enter to continue...");  
    Console.ReadLine();
    ```

Ein virtueller Computer muss in einem Subnetz eines [virtuellen Netzwerks](../../virtual-network/virtual-networks-overview.md) enthalten sein.

1. Fügen Sie der Program-Klasse die folgende Methode hinzu, um ein Subnetz und ein virtuelles Netzwerk zu erstellen:

    ```   
    public static async Task<VirtualNetwork> CreateVirtualNetworkAsync(
      NetworkManagementClient networkManagementClient,
      string groupName,
      string location)
    {
      var subnet = new Subnet
      {
        Name = "mySubnet",
        AddressPrefix = "10.0.0.0/24"
      };
      var address = new AddressSpace 
      {
        AddressPrefixes = new List<string> { "10.0.0.0/16" }
      };
      return await networkManagementClient.VirtualNetworks.CreateOrUpdateAsync(
        groupName,
        "myVNet",
        new VirtualNetwork
        {
          Location = location,
          AddressSpace = address,
          Subnets = new List<Subnet> { subnet }
        });
    }
    ```

2. Fügen Sie der Main-Methode den folgenden Code hinzu, um die zuvor hinzugefügte Methode aufzurufen:

    ```   
    var vnResult = CreateVirtualNetworkAsync(
      networkManagementClient,
      groupName,
      location);
    Console.WriteLine("Virtual network creation: " + 
      vnResult.Result.ProvisioningState + 
      ". Press enter to continue...");  
    Console.ReadLine();
    ```

Ein virtueller Computer benötigt eine Netzwerkschnittstelle, um in dem virtuellen Netzwerk kommunizieren zu können.

1. Fügen Sie der Program-Klasse die folgende Methode hinzu, um eine Netzwerkschnittstelle zu erstellen:

    ```   
    public static async Task<NetworkInterface> CreateNetworkInterfaceAsync(
      NetworkManagementClient networkManagementClient,
      string groupName,
      string location)
    {
      var subnet = await networkManagementClient.Subnets.GetAsync(
        groupName,
        "myVNet",
        "mySubnet");
      var publicIP = await networkManagementClient.PublicIPAddresses.GetAsync(
        groupName, 
        "myIPaddress");
      return await networkManagementClient.NetworkInterfaces.CreateOrUpdateAsync(
        groupName,
        "myNic",
        new NetworkInterface
        {
          Location = location,
          IpConfigurations = new List<NetworkInterfaceIPConfiguration>
          {
            new NetworkInterfaceIPConfiguration
            {
              Name = "myNic",
              PublicIPAddress = publicIP,
              Subnet = subnet
            }
          }
        });
    }
    ```

2. Fügen Sie der Main-Methode den folgenden Code hinzu, um die zuvor hinzugefügte Methode aufzurufen:
   
    ```
    var ncResult = CreateNetworkInterfaceAsync(
      networkManagementClient,
      groupName,
      location);
    Console.WriteLine("Network interface creation: " + 
      ncResult.Result.ProvisioningState + 
      ". Press enter to continue...");  
    Console.ReadLine();
    ```

Nachdem Sie nun alle benötigten Ressourcen erstellt haben, können Sie einen virtuellen Computer erstellen.

1. Fügen Sie der Program-Klasse die folgende Methode hinzu, um den virtuellen Computer zu erstellen:

    ```   
    public static async Task<VirtualMachine> CreateVirtualMachineAsync(
      NetworkManagementClient networkManagementClient,
      ComputeManagementClient computeManagementClient,
      string groupName,
      string location)
    {
 
      var nic = await networkManagementClient.NetworkInterfaces.GetAsync(
        groupName, 
        "myNic");
      var avSet = await computeManagementClient.AvailabilitySets.GetAsync(
        groupName, 
        "myAVSet");
      return await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(
        groupName,
        "myVM",
        new VirtualMachine
        {
          Location = location,
          AvailabilitySet = new Microsoft.Azure.Management.Compute.Models.SubResource
          {
            Id = avSet.Id
          },
          HardwareProfile = new HardwareProfile
          {
            VmSize = "Standard_DS1"
          },
          OsProfile = new OSProfile
          {
            AdminUsername = "azureuser",
            AdminPassword = "Azure12345678",
            ComputerName = "myVM",
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
            }
          }
        });
    }
    ```

    > [!NOTE]
    > In diesem Tutorial wird eine virtuelle Maschine erstellt, auf dem eine Version des Windows Server-Betriebssystems ausgeführt wird. Weitere Informationen zur Auswahl von anderen Images finden Sie unter [Navigieren zu und Auswählen von Images virtueller Linux-Computer in Azure mithilfe der Befehlszeilenschnittstelle oder PowerShell](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
    > 
    >

2. Fügen Sie der Main-Methode den folgenden Code hinzu, um die zuvor hinzugefügte Methode aufzurufen:
   
    ```
    var vmResult = CreateVirtualMachineAsync(
      networkManagementClient,
      computeManagementClient,
      groupName,
      location);
    Console.WriteLine("Virtual machine creation: " + 
      vmResult.Result.ProvisioningState + 
      ". Press enter to continue...");
    Console.ReadLine();
    ```

## <a name="perform-management-tasks"></a>Ausführen von Verwaltungsaufgaben

Während der Lebensdauer eines virtuellen Computers können Sie Verwaltungsaufgaben wie das Starten, Beenden oder Löschen eines virtuellen Computers ausführen. Darüber hinaus empfiehlt es sich, Code zum Automatisieren von wiederkehrenden oder komplexen Aufgaben zu erstellen.

### <a name="get-information-about-the-vm"></a>Abrufen von Informationen zum virtuellen Computer

1. Fügen Sie der Program-Klasse die folgende Methode hinzu, um Informationen zum virtuellen Computer abzurufen:

    ```
    public static async void GetVirtualMachineAsync(
      string groupName,
      string vmName,
      ComputeManagementClient computeManagementClient)
    {
      Console.WriteLine("Getting information about the virtual machine...");

      var vmResult = await computeManagementClient.VirtualMachines.GetAsync(
        groupName,
        vmName,
        InstanceViewTypes.InstanceView);

      Console.WriteLine("hardwareProfile");
      Console.WriteLine("   vmSize: " + vmResult.HardwareProfile.VmSize);

      Console.WriteLine("\nstorageProfile");
      Console.WriteLine("  imageReference");
      Console.WriteLine("    publisher: " + vmResult.StorageProfile.ImageReference.Publisher);
      Console.WriteLine("    offer: " + vmResult.StorageProfile.ImageReference.Offer);
      Console.WriteLine("    sku: " + vmResult.StorageProfile.ImageReference.Sku);
      Console.WriteLine("    version: " + vmResult.StorageProfile.ImageReference.Version);
      Console.WriteLine("  osDisk");
      Console.WriteLine("    osType: " + vmResult.StorageProfile.OsDisk.OsType);
      Console.WriteLine("    name: " + vmResult.StorageProfile.OsDisk.Name);
      Console.WriteLine("    createOption: " + vmResult.StorageProfile.OsDisk.CreateOption);
      Console.WriteLine("    caching: " + vmResult.StorageProfile.OsDisk.Caching);
      Console.WriteLine("\nosProfile");
      Console.WriteLine("  computerName: " + vmResult.OsProfile.ComputerName);
      Console.WriteLine("  adminUsername: " + vmResult.OsProfile.AdminUsername);
      Console.WriteLine("  provisionVMAgent: " + vmResult.OsProfile.WindowsConfiguration.ProvisionVMAgent.Value);
      Console.WriteLine("  enableAutomaticUpdates: " + vmResult.OsProfile.WindowsConfiguration.EnableAutomaticUpdates.Value);

      Console.WriteLine("\nnetworkProfile");
      foreach (NetworkInterfaceReference nic in vmResult.NetworkProfile.NetworkInterfaces)
      {
        Console.WriteLine("  networkInterface id: " + nic.Id);
      }

      Console.WriteLine("\nvmAgent");
      Console.WriteLine("  vmAgentVersion" + vmResult.InstanceView.VmAgent.VmAgentVersion);
      Console.WriteLine("    statuses");
      foreach (InstanceViewStatus stat in vmResult.InstanceView.VmAgent.Statuses)
      {
        Console.WriteLine("    code: " + stat.Code);
        Console.WriteLine("    level: " + stat.Level);
        Console.WriteLine("    displayStatus: " + stat.DisplayStatus);
        Console.WriteLine("    message: " + stat.Message);
        Console.WriteLine("    time: " + stat.Time);
      }

      Console.WriteLine("\ndisks");
      foreach (DiskInstanceView idisk in vmResult.InstanceView.Disks)
      {
        Console.WriteLine("  name: " + idisk.Name);
        Console.WriteLine("  statuses");
        foreach (InstanceViewStatus istat in idisk.Statuses)
        {
          Console.WriteLine("    code: " + istat.Code);
          Console.WriteLine("    level: " + istat.Level);
          Console.WriteLine("    displayStatus: " + istat.DisplayStatus);
          Console.WriteLine("    time: " + istat.Time);
        }
      }

      Console.WriteLine("\nVM general status");
      Console.WriteLine("  provisioningStatus: " + vmResult.ProvisioningState);
      Console.WriteLine("  id: " + vmResult.Id);
      Console.WriteLine("  name: " + vmResult.Name);
      Console.WriteLine("  type: " + vmResult.Type);
      Console.WriteLine("  location: " + vmResult.Location);
      Console.WriteLine("\nVM instance status");
      foreach (InstanceViewStatus istat in vmResult.InstanceView.Statuses)
      {
        Console.WriteLine("\n  code: " + istat.Code);
        Console.WriteLine("  level: " + istat.Level);
        Console.WriteLine("  displayStatus: " + istat.DisplayStatus);
      }
    }
    ```

2. Fügen Sie der Main-Methode den folgenden Code hinzu, um die zuvor hinzugefügte Methode aufzurufen:

    ```
    GetVirtualMachineAsync(
      groupName,
      vmName,
      computeManagementClient);
    Console.WriteLine("\nPress enter to continue...");
    Console.ReadLine();
    ```

### <a name="stop-the-vm"></a>Beenden des virtuellen Computers

Sie können einen virtuellen Computer beenden und sämtliche Einstellungen beibehalten – dafür werden allerdings weiterhin Gebühren berechnet –, oder Sie können einen virtuellen Computer beenden und die Zuordnung aufheben. Wenn die Zuordnung eines virtuellen Computers aufgehoben wird, wird auch die Zuordnung aller mit diesem Computer verknüpften Ressourcen aufgehoben, und die Abrechnung wird beendet.

1. Zum Beenden des virtuellen Computers ohne Aufheben seiner Zuordnung fügen Sie der Program-Klasse diese Methode hinzu:

    ```
    public static async void StopVirtualMachineAsync(
      string groupName,
      string vmName,
      ComputeManagementClient computeManagementClient)
    {
      Console.WriteLine("Stopping the virtual machine...");
      await computeManagementClient.VirtualMachines.PowerOffAsync(groupName, vmName);
    }
    ```

    Wenn Sie die Zuordnung des virtuellen Computers aufheben möchten, ändern Sie den Aufruf zum Herunterfahren in folgenden Code:

    ```
    await computeManagementClient.VirtualMachines.Deallocate(groupName, vmName);
    ```

2. Fügen Sie der Main-Methode den folgenden Code hinzu, um die gerade hinzugefügte Methode aufzurufen:

    ```
    StopVirtualMachineAsync(
      groupName,
      vmName,
      computeManagementClient);
    Console.WriteLine("\nPress enter to continue...");
    Console.ReadLine();
    ```

### <a name="start-the-vm"></a>Starten des virtuellen Computers

1. Fügen Sie der Program-Klasse die folgende Methode hinzu, um den virtuellen Computer zu starten:

    ```
    public static async void StartVirtualMachineAsync(
      string groupName, 
      string vmName, 
      ComputeManagementClient computeManagementClient)
    {
      Console.WriteLine("Starting the virtual machine...");
      await computeManagementClient.VirtualMachines.StartAsync(groupName, vmName);
    }
    ```

2. Fügen Sie der Main-Methode den folgenden Code hinzu, um die gerade hinzugefügte Methode aufzurufen:

    ```
    StartVirtualMachineAsync(
      groupName,
      vmName,
      ComputeManagementClient computeManagementClient);
    Console.WriteLine("\nPress enter to continue...");
    Console.ReadLine();
    ```

### <a name="resize-the-vm"></a>Ändern der Größe des virtuellen Computers

Viele Aspekte der Bereitstellung müssen berücksichtigt werden, wenn Sie die Größe für den virtuellen Computer festlegen. Weitere Informationen finden Sie unter [VM-Größen](sizes.md).  

1. Fügen Sie der Program-Klasse die folgende Methode hinzu, um die Größe des virtuellen Computers zu ändern:

    ```
    public static async void UpdateVirtualMachineAsync(
      string groupName,
      string vmName,
      ComputeManagementClient computeManagementClient)
    {
      Console.WriteLine("Updating the virtual machine...");
      var vmResult = await computeManagementClient.VirtualMachines.GetAsync(groupName, vmName);
      vmResult.HardwareProfile.VmSize = "Standard_DS3";
      await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(groupName, vmName, vmResult);
    }
    ```

2. Fügen Sie der Main-Methode den folgenden Code hinzu, um die gerade hinzugefügte Methode aufzurufen:

    ```
    UpdateVirtualMachineAsync(
      groupName,
      vmName,
      computeManagementClient);
    Console.WriteLine("\nPress enter to continue...");
    Console.ReadLine();
    ```

### <a name="add-a-data-disk-to-the-vm"></a>Hinzufügen eines Datenträgers zum virtuellen Computer

1. Fügen Sie der Program-Klasse die folgende Methode hinzu, um dem virtuellen Computer einen Datenträger hinzuzufügen:

    ```
    public static async void AddDataDiskAsync(
      string groupName,
      string vmName,
      ComputeManagementClient computeManagementClient)
    {
      Console.WriteLine("Adding the disk to the virtual machine...");
      var vmResult = await computeManagementClient.VirtualMachines.GetAsync(groupName, vmName);
      vmResult.StorageProfile.DataDisks.Add(
        new DataDisk
        {
          Lun = 0,
          Name = "myDataDisk1",
          CreateOption = DiskCreateOptionTypes.Empty,
          DiskSizeGB = 2,
          Caching = CachingTypes.ReadWrite
        });
      await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(groupName, vmName, vmResult);
    }
    ```

2. Fügen Sie der Main-Methode den folgenden Code hinzu, um die gerade hinzugefügte Methode aufzurufen:

    ```
    AddDataDiskAsync(
      groupName,
      vmName,
      computeManagementClient);
    Console.WriteLine("\nPress enter to continue...");
    Console.ReadLine();
    ```

## <a name="delete-resources"></a>Löschen von Ressourcen

Da in Azure die genutzten Ressourcen in Rechnung gestellt werden, empfiehlt es sich grundsätzlich, nicht mehr benötigte Ressourcen zu löschen. Um den virtuellen Computer und alle unterstützenden Ressourcen zu löschen, müssen Sie lediglich die Ressourcengruppe löschen.

1. Fügen Sie der Program-Klasse die folgende Methode hinzu, um die Ressourcengruppe zu löschen:
   
    ```
    public static async void DeleteResourceGroupAsync(
      ResourceManagementClient resourceManagementClient,
      string groupName)
    {
      Console.WriteLine("Deleting resource group...");
      await resourceManagementClient.ResourceGroups.DeleteAsync(groupName);
    }
    ```

2. Fügen Sie der Main-Methode den folgenden Code hinzu, um die zuvor hinzugefügte Methode aufzurufen:
   
    ```   
    DeleteResourceGroupAsync(
      resourceManagementClient,
      groupName);
    Console.ReadLine();
    ```

3. Speichern Sie das Projekt.

## <a name="run-the-application"></a>Ausführen der Anwendung

1. Klicken Sie zum Ausführen der Konsolenanwendung in Visual Studio auf **Starten** , und melden Sie sich dann bei Azure AD mit demselben Benutzernamen und Kennwort an, die Sie für Ihr Abonnement verwenden.

2. Drücken Sie die **EINGABETASTE**, nachdem der Status der einzelnen Ressourcen zurückgegeben wurde. In den Statusinformationen sollte **Erfolgreich** als Bereitstellungsstatus angezeigt werden. Nachdem der virtuelle Computer erstellt wurde, können Sie alle Ressourcen löschen, die Sie erstellt haben. Bevor Sie die **EINGABETASTE** drücken, um mit dem Löschen der Ressourcen zu beginnen, können Sie sich ein paar Minuten Zeit nehmen, um deren Erstellung im Azure-Portal zu überprüfen. Wenn das Azure-Portal geöffnet ist, müssen Sie möglicherweise das Blatt aktualisieren, um neue Ressourcen zu sehen.  

    Die vollständige Ausführung dieser Konsolenanwendung sollte etwa fünf Minuten dauern. Nach Abschluss der Anwendung dauert es möglicherweise einige Minuten, bevor alle Ressourcen und die Ressourcengruppe gelöscht werden.

## <a name="next-steps"></a>Nächste Schritte
* Nutzen Sie die Vorteile der Erstellung eines virtuellen Computers per Vorlage, indem Sie sich die Informationen unter [Bereitstellen eines virtuellen Azure-Computers mit C# und einer Resource Manager-Vorlage](csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)durchlesen.
* Erfahren Sie mehr zur Verwendung der [Azure-Bibliotheken für .NET](https://docs.microsoft.com/dotnet/azure/index?view=azure-dotnet).


