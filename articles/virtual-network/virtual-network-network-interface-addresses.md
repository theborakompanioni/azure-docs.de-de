---
title: "Konfigurieren von IP-Adressen für Azure-Netzwerkschnittstellen | Microsoft-Dokumentation"
description: "Informationen zum Hinzufügen, Ändern und Entfernen öffentlicher und privater IP-Adressen für NICs"
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
ms.sourcegitcommit: afa23b1395b8275e72048bd47fffcf38f9dcd334
ms.openlocfilehash: cf7dbc648136897772bbd068687313eec16bed75
ms.contentlocale: de-de
ms.lasthandoff: 05/12/2017


---

# <a name="add-change-or-remove-ip-addresses-for-azure-network-interfaces"></a>Hinzufügen, Ändern oder Entfernen von IP-Adressen für Azure-Netzwerkschnittstellen

Informationen zum Hinzufügen, Ändern und Entfernen öffentlicher und privater IP-Adressen für Netzwerkschnittstellenkarten (NICs). Private IP-Adressen, die einer NIC zugewiesen sind, ermöglichen einer VM die Kommunikation mit dem Internet und anderen Ressourcen, die mit einem virtuellen Azure-Netzwerk (VNet) verbunden sind. Öffentliche IP-Adressen, die einer NIC zugewiesen sind, ermöglichen aus dem Internet eingehende Kommunikation mit einer VM. 

Informationen zum Erstellen, Ändern oder Löschen von NICs finden Sie im Artikel zu [Erstellen, Ändern oder Löschen von Netzwerkschnittstellenkarten](virtual-network-network-interface.md). Informationen zum Hinzufügen von NICs zu oder Entfernen von NICs von VMs finden Sie im Artikel zum [Hinzufügen und Entfernen von NICs](virtual-network-network-interface-vm.md). 


## <a name="before"></a>Voraussetzungen

Führen Sie zuerst die folgenden Aufgaben aus, ehe Sie die Schritte in den Abschnitten dieses Artikels durchführen:

