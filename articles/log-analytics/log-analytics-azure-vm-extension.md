---
title: Verbinden von virtuellen Azure-Computern mit Log Analytics | Microsoft Docs
description: "Für Windows- und Linux-VMs sollte die Azure-VM-Erweiterung Log Analytics für erfasste Protokolle und Metriken installiert werden. Sie können das Azure-Portal oder PowerShell zum Installieren der VM-Erweiterung Log Analytics auf Azure-VMs verwenden."
services: log-analytics
documentationcenter: 
author: richrundmsft
manager: jochan
editor: 
ms.assetid: ca39e586-a6af-42fe-862e-80978a58d9b1
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2016
ms.author: richrund
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: a0c8af30fbed064001c3fd393bf0440aa1cb2835
ms.openlocfilehash: 3bb103a8def2e1c56695169568c2d3c64b7f291f
ms.lasthandoff: 02/28/2017


---
# <a name="connect-azure-virtual-machines-to-log-analytics-with-a-log-analytics-agent"></a>Verbinden von virtuellen Azure-Computern mit Log Analytics mithilfe eines Log Analytics-Agents

Die empfohlene Methode zum Erfassen von Protokollen und Metriken für Windows- und Linux-Computer ist die Installation des Log Analytics-Agents.

Die einfachste Möglichkeit für die Installation des Log Analytics-Agents auf virtuellen Azure-Computern stellt die Log Analytics-VM-Erweiterung dar.  Die Verwendung der Erweiterung vereinfacht den Installationsvorgang. Außerdem wird der Agent zum Senden von Daten an den angegebenen Log Analytics-Arbeitsbereich automatisch konfiguriert. Der Agent wird auch automatisch aktualisiert, damit Sie immer über die neuesten Features und Fixes verfügen.

Auf virtuellen Windows-Computern aktivieren Sie die VM-Erweiterung *Microsoft Monitoring Agent*.
Auf virtuellen Linux-Computern aktivieren Sie die VM-Erweiterung *OMS-Agent für Linux*.

