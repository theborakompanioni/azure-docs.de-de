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
ms.date: 01/24/2016
ms.author: cjiang
translationtype: Human Translation
ms.sourcegitcommit: 73c4ff6fdbde09dd08d1bcacf0ec87c7fa43f3b2
ms.openlocfilehash: 7ded94f432f8e61153ee8dfbf2508b288a4ad5fd


---
# <a name="i-cant-sign-up-for-azure"></a>Ich kann mich nicht für Azure registrieren
Wenn Sie sich nicht für Azure registrieren können, können Sie mehrere Dinge überprüfen, um das Problem zu beheben.

## <a name="progress-bar-hangs-in-identity-verification-by-card-section"></a>Statusanzeige hängt im Abschnitt „Identity verification by card“ (Überprüfung der Identität mit Kreditkarte)

Während der Azure-Registrierung erfolgt eine Überprüfung der Identität mit der Kreditkarte. Wenn die Statusanzeige hängt:

![Screenshot des Abschnitts „Identity verification by card“ (Überprüfung der Identität mit Kreditkarte) während der Registrierung](./media/billing-troubleshoot-azure-sign-up-issues/identity-verification-hangs.PNG)

Dieses Problem tritt auf, wenn Cookies von Drittanbietern in Ihren Browser blockiert werden.

### <a name="suggestion"></a>Vorschlag

1. Lassen Sie Cookies von Drittanbieter in den Browsereinstellungen zu.
  * In Edge: Aktivieren Sie unter „Einstellungen“ > „Erweiterte Einstellungen anzeigen“ > „Cookies“ die Option „Keine Cookies blockieren“ aus.
  * In Chrome: Deaktivieren Sie unter „Einstellungen“ > „Erweiterte Einstellungen anzeigen“ > „Datenschutz“ > „Inhaltseinstellungen“ die Option „Drittanbieter-Cookies und Websitedaten blockieren“.
2. Aktualisieren Sie die Azure Registrierungsseite, und überprüfen Sie, ob das Problem behoben ist.
3. Wenn Das Problem durch die Aktualisierung nicht behoben wurde, beenden Sie den Browser, und starten Sie ihn neu. Wiederholen Sie die Registrierung anschließend.

## <a name="no-text-messages-or-calls-during-sign-up-account-verification"></a>Keine SMS oder Anrufe während der Kontoüberprüfung für die Registrierung
* Stellen Sie sicher, dass Sie über die angegebene Telefonnummer SMS empfangen können.
* Überprüfen Sie im Dropdownmenü die eingegebene Telefonnummer einschließlich Vorwahl.
* Vergewissern Sie sich, dass Ihr Telefon bei Wahl von „Textnachricht senden“ Textnachrichten (SMS) ober bei Wahl von „Anrufen“ Telefonanrufe empfangen kann.
* Wenn Sie ein Mobiltelefon verwenden, sollten Sie sicherstellen, dass Sie über eine gute Verbindung verfügen.
* Räumen Sie bei Auswahl von „Textnachricht senden“ dem SMS-System bis zu vier Minuten für die Übermittlung Ihres Textcodes ein.
* Nachdem Sie die Nachricht erhalten haben, fügen Sie den Code in das Textfeld ein und klicken auf die Schaltfläche „Überprüfung“, um fortzufahren.

### <a name="suggestions"></a>Vorschläge
* Wenn Sie keine Textnachrichten (SMS) auf Ihrem Mobiltelefon erhalten, verwenden Sie die alternative Überprüfungsmethode "Anrufen".
* Verwenden Sie eine andere Telefonnummer, wenn beim telefonischen Überprüfungsschritt die beiden Methoden (Textnachricht und Anruf) keinen Erfolg haben.
* Für den Überprüfungsvorgang per Telefon kann keine VoIP-Telefonnummer verwendet werden.

> [!NOTE]
> Sie können Ihre bevorzugte Telefonnummer später durch [Aktualisierung Ihrer Profilinformationen](billing-how-to-change-azure-account-profile.md)ändern.
>
>

## <a name="credit-card-declined-or-not-accepted"></a>Kreditkarte abgelehnt bzw. nicht akzeptiert
Stellen Sie sicher, dass die bei der Registrierung angegebene Zahlungsmethode für Azure-Aktivierungen und -Zahlungen unterstützt wird.

* Virtuelle oder vorausbezahlte Kredit- und Lastschriftkarten werden nicht akzeptiert.
* Akzeptierte Anbieter von Kredit- und Lastschriftkarten variieren je nach Land.

### <a name="suggestion"></a>Vorschlag
Informationen zu häufigen Registrierungsproblemen bei Verwendung einer Kredit- oder Debitkarte finden Sie unter [Ihre Kredit- oder Debitkarte wurde bei der Azure-Registrierung abgelehnt](billing-credit-card-fails-during-azure-sign-up.md).

