<properties
   pageTitle="Azure SQL-Datenbank – Azure-Fallstudie: Umbraco | Microsoft Azure"
   description="Erfahren Sie, wie Umbraco SQL-Datenbank verwendet, um Dienste für Tausende von Mandanten in der Cloud schnell bereitzustellen und zu skalieren."
   services="sql-database"
   documentationCenter=""
   authors="carlrabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/08/2016"
   ms.author="carlrab"/>

# Umbraco verwendet Azure SQL-Datenbank, um Dienste für Tausende von Mandanten in der Cloud schnell bereitzustellen und zu skalieren

![Umbraco-Logo](./media/sql-database-case-study-umbraco/umbracologo.png)

Umbraco ist ein beliebtes Open Source-CMS (Content Management System), das jede Workload unterstützt, von kleinen Kampagnen- oder Werbewebsites bis hin zu komplexen Anwendungen für Fortune 500-Unternehmen und globale Medienwebsites.

> „Wir haben eine sehr große Community aus Entwicklern, die das System verwenden. In unseren Foren tauschen sich mehr als 100.000 Entwickler aus, und mehr als 350.000 Live-Websites werden von Umbraco unterstützt.“

> – Morten Christensen, Technical Lead, Umbraco

> [AZURE.VIDEO azure-sql-database-case-study-umbraco]

Um die Kundenbereitstellungen zu vereinfachen, hat Umbraco UaaS (Umbraco-as-a-Service) hinzugefügt, ein SaaS-Angebot (Software-as-a-Service), das lokale Bereitstellungen überflüssig macht, integrierte Skalierungsmöglichkeiten bietet und den Verwaltungsaufwand minimiert. Entwickler müssen sich nicht mehr mit der Lösungsverwaltung beschäftigen, sondern können sich auf Produktinnovationen konzentrieren. Umbraco kann all diese Vorteile bereitstellen, da das Unternehmen sich auf das flexible PaaS-Modell (Platform-as-a-Service) von Microsoft Azure verlassen kann.

UaaS ermöglicht SaaS-Kunden die Verwendung der CMS-Funktionen von Umbraco, die bisher außerhalb ihrer Reichweite lagen. Diesen Kunden wird eine funktionierende CMS-Umgebung bereitgestellt, die eine Produktionsdatenbank umfasst. Kunden können je nach Anforderungen bis zu zwei zusätzliche Datenbanken für Entwicklungs- und Stagingumgebungen hinzufügen. Wenn eine neue Umgebung angefordert wird, weist ein automatisierter Prozess diesem Kunden automatisch eine Datenbank zu. Die neue Datenbank steht innerhalb weniger Sekunden bereit, da sie von Umbraco aus einem elastischen Azure-Pool aus verfügbaren Datenbanken bereits vorab bereitgestellt wurde (siehe Abbildung 1).


![Abbildung 1](./media/sql-database-case-study-umbraco/figure1.png)

Abbildung 1. Bereitstellungslebenszyklus für Umbraco-as-a-Service (UaaS)
 
##Elastische Azure-Pools und Automatisierung vereinfachen Bereitstellungen

Mit Azure SQL-Datenbank und weiteren Azure-Diensten können Umbraco-Kunden ihre Umgebungen selbst bereitstellen, und Umbraco kann Datenbanken im Rahmen eines intuitiven Workflows problemlos überwachen und verwalten:

1.	Bereitstellung

    Umbraco hält eine Kapazität von 200 verfügbaren, vorab bereitgestellten Datenbanken aus elastischen Pools bereit. Wenn sich ein neuer Kunde für UaaS registriert, stellt Umbraco diesem Kunden nahezu in Echtzeit eine neue CMS-Umgebung bereit, indem dem Kunden eine Datenbank aus dem Verfügbarkeitspool zugewiesen wird.

    Wenn ein Verfügbarkeitspool einen bestimmten Schwellenwert erreicht, wird ein neuer elastischer Pool erstellt, und neue Datenbanken werden vorab bereitgestellt, um den Kunden bei Bedarf zugewiesen werden zu können.

    Die Implementierung wird mithilfe von C#-Verwaltungsbibliotheken und Azure Service Bus-Warteschlangen vollständig automatisiert.

