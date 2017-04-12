---
title: "Testumgebung für Branchenanwendung | Microsoft Docs"
description: "Erfahren Sie, wie Sie eine webbasierte Branchenanwendung in einer Hybrid Cloud-Umgebung für Tests durch IT-Spezialisten oder zu Entwicklungszwecken erstellen."
services: virtual-machines-windows
documentationcenter: 
author: JoeDavies-MSFT
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 92d2d8ce-60ed-4512-95e5-a7fe3b0ca00b
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/30/2016
ms.author: josephd
translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: 8e9a380458bfede80ed0fc1ee7e62b0caec09501
ms.lasthandoff: 03/31/2017


---
# <a name="set-up-a-web-based-lob-application-in-a-hybrid-cloud-for-testing"></a>Einrichten einer webbasierten Branchenanwendung in einer Hybrid Cloud zu Testzwecken
In diesem Thema werden die Schritte zum Erstellen einer simulierten Hybrid Cloud-Umgebung zum Testen einer in Microsoft Azure gehosteten webbasierten Branchenanwendung erläutert. Die resultierende Konfiguration sieht folgendermaßen aus.

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)

Diese Konfiguration umfasst Folgendes:

* Ein simuliertes lokales Netzwerk, das unter Azure gehostet wird (TestLab VNet)
* Ein standortübergreifendes virtuelles und in Azure gehostetes Netzwerk (TestVNET)
* Eine VNet-zu-VNet-VPN-Verbindung
* Einen webbasierten Branchenserver, einen SQL Server-Computer und einen sekundären Domänencontroller im virtuellen Netzwerk „TestVNET“

Diese Konfiguration bietet eine Grundlage und einen allgemeinen Ausgangspunkt für Folgendes:

* Entwickeln und Testen von Branchenanwendungen, die in Internetinformationsdienste (IIS) gehostet werden und über ein SQL Server 2014-Datenbank-Back-End in Azure verfügen
* Ausführen und Testen dieser simulierten Hybrid Cloud-basierten IT-Workload

Die Einrichtung dieser Hybrid Cloud-Testumgebung besteht aus drei Hauptphasen:

1. Einrichten der simulierten Hybrid Cloud-Umgebung
2. Konfigurieren des SQL Server-Computers (SQL1)
3. Konfigurieren den Branchenservers (LOB1)

