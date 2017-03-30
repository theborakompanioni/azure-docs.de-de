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
ms.date: 03/21/2017
ms.author: cjiang
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: a50b4fb4b19acf270aaf94fea613e745ec2af649
ms.lasthandoff: 03/22/2017

---
# <a name="troubleshoot-sign-up-issues-for-azure"></a>Registrierungsprobleme für Azure behandeln
Wenn Sie sich nicht für Azure registrieren können, verwenden Sie die in diesem Artikel behandelten Tipps, um häufige Probleme zu beheben. Wenn bei der Registrierung ein Problem mit Ihrer Kreditkarte auftritt, lesen Sie [Kreditkarte oder Debitkarte wird abgelehnt, wenn ich versuche, mich bei Azure zu registrieren](billing-credit-card-fails-during-azure-sign-up.md). Wenn Sie über ein Azure-Konto verfügen, sich aber nicht anmelden können, lesen Sie [Ich kann mich nicht anmelden, um mein Azure-Abonnement zu verwalten](billing-cannot-login-subscription.md).

## <a name="progress-bar-hangs-in-identity-verification-by-card-section"></a>Statusanzeige hängt im Abschnitt „Identity verification by card“ (Überprüfung der Identität mit Kreditkarte)

Um die Identitätsprüfung mithilfe der Karte abzuschließen, müssen Cookies von Drittanbietern in Ihrem Browser zugelassen sein.

![Screenshot des Abschnitts „Identity verification by card“ (Überprüfung der Identität mit Kreditkarte) während der Registrierung](./media/billing-troubleshoot-azure-sign-up-issues/identity-verification-hangs.PNG)

Führen Sie die folgenden Schritte aus, um die Cookie-Einstellungen Ihres Browsers zu aktualisieren.

1. Wenn Sie Chrome verwenden, gehen Sie zu **Einstellungen** > **Erweiterte Einstellungen anzeigen** > **Datenschutz** > **Inhaltseinstellungen**. Deaktivieren Sie **Cookies und Websitedaten von Drittanbietern blockieren**.
2. Wenn Sie Edge verwenden, gehen Sie zu **Einstellungen** > **Erweiterte Einstellungen anzeigen** > **Cookies**. Wählen Sie **Keine Cookies blockieren** aus.
3. Aktualisieren Sie die Azure Registrierungsseite, und überprüfen Sie, ob das Problem behoben ist.
4. Wenn Das Problem durch die Aktualisierung nicht behoben wurde, beenden Sie den Browser, und starten Sie ihn neu. Wiederholen Sie die Registrierung anschließend.

## <a name="no-text-messages-or-calls-during-sign-up-account-verification"></a>Keine SMS oder Anrufe während der Kontoüberprüfung für die Registrierung
Wenn Sie **Textnachricht senden** aktiviert haben, kann die Übermittlung Ihres Textcodes an Ihr Telefon bis zu fünf Minuten dauern. Zur Identitätsprüfung müssen Sie naturgemäß eine Telefonnummer eingeben, unter der SMS-Nachrichten empfangen werden können, oder, wenn Sie die Option **Mich anrufen** verwenden, unter der das Telefon Anrufe empfangen kann. Die eingegebene Telefonnummer wird nur zur Identitätsprüfung verwendet und nicht als Kontaktnummer für das Konto gespeichert.

Hier einige Tipps:
* Für den Überprüfungsvorgang per Telefon kann keine VoIP-Telefonnummer verwendet werden.
* Überprüfen Sie im Dropdownmenü die eingegebene Telefonnummer einschließlich Landesvorwahl.
* Wenn Ihr Telefon keine Textnachrichten (SMS) empfängt, verwenden Sie die Option **Mich anrufen**.
* Wenn bei dem Schritt zur telefonischen Überprüfung ein Fehler auftritt, nachdem Sie sowohl **Textnachricht senden** als auch **Mich anrufen** ausprobiert haben, verwenden Sie eine andere Telefonnummer.

Wenn Sie die Textnachricht oder den Anruf empfangen, geben Sie den erhaltenen Code in das Textfeld ein.

