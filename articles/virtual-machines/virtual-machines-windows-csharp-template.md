---
title: Erstellen eines virtuellen Computer mithilfe von C# und einer Resource Manager-Vorlage | Microsoft Docs
description: Erfahren Sie, wie Sie mit C# und einer Resource Manager-Vorlage einen virtuellen Azure-Computer bereitstellen.
services: virtual-machines-windows
documentationcenter: ''
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager

ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/10/2016
ms.author: davidmu

---
# <a name="deploy-an-azure-virtual-machine-using-c#-and-a-resource-manager-template"></a>Bereitstellen eines virtuellen Azure-Computers mit C# und einer Resource Manager-Vorlage
Durch das Verwenden von Ressourcengruppen und Vorlagen können Sie alle Ressourcen gemeinsam verwalten, die Ihre Anwendung unterstützen. In diesem Artikel erfahren Sie, wie Sie mithilfe von Visual Studio und C# eine Authentifizierung einrichten, eine Vorlage erstellen und Azure-Ressourcen mithilfe der erstellten Vorlage bereitstellen.

Zuerst müssen Sie sicherstellen, dass folgende Einrichtungsschritte durchgeführt wurden:

* Installieren von [Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx)
* Überprüfen der Installation von [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) oder [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855)
* Abrufen eines [Authentifizierungstokens](../resource-group-authenticate-service-principal.md)
* Erstellen einer Ressourcengruppe mit [Azure PowerShell](../resource-group-template-deploy.md), der [Azure-Befehlszeilenschnittstelle](../resource-group-template-deploy-cli.md) oder dem [Azure-Portal](../resource-group-template-deploy-portal.md).

Die Durchführung dieser Schritte dauert etwa 30 Minuten.

## <a name="step-1:-create-the-visual-studio-project,-the-template-file,-and-the-parameters-file"></a>Schritt 1: Erstellen des Visual Studio-Projekts, der Vorlagendatei und der Parameterdatei
### <a name="create-the-template-file"></a>Erstellen der Vorlagendatei
Eine Azure Resource Manager-Vorlage ermöglicht Ihnen das gemeinsame Bereitstellen und Verwalten von mehreren Azure-Ressourcen. Die Vorlage ist eine JSON-Beschreibung der Ressourcen sowie der zugehörigen Bereitstellungsparameter.

Führen Sie in Visual Studio folgende Schritte aus:

1. Klicken Sie auf **Datei** > **Neu** > **Projekt**.
2. Wählen Sie unter **Vorlagen** > **Visual C#** die Option **Konsolenanwendung** aus, geben Sie Name und Speicherort des Projekts ein, und klicken Sie dann auf **OK**.
3. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen, und klicken Sie dann auf **Hinzufügen** > **Neues Element**.
4. Klicken Sie auf „Web“, wählen Sie „JSON-Datei“ aus, geben Sie *VirtualMachineTemplate.json* als Namen ein, und klicken Sie dann auf **Hinzufügen**.
5. Fügen Sie zwischen die öffnenden und schließenden Klammern der Datei „VirtualMachineTemplate.json“ das erforderliche Schemaelement und das erforderliche contentVersion-Element ein:
   
        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
        }
6. [Parameter](../resource-group-authoring-templates.md#parameters) sind nicht immer erforderlich, ermöglichen aber die Eingabe von Werten bei der Bereitstellung der Vorlage. Fügen Sie das Parameterelement und seine untergeordneten Elemente nach dem ContentVersion-Element hinzu:
   
        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
        }
7. [Variablen](../resource-group-authoring-templates.md#variables) können in einer Vorlage verwendet werden, um die Werte festzulegen, die sich häufig ändern, oder um die Werte festzulegen, die aus einer Kombination von Parameterwerten erstellt werden müssen. Fügen Sie das Variablenelement nach dem Parameterabschnitt hinzu:
   
        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"  
          },
        }
