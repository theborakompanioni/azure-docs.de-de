<properties
   pageTitle="Übersicht über Azure-Lastenausgleich | Microsoft Azure"
   description="Übersicht über Features, Architektur und Implementierung des Azure Load Balancers. Erfahren Sie, wie der Load Balancer funktioniert, und nutzen Sie ihn in der Cloud."
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/19/2016"
   ms.author="sewhee" />


# Übersicht über Azure Load Balancer

Der Azure Load Balancer bietet Ihren Anwendungen hohe Verfügbarkeit und Netzwerkleistung. Es handelt sich um einen Lastenausgleich des Layer-4-Typs (TCP, UDP), der eingehenden Datenverkehr zwischen funktionierenden Dienstinstanzen in Clouddiensten oder auf virtuelle Computer verteilt, die in einer Gruppe mit Lastenausgleich definiert sind.

Es gibt folgende Konfigurationsmöglichkeiten:

- Lastenausgleich des eingehenden Internetdatenverkehrs für virtuelle Computer. Wir bezeichnen dies als [Lastenausgleich für Internetzugriff](load-balancer-internet-overview.md).
- Lastenausgleich für Datenverkehr zwischen virtuellen Computern in einem virtuellen Netzwerk, zwischen virtuellen Computern in Clouddiensten oder zwischen lokalen und virtuellen Computern in einem standortübergreifenden virtuellen Netzwerk. Wir bezeichnen dies als [internen Lastenausgleich](load-balancer-internal-overview.md).
- Weiterleiten von externem Datenverkehr an eine bestimmte Instanz eines virtuellen Computers.

Alle Ressourcen in der Cloud benötigen eine öffentliche IP-Adresse, damit sie im Internet erreichbar sind. Die Cloudinfrastruktur in Microsoft Azure verwendet für ihre Ressourcen nicht routingfähige IP-Adressen. Für die Kommunikation mit dem Internet nutzt sie die Netzwerkadressübersetzung (NAT) mit öffentlichen IP-Adressen.

Es ist wichtig, die Unterschiede zwischen den [Bereitstellungsmodellen](../resource-manager-deployment-model.md) von klassischem Azure und von Resource Manager zu verstehen, da der Load Balancer jeweils unterschiedlich konfiguriert wird.

### Klassisches Azure-Bereitstellungsmodell

Bei diesem Modell werden einem Clouddienst eine öffentliche IP-Adresse und ein FQDN zugewiesen. In einem Clouddienst bereitgestellte virtuelle Computer können gruppiert werden, um einen Lastenausgleich zu verwenden. Der Load Balancer übernimmt die Portübersetzung und den Lastenausgleich im Netzwerkdatenverkehr, wobei die öffentliche IP-Adresse für den Clouddienst genutzt wird.

Die Portübersetzung erfolgt über Endpunkte, die eine 1:1-Beziehung zwischen dem öffentlichen zugewiesenen Port der öffentlichen IP-Adresse und dem lokalen Port bilden, der zum Senden von Datenverkehr an einen bestimmten virtuellen Computer zugewiesen wurde. Der Lastenausgleich erfolgt über vom Load Balancer festgelegte Endpunkte. Diese Endpunkte haben eine 1:n-Beziehung zwischen der öffentlichen IP-Adresse und den lokalen Ports, die allen virtuellen Computern im Clouddienst zugewiesen sind, der auf den Netzwerkdatenverkehr mit Lastenausgleich reagiert.

Die Domänenbezeichnung für die öffentliche IP-Adresse, die der Load Balancer in diesem Bereitstellungsmodell verwendet, lautet „<Clouddienstname>.cloudapp.net“. Die folgende Grafik zeigt den Azure Load Balancer in diesem Modell.

![Azure Load Balancer im klassischen Bereitstellungsmodell](./media/load-balancer-overview/asm-lb.png)

### Azure Resource Manager-Bereitstellungsmodell

Bei diesem Modell ist es nicht erforderlich, einen Clouddienst für den Lastenausgleich von Datenverkehr zu virtuellen Computern zu erstellen, doch der Load Balancer muss explizit erstellt werden.

Eine öffentliche IP-Adresse ist eine eigene Ressource und kann einer Domänenbezeichnung oder einem DNS-Namen zugeordnet werden. Die öffentliche IP-Adresse ist in diesem Fall der Load Balancer-Ressource zugeordnet. Auf diese Weise verwenden Load Balancer-Regeln und eingehende NAT-Regeln die öffentliche IP-Adresse als Internetendpunkt für die Ressourcen, die Netzwerkverkehr mit Lastenausgleich empfangen.

Eine private oder öffentliche IP-Adresse wird der Netzwerkschnittstellen-Ressource eines virtuellen Computers zugewiesen. Nachdem eine Netzwerkschnittstelle einem IP-Adresspool des Load Balancer-Back-Ends hinzugefügt wurde, beginnt der Load Balancer basierend auf den erstellten Lastenausgleichsregeln mit dem Senden des Netzwerkdatenverkehrs mit Lastenausgleich.

