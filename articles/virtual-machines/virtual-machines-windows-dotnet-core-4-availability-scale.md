---
title: "Verfügbarkeit und Skalierung in Azure Resource Manager-Vorlagen | Microsoft Docs"
description: ".NET Core-Tutorial für virtuelle Azure-Computer"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 494724b5-06af-4dea-a774-ba580cf27527
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 11/21/2016
ms.author: nepeters
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: cea53acc33347b9e6178645f225770936788f807
ms.openlocfilehash: b321683b2080e5af3a112791eae416e03dee0080
ms.lasthandoff: 03/03/2017


---
# <a name="availability-and-scale-in-azure-resource-manager-templates-for-windows-vms"></a>Verfügbarkeit und Skalierung in Azure Resource Manager-Vorlagen für Windows-VMs

Bei der Verfügbarkeit und Skalierung geht es um die Betriebszeit und die Erfüllung des jeweiligen Bedarfs. Wenn für eine Anwendung eine Betriebszeit von 99,9% erforderlich ist, muss sie über eine Architektur verfügen, in der mehrere Computeressourcen gleichzeitig genutzt werden können. Im Gegensatz zu einer einzelnen Website umfasst eine Konfiguration mit einem höheren Verfügbarkeitsgrad beispielsweise mehrere Instanzen derselben Website mit einer vorgeschalteten Lastenausgleichstechnologie. In dieser Konfiguration kann eine Instanz der Anwendung zu Wartungszwecken außer Betrieb genommen werden, während die anderen Instanzen weiter ausgeführt werden. Mit der Skalierung wird erreicht, dass Anwendungen den jeweiligen Bedarf erfüllen können. Bei einer Anwendung mit Lastenausgleich kann diese durch das Hinzufügen oder Entfernen von Instanzen aus dem Pool je nach Bedarf skaliert werden.

