---
title: "Übertragung des Besitzes eines Azure-Abonnements auf ein anderes Konto | Microsoft-Dokumentation"
description: "In diesem Artikel wird die beschrieben, wie die Übertragung eines Azure-Abonnements auf einen anderen Benutzer erfolgt, und Sie finden hier einige häufig gestellte Fragen (FAQs) zu dem Prozess"
keywords: "azure-abonnement übertragen, azure übertragung abonnement, verschieben azure-abonnement auf anderes konto, azure änderung besitzer des abonnements, übertragung azure-abonnement auf anderes konto"
services: 
documentationcenter: 
author: genlin
manager: jlian
editor: 
tags: billing,top-support-issue
ms.assetid: c8ecdc1e-c9c5-468c-a024-94ae41e64702
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: genli
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: 4f77c7a615aaf5f87c0b260321f45a4e7129f339
ms.openlocfilehash: 15c6d3b8a335be068ead512c9b9d93940086c9ab
ms.contentlocale: de-de
ms.lasthandoff: 09/22/2017

---
# <a name="transfer-ownership-of-an-azure-subscription-to-another-account"></a>Übertragen des Besitzes eines Azure-Abonnements auf ein anderes Konto

Sie können Ihr Abonnement im Kontocenter auf einen anderen Benutzer übertragen. Verwenden Sie diese Funktion, um die Abonnementabrechnung an eine andere Person zu übergeben, das Anmeldekonto zu ändern oder das Abonnement in ein anderes Verzeichnis zu verschieben. Wie Sie Ihr Abonnement in ein anderes Angebot ändern können, erfahren Sie unter [Umstellen Ihres Azure-Abonnements auf ein anderes Angebot](billing-how-to-switch-azure-offer.md).

