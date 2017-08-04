---
title: Grundlegendes zur Azure Active Directory-Architektur | Microsoft-Dokumentation
description: "Hier wird erklärt, was ein Azure AD-Mandant ist und wie Azure über Azure Active Directory verwaltet wird."
services: active-directory
documentationcenter: 
author: MarkusVi
writer: v-lorisc
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/02/2017
ms.author: markvi
ms.translationtype: Human Translation
ms.sourcegitcommit: 07635b0eb4650f0c30898ea1600697dacb33477c
ms.openlocfilehash: 5c60fa737c0133482af8b653f795bf9086c39969
ms.contentlocale: de-de
ms.lasthandoff: 03/28/2017

---
# <a name="understand-azure-active-directory-architecture"></a>Grundlegendes zur Azure Active Directory-Architektur
Mit Azure Active Directory (Azure AD) können Sie den Zugriff auf Azure-Dienste und Ressourcen für Ihre Benutzer sicher verwalten. In Azure AD ist eine vollständige Suite mit Funktionen zur Identitätsverwaltung enthalten. Weitere Informationen zu Azure AD-Features finden Sie unter [Was ist Azure Active Directory?](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-whatis).

Mit Azure AD können Sie Benutzer und Gruppen erstellen und verwalten und Berechtigungen zum Zulassen und Verweigern des Zugriffs auf Unternehmensressourcen aktivieren. Informationen zur Identitätsverwaltung finden Sie unter [Grundlagen der Identitätsverwaltung in Azure](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals-identity).

## <a name="azure-ad-architecture"></a>Azure AD-Architektur
Die geografisch verteilte Architektur von Azure AD bietet umfassende Funktionen für Überwachung, automatisierte Umleitungen, Failover und Wiederherstellung, damit wir für unsere Kunden eine professionelle Verfügbarkeit und Leistung bereitstellen können.

In diesem Artikel werden die folgenden Architekturelemente behandelt:
 *  Design der Dienstarchitektur
 *  Skalierbarkeit 
 *  Fortlaufende Verfügbarkeit
 *  Rechenzentren

### <a name="service-architecture-design"></a>Design der Dienstarchitektur
Zum Erstellen eines skalierbaren und hoch verfügbaren Systems mit umfassenden Daten werden am häufigsten unabhängige Bausteine oder Skalierungseinheiten für die Azure AD-Datenschicht verwendet. Skalierungseinheiten werden als *Partitionen* bezeichnet. 

Die Datenschicht verfügt über mehrere Front-End-Dienste, über die Lese-/Schreibfunktionen bereitgestellt werden. Im folgenden Diagramm ist dargestellt, wie die Komponenten einer Partition mit einem Verzeichnis auf geografisch verteilte Rechenzentren verteilt werden. 

  ![Partitionen mit einem Verzeichnis](./media/active-directory-architecture/active-directory-architecture.png)

Die Komponenten der Azure AD-Architektur enthalten ein primäres Replikat und sekundäre Replikate.

**Primäres Replikat**

Das *primäre Replikat* empfängt alle *Schreibvorgänge* für die Partition, zu der es gehört. Jeder Schreibvorgang wird sofort auf einem sekundären Replikat in einem anderen Rechenzentrum repliziert, bevor die Erfolgsmeldung an den Aufrufer zurückgegeben wird, um für Schreibvorgänge eine Georedundanz sicherzustellen.

**Sekundäre Replikate**

Alle *Lesevorgänge* des Verzeichnisses werden über *sekundäre Replikate* abgewickelt, bei denen es sich um Rechenzentren handelt, die physisch auf verschiedene geografische Regionen verteilt sind. Es gibt viele sekundäre Replikate, weil die Daten asynchron repliziert werden. Lesevorgänge des Verzeichnisses, z.B. Authentifizierungsanforderungen, werden über Rechenzentren abgewickelt, die sich in der Nähe unserer Kunden befinden. Die sekundären Replikate sind für die Skalierbarkeit der Lesevorgänge zuständig.

### <a name="scalability"></a>Skalierbarkeit

Die Skalierbarkeit ist die Fähigkeit eines Diensts zur Erweiterung, um wachsende Anforderungen an die Leistung zu erfüllen. Die Skalierbarkeit von Schreibvorgängen wird durch das Partitionieren der Daten erreicht. Die Skalierbarkeit von Lesevorgängen wird erreicht, indem Daten von einer Partition auf mehreren sekundären Replikaten weltweit repliziert werden.

Anforderungen von Verzeichnisanwendungen werden normalerweise an das Rechenzentrum geleitet, die physisch am wenigsten weit entfernt sind. Schreibvorgänge werden transparent an das primäre Replikat umgeleitet, um für eine Lese-/Schreibkonsistenz zu sorgen. Mit sekundären Replikaten wird die Skalierung von Partitionen erheblich erweitert, da von den Verzeichnissen größtenteils Lesevorgänge bereitgestellt werden.

