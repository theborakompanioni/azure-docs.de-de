---
title: Benachrichtigungen zu Azure Active Directory-Berichten
description: "Verwenden von Benachrichtigungen zu Azure Active Directory-Berichten zu verdächtigen Anmeldungen."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ae6d4b0e-5931-4cb3-98bf-9be91b381c92
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/15/2017
ms.author: dhanyahk;markvi
ms.custom: oldportal
ms.reviewer: dhanyahk
ms.translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 90ba006f25c27e1dc24c85fdc4ef1d5e2a8d0e3d
ms.contentlocale: de-de
ms.lasthandoff: 12/28/2016

---
# Benachrichtigungen zu Azure Active Directory-Berichten
<a id="azure-active-directory-reporting-notifications" class="xliff"></a>
## Welche Berichte generieren E-Mail-Benachrichtigungen?
<a id="what-reports-generate-email-notifications" class="xliff"></a>
Zu diesem Zeitpunkt werden E-Mail-Benachrichtigungen nur durch den Bericht zu irregulären Anmeldeaktivitäten ausgelöst.

## Was ist eine "irreguläre Anmeldung"?
<a id="what-is-an-irregular-sign-in" class="xliff"></a>
Irreguläre Anmeldungen sind Anmeldungen, die von Machine Learning-Algorithmen auf der Grundlage einer Bedingung für eine „unmögliche Strecke“ in Kombination mit einem anomalen Standort und Gerät für die Anmeldung identifiziert wurden. Dies kann ein Hinweis sein, dass ein Hacker versucht hat, sich mit diesem Konto anzumelden.

## Wer empfängt die E-Mail-Benachrichtigungen?
<a id="who-receives-the-email-notifications" class="xliff"></a>
Die E-Mail wird an alle globalen Administratoren mit einer Active Directory Premium-Lizenz gesendet. Um die Übermittlung sicherzustellen, wird sie auch an die alternative E-Mail-Adresse der Administratoren gesendet. Administratoren müssen aad-alerts-noreply@mail.windowsazure.com ihrer Liste sicherer Absender hinzufügen, damit sie die e-Mail nicht verpassen.

## Wie oft werden diese E-Mails gesendet?
<a id="how-often-are-these-emails-sent" class="xliff"></a>
Die E-Mail wird gesendet, wenn zehn neue irreguläre Anmeldeaktivitäten in den letzten 30 Tagen oder seit der letzten E-Mail aufgetreten sind, je nachdem, welcher Zeitraum kürzer ist.

## Wie greife ich auf den in der E-Mail genannten Bericht zu?
<a id="how-do-i-access-the-report-mentioned-in-the-email" class="xliff"></a>
Wenn Sie auf den Link klicken, werden Sie zur Berichtsseite im klassischen Azure-Portal weitergeleitet. Für den Zugriff auf den Bericht müssen Sie die beiden folgenden Rollen bekleiden:

* Administrator oder Co-Administrator Ihres Azure-Abonnements
* Globaler Administrator im Verzeichnis mit Active Directory Premium-Lizenz Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](active-directory-editions.md).

## Kann ich diese E-Mails deaktivieren?
<a id="can-i-turn-off-these-emails" class="xliff"></a>
Ja, zum Deaktivieren von Benachrichtigungen in Zusammenhang mit anomalen Anmeldungen klicken Sie im klassischem Azure-Portal auf **Konfigurieren** und wählen **Deaktiviert** im Abschnitt **Benachrichtigungen** aus.

## Nächste Schritte
<a id="whats-next" class="xliff"></a>
* Möchten Sie wissen, welche Sicherheits-, Überwachungs- und Aktivitätsberichte zur Verfügung stehen? Lesen Sie [Sicherheits-, Überwachungs- und Aktivitätsberichte](active-directory-view-access-usage-reports.md)
* [Erste Schritte mit Azure Active Directory Premium](active-directory-get-started-premium.md)
* [Hinzufügen Ihres Unternehmensbranding zur Anmelde- und Zugriffsbereichsseite](active-directory-add-company-branding.md)


