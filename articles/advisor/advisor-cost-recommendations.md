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
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: 5eef2116f238b477fa8de46ce7b25728c393739c
ms.lasthandoff: 03/31/2017

---

# <a name="advisor-cost-recommendations"></a>Advisor-Empfehlungen zu Kosten

Advisor hilft Ihnen beim Optimieren und Senken Ihrer Gesamtausgaben für Azure, indem Ressourcen im Leerlauf bzw. zu gering ausgelastete Ressourcen ermittelt werden. Kostenempfehlungen finden Sie auf dem Ratgeberdashboard auf der Registerkarte **Kosten**.

![Ratgeberregisterkarte „Kosten“](./media/advisor-cost-recommendations/advisor-cost-tab2.png)

## <a name="optimize-virtual-machine-spend-by-resizing-underutilized-instances"></a>Optimieren der Kosten für virtuelle Computer durch Ändern der Größe von nicht ausgelasteten Instanzen 
Obwohl bestimmte Anwendungsszenarien zielgerichtet eine geringe Auslastung bewirken, können Sie häufig Kosten sparen, indem Sie Größe und Anzahl Ihrer virtuellen Computer steuern. Der Ratgeber überwacht die Verwendung Ihrer virtuellen Computer 14 Tage lang und ermittelt virtuelle Computer mit geringer Auslastung. Virtuelle Computer, bei denen an mindestens vier Tagen die CPU-Auslastung unter 5 Prozent und die Netzwerklast unter 7 MB lag, gelten als virtuelle Computer mit geringer Auslastung.

Der Ratgeber gibt die geschätzten Kosten für die weitere Ausführung des virtuellen Computers an, sodass Sie entscheiden können, ob Sie diesen herunterfahren oder seine Größe ändern.  

![Advisor-Kostenempfehlungen für das Ändern der Größe virtueller Computer](./media/advisor-cost-recommendations/advisor-cost-resizevms.png)

## <a name="use-a-cost-effective-solution-to-manage-performance-goals-of-multiple-sql-databases"></a>Verwenden einer kostengünstigen Lösung, um die Leistungsziele mehrerer SQL-Datenbanken zu erreichen
Advisor ermittelt SQL Server-Instanzen, die vom Erstellen von Pools für elastische Datenbanken profitieren können. Pools für elastische Datenbanken stellen eine einfache und wirtschaftliche Lösung zum Erreichen der Leistungsziele mehrerer Datenbanken dar, die unterschiedliche Nutzungsmuster aufweisen. Unter [Was ist ein Azure-Pool für elastische Datenbanken?](https://azure.microsoft.com/en-us/documentation/articles/sql-database-elastic-pool/) finden Sie Informationen zu Azure-Pools für elastische Datenbanken.

![Advisor-Kostenempfehlungen für Pools für elastische Datenbanken](./media/advisor-cost-recommendations/advisor-cost-elasticdbpools.png)

## <a name="how-to-access-cost-recommendations-in-azure-advisor"></a>Zugreifen auf Kostenempfehlungen in Azure Advisor

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

2. Klicken Sie im linken Bereich auf **Weitere Dienste**.

3. Klicken Sie im Dienstmenübereich unter **Überwachung und Verwaltung** auf **Azure-Ratgeber**.  
 Das Ratgeberdashboard wird angezeigt.

4. Klicken Sie auf dem Ratgeberdashboard auf die Registerkarte **Kosten**.

5. Wählen Sie das Abonnement aus, für das Sie Empfehlungen erhalten möchten, und klicken Sie anschließend auf **Empfehlungen abrufen**.

> [!NOTE]
> Für den Zugriff auf die Empfehlungen des Ratgebers müssen Sie Ihr Abonnement zunächst beim Ratgeber *registrieren*. Ein Abonnement wird registriert, wenn ein *Abonnementbesitzer* das Ratgeberdashboard aufruft und auf die Schaltfläche **Empfehlungen abrufen** klickt. Dies ist eine *einmalige Aufgabe*. Sobald ein Abonnement registriert wurde, können *Besitzer*, *Mitwirkende* oder *Leser* eines Abonnements, einer Ressourcengruppe oder einer bestimmten Ressource auf die Ratgeberempfehlungen zugreifen.

## <a name="next-steps"></a>Nächste Schritte

Hier finden Sie weitere Informationen zu Empfehlungen des Ratgebers:
* [Einführung in Advisor](advisor-overview.md)
* [Erste Schritte](advisor-get-started.md)
* [Advisor-Empfehlungen zur Leistung](advisor-cost-recommendations.md)
* [Advisor-Empfehlungen für hohe Verfügbarkeit](advisor-cost-recommendations.md)
* [Advisor-Empfehlungen zur Sicherheit](advisor-cost-recommendations.md)

