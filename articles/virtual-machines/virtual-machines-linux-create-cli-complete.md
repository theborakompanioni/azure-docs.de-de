<properties
   pageTitle="Erstellen einer Linux-VM von Grund auf mit der Azure-Befehlszeilenschnittstelle | Microsoft Azure"
   description="Erfahren Sie, wie Sie die Komponenten Linux-VM, Speicher, Virtual Network und Subnetz, NIC, öffentliche IP-Adresse und Netzwerksicherheitsgruppe von Grund auf erstellen, indem Sie die Azure-Befehlszeilenschnittstelle verwenden."
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="iainfoulds"
   manager="squillace"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="04/29/2016"
   ms.author="iainfou"/>

# Erstellen einer Linux-VM von Grund auf mit der Azure-Befehlszeilenschnittstelle

Um einen virtuellen Linux-Computer zu erstellen, benötigen Sie [die Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md) im Resource Manager-Modus (`azure config mode arm`)und ein JSON-Analysetool. In diesem Dokument verwenden wir [jq](https://stedolan.github.io/jq/).

## Schnellbefehle

Erstellen der Ressourcengruppe

```bash
azure group create TestRG -l westeurope
```

Überprüfen der Ressourcengruppe mithilfe des JSON-Parsers

```bash
azure group show TestRG --json | jq '.'
```

Erstellen des Speicherkontos

```bash
azure storage account create -g TestRG -l westeurope --type GRS computeteststore
```

Überprüfen des Speichers mithilfe des JSON-Parsers

```bash
azure storage account show -g TestRG computeteststore --json | jq '.'
```

Erstellen des virtuellen Netzwerks

```bash
azure network vnet create -g TestRG -n TestVNet -a 192.168.0.0/16 -l westeurope
```

Erstellen des Subnetzes

```bash
azure network vnet subnet create -g TestRG -e TestVNet -n FrontEnd -a 192.168.1.0/24
```

Überprüfen des VNET und Subnetzes mithilfe des JSON-Parsers

```bash
azure network vnet show TestRG TestVNet --json | jq '.'
```

Erstellen einer öffentlichen IP

```bash
azure network public-ip create -g TestRG -n TestLBPIP -l westeurope -d testlb -a static -i 4
```

Erstellen des Load Balancers

```bash
azure network lb create -g TestRG -n TestLB -l westeurope
```

Erstellen eines Front-End-IP-Pools für den Load Balancer und Zuordnen der öffentlichen IP

```bash
azure network lb frontend-ip create -g TestRG -l TestLB -n TestFrontEndPool -i TestLBPIP
```

Erstellen des Back-End-IP-Pools für den Load Balancer

```bash
azure network lb address-pool create -g TestRG -l TestLB -n TestBackEndPool
```

Erstellen der eingehenden NAT-Regeln (SSH) für den Load Balancer

```bash
azure network lb inbound-nat-rule create -g TestRG -l TestLB -n VM1-SSH -p tcp -f 4222 -b 22
azure network lb inbound-nat-rule create -g TestRG -l TestLB -n VM2-SSH -p tcp -f 4223 -b 22
```

Erstellen der eingehenden NAT-Regeln (Web) für den Load Balancer

```bash
azure network lb rule create -g TestRG -l TestLB -n WebRule -p tcp -f 80 -b 80 \
     -t TestFrontEndPool -o TestBackEndPool
```

Erstellen des Integritätstests für den Load Balancer

```bash
azure network lb probe create -g TestRG -l TestLB -n HealthProbe -p "http" -f healthprobe.aspx -i 15 -c 4
```

Überprüfen des Load Balancers, der IP-Pools und der NAT-Regeln mithilfe des JSON-Parsers

```bash
azure network lb show -g TestRG -n TestLB --json | jq '.'
```

Erstellen der ersten Netzwerkkarte

```bash
azure network nic create -g TestRG -n LB-NIC1 -l westeurope --subnet-vnet-name TestVNet --subnet-name FrontEnd
    -d "/subscriptions/########-####-####-####-############/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/backendAddressPools/TestBackEndPool"
    -e "/subscriptions/########-####-####-####-############/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/inboundNatRules/VM1-SSH"
```

Erstellen der zweiten Netzwerkkarte

```bash
azure network nic create -g TestRG -n LB-NIC2 -l westeurope --subnet-vnet-name TestVNet --subnet-name FrontEnd
    -d "/subscriptions/########-####-####-####-############/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/backendAddressPools/TestBackEndPool"
    -e "/subscriptions/########-####-####-####-############/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/inboundNatRules/VM2-SSH"
```

Überprüfen der Netzwerkkarten mithilfe des JSON-Parsers

```bash
azure network nic show TestRG LB-NIC1 --json | jq '.'
azure network nic show TestRG LB-NIC2 --json | jq '.'
```

Erstellen der NSG

```bash
azure network nsg create -g TestRG -n TestNSG -l westeurope
```

Hinzufügen der eingehenden Regeln für die NSG

```bash
azure network nsg rule create --protocol tcp --direction inbound --priority 1000 \
    --destination-port-range 22 --access allow -g TestRG -a TestNSG -n SSHRule
azure network nsg rule create --protocol tcp --direction inbound --priority 1001 \
    --destination-port-range 80 --access allow -g TestRG -a TestNSG -n HTTPRule
```

Überprüfen der NSG und der eingehenden Regeln mithilfe des JSON-Parsers

```bash
azure network nsg show -g TestRG -n TestNSG --json | jq '.'
```

Binden der NSG an die Netzwerkkarten

```bash
azure network nic set -g TestRG -n LB-NIC1 -o TestNSG
azure network nic set -g TestRG -n LB-NIC2 -o TestNSG
```

Erstellen der Verfügbarkeitsgruppe

```bash
azure availset create -g TestRG -n TestAvailSet -l westeurope
```

Erstellen des ersten virtuellen Linux-Computers

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
    --image-urn canonical:UbuntuServer:14.04.4-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username ops
```

Erstellen des zweiten virtuellen Linux-Computers

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
    --image-urn canonical:UbuntuServer:14.04.4-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username ops
```

Überprüfen aller erstellten Elemente mithilfe des JSON-Parsers

```bash
azure vm show -g TestRG -n TestVM1 --json | jq '.'
azure vm show -g TestRG -n TestVM2 --json | jq '.'
```

## Ausführliche exemplarische Vorgehensweise

### Einführung

In diesem Artikel wird eine Bereitstellung mit zwei virtuellen Linux-Computern hinter einem Load Balancer erstellt. Sie werden Befehl für Befehl durch die gesamte grundlegende Bereitstellung geführt, bis Sie über funktionierende, sichere virtuelle Linux-Computer verfügen, mit denen Sie über das Internet von jedem Ort aus eine Verbindung herstellen können.

Im Laufe dieses Vorgangs lernen Sie die Abhängigkeitshierarchie kennen, die Sie mit dem Resource Manager-Bereitstellungsmodell erhalten, sowie die damit verbundene hohe Leistungsfähigkeit. Nachdem Sie über den Aufbau des Systems Bescheid wissen, können Sie das System viel schneller neu erstellen, indem Sie direktere Befehle der Azure-Befehlszeilenschnittstelle verwenden. ([Hier](virtual-machines-linux-quick-create-cli.md) finden Sie Informationen zu einer sehr ähnlichen Bereitstellung, bei der der Befehl `azure vm quick-create` genutzt wird.) Sie können sich auch darüber informieren, wie Sie vollständige Netzwerk- und Anwendungsbereitstellungen entwerfen, automatisieren und mit [Azure Resource Manager-Vorlagen](../resource-group-authoring-templates.md) aktualisieren. Wenn Sie sehen, wie die Teile Ihrer Bereitstellung zusammenpassen, wird die Erstellung von Vorlagen für deren Automatisierung vereinfacht.

Wir erstellen ein einfaches Netzwerk und einen Load Balancer mit zwei virtuellen Computern, die für die Entwicklung und einfache Computevorgänge gut geeignet sind, und wir beschreiben dabei jeweils die einzelnen Schritte. Anschließend können Sie sich mit komplexeren Netzwerken und Bereitstellungen beschäftigen.

## Erstellen einer Ressourcengruppe und Auswählen von Standorten für die Bereitstellung

Azure-Ressourcengruppen sind logische Bereitstellungsentitäten, die Konfigurationsdaten und andere Metadaten enthalten, um die logische Verwaltung von Ressourcenbereitstellungen zu ermöglichen.

```bash
azure group create TestRG westeurope
```

Ausgabe

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

## Speicherkonto erstellen

Sie benötigen unter anderem Speicherkonten für Ihre VM-Datenträger und für alle zusätzlichen Datenträger, die Sie hinzufügen möchten. Anders ausgedrückt: Sie erstellen Speicherkonten normalerweise immer direkt nach der Erstellung von Ressourcengruppen.

Hier verwenden wir den Befehl `azure storage account create` und übergeben den Speicherort des Kontos, die Ressourcengruppe, mit der es gesteuert wird, und den Typ der gewünschten Speicherunterstützung.

```bash
azure storage account create \  
--location westeurope \
--resource-group TestRG \
--type GRS \
computeteststore
```

Ausgabe

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

Wir nutzen das Tool [jq](https://stedolan.github.io/jq/) (Sie können **jsawk** oder eine andere bevorzugte Sprachbibliothek für die JSON-Analyse verwenden) zusammen mit der Option `--json` der Azure-Befehlszeilenschnittstelle, um die Ressourcengruppe mit dem Befehl `azure group show` zu untersuchen.

```bash
azure group show TestRG --json | jq                                                                                      
```


Ausgabe

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

Zum Untersuchen des Speicherkontos mit der Befehlszeilenschnittstelle müssen Sie zuerst die Kontonamen und Schlüssel festlegen, indem Sie eine Variante des unten angegebenen Befehls verwenden. Ersetzen Sie den Namen des Speicherkontos in diesem Artikel durch den Namen Ihres Speicherkontos.

```
AZURE_STORAGE_CONNECTION_STRING="$(azure storage account connectionstring show computeteststore --resource-group testrg --json | jq -r '.string')"
```

Sie können die Speicherinformationen dann leicht anzeigen:

```bash
azure storage container list
```

Ausgabe

```bash
info:    Executing command storage container list
+ Getting storage containers
data:    Name  Public-Access  Last-Modified
data:    ----  -------------  -----------------------------
data:    vhds  Off            Sun, 27 Sep 2015 19:03:54 GMT
info:    storage container list command OK
```

## Erstellen des Virtual Network und des Subnetzes

Sie müssen ein Azure Virtual Network und ein Subnetz erstellen, in dem Sie die virtuellen Computer installieren können.

```bash
azure network vnet create -g TestRG -n TestVNet -a 192.168.0.0/16 -l westeurope
```

Ausgabe

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

Wir erstellen die Ressourcen, indem wir die Option „--json“ von `azure group show` und **jq** verwenden. Wir verfügen nun über eine `storageAccounts`-Ressource und eine `virtualNetworks`-Ressource.

```bash
azure group show TestRG --json | jq '.'
```

Ausgabe

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

Jetzt erstellen wir ein Subnetz im virtuellen Netzwerk `TestVnet`, in dem die virtuellen Computer bereitgestellt werden. Wir verwenden den Befehl `azure network vnet subnet create` zusammen mit den Ressourcen, die wir bereits erstellt haben: mit der Ressourcengruppe `TestRG` und dem virtuellen Netzwerk `TestVNet`. Außerdem fügen wir den Subnetznamen `FrontEnd` und das Subnetzadressen-Präfix `192.168.1.0/24` wie folgt hinzu:

```bash
azure network vnet subnet create -g TestRG -e TestVNet -n FrontEnd -a 192.168.1.0/24
```

Ausgabe

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

Da das Subnetz logisch im virtuellen Netzwerk angeordnet ist, ermitteln wir die Subnetzinformationen mit einem etwas anderen Befehl: `azure network vnet show`. Die JSON-Ausgabe wird aber weiterhin mit **jq** untersucht.

```bash
azure network vnet show TestRG TestVNet --json | jq '.'
```

Ausgabe

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

## Erstellen der öffentlichen IP-Adresse (PIP)

Jetzt erstellen wir Ihre öffentliche IP-Adresse (PIP), die dem Load Balancer zugewiesen wird und über die Sie mit dem Befehl `azure network public-ip create` eine Verbindung mit den virtuellen Computern über das Internet herstellen können. Da die Standardeinstellung eine dynamische Adresse ist, erstellen wir in der Domäne **cloudapp.azure.com** mit der Option `-d testsubdomain` einen benannten DNS-Eintrag.

```bash
azure network public-ip create -d testsubdomain TestRG TestPIP westeurope
```

Ausgabe

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

Dies ist außerdem eine Ressource der obersten Ebene, die Sie mit `azure group show` anzeigen können.

```bash
azure group show TestRG --json | jq '.'
```

Ausgabe

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

Wie immer können Sie mit dem umfassenderen Befehl `azure network public-ip show` auch auf weitere Ressourcendetails zugreifen, z.B. auf den vollqualifizierten Domänennamen (FQDN) der Unterdomäne. Beachten Sie, dass die öffentliche IP-Adressressource logisch zugeordnet wurde, aber noch keine spezielle Adresse zugewiesen wurde. Hierfür benötigen Sie einen Load Balancer, den wir noch nicht erstellt haben.

```bash
azure network public-ip show TestRG TestPIP --json | jq '.'
```

Ausgabe

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

## Erstellen des Load Balancers und der IP-Pools
Wenn Sie einen Load Balancer erstellen, können Sie den Datenverkehr auf mehrere virtuelle Computer verteilen, z.B. beim Ausführen von Webanwendungen. Der Load Balancer gewährleistet zudem Redundanz für Ihre Anwendung, indem mehrere virtuelle Computer ausgeführt werden, die bei der Wartung oder bei hoher Auslastung auf Anforderungen von Benutzern reagieren.

Der Load Balancer wird wie folgt erstellt:

```bash
azure network lb create -g TestRG -n TestLB -l westeurope
```

```bash
azure network lb create -g TestRG -n TestLB -l westeurope
```

Ausgabe

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
Da der Load Balancer leer ist, erstellen wir einige IP-Pools. Wir möchten zwei IP-Pools für den Load Balancer erstellen: einen für das Front-End und einen für das Back-End. Der Front-End-IP-Pool ist öffentlich sichtbar. In diesem IP-Pool weisen wir die zuvor erstellte PIP zu. Der Back-End-IP-Pool wird dann für die Verbindung mit den virtuellen Computern verwendet, sodass der Datenverkehr durch den Load Balancer an sie geleitet wird.

Zunächst erstellen wir den Front-End-IP-Pool:

```bash
azure network lb frontend-ip create -g TestRG -l TestLB -n TestFrontEndPool -i TestLBPIP
```

Ausgabe

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

Beachten Sie die Verwendung des Switchs `--public-ip-name` zum Übergeben der zuvor erstellten TestLBPIP. Damit wird die öffentliche IP-Adresse dem Load Balancer zugewiesen, sodass die virtuellen Computer über das Internet erreicht werden können.

Als Nächstes erstellen wir den zweiten IP-Pool für den Back-End-Datenverkehr:

```bash
azure network lb address-pool create -g TestRG -l TestLB -n TestBackEndPool
```

Ausgabe

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

Ausgabe

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
Damit der Datenverkehr tatsächlich durch den Load Balancer fließt, müssen wir NAT-Regeln erstellen, die eingehende oder ausgehende Aktionen angeben. Sie können das verwendete Protokoll angeben und dann wie gewünscht externe Ports internen Ports zuordnen. Wir erstellen für unsere Umgebung einige Regeln, die SSH-Datenverkehr durch den Load Balancer an die virtuellen Computer ermöglichen. Wir richten die TCP-Ports 4222 und 4223 so ein, dass der Datenverkehr an den TCP-Port 22 auf den virtuellen Computern geleitet wird (die wir später erstellen):

```bash
azure network lb inbound-nat-rule create -g TestRG -l TestLB -n VM1-SSH -p tcp -f 4222 -b 22
```

Ausgabe

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

Wir erstellen auch eine NAT-Regel für den TCP-Port 80 und binden die Regel in die IP-Pools ein. Da die Regel nicht einzeln in die virtuellen Computer eingebunden wird, können virtuelle Computer im IP-Pool einfach hinzugefügt und entfernt werden, und der Load Balancer passt automatisch den Fluss des Datenverkehrs an.

```bash
azure network lb rule create -g TestRG -l TestLB -n WebRule -p tcp -f 80 -b 80 \
     -t TestFrontEndPool -o TestBackEndPool
```

Ausgabe

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

Mit einem Integritätstest werden die virtuellen Computer hinter dem Load Balancer regelmäßig überprüft, um sicherzustellen, dass sie wie definiert ausgeführt werden und auf Anforderungen reagieren. Wenn dies nicht der Fall ist, werden sie außer Betrieb gesetzt, sodass Benutzer nicht an sie weitergeleitet werden. Sie können benutzerdefinierte Überprüfungen sowie Intervalle und Timeoutwerte für den Integritätstest definieren. Weitere Informationen zu Integritätstests finden Sie unter [Load Balancer-Tests](../load-balancer/load-balancer-custom-probe-overview.md).

```bash
azure network lb probe create -g TestRG -l TestLB -n HealthProbe -p "http" -f healthprobe.aspx -i 15 -c 4
```

Ausgabe

```bash
info:    Executing command network lb probe create
warn:    Using default probe port: 80
+ Looking up the load balancer "TestLB"
+ Updating load balancer "TestLB"
data:    Name                            : HealthProbe
data:    Provisioning state              : Succeeded
data:    Protocol                        : Http
data:    Port                            : 80
data:    Interval in seconds             : 15
data:    Number of probes                : 4
info:    network lb probe create command OK
```

Hier haben wir ein Intervall von 15 Sekunden für die Integritätstests angegeben. Maximal vier Tests (1 Minute) können ausfallen, bevor der Load Balancer feststellt, dass der Host nicht mehr funktionsfähig ist.

## Überprüfen des Load Balancers
Damit ist die Load Balancer-Konfiguration abgeschlossen. Sie haben einen Load Balancer erstellt, dann einen Front-End-IP-Pool erstellt und ihm eine öffentliche IP zugewiesen, und schließlich haben Sie einen Back-End-IP-Pool erstellt, mit dem die virtuellen Computer verbunden werden. Als Nächstes haben Sie NAT-Regeln für den SSH-Datenverkehr für die virtuellen Computer für Verwaltungszwecke sowie eine Regel für den TCP-Port 80 für die Web-App erstellt. Um sicherzustellen, dass Benutzer nicht auf einen virtuellen Computer zugreifen, der nicht mehr funktionsfähig ist und keine Inhalte mehr enthält, haben Sie schließlich einen Integritätstest hinzugefügt, um die virtuellen Computer in regelmäßigen Abständen zu überprüfen.

Sehen wir uns nun den Load Balancer an:

```bash
azure network lb show -g TestRG -n TestLB --json | jq '.'
```

Ausgabe

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
      "id": "/subscriptions/guid/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/probes/HealthProbe",
      "protocol": "Http",
      "port": 80,
      "intervalInSeconds": 15,
      "numberOfProbes": 4,
      "requestPath": "healthprobe.aspx",
      "provisioningState": "Succeeded",
      "name": "HealthProbe"
    }
  ]
}
```

## Erstellen einer Netzwerkschnittstellenkarte zur Verwendung mit der Linux-VM

Netzwerkschnittstellen sind auch programmgesteuert verfügbar, und Sie können Regeln für ihre Verwendung anwenden und mehr als eine verwenden. Mit dem folgenden `azure network nic create`-Befehl wird die Netzwerkkarte in den Back-End-IP-Pool eingebunden und der NAT-Regel für den SSH-Datenverkehr zugeordnet. Dazu müssen Sie anstelle von `<GUID>` die Abonnement-ID Ihres Azure-Abonnements angeben:

```bash
azure network nic create -g TestRG -n LB-NIC1 -l westeurope --subnet-vnet-name TestVNet --subnet-name FrontEnd -d
```

Ausgabe

```bash 
/subscriptions/<GUID>/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/backendAddressPools/TestBackEndPool -e /subscriptions/<GUID>/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/inboundNatRules/VM1-SSH
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

