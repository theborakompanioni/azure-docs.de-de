<properties 
	pageTitle="Testumgebung für SharePoint 2013-Farm | Microsoft Azure" 
	description="Erfahren Sie, wie Sie eine zweistufige SharePoint Server 2013-Intranetfarm in einer Hybrid Cloud-Umgebung für IT-Experten oder Entwicklungstests erstellen." 
	services="virtual-machines-windows" 
	documentationCenter="" 
	authors="JoeDavies-MSFT" 
	manager="timlt" 
	editor=""
	tags="azure-resource-manager"/>

<tags 
	ms.service="virtual-machines-windows" 
	ms.workload="infrastructure-services" 
	ms.tgt_pltfrm="vm-windows" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="04/01/2016" 
	ms.author="josephd"/>

# Einrichten einer SharePoint-Intranetfarm in einer Hybrid Cloud zu Testzwecken

In diesem Thema lernen Sie Schritt für Schritt, wie Sie eine Hybrid Cloud-Umgebung zum Testen einer in Microsoft Azure gehosteten Intranet-Sharepoint-Farm erstellen. Hier sehen Sie die daraus resultierende Konfiguration.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sp/virtual-machines-windows-ps-hybrid-cloud-test-env-sp-ph3.png)
 
Mit dieser Konfiguration wird ein SharePoint in einer Azure-Produktionsumgebung an Ihrem Speicherort im Internet simuliert. Sie besteht aus:

- einem vereinfachten lokalen Netzwerk (dem Corpnet-Subnetz).
- einem standortübergreifenden virtuellen Netzwerk (TestVNET), das in Azure gehostet wird.
- einer Site-to-Site-VPN-Verbindung.
- einer zwei-Ebenen-SharePoint-Farm und einem sekundären Domänencontroller im virtuellen Netzwerk TestVNET.

Diese Konfiguration bietet eine Grundlage und einen allgemeinen Ausgangspunkt für Folgendes:

- Entwickeln und Testen von Anwendungen auf einer SharePoint-Intranetfarm in einer Hybrid Cloud-Umgebung.
- Ausführen und Testen dieses Hybrid Cloud-basierten IT-Workloads.

Die Einrichtung dieser Hybrid Cloud-Testumgebung besteht aus drei Hauptphasen:

1.	Einrichten der Hybrid Cloud-Umgebung zu Testzwecken
2.	Konfigurieren des SQL Server-Computers (SQL1)
3.	Konfigurieren des SharePoint-Servers (SP1).

Für diese Workload ist ein Azure-Abonnement nötig. Wenn Sie ein MSDN- oder Visual Studio-Abonnement besitzen, finden Sie weitere Informationen unter [Monatliche Azure-Gutschrift für Visual Studio-Abonnenten](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

## Phase 1: Einrichten der Hybridcloudumgebung

Folgen Sie den Anweisungen im Thema [Einrichten einer Hybridcloudumgebung zu Testzwecken](virtual-machines-windows-ps-hybrid-cloud-test-env-base.md). Da das Vorhandensein des APP1-Servers im Subnetz des Unternehmensnetzwerks in dieser Testumgebung nicht erforderlich ist, können Sie jetzt herunterfahren.

Die aktuelle Konfiguration sieht folgendermaßen aus.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sp/virtual-machines-windows-ps-hybrid-cloud-test-env-sp-ph1.png)

> [AZURE.NOTE] Für Phase 1 können Sie auch die [simulierte Hybrid Cloud-Testumgebung](virtual-machines-windows-ps-hybrid-cloud-test-env-sim.md) einrichten.
 
## Phase 2: Konfigurieren des SQL Server-Computers (SQL1)

Starten Sie gegebenenfalls den DC2-Computer über das Azure-Portal.

Stellen Sie über das Azure-Portal eine Verbindung mit DC2 mithilfe der „CORP\\User1“-Anmeldeinformationen her.

Erstellen Sie dann ein SharePoint-Farm-Administratorkonto. Öffnen Sie auf DC2 eine Windows PowerShell-Eingabeaufforderung auf Administratorebene, und führen Sie den folgenden Befehl aus.

	New-ADUser -SamAccountName SPFarmAdmin -AccountPassword (read-host "Set user password" -assecurestring) -name "SPFarmAdmin" -enabled $true -ChangePasswordAtLogon $false

