---
title: Aspekte der Netzwerktopologie bei Verwendung des Azure Active Directory-Anwendungsproxys | Microsoft-Dokumentation
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
ms.date: 03/22/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 6869453e0776405841890978eef97f549be97541
ms.lasthandoff: 04/03/2017


---

# <a name="network-topology-considerations-when-using-azure-active-directory-application-proxy"></a>Aspekte der Netzwerktopologie bei Verwendung des Azure Active Directory-Anwendungsproxys

In diesem Artikel geht es darum, welche Aspekte der Netzwerktopologie wichtig sind, wenn der Azure Active Directory-Anwendungsproxy (Azure AD) für die Veröffentlichung und den Zugriff auf Ihre Anwendungen per Remotezugriff verwendet wird.

## <a name="traffic-flow"></a>Datenverkehrsfluss

Wenn eine Anwendung über den Azure AD-Anwendungsproxy veröffentlicht wird, fließt der gesamte Datenverkehr von den Benutzern zu den Back-End-Zielanwendungen über die folgenden Hops:

* Hop 1: Vom Benutzer zum öffentlichen Endpunkt des Azure AD-Anwendungsproxydiensts in Azure
* Hop 2: Vom Anwendungsproxydienst zum Connector
* Hop 3: Vom Connector zur Zielanwendung

 ![Diagramm mit dem Datenverkehrsfluss vom Benutzer zur Zielanwendung](./media/application-proxy-network-topologies/application-proxy-three-hops.png)

## <a name="tenant-location-and-application-proxy-service"></a>Mandantenstandort und Anwendungsproxydienst

Wenn Sie die Registrierung für einen Azure AD-Mandanten durchführen, wird die Region Ihres Mandanten anhand des von Ihnen angegebenen Lands ermittelt. Bei Aktivierung des Anwendungsproxys werden die Instanzen des Anwendungsproxydiensts für Ihren Mandanten in derselben Region wie Ihr Azure AD-Mandant bzw. in der nächstgelegenen Region angezeigt.

Wenn die Region Ihres Azure AD-Mandanten beispielsweise die Europäische Union (EU) ist, werden für alle Anwendungsproxyconnectors Dienstinstanzen in Azure-Datencentern in der EU verwendet. Dies bedeutet auch, dass der Weg für Ihre gesamten Benutzer über die Instanzen des Anwendungsproxydiensts an diesem Standort verläuft, wenn sie versuchen, auf veröffentlichte Anwendungen zuzugreifen.

## <a name="considerations-for-reducing-latency"></a>Reduzieren der Wartezeit

Für alle Proxylösungen kommt es in Bezug auf Ihre Netzwerkverbindungen zu Wartezeit. Unabhängig davon, welche Proxy- oder VPN-Lösung Sie als Lösung für den Remotezugriff wählen, ist immer eine Gruppe von Servern vorhanden, mit denen die Verbindung mit Ihrem Unternehmensnetzwerk ermöglicht wird.

Organisationen haben normalerweise Serverendpunkte in ihr Umkreisnetzwerk integriert. Bei Nutzung eines Azure AD-Anwendungsproxys ist aber kein Umkreisnetzwerk erforderlich. Dies liegt daran, dass der Datenverkehr über den Proxydienst in die Cloud fließt, während sich die Connectors in Ihrem Unternehmensnetzwerk befinden.

### <a name="connector-placement"></a>Platzierung von Connectors

Der Anwendungsproxy wählt den Standort der Instanzen basierend auf Ihrem Mandantenstandort für Sie aus. Daher können Sie entscheiden, wo der Connector installiert werden soll, und die Merkmale der Wartezeit für Ihren Netzwerkdatenverkehr definieren.

Beim Einrichten des Anwendungsproxydiensts sollten Sie die folgenden Fragen stellen:

* Wo befindet sich die App?
* Wo befinden sich die meisten Benutzer, die auf die App zugreifen?
* Wo befindet sich die Anwendungsproxyinstanz (basierend auf Ihrem Mandanten)?
* Verfügen Sie bereits über eine dedizierte Netzwerkverbindung mit Azure-Datencentern (z.B. Azure ExpressRoute oder ein ähnliches VPN)?

Die Anordnung des Connectors bestimmt die Wartezeit für die Hops 2 und 3 (im vorherigen Abschnitt beschrieben). Beachten Sie beim Auswerten der Platzierung des Connectors Folgendes:

