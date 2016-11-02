
<properties
   pageTitle="Erstellen einer vollständigen Linux-Umgebung über die Azure-Befehlszeilenschnittstelle | Microsoft Azure"
   description="Erfahren Sie, wie Sie Speicher, einen virtuellen Linux-Computer, ein virtuelles Netzwerk mitsamt Subnetz, einen Load Balancer, eine NIC, eine öffentliche IP-Adresse und eine Netzwerksicherheitsgruppe über die Azure-CLI von Grund auf neu erstellen."
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="iainfoulds"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="08/23/2016"
   ms.author="iainfou"/>

# Erstellen einer vollständigen Linux-Umgebung über die Azure-Befehlszeilenschnittstelle

In diesem Artikel erstellen wir ein einfaches Netzwerk mit einem Load Balancer und zwei virtuellen Computern, die zu Entwicklungszwecken und für einfache Berechnungen nützlich sind. Wir führen Sie Befehl für Befehl durch den Prozess, bis Sie über zwei funktionierende, sichere virtuelle Linux-Computer verfügen, mit denen Sie über das Internet von jedem Ort aus eine Verbindung herstellen können. Anschließend können Sie sich mit komplexeren Netzwerken und Umgebungen beschäftigen.

Im Laufe dieses Vorgangs lernen Sie die Abhängigkeitshierarchie kennen, die Sie mit dem Resource Manager-Bereitstellungsmodell erhalten, sowie die damit verbundene hohe Leistungsfähigkeit. Wenn Sie wissen, wie das System aufgebaut ist, können Sie es mithilfe von [Azure Resource Manager-Vorlagen](../resource-group-authoring-templates.md) deutlich schneller neu erstellen. Und wenn Sie wissen, wie die einzelnen Komponenten Ihrer Umgebung zusammenwirken, ist die Erstellung von Vorlagen für deren Automatisierung viel einfacher.

Die Umgebung enthält:

- Zwei virtuelle Computer in einer Verfügbarkeitsgruppe
- Load Balancer mit Lastenausgleichsregel an Port 80
- Regeln für Netzwerksicherheitsgruppen (NSG) zum Schutz der virtuellen Computer vor unerwünschtem Datenverkehr

![Grundlegende Umgebung – Übersicht](./media/virtual-machines-linux-create-cli-complete/environment_overview.png)

