---
title: Erweiterte Bedrohungserkennung von Azure | Microsoft-Dokumentation
description: "Erhalten Sie Informationen zum Identitätsschutz und den zugehörigen Funktionen."
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2017
ms.author: TomSh
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 7db677614c23a3447e3e40ae867711a754b06d0d
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---

# <a name="azure-advanced-threat-detection"></a>Erweiterte Bedrohungserkennung von Azure
## <a name="introduction"></a>Einführung

### <a name="overview"></a>Übersicht

Microsoft hat eine Reihe von Whitepapers, Sicherheitsübersichten, bewährten Methoden und Prüflisten entwickelt, um Azure-Kunden bei den verschiedenen sicherheitsbezogenen Funktionen zu unterstützen, die in und um die Azure Platform herum verfügbar sind. Die Themen variieren in Bezug auf Umfang und Reichweite und werden regelmäßig aktualisiert. Dieses Dokument ist Teil dieser Serie, wie im folgenden Abschnitt zusammengefasst.

### <a name="azure-platform"></a>Azure Platform

Azure ist eine öffentliche Clouddienstplattform, die die breiteste Palette an Betriebssystemen, Programmiersprachen, Frameworks, Tools, Datenbanken und Geräten unterstützt.
Die folgenden Programmiersprachen werden unterstützt:
-   Linux-Container mit Docker-Integration ausführen.
-   Apps mit JavaScript, Python, .NET, PHP, Java und Node.js erstellen.
-   Back-Ends für iOS-, Android- und Windows-Geräte erstellen.

Die öffentlichen Azure-Clouddienste unterstützen dieselben Technologien, die bereits von Millionen von Entwicklern und IT-Profis zuverlässig eingesetzt werden.

Wenn Sie mit mithilfe einer Organisation zu einer öffentlichen Cloud migrieren, ist diese Organisation dafür verantwortlich, dass Ihre Daten geschützt werden. Sie ist für die Sicherheit und Governance für das System verantwortlich.

Die Infrastruktur von Azure ist von den Hardwareressourcen bis zu den Anwendungen vollständig auf das gleichzeitige Hosten von Millionen von Kunden ausgelegt und stellt für Unternehmen eine vertrauenswürdige Grundlage zur Erfüllung ihrer Sicherheitsanforderungen dar. Azure bietet eine Vielzahl von Optionen zum Konfigurieren und Anpassen der Sicherheit, um den Anforderungen Ihrer App-Bereitstellungen zu entsprechen. Dieses Dokument unterstützt Sie bei der Erfüllung dieser Anforderungen.

### <a name="abstract"></a>Zusammenfassung

Microsoft Azure bietet integrierte Funktionen zur erweiterten Bedrohungserkennung über Dienste wie Azure Active Directory, Azure Operations Management Suite (OMS) und Azure Security Center. Diese Auflistung von Sicherheitsdiensten und -funktionen bietet eine einfache und schnelle Möglichkeit zu verstehen, was innerhalb Ihrer Azure-Bereitstellungen abläuft.

Dieses Whitepaper führt Sie durch die „Microsoft Azure-Ansätze“ zur Diagnose von Sicherheitsrisiken und der Analyse des Risikos, das mit böswilligen Aktivitäten verbunden ist, die auf Server und andere Azure-Ressourcen ausgerichtet sind. Dies hilft Ihnen dabei, die Methoden zur Identifikation und Sicherheitsrisikoverwaltung mit optimierten Lösungen von Azure Platform und kundenorientierte Sicherheitsdiensten und Technologien zu identifizieren.

Dieses Whitepaper legt den Schwerpunkt auf die Technologie der Azure Platform und auf kundenorientierte Kontrollen. Es behandelt weder SLAs, Preismodelle noch Überlegungen zu DevOps-Methoden.

## <a name="azure-active-directory-identity-protection"></a>Azure Active Directory Identity Protection

![Azure Active Directory Identity Protection](./media/azure-threat-detection/azure-threat-detection-fig1.png)


[Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) ist ein Feature der [Azure AD Premium P2](https://docs.microsoft.com/azure/active-directory/active-directory-editions)-Edition, das Ihnen eine Übersicht über Risikoereignisse und potenzielle Sicherheitsrisiken bietet, die für die Identitäten Ihrer Organisation bestehen. Microsoft beschäftigt sich schon seit mehr als einem Jahrzehnt mit dem Schutz cloudbasierter Identitäten, und mit Azure AD Identity Protection stellt Microsoft diese Schutzsysteme jetzt für Unternehmenskunden zur Verfügung. Für Identity Protection werden die vorhandenen Azure AD-Funktionen zur Erkennung von Anomalien genutzt (über die [Berichte zu anomalen Aktivitäten von Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-view-access-usage-reports#anomalous-activity-reports)), und es werden neue Risikoereignistypen eingeführt, mit denen Echtzeitanomalien erkannt werden können.

Identity Protection nutzt adaptive Machine Learning-Algorithmen und heuristische Verfahren, um Anomalien und Risikoereignisse zu erkennen, die auf die Kompromittierung einer Identität hindeuten können. Mit diesen Daten generiert Identity Protection Berichte und Warnungen, damit Sie diese Risikoereignisse untersuchen und entsprechende Aktionen zur Korrektur oder Lösung durchführen können.

Azure Active Directory Identity Protection ist aber viel mehr als nur ein Tool für die Überwachung und Berichterstellung. Basierend auf Risikoereignissen berechnet Identity Protection eine Benutzerrisikostufe für jeden Benutzer, sodass Sie risikobasierte Richtlinien konfigurieren können, um die Identitäten Ihrer Organisation automatisch zu schützen.

Mit diesen risikobasierten Richtlinien in Verbindung mit anderen Kontrollelementen für den [bedingten Zugriff](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access) von Azure Active Directory und [EMS](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access) können adaptive Korrekturaktionen blockiert oder bereitgestellt werden. Hierzu gehören Kennwortzurücksetzungen und die Durchsetzung der Multi-Factor Authentication.

### <a name="identity-protections-capabilities"></a>Funktionen von Identity Protection

Azure Active Directory Identity Protection ist viel mehr als nur ein Tool für die Überwachung und Berichterstellung. Zum Schutz der Identitäten Ihrer Organisation können Sie risikobasierte Richtlinien konfigurieren, die automatisch auf erkannte Probleme reagieren, wenn eine angegebene Risikostufe erreicht wurde. Mit diesen Richtlinien in Verbindung mit anderen Kontrollelementen für den bedingten Zugriff von Azure Active Directory und EMS können adaptive Korrekturaktionen entweder blockiert oder initiiert werden. Hierzu gehören Kennwortzurücksetzungen und die Durchsetzung der Multi-Factor Authentication.

Beispiele zu einigen Methoden, mit denen Azure Identity Protection dabei helfen kann, Ihre Konten und Identitäten zu schützen:

[Erkennen von Risikoereignissen und gefährdeten Konten:](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection#detection)
-   Erkennen von sechs Risikoereignistypen per Machine Learning und heuristischen Regeln
-   Berechnen von Benutzerrisikostufen
-   Bereitstellen von benutzerdefinierten Empfehlungen zur Verbesserung der allgemeinen Sicherheit, indem Sicherheitsrisiken aufgedeckt werden

[Untersuchen von Risikoereignissen:](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection#investigation)
-   Senden von Benachrichtigungen für Risikoereignisse
-   Untersuchen von Risikoereignissen mit relevanten und kontextbezogenen Informationen
-   Bereitstellen grundlegender Workflows zum Nachverfolgen von Untersuchungen
-   Bereitstellen des einfachen Zugriffs auf Korrekturaktionen, z. B. Kennwortzurücksetzung

[Risikobasierte Richtlinien für bedingten Zugriff:](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection#risky-sign-ins)
-   Richtlinie für Abwehrmaßnahmen bei risikobehafteten Anmeldungen, indem Anmeldungen blockiert oder die Multi-Factor Authentication erzwungen wird.
-   Richtlinie zum Blockieren oder Schützen risikobehafteter Benutzerkonten
-   Richtlinie zur Durchsetzung der Registrierung von Benutzern für die Multi-Factor Authentication

### <a name="azure-ad-privileged-identity-management-pim"></a>Azure AD Privileged Identity Management (PIM)

Mit [Azure Active Directory (AD) Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure),

![Azure AD Privileged Identity Management](./media/azure-threat-detection/azure-threat-detection-fig2.png)

können Sie den Zugriff in Ihrem Unternehmen verwalten, überwachen und steuern. Dazu zählt der Zugriff auf Ressourcen in Azure AD und anderen Microsoft-Onlinediensten wie Office 365 oder Microsoft Intune.

Azure AD Privileged Identity Management ermöglicht Ihnen Folgendes:

-   Abrufen von Warnungen und Berichten zu Azure AD-Administratoren und Just-In-Time-Administratorzugriff auf Microsoft Online Services wie Office 365 und Intune

-   Abrufen von Berichten zum Administratorzugriffsverlauf und zu Änderungen bei Administratorzuweisungen

-   Aktivieren von Benachrichtigungen zum Zugriff auf eine privilegierte Rolle

## <a name="microsoft-operations-management-suite-oms"></a>Microsoft Operations Management Suite (OMS)

[Microsoft Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) ist die cloudbasierte IT-Verwaltungslösung von Microsoft, die Ihnen das Verwalten und Schützen Ihrer Infrastruktur lokal und in der Cloud erleichtert. Da OMS als cloudbasierter Dienst implementiert wird, ist die Lösung mit minimalen Investitionen in Infrastrukturdienste schnell betriebsbereit. Neue Sicherheitsfeatures werden automatisch bereitgestellt, sodass Sie laufende Wartungs- und Upgradekosten sparen.

Zusätzlich zur Bereitstellung wertvoller Dienste kann OMS in System Center-Komponenten wie [System Center Operations Manager](https://blogs.technet.microsoft.com/cbernier/2013/10/23/monitoring-windows-azure-with-system-center-operations-manager-2012-get-me-started/) integriert werden, um Ihre bestehenden Investitionen für die Sicherheitsverwaltung in die Cloud zu erweitern. Durch die Kombination von System Center und OMS können Sie vollständig vom Hybrid-Management profitieren.

### <a name="holistic-security-and-compliance-posture"></a>Ganzheitlicher Status zu Sicherheit und Compliance

Das [Sicherheits- und Überwachungsdashboard von OMS](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started) ermöglicht mit integrierten Suchabfragen für relevante Probleme, die Ihre Aufmerksamkeit erfordern, einen umfassenden Einblick in die Lage der IT-Sicherheit Ihres Unternehmens. Das Dashboard für Sicherheit und Überwachung ist die Startseite für alle Aspekte, die sich in OMS auf die Sicherheit beziehen. Hier erhalten Sie einen allgemeinen Überblick über den Sicherheitszustand Ihres Computers. Darüber hinaus können Sie alle Ereignisse der letzten 24 Stunden, sieben Tage oder eines anderen benutzerdefinierten Zeitraums anzeigen.

OMS-Dashboards helfen Ihnen dabei, den Gesamtsicherheitsstatus einer beliebigen Umgebung schnell und einfach im Kontext von IT-Vorgängen wie den Folgenden zu verstehen: Bewertung von Softwareupdates, Antischadsoftwarebewertung und Konfigurationsgrundwerte. Darüber hinaus sind die Sicherheitsprotokolldaten leicht zugänglich, um die Überwachungsprozesse für Sicherheit und Compliance zu optimieren.

Das OMS-Dashboard „Security and Audit“ (Sicherheit und Überwachung) ist in vier Hauptkategorien unterteilt:

![OMS-Dashboard „Security and Audit“ (Sicherheit und Überwachung)](./media/azure-threat-detection/azure-threat-detection-fig3.jpg)

-   **Security Domains (Sicherheitsdomänen)**: In diesem Bereich können Sie die Sicherheitsdatensätze in Abhängigkeit der Zeit näher untersuchen. Außerdem können Sie die Bereiche Bewertung von Schadsoftware, Updatebewertung, Netzwerksicherheit, Informationen zu Identität und Zugriff und Computer mit Sicherheitsereignissen nutzen und schnell auf das Azure Security Center-Dashboard zugreifen.

-   **Notable Issues (Relevante Probleme)**: Mit dieser Option können Sie schnell identifizieren, wie viele aktive Probleme vorhanden sind und welchen Schweregrad sie jeweils haben.

-   **Detections (Preview) (Erkennungen (Vorschau))**: Ermöglicht die Identifizierung von Angriffsmustern, indem Sicherheitswarnungen visualisiert werden, wenn sie für Ihre Ressourcen auftreten.

-   **Threat Intelligence (Informationen zu Bedrohungen)**: Ermöglicht die Identifizierung von Angriffsmustern, indem die Gesamtzahl der Server mit ausgehendem schädlichem IP-Datenverkehr, der Bedrohungstyp und eine Karte mit den Ursprungsorten dieser IP-Adressen visualisiert werden.

-   **Common security queries (Allgemeine Sicherheitsabfragen)**: Diese Option liefert Ihnen eine Liste mit den häufigsten Sicherheitsabfragen, die Sie zum Überwachen der Umgebung verwenden können. Wenn Sie in eine dieser Abfragen klicken, wird das Blatt „Suche“ mit den Abfrageergebnissen geöffnet.

### <a name="insight-and-analytics"></a>Insight & Analytics
Im Mittelpunkt von [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) steht das OMS-Repository, das in der Azure-Cloud gehostet wird.

![Insight & Analytics](./media/azure-threat-detection/azure-threat-detection-fig4.png)

Konfigurieren Sie Datenquellen und fügen Sie Ihrem Abonnement Lösungen hinzu, um Daten aus verbundenen Quellen im Repository zu sammeln.

![Abonnement](./media/azure-threat-detection/azure-threat-detection-fig5.png)

Datenquellen und Lösungen erzeugen verschiedene Arten von Datensätzen, die jeweils über einen eigenen Eigenschaftensatz verfügen, aber dennoch in Abfragen im Repository gemeinsam analysiert werden können. So können Sie die gleichen Tools und Methoden für verschiedene Arten von Daten verwenden, die aus verschiedenen Quellen gesammelt werden.


Der größte Teil der Interaktion mit Log Analytics erfolgt über das OMS-Portal, das in jedem Browser ausgeführt werden kann und Ihnen Zugriff auf Konfigurationseinstellungen und verschiedene Tools bietet, mit denen Sie die gesammelten Daten analysieren und mit den Daten agieren können. Im Portal können Sie mithilfe von [Protokollsuchvorgängen](https://docs.microsoft.com/azure/log-analytics/log-analytics-log-searches) Abfragen erstellen, um die gesammelten Daten zu analysieren. Sie können [Dashboards](https://docs.microsoft.com/azure/log-analytics/log-analytics-dashboards) nutzen und Ihre wichtigsten Suchvorgänge mit grafischen Ansichten anpassen. Darüber hinaus bieten [Lösungen](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) zusätzliche Funktionalität und Analysemöglichkeiten.

![Analysetools](./media/azure-threat-detection/azure-threat-detection-fig6.png)

Lösungen fügen Log Analytics weitere Funktionalität hinzu. Sie werden primär in der Cloud ausgeführt und ermöglichen die Analyse von Daten, die im OMS-Repository gesammelt wurden. Sie können auch neue Datensatztypen definieren, die gesammelt und mit Protokollsuchvorgängen oder über eine zusätzliche Benutzeroberfläche analysiert werden können, die von der Lösung im OMS-Dashboard bereitgestellt wird.
„Security and Audit“ (Sicherheit und Überwachung) ist ein Beispiel für diese Arten von Lösungen.



### <a name="automation--control-alert-on-security-configuration-drifts"></a>Automation & Control: Warnung zu Abweichungen von der Sicherheitskonfiguration

Azure Automation automatisiert Verwaltungsprozesse mithilfe von Runbooks, die auf PowerShell basieren und in der Azure-Cloud ausgeführt werden. Runbooks können auch auf einem Server in Ihrem lokalen Rechenzentrum ausgeführt werden, um lokale Ressourcen zu verwalten. Azure Automation stellt die Konfigurationsverwaltung mit PowerShell DSC (Desired State Configuration) bereit.

![Azure-Automatisierung](./media/azure-threat-detection/azure-threat-detection-fig7.png)

Sie können DSC-Ressourcen erstellen und verwalten, die in Azure gehostet werden, und sie auf Cloud- und lokale Systeme anwenden, um ihre Konfiguration zu definieren und automatisch zu erzwingen oder Berichte zu Abweichungen abzurufen, die dabei helfen sicherzustellen, dass Sicherheitskonfigurationen die Richtlinie einhalten.

## <a name="azure-security-center"></a>Azure Security Center

Azure Security Center unterstützt Sie beim Schützen Ihrer Azure-Ressourcen. Es bietet eine integrierte Sicherheitsüberwachung und Richtlinienverwaltung für Ihre Azure-Abonnements. Innerhalb des Diensts können Sie Richtlinien nicht nur für Ihre Azure-Abonnements definieren, sondern auch für [Ressourcengruppen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal), sodass Sie differenzierter vorgehen können.

![Azure Security Center](./media/azure-threat-detection/azure-threat-detection-fig8.png)

Microsoft-Sicherheitsexperten suchen ständig nach neuen Bedrohungen. Sie haben Zugriff auf umfassende Telemetriedaten, die auf der globalen Präsenz von Microsoft in der Cloud und in lokalen Umgebungen basieren. Aufgrund dieser weit reichenden und verschiedenartigen Sammlung von Datasets kann Microsoft neue Angriffsmuster und Trends für seine lokalen Privatkunden- und Unternehmensprodukte sowie für seine Onlinedienste erkennen.

Auf diese Weise können die Erkennungsalgorithmen von Security Center schnell aktualisiert werden, wenn Angreifer neue und immer anspruchsvollere Exploit-Verfahren nutzen. So können Sie mit der rasanten Entwicklung von Bedrohungen Schritt halten.

![Security Center](./media/azure-threat-detection/azure-threat-detection-fig9.jpg)

Bei der Bedrohungserkennung von Security Center werden automatisch Sicherheitsinformationen von Ihren Azure-Ressourcen, aus dem Netzwerk und von verbundenen Partnerlösungen gesammelt.  Diese Informationen, bei denen es sich um korrelierende Informationen aus mehreren Quellen handelt, werden analysiert, um Bedrohungen zu identifizieren.
Sicherheitswarnungen werden in Security Center zusammen mit Empfehlungen zur Lösung der Bedrohung priorisiert.

Für Security Center werden professionelle Sicherheitsanalysen genutzt, die weit über signaturbasierte Ansätze hinausgehen. Bahnbrechende Weiterentwicklungen der Big Data- und [Machine Learning](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/)-Technologien kommen zum Einsatz, um Ereignisse im gesamten Cloudfabric auszuwerten. So lassen sich Bedrohungen erkennen, die bei Verwendung von manuellen Konzepten nicht identifiziert werden könnten, und die Entwicklung von Angriffen kann vorhergesagt werden. Zu diesen Sicherheitsanalysen gehört Folgendes.

### <a name="threat-intelligence"></a>Threat Intelligence

Microsoft verfügt über eine große Menge von Informationen zu globalen Bedrohungen.
Die Telemetriedaten stammen aus mehreren Quellen, z. B. Azure, Office 365, Microsoft CRM Online, Microsoft Dynamics AX, outlook.com, MSN.com, Microsoft Digital Crimes Unit (DCU) und Microsoft Security Response Center (MSRC).

![Threat Intelligence](./media/azure-threat-detection/azure-threat-detection-fig10.jpg)

Die Sicherheitsexperten erhalten außerdem Informationen zu Bedrohungen, die zwischen großen Cloudanbietern ausgetauscht werden, und haben Threat Intelligence-Feeds von Drittanbietern abonniert. Azure Security Center kann diese Informationen verwenden, um Sie vor Bedrohungen durch bekannte Angreifer zu warnen. Beispiele hierfür sind:

-   **Nutzen des Machine Learning-Potenzials**: Azure Security Center hat Zugriff auf eine riesige Menge an Daten zur Cloudnetzwerkaktivität, die dazu verwendet werden können, um Bedrohungen zu erkennen, die auf Ihre Azure-Bereitstellungen ausgerichtet sind. Beispiel:

-   **Brute-Force-Erkennungen**: Machine Learning wird dazu verwendet, um ein Verlaufsmuster von Remotezugriffsversuchen zu erstellen, mit denen Brute-Force-Angriffe auf SSH-, RDP- und SQL-Ports erkannt werden können.

-   **Ausgehende DDoS- und Botnet-Erkennung**: Ein häufiges Ziel bei Angriffen auf Cloudressourcen ist es, die Verarbeitungsleistung dieser Ressourcen zum Ausführen von anderen Angriffen zu nutzen.

-   **Neue Server und virtuelle Computer für Verhaltensanalysen**: Nachdem ein Server oder virtueller Computer gefährdet ist, verwenden Angreifer eine Vielzahl von Techniken, um schädlichen Code auf diesem System auszuführen, wobei die Erkennung vermieden und Persistenz sichergestellt wird sowie Sicherheitskontrollen verhindert werden.

-   **Bedrohungserkennung von Azure SQL-Datenbank**: Die Bedrohungserkennung für Azure SQL-Datenbank, die anomale Datenbankaktivitäten identifiziert, indem ungewöhnliche und potenziell schädliche Zugriffsversuche auf Datenbanken (Exploit) angezeigt werden.

### <a name="behavioral-analytics"></a>Verhaltensanalyse

Die Verhaltensanalyse ist ein Verfahren, bei dem Daten mit einer Sammlung bekannter Muster analysiert und verglichen werden. Bei diesen Mustern handelt es sich aber nicht nur um einfache Signaturen. Sie werden anhand von komplexen Machine Learning-Algorithmen bestimmt, die auf große Datasets angewendet werden.

![Verhaltensanalyse](./media/azure-threat-detection/azure-threat-detection-fig11.jpg)


Außerdem werden sie anhand einer sorgfältigen Analyse von schädlichem Verhalten durch erfahrene Analysten bestimmt. Azure Security Center kann die Verhaltensanalyse verwenden, um kompromittierte Ressourcen basierend auf der Analyse der Protokolle von virtuellen Computern, virtuellen Netzwerkgeräten, Fabric-Protokolle, Absturzabbilder und anderen Quellen zu identifizieren.

Außerdem ist eine Korrelation mit anderen Signalen vorhanden, damit weitere Beweise für eine größere Aktion ermittelt werden können. So können Ereignisse identifiziert werden, die mit vorhandenen Indikatoren für eine Kompromittierung übereinstimmen.

Beispiele hierfür sind:
-   **Ausführung von verdächtigen Prozessen**: Angreifer nutzen verschiedene Verfahren, um unbemerkt Schadsoftware auszuführen. Beispielsweise können Angreifer Schadsoftware mit dem Namen regulärer Systemdateien versehen, diese dann aber an einem anderen Speicherort ablegen. Alternativ können sie einen Namen verwenden, der dem Namen einer unbedenklichen Datei ähnelt, oder die tatsächliche Dateierweiterung verschleiern. Security Center modelliert Prozessverhalten und überwacht die Prozessausführung, um Ausreißer dieser Art zu erkennen.

-   **Versteckte Schadsoftware und versuchte Ausnutzung von Schwachstellen**: Professionelle Schadsoftware kann herkömmliche Antischadsoftware-Produkte umgehen, indem sie entweder nie auf den Datenträger schreibt oder auf dem Datenträger gespeicherte Softwarekomponenten verschlüsselt. Schadsoftware dieser Art kann aber mithilfe der Arbeitsspeicheranalyse erkannt werden, da die Schadsoftware Spuren im Arbeitsspeicher hinterlassen muss, um funktionieren zu können. Beim Absturz von Software wird in einem Absturzabbild ein Teil des Arbeitsspeichers zum Zeitpunkt des Absturzes erfasst. Indem die Arbeitsspeicherdaten im Absturzabbild analysiert werden, kann Azure Security Center Verfahren erkennen, die für folgende Zwecke verwendet werden: Ausnutzen von Schwachstellen in Software, Zugreifen auf vertrauliche Daten und Bewegen auf einem kompromittierten Computer ohne Auswirkung auf die Leistung des Computers.

-   **Seitwärtsbewegung und interne Aufklärung**: Um wertvolle Daten in einem kompromittierten Netzwerk ausfindig machen und abschöpfen zu können, versuchen Angreifer häufig, vom kompromittierten Computer aus auf andere Computer im gleichen Netzwerk zuzugreifen. Security Center überwacht die Verarbeitungs- und Anmeldeaktivitäten, um versuchte Aktionen aufzudecken, mit denen Angreifer ihre Basis im Netzwerk ausbauen möchten, z. B. Ausführung von Remotebefehlen im Netzwerk und Kontoauflistung.

-   **Schädliche PowerShell-Skripts**: PowerShell wird von Angreifern verwendet, um mit den verschiedenen Zielen Schadcode auf virtuellen Zielcomputern auszuführen. Security Center untersucht die PowerShell-Aktivitäten auf Beweise für verdächtige Vorgänge.

-   **Ausgehende Angriffe**: Angreifer nehmen häufig Cloudressourcen ins Visier, um diese zur Durchführung weiterer Angriffe zu nutzen. Kompromittierte virtuelle Computer können beispielsweise verwendet werden, um Brute-Force-Angriffe auf andere virtuelle Computer zu starten, Spam zu senden oder nach offenen Ports und anderen Geräten im Internet zu suchen. Indem Machine Learning-Verfahren auf den Netzwerkdatenverkehr angewendet werden, kann Security Center erkennen, wenn ausgehende Netzwerkkommunikation außerhalb der Norm liegt. In Bezug auf Spam korreliert Security Center außerdem ungewöhnlichen E-Mail-Datenverkehr mit Informationen aus Office 365, um zu ermitteln, ob die E-Mail vermutlich schädlich oder das Ergebnis einer legitimen E-Mail-Kampagne ist.

### <a name="anomaly-detection"></a>Erkennung von Anomalien

Azure Security Center nutzt auch die Anomalieerkennung, um Bedrohungen zu identifizieren. Im Gegensatz zur Verhaltensanalyse (basiert auf bekannten Mustern, die aus großen Datasets abgeleitet werden) ist die Anomalieerkennung „personalisierter“ und nutzt Baselines, die speziell für Ihre Bereitstellungen gelten. Machine Learning wird angewendet, um die normale Aktivität für Ihre Bereitstellungen zu ermitteln. Anschließend werden Regeln generiert, um Ausreißerbedingungen zu definieren, die ein Sicherheitsereignis darstellen können. Hier ist ein Beispiel angegeben:

-   **Eingehende RDP/SSH-Brute-Force-Angriffe**: Es kann sein, dass Ihre Bereitstellungen gleichzeitig stark ausgelastete virtuelle Computer mit vielen täglichen Anmeldungen und andere virtuelle Computer mit wenigen oder gar keinen Anmeldungen umfassen. Azure Security Center kann eine Baseline der Anmeldeaktivität für diese virtuellen Computer ermitteln und anhand von Machine Learning-Verfahren definieren, welche Aktionen normale Anmeldeaktivitäten sind. Wenn es Abweichungen bei der Baseline gibt, die für anmeldungsbezogene Eigenschaften definiert ist, wird möglicherweise eine Warnung generiert. Auch hier wird wieder per Machine Learning-Verfahren ermittelt, was relevant ist.

### <a name="continuous-threat-intelligence-monitoring"></a>Fortlaufende Threat Intelligence-Überwachung

Azure Security Center arbeitet mit Sicherheitsforschungs- und Data Science-Teams auf der ganzen Welt, die ständig Änderungen der Bedrohungslandschaft überwachen. Dies umfasst Folgendes:

-   **Threat Intelligence-Überwachung**: Informationen zu Bedrohungen (Threat Intelligence) beinhalten Mechanismen, Indikatoren, Auswirkungen und nützliche Hinweise zu vorhandenen oder neuen Bedrohungen. Diese Informationen werden in der Sicherheitscommunity bereitgestellt, und Microsoft überwacht fortlaufend Threat Intelligence-Feeds von internen und externen Quellen.

-   **Signalaustausch**: Die Erkenntnisse der Sicherheitsteams aus dem großen Microsoft-Portfolio mit Clouddiensten und lokalen Diensten, Servern und Clientendpunkt-Geräten werden ausgetauscht und analysiert.

-   **Microsoft-Sicherheitsexperten**: Ständiger Austausch mit Teams von Microsoft, die sich mit speziellen Sicherheitsfeldern beschäftigen, z. B. Forensik und Erkennung von Webangriffen.

-   **Erkennungsoptimierung**: Algorithmen werden für echte Kundendatasets ausgeführt, und Sicherheitsexperten arbeiten mit Kunden zusammen, um die Ergebnisse zu prüfen. Richtige und falsche Positivmeldungen werden verwendet, um Machine Learning-Algorithmen zu verfeinern.

Diese kombinierten Verfahren führen zu neuen und verbesserten Erkennungsergebnissen, von denen Sie sofort profitieren können. Sie müssen dabei nichts unternehmen.

## <a name="advanced-threat-detection-features---other-azure-services"></a>Features zur erweiterten Bedrohungserkennung – andere Azure-Dienste

### <a name="virtual-machine-microsoft-antimalware"></a>Virtueller Computer: Microsoft Antimalware

[Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) für Azure ist eine Lösung mit einem einzelnen Agent für Anwendungen und Mandantenumgebungen, die im Hintergrund ohne Eingreifen des Benutzers ausgeführt wird. Sie können Schutz basierend auf den Anforderungen der Anwendungsworkloads bereitstellen, entweder mit der einfachen Konfiguration, die Schutz durch die Standardeinstellungen bietet, oder mit einer erweiterten benutzerdefinierten Konfiguration, einschließlich Antischadsoftwareüberwachung. Azure-Antischadsoftware ist eine Sicherheitsoption für Azure Virtual Machines, die automatisch auf allen virtuellen Azure-PaaS-Computern installiert wird.

**Features von Azure zum Bereitstellen und Aktivieren von Microsoft Antimalware für Ihre Anwendungen**

#### <a name="microsoft-antimalware-core-features"></a>Kernfeatures von Microsoft Antimalware

-   **Echtzeitschutz**: Überwacht die Aktivitäten in Clouddiensten und auf virtuellen Computern, um die Ausführung von Schadsoftware zu erkennen und zu blockieren.

-   **Geplantes Scannen**: Führt regelmäßig gezielte Scans aus, um Schadsoftware, einschließlich aktiv ausgeführter Programme, zu erkennen.

-   **Schadsoftwarebehandlung**: Führt automatisch Aktionen für erkannte Schadsoftware aus, z. B. das Löschen schädlicher Dateien, das Unter-Quarantäne-Stellen schädlicher Dateien und das Bereinigen von schädlichen Registrierungseinträgen.

-   **Signaturaktualisierungen**: Installiert automatisch mit einer vordefinierten Häufigkeit die neuesten Schutzsignaturen (Virendefinitionen), um sicherzustellen, dass der Schutz auf dem neuesten Stand ist.

-   **Aktualisierungen des Antimalware-Moduls**: Aktualisiert automatisch das Microsoft Antimalware-Modul.

-   **Aktualisierungen der Antimalware-Plattform**: Aktualisiert automatisch die Microsoft Antimalware-Plattform.

-   **Aktiver Schutz**: Übermittelt Telemetriemetadaten über erkannte Bedrohungen und verdächtige Ressourcen an Microsoft Azure, um eine schnelle Reaktion auf die zunehmenden Bedrohungen sicherzustellen und eine synchrone Echtzeitübermittlung von Signaturen über MAPS (Microsoft Active Protection Service) zu ermöglichen.

-   **Übermittlung von Stichproben**: Übermittelt Stichproben an den Microsoft Antimalware-Dienst, um den Dienst zu optimieren und die Problembehandlung zu ermöglichen.

-   **Ausschlüsse**: Ermöglicht Anwendungs- und Dienstadministratoren, aus Leistungs- und/oder anderen Gründen bestimmte Dateien, Prozesse und Laufwerke vom Schutz und von Scans auszuschließen.

-   **Antimalware-Ereigniserfassung**: Zeichnet die Integrität des Antimalware-Diensts, verdächtige Aktivitäten und durchgeführte Wiederherstellungsaktionen im Ereignisprotokoll des Betriebssystems auf und erfasst sie im Azure Storage-Konto des Kunden.

### <a name="azure-sql-database-threat-detection"></a>Bedrohungserkennung von Azure SQL-Datenbank

[Bedrohungserkennung von Azure SQL-Datenbank](https://azure.microsoft.com/blog/azure-sql-database-threat-detection-your-built-in-security-expert/) ist ein neues Security Intelligence-Feature, das im Azure SQL-Datenbankdienst integriert ist. Die Bedrohungserkennung von Azure SQL-Datenbank arbeitet rund um die Uhr, um Informationen zu anomalen Datenbankaktivitäten zu erhalten, Profile über diese Aktivitäten zu erstellen und sie zu erkennen, um potenzielle Bedrohungen für die Datenbank zu identifizieren.

Sicherheitsbeauftragte oder andere zugewiesene Administratoren können eine sofortige Benachrichtigung über verdächtige Datenbankaktivitäten erhalten, sobald diese auftreten. Jede Benachrichtigung enthält Details zur verdächtigen Aktivität und empfiehlt die Vorgehensweise zur weiteren Untersuchung und Abwendung der Bedrohung.

Derzeit erkennt die Bedrohungserkennung von Azure SQL-Datenbank potenzielle Sicherheitsrisiken und Angriffe mit Einschleusung von SQL-Befehlen sowie anomale Datenbankzugriffsmuster.

Nach dem Eingang einer E-Mail-Benachrichtigung zur Bedrohungserkennung können Benutzer mithilfe des Deep-Links in der E-Mail zu den relevanten Überwachungsdatensätzen navigieren und diese anzeigen. Der Deep-Link öffnet eine Überwachungsansicht und/oder eine vorkonfigurierte Excel-Vorlage für die Überwachung, die die relevanten Überwachungdatensätze gemäß Folgendem für den Zeitpunkt des verdächtigen Ereignisses anzeigt:
-   Überwachungsspeicher für die Datenbank bzw. den Server mit den anomalen Datenbankaktivitäten

-   Relevante Überwachungsspeichertabellen, die zum Zeitpunkt des Ereignisses verwendet wurden, um das Überwachungsprotokoll zu schreiben

-   Überwachungsdatensätze für die nachfolgende Stunde nach dem Auftreten des Ereignisses.

-   Überwachungsdatensätze mit ähnlicher Ereignis-ID zum Zeitpunkt des Ereignisses (optional für einige Erkennungsmodule)

Erkennungsmodule für SQL-Datenbankbedrohungen verwenden eine der folgenden Methoden für die Erkennung:

-   **Deterministische Erkennung**: Erkennt verdächtige Muster (regelbasiert) in den SQL-Clientabfragen, die bekannten Angriffen entsprechen. Diese Methode weist eine hohe Erkennungsrate und wenig falsch positive Ergebnisse auf, jedoch ist die Abdeckung begrenzt, da sie in die Kategorie „atomare Erkennungen“ fällt.

-   **Verhaltensbedingte Erkennung**: Erkennt anormale Aktivitäten, bei denen es sich um anormales Verhalten für die Datenbank handelt, das während der letzten 30 Tage nicht erkannt wurde.  Beispiele für anormale Aktivitäten eines SQL-Clients können Steigerungen bei den fehlerhaften Anmeldungen/Abfragen, große Menge extrahierter Daten, ungewöhnliche kanonische Abfragen und unbekannte IP-Adressen für den Datenbankzugriff sein.

### <a name="application-gateway-web-application-firewall"></a>Web Application Firewall für Application Gateway

[Web Application Firewall](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-web-application-firewall) ist ein Feature von [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-webapplicationfirewall-overview), das Schutz für Webanwendungen bietet, die Application Gateway für ADC-Standardfunktionen ([Application Delivery Control](https://kemptechnologies.com/in/application-delivery-controllers), Steuerung der Anwendungsbereitstellung) nutzen. Web Application Firewall schützt sie vor den nach [OWASP 10 häufigsten Web-Sicherheitslücken](https://www.owasp.org/index.php/Top_10_2010-Main).

![Web Application Firewall für Application Gateway](./media/azure-threat-detection/azure-threat-detection-fig13.png)

-   Schutz vor Einschleusung von SQL-Befehlen

-   Schutz vor websiteübergreifenden Skripts

-   Schutz vor allgemeinen Webangriffen wie Befehlseinschleusung, HTTP Request Smuggling, HTTP Response Splitting und Remote File Inclusion

-   Schutz vor Verletzungen des HTTP-Protokolls

-   Schutz vor HTTP-Protokollanomalien, z.B. fehlende user-agent- und accept-Header des Hosts

-   Verhindern von Bots, Crawlern und Scannern

-   Erkennung häufiger Fehler bei der Anwendungskonfiguration (d. h. Apache, IIS usw.)

Das Konfigurieren einer WAF auf dem Anwendungsgateway bietet die folgenden Vorteile:

-   Schützen Sie Ihre Webanwendung vor Sicherheitsrisiken und Angriffen im Web, ohne den Back-End-Code zu verändern.

-   Schützen Sie mehrere Webanwendungen gleichzeitig hinter einem Anwendungsgateway. Application Gateway unterstützt das Hosten von bis zu 20 Websites hinter einem einzelnen Gateway, die alle vor Webangriffen geschützt werden können.

-   Überwachen Sie Ihre Webanwendung auf Angriffe, indem Sie Echtzeitberichte verwenden, die anhand von WAF-Protokollen des Anwendungsgateways generiert werden.

-   Bestimmte Kompatibilitätssteuerelemente erfordern, dass alle über das Internet zugänglichen Endpunkte durch eine WAF-Lösung geschützt werden müssen. Durch die Verwendung von Application Gateway mit aktivierter WAF können Sie diese Kompatibilitätsauflagen erfüllen.

### <a name="anomaly-detection--an-api-built-with-azure-machine-learning"></a>Anomalieerkennung – mit Azure Machine Learning erstellte API

Die Anomalieerkennung ist eine mit Azure Machine Learning erstellte API, die für die Erkennung verschiedener Arten von anomalen Mustern in Ihren Zeitreihendaten hilfreich ist. Die API weist jedem Datenpunkt in der Zeitreihe einen Anomaliewert zu, der zum Generieren von Warnungen, zur Überwachung über Dashboards oder zum Verbinden mit Ihren Ticketausstellungssystem verwendet werden kann.

Die [API zur Anomalieerkennung](https://docs.microsoft.com/azure/machine-learning/machine-learning-apps-anomaly-detection-api) kann die folgenden Arten von Anomalien in Zeitreihendaten erkennen:

-   **Spitzen und Abfälle**: Beim Überwachen der Anzahl von fehlerhaften Anmeldeversuchen bei einem Dienst oder der Anzahl von Auscheckvorgängen in einer E-Commerce-Website könnten ungewöhnliche Spitzen oder Abfälle z. B. auf Sicherheitsangriffe oder Dienstunterbrechungen hinweisen.

-   **Positive und negative Trends**: Beim Überwachen der Speicherauslastung bei der Verarbeitung kann z. B. die starke Abnahme des freien Speicherplatzes ein Anzeichen für einen potenziellen Speicherverlust sein. Bei der Überwachung der Länge der Dienstwarteschlange kann ein dauerhafter Aufwärtstrend möglicherweise auf ein zugrunde liegendes Softwareproblem hinweisen.

-   **Niveauänderungen und Änderungen am dynamischen Bereich von Werten**: Niveauänderungen bei den Latenzzeiten eines Diensts nach einem Dienstupgrade oder ein geringerer Grad an Ausnahmen nach dem Upgrade können für die Überwachung interessant sein.

Die Machine Learning-basierte API ermöglicht Folgendes:

-   **Flexible und robuste Erkennung**: Die Modelle zur Erkennung von Anomalien ermöglichen Benutzern die Konfiguration von Empfindlichkeitseinstellungen und die Erkennung von Anomalien über saisonale und nicht saisonale Datasets. Benutzer können das Modell zur Anomalieerkennung anpassen, um die API für die Erkennung entsprechend ihren Anforderungen mehr oder weniger empfindlich einzustellen. Das würde die Erkennung von mehr oder weniger sichtbaren Anomalien in Daten mit und ohne saisonalen Mustern bedeuten.

-   **Skalierbare und rechtzeitige Erkennung**: Die traditionelle Methode zur Überwachung mit vorhandenen Schwellenwerten, die von Experten mit Domänenkenntnissen festgelegt werden, sind teuer und können nicht für Millionen von sich dynamisch ändernden Datasets skaliert werden. Die Modelle zur Anomalieerkennung in dieser API sind angelernt und werden mithilfe von Verlaufsdaten und Echtzeitdaten automatisch optimiert.

-   **Proaktive und handlungsrelevante Erkennung**: Für die frühe Erkennung von Anomalien kann die Erkennung langsamer Trends und von Niveauänderungen angewendet werden. Die früh erkannten anormalen Signale können dazu verwendet werden, um Benutzer anzuweisen, Untersuchungen durchzuführen und auf Problembereiche zu reagieren.  Darüber hinaus können zusätzlich zu diesem API-Dienst zur Anomalieerkennung entsprechende Analysemodelle für Hauptursachen und Benachrichtigungstools entwickelt werden.

Die API zur Erkennung von Anomalien ist eine effektive und effiziente Lösung für eine Vielzahl von Szenarien wie Dienstintegrität und KPI-Überwachung, IoT, Leistungsüberwachung und Überwachung des Netzwerkdatenverkehrs. Es folgen einige verbreitete Szenarien, in denen diese API hilfreich sein kann:
- IT-Abteilungen benötigen Tools zum zeitnahen Überwachen von Ereignissen, Fehlercodes, Nutzungsprotokollen und Leistung (CPU, Arbeitsspeicher usw.).

-   Online Commerce-Websites möchten Kundenaktivitäten, Seitenaufrufe, Klicks usw. nachverfolgen.

-   Versorgungsunternehmen möchten den Verbrauch von Wasser, Gas, Strom und anderen Ressourcen nachverfolgen.

-   Gebäudemanagementdienste möchten Temperatur, Feuchtigkeit, Datenverkehr usw. überwachen.

-   IoT/Hersteller möchten Sensordaten in Zeitreihen verwenden, um Arbeitsabläufe, Qualität usw. zu überwachen.

-   Dienstanbieter wie Callcenter müssen Trends bei Serviceanforderungen, Ereignisvolumen, Warteschlangenlängen usw. überwachen.

-   Business Analytics-Gruppen möchten anormale Bewegungen bei den Leistungsindikatoren von Unternehmen in Echtzeit überwachen (z. B. Umsatzvolumen, Kundenmeinungen, Preise).

### <a name="cloud-app-security"></a>Cloud App Security

[Cloud App Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security) ist eine wichtige Komponente des Microsoft Cloud Security-Stapels. Es ist eine umfassende Lösung, die Ihre Organisation bei der optimalen Nutzung von Cloudanwendungen unterstützen kann, während Sie jedoch durch eine verbesserte Aktivitätsübersicht die Kontrolle behalten. Sie hilft zudem dabei, den Schutz wichtiger Daten in Cloudanwendungen zu erhöhen.

Mithilfe von Tools, die Sie dabei unterstützen, Schatten-IT zu enthüllen, Risiken zu bewerten, Richtlinien durchzusetzen, Aktivitäten zu untersuchen und Bedrohungen zu stoppen, kann Ihr Unternehmen sicherer zur Cloud wechseln, während es gleichzeitig die Kontrolle über wichtige Daten behält.

<table style="width:100%">
 <tr>
   <td>Entdecken</td>
   <td>Enthüllen Sie Schatten-IT mit Cloud App Security. Erhalten Sie den Überblick, indem Sie Apps, Aktivitäten, Benutzer, Daten und Dateien in Ihrer Cloudumgebung entdecken. Entdecken Sie Apps von Drittanbietern, die mit Ihrer Cloud verbunden sind.</td>
 </tr>
 <tr>
   <td>Untersuchen</td>
   <td>Untersuchen Sie Ihre Cloud-Apps mithilfe von Tools für die Cloudforensik, um ausführliche Informationen zu riskanten Apps, bestimmten Benutzern und Dateien in Ihrem Netzwerk zu erhalten. Suchen Sie in den aus der Cloud erfassten Daten nach Mustern. Generieren Sie Berichte, um Ihre Cloud zu überwachen.</td>

 </tr>
 <tr>
   <td>Kontrolle</td>
   <td>Verringern Sie das Risiko, indem Sie Richtlinien und Warnungen festlegen, um die volle Kontrolle über den Netzwerkdatenverkehr in der Cloud zu erhalten. Verwenden Sie Cloud App Security, um Ihre Benutzer in sichere, unterstützte Alternativen für Cloud-Apps zu migrieren.</td>

 </tr>
 <tr>
   <td>Schützen</td>
   <td>Verwenden Sie Cloud App Security, um Anwendungen zu unterstützen oder zu verbieten, die Verhinderung von Datenverlust zu erzwingen, Berechtigungen und Freigaben zu kontrollieren sowie benutzerdefinierte Berichte und Warnungen zu generieren.</td>

 </tr>
 <tr>
   <td>Kontrolle</td>
   <td>Verringern Sie das Risiko, indem Sie Richtlinien und Warnungen festlegen, um die volle Kontrolle über den Netzwerkdatenverkehr in der Cloud zu erhalten. Verwenden Sie Cloud App Security, um Ihre Benutzer in sichere, unterstützte Alternativen für Cloud-Apps zu migrieren.</td>

 </tr>
</table>


![Cloud App Security](./media/azure-threat-detection/azure-threat-detection-fig14.png)

Cloud App Security integriert durch Folgendes die Sichtbarkeit in Ihre Cloud:

-   Verwenden von Cloud Discovery zum Zuordnen und Identifizieren Ihrer Cloudumgebung und der Cloud-Apps, die von Ihrem Unternehmen verwendet werden.


-   Unterstützen und Verbieten von Apps in Ihrer Cloud.



-   Verwenden von einfach bereitzustellenden App-Verbindungen, die Anbieter-APIs für die Sichtbarkeit und Governance von Apps verwenden, mit denen Sie eine Verbindung herstellen.

-   Bereitstellen von Unterstützung, damit Sie ständig die Kontrolle behalten, indem Richtlinien festgelegt und dann fortlaufend optimiert werden.

Beim Erfassen der Daten aus diesen Quellen führt Cloud App Security ausgereifte Datenanalysen aus. Sie werden sofort über ungewöhnliche Aktivitäten informiert und erhalten einen umfassenden Einblick in die Cloudumgebung. Sie können eine Richtlinie in Cloud App Security konfigurieren und damit alles in Ihrer Cloudumgebung schützen.

## <a name="third-party-atd-capabilities-through-azure-marketplace"></a>ATD-Funktionen von Drittanbietern über Azure Marketplace

### <a name="web-application-firewall"></a>Web Application Firewall

Web Application Firewall untersucht eingehenden Webdatenverkehr und blockiert die Einschleusung von SQL-Befehlen, websiteübergreifendes Scripting, das Hochladen von Schadsoftware sowie DDoS- und andere Angriffe, die gegen ihre Webanwendungen gerichtet sind. WAF überprüft auch zur Verhinderung von Datenverlust (Data Loss Prevention, DLP) die Antworten von den Back-End-Webservern. Das integrierte Modul für die Zugriffssteuerung ermöglicht es Administratoren, präzise Zugriffssteuerungsrichtlinien für die Authentifizierung, Autorisierung und Kontenführung zu erstellen, die Unternehmen eine strenge Authentifizierung und Benutzerkontrolle ermöglichen.

**Highlights:**
-   Erkennt und blockiert die Einschleusung von SQL-Befehlen, websiteübergreifendes Scripting, das Hochladen von Schadsoftware, sowie DDoS- und andere Angriffe, die gegen Ihre Anwendung gerichtet sind.

-   Authentifizierung und Zugriffssteuerung.

-   Überprüfung des ausgehenden Datenverkehrs, um sensible Daten zu erkennen und ggf. zu maskieren oder zu blockieren, damit die Informationen nicht von unberechtigten Personen eingesehen werden können.

-   Beschleunigt die Übermittlung von Webanwendungsinhalten mithilfe von Funktionen wie Zwischenspeicherung, Komprimierung und anderen Optimierungen des Datenverkehrs.

Nachfolgend finden Sie ein Beispiel für Web Application Firewalls, die in Azure Marketplace verfügbar sind:

[Barracuda Web Application Firewall, Brocade Virtual Web Application Firewall (Brocade vWAF), Imperva SecureSphere und ThreatSTOP IP Firewall.](https://azure.microsoft.com/marketplace/partners/brocade_communications/brocade-virtual-web-application-firewall-templatevtmcluster/)

## <a name="next-steps"></a>Nächste Schritte

- [Azure Security Center-Erkennungsfunktionen](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities)

Mit den erweiterten Erkennungsfunktionen von Azure Security Center können aktive Bedrohungen Ihrer Microsoft Azure-Ressourcen identifiziert werden, und Sie erhalten die Einblicke, die zum schnellen Ergreifen von Maßnahmen erforderlich sind.

- [Bedrohungserkennung von Azure SQL-Datenbank](https://azure.microsoft.com/blog/azure-sql-database-threat-detection-your-built-in-security-expert/)

Die Bedrohungserkennung von Azure SQL-Datenbank hat dabei geholfen, ihre Bedenken hinsichtlich potenzieller Gefahren für ihre Datenbank zu behandeln.

