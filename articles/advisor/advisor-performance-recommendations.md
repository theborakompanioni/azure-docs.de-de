---
title: Azure Advisor-Empfehlungen zur Leistung | Microsoft Docs
description: Nutzen Sie Advisor, um die Leistung Ihrer Azure-Bereitstellungen zu optimieren.
services: advisor
documentationcenter: NA
author: kumudd
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
translationtype: Human Translation
ms.sourcegitcommit: a560aa9cc6c70fd3827f649627466c9071b8ab8b
ms.openlocfilehash: c350134e8cea5771900e2e88b02b572c20e9d5bd

---

# <a name="advisor-performance-recommendations"></a>Advisor-Empfehlungen zur Leistung

Dank der Empfehlungen von Advisor zur Leistung können Sie die Geschwindigkeit und Reaktionsfähigkeit Ihrer unternehmenskritischen Anwendungen verbessern. Empfehlungen von Advisor zur Leistung erhalten Sie auf dem Advisor-Dashboard auf der Registerkarte **Leistung**.

![Advisor-Registerkarte „Leistung“](./media/advisor-performance-recommendations/advisor-performance-tab.png)

## <a name="sql-database-recommendations"></a>Empfehlungen für SQL-Datenbank

Advisor bietet Ihnen eine einheitliche, konsolidierte Ansicht der Empfehlungen für alle Ihre Azure-Ressourcen. Azure Advisor und Azure SQL Database Advisor sind integriert, um Ihnen Empfehlungen zum Verbessern der Leistung Ihrer SQL Azure-Datenbank zu bieten. SQL Database Advisor bewertet die Leistung durch eine Analyse des Nutzungsverlaufs Ihrer SQL Azure-Datenbank. Anschließend werden Empfehlungen gegeben, die für die typische Workload Ihrer Datenbank am besten geeignet sind. 

> [!NOTE]
> Um Empfehlungen zu erhalten, muss eine Datenbank ungefähr eine Woche lang genutzt werden, und innerhalb dieser Woche müssen durchweg Aktivitäten erfolgen. SQL Database Advisor kann leichter für einheitliche Abfragemuster als für zufällige, unregelmäßige Aktivitätsspitzen optimiert werden.

Weitere Informationen zu SQL Database Advisor finden Sie unter [SQL Database Advisor](https://azure.microsoft.com/en-us/documentation/articles/sql-database-advisor/).

![Empfehlungen für SQL-Datenbank](./media/advisor-performance-recommendations/advisor-performance-sql.png)

## <a name="redis-cache-recommendations"></a>Empfehlungen für Redis Cache

Azure Advisor ermittelt Redis Cache-Instanzen, bei denen die Leistung ggf. durch eine hohe Auslastung von Arbeitsspeicher oder Netzwerkbandbreite, Serverlast oder große Anzahl von Clientverbindungen beeinträchtigt wird. Darüber hinaus werden bewährte Methoden empfohlen, um potenzielle Probleme zu vermeiden. Weitere Informationen zu Redis Cache-Empfehlungen finden Sie unter [Redis Cache Advisor](https://azure.microsoft.com/en-us/documentation/articles/cache-configure/#redis-cache-advisor).


## <a name="app-services-recommendations"></a>Empfehlungen für App Services

Azure Advisor befolgt empfohlene bewährte Methoden zum Verbessern der App Services-Leistung und Erkennen relevanter Plattformfunktionen. Beispielempfehlungen für App Services sind u.a.:
- Erkennen von Instanzen, bei denen Arbeitsspeicher- oder CPU-Ressourcen durch App-Laufzeiten ausgeschöpft sind, samt Abhilfeoptionen
- Erkennen von Instanzen, bei denen durch Zusammenstellen von Ressourcen wie Web-Apps und Datenbanken die Leistung verbessert und Kosten gesenkt werden können 

Weitere Informationen zu App Services-Empfehlungen finden Sie unter [Bewährte Methoden für Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/app-service-best-practices/).
![Empfehlungen für App Services](./media/advisor-performance-recommendations/advisor-performance-app-service.png)

## <a name="how-to-access-performance-recommendations-in-advisor"></a>Zugreifen auf Advisor-Empfehlungen zur Leistung

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie im linken Navigationsbereich auf **Weitere Dienste**, scrollen Sie im Dienstmenübereich nach unten zu **Überwachung und Verwaltung**, und klicken Sie dann auf **Azure Advisor**. Dadurch wird das Advisor-Dashboard gestartet. 
3. Klicken Sie auf dem Advisor-Dashboard auf die Registerkarte **Leistung**, und wählen Sie das Abonnement aus, für das Sie Empfehlungen erhalten möchten.

> [!NOTE]
> Advisor generiert Empfehlungen für Abonnements, in denen Ihnen die Rolle **Besitzer, Mitwirkender oder Leser** zugewiesen ist.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Advisor-Empfehlungen finden Sie in den folgenden Ressourcen:

-  [Einführung in Advisor](advisor-overview.md)
-  [Erste Schritte mit Advisor](advisor-get-started.md)
-  [Advisor-Empfehlungen für hohe Verfügbarkeit](advisor-high-availability-recommendations.md)
-  [Advisor-Empfehlungen zur Sicherheit](advisor-security-recommendations.md)
-  [Advisor-Empfehlungen zu Kosten](advisor-performance-recommendations.md)



<!--HONumber=Nov16_HO3-->


