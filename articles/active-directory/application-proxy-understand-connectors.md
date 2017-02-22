---
title: Grundlegendes zu Azure AD-Anwendungsproxyconnectors | Microsoft-Dokumentation
description: Hier finden Sie grundlegende Informationen zu Azure AD-Anwendungsproxyconnectors.
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
ms.sourcegitcommit: 36e737ebc3451a190e99dc2bc91ef4242d2f573e
ms.openlocfilehash: e9dfe8ad62dfa0eec810ecdeeddadbecc25b9163


---

# <a name="understand-azure-ad-application-proxy-connectors"></a>Grundlegendes zu Azure AD-Anwendungsproxyconnectors

In diesem Artikel werden Connectors beschrieben, die für den Azure AD-Anwendungsproxy eine wichtige Rolle spielen. Sie sind einfach aufgebaut, leicht bereitzustellen und zu verwalten und haben eine hohe Leistungsstärke.

> [!NOTE]
> Das Feature "Anwendungsproxy" ist nur verfügbar, wenn Sie Azure Active Directory auf die Premium oder Basic Edition aktualisiert haben. Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](active-directory-editions.md).
> 
> 

## <a name="what-are-azure-ad-application-proxy-connectors"></a>Was sind Azure AD-Anwendungsproxyconnectors?
Der Anwendungsproxy kann verwendet werden, nachdem Sie den Windows Server-Dienst (den „Connector“) in Ihrem Netzwerk installiert haben. Sie können Connectors basierend auf Ihren Anforderungen an die hohe Verfügbarkeit und Skalierbarkeit installieren. Beginnen Sie mit einem Connector, und fügen Sie dann bei Bedarf weitere hinzu. Bei jeder Installation eines Connectors wird dieser dem Pool mit den Connectors hinzugefügt, der für Ihren Mandanten verwendet wird.

Es wird empfohlen, die Connectors nicht auf den Anwendungsservern selbst zu installieren, auch wenn dies, vor allem für kleinere Bereitstellungen, möglich ist.

Es ist nicht erforderlich, Connectors zu löschen, die Sie nicht mehr verwenden. Wenn ein Connector ausgeführt wird, bleibt er beim Herstellen der Verbindung mit dem Dienst aktiv. Connectors, die nicht verwendet werden, werden als _inaktiv_ gekennzeichnet und nach zehn Tagen Inaktivität entfernt. 