> [!IMPORTANT]
> 
> Derzeit werden Abonnementübertragungen für kostenlose Testversionen oder [Azure in Open](https://azure.microsoft.com/offers/ms-azr-0111p/)-Abonnements (AIO) nicht unterstützt. Wie Sie dies umgehen können, erfahren Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../azure-resource-manager/resource-group-move-resources.md).

<a id="supported"></a>

## <a name="whats-supported"></a>Unterstützte Umstellungen:

Die Self-Service-Abonnementübertragung ist für die in der folgenden Tabelle aufgeführten Angebote oder Abonnementtypen verfügbar. Zur Übertragung anderer Abonnements, z.B. [Sponsorship](https://azure.microsoft.com/offers/ms-azr-0036p/) oder Supportpläne, [wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

| Angebotsname                                                                             | Angebotsnummer |
|----------------------------------------------------------------------------------------|--------------|
| [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/)\*|MS-AZR-0017P        |
| [Microsoft Partner Network](https://azure.microsoft.com/offers/ms-azr-0025p/)          | MS-AZR-0025P        |
| [MSDN-Plattformen](https://azure.microsoft.com/offers/ms-azr-0062p/)                     | MS-AZR-0062P        |
| [Nutzungsbasierte Bezahlung](https://azure.microsoft.com/offers/ms-azr-0003p/)                      | MS-AZR-0003P        |
| [Pay-As-You-Go Dev/Test](https://azure.microsoft.com/offers/ms-azr-0023p/)             | MS-AZR-0023P        |
| [Visual Studio Enterprise](https://azure.microsoft.com/offers/ms-azr-0063p/)           | MS-AZR-0063P        |
| [Visual Studio Enterprise: BizSpark](https://azure.microsoft.com/offers/ms-azr-0064p/) | MS-AZR-0064P        |
| [Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p/)         | MS-AZR-0059P        |
| [Visual Studio Test Professional](https://azure.microsoft.com/offers/ms-azr-0060p/)    | MS-AZR-0060P        |

\*[Über das EA-Portal](#EA)

## <a name="transfer-ownership-of-an-azure-subscription"></a>Übertragen des Besitzes eines Azure-Abonnements

> [!VIDEO https://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/Transfer-an-Azure-subscription/player]
>
>

1. Melden Sie sich beim [Azure-Kontocenter](https://account.windowsazure.com/Subscriptions) als Kontoadministrator an. Wie Sie herausfinden, wer der Kontoadministrator des Abonnements ist, erfahren Sie unter [Häufig gestellte Fragen (FAQ)](#faq).

1. Wählen Sie das Abonnement, das Sie übertragen möchten.

1. Überprüfen Sie, ob Ihr Abonnement zur Self-Service-Übertragung berechtigt ist, indem Sie das **Angebot** und die **Angebots-ID** in der [Liste der unterstützten Angebote](#supported) prüfen.

   ![Überprüfen der Angebots-ID des Abonnements im Kontocenter](./media/billing-subscription-transfer/image0.png)
1. Klicken Sie auf **Abonnement übertragen**.

   ![Registerkarte „Azure-Kontoabonnements“](./media/billing-subscription-transfer/image1.png)
1. Geben Sie den Empfänger an.

   ![Dialogfeld „Abonnement übertragen“](./media/billing-subscription-transfer/image2.PNG)
1. Der Empfänger erhält automatisch eine E-Mail mit einem Link für den Vorgang zum Akzeptieren.

   ![E-Mail zur Abonnementübertragung an den Empfänger](./media/billing-subscription-transfer/image3.png)
1. Der Empfänger klickt auf den Link und folgt den Anweisungen. Außerdem gibt er seine Zahlungsinformationen ein.

   ![Erste Webseite zur Abonnementübertragung](./media/billing-subscription-transfer/image4.png)

   ![Zweite Webseite zur Abonnementübertragung](./media/billing-subscription-transfer/image5.png)
1. Erfolg! Das Abonnement ist jetzt übertragen.

<a id="EA"></a>

## <a name="transfer-subscription-ownership-for-enterprise-agreement-ea-customers"></a>Übertragen von Abonnements für Kunden mit Enterprise Agreement (EA)

Der Unternehmensadministrator kann den Besitz von Abonnements in einer Registrierung übertragen. Informationen zum Einstieg finden Sie unter [Transfer Account Ownership](https://ea.azure.com/helpdocs/changeAccountOwnerForASubscription) (Übertragen des Kontobesitzes) im EA-Portal.

## <a name="next-steps-after-accepting-ownership-of-a-subscription"></a>Nächste Schritte nach dem Übernehmen des Besitzes eines Abonnements

1. Sie sind nun der Kontoadministrator. Überprüfen und aktualisieren Sie den Dienstadministrator und die Co-Administratoren. Sie verwalten Administratoren im [klassischen Azure-Portal](https://manage.windowsazure.com) unter „Einstellungen“. [Erfahren Sie mehr über Administratorrollen](billing-add-change-azure-subscription-administrator.md).
1. Sie können auch die rollenbasierte Zugriffskontrolle (RBAC) für Ihr Abonnement und die Dienste verwenden. Besuchen Sie das [Azure-Portal](https://portal.azure.com). [Erfahren Sie mehr über RBAC](../active-directory/role-based-access-control-configure.md).
1. Aktualisieren Sie die Anmeldeinformationen für die Dienste dieses Abonnements, darunter:
   1. Verwaltungszertifikate, die dem Benutzer Administratorrechte auf Abonnementressourcen gewähren. Weitere Informationen finden Sie unter [Erstellen und Hochladen eines Verwaltungszertifikats für Azure](../cloud-services/cloud-services-certs-create.md)
   1. Tastenkombinationen für Dienste wie Storage. Weitere Informationen finden Sie unter [Informationen zu Azure-Speicherkonten](../storage/common/storage-create-storage-account.md).
   1. Anmeldeinformationen für den Remotezugriff für Dienste wie Azure Virtual Machines. 
1. [Aktualisieren Sie die Abrechnungswarnungen für dieses Abonnement](billing-set-up-alerts.md) im [Azure-Kontocenter](https://account.windowsazure.com/Subscriptions). 
1. Wenn Sie mit einem Partner arbeiten, sollten Sie die Partner-ID in diesem Abonnement aktualisieren. Sie können die Partner-ID im [Azure-Kontocenter](https://account.windowsazure.com/Subscriptions) aktualisieren.

<a id="faq"></a>

## <a name="frequently-asked-questions-faq"></a>Häufig gestellte Fragen (FAQ)

### <a name="whoisaa"></a> Wer ist der Kontoadministrator des Abonnements?

Der Kontoadministrator ist die Person, die sich für das Azure-Abonnement registriert bzw. die es erworben hat. Die Person ist zum Zugreifen auf das [Kontocenter](https://account.azure.com/Subscriptions) und Durchführen verschiedener Verwaltungsaufgaben berechtigt, z.B. Erstellen von Abonnements, Kündigen von Abonnements, Ändern der Abrechnung für ein Abonnement oder Ändern des Dienstadministrators. Verwenden Sie die folgenden Schritte für die Ermittlung, wenn Sie nicht sicher sind, wer der Kontoadministrator für ein Abonnement ist.

1. Navigieren Sie [im Azure-Portal zur Seite „Abonnements“](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Wählen Sie das zu überprüfende Abonnement aus, und sehen Sie unter **Einstellungen** nach.
1. Wählen Sie **Eigenschaften** aus. Der Kontoadministrator des Abonnements wird im Feld **Kontoadministrator** angezeigt.

### <a name="does-everything-transfer-including-resource-groups-vms-disks-and-other-running-services"></a>Wird alles übertragen? Auch Ressourcengruppen, virtuelle Computer, Datenträger und andere aktive Dienste?

Ja, alle Ressourcen wie virtuelle Computer, Datenträger und Websites werden auf den neuen Besitzer übertragen. Allerdings werden [Administratorrollen](billing-add-change-azure-subscription-administrator.md) und Richtlinien für die [rollenbasierte Zugriffssteuerung (RBAC)](../active-directory/role-based-access-control-configure.md), die Sie eingerichtet haben, nicht zwischen verschiedenen Verzeichnissen übertragen.

### <a id="no-button"></a> Warum wird die Schaltfläche „Abonnement übertragen“ nicht angezeigt?

Leider ist die Self-Service-Abonnementübertragung für Ihr Angebot oder Land nicht verfügbar. Zum Übertragen Ihres Abonnements [wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="does-a-subscription-transfer-result-in-any-service-downtime"></a>Führt eine Übertragung des Abonnements zu Ausfallzeiten?

Die Übertragung hat keine Auswirkung auf den Dienst. Durch die Übertragung des Abonnements wird das Abonnement unter dem aktuellen Kontoadministrator gekündigt und ein Abonnement unter dem Konto des Empfängers erstellt. Das neue Abonnement wird den zugrunde liegenden Azure-Diensten zugeordnet. Die Abonnement-ID bleibt unverändert.

### <a name="how-do-i-use-this-process-to-change-the-directory-for-subscription"></a>Wie verwende ich diesen Prozess zum Ändern des Verzeichnisses für ein Abonnement?

Azure-Abonnements werden in dem Verzeichnis erstellt, zu dem der Kontoadministrator gehört. Übertragen Sie das Abonnement auf ein Benutzerkonto im Zielverzeichnis, um das Verzeichnis zu ändern. Wenn der Benutzer die Schritte zum Akzeptieren der Übertragung abschließt, wird das Abonnement automatisch in das Zielverzeichnis verschoben.

### <a name="if-i-take-over-billing-ownership-of-a-subscription-from-another-organization-do-they-continue-to-have-access-to-my-resources"></a>Kann eine andere Organisation, von der ich die Abrechnung eines Abonnements übernehme, weiterhin auf meine Ressourcen zugreifen?

Wenn das Abonnement auf einen anderen Mandanten übertragen wird, verlieren die Benutzer, die dem vorherigen Mandanten zugeordnet sind, den Zugriff auf das Abonnement. Selbst wenn ein Benutzer kein Dienstadministrator oder Co-Administrator mehr ist, hat er über andere Sicherheitsmechanismen möglicherweise immer noch Zugriff auf das Abonnement, einschließlich:

* Verwaltungszertifikate, die dem Benutzer Administratorrechte auf Abonnementressourcen gewähren. Weitere Informationen finden Sie unter [Erstellen und Hochladen eines Verwaltungszertifikats für Azure](../cloud-services/cloud-services-certs-create.md).
* Tastenkombinationen für Dienste wie Storage. Weitere Informationen finden Sie unter [Informationen zu Azure-Speicherkonten](../storage/common/storage-create-storage-account.md).
* Anmeldeinformationen für den Remotezugriff für Dienste wie Azure Virtual Machines.

Der Empfänger sollte sich überlegen, ob er die dem Dienst zugeordneten geheimen Schlüssel aktualisiert, wenn er den Zugriff auf die Ressourcen einschränken möchte. Die meisten Ressourcen können durch Ausführen der folgenden Schritte aktualisiert werden:

  1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
  2. Wählen Sie im Menü „Hub“ die Option **Alle Ressourcen** aus.
  3. Wählen Sie die Ressource.
  4. Klicken Sie im Blatt "Ressourcen" auf **Einstellungen**. Hier können Sie die vorhandenen Schlüssel anzeigen und aktualisieren.

### <a name="if-i-transfer-the-subscription-in-the-middle-of-the-billing-cycle-does-the-recipient-pay-for-the-entire-billing-cycle"></a>Wenn ich das Abonnement in der Mitte des Abrechnungszyklus übertrage, zahlt der Empfänger die Rechnung für den gesamten Zyklus?

Der Absender ist zuständig für die Zahlung für jegliche Nutzung, die bis zu dem Zeitpunkt gemeldet wurde, an dem die Übertragung abgeschlossen war. Der Empfänger ist verantwortlich für die Verwendung, die vom Zeitpunkt der Übertragung an berichtet wird. Möglicherweise gibt es Nutzung, die vor der Übertragung stattgefunden hat, aber danach gemeldet wurde. Diese Nutzung ist in der Rechnung des Empfängers enthalten.

### <a name="does-the-recipient-have-access-to-usage-and-billing-history"></a>Verfügt der Empfänger über Zugriff auf den Nutzungs- und Abrechnungsverlauf?

  Die einzige verfügbare Information für den Empfänger ist der Betrag der letzten Rechnung oder, wenn das Abonnement vor dem Generieren der ersten Rechnung übertragen wurde, der derzeitige Kontostand. Der restliche Nutzungs- und Abrechnungsverlauf wird nicht mit dem Abonnement übertragen.

### <a name="can-the-offer-be-changed-during-a-transfer"></a>Kann das Angebot während der Übertragung geändert werden?

Das Angebot muss unverändert bleiben. Wie Sie Ihr Angebot ändern können, erfahren Sie unter [Umstellen Ihres Azure-Abonnements auf ein anderes Angebot](billing-how-to-switch-azure-offer.md).

### <a name="can-i-transfer-a-subscription-to-a-user-account-in-another-country"></a>Kann ein Abonnement auf ein Benutzerkonto in einem anderen Land übertragen werden?

Nein. Das Übertragen eines Abonnements auf ein Benutzerkonto in einem anderen Land wird nicht unterstützt. Das Benutzerkonto des Empfängers muss sich im gleichen Land befinden.

### <a name="can-the-recipient-use-a-different-payment-method"></a>Kann der Empfänger eine andere Zahlungsmethode nutzen?

Ja. Der Abrechnungsverlauf des Abonnements ist aber auf zwei Konten aufgeteilt.  

### <a name="is-the-payment-method-impacted-after-i-transferred-an-azure-subscription"></a>Hat die Übertragung eines Azure-Abonnements Auswirkungen auf die Zahlungsmethode?

Damit eine Abonnementübertragung akzeptiert wird, muss eine Kreditkarte oder eine ähnliche Zahlungsmethode für das Abonnement angegeben werden. Beispiel: Wenn Bob ein Abonnement auf Jane überträgt und Jane die Übertragung akzeptiert, muss Jane auch eine Zahlungsmethode für die Bezahlung des Abonnements angeben. Nachdem die Übertragung abgeschlossen ist, wird das Abonnement Jane in Rechnung gestellt, nicht Bob.

### <a name="how-do-i-migrate-data-and-services-for-my-azure-subscription-to-new-subscription"></a>Wie migriere ich Daten und Dienste für mein Azure-Abonnement in ein neues Abonnement?

Wenn Sie ein Abonnement nicht übertragen können, können Sie Ihre Ressourcen manuell migrieren. Informationen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../azure-resource-manager/resource-group-move-resources.md).

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Wenden Sie sich an den Support.

[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls Sie weitere Hilfe benötigen, um das Problem schnell beheben zu lassen.
