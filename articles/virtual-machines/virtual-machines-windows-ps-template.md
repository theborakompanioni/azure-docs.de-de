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
	ms.date="07/14/2016"
	ms.author="davidmu"/>

# Erstellen Sie einen virtuellen Windows-Computer mit einer Resource Manager-Vorlage

Dieser Artikel bietet eine Einführung in Azure Resource Manager-Vorlagen und zeigt Ihnen, wie Sie sie mithilfe von PowerShell bereitstellen. Mit dieser Vorlage wird ein einzelner virtueller Computer mit Windows Server in einem neuen virtuellen Netzwerk mit einem einzelnen Subnetz bereitgestellt.

Die Ausführung der Schritte in diesem Artikel dauert ungefähr 20 Minuten.

> [AZURE.IMPORTANT] Wenn der virtuelle Computer einer Verfügbarkeitsgruppe angehören soll, müssen Sie ihn bei seiner Erstellung der Gruppe hinzufügen. Derzeit ist es nicht möglich, einen virtuellen Computer einer Verfügbarkeitsgruppe hinzuzufügen, nachdem er erstellt wurde.

## Schritt 1: Erstellen der Vorlagendatei

Sie können eine eigene Vorlage mithilfe der Informationen unter [Erstellen von Azure Resource Manager-Vorlagen](../resource-group-authoring-templates.md) erstellen. Sie können auch Vorlagen bereitstellen, die für Sie aus den [Azure-Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates/) erstellt wurden.

1. Öffnen Sie Ihren bevorzugten Text-Editor, und kopieren Sie diese JSON-Informationen in eine neue Datei namens *VirtualMachineTemplate.json*:

        {
          "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "newStorageAccountName": {
              "type": "string",
              "metadata": {
                "Description": "The name of the storage account where the VM disk is stored."
              }
            },
            "adminUsername": {
              "type": "string",
              "metadata": {
                "Description": "The name of the administrator account on the VM."
              }
            },
            "adminPassword": {
              "type": "securestring",
              "metadata": {
                "Description": "The administrator account password on the VM."
              }
            },
            "dnsNameForPublicIP": {
              "type": "string",
              "metadata": {
                "Description": "The name of the public IP address used to access the VM."
              }
            }
          },
          "variables": {
            "location": "Central US",
            "imagePublisher": "MicrosoftWindowsServer",
            "imageOffer": "WindowsServer",
            "windowsOSVersion": "2012-R2-Datacenter",
            "OSDiskName": "osdisk1",
            "nicName": "nc1",
            "addressPrefix": "10.0.0.0/16",
            "subnetName": "sn1",
            "subnetPrefix": "10.0.0.0/24",
            "storageAccountType": "Standard_LRS",
            "publicIPAddressName": "ip1",
            "publicIPAddressType": "Dynamic",
            "vmStorageAccountContainerName": "vhds",
            "vmName": "vm1",
            "vmSize": "Standard_A0",
            "virtualNetworkName": "vn1",
            "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]"
          },
          "resources": [
            {
              "type": "Microsoft.Storage/storageAccounts",
              "name": "[parameters('newStorageAccountName')]",
              "apiVersion": "2015-06-15",
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
                  "addressPrefixes": [
                    "[variables('addressPrefix')]"
                  ]
                },
                "subnets": [
                  {
                    "name": "[variables('subnetName')]",
                    "properties": {
                      "addressPrefix": "[variables('subnetPrefix')]"
                    }
                  }
                ]
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
                "ipConfigurations": [
                  {
                    "name": "ipconfig1",
                    "properties": {
                      "privateIPAllocationMethod": "Dynamic",
                      "publicIPAddress": {
                        "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]"
                      },
                      "subnet": {
                        "id": "[variables('subnetRef')]"
                      }
                    }
                  }
                ]
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
                  "computername": "[variables('vmName')]",
                  "adminUsername": "[parameters('adminUsername')]",
                  "adminPassword": "[parameters('adminPassword')]"
                },
                "storageProfile": {
                  "imageReference": {
                    "publisher": "[variables('imagePublisher')]",
                    "offer": "[variables('imageOffer')]",
                    "sku" : "[variables('windowsOSVersion')]",
                    "version":"latest"
                  },
                  "osDisk" : {
                    "name": "osdisk",
                    "vhd": {
                      "uri": "[concat('http://',parameters('newStorageAccountName'),'.blob.core.windows.net/',variables('vmStorageAccountContainerName'),'/',variables('OSDiskName'),'.vhd')]"
                    },
                    "caching": "ReadWrite",
                    "createOption": "FromImage"
                  }
                },
                "networkProfile": {
                  "networkInterfaces": [
                    {
                      "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
                    }
                  ]
                }
              }
            }
          ]
        }
        
    >[AZURE.NOTE] In diesem Artikel wird ein virtueller Computer erstellt, auf dem eine Version des Windows Server-Betriebssystems ausgeführt wird. Weitere Informationen zur Auswahl von anderen Images finden Sie unter [Navigieren zu und Auswählen von Images virtueller Linux-Computer in Azure mithilfe der Befehlszeilenschnittstelle oder PowerShell](virtual-machines-linux-cli-ps-findimage.md).
    
