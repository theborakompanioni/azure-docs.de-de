<properties
	pageTitle="Erstellen eines virtuellen Computer mithilfe von C# und einer Resource Manager-Vorlage | Microsoft Azure"
	description="Erfahren Sie, wie Sie mit C# und einer Resource Manager-Vorlage einen virtuellen Azure-Computer bereitstellen."
	services="virtual-machines-windows"
	documentationCenter=""
	authors="davidmu1"
	manager="timlt"
	editor="tysonn"
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="na"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/24/2016"
	ms.author="davidmu"/>

# Bereitstellen eines virtuellen Azure-Computers mit C# und einer Resource Manager-Vorlage

Durch das Verwenden von Ressourcengruppen und Vorlagen, können Sie alle Ressourcen gemeinsam verwalten, die Ihre Anwendung unterstützen. In diesem Artikel erfahren Sie, wie Sie die Authentifizierung und den Speicher mit Azure PowerShell einrichten und dann eine Vorlage mit C# erstellen und bereitstellen können, um Azure-Ressourcen zu erstellen.

Zuerst müssen Sie sicherstellen, dass Sie Folgendes erledigt haben:

- Installieren von [Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx)
- Überprüfen der Installation von [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) oder [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855)
- Abrufen eines [Authentifizierungstokens](../resource-group-authenticate-service-principal.md)

Die Durchführung dieser Schritte dauert etwa 30 Minuten.
    
## Schritt 1: Erstellen einer Ressourcengruppe für die Speicherung der Vorlage

Alle Ressourcen müssen in einer Ressourcengruppe bereitgestellt werden. Weitere Informationen finden Sie unter [Übersicht über den Azure Resource Manager](../resource-group-overview.md).

1. Rufen Sie eine Liste mit den verfügbaren Standorten ab, an denen Ressourcen erstellt werden können.

	    Get-AzureRmLocation | sort Location | Select Location
        
2. Ersetzen Sie den Wert von **$locName** durch einen Standort aus der Liste, z.B. **USA, Mitte**. Erstellen Sie die Variable.

        $locName = "location name"
        
3. Ersetzen Sie den Wert von **$rgName** durch den Namen der neuen Ressourcengruppe. Erstellen Sie die Variable und die Ressourcengruppe.

        $rgName = "resource group name"
        New-AzureRmResourceGroup -Name $rgName -Location $locName
        
    Die Ausgabe sollte folgendermaßen aussehen:
    
        ResourceGroupName : myrg1
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/{subscription-id}/resourceGroups/myrg1
    
## Schritt 2: Erstellen eines Speicherkontos und des Vorlagencontainers

Ein Speicherkonto ist erforderlich, um die Vorlage zu speichern, die Sie erstellen und bereitstellen möchten.

1. Ersetzen Sie den Wert von „$stName“ (nur Kleinbuchstaben und Zahlen) durch den Namen des Speicherkontos. Testen Sie den Namen auf Eindeutigkeit.

        $stName = "storage account name"
        Get-AzureRmStorageAccountNameAvailability $stName

    Wenn dieser Befehl **True** zurückgibt, ist der vorgeschlagene Name eindeutig.
    
2. Führen Sie nun den Befehl zum Erstellen des Speicherkontos aus.
    
        New-AzureRmStorageAccount -ResourceGroupName $rgName -Name $stName -SkuName "Standard_LRS" -Kind "Storage" -Location $locName
        
3. Ersetzen Sie {blob-storage-endpoint} durch den Endpunkt des Blobspeichers in Ihrem Konto. Ersetzen Sie {storage-account-name} durch den Namen Ihres Speicherkontos. Ersetzen Sie {primary-storage-key} durch den primären Zugriffsschlüssel. Führen Sie diese Befehle zum Erstellen des Containers aus, in dem die Dateien gespeichert sind. Sie können die Endpunkt- und Schlüsselwerte aus dem Azure-Portal abrufen.

        $ConnectionString = "DefaultEndpointsProtocol=http;BlobEndpoint={blob-storage-endpoint};AccountName={storage-account-name};AccountKey={primary-storage-key}"
        $ctx = New-AzureStorageContext -ConnnectionString $ConnectionString
        New-AzureStorageContainer -Name "templates" -Permission Blob -Context $ctx