Sie können die Details sehen, wenn Sie die Ressource mit dem Befehl `azure network nic show` direkt untersuchen.

```bash
azure network nic show TestRG LB-NIC1 --json | jq '.'
```

Ausgabe

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

Wir erstellen nun die zweite Netzwerkkarte und binden auch diese in den Back-End-IP-Pool ein und ordnen sie der zweiten NAT-Regel für den SSH-Datenverkehr zu:

```bash
azure network nic create -g TestRG -n LB-NIC2 -l westeurope --subnet-vnet-name TestVNet --subnet-name FrontEnd -d
```

Ausgabe

```bash
 /subscriptions/<GUID>/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/backendAddressPools/TestBackEndPool -e /subscriptions/<GUID>/resourceGroups/TestRG/providers/Microsoft.Network/loadBalancers/TestLB/inboundNatRules/VM2-SSH
```

## Erstellen der Netzwerksicherheitsgruppe und der Regeln

Als Nächstes erstellen wir eine Netzwerksicherheitsgruppe (NSG) und die eingehenden Regeln, mit denen der Zugriff auf die Netzwerkschnittstellenkarte gesteuert wird.

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

> [AZURE.NOTE] Die eingehende Regel ist ein Filter für eingehende Netzwerkverbindungen. In diesem Beispiel binden wir die NSG an die virtuelle Netzwerkschnittstellenkarte (NIC) der virtuellen Computer. Dies bedeutet, dass alle Anforderungen an Port 22 über die NIC auf dem virtuellen Computer übergeben werden. Da dies eine Regel für eine Netzwerkverbindung ist – und kein Endpunkt wie bei klassischen Bereitstellungen –, müssen Sie zum Öffnen eines Ports für `--source-port-range` die Einstellung „*“ (Standardwert) beibehalten. Dann können eingehende Anforderungen von **allen** anfordernden Ports akzeptiert werden, die normalerweise dynamisch sind.

