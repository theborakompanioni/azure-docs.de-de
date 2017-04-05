---
title: Azure-Netzwerkschnittstellen | Microsoft-Dokumentation
description: "Hier erfahren Sie, wie Sie Netzwerkschnittstellen (NICs) erstellen und löschen und wie Sie ihnen öffentliche und private IP-Adressen zuweisen. Außerdem erfahren Sie, wie Sie NICs an virtuelle Azure-Computer anfügen und trennen."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/14/2017
ms.author: jdial
translationtype: Human Translation
ms.sourcegitcommit: 07635b0eb4650f0c30898ea1600697dacb33477c
ms.openlocfilehash: f691f3886fce217ea784237f03a4f02ed58e12ee
ms.lasthandoff: 03/28/2017


---

# <a name="network-interfaces"></a>Netzwerkschnittstellen

Hier finden Sie Informationen zu Netzwerkschnittstellen (NICs) und zu deren Verwendung. Bei einer NIC handelt es sich um eine Verbindung zwischen einem virtuellen Azure-Computer (Virtual Machine, VM) und dem zugrunde liegenden Softwarenetzwerk. Die folgende Abbildung veranschaulicht die Funktionen einer NIC:

![Netzwerkschnittstelle](./media/virtual-network-network-interface/nic.png)

In diesem Artikel erfahren Sie, wie Sie die in der Abbildung gezeigten Konzepte verwenden. Klicken Sie auf eines der folgenden Konzepte, um direkt zum entsprechenden Abschnitt des Artikels zu gelangen:

