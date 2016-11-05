---
title: Traffic Manager-Endpunkttypen | Microsoft Docs
description: In diesem Artikel werden die unterschiedlichen Arten von Endpunkten beschrieben, die mit Azure Traffic Manager verwendet werden können.
services: traffic-manager
documentationcenter: ''
author: sdwheeler
manager: carmonm
editor: tysonn

ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/08/2016
ms.author: sewhee

---
# Traffic Manager-Endpunkte
Mit Microsoft Azure Traffic Manager können Sie steuern, wie Datenverkehr von Benutzern auf Ihre Anwendungsbereitstellungen in verschiedenen Rechenzentren oder an anderen Standorten weltweit verteilt wird.

Jede Anwendungsbereitstellung muss in Traffic Manager als „Endpunkt“ konfiguriert werden. Wenn Traffic Manager eine DNS-Anforderung empfängt, wird einer dieser Endpunkte für die Rückgabe in der DNS-Antwort ausgewählt. Dies richtet sich nach der derzeitigen Endpunktverfügbarkeit und der gewählten Methode für das Datenverkehrsrouting. Weitere Informationen finden Sie unter [Funktionsweise von Traffic Manager](traffic-manager-how-traffic-manager-works.md).

Auf dieser Seite wird beschrieben, wie Traffic Manager Endpunkte unterschiedlichen Typs unterstützt.

## Arten von Traffic Manager-Endpunkten
Es gibt drei Arten von Endpunkten, die von Traffic Manager unterstützt werden:

* **Azure-Endpunkte** werden für Dienste verwendet, die unter Azure gehostet werden.
* **Externe Endpunkte** werden für außerhalb von Azure gehostete Dienste verwendet – entweder lokal oder über einen anderen Hostinganbieter.
* **Geschachtelte Endpunkte** werden zum Kombinieren von Traffic Manager-Profilen verwendet, um flexiblere Schemas für das Routing von Datenverkehr zu erstellen. So können die Anforderungen von größeren und komplexeren Bereitstellungen erfüllt werden.

Es gibt keine Einschränkung, wie Endpunkte unterschiedlichen Typs in einem Traffic Manager-Profil kombiniert werden können. Jedes Profil kann eine beliebige Mischung von Endpunkttypen enthalten.

In den folgenden Abschnitten werden die einzelnen Endpunkttypen ausführlicher beschrieben.

### Azure-Endpunkte
Azure-Endpunkte werden zum Konfigurieren von Azure-basierten Diensten in Traffic Manager verwendet. Azure-Endpunkte unterstützen derzeit die folgenden Azure-Ressourcentypen:

* „Klassische“ IaaS-VMs und PaaS-Clouddienste
* Web-Apps
* PublicIPAddress-Ressourcen (können entweder direkt oder über einen Azure Load Balancer mit VMs verbunden werden) Beachten Sie, dass der publicIpAddress-Ressource ein DNS-Name zugewiesen werden muss, damit sie in Traffic Manager verwendet werden kann.

PublicIPAddress-Ressourcen sind Azure Resource Manager-Ressourcen und nicht in den Azure-Dienstverwaltungs-APIs enthalten. Daher werden sie nur in Azure Resource Manager-Oberflächen von Traffic Manager unterstützt. Die anderen Endpunkttypen werden sowohl für Resource Manager- als auch für Dienstverwaltungsoberflächen in Traffic Manager unterstützt.

