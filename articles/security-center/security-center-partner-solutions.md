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
ms.date: 10/26/2016
ms.author: terrylan
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: a3d7254fded543fa12b6e82cd50ece8260858955


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

Die Kachel **Partnerlösungen** zeigt die Anzahl der Partnerlösungen und eine Statusübersicht für diese Lösungen an.

Eine Partnerlösung kann den folgenden **STATUS** haben:

* Geschützt (grün): Es liegt kein Integritätsproblem vor.
* Fehlerhaft (rot): Es liegt ein Integritätsproblem vor, das sofort untersucht werden muss.
* Melden beendet (orange): Die Lösung hat das Melden der Integrität beendet.
* Unbekannter Schutzstatus (orange): Der Integritätsstatus der Lösung ist derzeit unbekannt, da beim Hinzufügen einer neuen Ressource zur vorhandenen Lösung ein Fehler aufgetreten ist.
* Nicht gemeldet (grau): Die Lösung hat noch keine Meldung berichtet. Der Status einer Lösung kann unter Umständen noch nicht gemeldet worden sein, wenn die Verbindung gerade hergestellt wurde und die Bereitstellung noch nicht abgeschlossen ist.

Wenn in Ihr Abonnement keine Lösungen integriert sind, zeigt die Kachel an, dass es keine Lösungen gibt. Wenn Sie die Kachel **Partnerlösungen** auswählen, können Sie das Blatt **Empfehlungen** zum Bereitstellen von Partnersicherheitslösungen öffnen.
![Keine Partnerlösungen][2]

So zeigen der Integrität Ihrer Partnerlösungen an:

1. Wählen Sie die Kachel **Partnerlösungen** . Ein Blatt mit einer Liste der Partnerlösungen, die mit dem Security Center verbunden sind, wird geöffnet.
   ![Partnerlösungen][3]
2. Wählen Sie eine Partnerlösung aus. In diesem Beispiel wählen wir die Lösung **F5-WAF2** aus.  Auf einem Blatt werden der Status der Partnerlösung und die zugeordneten Ressourcen der Lösung angezeigt. Wählen Sie die Option **Lösungskonsole** , um die Verwaltungsoberfläche des Partners für diese Lösung zu öffnen.
   ![Details der Partnerlösung][4]
3. Wechseln Sie zurück zum Blatt **F5-WAF2**, und wählen Sie **App verknüpfen** aus. Das Blatt **Anwendungen verknüpfen** wird geöffnet. Hier können Sie Ressourcen mit der Partnerlösung verknüpfen.
   ![Verknüpfen Sie Ressourcen mit Partnerlösungen][5]

## <a name="see-also"></a>Siehe auch
In diesem Dokument wurden Ihnen die **Partnerlösungen** in Security Center vorgestellt. Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](security-center-policies.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
* [Verwalten von Sicherheitsempfehlungen in Azure Security Center:](security-center-recommendations.md) Hier erfahren Sie, wie Empfehlungen Ihnen beim Schutz der Azure-Ressourcen helfen.
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md) : Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md) : Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md) : Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Neuigkeiten und Informationen zur Azure-Sicherheit.

<!--Image references-->
[1]: ./media/security-center-partner-solutions/partner-solutions-tile.png
[2]: ./media/security-center-partner-solutions/no-partner-solutions-to-display.png
[3]: ./media/security-center-partner-solutions/partner-solutions.png
[4]: ./media/security-center-partner-solutions/partner-solutions-detail.png
[5]: ./media/security-center-partner-solutions/link-applications.png



<!--HONumber=Dec16_HO2-->


