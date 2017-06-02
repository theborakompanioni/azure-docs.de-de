---

title: "Übersicht über die Self-Service-Kennwortzurücksetzung in Azure AD | Microsoft-Dokumentation"
description: "Wie kann Ihre Organisation von der Self-Service-Kennwortzurücksetzung in Azure AD profitieren?"
services: active-directory
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: joflore
ms.editor: gahug
ms.custom: it-pro
ms.translationtype: Human Translation
ms.sourcegitcommit: 300958a69fc854cb8db02120a383a4cbbfcacd7b
ms.openlocfilehash: 9f72a4900f20282827ff939aa41bc4f306e547a3
ms.contentlocale: de-de
ms.lasthandoff: 05/20/2017


---
# <a name="azure-ad-self-service-password-reset-for-the-it-professional"></a>Self-Service-Kennwortzurücksetzung in Azure AD für IT-Experten

„Self-Service“ ist ein Modewort, das in vielen IT-Abteilungen auf der ganzen Welt mit unterschiedlichen Bedeutungen verwendet wird. Der Markt wird mit Produkten überflutet, mit denen Sie lokale Gruppen, Kennwörter oder Benutzerprofile über die Cloud oder lokal verwalten können.

Die Self-Service-Kennwortzurücksetzung (SSPR) in Azure Active Directory (Azure AD) hebt sich von anderen Produkten durch eine einfache Verwendung und Bereitstellung ab. Die Self-Service-Kennwortzurücksetzung in Azure AD kombiniert eine Reihe von Funktionen, die Folgendes ermöglichen:

* Zulassen, dass Ihre Benutzer ihre eigenen Kennwörter verwalten
  * Auf einem beliebigen Gerät
  * An einem beliebigen Ort
  * Zu einem beliebigen Zeitpunkt
* Einhalten von Richtlinien, die Sie als Administrator definieren

Wenn Sie bereit sind, können Sie mithilfe unserer [Schnellstartanleitung](active-directory-passwords-getting-started.md) mit der Nutzung von Azure AD SSPR beginnen und schnell Ihre Benutzer in die Lage versetzen, ihre eigenen Kennwörter zurückzusetzen.

## <a name="what-is-possible"></a>Funktionen

