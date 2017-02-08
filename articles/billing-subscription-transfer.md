---
title: "Übertragen eines Azure-Abonnements | Microsoft-Dokumentation"
description: "Übertragung eines Azure-Abonnements auf einen anderen Benutzer, und einige häufig gestellte Fragen (FAQs) zu dem Prozess."
services: 
documentationcenter: 
author: genlin
manager: narmstr
editor: 
tags: billing,top-support-issue
ms.assetid: c8ecdc1e-c9c5-468c-a024-94ae41e64702
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/8/2016
ms.author: genli
translationtype: Human Translation
ms.sourcegitcommit: f7b097883ca0340a3af83fc9531acfe571295994
ms.openlocfilehash: c0323a63676a784660146f84a7813cf2cd5de7b0


---
# <a name="transferring-ownership-of-an-azure-subscription"></a>Übertragen eines Azure-Abonnements

Sie können Ihr Abonnement auf einfache Weise auf einen anderen Benutzer übertragen. Dies ist für Abonnements vom Typ „Nutzungsbasierte Bezahlung“, „MSDN“, „Action Pack“ und „BizSpark“ möglich. Sie können den Kontoadministrator für ein beliebiges Ihrer Abonnements mit nutzungsbasierter Bezahlung bzw. MSDN-, Action Pack- oder BizSpark-Abonnements ändern. Dabei ist es unerheblich, in welchem Land Sie sich befinden. Wir unterstützen die Übertragung von Azure Marketplace-Einkäufen für diese Abonnementtypen. 

Es kann beispielsweise folgende Gründe haben, warum Sie den Besitz eines Azure-Abonnements übertragen möchten:

* Sie müssen die Abrechnung für Ihr Azure-Abonnement an eine andere Person übergeben.
* Sie möchten das Konto wechseln, das Sie für die Anmeldung in Azure verwenden. Unter Umständen haben Sie Ihr Microsoft-Konto verwendet, wollten aber eigentlich Ihr Geschäfts-, Schul- oder Unikonto verwenden.
* Sie möchten Ihr Azure-Abonnement von einem Verzeichnis in ein anderes verschieben.
* Sie verwenden Azure und Office 365 unter verschiedenen Mandanten und möchten diese Komponenten zusammenfassen.

Wie Sie Ihr Abonnement in ein anderes Angebot ändern können, erfahren Sie unter [Umstellen Ihres Azure-Abonnements auf ein anderes Angebot](billing-how-to-switch-azure-offer.md). 

