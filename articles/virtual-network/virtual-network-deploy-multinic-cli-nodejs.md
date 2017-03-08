---
title: "Erstellen einer VM mit mehreren Netzwerkkarten – Azure CLI 1.0 | Microsoft-Dokumentation"
description: Erfahren Sie, wie Sie eine VM mit mehreren Netzwerkkarten mithilfe der Azure CLI 1.0 erstellen.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 8e906a4b-8583-4a97-9416-ee34cfa09a98
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 63f2f6dde56c1b5c4b3ad2591700f43f6542874d
ms.openlocfilehash: b95bcb38664718bf25ec6981c803415790c6da3d
ms.lasthandoff: 02/28/2017


---
# <a name="create-a-vm-with-multiple-nics-using-the-azure-cli-10"></a>Erstellen einer VM mit mehreren Netzwerkkarten mithilfe der Azure CLI 1.0

[!INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[!INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

> [!NOTE]
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager-Bereitstellung und klassische Bereitstellung](../resource-manager-deployment-model.md).  Dieser Artikel befasst sich mit dem Resource Manager-Bereitstellungsmodell, das von Microsoft für die meisten neuen Bereitstellungen anstatt des [klassischen Bereitstellungsmodells](virtual-network-deploy-multinic-classic-cli.md) empfohlen wird.
>

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

In den folgenden Schritten verwenden Sie eine Ressourcengruppe mit dem Namen *IaaSStory* für die Webserver und eine Ressourcengruppe mit dem Namen *IaaSStory-BackEnd* für die DB-Server. Sie können diese Aufgabe mithilfe der Azure CLI 1.0 (dieser Artikel) oder mit der [Azure CLI 2.0](virtual-network-deploy-static-pip-arm-cli.md) ausführen. Mithilfe der Werte in Anführungszeichen ("") in den folgenden Schritten werden Ressourcen mit Einstellungen aus dem Szenario erstellt. Ersetzen Sie die Werte ggf. durch entsprechende Werte für Ihre Umgebung.

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie die DB-Server erstellen können, müssen Sie die Ressourcengruppe *IaaSStory* mit den erforderlichen Ressourcen für dieses Szenario erstellen. Führen Sie die folgenden Schritte aus, um diese Ressourcen zu erstellen:

1. Wechseln Sie zu [azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC)(in englischer Sprache).
2. Klicken Sie auf der Vorlagenseite rechts neben **Parent Resource group** (übergeordnete Ressourcengruppe) auf **Deploy to Azure** (In Azure bereitstellen).
3. Ändern Sie bei Bedarf die Parameterwerte, und führen Sie die folgenden Schritte im Azure-Vorschauportal aus, um die Ressourcengruppe bereitzustellen.

> [!IMPORTANT]
> Achten Sie darauf, eindeutige Speicherkontonamen zu verwenden. In Azure dürfen keine doppelten Speicherkontennamen verwendet werden.
> 

[!INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-the-back-end-vms"></a>Erstellen der Back-End-VMs
Die Back-End-VMs sind auf die Erstellung der folgenden Ressourcen angewiesen:

* **Speicherkonto für Datenträger.** Für eine bessere Leistung verwenden die Datenträger auf den Datenbankservern Solid State Drive (SSD)-Technik. Dafür ist ein Storage Premium-Konto erforderlich. Achten Sie darauf, dass der Azure-Speicherort für die Bereitstellung Storage Premium unterstützt.
* **NICs**. Jeder virtuelle Computer hat zwei Netzwerkkarten, eine für den Datenbankzugriff und eine für die Verwaltung.
* **Verfügbarkeitsgruppe**. Alle Datenbankserver werden einer einzigen Verfügbarkeitsgruppe hinzugefügt, damit sichergestellt ist, dass mindestens ein virtueller Computer während der Wartung ausgeführt wird.

### <a name="step-1---start-your-script"></a>Schritt 1: Starten des Skripts
Sie können das verwendete Bash-Skript ungekürzt [hier](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/arm/virtual-network-deploy-multinic-arm-cli.sh) herunterladen. Gehen Sie folgendermaßen vor, um das Skript an Ihre Arbeitsumgebung anzupassen.

1. Ändern Sie die Werte der nachstehenden Variablen basierend auf der im obigen Abschnitt [Voraussetzungen](#Prerequisites) bereitgestellten Ressourcengruppe.

    ```azurecli
    existingRGName="IaaSStory"
    location="westus"
    vnetName="WTestVNet"
    backendSubnetName="BackEnd"
    remoteAccessNSGName="NSG-RemoteAccess"
    ```
2. Ändern Sie die Werte der nachstehenden Variablen basierend auf den Werten, die Sie für die Back-End-Bereitstellung verwenden möchten.

    ```azurecli
    backendRGName="IaaSStory-Backend"
    prmStorageAccountName="wtestvnetstorageprm"
    avSetName="ASDB"
    vmSize="Standard_DS3"
    diskSize=127
    publisher="Canonical"
    offer="UbuntuServer"
    sku="14.04.2-LTS"
    version="latest"
    vmNamePrefix="DB"
    osDiskName="osdiskdb"
    dataDiskName="datadisk"
    nicNamePrefix="NICDB"
    ipAddressPrefix="192.168.2."
    username='adminuser'
    password='adminP@ssw0rd'
    numberOfVMs=2
    ```

3. Rufen Sie die ID für das `BackEnd` -Subnetz ab, in dem die virtuellen Computer erstellt werden sollen. Dieser Schritt ist erforderlich, da sich die mit diesem Subnetz verknüpften Netzwerkschnittstellenkarten in einer anderen Ressourcengruppe befinden.

    ```azurecli
    subnetId="$(azure network vnet subnet show --resource-group $existingRGName \
            --vnet-name $vnetName \
            --name $backendSubnetName|grep Id)"
    subnetId=${subnetId#*/}
    ```

   > [!TIP]
   > Der erste Befehl oben verwendet [grep](http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_04_02.html) und die [Zeichenkettenmanipulation](http://tldp.org/LDP/abs/html/string-manipulation.html), insbesondere Substringentfernung.
   >

4. Rufen Sie die ID für die `NSG-RemoteAccess`-NSG ab. Dieser Schritt ist erforderlich, da sich die mit dieser NSG verknüpften Netzwerkschnittstellenkarten in einer anderen Ressourcengruppe befinden.

    ```azurecli
    nsgId="$(azure network nsg show --resource-group $existingRGName \
        --name $remoteAccessNSGName|grep Id)"
        nsgId=${nsgId#*/}
    ```

### <a name="step-2---create-necessary-resources-for-your-vms"></a>Schritt 2: Erstellen der erforderlichen Ressourcen für Ihre virtuellen Computer

1. Erstellen Sie eine neue Ressourcengruppe für alle Back-End-Ressourcen. Beachten Sie die Verwendung der `$backendRGName`-Variable für den Ressourcengruppennamen und `$location` für die Azure-Region.

    ```azurecli
    azure group create $backendRGName $location
    ```

2. Erstellen Sie ein Storage Premium-Konto für das Betriebssystem und die Datenträger, die von den virtuellen Computern verwendet werden sollen.

    ```azurecli
    azure storage account create $prmStorageAccountName \
        --resource-group $backendRGName \
        --location $location \
        --type PLRS
    ```

3. Erstellen Sie eine Verfügbarkeitsgruppe für die VM.

    ```azurecli
    azure availset create --resource-group $backendRGName \
        --location $location \
        --name $avSetName
    ```

### <a name="step-3---create-the-nics-and-back-end-vms"></a>Schritt 3: Erstellen der Netzwerkschnittstellenkarten und Back-End-VMs

1. Erstellen Sie mithilfe einer Schleife mehrere virtuelle Computer auf Grundlage der `numberOfVMs`-Variablen.

    ```azurecli
    for ((suffixNumber=1;suffixNumber<=numberOfVMs;suffixNumber++));
    do
    ```

2. Erstellen Sie für jeden virtuellen Computer eine Netzwerkschnittstellenkarte für den Datenbankzugriff.

    ```azurecli
    nic1Name=$nicNamePrefix$suffixNumber-DA
    x=$((suffixNumber+3))
    ipAddress1=$ipAddressPrefix$x
    azure network nic create --name $nic1Name \
        --resource-group $backendRGName \
        --location $location \
        --private-ip-address $ipAddress1 \
        --subnet-id $subnetId
    ```

3. Erstellen Sie für jeden virtuellen Computer eine Netzwerkschnittstellenkarte für den Remotezugriff. Achten Sie auf den `--network-security-group`-Parameter, über den die NIC der NSG zugeordnet wird.

    ```azurecli
    nic2Name=$nicNamePrefix$suffixNumber-RA
    x=$((suffixNumber+53))
    ipAddress2=$ipAddressPrefix$x
    azure network nic create --name $nic2Name \
        --resource-group $backendRGName \
        --location $location \
        --private-ip-address $ipAddress2 \
        --subnet-id $subnetId $vnetName \
        --network-security-group-id $nsgId
    ```

4. Erstellen Sie den virtuellen Computer.

    ```azurecli
    azure vm create --resource-group $backendRGName \
        --name $vmNamePrefix$suffixNumber \
        --location $location \
        --vm-size $vmSize \
        --subnet-id $subnetId \
        --availset-name $avSetName \
        --nic-names $nic1Name,$nic2Name \
        --os-type linux \
        --image-urn $publisher:$offer:$sku:$version \
        --storage-account-name $prmStorageAccountName \
        --storage-account-container-name vhds \
        --os-disk-vhd $osDiskName$suffixNumber.vhd \
        --admin-username $username \
        --admin-password $password
    ```

5. Erstellen Sie für jeden virtuellen Computer zwei Datenträger, und beenden Sie die Schleife mit dem `done`-Befehl.

    ```azurecli
    azure vm disk attach-new --resource-group $backendRGName \
        --vm-name $vmNamePrefix$suffixNumber \
        --storage-account-name $prmStorageAccountName \
        --storage-account-container-name vhds \
        --vhd-name $dataDiskName$suffixNumber-1.vhd \
        --size-in-gb $diskSize \
        --lun 0

    azure vm disk attach-new --resource-group $backendRGName \
        --vm-name $vmNamePrefix$suffixNumber \        
        --storage-account-name $prmStorageAccountName \
        --storage-account-container-name vhds \
        --vhd-name $dataDiskName$suffixNumber-2.vhd \
        --size-in-gb $diskSize \
        --lun 1
        done
    ```

### <a name="step-4---run-the-script"></a>Schritt 4: Ausführen des Skripts
Führen Sie das Skript aus, nachdem sie es heruntergeladen und angepasst haben, um die Back-End-VMs mit mehreren Netzwerkkarten zu erstellen.

1. Speichern Sie Ihr Skript, und führen Sie es im **Bash**-Terminal aus. Anfänglich wird die folgende Ausgabe angezeigt.
   
        info:    Executing command group create
        info:    Getting resource group IaaSStory-Backend
        info:    Creating resource group IaaSStory-Backend
        info:    Created resource group IaaSStory-Backend
        data:    Id:                  /subscriptions/[Subscription ID]/resourceGroups/IaaSStory-Backend
        data:    Name:                IaaSStory-Backend
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK
        info:    Executing command storage account create
        info:    Creating storage account
        info:    storage account create command OK
        info:    Executing command availset create
        info:    Looking up the availability set "ASDB"
        info:    Creating availability set "ASDB"
        info:    availset create command OK
        info:    Executing command network nic create
        info:    Looking up the network interface "NICDB1-DA"
        info:    Creating network interface "NICDB1-DA"
        info:    Looking up the network interface "NICDB1-DA"
        data:    Id                              : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory-Backend/providers/Microsoft.Network/networkInterfaces/NICDB1-DA
        data:    Name                            : NICDB1-DA
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.2.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet/subnets/BackEnd
        data:
        info:    network nic create command OK
        info:    Executing command network nic create
        info:    Looking up the network interface "NICDB1-RA"
        info:    Creating network interface "NICDB1-RA"
        info:    Looking up the network interface "NICDB1-RA"
        data:    Id                              : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory-Backend/providers/Microsoft.Network/networkInterfaces/NICDB1-RA
        data:    Name                            : NICDB1-RA
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    Network security group          : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory/providers/Microsoft.Network/networkSecurityGroups/NSG-RemoteAccess
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.2.54
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet/subnets/BackEnd
        data:
        info:    network nic create command OK
        info:    Executing command vm create
        info:    Looking up the VM "DB1"
        info:    Using the VM Size "Standard_DS3"
        info:    The [OS, Data] Disk or image configuration requires storage account
        info:    Looking up the storage account wtestvnetstorageprm
        info:    Looking up the availability set "ASDB"
        info:    Found an Availability set "ASDB"
        info:    Looking up the NIC "NICDB1-DA"
        info:    Looking up the NIC "NICDB1-RA"
        info:    Creating VM "DB1"
2. Nach einigen Minuten wird die Ausführung beendet, und Sie sehen den im Folgenden gezeigten Rest der Ausgabe.
   
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Looking up the VM "DB1"
        info:    Looking up the storage account wtestvnetstorageprm
        info:    New data disk location: https://wtestvnetstorageprm.blob.core.windows.net/vhds/datadisk1-1.vhd
        info:    Updating VM "DB1"
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Looking up the VM "DB1"
        info:    Looking up the storage account wtestvnetstorageprm
        info:    New data disk location: https://wtestvnetstorageprm.blob.core.windows.net/vhds/datadisk1-2.vhd
        info:    Updating VM "DB1"
        info:    vm disk attach-new command OK
        info:    Executing command network nic create
        info:    Looking up the network interface "NICDB2-DA"
        info:    Creating network interface "NICDB2-DA"
        info:    Looking up the network interface "NICDB2-DA"
        data:    Id                              : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory-Backend/providers/Microsoft.Network/networkInterfaces/NICDB2-DA
        data:    Name                            : NICDB2-DA
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.2.5
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet/subnets/BackEnd
        data:
        info:    network nic create command OK
        info:    Executing command network nic create
        info:    Looking up the network interface "NICDB2-RA"
        info:    Creating network interface "NICDB2-RA"
        info:    Looking up the network interface "NICDB2-RA"
        data:    Id                              : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory-Backend/providers/Microsoft.Network/networkInterfaces/NICDB2-RA
        data:    Name                            : NICDB2-RA
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    Network security group          : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory/providers/Microsoft.Network/networkSecurityGroups/NSG-RemoteAccess
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.2.55
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet/subnets/BackEnd
        data:
        info:    network nic create command OK
        info:    Executing command vm create
        info:    Looking up the VM "DB2"
        info:    Using the VM Size "Standard_DS3"
        info:    The [OS, Data] Disk or image configuration requires storage account
        info:    Looking up the storage account wtestvnetstorageprm
        info:    Looking up the availability set "ASDB"
        info:    Found an Availability set "ASDB"
        info:    Looking up the NIC "NICDB2-DA"
        info:    Looking up the NIC "NICDB2-RA"
        info:    Creating VM "DB2"
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Looking up the VM "DB2"
        info:    Looking up the storage account wtestvnetstorageprm
        info:    New data disk location: https://wtestvnetstorageprm.blob.core.windows.net/vhds/datadisk2-1.vhd
        info:    Updating VM "DB2"
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Looking up the VM "DB2"
        info:    Looking up the storage account wtestvnetstorageprm
        info:    New data disk location: https://wtestvnetstorageprm.blob.core.windows.net/vhds/datadisk2-2.vhd
        info:    Updating VM "DB2"
        info:    vm disk attach-new command OK