- [Netzwerkschnittstellen:](#nics) Eine NIC ist mit einem Subnetz in einem virtuellen Azure-Netzwerk (VNET) verbunden. In der Abbildung verfügt **VM1** über zwei angefügte NICs und **VM2** über eine einzelne angefügte NIC. Jede NIC ist mit dem gleichen VNET, aber mit unterschiedlichen Subnetzen verbunden. In diesem Abschnitt wird erläutert, wie Sie vorhandene NICs auflisten und NICs erstellen, ändern und löschen.
- [IP-Konfigurationen:](#ip-configs) Jeder NIC ist mindestens eine IP-Konfiguration zugeordnet. Jeder IP-Konfiguration ist eine private IP-Adresse zugewiesen. Einer IP-Konfiguration kann auch eine private IP-Adresse zugeordnet werden. In der Abbildung ist **NIC1** und **NIC3** jeweils eine IP-Konfiguration zugeordnet. **NIC2** sind hingegen zwei IP-Konfigurationen zugeordnet. Bei NIC1 und NIC3 sind der zugewiesenen IP-Konfiguration öffentliche IP-Adressen zugewiesen. Bei NIC2 ist hingegen keiner der IP-Konfigurationen eine öffentliche IP-Adresse zugewiesen. Dieser Abschnitt enthält Schritte zum Erstellen, Ändern und Löschen von IP-Konfigurationen, denen mithilfe statischer oder dynamischer Zuweisungsmethoden private IP-Adressen zugewiesen wurden. Darüber hinaus erfahren Sie in diesem Abschnitt, wie Sie einer IP-Konfiguration öffentliche IP-Adressen zuordnen und die Zuordnung öffentlicher IP-Adressen aufheben.
- [Netzwerksicherheitsgruppen:](#nsgs) Netzwerksicherheitsgruppen (NSGs) enthalten mindestens eine Ein- oder Ausgangssicherheitsregel. Mit diesen Regeln wird gesteuert, welche Art von eingehendem und ausgehendem Datenverkehr für eine Netzwerkschnittstelle und/oder für ein Subnetz zulässig ist. In der Abbildung ist **NIC1** und **NIC3** eine NSG zugeordnet. Bei **NIC2** ist dies nicht der Fall. In diesem Abschnitt erfahren Sie, wie Sie die NSGs anzeigen, die auf eine NIC angewendet wurden, wie Sie einer NIC eine NSG hinzufügen und wie Sie eine NSG von einer NIC entfernen.
- [Virtuelle Computer:](#vms) An einen virtuellen Computer ist mindestens eine NIC angefügt. Je nach Größe des virtuellen Computers können aber auch mehrere NICs angefügt sein. Informationen zur unterstützten NIC-Anzahl der einzelnen VM-Größen finden Sie im entsprechenden Artikel für [Windows](../virtual-machines/virtual-machines-windows-sizes.md) bzw. für [Linux](../virtual-machines/virtual-machines-linux-sizes.md). In diesem Abschnitt erfahren Sie, wie Sie virtuelle Computer mit einzelnen oder mehreren NICs erstellen und wie Sie NICs an vorhandene virtuelle Computer anfügen und NICs von vorhandenen virtuellen Computern trennen.

Falls Sie noch nicht mit NICs und virtuellen Computern in Azure vertraut sind, empfiehlt es sich, vor der Lektüre dieses Artikels zunächst die Übung unter [Erstellen Ihres ersten virtuellen Netzwerks](virtual-network-get-started-vnet-subnet.md) zu absolvieren. In dieser Übung werden Sie an VNETs und virtuelle Computer herangeführt.

Dieser Artikel bezieht sich auf VNETs und NICs, die im Rahmen des Azure Resource Manager-Bereitstellungsmodells erstellt wurden. Microsoft empfiehlt für die Erstellung von Ressourcen die Verwendung des Resource Manager-Bereitstellungsmodells (anstelle des klassischen Bereitstellungsmodells). Die Unterschiede zwischen den beiden Modellen werden im Artikel [Azure Resource Manager-Bereitstellung im Vergleich zur klassischen Bereitstellung: Grundlegendes zu Bereitstellungsmodellen und zum Status von Ressourcen](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) erläutert.

In den restlichen Abschnitten dieses Artikels finden Sie Anleitungen für sämtliche NIC-bezogene Aufgaben. Die einzelnen Abschnitte enthalten jeweils Folgendes:
- Schritte zum Ausführen der Aufgabe innerhalb des Azure-Portals. Um die Schritte ausführen zu können, müssen Sie beim [Azure-Portal](http://portal.azure.com) angemeldet sein. Falls Sie noch nicht über ein Konto verfügen, können Sie sich für ein [kostenloses Testkonto](https://azure.microsoft.com/free) registrieren.
- Befehle zum Ausführen der Aufgabe mithilfe von Azure PowerShell sowie Links zur Befehlsreferenz für den Befehl. Installieren und konfigurieren Sie PowerShell gemäß der Anleitung im Artikel [Get started with Azure PowerShell cmdlets](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json) (Erste Schritte mit Azure PowerShell-Cmdlets). Hilfe und Beispiele für PowerShell-Befehle erhalten Sie durch Eingabe von `get-help <command> -full`.
- Befehle zum Ausführen der Aufgabe mithilfe der Azure-Befehlszeilenschnittstelle (Command-Line Interface, CLI) sowie Links zur Befehlsreferenz für den Befehl. Installieren und konfigurieren Sie die Azure-Befehlszeilenschnittstelle gemäß der Anleitung im Artikel [Install Azure CLI 2.0](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json) (Installieren von Azure CLI 2.0). Hilfe zu den Befehlen der Befehlszeilenschnittstelle erhalten Sie durch Eingabe von `az <command> -h`.

## <a name="nics"></a>Netzwerkschnittstellen
Mithilfe der Schritte in den folgenden Abschnitten können Sie Netzwerkschnittstellen und Einstellungen erstellen, anzeigen, ändern und löschen:

### <a name="create-nic"></a>Erstellen einer Netzwerkschnittstelle

Eine NIC kann an einen virtuellen Computer angefügt oder eigenständig sein. Informationen zum Anfügen einer NIC an einen virtuellen Computer finden Sie im Abschnitt [Anfügen einer NIC an einen virtuellen Computer](#vm-attach-nic) dieses Artikels.

Gehen Sie zum Erstellen einer NIC wie folgt vor:

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind, beim [Azure-Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie im Artikel [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie im oberen Bereich des Azure-Portals im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Netzwerkschnittstellen* ein. Wenn **Netzwerkschnittstellen** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie auf dem daraufhin angezeigten Blatt **Netzwerkschnittstellen** auf **+ Hinzufügen**.
4. Das Blatt **Netzwerkschnittstelle erstellen** wird angezeigt. Geben Sie hier Werte für die folgenden Einstellungen an, und klicken Sie anschließend auf **Erstellen**:

    |**Einstellung**|**Erforderlich?**|**Details**|
    |---|---|---|
    |**Name**|Ja|Nach Erstellung der NIC kann der Name nicht mehr geändert werden. Der Name muss innerhalb der ausgewählten Ressourcengruppe eindeutig sein. Benennungsvorschläge finden Sie im Artikel [Naming conventions](/azure/architecture/best-practices/naming-conventions?toc=%2fazure%2fvirtual-network%2ftoc.json#naming-rules-and-restrictions) (Benennungskonventionen).|
    |**Virtuelles Netzwerk**|Ja|Eine NIC kann nur mit einem VNET verbunden werden, das sich im gleichen Abonnement und am gleichen Standort befindet wie die NIC. Der virtuelle Computer, an den die NIC angefügt ist, muss sich ebenfalls am gleichen Standort und im gleichen Abonnement befinden wie die NIC. Sind keine VNETs aufgeführt, müssen Sie eines erstellen. Gehen Sie zum Erstellen eines VNETs gemäß der Anleitung im Artikel [Erstellen eines virtuellen Netzwerks im Azure-Portal](virtual-networks-create-vnet-arm-pportal.md) vor. Nach der Erstellung einer NIC können Sie das VNET, mit dem sie verbunden ist, nicht mehr ändern.|
    |**Subnetz**|Ja|Wählen Sie ein Subnetz innerhalb des ausgewählten VNETs aus. Das Subnetz, mit dem die NIC verbunden ist, kann nach der Erstellung geändert werden.|
    |**Zuweisung der privaten IP-Adresse**|Ja| Eine private IP-Adresse wird der NIC bei ihrer Erstellung vom Azure-DHCP-Server zugewiesen. Der DHCP-Server weist eine verfügbare Adresse aus dem Subnetzadressbereich zu, der für das Subnetz definiert ist, mit dem Sie die NIC verbinden. **Dynamisch:** Azure kann einer NIC eine andere Adresse zuweisen, wenn der virtuelle Computer, an den sie angefügt ist, gestartet wird, nachdem er sich zuvor im Zustand „Beendet (Zuordnung aufgehoben)“ befand. Die Adresse bleibt unverändert, wenn der virtuelle Computer neu gestartet wird, ohne sich zuvor im Zustand „Beendet (Zuordnung aufgehoben)“ befunden zu haben. **Statisch:** Statische Adressen bleiben unverändert, bis Sie sie ändern oder die NIC gelöscht wird. Die Zuweisungsmethode kann nach Erstellung der NIC geändert werden.|
    |**Netzwerksicherheitsgruppe**|Nein|Mithilfe von Netzwerksicherheitsgruppen können Sie den ein- und ausgehenden Netzwerkdatenverkehr für eine NIC steuern. Weitere Informationen zu NSGs finden Sie im Artikel [Steuern des Netzwerkdatenverkehrs mit Netzwerksicherheitsgruppen](virtual-networks-nsg.md). Auf eine NIC kann maximal eine Netzwerksicherheitsgruppe (NSG) angewendet werden. Maximal eine NSG kann auch auf das Subnetz angewendet werden, mit dem die NIC verbunden ist. Wenn eine NSG auf eine NIC und auf das Subnetz angewendet wird, mit dem sie verbunden ist, kommt es manchmal zu unerwarteten Ergebnissen. Informationen zum Behandeln von Problemen mit NSGs, die auf NICs angewendet wurden, finden Sie im Artikel [Problembehandlung bei Netzwerksicherheitsgruppen über das Azure-Portal](virtual-network-nsg-troubleshoot-portal.md#view-effective-security-rules-for-a-network-interface).|
    |**Abonnement**|Ja| Der virtuelle Computer, an den Sie eine NIC anfügen, und das VNET, mit dem Sie eine Verbindung herstellen, müssen sich im gleichen Abonnement befinden.|
    |**Ressourcengruppe**|Ja| Die NIC kann sich in der gleichen (oder in einer anderen) Ressourcengruppe befinden wie der virtuelle Computer, an den Sie sie anfügen, oder wie das VNET, mit dem Sie eine Verbindung herstellen.|
    |**Standort**|Ja|Der virtuelle Computer, an den Sie eine NIC anfügen, und das VNET, mit dem Sie eine Verbindung herstellen, müssen sich am gleichen Standort befinden.|

Das Azure-Portal erstellt eine primäre IP-Konfiguration namens **ipconfig1** mit einer dynamischen privaten IP-Adresse und ordnet sie der NIC zu, die Sie erstellen. Weitere Informationen zu IP-Konfigurationen finden Sie in diesem Artikel im Abschnitt [IP-Konfigurationen](#ip-configs). Beim Erstellen der NIC können Sie weder den Namen der vom Portal erstellten IP-Konfiguration angeben noch eine statische private IP-Adresse oder eine öffentliche IP-Adresse zuweisen. Wenn Sie die NIC mithilfe von PowerShell oder mithilfe der Befehlszeilenschnittstelle erstellen, können Sie den Namen der IP-Konfiguration und eine statische private IP-Adresse angeben sowie eine öffentliche IP-Adresse zuweisen. Sie können nach der Erstellung der NIC die Zuweisungsmethode für die private IP-Adresse ändern sowie festlegen, ob der NIC eine öffentliche IP-Adresse zugeordnet sein soll. Eine Anleitung zum Ändern der Einstellungen einer bereits erstellten NIC finden Sie in diesem Artikel im Abschnitt [Ändern einer IP-Konfiguration](#change-ip-config).

>[!Note]
> Azure weist der NIC erst dann eine MAC-Adresse zu, wenn die NIC an einen virtuellen Computer angefügt wurde und der virtuelle Computer erstmals gestartet wird. Die MAC-Adresse, die Azure der NIC zuweist, kann nicht angegeben werden. Die MAC-Adresse bleibt der NIC zugewiesen, bis die NIC gelöscht oder die private IP-Adresse, die der primären IP-Konfiguration der primären NIC zugewiesen ist, geändert wird. Weitere Informationen zu IP-Konfigurationen finden Sie in diesem Artikel im Abschnitt [IP-Konfigurationen](#ip-configs).

|**Tool**|**Befehl**|
|:---|:---|
|**BEFEHLSZEILENSCHNITTSTELLE (CLI)**|[az network nic create](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|**PowerShell**|[New-AzureRmNetworkInterface](/powershell/resourcemanager/azurerm.network/v3.4.0/new-azurermnetworkinterface/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|

### <a name="view-nics"></a>Anzeigen und Ändern von Netzwerkschnittstellen und Einstellungen

Gehen Sie zum Anzeigen und Ändern von Netzwerkschnittstellen und Einstellungen wie folgt vor:

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind, beim [Azure-Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie im Artikel [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie im oberen Bereich des Azure-Portals im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Netzwerkschnittstellen* ein. Wenn **Netzwerkschnittstellen** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie auf dem daraufhin angezeigten Blatt **Netzwerkschnittstellen** auf die NIC, deren Einstellungen Sie anzeigen oder ändern möchten.
4. Auf dem Blatt, das für die ausgewählte NIC angezeigt wird, stehen folgende Einstellungen zur Verfügung:
    - **Übersicht:** Liefert Informationen zur NIC. Hierzu zählen beispielsweise die zugewiesenen IP-Adressen, das VNET/Subnetz, mit dem die NIC verbunden ist, und der virtuelle Computer, an den die NIC angefügt ist (sofern zutreffend). Die folgende Abbildung zeigt die Übersichtseinstellungen für eine NIC namens **mywebserver256**:   ![Übersicht über die Netzwerkschnittstelle](./media/virtual-network-network-interface/nic-overview.png)
    - **IP-Konfigurationen:** Einer NIC ist mindestens eine IP-Konfiguration zugewiesen, sie kann aber auch über mehrere zugewiesene IP-Konfigurationen verfügen. Informationen zur maximal unterstützten Anzahl von IP-Konfigurationen für eine NIC finden Sie im Artikel [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). Jeder IP-Konfiguration ist eine private IP-Adresse zugewiesen. Optional kann ihr auch eine öffentliche IP-Adresse zugeordnet sein. Führen Sie zum Ändern der Anzeige die Schritte im Abschnitt [Hinzufügen einer sekundären IP-Konfiguration zu einer NIC](#create-ip-config), [Ändern einer IP-Konfiguration](#change-ip-config) oder [Löschen einer IP-Konfiguration](#delete-ip-config) dieses Artikels aus.
    - **DNS-Server:** Sie können angeben, welcher DNS-Server einer NIC durch die Azure-DHCP-Server zugewiesen wird. Wählen Sie zwischen dem internen Azure-DNS-Server und einem benutzerdefinierten DNS-Server. Führen Sie zum Ändern der Anzeige die Schritte im Abschnitt [Ändern der DNS-Einstellungen für eine NIC](#dns) dieses Artikels aus.
    - **Netzwerksicherheitsgruppe (NSG):** Gibt Aufschluss darüber, ob der NIC eine NSG zugeordnet ist. Falls der NIC eine NSG zugeordnet ist, wird der Name der zugeordneten NSG angezeigt. Führen Sie zum Ändern der Anzeige die Schritte im Abschnitt [Zuordnen einer NSG zu einer Netzwerkschnittstelle oder Aufheben der NSG-Zuordnung](#associate-nsg) dieses Artikels aus.
    - **Eigenschaften:** Zeigt wichtige Einstellungen für die NIC an (unter anderem die MAC-Adresse und das Abonnement, dem sie angehört). Sie können eine NIC in eine andere Ressourcengruppe oder in ein anderes Abonnement verschieben. Dabei müssen jedoch auch alle Ressourcen verschoben werden, die mit der NIC in Verbindung stehen. Wenn die NIC also etwa an einen virtuellen Computer angefügt ist, müssen Sie auch den virtuellen Computer sowie alle damit zusammenhängenden Ressourcen verschieben. Informationen zum Verschieben einer NIC finden Sie im Artikel [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../azure-resource-manager/resource-group-move-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json#use-portal). In dem Artikel erfahren Sie, welche Voraussetzungen erfüllt sein müssen und wie Sie Ressourcen über das Azure-Portal, mithilfe von PowerShell oder unter Verwendung der Azure-Befehlszeilenschnittstelle verschieben.
    - **Effektive Sicherheitsregeln:** Sicherheitsregeln werden aufgelistet, wenn die NIC an einen aktiven virtuellen Computer angefügt und der NIC und/oder dem Subnetz, mit dem sie verbunden ist, eine NSG zugeordnet ist. Weitere Informationen zur Anzeige finden Sie im Artikel [Problembehandlung bei Netzwerksicherheitsgruppen über das Azure-Portal](virtual-network-nsg-troubleshoot-portal.md#view-effective-security-rules-for-a-network-interface). Weitere Informationen zu NSGs finden Sie im Artikel [Steuern des Netzwerkdatenverkehrs mit Netzwerksicherheitsgruppen](virtual-networks-nsg.md).
    - **Effektive Routen:** Routen werden aufgelistet, wenn die NIC an einen aktiven virtuellen Computer angefügt ist. Bei den Routen handelt es sich um eine Kombination aus den Azure-Standardrouten, benutzerdefinierten Routen und BGP-Routen, die ggf. für das Subnetz vorhanden sind, mit dem die NIC verbunden ist. Weitere Informationen zur Anzeige finden Sie im Artikel [Problembehandlung bei Routen über das Azure-Portal](virtual-network-routes-troubleshoot-portal.md#view-effective-routes-for-a-network-interface). Weitere Informationen zu benutzerdefinierten Routen finden Sie im Artikel [Benutzerdefinierte Routen und IP-Weiterleitung](virtual-networks-udr-overview.md).
    - **Allgemeine Azure Resource Manager-Einstellungen:** Weitere Informationen zu allgemeinen Azure Resource Manager-Einstellungen finden Sie unter [Aktivitätsprotokolle](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#activity-logs), [Zugriffssteuerung](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#access-control), [Tags](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#tags), [Sperren von Ressourcen, um unerwartete Änderungen zu verhindern](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json) und [Exportieren der Vorlage aus der Ressourcengruppe](../azure-resource-manager/resource-manager-export-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json#export-the-template-from-resource-group).

|**Tool**|**Befehl**|
|---|---|
|**BEFEHLSZEILENSCHNITTSTELLE (CLI)**|[az network nic list](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#list): Anzeigen von im Abonnement enthaltenen NICs. [az network nic show](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#show): Anzeigen der Einstellungen für eine NIC.|
|**PowerShell**|[Get-AzureRmNetworkInterface](/powershell/resourcemanager/azurerm.network/v3.4.0/get-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json): Anzeigen von im Abonnement enthaltenen NICs oder der Einstellungen für eine NIC.|

### <a name="dns"></a>Ändern der DNS-Einstellungen für eine NIC

Führen Sie zum Ändern der DNS-Einstellungen für eine NIC die folgenden Schritte aus. Der DNS-Server wird dem virtuellen Computer durch den Azure-DHCP-Server zugewiesen. Weitere Informationen zu Namensauflösungseinstellungen für eine NIC finden Sie im Artikel [Namensauflösung für virtuelle Computer und Rolleninstanzen](virtual-networks-name-resolution-for-vms-and-role-instances.md).

1. Führen Sie für die NIC, deren Einstellungen Sie ändern möchten, die Schritte 1 bis 3 im Abschnitt [Anzeigen und Ändern von Netzwerkschnittstellen und Einstellungen](#view-nics) dieses Artikels aus.
2. Klicken Sie auf dem Blatt für die ausgewählte NIC auf **DNS-Server**.
3. Klicken Sie anschließend auf eine der folgenden Optionen:
    - **Von virtuellem Netzwerk erben** (Standardeinstellung): Wählen Sie diese Option, wenn die DNS-Servereinstellung des virtuellen Netzwerks übernommen werden soll, mit dem die NIC verbunden ist. Auf der VNET-Ebene ist entweder ein benutzerdefinierter DNS-Server oder der von Azure bereitgestellte DNS-Server definiert. Der von Azure bereitgestellte DNS-Server kann Namen für Ressourcen auflösen, die mit dem gleichen VNET verbunden sind, aber nicht für Ressourcen, die mit anderen VNETs verbunden sind.
    - **Benutzerdefiniert:** Sie können einen eigenen DNS-Server für eine VNET-übergreifende Namensauflösung konfigurieren. Geben Sie die IP-Adresse des Servers ein, den Sie als DNS-Server verwenden möchten. Die von Ihnen angegebene DNS-Serveradresse wird nur dieser NIC zugewiesen und überschreibt alle DNS-Einstellungen für das VNET, mit dem die NIC verbunden ist.
4. Klicken Sie auf **Speichern**.

|**Tool**|**Befehl**|
|---|---|
|**BEFEHLSZEILENSCHNITTSTELLE (CLI)**|[az network nic update](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|**PowerShell**|[Set-AzureRmNetworkInterface](/powershell/resourcemanager/azurerm.network/v3.4.0/set-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json)|

### <a name="ip-forwarding"></a>Ändern der IP-Weiterleitung für eine NIC

Eine IP-Weiterleitung ermöglicht dem virtuellen Computer, an den eine NIC angefügt ist, Folgendes:
- Empfangen von Netzwerkdatenverkehr, der nicht für eine der IP-Adressen bestimmt ist, die einer der IP-Konfigurationen für die NIC zugewiesen ist
- Senden von Netzwerkdatenverkehr mit einer IP-Quelladresse, die nicht der Adresse entspricht, die einer der zugehörigen IP-Konfigurationen zugewiesen ist

Die Einstellung muss für jede an den virtuellen Computer angefügte NIC aktiviert werden, die Datenverkehr empfängt, den der virtuelle Computer weiterleiten muss. Ein virtueller Computer kann Datenverkehr unabhängig davon weiterleiten, ob er über mehrere angefügte NICs oder nur über eine einzelne angefügte NIC verfügt. Bei der IP-Weiterleitung handelt es sich zwar um eine Azure-Einstellung, auf dem virtuellen Computer muss aber auch eine Anwendung ausgeführt werden, die den Datenverkehr weiterleiten kann – beispielsweise eine Firewall, ein WAN-Optimierer oder ein Lastenausgleich. Wenn ein virtueller Computer Netzwerkanwendungen ausführt, wird er häufig als virtuelles Netzwerkgerät (Network Virtual Appliance, NVA) bezeichnet. Eine Liste mit bereitstellungsbereiten NVAs finden Sie im [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances). Die IP-Weiterleitung wird in der Regel mit benutzerdefinierten Routen verwendet. Weitere Informationen zu benutzerdefinierten Routen finden Sie im Artikel [Benutzerdefinierte Routen und IP-Weiterleitung](virtual-networks-udr-overview.md).

Führen Sie zum Ändern der IP-Weiterleitungseinstellungen für eine NIC die folgenden Schritte aus:

1. Führen Sie für die NIC, die Sie anpassen möchten, die Schritte 1 bis 3 im Abschnitt [Anzeigen und Ändern von Netzwerkschnittstellen und Einstellungen](#view-nics) dieses Artikels aus.
2. Klicken Sie auf dem Blatt für die ausgewählte NIC auf „IP-Konfigurationen“.
3. Klicken Sie auf **Aktiviert** oder **Deaktiviert** (Standardeinstellung), um die Einstellung zu ändern.
4. Klicken Sie auf **Speichern**.

|**Tool**|**Befehl**|
|---|---|
|**BEFEHLSZEILENSCHNITTSTELLE (CLI)**|[az network nic update](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|**PowerShell**|[Set-AzureRmNetworkInterface](/powershell/resourcemanager/azurerm.network/v3.4.0/set-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json)|

### <a name="subnet"></a>Ändern des Subnetzes, mit dem eine NIC verbunden ist

Im Gegensatz zum VNET können Sie das Subnetz ändern, mit dem eine NIC verbunden ist. Wenn Sie das Subnetz ändern möchten, müssen allen der NIC zugeordneten IP-Konfigurationen dynamische private IP-Adressen zugewiesen sein. Gehen Sie zum Ändern der Subnetzes, mit dem eine NIC verbunden ist, wie folgt vor:

1. Führen Sie für die NIC, die Sie mit einem anderen Subnetz verbinden möchten, die Schritte 1 bis 3 im Abschnitt [Anzeigen und Ändern von Netzwerkschnittstellen und Einstellungen](#view-nics) dieses Artikels aus.
2. Klicken Sie auf dem Blatt für die ausgewählte NIC auf **IP-Konfigurationen**. Falls einer der privaten IP-Adressen für eine der aufgeführten IP-Konfigurationen eine private IP-Adresse mit der statischen Methode zugewiesen wurde, muss die Methode mithilfe der folgenden Schritte auf „Dynamisch“ festgelegt werden. Falls die Adressen mithilfe der dynamischen Methode zugewiesen wurden, fahren Sie mit Schritt 3 fort:
    - Klicken Sie in der Liste mit den IP-Konfigurationen auf die statische IP-Adresse für die IP-Konfiguration, die Sie ändern möchten.
    - Klicken Sie auf dem Blatt, das für die IP-Konfiguration angezeigt wird, unter der Zuweisungsmethode****auf **Dynamisch**.
    - Klicken Sie auf **Speichern**.
3. Wählen Sie in der Dropdownliste **Subnetz** das Subnetz aus, mit dem Sie die NIC verbinden möchten.
4. Klicken Sie auf **Speichern**. Neue dynamische Adressen werden aus dem Subnetzadressbereich zugewiesen. Bei Bedarf können Sie anschließend statische IP-Adressen aus dem neuen Subnetzadressbereich zuweisen.

|**Tool**|**Befehl**|
|---|---|
|**BEFEHLSZEILENSCHNITTSTELLE (CLI)**|[az network nic ip-config update](/cli/azure/network/nic/ip-config?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|**PowerShell**|[Set-AzureRmNetworkInterfaceIpConfig](/powershell/resourcemanager/azurerm.network/v3.4.0/set-azurermnetworkinterfaceipconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|


### <a name="delete-nic"></a>Löschen einer Netzwerkschnittstelle

Sie können eine NIC löschen, sofern sie an keinen virtuellen Computer angefügt ist. Sollte sie an einen virtuellen Computer angefügt sein, kann sie erst gelöscht werden, nachdem sie von dem virtuellen Computer getrennt wurde. Die Schritte zum Trennen einer NIC von einem virtuellen Computer finden Sie in diesem Artikel im Abschnitt [Trennen einer NIC von einem virtuellen Computer](#vm-detach-nic).

1. Führen Sie für die NIC, die Sie löschen möchten, die Schritte 1 und 2 im Abschnitt [Anzeigen und Ändern von Netzwerkschnittstellen und Einstellungen](#view-nics) dieses Artikels aus.
2. Klicken Sie mit der rechten Maustaste auf die NIC, die Sie löschen möchten, und klicken Sie anschließend auf **Löschen**.
3. Klicken Sie auf **Ja**, um das Löschen der NIC zu bestätigen.

Wenn Sie eine NIC löschen, werden alle ihr zugewiesenen MAC- und IP-Adressen freigegeben.

|**Tool**|**Befehl**|
|---|---|
|**BEFEHLSZEILENSCHNITTSTELLE (CLI)**|[az network nic delete](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|**PowerShell**|[Remove-AzureRmNetworkInterface](/powershell/resourcemanager/azurerm.network/v3.1.0/remove-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="ip-configs"></a>IP-Konfigurationen
Jede NIC verfügt über mindestens eine IP-Konfiguration (die so genannte **primäre** Konfiguration). Zusätzlich können einer NIC *sekundäre* IP-Konfigurationen zugeordnet werden. Die Anzahl von IP-Adressen, die Sie einer NIC zuweisen können, ist begrenzt. Ausführliche Informationen finden Sie im Artikel [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). Für jede IP-Konfiguration gilt Folgendes:
- Ihr ist eine einzelne private IP-Adresse zugewiesen (entweder mit der statischen oder mit der dynamischen Zuweisungsmethode). Dynamische IP-Adressen können sich ändern, wenn ein virtueller Computer gestartet wird, der sich zuvor im Zustand „Beendet (Zuordnung aufgehoben)“ befand. Statische IP-Adressen werden von einer NIC erst freigegeben, wenn die NIC gelöscht wird.
- Ihr kann eine einzelne öffentliche IP-Adresse zugeordnet sein.

Die Azure-DHCP-Server weisen der NIC innerhalb des Betriebssystems des virtuellen Computers die private IP-Adresse für die primäre IP-Konfiguration der NIC zu.

In Szenarien wie den folgenden kann es hilfreich sein, einer NIC mehrere IP-Adressen zuzuweisen:
- Hosten mehrerer Websites oder Dienste mit unterschiedlichen IP-Adressen und SSL-Zertifikaten auf einem einzelnen Server
- Verwenden eines virtuellen Computers als virtuelles Netzwerkgerät (etwa als Firewall oder Lastenausgleich)
- Fähigkeit zum Hinzufügen einer privaten IP-Adresse für eine der NICs zu einem Azure Load Balancer-Back-End-Pool hinzufügen. Bisher konnte nur die primäre IP-Adresse für die primäre NIC einem Back-End-Pool hinzugefügt werden. Weitere Informationen dazu, wie in Konfigurationen mit mehreren IPs ein Lastenausgleich durchgeführt werden kann, finden Sie im Artikel [Load balancing multiple IP configurations](../load-balancer/load-balancer-multiple-ip.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Lastenausgleich bei Konfigurationen mit mehreren IPs).

Die Anzahl öffentlicher IP-Adressen, die im Rahmen eines Abonnements verwendet werden können, sowie die Anzahl privater IP-Adressen, die einer NIC zugewiesen werden können, sind begrenzt. Weitere Informationen zu diesen Einschränkungen finden Sie im Artikel [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

### <a name="create-ip-config"></a>Hinzufügen einer sekundären IP-Konfiguration zu einer NIC

Einer NIC können beliebig viele IP-Konfigurationen hinzugefügt werden (im Rahmen der im Artikel [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) aufgeführten Grenzwerte). Gehen Sie wie folgt vor, um einer NIC eine IP-Konfiguration hinzuzufügen:

1. Führen Sie für die NIC, der Sie eine IP-Konfiguration hinzufügen möchten, die Schritte 1 bis 3 im Abschnitt [Anzeigen und Ändern von Netzwerkschnittstellen und Einstellungen](#view-nics) dieses Artikels aus.
2. Klicken Sie auf dem Blatt für die ausgewählte NIC auf **IP-Konfigurationen**.
3. Klicken Sie auf dem Blatt, das für IP-Konfigurationen geöffnet wird, auf **+ Hinzufügen**.
4. Geben Sie Folgendes an, und klicken Sie anschließend auf **OK**, um das Blatt **IP-Konfiguration hinzufügen** zu schließen:

    |**Einstellung**|**Erforderlich?**|**Details**|
    |---|---|---|
    |**Name**|Ja|Muss für die NIC eindeutig sein.|
    |**Typ**|Ja|Da Sie die IP-Konfiguration einer vorhandenen NIC hinzufügen und jede NIC über eine primäre IP-Konfiguration verfügen muss, steht nur die Option **Sekundär** zur Verfügung.|
    |**Zuweisung der privaten IP-Adresse**|Ja|Adressen vom Typ **Dynamisch** können sich ändern, wenn der virtuelle Computer neu gestartet wird, nachdem er sich zuvor im Zustand „Beendet (Zuordnung aufgehoben)“ befand. Adressen vom Typ **Statisch** werden erst freigegeben, wenn die NIC gelöscht wird. Geben Sie eine IP-Adresse aus dem Subnetzadressbereich an, die derzeit nicht von einer anderen IP-Konfiguration verwendet wird.|
    |**Öffentliche IP-Adresse**|Nein|**Deaktiviert:** Der IP-Konfiguration ist derzeit keine öffentliche IP-Adressressource zugeordnet. **Aktiviert:** Wählen Sie eine vorhandene öffentliche IP-Adresse aus, oder erstellen Sie eine neue. Eine Anleitung zur Erstellung einer öffentlichen IP-Adresse finden Sie im Artikel [Public IP addresses](virtual-network-public-ip-address.md#create) (Öffentliche IP-Adressen).|
5. Fügen Sie dem Betriebssystem des virtuellen Computers manuell sekundäre private IP-Adressen hinzu, wie im Artikel [Zuweisen von mehreren IP-Adressen zu virtuellen Computern mithilfe des Azure-Portals](virtual-network-multiple-ip-addresses-portal.md#os-config) beschrieben. Fügen Sie dem Betriebssystem des virtuellen Computers keine öffentlichen IP-Adressen hinzu.

|**Tool**|**Befehl**|
|---|---|
|**BEFEHLSZEILENSCHNITTSTELLE (CLI)**|[az network nic ip-config create](/cli/azure/network/nic/ip-config?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|**PowerShell**|[Add-AzureRmNetworkInterfaceIpConfig](/powershell/resourcemanager/azurerm.network/v3.4.0/add-azurermnetworkinterfaceipconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

### <a name="change-ip-config"></a>Ändern einer IP-Konfiguration

Gehen Sie zum Ändern der Einstellungen für private und öffentliche IP-Adressen einer beliebigen primären oder sekundären IP-Konfiguration wie folgt vor:

1. Führen Sie für die NIC, die Sie anpassen möchten, die Schritte 1 bis 3 im Abschnitt [Anzeigen und Ändern von Netzwerkschnittstellen und Einstellungen](#view-nics) dieses Artikels aus.
2. Klicken Sie auf dem Blatt für die ausgewählte NIC auf **IP-Konfigurationen**.
3. Klicken Sie auf dem Blatt, das für IP-Konfigurationen geöffnet wird, in der Liste auf die IP-Konfiguration, die Sie ändern möchten.
4. Ändern Sie die Einstellungen nach Bedarf. Informationen zu den Einstellungen finden Sie in diesem Artikel im Abschnitt [Hinzufügen einer IP-Konfiguration](#create-ip-config). Klicken Sie anschließend auf **Speichern**, um das Blatt für die ausgewählte IP-Konfiguration zu schließen.

>[!NOTE]
>Falls die primäre NIC über mehrere IP-Konfigurationen verfügt und Sie die private IP-Adresse der primären IP-Konfiguration ändern, müssen Sie unter Windows alle sekundären IP-Adressen manuell wieder der NIC zuweisen. (Unter Linux ist dies nicht erforderlich.) Wie Sie IP-Adressen im Rahmen eines Betriebssystems manuell einer NIC zuweisen, erfahren Sie im Artikel [Zuweisen von mehreren IP-Adressen zu virtuellen Computern mithilfe des Azure-Portals](virtual-network-multiple-ip-addresses-portal.md#os-config). Fügen Sie dem Betriebssystem des virtuellen Computers keine öffentlichen IP-Adressen hinzu.

|**Tool**|**Befehl**|
|---|---|
|**BEFEHLSZEILENSCHNITTSTELLE (CLI)**|[az network nic ip-config update](/cli/azure/network/nic/ip-config?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|**PowerShell**|[Set-AzureRMNetworkInterfaceIpConfig](/powershell/resourcemanager/azurerm.network/v3.4.0/set-azurermnetworkinterfaceipconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

### <a name="delete-ip-config"></a>Löschen einer sekundären IP-Konfiguration aus einer NIC

Führen Sie die folgenden Schritte aus, um eine sekundäre IP-Konfiguration aus einer NIC zu löschen:

1. Führen Sie für die NIC, die Sie anpassen möchten, die Schritte 1 bis 3 im Abschnitt [Anzeigen und Ändern von Netzwerkschnittstellen und Einstellungen](#view-nics) dieses Artikels aus.
2. Klicken Sie auf dem Blatt für die ausgewählte NIC auf **IP-Konfigurationen**.
3. Klicken Sie mit der rechten Maustaste auf die sekundäre IP-Konfiguration, die Sie löschen möchten, und klicken Sie anschließend auf **Löschen**. Falls der Konfiguration eine öffentliche IP-Adressressource zugeordnet war, wird die Zuweisung der Ressource zur IP-Konfiguration aufgehoben, die Ressource wird jedoch nicht gelöscht.
4. Schließen Sie das Blatt **IP-Konfigurationen**.

|**Tool**|**Befehl**|
|---|---|
|**BEFEHLSZEILENSCHNITTSTELLE (CLI)**|[az network nic ip-config delete](/cli/azure/network/nic/ip-config?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|**PowerShell**|[Remove-AzureRmNetworkInterfaceIpConfig](/powershell/resourcemanager/azurerm.network/v3.4.0/remove-azurermnetworkinterfaceipconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## <a name="nsgs"></a>Netzwerksicherheitsgruppen
Eine Netzwerksicherheitsgruppe (NSG) enthält eine Liste mit Ein- und Ausgangsregeln, die Netzwerkdatenverkehr für eine NIC zulassen oder verweigern. Einer NIC und dem Subnetz, mit dem die NIC verbunden ist, muss nicht unbedingt eine NSG zugeordnet sein. Es ist jedoch möglich, einer NIC und/oder dem Subnetz, mit dem die NIC verbunden ist, eine NSG zuzuordnen. NSGs mit Regeln, die für alle mit dem Subnetz verbundenen NICs gelten, werden üblicherweise Subnetzen zugeordnet. Eine NSG mit präziseren Regeln kann dann auf einzelne NICs angewendet werden. Weitere Informationen zu Netzwerksicherheitsgruppen finden Sie im Artikel [Steuern des Netzwerkdatenverkehrs mit Netzwerksicherheitsgruppen](virtual-networks-nsg.md).

### <a name="associate-nsg"></a>Zuordnen einer NSG zu einer Netzwerkschnittstelle oder Aufheben der NSG-Zuordnung

Führen Sie die folgenden Schritte aus, um eine NSG einer NIC zuzuordnen oder um die Zuordnung einer NSG zu einer NIC aufzuheben:

1. Führen Sie für die NIC, der Sie eine NSG zuordnen oder für die Sie die Zuordnung einer NSG aufheben möchten, die Schritte 1 bis 3 im Abschnitt [Anzeigen und Ändern von Netzwerkschnittstellen und Einstellungen](#view-nics) dieses Artikels aus.
2. Klicken Sie auf dem Blatt für die ausgewählte NIC auf **Netzwerksicherheitsgruppe**. Im oberen Bereich des daraufhin angezeigten Blatts befindet **Bearbeiten**. Falls der NIC derzeit keine NSG zugeordnet ist, wird für **Netzwerksicherheitsgruppe** der Text *Keine* angezeigt. Falls einer NIC bereits eine NSG zugeordnet ist, wird für **Netzwerksicherheitsgruppe** der *Name der NSG* angezeigt.
3. Klicken Sie auf **Bearbeiten**.
4. Klicken Sie auf **Netzwerksicherheitsgruppen**. Sollten keine Netzwerksicherheitsgruppen aufgeführt werden, sind in Ihrem Abonnement keine vorhanden. Führen Sie zum Erstellen einer NSG die Schritte im Artikel [Steuern des Netzwerkdatenverkehrs mit Netzwerksicherheitsgruppen](virtual-networks-create-nsg-arm-pportal.md) aus.
5. Klicken Sie auf dem angezeigten Blatt **Netzwerksicherheitsgruppe auswählen** in der Liste auf eine vorhandene NSG, um sie der NIC zuzuordnen, oder klicken Sie auf **Keine**, um die Zuordnung einer NSG aufzuheben, die derzeit einer NIC zugeordnet ist.
6. Klicken Sie auf **Speichern**.

|**Tool**|**Befehl**|
|---|---|
|**BEFEHLSZEILENSCHNITTSTELLE (CLI)**|[az network nic update](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|**PowerShell**|[Set-AzureRmNetworkInterface](/powershell/resourcemanager/azurerm.network/v3.4.0/set-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vms"></a>Anfügen von NICs an einen virtuellen Computer oder Trennen von NICs von einem virtuellen Computer

Sie können eine vorhandene NIC an einen virtuellen Computer anfügen, wenn Sie diesen erstellen, oder eine vorhandene NIC an einen vorhandenen virtuellen Computer anfügen. Außerdem können Sie eine NIC von einem vorhandenen virtuellen Computer trennen, sofern dieser über mindestens zwei NICs verfügt. Das Portal erstellt zwar eine NIC, wenn Sie einen virtuellen Computer erstellen, Folgendes ist über das Portal jedoch nicht möglich:

- Angeben einer vorhandenen NIC, um sie beim Erstellen des virtuellen Computers anzufügen
- Erstellen eines virtuellen Computers mit mehreren angefügten NICs
- Angeben eines Namens für die NIC (wird vom Portal mit einem Standardnamen erstellt)
- Angeben, dass zum Zuweisen der privaten IP-Adresse die statische Methode verwendet werden soll. Das Portal weist automatisch eine dynamische private IP-Adresse zu. Die Zuweisungsmethode kann jedoch geändert werden, nachdem das Portal die NIC erstellt hat.

Sie können PowerShell oder die Befehlszeilenschnittstelle verwenden, um eine NIC oder einen virtuellen Computer mit den zuvor erwähnten Attributen zu erstellen, für die das Portal nicht verwendet werden kann. Berücksichtigen Sie bei den Aufgaben in den folgenden Abschnitten die folgenden Einschränkungen und Verhaltensweisen:

- Unterschiedliche VM-Größen unterstützen unterschiedlich viele NICs. Weitere Informationen zur unterstützten NIC-Anzahl der einzelnen VM-Größen finden Sie im entsprechenden Artikel für [Linux](../virtual-machines/virtual-machines-linux-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) oder [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 
- Standardmäßig wird die erste NIC, die an einen virtuellen Computer angefügt wird, als die *primäre* NIC definiert. Alle anderen NICs, die an den virtuellen Computer angefügt werden, sind *sekundäre* NICs.
- Standardmäßig wird sämtlicher ausgehende Datenverkehr des virtuellen Computers über die IP-Adresse gesendet, die der primären IP-Konfiguration der primären NIC zugewiesen ist. Sie können selbstverständlich im Betriebssystem des virtuellen Computers steuern, welche IP-Adresse für ausgehenden Datenverkehr verwendet wird.
- In der Vergangenheit mussten alle virtuellen Computer in der gleichen Verfügbarkeitsgruppe über eine einzelne NIC (oder über mehrere NICs) verfügen. Inzwischen kann eine Verfügbarkeitsgruppe virtuelle Computer mit einer beliebigen Anzahl von NICs enthalten. Ein virtueller Computer kann allerdings nur zum Zeitpunkt der Erstellung zu einer Verfügbarkeitsgruppe hinzugefügt werden. Weitere Informationen zu Verfügbarkeitsgruppen finden Sie im Artikel [Verwalten der Verfügbarkeit virtueller Windows-Computer in Azure](../virtual-machines/virtual-machines-windows-manage-availability.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) Artikel.
- An den gleichen virtuellen Computer angefügte NICs können zwar mit unterschiedlichen Subnetzen eines VNETs verbunden sein, die NICs müssen jedoch alle mit dem gleichen VNET verbunden sein.
- Sie können eine beliebige IP-Adresse für eine beliebige IP-Konfiguration einer beliebigen primären oder sekundären NIC einem Back-End-Pool von Azure Load Balancer hinzufügen. Bisher konnte nur die primäre IP-Adresse für die primäre NIC einem Back-End-Pool hinzugefügt werden.
- Beim Löschen eines virtuellen Computers werden die an ihn angefügten NICs nicht gelöscht. Wenn ein virtueller Computer gelöscht wird, werden die NICs vom virtuellen Computer getrennt. Sie können die NICs an andere virtuelle Computer anfügen oder löschen.

### <a name="vm-create"></a>Anfügen von NICs beim Erstellen eines virtuellen Computers

Über das Azure-Portal ist es nicht möglich, vorhandene NICs an einen neuen virtuellen Computer anzufügen oder einen virtuellen Computer mit mehreren NICs zu erstellen. Mit dem folgenden Befehl der Azure-Befehlszeilenschnittstelle bzw. mit dem folgenden PowerShell-Befehl können Sie vorhandene NICs beim Erstellen eines virtuellen Computers anfügen:

- **Befehlszeilenschnittstelle:** [az vm create](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#create)
- **PowerShell:** [New-AzureRmVM](/powershell/resourcemanager/azurerm.compute/v2.5.0/new-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)

### <a name="vm-view-nic"></a>Anzeigen von NICs, die an einen virtuellen Computer angefügt sind

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement die Rolle „Besitzer“, „Mitwirkender“ oder „Netzwerkmitwirkender“ zugewiesen ist, beim [Azure-Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen zu Konten finden Sie im Artikel [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie im oberen Bereich des Azure-Portals im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Virtuelle Computer* ein. Wenn **Virtuelle Computer** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie auf dem daraufhin angezeigten Blatt **Virtuelle Computer** auf den Namen des virtuellen Computers, für den Sie angefügte Netzwerkschnittstellen anzeigen möchten.
4. Klicken Sie auf dem Blatt **Virtueller Computer**, das für den ausgewählten virtuellen Computer angezeigt wird, auf **Netzwerkschnittstellen**.

|**Tool**|**Befehl**|
|---|---|
|**BEFEHLSZEILENSCHNITTSTELLE (CLI)**|[az vm show](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#show)|
|**PowerShell**|[Get-AzureRmVM](/powershell/resourcemanager/azurerm.compute/v1.3.4/get-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

### <a name="vm-attach-nic"></a>Anfügen einer NIC an einen vorhandenen virtuellen Computer

Der virtuelle Computer, an den Sie eine NIC anfügen möchten, muss mehrere NICs unterstützen und sich im Zustand „Beendet (Zuordnung aufgehoben)“ befinden. NICs können nicht über das Azure-Portal an einen vorhandenen virtuellen Computer angefügt werden. Verwenden Sie den folgenden Befehl der Azure-Befehlszeilenschnittstelle bzw. den folgenden PowerShell-Befehl, um NICs an virtuelle Computer anzufügen:

- **Befehlszeilenschnittstelle:** [az vm nic add](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#add)
- **PowerShell:** [Add-AzureRmVMNetworkInterface](/powershell/resourcemanager/azurerm.compute/v2.5.0/add-azurermvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json)

### <a name="vm-detach-nic"></a>Trennen einer NIC von einem vorhandenen virtuellen Computer

Der virtuelle Computer, von dem Sie eine NIC trennen möchten, muss sich im Zustand „Beendet (Zuordnung aufgehoben)“ befinden und über mindestens zwei angefügte NICs verfügen. Sie können jede beliebige NIC trennen, an den virtuellen Computer muss aber immer mindestens eine NIC angefügt sein. Wenn Sie eine primäre NIC trennen, weist Azure das Primärattribut einer verbleibenden angefügten NIC zu, die am längsten an den virtuellen Computer angefügt ist. Sie können allerdings auch selbst eine beliebige NIC als primäre NIC festlegen. Über das Azure-Portal können Sie weder NICs von einem virtuellen Computer trennen noch das Primärattribut für eine NIC festlegen. Diese beiden Aktionen können jedoch über die Befehlszeilenschnittstelle oder mithilfe von PowerShell ausgeführt werden. Verwenden Sie den folgenden Befehl der Azure-Befehlszeilenschnittstelle bzw. den folgenden PowerShell-Befehl, um NICs von virtuellen Computern zu trennen:

- **Befehlszeilenschnittstelle:** [az vm nic remove](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#remove)
- **PowerShell:** [Remove-AzureRMVMNetworkInterface](/powershell/resourcemanager/azurerm.compute/v2.5.0/remove-azurermvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json)

## <a name="next-steps"></a>Nächste Schritte
In den folgenden Artikeln erfahren Sie, wie Sie mithilfe von Skripts einen virtuellen Computer mit mehreren NICs oder IP-Konfigurationen erstellen:

|**Aufgabe**|**Tool**|
|---|---|
|**Erstellen eines virtuellen Computers mit mehreren Netzwerkschnittstellenkarten (NICs)**|[Befehlszeilenschnittstelle](virtual-network-deploy-multinic-arm-cli.md) und [PowerShell](virtual-network-deploy-multinic-arm-ps.md)|
|**Erstellen eines virtuellen Computers mit einer einzelnen NIC und mehreren zugewiesenen IP-Adressen**|[Befehlszeilenschnittstelle](virtual-network-multiple-ip-addresses-cli.md) und [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|

