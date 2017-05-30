---
title: "Verwalten von Partnerlösungen in Azure Security Center | Microsoft Docs"
description: "Dieses Dokument beschreibt, wie Sie mithilfe von Azure Security Center auf einen Blick den Integritätsstatus Ihrer Partnerlösungen prüfen können, die in Ihr Azure-Abonnement integriert sind."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2017
ms.author: terrylan
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 5ffc3253cf07018ad6fe9f0112c75c2e1495e5df
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="monitoring-partner-solutions-with-azure-security-center"></a>Überwachen von Partnerlösungen mit Azure Security Center
In diesem Dokument erfahren Sie, wie der Integritätsstatus Ihrer Partnerlösungen in Azure Security Center überwacht wird.

> [!NOTE]
> Der Dienst wird anhand einer Beispielbereitstellung vorgestellt. Es ist keine schrittweise Anleitung.
>
>

## <a name="monitoring-partner-solutions"></a>Überwachen von Partnerlösungen
Auf der Kachel **Partnerlösungen** auf dem Blatt **Security Center** können Sie auf einen Blick den Integritätsstatus Ihrer Partnerlösungen prüfen, die in Ihr Azure-Abonnement integriert sind.

![Kachel „Partnerlösungen“][1]

Die Kachel **Partnerlösungen** zeigt die Anzahl der in Ihr Abonnement integrierten Partnerlösungen an. Sind keine Lösungen integriert, zeigt die Kachel die Zahl 0 (null) an.

So zeigen der Integrität Ihrer Partnerlösungen an:

1. Wählen Sie die Kachel **Partnerlösungen** . Das Blatt **Partnerlösungen** mit einer Liste der Partnerlösungen, die mit dem Security Center verbunden sind, wird geöffnet.

   ![Partnerlösungen][3]

   Eine Partnerlösung kann den folgenden Status aufweisen:

   * Geschützt (grün): Es liegt kein Integritätsproblem vor.
   * Fehlerhaft (rot): Es liegt ein Integritätsproblem vor, das sofort untersucht werden muss.
   * Melden beendet (orange): Die Lösung hat das Melden der Integrität beendet.
   * Unbekannter Schutzstatus (orange): Der Integritätsstatus der Lösung ist derzeit unbekannt, da beim Hinzufügen einer neuen Ressource zur vorhandenen Lösung ein Fehler aufgetreten ist.
   * Nicht gemeldet (grau): Die Lösung hat noch keine Meldung berichtet. Der Status einer Lösung kann unter Umständen noch nicht gemeldet worden sein, wenn die Verbindung gerade hergestellt wurde und die Bereitstellung noch nicht abgeschlossen ist.

2. Wählen Sie eine Partnerlösung aus. In diesem Beispiel wählen wir die Lösung **Qualys** aus.  Auf einem Blatt werden der Status der Partnerlösung und die zugeordneten Ressourcen der Lösung angezeigt. Wählen Sie die Option **Lösungskonsole** , um die Verwaltungsoberfläche des Partners für diese Lösung zu öffnen.

   ![Details der Partnerlösung][4]
3. Wechseln Sie zurück zum Blatt **Qualys**, und wählen Sie **Virtuellen Computer verknüpfen** aus. Das Blatt **Anwendungen verknüpfen** wird geöffnet. Hier können Sie Ressourcen mit der Partnerlösung verknüpfen.

   ![Verknüpfen Sie Ressourcen mit Partnerlösungen][5]

## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument wurden Ihnen die **Partnerlösungen** in Security Center vorgestellt. Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](security-center-policies.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
* [Verwalten von Sicherheitsempfehlungen in Azure Security Center:](security-center-recommendations.md) Hier erfahren Sie, wie Empfehlungen Ihnen beim Schutz der Azure-Ressourcen helfen.
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md) : Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md) : Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md) : Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Neuigkeiten und Informationen zur Azure-Sicherheit.

<!--Image references-->
[1]: ./media/security-center-partner-solutions/partner-solutions-tile.png
[3]: ./media/security-center-partner-solutions/partner-solutions.png
[4]: ./media/security-center-partner-solutions/partner-solutions-detail.png
[5]: ./media/security-center-partner-solutions/link-applications.png

