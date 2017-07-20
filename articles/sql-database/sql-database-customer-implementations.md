---
title: "Azure SQL-Datenbank-Kundenimplementierungen – technische Details | Microsoft-Dokumentation"
description: "Hier erfahren Sie technische Details zu Kundenimplementierungen von Azure SQL-Datenbank zum Lösen von Geschäftsproblemen."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 00c8a713-f20c-4d6b-b8b7-0c1b9ba5f05b
ms.service: sql-database
ms.custom: reference
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/03/2017
ms.author: carlrab
ms.translationtype: Human Translation
ms.sourcegitcommit: f7479260c7c2e10f242b6d8e77170d4abe8634ac
ms.openlocfilehash: 24c345e173ffe3cd215552b604e8b6cc525d8aa7
ms.contentlocale: de-de
ms.lasthandoff: 06/21/2017


---
# <a name="azure-sql-database-customer-implementation-technical-studies"></a>Azure SQL-Datenbank-Kundenimplementierungen – technische Details – Studien

- [Daxko](sql-database-implementation-daxko.md): Daxko/CSI Software stand vor einer großen Herausforderung: Der Kundenstamm an Fitness- und Erholungszentren wuchs dank des Erfolgs der umfassenden Unternehmenssoftwarelösung sehr schnell, aber die IT-Infrastrukturanforderungen für diesen wachsenden Kundenstamm stellten die IT-Mitarbeiter des Unternehmens auf eine harte Probe. Das Unternehmen hatte mit einem immer höheren Betriebsaufwand zu kämpfen, insbesondere für die Verwaltung der wachsenden Datenbanken. Schlimmer noch: Dieser Betriebsaufwand beanspruchte auch Entwicklungsressourcen, die eigentlich für neue Initiativen benötigt wurden, beispielsweise für neue Mobilitätsfunktionen für die Software des Unternehmens.

- [GEP](sql-database-implementation-gep.md): GEP stellt Software und Dienste bereit, die es Führungskräften in der Beschaffung auf der ganzen Welt ermöglichen, ihren Einfluss auf den operativen Betrieb, die Strategien und die finanzielle Performance ihrer Unternehmen zu maximieren. Das Unternehmen bietet neben Beratungs- und Verwaltungsdiensten die umfassende cloudbasierte Softwareplattform SMART by GEP® für die Beschaffung. Als es darum ging, Lösungen wie SMART by GEP mit den eigenen lokalen Datencentern zu unterstützen, geriet GEP jedoch an seine Grenzen: Es waren massive Investitionen notwendig, und durch gesetzliche Anforderungen in anderen Ländern wäre das Ausmaß der Investitionen noch gewaltiger geworden. Durch den Umstieg auf die Cloud konnte GEP IT-Ressourcen freisetzen. Gleichzeitig muss sich das Unternehmen jetzt weniger Sorgen um den IT-Betrieb machen und kann sich vermehrt darauf konzentrieren, für seine Kunden auf der ganzen Welt neue Wertschöpfungsquellen zu entwickeln.

- [SnelStart](sql-database-implementation-snelstart.md): SnelStart entwickelt beliebte Software für das Finanz- und Businessmanagement für kleine und mittelgroße Unternehmen in den Niederlanden. 110 Mitarbeiter (einschließlich 35 IT-Mitarbeitern) kümmern sich um 55.000 Kunden. Durch die Umstellung von Desktopsoftware zu einem auf Azure aufgebauten SaaS-Angebot (Software-as-a-Service) kann SnelStart die integrierten Dienste optimal nutzen. Die Verwaltung wird mit einer vertrauten Umgebung in C# automatisiert, und dank elastischer Pools lassen sich Leistung und Skalierbarkeit optimieren, ohne Ressourcen für die Kunden über- oder unterzudimensionieren. Azure bietet SnelStart die Flexibilität und Agilität, Kundendaten zwischen lokalen Systemen und der Cloud zu verschieben.

- [Umbraco](sql-database-implementation-umbraco.md): Um die Kundenbereitstellungen zu vereinfachen, hat Umbraco UaaS (Umbraco-as-a-Service) hinzugefügt, ein SaaS-Angebot (Software-as-a-Service), das lokale Bereitstellungen überflüssig macht, integrierte Skalierungsmöglichkeiten bietet und den Verwaltungsaufwand minimiert. Entwickler müssen sich nicht mehr mit der Lösungsverwaltung beschäftigen, sondern können sich auf Produktinnovationen konzentrieren. Umbraco kann all diese Vorteile bereitstellen, da das Unternehmen sich auf das flexible PaaS-Modell (Platform-as-a-Service) von Microsoft Azure verlassen kann.

- [Quorum](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database): Quorum verdoppelt die Arbeitslast der wichtigen Datenbank, während die DTU mit der SQL-Datenbank um 70 % verringert wird.

- [Quest](https://customers.microsoft.com/en-US/story/quest): Mit dem Spotlight on SQL Server Enterprise-Dienst von Quest erhalten Datenbankexperten die besten Tools, die zum Sichern von Daten, Verschieben von Daten und Überwachen von Datenbankvorgängen verfügbar sind. Durch Spotlight können SQL Server-Datenbankadministratoren mit Microsoft Azure und Azure SQL-Datenbank nicht nur die Leistung von SQL Server überwachen, sondern auch Leistungsprobleme erkennen, diagnostizieren und eine Möglichkeit für ihre Behebung bereitstellen – ganz gleich, ob sie sich an ihrem Arbeitsplatz befinden oder von zu Hause aus arbeiten.

- [Microsoft Synamics](https://customers.microsoft.com/story/dynamics365operationsproductteam): In dieser kurzen Fallstudie werden die Best Practices und die Erkenntnisse vorgestellt, die die Dynamics 365 for Operations-Produktteams bei der Migration zu Azure SQL-Datenbank gewonnen haben, um Kunden ein vollständig verwaltetes SaaS-Angebot (Software as a Service) zu bieten. Mithilfe von Azure SQL-Datenbank war das Dynamics 365 for Operations-Team in der Lage, den Dienst mit einer deutlich kleineren Anzahl von Mitarbeitern zu verwalten und zu betreiben und mühelos leistungsstarke Verwaltungsfunktionen zu skalieren. Hierzu zählen z.B. Funktionen zur automatischen Datenbanksicherung, Aufbewahrung von Datenbanksicherungen sowie Funktionen für hohe Verfügbarkeit und die Notfallwiederherstellung. Diese Faktoren und die Möglichkeit, Datenbanken mit einfacher Automatisierung auszustatten, hat Azure SQL-Datenbank zu einer hervorragenden Plattform für die Bereitstellung umfangreicher Dienste gemacht.