## <a name="cant-activate-azure-benefit-plan-like-msdn-bizspark-bizsparkplus-or-mpn"></a>Ich kann kein Azure-Vorteilsprogramm wie MSDN, BizSpark, BizSparkPlus oder MPN aktivieren
Vergewissern Sie sich über Ihren Vorteilsprogrammkanal, ob Sie für den gewählten Plan berechtigt sind:

* MSDN
  * Überprüfen Sie auf der [MSDN-Kontoseite](https://msdn.microsoft.com/subscriptions/manage/default.aspx)Ihren Berechtigungsstatus.
  * Wenn Sie Ihren Status nicht überprüfen können, wenden Sie sich an das [Kundendienstcenter für MSDN-Abonnements](https://msdn.microsoft.com/subscriptions/contactus.aspx)
* MPN
  * Melden Sie sich beim [MPN-Portal](https://mspartner.microsoft.com/en/us/Pages/Locale.aspx) an, und überprüfen Sie Ihren Berechtigungsstatus. Ihnen stehen ggf. weitere Leistungen zu, wenn Sie über die entsprechenden [Kompetenzen für Cloudplattformen](https://mspartner.microsoft.com/en/us/pages/membership/cloud-platform-competency.aspx) verfügen.
  * Wenn Sie Ihren Status nicht überprüfen können, wenden Sie sich an den [MPN-Support](https://mspartner.microsoft.com/en/us/Pages/Support/Premium/contact-support.aspx).
* BizSpark
  * Melden Sie sich beim [BizSpark-Portal](https://www.microsoft.com/bizspark/default.aspx#start-two) an, und überprüfen Sie Ihren Berechtigungsstatus für BizSpark und BizSpark Plus.
  * Wenn Sie Ihren Status nicht überprüfen können, wenden Sie sich an den BizSpark-Support, indem Sie eine [E-Mail an das BizSpark-Team senden](mailto:bizspark@microsoft.com?subject=BizSpark%20Support&body=Thank%20you%20for%20contacting%20BizSpark.%20Please%20provide%20as%20much%20of%20the%20following%20information%20as%20possible,%20as%20it%20will%20help%20expedite%20our%20response%20to%20you.%0aContact%20name:%0aStartup%20name:%0aMicrosoft%20Account/Live%20ID:%0aSpecific%20description%20of%20issue%20experienced%20or%20question:%0a%0aThank%20you,%0a%0aThe%20BizSpark%20Team)

### <a name="suggestion"></a>Vorschlag
Wenn Sie versucht haben, ein neues Vorteilsabonnement zu aktivieren und bei der Registrierung ein Fehler auftritt, sollten Sie auf der [Azure-Abonnementseite](http://account.windowsazure.com/Subscriptions) überprüfen, ob die Einrichtung des Abonnements abgeschlossen wurde. Es dauert ein paar Minuten, bis Ihr Abonnement als aktiv angezeigt wird. Sie erhalten eine E-Mail, wenn Ihr Abonnement aktiviert wurde. Wenn Ihr Abonnementstatus länger als vier Minuten „Ausstehend“ lautet, sollten Sie sich [an den Azure-Support wenden](http://go.microsoft.com/fwlink/?linkid=544831&clcid=0x409), um Hilfe zu erhalten.

## <a name="cant-activate-new-azure-in-open-subscription"></a>Ich kann kein neues Azure In Open-Abonnement aktivieren
Sie benötigen einen gültigen OSA-Schlüssel, dem mindestens ein Azure in Open-Token zugeordnet ist, um ein neues Azure in Open-Abonnement zu aktivieren.

### <a name="suggestion"></a>Vorschlag
Wenn Sie keinen OSA-Schlüssel haben, wenden Sie sich an einen der unter [Microsoft Pinpoint](http://pinpoint.microsoft.com/) aufgeführten Microsoft-Partner.

## <a name="cant-activate-azure-free-trial"></a>Ich kann die kostenlose Azure-Testversion nicht aktivieren
Haben Sie bereits einmal ein Azure-Abonnement verwendet? Laut Vereinbarung in den Azure-Nutzungsbedingungen ist die Aktivierung einer kostenlosen Testversion nur für Benutzer eingeschränkt, die neu bei Azure sind. Wenn Sie bereits einen anderen Typ von Azure-Abonnement verwendet haben, können Sie keine kostenlose Testversion aktivieren.

### <a name="suggestion"></a>Vorschlag
* Wenn Sie schon einmal ein Azure-Abonnement aktiviert haben und bei der Aktivierung der kostenlosen Testversion ein Fehler auftritt, können Sie die Nutzung eines Abonnements mit nutzungsbasierter Bezahlung erwägen.
* Prüfen Sie, ob Sie über die Berechtigung für ein Vorteilsangebot verfügen. Weitere Informationen finden Sie auf der Seite mit den [Microsoft Azure-Angebotsdetails](https://azure.microsoft.com/support/legal/offer-details/). Für Vorteilspläne müssen bestimmte Voraussetzungen erfüllt sein.

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Wenden Sie sich an den Support.
[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls Sie weitere Hilfe benötigen, um das Problem schnell beheben zu lassen.



<!--HONumber=Jan17_HO1-->


