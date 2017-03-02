---
title: Kostenempfehlungen von Azure Advisor | Microsoft Docs
description: Nutzen Sie Azure Advisor, um die Kosten Ihrer Azure-Bereitstellungen zu optimieren.
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
ms.sourcegitcommit: e7fc6cb4ee4752bb7ed90a8f64edcc7c82ec3a88
ms.openlocfilehash: 7999398f69ed0ce5129dd38f8e9e1994db25f9a5
ms.lasthandoff: 02/21/2017

---

# <a name="advisor-cost-recommendations"></a>Advisor-Empfehlungen zu Kosten

Advisor hilft Ihnen beim Optimieren und Senken Ihrer Gesamtausgaben für Azure, indem Ressourcen im Leerlauf bzw. zu gering ausgelastete Ressourcen ermittelt werden. Kostenempfehlungen finden Sie auf dem Advisor-Dashboard auf der Registerkarte **Kosten**.

![Advisor-Registerkarte „Kosten“](./media/advisor-cost-recommendations/advisor-cost-tab2.png)

## <a name="low-utilization-virtual-machines"></a>Virtuelle Computer mit geringer Auslastung 

Wenngleich bestimmte Anwendungsszenarien absichtlich zu einer geringen Auslastung führen, können Sie sich häufig Kosten sparen, indem Sie Größe und Anzahl virtueller Computer steuern. Advisor überwacht die Verwendung Ihrer virtuellen Computer 14 Tage lang und ermittelt virtuelle Computer mit geringer Auslastung. Virtuelle Computer, bei denen an mindestens vier Tagen die CPU-Auslastung unter 5% und die Netzwerklast unter 7 MB lag, gelten als virtuelle Computer mit geringer Auslastung.

Advisor zeigt Ihnen die geschätzten Kosten der weiteren Ausführung des virtuellen Computers. Sie können den virtuellen Computer herunterfahren oder seine Größe ändern.  

![Advisor-Kostenempfehlungen für das Ändern der Größe virtueller Computer](./media/advisor-cost-recommendations/advisor-cost-resizevms.png)

## <a name="sql-elastic-database-pool-recommendations"></a>Empfehlungen für Pools für elastische SQL-Datenbanken

Advisor ermittelt SQL Server-Instanzen, die vom Erstellen von Pools für elastische Datenbanken profitieren können. Pools für elastische Datenbanken stellen eine einfache und wirtschaftliche Lösung zum Erreichen der Leistungsziele mehrerer Datenbanken dar, die unterschiedliche Nutzungsmuster aufweisen. Weitere Informationen zu Azure-Pools für elastische Datenbanken finden Sie unter [Was ist ein Azure-Pool für elastische Datenbanken?](https://azure.microsoft.com/en-us/documentation/articles/sql-database-elastic-pool/)

![Advisor-Kostenempfehlungen für Pools für elastische Datenbanken](./media/advisor-cost-recommendations/advisor-cost-elasticdbpools.png)

## <a name="how-to-access-cost-recommendations-in-azure-advisor"></a>Zugreifen auf Kostenempfehlungen in Azure Advisor

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie im linken Navigationsbereich auf **Weitere Dienste**, scrollen Sie im Dienstmenübereich nach unten zu **Überwachung und Verwaltung**, und klicken Sie dann auf **Azure Advisor**. Dadurch wird das Advisor-Dashboard gestartet. 
3. Klicken Sie auf dem Advisor-Dashboard auf die Registerkarte **Kosten**, wählen Sie das Abonnement aus, für das Sie Empfehlungen erhalten möchten, und klicken Sie dann auf **Empfehlungen abrufen**

> [!NOTE]
> Für den Zugriff auf die Empfehlungen von Ratgeber müssen Sie Ihr Abonnement zunächst bei Ratgeber **registrieren**. Ein Abonnement wird registriert, wenn ein **Abonnementbesitzer** das Ratgeber-Dashboard startet und auf die Schaltfläche **Abrufen von Empfehlungen** klickt. Dies ist eine **einmalige Aufgabe**. Sobald ein Abonnement registriert wurde, können **Besitzer**, **Mitwirkende** oder **Leser** eines Abonnements, einer Ressourcengruppe oder einer bestimmten Ressource auf die Empfehlungen von Ratgeber zugreifen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Advisor-Empfehlungen finden Sie in den folgenden Ressourcen:
-  [Einführung in Advisor](advisor-overview.md)
-  [Erste Schritte](advisor-get-started.md)
-  [Advisor-Empfehlungen für hohe Verfügbarkeit](advisor-cost-recommendations.md)
-  [Advisor-Empfehlungen zur Sicherheit](advisor-cost-recommendations.md)
-  [Advisor-Empfehlungen zur Leistung](advisor-cost-recommendations.md)

