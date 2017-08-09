---
title: "Erstellen, Ändern und Löschen von Azure-Netzwerkschnittstellen | Microsoft-Dokumentation"
description: "Erfahren Sie, was Netzwerkschnittstellen sind und wie Sie sie erstellen, ihre Einstellungen ändern und sie löschen."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/24/2017
ms.author: jdial
ms.translationtype: HT
ms.sourcegitcommit: 7bf5d568e59ead343ff2c976b310de79a998673b
ms.openlocfilehash: 37405fb9e8b85040fbbd1e91efd44a80c4f87ff0
ms.contentlocale: de-de
ms.lasthandoff: 08/01/2017

---

# <a name="create-change-or-delete-a-network-interface"></a>Erstellen, Ändern oder Löschen von Netzwerkschnittstellen

Erfahren Sie, wie Sie Netzwerkschnittstellenkarten erstellen, ihre Einstellungen ändern und sie löschen. Eine Netzwerkschnittstelle ermöglicht einem virtuellen Azure-Computer die Kommunikation mit dem Internet, Azure und lokalen Ressourcen. Wenn Sie im Azure-Portal eine VM erstellen, generiert das Portal für Sie eine Netzwerkschnittstelle mit Standardeinstellungen. Sie können stattdessen Netzwerkschnittstellen mit benutzerdefinierten Einstellungen erstellen und einem virtuellen Computer bei der Erstellung Netzwerkschnittstellen hinzufügen. Sie können auch die Standardeinstellungen einer vorhandenen Netzwerkschnittstelle ändern. In diesem Artikel wird erläutert, wie Sie Netzwerkschnittstellen mit benutzerdefinierten Einstellungen erstellen, vorhandene Netzwerkschnittstelleneinstellungen ändern, z.B. die Netzwerkfilterzuweisung (Netzwerksicherheitsgruppen), Subnetzzuweisung, DNS-Servereinstellungen und IP-Weiterleitung, und Netzwerkschnittstellen löschen.

Weitere Informationen zum Hinzufügen, Ändern oder Entfernen von IP-Adressen für eine Netzwerkschnittstelle finden Sie im Artikel [Verwalten von IP-Adressen](virtual-network-network-interface-addresses.md). Wenn Sie auf einem virtuellen Computer Netzwerkschnittstellen hinzufügen oder entfernen möchten, lesen Sie den Artikel [Hinzufügen oder Entfernen von Netzwerkschnittstellen](virtual-network-network-interface-vm.md).


## <a name="before-you-begin"></a>Voraussetzungen

Führen Sie zuerst die folgenden Aufgaben aus, ehe Sie die Schritte in den Abschnitten dieses Artikels durchführen:

- Lesen Sie den Artikel [Einschränkungen für Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits), um mehr über Einschränkungen von Netzwerkschnittstellen zu erfahren.
- Melden Sie sich mit einem Azure-Konto beim [Azure-Portal](https://portal.azure.com), der Azure-Befehlszeilenschnittstelle (CLI) oder bei Azure PowerShell an. Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für ein [kostenloses Testkonto](https://azure.microsoft.com/free) registrieren.
- Wenn Sie PowerShell-Befehle verwenden, um Aufgaben in diesem Artikel durchzuführen, [installieren und konfigurieren Sie Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Stellen Sie sicher, dass Sie die neueste Version der Azure PowerShell-Cmdlets installiert haben. Hilfe und Beispiele für PowerShell-Befehle erhalten Sie durch Eingabe von `get-help <command> -full`.
- Wenn Sie Befehle der Azure-Befehlszeilenschnittstelle (CLI) verwenden, um Aufgaben in diesem Artikel durchzuführen, [installieren und konfigurieren Sie Azure CLI](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Stellen Sie sicher, dass Sie die neueste Version der Azure CLI installiert haben. Hilfe zu den Befehlen der Befehlszeilenschnittstelle erhalten Sie durch Eingabe von `az <command> --help`. Anstatt die CLI und ihre Voraussetzungen zu installieren, können Sie die Azure Cloud Shell verwenden. Azure Cloud Shell ist eine kostenlose Bash-Shell, die Sie direkt im Azure-Portal ausführen können. Die Azure CLI ist vorinstalliert und für die Verwendung mit Ihrem Konto konfiguriert. Wenn Sie die Cloud Shell verwenden möchten, klicken Sie oben im [Portal](https://portal.azure.com) auf die Cloud Shell-Schaltfläche **>_**.

## <a name="create-a-network-interface"></a>Erstellen einer Netzwerkschnittstelle

Wenn Sie im Azure-Portal eine VM erstellen, generiert das Portal für Sie eine Netzwerkschnittstelle mit Standardeinstellungen. Wenn Sie lieber alle Netzwerkschnittstelleneinstellungen angeben möchten, können Sie eine Netzwerkschnittstelle mit benutzerdefinierten Einstellungen erstellen und beim Erstellen eines virtuellen Computers an diesen anfügen (mithilfe von PowerShell oder der Azure-Befehlszeilenschnittstelle). Sie können auch eine Netzwerkschnittstelle erstellen und an einen vorhandenen virtuellen Computer anfügen (mithilfe von PowerShell oder der Azure-Befehlszeilenschnittstelle). Weitere Informationen zum Erstellen eines virtuellen Computers mit einer vorhandenen Netzwerkschnittstelle oder zum Hinzufügen und Entfernen von Netzwerkschnittstellen bei vorhandenen virtuellen Computern finden Sie im Artikel [Hinzufügen oder Entfernen von Netzwerkschnittstellen](virtual-network-network-interface-vm.md). Vor dem Erstellen einer Netzwerkschnittstelle müssen Sie über ein [virtuelles Netzwerk](virtual-networks-create-vnet-arm-pportal.md) am selben Standort und im selben Abonnement verfügen, in dem Sie eine Netzwerkschnittstelle erstellen.

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind, beim [Azure-Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie im Artikel [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie im oberen Bereich des Azure-Portals im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Netzwerkschnittstellen* ein. Wenn **Netzwerkschnittstellen** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie auf dem daraufhin angezeigten Blatt **Netzwerkschnittstellen** auf **+ Hinzufügen**.
4. Das Blatt **Netzwerkschnittstelle erstellen** wird angezeigt. Geben Sie hier Werte für die folgenden Einstellungen an, und klicken Sie anschließend auf **Erstellen**:

    |Einstellung|Erforderlich|Details|
    |---|---|---|
    |Name|Ja|Der Name muss innerhalb der ausgewählten Ressourcengruppe eindeutig sein. Im Lauf der Zeit verfügen Sie wahrscheinlich über mehrere Netzwerkschnittstellen in Ihrem Azure-Abonnement. Lesen Sie den Artikel zu [Benennungskonventionen](/azure/architecture/best-practices/naming-conventions?toc=%2fazure%2fvirtual-network%2ftoc.json#naming-rules-and-restrictions) mit Vorschlägen zum Festlegen einer Benennungskonvention zum Vereinfachen der Verwaltung mehrerer Netzwerkschnittstellen. Nach dem Erstellen kann der Name der Netzwerkschnittstelle nicht mehr geändert werden.|
    |Virtuelles Netzwerk|Ja|Wählen Sie das virtuelle Netzwerk für die Netzwerkschnittstelle aus. Sie können eine Netzwerkschnittstelle nur einem virtuellen Netzwerk zuweisen, das dasselbe Abonnement und denselben Standort wie die Netzwerkschnittstelle hat. Nachdem eine Netzwerkschnittstelle erstellt wurde, können Sie das virtuelle Netzwerk, dem sie zugewiesen ist, nicht mehr ändern. Der virtuelle Computer, dem Sie die Netzwerkschnittstelle hinzufügen, muss auch denselben Standort und dasselbe Abonnement wie die Netzwerkschnittstelle aufweisen.|
    |Subnetz|Ja|Wählen Sie ein Subnetz innerhalb des ausgewählten virtuellen Netzwerks aus. Das Subnetz, dem die Netzwerkschnittstelle zugewiesen ist, kann nach der Erstellung geändert werden.|
    |Zuweisung der privaten IP-Adresse|Ja| Mit dieser Einstellung wählen Sie die Zuweisungsmethode für die IPv4-Adresse aus. Wählen Sie aus den folgenden Methoden für die Zuweisung: **Dynamisch:** Bei Wahl dieser Option weist Azure automatisch eine verfügbare Adresse aus dem Adressraum des Subnetzes zu, das Sie ausgewählt haben. Azure kann einer Netzwerkschnittstelle eine andere Adresse zuweisen, wenn der zugehörige virtuelle Computer gestartet wird, nachdem er sich zuvor im Zustand „Beendet (Zuordnung aufgehoben)“ befand. Die Adresse bleibt unverändert, wenn der virtuelle Computer neu gestartet wird, ohne sich zuvor im Zustand „Beendet (Zuordnung aufgehoben)“ befunden zu haben. **Statisch:** Bei Wahl dieser Option müssen Sie eine verfügbare IP-Adresse innerhalb des Adressraums des ausgewählten Subnetzes manuell zuweisen. Statische Adressen bleiben unverändert, bis Sie sie ändern oder die Netzwerkschnittstelle gelöscht wird. Die Zuweisungsmethode kann nach Erstellung der Netzwerkschnittstelle geändert werden. Der DHCP-Server von Azure weist diese Adresse der Netzwerkschnittstelle innerhalb des Betriebssystems des virtuellen Computers zu.|
    |Netzwerksicherheitsgruppe|Nein| Belassen Sie diese Einstellung auf **Keine** festgelegt, wählen Sie eine vorhandene [Netzwerksicherheitsgruppe](virtual-networks-nsg.md) aus, oder [erstellen Sie eine Netzwerksicherheitsgruppe](virtual-networks-create-nsg-arm-pportal.md). Mithilfe von Netzwerksicherheitsgruppen können Sie den ein- und ausgehenden Netzwerkdatenverkehr für eine Netzwerkschnittstelle steuern. Auf eine Netzwerkschnittstelle kann maximal eine Netzwerksicherheitsgruppe angewendet werden. Auch auf das Subnetz, dem die Netzwerkschnittstelle zugewiesen ist, kann maximal eine Netzwerksicherheitsgruppe angewendet werden. Wenn eine Netzwerksicherheitsgruppe auf eine Netzwerkschnittstelle und auf das Subnetz, dem die Netzwerkschnittstelle zugewiesen ist, angewendet wird, kommt es in einigen Fällen zu unerwarteten Ergebnissen. Informationen zum Beheben von Problemen mit Netzwerksicherheitsgruppen, die auf Netzwerkschnittstellen und Subnetze angewendet werden, finden Sie im Artikel [Beheben von Problemen bei Netzwerksicherheitsgruppen](virtual-network-nsg-troubleshoot-portal.md#nsg).|
    |Abonnement|Ja|Wählen Sie eines Ihrer Azure-[Abonnements](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) aus. Der virtuelle Computer, an den Sie eine Netzwerkschnittstelle anfügen, und das virtuelle Netzwerk, mit dem Sie eine Verbindung herstellen, müssen zu demselben Abonnement gehören.|
    |Private IP-Adresse (IPv6)|Nein| Wenn Sie dieses Kontrollkästchen aktivieren, wird der Netzwerkschnittstelle zusätzlich zu der IPv4-Adresse der Netzwerkschnittstelle eine IPv6-Adresse zugewiesen. Im Abschnitt [IPv6](#IPv6) dieses Artikels finden Sie wichtige Informationen zur Verwendung von IPv6 bei Netzwerkschnittstellen. Eine Auswahl der Zuweisungsmethode für die IPv6-Adresse ist nicht möglich. Wenn Sie eine IPv6-Adresse zuweisen, erfolgt die Zuordnung mit der dynamischen Methode.
    |IPv6-Name (wird nur angezeigt, wenn das Kontrollkästchen **Private IP-Adresse (IPv6)** aktiviert ist) |Ja, wenn das Kontrollkästchen **Private IP-Adresse (IPv6)** aktiviert ist.| Dieser Name wird einer sekundären IP-Konfiguration für die Netzwerkschnittstelle zugewiesen. Weitere Informationen zu IP-Konfigurationen finden Sie im Abschnitt [Anzeigen der Einstellungen von Netzwerkschnittstellen](#view-network-interface-settings) in diesem Artikel.|
    |Ressourcengruppe|Ja|Wählen Sie eine vorhandene [Ressourcengruppe](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) aus, oder erstellen Sie eine. Eine Netzwerkschnittstelle kann sich in der gleichen (oder in einer anderen) Ressourcengruppe befinden wie der virtuelle Computer, an den Sie sie anfügen, oder wie das virtuelle Netzwerk, mit dem Sie eine Verbindung herstellen.|
    |Standort|Ja|Der virtuelle Computer, an den Sie eine Netzwerkschnittstelle anfügen, und das virtuelle Netzwerk, mit dem Sie eine Verbindung herstellen, müssen sich an demselben [Standort](https://azure.microsoft.com/regions) befinden. Dieser wird auch als Region bezeichnet.|

Das Portal stellt keine Option bereit, mit der Sie der Netzwerkschnittstelle beim Erstellen eine öffentliche IP-Adresse zuweisen können. Sie können allerdings im Portal eine öffentliche IP-Adresse erstellen und diese einer Netzwerkschnittstelle zuweisen, wenn Sie im Portal einen virtuellen Computer erstellen. Informationen zum Hinzufügen einer öffentlichen IP-Adresse zur Netzwerkschnittstelle nach deren Erstellung finden Sie im Artikel zum [Verwalten von IP-Adressen](virtual-network-network-interface-addresses.md). Wenn Sie eine Netzwerkschnittstelle mit einer öffentlichen IP-Adresse erstellen möchten, müssen Sie zum Erstellen der Netzwerkschnittstelle die Befehlszeilenschnittstelle oder PowerShell verwenden.

>[!Note]
> Azure weist der Netzwerkschnittstelle erst dann eine MAC-Adresse zu, wenn die Netzwerkschnittstelle an einen virtuellen Computer angefügt und der virtuelle Computer erstmalig gestartet wurde. Die MAC-Adresse, die Azure der Netzwerkschnittstelle zuweist, kann nicht angegeben werden. Die MAC-Adresse bleibt der Netzwerkschnittstelle zugewiesen, bis die Netzwerkschnittstelle gelöscht oder die private IP-Adresse, die der primären IP-Konfiguration der primären Netzwerkschnittstelle zugewiesen ist, geändert wird. Weitere Informationen zu IP-Adressen und IP-Konfigurationen finden Sie im Artikel [Verwalten von IP-Adressen](virtual-network-network-interface-addresses.md).

**Befehle**

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network nic create](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|

## <a name="view-network-interface-settings"></a>Anzeigen der Einstellungen von Netzwerkschnittstellen

Sie können die meisten Einstellungen für eine Netzwerkschnittstelle anzeigen und ändern, nachdem diese erstellt wurde.

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind, beim [Azure-Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie im Artikel [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie im oberen Bereich des Azure-Portals im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Netzwerkschnittstellen* ein. Wenn **Netzwerkschnittstellen** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie auf dem angezeigten Blatt **Netzwerkschnittstellen** auf die Netzwerkschnittstelle, deren Einstellungen Sie anzeigen oder ändern möchten.
4. Auf dem Blatt, das für die ausgewählte Netzwerkschnittstelle angezeigt wird, stehen folgende Einstellungen zur Verfügung:
    - **Übersicht:** enthält Informationen über die Netzwerkschnittstelle, z.B. die zugewiesenen IP-Adressen, das virtuelle Netzwerk/Subnetz, dem die Netzwerkschnittstelle zugewiesen ist, und der virtuelle Computer, an den die Netzwerkschnittstelle angefügt ist (sofern zutreffend). Die folgende Abbildung zeigt die Übersicht über die Einstellungen für eine Netzwerkschnittstelle mit dem Namen **mywebserver256**: ![Übersicht über die Netzwerkschnittstelle](./media/virtual-network-network-interface/nic-overview.png) Sie können eine Netzwerkschnittstelle in eine andere Ressourcengruppe oder ein anderes Abonnement verschieben, indem Sie neben der **Ressourcengruppe** oder dem **Abonnementnamen** auf (**ändern**) klicken. Wenn Sie die Netzwerkschnittstelle verschieben, müssen Sie alle Ressourcen für die Netzwerkschnittstelle ebenfalls verschieben. Wenn die Netzwerkschnittstelle an einen virtuellen Computer angefügt ist, müssen Sie z.B. auch den virtuellen Computer und andere mit dem virtuellen Computer verknüpfte Ressourcen verschieben. Informationen zum Verschieben von Netzwerkschnittstellen finden Sie im Artikel [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../azure-resource-manager/resource-group-move-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json#use-portal). Hier erfahren Sie, welche Voraussetzungen erfüllt sein müssen und wie Sie Ressourcen über das Azure-Portal, mithilfe von PowerShell oder unter Verwendung der Azure CLI verschieben.
    - **IP-Konfigurationen:** Hier werden die öffentlichen und privaten IPv4- und IPv6-Adressen aufgeführt, die IP-Konfigurationen zugewiesen sind. Wenn einer IP-Konfiguration eine IPv6-Adresse zugewiesen ist, wird die Adresse nicht angezeigt. Weitere Informationen zu IP-Konfigurationen und zum Hinzufügen und Entfernen von IP-Adressen finden Sie im Artikel [Konfigurieren von IP-Adressen für eine Azure-Netzwerkschnittstelle](virtual-network-network-interface-addresses.md). Die IP-Weiterleitung und Subnetzzuweisung werden auch in diesem Abschnitt konfiguriert. Weitere Informationen zu diesen Einstellungen finden Sie im Abschnitt [Aktivieren oder Deaktivieren der IP-Weiterleitung](#enable-or-disable-ip-forwarding) und [Ändern der Subnetzzuweisung](#change-subnet-assignment) in diesem Artikel.
    - **DNS-Server:** Sie können angeben, welcher DNS-Server einer Netzwerkschnittstelle durch die Azure-DHCP-Server zugewiesen wird. Die Netzwerkschnittstelle kann die Einstellung vom virtuellen Netzwerk, dem die Netzwerkschnittstelle zugewiesen ist, erben. Sie können aber auch benutzerdefinierte Einstellungen festlegen, die die entsprechenden Einstellungen für das virtuelle Netzwerk, dem sie zugewiesen ist, überschreiben. Führen Sie zum Ändern der Anzeige die Schritte im Abschnitt [Ändern von DNS-Servern](#change-dns-servers) dieses Artikels aus.
    - **Netzwerksicherheitsgruppe (NSG):** zeigt an, welche Netzwerksicherheitsgruppe (NSG) der Netzwerkschnittstelle zugeordnet ist (falls vorhanden). Eine NSG enthält ein- und ausgehende Regeln zum Filtern von Netzwerkdatenverkehr für die Netzwerkschnittstelle. Falls der Netzwerkschnittstelle eine NSG zugeordnet ist, wird der Name der zugeordneten NSG angezeigt. Zum Ändern der angezeigten Informationen führen Sie die Schritte im Artikel [Verwalten der Zuordnungen von Netzwerksicherheitsgruppen](virtual-network-manage-nsg-arm-portal.md#manage-associations) durch.
    - **Eigenschaften:** zeigt wichtige Einstellungen für die Netzwerkschnittstelle an, z.B. die MAC-Adresse (leer, wenn die Netzwerkschnittstelle an keine VM angefügt ist) und das Abonnement, dem sie angehört.
    - **Effektive Sicherheitsregeln:** Sicherheitsregeln werden aufgelistet, wenn die Netzwerkschnittstelle an einen aktiven virtuellen Computer angefügt und der Netzwerkschnittstelle oder dem Subnetz, mit dem sie verbunden ist, eine NSG zugeordnet ist. Weitere Informationen zu den angezeigten Informationen finden Sie im Artikel [Problembehandlung bei Netzwerksicherheitsgruppen über das Azure-Portal](virtual-network-nsg-troubleshoot-portal.md#nsg). Weitere Informationen zu NSGs finden Sie im Artikel [Steuern des Netzwerkdatenverkehrs mit Netzwerksicherheitsgruppen](virtual-networks-nsg.md).
    - **Effektive Routen:** Routen werden aufgelistet, wenn die Netzwerkschnittstelle an einen aktiven virtuellen Computer angefügt ist. Bei den Routen handelt es sich um eine Kombination aus den Azure-Standardrouten, benutzerdefinierten Routen und BGP-Routen, die ggf. für das Subnetz vorhanden sind, dem die Netzwerkschnittstelle zugewiesen ist. Weitere Informationen zur Anzeige finden Sie im Artikel [Problembehandlung bei Routen über das Azure-Portal](virtual-network-routes-troubleshoot-portal.md#view-effective-routes-for-a-network-interface). Weitere Informationen zu Azure-Standardeinstellungen und benutzerdefinierten Routen finden Sie im Artikel [Benutzerdefinierte Routen und IP-Weiterleitung](virtual-networks-udr-overview.md).
    - **Allgemeine Azure Resource Manager-Einstellungen:** Weitere Informationen zu allgemeinen Azure Resource Manager-Einstellungen finden Sie unter [Aktivitätsprotokolle](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#activity-logs), [Zugriffssteuerung](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#access-control), [Tags](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#tags), [Sperren von Ressourcen, um unerwartete Änderungen zu verhindern](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json) und [Exportieren der Vorlage aus der Ressourcengruppe](../azure-resource-manager/resource-manager-export-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json#export-the-template-from-resource-group).

**Befehle**

Wenn eine IPv6-Adresse einer Netzwerkschnittstelle zugewiesen ist, gibt die PowerShell-Ausgabe zurück, dass die Adresse zugewiesen ist, aber nicht die zugewiesene Adresse selbst. Auf ähnliche Weise gibt die Befehlszeilenschnittstelle zurück, dass die Adresse zugewiesen ist, als Ausgabe für die Adresse jedoch lediglich *NULL*.

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network nic list](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#list): Anzeigen von im Abonnement enthaltenen Netzwerkschnittstellen; [az network nic show](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#show): Anzeigen der Einstellungen für eine Netzwerkschnittstelle|
|PowerShell|[Get-AzureRmNetworkInterface](/powershell/resourcemanager/azurerm.network/get-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json): Anzeigen von im Abonnement enthaltenen Netzwerkschnittstellen oder der Einstellungen für eine Netzwerkschnittstelle|

## <a name="change-dns-servers"></a>Ändern von DNS-Servern

Der DHCP-Server von Azure weist den DNS-Server der Netzwerkschnittstelle innerhalb des Betriebssystems des virtuellen Computers zu. Der zugewiesene DNS-Server entspricht der geltenden DNS-Servereinstellung für eine Netzwerkschnittstelle. Weitere Informationen zu Namensauflösungseinstellungen für eine Netzwerkschnittstelle finden Sie im Artikel [Namensauflösung für virtuelle Computer](virtual-networks-name-resolution-for-vms-and-role-instances.md). Die Netzwerkschnittstelle kann die Einstellungen des virtuellen Netzwerks übernehmen oder eigene eindeutige Einstellungen verwenden, die die Einstellung für das virtuelle Netzwerk überschreiben.

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind, beim [Azure-Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie im Artikel [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie im oberen Bereich des Azure-Portals im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Netzwerkschnittstellen* ein. Wenn **Netzwerkschnittstellen** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie auf dem angezeigten Blatt **Netzwerkschnittstellen** auf die Netzwerkschnittstelle, deren Einstellungen Sie anzeigen oder ändern möchten.
4. Klicken Sie auf dem Blatt für die ausgewählte Netzwerkschnittstelle unter **EINSTELLUNGEN** auf **DNS-Server**.
5. Klicken Sie anschließend auf eine der folgenden Optionen:
    - **Von virtuellem Netzwerk erben** (Standardeinstellung): Wählen Sie diese Option aus, wenn die DNS-Servereinstellung des virtuellen Netzwerks übernommen werden soll, dem die Netzwerkschnittstelle zugewiesen ist. Auf Ebene des virtuellen Netzwerks ist entweder ein benutzerdefinierter DNS-Server oder der von Azure bereitgestellte DNS-Server definiert. Der von Azure bereitgestellte DNS-Server kann Hostnamen für Ressourcen auflösen, die demselben virtuellen Netzwerk zugewiesen sind. Zum Auflösen von Ressourcen, die anderen virtuellen Netzwerken zugewiesen sind, müssen vollqualifizierte Domänennamen (FQDNs) verwendet werden.
    - **Benutzerdefiniert:** Sie können einen eigenen DNS-Server für eine die virtuellen Netzwerke übergreifende Namensauflösung konfigurieren. Geben Sie die IP-Adresse des Servers ein, den Sie als DNS-Server verwenden möchten. Die von Ihnen angegebene DNS-Serveradresse wird nur dieser Netzwerkschnittstelle zugewiesen. Sie überschreibt alle DNS-Einstellungen für das virtuelle Netzwerk, dem die Netzwerkschnittstelle zugewiesen ist.
6. Klicken Sie auf **Speichern**.

**Befehle**

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network nic update](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmNetworkInterface](/powershell/resourcemanager/azurerm.network/set-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="enable-or-disable-ip-forwarding"></a>Aktivieren oder Deaktivieren der IP-Weiterleitung

Die IP-Weiterleitung ermöglicht virtuellen Computern mit einer zugewiesenen Netzwerkschnittstelle Folgendes:
- Empfangen von Netzwerkdatenverkehr, der nicht für eine der IP-Adressen bestimmt ist, die einer der IP-Konfigurationen für die Netzwerkschnittstelle zugewiesen ist
- Senden von Netzwerkdatenverkehr mit einer IP-Quelladresse, die nicht der Adresse entspricht, die den IP-Konfigurationen einer Netzwerkschnittstelle zugewiesen ist

Die Einstellung muss für jede Netzwerkschnittstelle aktiviert werden, die an den virtuellen Computer angefügt ist, der Datenverkehr empfängt und weiterleiten muss. Ein virtueller Computer kann Datenverkehr unabhängig davon weiterleiten, ob er über mehrere Netzwerkschnittstellen verfügt oder nur eine einzelne Netzwerkschnittstelle angefügt ist. Bei der IP-Weiterleitung handelt es sich zwar um eine Azure-Einstellung, auf dem virtuellen Computer muss aber auch eine Anwendung ausgeführt werden, die den Datenverkehr weiterleiten kann – beispielsweise eine Firewall, ein WAN-Optimierer oder ein Lastenausgleich. Wenn ein virtueller Computer Netzwerkanwendungen ausführt, wird er häufig als virtuelles Netzwerkgerät (Network Virtual Appliance) bezeichnet. Eine Liste mit für die Bereitstellung vorbereiteten virtuellen Netzwerkgeräten finden Sie im [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances). Die IP-Weiterleitung wird in der Regel mit benutzerdefinierten Routen verwendet. Weitere Informationen zu benutzerdefinierten Routen finden Sie im Artikel [Benutzerdefinierte Routen und IP-Weiterleitung](virtual-networks-udr-overview.md).

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind, beim [Azure-Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie im Artikel [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie im oberen Bereich des Azure-Portals im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Netzwerkschnittstellen* ein. Wenn **Netzwerkschnittstellen** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie auf dem daraufhin angezeigten Blatt **Netzwerkschnittstellen** auf die Netzwerkschnittstelle, für die Sie die IP-Weiterleitung aktivieren oder deaktivieren möchten.
4. Klicken Sie auf dem Blatt der ausgewählten Netzwerkschnittstelle im Abschnitt **EINSTELLUNGEN** auf **IP-Konfigurationen**.
5. Klicken Sie auf **Aktiviert** oder **Deaktiviert** (Standardeinstellung), um die Einstellung zu ändern.
6. Klicken Sie auf **Speichern**.

**Befehle**

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network nic update](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmNetworkInterface](/powershell/resourcemanager/azurerm.network/set-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="change-subnet-assignment"></a>Ändern der Subnetzzuweisung

Sie können das Subnetz, aber nicht das virtuelle Netzwerk ändern, dem eine Netzwerkschnittstelle zugewiesen ist.

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind, beim [Azure-Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie im Artikel [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie im oberen Bereich des Azure-Portals im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Netzwerkschnittstellen* ein. Wenn **Netzwerkschnittstellen** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie auf dem angezeigten Blatt **Netzwerkschnittstellen** auf die Netzwerkschnittstelle, deren Einstellungen Sie anzeigen oder ändern möchten.
4. Klicken Sie auf dem Blatt für die ausgewählte Netzwerkschnittstelle unter **EINSTELLUNGEN** auf **IP-Konfigurationen**. Falls neben privaten IP-Adressen für aufgeführte IP-Konfigurationen **(Statisch)** angegeben ist, müssen Sie die IP-Adresszuweisungsmethode in „Dynamisch“ ändern, indem Sie die folgenden Schritte ausführen. Alle privaten IP-Adressen müssen mit der dynamischen Zuweisungsmethode zugewiesen werden, um die Subnetzzuweisung für die Netzwerkschnittstelle zu ändern. Falls die Adressen mithilfe der dynamischen Methode zugewiesen wurden, fahren Sie mit Schritt 5 fort. Falls IPv4-Adressen mit der statischen Zuweisungsmethode zugewiesen wurden, führen Sie die folgenden Schritte aus, um die Zuweisungsmethode in „Dynamisch“ zu ändern:
    - Klicken Sie in der Liste mit den IP-Konfigurationen auf die IP-Konfiguration, deren IPv4-Adresszuweisungsmethode Sie ändern möchten.
    - Klicken Sie auf dem Blatt, das für die IP-Konfiguration angezeigt wird, unter der **Zuweisungsmethode**auf **Dynamisch**. Sie können IPv6-Adressen nicht mit der statischen Zuweisungsmethode zuweisen.
    - Klicken Sie auf **Speichern**.
5. Wählen Sie in der Dropdownliste **Subnetz** das Subnetz aus, mit dem Sie die Netzwerkschnittstelle verbinden möchten.
6. Klicken Sie auf **Speichern**. Neue dynamische Adressen werden aus dem Subnetzadressbereich des neuen Subnetzes zugewiesen. Nach dem Zuweisen der Netzwerkschnittstelle zu einem neuen Subnetz können Sie nach Wunsch eine statische IPv4-Adresse aus dem Adressbereich des neuen Subnetzes zuweisen. Weitere Informationen zum Hinzufügen, Ändern und Entfernen von IP-Adressen für eine Netzwerkschnittstelle finden Sie im Artikel [Verwalten von IP-Adressen](virtual-network-network-interface-addresses.md).

**Befehle**

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network nic ip-config update](/cli/azure/network/nic/ip-config?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmNetworkInterfaceIpConfig](/powershell/resourcemanager/azurerm.network/set-azurermnetworkinterfaceipconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## <a name="delete-a-network-interface"></a>Löschen einer Netzwerkschnittstelle

Sie können eine Netzwerkschnittstelle löschen, solange diese nicht an einen virtuellen Computer angefügt ist. Wenn sie an einen virtuellen Computer angefügt ist, müssen Sie den virtuellen Computer zunächst in den Status „Beendet (Zuordnung aufgehoben)“ versetzen und dann die Netzwerkschnittstelle vom virtuellen Computer trennen, bevor Sie die Netzwerkschnittstelle löschen können. Die Schritte zum Trennen einer Netzwerkschnittstelle von einem virtuellen Computer finden Sie im Abschnitt [Trennen einer Netzwerkschnittstelle von einem virtuellen Computer](virtual-network-network-interface-vm.md#vm-remove-nic) im Artikel [Hinzufügen oder Entfernen von Netzwerkschnittstellen](virtual-network-network-interface-vm.md). Beim Löschen eines virtuellen Computers werden alle angefügten Netzwerkschnittstellen getrennt, die Netzwerkschnittstellen werden aber nicht gelöscht.

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind, beim [Azure-Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie im Artikel [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie im oberen Bereich des Azure-Portals im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Netzwerkschnittstellen* ein. Wenn **Netzwerkschnittstellen** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie mit der rechten Maustaste auf die Netzwerkschnittstelle, die Sie löschen möchten, und klicken Sie anschließend auf **Löschen**.
4. Klicken Sie auf **Ja**, um das Löschen der Netzwerkschnittstelle zu bestätigen.

Wenn Sie eine Netzwerkschnittstelle löschen, werden alle ihr zugewiesenen MAC- und IP-Adressen freigegeben.

**Befehle**

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network nic delete](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Remove-AzureRmNetworkInterface](/powershell/resourcemanager/azurerm.network/remove-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="next-steps"></a>Nächste Schritte
In den folgenden Artikeln erfahren Sie, wie Sie virtuelle Computer mit mehreren Netzwerkschnittstellen oder IP-Adressen erstellen:

**Befehle**

|Task|Tool|
|---|---|
|Erstellen eines virtuellen Computers mit mehreren Netzwerkschnittstellenkarten (NICs)|[CLI](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Erstellen eines virtuellen Computers mit einer NIC und mehreren IPv4-Adressen|[CLI](virtual-network-multiple-ip-addresses-cli.md), [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Erstellen eines virtuellen Computers mit einer NIC und einer IPv6-Adresse (mit Azure Load Balancer)|[CLI](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [Azure Resource Manager-Vorlage](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|

