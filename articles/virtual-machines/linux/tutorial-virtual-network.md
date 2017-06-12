---
title: Virtuelle Azure-Netzwerke und virtuelle Linux-Computer | Microsoft-Dokumentation
description: 'Tutorial: Verwalten von virtuellen Azure-Netzwerken und virtuellen Linux-Computern mit der Azure-Befehlszeilenschnittstelle'
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: nepeters
ms.translationtype: Human Translation
ms.sourcegitcommit: 44eac1ae8676912bc0eb461e7e38569432ad3393
ms.openlocfilehash: e843e444d2fe32f578c5a887b606db982920a9e0
ms.contentlocale: de-de
ms.lasthandoff: 05/17/2017

---

# <a name="manage-azure-virtual-networks-and-linux-virtual-machines-with-the-azure-cli"></a>Verwalten von virtuellen Azure-Netzwerken und virtuellen Linux-Computern mit der Azure-Befehlszeilenschnittstelle

Virtuelle Azure-Computer nutzen Azure-Netzwerke für interne und externe Kommunikation. Dieses Tutorial enthält ausführliche Informationen zum Bereitstellen von zwei virtuellen Computern sowie zum Konfigurieren von Azure-Netzwerken für diese virtuellen Computer. In den Beispielen in diesem Tutorial wird davon ausgegangen, dass auf den virtuellen Computern eine Webanwendung mit einem Datenbank-Back-End gehostet wird. Eine Anwendung wird im Tutorial jedoch nicht bereitgestellt. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Bereitstellen eines virtuellen Netzwerks
> * Erstellen eines Subnetzes in einem virtuellen Netzwerk
> * Anfügen virtueller Computer an ein Subnetz
> * Verwalten der öffentlichen IP-Adressen des virtuellen Computers
> * Sichern des eingehenden Internetdatenverkehrs
> * Sichern des Datenverkehrs zwischen virtuellen Computern

Für dieses Tutorial ist mindestens Version 2.0.4 der Azure CLI erforderlich. Führen Sie zum Ermitteln der CLI-Version `az --version` aus. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. Sie können auch [Cloud Shell](/azure/cloud-shell/quickstart) in Ihrem Browser verwenden.

## <a name="vm-networking-overview"></a>Übersicht über VM-Netzwerke

Virtuelle Azure-Netzwerke ermöglichen sichere Netzwerkverbindungen zwischen virtuellen Computern, dem Internet und anderen Azure-Diensten wie Azure SQL-Datenbank. Virtuelle Netzwerke werden in logische Segmente, sogenannte Subnetze, unterteilt. Subnetze dienen zur Steuerung des Netzwerkdatenflusses und als Sicherheitsgrenze. Bei der Bereitstellung eines virtuellen Computers ist in der Regel eine virtuelle Netzwerkschnittstelle enthalten, die an ein Subnetz angefügt ist.

## <a name="deploy-virtual-network"></a>Bereitstellen eines virtuellen Netzwerks

In diesem Tutorial wird ein einzelnes virtuelles Netzwerk mit zwei Subnetzen erstellt: Einem Front-End-Subnetz zum Hosten einer Webanwendung und einem Back-End-Subnetz zum Hosten eines Datenbankservers.

