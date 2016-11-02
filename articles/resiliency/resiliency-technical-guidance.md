<properties
   pageTitle="Index des technischen Leitfadens zur Resilienz | Microsoft Azure"
   description="Index der technischen Artikel zu den Grundlagen und zum Entwerfen von robusten, hoch verfügbaren, fehlertoleranten Anwendungen sowie zur Planung von Notfallwiederherstellung und Geschäftskontinuität."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#Technischer Leitfaden zur Resilienz in Azure

##Einführung

Zur Erfüllung der Anforderungen in puncto hohe Verfügbarkeit und Notfallwiederherstellung sind zwei Dinge erforderlich:

- Detaillierte technische Kenntnisse hinsichtlich der Funktionen einer Cloudplattform
- Kenntnisse für die ordnungsgemäße Konzipierung eines verteilten Diensts

Diese Artikelserie befasst sich mit dem ersten Punkt – also mit den Möglichkeiten und Einschränkungen der Azure-Plattform bezüglich der Resilienz (manchmal auch als Geschäftskontinuität bezeichnet). Informationen zum zweiten Punkt finden Sie in der Artikelserie [Notfallwiederherstellung und hohe Verfügbarkeit für Azure-Anwendungen](https://aka.ms/drtechguide). In dieser Artikelserie werden zwar auch Architektur- und Entwurfsmuster angesprochen, sie stehen jedoch nicht im Mittelpunkt. Hilfreiche Entwurfsinformationen finden Sie unter [Zusätzliche Ressourcen](#additional-resources).

Die Informationen sind in folgende Artikel gegliedert:

- [Wiederherstellung nach lokalen Fehlern](resiliency-technical-guidance-recovery-local-failures.md). Physische Hardware (etwa Laufwerke, Server und Netzwerkgeräte) kann ausfallen. Ressourcen können bei Lastspitzen überlastet sein. Dieser Artikel beschreibt die Funktionen von Azure, die auch unter diesen Bedingungen eine hohe Verfügbarkeit gewährleisten.

- [Wiederherstellung nach einer regionsweiten Dienstunterbrechung von Azure](resiliency-technical-guidance-recovery-loss-azure-region.md). Großflächige Ausfälle sind zwar selten, aber theoretisch möglich. Gesamte Regionen können aufgrund von Netzwerkfehlern isoliert oder bei Naturkatastrophen physisch beschädigt werden. In diesem Artikel wird erläutert, wie mithilfe von Azure Anwendungen erstellt werden, die sich über geografisch verteilte Regionen erstrecken.

- [Wiederherstellung von einem lokalen Standort nach Azure](resiliency-technical-guidance-recovery-on-premises-azure.md). Die Cloud verändert die Wirtschaftlichkeit der Notfallwiederherstellung erheblich und ermöglicht Organisationen die Einrichtung eines zweiten Standorts für die Notfallwiederherstellung mithilfe von Azure. Im Gegensatz zur Einrichtung und Verwaltung eines sekundären Datencenters fällt dabei nur ein Bruchteil der Kosten an. Dieser Artikel beschreibt die Azure-Funktionen zur Ausweitung eines lokalen Datencenters auf die Cloud.

- [Wiederherstellung nach Datenbeschädigung oder unbeabsichtigtem Löschen](resiliency-technical-guidance-recovery-data-corruption.md). Daten können durch Fehler in Anwendungen beschädigt werden. Wichtige Daten können ungewollt durch Benutzer gelöscht werden. In diesem Artikel wird erläutert, welche Funktionen Azure bereitstellt, um Daten zu sichern und den Zustand eines früheren Zeitpunkts wiederherzustellen.

##Zusätzliche Ressourcen

- [Notfallwiederherstellung und hohe Verfügbarkeit für in Microsoft Azure erstellte Anwendungen](resiliency-disaster-recovery-high-availability-azure-applications.md). Dieser Artikel ist eine detaillierte Übersicht zu Verfügbarkeit und Notfallwiederherstellung. Sie behandelt die Herausforderung der manuellen Replikation von Referenz- und Transaktionsdaten. Die letzten Abschnitte bieten Zusammenfassungen für verschiedene Arten von Notfallwiederherstellungs-Topologien, die für den höchsten Grad an Verfügbarkeit verschiedene Azure-Regionen umfassen.

- [Checkliste für hohe Verfügbarkeit](resiliency-high-availability-checklist.md). Dieser Artikel enthält eine Liste mit Features, Diensten und Designs, mit denen Sie die Resilienz und Verfügbarkeit Ihrer Anwendung steigern können.

- [Leitfaden zur Resilienz von Diensten in Microsoft Azure](resiliency-service-guidance-index.md). Dieser Artikel enthält einen Index der Azure-Dienste sowie Links zu Notfallwiederherstellungs- und Entwurfsleitfäden.

- [Übersicht: Geschäftskontinuität für die Cloud und Notfallwiederherstellung für Datenbanken mit SQL-Datenbank](../sql-database/sql-database-business-continuity.md). Dieser Artikel behandelt Verfügbarkeitstechniken der Azure SQL-Datenbank. Dabei stehen Sicherungs- und Wiederherstellungsstrategien im Mittelpunkt. Wenn Sie Azure SQL-Datenbank im Clouddienst verwenden, sollten Sie diesen Artikel und die zugehörigen Ressourcen durcharbeiten.

- [Hochverfügbarkeit und Notfallwiederherstellung für SQL Server auf virtuellen Azure-Computern](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md). In diesem Artikel werden Verfügbarkeitsoptionen erläutert, mit denen Sie sich befassen können, wenn Sie Infrastructure-as-a-Service (IaaS) zum Hosten von Datenbankdiensten verwenden. Der Artikel geht auf AlwaysOn-Verfügbarkeitsgruppen, Datenbankspiegelung, Protokollversand und Sicherung/Wiederherstellung ein. Die Verwendung dieser Techniken wird im Rahmen mehrerer Tutorials veranschaulicht.

- [Bewährte Methoden für das Entwerfen umfassender Dienste mit Azure Cloud Services](https://azure.microsoft.com//blog/best-practices-for-designing-large-scale-services-on-windows-azure/). Dieser Artikel konzentriert sich auf die Entwicklung von hochgradig skalierbaren Cloudarchitekturen. Viele der Techniken zur Verbesserung der Skalierbarkeit erhöhen auch die Verfügbarkeit. Wenn Ihre Anwendung bei erhöhter Last nicht skaliert werden kann, wird die Skalierbarkeit zum Verfügbarkeitsproblem.

- [Sicherung und Wiederherstellung für SQL Server auf virtuellen Azure-Computern](../virtual-machines/virtual-machines-windows-sql-backup-recovery.md). Dieser Artikel enthält eine technische Anleitung zum Sichern und Wiederherstellen von Microsoft SQL Server auf virtuellen Azure-Computern.

- [Failsafe: Guidance for resilient cloud architectures](https://channel9.msdn.com/Series/FailSafe) (Ausfallsicher: Leitfaden für stabile Cloudarchitekturen). Dieser Artikel enthält Richtlinien zur Erstellung stabiler Cloudarchitekturen, Orientierungshilfen zum Implementieren dieser Architekturen mit Microsoft-Technologien und Anleitungen zum Implementieren dieser Architekturen in bestimmte Szenarien.

- [Technische Fallstudie: „Using cloud technologies to improve disaster recovery“](https://www.microsoft.com/itshowcase/Article/Content/737/Using-cloud-technologies-to-improve-disaster-recovery) (Verwendung von Cloudtechnologien zur Verbesserung der Notfallwiederherstellung). Diese Fallstudie zeigt, wie Microsoft IT mithilfe von Azure die Notfallwiederherstellung verbessert.

##Nächste Schritte

Dieser Artikel gehört zu einer Reihe von Artikeln, die als technischer Leitfaden zur Resilienz in Azure fungieren. Wenn Sie an anderen Artikeln dieser Serie interessiert sind, ist [Wiederherstellung nach lokalen Fehlern](resiliency-technical-guidance-recovery-local-failures.md) ein guter Einstiegspunkt.

<!---HONumber=AcomDC_0824_2016-->