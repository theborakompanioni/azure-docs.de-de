<properties
    pageTitle="Erstellen eines virtuellen Computers mit einer Resource Manager-Vorlage | Microsoft Azure"
    description="Verwenden Sie eine Resource Manager-Vorlage und PowerShell, um ganz einfach einen neuen virtuellen Windows-Computer zu erstellen."
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
    ms.date="10/06/2016"
    ms.author="davidmu"/>


# <a name="create-a-windows-virtual-machine-with-a-resource-manager-template"></a>Erstellen Sie einen virtuellen Windows-Computer mit einer Resource Manager-Vorlage

Dieser Artikel bietet eine Einführung in Azure Resource Manager-Vorlagen und zeigt Ihnen, wie Sie sie mithilfe von PowerShell bereitstellen. Mit dieser Vorlage wird ein einzelner virtueller Computer mit Windows Server in einem neuen virtuellen Netzwerk mit einem einzelnen Subnetz bereitgestellt.

Die Ausführung der Schritte in diesem Artikel dauert ungefähr 20 Minuten.

> [AZURE.IMPORTANT] Wenn der virtuelle Computer einer Verfügbarkeitsgruppe angehören soll, fügen Sie ihn bei seiner Erstellung der Gruppe hinzu. Derzeit ist es nicht möglich, einen virtuellen Computer einer Verfügbarkeitsgruppe hinzuzufügen, nachdem er erstellt wurde.

## <a name="step-1:-create-the-template-file"></a>Schritt 1: Erstellen der Vorlagendatei

Sie können eine eigene Vorlage mithilfe der Informationen unter [Erstellen von Azure Resource Manager-Vorlagen](../resource-group-authoring-templates.md)erstellen. Sie können auch Vorlagen bereitstellen, die für Sie aus den [Azure-Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates/)erstellt wurden.

1. Öffnen Sie Ihren bevorzugten Text-Editor, und fügen Sie das erforderliche schema-Element und das erforderliche contentVersion-Element ein:

        {
          "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
        }
2. [Parameter](../resource-group-authoring-templates.md#parameters) sind nicht immer erforderlich, ermöglichen aber die Eingabe von Werten, wenn die Vorlage bereitgestellt wird. Fügen Sie das Parameterelement und seine untergeordneten Elemente nach dem ContentVersion-Element hinzu:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
        }

3. [Variablen](../resource-group-authoring-templates.md#variables) können in einer Vorlage verwendet werden, um die Werte festzulegen, die sich häufig ändern, oder um die Werte festzulegen, die aus einer Kombination von Parameterwerten erstellt werden müssen. Fügen Sie das Variablenelement nach dem Parameterabschnitt hinzu:

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
        
4. [Ressourcen](../resource-group-authoring-templates.md#resources) wie der virtuelle Computer, das virtuelle Netzwerk und das Speicherkonto werden als Nächstes in der Vorlage definiert. Fügen Sie nach dem Variablenabschnitt den Ressourcenabschnitt hinzu:

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
          
    >[AZURE.NOTE] In diesem Artikel wird ein virtueller Computer erstellt, auf dem eine Version des Windows Server-Betriebssystems ausgeführt wird. Weitere Informationen zur Auswahl von anderen Images finden Sie unter [Navigieren zu und Auswählen von Images virtueller Linux-Computer in Azure mithilfe der Befehlszeilenschnittstelle oder PowerShell](virtual-machines-linux-cli-ps-findimage.md).  
            
2. Speichern Sie die Vorlagendatei als *VirtualMachineTemplate.json*.

## <a name="step-2:-create-the-parameters-file"></a>Schritt 2: Erstellen der Parameterdatei

Um Werte für die Ressourcenparameter anzugeben, die in der Vorlage definiert wurden, müssen Sie eine Parameterdatei erstellen, die die Werte enthält, die beim Bereitstellen der Vorlage verwendet werden.

1. Kopieren Sie im Text-Editor diesen JSON-Inhalt in eine neue Datei namens *Parameters.json*:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "value": "mytestacct1" },
            "adminPassword": { "value": "mytestpass1" }
          }
        }

    >[AZURE.NOTE] Weitere Informationen finden Sie unter den [Anforderungen für Benutzernamen und Kennwörter](virtual-machines-windows-faq.md#what-are-the-username-requirements-when-creating-a-vm).

2. Speichern Sie die Parameterdatei.

## <a name="step-3:-install-azure-powershell"></a>Schritt 3: Installieren von Azure PowerShell

Unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) erfahren Sie, wie Sie die neueste Version von Azure PowerShell installieren, Ihr Abonnement auswählen und sich bei Ihrem Konto anmelden.

