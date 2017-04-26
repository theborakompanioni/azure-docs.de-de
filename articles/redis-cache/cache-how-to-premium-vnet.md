---
title: "Konfigurieren eines virtuellen Netzwerks für einen Azure Redis Cache vom Typ „Premium“ | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie die Unterstützung für virtuelle Netzwerke für Azure Redis Cache-Instanzen im Tarif &quot;Premium&quot; erstellen und verwalten."
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 8b1e43a0-a70e-41e6-8994-0ac246d8bf7f
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: sdanie
translationtype: Human Translation
ms.sourcegitcommit: c300ba45cd530e5a606786aa7b2b254c2ed32fcd
ms.openlocfilehash: cf3c1a3c669e0da810c32939492cb262e76492c7
ms.lasthandoff: 04/14/2017


---
# <a name="how-to-configure-virtual-network-support-for-a-premium-azure-redis-cache"></a>Konfigurieren der Unterstützung virtueller Netzwerke für Azure Redis Cache vom Typ "Premium"
Für Azure Redis Cache stehen verschiedene Cacheangebote bereit, die Flexibilität bei der Auswahl von Cachegröße und -features bieten, einschließlich Features des Premium-Tarifs wie die Unterstützung für Clustering, Persistenz und virtuelle Netzwerke. Ein VNet ist ein privates Netzwerk in der Cloud. Wenn eine Azure Redis Cache-Instanz mit einem VNet konfiguriert wird, ist dieses nicht öffentlich adressierbar, und auf das VNet kann nur über virtuelle Computer und Anwendungen innerhalb des VNet zugegriffen werden. In diesem Artikel wird erläutert, wie die Unterstützung eines virtuellen Netzwerks für eine Azure Redis Cache-Instanz vom Typ „Premium“ konfiguriert wird.

> [!NOTE]
> Azure Redis Cache unterstützt sowohl klassische als auch Ressourcen-Manager-VNets.
> 
> 

Weitere Informationen zu anderen Premium-Cache-Features finden Sie unter [Einführung in den Premium-Tarif von Azure Redis Cache](cache-premium-tier-intro.md).

