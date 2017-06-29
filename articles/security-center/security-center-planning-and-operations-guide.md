---
title: "Planungs- und Betriebshandbuch für Security Center | Microsoft Docs"
description: "Dieses Dokument ist hilfreich bei der Planung der Verwendung von Azure Security Center und informiert über Aspekte des täglichen Betriebs."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: f984e4a2-ac97-40bf-b281-2f7f473494c4
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: yurid
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: c502e4363dbaa37455d1aad90d1e9fa855fd09b0
ms.contentlocale: de-de
ms.lasthandoff: 06/17/2017


---
# <a name="azure-security-center-planning-and-operations-guide"></a>Planungs- und Betriebshandbuch für Azure Security Center
Dieses Handbuch ist für IT-Experten, IT-Architekten, Informationssicherheitsanalysten und Cloudadministratoren konzipiert, deren Organisation Azure Security Center verwenden möchte.

>[!NOTE] 
>Ab Anfang Juni 2017 verwendet Security Center den Microsoft Monitoring Agent zum Sammeln und Speichern von Daten. Weitere Informationen finden Sie unter [Azure Security Center – Plattformmigration](security-center-platform-migration.md). Die Informationen in diesem Artikel stellen Security Center-Funktionen nach dem Umstieg auf den Microsoft Monitoring Agent vor.
>

## <a name="planning-guide"></a>Planungshandbuch
Dieses Handbuch enthält eine Reihe von Schritten und Aufgaben, mit denen Sie die Verwendung von Security Center auf Grundlage der Sicherheitsanforderungen und des Cloudverwaltungsmodells Ihres Unternehmens optimieren können. Für eine optimale Nutzung von Security Center ist es wichtig zu verstehen, wie der Dienst von den verschiedenen Personen oder Teams in Ihrer Organisation genutzt wird, um die Anforderungen in puncto sichere Entwicklung und sicherer Betrieb, Überwachung, Governance und Reaktionen auf Vorfälle zu erfüllen. Bei der Planung zur Verwendung von Security Center sind insbesondere folgende Aspekte zu berücksichtigen:

* Sicherheitsrollen und Zugriffsteuerungen
* Sicherheitsrichtlinien und -empfehlungen
* Datensammlung und -speicherung
* Kontinuierliche Sicherheitsüberwachung
* Reaktion auf Vorfälle

Im nächsten Abschnitt erfahren Sie, wie Sie jeden dieser Aspekte bei Ihrer Planung berücksichtigen und die Empfehlungen auf der Grundlage Ihrer Anforderungen umsetzen.

> [!NOTE]
> Unter [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md) finden Sie eine Liste mit häufig gestellten Fragen, die in der Entwurfs- und Planungsphase ebenfalls nützlich sein können.
> 

## <a name="security-roles-and-access-controls"></a>Sicherheitsrollen und Zugriffsteuerungen
Je nach Größe und Struktur Ihrer Organisation wird Azure Security Center unter Umständen von mehreren Einzelpersonen und Teams zur Durchführung unterschiedlicher sicherheitsbezogener Aufgaben verwenden. Im folgenden Diagramm sehen Sie ein Beispiel für fiktive Personae sowie ihre jeweiligen Rollen und Sicherheitsaufgaben:

![Rollen](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig01-new.png)

Security Center ermöglicht es den Personen, diese unterschiedlichen Aufgaben auszuführen. Beispiel:

**Jeff (Cloudworkloadbesitzer)**

* Verwaltet eine Cloudworkload und die dazugehörigen Ressourcen
* Verantwortlich für die Implementierung und Verwaltung von Schutzmaßnahmen in Übereinstimmung mit der Sicherheitsrichtlinie des Unternehmens

**Ellen (CISO/CIO)**

* Verantwortlich für alle Sicherheitsaspekte im Unternehmen
* Möchte die Sicherheitssituation des Unternehmens in Bezug auf Cloudworkloads verstehen
* Muss über größere Angriffe und Risiken informiert werden

**David (IT-Sicherheit)**

* Legt die Sicherheitsrichtlinien des Unternehmens fest, um für die richtigen Schutzmaßnahmen zu sorgen
* Überwacht die Einhaltung von Richtlinien
* Erstellt Berichte für die Geschäftsleitung oder Prüfer

