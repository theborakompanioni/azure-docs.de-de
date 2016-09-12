<properties
   pageTitle="Übersicht über Azure-Lastenausgleich | Microsoft Azure"
   description="Übersicht über Features, Architektur und Implementierung des Azure Load Balancers. Erfahren Sie, wie der Load Balancer funktioniert, und nutzen Sie ihn in der Cloud."
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/22/2016"
   ms.author="sewhee" />

# Übersicht über Azure Load Balancer

Der Azure Load Balancer bietet hohe Verfügbarkeit und Netzwerkleistung für Ihre Anwendungen. Es ist ein Layer-4-Lastenausgleichsmodul (TCP, UDP), das eingehenden Datenverkehr auf funktionierende Dienstinstanzen verteilt, die in einer Lastenausgleichsgruppe definiert sind.

## Konfiguration von Azure Load Balancer

Es gibt folgende Konfigurationsmöglichkeiten:

* Lastenausgleich des eingehenden Internetdatenverkehrs für virtuelle Computer. Diese Konfiguration wird als [Lastenausgleich für Internetzugriff](load-balancer-internet-overview.md) bezeichnet.
* Lastenausgleich für Datenverkehr zwischen virtuellen Computern in einem virtuellen Netzwerk, zwischen virtuellen Computern in Clouddiensten oder zwischen lokalen und virtuellen Computern in einem standortübergreifenden virtuellen Netzwerk. Diese Konfiguration wird als [interner Lastenausgleich](load-balancer-internal-overview.md) bezeichnet.
* Weiterleiten von externem Datenverkehr an eine bestimmte Instanz eines virtuellen Computers.

Alle Ressourcen in der Cloud benötigen eine öffentliche IP-Adresse, damit sie im Internet erreichbar sind. Innerhalb der Cloudinfrastruktur verwendet Microsoft Azure nicht routingfähige IP-Adressen für die Ressourcen. Für die Kommunikation mit dem Internet verwendet Azure die Netzwerkadressübersetzung (Network Address Translation, NAT) mit öffentlichen IP-Adressen.

## Azure-Bereitstellungsmodelle

Es ist wichtig, die Unterschiede zwischen dem [klassischen Bereitstellungsmodell und dem Azure Resource Manager-Bereitstellungsmodell](../resource-manager-deployment-model.md) zu verstehen. Azure Load Balancer wird in den beiden Modellen unterschiedlich konfiguriert.

### Klassisches Azure-Bereitstellungsmodell

Bei diesem Modell werden einem Clouddienst eine öffentliche IP-Adresse und ein FQDN zugewiesen. In einem Clouddienst bereitgestellte virtuelle Computer können gruppiert werden, um einen Lastenausgleich zu verwenden. Der Lastenausgleich übernimmt die Portübersetzung und verteilt den Netzwerkdatenverkehr, der über die öffentliche IP-Adresse des Clouddiensts empfangen wird.

Datenverkehr mit Lastenausgleich wird durch die Endpunkte definiert. Die Endpunkte der Portübersetzung bilden eine 1:1-Beziehung zwischen dem öffentlichen zugewiesenen Port der öffentlichen IP-Adresse und dem lokalen Port, der dem Dienst auf einem bestimmten virtuellen Computer zugewiesen wurde. Lastenausgleichs-Endpunkte bilden eine 1:n-Beziehung zwischen der öffentlichen IP-Adresse und den lokalen Ports, die den Diensten auf den virtuellen Computern im Clouddienst zugewiesen sind.

Die Domänenbezeichnung für die öffentliche IP-Adresse, die der Lastenausgleich für dieses Bereitstellungsmodell verwendet, lautet „<Clouddienstname>.cloudapp.net“. Die folgende Grafik zeigt Azure Load Balancer in diesem Modell:

![Azure Load Balancer im klassischen Bereitstellungsmodell](./media/load-balancer-overview/asm-lb.png)

### Azure Resource Manager-Bereitstellungsmodell

Im Resource Manager-Bereitstellungsmodell ist es nicht notwendig, einen Clouddienst zu erstellen. Der Lastenausgleich kann explizit zum Weiterleiten des Datenverkehrs zwischen virtuellen Computern erstellt werden.

Im Resource Manager-Modell ist eine öffentliche IP-Adresse eine eigene Ressource, die einer Domänenbezeichnung oder einem DNS-Namen zugeordnet werden kann. In diesem Fall ist die öffentliche IP-Adresse der Lastenausgleichsressource zugeordnet. Lastenausgleichsregeln und eingehende NAT-Regeln verwenden die öffentliche IP-Adresse als Internetendpunkt für die Ressourcen, die Netzwerkverkehr mit Lastenausgleich empfangen.

Eine private oder öffentliche IP-Adresse wird der Netzwerkschnittstellen-Ressource eines virtuellen Computers zugewiesen. Nachdem eine Netzwerkschnittstelle dem Back-End-IP-Adresspool eines Lastenausgleichsmoduls hinzugefügt wurde, kann der Lastenausgleich basierend auf den erstellten Lastenausgleichsregeln Netzwerkdatenverkehr mit Lastenausgleich senden.