## <a name="why-vnet"></a>Gründe für ein VNet
Die Bereitstellung über [Azure Virtual Network (VNet)](https://azure.microsoft.com/services/virtual-network/) bietet ein höheres Maß an Sicherheit und Isolation sowohl für Ihren Azure Redis Cache als auch für Ihre Subnetze. Sie profitieren ebenso von Richtlinien für die Zugriffssteuerung sowie von weiteren Features zur Begrenzung des Zugriffs.

## <a name="virtual-network-support"></a>Unterstützung für virtuelle Netzwerke
Die Unterstützung für ein virtuelles Netzwerk (VNet) wird während der Erstellung des Caches auf dem Blatt **Neuer Redis Cache** konfiguriert. 

[!INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

Nachdem Sie einen Premium-Tarif ausgewählt haben, können Sie die Redis-VNet-Integration konfigurieren, indem Sie ein virtuelles Netzwerk auswählen, das sich im selben Abonnement und am gleichen Speicherort wie Ihr Cache befindet. Um ein neues virtuelles Netzwerk verwenden zu können, müssen Sie es zuerst erstellen. Befolgen Sie dazu die Anleitungen unter [Erstellen eines virtuellen Netzwerks über das Azure-Portal](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) und [Erstellen eines virtuellen Netzwerks (klassisch) über das Azure-Portal](../virtual-network/virtual-networks-create-vnet-classic-portal.md). Kehren Sie anschließend zum Blatt **Neuer Redis Cache** zurück, um Ihrem Premium-Cache zu erstellen und zu konfigurieren.

Klicken Sie zum Konfigurieren des virtuellen Netzwerks für Ihren neuen Cache auf dem Blatt **Neuer Redis Cache** auf **Virtuelles Netzwerk**, und wählen Sie das gewünschte virtuelle Netzwerk aus der Dropdownliste aus.

![Virtuelles Netzwerk][redis-cache-vnet]

Wählen Sie in der Dropdownliste **Subnetz** das gewünschte Subnetz aus, und geben Sie die gewünschte **statische IP-Adresse** ein. Wenn Sie ein klassisches virtuelles Netzwerk verwenden, ist das Feld **Statische IP-Adresse** optional. Wenn keine Angabe erfolgt, wird eine Adresse aus dem ausgewählten Subnetz ausgewählt.

> [!IMPORTANT]
> Beim Bereitstellen eines Azure Redis Cache in einem Ressourcen-Manager-VNet muss sich der Cache in einem dedizierten Subnetz befinden, das keine anderen Ressourcen außer Azure Redis Cache-Instanzen enthält. Wird versucht, einen Azure Redis Cache in einem virtuellen Ressourcen-Manager-VNet in einem Subnetz bereitzustellen, das andere Ressourcen enthält, tritt bei der Bereitstellung ein Fehler auf.
> 
> 

![Virtuelles Netzwerk][redis-cache-vnet-ip]

> [!IMPORTANT]
> Einige IP-Adressen innerhalb jedes Subnetzes sind in Azure reserviert und können deshalb nicht genutzt werden. Die erste und letzte IP-Adresse der Subnetze sind aus Gründen der Protokollkonformität reserviert. Darüber hinaus sind drei weitere für Azure-Dienste verwendete IP-Adressen reserviert. Weitere Informationen finden Sie unter [Unterliegen die in den Subnetzen verwendeten IP-Adressen bestimmten Beschränkungen?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)
> 
> Zusätzlich zu den IP-Adressen, die von der Azure-VNet-Infrastruktur verwendet werden, nutzt jede Redis-Instanz im Subnetz zwei IP-Adressen pro Shard und eine zusätzliche IP-Adresse für den Lastenausgleich. Ein nicht gruppierter Cache hat definitionsgemäß einen Shard.
> 
> 

Nachdem der Cache erstellt wurde, können Sie die Konfiguration für das virtuelle Netzwerk anzeigen, indem Sie im **Ressourcenmenü** auf **Virtuelles Netzwerk** klicken.

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

## <a name="azure-redis-cache-vnet-faq"></a>Azure Redis Cache – häufig gestellte Fragen zum VNet
Die folgende Liste enthält Antworten auf häufig gestellte Fragen zur Skalierung von Azure Redis Cache-Instanzen.

* [Welche Probleme treten häufig bei einer fehlerhaften Konfiguration von Azure Redis Cache und VNets auf?](#what-are-some-common-misconfiguration-issues-with-azure-redis-cache-and-vnets)
* [Kann ich VNets mit einem Standard-Cache oder Basic-Cache verwenden?](#can-i-use-vnets-with-a-standard-or-basic-cache)
* [Warum misslingt das Erstellen eines Redis-Caches in einigen Subnetzen, aber in anderen nicht?](#why-does-creating-a-redis-cache-fail-in-some-subnets-but-not-others)
* [Welche Anforderungen gelten für den Subnetzadressraum?](#what-are-the-subnet-address-space-requirements)
* [Funktionieren alle Cachefeatures beim Hosten eines Cache in einem VNET?](#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)

## <a name="what-are-some-common-misconfiguration-issues-with-azure-redis-cache-and-vnets"></a>Welche Probleme treten häufig bei einer fehlerhaften Konfiguration von Azure Redis Cache und VNets auf?
Beim Hosten von Azure Redis Cache in einem VNet werden die in der folgenden Tabelle angegebenen Ports verwendet. Wenn diese Ports gesperrt sind, funktioniert der Cache möglicherweise nicht ordnungsgemäß. Eine bestehende Sperre für mindestens einen dieser Ports ist die häufigste Fehlkonfiguration, die beim Verwenden von Azure Redis Cache in einem VNet auftritt.

| Port(s) | Richtung | Transportprotokoll | Zweck | Remote-IP |
| --- | --- | --- | --- | --- |
| 80, 443 |Ausgehend |TCP |Redis-Abhängigkeiten von Azure Storage/PKI (Internet) |* |
| 53 |Ausgehend |TCP/UDP |Redis-Abhängigkeiten von DNS (Internet/VNet) |* |
| 6379, 6380 |Eingehend |TCP |Clientkommunikation mit Redis, Azure-Lastenausgleich |VIRTUAL_NETWORK, AZURE_LOADBALANCER |
| 8443 |Eingehend/ausgehend |TCP |Implementierungsdetail für Redis |VIRTUAL_NETWORK |
| 8500 |Eingehend |TCP/UDP |Azure-Lastenausgleich |AZURE_LOADBALANCER |
| 10221-10231 |Eingehend/ausgehend |TCP |Implementierungsdetail für Redis (kann den Remoteendpunkt auf VIRTUAL_NETWORK beschränken) |VIRTUAL_NETWORK, AZURE_LOADBALANCER |
| 13000-13999 |Eingehend |TCP |Clientkommunikation mit Redis-Clustern, Azure-Lastenausgleich |VIRTUAL_NETWORK, AZURE_LOADBALANCER |
| 15000-15999 |Eingehend |TCP |Clientkommunikation mit Redis-Clustern, Azure-Lastenausgleich |VIRTUAL_NETWORK, AZURE_LOADBALANCER |
| 16001 |Eingehend |TCP/UDP |Azure-Lastenausgleich |AZURE_LOADBALANCER |
| 20226 |Eingehend und ausgehend |TCP |Implementierungsdetail für Redis-Cluster |VIRTUAL_NETWORK |

Es gibt Netzwerkverbindungsanforderungen für Azure Redis Cache, die ursprünglich nicht von einem virtuellen Netzwerk erfüllt werden konnten. Azure Redis Cache erfordert bei Verwendung in einem virtuellen Netzwerk, dass alle folgenden Voraussetzungen erfüllt sind.

* Ausgehende Netzwerkverbindungen mit Azure-Speicherendpunkten in der ganzen Welt. Dies umfasst sowohl Endpunkte, die sich in derselben Region wie die Azure Redis Cache-Instanz befinden, als auch Speicherendpunkte in **anderen** Azure-Regionen. Azure Storage-Endpunkte werden unter den folgenden DNS-Domänen aufgelöst: *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net* und *file.core.windows.net*. 
* Ausgehende Netzwerkverbindungen mit *ocsp.msocsp.com*, *mscrl.microsoft.com* und *crl.microsoft.com*. Diese Verbindungen sind zur Unterstützung von SSL-Funktionen erforderlich.
* Die DNS-Konfiguration für das virtuelle Netzwerk muss alle der zuvor genannten Endpunkte und Domänen auflösen können. Diese DNS-Anforderungen können erfüllt werden, indem Sie sicherstellen, dass eine gültige DNS-Infrastruktur für das virtuelle Netzwerk konfiguriert und beibehalten wird.
* Ausgehende Netzwerkkonnektivität zu den folgenden Azure Monitoring-Endpunkten, die zu den folgenden DNS-Domänen auflösen: shoebox2-black.shoebox2.metrics.nsatc.net, north-prod2.prod2.metrics.nsatc.net, azglobal-black.azglobal.metrics.nsatc.net, shoebox2-red.shoebox2.metrics.nsatc.net, east-prod2.prod2.metrics.nsatc.net, azglobal-red.azglobal.metrics.nsatc.net.

### <a name="can-i-use-vnets-with-a-standard-or-basic-cache"></a>Kann ich VNets mit einem Standard-Cache oder Basic-Cache verwenden?
VNets können nur mit Premium-Caches verwendet werden.

### <a name="why-does-creating-a-redis-cache-fail-in-some-subnets-but-not-others"></a>Warum misslingt das Erstellen eines Redis-Caches in einigen Subnetzen, aber in anderen nicht?
Wenn Sie einen Azure Redis Cache in einem Ressourcen-Manager-VNet bereitstellen, muss sich der Cache in einem dedizierten Subnetz befinden, das keine anderen Ressourcentypen enthält. Wird versucht, einen Azure Redis Cache in einem virtuellen Ressourcen-Manager-VNet in einem Subnetz bereitzustellen, das andere Ressourcen enthält, tritt bei der Bereitstellung ein Fehler auf. Bevor Sie einen neuen Redis-Cache erstellen können, müssen Sie die im Subnetz vorhandenen Ressourcen löschen.

Sie können mehrere Typen von Ressourcen in einem klassischen VNet bereitstellen, solange Sie über genügend IP-Adressen verfügen.

### <a name="what-are-the-subnet-address-space-requirements"></a>Welche Anforderungen gelten für den Subnetzadressraum?
Einige IP-Adressen innerhalb jedes Subnetzes sind in Azure reserviert und können deshalb nicht genutzt werden. Die erste und letzte IP-Adresse der Subnetze sind aus Gründen der Protokollkonformität reserviert. Darüber hinaus sind drei weitere für Azure-Dienste verwendete IP-Adressen reserviert. Weitere Informationen finden Sie unter [Unterliegen die in den Subnetzen verwendeten IP-Adressen bestimmten Beschränkungen?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)

Zusätzlich zu den IP-Adressen, die von der Azure-VNet-Infrastruktur verwendet werden, nutzt jede Redis-Instanz im Subnetz zwei IP-Adressen pro Shard und eine zusätzliche IP-Adresse für den Lastenausgleich. Ein nicht gruppierter Cache hat definitionsgemäß einen Shard.

### <a name="do-all-cache-features-work-when-hosting-a-cache-in-a-vnet"></a>Funktionieren alle Cachefeatures beim Hosten eines Cache in einem VNET?
Wenn der Cache zu einem virtuellen Netzwerk gehört, haben nur Clients in diesem virtuellen Netzwerk Zugriff auf den Cache. Daher stehen zurzeit in diesem Fall folgende Cache-Verwaltungsfunktionen nicht zur Verfügung.

* Redis-Konsole: Da die Redis-Konsole in Ihrem lokalen Browser ausgeführt wird, der sich außerhalb des VNet befindet, kann sie sich nicht mit Ihrem Cache verbinden.

## <a name="use-expressroute-with-azure-redis-cache"></a>Verwenden von ExpressRoute mit Azure Redis Cache
Kunden können eine [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) -Verbindung mit ihrer virtuellen Netzwerkinfrastruktur verbinden und so ihr lokales Netzwerks auf Azure ausdehnen. 

In der Standardeinstellung kündigt eine neu erstellte ExpressRoute-Verbindung eine Standardroute an, die ausgehende Internetverbindungen zulässt. Bei dieser Konfiguration können Clientanwendungen Verbindungen mit anderen Azure-Endpunkten herstellen, einschließlich Azure Redis Cache.

Eine gängige Kundenkonfiguration sieht hingegen das Definieren einer eigenen Standardroute (0.0.0.0/0) vor, die ausgehenden Internetdatenverkehr stattdessen zwingt, die lokale Infrastruktur zu durchlaufen. Dieser Datenverkehrsfluss unterbricht die Verbindung mit Azure Redis Cache, wenn der ausgehende Datenverkehr anschließend lokal so blockiert wird, dass die Azure Redis Cache-Instanz nicht mit ihren Abhängigkeiten kommunizieren kann.

Die Lösung besteht darin, mindestens eine benutzerdefinierte Route (User-Defined Routes, UDRs) in dem Subnetz zu definieren, das den Azure Redis Cache enthält. Eine benutzerdefinierte Route definiert subnetzspezifische Routen, die anstelle der Standardroute berücksichtigt werden.

Nach Möglichkeit sollte die folgende Konfiguration verwendet werden:

* Die ExpressRoute-Konfiguration kündigt 0.0.0.0/0 an und tunnelt standardmäßig allen ausgehenden Datenverkehr zwangsweise lokal.
* Die für das Subnetz mit dem Azure Redis Cache geltende benutzerdefinierte Route definiert 0.0.0.0/0 mit einer funktionierenden Route für TCP/IP-Datenverkehr zum öffentlichen Internet, beispielsweise durch Festlegen des nächsten Hoptyps auf „Internet“.

Gemeinsam führen diese Schritte dazu, dass die benutzerdefinierte Route auf Subnetzebene Vorrang vor erzwungenen ExpressRoute-Tunneln hat. Dadurch wird ausgehender Internetzugriff aus dem Azure Redis Cache sichergestellt.

Das Herstellen einer Verbindung mit einem Azure Redis Cache aus einer lokalen Anwendung über ExpressRoute ist aus Leistungsgründen kein typisches Nutzungsszenario (die beste Leistung erzielen Azure Redis Cache-Clients, wenn sie sich in derselben Region wie der Azure Redis Cache befinden).

>[!IMPORTANT] 
>Die in einer UDR definierten Routen **müssen** ausreichend spezifisch sein, damit sie Vorrang vor allen von der ExpressRoute-Konfiguration angekündigten Routen erhalten. Im folgenden Beispiel wird der allgemeine Adressbereich 0.0.0.0/0 verwendet, der durch Routenankündigungen mit spezifischeren Adressbereichen versehentlich überschrieben werden kann.

>[!WARNING]  
>Azure Redis Cache wird nicht mit ExpressRoute-Konfigurationen unterstützt, die **fälschlicherweise Routen „über Kreuz“ vom öffentlichen Peeringpfad zum privaten Peeringpfad ankündigen**. ExpressRoute-Konfigurationen, für die öffentliches Peering konfiguriert ist, erhalten Routenankündigungen von Microsoft für zahlreiche Microsoft Azure-IP-Adressbereiche. Werden diese Adressbereiche fälschlicherweise „über Kreuz“ im privaten Peeringpfad angekündigt, führt dies dazu, dass alle ausgehenden Netzwerkpakete aus dem Subnetz des Azure Redis Caches fälschlicherweise zwangsweise zur lokalen Netzwerkinfrastruktur eines Kunden getunnelt werden. Dieser Netzwerkdatenfluss unterbricht Azure Redis Cache. Die Lösung für dieses Problem besteht darin, „Über-Kreuz-Ankündigungen“ von Routen vom öffentlichen Peeringpfad zum privaten Peeringpfad zu verhindern.


Hintergrundinformationen zu benutzerdefinierten Routen finden Sie in dieser [Übersicht](../virtual-network/virtual-networks-udr-overview.md).

Weitere Informationen über ExpressRoute finden Sie unter [ExpressRoute – technische Übersicht](../expressroute/expressroute-introduction.md).

## <a name="next-steps"></a>Nächste Schritte
Informationen zur Verwendung weiterer Funktionen des Premium-Caches finden Sie in den folgenden Artikeln.

* [Einführung in den Premium-Tarif von Azure Redis Cache](cache-premium-tier-intro.md)

<!-- IMAGES -->

[redis-cache-vnet]: ./media/cache-how-to-premium-vnet/redis-cache-vnet.png

[redis-cache-vnet-ip]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-ip.png

[redis-cache-vnet-info]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-info.png