**Judy (Security Operations)**

* Überwacht und reagiert auf Sicherheitswarnungen rund um die Uhr
* Eskaliert zu Cloudworkloadbesitzer oder IT Security Analyst

**Sam (Security Analyst)**

* Untersucht Angriffe
* Arbeitet zusammen mit dem Cloudworkloadbesitzer an der Lösung 

Security Center verwendet die [rollenbasierte Zugriffssteuerung](../active-directory/role-based-access-control-configure.md) (Role-Based Access Control, RBAC). Dabei werden [integrierte Rollen](../active-directory/role-based-access-built-in-roles.md) bereitgestellt, die Benutzern, Gruppen und Diensten in Azure zugewiesen werden können. Wenn Benutzer Security Center öffnen, werden nur Informationen zu den Ressourcen angezeigt, auf die jeweils Zugriff besteht. Dies bedeutet, dass dem Benutzer die Rolle „Besitzer“, „Mitwirkender“ oder „Leser“ für das Abonnement oder die Ressourcengruppe einer Ressource zugewiesen wird. Neben diesen Rollen gibt es zwei spezifische Security Center-Rollen:

- **Benutzer mit Leseberechtigung für Sicherheitsfunktionen:** Benutzer, die zu dieser Rolle gehören, können Informationen in Security Center anzeigen. Dazu gehören Empfehlungen, Warnungen, Richtlinien und die Integrität. Änderungen können sie jedoch nicht vornehmen.
- **Sicherheitsadministrator:** Es gelten die gleichen Berechtigungen wie bei einem Benutzer mit Leseberechtigung für Sicherheitsfunktionen, zusätzlich sind jedoch auch Rechte zum Ändern der Sicherheitsrichtlinie und zum Verwerfen von Empfehlungen und Warnungen vorhanden.

Die oben beschriebenen Security Center-Rollen haben keinen Zugriff auf andere Dienstbereiche von Azure wie Storage, Web & Mobile oder Internet der Dinge (IoT).  

> [!NOTE]
> Ein Benutzer muss mindestens ein Abonnementadministrator oder ein Ressourcengruppenbesitzer oder -mitwirkender sein, damit Security Center in Azure sichtbar ist. 
> 
> 

Für die Personae im obigen Diagramm wären die folgenden RBAC-Einstellungen erforderlich:

**Jeff (Cloudworkloadbesitzer)**

* Ressourcengruppenbesitzer/-mitarbeiter

**David (IT-Sicherheit)**

* Abonnementbesitzer/-mitarbeiter oder Sicherheitsadministrator

**Judy (Security Operations)**

* Abonnementleser oder Benutzer mit Leseberechtigung für Sicherheitsfunktionen zum Anzeigen von Warnungen
* Abonnementbesitzer/-mitarbeiter oder Sicherheitsadministrator zum Verwerfen von Warnungen

**Sam (Security Analyst)**

* Abonnementleser zum Anzeigen von Warnungen
* Abonnementbesitzer/-mitarbeiter zum Verwerfen von Warnungen
* Möglicherweise ist ein Zugriff auf den Arbeitsbereich erforderlich.

Einige weitere wichtige Informationen, die es zu berücksichtigen gilt:

* Nur Abonnementbesitzer/-mitwirkende und Sicherheitsadministratoren können eine Sicherheitsrichtlinie bearbeiten.
* Nur Abonnement- und Ressourcengruppenbesitzer und Mitwirkende können Sicherheitsempfehlungen für eine Ressource umsetzen.

Bei der Planung der Zugriffssteuerung mit RBAC für Security Center müssen Sie zunächst ermitteln, wer in Ihrer Organisation Security Center verwendet. Außerdem muss ermittelt werden, welche Aufgaben diese Personen jeweils ausführen, und dann müssen die RBAC-Einstellungen entsprechend konfiguriert werden.

> [!NOTE]
> Es empfiehlt sich, den Benutzern eine Rolle zuzuweisen, die jeweils nur so viele Berechtigungen umfasst wie für die Erfüllung ihrer Aufgaben erforderlich sind. So sollte etwa Benutzern, die nur Informationen zum Sicherheitszustand von Ressourcen anzeigen, aber keine Aktionen durchführen müssen (also beispielsweise keine Empfehlungen umsetzen oder Richtlinien bearbeiten), nur die Leserrolle zugewiesen werden.
> 
> 

