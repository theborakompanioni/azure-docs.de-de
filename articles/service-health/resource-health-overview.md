---
title: "Übersicht über Azure Resource Health | Microsoft Docs"
description: "Übersicht über Azure Resource Health"
services: Resource health
documentationcenter: 
author: BernardoAMunoz
manager: 
editor: 
ms.assetid: 85cc88a4-80fd-4b9b-a30a-34ff3782855f
ms.service: service-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 07/01/2017
ms.author: BernardoAMunoz
ms.translationtype: HT
ms.sourcegitcommit: f76de4efe3d4328a37f86f986287092c808ea537
ms.openlocfilehash: 040d58a81a9b41fe660e4276d698bf884f90bb6c
ms.contentlocale: de-de
ms.lasthandoff: 07/10/2017


---
# <a name="azure-resource-health-overview"></a>Übersicht über Azure Resource Health
 
Resource Health hilft Ihnen bei der Diagnose und beim Anfordern von Support, wenn ein Azure-Problem Ihre Ressourcen beeinträchtigt. Der Dienst informiert Sie über die aktuelle und frühere Integrität Ihrer Ressourcen und unterstützt Sie beim Beheben von Problemen. Resource Health bietet technischen Support, wenn Sie Unterstützung bei Azure-Dienstproblemen benötigen.