* Der Connector benötigt eine „Sichtverbindung“ mit einem Datencenter. Auf diese Weise kann der Connector die Vorgänge für die eingeschränkte Kerberos-Delegierung (Kerberos Constrained Delegation, KCD) durchführen, wenn Sie das einmalige Anmelden (SSO) für Back-End-Anwendungen realisieren möchten.
* Der Connector wird meist in geringerer Entfernung von der Anwendung installiert, um die Zeit zu reduzieren, die für den Weg vom Connector zur Anwendung benötigt wird.

### <a name="general-approach-to-minimize-latency"></a>Allgemeiner Ansatz zum Verringern der Wartezeit

Sie können versuchen, die Wartezeit des End-to-End-Datenverkehrs zu verringern, indem Sie die einzelnen Netzwerkhops optimieren. Jeder Hop kann wie folgt optimiert werden:

* Reduzieren Sie den Abstand zwischen den beiden Enden des Hops.
* Wählen Sie das richtige zu durchlaufende Netzwerk. Wenn anstelle des öffentlichen Internet beispielsweise ein privates Netzwerk durchlaufen wird, kann dies aufgrund von dedizierten Links schneller gehen.

Wenn Sie für die Verbindung zwischen Azure und Ihrem Unternehmensnetzwerk über eine dedizierte VPN- oder ExpressRoute-Verbindung verfügen, können Sie diese hierfür nutzen.

## <a name="focus-your-optimizing-strategy"></a>Setzen von Schwerpunkten bei der Optimierungsstrategie

Da Ihre Benutzer auf Apps unter Umständen per Remoteverbindung über das Internet zugreifen, sollten Sie den Schwerpunkt immer auf die Optimierung der Hops 2 und 3 legen. Im Folgenden sind einige häufige Muster angegeben, die Sie nutzen können.

### <a name="pattern-1-optimize-hop-3"></a>Muster 1: Optimieren von Hop 3

Zum Optimieren von Hop 3 wird der Connector in geringer Entfernung von der Zielanwendung im Kundennetzwerk platziert. Dies ist ein Vorteil, weil für den Connector voraussichtlich eine „Sichtverbindung“ mit dem Domänencontroller erforderlich ist. Dieser Ansatz ist in den meisten Szenarien ausreichend. (Die meisten unserer Kunden halten sich an dieses Muster.)

 ![Diagramm zur Optimierung von Hop 3, Anordnung des Connectors in der Nähe der Zielanwendung](./media/application-proxy-network-topologies/application-proxy-hop3.png)


> [!NOTE]
Es gibt einige Szenarien, in denen Sie sowohl Hop 2 als auch Hop 3 optimieren müssen, um die gewünschte Wartezeit zu erhalten. Wenn Sie beispielsweise eine VPN- oder ExpressRoute-Verbindung zwischen Ihrem Netzwerk und dem Azure-Datencenter eingerichtet haben, können Sie beide Hops optimieren.

### <a name="pattern-2-take-advantage-of-expressroute-with-public-peering"></a>Muster 2: Nutzen von ExpressRoute mit öffentlichem Peering

Wenn Sie für ExpressRoute das öffentliche Peering eingerichtet haben, können Sie für Hop 2 die schnellere ExpressRoute-Verbindung verwenden. (Hop 3 wurde bereits optimiert, indem der Connector nicht weit entfernt von der App in Ihrem Netzwerk angeordnet wird.)

![Diagramm zur Optimierung von Hop 2, Verwendung einer ExpressRoute-Verbindung](./media/application-proxy-network-topologies/application-proxy-expressroute-public.png)

### <a name="pattern-3-take-advantage-of-expressroute-with-private-peering"></a>Muster 3: Nutzen von ExpressRoute mit privatem Peering

Wenn Sie über ein dediziertes VPN- oder ExpressRoute-Setup mit privatem Peering zwischen Azure und Ihrem Unternehmensnetzwerk verfügen, haben Sie eine weitere Option. Bei dieser Konfiguration wird das virtuelle Netzwerk in Azure normalerweise als Erweiterung des Unternehmensnetzwerks angesehen. Sie können den Connector also im Azure-Datencenter installieren und für die Verbindung vom Connector zur App trotzdem die Anforderungen an die Wartezeit für Hop 3 erfüllen.

