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
ms.date: 06/02/2017
ms.author: kgremban
ms.translationtype: HT
ms.sourcegitcommit: f76de4efe3d4328a37f86f986287092c808ea537
ms.openlocfilehash: 0c9adf369dfecc25d1bf3dc8a77cb3778e867af5
ms.contentlocale: de-de
ms.lasthandoff: 07/10/2017


---

# <a name="understand-azure-ad-application-proxy-connectors"></a>Grundlegendes zu Azure AD-Anwendungsproxyconnectors

Connectors sind die Komponenten, die den Azure AD-Anwendungsproxy erst möglich machen. Sie sind einfach aufgebaut, leicht bereitzustellen und zu verwalten und haben eine hohe Leistungsstärke. Dieser Artikel erläutert, was Connectors sind und wie sie funktionieren und stellt einige bewährte Methoden vor, damit Sie optimal von Ihrer Bereitstellung profitieren können. 

## <a name="deployment"></a>Bereitstellung

Der Anwendungsproxy kann verwendet werden, nachdem Sie den Windows Server-Dienst (den „Connector“) in Ihrem Netzwerk installiert haben. Sie können mehrere Connectors basierend auf Ihren Anforderungen an eine hohe Verfügbarkeit und Skalierbarkeit installieren. Beginnen Sie mit einem Connector, und fügen Sie dann bei Bedarf weitere hinzu. Bei jeder Installation eines Connectors wird dieser dem Pool mit den Connectors hinzugefügt, der für Ihren Mandanten verwendet wird.

Es empfiehlt sich, die Connectors nicht auf den gleichen Servern zu installieren, die Ihre Anwendungen hosten. Allerdings müssen Sie auf die Anwendung auf dem Server zugreifen können, auf dem Sie den Connector installieren.


## <a name="maintenance"></a>Wartung 
Die Connectors und der Dienst führen alle Aufgaben in Bezug auf die hohe Verfügbarkeit aus. Diese können dynamisch hinzugefügt oder entfernt werden. Jedes Mal, wenn eine neue Anforderung eintrifft, wird diese an einen der gerade verfügbaren Connectors geleitet. Falls ein Connector vorübergehend nicht verfügbar ist, reagiert er nicht auf diesen Datenverkehr.

Die Connectors sind zustandslos und besitzen keine Konfigurationsdaten auf dem Computer. Die einzigen Ausnahmen sind Daten zur Verbindung mit den Diensteinstellungen und das Zertifikat, mit dem dieser Connector authentifiziert wird. Wenn die Verbindung mit dem Dienst hergestellt wird, werden alle erforderlichen Konfigurationsdaten abgerufen und jeweils nach einigen Minuten aktualisiert.
Außerdem wird der Server abgefragt, um zu ermitteln, ob eine neuere Version des Connectors vorhanden ist. Wenn ja, führen die Connectors die Aktualisierung selbst durch.

Sie können Ihre Connectors auf dem Computer überwachen, auf dem sie ausgeführt werden, indem Sie das Ereignisprotokoll und die Leistungsindikatoren verwenden. Alternativ dazu können Sie die Connectors auch im Azure-Portal auf der Seite „Anwendungsproxy“ überwachen.

 ![Azure AD-Anwendungsproxyconnectors](./media/application-proxy-understand-connectors/app-proxy-connectors.png)

Sie müssen nicht verwendete Connectors nicht manuell löschen. Wenn ein Connector ausgeführt wird, bleibt er beim Herstellen der Verbindung mit dem Dienst aktiv. Connectors, die nicht verwendet werden, werden als _inaktiv_ gekennzeichnet und nach zehn Tagen Inaktivität entfernt. 

## <a name="automatic-updates"></a>Automatische Aktualisierungen

Azure AD unterstützt automatische Updates für alle von Ihnen bereitgestellten Connectors. Solange der Connectorupdatedienst für den Anwendungsproxy ausgeführt wird, werden Ihre Connectors automatisch aktualisiert. Falls der Connectorupdatedienst auf Ihrem Server nicht angezeigt wird, müssen Sie den [Connector neu installieren](active-directory-application-proxy-enable.md), um Updates zu erhalten. Bei Mandanten mit mehreren Connectors werden die automatischen Updates nacheinander auf die einzelnen Connectors in jeder Gruppe angewendet, um Ausfallzeiten in Ihrer Umgebung zu vermeiden. 