## <a name="step-4:-create-a-resource-group"></a>Schritt 4: Erstellen einer Ressourcengruppe

Alle Ressourcen müssen in einer [Ressourcengruppe](../resource-group-overview.md) bereitgestellt werden.

1. Rufen Sie eine Liste mit den verfügbaren Standorten ab, an denen Ressourcen erstellt werden können.

        Get-AzureRmLocation | sort DisplayName | Select DisplayName

2. Ersetzen Sie den Wert von **$locName** durch einen Standort aus der Liste, z.B. **USA, Mitte**. Erstellen Sie die Variable.

        $locName = "location name"
        
3. Ersetzen Sie den Wert von **$rgName** durch den Namen der neuen Ressourcengruppe. Erstellen Sie die Variable und die Ressourcengruppe.

        $rgName = "resource group name"
        New-AzureRmResourceGroup -Name $rgName -Location $locName
        
    Die Ausgabe sollte in etwa wie das folgende Beispiel aussehen:
    
        ResourceGroupName : myrg1
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/{subscription-id}/resourceGroups/myrg1

## <a name="step-5:-create-the-resources-with-the-template-and-parameters"></a>Schritt 5: Erstellen von Ressourcen mit der Vorlage und Parametern

Ersetzen Sie den Wert von **$templateFile** durch den Pfad und Namen der Vorlagendatei. Ersetzen Sie den Wert von **$parameterFile** durch den Pfad und Namen der Parameterdatei. Erstellen Sie die Variablen, und stellen Sie anschließend die Vorlage bereit. 

        $templateFile = "template file"
        $parameterFile = "parameter file"
        New-AzureRmResourceGroupDeployment -ResourceGroupName $rgName -TemplateFile $templateFile -TemplateParameterFile $parameterFile

    You will see something like this:

        DeploymentName    : VirtualMachineTemplate
        ResourceGroupName : myrg1
        ProvisioningState : Succeeded
        Timestamp         : 4/14/2016 8:11:37 PM
        Mode              : Incremental
        TemplateLink      :
        Parameters        :
                            Name             Type                       Value
                            ===============  =========================  ==========
                            adminUsername    String                     mytestacct1
                            adminPassword    SecureString

        Outputs           :

>[AZURE.NOTE] Sie können auch Vorlagen und Parameter aus einem Azure-Speicherkonto bereitstellen. Weitere Informationen finden Sie unter [Verwenden von Azure PowerShell mit Azure Storage](../storage/storage-powershell-guide-full.md).

## <a name="next-steps"></a>Nächste Schritte

- Falls bei der Bereitstellung Probleme aufgetreten sind, empfiehlt sich unter Umständen die Lektüre des Artikels [Anzeigen von Bereitstellungsvorgängen mit dem Azure-Portal](../resource-manager-troubleshoot-deployments-portal.md).
- Informationen zur Verwaltung des erstellten virtuellen Computers finden Sie unter [Verwalten von virtuellen Azure-Computern mit Resource Manager und PowerShell](virtual-machines-windows-ps-manage.md).



<!--HONumber=Oct16_HO2-->