Um diese benutzerdefinierte Umgebung zu erstellen, benötigen Sie die aktuelle [Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md) im Resource Manager-Modus (`azure config mode arm`). Sie benötigen außerdem ein JSON-Analysetool. In diesem Beispiel wird [jq](https://stedolan.github.io/jq/) verwendet.

## Schnellbefehle
Sie können die folgenden Schnellbefehle zum Erstellen Ihrer benutzerdefinierten Umgebung verwenden. In der [folgenden ausführlichen exemplarischen Vorgehensweise](#detailed-walkthrough) erfahren Sie, welche Aufgaben Sie beim Erstellen der Umgebung mit den einzelnen Befehlen ausführen können.

Erstellen Sie die Ressourcengruppe:

```bash
azure group create TestRG -l westeurope
```

Überprüfen Sie die Ressourcengruppe mit dem JSON-Parser:

```bash
azure group show TestRG --json | jq '.'
```

Erstellen Sie das Speicherkonto:

```bash
azure storage account create -g TestRG -l westeurope --kind Storage --sku-name GRS computeteststore
```

Überprüfen Sie das Speicherkonto mithilfe des JSON-Parsers:

```bash
azure storage account show -g TestRG computeteststore --json | jq '.'
```

Erstellen Sie das virtuelle Netzwerk:

```bash
azure network vnet create -g TestRG -n TestVNet -a 192.168.0.0/16 -l westeurope
```

Erstellen Sie das Subnetz:

```bash
azure network vnet subnet create -g TestRG -e TestVNet -n FrontEnd -a 192.168.1.0/24
```

Überprüfen Sie das virtuelle Netzwerk und Subnetz mit der JSON-Parser:


```bash
azure network vnet show TestRG TestVNet --json | jq '.'
```

Erstellen Sie eine öffentliche IP-Adresse:

```bash
azure network public-ip create -g TestRG -n TestLBPIP -l westeurope -d testlb -a static -i 4
```

Erstellen Sie den Load Balancer:

```bash
azure network lb create -g TestRG -n TestLB -l westeurope
```

Erstellen Sie einen Front-End-IP-Pool für den Load Balancer, und ordnen Sie die öffentliche IP-Adresse zu:

```bash
azure network lb frontend-ip create -g TestRG -l TestLB -n TestFrontEndPool -i TestLBPIP
```

Erstellen Sie den Back-End-IP-Pool für den Load Balancer:

```bash
azure network lb address-pool create -g TestRG -l TestLB -n TestBackEndPool
```

Erstellen Sie die eingehenden NAT-Regeln (SSH) für den Load Balancer:

```bash
azure network lb inbound-nat-rule create -g TestRG -l TestLB -n VM1-SSH -p tcp -f 4222 -b 22
azure network lb inbound-nat-rule create -g TestRG -l TestLB -n VM2-SSH -p tcp -f 4223 -b 22
```

Erstellen Sie die eingehenden NAT-Regeln (Web) für den Load Balancer:

```bash
azure network lb rule create -g TestRG -l TestLB -n WebRule -p tcp -f 80 -b 80 \
     -t TestFrontEndPool -o TestBackEndPool
```

Erstellen Sie den Integritätstest für den Load Balancer:

```bash
azure network lb probe create -g TestRG -l TestLB -n HealthProbe -p "tcp" -i 15 -c 4
```

Überprüfen Sie Load Balancer, IP-Pools und NAT-Regeln mithilfe des JSON-Parsers:

```bash
azure network lb show -g TestRG -n TestLB --json | jq '.'
```

Erstellen Sie die erste Netzwerkschnittstellenkarte (NIC, Network Interface Card):

```bash
azure network nic create -g TestRG -n LB-NIC1 -l westeurope --subnet-vnet-name TestVNet --subnet-name FrontEnd \
    -d "/subscriptions/########-####-####-####-############/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/backendAddressPools/TestBackEndPool" \
    -e "/subscriptions/########-####-####-####-############/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/inboundNatRules/VM1-SSH"
```

Erstellen Sie die zweite Netzwerkkarte:

```bash
azure network nic create -g TestRG -n LB-NIC2 -l westeurope --subnet-vnet-name TestVNet --subnet-name FrontEnd \
    -d "/subscriptions/########-####-####-####-############/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/backendAddressPools/TestBackEndPool" \
    -e "/subscriptions/########-####-####-####-############/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/inboundNatRules/VM2-SSH"
```

Überprüfen Sie die Netzwerkkarten mithilfe des JSON-Parsers:

```bash
azure network nic show TestRG LB-NIC1 --json | jq '.'
azure network nic show TestRG LB-NIC2 --json | jq '.'
```

Erstellen Sie die NSG:

```bash
azure network nsg create -g TestRG -n TestNSG -l westeurope
```

Fügen Sie die eingehenden Regeln für die NSG hinzu:

```bash
azure network nsg rule create --protocol tcp --direction inbound --priority 1000 \
    --destination-port-range 22 --access allow -g TestRG -a TestNSG -n SSHRule
azure network nsg rule create --protocol tcp --direction inbound --priority 1001 \
    --destination-port-range 80 --access allow -g TestRG -a TestNSG -n HTTPRule
```

Überprüfen Sie die NSG und die eingehenden Regeln mithilfe des JSON-Parsers:

```bash
azure network nsg show -g TestRG -n TestNSG --json | jq '.'
```

Binden der NSG an die Netzwerkkarten:

```bash
azure network nic set -g TestRG -n LB-NIC1 -o TestNSG
azure network nic set -g TestRG -n LB-NIC2 -o TestNSG
```

Erstellen Sie die Verfügbarkeitsgruppe:

```bash
azure availset create -g TestRG -n TestAvailSet -l westeurope
```

Erstellen Sie den ersten virtuellen Linux-Computer:

```bash
azure vm create \
    --resource-group TestRG \
    --name TestVM1 \
    --location westeurope \
    --os-type linux \
    --availset-name TestAvailSet \
    --nic-name LB-NIC1 \
    --vnet-name TestVnet \
    --vnet-subnet-name FrontEnd \
    --storage-account-name computeteststore \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username ops
```

Erstellen Sie den zweiten virtuellen Linux-Computer:

```bash
azure vm create \
    --resource-group TestRG \
    --name TestVM2 \
    --location westeurope \
    --os-type linux \
    --availset-name TestAvailSet \
    --nic-name LB-NIC2 \
    --vnet-name TestVnet \
    --vnet-subnet-name FrontEnd \
    --storage-account-name computeteststore \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username ops
```

Stellen Sie mit dem JSON-Parser sicher, dass alle Elemente erstellt wurden:

```bash
azure vm show -g TestRG -n TestVM1 --json | jq '.'
azure vm show -g TestRG -n TestVM2 --json | jq '.'
```

Exportieren Sie die erstellte Umgebung in eine Vorlage, damit Sie schnell neue Instanzen erstellen können:

```bash
azure resource export TestRG
```

## Ausführliche exemplarische Vorgehensweise
In den folgenden ausführlichen Schritten wird erklärt, was jeder Befehl macht, wenn Sie Ihre Umgebung erstellen. Diese Konzepte helfen Ihnen beim Erstellen Ihrer eigenen benutzerdefinierten Umgebungen für Entwicklung oder Produktion.

## Erstellen einer Ressourcengruppe und Auswählen von Standorten für die Bereitstellung

Azure-Ressourcengruppen sind logische Bereitstellungsentitäten, die Konfigurationsdaten und Metadaten enthalten, um die logische Verwaltung von Ressourcenbereitstellungen zu ermöglichen.

```bash
azure group create TestRG westeurope
```

Ausgabe:

```bash                        
info:    Executing command group create
+ Getting resource group TestRG
+ Creating resource group TestRG
info:    Created resource group TestRG
data:    Id:                  /subscriptions/<yoursub>/resourceGroups/TestRG
data:    Name:                TestRG
data:    Location:            westeurope
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

## Erstellen Sie ein Speicherkonto.

Sie benötigen unter anderem Speicherkonten für Ihre VM-Datenträger und für alle zusätzlichen Datenträger, die Sie hinzufügen möchten. Sie erstellen Speicherkonten normalerweise immer direkt nach der Erstellung von Ressourcengruppen.

Hier verwenden wir den Befehl `azure storage account create` und übergeben den Speicherort des Kontos, die Ressourcengruppe, mit der es gesteuert wird, und den Typ der gewünschten Speicherunterstützung.

```bash
azure storage account create \  
--location westeurope \
--resource-group TestRG \
--kind Storage --sku-name GRS \
computeteststore
```

Ausgabe:

```bash
info:    Executing command storage account create
+ Creating storage account
info:    storage account create command OK
ahmet•~/workspace/keygen» azure group show testrg
info:    Executing command group show
+ Listing resource groups
+ Listing resources for the group
data:    Id:                  /subscriptions/<guid>/resourceGroups/TestRG
data:    Name:                TestRG
data:    Location:            westeurope
data:    Provisioning State:  Succeeded
data:    Tags: null
data:    Resources:
data:
data:      Id      : /subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Storage/storageAccounts/computeteststore
data:      Name    : computeteststore
data:      Type    : storageAccounts
data:      Location: westeurope
data:      Tags    :
data:
data:    Permissions:
data:      Actions: *
data:      NotActions:
data:
info:    group show command OK
```

Um unsere Ressourcengruppe mit dem Befehl `azure group show` zu überprüfen, setzen wir das Tool [jq](https://stedolan.github.io/jq/) zusammen mit der Azure-CLI-Option `--json` ein. (Zur Analyse von JSON können Sie **jsawk** oder eine andere Sprachbibliothek Ihrer Wahl verwenden.)

```bash
azure group show TestRG --json | jq                                                                                      
```


Ausgabe:

```bash
{
  "tags": {},
  "id": "/subscriptions/<guid>/resourceGroups/TestRG",
  "name": "TestRG",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Storage/storageAccounts/computeteststore",
      "name": "computeteststore",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

Um das Speicherkonto mithilfe der Befehlszeilenschnittstelle zu untersuchen, müssen Sie zunächst die Kontonamen und Schlüssel festlegen, indem Sie eine Variante des folgenden Befehls verwenden. Ersetzen Sie den Namen des Speicherkontos im folgenden Beispiel mit einem Namen Ihrer Wahl:

```
AZURE_STORAGE_CONNECTION_STRING="$(azure storage account connectionstring show computeteststore --resource-group testrg --json | jq -r '.string')"
```

Sie können Ihre Speicherinformationen dann leicht anzeigen:

```bash
azure storage container list
```

Ausgabe:

```bash
info:    Executing command storage container list
+ Getting storage containers
data:    Name  Public-Access  Last-Modified
data:    ----  -------------  -----------------------------
data:    vhds  Off            Sun, 27 Sep 2015 19:03:54 GMT
info:    storage container list command OK
```

## Erstellen eines virtuellen Netzwerks und des Subnetzes

Als Nächstes müssen Sie ein virtuelles Netzwerk erstellen, das in Azure ausgeführt wird, und ein Subnetz, in dem Sie Ihre virtuellen Computer installieren können.

```bash
azure network vnet create -g TestRG -n TestVNet -a 192.168.0.0/16 -l westeurope
```

Ausgabe:

```bash
info:    Executing command network vnet create
+ Looking up virtual network "TestVNet"
+ Creating virtual network "TestVNet"
+ Loading virtual network state
data:    Id                              : /subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
data:    Name                            : TestVNet
data:    Type                            : Microsoft.Network/virtualNetworks
data:    Location                        : westeurope
data:    ProvisioningState               : Succeeded
data:    Address prefixes:
data:      192.168.0.0/16
info:    network vnet create command OK
```

Auch hier verwenden wir wieder die Option „--json“ von `azure group show` und **jq**, um zu sehen, wie wir unsere Ressourcen erstellen. Wir verfügen nun über eine `storageAccounts`-Ressource und eine `virtualNetworks`-Ressource.

```bash
azure group show TestRG --json | jq '.'
```

Ausgabe:

```bash
{
  "tags": {},
  "id": "/subscriptions/<guid>/resourceGroups/TestRG",
  "name": "TestRG",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet",
      "name": "TestVNet",
      "type": "virtualNetworks",
      "location": "westeurope",
      "tags": null
    },
    {
      "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Storage/storageAccounts/computeteststore",
      "name": "computeteststore",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

Jetzt erstellen wir ein Subnetz im virtuellen Netzwerk `TestVnet`, in dem die virtuellen Computer bereitgestellt werden. Wir verwenden den Befehl `azure network vnet subnet create` zusammen mit den Ressourcen, die wir bereits erstellt haben: die Ressourcengruppe `TestRG` und das virtuelle Netzwerk `TestVNet`. Wir fügen den Subnetznamen `FrontEnd` und das Subnetzadresspräfix `192.168.1.0/24` wie folgt hinzu:

```bash
azure network vnet subnet create -g TestRG -e TestVNet -n FrontEnd -a 192.168.1.0/24
```

Ausgabe:

```bash
info:    Executing command network vnet subnet create
+ Looking up the subnet "FrontEnd"
+ Creating subnet "FrontEnd"
+ Looking up the subnet "FrontEnd"
data:    Id                              : /subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
data:    Type                            : Microsoft.Network/virtualNetworks/subnets
data:    ProvisioningState               : Succeeded
data:    Name                            : FrontEnd
data:    Address prefix                  : 192.168.1.0/24
data:
info:    network vnet subnet create command OK
```

Da das Subnetz logisch im virtuellen Netzwerk angeordnet ist, ermitteln wir die Subnetzinformationen mit einem etwas anderen Befehl. Wir verwenden den Befehl `azure network vnet show`, doch wir untersuchen die JSON-Ausgabe weiterhin mit **jq**.

```bash
azure network vnet show TestRG TestVNet --json | jq '.'
```

Ausgabe:

```bash
{
  "subnets": [
    {
      "ipConfigurations": [],
      "addressPrefix": "192.168.1.0/24",
      "provisioningState": "Succeeded",
      "name": "FrontEnd",
      "etag": "W/"974f3e2c-028e-4b35-832b-a4b16ad25eb6"",
      "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd"
    }
  ],
  "tags": {},
  "addressSpace": {
    "addressPrefixes": [
      "192.168.0.0/16"
    ]
  },
  "dhcpOptions": {
    "dnsServers": []
  },
  "provisioningState": "Succeeded",
  "etag": "W/"974f3e2c-028e-4b35-832b-a4b16ad25eb6"",
  "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet",
  "name": "TestVNet",
  "location": "westeurope"
}
```

## Erstellen einer öffentlichen IP-Adresse (PIP)

Jetzt erstellen wir die öffentliche IP-Adresse (PIP), die wir Ihrem Load Balancer zuweisen werden. Damit können Sie mithilfe des Befehls `azure network public-ip create` über das Internet eine Verbindung mit Ihren virtuellen Computern herstellen. Da die Standardadresse dynamisch ist, erstellen wir in der Domäne **cloudapp.azure.com** mit der Option `-d testsubdomain` einen benannten DNS-Eintrag.

```bash
azure network public-ip create -d testsubdomain TestRG TestPIP westeurope
```

Ausgabe:

```bash
info:    Executing command network public-ip create
+ Looking up the public ip "TestPIP"
+ Creating public ip address "TestPIP"
+ Looking up the public ip "TestPIP"
data:    Id                              : /subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP
data:    Name                            : TestPIP
data:    Type                            : Microsoft.Network/publicIPAddresses
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Allocation method               : Dynamic
data:    Idle timeout                    : 4
data:    Domain name label               : testsubdomain
data:    FQDN                            : testsubdomain.westeurope.cloudapp.azure.com
info:    network public-ip create command OK
```

Die öffentliche IP-Adresse ist außerdem eine Ressource der obersten Ebene, die Sie mit `azure group show` anzeigen können.

```bash
azure group show TestRG --json | jq '.'
```

Ausgabe:

```bash
{
"tags": {},
"id": "/subscriptions/guid/resourceGroups/TestRG",
"name": "TestRG",
"provisioningState": "Succeeded",
"location": "westeurope",
"properties": {
    "provisioningState": "Succeeded"
},
"resources": [
    {
    "id": "/subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC",
    "name": "TestNIC",
    "type": "networkInterfaces",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/testrg/providers/Microsoft.Network/publicIPAddresses/testpip",
    "name": "testpip",
    "type": "publicIPAddresses",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet",
    "name": "TestVNet",
    "type": "virtualNetworks",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Storage/storageAccounts/computeteststore",
    "name": "computeteststore",
    "type": "storageAccounts",
    "location": "westeurope",
    "tags": null
    }
],
"permissions": [
    {
    "actions": [
        "*"
    ],
    "notActions": []
    }
]
}
```

Sie können mit dem umfassenden Befehl `azure network public-ip show` auf weitere Ressourcendetails zugreifen, z.B. auf den vollqualifizierten Domänennamen (FQDN) der Unterdomäne. Die öffentliche IP-Adressressource wurde logisch zugeordnet, aber es wurde noch keine spezielle Adresse zugewiesen. Um eine IP-Adresse zu erhalten, benötigen Sie einen Load Balancer, den wir noch nicht erstellt haben.

```bash
azure network public-ip show TestRG TestPIP --json | jq '.'
```

Ausgabe:

```bash
{
"tags": {},
"publicIpAllocationMethod": "Dynamic",
"dnsSettings": {
    "domainNameLabel": "testsubdomain",
    "fqdn": "testsubdomain.westeurope.cloudapp.azure.com"
},
"idleTimeoutInMinutes": 4,
"provisioningState": "Succeeded",
"etag": "W/"c63154b3-1130-49b9-a887-877d74d5ebc5"",
"id": "/subscriptions/guid/resourceGroups/testrg/providers/Microsoft.Network/publicIPAddresses/testpip",
"name": "testpip",
"location": "westeurope"
}
```

## Erstellen eines Load Balancers und IP-Pools
Wenn Sie einen Load Balancer erstellen, können sie den Datenverkehr auf mehrere virtuelle Computer verteilen. Der Load Balancer gewährleistet zudem Redundanz für Ihre Anwendung, indem mehrere virtuelle Computer ausgeführt werden, die bei der Wartung oder bei hoher Auslastung auf Anforderungen von Benutzern reagieren.

Der Load Balancer wird wie folgt erstellt:

```bash
azure network lb create -g TestRG -n TestLB -l westeurope
```

```bash
azure network lb create -g TestRG -n TestLB -l westeurope
```

Ausgabe:

```bash
info:    Executing command network lb create
+ Looking up the load balancer "TestLB"
+ Creating load balancer "TestLB"
data:    Id                              : /subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB
data:    Name                            : TestLB
data:    Type                            : Microsoft.Network/loadBalancers
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
info:    network lb create command OK
```
Da der Load Balancer ziemlich leer ist, erstellen wir einige IP-Pools. Wir möchten zwei IP-Pools für den Load Balancer erstellen: einen für das Front-End und einen für das Back-End. Der Front-End-IP-Adresspool ist öffentlich sichtbar. Es ist auch der Speicherort, den wir der PIP zuweisen, die wir zuvor erstellt haben. Dann verwenden wir den Back-End-Pool als Speicherort für die virtuellen Computer, mit denen wir eine Verbindung herstellen. Auf diese Weise kann der Datenverkehr über den Load Balancer zu den virtuellen Computern weitergeleitet werden.

Zunächst erstellen wir den Front-End-IP-Pool:

```bash
azure network lb frontend-ip create -g TestRG -l TestLB -n TestFrontEndPool -i TestLBPIP
```

Ausgabe:

```bash
info:    Executing command network lb frontend-ip create
+ Looking up the load balancer "TestLB"
+ Looking up the public ip "TestLBPIP"
+ Updating load balancer "TestLB"
data:    Name                            : TestFrontEndPool
data:    Provisioning state              : Succeeded
data:    Private IP allocation method    : Dynamic
data:    Public IP address id            : /subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestLBPIP
info:    network lb frontend-ip create command OK
```

Beachten Sie die Verwendung der Option `--public-ip-name` zum Übergeben der zuvor erstellten TestLBPIP-Adresse. Die Zuweisung der öffentlichen IP-Adresse zum Load Balancer ermöglicht Ihnen, die virtuellen Computer über das Internet zu erreichen.

Als Nächstes erstellen wir den zweiten IP-Pool für den Back-End-Datenverkehr:

```bash
azure network lb address-pool create -g TestRG -l TestLB -n TestBackEndPool
```

Ausgabe:

```bash
info:    Executing command network lb address-pool create
+ Looking up the load balancer "TestLB"
+ Updating load balancer "TestLB"
data:    Name                            : TestBackEndPool
data:    Provisioning state              : Succeeded
info:    network lb address-pool create command OK
```

Wir können den Load Balancer mit `azure network lb show` überprüfen und die JSON-Ausgabe untersuchen:

```bash
azure network lb show TestRG TestLB --json | jq '.'
```

Ausgabe:

```bash
{
  "etag": "W/"29c38649-77d6-43ff-ab8f-977536b0047c"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [],
  "id": "/subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB",
  "name": "TestLB",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "frontendIPConfigurations": [
    {
      "etag": "W/"29c38649-77d6-43ff-ab8f-977536b0047c"",
      "name": "TestFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestLBPIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "id": "/subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/frontendIPConfigurations/TestFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/"29c38649-77d6-43ff-ab8f-977536b0047c"",
      "name": "TestBackEndPool",
      "provisioningState": "Succeeded",
      "id": "/subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/backendAddressPools/TestBackEndPool"
    }
  ],
  "loadBalancingRules": [],
  "probes": []
}
```

## Erstellen der NAT-Regeln für den Load Balancer
Damit der Datenverkehr durch den Load Balancer fließt, müssen wir NAT-Regeln erstellen, die eingehende oder ausgehende Aktionen angeben. Sie können das verwendete Protokoll angeben und dann wie gewünscht externe Ports internen Ports zuordnen. Wir erstellen für unsere Umgebung einige Regeln, die SSH-Datenverkehr durch den Load Balancer an die virtuellen Computer ermöglichen. Wir richten die TCP-Ports 4222 und 4223 so ein, dass der Datenverkehr auf den virtuellen Computern (die wir später erstellen) an den TCP-Port 22 geleitet wird:

```bash
azure network lb inbound-nat-rule create -g TestRG -l TestLB -n VM1-SSH -p tcp -f 4222 -b 22
```

Ausgabe:

```bash
info:    Executing command network lb inbound-nat-rule create
+ Looking up the load balancer "TestLB"
warn:    Using default enable floating ip: false
warn:    Using default idle timeout: 4
warn:    Using default frontend IP configuration "TestFrontEndPool"
+ Updating load balancer "TestLB"
data:    Name                            : VM1-SSH
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    Frontend port                   : 4222
data:    Backend port                    : 22
data:    Enable floating IP              : false
data:    Idle timeout in minutes         : 4
data:    Frontend IP configuration id    : /subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/frontendIPConfigurations/TestFrontEndPool
info:    network lb inbound-nat-rule create command OK
```

Wiederholen Sie dies für die zweite NAT-Regel für SSH:

```bash
azure network lb inbound-nat-rule create -g TestRG -l TestLB -n VM2-SSH -p tcp -f 4223 -b 22
```

Wir erstellen auch eine NAT-Regel für den TCP-Port 80 und binden die Regel in die IP-Pools ein. Wenn wir die Regel in den IP-Pool einbinden, anstatt die Regel einzeln auf den virtuellen Computern einzubinden, können wir VMs einfach dem IP-Pool hinzufügen oder daraus entfernen. Der Load Balancer passt dann automatisch den Fluss des Datenverkehrs an:

```bash
azure network lb rule create -g TestRG -l TestLB -n WebRule -p tcp -f 80 -b 80 \
     -t TestFrontEndPool -o TestBackEndPool
```

Ausgabe:

```bash
info:    Executing command network lb rule create
+ Looking up the load balancer "TestLB"
warn:    Using default idle timeout: 4
warn:    Using default enable floating ip: false
warn:    Using default load distribution: Default
+ Updating load balancer "TestLB"
data:    Name                            : WebRule
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    Frontend port                   : 80
data:    Backend port                    : 80
data:    Enable floating IP              : false
data:    Load distribution               : Default
data:    Idle timeout in minutes         : 4
data:    Frontend IP configuration id    : /subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/frontendIPConfigurations/TestFrontEndPool
data:    Backend address pool id         : /subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/backendAddressPools/TestBackEndPool
info:    network lb rule create command OK
```

## Erstellen des Integritätstests für den Load Balancer

Mit einem Integritätstest werden die virtuellen Computer hinter dem Load Balancer regelmäßig überprüft, um sicherzustellen, dass sie wie definiert ausgeführt werden und auf Anforderungen reagieren. Wenn dies nicht der Fall ist, werden sie außer Betrieb gesetzt, sodass Benutzer nicht an sie weitergeleitet werden. Sie können benutzerdefinierte Überprüfungen sowie Intervalle und Timeoutwerte für den Integritätstest definieren. Weitere Informationen zu Integritätstests finden Sie unter [Load Balancer-Integritätstest](../load-balancer/load-balancer-custom-probe-overview.md).

```bash
azure network lb probe create -g TestRG -l TestLB -n HealthProbe -p "tcp" -i 15 -c 4
```

Ausgabe:

```bash
info:    Executing command network lb probe create
warn:    Using default probe port: 80
+ Looking up the load balancer "TestLB"
+ Updating load balancer "TestLB"
data:    Name                            : HealthProbe
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    Port                            : 80
data:    Interval in seconds             : 15
data:    Number of probes                : 4
info:    network lb probe create command OK
```

Hier haben wir ein Intervall von 15 Sekunden für unsere Integritätstests angegeben. Bei maximal vier Tests (in 1 Minute) können Fehler auftreten, bevor der Load Balancer feststellt, dass der Host nicht mehr funktionsfähig ist.

## Überprüfen des Load Balancers
Jetzt ist die Load Balancer-Konfiguration abgeschlossen. Folgende Schritte haben Sie ausgeführt:

1. Zunächst erstellt Sie einen Load Balancer.
2. Dann erstellten Sie einen Front-End-IP-Adresspool und wiesen ihm eine öffentliche IP-Adresse zu.
3. Anschließend erstellten Sie einen Back-End-IP-Adresspool, mit dem virtuelle Computer eine Verbindung herstellen können.
4. Danach haben Sie NAT-Regeln für den SSH-Datenverkehr für die virtuellen Computer zu Verwaltungszwecken sowie eine Regel für den TCP-Port 80 für die Web-App erstellt.
5. Schließlich haben Sie einen Integritätstest zur regelmäßigen Überprüfung der virtuellen Computer hinzugefügt. Dieser Integritätstest stellt sicher, dass Benutzer nicht versuchen, auf einen virtuellen Computer zuzugreifen, der nicht mehr funktioniert bzw. keine Inhalte mehr liefert.

So sieht Ihr Load Balancer nun aus:

```bash
azure network lb show -g TestRG -n TestLB --json | jq '.'
```

Ausgabe:

```bash
{
  "etag": "W/"62a7c8e7-859c-48d3-8e76-5e078c5e4a02"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [
    {
      "etag": "W/"62a7c8e7-859c-48d3-8e76-5e078c5e4a02"",
      "name": "VM1-SSH",
      "id": "/subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/inboundNatRules/VM1-SSH",
      "frontendIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/frontendIPConfigurations/TestFrontEndPool"
      },
      "protocol": "Tcp",
      "frontendPort": 4222,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    },
    {
      "etag": "W/"62a7c8e7-859c-48d3-8e76-5e078c5e4a02"",
      "name": "VM2-SSH",
      "id": "/subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/inboundNatRules/VM2-SSH",
      "frontendIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/frontendIPConfigurations/TestFrontEndPool"
      },
      "protocol": "Tcp",
      "frontendPort": 4223,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    }
  ],
  "id": "/subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB",
  "name": "TestLB",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "frontendIPConfigurations": [
    {
      "etag": "W/"62a7c8e7-859c-48d3-8e76-5e078c5e4a02"",
      "name": "TestFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestLBPIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/loadBalancingRules/WebRule"
        }
      ],
      "inboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/inboundNatRules/VM1-SSH"
        },
        {
          "id": "/subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/inboundNatRules/VM2-SSH"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/frontendIPConfigurations/TestFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/"62a7c8e7-859c-48d3-8e76-5e078c5e4a02"",
      "name": "TestBackEndPool",
      "provisioningState": "Succeeded",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/loadBalancingRules/WebRule"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/backendAddressPools/TestBackEndPool"
    }
  ],
  "loadBalancingRules": [
    {
      "etag": "W/"62a7c8e7-859c-48d3-8e76-5e078c5e4a02"",
      "name": "WebRule",
      "provisioningState": "Succeeded",
      "enableFloatingIP": false,
      "id": "/subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/loadBalancingRules/WebRule",
      "frontendIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/frontendIPConfigurations/TestFrontEndPool"
      },
      "backendAddressPool": {
        "id": "/subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/backendAddressPools/TestBackEndPool"
      },
      "protocol": "Tcp",
      "loadDistribution": "Default",
      "frontendPort": 80,
      "backendPort": 80,
      "idleTimeoutInMinutes": 4
    }
  ],
  "probes": [
    {
      "etag": "W/"62a7c8e7-859c-48d3-8e76-5e078c5e4a02"",
      "name": "HealthProbe",
      "provisioningState": "Succeeded",
      "numberOfProbes": 4,
      "intervalInSeconds": 15,
      "port": 80,
      "protocol": "Tcp",
      "id": "/subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/probes/HealthProbe"
    }
  ]
}
```

## Erstellen einer Netzwerkschnittstellenkarte zur Verwendung mit der Linux-VM

 NICs sind programmgesteuert verfügbar, da Sie Regeln zu ihrer Verwendung anwenden können. Sie können auch mehrere Regeln anwenden. Mit dem folgenden Befehl `azure network nic create` wird die Netzwerkkarte in den Last-Back-End-IP-Pool eingebunden und der NAT-Regel für das Zulassen von SSH-Datenverkehr zugeordnet. Dazu müssen Sie anstelle von `<GUID>` die Abonnement-ID Ihres Azure-Abonnements angeben:

```bash
azure network nic create -g TestRG -n LB-NIC1 -l westeurope --subnet-vnet-name TestVNet --subnet-name FrontEnd \
     -d /subscriptions/<GUID>/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/backendAddressPools/TestBackEndPool \
     -e /subscriptions/<GUID>/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/inboundNatRules/VM1-SSH