Informationen zum Beheben von Azure AD-Konnektivitätsproblemen finden Sie unter [How to troubleshoot Azure AD Application Proxy connectivity problems](https://blogs.technet.microsoft.com/applicationproxyblog/2015/03/24/how-to-troubleshoot-azure-ad-application-proxy-connectivity-problems) (Behandeln von Problemen mit der Azure AD-Anwendungsproxykonnektivität). 

## <a name="what-are-the-cloud-rules-for-connectors"></a>Welche Cloudregeln gelten für Connectors?
Die Connectors und der Dienst führen alle Aufgaben in Bezug auf die hohe Verfügbarkeit aus. Diese können dynamisch hinzugefügt oder entfernt werden. Jedes Mal, wenn eine neue Anforderung eingeht, wird diese an einen der gerade verfügbaren Connectors geleitet. Falls ein Connector vorübergehend nicht verfügbar ist, reagiert er also nicht auf diesen Datenverkehr.

Die Connectors sind zustandslos und verfügen auf dem Computer nicht über Konfigurationsdaten. Die einzigen Ausnahmen sind die Konnektivität zu den Diensteinstellungen und das Zertifikat, mit dem dieser Connector authentifiziert wird. Wenn die Verbindung mit dem Dienst hergestellt wird, werden alle erforderlichen Konfigurationsdaten abgerufen und jeweils nach einigen Minuten aktualisiert.
Außerdem wird der Server abgefragt, um zu ermitteln, ob eine neuere Version des Connectors vorhanden ist. Wenn ja, führen die Connectors die Aktualisierung selbst durch.

Sie können Ihre Connectors über den Computer überwachen, auf dem sie ausgeführt werden, indem Sie entweder das Ereignisprotokoll und die Leistungsindikatoren verwenden, oder über die Cloud mithilfe der Seite „Connectorstatus“. Dies ist unten dargestellt.

 ![Azure AD-Anwendungsproxyconnectors](./media/application-proxy-understand-connectors/app-proxy-connectors.png)

## <a name="all-networking-is-outbound"></a>Alle Netzwerkverbindungen nur in ausgehender Richtung
Connectors senden nur ausgehende Anforderungen, sodass die Verbindung immer von den Connectors initiiert wird. Das Öffnen von eingehenden Ports ist nicht erforderlich, da der Datenverkehr nach dem Einrichten einer Sitzung in beide Richtungen fließt.

Der ausgehende Datenverkehr wird an den Anwendungsproxydienst und die veröffentlichten Anwendungen gesendet. Der Datenverkehr an den Dienst wird über mehrere unterschiedliche Portnummern an Azure-Datencenter gesendet. Ausführliche Informationen hierzu finden Sie unter [Aktivieren des Anwendungsproxys über das Azure-Portal](active-directory-application-proxy-enable.md).

Da es nur um ausgehenden Datenverkehr geht, muss kein Lastenausgleich zwischen den Connectors eingerichtet und auch kein Zugriff in eingehender Richtung über die Firewalls konfiguriert werden.

Informationen zur Konfiguration von Firewallregeln für ausgehenden Datenverkehr finden Sie unter [Work with existing on-premise Proxy servers](application-proxy-working-with-proxy-servers.md) (Verwenden von vorhandenen lokalen Proxyservern).

## <a name="network-security"></a>Netzwerksicherheit

Connectors können überall im Netzwerk installiert werden, sodass Anforderungen sowohl an den Dienst als auch an die Back-End-Anwendungen gesendet werden können. Sie funktionieren gut, wenn Sie sie im Unternehmensnetzwerk innerhalb einer DMZ (demilitarisierte Zone) oder sogar auf einem virtuellen Computer installieren, der in der Cloud mit Zugriff auf die Apps ausgeführt wird.

DMZ-Bereitstellungen sind normalerweise komplizierter. Einer der Gründe für die Bereitstellung von Connectors in der DMZ ist die Verwendung einer anderen Infrastruktur, die für darin ausgeführte Komponenten verfügbar ist, z.B. Lastenausgleichsmodule für Back-End-Anwendungen bzw. Sicherheitskontrollen wie Angriffserkennungssysteme.

## <a name="domain-joining"></a>Domänenbeitritt

Connectors können auf einem Computer ausgeführt werden, der nicht in eine Domäne eingebunden ist. Ein in eine Domäne eingebundener Computer ist aber erforderlich, wenn Sie sich für die Verwendung des einmaligen Anmeldens (SSO) für Anwendungen entscheiden, für die die Integrierte Windows-Authentifizierung (IWA) genutzt wird. 

In diesem Fall müssen die Connectorcomputer in eine Domäne eingebunden werden, für die die eingeschränkte [Kerberos](https://web.mit.edu/kerberos)-Delegierung im Namen der relevanten Benutzer für die veröffentlichten Anwendungen durchgeführt werden kann.

Außerdem können Connectors auch Domänen oder Gesamtstrukturen mit einer Teilvertrauensstellung oder schreibgeschützten Domänencontrollern (Read-Only Domain Controllers, RODC) beitreten.

## <a name="connectors-on-hardened-environments"></a>Connectors in festgeschriebenen Umgebungen

In den meisten Fällen ist die Connectorbereitstellung sehr unkompliziert und benötigt keine spezielle Konfiguration. Es sollten aber mehrere besondere Bedingungen berücksichtigt werden:

* Organisationen, in denen der ausgehende Datenverkehr eingeschränkt ist, müssen die hier angegebene Anleitung zum Öffnen der erforderlichen Ports befolgen.
* Unter Umständen sind FIPS-konforme Computer zum Ändern der Konfiguration erforderlich, damit vom Connectordienst, Connectorupdatedienst und Installationsprogramm ein Zertifikat auf diesem Computer generiert und gespeichert werden kann.
* Organisationen mit einer Sperrung der Umgebung basierend auf den Prozessen, von denen die Netzwerkanforderungen ausgegeben werden, müssen Folgendes sicherstellen: Für die Connectordienste ist der Zugriff auf alle erforderlichen Ports und IPs zugelassen.
* In einigen Fällen kann es sein, dass Proxys für die ausgehende Weiterleitung die zweistufige Authentifizierung per Zertifikat verhindern und so bewirken, dass die Kommunikation nicht erfolgreich ist.

## <a name="all-connectors-are-created-almost-equal"></a>Alle Connectors sind nahezu gleich

Es wird vorausgesetzt, dass alle Connectors miteinander identisch sind und dass jede eingehende Anforderung auf jedem Connector eingehen kann. Dies bedeutet, dass alle Connectors über die gleiche Netzwerkkonnektivität und die gleichen [Kerberos](https://web.mit.edu/kerberos)-Einstellungen verfügen sollten.

Die gesamte Kommunikation vom Connector zum Dienst ist durch ein Clientzertifikat geschützt, das ausgestellt und dann auf dem Connectorcomputer installiert wird. Informationen zur Erneuerung von Connectorzertifikaten finden Sie unter [Aktivieren des Anwendungsproxys über das Azure-Portal](active-directory-application-proxy-enable.md).

## <a name="connector-authentication"></a>Connectorauthentifizierung

Zur Bereitstellung eines sicheren Diensts müssen sich Connectors gegenüber dem Dienst authentifizieren, und auch der Dienst muss sich gegenüber dem Connector authentifizieren. Hierfür werden Client- und Serverzertifikate verwendet, wenn die Connectors die Verbindung initiieren. So wird erreicht, dass der Benutzername und das Kennwort des Administrator nicht auf dem Connectorcomputer gespeichert werden.

Die verwendeten Zertifikate gelten nur für den Anwendungsproxydienst. Sie werden während der ersten Registrierung erstellt und von den Connectors nach einigen Monaten automatisch erneuert. 

Wenn ein Connector über mehrere Monate hinweg keine Verbindung mit dem Dienst herstellt, verfügt er unter Umständen über veraltete Zertifikate. In diesem Fall ist eine Registrierung erforderlich, und Sie müssen den Connector deinstallieren und neu installieren, um die Registrierung auszulösen. Sie können die folgenden PowerShell-Befehle ausführen:

```
* Import-module AppProxyPSModule
* Register-AppProxyConnector
```

## <a name="performance-and-scalability"></a>Leistung und Skalierbarkeit

Auch wenn die Skalierung für den Onlinedienst transparent ist, ist die Skalierung in Bezug auf Connectors ein wichtiger Faktor. Sie müssen über eine ausreichende Zahl von Connectors verfügen, um Spitzenlasten verarbeiten zu können. Da Connectors zustandslos sind, sind sie nicht von der Anzahl von Benutzern oder Sitzungen abhängig. Stattdessen sind sie von der Anzahl von Anforderungen und deren Nutzlastgröße abhängig. Für den Standard-Webdatenverkehr wurde gezeigt, dass ein Computer mit durchschnittlicher Leistungsfähigkeit einige Tausend Anforderungen pro Sekunde verarbeiten kann. Dies richtet sich nach den genauen Eigenschaften des Computers.

Die Connectorleistung wird durch die CPU und das Netzwerk bestimmt. Eine gute CPU-Leistung wird für die SSL-Verschlüsselung und -Entschlüsselung benötigt, und die Netzwerkeigenschaften sind wichtig, um eine gute Konnektivität für die Anwendungen und den Onlinedienst in Azure zu erzielen. Der Arbeitsspeicher ist für Connectors dagegen weniger wichtig.

In Bezug auf den Connectordienst wird angestrebt, die Connectors möglichst stark zu entlasten. Der Onlinedienst übernimmt einen Großteil der Verarbeitung und den gesamten nicht authentifizierten Datenverkehr. Alle Schritte, die in der Cloud ausgeführt werden können, werden auch in der Cloud ausgeführt.

Ein weiterer Leistungsfaktor ist die Qualität der Netzwerkverbindung zwischen den Connectors, z.B.:

* _Onlinedienst:_ Wenn die Verbindung langsam ist oder eine lange Wartezeit aufweist. Wirkt sich auf den Servicelevel aus. Es ist am besten, wenn Ihre Organisation per ExpressRoute mit Azure verbunden ist. Achten Sie andernfalls darauf, dass vom Netzwerkteam sichergestellt wird, dass Verbindungen mit Azure auf effiziente Weise verarbeitet werden.

* _Back-End-Anwendungen:_ In einigen Fällen werden zwischen dem Connector und den Back-End-Anwendungen zusätzliche Proxys verwendet. Hierfür kann die Problembehandlung leicht durchgeführt werden, indem ein Browser auf dem Connectorcomputer geöffnet und auf diese Anwendungen zugegriffen wird. Wenn Sie die Connectors in Azure ausführen und die Anwendungen lokal vorliegen, ist die Benutzerfreundlichkeit für die Benutzer ggf. nicht so hoch wie erwartet.
* _Domänencontroller:_ Wenn die Connectors SSO per eingeschränkter Kerberos-Delegierung (KCD) durchführen, nehmen sie vor dem Senden der Anforderung an das Back-End Kontakt mit den Domänencontrollern auf. Die Connectors verfügen zwar über einen Cache mit Kerberos-Tickets, aber in Umgebungen mit hoher Auslastung kann es aufgrund einer verringerten Reaktionsfähigkeit der Domänencontroller zu einer Verlangsamung kommen. Dies ist häufiger für Connectors der Fall, die in Azure ausgeführt werden, während die Domänencontroller lokal vorliegen.

##<a name="automatic-updates-to-the-connector"></a>Automatische Updates des Connectors

Mit dem Connectorupdatedienst erhalten Sie eine automatisierte Möglichkeit, um alles auf dem neuesten Stand zu halten. So kommen Sie in den Genuss, immer über alle neuen Features und Sicherheits- und Leistungsverbesserungen zu verfügen.

Azure AD unterstützt automatische Updates für alle von Ihnen bereitgestellten Connectors. Solange der Anwendungsproxy-Connectorupdatedienst ausgeführt wird, werden Ihre Connectors automatisch ohne Ausfallzeit und ohne manuellen Eingriff aktualisiert. Falls der Connectorupdatedienst auf Ihrem Server nicht angezeigt wird, müssen Sie den Connector neu installieren, um Updates zu erhalten. Weitere Informationen zur Installation von Connectors finden Sie unter [Aktivieren des Anwendungsproxys über das Azure-Portal](https://azure.microsoft.com/en-us/documentation/articles/active-directory-application-proxy-enable.md).

### <a name="updater-impact"></a>Auswirkungen des Updatediensts

_Mandanten mit einem Connector:_ Wenn Sie nur einen Connector verwenden, wird dieser Connector als Teil der aktuellen Gruppe aktualisiert. Da kein anderer Connector vorhanden ist, über den der Datenverkehr umgeleitet werden kann, ist der Dienst während des Updatevorgangs nicht verfügbar. Wir empfehlen Ihnen, einen zweiten Connector zu installieren und eine Connectorgruppe zu erstellen, um diese Ausfallzeit zu vermeiden und die hohe Verfügbarkeit leichter sicherstellen zu können. Informationen zur Vorgehensweise finden Sie in der [Dokumentation zu Connectorgruppen](https://azure.microsoft.com/en-us/documentation/articlesactive-directory-application-proxy-connectors.md).

_Andere Mandanten:_ Während des Connectorupdates wird der Datenverkehr an Ihre anderen Connectors umgeleitet, um die Störungen möglichst gering zu halten. Alle Transaktionen, die beim Starten des Updatevorgangs in Bearbeitung sind, werden aber ggf. verworfen. Ihr Browser sollte automatisch versuchen, den Vorgang zu wiederholen, um dieses mögliche Verwerfen für Sie transparent zu machen. Andernfalls müssen Sie Ihre Seite ggf. aktualisieren.

Uns ist bewusst, dass auch eine einzige Minute Ausfallzeit sehr problematisch sein kann, aber mit diesen Updates können wir Ihnen einen Connector mit vielen Verbesserungen bereitstellen, sodass dieser Vorgang unserer Meinung nach für Sie vorteilhaft ist.

Weitere Informationen zu den Änderungen unseres letzten Connectorupdates finden Sie auf der [Seite zum letzten Update](https://azure.microsoft.com/en-us/updates/app-proxy-connector-12sept2016). Wir überarbeiten die Seite bei jedem Update.

## <a name="under-the-hood"></a>Im Hintergrund

Wir stellen in Azure viele nützliche Tools für Sie bereit. Vor allem Connectors verfügen über viele nützliche Funktionen. Da Connectors auf dem Windows Server-Webanwendungsproxy basieren, haben sie Zugriff auf die meisten dazugehörigen Verwaltungstools, z.B. die umfassende Gruppe von Windows-Ereignisprotokollen und Windows-Leistungsindikatoren. Dies ist unten für die Ereignisanzeige dargestellt:

 ![Azure AD-Ereignisanzeige](./media/application-proxy-understand-connectors/event-view-window.png)

Systemmonitor:

 ![Azure AD-Systemmonitor](./media/application-proxy-understand-connectors/performance-monitor.png)

Die Connectors verfügen über Administrator- und Sitzungsprotokolle. Die Administratorprotokolle enthalten wichtige Ereignisse und die dazugehörigen Fehler. Die Sitzungsprotokolle enthalten alle Transaktionen und die dazugehörigen Verarbeitungsdetails. 

Zum Anzeigen müssen Sie im Menü „Ansicht“ der Ereignisanzeige die Option „Analytische und Debugprotokolle einblenden“ aktivieren. Anschließend müssen Sie sie aktivieren, um mit dem Erfassen von Ereignissen zu beginnen. Diese Protokolle werden nicht im Webanwendungsproxy in Windows Server 2012 R2 angezeigt, da die Connectors auf einer neueren Version basieren.

 ![Sitzung der Azure AD-Ereignisanzeige](./media/application-proxy-understand-connectors/event-view-window-session.png)

Sie können den Zustand des Diensts im Fenster „Dienste“ untersuchen. Der Connector umfasst zwei Windows-Dienste: den eigentlichen Connector und einen zusätzlichen Dienst, der für das Update zuständig ist. Beide Dienste müssen jederzeit ausgeführt werden.

 ![Lokale Azure AD-Dienste](./media/application-proxy-understand-connectors/aad-connector-services.png)

Informationen zum Beheben von Fehlern in Bezug auf den Anwendungsproxyconnector finden Sie unter [Problembehandlung von Anwendungsproxys](https://azure.microsoft.com/en-us/documentation/articles/active-directory-application-proxy-troubleshoot).

##<a name="next-steps"></a>Nächste Schritte
[Verwenden von vorhandenen lokalen Proxyservern](application-proxy-working-with-proxy-servers.md)<br>
[Installieren des Azure AD-Anwendungsproxyconnectors im Hintergrund](active-directory-application-proxy-silent-installation.md)




<!--HONumber=Feb17_HO1-->


