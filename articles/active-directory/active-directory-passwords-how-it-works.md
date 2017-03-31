---
title: 'So funktioniert&quot;s: Azure AD-Kennwortverwaltung | Microsoft-Dokumentation'
description: "Lernen Sie die verschiedenen Komponenten der Azure AD-Kennwortverwaltung kennen, und erfahren Sie, wo Benutzer sich registrieren und ihre Kennwörter zurücksetzen und ändern können und wo Administratoren die Verwaltung lokaler Active Directory-Kennwörter konfigurieren und aktivieren sowie Berichte zur Kennwortverwaltung anzeigen können."
services: active-directory
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
editor: curtand
ms.assetid: 618c5908-5bf6-4f0d-bf88-5168dfb28a88
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2017
ms.author: joflore
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 07635b0eb4650f0c30898ea1600697dacb33477c
ms.openlocfilehash: 24c94f80afdb3d3330a835fae2f77ecb213a9ae5
ms.lasthandoff: 03/28/2017


---
# <a name="how-password-management-works-in-azure-active-directory"></a>Funktionsweise der Kennwortverwaltung in Azure Active Directory
> [!IMPORTANT]
> **Sind Sie hier, weil Sie Probleme bei der Anmeldung haben?** Wenn ja, helfen Ihnen die Informationen zum [Ändern und Zurücksetzen Ihres eigenen Kennworts](active-directory-passwords-update-your-own-password.md#reset-your-password).

Die Kennwortverwaltung in Azure Active Directory (Azure AD) setzt sich aus folgenden logischen Komponenten zusammen:

* **Portal für die Konfiguration der Kennwortverwaltung:** Im [Azure-Portal](https://manage.windowsazure.com) können Sie auf der Registerkarte **Konfigurieren** verschiedene Aspekte der Kennwortverwaltung für Ihre Mandanten steuern.
* **Portal für die Benutzerregistrierung:** Benutzer können sich über dieses Webportal für die Kennwortzurücksetzung registrieren.
* **Portal für das Zurücksetzen von Benutzerkennwörtern:** Benutzer können ihre eigenen Kennwörter durch Ausführen verschiedener Überprüfungsschritte zurücksetzen. (Die Schritte werden über die vom Administrator gesteuerte Richtlinie für die Kennwortzurücksetzung festlegt.)
* **Portal zum Ändern von Benutzerkennwörtern:** Benutzer können ihre eigenen Kennwörter jederzeit ändern, indem sie über dieses Webportal ihr altes Kennwort eingeben und dann ein neues Kennwort festlegen.
* **Berichte zur Kennwortverwaltung:** Im [Azure-Portal](https://manage.windowsazure.com) können Sie auf Ihrer verzeichnisspezifischen Registerkarte **Berichte** im Abschnitt **Aktivitätsberichte** Aktivitäten zur Kennwortzurücksetzung und zur Registrierung in Ihrem Mandanten anzeigen und analysieren.
* **Azure AD Connect-Komponente zum Kennwortrückschreiben:** Beim Installieren von Azure AD Connect können Sie optional das Kennwortrückschreiben-Feature aktivieren, um die Verwaltung von Verbundkennwörtern oder synchronisierten Benutzerkennwörtern aus der Cloud zu ermöglichen.

## <a name="password-management-configuration-portal"></a>Portal für die Konfiguration der Kennwortverwaltung
Im [Azure-Portal](https://manage.windowsazure.com) können Sie auf der Registerkarte **Konfigurieren** im Abschnitt **Richtlinie zum Zurücksetzen des Benutzerkennworts** Kennwortverwaltungsrichtlinien für ein bestimmtes Verzeichnis konfigurieren. Auf dieser Konfigurationsseite können Sie zahlreiche Aspekte der Kennwortverwaltung für Ihre Organisation steuern, darunter:

* Aktivieren und Deaktivieren der Kennwortzurücksetzung für alle Benutzer in einem Verzeichnis
* Festlegen der Mindestanzahl von Überprüfungsschritten (einer oder zwei), die ein Benutzer zum Zurücksetzen des Kennworts durchlaufen muss
* Festlegen der Art von Überprüfungsschritten, die Sie für Benutzer in Ihrer Organisation aktivieren möchten. Folgende Optionen stehen zur Auswahl:
  * Mobiltelefon (Prüfcode per SMS oder Sprachanruf)
  * Bürotelefon (Sprachanruf)
  * Alternative E-Mail-Adresse (Prüfcode per E-Mail)
  * Sicherheitsfragen (informationsbasierte Authentifizierung)
* Festlegen der Anzahl von Fragen, die ein Benutzer registrieren muss, damit er die Authentifizierungsmethode „Sicherheitsfragen“ verwenden kann (nur sichtbar, wenn Sicherheitsfragen aktiviert sind)
* Festlegen der Anzahl von Fragen, die ein Benutzer beim Zurücksetzen beantworten muss, um die Authentifizierungsmethode „Sicherheitsfragen“ verwenden zu können (nur sichtbar, wenn Sicherheitsfragen aktiviert sind)
* Verwenden vordefinierter, lokalisierter Sicherheitsfragen, aus denen ein Benutzer beim Registrieren für die Kennwortzurücksetzung auswählen kann (nur sichtbar, wenn Sicherheitsfragen aktiviert sind)
* Definieren der benutzerdefinierten Sicherheitsfragen, die ein Benutzer beim Registrieren für die Kennwortzurücksetzung auswählen kann (nur sichtbar, wenn Sicherheitsfragen aktiviert sind)
* Erzwingen der Benutzerregistrierung für die Kennwortzurücksetzung, wenn Benutzer zum [Zugriffsbereich](http://myapps.microsoft.com) der Anwendung navigieren
* Erzwingen der erneuten Bestätigung zuvor registrierter Daten durch die Benutzer nach einer bestimmten Anzahl von Tagen (nur sichtbar, wenn die erzwungene Registrierung aktiviert ist)
* Bereitstellen einer benutzerdefinierten Helpdesk-E-Mail-Adresse oder -URL, die Benutzern angezeigt wird, wenn beim Zurücksetzen von Kennwörtern Probleme auftreten
* Aktivieren oder Deaktivieren des Kennwortrückschreibens (wenn das Kennwortrückschreiben mithilfe von Azure AD Connect bereitgestellt wurde)
* Anzeigen des Status des Agents für das Kennwortrückschreiben (wenn das Kennwortrückschreiben mithilfe von Azure AD Connect bereitgestellt wurde)
* Aktivieren von E-Mail-Benachrichtigungen für Benutzer, wenn deren Kennwort zurückgesetzt wurde (zu finden im Abschnitt **Benachrichtigungen** des [Azure-Portals](https://manage.windowsazure.com))
* Aktivieren von E-Mail-Benachrichtigungen für Administratoren, wenn andere Administratoren deren Kennwörter zurücksetzen (zu finden im Abschnitt **Benachrichtigungen** des [Azure-Portals](https://manage.windowsazure.com))
* Branding des Portals für das Zurücksetzen von Benutzerkennwörtern und von E-Mails zur Kennwortzurücksetzung mit Ihrem Unternehmenslogo und -namen über das Anpassungsfeature für das Mandantenbranding (zu finden im Abschnitt **Verzeichniseigenschaften** des [Azure-Portals](https://manage.windowsazure.com))

Weitere Informationen zum Konfigurieren der Kennwortverwaltung in Ihrer Organisation finden Sie unter [Erste Schritte mit der Kennwortverwaltung](active-directory-passwords-getting-started.md).

## <a name="user-registration-portal"></a>Portal für die Benutzerregistrierung
Damit Ihre Benutzer die Kennwortzurücksetzung verwenden können, müssen Sie deren Cloudbenutzerkonten mit den korrekten Authentifizierungsdaten aktualisieren. Auf diese Weise wird sichergestellt, dass sie die geeignete Anzahl von Überprüfungsschritten für die Kennwortzurücksetzung durchlaufen können, die Sie definieren. Sie können diese Authentifizierungsinformationen im Namen der Benutzer über das Azure- oder das Office-Webportal, mithilfe von DirSync/Azure AD Connect oder mithilfe von Windows PowerShell definieren.

Falls Benutzer eigene Daten registrieren sollen, steht aber auch eine Webseite zur Verfügung, auf der Benutzer diese Informationen selbst hinzufügen können. Auf dieser Seite können Benutzer Authentifizierungsinformationen gemäß den Richtlinien für die Kennwortzurücksetzung eingeben, die in ihrer Organisation aktiviert wurde. Die überprüften Daten werden im zugehörigen Cloudbenutzerkonto gespeichert, damit sie später zur Kontowiederherstellung verwendet werden können.

Das Registrierungsportal sieht folgendermaßen aus:

  ![][001]

Weitere Informationen finden Sie unter [Erste Schritte mit der Kennwortverwaltung](active-directory-passwords-getting-started.md) sowie unter [Bereitstellen der Kennwortverwaltung und Schulen der Benutzer zu deren Verwendung](active-directory-passwords-best-practices.md).

## <a name="user-password-reset-portal"></a>Portal für das Zurücksetzen von Benutzerkennwörtern
Richten Sie nach Aktivierung der Self-Service-Kennwortzurücksetzung eine Unternehmensrichtlinie für die Self-Service-Kennwortzurücksetzung ein, und stellen Sie sicher, dass Ihre Benutzer im Verzeichnis über geeignete Kontaktdaten verfügen. Benutzer innerhalb der Organisation können ihre eigenen Kennwörter automatisch über eine beliebige Webseite zurücksetzen, die ein Geschäfts-, Schul- oder Unikonto für die Anmeldung verwendet (Beispiel: [portal.microsoftonline.com](https://portal.microsoftonline.com)). Auf Seiten wie dieser wird den Benutzern der Link **Können Sie nicht auf Ihr Konto zugreifen?** angezeigt.

  ![][002]

Durch Klicken auf diesen Link wird das Self-Service-Portal für die Kennwortzurücksetzung geöffnet.

  ![][003]

Weitere Informationen dazu, wie Benutzer ihre eigenen Kennwörter zurücksetzen können, finden Sie unter [Erste Schritte mit der Kennwortverwaltung](active-directory-passwords-getting-started.md).

## <a name="user-password-change-portal"></a>Portal zum Ändern von Benutzerkennwörtern
Wenn Ihre Benutzer ihre eigenen Kennwörter ändern möchten, können sie hierzu jederzeit das Portal zum Ändern von Benutzerkennwörtern verwenden. Benutzer können über die Profilseite im Zugriffsbereich auf das Portal zur Kennwortänderung zugreifen oder in einer Office 365-Anwendung auf den Link **Kennwort ändern** klicken. Falls ein Kennwort abläuft, werden Benutzer bei der Anmeldung automatisch aufgefordert, ihr Kennwort zu ändern.

  ![][004]

In beiden Fällen werden die geänderten Kennwörter – sofern das Kennwortrückschreiben aktiviert ist und der Benutzer entweder ein Verbundkennwort oder ein synchronisiertes Kennwort verwendet – in das lokale Active Directory zurückgeschrieben.

Das Portal zum Ändern von Benutzerkennwörtern sieht folgendermaßen aus:

  ![][005]

Weitere Informationen dazu, wie Benutzer ihre eigenen, lokalen Active Directory-Kennwörter zurücksetzen können, finden Sie unter [Erste Schritte mit der Kennwortverwaltung](active-directory-passwords-getting-started.md).

## <a name="password-management-reports"></a>Berichte zur Kennwortverwaltung
Auf der Registerkarte **Berichte** werden unter dem Abschnitt **Aktivitätsprotokolle** zwei Berichte zur Kennwortverwaltung angezeigt: **Aktivität „Zurücksetzen des Kennworts“** und **Aktivität „Registrierung für Zurücksetzen des Kennworts“**. Diese beiden Berichte liefern einen Überblick über Benutzer innerhalb Ihrer Organisation, die sich für das Zurücksetzen von Kennwörtern registrieren und die Kennwortzurücksetzung verwenden.

Im [Azure-Portal](https://manage.windowsazure.com) sehen diese Berichte wie folgt aus:

  ![][006]

Weitere Informationen finden Sie unter [Operative Einblicke durch Berichte zur Kennwortverwaltung](active-directory-passwords-get-insights.md).

## <a name="password-writeback-component-of-azure-ad-connect"></a>Komponente zur Kennwortrückschreibung von Azure AD Connect
Wenn die Kennwörter von Benutzern in Ihrer Organisation aus Ihrer lokalen Umgebung stammen (Verbund oder Kennwortsynchronisierung), können Sie die aktuelle Version von Azure AD Connect installieren, um diese Kennwörter direkt aus der Cloud zu aktualisieren. Das bedeutet, dass Benutzer ihr Azure AD-Kennwort direkt über das Web zurücksetzen oder ändern können, wenn sie es vergessen haben oder ändern möchten. Die Kennwortrückschreibung wird an dieser Stelle im Installations-Assistenten von Azure AD Connect aktiviert:

  ![][007]

Weitere Informationen zu Azure AD Connect finden Sie unter [Herstellen einer Verbindung zwischen Active Directory und Azure Active Directory](active-directory-aadconnect.md). Weitere Informationen zum Kennwortrückschreiben finden Sie unter [Erste Schritte mit der Kennwortverwaltung](active-directory-passwords-getting-started.md).


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie auf den folgenden Seiten zur Azure AD-Kennwortzurücksetzung:

* **Sind Sie hier, weil Sie Probleme bei der Anmeldung haben?** Falls ja, erfahren Sie [hier](active-directory-passwords-update-your-own-password.md#reset-your-password), wie Sie Ihr eigenes Kennwort ändern und zurücksetzen.
* [**Erste Schritte:**](active-directory-passwords-getting-started.md) Hier erfahren Sie, wie Sie Benutzern das Zurücksetzen und Ändern ihrer cloudbasierten oder lokalen Kennwörter ermöglichen.
* [**Anpassen:**](active-directory-passwords-customize.md) Hier erfahren Sie, wie Sie das Erscheinungsbild und das Verhalten des Diensts auf die Anforderungen Ihrer Organisation abstimmen.
* [**Best Practices:**](active-directory-passwords-best-practices.md) Hier erfahren Sie, wie Sie Kennwörter in Ihrer Organisation schnell bereitstellen und effektiv verwalten.
* [**Einblicke erhalten:**](active-directory-passwords-get-insights.md) Hier erfahren Sie mehr über unsere integrierten Berichtsfunktionen.
* [**Häufig gestellte Fragen:**](active-directory-passwords-faq.md) Hier erhalten Sie Antworten auf häufig gestellte Fragen.
* [**Problembehandlung:**](active-directory-passwords-troubleshoot.md) Hier erfahren Sie, wie Sie Probleme mit dem Dienst schnell beheben.
* [**Weitere Informationen:**](active-directory-passwords-learn-more.md) Hier erhalten Sie detaillierte technische Informationen zur Funktionsweise des Diensts.

[001]: ./media/active-directory-passwords-how-it-works/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-how-it-works/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-how-it-works/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-how-it-works/004.jpg "Image_004.jpg"
[005]: ./media/active-directory-passwords-how-it-works/005.jpg "Image_005.jpg"
[006]: ./media/active-directory-passwords-how-it-works/006.jpg "Image_006.jpg"
[007]: ./media/active-directory-passwords-how-it-works/007.jpg "Image_007.jpg"

