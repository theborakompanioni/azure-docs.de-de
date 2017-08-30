---
title: Azure Advisor-Empfehlungen zur Leistung | Microsoft Docs
description: Nutzen Sie den Ratgeber, um die Leistung Ihrer Azure-Bereitstellungen zu optimieren.
services: advisor
documentationcenter: NA
author: KumudD
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: 5fb86c60b2d1f258dde5636ff8854b6f30f7f1c8
ms.contentlocale: de-de
ms.lasthandoff: 03/31/2017

---

# <a name="advisor-performance-recommendations"></a>Advisor-Empfehlungen zur Leistung

Dank der Empfehlungen des Azure-Ratgebers zur Leistung können Sie die Geschwindigkeit und Reaktionsfähigkeit Ihrer unternehmenskritischen Anwendungen verbessern. Empfehlungen des Ratgebers zur Leistung erhalten Sie auf dem Ratgeberdashboard auf der Registerkarte **Leistung**.

![Advisor-Registerkarte „Leistung“](./media/advisor-performance-recommendations/advisor-performance-tab.png)

## <a name="improve-database-performance-with-sql-db-advisor"></a>Verbessern der Datenbankleistung mit SQL DB Advisor

Advisor bietet Ihnen eine einheitliche, konsolidierte Ansicht der Empfehlungen für alle Ihre Azure-Ressourcen. Azure Advisor und Azure SQL Database Advisor sind integriert, um Ihnen Empfehlungen zum Verbessern der Leistung Ihrer SQL Azure-Datenbank zu bieten. SQL Database Advisor bewertet die Leistung durch eine Analyse des Nutzungsverlaufs Ihrer SQL Azure-Datenbank. Anschließend werden Empfehlungen gegeben, die für die typische Workload der Datenbank am besten geeignet sind. 

> [!NOTE]
> Um Empfehlungen zu erhalten, muss eine Datenbank ungefähr eine Woche lang genutzt werden, und innerhalb dieser Woche müssen durchweg Aktivitäten erfolgen. SQL Database Advisor kann leichter für einheitliche Abfragemuster als für zufällige Aktivitätsspitzen optimiert werden.

Weitere Informationen zu SQL Database Advisor finden Sie unter [SQL Database Advisor](https://azure.microsoft.com/en-us/documentation/articles/sql-database-advisor/).

![Empfehlungen für SQL-Datenbank](./media/advisor-performance-recommendations/advisor-performance-sql.png)

## <a name="improve-redis-cache-performance-and-reliability"></a>Verbessern der Leistung und Zuverlässigkeit von Redis-Cache

Der Ratgeber ermittelt Redis Cache-Instanzen, bei denen die Leistung ggf. durch eine hohe Auslastung von Arbeitsspeicher oder Netzwerkbandbreite, Serverlast oder große Anzahl von Clientverbindungen beeinträchtigt wird. Darüber hinaus empfiehlt der Ratgeber bewährte Methoden, um potenzielle Probleme zu vermeiden. Weitere Informationen zu Redis Cache-Empfehlungen finden Sie unter [Redis Cache Advisor](https://azure.microsoft.com/en-us/documentation/articles/cache-configure/#redis-cache-advisor).


## <a name="improve-app-service-performance-and-reliability"></a>Verbessern der Leistung und Zuverlässigkeit von App Services

Azure Advisor befolgt empfohlene bewährte Methoden zum Verbessern der App Services-Leistung und Erkennen relevanter Plattformfunktionen. Beispielempfehlungen für App Services sind u.a.:
* Erkennen von Instanzen, bei denen Arbeitsspeicher- oder CPU-Ressourcen durch App-Laufzeiten ausgeschöpft sind, samt Abhilfeoptionen
* Erkennen von Instanzen, bei denen durch Zusammenstellen von Ressourcen wie Web-Apps und Datenbanken die Leistung verbessert und Kosten gesenkt werden können 

Weitere Informationen zu App Services-Empfehlungen finden Sie unter [Bewährte Methoden für Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/app-service-best-practices/).
![Empfehlungen für App Services](./media/advisor-performance-recommendations/advisor-performance-app-service.png)

## <a name="how-to-access-performance-recommendations-in-advisor"></a>Zugreifen auf Advisor-Empfehlungen zur Leistung

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

2. Klicken Sie im linken Bereich auf **Weitere Dienste**.

3. Klicken Sie im Dienstmenübereich unter **Überwachung und Verwaltung** auf **Azure-Ratgeber**.  
 Das Ratgeberdashboard wird angezeigt.

4. Klicken Sie auf dem Ratgeberdashboard auf die Registerkarte **Leistung**.

5. Wählen Sie das Abonnement aus, für das Sie Empfehlungen erhalten möchten, und klicken Sie anschließend auf **Empfehlungen abrufen**.

> [!NOTE]
> Für den Zugriff auf die Empfehlungen des Ratgebers müssen Sie Ihr Abonnement zunächst beim Ratgeber *registrieren*. Ein Abonnement wird registriert, wenn ein *Abonnementbesitzer* das Ratgeberdashboard aufruft und auf die Schaltfläche **Empfehlungen abrufen** klickt. Dies ist eine *einmalige Aufgabe*. Sobald ein Abonnement registriert wurde, können *Besitzer*, *Mitwirkende* oder *Leser* eines Abonnements, einer Ressourcengruppe oder einer bestimmten Ressource auf die Ratgeberempfehlungen zugreifen.

## <a name="next-steps"></a>Nächste Schritte

Hier finden Sie weitere Informationen zu Empfehlungen des Ratgebers:

* [Einführung in Advisor](advisor-overview.md)
* [Erste Schritte mit Advisor](advisor-get-started.md)
* [Advisor-Empfehlungen zu Kosten](advisor-performance-recommendations.md)
* [Advisor-Empfehlungen für hohe Verfügbarkeit](advisor-high-availability-recommendations.md)
* [Advisor-Empfehlungen zur Sicherheit](advisor-security-recommendations.md)