## Binden an die Netzwerkkarte

Binden der NSG an die Netzwerkkarten:

```bash
azure network nic set -g TestRG -n LB-NIC1 -o TestNSG
```

```bash
azure network nic set -g TestRG -n LB-NIC2 -o TestNSG
```

## Verfügbarkeitsgruppe erstellen
Mit Verfügbarkeitsgruppen können die virtuellen Computer auf sogenannte Fehlerdomänen und Upgradedomänen verteilt werden. Wir erstellen eine Verfügbarkeitsgruppe für die virtuellen Computer:

```bash
azure availset create -g TestRG -n TestAvailSet -l westeurope
```

Durch Fehlerdomänen wird eine Gruppe virtueller Computer definiert, die eine Stromquelle und einen Netzwerkswitch gemeinsam nutzen. Die innerhalb der Verfügbarkeitsgruppe konfigurierten virtuellen Computer werden standardmäßig auf bis zu drei Fehlerdomänen verteilt. Dadurch wirkt sich ein Hardwareproblem in einer dieser Fehlerdomänen nicht auf jeden virtuellen Computer aus, auf dem Ihre Anwendung ausgeführt wird. In Azure werden virtuelle Computer automatisch auf die verschiedenen Fehlerdomänen verteilt, wenn sie sich in einer Verfügbarkeitsgruppe befinden.

