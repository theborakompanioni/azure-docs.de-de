---
title: "Auflösen von Endpoint Protection-Integritätswarnungen in Azure Security Center | Microsoft Docs"
description: "In diesem Dokument wird erläutert, wie Sie die Azure Security Center-Empfehlung „Endpoint Protection-Integritätswarnungen auflösen“ umsetzen."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 4050f453-98fc-4314-8438-d476469757fb
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2016
ms.author: terrylan
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 5e6b136d6bd3b11fb82126d104fd0cb149255118


---
# <a name="resolve-endpoint-protection-health-alerts-in-azure-security-center"></a>Auflösen von Endpoint Protection-Integritätswarnungen in Azure Security Center
Azure Security Center empfiehlt, erkannte Endpoint Protection-Integritätswarnungen aufzulösen.  In Security Center können Sie virtuelle Computer (VMs) mit Endpoint Protection-Fehlern sowie die Anzahl der Fehler ermitteln.

> [!NOTE]
> Der Dienst wird anhand einer Beispielbereitstellung vorgestellt. Es ist keine schrittweise Anleitung.
> 
> 

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlung
1. Wählen Sie auf dem Blatt **Empfehlungen** die Option **Endpoint Protection-Integritätswarnungen auflösen** aus.
   ![Auflösen von Endpoint Protection-Integritätswarnungen][1]
2. Daraufhin wird das Blatt **Endpoint Protection-Fehler** geöffnet, auf dem VMs mit Fehlern und die Anzahl der Fehler für jede VM aufgeführt sind. Wählen Sie in der Liste eine VM aus.
   ![Endpoint protection failure][2]
3. Für die ausgewählte VM wird das Blatt **Fehlerliste** mit einer Liste der Fehler geöffnet. Wählen Sie in der Liste einen Fehler aus, um mehr zu erfahren. Daraufhin wird ein Blatt mit Informationen zum ausgewählten Fehler geöffnet.
   ![Fehlerliste][3]
    ![Ereignis „Fehler“][4]

## <a name="see-also"></a>Siehe auch
Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](security-center-policies.md)Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
* [Verwalten von Sicherheitsempfehlungen in Azure Security Center:](security-center-recommendations.md)Hier erfahren Sie, wie Empfehlungen Ihnen beim Schutz der Azure-Ressourcen helfen.
* [Überwachen der Sicherheitsintegrität in Azure Security Center:](security-center-monitoring.md)Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center:](security-center-managing-and-responding-alerts.md)Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Überwachen von Partnerlösungen mit Azure Security Center:](security-center-partner-solutions.md) Erfahren Sie, wie Sie den Integritätsstatus Ihrer Partnerlösungen überwachen.
* [Azure Security Center – Häufig gestellte Fragen:](security-center-faq.md)Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/)(Blog zur Azure-Sicherheit): Hier finden Sie Neuigkeiten und Informationen zur Azure-Sicherheit.

<!--Image references-->
[1]: ./media/security-center-resolve-endpoint-protection/resolve-endpoint-protection.png
[2]: ./media/security-center-resolve-endpoint-protection/endpoint-protection-failure.png
[3]: ./media/security-center-resolve-endpoint-protection/failure-list.png
[4]: ./media/security-center-resolve-endpoint-protection/failure-event.png



<!--HONumber=Nov16_HO3-->


