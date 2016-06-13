<properties 
	pageTitle="Testumgebung für Office 365 DirSync | Microsoft Azure" 
	description="Erfahren Sie, wie für ein Office 365-Verzeichnissynchronisierung (DirSync)-Serverkonfiguration in einer hybridcloud für IT Pro oder Entwicklung zu testen." 
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

# Office 365-Verzeichnissynchronisierung (DirSync) in einer hybridcloud zu Testzwecken einrichten
 
Dieses Thema führt Sie durch die Erstellung einer hybriden Cloud-Umgebung zum Testen von Office 365-Verzeichnissynchronisierung (DirSync) mit Kennwortsynchronisierung in Microsoft Azure gehostet. Hier sehen Sie die daraus resultierende Konfiguration.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-dirsync/virtual-machines-windows-ps-hybrid-cloud-test-env-dirsync-ph3.png)
 
Diese Konfiguration simuliert einen Dirsync-Server in Azure-Produktionsumgebung von Ihrem Speicherort im Internet. Sie besteht aus:

- einem vereinfachten lokalen Netzwerk (dem Corpnet-Subnetz).
- Einem standortübergreifenden virtuellen in Azure gehosteten Netzwerk (TestVNET)
- einer Site-to-Site-VPN-Verbindung.
- Ein Office 365 Fast Track-Testabonnement.
- Ein DirSync-Server, der das Azure AD Connect-Tool ausführt, und ein sekundärer Domänencontroller im virtuellen Netzwerk TestVNET.

Diese Konfiguration bietet eine Grundlage und einen allgemeinen Ausgangspunkt für Folgendes:

- Entwickeln und Testen von Anwendungen für Office 365, die von der Synchronisierung mit einer lokalen Active Directory-Domäne mithilfe der Kennwortsynchronisierung abhängen.
- Testen dieses cloudbasierten IT-Workloads

Die Einrichtung dieser Hybrid Cloud-Testumgebung besteht aus drei Hauptphasen:

1.	Einrichten der Hybrid Cloud-Umgebung zu Testzwecken
2.	Konfigurieren Sie die Testversion von Office 365 Fast Track.
3.	Konfigurieren Sie den DirSync-Server (DS1).

Wenn Sie noch kein Azure-Abonnement besitzen, können Sie sich unter [Azure ausprobieren](https://azure.microsoft.com/pricing/free-trial/) für ein kostenloses Konto registrieren. Wenn Sie ein MSDN- oder Visual Studio-Abonnement besitzen, finden Sie weitere Informationen unter [Monatliche Azure-Gutschrift für Visual Studio-Abonnenten](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

## Phase 1: Einrichten der Hybridcloudumgebung

Folgen Sie den Anweisungen im Thema [Einrichten einer Hybridcloudumgebung zu Testzwecken](virtual-machines-windows-ps-hybrid-cloud-test-env-base.md). Da das Vorhandensein des APP1-Servers im Subnetz des Unternehmensnetzwerks in dieser Testumgebung nicht erforderlich ist, können Sie jetzt herunterfahren.

Die aktuelle Konfiguration sieht folgendermaßen aus.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-dirsync/virtual-machines-windows-ps-hybrid-cloud-test-env-dirsync-ph1.png)

> [AZURE.NOTE] Für Phase 1 können Sie auch die [simulierte Hybrid Cloud-Testumgebung](virtual-machines-windows-ps-hybrid-cloud-test-env-sim.md) einrichten.

## Phase 2: Konfigurieren der Testversion von Office 365 Fast Track

Um Ihre Testversion von Office 365 Fast Track zu starten, benötigen Sie einen fiktiven Firmennamen und ein Microsoft-Konto. Wir empfehlen Ihnen, dass Sie eine Variante des Unternehmensnamens Contoso für den Namen Ihres Unternehmens verwenden. Das ist ein fiktives Unternehmen, das in Microsoft-Beispielinhalten verwendet wird, wobei dies aber nicht unbedingt erforderlich ist.

Melden Sie sich als Nächstes ein neues Microsoft-Konto an. Wechseln Sie zu ****http://outlook.com**, und erstellen Sie ein Konto mit einer E-Mail-Adresse wie user123@outlook.com. Sie können für eine Testversion von Office 365 Fast Track mit diesem Konto anmelden.

Melden Sie sich als Nächstes für eine neue Office 365 Enterprise E3-Testversion an.

1.	Melden Sie sich mit den Anmeldeinformationen des CORP\\User1 CLIENT1 an.
2.	Öffnen Sie Internet Explorer, und wechseln Sie zu ****https://go.microsoft.com/fwlink/p/?LinkID=403802**.
3.	Melden Sie sich für die Testversion von Office 365 Enterprise E3 an.

Wenn Sie zur Eingabe Ihrer **geschäftlichen E-Mail-Adresse** aufgefordert werden, geben Sie Ihr neues Microsoft-Konto an.

