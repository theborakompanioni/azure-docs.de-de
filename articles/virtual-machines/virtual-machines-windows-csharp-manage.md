<properties
	pageTitle="Verwalten von virtuellen Computern mithilfe von Azure Resource Manager und C# | Microsoft Azure"
	description="Verwalten Sie virtuelle Computer mit Azure Resource Manager und C#."
	services="virtual-machines-windows"
	documentationCenter=""
	authors="davidmu1"
	manager="timlt"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="na"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="04/17/2016"
	ms.author="davidmu"/>

# Verwalten von virtuellen Azure-Computern mit Azure Resource Manager und C#  

Zum Ausführen der Anweisungen in diesem Artikel benötigen Sie Folgendes:

- [Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx)
- [Einen Authentifizierungstoken](../resource-group-authenticate-service-principal.md)

## Erstellen eines Visual Studio-Projekts und Installieren von Paketen

NuGet-Pakete sind die einfachste Möglichkeit, um die Bibliotheken zu installieren, die Sie zum Fertigstellen der Aufgaben in diesem Artikel benötigen. Sie müssen die Azure Active Directory-Authentifizierungsbibliothek und die Computerressourcenanbieter-Bibliothek installieren. Führen Sie folgende Schritte aus, um diese Bibliotheken in Visual Studio abzurufen:

1. Klicken Sie auf **Datei** > **Neu** > **Projekt**.

2. Wählen Sie unter **Vorlagen** > **Visual C#** **Konsolenanwendung** aus, geben Sie den Namen und Speicherort des Projekts ein, und klicken Sie dann auf **OK**.

3. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen und anschließend auf **NuGet-Pakete verwalten**.

4. Geben Sie *Active Directory* in das Suchfeld ein, klicken Sie für das Active Directory-Authentifizierungsbibliothek-Paket auf **Installieren**, und befolgen Sie dann die Anweisungen zum Installieren des Pakets.

5. Wählen Sie am oberen Rand der Seite **Vorabversion einschließen** aus. Geben Sie *Microsoft.Azure.Management.Compute* in das Suchfeld ein, klicken Sie für die Compute-Bibliotheken von .NET auf **Installieren**, und befolgen Sie dann die Anweisungen zum Installieren des Pakets.

Sie können nun die Bibliotheken verwenden, um Ihre virtuellen Computer zu verwalten.

## Einrichten des Projekts

Nachdem die Azure Active Directory-Anwendung erstellt und die Authentifizierungsbibliothek installiert wurde, formatieren Sie nun die Anwendungsinformationen zu Anmeldeinformationen, die zum Authentifizieren von Anforderungen an den Azure-Ressourcen-Manager verwendet werden.

1. Öffnen Sie die Datei „Program.cs“ für das von Ihnen erstellte Projekt, und fügen Sie dann die folgenden using-Anweisungen am Anfang der Datei hinzu:

        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.Compute;
        using Microsoft.Azure.Management.Compute.Models;
        using Microsoft.Rest;
        
2. Fügen Sie der Main-Methode der Program-Klasse Variablen hinzu, um die Namen der Ressourcen, die Sie verwalten möchten, den Speicherort der Ressourcen (z. B. „USA, Mitte“) und Ihre Abonnement-ID anzugeben:

        var groupName = "resource group name";
        var vmName = "virtual machine name";  
        var location = "location name";
        var subscriptionId = "subsciption id";

    Ersetzen Sie alle Variablenwerte durch die Namen und den Bezeichner, die Sie verwenden möchten. Sie erhalten die Abonnement-ID durch Ausführen von Get-AzureRmSubscription.
    
3. Fügen Sie der Program-Klasse die folgende Methode hinzu, um das Token abzurufen, das zum Erstellen der Anmeldeinformationen benötigt wird:

	    private static string GetAuthorizationHeader()
	    {
          ClientCredential cc = new ClientCredential("{application-id}", "{password}");
          var context = new AuthenticationContext("https://login.windows.net/{tenant-id}");
          var result = context.AcquireTokenAsync("https://management.azure.com/", cc);

          if (result == null)
          {
            throw new InvalidOperationException("Failed to obtain the JWT token");
          }

          string token = result.Result.AccessToken;

          return token;
        }
	
    Ersetzen Sie {application-id} durch die Anwendungs-ID, die Sie zuvor notiert haben, {password} durch das Kennwort, das Sie für die AD-Anwendung gewählt haben, und {tenant-id} durch die Mandanten-ID für Ihr Abonnement.
    
4. Fügen Sie der Main-Methode in der Datei „Program.cs“ den folgenden Code hinzu, um die Anmeldeinformationen zu erstellen:

        var token = GetAuthorizationHeader();
        var credential = new TokenCredentials(token);

