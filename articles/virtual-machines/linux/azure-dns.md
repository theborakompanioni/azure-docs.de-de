---
title: "DNS-Namensauflösungsoptionen für Linux-VMs in Azure"
description: "Namensauflösungsszenarios für Linux-VMs in Azure IaaS, einschließlich bereitgestellter DNS-Dienste, hybridem externen DNS und Bring Your Own DNS-Server (Verwenden eines eigenen DNS-Servers)."
services: virtual-machines
documentationcenter: na
author: RicksterCDN
manager: timlt
editor: tysonn
ms.assetid: 787a1e04-cebf-4122-a1b4-1fcf0a2bbf5f
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/19/2016
ms.author: rclaus
ms.translationtype: Human Translation
ms.sourcegitcommit: 97fa1d1d4dd81b055d5d3a10b6d812eaa9b86214
ms.openlocfilehash: b95a8c3067b9bfd0a9995b26608c117a29ed8b4c
ms.contentlocale: de-de
ms.lasthandoff: 05/11/2017


---
# <a name="dns-name-resolution-options-for-linux-vms-in-azure"></a>DNS-Namensauflösungsoptionen für Linux-VMs in Azure
Azure stellt die DNS-Namensauflösung standardmäßig für alle in einem einzelnen virtuellen Netzwerk enthaltenen virtuellen Computer bereit. Sie können Ihre eigene Lösung für die DNS-Namensauflösung implementieren, indem Sie auf Ihren Azure-VMs Ihre eigenen DNS-Dienste konfigurieren. Die folgenden Szenarios sollten Ihnen dabei helfen, situationsabhängig die bessere Lösung zu wählen.