Wenn Sie zum Erstellen einer ID aufgefordert werden, geben Sie den Namen eines anfänglichen Office 365-Kontos, den fiktiven Firmennamen und ein Kennwort ein. Notieren Sie die zurückgegebene E-Mail-Adresse (z. B. user123@contoso123.onmicrosoft.com), und bewahren Sie sie mit dem Kennwort sicher auf. Sie benötigen diese Informationen, um die Azure AD Connect-Konfiguration in Phase 3 abzuschließen.

Danach sollte die Hauptseite des Office 365-Portals angezeigt werden. Klicken Sie im oberen Menüband auf **Admin**, und klicken Sie dann auf **Office 365**. Die Office 365 Admin Center-Seite angezeigt wird. Lassen Sie auf dieser Seite auf "client1" geöffnet.

Die aktuelle Konfiguration sieht folgendermaßen aus.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-dirsync/virtual-machines-windows-ps-hybrid-cloud-test-env-dirsync-ph2.png)

## Phase 3: Konfigurieren des DirSync-Servers (DS1)

Starten Sie gegebenenfalls den DC2-Computer über das Azure-Portal.

Erstellen Sie als Nächstes einen virtuellen Azure-Computer für DS1 mit diesen Befehlen bei der Azure PowerShell-Eingabeaufforderung auf dem lokalen Computer. Füllen Sie vor dem Ausführen dieser Befehle die Variablenwerte aus, und entfernen Sie die Zeichen < and >.

	$rgName="<your resource group name>"
	$locName="<your Azure location, such as West US>"
	$saName="<your storage account name>"
	
	$vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
	$subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
	$pip=New-AzureRMPublicIpAddress -Name DS1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
	$nic=New-AzureRMNetworkInterface -Name DS1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
	$vm=New-AzureRMVMConfig -VMName DS1 -VMSize Standard_A1
	$storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
	$cred=Get-Credential -Message "Type the name and password of the local administrator account for DS1."
	$vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName DS1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
	$vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
	$vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
	$osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DS1-TestLab-OSDisk.vhd"
	$vm=Set-AzureRMVMOSDisk -VM $vm -Name DS1-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
	New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Verwenden Sie als Nächstes das Azure-Portal zum Herstellen einer Verbindung mit dem DS1 (virtueller Computer) mithilfe der Anmeldeinformationen des lokalen Administratorkontos.

Konfigurieren Sie anschließend eine Windows-Firewall-Regel, um Datenverkehr zum Testen der allgemeinen Konnektivität zuzulassen. Ein auf Windows PowerShell-Eingabeaufforderung auf DS1 führen Sie diese Befehle.

	Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
	ping dc2.corp.contoso.com

Der Ping-Befehl sollte zu vier erfolgreichen Antworten von der IP-Adresse 192.168.0.4 führen.

Verknüpfen Sie DS1 als Nächstes mit der Active Directory-Domäne CORP, indem Sie diese Befehle bei der Windows PowerShell-Eingabeaufforderung verwenden.

	Add-Computer -DomainName corp.contoso.com
	Restart-Computer

Verwenden Sie das Konto „CORP\\User1“, wenn Sie aufgefordert werden, für den Befehl **Add-Computer** Anmeldeinformationen für ein Domänenkonto anzugeben.

Verwenden Sie nach dem Neustart das Azure-Portal zum Herstellen einer Verbindung mit DS1 mithilfe des Kontos „CORP\\User1“ und des Kennworts.

Installieren Sie als Nächstes .NET 3.5 auf DS1 mit dem folgenden Befehl an einer Windows PowerShell-Eingabeaufforderung auf Administratorebene.

	Add-WindowsFeature NET-Framework-Core

Aktivieren Sie als Nächstes Verzeichnissynchronisierung für Ihre Office 365 Fast Track-Testversion.

1.	Klicken Sie auf CLIENT1 auf der Seite **Office 365 Admin Center** im linken Bereich auf **Benutzer**, und klicken Sie dann auf **Aktive Benutzer**.
2.	Klicken Sie unter **Active Directory-Synchronisierung** auf **Einrichten**.
3.	Klicken Sie auf der Seite "Active Directory-Synchronisierung einrichten und verwalten" unter Schritt 3 auf **Aktivieren**.
4.	Wenn **Möchten Sie die Active Directory-Synchronisierung aktivieren?** angezeigt wird, klicken Sie auf **Aktivieren**. Danach wird **Active Directory-Synchronisierung ist aktiviert** in Schritt 3 angezeigt.
5.	Lassen Sie die Seite **Active Directory-Synchronisierung einrichten und verwalten** auf CLIENT1 geöffnet.

Führen Sie als Nächstes bei der Windows PowerShell-Eingabeaufforderung auf DC1 diese Befehle **nacheinander** aus, um eine neue Organisationseinheit namens "contoso\_users" zu erstellen. Fügen Sie zudem zwei neue Benutzerkonten für Marci Kaufman und Lynda Meyer hinzu.

	New-ADOrganizationalUnit -Name contoso_users -Path "DC=corp,DC=contoso,DC=com"
	New-ADUser -SamAccountName marcik -AccountPassword (Read-Host "Set user password" -AsSecureString) -name "Marci Kaufman" -enabled $true -PasswordNeverExpires $true -ChangePasswordAtLogon $false -Path "OU=contoso_users,DC=corp,DC=contoso,DC=com"
	New-ADUser -SamAccountName lyndam -AccountPassword (Read-Host "Set user password" -AsSecureString) -name "Lynda Meyer" -enabled $true -PasswordNeverExpires $true -ChangePasswordAtLogon $false -Path "OU=contoso_users,DC=corp,DC=contoso,DC=com"