- Lesen Sie den Artikel [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits), um mehr über Einschränkungen für öffentliche und private IP-Adressen zu erfahren.
- Melden Sie sich mit einem Azure-Konto beim Azure-Portal, der Azure-Befehlszeilenschnittstelle (CLI) oder bei Azure PowerShell an. Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für ein [kostenloses Testkonto](https://azure.microsoft.com/free) registrieren.
- Bei Verwenden von PowerShell zum Ausführen der Aufgaben in diesem Artikel installieren und konfigurieren Sie Azure PowerShell gemäß den Anweisungen im Artikel [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Stellen Sie sicher, dass Sie die neueste Version der Azure PowerShell-Cmdlets installiert haben. Hilfe und Beispiele für PowerShell-Befehle erhalten Sie durch Eingabe von `get-help <command> -full`.
- Bei Verwenden der Azure CLI zum Ausführen der Aufgaben in diesem Artikel installieren und konfigurieren Sie die Azure CLI gemäß den Anweisungen im Artikel zum [Installieren und Konfigurieren der Azure CLI](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Stellen Sie sicher, dass Sie die neueste Version der Azure CLI installiert haben. Hilfe zu den Befehlen der Befehlszeilenschnittstelle erhalten Sie durch Eingabe von `az <command> --help`.

## <a name="about"></a>Informationen zu NICs und IP-Adressen

Jeder Netzwerkkarte können mehrere private und öffentliche IP-Adressen innerhalb der [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) zugewiesen werden. In Szenarien wie den folgenden kann es hilfreich sein, einer NIC mehrere IP-Adressen zuzuweisen:
- Hosten mehrerer Websites oder Dienste mit unterschiedlichen IP-Adressen und SSL-Zertifikaten auf einem einzelnen Server
- Verwenden eines virtuellen Computers als virtuelles Netzwerkgerät (etwa als Firewall oder Lastenausgleich)
- Fähigkeit zum Hinzufügen einer privaten IP-Adresse für eine der NICs zu einem Azure Load Balancer-Back-End-Pool hinzufügen. Bisher konnte nur die primäre IP-Adresse für die primäre NIC einem Back-End-Pool hinzugefügt werden. Weitere Informationen dazu, wie in Konfigurationen mit mehreren IPs ein Lastenausgleich durchgeführt werden kann, finden Sie im Artikel [Load balancing multiple IP configurations](../load-balancer/load-balancer-multiple-ip.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Lastenausgleich bei Konfigurationen mit mehreren IPs).

IP-Adressen werden einer IP-Konfiguration zugewiesen. Einer NIC wird stets eine **primäre** IP-Konfiguration zugewiesen, ihr können aber auch mehrere **sekundäre** Konfigurationen zugewiesen sein. IP-Konfigurationen werden einem oder beiden der folgenden Typen von Adressen zugewiesen:
- **Privat:** Private IP-Adressen ermöglichen einer VM die Kommunikation mit anderen Ressourcen, die mit dem VNet verbunden sind, in dem sich die NIC befindet. Einer IP-Konfiguration muss eine private IP-Adresse zugewiesen sein. Die Azure-DHCP-Server weisen der NIC innerhalb des Betriebssystems des virtuellen Computers die private IP-Adresse für die primäre IP-Konfiguration der NIC zu. Eine private IP-Adresse ermöglicht der VM auch ausgehende Kommunikation mit dem Internet. Für ausgehende Verbindungen erfolgt eine Übersetzung in die Quellnetzwerkadresse (SNAT). Weitere Informationen zu ausgehenden Internetverbindungen in Azure finden Sie im Artikel [Grundlegendes zu ausgehenden Verbindungen in Azure](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Aus dem Internet ist keine eingehende Kommunikation mit dem privaten IP-Adressbereich einer VM möglich.
- **Öffentlich:** Öffentliche IP-Adressen ermöglichen aus dem Internet eingehende Verbindungen mit einer VM. Für ausgehende Verbindungen in das Internet erfolgt keine Übersetzung in die Quellnetzwerkadresse (SNAT). Sie können einer IP-Konfiguration eine öffentliche IP-Adresse zuweisen, was jedoch nicht erforderlich ist. Weitere Informationen zu öffentlichen IP-Adressen finden Sie im Artikel [Öffentliche IP-Adressen](virtual-network-public-ip-address.md).

Die Anzahl öffentlicher und privater IP-Adressen, die Sie einer NIC zuweisen können, ist begrenzt. Ausführliche Informationen finden Sie im Artikel [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

Öffentliche und private IP-Adressen können mithilfe der folgenden Zuordnungsmethoden zugewiesen werden:
- **Dynamisch:** Dynamische private und öffentliche IP-Adressen werden standardmäßig zugewiesen. Dynamische Adressen können sich ändern, wenn die VM neu gestartet wird, nachdem sie sich zuvor im Zustand „Beendet (Zuordnung aufgehoben)“ befand und neu gestartet wurde. Wenn sich die IP-Adresse für die Lebensdauer des virtuellen Computers nicht ändern soll, verwenden Sie eine statische Adresse.
- **Statisch:** Statische Adressen bleiben unverändert, bis eine VM gelöscht wird. Sie weisen eine statische private IP-Adresse aus dem Adressraum des Subnetzes zu, mit dem die NIC verbunden ist. Weitere Informationen dazu, wie Azure öffentliche IP-Adressen zuweist, finden Sie im Artikel [Öffentliche IP-Adressen](virtual-network-public-ip-address.md).

## <a name="create-ip-config"></a>Hinzufügen von IP-Adressen

Einer NIC können beliebig viele IP-Adressen hinzugefügt werden (im Rahmen der im Artikel [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) aufgeführten Grenzwerte).

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind, beim [Azure-Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie im Artikel [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie im oberen Bereich des Azure-Portals im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Netzwerkschnittstellen* ein. Wenn **Netzwerkschnittstellen** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie auf dem daraufhin angezeigten Blatt **Netzwerkschnittstellen** auf die NIC, der Sie eine IP-Adresse hinzufügen möchten.
4. Klicken Sie auf dem Blatt der ausgewählten NIC im Abschnitt **EINSTELLUNGEN** auf **IP-Konfigurationen**.
5. Klicken Sie auf dem Blatt, das für IP-Konfigurationen geöffnet wird, auf **+ Hinzufügen**.
6. Geben Sie Folgendes an, und klicken Sie anschließend auf **OK**, um das Blatt **IP-Konfiguration hinzufügen** zu schließen:

    |Einstellung|Erforderlich|Details|
    |---|---|---|
    |Name|Ja|Muss für die NIC eindeutig sein.|
    |Typ|Ja|Da Sie die IP-Konfiguration einer vorhandenen NIC hinzufügen und jede NIC über eine primäre IP-Konfiguration verfügen muss, steht nur die Option **Sekundär** zur Verfügung.|
    |Zuweisungsmethode für private IP-Adressen|Ja|Adressen vom Typ **Dynamisch** können sich ändern, wenn der virtuelle Computer neu gestartet wird, nachdem er sich zuvor im Zustand „Beendet (Zuordnung aufgehoben)“ befand. Azure weist eine verfügbare Adresse aus dem Adressraum des Subnetzes zu, mit dem die NIC verbunden ist. Adressen vom Typ **Statisch** werden erst freigegeben, wenn die NIC gelöscht wird. Geben Sie eine IP-Adresse aus dem Subnetzadressbereich an, die derzeit nicht von einer anderen IP-Konfiguration verwendet wird.|
    |Öffentliche IP-Adresse|Nein|**Deaktiviert:** Der IP-Konfiguration ist derzeit keine öffentliche IP-Adressressource zugeordnet. **Aktiviert:** Wählen Sie eine vorhandene öffentliche IP-Adresse aus, oder erstellen Sie eine neue. Eine Anleitung zur Erstellung einer öffentlichen IP-Adresse finden Sie im Artikel [Public IP addresses](virtual-network-public-ip-address.md#create) (Öffentliche IP-Adressen).|
7. Fügen Sie dem Betriebssystem des virtuellen Computers manuell sekundäre private IP-Adressen hinzu, wie im Artikel [Zuweisen von mehreren IP-Adressen zu virtuellen Computern mithilfe des Azure-Portals](virtual-network-multiple-ip-addresses-portal.md#os-config) beschrieben. Fügen Sie dem Betriebssystem des virtuellen Computers keine öffentlichen IP-Adressen hinzu.

**Befehle**

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network nic ip-config create](/cli/azure/network/nic/ip-config?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Add-AzureRmNetworkInterfaceIpConfig](/powershell/resourcemanager/azurerm.network/v3.4.0/add-azurermnetworkinterfaceipconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="change-ip-config"></a>Ändern von IP-Adresseinstellungen

Sie müssen ggf. die Zuweisungsmethode einer IP-Adresse, die statische IP-Adresse oder die einer NIC zugewiesene öffentliche IP-Adresse ändern. Wenn Sie die private IP-Adresse einer sekundären IP-Konfiguration ändern, die einer sekundären NIC einer VM zugeordnet ist (unter [Primäre und sekundäre NICs](virtual-network-network-interface-vm.md#about) erfahren Sie mehr), ändern Sie den Status der VM in „Beendet (Zuordnung aufgehoben)“, bevor Sie die folgenden Schritte ausführen: 

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind, beim [Azure-Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie im Artikel [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie im oberen Bereich des Azure-Portals im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Netzwerkschnittstellen* ein. Wenn **Netzwerkschnittstellen** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie auf dem daraufhin angezeigten Blatt **Netzwerkschnittstellen** auf die NIC, deren IP-Adresseinstellungen Sie anzeigen oder ändern möchten.
4. Klicken Sie auf dem Blatt der ausgewählten NIC im Abschnitt **EINSTELLUNGEN** auf **IP-Konfigurationen**.
5. Klicken Sie auf dem Blatt, das für IP-Konfigurationen geöffnet wird, in der Liste auf die IP-Konfiguration, die Sie ändern möchten.
6. Ändern Sie die Einstellungen nach Wunsch. Informationen zu den Einstellungen finden Sie in diesem Artikel im Abschnitt [Hinzufügen einer IP-Konfiguration](#create-ip-config) in Schritt 6. Klicken Sie anschließend auf **Speichern**, um das Blatt für die ausgewählte IP-Konfiguration zu schließen.

>[!NOTE]
>Falls die primäre NIC über mehrere IP-Konfigurationen verfügt und Sie die private IP-Adresse der primären IP-Konfiguration ändern, müssen Sie unter Windows alle sekundären IP-Adressen manuell wieder der NIC zuweisen. (Unter Linux ist dies nicht erforderlich.) Wie Sie IP-Adressen im Rahmen eines Betriebssystems manuell einer NIC zuweisen, erfahren Sie im Artikel [Zuweisen von mehreren IP-Adressen zu virtuellen Computern mithilfe des Azure-Portals](virtual-network-multiple-ip-addresses-portal.md#os-config). Fügen Sie dem Betriebssystem des virtuellen Computers keine öffentlichen IP-Adressen hinzu.

**Befehle**

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network nic ip-config update](/cli/azure/network/nic/ip-config?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRMNetworkInterfaceIpConfig](/powershell/resourcemanager/azurerm.network/v3.4.0/set-azurermnetworkinterfaceipconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="delete-ip-config"></a>Entfernen von IP-Adressen

Sie können private und öffentliche IP-Adressen von einer NIC entfernen. Doch einer NIC muss stets mindestens eine private IP-Adresse zugewiesen sein.

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind, beim [Azure-Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie im Artikel [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie im oberen Bereich des Azure-Portals im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Netzwerkschnittstellen* ein. Wenn **Netzwerkschnittstellen** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie auf dem daraufhin angezeigten Blatt **Netzwerkschnittstellen** auf die NIC, von der Sie IP-Adressen entfernen möchten.
4. Klicken Sie auf dem Blatt der ausgewählten NIC im Abschnitt **EINSTELLUNGEN** auf **IP-Konfigurationen**.
5. Klicken Sie mit der rechten Maustaste auf eine sekundäre IP-Konfiguration (die primäre Konfiguration können Sie nicht löschen), die Sie löschen möchten. Klicken Sie auf **Löschen** und dann auf **Ja**, um den Löschvorgang zu bestätigen. Falls der Konfiguration eine öffentliche IP-Adressressource zugeordnet war, wird die Zuweisung der Ressource zur IP-Konfiguration aufgehoben, die Ressource wird jedoch nicht gelöscht.
6. Schließen Sie das Blatt **IP-Konfigurationen**.

**Befehle**

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network nic ip-config delete](/cli/azure/network/nic/ip-config?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Remove-AzureRmNetworkInterfaceIpConfig](/powershell/resourcemanager/azurerm.network/v3.4.0/remove-azurermnetworkinterfaceipconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## <a name="next-steps"></a>Nächste Schritte
In den folgenden Artikeln erfahren Sie, wie Sie virtuelle Computer mit mehreren NICs oder IP-Adressen erstellen:

**Befehle**

|Task|Tool|
|---|---|
|Erstellen eines virtuellen Computers mit mehreren Netzwerkschnittstellenkarten (NICs)|[BEFEHLSZEILENSCHNITTSTELLE (CLI)](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
||[PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Erstellen eines virtuellen Computers mit einer NIC und mehreren IP-Adressen|[BEFEHLSZEILENSCHNITTSTELLE (CLI)](virtual-network-multiple-ip-addresses-cli.md)|
||[PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|

