Verschiedene Gründe können zu Problemen beim Starten einer Anwendung oder beim Herstellen einer Verbindung mit einer Anwendung führen, die auf einem virtuellen Azure-Computer ausgeführt wird, z.B. wenn die Anwendung nicht ausgeführt wird oder nicht an den erwarteten Ports lauscht, der Lauschport blockiert ist oder Netzwerkregeln den Datenverkehr nicht ordnungsgemäß an die Anwendung weiterleiten. In diesem Artikel wird ein methodischer Ansatz zum Ermitteln und Beheben der Probleme beschrieben.

Wenn beim Herstellen einer Verbindung mit dem virtuellen Computer mit RDP oder SSH Probleme auftreten, finden Sie entsprechende Informationen zunächst in den folgenden Artikeln:

 - [Problembehandlung bei Remotedesktopverbindungen mit einem Windows-basierten virtuellen Azure-Computer](../articles/virtual-machines/virtual-machines-windows-troubleshoot-rdp-connection.md)
 - [Behandeln von Problemen mit Secure Shell-Verbindungen (SSH) mit einem Linux-basierten virtuellen Azure-Computer](../articles/virtual-machines/virtual-machines-linux-troubleshoot-ssh-connection.md)

> [AZURE.NOTE] Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Ressourcen-Manager und klassische Bereitstellungen](../articles/resource-manager-deployment-model.md). Dieser Artikel behandelt die Verwendung beider Modelle, Microsoft empfiehlt jedoch für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells.

