---
title: 'Azure AD Connect: Problembehandlung beim nahtlosen einmaligen Anmelden | Microsoft-Dokumentation'
description: "In diesem Thema wird beschrieben, wie Sie Probleme beim nahtlosen einmaligen Anmelden in Azure AD (nahtlose SSO in Azure AD) behandeln können."
services: active-directory
keywords: "Was ist Azure AD Connect, Active Directory installieren, erforderliche Komponenten für Azure AD, SSO, Single Sign-On, einmaliges Anmelden"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: billmath
ms.translationtype: Human Translation
ms.sourcegitcommit: 18d4994f303a11e9ce2d07bc1124aaedf570fc82
ms.openlocfilehash: a543be452abbbe3057a5e275612968cd52c8b7aa
ms.contentlocale: de-de
ms.lasthandoff: 05/09/2017

---

# <a name="how-to-troubleshoot-azure-active-directory-seamless-single-sign-on"></a>Problembehandlung beim nahtlosen einmaligen Anmeldung mit Azure Active Directory

## <a name="known-issues"></a>Bekannte Probleme

- Wenn Sie mehr als 30 AD-Gesamtstrukturen mit Azure AD Connect synchronisieren, funktioniert der für das Einrichten des nahtlosen SSO verwendete Assistent nicht richtig. Zur Problembehebung können Sie das nahtlose SSO auf Ihrem Mandanten [manuell aktivieren](#manual-reset-of-azure-ad-seamless-sso).
- Fügen Sie Dienst-URLs von Azure AD (https://autologon.microsoftazuread-sso.com, https://aadg.windows.net.nsatc.net) zu der Zone „Vertrauenswürdige Seiten“ statt zur Zone „Lokals Intranet“ hinzu.

## <a name="troubleshooting-checklist"></a>Checkliste zur Problembehandlung

Verwenden Sie die folgende Checkliste zur Behebung von Problemen in Bezug auf die nahtlose einmalige Anmeldung in Azure AD:

1. Überprüfen Sie, ob die Funktion zur nahtlosen einmaligen Anmeldung für Ihren Mandanten im Azure AD Connect-Tool aktiviert ist. Wenn Sie die Funktion (z.B. aufgrund eines blockierten Ports) nicht aktivieren können, vergewissern Sie sich, dass Sie alle [Voraussetzungen](active-directory-aadconnect-sso.md#pre-requisites) erfüllen. Wenn die Probleme in Verbindung mit der Aktivierung der Funktion weiterhin bestehen, wenden Sie sich an den Microsoft-Support.
2. Beide Dienst-URLs („https://autologon.microsoftazuread-sso.com“ und „https://aadg.windows.net.nsatc.net“) sind als Teil der Intranetzoneneinstellungen definiert.
3. Stellen Sie sicher, dass der Unternehmensdesktop mit der AD-Domäne verknüpft ist.
4. Stellen Sie sicher, dass der Benutzer mit einem AD-Domänenkonto beim Desktop angemeldet ist.
5. Stellen Sie sicher, dass das Benutzerkonto aus einer AD-Gesamtstruktur stammt, in der das nahtlose einmalige Anmelden eingerichtet wurde.
6. Stellen Sie sicher, dass der Desktop mit dem Unternehmensnetzwerk verbunden ist.
7. Stellen Sie sicher, dass die Uhrzeit des Desktops mit der Uhrzeit von Active Directory und der der Domänencontroller synchronisiert ist und diese nicht mehr als 5 Minuten voneinander abweichen.
8. Bereinigen Sie Ihren Desktop von vorhandenen Kerberos-Tickets. Dies lässt sich durch Ausführung des Befehls **klist-Bereinigung** über eine Eingabeaufforderung erreichen. Die Kerberos-Tickets von Benutzern sind normalerweise 12 Stunden gültig. Beachten Sie, dass Sie dies möglicherweise anders in AD eingerichtet haben.
9. Überprüfen Sie die Protokolle der Konsole des Browsers (unter „Entwicklertools“), um potenzielle Probleme zu ermitteln.
10. Schauen Sie sich außerdem die [Domänencontrollerprotokolle](#domain-controller-logs) an.

### <a name="domain-controller-logs"></a>Domänencontrollerprotokolle

Wenn die erfolgreiche Überwachung auf Ihrem Domänencontroller aktiviert ist, wird bei jeder nahtlosen einmaligen Anmeldung durch einen Benutzer ein Sicherheitseintrag (das dem Computerkonto **AzureADSSOAcc$** zugeordnete Ereignis 4769) im Ereignisprotokoll erfasst. Sie können diese Sicherheitsereignisse mithilfe der folgenden Abfrage abrufen:

```
    <QueryList>
      <Query Id="0" Path="Security">
    <Select Path="Security">*[EventData[Data[@Name='ServiceName'] and (Data='AZUREADSSOACC$')]]</Select>
      </Query>
    </QueryList>
```

## <a name="manual-reset-of-azure-ad-seamless-sso"></a>Manuelles Zurücksetzen vom nahtlosen SSO in Azure AD

Wenn die Problembehandlung nicht hilft, verwenden Sie die folgenden Schritte, um die Funktion auf Ihrem Mandanten manuell zurückzusetzen bzw. zu aktivieren:

### <a name="1-import-the-seamless-sso-powershell-module"></a>1. Importieren Sie das PowerShell-Modul „Nahtlose SSO“

- Laden Sie zuerst den [Microsoft Online Services-Anmelde-Assistenten](http://go.microsoft.com/fwlink/?LinkID=286152)herunter, und installieren Sie ihn.
- Laden Sie anschließend das [Azure Active Directory-Modul für Windows PowerShell (64 Bit)](http://go.microsoft.com/fwlink/p/?linkid=236297)herunter, und installieren Sie es.
- Navigieren Sie zum Ordner `%programfiles%\Microsoft Azure Active Directory Connect`.
- Importieren Sie das PowerShell-Modul „Nahtlose SSO“ mit folgendem Befehl: `Import-Module .\AzureADSSO.psd1`.

### <a name="2-get-the-list-of-ad-forests-on-which-seamless-sso-has-been-enabled"></a>2. Rufen Sie die Liste der AD-Gesamtstrukturen ab, für die nahtlose einmaliges Anmelden aktiviert wurde.

- Rufen Sie `New-AzureADSSOAuthenticationContext` in PowerShell auf. Ein Fenster sollte eingeblendet werden, in dem Sie Ihre Anmeldeinformationen als Azure AD-Mandantenadministrator eingeben.
- Rufen Sie `Get-AzureADSSOStatus` auf. Dadurch erhalten Sie die Liste der AD-Gesamtstrukturen (sehen Sie sich die Liste „Domänen“ an), für die dieses Feature aktiviert ist.

### <a name="3-disable-seamless-sso-for-each-ad-forest-that-it-was-set-it-up-on"></a>3. Deaktivieren Sie die nahtlose SSO für jede AD-Gesamtstruktur, auf der sie eingerichtet wurde.

- Rufen Sie `New-AzureADSSOAuthenticationContext` in PowerShell auf. Ein Fenster sollte eingeblendet werden, in dem Sie Ihre Anmeldeinformationen als Azure AD-Mandantenadministrator eingeben.
- Rufen Sie `$creds = Get-Credential` auf. Ein Fenster sollte eingeblendet werden, in dem Sie die Anmeldeinformationen des Domänenadministrators für die vorgesehene AD-Gesamtstruktur eingeben können.
- Rufen Sie `Disable-AzureADSSOForest -OnPremCredentials $creds` auf. Dadurch wird sowohl das Computerkonto „AZUREADSSOACCT“ vom lokalen Domänencontroller entfernt als auch das Feature für diese spezifische AD-Gesamtstruktur deaktiviert.
- Wiederholen Sie die oben stehenden Schritte für jeden AD-Gesamtstruktur, für die Sie das Feature eingerichtet haben.

### <a name="4-enable-seamless-sso-for-each-ad-forest"></a>4. Aktivieren der nahtlosen SSO für jede AD-Gesamtstruktur

- Rufen Sie `New-AzureADSSOAuthenticationContext` auf. Ein Fenster sollte eingeblendet werden, in dem Sie Ihre Anmeldeinformationen als Azure AD-Mandantenadministrator eingeben.
- Rufen Sie `Enable-AzureADSSOForest` auf. Ein Fenster sollte eingeblendet werden, in dem Sie die Anmeldeinformationen des Domänenadministrators für die vorgesehene AD-Gesamtstruktur eingeben können.
- Wiederholen Sie die oben stehenden Schritte für jeden AD-Gesamtstruktur, für die Sie das Feature einrichten möchten.

### <a name="5-enable-seamless-sso-on-your-tenant"></a>5. Aktivieren der nahtlosen SSO für Ihren Mandanten

- Rufen Sie `New-AzureADSSOAuthenticationContext` auf. Ein Fenster sollte eingeblendet werden, in dem Sie Ihre Anmeldeinformationen als Azure AD-Mandantenadministrator eingeben.
- Rufen Sie `Enable-AzureADSSO` auf, und geben Sie „TRUE“ bei der Aufforderung `Enable: ` ein, um das Feature in Ihrem Mandanten zu aktivieren.