Die folgende Grafik zeigt den Azure Load Balancer in diesem Modell:

![Azure Load Balancer in Resource Manager](./media/load-balancer-overview/arm-lb.png)

## Load Balancer-Features

### Hashbasierte Verteilung

Der Load Balancer verwendet einen Verteilungsalgorithmus auf Hashbasis. Standardmäßig wird ein 5-Tupel-Hash (Quell-IP, Quellport, IP-Zieladresse, Zielport und Protokolltyp) zum Zuordnen des Datenverkehrs an verfügbare Server verwendet. Dabei wird Stickiness nur *innerhalb* einer Transportsitzung angeboten. Pakete in derselben TCP- oder UDP-Sitzung werden an die gleiche Instanz der Rechenzentrums-IP hinter dem Lastenausgleichsendpunkt geleitet. Wenn der Client die Verbindung schließt und erneut öffnet oder eine neue Sitzung über die gleiche Quell-IP startet, wird der Quellport geändert. Dadurch kann der Datenverkehr an einen anderen Rechenzentrums-IP-Endpunkt geleitet werden.

Weitere Informationen finden Sie unter [Load Balancer-Verteilungsmodus](load-balancer-distribution-mode.md). Die folgende Grafik zeigt die hashbasierte Verteilung:

![Hashbasierte Verteilung](./media/load-balancer-overview/load-balancer-distribution.png)

### Portweiterleitung

Mit dem Load Balancer steuern Sie, wie die eingehende Kommunikation verwaltet wird. Diese Kommunikation kann Datenverkehr umfassen, der von Internethosts oder virtuellen Computern in anderen Clouddiensten oder virtuellen Netzwerken initialisiert wird. Diese Steuerung wird durch einen Endpunkt (auch als Eingabeendpunkt bezeichnet) abgebildet.

Ein Endpunkt mit einem öffentlichen Port überwacht Datenverkehr und leitet diesen an einen internen Port. Sie können dieselben Ports einem internen oder externen Endpunkt zuordnen oder einen anderen Port verwenden. Beispiel: Sie können einen Webserver konfigurieren, der an Port 81 lauscht, während Port 80 dem öffentlichen Endpunkt zugeordnet ist. Die Erstellung eines öffentlichen Endpunkts löst die Erstellung einer Load Balancer-Instanz aus.

Die Standardnutzung und -konfiguration von Endpunkten auf einem virtuellen Computer, der über das Azure-Portal erstellt wurde, sind auf das RDP- (Remotedesktopprotokoll) und Windows PowerShell-Sitzungsdatenverkehr ausgerichtet. Über diese Endpunkte können Sie den virtuellen Computer remote über das Internet verwalten.


### Automatische Neukonfiguration

Der Load Balancer konfiguriert sich selbst sofort neu, wenn Sie Instanzen hoch- oder herunterskalieren. Diese Neukonfiguration kann z. B. erfolgen, wenn Sie die Anzahl der Instanzen für Web-/Workerrollen in einem Clouddienst erhöhen oder wenn Sie weitere virtuelle Computer in die gleiche Gruppe mit Lastenausgleich aufnehmen.


### Dienstüberwachung

Der Load Balancer kann die Integrität der verschiedenen Serverinstanzen testen. Wenn ein Test nicht reagiert, beendet der Load Balancer das Senden neuer Verbindungen an die fehlerhaften Instanzen. Vorhandene Verbindungen sind nicht betroffen.

Drei Testarten werden unterstützt:

- **Gast-Agent-Test (nur PaaS-VMs)****:**Der Load Balancer nutzt den Gast-Agent im virtuellen Computer. Er lauscht und antwortet nur dann mit dem HTTP-OK-Code 200, wenn die Instanz sich im Zustand „Bereit“ befindet (d. h. nicht in einem Zustand wie „Beschäftigt“, „Wiederverwendung“ oder „Wird beendet“). Wenn der Gast-Agent nicht mit dem HTTP-OK-Code 200 antwortet, kennzeichnet der Load Balancer die Instanz als nicht reagierend und sendet keinen Datenverkehr mehr an diese Instanz. Der Load Balancer pingt die Instanz weiterhin. Wenn der Gast-Agent mit dem HTTP-Code 200 antwortet, sendet der Load Balancer wieder Datenverkehr an diese Instanz. Wenn Sie eine Webrolle verwenden, wird Ihr Websitecode in der Regel in „w3wp.exe“ ausgeführt. Dieses Programm wird nicht von der Azure-Fabric oder vom Gast-Agent überwacht. Das bedeutet, dass Fehler in „w3wp.exe“ (z. B. HTTP 500-Antworten) nicht an den Gast-Agent gemeldet werden und der Load Balancer nicht erkennen kann, dass diese Instanz aus der Rotation entfernt werden soll.
- **Benutzerdefinierter HTTP-Test:** Dieser Test setzt den (Gast-Agent-)Standardtest außer Kraft. Sie können damit Ihre eigene Logik zum Ermitteln der Integrität der Rolleninstanz erstellen. Der Load Balancer testet regelmäßig Ihren Endpunkt (standardmäßig alle 15 Sekunden). Die Instanz wird als rotierend betrachtet, wenn sie innerhalb des Zeitlimits (standardmäßig 31 Sekunden) mit einem TCP-ACK oder HTTP-Code 200 reagiert. Dies kann für die Implementierung Ihrer eigenen Logik zum Entfernen von Instanzen aus der Load Balancer-Rotation nützlich sein. Beispielsweise können Sie die Instanz so konfigurieren, dass sie einen anderen Status als 200 zurückgibt, wenn die Instanz bei über 90 % CPU-Auslastung liegt. Für Webrollen, die „w3wp.exe“ verwenden, wird die Website außerdem automatisch überwacht, da Fehler im Websitecode einen anderen Status als 200 an den Test zurückgeben.
- **Benutzerdefinierter TCP-Test:** TCP-Tests basieren auf der erfolgreichen Einrichtung von TCP-Sitzungen an einem definierten Testport.