Upgradedomänen definieren Gruppen virtueller Computer und die zugrunde liegende physische Hardware, die gleichzeitig neu gestartet werden können. Während einer geplanten Wartung werden die Upgradedomänen möglicherweise nicht der Reihe nach neu gestartet. Es wird aber jeweils nur eine Upgradedomäne neu gestartet. Auch hier werden virtuelle Computer automatisch auf die verschiedenen Upgradedomänen verteilt, wenn sie sich in einer Verfügbarkeitsgruppe befinden.

Weitere Informationen finden Sie unter [Verwalten der Verfügbarkeit virtueller Computer](./virtual-machines-linux-manage-availability.md).

## Erstellen der virtuellen Linux-Computer

Sie haben die Speicher- und Netzwerkressourcen zur Unterstützung von über das Internet erreichbaren virtuellen Computern erstellt. Wir erstellen jetzt diese virtuellen Computer und schützen sie mit einem SSH-Schlüssel ohne Kennwort. In diesem Fall erstellen wir eine Ubuntu-VM basierend auf dem aktuellen LTS-Stand. Wir ermitteln diese Imageinformationen per `azure vm image list`. Dies wird unter [Suchen nach Azure VM-Images](virtual-machines-linux-cli-ps-findimage.md) beschrieben. Wir haben ein Image mit dem Befehl `azure vm image list westeurope canonical | grep LTS` ausgewählt. Hier verwenden wir `canonical:UbuntuServer:14.04.4-LTS:14.04.201604060`, aber für das letzte Feld übergeben wir `latest`, damit wir in Zukunft immer den aktuellen Build erhalten (verwendete Zeichenfolge: `canonical:UbuntuServer:14.04.4-LTS:14.04.201604060`).

