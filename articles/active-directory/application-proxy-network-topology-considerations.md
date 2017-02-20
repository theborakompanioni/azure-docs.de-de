---
title: Aspekte der Netzwerktopologie bei Verwendung des Azure AD-Anwendungsproxys | Microsoft-Dokumentation
description: Es werden die Aspekte der Netzwerktopologie beschrieben, die bei Verwendung des Azure AD-Anwendungsproxys wichtig sind.
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/12/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 1fdc00865f31352026a64c2769d9d65047bb2fa6
ms.openlocfilehash: 26ec9167f1f883c3c71947a55142020fb48e206a


---

# <a name="network-topology-considerations-when-using-azure-ad-application-proxy"></a>Aspekte der Netzwerktopologie bei Verwendung des Azure AD-Anwendungsproxys
> [!NOTE]
> Das Feature "Anwendungsproxy" ist nur verfügbar, wenn Sie Azure Active Directory auf die Premium oder Basic Edition aktualisiert haben. Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](active-directory-editions.md).
> 

In diesem Artikel geht es darum, welche Aspekte der Netzwerktopologie wichtig sind, wenn der Azure AD-Anwendungsproxy für die Veröffentlichung und den Zugriff auf Ihre Anwendungen per Remotezugriff verwendet wird. 

## <a name="traffic-flow"></a>Datenverkehrsfluss

Wenn eine Anwendung über den Azure AD-Anwendungsproxy veröffentlicht wird, fließt der gesamte Datenverkehr von den Benutzern zu den Back-End-Zielanwendungen über die folgenden Hops:

* Hop 1: Vom Benutzer zum öffentlichen Endpunkt des Azure AD-Anwendungsproxydiensts in Azure
* Hop 2: Vom Anwendungsproxydienst zum Connector
* Hop 3: Vom Connector zur Zielanwendung

 ![Azure AD IaaS – Mehrere Cloudanbieter](./media/application-proxy-network-topologies/application-proxy-three-hops.png)

## <a name="tenant-location-and-app-proxy-service"></a>Mandantenstandort und Anwendungsproxydienst

Wenn Sie die Registrierung für einen Azure AD-Mandanten durchführen, wird die Region Ihres Mandanten (USA, EMEA, APAC usw.) anhand des von Ihnen angegebenen Lands ermittelt. Bei Aktivierung des Anwendungsproxys werden die Instanzen des Anwendungsproxydiensts für Ihren Mandanten in derselben Region wie Ihr Azure AD-Mandant bzw. in der nächstgelegenen Region angezeigt. 

Wenn die Region Ihres Azure AD-Mandanten beispielsweise die Europäische Union (EU) ist, werden für Ihre gesamten Azure AD-Anwendungsproxyconnectors Verbindungen mit den Instanzen des Anwendungsproxydiensts in Azure-Rechenzentren in der EU hergestellt. Dies bedeutet auch, dass der Weg für Ihre gesamten Benutzer über die Instanzen des Anwendungsproxydiensts an diesem Standort verläuft, wenn sie versuchen, auf veröffentlichte Anwendungen zuzugreifen.

## <a name="considerations-for-reducing-latency"></a>Reduzieren der Wartezeit

Für alle Proxylösungen kommt es in Bezug auf Ihre Netzwerkverbindungen zu Wartezeit. Unabhängig davon, welche Proxy- oder VPN-Lösung Sie als Lösung für den Remotezugriff wählen, ist immer eine Gruppe von Servern vorhanden, mit denen die Verbindung mit Ihrem Unternehmensnetzwerk ermöglicht wird. 

Unternehmen verfügen in Ihrer DMZ (demilitarisierte Zone) normalerweise über Serverendpunkte. Beim Azure AD-Anwendungsproxy ist aber keine DMZ erforderlich.  Dies liegt daran, dass der Anwendungsproxy-Datenverkehr über den Proxydienst in die Cloud fließt, während sich die Connectors in Ihrem Unternehmensnetzwerk befinden.

### <a name="connector-placement"></a>Platzierung von Connectors

Der Anwendungsproxydienst wählt den Standort Ihrer Instanzen basierend auf Ihrem Mandantenstandort für Sie aus. Daher können Sie entscheiden, wo der Connector installiert werden soll, und die Merkmale der Wartezeit für Ihren Netzwerkdatenverkehr umfassend definieren.

Beim Einrichten des Anwendungsproxydiensts sollten Sie beispielsweise die folgenden Fragen stellen und beantworten:

* Wo befindet sich die App?
* Wo befinden sich die meisten Benutzer, die auf die App zugreifen?
* Wo befindet sich die Anwendungsproxyinstanz (basierend auf Ihrem Mandanten)?
* Verfügen Sie bereits über eine dedizierte Netzwerkverbindung mit Azure-Datencentern (z.B. ExpressRoute oder ein ähnliches VPN-Setup)?

Die Wartezeit für Hop 2 und Hop 3 richtet sich nach der Platzierung des Connectors. Beachten Sie beim Auswerten der Platzierung des Connectors Folgendes:

* Der Connector benötigt eine „Sichtverbindung“ mit einem Rechenzentrum, um die Vorgänge für die eingeschränkte Kerberos-Delegierung (Kerberos Constrained Delegation, KCD) durchzuführen, wenn Sie das einmalige Anmelden (SSO) für Back-End-Anwendungen realisieren möchten.
* Der Connector wird meist in geringerer Entfernung von der Anwendung installiert, um die Zeit zu reduzieren, die für den Weg vom Connector zur Anwendung benötigt wird.

### <a name="general-approach-to-minimize-latency"></a>Allgemeiner Ansatz zum Verringern der Wartezeit

Sie können versuchen, die Wartezeit des End-to-End-Datenverkehrs zu verringern, indem Sie die einzelnen Netzwerkhops optimieren, damit der Datenverkehr besser fließt.

Jeder Hop kann wie folgt optimiert werden:

* Reduzieren Sie den Abstand zwischen den beiden Enden des Hops.
* Wählen Sie das richtige zu durchlaufende Netzwerk. Wenn anstelle des öffentlichen Internet beispielsweise ein privates Netzwerk durchlaufen wird, kann dies aufgrund von dedizierten Links schneller gehen.
 
Wenn Sie für die Verbindung zwischen Azure und Ihrem Unternehmensnetzwerk über eine dedizierte VPN/ExpressRoute-Verknüpfung verfügen, können Sie diese hierfür nutzen.

## <a name="focus-your-optimizing-strategy"></a>Setzen von Schwerpunkten bei der Optimierungsstrategie

Da Ihre Benutzer auf Apps unter Umständen per Remoteverbindung über das Internet zugreifen, sollten Sie den Schwerpunkt immer auf die Optimierung der Hops 2 und 3 legen. Unten sind einige häufige Muster angegeben, die Sie nutzen können.

### <a name="pattern-1-optimize-hop-3"></a>Muster 1: Optimieren von Hop 3:

Zum Optimieren von Hop 3 wird der Connector in geringer Entfernung von der Zielanwendung im Kundennetzwerk platziert. Die ist ein Vorteil, weil für den Connector voraussichtlich eine „Sichtverbindung“ mit dem Domänencontroller erforderlich ist (wie oben erwähnt). Dieser Ansatz ist normalerweise für die meisten Kunden und Szenarien ausreichend. Die meisten unserer Kunden halten sich an dieses Muster.

 ![Azure AD IaaS – Mehrere Cloudanbieter](./media/application-proxy-network-topologies/application-proxy-hop3.png)


> [!NOTE]
Es gibt einige Szenarien, in denen Sie sowohl Hop 2 als auch Hop 3 optimieren müssen, um die gewünschte Wartezeit zu erhalten. Wenn Sie beispielsweise ein VPN- oder ExpressRoute-Setup zwischen Ihrem Netzwerk und dem Azure-Datencenter verwenden, können Sie in diesem Szenario Hop 2 zusätzlich zu Hop 3 optimieren.
>

### <a name="pattern-2-take-advantage-of-expressroute-with-public-peering"></a>Muster 2: Nutzen von ExpressRoute mit öffentlichem Peering

Wenn Sie über ein ExpressRoute-Setup mit öffentlichem Peering verfügen, nutzen wir die schnellere ExpressRoute-Verbindung für Hop 2. Hop 3 wurde bereits optimiert, indem der Connector nicht weit entfernt von der App im Kundennetzwerk platziert wird.

![Azure AD IaaS – Mehrere Cloudanbieter](./media/application-proxy-network-topologies/application-proxy-expressroute-public.png)

### <a name="pattern-3-taking-advantage-expressroute-with-private-peering"></a>Muster 3: Nutzen von ExpressRoute mit privatem Peering

Wenn Sie über ein dediziertes VPN- oder ExpressRoute-Setup mit privatem Peering zwischen Azure und Ihrem Unternehmensnetzwerk verfügen, in dem die App installiert ist, haben Sie eine weitere Option. Bei dieser Konfiguration wird das virtuelle Netzwerk in Azure normalerweise als Erweiterung des Unternehmensnetzwerks angesehen. Sie können den Connector also im Azure-Datencenter installieren und für die Verbindung vom Connector zur App trotzdem die Anforderungen an die Wartezeit für Hop 3 erfüllen. 