Für Verzeichnisanwendungen wird eine Verbindung mit den Rechenzentren in der Nähe hergestellt. Dies führt zu einer Verbesserung der Leistung, sodass eine horizontale Skalierung möglich ist. Da eine Verzeichnispartition über viele sekundäre Replikate verfügen kann, können sekundäre Replikate näher an den Verzeichnisclients angeordnet werden. Nur interne Verzeichnisdienstkomponenten, die sehr schreibintensiv sind, zielen direkt auf das aktive primäre Replikat ab.

### <a name="continuous-availability"></a>Fortlaufende Verfügbarkeit

Anhand der Verfügbarkeit (bzw. Betriebszeit) wird definiert, inwiefern ein System ohne Unterbrechungen betrieben werden kann. Der Schlüssel zur hohen Verfügbarkeit von Azure AD liegt darin, dass unsere Dienste Datenverkehr schnell in mehrere geografisch verteilte Rechenzentren verlagern können. Jedes Rechenzentrum ist unabhängig, sodass Fehlermodi ohne Korrelation möglich sind.

Der Partitionsentwurf von Azure AD wurde gegenüber dem AD-Entwurf für Unternehmen vereinfacht, da dies für das zentrale Hochskalieren des Systems wichtig ist. Wir haben einen Entwurf mit einem einzelnen Master gewählt, der einen sorgfältig orchestrierten und deterministischen Failoverprozess für primäre Replikate umfasst.

**Fehlertoleranz**

Ein System weist eine höhere Verfügbarkeit auf, wenn es gegenüber Hardware-, Netzwerk- und Softwareausfällen tolerant ist. Für jede Partition im Verzeichnis ist ein hoch verfügbares Masterreplikat vorhanden: das primäre Replikat. Auf diesem Replikat werden nur Schreibvorgänge für die Partition durchgeführt. Dieses Replikat wird fortlaufend und eingehend überwacht, und Schreibvorgänge können sofort auf ein anderes Replikat verlagert werden (das dann zum neuen primären Replikat wird), wenn ein Fehler erkannt wird. Während des Failovers kann es zu einem Ausfall von Schreibvorgängen kommen, der meist ein bis zwei Minuten dauert. Die Lesevorgänge sind während dieses Zeitraums nicht beeinträchtigt.

Für Lesevorgänge (deren Zahl die Schreibvorgänge weit übersteigt) werden nur sekundäre Replikate verwendet. Da sekundäre Replikate idempotent sind, kann der Verlust eines Replikats auf einer Partition leicht kompensiert werden, indem die Lesevorgänge auf ein anderes Replikat verlagert werden (normalerweise in demselben Rechenzentrum).

**Dauerhaftigkeit von Daten**

Bevor ein Schreibvorgang bestätigt wird, erfolgt dafür ein dauerhafter Commit in mindestens zwei Rechenzentren. Hierfür wird für den Schreibvorgang zuerst auf dem primären Replikat ein Commit durchgeführt, und anschließend wird er sofort in mindestens einem anderen Rechenzentrum repliziert. So wird sichergestellt, dass in einer Notfallsituation ein potenzieller Ausfall des Rechenzentrums, auf dem das primäre Replikat gehostet wird, nicht zu Datenverlust führt.

