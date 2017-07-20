---
title: "Erstellen, Ändern oder Löschen von öffentlichen Azure-IP-Adressen | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie öffentliche IP-Adressen erstellen, ändern oder löschen können."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: bb71abaf-b2d9-4147-b607-38067a10caf6
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/05/2017
ms.author: jdial
ms.translationtype: Human Translation
ms.sourcegitcommit: 7948c99b7b60d77a927743c7869d74147634ddbf
ms.openlocfilehash: 47237fe499cd9244266487cd97f6be0d2cb2e977
ms.contentlocale: de-de
ms.lasthandoff: 06/20/2017


---

# <a name="create-change-or-delete-public-ip-addresses"></a>Erstellen, Ändern oder Löschen öffentlicher IP-Adressen

Sie erhalten Informationen über öffentliche IP-Adressen und darüber, wie Sie diese erstellen, ändern und löschen können. Eine öffentliche IP-Adresse ist eine Ressource mit eigenen konfigurierbaren Einstellungen. Wird anderen Azure-Ressourcen eine öffentliche IP-Adresse zugewiesen, wird Folgendes ermöglicht:
- Eingehende Internetkonnektivität mit Ressourcen wie Azure Virtual Machines (VM), Azure-VM-Skalierungsgruppen, Azure-VPN-Gateway und internetfähigen Instanzen von Azure Load Balancer.
- Ausgehende Konnektivität mit dem Internet, für die keine Netzwerkadressübersetzung (Network Address Translation, NAT) verwendet wird. Beispielsweise kann ein virtueller Computer, obwohl ihm keine öffentliche IP-Adresse zugewiesen ist, ausgehend mit dem Internet kommunizieren, weil seine Netzwerkadresse von Azure übersetzt wird. Weitere Informationen zu ausgehenden Verbindungen von Azure-Ressourcen finden Sie im Artikel [Grundlegendes zu ausgehenden Verbindungen in Azure](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Dieser Artikel beschreibt, die wie mit öffentlichen IP-Adressen arbeiten, die über das Azure Resource Manager-Bereitstellungsmodell bereitgestellt werden.

## <a name="before"></a>Voraussetzungen

Führen Sie zuerst die folgenden Aufgaben aus, ehe Sie die Schritte in den Abschnitten dieses Artikels durchführen:

- Lesen Sie den Artikel zu [Einschränkungen bei Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits), um mehr über die Einschränkungen für öffentliche IP-Adressen zu erfahren.
- Melden Sie sich mit einem Azure-Konto beim Azure-Portal, der Azure-Befehlszeilenschnittstelle (CLI) oder bei Azure PowerShell an. Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für ein [kostenloses Testkonto](https://azure.microsoft.com/free) registrieren.
- Bei Verwenden von PowerShell zum Ausführen der Aufgaben in diesem Artikel installieren und konfigurieren Sie Azure PowerShell gemäß den Anweisungen im Artikel [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Stellen Sie sicher, dass Sie die neueste Version der Azure PowerShell-Cmdlets installiert haben. Hilfe und Beispiele für PowerShell-Befehle erhalten Sie durch Eingabe von `get-help <command> -full`.
- Bei Verwenden der Azure CLI zum Ausführen der Aufgaben in diesem Artikel installieren und konfigurieren Sie die Azure CLI gemäß den Anweisungen im Artikel zum [Installieren und Konfigurieren der Azure CLI](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Stellen Sie sicher, dass Sie die neueste Version der Azure CLI installiert haben. Hilfe zu den Befehlen der Befehlszeilenschnittstelle erhalten Sie durch Eingabe von `az <command> --help`.

Für öffentliche IP-Adressen fällt eine Schutzgebühr an. Informationen zu den Preisen finden Sie auf der Seite [Preise für IP-Adressen](https://azure.microsoft.com/pricing/details/ip-addresses). 

## <a name="create"></a>Erstellen einer öffentlichen IP-Adresse

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind, beim [Azure-Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie im Artikel [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie im oberen Bereich des Azure-Portals im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *öffentliche IP-Adresse* ein. Wenn **Öffentliche IP-Adressen** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie auf dem Blatt **Öffentliche IP-Adresse** auf **+ Hinzufügen**.
4. Das Blatt **Öffentliche IP-Adresse erstellen** wird angezeigt. Geben Sie hier direkt oder durch Auswählen Werte für die folgenden Einstellungen ein, und klicken Sie dann auf **Erstellen**:

    |Einstellung|Erforderlich|Details|
    |---|---|---|
    |Name|Ja|Der Name muss innerhalb der ausgewählten Ressourcengruppe eindeutig sein.|
    |IP-Adresszuweisung|Ja|**Dynamisch:** Dynamische Adressen werden nur zugewiesen, nachdem die öffentliche IP-Adresse einer Netzwerkkarte (NIC) zugewiesen wurde, die mit einem virtuellen Computer verbunden ist, und der virtuelle Computer erstmalig gestartet wurde. Dynamische Adressen können sich ändern, wenn der virtuelle Computer, mit dem die NIC verbunden ist, beendet (Zuordnung aufgehoben) wird. Die Adresse bleibt unverändert, wenn derselbe virtuelle Computer neu gestartet oder (ohne Aufheben der Zuordnung) beendet wird. **Statisch:** Statische Adressen werden zugewiesen, wenn die öffentliche IP-Adresse erstellt wird. Statische Adressen ändern sich nicht, selbst wenn der virtuelle Computer in den Status „Beendet (Zuordnung aufgehoben)“ versetzt wird. Die Adresse wird nur freigegeben, wenn die NIC gelöscht wird. Die Zuweisungsmethode kann nach Erstellung der NIC geändert werden.|
    |Leerlaufzeitüberschreitung (Minuten)|Nein|Gibt an, wie viele Minuten eine TCP- oder HTTP-Verbindung geöffnet bleiben soll, ohne dass Clients Keep-Alive-Meldungen senden müssen.|
    |DNS-Namensbezeichnung|Nein|Muss in dem Azure-Standort, in dem Sie den Namen erstellen, eindeutig sein (über alle Abonnements und Kunden hinweg). Der öffentliche DNS-Dienst von Azure registriert den Namen und die IP-Adresse automatisch, sodass Sie über den Namen eine Verbindung mit der Ressource herstellen können. Azure fügt *location.cloudapp.azure.com* (wobei „location“ der Standort ist, den Sie auswählen) an den von Ihnen bereitgestellten Namen an, um den vollqualifizierten DNS-Namen zu erstellen.|
    |Abonnement|Ja|Muss im selben [Abonnement](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) wie die Ressource vorhanden sein, der Sie die öffentliche IP-Adresse zuordnen möchten|
    |Ressourcengruppe|Ja|Kann in derselben oder in einer anderen [Ressourcengruppe](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) wie die Ressource vorhanden sein, der Sie die öffentliche IP-Adresse zuordnen möchten|
    |Ort|Ja|Muss am selben [Standort](https://azure.microsoft.com/regions), auch als Region bezeichnet, wie die Ressource vorhanden sein, der Sie die öffentliche IP-Adresse zuordnen möchten.|

**Befehle**

|Tool|Befehl|
|---|---|
|CLI|[az network public-ip-create](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress)|

## <a name="change"></a>Anzeigen, Ändern von Einstellungen oder Löschen einer öffentlichen IP-Adresse

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind, beim [Azure-Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie im Artikel [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie im oberen Bereich des Azure-Portals im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *öffentliche IP-Adresse* ein. Wenn **Öffentliche IP-Adressen** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie auf dem angezeigten Blatt **Öffentliche IP-Adressen** auf den Namen der öffentlichen IP-Adresse, deren Einstellungen Sie anzeigen, ändern oder die Sie löschen möchten.
4. Führen Sie auf dem für die öffentliche IP-Adresse angezeigten Blatt abhängig davon, ob Sie die öffentliche IP-Adresse löschen oder ändern möchten, eine der folgenden Optionen aus.
    - **Ansicht**: Im Abschnitt „Übersicht“ des Blattes werden die wichtigsten Einstellungen für die öffentliche IP-Adresse angezeigt, darunter die zugeordnete Netzwerkschnittstelle (sofern die Adresse einer Netzwerkschnittstelle zugeordnet ist).
    - **Löschen:** Um die öffentliche IP-Adresse zu löschen, klicken Sie im Blattabschnitt **Übersicht** auf **Löschen**. Ist die Adresse zurzeit einer IP-Konfiguration zugeordnet, kann sie nicht gelöscht werden. Ist die Adresse zurzeit einer Konfiguration zugeordnet, klicken Sie auf **Trennen**, um die Adresse von der IP-Konfiguration zu trennen.
    - **Ändern:** Klicken Sie auf **Konfiguration**. Ändern Sie Einstellungen anhand der Informationen aus Schritt 4 des Abschnitts [Erstellen einer öffentlichen IP-Adresse](#create) in diesem Artikel. Möchten Sie die Zuweisung von statisch in dynamisch ändern, müssen Sie die öffentliche IP-Adresse zunächst von der IP-Konfiguration trennen, der sie zugewiesen ist. Danach können Sie die Zuweisungsmethode in dynamisch ändern und auf **Zuordnen** klicken, um die IP-Adresse zur selben IP-Konfiguration oder zu einer anderen Konfiguration zuzuordnen. Sie können die Trennung aber auch beibehalten. Um eine öffentliche IP-Adresse von einer IP-Konfiguration zu trennen, klicken Sie in der **Übersicht** auf **Trennen**.

>[!WARNING]
>Wenn Sie die Zuweisungsmethode von statisch in dynamisch ändern, geht die IP-Adresse verloren, die der öffentlichen IP-Adresse zugewiesen war. Während die öffentlichen DNS-Server von Azure eine Zuordnung zwischen statischen oder dynamischen Adressen und jeder DNS-Namensbezeichnung (sofern eine solche definiert ist) verwalten, kann sich eine dynamische IP-Adresse ändern, wenn der virtuelle Computer gestartet wird, nachdem er sich im Status „Beendet (Zuordnung aufgehoben)“ befunden hat. Möchten Sie verhindern, dass sich die Adresse ändert, weisen Sie eine statische IP-Adresse zu.

**Befehle**

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network public-ip-list](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#list) listet die öffentlichen IP-Adressen auf, [az network public-ip-show](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#show) zeigt die Einstellungen an, [az network public-ip update](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#update) führt eine Aktualisierung durch, [az network public-ip delete](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#delete) löscht|
|PowerShell|[Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress?toc=%2fazure%2fvirtual-network%2ftoc.json) ruft ein öffentliches IP-Adressenobjekt ab und zeigt dessen Einstellungen an, [Set-AzureRmPublicIpAddress](/powershell/resourcemanager/azurerm.network/set-azurermpublicipaddress?toc=%2fazure%2fvirtual-network%2ftoc.json) aktualisiert die Einstellungen, [Remove-AzureRmPublicIpAddress](/powershell/module/azurerm.network/remove-azurermpublicipaddress) löscht|

## <a name="next-steps"></a>Nächste Schritte
Zuweisen von öffentlichen IP-Adressen, wenn die folgenden Azure-Ressourcen erstellt werden:

- [Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-network%2ftoc.json)- oder [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)-VMs
- [Azure Load Balancer mit Internetzugriff](../load-balancer/load-balancer-get-started-internet-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Azure Application Gateway](../application-gateway/application-gateway-create-gateway-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Standort-zu-Standort-Verbindung über ein Azure VPN Gateway](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Azure VM-Skalierungsgruppe](../virtual-machine-scale-sets/virtual-machine-scale-sets-portal-create.md?toc=%2fazure%2fvirtual-network%2ftoc.json)

