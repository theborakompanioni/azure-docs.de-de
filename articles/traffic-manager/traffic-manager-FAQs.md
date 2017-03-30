---
title: "Azure Traffic Manager – Häufig gestellte Fragen (FAQs) | Microsoft-Dokumentation"
description: "Dieser Artikel enthält Antworten auf häufig gestellte Fragen zu Traffic Manager"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 75d5ff9a-f4b9-4b05-af32-700e7bdfea5a
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2017
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: 6d749e5182fbab04adc32521303095dab199d129
ms.openlocfilehash: cc095b419eae7e85590cdd323a5cf3809c45452e
ms.lasthandoff: 03/22/2017

---

# <a name="traffic-manager-frequently-asked-questions-faq"></a>Häufig gestellte Fragen (FAQ) zu Traffic Manager

## <a name="traffic-manager-basics"></a>Traffic Manager-Grundlagen

### <a name="what-ip-address-does-traffic-manager-use"></a>Welche IP-Adresse verwendet Traffic Manager?

Traffic Manager arbeitet, wie unter [Funktionsweise von Traffic Manager](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works) erläutert, auf der DNS-Ebene. Clients werden mithilfe von DNS-Antworten an den geeigneten Dienstendpunkt umgeleitet. Anschließend stellen Clients dann eine direkte Verbindung mit dem Dienstendpunkt her (nicht über Traffic Manager).

Traffic Manager bietet daher keinen Endpunkt und keine IP-Adresse für die Verbindungsherstellung von Clients. Wenn also für einen Dienst eine statische IP-Adresse benötigt wird, muss diese im Dienst und nicht in Traffic Manager konfiguriert werden.

### <a name="does-traffic-manager-support-sticky-sessions"></a>Unterstützt Traffic Manager persistente Sitzungen?

Traffic Manager arbeitet, wie unter [Funktionsweise von Traffic Manager](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works) erläutert, auf der DNS-Ebene. Clients werden mithilfe von DNS-Antworten an den geeigneten Dienstendpunkt umgeleitet. Anschließend stellen Clients dann eine direkte Verbindung mit dem Dienstendpunkt her (nicht über Traffic Manager). Traffic Manager hat somit keinen Zugriff auf den HTTP-Datenverkehr zwischen Client und Server.

Zudem handelt es sich bei der IP-Quelladresse der von Traffic Manager empfangenen DNS-Abfrage um die IP-Adresse des rekursiven DNS-Diensts (und nicht um die IP-Adresse des Clients). Aus diesem Grund kann Traffic Manager keine individuellen Clients nachverfolgen und keine persistenten Sitzungen implementieren. Diese Einschränkung besteht bei allen DNS-basierten Systemen zur Datenverkehrsverwaltung, nicht nur bei Verwendung von Traffic Manager.

### <a name="why-am-i-seeing-an-http-error-when-using-traffic-manager"></a>Warum tritt bei mir ein HTTP-Fehler auf, wenn ich Traffic Manager verwende?

Traffic Manager arbeitet, wie unter [Funktionsweise von Traffic Manager](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works) erläutert, auf der DNS-Ebene. Clients werden mithilfe von DNS-Antworten an den geeigneten Dienstendpunkt umgeleitet. Anschließend stellen Clients dann eine direkte Verbindung mit dem Dienstendpunkt her (nicht über Traffic Manager). HTTP-Datenverkehr zwischen Client und Server wird nicht über Traffic Manager abgewickelt. Daher müssen auftretende HTTP-Fehler auf Ihre Anwendung zurückzuführen sein. Damit der Client eine Verbindung mit der Anwendung herstellen kann, werden alle Schritte zur DNS-Auflösung durchgeführt. Hierzu gehört auch die Interaktion von Traffic Manager im Datenverkehrsfluss der Anwendung.

Konzentrieren Sie sich daher bei der weiteren Untersuchung auf die Anwendung.

Der vom Browser des Clients gesendete HTTP-Hostheader ist die häufigste Ursache für Probleme. Konfigurieren Sie die Anwendung so, dass der richtige Hostheader für den verwendeten Domänennamen akzeptiert wird. Informationen zu Endpunkten, die den Azure App Service nutzen, finden Sie unter [Konfigurieren eines benutzerdefinierten Domänennamens für eine Web-App in Azure App Services, der Traffic Manager verwendet](../app-service-web/web-sites-traffic-manager-custom-domain-name.md).