Die folgende Grafik zeigt Azure Load Balancer in diesem Modell:

![Azure Load Balancer in Resource Manager](./media/load-balancer-overview/arm-lb.png)

#### Vorlagenbasierte Bereitstellungen über den Azure-Ressourcen-Manager

Der Load Balancer kann jetzt mithilfe von Resource Manager-APIs und -Tools verwaltet werden. Weitere Informationen zu Resource Manager finden Sie unter [Übersicht über Resource Manager](../resource-group-overview.md).

## Load Balancer-Features

* Hashbasierte Verteilung

    Der Load Balancer verwendet einen Verteilungsalgorithmus auf Hashbasis. Standardmäßig wird ein 5-Tupel-Hash (Quell-IP, Quellport, IP-Zieladresse, Zielport und Protokolltyp) zum Zuordnen des Datenverkehrs an verfügbare Server verwendet. Dabei wird Stickiness nur *innerhalb* einer Transportsitzung angeboten. Pakete in derselben TCP- oder UDP-Sitzung werden an die gleiche Instanz hinter dem Lastenausgleichs-Endpunkt geleitet. Wenn der Client die Verbindung schließt und erneut öffnet oder eine neue Sitzung über die gleiche Quell-IP startet, wird der Quellport geändert. Dadurch wird der Datenverkehr möglicherweise an einen anderen Endpunkt in einem anderen Rechenzentrum geleitet.

    Weitere Informationen finden Sie unter [Load Balancer-Verteilungsmodus](load-balancer-distribution-mode.md). Die folgende Grafik zeigt die hashbasierte Verteilung:

    ![Hashbasierte Verteilung](./media/load-balancer-overview/load-balancer-distribution.png)

* Portweiterleitung

    Mit dem Load Balancer steuern Sie, wie die eingehende Kommunikation verwaltet wird. Diese Kommunikation kann Datenverkehr umfassen, der von Internethosts oder virtuellen Computern in anderen Clouddiensten oder virtuellen Netzwerken initialisiert wird. Diese Steuerung wird durch einen Endpunkt (auch als Eingabeendpunkt bezeichnet) abgebildet.

    Ein Eingabeendpunkt lauscht an einem öffentlichen Port und leitet Datenverkehr an einen internen Port weiter. Sie können dieselben Ports einem internen oder externen Endpunkt zuordnen oder einen anderen Port verwenden. Beispiel: Sie können einen Webserver konfigurieren, der an Port 81 lauscht, während Port 80 dem öffentlichen Endpunkt zugeordnet ist. Die Erstellung eines öffentlichen Endpunkts löst die Erstellung einer Load Balancer-Instanz aus.

    Beim Erstellen über das Azure-Portal erstellt das Portal für den virtuellen Computer automatisch Endpunkte für den Datenverkehr von Remotedesktopprotokoll (RDP) und Windows PowerShell-Remotesitzungen. Über diese Endpunkte können Sie den virtuellen Computer remote über das Internet verwalten.

* Automatische Neukonfiguration

    Der Load Balancer konfiguriert sich selbst sofort neu, wenn Sie Instanzen hoch- oder herunterskalieren. Diese Neukonfiguration kann z.B. erfolgen, wenn Sie die Anzahl der Instanzen für Web-/Workerrollen in einem Clouddienst erhöhen oder wenn Sie weitere virtuelle Computer in die gleiche Gruppe mit Lastenausgleich aufnehmen.

