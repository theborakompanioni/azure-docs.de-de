---
title: Bedingter Zugriff im klassischen Azure-Portal | Microsoft-Dokumentation
description: "Verwenden Sie die Steuerung des bedingten Zugriffs im klassischen Azure-Portal, um bei der Authentifizierung für den Zugriff auf Anwendungen eine Überprüfung auf bestimmte Bedingungen durchzuführen."
services: active-directory
keywords: "bedingter Zugriff auf Apps, bedingter Zugriff mit Azure AD, sicherer Zugriff auf Unternehmensressourcen, Richtlinien für bedingten Zugriff"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: da3f0a44-1399-4e0b-aefb-03a826ae4ead
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/01/2017
ms.author: markvi
ms.reviewer: calebb
ms.custom: oldportal
ms.translationtype: Human Translation
ms.sourcegitcommit: bb794ba3b78881c967f0bb8687b1f70e5dd69c71
ms.openlocfilehash: b19fd8c0c41638ffec5fda32b6b1a855be2abc7b
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017

---
# <a name="conditional-access-in-the-azure-classic-portal"></a>Bedingter Zugriff im klassischen Azure-Portal

In diesem Thema wird der bedingte Zugriff im klassischen Azure-Portal erläutert. Aktuelle Informationen zum bedingten Zugriff in Azure Active Directory finden Sie unter [Bedingter Zugriff in Azure Active Directory](active-directory-conditional-access-azure-portal.md).


Die Steuerungsfunktionen für den bedingten Zugriff in Azure Active Directory (Azure AD) bieten einfache Methoden, mit denen Sie Ressourcen in der Cloud und lokal schützen können. Richtlinien für den bedingten Zugriff, z.B. die Anforderung einer mehrstufigen Authentifizierung, können Schutz davor bieten, dass Anmeldeinformationen gestohlen oder ausgespäht werden. Mit weiteren Richtlinien für den bedingten Zugriff können Sie Ihre Organisationsdaten schützen. Beispielsweise können Sie zusätzlich zur Anforderung von Anmeldeinformationen eine Richtlinie verwenden, die vorschreibt, dass nur in einem Verwaltungssystem für mobile Geräte – beispielsweise Microsoft Intune – registrierte Geräte auf vertrauliche Dienste Ihrer Organisation zugreifen dürfen.