Wenn Sie dazu aufgefordert werden, geben Sie das Kennwort für das Konto „SPFarmAdmin“ an. Wählen Sie ein sicheres Kennwort, und hinterlegen Sie es an einem sicheren Ort.

Erstellen Sie dann in der Azure PowerShell-Eingabeaufforderung auf dem lokalen Computer mit den folgenden Befehlen einen virtuellen Azure-Computer für SQL1. Füllen Sie vor dem Ausführen dieser Befehle die Variablenwerte aus, und entfernen Sie die Zeichen < and >.

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

Verwenden Sie danach das Azure-Portal zum Herstellen einer Verbindung mit SQL1 mithilfe des lokalen Administratorkontos.

Konfigurieren Sie anschließend Windows-Firewall-Regeln, um Datenverkehr zum Testen der allgemeinen Konnektivität und von SQL Server zuzulassen. Führen Sie in der Windows PowerShell-Eingabeaufforderung von SQL1 die folgenden Befehle auf Administratorebene aus.

	New-NetFirewallRule -DisplayName "SQL Server" -Direction Inbound -Protocol TCP -LocalPort 1433,1434,5022 -Action allow 
	Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
	ping dc2.corp.contoso.com

Der Ping-Befehl sollte zu vier erfolgreichen Antworten von der IP-Adresse 192.168.0.4 führen.

Fügen Sie anschließend den zusätzlichen Datenträger als neues Volume mit dem Laufwerkbuchstaben F: hinzu.

1.	Klicken Sie im linken Bereich des Server-Managers auf **Datei- und Speicherdienste** und anschließend auf **Datenträger**.
2.	Klicken Sie im Inhaltsbereich in der Gruppe **Datenträger** auf **Datenträger 2** (wobei **Partition** die Einstellung **Unbekannt** aufweisen muss).
3.	Klicken Sie auf **Aufgaben** und anschließend auf **Neues Volume**.
4.	Klicken Sie auf der Seite „Voraussetzungen“ des Assistenten für neue Volumes auf **Weiter**.
5.	Klicken Sie auf der Seite „Server und Datenträger auswählen“ auf **Datenträger 2** und anschließend auf **Weiter**. Wenn Sie dazu aufgefordert werden, klicken Sie auf **OK**.
6.	Klicken Sie auf der Seite „Geben Sie die Größe des Volumes an“ auf **Weiter**.
7.	Klicken Sie auf der Seite „Einem Laufwerkbuchstaben oder Ordner zuweisen“ auf **Weiter**.
8.	Klicken Sie auf der Seite „Dateisystemeinstellungen auswählen“ auf **Weiter**.
9.	Klicken Sie auf der Seite „Auswahl bestätigen“ auf **Erstellen**.
10.	Wenn Sie fertig sind, klicken Sie auf **Schließen**.

Führen Sie die folgenden Befehle in der Windows PowerShell-Eingabeaufforderung auf SQL1 aus:

	md f:\Data
	md f:\Log
	md f:\Backup

Verknüpfen Sie SQL1 als Nächstes mit der Active Directory-Domäne CORP, indem Sie diese Befehle bei der Windows PowerShell-Eingabeaufforderung verwenden.

	Add-Computer -DomainName corp.contoso.com
	Restart-Computer

Verwenden Sie das Konto „CORP\\User1“, wenn Sie aufgefordert werden, für den Befehl **Add-Computer** Anmeldeinformationen für ein Domänenkonto anzugeben.

Verwenden Sie nach dem Neustart das Azure-Portal zum Herstellen einer Verbindung mit SQL1 mithilfe des *lokalen Administratorkontos*.

Konfigurieren Sie anschließend SQL Server 2014 so, dass das Laufwerk F: für neue Datenbanken und Berechtigungen für Benutzerkonten verwendet werden kann.

1.	Geben Sie auf dem Startbildschirm **SQL Server Management** ein, und klicken Sie auf **SQL Server 2014 Management Studio**.
2.	Klicken Sie unter **Mit Server verbinden** auf **Verbinden**.
3.	Klicken Sie im Strukturbereich "Objekt-Explorer" mit der rechten Maustaste auf **SQL1**, und klicken Sie anschließend auf **Eigenschaften**.
4.	Klicken Sie im Fenster **Servereigenschaften** auf **Datenbankeinstellungen**.
5.	Suchen Sie den Eintrag **Standardspeicherorte für Datenbank**, und legen Sie die folgenden Werte fest: 
	- Geben Sie für **Daten** den Pfad **f:\\Data** ein.
	- Geben Sie für **Protokoll** den Pfad **f:\\Log** ein.
	- Geben Sie für **Sicherung** den Pfad **f:\\Backup** ein.
	- Beachten Sie, dass diese Speicherorte nur für die neuen Datenbanken verwendet werden.