Wenn Sie jeden Windows PowerShell-Befehl **New-ADUser** ausführen, werden Sie zur Eingabe des Kennworts des neuen Benutzers aufgefordert. Diese Kennwörter zu erfassen und an einem sicheren Ort aufbewahren. Diese werden später benötigt werden.

Installieren und konfigurieren Sie als Nächstes das Azure AD Connect-Tool auf DS1.

1.	Führen Sie Internet Explorer aus, geben Sie ****https://www.microsoft.com/download/details.aspx?id=47594** in der **Adressleiste** ein, und betätigen Sie dann die EINGABETASTE.
2.	Führen Sie das Microsoft Azure AD Connect-Setupprogramm aus.
3.	Doppelklicken Sie auf dem Desktop auf ** Azure AD Connect**.
4.	Wählen Sie auf der Seite **Willkommen** die Option **Ich stimme den Lizenzbedingungen und dem Datenschutzhinweis zu**, und klicken Sie dann auf **Weiter**.
5.	Klicken Sie auf der Seite **Expresseinstellungen** auf **Expresseinstellungen verwenden**.
6.	Geben Sie auf der Seite **Herstellen einer Verbindung mit Azure AD** die E-Mail-Adresse und das Kennwort des anfänglichen Kontos ein, das Sie beim Einrichten der Testversion von Office 365 Fast Track in Phase 2 erstellt haben. Klicken Sie auf **Weiter**.
7.	Geben Sie auf der Seite **Herstellen einer Verbindung mit AD DS** den Namen **CORP\\User1** unter **Benutzername** und das User1-Kontokennwort unter **Kennwort** ein. Klicken Sie auf Weiter.
8.	Überprüfen Sie auf der Seite **Bereit zum Konfigurieren** die Einstellungen, und klicken Sie dann auf **Installieren**.
9.	Klicken Sie auf der Seite **Konfiguration abgeschlossen** auf **Beenden**.

Als Nächstes stellen Sie sicher, dass die Benutzerkonten in der CORP-Domäne zu Office 365 synchronisiert werden. Beachten Sie, dass es möglicherweise ein paar Minuten dauert, bis die Synchronisierung erfolgt.

Klicken Sie auf CLIENT1 auf der Seite **Active Directory-Synchronisierung einrichten und verwalten** auf den Link **Benutzer** in Schritt 6 auf der Seite. Wenn die Verzeichnissynchronisierung erfolgreich war, sollte etwa wie folgt angezeigt werden.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-dirsync/virtual-machines-windows-ps-hybrid-cloud-test-env-dirsync-example.png)

Die Spalte **Status** gibt an, dass das Konto über die Synchronisierung mit einer Active Directory-Domäne abgerufen wurde.

Führen Sie dann die Office 365-Kennwortsynchronisierung mit dem Active Directory-Konto „Lynda Myer“ aus.

1.	Wählen Sie auf CLIENT1 auf der Seite **Aktive Benutzer** das Konto **Lynda Meyer** aus.
2.	Klicken Sie in den Eigenschaften des Kontos "Lynda Meyer" unter **Zugewiesene Lizenz** auf **Bearbeiten**.
3.	Wählen Sie auf der Registerkarte **Lizenz zuweisen** einen Standort unter **Benutzerstandort einstellen** aus (z. B. Vereinigte Staaten).
4.	Wählen Sie **Microsoft Office 365 (Plan E3)** aus, und klicken Sie dann auf **Speichern**.
5.	Schließen Sie Internet Explorer.
6.	Führen Sie Internet Explorer aus, und wechseln Sie zu ****http://portal.microsoftonline.com**.
7.	Melden Sie sich mit den Office 365-Anmeldeinformationen von Lynda Meyer an. Der Benutzername lautet dann "lyndam@<*Ihr fiktiver Name*>.onmicrosoft.com". Das Kennwort ist das Kennwort für das Lynda Dressler Active Directory-Benutzerkonto.
8.	Nach der erfolgreichen Anmeldung sehen Sie die Hauptseite des Office 365-Portals mit **Heute machen wir einen Unterschied**.

Die aktuelle Konfiguration sieht folgendermaßen aus.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-dirsync/virtual-machines-windows-ps-hybrid-cloud-test-env-dirsync-ph3.png)
 
Diese Umgebung ist nun bereit für Sie zum Testen des Office 365-Anwendungen, die auf Office 365 DirSync-Funktion basieren oder DirSync-Funktionalität und Leistung von DS1 testen.

## Nächster Schritt

- Bereitstellen dieser Workload [in der Produktionsumgebung](http://technet.microsoft.com/library/dn635310.aspx).

<!---HONumber=AcomDC_0601_2016-->