In diesem Dokument wird erläutert, wie die Music Store-Beispielbereitstellung in Bezug auf die Verfügbarkeit und Skalierung konfiguriert wird. Alle Abhängigkeiten und individuellen Konfigurationen werden hervorgehoben. Stellen Sie am besten vorab eine Instanz der Lösung in Ihrem Azure-Abonnement bereit, und orientieren Sie sich an der Azure Resource Manager-Vorlage. Die vollständige Vorlage finden Sie hier – [Music Store Deployment on Windows (Music Store-Bereitstellung unter Windows)](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="availability-set"></a>Verfügbarkeitsgruppe
Mit einer Verfügbarkeitsgruppe werden virtuelle Azure-Computer auf physischen Hosts und andere Infrastrukturkomponenten logisch zusammengefasst, z.B. Stromversorgungseinheiten und physische Netzwerkhardware. Verfügbarkeitsgruppen sorgen dafür, dass bei Wartungsarbeiten, Geräteausfällen oder anderen Ausfällen nicht alle virtuellen Computer betroffen sind. Eine Verfügbarkeitsgruppe kann einer Azure Resource Manager-Vorlage mithilfe des Visual Studio-Assistenten zum Hinzufügen neuer Ressourcen oder durch Einfügen von gültigem JSON-Code in eine Vorlage hinzugefügt werden.

Das entsprechende JSON-Beispiel in der Resource Manager-Vorlage finden Sie unter [Availability Set](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L368)(Verfügbarkeitsgruppe).

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/availabilitySets",
  "name": "[variables('availabilitySetName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "availability-set"
  },
  "properties": {
  }
}
```

Eine Verfügbarkeitsgruppe wird als Eigenschaft eines virtuellen Computers deklariert. 

Das entsprechende JSON-Beispiel in der Resource Manager-Vorlage finden Sie unter [Availability Set association with Virtual Machine](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L302)(Zuordnung der Verfügbarkeitsgruppe zum virtuellen Computer).

```json
"properties": {
  "availabilitySet": {
    "id": "[resourceId('Microsoft.Compute/availabilitySets', variables('availabilitySetName'))]"
  }
```
Hier ist die Ansicht der Verfügbarkeitsgruppe über das Azure-Portal dargestellt. Alle virtuellen Computer und die Details zur Konfiguration sind angegeben.

![Verfügbarkeitsgruppe](./media/virtual-machines-windows-dotnet-core/ase-win.png)

Ausführliche Informationen zu Verfügbarkeitsgruppen finden Sie unter [Verwalten der Verfügbarkeit virtueller Computer](virtual-machines-windows-manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

## <a name="network-load-balancer"></a>Lastenausgleich für das Netzwerk
Während eine Verfügbarkeitsgruppe die Fehlertoleranz einer Anwendung sicherstellt, werden beim Lastenausgleich viele Instanzen der Anwendung unter einer einzelnen Netzwerkadresse verfügbar gemacht. Mehrere Instanzen einer Anwendung können auf vielen virtuellen Computern gehostet werden, die jeweils mit einem Lastenausgleichsmodul verbunden sind. Wenn auf die Anwendung zugegriffen wird, leitet der Lastenausgleich die eingehende Anforderung an die angefügten Mitglieder weiter. Sie können einen Lastenausgleich mit dem Visual Studio-Assistenten „Neue Ressource hinzufügen“ hinzufügen, oder indem Sie eine richtig formatierte JSON-Ressource in die Azure Resource Manager-Vorlage einfügen.

Das entsprechende JSON-Beispiel in der Resource Manager-Vorlage finden Sie unter [Network Load Balancer](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L198)(Netzwerklastenausgleich).

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/loadBalancers",
  "name": "[variables('loadBalancerName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "load-balancer"
  },
  ........<truncated>
}
```

Da die Beispielanwendung im Internet über eine öffentliche IP-Adresse verfügbar gemacht wird, wird diese Adresse dem Lastenausgleich zugeordnet. 

Das entsprechende JSON-Beispiel in der Resource Manager-Vorlage finden Sie unter [Network Load Balancer association with Public IP Address](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L211)(Zuordnung des Netzwerklastenausgleichs zur öffentlichen IP-Adresse).

```json
"frontendIPConfigurations": [
  {
    "properties": {
      "publicIPAddress": {
        "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicIpAddressName'))]"
      }
    },
    "name": "LoadBalancerFrontend"
  }
]
```

Im Azure-Portal wird die Zuordnung zur öffentlichen IP-Adresse in der Übersicht des Netzwerklastenausgleichs angezeigt.

![Lastenausgleich für das Netzwerk](./media/virtual-machines-windows-dotnet-core/nlb-win.png)

## <a name="load-balancer-rule"></a>Lastenausgleichsregel
Beim Verwenden eines Lastenausgleichs werden Regeln konfiguriert, mit denen gesteuert werden kann, wie Datenverkehr auf die gewünschten Ressourcen verteilt wird. Bei der Music Store-Beispielanwendung geht der Datenverkehr an Port 80 der öffentlichen IP-Adresse ein und wird über Port 80 aller virtuellen Computer verteilt. 

Das entsprechende JSON-Beispiel in der Resource Manager-Vorlage finden Sie unter [Load Balancer Rule](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L226)(Lastenausgleichsregel).

```json
"loadBalancingRules": [
  {
    "name": "[variables('loadBalencerRule')]",
    "properties": {
      "frontendIPConfiguration": {
        "id": "[concat(resourceId('Microsoft.Network/loadBalancers', variables('loadBalancerName')), '/frontendIPConfigurations/LoadBalancerFrontend')]"
      },
      "backendAddressPool": {
        "id": "[variables('lbPoolID')]"
      },
      "protocol": "Tcp",
      "frontendPort": 80,
      "backendPort": 80,
      "enableFloatingIP": false,
      "idleTimeoutInMinutes": 5,
      "probe": {
        "id": "[variables('lbProbeID')]"
      }
    }
  }
]
```

Hier ist eine Ansicht der Regel für den Netzwerklastenausgleich im Portal dargestellt.

![Regel für den Netzwerklastenausgleich](./media/virtual-machines-windows-dotnet-core/lbrule-win.png)

## <a name="load-balancer-probe"></a>Lastenausgleichstest
Über den Lastenausgleich muss außerdem jeder virtuelle Computer überwacht werden, damit Anforderungen nur für ausgeführte Systeme bereitgestellt werden. Diese Überwachung wird durchgeführt, indem für einen vordefinierten Port ständig Stichproben genommen werden. Die Music Store-Bereitstellung ist so konfiguriert, dass Port 80 auf allen beteiligten virtuellen Computern getestet wird. 

Das entsprechende JSON-Beispiel in der Resource Manager-Vorlage finden Sie unter [Load Balancer Probe](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L247)(Lastenausgleichstest).

```json
"probes": [
  {
    "properties": {
      "protocol": "Tcp",
      "port": 80,
      "intervalInSeconds": 15,
      "numberOfProbes": 2
    },
    "name": "lbprobe"
  }
]
```

Hier ist der Lastenausgleichstest im Azure-Portal dargestellt.

![Netzwerklastenausgleichstest](./media/virtual-machines-windows-dotnet-core/lbprobe-win.png)

## <a name="inbound-nat-rules"></a>Eingehende NAT-Regeln
Bei der Verwendung eines Lastenausgleichs müssen Regeln festgelegt werden, die den Zugriff auf jeden virtuellen Computer ohne Lastenausgleich ermöglichen. Wenn beispielsweise eine RDP-Verbindung mit jedem virtuellen Computer hergestellt wird, sollte für diesen Datenverkehr kein Lastenausgleich erfolgen, sondern ein vorbestimmter Pfad konfiguriert werden. Vorbestimmte Pfade werden mit einer Ressource vom Typ „Eingehende NAT-Regel“ konfiguriert. Mit dieser Ressource kann die eingehende Kommunikation einzelnen virtuellen Computern zugeordnet werden. 

Mit der Music Store-Anwendung wird ein Port ab 5000 dem Port 3389 auf jedem virtuellen Computer für den RDP-Zugriff zugeordnet. Die Funktion `copyindex()` wird zum Inkrementieren des eingehenden Ports verwendet, sodass der zweite virtuelle Computer den eingehenden Port 5001, der dritte den Port 5002 usw. erhält.

Das entsprechende JSON-Beispiel in der Resource Manager-Vorlage finden Sie unter [Inbound NAT Rules](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L260)(Eingehende NAT-Regeln). 

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/loadBalancers/inboundNatRules",
  "name": "[concat(variables('loadBalancerName'), '/', 'RDP-VM', copyIndex())]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "load-balancer-nat-rule"
  },
  "copy": {
    "name": "lbNatLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "dependsOn": [
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
  ],
  "properties": {
    "frontendIPConfiguration": {
      "id": "[variables('ipConfigID')]"
    },
    "protocol": "tcp",
    "frontendPort": "[copyIndex(5000)]",
    "backendPort": 3389,
    "enableFloatingIP": false
  }
}
```

Hier ist ein Beispiel für eine NAT-Regel für eingehenden Datenverkehr im Azure-Portal angegeben. Für jeden virtuellen Computer der Bereitstellung wird eine RDP-NAT-Regel erstellt.

![Eingehende NAT-Regel](./media/virtual-machines-windows-dotnet-core/natrule-win.png)

Ausführliche Informationen zum Azure-Netzwerklastenausgleich finden Sie unter [Lastenausgleich für Azure-Infrastrukturdienste](virtual-machines-windows-load-balance.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="deploy-multiple-vms"></a>Bereitstellen mehrerer VMs
Damit eine Verfügbarkeitsgruppe oder der Lastenausgleich effektiv funktioniert, sind mehrere virtuelle Computer erforderlich. Sie können mehrere VMs mit der Kopierfunktion der Azure Resource Manager-Vorlage bereitstellen. Bei Verwendung der Kopierfunktion ist es nicht nötig, eine endliche Anzahl von virtuellen Computern zu definieren, sondern dieser Wert kann während der Bereitstellung dynamisch angegeben werden. Bei der Kopierfunktion wird die Anzahl von zu erstellenden Instanzen berücksichtigt und die richtige Anzahl von virtuellen Computern und den dazugehörigen Ressourcen bereitgestellt.

In der Vorlage des Music Store-Beispiels wird ein Parameter definiert, für den die Instanzanzahl verwendet wird. Diese Anzahl wird in der Vorlage jeweils verwendet, wenn virtuelle Computer und die dazugehörigen Ressourcen erstellt werden.

```json
"numberOfInstances": {
  "type": "int",
  "minValue": 1,
  "defaultValue": 2,
  "metadata": {
    "description": "Number of VM instances to be created behind load balancer."
  }
},
```

In der Ressource des virtuellen Computers werden für die Kopierschleife ein Name und die Anzahl von Instanzen angegeben, die vom Parameter zum Steuern der Anzahl von sich ergebenden Kopien verwendet werden.

Das entsprechende JSON-Beispiel in der Resource Manager-Vorlage finden Sie unter [Virtual Machine Copy Function](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L290)(Kopierfunktion für virtuelle Computer). 

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/virtualMachines",
  "name": "[concat(variables('vmName'),copyindex())]",
  "location": "[resourceGroup().location]",
  "copy": {
    "name": "virtualMachineLoop",
    "count": "[parameters('numberOfInstances')]"
  }
```

