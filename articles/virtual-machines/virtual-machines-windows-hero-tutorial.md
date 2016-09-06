<properties
	pageTitle="Erstellen Ihres ersten virtuellen Windows-Computers | Microsoft Azure"
	description="Hier erfahren Sie, wie Sie mithilfe des Azure-Portals Ihren ersten virtuellen Windows-Computer erstellen."
	keywords="virtuelle Windows-Maschine, Erstellen einer virtuellen Maschine, virtueller Computer, Einrichten einer virtuellen Maschine"
	services="virtual-machines-windows"
	documentationCenter=""
	authors="cynthn"
	manager="timlt"
	editor=""
	tags="azure-resource-manager"/>
<tags
	ms.service="virtual-machines-windows"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="hero-article"
	ms.date="08/29/2016"
	ms.author="cynthn"/>

# Erstellen Ihres ersten virtuellen Windows-Computers im Azure-Portal

In diesem Tutorial erfahren Sie, wie einfach es ist, in wenigen Minuten einen virtuellen Windows-Computer mithilfe des Azure-Portals zu erstellen.

Falls Sie nicht über ein Azure-Abonnement verfügen, können Sie in wenigen Minuten ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen.

Hier finden Sie ein [Video mit einer exemplarischen Vorgehensweise](https://channel9.msdn.com/Blogs/Azure-Documentation-Shorts/Create-A-Virtual-Machine-Running-Windows-In-The-Azure-Preview-Portal) für dieses Tutorial.


## Auswählen des VM-Image aus dem Marketplace

Als Beispiel wird ein Windows Server 2012 R2 Datacenter-Image verwendet. Dies ist jedoch nur eines von vielen Images, die Azure bietet. Ihre Imageauswahl hängt von Ihrem Abonnement ab. Für [MSDN-Abonnenten](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) können beispielsweise Desktop-Images verfügbar sein.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Klicken Sie im Menü „Hub“ auf **Neu** > **Virtuelle Computer** > **Windows Server 2012 R2 Datacenter**.

	![Screenshot, der die Azure-VM-Images zeigt, die im Portal verfügbar sind](./media/virtual-machines-windows-hero-tutorial/marketplace-new.png)


3. Vergewissern Sie sich, dass auf dem Blatt **Windows Server 2012 R2 Datacenter** unter **Bereitstellungsmodell auswählen** die Option **Resource Manager** ausgewählt ist. Klicken Sie auf **Erstellen**.

	![Screenshot mit dem auszuwählenden Bereitstellungsmodell für den virtuellen Computer](./media/virtual-machines-windows-hero-tutorial/deployment-model.png)

## Erstellen des virtuellen Windows-Computers

Nachdem Sie das Image ausgewählt haben, können Sie für den Großteil der Konfiguration die Azure-Standardeinstellungen verwenden und den virtuellen Computer rasch erstellen.

1. Geben Sie auf dem Blatt **Grundlagen** einen Namen für den virtuellen Computer ein. Der Name kann bis zu 15 Zeichen lang sein und darf keine Sonderzeichen enthalten.

2. Geben Sie einen Benutzernamen und ein sicheres Kennwort für die Erstellung eines lokalen Kontos auf dem virtuellen Computer ein. Das lokale Konto wird für die Anmeldung bei dem virtuellen Computer sowie für dessen Verwaltung verwendet.

	Das Kennwort muss 8 bis 123 Zeichen lang sein und drei der folgenden vier Komplexitätsanforderungen erfüllen: ein Kleinbuchstabe, ein Großbuchstabe, eine Zahl und ein Sonderzeichen. Weitere Informationen zu den [Anforderungen für Benutzernamen und Kennwörter](virtual-machines-windows-faq.md#what-are-the-username-requirements-when-creating-a-vm).


3. Wählen Sie eine vorhandene Ressourcengruppe[](../resource-group-overview.md#resource-groups) aus, oder geben Sie den Namen für eine neue Ressourcengruppe ein. Geben Sie den Standort eines Azure-Datencenters ein (beispielsweise **USA, Westen**).

4. Klicken Sie anschließend auf **OK**, um zum nächsten Abschnitt zu gelangen.

	![Screenshot des Blatts **Grundlagen** mit den Einstellungen für die Konfiguration einer virtuellen Azure-VM](./media/virtual-machines-windows-hero-tutorial/basics-blade.png)

	
5. Wählen Sie eine VM-Größe[](virtual-machines-windows-sizes.md) aus, und klicken Sie anschließend auf **Auswählen**, um den Vorgang fortzusetzen.

	![Screenshot des Blatts „Größe“ mit den Größenoptionen für den virtuellen Azure-Computer](./media/virtual-machines-windows-hero-tutorial/size-blade.png)

6. Auf dem Blatt **Einstellungen** können Sie die Speicher- und Netzwerkoptionen ändern. Übernehmen Sie für dieses Tutorial die Standardwerte. Wenn Sie eine geeignete Größe für den virtuellen Computer ausgewählt haben, können Sie durch Auswahl von **Premium (SSD)** unter **Datenträgertyp** Storage Premium testen. Klicken Sie auf **OK**, nachdem Sie alle gewünschten Änderungen vorgenommen haben.

	![Screenshot des Blatts „Einstellungen“ zum Konfigurieren optionaler Features für einen virtuellen Azure-Computer](./media/virtual-machines-windows-hero-tutorial/settings-blade.png)

7. Klicken Sie auf **Zusammenfassung**, um Ihre Auswahl zu überprüfen. Wenn die Meldung **Überprüfung erfolgreich** angezeigt wird, klicken Sie auf **OK**.

	![Screenshot der Seite „Zusammenfassung“ mit der getroffenen Konfigurationsauswahl für den virtuellen Azure-Computer](./media/virtual-machines-windows-hero-tutorial/summary-blade.png)

8. Im Hubmenü können Sie unter **Virtuelle Computer** den Erstellungsstatus des virtuellen Computers nachverfolgen.


## Stellen Sie eine Verbindung mit dem virtuellen Computer her, und melden Sie sich an.

1.	Klicken Sie im Hubmenü auf **Virtuelle Computer**.

2.	Wählen Sie den gewünschten virtuellen Computer aus der Liste aus.

3. Klicken Sie auf dem Blatt für den virtuellen Computer auf **Verbinden**. Dadurch wird eine Remotedesktopprotokoll-Datei (RDP-Datei) erstellt und heruntergeladen, mit der Sie wie bei einer Verknüpfung eine Verbindung mit Ihrem Computer herstellen können. Aus Komfortgründen empfiehlt es sich, die Datei auf dem Desktop abzulegen. Öffnen Sie die Datei, um eine Verbindung mit Ihrer VM herzustellen.

	![Screenshot des Azure-Portals beim Herstellen einer Verbindung mit Ihrem virtuellen Computer](./media/virtual-machines-windows-hero-tutorial/connect.png)

4. Es erscheint eine Warnung mit dem Hinweis, dass die RDP-Datei von einem unbekannten Herausgeber stammt. Dies ist normal. Klicken Sie im Fenster "Remotedesktop" auf **Verbinden**, um den Vorgang fortzusetzen.

	![Screenshot einer Warnung wegen eines unbekannten Herausgebers](./media/virtual-machines-windows-hero-tutorial/rdp-warn.png)

5. Geben Sie im Fenster „Windows-Sicherheit“ den Benutzernamen und das Kennwort für das lokale Konto ein, das Sie beim Erstellen des virtuellen Computers erstellt haben. Der Benutzername muss im Format *VMName*&#92;*Benutzername* eingegeben werden. Klicken Sie anschließend auf **OK**.

	![Screenshot der Eingabe von VM-Name, Benutzername und Kennwort](./media/virtual-machines-windows-hero-tutorial/credentials.png)
 	
6.	Es erscheint eine Warnung mit dem Hinweis, dass das Zertifikat nicht überprüft werden kann. Dies ist normal. Klicken Sie auf **Ja**, um die Identität des virtuellen Computers zu bestätigen und das Anmelden zu beenden.

	![Screenshot mit einer Meldung zur Überprüfung der Identität des virtuellen Computers](./media/virtual-machines-windows-hero-tutorial/cert-warning.png)


Informationen zum Behandeln von Verbindungsproblemen finden Sie unter [Problembehandlung bei Remotedesktopverbindungen mit einem Windows-basierten virtuellen Azure-Computer](virtual-machines-windows-troubleshoot-rdp-connection.md).

Sie können jetzt mit dem virtuellen Computer wie mit jedem anderen Server arbeiten.

## Installieren von IIS auf der VM

Nachdem Sie sich bei der VM angemeldet haben, installieren Sie eine Serverrolle, damit Sie noch etwas mehr experimentieren können.

1. Öffnen Sie den **Server-Manager**, sofern er noch nicht geöffnet ist. Klicken Sie dazu im Menü **Start** auf **Server-Manager**.
2. Wählen Sie im **Server-Manager** im linken Bereich **Lokaler Server** aus.
3. Klicken Sie im Menü auf **Verwalten** > **Rollen und Features hinzufügen**.
4. Wählen Sie im Assistenten zum Hinzufügen von Rollen und Features auf der Seite **Installationstyp** die Option **Rollenbasierte oder featurebasierte Installation** aus, und klicken Sie dann auf **Weiter**.

	![Screenshot der Registerkarte „Installationstyp“ im Assistenten zum Hinzufügen von Rollen und Features](./media/virtual-machines-windows-hero-tutorial/role-wizard.png)

5. Wählen Sie die VM aus dem Serverpool aus, und klicken Sie auf **Weiter**.
6. Wählen Sie auf der Seite **Serverrollen** die Option **Webserver (IIS)** aus.

	![Screenshot der Registerkarte „Serverrollen“ im Assistenten zum Hinzufügen von Rollen und Features](./media/virtual-machines-windows-hero-tutorial/add-iis.png)

7. Stellen Sie im Popupfenster zum Hinzufügen der erforderlichen Features für IIS sicher, dass **Verwaltungstools einschließen** ausgewählt ist, und klicken Sie dann auf **Features hinzufügen**. Nachdem das Popupfenster geschlossen wurde, klicken Sie im Assistenten auf **Weiter**.

	![Screenshot des Popupfensters zum Bestätigen des Hinzufügens der IIS-Rolle](./media/virtual-machines-windows-hero-tutorial/confirm-add-feature.png)

8. Klicken Sie auf der Seite „Features“ auf **Weiter**.
9. Klicken Sie auf der Seite **Rolle „Webserver“ (IIS)** auf **Weiter**.
10. Klicken Sie auf der Seite **Rollendienste** auf **Weiter**.
11. Klicken Sie auf der Bestätigungsseite auf **Installieren**.
12. Klicken Sie nach Abschluss der Installation im Assistenten auf **Schließen**.



## Öffnen von Port 80 

Damit Ihre VM eingehenden Datenverkehr über Port 80 akzeptiert, müssen Sie der Netzwerksicherheitsgruppe eine Eingangsregel hinzufügen.

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
2. Wählen Sie unter **Virtuelle Computer** die VM aus, die Sie erstellt haben.
3. Wählen Sie in den Einstellungen für virtuelle Computer **Netzwerkschnittstellen** und dann die vorhandene Netzwerkschnittstelle aus.

	![Screenshot der Einstellungen des virtuellen Computers für die Netzwerkschnittstellen](./media/virtual-machines-windows-hero-tutorial/network-interface.png)

4. Klicken Sie in der ** Zusammenfassung** für die Netzwerkschnittstelle auf die **Netzwerksicherheitsgruppe**.

	![Screenshot des Abschnitts „Zusammenfassung“ für die Netzwerkschnittstelle](./media/virtual-machines-windows-hero-tutorial/select-nsg.png)

5. Auf dem Blatt **Zusammenfassung** für die Netzwerksicherheitsgruppe sollte eine Standardeingangsregel für **default-allow-rdp** vorhanden sein, die Ihnen die Anmeldung bei der VM ermöglicht. Sie fügen eine weitere Eingangsregel zum Zulassen von IIS-Datenverkehr hinzu. Klicken Sie auf **Eingangssicherheitsregel**.

	![Screenshot des Abschnitts „Zusammenfassung“ für die Netzwerksicherheitsgruppe](./media/virtual-machines-windows-hero-tutorial/inbound.png)

6. Klicken Sie im Abschnitt **Eingangssicherheitsregeln** auf **Hinzufügen**.

	![Screenshot der Schaltfläche zum Hinzufügen einer Sicherheitsregel](./media/virtual-machines-windows-hero-tutorial/add-rule.png)

7. Klicken Sie im Abschnitt **Eingangssicherheitsregeln** auf **Hinzufügen**. Geben Sie **80** für den Portbereich ein, und stellen Sie sicher, dass **Zulassen** ausgewählt ist. Klicken Sie anschließend auf **OK**.

	![Screenshot der Schaltfläche zum Hinzufügen einer Sicherheitsregel](./media/virtual-machines-windows-hero-tutorial/port-80.png)
 
Weitere Informationen zu Netzwerksicherheitsgruppen sowie Ein- und Ausgangsregeln finden Sie unter [Ermöglichen des externen Zugriffs auf einen virtuellen Computer über das Azure-Portal](virtual-machines-windows-nsg-quickstart-portal.md).
 
## Herstellen einer Verbindung mit der IIS-Standardwebsite

1. Klicken Sie im Azure-Portal auf **Virtuelle Computer**, und wählen Sie anschließend Ihre VM aus.
2. Kopieren Sie auf dem Blatt **Zusammenfassung** Ihre **Öffentliche IP-Adresse**.

	![Screenshot, der zeigt, wo Sie die öffentliche IP-Adresse Ihrer VM finden](./media/virtual-machines-windows-hero-tutorial/ipaddress.png)

2. Öffnen Sie einen Browser, und geben Sie Ihre öffentliche IP-Adresse wie folgt in die Adressleiste ein: http://<öffentlicheIPAdresse&gt. Drücken Sie anschließend die EINGABETASTE, um zu dieser Adresse zu wechseln.
3. Die IIS-Standardwebseite sollte in etwa wie folgt in Ihrem Browser angezeigt werden:

	![Screenshot der IIS-Standardseite in einem Browser](./media/virtual-machines-windows-hero-tutorial/iis-default.png)


## Beenden des virtuellen Computers

Es empfiehlt sich, den virtuellen Computer wieder zu beenden, damit keine Kosten anfallen, obwohl Sie ihn gar nicht verwenden. Klicken Sie hierzu auf die Schaltfläche **Beenden** und anschließend auf **Ja**.

![Screenshot der Schaltfläche zum Beenden eines virtuellen Computers](./media/virtual-machines-windows-hero-tutorial/stop-vm.png)
	
Wenn Sie die VM wieder verwenden möchten, klicken Sie einfach auf die Schaltfläche **Starten**, um sie neu zu starten.


## Nächste Schritte

* Sie können auch mit dem [Anfügen eines Datenträgers](virtual-machines-windows-attach-disk-portal.md) an Ihren virtuellen Computer experimentieren. Datenträger bieten mehr Speicherplatz für den virtuellen Computer.

* Darüber hinaus haben Sie die Möglichkeit zum [Erstellen einer Windows-VM mit dem Resource Manager und PowerShell](virtual-machines-windows-ps-create.md) sowie zum [Erstellen eines virtuellen Linux-Computers in Azure mithilfe der Befehlszeilenschnittstelle](virtual-machines-linux-quick-create-cli.md).

* Falls Sie Bereitstellungen automatisieren möchten, finden Sie unter [Erstellen Sie einen virtuellen Windows-Computer mit einer Resource Manager-Vorlage](virtual-machines-windows-ps-template.md) weitere Informationen.

<!---HONumber=AcomDC_0831_2016-->