Wenn Sie nicht warten möchten, bis ein Connector automatisch aktualisiert wird, können Sie die Aktualisierung manuell vornehmen. Rufen Sie auf dem Server mit dem Connector die [Downloadseite für den Connector](https://download.msappproxy.net/subscription/d3c8b69d-6bf7-42be-a529-3fe9c2e70c90/connector/download) auf, und wählen Sie **Herunterladen**. Dadurch wird ein Upgrade für den lokalen Connector gestartet. 

In folgenden Fällen kann es während der Aktualisierung eines Connectors zu Ausfallzeiten kommen:  
- Sie verfügen nur über einen Connector. Wir empfehlen Ihnen, einen zweiten Connector zu installieren und eine [Connectorgruppe zu erstellen](active-directory-application-proxy-connectors-azure-portal.md), um diese Ausfallzeiten zu vermeiden und die hohe Verfügbarkeit zu verbessern.  
- Zu Beginn der Aktualisierung befand sich ein Connector mitten in einer Transaktion. Ihr Browser sollte automatisch versuchen, den Vorgang zu wiederholen. Andernfalls können Sie die Seite aktualisieren. Wenn die Anforderung erneut gesendet wird, wird der Datenverkehr an einen Backupconnector weitergeleitet.

## <a name="outbound-only-networking"></a>Nur ausgehende Netzwerkdaten
Connectors senden nur ausgehende Anforderungen, sodass die Verbindung immer von den Connectors initiiert wird. Das Öffnen von eingehenden Ports ist nicht erforderlich, da der Datenverkehr nach dem Einrichten einer Sitzung in beide Richtungen fließt.

Der ausgehende Datenverkehr wird an den Anwendungsproxydienst und die veröffentlichten Anwendungen gesendet. Der Datenverkehr an den Dienst wird über mehrere unterschiedliche Portnummern an Azure-Datencenter gesendet. Weitere Informationen zu den verwendeten Ports finden Sie unter [Aktivieren des Anwendungsproxys über das Azure-Portal](active-directory-application-proxy-enable.md).

Da es nur um ausgehenden Datenverkehr geht, muss kein Lastenausgleich zwischen den Connectors eingerichtet und auch kein Zugriff in eingehender Richtung über die Firewalls konfiguriert werden.

Informationen zur Konfiguration von Firewallregeln für ausgehenden Datenverkehr finden Sie unter [Work with existing on-premises Proxy servers](application-proxy-working-with-proxy-servers.md) (Verwenden von vorhandenen lokalen Proxyservern).

Verwenden Sie den [Azure AD Application Proxy Connector Ports Test Tool (Testtool der Anwendungsproxy-Connectortools von Azure AD)](https://aadap-portcheck.connectorporttest.msappproxy.net/), um sicherzustellen, dass Ihr Connector den Anwendungsproxydienst erreichen kann. Stellen Sie zumindest sicher, dass die Region USA (Mitte) und die Ihnen am nächsten gelegene Region alle über grüne Häkchen verfügen. Darüber hinaus bedeuten mehr grüne Häkchen größere Resilienz. 

## <a name="network-security"></a>Netzwerksicherheit

Connectors können überall im Netzwerk installiert werden, sodass Anforderungen sowohl an den Anwendungsproxydienst als auch an die Back-End-Anwendungen gesendet werden können. Sie funktionieren gut, wenn Sie sie innerhalb des Unternehmensnetzwerks, in einer DMZ (demilitarisierte Zone) oder sogar auf einem virtuellen Computer installieren, der in der Cloud betrieben wird. Wichtig ist nur, dass der Computer, auf dem der Connector ausgeführt ist, ebenfalls Zugriff auf Ihre Apps hat.

DMZ-Bereitstellungen sind komplizierter. Es gibt jedoch einen Grund, aus dem Sie Connectors möglicherweise doch in einer DMZ bereitstellen möchten: Wenn Sie andere Infrastrukturen wie z.B. Lastenausgleichsmodule in Back-End-Anwendungen oder Angriffserkennungssysteme verwenden möchten.

## <a name="domain-joining"></a>Domänenbeitritt

Connectors können auf einem Computer ausgeführt werden, der nicht in eine Domäne eingebunden ist. Wenn Sie jedoch das einmalige Anmelden für Anwendungen einrichten möchten, die die integrierte Windows-Authentifizierung (IWA) nutzen, benötigen Sie einen Computer, der in eine Domäne eingebunden ist. In diesem Fall müssen die Connectorcomputer in eine Domäne eingebunden werden, für die die eingeschränkte [Kerberos](https://web.mit.edu/kerberos)-Delegierung im Namen der relevanten Benutzer für die veröffentlichten Anwendungen durchgeführt werden kann.

Connectors können auch in Domänen oder Gesamtstrukturen mit einer Teilvertrauensstellung oder in schreibgeschützte Domänencontroller eingebunden werden.

## <a name="connectors-on-hardened-environments"></a>Connectors in festgeschriebenen Umgebungen

In der Regel ist die Connectorbereitstellung unkompliziert und erfordert keine spezielle Konfiguration. Es gibt jedoch einige Bedingungen, die berücksichtigt werden müssen:

* Organisationen, die ausgehenden Datenverkehr einschränken, müssen [erforderliche Ports öffnen](active-directory-application-proxy-enable.md#open-your-ports).
* Möglicherweise muss die Konfiguration von FIPS-konformen Computern geändert werden, damit der Connectordienst, der Connectorupdatedienst und die zugehörigen Installationsprogramme ein Zertifikat generieren und speichern können.
* Organisationen, die ihre Umgebung basierend auf den Prozessen sperren, die die Netzwerkanforderungen ausgeben, müssen sicherstellen, dass für beide Connectordienste der Zugriff auf alle erforderlichen Ports und IPs zugelassen ist.
* In einigen Fällen kann es passieren, dass Proxys für die Weiterleitung des ausgehenden Datenverkehrs die zweistufige Authentifizierung per Zertifikat verhindern, sodass bei der Kommunikation ein Fehler auftritt.

## <a name="connector-authentication"></a>Connectorauthentifizierung

Zur Bereitstellung eines sicheren Diensts müssen sich Connectors beim Dienst authentifizieren, und der Dienst muss sich beim Connector authentifizieren. Hierfür werden Client- und Serverzertifikate verwendet, wenn die Connectors die Verbindung initiieren. So wird erreicht, dass der Benutzername und das Kennwort des Administrator nicht auf dem Connectorcomputer gespeichert werden.

Die verwendeten Zertifikate gelten nur für den Anwendungsproxydienst. Sie werden während der ersten Registrierung erstellt und von den Connectors alle paar Monate automatisch verlängert. 

Wenn ein Connector über mehrere Monate hinweg keine Verbindung mit dem Dienst herstellt, sind die Zertifikate möglicherweise veraltet. In diesem Fall müssen Sie den Connector deinstallieren und neu installieren, um die Registrierung auszulösen. Sie können die folgenden PowerShell-Befehle ausführen:

```
Import-module AppProxyPSModule
Register-AppProxyConnector
```

## <a name="performance-and-scalability"></a>Leistung und Skalierbarkeit

Auch wenn die Skalierung für den Onlinedienst transparent ist, ist die Skalierung in Bezug auf Connectors ein wichtiger Faktor. Sie müssen über eine ausreichende Zahl von Connectors verfügen, um Spitzenlasten verarbeiten zu können. Da Connectors zustandslos sind, wirkt sich die Anzahl von Benutzern oder Sitzungen nicht auf sie aus. Stattdessen stehen sie in Zusammenhang mit der Anzahl von Anforderungen und deren Nutzlastgröße. Bei standardmäßigem Webdatenverkehr kann ein Computer mit durchschnittlicher Leistungsfähigkeit einige Tausend Anforderungen pro Sekunde verarbeiten. Diese Kapazität richtet sich nach den genauen Eigenschaften des jeweiligen Computers.

Die Connectorleistung wird durch die CPU und das Netzwerk bestimmt. Eine gute CPU-Leistung wird für die SSL-Verschlüsselung und -Entschlüsselung benötigt, und die Netzwerkeigenschaften sind wichtig, um eine gute Konnektivität für die Anwendungen und den Onlinedienst in Azure zu erzielen. 

Der Arbeitsspeicher ist für Connectors dagegen weniger wichtig. Der Onlinedienst übernimmt einen Großteil der Verarbeitung und den gesamten nicht authentifizierten Datenverkehr. Alle Schritte, die in der Cloud ausgeführt werden können, werden auch in der Cloud ausgeführt.

Ein weiterer Leistungsfaktor ist die Qualität der Netzwerkverbindung zwischen den Connectors, z.B.:

* **Onlinedienst:** Langsame Verbindungen oder Verbindungen mit hoher Latenz beeinträchtigen den Connectordienst. Es ist am besten, wenn Ihre Organisation per ExpressRoute mit Azure verbunden ist. Andernfalls muss das Netzwerkteam sicherstellen, dass Verbindungen mit Azure auf effiziente Weise verarbeitet werden.  
* **Back-End-Anwendungen:** In einigen Fällen sind zwischen dem Connector und den Back-End-Anwendungen zusätzliche Proxys vorhanden. Dieses Problem lässt sich beheben, indem Sie auf dem Connectorcomputer einen Browser öffnen und so auf diese Anwendungen zugreifen. Wenn Sie die Connectors in Azure ausführen und die Anwendungen lokal vorliegen, ist die Benutzerfreundlichkeit ggf. nicht so hoch wie erwartet.
* **Domänencontroller:** Wenn die Connectors SSO per eingeschränkter Kerberos-Delegierung (KCD) durchführen, nehmen sie vor dem Senden der Anforderung an das Back-End Kontakt mit den Domänencontrollern auf. Die Connectors verfügen zwar über einen Cache mit Kerberos-Tickets, aber in einer Umgebung mit hoher Auslastung kann sich die Reaktionsfähigkeit der Domänencontroller auf die Leistung auswirken. Dieses Problem tritt häufiger bei Connectors auf, die in Azure ausgeführt werden, während die Domänencontroller lokal installiert sind.

## <a name="under-the-hood"></a>Im Hintergrund

Connectors basieren auf dem Windows Server-Webanwendungsproxy und verwenden daher einen Großteil der gleichen Verwaltungstools wie z.B. Windows-Ereignisprotokolle.

 ![Verwalten von Ereignisprotokollen mit der Ereignisanzeige](./media/application-proxy-understand-connectors/event-view-window.png)

und Windows-Leistungsindikatoren. 

 ![Hinzufügen von Leistungsindikatoren zum Connector mit dem Leistungsmonitor](./media/application-proxy-understand-connectors/performance-monitor.png)

Die Connectors verfügen über Administrator- und Sitzungsprotokolle. Die Administratorprotokolle enthalten wichtige Ereignisse und die dazugehörigen Fehler. Die Sitzungsprotokolle enthalten alle Transaktionen und die dazugehörigen Verarbeitungsdetails. 

Um die Protokolle anzuzeigen, wechseln Sie zur Ereignisanzeige, öffnen Sie das Menü **Ansicht**, und aktivieren Sie die Option **Analytische und Debugprotokolle einblenden**. Aktivieren Sie die Protokolle, um mit dem Erfassen von Ereignissen zu beginnen. Diese Protokolle werden nicht im Webanwendungsproxy in Windows Server 2012 R2 angezeigt, da die Connectors auf einer neueren Version basieren.

Sie können den Zustand des Diensts im Fenster „Dienste“ untersuchen. Der Connector besteht aus zwei Windows-Diensten: dem eigentlichen Connector und dem Updatedienst. Beide Dienste müssen jederzeit ausgeführt werden.

 ![Lokale Azure AD-Dienste](./media/application-proxy-understand-connectors/aad-connector-services.png)

## <a name="next-steps"></a>Nächste Schritte
[Beheben von Problemen mit Anwendungsproxys und Connectors](active-directory-application-proxy-troubleshoot.md)

[Verwenden von vorhandenen lokalen Proxyservern](application-proxy-working-with-proxy-servers.md)

[Installieren des Azure AD-Anwendungsproxyconnectors im Hintergrund](active-directory-application-proxy-silent-installation.md)


