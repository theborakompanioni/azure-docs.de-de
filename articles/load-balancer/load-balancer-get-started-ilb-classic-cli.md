---
title: "Erstellen eines internen Lastenausgleichs – Azure CLI (klassisch) | Microsoft-Dokumentation"
description: Erfahren Sie, wie Sie einen internen Load Balancer im klassischen Bereitstellungsmodell mithilfe der Azure-Befehlszeilenschnittstelle erstellen.
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: becbbbde-a118-4269-9444-d3153f00bf34
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: fd5960a4488f2ecd93ba117a7d775e78272cbffd
ms.openlocfilehash: dc1ca3ce8befb0c5b707d6db2fb16178afe3de1e

---

# <a name="get-started-creating-an-internal-load-balancer-classic-using-the-azure-cli"></a>Erste Schritte zum Erstellen eines internen Load Balancers (klassisch) mithilfe der Azure-Befehlszeilenschnittstelle

> [!div class="op_single_selector"]
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-classic-ps.md)
> * [Azure-Befehlszeilenschnittstelle](../load-balancer/load-balancer-get-started-ilb-classic-cli.md)
> * [Clouddienste](../load-balancer/load-balancer-get-started-ilb-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!IMPORTANT]
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager-Bereitstellung und klassische Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md).  Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells. Erfahren Sie, wie Sie [diese Schritte mit dem Resource Manager-Modell ausführen](load-balancer-get-started-ilb-arm-cli.md).

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="to-create-an-internal-load-balancer-set-for-virtual-machines"></a>Erstellen einer internen Load Balancer-Gruppe für virtuelle Computer

Zum Erstellen einer internen Load Balancer-Gruppe und der Server, die den Datenverkehr an diese Gruppe senden, müssen Sie Folgendes ausführen:

1. Erstellen Sie eine ILB-Instanz, die als Endpunkt für den eingehenden Datenverkehr dient, für den Lastenausgleich zwischen den Servern einer Lastenausgleichsgruppe durchgeführt wird.
2. Fügen Sie Endpunkte für die virtuellen Computer hinzu, die den eingehenden Datenverkehr empfangen.
3. Konfigurieren Sie die Server, die den Datenverkehr für den Lastenausgleich senden, so, dass der Datenverkehr an die virtuelle IP-Adresse (VIP) der ILB-Instanz gesendet wird.

## <a name="step-by-step-creating-an-internal-load-balancer-using-cli"></a>Schritte zum Erstellen eines internen Load Balancers mit der Befehlszeilenschnittstelle

Anhand der folgenden Anleitung erstellen Sie auf der Grundlage des oben beschriebenen Szenarios einen internen Load Balancer.

1. Wenn Sie die Azure-Befehlszeilenschnittstelle noch nie verwendet haben, ziehen Sie [Installieren und Konfigurieren der Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md) zurate, und folgen Sie den Anweisungen bis zu dem Punkt, an dem Sie Ihr Azure-Konto und Ihr Abonnement auswählen.
2. Führen Sie den Befehl **azure config mode** aus, um in den klassischen Modus zu wechseln, wie unten dargestellt.

    ```azurecli
    azure config mode asm
    ```

    Erwartete Ausgabe:

        info:    New mode is asm

## <a name="create-endpoint-and-load-balancer-set"></a>Erstellen eines Endpunkts und einer Load Balancer-Gruppe

Das Szenario setzt die virtuellen Computer „DB1“ und „DB2“ in einem Clouddienst namens „mytestcloud“ voraus. Beide virtuelle Computer verwenden ein virtuelles Netzwerk namens „my testvnet“ mit dem Subnetz „subnet-1“.

Dieses Handbuch erstellt eine interne Load Balancer-Gruppe, die Port 1433 als privaten Port und Port 1433 als lokalen Port verwendet.

Dies ist ein häufiges Szenario, bei dem sich die virtuellen SQL-Computer im Back-End befinden und über einen internen Load Balancer sichergestellt wird, dass die Datenbankserver nicht direkt über eine öffentliche IP-Adresse offen gelegt werden.

### <a name="step-1"></a>Schritt 1

Erstellen Sie einen internen Load Balancer mit `azure network service internal-load-balancer add`.

```azurecli
azure service internal-load-balancer add --serviceName mytestcloud --internalLBName ilbset --subnet-name subnet-1 --static-virtualnetwork-ipaddress 192.168.2.7
```

Weitere Informationen finden Sie unter `azure service internal-load-balancer --help` .

Sie können die Eigenschaften des internen Load Balancers mithilfe des Befehls `azure service internal-load-balancer list` *Clouddienstname*überprüfen.

Dies ist ein Beispiel für die Ausgabe:

    azure service internal-load-balancer list my-testcloud
    info:    Executing command service internal-load-balancer list
    + Getting cloud service deployment
    data:    Name    Type     SubnetName  StaticVirtualNetworkIPAddress
    data:    ------  -------  ----------  -----------------------------
    data:    ilbset  Private  subnet-1    192.168.2.7
    info:    service internal-load-balancer list command OK


