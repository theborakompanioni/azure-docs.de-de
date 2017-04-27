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
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: a662841bebda460d4225e080f16705b3f16fdc46
ms.lasthandoff: 03/31/2017

---

# <a name="get-started-with-azure-advisor"></a>Erste Schritte mit Azure Advisor

Erfahren Sie, wie Sie über das Azure-Portal auf den Ratgeber zugreifen und wie Sie Empfehlungen abrufen, umsetzen, suchen und aktualisieren.

## <a name="get-advisor-recommendations"></a>Abrufen von Advisor-Empfehlungen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

2. Klicken Sie im linken Bereich auf **Weitere Dienste**.

3. Klicken Sie im Dienstmenübereich unter **Überwachung und Verwaltung** auf **Azure-Ratgeber**.  
 Das Ratgeberdashboard wird angezeigt.

   ![Zugreifen auf Azure Advisor im Azure-Portal](./media/advisor-overview/advisor-azure-portal-menu.png) 

4. Wählen Sie auf dem Ratgeberdashboard das Abonnement aus, zu dem Sie Empfehlungen erhalten möchten.  
Das Advisor-Dashboard zeigt personalisierte Empfehlungen für ein ausgewähltes Abonnement. 

5. Um Empfehlungen zu einer bestimmten Kategorie zu erhalten, klicken Sie auf eine dieser Registerkarten: **Hohe Verfügbarkeit**, **Sicherheit**, **Leistung** oder **Kosten**.
 
> [!NOTE]
> Für den Zugriff auf die Empfehlungen des Ratgebers müssen Sie Ihr Abonnement zunächst beim Ratgeber *registrieren*. Ein Abonnement wird registriert, wenn ein *Abonnementbesitzer* das Ratgeberdashboard aufruft und auf die Schaltfläche **Empfehlungen abrufen** klickt. Dies ist eine *einmalige Aufgabe*. Sobald ein Abonnement registriert wurde, können *Besitzer*, *Mitwirkende* oder *Leser* eines Abonnements, einer Ressourcengruppe oder einer bestimmten Ressource auf die Ratgeberempfehlungen zugreifen.

  ![Azure Advisor-Dashboard](./media/advisor-overview/advisor-all-tab.png)

## <a name="get-advisor-recommendation-details-and-implement-a-solution"></a>Abrufen von Details zu einer Ratgeberempfehlung und Umsetzen einer Lösung

Das Blatt **Empfehlung** im Ratgeber bietet zusätzliche Informationen zur Empfehlung. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und starten Sie den [Azure-Ratgeber](https://aka.ms/azureadvisordashboard).

2. Klicken Sie auf dem Dashboard **Ratgeberempfehlungen** auf **Empfehlung abrufen**.

3. Klicken Sie in der Liste auf eine der Empfehlungen, die Sie sich näher ansehen möchten.  
Das Blatt **Empfehlungen** wird angezeigt.

4. Überprüfen Sie auf dem Blatt **Empfehlungen** die Informationen zu Aktionen, die Sie ausführen können, um ein potenzielles Problem zu beheben oder um eine Möglichkeit zur Kostenersparnis zu nutzen. 
  
  ![Das Blatt „Ratgeberempfehlungen“](./media/advisor-overview/advisor-recommendation-action-example.png)

## <a name="search-for-advisor-recommendations"></a>Suchen nach Advisor-Empfehlungen

Sie können für ein bestimmtes Abonnement oder eine Ressourcengruppe Empfehlungen suchen. Sie können auch Empfehlungen nach Status suchen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und starten Sie den [Azure-Ratgeber](https://aka.ms/azureadvisordashboard).

2. Suchen Sie nach den Empfehlungen, indem Sie nach Abonnements, Ressourcengruppen und Empfehlungsstatus filtern (**Aktiv** oder **Zurückgestellt**).

3. Klicken Sie auf **Empfehlungen abrufen**, um eine Liste von Ratgeberempfehlungen basierend auf Ihren Suchfilterkriterien abzurufen.

  ![Suchfilterkriterien für den Ratgeber](./media/advisor-get-started/advisor-search.png)

## <a name="snooze-or-dismiss-advisor-recommendations"></a>Zurückstellen oder Verwerfen von Ratgeberempfehlungen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und starten Sie den [Azure-Ratgeber](https://aka.ms/azureadvisordashboard).

2. Klicken Sie auf **Empfehlung abrufen** und dann in der Liste der Empfehlungen auf eine Empfehlung.

3. Klicken Sie auf dem Blatt **Empfehlungen** auf **Zurückstellen**.  

   ![Beispiel einer Aktion bei einer Advisor-Empfehlung](./media/advisor-get-started/advisor-snooze.png)

4. Geben Sie einen Zurückstellungszeitraum an, oder wählen Sie **Nie** aus, um die Empfehlung zu verwerfen.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Ratgeber:
* [Einführung in Azure Advisor](advisor-overview.md)
* [Advisor-Empfehlungen für hohe Verfügbarkeit](advisor-high-availability-recommendations.md)
* [Advisor-Empfehlungen zur Sicherheit](advisor-security-recommendations.md)
-  [Advisor-Empfehlungen zur Leistung](advisor-performance-recommendations.md)
* [Advisor-Empfehlungen zu Kosten](advisor-performance-recommendations.md)