2. Speichern Sie die Vorlagendatei.

## Schritt 2: Erstellen der Parameterdatei

Um Werte für die Ressourcenparameter anzugeben, die in der Vorlage definiert wurden, müssen Sie eine Parameterdatei erstellen, die die Werte enthält und sie mit der Vorlage an den Ressourcen-Manager senden.

1. Kopieren Sie im Text-Editor diese JSON-Informationen in eine neue Datei namens *Parameters.json*:

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

    >[AZURE.NOTE] Weitere Informationen finden Sie unter den [Anforderungen für Benutzernamen und Kennwörter](virtual-machines-windows-faq.md#what-are-the-username-requirements-when-creating-a-vm).

2. Speichern Sie die Parameterdatei.

## Schritt 3: Installieren von Azure PowerShell

Unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) finden Sie Informationen dazu, wie Sie die neueste Version von Azure PowerShell installieren, das gewünschte Abonnement auswählen und sich bei Ihrem Azure-Konto anmelden.

## Schritt 4: Erstellen einer Ressourcengruppe

Alle Ressourcen müssen in einer Ressourcengruppe bereitgestellt werden. Weitere Informationen finden Sie unter [Übersicht über Azure Resource Manager](../resource-group-overview.md).

1. Rufen Sie eine Liste mit den verfügbaren Standorten ab, an denen Ressourcen erstellt werden können.

	    Get-AzureRmLocation | sort DisplayName | Select DisplayName

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

### Schritt 5: Erstellen von Ressourcen mit der Vorlage und Parametern

1. Ersetzen Sie den Wert von **$deployName** durch den Namen der Bereitstellung. Ersetzen Sie den Wert von **$templatePath** durch den Pfad und Namen der Vorlagendatei. Ersetzen Sie den Wert von **$parameterFile** durch den Pfad und Namen der Parameterdatei. Erstellen Sie die Variablen.

        $deployName="deployment name"
        $templatePath = "template path"
        $parameterFile = "parameter file"

2. Stellen Sie die Vorlage bereit.

        New-AzureRmResourceGroupDeployment -ResourceGroupName "davidmurg6" -TemplateFile $templatePath -TemplateParameterFile $parameterFile

    Folgendes sollte angezeigt werden:

        DeploymentName    : VirtualMachineTemplate
        ResourceGroupName : myrg1
        ProvisioningState : Succeeded
        Timestamp         : 4/14/2016 8:11:37 PM
        Mode              : Incremental
        TemplateLink      :
        Parameters        :
                            Name             Type                       Value
                            ===============  =========================  ==========
                            newStorageAccountName  String                     mytestsa1
                            adminUsername    String                     mytestacct1
                            adminPassword    SecureString
                            dnsNameForPublicIP  String                     mytestdns1

        Outputs           :

    >[AZURE.NOTE] Sie können auch Vorlagen und Parameter aus einem Azure-Speicherkonto bereitstellen. Weitere Informationen finden Sie unter [Verwenden von Azure PowerShell mit Azure Storage](../storage-powershell-guide-full.md).

## Nächste Schritte

- Falls bei der Bereitstellung Probleme aufgetreten sind, empfiehlt sich unter Umständen die Lektüre des Artikels [Problembehandlung beim Bereitstellen von Ressourcengruppen mit dem Azure-Portal](../resource-manager-troubleshoot-deployments-portal.md).
- Informationen zum Verwalten des soeben erstellten virtuellen Computers finden Sie unter [Verwalten von virtuellen Computern mit Azure Resource Manager und PowerShell](virtual-machines-windows-ps-manage.md).

<!---HONumber=AcomDC_0921_2016-->