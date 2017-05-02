---
title: "Azure Active Directory-PoC-Playbook – Einführung | Microsoft-Dokumentation"
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
ms.openlocfilehash: bca54013e765315d2bbf2691503872f5e9ca859a
ms.lasthandoff: 04/12/2017


---
# <a name="azure-active-directory-proof-of-concept-playbook-introduction"></a>Azure Active Directory-PoC-Playbook: Einführung

Dieser Artikel enthält Anleitungen zum Erkunden unterschiedlicher Azure AD-Funktionen in einem Proof of Concept (PoC). Die Zielgruppe für dieses Dokument sind Identitätsarchitekten, IT-Experten und Systemintegratoren.

## <a name="how-to-use-this-playbook"></a>Verwenden dieses Playbooks

1. Wählen Sie im Abschnitt [Design](active-directory-playbook-ingredients.md#theme) die für Sie interessanten Bereiche aus, die Ihren Anforderungen entsprechen.  
2. Schränken Sie den PoC durch Auswählen der Szenarien ein, die auf Ihre Geschäftsziele ausgerichtet sind. Je kürzer, desto besser. Es wird empfohlen, so kurz und präzise wie möglich zu bleiben, um allen Beteiligten die Vorteile zu vermitteln, aber gleichzeitig die Komplexität bei der Umsetzung zu minimieren.  
3. Verwenden Sie den Abschnitt [Implementierung](active-directory-playbook-implementation.md), um die Szenarien und ihre Bedeutung für Ihre Umgebung besser zu verstehen. In jedem Szenario wird beschrieben, wie dieses eingerichtet wird (wir bezeichnen dies als [Bausteine](active-directory-playbook-building-blocks.md)) und wie Sie zwischen den Szenarien navigieren. 
4. Jeder Baustein erläutert die erforderlichen Voraussetzungen sowie die geschätzte Dauer der Durchführung. Das hilft Ihnen bei der Planung. 
5. Basierend auf den Punkten 1 bis 3 oben definieren Sie die [Umgebung](active-directory-playbook-ingredients.md#environment) für die Ausführung. Es wird empfohlen, eine Produktionsumgebung anzustreben, um ein besseres Gefühl für die Auswirkungen auf Ihre Benutzer zu erhalten. 
6. Wenn bei den Anforderungen Konflikt entstehen, verwenden Sie die folgende Matrix für die Suche nach einem Kompromiss. 
   * Designorientiert – zur Veranschaulichung des Werts  
   * Einfachheit bei der Vorbereitung, Einrichtung und Ausführung der Szenarien 
   * Minimale Zeit zum Ausführen der Zielszenarien 
   * Maximale Ähnlichkeit zur Produktionsumgebung im Rahmen der gegebenen Einschränkungen 

>[!NOTE]
> In diesem Artikel werden einige Drittanbieteranwendungen und -produkte genannt, die der Einfachheit halber als Beispiele dienen. Azure AD unterstützt Tausende von Anwendungen aus unserem [Anwendungskatalog](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps), die Sie je nach Ihren Anforderungen und Ihrer Umgebung verwenden können. 



[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]
