---
title: "Azure MFA-Anmeldevorgang mit Überprüfung in zwei Schritten | Microsoft-Dokumentation"
description: "Auf dieser Seite finden Sie eine Anleitung zu den verschiedenen Anmeldemethoden, die mit Azure MFA verfügbar sind."
keywords: "Benutzerauthentifizierung, Anmeldevorgang, Anmelden mit dem Mobiltelefon, Anmelden mit dem Bürotelefon"
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: pblachar
ms.assetid: b310b762-471b-4b26-887a-a321c9e81d46
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/02/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: c0af680c83a72a9f2a415999141f9913c8b724a4
ms.openlocfilehash: 85e0212ed9d7bde001f9b5cd4776d34ad1f01816


---
# <a name="the-sign-in-experience-with-azure-multi-factor-authentication"></a>Der Anmeldevorgang mit Azure Multi-Factor Authentication
> [!NOTE]
> Zweck dieses Artikels ist eine exemplarische Vorgehensweise für einen typischen Anmeldevorgang. Hilfe zur Anmeldung oder zur Behebung von Problemen finden Sie unter [Beheben von Problemen mit Azure Multi-Factor Authentication](multi-factor-authentication-end-user-troubleshoot.md).

## <a name="what-will-your-sign-in-experience-be"></a>Was sieht Ihr Anmeldevorgang aus?
Der Anmeldevorgang hängt davon ab, wie Sie sich anmelden und Multi-Factor Authentication verwenden. Wählen Sie die Option aus, die am besten auf Sie zutrifft:

| Wie melden Sie sich an? | 
| --- |
| [Mit einem Mobil- oder Bürotelefon](#signing-in-with-mobile-or-office-phone) |
| [Mit Benachrichtigungen aus der Microsoft Authenticator-App](#signing-in-with-the-microsoft-authenticator-app-using-notification) |
| [Mit Bestätigungscodes aus der Microsoft Authenticator-App](#signing-in-with-the-microsoft-authenticator-app-using-verification-code) |
| [Mit einer alternativen Methode, da ich meine bevorzugte Methode derzeit nicht verwenden kann](#signing-in-with-an-alternate-method) |

## <a name="signing-in-with-mobile-or-office-phone"></a>Anmeldung mit Mobil- oder Bürotelefon
Im Folgenden wird der Vorgang für Multi-Factor Authentication mit dem Büro- oder Mobiltelefon beschrieben.

1. Melden Sie sich mit Ihrem Benutzernamen und Kennwort bei einer Anwendung oder einem Dienst wie Office 365 an.  
2. Sie erhalten einen Anruf von Microsoft.  
3. Nehmen Sie den Anruf entgegen, und drücken Sie die #-Taste.  
4. Sie sollten jetzt angemeldet sein.  

## <a name="signing-in-with-the-microsoft-authenticator-app"></a>Anmelden mit der Microsoft Authenticator-App 
Nachstehend wird der Anmeldevorgang mit der Microsoft Authenticator-App für Überprüfungen in zwei Schritten beschrieben. Es gibt zwei Möglichkeiten, die App zu verwenden. Sie können entweder Pushbenachrichtigungen auf Ihrem Gerät erhalten oder die App öffnen, um einen Prüfcode abzurufen.

### <a name="to-sign-in-with-a-notification-from-the-microsoft-authenticator-app"></a>So melden Sie sich mit einer von der Microsoft Authenticator-App gesendeten Benachrichtigung an
1. Melden Sie sich mit Ihrem Benutzernamen und Kennwort bei einer Anwendung oder einem Dienst wie Office 365 an.
2. Microsoft sendet eine Benachrichtigung an die Microsoft Authenticator-App auf Ihrem Gerät.

  ![Microsoft sendet Benachrichtigung](./media/multi-factor-authentication-end-user-signin/notify.png)

3. Öffnen Sie die Benachrichtigung auf Ihrem Telefon, und wählen den Schlüssel **Überprüfen** aus. Wenn Ihr Unternehmen eine PIN anfordert, geben Sie sie hier ein.
4. Sie sollten jetzt angemeldet sein.

### <a name="to-sign-in-using-a-verification-code-with-the-microsoft-authenticator-app"></a>So melden Sie sich mit der Microsoft Authenticator-App über einen Überprüfungscode an

Wenn Sie die Microsoft-Authenticator-App zum Abrufen von Prüfcodes verwenden, wird Ihnen beim Öffnen der App unter dem Namen Ihres Kontos eine Zahl angezeigt. Diese Zahl ändert sich alle&30; Sekunden, damit Sie die gleiche Zahl nicht zweimal verwenden. Wenn ein Prüfcode angefordert wird, öffnen Sie die App und verwenden die derzeit angezeigte Zahl. 

1. Melden Sie sich mit Ihrem Benutzernamen und Kennwort bei einer Anwendung oder einem Dienst wie Office 365 an.
2. Sie werden von Microsoft aufgefordert, einen Bestätigungscode einzugeben.

  ![Eingeben des Überprüfungscodes](./media/multi-factor-authentication-end-user-signin/verify3.png)

3. Öffnen Sie die Microsoft Authenticator-App auf Ihrem Smartphone, und geben Sie den Code bei der Anmeldung in das entsprechende Feld ein.
4. Sie sollten jetzt angemeldet sein.

## <a name="signing-in-with-an-alternate-method"></a>Anmelden mit einer alternativen Methode
Mitunter verfügen Sie nicht über das Telefon oder Gerät, das Sie als Ihre bevorzugte Überprüfungsmethode festgelegt haben. Genau aus diesem Grund empfehlen wird, alternative Anmeldemethoden für Ihr Konto einzurichten. Im folgenden Abschnitt erfahren Sie, wie Sie sich über eine alternative Methode anmelden, wenn die primäre Methode nicht verfügbar ist.

1. Melden Sie sich mit Ihrem Benutzernamen und Kennwort bei einer Anwendung oder einem Dienst wie Office 365 an.
2. Wählen Sie **Andere Überprüfungsoption verwenden** aus. Es werden verschiedene Überprüfungsoptionen abhängig davon angezeigt, wie viele Sie eingerichtet haben.

  ![Anwenden einer alternativen Methode](./media/multi-factor-authentication-end-user-signin/alt.png)

3. Wählen Sie eine alternative Methode aus, und melden Sie sich an.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie Probleme bei der Anmeldung mit der Überprüfung in zwei Schritten haben, finden Sie unter [Probleme mit Azure Multi-Factor Authentication](multi-factor-authentication-end-user-troubleshoot.md) weitere Informationen.

Lesen Sie den Artikel [Verwalten der Einstellungen für die Überprüfung in zwei Schritten](multi-factor-authentication-end-user-manage-settings.md).

In [Erste Schritte mit der Microsoft Authenticator-App](microsoft-authenticator-app-how-to.md) erfahren Sie, wie Sie sich mithilfe von Benachrichtigungen anstelle von SMS oder Telefonanrufen anmelden können. 


<!--HONumber=Feb17_HO1-->