6.	Klicken Sie auf **OK**, um das Fenster zu schließen.
7.	Öffnen Sie im Strukturbereich **Objekt-Explorer** den Abschnitt **Sicherheit**.
8.	Klicken Sie mit der rechten Maustaste auf **Anmeldungen**, und klicken Sie dann auf **Neue Anmeldung**.
9.	Geben Sie in das Feld **Anmeldename** den Namen **CORP\\User1** ein.
10.	Klicken Sie auf der Seite **Serverrollen** auf **Sysadmin** und anschließend auf **OK**.
11.	Klicken Sie im Strukturbereich **Objekt-Explorer** mit der rechten Maustaste auf **Anmeldungen** und anschließend auf **Neue Anmeldung**.
12.	Geben Sie auf der Seite **Allgemein** unter **Anmeldename** den Namen **CORP\\SPFarmAdmin** ein.
13.	Wählen Sie auf der Seite **Serverrollen** die Option **dbcreator** aus, und klicken Sie dann auf **OK**.
14.	Schließen Sie Microsoft SQL Server Management Studio.

Die aktuelle Konfiguration sieht folgendermaßen aus.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sp/virtual-machines-windows-ps-hybrid-cloud-test-env-sp-ph2.png)

## Phase 3: Konfigurieren des SharePoint-Servers (SP1)

Erstellen Sie zunächst in der Eingabeaufforderung von Azure PowerShell auf dem lokalen Computer mit den folgenden Befehlen einen virtuellen Azure-Computer für SP1.

	$rgName="<your resource group name>"
	$locName="<the Azure location of your resource group>"
	$saName="<your storage account name>"
	
	$vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
	$subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
	$pip=New-AzureRMPublicIpAddress -Name SP1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
	$nic=New-AzureRMNetworkInterface -Name SP1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
	$vm=New-AzureRMVMConfig -VMName SP1 -VMSize Standard_A3
	$cred=Get-Credential -Message "Type the name and password of the local administrator account for the SharePoint 2013 server." 
	$vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName SP1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
	$vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftSharePoint -Offer MicrosoftSharePointServer -Skus 2013 -Version "latest"
	$vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
	$storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
	$osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SP1-OSDisk.vhd"
	$vm=Set-AzureRMVMOSDisk -VM $vm -Name "OSDisk" -VhdUri $osDiskUri -CreateOption fromImage
	New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Verwenden Sie als Nächstes das Azure-Portal zum Herstellen einer Verbindung mit dem virtuellen SP1-Computer mithilfe der Anmeldeinformationen des lokalen Administratorkontos.

Konfigurieren Sie anschließend eine Windows-Firewall-Regel, um Datenverkehr zum Testen der allgemeinen Konnektivität zuzulassen. Führen Sie die folgenden Befehle in der Windows PowerShell-Eingabeaufforderung auf SP1 aus:

	Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
	ping dc2.corp.contoso.com

Der Ping-Befehl sollte zu vier erfolgreichen Antworten von der IP-Adresse 192.168.0.4 führen.

Verknüpfen Sie SP1 als Nächstes mit der Active Directory-Domäne „CORP“, indem Sie diese Befehle bei der Windows PowerShell-Eingabeaufforderung verwenden.

	Add-Computer -DomainName corp.contoso.com
	Restart-Computer

Verwenden Sie das Konto „CORP\\User1“, wenn Sie aufgefordert werden, für den Befehl **Add-Computer** Anmeldeinformationen für ein Domänenkonto anzugeben.

Verwenden Sie nach dem Neustart das Azure-Portal zum Herstellen einer Verbindung mit SP1 mithilfe des Kontos „CORP\\User1“ und des Kennworts.

Konfigurieren Sie anschließend SP1 für eine neue SharePoint-Farm und eine Standard-Teamwebsite.