* [Von Azure bereitgestellte Namensauflösung](#azure-provided-name-resolution)
* [Namensauflösung mithilfe eines eigenen DNS-Servers](#name-resolution-using-your-own-dns-server) 

Welche Art der Namensauflösung Sie verwenden, hängt davon ab, wie die virtuellen Computer und Rolleninstanzen miteinander kommunizieren müssen.

**In der folgende Tabelle werden die Szenarien und entsprechenden Lösungen für die Namensauflösung dargestellt:**

| **Szenario** | **Lösung** | **Suffix** |
| --- | --- | --- |
| Namensauflösung zwischen Rolleninstanzen oder VMs im gleichen virtuellen Netzwerk |[Von Azure bereitgestellte Namensauflösung](#azure-provided-name-resolution) |Hostname oder FQDN |
| Namensauflösung zwischen Rolleninstanzen und virtuellen Computern in unterschiedlichen virtuellen Netzwerken |Vom Kunden verwaltete DNS-Server leiten Abfragen zwischen virtuellen Netzwerken zur Auflösung durch Azure (DNS-Proxy) weiter.  Siehe [Namensauflösung mit einem eigenen DNS-Server](#name-resolution-using-your-own-dns-server) |Nur FQDN |
| Auflösung lokaler Computer- und Dienstnamen von Rolleninstanzen oder virtuellen Computern in Azure |Vom Kunden verwaltete DNS-Server (z.B. lokale Domänencontroller, lokale schreibgeschützte Domänencontroller oder ein sekundärer DNS-Server, der mithilfe von Zonenübertragungen synchronisiert wird).  See [Namensauflösung mit einem eigenen DNS-Server](#name-resolution-using-your-own-dns-server) |Nur FQDN |
| Auflösung von Azure-Hostnamen von lokalen Computern |Weiterleiten von Abfragen an einen vom Kunden verwalteten DNS-Proxyserver im zugehörigen virtuellen Netzwerk. Der Proxyserver leitet Abfragen zur Auflösung an Azure weiter. See [Namensauflösung mit einem eigenen DNS-Server](#name-resolution-using-your-own-dns-server) |Nur FQDN |
| Reverse-DNS für interne IPs |[Namensauflösung mithilfe eines eigenen DNS-Servers](#name-resolution-using-your-own-dns-server) |– |

## <a name="azure-provided-name-resolution"></a>Von Azure bereitgestellte Namensauflösung
Zusammen mit der Auflösung des öffentlichen DNS-Namens bietet Azure die Auflösung interner Namen für VMs und Rolleninstanzen, die sich innerhalb des gleichen virtuellen Netzwerks befinden.  In ARM-basierten virtuellen Netzwerken ist das DNS-Suffix im ganzen virtuellen Netzwerk gleich (FQDN nicht erforderlich), und DNS-Namen können entweder Netzwerkkarten oder virtuellen Computern zugewiesen werden. Obwohl für die von Azure durchgeführte Namensauflösung keine Konfiguration erforderlich ist, ist sie nicht für alle Bereitstellungsszenarien die beste Lösung (siehe die vorherige Tabelle).

### <a name="features-and-considerations"></a>Funktionen und Überlegungen
**Funktionen:**

* Benutzerfreundlichkeit: Es ist keine Konfiguration erforderlich, um die von Azure bereitgestellte Namensauflösung zu verwenden.
* Der von Azure bereitgestellte Dienst zur Namensauflösung ist hoch verfügbar und erspart Ihnen das Erstellen und Verwalten von Clustern Ihrer eigenen DNS-Server.
* Kann in Verbindung mit Ihren eigenen DNS-Servern zum Auflösen von lokalen und Azure-Hostnamen verwendet werden.
* Die Namensauflösung wird zwischen VMs in virtuellen Netzwerken bereitgestellt, ohne dass der FQDN erforderlich ist. 
* Sie können Hostnamen verwenden, die Ihre Bereitstellungen am besten beschreiben, und müssen nicht mit automatisch generierten Namen arbeiten.

**Überlegungen:**

* Das von Azure erstellte DNS-Suffix kann nicht geändert werden.
* Sie können keine eigenen Einträge manuell registrieren.
* WINS und NetBIOS werden nicht unterstützt.
* Hostnamen müssen DNS-kompatibel sein. (Es dürfen nur 0-9, a-Z und "-" verwendet werden, und sie dürfen nicht mit "-" beginnen. Siehe RFC 3696 Abschnitt 2).
* Der DNS-Abfragedatenverkehr wird für den jeweiligen virtuellen Computer gedrosselt. Dies sollte auf die meisten Anwendungen keine Auswirkungen haben.  Wenn eine Drosselung der Anforderungen festgestellt wird, stellen Sie sicher, dass clientseitiges Zwischenspeichern aktiviert ist.  Weitere Informationen finden Sie unter [Die optimale Nutzung der von Azure bereitgestellten Namensauflösung](#Getting-the-most-from-Azure-provided-name-resolution).

### <a name="getting-the-most-from-azure-provided-name-resolution"></a>Getting the most from Azure-provided name resolution
**Clientseitiges Caching:**

Nicht alle DNS-Abfragen werden über das Netzwerk gesendet.  Clientseitiges Zwischenspeichern kann die Latenz verringern und die Flexibilität bei Netzwerkproblemen verbessern, indem sich wiederholende DNS-Abfragen aus einem lokalen Cache aufgelöst werden.  DNS-Einträge enthalten ein Time-To-Live (TTL), damit der Cache den Datensatz so lange wie möglich speichern kann, ohne die Aktualität der Datensätze zu beeinträchtigen.  Aus diesem Grund ist das clientseitige Zwischenspeichern in den meisten Situationen geeignet.

Einige Linux-Distributionen bieten standardmäßig kein Zwischenspeichern.  Deshalb wird empfohlen, diese Funktion jedem virtuellen Linux-Computer hinzuzufügen (nachdem Sie sich vergewissert haben, dass noch kein lokaler Cache vorhanden ist).

Es gibt eine Reihe von DNS-Cachingpaketen, z.B. dnsmasq. Es folgen die Schritte zur Installation von dnsmasq auf den am häufigsten verwendeten Distributionen:

* **Ubuntu (verwendet resolvconf)**:
  * Installieren Sie einfach das dnsmasq-Paket ("sudo apt-get install dnsmasq").
* **SUSE (verwendet netconf)**:
  * Installieren Sie das dnsmasq-Paket ("sudo zypper install dnsmasq"). 
  * Aktivieren Sie den dnsmasq-Dienst ("systemctl enable dnsmasq.service"). 
  * Starten Sie den dnsmasq-Dienst ("systemctl start dnsmasq.service"). 
  * Bearbeiten Sie "/etc/sysconfig/network/config", und ändern Sie NETCONFIG_DNS_FORWARDER="" auf "dnsmasq".
  * Aktualisieren Sie resolv.conf ("netconfig update"), um den Cache als lokalen DNS-Auflöser festzulegen.
* **CentOS von Rogue Wave Software (früher OpenLogic, verwendet NetworkManager)**:
  * Installieren Sie das dnsmasq-Paket ("sudo yum install dnsmasq").
  * Aktivieren Sie den dnsmasq-Dienst ("systemctl enable dnsmasq.service").
  * Starten Sie den dnsmasq-Dienst ("systemctl start dnsmasq.service").
  * Fügen Sie "prepend domain-name-servers 127.0.0.1;" zu "/etc/dhclient-eth0.conf" hinzu.
  * Starten Sie den Netzwerkdienst neu ("service network restart"), um den Cache als lokalen DNS-Auflöser festzulegen.

> [!NOTE]
> : Das dnsmasq-Paket ist nur einer der vielen DNS-Caches, die für Linux verfügbar sind.  Bevor Sie es nutzen, überprüfen Sie dessen Eignung für Ihre besonderen Bedürfnisse und außerdem, ob kein anderer Cache installiert ist.
> 
> 

**Clientseitige Wiederholungsversuche:**

DNS ist in erster Linie ein UDP-Protokoll.  Da das UDP-Protokoll keine Nachrichtenübermittlung garantiert, wird die Wiederholungslogik im DNS-Protokoll selbst behandelt.  Jeder DNS-Client (Betriebssystem) kann eine unterschiedliche Wiederholungslogik je nach Vorliebe des Erstellers aufweisen:

* Windows-Betriebssysteme starten nach einer Sekunde einen Wiederholungsversuch und dann erneut nach weiteren zwei, vier und weiteren vier Sekunden. 
* Das standardmäßige Linux-Setup führt nach fünf Sekunden einen Wiederholungsversuch aus.  Sie sollten dies so ändern, dass fünfmal im Abstand von einer Sekunde ein Wiederholungsversuch gestartet wird.  

Geben Sie zum Überprüfen der aktuellen Einstellungen auf einem virtuellen Linux-Computer „cat /etc/resolv.conf“ ein, und betrachten Sie die Zeile „options“, z.B.:

    options timeout:1 attempts:5

Die Datei „resolv.conf“ wird automatisch generiert und darf nicht bearbeitet werden.  Die entsprechenden Schritte zum Hinzufügen der Zeile "Optionen" variieren je nach Distribution:

* **Ubuntu** (verwendet resolvconf):
  * Fügen Sie die Optionszeile zu "/etc/resolveconf/resolv.conf.d/head" hinzu. 
  * Führen Sie "resolvconf -u" zum Aktualisieren aus.
* **SUSE** (verwendet netconf):
  * Fügen Sie "timeout:1 attempts:5' zum Parameter NETCONFIG_DNS_RESOLVER_OPTIONS="" in "/etc/sysconfig/network/config" hinzu. 
  * Führen Sie "netconfig update" zum Aktualisieren aus.
* **CentOS von Rogue Wave Software (früher OpenLogic)** (verwendet NetworkManager):
  * Fügen Sie 'echo "options timeout:1 attempts:5"' zu "/etc/NetworkManager/dispatcher.d/11-dhclient" hinzu. 
  * Führen Sie "service network restart" zum Aktualisieren aus.

## <a name="name-resolution-using-your-own-dns-server"></a>Namensauflösung mithilfe eines eigenen DNS-Servers
Es gibt verschiedene Situationen, in denen Ihre Namensauflösungsanforderungen möglicherweise über die von Azure bereitgestellten Funktionen hinausgehen. Dies kann beispielsweise dann der Fall sein, wenn eine DNS-Auflösung zwischen virtuellen Netzwerken (VNets) erforderlich ist.  Um dieses Szenario abzudecken, bietet Azure Ihnen die Möglichkeit, eigene DNS-Server zu verwenden.  

DNS-Server innerhalb eines virtuellen Netzwerks können DNS-Abfragen an die rekursiven Resolver von Azure weiterleiten, um Hostnamen innerhalb dieses virtuellen Netzwerks aufzulösen.  Beispielsweise kann ein in Azure ausgeführter DNS-Server auf DNS-Abfragen für die eigenen DNS-Zonendateien antworten und alle anderen Abfragen an Azure weiterleiten.  Auf diese Weise sind sowohl Ihre Einträge in Ihren Zonendateien als auch Ihre von Azure bereitgestellten Hostnamen (über die Weiterleitung) für die virtuellen Computer sichtbar.  Der Zugriff auf die rekursiven Resolver von Azure wird über die virtuelle IP 168.63.129.16 bereitgestellt.

Durch die DNS-Weiterleitung wird außerdem eine DNS-Auflösung zwischen virtuellen Netzwerken ermöglicht, und die lokalen Computer können von Azure bereitgestellte Hostnamen auflösen.  Um einen Hostnamen eines virtuellen Computers aufzulösen, muss sich die DNS-Server-VM im selben virtuellen Netzwerk befinden und zur Weiterleitung von Abfragen für Hostnamen an Azure konfiguriert sein.  Da jedes virtuelle Netzwerk ein eigenes DNS-Suffix verwendet, können Sie mithilfe von Regeln für die bedingte Weiterleitung DNS-Abfragen zur Auflösung an das richtige virtuelle Netzwerk senden.  Die folgende Abbildung zeigt zwei virtuelle Netzwerke und ein lokales Netzwerk, in dem eine DNS-Auflösung zwischen virtuellen Netzwerken mithilfe dieser Methode durchgeführt wird:

![DNS-Auflösung zwischen virtuellen Netzwerken](./media/azure-dns/inter-vnet-dns.png)

Bei Verwendung der von Azure bereitgestellten Namensauflösung wird jedem virtuellen Computer, der DHCP verwendet, das interne DNS-Suffix bereitgestellt.  Wenn Sie eine eigene Lösung für die Namensauflösung verwenden, wird dieses Suffix nicht bereitgestellt, weil es Konflikte mit anderen DNS-Architekturen verursacht.  Um per FQDN auf Computer zu verweisen oder das Suffix auf Ihren virtuellen Computern zu konfigurieren, kann das Suffix mithilfe von PowerShell oder der API ermittelt werden:

* Für von Azure Resource Management verwaltete VNets finden Sie das Suffix über die Ressource [Netzwerkkarte](https://msdn.microsoft.com/library/azure/mt163668.aspx), oder Sie können den Befehl `azure network public-ip show <resource group> <pip name>` ausführen, um die Details Ihrer öffentlichen IP-Adresse, einschließlich des FQDN Ihrer NIC, anzuzeigen.    

Wenn eine Abfrageweiterleitung an Azure nicht Ihren Anforderungen entspricht, müssen Sie eine eigene DNS-Lösung bereitstellen.  Die DNS-Lösung muss Folgendes leisten:

* Bereitstellung einer geeigneten Hostnamensauflösung, z.B. über [DDNS](../../virtual-network/virtual-networks-name-resolution-ddns.md).  Beachten Sie, dass Sie bei der Verwendung von DDNS möglicherweise die DNS-Eintragsbereinigung deaktivieren müssen, da die DHCP-Leases von Azure sehr lange gültig sind und die DNS-Einträge durch eine Bereinigung möglicherweise zu früh entfernt werden. 
* Bereitstellung einer geeigneten rekursiven Lösung, um eine Auflösung externer Domänennamen zu ermöglichen.
* Möglichkeit zum Zugriff (TCP und UDP auf Port 53) von den Clients, die Dienste beziehen, und Internetzugriff.
* Schutz vor einem Zugriff aus dem Internet, um mögliche Bedrohungen durch externe Agents zu minimieren.

> [!NOTE]
> Für eine optimale Leistung bei Verwendung von virtuellen Azure-Computern als DNS-Server sollte IPv6 deaktiviert und eine [öffentliche IP-Adresse auf Instanzebene](../../virtual-network/virtual-networks-instance-level-public-ip.md) jedem virtuellen DNS-Servercomputer zugewiesen werden.  
> 
> 


