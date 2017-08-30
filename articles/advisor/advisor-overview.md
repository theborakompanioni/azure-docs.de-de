---
title: "Einführung in Azure Advisor | Microsoft Docs"
description: Nutzen Sie Azure Advisor, um Ihre Azure-Bereitstellungen zu optimieren.
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
ms.openlocfilehash: 35678142550f9f887562f311a5e7d9516495cf53
ms.contentlocale: de-de
ms.lasthandoff: 03/31/2017

---

# <a name="introduction-to-azure-advisor"></a>Einführung in Azure Advisor

Hier erfahren Sie mehr über den Azure-Ratgeber und seine wichtigsten Funktionen, und Sie erhalten Sie Antworten auf häufig gestellte Fragen.

## <a name="what-is-advisor"></a>Was ist der Ratgeber?
Beim Ratgeber handelt es sich um einen personalisierten Cloudberater, der Sie mit bewährten Methoden zum Optimieren von Azure-Bereitstellungen unterstützt. Das Tool analysiert die Konfiguration Ihrer Ressourcen und Telemetriedaten zur Nutzung und macht anschließend Vorschläge, wie Sie die Wirtschaftlichkeit, Leistung, Verfügbarkeit und Sicherheit Ihrer Azure-Ressourcen steigern können.

Der Ratgeber ermöglicht Folgendes:
* Abrufen proaktiver, umsetzbarer und individueller Empfehlungen und bewährter Methoden. 
* Verbessern der Leistung, Sicherheit und Verfügbarkeit Ihrer Ressourcen und Ermitteln von Möglichkeiten der Senkung Ihrer Gesamtausgaben für Azure.
* Abrufen von Empfehlungen mit vorgeschlagenen Inlineaktionen.

