---
title: "Beschleunigter Netzwerkbetrieb für einen virtuellen Computer mithilfe des Azure-Portals | Microsoft Docs"
description: "Erfahren Sie mehr über das Konfigurieren des beschleunigten Netzwerkbetriebs für einen virtuellen Azure-Computer mithilfe des Azure-Portals."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: af4515c6-4377-4d4a-a104-18fe1348252c
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/26/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 63f2f6dde56c1b5c4b3ad2591700f43f6542874d
ms.openlocfilehash: a2e8e0dc40a63c363f295149e35f9823c8e90fa8
ms.lasthandoff: 02/28/2017


---
# <a name="accelerated-networking-for-a-virtual-machine-using-the-azure-portal"></a>Beschleunigter Netzwerkbetrieb für einen virtuellen Computer mithilfe des Azure-Portals
> [!div class="op_single_selector"]
> * [Azure-Portal](virtual-network-accelerated-networking-portal.md)
> * [PowerShell](virtual-network-accelerated-networking-powershell.md)
> 
> 

Accelerated Networking ermöglicht die E/A-Virtualisierung mit Einzelstamm (Single Root I/O Virtualization, SR-IOV) in einem virtuellen Computer (VM) und somit eine erhebliche Steigerung der Netzwerkleistung. Über diesen Hochleistungspfad wird der Host des Datenpfads umgangen, um Latenzen, Jitter und CPU-Auslastung zu verringern. So können auf unterstützten VM-Typen die anspruchsvollsten Netzwerkworkloads genutzt werden. Dieser Artikel erläutert die Verwendung des Azure-Portals für das Konfigurieren des beschleunigten Netzwerkbetriebs im Azure Resource Manager-Bereitstellungsmodell. Sie können auch einen virtuellen Computer mit beschleunigtem Netzwerkbetrieb mithilfe von Azure PowerShell erstellen. Informationen zur Vorgehensweise erhalten Sie, indem Sie am Anfang dieses Artikels auf das Feld „PowerShell“ klicken.

Die folgende Abbildung zeigt die Kommunikation zwischen zwei virtuellen Computern (VM) mit und ohne Accelerated Networking:

![Vergleich](./media/virtual-network-accelerated-networking-portal/image1.png)