* Dienstüberwachung

    Der Load Balancer kann die Integrität der verschiedenen Serverinstanzen testen. Wenn ein Test nicht reagiert, beendet der Load Balancer das Senden neuer Verbindungen an die fehlerhaften Instanzen. Vorhandene Verbindungen sind nicht betroffen.

    Drei Testarten werden unterstützt:

    + **Gast-Agent-Test (nur PaaS-VMs)****:**Der Load Balancer nutzt den Gast-Agent im virtuellen Computer. Der Gast-Agent lauscht und antwortet nur dann mit dem HTTP-Code „200 OK“, wenn die Instanz sich im Zustand „Bereit“ befindet (d.h. nicht beschäftigt ist bzw. recycelt oder angehalten wird). Wenn der Agent nicht mit dem HTTP-Code „200 OK“ antwortet, kennzeichnet das Lastenausgleichsmodul die Instanz als nicht reagierend und sendet keinen Datenverkehr mehr an diese Instanz. Das Lastenausgleichsmodul pingt die Instanz weiterhin. Wenn der Gast-Agent mit dem HTTP-Code 200 antwortet, sendet der Load Balancer wieder Datenverkehr an diese Instanz. Wenn Sie eine Webrolle verwenden, wird Ihr Websitecode in der Regel in „w3wp.exe“ ausgeführt. Dieses Programm wird nicht von der Azure-Fabric oder vom Gast-Agent überwacht. Das bedeutet, dass Fehler in „w3wp.exe“ (z. B. HTTP 500-Antworten) nicht an den Gast-Agent gemeldet werden und der Load Balancer nicht erkennen kann, dass diese Instanz aus der Rotation entfernt werden soll.
    + **Benutzerdefinierter HTTP-Test:** Dieser Test setzt den (Gast-Agent-)Standardtest außer Kraft. Sie können damit Ihre eigene Logik zum Ermitteln der Integrität der Rolleninstanz erstellen. Der Load Balancer testet regelmäßig Ihren Endpunkt (standardmäßig alle 15 Sekunden). Die Instanz wird als rotierend betrachtet, wenn sie innerhalb des Zeitlimits (standardmäßig 31 Sekunden) mit einem TCP-ACK oder HTTP-Code 200 reagiert. Dies ist hilfreich für die Implementierung Ihrer eigenen Logik zum Entfernen von Instanzen aus der Rotation des Lastenausgleichs. Beispielsweise können Sie die Instanz so konfigurieren, dass sie einen anderen Status als 200 zurückgibt, wenn die Instanz bei über 90 % CPU-Auslastung liegt. Für Webrollen, die „w3wp.exe“ verwenden, wird die Website außerdem automatisch überwacht, da Fehler im Websitecode einen anderen Status als 200 an den Test zurückgeben.
    + **Benutzerdefinierter TCP-Test:** TCP-Tests basieren auf der erfolgreichen Einrichtung von TCP-Sitzungen an einem definierten Testport.

    Weitere Informationen finden Sie unter [LoadBalancerProbe-Schema](https://msdn.microsoft.com/library/azure/jj151530.aspx).

* Quell-NAT

    Der gesamte von Ihrem Dienst gesendete Internetdatenverkehr unterliegt einer Quell-NAT (SNAT, Source Network Address Translation) mit der gleichen VIP-Adresse wie für eingehenden Datenverkehr. SNAT bietet wichtige Vorteile:

    + Sie ermöglicht einfache Upgrades und Notfallwiederherstellung von Diensten, da die VIP-Adresse dynamisch einer anderen Instanz des Diensts zugeordnet werden kann.
    + Sie vereinfacht die Verwaltung von Zugriffssteuerungslisten. Zugriffssteuerungslisten, die als VIPs ausgedrückt werden, ändern sich nicht, wenn Dienste zentral hoch- oder herunterskaliert oder erneut bereitgestellt werden.

    Die Load Balancer-Konfiguration unterstützt vollständige Cone-NAT für UDP. Vollständige Cone-NAT ist ein NAT-Typ, bei dem der Port eingehende Verbindungen von jedem externen Host (als Reaktion auf eine externe Anforderung) erlaubt.


    >[AZURE.NOTE] Für jede neue ausgehende Verbindung, die von einem virtuellen Computer initiiert wird, wird auch ein ausgehender Port vom Azure Load Balancer zugeordnet. Dem externen Host wird Datenverkehr von einem Port angezeigt, der einer virtuellen IP (VIP) zugewiesen ist. In Szenarien, die eine große Anzahl von ausgehenden Verbindungen erfordern, empfiehlt sich die Verwendung [öffentlicher IP-Adressen auf Instanzebene](../virtual-network/virtual-networks-instance-level-public-ip.md), damit die virtuellen Computer eine dedizierte ausgehende IP-Adresse für SNAT besitzen. Dies reduziert das Risiko einer Portauslastung.
    >
    >Die maximale Anzahl von Ports, die von der VIP oder öffentlichen IP auf Instanzebene (PIPs) verwendet werden kann, beträgt 64.000. Dies ist eine standardmäßige Einschränkung für TCP.


### Unterstützung für mehrere IP-Adressen mit Lastenausgleich für virtuelle Computer

Sie können einer Gruppe von virtuellen Computern mehr als eine öffentliche IP-Adresse mit Lastenausgleich zuweisen. Mit dieser Funktion können Sie mehrere SSL-Websites und/oder mehrere Listener für SQL Server-AlwaysOn-Verfügbarkeitsgruppen in der gleichen Gruppe virtueller Computer hosten. Weitere Informationen finden Sie unter [Mehrere VIPs pro Clouddienst](load-balancer-multivip.md).

[AZURE.INCLUDE [load-balancer-compare-tm-ag-lb-include.md](../../includes/load-balancer-compare-tm-ag-lb-include.md)]

## Nächste Schritte

[Lastenausgleich für Internetzugriff (Übersicht)](load-balancer-internet-overview.md)

[Interner Lastenausgleich (Übersicht)](load-balancer-internal-overview.md)

[Erste Schritte zum Erstellen eines Lastenausgleichs für den Internetzugriff](load-balancer-get-started-internet-arm-ps.md)

<!---HONumber=AcomDC_0831_2016-->