### <a name="what-is-the-performance-impact-of-using-traffic-manager"></a>Wie wirkt sich die Verwendung von Traffic Manager auf die Leistung aus?

Traffic Manager arbeitet, wie unter [Funktionsweise von Traffic Manager](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works) erläutert, auf der DNS-Ebene. Da Clients eine direkte Verbindung mit Ihren Endpunkten herstellen, hat die Verwendung von Traffic Manager keinerlei Auswirkungen auf die Leistung, sobald die Verbindung hergestellt wurde.

Da sich Traffic Manager mit Anwendungen auf der DNS-Ebene integriert, muss ein zusätzlicher DNS-Lookup erfolgen, der in die DNS-Auflösungskette eingefügt werden muss. Die DNS-Auflösungszeit wird durch Traffic Manager nur minimal beeinträchtigt. Traffic Manager verwendet ein globales Netzwerk von Namenservern sowie [Anycastnetzwerke](https://en.wikipedia.org/wiki/Anycast), um sicherzustellen, dass DNS-Abfragen immer an den nächstgelegenen verfügbaren Namenserver weitergeleitet werden. Dank der Zwischenspeicherung von DNS-Antworten wirkt sich die zusätzliche, durch die Verwendung von Traffic Manager bedingte DNS-Latenz lediglich auf einen Bruchteil der Sitzungen aus.

Mit der leistungsorientierten Methode wird der Datenverkehr an den nächstgelegenen verfügbaren Endpunkt geleitet. Das führt dazu, dass die mit dieser Methode verbundenen Auswirkungen auf die Gesamtleistung minimal sind. Eine höhere DNS-Latenz muss durch eine geringere Netzwerklatenz für den Endpunkt ausgeglichen werden.

### <a name="what-application-protocols-can-i-use-with-traffic-manager"></a>Welche Anwendungsprotokolle kann ich mit Traffic Manager verwenden?

Traffic Manager arbeitet, wie unter [Funktionsweise von Traffic Manager](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works) erläutert, auf der DNS-Ebene. Nach Abschluss des DNS-Lookups stellen Clients eine direkte Verbindung mit dem Dienstendpunkt her (nicht über Traffic Manager). Daher kann für diese Verbindung ein beliebiges Anwendungsprotokoll verwendet werden. Für die Endpunkt-Integritätsprüfungen von Traffic Manager wird jedoch entweder ein HTTP- oder ein HTTPS-Endpunkt benötigt. Für die Integritätsprüfung kann ein anderer Endpunkt als der Anwendungsendpunkt verwendet werden, mit dem Clients eine Verbindung herstellen.

### <a name="can-i-use-traffic-manager-with-a-naked-domain-name"></a>Kann ich Traffic Manager mit einem reinen Domänennamen verwenden?

Nein. Die DNS-Standards lassen keine Koexistenz von CNAME-Einträgen und anderen DNS-Einträgen mit demselben Namen zu. Die Spitze (oder der Stamm) einer DNS-Zone enthält immer zwei bereits vorhandene DNS-Einträge: den SOA-Eintrag und den autoritativen NS-Eintrag. Das bedeutet, dass im Zonenspitze kein CNAME-Eintrag erstellt werden kann, ohne gegen die DNS-Standards zu verstoßen.

Traffic Manager benötigt einen DNS-CNAME-Eintrag zum Zuordnen des Vanity-DNS-Namens. „www.contoso.com“ wird beispielsweise dem DNS-Namen des Traffic Manager-Profils „contoso.trafficmanager.net“ zugeordnet. Darüber hinaus gibt das Traffic Manager-Profil selbst einen zweiten DNS-CNAME-Eintrag zurück, um anzugeben, mit welchem Endpunkt der Client eine Verbindung herstellen soll.

Um dieses Problem zu umgehen, wird empfohlen, eine HTTP-Umleitung zu verwenden, um Datenverkehr von einem reinen Domänennamen an eine andere URL umzuleiten, die Traffic Manager verwenden kann. So kann beispielsweise die reine Domäne „contoso.com“ Benutzer an den CNAME „www.contoso.com“ weiterleiten, der auf den DNS-Namen von Traffic Manager verweist.

Die uneingeschränkte Unterstützung von Naked-Domänen in Traffic Manager ist Teil unseres Feature-Backlogs. Sie können die Unterstützung für diese Funktionsanforderung registrieren, indem Sie [auf unserer Community-Feedbackwebsite dafür abstimmen](https://feedback.azure.com/forums/217313-networking/suggestions/5485350-support-apex-naked-domains-more-seamlessly).


## <a name="traffic-manager-geographic-traffic-routing-method"></a>Geografische Methode für das Datenverkehrsrouting in Traffic Manager

### <a name="what-are-some-use-cases-where-geographic-routing-is-useful"></a>Was sind einige Anwendungsfälle, in denen geografisches Routing nützlich ist? 
Der geografische Routingtyp kann in jedem Szenario verwendet werden, in dem ein Azure-Kunde seine Benutzer auf der Grundlage geografischer Regionen unterscheiden muss. Ein Beispiel dafür wäre, für Benutzer aus bestimmten Regionen eine andere Benutzeroberfläche als für Benutzer aus anderen Regionen anzuzeigen. Ein weiteres Beispiel wäre die Einhaltung ortsbezogener Datenhoheitsmandate, für die es erforderlich ist, dass Benutzer aus einer bestimmten Region nur von Endpunkten in der betreffenden Region bedient werden.

### <a name="what-are-the-regions-that-are-supported-by-traffic-manager-for-geographic-routing"></a>Welche Bereiche werden von Traffic Manager für das geografische Routing unterstützt? 
Die Landes/Regionshierarchie, die von Traffic Manager verwendet wird, finden Sie [hier](traffic-manager-geographic-regions.md). Diese Seite wird jeweils auf dem neuesten Änderungsstatus gehalten, jedoch können Sie die gleichen Informationen auch programmgesteuert mithilfe der [Azure Traffic Manager-REST-API](https://docs.microsoft.com/rest/api/trafficmanager/) abrufen. 

### <a name="how-does-traffic-manager-determine-where-a-user-is-querying-from"></a>Wie bestimmt Traffic Manager, woher die Abfrage eines Benutzers stammt? 
Traffic Manager untersucht die IP-Quelladresse der Abfrage (dabei handelt es sich mit größter Wahrscheinlichkeit um einen lokalen DNS-Auflöser, der die Abfrage im Auftrag des Benutzers ausführt) und verwendet eine interne Zuordnung von IPs zu Regionen, um den Standort zu bestimmen. Diese Zuordnung wird fortlaufend aktualisiert, um Änderungen im Internet Rechnung zu tragen. 

###  <a name="does-an-endpoint-need-to-be-physically-located-in-the-same-region-as-the-one-it-is-configured-with-for-geographic-routing"></a>Muss sich ein Endpunkt physisch in der Region befinden, für die er für geografisches Routen konfiguriert ist? 
Nein, der Standort des Endpunkts bringt keine Einschränkungen hinsichtlich der ihm zuzuordnenden Regionen mit sich. Beispielsweise können einem Endpunkt in der Azure-Region „US-Central“ alle Benutzer aus Indien zugeleitet werden.

### <a name="can-i-assign-geographic-regions-to-endpoints-in-a-profile-that-is-not-configured-to-do-geographic-routing"></a>Kann ich geografische Regionen Endpunkten in einem Profil zuordnen, das nicht für geografisches Routing konfiguriert ist? 
Ja, falls die Routingmethode eines Profils nicht geografisch ist, können Sie die [Azure Traffic Manager-REST-API](https://docs.microsoft.com/rest/api/trafficmanager/) verwenden, um Endpunkten im betreffenden Profil geografische Regionen zuzuweisen. Diese Konfiguration wird für Profile von nicht geografischen Routingtypen ignoriert. Wenn Sie ein derartiges Profil zu einem späteren Zeitpunkt auf den geografischen Routingtyp umstellen, verwendet Traffic Manager diese Zuordnungen.


### <a name="when-i-try-to-change-the-routing-method-of-an-existing-profile-to-geographic-i-am-getting-an-error"></a>Führt es zu einem Fehler, wenn ich versuche, die Routingmethode eines vorhandenen Profils in geografisch zu ändern?
Allen Endpunkten in einem Profil mit geografischem Routing muss mindestens eine geografische Region zugeordnet sein. Um ein vorhandenes Profil in den geografischen Routingtyp zu konvertieren, müssen Sie zuerst mithilfe der [Azure Traffic Manager-REST-API](https://docs.microsoft.com/rest/api/trafficmanager/) allen seinen Endpunkten geografische Regionen zuordnen, bevor Sie den Routingtyp in geografisch ändern. Wenn Sie das Portal verwenden, müssen Sie zuerst die Endpunkte löschen, die Routingmethode des Profils in geografisch ändern und anschließend alle Endpunkte zusammen mit ihrer geografischen Regionszuordnung hinzufügen. 


###  <a name="why-is-it-strongly-recommended-that-customers-create-nested-profiles-instead-of-endpoints-under-a-profile-with-geographic-routing-enabled"></a>Warum wird es Kunden dringend empfohlen, in einem Profil mit aktiviertem geografischem Routing geschachtelte Profile anstelle von Endpunkten zu erstellen? 
Beim geografischen Routingtyp kann eine Region nur einem Endpunkt innerhalb eines Profils zugeordnet sein. Wenn der betreffende Endpunkt kein geschachtelter Typ mit zugeordnetem untergeordnetem Profil ist, fährt Traffic Manager auch bei nicht intaktem Integritätsstatus des betreffenden Endpunkts fort, Verkehr an den Endpunkt zu senden, da die Alternative, gar keinen Verkehr zu senden, nicht besser ist. Traffic Manager führt kein Failover zu einem anderen Endpunkt aus, selbst wenn die zugeordnete Region dem nicht mehr intakten Endpunkt „übergeordnet“ ist (z.B. erfolgt kein Failover auf einen anderen Endpunkt mit der Region „Europa“, wenn ein Endpunkt mit der Region „Spanien“ nicht mehr intakt ist). Dies erfolgt, um sicherzustellen, dass Traffic Manager die geografischen Grenzen beachtet, die Kunden in ihren Profilen konfiguriert haben. Um den Vorzug des Failovers auf einen anderen Endpunkt nutzen zu können, wenn die Integrität eines Endpunkts nicht mehr intakt ist, wird empfohlen, dass geografische Regionen geschachtelten Profilen, die mehrere Endpunkte enthalten, anstelle von einzelnen Endpunkten zugeordnet werden. Wenn ein Endpunkt innerhalb des untergeordneten Profils ausfällt, kann auf diese Weise ein Failover des Datenverkehrs auf einen anderen Endpunkt innerhalb des gleichen geschachtelten untergeordneten Profils erfolgen.

### <a name="are-there-any-restrictions-on-the-api-version-that-supports-this-routing-type"></a>Gibt es Einschränkungen hinsichtlich der API-Version, die diesen Routingtyp unterstützt?

Ja, nur die API-Version 2017-03-01 und höhere Versionen unterstützen den geografischen Routingtyp. Ältere API-Versionen können nicht zum Erstellen von Profilen vom geografischen Routingtyp oder zum Zuweisen von geografischen Regionen zu Endpunkten verwendet werden. Wenn eine ältere API-Version zum Abrufen von Profilen aus einem Azure-Abonnement verwendet wird, werden keine Profile mit dem geografischen Routingtyp zurückgegeben. Darüber hinaus wird bei Verwendung älterer API-Versionen für zurückgegebene Profile, die Endpunkte mit einer geografischen Regionszuweisung aufweisen, die geografische Regionszuweisung nicht angezeigt.



## <a name="traffic-manager-endpoints"></a>Traffic Manager-Endpunkte

### <a name="can-i-use-traffic-manager-with-endpoints-from-multiple-subscriptions"></a>Kann ich Traffic Manager mit Endpunkten mehrerer Abonnements verwenden?

Die Verwendung von Endpunkten aus mehreren Abonnements ist mit Azure-Web-Apps nicht möglich. Für Azure-Web-Apps dürfen mit Web-Apps verwendete, benutzerdefinierte Domänennamen nur innerhalb eines einzelnen Abonnements genutzt werden. Es ist nicht möglich, Web-Apps aus mehreren Abonnements mit dem gleichen Domänennamen zu verwenden.

Für andere Endpunkttypen kann Traffic Manager mit Endpunkten aus mehreren Abonnements verwendet werden. Das Konfigurieren von Traffic Manager hängt davon ab, ob Sie mit dem klassischen Bereitstellungsmodell oder der Resource Manager-Umgebung arbeiten.

* In Resource Manager können Endpunkte aus jedem Abonnement zu Traffic Manager hinzugefügt werden, solange die Person, die das Traffic Manager-Profil konfiguriert, über Lesezugriff für den Endpunkt verfügt. Diese Berechtigungen können über die [rollenbasierte Zugriffssteuerung (RBAC) von Azure Resource Manager](../active-directory/role-based-access-control-configure.md)gewährt werden.
* In dem klassischen Bereitstellungsmodell ist es für Traffic Manager erforderlich, dass sich der als Azure-Endpunkt konfigurierte Clouddienst bzw. die Web-App im gleichen Abonnement wie das Traffic Manager-Profil befindet. Clouddienstendpunkte in anderen Abonnements können als „externe“ Endpunkte zu Traffic Manager hinzugefügt werden. Diese externen Endpunkte werden als Azure-Endpunkte berechnet, nicht mit der externen Rate.

### <a name="can-i-use-traffic-manager-with-cloud-service-staging-slots"></a>Kann ich Traffic Manager mit „Stagingslots“ des Clouddiensts verwenden?

Ja. Stagingslots des Clouddiensts können in Traffic Manager als externe Endpunkte konfiguriert werden. Integritätsprüfungen werden weiterhin mit der Rate von Azure-Endpunkten berechnet. Änderungen des zugrunde liegenden Diensts werden nicht automatisch übernommen, da ein Endpunkt vom Typ „Extern“ verwendet wird. Mit externen Endpunkten kann Traffic Manager nicht erkennen, wenn der Clouddienst beendet oder gelöscht wird. Aus diesem Grund wird die Abrechnung für Integritätsprüfungen von Traffic Manager fortgesetzt, bis der Endpunkt deaktiviert oder gelöscht wird.

### <a name="does-traffic-manager-support-ipv6-endpoints"></a>Unterstützt Traffic Manager IPv6-Endpunkte?

Traffic Manager stellt derzeit keine IPv6-adressierbaren Nameserver zur Verfügung. Traffic Manager kann jedoch weiterhin von IPv6-Clients verwendet werden, die IPv6-Endpunkte verbinden. Ein Client erstellt keine DNS-Anfragen direkt an Traffic Manager. Stattdessen verwendet der Client einen rekursiven DNS-Dienst. Ein reiner IPv6-Client sendet Anforderungen an den rekursiven DNS-Dienst über IPv6. Der rekursive Dienst sollte anschließend in der Lage sein, die Traffic Manager-Namensserver per IPv4 zu kontaktieren.

Traffic Manager antwortet mit dem DNS-Namen des Endpunkts. Ein IPv6-Endpunkt kann unterstützt werden, indem ein DNS AAAA-Eintrag konfiguriert wird, mit dem der DNS-Name des Endpunkts auf die IPv6-Adresse verwiesen wird. Traffic Manager-Integritätsprüfungen unterstützen nur IPv4-Adressen. Der Dienst muss einen IPv4-Endpunkt auf dem gleichen DNS-Namen verfügbar machen.

### <a name="can-i-use-traffic-manager-with-more-than-one-web-app-in-the-same-region"></a>Kann ich Traffic Manager mit mehr als einer Web-App in derselben Region verwenden?

Normalerweise wird Traffic Manager verwendet, um Datenverkehr an Anwendungen zu leiten, die in verschiedenen Regionen bereitgestellt wurden. Traffic Manager kann aber auch eingesetzt werden, wenn eine Anwendung über mehr als eine Bereitstellung in derselben Region verfügt. Die Traffic Manager-Azure-Endpunkte lassen es nicht zu, dass demselben Traffic Manager-Profil mehr als ein Web-App-Endpunkt für dieselbe Azure-Region hinzugefügt wird.

Die folgenden Schritte stellen eine Umgehung dieser Einschränkung dar:

1. Überprüfen Sie, ob sich Ihre Endpunkte in verschiedenen Web-App-„Skalierungseinheiten“ befinden. Ein Domänenname muss zu einem einzelnen Standort in einer bestimmten Skalierungseinheit zugeordnet werden. Aus diesem Grund können sich zwei Web-Apps in der gleichen Skalierungseinheit nicht ein Traffic Manager-Profil teilen.
2. Fügen Sie Ihren Vanity-Domänennamen als einen benutzerdefinierten Hostnamen zu jeder Web-App hinzu. Jede Web-App muss sich in einer anderen Skalierungseinheit befinden. Alle Web-Apps müssen zu demselben Abonnement gehören.
3. Fügen Sie (nur) einen Web-App-Endpunkt dem Traffic Manager-Profil als Azure-Endpunkt hinzu.
4. Fügen Sie jeden weiteren Web-App-Endpunkt dem Traffic Manager-Profil als externen Endpunkt hinzu. Externe Endpunkte können nur mit dem Resource Manager-Bereitstellungsmodell hinzugefügt werden.
5. Erstellen Sie einen DNS CNAME-Eintrag in Ihrer Vanity-Domäne, der auf den DNS-Namen des Traffic Manager-Profils verweist (<…>.trafficmanager.net).
6. Greifen Sie über den Vanity-Domänennamen auf Ihre Website zu, nicht über den DNS-Namen des Traffic Manager-Profils.

##  <a name="traffic-manager-endpoint-monitoring"></a>Traffic Manager-Endpunktüberwachung

### <a name="is-traffic-manager-resilient-to-azure-region-failures"></a>Ist Traffic Manager gegenüber Ausfällen von Azure-Regionen resistent?

Traffic Manager ist eine Schlüsselkomponente der Bereitstellung von hoch verfügbaren Anwendungen in Azure.
Um hohe Verfügbarkeit bereitzustellen, muss Traffic Manager ein außergewöhnlich hohes Maß an Verfügbarkeit aufweisen und resistent gegenüber Regionsausfällen sein.

Traffic Manager-Komponenten sind standardmäßig gegenüber einem vollständigen Ausfall jeder Azure-Region resistent. Diese Resilienz gilt für alle Traffic Manager-Komponenten: DNS-Namensserver, API, Speicherebene und Endpunkt-Überwachungsdienst.

Im unwahrscheinlichen Fall eines Ausfalls einer gesamten Azure-Region wird erwartet, dass Traffic Manager weiterhin normal funktioniert. Bei Anwendungen, die in mehreren Azure-Regionen bereitgestellt wurden, kann darauf vertraut werden, dass Traffic Manager Datenverkehr an eine verfügbare Instanz der Anwendung leitet.

### <a name="how-does-the-choice-of-resource-group-location-affect-traffic-manager"></a>Wie wirkt sich die Wahl des Ressourcengruppenstandorts auf Traffic Manager aus?

Traffic Manager ist ein einzelner globaler Dienst. Es ist kein regionaler Dienst. Die Wahl des Ressourcengruppenstandorts spielt für Traffic Manager-Profile, die in der Ressourcengruppe bereitgestellt werden, keine Rolle.

Für Azure Resource Manager ist es erforderlich, dass für alle Ressourcengruppen ein Standort angegeben wird, mit dem der Standardstandort für die in dieser Ressourcengruppe bereitgestellten Ressourcen bestimmt wird. Wenn Sie ein Traffic Manager-Profil erstellen, wird es in einer Ressourcengruppe erstellt. Für alle Traffic Manager-Profile wird als Standort **global** verwendet, wobei der Standardwert der Ressourcengruppe überschrieben wird.

### <a name="how-do-i-determine-the-current-health-of-each-endpoint"></a>Wie kann ich den aktuellen Integritätsstatus jedes Endpunkts bestimmen?

Der aktuelle Überwachungsstatus jedes Endpunkts sowie das Gesamtprofil werden im Azure-Portal angezeigt. Diese Informationen sind auch über die [REST-API](https://msdn.microsoft.com/library/azure/mt163667.aspx) von Traffic Manager, [PowerShell-Cmdlets](https://msdn.microsoft.com/library/mt125941.aspx) und die [plattformübergreifende Azure-Befehlszeilenschnittstelle](../cli-install-nodejs.md) verfügbar.

Azure stellt keine alten Informationen über vergangene Endpunktintegrität bzw. die Möglichkeit zum Auslösen von Alarmen bei Änderungen der Endpunktintegrität bereit.

### <a name="can-i-monitor-https-endpoints"></a>Kann ich HTTPS-Endpunkte überwachen?

Ja. Traffic Manager unterstützt Tests per HTTPS. Konfigurieren Sie in der Überwachungskonfiguration **HTTPS** als Protokoll.

Traffic Manager kann keine Zertifikatüberprüfung bereitstellen. Dazu zählt Folgendes:

* Serverseitige Zertifikate werden nicht überprüft.
* Serverseitige SNI-Zertifikate werden nicht unterstützt.
* Clientzertifikate werden nicht unterstützt.

### <a name="what-host-header-do-endpoint-health-checks-use"></a>Welcher Hostheader wird für die Integritätsprüfungen für Endpunkte verwendet?

Traffic Manager verwendet Hostheader in HTTP- und HTTPS-Integritätsprüfungen. Der von Traffic Manager verwendete Hostheader ist der Name des Endpunktziels, das im Profil konfiguriert wurde. Der im Hostheader verwendete Wert kann nicht separat von der Zieleigenschaft („target“) angegeben werden.

### <a name="what-are-the-ip-addresses-from-which-the-health-checks-originate"></a>Wie lauten die IP-Adressen, von denen die Integritätsüberprüfungen stammen?

Die folgende Liste enthält die IP-Adressen, aus denen Traffic Manager-Integritätsprüfungen stammen können. Sie können anhand dieser Liste sicherstellen, dass eingehende Verbindungen von diesen IP-Adressen an den Endpunkten zur Überprüfung des Integritätsstatus zugelassen sind.

* 40.68.30.66
* 40.68.31.178
* 137.135.80.149
* 137.135.82.249
* 23.96.236.252
* 65.52.217.19
* 40.87.147.10
* 40.87.151.34
* 13.75.124.254
* 13.75.127.63
* 52.172.155.168
* 52.172.158.37
* 104.215.91.84
* 13.75.153.124
* 13.84.222.37
* 23.101.191.199
* 23.96.213.12
* 137.135.46.163
* 137.135.47.215
* 191.232.208.52
* 191.232.214.62
* 13.75.152.253
* 104.41.187.209
* 104.41.190.203

## <a name="traffic-manager-nested-profiles"></a>Geschachtelte Profile in Traffic Manager

### <a name="how-do-i-configure-nested-profiles"></a>Wie konfiguriere ich geschachtelte Profile?

Geschachtelte Traffic Manager-Profile können sowohl mit Azure Resource Manager als auch mit den klassischen Azure-REST-APIs, Azure PowerShell-Cmdlets und plattformübergreifenden Befehlen der Azure-Befehlszeilenschnittstelle konfiguriert werden. Sie werden auch im neuen Azure-Portal unterstützt. Über das klassische Portal werden sie jedoch nicht unterstützt.

### <a name="how-many-layers-of-nesting-does-traffic-manger-support"></a>Wie viele Schachtelungsebenen werden in Traffic Manager unterstützt?

Sie können Profile bis zu 10 Ebenen schachteln. Schleifen sind nicht zulässig.

### <a name="can-i-mix-other-endpoint-types-with-nested-child-profiles-in-the-same-traffic-manager-profile"></a>Kann ich andere Endpunkttypen mit geschachtelten untergeordneten Profilen im gleichen Traffic Manager-Profil kombinieren?

Ja. Es gibt keine Einschränkungen hinsichtlich der Kombination von Endpunkten verschiedener Typen innerhalb eines Profils.

### <a name="how-does-the-billing-model-apply-for-nested-profiles"></a>Wie wird das Abrechnungsmodell auf geschachtelte Profile angewendet?

Die Verwendung geschachtelter Profile wirkt sich nicht negativ auf die Preise aus.

Die Abrechnung für Traffic Manager besteht aus zwei Komponenten: Integritätsprüfungen der Endpunkte und Millionen von DNS-Abfragen.

* Überprüfungen der Endpunktintegrität: Wenn ein untergeordnetes Profil als Endpunkt in einem übergeordneten Profil konfiguriert wird, fallen keine Gebühren an. Die Überwachung von Endpunkten im untergeordneten Profil wird wie üblich abgerechnet.
* DNS-Abfragen: Jede Abfrage wird nur einmal gezählt. Die Abfrage eines übergeordneten Profils, die einen Endpunkt aus einem untergeordneten Profil zurückgibt, wird nur im übergeordneten Profil gezählt.

Ausführliche Informationen finden Sie auf der Seite [Traffic Manager Preise](https://azure.microsoft.com/pricing/details/traffic-manager/).

### <a name="is-there-a-performance-impact-for-nested-profiles"></a>Wirken sich geschachtelte Profile auf die Leistung aus?

Nein. Durch geschachtelte Profile entstehen keine Leistungseinbußen.

Die Traffic Manager-Namenserver durchlaufen bei der Verarbeitung der einzelnen DNS-Abfragen intern die Profilhierarchie. Eine DNS-Abfrage an ein übergeordnetes Profil kann eine DNS-Antwort mit einem Endpunkt aus einem untergeordneten Profil erhalten. Unabhängig davon, ob Sie ein einzelnes Profil oder geschachtelte Profile verwenden, wird nur ein einziger CNAME-Eintrag verwendet. Es gibt keine Notwendigkeit, für jedes Profil in der Hierarchie einen CNAME-Eintrag zu erstellen.

### <a name="how-does-traffic-manager-compute-the-health-of-a-nested-endpoint-in-a-parent-profile"></a>Wie berechnet Traffic Manager die Integrität eines geschachtelten Endpunkts in einem übergeordneten Profil?

Das übergeordnete Profil führt die Integritätsprüfung nicht direkt am untergeordneten Profil durch. Stattdessen wird mit der Integrität der Endpunkte des untergeordneten Profils die Gesamtintegrität des untergeordneten Profils berechnet. Diese Informationen werden in der Hierarchie der geschachtelten Profile nach oben weitergegeben, um die Integrität des geschachtelten Endpunkts zu bestimmen. Das übergeordnete Profil verwendet diese aggregierte Integrität, um festzulegen, ob der Datenverkehr an das untergeordnete Profil weitergeleitet wird.

Die folgende Tabelle beschreibt das Verhalten von Traffic Manager-Integritätsprüfungen für einen geschachtelten Endpunkt.

| Überwachungsstatus von untergeordneten Profilen | Überwachungsstatus von übergeordneten Endpunkten | Hinweise |
| --- | --- | --- |
| Deaktiviert. Das untergeordnete Profil wurde deaktiviert. |Stopped |Der Status des übergeordneten Endpunkts ist "Beendet", nicht "Deaktiviert". Der Status "Deaktiviert" zeigt lediglich an, dass Sie den Endpunkt im übergeordneten Profil deaktiviert haben. |
| Heruntergestuft. Der Status mindestens eines Endpunkts wurde heruntergestuft. |Online: Die Anzahl von Onlineendpunkten im untergeordneten Profil ist mindestens gleich dem Wert von MinChildEndpoints.<BR>CheckingEndpoint: Die Anzahl von Online- plus CheckingEndpoint-Endpunkten im untergeordneten Profil ist mindestens gleich dem Wert von MinChildEndpoints.<BR>Heruntergestuft: andere Fälle. |Der Datenverkehr wird an einen Endpunkt mit dem Status „CheckingEndpoint“ weitergeleitet. Wenn MinChildEndpoints zu hoch festgelegt wurde, wird der Endpunkt immer heruntergestuft. |
| Online. Mindestens ein untergeordneter Profilendpunkt weist den Status „Online“ auf. Kein Endpunkt weist den Status „Heruntergestuft“ auf. |Siehe oben. | |
| CheckingEndpoints. Mindestens ein untergeordneter Profilendpunkt weist den Status „CheckingEndpoint“ auf. Kein Endpunkt weist den Status „Online“ oder „Heruntergestuft“ auf. |Wie oben. | |
| Inaktiv. Alle untergeordneten Profilendpunkte sind „Deaktiviert“ oder „Beendet“, oder dieses Profil weist keine Endpunkte auf. |Beendet | |

## <a name="next-steps"></a>Nächste Schritte:
- Informieren Sie sich ausführlicher über [Endpunktüberwachung und automatisches Failover](../traffic-manager/traffic-manager-monitoring.md)von Traffic Manager.
- Informieren Sie sich ausführlicher über [Traffic Manager-Routingmethoden](../traffic-manager/traffic-manager-routing-methods.md).