## Schritt 3: Erstellen des Visual Studio-Projekts, der Vorlagendatei und der Parameterdatei

### Erstellen der Vorlagendatei

Mithilfe der Azure-Ressourcen-Manager-Vorlage können Sie die Azure-Ressourcen gemeinsam bereitstellen und verwalten. Sie verwenden hierfür eine JSON-Beschreibung der Ressourcen sowie der zugeordneten Bereitstellungsparameter.

Führen Sie in Visual Studio Folgendes aus:

1. Klicken Sie auf **Datei** > **Neu** > **Projekt**.

2. Wählen Sie unter **Vorlagen** > **Visual C#** **Konsolenanwendung** aus, geben Sie den Namen und Speicherort des Projekts ein, und klicken Sie dann auf **OK**.

3. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen und dann auf **Hinzufügen** > **Neues Element**.

4. Klicken Sie auf „Web“, wählen Sie „JSON-Datei“ aus, geben Sie *VirtualMachineTemplate.json* als Namen ein, und klicken Sie dann auf **Hinzufügen**.

5. Fügen Sie zwischen die öffnenden und schließenden Klammern der Datei „VirtualMachineTemplate.json“ das erforderliche Schemaelement und das erforderliche contentVersion-Element ein:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
        }

6. [Parameter](../resource-group-authoring-templates.md#parameters) sind nicht immer erforderlich, aber sie erleichtern die Vorlagenverwaltung. Sie beschreiben den Typ des Werts, ggf. den Standardwert und möglicherweise die zulässigen Werte des Parameters. Für dieses Tutorial werden die Parameter, mit denen ein virtueller Computer, ein Speicherkonto und ein virtuelles Netzwerk erstellt werden, der Vorlage hinzugefügt. Fügen Sie das Parameterelement und seine untergeordneten Elemente nach dem ContentVersion-Element hinzu:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "newStorageAccountName": { "type": "string" },
            "adminUserName": { "type": "string" },
            "adminPassword": { "type": "securestring" },
            "dnsNameForPublicIP": { "type": "string" }
          },
        }

7. [Variablen](../resource-group-authoring-templates.md#variables) können in einer Vorlage verwendet werden, um die Werte festzulegen, die sich häufig ändern, oder um die Werte festzulegen, die aus einer Kombination von Parameterwerten erstellt werden müssen. Fügen Sie das Variablenelement nach dem Parameterabschnitt hinzu:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "newStorageAccountName": { "type": "string" },
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" },
            "dnsNameForPublicIP": { "type": "string" },
          },
          "variables": {
            "location": "West US",
            "imagePublisher": "MicrosoftWindowsServer",
            "imageOffer": "WindowsServer",
            "windowsOSVersion": "2012-R2-Datacenter",
            "OSDiskName": "osdiskforwindowssimple",
            "nicName": "myVMnic1",
            "addressPrefix": "10.0.0.0/16",
            "subnetName": "Subnet",
            "subnetPrefix": "10.0.0.0/24",
            "storageAccountType": "Standard_LRS",
            "publicIPAddressName": "myPublicIP",
            "publicIPAddressType": "Dynamic",
            "vmStorageAccountContainerName": "vhds",
            "vmName": "MyWindowsVM",
            "vmSize": "Standard_A2",
            "virtualNetworkName": "MyVNET",
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
            "subnet1Ref": "[concat(variables('vnetID'),'/subnets/',variables('subnet1Name'))]"  
          },
        }