> [AZURE.NOTE] Der nächste Schritt ist allen Benutzern vertraut, die mit **ssh-keygen -t rsa -b 2048** bereits ein Paar aus einem öffentlichen und einem privaten SSH-RSA-Schlüssel in unter Linux oder Macintosh erstellt haben. Falls in Ihrem `~/.ssh`-Verzeichnis keine Zertifikatschlüsselpaare enthalten sind, besteht eine Möglichkeit darin, sie: <br /> 1. automatisch mit der Option `azure vm create --generate-ssh-keys` oder 2. manuell anhand der [Anweisungen für die eigene Erstellung](virtual-machines-linux-ssh-from-linux.md) zu erstellen.<br /> Alternativ dazu können Sie die Optionen `azure vm create --admin-username --admin-password` nutzen, um die normalerweise weniger sichere Methode mit Benutzername und Kennwort für die Authentifizierung Ihrer SSH-Verbindungen zu verwenden, nachdem die VM erstellt wurde.

Wir erstellen die VM, indem wir alle Ressourcen und Informationen mit dem Befehl `azure vm create` zusammenfassen.

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
    --image-urn canonical:UbuntuServer:14.04.4-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username ops
```

Ausgabe

```bash
info:    Executing command vm create
+ Looking up the VM "TestVM1"
info:    Verifying the public key SSH file: /home/ifoulds/.ssh/id_rsa.pub
info:    Using the VM Size "Standard_A1"
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