Die Wartezeit wird nicht negativ beeinträchtigt, weil der Datenverkehr über eine dedizierte Verbindung fließt. Ein Vorteil bei diesem Ansatz ist aber, dass die Wartezeiteigenschaften für Hop 2 verbessert werden. Dies liegt daran, dass der Hop für die Verbindung vom Proxydienst zum Connector (Hop 2) kürzer ist, da der Connector in einem Azure-Datencenter in der Nähe Ihres AAD-Mandanten (und somit auch des Anwendungsproxys) installiert ist.

![Azure AD IaaS – Mehrere Cloudanbieter](./media/application-proxy-network-topologies/application-proxy-expressroute-private.png)

### <a name="other-approaches"></a>Andere Ansätze

Bisher ging es in diesem Artikel vor allem um die Platzierung von Connectors. Falls das Verschieben der Anwendung für Sie eine Option ist (z.B. in Azure oder eine andere gehostete Umgebung), können Sie die Platzierung der Anwendung ändern, um bessere Wartezeiteigenschaften zu erzielen. 

Immer mehr Organisationen verschieben ihre Netzwerke in gehostete Umgebungen. Auf diese Weise können sie ihre Apps in der gehosteten Umgebung platzieren, die gleichzeitig Teil ihres Unternehmensnetzwerks ist und sich noch innerhalb der Domäne befindet. In diesem Fall können die obigen Muster auf den neuen Anwendungsstandort angewendet werden.