5. Speichern Sie die Datei "Program.cs".

## Anzeigen von Informationen zu einem virtuellen Computer

1. Fügen Sie der Program-Klasse im Projekt, das Sie zuvor erstellt haben, diese Methode hinzu:

        public static void GetVirtualMachine(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Getting information about the virtual machine...");

          var computeManagementClient = new ComputeManagementClient(credential);
          computeManagementClient.SubscriptionId = subscriptionId;
          var vmResult = computeManagementClient.VirtualMachines.Get(
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
          Console.WriteLine("    uri: " + vmResult.StorageProfile.OsDisk.Vhd.Uri);
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

2. Fügen Sie der Main-Methode den folgenden Code hinzu, um die gerade hinzugefügte Methode aufzurufen:

        GetVirtualMachine(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();
    
3. Speichern Sie die Datei "Program.cs".

4. Klicken Sie in Visual Studio auf **Starten**, und melden Sie sich dann bei Azure AD mit demselben Benutzernamen und Kennwort an, die Sie für Ihr Abonnement verwenden.

	Beim Ausführen dieser Methode sollte die Ausgabe ähnlich dem Folgenden aussehen:
    
        Getting information about the virtual machine...
        hardwareProfile
          vmSize: Standard_A0

        storageProfile
          imageReference
            publisher: MicrosoftWindowsServer
            offer: WindowsServer
            sku: 2012-R2-Datacenter
            version: latest
          osDisk
            osType: Windows
            name: myosdisk
            createOption: FromImage
            uri: http://store1.blob.core.windows.net/vhds/myosdisk.vhd
            caching: ReadWrite

          osProfile
            computerName: vm1
            adminUsername: account1
            provisionVMAgent: True
            enableAutomaticUpdates: True

          networkProfile
            networkInterface 
              id: /subscriptions/{subscription-id}
                /resourceGroups/rg1/providers/Microsoft.Network/networkInterfaces/nc1

          vmAgent
            vmAgentVersion2.7.1198.766
            statuses
            code: ProvisioningState/succeeded
            level: Info
            displayStatus: Ready
            message: GuestAgent is running and accepting new configurations.
            time: 4/13/2016 8:35:32 PM

          disks
            name: myosdisk
            statuses
              code: ProvisioningState/succeeded
              level: Info
              displayStatus: Provisioning succeeded
              time: 4/13/2016 8:04:36 PM

          VM general status
            provisioningStatus: Succeeded
            id: /subscriptions/{subscription-id}
              /resourceGroups/rg1/providers/Microsoft.Compute/virtualMachines/vm1
            name: vm1
            type: Microsoft.Compute/virtualMachines
            location: centralus

          VM instance status
            code: ProvisioningState/succeeded
              level: Info
              displayStatus: Provisioning succeeded
            code: PowerState/running
              level: Info
              displayStatus: VM running

## Starten eines virtuellen Computers

1. Kommentieren Sie Code aus, den Sie zuvor der Main-Methode hinzugefügt haben, mit Ausnahme des Codes, der zum Abrufen der Anmeldeinformationen erforderlich ist.

2. Fügen Sie der Program-Klasse die folgende Methode hinzu:

        public static void StartVirtualMachine(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Starting the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential);
          computeManagementClient.SubscriptionId = subscriptionId;
          computeManagementClient.VirtualMachines.Start(groupName, vmName);
        }

3. Fügen Sie der Main-Methode den folgenden Code hinzu, um die gerade hinzugefügte Methode aufzurufen:

        StartVirtualMachine(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Speichern Sie die Datei "Program.cs".

5. Klicken Sie in Visual Studio auf **Starten**, und melden Sie sich dann bei Azure AD mit demselben Benutzernamen und Kennwort an, die Sie für Ihr Abonnement verwenden.

	Daraufhin sollte der Status des virtuellen Computers in „Wird ausgeführt“ geändert werden.

## Beenden eines virtuellen Computers

Sie können einen virtuellen Computer auf zwei Arten beenden. Sie können einen virtuellen Computer beenden und alle zugehörigen Einstellungen beibehalten, was Ihnen jedoch weiterhin in Rechnung gestellt wird. Alternativ können Sie einen virtuellen Computer beenden und ihn freigegeben, wodurch auch alle ihm zugeordneten Ressourcen freigegeben werden und die Abrechnung für den virtuellen Computer beendet wird.

1. Kommentieren Sie Code aus, den Sie zuvor der Main-Methode hinzugefügt haben, mit Ausnahme des Codes, der zum Abrufen der Anmeldeinformationen erforderlich ist.

2. Fügen Sie der Program-Klasse die folgende Methode hinzu:

        public static void StopVirtualMachine(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Stopping the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential);
          computeManagementClient.SubscriptionId = subscriptionId;
          computeManagementClient.VirtualMachines.PowerOff(groupName, vmName);
        }

	Wenn Sie den virtuellen Computer freigeben möchten, ändern Sie den Aufruf zum Herunterfahren in Folgendes:

        computeManagementClient.VirtualMachines.Deallocate(groupName, vmName);

3. Fügen Sie der Main-Methode den folgenden Code hinzu, um die gerade hinzugefügte Methode aufzurufen:

        StopVirtualMachine(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Speichern Sie die Datei "Program.cs".

5. Klicken Sie in Visual Studio auf **Starten**, und melden Sie sich dann bei Azure AD mit demselben Benutzernamen und Kennwort an, die Sie für Ihr Abonnement verwenden.

    Daraufhin sollte der Status des virtuellen Computers in „Beendet“ geändert werden. Wenn Sie die Methode ausgeführt haben, die „Freigegeben“ aufruft, lautet der Status „Beendet (Zuordnung aufgehoben)“.

## Neustarten eines ausgeführten virtuellen Computers

1. Kommentieren Sie Code aus, den Sie zuvor der Main-Methode hinzugefügt haben, mit Ausnahme des Codes, der zum Abrufen der Anmeldeinformationen erforderlich ist.

2. Fügen Sie der Program-Klasse die folgende Methode hinzu:

        public static void RestartVirtualMachine(
          TokenCredentials credential,
          string groupName,
          string vmName,
          string subscriptionId)
        {
          Console.WriteLine("Restarting the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential);
          computeManagementClient.SubscriptionId = subscriptionId;
          computeManagementClient.VirtualMachines.Restart(groupName, vmName);
        }

3. Fügen Sie der Main-Methode den folgenden Code hinzu, um die gerade hinzugefügte Methode aufzurufen:

        RestartVirtualMachine(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Speichern Sie die Datei "Program.cs".

5. Klicken Sie in Visual Studio auf **Starten**, und melden Sie sich dann bei Azure AD mit demselben Benutzernamen und Kennwort an, die Sie für Ihr Abonnement verwenden.

## Löschen eines virtuellen Computers

1. Kommentieren Sie Code aus, den Sie zuvor der Main-Methode hinzugefügt haben, mit Ausnahme des Codes, der zum Abrufen der Anmeldeinformationen erforderlich ist.

2. Fügen Sie der Program-Klasse die folgende Methode hinzu:

        public static void DeleteVirtualMachine(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Deleting the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential);
          computeManagementClient.SubscriptionId = subscriptionId;
          computeManagementClient.VirtualMachines.Delete(groupName, vmName);
        }

3. Fügen Sie der Main-Methode den folgenden Code hinzu, um die gerade hinzugefügte Methode aufzurufen:

        DeleteVirtualMachine(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Speichern Sie die Datei "Program.cs".

5. Klicken Sie in Visual Studio auf **Starten**, und melden Sie sich dann bei Azure AD mit demselben Benutzernamen und Kennwort an, die Sie für Ihr Abonnement verwenden.

## Aktualisieren eines virtuellen Computers

Dieses Beispiel zeigt, wie Sie die Größe eines ausgeführten virtuellen Computers ändern können.

1. Kommentieren Sie Code aus, den Sie zuvor der Main-Methode hinzugefügt haben, mit Ausnahme des Codes, der zum Abrufen der Anmeldeinformationen erforderlich ist.

2. Fügen Sie der Program-Klasse die folgende Methode hinzu:

        public static void UpdateVirtualMachine(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Updating the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential);
          computeManagementClient.SubscriptionId = subscriptionId;
          var vmResult = computeManagementClient.VirtualMachines.Get(groupName, vmName);
          vmResult.HardwareProfile.VmSize = "Standard_A1";
          computeManagementClient.VirtualMachines.CreateOrUpdate(groupName, vmName, vmResult);
        }

3. Fügen Sie der Main-Methode den folgenden Code hinzu, um die gerade hinzugefügte Methode aufzurufen:

        UpdateVirtualMachine(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Speichern Sie die Datei "Program.cs".

5. Klicken Sie in Visual Studio auf **Starten**, und melden Sie sich dann bei Azure AD mit demselben Benutzernamen und Kennwort an, die Sie für Ihr Abonnement verwenden.

    Daraufhin sollte die Größe des virtuellen Computers in „Standard\_A1“ geändert werden.
    
## Nächste Schritte

Falls bei der Bereitstellung Probleme aufgetreten sind, finden Sie womöglich Hilfe unter [Problembehandlung beim Bereitstellen von Ressourcengruppen mit dem Azure-Portal](../resource-manager-troubleshoot-deployments-portal.md).

<!---HONumber=AcomDC_0420_2016-->