<properties
	pageTitle="Klonen eines klassischen virtuellen Computers für Azure Resource Manager mithilfe von PowerShell-Skripts"
	description="In diesem Artikel erfahren Sie, wie Sie einen einzelnen klassischen virtuellen Computer mit PowerShell-Skripts für Azure Resource Manager klonen."
	services="virtual-machines-windows"
	documentationCenter=""
	authors="singhkays"
	manager="drewm"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/03/2016"
	ms.author="singhkay"/>

# Klonen eines klassischen virtuellen Computers für Azure Resource Manager mithilfe von PowerShell-Skripts

In diesem Artikel erfahren Sie, wie Sie die Skripts unter [Azure/classic-iaas-resourcemanager-migration](https://github.com/Azure/classic-iaas-resourcemanager-migration) verwenden, um einen **einzelnen** klassischen virtuellen Computer mit dem Azure Resource Manager-Bereitstellungsmodell zu klonen.

>[AZURE.IMPORTANT]Beim Klonen mit diesen Skripts kommt es für Ihren klassischen virtuellen Computer zu Ausfallzeiten. Informationen zur plattformgestützten Migration finden Sie hier:
- [Plattformgestützte Migration von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zum Azure Resource Manager-Stapel](./virtual-machines-windows-migration-classic-resource-manager.md)
- [Ausführliche technische Informationen zur plattformgestützten Migration vom klassischen Bereitstellungsmodell zu Azure Resource Manager](./virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
- [Migrieren von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager mithilfe von Azure PowerShell](./virtual-machines-windows-ps-migration-classic-resource-manager.md)

## Beschaffen der Skripts

>[AZURE.NOTE]Die PowerShell-Skripts werden vom Microsoft-Support nicht offiziell unterstützt. Daher sind sie auf GitHub als Open-Source-Versionen verfügbar, und wir sind jederzeit offen für Vorschläge in Bezug auf Fixes oder zusätzliche Szenarien.

Sie erhalten die Skripts, indem Sie entweder die ZIP-Datei aus dem oben angegebenen Repository herunterladen oder das Repository mit **einem** der beiden folgenden Befehle klonen:

```
git clone https://github.com/Azure/classic-iaas-resourcemanager-migration.git
```

**ODER**

```
git clone git@github.com:Azure/classic-iaas-resourcemanager-migration.git
```

## Importieren des Migrationsmoduls

Der nächste Schritt ist das Importieren des Moduls in der PowerShell-Sitzung. Dies erreichen Sie mit dem unten angegebenen Befehl.

```
Import-Module .\asm2arm.psd1
```

## Authentifizieren mit dem klassischen Bereitstellungsmodell und Azure Resource Manager-Abonnements

Bevor Sie den klassischen virtuellen Computer klonen können, müssen Sie Ihre PowerShell-Sitzung sowohl für das klassische Bereitstellungsmodell als auch für den Azure Resource Manager-Stapel authentifizieren. Hierfür können Sie die folgenden Cmdlets verwenden:

```
Add-AzureAccount
Login-AzureRmAccount
```

>[AZURE.IMPORTANT]Stellen Sie sicher, dass Standardabonnements ausgewählt sind, indem Sie `Select-AzureSubscription` für das klassische Bereitstellungsmodell und Set-AzureRmContext für Azure Resource Manager verwenden.

## Verwenden der Skripts

### Cmdlets

Beim Importieren des Moduls werden der Sitzung die beiden unten angegebenen Cmdlets hinzugefügt.

```
Add-AzureSMVmToRM
New-AzureSmToRMDeployment
```

Mit diesen Cmdlets wird die unten angegebene Funktionalität ermöglicht.

#### Add-AzureSMVmToRM
- Generiert eine Gruppe von Azure Resource Manager-Vorlagen und imperative PowerShell-Skripts (zum Kopieren der Datenträgerblobs und bei Vorhandensein von VM-Agent-Erweiterungen für die VM) für einen virtuellen Computer.

>[AZURE.IMPORTANT]Wenn der Switch `-Deploy` angegeben ist, ruft dieses Cmdlet das `New-AzureSmToRMDeployment`-Cmdlet auf, um die oben generierten Azure Resource Manager-Vorlagen und imperativen PowerShell-Skripts bereitzustellen.

#### New-AzureSmToRMDeployment
- Stellt die Vorlagen und imperativen PowerShell-Skripts bereit, die vom `Add-AzureSMVmToRM`-Cmdlet generiert werden, um die Migration zu starten.

### Identifizieren des zu klonenden virtuellen Computers

Dies erreichen Sie, indem Sie entweder den Zustand des klassischen virtuellen Computers in einer Variablen speichern und diese an das `Add-AzureSMVmToRM`-Cmdlet übergeben oder indem Sie direkt die VM-Parameter `ServiceName` und `Name` verwenden. Nachdem Sie den klassischen virtuellen Computer identifiziert haben, können Sie das `Add-AzureSMVmToRM`-Cmdlet verwenden, um es zu klonen.

```
$vm = Get-AzureVm -ServiceName acloudservice -Name atestvm
Add-AzureSMVmToRM -VM $vm -ResourceGroupName aresourcegroupname -DiskAction CopyDisks -OutputFileFolder D:\myarmtemplates -AppendTimeStampForFiles -Deploy
```

**ODER**

```
Add-AzureSMVmToRM -ServiceName acloudservice -Name atestvm -ResourceGroupName aresourcegroupname -DiskAction CopyDisks -OutputFileFolder D:\myarmtemplates -AppendTimeStampForFiles -Deploy
```

>[AZURE.IMPORTANT]Da wir den Switch `-Deploy` verwenden, ist der Einsatz des `New-AzureSmToRMDeployment`-Cmdlets in den obigen Beispielen nicht erforderlich.

## Wie funktioniert die Migration mit diesen Skripts?

Die Cmdlets durchlaufen die Schritte für das Klonen des klassischen virtuellen Computers und generieren Ressourcen als benutzerdefinierte PowerShell-Hashtabellen für Ressourcenanbieter für die Bereiche Speicher, Netzwerk und Compute. Diese Hashtabellen, die für die Ressourcen stehen, werden an ein Array angefügt und später dann per Serialisierung in JSON-Code in eine Vorlage geändert und in eine Datei geschrieben.

Die Vorlage erstellt Dateien je nach Vorhandensein von Agent-Erweiterungen für den klassischen virtuellen Computer und des DiskAction-Optionswerts. Die Dateien werden alle in dem Verzeichnis abgelegt, das unter dem Parameter OutputFileFolder angegeben ist. Die Dateien lauten:

1. `<ServiceName>-<VMName>-setup<optional timestamp>.json`: Diese Datei steht für die Ressourcen, die vorbereitet werden müssen, bevor der klassische virtuelle Computer geklont wird, und die meist für alle nachfolgenden VMs gleich sind. (Der Zustand wird zwischen den nachfolgenden Ausführungen nicht beibehalten. Da vor dem (imperativen) Kopieren eines Blobs aber ein Speicherkonto bereitgestellt werden muss, ist es nur logisch, ähnliche Ressourcen zu gruppieren.)

2.  `<ServiceName>-<VMName>-deploy<optional timestamp>.json`: Enthält die Vorlage für den virtuellen Resource Manager-Computer.
3.  `<ServiceName>-<VMName>-parameters<optional timestamp>.json`: Enthält die eigentlichen Parameter, die an die Vorlagen übergeben werden.
4.  `<ServiceName>-<VMName>-setextensions<optional timestamp>.json`: Eine Reihe von PowerShell-Cmdlets, die ausgeführt werden, um die VM-Agent-Erweiterungen festzulegen.
4.  `<ServiceName>-<VMName>-copydisks<optional timestamp>.json`: Eine Gruppe von PowerShell-Cmdlets, die zum Kopieren von Datenträgerblobs ausgeführt werden, wenn die Option CopyDisks angegeben ist.

Wenn das Flag „-Deploy“ festgelegt ist, stellt das Cmdlet nach dem Generieren der Dateien die <Dienstname>-<VM-Name>-setup.json-Vorlage bereit und kopiert die Datenträgerblobs des virtuellen Quellcomputers, wenn der DiskAction-Parameter auf CopyDisks festgelegt ist. Anschließend stellt es die <Dienstname>-<VM-Name>-deploy.json-Vorlage bereit und verwendet dabei die <Dienstname>-<VM-Name>-parameters.json-Datei für Parameter. Nach der Bereitstellung des virtuellen Computers wird, falls vorhanden, ein imperatives Skript (für VM-Agent-Erweiterungen) oder ein Skript zum Kopieren der Datenträger ausgeführt.

### Netzwerkdetails
Für das Cmdlet besteht die Absicht, nicht die klassischen Netzwerkeinstellungen für Resource Manager zu klonen. Die Netzwerkeinrichtungen werden so genutzt, dass das Klonen des virtuellen Computers selbst am einfachsten ist. Folgendes passiert unter den unterschiedlichen Bedingungen:

1.  Kein virtuelles Netzwerk für die Zielressourcengruppe
    - Virtueller Quellcomputer befindet sich nicht in einem Subnetz: Ein virtuelles Standardnetzwerk mit dem Adressraum 10.0.0.0/16 wird zusammen mit einem Subnetz mit dem Adressraum 10.0.0.0/22 erstellt.
    - Virtueller Quellcomputer befindet sich in einem Subnetz: Das virtuelle Netzwerk des virtuellen Computers wird erkannt, und die Spezifikation des virtuellen Netzwerks und die Subnetze werden kopiert.
2.  Die Zielressourcengruppe verfügt über ein virtuelles Netzwerk mit dem Namen `<VM virtual network>arm` (die Zeichenfolge „arm“ wird angefügt).
    - Wenn das virtuelle Netzwerk über ein Subnetz mit dem gleichen Namen und Adressraum verfügt, können Sie es verwenden.
    - Falls kein passendes Subnetz gefunden wird, können Sie nach einem Adressblock der vorhandenen Subnetze mit einer Maske mit 22 Bit suchen und verwenden.

## Vergleich des Klonens und der Plattformmigration

Es gibt einige Unterschiede zwischen dem derzeitigen Klonansatz und der plattformgestützten Migration.

### Klonen


| Vorteile | Nachteile |
|--------------------------------------------------------|:----------------------------------------------------------------:|
| Klonen eines virtuellen Computers innerhalb eines Clouddiensts | Ausfallzeiten für die VMs, wenn sie heruntergefahren werden müssen |
| Klonen eines virtuellen Computers in einem VNET oder außerhalb eines VNET | Lange Verzögerungen bei einem Szenario, für das das Kopieren von Datenträgerblobs erforderlich ist |
| | Änderung der Netzwerkkonfiguration für die internen IP-Adressen (VIPs) der VM |


### Plattformmigration


| Vorteile | Nachteile |
|----------------------------------|:-------------------------------------------:|
| Bei den meisten VM-Konfigurationen in einem virtuellen Netzwerk kommt es nicht zu Ausfallzeiten | Es müssen entweder alle VMs in einem Clouddienst oder das VNET mit allen bereitgestellten Ressourcen zusammen migriert werden |
 
 
## Nicht unterstützte Szenarien

**Folgende Bereiche sind durch Klonskripts nicht abgedeckt:**

 1. Beenden eines ausgeführten virtuellen Computers
 2. Ändern von Daten/Datenträgern
 3. Klonen ausgeführter VMs
 4. Automatisches Klonen mehrerer VMs in einem komplexen Szenario
 5. Klonen der gesamten ASM-Netzwerkkonfiguration
 6. Erstellt VMs mit Lastenausgleich (Es wird vorausgesetzt, dass dies eine Konfiguration ist, die vom Azure-Experten explizit durchgeführt werden muss.)
 
 
## Getestete Konfigurationen

Das _Add-AzureSMVmToRM_-Cmdlet wurde anhand der folgenden Testfälle überprüft:

| # | Beschreibung |
|:---|:---|
| 1 | Virtueller Windows-Computer mit einem vorhandenen Betriebssystem-Datenträger |
| 2 | Virtueller Linux-Computer mit einem vorhandenen Betriebssystem-Datenträger |
| 3 | Virtueller Windows-Computer mit einem vorhandenen Betriebssystem-Datenträger und Datenträgern für Daten |
| 4 | Virtueller Linux-Computer mit einem vorhandenen Betriebssystem-Datenträger und Datenträgern für Daten |
| 5 | Virtueller Windows-Computer mit neuem Betriebssystem-Datenträger aus dem Image-Katalog |
| 6 | Virtueller Linux-Computer mit neuem Betriebssystem-Datenträger aus dem Image-Katalog |
| 7 | Virtueller Windows-Computer mit neuem Betriebssystem-Datenträger und leeren Datenträgern |
| 8 | Virtueller Linux-Computer mit neuem Betriebssystem-Datenträger und leeren Datenträgern |
| 9 | Virtueller Windows-Computer mit öffentlichen Endpunkten |
| 10 | Virtueller Linux-Computer mit öffentlichen Endpunkten |
| 11 | Virtueller Windows-Computer mit WinRM-Zertifikat |
| 12 | Virtueller Windows-Computer in einem VNET und Subnetz |
| 13 | Virtueller Linux-Computer in einem VNET und Subnetz |
| 14 | Virtueller Windows-Computer mit benutzerdefinierten Erweiterungen |
| 15 | Virtueller Windows-Computer in einer Verfügbarkeitsgruppe |
| 16 | Virtueller Windows-Computer in einer Verfügbarkeitsgruppe mit mehreren Datenträgern, öffentlichen Endpunkten, in einem VNET und Subnetz und mit benutzerdefinierten Erweiterungen |
| 17 | Virtueller Linux-Computer in einer Verfügbarkeitsgruppe mit mehreren Datenträgern, öffentlichen Endpunkten, in einem VNET und Subnetz und mit benutzerdefinierten Erweiterungen |

## Hinweise
1. Wenn mehrere VMs nacheinander mit kurzen Pausen dazwischen geklont werden, kann es für die öffentlichen IP-Adressen aufgrund des Aktualisierungszeitraums für den DNS-Cache zu DNS-Namenskonflikten kommen.

<!---HONumber=AcomDC_0817_2016-->