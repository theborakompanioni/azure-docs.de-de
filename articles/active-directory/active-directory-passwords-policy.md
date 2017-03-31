---
title: "Kennwortrichtlinien und -einschränkungen in Azure Active Directory | Microsoft-Dokumentation"
description: "Beschreibt die Richtlinien, die in Azure Active Directory für Kennwörter gelten, einschließlich der zulässigen Zeichen, der Länge und dem Ablauf."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: e7cae5cf-e0ee-467b-9a90-db6fdf56cd52
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2017
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: 07635b0eb4650f0c30898ea1600697dacb33477c
ms.openlocfilehash: 9f6c36011941510d2da7f6a043128e493c13d47b
ms.lasthandoff: 03/28/2017


---
# <a name="password-policies-and-restrictions-in-azure-active-directory"></a>Kennwortrichtlinien und -einschränkungen in Azure Active Directory
Dieser Artikel beschreibt die Kennwortrichtlinien und Komplexitätsanforderungen im Zusammenhang mit den in Ihrem Azure AD-Verzeichnis gespeicherten Benutzerkonten.

> [!IMPORTANT]
> **Sind Sie hier, weil Sie Probleme bei der Anmeldung haben?** Wenn ja, helfen Ihnen die Informationen zum [Ändern und Zurücksetzen Ihres eigenen Kennworts](active-directory-passwords-update-your-own-password.md#reset-your-password)weiter.
>
>

## <a name="userprincipalname-policies-that-apply-to-all-user-accounts"></a>UserPrincipalName-Richtlinien, die für alle Benutzerkonten gelten
Jedes Benutzerkonto, das sich beim Azure AD-Authentifizierungssystem anmeldet, muss über ein eindeutiges, diesem Konto zugeordnetes Benutzerprinzipalnamenattribut (UPN, User Principal Name) verfügen. Die folgende Tabelle zeigt die Richtlinien, die sowohl für lokale Active Directory-Benutzerkonten (mit Cloudsynchronisierung) als auch für Benutzerkonten gelten, die ausschließlich in der Cloud gespeichert werden.

| Eigenschaft | UserPrincipalName-Richtlinien |
| --- | --- |
| Zulässige Zeichen |<ul> <li>A – Z</li> <li>a – z</li><li>0 – 9</li> <li> verfügbar. - \_ ! \# ^ \~</li></ul> |
| Unzulässige Zeichen |<ul> <li>Alle @-Zeichen, die nicht den Benutzernamen und die Domäne trennen.</li> <li>Darf keinen Punkt (.) unmittelbar vor dem @-Symbol enthalten.</li></ul> |
| Längenbeschränkungen |<ul> <li>Die Gesamtlänge darf 113 Zeichen nicht überschreiten.</li><li>64 Zeichen vor dem @-Symbol</li><li>48 Zeichen nach dem @-Symbol</li></ul> |

## <a name="password-policies-that-apply-only-to-cloud-user-accounts"></a>Kennwortrichtlinien, die nur für Cloudbenutzerkonten gelten
Die folgende Tabelle beschreibt die verfügbaren Kennwortrichtlinieneinstellungen, die auf die in Azure AD erstellten und verwalteten Benutzerkonten angewendet werden können.

| Eigenschaft | Anforderungen |
| --- | --- |
| Zulässige Zeichen |<ul><li>A – Z</li><li>a – z</li><li>0 – 9</li> <li>@ # $ % ^ & * - _ ! + = [ ] { } &#124; \ : ‘ , . ? / ` ~ “ ( ) ;</li></ul> |
| Unzulässige Zeichen |<ul><li>Unicode-Zeichen</li><li>Leerzeichen</li><li> **Nur für sichere Kennwörter**: Dürfen keinen Punkt (.) unmittelbar vor dem @-Symbol enthalten.</li></ul> |
| Kennworteinschränkungen |<ul><li>Mindestens 8 Zeichen und höchstens 16 Zeichen</li><li>**Nur für sichere Kennwörter**: Muss drei der folgenden vier Elemente enthalten:<ul><li>Kleinbuchstaben</li><li>Großbuchstaben</li><li>Zahlen (0 bis 9)</li><li>Symbole (siehe „Kennworteinschränkungen“ weiter oben)</li></ul></li></ul> |
| Zeitraum bis zum Ablauf des Kennworts |<ul><li>Standardwert: **90** Tage </li><li>Der Wert kann im Azure Active Directory-Modul für Windows PowerShell mit dem Cmdlet „Set-MsolPasswordPolicy“ konfiguriert werden.</li></ul> |
| Benachrichtigung bei Ablauf des Kennworts |<ul><li>Standardwert: **14** Tage (bevor das Kennwort abläuft)</li><li>Der Wert kann mit dem Cmdlet „Set-MsolPasswordPolicy“ konfiguriert werden.</li></ul> |
| Ablauf des Kennworts |<ul><li>Standardwert: **false** Tage (Gibt an, dass der Kennwortablauf aktiviert ist.) </li><li>Der Wert kann mit dem Cmdlet „Set-MsolUser“ für einzelne Benutzerkonten konfiguriert werden. </li></ul> |
| Verlauf der **Kennwortänderungen** |Das letzte Kennwort kann beim **Ändern** eines Kennworts **nicht** erneut verwendet werden. |
| Verlauf der **Kennwortzurücksetzungen** | Das letzte Kennwort **darf** erneut verwendet werden, wenn ein vergessenes Kennwort **zurückgesetzt** wird. |
| Kontosperrung |Nach zehn nicht erfolgreichen Anmeldeversuchen (falsches Kennwort) wird der Benutzer für eine Minute gesperrt. Mit jeder weiteren nicht erfolgreichen Anmeldung verlängert sich die Dauer, die der Benutzer gesperrt ist. |

## <a name="next-steps"></a>Nächste Schritte
* **Sind Sie hier, weil Sie Probleme bei der Anmeldung haben?** Wenn ja, helfen Ihnen die Informationen zum [Ändern und Zurücksetzen Ihres eigenen Kennworts](active-directory-passwords-update-your-own-password.md#reset-your-password)weiter.
* [Verwalten von Kennwörtern von einem beliebigen Ort aus](active-directory-passwords.md)
* [Funktionsweise der Kennwortverwaltung](active-directory-passwords-how-it-works.md)
* [Erste Schritte mit der Kennwortverwaltung](active-directory-passwords-getting-started.md)
* [Anpassen der Kennwortverwaltung](active-directory-passwords-customize.md)
* [Best Practices für die Kennwortverwaltung](active-directory-passwords-best-practices.md)
* [Operative Einblicke durch Berichte zur Kennwortverwaltung](active-directory-passwords-get-insights.md)
* [Häufig gestellte Fragen zur Kennwortverwaltung](active-directory-passwords-faq.md)
* [Problembehandlung für die Kennwortverwaltung](active-directory-passwords-troubleshoot.md)
* [Weitere Informationen](active-directory-passwords-learn-more.md)

