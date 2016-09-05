<properties
   pageTitle="Übertragen eines Azure-Abonnements | Microsoft Azure"
   description="Übertragung eines Azure-Abonnements auf einen anderen Benutzer, und einige häufig gestellte Fragen (FAQs) zu dem Prozess."
   services=""
   documentationCenter=""
   authors="genlin"
   manager="stevenpo"
   editor=""
   tags="billing,top-support-issue"/>

<tags
   ms.service="billing"
   ms.workload="na"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/17/2016"
   ms.author="genli"/>

# Übertragen eines Azure-Abonnements

Möchten Sie:

- die Abrechnung für Ihr Azure-Abonnement an eine andere Person übergeben?
- das Konto wechseln, das Sie für die Anmeldung in Azure verwenden? Vielleicht haben Sie Ihr Microsoft-Konto verwendet, wollten aber eigentlich Ihr Geschäfts- oder Schulkonto verwenden?
- Ihre Azure-Abonnement von einem Verzeichnis in ein anderes verschieben?
- Azure und Office 365, die in verschiedenen Mandanten sind, konsolidieren?

Sie können dies jetzt problemlos für Abonnements mit nutzungsbasierter Bezahlung sowie für MSDN-, Action Pack- und BizSpark-Abonnements im Microsoft Azure-Kontocenter tun. Wir haben die Möglichkeit hinzugefügt, Ihr Abonnement auf einen anderen Benutzer zu übertragen. Anders ausgedrückt können Sie jetzt das Administratorkonto für ein beliebiges Ihrer nutzungsbasierten Abonnements bzw. MSDN-, Action Pack- oder BizSpark-Abonnements ändern. Dabei ist es unerheblich, in welchem Land Sie sich befinden. Wir unterstützen jetzt auch die Übertragung von Azure Marketplace-Einkäufen für diese Abonnementtypen.

> [AZURE.NOTE]  Wie Sie Ihr Abonnement in ein anderes Angebot ändern können, erfahren Sie unter [Umstellen Ihres Azure-Abonnements auf ein anderes Angebot](billing-how-to-switch-azure-offer.md).