Während [Azure-Status](https://status.azure.com) Sie über Dienstprobleme informiert, die einen großen Azure-Kundenkreis betreffen, bietet Ihnen Resource Health ein personalisiertes Dashboard der Integrität Ihrer Ressourcen. Resource Health teilt Ihnen mit, wie oft Ihre Ressourcen in der Vergangenheit aufgrund von Azure-Dienstproblemen nicht verfügbar waren. So können Sie leicht erkennen, ob eine SLA verletzt wurde. 

## <a name="what-is-considered-a-resource-and-how-does-resource-health-decides-if-a-resource-is-healthy-or-not"></a>Was wird als Ressource angesehen, und wie entscheidet Resource Health, ob die Ressource fehlerfrei ist?
Eine Ressource ist eine Instanz eines Ressourcentyps, der von einem Azure-Dienst über den Azure Resource Manager bereitgestellt wird, z.B. ein virtueller Computer, eine Web-App oder eine SQL-Datenbank.

Resource Health basiert auf Signalen, die von den verschiedenen Azure-Diensten ausgesendet werden, damit ermittelt werden kann, ob eine Ressource fehlerfrei ist. Wenn eine Ressource fehlerhaft ist, analysiert Resource Health zusätzliche Informationen, um die Quelle des Problems zu bestimmen. Resource Health identifiziert auch Aktionen, die Microsoft zum Beheben des Problems durchführt, oder welche Aktionen Sie durchführen können, um die Ursache des Problems zu beseitigen. 

Entnehmen Sie weitere Informationen zur Bewertung der Integrität der vollständigen Liste der Ressourcentypen und Integritätsprüfungen in [Ressourcentypen und Integritätsprüfungen in Azure Resource Health](resource-health-checks-resource-types.md).

## <a name="health-status-provided-by-resource-health"></a>Der von Resource Health gemeldete Integritätsstatus
Die Integrität einer Ressource kann einen der folgenden Status annehmen:

### <a name="available"></a>Verfügbar
Der Dienst hat keine Ereignisse erkannt, die Auswirkungen auf die Integrität der Ressource haben. In Fällen, in denen die Ressource während der letzten 24 Stunden nach einer ungeplanten Ausfallzeit wiederhergestellt worden ist, sehen Sie die **vor kurzem wiederhergestellt**-Benachrichtigung.

![Resource Health, Verfügbar, virtueller Computer](./media/resource-health-overview/Available.png)

### <a name="unavailable"></a>Nicht verfügbar
Der Dienst hat ein laufendes Plattform- oder Nicht-Plattformereignis erkannt, das Auswirkungen auf die Integrität der Ressource hat.

#### <a name="platform-events"></a>Plattformereignisse
Diese Ereignisse werden durch mehrere Komponenten der Azure-Infrastruktur ausgelöst und umfassen sowohl geplante Aktionen wie z.B. geplante Wartung als auch unerwartete Incidents wie einen ungeplanten Hostneustart.

Resource Health berichtet zusätzliche Details zu dem Ereignis sowie dem Wiederherstellungsprozess und ermöglicht Ihnen, sich auch dann an den Support zu wenden, wenn Sie nicht über eine aktive Supportvereinbarung mit Microsoft verfügen.

![Resource Health, aufgrund eines Plattformereignisses nicht verfügbarer virtueller Computer](./media/resource-health-overview/Unavailable.png)

#### <a name="non-platform-events"></a>Nicht-Plattformereignisse
Diese Ereignisse werden durch Benutzeraktionen ausgelöst, z.B. das Anhalten eines virtuellen Computers oder das Erreichen der maximalen Anzahl von Verbindungen mit einem Redis Cache.

![Resource Health, aufgrund eines Nicht-Plattformereignisses nicht verfügbarer virtueller Computer](./media/resource-health-overview/Unavailable_NonPlatform.png)

### <a name="unknown"></a>Unknown
Dieser Integritätsstatus gibt an, dass Resource Health seit mehr als 10 Minuten keine Informationen mehr zu dieser Ressource empfangen hat. Dieser Status ist zwar keine definitive Angabe des Ressourcenzustands, aber ein wichtiger Datenpunkt im Problembehandlungsprozess:
* Wenn die Ressource wie erwartet ausgeführt wird, wird der Status der Ressource nach wenigen Minuten zu „Verfügbar“ aktualisiert.
* Wenn Probleme bei der Ressource auftreten, könnte der Integritätsstatus „Unbekannt“ darauf hindeuten, dass die Ressource durch ein Plattformereignis beeinträchtigt wird.

![Resource Health, Unbekannt, virtueller Computer](./media/resource-health-overview/Unknown.png)

## <a name="report-an-incorrect-status"></a>Melden eines falschen Status
Wenn Sie zu einem beliebigen Zeitpunkt glauben, der aktuelle Integritätsstatus sei falsch, können Sie uns dies durch Klicken auf **Falschen Integritätsstatus melden** mitteilen. In Fällen, in denen Sie von einem Azure-Problem betroffen sind, empfehlen wir Ihnen, sich auf dem Resource Health-Blatt an den Support zu wenden. 

![Resource Health, Falschen Integritätsstatus melden](./media/resource-health-overview/incorrect-status.png)

## <a name="historical-information"></a>Verlaufsinformationen
Sie können auf bis zu 14 Tage alte Integritätsverlaufsdaten zugreifen, indem Sie auf dem Resource Health-Blatt auf **Verlauf anzeigen** klicken. 

![Resource Health, Berichtsverlauf](./media/resource-health-overview/history-blade.png)

## <a name="getting-started"></a>Erste Schritte
Öffnen von Resource Health für eine Ressource
1.  Melden Sie sich beim Azure-Portal an.
2.  Wechseln Sie zu Ihrer Ressource.
3.  Klicken Sie auf der linken Seite im Ressourcenmenü auf **Resource Health**.

![Öffnen von Resource Health vom Ressourcenblatt aus](./media/resource-health-overview/from-resource-blade.png)

Sie können auch auf Resource Health zugreifen, indem Sie auf **Weitere Dienste** klicken und **Resource Health** im Filtertextfeld eingeben, um das Blatt **Hilfe und Support** zu öffnen. Klicken Sie abschließend auf [**Resource Health**](https://ms.portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/resourceHealth).

![Öffnen von Resource Health von Weitere Dienste aus](./media/resource-health-overview/FromOtherServices.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Resource Health finden Sie in den folgenden Dokumenten:
-  [Ressourcentypen und Integritätsprüfungen in Azure Resource Health](resource-health-checks-resource-types.md)
-  [Azure Resource Health – FAQ](resource-health-faq.md)





