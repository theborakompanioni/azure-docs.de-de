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
ms.custom: customer implementations
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/03/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 97acd09d223e59fbf4109bc8a20a25a2ed8ea366
ms.openlocfilehash: b05b24bf9a14570b07c2c1ab3076835c7167b647
ms.lasthandoff: 03/10/2017


---
# <a name="azure-sql-database-customer-implementation-technical-studies"></a>Azure SQL-Datenbank-Kundenimplementierungen – technische Details – Studien

- [Daxko](sql-database-implementation-daxko.md): Daxko/CSI Software stand vor einer großen Herausforderung: Der Kundenstamm an Fitness- und Erholungszentren wuchs dank des Erfolgs der umfassenden Unternehmenssoftwarelösung sehr schnell, aber die IT-Infrastrukturanforderungen für diesen wachsenden Kundenstamm stellten die IT-Mitarbeiter des Unternehmens auf eine harte Probe. Das Unternehmen hatte mit einem immer höheren Betriebsaufwand zu kämpfen, insbesondere für die Verwaltung der wachsenden Datenbanken. Schlimmer noch: Dieser Betriebsaufwand beanspruchte auch Entwicklungsressourcen, die eigentlich für neue Initiativen benötigt wurden, beispielsweise für neue Mobilitätsfunktionen für die Software des Unternehmens.

- [GEP](sql-database-implementation-gep.md): GEP stellt Software und Dienste bereit, die es Führungskräften in der Beschaffung auf der ganzen Welt ermöglichen, ihren Einfluss auf den operativen Betrieb, die Strategien und die finanzielle Performance ihrer Unternehmen zu maximieren. Das Unternehmen bietet neben Beratungs- und Verwaltungsdiensten die umfassende cloudbasierte Softwareplattform SMART by GEP® für die Beschaffung. Als es darum ging, Lösungen wie SMART by GEP mit den eigenen lokalen Datencentern zu unterstützen, geriet GEP jedoch an seine Grenzen: Es waren massive Investitionen notwendig, und durch gesetzliche Anforderungen in anderen Ländern wäre das Ausmaß der Investitionen noch gewaltiger geworden. Durch den Umstieg auf die Cloud konnte GEP IT-Ressourcen freisetzen. Gleichzeitig muss sich das Unternehmen jetzt weniger Sorgen um den IT-Betrieb machen und kann sich vermehrt darauf konzentrieren, für seine Kunden auf der ganzen Welt neue Wertschöpfungsquellen zu entwickeln.

- [SnelStart](sql-database-implementation-snelstart.md): SnelStart entwickelt beliebte Software für das Finanz- und Businessmanagement für kleine und mittelgroße Unternehmen in den Niederlanden. 110 Mitarbeiter (einschließlich 35 IT-Mitarbeitern) kümmern sich um 55.000 Kunden. Durch die Umstellung von Desktopsoftware zu einem auf Azure aufgebauten SaaS-Angebot (Software-as-a-Service) kann SnelStart die integrierten Dienste optimal nutzen. Die Verwaltung wird mit einer vertrauten Umgebung in C# automatisiert, und dank elastischer Pools lassen sich Leistung und Skalierbarkeit optimieren, ohne Ressourcen für die Kunden über- oder unterzudimensionieren. Azure bietet SnelStart die Flexibilität und Agilität, Kundendaten zwischen lokalen Systemen und der Cloud zu verschieben.

- [Umbraco](sql-database-implementation-umbraco.md): Um die Kundenbereitstellungen zu vereinfachen, hat Umbraco UaaS (Umbraco-as-a-Service) hinzugefügt, ein SaaS-Angebot (Software-as-a-Service), das lokale Bereitstellungen überflüssig macht, integrierte Skalierungsmöglichkeiten bietet und den Verwaltungsaufwand minimiert. Entwickler müssen sich nicht mehr mit der Lösungsverwaltung beschäftigen, sondern können sich auf Produktinnovationen konzentrieren. Umbraco kann all diese Vorteile bereitstellen, da das Unternehmen sich auf das flexible PaaS-Modell (Platform-as-a-Service) von Microsoft Azure verlassen kann.
