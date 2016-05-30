<properties
	pageTitle="Behandeln von Problemen mit der SSH-Verbindung mit einem virtuellen Azure-Computer | Microsoft Azure"
	description="Beheben Sie SSH-Fehler wie etwa fehlgeschlagene oder abgelehnte SSH-Verbindungen auf virtuellen Azure-Computern unter Linux."
	keywords="SSH-Verbindung abgelehnt,SSH-Fehler,Azure SSH,SSH-Verbindungsfehler"
	services="virtual-machines-linux"
	documentationCenter=""
	authors="iainfoulds"
	manager="timlt"
	editor=""
	tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
	ms.service="virtual-machines-linux"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-linux"
	ms.devlang="na"
	ms.topic="support-article"
	ms.date="04/12/2016"
	ms.author="iainfou"/>

# Problembehandlung für SSH-Verbindungen (Secure Shell) mit einem Linux-basierten virtuellen Azure-Computer

Es gibt verschiedene Gründe dafür, dass SSH-Fehler (Secure Shell) auftreten, wenn Sie versuchen, eine Verbindung mit einem Linux-basierten virtuellen Azure-Computer herzustellen. Dieser Artikel hilft Ihnen, diese Probleme zu ermitteln und zu beheben.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Wenn Sie beim Lesen dieses Artikels feststellen, dass Sie weitere Hilfe benötigen, können Sie Azure-Experten im [MSDN Azure-Forum oder im Stack Overflow-Forum](http://azure.microsoft.com/support/forums/) Fragen stellen. Alternativ dazu haben Sie die Möglichkeit, einen Azure-Supportfall zu erstellen. Rufen Sie die [Azure-Support-Website](http://azure.microsoft.com/support/options/) auf, und wählen Sie **Support erhalten** aus. Informationen zur Nutzung von Azure-Support finden Sie unter [Microsoft Azure-Support-FAQ](http://azure.microsoft.com/support/faq/).


## Allgemeine SSH-Fehler beheben.

Dieser Abschnitt enthält Anweisungen für die schnelle Behebung häufig auftretender Probleme mit der SSH-Verbindung.

### Problembehandlung bei virtuellen Computern, die mit dem klassischen Bereitstellungsmodell erstellt wurden

Führen Sie die folgenden Schritte aus, um die häufigsten SSH-Verbindungsfehler für virtuelle Computer zu beheben, die mit dem klassischen Bereitstellungsmodell erstellt wurden: Versuchen Sie nach jedem Schritt, die Verbindung mit dem virtuellen Computer erneut herzustellen.

- Remotezugriff zurücksetzen über das [Azure-Portal](https://portal.azure.com). Wählen Sie im Azure-Portal die Option **Durchsuchen** > **Virtuelle Computer (klassisch)** > *Ihr virtueller Linux-Computer* > **Remotezugriff zurücksetzen** aus.

- Starten Sie den virtuellen Computer neu. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf **Durchsuchen** > **Virtuelle Computer (klassisch)** > *Ihr virtueller Linux-Computer* > **Neustart**.

	ODER

	Öffnen Sie im [klassischen Azure-Portal](https://manage.windowsazure.com) nacheinander **Virtuelle Computer** > **Instanzen** > **Neustart**.

- Stellen Sie den virtuellen Computer in einem neuen Azure-Knoten erneut bereit. Informationen hierzu finden Sie unter [Einen virtuellen Computer in einem neuen Azure-Knoten erneut bereitstellen](virtual-machines-windows-redeploy-to-new-node.md).

	Beachten Sie, dass nach Beenden dieses Vorgangs kurzlebige Datenträgerdaten verloren gehen und dynamische, dem virtuellen Computer zugeordnete IP-Adressen aktualisiert werden.

- Führen Sie die unter [Zurücksetzen eines Kennworts oder einer SSH für Linux-basierte virtuelle Computer](virtual-machines-linux-classic-reset-access.md) beschriebenen Anweisungen für folgende Schritte aus:
	- Zurücksetzen des Kennworts oder des SSH-Schlüssels
	- Erstellen eines neuen Benutzerkontos in _sudo_
	- Zurücksetzen der SSH-Konfiguration

- Überprüfen Sie die Ressourcenintegrität des virtuellen Computers auf etwaige Plattformprobleme.<br> Klicken Sie auf **Durchsuchen** > **Virtuelle Computer (klassisch)** > *Ihr virtueller Linux-Computer* > **Einstellungen** > **Integrität prüfen**.


### Problembehandlung bei virtuellen Computern, die mit dem Resource Manager-Bereitstellungsmodell erstellt wurden

Führen Sie die folgenden Schritte aus, um die häufigsten SSH-Verbindungsfehler für virtuelle Computer zu beheben, die mit dem Azure Resource Manager-Bereitstellungsmodell erstellt wurden:

#### Zurücksetzen der SSH-Verbindung
Stellen Sie bei Verwendung der Azure-Befehlszeilenschnittstelle sicher, dass [Microsoft Azure Linux Agent](virtual-machines-linux-agent-user-guide.md) Version 2.0.5 oder höher installiert ist.

[Installieren Sie die Azure-Befehlszeilenschnittstelle, und stellen Sie eine Verbindung mit Ihrem Azure-Abonnement her](../xplat-cli-install.md) (falls noch nicht geschehen). Melden Sie sich mit dem `azure login`-Befehl an. Stellen Sie sicher, dass Sie sich im Ressourcen-Manager-Modus befinden: ```
	azure config mode arm
	```

Setzen Sie die SSH-Verbindung mit einer der folgenden Methoden zurück:

**Methode 1**: Verwenden Sie den Befehl `vm reset-access` wie im folgenden Beispiel. Die `VMAccessForLinux`-Erweiterung wird auf Ihrem virtuellen Computer installiert:

	```
	azure vm reset-access -g YourResourceGroupName -n YourVirtualMachineName -r
	```



**Methode 2:** Erstellen Sie eine Datei mit dem Namen „PrivateConf.json“ mit folgendem Inhalt:

	```
	{  
		"reset_ssh":"True"
	}
	```

Führen Sie anschließend manuell die `VMAccessForLinux`-Erweiterung aus, um die SSH-Verbindung zurückzusetzen:

	```
	azure vm extension set "YourResourceGroupName" "YourVirtualMachineName" VMAccessForLinux Microsoft.OSTCExtensions "1.2" --private-config-path PrivateConf.json
	```

#### Zurücksetzen der SSH-Anmeldeinformationen

**Methode 1:** Führen Sie den Befehl `vm reset-access` aus, um die SSH-Anmeldeinformationen festzulegen.

	```
	azure vm reset-access TestRgV2 TestVmV2 -u NewUser -p NewPassword
	```

Weitere Informationen hierzu erhalten Sie, indem Sie in der Befehlszeile `azure vm reset-access -h` eingeben.

**Methode 2:** Erstellen Sie eine Datei mit dem Namen „PrivateConf.json“ mit folgendem Inhalt:

	```
	{
		"username":"NewUsername", "password":"NewPassword", "expiration":"2016-01-01", "ssh_key":"", "reset_ssh":false, "remove_user":""
	}
	```

Führen Sie dann die Linux-Erweiterung mit dem eben beschriebenen Dateinamen aus ("PrivateConf.json"):

	```
	$azure vm extension set "testRG" "testVM" VMAccessForLinux Microsoft.OSTCExtensions "1.2" --private-config-path PrivateConf.json
	```

Wenn Sie andere Ansätze für die Problembehandlung ausprobieren möchten, können Sie Schritte wie unter [Zurücksetzen eines Kennworts oder einer SSH für Linux-basierte virtuelle Computer](virtual-machines-linux-classic-reset-access.md) ausführen. Denken Sie hierbei daran, die Anweisungen der Azure-Befehlszeilenschnittstelle für den Ressourcen-Manager-Modus zu ändern.


## SSH-Fehlerbehebung im Detail

Es gibt viele mögliche Ursachen dafür, dass der SSH-Client weiterhin nicht in der Lage ist, den SSH-Dienst auf dem virtuellen Computer zu erreichen. Das folgende Diagramm zeigt die beteiligten Komponenten.

![Diagramm mit den Komponenten des SSH-Diensts](./media/virtual-machines-linux-troubleshoot-ssh-connection/ssh-tshoot1.png)

Mit den folgenden Schritten können Sie die Fehlerquelle isolieren und Lösungen oder Problemumgehungen ermitteln.

### Durchführen vorbereitender Schritte

Überprüfen Sie zunächst den Status des virtuellen Computers im Portal.

Gehen Sie im [klassischen Azure-Portal](https://manage.windowsazure.com) folgendermaßen für virtuelle Computer vor, die mit dem klassischen Bereitstellungsmodell erstellt wurden:

1. Klicken Sie auf **Virtuelle Computer** > *VM-Name*.
2. Wählen Sie das **Dashboard** des virtuellen Computers aus, um den Status zu überprüfen.
3. Wählen Sie **Überwachen** aus, um aktuelle Aktivitäten für Compute-, Speicher- und Netzwerkressourcen anzuzeigen.
4. Wählen Sie **Endpunkte** aus, und vergewissern Sie sich, dass ein Endpunkt für SSH-Datenverkehr vorhanden ist.

Im [Azure-Portal](https://portal.azure.com):

1. Klicken Sie für virtuelle Computer, die mit dem klassischen Bereitstellungsmodell erstellt wurden, auf **Durchsuchen** > **Virtuelle Computer (klassisch)** > *VM-Name*.

	ODER

	Klicken Sie für virtuelle Computer, die mit dem Ressourcen-Manager-Bereitstellungsmodell erstellt wurden, auf **Durchsuchen** > **Virtuelle Computer** > *VM-Name*.

	Im Statusbereich für den virtuellen Computer wird **Wird ausgeführt** angezeigt. Führen Sie einen Bildlauf nach unten durch, um aktuelle Aktivitäten für Compute-, Speicher- und Netzwerkressourcen anzuzeigen.

2. Wählen Sie **Einstellungen** aus, um Endpunkte, IP-Adressen und andere Einstellungen zu überprüfen.

	Um Endpunkte auf virtuellen Computern zu identifizieren, die mit dem Ressourcen-Manager erstellt wurden, vergewissern Sie sich, ob eine [Netzwerksicherheitsgruppe](../virtual-network/virtual-networks-nsg.md) definiert wurde. Stellen Sie außerdem sicher, dass die Regeln auf die Netzwerksicherheitsgruppe angewendet wurden und dass im Subnetz darauf verwiesen wird.

Analysieren Sie zum Überprüfen der Netzwerkverbindung die konfigurierten Endpunkte, und testen Sie, ob der virtuelle Computer über ein anderes Protokoll (z. B. HTTP oder über einen anderen Dienst) erreichbar ist.

Versuchen Sie nach diesen Schritten erneut, die SSH-Verbindung herzustellen.


### Ermitteln der Ursache des Problems

Das Verbindungsproblem zwischen dem SSH-Client auf Ihrem Computer und dem SSH-Dienst auf dem virtuellen Azure-Computer ist u.U. auf die folgenden Problem- oder Konfigurationsfehlerquellen zurückzuführen:

- SSH-Clientcomputer
- Edgegerät der Organisation
- Clouddienst-Endpunkt und die Zugriffssteuerungsliste (ACL)
- Netzwerksicherheitsgruppen
- Linux-basierter virtueller Azure-Computer

#### Quelle 1: SSH-Clientcomputer

Überprüfen Sie, ob Ihr Computer SSH-Verbindungen mit einem anderen Linux-basierten lokalen Computer herstellen kann, um Ihren Computer als Ursache des Fehlers auszuschließen.

![Diagramm, in dem die SSH-Clientcomputerkomponenten hervorgehoben sind](./media/virtual-machines-linux-troubleshoot-ssh-connection/ssh-tshoot2.png)

Falls dies nicht möglich ist, suchen Sie auf Ihrem Computer Folgendes:

- Eine lokale Firewall-Einstellung, die eingehenden oder ausgehenden SSH-Datenverkehr (TCP-22) blockiert
- Lokal installierte Proxy-Clientsoftware, die SSH-Verbindungen verhindert
- Lokal installierte Netzwerküberwachungssoftware, die SSH-Verbindungen verhindert
- Andere Arten von Sicherheitssoftware, die Datenverkehr überwachen oder bestimmte Arten von Datenverkehr zulassen/unterbinden

Wenn eine dieser Bedingungen zutrifft, deaktivieren Sie die Software vorübergehend, und versuchen Sie, eine SSH-Verbindung mit einem lokalen Computer herzustellen, um zu ermitteln, warum die Verbindung auf Ihrem Computer blockiert wird. Korrigieren Sie dann in Zusammenarbeit mit Ihrem Netzwerkadministrator die Einstellungen der Software, um SSH-Verbindungen zu ermöglichen.

Vergewissern Sie sich bei Verwendung der Zertifikatauthentifizierung, dass Sie für den SSH-Ordner in Ihrem Basisverzeichnis über folgende Berechtigungen verfügen:

- Chmod 700 ~/.ssh
- Chmod 644 ~/.ssh/*.pub
- Chmod 600 ~/.ssh/id\_rsa (bzw. beliebige andere Dateien, in denen Ihre privaten Schlüssel gespeichert sind)
- Chmod 644 ~/.ssh/known\_hosts (enthält Hosts, mit denen Sie eine SSH-Verbindung hergestellt haben)

#### Quelle 2: Edgegerät der Organisation

Überprüfen Sie, ob ein direkt mit dem Internet verbundener Computer SSH-Verbindungen mit Ihrem virtuellen Azure-Computer herstellen kann, um das Edgegerät der Organisation als Fehlerquelle auszuschließen. Wenn der Zugriff auf den virtuellen Computer über ein Site-to-Site-VPN oder über eine Azure ExpressRoute-Verbindung erfolgt, fahren Sie mit [Quelle 4: Netzwerksicherheitsgruppen](#nsg) fort.

![Diagramm, in dem das Edgegerät der Organisation hervorgehoben ist](./media/virtual-machines-linux-troubleshoot-ssh-connection/ssh-tshoot3.png)

Wenn Sie keinen Computer haben, der direkt mit dem Internet verbunden ist, können Sie problemlos einen neuen virtuellen Azure-Computer in einer eigenen Ressourcengruppe oder einem eigenen Clouddienst erstellen und verwenden. Weitere Informationen finden Sie unter [Erstellen eines virtuellen Computers unter Linux in Azure](virtual-machines-linux-quick-create-cli.md). Löschen Sie die Ressourcengruppe oder den virtuellen Computer und den Clouddienst, wenn Sie Ihre Tests abgeschlossen haben.

Wenn Sie eine SSH-Verbindung mit einem direkt mit dem Internet verbundenen Computer herstellen können, überprüfen Sie das Edgegerät Ihrer Organisation auf Folgendes:

- Eine interne Firewall, die SSH-Verbindungen mit dem Internet blockiert
- Ein Proxy-Server, der SSH-Verbindungen verhindert
- Auf dem Edgegerät ausgeführte Software zur Netzwerküberwachung oder zur Erkennung von Eindringversuchen, die SSH-Verbindungen verhindert

Korrigieren Sie in Zusammenarbeit mit Ihrem Netzwerkadministrator die Einstellungen des Edgegeräts Ihrer Organisation, um SSH-Datenverkehr über das Internet zuzulassen.

#### Quelle 3: Clouddienst-Endpunkt und ACL

> [AZURE.NOTE] Diese Quelle gilt nur für virtuelle Computer, die mit dem klassischen Bereitstellungsmodell erstellt wurden. Für virtuelle Computer, die mit dem Ressourcen-Manager erstellt wurden, fahren Sie mit [Quelle 4: Netzwerksicherheitsgruppen](#nsg) fort.

Überprüfen Sie, ob ein anderer virtueller Azure-Computer im gleichen virtuellen Netzwerk eine SSH-Verbindung mit Ihrem virtuellen Computer herstellen kann, um den Clouddienst-Endpunkt und die ACL als Fehlerquelle auszuschließen.

![Diagramm, in dem der Clouddienstendpunkt und ACL hervorgehoben sind](./media/virtual-machines-linux-troubleshoot-ssh-connection/ssh-tshoot4.png)

Ist kein anderer virtueller Computer im gleichen virtuellen Netzwerk vorhanden, können Sie problemlos einen neuen virtuellen Computer erstellen. Weitere Informationen finden Sie unter [Erstellen eines virtuellen Linux-Computers in Azure mithilfe der Befehlszeilenschnittstelle](virtual-machines-linux-quick-create-cli.md). Löschen Sie den zusätzlichen virtuellen Computer, wenn Sie Ihre Tests abgeschlossen haben.

Wenn Sie eine SSH-Verbindung mit einem virtuellen Computer im gleichen virtuellen Netzwerk herstellen können, überprüfen Sie Folgendes:

- **Die Endpunktkonfiguration für den SSH-Datenverkehr auf dem virtuellen Zielcomputer.** Der private TCP-Port des Endpunkts sollte dem TCP-Port entsprechen, an dem der SSH-Dienst auf dem virtuellen Computer lauscht. (Der Standardport ist 22.) Überprüfen Sie bei virtuellen Computern, die mit dem Ressourcen-Manager-Bereitstellungsmodell erstellt wurden, die SSH-TCP-Portnummer im Azure-Portal, indem Sie **Durchsuchen** > **Virtuelle Computer (v2)** > *VM-Name* > **Einstellungen** > **Endpunkte** auswählen.

- **Die ACL für den SSH-Datenverkehrsendpunkt auf dem virtuellen Zielcomputer.** In ACLs können Sie anhand der Quell-IP-Adresse angeben, ob eingehender Datenverkehr aus dem Internet zugelassen oder verweigert wird. Falsch konfigurierte ACLs können dazu führen, dass eingehender SSH-Datenverkehr den Endpunkt nicht erreicht. Überprüfen Sie die ACLs, um sicherzustellen, dass eingehender Verkehr von den öffentlichen IP-Adressen des Proxy- oder eines anderen Edgeservers zugelassen wird. Weitere Informationen finden Sie unter [Was ist eine Netzwerk-Zugriffssteuerungsliste (Access Control List, ACL)?](../virtual-network/virtual-networks-acl.md)

Entfernen Sie den aktuellen Endpunkt, und erstellen Sie einen neuen Endpunkt mit dem SSH-Namen und dem TCP-Port 22 als öffentliche und private Portnummer, um den Endpunkt als Problemquelle auszuschließen. Weitere Informationen finden Sie unter [Festlegen von Endpunkten auf einem virtuellen Computer in Azure](virtual-machines-windows-classic-setup-endpoints.md).

<a id="nsg"></a>
#### Quelle 4: Netzwerksicherheitsgruppen

Netzwerksicherheitsgruppen ermöglichen Ihnen eine präzisere Steuerung des zulässigen eingehenden und ausgehenden Datenverkehrs. Sie können Regeln erstellen, die mehrere Subnetze und Clouddienste in einem virtuellen Azure-Netzwerk umfassen. Überprüfen Sie die Regeln der Netzwerksicherheitsgruppe, um sicherzustellen, dass sowohl eingehender als auch ausgehender SSH-Datenverkehr über das Internet zulässig ist. Weitere Informationen finden Sie unter [Informationen zu Netzwerksicherheitsgruppen](../virtual-network/virtual-networks-nsg.md).

#### Quelle 5: Linux-basierter virtueller Azure-Computer

Die letzte mögliche Problemquelle ist der virtuelle Azure-Computer selbst.

![Diagramm, in dem der Linux-basierte virtuelle Azure-Computer hervorgehoben ist](./media/virtual-machines-linux-troubleshoot-ssh-connection/ssh-tshoot5.png)

Führen Sie die unter [Zurücksetzen eines Kennworts oder einer SSH für Linux-basierte virtuelle Computer](virtual-machines-linux-classic-reset-access.md) beschriebenen Anweisungen aus (falls noch nicht erfolgt).

Versuchen Sie erneut, die Verbindung von Ihrem Computer aus herzustellen. Wenn dies weiterhin nicht möglich ist, können u.a. die folgenden Probleme der Grund dafür sein:

- Der SSH-Dienst wird auf dem virtuellen Zielcomputer nicht ausgeführt.
- Der SSH-Dienst lauscht nicht am TCP-Port 22. Installieren Sie zu Testzwecken einen Telnet-Client auf Ihrem lokalen Computer, und führen Sie „telnet *Name des Clouddiensts*.cloudapp.net 22“ aus. So können Sie ermitteln, ob der virtuelle Computer die eingehende und ausgehende Kommunikation mit dem SSH-Endpunkt zulässt.
- Für die lokale Firewall auf dem virtuellen Zielcomputer sind Regeln definiert, die eingehenden oder ausgehenden SSH-Datenverkehr verhindern.
- Auf dem virtuellen Azure-Computer wird Software zur Netzwerküberwachung oder zur Erkennung von Eindringversuchen ausgeführt, die SSH-Verbindungen verhindert.


## Zusätzliche Ressourcen

- Weitere Informationen zur Problembehandlung bei virtuellen Computern, die mit dem klassischen Bereitstellungsmodell erstellt wurden, finden Sie unter [Zurücksetzen eines Kennworts oder einer SSH für Linux-basierte virtuelle Computer](virtual-machines-linux-classic-reset-access.md).

- Weitere Informationen zur Problembehandlung bei virtuellen Computern, die mit dem Ressourcen-Manager-Bereitstellungsmodell erstellt wurden, finden Sie unter:
	- [Problembehandlung bei Windows-Remotedesktopverbindungen mit einem Windows-basierten virtuellen Azure-Computer](virtual-machines-windows-troubleshoot-rdp-connection.md)
	-	[Problembehandlung beim Zugriff auf eine Anwendung, die auf einem virtuellen Azure-Computer ausgeführt wird](virtual-machines-linux-troubleshoot-app-connection.md)

<!---HONumber=AcomDC_0518_2016-->