1.	Geben Sie auf dem Startbildschirm **SharePoint 2013-Produkte** ein, und klicken Sie dann auf **Konfigurations-Assistent für SharePoint 2013-Produkte**. Klicken Sie auf die Frage, ob das Programm Änderungen am Computer vornehmen darf, auf **Ja**.
2.	Klicken Sie auf der Seite „Willkommen bei den SharePoint-Produkten“ auf **Weiter**. 
3.	Wenn ein Dialogfeld mit der Meldung angezeigt wird, dass während der Konfiguration einige Dienste möglicherweise neu gestartet werden müssen, klicken Sie auf **Ja**.
4.	Klicken Sie auf der Seite "Verbindung mit einer Serverfarm herstellen" auf **Eine neue Serverfarm erstellen**, und klicken Sie dann auf **Weiter**.
5.	Geben Sie auf der Seite "Einstellungen für die Konfigurationsdatenbank angeben" **sql1.corp.contoso.com** unter **Datenbankserver** ein, geben Sie unter **Benutzername** den Namen **CORP\\SPFarmAdmin** ein, geben Sie unter **Kennwort** das Kennwort für das Konto "SPFarmAdmin" ein, und klicken Sie dann auf **Weiter**.
6.	Geben Sie auf der Seite "Farmsicherheitseinstellungen angeben" sowohl unter **Passphrase** als auch unter **Passphrase bestätigen** die Buchstabenkombination **P@ssphrase** ein, und klicken Sie dann auf **Weiter**.
7.	Klicken Sie auf der Seite „Webanwendung für die SharePoint-Zentraladministration konfigurieren“ auf **Weiter**.
8.	Klicken Sie auf der Seite "Der Konfigurations-Assistent für SharePoint-Produkte wird abgeschlossen" auf **Weiter**. Das Abschließen des Konfigurations-Assistenten für SharePoint-Produkte kann einige Minuten dauern.
9.	Klicken Sie auf der Seite „Konfiguration erfolgreich“ auf **Fertig stellen**. Nach dem Abschluss wird Internet Explorer mit einer Registerkarte namens „Assistent für die Farmerstkonfiguration“ gestartet.
10.	Klicken Sie im Dialogfeld **Helfen Sie uns bei der Verbesserung von SharePoint** auf **Nein, ich möchte nicht teilnehmen** und anschließend auf **OK**.
11.	Klicken Sie unter **Wie möchten Sie die SharePoint-Farm konfigurieren?** auf **Assistenten starten**.
12.	Klicken Sie auf der Seite "SharePoint-Farm konfigurieren" unter **Dienstkonto** auf **Vorhandenes verwaltetes Konto verwenden**.
13.	Deaktivieren Sie unter **Dienste** alle Kontrollkästchen außer demjenigen neben **Statusdienst**, und klicken Sie dann auf **Weiter**. Vor der Fertigstellung könnte für eine Weile die Seite „In Bearbeitung“ angezeigt werden.
14.	Geben Sie auf der Seite "Websitesammlung erstellen" unter **Titel und Beschreibung** > **Titel** den Titel **Contoso Corporation** ein, geben Sie die URL **http://sp1**/ an, und klicken Sie dann auf **OK**. Vor der Fertigstellung könnte für eine Weile die Seite „In Bearbeitung“ angezeigt werden. Mit diesem Schritt wird eine Teamwebsite mit der URL http://sp1 erstellt.
15.	Klicken Sie auf der Seite "Farmkonfigurations-Assistent" auf **Fertig stellen**. Auf der Registerkarte in Internet Explorer wird die Website „SharePoint 2013-Zentraladministration“ angezeigt.
16.	Melden Sie sich am Computer CLIENT1 mit den Anmeldeinformationen des Kontos „CORP\\User1“ an, und starten Sie Internet Explorer.
17.	Geben Sie in die Adressleiste **http://sp1/** ein, und drücken Sie dann die EINGABETASTE. Nun sollte die SharePoint-Teamwebsite angezeigt werden. Eventuell dauert das Rendern der Seite ein wenig.

Die aktuelle Konfiguration sieht folgendermaßen aus.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sp/virtual-machines-windows-ps-hybrid-cloud-test-env-sp-ph3.png)
 
Ihre SharePoint-Intranetfarm kann jetzt in einer Hybrid Cloud-Umgebung getestet werden.

## Nächster Schritt

- [Konfigurieren](https://technet.microsoft.com/library/ee836142.aspx) der SharePoint-Farm

<!---HONumber=AcomDC_0601_2016-->