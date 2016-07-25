<properties
   pageTitle="Erstellen und Hochladen eines FreeBSD-VM-Images | Microsoft Azure"
   description="Erfahren Sie, wie Sie eine virtuelle Festplatte (Virtual Hard Disk, VHD), die das FreeBSD-Betriebssystem enthält, erstellen und hochladen, um einen virtuelle Azure-Computer zu erstellen."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="KylieLiang"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="06/07/2016"
   ms.author="kyliel"/>

# Erstellen und Hochladen einer FreeBSD-VHD in Azure

In diesem Artikel erfahren Sie, wie Sie eine virtuelle Festplatte (Virtual Hard Disk, VHD) mit FreeBSD-Betriebssystem erstellen und hochladen. Nach dem Hochladen können Sie sie als eigenes Image verwenden, um einen virtuellen Computer (Virtual Machine, VM) in Azure zu erstellen.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


## Voraussetzungen
In diesem Artikel wird davon ausgegangen, dass Sie über die folgenden Elemente verfügen:

- **Azure-Abonnement**: Falls Sie noch nicht über ein Konto verfügen, können Sie in wenigen Minuten eines erstellen. MSDN-Abonnenten finden weitere Informationen unter [Monatliche Azure-Gutschrift für Visual Studio-Abonnenten](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Alle anderen Benutzer können sich unter [Erstellen eines kostenlosen Testkontos](https://azure.microsoft.com/pricing/free-trial/) informieren.

- **Azure PowerShell-Tools**: Das Azure PowerShell-Modul muss installiert und für die Verwendung Ihres Azure-Abonnements konfiguriert sein. Informationen zum Herunterladen dieses Moduls finden Sie unter [Azure-Downloads](https://azure.microsoft.com/downloads/). Ein Tutorial zum Installieren und Konfigurieren des Moduls ist ebenfalls verfügbar. Verwenden Sie das Cmdlet [Azure-Downloads](https://azure.microsoft.com/downloads/), um die VHD hochzuladen.

- **In einer VHD-Datei installiertes FreeBSD-Betriebssystem**: Auf einer virtuellen Festplatte muss ein unterstütztes FreeBSD-Betriebssystem installiert sein. Zum Erstellen von VHD-Dateien stehen mehrere verschiedene Tools bereit. Sie können beispielsweise eine Virtualisierungslösung wie Hyper-V verwenden, um die VHD-Datei zu erstellen und das Betriebssystem zu installieren. Eine Anleitung zum Installieren und Verwenden von Hyper-V finden Sie unter [Installieren von Hyper-V und Erstellen eines virtuellen Computers](http://technet.microsoft.com/library/hh846766.aspx).

> [AZURE.NOTE] Das modernere VHDX-Format wird in Azure noch nicht unterstützt. Sie können den Datenträger mit dem Hyper-V-Manager oder dem Cmdlet [convert-vhd](https://technet.microsoft.com/library/hh848454.aspx) in das VHD-Format konvertieren. Auf MSDN steht zudem ein [Tutorial zum Verwenden von FreeBSD mit Hyper-V](http://blogs.msdn.com/b/kylie/archive/2014/12/25/running-freebsd-on-hyper-v.aspx) zur Verfügung.

Diese Aufgabe umfasst die folgenden fünf Schritte.

## Schritt 1: Vorbereiten des hochzuladenden Images

Führen Sie auf dem virtuellen Computer, auf dem Sie das FreeBSD-Betriebssystem installiert haben, die folgenden Schritte aus:

1. Aktivieren Sie DHCP.

		# echo 'ifconfig_hn0="SYNCDHCP"' >> /etc/rc.conf
		# service netif restart

2. Aktivieren Sie SSH.

    SSH wird nach der Installation von einem Datenträger standardmäßig aktiviert. Falls das Feature aus irgendeinem Grund nicht aktiviert ist oder Sie die virtuelle FreeBSD-Festplatte direkt verwenden, geben Sie Folgendes ein:

		# echo 'sshd_enable="YES"' >> /etc/rc.conf
		# ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key
		# ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key
		# service sshd restart

3. Richten Sie eine serielle Konsole ein.

		# echo 'console="comconsole vidconsole"' >> /boot/loader.conf
		# echo 'comconsole_speed="115200"' >> /boot/loader.conf

4. Installieren Sie sudo.

    Das Stammkonto ist in Azure deaktiviert. sudo muss daher mit einem nicht privilegierten Benutzer verwendet werden, um Befehle mit erhöhten Berechtigungen auszuführen.

		# pkg install sudo
;
5. Voraussetzungen für den Azure-Agent:

		# pkg install python27  
		# pkg install Py27-setuptools27   
		# ln -s /usr/local/bin/python2.7 /usr/bin/python   
		# pkg install git

6. Installieren Sie den Azure-Agent.

    Die neueste Version des Azure-Agents finden Sie immer auf [github](https://github.com/Azure/WALinuxAgent/releases). Die Version 2.0.10+ unterstützt offiziell FreeBSD 10 und 10.1, und die Version 2.1.4 unterstützt offiziell FreeBSD 10.2 und neuere Versionen.

		# git clone https://github.com/Azure/WALinuxAgent.git  
		# cd WALinuxAgent  
		# git tag  
		…
		WALinuxAgent-2.0.16
		…
		v2.1.4
		v2.1.4.rc0
		v2.1.4.rc1

    Für 2.0 verwenden wir 2.0.16 als Beispiel:

		# git checkout WALinuxAgent-2.0.16
		# python setup.py install  
		# ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  

    Für 2.1 verwenden wir 2.1.4 als Beispiel:

		# git checkout v2.1.4
		# python setup.py install  
		# ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  
		# ln -sf /usr/local/sbin/waagent2.0 /usr/sbin/waagent2.0

    >[AZURE.IMPORTANT] Vergewissern Sie sich nach der Installation des Azure-Agents, dass er ausgeführt wird:

		# waagent -version
		WALinuxAgent-2.1.4 running on freebsd 10.3
		Python: 2.7.11
		# service –e | grep waagent
		/etc/rc.d/waagent
		# cat /var/log/waagent.log

7. Heben Sie die Bereitstellung des Systems auf.

    Heben Sie die Bereitstellung des Systems auf, um es zu bereinigen und für eine erneute Bereitstellung vorzubereiten. Mit dem folgenden Befehl werden auch das zuletzt bereitgestellte Benutzerkonto und die dazugehörigen Daten gelöscht:

		# echo "y" |  /usr/local/sbin/waagent -deprovision+user  
		# echo  'waagent_enable="YES"' >> /etc/rc.conf

    Nun können Sie Ihren virtuellen Computer herunterfahren.

## Schritt 2: Erstellen eines Speicherkontos in Azure ##

Sie benötigen ein Speicherkonto in Azure, um eine VHD-Datei hochladen zu können, die zum Erstellen eines virtuellen Computers verwendet werden kann. Über das klassische Azure-Portal können Sie ein Speicherkonto erstellen.

1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com) an.

2. Wählen Sie in der Befehlsleiste **Neu**.

3. Klicken Sie auf **Datendienste** > **Speicher** > **Schnellerfassung**.

	![Speicherkonto schnell erfassen](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Storage-quick-create.png)

4. Füllen Sie die Felder wie folgt aus:

	- Geben Sie unter **URL** einen Unterdomänennamen ein, der in der Speicherkonto-URL verwendet werden soll. Die Eingabe kann zwischen drei und 24 Zahlen und Kleinbuchstaben enthalten. Dieser Name wird zum Hostnamen in der URL, die zum Adressieren von Azure Blob Storage-, Azure Queue Storage- oder Azure Table Storage-Ressourcen für das Abonnement verwendet wird.

	- Wählen Sie im Dropdownmenü **Region/Affinitätsgruppe** die **Region oder Affinitätsgruppe** für das Speicherkonto aus. Mit einer Affinitätsgruppe können Sie Ihre Clouddienste und Speicher im gleichen Rechenzentrum platzieren.

	- Im Feld **Replikation** können Sie wählen, ob Sie die **Georeplikation** für das Speicherkonto verwenden möchten. Georeplikation ist als Standard voreingestellt. Mit dieser Option werden Ihre Daten kostenfrei an einem sekundären Speicherort repliziert. So wird die Speicherung auf jenen Speicherort umgeschaltet, wenn am primären Speicherort ein größerer Ausfall auftritt. Der sekundäre Speicherort wird automatisch zugewiesen und kann nicht geändert werden. Wenn Sie aufgrund gesetzlicher Vorschriften oder Unternehmensrichtlinien mehr Kontrolle über den Speicherort des cloudbasierten Speichers benötigen, können Sie die Georeplikation deaktivieren. Beachten Sie jedoch, dass bei einem späteren Aktivieren der Georeplikation eine einmalige Datenübertragungsgebühr fällig wird, um Ihre vorhandenen Daten in dem sekundären Speicherort zu replizieren. Die Speicherdienste ohne Georeplikation werden mit einem Rabatt angeboten. Ausführliche Informationen zur Verwaltung der Georeplikation für Speicherkonten finden Sie unter [Erstellen, Verwalten oder Löschen eines Speicherkontos](../storage-create-storage-account/#replication-options).

	![Speicherkontodetails eingeben](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Storage-create-account.png)


5. Klicken Sie auf **Speicherkonto erstellen**. Das Konto wird nun unter **Speicher** angezeigt.

	![Speicherkonto erfolgreich erstellt](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Storagenewaccount.png)

6. Erstellen Sie nun einen Container für die hochgeladenen VHD-Dateien. Wählen Sie den Namen des Speicherkontos und anschließend die Option **Container** aus.

	![Speicherkontodetails](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/storageaccount_detail.png)

7. Wählen Sie **Container erstellen** aus.

	![Speicherkontodetails](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/storageaccount_container.png)

8. Geben Sie im Feld **Name** einen Namen für den Container ein. Wählen Sie anschließend im Dropdownmenü **Zugriff** die gewünschte Art von Zugriffsrichtlinie aus.

	![Containername](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/storageaccount_containervalues.png)

    > [AZURE.NOTE] Der Container ist standardmäßig privat, und der Containerzugriff ist auf den Kontobesitzer eingeschränkt. Verwenden Sie die Option **Öffentliches Blob**, um öffentliche Lesezugriffe auf die im Container enthaltenen Blobs, jedoch nicht auf die Containereigenschaften und -metadaten zuzulassen. Verwenden Sie die Option **Öffentlicher Container**, um vollständigen öffentlichen Lesezugriff auf den Container und die BLOBs zuzulassen.

## Schritt 3: Vorbereiten der Verbindung zu Azure

Bevor Sie eine VHD-Datei hochladen können, müssen Sie eine sichere Verbindung zwischen dem Computer und Ihrem Azure-Abonnement herstellen. Zu diesem Zweck können Sie die Azure Active Directory (Azure AD)-Methode oder die Zertifikatmethode verwenden.

### Hochladen einer VHD-Datei mithilfe der Azure AD-Methode

1. Öffnen Sie die Azure PowerShell-Konsole.

2. Geben Sie den folgenden Befehl ein: `Add-AzureAccount`

	Durch diesen Befehl wird ein Anmeldefenster geöffnet, in dem Sie sich mit Ihrem Geschäfts-, Schul- oder Unikonto anmelden können.

	![PowerShell-Fenster](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/add_azureaccount.png)

3. Die Anmeldeinformationen werden von Azure authentifiziert und gespeichert. Danach wird das Fenster geschlossen.

### Hochladen einer VHD-Datei mithilfe der Zertifikatmethode

1. Öffnen Sie die Azure PowerShell-Konsole.

2. Geben Sie Folgendes ein: `Get-AzurePublishSettingsFile`.

3. Es wird ein Browserfenster geöffnet, und Sie werden aufgefordert, eine PUBLISHSETTINGS-Datei herunterzuladen. Diese Datei enthält Informationen und ein Zertifikat für Ihr Azure-Abonnement.

	![Downloadseite des Browsers](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Browser_download_GetPublishSettingsFile.png)

3. Speichern Sie die .publishsettings-Datei.

4. Geben Sie Folgendes ein: `Import-AzurePublishSettingsFile <PathToFile>`. Dabei ist `<PathToFile>` der vollständige Pfad zur PUBLISHSETTINGS-Datei.

   Weitere Informationen finden Sie unter [Erste Schritte mit Azure Cmdlets](http://msdn.microsoft.com/library/windowsazure/jj554332.aspx).

   Weitere Informationen zum Installieren und Konfigurieren von PowerShell finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

## Schritt 4: Hochladen der VHD-Datei

Wenn Sie die VHD-Datei hochladen, können Sie sie an einem beliebigen Speicherort innerhalb des Blobspeichers ablegen. Im Anschluss finden Sie einige Begriffe, die beim Hochladen der Datei verwendet werden:
-  **BlobStorageURL** ist die URL für das in Schritt 2 erstellte Speicherkonto.
-  Bei **YourImagesFolder** handelt es sich um den Container innerhalb des Blobspeichers, in dem Sie Ihre Images speichern möchten.
- **VHDName** steht für die Bezeichnung, die im klassischen Azure-Portal zur Identifizierung der virtuellen Festplatte angezeigt wird.
- **PathToVHDFile** stellt den vollständigen Pfad und den Namen der .vhd-Datei dar.


Geben Sie über das Azure PowerShell-Fenster, welches Sie im vorherigen Schritt verwendet haben, Folgendes ein:

		Add-AzureVhd -Destination "<BlobStorageURL>/<YourImagesFolder>/<VHDName>.vhd" -LocalFilePath <PathToVHDFile>

## Schritt 5: Erstellen eines virtuellen Computers mit der hochgeladenen VHD-Datei
Nach dem Hochladen der VHD-Datei können Sie sie als Image zu der Ihrem Abonnement zugeordneten Liste benutzerdefinierter Images hinzufügen und mit diesem benutzerdefinierten Image einen virtuellen Computer erstellen.

1. Geben Sie über das Azure PowerShell-Fenster, welches Sie im vorherigen Schritt verwendet haben, Folgendes ein:

		Add-AzureVMImage -ImageName <Your Image's Name> -MediaLocation <location of the VHD> -OS <Type of the OS on the VHD>

    > [AZURE.NOTE]Verwenden Sie Linux als Betriebssystemtyp. Die aktuelle Version von Azure PowerShell akzeptiert nur „Linux“ oder „Windows“ als Parameter.

2. Nachdem Sie die vorherigen Schritte abgeschlossen haben, wird das neue Image aufgelistet, sobald Sie die Registerkarte **Images** im klassischen Azure-Portal auswählen.

    ![Wählen Sie ein Image aus](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/addfreebsdimage.png)

3. Erstellen Sie einen virtuellen Computer über den Katalog. Dieses neue Image steht unter **Eigene Images** zur Verfügung.
4. Wählen Sie das neue Image aus. Gehen Sie die Eingabeaufforderungen durch, um Hostname, Kennwort/SSH-Schlüssel usw. einzurichten.

	![Benutzerdefiniertes Image](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/createfreebsdimageinazure.png)

4. Nach Abschluss der Bereitstellung sehen Sie, dass Ihr virtueller FreeBSD-Computer in Azure ausgeführt wird.

	![FreeBSD-Image in Azure](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/freebsdimageinazure.png)

<!---HONumber=AcomDC_0713_2016-->