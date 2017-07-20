---
title: "Problembehandlung bei der Überprüfung in zwei Schritten | Microsoft Docs"
description: In diesem Dokument erhalten Benutzer Informationen zur Vorgehensweise, wenn bei Multi-Factor Authentication ein Problem auftreten sollte.
services: multi-factor-authentication
keywords: "Client für Multi-Factor Authentication, Authentifizierungsproblem, Korrelations-ID"
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 8f3aef42-7f66-4656-a7cd-d25a971cb9eb
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: end-user
ms.translationtype: Human Translation
ms.sourcegitcommit: 6adaf7026d455210db4d7ce6e7111d13c2b75374
ms.openlocfilehash: 10b63a859c67b965734e32ec979b26a1ea6cc516
ms.contentlocale: de-de
ms.lasthandoff: 06/22/2017

---
# <a name="having-trouble-with-two-step-verification"></a>Probleme bei der Überprüfung in zwei Schritten
Dieser Artikel erläutert einige Probleme, die bei der Überprüfung in zwei Schritten auftreten können. Wenn Ihr Problem hier nicht aufgeführt wird, hinterlassen Sie detailliertes Feedback im Abschnitt „Kommentare“, sodass wir den Artikel erweitern und verbessern können.

## <a name="i-lost-my-phone-or-it-was-stolen"></a>Ich habe mein Telefon verloren, oder es wurde mir gestohlen
Es gibt zwei Möglichkeiten, um wieder auf Ihr Konto zuzugreifen. Die erste besteht darin, die Nummer Ihres alternativen Authentifizierungstelefons zu verwenden, falls Sie eine solche eingerichtet haben. Die zweite besteht darin, Ihren Administrator zu bitten, Ihre Einstellungen zu löschen.

Wenn Ihr Telefon verloren gegangen ist oder gestohlen wurde, empfiehlt es sich auch, Ihren Administrator zu bitten, Ihre App-Kennwörter zurückzusetzen und alle gespeicherten Geräte zu löschen. Wenn der Administrator nicht sicher ist, wie dies funktioniert, empfehlen Sie diesen Artikel: [Verwalten von Benutzern und Geräten](../multi-factor-authentication-manage-users-and-devices.md).

### <a name="use-an-alternate-phone-number"></a>Verwenden einer alternativen Telefonnummer
Wenn Sie mehrere Überprüfungsoptionen eingerichtet haben, einschließlich einer sekundären Telefonnummer oder einer Authentifizierungs-App auf einem anderen Gerät, können Sie eine der Möglichkeiten nutzen, um sich anzumelden.

Um sich mithilfe der alternativen Telefonnummer anzumelden, führen Sie diese Schritte aus:

1. Melden Sie sich wie gewohnt an.
2. Wenn Sie aufgefordert werden, Ihr Konto zu verifizieren, wählen Sie **Andere Überprüfungsoption verwenden** aus.
   
    ![Andere Überprüfung](./media/multi-factor-authentication-end-user-manage/differentverification.png)
3. Wählen Sie die Telefonnummer aus, auf die zugreifen können.
   
    ![Alternatives Telefon](./media/multi-factor-authentication-end-user-manage/altphone2.png)
4. Wenn Sie wieder bei Ihrem Konto angemeldet sind, [verwalten Sie Ihre Einstellungen](multi-factor-authentication-end-user-manage-settings.md), um die Nummer Ihres Authentifizierungstelefons zu ändern.

> [!IMPORTANT]
> Konfigurieren Sie unbedingt eine sekundäre Authentifizierungstelefonnummer. Wenn sich Ihre primäre Telefonnummer und Ihre mobile App auf dem gleichen Telefon befinden, benötigen Sie eine dritte Option, falls Ihr Telefon verloren geht oder gestohlen wird.   

### <a name="clear-your-settings"></a>Löschen der Einstellungen
Wenn Sie keine sekundäre Telefonnummer zur Authentifizierung konfiguriert haben, müssen Sie sich an Ihren Administrator wenden. Der Administrator muss Ihre Einstellungen löschen, sodass Sie bei der nächsten Anmeldung erneut aufgefordert werden, [Ihr Konto einzurichten](multi-factor-authentication-end-user-first-time.md).

## <a name="i-am-not-receiving-a-text-or-call-on-my-phone"></a>Ich empfange keine SMS und keinen Anruf auf meinem Telefon
Es gibt verschiedene Gründe, aus denen Sie beim Anmeldeversuch keine SMS und keinen Telefonanruf erhalten. Wenn Sie zuvor bereits erfolgreich SMS oder Telefonanrufe auf Ihrem Telefon erhalten haben, liegt das Problem möglicherweise beim Telefonanbieter, nicht bei Ihrem Konto. Stellen Sie sicher, dass der Empfang gut ist, und wenn Sie versuchen, eine SMS zu erhalten, stellen Sie auch sicher, dass Ihr Telefontarif SMS-Nachrichten unterstützt.

