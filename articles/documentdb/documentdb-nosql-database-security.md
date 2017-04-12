---
title: "Sicherheit von NoSQL-Datenbanken – Azure DocumentDB | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Azure DocumentDB Datenbankschutz und Datensicherheit für Ihre NoSQL-Daten bereitstellt."
keywords: nosql database security, information security, data security, database encryption, database protection, security policies, security testing
services: documentdb
author: mimig1
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: a02a6a82-3baf-405c-9355-7a00aaa1a816
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: mimig
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 9cf87aa75b2ef65719a38e446a81086d265e7f4d
ms.lasthandoff: 04/03/2017


---

# <a name="documentdb-nosql-database-security"></a>DocumentDB-Sicherheit für NoSQL-Datenbanken

 Dieser Artikel erläutert bewährte Methoden und wichtige Funktionen von Azure DocumentDB für die Sicherheit von NoSQL-Datenbanken, mit denen Sie Sicherheitsverletzungen in einer Datenbank verhindern bzw. erkennen und darauf reagieren können.

## <a name="how-do-i-secure-my-nosql-database"></a>Wie sichere ich meine NoSQL-Datenbank? 

Für die Sicherheit der Daten sind Sie, Ihr Kunde und Ihr Datenbankanbieter gemeinsam verantwortlich. Der Umfang Ihres Verantwortungsbereichs kann je nach Datenbankanbieter unterschiedlich ausfallen. Wenn Sie sich für eine lokale Lösung entschieden haben, müssen Sie weitgehend selbst für die Sicherheit sorgen – vom Endpunktschutz bis hin zur physischen Sicherung Ihrer Hardware. Das ist keine leichte Aufgabe. Wenn Sie einen PaaS-basierten Clouddatenbankanbieter wie z.B. Azure DocumentDB ins Boot holen, verkleinert sich Ihr Verantwortungsbereich erheblich. Die folgende Abbildung – entnommen aus dem Microsoft-Whitepaper [Shared Responsibilities for Cloud Computing](https://aka.ms/sharedresponsibility) (Gemeinsame Verantwortung für das Cloud Computing) – zeigt, wie viel weniger Verantwortung auf Ihren Schultern liegt, wenn Sie mit einem PaaS-Anbieter wie Azure DocumentDB arbeiten.

![Verantwortlichkeiten von Kunde und Datenbankanbieter](./media/documentdb-nosql-database-security/nosql-database-security-responsibilities.png)

Das obige Diagramm zeigt allgemeine Sicherheitskomponenten in der Cloud. Um welche Elemente müssen Sie sich aber im Speziellen für Ihre NoSQL-Datenbanklösung kümmern? Und wie vergleichen Sie Lösungen sinnvoll miteinander? 

Wir empfehlen die folgende Checkliste mit Anforderungen, anhand derer Sie NoSQL-Datenbanksysteme vergleichen können:

- Netzwerksicherheit und Firewalleinstellungen
- Benutzerauthentifizierung und differenzierte Benutzersteuerung
- Möglichkeit zum globalen Replizieren von Daten bei regionalen Ausfällen
- Möglichkeit zum Durchführen eines Failovers zwischen Rechenzentren
- Lokale Datenreplikation innerhalb eines Rechenzentrums
- Automatische Datensicherungen
- Wiederherstellung gelöschter Daten aus Sicherungskopien
- Schützen und Isolieren von vertraulichen Daten
- Überwachen auf Angriffe
- Reagieren auf Angriffe
- Möglichkeit, Daten per Geofencing auf einen geografischen Raum einzugrenzen, um Anforderungen an die Datengovernance zu erfüllen
- Physischer Schutz von Servern in geschützten Rechenzentren

Folgendes mag auf der Hand liegen, aber [umfassende Verletzungen der Datenbanksicherheit](http://thehackernews.com/2017/01/mongodb-database-security.html) in jüngster Zeit erinnern uns daran, wie wichtig diese vermeintlich einfachen Anforderungen sind:
- Gepatchte Server, die immer auf dem neuesten Stand gehalten werden
- Standardmäßig HTTPS sowie SSL-Verschlüsselung
- Administratorkonten mit sicheren Kennwörtern

## <a name="how-does-azure-documentdb-secure-my-database"></a>Wie sichert Azure DocumentDB meine Datenbank?

Sehen wir uns die obige Liste an: Wie viele dieser Sicherheitsanforderungen erfüllt Azure DocumentDB? Jede einzelne.

Betrachten wir die Anforderungen nun im Detail.

|Sicherheitsanforderung|Sicherheitsansatz von DocumentDB|
|---|---|---|
|Netzwerksicherheit|Die Verwendung einer IP-Firewall ist die erste Schutzschicht, um Ihre NoSQL-Datenbank zu sichern. DocumentDB unterstützt die richtliniengesteuerte IP-basierte Zugriffssteuerung zur Unterstützung der Firewall für eingehende Verbindungen. Die IP-basierte Zugriffssteuerung ähnelt den Firewallregeln, die von herkömmlichen Datenbanksystemen verwendet werden, erweitert diese jedoch: Auf ein DocumentDB-Datenbankkonto kann nur von genehmigten Computern oder Clouddiensten aus zugegriffen werden. <br><br>Mit DocumentDB können Sie eine bestimmte IP-Adresse (168.61.48.0), einen IP-Adressbereich (168.61.48.0/8) sowie Kombinationen aus IP-Adressen und -Adressbereichen aktivieren. <br><br>Alle Anforderungen von Computern, die sich nicht auf dieser Zulassungsliste befinden, werden von DocumentDB blockiert. Anforderungen von zugelassenen Computern und Clouddiensten müssen den Authentifizierungsprozess durchlaufen, um Zugriff auf die Ressourcen zu erhalten.<br><br>Weitere Informationen dazu erhalten Sie unter [DocumentDB-Firewallunterstützung](documentdb-firewall-support.md).|
|Autorisierung|DocumentDB verwendet einen hashbasierten Code für die Authentifizierung von Nachrichten (Hash-based Message Authentication Code, HMAC) zur Autorisierung. <br><br>Für jede Anforderung wird mithilfe des geheimen Kontoschlüssels ein Hash erstellt, und der daraus resultierende Base64-codierte Hashwert wird mit jedem Aufruf an DocumentDB gesendet. Zum Überprüfen der Anforderung verwendet der DocumentDB-Dienst den richtigen geheimen Schlüssel und die zugehörigen Eigenschaften, um einen Hash zu generieren. Anschließend vergleicht der Dienst diesen Wert mit dem Wert in der Anforderung. Wenn die beiden Werte übereinstimmen, wird der Vorgang autorisiert und die Anforderung verarbeitet. Andernfalls wird ein Autorisierungsfehler ausgegeben und die Anforderung abgelehnt.<br><br>Sie können einen [Hauptschlüssel](documentdb-secure-access-to-data.md#master-keys) oder ein [Ressourcentoken](documentdb-secure-access-to-data.md#resource-tokens) verwenden, das differenzierten Zugriff auf eine Ressource wie z.B. ein Dokument ermöglicht.<br><br>Weitere Informationen dazu erhalten Sie unter [Sicherer Zugriff auf Daten in DocumentDB](documentdb-secure-access-to-data.md).|
|Benutzer und Berechtigungen|Unter Verwendung des [Hauptschlüssels](#master-key) für das Konto können Sie Benutzerressourcen und Berechtigungsressourcen für jede Datenbank erstellen. Ein [Ressourcentoken](#resource-token) wird einer Berechtigung in einer Datenbank zugeordnet und legt fest, ob ein Benutzer Zugriff (Lese-/Schreibzugriff, schreibgeschützter Zugriff oder kein Zugriff) auf eine Anwendungsressourcen in der Datenbank erhält. Zu Anwendungsressourcen gehören Auflistungen, Dokumente, Anlagen, gespeicherte Prozeduren, Trigger und benutzerdefinierte Funktionen. Das Ressourcentoken wird dann während der Authentifizierung verwendet, um den Zugriff auf die Ressource zu gewähren oder zu verweigern.<br><br>Weitere Informationen dazu erhalten Sie unter [Sicherer Zugriff auf Daten in DocumentDB](documentdb-secure-access-to-data.md).|
|Active Directory-Integration (RBAC)| Sie können den Zugriff auf das Datenbankkonto auch mithilfe der Zugriffssteuerung (IAM) im Azure-Portal einrichten. IAM stellt eine rollenbasierte Zugriffssteuerung bereit und lässt sich in Active Directory integrieren. Sie können integrierte oder benutzerdefinierte Rollen für Einzelbenutzer oder Gruppen verwenden, wie in der folgenden Abbildung gezeigt.<br><br>![Zugriffssteuerung (IAM) im Azure-Portal: Veranschaulichung der NoSQL-Datenbanksicherheit](./media/documentdb-nosql-database-security/nosql-database-security-identity-access-management-iam-rbac.png)|
|Globale Replikation|DocumentDB bietet eine sofort einsatzbereite globale Verteilung, mit der Sie Ihre Daten mit einem einzigen Mausklick in jedes der weltweiten Azure-Rechenzentren replizieren können. Mit der globalen Replikation können Sie global skalieren und für niedrige Latenzen beim Zugriff auf Ihre Daten auf der ganzen Welt sorgen.<br><br>Im Kontext der Sicherheit schützt die globale Replikation Ihre Daten bei regionalen Ausfällen.<br><br>Weitere Informationen finden Sie unter [Globale Verteilung von Daten](documentdb-distribute-data-globally.md).|
|Regionale Failover|Wenn Sie Ihre Daten in mehr als ein Rechenzentrum repliziert haben, führt DocumentDB automatisch ein Rollover Ihrer Vorgänge aus, falls ein regionales Rechenzentrum offline geschaltet wird. Sie können anhand der Regionen, in denen Ihre Daten repliziert werden, eine Prioritätsliste mit Failoverregionen erstellen. <br><br>Weitere Informationen finden Sie unter [Regionale Failover in Azure DocumentDB](documentdb-regional-failovers.md).|
|Lokale Replikation|DocumentDB repliziert Daten automatisch auch innerhalb eines einzelnen Rechenzentrums und sorgt so für hohe Verfügbarkeit sowie die Möglichkeit, verschiedene [Konsistenzebenen](documentdb-consistency-levels.md) auszuwählen. So gewährleistet DocumentDB eine  [SLA mit 99,99 % Betriebszeit](https://azure.microsoft.com/support/legal/sla/documentdb/v1_1/) und umfasst auch eine finanzielle Garantie – dies bietet kein anderer NoSQL-Datenbankdienst.|
|Automatisierte Onlinesicherungen|DocumentDB-Datenbanken werden regelmäßig aktualisiert und in einem georedundanten Speicher gespeichert. <br><br>Weitere Informationen erhalten Sie unter [Automatische Onlinesicherung und -wiederherstellung mit DocumentDB](documentdb-online-backup-and-restore.md).|
|Wiederherstellen gelöschter Daten|Automatisierte Onlinesicherungen können zum Wiederherstellen von Daten verwendet werden, die versehentlich gelöscht wurden. Dies ist bis zu ca. 30 Tage nach dem Löschen möglich. <br><br>Weitere Informationen erhalten Sie unter [Automatische Onlinesicherung und -wiederherstellung mit DocumentDB](documentdb-online-backup-and-restore.md).|
|Schützen und Isolieren von vertraulichen Daten|Personenbezogene und vertrauliche Daten können in bestimmte Sammlungen isoliert werden, und der Lese-/Schreibzugriff bzw. der schreibgeschützte Zugriff kann auf bestimmte Benutzer beschränkt werden.|
|Überwachen auf Angriffe|Mithilfe von Überwachungsprotokollierung und Aktivitätsprotokollen können Sie Ihr Konto auf normale und ungewöhnliche Aktivitäten überwachen. Sie können anzeigen, welche Vorgänge für Ihre Ressourcen ausgeführt wurden, wer den jeweiligen Vorgang initiiert hat, wann der Vorgang ausgeführt wurde, welchen Status der Vorgang aufweist und vieles mehr.<br><br>![Aktivitätsprotokolle für Azure DocumentDB](./media/documentdb-nosql-database-security/nosql-database-security-application-logging.png)|
|Reagieren auf Angriffe|Nachdem Sie den Azure-Support kontaktiert haben, um einen möglichen Angriff zu melden, wird ein aus fünf Schritten bestehender Prozess zur Reaktion auf den Incident angestoßen. Ziel dieses Prozesses ist es, die normale Dienstsicherheit und den normalen Betrieb so schnell wie möglich wiederherzustellen, nachdem ein Problem erkannt und eine Untersuchung gestartet wurde.<br><br>Weitere Informationen erhalten Sie unter [Microsoft Azure Security Response in the Cloud](https://aka.ms/securityresponsepaper) (Sicherheitsreaktion von Microsoft Azure in der Cloud).|
|Geofencing (geografische Eingrenzung)|DocumentDB stellt die Datengovernance und -compliance für unabhängige Regionen sicher (z.B. Deutschland, China, US Government).|
|Geschützte Einrichtungen|Die Daten in DocumentDB sind auf SSDs in den geschützten Rechenzentren von Azure gespeichert.<br><br>Weitere Informationen erhalten Sie unter [Globale Rechenzentren von Microsoft](https://www.microsoft.com/en-us/cloud-platform/global-datacenters).|
|HTTPS-/SSL-/TLS-Verschlüsselung|Alle DocumentDB-Interaktionen zwischen Client und Dienst erfolgen über SSL/TLS 1.2. Auch alle Replikationen innerhalb eines Rechenzentrums und zwischen verschiedenen Rechenzentren erfolgen über SSL/TLS 1.2.|
|Gepatchte Server|Da DocumentDB eine verwaltete NoSQL-Datenbank ist, müssen Sie keine Server verwalten oder patchen – das wird automatisch für Sie erledigt.|
|Administratorkonten mit sicheren Kennwörtern|Es ist kaum zu glauben, dass diese Anforderung überhaupt erwähnt werden muss, aber im Gegensatz zu einigen unserer Wettbewerber ist es in DocumentDB unmöglich, ein Administratorkonto ohne Kennwort einzurichten.<br><br> Die Sicherheit über SSL und die auf HMAC-Geheimnissen basierende Authentifizierung sind standardmäßig integriert.|
|Zertifizierungen für Sicherheit und Datenschutz|DocumentDB verfügt über folgende Zertifizierungen: [ISO 27001](https://www.microsoft.com/en-us/TrustCenter/Compliance/ISO-IEC-27001), [EU-Musterklauseln](https://www.microsoft.com/en-us/TrustCenter/Compliance/EU-Model-Clauses) und [HIPAA](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA). Weitere Zertifizierungen folgen.|

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Hauptschlüsseln und Ressourcentoken finden Sie unter [Sicherer Zugriff auf Daten in DocumentDB](documentdb-secure-access-to-data.md).

Weitere Informationen zu Microsoft-Zertifizierungen finden Sie im [Azure Trust Center](https://azure.microsoft.com/en-us/support/trust-center/).
