<properties
   pageTitle="Index des technischen Leitfadens zur Resilienz | Microsoft Azure"
   description="Index der technischen Artikel über die Grundlagen und den Entwurf von robusten, hoch verfügbaren, fehlertoleranten Anwendungen sowie die Planung von Notfallwiederherstellung und Geschäftskontinuität."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="hongfeig"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/13/2016"
   ms.author="patw;jroth;aglick"/>

#Technischer Leitfaden zur Resilienz in Azure

##Einführung
Zwei Arten von Wissen sind notwendig, um die Voraussetzungen für hohe Verfügbarkeit und Notfallwiederherstellung zu erfüllen: 1) umfassende technische Kenntnisse der Funktionen einer Cloudplattform und 2) wie verteilte Dienste richtig entworfen werden. Diese Artikelserie befasst sich mit dem ersten Punkt, den Möglichkeiten und Einschränkungen der Azure-Plattform bezüglich der Resilienz (manchmal auch als Geschäftskontinuität bezeichnet). Näheres zum zweiten Thema finden Sie in der Artikelserie [Notfallwiederherstellung und hohe Verfügbarkeit für Azure-Anwendungen](https://aka.ms/drtechguide). Wenngleich in dieser Artikelserie Architektur- und Entwurfsmuster erwähnt werden, sind diese nicht der Schwerpunkt der Serie. Leser können die im Abschnitt [Zusätzliche Ressourcen](#additional-resources) genannten Quellen nutzen, um Unterstützung beim Entwurf zu erhalten.

Die Informationen sind in folgende Artikel gegliedert:
##[Wiederherstellung nach lokalen Fehlern](resiliency-technical-guidance-recovery-local-failures.md)
Physische Hardware (z. B. Laufwerke, Server und Netzwerkgeräte) kann gänzlich ausfallen und Ressourcen können bei Lastspitzen erschöpft werden. Dieser Abschnitt beschreibt die Funktionen, die Azure bietet, um auch unter diesen Bedingungen eine hohe Verfügbarkeit sicherzustellen.

##[Wiederherstellung nach einer regionsweiten Dienstunterbrechung von Azure](resiliency-technical-guidance-recovery-loss-azure-region.md)
Weitverbreitete Ausfälle sind selten, aber theoretisch möglich. Gesamte Regionen können aufgrund von Netzwerkfehlern isoliert oder durch Naturkatastrophen physisch beschädigt werden. In diesem Abschnitt wird erläutert, wie mithilfe von Azure-Funktionen Anwendungen erstellt werden, die geografisch verteilte Regionen umfassen.

##[Wiederherstellung von einem lokalem Standort nach Azure](resiliency-technical-guidance-recovery-on-premises-azure.md)
Die Cloud verändert die Wirtschaftlichkeit der Notfallwiederherstellung erheblich. Dadurch können Organisationen Azure nutzen, um einen zweiten Standort für die Notfallwiederherstellung einzurichten. Dies kann zu einem Bruchteil der Kosten für der Erstellung und Verwaltung eines sekundären Datencenters geschehen. Dieser Abschnitt beschreibt die von Azure bereitgestellten Funktionen, um ein lokales Rechenzentrum auf die Cloud auszuweiten.

##[Wiederherstellung nach einer Datenbeschädigung oder unbeabsichtigtem Löschen](resiliency-technical-guidance-recovery-data-corruption.md)
Anwendungen können Fehler aufweisen, die zu einer Datenbeschädigung führen, und Operatoren können versehentlich wichtige Daten löschen. In diesem Abschnitt wird erläutert, welche Funktionen Azure bereitstellt, um Daten zu sichern und im Zustand eines früheren Zeitpunkts wiederherzustellen.

##Weitere Ressourcen

###[Technischer Leitfaden zur Geschäftskontinuität in Azure](resiliency-technical-guidance.md)
Dieser Artikel ist eine detaillierte Liste mit Konzepten und bewährten Methoden zum Erzielen von Geschäftskontinuität und Resilienz mit Microsoft Azure für lokale, hybride und öffentliche Cloudanwendungen.

###[Notfallwiederherstellung und hohe Verfügbarkeit für in Microsoft Azure erstellte Anwendungen](resiliency-disaster-recovery-high-availability-azure-applications.md)
Dieser Artikel ist eine detaillierte Übersicht zu Verfügbarkeit und Notfallwiederherstellung. Sie behandelt die Herausforderung der manuellen Replikation von Referenz- und Transaktionsdaten. Die letzten Abschnitte bieten Zusammenfassungen für verschiedene Arten von Notfallwiederherstellungs-Topologien, die für den höchsten Grad an Verfügbarkeit verschiedene Azure-Regionen umfassen.

###[Checkliste für hohe Verfügbarkeit](resiliency-high-availability-checklist.md)
Dieser Artikel ist eine Liste der Features, Dienste und Designs, mit denen Sie Resilienz und Verfügbarkeit Ihrer Anwendung steigern können.

###[Leitfaden zur Resilienz von Diensten in Microsoft Azure](resiliency-service-guidance-index.md)
Dieser Artikel ist ein Index der Azure-Dienste und der Links zu Leitfäden sowohl zu ihrer Wiederherstellung im Notfall als auch zum Design.

###[Übersicht: Geschäftskontinuität für die Cloud und Notfallwiederherstellung für Datenbanken mit SQL-Datenbank](../sql-database/sql-database-business-continuity.md)
Dieser Artikel behandelt ausschließlich Azure SQL-Datenbanktechniken zur Bereitstellung von Verfügbarkeit, wobei der Schwerpunkt in erster Linie auf Strategien für Sicherung und Wiederherstellung liegt. Wenn Sie Azure SQL-Datenbank im Clouddienst verwenden, sollten Sie diesen Artikel und die zugehörigen Ressourcen durcharbeiten.

###[Hohe Verfügbarkeit und Notfallwiederherstellung für SQL Server auf virtuellen Azure-Computern](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md)
Dieser Artikel erläutert die möglichen Verfügbarkeitsoptionen bei der Verwendung von Infrastructure-as-a-Service (IaaS) zum Hosten von Datenbankdiensten. Hier werden AlwaysOn-Verfügbarkeitsgruppen, Datenbankspiegelung, Protokollversand und Sicherung/Wiederherstellung behandelt. Beachten Sie, dass im selben Bereich auch mehrere Tutorials zur Verfügung stehen, die die Anwendung dieser Techniken demonstrieren.

###[Bewährte Methoden für das Entwerfen umfassender Dienste mit Azure Cloud Services](https://azure.microsoft.com//blog/best-practices-for-designing-large-scale-services-on-windows-azure/)
Dieser Artikel konzentriert sich auf die Entwicklung von hochgradig skalierbaren Cloudarchitekturen. Viele der Techniken zur Verbesserung der Skalierbarkeit erhöhen auch die Verfügbarkeit. Wenn Ihre Anwendung nicht bei erhöhter Last skaliert werden kann, wird die Skalierbarkeit zum Verfügbarkeitsproblem.

###[Sicherung und Wiederherstellung für SQL Server in Azure Virtual Machines](../virtual-machines/virtual-machines-windows-sql-backup-recovery.md)
Dieser Artikel bietet eine technische Anleitung zum Sichern und Wiederherstellen von Microsoft SQL Server in Azure Virtual Machines.

###[Ausfallsicher: Richtlinien für stabile Cloudarchitekturen](https://channel9.msdn.com/Series/FailSafe)
Dieser Artikel enthält Richtlinien zur Erstellung stabiler Cloudarchitekturen, Orientierungshilfen zum Implementieren dieser Architekturen mit Microsoft-Technologien und Anleitungen zum Implementieren dieser Architekturen in bestimmte Szenarien.

###[Technische Fallstudie: „Using cloud technologies to improve disaster recovery“](https://www.microsoft.com/itshowcase/Article/Content/737/Using-cloud-technologies-to-improve-disaster-recovery) (Verwendung von Cloudtechnologien zur Verbesserung der Notfallwiederherstellung)
Dieser Artikel ist eine technische Fallstudie, die zeigt, wie Microsoft IT Azure verwendet, um die Wiederherstellung im Notfall zu verbessern.

##Nächste Schritte
Dieser Artikel gehört zu einer Reihe von Artikeln, die als [Technischer Leitfaden zur Resilienz in Azure](resiliency-technical-guidance.md) dienen. Wenn Sie an anderen Artikeln dieser Serie interessiert sind, können Sie mit dem nächsten Artikel [Wiederherstellung nach lokalen Fehlern](resiliency-technical-guidance-recovery-local-failures.md) fortfahren.

<!---HONumber=AcomDC_0608_2016-->