Azure AD verfügt über eine [Recovery Time Objective (RTO)](https://en.wikipedia.org/wiki/Recovery_time_objective) von null für die Tokenausstellung und Verzeichnislesevorgänge und eine RTO im Minutenbereich (ca. 5 Minuten) für Verzeichnisschreibvorgänge. Außerdem wird eine [Recovery Point Objective (RPO)](https://en.wikipedia.org/wiki/Recovery_point_objective) von null verwendet, sodass es bei Failovern nicht zu Datenverlust kommt.

### <a name="data-centers"></a>Rechenzentren

Die Replikate von Azure AD werden in Rechenzentren gespeichert, die weltweit verteilt sind. Weitere Informationen finden Sie unter [Azure-Rechenzentren](https://azure.microsoft.com/en-us/overview/datacenters).

Azure AD wird basierend auf Rechenzentren mit den folgenden Merkmalen betrieben:

 * Authentifizierung, Graph und andere AD-Dienste sind hinter dem Gatewaydienst angeordnet. Das Gateway verwaltet den Lastenausgleich dieser Dienste. Es wird automatisch ein Failover durchgeführt, wenn mit transaktionalen Integritätstests fehlerhafte Server erkannt werden. Basierend auf diesen Integritätstests wird Datenverkehr vom Gateway dynamisch an fehlerfreie Rechenzentren weitergeleitet.
 * Für *Lesevorgänge* verfügt das Verzeichnis über sekundäre Replikate und entsprechende Front-End-Dienste in einer Aktiv/Aktiv-Konfiguration, die in mehreren Rechenzentren betrieben wird. Wenn ein gesamtes Rechenzentrum ausfällt, wird der Datenverkehr automatisch an ein anderes Rechenzentrum weitergeleitet.
 *  Für *Schreibvorgänge* führt das Verzeichnis für primäre Replikate (Master) ein Failover für alle Rechenzentren durch, indem Schritte für geplante Failover (neues primäres Replikat wird mit altem primärem Replikat synchronisiert) oder Notfallfailover ausgeführt werden. Die Dauerhaftigkeit der Daten wird erreicht, indem ein Commit in mindestens zwei Rechenzentren repliziert wird.

**Datenkonsistenz**

Das Verzeichnismodell lautet „Letztliche Konsistenz“. Ein typisches Problem mit verteilten Systemen mit asynchroner Replikation ist, dass die Daten, die von einem „bestimmten“ Replikat zurückgegeben werden, unter Umständen nicht aktuell sind. 

Azure AD bietet Lese-/Schreibkonsistenz für Anwendungen, die auf ein sekundäres Replikat abzielen, indem die zugehörigen Schreibvorgänge an das primäre Replikat weitergeleitet und synchron dazu die Schreibvorgänge zurück auf das sekundäre Replikat verschoben werden.

Anwendungsschreibvorgänge, für die die Graph-API von Azure AD verwendet wird, werden in Bezug auf die Beibehaltung der Affinität mit einem Verzeichnisreplikat für Schreib-/Lesekonsistenz abstrahiert. Der Graph-Dienst von Azure AD verfügt über eine logische Sitzung mit einer Affinität mit einem sekundären Replikat, das für Lesevorgänge verwendet wird. Die Affinität wird in einem „Replikattoken“ erfasst, das vom Graphdienst mithilfe eines verteilten Cache zwischengespeichert wird. Dieses Token wird dann für nachfolgende Vorgänge in derselben logischen Sitzung verwendet. 

 >[!NOTE]
 >Schreibvorgänge werden sofort auf dem sekundären Replikat repliziert, für das die Lesevorgänge der logischen Sitzung ausgestellt wurden.
 >

**Sicherungsschutz**

Das Verzeichnis implementiert anstelle des endgültigen Löschens das vorläufige Löschen für Benutzer und Mandanten, um eine einfache Wiederherstellung zu ermöglichen, falls Daten von einem Kunden versehentlich gelöscht werden. Wenn Ihr Mandantenadministrator Benutzer versehentlich löscht, kann der Vorgang leicht rückgängig gemacht werden, und die gelöschten Benutzer können wiederhergestellt werden. 

Azure AD implementiert tägliche Sicherungen aller Daten und kann die Daten so autoritativ wiederherstellen, falls es zu logischen Löschungen oder Beschädigungen kommt. Für unsere Datenschicht werden Fehlerbehebungscodes verwendet, damit eine Überprüfung auf Fehler durchgeführt werden kann und bestimmte Arten von Datenträgerfehlern automatisch behoben werden können.

**Metriken und Überwachungen**

Zum Ausführen eines hoch verfügbaren Diensts sind professionelle Metriken und Überwachungsfunktionen erforderlich. Azure AD führt eine fortlaufende Analyse durch und meldet wichtige Metriken zur Dienstintegrität und Erfolgskriterien für die einzelnen Dienste. Wir entwickeln und optimieren ständig Metriken, Überwachungsfunktionen und Warnungen für jedes Szenario in jedem Azure AD-Dienst und über alle Dienste hinweg.

Falls ein Azure AD-Dienst nicht wie erwartet funktioniert, ergreifen wir sofort Maßnahmen, um die Funktionalität so schnell wie möglich wiederherzustellen. Als wichtigste Metrik wird von Azure AD nachverfolgt, wie schnell wir ein Problem mit einem Kunden oder einer Livewebsite erkennen und beheben können. Wir investieren in hohem Maße in die Überwachung und Warnungen, um die Erkennungsdauer zu verringern (Ziel: weniger als 5 Minuten), und in die Betriebsbereitschaft, um die Lösungsdauer zu verringern (Ziel: weniger als 30 Minuten).

**Sichere Vorgänge**

Wir nutzen Kontrolleinrichtungen für den Betrieb, z.B. Multi-Factor Authentication (MFA) für alle Vorgänge, und führen für alle Vorgänge außerdem Überprüfungen durch. Darüber hinaus setzen wir ein Just-in-Time-System zur Erhöhung von Rechten ein, um den benötigten temporären Zugriff für alle betrieblichen Aufgaben fortlaufend nach Bedarf gewähren zu können. Weitere Informationen finden Sie unter [Die vertrauenswürdige Cloud](https://azure.microsoft.com/en-us/support/trust-center).

## <a name="next-steps"></a>Nächste Schritte
[Entwicklerhandbuch zu Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-developers-guide)