```

Ausgabe:

```bash
info:    Executing command network nic create
+ Looking up the subnet "FrontEnd"
+ Looking up the network interface "LB-NIC1"
+ Creating network interface "LB-NIC1"
data:    Id                              : /subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/LB-NIC1
data:    Name                            : LB-NIC1
data:    Type                            : Microsoft.Network/networkInterfaces
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Enable IP forwarding            : false
data:    IP configurations:
data:      Name                          : Nic-IP-config
data:      Provisioning state            : Succeeded
data:      Private IP address            : 192.168.1.4
data:      Private IP allocation method  : Dynamic
data:      Subnet                        : /subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
data:      Load balancer backend address pools:
data:        Id                          : /subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/backendAddressPools/TestBackEndPool
data:      Load balancer inbound NAT rules:
data:        Id                          : /subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/inboundNatRules/VM1-SSH
data:
info:    network nic create command OK
```

Sie können die Details sehen, wenn Sie die Ressource direkt untersuchen. Untersuchen Sie die Ressource mithilfe des `azure network nic show`-Befehls:

```bash
azure network nic show TestRG LB-NIC1 --json | jq '.'
```

Ausgabe:

```bash
{
  "etag": "W/"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a"",
  "provisioningState": "Succeeded",
  "id": "/subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/LB-NIC1",
  "name": "LB-NIC1",
  "type": "Microsoft.Network/networkInterfaces",
  "location": "westeurope",
  "ipConfigurations": [
    {
      "etag": "W/"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a"",
      "id": "/subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/LB-NIC1/ipConfigurations/Nic-IP-config",
      "loadBalancerBackendAddressPools": [
        {
          "id": "/subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/backendAddressPools/TestBackEndPool"
        }
      ],
      "loadBalancerInboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/inboundNatRules/VM1-SSH"
        }
      ],
      "privateIPAddress": "192.168.1.4",
      "privateIPAllocationMethod": "Dynamic",
      "subnet": {
        "id": "/subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd"
      },
      "provisioningState": "Succeeded",
      "name": "Nic-IP-config"
    }
  ],
  "dnsSettings": {
    "appliedDnsServers": [],
    "dnsServers": []
  },
  "enableIPForwarding": false,
  "resourceGuid": "a20258b8-6361-45f6-b1b4-27ffed28798c"
}
```

Jetzt erstellen wir die zweite NIC und binden sie erneut in unseren Back-End-IP-Adresspool ein. Dieses Mal lässt die zweite NAT-Regel SSH-Datenverkehr zu:

```bash
azure network nic create -g TestRG -n LB-NIC2 -l westeurope --subnet-vnet-name TestVNet --subnet-name FrontEnd \
    -d  /subscriptions/<GUID>/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/backendAddressPools/TestBackEndPool \
    -e /subscriptions/<GUID>/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/inboundNatRules/VM2-SSH