Auf die aktuelle Iteration der Kopierfunktion kann mit der Funktion `copyIndex()` zugegriffen werden. Der Wert der Funktion zum Kopieren des Index kann zum Benennen von virtuellen Computern und anderen Ressourcen verwendet werden. Wenn zwei Instanzen eines virtuellen Computers bereitgestellt werden, müssen sie unterschiedliche Namen haben. Die Funktion `copyIndex()` kann als Teil des VM-Namens verwendet werden, um einen eindeutigen Namen zu erstellen. Ein Beispiel für die Funktion `copyindex()` zu Benennungszwecken finden Sie in der virtuellen Computerressource. Hier ist der Computername eine Verkettung des Parameters `vmName` mit der Funktion `copyIndex()`. 

Das entsprechende JSON-Beispiel in der Resource Manager-Vorlage finden Sie unter [Copy Index Function](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L309)(Funktion „copyIndex“). 

```json
"osProfile": {
  "computerName": "[concat(variables('vmName'),copyindex())]",
  "adminUsername": "[parameters('adminUsername')]",
  "adminPassword": "[parameters('adminPassword')]"
}
```

Die Funktion `copyIndex` wird in der Vorlage des Music Store-Beispiels mehrfach verwendet. Ressourcen und Funktionen, für die `copyIndex` genutzt wird, umfassen die spezifischen Bestandteile einer Einzelinstanz des virtuellen Computers, z.B. Netzwerkschnittstelle, Lastenausgleichsregeln und Funktionsabhängigkeiten. 

Weitere Informationen zur Verwendung der Kopierfunktion finden Sie unter [Erstellen mehrerer Instanzen von Ressourcen in Azure Resource Manager](../azure-resource-manager/resource-group-create-multiple.md).

## <a name="next-step"></a>Nächster Schritt
<hr>

[Schritt 4: Anwendungsbereitstellung mit Azure Resource Manager-Vorlagen](virtual-machines-windows-dotnet-core-5-app-deployment.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)


