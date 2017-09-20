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
ms.date: 07/06/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: end-user
ms.translationtype: HT
ms.sourcegitcommit: 4c2be7c35f678430d0ad83a3374ef25f68fd2509
ms.openlocfilehash: 9dbe88a59b68bfb424c43dd89acf55d8c73fdf39
ms.contentlocale: de-de
ms.lasthandoff: 09/20/2017

---
# <a name="get-help-with-two-step-verification"></a>Hilfe bei der Überprüfung in zwei Schritten
Dieser Artikel behandelt die häufigsten Fragen von Anwendern zur Überprüfung in zwei Schritten. 

## <a name="why-do-i-have-to-perform-two-step-verification-can-i-turn-it-off"></a>Wozu muss ich die Überprüfung in zwei Schritten ausführen? Kann ich die Funktion deaktivieren?

Die Überprüfung in zwei Schritten ist eine Sicherheitsfunktion, die von Ihrer Organisation zum Schutz Ihrer Konten ausgewählt wurde. Sie ist sicherer als nur ein Kennwort, da sie auf zwei Formen der Authentifizierung basiert: etwas, das Sie wissen, und etwas, das Sie bei sich tragen. Was Sie wissen, ist Ihr Kennwort. Was Sie bei sich tragen, ist ein Smartphone oder ein anderes Gerät, das Sie häufig mit sich führen. Wenn Ihr Konto mit einer Überprüfung in zwei Schritten geschützt ist, kann sich ein bösartiger Hacker nicht mit Ihrer Identität anmelden, selbst wenn ihm Ihr Kennwort bekannt ist. Er kann sich nicht anmelden, da er keinen Zugang zu Ihrem Smartphone hat. 

Microsoft bietet die Überprüfung in zwei Schritten an, doch Ihre Organisation hat sich dazu entschieden, die Funktion zu verwenden. Sie können die Funktion nicht deaktivieren, wenn Ihre IT-Abteilung sie verlangt, genauso wie Sie die Verwendung eines Kennworts zum Schutz Ihres Kontos nicht deaktivieren können. 

Wenn die Überprüfung in zwei Schritten für Ihr persönliches Microsoft-Konto aktiviert ist und Sie Ihre Einstellungen ändern möchten, lesen Sie die Informationen zur [Überprüfung in zwei Schritten](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification). 

## <a name="i-dont-have-my-phone-with-me-today"></a>Ich habe heute mein Smartphone nicht bei mir

An manchen Tagen lassen Sie Ihr Smartphone möglicherweise zu Hause, müssen sich aber dennoch bei der Arbeit anmelden. In diesem Fall sollten Sie zuerst versuchen, sich über eine andere Überprüfungsmethode anzumelden. Haben Sie bei der Registrierung zur Überprüfung in zwei Schritten mehr als eine Telefonnummer eingerichtet? Zum Anmelden mit einer anderen Methode gehen Sie folgendermaßen vor:

1. Melden Sie sich wie gewohnt an.
2. Wählen Sie auf der Seite „Überprüfung in zwei Schritten“ die Option **Andere Überprüfungsoption verwenden** aus.

   ![Andere Überprüfung](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)

3. Wählen Sie die Überprüfungsoption aus, die Sie verwenden möchten. 
  - Wenn Sie auch keinen Zugriff auf Ihre alternativen Methoden haben, wenden Sie sich an Ihre IT-Abteilung, um Unterstützung beim Anmelden bei Ihrem Konto zu erhalten.
  - Wenn Sie Zugriff auf die alternativen Methoden haben, fahren Sie mit der Überprüfung in zwei Schritten fort.

Wenn die Verknüpfung **Andere Überprüfungsoption verwenden** nicht angezeigt wird, haben Sie bei der ersten Registrierung zur Überprüfung in zwei Schritten keine alternativen Methoden eingerichtet. Wenden Sie sich an Ihre IT-Abteilung, um Hilfe bei der Anmeldung bei Ihrem Konto zu bekommen. Sobald Sie sich angemeldet haben, [verwalten Sie Ihre Einstellungen](multi-factor-authentication-end-user-manage-settings.md), und fügen Sie für das nächste Mal zusätzliche Überprüfungsmethoden hinzu. 

## <a name="i-lost-my-phone-or-got-a-new-number"></a>Ich habe mein Smartphone verloren oder habe eine neue Nummer
Es gibt zwei Möglichkeiten, um wieder auf Ihr Konto zuzugreifen. Die erste besteht darin, die Nummer Ihres alternativen Authentifizierungstelefons zu verwenden, falls Sie eine solche eingerichtet haben. Die zweite besteht darin, Ihre IT-Abteilung zu bitten, Ihre Einstellungen zu löschen.

Wenn Ihr Telefon verloren gegangen ist oder entwendet wurde, empfehlen wir Ihnen ebenfalls, Ihre IT-Abteilung zu informieren. Die IT-Mitarbeiter müssen Ihre App-Kennwörter zurücksetzen und alle gespeicherten Geräte löschen. 