Erwägen Sie die Verwendung von Connectorgruppen für Apps, die sich an unterschiedlichen Standorten und in unterschiedlichen Netzwerken befinden. Informationen zu diesem Ansatz finden Sie unter [Azure AD Domain Services](https://azure.microsoft.com/en-us/services/active-directory-ds). 

## <a name="common-scenarios"></a>Gängige Szenarien

In diesem Abschnitt werden einige Anwendungsfälle beschrieben. Sie können für alle folgenden Anwendungsfälle voraussetzen, dass sich der Azure AD-Mandant (und somit auch der Proxy-Dienstendpunkt) in den USA befindet. Für andere Regionen weltweit gelten normalerweise die gleichen Überlegungen.

### <a name="use-case-1"></a>Anwendungsfall 1

Die App befindet sich in einem Kundennetzwerk in den USA und verfügt über Benutzer in derselben Region. Zwischen dem Azure-Domänencontroller und dem Unternehmensnetzwerk besteht keine ExpressRoute- oder VPN-Verbindung.

**Empfehlung:** Halten Sie sich an den oben beschriebenen Anwendungsfall 1. Erwägen Sie zur Verbesserung der Wartezeit, falls erforderlich, die Verwendung von ExpressRoute (siehe Anwendungsfall 3 und 4).

Dies ist ein einfaches Muster. Das am häufigsten verwendete Muster ist die Optimierung von Hop 3, bei dem der Connector in der Nähe der App platziert wird. Dies ist eine natürliche Wahl, da der Connector normalerweise mit einer „Sichtverbindung“ zur App und zum Domänencontroller installiert wird, um KCD-Vorgänge durchführen zu können.
Für diesen Anwendungsfall wird das unten beschriebene Muster 1 verwendet.

![Azure AD IaaS – Mehrere Cloudanbieter](./media/application-proxy-network-topologies/application-proxy-pattern1.png)

### <a name="use-case-2"></a>Anwendungsfall 2

Die App befindet sich in einem Kundennetzwerk in den USA, und die Benutzer sind weltweit verteilt. Zwischen dem Azure-Domänencontroller und dem Unternehmensnetzwerk besteht keine ExpressRoute- oder VPN-Verbindung.

**Empfehlung:** Halten Sie sich an den oben beschriebenen Anwendungsfall 2. Erwägen Sie zur Verbesserung der Wartezeit, falls erforderlich, die Verwendung von ExpressRoute (siehe Anwendungsfall 3 und 4).

Das übliche Muster ist wieder die Optimierung von Hop 3, wobei der Connector aus den oben beschriebenen Gründen in der Nähe der App platziert wird. Hop 3 ist normalerweise nicht mit hohen Kosten verbunden, wenn sich alles in derselben Region abspielt. Die Kosten für Hop 1 können je nach Standort eines Benutzers dagegen höher sein, da alle Benutzer auf die Anwendungsproxyinstanz in den USA zugreifen. Beachten Sie, dass alle Proxylösungen hierbei in Bezug auf die weltweite Verteilung der Benutzer über ähnliche Eigenschaften verfügen.

Für diesen Anwendungsfall wird das unten beschriebene Muster 2 verwendet.

![Azure AD IaaS – Mehrere Cloudanbieter](./media/application-proxy-network-topologies/application-proxy-pattern2.png)

### <a name="use-case-3"></a>Anwendungsfall 3

Die App befindet sich in einem Kundennetzwerk in den USA. ExpressRoute mit öffentlichem Peering ist zwischen Azure und dem Unternehmensnetzwerk vorhanden.

**Empfehlung:** Platzieren Sie den Connector so nah wie möglich an der App. Für Hop 2 wird automatisch ExpressRoute verwendet. Dies ist die oben beschriebene Vorgehensweise für Muster 2.

Wenn für die ExpressRoute-Verknüpfung das öffentliche Peering verwendet wird, fließt der Datenverkehr zwischen dem Proxy und dem Connector über diesen Link, und die Wartezeit für Hop 2 wird optimiert.

Für diesen Anwendungsfall wird das unten beschriebene Muster 3 verwendet.

![Azure AD IaaS – Mehrere Cloudanbieter](./media/application-proxy-network-topologies/application-proxy-pattern3.png)

### <a name="use-case-4"></a>Anwendungsfall 4

Die App befindet sich in einem Kundennetzwerk in den USA. ExpressRoute mit privatem Peering ist zwischen Azure und dem Unternehmensnetzwerk vorhanden.

**Empfehlung:** Platzieren Sie den Connector auf dem Azure-Domänencontroller, der über das private ExpressRoute-Peering mit dem Unternehmensnetzwerk verbunden ist. Dies ist die oben beschriebene Vorgehensweise für Muster 3.

Der Connector kann auf dem Azure-Domänencontroller platziert werden. Da er weiterhin über eine Sichtverbindung mit der Anwendung und dem Domänencontroller über das private Netzwerk verfügt, bleibt Hop 3 optimiert. Dieses Setup bewirkt aber zusätzlich eine weitere Optimierung von Hop 2.

Für diesen Anwendungsfall wird das unten beschriebene Muster 4 verwendet.

![Azure AD IaaS – Mehrere Cloudanbieter](./media/application-proxy-network-topologies/application-proxy-pattern4.png)

### <a name="use-case-5"></a>Anwendungsfall 5

Die App befindet sich in einem Kundennetzwerk in der EU, und die meisten Benutzer befinden sich in den USA.

**Empfehlung:** Platzieren Sie den Connector in der Nähe der App. Aus den oben beschriebenen Gründen ist dies die beste Wahl. Da Benutzer in den USA auf eine Anwendungsproxyinstanz zugreifen, die sich in derselben Region befindet, sind die Kosten für Hop 1 nicht übermäßig hoch. Hop 3 ist optimiert. Aber Hop 2 ist bei diesem Anwendungsfall normalerweise mit höheren Kosten verbunden.

Für diesen Anwendungsfall wird das unten beschriebene Muster 5a verwendet.

![Azure AD IaaS – Mehrere Cloudanbieter](./media/application-proxy-network-topologies/application-proxy-pattern5a.png)

Erwägen Sie die Nutzung von ExpressRoute, wie oben unter den Mustern 2 und 3 beschrieben. Unten ist dargestellt, wie Muster 2 angewendet wird.

Für diesen Anwendungsfall wird das unten beschriebene Muster 5b verwendet.

![Azure AD IaaS – Mehrere Cloudanbieter](./media/application-proxy-network-topologies/application-proxy-pattern5b.png)

Unten ist noch eine andere mögliche Variante für diesen Anwendungsfall angegeben.

Wenn sich die meisten Benutzer der Organisation in den USA befinden, ist die Wahrscheinlichkeit hoch, dass auch Ihr Netzwerk bis in die USA reicht. In diesem Fall kann der Connector in den USA platziert und die dedizierte interne Unternehmensnetzwerkleitung zur Anwendung in der EU genutzt werden. Auf diese Weise werden Hop 2 und Hop 3 optimiert.

Für diesen Anwendungsfall wird das unten beschriebene Muster 5c verwendet.

![Azure AD IaaS – Mehrere Cloudanbieter](./media/application-proxy-network-topologies/application-proxy-pattern5c.png)

## <a name="next-steps"></a>Nächste Schritte
[Aktivieren des Anwendungsproxys](active-directory-application-proxy-enable.md)<br>
[Aktivieren der einmaligen Anmeldung](active-directory-application-proxy-sso-using-kcd.md)<br>
[Aktivieren des bedingten Zugriffs](active-directory-application-proxy-conditional-access.md)<br>
[Problembehandlung von Anwendungsproxys](active-directory-application-proxy-troubleshoot.md)





<!--HONumber=Feb17_HO1-->


