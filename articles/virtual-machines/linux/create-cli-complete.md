---
title: Erstellen einer Linux-Umgebung mit der Azure CLI 2.0 | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie Speicher, einen virtuellen Linux-Computer, ein virtuelles Netzwerk mitsamt Subnetz, einen Load Balancer, eine Netzwerkschnittstellenkarte, eine öffentliche IP-Adresse und eine Netzwerksicherheitsgruppe über die Azure CLI 2.0 von Grund auf neu erstellen."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4ba4060b-ce95-4747-a735-1d7c68597a1a
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/07/2017
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 6552d590786859829597614d4b984f7798e38c94
ms.lasthandoff: 04/03/2017


---
# <a name="create-a-complete-linux-environment-with-the-azure-cli-20"></a>Erstellen einer vollständigen Linux-Umgebung mit der Azure CLI 2.0
In diesem Artikel erstellen wir ein einfaches Netzwerk mit einem Load Balancer und zwei virtuellen Computern, die zu Entwicklungszwecken und für einfache Berechnungen nützlich sind. Wir führen Sie Befehl für Befehl durch den Prozess, bis Sie über zwei funktionierende, sichere virtuelle Linux-Computer verfügen, mit denen Sie über das Internet von jedem Ort aus eine Verbindung herstellen können. Anschließend können Sie sich mit komplexeren Netzwerken und Umgebungen beschäftigen. In diesem Artikel wird erläutert, wie Sie die Umgebung mit der Azure CLI-2.0-erstellen. Sie können diese Schritte auch mit der [Azure CLI 1.0](create-cli-complete-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ausführen.

Im Laufe dieses Vorgangs lernen Sie die Abhängigkeitshierarchie kennen, die Sie mit dem Resource Manager-Bereitstellungsmodell erhalten, sowie die damit verbundene hohe Leistungsfähigkeit. Wenn Sie wissen, wie das System aufgebaut ist, können Sie es mithilfe von [Azure Resource Manager-Vorlagen](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)deutlich schneller neu erstellen. Und wenn Sie wissen, wie die einzelnen Komponenten Ihrer Umgebung zusammenwirken, ist die Erstellung von Vorlagen für deren Automatisierung viel einfacher.

Die Umgebung enthält:

* Zwei virtuelle Computer in einer Verfügbarkeitsgruppe
* Load Balancer mit Lastenausgleichsregel an Port 80
* Regeln für Netzwerksicherheitsgruppen (NSG) zum Schutz der virtuellen Computer vor unerwünschtem Datenverkehr

![Grundlegende Umgebung – Übersicht](./media/create-cli-complete/environment_overview.png)

## <a name="quick-commands"></a>Schnellbefehle
Im folgenden Abschnitt werden die grundlegenden Befehlen zum Hochladen eines virtuellen Computers in Azure beschrieben, falls Sie die Aufgabe schnell durchführen müssen. Ausführlichere Informationen und Kontext für die einzelnen Schritte finden Sie im übrigen Dokument ([ab hier](#detailed-walkthrough)).

Ersetzen Sie in den folgenden Beispielen die Beispielparameternamen durch Ihre eigenen Werte. Als Beispielparameternamen werden `myResourceGroup`, `mystorageaccount` und `myVM` verwendet.

Zum Erstellen dieser benutzerdefinierten Umgebung benötigen Sie die installierte neueste Version [Azure CLI 2.0](/cli/azure/install-az-cli2) und müssen mithilfe von [az login](/cli/azure/#login) bei einem Azure-Konto angemeldet sein.

Erstellen Sie zunächst die Ressourcengruppe mithilfe von [az group create](/cli/azure/group#create). Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen `myResourceGroup` am Standort `westeurope`:

```azurecli
az group create --name myResourceGroup --location westeurope
```

Der nächste Schritt ist optional. Die Standardaktion beim Erstellen eines virtuellen Computers über die Azure CLI 2.0 ist die Verwendung von Azure Managed Disks. Weitere Informationen zu Azure Managed Disks finden Sie in der [Übersicht über Managed Disks](../../storage/storage-managed-disks-overview.md). Wenn Sie stattdessen nicht verwaltete Datenträger verwenden möchten, müssen Sie mithilfe von [az storage account create](/cli/azure/storage/account#create) ein Speicherkonto erstellen. Im folgenden Beispiel wird ein Speicherkonto mit dem Namen `mystorageaccount` erstellt. (Der Name des Speicherkontos muss eindeutig sein, geben Sie daher einen eigenen eindeutigen Namen an.)

```azurecli
az storage account create --resource-group myResourceGroup --location westeurope \
  --name mystorageaccount --kind Storage --sku Standard_LRS
```

Erstellen Sie das virtuelle Netzwerk mit [az network vnet create](/cli/azure/network/vnet#create). Im folgenden Beispiel wird ein virtuelles Netzwerk mit dem Namen `myVnet` und ein Subnetz mit dem Namen `mySubnet` erstellt:

```azurecli
az network vnet create --resource-group myResourceGroup --location westeurope --name myVnet \
  --address-prefix 192.168.0.0/16 --subnet-name mySubnet --subnet-prefix 192.168.1.0/24
```

Erstellen Sie mit [az network public-ip create](/cli/azure/network/public-ip#create) eine öffentliche IP-Adresse. Im folgenden Beispiel wird eine öffentliche IP-Adresse namens `myPublicIP` mit dem DNS-Namen `mypublicdns` erstellt. (Der DNS-Name muss eindeutig sein. Geben Sie daher einen eigenen eindeutigen Namen an.)

```azurecli
az network public-ip create --resource-group myResourceGroup --location westeurope \
  --name myPublicIP --dns-name mypublicdns --allocation-method static --idle-timeout 4
```

Erstellen Sie mit [az network lb create](/cli/azure/network/lb#create) den Load Balancer. Das folgende Beispiel:

- erstellt einen Load Balancer mit dem Namen `myLoadBalancer`
- ordnet die öffentliche IP-Adresse zu `myPublicIP`
- erstellt einen Front-End-IP-Pool mit dem Namen `mySubnetPool`
- erstellt einen Back-End-IP-Pool mit dem Namen `myBackEndPool`

```azurecli
az network lb create --resource-group myResourceGroup --location westeurope \
  --name myLoadBalancer --public-ip-address myPublicIP \
  --frontend-ip-name myFrontEndPool --backend-pool-name myBackEndPool
```

Erstellen Sie mithilfe von [az network lb inbound-nat-rule create](/cli/azure/network/lb/inbound-nat-rule#create) Regeln für die eingehende SSH-Netzwerkadressenübersetzung (Network Address Translation, NAT) für den Load Balancer. Im folgenden Beispiel werden zwei Load Balancer-Regeln (`myLoadBalancerRuleSSH1` und `myLoadBalancerRuleSSH2`) erstellt:

```azurecli
az network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH1 --protocol tcp \
  --frontend-port 4222 --backend-port 22 --frontend-ip-name myFrontEndPool
az network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH2 --protocol tcp \
  --frontend-port 4223 --backend-port 22 --frontend-ip-name myFrontEndPool
```

Erstellen Sie mit [az network lb probe create](/cli/azure/network/lb/probe#create) den Integritätstest für den Load Balancer. Im folgenden Beispiel wird ein TCP-Test namens `myHealthProbe` erstellt:

```azurecli
az network lb probe create --resource-group myResourceGroup --lb-name myLoadBalancer \
  --name myHealthProbe --protocol tcp --port 80 --interval 15 --threshold 4
```

Erstellen Sie die eingehenden Web-NAT-Regeln für den Load Balancer mit [az network lb rule create](/cli/azure/network/lb/rule#create). Im folgenden Beispiel wird eine Load Balancer-Regel mit dem Namen `myLoadBalancerRuleWeb` erstellt und dem Test `myHealthProbe` zugeordnet:

```azurecli
az network lb rule create --resource-group myResourceGroup --lb-name myLoadBalancer \
  --name myLoadBalancerRuleWeb --protocol tcp --frontend-port 80 --backend-port 80 \
  --frontend-ip-name myFrontEndPool --backend-pool-name myBackEndPool \
  --probe-name myHealthProbe
```

Überprüfen Sie den Load Balancer, die IP-Pools und die NAT-Regeln mit [az network lb show](/cli/azure/network/lb#show):

```azurecli
az network lb show --resource-group myResourceGroup --name myLoadBalancer
```

Erstellen Sie die Netzwerksicherheitsgruppe mit [az network nsg create](/cli/azure/network/nsg#create). Im folgenden Beispiel wird eine Netzwerksicherheitsgruppe namens `myNetworkSecurityGroup` erstellt:

```azurecli
az network nsg create --resource-group myResourceGroup --location westeurope \
  --name myNetworkSecurityGroup
```

Fügen Sie mithilfe von [az network nsg rule create](/cli/azure/network/nsg/rule#create) zwei eingehende Regeln für die Netzwerksicherheitsgruppe hinzu. Im folgenden Beispiel werden zwei Regeln (`myNetworkSecurityGroupRuleSSH` und `myNetworkSecurityGroupRuleHTTP`) erstellt:

```azurecli
az network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --name myNetworkSecurityGroupRuleSSH \
  --protocol tcp --direction inbound --priority 1000 --source-address-prefix '*' \
  --source-port-range '*' --destination-address-prefix '*' --destination-port-range 22 \
  --access allow
az network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --name myNetworkSecurityGroupRuleHTTP \
  --protocol tcp --direction inbound --priority 1001 --source-address-prefix '*' \
  --source-port-range '*' --destination-address-prefix '*' --destination-port-range 80 \
  --access allow
```

Erstellen Sie die erste Netzwerkkarte (Network Interface Card, NIC) mit [az network nic create](/cli/azure/network/nic#create). Im folgenden Beispiel wird eine NIC mit dem Namen `myNic1` erstellt und dem Load Balancer `myLoadBalancer` und den passenden Pools sowie der `myNetworkSecurityGroup` zugeordnet:

```azurecli
az network nic create --resource-group myResourceGroup --location westeurope --name myNic1 \
  --vnet-name myVnet --subnet mySubnet --network-security-group myNetworkSecurityGroup \
  --lb-name myLoadBalancer --lb-address-pools myBackEndPool \
  --lb-inbound-nat-rules myLoadBalancerRuleSSH1
```

Erstellen Sie die zweite NIC, wiederum mit **az network nic create**. Im folgenden Beispiel wird eine NIC namens `myNic2` erstellt:

```azurecli
az network nic create --resource-group myResourceGroup --location westeurope --name myNic2 \
  --vnet-name myVnet --subnet mySubnet --network-security-group myNetworkSecurityGroup \
  --lb-name myLoadBalancer --lb-address-pools myBackEndPool \
  --lb-inbound-nat-rules myLoadBalancerRuleSSH2
```

Erstellen Sie mithilfe von [az vm availability-set create](/cli/azure/vm/availability-set#create) die Verfügbarkeitsgruppe. Im folgenden Beispiel wird eine Verfügbarkeitsgruppe namens `myAvailabilitySet` erstellt:

```azurecli
az vm availability-set create --resource-group myResourceGroup --location westeurope \
  --name myAvailabilitySet \
  --platform-fault-domain-count 3 --platform-update-domain-count 2
```

Erstellen Sie den ersten virtuellen Linux-Computer mit [az vm create](/cli/azure/vm#create). Im folgenden Beispiel wird ein virtueller Computer namens `myVM1` erstellt, der Azure Managed Disks verwendet. Wenn Sie nicht verwaltete Datenträger verwenden möchten, finden Sie weitere Informationen im obigen Zusatzhinweis.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM1 \
    --location westeurope \
    --availability-set myAvailabilitySet \
    --nics myNic1 \
    --image UbuntuLTS \
    --ssh-key-value ~/.ssh/id_rsa.pub \
    --admin-username azureuser
```

Wenn Sie Azure Managed Disks verwenden, überspringen Sie diesen Schritt. Wenn Sie nicht verwaltete Datenträger verwenden möchten und in den vorherigen Schritten ein Speicherkonto erstellt haben, müssen Sie dem oben stehenden Befehl einige zusätzliche Parameter hinzufügen. Fügen Sie dem oben stehenden Befehl die folgenden zusätzlichen Parameter hinzu, um die nicht verwalteten Datenträger im Speicherkonto `mystorageaccount` zu erstellen: 

```azurecli
  --use-unmanaged-disk \
  --storage-account mystorageaccount
```

Erstellen Sie den zweiten virtuellen Linux-Computer, wiederum mit **az vm create**. Im folgenden Beispiel wird ein virtueller Computer namens `myVM2` erstellt:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM2 \
    --location westeurope \
    --availability-set myAvailabilitySet \
    --nics myNic2 \
    --image UbuntuLTS \
    --ssh-key-value ~/.ssh/id_rsa.pub \
    --admin-username azureuser
```

Auch hier gilt: Wenn Sie die Standardeinstellung „Azure Managed Disks“ nicht verwenden, fügen Sie dem oben stehenden Befehl die folgenden zusätzlichen Parameter hinzu, um die nicht verwalteten Datenträger im Speicherkonto `mystorageaccount` zu erstellen:

```azurecli
  --use-unmanaged-disk \
  --storage-account mystorageaccount
``` 

Überprüfen Sie mit [az vm show](/cli/azure/vm#show), ob alles ordnungsgemäß erstellt wurde:

```azurecli
az vm show --resource-group myResourceGroup --name myVM1
az vm show --resource-group myResourceGroup --name myVM2
```

Exportieren Sie die neue Umgebung mithilfe von [az group export](/cli/azure/group#export) in eine Vorlage, um schnell neue Instanzen erstellen zu können:

```azurecli
az group export --name myResourceGroup > myResourceGroup.json
```

## <a name="detailed-walkthrough"></a>Ausführliche exemplarische Vorgehensweise
In den folgenden ausführlichen Schritten wird erklärt, was jeder Befehl macht, wenn Sie Ihre Umgebung erstellen. Diese Konzepte helfen Ihnen beim Erstellen Ihrer eigenen benutzerdefinierten Umgebungen für Entwicklung oder Produktion.

Achten Sie darauf, dass Sie die neueste Version [Azure CLI 2.0](/cli/azure/install-az-cli2) installiert haben und mit [az login](/cli/azure/#login) bei einem Azure-Konto angemeldet sind.

Ersetzen Sie in den folgenden Beispielen die Beispielparameternamen durch Ihre eigenen Werte. Als Beispielparameternamen werden `myResourceGroup`, `mystorageaccount` und `myVM` verwendet.

## <a name="create-resource-groups-and-choose-deployment-locations"></a>Erstellen einer Ressourcengruppe und Auswählen von Standorten für die Bereitstellung
Azure-Ressourcengruppen sind logische Bereitstellungsentitäten, die Konfigurationsdaten und Metadaten enthalten, um die logische Verwaltung von Ressourcenbereitstellungen zu ermöglichen. Erstellen Sie die Ressourcengruppe mithilfe von [az group create](/cli/azure/group#create). Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen `myResourceGroup` am Standort `westeurope`:

```azurecli
az group create --name myResourceGroup --location westeurope
```

Standardmäßig erfolgt die Ausgabe in JSON (JavaScript Object Notation). Um beispielsweise eine Ausgabe in Listen- oder Tabellenform zu erhalten, verwenden Sie [az configure --output](/cli/azure/#configure). Ferner können Sie `--output` an beliebige Befehle anfügen, um eine einmalige Änderung des Ausgabeformats zu erreichen. Das folgende Beispiel zeigt die JSON-Ausgabe des Befehls **az group create**:

```json                       
{
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "location": "westeurope",
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="create-a-storage-account"></a>Speicherkonto erstellen
Der nächste Schritt ist optional. Die Standardaktion beim Erstellen eines virtuellen Computers über die Azure CLI 2.0 ist die Verwendung von Azure Managed Disks. Diese Datenträger werden von der Azure-Plattform verarbeitet und erfordern keine Vorbereitung und keinen Speicherort zur Aufbewahrung. Weitere Informationen zu Azure Managed Disks finden Sie in der [Übersicht über Azure Managed Disks](../../storage/storage-managed-disks-overview.md). Fahren Sie mit [Erstellen eines virtuellen Netzwerks und des Subnetzes](#create-a-virtual-network-and-subnet) fort, wenn Sie Azure Managed Disks verwenden möchten. 

Wenn Sie nicht verwaltete Datenträger verwenden möchten, müssen Sie ein Speicherkonto für Ihre VM-Datenträger und für alle zusätzlichen Datenträger erstellen, die Sie hinzufügen möchten.

Hier verwenden wir den Befehl [az storage account create](/cli/azure/storage/account#create) und übergeben den Speicherort des Kontos, die Ressourcengruppe, mit der es gesteuert wird, und den Typ der gewünschten Speicherunterstützung. Das folgende Beispiel erstellt ein Speicherkonto namens `mystorageaccount`:

```azurecli
az storage account create --resource-group myResourceGroup --location westeurope \
  --name mystorageaccount --kind Storage --sku Standard_LRS
```

Ausgabe:

```json
{
  "accessTier": null,
  "creationTime": "2016-12-07T17:59:50.090092+00:00",
  "customDomain": null,
  "encryption": null,
  "id": "/subscriptions/guid/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
  "kind": "Storage",
  "lastGeoFailoverTime": null,
  "location": "westeurope",
  "name": "mystorageaccount",
  "primaryEndpoints": {
    "blob": "https://mystorageaccount.blob.core.windows.net/",
    "file": "https://mystorageaccount.file.core.windows.net/",
    "queue": "https://mystorageaccount.queue.core.windows.net/",
    "table": "https://mystorageaccount.table.core.windows.net/"
  },
  "primaryLocation": "westeurope",
  "provisioningState": "Succeeded",
  "resourceGroup": "myresourcegroup",
  "secondaryEndpoints": null,
  "secondaryLocation": null,
  "sku": {
    "name": "Standard_LRS",
    "tier": "Standard"
  },
  "statusOfPrimary": "Available",
  "statusOfSecondary": null,
  "tags": {},
  "type": "Microsoft.Storage/storageAccounts"
}
```

Um das Speicherkonto mithilfe der Befehlszeilenschnittstelle zu untersuchen, müssen Sie zunächst die Kontonamen und Schlüssel festlegen. Verwenden Sie [az storage account show-connection-string](/cli/azure/storage/account#show-connection-string). Ersetzen Sie den Namen des Speicherkontos im folgenden Beispiel mit einem Namen Ihrer Wahl:

```bash
export AZURE_STORAGE_CONNECTION_STRING="$(az storage account show-connection-string --resource-group myResourceGroup --name mystorageaccount --query connectionString)"
```

Anschließend können Sie Ihre Speicherinformationen mit [az storage container list](/cli/azure/storage/container#list) anzeigen:

```azurecli
az storage container list
```

Ausgabe:

```azurecli
[
  {
    "metadata": null,
    "name": "vhds",
    "properties": {
      "etag": "\"0x8D41F912D472F94\"",
      "lastModified": "2016-12-08T17:39:35+00:00",
      "lease": {
        "duration": null,
        "state": null,
        "status": null
      },
      "leaseDuration": "infinite",
      "leaseState": "leased",
      "leaseStatus": "locked"
    }
  }
]
```

## <a name="create-a-virtual-network-and-subnet"></a>Erstellen eines virtuellen Netzwerks und des Subnetzes
Als Nächstes müssen Sie ein virtuelles Netzwerk erstellen, das in Azure ausgeführt wird, und ein Subnetz, in dem Sie Ihre virtuellen Computer erstellen können. Im folgenden Beispiel wird [az network vnet create](/cli/azure/network/vnet#create) verwendet, um ein virtuelles Netzwerk mit dem Namen `myVnet` mit dem Adresspräfix `192.168.0.0/16` und ein Subnetz mit dem Namen `mySubnet` und dem Subnetzadresspräfix `192.168.1.0/24` zu erstellen:

```azurecli
az network vnet create --resource-group myResourceGroup --location westeurope \
  --name myVnet --address-prefix 192.168.0.0/16 \
  --subnet-name mySubnet --subnet-prefix 192.168.1.0/24
```

Die Ausgabe zeigt das Subnetz als logisch innerhalb des virtuellen Netzwerks erstellt an:

```json
{
  "addressSpace": {
    "addressPrefixes": [
      "192.168.0.0/16"
    ]
  },
  "dhcpOptions": {
    "dnsServers": []
  },
  "etag": "W/\"e95496fc-f417-426e-a4d8-c9e4d27fc2ee\"",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
  "location": "westeurope",
  "name": "myVnet",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "resourceGuid": "ed62fd03-e9de-430b-84df-8a3b87cacdbb",
  "subnets": [
    {
      "addressPrefix": "192.168.1.0/24",
      "etag": "W/\"e95496fc-f417-426e-a4d8-c9e4d27fc2ee\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet",
      "ipConfigurations": null,
      "name": "mySubnet",
      "networkSecurityGroup": null,
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "resourceNavigationLinks": null,
      "routeTable": null
    }
  ],
  "tags": {},
  "type": "Microsoft.Network/virtualNetworks",
  "virtualNetworkPeerings": null
}
```


## <a name="create-a-public-ip-address"></a>Erstellen einer öffentlichen IP-Adresse
Jetzt erstellen wir die öffentliche IP-Adresse (PIP), die wir Ihrem Load Balancer zuweisen werden. Damit können Sie mithilfe des Befehls [az network public-ip create](/cli/azure/network/public-ip#create) über das Internet eine Verbindung mit Ihren virtuellen Computern herstellen. Da die Standardadresse dynamisch ist, erstellen wir in der Domäne **cloudapp.azure.com** mit der Option `--domain-name-label` einen benannten DNS-Eintrag. Im folgenden Beispiel wird eine öffentliche IP-Adresse namens `myPublicIP` mit dem DNS-Namen `mypublicdns` erstellt. Der DNS-Name muss eindeutig sein. Geben Sie daher einen eigenen eindeutigen Namen an:

```azurecli
az network public-ip create --resource-group myResourceGroup --location westeurope \
  --name myPublicIP --dns-name mypublicdns --allocation-method static --idle-timeout 4
```

Ausgabe:

```json
{
  "publicIp": {
    "dnsSettings": {
      "domainNameLabel": "mypublicdns",
      "fqdn": "mypublicdns.westeurope.cloudapp.azure.com",
      "reverseFqdn": ""
    },
    "idleTimeoutInMinutes": 4,
    "ipAddress": "52.174.48.109",
    "provisioningState": "Succeeded",
    "publicIPAllocationMethod": "Static",
    "resourceGuid": "78218510-ea54-42d7-b2c2-44773fc14af5"
  }
}
```

Die öffentliche IP-Adressressource wurde logisch zugeordnet, aber es wurde noch keine spezielle Adresse zugewiesen. Um eine IP-Adresse zu erhalten, benötigen Sie einen Load Balancer, den wir noch nicht erstellt haben.

## <a name="create-a-load-balancer-and-ip-pools"></a>Erstellen eines Load Balancers und IP-Pools
Wenn Sie einen Load Balancer erstellen, können sie den Datenverkehr auf mehrere virtuelle Computer verteilen. Der Load Balancer gewährleistet zudem Redundanz für Ihre Anwendung, indem mehrere virtuelle Computer ausgeführt werden, die bei der Wartung oder bei hoher Auslastung auf Anforderungen von Benutzern reagieren. Das folgende Beispiel verwendet [az network lb create](/cli/azure/network/lb#create), um einen Load Balancer mit dem Namen `myLoadBalancer` und einen Front-End-IP-Pool mit dem Namen `myFrontEndPool` zu erstellen und sie mit der `myPublicIP`-Ressource zu verbinden:

```azurecli
az network lb create --resource-group myResourceGroup --location westeurope \
  --name myLoadBalancer --public-ip-address myPublicIP --frontend-ip-name myFrontEndPool
```

Ausgabe:

```json
{
  "loadBalancer": {
    "backendAddressPools": [
      {
        "etag": "W/\"7a05df7a-5ee2-4581-b411-4b6f048ab291\"",
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myLoadBalancerbepool",
        "name": "myLoadBalancerbepool",
        "properties": {
          "provisioningState": "Succeeded"
        },
        "resourceGroup": "myResourceGroup"
      }
    ],
    "frontendIPConfigurations": [
      {
        "etag": "W/\"7a05df7a-5ee2-4581-b411-4b6f048ab291\"",
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/frontendIPConfigurations/myFrontEndPool",
        "name": "myFrontEndPool",
        "properties": {
          "privateIPAllocationMethod": "Dynamic",
          "provisioningState": "Succeeded",
          "publicIPAddress": {
            "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
            "resourceGroup": "myResourceGroup"
          }
        },
        "resourceGroup": "myResourceGroup"
      }
    ],
    "inboundNatPools": [],
    "inboundNatRules": [],
    "loadBalancingRules": [],
    "outboundNatRules": [],
    "probes": [],
    "provisioningState": "Succeeded",
    "resourceGuid": "f4879d84-5ae8-4e06-8b9b-1419baa875d9"
  }
}
```

Beachten Sie die Verwendung der Option `--public-ip-address` zum Übergeben der zuvor erstellten `myPublicIP`-Adresse. Die Zuweisung der öffentlichen IP-Adresse zum Load Balancer ermöglicht Ihnen, die virtuellen Computer über das Internet zu erreichen.

Wir verwenden einen Back-End-Pool als Speicherort für die virtuellen Computer, mit denen wir eine Verbindung herstellen. Auf diese Weise kann der Datenverkehr über den Load Balancer zu den virtuellen Computern weitergeleitet werden. Erstellen wir nun den IP-Pool für den Back-End-Datenverkehr mit [az network lb address-pool create](/cli/azure/network/lb/address-pool#create). Im folgenden Beispiel wird ein Back-End-Pool namens `myBackEndPool` erstellt:

```azurecli
az network lb address-pool create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myBackEndPool
```

Ausschnitt der Ausgabe:

```json
  "backendAddressPools": [
    {
      "backendIpConfigurations": null,
      "etag": "W/\"a61814bc-ce4c-4fb2-9d6a-5b1949078345\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myLoadBalancerbepool",
      "loadBalancingRules": null,
      "name": "myLoadBalancerbepool",
      "outboundNatRule": null,
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup"
    },
    {
      "backendIpConfigurations": null,
      "etag": "W/\"a61814bc-ce4c-4fb2-9d6a-5b1949078345\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool",
      "loadBalancingRules": null,
      "name": "myBackEndPool",
      "outboundNatRule": null,
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup"
    }
  ],
  "etag": "W/\"a61814bc-ce4c-4fb2-9d6a-5b1949078345\"",
```


## <a name="create-load-balancer-nat-rules"></a>Erstellen der NAT-Regeln für den Load Balancer
Damit der Datenverkehr über den Load Balancer abgewickelt wird, müssen wir Regeln für die Netzwerkadressenübersetzung (Network Address Translation, NAT) erstellen, die eingehende oder ausgehende Aktionen angeben. Sie können das verwendete Protokoll angeben und dann wie gewünscht externe Ports internen Ports zuordnen. Erstellen wir für unsere Umgebung mit [az network lb inbound-nat-rule create](/cli/azure/network/lb/inbound-nat-rule#create) ein paar Regeln, die SSH durch den Load Balancer zu unseren VMs durchlassen. Wir richten die TCP-Ports 4222 und 4223 so ein, dass der Datenverkehr auf den virtuellen Computern (die wir später erstellen) an den TCP-Port 22 geleitet wird. Im folgenden Beispiel wird eine Regel namens `myLoadBalancerRuleSSH1` erstellt, um den TCP-Port 4222 dem Port 22 zuzuordnen:

```azurecli
az network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH1 --protocol tcp \
  --frontend-port 4222 --backend-port 22 --frontend-ip-name myFrontEndPool
```

Ausgabe:

```json
{
  "backendIpConfiguration": null,
  "backendPort": 22,
  "enableFloatingIp": false,
  "etag": "W/\"72843cf8-b5fb-4655-9ac8-9cbbbbf1205a\"",
  "frontendIpConfiguration": {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/frontendIPConfigurations/myFrontEndPool",
    "resourceGroup": "myResourceGroup"
  },
  "frontendPort": 4222,
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1",
  "idleTimeoutInMinutes": 4,
  "name": "myLoadBalancerRuleSSH1",
  "protocol": "Tcp",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup"
}
```

Wiederholen Sie dieses Verfahren für Ihre zweite NAT-Regel für SSH. Im folgenden Beispiel wird eine Regel namens `myLoadBalancerRuleSSH2` erstellt, um den TCP-Port 4223 dem Port 22 zuzuordnen:

```azurecli
az network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH2 --protocol tcp \
  --frontend-port 4223 --backend-port 22 --frontend-ip-name myFrontEndPool
```

## <a name="create-a-load-balancer-health-probe"></a>Erstellen des Integritätstests für den Load Balancer
Mit einem Integritätstest werden die virtuellen Computer hinter dem Load Balancer regelmäßig überprüft, um sicherzustellen, dass sie wie definiert ausgeführt werden und auf Anforderungen reagieren. Wenn dies nicht der Fall ist, werden sie außer Betrieb gesetzt, sodass Benutzer nicht an sie weitergeleitet werden. Sie können benutzerdefinierte Überprüfungen sowie Intervalle und Timeoutwerte für den Integritätstest definieren. Weitere Informationen zu Integritätstests finden Sie unter [Load Balancer-Integritätstest](../../load-balancer/load-balancer-custom-probe-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Im folgenden Beispiel wird [az network lb probe create](/cli/azure/network/lb/probe#create) verwendet, um einen TCP-Integritätstest mit dem Namen `myHealthProbe` zu erstellen:

```azurecli
az network lb probe create --resource-group myResourceGroup --lb-name myLoadBalancer \
  --name myHealthProbe --protocol tcp --port 80 --interval 15 --threshold 4
```

Ausgabe:

```json
{
  "etag": "W/\"757018f6-b70a-4651-b717-48b511d82ba0\"",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/probes/myHealthProbe",
  "intervalInSeconds": 15,
  "loadBalancingRules": null,
  "name": "myHealthProbe",
  "numberOfProbes": 4,
  "port": 80,
  "protocol": "Tcp",
  "provisioningState": "Succeeded",
  "requestPath": null,
  "resourceGroup": "myResourceGroup"
}
```

Hier haben wir ein Intervall von 15 Sekunden für unsere Integritätstests angegeben. Bei maximal vier Tests (in 1 Minute) können Fehler auftreten, bevor der Load Balancer feststellt, dass der Host nicht mehr funktionsfähig ist.

Wir erstellen auch eine NAT-Regel für den TCP-Port 80 (Webdatenverkehr) und binden die Regel in die IP-Pools ein. Wenn wir die Regel in einen IP-Pool einbinden, anstatt die Regel einzeln auf den virtuellen Computern einzubinden, können wir virtuelle Computer dem IP-Pool hinzufügen oder sie daraus entfernen. Der Load Balancer passt dann automatisch den Datenverkehrsfluss an. Das folgende Beispiel verwendet [az network lb rule create](/cli/azure/network/lb/rule#create) zum Erstellen einer Regel mit dem Namen `myLoadBalancerRuleWeb`, um den TCP-Port 80 dem Port 80 zuzuordnen und den Integritätstest mit dem Namen `myHealthProbe` zu verbinden:

```azurecli
az network lb rule create --resource-group myResourceGroup --lb-name myLoadBalancer \
  --name myLoadBalancerRuleWeb --protocol tcp --frontend-port 80 --backend-port 80 \
  --frontend-ip-name myFrontEndPool --backend-pool-name myBackEndPool \
  --probe-name myHealthProbe
```

Ausgabe:

```json
{
  "backendAddressPool": {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool",
    "resourceGroup": "myResourceGroup"
  },
  "backendPort": 80,
  "enableFloatingIp": false,
  "etag": "W/\"f0d77680-bf42-4d11-bfab-5d2c541bee56\"",
  "frontendIpConfiguration": {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/frontendIPConfigurations/myFrontEndPool",
    "resourceGroup": "myResourceGroup"
  },
  "frontendPort": 80,
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb",
  "idleTimeoutInMinutes": 4,
  "loadDistribution": "Default",
  "name": "myLoadBalancerRuleWeb",
  "probe": {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/probes/myHealthProbe",
    "resourceGroup": "myResourceGroup"
  },
  "protocol": "Tcp",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="verify-the-load-balancer"></a>Überprüfen des Load Balancers
Jetzt ist die Load Balancer-Konfiguration abgeschlossen. Folgende Schritte haben Sie ausgeführt:

1. Sie haben einen Load Balancer erstellt.
2. Dann haben Sie einen Front-End-IP-Adresspool erstellt und ihm eine öffentliche IP-Adresse zugewiesen.
3. Anschließend haben Sie einen Back-End-IP-Adresspool erstellt, mit dem virtuelle Computer eine Verbindung herstellen können.
4. Sie haben NAT-Regeln für den SSH-Datenverkehr für die virtuellen Computer zu Verwaltungszwecken sowie eine Regel für den TCP-Port 80 für die Web-App erstellt.
5. Sie haben einen Integritätstest zur regelmäßigen Überprüfung der virtuellen Computer hinzugefügt. Dieser Integritätstest stellt sicher, dass Benutzer nicht versuchen, auf einen virtuellen Computer zuzugreifen, der nicht mehr funktioniert bzw. keine Inhalte mehr liefert.

Überprüfen wir nun mit [az network lb show](/cli/azure/network/lb#show), wie Ihr Load Balancer aussieht:

```azurecli
az network lb show --resource-group myResourceGroup --name myLoadBalancer
```

Ausgabe:

```json
{
  "backendAddressPools": [
    {
      "backendIpConfigurations": null,
      "etag": "W/\"a0e313a1-6de1-48be-aeb6-df57188d708c\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myLoadBalancerbepool",
      "loadBalancingRules": null,
      "name": "myLoadBalancerbepool",
      "outboundNatRule": null,
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup"
    },
    {
      "backendIpConfigurations": null,
      "etag": "W/\"a0e313a1-6de1-48be-aeb6-df57188d708c\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool",
      "loadBalancingRules": null,
      "name": "myBackEndPool",
      "outboundNatRule": null,
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup"
    }
  ],
  "etag": "W/\"a0e313a1-6de1-48be-aeb6-df57188d708c\"",
  "frontendIpConfigurations": [
    {
      "etag": "W/\"a0e313a1-6de1-48be-aeb6-df57188d708c\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/frontendIPConfigurations/LoadBalancerFrontEnd",
      "inboundNatPools": null,
      "inboundNatRules": null,
      "loadBalancingRules": null,
      "name": "LoadBalancerFrontEnd",
      "outboundNatRules": null,
      "privateIpAddress": null,
      "privateIpAllocationMethod": "Dynamic",
      "provisioningState": "Succeeded",
      "publicIpAddress": {
        "dnsSettings": null,
        "etag": null,
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/PublicIPmyLoadBalancer",
        "idleTimeoutInMinutes": null,
        "ipAddress": null,
        "ipConfiguration": null,
        "location": null,
        "name": null,
        "provisioningState": null,
        "publicIpAddressVersion": null,
        "publicIpAllocationMethod": null,
        "resourceGroup": "myResourceGroup",
        "resourceGuid": null,
        "tags": null,
        "type": null
      },
      "resourceGroup": "myResourceGroup",
      "subnet": null
    },
    {
      "etag": "W/\"a0e313a1-6de1-48be-aeb6-df57188d708c\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/frontendIPConfigurations/myFrontEndPool",
      "inboundNatPools": null,
      "inboundNatRules": null,
      "loadBalancingRules": null,
      "name": "myFrontEndPool",
      "outboundNatRules": null,
      "privateIpAddress": null,
      "privateIpAllocationMethod": "Dynamic",
      "provisioningState": "Succeeded",
      "publicIpAddress": {
        "dnsSettings": null,
        "etag": null,
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
        "idleTimeoutInMinutes": null,
        "ipAddress": null,
        "ipConfiguration": null,
        "location": null,
        "name": null,
        "provisioningState": null,
        "publicIpAddressVersion": null,
        "publicIpAllocationMethod": null,
        "resourceGroup": "myResourceGroup",
        "resourceGuid": null,
        "tags": null,
        "type": null
      },
      "resourceGroup": "myResourceGroup",
      "subnet": null
    }
  ],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "inboundNatPools": [],
  "inboundNatRules": [],
  "loadBalancingRules": [],
  "location": "westeurope",
  "name": "myLoadBalancer",
  "outboundNatRules": [],
  "probes": [],
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "resourceGuid": "b5815801-b53d-4c22-aafc-2a9064f90f3c",
  "tags": {},
  "type": "Microsoft.Network/loadBalancers"
}
```

## <a name="create-a-network-security-group-and-rules"></a>Erstellen einer Netzwerksicherheitsgruppe und von Regeln
Als Nächstes erstellen wir eine Netzwerksicherheitsgruppe und die eingehenden Regeln, mit denen der Zugriff auf die Netzwerkschnittstellenkarte gesteuert wird. Eine Netzwerksicherheitsgruppe kann auf eine NIC oder auf ein Subnetz angewendet werden. Der ein- und ausgehende Datenverkehr Ihrer virtuellen Computer wird durch Definieren entsprechender Regeln gesteuert. Im folgenden Beispiel wird mithilfe von [az network nsg create](/cli/azure/network/nsg#create) eine Netzwerksicherheitsgruppe mit dem Namen `myNetworkSecurityGroup` erstellt:

```azurecli
az network nsg create --resource-group myResourceGroup --location westeurope \
  --name myNetworkSecurityGroup
```

Fügen wir die eingehende Regel für die NSG mit [az network nsg rule create](/cli/azure/network/nsg/rule#create) hinzu, um eingehende Verbindungen an Port 22 zuzulassen (um SSH zu unterstützen). Im folgenden Beispiel wird eine Regel namens `myNetworkSecurityGroupRuleSSH` erstellt, um TCP auf Port 22 zuzulassen:

```azurecli
az network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --name myNetworkSecurityGroupRuleSSH \
  --protocol tcp --direction inbound --priority 1000 \
  --source-address-prefix '*' --source-port-range '*' \
  --destination-address-prefix '*' --destination-port-range 22 --access allow
```

Als Nächstes fügen wir die eingehende Regel für die NSG hinzu, um eingehende Verbindungen über Port 80 zuzulassen (Unterstützung von Webdatenverkehr). Im folgenden Beispiel wird eine Regel namens `myNetworkSecurityGroupRuleHTTP` erstellt, um TCP auf Port 80 zuzulassen:

```azurecli
az network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --name myNetworkSecurityGroupRuleHTTP \
  --protocol tcp --direction inbound --priority 1001 \
  --source-address-prefix '*' --source-port-range '*' \
  --destination-address-prefix '*' --destination-port-range 80 --access allow
```

> [!NOTE]
> Die eingehende Regel ist ein Filter für eingehende Netzwerkverbindungen. In diesem Beispiel binden wir die NSG an die virtuelle NIC der virtuellen Computer. Dies bedeutet, dass alle Anforderungen an Port 22 über die NIC auf dem virtuellen Computer übergeben werden. Dies ist eine Eingangsregel über eine Netzwerkverbindung und nicht über einen Endpunkt wie in klassischen Bereitstellungen. Zum Öffnen eines Ports muss `--source-port-range` auf „\*“ (Standardwert) festgelegt bleiben, damit eingehende Anforderungen von **jedem** anfordernden Port akzeptiert werden. Ports sind in der Regel dynamisch.

Untersuchen Sie die Netzwerksicherheitsgruppe und die Regeln mit [az network nsg show](/cli/azure/network/nsg#show):

```azurecli
az network nsg show --resource-group myResourceGroup --name myNetworkSecurityGroup
```

Ausgabe:

```json
{
  "defaultSecurityRules": [
    {
      "access": "Allow",
      "description": "Allow inbound traffic from all VMs in VNET",
      "destinationAddressPrefix": "VirtualNetwork",
      "destinationPortRange": "*",
      "direction": "Inbound",
      "etag": "W/\"2b656589-f332-4ba4-92f3-afb3be5c192c\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/AllowVnetInBound",
      "name": "AllowVnetInBound",
      "priority": 65000,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "VirtualNetwork",
      "sourcePortRange": "*"
    },
    {
      "access": "Allow",
      "description": "Allow inbound traffic from azure load balancer",
      "destinationAddressPrefix": "*",
      "destinationPortRange": "*",
      "direction": "Inbound",
      "etag": "W/\"2b656589-f332-4ba4-92f3-afb3be5c192c\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/AllowAzureLoadBalancerInBound",
      "name": "AllowAzureLoadBalancerInBound",
      "priority": 65001,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "AzureLoadBalancer",
      "sourcePortRange": "*"
    },
    {
      "access": "Deny",
      "description": "Deny all inbound traffic",
      "destinationAddressPrefix": "*",
      "destinationPortRange": "*",
      "direction": "Inbound",
      "etag": "W/\"2b656589-f332-4ba4-92f3-afb3be5c192c\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/DenyAllInBound",
      "name": "DenyAllInBound",
      "priority": 65500,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    },
    {
      "access": "Allow",
      "description": "Allow outbound traffic from all VMs to all VMs in VNET",
      "destinationAddressPrefix": "VirtualNetwork",
      "destinationPortRange": "*",
      "direction": "Outbound",
      "etag": "W/\"2b656589-f332-4ba4-92f3-afb3be5c192c\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/AllowVnetOutBound",
      "name": "AllowVnetOutBound",
      "priority": 65000,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "VirtualNetwork",
      "sourcePortRange": "*"
    },
    {
      "access": "Allow",
      "description": "Allow outbound traffic from all VMs to Internet",
      "destinationAddressPrefix": "Internet",
      "destinationPortRange": "*",
      "direction": "Outbound",
      "etag": "W/\"2b656589-f332-4ba4-92f3-afb3be5c192c\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/AllowInternetOutBound",
      "name": "AllowInternetOutBound",
      "priority": 65001,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    },
    {
      "access": "Deny",
      "description": "Deny all outbound traffic",
      "destinationAddressPrefix": "*",
      "destinationPortRange": "*",
      "direction": "Outbound",
      "etag": "W/\"2b656589-f332-4ba4-92f3-afb3be5c192c\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/DenyAllOutBound",
      "name": "DenyAllOutBound",
      "priority": 65500,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    }
  ],
  "etag": "W/\"2b656589-f332-4ba4-92f3-afb3be5c192c\"",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup",
  "location": "westeurope",
  "name": "myNetworkSecurityGroup",
  "networkInterfaces": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "resourceGuid": "79c2c293-ee3e-4616-bf9c-4741ff1f708a",
  "securityRules": [
    {
      "access": "Allow",
      "description": null,
      "destinationAddressPrefix": "*",
      "destinationPortRange": "22",
      "direction": "Inbound",
      "etag": "W/\"2b656589-f332-4ba4-92f3-afb3be5c192c\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/securityRules/myNetworkSecurityGroupRuleSSH",
      "name": "myNetworkSecurityGroupRuleSSH",
      "priority": 1000,
      "protocol": "tcp",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    },
    {
      "access": "Allow",
      "description": null,
      "destinationAddressPrefix": "*",
      "destinationPortRange": "80",
      "direction": "Inbound",
      "etag": "W/\"2b656589-f332-4ba4-92f3-afb3be5c192c\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/securityRules/myNetworkSecurityGroupRuleHTTP",
      "name": "myNetworkSecurityGroupRuleHTTP",
      "priority": 1001,
      "protocol": "tcp",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    }
  ],
  "subnets": null,
  "tags": {},
  "type": "Microsoft.Network/networkSecurityGroups"
}
```

## <a name="create-an-nic-to-use-with-the-linux-vm"></a>Erstellen einer Netzwerkschnittstellenkarte zur Verwendung mit der Linux-VM
NICs sind programmgesteuert verfügbar, da Sie Regeln zu ihrer Verwendung anwenden können. Sie können auch mehrere Regeln anwenden. Mit dem folgenden Befehl [az network nic create](https://docs.microsoft.com/en-us/cli/azure/network/nic#create) wird die Netzwerkkarte in den Last-Back-End-IP-Pool eingebunden und der NAT-Regel sowie der Netzwerksicherheitsgruppe für das Zulassen von SSH-Datenverkehr zugeordnet.

Im folgenden Beispiel wird eine NIC namens `myNic1` erstellt:

```azurecli
az network nic create --resource-group myResourceGroup --location westeurope --name myNic1 \
  --vnet-name myVnet --subnet mySubnet --network-security-group myNetworkSecurityGroup \
  --lb-name myLoadBalancer --lb-address-pools myBackEndPool \
  --lb-inbound-nat-rules myLoadBalancerRuleSSH1
```

Ausgabe:

```json
{
  "newNIC": {
    "dnsSettings": {
      "appliedDnsServers": [],
      "dnsServers": []
    },
    "enableIPForwarding": false,
    "ipConfigurations": [
      {
        "etag": "W/\"a76b5c0d-14e1-4a99-afd4-5cd8ac0465ca\"",
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1/ipConfigurations/ipconfig1",
        "name": "ipconfig1",
        "properties": {
          "loadBalancerBackendAddressPools": [
            {
              "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool",
              "resourceGroup": "myResourceGroup"
            }
          ],
          "loadBalancerInboundNatRules": [
            {
              "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1",
              "resourceGroup": "myResourceGroup"
            }
          ],
          "primary": true,
          "privateIPAddress": "192.168.1.4",
          "privateIPAllocationMethod": "Dynamic",
          "provisioningState": "Succeeded",
          "subnet": {
            "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet",
            "resourceGroup": "myResourceGroup"
          }
        },
        "resourceGroup": "myResourceGroup"
      }
    ],
    "networkSecurityGroup": {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup",
      "resourceGroup": "myResourceGroup"
    },
    "provisioningState": "Succeeded",
    "resourceGuid": "838977cb-cf4b-4c4a-9a32-0ddec7dc818b"
  }
}
```

Jetzt erstellen wir die zweite NIC und binden sie erneut in unseren Back-End-IP-Adresspool ein. Dieses Mal lässt die zweite NAT-Regel SSH-Datenverkehr zu. Im folgenden Beispiel wird eine NIC namens `myNic2` erstellt:

```azurecli
az network nic create --resource-group myResourceGroup --location westeurope --name myNic2 \
  --vnet-name myVnet --subnet mySubnet --network-security-group myNetworkSecurityGroup \
  --lb-name myLoadBalancer --lb-address-pools myBackEndPool \
  --lb-inbound-nat-rules myLoadBalancerRuleSSH2
```


## <a name="create-an-availability-set"></a>Verfügbarkeitsgruppe erstellen
Mit Verfügbarkeitsgruppen können die virtuellen Computer auf Fehlerdomänen und Upgradedomänen verteilt werden. Erstellen wir eine Verfügbarkeitsgruppe für Ihre virtuellen Computer mit [az vm availability-set create](/cli/azure/vm/availability-set#create). Im folgenden Beispiel wird eine Verfügbarkeitsgruppe namens `myAvailabilitySet` erstellt:

```azurecli
az vm availability-set create --resource-group myResourceGroup --location westeurope \
  --name myAvailabilitySet \
  --platform-fault-domain-count 3 --platform-update-domain-count 2
```

Durch Fehlerdomänen wird eine Gruppe virtueller Computer definiert, die eine Stromquelle und einen Netzwerkswitch gemeinsam nutzen. Die innerhalb der Verfügbarkeitsgruppe konfigurierten virtuellen Computer werden standardmäßig auf bis zu drei Fehlerdomänen verteilt. Dadurch wirkt sich ein Hardwareproblem in einer dieser Fehlerdomänen nicht auf jeden virtuellen Computer aus, auf dem Ihre Anwendung ausgeführt wird. Azure verteilt virtuelle Computer automatisch auf die Fehlerdomänen, wenn sie in einer Verfügbarkeitsgruppe platziert werden.

Upgradedomänen definieren Gruppen virtueller Computer und die zugrunde liegende physische Hardware, die gleichzeitig neu gestartet werden können. Während einer geplanten Wartung werden die Upgradedomänen möglicherweise nicht der Reihe nach neu gestartet. Es wird aber jeweils nur eine Upgradedomäne neu gestartet. Azure verteilt Ihre virtuellen Computer auch hier automatisch auf die Upgradedomänen, wenn sie in einer Verfügbarkeitsgruppe platziert werden.

Weitere Informationen finden Sie unter [Verwalten der Verfügbarkeit virtueller Computer](manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="create-the-linux-vms"></a>Erstellen der Linux-VMs
Sie haben die Netzwerkressourcen zur Unterstützung von über das Internet erreichbaren virtuellen Computern erstellt. Wir erstellen jetzt diese virtuellen Computer und schützen sie mit einem SSH-Schlüssel ohne Kennwort. In diesem Fall erstellen wir eine Ubuntu-VM basierend auf dem aktuellen LTS-Stand. Wir ermitteln diese Imageinformationen mithilfe von [az vm image list](/cli/azure/vm/image#list). Dies wird unter [Suchen nach Azure VM-Images](cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) beschrieben.

Wir legen außerdem einen SSH-Schlüssel an, der für die Authentifizierung verwendet wird. Wenn Sie nicht über SSH-Schlüssel verfügen, können Sie sie mithilfe [dieser Anweisungen](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) erstellen. Alternativ können Sie Ihre SSH-Verbindungen nach der Erstellung des virtuellen Computers mithilfe der `--admin-password`-Methode authentifizieren. Diese Methode ist in der Regel weniger sicher.

Wir erstellen den virtuellen Computer, indem wir alle Ressourcen und Informationen mit dem Befehl [az vm create](/cli/azure/vm#create) zusammenfassen. Im folgenden Beispiel wird ein virtueller Computer namens `myVM1` erstellt, der Azure Managed Disks verwendet. Wenn Sie nicht verwaltete Datenträger verwenden möchten, finden Sie weitere Informationen im obigen Zusatzhinweis.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM1 \
    --location westeurope \
    --availability-set myAvailabilitySet \
    --nics myNic1 \
    --image UbuntuLTS \
    --ssh-key-value ~/.ssh/id_rsa.pub \
    --admin-username azureuser
```

Wenn Sie Azure Managed Disks verwenden, überspringen Sie diesen Schritt. Wenn Sie nicht verwaltete Datenträger verwenden möchten und in den vorherigen Schritten ein Speicherkonto erstellt haben, müssen Sie dem oben stehenden Befehl einige zusätzliche Parameter hinzufügen. Fügen Sie dem oben stehenden Befehl die folgenden zusätzlichen Parameter hinzu, um die nicht verwalteten Datenträger im Speicherkonto `mystorageaccount` zu erstellen: 

```azurecli
  --use-unmanaged-disk \
  --storage-account mystorageaccount
```

Ausgabe:

```json
{
  "fqdn": "",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM1",
  "macAddress": "",
  "privateIpAddress": "",
  "publicIpAddress": "",
  "resourceGroup": "myResourceGroup"
}
```

Anschließend können Sie mit Ihren standardmäßigen SSH-Schlüsseln eine Verbindung mit dem virtuellen Computer herstellen. Stellen Sie sicher, dass Sie den entsprechenden Port angeben, da die Weiterleitung über den Load Balancer erfolgt. (Für unseren ersten virtuellen Computer richten wir die NAT-Regel zum Weiterleiten von Port 4222 an den virtuellen Computer ein.)

```bash
ssh ops@mypublicdns.westeurope.cloudapp.azure.com -p 4222 -i ~/.ssh/id_rsa.pub
```

Ausgabe:

```bash
The authenticity of host '[mypublicdns.westeurope.cloudapp.azure.com]:4222 ([xx.xx.xx.xx]:4222)' can't be established.
ECDSA key fingerprint is 94:2d:d0:ce:6b:fb:7f:ad:5b:3c:78:93:75:82:12:f9.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '[mypublicdns.westeurope.cloudapp.azure.com]:4222,[xx.xx.xx.xx]:4222' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 16.04.1 LTS (GNU/Linux 4.4.0-34-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

ops@myVM1:~$
```

Erstellen Sie nun den zweiten virtuellen Computer auf die gleiche Weise:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM2 \
    --location westeurope \
    --availability-set myAvailabilitySet \
    --nics myNic2 \
    --image UbuntuLTS \
    --ssh-key-value ~/.ssh/id_rsa.pub \
    --admin-username azureuser
```

Auch hier gilt: Wenn Sie die Standardeinstellung „Azure Managed Disks“ nicht verwenden, fügen Sie dem oben stehenden Befehl die folgenden zusätzlichen Parameter hinzu, um die nicht verwalteten Datenträger im Speicherkonto `mystorageaccount` zu erstellen:

```azurecli
  --use-unmanaged-disk \
  --storage-account mystorageaccount
``` 

Nun werden die virtuellen Ubuntu-Computer, bei denen Sie sich nur mit Ihrem SSH-Schlüsselpaar anmelden können (weil Kennwörter deaktiviert sind), hinter einem Load Balancer in Azure ausgeführt. Sie können „nginx“ oder „httpd“ installieren und eine Web-App bereitstellen. Zudem können Sie den Datenverkehrsfluss durch den Load Balancer an beide virtuellen Computer sehen.


## <a name="export-the-environment-as-a-template"></a>Exportieren der Umgebung als Vorlage
Nachdem Sie nun diese Umgebung erstellt haben, möchten Sie vielleicht eine weitere Entwicklungsumgebung mit den gleichen Parametern oder eine entsprechende Produktionsumgebung erstellen. Der Resource Manager verwendet die JSON-Vorlagen, die alle Parameter für Ihre Umgebung definieren. Sie erstellen ganze Umgebungen durch Verweisen auf diese JSON-Vorlage. Sie können [JSON-Vorlagen manuell erstellen](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) oder eine vorhandene Umgebung exportieren, um eine JSON-Vorlage zu erstellen. Verwenden Sie [az group export](/cli/azure/group#export) wie folgt, um Ihre Ressourcengruppe zu exportieren:

```azurecli
az group export --name myResourceGroup > myResourceGroup.json
```

Dieser Befehl erstellt die `myResourceGroup.json`-Datei in Ihrem aktuellen Arbeitsverzeichnis. Wenn Sie eine Umgebung aus dieser Vorlage erstellen, werden Sie aufgefordert, alle Ressourcennamen, beispielsweise die Namen für Load Balancer, Netzwerkschnittstellen oder virtuelle Computer einzugeben. Sie können diese Namen in Ihre Vorlagendatei eintragen, indem Sie den Parameter `--include-parameter-default-value` dem Befehl **az group export** hinzufügen, der zuvor gezeigt wurde. Bearbeiten Sie die JSON-Vorlage zum Angeben der Ressourcennamen, oder [erstellen Sie eine Datei „parameters.json“](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) , die die Ressourcennamen angibt.

Verwenden Sie zum Erstellen einer Umgebung aus Ihrer Vorlage [az group deployment create](/cli/azure/group/deployment#create) wie folgt:

```azurecli
az group deployment create --resource-group myNewResourceGroup \
  --template-file myResourceGroup.json
```

Möglicherweise möchten Sie [weitere Informationen zum Bereitstellen von Vorlagen](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)lesen. Erfahren Sie, wie Sie Umgebungen unter Verwendung der Parameterdatei schrittweise aktualisieren und von einem zentralen Speicherort aus auf Vorlagen zugreifen.

## <a name="next-steps"></a>Nächste Schritte
Sie können jetzt beginnen, mit mehreren Netzwerkkomponenten und VMs zu arbeiten. Sie können diese Beispielumgebung nutzen, um Ihre Anwendung mithilfe der hier eingeführten zentralen Komponenten zu erstellen.

