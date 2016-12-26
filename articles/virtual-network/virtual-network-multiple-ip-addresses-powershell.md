---
title: "Mehrere IP-Adressen für virtuelle Computer – PowerShell | Microsoft Docs"
description: Erfahren Sie, wie Sie mehrere IP-Adressen zu einem virtuellen Computer mithilfe von PowerShell zuweisen | Resource Manager.
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: c44ea62f-7e54-4e3b-81ef-0b132111f1f8
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/16/2016
ms.author: jdial;annahar
translationtype: Human Translation
ms.sourcegitcommit: a0d8a6dbe793bc4ea5211e772439d931c8e84a04
ms.openlocfilehash: cf2a57f96576b18692dd42a9680d7f3b8d8f7c69


---
# <a name="assign-multiple-ip-addresses-to-virtual-machines-using-powershell"></a>Zuweisen von mehreren IP-Adressen zu virtuellen Computern mithilfe von PowerShell

> [!div class="op_single_selector"]
> * [Azure-Portal](virtual-network-multiple-ip-addresses-portal.md)
> * [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)
> * [BEFEHLSZEILENSCHNITTSTELLE (CLI)](virtual-network-multiple-ip-addresses-cli.md)
>

Ein virtueller Azure-Computer (VM) verfügt über eine oder mehrere angefügte Netzwerkschnittstellen (NIC). Jede NIC kann eine oder mehrere zugewiesene statische und dynamische öffentliche oder private IP-Adressen aufweisen. Durch das Zuweisen von mehreren IP-Adressen zu einem virtuellen Computer bestehen folgende Möglichkeiten:

* Hosten mehrerer Websites oder Dienste mit unterschiedlichen IP-Adressen und SSL-Zertifikaten auf einem einzelnen Server
* Übernehmen der Rolle als virtuelles Netzwerkgerät, z.B. als Firewall oder Load Balancer
* Fähigkeit zum Hinzufügen einer privaten IP-Adresse für eine der NICs zu einem Azure Load Balancer-Back-End-Pool hinzufügen. Bisher konnte nur die primäre IP-Adresse für die primäre NIC einem Back-End-Pool hinzugefügt werden. Weitere Informationen darüber, wie in Konfigurationen mit mehreren IPs ein Lastenausgleich durchgeführt werden kann, finden Sie im Artikel [Load balancing multiple IP configurations (Lastenausgleich bei Konfigurationen mit mehreren IPs)](../load-balancer/load-balancer-multiple-ip.md).

Jeder an eine VM angeschlossenen NIC sind eine oder mehrere IP-Konfigurationen zugeordnet. Jede Konfiguration wird als statische oder dynamische private IP-Adresse zugewiesen. Jeder Konfiguration kann auch eine öffentliche IP-Adressressource zugeordnet sein. Einer öffentlichen IP-Adressressource wird entweder eine dynamische oder eine statische IP-Adresse zugewiesen. Wenn Sie nicht mit IP-Adressen in Azure vertraut sind, finden Sie weitere Informationen im Artikel [IP-Adressen in Azure](virtual-network-ip-addresses-overview-arm.md) .

In diesem Artikel wird erläutert, wie mit Azure PowerShell einem mit dem Azure Resource Manager-Bereitstellungsmodell erstellten virtuellen Computer mehrere IP-Adressen zugewiesen werden. Ressourcen, die mit dem klassischen Bereitstellungsmodell erstellt wurden, können nicht mehrere IP-Adressen zugewiesen werden. Weitere Informationen zu den Azure-Bereitstellungsmodellen finden Sie im Artikel zum Thema [Understand deployment models (Bereitstellungsmodelle verstehen)](../resource-manager-deployment-model.md).

[!INCLUDE [virtual-network-preview](../../includes/virtual-network-preview.md)]

## <a name="scenario"></a>Szenario
Ein virtueller Computer mit einer einzigen NIC wird erstellt und mit einem virtuellen Netzwerk verbunden. Der virtuelle Computer benötigt drei verschiedene *private* IP-Adressen und zwei *öffentliche* IP-Adressen. Die IP-Adressen werden den folgenden IP-Konfigurationen zugewiesen:

* **IPConfig-1:** weist eine *dynamische* private IP-Adresse (Standard) und eine *statische* öffentliche IP-Adresse zu.
* **IPConfig-2:** weist eine *statische* private IP-Adresse (Standard) und eine *statische* öffentliche IP-Adresse zu.
* **IPConfig-3:** weist eine *dynamische* private IP-Adresse und keine öffentliche IP-Adresse zu.
  
    ![Mehrere IP-Adressen](./media/virtual-network-multiple-ip-addresses-powershell/OneNIC-3IP.png)

Die IP-Konfigurationen werden der NIC zugewiesen, wenn diese erstellt und beim Erstellen des virtuellen Computers an diesen angefügt wird. Die für das Szenario verwendeten IP-Adresstypen dienen der Veranschaulichung. Sie können beliebige IP-Adressen und Zuweisungstypen zuweisen.

## <a name="a-name--createacreate-a-vm-with-multiple-ip-addresses"></a><a name = "create"></a>Erstellen eines virtuellen Computers mit mehreren IP-Adressen

In den folgenden Schritten wird beschrieben, wie gemäß dem Szenario beispielhaft ein virtueller Computer mit mehreren IP-Adressen erstellt werden kann. Sie können die Variablennamen und IP-Adresstypen an Ihren Implementierungsbedarf anpassen.

1. Öffnen Sie eine PowerShell-Eingabeaufforderung, und führen Sie die restlichen Schritte in diesem Abschnitt innerhalb einer einzelnen PowerShell-Sitzung aus. Wenn Sie PowerShell nicht installiert und konfiguriert haben, befolgen Sie die Anweisungen im Artikel [Gewusst wie: Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) .
2. Wenn Sie sich für die Vorschau registrieren möchten, senden Sie eine E-Mail an [Mehrere IPs](mailto:MultipleIPsPreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e), und geben Sie Ihre Abonnement-ID und die beabsichtigte Verwendung an. Versuchen Sie nicht, die verbleibenden Schritte auszuführen:
    - Führen Sie die verbleibenden Schritte erst aus, wenn Sie eine E-Mail-Benachrichtigung mit dem Hinweis erhalten haben, dass Sie für die Vorschauversion akzeptiert wurden.
    - Ohne den in der E-Mail enthaltenen Anweisungen zu folgen, erhalten Sie:
3. Führen Sie die Schritte 1-4 des Artikels [Erstellen einer Windows-VM](../virtual-machines/virtual-machines-windows-ps-create.md) aus. Führen Sie Schritt 5 (Erstellen einer öffentlichen IP-Adresse und Netzwerkschnittstelle) nicht aus. Wenn Sie die Namen von in diesem Artikel verwendeten Variablen verwenden, ändern Sie auch die Namen der Variablen in den übrigen Schritten. Wählen Sie ein Linux-Betriebssystem und nicht Windows aus, um einen virtuellen Linux-Computer zu erstellen.
4. Erstellen Sie eine Variable, um das in Schritt 4 (Erstellen eines virtuellen Netzwerks) des Artikels „Erstellen einer Windows-VM“ erstellte virtuelle Netzwerk zu speichern, indem Sie den folgenden Befehl eingeben:

    ```powershell
    $SubnetName = $mySubnet.Name
    $Subnet = $myVnet.Subnets | Where-Object { $_.Name -eq $SubnetName }
    ```