8. [Ressourcen](../resource-group-authoring-templates.md#resources) wie der virtuelle Computer, das virtuelle Netzwerk und das Speicherkonto werden als Nächstes in der Vorlage definiert. Fügen Sie nach dem Variablenabschnitt den Ressourcenabschnitt hinzu:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "newStorageAccountName": { "type": "string" },
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" },
            "dnsNameForPublicIP": { "type": "string" }
          },
          "variables": {
            "location": "West US",
            "imagePublisher": "MicrosoftWindowsServer",
            "imageOffer": "WindowsServer",
            "OSDiskName": "osdiskforwindowssimple",
            "windowsOSVersion": "2012-R2-Datacenter",
            "nicName": "myVMnic1",
            "addressPrefix": "10.0.0.0/16",
            "subnetName": "Subnet",
            "subnetPrefix": "10.0.0.0/24",
            "storageAccountType": "Standard_LRS",
            "publicIPAddressName": "myPublicIP",
            "publicIPAddressType": "Dynamic",
            "vmStorageAccountContainerName": "vhds",
            "vmName": "MyWindowsVM",
            "vmSize": "Standard_A2",
            "virtualNetworkName": "MyVNET",
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]"
          },
          "resources": [
            {
              "apiVersion": "2015-06-15",
              "type": "Microsoft.Storage/storageAccounts",
              "name": "[parameters('newStorageAccountName')]",
              "location": "[variables('location')]",
              "properties": {
                "accountType": "[variables('storageAccountType')]"
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/publicIPAddresses",
              "name": "[variables('publicIPAddressName')]",
              "location": "[variables('location')]",
              "properties": {
                "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
                "dnsSettings": {
                  "domainNameLabel": "[parameters('dnsNameForPublicIP')]"
                }
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/virtualNetworks",
              "name": "[variables('virtualNetworkName')]",
              "location": "[variables('location')]",
              "properties": {
                "addressSpace": {
                  "addressPrefixes": [ "[variables('addressPrefix')]" ]
                },
                "subnets": [ {
                  "name": "[variables('subnetName')]",
                  "properties": {
                    "addressPrefix": "[variables('subnetPrefix')]"
                  }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/networkInterfaces",
              "name": "[variables('nicName')]",
              "location": "[variables('location')]",
              "dependsOn": [
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]",
                "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
              ],
              "properties": {
                "ipConfigurations": [ {
                  "name": "ipconfig1",
                  "properties": {
                    "privateIPAllocationMethod": "Dynamic",
                    "publicIPAddress": {
                      "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicIPAddressName'))]"
                    },
                    "subnet": {
                      "id": "[variables('subnetRef')]"
                    }
                  }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Compute/virtualMachines",
              "name": "[variables('vmName')]",
              "location": "[variables('location')]",
              "dependsOn": [
                "[concat('Microsoft.Storage/storageAccounts/', parameters('newStorageAccountName'))]",
                "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
              ],
              "properties": {
                "hardwareProfile": {
                  "vmSize": "[variables('vmSize')]"
                },
                "osProfile": {
                  "computerName": "[variables('vmName')]",
                  "adminUsername": "[parameters('adminUsername')]",
                  "adminPassword": "[parameters('adminPassword')]",
                },
                "storageProfile": {
                  "imageReference": {
                    "publisher": "[variables('imagePublisher')]",
                    "offer": "[variables('imageOffer')]",
                    "sku": "[variables('windowsOSVersion')]",
                    "version" : "latest"
                  },
                  "osDisk": {
                    "name": "osdisk",
                    "vhd": {
                      "uri": "[concat('http://',parameters('newStorageAccountName'),'.blob.core.windows.net/',variables('vmStorageAccountContainerName'),'/',variables('OSDiskName'),'.vhd')]"
                    },
                    "caching": "ReadWrite",
                    "createOption": "FromImage"
                  }
                },
                "networkProfile": {
                  "networkInterfaces" : [ {
                    "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
                  } ]
                }
              }
            } ]
          }
      
9. Speichern Sie Vorlagendatei, die Sie erstellt haben.

### Erstellen der Parameterdatei

Um Werte für die Ressourcenparameter anzugeben, die in der Vorlage definiert wurden, müssen Sie eine Parameterdatei erstellen, die die Werte enthält und sie mit der Vorlage an den Ressourcen-Manager senden. Führen Sie in Visual Studio Folgendes aus:

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen und dann auf **Hinzufügen** > **Neues Element**.

2. Klicken Sie auf „Web“, wählen Sie „JSON-Datei“ aus, geben Sie *Parameters.json* als Namen ein, und klicken Sie dann auf **Hinzufügen**.

3. Öffnen Sie die Datei „parameters.json“, und fügen Sie den folgenden JSON-Inhalt hinzu:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "newStorageAccountName": { "value": "mytestsa1" },
            "adminUserName": { "value": "mytestacct1" },
            "adminPassword": { "value": "mytestpass1" },
            "dnsNameForPublicIP": { "value": "mytestdns1" }
          }
        }

    >[AZURE.NOTE] In diesem Artikel wird ein virtueller Computer erstellt, auf dem eine Version des Windows Server-Betriebssystems ausgeführt wird. Weitere Informationen zur Auswahl von anderen Images finden Sie unter [Navigieren zu und Auswählen von Images virtueller Linux-Computer in Azure mithilfe der Befehlszeilenschnittstelle oder PowerShell](virtual-machines-linux-cli-ps-findimage.md).

4. Speichern Sie Parameterdatei, die Sie erstellt haben.

### Hochladen der Dateien und Festlegen der Nutzungsberechtigung 

Der Azure-Ressourcen-Manager greift auf die Vorlagendatei und die Parameterdatei in einem Azure-Speicherkonto zu. Um die Dateien in dem ersten von Ihnen erstellten Speicher abzulegen, führen Sie folgende Schritte aus:

1. Öffnen Sie Cloud-Explorer, und navigieren Sie dann zum Vorlagencontainer in dem Speicherkonto, das Sie zuvor erstellt haben.

2. Klicken Sie in der rechten, oberen Ecke des Vorlagencontainerfensters auf das Symbol zum Hochladen von Blobs, navigieren Sie zur Datei „VirtualMachineTemplate.json“, die Sie erstellt haben, und klicken Sie dann auf **Öffnen**.

3. Klicken Sie erneut auf das Symbol zum Hochladen von Blobs, navigieren Sie zur Datei "Parameters.json", die Sie erstellt haben, und klicken Sie dann auf **Öffnen**.

## Schritt 4: Installieren der Bibliotheken

NuGet-Pakete sind die einfachste Möglichkeit, um die Bibliotheken zu installieren, die Sie zum Abschluss dieses Lernprogramms benötigen. Sie müssen die Azure-Ressourcenverwaltungsbibliothek und die Azure Active Directory-Authentifizierungsbibliothek installieren. Führen Sie folgende Schritte aus, um diese Bibliotheken in Visual Studio abzurufen:

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen und anschließend auf **NuGet-Pakete verwalten**.

2. Geben Sie *Active Directory* in das Suchfeld ein, klicken Sie für das Active Directory-Authentifizierungsbibliothek-Paket auf **Installieren**, und befolgen Sie dann die Anweisungen zum Installieren des Pakets.

4. Wählen Sie am oberen Rand der Seite **Vorabversion einschließen** aus. Geben Sie *Microsoft.Azure.Management.ResourceManager* in das Suchfeld ein, klicken Sie für die Microsoft Azure-Ressourcenverwaltungsbibliotheken auf **Installieren**, und befolgen Sie dann die Anweisungen zum Installieren des Pakets.

Sie können nun die Bibliotheken verwenden, um Ihre Anwendung zu erstellen.

##Schritt 5: Erstellen der Anmeldeinformationen zum Authentifizieren von Anforderungen

Nachdem die Azure Active Directory-Anwendung erstellt und die Authentifizierungsbibliothek installiert wurde, formatieren Sie nun die Anwendungsinformationen als Anmeldeinformationen, die zum Authentifizieren von Anforderungen an den Azure Resource Manager verwendet werden. Gehen Sie folgendermaßen vor:

1. Öffnen Sie die Datei „Program.cs“ für das von Ihnen erstellte Projekt, und fügen Sie dann die folgenden using-Anweisungen am Anfang der Datei hinzu:

        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.Azure.Management.ResourceManager.Models;
        using Microsoft.Rest;

2.	Fügen Sie der Program-Klasse die folgende Methode hinzu, um das Token abzurufen, das zum Erstellen der Anmeldeinformationen benötigt wird:

        private static async Task<AuthenticationResult> GetAccessTokenAsync()
        {
          var cc = new ClientCredential("{client-id}", "{client-secret}");
          var context = new AuthenticationContext("https://login.windows.net/{tenant-id}");
          var token = await context.AcquireTokenAsync("https://management.azure.com/", cc);
          if (token == null)
          {
            throw new InvalidOperationException("Could not get the token.");
          }
          return token;
        }

    Ersetzen Sie {client-id} durch den Bezeichner der Azure Active Directory-Anwendung, {-client-secret} durch den Zugriffsschlüssel der AD-Anwendung und {tenant-id} durch die Mandanten-ID für Ihr Abonnement. Sie erhalten die Mandanten-ID durch Ausführen von Get-AzureRmSubscription. Den Zugriffsschlüssel finden Sie mithilfe des Azure-Portals.

3. Fügen Sie der Main-Methode in der Datei „Program.cs“ den folgenden Code hinzu, um die Anmeldeinformationen zu erstellen:

        var token = GetAccessTokenAsync();
        var credential = new TokenCredentials(token.Result.AccessToken);

4. Speichern Sie die Datei "Program.cs".

## Schritt 6: Hinzufügen von Code zum Bereitstellen der Vorlage

In diesem Schritt verwenden Sie die [ResourceGroup](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.models.resourcegroup.aspx)- und [ResourceManagementClient](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.resourcemanagementclient.aspx)-Klassen, um die Ressourcengruppe zu erstellen, in der die Ressourcen bereitgestellt werden.

1. Fügen Sie der Main-Methode der Program-Klasse Variablen hinzu, um die Namen, die Sie für die Ressourcen verwenden möchten, den Speicherort der Ressourcen (z. B. „USA, Mitte“), Administratorkontoinformationen und Ihre Abonnement-ID anzugeben:

        var groupName = "resource group name";
        var storageName = "storage account name";
        var location = "location name";
        var subscriptionId = "subsciption id";

    Ersetzen Sie alle Variablenwerte durch die Namen und den Bezeichner, die Sie verwenden möchten. Sie erhalten die Abonnement-ID durch Ausführen von Get-AzureRmSubscription. Der Wert der storageName-Variablen ist der Name des Speicherkontos, in dem die Vorlage gespeichert wurde.
    
2. Fügen Sie der Program-Klasse die folgende Methode hinzu, um die Ressourcengruppe zu erstellen:

        public static async Task<ResourceGroup> CreateResourceGroupAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location)
        {
          Console.WriteLine("Creating the resource group...");
          var resourceManagementClient = new ResourceManagementClient(credential) 
            { SubscriptionId = subscriptionId };
          var resourceGroup = new ResourceGroup { Location = location };
          return await resourceManagementClient.ResourceGroups.CreateOrUpdateAsync(groupName, resourceGroup);
        }

2. Fügen Sie der Main-Methode den folgenden Code hinzu, um die gerade hinzugefügte Methode aufzurufen:

        var rgResult = CreateResourceGroupAsync(
          credential,
          groupName,
          subscriptionId,
          location);
        Console.WriteLine(rgResult.Result.Properties.ProvisioningState);
        Console.ReadLine();

3. Fügen Sie der Program-Klasse die folgende Methode hinzu, um der Ressourcengruppe mithilfe der von Ihnen definierten Vorlage die Ressourcen bereitzustellen:

        public static async Task<DeploymentExtended> CreateTemplateDeploymentAsync(
          TokenCredentials credential,
          string groupName,
          string storageName,
          string deploymentName,
          string subscriptionId)
        {
          Console.WriteLine("Creating the template deployment...");
          var deployment = new Deployment();
          deployment.Properties = new DeploymentProperties
          {
            Mode = DeploymentMode.Incremental,
            TemplateLink = new TemplateLink
            {
              Uri = "https://" + storageName + ".blob.core.windows.net/templates/VirtualMachineTemplate.json"
            },
            ParametersLink = new ParametersLink
            {
              Uri = "https://" + storageName + ".blob.core.windows.net/templates/Parameters.json"
            }
          };
          var resourceManagementClient = new ResourceManagementClient(credential) 
            { SubscriptionId = subscriptionId };
          return await resourceManagementClient.Deployments.CreateOrUpdateAsync(
            groupName,
            deploymentName,
            deployment);
        }

4. Fügen Sie der Main-Methode den folgenden Code hinzu, um die gerade hinzugefügte Methode aufzurufen:

        var dpResult = CreateTemplateDeploymentAsync(
          credential,
          groupName",
          storageName,
          deploymentName,
          subscriptionId);
        Console.WriteLine(dpResult.Result.Properties.ProvisioningState);
        Console.ReadLine();

##Schritt 7: Hinzufügen von Code zum Löschen der Ressourcen

Da in Azure die genutzten Ressourcen in Rechnung gestellt werden, empfiehlt es sich grundsätzlich, nicht mehr benötigte Ressourcen zu löschen. Sie müssen nicht jede Ressource einzeln aus einer Ressourcengruppe löschen. Löschen Sie die Ressourcengruppe, sodass alle darin enthaltenen Ressourcen automatisch gelöscht werden.

1.	Fügen Sie der Program-Klasse die folgende Methode hinzu, um die Ressourcengruppe zu löschen:

        public static async void DeleteResourceGroupAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId)
        {
          Console.WriteLine("Deleting resource group...");
          var resourceManagementClient = new ResourceManagementClient(credential)
            { SubscriptionId = subscriptionId };
          return await resourceManagementClient.ResourceGroups.DeleteAsync(groupName);
        }

2.	Fügen Sie der Main-Methode den folgenden Code hinzu, um die gerade hinzugefügte Methode aufzurufen:

        DeleteResourceGroupAsync(
          credential,
          groupName,
          subscriptionId);
        Console.ReadLine();

##Schritt 8: Ausführen der Konsolenanwendung

1.	Klicken Sie zum Ausführen der Konsolenanwendung in Visual Studio auf **Starten**, und melden Sie sich dann bei Azure AD mit denselben Anmeldeinformationen an, die Sie für Ihr Abonnement verwenden.

2.	Wenn der Status „Akzeptiert“ angezeigt wird, drücken Sie die **EINGABETASTE**.

	Die vollständige Ausführung dieser Konsolenanwendung von Anfang bis zum Ende sollte etwa 5 Minuten dauern. Bevor Sie die EINGABETASTE drücken, um das Löschen der Ressourcen zu starten, können Sie sich ein paar Minuten Zeit nehmen, um die Erstellung der Ressourcen im Azure-Portal zu überprüfen, bevor Sie diese löschen.

3. Navigieren Sie im Azure-Portal zu den Überwachungsprotokollen, um den Status der Ressourcen anzuzeigen:

	![Durchsuchen von Überwachungsprotokollen im Azure-Portal](./media/virtual-machines-windows-csharp-template/crpportal.png)

## Nächste Schritte

- Falls bei der Bereitstellung Probleme aufgetreten sind, können Sie beispielsweise mit dem Artikel [Problembehandlung beim Bereitstellen von Ressourcengruppen mit dem Azure-Portal](../resource-manager-troubleshoot-deployments-portal.md) fortfahren.
- Informationen zum Verwalten des gerade erstellten virtuellen Computers finden Sie unter [Verwalten von virtuellen Computern mit Azure Resource Manager und PowerShell](virtual-machines-windows-csharp-manage.md).

<!---HONumber=AcomDC_0629_2016-->