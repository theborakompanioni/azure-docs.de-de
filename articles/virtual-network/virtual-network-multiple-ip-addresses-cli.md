---
title: "Mehrere IP-Adressen für virtuelle Computer – Azure CLI | Microsoft Docs"
description: Erfahren Sie mehr zum Zuweisen von mehreren IP-Adressen zu einem virtuellen Computer mit Azure CLI. | Resource Manager
services: virtual-network
documentationcenter: na
author: anavinahar
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/17/2016
ms.author: annahar
translationtype: Human Translation
ms.sourcegitcommit: bf92cdc3f6adf1bfaffbcae4e8a0257d0682e33d
ms.openlocfilehash: 0bb3e5f74cb6d013a2b14b4e4c81f0936f90004a


---
# <a name="assign-multiple-ip-addresses-to-virtual-machines-using-azure-cli"></a>Zuweisen von mehreren IP-Adressen zu virtuellen Computern mit Azure CLI

> [!div class="op_single_selector"]
> * [Azure-Portal](virtual-network-multiple-ip-addresses-portal.md)
> * [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)
> * [BEFEHLSZEILENSCHNITTSTELLE (CLI)](virtual-network-multiple-ip-addresses-cli.md)

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

1. Installieren und konfigurieren Sie die Azure CLI anhand der Schritte im Artikel [Installieren und Konfigurieren der Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md), und melden Sie sich dann an Ihrem Azure-Konto an.

2. Wenn Sie sich für die Vorschau registrieren möchten, senden Sie eine E-Mail an [Mehrere IPs](mailto:MultipleIPsPreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e), und geben Sie Ihre Abonnement-ID und die beabsichtigte Verwendung an. Versuchen Sie nicht, die verbleibenden Schritte auszuführen:
    - Führen Sie die verbleibenden Schritte erst aus, wenn Sie eine E-Mail-Benachrichtigung mit dem Hinweis erhalten haben, dass Sie für die Vorschauversion akzeptiert wurden.
    - Ohne den in der E-Mail enthaltenen Anweisungen zu folgen, erhalten Sie:
3. [Erstellen Sie eine Ressourcengruppe](../virtual-machines/virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-resource-groups-and-choose-deployment-locations) und anschließend ein [virtuelles Netzwerk und ein Subnetz](../virtual-machines/virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-virtual-network-and-subnet). Ändern Sie die Felder ``` --address-prefixes ``` und ```--address-prefix``` auf die folgenden Werte, um das in diesem Artikel skizzierte Szenario exakt einzuhalten:

    ```azurecli
    --address-prefixes 10.0.0.0/16
    --address-prefix 10.0.0.0/24
    ```
    >[!NOTE] 
    >Im oben referenzierten Artikel wird Europa, Westen als Standort für die Ressourcenerstellung gewählt, aber in diesem Artikel verwenden wir die Region „USA, Westen-Mitte“. Führen Sie bitte die entsprechenden Änderungen durch.

4. [Erstellen Sie ein Speicherkonto](../virtual-machines/virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-storage-account) für Ihren virtuellen Computer.