### <a name="use-an-alternate-phone-number"></a>Verwenden einer alternativen Telefonnummer
Wenn Sie mehrere Überprüfungsoptionen eingerichtet haben, beispielsweise eine sekundäre Telefonnummer oder eine Authentifikator-App auf einem anderen Gerät, nutzen Sie eine der Möglichkeiten, um sich anzumelden.

Um sich mithilfe der alternativen Telefonnummer anzumelden, führen Sie diese Schritte aus:

1. Melden Sie sich wie gewohnt an.
2. Wenn Sie aufgefordert werden, Ihr Konto zu verifizieren, wählen Sie **Andere Überprüfungsoption verwenden** aus.
   
   ![Andere Überprüfung](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)

3. Wählen Sie die Telefonnummer oder das Gerät aus, auf das Sie zugreifen können.
4. Wenn Sie wieder bei Ihrem Konto angemeldet sind, [verwalten Sie Ihre Einstellungen](multi-factor-authentication-end-user-manage-settings.md), um die Nummer Ihres Authentifizierungstelefons zu ändern.

### <a name="clear-your-settings"></a>Löschen der Einstellungen
Wenn Sie keine sekundäre Telefonnummer zur Authentifizierung konfiguriert haben, wenden Sie sich an Ihre IT-Abteilung. Ihre Einstellungen werden dann gelöscht, sodass Sie bei der nächsten Anmeldung erneut aufgefordert werden, sich für die [Überprüfung in zwei Schritten zu registrieren](multi-factor-authentication-end-user-first-time.md).

## <a name="i-am-not-receiving-a-text-or-call-on-my-phone"></a>Ich empfange keine SMS und keinen Anruf auf meinem Telefon
Es gibt verschiedene Gründe, aus denen Sie beim Anmeldeversuch keine SMS und keinen Telefonanruf erhalten. Wenn Sie zuvor bereits erfolgreich SMS oder Telefonanrufe auf Ihrem Telefon erhalten haben, liegt das Problem möglicherweise beim Telefonanbieter, nicht bei Ihrem Konto. Stellen Sie sicher, dass der Empfang gut ist. Wenn Sie versuchen, eine SMS zu erhalten, stellen Sie auch sicher, dass Sie SMS-Nachrichten empfangen können. Bitten Sie einen Freund, Sie probehalber anzurufen oder Ihnen eine SMS zu schicken. 

Wenn Sie bereits mehrere Minuten auf eine SMS oder einen Anruf gewartet haben, ist der Versuch, eine andere Authentifizierungsoption zu verwenden, die schnellste Möglichkeit, sich beim Konto anzumelden.

1. Wählen Sie auf der Seite, die auf Ihre Überprüfung warten, den Eintrag **Andere Überprüfungsoption verwenden** aus.
   
    ![Andere Überprüfung](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)
2. Wählen Sie die Telefonnummer oder Übermittlungsmethode, die Sie verwenden möchten.
   
    Wenn Sie mehrere Überprüfungscodes erhalten haben, verwenden Sie den neuesten.

Wenn Sie keine andere Methode konfiguriert haben, wenden Sie sich an Ihre IT-Abteilung mit der Bitte, Ihre Einstellungen zu löschen. Bei der nächsten Anmeldung werden Sie erneut aufgefordert, die [Multi-Factor Authentication einzurichten](multi-factor-authentication-end-user-first-time.md).

Wenn Sie häufig Verzögerungen aufgrund schlechten Empfangs bemerken, empfiehlt sich die Verwendung der [Microsoft Authenticator-App](microsoft-authenticator-app-how-to.md) auf Ihrem Smartphone. Die App kann zufällige Sicherheitscodes generieren, die Sie zum Anmelden verwenden. Diese Codes erfordern weder mobilen Empfang noch eine Internetverbindung.

## <a name="app-passwords-are-not-working"></a>App-Kennwörter funktionieren nicht
Stellen Sie zunächst sicher, dass Sie das App-Kennwort richtig eingegeben haben. Das generierte App-Kennwort ersetzt Ihr übliches Kennwort. Dies gilt jedoch nur für ältere Desktopanwendungen, die die Überprüfung in zwei Schritten nicht unterstützen. Wenn es weiterhin nicht funktioniert, versuchen Sie, sich anzumelden, und [erstellen Sie ein neues App-Kennwort](multi-factor-authentication-end-user-app-passwords.md).  Wenn dies immer noch nicht funktioniert, wenden Sie sich an Ihre IT-Abteilung mit der Bitte, [Ihre vorhandenen App-Kennwörter zu löschen](../multi-factor-authentication-manage-users-and-devices.md). Erstellen Sie anschließend ein neues Kennwort.

## <a name="i-didnt-find-an-answer-to-my-problem"></a>Ich konnte keine Lösung für mein Problem finden
Wenn Sie diese Schritte zur Problembehandlung ausgeführt haben, das Problem aber weiterhin besteht, wenden Sie sich an Ihre IT-Abteilung. Diese Personen sollten Ihnen helfen können.

## <a name="related-topics"></a>Verwandte Themen
* [Verwalten der Einstellungen für die Überprüfung in zwei Schritten](multi-factor-authentication-end-user-manage-settings.md)  
* [Microsoft Authenticator-App – häufig gestellte Fragen](microsoft-authenticator-app-faq.md)