Für diese Workload ist ein Azure-Abonnement nötig. Wenn Sie ein MSDN- oder Visual Studio-Abonnement besitzen, finden Sie weitere Informationen unter [Monatliche Azure-Gutschrift für Visual Studio-Abonnenten](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

Ein Beispiel für eine in Azure gehostete Produktionsbranchenanwendung finden Sie in der Architekturblaupause für **Branchenanwendungen** unter [Blaupausen für Software-Architekten](http://msdn.microsoft.com/dn630664).

## <a name="phase-1-set-up-the-simulated-hybrid-cloud-environment"></a>Phase 1: Einrichten der simulierten Hybrid Cloud-Umgebung
Erstellen Sie die [simulierte Hybrid Cloud-Testumgebung](ps-hybrid-cloud-test-env-sim.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Da das Vorhandensein des APP1-Servers im Subnetz „Corpnet“ in dieser Testumgebung nicht erforderlich ist, können Sie ihn vorerst herunterfahren.

Die aktuelle Konfiguration sieht folgendermaßen aus.

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph1.png)

## <a name="phase-2-configure-the-sql-server-computer-sql1"></a>Phase 2: Konfigurieren des SQL Server-Computers (SQL1)
Starten Sie gegebenenfalls den DC2-Computer über das Azure-Portal.

Erstellen Sie als Nächstes über eine Azure PowerShell-Eingabeaufforderung auf dem lokalen Computer mit den folgenden Befehlen einen virtuellen Computer für SQL1. Füllen Sie vor dem Ausführen dieser Befehle die Variablenwerte aus, und entfernen Sie die Zeichen < und >.

    $rgName="<your resource group name>"
    $locName="<the Azure location of your resource group>"
    $saName="<your storage account name>"

    $vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
    $pip=New-AzureRMPublicIpAddress -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
    $vm=New-AzureRMVMConfig -VMName SQL1 -VMSize Standard_A4
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-SQLDataDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name "Data" -DiskSizeInGB 100 -VhdUri $vhdURI  -CreateOption empty

    $cred=Get-Credential -Message "Type the name and password of the local administrator account for the SQL Server computer." 
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName SQL1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftSQLServer -Offer SQL2014-WS2012R2 -Skus Standard -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name "OSDisk" -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Verwenden Sie das Azure-Portal zum Herstellen einer Verbindung mit SQL1 mithilfe des lokalen Administratorkontos von SQL1.

Konfigurieren Sie anschließend Windows-Firewallregeln, um das Testen der allgemeinen Konnektivität und SQL Server-Datenverkehr zuzulassen. Führen Sie in der Windows PowerShell-Eingabeaufforderung von SQL1 die folgenden Befehle auf Administratorebene aus.

    New-NetFirewallRule -DisplayName "SQL Server" -Direction Inbound -Protocol TCP -LocalPort 1433,1434,5022 -Action allow 
    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

Der Ping-Befehl sollte zu vier erfolgreichen Antworten von der IP-Adresse 192.168.0.4 führen.

Fügen Sie anschließend den zusätzlichen Datenträger auf SQL1 als neues Volume mit dem Laufwerkbuchstaben „F:“ hinzu.

1. Klicken Sie im linken Bereich des Server-Managers auf **Datei- und Speicherdienste** und anschließend auf **Datenträger**.
2. Klicken Sie im Inhaltsbereich in der Gruppe **Datenträger** auf **Datenträger 2** (wobei **Partition** die Einstellung **Unbekannt** aufweisen muss).
3. Klicken Sie auf **Aufgaben** und anschließend auf **Neues Volume**.
4. Klicken Sie auf der Seite „Voraussetzungen“ des Assistenten für neue Volumes auf **Weiter**.
5. Klicken Sie auf der Seite „Server und Datenträger auswählen“ auf **Datenträger 2** und anschließend auf **Weiter**. Wenn Sie dazu aufgefordert werden, klicken Sie auf **OK**.
6. Klicken Sie auf der Seite „Geben Sie die Größe des Volumes an“ auf **Weiter**.
7. Klicken Sie auf der Seite „Einem Laufwerkbuchstaben oder Ordner zuweisen“ auf **Weiter**.
8. Klicken Sie auf der Seite „Dateisystemeinstellungen auswählen“ auf **Weiter**.
9. Klicken Sie auf der Seite „Auswahl bestätigen“ auf **Erstellen**.
10. Wenn Sie fertig sind, klicken Sie auf **Schließen**.

Führen Sie die folgenden Befehle in der Windows PowerShell-Eingabeaufforderung auf SQL1 aus:

    md f:\Data
    md f:\Log
    md f:\Backup

Verknüpfen Sie SQL1 als Nächstes mit der Windows Server Active Directory-Domäne CORP, indem Sie diese Befehle an der Windows PowerShell-Eingabeaufforderung auf SQL1 verwenden.

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

Verwenden Sie das Konto „CORP\User1“, wenn Sie aufgefordert werden, für den Befehl **Add-Computer** Anmeldeinformationen für ein Domänenkonto anzugeben.

Verwenden Sie nach dem Neustart das Azure-Portal zum Herstellen einer Verbindung mit SQL1 mithilfe des *lokalen Administratorkontos von SQL1*.

Konfigurieren Sie anschließend SQL Server 2014 so, dass das Laufwerk F: für neue Datenbanken und Berechtigungen für Benutzerkonten verwendet werden kann.

1. Geben Sie auf dem Startbildschirm **SQL Server Management** ein, und klicken Sie auf **SQL Server 2014 Management Studio**.
2. Klicken Sie unter **Mit Server verbinden** auf **Verbinden**.
3. Klicken Sie im Strukturbereich "Objekt-Explorer" mit der rechten Maustaste auf **SQL1** **Eigenschaften**.
4. Klicken Sie im Fenster **Servereigenschaften** auf **Datenbankeinstellungen**.
5. Suchen Sie den Eintrag **Standardspeicherorte für Datenbank** , und legen Sie die folgenden Werte fest: 
   * Geben Sie für **Daten** den Pfad **f:\Data** ein.
   * Geben Sie für **Protokoll** den Pfad **f:\Log** ein.
   * Geben Sie für **Sicherung** den Pfad **f:\Backup** ein.
   * Hinweis: Nur neue Datenbanken verwenden diese Speicherorte.
6. Klicken Sie auf **OK** , um das Fenster zu schließen.
7. Öffnen Sie im Strukturbereich **Objekt-Explorer** den Abschnitt **Sicherheit**.
8. Klicken Sie mit der rechten Maustaste auf **Anmeldungen**, und klicken Sie dann auf **Neue Anmeldung**.
9. Geben Sie in das Feld **Anmeldename** den Namen **CORP\User1** ein.
10. Klicken Sie auf der Seite **Serverrollen** auf **Sysadmin** und anschließend auf **OK**.
11. Schließen Sie Microsoft SQL Server Management Studio.

Die aktuelle Konfiguration sieht folgendermaßen aus.

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph2.png)

## <a name="phase-3-configure-the-lob-server-lob1"></a>Phase 3: Konfigurieren den Branchenservers (LOB1)
Führen Sie als Erstes auf dem lokalen Computer die folgenden Befehle in der Azure PowerShell-Eingabeaufforderung aus, um einen virtuellen Computer für LOB1 zu erstellen.

    $rgName="<your resource group name>"
    $locName="<your Azure location, such as West US>"
    $saName="<your storage account name>"

    $vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
    $pip=New-AzureRMPublicIpAddress -Name LOB1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name LOB1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
    $vm=New-AzureRMVMConfig -VMName LOB1 -VMSize Standard_A2
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for LOB1."
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName LOB1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/LOB1-TestLab-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name LOB1-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Verwenden Sie als Nächstes das Azure-Portal zum Herstellen einer Verbindung mit LOB1 mithilfe der Anmeldeinformationen des lokalen Administratorkontos von LOB1.

Konfigurieren Sie anschließend eine Windows-Firewall-Regel, um Datenverkehr zum Testen der allgemeinen Konnektivität zuzulassen. Führen Sie auf LOB1 in einer Windows PowerShell-Eingabeaufforderung auf Administratorebene die folgenden Befehle aus.

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

Der Ping-Befehl sollte zu vier erfolgreichen Antworten von der IP-Adresse 192.168.0.4 führen.

Verknüpfen Sie LOB1 als Nächstes mit der Active Directory-Domäne CORP, indem Sie diese Befehle bei der Windows PowerShell-Eingabeaufforderung verwenden.

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

Verwenden Sie das Konto „CORP\User1“, wenn Sie aufgefordert werden, für den Befehl **Add-Computer** Anmeldeinformationen für ein Domänenkonto anzugeben.

Verwenden Sie nach dem Neustart das Azure-Portal zum Herstellen einer Verbindung mit LOB1 mithilfe des Kontos „CORP\User1“ und des Kennworts.

Konfigurieren Sie dann LOB1 für IIS, und testen Sie den Zugriff von CLIENT1.

1. Klicken Sie im Server-Manager auf **Rollen und Features hinzufügen**.
2. Klicken Sie auf der Seite **Voraussetzungen** auf **Weiter**.
3. Klicken Sie auf der Seite **Installationstyp auswählen** auf **Weiter**.
4. Klicken Sie auf der Seite **Zielserver auswählen** auf **Weiter**.
5. Klicken Sie auf der Seite **Serverrollen** in der Liste **Rollen** auf **Webserver (IIS)**.
6. Klicken Sie bei der entsprechenden Aufforderung auf **Features hinzufügen** und anschließend auf **Weiter**.
7. Klicken Sie auf der Seite **Features auswählen** auf **Weiter**.
8. Klicken Sie auf der Seite **Webserver (IIS)**auf **Weiter**.
9. Aktivieren oder deaktivieren Sie auf der Seite **Rollendienste auswählen** die Kontrollkästchen für die Dienste, die Sie zum Testen der Branchenanwendung benötigen, und klicken Sie dann auf **Weiter**.
10. Klicken Sie auf der Seite **Installationsauswahl bestätigen** auf **Installieren**.
11. Warten Sie, bis die Installation der Komponenten abgeschlossen ist, und klicken Sie dann auf **Schließen**.
12. Stellen Sie über das Azure-Portal eine Verbindung mit dem Computer CLIENT1 mit den Anmeldeinformationen des Kontos „CORP\User1“ her, und starten Sie anschließend Internet Explorer.
13. Geben Sie in die Adressleiste **http://lob1/** ein, und drücken Sie dann die EINGABETASTE. Die Standardwebseite für IIS 8 sollte angezeigt werden.

Die aktuelle Konfiguration sieht folgendermaßen aus.

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)

Diese Umgebung kann jetzt zum Bereitstellen der webbasierten Anwendung auf LOB1 und für Funktionstests von CLIENT1 aus dem Subnetz „Corpnet“ verwendet werden.

## <a name="next-step"></a>Nächster Schritt
* Hinzufügen eines neuen virtuellen Computers über das [Azure-Portal](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)