## <a name="security-policies-and-recommendations"></a>Sicherheitsrichtlinien und -empfehlungen
In einer Sicherheitsrichtlinie wird der Satz von Sicherheitsmechanismen definiert, die für Ressourcen in dem angegebenen Abonnement zu empfehlen sind. In Security Center definieren Sie Richtlinien auf Grundlage der Sicherheitsanforderungen Ihres Unternehmens sowie auf Grundlage der Art von Anwendungen oder der Vertraulichkeit der Daten.

Auf Abonnementebene aktivierte Richtlinien werden automatisch an alle Ressourcen innerhalb des Abonnements verteilt. Dies ist im folgenden Diagramm dargestellt:

![Sicherheitsrichtlinien](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig2-newUI.png)

> [!NOTE]
> Welche Richtlinien geändert wurden, erfahren Sie in den [Azure-Überwachungsprotokollen](https://blogs.msdn.microsoft.com/cloud_solution_architect/2015/03/10/audit-logs-for-azure-events/). Richtlinienänderungen werden immer in Azure-Überwachungsprotokollen protokolliert.
> 
> 

### <a name="security-recommendations"></a>Sicherheitsempfehlungen
Ermitteln Sie vor dem Konfigurieren der Sicherheitsrichtlinien zunächst anhand der [Sicherheitsempfehlungen](security-center-recommendations.md), ob diese Richtlinien für Ihre verschiedenen Abonnements und Ressourcengruppen geeignet sind. Es ist auch wichtig zu verstehen, welche Aktionen aufgrund der [Sicherheitsempfehlungen](https://docs.microsoft.com/en-us/azure/security-center/security-center-recommendations) ausgeführt werden sollten und wer in Ihrer Organisation für die Überwachung neuer Empfehlungen und die Durchführung der erforderlichen Schritte verantwortlich ist.

In Security Center wird die Bereitstellung von Sicherheitskontaktinformationen für Ihr Azure-Abonnement empfohlen. Microsoft kontaktiert Sie anhand dieser Informationen, wenn Microsoft Security Response Center (MSRC) feststellt, dass Personen unrechtmäßig oder unbefugt auf Ihre Kundendaten zugegriffen haben. Weitere Informationen zur Aktivierung dieser Empfehlung finden Sie unter [Bereitstellen von Sicherheitskontaktinformationen in Azure Security Center](security-center-provide-security-contact-details.md) .

## <a name="data-collection-and-storage"></a>Datensammlung und -speicherung
Azure Security Center verwendet den Microsoft Monitoring Agent – derselbe Agent wird auch von der Operations Management Suite und dem Log Analytics-Dienst verwendet –, um sicherheitsrelevante Daten von Ihren virtuellen Computern zu sammeln. Die von diesem Agent gesammelten Daten werden in Ihren Log Analytics-Arbeitsbereichen gespeichert.

### <a name="agent"></a>Agent

Nach dem Aktivieren der Datensammlung in der Sicherheitsrichtlinie wird der Microsoft Monitoring Agent (für [Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-windows-agents) oder [Linux](https://docs.microsoft.com/azure/log-analytics/log-analytics-linux-agents)) auf allen unterstützten Azure-VMs und allen neu erstellten VMs installiert.  Wenn auf dem virtuellen Computer bereits Microsoft Monitoring Agent installiert wurde, nutzt Azure Security Center den installierten Agent. Der Prozess des Agents ist so konzipiert, dass er nicht invasiv ist und nur eine minimale Auswirkung auf die VM-Leistung hat.

Der Microsoft Monitoring Agent für Windows erfordert TCP-Port 443. Weitere Informationen finden Sie im Artikel zur [Problembehandlung](security-center-troubleshooting-guide.md).

Die Datensammlung kann bei Bedarf in der Sicherheitsrichtlinie deaktiviert werden. Da der Microsoft Monitoring Agent möglicherweise durch andere Azure-Dienste zur Verwaltung und Überwachung verwendet wird, wird der Agent jedoch nicht automatisch deinstalliert, wenn Sie die Datensammlung in Security Center deaktivieren. Sie können den Agent bei Bedarf manuell deinstallieren.

> [!NOTE]
> Eine Liste mit unterstützten virtuellen Computern finden Sie unter [Azure Security Center – Häufig gestellte Fragen (FAQ)](security-center-faq.md).
> 

### <a name="workspace"></a>Arbeitsbereich

Die vom Microsoft Monitoring Agent (für Azure Security Center) gesammelten Daten werden in einem vorhandenen Log Analytics-Arbeitsbereich, der mit Ihrem Azure-Abonnement verknüpft ist, oder unter Berücksichtigung des geografischen Standorts des virtuellen Computers in neuen Arbeitsbereichen gespeichert. 

Sie können das Azure-Portal durchsuchen, um eine Liste Ihrer Log Analytics-Arbeitsbereiche (einschließlich aller von Azure Security Center erstellten) anzuzeigen. Für neue Arbeitsbereiche wird eine zugehörige Ressourcengruppe erstellt. Für beide gilt folgende Namenskonvention: 

* Arbeitsbereich: *DefaultWorkspace-[Abonnement-ID]-[Region]*
* Ressourcengruppe: *DefaultResouceGroup-[Region]*

Für Arbeitsbereiche, die von Azure Security Center erstellt wurden, werden die Daten 30 Tage lang beibehalten. Bei vorhandenen Arbeitsbereichen basiert die Vermerkdauer auf dem Tarif des Arbeitsbereichs.

> [!NOTE]
> Microsoft unternimmt große Anstrengungen, um für den Datenschutz und die Sicherheit dieser Daten zu sorgen. Microsoft hält strenge Compliance- und Sicherheitsrichtlinien ein – angefangen bei der Codierung bis hin zum Betreiben von Diensten. Weitere Informationen zur Behandlung von Daten und zum Datenschutz finden Sie unter [Azure Security Center-Datensicherheit](security-center-data-security.md).
> 

## <a name="ongoing-security-monitoring"></a>Kontinuierliche Sicherheitsüberwachung
Nach Konfiguration und Anwendung der Security Center-Empfehlungen müssen Sie sich Gedanken zu den Betriebsprozessen von Security Center machen.

Klicken Sie im Azure-Portal auf **Durchsuchen**, und geben Sie im Feld **Filter** die Zeichenfolge **Security Center** ein, um Security Center aufzurufen. Die Ansichten, die den Benutzern angezeigt werden, hängen von diesen angewendeten Filtern ab. Das Beispiel unten zeigt eine Umgebung mit vielen Problemen:

![Dashboard](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig6.png)

> [!NOTE]
> Der Normalbetrieb wird durch Security Center nicht beeinträchtigt. Die Lösung überwacht passiv Ihre Bereitstellungen und liefert Empfehlungen auf der Grundlage der aktivierten Sicherheitsrichtlinien.

Wenn Sie in Security Center erstmals die Datensammlung für die aktuelle Azure-Umgebung registrieren, sollten Sie sich unbedingt alle Empfehlungen ansehen – entweder auf der Kachel **Empfehlungen** oder pro Ressource (**Compute**, **Netzwerk**, **Speicher und Daten** und **Anwendung**).

Nach der Umsetzung aller Empfehlungen werden im Abschnitt **Prävention** alle behandelten Ressourcen grün dargestellt. Eine kontinuierliche Überwachung wird dann einfacher, da Sie nur bei einer Veränderung der Kacheln für Ressourcensicherheitsintegrität und Empfehlungen aktiv werden müssen.

Der Abschnitt **Erkennung** ist eher reaktionsorientiert. Die Warnungen beziehen sich auf Probleme, die entweder aktuell vorliegen oder in der Vergangenheit aufgetreten sind und bei Security Center-Kontrollen oder von Drittanbietersystemen erkannt wurden. Auf der Kachel „Sicherheitswarnungen“ werden anhand von Balkendiagrammen die Anzahl gefundener Bedrohungserkennungswarnungen pro Tag sowie deren Verteilung auf unterschiedliche Schweregradkategorien (niedrig, mittel, hoch) angezeigt. Weitere Informationen zu Sicherheitswarnungen finden Sie unter [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md).

> [!NOTE]
> Sie können auch Microsoft Power BI zur Visualisierung Ihrer Security Center-Daten verwenden. Weitere Informationen finden Sie unter [Gewinnen von Erkenntnissen aus Azure Security Center-Daten mit Power BI](security-center-powerbi.md).
> 
> 

### <a name="monitoring-for-new-or-changed-resources"></a>Überwachung auf neue oder geänderte Ressourcen
Die meisten Azure-Umgebungen sind dynamisch, sodass unter anderem immer wieder neue Ressourcen hinzukommen oder wegfallen und Konfigurationen geändert werden. Security Center trägt dazu bei, dass Sie den Überblick über den Sicherheitszustand dieser neuen Ressourcen behalten.

Wenn Sie Ihrer Azure-Umgebung neue Ressourcen (virtuelle Computer, SQL-Datenbanken) hinzufügen, erkennt Security Center dies automatisch und beginnt mit der Überwachung ihrer Sicherheit. Hierzu gehören auch PaaS-Webrollen und -Workerrollen. Wenn in der [Sicherheitsrichtlinie](security-center-policies.md)die Datensammlung aktiviert ist, werden für Ihre virtuellen Computer automatisch zusätzliche Überwachungsfunktionen aktiviert.

![Wichtige Bereiche](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig3-newUI.png)

1. Klicken Sie für virtuelle Computer im Abschnitt **Prävention** auf **Compute**. Probleme mit der Aktivierung der Datensammlung oder entsprechenden Empfehlungen werden auf der Registerkarte **Übersicht** und im Abschnitt **Überwachungsempfehlungen** behandelt.
2. In den **Empfehlungen** erfahren Sie, ob (und wenn ja, welche) Sicherheitsrisiken für die neue Ressource gefunden wurden.
3. Auf virtuellen Computern, die der Umgebung neu hinzugefügt werden, ist zunächst häufig nur das Betriebssystem installiert. Der Ressourcenbesitzer benötigt unter Umständen etwas Zeit, um weitere Apps bereitzustellen, die von diesen virtuellen Computern verwendet werden.  Im Idealfall sollten Sie den endgültigen Zweck der Workload kennen. Wird der Computer als Anwendungsserver verwendet? Je nach Zweck der neuen Workload können Sie dann die passende **Sicherheitsrichtlinie** aktivieren. Dies ist der dritte Schritt in diesem Workflow.
4. Wenn der Azure-Umgebung neue Ressourcen hinzugefügt werden, werden auf der Kachel **Sicherheitswarnungen** unter Umständen neue Warnungen angezeigt. Prüfen Sie die Kachel immer auf neue Warnungen, und setzen Sie die Empfehlungen von Security Center um.

Außerdem empfiehlt es sich, regelmäßig den Zustand vorhandener Ressourcen zu prüfen, um Konfigurationsänderungen zu identifizieren, die zu Sicherheitsrisiken geführt haben, nicht den empfohlenen Grundwerten entsprechen und Sicherheitswarnungen generiert haben. Navigieren Sie hierzu zunächst zum Security Center-Dashboard. Hier gibt es drei Hauptbereiche, die Sie regelmäßig überprüfen sollten.

![Vorgänge](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig4-newUI.png)

1. Über den Abschnittsbereich **Prävention** können Sie schnell auf Ihre wichtigsten Ressourcen zugreifen. Verwenden Sie diese Option für die Überwachung von Compute, Netzwerk, Speicher und Daten und Anwendungen.
2. Der Bereich **Empfehlungen** enthält die Empfehlungen von Security Center. Im Rahmen der kontinuierlichen Überwachung stehen möglicherweise nicht jeden Tag Empfehlungen zur Verfügung. Dies ist normal, da Sie ja alle Empfehlungen bei der Ersteinrichtung von Security Center umgesetzt haben. Aus diesem Grund enthält der Abschnitt unter Umständen nicht täglich neue Informationen, sodass Sie nur bei Bedarf darauf zugreifen müssen.
3. Der Abschnitt **Erkennung** kann sich sehr häufig oder auch nur sehr selten ändern. Prüfen Sie immer Ihre Sicherheitswarnungen, und setzen Sie die Empfehlungen von Security Center um.

## <a name="incident-response"></a>Reaktion auf Vorfälle
Security Center erkennt Bedrohungen, wenn diese entstehen, und macht Sie darauf aufmerksam. Organisationen sollten eine Überwachung auf neue Sicherheitswarnungen implementieren und bei Bedarf geeignete Maßnahmen zur weiteren Untersuchung oder Abwehr des Angriffs ergreifen. Weitere Informationen zur Funktionsweise der Security Center-Bedrohungserkennung finden Sie unter [Azure Security Center-Erkennungsfunktionen](security-center-detection-capabilities.md).

Dieser Artikel soll Ihnen zwar nicht als Hilfe beim Erstellen eines eigenen Plans für Reaktionen auf Zwischenfälle dienen, aber wir verwenden Microsoft Azure Security Response im Cloudlebenszyklus als Grundlage für die Phasen der Reaktionen auf Zwischenfälle. Die Phasen sind in der folgenden Abbildung dargestellt:

![Verdächtige Aktivitäten](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig5-1.png)

> [!NOTE]
> Zu diesem Zweck können Sie den [Computer Security Incident Handling Guide](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf) des NIST (National Institute of Standards and Technology) verwenden.
> 

Sie können Security Center-Warnungen in den folgenden Phasen verwenden:

* **Erkennen**: Identifizieren einer verdächtigen Aktivität in Ressourcen 
* **Bewerten**: Durchführen der ersten Bewertung, um weitere Informationen zur verdächtigen Aktivität zu erhalten
* **Diagnostizieren**: Verwenden der Wiederherstellungsschritte für das technische Verfahren zum Beheben des Problems

Die in jeder Sicherheitswarnung enthaltenen Informationen ermöglichen es, die Art des Angriffs zu ermitteln und mögliche Lösungen vorzuschlagen. Einige Warnungen enthalten auch Links zu weiteren Informationen oder zu anderen Informationsquellen in Azure. Sie können die bereitgestellten Informationen zur weiteren Untersuchung und zum Starten von Korrekturmaßnahmen verwenden. Außerdem können Sie die in Ihrem Arbeitsbereich gespeicherten, sicherheitsrelevanten Daten auch durchsuchen.

Das folgende Beispiel zeigt eine verdächtige RDP-Aktivität:

![Verdächtige Aktivitäten](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig5-ga.png)

Wie Sie sehen, enthält dieses Blatt Details zur Angriffszeit, den Hostnamen der Quelle, den Ziel-VM sowie empfohlene Schritte. In bestimmten Fällen sind die Quellinformationen des Angriffs möglicherweise leer. Weitere Informationen hierzu finden Sie unter [Fehlende Quellinformationen in Azure Security Center-Warnungen](https://blogs.msdn.microsoft.com/azuresecurity/2016/03/25/missing-source-information-in-azure-security-center-alerts/) .

Das Video [How to Leverage the Azure Security Center & Microsoft Operations Management Suite for an Incident Response](https://channel9.msdn.com/Blogs/Taste-of-Premier/ToP1703) (Nutzen von Azure Security Center und Microsoft Operations Management Suite für eine Reaktion auf Vorfälle) enthält Demos, mit denen verdeutlicht wird, wie Sie Security Center in den einzelnen Phasen nutzen können.

> [!NOTE]
> Unter [Verwenden von Azure Security Center zur Reaktion auf Vorfälle](security-center-incident-response.md) erhalten Sie weitere Informationen dazu, wie Security Center-Funktionen Sie während des Prozesses zur Reaktion auf Vorfälle unterstützen. 
> 
> 

## <a name="see-also"></a>Weitere Informationen
In diesem Dokument haben Sie erfahren, wie Sie die Einführung von Security Center planen. Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md)
* [Überwachen der Sicherheitsintegrität in Azure Security Center:](security-center-monitoring.md) Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Überwachen von Partnerlösungen mit Azure Security Center:](security-center-partner-solutions.md) Erfahren Sie, wie der Integritätsstatus Ihrer Partnerlösungen überwacht wird.
* [Azure Security Center – Häufig gestellte Fragen:](security-center-faq.md) Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Blogbeiträge zur Azure-Sicherheit und -Compliance.


