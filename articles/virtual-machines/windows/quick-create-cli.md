---
title: "Azure-Schnellstart – Erstellen einer Windows-VM mit der CLI | Microsoft-Dokumentation"
description: Hier lernen Sie schnell, virtuelle Windows-Computer mit der Azure CLI zu erstellen.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 04/03/2017
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: 303cb9950f46916fbdd58762acd1608c925c1328
ms.openlocfilehash: d131a13d0d347a676c3cff0d668034ffc9373e4c
ms.lasthandoff: 04/04/2017

---

# <a name="create-a-windows-virtual-machine-with-the-azure-cli"></a>Erstellen einer Windows-VM mit der Azure CLI

Die Azure CLI dient zum Erstellen und Verwalten von Azure-Ressourcen über die Befehlszeile oder mit Skripts. In dieser Anleitung wird die Bereitstellung einer Windows Server 2016 ausführenden VM mithilfe der Azure CLI ausführlich beschrieben. Nach Abschluss der Bereitstellung stellen wir eine Verbindung mit dem Server her und installieren IIS.

Bevor Sie beginnen, stellen Sie sicher, dass die Azure CLI installiert wurde. Weitere Informationen finden Sie im [Azure CLI-Installationshandbuch](https://docs.microsoft.com/cli/azure/install-azure-cli).

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-azure"></a>Anmelden an Azure 

Melden Sie sich mit dem Befehl [az login](/cli/azure/#login) bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm.

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit [az group create](/cli/azure/group#create) eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. 

Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen `myResourceGroup` am Standort `westeurope` erstellt.

```azurecli
az group create --name myResourceGroup --location westeurope
```

## <a name="create-virtual-machine"></a>Erstellen eines virtuellen Computers

Erstellen Sie mit [az vm create](/cli/azure/vm#create) einen virtuellen Computer. 

Im folgenden Beispiel wird ein virtueller Computer namens `myVM` erstellt. Dieses Beispiel verwendet `azureuser` als Administratorbenutzernamen und ` myPassword12` als Kennwort. Aktualisieren Sie diese Werte auf andere, für Ihre Umgebung geeignete Werte. Diese Werte werden benötigt, wenn eine Verbindung mit dem virtuellen Computer hergestellt wird.

```azurecli
az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter --admin-username azureuser --admin-password myPassword12
```

Nach dem Erstellen der VM zeigt die Azure CLI ähnliche Informationen wie im folgenden Beispiel an. Notieren Sie sich die öffentliche IP-Adresse. Diese Adresse wird verwendet, um auf den virtuellen Computer zuzugreifen.

```azurecli
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "westeurope",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="open-port-80-for-web-traffic"></a>Öffnen von Port 80 für Webdatenverkehr 

Standardmäßig sind für in Azure bereitgestellte virtuelle Windows-Computer nur eingehende RDP-Verbindungen zulässig. Wenn dieser virtuelle Computer als Webserver fungieren soll, muss Port 80 für Datenverkehr aus dem Internet geöffnet werden.  Der gewünschte Port kann mit einem einzelnen Befehl geöffnet werden.  
 
 ```azurecli 
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```


## <a name="connect-to-virtual-machine"></a>Herstellen der Verbindung mit dem virtuellen Computer

Erstellen Sie mit dem folgenden Befehl eine Remotedesktopsitzung mit dem virtuellen Computer. Ersetzen Sie die IP-Adresse mit der öffentlichen IP-Adresse des virtuellen Computers. Geben Sie nach Aufforderung die beim Erstellen des virtuellen Computers verwendeten Anmeldeinformationen ein.

```bash 
mstsc /v:<Public IP Address>
```

## <a name="install-iis-using-powershell"></a>Installieren von IIS mithilfe von PowerShell

Nach der Anmeldung bei dem virtuellen Computer können Sie mit einer einzelnen PowerShell-Codezeile IIS installieren und die lokale Firewallregel aktivieren, um Webdatenverkehr zuzulassen.  Öffnen Sie eine PowerShell-Eingabeaufforderung, und führen Sie den folgenden Befehl aus:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-the-iis-welcome-page"></a>Anzeigen der IIS-Willkommensseite

Nachdem Sie IIS installiert und Port 80 auf Ihrem virtuellen Computer für den Zugriff über das Internet geöffnet haben, können Sie nun mit einem Webbrowser Ihrer Wahl die IIS-Standardwillkommensseite anzeigen. Verwenden Sie dabei den weiter oben dokumentierten Wert von `publicIpAddress`, um die Standardseite zu besuchen. 

![IIS-Standardwebsite](./media/quick-create-powershell/default-iis-website.png) 
## <a name="delete-virtual-machine"></a>Löschen des virtuellen Computers

Wenn Ressourcengruppe, VM und alle zugehörigen Ressourcen nicht mehr benötigt werden, können Sie sie mit dem folgenden Befehl entfernen.

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

[Experimentieren mit der Installation einer Rolle auf der Windows-VM](hero-role.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Azure CLI-Beispiele für Windows-VMs](cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
