---
title: Mehrere IP-Adressen für virtuelle Computer – PowerShell | Microsoft Docs
description: Informationen zum Zuweisen von mehreren IP-Adressen zu einem virtuellen Computer mit Azure PowerShell.
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: ''
tags: azure-resource-manager

ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/05/2016
ms.author: jdial;annahar

---
# <a name="assign-multiple-ip-addresses-to-virtual-machines"></a>Zuweisen von mehreren IP-Adressen zu virtuellen Computern
Ein virtueller Azure-Computer (VM) kann über eine oder mehrere angefügte Netzwerkschnittstellen (NIC) verfügen. Jede NIC kann eine oder mehrere zugewiesene öffentliche oder private IP-Adressen aufweisen. Wenn Sie nicht mit IP-Adressen in Azure vertraut sind, finden Sie weitere Informationen im Artikel [IP-Adressen in Azure](virtual-network-ip-addresses-overview-arm.md) . In diesem Artikel wird erläutert, wie mit Azure PowerShell einer NIC im Azure Resource Manager-Bereitstellungsmodell mehrere IP-Adressen zugewiesen werden.

Durch das Zuweisen von mehreren IP-Adressen zu einer NIC bestehen für den virtuellen Computer folgende Möglichkeiten:

* Hosten mehrerer Websites oder Dienste mit unterschiedlichen IP-Adressen und SSL-Zertifikaten auf einem einzelnen Server
* Übernehmen der Rolle als virtuelles Netzwerkgerät, z.B. als Firewall oder Load Balancer

[!INCLUDE [virtual-network-preview](../../includes/virtual-network-preview.md)]

Wenn Sie sich für die Vorschau registrieren möchten, senden Sie eine E-Mail an [Mehrere IPs](mailto:MultipleIPsPreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) , und geben Sie Ihre Abonnement-ID und die beabsichtigte Verwendung an.

## <a name="scenario"></a>Szenario
In diesem Artikel ordnen Sie drei IP-Konfigurationen einer Netzwerkschnittstelle zu.
Dabei werden die folgenden Beispielkonfigurationen erstellt und einer NIC zugewiesen, der wiederum drei private IP-Adressen und eine öffentliche IP-Adresse zugewiesen werden:

* IPConfig-1: Eine dynamische private IP-Adresse (Standard) und eine öffentliche IP-Adresse aus der öffentlichen IP-Adressressource namens „PIP1“.
* IPConfig-2: Eine statische private IP-Adresse und keine öffentliche IP-Adresse.
* IPConfig-3: Eine dynamische private IP-Adresse und keine öffentliche IP-Adresse.
  
    ![Alternativer Bildtext](./media/virtual-network-multiple-ip-addresses-powershell/OneNIC-3IP.png)

In diesem Szenario wird vorausgesetzt, dass Sie über eine Ressourcengruppe namens *RG1* verfügen, die ein VNET namens *VNet1* und ein Subnetz namens *Subnet1* enthält. Darüber hinaus wird vorausgesetzt, dass Sie über einen virtuellen Computer namens *VM1*, über eine diesem virtuellen Computer zugeordnete Netzwerkschnittstelle namens *VM1-NIC1* und über eine öffentliche IP-Adresse namens *PIP1* verfügen.

Eine Anleitung zum Erstellen der obigen Ressourcen finden Sie in [diesem Artikel](../virtual-machines/virtual-machines-windows-ps-create.md).