```

## Erstellen einer Netzwerksicherheitsgruppe und von Regeln

Als Nächstes erstellen wir eine NSG und die eingehenden Regeln, mit denen der Zugriff auf die Netzwerkschnittstellenkarte gesteuert wird.

```bash
azure network nsg create TestRG TestNSG westeurope
```

Wir fügen die eingehende Regel für die NSG hinzu, um eingehende Verbindungen für Port 22 zuzulassen (für die SSH-Unterstützung):

```bash
azure network nsg rule create --protocol tcp --direction inbound --priority 1000 \
    --destination-port-range 22 --access allow TestRG TestNSG SSHRule
```

```bash
azure network nsg rule create --protocol tcp --direction inbound --priority 1001 \
    --destination-port-range 80 --access allow -g TestRG -a TestNSG -n HTTPRule
```

> [AZURE.NOTE] Die eingehende Regel ist ein Filter für eingehende Netzwerkverbindungen. In diesem Beispiel binden wir die NSG an die virtuelle NIC der virtuellen Computer. Dies bedeutet, dass alle Anforderungen an Port 22 über die NIC auf dem virtuellen Computer übergeben werden. Dies ist eine Eingangsregel über eine Netzwerkverbindung und nicht über einen Endpunkt wie in klassischen Bereitstellungen. Zum Öffnen eines Ports müssen Sie die Festlegung von `--source-port-range` auf „*“ (Standardwert) beibehalten, um eingehende Anforderungen von **jedem** anfordernden Port zu akzeptieren. Ports sind in der Regel dynamisch.

## Binden an die Netzwerkkarte

Binden der NSG an die Netzwerkkarten:

```bash
azure network nic set -g TestRG -n LB-NIC1 -o TestNSG
```

```bash
azure network nic set -g TestRG -n LB-NIC2 -o TestNSG
```

## Verfügbarkeitsgruppe erstellen
Mit Verfügbarkeitsgruppen können die virtuellen Computer auf Fehlerdomänen und Upgradedomänen verteilt werden. Wir erstellen eine Verfügbarkeitsgruppe für die virtuellen Computer:

```bash
azure availset create -g TestRG -n TestAvailSet -l westeurope
```

Durch Fehlerdomänen wird eine Gruppe virtueller Computer definiert, die eine Stromquelle und einen Netzwerkswitch gemeinsam nutzen. Die innerhalb der Verfügbarkeitsgruppe konfigurierten virtuellen Computer werden standardmäßig auf bis zu drei Fehlerdomänen verteilt. Dadurch wirkt sich ein Hardwareproblem in einer dieser Fehlerdomänen nicht auf jeden virtuellen Computer aus, auf dem Ihre Anwendung ausgeführt wird. Azure verteilt virtuelle Computer automatisch auf die Fehlerdomänen, wenn sie in einer Verfügbarkeitsgruppe platziert werden.

Upgradedomänen definieren Gruppen virtueller Computer und die zugrunde liegende physische Hardware, die gleichzeitig neu gestartet werden können. Während einer geplanten Wartung werden die Upgradedomänen möglicherweise nicht der Reihe nach neu gestartet. Es wird aber jeweils nur eine Upgradedomäne neu gestartet. Azure verteilt Ihre virtuellen Computer auch hier automatisch auf die Upgradedomänen, wenn sie in einer Verfügbarkeitsgruppe platziert werden.

Weitere Informationen finden Sie unter [Verwalten der Verfügbarkeit virtueller Computer](./virtual-machines-linux-manage-availability.md).

## Erstellen der Linux-VMs

Sie haben die Speicher- und Netzwerkressourcen zur Unterstützung von über das Internet erreichbaren virtuellen Computern erstellt. Wir erstellen jetzt diese virtuellen Computer und schützen sie mit einem SSH-Schlüssel ohne Kennwort. In diesem Fall erstellen wir eine Ubuntu-VM basierend auf dem aktuellen LTS-Stand. Wir ermitteln diese Imageinformationen per `azure vm image list`. Dies wird unter [Suchen nach Azure VM-Images](virtual-machines-linux-cli-ps-findimage.md) beschrieben.

Wir haben ein Image mithilfe des Befehls `azure vm image list westeurope canonical | grep LTS` ausgewählt. In diesem Fall verwenden wir `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`. Für das letzte Feld übergeben wir `latest`, sodass in Zukunft immer die letzten Builds abgerufen werden. (Die Zeichenfolge, die wir verwenden, ist `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`).

Der nächste Schritt ist allen Benutzern vertraut, die mit **ssh-keygen -t rsa -b 2048** bereits ein Paar aus einem öffentlichen und einem privaten SSH-RSA-Schlüssel unter Linux oder Mac erstellt haben. Wenn sich in Ihrem `~/.ssh`-Verzeichnis keine Zertifikatsschlüsselpaare befinden, können Sie sie erstellen:

- Automatisch mithilfe der Option `azure vm create --generate-ssh-keys`.
- Manuell mithilfe [der Anweisungen zur Selbsterstellung](virtual-machines-linux-mac-create-ssh-keys.md).

Alternativ können Sie mit der --admin-Kennwortmethode Ihre SSH-Verbindungen nach der Erstellung des virtuellen Computers authentifizieren. Diese Methode ist in der Regel weniger sicher.

Wir erstellen den virtuellen Computer, indem wir alle Ressourcen und Informationen mit dem Befehl `azure vm create` zusammenfassen:

```bash
azure vm create \            
    --resource-group TestRG \
    --name TestVM1 \
    --location westeurope \
    --os-type linux \
    --availset-name TestAvailSet \
    --nic-name LB-NIC1 \
    --vnet-name TestVnet \
    --vnet-subnet-name FrontEnd \
    --storage-account-name computeteststore \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username ops