Vor der Erstellung eines virtuellen Netzwerks müssen Sie zunächst mit [az group create](/cli/azure/group#create) eine Ressourcengruppe erstellen. Das folgende Beispiel erstellt am Standort „eastus“ eine Ressourcengruppe mit dem Namen *myRGNetwork*.

```azurecli
az group create --name myRGNetwork --location eastus
```

### <a name="create-virtual-network"></a>Virtuelles Netzwerk erstellen

Erstellen Sie mit dem Befehl [az network vnet create](/cli/azure/network/vnet#create) ein virtuelles Netzwerk. In diesem Beispiel erhält das Netzwerk den Namen *mvVnet* und das Adresspräfix *10.0.0.0/16*. Zudem wird ein Subnetz mit dem Namen *mySubnetFrontEnd* und dem Präfix *10.0.1.0/24* erstellt. Weiter unten in diesem Tutorial wird eine Verbindung zwischen einem virtuellen Front-End-Computer und diesem Subnetz hergestellt. 

```azurecli
az network vnet create \
  --resource-group myRGNetwork \
  --name myVnet \
  --address-prefix 10.0.0.0/16 \
  --subnet-name mySubnetFrontEnd \
  --subnet-prefix 10.0.1.0/24
```

### <a name="create-subnet"></a>Erstellen eines Subnetzes

Mit dem Befehl [az network vnet subnet create](/cli/azure/network/vnet/subnet#create) wird dem virtuellen Netzwerk ein neues Subnetz hinzugefügt. In diesem Beispiel erhält das Subnetz den Namen *mySubnetBackEnd* und das Adresspräfix *10.0.2.0/24*. Dieses Subnetz wird für alle Back-End-Dienste verwendet.

```azurecli
az network vnet subnet create \
  --resource-group myRGNetwork \
  --vnet-name myVnet \
  --name mySubnetBackEnd \
  --address-prefix 10.0.2.0/24
```

Zu diesem Zeitpunkt wurde ein Netzwerk erstellt und in zwei Subnetze unterteilt: eines für Front-End-Dienste und eines für Back-End-Dienste. Im nächsten Abschnitt werden virtuelle Computer erstellt und mit diesen Subnetzen verbunden.

## <a name="understand-public-ip-address"></a>Grundlegendes zu öffentlichen IP-Adressen

Dank einer öffentlichen IP-Adresse kann über das Internet auf Azure-Ressourcen zugegriffen werden. In diesem Abschnitt des Tutorials wird ein virtueller Computer erstellt, um das Arbeiten mit öffentlichen IP-Adressen zu veranschaulichen.

### <a name="allocation-method"></a>Zuordnungsmethode

Eine öffentliche IP-Adresse kann dynamisch oder statisch zugeordnet werden. Standardmäßig wird die öffentliche IP-Adresse dynamisch zugeordnet. Dynamische IP-Adressen werden freigegeben, wenn die Zuordnung eines virtuellen Computers aufgehoben wird. Dieses Verhalten bewirkt, dass sich die IP-Adresse bei jedem Vorgang ändert, der eine Aufhebung der VM-Zuordnung beinhaltet.

Als Zuordnungsmethode kann die statische Zuordnung festgelegt werden. Dadurch wird sichergestellt, dass die IP-Adresse einem virtuellen Computer zugewiesen bleibt, auch wenn seine Zuordnung aufgehoben ist. Wenn Sie eine statisch zugeordnete IP-Adresse verwenden, kann die IP-Adresse selbst nicht angegeben werden. Stattdessen wird sie aus einem Pool verfügbarer Adressen zugeordnet.

### <a name="dynamic-allocation"></a>Dynamische Zuordnung

Beim Erstellen eines virtuellen Computers mit dem Befehl [az vm create](/cli/azure/vm#create) wird die öffentliche IP-Adresse standardmäßig dynamisch zugeordnet. Im folgenden Beispiel wird ein virtueller Computer mit einer dynamischen IP-Adresse erstellt. 

```azurecli
az vm create \
  --resource-group myRGNetwork \
  --name myFrontEndVM \
  --vnet-name myVnet \
  --subnet mySubnetFrontEnd \
  --nsg myNSGFrontEnd \
  --public-ip-address myFrontEndIP \
  --image UbuntuLTS \
  --generate-ssh-keys
```

### <a name="static-allocation"></a>Statische Zuordnung

Fügen Sie beim Erstellen eines virtuellen Computers mit dem Befehl [az vm create](/cli/azure/vm#create) das Argument `--public-ip-address-allocation static` ein, um eine statische öffentliche IP-Adresse zuzuweisen. Dieser Vorgang wird in diesem Tutorial nicht veranschaulicht. Im nächsten Abschnitt wird eine dynamisch zugeordnete IP-Adresse jedoch in eine statisch zugeordnete Adresse geändert. 

### <a name="change-allocation-method"></a>Ändern der Zuordnungsmethode

Die Zuordnungsmethode für die IP-Adresse kann mit dem Befehl [az network public-ip update](/cli/azure/network/public-ip#update) geändert werden. In diesem Beispiel wird die IP-Adresszuordnungsmethode des virtuellen Front-End-Computers in statisch geändert.

Heben Sie zunächst die Zuordnung des virtuellen Computer auf.

```azurecli
az vm deallocate --resource-group myRGNetwork --name myFrontEndVM
```

Verwenden Sie den Befehl [az network public-ip update](/azure/network/public-ip#update), um die Zuordnungsmethode zu aktualisieren. In diesem Fall wird für `--allocaion-metod` die Methode *static* festgelegt.

```azurecli
az network public-ip update --resource-group myRGNetwork --name myFrontEndIP --allocation-method static
```

Starten Sie den virtuellen Computer.

```azurecli
az vm start --resource-group myRGNetwork --name myFrontEndVM --no-wait
```

### <a name="no-public-ip-address"></a>Keine öffentliche IP-Adresse

Häufig muss ein virtueller Computer nicht über das Internet zugänglich sein. Verwenden Sie zum Erstellen eines virtuellen Computers ohne öffentliche IP-Adresse das Argument `--public-ip-address ""` mit einem leeren Satz doppelter Anführungszeichen. Diese Konfiguration wird weiter unten in diesem Tutorial veranschaulicht.

## <a name="secure-network-traffic"></a>Netzwerkdatenverkehr absichern

Eine Netzwerksicherheitsgruppe (NSG) enthält eine Liste mit Sicherheitsregeln, mit denen Netzwerkdatenverkehr für Ressourcen, die mit virtuellen Azure-Netzwerken (VNet) verbunden sind, zugelassen oder abgelehnt wird. Netzwerksicherheitsgruppen können Subnetzen oder einzelnen Netzwerkschnittstellen zugeordnet werden. Wenn eine Netzwerksicherheitsgruppe einer Netzwerkschnittstelle zugewiesen ist, gilt sie nur für den zugeordneten virtuellen Computer. Wenn eine NSG einem Subnetz zugeordnet ist, gelten die Regeln für alle Ressourcen, die mit dem Subnetz verbunden sind. 

### <a name="network-security-group-rules"></a>Netzwerksicherheitsgruppen-Regeln

NSG-Regeln definieren Netzwerkports, über die Datenverkehr zugelassen oder verweigert wird. Die Regeln können Quell- und Ziel-IP-Adressbereiche enthalten, um Datenverkehr zwischen bestimmten Systemen oder Subnetzen zu steuern. NSG-Regeln enthalten darüber hinaus eine Priorität (zwischen 1 und 4.096). Regeln werden gemäß ihrer Priorität ausgewertet. Eine Regel mit einer Priorität von 100 wird vor einer Regel der Priorität 200 ausgewertet.

Alle NSGs enthalten eine Gruppe von Standardregeln. Die Standardregeln können zwar nicht gelöscht werden, haben aber niedrigste Priorität und können somit durch selbst erstellte Regeln außer Kraft gesetzt werden.

- **Virtuelles Netzwerk:** Datenverkehr wird aus einem bzw. in ein virtuelles Netzwerk in ein- und ausgehender Richtung zugelassen.
- **Internet:** Ausgehender Datenverkehr wird zugelassen, aber eingehender Datenverkehr wird blockiert.
- **Lastenausgleich:** Lassen Sie für den Lastenausgleich (Load Balancer) von Azure die Überprüfung der Integrität der virtuellen Computer und der Rolleninstanzen zu. Sie können diese Regel außer Kraft setzen, wenn Sie keine Gruppe mit Lastenausgleich verwenden.

### <a name="create-network-security-groups"></a>Erstellen von Netzwerksicherheitsgruppen

Eine Netzwerksicherheitsgruppe kann mithilfe des Befehls [az vm create](/cli/azure/vm#create) zum gleichen Zeitpunkt erstellt werden wie ein virtueller Computer. Dabei wird die NSG der Netzwerkschnittstelle des virtuellen Computers zugewiesen, und zum Zulassen des Datenverkehrs an Port *22* von jeder Quelle wird automatisch eine NSG-Regel erstellt. Weiter oben in diesem Tutorial wurde die Front-End-NSG automatisch mit dem virtuellen Front-End-Computer erstellt. Für Port 22 wurde ebenfalls automatisch eine NSG-Regel erstellt. 

In einigen Fällen kann es hilfreich sein, vorab eine Netzwerksicherheitsgruppe zu erstellen, beispielsweise wenn keine SSH-Standardregeln erstellt werden sollen oder wenn die Netzwerksicherheitsgruppe an ein Subnetz angefügt werden soll. 

Erstellen Sie mit dem Befehl [az network nsg create](/cli/azure/network/nsg#create) eine Netzwerksicherheitsgruppe.

```azurecli
az network nsg create --resource-group myRGNetwork --name myNSGBackEnd
```

Die NSG wird keiner Netzwerkschnittstelle, sondern einem Subnetz zugewiesen. In dieser Konfiguration erbt jeder an das Subnetz angefügte virtuelle Computer die NSG-Regeln.

Aktualisieren Sie das vorhandene Subnetz namens *mySubnetBackEnd* mit der neuen Netzwerksicherheitsgruppe.

```azurecli
az network vnet subnet update \
  --resource-group myRGNetwork \
  --vnet-name myVnet \
  --name mySubnetBackEnd \
  --network-security-group myNSGBackEnd
```

Erstellen Sie nun einen virtuellen Computer, der mit *mySubnetBackEnd* verbunden ist. Beachten Sie, dass für das Argument `--nsg` als Wert leere doppelte Anführungszeichen angegeben sind. Eine NSG muss nicht mit dem virtuellen Computer erstellt werden. Der virtuelle Computer ist mit dem Back-End-Subnetz verbunden, das durch die vorab erstellte Back-End-NSG geschützt wird. Diese NSG gilt für den virtuellen Computer. Beachten Sie darüber hinaus, dass für das Argument `--public-ip-address` als Wert leere doppelte Anführungszeichen angegeben sind. Diese Konfiguration erstellt einen virtuellen Computer ohne öffentliche IP-Adresse. 

```azurecli
az vm create \
  --resource-group myRGNetwork \
  --name myBackEndVM \
  --vnet-name myVnet \
  --subnet mySubnetBackEnd \
  --public-ip-address "" \
  --nsg "" \
  --image UbuntuLTS \
  --generate-ssh-keys
```

### <a name="secure-incoming-traffic"></a>Sichern des eingehenden Datenverkehrs

Als der virtuelle Fron-End-Computer erstellt wurde, wurde eine NSG-Regel generiert, um eingehenden Datenverkehr an Port 22 zuzulassen. Diese Regel lässt SSH-Verbindungen mit dem virtuellen Computer zu. In diesem Beispiel sollte der Datenverkehr auch an Port *80* zugelassen werden. Diese Konfiguration ermöglicht auf dem virtuellen Computer den Zugriff auf eine Webanwendung.

Erstellen Sie mit dem Befehl [az network nsg rule create](/cli/azure/network/nsg/rule#create) eine Regel für Port *80*.

```azurecli
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myNSGFrontEnd \
  --name http \
  --access allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 200 \
  --source-address-prefix "*" \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range 80
```

Auf den virtuellen Front-End-Computer kann nun an Port *22* und Port *80* zugegriffen werden. Der gesamte andere eingehende Datenverkehr wird an der Netzwerksicherheitsgruppe blockiert. Es ist möglicherweise hilfreich, die NSG-Regelkonfigurationen visuell darzustellen. Geben Sie die NSG-Regelkonfiguration mit dem Befehl [az network rule list](/cli/azure/network/nsg/rule#list) zurück. 

```azurecli
az network nsg rule list --resource-group myRGNetwork --nsg-name myNSGFrontEnd --output table
```

Ausgabe:

```azurecli
Access    DestinationAddressPrefix      DestinationPortRange  Direction    Name                 Priority  Protocol    ProvisioningState    ResourceGroup    SourceAddressPrefix    SourcePortRange
--------  --------------------------  ----------------------  -----------  -----------------  ----------  ----------  -------------------  ---------------  ---------------------  -----------------
Allow     *                                               22  Inbound      default-allow-ssh        1000  Tcp         Succeeded            myRGNetwork      *                      *
Allow     *                                               80  Inbound      http                      200  Tcp         Succeeded            myRGNetwork      *                      *
```

### <a name="secure-vm-to-vm-traffic"></a>Sichern des Datenverkehrs zwischen virtuellen Computern

Netzwerksicherheitsgruppen-Regeln können auch zwischen virtuellen Computern gelten. In diesem Beispiel muss der virtuelle Front-End-Computer an Port *22* und *3306* mit dem virtuellen Back-End-Computer kommunizieren. Diese Konfiguration lässt SSH-Verbindungen vom virtuellen Front-End-Computer sowie die Kommunikation zwischen einer Anwendung auf dem virtuellen Front-End-Computer und einer Back-End-MySQL-Datenbank zu. Der gesamte andere Datenverkehr sollte zwischen dem virtuellen Front-End-Computer und dem virtuellen Back-End-Computer blockiert werden.

Erstellen Sie mit dem Befehl [az network nsg rule create](/cli/azure/network/nsg/rule#create) eine Regel für Port 22. Beachten Sie, dass das Argument `--source-address-prefix` den Wert *10.0.1.0/24* angibt. Diese Konfiguration sorgt dafür, dass nur Datenverkehr vom Front-End-Subnetz die Netzwerksicherheitsgruppe passieren kann.

```azurecli
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myNSGBackEnd \
  --name SSH \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 100 \
  --source-address-prefix 10.0.1.0/24 \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "22"
```

Fügen Sie nun eine Regel für MySQL-Datenverkehr an Port 3306 hinzu.

```azurecli
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myNSGBackEnd \
  --name MySQL \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 200 \
  --source-address-prefix 10.0.1.0/24 \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "3306"
```

Da Netzwerksicherheitsgruppen eine Standardregel zum Zulassen des gesamten Datenverkehrs zwischen virtuellen Computern im gleichen VNET enthalten, kann schließlich eine Regel für die Netzwerksicherheitsgruppen des Back-Ends erstellt werden, um den gesamten Datenverkehr zu blockieren. Beachten Sie dabei, dass `--priority` den Wert *300* erhält. Dieser Wert ist niedriger als die NSG- und die MySQL-Regeln. Mit dieser Konfiguration wird sichergestellt, dass SSH- und MySQL-Datenverkehr weiterhin die Netzwerksicherheitsgruppe passieren kann.

```azurecli
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myNSGBackEnd \
  --name denyAll \
  --access Deny \
  --protocol Tcp \
  --direction Inbound \
  --priority 300 \
  --source-address-prefix "*" \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "*"
```

Vom Front-End-Subnetz kann nun an Port *22* und Port *3306* auf den virtuellen Back-End-Computer zugegriffen werden. Der gesamte andere eingehende Datenverkehr wird an der Netzwerksicherheitsgruppe blockiert. Es ist möglicherweise hilfreich, die NSG-Regelkonfigurationen visuell darzustellen. Geben Sie die NSG-Regelkonfiguration mit dem Befehl [az network rule list](/cli/azure/network/nsg/rule#list) zurück. 

```azurecli
az network nsg rule list --resource-group myRGNetwork --nsg-name myNSGBackEnd --output table
```

Ausgabe:

```azurecli
Access    DestinationAddressPrefix    DestinationPortRange    Direction    Name       Priority  Protocol    ProvisioningState    ResourceGroup    SourceAddressPrefix    SourcePortRange
--------  --------------------------  ----------------------  -----------  -------  ----------  ----------  -------------------  ---------------  ---------------------  -----------------
Allow     *                           22                      Inbound      SSH             100  Tcp         Succeeded            myRGNetwork      10.0.1.0/24            *
Allow     *                           3306                    Inbound      MySQL           200  Tcp         Succeeded            myRGNetwork      10.0.1.0/24            *
Deny      *                           *                       Inbound      denyAll         300  Tcp         Succeeded            myRGNetwork      *                      *
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Azure-Netzwerke in Bezug auf virtuelle Computer erstellt und erhalten. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Bereitstellen eines virtuellen Netzwerks
> * Erstellen eines Subnetzes in einem virtuellen Netzwerk
> * Anfügen virtueller Computer an ein Subnetz
> * Verwalten der öffentlichen IP-Adressen des virtuellen Computers
> * Sichern des eingehenden Internetdatenverkehrs
> * Sichern des Datenverkehrs zwischen virtuellen Computern

Im nächsten Tutorial erhalten Sie Informationen zum Sichern von Daten auf virtuellen Computern mit Azure Backup. 

> [!div class="nextstepaction"]
> [Back up Linux virtual machines in Azure](./tutorial-backup-vms.md) (Sichern virtueller Linux-Computer in Azure)