5. Erstellen Sie NIC und die IP-Konfigurationen, die Sie der NIC zuweisen möchten. Sie können Konfigurationen je nach Bedarf hinzufügen, entfernen oder ändern. Die folgenden Konfigurationen werden in dem Szenario beschrieben:

    **IPConfig-1**

    Geben Sie die folgenden Befehle ein, um die folgenden Elemente zu erstellen:

    - eine öffentliche IP-Adressressource mit einer statischen öffentlichen IP-Adresse
    - eine IP-Konfiguration mit der öffentlichen IP-Adressressource und einer dynamischen privaten IP-Adresse

    ```azurecli
    azure network public-ip create --resource-group myResourceGroup --location westcentralus --name myPublicIP --domain-name-label mypublicdns --allocation-method Static
    ```
    > [!NOTE]
    > Für öffentliche IP-Adressen fällt eine geringe Gebühr an. Weitere Informationen zu den Preisen finden Sie auf der Seite [Preise für IP-Adressen](https://azure.microsoft.com/pricing/details/ip-addresses) . Die Anzahl der öffentlichen IP-Adressen, die in einem Abonnement verwendet werden können, ist beschränkt. Weitere Informationen über die Einschränkungen finden Sie im Artikel zu den [Azure-Einschränkungen](../azure-subscription-service-limits.md#networking-limits) .

    ```azurecli
    azure network nic create --resource-group myResourceGroup --location westcentralus --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic1 --public-ip-name myPublicIP
    ```

    **IPConfig-2**

     Geben Sie die folgenden Befehle ein, um eine neue öffentliche IP-Adressressource und eine neue IP-Konfiguration mit einer statischen öffentlichen IP-Adresse und einer statischen privaten IP-Adresse zu erstellen:
    
    ```azurecli
    azure network public-ip create --resource-group myResourceGroup --location westcentralus --name myPublicIP2 --domain-name-label mypublicdns2 --allocation-method Static

    azure network nic ip-config create --resource-group myResourceGroup --nic-name myNic1 --name IPConfig-2 --private-ip-address 10.0.0.5 --public-ip-name myPublicIP2
    ```

    **IPConfig-3**

    Geben Sie die folgenden Befehle ein, um eine IP-Konfiguration mit einer dynamischen privaten IP-Adresse und ohne öffentliche IP-Adressen zu erstellen:

    ```azurecli
    azure network nic ip-config create --resource-group myResourceGroup --nic-name myNic1 --name IPConfig-3
    ```

    >[!NOTE] 
    >Obwohl in diesem Artikel alle IP-Konfigurationen einer einzigen NIC zugewiesen werden, ist es auch möglich, jeder NIC in einem virtuellen Computer mehrere IP-Konfigurationen zuzuweisen. Erfahren Sie im Artikel „Erstellen eines virtuellen Computers mit mehreren Netzwerkschnittstellenkarten (NICs)“, wie Sie einen virtuellen Computer mit mehreren NICs erstellen.

6. Artikel: [Erstellen der Linux-VMs](../virtual-machines/virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-the-linux-vms). Achten Sie darauf, die Eigenschaft ```  --availset-name myAvailabilitySet \ ``` zu entfernen, da sie in diesem Szenario nicht benötigt wird. Verwenden Sie basierend auf Ihrem Szenario den richtigen Standort. 

    >[!WARNING] 
    > Schritt 6 im Artikel „Erstellen eines virtuellen Computers“ wird scheitern, wenn die Größe des virtuellen Computers an dem von Ihnen gewählten Standort nicht unterstützt wird. Führen Sie den folgenden Befehl aus, um z.B. eine vollständige Liste der virtuellen Computer in der Region „USA, Westen-Mitte“ zu erhalten. Dieser Standortname kann gemäß Ihrem Szenario geändert werden.
    > 
    >       azure vm sizes --location westcentralus

    Um z.B. die Größe des virtuellen Computers auf Standard DS2 v2 zu ändern, fügen Sie einfach die Eigenschaft ```  --vm-size Standard_DS3_v2``` zu dem Befehl ``` azure vm create ``` in Schritt 6 hinzu.

7. Geben Sie den folgenden Befehl ein, um die NIC und die zugeordneten IP-Konfigurationen anzuzeigen:

    ```azurecli
    azure network nic show --resource-group myResourceGroup --name myNic1
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

1. Öffnen Sie Azure CLI, und führen Sie die in diesem Abschnitt verbleibenden Schritte innerhalb einer einzigen CLI-Sitzung aus. Wenn Azure CLI nicht schon installiert und konfiguriert ist, installieren und konfigurieren Sie Azure CLI anhand der Schritte im Artikel [Installieren und Konfigurieren der Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md), und melden Sie sich dann bei Ihrem Azure-Konto an.

2. Wenn Sie sich für die Vorschau registrieren möchten, senden Sie eine E-Mail an [Mehrere IPs](mailto:MultipleIPsPreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e), und geben Sie Ihre Abonnement-ID und die beabsichtigte Verwendung an. Versuchen Sie nicht, die verbleibenden Schritte auszuführen:
    - Führen Sie die verbleibenden Schritte erst aus, wenn Sie eine E-Mail-Benachrichtigung mit dem Hinweis erhalten haben, dass Sie für die Vorschauversion akzeptiert wurden.
    - Ohne den in der E-Mail enthaltenen Anweisungen zu folgen, erhalten Sie:


3. Führen Sie gemäß Ihren Anforderungen die Schritte eines der folgenden Abschnitte aus:

    **Hinzufügen einer privaten IP-Adresse**
    
    Um eine private IP-Adresse zu einer NIC hinzuzufügen, müssen Sie mit dem folgenden Befehl eine IP-Konfiguration erstellen.  Wenn Sie eine dynamische private IP-Adresse hinzufügen möchten, entfernen Sie vor der Eingabe des Befehls ```-PrivateIpAddress 10.0.0.7```. Wenn Sie eine statische IP-Adresse angeben, darf es sich dabei nicht um eine für das Subnetz verwendete Adresse handeln.

    ```azurecli
    azure network nic ip-config create --resource-group myResourceGroup --nic-name myNic1 --private-ip-address 10.0.0.7 --name IPConfig-4
    ```
    Sie können so viele Konfigurationen wie benötigt erstellen. Verwenden Sie dabei eindeutige Namen für die Konfigurationen und privaten IP-Adressen (bei Konfigurationen mit statischen IP-Adressen).

    **Hinzufügen einer öffentlichen IP-Adresse**
    
    Eine öffentliche IP-Adresse wird hinzugefügt, indem entweder eine neue oder eine vorhandene IP-Konfiguration zugeordnet wird. Führen Sie gemäß Ihren Anforderungen die Schritte des entsprechenden Abschnitts durch.

    > [!NOTE]
    > Für öffentliche IP-Adressen fällt eine geringe Gebühr an. Weitere Informationen zu den Preisen finden Sie auf der Seite [Preise für IP-Adressen](https://azure.microsoft.com/pricing/details/ip-addresses) . Die Anzahl der öffentlichen IP-Adressen, die in einem Abonnement verwendet werden können, ist beschränkt. Weitere Informationen über die Einschränkungen finden Sie im Artikel zu den [Azure-Einschränkungen](../azure-subscription-service-limits.md#networking-limits) .
    >

    **Zuordnen der Ressource zu einer neuen IP-Konfiguration**
    
    Immer, wenn Sie in einer neuen IP-Konfiguration eine öffentliche IP-Adresse hinzufügen, müssen Sie auch eine private IP-Adresse hinzufügen, denn in jeder IP-Konfiguration muss es eine private IP-Adresse geben. Sie können entweder eine vorhandene öffentliche IP-Adressressource hinzufügen oder eine neue erstellen. Verwenden Sie den folgenden Befehl, um eine neue zu erstellen:
    
    ```azurecli
    azure network public-ip create --resource-group myResourceGroup --location westcentralus --name myPublicIP3 --domain-name-label mypublicdns3
    ```

    Um eine neue IP-Konfiguration mit einer dynamischen privaten IP-Adresse und der zugeordneten öffentlichen IP-Adressressource *myPublicIP3* zu erstellen, geben Sie den folgenden Befehl ein:

    ```azurecli
    azure network nic ip-config create --resource-group myResourceGroup --nic-name myNic --name IPConfig-4 --public-ip-name myPublicIP3
    ```

    **Zuordnen der Ressource zu einer vorhandenen IP-Konfiguration**
    Eine öffentliche IP-Adressressource kann nur einer IP-Konfiguration zugeordnet werden, der noch keine öffentliche IP-Adressressource zugeordnet ist. Ob einer IP-Konfiguration eine öffentliche IP-Adresse zugeordnet ist, können Sie mithilfe des folgenden Befehls ermitteln:

    ```azurecli
    azure network nic ip-config list --resource-group myResourceGroup --nic-name myNic1
    ```

    Suchen Sie nach einer Zeile, die der folgenden Rückgabezeile ähnelt:
    
        Name               Provisioning state  Primary  Private IP allocation  Private IP version  Private IP address  Subnet    Public IP
        -----------------  ------------------  -------  ---------------------  ------------------  ------------------  --------  -----------
        default-ip-config  Succeeded           true     Dynamic                IPv4                10.0.0.4            mySubnet  myPublicIP
        IPConfig-2         Succeeded           false    Static                 IPv4                10.0.0.5            mySubnet  myPublicIP2
        IPConfig-3         Succeeded           false    Dynamic                IPv4                10.0.0.6            mySubnet
     
    Da die Spalte **Public IP** für *IpConfig-3* leer ist, ist derzeit keine öffentliche IP-Adressressource zugeordnet. Sie können IpConfig-3 eine vorhandene öffentliche IP-Adressressource zuordnen oder mit dem folgenden Befehl eine erstellen:

    ```azurecli
    azure network public-ip create --resource-group  myResourceGroup --location westcentralus --name myPublicIP3 --domain-name-label mypublicdns3 --allocation-method Static
    ```
    
    Geben Sie den folgenden Befehl ein, um die öffentliche IP-Adressressource der vorhandenen IP-Konfiguration namens *IPConfig-3* zuzuordnen:
    
    ```azurecli
    azure network nic ip-config set --resource-group myResourceGroup --nic-name myNic1 --name IPConfig-3 --public-ip-name myPublicIP3
    ```


7. Geben Sie den folgenden Befehl ein, um die privaten IP-Adressen und die der NIC zugewiesene öffentliche IP-Adressressource anzuzeigen:

    ```azurecli
    azure network nic ip-config list --resource-group myResourceGroup --nic-name myNic1
    ```
    Eine Ausgabe ähnlich der folgenden sollte angezeigt werden: 
    
        Name               Provisioning state  Primary  Private IP allocation  Private IP version  Private IP address  Subnet    Public IP
        -----------------  ------------------  -------  ---------------------  ------------------  ------------------  --------  -----------
        default-ip-config  Succeeded           true     Dynamic                IPv4                10.0.0.4            mySubnet  myPublicIP
        IPConfig-2         Succeeded           false    Static                 IPv4                10.0.0.5            mySubnet  myPublicIP2
        IPConfig-3         Succeeded           false    Dynamic                IPv4                10.0.0.6            mySubnet  myPublicIP3
     
9. Fügen Sie die IP-Adressen hinzu, die Sie der NIC im Betriebssystem des virtuellen Computers hinzugefügt haben. Befolgen Sie dazu die Anweisungen im Abschnitt [Hinzufügen von IP-Adressen zu einem VM-Betriebssystem](#OsConfig) in diesem Artikel.


<!--HONumber=Nov16_HO3-->


