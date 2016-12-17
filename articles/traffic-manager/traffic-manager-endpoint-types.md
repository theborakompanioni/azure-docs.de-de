---
title: Traffic Manager-Endpunkttypen | Microsoft Docs
description: "In diesem Artikel werden die unterschiedlichen Arten von Endpunkten beschrieben, die mit Azure Traffic Manager verwendet werden können."
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 4e506986-f78d-41d1-becf-56c8516e4d21
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2016
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: 69b94c93ad3e9c9745af8485766b4237cac0062c
ms.openlocfilehash: 2ced9e73a65160f4f3c8ba92affc143ca554d07c

---

# <a name="traffic-manager-endpoints"></a>Traffic Manager-Endpunkte
Mit Microsoft Azure Traffic Manager können Sie steuern, wie Datenverkehr im Netzwerk auf Anwendungsbereitstellungen in verschiedenen Rechenzentren verteilt wird. Sie konfigurieren jede Anwendungsbereitstellung in Traffic Manager als „Endpunkt“. Wenn Traffic Manager eine DNS-Anforderung empfängt, wählt er einen verfügbaren Endpunkt, der in der DNS-Antwort zurückgegeben wird. Traffic Manager trifft die Auswahl nach dem aktuellen Endpunktstatus und der Methode für das Datenverkehrsrouting. Weitere Informationen finden Sie unter [Funktionsweise von Traffic Manager](traffic-manager-how-traffic-manager-works.md).

Es gibt drei Arten von Endpunkten, die von Traffic Manager unterstützt werden:

* **Azure-Endpunkte** werden für Dienste verwendet, die unter Azure gehostet werden.
* **Externe Endpunkte** werden für außerhalb von Azure gehostete Dienste verwendet – entweder lokal oder über einen anderen Hostinganbieter.
* **Geschachtelte Endpunkte** werden zum Kombinieren von Traffic Manager-Profilen verwendet, um flexiblere Schemas für das Routing von Datenverkehr zu erstellen. So können die Anforderungen von größeren und komplexeren Bereitstellungen erfüllt werden.

Es gibt keine Einschränkung, wie Endpunkte unterschiedlichen Typs in einem Traffic Manager-Profil kombiniert werden können. Jedes Profil kann eine beliebige Mischung von Endpunkttypen enthalten.

In den folgenden Abschnitten werden die einzelnen Endpunkttypen ausführlicher beschrieben.

## <a name="azure-endpoints"></a>Azure-Endpunkte

Azure-Endpunkte werden für Azure-basierte Dienste in Traffic Manager verwendet. Die folgenden Typen von Azure-Ressourcen werden unterstützt:

* „Klassische“ IaaS-VMs und PaaS-Clouddienste
* Web-Apps
* PublicIPAddress-Ressourcen (können entweder direkt oder über einen Azure Load Balancer mit VMs verbunden werden) Der publicIpAddress-Ressource muss ein DNS-Name zugewiesen werden, damit sie in einem Traffic Manager-Profil verwendet werden kann.

PublicIPAddress-Ressourcen sind Azure Resource Manager-Ressourcen. Diese sind in dem klassischen Bereitstellungsmodell nicht vorhanden. Daher werden sie nur in Azure Resource Manager-Oberflächen von Traffic Manager unterstützt. Die anderen Endpunkttypen werden über Resource Manager und das klassische Bereitstellungsmodell unterstützt.