Erfahren Sie mehr über [Azure VM-Erweiterungen](../virtual-machines/virtual-machines-windows-extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) und den [Linux-Agent](../virtual-machines/virtual-machines-linux-agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Wenn die Erfassung von Protokolldaten auf Agentbasis erfolgt, müssen Sie [Datenquellen in Log Analytics](log-analytics-data-sources.md) konfigurieren, um die zu erfassenden Protokolle und Metriken anzugeben.

> [!IMPORTANT]
> Wenn Sie Log Analytics so konfiguriert haben, dass Protokolldaten mithilfe von [Azure-Diagnose](log-analytics-azure-storage.md) indiziert werden, und Sie den Agent für das Erfassen derselben Protokolle konfigurieren, werden diese Protokolle doppelt erfasst. Ihnen werden dann beide Datenquellen in Rechnung gestellt. Wenn der Agent installiert ist, sollten Sie die Protokolldaten ausschließlich mit dem Agent sammeln. Konfigurieren Sie in diesem Fall nicht Log Analytics für das Sammeln von Daten aus Azure-Diagnose.
>
>

Es stehen drei einfache Methoden zur Verfügung, um die VM-Erweiterung Log Analytics zu aktivieren:

* Über das Azure-Portal
* Mit Azure PowerShell
* Verwenden einer Azure Resource Manager-Vorlage

## <a name="enable-the-vm-extension-in-the-azure-portal"></a>Aktivieren der VM-Erweiterung im Azure-Portal
Sie können den Agent für Log Analytics installieren und den virtuellen Azure-Computer, auf dem er ausgeführt wird, über das [Azure-Portal](https://portal.azure.com) verbinden.

### <a name="to-install-the-log-analytics-agent-and-connect-the-virtual-machine-to-a-log-analytics-workspace"></a>So installieren Sie den Log Analytics-Agent und verbinden die VM mit einem Log Analytics-Arbeitsbereich
1. Melden Sie sich beim [Azure-Portal](http://portal.azure.com)an.
2. Wählen Sie auf der linken Seite des Portals **Durchsuchen** und anschließend **Log Analytics (OMS)** aus.
3. Wählen Sie in der Liste der Log Analytics-Arbeitsbereiche den Arbeitsbereich aus, den Sie mit dem virtuelle Azure-Computer verwenden möchten.  
   ![OMS-Arbeitsbereiche](./media/log-analytics-azure-vm-extension/oms-connect-azure-01.png)
4. Wählen Sie unter **Log Analytics-Management** die Option **Virtuelle Computer** aus.  
   ![Virtuelle Computer](./media/log-analytics-azure-vm-extension/oms-connect-azure-02.png)
5. Wählen Sie in der Liste **Virtuelle Computer** den virtuellen Computer aus, auf dem Sie den Agent installieren möchten. Der **OMS-Verbindungsstatus** für den virtuellen Computer ist **Nicht verbunden**.  
   ![Virtueller Computer nicht verbunden](./media/log-analytics-azure-vm-extension/oms-connect-azure-03.png)
6. Klicken Sie in den Details Ihres virtuellen Computers auf **Verbinden**. Der Agent wird automatisch installiert und für den Log Analytics-Arbeitsbereich konfiguriert. Dieser Vorgang dauert einige Minuten, während dieses Zeitraums lautet der OMS-Verbindungsstatus *Verbindung wird hergestellt...*  
   ![Virtuellen Computer verbinden](./media/log-analytics-azure-vm-extension/oms-connect-azure-04.png)
7. Nachdem der Agent installiert und verbunden wurde, ändert sich der Status der **OMS-Verbindung** in **Dieser Arbeitsbereich**.  
   ![Verbunden](./media/log-analytics-azure-vm-extension/oms-connect-azure-05.png)

## <a name="enable-the-vm-extension-using-powershell"></a>Aktivieren Sie die VM-Erweiterung mithilfe von PowerShell
Es gibt verschiedene Befehle für klassische virtuelle Azure-Computer und virtuelle Resource Manager-Computer. Im Folgenden finden Sie Beispiele für klassische virtuelle Computer und virtuelle Resource Manager-Computer.

Verwenden Sie für klassische virtuelle Computer das folgende PowerShell-Beispiel:

```
Add-AzureAccount

$workspaceId = "enter workspace ID here"
$workspaceKey = "enter workspace key here"
$hostedService = "enter hosted service here"

$vm = Get-AzureVM –ServiceName $hostedService

# For Windows VM uncomment the following line
# Set-AzureVMExtension -VM $vm -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionName 'MicrosoftMonitoringAgent' -Version '1.*' -PublicConfiguration "{'workspaceId': '$workspaceId'}" -PrivateConfiguration "{'workspaceKey': '$workspaceKey' }" | Update-AzureVM -Verbose

# For Linux VM uncomment the following line
# Set-AzureVMExtension -VM $vm -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionName 'OmsAgentForLinux' -Version '1.*' -PublicConfiguration "{'workspaceId': '$workspaceId'}" -PrivateConfiguration "{'workspaceKey': '$workspaceKey' }" | Update-AzureVM -Verbose
```

Verwenden Sie für virtuelle Resource Manager-Computer das folgende PowerShell-Beispiel:

```
Login-AzureRMAccount
Select-AzureSubscription -SubscriptionId "**"

$workspaceName = "your workspace name"
$VMresourcegroup = "**"
$VMresourcename = "**"

$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq $workspaceName})

if ($workspace.Name -ne $workspaceName)
{
    Write-Error "Unable to find OMS Workspace $workspaceName. Do you need to run Select-AzureRMSubscription?"
}

$workspaceId = $workspace.CustomerId
$workspaceKey = (Get-AzureRmOperationalInsightsWorkspaceSharedKeys -ResourceGroupName $workspace.ResourceGroupName -Name $workspace.Name).PrimarySharedKey

$vm = Get-AzureRmVM -ResourceGroupName $VMresourcegroup -Name $VMresourcename
$location = $vm.Location

# For Windows VM uncomment the following line
# Set-AzureRmVMExtension -ResourceGroupName $VMresourcegroup -VMName $VMresourcename -Name 'MicrosoftMonitoringAgent' -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionType 'MicrosoftMonitoringAgent' -TypeHandlerVersion '1.0' -Location $location -SettingString "{'workspaceId': '$workspaceId'}" -ProtectedSettingString "{'workspaceKey': '$workspaceKey'}"

# For Linux VM uncomment the following line
# Set-AzureRmVMExtension -ResourceGroupName $VMresourcegroup -VMName $VMresourcename -Name 'OmsAgentForLinux' -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionType 'OmsAgentForLinux' -TypeHandlerVersion '1.0' -Location $location -SettingString "{'workspaceId': '$workspaceId'}" -ProtectedSettingString "{'workspaceKey': '$workspaceKey'}"


```
Bei der Konfiguration der virtuellen Computer mit PowerShell müssen Sie die **Arbeitsbereichs-ID** und einen **Primärschlüssel** angeben. Sie finden Ihre Arbeitsbereichs-ID und den Primärschlüssel auf der Seite **Einstellungen** im OMS-Portal oder indem Sie PowerShell wie im Beispiel oben gezeigt verwenden.

![Arbeitsbereichs-ID und Primärschlüssel](./media/log-analytics-azure-vm-extension/oms-analyze-azure-sources.png)

## <a name="deploy-the-vm-extension-using-a-template"></a>Bereitstellen der VM-Erweiterung mithilfe einer Vorlage
Mit dem Azure Resource Manager können Sie eine einfache Vorlage (im JSON-Format) erstellen, mit der die Bereitstellung und Konfiguration Ihrer Anwendung definiert wird. Diese Vorlage wird als Ressourcen-Manager-Vorlage bezeichnet und ist eine deklarative Möglichkeit zum Definieren der Bereitstellung. Mit einer Vorlage können Sie die Anwendung während des gesamten App-Lebenszyklus wiederholt bereitstellen und dabei sicher sein, dass Ihre Ressourcen einheitlich bereitgestellt werden.

Wenn Sie den Log Analytics-Agent in Ihre Resource Manager-Vorlage einschließen, können Sie sicherstellen, dass jeder virtuelle Computer zum Berichten an Ihren Log Analytics-Arbeitsbereich vorkonfiguriert wird.

Weitere Informationen zu den Resource Manager-Vorlagen finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md).

Im Folgenden finden Sie ein Beispiel für eine Resource Manager-Vorlage für die Bereitstellung eines virtuellen Computers, auf dem Windows mit installierter Microsoft Monitoring Agent-Erweiterung ausgeführt wird. Diese Vorlage ist eine typische VM-Vorlage mit folgenden Ergänzungen:

* Parameter workspaceId und workspaceName 
* Ressourcenabschnitterweiterung Microsoft.EnterpriseCloud.Monitoring 
* Ausgaben für die Suche nach workspaceId und workspaceSharedKey

```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "adminUsername": {
      "type": "string",
      "metadata": {
        "description": "Username for the Virtual Machine."
      }
    },
    "adminPassword": {
      "type": "securestring",
      "metadata": {
        "description": "Password for the Virtual Machine."
      }
    },
    "dnsLabelPrefix": {
       "type": "string",
       "metadata": {
          "description": "DNS Label for the Public IP. Must be lowercase. It should match with the following regular expression: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$ or it will raise an error."
       }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "OMS workspace ID"
      }
    },
    "workspaceName": {
      "type": "string",
      "metadata": {
         "description": "OMS workspace name"
      }
    },
    "windowsOSVersion": {
      "type": "string",
      "defaultValue": "2012-R2-Datacenter",
      "allowedValues": [
        "2008-R2-SP1",
        "2012-Datacenter",
        "2012-R2-Datacenter",
        "Windows-Server-Technical-Preview"
      ],
      "metadata": {
        "description": "The Windows version for the VM. This will pick a fully patched image of this given Windows version. Allowed values: 2008-R2-SP1, 2012-Datacenter, 2012-R2-Datacenter, Windows-Server-Technical-Preview."
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]",
    "apiVersion": "2015-06-15",
    "imagePublisher": "MicrosoftWindowsServer",
    "imageOffer": "WindowsServer",
    "OSDiskName": "osdiskforwindowssimple",
    "nicName": "myVMNic",
    "addressPrefix": "10.0.0.0/16",
    "subnetName": "Subnet",
    "subnetPrefix": "10.0.0.0/24",
    "storageAccountType": "Standard_LRS",
    "publicIPAddressName": "myPublicIP",
    "publicIPAddressType": "Dynamic",
    "vmStorageAccountContainerName": "vhds",
    "vmName": "MyWindowsVM",
    "vmSize": "Standard_DS1",
    "virtualNetworkName": "MyVNET",
    "resourceId": "[resourceGroup().id]",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "[variables('apiVersion')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "accountType": "[variables('storageAccountType')]"
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIPAddressName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
        "dnsSettings": {
          "domainNameLabel": "[parameters('dnsLabelPrefix')]"
        }
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/virtualNetworks",
      "name": "[variables('virtualNetworkName')]",
      "location": "[resourceGroup().location]",
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
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[variables('nicName')]",
      "location": "[resourceGroup().location]",
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
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[variables('vmName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
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
            "sku": "[parameters('windowsOSVersion')]",
            "version": "latest"
          },
          "osDisk": {
            "name": "osdisk",
            "vhd": {
              "uri": "[concat('http://',variables('storageAccountName'),'.blob.core.windows.net/',variables('vmStorageAccountContainerName'),'/',variables('OSDiskName'),'.vhd')]"
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
        },
        "diagnosticsProfile": {
          "bootDiagnostics": {
             "enabled": "true",
             "storageUri": "[concat('http://',variables('storageAccountName'),'.blob.core.windows.net')]"
          }
        }
      },
      "resources": [
        {
          "type": "extensions",
          "name": "Microsoft.EnterpriseCloud.Monitoring",
          "apiVersion": "[variables('apiVersion')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
          ],
          "properties": {
            "publisher": "Microsoft.EnterpriseCloud.Monitoring",
            "type": "MicrosoftMonitoringAgent",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
              "workspaceId": "[parameters('workspaceId')]"
            },
            "protectedSettings": {
              "workspaceKey": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-03-20').primarySharedKey]"
            }
          }
        }
      ]
    }
  ],
  "outputs": {
      "sharedKeyOutput": {
         "value": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName')), '2015-03-20').primarySharedKey]",
         "type": "string"
      },
      "workspaceIdOutput": {
         "value": "[reference(concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName')), '2015-03-20').customerId]",
        "type" : "string"
      }
  }
}
```

Sie können eine Vorlage mit dem folgenden PowerShell-Befehl bereitstellen:

```
New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath
```

## <a name="troubleshooting-windows-virtual-machines"></a>Behandeln von Problemen bei virtuellen Windows-Computern
Wenn die VM-Agent-Erweiterung *Microsoft Monitoring Agent* nicht installiert ist oder keine Berichte erstellt, können Sie die folgenden Schritte ausführen, um das Problem zu beheben.

1. Überprüfen Sie, ob der Azure-VM-Agent installiert ist und ordnungsgemäß funktioniert, indem Sie die Schritte unter [KB 2965986](https://support.microsoft.com/kb/2965986#mt1) ausführen.
   * Sie können auch die Protokolldatei `C:\WindowsAzure\logs\WaAppAgent.log` des VM-Agents überprüfen.
   * Wenn das Protokoll nicht vorhanden ist, wurde der VM-Agent nicht installiert.
     * [Installieren des Azure-VM-Agents auf klassischen VMs](../virtual-machines/virtual-machines-windows-classic-agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
2. Vergewissern Sie sich anhand der folgenden Schritte, dass die Taktaufgabe der Microsoft Monitoring Agent-Erweiterung ausgeführt wird:
   * Melden Sie sich beim virtuellen Computer an.
   * Öffnen Sie den Taskplaner, und suchen Sie die Aufgabe `update_azureoperationalinsight_agent_heartbeat`.
   * Vergewissern Sie sich, dass die Aufgabe aktiviert ist und jede Minute ausgeführt wird.
   * Überprüfen Sie die Taktprotokolldatei in `C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\heartbeat.log`.
3. Überprüfen Sie die Protokolldateien der Microsoft Monitoring Agent-VM-Erweiterung in `C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent`.
4. Stellen Sie sicher, dass der virtuelle Computer PowerShell-Skripts ausführen kann.
5. Vergewissern Sie sich, dass die Berechtigungen für „C:\Windows\temp“ nicht geändert wurden.
6. Zeigen Sie den Status des Microsoft Monitoring Agents an, indem Sie Folgendes in einem PowerShell-Fenster mit erhöhten Rechten auf dem virtuellen Computer eingeben: `  (New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg').GetCloudWorkspaces() | Format-List`.
7. Überprüfen Sie die Setupprotokolldateien des Microsoft Monitoring Agents in `C:\Windows\System32\config\systemprofile\AppData\Local\SCOM\Logs`.

Weitere Informationen finden Sie unter [Behandeln von Problemen bei Windows-Erweiterungen](../virtual-machines/virtual-machines-windows-extensions-troubleshoot.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="troubleshooting-linux-virtual-machines"></a>Behandeln von Problemen bei virtuellen Linux-Computern
Wenn die VM-Agent-Erweiterung *OMS-Agent für Linux* nicht installiert ist oder keine Berichte erstellt, können Sie die folgenden Schritte ausführen, um das Problem zu beheben.

1. Wenn der Status der Erweiterung *Unbekannt* lautet, überprüfen Sie, ob der Azure-VM-Agent installiert ist und ordnungsgemäß funktioniert. Dazu überprüfen Sie die Protokolldatei des VM-Agents: `/var/log/waagent.log`.
   * Wenn das Protokoll nicht vorhanden ist, wurde der VM-Agent nicht installiert.
   * [Installieren des Azure-VM-Agents auf Linux-VMs](../virtual-machines/virtual-machines-linux-agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
2. Bei einem anderen fehlerhaften Status überprüfen Sie die Protokolldateien der VM-Erweiterung OMS-Agent für Linux in `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/extension.log` und `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/CommandExecution.log`.
3. Wenn der Status der Erweiterung fehlerfrei ist, aber keine Daten hochgeladen werden, überprüfen Sie Protokolldateien des OMS-Agents für Linux in `/var/opt/microsoft/omsagent/log/omsagent.log`.

Weitere Informationen finden Sie unter [Behandeln von Problemen bei Linux-Erweiterungen](../virtual-machines/virtual-machines-linux-extensions-troubleshoot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="next-steps"></a>Nächste Schritte
* Konfigurieren Sie [Datenquellen in Log Analytics](log-analytics-data-sources.md) , um die zu sammelnden Protokolle und Metriken anzugeben.
* Zum Sammeln von Daten von VMs [fügen Sie Log Analytics-Lösungen aus dem Lösungskatalog hinzu](log-analytics-add-solutions.md).
* [Sammeln Sie mithilfe von Azure-Diagnose Daten](log-analytics-azure-storage.md) für andere Ressourcen, die in Azure ausgeführt werden.

Für Computer, die nicht in Azure ausgeführt werden, können Sie den Log Analytics-Agent mithilfe der in den folgenden Artikeln beschriebenen Methoden installieren:

* [Verbinden von Windows-Computern mit Log Analytics](log-analytics-windows-agents.md)
* [Verbinden von Linux-Computern mit Log Analytics](log-analytics-linux-agents.md)