## <a name="prerequisites"></a>Voraussetzungen
Der bedingte Zugriff in Azure AD ist ein Feature von [Azure Active Directory Premium](http://www.microsoft.com/identity). Alle Benutzer, die beim Zugriff auf eine Anwendung eine Richtlinie für den bedingten Zugriff verwenden, müssen über eine Azure AD Premium-Lizenz verfügen. Weitere Informationen zu Lizenzanforderungen finden Sie im [Bericht zu nicht lizenzierter Nutzung](https://aka.ms/utc5ix).

## <a name="how-is-conditional-access-control-enforced"></a>Wie wird die bedingte Zugriffssteuerung durchgesetzt?
Bei Verwendung der bedingten Zugriffssteuerung prüft Azure AD auf bestimmte Bedingungen, die Sie für einen Benutzer festlegen, damit dieser auf eine Anwendung zugreifen kann. Wenn diese Zugriffsanforderungen erfüllt sind, wird der Benutzer authentifiziert und erhält Zugriff auf die Anwendung.  

![Bedingter Zugriff – Überblick](./media/active-directory-conditional-access/conditionalaccess-overview.png)

## <a name="conditions"></a>Bedingungen
Diese Bedingungen können Sie in eine Richtlinie für den bedingten Zugriff aufnehmen:

* **Gruppenmitgliedschaft**: Steuert den Benutzerzugriff basierend auf der Mitgliedschaft in einer Gruppe.
* **Standort**: Verwenden Sie den Standort des Benutzers, um eine mehrstufige Authentifizierung auszulösen und Steuerelemente zu blockieren, wenn ein Benutzer sich nicht in einem vertrauenswürdigen Netzwerk befindet.
* **Geräteplattform**: Verwenden Sie den Typ der Geräteplattform, z.B. iOS, Android, Windows Mobile und Windows, als Bedingung für die Anwendung der Richtlinie.
* **Gerät aktiviert**: Der Status „Gerät aktiviert/Gerät deaktiviert“ wird während der Auswertung der Geräterichtlinie überprüft. Wenn ein verlorenes oder gestohlenes Gerät im Verzeichnis deaktiviert wird, kann es nicht mehr verwendet werden, um Richtlinienanforderungen zu erfüllen.
* **Anmelde- und Benutzerrisiko**: Sie können [Azure AD Identity Protection](active-directory-identityprotection.md) für Richtlinien für den bedingten Zugriff verwenden, die auf Risikofaktoren basieren. Diese Richtlinien für den bedingten Zugriff bieten Ihrer Organisation erweiterten Schutz basierend auf Risikoereignissen und ungewöhnlichen Anmeldeaktivitäten.

## <a name="controls"></a>Steuerelemente
Diese Steuerelemente können Sie zum Erzwingen einer Richtlinie für den bedingten Zugriff verwenden:

* **Mehrstufige Authentifizierung**: Sie können über die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) eine sichere Authentifizierung erzwingen. Sie können die mehrstufige Authentifizierung über Azure Multi-Factor Authentication oder über einen lokalen MFA-Anbieter verwenden, kombiniert mit den Active Directory-Verbunddiensten (AD FS). Die mehrstufige Authentifizierung unterstützt den Schutz Ihrer Ressourcen vor dem Zugriff durch einen nicht autorisierten Benutzer, der möglicherweise Zugriff auf die Anmeldeinformationen eines gültigen Benutzers erlangt hat.
* **Blockierung**: Bedingungen wie z.B. der Benutzerstandort können zum Blockieren des Benutzerzugriffs herangezogen werden. Beispielsweise können Sie den Zugriff blockieren, wenn sich ein Benutzer nicht in einem vertrauenswürdigen Netzwerk befindet.
* **Kompatible Geräte**: Sie können die Richtlinien für den bedingten Zugriff auf Geräteebene festlegen. Sie können eine Richtlinie einrichten, sodass nur in die Domäne eingebundene Computer oder mobile Geräte, die in einer Anwendung zur Verwaltung mobiler Geräte registriert sind, auf Ihre Organisationsressourcen zugreifen können. Beispielsweise können Sie Intune verwenden, um die Gerätekompatibilität zu überprüfen und das Ergebnis zur Richtlinienerzwingung an Azure AD übermitteln, wenn der Benutzer versucht, auf eine Anwendung zuzugreifen. Ausführliche Anleitungen dazu, wie Intune zum Schutz von Apps und Daten eingesetzt werden kann, finden Sie unter [Schützen von Apps und Daten mit Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/protect-apps-and-data-with-microsoft-intune). Sie können mithilfe von Intune auch Datenschutz für verlorene oder gestohlene Geräte erzwingen. Weitere Informationen finden Sie unter [Schützen Ihrer Daten mit vollständigem oder selektivem Löschen über Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune).

## <a name="applications"></a>Anwendungen
Sie können eine Richtlinie für den bedingten Zugriff auf Anwendungsebene erzwingen. Richten Sie Zugriffsebenen für Anwendungen und Dienste in der Cloud oder lokal ein. Die Richtlinie wird direkt auf die Website oder den Dienst angewendet. Die Richtlinie wird für den Zugriff auf den Browser erzwungen und auf Anwendungen, die auf diesen Dienst zugreifen.

## <a name="device-based-conditional-access"></a>Gerätebasierter bedingter Zugriff
Sie können den Zugriff auf Anwendungen auch auf Geräte beschränken, die bei Azure AD registriert sind und bestimmte Bedingungen erfüllen. Dies ist hilfreich, um Organisationsressourcen vor dem Zugriff durch Benutzer über folgende Geräte zu schützen:

* Unbekannte oder nicht verwaltete Geräte
* Geräte, die die Sicherheitsrichtlinien Ihrer Organisation nicht erfüllen

Sie können Richtlinien basierend auf diesen Anforderungen festlegen:

* **In die Domäne eingebundene Geräte**: Sie können eine Richtlinie festlegen, um den Zugriff auf Geräte zu beschränken, die einer lokalen Active Directory-Domäne beigetreten und außerdem bei Azure AD registriert sind. Diese Richtlinie gilt für Windows-Desktops, -Laptops und -Unternehmenstablets.
  Weitere Informationen zum Einrichten der automatischen Registrierung von in die Domäne eingebundenen Geräten bei Azure AD finden Sie unter [Einrichten der automatischen Registrierung von in die Domäne eingebundenen Windows-Geräten bei Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).
* **Kompatible Geräte**: Sie können eine Richtlinie festlegen, um den Zugriff auf Geräte zu beschränken, die vom Verwaltungssystem im Verzeichnis als **kompatibel** gekennzeichnet wurden. Diese Richtlinie stellt sicher, dass nur Geräten der Zugriff gewährt wird, die Sicherheitsrichtlinien erfüllen, wie z.B. das Erzwingen der Dateiverschlüsselung auf einem Gerät. Diese Richtlinie kann verwendet werden, um den Zugriff über folgende Geräte zu beschränken:
  
  * **In eine Windows-Domäne eingebundene Geräte**: Die Verwaltung erfolgt über System Center Configuration Manager (Current Branch) in einer Hybridkonfiguration.
  * **Windows 10 Mobile-Firmengeräte oder persönliche Geräte**: Werden über Intune durch ein unterstütztes Drittanbietersystem für die Verwaltung mobiler Geräte verwaltet.
  * **iOS- und Android-Geräte**: Werden über Intune verwaltet.

Benutzer, die auf Anwendungen zugreifen, die über eine gerätebasierte Richtlinie einer Zertifizierungsstelle geschützt werden, müssen über ein Gerät auf die Anwendung zugreifen, das den Richtlinienanforderungen entspricht. Der Zugriff wird verweigert, wenn er über ein Gerät erfolgt, das die Richtlinienanforderungen nicht erfüllt.

Informationen zum Konfigurieren einer gerätebasierten Richtlinie einer Zertifizierungsstelle in Azure AD finden Sie unter [Festlegen von gerätebasierten Azure Active Directory-Richtlinien für bedingten Zugriff zur Steuerung des Zugriffs auf über Azure Active Directory verbundene Anwendungen](active-directory-conditional-access-policy-connected-applications.md).

## <a name="resources"></a>Ressourcen
In den folgenden Ressourcenkategorien und Artikeln werden weitere Informationen zum Einrichten des bedingten Zugriffs für Ihre Organisation bereitgestellt.

### <a name="multi-factor-authentication-and-location-policies"></a>Mehrstufige Authentifizierung und Standortrichtlinien
* [Erste Schritte mit bedingtem Zugriff auf verbundene Azure AD-Apps basierend auf Gruppen-, Standort- und MFA-Richtlinien](active-directory-conditional-access-azuread-connected-apps.md)
* [Unterstützte Anwendungen und Browser](active-directory-conditional-access-supported-apps.md)

### <a name="device-based-conditional-access"></a>Gerätebasierter bedingter Zugriff
* [Festlegen von gerätebasierten Richtlinien für bedingten Zugriff zur Steuerung des Zugriffs auf über Azure Active Directory verbundene Anwendungen](active-directory-conditional-access-policy-connected-applications.md)
* [Einrichten der automatischen Registrierung bei Azure Active Directory für in Domänen eingebundene Windows-Geräte](active-directory-conditional-access-automatic-device-registration-setup.md)
* [Problembehandlung für Azure Active Directory-Zugriffsprobleme](active-directory-conditional-access-device-remediation.md)
* [Schützen von Daten auf verlorenen oder gestohlenen Geräten mit Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune)

### <a name="protect-resources-based-on-sign-in-risk"></a>Schützen von Ressource basierend auf dem Anmelderisiko
* [Azure AD Identity Protection](active-directory-identityprotection.md)

### <a name="next-steps"></a>Nächste Schritte
* [Häufig gestellte Fragen zum bedingten Zugriff](active-directory-conditional-faqs.md)
* [Technische Referenz](active-directory-conditional-access-technical-reference.md)


