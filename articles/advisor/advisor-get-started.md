---
title: Erste Schritte mit Azure Advisor | Microsoft Docs
description: Erste Schritte mit Azure Advisor.
services: advisor
documentationcenter: NA
author: manbeenkohli
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/10/2017
ms.author: makohli
translationtype: Human Translation
ms.sourcegitcommit: e7fc6cb4ee4752bb7ed90a8f64edcc7c82ec3a88
ms.openlocfilehash: 8ed18f6f62b6d2895e2470df1c3bdd448ea98210
ms.lasthandoff: 02/21/2017

---

# <a name="get-started-with-azure-advisor"></a>Erste Schritte mit Azure Advisor

Erfahren Sie, wie Sie im Azure-Portal auf Advisor zugreifen und wie Sie Empfehlungen abrufen, umsetzen, suchen und aktualisieren.

## <a name="get-advisor-recommendations"></a>Abrufen von Advisor-Empfehlungen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie im linken Navigationsbereich auf **Weitere Dienste**, scrollen Sie im Dienstmenübereich nach unten zu **Überwachung und Verwaltung**, und klicken Sie dann auf **Azure Advisor**. Dadurch wird das Advisor-Dashboard gestartet.

  ![Zugreifen auf Azure Advisor im Azure-Portal](./media/advisor-overview/advisor-azure-portal-menu.png) 

3. Wählen Sie auf dem Advisor-Dashboard das Abonnement aus, für das Sie Empfehlungen erhalten möchten. Das Advisor-Dashboard zeigt personalisierte Empfehlungen für ein ausgewähltes Abonnement. 
4. Um Empfehlungen für eine bestimmte Kategorie zu erhalten, klicken Sie auf eine dieser Registerkarten: **Hohe Verfügbarkeit**, **Sicherheit**, **Leistung** oder **Kosten**.
 
> [!NOTE]
> Für den Zugriff auf die Empfehlungen von Ratgeber müssen Sie Ihr Abonnement zunächst bei Ratgeber **registrieren**. Ein Abonnement wird registriert, wenn ein **Abonnementbesitzer** das Ratgeber-Dashboard startet und auf die Schaltfläche **Abrufen von Empfehlungen** klickt. Dies ist eine **einmalige Aufgabe**. Sobald ein Abonnement registriert wurde, können **Besitzer**, **Mitwirkende** oder **Leser** eines Abonnements, einer Ressourcengruppe oder einer bestimmten Ressource auf die Empfehlungen von Ratgeber zugreifen.

  ![Azure Advisor-Dashboard](./media/advisor-overview/advisor-all-tab.png)

## <a name="get-advisor-recommendation-details-and-implement-a-recommendation"></a>Abrufen von Details zu einer Advisor-Empfehlung und Umsetzen einer Empfehlung

Das Blatt **Empfehlung** in Advisor bietet zusätzliche Informationen zur Advisor-Empfehlung. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und starten Sie [Azure Advisor](https://aka.ms/azureadvisordashboard).
2. Klicken Sie auf dem Dashboard **Advisor-Empfehlungen** auf **Empfehlung abrufen**.
3. Klicken Sie in der Liste auf eine der Empfehlungen, die Sie sich näher anschauen möchten. Dadurch wird das Blatt „Empfehlungen“ geöffnet.
4. Überprüfen Sie auf dem Blatt „Empfehlungen“ die Informationen zu Aktionen, die Sie ausführen können, um ein potenzielles Problem zu beheben oder um eine Möglichkeit zur Kostenersparnis zu nutzen. 
  
  ![Beispiel einer Aktion bei einer Advisor-Empfehlung](./media/advisor-overview/advisor-recommendation-action-example.png)

## <a name="search-for-advisor-recommendations"></a>Suchen nach Advisor-Empfehlungen

Sie können für ein bestimmtes Abonnement oder eine Ressourcengruppe Empfehlungen suchen. Sie können auch Empfehlungen nach Status suchen.

1. Melden Sie sich beim Azure-Portal an, und starten Sie Azure Advisor.
2. Suchen Sie nach den Empfehlungen, indem Sie nach Abonnements, Ressourcengruppen und Empfehlungsstatus filtern (**Aktiv** oder **Zurückgestellt**).
3. Klicken Sie auf **Empfehlungen abrufen**, um eine Liste von Advisor-Empfehlungen basierend auf Ihren Suchfiltern abzurufen.

  ![](./media/advisor-get-started/advisor-search.png)

## <a name="snooze-advisor-recommendations"></a>Zurückstellen von Advisor-Empfehlungen

1. Melden Sie sich beim Azure-Portal an, und starten Sie Azure Advisor.
2. Klicken Sie auf **Empfehlung abrufen** und dann in der eingeblendeten Liste auf eine Empfehlung.
3. Klicken Sie auf dem Blatt **Empfehlungen** auf **Zurückstellen**.  Sie können einen Zurückstellungszeitraum angeben oder **Nie** auswählen, um die Empfehlung zu verwerfen.

  ![Beispiel einer Aktion bei einer Advisor-Empfehlung](./media/advisor-get-started/advisor-snooze.png)



## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Advisor finden Sie in den folgenden Ressourcen:
-  [Einführung in Azure Advisor](advisor-overview.md)
-  [Advisor-Empfehlungen für hohe Verfügbarkeit](advisor-high-availability-recommendations.md)
-  [Advisor-Empfehlungen zur Sicherheit](advisor-security-recommendations.md)
-  [Advisor-Empfehlungen zur Leistung](advisor-performance-recommendations.md)
-  [Advisor-Empfehlungen zu Kosten](advisor-performance-recommendations.md)

