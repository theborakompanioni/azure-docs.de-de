---
title: Azure Active Directory Identity Protection-Benachrichtigungen | Microsoft Docs
description: "Erfahren Sie, wie Benachrichtigungen Ihre Untersuchungsaktivitäten unterstützen."
services: active-directory
keywords: Azure Active Directory Identity Protection, Cloud App Discovery, Verwalten von Anwendungen, Sicherheit, Risiko, Risikostufe, Sicherheitsrisiko, Sicherheitsrichtlinie
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 65ca79b9-4da1-4d5b-bebd-eda776cc32c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/20/2016
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: ce3379d5b5e883c6601c40aca191e8b84e3ad8d3
ms.openlocfilehash: 0170b5d2435f6e856478ee9e55ae26c626288f75


---
# <a name="azure-active-directory-identity-protection-notifications"></a>Azure Active Directory Identity Protection Benachrichtigungen
Von Azure AD Identity Protection werden zwei Arten von automatisierten Benachrichtigungs-E-Mails gesendet, die der Verwaltung von Benutzerrisiko- und Risikoereignissen dienen:

* E-Mail als Warnung vor kompromittiertem Benutzer
* Wöchentliche E-Mail mit Übersicht

## <a name="user-compromised-alert-email"></a>E-Mail als Warnung vor kompromittiertem Benutzer
Eine E-Mail als Warnung vor einem kompromittiertem Benutzer wird generiert, wenn ein Konto von Azure AD Identity Protection als kompromittiert erkannt wird. Die E-Mail enthält einen Link zum Bericht vom Typ „Benutzer mit Risikokennzeichnung“ im Identity Protection Dashboard. Es wird empfohlen, dass Sie Benachrichtigungen zu kompromittierten Konten sofort untersuchen.

## <a name="weekly-digest-email"></a>Wöchentliche E-Mail mit Übersicht
Die wöchentliche E-Mail mit einer Übersicht enthält eine Zusammenfassung der neuen Risikoereignisse.<br>
Sie hat folgenden Inhalt:

* Gefährdete Benutzer
* Verdächtige Aktivitäten
* Erkannte Sicherheitsrisiken
* Links zu verwandten Berichten in Identity Protection

<br>
![Wartung](./media/active-directory-identityprotection-notifications/400.png "Remediation")
<br>

Sie können das Senden einer wöchentlichen Übersichts-E-Mail deaktivieren.
<br><br>
![Benutzerrisiken](./media/active-directory-identityprotection-notifications/62.png "User risks")
<br>

**So öffnen Sie das entsprechende Konfigurationsdialogfeld:**

1. Klicken Sie auf dem Blatt **Azure AD Identity Protection** auf **Einstellungen**.
   <br><br>
   ![Richtlinie zum Benutzerrisiko](./media/active-directory-identityprotection-notifications/401.png "User risk policy")
   <br>
2. Klicken Sie im Abschnitt **Allgemein** auf **Benachrichtigungen**.
   <br><br>
   ![Richtlinie zum Benutzerrisiko](./media/active-directory-identityprotection-notifications/405.png "User risk policy")
   <br>

## <a name="see-also"></a>Weitere Informationen
* [Azure Active Directory Identity Protection](active-directory-identityprotection.md)



<!--HONumber=Dec16_HO4-->