Wenn Sie beim Lesen dieses Artikels feststellen, dass Sie weitere Hilfe benötigen, können Sie Ihre Frage im [MSDN Azure-Forum oder im Stack Overflow-Forum](https://azure.microsoft.com/support/forums/) stellen, um dort Hilfe von Azure-Experten zu erhalten. Alternativ dazu haben Sie die Möglichkeit, einen Azure-Supportfall zu erstellen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie **Support erhalten** aus.

## Allgemeine Problembehandlung bei Endpunktverbindungen

Wenn beim Herstellen der Verbindung mit einer Anwendung Probleme auftreten, führen Sie die folgenden allgemeinen Schritte zur Problembehandlung aus. Versuchen Sie nach jedem Schritt erneut, eine Verbindung mit der Anwendung herzustellen.

- Starten Sie den virtuellen Computer neu.
- Erstellen Sie den Endpunkt, die Firewallregeln und die Regeln für die Netzwerksicherheitsgruppen (NSG) neu.
	- [Klassisches Modell: Verwalten der Endpunkte der Clouddienste](../articles/cloud-services/cloud-services-enable-communication-role-instances.md)
	- [Resource Manager-Modell: Verwalten von Netzwerksicherheitsgruppen](../articles/virtual-network/virtual-networks-create-nsg-arm-pportal.md)
- Stellen Sie die Verbindung über einen anderen Ort her, z.B. über ein anderes virtuelles Azure-Netzwerk.
- Stellen Sie den virtuellen Computer erneut bereit.
	- [Erneutes Bereitstellen von virtuellen Windows-Computern](../articles/virtual-machines/virtual-machines-windows-redeploy-to-new-node.md)
	- [Erneutes Bereitstellen von virtuellen Linux-Computern](../articles/virtual-machines/virtual-machines-linux-redeploy-to-new-node.md)
- Erstellen Sie den virtuellen Computer neu.

Weitere Informationen finden Sie unter [Problembehandlung bei Endpunktverbindungen (RDP/SSH/HTTP-Fehler usw.)](https://social.msdn.microsoft.com/Forums/azure/de-DE/538a8f18-7c1f-4d6e-b81c-70c00e25c93d/troubleshooting-endpoint-connectivity-rdpsshhttp-etc-failures?forum=WAVirtualMachinesforWindows).

## Übersicht: Ausführliche Problembehandlung

Die Problembehandlung beim Zugriff auf eine Anwendung, die auf einem virtuellen Azure-Computer ausgeführt wird, konzentriert sich auf vier Hauptbereiche.

![Problembehandlung, wenn die Anwendung nicht gestartet werden kann](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access1.png)

1.	Die Anwendung, die auf dem virtuellen Azure-Computer ausgeführt wird.
	- Wird die Anwendung selbst ordnungsgemäß ausgeführt?
2.	Den virtuellen Azure-Computer.
	- Wird der virtuelle Computer selbst ordnungsgemäß ausgeführt, und reagiert er auf Anforderungen?
3.	Azure-Endpunkte für den Clouddienst, der den virtuellen Computer enthält (für virtuelle Computer im klassischen Bereitstellungsmodell), eingehende NAT-Regeln (für virtuelle Computer im Resource Manager-Bereitstellungsmodell) und Netzwerksicherheitsgruppen
	- Ist der Datenverkehrsfluss von den Benutzern zu dem virtuellen Computer bzw. der Anwendung über die erwarteten Ports möglich?
4.	Ihre Internet-Edgegerät.
	- Verhindern Firewallregeln den ordnungsgemäßen Datenverkehrsfluss?

Bei Clientcomputern, die über eine Site-to-Site-VPN- oder ExpressRoute-Verbindung auf die Anwendung zugreifen, sind die Hauptbereiche, die zu Problemen führen können, die Anwendung und der virtuelle Azure-Computer. Gehen Sie folgendermaßen vor, um die Quelle des Problems und dessen Korrektur zu bestimmen.

## Schritt 1: Können Sie von der virtuellen Ziel-VM aus auf die Anwendung zugreifen?

Versuchen Sie, mithilfe des geeigneten Clientprogramms von der VM, auf der es läuft, auf die Anwendung zuzugreifen. Verwenden Sie den lokalen Hostnamen, die lokale IP-Adresse oder die Loopbackadresse (127.0.0.1).

![Anwendung direkt von der VM starten](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access2.png)

Wenn es sich bei der Anwendung beispielsweise um einen Webserver handelt, öffnen Sie einen Browser auf der VM, und versuchen Sie, auf eine Webseite zuzugreifen, die auf der VM gehostet wird.

Wenn Sie auf die Anwendung zugreifen können, fahren Sie mit [Schritt 2](#step2) fort.

Wenn kein Zugriff auf die Anwendung möglich ist, überprüfen Sie Folgendes:

- Die Anwendung wird auf dem virtuellen Zielcomputer ausgeführt.
- Die Anwendung überwacht die erwarteten TCP- und UDP-Ports.

Verwenden Sie bei Windows- und Linux-basierten virtuellen Computern den Befehl **netstat -a**, um die aktiven Überwachungsports anzuzeigen. Überprüfen Sie die Ausgabe für die erwarteten Ports, die die Anwendung überwachen soll. Starten Sie die Anwendung neu, oder konfigurieren Sie sie so, dass die erwarteten Ports nach Bedarf verwendet werden, und versuchen Sie erneut, lokal auf die Anwendung zuzugreifen.

## <a id="step2"></a>Schritt 2: Können von einem anderen virtuellen Computer im gleichen virtuellen Netzwerk aus auf die Anwendung zugreifen?

Versuchen Sie, von einer anderen VM im gleichen virtuellen Netzwerk auf die Anwendung zuzugreifen. Verwenden Sie dazu den VM-Hostnamen oder die von Azure zugewiesene öffentliche, private oder Anbieter-IP-Adresse. Verwenden Sie für mithilfe des klassischen Bereitstellungsmodells erstellte virtuelle Computer nicht die öffentliche IP-Adresse des Clouddiensts.

![Anwendung von einer anderen VM starten](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access3.png)

Wenn es sich bei der Anwendung beispielsweise um einen Webserver handelt, versuchen Sie, in einem Browser auf einer anderen VM im gleichen virtuellen Netzwerk auf eine Webseite zuzugreifen.

Wenn Sie auf die Anwendung zugreifen können, fahren Sie mit [Schritt 3](#step3) fort.

Wenn kein Zugriff auf die Anwendung möglich ist, überprüfen Sie Folgendes:

- Die Hostfirewall auf der Ziel-VM lässt die eingehende Anforderung und den ausgehenden Antwortdatenverkehr zu.
- Software zur Angriffserkennung oder Netzwerküberwachung, die auf der Ziel-VM ausgeführt wird, lässt den Datenverkehr zu.
- Die Endpunkte der Clouddienste oder die Netzwerksicherheitsgruppen lassen den Datenverkehr zu.
	- [Klassisches Modell: Verwalten der Endpunkte der Clouddienste](../articles/cloud-services/cloud-services-enable-communication-role-instances.md)
	- [Resource Manager-Modell: Verwalten von Netzwerksicherheitsgruppen](../articles/virtual-network/virtual-networks-create-nsg-arm-pportal.md)
- Eine separate Komponente, die auf Ihrer VM auf dem Pfad zwischen der Test-VM und Ihrer VM ausgeführt wird, z.B. ein Load Balancer oder eine Firewall, lässt den Datenverkehr zu.

Verwenden Sie auf einem Windows-basierten virtuellen Computer die Windows-Firewall mit erweiterter Sicherheit, um zu bestimmen, ob die Firewallregeln den eingehenden und ausgehenden Datenverkehr der Anwendung ausschließen.

## <a id="step3"></a>Schritt 3: Können Sie von einem Computer aus, der sich außerhalb des virtuellen Netzwerks befindet, aber nicht mit demselben Netzwerk wie Ihr Computer verbunden ist, auf die Anwendung zugreifen?

Versuchen Sie, von einem Computer, der sich nicht im gleichen Netzwerk wie die VM befindet, auf der die Anwendung ausgeführt wird, auf die Anwendung zuzugreifen. Der Computer darf sich aber ebenso wenig im Netzwerk Ihres ursprünglichen Clientcomputers befinden.

![Anwendung von einem Computer außerhalb des virtuellen Netzwerks starten](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access4.png)

Wenn es sich bei der Anwendung beispielsweise um einen Webserver handelt, versuchen Sie, in einem Browser auf eine Webseite zuzugreifen, der auf einem Computer ausgeführt wird, der sich nicht im virtuellen Netzwerk befindet.

Wenn kein Zugriff auf die Anwendung möglich ist, überprüfen Sie Folgendes:

- Bei virtuellen Computern, die mit dem klassischen Bereitstellungsmodell erstellt wurden:
	- Die Endpunktkonfiguration für den virtuellen Computer muss eingehenden Datenverkehr zulassen, insbesondere das Protokoll (TCP oder UDP) und die öffentlichen und privaten Portnummern.
	- Die Zugriffssteuerungslisten (Access Control Lists, ACLs) auf dem Endpunkt dürfen aus dem Internet eingehenden Datenverkehr nicht verhindern.
	- Weitere Informationen finden Sie unter [Einrichten von Endpunkten für einen virtuellen Computer](../articles/virtual-machines/virtual-machines-windows-classic-setup-endpoints.md).

- Bei virtuellen Computern, die mit dem Resource Manager-Bereitstellungsmodell erstellt wurden:
	- Die Konfiguration der eingehenden NAT-Regel für den virtuellen Computer muss eingehenden Datenverkehr zulassen, insbesondere das Protokoll (TCP oder UDP) und die öffentlichen und privaten Portnummern.
	- Netzwerksicherheitsgruppen lassen die eingehende Anforderung und den ausgehende Antwortdatenverkehr zu.
	- Weitere Informationen finden Sie unter [Was ist eine Netzwerksicherheitsgruppe (NSG)?](../articles/virtual-network/virtual-networks-nsg.md).

Wenn der virtuelle Computer oder der Endpunkt Mitglied einer Gruppe mit Lastenausgleich ist:

- Stellen Sie sicher, dass der Prüfpunkt-Protokoll (TCP oder UDP) und die Portnummer richtig sind.
- Wenn das Prüfpunkt-Protokoll und der Port vom Protokoll und Port der Gruppe mit Lastenausgleich abweichen:
	- Überprüfen Sie, ob die Anwendung auf das Testprotokoll (TCP oder UDP) und die Portnummer lauscht (verwenden Sie **netstat –a** auf dem virtuellen Zielcomputer).
	- Die Hostfirewall auf der Ziel-VM lässt die eingehende Testanforderung und den ausgehenden Prüfpunkt-Antwortdatenverkehr zu.

Wenn Sie auf die Anwendung zugreifen können, stellen Sie sicher, dass Ihre Internet-Edgegeräte Folgendes zulassen:

- Den ausgehenden Anwendungsanforderungs-Datenverkehr vom Clientcomputer zum virtuellen Azure-Computer.
- Den eingehenden Anwendungs-Antwortdatenverkehr vom virtuellen Azure-Computer.

## Zusätzliche Ressourcen

[Problembehandlung bei Remotedesktopverbindungen mit einem Windows-basierten virtuellen Azure-Computer](../articles/virtual-machines/virtual-machines-windows-troubleshoot-rdp-connection.md)

[Behandeln von Problemen mit Secure Shell (SSH)-Verbindungen mit einem Linux-basierten virtuellen Azure-Computer](../articles/virtual-machines/virtual-machines-linux-troubleshoot-ssh-connection.md)

<!---HONumber=AcomDC_0713_2016-->