## <a name="how-to-transfer-ownership-of-an-azure-subscription"></a>Übertragen eines Azure-Abonnements
> [!VIDEO https://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/Transfer-an-Azure-subscription/player]
>
>

1. Melden Sie sich unter <https://account.windowsazure.com/Subscriptions> an. Sie müssen der Kontoadministrator sein, um ein Abonnement an einen anderen Benutzer zu übertragen. Wie Sie herausfinden, wer der Kontoadministrator des Abonnements ist, erfahren Sie unter [Häufig gestellte Fragen (FAQ)](#faq).
2. Wählen Sie das Abonnement, das Sie übertragen möchten.
3. Klicken Sie auf die Option **Abonnement übertragen** .

   ![Registerkarte „Azure-Kontoabonnements“](./media/billing-subscription-transfer/image1.png)
4. Folgen Sie den Anweisungen, um den Empfänger anzugeben.

   ![Dialogfeld „Abonnement übertragen“](./media/billing-subscription-transfer/image2.PNG)
5. Der Empfänger erhält automatisch eine E-Mail mit einem Link für den Vorgang zum Akzeptieren.

   ![E-Mail zur Abonnementübertragung an den Empfänger](./media/billing-subscription-transfer/image3.png)
6. Der Empfänger klickt auf den Link und folgt den Anweisungen. Außerdem gibt er seine Zahlungsinformationen ein.

   ![Erste Webseite zur Abonnementübertragung](./media/billing-subscription-transfer/image4.png)

   ![Zweite Webseite zur Abonnementübertragung](./media/billing-subscription-transfer/image5.png)
7. Erfolg! Das Abonnement ist jetzt übertragen.

<a id="faq"></a>

## <a name="frequently-asked-questions-faq"></a>Häufig gestellte Fragen (FAQ)
* <a name="whoisaa"></a> **Wer ist der Kontoadministrator des Abonnements?**

  Der Kontoadministrator ist die Person, die sich für das Azure-Abonnement registriert bzw. die es erworben hat. Die Person ist zum Zugreifen auf das [Kontocenter](https://account.windowsazure.com/Home/Index) und Durchführen verschiedener Verwaltungsaufgaben berechtigt, z.B. Erstellen von Abonnements, Kündigen von Abonnements, Ändern der Abrechnung für ein Abonnement oder Ändern des Dienstadministrators. Verwenden Sie die folgenden Schritte für die Ermittlung, wenn Sie nicht sicher sind, wer der Kontoadministrator für ein Abonnement ist.

  1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
  2. Wählen Sie im Menü „Hub“ die Option **Abonnement** aus.
  3. Wählen Sie das zu überprüfende Abonnement aus, und sehen Sie unter **Einstellungen** nach.
  4. Wählen Sie **Eigenschaften** aus. Der Kontoadministrator des Abonnements wird im Feld **Kontoadministrator** angezeigt.  
* **Führt eine Übertragung des Abonnements zu Ausfallzeiten?**

  Die Übertragung hat keine Auswirkung auf den Dienst. Durch die Übertragung des Abonnements wird das Abonnement unter dem aktuellen Kontoadministrator gekündigt und ein Abonnement unter dem Konto des Empfängers erstellt. Das neue Abonnement wird den zugrunde liegenden Azure-Diensten zugeordnet. Die Abonnement-ID bleibt unverändert.
* **Wie verwende ich diesen Prozess zum Ändern des Verzeichnisses für ein Abonnement?**   
  Azure-Abonnements werden in dem Verzeichnis erstellt, zu dem der Kontoadministrator gehört. Übertragen Sie das Abonnement auf ein Benutzerkonto im Zielverzeichnis, um das Verzeichnis zu ändern. Wenn der Benutzer die Schritte zum Akzeptieren der Übertragung abschließt, wird das Abonnement automatisch in das Zielverzeichnis verschoben.
* **Kann eine andere Organisation, von der ich die Abrechnung eines Abonnements übernehme, weiterhin auf meine Ressourcen zugreifen?**

  Wenn das Abonnement auf einen anderen Mandanten übertragen wird, verlieren die Benutzer, die dem vorherigen Mandanten zugeordnet sind, den Zugriff auf das Abonnement. Selbst wenn ein Benutzer kein Dienstadministrator oder Co-Administrator mehr ist, hat er über andere Sicherheitsmechanismen möglicherweise immer noch Zugriff auf das Abonnement. Diese umfassen:

  * Verwaltungszertifikate, die dem Benutzer Administratorrechte auf Abonnementressourcen gewähren. Weitere Informationen finden Sie unter [Erstellen und Hochladen eines Verwaltungszertifikats für Azure](https://msdn.microsoft.com/library/azure/gg551722.aspx)
  * Tastenkombinationen für Dienste wie Storage. Weitere Informationen finden Sie unter [Informationen zu Azure-Speicherkonten](storage/storage-create-storage-account.md).
  * RAS-Anmeldeinformationen für Dienste wie Azure Virtual Machines

  Dies ist keine vollständige Liste. Der Empfänger sollte sich überlegen, ob er dem Dienst zugeordnete Schlüssel aktualisiert, wenn der Zugriff auf die Ressourcen eingeschränkt werden soll. Die meisten Ressourcen können wie folgt aktualisiert werden:

  1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
  2. Wählen Sie im Menü „Hub“ die Option **Alle Ressourcen** aus.
  3. Wählen Sie die Ressource. 
  4. Klicken Sie im Blatt "Ressourcen" auf **Einstellungen**. Hier können Sie die vorhandenen Schlüssel anzeigen und aktualisieren.
* **Wenn ich das Abonnement in der Mitte des Abrechnungszyklus übertrage, zahlt der Empfänger die Rechnung für den gesamten Zyklus?**

  Der Absender ist zuständig für die Zahlung für jegliche Nutzung, die bis zu dem Zeitpunkt gemeldet wurde, an dem die Übertragung abgeschlossen war. Der Empfänger ist verantwortlich für die Verwendung, die vom Zeitpunkt der Übertragung an berichtet wird. Möglicherweise gibt es Nutzung, die vor der Übertragung stattgefunden hat, aber danach gemeldet wurde. Diese Nutzung ist in der Rechnung des Empfängers enthalten.
* **Verfügt der Empfänger über Zugriff auf den Nutzungs- und Abrechnungsverlauf?**

  Die einzige verfügbare Information für den Empfänger ist der Betrag der letzten Rechnung oder, wenn das Abonnement vor dem Generieren der ersten Rechnung übertragen wurde, der derzeitige Kontostand. Der restliche Nutzungs- und Abrechnungsverlauf wird nicht mit dem Abonnement übertragen.
* **Kann das Angebot während der Übertragung geändert werden?**

  Das Angebot muss unverändert bleiben. Um Ihr Angebot zu ändern, müssen Sie sich [an den Support wenden](http://go.microsoft.com/fwlink/?LinkID=619338).
* **Kann ein Abonnement auf ein Benutzerkonto in einem anderen Land übertragen werden?**

  Nein. Das Übertragen eines Abonnements auf ein Benutzerkonto in einem anderen Land wird nicht unterstützt. Das Benutzerkonto des Empfängers muss sich im gleichen Land befinden.
* **Kann der Empfänger eine andere Zahlungsmethode nutzen?**

  Ja. Der Abrechnungsverlauf des Abonnements ist aber auf zwei Konten aufgeteilt.  

* **Hat die Übertragung eines Azure-Abonnements Auswirkungen auf die Zahlungsmethode?**

Damit eine Abonnementübertragung akzeptiert wird, muss eine Kreditkarte oder eine ähnliche Zahlungsmethode für das Abonnement angegeben werden. Beispiel: Wenn Bob ein Abonnement auf Jane überträgt und Jane die Übertragung akzeptiert, muss Jane auch eine Zahlungsmethode angeben, die sie zur Bezahlung des Abonnements verwendet. Nachdem die Übertragung abgeschlossen ist, wird das Abonnement, das Bob auf Jane übertragen hat, nicht mehr über ihn abgerechnet.

* **Wie migriere ich Daten und Dienste für mein Azure-Abonnement in ein neues Abonnement?**

  Informationen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](./azure-resource-manager/resource-group-move-resources.md).

## <a name="next-steps-after-accepting-ownership-of-a-subscription"></a>Nächste Schritte nach dem Übernehmen des Besitzes eines Abonnements
1. Sie sind nun der Kontoadministrator. Überprüfen und aktualisieren Sie den Dienstadministrator und die Co-Administratoren. Sie verwalten Administratoren im [klassischen Azure-Portal](https://manage.windowsazure.com) unter „Einstellungen“. [Weitere Informationen](http://go.microsoft.com/fwlink/?LinkID=533293).
2. Sie können auch die rollenbasierte Zugriffskontrolle (RBAC) für Ihr Abonnement und die Dienste verwenden. Besuchen Sie das [Azure-Portal](https://portal.azure.com) [Weitere Informationen zur RBAC](http://go.microsoft.com/fwlink/?LinkID=544802)
3. Aktualisieren Sie die Anmeldeinformationen für die Dienste dieses Abonnements. Diese umfassen:
   * Verwaltungszertifikate, die dem Benutzer Administratorrechte auf Abonnementressourcen gewähren. Weitere Informationen finden Sie unter [Erstellen und Hochladen eines Verwaltungszertifikats für Azure](https://msdn.microsoft.com/library/azure/gg551722.aspx)
   * Tastenkombinationen für Dienste wie Storage. Weitere Informationen finden Sie unter [Informationen zu Azure-Speicherkonten](storage/storage-create-storage-account.md).
   * RAS-Anmeldeinformationen für Dienste wie Azure Virtual Machines
4. Aktualisieren Sie die Abrechnungswarnungen für dieses Abonnement im [Azure-Kontocenter](https://account.windowsazure.com/Subscriptions). [Weitere Informationen](http://go.microsoft.com/fwlink/?LinkID=533292)
5. Wenn Sie mit einem Partner arbeiten, sollten Sie die Partner-ID in diesem Abonnement aktualisieren. Sie können die Partner-ID im [Azure-Kontocenter](https://account.windowsazure.com/Subscriptions) aktualisieren.


## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Wenden Sie sich an den Support.
[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls Sie weitere Hilfe benötigen, um das Problem schnell beheben zu lassen. 





<!--HONumber=Dec16_HO2-->