Wenn Sie bereits mehrere Minuten auf eine SMS oder einen Anruf gewartet haben, ist der Versuch, eine andere Authentifizierungsoption zu verwenden, die schnellste Möglichkeit, sich beim Konto anzumelden.

1. Wählen Sie auf der Seite, die auf Ihre Überprüfung warten, den Eintrag **Andere Überprüfungsoption verwenden** aus.
   
    ![Andere Überprüfung](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)
2. Wählen Sie die Telefonnummer oder Übermittlungsmethode, die Sie verwenden möchten.
   
    Wenn Sie mehrere Überprüfungscodes erhalten haben, verwenden Sie den neuesten.

Wenn Sie keine andere Methode konfiguriert haben, wenden Sie sich an Ihren Administrator und bitten ihn, Ihre Einstellungen zu löschen. Bei der nächsten Anmeldung werden Sie erneut aufgefordert, die [Multi-Factor Authentication einzurichten](multi-factor-authentication-end-user-first-time.md).

Wenn Sie häufig Verzögerungen aufgrund schlechten Empfangs bemerken, empfiehlt sich die Verwendung der [Microsoft Authenticator-App](microsoft-authenticator-app-how-to.md) auf Ihrem Smartphone. Die App kann zufällige Sicherheitscodes generieren, die Sie zum Anmelden verwenden. Diese Codes erfordern weder mobilen Empfang noch eine Internetverbindung.

## <a name="app-passwords-are-not-working"></a>App-Kennwörter funktionieren nicht
Stellen Sie zunächst sicher, dass Sie das App-Kennwort richtig eingegeben haben. Wenn es weiterhin nicht funktioniert, versuchen Sie, sich anzumelden, und [erstellen Sie ein App-Kennwort](multi-factor-authentication-end-user-app-passwords.md).  Wenn dies immer noch nicht funktioniert, bitten Sie den Administrator, [Ihre vorhandenen App-Kennwörter zu löschen](../multi-factor-authentication-manage-users-and-devices.md), und erstellen Sie dann ein neues Kennwort.

## <a name="i-didnt-find-an-answer-to-my-problem"></a>Ich konnte keine Lösung für mein Problem finden
Wenn Sie diese Schritte zur Problembehandlung ausgeführt haben, aber weiterhin Probleme auftreten, wenden Sie sich an Ihren Administrator bzw. an die Person, die die Multi-Factor Authentication für Sie eingerichtet hat. Diese Personen sollten Ihnen helfen können.

Sie können auch eine Frage in den [Azure AD-Foren](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD) stellen oder [den Support kontaktieren](https://support.microsoft.com/contactus). Wir melden uns so schnell wie möglich bei Ihnen.

Wenn Sie sich an den Support wenden, halten Sie die folgenden Informationen bereit:

* **Benutzer-ID**: Mit welcher E-Mail-Adresse haben Sie versucht sich anzumelden?
* **Allgemeine Beschreibung des Fehlers** – Welche Fehlermeldung wurde Ihnen angezeigt?  Wenn keine Fehlermeldung angezeigt wird, beschreiben Sie das beobachtete unerwartete Verhalten so genau wie möglich.
* **Seite** – Auf welcher Seite haben Sie sich befunden, als der Fehler aufgetreten ist (einschließlich URL)?
* **ErrorCode** : Der Fehlercode, den Sie erhalten.
* **SessionId** : Die spezifische Sitzungs-ID, die Sie erhalten.
* **Korrelations-ID** : Der generierte Code der Korrelations-ID bei Anzeige des Fehlers.
* **Zeitstempel** : Genaues Datum samt Uhrzeit und Zeitzone des Auftretens des Fehlers.

Viele dieser Informationen finden Sie auf Ihrer Anmeldeseite. Wenn Sie Ihre Anmeldung nicht rechtzeitig bestätigen, wählen Sie **Details anzeigen** aus.

![Details zu Anmeldefehlern](./media/multi-factor-authentication-end-user-troubleshoot/view_details.png)

Diese Informationen helfen uns dabei, Ihr Problem so schnell wie möglich zu beheben.

## <a name="related-topics"></a>Verwandte Themen
* [Verwalten der Einstellungen für die Überprüfung in zwei Schritten](multi-factor-authentication-end-user-manage-settings.md)  
* [Microsoft Authenticator-App – häufig gestellte Fragen](microsoft-authenticator-app-faq.md)