Die Wartezeit wird nicht negativ beeinträchtigt, weil der Datenverkehr über eine dedizierte Verbindung fließt. Ein weiterer Vorteil bei diesem Ansatz ist, dass die Wartezeiteigenschaften für Hop 2 verbessert werden. Dies liegt daran, dass die Verbindung des Anwendungsproxydiensts mit dem Connector jetzt ein kürzerer Hop ist. Der Connector ist in einem Azure-Datencenter in kurzer Entfernung zum Standort Ihres Azure AD-Mandanten (und somit auch des Anwendungsproxys) installiert.

![Diagramm zur Installation des Connectors in einem Azure-Datencenter](./media/application-proxy-network-topologies/application-proxy-expressroute-private.png)

### <a name="other-approaches"></a>Andere Ansätze

Der Schwerpunkt dieses Artikels ist zwar die Anordnung des Connectors, aber Sie können die Anordnung der Anwendung auch ändern, um bessere Wartezeiteigenschaften zu erhalten.

Immer mehr Organisationen verschieben ihre Netzwerke in gehostete Umgebungen. Auf diese Weise können sie ihre Apps in einer gehosteten Umgebung anordnen, die gleichzeitig Teil ihres Unternehmensnetzwerks ist und sich noch innerhalb der Domäne befindet. In diesem Fall können die in den vorherigen Abschnitten beschriebenen Muster auf den neuen Anwendungsspeicherort angewendet werden.

Erwägen Sie die Verwendung von Connectorgruppen für Apps, die sich an unterschiedlichen Standorten und in unterschiedlichen Netzwerken befinden. Informationen zu diesem Ansatz finden Sie unter [Azure AD Domain Services](../active-directory-domain-services/active-directory-ds-overview.md).

## <a name="common-scenarios"></a>Gängige Szenarien

In diesem Abschnitt werden einige Anwendungsfälle beschrieben. Angenommen, der Azure AD-Mandant (und somit auch der Proxy-Dienstendpunkt) befindet sich in den USA. Die in diesen Anwendungsfällen beschriebenen Aspekte gelten normalerweise auch für andere Regionen weltweit.

### <a name="use-case-1"></a>Anwendungsfall 1

Die App befindet sich in den USA im Netzwerk einer Organisation mit Benutzern in derselben Region. Zwischen dem Azure-Datencenter und dem Unternehmensnetzwerk besteht keine ExpressRoute- oder VPN-Verbindung.

**Empfehlung:** Verwenden Sie Muster 1, das im vorherigen Abschnitt beschrieben wird. Erwägen Sie ggf. die Verwendung von ExpressRoute, um die Wartezeit zu verbessern.

Dies ist ein einfaches Muster. Sie optimieren Hop 3, indem Sie den Connector in der Nähe der App anordnen. Dies ist eine natürliche Wahl, da der Connector normalerweise mit einer „Sichtverbindung“ zur App und zum Datencenter installiert wird, um KCD-Vorgänge durchführen zu können.

![Diagramm mit Kontur der USA und Anordnung der Hops wie in diesem Anwendungsfall](./media/application-proxy-network-topologies/application-proxy-pattern1.png)

### <a name="use-case-2"></a>Anwendungsfall 2

Die App befindet sich in den USA im Netzwerk einer Organisation, und die Benutzer sind weltweit verteilt. Zwischen dem Azure-Datencenter und dem Unternehmensnetzwerk besteht keine ExpressRoute- oder VPN-Verbindung.

**Empfehlung:** Verwenden Sie Muster 2, das im vorherigen Abschnitt beschrieben wird. Erwägen Sie ggf. die Verwendung von ExpressRoute, um die Wartezeit zu verbessern.

Das am häufigsten verwendete Muster ist wieder die Optimierung von Hop 3, bei dem Sie den Connector in der Nähe der App anordnen. Hop 3 ist normalerweise nicht mit hohen Kosten verbunden, wenn sich alles in derselben Region abspielt. Die Kosten für Hop 1 können je nach Standort eines Benutzers dagegen höher sein, da alle Benutzer auf die Anwendungsproxyinstanz in den USA zugreifen. Hinweis: Alle Proxylösungen verfügen über ähnliche Merkmale, was die weltweite Verteilung von Benutzern betrifft.