Sie können sofort mithilfe der SSH-Standardschlüssel eine Verbindung mit dem virtuellen Computer herstellen. Achten Sie dabei auf die Angabe des entsprechenden Ports, da der Datenverkehr über den Load Balancer erfolgt (für den ersten virtuellen Computer haben wir die NAT-Regel für die Weiterleitung an Port 4222 des virtuellen Computers eingerichtet):

```bash
 ssh ops@testlb.westeurope.cloudapp.azure.com -p 4222
```

Ausgabe

```bash
The authenticity of host '[testlb.westeurope.cloudapp.azure.com]:4222 ([xx.xx.xx.xx]:4222)' can't be established.
ECDSA key fingerprint is 94:2d:d0:ce:6b:fb:7f:ad:5b:3c:78:93:75:82:12:f9.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '[testlb.westeurope.cloudapp.azure.com]:4222,[xx.xx.xx.xx]:4222' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 14.04.4 LTS (GNU/Linux 3.19.0-58-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

  System information as of Wed Apr 27 23:44:06 UTC 2016

  System load: 0.37              Memory usage: 5%   Processes:       81
  Usage of /:  37.3% of 1.94GB   Swap usage:   0%   Users logged in: 0

  Graph this data and manage this system at:
    https://landscape.canonical.com/

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.



The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

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
    --image-urn canonical:UbuntuServer:14.04.4-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username ops
```

