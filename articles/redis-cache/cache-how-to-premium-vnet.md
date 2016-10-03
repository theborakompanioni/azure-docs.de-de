<properties 
	pageTitle="Konfigurieren der Unterstützung virtueller Netzwerke für Azure Redis Cache vom Typ „Premium“ | Microsoft Azure" 
	description="Erfahren Sie, wie Sie die Unterstützung für virtuelle Netzwerke für Azure Redis Cache-Instanzen im Tarif ";Premium"; erstellen und verwalten." 
	services="redis-cache" 
	documentationCenter="" 
	authors="steved0x" 
	manager="douge" 
	editor=""/>

<tags 
	ms.service="cache" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="cache-redis" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="09/15/2016" 
	ms.author="sdanie"/>

# Konfigurieren der Unterstützung virtueller Netzwerke für Azure Redis Cache vom Typ "Premium"
Für Azure Redis Cache stehen verschiedene Cacheangebote bereit, die Flexibilität bei der Auswahl von Cachegröße und -features bieten, einschließlich des neuen Premium-Tarifs.

Der Premium-Tarif für Azure Redis Cache umfasst Clustering, Persistenz und die Unterstützung virtueller Netzwerke (VNet). Ein VNet ist ein privates Netzwerk in der Cloud. Wenn eine Azure Redis Cache-Instanz mit einem VNet konfiguriert wird, ist dieses nicht öffentlich adressierbar, und auf das VNet kann nur über virtuelle Computer und Anwendungen innerhalb des VNet zugegriffen werden. In diesem Artikel wird erläutert, wie die Unterstützung eines virtuellen Netzwerks für eine Azure Redis Cache-Instanz vom Typ „Premium“ konfiguriert wird.

>[AZURE.NOTE] Azure Redis Cache unterstützt sowohl klassische als auch ARM-VNets.

Weitere Informationen zu anderen Premium-Cache-Features finden Sie unter [Einführung in den Premium-Tarif von Azure Redis Cache](cache-premium-tier-intro.md).

## Gründe für ein VNet
Die Bereitstellung von [Azure Virtual Network (VNet)](https://azure.microsoft.com/services/virtual-network/) bietet eine verbesserte Sicherheit und Isolation für den Azure Redis Cache sowie Subnetze, Zugriffssteuerungsrichtlinien und andere Features, mit denen der Zugriff auf Azure Redis Cache weiter eingeschränkt werden kann.

## Unterstützung für virtuelle Netzwerke
Die Unterstützung für ein virtuelles Netzwerk (VNet) wird während der Erstellung des Caches auf dem Blatt **Neuer Redis Cache** konfiguriert.

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

Nachdem Sie einen Premium-Tarif ausgewählt haben, können Sie die Azure Redis Cache-VNet-Integration konfigurieren, indem Sie ein virtuelles Netzwerk auswählen, das sich in demselben Abonnement und im gleichen Speicherort wie Ihr Cache befindet. Um ein neues virtuelles Netzwerk verwenden zu können, müssen Sie es zuerst erstellen. Befolgen Sie dazu die Anleitungen unter [Erstellen eines virtuellen Netzwerks über das Azure-Portal](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) und [Erstellen eines virtuellen Netzwerks (klassisch) über das Azure-Portal](../virtual-network/virtual-networks-create-vnet-classic-portal.md). Kehren Sie anschließend zum Blatt **Neuer Redis Cache** zurück, um Ihrem Premium-Cache zu erstellen und zu konfigurieren.

Klicken Sie zum Konfigurieren des virtuelles Netzwerks für Ihren neuen Cache auf dem Blatt **Neuer Redis Cache** auf **Virtuelles Netzwerk**, und wählen Sie das gewünschte virtuelle Netzwerk aus der Dropdownliste aus.

![Virtuelles Netzwerk][redis-cache-vnet]

Wählen Sie in der Dropdownliste **Subnetz** das gewünschte Subnetz aus, und geben Sie die gewünschte **statische IP-Adresse** ein. Wenn Sie ein klassisches virtuelles Netzwerk verwenden, ist das Feld **Statische IP-Adresse** optional. Wenn keine Angabe erfolgt, wird eine Adresse aus dem ausgewählten Subnetz ausgewählt.

>[AZURE.IMPORTANT] Beim Bereitstellen eines Azure Redis Cache in einem ARM-VNet muss sich der Cache in einem dedizierten Subnetz befinden, das keine anderen Ressourcen außer Azure Redis Cache-Instanzen enthält. Wird versucht, einen Azure Redis Cache in einem virtuellen ARM-Subnetz bereitzustellen, das andere Ressourcen enthält, tritt bei der Bereitstellung ein Fehler auf.

![Virtuelles Netzwerk][redis-cache-vnet-ip]

>[AZURE.IMPORTANT] Die ersten vier Adressen in einem Subnetz sind reserviert und können nicht verwendet werden. Weitere Informationen finden Sie unter [Unterliegen die in den Subnetzen verwendeten IP-Adressen bestimmten Beschränkungen?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)

Nachdem der Cache erstellt wurde, können Sie die Konfiguration für das virtuelle Netzwerk anzeigen, indem Sie auf dem Blatt **Einstellungen** auf **Virtuelles Netzwerk** klicken.

![Virtuelles Netzwerk][redis-cache-vnet-info]


Um bei Verwenden eines VNet eine Verbindung mit Ihrer Azure Redis Cache-Instanz herzustellen, geben Sie in der Verbindungszeichenfolge den Hostnamen Ihres Caches an (siehe das folgende Beispiel).

	private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
	{
	    return ConnectionMultiplexer.Connect("contoso5premium.redis.cache.windows.net,abortConnect=false,ssl=true,password=password");
	});
	
	public static ConnectionMultiplexer Connection
	{
	    get
	    {
	        return lazyConnection.Value;
	    }
	}