Ohne Accelerated Networking muss sämtlicher Netzwerkdatenverkehr zur und von der VM den Host und den virtuellen Switch passieren. Der virtuelle Switch setzt alle Richtlinien für den Netzwerkdatenverkehr um, z.B. Netzwerksicherheitsgruppen, Zugriffssteuerungslisten, Isolation und andere virtualisierte Netzwerkdienste. Weitere Informationen finden Sie im Artikel [Hyper-V Network Virtualization and Virtual Switch](https://technet.microsoft.com/library/jj945275.aspx) (Hyper-V-Netzwerkvirtualisierung und virtueller Switch).

Mit Accelerated Networking gelangt der Netzwerkdatenverkehr zur Netzwerkkarte (NIC) und wird dann an den virtuellen Computer weitergeleitet. Alle Netzwerkrichtlinien, die ohne Accelerated Networking vom virtuellen Switch angewendet werden, werden ausgelagert und in der Hardware angewendet. Durch das Anwenden von Richtlinien in der Hardware kann die NIC Netzwerkdatenverkehr direkt an den virtuellen Computer weiterleiten und dabei den Host und den virtuellen Switch umgehen, während gleichzeitig sämtliche Richtlinien umgesetzt werden, die sonst auf dem Host angewendet worden wären.

Die Vorteile des Accelerated Networking gelten nur für den virtuellen Computer, auf dem dieses Feature aktiviert ist. Die besten Ergebnisse erzielen Sie, wenn Sie dieses Feature auf mindestens zwei virtuellen Computern aktivieren, die mit demselben VNET verbunden sind. Bei der Kommunikation über VNETs oder bei lokalen Verbindungen hat dieses Feature nur minimale Auswirkungen auf die Latenz.

[!INCLUDE [virtual-network-preview](../../includes/virtual-network-preview.md)]

## <a name="benefits"></a>Vorteile
* **Geringere Latenz/mehr Pakete pro Sekunde (pps):** Durch das Entfernen des virtuellen Switch aus dem Datenpfad wird die Zeit reduziert, die Pakete auf dem Host für die Verarbeitung von Richtlinien warten müssen. Gleichzeitig wird die Anzahl von Paketen erhöht, die im virtuellen Computer verarbeitet werden können.
* **Reduzierte Jitter:** Die Verarbeitung im virtuellen Switch hängt vom Umfang der anzuwendenden Richtlinien und von der Workload der CPU ab, die die Verarbeitung durchführt. Das Auslagern der Richtlinienerzwingung auf die Hardware entfernt diesen Umweg, indem die Pakete direkt an den virtuellen Computer gesendet werden. Damit werden der Host und alle Softwareinterrupts und Kontextwechsel aus der VM-Kommunikation entfernt.
* **Verringerte CPU-Auslastung:** Das Umgehen des virtuellen Switchs auf dem Host führt zu weniger CPU-Auslastung für die Verarbeitung des Netzwerkdatenverkehrs.

## <a name="limitations"></a>Einschränkungen
Die folgenden Einschränkungen gelten für die Verwendung dieser Funktion:

* **Erstellung von Netzwerkschnittstellen:** Accelerated Networking kann nur für eine neue Netzwerkschnittstelle aktiviert werden.  Auf einer vorhandenen Netzwerkschnittstelle kann die Funktion nicht aktiviert werden.
* **VM-Erstellung:** Eine Netzwerkschnittstelle, auf der Accelerated Networking aktiviert ist, kann nur an eine VM angefügt werden, während diese VM erstellt wird. Die Netzwerkschnittstelle kann nicht an vorhandene virtuelle Computer angefügt werden.
* **Regionen:** Dieses Angebot gilt nur in den Regionen USA, Westen-Mitte und Europa, Westen. Die Anzahl der Regionen wird in Zukunft noch erweitert.
* **Unterstützte Betriebssysteme:** Microsoft Windows Server 2012 R2 und Windows Server 2016 Technical Preview 5. Die Unterstützung für Linux und Windows Server 2012 wird in Kürze hinzugefügt.
* **VM-Größe:** Es werden nur die VM-Instanzgrößen „Standard_D15_v2“ und „Standard_DS15_v2“ unterstützt. Weitere Informationen finden Sie im Artikel [Windows-VM-Größen](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) . Die Anzahl von unterstützten VM-Instanzgrößen wird in Zukunft erweitert werden.

Änderungen an diesen Einschränkungen werden auf der Seite [Azure-Updates für Virtual Network](https://azure.microsoft.com/updates/accelerated-networking-in-preview) bekannt gemacht.

## <a name="create-a-windows-vm-with-accelerated-networking"></a>Erstellen einer Windows-VM mit Accelerated Networking
1. Wenn Sie sich für die Vorschau registrieren möchten, senden Sie eine E-Mail an [Accelerated Networking Subscriptions (Abonnements für beschleunigten Netzwerkbetrieb)](mailto:axnpreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e), und geben Sie Ihre Abonnement-ID und die beabsichtigte Verwendung an. Führen Sie die verbleibenden Schritte erst aus, wenn Sie eine E-Mail-Benachrichtigung mit dem Hinweis erhalten haben, dass Sie für die Vorschauversion akzeptiert wurden.
2. Melden Sie sich unter „http://portal.azure.com“ beim Azure-Portal an.
3. Erstellen Sie eine VM, indem Sie die Schritte im Artikel [Erstellen Ihres ersten virtuellen Windows-Computers im Azure-Portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) ausführen und die folgenden Optionen auswählen:
   
   * Wählen Sie ein Betriebssystem aus dem Abschnitt „Einschränkungen“ in diesem Artikel.
   * Wählen Sie einen Standort (Region) aus dem Abschnitt „Einschränkungen“ in diesem Artikel.
   * Wählen Sie eine VM-Größe aus dem Abschnitt „Einschränkungen“ in diesem Artikel. Wenn eine der unterstützten Größen nicht aufgeführt ist, klicken Sie auf dem Blatt **Größe auswählen** auf **Alle anzeigen**, um eine Größe aus einer erweiterten Liste auszuwählen.
   * Klicken Sie auf dem Blatt **Einstellungen** für *Beschleunigter Netzwerkbetrieb* auf **Aktiviert**, wie in der folgenden Abbildung gezeigt:
     
       ![Einstellungen](./media/virtual-network-accelerated-networking-portal/image3.png)
     
     > [!NOTE]
     > Die Option „Beschleunigter Netzwerkbetrieb“ ist nur dann sichtbar, wenn Sie:
     > 
     > * in der Vorschau zugelassen wurde
     > * das unterstützte Betriebssystem, den Standort und die VM-Größen, die im Abschnitt „Einschränkungen“ dieses Artikels erwähnt wurden, ausgewählt haben
     > 
     > 
4. Laden Sie nach dem Erstellen des virtuellen Computers den [Treiber für den beschleunigten Netzwerkbetrieb](https://gallery.technet.microsoft.com/Azure-Accelerated-471b5d84) herunter, stellen Sie eine Verbindung mit dem virtuellen Computer her, melden Sie sich beim virtuellen Computer an, und führen Sie das Treiberinstallationsprogramm innerhalb des virtuellen Computers aus.
5. Klicken Sie mit der rechten Maustaste auf die Windows-Schaltfläche, und klicken Sie auf **Geräte-Manager**. Vergewissern Sie sich, dass der **Mellanox ConnectX-3 Virtual Function Ethernet Adapter** wie in der folgenden Abbildung gezeigt unterhalb der Option **Netzwerkadapter** angezeigt wird, wenn Sie diese erweitern:
   
    ![Geräte-Manager](./media/virtual-network-accelerated-networking-portal/image2.png)


