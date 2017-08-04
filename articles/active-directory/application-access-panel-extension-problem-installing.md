---
title: "Problem beim Installieren der Browsererweiterung für den Anwendungszugriffsbereich | Microsoft-Dokumentation"
description: "Korrigieren von Fehlern, die beim Installieren der Browsererweiterung für den Anwendungszugriffsbereich häufig auftreten"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.reviewer: japere
ms.translationtype: HT
ms.sourcegitcommit: 54454e98a2c37736407bdac953fdfe74e9e24d37
ms.openlocfilehash: 8b7327508633e33917d1fa9c1f35ed1bde5a26e1
ms.contentlocale: de-de
ms.lasthandoff: 07/13/2017

---

# <a name="problem-installing-the-application-access-panel-browser-extension"></a>Problem beim Installieren der Browsererweiterung für den Anwendungszugriffsbereich

Der Zugriffsbereich ist ein webbasiertes Portal, mit dem Benutzer, die in Azure Active Directory (Azure AD) über ein Geschäfts-, Schul- oder Unikonto verfügen, cloudbasierte Anwendungen anzeigen und starten können, für die der Azure AD-Administrator ihnen Zugriff gewährt hat. Ein Benutzer, der über Azure AD-Editionen verfügt, kann über den Zugriffsbereich auch die Funktionen für die Self-Service-basierte Gruppen- und App-Verwaltung nutzen. Der Zugriffsbereich ist unabhängig vom Azure-Portal und setzt nicht voraus, dass Benutzer über ein Azure-Abonnement verfügen.

Für die Verwendung des einmaligen Anmeldens per Kennwort im Anwendungsbereich muss die Erweiterung für den Zugriffsbereich im Browser des Benutzers installiert sein. Diese Erweiterung wird automatisch heruntergeladen, wenn ein Benutzer eine Anwendung auswählt, die für kennwortbasiertes SSO konfiguriert ist.

## <a name="meeting-browser-requirements-for-the-access-panel"></a>Erfüllen der Browseranforderungen für den Zugriffsbereich

Der Zugriffsbereich erfordert einen Browser, der JavaScript unterstützt und in dem CSS aktiviert ist. Für die Verwendung des einmaligen Anmeldens per Kennwort im Anwendungsbereich muss die Erweiterung für den Zugriffsbereich im Browser des Benutzers installiert sein. Diese Erweiterung wird automatisch heruntergeladen, wenn ein Benutzer eine Anwendung auswählt, die für kennwortbasiertes SSO konfiguriert ist.

Für kennwortbasiertes SSO eignen sich folgende Browser des Endbenutzers:

-   Internet Explorer 8, 9, 10, 11 – unter Windows 7 oder höher

-   Edge in Windows 10 Anniversary Edition oder höher 

-   Chrome – unter Windows 7 oder höher und MacOS x oder höher

-   Firefox 26.0 oder höher – unter Windows XP SP2 oder höher und Mac OS X 10.6 oder höher

## <a name="how-to-install-the-access-panel-browser-extension"></a>Installieren der Browsererweiterung für den Zugriffsbereich

Führen Sie zum Installieren der Browsererweiterung für den Zugriffsbereich die folgenden Schritte aus:

1.  Öffnen Sie in einem der unterstützten Browser den [Zugriffsbereich](https://myapps.microsoft.com), und melden Sie sich in Azure AD als **Benutzer** an.

2.  Klicken Sie im Zugriffsbereich auf eine **für kennwortbasiertes SSO konfigurierte Anwendung**.

3.  Wählen Sie in der Aufforderung zum Installieren der Software **Jetzt installieren** aus.

4.  Sie werden basierend auf Ihrem Browser zum Downloadlink weitergeleitet. **Fügen** Sie die Erweiterung Ihrem Browser hinzu.

5.  Wenn Sie im Browser zur Auswahl aufgefordert werden, wählen Sie die Option zum **Aktivieren** oder **Zulassen** der Erweiterung aus.

6.  Führen Sie nach der Installation einen **Neustart** Ihrer Browsersitzung durch.

7.  Melden Sie sich beim Zugriffsbereich an, und überprüfen Sie, ob Sie die für kennwortbasiertes SSO konfigurierten Anwendungen **starten** können.

Sie können die Erweiterung für Chrome und Edge auch direkt über die folgenden Links herunterladen:

-   [Zugriffsbereichserweiterung für Chrome](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Zugriffsbereichserweiterung für Edge](https://www.microsoft.com/store/apps/9pc9sckkzk84) 

## <a name="setting-up-a-group-policy-for-internet-explorer"></a>Einrichten einer Gruppenrichtlinie für Internet Explorer

Sie können eine Gruppenrichtlinie einrichten, um die Zugriffsbereichserweiterung für Internet Explorer per Remotezugriff auf den Computern Ihrer Benutzer zu installieren.

Die folgenden Voraussetzungen müssen erfüllt sein:

-   Sie haben [Active Directory-Domänendienste](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx)eingerichtet und die Computer Ihrer Benutzer Ihrer Domäne hinzugefügt.

-   Zum Bearbeiten des Gruppenrichtlinienobjekts benötigen Sie die Berechtigung „Einstellungen bearbeiten“. Standardmäßig verfügen Mitglieder der folgenden Sicherheitsgruppen über diese Berechtigung: Domänenadministratoren, Organisationsadministratoren und Richtlinien-Ersteller-Besitzer. [Weitere Informationen](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).

Das Tutorial [Bereitstellen der Zugriffsbereichserweiterung für Internet Explorer mit der Gruppenrichtlinie](active-directory-saas-ie-group-policy.md) bietet Ihnen Schritt-für-Schritt-Anweisungen zum Konfigurieren der Gruppenrichtlinie und Bereitstellen der Gruppenrichtlinie für Benutzer.

## <a name="troubleshoot-the-access-panel-in-internet-explorer"></a>Problembehandlung für den Zugriffsbereich in Internet Explorer

Befolgen Sie die Anleitung [Problembehandlung in der Zugriffsbereichserweiterung für Internet Explorer](active-directory-saas-ie-troubleshooting.md), um Zugriff auf ein Diagnosetool und Schritt-für-Schritt-Anweisungen zum Konfigurieren der Erweiterung für Internet Explorer zu erhalten.

## <a name="if-these-troubleshooting-steps-do-not-resolve-the-issue"></a>Wenn das Problem mit diesen Problembehandlungsschritten nicht behoben wird

Öffnen Sie ein Supportticket mit den folgenden Informationen, sofern verfügbar:

-   Fehlerkorrelations-ID

-   UPN (E-Mail-Adresse des Benutzers)

-   Mandanten-ID

-   Browsertyp

-   Zeitzone und Uhrzeit/Zeitraum des Auftretens des Fehlers

-   Fiddler-Ablaufverfolgungen

## <a name="next-steps"></a>Nächste Schritte
[Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