Jetzt können Sie mit dem Befehl `azure vm show testrg testvm` die erstellten Elemente untersuchen. Nun werden die virtuellen Ubuntu-Computer hinter einem Load Balancer in Azure ausgeführt, an denen Sie sich nur mit Ihrem SSH-Schlüsselpaar anmelden können. Kennwörter sind deaktiviert. Sie können nginx oder httpd installieren und eine Web-App bereitstellen. Zudem können Sie den Datenverkehrsfluss durch den Load Balancer an beide virtuellen Computer sehen.

```bash
azure vm show TestRG TestVM1
```

Ausgabe

```bash
info:    Executing command vm show
+ Looking up the VM "TestVM1"
+ Looking up the NIC "LB-NIC1"
data:    Id                              :/subscriptions/8fa5cd83-7fbb-431a-af16-4a20dede8802/resourceGroups/testrg/providers/Microsoft.Compute/virtualMachines/TestVM1
data:    ProvisioningState               :Succeeded
data:    Name                            :TestVM1
data:    Location                        :westeurope
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_A1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :canonical
data:        Offer                       :UbuntuServer
data:        Sku                         :14.04.4-LTS
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :cli1cca1d20a1dcf56c-os-1461800591317
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://computeteststore.blob.core.windows.net/vhds/cli1cca1d20a1dcf56c-os-1461800591317.vhd
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
data:          MAC Address               :00-0D-3A-20-F8-8B
data:          Provisioning State        :Succeeded
data:          Name                      :LB-NIC1
data:          Location                  :westeurope
data:
data:    AvailabilitySet:
data:      Id                            :/subscriptions/guid/resourceGroups/testrg/providers/Microsoft.Compute/availabilitySets/TESTAVAILSET
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://computeteststore.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm show command OK
```

## Nächste Schritte

Sie können jetzt mit mehreren Netzwerkkomponenten und VMs starten.

<!---HONumber=AcomDC_0504_2016-->