2.	Nutzung

    Kunden verwenden bis zu drei Umgebungen (Produktion, Staging und/oder Entwicklung), die jeweils über eine eigene Datenbank verfügen. Die Kundendatenbanken befinden sich in elastischen Datenbankpools, sodass Umbraco eine effiziente Skalierung sicherstellen kann, ohne Ressourcen überdimensionieren zu müssen.

    ![Abbildung 2](./media/sql-database-case-study-umbraco/figure2.png)

    ![Abbildung 3](./media/sql-database-case-study-umbraco/figure3.png)

    Abbildung 2. UaaS-Kundenwebsite mit Projektübersicht und Projektdetails

    Azure SQL-Datenbank verwendet DTUs (Database Transaction Units, Datenbanktransaktionseinheiten), um die relative Leistung darzustellen, die für tatsächliche Datenbanktransaktionen erforderlich ist. Bei den UaaS-Kunden arbeiten die Datenbanken in der Regel mit etwa 10 DTUs, aber jede Datenbank ist elastisch und kann bei Bedarf skaliert werden. Auf diese Weise kann UaaS sicherstellen, dass Kunden jederzeit über die notwendigen Ressourcen verfügen, auch in Zeiten höchster Auslastung. Vor einiger Zeit fand beispielsweise an einem Sonntagabend ein großes Sportereignis statt, während dessen ein UaaS-Kunde Spitzenwerte von bis zu 100 DTUs pro Datenbank feststellte. Dank der elastischen Azure-Pools kann Umbraco solche hohen Anforderungen ohne Leistungseinbußen erfüllen.

3.	Überwachung

    Umbraco überwacht die Datenbankaktivität mithilfe von Dashboards im Azure-Portal sowie mit benutzerdefinierten E-Mail-Benachrichtigungen.

4.	Notfallwiederherstellung

    Azure bietet zwei Optionen für die Notfallwiederherstellung: die aktive Georeplikation und die Geowiederherstellung. Welche Option ein Unternehmen auswählt, richtet sich nach den jeweiligen [Zielen hinsichtlich der Geschäftskontinuität](sql-database-business-continuity.md).

    Die aktive Georeplikation bietet die schnellste Reaktion auf einen Ausfall. Mit der aktiven Georeplikation können Sie bis zu vier lesbare sekundäre Datenbanken auf Servern in verschiedenen Regionen erstellen und bei einem Ausfall ein Failover auf eine beliebige dieser sekundären Datenbanken auslösen.

    Umbraco benötigt keine Georeplikation, nutzt jedoch die Azure-Geowiederherstellung, um Ausfallzeiten zu minimieren. Die Geowiederherstellung basiert auf Datenbanksicherungen in georedundantem Azure-Speicher. So können Benutzer nach einem Ausfall einer primären Region eine Wiederherstellung aus einer Sicherungskopie durchführen.

5.	Aufheben der Bereitstellung

    Wenn eine Projektumgebung gelöscht wird, werden während der Bereinigung der Azure Service Bus-Warteschlange alle zugehörigen Datenbanken (Entwicklungs-, Staging- oder Livedatenbanken) entfernt. Dieser automatische Prozess stellt die nicht verwendeten Datenbanken im elastischen Verfügbarkeitspool von Umbraco wieder her und macht sie für eine zukünftige Bereitstellung verfügbar. Gleichzeitig wird für eine maximale Nutzung gesorgt.

##Problemlose Skalierung von UaaS dank elastischer Pools

Durch Nutzung von elastischen Azure-Datenbankpools kann Umbraco die Leistung für seine Kunden ohne Über- oder Unterdimensionierung von Ressourcen optimieren. Umbraco verfügt zurzeit über fast 3.000 Datenbanken in 19 elastischen Datenbankpools und kann diese bei Bedarf problemlos skalieren, um die Anforderungen der 325.000 Bestandskunden oder neuer Kunden zu erfüllen, die ein CMS in der Cloud bereitstellen möchten.

Morten Christensen, Technical Lead bei Umbraco, erklärt: „UaaS wächst zurzeit um etwa 30 neue Kunden pro Tag. Unsere Kunden wissen es sehr zu schätzen, dass sie neue Projekte in Sekundenschnelle bereitstellen, Updates ihrer Livewebsites sofort mit nur einem Klick aus der Entwicklungsumgebung heraus veröffentlichen und eventuelle Fehler im Handumdrehen korrigieren können.“