Der Zugriff auf Advisor erfolgt im [Azure-Portal](https://aka.ms/azureadvisordashboard). Melden Sie sich beim [Portal](https://portal.azure.com) an, wählen Sie **Durchsuchen** aus, und navigieren Sie dann zu **Azure-Ratgeber**. Das Advisor-Dashboard zeigt personalisierte Empfehlungen für ein ausgewähltes Abonnement. 

Die Empfehlungen sind in vier Kategorien unterteilt: 

* **Hohe Verfügbarkeit**: Der Ratgeber hilft Ihnen, die ununterbrochene Verfügbarkeit Ihrer unternehmenskritischen Anwendungen zu gewährleisten und zu verbessern. Weitere Informationen finden Sie unter [Advisor-Empfehlungen für hohe Verfügbarkeit](advisor-high-availability-recommendations.md).

* **Sicherheit**: Der Ratgeber hilft beim Erkennen von Bedrohungen und Sicherheitsrisiken, die zu Sicherheitslücken führen können. Weitere Informationen finden Sie unter [Advisor-Empfehlungen zur Sicherheit](advisor-security-recommendations.md).

* **Leistung**: Der Ratgeber hilft, die Geschwindigkeit Ihrer Anwendungen zu verbessern. Weitere Informationen finden Sie unter [Advisor-Empfehlungen zur Leistung](advisor-performance-recommendations.md).

* **Kosten**: Mit dem Ratgeber können Sie Ihre Gesamtausgaben für Azure senken und optimieren. Weitere Informationen finden Sie unter [Advisor-Empfehlungen zu Kosten](advisor-cost-recommendations.md).

  ![Typen von Advisor-Empfehlungen](./media/advisor-overview/advisor-all-tab-examples.png)

> [!NOTE]
> Für den Zugriff auf die Empfehlungen des Ratgebers müssen Sie Ihr Abonnement zunächst beim Ratgeber *registrieren*. Ein Abonnement wird registriert, wenn ein *Abonnementbesitzer* das Ratgeber-Dashboard aufruft und auf die Schaltfläche **Empfehlungen abrufen** klickt. Dies ist eine *einmalige Aufgabe*. Sobald ein Abonnement registriert wurde, können *Besitzer*, *Mitwirkende* oder *Leser* eines Abonnements, einer Ressourcengruppe oder einer bestimmten Ressource auf die Ratgeberempfehlungen zugreifen.

Sie können auf eine Empfehlung klicken, um weitere Informationen dazu zu erhalten. Sie lernen außerdem Aktionen kennen, die Sie ausführen können, um eine Chance zu nutzen oder ein Problem zu beheben. 

Advisor bietet Empfehlungen mit Inlineaktionen oder Links zu Dokumentation. Bei Klicken auf eine Inlineaktion werden Sie zur Umsetzung durch eine Folge von Schritten begleitet. Bei Klicken auf einen Link zur Dokumentation werden Sie zum Artikel geleitet, in dem die manuelle Umsetzung der Empfehlung beschrieben ist. 

Der Ratgeber aktualisiert Empfehlungen stündlich. Wenn Sie nicht vorhaben, bei einer Empfehlung direkt eine Maßnahme zu ergreifen, können Sie sie für einen bestimmten Zeitraum zurückstellen oder verwerfen. 

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="how-do-i-access-advisor"></a>Wie greife ich auf Advisor zu?
Der Zugriff auf Advisor erfolgt im [Azure-Portal](https://aka.ms/azureadvisordashboard). Melden Sie sich beim [Portal](https://portal.azure.com) an, wählen Sie **Durchsuchen** aus, und navigieren Sie dann zu **Azure-Ratgeber**. Das Advisor-Dashboard zeigt personalisierte Empfehlungen für ein ausgewähltes Abonnement. 

Sie können die Advisor-Empfehlungen auch auf dem Blatt mit den VM-Ressourcen anzeigen. Wählen Sie einen virtuellen Computer aus, und wechseln Sie dann zu den Advisor-Empfehlungen im Menü. 

### <a name="what-permissions-do-i-need-to-access-advisor"></a>Welche Berechtigungen benötige ich für den Zugriff auf Advisor?

Für den Zugriff auf die Empfehlungen des Ratgebers müssen Sie Ihr Abonnement zunächst beim Ratgeber *registrieren*. Ein Abonnement wird registriert, wenn ein *Abonnementbesitzer* das Ratgeber-Dashboard aufruft und auf die Schaltfläche **Empfehlungen abrufen** klickt. Dies ist eine *einmalige Aufgabe*. Sobald ein Abonnement registriert wurde, können *Besitzer*, *Mitwirkende* oder *Leser* eines Abonnements, einer Ressourcengruppe oder einer bestimmten Ressource auf die Ratgeberempfehlungen zugreifen.

### <a name="how-often-are-advisor-recommendations-updated"></a>Wie oft werden Empfehlungen von Advisor aktualisiert?

Empfehlungen des Ratgebers werden stündlich aktualisiert.

### <a name="what-resources-does-advisor-provide-recommendations-for"></a>Für welche Ressourcen bietet Advisor Empfehlungen?

Der Ratgeber bietet Empfehlungen für virtuelle Computer, Verfügbarkeitsgruppen, Anwendungsgateways, App Services, SQL-Server, SQL-Datenbanken und Redis Cache.

### <a name="can-i-snooze-or-dismiss-a-recommendation"></a>Kann ich eine Empfehlung zurückstellen oder verwerfen?

Um eine Empfehlung zurückzustellen oder zu verwerfen, klicken Sie auf die Schaltfläche oder den Link **Zurückstellen**. Sie können einen Zurückstellungszeitraum angeben oder **Nie** auswählen, um die Empfehlung zu verwerfen.

## <a name="next-steps"></a>Nächste Schritte

Hier finden Sie weitere Informationen zu Empfehlungen des Ratgebers:

* [Erste Schritte mit Advisor](advisor-get-started.md)
* [Advisor-Empfehlungen für hohe Verfügbarkeit](advisor-high-availability-recommendations.md)
* [Advisor-Empfehlungen zur Sicherheit](advisor-security-recommendations.md)
* [Advisor-Empfehlungen zur Leistung](advisor-performance-recommendations.md)
* [Advisor-Empfehlungen zu Kosten](advisor-cost-recommendations.md)