5. Definieren Sie die IP-Konfigurationen, die Sie der NIC zuweisen möchten. Sie können Konfigurationen je nach Bedarf hinzufügen, entfernen oder ändern. Die folgenden Konfigurationen werden in dem Szenario beschrieben:

    **IPConfig-1**

    Geben Sie die folgenden Befehle ein, um die folgenden Elemente zu erstellen:
    - eine öffentliche IP-Adressressource mit einer statischen öffentlichen IP-Adresse
    - eine IP-Konfiguration mit der öffentlichen IP-Adressressource und einer dynamischen privaten IP-Adresse

    ```powershell
    $myPublicIp1     = New-AzureRmPublicIpAddress -Name "myPublicIp1" -ResourceGroupName $myResourceGroup -Location $location -AllocationMethod Static
    $IpConfigName1  = "IPConfig-1"
    $IpConfig1      = New-AzureRmNetworkInterfaceIpConfig -Name $IpConfigName1 -Subnet $Subnet -PublicIpAddress $myPublicIp1 -Primary
    ```

    Beachten Sie den Wechsel von `-Primary` im vorherigen Befehl. Wenn Sie einer NIC mehrere IP-Konfigurationen zuweisen, muss eine Konfiguration mit dem Wert *Primary*zugewiesen werden.

    > [!NOTE]
    > Für öffentliche IP-Adressen fällt eine geringe Gebühr an. Weitere Informationen zu den Preisen finden Sie auf der Seite [Preise für IP-Adressen](https://azure.microsoft.com/pricing/details/ip-addresses) . Die Anzahl der öffentlichen IP-Adressen, die in einem Abonnement verwendet werden können, ist beschränkt. Weitere Informationen über die Einschränkungen finden Sie im Artikel zu den [Azure-Einschränkungen](../azure-subscription-service-limits.md#networking-limits) .
    >

    **IPConfig-2**

    Ändern Sie den Wert der Variable **$IPAddress**, die auf eine verfügbare, gültige Adresse im Subnetz folgt, das Sie erstellt haben. Um zu überprüfen, ob die Adresse 10.0.0.5 im Subnetz verfügbar ist, geben Sie den Befehl `Test-AzureRmPrivateIPAddressAvailability -IPAddress 10.0.0.5 -VirtualNetwork $myVnet` ein. Wenn die Adresse verfügbar ist, gibt die Ausgabe *TRUE* zurück. Wenn sie nicht verfügbar ist, gibt die Ausgabe *FALSE* und eine Liste der verfügbaren Adressen zurück. Geben Sie die folgenden Befehle ein, um eine neue öffentliche IP-Adressressource und eine neue IP-Konfiguration mit einer statischen öffentlichen IP-Adresse und einer statischen privaten IP-Adresse zu erstellen:
    
    ```powershell
    $IpConfigName2 = "IPConfig-2"
    $IPAddress     = 10.0.0.5
    $myPublicIp2   = New-AzureRmPublicIpAddress -Name "myPublicIp2" -ResourceGroupName $myResourceGroup `
    -Location $location -AllocationMethod Static
    $IpConfig2     = New-AzureRmNetworkInterfaceIpConfig -Name $IpConfigName2 `
    -Subnet $Subnet -PrivateIpAddress $IPAddress -PublicIpAddress $myPublicIp2
    ```

    **IPConfig-3**

    Geben Sie die folgenden Befehle ein, um eine IP-Konfiguration mit einer dynamischen privaten IP-Adresse und ohne öffentliche IP-Adressen zu erstellen:

    ```powershell
    $IpConfigName3 = "IpConfig-3"
    $IpConfig3 = New-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName3 -Subnet $Subnet
    ```
6. Erstellen Sie die NIC mit den im vorherigen Schritt definierten IP-Adresskonfigurationen, indem Sie den folgenden Befehl eingeben:

    ```powershell
    $myNIC = New-AzureRmNetworkInterface -Name myNIC -ResourceGroupName $myResourceGroup `
    -Location $location -IpConfiguration $IpConfig1,$IpConfig2,$IpConfig3
    ```
    > [!NOTE]
    > Obwohl in diesem Artikel alle IP-Konfigurationen einer einzigen NIC zugewiesen werden, ist es auch möglich, jeder NIC in einem virtuellen Computer mehrere IP-Konfigurationen zuzuweisen. Erfahren Sie im Artikel [Bereitstellen von Multi-NIC-VMs mit PowerShell](virtual-network-deploy-multinic-arm-ps.md), wie Sie einen virtuellen Computer mit mehreren NICs erstellen.

7. Schließen Sie Schritt 6 des Artikels [Erstellen einer Windows-VM](../virtual-machines/virtual-machines-windows-ps-create.md) ab. 

    > [!WARNING]
    > Schritt 6 im Artikel „Erstellen einer Windows-VM“ schlägt fehl, wenn:
    > - Sie die Variable mit dem Namen $myNIC in Schritt 6 dieses Artikels in etwas anderes geändert haben.
    > - Sie die vorherigen Schritte dieses Artikels und des Artikels „Erstellen einer Windows-VM“ nicht abgeschlossen haben.
    >
8. Geben Sie den folgenden Befehl ein, um die privaten IP-Adressen und die der NIC zugewiesene öffentliche IP-Adressressource anzuzeigen:

    ```powershell
    $myNIC.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary
    ```

## <a name="a-nameosconfigaadd-ip-addresses-to-a-vm-operating-system"></a><a name="OsConfig"></a>Hinzufügen von IP-Adressen zu einem VM-Betriebssystem

Stellen Sie eine Verbindung mit einem virtuellen Computer her, den Sie mit mehreren privaten IP-Adresse erstellt haben, und melden Sie sich an. Alle dem virtuellen Computer hinzugefügten privaten IP-Adressen müssen manuell hinzugefügt werden. (Das gilt auch für die primäre Adresse.) Führen Sie für das Betriebssystem Ihres virtuellen Computers folgende Schritte aus:

### <a name="windows"></a>Windows

1. Geben Sie an einer Eingabeaufforderung den Befehl *ipconfig /all*ein.  Sie können nur die *primäre* private Adresse anzeigen (über DHCP).
2. Geben Sie an der Eingabeaufforderung *ncpa.cpl* ein, um das Fenster **Netzwerkverbindungen** zu öffnen.
3. Öffnen Sie die Eigenschaften für **LAN-Verbindung**.
4. Doppelklicken Sie auf „Internetprotokoll, Version 4“ (IPv4).
5. Wählen Sie **Folgende IP-Adresse verwenden** aus, und geben Sie die folgenden Werte ein:

    * **IP-Adresse:**Geben Sie die *primäre* private IP-Adresse ein.
    * **Subnetzmaske:**wird basierend auf dem Subnetz festgelegt. Wenn das Subnetz beispielsweise ein /24-Subnetz ist, lautet die Subnetzmaske „255.255.255.0“.
    * **Standardgateway:**die erste IP-Adresse im Subnetz. Wenn das Subnetz „10.0.0.1/24“ ist, lautet die Gateway-IP-Adresse „10.0.0.0/24“.
    * Klicken Sie auf **Folgende DNS-Serveradressen verwenden** , und geben Sie die folgenden Werte ein:
        * **Bevorzugter DNS-Server:** Geben Sie „168.63.129.16“ ein, falls Sie nicht Ihren eigenen DNS-Server verwenden.  Wenn Sie Ihren eigenen DNS-Server verwenden, geben Sie die IP-Adresse für Ihren Server ein.
    * Klicken Sie auf die Schaltfläche **Erweitert** , und fügen Sie zusätzliche IP-Adressen hinzu. Fügen Sie jede der in Schritt 8 aufgeführten sekundären privaten IP-Adressen der NIC mit dem gleichen Subnetz hinzu, das für die primäre IP-Adresse angegeben ist.
    * Klicken Sie auf **OK**, um die TCP/IP-Einstellungen zu schließen, und dann erneut auf **OK**, um die Adaptereinstellungen zu schließen. Die RDP-Verbindung wird wiederhergestellt.
6. Geben Sie an einer Eingabeaufforderung den Befehl *ipconfig /all*ein. Alle hinzugefügten IP-Adressen werden angezeigt, und DHCP ist deaktiviert.
    
### <a name="linux-ubuntu"></a>Linux (Ubuntu)

1. Öffnen Sie ein Terminalfenster.
2. Stellen Sie sicher, dass Sie der root-Benutzer sind. Geben Sie andernfalls den folgenden Befehl ein:

    ```bash
    sudo -i
    ```

3. Aktualisieren Sie die Konfigurationsdatei der Netzwerkschnittstelle (es wird von „eth0“ ausgegangen).

    * Behalten Sie den vorhandenen Eintrag für DHCP bei. Die Konfiguration der primären IP-Adresse bleibt unverändert.
    * Fügen Sie mit den folgenden Befehlen eine Konfiguration für eine weitere statische IP-Adresse hinzu:

        ```bash
        cd /etc/network/interfaces.d/
        ls
        ```

    Es sollte eine CFG-Datei angezeigt werden.
4. Öffnen Sie die Datei: vi *Dateiname*.

    Am Ende der Datei sollten die folgenden Zeilen angezeigt werden:

    ```bash
    auto eth0
    iface eth0 inet dhcp
    ```

5. Fügen Sie nach den vorhandenen Zeilen in dieser Datei die folgenden Zeilen ein:

    ```bash
    iface eth0 inet static
    address <your private IP address here>
    ```

6. Speichern Sie die Datei mit dem folgenden Befehl:

    ```bash
    :wq
    ```

7. Setzen Sie die Netzwerkschnittstelle mit dem folgenden Befehl zurück:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

    > [!IMPORTANT]
    > Führen Sie bei Verwendung einer Remoteverbindung „ifdown“ und „ifup“ in der gleichen Zeile aus.
    >

8. Geben Sie den folgenden Befehl ein, um zu überprüfen, ob die IP-Adresse der Netzwerkschnittstelle hinzugefügt wurde:

    ```bash
    Ip addr list eth0
    ```

    Es sollte die IP-Adresse angezeigt werden, die Sie als Teil der Liste hinzugefügt haben.
    
### <a name="linux-redhat-centos-and-others"></a>Linux (Redhat, CentOS u.a.)

1. Öffnen Sie ein Terminalfenster.
2. Stellen Sie sicher, dass Sie der root-Benutzer sind. Geben Sie andernfalls den folgenden Befehl ein:

    ```bash
    sudo -i
    ```

3. Geben Sie Ihr Kennwort ein, und befolgen Sie die entsprechenden Anweisungen. Nachdem Sie der root-Benutzer sind, navigieren Sie mit dem folgenden Befehl zum Ordner der Netzwerkskripts:

    ```bash
    cd /etc/sysconfig/network-scripts
    ```

4. Listen Sie die zugehörigen ifcfg-Dateien mit dem folgenden Befehl auf:

    ```bash
    ls ifcfg-*
    ```

    Unter den Dateien sollte die Datei *ifcfg-eth0* angezeigt werden.

5. Kopieren Sie die Datei *ifcfg-eth0*, und benennen Sie sie mit dem folgenden Befehl in *ifcfg-eth0:0* um:

    ```bash
    cp ifcfg-eth0 ifcfg-eth0:0
    ```

6. Bearbeiten Sie die Datei *ifcfg-eth0:0* mit dem folgenden Befehl:

    ```bash
    vi ifcfg-eth1
    ```

7. Geben Sie den folgenden Befehl ein, um das Gerät in den entsprechenden Namen in der Datei zu ändern (in diesem Fall *eth0:0* ):

    ```bash
    DEVICE=eth0:0
    ```

8. Ändern Sie die Zeile *IPADDR = YourPrivateIPAddress* entsprechend der IP-Adresse.
9. Speichern Sie die Datei mit dem folgenden Befehl:

    ```bash
    :wq
    ```

10. Starten Sie die Netzwerkdienste neu, und stellen Sie sicher, dass die Änderungen übernommen wurden. Führen Sie dazu die folgenden Befehle aus:

    ```bash
    /etc/init.d/network restart
    Ipconfig
    ```

    In der zurückgegebenen Liste sollte die hinzugefügte IP-Adresse *eth0:0*angezeigt werden.

## <a name="a-nameaddaadd-ip-addresses-to-a-vm"></a><a name="add"></a>Hinzufügen von IP-Adressen zu einem virtuellen Computer

Sie können zu einer vorhandenen NIC zusätzliche private und öffentliche IP-Adressen hinzufügen. Führen Sie dazu die folgenden Schritte aus. Die Beispiele bauen auf dem in diesem Artikel beschriebenen [Szenario](#Scenario) auf.

1. Öffnen Sie eine PowerShell-Eingabeaufforderung, und führen Sie die restlichen Schritte in diesem Abschnitt innerhalb einer einzelnen PowerShell-Sitzung aus. Wenn Sie PowerShell nicht installiert und konfiguriert haben, befolgen Sie die Anweisungen im Artikel [Gewusst wie: Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) .
2. Wenn Sie sich für die Vorschau registrieren möchten, senden Sie eine E-Mail an [Mehrere IPs](mailto:MultipleIPsPreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e), und geben Sie Ihre Abonnement-ID und die beabsichtigte Verwendung an. Versuchen Sie nicht, die verbleibenden Schritte auszuführen:
    - Führen Sie die verbleibenden Schritte erst aus, wenn Sie eine E-Mail-Benachrichtigung mit dem Hinweis erhalten haben, dass Sie für die Vorschauversion akzeptiert wurden.
    - Ohne den in der E-Mail enthaltenen Anweisungen zu folgen, erhalten Sie:
3. Ändern Sie die Werte der folgenden Variablen in den Namen der NIC, der Sie IP-Adressen hinzufügen möchten, und der Ressourcengruppe und des Standorts, in der bzw. an dem die NIC vorhanden ist:

    ```powershell
    $NICname         = "myNIC"
    $myResourceGroup = "myResourceGroup"
    $location        = "westcentralus"
    ```

    Wenn Sie den Namen der zu ändernden NIC nicht kennen, geben Sie die folgenden Befehle ein. Ändern Sie dann die Werte der vorherigen Variablen:

    ```powershell
    Get-AzureRmNetworkInterface | Format-Table Name, ResourceGroupName, Location
    ```
4. Erstellen Sie mithilfe des folgenden Befehls eine Variable, und legen Sie sie auf die vorhandene NIC fest:

    ```powershell
    $myNIC = Get-AzureRmNetworkInterface -Name $NICname -ResourceGroupName $myResourceGroup
    ```
5. Ändern Sie in den folgenden Befehlen *myVNet* und *mySubnet* in die Namen des VNETs und Subnetzes, mit denen die NIC verbunden ist. Geben Sie die Befehle ein, um das VNET und die Subnetzobjekte, mit denen die NIC verbunden ist, aufzurufen:

    ```powershell
    $myVnet = Get-AzureRMVirtualnetwork -Name myVNet -ResourceGroupName $myResourceGroup
    $Subnet = $myVnet.Subnets | Where-Object { $_.Name -eq "mySubnet" }
    ```
    Wenn Sie den Namen des VNET oder Subnetzes nicht kennen, mit denen die NIC verbunden ist, geben Sie den folgenden Befehl ein:
    ```powershell
    $mynic.IpConfigurations
    ```
    Suchen Sie nach Text, der dem Text in der folgenden Rückgabezeile ähnelt:

        Subnet   : {
                     "Id": "/subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"

    In dieser Ausgabe ist *myVnet* das VNET und *mySubnet* das Subnetz, mit dem die NIC verbunden ist.

6. Führen Sie gemäß Ihren Anforderungen die Schritte eines der folgenden Abschnitte aus:

    **Hinzufügen einer privaten IP-Adresse**
    
    Um eine private IP-Adresse zu einer NIC hinzuzufügen, müssen Sie eine IP-Konfiguration erstellen. Der folgende Befehl erstellt eine Konfiguration mit der statischen IP-Adresse 10.0.0.7. Wenn Sie eine dynamische private IP-Adresse hinzufügen möchten, entfernen Sie vor der Eingabe des Befehls `-PrivateIpAddress 10.0.0.7`. Wenn Sie eine statische IP-Adresse angeben, darf es sich dabei nicht um eine für das Subnetz verwendete Adresse handeln. Es wird empfohlen, dass Sie die Adresse zuerst auf Ihre Verfügbarkeit testen, indem Sie den Befehl `Test-AzureRmPrivateIPAddressAvailability -IPAddress 10.0.0.7 -VirtualNetwork $myVnet` eingeben. Wenn die IP-Adresse verfügbar ist, gibt die Ausgabe *TRUE* zurück. Wenn sie nicht verfügbar ist, gibt die Ausgabe *FALSE* und eine Liste der verfügbaren Adressen zurück.

    ```powershell
    Add-AzureRmNetworkInterfaceIpConfig -Name IPConfig-4 -NetworkInterface `
     $myNIC -Subnet $Subnet -PrivateIpAddress 10.0.0.7
    ```
    Sie können so viele Konfigurationen wie benötigt erstellen. Verwenden Sie dabei eindeutige Namen für die Konfigurationen und privaten IP-Adressen (bei Konfigurationen mit statischen IP-Adressen).

    **Hinzufügen einer öffentlichen IP-Adresse**
    
    Eine öffentliche IP-Adresse wird hinzugefügt, indem entweder eine neue oder eine vorhandene IP-Konfiguration zugeordnet wird. Führen Sie gemäß Ihren Anforderungen die Schritte des entsprechenden Abschnitts durch.

    > [!NOTE]
    > Für öffentliche IP-Adressen fällt eine geringe Gebühr an. Weitere Informationen zu den Preisen finden Sie auf der Seite [Preise für IP-Adressen](https://azure.microsoft.com/pricing/details/ip-addresses) . Die Anzahl der öffentlichen IP-Adressen, die in einem Abonnement verwendet werden können, ist beschränkt. Weitere Informationen über die Einschränkungen finden Sie im Artikel zu den [Azure-Einschränkungen](../azure-subscription-service-limits.md#networking-limits) .
    >

    **Zuordnen der Ressource zu einer neuen IP-Konfiguration**
    
    Immer, wenn Sie in einer neuen IP-Konfiguration eine öffentliche IP-Adresse hinzufügen, müssen Sie auch eine private IP-Adresse hinzufügen, denn in jeder IP-Konfiguration muss es eine private IP-Adresse geben. Sie können entweder eine vorhandene öffentliche IP-Adressressource hinzufügen oder eine neue erstellen. Verwenden Sie den folgenden Befehl, um eine neue zu erstellen:
    
    ```powershell
    $myPublicIp3   = New-AzureRmPublicIpAddress -Name "myPublicIp3" -ResourceGroupName $myResourceGroup `
    -Location $location -AllocationMethod Static
    ```

    Um eine neue IP-Konfiguration mit einer dynamischen privaten IP-Adresse und der zugeordneten öffentlichen IP-Adressressource *myPublicIP3* zu erstellen, geben Sie den folgenden Befehl ein:

    ```powershell
    Add-AzureRmNetworkInterfaceIpConfig -Name IPConfig-4 -NetworkInterface `
     $myNIC -Subnet $Subnet -PublicIpAddress $myPublicIp3
    ```

    **Zuordnen der Ressource zu einer vorhandenen IP-Konfiguration**
    Eine öffentliche IP-Adressressource kann nur einer IP-Konfiguration zugeordnet werden, der noch keine öffentliche IP-Adressressource zugeordnet ist. Ob einer IP-Konfiguration eine öffentliche IP-Adresse zugeordnet ist, können Sie mithilfe des folgenden Befehls ermitteln:

    ```powershell
    $myNIC.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary
    ```

    Suchen Sie nach einer Zeile, die der folgenden Rückgabezeile ähnelt:

        Name       PrivateIpAddress PublicIpAddress                                           Primary
        ----       ---------------- ---------------                                           -------
        IPConfig-1 10.0.0.4         Microsoft.Azure.Commands.Network.Models.PSPublicIpAddress    True
        IPConfig-2 10.0.0.5         Microsoft.Azure.Commands.Network.Models.PSPublicIpAddress   False
        IpConfig-3 10.0.0.6                                                                     False

    Da die Spalte **PublicIpAddress (öffentliche IP-Adresse)** für *IpConfig-3* leer ist, ist derzeit keine öffentliche IP-Adressressource zugeordnet. Sie können IpConfig-3 eine vorhandene öffentliche IP-Adressressource zuordnen oder mit dem folgenden Befehl eine erstellen:

    ```powershell
    $myPublicIp3   = New-AzureRmPublicIpAddress -Name "myPublicIp3" -ResourceGroupName $myResourceGroup `
    -Location $location -AllocationMethod Static
    ```

    Geben Sie den folgenden Befehl ein, um die öffentliche IP-Adressressource der vorhandenen IP-Konfiguration namens *IpConfig-3* zuzuordnen:
    
    ```powershell
    Set-AzureRmNetworkInterfaceIpConfig -Name IpConfig-3 -NetworkInterface $mynic -Subnet $Subnet -PublicIpAddress $myPublicIp3
    ```

7. Geben Sie den folgenden Befehl ein, um die NIC mit der IP-Konfiguration festzulegen:

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $myNIC
    ```

8. Geben Sie den folgenden Befehl ein, um die privaten IP-Adressen und die der NIC zugewiesene öffentliche IP-Adressressource anzuzeigen:

    ```powershell   
    $myNIC.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary
    ```

9. Fügen Sie die IP-Adressen hinzu, die Sie der NIC im Betriebssystem des virtuellen Computers hinzugefügt haben. Befolgen Sie dazu die Anweisungen im Abschnitt [Hinzufügen von IP-Adressen zu einem VM-Betriebssystem](#OsConfig) in diesem Artikel.



<!--HONumber=Nov16_HO3-->


