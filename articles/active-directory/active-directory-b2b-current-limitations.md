---
title: "Einschränkungen der Azure Active Directory B2B-Zusammenarbeit | Microsoft-Dokumentation"
description: "Aktuelle Einschränkungen der Azure Active Directory B2B-Zusammenarbeit"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/04/2017
ms.author: sasubram
ms.translationtype: Human Translation
ms.sourcegitcommit: afa23b1395b8275e72048bd47fffcf38f9dcd334
ms.openlocfilehash: f13f4ac6e32f568fb5f56261b542e0e2c49f5b3d
ms.contentlocale: de-de
ms.lasthandoff: 05/12/2017


---

# <a name="limitations-of-azure-ad-b2b-collaboration"></a>Einschränkungen der Azure AD B2B-Zusammenarbeit
Azure Active Directory B2B-Zusammenarbeit (Azure AD) unterliegt derzeit den in diesem Artikel beschriebenen Einschränkungen.

## <a name="possible-double-multi-factor-authentication"></a>Mögliche doppelte Multi-Factor Authentication
Mit Azure AD B2B können Sie die Multi-Factor Authentication bei der Ressourcenorganisation (die einladende Organisation) erzwingen. Die Gründe für diesen Ansatz werden in [Multi-Factor Authentication für Benutzer der Azure Active Directory B2B-Zusammenarbeit](active-directory-b2b-mfa-instructions.md) erläutert. Dies bedeutet Folgendes: Wenn ein Partner bereits Multi-Factor Authentication eingerichtet hat und sie erzwingt, müssen die Benutzer beim Partner die Authentifizierung möglicherweise einmal in ihrer eigenen Organisation ausführen und dann erneut in Ihrer.

In einem zukünftigen Release möchten wir eine Richtlinie einführen, mit der Sie das Problem der doppelten Authentifizierung vermeiden können, indem Sie auswählen, der Multi-Factor Authentication des Partners zu vertrauen.


## <a name="instant-on"></a>Instant-On
Im Workflow der B2B-Zusammenarbeit werden Benutzer zum Verzeichnis hinzugefügt und während der Einlösung der Einladung, der App-Zuweisung usw. dynamisch aktualisiert. Die Aktualisierungs- und Schreibvorgänge erfolgen im Allgemeinen in einer Verzeichnisinstanz und müssen in allen Instanzen repliziert werden. Die Replikation wird durchgeführt, nachdem alle Instanzen aktualisiert wurden. Wenn das Objekt in einer Instanz von Active Directory geschrieben oder aktualisiert wurde und der Aufruf zum Abrufen dieses Objekts aufgrund von Lastenausgleich in einer anderen Instanz erfolgt ist, hat dies zuweilen zu Autorisierungsproblemen geführt. Wir haben viel unternommen, um diese Replikationswartezeiten zu eliminieren oder zu reduzieren, sie können jedoch in seltenen Fällen noch auftreten. Wenn dies der Fall ist, führen Sie eine Aktualisierung aus, oder wiederholen Sie den Vorgang. Wenn Sie mit unserer API eine App schreiben, ist die Wiederholung des Vorgangs mit einem Backoff-Intervall ein sinnvolles Verfahren, um dieses Problem zu verringern.

## <a name="next-steps"></a>Nächste Schritte

Weitere Artikel zur Azure AD B2B-Kollaboration:

* [Was ist die Azure AD B2B-Zusammenarbeit?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Eigenschaften von B2B-Zusammenarbeitsbenutzern](active-directory-b2b-user-properties.md)
* [Hinzufügen eines B2B-Zusammenarbeitsbenutzers zu einer Rolle](active-directory-b2b-add-guest-to-role.md)
* [Delegieren von Einladungen zur B2B-Zusammenarbeit](active-directory-b2b-delegate-invitations.md)
* [Dynamische Gruppen und B2B-Zusammenarbeit](active-directory-b2b-dynamic-groups.md)
* [B2B-Zusammenarbeit: Code- und PowerShell-Beispiele](active-directory-b2b-code-samples.md)
* [Konfigurieren von SaaS-Apps für die B2B-Zusammenarbeit](active-directory-b2b-configure-saas-apps.md)
* [Benutzertoken für die B2B-Zusammenarbeit](active-directory-b2b-user-token.md)
* [Zuordnen von Benutzeransprüchen für die B2B-Zusammenarbeit](active-directory-b2b-claims-mapping.md)
* [Externe Office 365-Freigaben](active-directory-b2b-o365-external-user.md)