> [AZURE.NOTE] Wenn Sie bei irgendeinem Verfahren in diesem Artikel weitere Hilfe benötigen, [wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um das Problem schnell zu lösen.

> Sie müssen der Kontoadministrator sein, um ein Abonnement an einen anderen Benutzer zu übertragen. Wie Sie herausfinden, wer der Kontoadministrator des Abonnements ist, erfahren Sie unter [Häufig gestellte Fragen (FAQ)](#faq).

## Übertragen eines Azure-Abonnements

> [AZURE.VIDEO transfer-an-azure-subscription]

1.  Melden Sie sich bei <https://account.windowsazure.com/Subscriptions> an.

2.  Wählen Sie das Abonnement, das Sie übertragen möchten.

3.  Klicken Sie auf die Option **Abonnement übertragen**.

    ![Registerkarte „Azure-Kontoabonnements“](./media/billing-subscription-transfer/image1.png)

4.  Folgen Sie den Anweisungen, um den Empfänger anzugeben.

    ![Dialogfeld „Abonnement übertragen“](./media/billing-subscription-transfer/image2.PNG)

5.  Der Empfänger erhält automatisch eine E-Mail mit einem Link zum Akzeptieren.

    ![E-Mail zur Abonnementübertragung an den Empfänger](./media/billing-subscription-transfer/image3.png)

6.  Der Empfänger klickt auf den Link und folgt den Anweisungen. Außerdem gibt er seine Zahlungsinformationen ein.

    ![Erste Webseite zur Abonnementübertragung](./media/billing-subscription-transfer/image4.PNG)

    ![Zweite Webseite zur Abonnementübertragung](./media/billing-subscription-transfer/image5.PNG)

7. Erfolg! Das Abonnement ist jetzt übertragen.

<a id="faq"></a>
## Häufig gestellte Fragen (FAQ)

-   **Wie kann ich überprüfen, wer der Kontoadministrator des Abonnements ist?**

    Sie können wie folgt überprüfen, wer der Kontoadministrator des Abonnements ist:

    1. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com) an.
    2. Wählen Sie im Menü „Hub“ die Option **Abonnement** aus.
    3. Wählen Sie das zu überprüfende Abonnement und dann **Einstellungen** aus.
    4. Wählen Sie **Eigenschaften** aus. Der Kontoadministrator des Abonnements wird im Feld **Kontoadministrator** angezeigt.

-   **Führt eine Übertragung des Abonnements zu Ausfallzeiten?**

    Die Übertragung hat keine Auswirkung auf den Dienst. Effektiv wird dabei das Abonnement für den aktuellen Kontoadministrator gekündigt und ein neues im Konto des Empfängers erstellt. Dabei werden die zugrunde liegenden Azure-Dienste dem neuen Abonnement zugeordnet. Die Abonnement-ID bleibt unverändert.

-   **Wie verwende ich diesen Mechanismus zum Ändern des Verzeichnisses für ein Abonnement?** – Ein Azure-Abonnement wird in dem Verzeichnis erstellt, zu dem das Administratorkonto gehört. Um das Verzeichnis zu ändern, müssen Sie also nur das Abonnement auf ein Benutzerkonto im Zielverzeichnis übertragen. Wenn der Benutzer die Schritte zum Akzeptieren der Übertragung abschließt, wird das Abonnement automatisch in das Zielverzeichnis verschoben.

-   **Wenn ich die Abrechnung eines Abonnements aus einer anderen Organisation übernehme, kann diese weiterhin auf meine Ressourcen zugreifen?**

    Wenn das Abonnement auf einen anderen Mandanten übertragen wird, verlieren die Benutzer, die dem vorherigen Mandanten zugeordnet sind, den Zugriff auf das Abonnement. Selbst wenn ein Benutzer kein Dienstadministrator oder Co-Administrator mehr ist, hat er über andere Sicherheitsmechanismen möglicherweise immer noch Zugriff auf das Abonnement. Diese umfassen:
    - Verwaltungszertifikate, die dem Benutzer Administratorrechte auf Abonnementressourcen gewähren. Weitere Informationen finden Sie unter [Erstellen und Hochladen eines Verwaltungszertifikats für Azure](https://msdn.microsoft.com/library/azure/gg551722.aspx).
    -	Tastenkombinationen für Dienste wie Storage. Weitere Informationen finden Sie unter [Anzeigen, Kopieren und erneutes Generieren von Speicherzugriffsschlüsseln](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
    -	RAS-Anmeldeinformationen für Dienste wie Azure Virtual Machines

    Dies ist keine vollständige Liste. Der Empfänger sollte sich überlegen, ob er dem Dienst zugeordnete Schlüssel aktualisiert, wenn der Zugriff auf die Ressourcen eingeschränkt werden soll. Die meisten Ressourcen können wie folgt aktualisiert werden:

    1.   Öffnen Sie das Azure-Portal unter [*https://portal.azure.com*](https://portal.azure.com).

    2.    Klicken Sie auf alle „Browse All -&gt; All Resources“.

    3.    Wählen Sie die Ressource. Daraufhin wird das Blatt "Ressourcen" geöffnet.

    4.    Klicken Sie im Blatt "Ressourcen" auf **Einstellungen**. Hier können Sie die vorhandenen Schlüssel anzeigen und aktualisieren.


-   **Wenn ich das Abonnement in der Mitte des Abrechnungszyklus übertrage, zahlt der Empfänger die Rechnung für den gesamten Zyklus?**

    Der Absender ist zuständig für die Zahlung für jegliche Nutzung, die bis zu dem Zeitpunkt gemeldet wurde, an dem die Übertragung abgeschlossen war. Der Empfänger ist verantwortlich für die Verwendung, die vom Zeitpunkt der Übertragung an berichtet wird. Möglicherweise gibt es Nutzung, die vor der Übertragung stattgefunden hat, aber danach gemeldet wurde. Dies wird in der Rechnung des Empfängers enthalten sein.

-   **Verfügt der Empfänger über Zugriff auf den Nutzungs- und Abrechnungsverlauf?**

    Zu diesem Zeitpunkt ist die einzige Information, die dem Empfänger angezeigt wird, der Betrag der letzten Rechnung (oder der derzeitige Kontostand, wenn das Abonnement übertragen wurde, bevor die erste Rechnung generiert wurde). Der restliche Nutzungs- und Abrechnungsverlauf wird nicht mit dem Abonnement übertragen.

-   **Kann das Angebot während der Übertragung geändert werden?**

    Das Angebot muss unverändert bleiben. Um Ihr Angebot zu ändern, müssen Sie sich [an den Support wenden](http://go.microsoft.com/fwlink/?LinkID=619338).

-   **Kann ein Abonnement auf ein Benutzerkonto in einem anderen Land übertragen werden?**

    Nein, zu diesem Zeitpunkt wird dies nicht unterstützt. Das Benutzerkonto des Empfängers muss sich im gleichen Land befinden.

-   **Kann der Empfänger einen anderen Zahlungsmechanismus nutzen?**

    Ja. Es gibt hier Einschränkungen: der Abrechnungsverlauf des Abonnements ist nun auf zwei Konten aufgeteilt. Der Vorteil ist jedoch, dass Sie sich dazu nicht an den [Support wenden](http://go.microsoft.com/fwlink/?LinkID=619338) müssen.

-   **Hat die Übertragung eines Azure-Abonnements Auswirkungen auf die Zahlungsmethode?**

    Damit eine Abonnementübertragung akzeptiert wird, muss eine Kreditkarte oder eine ähnliche Zahlungsmethode für das Abonnement angegeben werden. Beispiel: Wenn Bob ein Abonnement auf Jane überträgt und Jane die Übertragung akzeptiert, muss Jane auch eine Zahlungsmethode angeben, die sie zur Bezahlung des Abonnements verwendet. Nachdem die Übertragung abgeschlossen ist, wird das Abonnement, das Bob auf Jane übertragen hat, nicht mehr über ihn abgerechnet.

## Nächste Schritte nach dem Übernehmen des Besitzes eines Abonnements

1. Sie sind nun der Kontoadministrator. Überprüfen und aktualisieren Sie den Dienstadministrator und die Co-Administratoren. Sie verwalten Administratoren im [klassischen Azure-Portal](https://manage.windowsazure.com) unter „Einstellungen“. [Weitere Informationen](http://go.microsoft.com/fwlink/?LinkID=533293).
2. Sie können auch die rollenbasierte Zugriffskontrolle (RBAC) für Ihr Abonnement und die Dienste verwenden. Besuchen Sie das [Azure-Portal](https://portal.azure.com) [Weitere Informationen zur RBAC](http://go.microsoft.com/fwlink/?LinkID=544802)
3. Aktualisieren Sie die Anmeldeinformationen für die Dienste dieses Abonnements. Diese umfassen:
    - Verwaltungszertifikate, die dem Benutzer Administratorrechte auf Abonnementressourcen gewähren. Weitere Informationen finden Sie unter [Erstellen und Hochladen eines Verwaltungszertifikats für Azure](https://msdn.microsoft.com/library/azure/gg551722.aspx).
    -	Tastenkombinationen für Dienste wie Storage. Weitere Informationen finden Sie unter [Anzeigen, Kopieren und erneutes Generieren von Speicherzugriffsschlüsseln](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
    -	RAS-Anmeldeinformationen für Dienste wie Azure Virtual Machines
4. Aktualisieren Sie die Abrechnungswarnungen für dieses Abonnement im [Azure-Kontocenter](https://account.windowsazure.com/Subscriptions). [Weitere Informationen](http://go.microsoft.com/fwlink/?LinkID=533292).
5. 	Wenn Sie mit einem Partner arbeiten, sollten Sie die Partner-ID in diesem Abonnement aktualisieren. Sie können diese im [Azure Account Center](https://account.windowsazure.com/Subscriptions) durchführen.

> [AZURE.NOTE] Wenn Sie weitere Fragen haben, [wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um das Problem schnell zu lösen.

<!---HONumber=AcomDC_0824_2016-->