```

Ausgabe:

```bash
info:    Executing command vm create
+ Looking up the VM "TestVM1"
info:    Verifying the public key SSH file: /home/ahmet/.ssh/id_rsa.pub
info:    Using the VM Size "Standard_DS1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Looking up the storage account computeteststore
+ Looking up the availability set "TestAvailSet"
info:    Found an Availability set "TestAvailSet"
+ Looking up the NIC "LB-NIC1"
info:    Found an existing NIC "LB-NIC1"
info:    Found an IP configuration with virtual network subnet id "/subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd" in the NIC "LB-NIC1"
info:    This is an NIC without publicIP configured
info:    The storage URI 'https://computeteststore.blob.core.windows.net/' will be used for boot diagnostics settings, and it can be overwritten by the parameter input of '--boot-diagnostics-storage-uri'.
info:    vm create command OK
```

Anschließend können Sie mit Ihren standardmäßigen SSH-Schlüsseln eine Verbindung mit dem virtuellen Computer herstellen. Stellen Sie sicher, dass Sie den entsprechenden Port angeben, da die Weiterleitung über den Load Balancer erfolgt. (Für unseren ersten virtuellen Computer richten wir die NAT-Regel zum Weiterleiten von Port 4222 an den virtuellen Computer ein):

```bash
 ssh ops@testlb.westeurope.cloudapp.azure.com -p 4222
