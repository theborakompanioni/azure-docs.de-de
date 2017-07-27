---
title: "Erstellen, Ändern und Löschen von Azure-Netzwerkschnittstellenkarten | Microsoft-Dokumentation"
description: "Erfahren Sie, was Netzwerkschnittstellenkarten (Network Interface Cards, NICs) sind und wie Sie sie erstellen, ihre Einstellungen ändern und sie löschen."
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
ms.date: 05/04/2017
ms.author: jdial
ms.translationtype: Human Translation
ms.sourcegitcommit: 07584294e4ae592a026c0d5890686eaf0b99431f
ms.openlocfilehash: 871d469ada9857af6d6fe70f1dd38385fb18e312
ms.contentlocale: de-de
ms.lasthandoff: 06/01/2017


---

# <a name="create-change-or-delete-network-interfaces"></a>Erstellen, Ändern oder Löschen von Netzwerkschnittstellenkarten

Erfahren Sie, wie Sie Netzwerkschnittstellenkarten (NICs) erstellen, ihre Einstellungen ändern und sie löschen. Eine NIC ermöglicht einem virtuellen Azure-Computer (VM) die Kommunikation mit dem Internet, Azure und lokalen Ressourcen. Wenn Sie im Azure-Portal eine VM erstellen, generiert das Portal für Sie eine NIC mit Standardeinstellungen. Sie können nach Wunsch NICs mit benutzerdefinierten Einstellungen erstellen und VMs bei deren Erstellung eine oder mehrere hinzufügen. Sie können auch die Standardeinstellungen vorhandener NICs ändern. In diesem Artikel wird erläutert, wie Sie NICs mit benutzerdefinierten Einstellungen erstellen, vorhandene NIC-Einstellungen ändern, z. B. die Netzwerkfilterzuweisung (Netzwerksicherheitsgruppen), Subnetzzuweisung, DNS-Servereinstellungen und IP-Weiterleitung, und NICs löschen.

Informationen zum Hinzufügen, Ändern oder Entfernen von IP-Adressen für eine NIC finden Sie im Artikel [Hinzufügen, Ändern oder Entfernen von IP-Adressen](virtual-network-network-interface-addresses.md). Informationen zum Hinzufügen von NICs zu oder Entfernen von NICs von VMs finden Sie im Artikel zum [Hinzufügen und Entfernen von NICs](virtual-network-network-interface-vm.md).


## <a name="before"></a>Voraussetzungen

Führen Sie zuerst die folgenden Aufgaben aus, ehe Sie die Schritte in den Abschnitten dieses Artikels durchführen:

- Lesen Sie den Artikel [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits), um mehr über Einschränkungen von NICs zu erfahren.
- Melden Sie sich mit einem Azure-Konto beim Azure-Portal, der Azure-Befehlszeilenschnittstelle (CLI) oder bei Azure PowerShell an. Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für ein [kostenloses Testkonto](https://azure.microsoft.com/free) registrieren.
- Bei Verwenden von PowerShell zum Ausführen der Aufgaben in diesem Artikel installieren und konfigurieren Sie Azure PowerShell gemäß den Anweisungen im Artikel [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Stellen Sie sicher, dass Sie die neueste Version der Azure PowerShell-Cmdlets installiert haben. Hilfe und Beispiele für PowerShell-Befehle erhalten Sie durch Eingabe von `get-help <command> -full`.
- Bei Verwenden der Azure CLI zum Ausführen der Aufgaben in diesem Artikel installieren und konfigurieren Sie die Azure CLI gemäß den Anweisungen im Artikel zum [Installieren und Konfigurieren der Azure CLI](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Stellen Sie sicher, dass Sie die neueste Version der Azure CLI installiert haben. Hilfe zu den Befehlen der Befehlszeilenschnittstelle erhalten Sie durch Eingabe von `az <command> --help`.

## <a name="create-nic"></a>Erstellen einer NIC
Wenn Sie im Azure-Portal eine VM erstellen, generiert das Portal für Sie eine NIC mit Standardeinstellungen. Wenn Sie lieber alle NIC-Einstellungen selbst angeben möchten, können Sie eine NIC mit benutzerdefinierten Einstellungen erstellen und an die VM bei deren Erstellung anfügen. Sie können außerdem eine NIC erstellen und an einen vorhandenen virtuellen Computer anfügen. Informationen zum Erstellen einer VM mit einer vorhandenen NIC oder zum Hinzufügen oder Entfernen von NICs bei vorhandenen VMs finden Sie im Artikel zum [Hinzufügen und Entfernen von NICs](virtual-network-network-interface-vm.md). Vor dem Erstellen einer NIC müssen Sie über ein virtuelles Netzwerk (VNet) am selben Standort und im selben Abonnement verfügen, in dem Sie eine NIC erstellen. Informationen zum Erstellen eines VNet finden Sie im Artikel [Erstellen eines VNet](virtual-networks-create-vnet-arm-pportal.md).

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind, beim [Azure-Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie im Artikel [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie im oberen Bereich des Azure-Portals im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Netzwerkschnittstellen* ein. Wenn **Netzwerkschnittstellen** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie auf dem daraufhin angezeigten Blatt **Netzwerkschnittstellen** auf **+ Hinzufügen**.
4. Das Blatt **Netzwerkschnittstelle erstellen** wird angezeigt. Geben Sie hier Werte für die folgenden Einstellungen an, und klicken Sie anschließend auf **Erstellen**:

    |Einstellung|Erforderlich|Details|
    |---|---|---|
    |Name|Ja|Der Name muss innerhalb der ausgewählten Ressourcengruppe eindeutig sein. Im Laufe der Zeit verfügen Sie wahrscheinlich über mehrere NICs in Ihrem Azure-Abonnement. Lesen Sie den Artikel zu [Benennungskonventionen](/azure/architecture/best-practices/naming-conventions?toc=%2fazure%2fvirtual-network%2ftoc.json#naming-rules-and-restrictions) mit Vorschlägen zum Festlegen einer Benennungskonvention zum Vereinfachen der Verwaltung mehrerer NICs. Nach Erstellung der NIC kann der Name nicht mehr geändert werden.|
    |Virtuelles Netzwerk|Ja|Wählen Sie ein VNet, mit dem die NIC verbunden werden soll. Eine NIC kann nur mit einem VNET verbunden werden, das sich im gleichen Abonnement und am gleichen Standort befindet wie die NIC. Nach der Erstellung einer NIC können Sie das VNET, mit dem sie verbunden ist, nicht mehr ändern. Der virtuelle Computer, dem Sie die NIC hinzufügen, muss sich ebenfalls am gleichen Standort und im gleichen Abonnement befinden wie die NIC.|
    |Subnetz|Ja|Wählen Sie ein Subnetz innerhalb des ausgewählten VNETs aus. Das Subnetz, mit dem die NIC verbunden ist, kann nach der Erstellung geändert werden.|
    |Zuweisung der privaten IP-Adresse|Ja| Wählen Sie aus den folgenden Methoden für die Zuweisung: **Dynamisch:** Bei Wahl dieser Option weist Azure automatisch eine verfügbare Adresse aus dem Adressraum des Subnetzes zu, das Sie ausgewählt haben. Azure kann einer NIC eine andere Adresse zuweisen, wenn der zugehörige virtuelle Computer gestartet wird, nachdem er sich zuvor im Zustand „Beendet (Zuordnung aufgehoben)“ befand. Die Adresse bleibt unverändert, wenn der virtuelle Computer neu gestartet wird, ohne sich zuvor im Zustand „Beendet (Zuordnung aufgehoben)“ befunden zu haben. **Statisch:** Bei Wahl dieser Option müssen Sie eine verfügbare IP-Adresse innerhalb des Adressraums des ausgewählten Subnetzes manuell zuweisen. Statische Adressen bleiben unverändert, bis Sie sie ändern oder die NIC gelöscht wird. Die Zuweisungsmethode kann nach Erstellung der NIC geändert werden. Der DHCP-Server von Azure weist diese Adresse der NIC innerhalb des Betriebssystems des virtuellen Computers zu.|
    |Netzwerksicherheitsgruppe|Nein| Belassen Sie diese Einstellung auf **Keine** festgelegt, wählen Sie eine vorhandene Netzwerksicherheitsgruppe (NSG), oder erstellen Sie eine NSG. NSGs ermöglichen das Filtern des in die NIC eingehenden und aus ihr ausgehenden Netzwerkdatenverkehrs. Weitere Informationen zu NSGs finden Sie im Artikel [Steuern des Netzwerkdatenverkehrs mit Netzwerksicherheitsgruppen](virtual-networks-nsg.md). Informationen zum Erstellen einer NSG finden Sie im Artikel zum [Erstellen einer NSG](virtual-networks-create-nsg-arm-pportal.md). Sie können auf eine NIC eine oder keine NSG anwenden. Maximal eine NSG kann auch auf das Subnetz angewendet werden, mit dem die NIC verbunden ist. Wenn eine NSG auf eine NIC und auf das Subnetz angewendet wird, mit dem die NIC verbunden ist, kommt es mitunter zu unerwarteten Ergebnissen. Informationen zum Behandeln von Problemen mit NSGs, die auf NICs und Subnetze angewendet werden, finden Sie im Artikel zur [Problembehandlung von Netzwerksicherheitsgruppen](virtual-network-nsg-troubleshoot-portal.md#a-namensgaview-effective-security-rules-for-a-network-security-group-nsg).|
    |Abonnement|Ja|Wählen Sie eines Ihrer Azure-[Abonnements](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) aus. Der virtuelle Computer, an den Sie eine NIC anfügen, und das VNET, mit dem Sie eine Verbindung herstellen, müssen sich im gleichen Abonnement befinden.|
    |Ressourcengruppe|Ja|Wählen Sie eine vorhandene [Ressourcengruppe](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) aus, oder erstellen Sie eine. Eine NIC kann sich in der gleichen (oder in einer anderen) Ressourcengruppe befinden wie der virtuelle Computer, an den Sie sie anfügen, oder wie das VNET, mit dem Sie eine Verbindung herstellen.|
    |Ort|Ja|Der virtuelle Computer, an den Sie eine NIC anfügen, und das VNet, mit dem Sie eine Verbindung herstellen, müssen sich am gleichen [Standort](https://azure.microsoft.com/regions) (auch als Region bezeichnet) befinden.|

Das Portal ermöglicht nicht das Zuweisen einer öffentlichen IP-Adresse zur NIC bei deren Erstellung. Es weist jedoch einer NIC eine öffentliche IP-Adresse zu, wenn Sie eine VM im Portal erstellen. Informationen zum Hinzufügen einer öffentlichen IP-Adresse zur NIC nach deren Erstellung finden Sie im Artikel zum [Hinzufügen, Ändern oder Entfernen von IP-Adressen](virtual-network-network-interface-addresses.md). Wenn Sie eine NIC mit einer öffentlichen IP-Adresse erstellen möchten, müssen Sie zum Erstellen der NIC die CLI oder PowerShell verwenden.

>[!Note]
> Azure weist der NIC erst dann eine MAC-Adresse zu, wenn die NIC an einen virtuellen Computer angefügt wurde und der virtuelle Computer erstmals gestartet wird. Die MAC-Adresse, die Azure der NIC zuweist, kann nicht angegeben werden. Die MAC-Adresse bleibt der NIC zugewiesen, bis die NIC gelöscht oder die private IP-Adresse, die der primären IP-Konfiguration der primären NIC zugewiesen ist, geändert wird. Weitere Informationen zu IP-Adressen und IP-Konfigurationen finden Sie im Artikel [Hinzufügen, Ändern oder Entfernen von IP-Adressen](virtual-network-network-interface-addresses.md).

**Befehle**

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network nic create](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[New-AzureRmNetworkInterface](/powershell/resourcemanager/azurerm.network/v3.4.0/new-azurermnetworkinterface/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|

## <a name="view-nics"></a>Anzeigen von NIC-Einstellungen

Sie können die meisten Einstellungen für eine NIC anzeigen und ändern.

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind, beim [Azure-Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie im Artikel [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie im oberen Bereich des Azure-Portals im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Netzwerkschnittstellen* ein. Wenn **Netzwerkschnittstellen** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie auf dem daraufhin angezeigten Blatt **Netzwerkschnittstellen** auf die NIC, deren Einstellungen Sie anzeigen oder ändern möchten.
4. Auf dem Blatt, das für die ausgewählte NIC angezeigt wird, stehen folgende Einstellungen zur Verfügung:
    - **Übersicht:** Liefert Informationen zur NIC. Hierzu zählen beispielsweise die zugewiesenen IP-Adressen, das VNet/Subnetz, mit dem die NIC verbunden ist, und der virtuelle Computer, an den die NIC angefügt ist (sofern zutreffend). Die folgende Abbildung zeigt die Übersicht über die Einstellungen für eine NIC mit dem Namen **mywebserver256**:   ![Übersicht über die Netzwerkschnittstelle](./media/virtual-network-network-interface/nic-overview.png) Sie können eine NIC in eine andere Ressourcengruppe oder ein anderes Abonnement verschieben, indem Sie neben der **Ressourcengruppe** oder dem **Abonnementnamen** auf (**ändern**) klicken. Wenn Sie die NIC verschieben, müssen Sie alle Ressourcen in ihrem Zusammenhang verschieben. Wenn die NIC also etwa an einen virtuellen Computer angefügt ist, müssen Sie auch den virtuellen Computer sowie alle anderen damit zusammenhängenden Ressourcen verschieben. Informationen zum Verschieben einer NIC finden Sie im Artikel [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../azure-resource-manager/resource-group-move-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json#use-portal). Hier erfahren Sie, welche Voraussetzungen erfüllt sein müssen und wie Sie Ressourcen über das Azure-Portal, mithilfe von PowerShell oder unter Verwendung der Azure CLI verschieben.
    - **IP-Konfigurationen:** Öffentliche und private IP-Adressen werden einer oder mehreren IP-Konfigurationen für eine NIC zugewiesen. Weitere Informationen zur maximal unterstützten Anzahl von IP-Konfigurationen für eine NIC finden Sie im Artikel [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). Jeder IP-Konfiguration ist eine private IP-Adresse zugewiesen. Optional kann ihr auch eine öffentliche IP-Adresse zugeordnet sein. Führen Sie zum Hinzufügen, Ändern oder Löschen von IP-Konfigurationen für die NIC die Schritte in den Abschnitten [Hinzufügen einer sekundären IP-Konfiguration zu einer NIC](virtual-network-network-interface-addresses.md#create-ip-config), [Ändern einer IP-Konfiguration](virtual-network-network-interface-addresses.md#change-ip-config) oder [Löschen einer IP-Konfiguration](virtual-network-network-interface-addresses.md#delete-ip-config) im Artikel [Hinzufügen, Ändern und Entfernen von IP-Adressen](virtual-network-network-interface-addresses.md) aus. Die IP-Weiterleitung und Subnetzzuweisung werden auch in diesem Abschnitt konfiguriert. Weitere Informationen zu diesen Einstellungen finden Sie im Abschnitt [Aktivieren und Deaktivieren der IP-Weiterleitung](#ip-forwarding) und [Ändern der Subnetzzuweisung](#subnet) in diesem Artikel.
    - **DNS-Server:** Sie können angeben, welcher DNS-Server einer NIC durch die Azure-DHCP-Server zugewiesen wird. Die NIC kann die Einstellung für das VNet übernehmen, mit dem sie verbunden ist. Sie kann auch eine benutzerdefinierte Einstellung haben, die die Einstellung für das VNet überschreibt, mit dem die NIC verbunden ist. Führen Sie zum Ändern der Anzeige die Schritte im Abschnitt [Ändern von DNS-Servern](#dns) dieses Artikels aus.
    - **Netzwerksicherheitsgruppe (NSG):** Gibt Aufschluss darüber, welche NSG der NIC zugeordnet ist (falls vorhanden). Eine NSG enthält ein- und ausgehende Regeln zum Filtern von Netzwerkdatenverkehr für die NIC. Falls der NIC eine NSG zugeordnet ist, wird der Name der zugeordneten NSG angezeigt. Führen Sie zum Ändern der Anzeige die Schritte im Abschnitt [Zuordnen einer NSG zu einer Netzwerkschnittstelle oder Aufheben der NSG-Zuordnung](#associate-nsg) dieses Artikels aus.
    - **Eigenschaften:** Zeigt wichtige Einstellungen für die NIC an, so z.B. die MAC-Adresse (leer, wenn die NIC an keine VM angefügt ist) und das Abonnement, dem sie angehört.
    - **Effektive Sicherheitsregeln:** Sicherheitsregeln werden aufgelistet, wenn die NIC an einen aktiven virtuellen Computer angefügt und der NIC und/oder dem Subnetz, mit dem sie verbunden ist, eine NSG zugeordnet ist. Weitere Informationen zu den angezeigten Informationen finden Sie im Artikel [Problembehandlung bei Netzwerksicherheitsgruppen über das Azure-Portal](virtual-network-nsg-troubleshoot-portal.md#a-namensgaview-effective-security-rules-for-a-network-security-group-nsg). Weitere Informationen zu NSGs finden Sie im Artikel [Steuern des Netzwerkdatenverkehrs mit Netzwerksicherheitsgruppen](virtual-networks-nsg.md).
    - **Effektive Routen:** Routen werden aufgelistet, wenn die NIC an einen aktiven virtuellen Computer angefügt ist. Bei den Routen handelt es sich um eine Kombination aus den Azure-Standardrouten, benutzerdefinierten Routen und BGP-Routen, die ggf. für das Subnetz vorhanden sind, mit dem die NIC verbunden ist. Weitere Informationen zur Anzeige finden Sie im Artikel [Problembehandlung bei Routen über das Azure-Portal](virtual-network-routes-troubleshoot-portal.md#view-effective-routes-for-a-network-interface). Weitere Informationen zu Azure-Standardeinstellungen und benutzerdefinierten Routen finden Sie im Artikel [Benutzerdefinierte Routen und IP-Weiterleitung](virtual-networks-udr-overview.md).
    - **Allgemeine Azure Resource Manager-Einstellungen:** Weitere Informationen zu allgemeinen Azure Resource Manager-Einstellungen finden Sie unter [Aktivitätsprotokolle](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#activity-logs), [Zugriffssteuerung](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#access-control), [Tags](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#tags), [Sperren von Ressourcen, um unerwartete Änderungen zu verhindern](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json) und [Exportieren der Vorlage aus der Ressourcengruppe](../azure-resource-manager/resource-manager-export-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json#export-the-template-from-resource-group).

**Befehle**

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network nic list](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#list): Anzeigen von im Abonnement enthaltenen NICs. [az network nic show](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#show): Anzeigen der Einstellungen für eine NIC.|
|PowerShell|[Get-AzureRmNetworkInterface](/powershell/resourcemanager/azurerm.network/v3.4.0/get-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json): Anzeigen von im Abonnement enthaltenen NICs oder der Einstellungen für eine NIC.|

## <a name="dns"></a>Ändern von DNS-Servern

Der DNS-Server wird vom Azure-DHCP-Server der NIC innerhalb des VM-Betriebssystems zugewiesen. Der DNS-Server entspricht der geltenden DNS-Servereinstellung für eine NIC. Weitere Informationen zu Namensauflösungseinstellungen für eine NIC finden Sie im Artikel [Namensauflösung für virtuelle Computer und Rolleninstanzen](virtual-networks-name-resolution-for-vms-and-role-instances.md). Die NIC kann die Einstellungen des VNet übernehmen oder eigene eindeutige Einstellungen verwenden, die die Einstellung für das VNet überschreiben.

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind, beim [Azure-Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie im Artikel [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie im oberen Bereich des Azure-Portals im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Netzwerkschnittstellen* ein. Wenn **Netzwerkschnittstellen** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie auf dem daraufhin angezeigten Blatt **Netzwerkschnittstellen** auf die NIC, deren Einstellungen Sie anzeigen oder ändern möchten.
4. Klicken Sie auf dem Blatt für die ausgewählte NIC unter **EINSTELLUNGEN** auf **DNS-Server**.
5. Klicken Sie anschließend auf eine der folgenden Optionen:
    - **Von virtuellem Netzwerk erben** (Standardeinstellung): Wählen Sie diese Option, wenn die DNS-Servereinstellung des virtuellen Netzwerks übernommen werden soll, mit dem die NIC verbunden ist. Auf der VNET-Ebene ist entweder ein benutzerdefinierter DNS-Server oder der von Azure bereitgestellte DNS-Server definiert. Der von Azure bereitgestellte DNS-Server kann Hostnamen für Ressourcen auflösen, die mit demselben VNet verbunden sind. Vollqualifizierte Domänennamen (FQDNs) müssen zum Auflösen für Ressourcen verwendet werden, die mit anderen VNets verbunden sind.
    - **Benutzerdefiniert:** Sie können einen eigenen DNS-Server für eine VNET-übergreifende Namensauflösung konfigurieren. Geben Sie die IP-Adresse des Servers ein, den Sie als DNS-Server verwenden möchten. Die von Ihnen angegebene DNS-Serveradresse wird nur dieser NIC zugewiesen und überschreibt alle DNS-Einstellungen für das VNET, mit dem die NIC verbunden ist.
6. Klicken Sie auf **Speichern**.

**Befehle**

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network nic update](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmNetworkInterface](/powershell/resourcemanager/azurerm.network/v3.4.0/set-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="ip-forwarding"></a>Aktivieren und Deaktivieren der IP-Weiterleitung

Eine IP-Weiterleitung ermöglicht dem virtuellen Computer, an den eine NIC angefügt ist, Folgendes:
- Empfangen von Netzwerkdatenverkehr, der nicht für eine der IP-Adressen bestimmt ist, die einer der IP-Konfigurationen für die NIC zugewiesen ist
- Senden von Netzwerkdatenverkehr mit einer IP-Quelladresse, die nicht der Adresse entspricht, die den IP-Konfigurationen einer NIC zugewiesen ist

Die Einstellung muss für jede an den virtuellen Computer angefügte NIC aktiviert werden, die Datenverkehr empfängt, den der virtuelle Computer weiterleiten muss. Ein virtueller Computer kann Datenverkehr unabhängig davon weiterleiten, ob er über mehrere angefügte NICs oder nur über eine einzelne angefügte NIC verfügt. Bei der IP-Weiterleitung handelt es sich zwar um eine Azure-Einstellung, auf dem virtuellen Computer muss aber auch eine Anwendung ausgeführt werden, die den Datenverkehr weiterleiten kann – beispielsweise eine Firewall, ein WAN-Optimierer oder ein Lastenausgleich. Wenn ein virtueller Computer Netzwerkanwendungen ausführt, wird er häufig als virtuelles Netzwerkgerät (Network Virtual Appliance, NVA) bezeichnet. Eine Liste mit bereitstellungsbereiten NVAs finden Sie im [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances). Die IP-Weiterleitung wird in der Regel mit benutzerdefinierten Routen verwendet. Weitere Informationen zu benutzerdefinierten Routen finden Sie im Artikel [Benutzerdefinierte Routen und IP-Weiterleitung](virtual-networks-udr-overview.md).

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind, beim [Azure-Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie im Artikel [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie im oberen Bereich des Azure-Portals im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Netzwerkschnittstellen* ein. Wenn **Netzwerkschnittstellen** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie auf dem daraufhin angezeigten Blatt **Netzwerkschnittstellen** auf die NIC, für die Sie die IP-Weiterleitung aktivieren oder deaktivieren möchten.
4. Klicken Sie auf dem Blatt der ausgewählten NIC im Abschnitt **EINSTELLUNGEN** auf **IP-Konfigurationen**.
5. Klicken Sie auf **Aktiviert** oder **Deaktiviert** (Standardeinstellung), um die Einstellung zu ändern.
6. Klicken Sie auf **Speichern**.

**Befehle**

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network nic update](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmNetworkInterface](/powershell/resourcemanager/azurerm.network/v3.4.0/set-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="subnet"></a>Ändern der Subnetzzuweisung

Im Gegensatz zum VNET können Sie das Subnetz ändern, mit dem eine NIC verbunden ist.

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind, beim [Azure-Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie im Artikel [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie im oberen Bereich des Azure-Portals im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Netzwerkschnittstellen* ein. Wenn **Netzwerkschnittstellen** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie auf dem daraufhin angezeigten Blatt **Netzwerkschnittstellen** auf die NIC, deren Einstellungen Sie anzeigen oder ändern möchten.
4. Klicken Sie auf dem Blatt für die ausgewählte NIC unter **EINSTELLUNGEN** auf **IP-Konfigurationen**. Falls neben privaten IP-Adressen für aufgeführte IP-Konfigurationen **(Statisch)** angegeben ist, müssen Sie die IP-Adresszuweisungsmethode in „Dynamisch“ ändern, indem Sie die folgenden Schritte ausführen. Alle privaten IP-Adressen müssen mit der dynamischen Zuweisungsmethode zugewiesen werden, um die Subnetzzuweisung für die NIC zu ändern. Falls die Adressen mithilfe der dynamischen Methode zugewiesen wurden, fahren Sie mit Schritt 5 fort. Falls Adressen mit der statischen Zuordnungsmethode zugewiesen wurden, führen Sie die folgenden Schritte aus, um die Zuweisungsmethode in „Dynamisch“ zu ändern:
    - Klicken Sie in der Liste mit den IP-Konfigurationen auf die IP-Konfiguration, deren IP-Adresszuweisungsmethode Sie ändern möchten.
    - Klicken Sie auf dem Blatt, das für die IP-Konfiguration angezeigt wird, unter der **Zuweisungsmethode**auf **Dynamisch**.
    - Klicken Sie auf **Speichern**.
5. Wählen Sie in der Dropdownliste **Subnetz** das Subnetz aus, mit dem Sie die NIC verbinden möchten.
6. Klicken Sie auf **Speichern**. Neue dynamische Adressen werden aus dem Subnetzadressbereich des neuen Subnetzes zugewiesen. Nach dem Zuweisen der NIC zu einem neuen Subnetz können Sie nach Wunsch eine statische IP-Adresse aus dem Adressbereich des neuen Subnetzes zuweisen. Weitere Informationen zum Hinzufügen, Ändern und Entfernen von IP-Adressen für eine NIC finden Sie im Artikel [Hinzufügen, Ändern oder Entfernen von IP-Adressen](virtual-network-network-interface-addresses.md).

**Befehle**

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network nic ip-config update](/cli/azure/network/nic/ip-config?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmNetworkInterfaceIpConfig](/powershell/resourcemanager/azurerm.network/v3.4.0/set-azurermnetworkinterfaceipconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## <a name="delete-nic"></a>Löschen einer NIC

Sie können eine NIC löschen, sofern sie an keinen virtuellen Computer angefügt ist. Sollte sie an einen virtuellen Computer angefügt sein, müssen Sie den virtuellen Computer zunächst in den Zustand „Beendet (Zuordnung aufgehoben)“ versetzen und dann die NIC vom virtuellen Computer trennen, ehe Sie die NIC löschen können. Die Schritte zum Trennen einer NIC von einem virtuellen Computer finden Sie im Abschnitt [Trennen einer NIC von einem virtuellen Computer](virtual-network-network-interface-vm.md#vm-remove-nic) im Artikel [Hinzufügen und Entfernen von Netzwerkschnittstellenkarten](virtual-network-network-interface-vm.md). Beim Löschen eines virtuellen Computers werden alle an ihn angefügten NICs getrennt, ohne dass diese gelöscht werden.

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind, beim [Azure-Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie im Artikel [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie im oberen Bereich des Azure-Portals im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Netzwerkschnittstellen* ein. Wenn **Netzwerkschnittstellen** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie mit der rechten Maustaste auf die NIC, die Sie löschen möchten, und klicken Sie anschließend auf **Löschen**.
4. Klicken Sie auf **Ja**, um das Löschen der NIC zu bestätigen.

Wenn Sie eine NIC löschen, werden alle ihr zugewiesenen MAC- und IP-Adressen freigegeben.

**Befehle**

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network nic delete](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Remove-AzureRmNetworkInterface](/powershell/resourcemanager/azurerm.network/v3.1.0/remove-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## <a name="next-steps"></a>Nächste Schritte
In den folgenden Artikeln erfahren Sie, wie Sie virtuelle Computer mit mehreren NICs oder IP-Adressen erstellen:

**Befehle**

|Task|Tool|
|---|---|
|Erstellen eines virtuellen Computers mit mehreren Netzwerkschnittstellenkarten (NICs)|[BEFEHLSZEILENSCHNITTSTELLE (CLI)](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
||[PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Erstellen eines virtuellen Computers mit einer NIC und mehreren IP-Adressen|[BEFEHLSZEILENSCHNITTSTELLE (CLI)](virtual-network-multiple-ip-addresses-cli.md)|
||[PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|