Wenn ein Kunde die zweite oder dritte Umgebung nicht mehr benötigt, kann er diese Umgebung(en) einfach entfernen. Damit werden Ressourcen freigegeben, die im Rahmen des elastischen Pools an verfügbaren Datenbanken von Umbraco für andere Kunden genutzt werden können.

![Abbildung 4](./media/sql-database-case-study-umbraco/figure4.png)

Abbildung 3. UaaS-Bereitstellungsarchitektur in Microsoft Azure

##Vom Rechenzentrum in die Cloud

Als die Entwickler bei Umbraco beschlossen, auf ein SaaS-Modell umzusteigen, wussten sie bereits, dass sie eine kostengünstige und skalierbare Möglichkeit benötigen würden, um den Dienst auszubauen.

> „Elastische Datenbankpools eignen sich perfekt für unser SaaS-Angebot, da wir die Kapazität ganz nach Bedarf erweitern oder verkleinern können. Die Bereitstellung ist einfach, und mit unserem Setup können wir die Auslastung maximieren.“

> – Morten Christensen, Technical Lead, Umbraco

„Wir wollten unsere Zeit nicht mit der Infrastrukturverwaltung vergeuden, sondern sinnvoll für die Lösung der Probleme unserer Kunden einsetzen. Und wir wollten es unseren Kunden so einfach wie möglich machen, optimal von unserem System zu profitieren“, erklärt Niels Hartvig, der Gründer von Umbraco. „Wir hatten am Anfang darüber nachgedacht, die Server selbst zu hosten – aber allein die Kapazitätsplanung wäre ein einziger Albtraum gewesen.“ Umbraco beschäftigt keinen einzigen Datenbankadministrator, was einen der entscheidenden Wertbeiträge von UaaS unterstreicht.

Ein wichtiges Ziel für die Entwickler bei Umbraco war es, den UaaS-Kunden eine Möglichkeit zu bieten, Umgebungen schnell und ohne Kapazitätseinschränkungen bereitzustellen. Die Bereitstellung eines dedizierten gehosteten Diensts in den Rechenzentren von Umbraco hätte jedoch sehr viel überschüssige Kapazität für Spitzenlasten bei der Verarbeitung erfordert. Dafür wäre eine beträchtliche Menge an Recheninfrastruktur notwendig gewesen, die regelmäßig zu wenig ausgelastet gewesen wäre.

Darüber hinaus wollte das Umbraco-Entwicklungsteam eine Lösung, mit der so viel vorhandener Code wie möglich wiederverwendet werden konnte. Mikkel Hansen, Entwickler bei Umbraco, formuliert es so: „Wir waren zufrieden mit den Microsoft-Entwicklungstools, die wir bereits kannten, wie z.B. Microsoft SQL Server, Microsoft Azure SQL-Datenbank, ASP.NET und die Internetinformationsdienste. Vor der Investition in eine IaaS- oder PaaS-Cloudlösung wollten wir sichergehen, dass eine solche Lösung unsere Microsoft-Tools und -Plattformen unterstützen würde, um keine massiven Änderungen an unserer Codebasis vornehmen zu müssen.“

Um alle Kriterien zu erfüllen, suchte Umbraco nach einem Cloudpartner mit folgenden Qualifikationen:

-	Ausreichende Kapazität und Zuverlässigkeit
-	Unterstützung für Microsoft-Entwicklungstools, um die Umbraco-Entwickler nicht zu zwingen, ihre Entwicklungsumgebung komplett neu zu erfinden
-	Präsenz in allen geografischen Märkten, in denen UaaS angeboten wird (Unternehmen müssen sicherstellen, dass sie schnell auf ihre Daten zugreifen können und dass ihre Daten an einem Ort gespeichert sind, der die regionalen gesetzlichen Vorschriften erfüllt)

##Darum wählte Umbraco Azure für UaaS

