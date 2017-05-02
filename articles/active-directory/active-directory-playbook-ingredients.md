---
title: Elemente eines Azure Active Directory-PoC-Playbooks | Microsoft-Dokumentation
description: "Untersuchen und schnelles Implementieren von Szenarien mit Identitäts- und Zugriffsverwaltung"
services: active-directory
keywords: Azure Active Directory, Playbook, Proof of Concept, PoC
documentationcenter: 
author: dstefanMSFT
manager: asuthar
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/12/2017
ms.author: dstefan
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 2a555326e5bb2eb7b89e80be721b20488c4f7a2d
ms.lasthandoff: 04/12/2017


---
# <a name="azure-active-directory-proof-of-concept-playbook-ingredients"></a>Azure Active Directory-PoC-Playbook: Elemente 

## <a name="theme"></a>Design
Azure AD stellt Identitäts- und Zugriffslösungen in mehreren Bereichen im Unternehmen bereit. Wir unterteilen die Szenarien in die folgenden Bereiche: 

* [Viele Apps, eine Identität](active-directory-playbook-implementation.md#theme---lots-of-apps-one-identity) 
* [Erhöhen der Sicherheit](active-directory-playbook-implementation.md#theme---increase-your-security) 
* [Self-Service-Skalierung](active-directory-playbook-implementation.md#theme---scale-with-self-service) 

Das Definieren eines PoC-Designs ermöglicht die Konzentration auf Aufgaben, die mit den Geschäftszielen in Einklang sind. Häufig sind dies bei einem Proof of Concept von vornherein die interessantesten Punkte. 

## <a name="environment"></a>Environment

Es ist wichtig, die Details der Umgebung zu bestimmen, in der der PoC durchgeführt wird. Im Idealfall können Sie nach Abschluss des PoC darauf aufbauen. Die Zielumgebung ist entscheidend, und Sie sollten die richtige Balance zwischen einer möglichst originalgetreuen Implementierung und dem Aufwand für Einschränkungen oder zusätzliche Faktoren finden. Typische Umgebungen für einen PoC sind:
* **Produktion:** Die Szenarien werden in der Produktionsumgebung und bereits bereitgestellten Microsoft Cloud-Diensten (Produktions-AD, Office 365, Azure AD-Mandanten-/SSO-Lösung) implementiert. 
* **Benutzerakzeptanztest/Entwicklungsumgebung:** Sie verfügen über eine Testinfrastruktur (paralleles AD und potenzielle Azure AD-Mandanten-/SSO-Lösung) mit Testdaten, die der Produktion ähneln. In der Regel wird die Testumgebung für verschiedene Projekte im Unternehmen gemeinsam genutzt.

Die meisten Szenarien in dieser Anleitung sind aufeinander aufbauend. Daher können sie im Produktionsmandanten bereitgestellt werden, ohne Auswirkungen auf Benutzer außerhalb des PoC zu haben. Wir werden in diesem Dokument darauf hinweisen, welche Szenarien mandantenweite Auswirkungen hätten. In diesen Fällen empfiehlt es sich, nicht die Produktionsumgebung zu verwenden. 


## <a name="target-users"></a>Zielbenutzer

Es ist wichtig, die Zielbenutzer zu ermitteln, die diese Szenarien durchführen, insbesondere wenn die Produktions- oder Testumgebung verwendet wird. Folgende Kategorien von Zielbenutzern für einen PoC gibt es:
* **Pilotbenutzer:** echte Benutzer in der Umgebung, die die Lösung mit dem Konto für ihre täglichen Arbeitsaufgaben verwenden
* **Testbenutzer:** Testkonten, die in der Umgebung erstellt werden 

Die meisten Szenarien in dieser Anleitung können durch Pilotbenutzer ausgeführt werden. In diesem Dokument werden wir ggf. auf Überlegungen zu den Zielbenutzern hinweisen.


[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]