Bei der Verwendung von Azure-Endpunkten erkennt Traffic Manager, wenn eine „klassische“ IaaS-VM, ein Clouddienst oder eine Web-App beendet und gestartet wird. Dieser Status wird im Endpunktstatus wiedergegeben. Weitere Informationen finden Sie unter [Traffic Manager-Endpunktüberwachung](traffic-manager-monitoring.md#endpoint-and-profile-status). Wenn der zugrunde liegende Dienst beendet wird, führt Traffic Manager keine Endpunkt-Systemdiagnosen oder direkten Datenverkehr zum Endpunkt durch. Für die beendete Instanz treten keine Traffic Manager-Abrechnungsereignisse auf. Wenn der Dienst neu gestartet wird, wird die Abrechnung fortgesetzt und der Endpunkt ist berechtigt, Datenverkehr zu empfangen. Diese Erkennung gilt nicht für PublicIpAddress-Endpunkte.

## <a name="external-endpoints"></a>Externe Endpunkte

Externe Endpunkte werden für Dienste außerhalb von Azure verwendet. Beispielsweise ein Dienst, der lokal oder von einem anderen Anbieter gehostet wird. Externe Endpunkte können allein oder in Kombination mit Azure-Endpunkten in demselben Traffic Manager-Profil verwendet werden. Die Kombination von Azure-Endpunkten mit externen Endpunkten ermöglicht unterschiedliche Szenarios:

* Verwenden Sie Azure zum Bereitstellen einer erhöhten Redundanz für eine vorhandene lokale Anwendung in einem Aktiv/Aktiv- oder Aktiv/Passiv-Failovermodell.
* Erweitern Sie eine vorhandene lokale Anwendung zur Reduzierung der Anwendungslatenz für Benutzer auf der ganzen Welt auf zusätzliche geografische Standorte in Azure. Weitere Informationen finden Sie unter [Traffic Manager Leistungsdatenverkehrrouting](traffic-manager-routing-methods.md#performance-traffic-routing-method).
* Verwenden Sie Azure zum Bereitstellen zusätzlicher Kapazität für eine vorhandene lokale Anwendung, entweder fortlaufend oder als „Burst-to-Cloud“ für Spitzenlasten.

In bestimmten Fällen ist es hilfreich, externe Endpunkte zum Verweisen auf Azure-Dienste zu verwenden (Beispiele unter [Häufig gestellte Fragen](#faq)). Die Integritätsprüfungen werden dann mit der Rate der Azure-Endpunkte berechnet, nicht mit der Rate der externen Endpunkte. Wenn Sie den zugrunde liegenden Dienst beenden oder löschen, werden die entsprechenden Integritätsprüfungen im Gegensatz zu Azure-Endpunkten aber weiter berechnet, bis Sie den Endpunkt in Traffic Manager deaktivieren oder löschen.

## <a name="nested-endpoints"></a>Geschachtelte Endpunkte

Geschachtelte Endpunkte werden zum Kombinieren von Traffic Manager-Profilen verwendet, um flexiblere Schemas für das Routing von Datenverkehr zu erstellen. So können die Anforderungen von größeren und komplexeren Bereitstellungen erfüllt werden. Bei geschachtelten Endpunkten wird ein „untergeordnetes“ Profil einem „übergeordneten“ Profil als Endpunkt hinzugefügt. Die untergeordneten und übergeordneten Profile können wiederum andere Endpunkte eines beliebigen Typs enthalten, z.B. auch andere geschachtelte Profile. Weitere Informationen finden Sie unter [Geschachtelte Traffic Manager-Profile](traffic-manager-nested-profiles.md).

## <a name="web-apps-as-endpoints"></a>Web-Apps als Endpunkte

Es gelten noch einige zusätzliche Aspekte in Bezug auf die Konfiguration von Web-Apps als Endpunkte in Traffic Manager:

1. Nur Web-Apps mit der SKU „Standard“ oder höher können mit Traffic Manager verwendet werden. Der Versuch, eine Web App mit einer niedrigeren SKU hinzuzufügen, löst einen Fehler aus. Das Herabstufen der SKU einer vorhandenen Web-App führt dazu, dass Traffic Manager keinen Datenverkehr mehr an diese Web-App sendet.
2. Wenn ein Endpunkt eine HTTP-Anforderung empfängt, wird der „Hostheader“ in der Anforderung verwendet, um zu bestimmen, welche Web-App die Anforderung verarbeiten soll. Der Hostheader enthält den DNS-Namen zum Initiieren der Anforderung, z.B. „contosoapp.azurewebsites.net“. Um einen anderen DNS-Namen mit Ihrer Web-App verwenden zu können, muss der DNS-Name als benutzerdefinierter Domänenname für die App registriert sein. Wenn ein Web-App-Endpunkt als Azure-Endpunkt hinzugefügt wird, wird der DNS-Name des Traffic Manager-Profils automatisch für die App registriert. Diese Registrierung wird automatisch entfernt, wenn der Endpunkt gelöscht wird.
3. Jedes Traffic Manager-Profil kann maximal einen Web-App-Endpunkt aus jeder Azure-Region aufweisen. Zur Umgehung dieser Einschränkung können Sie eine Web-App als externen Endpunkt konfigurieren. Weitere Informationen finden Sie in den [häufig gestellten Fragen](#faq).

## <a name="enabling-and-disabling-endpoints"></a>Aktivieren und Deaktivieren von Endpunkten

Das Deaktivieren eines Endpunkts in Traffic Manager kann sehr hilfreich sein, um Datenverkehr vorübergehend von einem Endpunkt zu entfernen, der sich im Wartungsmodus befindet oder der erneut bereitgestellt wird. Sobald der Endpunkt wieder betriebsbereit ist, kann er erneut aktiviert werden.

Endpunkte können per Traffic Manager-Portal, PowerShell, CLI oder REST-API aktiviert und deaktiviert werden, die im Resource Manager und dem klassischen Bereitstellungsmodell unterstützt werden.

> [!NOTE]
> Das Deaktivieren eines Azure-Endpunkts hat nichts mit dessen Bereitstellungsstatus in Azure zu tun. Ein Azure-Dienst (z.B. eine VM oder eine Webanwendung) wird weiterhin ausgeführt und kann auch dann Datenverkehr empfangen, wenn er im Traffic Manager deaktiviert ist. Datenverkehr kann nicht über den Traffic Manager-Profil DNS-Namen, sondern direkt an die Dienstinstanz adressiert werden. Weitere Informationen finden Sie unter [Funktionsweise von Traffic Manager](traffic-manager-how-traffic-manager-works.md).

Die aktuelle Berechtigung jedes Endpunkts zum Empfangen von Datenverkehr hängt von den folgenden Faktoren ab:

* Profilstatus (aktiviert/deaktiviert)
* Endpunktstatus (aktiviert/deaktiviert)
* Ergebnisse der Integritätsüberprüfungen für diesen Endpunkt

Weitere Informationen finden Sie unter [Traffic Manager-Endpunktüberwachung](traffic-manager-monitoring.md#endpoint-and-profile-status).

> [!NOTE]
> Da der Traffic Manager auf DNS-Ebene verwendet wird, kann er vorhandene Verbindungen mit Endpunkten nicht beeinflussen. Wenn ein Endpunkt nicht verfügbar ist, leitet Traffic Manager neue Verbindungen zu einem anderen verfügbaren Endpunkt. Allerdings kann der Host hinter dem deaktivierten oder fehlerhaften Endpunkt unter Umständen weiterhin Datenverkehr über vorhandene Verbindungen empfangen, bis diese Sitzungen beendet werden. Anwendungen sollten die Sitzungsdauer beschränken, damit der Datenverkehr von bestehenden Verbindungen ausgeglichen werden kann.

Wenn alle Endpunkte eines Profils deaktiviert werden oder das Profil selbst deaktiviert wird, beantwortet Traffic Manager DNS-Abfragen mit einer „NXDOMAIN“-Antwort.

## <a name="faq"></a>Häufig gestellte Fragen

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

## <a name="next-steps"></a>Nächste Schritte

* Informationen zur [Funktionsweise von Traffic Manager](traffic-manager-how-traffic-manager-works.md)
* Informationen zu [Endpunktüberwachung und automatisches Failover](traffic-manager-monitoring.md)von Traffic Manager
* Informationen zu Traffic Manager- [Routingmethoden für Datenverkehr](traffic-manager-routing-methods.md)




<!--HONumber=Nov16_HO3-->


