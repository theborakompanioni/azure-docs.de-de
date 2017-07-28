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
ms.date: 07/12/2017
ms.author: billmath
ms.translationtype: Human Translation
ms.sourcegitcommit: ef1e603ea7759af76db595d95171cdbe1c995598
ms.openlocfilehash: 4466a5aa1d55b178a584832d03f68d307767d167
ms.contentlocale: de-de
ms.lasthandoff: 06/16/2017

---

# <a name="troubleshoot-azure-active-directory-seamless-single-sign-on"></a>Problembehandlung beim nahtlosen einmaligen Anmelden mit Azure Active Directory

In diesem Artikel finden Sie Informationen zur Problembehandlung bei bekannten Problemen mit der nahtlosen einmaligen Anmeldung in Azure AD.

## <a name="known-issues"></a>Bekannte Probleme

- Wenn Sie 30 und mehr AD-Gesamtstrukturen synchronisieren, kann die nahtlose einmalige Anmeldung nicht mit Azure AD Connect aktiviert werden. Zur Problembehebung können Sie die Funktion auf Ihrem Mandanten [manuell aktivieren](#manual-reset-of-azure-ad-seamless-sso).
- Wenn Sie Dienst-URLs von Azure AD (https://autologon.microsoftazuread-sso.com, https://aadg.windows.net.nsatc.net) statt zur Zone „Lokales Intranet“ zur Zone „Vertrauenswürdige Sites“ hinzufügen, können sich Benutzer nicht anmelden.
- Das nahtlose einmalige Anmelden funktioniert in Firefox nicht im privaten Modus.

## <a name="sign-in-failure-reasons-on-the-azure-active-directory-admin-center"></a>Gründe für Anmeldefehler im Azure Active Directory Admin Center

Sie sollten die Problembehandlung von benutzerseitigen Anmeldefehlern mit der nahtlosen einmaligen Anmeldung in den [Berichten zu Anmeldeaktivitäten](../active-directory-reporting-activity-sign-ins.md) im [Azure Active Directory Admin Center](https://aad.portal.azure.com/) beginnen.

![Bericht zu Anmeldeaktivitäten](./media/active-directory-aadconnect-sso/sso9.png)

Navigieren Sie im [Azure Active Directory Admin Center](https://aad.portal.azure.com/) zu **Azure Active Directory** -> **Anmeldungen**, und klicken Sie auf die Anmeldeaktivität eines bestimmten Benutzers. Suchen Sie nach dem Feld **Code des Anmeldefehlers**. Ordnen Sie den Wert in diesem Feld mithilfe der folgenden Tabelle einer Ursache und einer Lösung zu:

|Anmeldefehler|Grund des Anmeldefehlers|Lösung
| --- | --- | ---
| 81001 | Das Kerberos-Ticket des Benutzers ist zu groß. | Reduzieren Sie die Gruppenmitgliedschaften des Benutzers aus, und versuchen Sie es erneut.
| 81002 | Das Kerberos-Ticket des Benutzers kann nicht überprüft werden. | Informationen hierzu finden Sie in der [Checkliste zur Problembehandlung](#troubleshooting-checklist).
| 81003 | Das Kerberos-Ticket des Benutzers kann nicht überprüft werden. | Informationen hierzu finden Sie in der [Checkliste zur Problembehandlung](#troubleshooting-checklist).
| 81004 | Fehler beim Versuch, die Kerberos-Authentifizierung durchzuführen. | Informationen hierzu finden Sie in der [Checkliste zur Problembehandlung](#troubleshooting-checklist).
| 81008 | Das Kerberos-Ticket des Benutzers kann nicht überprüft werden. | Informationen hierzu finden Sie in der [Checkliste zur Problembehandlung](#troubleshooting-checklist).
| 81009 | Das Kerberos-Ticket des Benutzers kann nicht überprüft werden. | Informationen hierzu finden Sie in der [Checkliste zur Problembehandlung](#troubleshooting-checklist).
| 81010 | Fehler beim nahtlosen einmaligen Anmelden, da das Kerberos-Ticket des Benutzers abgelaufen oder ungültig ist. | Der Benutzer muss sich über ein in eine Domäne eingebundenes Gerät in Ihrem Unternehmensnetzwerk anmelden.
| 81011 | Das Benutzerobjekt konnte anhand der Informationen im Kerberos-Ticket des Benutzers nicht gefunden werden. | Verwenden Sie Azure AD Connect, um Benutzerinformationen in Azure AD zu synchronisieren.
| 81012 | Der Benutzer, der versucht, sich an Azure AD anzumelden, unterscheidet sich von dem Benutzer, der am Gerät angemeldet ist. | Melden Sie sich über ein anderes Gerät an.
| 81013 | Das Benutzerobjekt konnte anhand der Informationen im Kerberos-Ticket des Benutzers nicht gefunden werden. |Verwenden Sie Azure AD Connect, um Benutzerinformationen in Azure AD zu synchronisieren. 

## <a name="troubleshooting-checklist"></a>Checkliste zur Problembehandlung

Verwenden Sie die folgende Checkliste zur Behebung von Problemen in Bezug auf die nahtlose einmalige Anmeldung:

- Überprüfen Sie, ob die Funktion zur nahtlosen einmaligen Anmeldung in Azure AD Connect aktiviert ist. Wenn Sie die Funktion (z.B. aufgrund eines blockierten Ports) nicht aktivieren können, vergewissern Sie sich, dass Sie alle [Voraussetzungen](active-directory-aadconnect-sso-quick-start.md#step-1-check-prerequisites) erfüllen.
- Prüfen Sie, ob beide Dienst-URLs von Azure AD (https://autologon.microsoftazuread-sso.com und https://aadg.windows.net.nsatc.net) Teil der Intranetzoneneinstellungen des Benutzers sind.
- Stellen Sie sicher, dass das Unternehmensgerät mit der AD-Domäne verknüpft ist.
- Stellen Sie sicher, dass der Benutzer mit einem AD-Domänenkonto beim Gerät angemeldet ist.
- Stellen Sie sicher, dass das Benutzerkonto aus einer AD-Gesamtstruktur stammt, in der das nahtlose einmalige Anmelden eingerichtet wurde.
- Stellen Sie sicher, dass das Gerät mit dem Unternehmensnetzwerk verbunden ist.
- Stellen Sie sicher, dass die Uhrzeit des Geräts mit der Uhrzeit von Active Directory und der des Domänencontrollers synchronisiert ist und diese nicht mehr als fünf Minuten voneinander abweichen.
- Listen Sie vorhandene Kerberos-Tickets auf dem Gerät mit dem Befehl **klist** über eine Eingabeaufforderung auf. Prüfen Sie, ob die für das Computerkonto `AZUREADSSOACCT` ausgestellten Tickets vorhanden sind. Die Kerberos-Tickets von Benutzern sind normalerweise 12 Stunden gültig. Sie haben diese Einstellung in Active Directory möglicherweise anders festgelegt.
- Löschen Sie vorhandene Kerberos-Tickets auf dem Gerät mit dem Befehl **klist purge**, und wiederholen Sie den Vorgang.
- Überprüfen Sie die Konsolenprotokolle des Browsers (unter „Entwicklertools“), um zu ermitteln, ob Probleme vorliegen, die sich auf JavaScript beziehen.
- Schauen Sie sich außerdem die [Domänencontrollerprotokolle](#domain-controller-logs) an.

### <a name="domain-controller-logs"></a>Domänencontrollerprotokolle

Wenn die erfolgreiche Überwachung auf Ihrem Domänencontroller aktiviert ist, wird bei jeder nahtlosen einmaligen Anmeldung durch einen Benutzer ein Sicherheitseintrag im Ereignisprotokoll erfasst. Sie finden diese Sicherheitsereignisse mithilfe der folgenden Abfrage (suchen Sie nach Ereignis **4769** in Verbindung mit dem Computerkonto **AzureADSSOAcc$**):

```
    <QueryList>
      <Query Id="0" Path="Security">
    <Select Path="Security">*[EventData[Data[@Name='ServiceName'] and (Data='AZUREADSSOACC$')]]</Select>
      </Query>
    </QueryList>
```

## <a name="manual-reset-of-azure-ad-seamless-sso"></a>Manuelles Zurücksetzen vom nahtlosen SSO in Azure AD

Wenn die Problembehandlung nicht hilft, verwenden Sie die folgenden Schritte, um die Funktion auf Ihrem Mandanten manuell zurückzusetzen:

### <a name="step-1-import-the-seamless-sso-powershell-module"></a>Schritt 1: Importieren Sie das PowerShell-Modul „Nahtlose SSO“

1. Laden Sie zuerst den [Microsoft Online Services-Anmelde-Assistenten](http://go.microsoft.com/fwlink/?LinkID=286152) herunter, und installieren Sie ihn.
2. Laden Sie anschließend das [Azure Active Directory-Modul für Windows PowerShell (64 Bit)](http://go.microsoft.com/fwlink/p/?linkid=236297)herunter, und installieren Sie es.
3. Navigieren Sie zum Ordner `%programfiles%\Microsoft Azure Active Directory Connect`.
4. Importieren Sie das PowerShell-Modul „Nahtlose SSO“ mit folgendem Befehl: `Import-Module .\AzureADSSO.psd1`.

### <a name="step-2-get-the-list-of-ad-forests-on-which-seamless-sso-has-been-enabled"></a>Schritt 2: Rufen Sie die Liste der AD-Gesamtstrukturen ab, für die nahtlose einmaliges Anmelden aktiviert wurde.

1. Rufen Sie in PowerShell `New-AzureADSSOAuthenticationContext` auf. Wenn Sie dazu aufgefordert werden, geben Sie die Anmeldeinformationen des Administratorkontos Ihres Azure AD-Mandanten ein.
2. Rufen Sie `Get-AzureADSSOStatus` auf. Dadurch erhalten Sie die Liste der AD-Gesamtstrukturen (siehe die Liste „Domänen“), in denen diese Funktion aktiviert ist.

### <a name="step-3-disable-seamless-sso-for-each-ad-forest-that-it-was-set-it-up-on"></a>Schritt 3: Deaktivieren Sie die nahtlose SSO für jede AD-Gesamtstruktur, auf der sie eingerichtet wurde.

1. Rufen Sie `$creds = Get-Credential` auf. Wenn Sie dazu aufgefordert werden, geben Sie die Anmeldeinformationen des Domänenadministrators für die vorgesehene AD-Gesamtstruktur ein.
2. Rufen Sie `Disable-AzureADSSOForest -OnPremCredentials $creds` auf. Mit diesem Befehl wird das Computerkonto `AZUREADSSOACCT` vom lokalen Domänencontroller für diese spezifische AD-Gesamtstruktur entfernt.
3. Wiederholen Sie die oben stehenden Schritte für jede AD-Gesamtstruktur, für die Sie das Feature eingerichtet haben.

### <a name="step-4-enable-seamless-sso-for-each-ad-forest"></a>Schritt 4: Aktivieren Sie die nahtlose SSO für jede AD-Gesamtstruktur.

1. Rufen Sie `Enable-AzureADSSOForest` auf. Wenn Sie dazu aufgefordert werden, geben Sie die Anmeldeinformationen des Domänenadministrators für die vorgesehene AD-Gesamtstruktur ein.
2. Wiederholen Sie die oben stehenden Schritte für jede AD-Gesamtstruktur, für die Sie das Feature eingerichtet haben.

### <a name="step-5-enable-the-feature-on-your-tenant"></a>Schritt 5: Aktivieren Sie das Feature für Ihren Mandanten.

Rufen Sie `Enable-AzureADSSO` auf, und geben Sie „TRUE“ bei der Aufforderung `Enable: ` ein, um das Feature in Ihrem Mandanten zu aktivieren.

