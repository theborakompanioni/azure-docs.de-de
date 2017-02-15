---
title: "Notfallwiederherstellung und hohe Verfügbarkeit für Azure-Anwendungen | Microsoft Docs"
description: "Technische Übersichten und ausführliche Informationen zum Entwerfen von Anwendungen für hohe Verfügbarkeit und zur Notfallwiederherstellung von in Microsoft Azure erstellten Anwendungen."
services: 
documentationcenter: na
author: adamglick
manager: saladki
editor: 
ms.assetid: e13d5f49-2b91-44ba-829a-1e0f1fceaae8
ms.service: resiliency
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2016
ms.author: aglick
translationtype: Human Translation
ms.sourcegitcommit: 0c23ee550d8ac88994e8c7c54a33d348ffc24372
ms.openlocfilehash: ee3e7103bd647c2d8651ec912fbb764a342781e2


---
# <a name="disaster-recovery-and-high-availability-for-applications-built-on-microsoft-azure"></a>Notfallwiederherstellung und hohe Verfügbarkeit für in Microsoft Azure erstellte Anwendungen
## <a name="introduction"></a>Einführung
Dieser Artikel beschäftigt sich mit hoher Verfügbarkeit für Anwendungen, die in Azure ausgeführt werden. Zur Gesamtstrategie für hohe Verfügbarkeit gehört auch die Notfallwiederherstellung. Das Planen für Ausfälle und Notfälle in der Cloud beginnt damit, dass Sie Ausfälle schnell erkennen. Anschließend setzen Sie eine Strategie um, die zur tolerierten Ausfallzeit für die Anwendung passt. Außerdem müssen Sie das Ausmaß des Datenverlusts berücksichtigen, das bei einer Anwendung toleriert werden kann: Eine Wiederherstellung von Daten sollte keine negativen geschäftlichen Konsequenzen haben.

Die meisten Unternehmen geben an, dass sie auf vorübergehende und umfangreiche Ausfälle vorbereitet sind. Bevor Sie jedoch diese Frage für sich selbst beantworten: Werden in Ihrem Unternehmen Übungen für Ausfallsituationen durchgeführt? Testen Sie das Wiederherstellen von Datenbanken, um sicherzustellen, dass Sie über die richtigen Prozesse verfügen? Wahrscheinlich nicht. Das liegt daran, dass die Implementierung von Prozessen für eine erfolgreiche Notfallwiederherstellung mit einigem Aufwand für Planung und Architekturentwurf verbunden ist. Leider wird für die Notfallwiederherstellung selten genügend Vorabanalyse und Vorbereitungszeit eingeplant – darin gleicht sie vielen anderen nicht funktionsbezogenen Aufgabenbereichen, z.B. der Sicherheit. Außerdem verfügen die meisten Unternehmen nicht über das Budget für die Nutzung geografisch verteilter Regionen mit redundanter Kapazität. Daher sind selbst unternehmenskritische Anwendungen häufig nicht durch ordnungsgemäße Notfallwiederherstellungspläne abgedeckt.

Cloudplattformen wie Microsoft Azure bieten geografisch verteilte Regionen in allen Teilen der Welt. Diese Plattformen verfügen zudem über Funktionen, die die Verfügbarkeit sowie eine Reihe von Notfallwiederherstellungsszenarien unterstützen. Dadurch kann jede unternehmenskritische Cloudanwendung bei der Notfallabsicherung des Systems angemessen berücksichtigt werden. Bei Azure sind Resilienz und Notfallwiederherstellung von vornherein Bestandteil vieler Dienste. Beschäftigen Sie sich gründlich mit den Features der Plattform, und ergänzen Sie sie durch geeignete Strategien für Ihre Anwendungen.

