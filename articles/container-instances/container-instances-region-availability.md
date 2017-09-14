---
title: "Regionale und Ressourcenverfügbarkeit für Azure Container Instances | Azure-Dokumentation"
description: "Erfahren Sie, in welchen Azure-Regionen die Bereitstellung von Containerinstanzen unterstützt wird und welche CPU- und Arbeitsspeicherlimits für diese Instanzen gelten."
services: container-instances
documentationcenter: 
author: mmacy
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/31/2017
ms.author: marsma
ms.custom: 
ms.translationtype: HT
ms.sourcegitcommit: 3eb68cba15e89c455d7d33be1ec0bf596df5f3b7
ms.openlocfilehash: 2b9b1b864bbfd73383759212dd7d91f8e4941544
ms.contentlocale: de-de
ms.lasthandoff: 09/01/2017

---

# <a name="region-availability-for-azure-container-instances"></a>Regionale Verfügbarkeit für Azure Container Instances

Während der Vorschauphase ist Azure Container Instances mit den angegebenen CPU- und Arbeitsspeicherlimits in den folgenden Regionen verfügbar.

| Standort | Betriebssystem | CPU | Arbeitsspeicher (GB) |
| -------- | -- | :---: | :-----------: |
| Europa, Westen; USA, Westen; USA, Osten | Linux | 2 | 7 |
| Europa, Westen; USA, Westen; USA, Osten | Windows | 2 | 3,5 |

## <a name="resource-availability"></a>Ressourcenverfügbarkeit

Containerinstanzen, die innerhalb dieser Ressourcenlimits erstellt werden, unterliegen der Verfügbarkeit in der Bereitstellungsregion. Wenn eine Region stark ausgelastet ist, kann bei der Bereitstellung von Instanzen ein Fehler auftreten.

Um einen solchen Fehler bei der Bereitstellung zu beheben, versuchen Sie, Instanzen mit niedrigeren CPU- und Arbeitsspeichereinstellungen bereitzustellen, oder führen Sie die Bereitstellung zu einem späteren Zeitpunkt durch.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Problembehandlung bei der Bereitstellung von Containerinstanzen finden Sie unter [Beheben von Bereitstellungsproblemen für Azure Container Instances](container-instances-troubleshooting.md).
