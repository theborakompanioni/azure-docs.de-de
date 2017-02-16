---
title: "Der Azure MFA-Anmeldevorgang mit Überprüfung in zwei Schritten | Microsoft Docs"
description: "Auf dieser Seite finden Sie Anleitung zu den verschiedenen Anmeldemethoden, die mit Azure MFA verfügbar sind."
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
ms.date: 12/15/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 4ff028f88e984f28bc0f4a228aabed1fabc90560
ms.openlocfilehash: 63951136d4f7bc2eb81c24ddfd58e696fc31cf37


---
# <a name="the-sign-in-experience-with-azure-multi-factor-authentication"></a>Der Anmeldevorgang mit Azure Multi-Factor Authentication
> [!NOTE]
> Die folgende Dokumentation auf dieser Seite zeigt eine normale Anmeldeumgebung.  Hilfe zur Anmeldung finden Sie unter [Beheben von Problemen mit Azure Multi-Factor Authentication](multi-factor-authentication-end-user-manage-settings.md)
>
>

## <a name="what-will-your-sign-in-experience-be"></a>Was sieht Ihr Anmeldevorgang aus?
Der Anmeldevorgang hängt davon ab, wie Sie sich anmelden und Multi-Factor Authentication verwenden.  In diesem Abschnitt finden Sie Informationen dazu, was Sie bei der Anmeldung erwartet.  Wählen Sie die Option aus, die am besten auf Sie zutrifft.

| Vorgehensweise | Beschreibung |
|:--- |:--- |
| [Anmeldung mit Mobil- oder Bürotelefon](#signing-in-with-mobile-or-office-phone) |Beschreibt die Anmeldung mit einem Mobil- oder Bürotelefon. |
| [Anmelden mit der Microsoft Authenticator-App über eine Benachrichtigung](#signing-in-with-the-microsoft-authenticator-app-using-notification) |Beschreibt die benachrichtigungsbasierte Anmeldung mit der Microsoft Authenticator-App. |
| [Anmelden mit der Microsoft Authenticator-App über einen Überprüfungscode](#signing-in-with-the-microsoft-authenticator-app-using-verification-code) |Beschreibt die prüfcodebasierte Anmeldung mit der Microsoft Authenticator-App. |
| [Anmelden mit einer alternativen Methode](#signing-in-with-an-alternate-method) |Hier erfahren Sie, was Sie erwartet, wenn Sie eine alternative Methode verwenden möchten. |

## <a name="signing-in-with-mobile-or-office-phone"></a>Anmeldung mit Mobil- oder Bürotelefon
Im Folgenden wird der Vorgang für Multi-Factor Authentication mit dem Büro- oder Mobiltelefon beschrieben.

### <a name="to-sign-in-with-a-call-to-your-office-or-mobile-phone"></a>So melden Sie sich über einen Aufruf bei Ihrem Büro- oder Mobiltelefon an
* Melden Sie sich mit Ihrem Benutzernamen und Kennwort bei einer Anwendung oder einem Dienst wie Office 365 an.  
* Sie erhalten einen Anruf von Microsoft.  
* Nehmen Sie den Anruf entgegen, und drücken Sie die #-Taste.  
* Sie sollten jetzt angemeldet sein.  

## <a name="signing-in-with-the-microsoft-authenticator-app-using-notification"></a>Anmelden mit der Microsoft Authenticator-App über eine Benachrichtigung
Die folgenden Informationen beschreiben die Verwendung der Multi-Factor Authentication mit der Microsoft Authenticator-App über eine an Sie gesendete Benachrichtigung.

### <a name="to-sign-in-with-a-notification-sent-the-microsoft-authenticator-app"></a>So melden Sie sich mit der Microsoft Authenticator-App über eine Benachrichtigung an
* Melden Sie sich mit Ihrem Benutzernamen und Kennwort bei einer Anwendung oder einem Dienst wie Office 365 an.
* Sie erhalten eine Benachrichtigung von Microsoft.

![Microsoft sendet Benachrichtigung](./media/multi-factor-authentication-end-user-signin/notify.png)

* Rufen Sie die Benachrichtigung auf Ihrem Mobiltelefon auf, und tippen Sie auf die Schaltfläche "Überprüfen".  Wenn Ihr Unternehmen eine PIN anfordert, werden Sie hier zur Eingabe gebeten.

![Überprüfen](./media/multi-factor-authentication-end-user-signin/phone2.png)

![Einrichtung](./media/multi-factor-authentication-end-user-first-time/scan3.png)

* Sie sollten jetzt angemeldet sein.

## <a name="signing-in-with-the-microsoft-authenticator-app-using-verification-code"></a>Anmelden mit der Microsoft Authenticator-App über einen Überprüfungscode
Im Folgenden wird der Vorgang für die Multi-Factor Authentication mit der Microsoft Authenticator-App unter Verwendung eines Überprüfungscodes beschrieben.

### <a name="to-sign-in-using-a-verification-code-with-the-microsoft-authenticator-app"></a>So melden Sie sich mit der Microsoft Authenticator-App über einen Überprüfungscode an
* Melden Sie sich mit Ihrem Benutzernamen und Kennwort bei einer Anwendung oder einem Dienst wie Office 365 an.
* Sie werden von Microsoft aufgefordert, einen Bestätigungscode einzugeben.

![Eingeben des Überprüfungscodes](./media/multi-factor-authentication-end-user-signin/verify3.png)

* Öffnen Sie die Microsoft Authenticator-App auf Ihrem Smartphone, und geben Sie den Code bei der Anmeldung in das entsprechende Feld ein.

![Abrufen des Codes](./media/multi-factor-authentication-end-user-signin/phone3.png)

* Sie sollten jetzt angemeldet sein.

## <a name="signing-in-with-an-alternate-method"></a>Anmelden mit einer alternativen Methode
Im folgenden Abschnitt erfahren Sie, wie Sie sich über eine alternative Methode anmelden, wenn die primäre Methode nicht verfügbar ist.

### <a name="to-sign-in-with-an-alternate-method"></a>So melden Sie sich mit einer alternativen Methode an
* Melden Sie sich mit Ihrem Benutzernamen und Kennwort bei einer Anwendung oder einem Dienst wie Office 365 an.
* Wählen Sie eine andere Überprüfungsoption aus.  Es stehen verschiedene Optionen zur Auswahl. Die Anzahl der verfügbaren Optionen hängt davon ab, wie viele Sie eingerichtet haben.

![Anwenden einer alternativen Methode](./media/multi-factor-authentication-end-user-signin/alt.png)

* Wählen Sie eine alternative Methode aus, und melden Sie sich an.



<!--HONumber=Dec16_HO4-->


