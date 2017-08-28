---
title: "Konfigurieren von IP-Adressen für Azure-Netzwerkschnittstellen | Microsoft-Dokumentation"
description: "Erfahren Sie etwas über das Hinzufügen, Ändern und Entfernen öffentlicher und privater IP-Adressen für Netzwerkschnittstellen."
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
ms.sourcegitcommit: b309108b4edaf5d1b198393aa44f55fc6aca231e
ms.openlocfilehash: 17ddb30c87d757176ce9428264135252c02bf713
ms.contentlocale: de-de
ms.lasthandoff: 08/15/2017

---

# <a name="add-change-or-remove-ip-addresses-for-an-azure-network-interface"></a>Hinzufügen, Ändern oder Entfernen von IP-Adressen für Azure-Netzwerkschnittstellen

Erfahren Sie etwas über das Hinzufügen, Ändern und Entfernen öffentlicher und privater IP-Adressen für Netzwerkschnittstellen. Die einer Netzwerkschnittstelle zugewiesenen privaten IP-Adressen ermöglichen dem virtuellen Computer die Kommunikation mit anderen Ressourcen im virtuellen Azure-Netzwerk und verbundenen Netzwerken. Eine private IP-Adresse ermöglicht ebenfalls die ausgehende Kommunikation mit dem Internet über eine nicht vorhersagbare IP-Adresse. Das Zuweisen einer [öffentlichen IP-Adresse](virtual-network-public-ip-address.md) zu einer Netzwerkschnittstelle ermöglicht eingehende Kommunikation an einen virtuellen Computer aus dem Internet. Die Adresse ermöglicht außerdem die ausgehende Kommunikation vom virtuellen Computer mit dem Internet über eine vorhersagbare IP-Adresse. Weitere Informationen finden Sie unter [Grundlegendes zu ausgehenden Verbindungen in Azure](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 

Informationen zum Erstellen, Ändern oder Löschen von Netzwerkschnittstellen finden Sie im Artikel [Verwalten von Netzwerkschnittstellen](virtual-network-network-interface.md). Wenn Sie auf einem virtuellen Computer Netzwerkschnittstellen hinzufügen oder entfernen möchten, lesen Sie den Artikel [Hinzufügen oder Entfernen von Netzwerkschnittstellen](virtual-network-network-interface-vm.md). 


## <a name="before-you-begin"></a>Voraussetzungen

Führen Sie zuerst die folgenden Aufgaben aus, ehe Sie die Schritte in den Abschnitten dieses Artikels durchführen:

- Lesen Sie den Artikel [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits), um mehr über Einschränkungen für öffentliche und private IP-Adressen zu erfahren.
- Melden Sie sich mit einem Azure-Konto beim [Azure-Portal](https://portal.azure.com), der Azure-Befehlszeilenschnittstelle (CLI) oder bei Azure PowerShell an. Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für ein [kostenloses Testkonto](https://azure.microsoft.com/free) registrieren.
- Wenn Sie PowerShell-Befehle verwenden, um Aufgaben in diesem Artikel durchzuführen, [installieren und konfigurieren Sie Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Stellen Sie sicher, dass Sie die neueste Version der Azure PowerShell-Cmdlets installiert haben. Hilfe und Beispiele für PowerShell-Befehle erhalten Sie durch Eingabe von `get-help <command> -full`.
- Wenn Sie Befehle der Azure-Befehlszeilenschnittstelle (CLI) verwenden, um Aufgaben in diesem Artikel durchzuführen, [installieren und konfigurieren Sie Azure CLI](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Stellen Sie sicher, dass Sie die neueste Version der Azure CLI installiert haben. Hilfe zu den Befehlen der Befehlszeilenschnittstelle erhalten Sie durch Eingabe von `az <command> --help`. Anstatt die CLI und ihre Voraussetzungen zu installieren, können Sie die Azure Cloud Shell verwenden. Azure Cloud Shell ist eine kostenlose Bash-Shell, die Sie direkt im Azure-Portal ausführen können. Die Azure CLI ist vorinstalliert und für die Verwendung mit Ihrem Konto konfiguriert. Wenn Sie die Cloud Shell verwenden möchten, klicken Sie oben im [Portal](https://portal.azure.com) auf die Cloud Shell-Schaltfläche **>_**.

## <a name="add-ip-addresses"></a>Hinzufügen von IP-Adressen

Einer Netzwerkschnittstelle können beliebig viele [private](#private) und [öffentliche](#public) [IPv4](#ipv4)-Adressen hinzugefügt werden (im Rahmen der im Artikel [Einschränkungen bei Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) aufgeführten Grenzwerte). Sie können einer vorhandenen Netzwerkschnittstelle nicht über das Portal eine IPv6-Adresse hinzufügen. (Sie können im Portal jedoch einer Netzwerkschnittstelle bei ihrer Erstellung eine private IPv6-Adresse hinzufügen.) Sie können PowerShell oder die Befehlszeilenschnittstelle verwenden, um einer [sekundären IP-Konfiguration](#secondary) eine private IPv6-Adresse für eine vorhandene Netzwerkschnittstelle, die nicht einem virtuellen Computer zugeordnet ist, hinzuzufügen (solange noch keine sekundären IP-Konfigurationen vorhanden sind). Sie können nicht jedes Tool verwenden, um einer Netzwerkschnittstelle eine öffentliche IPv6-Adresse hinzuzufügen. Ausführliche Informationen zur Verwendung von IPv6-Adressen finden Sie unter [IPv6](#ipv6). 

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind, beim [Azure-Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie im Artikel [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie im oberen Bereich des Azure-Portals im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Netzwerkschnittstellen* ein. Wenn **Netzwerkschnittstellen** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie auf dem daraufhin angezeigten Blatt **Netzwerkschnittstellen** auf die Netzwerkschnittstelle, der Sie eine IPv4-Adresse hinzufügen möchten.
4. Klicken Sie auf dem Blatt der ausgewählten Netzwerkschnittstelle im Abschnitt **EINSTELLUNGEN** auf **IP-Konfigurationen**.
5. Klicken Sie auf dem Blatt, das für IP-Konfigurationen geöffnet wird, auf **+ Hinzufügen**.
6. Geben Sie Folgendes an, und klicken Sie anschließend auf **OK**, um das Blatt **IP-Konfiguration hinzufügen** zu schließen:

    |Einstellung|Erforderlich|Details|
    |---|---|---|
    |Name|Ja|Muss für die Netzwerkschnittstelle eindeutig sein|
    |Typ|Ja|Da Sie die IP-Konfiguration einer vorhandenen Netzwerkschnittstelle hinzufügen und jede Netzwerkschnittstelle über eine [primäre](#primary) IP-Konfiguration verfügen muss, steht nur die Option **Sekundär** zur Verfügung.|
    |Zuweisungsmethode für private IP-Adressen|Ja|Adressen vom Typ [**Dynamisch**](#dynamic) können sich ändern, wenn der virtuelle Computer neu gestartet wird, nachdem er sich zuvor im Zustand „Beendet (Zuordnung aufgehoben)“ befand. Azure weist eine verfügbare Adresse aus dem Adressraum des Subnetzes zu, mit dem die Netzwerkschnittstelle verbunden ist. Adressen vom Typ [**Statisch**](#static) werden erst freigegeben, wenn die Netzwerkschnittstelle gelöscht wird. Geben Sie eine IP-Adresse aus dem Subnetzadressbereich an, die derzeit nicht von einer anderen IP-Konfiguration verwendet wird.|
    |Öffentliche IP-Adresse|Nein|**Deaktiviert:** Der IP-Konfiguration ist derzeit keine öffentliche IP-Adressressource zugeordnet. **Aktiviert:** Wählen Sie eine vorhandene öffentliche IPv4-Adresse aus, oder erstellen Sie eine neue. Eine Anleitung zur Erstellung einer öffentlichen IP-Adresse finden Sie im Artikel [Public IP addresses](virtual-network-public-ip-address.md#create-a-public-ip-address) (Öffentliche IP-Adressen).|
7. Fügen Sie dem Betriebssystem des virtuellen Computers manuell sekundäre private IP-Adressen hinzu. Gehen Sie dabei wie im Artikel [Zuweisen von mehreren IP-Adressen zu Betriebssystemen virtueller Computer](virtual-network-multiple-ip-addresses-portal.md#os-config) beschrieben vor. Spezielle Überlegungen vor dem manuellen Hinzufügen von IP-Adressen zum Betriebssystem virtueller Computer finden Sie unter [private](#private) IP-Adressen. Fügen Sie dem Betriebssystem des virtuellen Computers keine öffentlichen IP-Adressen hinzu.

**Befehle**

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network nic ip-config create](/cli/azure/network/nic/ip-config?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Add-AzureRmNetworkInterfaceIpConfig](/powershell/module/azurerm.network/add-azurermnetworkinterfaceipconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="change-ip-address-settings"></a>Ändern von IP-Adresseinstellungen

Sie müssen ggf. die Zuweisungsmethode einer IPv4-Adresse, die statische IPv4-Adresse oder die einer Netzwerkschnittstelle zugewiesene öffentliche IP-Adresse ändern. Wenn Sie die private IPv4-Adresse einer sekundären IP-Konfiguration ändern, die einer sekundären Netzwerkschnittstelle eines virtuellen Computers zugeordnet ist (siehe [Primäre und sekundäre Netzwerkschnittstellen](virtual-network-network-interface-vm.md#about)), ändern Sie den Status des virtuellen Computers in „Beendet (Zuordnung aufgehoben)“, bevor Sie die folgenden Schritte ausführen: 

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind, beim [Azure-Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie im Artikel [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie im oberen Bereich des Azure-Portals im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Netzwerkschnittstellen* ein. Wenn **Netzwerkschnittstellen** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie auf dem angezeigten Blatt **Netzwerkschnittstellen** auf die Netzwerkschnittstelle, deren IP-Adresseinstellungen Sie anzeigen oder ändern möchten.
4. Klicken Sie auf dem Blatt der ausgewählten Netzwerkschnittstelle im Abschnitt **EINSTELLUNGEN** auf **IP-Konfigurationen**.
5. Klicken Sie auf dem Blatt, das für IP-Konfigurationen geöffnet wird, in der Liste auf die IP-Konfiguration, die Sie ändern möchten.
6. Ändern Sie die Einstellungen nach Wunsch. Informationen zu den Einstellungen finden Sie in diesem Artikel im Abschnitt [Hinzufügen einer IP-Konfiguration](#create-ip-config) in Schritt 6. Klicken Sie anschließend auf **Speichern**, um das Blatt für die ausgewählte IP-Konfiguration zu schließen.

>[!NOTE]
>Falls die primäre Netzwerkschnittstelle über mehrere IP-Konfigurationen verfügt und Sie die private IP-Adresse der primären IP-Konfiguration ändern, müssen Sie unter Windows die primäre und alle sekundären IP-Adressen manuell wieder der Netzwerkschnittstelle zuweisen. (Unter Linux ist dies nicht erforderlich.) Wie Sie IP-Adressen im Rahmen eines Betriebssystems manuell einer Netzwerkschnittstelle zuweisen, erfahren Sie im Artikel [Zuweisen von mehreren IP-Adressen zu virtuellen Computern](virtual-network-multiple-ip-addresses-portal.md#os-config). Spezielle Überlegungen vor dem manuellen Hinzufügen von IP-Adressen zum Betriebssystem virtueller Computer finden Sie unter [private](#private) IP-Adressen. Fügen Sie dem Betriebssystem des virtuellen Computers keine öffentlichen IP-Adressen hinzu.

**Befehle**

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network nic ip-config update](/cli/azure/network/nic/ip-config?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRMNetworkInterfaceIpConfig](/powershell/module/azurerm.network/set-azurermnetworkinterfaceipconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="remove-ip-addresses"></a>Entfernen von IP-Adressen

Sie können [private](#private) und [öffentliche](#public) IP-Adressen von einer Netzwerkschnittstelle entfernen. Doch einer Netzwerkschnittstelle muss stets mindestens eine private IPv4-Adresse zugewiesen sein.

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind, beim [Azure-Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie im Artikel [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie im oberen Bereich des Azure-Portals im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Netzwerkschnittstellen* ein. Wenn **Netzwerkschnittstellen** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie auf dem daraufhin angezeigten Blatt **Netzwerkschnittstellen** auf die Netzwerkschnittstelle, von der Sie IP-Adressen entfernen möchten.
4. Klicken Sie auf dem Blatt der ausgewählten Netzwerkschnittstelle im Abschnitt **EINSTELLUNGEN** auf **IP-Konfigurationen**.
5. Klicken Sie mit der rechten Maustaste auf eine [sekundäre](#secondary) IP-Konfiguration (die [primäre](#primary) Konfiguration können Sie nicht löschen), die Sie löschen möchten. Klicken Sie auf **Löschen** und dann auf **Ja**, um den Löschvorgang zu bestätigen. Falls der Konfiguration eine öffentliche IP-Adressressource zugeordnet war, wird die Zuweisung der Ressource zur IP-Konfiguration aufgehoben, die Ressource wird jedoch nicht gelöscht.
6. Schließen Sie das Blatt **IP-Konfigurationen**.

**Befehle**

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network nic ip-config delete](/cli/azure/network/nic/ip-config?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Remove-AzureRmNetworkInterfaceIpConfig](/powershell/module/azurerm.network/remove-azurermnetworkinterfaceipconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="ip-configurations"></a>IP-Konfigurationen

[Private](#private) und (optional) [öffentliche](#public) IP-Adressen werden einer oder mehreren IP-Konfigurationen zugewiesen, die wiederum einer Netzwerkschnittstelle zugewiesen sind. Es gibt zwei Arten von IP-Konfigurationen:

### <a name="primary"></a>Primär

Jede Netzwerkschnittstelle ist einer primären IP-Konfiguration zugewiesen. Einer primären IP-Konfiguration:

- Ist eine [private](#private) [IPv4](#ipv4)-Adresse zugewiesen. Sie können eine private [IPv6](#ipv6)-Adresse nicht einer primären IP-Konfiguration zuweisen.
- Kann auch eine [öffentliche](#public) IPv4-Adresse zugewiesen werden. Sie können eine öffentliche IPv6-Adresse nicht einer primären oder sekundären IP-Konfiguration zuweisen. Sie können jedoch einer Azure Load Balancer-Instanz eine öffentliche IPv6-Adresse zuweisen, die dann einen Lastenausgleich für den Datenverkehr zur privaten IPv6-Adresse eines virtuellen Computers durchführt. Weitere Informationen finden Sie unter [Details und Einschränkungen für IPv6](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#details-and-limitations).

### <a name="secondary"></a>Sekundär

Zusätzlich zu einer primären IP-Konfiguration können einer Netzwerkschnittstelle null oder mehr sekundäre IP-Konfigurationen zugewiesen werden. Einer sekundären IP-Konfiguration:

- Muss eine private IPv4- oder IPv6-Adresse zugewiesen werden. Wenn es sich um eine IPv6-Adresse handelt, kann die Netzwerkschnittstelle nur über eine sekundäre IP-Konfiguration verfügen. Wenn es sich um eine IPv4-Adresse handelt, können der Netzwerkschnittstelle mehrere sekundäre IP-Konfiguration zugewiesen werden. Weitere Informationen zur Anzahl der öffentlichen und privaten IPv4-Adressen, die einer Netzwerkschnittstelle zugewiesen werden können, finden Sie im Artikel [Einschränkungen bei Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).  
- Kann auch eine öffentliche IPv4-Adresse zugewiesen werden, wenn die private IP-Adresse eine IPv4-Adresse ist. Wenn die private IP-Adresse eine IPv6-Adresse ist, können Sie der IP-Konfiguration keine öffentliche IPv4- oder IPv6-Adresse zuweisen. In Szenarien wie den folgenden kann es hilfreich sein, einer Netzwerkschnittstelle mehrere IP-Adressen zuzuweisen:
    - Hosten mehrerer Websites oder Dienste mit unterschiedlichen IP-Adressen und SSL-Zertifikaten auf einem einzelnen Server
    - Verwenden eines virtuellen Computers als virtuelles Netzwerkgerät (etwa als Firewall oder Lastenausgleich)
    - Fähigkeit zum Hinzufügen einer privaten IPv4-Adresse für eine der Netzwerkschnittstellen zu einem Azure Load Balancer-Back-End-Pool. Bisher konnte nur die primäre IPv4-Adresse für die primäre Netzwerkschnittstelle einem Back-End-Pool hinzugefügt werden. Weitere Informationen dazu, wie bei mehreren IPv4-Konfigurationen ein Lastenausgleich durchgeführt werden kann, finden Sie im Artikel [Lastenausgleich bei mehreren IP-Konfigurationen](../load-balancer/load-balancer-multiple-ip.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 
    - Die Möglichkeit eines Lastenausgleichs für eine IPv6-Adresse, die einer Netzwerkschnittstelle zugewiesen ist. Weitere Informationen zum Lastenausgleich bei einer privaten IPv6-Adresse finden Sie im Artikel [Lastenausgleich bei IPv6-Adressen](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).


## <a name="address-types"></a>Adresstypen

Sie können einer [IP-Konfiguration](#ip-configurations) die folgenden Typen von IP-Adressen zuweisen:

### <a name="private"></a>Private

Die privaten [IPv4](#ipv4)-Adressen ermöglichen einem virtuellen Computer die Kommunikation mit anderen Ressourcen im virtuellen Netzwerk und verbundenen Netzwerken. Die eingehende Kommunikation mit einem virtuellen Computer ist nicht möglich, und der virtuelle Computer kann auch nicht ausgehend mit einer privaten [IPv6](#ipv6)-Adresse kommunizieren. Dazu gibt es jedoch eine Ausnahme. Ein virtueller Computer kann mit Azure Load Balancer über eine IPv6-Adresse kommunizieren. Weitere Informationen finden Sie unter [Details und Einschränkungen für IPv6](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#details-and-limitations). 

Standardmäßig weisen die Azure-DHCP-Server der Netzwerkschnittstelle die private IPv4-Adresse für die [primäre IP-Konfiguration](#primary) der Netzwerkschnittstelle innerhalb des Betriebssystems des virtuellen Computers zu. Sofern dies nicht unbedingt erforderlich ist, sollten Sie die IP-Adresse einer Netzwerkschnittstelle nie manuell im Betriebssystem des virtuellen Computers festlegen. 

> [!WARNING]
> Wenn sich die als primäre IP-Adresse einer Netzwerkschnittstelle im Betriebssystem des virtuellen Computers festgelegte IPv4-Adresse je von der privaten IPv4-Adresse unterscheiden sollte, die der primären IP-Konfiguration der primären Netzwerkschnittstelle eines virtuellen Computers in Azure zugewiesen ist, verlieren Sie die Konnektivität mit dem virtuellen Computer.

Es gibt Situationen, in denen die IP-Adresse einer Netzwerkschnittstelle im Betriebssystem des virtuellen Computers manuell festgelegt werden muss. Dies gilt beispielsweise für das manuelle Festlegen der primären und sekundären IP-Adressen eines Windows-Betriebssystems beim Hinzufügen mehrerer IP-Adressen zu einem virtuellen Azure-Computer. Bei einem virtuellen Linux-Computer müssen Sie möglicherweise nur die sekundären IP-Adressen manuell festlegen. Weitere Informationen finden Sie unter [Hinzufügen von IP-Adressen zu einem VM-Betriebssystem](virtual-network-multiple-ip-addresses-portal.md#os-config). Wenn Sie die IP-Adresse im Betriebssystem manuell festlegen, sollten Sie die Adressen der IP-Konfiguration für eine Netzwerkschnittstelle immer mit der statischen (anstelle der dynamischen) Zuweisungsmethode zuweisen. Durch das Zuweisen der Adresse mithilfe der statischen Methode wird sichergestellt, dass sich die Adresse in Azure nicht ändert. Wenn Sie die einer IP-Konfiguration zugewiesene Adresse einmal ändern müssen, beachten Sie folgende Empfehlungen:

1. Ändern Sie die Zuweisung der IP-Adresse innerhalb des Betriebssystems zurück in DHCP, und starten Sie den virtuellen Computer neu, um sicherzustellen, dass der virtuelle Computer eine Adresse von den Azure-DHCP-Servern erhält.
2. Beenden Sie den virtuellen Computer (und heben die Zuordnung auf).
3. Ändern Sie die IP-Adresse für die IP-Konfiguration in Azure.
4. Starten Sie den virtuellen Computer.
5. [Konfigurieren Sie manuell](virtual-network-multiple-ip-addresses-portal.md#os-config) die sekundären IP-Adressen innerhalb des Betriebssystems (und auch die primäre IP-Adresse in Windows), damit diese mit Ihren Einstellungen in Azure übereinstimmen.
 
Wenn Sie die angegebenen Schritte befolgen, bleibt die zugewiesene private IP-Adresse der Netzwerkschnittstelle in Azure und im Betriebssystem des virtuellen Computers unverändert. Zum Nachverfolgen der virtuelle Computer innerhalb Ihres Abonnements, für die Sie manuell IP-Adressen im Betriebssystem festgelegt haben, sollten Sie den virtuellen Computern ein Azure-[Tag](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#tags) hinzufügen. Sie können z.B. „IP-Adresszuweisung: statisch“ verwenden. Auf diese Weise können Sie problemlos die virtuellen Computer in Ihrem Abonnement finden, für die Sie manuell die IP-Adresse innerhalb des Betriebssystems festgelegt haben.

Zusätzlich zum Aktivieren der Kommunikation eines virtuellen Computers mit anderen Ressourcen innerhalb desselben Netzwerks oder in verbundenen virtuellen Netzwerken ermöglicht eine private IP-Adresse einem virtuellen Computer auch die ausgehende Kommunikation mit dem Internet. Für ausgehende Verbindungen erfolgt durch Azure eine Übersetzung der Quellnetzwerkadresse in eine nicht vorhersagbare öffentliche IP-Adresse. Weitere Informationen zu ausgehenden Internetverbindungen in Azure finden Sie im Artikel [Grundlegendes zu ausgehenden Verbindungen in Azure](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Aus dem Internet ist keine eingehende Kommunikation mit dem privaten IP-Adressbereich eines virtuellen Computers möglich.

### <a name="public"></a>Öffentlich

Öffentliche IP-Adressen ermöglichen aus dem Internet eingehende Verbindungen mit einem virtuellen Computer. Ausgehende Verbindungen mit dem Internet verwenden eine vorhersagbare IP-Adresse. Weitere Informationen finden Sie unter [Grundlegendes zu ausgehenden Verbindungen in Azure](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Sie können einer IP-Konfiguration eine öffentliche IP-Adresse zuweisen, was jedoch nicht erforderlich ist. Wenn Sie einem virtuellen Computer keine öffentliche IP-Adresse zuweisen, kann dieser trotzdem über die private IP-Adresse ausgehend mit dem Internet kommunizieren. Weitere Informationen zu öffentlichen IP-Adressen finden Sie im Artikel [Öffentliche IP-Adressen](virtual-network-public-ip-address.md).

Die Anzahl öffentlicher und privater IP-Adressen, die Sie einer Netzwerkschnittstelle zuweisen können, ist begrenzt. Ausführliche Informationen finden Sie im Artikel [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

> [!NOTE]
> Azure übersetzt die private IP-Adresse eines virtuellen Computers in eine öffentliche IP-Adresse. Daher weiß das Betriebssystem nichts über die ihm zugewiesenen öffentlichen IP-Adressen. Aus diesem Grund besteht auch keine Notwendigkeit, innerhalb des Betriebssystems eine öffentliche IP-Adresse manuell zuzuweisen.

## <a name="assignment-methods"></a>Zuweisungsmethoden

Öffentliche und private IP-Adressen können mithilfe der folgenden Zuweisungsmethoden zugewiesen werden:

### <a name="dynamic"></a>Dynamisch

Standardmäßig werden dynamische private IPv4- und (optionale) IPv6-Adressen zugewiesen. Dynamische Adressen können sich ändern, wenn der virtuelle Computer neu gestartet wird, nachdem er sich zuvor im Zustand „Beendet (Zuordnung aufgehoben)“ befand. Wenn Sie nicht möchten, dass sich die IPv4-Adressen für die Lebensdauer des virtuellen Computers ändern, weisen Sie die Adressen mithilfe der statischen Methode zu. Sie können mit der dynamischen Zuweisungsmethode nur eine private IPv6-Adresse zuweisen. Sie können eine öffentliche IPv6-Adresse mit keiner der beiden Methoden einer IP-Konfiguration zuweisen.

### <a name="static"></a>statischen

Die mithilfe der statischen Methode zugewiesenen Adressen ändern sich bis zum Löschen des virtuellen Computers nicht. Sie weisen einer IP-Konfiguration manuell eine statische private IPv4-Adresse aus dem Adressraum für das Subnetz der Netzwerkschnittstelle zu. Sie können einer IP-Konfiguration (optional) eine öffentliche oder private statische IPv4-Adresse zuweisen. Sie können einer IP-Konfiguration jedoch keine öffentliche oder private statische IPv6-Adresse zuweisen. Weitere Informationen dazu, wie Azure öffentliche statische IPv4-Adressen zuweist, finden Sie im Artikel [Öffentliche IP-Adressen](virtual-network-public-ip-address.md).

## <a name="ip-address-versions"></a>IP-Adressversionen

Sie können beim Zuweisen von Adressen die folgenden Versionen angeben:

### <a name="ipv4"></a>IPv4

Jede Netzwerkschnittstelle benötigt eine [primäre](#primary) IP-Konfiguration mit einer zugewiesenen [privaten](#private) [IPv4](#ipv4)-Adresse. Sie können eine oder mehrere [sekundäre](#secondary) IP-Konfigurationen hinzufügen, die jeweils über eine private IPv4- und (optional) eine [öffentliche](#public) IPv4-Adresse verfügen.

### <a name="ipv6"></a>IPv6

Sie können der sekundären IP-Konfiguration einer Netzwerkschnittstelle null oder eine private [IPv6](#ipv6)-Adresse zuweisen. Die Netzwerkschnittstelle kann nicht über sekundäre IP-Konfigurationen verfügen. Sie können über das Portal keine IP-Konfiguration mit einer IPv6-Adresse hinzufügen. Verwenden Sie PowerShell oder die Befehlszeilenschnittstelle, um einer vorhandenen Netzwerkschnittstelle eine IP-Konfiguration mit einer privaten IPv6-Adresse hinzuzufügen. Die Netzwerkschnittstelle kann nicht an vorhandene virtuelle Computer angefügt werden.

> [!NOTE]
> Sie können über das Portal zwar eine Netzwerkschnittstelle mit einer IPv6-Adresse erstellen, es ist jedoch nicht möglich, einem neuen oder vorhandenen virtuellen Computer über das Portal eine vorhandene Netzwerkschnittstelle hinzuzufügen. Verwenden Sie PowerShell oder Azure CLI 2.0, um eine Netzwerkschnittstelle mit einer privaten IPv6-Adresse zu erstellen, und fügen Sie die Netzwerkschnittstelle dann beim Erstellen eines virtuellen Computers an. Eine Netzwerkschnittstelle mit einer zugewiesenen privaten IPv6-Adresse kann nicht einem vorhandenen virtuellen Computer angefügt werden. Sie können einer IP-Konfiguration für eine beliebige Netzwerkschnittstelle eines virtuellen Computers keine private IPv6-Adresse hinzufügen (egal ob mit Portal, CLI oder PowerShell).

Sie können eine öffentliche IPv6-Adresse nicht einer primären oder sekundären IP-Konfiguration zuweisen.

## <a name="next-steps"></a>Nächste Schritte
Um einen virtuellen Computer mit unterschiedlichen IP-Konfigurationen zu erstellen, lesen Sie die folgenden Artikel:

|Aufgabe|Tool|
|---|---|
|Erstellen eines virtuellen Computers mit mehreren Netzwerkschnittstellenkarten (NICs)|[CLI](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Erstellen eines virtuellen Computers mit einer NIC und mehreren IPv4-Adressen|[CLI](virtual-network-multiple-ip-addresses-cli.md), [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Erstellen eines virtuellen Computers mit einer NIC und einer IPv6-Adresse (mit Azure Load Balancer)|[CLI](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [Azure Resource Manager-Vorlage](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|