In diesem Artikel werden die unerlässlichen architekturbezogenen Schritte beschrieben, um eine Azure-Bereitstellung gegen Notfälle abzusichern. Anschließend können Sie den umfassenderen Prozess für die Geschäftskontinuität implementieren. Ein Geschäftskontinuitätsplan ist eine Roadmap für die Fortsetzung des Geschäftsbetriebs auch unter widrigen Umständen. Solche Umstände können der Ausfall einer Technologie (z.B. ein nicht funktionierender Dienst) oder eine Naturkatastrophe wie ein Sturm oder ein Stromausfall sein. Anwendungsresilienz (Robustheit) bei Notfällen ist nur ein Teil des umfangreicheren Notfallwiederherstellungsprozesses, wie im folgenden NIST-Dokument beschrieben: [Contingency Planning Guide for Information Technology Systems](https://www.fismacenter.com/sp800-34.pdf) (Leitfaden zur Notfallplanung für IT-Systeme).

In den folgenden Abschnitten werden verschiedene Ausfallebenen, Techniken zum Umgang mit diesen Ausfällen und Architekturen zur Unterstützung dieser Techniken definiert. Diese Informationen helfen Ihnen dabei, Ihre Prozesse und Verfahren für die Notfallwiederherstellung so zu gestalten, dass Ihre Notfallwiederherstellungsstrategie ordnungsgemäß und effizient funktioniert.

## <a name="characteristics-of-resilient-cloud-applications"></a>Eigenschaften robuster Cloudanwendungen
Eine Anwendung mit gut durchdachter Architektur ist widerstandsfähig gegenüber Funktionsausfällen auf taktischer Ebene und kann auch strategische systemweite Ausfälle auf Regionsebene tolerieren. In den folgenden Abschnitten wird die in diesem Dokument verwendete Terminologie eingeführt, mit der verschiedene Aspekte robuster Clouddienste beschrieben werden.

### <a name="high-availability"></a>Hohe Verfügbarkeit
Bei einer hoch verfügbaren Cloudanwendung werden Strategien implementiert, mit denen der Ausfall von Abhängigkeiten abgefedert werden kann – etwa ein Ausfall der verwalteten Dienste, die von der Cloudplattform bereitgestellt werden. Dieser Ansatz ermöglicht es, dass die Anwendung trotz des möglichen Ausfalls von Funktionen der Cloudplattform weiterhin die erwarteten funktionalen und nicht funktionalen systemischen Eigenschaften demonstriert. Ausführliche Informationen hierzu erhalten Sie in der Channel 9-Videoserie [Failsafe: Guidance for Resilient Cloud Architectures](https://channel9.msdn.com/Series/FailSafe)(Ausfallsicher: Richtlinien für stabile Cloudarchitekturen).

Wenn Sie die Anwendung implementieren, müssen Sie die Wahrscheinlichkeit eines Funktionsausfalls berücksichtigen. Außerdem müssen Sie die Auswirkungen aus geschäftlicher Sicht betrachten, die ein solcher Ausfall auf die Anwendung hat. Erst danach sollten Sie sich mit den Details der Implementierungsstrategien beschäftigen. Ohne angemessene Berücksichtigung der geschäftlichen Auswirkungen und der Wahrscheinlichkeit einer Risikosituation kann eine Implementierung teuer und potenziell unnötig sein.

Nehmen Sie zum Vergleich ein Beispiel aus der Automobilindustrie. Selbst hochwertige Teile und eine erstklassige Konstruktion können Ausfälle nicht vollkommen verhindern. Wenn Ihr Auto z.B. eine Reifenpanne hat, fährt es immer noch, aber Sie können damit nicht einfach wie gewohnt weiterfahren. Vielleicht haben Sie Vorkehrungen für diesen Eventualfall getroffen. Dann können Sie ein Notrad montieren und so zumindest bis zur nächsten Werkstatt fahren. Obwohl Sie mit dem Notrad nicht so schnell vorankommen, können Sie mit dem Fahrzeug so lange fahren, bis der defekte Reifen ersetzt ist. Ganz ähnlich können Sie bei einem Clouddienst durch eine Planung für mögliche Funktionsausfälle verhindern, dass ein relativ kleines Problem zum Ausfall einer gesamten Anwendung führt. Dies gilt auch dann, wenn der Clouddienst mit eingeschränkter Funktionalität ausgeführt werden muss.

Hoch verfügbare Clouddienste zeichnen sich durch einige Schlüsseleigenschaften aus: Verfügbarkeit, Skalierbarkeit und Fehlertoleranz. Natürlich stehen diese Eigenschaften in enger Beziehung zueinander. Dennoch ist es wichtig, sie einzeln zu verstehen und nachzuvollziehen, wie sie zur Gesamtverfügbarkeit der Lösung beitragen.

### <a name="availability"></a>Verfügbarkeit
Bei einer auf Verfügbarkeit ausgelegten Anwendung wird die Verfügbarkeit der zugrunde liegenden Infrastruktur und der abhängigen Dienste berücksichtigt. Bei auf Verfügbarkeit ausgelegten Anwendungen werden Single Points of Failure durch Redundanz und robustes Design vermieden. Wenn Sie die Verfügbarkeit in Azure in einem weiteren Kontext betrachten, ist es wichtig, das Konzept der effektiven Verfügbarkeit der Plattform zu verstehen. Bei der effektiven Verfügbarkeit werden die Vereinbarungen zum Servicelevel (Service Level Agreements, SLAs) für jeden abhängigen Dienst sowie die kumulativen Auswirkungen auf die Gesamtverfügbarkeit des Systems berücksichtigt.

Die Systemverfügbarkeit entspricht dem prozentualen Anteil eines Zeitfensters, in dem das System betrieben werden kann. Beispielsweise beinhaltet die SLA für mindestens zwei Instanzen einer Web- oder Workerrolle in Azure eine Verfügbarkeit von 99,95 Prozent. Die Leistung oder die Funktionalität der auf diesen Rollen ausgeführten Dienste wird dabei nicht gemessen. Die effektive Verfügbarkeit Ihres Clouddiensts wird jedoch auch von den verschiedenen SLAs für andere abhängige Dienste beeinflusst. Je mehr „bewegliche Teile“ im System vorhanden sind, desto mehr Sorgfalt müssen Sie aufwenden, um sicherzustellen, dass Ihre Anwendung die Verfügbarkeitsanforderungen der Endbenutzer stabil erfüllt.

Betrachten Sie die folgenden SLAs für einen Azure-Dienst, der folgende Azure-Dienste nutzt: Compute, Azure SQL-Datenbank und Azure Storage.

| Azure-Dienst | SLA | Potenzielle Ausfallzeit in Minuten/Monat (30 Tage) |
|:--- |:--- |:---:|
| Compute |99,95 % |21,6 Minuten |
| SQL-Datenbank |99,99 % |4,3 Minuten |
| Speicher |99,90 % |43,2 Minuten |

Sie müssen bei Ihrer Planung bedenken, dass alle Dienste möglicherweise zu verschiedenen Zeiten ausfallen. In diesem vereinfachten Beispiel kann die Anwendung insgesamt pro Monat 108 Minuten ausfallen. Ein Monat mit 30 Tagen hat insgesamt 43.200 Minuten. 108 Minuten entsprechen 0,25 Prozent der Gesamtanzahl von Minuten in einem Monat mit 30 Tagen (43.200 Minuten). Dies ergibt für den Clouddienst eine effektive Verfügbarkeit von 99,75 Prozent.

Allerdings können Sie diese Verfügbarkeit mit den in diesem Artikel beschriebenen Techniken verbessern. Wenn Sie z.B. Ihre Anwendung so gestalten, dass sie bei einem Ausfall der SQL-Datenbank weiterhin ausgeführt wird, können sie diesen Anteil aus der Rechnung streichen. Das heißt dann, dass die Anwendung unter Umständen mit eingeschränkten Funktionsumfang ausgeführt wird, und Sie müssen deshalb auch geschäftliche Anforderungen berücksichtigen. Eine vollständige Liste mit den Azure-SLAs finden Sie unter [Vereinbarungen zum Servicelevel](https://azure.microsoft.com/support/legal/sla/).

### <a name="scalability"></a>Skalierbarkeit
Skalierbarkeit wirkt sich direkt auf die Verfügbarkeit aus. Eine Anwendung, die bei erhöhter Auslastung ausfällt, ist nicht mehr verfügbar. Skalierbare Anwendungen können in akzeptablen Zeiträumen und mit konsistenten Ergebnissen einen erhöhten Bedarf erfüllen. Wenn ein System skalierbar ist, kann es entweder horizontal oder vertikal skaliert werden, um die erhöhte Last bei konstanter Leistung zu bewältigen. Einfach ausgedrückt: Bei der horizontalen Skalierung werden mehr Computer gleicher Größe (Prozessor, Arbeitsspeicher und Bandbreite) hinzugefügt. Bei der vertikalen Skalierung werden dagegen die vorhandenen Computer vergrößert. Bei Azure stehen Ihnen für die vertikale Skalierung im Bereich „Compute“ verschiedene Optionen mit unterschiedlich großen virtuellen Computern zur Verfügung. Für eine Änderung der Computergröße ist jedoch eine erneute Bereitstellung nötig. Für ein Maximum an Flexibilität wird deshalb die horizontale Skalierung bevorzugt. Das betrifft insbesondere den Computebereich, da sich die Anzahl ausgeführter Instanzen einer Web- oder Workerrolle problemlos erhöhen lässt. Diese zusätzlichen Instanzen verarbeiten den erhöhten Datenverkehr über das Azure-Webportal, PowerShell-Skripts oder Code. Gründen Sie Ihre Skalierungsentscheidung auf Erhöhungen in spezifischen überwachten Metriken. In einem solchen Szenario sind unter höherer Last sowohl bei der Leistung für die Benutzer als auch bei den Metriken keine spürbaren Verschlechterungen zu verzeichnen. In der Regel speichern die Web- und Workerrollen alle Zustände extern. Dies ermöglicht einen flexiblen Lastenausgleich und eine ordnungsgemäße Durchführung von Änderungen bei der Anzahl von Instanzen. Die horizontale Skalierung eignet sich auch sehr gut für Dienste, die keine abgestuften Optionen für die vertikale Skalierung bieten, wie z.B. Azure Storage.

Cloudbereitstellungen sollten als eine Sammlung von Skalierungseinheiten verstanden werden. Dadurch kann die Anwendung die Durchsatzanforderungen der Endbenutzer flexibel erfüllen. Die Skalierungseinheiten lassen sich leichter auf der Ebene von Webserver und Anwendungsserver visualisieren. Dies ist so, weil Azure bereits zustandslose Computeknoten über Web- und Workerrollen bereitstellt. Das Hinzufügen weiterer Computeskalierungseinheiten zu einer Bereitstellung hat keine Nebeneffekte für die Anwendungszustandsverwaltung, da Computeskalierungseinheiten zustandslos sind. Eine Speicherskalierungseinheit ist zuständig für die Verwaltung einer Partition mit Daten (strukturiert oder unstrukturiert). Beispiele für Speicherskalierungseinheiten wären eine Azure-Tabellenpartition, ein Azure-Blobcontainer oder eine SQL-Datenbank. Selbst die Verwendung mehrerer Azure Storage-Konten wirkt sich direkt auf die Anwendungsskalierbarkeit aus. Sie müssen einen hochgradig skalierbaren Clouddienst entwerfen, wenn Sie mehrere Speicherskalierungseinheiten integrieren möchten. Ein Beispiel: Wenn eine Anwendung relationale Daten verwendet, empfiehlt es sich, die Daten für mehrere Instanzen der SQL-Datenbank zu partitionieren. Auf diese Weise ist der Speicher dem flexiblen Modell der Computeskalierungseinheiten gewachsen. Ganz ähnlich können Sie mit Azure Storage Datenpartitionierungsschemas mit ausgefeilten Entwürfen erstellen, um die Dursatzanforderungen der Computeschicht zu erfüllen. Eine Liste mit bewährten Methoden zum Entwerfen von skalierbaren Clouddiensten finden Sie unter [Best Practices for the Design of Large-Scale Services on Azure Cloud Services](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/)(Bewährte Methoden für den Entwurf umfangreicher Dienste in Azure Cloud Services).

### <a name="fault-tolerance"></a>Fehlertoleranz
Bei Anwendungen muss berücksichtigt werden, dass alle abhängigen Cloudfunktionen zu einem bestimmten Zeitpunkt ausfallen können und werden. Fehlertolerante Anwendungen erkennen ausgefallene Elemente und können sie umgehen, um funktionsfähig zu bleiben und innerhalb eines bestimmten Zeitraums die richtigen Ergebnisse zurückzugeben. Für vorübergehende Fehlerzustände nutzt ein fehlertolerantes System eine Wiederholungsrichtlinie. Bei ernsthafteren Fehlern erkennt die Anwendung die jeweiligen Probleme und führt ein Failover auf alternative Hardware durch oder setzt Notfallpläne um, bis der Fehler korrigiert ist. Eine zuverlässige Anwendung kann den Ausfall eines Teils oder mehrerer Teile planvoll verwalten und ihren Betrieb ordnungsgemäß fortsetzen. Um Fehlertoleranz bei Anwendungen zu erreichen, werden verschiedene Entwurfsstrategien eingesetzt, z.B. Redundanz, Replikation oder eingeschränkte Funktionalität.

## <a name="disaster-recovery"></a>Notfallwiederherstellung
Bei einem systemischen Ausfall der abhängigen Dienste oder der zugrunde liegenden Infrastruktur funktioniert eine Cloudbereitstellung möglicherweise nicht mehr. Unter solchen Umständen löst ein Geschäftskontinuitätsplan den Prozess für die Notfallwiederherstellung aus. Bei diesem Prozess sorgen Mitarbeiter aus dem IT-Betrieb und automatisierte Verfahren dafür, dass die Anwendung in einer verfügbaren Region reaktiviert wird. Dabei müssen Anwendungsbenutzer, Daten und Dienste in die neue Region übertragen werden. Außerdem muss auf Sicherungsmedien oder eine laufende Replikation zurückgegriffen werden.

Denken Sie zurück an das Beispiel, bei dem hohe Verfügbarkeit mit der Möglichkeit verglichen wurde, auf eine Reifenpanne mit dem Einbau eines Notrads zu reagieren und weiterzufahren. Im Gegensatz dazu ähnelt die Notfallwiederherstellung den Schritten nach einem Verkehrsunfall, bei dem das Auto fahruntüchtig wurde. In diesem Fall besteht die beste Lösung darin, ein Ersatzauto zu organisieren – etwa durch einen Anruf bei einem Automobilclub oder bei einem Bekannten. Es dauert in diesem Szenario meist etwas länger, bis Sie Ihre Fahrt fortsetzen können. Außerdem ist es deutlich komplizierter, das ursprüngliche Fahrzeug zu reparieren und wieder in Betrieb zu nehmen. Auf ähnliche Weise ist die Notfallwiederherstellung in einer anderen Region eine komplexe Aufgabe, die meist mit Ausfallzeiten und Datenverlusten verbunden ist. Um die Strategien zur Notfallwiederherstellung besser verstehen und bewerten zu können, benötigen wir Definitionen für zwei Begriffe: Recovery Time Objective (RTO) und Recovery Point Objective (RPO).

### <a name="recovery-time-objective"></a>RTO (Recovery Time Objective)
Bei RTO handelt es sich um die maximal für die Wiederherstellung der Anwendungsfunktionen vorgesehene Zeit. Sie beruht auf den Geschäftsanforderungen und hängt mit der Bedeutung der Anwendung zusammen. Für unternehmenskritische Geschäftsanwendungen wird eine niedrige RTO benötigt.

### <a name="recovery-point-objective"></a>RPO (Recovery Point Objective)
Bei RPO handelt es sich um den Zeitraum, für den ein Datenverlust im Zuge der Wiederherstellung akzeptabel ist. Wenn Sie z.B. eine RPO von einer Stunde wünschen, müssen Sie Ihre Daten mindestens jede Stunde vollständig sichern oder replizieren. Nachdem Sie die Anwendung in einer alternativen Region gestartet haben, fehlen in den Daten aus der Datensicherung möglicherweise die Daten einer gesamten Stunde. Ebenso wie bei der RTO wird für unternehmenskritische Anwendungen eine sehr viel geringere RPO angestrebt.

## <a name="checklist"></a>Checkliste
Fassen wir die wichtigsten Punkte aus diesem Artikel und aus den verwandten Artikeln zu [hoher Verfügbarkeit](resiliency-high-availability-azure-applications.md) und [Notfallwiederherstellung](resiliency-disaster-recovery-azure-applications.md) für Azure-Anwendungen zusammen. Am besten eignet sich dafür eine Checkliste mit den Aspekten, die Sie bei Ihrer eigenen Planung für Verfügbarkeit und Notfallwiederherstellung berücksichtigen sollten. Dies sind bewährte Methoden, mit denen Kunden ernsthaft und erfolgreich eine entsprechende Lösung implementiert haben.

1. Führen Sie für jede Anwendung eine Risikobewertung durch, da für jede Anwendung andere Anforderungen gelten können. Einige Anwendungen sind wichtiger als andere. Für sie ist sind u.U. Zusatzkosten gerechtfertigt, um eine Architektur für die Notfallwiederherstellung zu entwerfen.
2. Verwenden Sie die bei der Bewertung gewonnenen Informationen zum Definieren einer RTO (Recovery Time Objective) und einer RPO (Recovery Point Objective) für jede Anwendung.
3. Berücksichtigen Sie mögliche Ausfälle schon beim Entwurf, und zwar zuerst bei der Anwendungsarchitektur.
4. Setzen Sie bewährte Methoden für hohe Verfügbarkeit um, aber wahren Sie dabei die Balance zwischen Kosten, Komplexität und Risiko.
5. Implementieren Sie Pläne und Prozesse für die Notfallwiederherstellung.
   * Berücksichtigen Sie alle Grade von Ausfällen – Ausfälle auf Modulebene genauso wie den kompletten Ausfall der Cloud.
   * Erarbeiten Sie Sicherungsstrategien für alle Verweis- und Transaktionsdaten.
   * Wählen Sie eine Architektur für die Notfallwiederherstellung, die mehrere Standorte umfasst.
6. Legen Sie einen Verantwortlichen für die Prozesse, die Automatisierung und den Test der Notfallwiederherstellung fest. Dieser Verantwortliche sollte für den gesamten Prozess zuständig sein und ihn steuern.
7. Dokumentieren Sie die Prozesse, damit sie leicht wiederholbar sind. Obwohl es einen einzigen Verantwortlichen gibt, sollten mehrere Personen die Prozesse verstehen und im Notfall ausführen können.
8. Schulen Sie die Mitarbeiter beim Umsetzen der Prozesse.
9. Simulieren Sie Notfälle regelmäßig, um Mitarbeiter zu schulen und die Prozesse zu überprüfen.

## <a name="summary"></a>Zusammenfassung
Wenn Hardware oder Anwendungen in Azure ausfallen, werden andere Techniken und Strategien genutzt als bei Ausfällen in lokalen Systemen. Der Hauptgrund dafür ist, dass Cloudlösungen meist auf vielfältige Weise von einer Infrastruktur abhängig sind, die über eine Azure-Region verteilt ist und in Form separater Dienste verwaltet wird. Auf Teilausfälle müssen Sie mit Techniken für hohe Verfügbarkeit reagieren. Bei schwerer wiegenden Ausfällen, möglicherweise aufgrund von Notfallereignissen, verwenden Sie Strategien zur Notfallwiederherstellung.

Azure erkennt und behandelt viele Ausfälle, aber es gibt auch viele Arten von Fehlern, für die anwendungsspezifische Strategien verfolgt werden müssen. Sie müssen sich auf den Ausfall von Anwendungen, Diensten und Daten aktiv vorbereiten und entsprechende Verwaltungsmaßnahmen treffen.

Berücksichtigen Sie die geschäftlichen Konsequenzen eines Anwendungsausfalls, wenn Sie einen Plan für die Anwendungsverfügbarkeit und die Notfallwiederherstellung erarbeiten. Definieren Sie Prozesse, Richtlinien und Verfahren für die Wiederherstellung unternehmenskritischer Systeme nach Notfällen – das erfordert Zeit, Planung und Engagement. Und wenn Sie die Pläne erarbeitet haben, ist Ihre Aufgabe noch nicht abgeschlossen. Sie müssen die Pläne auf Basis Ihres Anwendungsportfolios, der geschäftlichen Erfordernisse und der verfügbaren Technologien regelmäßig analysieren, testen und fortlaufend verbessern. Azure stellt Sie vor neue Herausforderungen, bietet Ihnen aber auch neue Möglichkeiten beim Erstellen robuster Anwendungen, die Ausfällen standhalten.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
[Hohe Verfügbarkeit für in Microsoft Azure erstellte Anwendungen](resiliency-high-availability-azure-applications.md)

[Notfallwiederherstellung für in Microsoft Azure erstellte Anwendungen](resiliency-disaster-recovery-azure-applications.md)

[Technischer Leitfaden zur Resilienz in Azure](resiliency-technical-guidance.md)

[Übersicht: Geschäftskontinuität für die Cloud und Notfallwiederherstellung für Datenbanken mit SQL-Datenbank](../sql-database/sql-database-business-continuity.md)

[Hochverfügbarkeit und Notfallwiederherstellung für SQL Server auf virtuellen Azure-Computern](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md)

[Failsafe: Guidance for resilient cloud architectures (Ausfallsicher: Leitfaden für stabile Cloudarchitekturen)](https://channel9.msdn.com/Series/FailSafe)

[Best Practices for the design of large-scale services on Azure Cloud Services (Bewährte Methoden für den Entwurf umfangreicher Dienste in Azure Cloud Services)](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/)

## <a name="next-steps"></a>Nächste Schritte
Dieser Artikel ist Teil einer Reihe von Artikeln mit Fokus auf Notfallwiederherstellung und hohe Verfügbarkeit für Azure-Anwendungen. Der nächste Artikel in dieser Reihe ist [Hohe Verfügbarkeit für in Microsoft Azure erstellte Anwendungen](resiliency-high-availability-azure-applications.md).




<!--HONumber=Feb17_HO2-->


