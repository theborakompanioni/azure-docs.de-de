---
title: Behandeln von Problemen bei der Registrierung bei Azure | Microsoft-Dokumentation
description: "Es wird beschrieben, wie Sie einige häufige Probleme beheben, die bei der Azure-Registrierung auftreten können."
services: 
documentationcenter: 
author: JiangChen79
manager: adpick
editor: 
tags: billing,top-support-issue
ms.assetid: a0907da1-cb2d-41d1-a97f-43841fabe355
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: cjiang
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: 763bc597bdfc40395511cdd9d797e5c7aaad0fdf
ms.openlocfilehash: af8a7bbc4bf007dfa5bef7ceb9cf940ad752239a
ms.contentlocale: de-de
ms.lasthandoff: 09/06/2017

---
# <a name="troubleshoot-sign-up-issues-for-azure"></a>Registrierungsprobleme für Azure behandeln
Wenn Sie sich nicht für Azure registrieren können, verwenden Sie die in diesem Artikel behandelten Tipps, um häufige Probleme zu beheben. Wenn bei der Registrierung ein Problem mit Ihrer Kreditkarte auftritt, lesen Sie [Kreditkarte oder Debitkarte wird abgelehnt, wenn ich versuche, mich bei Azure zu registrieren](billing-credit-card-fails-during-azure-sign-up.md). Wenn Sie über ein Azure-Konto verfügen, sich aber nicht anmelden können, lesen Sie [Ich kann mich nicht anmelden, um mein Azure-Abonnement zu verwalten](billing-cannot-login-subscription.md).

## <a name="error-we-cannot-proceed-with-signup-due-to-an-issue-with-your-account-please-contact-billing-support"></a>Fehler „Die Anmeldung kann aufgrund eines Problems mit Ihrem Konto nicht fortgesetzt werden. Wenden Sie sich an den Abrechnungssupport.“ 

Gehen Sie folgendermaßen vor, um das Problem zu beheben:

1. Melden Sie sich mit den Anmeldeinformationen des Kontoadministrators beim [Azure-Kontocenter](https://account.azure.com) an. 
2. Klicken Sie auf **Profil** und dann auf **Details bearbeiten**.
3. Stellen Sie sicher, dass alle Adressfelder ausgefüllt und gültig sind. 
4. Vergewissern Sie sich bei der Registrierung des Azure-Abonnements, dass die beim Registrieren der Kreditkarte eingegebene Rechnungsadresse mit den Angaben der Bank übereinstimmt.

Wenn die Fehlermeldung weiterhin angezeigt wird, verwenden Sie einen anderen Browser für die Registrierung.

## <a name="progress-bar-hangs-in-identity-verification-by-card-section"></a>Statusanzeige hängt im Abschnitt „Identity verification by card“ (Überprüfung der Identität mit Kreditkarte)

Um die Identitätsprüfung mithilfe der Karte abzuschließen, müssen Cookies von Drittanbietern in Ihrem Browser zugelassen sein.

![Screenshot des Abschnitts „Identity verification by card“ (Überprüfung der Identität mit Kreditkarte) während der Registrierung](./media/billing-troubleshoot-azure-sign-up-issues/identity-verification-hangs.PNG)

Führen Sie die folgenden Schritte aus, um die Cookie-Einstellungen Ihres Browsers zu aktualisieren.

1. Wenn Sie Chrome verwenden, gehen Sie zu **Einstellungen** > **Erweiterte Einstellungen anzeigen** > **Datenschutz** > **Inhaltseinstellungen**. Deaktivieren Sie **Cookies und Websitedaten von Drittanbietern blockieren**.
2. Wenn Sie Edge verwenden, gehen Sie zu **Einstellungen** > **Erweiterte Einstellungen anzeigen** > **Cookies**. Wählen Sie **Keine Cookies blockieren** aus.
3. Aktualisieren Sie die Azure Registrierungsseite, und überprüfen Sie, ob das Problem behoben ist.
4. Wenn Das Problem durch die Aktualisierung nicht behoben wurde, beenden Sie den Browser, und starten Sie ihn neu. Wiederholen Sie die Registrierung anschließend.

## <a name="credit-card-form-doesnt-support-my-billing-address"></a>Das Kreditkartenformular akzeptiert meine Rechnungsadresse nicht.
Ihre Rechnungsadresse muss sich in dem Land befinden, das Sie im Abschnitt **Informationen zu Ihrer Person** ausgewählt haben. Stellen Sie sicher, dass Sie das richtige Land auswählen.

## <a name="no-text-messages-or-calls-during-sign-up-account-verification"></a>Keine SMS oder Anrufe während der Kontoüberprüfung für die Registrierung
Die Zustellung des Prüfcodes kann bis zu vier Minuten dauern, geschieht in der Regel jedoch deutlich schneller. Die zur Überprüfung eingegebene Telefonnummer wird nicht als Kontaktnummer für das Konto gespeichert.

Hier sind einige zusätzlichen Tipps:
* Für den Überprüfungsvorgang per Telefon kann keine VoIP-Telefonnummer verwendet werden.
* Überprüfen Sie im Dropdownmenü die eingegebene Telefonnummer einschließlich Landesvorwahl.
* Wenn Ihr Telefon keine Textnachrichten (SMS) empfängt, versuchen Sie es mit der Option **Mich anrufen**.
* Stellen Sie sicher, dass Ihr Telefon Anrufe oder SMS-Nachrichten von einer Nummer aus den USA empfangen kann.

Wenn Sie die Textnachricht oder den Telefonanruf erhalten haben, geben Sie den empfangenen Code in das Textfeld ein.

## <a name="credit-card-declined-or-not-accepted"></a>Kreditkarte abgelehnt bzw. nicht akzeptiert
Virtuelle oder Prepaid-Kreditkarten oder -Debitkarten werden als Zahlungsmittel für Azure-Abonnements nicht akzeptiert. Weitere Informationen zu möglichen Ursachen für die Ablehnung Ihrer Karte finden Sie unter [Kreditkarte oder Debitkarte wird abgelehnt, wenn ich versuche, mich bei Azure zu registrieren](billing-credit-card-fails-during-azure-sign-up.md).

## <a name="free-trial-is-not-available"></a>„Kostenlose Testversion ist nicht verfügbar“
Haben Sie bereits einmal ein Azure-Abonnement verwendet? Laut Vereinbarung in den Azure-Nutzungsbedingungen ist die Aktivierung einer kostenlosen Testversion nur für Benutzer eingeschränkt, die neu bei Azure sind. Wenn Sie bereits einen anderen Typ von Azure-Abonnement verwendet haben, können Sie keine kostenlose Testversion aktivieren. Sie könnten sich auch für ein [Abonnement mit nutzungsbasierter Bezahlung](https://azure.microsoft.com/offers/ms-azr-0003p/) registrieren.

## <a name="i-saw-a-charge-on-my-free-trial-account"></a>In meinem kostenlosen Testkonto werden Gebühren angezeigt.
Nach der Registrierung wird möglicherweise eine kleine Prüfsperre in Ihrem Kreditkartenkonto angezeigt, die innerhalb von 3 bis 5 Tagen entfernt wird. Wenn Sie Bedenken bezüglich der Verwaltung von Kosten haben, lesen Sie den Abschnitt [Vermeiden unerwarteter Kosten](https://docs.microsoft.com/azure/billing/billing-getting-started).

## <a name="cant-activate-azure-benefit-plan-like-msdn-bizspark-bizsparkplus-or-mpn"></a>Ich kann kein Azure-Vorteilsprogramm wie MSDN, BizSpark, BizSparkPlus oder MPN aktivieren
Überprüfen Sie, ob Sie die richtigen Anmeldeinformationen verwenden. Überprüfen Sie anschließend das Vorteilsprogramm, um sich zu vergewissern, dass Sie berechtigt sind. 

* MSDN
  * Überprüfen Sie auf der [MSDN-Kontoseite](https://msdn.microsoft.com/subscriptions/manage/default.aspx)Ihren Berechtigungsstatus.
  * Wenn Sie Ihren Status nicht überprüfen können, wenden Sie sich an das [Kundendienstcenter für MSDN-Abonnements](https://msdn.microsoft.com/subscriptions/contactus.aspx)
* BizSpark
  * Melden Sie sich beim [BizSpark-Portal](https://www.microsoft.com/bizspark/default.aspx#start-two) an, und überprüfen Sie Ihren Berechtigungsstatus für BizSpark und BizSpark Plus.
  * Wenn Sie Ihren Status nicht überprüfen können, können Sie [weitere Unterstützung in den BizSpark-Foren beziehen](http://aka.ms/bzforums).
* MPN
  * Melden Sie sich beim [MPN-Portal](https://mspartner.microsoft.com/en/us/Pages/Locale.aspx) an, und überprüfen Sie Ihren Berechtigungsstatus. Ihnen stehen ggf. weitere Leistungen zu, wenn Sie über die entsprechenden [Kompetenzen für Cloudplattformen](https://mspartner.microsoft.com/en/us/pages/membership/cloud-platform-competency.aspx) verfügen.
  * Wenn Sie Ihren Status nicht überprüfen können, wenden Sie sich an den [MPN-Support](https://mspartner.microsoft.com/en/us/Pages/Support/Premium/contact-support.aspx).

## <a name="cant-activate-new-azure-in-open-subscription"></a>Ich kann kein neues Azure In Open-Abonnement aktivieren
Um ein Azure In Open-Abonnement zu erstellen, müssen Sie über einen gültigen OSA-Schlüssel (Online Service Activation) verfügen, dem mindestens ein Azure In Open-Token zugeordnet ist. Wenn Sie keinen OSA-Schlüssel haben, wenden Sie sich an einen der unter [Microsoft Pinpoint](http://pinpoint.microsoft.com/) aufgeführten Microsoft-Partner.

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Wenden Sie sich an den Support.
[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls Sie weitere Hilfe benötigen, um das Problem schnell beheben zu lassen.

