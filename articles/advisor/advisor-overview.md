---
title: "Einführung in Azure Advisor | Microsoft Docs"
description: Nutzen Sie Azure Advisor, um Ihre Azure-Bereitstellungen zu optimieren.
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
ms.openlocfilehash: 3c4f6d13dd43ae557b177883e14044e637bd803b
ms.lasthandoff: 02/21/2017

---

# <a name="introduction-to-azure-advisor"></a>Einführung in Azure Advisor

Weitere Informationen zu Azure Advisor, seinen wichtigsten Funktionen und häufig gestellte Fragen.

## <a name="what-is-azure-advisor"></a>Was ist Azure Advisor?
Bei Azure Advisor handelt es sich um einen personalisierten Cloudberater, der Sie mit bewährten Methoden zum Optimieren von Azure-Bereitstellungen unterstützt. Das Tool analysiert die Konfiguration Ihrer Ressourcen und Telemetriedaten zur Nutzung und macht Vorschläge, wie Sie die Wirtschaftlichkeit, Leistung, Verfügbarkeit und Sicherheit Ihrer Azure-Ressourcen steigern können.

Azure Advisor ermöglicht Folgendes:
-    Abrufen proaktiver, umsetzbarer und individueller Empfehlungen und bewährter Methoden 
-    Verbessern der Leistung, Sicherheit und Verfügbarkeit Ihrer Ressourcen und Suchen nach Möglichkeiten der Senkung Ihre Gesamtausgaben für Azure
-    Abrufen von Empfehlungen mit Inlineaktionen

Der Zugriff auf Advisor erfolgt im [Azure-Portal](https://aka.ms/azureadvisordashboard). Melden Sie sich beim [Portal](https://portal.azure.com) an, wählen Sie **Durchsuchen** aus, und navigieren Sie zu **Azure Advisor**. Das Advisor-Dashboard zeigt personalisierte Empfehlungen für ein ausgewähltes Abonnement. Die Empfehlungen sind in vier Kategorien unterteilt. 

-   **Hohe Verfügbarkeit**: Advisor hilft Ihnen, die ununterbrochene Verfügbarkeit Ihrer unternehmenskritischen Anwendungen zu gewährleisten und zu verbessern. Weitere Informationen finden Sie unter [Advisor-Empfehlungen für hohe Verfügbarkeit](advisor-high-availability-recommendations.md).

-   **Sicherheit**: Advisor hilft beim Erkennen von Bedrohungen und Sicherheitsrisiken, die zu potenziellen Sicherheitslücken führen können. Weitere Informationen finden Sie unter [Advisor-Empfehlungen zur Sicherheit](advisor-security-recommendations.md).

-   **Leistung**: Advisor hilft, die Geschwindigkeit Ihrer Anwendungen zu verbessern. Weitere Informationen finden Sie unter [Advisor-Empfehlungen zur Leistung](advisor-performance-recommendations.md).

-   **Kosten**: Mit Advisor können Sie Ihre Gesamtausgaben für Azure senken und optimieren. Weitere Informationen finden Sie unter [Advisor-Empfehlungen zu Kosten](advisor-cost-recommendations.md).

  ![Typen von Advisor-Empfehlungen](./media/advisor-overview/advisor-all-tab-examples.png)

> [!NOTE]
> Für den Zugriff auf die Empfehlungen von Ratgeber müssen Sie Ihr Abonnement zunächst bei Ratgeber **registrieren**. Ein Abonnement wird registriert, wenn ein **Abonnementbesitzer** das Ratgeber-Dashboard startet und auf die Schaltfläche **Abrufen von Empfehlungen** klickt. Dies ist eine **einmalige Aufgabe**. Sobald ein Abonnement registriert wurde, können **Besitzer**, **Mitwirkende** oder **Leser** eines Abonnements, einer Ressourcengruppe oder einer bestimmten Ressource auf die Empfehlungen von Ratgeber zugreifen.

Sie können auf eine Empfehlung klicken, um weitere Informationen dazu zu erhalten. Sie lernen außerdem Aktionen kennen, die Sie ausführen können, um eine Chance zu nutzen oder ein Problem zu beheben. Advisor bietet Empfehlungen mit Inlineaktionen oder Links zu Dokumentation. Bei Klicken auf eine Inlineaktion werden Sie zur Umsetzung durch eine Folge von Schritten begleitet. Bei Klicken auf einen Link zur Dokumentation werden Sie zum Artikel geleitet, in dem die manuelle Umsetzung der Empfehlung beschrieben ist. 

Advisor aktualisiert Empfehlungen stündlich. Wenn Sie nicht vorhaben, bei einer Empfehlung direkt eine Maßnahme zu ergreifen, können Sie sie für einen bestimmten Zeitraum zurückstellen oder verwerfen. 

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="how-do-i-access-advisor"></a>Wie greife ich auf Advisor zu?
Der Zugriff auf Advisor erfolgt im Azure-Portal. Melden Sie sich beim Portal an, wählen Sie **Durchsuchen** aus, und navigieren Sie zu **Azure Advisor**. Sie können die Advisor-Empfehlungen auch auf dem Blatt mit den VM-Ressourcen anzeigen. Wählen Sie einen virtuellen Computer aus, und wechseln Sie dann zu den Advisor-Empfehlungen im Menü. 

### <a name="what-permissions-do-i-need-to-access-advisor"></a>Welche Berechtigungen benötige ich für den Zugriff auf Advisor?

Für den Zugriff auf die Empfehlungen von Ratgeber müssen Sie Ihr Abonnement zunächst bei Ratgeber **registrieren**. Ein Abonnement wird registriert, wenn ein Abonnementbesitzer das Ratgeber-Dashboard startet und auf die Schaltfläche **Abrufen von Empfehlungen** klickt. Dies ist eine **einmalige Aufgabe**. Sobald ein Abonnement registriert wurde, können **Besitzer**, **Mitwirkende** oder **Leser** eines Abonnements, einer Ressourcengruppe oder einer bestimmten Ressource auf die Empfehlungen von Ratgeber zugreifen.

### <a name="how-often-are-advisor-recommendations-updated"></a>Wie oft werden Empfehlungen von Advisor aktualisiert?

Advisor-Empfehlungen werden stündlich aktualisiert.

### <a name="what-resources-does-advisor-provide-recommendations-for"></a>Für welche Ressourcen bietet Advisor Empfehlungen?

Ratgeber bietet Empfehlungen für virtuelle Computer, Verfügbarkeitsgruppen, Anwendungsgateways, App Services, SQL Server, SQL-Datenbanken und Redis Cache.

### <a name="can-i-snooze-or-dismiss-a-recommendation"></a>Kann ich eine Empfehlung zurückstellen oder verwerfen?

Um eine Empfehlung zurückzustellen oder zu verwerfen, klicken Sie auf die Schaltfläche oder den Link **Zurückstellen**. Sie können einen Zurückstellungszeitraum angeben oder **Nie** auswählen, um die Empfehlung zu verwerfen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Advisor-Empfehlungen finden Sie in den folgenden Ressourcen:

-  [Erste Schritte mit Advisor](advisor-get-started.md)
-  [Advisor-Empfehlungen für hohe Verfügbarkeit](advisor-high-availability-recommendations.md)
-  [Advisor-Empfehlungen zur Sicherheit](advisor-security-recommendations.md)
-  [Advisor-Empfehlungen zur Leistung](advisor-performance-recommendations.md)
-  [Advisor-Empfehlungen zu Kosten](advisor-cost-recommendations.md)