## <a name="<a-name-=-"create"></a>create-a-vm-with-multiple-ip-addresses"></a><a name = "create"></a>Erstellen eines virtuellen Computers mit mehreren IP-Adressen
1. Öffnen Sie eine PowerShell-Eingabeaufforderung, und führen Sie die restlichen Schritte in diesem Abschnitt innerhalb einer einzelnen PowerShell-Sitzung aus. Wenn Sie PowerShell nicht installiert und konfiguriert haben, befolgen Sie die Anweisungen im Artikel [Gewusst wie: Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) .
2. Legen Sie die Werte der folgenden Variablen auf den Azure-[Standort](https://azure.microsoft.com/regions), an dem sich Ihr virtuelles Netzwerk befindet, auf den Namen Ihrer [Ressourcengruppe](../resource-group-overview.md#resource-groups), auf das VNET innerhalb der Ressourcengruppe, auf das Subnetz, mit dem Sie die NIC verbinden möchten, und auf den Namen der NIC fest.
   
        $Location = "westcentralus"
        $RgName   = "RG1"
        $VNetName   = "VNet1"
        $SubnetName = "Subnet1"
        $NicName     = "VM1-NIC1"
        $PIP = "PIP"
   
    Geben Sie die folgenden Befehle ein, wenn Sie den Namen eines vorhandenen Azure-Standorts oder einer vorhandenen Ressourcengruppe nicht kennen:
   
        Get-AzureRmLocation      | Format-Table Location
        Get-AzureRmResourceGroup | Format-Table ResourceGroupName
   
    <a name="subnet"></a>Die NIC muss über eine Verbindung mit einem Subnetz innerhalb einer vorhandenen Azure Virtual Network-Instanz (VNET) verfügen. Die drei Komponenten (NIC, Subnetz und VNET) müssen sich alle in der gleichen Region und im gleichen [Abonnement](../azure-glossary-cloud-terminology.md#subscription) befinden.  Wenn Sie noch nicht mit VNETs vertraut sind, finden Sie weitere allgemeine Informationen im Artikel [Übersicht über virtuelle Netzwerke](virtual-networks-overview.md). Informationen zum Erstellen von VNETs finden Sie im Artikel [Erstellen eines virtuellen Netzwerks](virtual-networks-create-vnet-arm-ps.md).
   
    Wenn Sie den Namen eines vorhandenen VNET nicht kennen, geben Sie den folgenden Befehl ein, und ersetzen Sie dabei *VNet1* in der vorherigen Variablen durch den Namen eines VNET:
   
        Get-AzureRmVirtualNetwork | Format-Table Name
   
    Wenn die zurückgegebene Liste leer ist, müssen Sie ein VNET erstellen. Informationen dazu finden Sie im Artikel [Erstellen eines virtuellen Netzwerks über PowerShell](virtual-networks-create-vnet-arm-ps.md) .
   
    Geben Sie die folgenden Befehle ein, um die Namen der im VNET vorhandenen Subnetze abzurufen. Ersetzen Sie dabei *Subnet1* oben durch den Namen eines Subnetzes:
   
        $VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RgName
        $VNet.Subnets | Format-Table Name, AddressPrefix
3. Geben Sie den folgenden Befehl ein, um das Subnetz abzurufen, und weisen Sie es einer Variablen zu.
   
        $Subnet = $VNet.Subnets | Where-Object { $_.Name -eq $SubnetName }
4. <a name="ipconfigs"></a>Definieren Sie die IP-Konfigurationen, die Sie der NIC zuweisen möchten. Jede Konfiguration kann eine statische oder dynamische private IP-Adresse und eine zugeordnete öffentliche IP-Adressressource mit einer statischen oder dynamischen Adresse enthalten.
   
    Sie können eine beliebige Anzahl der folgenden Konfigurationen hinzufügen oder entfernen, je nachdem, wie viele IP-Adressen Sie der NIC zuordnen und welche Einstellungen Sie konfigurieren möchten.
   
    **IPConfig-1**
   
    Ändern Sie den Wert *PIP1* in den Namen einer vorhandenen öffentlichen IP-Adressressource, die an dem Standort, in dem Sie die NIC erstellen, vorhanden und zurzeit keiner anderen NIC zugeordnet ist. Ändern Sie *RG1* in den Namen der Ressourcengruppe, in der die öffentliche IP-Adressressource vorhanden ist. Ändern Sie *IPConfig-1* in den gewünschten Namen der ersten IP-Konfiguration. Geben Sie die folgenden Befehle ein:
   
        $PIP1 = Get-AzureRmPublicIPAddress -Name "PIP1" -ResourceGroupName $RgName
   
        $IpConfigName1 = "IPConfig-1"
        $IPConfig1     = New-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName1 -Subnet $Subnet -PublicIpAddress $PIP1 -Primary
   
    Beachten Sie den Schalter *-Primary* . Wenn Sie einer NIC mehrere IP-Konfigurationen zuweisen, muss eine Konfiguration mit dem Wert *Primary*zugewiesen werden. Geben Sie den folgenden Befehl ein, wenn Sie den Namen einer vorhandenen öffentlichen IP-Adressressource nicht kennen:
   
        Get-AzureRMPublicIPAddress |Format-Table Name, Location, IPAddress, IpConfiguration
   
    Wenn die Spalte **IPConfiguration** in der zurückgegebenen Ausgabe keinen Wert enthält, ist die öffentliche IP-Adressressource keiner vorhandenen NIC zugeordnet und kann daher verwendet werden. Wenn die Liste leer ist oder keine öffentlichen IP-Adressressourcen verfügbar sind, können Sie eine mit dem Befehl **New-AzureRmPublicIPAddress** erstellen.
   
   > [!NOTE]
   > Für öffentliche IP-Adressen fällt eine geringe Gebühr an. Weitere Informationen zu den Preisen finden Sie auf der Seite [Preise für IP-Adressen](https://azure.microsoft.com/pricing/details/ip-addresses) .
   > 
   > 
   
    **IPConfig-2**
   
    Ändern Sie *IPConfig-2* in den gewünschten Namen für die zweite IP-Konfiguration und *10.0.0.5* in eine nicht verwendete gültige IP-Adresse für das Subnetz, dem Sie die NIC zuweisen. Geben Sie die folgenden Befehle ein:
   
        $IPConfigName2 = "IPConfig-2"
        $IPAddress = 10.0.0.5
   
        $IPConfig2 = New-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName2 -Subnet $Subnet -PrivateIpAddress $IPAddress
   
    Geben Sie den folgenden Befehl ein, wenn Sie den dem Subnetz zugewiesenen IP-Adressbereich nicht kennen:
   
        $VNet.Subnets | Format-Table Name, AddressPrefix
   
    **IPConfig-3**
   
    Ändern Sie *IPConfig-3* in den gewünschten Namen der dritten IP-Konfiguration, und geben Sie dann die folgenden Befehle ein:
   
        $IPConfigName3 = "IPConfig-3"
        $IPConfig3 = New-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName3 -Subnet $Subnet
   
   > [!NOTE]
   > Sie können einer NIC bis zu 250 private IP-Adressen zuweisen. Die Anzahl der öffentlichen IP-Adressen, die in einem Abonnement verwendet werden können, ist beschränkt. Weitere Informationen finden Sie im Artikel zu den [Azure-Einschränkungen](../azure-subscription-service-limits.md#networking-limits---azure-resource-manager) .
   > 
   > 
5. Erstellen Sie die NIC mit den im vorherigen Schritt definierten IP-Adresskonfigurationen.
   
        $nic = New-AzureRmNetworkInterface -Name $NicName -ResourceGroupName $RgName -Location $Location -IpConfiguration $IpConfig1,$IpConfig2,$IpConfig3
6. Fügen Sie die NIC beim Erstellen eines virtuellen Computers an. Führen Sie dazu die entsprechenden Schritte aus dem Artikel [Erstellen einer Windows-VM mit dem Resource Manager und PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md) aus. Obwohl in dem Artikel ein virtueller Computer unter Windows Server erstellt wird, sind die Schritte für einen virtuellen Linux-Computer identisch, mit der einzigen Ausnahme, dass ein anderes Betriebssystem ausgewählt wird. Führen Sie die Schritte 1 bis 3 des Artikels aus. Überspringen Sie die Schritte 4 und 5, und führen Sie dann Schritt 6 aus.
   
   > [!WARNING]
   > Schritt 6 des Artikels „Erstellen einer Windows-VM mit dem Resource Manager und PowerShell“ schlägt fehl, wenn Sie die Variable „$nic“ in Schritt 6 des Artikels in einen anderen Wert geändert haben oder die vorherigen Schritte des Artikels nicht ausgeführt haben.
   > 
   > 
7. Geben Sie den folgenden Befehl ein, um die privaten IP-Adressen, die der NIC über Azure-DHCP zugewiesen wurden, und die der NIC zugewiesene öffentliche IP-Adressressource anzuzeigen:
   
        $nic.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary
8. <a name="os"></a>Fügen Sie alle sekundären privaten IP-Adressen (IP-Adressen mit *False* in der Spalte **Primary** aus der Ausgabe im vorherigen Schritt) manuell in der TCP/IP-Konfiguration im Betriebssystem hinzu. Die private IP-Adresse, die in Schritt 5 *IPConfig-1* zugewiesen wurde, wird dem Betriebssystem über Azure-DHCP automatisch zugewiesen, da es sich um die als *Primary* definierte Konfiguration handelt.

**Windows**

1. Geben Sie an einer Eingabeaufforderung den Befehl *ipconfig /all*ein.  Sie können nur die *primäre* private Adresse anzeigen (über DHCP).
2. Geben Sie als Nächstes *ncpa.cpl* in das Eingabeaufforderungsfenster ein. Daraufhin öffnet sich ein neues Fenster.
3. Öffnen Sie die Eigenschaften für **LAN-Verbindung**.
4. Doppelklicken Sie auf „Internetprotokoll, Version 4“ (IPv4).
5. Wählen Sie **Folgende IP-Adresse verwenden** aus, und geben Sie die folgenden Werte ein:
   
   * **IP-Adresse:**Geben Sie die *primäre* private IP-Adresse ein.
   * **Subnetzmaske:**wird basierend auf dem Subnetz festgelegt. Wenn das Subnetz beispielsweise ein /24-Subnetz ist, lautet die Subnetzmaske „255.255.255.0“.
   * **Standardgateway:**die erste IP-Adresse im Subnetz. Wenn das Subnetz „10.0.0.1/24“ ist, lautet die Gateway-IP-Adresse „10.0.0.0/24“.
   * Klicken Sie auf **Folgende DNS-Serveradressen verwenden** , und geben Sie die folgenden Werte ein:
     * **Bevorzugter DNS-Server:** Geben Sie „168.63.129.16“ ein, wenn Sie nicht Ihren eigenen DNS-Server verwenden.  Ist das jedoch der Fall, geben Sie die IP-Adresse Ihres DNS-Servers ein.
   * Klicken Sie auf die Schaltfläche **Erweitert** , und fügen Sie zusätzliche IP-Adressen hinzu. Fügen Sie jede der in Schritt 8 aufgeführten sekundären privaten IP-Adressen der NIC mit dem gleichen Subnetz hinzu, das für die primäre IP-Adresse angegeben ist.
   * Klicken Sie auf **OK**, um die TCP/IP-Einstellungen zu schließen, und dann erneut auf **OK**, um die Adaptereinstellungen zu schließen. Daraufhin wird die RDP-Verbindung wiederhergestellt.
6. Geben Sie an einer Eingabeaufforderung den Befehl *ipconfig /all*ein. Alle hinzugefügten IP-Adressen werden angezeigt, und DHCP ist deaktiviert.

**Linux (Ubuntu)**

1. Öffnen Sie ein Terminalfenster.
2. Stellen Sie sicher, dass Sie der root-Benutzer sind. Wenn dies nicht der Fall ist, können Sie hierfür den folgenden Befehl verwenden:
   
            sudo -i
3. Aktualisieren Sie die Konfigurationsdatei der Netzwerkschnittstelle (es wird von „eth0“ ausgegangen).
   
   * Behalten Sie den vorhandenen Eintrag für DHCP bei. Dadurch wird die primäre IP-Adresse so konfiguriert, wie sie zuvor definiert war.
   * Fügen Sie mit den folgenden Befehlen eine Konfiguration für eine weitere statische IP-Adresse hinzu:
     
               cd /etc/network/interfaces.d/
               ls
     
       Es sollte eine CFG-Datei angezeigt werden.
4. Öffnen Sie die Datei: vi *Dateiname*.
   
    Am Ende der Datei sollten die folgenden Zeilen angezeigt werden:
   
            auto eth0
            iface eth0 inet dhcp
5. Fügen Sie nach den vorhandenen Zeilen in dieser Datei die folgenden Zeilen ein:
   
            iface eth0 inet static
            address <your private IP address here>
6. Speichern Sie die Datei mit dem folgenden Befehl:
   
            :wq
7. Setzen Sie die Netzwerkschnittstelle mit dem folgenden Befehl zurück:
   
           sudo ifdown eth0 && sudo ifup eth0
   
   > [!IMPORTANT]
   > Führen Sie bei Verwendung einer Remoteverbindung „ifdown“ und „ifup“ in der gleichen Zeile aus.
   > 
   > 
8. Geben Sie den folgenden Befehl ein, um zu überprüfen, ob die IP-Adresse der Netzwerkschnittstelle hinzugefügt wurde:
   
            ip addr list eth0
   
    Es sollte die IP-Adresse angezeigt werden, die Sie als Teil der Liste hinzugefügt haben.

**Linux (Redhat, CentOS u.a.)**

1. Öffnen Sie ein Terminalfenster.
2. Stellen Sie sicher, dass Sie der root-Benutzer sind. Wenn dies nicht der Fall ist, können Sie hierfür den folgenden Befehl verwenden:
   
            sudo -i
3. Geben Sie Ihr Kennwort ein, und befolgen Sie die entsprechenden Anweisungen. Nachdem Sie der root-Benutzer sind, navigieren Sie mit dem folgenden Befehl zum Ordner der Netzwerkskripts:
   
            cd /etc/sysconfig/network-scripts
4. Listen Sie die zugehörigen ifcfg-Dateien mit dem folgenden Befehl auf:
   
            ls ifcfg-*
   
    Unter den Dateien sollte die Datei *ifcfg-eth0* angezeigt werden.
5. Kopieren Sie die Datei *ifcfg-eth0*, und benennen Sie sie mit dem folgenden Befehl in *ifcfg-eth0:0* um:
   
            cp ifcfg-eth0 ifcfg-eth0:0
6. Bearbeiten Sie die Datei *ifcfg-eth0:0* mit dem folgenden Befehl:
   
            vi ifcfg-eth1
7. Geben Sie den folgenden Befehl ein, um das Gerät in den entsprechenden Namen in der Datei zu ändern (in diesem Fall *eth0:0* ):
   
            DEVICE=eth0:0
8. Ändern Sie die Zeile *IPADDR = YourPrivateIPAddress* entsprechend der IP-Adresse.
9. Speichern Sie die Datei mit dem folgenden Befehl:
   
            :wq
10. Starten Sie die Netzwerkdienste neu, und stellen Sie sicher, dass die Änderungen übernommen wurden. Führen Sie dazu die folgenden Befehle aus:
    
            /etc/init.d/network restart
            Ipconfig
    
    In der zurückgegebenen Liste sollte die hinzugefügte IP-Adresse *eth0:0*angezeigt werden.

## <a name="<a-name="add"></a>add-ip-addresses-to-an-existing-vm"></a><a name="add"></a>Hinzufügen von IP-Adressen zu einem vorhandenen virtuellen Computer
Führen Sie die folgenden Schritte aus, um einer vorhandenen NIC zusätzliche IP-Adressen hinzuzufügen:

1. Öffnen Sie eine PowerShell-Eingabeaufforderung, und führen Sie die restlichen Schritte in diesem Abschnitt innerhalb einer einzelnen PowerShell-Sitzung aus. Wenn Sie PowerShell nicht installiert und konfiguriert haben, befolgen Sie die Anweisungen im Artikel [Gewusst wie: Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) .
2. Ändern Sie die Werte der folgenden Variablen in den Namen der NIC, der Sie IP-Adressen hinzufügen möchten, und der Ressourcengruppe und des Standorts, in der bzw. an dem die NIC vorhanden ist:
   
        $NicName     = "RG1-VM1-NIC1"
        $RgName   = "RG1"
        $NicLocation = "westcentralus"
   
    Wenn Sie den Namen der zu ändernden NIC nicht kennen, geben Sie die folgenden Befehle ein. Ändern Sie dann die Werte der vorherigen Variablen:
   
        Get-AzureRmNetworkInterface | Format-Table Name, ResourceGroupName, Location
3. Erstellen Sie mithilfe des folgenden Befehls eine Variable, und legen Sie sie auf die vorhandene NIC fest:
   
        $nic = Get-AzureRmNetworkInterface -Name $NicName -ResourceGroupName $RgName
4. Rufen Sie die ID des Subnetzes ab, mit dem die NIC verbunden ist. Führen Sie dazu [Schritt 3](#subnet) des Abschnitts „Erstellen eines virtuellen Computers mit mehreren IP-Adressen“ in diesem Artikel aus.
5. Erstellen Sie die IP-Konfigurationen, die Sie dem Netzwerk hinzufügen möchten. Befolgen Sie dazu die Anweisungen in [Schritt 5](#ipconfigs) des Abschnitts „Erstellen eines virtuellen Computers mit mehreren IP-Adressen“ in diesem Artikel.
6. Ändern Sie *$IPConfigName4* in den Namen der IP-Konfiguration, die Sie im vorherigen Schritt erstellt haben. Geben Sie den folgenden Befehl ein, um die Konfiguration hinzuzufügen:
   
        Add-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName4 -NetworkInterface $nic -Subnet $Subnet1
7. Geben Sie den folgenden Befehl ein, um die NIC mit der IP-Konfiguration festzulegen:
   
        Set-AzureRmNetworkInterface -NetworkInterface $nic
8. Fügen Sie die IP-Adressen hinzu, die Sie der NIC im Betriebssystem des virtuellen Computers hinzugefügt haben. Befolgen Sie dazu die Anweisungen in [Schritt 9](#os) des Abschnitts „Erstellen eines virtuellen Computers mit mehreren IP-Adressen“ in diesem Artikel.

<!--HONumber=Oct16_HO2-->