### <a name="step-2"></a>Schritt 2

Sie konfigurieren die interne Load Balancer-Gruppe, wenn Sie den ersten Endpunkt hinzufügen. Sie ordnen den Endpunkt, den virtuellen Computer und den Testport der internen Load Balancer-Gruppe in diesem Schritt zu.

```azurecli
azure vm endpoint create db1 1433 --local-port 1433 --protocol tcp --probe-port 1433 --probe-protocol tcp --probe-interval 300 --probe-timeout 600 --internal-load-balancer-name ilbset
```

### <a name="step-3"></a>Schritt 3

Überprüfen Sie die Load Balancer-Konfiguration mithilfe von `azure vm show` *Name des virtuellen Computers*

```azurecli
azure vm show DB1
```

Ausgabe:

    azure vm show DB1
    info:    Executing command vm show
    + Getting virtual machines
    data:    DNSName "mytestcloud.cloudapp.net"
    data:    Location "East US 2"
    data:    VMName "DB1"
    data:    IPAddress "192.168.2.4"
    data:    InstanceStatus "ReadyRole"
    data:    InstanceSize "Standard_D1"
    data:    Image "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-20151022-en.us-127GB.vhd"
    data:    OSDisk hostCaching "ReadWrite"
    data:    OSDisk name "db1-DB1-0-201511120457370846"
    data:    OSDisk mediaLink "https://XXXX.blob.core.windows.net/vhd"
    data:    OSDisk sourceImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-20151022-en.us-127GB.vhd"
    data:    OSDisk operatingSystem "Windows"
    data:    OSDisk iOType "Standard"
    data:    ReservedIPName ""
    data:    VirtualIPAddresses 0 address "137.116.64.107"
    data:    VirtualIPAddresses 0 name "db1ContractContract"
    data:    VirtualIPAddresses 0 isDnsProgrammed true
    data:    VirtualIPAddresses 1 address "192.168.2.7"
    data:    VirtualIPAddresses 1 name "ilbset"
    data:    Network Endpoints 0 localPort 5986
    data:    Network Endpoints 0 name "PowerShell"
    data:    Network Endpoints 0 port 5986
    data:    Network Endpoints 0 protocol "tcp"
    data:    Network Endpoints 0 virtualIPAddress "137.116.64.107"
    data:    Network Endpoints 0 enableDirectServerReturn false
    data:    Network Endpoints 1 localPort 3389
    data:    Network Endpoints 1 name "Remote Desktop"
    data:    Network Endpoints 1 port 60173
    data:    Network Endpoints 1 protocol "tcp"
    data:    Network Endpoints 1 virtualIPAddress "137.116.64.107"
    data:    Network Endpoints 1 enableDirectServerReturn false
    data:    Network Endpoints 2 localPort 1433
    data:    Network Endpoints 2 name "tcp-1433-1433"
    data:    Network Endpoints 2 port 1433
    data:    Network Endpoints 2 loadBalancerProbe port 1433
    data:    Network Endpoints 2 loadBalancerProbe protocol "tcp"
    data:    Network Endpoints 2 loadBalancerProbe intervalInSeconds 300
    data:    Network Endpoints 2 loadBalancerProbe timeoutInSeconds 600
    data:    Network Endpoints 2 protocol "tcp"
    data:    Network Endpoints 2 virtualIPAddress "192.168.2.7"
    data:    Network Endpoints 2 enableDirectServerReturn false
    data:    Network Endpoints 2 loadBalancerName "ilbset"
    info:    vm show command OK

## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a>Erstellen eines Remotedesktopendpunkts für einen virtuellen Computer

Sie können mithilfe von `azure vm endpoint create`einen Remotedesktop-Endpunkt erstellen, um für einen bestimmten virtuellen Computer Netzwerkdatenverkehr von einem öffentlichen Port an einen lokalen Port weiterzuleiten.

```azurecli
azure vm endpoint create web1 54580 -k 3389
```

## <a name="remove-virtual-machine-from-load-balancer"></a>Entfernen eines virtuellen Computers aus einem Load Balancer

Sie können einen virtuellen Computer aus einer internen Load Balancer-Gruppe entfernen, indem Sie den zugehörigen Endpunkt löschen. Sobald der Endpunkt entfernt wurde, gehört der virtuelle Computer nicht mehr zu der Load Balancer-Gruppe.

Anhand des oben genannten Beispiels können Sie mithilfe des Befehls `azure vm endpoint delete`den für den virtuellen Computer „DB1“ erstellten Endpunkt aus dem internen Load Balancer „ilbset“ entfernen.

```azurecli
azure vm endpoint delete DB1 tcp-1433-1433
```

Weitere Informationen finden Sie unter `azure vm endpoint --help` .

## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren eines Load Balancer-Verteilungsmodus mithilfe der Quell-IP-Affinität](load-balancer-distribution-mode.md)

[Konfigurieren von TCP-Leerlauftimeout-Einstellungen für den Lastenausgleich](load-balancer-tcp-idle-timeout.md)



<!--HONumber=Jan17_HO4-->