Morten Christensen sagt: „Nachdem wir alle Optionen erwogen hatten, haben wir uns für Azure entschieden, da es all unsere Kriterien erfüllte – es lässt sich einfach verwalten und skalieren und bietet vertraute Tools und Kosteneffizienz. Wir richten die Umgebungen auf virtuellen Azure-Computern ein. Jede Umgebung verfügt über eine eigene Azure SQL-Datenbankinstanz, und alle Instanzen befinden sich in elastischen Datenbankpools. Durch Trennung der Datenbanken für Entwicklungs-, Staging- und Liveumgebungen können wir unseren Kunden eine stabile Leistung in Abstimmung mit der Skalierung bieten – ein großer Gewinn!“

Morten führt weiter aus: „Früher mussten wir die Server für Webdatenbanken manuell bereitstellen. Heute müssen wir darüber nicht einmal mehr nachdenken. Alles ist automatisiert – von der Bereitstellung bis hin zur Bereinigung.“

Morten ist auch mit den Skalierungsfunktionen von Azure höchst zufrieden. „Elastische Datenbankpools eignen sich perfekt für unser SaaS-Angebot, da wir die Kapazität ganz nach Bedarf erweitern oder verkleinern können. Die Bereitstellung ist einfach, und mit unserem Setup können wir die Auslastung maximieren.“ Morten sagt weiter: „Die Einfachheit der elastischen Pools zusammen mit der Sicherheit der auf Dienstebenen basierenden DTUs ermöglicht es uns, neue Ressourcenpools ganz nach Bedarf bereitzustellen. Kürzlich erlebte einer unserer größeren Kunden eine Spitzenauslastung von 100 DTUs in seiner Liveumgebung. Dank Azure konnten unsere elastischen Pools die Datenbanken des Kunden in Echtzeit mit den notwendigen Ressourcen versorgen, ohne dass DTU-Anforderungen in irgendeiner Form vorhergesagt werden mussten. Einfach gesagt: Unsere Kunden erhalten die Reaktionszeiten, die sie erwarten, und wir können unsere Leistungs-SLAs erfüllen.“

Mikkel Hansen fasst zusammen: „Wir haben uns den leistungsstarken Azure-Algorithmus zunutze gemacht, der ein häufiges SaaS-Szenario (Onboarding eines Kunden in Echtzeit und ganz an den Bedarf des Kunden angepasst) mit unserem Anwendungsmuster verknüpft (Vorabbereitstellung von Datenbanken, sowohl für Entwicklungs- als auch für Liveumgebungen). All dies baut auf der zugrunde liegende Technologie auf (Azure Service Bus-Warteschlangen zusammen mit Azure SQL-Datenbank).“

##Dank Azure übertrifft UaaS die Erwartungen der Kunden

Seitdem Umbraco sich für Azure als Cloudpartner entschieden hat, konnte das Unternehmen seinen UaaS-Kunden eine optimierte Leistung für ihre Content-Management-Systeme bieten – und zwar ohne die hohen Investitionen in IT-Ressourcen, die bei einer selbst gehosteten Lösung notwendig gewesen wären. Morten formuliert es so: „Wir schätzen die bequeme Entwicklung und die Skalierbarkeit, die Azure uns bietet. Und unsere Kunden sind ganz begeistert von der Funktionsvielfalt und Zuverlässigkeit. Insgesamt war es ein großer Gewinn für uns!“
 
## Weitere Informationen

- Weitere Informationen zu elastischen Azure-Datenbankpools finden Sie unter [Pools für elastische Datenbanken](sql-database-elastic-pool.md).

- Weitere Informationen zu Azure Service Bus finden Sie unter [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).

- Weitere Informationen zu Webrollen und Workerrollen finden Sie unter [Workerrollen](../fundamentals-introduction-to-azure.md#compute).

- Weitere Informationen zu virtuellen Netzwerken finden Sie unter [Virtuelle Netzwerke](https://azure.microsoft.com/documentation/services/virtual-network/).

- Weiter Informationen zur Sicherung und Wiederherstellung finden Sie unter [Geschäftskontinuität](sql-database-business-continuity.md).

- Weitere Informationen zur Überwachung von Pools finden Sie unter [Überwachen von Pools](sql-database-elastic-pool-manage-portal.md).

- Weitere Informationen zu Umbraco-as-a-Service finden Sie unter [Umbraco](https://umbraco.com/cloud).

<!---HONumber=AcomDC_0921_2016-->