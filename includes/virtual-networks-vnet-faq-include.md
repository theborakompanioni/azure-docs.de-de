## <a name="virtual-network-basics"></a>Grundlagen zu Virtual Networks
### <a name="what-is-an-azure-virtual-network-vnet"></a>Was ist ein Azure Virtual Network (VNet)?
Mit VNets können Sie virtuelle private Netzwerke (VPNs) in Azure bereitstellen und verwalten und die VNets optional mit anderen VNets in Azure oder mit Ihrer lokalen IT-Infrastruktur verbinden, um hybride oder standortübergreifende Lösungen zu erstellen. Jedes erstellte VNet verfügt über einen eigenen CIDR-Block und kann mit anderen VNets und lokalen Netzwerken verbunden werden, solange die CIDR-Blöcke nicht im Konflikt miteinander stehen. Sie können zudem Steuerelemente der DNS-Servereinstellungen für VNets festlegen und das VNet in Subnetze segmentieren.

Verwenden Sie VNets für Folgendes:

* Erstellen eines dedizierten privaten virtuellen Nur-Cloud-Netzwerks
  
    Manchmal benötigen Sie keine standortübergreifende Konfiguration für Ihre Lösung. Wenn Sie ein VNet erstellen, können die Dienste und virtuellen Computer des VNet direkt und sicher in der Cloud miteinander kommunizieren. Auf diese Weise verbleibt der Datenverkehr sicher im VNet, Sie können jedoch trotzdem in Ihrer Lösung Endpunktverbindungen für die virtuellen Computer und Dienste konfigurieren, die eine Internetkommunikation erfordern.
* Sicheres Erweitern des Rechenzentrums
  
    Mit VNets können Sie herkömmliche Standort-zu-Standort (S2S)-VPNs erstellen, um die Kapazität des Rechenzentrums sicher zu skalieren. S2S-VPNs verwenden IPsec, um eine sichere Verbindung zwischen dem unternehmenseigenen VPN-Gateway und Azure bereitzustellen.
* Unterstützung von Hybrid Cloud-Szenarios
  
    VNets bieten flexible Möglichkeiten, verschiedene Hybrid Cloud-Szenarios zu unterstützen. Sie können cloudbasierte Anwendungen auf sichere Weise mit beliebigen lokalen Systemen wie z. B. Mainframes oder Unix-Systemen verbinden.

### <a name="how-do-i-know-if-i-need-a-virtual-network"></a>Wie finde ich heraus, ob ich ein virtuelles Netzwerk benötige?
Auf der Webseite [Virtuelle Netzwerke – Übersicht](../articles/virtual-network/virtual-networks-overview.md) finden Sie eine Entscheidungstabelle, anhand derer Sie eine Wahl bezüglich des für Sie am besten geeigneten Netzwerkentwurfs treffen können.

