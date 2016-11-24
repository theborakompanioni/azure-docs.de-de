---
title: Erste Schritte mit Microsoft Azure-Sicherheit | Microsoft-Dokumentation
description: "Dieser Artikel enthält eine Übersicht über die Sicherheitsfunktionen von Microsoft Azure sowie allgemeine Informationen für Organisationen, die ihre Ressourcen zu einem Cloudanbieter migrieren."
services: security
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: TomSh
ms.assetid: 8d8a0088-c85a-48e7-bd04-2bc7b78b0691
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/19/2016
ms.author: yuridio
translationtype: Human Translation
ms.sourcegitcommit: ee34a7ebd48879448e126c1c9c46c751e477c406
ms.openlocfilehash: 2337c80ff0128c7ca4bf287ceb460dfc11404668


---
# <a name="getting-started-with-microsoft-azure-security"></a>Erste Schritte mit Microsoft Azure-Sicherheit
Wenn Sie IT-Ressourcen erstellen oder zu einem Cloudanbieter migrieren, verlassen Sie sich darauf, dass die jeweilige Organisation Ihre Anwendungen und Daten mit den Diensten und Steuerungsmöglichkeiten schützen kann, die sie zum Verwalten der Sicherheit Ihrer cloudbasierten Ressourcen bereitstellt.

Die Infrastruktur von Azure ist von den Hardwareressourcen bis zu den Anwendungen vollständig auf das gleichzeitige Hosten von Millionen von Kunden ausgelegt und stellt für Unternehmen eine vertrauenswürdige Grundlage zur Erfüllung ihrer Sicherheitsanforderungen dar. Darüber hinaus bietet Azure Ihnen ein breites Spektrum an konfigurierbaren Sicherheitsoptionen, die Sie selbst steuern können, um die Sicherheit an die individuellen Anforderungen Ihrer Bereitstellungen anzupassen.

In diesem allgemeinen Artikel zur Sicherheit von Azure werden folgende Themen behandelt:

* Azure-Dienste und -Features, die Sie beim Schützen Ihrer Dienste und Daten innerhalb von Azure unterstützen
* Absicherung der Azure-Infrastruktur durch Microsoft zum Schutz Ihrer Daten und Anwendungen

## <a name="identity-and-access-management"></a>Identitäts- und Zugriffsverwaltung
Das Steuern des Zugriffs auf die IT-Infrastruktur, Daten und Anwendungen ist von größter Bedeutung. Microsoft Azure stellt diese Funktionen durch Dienste wie Azure Active Directory (Azure AD) und Azure Storage sowie durch die Unterstützung zahlreicher Standards und APIs bereit.