```

Ausgabe:

```bash
The authenticity of host '[testlb.westeurope.cloudapp.azure.com]:4222 ([xx.xx.xx.xx]:4222)' can't be established.
ECDSA key fingerprint is 94:2d:d0:ce:6b:fb:7f:ad:5b:3c:78:93:75:82:12:f9.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '[testlb.westeurope.cloudapp.azure.com]:4222,[xx.xx.xx.xx]:4222' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 16.04.1 LTS (GNU/Linux 4.4.0-34-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

ops@TestVM1:~$
```

Erstellen Sie nun den zweiten virtuellen Computer auf die gleiche Weise:

```bash
azure vm create \            
    --resource-group TestRG \
    --name TestVM2 \
    --location westeurope \
    --os-type linux \
    --availset-name TestAvailSet \
    --nic-name LB-NIC2 \
    --vnet-name TestVnet \
    --vnet-subnet-name FrontEnd \
    --storage-account-name computeteststore \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username ops
```

Jetzt können Sie mit dem Befehl `azure vm show testrg testvm` die erstellten Elemente untersuchen. Nun werden die virtuellen Ubuntu-Computer, bei denen Sie sich nur mit Ihrem SSH-Schlüsselpaar anmelden können (weil Kennwörter deaktiviert sind), hinter einem Load Balancer in Azure ausgeführt. Sie können „nginx“ oder „httpd“ installieren und eine Web-App bereitstellen. Zudem können Sie den Datenverkehrsfluss durch den Load Balancer an beide virtuellen Computer sehen.

```bash
azure vm show TestRG TestVM1
```

Ausgabe:

```bash
info:    Executing command vm show
+ Looking up the VM "TestVM1"
+ Looking up the NIC "LB-NIC1"
data:    Id                              :/subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/TestVM1
data:    ProvisioningState               :Succeeded
data:    Name                            :TestVM1
data:    Location                        :westeurope
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :canonical
data:        Offer                       :UbuntuServer
data:        Sku                         :16.04.0-LTS
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clib45a8b650f4428a1-os-1471973896525
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://computeteststore.blob.core.windows.net/vhds/clib45a8b650f4428a1-os-1471973896525.vhd
data:
data:    OS Profile:
data:      Computer Name                 :TestVM1
data:      User Name                     :ops
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-24-D4-AA
data:          Provisioning State        :Succeeded
data:          Name                      :LB-NIC1
data:          Location                  :westeurope
data:
data:    AvailabilitySet:
data:      Id                            :/subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Compute/availabilitySets/TESTAVAILSET
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://computeteststore.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm show command OK

```


## Exportieren der Umgebung als Vorlage
Nachdem Sie nun diese Umgebung erstellt haben, möchten Sie vielleicht eine weitere Entwicklungsumgebung mit den gleichen Parametern oder eine entsprechende Produktionsumgebung erstellen. Der Resource Manager verwendet die JSON-Vorlagen, die alle Parameter für Ihre Umgebung definieren. Sie erstellen ganze Umgebungen durch Verweisen auf diese JSON-Vorlage. Sie können [JSON-Vorlagen entweder manuell erstellen](../resource-group-authoring-templates.md) oder einfach eine vorhandene Umgebung exportieren, um eine JSON-Vorlage zu erstellen:

```bash
azure group export TestRG
```

Dieser Befehl erstellt die `TestRG.json`-Datei in Ihrem aktuellen Arbeitsverzeichnis. Wenn Sie eine Umgebung aus dieser Vorlage erstellen, werden Sie aufgefordert, alle Ressourcennamen, beispielsweise die Namen für Load Balancer, Netzwerkschnittstellen oder virtuelle Computer einzugeben. Sie können diese Namen in Ihre Vorlagendatei eintragen, indem Sie die Parameter `-p` oder `--includeParameterDefaultValue` dem Befehl `azure group export` hinzufügen, der zuvor gezeigt wurde. Bearbeiten Sie die JSON-Vorlage zum Angeben der Ressourcennamen, oder [erstellen Sie eine Datei „parameters.json“](../resource-group-authoring-templates.md#parameters), die die Ressourcennamen angibt.

Erstellen Sie wie folgt eine Umgebung aus Ihrer Vorlage:

```bash
azure group deployment create -f TestRG.json -g NewRGFromTemplate
```

Möglicherweise möchten Sie [weitere Informationen zum Bereitstellen von Vorlagen](../resource-group-template-deploy-cli.md) lesen. Erfahren Sie, wie Sie Umgebungen unter Verwendung der Parameterdatei schrittweise aktualisieren und von einem zentralen Speicherort aus auf Vorlagen zugreifen.

## Nächste Schritte

Sie können jetzt beginnen, mit mehreren Netzwerkkomponenten und VMs zu arbeiten. Sie können diese Beispielumgebung nutzen, um Ihre Anwendung mithilfe der hier eingeführten zentralen Komponenten zu erstellen.

<!---HONumber=AcomDC_0914_2016-->