## <a name="credit-card-declined-or-not-accepted"></a>Kreditkarte abgelehnt bzw. nicht akzeptiert
Virtuelle oder Prepaid-Kreditkarten oder -Debitkarten werden als Zahlungsmittel für Azure-Abonnements nicht akzeptiert. Weitere Informationen zu möglichen Ursachen für die Ablehnung Ihrer Karte finden Sie unter [Kreditkarte oder Debitkarte wird abgelehnt, wenn ich versuche, mich bei Azure zu registrieren](billing-credit-card-fails-during-azure-sign-up.md).

## <a name="free-trial-is-not-available"></a>„Kostenlose Testversion ist nicht verfügbar“
Haben Sie bereits einmal ein Azure-Abonnement verwendet? Laut Vereinbarung in den Azure-Nutzungsbedingungen ist die Aktivierung einer kostenlosen Testversion nur für Benutzer eingeschränkt, die neu bei Azure sind. Wenn Sie bereits einen anderen Typ von Azure-Abonnement verwendet haben, können Sie keine kostenlose Testversion aktivieren. Sie könnten sich auch für ein [Abonnement mit nutzungsbasierter Bezahlung](https://azure.microsoft.com/offers/ms-azr-0003p/) registrieren.

## <a name="cant-activate-azure-benefit-plan-like-msdn-bizspark-bizsparkplus-or-mpn"></a>Ich kann kein Azure-Vorteilsprogramm wie MSDN, BizSpark, BizSparkPlus oder MPN aktivieren
Überprüfen Sie, ob Sie die richtigen Anmeldeinformationen verwenden. Überprüfen Sie anschließend das Vorteilsprogramm, um sich zu vergewissern, dass Sie berechtigt sind. 

* MSDN
  * Überprüfen Sie auf der [MSDN-Kontoseite](https://msdn.microsoft.com/subscriptions/manage/default.aspx)Ihren Berechtigungsstatus.
  * Wenn Sie Ihren Status nicht überprüfen können, wenden Sie sich an das [Kundendienstcenter für MSDN-Abonnements](https://msdn.microsoft.com/subscriptions/contactus.aspx)
* BizSpark
  * Melden Sie sich beim [BizSpark-Portal](https://www.microsoft.com/bizspark/default.aspx#start-two) an, und überprüfen Sie Ihren Berechtigungsstatus für BizSpark und BizSpark Plus.
  * Wenn Sie Ihren Status nicht überprüfen können, können Sie sich an das [BizSpark-Team wenden](mailto:bizspark@microsoft.com?subject=BizSpark%20Support&body=Thank%20you%20for%20contacting%20BizSpark.%20Please%20provide%20as%20much%20of%20the%20following%20information%20as%20possible,%20as%20it%20will%20help%20expedite%20our%20response%20to%20you.%0aContact%20name:%0aStartup%20name:%0aMicrosoft%20Account/Live%20ID:%0aSpecific%20description%20of%20issue%20experienced%20or%20question:%0a%0aThank%20you,%0a%0aThe%20BizSpark%20Team)
* MPN
  * Melden Sie sich beim [MPN-Portal](https://mspartner.microsoft.com/en/us/Pages/Locale.aspx) an, und überprüfen Sie Ihren Berechtigungsstatus. Ihnen stehen ggf. weitere Leistungen zu, wenn Sie über die entsprechenden [Kompetenzen für Cloudplattformen](https://mspartner.microsoft.com/en/us/pages/membership/cloud-platform-competency.aspx) verfügen.
  * Wenn Sie Ihren Status nicht überprüfen können, wenden Sie sich an den [MPN-Support](https://mspartner.microsoft.com/en/us/Pages/Support/Premium/contact-support.aspx).

## <a name="cant-activate-new-azure-in-open-subscription"></a>Ich kann kein neues Azure In Open-Abonnement aktivieren
Um ein Azure In Open-Abonnement zu erstellen, müssen Sie über einen gültigen OSA-Schlüssel (Online Service Activation) verfügen, dem mindestens ein Azure In Open-Token zugeordnet ist. Wenn Sie keinen OSA-Schlüssel haben, wenden Sie sich an einen der unter [Microsoft Pinpoint](http://pinpoint.microsoft.com/) aufgeführten Microsoft-Partner.

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Wenden Sie sich an den Support.
[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls Sie weitere Hilfe benötigen, um das Problem schnell beheben zu lassen.