[Azure AD](../active-directory/active-directory-whatis.md) ist ein Identitätsrepository und -modul zur Authentifizierung, Autorisierung und Zugriffssteuerung für die Benutzer, Gruppen und Objekte einer Organisation. Azure AD stellt Entwicklern außerdem eine effektive Methode zur Verfügung, um die Identitätsverwaltung in ihre Anwendungen zu integrieren. Standardprotokolle wie [SAML 2.0](https://en.wikipedia.org/wiki/SAML_2.0), [WS-Federation](https://msdn.microsoft.com/library/bb498017.aspx) und [OpenID Connect](http://openid.net/connect/) ermöglichen die Anmeldung bei Plattformen wie .NET, Java, Node.js und PHP.

Die REST-basierte Diagramm-API ermöglicht Entwicklern von jeder Plattform aus das Lesen und Schreiben im Verzeichnis. Durch die Unterstützung von [OAuth 2.0](http://oauth.net/2/) können Entwickler neben mobilen und webbasierten Anwendungen mit integrierten Web-APIs von Microsoft und Drittanbietern auch eigene sichere Web-APIs erstellen. Open Source-Clientbibliotheken sind für .NET, Windows Store, iOS und Android verfügbar; weitere Bibliotheken sind bereits in Entwicklung.

### <a name="how-azure-enables-identity-and-access-management"></a>Umsetzung der Identitäts- und Zugriffsverwaltung in Azure
Sie können Azure AD als eigenständiges Cloudverzeichnis für Ihre Organisation verwenden oder in Ihr vorhandenes lokales Active Directory integrieren. Einige Integrationsfeatures umfassen die Verzeichnissynchronisierung und einmaliges Anmelden (SSO). Diese Features erweitern die Reichweite Ihrer vorhandenen lokalen Identitäten auf die Cloud und verbessern die Benutzerfreundlichkeit für Administratoren und Benutzer.

Im Folgenden sind einige weitere Möglichkeiten zur Identitäts- und Zugriffsverwaltung aufgeführt:

* Azure AD ermöglicht [SSO](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/) für SaaS-Anwendungen unabhängig davon, wo sie gehostet werden. Einige Anwendungen bilden einen Verbund mit Azure AD, andere verwenden Kennwort-SSO. Verbundanwendungen können auch die Benutzerbereitstellung und Kennworttresore unterstützen.
* Der Zugriff auf Daten in [Azure Storage](https://azure.microsoft.com/services/storage/) wird über die Authentifizierung gesteuert. Jedes Speicherkonto verfügt über einen Primärschlüssel ([Speicherkontoschlüssel](https://msdn.microsoft.com/library/azure/ee460785.aspx); Storage Account Key, SAK) und einen sekundären geheimen Schlüssel (Shared Access Signature, SAS).
* Azure AD bietet Identity-as-a-Service durch Verbund (unter Verwendung der [Active Directory-Verbunddienste](../active-directory/fundamentals-identity.md)) sowie Synchronisierung und Replikation mit lokalen Verzeichnissen.
* [Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md) ist der Dienst für die mehrstufige Authentifizierung, bei der Benutzer Anmeldevorgänge über eine mobile App, einen Telefonanruf oder per SMS bestätigen müssen. Er kann mit Azure AD verwendet werden, um lokale Ressourcen mit dem Azure Multi-Factor Authentication-Server zu schützen, eignet sich aber auch für benutzerdefinierte Anwendungen und Verzeichnisse, die das SDK verwenden.
* Mit [Azure AD Domain Services](https://azure.microsoft.com/services/active-directory-ds/) können Sie virtuelle Azure-Computer in eine Domäne einbinden, ohne Domänencontroller bereitzustellen. Sie können sich bei diesen virtuellen Computern mithilfe Ihrer Active Directory-Unternehmensanmeldeinformationen anmelden und in die Domäne eingebundene virtuelle Computer mithilfe von Gruppenrichtlinien verwalten, um Sicherheitsbaselines für alle Ihre virtuellen Azure-Computer zu erzwingen.
* [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) bietet einen hoch verfügbaren, globalen Identitätsverwaltungsdienst für kundenorientierte Anwendungen, der für Millionen von Identitäten skaliert werden kann. Er kann über mobile und Webplattformen integriert werden. Ihre Kunden können sich über eine anpassbare Oberfläche bei all Ihren Anwendungen anmelden und zu diesem Zweck entweder vorhandene Konten aus sozialen Netzwerken nutzen oder neue Anmeldeinformationen festlegen.

## <a name="data-access-control-and-encryption"></a>Zugriffssteuerung und Verschlüsselung von Daten
Microsoft wendet die Grundsätze der Aufgabentrennung und der [geringstmöglichen Berechtigungen](https://en.wikipedia.org/wiki/Principle_of_least_privilege) in allen Azure-Vorgängen an. Für den Zugriff auf Daten benötigen Azure-Supportmitarbeiter Ihre explizite Erlaubnis, die auf „Just-in-Time“-Basis gewährt, protokolliert und überwacht und nach Abschluss der Aufgabe wieder entzogen wird.

Azure stellt außerdem mehrere Funktionen zum Schutz von Daten während der Übertragung und im Ruhezustand bereit. Hierzu zählt unter anderem die Verschlüsselung von Daten, Dateien, Anwendungen, Diensten, Laufwerken und der Kommunikation. Sie können Informationen vor dem Speichern in Azure verschlüsseln und Schlüssel in Ihren lokalen Datencentern speichern.

![Microsoft-Antischadsoftware in Azure](./media/azure-security-getting-started/sec-azgsfig1.PNG)

### <a name="azure-encryption-technologies"></a>Azure-Verschlüsselungstechniken
Sie können Details zum Verwaltungszugriff auf Ihre Abonnementumgebung mit [Azure AD Reporting](../active-directory/active-directory-reporting-audit-events.md) sammeln. Sie können die [BitLocker-Laufwerkverschlüsselung](https://technet.microsoft.com/library/cc732774.aspx) für virtuelle Festplatten mit vertraulichen Informationen in Azure konfigurieren.

Weitere Möglichkeiten zum Schutz Ihrer Daten in Azure:

* Anwendungsentwickler können die Verschlüsselung mithilfe von Windows [CryptoAPI](https://msdn.microsoft.com/library/ms867086.aspx) und .NET-Framework in die Anwendungen integrieren, die sie in Azure bereitstellen.
* Mit der clientseitigen Verschlüsselung für Azure Blob Storage haben Sie die vollständige Kontrolle über die Schlüssel. Dem Speicherdienst werden die Schlüssel niemals offengelegt, sodass er die Daten nicht entschlüsseln kann.
* [Azure Rights Management (Azure RMS)](https://technet.microsoft.com/library/jj585026.aspx) (mit dem [RMS SDK](https://msdn.microsoft.com/library/dn758244.aspx)) bietet Verschlüsselung auf Datei- und Datenebene sowie Schutz vor Datenverlust mittels richtlinienbasierter Zugriffsverwaltung.
* Azure unterstützt die [Verschlüsselung auf Tabellen- und auf Spaltenebene (TDE/CLE)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/05/12/recommendations-for-using-cell-level-encryption-in-azure-sql-database.aspx) in virtuellen SQL Server-Computern sowie lokale Drittanbieterserver zur Schlüsselverwaltung in Datencentern.
* Speicherkontoschlüssel, Shared Access Signatures (SAS), Verwaltungszertifikate und andere Schlüssel sind für die einzelnen Azure-Mandanten eindeutig.
* Der Azure [StorSimple](http://www.microsoft.com/server-cloud/products/storsimple/overview.aspx)-Hybridspeicher verschlüsselt Daten vor dem Hochladen in Azure Storage mithilfe eines 128-Bit-Schlüsselpaars aus einem öffentlichen und privaten Schlüssel.
* Azure unterstützt und verwendet zahlreiche Verschlüsselungsmechanismen. Hierzu zählen beispielsweise SSL/TLS, IPsec und AES (abhängig von den Datentypen, Containern und Transporten).

## <a name="virtualization"></a>Virtualisierung
Die Azure-Plattform verwendet eine virtualisierte Umgebung. Benutzerinstanzen werden als eigenständige virtuelle Computer betrieben, die keinen Zugriff auf einen physischen Hostserver haben. Diese Isolierung wird durch physische [Prozessorberechtigungsebenen (Ring 0/Ring 3)](https://en.wikipedia.org/wiki/Protection_ring) erzwungen.

Ring 0 entspricht den höchsten Berechtigungen und Ring 3 den geringsten. Zur Ausführung des Gastbetriebssystems wird ein weniger privilegierter Ring (Ring 1) und für Anwendungen der am wenigsten privilegierte Ring (Ring 3) verwendet. Die Virtualisierung physischer Ressourcen führt zu einer klaren Abgrenzung zwischen Gastbetriebssystem und Hypervisor, was eine zusätzliche Sicherheitstrennung zwischen den beiden bewirkt.

Der Hypervisor von Azure verhält sich wie ein Microkernel und übergibt alle Hardwarezugriffsanforderungen von virtuellen Gastcomputern an den Host, damit sie über eine Schnittstelle mit gemeinsam genutztem Speicherbereich namens VMBus verarbeitet werden. Dies verhindert, dass Benutzer RAW-Lese-, -Schreib- und -Ausführungszugriff auf das System erhalten, und verringert das Risiko der Freigabe von Systemressourcen.

![Microsoft-Antischadsoftware in Azure](./media/azure-security-getting-started/sec-azgsfig2.PNG)

### <a name="how-azure-implements-virtualization"></a>Implementierung der Virtualisierung in Azure
Azure verwendet eine Hypervisorfirewall (Paketfilter), die im Hypervisor implementiert und von einem Fabric Controller-Agent konfiguriert wird. So können Mandanten vor nicht autorisiertem Zugriff geschützt werden. Standardmäßig wird beim Erstellen eines virtuellen Computers sämtlicher Datenverkehr blockiert, und der Fabric Controller-Agent konfiguriert anschließend den Paketfilter, um *Regeln und Ausnahmen* für das Zulassen des autorisierten Datenverkehrs hinzuzufügen.

Es gibt zwei Kategorien von Regeln, die hier programmiert werden:

* **Computerkonfiguration oder Infrastrukturregeln**: In der Standardeinstellung wird die gesamte Kommunikation blockiert. Es gibt Ausnahmen, um einem virtuellen Computer das Senden und Empfangen von DHCP- und DNS-Datenverkehr zu ermöglichen. Virtuelle Computer können auch Datenverkehr an das „öffentliche“ Internet und an andere virtuelle Computer im Cluster und auf dem Betriebssystem-Aktivierungsserver senden. Die Liste mit zulässigen ausgehenden Zielen der virtuellen Computer enthält weder Azure-Routersubnetze noch das Azure-Verwaltungs-Back-End und andere Microsoft-Komponenten.
* **Rollenkonfigurationsdatei**: Diese definiert die eingehenden Zugriffssteuerungslisten (ACLs) auf der Grundlage des Dienstmodells des Mandanten. Wenn ein Mandant beispielsweise über ein Web-Front-End an Port 80 auf einem bestimmten virtuellen Computer verfügt, öffnet Azure den TCP-Port 80 für alle IP-Adressen, wenn Sie einen Endpunkt im [klassischen Azure-Bereitstellungsmodell](../resource-manager-deployment-model.md) konfigurieren. Wenn auf dem virtuellen Computer ein Back-End oder eine Workerrolle ausgeführt wird, wird die Workerrolle nur für den virtuellen Computer innerhalb des gleichen Mandanten geöffnet.

## <a name="isolation"></a>Isolation
Trennung ist eine weitere wichtige Cloudsicherheitsanforderung, um die nicht autorisierte und unbeabsichtigte Übertragung von Informationen zwischen Bereitstellungen in einer freigegebenen mehrinstanzenfähigen Architektur zu verhindern.

Azure implementiert [Netzwerkzugriffssteuerung](https://azure.microsoft.com/blog/network-isolation-options-for-machines-in-windows-azure-virtual-networks/) und Trennung mittels VLAN-Isolierung, ACLs, Lastenausgleichsmodulen und IP-Filtern. Externer eingehender Datenverkehr wird auf von Ihnen definierte Ports und Protokolle Ihrer virtuellen Computer beschränkt. Von Azure wird eine Netzwerkfilterung implementiert, um gefälschten Datenverkehr zu verhindern und eingehenden wie ausgehenden Datenverkehr auf vertrauenswürdige Plattformkomponenten zu beschränken. Richtlinien zum Datenverkehrsfluss werden auf Geräten zum Schutz der Netzwerkgrenzen implementiert, die den Datenverkehr standardmäßig verweigern.

![Microsoft-Antischadsoftware in Azure](./media/azure-security-getting-started/sec-azgsfig3.PNG)

Die Netzwerkadressenübersetzung wird zum Trennen des internen Netzwerkdatenverkehrs vom externen Datenverkehr eingesetzt. Der interne Datenverkehr kann nicht extern geroutet werden. [Virtuelle IP-Adressen](http://blogs.msdn.com/b/cloud_solution_architect/archive/2014/11/08/vips-dips-and-pips-in-microsoft-azure.aspx), die extern geroutet werden können, werden in [interne dynamische IP-Adressen](http://blogs.msdn.com/b/cloud_solution_architect/archive/2014/11/08/vips-dips-and-pips-in-microsoft-azure.aspx) übersetzt, die nur innerhalb von Azure geroutet werden können.

Externer Datenverkehr an virtuelle Azure-Computer wird bei Routern, Lastenausgleichsmodulen und Schicht 3-Switches mithilfe von ACLs durch eine Firewall geleitet. Nur bestimmte bekannte Protokolle sind zulässig. ACLs begrenzen den Datenverkehr von virtuellen Gastcomputern an andere VLANs, die zur Verwaltung verwendet werden. Darüber hinaus wird Datenverkehr sowohl in der Sicherungs- als auch in der Vermittlungsschicht über IP-Filter auf dem Hostbetriebssystem weiter begrenzt.

### <a name="how-azure-implements-isolation"></a>Implementierung der Isolation in Azure
Der Azure Fabric Controller ist zuständig für die Zuweisung von Infrastrukturressourcen zu Mandantenworkloads und verwaltet die unidirektionale Kommunikation des Hosts mit den virtuellen Computern. Der Azure-Hypervisor erzwingt eine Arbeitsspeicher- und Prozesstrennung zwischen virtuellen Computern und leitet Netzwerkdatenverkehr sicher an die Gastbetriebssystem-Mandanten weiter. Azure implementiert die Isolation auch für Mandanten, Speicher und virtuelle Netzwerke.

* Jeder Azure AD-Mandant ist logisch durch Sicherheitsgrenzen isoliert.
* Azure-Speicherkonten sind abonnementspezifisch, und der Zugriff darauf muss durch einen Speicherkontoschlüssel authentifiziert werden.
* Virtuelle Netzwerke sind logisch durch eine Kombination aus eindeutigen privaten IP-Adressen, Firewalls und IP-ACLs isoliert. Lastenausgleichsmodule leiten Datenverkehr basierend auf Endpunktdefinitionen an die entsprechenden Mandanten weiter.

## <a name="virtual-networks-and-firewalls"></a>Virtuelle Netzwerke und Firewalls
Mithilfe der [verteilten und virtuellen Netzwerke](http://download.microsoft.com/download/4/3/9/43902EC9-410E-4875-8800-0788BE146A3D/Windows%20Azure%20Network%20Security%20Whitepaper%20-%20FINAL.docx) in Azure können Sie sicherstellen, dass Ihr privater Netzwerkdatenverkehr logisch vom Datenverkehr anderer virtueller Azure-Netzwerke isoliert ist.

![Microsoft-Antischadsoftware in Azure](./media/azure-security-getting-started/sec-azgsfig4.PNG)

Ihr Abonnement kann mehrere isolierte private Netzwerke (sowie Firewall, Lastenausgleich und Netzwerkadressübersetzung) enthalten.

Azure bietet in jedem Azure-Cluster drei primäre Ebenen der Netzwerktrennung, um den Datenverkehr logisch aufzuteilen. [Virtuelle lokale Netzwerke](https://azure.microsoft.com/services/virtual-network/) (VLANs) werden verwendet, um Kundendatenverkehr vom übrigen Azure-Netzwerk zu trennen. Der Zugriff auf das Azure-Netzwerk von außerhalb des Clusters wird durch Lastenausgleichsmodule eingeschränkt.

Netzwerkdatenverkehr an virtuelle Computer und von virtuellen Computern muss den virtuellen Switch des Hypervisors durchlaufen. Die IP-Filterkomponente im Stammbetriebssystem isoliert den virtuellen Stammcomputer von den virtuellen Gastcomputern sowie die einzelnen virtuellen Gastcomputer voneinander. Sie führt die Filterung des Datenverkehrs durch, um die Kommunikation zwischen den Knoten eines Mandanten und dem öffentlichen Internet (basierend auf der Dienstkonfiguration des Kunden) einzuschränken, indem sie sie von anderen Mandanten trennt.

Der IP-Filter verhindert folgende Aktionen von virtuellen Gastcomputern:

* Generieren von gefälschtem Datenverkehr
* Empfangen von Datenverkehr, der nicht für sie bestimmt ist
* Weiterleiten von Datenverkehr an geschützte Infrastrukturendpunkte
* Senden oder Empfangen von unerwünschtem Broadcastdatenverkehr

Sie können Ihre virtuellen Computer in [virtuellen Azure-Netzwerken](https://azure.microsoft.com/documentation/services/virtual-network/) platzieren. Diese virtuellen Netzwerke sind vergleichbar mit den Netzwerken, die Sie in einer lokalen Umgebung konfigurieren, in der sie normalerweise einem virtuellen Switch zugeordnet sind. Mit dem gleichen virtuellen Netzwerk verbundene virtuelle Computer können ohne zusätzliche Konfiguration miteinander kommunizieren. Innerhalb Ihres virtuellen Netzwerks können auch verschiedene Subnetze konfiguriert werden.

Die folgenden Azure Virtual Network-Technologien ermöglichen den Schutz der Kommunikation in Ihrem virtuellen Netzwerk:

* [**Netzwerksicherheitsgruppen (NSGs)**](../virtual-network/virtual-networks-nsg.md): Mit einer NSG können Sie eingehenden Datenverkehr für Instanzen virtueller Computer in Ihrem virtuellen Netzwerk steuern. Eine NSG enthält Regeln zur Zugriffssteuerung, die den Datenverkehr auf Grundlage der Richtung des Datenverkehrs, des Protokolls, der Quelladresse und des Quellports, und der Zieladresse und des Zielports zulässt oder verweigert.
* [**Benutzerdefiniertes Routing**](../virtual-network/virtual-networks-udr-overview.md): Sie können das Routing von Paketen über ein virtuelles Gerät steuern, indem Sie benutzerdefinierte Routen erstellen, die festlegen, dass der nächste Hop für Pakete, die an ein bestimmtes Subnetz gesendet werden, an ein virtuelles Netzwerksicherheitsgerät erfolgen soll.
* [**IP-Weiterleitung**](../virtual-network/virtual-networks-udr-overview.md): Ein virtuelles Netzwerksicherheitsgerät muss eingehenden Datenverkehr empfangen können, der nicht an das Gerät selbst adressiert ist. Aktivieren Sie die IP-Weiterleitung für einen virtuellen Computer, damit dieser an andere Ziele gerichteten Datenverkehr empfangen kann.
* [**Tunnelerzwingung**](../vpn-gateway/vpn-gateway-about-forced-tunneling.md): Über die Tunnelerzwingung können Sie den gesamten Internetdatenverkehr, der von Ihren virtuellen Computern in einem virtuellen Netzwerk generiert wird, zur Überprüfung und Überwachung über einen Site-to-Site-VPN-Tunnel an Ihren lokalen Standort umleiten.
* [**Endpunkt-ACLs**](../virtual-machines/virtual-machines-windows-classic-setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json): Mithilfe von Endpunkt-ACLs können Sie steuern, für welche Computer eingehende Verbindungen aus dem Internet mit einem virtuellen Computer in Ihrem virtuellen Netzwerk zulässig sind.
* [**Partnerlösungen zur Netzwerksicherheit**](https://azure.microsoft.com/marketplace/): Im Azure Marketplace steht eine Reihe von Partnerlösungen zur Netzwerksicherheit zur Verfügung.

### <a name="how-azure-implements-virtual-networks-and-firewalls"></a>Implementierung virtueller Netzwerke und Firewalls in Azure
Azure implementiert standardmäßig Firewalls zur Paketfilterung auf allen virtuellen Host- und Gastcomputern. In Images des Windows-Betriebssystems aus dem Azure Marketplace ist außerdem die Windows-Firewall standardmäßig aktiviert. Lastenausgleichsmodule im Umkreis von öffentlich zugänglichen Azure-Netzwerken steuern die Kommunikation auf der Grundlage von IP-ACLs, die von Kundenadministratoren verwaltet werden.

Wenn Azure die Daten eines Kunden im Zuge des normalen Betriebs oder bei einem Notfall verschiebt, geschieht dies über private, verschlüsselte Kommunikationskanäle. Weitere von Azure genutzte Funktionen in virtuellen Netzwerken und Firewalls:

* **Native Hostfirewall**: Azure Service Fabric und Azure Storage werden unter einem nativen Betriebssystem ohne Hypervisor ausgeführt. Daher wird die Windows-Firewall mit den beiden oben genannten Regelsätzen konfiguriert. Storage wird zum Optimieren der Leistung systemeigen ausgeführt.
* **Hostfirewall**: Die Hostfirewall dient zum Schutz des Hostbetriebssystems, auf dem der Hypervisor ausgeführt wird. Die Regeln werden so programmiert, dass nur dem Service Fabric Controller und Jumpboxes die Kommunikation mit dem Hostbetriebssystem über einen bestimmten Port gestattet wird. Die anderen Ausnahmen dienen zum Zulassen von DHCP-Antworten und DNS-Antworten. Azure verwendet eine Computerkonfigurationsdatei mit der Vorlage von Firewallregeln für das Hostbetriebssystem. Der Host selbst wird vor Angriffen von außen durch eine Windows-Firewall geschützt, die so konfiguriert ist, dass nur die Kommunikation von bekannten, authentifizierten Quellen zugelassen wird.
* **Gastfirewall**: Repliziert die Regeln im VM-Switchpaketfilter, ist aber in einer anderen Software programmiert (wie etwa der Teil der Windows-Firewall im Gastbetriebssystem). Die Gastfirewall eines virtuellen Computers kann so konfiguriert werden, dass die Kommunikation mit dem virtuellen Gastcomputer auch dann eingeschränkt wird, wenn die Kommunikation durch Konfigurationen im Host-IP-Filter zulässig ist. So können Sie beispielsweise die Gastfirewall eines virtuellen Computers zum Einschränken der Kommunikation zwischen zwei VNets verwenden, die für das Herstellen einer gegenseitigen Verbindung konfiguriert wurden.
* **Speicherfirewall (FW)**: Die Firewall im Speicher-Front-End filtert den Datenverkehr, sodass dieser nur die Ports 80/443 und andere benötigte Hilfsprogrammports passieren kann. Die Firewall im Speicher-Back-End beschränkt die Kommunikation auf Datenverkehr von den Speicher-Front-End-Servern.
* **Gateway von Virtual Network**: Das [Azure Virtual Network-Gateway](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) fungiert als standortübergreifendes Gateway und verbindet Ihre Workloads in Azure Virtual Network mit Ihren lokalen Standorten. Die Verbindung mit lokalen Standorten muss über [IPsec-Site-to-Site-VPN-Tunnel](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md) oder über [ExpressRoute](../expressroute/expressroute-introduction.md)-Verbindungen hergestellt werden. Für IPsec/IKE-VPN-Tunnel führen die Gateways IKE-Handshakes durch und richten die IPsec-S2S-VPN-Tunnel zwischen den virtuellen Netzwerken und lokalen Standorten ein. Virtuelle Netzwerkgateways fungieren außerdem als Abschluss für [Point-to-Site-VPNs](../vpn-gateway/vpn-gateway-point-to-site-create.md).

## <a name="secure-remote-access"></a>Sicherer Remotezugriff
In der Cloud gespeicherte Daten müssen ausreichende Schutzvorrichtungen aktiviert haben, um Exploits zu verhindern und bei der Übertragung die Vertraulichkeit und Integrität zu wahren. Dies schließt die Netzwerksteuerfunktionen ein, die mit den richtlinienbasierten, überprüfbaren Mechanismen zur Identitäts- und Zugriffsverwaltung einer Organisation einhergehen.

Die integrierte Kryptografietechnologie ermöglicht Ihnen das Verschlüsseln der Kommunikation innerhalb von und zwischen Bereitstellungen, zwischen Azure-Regionen und von Azure zu lokalen Datencentern. Der Administratorzugriff auf virtuelle Computer über [Remotedesktopsitzungen](../virtual-machines/virtual-machines-windows-classic-connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json), [Windows PowerShell-Remotesitzungen](http://blogs.technet.com/b/heyscriptingguy/archive/2013/09/07/weekend-scripter-remoting-the-cloud-with-windows-azure-and-powershell.aspx) und das Azure-Portal wird immer verschlüsselt.

Um Ihr lokales Datencenter sicher auf die Cloud zu erweitern, bietet Azure sowohl [Site-to-Site-VPN](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md) als auch [Point-to-Site-VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md) sowie dedizierte Verknüpfungen über [ExpressRoute](../expressroute/expressroute-introduction.md) (Verbindungen mit Azure Virtual Networks über VPN sind verschlüsselt).

### <a name="how-azure-implements-secure-remote-access"></a>Implementierung des sicheren Remotezugriffs in Azure
Für Verbindungen mit dem Azure-Portal sind immer eine Authentifizierung und SSL/TLS erforderlich. Sie können Verwaltungszertifikate konfigurieren, um eine sichere Verwaltung zu ermöglichen. Branchenübliche Sicherheitsprotokolle wie [SSTP](https://technet.microsoft.com/magazine/2007.06.cableguy.aspx) und [IPsec](https://en.wikipedia.org/wiki/IPsec) werden vollständig unterstützt.

[Azure ExpressRoute](../expressroute/expressroute-introduction.md) ermöglicht es Ihnen, private Verbindungen zwischen Azure-Datencentern und einer Infrastruktur bei Ihnen vor Ort oder in einer Kollokationsumgebung zu erstellen. ExpressRoute-Verbindungen verlaufen nicht über das öffentliche Internet. Sie zeichnen sich im Vergleich zu normalen internetbasierten Verknüpfungen durch höhere Zuverlässigkeit, höhere Geschwindigkeit, kürzere Wartezeiten und höhere Sicherheit aus. In bestimmten Fällen können durch die Übertragung von Daten zwischen lokalen Standorten und Azure mithilfe von ExpressRoute-Verbindungen auch drastische Kosteneinsparungen erzielt werden.

## <a name="logging-and-monitoring"></a>Protokollierung und Überwachung
Azure bietet eine authentifizierte Protokollierung sicherheitsrelevanter Ereignisse, die einen Audit-Trail generieren, und ist gegen Manipulationen geschützt. Dies schließt Systeminformationen ein – beispielsweise Sicherheitsereignisprotokolle in virtuellen Computern der Azure-Infrastruktur und in Azure AD. Die Überwachung von Sicherheitsereignissen umfasst das Sammeln von Ereignissen wie Änderungen an den IP-Adressen von DHCP- oder DNS-Servern, Zugriffsversuche auf Ports, per Voreinstellung blockierte Protokolle oder IP-Adressen, Änderungen an Sicherheitsrichtlinien oder Firewalleinstellungen, Konten- oder Gruppenerstellung sowie unerwartete Prozesse oder Treiberinstallationen.

![Microsoft-Antischadsoftware in Azure](./media/azure-security-getting-started/sec-azgsfig5.PNG)

Überwachungsprotokolle, in denen Zugriffe und Aktivitäten privilegierter Benutzer, autorisierte und nicht autorisierte Zugriffsversuche, Systemausnahmen und Informationsereignisse aufgezeichnet werden, werden für einen festgelegten Zeitraum aufbewahrt. Die Aufbewahrung von Protokollen liegt in Ihrem eigenen Ermessen, da Sie die Protokollerfassung und -aufbewahrung Ihren eigenen Anforderungen entsprechend konfigurieren.

### <a name="how-azure-implements-logging-and-monitoring"></a>Implementierung der Protokollierung und Überwachung in Azure
Azure stellt Verwaltungs-Agents und ASM-Agents (Azure Security Monitor) für jeden verwalteten Compute-, Speicher- oder Fabric-Knoten bereit, unabhängig davon, ob es sich um systemeigene oder virtuelle Knoten handelt. Jeder Verwaltungs-Agent ist so konfiguriert, dass er sich mit einem Zertifikat aus dem Azure-Zertifikatspeicher bei einem Dienstteam-Speicherkonto authentifiziert und vorkonfigurierte Diagnose- und Ereignisdaten an das Speicherkonto weiterleitet. Diese Agents werden nicht auf virtuellen Kundencomputern bereitgestellt.

Azure-Administratoren greifen über ein Webportal auf Protokolle zu, um einen authentifizierten und kontrollierten Zugriff auf die Protokolle zu gewährleisten. Ein Administrator kann Protokolle filtern, korrelieren und analysieren. Die Azure-Dienstteam-Speicherkonten für Protokolle werden vor direktem Administratorzugriff geschützt, um Manipulationen des Protokolls zu verhindern.

Microsoft sammelt Protokolle von Netzwerkgeräten mithilfe des Syslog-Protokolls und von Hostservern mithilfe der Microsoft-Überwachungssammeldienste. Diese Protokolle werden in einer Protokolldatenbank abgelegt, von der aus Warnungen zu verdächtigen Ereignissen generiert werden. Der Administrator kann auf diese Protokolle zugreifen und sie analysieren.

Die [Azure-Diagnose](https://msdn.microsoft.com/library/azure/gg433048.aspx) ist ein Azure-Feature, das Ihnen das Sammeln von Diagnosedaten aus einer in Azure ausgeführten Anwendung ermöglicht. Hierbei handelt es sich um Diagnosedaten zum Debuggen und Behandeln von Problemen, zum Messen der Leistung, zum Überwachen der Ressourcenauslastung, zum Analysieren des Datenverkehrs, zum Planen der Kapazität und zum Durchführen der Überwachung. Nach dem Erfassen der Diagnosedaten können diese zur dauerhaften Speicherung an ein Azure-Speicherkonto übertragen werden. Datenübertragungen können entweder geplant oder bedarfsgesteuert erfolgen.

## <a name="threat-mitigation"></a>Bedrohungsabwehr
Zusätzlich zur Isolierung, Verschlüsselung und Filterung setzt Azure eine Anzahl von Mechanismen und Verfahren zur Bedrohungsabwehr ein, um Infrastruktur und Dienste zu schützen. Dazu zählen interne Kontrollen und Technologien, die zum Erkennen und Beheben erweiterter Bedrohungen dienen, darunter DDoS, Berechtigungsausweitung und die [OWASP Top 10](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project).

Die Sicherheitskontrollen und Verfahren zur Risikoverwaltung, die Microsoft zur Sicherung der Cloudinfrastruktur einsetzt, verringern das Risiko von Sicherheitsvorfällen. Bei einem Vorfall steht das SIM-Team (Security Incident Management) innerhalb des Microsoft-OSSC-Teams (Online Security Services and Compliance) jederzeit bereit.

### <a name="how-azure-implements-threat-mitigation"></a>Implementierung der Bedrohungsabwehr in Azure
Azure verfügt über Sicherheitskontrollen, um die Bedrohungsabwehr zu implementieren und um Kunden bei der Abwehr potenzieller Sicherheitsrisiken in ihrer Umgebung zu unterstützen. In der folgenden Liste werden die von Azure angebotenen Funktionen zur Bedrohungsabwehr zusammengefasst:

* [Azure-Antischadsoftware](azure-security-antimalware.md) ist standardmäßig auf allen Infrastrukturservern aktiviert. Sie kann optional auch auf Ihren eigenen virtuellen Computern aktiviert werden.
* Microsoft bietet eine kontinuierliche Überwachung über Server, Netzwerke und Anwendungen hinweg, um Gefahren zu erkennen und Exploits zu verhindern. Administratoren werden durch automatisierte Warnungen über anomale Verhalten benachrichtigt, sodass sie sowohl bei internen als auch bei externen Gefahren korrigierende Maßnahmen ergreifen können.
* Sie können in Ihren Abonnements Drittanbieter-Sicherheitslösungen wie etwa Web Application Firewalls von [Barracuda](https://techlib.barracuda.com/ng54/deployonazure) bereitstellen.
* Das Penetrationstestkonzept von Microsoft umfasst „[Red-Teaming](http://download.microsoft.com/download/C/1/9/C1990DBA-502F-4C2A-848D-392B93D9B9C3/Microsoft_Enterprise_Cloud_Red_Teaming.pdf)“. Dabei greifen Microsoft-Sicherheitsexperten (kundenfremde) Live-Produktionssysteme in Azure an, um Schutzmaßnahmen gegen komplexe und dauerhafte Bedrohungen in der Praxis zu testen.
* Integrierte Bereitstellungssysteme verwalten die Verteilung und Installation von Sicherheitspatches über die gesamte Azure-Plattform.

## <a name="next-steps"></a>Nächste Schritte
[Azure Trust Center](https://azure.microsoft.com/support/trust-center/)

[Blog des Azure-Sicherheitsteams](http://blogs.msdn.com/b/azuresecurity/)

[Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx)

[Active Directory-Blog](http://blogs.technet.com/b/ad/)




<!--HONumber=Nov16_HO3-->


