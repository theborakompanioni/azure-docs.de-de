---
title: "Hohe Verfügbarkeit von Azure Analysis Services | Microsoft-Dokumentation"
description: "Sicherstellung einer hohen Verfügbarkeit für Azure Analysis Services."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/18/2017
ms.author: owend
translationtype: Human Translation
ms.sourcegitcommit: 8c4e33a63f39d22c336efd9d77def098bd4fa0df
ms.openlocfilehash: c4eb1162edc42baafe96e6c33699805ffc121204
ms.lasthandoff: 04/20/2017


---

# <a name="analysis-services-high-availability"></a>Hohe Verfügbarkeit für Analysis Services
In diesem Artikel wird die Sicherstellung einer hohen Verfügbarkeit für Azure Analysis Services-Server beschrieben. 


## <a name="assuring-high-availability-during-a-service-disruption"></a>Sicherstellen einer hohen Verfügbarkeit während einer Dienstunterbrechung
Es kommt zwar selten vor, aber es ist möglich, dass ein Azure-Rechenzentrum ausfällt. Ein solcher Ausfall kann den Geschäftsbetrieb einige wenige Minuten oder mehrere Stunden lahmlegen. Hohe Verfügbarkeit wird am häufigsten mittels Serverredundanz erreicht. Mit Azure Analysis Services können Sie Redundanz erreichen, indem Sie in einer oder mehreren Regionen zusätzliche sekundäre Server erstellen. Beim Erstellen redundanter Server zum Sicherstellen, dass die Daten und Metadaten auf diesen Servern mit einem Server in einer Region synchron sind, der offline geschaltet wurde, haben Sie folgende Möglichkeiten:

* Bereitstellen von Modellen auf redundanten Servern in anderen Regionen. Diese Methode erfordert die parallele Verarbeitung von Daten auf sowohl Ihrem primären Server als auch auf redundanten Servern, was gewährleistet, dass alle Server synchron sind.

* Sichern von Datenbanken auf dem primären Server und deren Wiederherstellung auf redundanten Servern. Sie können beispielsweise über Nacht erfolgende Sicherungen in Azure-Speicher automatisieren und auf anderen redundanten Servern in anderen Regionen wiederherstellen. 

In beiden Fällen müssen Sie, wenn auf dem primären Server ein Ausfall auftritt, die Verbindungszeichenfolgen auf betroffenen Clients so ändern, dass eine Verbindung mit dem Server in einem Rechenzentrum in einer anderen Region hergestellt wird. Diese Änderung sollte als letztes Mittel angesehen werden und nur bei einem katastrophalen Ausfall eines regionalen Rechenzentrums erfolgen. Es ist wahrscheinlicher, dass ein ausgefallenes Rechenzentrum, das Ihren primären Server hostet, wieder online geschaltet wird, bevor Sie die Verbindungen auf allen Clients aktualisiert haben. 

Erwägen Sie beim Bestimmen der Vorgehensweise bei einer Dienstunterbrechung in Ihrer Organisation, wie Sie dafür sorgen, dass Ihre Daten aktuell und sicher bleiben. 


## <a name="related-information"></a>Verwandte Informationen
[Sichern und Wiederherstellen](analysis-services-backup.md) 
[Verwalten von Azure Analysis Services](analysis-services-manage.md) 