Bei der Verwendung von Azure-Endpunkten erkennt Traffic Manager, wann eine „klassische“ IaaS-VM oder ein PaaS-Clouddienst oder eine Web-App beendet und gestartet werden. Dies spiegelt sich im Endpunktstatus wider (Details unter [Traffic Manager-Endpunktüberwachung](traffic-manager-monitoring.md#endpoint-and-profile-status)). Wenn der zugrunde liegende Dienst beendet wird, berechnet Traffic Manager keine Überprüfungen der Endpunktintegrität mehr und leitet keinen Datenverkehr an den Endpunkt weiter. Wenn der Dienst wieder gestartet wird, wird die Berechnung der Kosten fortgesetzt, und der Endpunkt ist wieder für den Empfang von Datenverkehr verfügbar. Dies gilt nicht für PublicIpAddress-Endpunkte.

### Externe Endpunkte
Externe Endpunkte werden zum Konfigurieren von Traffic Manager verwendet, um Datenverkehr an Dienste außerhalb von Azure zu leiten, z.B. einen lokal gehosteten Dienst oder einen bei einem anderen Anbieter gehosteten Dienst.

Externe Endpunkte können allein oder in Kombination mit Azure-Endpunkten in demselben Traffic Manager-Profil verwendet werden. Die Kombination von Azure-Endpunkten mit externen Endpunkten ermöglicht viele unterschiedliche Szenarien:

* Verwenden von Azure zum Bereitstellen einer erhöhten Redundanz für eine vorhandene lokale Anwendung in einem Aktiv/Aktiv- oder Aktiv/Passiv-Failovermodell
* Verwenden von Azure zum Erweitern einer vorhandenen lokalen Anwendung auf weitere geografische Standorte in Kombination mit [Traffic Manager-Datenverkehrrouting vom Typ „Leistung“](traffic-manager-routing-methods.md#performance-traffic-routing-method), um die Anwendungslatenz zu reduzieren und die Leistung für Endbenutzer zu verbessern
* Verwenden von Azure zum Bereitstellen zusätzlicher Kapazität für eine vorhandene lokale Anwendung, entweder fortlaufend oder als „Burst-to-Cloud“ für Spitzenlasten

In bestimmten Fällen kann es hilfreich sein, externe Endpunkte zum Verweisen auf Azure-Dienste zu verwenden (Beispiele unter [Häufig gestellte Fragen](#faq)). Die Integritätsprüfungen werden dann mit der Rate der Azure-Endpunkte berechnet, nicht mit der Rate der externen Endpunkte. Wenn Sie den zugrunde liegenden Dienst beenden oder löschen, werden die entsprechenden Integritätsprüfungen im Gegensatz zu Azure-Endpunkten aber weiter berechnet, bis Sie den Endpunkt in Traffic Manager explizit deaktivieren oder löschen.

### Geschachtelte Endpunkte
Geschachtelte Endpunkte werden zum Kombinieren von Traffic Manager-Profilen verwendet, um flexiblere Schemas für das Routing von Datenverkehr zu erstellen. So können die Anforderungen von größeren und komplexeren Bereitstellungen erfüllt werden.

Bei geschachtelten Endpunkten wird ein „untergeordnetes“ Profil einem „übergeordneten“ Profil als Endpunkt hinzugefügt, um eine Hierarchie zu erstellen. Die untergeordneten und übergeordneten Profile können wiederum andere Endpunkte eines beliebigen Typs enthalten, z.B. auch andere geschachtelte Profile.

Weitere Informationen finden Sie unter [Geschachtelte Traffic Manager-Profile](traffic-manager-nested-profiles.md).

## Web-Apps als Endpunkte
Es gelten noch einige zusätzliche Aspekte in Bezug auf die Konfiguration von Web-Apps als Endpunkte in Traffic Manager:

1. Nur Web-Apps mit der SKU „Standard“ oder höher können mit Traffic Manager verwendet werden. Aufrufe von Traffic Manager zum Hinzufügen einer Web-App mit einer niedrigeren SKU sind nicht erfolgreich. Das Downgraden der SKU einer vorhandenen Web-App führt dazu, dass Traffic Manager keinen Datenverkehr mehr an diese Web-App sendet. Der Endpunkt kann aus Traffic Manager entfernt werden.
2. Wenn vom Web-Apps-Dienst eine HTTP-Anforderung empfangen wird, wird der „Hostheader“ in der Anforderung verwendet, um zu bestimmen, welche Web-App die Anforderung verarbeiten soll. Der Hostheader enthält den DNS-Namen zum Initiieren der Anforderung, z.B. „contosoapp.azurewebsites.net“. Um einen anderen DNS-Namen mit Ihrer Web-App verwenden zu können, muss der DNS-Name als benutzerdefinierte Domäne für die App registriert werden. Wenn ein Web-App-Endpunkt einem Traffic Manager-Profil als Azure-Endpunkt hinzugefügt wird, wird der DNS-Name des Traffic Manager-Profils automatisch als benutzerdefinierte Domäne für die App registriert. Diese Registrierung wird automatisch entfernt, wenn der Endpunkt gelöscht wird.
3. Eine Web-App wird normalerweise als Azure-Endpunkt konfiguriert. Es gibt aber Fälle, in denen es nützlich ist, eine Web-App mit einem externen Endpunkt zu konfigurieren (siehe beispielsweise nächstes Element). Web-Apps können nur als externer Endpunkt in Traffic Manager konfiguriert werden, wenn die Resource Manager-Oberflächen von Traffic Manager verwendet werden. Für die Oberflächen der Dienstverwaltung wird dies nicht unterstützt.
4. Jedes Traffic Manager-Profil kann maximal einen Web-App-Endpunkt aus jeder Azure-Region aufweisen. Informationen zur Umgehung dieser Einschränkung finden Sie unter [Häufig gestellte Fragen](#faq).

## Aktivieren und Deaktivieren von Endpunkten
Das Deaktivieren eines Endpunkts in Traffic Manager ist sehr hilfreich, um Datenverkehr vorübergehend von einem Endpunkt zu entfernen, der sich im Wartungsmodus befindet oder der erneut bereitgestellt wird. Sobald der Endpunkt wieder betriebsbereit ist, kann er erneut aktiviert werden.

Endpunkte können per Traffic Manager-Portal, PowerShell, CLI oder REST-API aktiviert und deaktiviert werden. All diese Komponenten werden sowohl für die Resource Manager-Oberfläche als auch für die Oberfläche der Dienstverwaltung unterstützt.

> [!NOTE]
> Das Deaktivieren eines Azure-Endpunkts hat nichts mit dessen Bereitstellungsstatus in Azure zu tun. Ein Azure-Dienst (z.B. eine VM oder Web-App) wird auch dann weiter ausgeführt und kann Datenverkehr empfangen, wenn er in Traffic Manager deaktiviert wurde, sofern Datenverkehr direkt an den Dienst adressiert ist, anstatt über den DNS-Namen des Traffic Manager-Profils. Weitere Informationen finden Sie unter [Funktionsweise von Traffic Manager](traffic-manager-how-traffic-manager-works.md).
> 
> 

Wenn ein Endpunkt deaktiviert ist, wird er nicht mehr in DNS-Antworten zurückgegeben, und es wird kein Datenverkehr an den Endpunkt geleitet. Außerdem werden die Integritätsprüfungen für den Endpunkt beendet und nicht mehr berechnet. Das Deaktivieren eines Endpunkts entspricht dem Löschen eines Endpunkts, aber mit dem Unterschied, dass der Endpunkt danach leichter wieder aktiviert werden kann.

Die jeweilige Verfügbarkeit eines Endpunkts für den Empfang von Datenverkehr richtet sich nach dem Profilstatus (aktiviert/deaktiviert), dem Endpunktstatus (aktiviert/deaktiviert) und den Ergebnissen der Integritätsprüfungen für den Endpunkt. Weitere Informationen finden Sie unter [Traffic Manager-Endpunktüberwachung](traffic-manager-monitoring.md#endpoint-and-profile-status).

> [!NOTE]
> Da der Traffic Manager auf DNS-Ebene verwendet wird, kann er vorhandene Verbindungen mit Endpunkten nicht beeinflussen. Beim Weiterleiten von Datenverkehr zwischen Endpunkten (entweder durch das Ändern der Profileinstellungen oder beim Failover oder Failback) leitet Traffic Manager neue Verbindungen an verfügbare Endpunkte weiter. Andere Endpunkte erhalten unter Umständen über vorhandene Verbindungen aber weiterhin Datenverkehr, bis diese Sitzungen beendet werden. Um diesen Datenverkehr aus den vorhandenen Verbindungen zu beseitigen, sollte die Sitzungsdauer für Anwendungen verringert werden, die für einen Endpunkt jeweils verwendet wird.
> 
> 

Wenn alle Endpunkte eines Profils deaktiviert werden oder das Profil selbst deaktiviert wird, werden DNS-Abfragen mit einer „NXDOMAIN“-Antwort beantwortet. Dies entspricht der Vorgehensweise nach dem Löschen des Profils.

## Häufig gestellte Fragen
### Kann ich Traffic Manager mit Endpunkten mehrerer Abonnements verwenden?
Für Azure-Web-Apps ist dies nicht möglich. Der Grund: Für Web-Apps ist es erforderlich, dass mit Web-Apps verwendete benutzerdefinierte Domänennamen nur innerhalb eines einzelnen Abonnements genutzt werden. Es ist nicht möglich, Web-Apps aus mehreren Abonnements mit dem gleichen Domänennamen zu verwenden, daher können sie nicht mit Traffic Manager genutzt werden.

Für andere Endpunkttypen kann Traffic Manager mit Endpunkten aus mehreren Abonnements verwendet werden. Ihre Vorgehensweise hängt hierbei davon ab, ob Sie die Dienstverwaltungs-APIs oder die Resource Manager-APIs für Traffic Manager verwenden. Im [Azure-Portal](https://portal.azure.com) wird Resource Manager verwendet, im [„klassischen“ Portal](https://manage.windowsazure.com) dagegen die Dienstverwaltung.

In Resource Manager können Endpunkte aus jedem Abonnement zu Traffic Manager hinzugefügt werden, solange die Person, die das Traffic Manager-Profil konfiguriert, über Lesezugriff für den Endpunkt verfügt. Diese Berechtigungen können über die [rollenbasierte Zugriffssteuerung (RBAC) von Azure Resource Manager](../active-directory/role-based-access-control-configure.md) gewährt werden.

In der Dienstverwaltung ist es für Traffic Manager erforderlich, dass sich der als Azure-Endpunkt konfigurierte Clouddienst bzw. die Web-App im gleichen Abonnement wie das Traffic Manager-Profil befindet. Clouddienst-Endpunkte in anderen Abonnements können als „externe“ Endpunkte zu Traffic Manager hinzugefügt werden (sie werden weiterhin mit der Rate für interne Endpunkte berechnet).

### Kann ich Traffic Manager mit „Stagingslots“ des Clouddiensts verwenden?
Ja. Stagingslots des Clouddiensts können in Traffic Manager als externe Endpunkte konfiguriert werden.

Da sich der vom Endpunkt referenzierte Dienst in Azure befindet, werden Integritätsprüfungen weiterhin mit der Rate für Azure-Endpunkte berechnet.

Änderungen des zugrunde liegenden Diensts werden nicht automatisch übernommen, da ein Endpunkt vom Typ „Extern“ verwendet wird. Wenn der Clouddienst beendet oder gelöscht wird, werden die Integritätsprüfungen des Traffic Manager-Endpunkts daher weiterhin berechnet, bis der Endpunkt in Traffic Manager deaktiviert oder gelöscht wird.

### Unterstützt Traffic Manager IPv6-Endpunkte?
Ja. Von Traffic Manager werden derzeit zwar keine per IPv6 adressierbaren Namensserver bereitgestellt, aber er kann trotzdem von IPv6-Clients verwendet werden, die Verbindungen mit IPv6-Endpunkten herstellen.

Ein reiner IPv6-Client kann Traffic Manager trotzdem verwenden, da der Client keine DNS-Anforderungen direkt an Traffic Manager sendet, sondern einen rekursiven DNS-Dienst verwendet. Er kann Anforderungen an diesen Dienst per IPv6 senden, und der rekursive Dienst sollte dann in der Lage sein, die Traffic Manager-Namensserver per IPv4 zu kontaktieren.

Nach dem Empfang einer DNS-Abfrage antwortet Traffic Manager mit dem DNS-Namen des Endpunkts, mit dem der Client eine Verbindung herstellen soll. Ein IPv6-Endpunkt kann unterstützt werden, indem einfach ein DNS AAAA-Eintrag konfiguriert wird, mit dem der DNS-Name des Endpunkts auf die IPv6-Adresse verwiesen wird.

Beachten Sie, dass der Dienst auch einen IPv4-Endpunkt verfügbar machen muss, damit die Traffic Manager-Integritätsprüfungen richtig funktionieren. Diese Zuordnung muss über denselben DNS-Endpunktnamen erfolgen, indem ein DNS A-Eintrag verwendet wird.

### Kann ich Traffic Manager mit mehr als einer Web-App in derselben Region verwenden?
Normalerweise wird Traffic Manager verwendet, um Datenverkehr an Anwendungen zu leiten, die in verschiedenen Regionen bereitgestellt wurden. Traffic Manager kann aber auch eingesetzt werden, wenn eine Anwendung über mehr als eine Bereitstellung in derselben Region verfügt.

Im Fall von Web-Apps lassen die Traffic Manager-Azure-Endpunkte es aber nicht zu, dass demselben Traffic Manager-Profil mehr als ein Web-App-Endpunkt für dieselbe Azure-Region hinzugefügt wird. Die folgenden Schritte stellen eine Umgehung dieser Einschränkung dar:

1. Überprüfen Sie, ob sich Ihre Web-Apps in derselben Region in unterschiedlichen Web-App-„Skalierungseinheiten“ befinden, also unterschiedlichen Instanzen des Web-App-Diensts. Überprüfen Sie hierzu den DNS-Pfad für den DNS-Eintrag „<...>.azurewebsites.net“. Die Skalierungseinheit sieht in etwa wie folgt aus: „waws-prod-xyz-123.vip.azurewebsites.net“. Ein Domänenname muss einer einzelnen Website in einer Skalierungseinheit zugeordnet sein. Aus diesem Grund ist es nicht möglich, dass zwei Web-Apps in derselben Skalierungseinheit ein Traffic Manager-Profil gemeinsam nutzen.
2. Angenommen, jede Web-App befindet sich in einer anderen Skalierungseinheit. Fügen Sie Ihren Vanity-Domänennamen dann jeder Web-App als benutzerdefinierten Hostnamen hinzu. Hierfür ist es erforderlich, dass alle Web-Apps zum gleichen Abonnement gehören.
3. Fügen Sie nur wie gewohnt einen Web-App-Endpunkt dem Traffic Manager-Profil als Azure-Endpunkt hinzu.
4. Fügen Sie jeden weiteren Web-App-Endpunkt dem Traffic Manager-Profil als externen Endpunkt hinzu. Sie müssen hierfür die Resource Manager-Oberfläche für Traffic Manager verwenden, nicht die Oberfläche der Dienstverwaltung.
5. Erstellen Sie einen DNS CNAME-Eintrag aus Ihrer Vanity-Domäne (wie oben in Schritt 2) zum DNS-Namen des Traffic Manager-Profils (<…>.trafficmanager.net).
6. Greifen Sie über den Vanity-Domänennamen auf Ihre Website zu, nicht über den DNS-Namen des Traffic Manager-Profils.

## Nächste Schritte
* Informationen zur [Funktionsweise von Traffic Manager](traffic-manager-how-traffic-manager-works.md)
* Informationen zu [Endpunktüberwachung und automatisches Failover](traffic-manager-monitoring.md) von Traffic Manager
* Informationen zu Traffic Manager-[Routingmethoden für Datenverkehr](traffic-manager-routing-methods.md)

<!---HONumber=AcomDC_0824_2016-->