Weitere Informationen finden Sie unter [LoadBalancerProbe-Schema](https://msdn.microsoft.com/library/azure/jj151530.aspx).

### Quell-NAT


Auf den gesamten von Ihrem Dienst ausgehenden Internetdatenverkehr wird Quell-NAT (SNAT, Source Network Address Translation) mit der gleichen VIP-Adresse wie für eingehenden Datenverkehr angewendet. SNAT bietet wichtige Vorteile:

- Sie ermöglicht einfache Upgrades und Notfallwiederherstellung von Diensten, da die VIP-Adresse dynamisch einer anderen Instanz des Diensts zugeordnet werden kann.
- Sie vereinfacht die ACL-Verwaltung (Access Control List, Zugriffssteuerungsliste), da die ACL im Hinblick auf VIPs ausgedrückt werden kann. Daher müssen Sie keine Änderungen vornehmen, wenn Sie Dienste zentral hoch- oder herunterskalieren oder erneut bereitstellen.

Die Load Balancer-Konfiguration unterstützt vollständige Cone-NAT für UDP. Vollständige Cone-NAT ist ein NAT-Typ, bei dem der Port eingehende Verbindungen von jedem externen Host (als Reaktion auf eine externe Anforderung) erlaubt.


>[AZURE.NOTE] Für jede neue ausgehende Verbindung, die von einem virtuellen Computer initiiert wird, wird auch ein ausgehender Port vom Azure Load Balancer zugeordnet. Dem externen Host wird Datenverkehr als mit einer virtuellen IP (VIP) zugewiesener Port angezeigt. Wenn für Ihre Szenarien eine große Anzahl von ausgehenden Verbindungen benötigt wird, empfehlen wir für die virtuellen Computer die Verwendung [öffentlicher IP-Adressen auf Instanzebene](../virtual-network/virtual-networks-instance-level-public-ip.md), damit sie eine dedizierte ausgehende IP-Adresse für SNAT besitzen. Dies reduziert das Risiko von Portauslastung.
>
>Die maximale Anzahl von Ports, die von der VIP oder öffentlichen IP auf Instanzebene (PIPs) verwendet werden kann, beträgt 64.000. Dies ist eine standardmäßige Einschränkung für TCP.


####Unterstützung für mehrere IP-Adressen mit Lastenausgleich für virtuelle Computer

Sie können einer Gruppe von virtuellen Computern mehr als eine öffentliche IP-Adresse mit Lastenausgleich zuweisen. Mit dieser Funktion können Sie mehrere SSL-Websites und/oder mehrere Listener für SQL Server-AlwaysOn-Verfügbarkeitsgruppen in der gleichen Gruppe virtueller Computer hosten. Weitere Informationen finden Sie unter [Mehrere VIPs pro Clouddienst](load-balancer-multivip.md).

####Vorlagenbasierte Bereitstellungen über den Azure-Ressourcen-Manager####

Der Load Balancer kann jetzt mithilfe von Resource Manager-APIs und -Tools verwaltet werden. Weitere Informationen zu Resource Manager finden Sie unter [Übersicht über Azure Resource Manager](../resource-group-overview.md).

[AZURE.INCLUDE [load-balancer-compare-tm-ag-lb-include.md](../../includes/load-balancer-compare-tm-ag-lb-include.md)]

## Nächste Schritte

[Lastenausgleich für Internetzugriff (Übersicht)](load-balancer-internet-overview.md)

[Interner Lastenausgleich (Übersicht)](load-balancer-internal-overview.md)

[Erste Schritte zum Erstellen eines Lastenausgleichs für den Internetzugriff](load-balancer-get-started-internet-arm-ps.md)

<!---HONumber=AcomDC_0824_2016-->