## Azure Redis Cache – häufig gestellte Fragen zum VNet

Die folgende Liste enthält Antworten auf häufig gestellte Fragen zur Skalierung von Azure Redis Cache-Instanzen.

-	[Welche Probleme treten häufig bei einer fehlerhaften Konfiguration von Azure Redis Cache und VNets auf?](#what-are-some-common-misconfiguration-issues-with-azure-redis-cache-and-vnets)
-	[Kann ich VNets mit einem Standard-Cache oder Basic-Cache verwenden?](#can-i-use-vnets-with-a-standard-or-basic-cache)
-	[Warum misslingt das Erstellen eines Redis-Caches in einigen Subnetzen, aber in anderen nicht?](#why-does-creating-a-redis-cache-fail-in-some-subnets-but-not-others)
-	[Funktionieren alle Cachefeatures beim Hosten eines Cache in einem VNET?](#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)


## Welche Probleme treten häufig bei einer fehlerhaften Konfiguration von Azure Redis Cache und VNets auf?

Beim Hosten von Azure Redis Cache in einem VNet werden die in der folgenden Tabelle angegebenen Ports verwendet. Wenn diese Ports gesperrt sind, funktioniert der Cache möglicherweise nicht ordnungsgemäß. Eine bestehende Sperre für mindestens einen dieser Ports ist die häufigste Fehlkonfiguration, die beim Verwenden von Azure Redis Cache in einem VNet auftritt.

| Port(s) | Richtung | Transportprotokoll | Zweck | Remote-IP |
|-------------|------------------|--------------------|-----------------------------------------------------------------------------------|-------------------------------------|
| 80, 443 | Ausgehend | TCP | Redis-Abhängigkeiten von Azure Storage/PKI (Internet) | * |
| 53 | Ausgehend | TCP/UDP | Redis-Abhängigkeiten von DNS (Internet/VNet) | * |
| 6379, 6380 | Eingehend | TCP | Clientkommunikation mit Redis, Azure-Lastenausgleich | VIRTUAL\_NETWORK, AZURE\_LOADBALANCER |
| 8443 | Eingehend/ausgehend | TCP | Implementierungsdetail für Redis | VIRTUAL\_NETWORK |
| 8500 | Eingehend | TCP/UDP | Azure-Lastenausgleich | AZURE\_LOADBALANCER |
| 10221-10231 | Eingehend/ausgehend | TCP | Implementierungsdetail für Redis (kann den Remoteendpunkt auf VIRTUAL\_NETWORK beschränken) | VIRTUAL\_NETWORK, AZURE\_LOADBALANCER |
| 13000-13999 | Eingehend | TCP | Clientkommunikation mit Redis-Clustern, Azure-Lastenausgleich | VIRTUAL\_NETWORK, AZURE\_LOADBALANCER |
| 15000-15999 | Eingehend | TCP | Clientkommunikation mit Redis-Clustern, Azure-Lastenausgleich | VIRTUAL\_NETWORK, AZURE\_LOADBALANCER |
| 16001 | Eingehend | TCP/UDP | Azure-Lastenausgleich | AZURE\_LOADBALANCER |
| 20226 | Eingehend und ausgehend | TCP | Implementierungsdetail für Redis-Cluster | VIRTUAL\_NETWORK |


Es gibt Netzwerkverbindungsanforderungen für Azure Redis Cache, die ursprünglich nicht von einem virtuellen Netzwerk erfüllt werden konnten. Azure Redis Cache erfordert bei Verwendung in einem virtuellen Netzwerk, dass alle folgenden Voraussetzungen erfüllt sind.

-  Ausgehende Netzwerkverbindungen mit Azure-Speicherendpunkten in der ganzen Welt. Dies umfasst sowohl Endpunkte, die sich in derselben Region wie die Azure Redis Cache-Instanz befinden, als auch Speicherendpunkte in **anderen** Azure-Regionen. Azure Storage-Endpunkte werden unter den folgenden DNS-Domänen aufgelöst: *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net* und *file.core.windows.net*.
-  Ausgehende Netzwerkverbindungen mit *ocsp.msocsp.com*, *mscrl.microsoft.com* und *crl.microsoft.com*. Diese Verbindungen sind zur Unterstützung von SSL-Funktionen erforderlich.
-  Die DNS-Konfiguration für das virtuelle Netzwerk muss alle der zuvor genannten Endpunkte und Domänen auflösen können. Diese DNS-Anforderungen können erfüllt werden, indem Sie sicherstellen, dass eine gültige DNS-Infrastruktur für das virtuelle Netzwerk konfiguriert und beibehalten wird.



### Kann ich VNets mit einem Standard-Cache oder Basic-Cache verwenden?

VNets können nur mit Premium-Caches verwendet werden.

### Warum misslingt das Erstellen eines Redis-Caches in einigen Subnetzen, aber in anderen nicht?

Wenn Sie einen Azure Redis Cache in einem ARM-VNet bereitstellen, muss sich der Cache in einem dedizierten Subnetz befinden, das keine anderen Ressourcentypen enthält. Wird versucht, einen Azure Redis Cache in einem virtuellen ARM-Subnetz bereitzustellen, das andere Ressourcen enthält, tritt bei der Bereitstellung ein Fehler auf. Bevor Sie einen neuen Redis-Cache erstellen können, müssen Sie die im Subnetz vorhandenen Ressourcen löschen.

Sie können mehrere Typen von Ressourcen in einem klassischen VNet bereitstellen, solange Sie über genügend IP-Adressen verfügen.

### Funktionieren alle Cachefeatures beim Hosten eines Cache in einem VNET?

Wenn der Cache zu einem virtuellen Netzwerk gehört, haben nur Clients in diesem virtuellen Netzwerk Zugriff auf den Cache. Daher stehen zurzeit in diesem Fall folgende Cache-Verwaltungsfunktionen nicht zur Verfügung.

-	Redis-Konsole: Da die Redis-Konsole den Client „redis-cli.exe“ verwendet, der auf virtuellen Computern gehostet wird, die nicht Teil des VNET sind, kann sie keine Verbindung mit dem Cache herstellen.


## Verwenden von ExpressRoute mit Azure Redis Cache

Kunden können eine [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/)-Verbindung mit ihrer virtuellen Netzwerkinfrastruktur verbinden und so ihr lokales Netzwerks auf Azure ausdehnen.

In der Standardeinstellung kündigt eine neu erstellte ExpressRoute-Verbindung eine Standardroute an, die ausgehende Internetverbindungen zulässt. Bei dieser Konfiguration können Clientanwendungen Verbindungen mit anderen Azure-Endpunkten herstellen, einschließlich Azure Redis Cache.

Eine gängige Kundenkonfiguration sieht hingegen das Definieren einer eigenen Standardroute (0.0.0.0/0) vor, die ausgehenden Internetdatenverkehr stattdessen zwingt, die lokale Infrastruktur zu durchlaufen. Bei diesem Datenverkehr funktioniert die Verbindung mit Azure Redis Cache nicht mehr, da ausgehender Datenverkehr entweder lokal blockiert oder mittels NAT in eine nicht mehr nachvollziehbare Gruppe von Adressen übersetzt wird, die nicht mehr mit verschiedenen Azure-Endpunkten funktionieren.

Die Lösung besteht darin, mindestens eine benutzerdefinierte Route (User-Defined Routes, UDRs) in dem Subnetz zu definieren, das den Azure Redis Cache enthält. Eine benutzerdefinierte Route definiert subnetzspezifische Routen, die anstelle der Standardroute berücksichtigt werden.

Nach Möglichkeit sollte die folgende Konfiguration verwendet werden:

- Die ExpressRoute-Konfiguration kündigt 0.0.0.0/0 an und tunnelt standardmäßig allen ausgehenden Datenverkehr zwangsweise lokal.
- Die auf das Subnetz mit dem Azure Redis Cache angewendete benutzerdefinierte Route (UDR) definiert 0.0.0.0/0 mit dem nächsten Hoptyp „Internet“ (ein Beispiel dazu finden Sie weiter unten in diesem Artikel).

Gemeinsam führen diese Schritte dazu, dass die benutzerdefinierte Route auf Subnetzebene Vorrang vor erzwungenen ExpressRoute-Tunneln hat. Dadurch wird ausgehender Internetzugriff aus dem Azure Redis Cache sichergestellt.

Wenngleich das Herstellen einer Verbindung mit einem Azure Redis Cache aus einer lokalen Anwendung über ExpressRoute aus Leistungsgründen kein typisches Nutzungsszenario ist (die beste Leistung erzielen Azure Redis Cache-Clients, wenn sie sich in derselben Region wie der Azure Redis Cache befinden), kann bei diesem Szenario der ausgehende Netzwerkpfad weder durch die internen Unternehmensproxys geleitet werden, noch kann er gezwungen werden, durch einen Tunnel zur lokalen Umgebung geleitet zu werden. Andernfalls wird die tatsächliche NAT-Adresse des ausgehenden Netzwerkdatenverkehrs des Azure Redis Caches geändert. Das Ändern der NAT-Adresse des ausgehenden Netzwerkdatenverkehrs einer Azure Redis Cache-Instanz verursacht bei vielen der oben genannten Endpunkte Verbindungsfehler. Dies führt dazu, dass Versuche der Erstellung von Azure Redis Caches misslingen.

**WICHTIG:** Die in einer UDR definierten Routen **müssen** ausreichend spezifisch sein, damit sie Vorrang vor allen von der ExpressRoute-Konfiguration angekündigten Routen erhalten. Im folgenden Beispiel wird der allgemeine Adressbereich 0.0.0.0/0 verwendet, der durch Routenankündigungen mit spezifischeren Adressbereichen versehentlich überschrieben werden kann.

**SEHR WICHTIG:** Azure Redis Cache wird nicht mit ExpressRoute-Konfigurationen unterstützt, die **fälschlicherweise Routen „über Kreuz“ vom öffentlichen Peeringpfad zum privaten Peeringpfad ankündigen**. ExpressRoute-Konfigurationen, für die öffentliches Peering konfiguriert ist, erhalten Routenankündigungen von Microsoft für zahlreiche Microsoft Azure-IP-Adressbereiche. Werden diese Adressbereiche fälschlicherweise „über Kreuz“ im privaten Peeringpfad angekündigt, führt dies dazu, dass alle ausgehenden Netzwerkpakete aus dem Subnetz des Azure Redis Caches fälschlicherweise zwangsweise zur lokalen Netzwerkinfrastruktur eines Kunden getunnelt werden. Dieser Netzwerkdatenfluss unterbricht Azure Redis Cache. Die Lösung für dieses Problem besteht darin, „Über-Kreuz-Ankündigungen“ von Routen vom öffentlichen Peeringpfad zum privaten Peeringpfad zu verhindern.

Hintergrundinformationen zu benutzerdefinierten Routen finden Sie in dieser [Übersicht](../virtual-network/virtual-networks-udr-overview.md).

Weitere Informationen über ExpressRoute finden Sie unter [ExpressRoute – technische Übersicht](../expressroute/expressroute-introduction.md)

## Nächste Schritte
Informationen zur Verwendung weiterer Funktionen des Premium-Caches finden Sie in den folgenden Artikeln.

-	[Einführung in den Premium-Tarif von Azure Redis Cache](cache-premium-tier-intro.md)





  
<!-- IMAGES -->

[redis-cache-vnet]: ./media/cache-how-to-premium-vnet/redis-cache-vnet.png

[redis-cache-vnet-ip]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-ip.png

[redis-cache-vnet-info]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-info.png

<!---HONumber=AcomDC_0921_2016-->