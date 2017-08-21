---
title: "Übersicht über Azure Service Health | Microsoft-Dokumentation"
description: "Personalisierte Informationen dazu, wie Ihre Azure-Apps von aktuellen und zukünftigen Problemen und Wartungen der Azure-Dienste betroffen sind."
services: Resource health
documentationcenter: 
author: rboucher
manager: 
editor: 
ms.assetid: 
ms.service: service-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 07/07/2017
ms.author: robb
ms.translationtype: HT
ms.sourcegitcommit: f76de4efe3d4328a37f86f986287092c808ea537
ms.openlocfilehash: 001dc1fa2a0fd7e132101944a87be3f8552d8738
ms.contentlocale: de-de
ms.lasthandoff: 07/10/2017


---
# <a name="azure-service-health"></a>Azure Service Health
Azure Service Health stellt zeitnahe und persönliche Informationen bereit, wenn sich Probleme in Azure-Diensten auf Ihre Dienste auswirken.  Zudem unterstützt Sie der Dienst bei der Vorbereitung auf anstehende geplante Wartungen.

## <a name="service-health-events"></a>Service Health-Ereignisse
Service Health verfolgt drei Typen von Integritätsereignissen nach, die sich auf Ihre Ressourcen auswirken könnten:
1. **Dienstprobleme**: Probleme in den Azure-Diensten, die Sie momentan betreffen. 
2. **Geplante Wartung**: Anstehende Wartung, die sich in der Zukunft auf die Verfügbarkeit Ihrer Dienste auswirken kann.  
3. **Integritätsempfehlungen**: Änderungen in Azure-Diensten, die Ihre Aufmerksamkeit erfordern. Beispielsweise, wenn Azure-Funktionen veraltet sind oder Sie ein Nutzungskontingent überschreiten.

    ![Service Health-Ereignisse](./media/service-health-overview/azure-service-health-overview-7.png)

## <a name="get-started-with-service-health"></a>Erste Schritte mit Service Health
Um Ihr Service Health-Dashboard zu starten, wählen Sie auf dem Dashboard Ihres Portals die Kachel „Service Health“ aus. Wenn Sie die Kachel zuvor entfernt haben oder ein benutzerdefiniertes Dashboard verwenden, suchen Sie den Service Health-Dienst unter „Weitere Dienste“ (unten links auf Ihrem Dashboard).
![Erste Schritte mit Service Health](./media/service-health-overview/azure-service-health-overview-1.png)

## <a name="see-current-issues-which-impact-your-services"></a>Anzeigen aktueller Probleme, die sich auf Ihre Dienste auswirken
Die Ansicht **Dienstprobleme** zeigt laufende Probleme in Azure-Diensten an, die sich auf Ihre Ressourcen auswirken. Sie können nachvollziehen, wann das Problem aufgetreten ist und welche Dienste und Regionen betroffen sind. Sie können auch das aktuelle Update lesen, um zu verstehen, was Azure unternimmt, um das Problem zu lösen. 
![Verwalten von Dienstproblemen](./media/service-health-overview/azure-service-health-overview-2.png)

Wählen Sie die Registerkarte **Potenzielle Auswirkung** aus, um die spezifische Liste der Ressourcen in Ihrem Besitz anzuzeigen, die von dem Problem betroffen sein könnten. Sie können eine CSV-Liste dieser Ressourcen herunterladen, um diese für Ihr Team freizugeben.
![Verwalten von Dienstproblemen – Auswirkung](./media/service-health-overview/azure-service-health-overview-4.png)

## <a name="get-links-and-downloadable-explanations"></a>Abrufen von Links und herunterladbaren Erläuterungen 
Sie können einen Link für das Problem abrufen, den Sie in Ihrem Problemverwaltungssystem verwenden können. Sie können PDF- und manchmal CSV-Dateien herunterladen, die Sie für Personen freigeben können, die keinen Zugriff auf das Azure-Portal haben.   
![Verwalten von Dienstproblemen – Problemverwaltung](./media/service-health-overview/azure-service-health-overview-3.png)

## <a name="get-support-from-microsoft"></a>Hilfe von Microsoft erhalten
Kontaktieren Sie den Support, wenn Ihre Ressource immer noch in schlechtem Zustand ist, obwohl das Problem gelöst wurde.  Verwenden Sie die Supportlinks rechts auf der Seite.  

## <a name="pin-a-personalized-health-map-to-your-dashboard"></a>Anheften einer personalisierten Integritätskarte an Ihr Dashboard
Filtern Sie Service Health, um Ihre geschäftskritischen Abonnements, Regionen und Ressourcentypen anzuzeigen. Speichern Sie den Filter, und heften Sie eine personalisierte Integritätsweltkarte an Ihr Dashboard im Portal an. 
![Filtern der personalisierten Integritätskarte](./media/service-health-overview/azure-service-health-overview-6a.png)
![Anheften einer personalisierten Integritätskarte](./media/service-health-overview/azure-service-health-overview-6b.png)

## <a name="configure-service-health-alerts"></a>Konfigurieren von Service Health-Warnungen
Azure Service Health lässt sich in Azure Monitor integrieren und warnt Sie per E-Mail, SMS und Webhookbenachrichtigungen, wenn Ihre geschäftskritischen Ressourcen beeinträchtigt sind. Richten Sie eine Aktivitätsprotokollwarnung für das entsprechende Service Health-Ereignis ein. Leiten Sie diese Warnung mithilfe von Aktionsgruppen an die entsprechenden Personen in Ihrer Organisation weiter. Weitere Informationen finden Sie unter [Erstellen von Aktivitätsprotokollwarnungen zu Dienstbenachrichtigungen](../monitoring-and-diagnostics/monitoring-activity-log-alerts-on-service-notifications.md).

# <a name="next-steps"></a>Nächste Schritte
Richten Sie Warnungen ein, damit Sie über Integritätsprobleme benachrichtigt werden. Weitere Informationen finden Sie unter [Erstellen von Aktivitätsprotokollwarnungen zu Dienstbenachrichtigungen](../monitoring-and-diagnostics/monitoring-activity-log-alerts-on-service-notifications.md). 
