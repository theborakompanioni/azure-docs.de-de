<properties
   pageTitle="Auflösen von Endpoint Protection-Integritätswarnungen in Azure Security Center | Microsoft Azure"
   description="In diesem Dokument wird erläutert, wie Sie die Azure Security Center-Empfehlung „Endpoint Protection-Integritätswarnungen auflösen“ umsetzen."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# Auflösen von Endpoint Protection-Integritätswarnungen in Azure Security Center

Azure Security Center empfiehlt, erkannte Endpoint Protection-Integritätswarnungen aufzulösen. In Security Center können Sie virtuelle Computer (VMs) mit Endpoint Protection-Fehlern sowie die Anzahl der Fehler ermitteln.

> [AZURE.NOTE] Der Dienst wird anhand einer Beispielbereitstellung vorgestellt. Es ist keine schrittweise Anleitung.

## Implementieren der Empfehlung

1. Wählen Sie auf dem Blatt **Empfehlungen** die Option **Endpoint Protection-Integritätswarnungen auflösen**. ![Endpoint Protection-Integritätswarnungen auflösen][1]

2. Daraufhin wird das Blatt **Endpoint Protection-Fehler** geöffnet, auf dem VMs mit Fehlern und die Anzahl der Fehler für jede VM aufgeführt sind. Wählen Sie in der Liste eine VM aus. ![Endpoint Protection-Fehler][2]

3. Für die ausgewählte VM wird das Blatt **Fehlerliste** mit einer Liste der Fehler geöffnet. Wählen Sie in der Liste einen Fehler aus, um mehr zu erfahren. Daraufhin wird ein Blatt mit Informationen zum ausgewählten Fehler geöffnet. ![Fehlerliste][3] ![Ereignis „Fehler“][4]

## Weitere Informationen

Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

- [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](security-center-policies.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
- [Verwalten von Sicherheitsempfehlungen in Azure Security Center:](security-center-recommendations.md) Hier erfahren Sie, wie Empfehlungen Ihnen beim Schutz der Azure-Ressourcen helfen.
- [Überwachen der Sicherheitsintegrität in Azure Security Center:](security-center-monitoring.md) Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
- [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center:](security-center-managing-and-responding-alerts.md) Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
- [Überwachen von Partnerlösungen mit Azure Security Center:](security-center-partner-solutions.md) Erfahren Sie, wie Sie den Integritätsstatus Ihrer Partnerlösungen überwachen.
- [Azure Security Center – Häufig gestellte Fragen:](security-center-faq.md) Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Neuigkeiten und Informationen zur Azure-Sicherheit.

<!--Image references-->
[1]: ./media/security-center-resolve-endpoint-protection/resolve-endpoint-protection.png
[2]: ./media/security-center-resolve-endpoint-protection/endpoint-protection-failure.png
[3]: ./media/security-center-resolve-endpoint-protection/failure-list.png
[4]: ./media/security-center-resolve-endpoint-protection/failure-event.png

<!---HONumber=AcomDC_0803_2016-->