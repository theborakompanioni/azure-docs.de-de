---
title: Aktualisieren der Betriebssystemversion in Azure Security Center | Microsoft Docs
description: In diesem Artikel wird gezeigt, wie Sie die Azure Security Center-Empfehlung **Betriebssystemversion aktualisieren** implementieren.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: aa372492-ecdb-4368-8fdd-d8ed31e216ee
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2016
ms.author: terrylan
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: ce0d178914907750e5da59f223a4b1e04b9bb6fb


---
# <a name="update-os-version-in-azure-security-center"></a>Aktualisieren der Betriebssystemversion in Azure Security Center
Für virtuelle Computer in Clouddiensten empfiehlt Azure Security Center die Aktualisierung des Betriebssystems, falls eine neuere Version zur Verfügung steht.  Es werden nur Clouddienst-Webrollen und -Workerrollen überwacht, die in Produktionsslots ausgeführt werden.

> [!NOTE]
> Der Dienst wird anhand einer Beispielbereitstellung vorgestellt.  Es ist keine schrittweise Anleitung.
> 
> 

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlung
1. Wählen Sie auf dem Blatt **Empfehlungen** die Option **Betriebssystemversion aktualisieren** aus.
   ![Betriebssystemversion aktualisieren][1]
2. Daraufhin wird das Blatt **Betriebssystemversion aktualisieren**geöffnet. Führen Sie die Schritte auf diesem Blatt aus, um die Betriebssystemversion zu aktualisieren.

## <a name="see-also"></a>Weitere Informationen
In diesem Artikel wurde gezeigt, wie Sie die Security Center-Empfehlung „Betriebssystemversion aktualisieren“ implementieren. Weitere Informationen zu Clouddiensten und zum Aktualisieren der Betriebssystemversion für einen Clouddienst finden Sie in folgenden Themen:

* [Übersicht über Cloud Services](../cloud-services/cloud-services-choose-me.md)
* [Aktualisieren eines Clouddiensts](../cloud-services/cloud-services-update-azure-service.md)
* [Konfigurieren von Clouddiensten](../cloud-services/cloud-services-how-to-configure-portal.md)

Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](security-center-policies.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
* [Verwalten von Sicherheitsempfehlungen in Azure Security Center:](security-center-recommendations.md) Hier erfahren Sie, wie Empfehlungen Ihnen beim Schutz der Azure-Ressourcen helfen.
* [Überwachen der Sicherheitsintegrität in Azure Security Center:](security-center-monitoring.md) Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center:](security-center-managing-and-responding-alerts.md) Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Überwachen von Partnerlösungen mit Azure Security Center:](security-center-partner-solutions.md) Erfahren Sie, wie der Integritätsstatus Ihrer Partnerlösungen überwacht wird.
* [Azure Security Center – Häufig gestellte Fragen:](security-center-faq.md) Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Neuigkeiten und Informationen zur Azure-Sicherheit.

<!--Image references-->
[1]: ./media/security-center-update-os-version/update-os-version.png



<!--HONumber=Nov16_HO3-->