* Mit der **Self-Service-Kennwortänderung** können Endbenutzer oder Administratoren ohne Unterstützung durch den Administrator ihre Kennwörter ändern.
* Mit der **Self-Service-Kontoentsperrung** können Endbenutzer ohne Unterstützung durch den Administrator ihre eigenen Konten entsperren.
* Mit der **Self-Service-Kennwortzurücksetzung** können Endbenutzer oder Administratoren ohne Unterstützung durch den Administrator ihre Kennwörter automatisch zurücksetzen. Die Self-Service-Kennwortzurücksetzung erfordert Azure AD Premium oder Basic – [Azure Active Directory-Editionen](active-directory-editions.md).
* Mit der vom **Administrator ausgelösten Kennwortzurücksetzung** kann ein Administrator über das [Azure-Portal](https://docs.microsoft.com/azure/azure-portal-overview) das Kennwort eines Endbenutzers oder eines anderen Administrators zurücksetzen.
* **Berichte zu Kennwortverwaltungsaktivitäten** bieten Administratoren Einblicke in die Aktivitäten, die sich in ihrer Organisation in Bezug auf die Kennwortzurücksetzung und -registrierung ereignen – [Verwaltungsberichte](active-directory-passwords-reporting.md)
* Das **Kennwortrückschreiben** ermöglicht die Verwaltung lokaler Kennwörter in der Cloud, sodass alle oben genannten Szenarien von oder im Auftrag von Verbundbenutzern oder Benutzern mit Kennwortsynchronisierung durchgeführt werden können. Für das Kennwortrückschreiben ist [Azure AD Premium](active-directory-get-started-premium.md) erforderlich.

## <a name="why-choose-azure-ad-self-service-password-reset"></a>Gründe für die Self-Service-Kennwortzurücksetzung in Azure AD

* **Senken Sie die Kosten**, da die vom Helpdesk und Support unterstützte Kennwortzurücksetzung in der Regel 20 % der IT-Ausgaben einer Organisation ausmachen.
* **Verbessern Sie die Endbenutzererfahrungen**, und **reduzieren Sie den Helpdesk**, indem Sie Endbenutzern die Möglichkeit geben, eigene Probleme mit Kennwörtern sofort zu beheben, ohne den Helpdesk anzurufen oder eine Supportanfrage zu öffnen.
* **Fördern Sie die Mobilität**, da Benutzer ihre Kennwörter unabhängig von ihrem Standort zurücksetzen können.

## <a name="azure-ad-self-service-password-reset-availability"></a>Verfügbarkeit der Self-Service-Kennwortzurücksetzung in Azure AD

Die Self-Service-Kennwortzurücksetzung in Azure AD ist in drei Tarifen abhängig von Ihrem Abonnement verfügbar.

* **Azure AD Free**: Cloudadministratoren können ihre eigenen Kennwörter zurücksetzen.
* **Azure AD Basic** oder ein beliebiges **kostenpflichtiges Office 365-Abonnement**: Cloudbenutzer und Cloudadministratoren können ihre eigenen Kennwörter zurücksetzen.
* **Azure AD Premium**: Alle Benutzer und alle Administratoren können ihre eigenen Kennwörter zurücksetzen, dazu gehören Cloudbenutzer, Verbundbenutzer und Benutzer mit Kennwortsynchronisierung. Für lokale Kennwörter muss das Kennwortrückschreiben aktiviert sein.

## <a name="azure-ad-self-service-password-reset-a-sum-of-the-parts"></a>Self-Service-Kennwortzurücksetzung in Azure AD – Komponenten

Die Self-Service-Kennwortzurücksetzung in Azure AD besteht aus den folgenden Komponenten:

* Im **Portal für die Konfiguration der Kennwortverwaltung** können Sie die Optionen steuern, mit denen Kennwörter in Ihrem Mandanten über das Azure-Portal verwaltet werden.
* Im **Portal für die Registrierung zur Kennwortzurücksetzung** können sich Benutzer selbst für die Kennwortzurücksetzung registrieren.
* Im **Portal für die Kennwortzurücksetzung** können Benutzer ihr Kennwort mithilfe der Prüfungen, die vom Administrator definiert wurden, und der Antworten, die von Benutzern bereitgestellt wurden, zurücksetzen.
* Im **Portal zum Ändern von Benutzerkennwörtern** können Benutzer ihre eigenen Kennwörter ändern, indem sie ihr altes Kennwort eingeben und ein neues Kennwort festlegen.
* Mit **Berichten zur Kennwortverwaltung** können Administratoren Berichte über Aktivitäten im Zusammenhang mit Kennwörtern für ihre Mandanten im Azure-Portal anzeigen und analysieren.
* Mit dem **Kennwortrückschreiben auf lokale Standorte mit Azure AD Connect** können Sie die Verwaltung von lokalen Benutzern, Verbundbenutzern oder Benutzern mit Kennwortsynchronisierung über die Cloud ermöglichen.

## <a name="azure-ad-pricing-sla-updates-and-roadmap"></a>Azure AD-Preise, SLA, Updates und Roadmap

Weitere Details zu diesen Themen finden Sie auf den folgenden Seiten:

* [**Azure Active Directory – Preise**](https://azure.microsoft.com/pricing/details/active-directory/)
* [**Office 365 – Preise**](https://products.office.com/compare-all-microsoft-office-products?tab=2)
* [**Azure – Vereinbarungen zum Servicelevel (SLAs)**](https://azure.microsoft.com/support/legal/sla/)
* [**Vereinbarung zum Servicelevel für Microsoft Online Services**](http://go.microsoft.com/fwlink/?LinkID=272026&clcid=0x409)
* [**Azure-Updates**](https://azure.microsoft.com/updates/)
* [**Roadmap für Azure**](https://www.microsoft.com/cloud-platform/roadmap-recently-available)

## <a name="next-steps"></a>Nächste Schritte

Die folgenden Links führen zu weiteren Informationen zur Kennwortzurücksetzung mit Azure AD:

* [**Schnellstart:**](active-directory-passwords-getting-started.md) Informieren Sie sich schnell über die Self-Service-Kennwortverwaltung von Azure AD. 
* [**Lizenzierung:**](active-directory-passwords-licensing.md) Konfigurieren Sie Ihre Azure AD-Lizenzierung.
* [**Daten:**](active-directory-passwords-data.md) Erfahren Sie, welche Daten erforderlich sind und wie sie für die Kennwortverwaltung verwendet werden.
* [**Rollout:**](active-directory-passwords-best-practices.md) Mithilfe der hier enthaltenen Anleitungen können Sie SSPR planen und für Ihre Benutzer bereitstellen.
* [**Anpassung:**](active-directory-passwords-customize.md) Passen Sie das Aussehen und Verhalten von SSPR für Ihr Unternehmen an.
* [**Berichterstellung:**](active-directory-passwords-reporting.md) Ermitteln Sie, ob, wann und wo Ihre Benutzer auf SSPR-Funktionen zugreifen.
* [**Ausführliche technische Informationen:**](active-directory-passwords-how-it-works.md) Steigen Sie tiefer ein, um zu verstehen, wie alles funktioniert.
* [**Häufig gestellte Fragen (FAQ):**](active-directory-passwords-faq.md) Wie? Warum? Was? Wo? Wer? Wann? - Antworten auf Fragen, die Sie schon immer stellen wollten
* [**Problembehandlung:**](active-directory-passwords-troubleshoot.md) Erfahren Sie, wie Sie häufig auftretende Probleme bei SSPR beheben.