8. [Ressourcen](../resource-group-authoring-templates.md#resources) wie der virtuelle Computer, das virtuelle Netzwerk und das Speicherkonto werden als Nächstes in der Vorlage definiert. Fügen Sie nach dem Variablenabschnitt den Ressourcenabschnitt hinzu:
   
        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"
          },
          "resources": [
            {
              "type": "Microsoft.Storage/storageAccounts",
              "name": "mystorage1",
              "apiVersion": "2015-06-15",
              "location": "[resourceGroup().location]",
              "properties": { "accountType": "Standard_LRS" }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/publicIPAddresses",
              "name": "myip1",
              "location": "[resourceGroup().location]",
              "properties": {
                "publicIPAllocationMethod": "Dynamic",
                "dnsSettings": { "domainNameLabel": "mydns1" }
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/virtualNetworks",
              "name": "myvnet1",
              "location": "[resourceGroup().location]",
              "properties": {
                "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
                "subnets": [ {
                  "name": "mysn1",
                  "properties": { "addressPrefix": "10.0.0.0/24" }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/networkInterfaces",
              "name": "mync1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/publicIPAddresses/myip1",
                "Microsoft.Network/virtualNetworks/myvn1"
              ],
              "properties": {
                "ipConfigurations": [ {
                  "name": "ipconfig1",
                  "properties": {
                    "privateIPAllocationMethod": "Dynamic",
                    "publicIPAddress": {
                      "id": "[resourceId('Microsoft.Network/publicIPAddresses', 'myip1')]"
                    },
                    "subnet": { "id": "[variables('subnetRef')]" }
                  }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Compute/virtualMachines",
              "name": "myvm1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/networkInterfaces/mync1",
                "Microsoft.Storage/storageAccounts/mystorage1"
              ],
              "properties": {
                "hardwareProfile": { "vmSize": "Standard_A1" },
                "osProfile": {
                  "computerName": "myvm1",
                  "adminUsername": "[parameters('adminUsername')]",
                  "adminPassword": "[parameters('adminPassword')]"
                },
                "storageProfile": {
                  "imageReference": {
                    "publisher": "MicrosoftWindowsServer",
                    "offer": "WindowsServer",
                    "sku": "2012-R2-Datacenter",
                    "version" : "latest"
                  },
                  "osDisk": {
                    "name": "myosdisk1",
                    "vhd": {
                      "uri": "https://mystorage1.blob.core.windows.net/vhds/myosdisk1.vhd"
                    },
                    "caching": "ReadWrite",
                    "createOption": "FromImage"
                  }
                },
                "networkProfile": {
                  "networkInterfaces" : [ {
                    "id": "[resourceId('Microsoft.Network/networkInterfaces','mync1')]"
                  } ]
                }
              }
            } ]
          }
9. Speichern Sie Vorlagendatei, die Sie erstellt haben.

### <a name="create-the-parameters-file"></a>Erstellen der Parameterdatei
Um Werte für die Ressourcenparameter anzugeben, die in der Vorlage definiert wurden, müssen Sie eine Parameterdatei erstellen, die die beim Bereitstellen der Vorlage verwendeten Werte enthält. Führen Sie in Visual Studio folgende Schritte aus:

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen, und klicken Sie dann auf **Hinzufügen** > **Neues Element**.
2. Klicken Sie auf „Web“, wählen Sie „JSON-Datei“ aus, geben Sie *Parameters.json* als Namen ein, und klicken Sie dann auf **Hinzufügen**.
3. Öffnen Sie die Datei „parameters.json“, und fügen Sie den folgenden JSON-Inhalt hinzu:
   
        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "value": "mytestacct1" },
            "adminPassword": { "value": "mytestpass1" }
          }
        }
   
   > [!NOTE]
   > In diesem Artikel wird ein virtueller Computer erstellt, auf dem eine Version des Windows Server-Betriebssystems ausgeführt wird. Weitere Informationen zur Auswahl von anderen Images finden Sie unter [Navigieren zu und Auswählen von Images virtueller Linux-Computer in Azure mithilfe der Befehlszeilenschnittstelle oder PowerShell](virtual-machines-linux-cli-ps-findimage.md).
   > 
   > 
4. Speichern Sie Parameterdatei, die Sie erstellt haben.

## <a name="step-2:-install-the-libraries"></a>Schritt 2: Installieren der Bibliotheken
NuGet-Pakete sind die einfachste Möglichkeit, um die Bibliotheken zu installieren, die Sie zum Abschluss dieses Lernprogramms benötigen. Sie benötigen die Azure-Ressourcenverwaltungsbibliothek und die Azure Active Directory-Authentifizierungsbibliothek, um die Ressourcen zu erstellen. Führen Sie folgende Schritte aus, um diese Bibliotheken in Visual Studio abzurufen:

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen, klicken Sie anschließend auf **NuGet-Pakete verwalten** und dann auf „Durchsuchen“.
2. Geben Sie *Active Directory* in das Suchfeld ein, klicken Sie für das Active Directory-Authentifizierungsbibliothek-Paket auf **Installieren** , und befolgen Sie dann die Anweisungen zum Installieren des Pakets.
3. Wählen Sie am oberen Rand der Seite **Vorabversion einschließen**aus. Geben Sie *Microsoft.Azure.Management.ResourceManager* in das Suchfeld ein, klicken Sie für die Microsoft Azure-Ressourcenverwaltungsbibliotheken auf **Installieren** , und befolgen Sie dann die Anweisungen zum Installieren des Pakets.

Sie können nun die Bibliotheken verwenden, um Ihre Anwendung zu erstellen.

## <a name="step-3:-create-the-credentials-that-are-used-to-authenticate-requests"></a>Schritt 3: Erstellen der Anmeldeinformationen zum Authentifizieren von Anforderungen
Die Azure Active Directory-Anwendung wird erstellt, und die Authentifizierungsbibliothek wird installiert. Formatieren Sie nun die Anwendungsinformationen als Anmeldeinformationen für die Authentifizierung von Anforderungen an Azure Resource Manager.

1. Öffnen Sie die Datei „Program.cs“ für das von Ihnen erstellte Projekt, und fügen Sie dann die folgenden using-Anweisungen am Anfang der Datei hinzu:
   
        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.Azure.Management.ResourceManager.Models;
        using Microsoft.Rest;
        using System.IO;
2. Fügen Sie der Program-Klasse die folgende Methode hinzu, um das Token abzurufen, das zum Erstellen der Anmeldeinformationen benötigt wird:
   
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
3. Fügen Sie der Main-Methode in der Datei „Program.cs“ den folgenden Code hinzu, um damit die Anmeldeinformationen zu erstellen:
   
        var token = GetAccessTokenAsync();
        var credential = new TokenCredentials(token.Result.AccessToken);
4. Speichern Sie die Datei "Program.cs".

## <a name="step-4:-deploy-the-template"></a>Schritt 4: Bereitstellen der Vorlage
In diesem Schritt verwenden Sie die zuvor erstellte Ressourcengruppe. Sie können aber auch mit den Klassen [ResourceGroup](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.models.resourcegroup.aspx) und [ResourceManagementClient](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.resourcemanagementclient.aspx) eine Ressourcengruppe erstellen.

1. Fügen Sie der Main-Methode der Program-Klasse Variablen hinzu, um die Namen der zuvor erstellten Ressourcen, den Namen der Bereitstellung und Ihre Abonnement-ID anzugeben:
   
        var groupName = "resource group name";
        var subscriptionId = "subsciption id";
        var deploymentName = "deployment name";
   
    Ersetzen Sie den Wert von groupName durch den Namen Ihrer Ressourcengruppe. Ersetzen Sie den Wert von deploymentName durch den Namen, den Sie für die Bereitstellung verwenden möchten. Sie erhalten die Abonnement-ID durch Ausführen von Get-AzureRmSubscription.
2. Fügen Sie der Program-Klasse die folgende Methode hinzu, um der Ressourcengruppe mithilfe der von Ihnen definierten Vorlage die Ressourcen bereitzustellen:
   
        public static async Task<DeploymentExtended> CreateTemplateDeploymentAsync(
          TokenCredentials credential,
          string groupName,
          string deploymentName,
          string subscriptionId)
        {
          Console.WriteLine("Creating the template deployment...");
          var deployment = new Deployment();
          deployment.Properties = new DeploymentProperties
          {
            Mode = DeploymentMode.Incremental,
            Template = File.ReadAllText("..\\..\\VirtualMachineTemplate.json"),
            Parameters = File.ReadAllText("..\\..\\Parameters.json")
          };
          var resourceManagementClient = new ResourceManagementClient(credential) 
            { SubscriptionId = subscriptionId };
          return await resourceManagementClient.Deployments.CreateOrUpdateAsync(
            groupName,
            deploymentName,
            deployment);
        }
   
    Wenn Sie die Vorlage aus einem Speicherkonto bereitstellen möchten, können Sie die Template-Eigenschaft durch die TemplateLink-Eigenschaft ersetzen.
3. Fügen Sie der Main-Methode den folgenden Code hinzu, um die gerade hinzugefügte Methode aufzurufen:
   
        var dpResult = CreateTemplateDeploymentAsync(
          credential,
          groupName,
          deploymentName,
          subscriptionId);
        Console.WriteLine(dpResult.Result.Properties.ProvisioningState);
        Console.ReadLine();

## <a name="step-5:-delete-the-resources"></a>Schritt 5: Löschen der Ressourcen
Da in Azure die genutzten Ressourcen in Rechnung gestellt werden, empfiehlt es sich grundsätzlich, nicht mehr benötigte Ressourcen zu löschen. Sie müssen nicht jede Ressource einzeln aus einer Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden alle darin enthaltenen Ressourcen automatisch gelöscht.

1. Fügen Sie der Program-Klasse die folgende Methode hinzu, um die Ressourcengruppe zu löschen:
   
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
2. Fügen Sie der Main-Methode den folgenden Code hinzu, um die gerade hinzugefügte Methode aufzurufen:
   
       DeleteResourceGroupAsync(
         credential,
         groupName,
         subscriptionId);
       Console.ReadLine();

## <a name="step-6:-run-the-console-application"></a>Schritt 6: Ausführen der Konsolenanwendung
1. Klicken Sie zum Ausführen der Konsolenanwendung in Visual Studio auf **Starten** , und melden Sie sich dann bei Azure AD mit denselben Anmeldeinformationen an, die Sie für Ihr Abonnement verwenden.
2. Wenn der Status „Akzeptiert“ angezeigt wird, drücken Sie die **EINGABETASTE** .
   
   Die vollständige Ausführung dieser Konsolenanwendung sollte etwa fünf Minuten dauern. Bevor Sie die EINGABETASTE drücken, um das Löschen der Ressourcen zu starten, können Sie sich ein paar Minuten Zeit nehmen, um die Erstellung der Ressourcen im Azure-Portal zu überprüfen, bevor Sie diese löschen.
3. Navigieren Sie im Azure-Portal zu den Überwachungsprotokollen, um den Status der Ressourcen anzuzeigen:
   
    ![Durchsuchen von Überwachungsprotokollen im Azure-Portal](./media/virtual-machines-windows-csharp-template/crpportal.png)

## <a name="next-steps"></a>Nächste Schritte
* Falls bei der Bereitstellung Probleme aufgetreten sind, sollten Sie den Artikel [Problembehandlung beim Bereitstellen von Ressourcengruppen mit dem Azure-Portal](../resource-manager-troubleshoot-deployments-portal.md) lesen.
* Informationen zur Verwaltung des erstellten virtuellen Computers finden Sie unter [Verwalten von virtuellen Computern mit Azure Resource Manager und PowerShell](virtual-machines-windows-csharp-manage.md).

<!--HONumber=Oct16_HO2-->


