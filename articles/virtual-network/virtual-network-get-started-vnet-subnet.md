---
title: Erstellen Ihres ersten virtuellen Azure-Netzwerks | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie ein virtuellen Azure-Netzwerk (VNet) erstellen, zwei virtuelle Computer (Virtual Machines, VMs) mit dem VNet verbinden und eine Verbindung mit den virtuellen Computern herstellen.
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2016
ms.author: jdial
translationtype: Human Translation
ms.sourcegitcommit: cea53acc33347b9e6178645f225770936788f807
ms.openlocfilehash: 071ef9c0784bdb24c7d29f49c36aee2271f4b524
ms.lasthandoff: 03/03/2017


---

# <a name="create-your-first-virtual-network"></a>Erstellen Ihres ersten virtuellen Netzwerks

Hier erfahren Sie, wie Sie ein virtuelles Netzwerk (VNet) mit zwei Subnetzen sowie zwei virtuelle Computer (Virtual Machines, VMs) erstellen und jeden der virtuellen Computer mit einem der Subnetze verbinden, wie in der folgenden Abbildung zu sehen:

![Diagramm des virtuellen Netzwerks](./media/virtual-network-get-started-vnet-subnet/vnet-diagram.png)

Ein virtuelles Azure-Netzwerk (VNet) ist eine Darstellung Ihres eigenen Netzwerks in der Cloud. Sie können Ihre Azure-Netzwerkeinstellungen steuern und DHCP-Adressblöcke, DNS-Einstellungen, Sicherheitsrichtlinien sowie das Routing definieren. Weitere Informationen zu VNet-Konzepten finden Sie in der [Übersicht über virtuelle Netzwerke](virtual-networks-overview.md). Führen Sie die folgenden Schritte aus, um die in der Abbildung dargestellten Ressourcen zu erstellen:

1. [Erstellen eines VNets mit zwei Subnetzen](#create-vnet)
2. [Erstellen von zwei virtuellen Computern mit jeweils einer Netzwerkschnittstelle (NIC)](#create-vms) und Zuordnen einer Netzwerksicherheitsgruppe (NSG) zu den einzelnen NICs
3. [Herstellen ein- und ausgehender Verbindungen für die virtuellen Computer](#connect-to-from-vms)
4. [Löschen aller Ressourcen](#delete-resources) Für manche der Ressourcen, die in dieser Übung erstellt werden, fallen Bereitstellungsgebühren an. Führen Sie daher nach Abschluss der Übung unbedingt die Schritte in diesem Abschnitt aus, um die erstellten Ressourcen zu löschen und die Gebühren möglichst gering zu halten.

Die Schritte in diesem Artikel vermitteln ein Grundverständnis für die Verwendungsmöglichkeiten eines VNets. Unter „Nächste Schritte“ können Sie sich ausführlicher mit der Verwendung von VNets beschäftigen.

## <a name="create-vnet"></a>Erstellen eines virtuellen Netzwerks mit zwei Subnetzen

Führen Sie die folgenden Schritte aus, um ein virtuelles Netzwerk mit zwei Subnetzen zu erstellen. Die Verwendung verschiedener Subnetze dient in der Regel zur Steuerung des Datenverkehrsflusses zwischen Subnetzen.

1. Melden Sie sich beim [Azure-Portal](<https://portal.azure.com>)an. Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für eine [kostenlose einmonatige Testversion](https://azure.microsoft.com/free) registrieren. 
2. Klicken Sie im Portalbereich **Favoriten** auf **Neu**.
3. Klicken Sie auf dem Blatt **Neu** auf **Netzwerk**. Klicken Sie auf dem Blatt **Netzwerk** auf **Virtuelles Netzwerk**, wie in der folgenden Abbildung zu sehen:

    ![Diagramm des virtuellen Netzwerks](./media/virtual-network-get-started-vnet-subnet/virtual-network.png)

4.  Behalten Sie auf dem Blatt **Virtuelles Netzwerk** die Wahl des Bereitstellungsmodells *Resource Manager* bei, und klicken Sie anschließend auf **Erstellen**.
5.  Geben Sie auf dem Blatt **Virtuelles Netzwerk erstellen** die folgenden Werte ein, und klicken Sie anschließend auf **Erstellen**:

    |**Einstellung**|**Wert**|**Details**|
    |---|---|---|
    |**Name**|*MyVNet*|Der Name muss innerhalb der Ressourcengruppe eindeutig sein.|
    |**Adressraum**|*10.0.0.0/16*|Geben Sie einen beliebigen Adressraum in CIDR-Notation an.|
    |**Subnetzname**|*Front-End*|Der Subnetzname muss innerhalb des virtuellen Netzwerks eindeutig sein.|
    |**Subnetzadressbereich**|*10.0.0.0/24*| Der angegebene Bereich muss innerhalb des Adressraums liegen, den Sie für das virtuelle Netzwerk definiert haben.|
    |**Abonnement**|*[Ihr Abonnement]*|Wählen Sie ein Abonnement aus, in dem Sie das VNet erstellen möchten. Ein VNet befindet sich innerhalb eines einzelnen Abonnements.|
    |**Ressourcengruppe**|**Neu erstellen:** *MyRG*|Erstellen Sie eine Ressourcengruppe. Der Name der Ressourcengruppe muss innerhalb des ausgewählten Abonnements eindeutig sein. Weitere Informationen zu Ressourcengruppen finden Sie in der [Übersicht über Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-groups).|
    |**Standort**|*USA, Westen*| In der Regel wird der Standort ausgewählt, der Ihrem physischen Standort am nächsten ist.|

    Die Erstellung des VNets dauert ein paar Sekunden. Nach der Erstellung wird es auf dem Dashboard des Azure-Portals angezeigt.

6. Klicken Sie nach der Erstellung des virtuellen Netzwerks im Azure-Portal auf den Bereich **Favoriten** und anschließend auf **Alle Ressourcen**. Klicken Sie auf dem Blatt **Alle Ressourcen** auf das virtuelle Netzwerk **MyVNet**. Falls das ausgewählte Abonnement bereits mehrere Ressourcen enthält, können Sie *MyVNet* in das Feld **Nach Name filtern...** eingeben und komfortabel auf das VNet zugreifen.
7. Das Blatt **MyVNet** wird geöffnet und zeigt Informationen zum VNet, wie in der folgenden Abbildung zu sehen:

    ![Diagramm des virtuellen Netzwerks](./media/virtual-network-get-started-vnet-subnet/myvnet.png)

8. Klicken Sie wie in der vorherigen Abbildung dargestellt auf **Subnetze**, um eine Liste mit den Subnetzen des virtuellen Netzwerks anzuzeigen. Das einzige vorhandene Subnetz ist das in Schritt 5 erstellte **Front-End**.
9. Klicken Sie auf dem Blatt „MyVNet – Subnetze“ auf **+ Subnetz**, um ein Subnetz mit den folgenden Informationen zu erstellen, und klicken Sie anschließend auf **OK**, um das Subnetz zu erstellen:

    |**Einstellung**|**Wert**|**Details**|
    |---|---|---|
    |**Name**|*Back-End*|Der Name muss innerhalb des virtuellen Netzwerks eindeutig sein.|
    |**Adressbereich**|*10.0.1.0/24*|Der angegebene Bereich muss innerhalb des Adressraums liegen, den Sie für das virtuelle Netzwerk definiert haben.|
    |**Netzwerksicherheitsgruppe** und **Routingtabelle**|*Keine* (Standardeinstellung)|Netzwerksicherheitsgruppen (NSGs) werden weiter unten in diesem Artikel behandelt. Weitere Informationen zu benutzerdefinierten Routen finden Sie im Artikel [Benutzerdefinierte Routen und IP-Weiterleitung](virtual-networks-udr-overview.md).|

10. Wenn das neue Subnetz dem VNet hinzugefügt wurde, können Sie das Blatt **MyVNet – Subnetze** und anschließend das Blatt **Alle Ressourcen** schließen.

## <a name="create-vms"></a>Erstellen virtueller Computer

Nach der Erstellung des VNets und der Subnetze können Sie die virtuellen Computer erstellen. In dieser Übung wird auf beiden virtuellen Computern das Windows Server-Betriebssystem verwendet. Für die virtuellen Computer kann aber auch ein anderes, von Azure unterstütztes Betriebssystem verwendet werden (einschließlich verschiedener Linux-Distributionen).

### <a name="create-web-server-vm"></a>Erstellen des virtuellen Webservercomputers

Führen Sie die folgenden Schritte aus, um den virtuellen Webservercomputer zu erstellen:

1. Klicken Sie im Favoritenbereich des Azure-Portals auf **Neu** > **Compute** und anschließend auf **Windows Server 2016 Datacenter**.
2. Klicken Sie auf dem Blatt **Windows Server 2016 Datacenter** auf **Erstellen**.
3. Geben Sie auf dem daraufhin angezeigten Blatt **Grundlagen** die folgenden Werte ein (oder wählen Sie sie aus), und klicken Sie anschließend auf **OK**:

    |**Einstellung**| **Wert**|**Details**|
    |---|---|---|
    |**Name**|*MyWebServer*|Dieser virtuelle Computer fungiert als Webserver, mit dem Internetressourcen eine Verbindung herstellen.|
    |**VM-Datenträgertyp**|*SSD*|
    |**Benutzername**|*Beliebig*|
    |**Kennwort und Kennwortbestätigung**|*Beliebig*|
    | **Abonnement**|*<Your subscription>*|Hierbei muss es sich um das Abonnement handeln, das Sie in Schritt 5 des Abschnitts [Erstellen eines virtuellen Netzwerks mit zwei Subnetzen](#create-vnet) in diesem Artikel ausgewählt haben. Das VNet, mit dem Sie einen virtuellen Computer verbinden, muss sich im gleichen Abonnement befinden wie der virtuelle Computer.|
    |**Ressourcengruppe**|**Vorhandene verwenden:** Wählen Sie *MyRG* aus.|Wir verwenden zwar die gleiche Ressourcengruppe wie für das VNet, die Ressourcen müssen sich jedoch nicht in der gleichen Ressourcengruppe befinden.|
    |**Standort**|*USA, Westen*|Hierbei muss es sich um den Standort handeln, den Sie in Schritt 5 des Abschnitts [Erstellen eines virtuellen Netzwerks mit zwei Subnetzen](#create-vnet) in diesem Artikel angegeben haben. Virtuelle Computer und die VNets, mit denen sie eine Verbindung herstellen, müssen sich am gleichen Standort befinden.|

4. Klicken Sie auf dem Blatt **Größe auswählen** auf *DS1_V2 Standard* und anschließend auf **Auswählen**. Eine Liste mit allen von Azure unterstützten Größen für virtuelle Windows-Computer finden Sie im Artikel [Größen für virtuelle Windows-Computer in Azure](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
5. Geben Sie auf dem Blatt **Einstellungen** die folgenden Werte ein (oder wählen Sie sie aus), und klicken Sie anschließend auf **OK**:

    |**Einstellung**|**Wert**|**Details**|
    |---|---|---|
    |**Speicher: Verwaltete Datenträger verwenden**|*Ja*||
    |**Virtuelles Netzwerk**| Wählen Sie *MyVNet* aus.|Sie können ein beliebiges VNet auswählen, es muss sich jedoch am gleichen Standort befinden wie der virtuelle Computer, den Sie erstellen. Weitere Informationen zu VNets und Subnetzen finden Sie im [Artikel zu virtuellen Netzwerken](virtual-networks-overview.md).|
    |**Subnetz**|Wählen Sie *Front-End* aus.|Sie können ein beliebiges Subnetz auswählen, es muss sich jedoch innerhalb des virtuellen Netzwerks befinden.|
    |**Öffentliche IP-Adresse**|Übernehmen Sie die Standardeinstellung.|Mit einer öffentlichen IP-Adresse können Sie über das Internet eine Verbindung mit dem virtuellen Computer herstellen. Weitere Informationen zu öffentlichen IP-Adressen finden Sie im [Artikel zu IP-Adressen](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses).|
    |**Netzwerksicherheitsgruppe (Firewall)**|Übernehmen Sie die Standardeinstellung.|Klicken Sie auf die vom Portal erstellte Standard-NSG **(new) MyWebServer-nsg**, um die Einstellungen anzuzeigen. Auf dem daraufhin geöffneten Blatt **Netzwerksicherheitsgruppe erstellen** sehen Sie, dass eine einzelne Regel für eingehenden Datenverkehr vorhanden ist, die Datenverkehr vom Typ „TCP/3389 (RDP)“ von einer beliebigen Quell-IP-Adresse zulässt.|
    |**Alle anderen Werte**|Übernehmen Sie die Standardeinstellungen.|Weitere Informationen zu den restlichen Einstellungen finden Sie in der [Übersicht über virtuelle Windows-Computer in Azure](../virtual-machines/virtual-machines-windows-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).|

    Mithilfe von Netzwerksicherheitsgruppen (NSGs) können Sie Ein-/Ausgangsregeln für die Art von Netzwerkdatenverkehr erstellen, der an Ihre virtuellen Computer und von Ihren virtuellen Computern übermittelt werden kann. Standardmäßig wird jeglicher eingehende Datenverkehr für den virtuellen Computer abgelehnt. Für einen Produktionswebserver können zusätzliche Eingangsregeln für TCP/80 (HTTP) und TCP/443 (HTTPS) hinzugefügt werden. Für ausgehenden Datenverkehr ist keine Regel vorhanden, da ausgehender Datenverkehr standardmäßig zugelassen wird. Sie können Regeln zur Steuerung des Datenverkehrs gemäß Ihren Richtlinien hinzufügen/entfernen. Weitere Informationen finden Sie im Artikel [Netzwerksicherheitsgruppen](virtual-networks-nsg.md).

6.  Überprüfen Sie die Einstellungen auf dem Blatt **Zusammenfassung**, und klicken Sie anschließend auf **OK**, um den virtuellen Computer zu erstellen. Während der Erstellung des virtuellen Computers wird auf dem Dashboard des Portals eine Statuskachel angezeigt. Die Erstellung kann einige Minuten dauern. Sie müssen nicht warten, bis der Vorgang abgeschlossen ist. Stattdessen können Sie mit dem nächsten Schritt fortfahren, während der virtuelle Computer erstellt wird.

### <a name="create-database-server-vm"></a>Erstellen des virtuellen Datenbankservercomputers

Führen Sie die folgenden Schritte aus, um den virtuellen Datenbankservercomputer zu erstellen:

1.  Klicken Sie im Favoritenbereich auf **Neu** > **Compute** und anschließend auf **Windows Server 2016 Datacenter**.
2.  Klicken Sie auf dem Blatt **Windows Server 2016 Datacenter** auf **Erstellen**.
3.  Geben Sie auf dem Blatt **Grundlagen** die folgenden Werte ein (oder wählen Sie sie aus), und klicken Sie anschließend auf **OK**:

    |**Einstellung**|**Wert**|**Details**|
    |---|---|---|
    |**Name**|*MyDBServer*|Dieser virtuelle Computer fungiert als Datenbankserver, mit dem der Webserver eine Verbindung herstellt. Eine Internetverbindung kann damit jedoch nicht hergestellt werden.|
    |**VM-Datenträgertyp**|*SSD*||
    |**Benutzername**|Beliebig||
    |**Kennwort und Kennwortbestätigung**|Beliebig||
    |**Abonnement**|<Your subscription>|Hierbei muss es sich um das Abonnement handeln, das Sie in Schritt 5 des Abschnitts [Erstellen eines virtuellen Netzwerks mit zwei Subnetzen](#create-vnet) in diesem Artikel ausgewählt haben.|
    |**Ressourcengruppe**|**Vorhandene verwenden:** Wählen Sie *MyRG* aus.|Wir verwenden zwar die gleiche Ressourcengruppe wie für das VNet, die Ressourcen müssen sich jedoch nicht in der gleichen Ressourcengruppe befinden.|
    |**Standort**|*USA, Westen*|Hierbei muss es sich um den Standort handeln, den Sie in Schritt 5 des Abschnitts [Erstellen eines virtuellen Netzwerks mit zwei Subnetzen](#create-vnet) in diesem Artikel angegeben haben.|

4.  Klicken Sie auf dem Blatt **Größe auswählen** auf *DS1_V2 Standard* und anschließend auf **Auswählen**.
5.  Geben Sie auf dem Blatt **Einstellungen** die folgenden Werte ein (oder wählen Sie sie aus), und klicken Sie anschließend auf **OK**:

    |**Einstellung**|**Wert**|**Details**|
    |----|----|---|
    |**Speicher: Verwaltete Datenträger verwenden**|*Ja*||
    |**Virtuelles Netzwerk**|Wählen Sie *MyVNet* aus.|Sie können ein beliebiges VNet auswählen, es muss sich jedoch am gleichen Standort befinden wie der virtuelle Computer, den Sie erstellen.|
    |**Subnetz**|Wählen Sie *Back-End* aus. Klicken Sie hierzu auf das Feld **Subnetz**, und wählen Sie anschließend auf dem Blatt **Wählen Sie ein Subnetz aus.** die Option **Back-End** aus.|Sie können ein beliebiges Subnetz auswählen, es muss sich jedoch innerhalb des virtuellen Netzwerks befinden.|
    |**Öffentliche IP-Adresse**|Keine. Klicken Sie auf die Standardadresse und anschließend auf dem Blatt **Öffentliche IP-Adresse wählen** auf **Keine**.|Ohne öffentliche IP-Adresse kann mit dem virtuellen Computer nur eine Verbindung über einen anderen virtuellen Computer im gleichen VNet hergestellt werden. Eine direkte Verbindung über das Internet ist nicht möglich.|
    |**Netzwerksicherheitsgruppe (Firewall)**|Übernehmen Sie die Standardeinstellung.| Diese NSG verfügt über die gleiche standardmäßige Eingangsregel wie die Standard-NSG, die für den virtuellen Computer „MyWebServer“ erstellt wurde. Für einen Datenbankserver kann eine weitere Eingangsregel für TCP/1433 (MS-SQL) hinzugefügt werden. Für ausgehenden Datenverkehr ist keine Regel vorhanden, da ausgehender Datenverkehr standardmäßig zugelassen wird. Sie können Regeln zur Steuerung des Datenverkehrs gemäß Ihren Richtlinien hinzufügen/entfernen.|
    |**Alle anderen Werte**|Übernehmen Sie die Standardeinstellungen.||

6.  Überprüfen Sie die Einstellungen auf dem Blatt **Zusammenfassung**, und klicken Sie anschließend auf **OK**, um den virtuellen Computer zu erstellen. Während der Erstellung des virtuellen Computers wird auf dem Dashboard des Portals eine Statuskachel angezeigt. Die Erstellung kann einige Minuten dauern. Sie müssen nicht warten, bis der Vorgang abgeschlossen ist. Stattdessen können Sie mit dem nächsten Schritt fortfahren, während der virtuelle Computer erstellt wird.

## <a name="review"></a>Überprüfen der Ressourcen

Sie haben zwar ein VNet und zwei virtuelle Computer erstellt, das Azure-Portal hat in der Ressourcengruppe „MyRG“ jedoch noch einige weitere Ressourcen für Sie erstellt. Führen Sie die folgenden Schritte aus, um den Inhalt der Ressourcengruppe „MyRG“ zu überprüfen:

1. Klicken Sie im Bereich **Favoriten** auf **Weitere Dienste**.
2. Geben Sie im Bereich **Weitere Dienste** die Zeichenfolge *Ressourcengruppen* in das Feld *Filter* ein. Klicken Sie auf **Ressourcengruppen**, wenn die Option in der gefilterten Liste erscheint.
3. Klicken Sie im Bereich **Ressourcengruppen** auf die Ressourcengruppe *MyRG*. Falls in Ihrem Abonnement bereits viele Ressourcengruppen vorhanden sind, können Sie die Zeichenfolge *MyRG* in das Feld *Nach Name filtern...* eingeben, um schnell auf die Ressourcengruppe „MyRG“ zuzugreifen.
4.  Auf dem Blatt **MyRG** sehen Sie, dass die Ressourcengruppe 12 Ressourcen enthält, wie in der folgenden Abbildung gezeigt:

    ![Inhalt der Ressourcengruppe](./media/virtual-network-get-started-vnet-subnet/resource-group-contents.png)

Weitere Informationen zu virtuellen Computern, Datenträgern und Speicherkonten finden Sie in den Artikeln [Übersicht über virtuelle Windows-Computer in Azure](../virtual-machines/virtual-machines-windows-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [Informationen zu Datenträgern und VHDs für Azure-Windows-VMs](../storage/storage-about-disks-and-vhds-windows.md?toc=%2fazure%2fvirtual-network%2ftoc.json) und [Einführung in Microsoft Azure Storage](../storage/storage-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Sie sehen die beiden Standard-NSGs, die das Portal für Sie erstellt hat. Außerdem sehen Sie, dass das Portal zwei Netzwerkschnittstellen (NICs) als Ressourcen erstellt hat. Mithilfe einer Netzwerkschnittstelle kann ein virtueller Computer über das VNet eine Verbindung mit anderen Ressourcen herstellen. Weitere Informationen zu Netzwerkschnittstellen finden Sie im Artikel [Was sind Netzwerkschnittstellen?](virtual-network-network-interface-overview.md). Das Portal hat auch eine öffentliche IP-Adressressource erstellt. Bei öffentlichen IP-Adressen handelt es sich um eine Einstellung für eine öffentliche IP-Adressressource. Weitere Informationen zu öffentlichen IP-Adressen finden Sie im [Artikel zu IP-Adressen](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses).

## <a name="connect-to-from-vms"></a>Herstellen einer Verbindung mit den virtuellen Computern

Nachdem Sie Ihr VNet und die beiden virtuellen Computer erstellt haben, können Sie die Schritte in den folgenden Abschnitten ausführen, um eine Verbindung mit den virtuellen Computern herzustellen:

### <a name="connect-from-internet"></a>Herstellen einer Verbindung mit dem virtuellen Webservercomputer über das Internet

Führen Sie die folgenden Schritte aus, um über das Internet eine Verbindung mit dem virtuellen Webservercomputer herzustellen:

1. Öffnen Sie im Portal die Ressourcengruppe „MyRG“, wie im Abschnitt [Überprüfen der Ressourcen](#review) dieses Artikels beschrieben.
2. Klicken Sie auf dem Blatt **MyRG** auf den virtuellen Computer **MyWebServer**.
3. Klicken Sie auf dem Blatt **MyWebServer** auf **Verbinden**, wie in der folgenden Abbildung gezeigt:

    ![Herstellen einer Verbindung mit der virtuellen Webservercomputer](./media/virtual-network-get-started-vnet-subnet/webserver.png)

4. Lassen Sie in Ihrem Browser das Herunterladen der Datei *MyWebServer.rdp* zu, und öffnen Sie sie.
5. Wenn ein Dialogfeld mit dem Hinweis erscheint, dass der Herausgeber der Remoteverbindung nicht überprüft werden kann, klicken Sie auf **Verbinden**.
6. Verwenden Sie bei der Anmeldung den Benutzernamen und das Kennwort, die Sie in Schritt 3 des Abschnitts [Erstellen des virtuellen Webservercomputers](#create-web-server-vm) in diesem Artikel angegeben haben. (Sollten im angezeigten Feld vom Typ **Windows-Sicherheit** nicht die korrekten Anmeldeinformationen enthalten sein, müssen Sie ggf. auf **More choices** (Weitere Optionen) und anschließend auf **Anderes Konto verwenden** klicken und den korrekten Benutzernamen und das korrekte Kennwort angeben.) Klicken Sie auf **OK**, um die Verbindung mit dem virtuellen Computer herzustellen.
7. Falls ein Feld vom Typ **Remotedesktopverbindung** mit dem Hinweis erscheint, dass die Identität des Remotecomputers nicht überprüft werden kann, klicken Sie auf **Ja**.
8. Sie sind nun über das Internet mit dem virtuellen Computer „MyWebServer“ verbunden. Lassen Sie die Remotedesktopverbindung für die Schritte im nächsten Abschnitt geöffnet.

Die Remoteverbindung besteht mit der öffentlichen IP-Adresse, die der öffentlichen IP-Adressressource im Portal zugewiesen ist, welche in Schritt 5 des Abschnitts [Erstellen eines virtuellen Netzwerks mit zwei Subnetzen](#create-vnet) in diesem Artikel erstellt wurde. Die Verbindung wird zugelassen, da die in der NSG **MyWebServer-nsg** erstellte Standardregel für den virtuellen Computer Datenverkehr vom Typ „TCP/3389 (RDP)“ von beliebigen Quell-IP-Adressen zulässt. Wenn Sie über einen anderen Port eine Verbindung mit dem virtuellen Computer herstellen möchten, ist dies nur erfolgreich, wenn Sie der NSG weitere Eingangsregeln hinzufügen, die die zusätzlichen Ports zulassen.

>[!NOTE]
>Falls Sie der NSG weitere Eingangsregeln hinzufügen, achten Sie darauf, dass die gleichen Ports auch in der Windows-Firewall geöffnet sind. Andernfalls kann die Verbindung nicht hergestellt werden.
>

### <a name="connect-to-internet"></a>Herstellen einer Internetverbindung über den virtuellen Webservercomputer

Führen Sie die folgenden Schritte aus, um über den virtuellen Webservercomputer eine ausgehende Internetverbindung herzustellen:

1. Falls Sie noch keine Remoteverbindung mit dem virtuellen Computer „MyWebServer“ geöffnet haben, stellen Sie mithilfe der Schritte des Abschnitts [Herstellen einer Verbindung mit dem virtuellen Webservercomputer über das Internet](#connect-from-internet) in diesem Artikel eine Remoteverbindung mit dem virtuellen Computer her.
2. Öffnen Sie Internet Explorer (über den Windows-Desktop). Klicken Sie im Dialogfeld **Internet Explorer 11 einrichten** auf **Empfohlene Einstellungen nicht verwenden** und anschließend auf **OK**. Bei einem Produktionsserver empfiehlt es sich, die empfohlenen Einstellungen zu akzeptieren.
3. Geben Sie auf der Adressleiste von Internet Explorer [bing.com](http:www.bing.com) ein. Sollte ein Internet Explorer-Dialogfeld angezeigt werden, klicken Sie auf **Hinzufügen**, klicken Sie im Dialogfeld **Vertrauenswürdige Sites** auf **Hinzufügen**, und klicken Sie anschließend auf **Schließen**. Wiederholen Sie diesen Vorgang für alle anderen Dialogfelder von Internet Explorer.
4. Geben Sie auf der Suchseite von Bing die Zeichenfolge *whatsmyipaddress* ein, und klicken Sie anschließend auf die Lupenschaltfläche. Bing gibt die öffentliche IP-Adresse zurück, die der öffentlichen IP-Adressressource zugewiesen ist, die das Portal bei der Erstellung des virtuellen Computers erstellt hat. In den Einstellungen für die Ressource **MyWebServer-ip** sehen Sie, dass der öffentlichen IP-Adressressource die gleiche IP-Adresse zugewiesen ist, wie in der folgenden Abbildung gezeigt. Ihrem virtuellen Computer ist dagegen eine andere IP-Adresse zugewiesen.

    ![Herstellen einer Verbindung mit der virtuellen Webservercomputer](./media/virtual-network-get-started-vnet-subnet/webserver-pip.png)

5.  Lassen Sie die Remotedesktopverbindung für die Schritte im nächsten Abschnitt geöffnet.

Sie können von dem virtuellen Computer aus eine Internetverbindung herstellen, da standardmäßig alle ausgehenden Verbindungen des virtuellen Computers zugelassen werden. Zum Einschränken ausgehender Verbindungen können Sie der NSG, die auf die Netzwerkschnittstelle angewendet wurde, und/oder dem Subnetz, mit dem die Netzwerkschnittstelle verbunden ist, weitere Regeln hinzufügen.

Wenn der virtuelle Computer über das Portal beendet (also seine Zuordnung aufgehoben) wird, ändert sich unter Umständen die öffentliche IP-Adresse. Falls die öffentliche IP-Adresse unverändert bleiben muss, verwenden Sie anstelle der dynamischen Standardmethode die statische Zuordnung für die IP-Adresse. Weitere Informationen zu den Unterschieden zwischen Zuordnungsmethoden finden Sie im Artikel [IP-Adresstypen und Zuordnungsmethoden in Azure](virtual-network-ip-addresses-overview-arm.md).

### <a name="webserver-to-dbserver"></a>Herstellen einer Verbindung mit dem virtuellen Datenbankservercomputer über den virtuellen Webservercomputer

Führen Sie die folgenden Schritte aus, um über den virtuellen Webservercomputer eine Verbindung mit dem virtuellen Datenbankservercomputer herzustellen:

1. Falls Sie noch keine Remoteverbindung mit dem virtuellen Computer „MyWebServer“ geöffnet haben, stellen Sie mithilfe der Schritte des Abschnitts [Herstellen einer Verbindung mit dem virtuellen Webservercomputer über das Internet](#connect-from-internet) in diesem Artikel eine Remoteverbindung mit dem virtuellen Computer her.
2. Klicken Sie links unten auf dem Windows-Desktop auf die Schaltfläche „Start“, und beginnen Sie mit der Eingabe des Begriffs *Remotedesktop*. Wenn in der Liste des Startmenüs die Option **Remotedesktopverbindung** angezeigt wird, klicken Sie darauf.
3. Geben Sie im Dialogfeld **Remotedesktopverbindung** den Computernamen *MyDBServer* ein, und klicken Sie anschließend auf **Verbinden**.
4. Geben Sie den Benutzernamen und das Kennwort ein, die Sie in Schritt 3 des Abschnitts [Erstellen des virtuellen Datenbankservercomputers](#create-database-server-vm) in diesem Artikel eingegeben haben, und klicken Sie anschließend auf **OK**.
5. Falls ein Dialogfeld mit dem Hinweis erscheint, dass die Identität des Remotecomputers nicht überprüft werden kann, klicken Sie auf **Ja**.
6. Lassen Sie die Remotedesktopverbindung mit beiden Servern für die Schritte im nächsten Abschnitt geöffnet.

Über den virtuellen Webservercomputer kann aus folgenden Gründen eine Verbindung mit dem virtuellen Datenbankservercomputer hergestellt werden:

- Durch die Standard-NSG, die in Schritt 5 des Abschnitts [Erstellen des virtuellen Datenbankservercomputers](#create-database-server-vm) in diesem Artikel erstellt wurde, werden eingehende TCP/3389-Verbindungen für beliebige Quell-IP-Adressen ermöglicht.
- Sie haben die Verbindung über den virtuellen Webservercomputer initiiert. Dieser ist mit dem gleichen VNet verbunden wie der virtuelle Datenbankservercomputer. Wenn Sie eine Verbindung mit einem virtuellen Computer herstellen möchten, dem keine öffentliche IP-Adresse zugewiesen ist, müssen Sie die Verbindung über einen anderen virtuellen Computer herstellen, der mit dem gleichen VNet verbunden ist, auch wenn der virtuelle Computer mit einem anderen Subnetz verbunden ist.
- Azure erstellt Standardrouten, die Verbindungen zwischen Subnetzen ermöglichen. Sie können die Standardrouten allerdings außer Kraft setzen und eigene Routen erstellen. Weitere Informationen zum Routing in Azure finden Sie im Artikel [Benutzerdefinierte Routen und IP-Weiterleitung](virtual-networks-udr-overview.md).

Wenn Sie versuchen, über das Internet eine Remoteverbindung mit dem virtuellen Datenbankservercomputer zu initiieren (wie im Abschnitt [Herstellen einer Verbindung mit dem virtuellen Webservercomputer über das Internet](#connect-from-internet) dieses Artikels), ist die Option **Verbinden** ausgegraut. Die Option ist ausgegraut, da dem virtuellen Computer keine öffentliche IP-Adresse zugewiesen ist und somit keine eingehenden Verbindungen über das Internet möglich sind.

### <a name="connect-to-the-internet-from-the-database-server-vm"></a>Herstellen einer Internetverbindung über den virtuellen Datenbankservercomputer

Führen Sie die folgenden Schritte aus, um über den virtuellen Datenbankservercomputer eine ausgehende Internetverbindung herzustellen:

1. Falls Sie noch keine Remoteverbindung mit dem virtuellen Computer „MyDBServer“ geöffnet haben, führen Sie die Schritte des Abschnitts [Herstellen einer Verbindung mit dem virtuellen Datenbankservercomputer über den virtuellen Webservercomputer](#webserver-to-dbserver) in diesem Artikel aus.
2. Öffnen Sie Internet Explorer (über den Windows-Desktop des virtuellen Computers „MyDBServer“), und gehen Sie bei den Dialogfeldern genauso vor wie in den Schritten 2 und 3 des Abschnitts [Herstellen einer Internetverbindung über den virtuellen Webservercomputer](#connect-to-internet) in diesem Artikel beschrieben.
3. Geben Sie auf der Adressleiste [bing.com](http:www.bing.com) ein.
4. Klicken Sie im angezeigten Internet Explorer-Dialogfeld auf **Hinzufügen**und im Dialogfeld **Vertrauenswürdige Sites** auf **Hinzufügen** und anschließend auf **Schließen**. Führen Sie die gleichen Schritte in allen weiteren Dialogfeldern aus, die ggf. angezeigt werden.
5. Geben Sie auf der Suchseite von Bing die Zeichenfolge *whatsmyipaddress* ein, und klicken Sie anschließend auf die Lupenschaltfläche. Bing gibt die öffentliche IP-Adresse zurück, die dem virtuellen Computer derzeit in der Azure-Infrastruktur zugewiesen ist. 6. Schließen Sie die Remotedesktopverbindung mit dem virtuellen Computer „MyDBServer“ über den virtuellen Computer „MyWebServer“, und schließen Sie anschließend die Remoteverbindung mit dem virtuellen Computer „MyWebServer“.

Die ausgehende Internetverbindung wird zugelassen, da standardmäßig jeglicher ausgehende Datenverkehr zugelassen wird, auch wenn dem virtuellen Computer „MyDBServer“ keine öffentliche IP-Adressressource zugewiesen ist. Jeder virtuelle Computer kann standardmäßig eine ausgehende Verbindung mit dem Internet herstellen. Dabei spielt es keine Rolle, ob ihm eine öffentliche IP-Adressressource zugewiesen ist. Sie können über das Internet aber keine Verbindung mit der öffentlichen IP-Adresse herstellen, wie dies bei dem virtuellen Computer „MyWebServer“ möglich war, dem eine öffentliche IP-Adressressource zugewiesen ist.

## <a name="delete-resources"></a>Löschen aller Ressourcen

Führen Sie die folgenden Schritte aus, um alle Ressourcen zu löschen, die in diesem Artikel erstellt wurden:

1. Führen Sie die Schritte 1 bis 3 des Abschnitts [Überprüfen der Ressourcen](#review) in diesem Artikel aus, um die in diesem Artikel erstellte Ressourcengruppe „MyRG“ anzuzeigen. Sehen Sie sich noch einmal die Ressourcen in der Ressourcengruppe an. Sofern Sie die Ressourcengruppe „MyRG“ gemäß den beschriebenen Schritten erstellt haben, werden die 12 Ressourcen aus der Abbildung zu Schritt 4 angezeigt.
2. Klicken Sie auf dem Blatt „MyRG“ auf die Schaltfläche **Löschen**.
3. Zur Bestätigung des Löschvorgangs werden Sie vom Portal zur Eingabe des Ressourcengruppennamens aufgefordert. Sollte es sich bei den angezeigten Ressourcen nicht um Ressourcen handeln, die in Schritt 4 des Abschnitts [Überprüfen der Ressourcen](#review) in diesem Artikel zu sehen sind, klicken Sie auf **Abbrechen**. Sofern nur die 12 Ressourcen vorhanden sind, die Sie im Rahmen dieses Artikels erstellt haben, geben Sie den Ressourcengruppennamen *MyRG* ein, und klicken Sie anschließend auf **Löschen**. Da beim Löschen einer Ressourcengruppe alle Ressourcen innerhalb der Ressourcengruppe gelöscht werden, überprüfen Sie vor dem Löschen immer den Inhalt der Ressourcengruppe. Das Portal löscht zuerst alle in der Ressourcengruppe enthaltenen Ressourcen und anschließend die Ressourcengruppe selbst. Dieser Vorgang dauert einige Minuten.

## <a name="next-steps"></a>Nächste Schritte

In dieser Übung haben Sie ein VNet und zwei virtuelle Computer erstellt. Bei der Erstellung der virtuellen Computer haben Sie einige benutzerdefinierte Einstellungen angegeben und mehrere Standardeinstellungen akzeptiert. Bevor Sie allerdings VNets und virtuelle Computer für die Produktion bereitstellen, sollten Sie sich in den folgenden Artikeln mit allen verfügbaren Einstellungen vertraut machen:

- [Virtuelle Netzwerke](virtual-networks-overview.md)
- [Öffentliche IP-Adressen](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses)
- [Netzwerkschnittstellen](virtual-network-network-interface-overview.md)
- [Netzwerksicherheitsgruppen](virtual-networks-nsg.md)
- [Virtuelle Computer](../virtual-machines/virtual-machines-windows-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)