### <a name="how-do-i-get-started"></a>Wie fange ich an?
Auf der Webseite [Dokumentation zu virtuellen Netzwerken](https://azure.microsoft.com/documentation/services/virtual-network/) finden Sie Informationen zu den ersten Schritten. Auf dieser Seite befinden sich Links zu allgemeinen Konfigurationsschritten sowie Informationen zu den verschiedenen Aspekten, die Sie berücksichtigen müssen, wenn Sie Ihr virtuelles Netzwerk entwerfen.

### <a name="what-services-can-i-use-with-vnets"></a>Welche Dienste können in VNets genutzt werden?
VNets können mit einer Vielzahl unterschiedlicher Azure-Dienste, z. B. Cloud Services (PaaS), Virtual Machines und Web-Apps, verwendet werden. Es gibt jedoch auch einige Dienste, die in einem VNet nicht unterstützt werden. Überprüfen Sie den spezifischen Dienst, den Sie verwenden möchten, und vergewissern Sie sich, ob er kompatibel ist.

### <a name="can-i-use-vnets-without-cross-premises-connectivity"></a>Können VNets ohne standortübergreifende Konnektivität verwendet werden?
Ja. Ein VNet kann auch ohne Standort-zu-Standort-Konnektivität genutzt werden. Dies ist insbesondere von Nutzen, wenn Sie Domänencontroller und SharePoint-Farmen in Azure ausführen möchten.

## <a name="virtual-network-configuration"></a>Konfiguration von virtuellen Netzwerken
### <a name="what-tools-do-i-use-to-create-a-vnet"></a>Welche Tools werden zum Erstellen eines VNet verwendet?
Sie können die folgenden Tools zum Erstellen oder Konfigurieren eines virtuellen Netzwerks verwenden:

* Azure-Portal (für klassische VNets und Resource Manager-VNets).
* Eine Netzwerkkonfigurationsdatei (NETCFG-Datei, nur für klassische VNets). Siehe [Konfigurieren eines virtuellen Netzwerks mithilfe einer Netzwerkkonfigurationsdatei](../articles/virtual-network/virtual-networks-using-network-configuration-file.md).
* PowerShell (für klassische VNets und Resource Manager-VNets)
* Azure-Befehlszeilenschnittstelle (für klassische VNets und Resource Manager-VNets)

### <a name="what-address-ranges-can-i-use-in-my-vnets"></a>Welche Adressbereiche kann ich in meinen VNets verwenden?
Sie können öffentliche IP-Adressbereiche und alle in [RFC 1918](http://tools.ietf.org/html/rfc1918)definierten IP-Adressbereiche verwenden.

### <a name="can-i-have-public-ip-addresses-in-my-vnets"></a>Können öffentliche IP-Adressen in VNets verwendet werden?
Ja. Weitere Informationen zu öffentlichen IP-Adressbereichen finden Sie unter [Öffentlicher IP-Adressraum in einem virtuellen Netzwerk (VNet)](../articles/virtual-network/virtual-networks-public-ip-within-vnet.md). Beachten Sie, dass auf Ihre öffentlichen IPs nicht direkt über das Internet zugegriffen werden kann.

### <a name="is-there-a-limit-to-the-number-of-subnets-in-my-virtual-network"></a>Ist die Anzahl der Subnetze im virtuellen Netzwerk begrenzt?
Sie können eine unbegrenzte Anzahl von Subnetzen in einem VNet verwenden. Alle Subnetze müssen vollständig im Adressraum des virtuellen Netzwerks enthalten sein und sollten sich nicht überschneiden.

### <a name="are-there-any-restrictions-on-using-ip-addresses-within-these-subnets"></a>Unterliegen die in den Subnetzen verwendeten IP-Adressen bestimmten Beschränkungen?
Einige IP-Adressen innerhalb jedes Subnetzes sind in Azure reserviert. Die erste und letzte IP-Adresse der Subnetze sind aus Gründen der Protokollkonformität reserviert. Darüber hinaus sind drei weitere für Azure-Dienste verwendete IP-Adressen reserviert.

### <a name="how-small-and-how-large-can-vnets-and-subnets-be"></a>Wie ist die minimale und maximale Größe von VNets und Subnetzen?
Das kleinste unterstützte Subnetz weist die Netzmaske /29 und das größte die Netzmaske /8 (gemäß CIDR-Subnetzdefinitionen) auf.

### <a name="can-i-bring-my-vlans-to-azure-using-vnets"></a>Können VLANs mithilfe von VNets in Azure integriert werden?
Nein. VNets sind Layer-3-Overlays. Layer-2-Semantik wird in Azure nicht unterstützt.

### <a name="can-i-specify-custom-routing-policies-on-my-vnets-and-subnets"></a>Können benutzerdefinierte Routingrichtlinien für VNets und Subnetze angegeben werden?
Ja. Sie können benutzerdefinierte Routen (UDR) verwenden. Weitere Informationen zu UDR finden Sie unter [Benutzerdefinierte Routen und IP-Weiterleitung](../articles/virtual-network/virtual-networks-udr-overview.md).

### <a name="do-vnets-support-multicast-or-broadcast"></a>Unterstützen VNets Multicasting oder Broadcasting?
Nein. Wir bieten keine Multicasting- oder Broadcastingunterstützung.

### <a name="what-protocols-can-i-use-within-vnets"></a>Welche Protokolle werden innerhalb von VNets unterstützt?
Sie können innerhalb von VNets IP-basierte Standardprotokolle verwenden. Verkapselte Multicast-, Broadcast- und IP-in-IP-Pakete sowie GRE (Generic Routing Encapsulation)-Pakete werden jedoch blockiert. Unterstützte Standardprotokolle sind:

* TCP
* UDP
* ICMP

### <a name="can-i-ping-my-default-routers-within-a-vnet"></a>Kann ich meine Standardrouter innerhalb eines VNet mit "ping" abfragen?
Nein.

### <a name="can-i-use-tracert-to-diagnose-connectivity"></a>Können Verbindungsdiagnosen mit "tracert" durchgeführt werden?
Nein.

### <a name="can-i-add-subnets-after-the-vnet-is-created"></a>Können Subnetze hinzugefügt werden, nachdem das VNet erstellt wurde?
Ja. Subnetze können VNets jederzeit hinzugefügt werden, solange die Subnetzadresse nicht Teil eines anderen Subnetzes im VNet ist.

### <a name="can-i-modify-the-size-of-my-subnet-after-i-create-it"></a>Kann die Größe des Subnetzes nach dessen Erstellung geändert werden?
Sie können mit PowerShell-Cmdlets oder der NETCFG-Datei ein Subnetz hinzufügen, entfernen, erweitern oder verkleinern, wenn in diesem Subnetz keine virtuellen Computer oder Dienste bereitgestellt werden. Sie können auch beliebige Präfixe hinzufügen, entfernen, erweitern oder verkleinern, solange die Subnetze, die virtuelle Computer oder Dienste enthalten, nicht von der Änderung betroffen sind.

### <a name="can-i-modify-subnets-after-i-created-them"></a>Können Subnetze nach dem Erstellen geändert werden?
Ja. Sie können die in einem VNet verwendeten CIDR-Blöcke hinzufügen, entfernen und ändern.

### <a name="can-i-connect-to-the-internet-if-i-am-running-my-services-in-a-vnet"></a>Kann eine Verbindung mit dem Internet hergestellt werden, wenn Dienste in einem VNet ausgeführt werden?
Ja. Alle Dienste, die innerhalb eines VNet bereitgestellt werden, können eine Verbindung mit dem Internet herstellen. Jeder in Azure bereitgestellte Clouddienst verfügt über eine öffentlich adressierbare, zugewiesene VIP-Adresse. Sie müssen Eingabeendpunkte für PaaS-Rollen und Endpunkte für virtuelle Computer definieren, damit diese Dienste Verbindungen über das Internet annehmen können.

### <a name="do-vnets-support-ipv6"></a>Unterstützen VNets IPv6?
Nein. Zum gegenwärtigen Zeitpunkt können Sie IPv6 mit VNets nicht verwenden.

### <a name="can-a-vnet-span-regions"></a>Kann sich ein VNet über mehrere Regionen erstrecken?
Nein. Ein VNet ist auf eine Region beschränkt.

### <a name="can-i-connect-a-vnet-to-another-vnet-in-azure"></a>Kann ein VNet mit einem anderen VNet in Azure verbunden werden?
Ja. Sie können VNet-zu-VNet-Kommunikation mithilfe der REST-APIs oder Windows PowerShell erstellen. VNets können auch mittels VNet-Peering verbunden werden. Ausführlichere Informationen zu Peering finden Sie [hier](../articles/virtual-network/virtual-network-peering-overview.md).

## <a name="name-resolution-dns"></a>Namensauflösung (DNS)
### <a name="what-are-my-dns-options-for-vnets"></a>Welche DNS-Optionen sind für VNets verfügbar?
Eine Übersicht über die verfügbaren DNS-Optionen finden Sie in der Entscheidungstabelle auf der Seite [Namensauflösung für virtuelle Computer und Rolleninstanzen](../articles/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) .

### <a name="can-i-specify-dns-servers-for-a-vnet"></a>Können DNS-Server für ein VNet angegeben werden?
Ja. Sie haben die Möglichkeit, IP-Adressen von DNS-Servern in den Einstellungen des VNet anzugeben. Diese gelten als DNS-Standardserver für alle virtuellen Computer im VNet.

### <a name="how-many-dns-servers-can-i-specify"></a>Wie viele DNS-Server können angegeben werden?
Sie können bis zu 12 DNS-Server angeben.

### <a name="can-i-modify-my-dns-servers-after-i-have-created-the-network"></a>Können DNS-Server geändert werden, nachdem das Netzwerk erstellt wurde?
Ja. Sie können die Liste der DNS-Server für das VNet jederzeit ändern. Wenn Sie die Liste der DNS-Server ändern, müssen Sie jeden virtuellen Computer im VNet neu starten, damit der neue DNS-Server übernommen wird.

### <a name="what-is-azure-provided-dns-and-does-it-work-with-vnets"></a>Was ist der von Azure bereitgestellte DNS-Dienst, und wie wird er bei VNets verwendet?
Der von Azure bereitgestellte DNS-Dienst ist ein von Microsoft angebotener mehrinstanzenfähiger DNS-Dienst. In Azure werden alle Ihre virtuellen Computer und Rolleninstanzen in diesem Dienst registriert. Dieser Dienst stellt die Namensauflösung nach dem Hostnamen für virtuelle Computer und Rolleninstanzen, die im gleichen Clouddienst enthalten sind, und nach dem FQDN für virtuelle Computer und Rolleninstanzen im gleichen VNet zur Verfügung.

> [!NOTE]
> Die mandantenübergreifende Namensauflösung mithilfe des von Azure bereitgestellten DNS-Diensts ist derzeit auf die ersten 100 Clouddienste im virtuellen Netzwerk beschränkt. Diese Einschränkung gilt nicht, wenn Sie einen eigenen DNS-Server verwenden.
> 
> 

### <a name="can-i-override-my-dns-settings-on-a-per-vm--service-basis"></a>Können DNS-Einstellungen für einzelne virtuelle Computer und Dienste überschrieben werden?
Ja. Sie können DNS-Server pro Clouddienst festlegen, um die standardmäßigen Netzwerkeinstellungen zu überschreiben. Es wird jedoch empfohlen, nach Möglichkeit den netzwerkweiten DNS-Server zu verwenden.

### <a name="can-i-bring-my-own-dns-suffix"></a>Können benutzerdefinierte DNS-Suffixe angegeben werden?
Nein. Sie können kein benutzerdefiniertes DNS-Suffix für die VNets angeben.

## <a name="vnets-and-vms"></a>VNets und virtuelle Computer
### <a name="can-i-deploy-vms-to-a-vnet"></a>Können virtuelle Computer in einem VNet bereitgestellt werden?
Ja.

### <a name="can-i-deploy-linux-vms-to-a-vnet"></a>Können virtuelle Linux-Computer in einem VNet bereitgestellt werden?
Ja. Sie können alle von Azure unterstützten Linux-Distributionen bereitstellen.

### <a name="what-is-the-difference-between-a-public-vip-and-an-internal-ip-address"></a>Worin besteht der Unterschied zwischen einer öffentlichen VIP-Adresse und einer internen IP-Adresse?
* Eine interne IP-Adresse ist eine IP-Adresse, die den einzelnen virtuellen Computern eines VNet über DHCP zugewiesen wird. Sie ist nicht öffentlich. Wenn Sie ein VNet erstellt haben, wird die interne IP-Adresse aus dem Bereich zugewiesen, den Sie in den Subnetzeinstellungen des VNet angegeben haben. Auch wenn Sie nicht über ein VNet verfügen, wird dennoch eine interne IP-Adresse zugewiesen. Die interne IP-Adresse bleibt dem virtuellen Computer während seiner gesamten Lebensdauer zugewiesen, sofern seine Zuordnung nicht aufgehoben wird.
* Eine öffentliche VIP-Adresse ist die öffentliche IP-Adresse, die dem Clouddienst oder Load Balancer zugewiesen wird. Sie wird der NIC des virtuellen Computers nicht direkt zugewiesen. Die VIP-Adresse wird für den Clouddienst, dem sie zugewiesen ist, beibehalten, bis die Zuordnung aller virtuellen Computer in diesem Clouddienst aufgehoben wird oder alle virtuellen Computer gelöscht werden. Zu diesem Zeitpunkt wird sie freigegeben.

### <a name="what-ip-address-will-my-vm-receive"></a>Welche IP-Adresse wird meinem virtuellen Computer zugewiesen?
* **Interne IP-Adresse:** Wenn Sie einen virtuellen Computer in einem VNet bereitstellen, erhält der virtuelle Computer immer eine interne IP-Adresse aus einem Pool interner IP-Adressen, den Sie angeben. Virtuelle Computer kommunizieren im VNet mithilfe interner IP-Adressen. In Azure wird eine dynamische interne IP-Adresse zugewiesen, Sie können jedoch eine statische Adresse für Ihren virtuellen Computer anfordern. Weitere Informationen zu statischen internen IP-Adressen finden Sie unter [Festlegen einer statischen internen IP-Adresse](../articles/virtual-network/virtual-networks-reserved-private-ip.md).
* **VIP:** Ihr virtueller Computer wird auch einer VIP-Adresse zugeordnet. Eine VIP-Adresse wird dem virtuellen Computer jedoch niemals direkt zugewiesen. Eine VIP-Adresse ist eine öffentliche IP-Adresse, die dem Clouddienst zugewiesen werden kann. Sie können optional eine VIP-Adresse für Ihren Clouddienst reservieren.
* **ILPIP:** Sie können außerdem eine öffentliche IP-Adresse auf Instanzebene (ILPIP) konfigurieren. ILPIPs sind dem virtuellen Computer direkt zugeordnet und nicht dem Clouddienst. Weitere Informationen zu ILPIPs finden Sie unter [Übersicht über die öffentliche IP auf Instanzebene](../articles/virtual-network/virtual-networks-instance-level-public-ip.md).

### <a name="can-i-reserve-an-internal-ip-address-for-a-vm-that-i-will-create-at-a-later-time"></a>Kann ich eine interne IP-Adresse für einen virtuellen Computer reservieren, den ich zu einem späteren Zeitpunkt erstelle?
Nein. Sie können keine interne IP-Adresse reservieren. Wenn eine interne IP-Adresse verfügbar ist, wird sie durch den DHCP-Server einem virtuellen Computer oder einer Rolleninstanz zugewiesen. Dieser virtuelle Computer ist möglicherweise der Computer, dem Sie die interne IP-Adresse zuweisen möchten, möglicherweise aber auch nicht. Sie können jedoch die interne IP-Adresse eines bereits erstellten virtuellen Computers in eine verfügbare interne IP-Adresse ändern.

### <a name="do-internal-ip-addresses-change-for-vms-in-a-vnet"></a>Ändern sich interne IP-Adressen für virtuelle Computer in einem VNet?
Ja. Interne IP-Adressen bleiben dem virtuellen Computer während seiner gesamten Lebensdauer zugewiesen, sofern diese Zuordnung nicht aufgehoben wird. Nach dem Aufheben der Zuordnung eines virtuellen Computers wird die interne IP-Adresse freigegeben, sofern Sie keine statische interne IP-Adresse definiert haben. Wenn der virtuelle Computer einfach beendet (und nicht in den Status **Beendet (Zuordnung aufgehoben)**versetzt) wird, bleibt die IP-Adresse dem virtuellen Computer weiterhin zugewiesen.

### <a name="can-i-manually-assign-ip-addresses-to-nics-in-vms"></a>Können IP-Adressen manuell NICs in virtuellen Computern zugewiesen werden?
Nein. Schnittstelleneigenschaften von virtuellen Computern dürfen nicht geändert werden. Änderungen können dazu führen, dass die Verbindung mit dem virtuellen Computer unterbrochen wird.

### <a name="what-happens-to-my-ip-addresses-if-i-shut-down-a-vm"></a>Was geschieht mit den IP-Adressen, wenn ein virtueller Computer heruntergefahren wird?
Nichts. Die IP-Adressen (sowohl die öffentliche VIP-Adresse als auch die interne IP-Adresse) bleiben dem Clouddienst oder virtuellen Computer weiterhin zugewiesen.

> [!NOTE]
> Wenn Sie den virtuellen Computer einfach herunterfahren möchten, verwenden Sie dazu nicht das Verwaltungsportal. Derzeit wird über die Schaltfläche zum Herunterfahren die Zuordnung des virtuellen Computers aufgehoben.
> 
> 

### <a name="can-i-move-vms-from-one-subnet-to-another-subnet-in-a-vnet-without-re-deploying"></a>Können virtuelle Computer ohne erneute Bereitstellung zwischen Subnetzen in einem VNet verschoben werden?
Ja. Weitere Informationen finden Sie [hier](../articles/virtual-network/virtual-networks-move-vm-role-to-subnet.md).

### <a name="can-i-configure-a-static-mac-address-for-my-vm"></a>Kann eine statische MAC-Adresse für einen virtuellen Computer konfiguriert werden?
Nein. Eine MAC-Adresse kann nicht statisch konfiguriert werden.

### <a name="will-the-mac-address-remain-the-same-for-my-vm-once-it-has-been-created"></a>Bleibt die MAC-Adresse eines virtuellen Computers nach ihrer Erstellung unverändert?
Ja, die MAC-Adresse bleibt für einen virtuellen Computer gleich, auch wenn der virtuelle Computer beendet (also seine Zuordnung aufgehoben) wurde und der virtuelle Computer neu gestartet wird.

### <a name="can-i-connect-to-the-internet-from-a-vm-in-a-vnet"></a>Kann ich von einem virtuellen Computer in einem VNet eine Verbindung mit dem Internet herstellen?
Ja. Alle Dienste, die innerhalb eines VNet bereitgestellt werden, können eine Verbindung mit dem Internet herstellen. Zusätzlich verfügt jeder in Azure bereitgestellte Clouddienst über eine öffentlich adressierbare zugewiesene VIP-Adresse. Sie müssen Eingabeendpunkte für PaaS-Rollen und Endpunkte für virtuelle Computer definieren, damit diese Dienste Verbindungen über das Internet annehmen können.

## <a name="vnets-and-services"></a>VNets und Dienste
### <a name="what-services-can-i-use-with-vnets"></a>Welche Dienste können in VNets genutzt werden?
Sie können innerhalb von VNets ausschließlich Compute Services verwenden. Compute Services sind auf Clouddienste (Web- und Workerrollen) und virtuelle Computer beschränkt.

### <a name="can-i-use-web-apps-with-virtual-network"></a>Können Web-Apps mit dem virtuellen Netzwerk verwendet werden?
Ja. Sie können Web-Apps in einem VNet mit ASE (App Service Environment, App Service-Umgebung) bereitstellen. Web-Apps können außerdem eine sichere Verbindung herstellen und auf Ressourcen im Azure-VNet zugreifen, wenn Sie für Ihr VNet „Punkt-zu-Site“ konfiguriert haben. Weitere Informationen finden Sie unter den folgenden Links:

* [Erstellen von Web-Apps in einer App Service-Umgebung](../articles/app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md)
* [Web Apps Virtual Network Integration (in englischer Sprache)](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/)
* [Using VNet Integration and Hybrid Connections with Web Apps (in englischer Sprache)](https://azure.microsoft.com/blog/2014/10/30/using-vnet-or-hybrid-conn-with-websites/)
* [Integrieren einer Web-App in einem Azure Virtual Network](../articles/app-service-web/web-sites-integrate-with-vnet.md)

### <a name="can-i-deploy-cloud-services-with-web-and-worker-roles-paas-in-a-vnet"></a>Können Clouddienste mit Web- und Workerrollen (PaaS) in einem VNet bereitgestellt werden?
Ja. Sie können PaaS-Dienste innerhalb von VNets bereitstellen.

### <a name="how-do-i-deploy-paas-roles-to-a-vnet"></a>Wie werden PaaS-Rollen für ein VNet bereitgestellt?
Dazu geben Sie den Namen des VNet und die Rollen-/Subnetzzuordnungen im Netzwerkkonfigurationsabschnitt der Dienstkonfiguration an. Sie müssen keine Binärdateien aktualisieren.

### <a name="can-i-move-my-services-in-and-out-of-vnets"></a>Können Dienste in und aus VNets verschoben werden?
Nein. Sie können Dienste nicht in oder aus VNets verschieben. Sie müssen den entsprechenden Dienst löschen und erneut bereitstellen, um ihn in ein anderes VNet zu verschieben.

## <a name="vnets-and-security"></a>VNets und Sicherheit
### <a name="what-is-the-security-model-for-vnets"></a>Welches Sicherheitsmodell wird für VNets verwendet?
VNets werden von anderen VNets und anderen in der Azure-Infrastruktur gehosteten Diensten vollständig isoliert ausgeführt. Die Vertrauensstellungsgrenze entspricht der Begrenzung des VNet.

### <a name="can-i-define-acls-or-nsgs-on-my-vnets"></a>Können ACLs oder NSGs für VNets definiert werden?
Nein. Sie können VNets keine ACLs oder NSGs zuordnen. ACLs können jedoch für Eingabeendpunkte virtueller Computer definiert werden, die in einem VNet bereitgestellt wurden. NSGs können Subnetzen oder NICs zugeordnet werden.

### <a name="is-there-a-vnet-security-whitepaper"></a>Gibt es ein Whitepaper zum Thema VNet-Sicherheit?
Ja. Sie können es [hier](http://go.microsoft.com/fwlink/?LinkId=386611)herunterladen.

## <a name="apis-schemas-and-tools"></a>APIs, Schemas und Tools
### <a name="can-i-manage-vnets-from-code"></a>Können VNets programmgesteuert verwaltet werden?
Ja. VNets und die standortübergreifende Konnektivität können mithilfe von REST-APIs verwaltet werden. Weitere Informationen finden Sie [hier](http://go.microsoft.com/fwlink/?LinkId=296833).

### <a name="is-there-tooling-support-for-vnets"></a>Sind Tools zur Unterstützung von VNets verfügbar?
Ja. Sie können PowerShell und Befehlszeilentools für zahlreiche Plattformen verwenden. Weitere Informationen finden Sie [hier](http://go.microsoft.com/fwlink/?LinkId=317721).