![Diagramm mit Kontur der Kontinente und Anordnung der Hops wie in diesem Anwendungsfall](./media/application-proxy-network-topologies/application-proxy-pattern2.png)

### <a name="use-case-3"></a>Anwendungsfall 3

Die App befindet sich im Netzwerk einer Organisation in den USA. ExpressRoute mit öffentlichem Peering ist zwischen Azure und dem Unternehmensnetzwerk vorhanden.

**Empfehlung:** Platzieren Sie den Connector so nah wie möglich an der App. Für Hop 2 wird automatisch ExpressRoute verwendet. Dies basiert auf Muster 2, das im vorherigen Abschnitt beschrieben wurde.

Wenn für die ExpressRoute-Verbindung das öffentliche Peering verwendet wird, fließt der Datenverkehr zwischen dem Proxy und dem Connector über diese Verbindung. Hop 2 verfügt über eine optimierte Wartezeit.

![Diagramm mit Kontur der USA und Anordnung der Hops wie in diesem Anwendungsfall](./media/application-proxy-network-topologies/application-proxy-pattern3.png)

### <a name="use-case-4"></a>Anwendungsfall 4

Die App befindet sich im Netzwerk einer Organisation in den USA. ExpressRoute mit privatem Peering ist zwischen Azure und dem Unternehmensnetzwerk vorhanden.

**Empfehlung:** Platzieren Sie den Connector in dem Azure-Datencenter, das über das private ExpressRoute-Peering mit dem Unternehmensnetzwerk verbunden ist. Dies basiert auf Muster 3, das im vorherigen Abschnitt beschrieben wurde.

Der Connector kann im Azure-Datencenter angeordnet werden. Da der Connector weiterhin über das private Netzwerk über eine Sichtverbindung mit der Anwendung und dem Datencenter verfügt, bleibt Hop 3 optimiert. Darüber hinaus wird Hop 2 weiter optimiert.

![Diagramm mit Kontur der USA und Anordnung der Hops wie in diesem Anwendungsfall](./media/application-proxy-network-topologies/application-proxy-pattern4.png)

### <a name="use-case-5"></a>Anwendungsfall 5

Die App befindet sich in der EU im Netzwerk einer Organisation, und die meisten Benutzer befinden sich in den USA.

**Empfehlung:** Platzieren Sie den Connector in der Nähe der App. Da Benutzer in den USA auf eine Anwendungsproxyinstanz zugreifen, die sich in derselben Region befindet, sind die Kosten für Hop 1 nicht übermäßig hoch. Hop 3 ist optimiert. Aber Hop 2 ist bei diesem Anwendungsfall normalerweise mit höheren Kosten verbunden.

![Diagramm mit Kontur der Kontinente und Anordnung der Hops wie in diesem Anwendungsfall](./media/application-proxy-network-topologies/application-proxy-pattern5a.png)

Erwägen Sie die Verwendung von ExpressRoute, wie in den vorherigen Abschnitten zu den Mustern 2 und 3 beschrieben.

![Diagramm mit Kontur der Kontinente und Anordnung der Hops wie in diesem Anwendungsfall](./media/application-proxy-network-topologies/application-proxy-pattern5b.png)

In dieser Situation können Sie auch eine andere Variante verwenden. Wenn sich die meisten Benutzer der Organisation in den USA befinden, ist die Wahrscheinlichkeit hoch, dass auch Ihr Netzwerk bis in die USA reicht. In diesem Fall kann der Connector in den USA angeordnet und die dedizierte interne Unternehmensnetzwerkleitung zur Anwendung in der EU genutzt werden. Auf diese Weise werden die Hops 2 und 3 optimiert.

![Diagramm mit Kontur der Kontinente und Anordnung der Hops wie in diesem Anwendungsfall](./media/application-proxy-network-topologies/application-proxy-pattern5c.png)

## <a name="next-steps"></a>Nächste Schritte

- [Aktivieren des Anwendungsproxys](active-directory-application-proxy-enable.md)
- [Aktivieren der einmaligen Anmeldung](active-directory-application-proxy-sso-using-kcd.md)
- [Aktivieren des bedingten Zugriffs](active-directory-application-proxy-conditional-access.md)
- [Problembehandlung von Anwendungsproxys](active-directory-application-proxy-troubleshoot.md)

