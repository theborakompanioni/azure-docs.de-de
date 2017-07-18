---
title: 'Lizenzierung: Azure AD SSPR | Microsoft-Dokumentation'
description: "Lizenzanforderungen für die Self-Service-Kennwortzurücksetzung in Azure AD"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: it-pro
ms.translationtype: Human Translation
ms.sourcegitcommit: be3ac7755934bca00190db6e21b6527c91a77ec2
ms.openlocfilehash: 3ed13c819f8e32cab44013cdcbf1b3a921ba98b8
ms.contentlocale: de-de
ms.lasthandoff: 05/03/2017

---
# Lizenzanforderungen für Azure AD-Self-Service-Kennwortzurücksetzung
<a id="licensing-requirements-for-azure-ad-self-service-password-reset" class="xliff"></a>

**In Ihrer Organisation muss mindestens eine Lizenz zugewiesen sein**, damit die Kennwortzurücksetzung in Azure AD funktioniert. Auf der Benutzeroberfläche für das Zurücksetzen von Kennwörtern wird keine Einzelbenutzerlizenzierung erzwungen. Um Konformität mit Ihrer Microsoft-Lizenzvereinbarung zu gewährleisten, müssen Sie allen Benutzern Lizenzen zuweisen, die Premiumfeatures verwenden.

* **Nur Cloudbenutzer:** beliebige kostenpflichtige SKU für Office 365 (O365) oder Azure AD Basic
* **Cloudbenutzer** und/oder **lokale Benutzer**: Azure AD Premium P1 oder P2, Enterprise Mobility + Security (EMS) oder Secure Productive Enterprise (SPE)

## Erforderliche Lizenzen für das Kennwortrückschreiben
<a id="licenses-required-for-password-writeback" class="xliff"></a>

Zur Verwendung des Kennwortrückschreibens muss in Ihrem Mandanten eine der folgenden Lizenzen zugewiesen sein.

* Azure AD Premium P1
* Azure AD Premium P2
* Enterprise Mobility + Security E3
* Enterprise Mobility + Security E5
* Secure Productive Enterprise E3
* Secure Productive Enterprise E5

> [!NOTE]
> Eigenständige Office 365-Lizenzierungspläne **bieten keine Unterstützung für das Kennwortrückschreiben**. Sie benötigen einen der obigen Pläne, damit diese Funktion verwendet werden kann.

Weitere Informationen zur Lizenzierung, einschließlich der Kosten, finden Sie auf den folgenden Seiten.

* [Website mit Preisen für Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Secure Productive Enterprise](https://www.microsoft.com/secure-productive-enterprise/default.aspx)

## Aktivieren der gruppen- oder benutzerbasierten Lizenzierung
<a id="enable-group-or-user-based-licensing" class="xliff"></a>

Azure AD unterstützt jetzt die gruppenbasierte Lizenzierung, mit der Administratoren in einem Massenvorgang einer Gruppe von Benutzern Lizenzen zuweisen können, anstatt sie einzeln zuzuweisen. [Zuweisen, Prüfen und Beheben von Problemen mit Lizenzen](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses)

Einige Microsoft-Dienste sind nicht an allen Standorten verfügbar. Bevor einem Benutzer eine Lizenz zugewiesen werden kann, muss der Administrator die Eigenschaft „Verwendungsstandort“ für den Benutzer angeben. Die Zuweisung von Lizenzen erfolgt im Azure-Portal unter „Benutzer“ > „Profil“ > „Einstellungen“. **Bei Verwendung der Gruppenlizenzzuweisung erben alle Benutzer ohne Verwendungsstandort den Standort des Verzeichnisses.**

## Nächste Schritte
<a id="next-steps" class="xliff"></a>

Die folgenden Links führen zu weiteren Informationen zur Kennwortzurücksetzung mit Azure AD

* [**Schnellstart:**](active-directory-passwords-getting-started.md) Informieren Sie sich schnell über die Self-Service-Kennwortverwaltung von Azure AD. 
* [**Daten:**](active-directory-passwords-data.md) Erfahren Sie, welche Daten erforderlich sind und wie sie für die Kennwortverwaltung verwendet werden.
* [**Rollout:**](active-directory-passwords-best-practices.md) Mithilfe der hier enthaltenen Anleitungen können Sie SSPR planen und für Ihre Benutzer bereitstellen.
* [**Anpassung:**](active-directory-passwords-customize.md) Passen Sie das Aussehen und Verhalten von SSPR für Ihr Unternehmen an.
* [**Berichterstellung:**](active-directory-passwords-reporting.md) Ermitteln Sie, ob, wann und wo Ihre Benutzer auf SSPR-Funktionen zugreifen.
* [**Ausführliche technische Informationen:**](active-directory-passwords-how-it-works.md) Steigen Sie tiefer ein, um zu verstehen, wie alles funktioniert.
* [**Häufig gestellte Fragen (FAQ):**](active-directory-passwords-faq.md) Wie? Warum? Was? Wo? Wer? Wann? - Antworten auf Fragen, die Sie schon immer stellen wollten
* [**Problembehandlung:**](active-directory-passwords-troubleshoot.md) Erfahren Sie, wie Sie häufig auftretende Probleme bei SSPR beheben.


