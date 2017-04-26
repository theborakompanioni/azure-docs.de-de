---
title: "Gewusst wie: Erstellen eines Microsoft-Entwicklerkontos für die Veröffentlichung in Azure Marketplace | Microsoft-Dokumentation"
description: "In diesem Artikel wird das Erstellen eines Microsoft-Entwicklerkontos für die Veröffentlichung in Azure Marketplace erläutert."
services: cloud-partner-portal
documentationcenter: 
author: rupeshazure
manager: hamidm
ms.service: cloud-partner-portal
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/03/2017
ms.author: rupesk@microsoft.com
translationtype: Human Translation
ms.sourcegitcommit: db7cb109a0131beee9beae4958232e1ec5a1d730
ms.openlocfilehash: edad5a8a2acb2767dc59b51429ae3fc581f74947
ms.lasthandoff: 04/18/2017


---
# <a name="create-a-microsoft-developer-account"></a>Erstellen eines Microsoft-Entwicklerkontos
In diesem Artikel werden die Schritte zur Erstellung und Registrierung eines Kontos erklärt, die notwendig sind, um zugelassener Microsoft-Entwickler für die Veröffentlichung in Azure Marketplace zu werden.

## <a name="1-create-a-microsoft-account"></a>1. Erstellen eines Microsoft-Kontos
Am Anfang des Veröffentlichungsprozesses steht die Registrierung im **Microsoft Developer Center**.Sie nutzen das hier registrierte Konto im **[Cloudpartnerportal](https://cloudpartner.azure.com/)**, um den Veröffentlichungsprozess zu starten. Sie sollten für Ihre Azure Marketplace-Angebote nur ein Microsoft-Konto verwenden. Verwenden Sie keine unterschiedlichen Konten für verschiedene Dienste oder Angebote.

Die Adresse, die für den Benutzernamen maßgeblich ist, sollte sich in Ihrer Domäne befinden und von Ihrem IT-Team gesteuert werden. Alle auf die Veröffentlichung bezogenen Aktivitäten sollten über dieses Konto durchgeführt werden.

> [!WARNING]
> Wörter wie **„Azure“** und **„Microsoft“** werden bei der Registrierung eines Microsoft-Kontos nicht unterstützt. Vermeiden Sie diese Wörter, um den Prozess der Kontoerstellung und -registrierung abzuschließen.
>
>

### <a name="guidelines-for-company-accounts"></a>Richtlinien für Unternehmenskonten
Beachten Sie beim Erstellen eines Unternehmenskontos diese Richtlinien, wenn mehrere Personen auf das Konto zugreifen müssen, indem sie sich bei dem Microsoft-Konto anmelden, das das Konto geöffnet hat.

> [!Important]
> Wichtig: Um mehreren Benutzern Zugriff auf Ihr Dev Center-Konto zu gewähren, sollten Sie über Azure Active Directory einzelnen Benutzern, die auf das Konto zugreifen können, indem sie sich mit ihren jeweiligen Azure AD-Anmeldeinformationen anmelden, Rollen zuweisen. Weitere Informationen finden Sie unter [Verwalten von Kontobenutzern](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users).

* Erstellen Sie Ihr Microsoft-Konto mit einer E-Mail-Adresse, die zur Domäne Ihres Unternehmens gehört, jedoch nicht einer Einzelperson – z.B. windowsapps@fabrikam.com.
* Beschränken Sie den Zugriff auf dieses Microsoft-Konto auf die kleinste mögliche Anzahl von Entwicklern.
* Richten Sie eine Unternehmens-E-Mail-Verteilerliste ein, die alle Personen enthält, die auf das Entwicklerkonto zugreifen müssen, und fügen Sie diese E-Mail-Adresse Ihren Sicherheitsinfos hinzu. So können alle Mitarbeiter auf der Liste bei Bedarf Sicherheitscodes erhalten und die Sicherheitsinfos Ihres Microsoft-Kontos verwaltet werden. Wenn das Einrichten einer Verteilerliste nicht möglich ist, muss der Besitzer des einzelnen E-Mail-Kontos bei Aufforderung verfügbar sein, um auf den Sicherheitscode zuzugreifen und ihn zu teilen (z.B. wenn dem Konto neue Sicherheitsinfos hinzugefügt werden, oder wenn ein neues Gerät darauf zugreifen muss).
* Fügen Sie eine Telefonnummer hinzu, die keine Durchwahl benötigt, und auf die wichtige Teammitglieder zugreifen können.
* Sorgen Sie im Allgemeinen dafür, dass Entwickler vertrauenswürdige Geräten verwenden, um sich beim Entwicklerkonto Ihres Unternehmens anzumelden. Alle wichtigen Teammitglieder sollten auf diese vertrauenswürdigen Geräten zugreifen können. Dies reduziert die Notwendigkeit, beim Zugriff auf das Konto Sicherheitscodes senden zu müssen.
* Wenn Sie Zugriff auf das Konto von einem nicht vertrauenswürdigen PC zulassen müssen, begrenzen Sie den Zugriff auf ein Maximum von fünf Entwicklern. Im Idealfall sollten diese Entwickler mit Computern auf das Konto zugreifen, die den gleichen geografischen und Netzwerkstandort haben.
* Überprüfen Sie die Sicherheitsinfos für Ihr Unternehmen regelmäßig unter [https://account.live.com/proofs/Manage](https://account.live.com/proofs/Manage), um sicherzustellen, dass sie aktuell sind.

Auf Ihr Entwicklerkonto sollte in erster Linie mit vertrauenswürdigen PCs zugegriffen werden. Dies ist wichtig, da die Anzahl der pro Konto und Woche generierten Codes beschränkt ist. Darüber hinaus ermöglicht dies die nahtloseste Anmeldeerfahrung.

Um weitere Informationen zu zusätzlichen Entwicklerkontorichtlinien und zur Sicherheit zu erhalten, klicken Sie [hier](https://msdn.microsoft.com/windows/uwp/publish/opening-a-developer-account#additional-guidelines-for-company-accounts).

### <a name="instructions"></a>Anleitung
1. Öffnen Sie eine neue Chrome-Inkognito- oder Internet Explorer-InPrivate-Browsersitzung, um sicherzustellen, dass Sie nicht bei einem vorhandenen Konto angemeldet sind.
2. Registrieren Sie die E-Mail-Adresse (gemäß obiger Richtlinien, z.B. windowsapp@fabrikam.com) über den Link [https://signup.live.com/signup.aspx](https://signup.live.com/signup.aspx) als Microsoft-Konto. Befolgen Sie die nachstehenden Anweisungen.

       A. During registering your account as a Microsoft account, you need to provide a valid phone number for the system to send you an account verification code as a text message or an automated call.

       B. During registering your account as a Microsoft account, you need to provide a valid email id for receiving an automated email for account verification.

    C. Verifizieren Sie die an die Verteilerliste gesendete E-Mail-Adresse.

    D. Sie können das neue Microsoft-Konto jetzt im Microsoft Developer Center verwenden.

## <a name="2-register-your-account-in-microsoft-developer-center"></a>2. Registrieren Ihres Kontos beim Microsoft Developer Center

Das Microsoft Developer Center wird für die einmalige Registrierung der Unternehmensinformationen verwendet. Die Person, die das Konto registriert, muss ein offizieller Vertreter des Unternehmens sein und ihre persönlichen Informationen zum Zweck der Identitätsüberprüfung bereitstellen. Diese Person muss ein Microsoft-Konto verwenden, das für das Unternehmen freigegeben ist. **Das gleiche Konto muss im Cloudpartnerportal verwendet werden.** Sie sollten überprüfen, ob Ihr Unternehmen nicht bereits über ein Microsoft Developer Center-Konto verfügt, bevor Sie ein Konto erstellen. Während des Prozesses erfassen wir Informationen zu Unternehmensadresse, Bankdaten und Steuer. Diese Informationen erhalten Sie üblicherweise über Bank- oder Geschäftskontakte.

> [!IMPORTANT]
> Sie müssen folgende Komponenten des Entwicklerprofils ausfüllen, um die verschiedenen Phasen der Angebotserstellung und -bereitstellung durchlaufen zu können.
>
>

| Entwicklerprofil | Entwurf starten | Staging | Kostenlos veröffentlichen und Lösungsvorlage | Für den Erwerb veröffentlichen |
| --- | --- | --- | --- | --- |
| Unternehmensregistrierung |Verpflichtend |Verpflichtend |Verpflichtend |Verpflichtend |
| Steuer-ID |Optional |Optional |Optional |Verpflichtend |
| Bankkonto |Optional |Optional |Optional |Verpflichtend |

> [!NOTE]
> Bring Your Own License (BYOL) wird nur für virtuelle Computer unterstützt und gilt als ein **kostenloses** Angebot.
>
>

### <a name="register-your-company-account"></a>Registrieren Ihres Unternehmenskontos
Schritt 1: Öffnen Sie eine neue Internet Explorer-InPrivate- oder Chrome-Inkognito-Browsersitzung, um sicherzustellen, dass Sie nicht bei einem persönlichen Konto angemeldet sind.

Schritt 2: Wechseln Sie zu [http://dev.windows.com/registration?accountprogram=azure](http://dev.windows.com/registration?accountprogram=azure) , um sich im Developer Center als Verkäufer zu registrieren. Bitte lesen Sie den folgenden wichtigen Hinweis, bevor Sie fortfahren.

![Abbildung][img-verify]

   > [!IMPORTANT]
   > Stellen Sie sicher, dass die E-Mail-ID oder Verteilerliste (eine Verteilerliste wird empfohlen, um die Abhängigkeit von einzelnen Personen aufzuheben), die Sie für die Registrierung im Developer Center verwenden werden, zunächst als Microsoft-Konto registriert wird. Falls nicht, dann registrieren Sie sich bitte über diesen [Link](https://signup.live.com/signup?uaid=e479342fe2824efeb0c3d92c8f961fd3&lic=1). Darüber hinaus kann für die Developer Center-Registrierung **keine E-Mail-ID unter der Microsoft-Unternehmensdomäne ( d.h. @microsoft.com) verwendet werden**.
   >
   >

![Dev Center-Anmeldung](./media/cloud-partner-portal-create-dev-center-registration/seller-dashboard-login.jpg)

Schritt 3: Führen Sie den Assistenten "Unterstützen Sie uns beim Schutz Ihres Kontos" vollständig durch. Damit wird Ihre Identität anhand einer Telefonnummer oder E-Mail-Adresse überprüft.

Schritt 4: Wählen Sie im Abschnitt „Registrierungskontoinformationen“ im Dropdownmenü **Land/Region des Kontos** die für Sie zutreffende Option aus.

 ![Abbildung](./media/cloud-partner-portal-create-dev-center-registration/imgRegisterCo_04.png)

   > [!WARNING]
   > **Verkäuferländer:** Um Ihre Dienste im Azure Marketplace verkaufen zu können, muss die registrierte Organisation in einem der obigen genehmigten Verkäuferländer ansässig sein. Diese Einschränkung hat auszahlungstechnische und steuerliche Gründe. Die Länderliste wird in Kürze erweitert, prüfen Sie sie also regelmäßig. Weitere Informationen finden Sie in den [Marketplace-Teilnahmerichtlinien](http://go.microsoft.com/fwlink/?LinkID=526833).
   >


Schritt 5: Wählen Sie als „Kontotyp“ **Unternehmen** aus, und klicken Sie dann auf die Schaltfläche **Weiter**.

   > [!IMPORTANT]
   > Lesen Sie die Seite [Kontotypen, Standorte und Gebühren](https://msdn.microsoft.com/library/windows/apps/jj863494.aspx)
   >
   >

   ![Abbildung](./media/cloud-partner-portal-create-dev-center-registration/imgRegisterCo_05.png)

Schritt 6: Geben Sie den **Anzeigenamen des Herausgebers** ein. Dies ist normalerweise der Name Ihres Unternehmens.

   > [!TIP]
   > Der im Developer Center eingegebene Anzeigename des Herausgebers wird im Azure Marketplace nicht angezeigt, sobald Ihr Angebot aufgeführt wird. Sie müssen dieses Feld aber ausfüllen, um den Registrierungsprozess abzuschließen.
   >


Schritt 7 Geben Sie die **Kontaktinformationen** für die Kontoüberprüfung ein.

   > [!IMPORTANT]
   > Es ist erforderlich, dass Sie genaue Kontaktinformationen angeben. Sie werden in unserem Überprüfungsprozess für Ihr Unternehmen und seine Genehmigung im Developer Center verwendet.
   >


Schritt 8: Geben Sie die Kontaktinformationen für den **Genehmiger des Unternehmens**ein. Der Genehmiger des Unternehmens ist die Person, die gewährleisten kann, dass Sie berechtigt sind, im Developer Center im Auftrag Ihres Unternehmens ein Konto zu erstellen. Klicken Sie nach Abschluss dieses Vorgangs auf **Weiter**, um mit dem Abschnitt **Zahlung** fortzufahren.

   ![Abbildung](./media/cloud-partner-portal-create-dev-center-registration/imgRegisterCo_08.png)

Schritt 9 Geben Sie Ihre Zahlungsinformationen für Ihr Konto ein. Wenn Sie einen Aktionscode haben, der die Kosten der Registrierung abdeckt, können Sie ihn hier eingeben. Geben Sie andernfalls Ihre Kreditkartendaten an (oder PayPal-Daten, wenn dies für Ihren Markt unterstützt wird). Klicken Sie nach Abschluss dieses Vorgangs auf **Weiter**, um mit dem Bildschirm **Überprüfen** fortzufahren.

  ![Abbildung](./media/cloud-partner-portal-create-dev-center-registration/imgRegisterCo_09.png)

Schritt 10 Überprüfen Sie Ihre Kontoinformationen, und bestätigen Sie, dass alles korrekt ist. Lesen und akzeptieren Sie dann die Nutzungsbedingungen des [Microsoft Azure Marketplace-Herausgebervertrags](http://go.microsoft.com/fwlink/?LinkID=699560). Aktivieren Sie das Kontrollkästchen, um anzugeben, dass Sie die Bedingungen gelesen und akzeptiert haben.

Schritt 11 Klicken Sie auf **Fertig stellen** , um die Registrierung zu bestätigen. Wir senden eine Bestätigungsnachricht an Ihre E-Mail-Adresse.

Schritt 12 Wenn Sie planen, nur kostenlose Angebote zu veröffentlichen, klicken Sie auf **Zum [Cloudpartnerportal](https://cloudpartner.azure.com/) wechseln**. Sie können dann mit Abschnitt 3 dieses Dokuments fortfahren.

Wenn Sie die Veröffentlichung von kommerziellen Angeboten planen (z.B. VM-Angebote mit stündlichem Abrechnungsmodell), klicken Sie auf **Kontoinformationen aktualisieren**. Unter dieser Option müssen Sie die Steuer- und Bankdaten für Ihr Developer Center-Konto angeben.

Wenn Sie Ihre Steuer- und Bankverbindungsdaten später aktualisieren möchten, können Sie mit dem nächsten Abschnitt (Abschnitt 3) dieses Dokuments fortfahren.

> [!IMPORTANT]
> Kommerzielle Angebote können Sie nicht ohne Angabe der Steuer- und Bankdaten in die Produktionsumgebung überführen.
>
>

### <a name="add-tax-and-banking-information"></a>Hinzufügen von Steuer- und Bankinformationen
 Wenn Sie Angebote für den Erwerb veröffentlichen möchten, müssen Sie auch Auszahlungs- und Steuerinformationen hinzufügen und zur Validierung im Developer Center absenden. Wenn Sie nur kostenlose Angebote (oder BYOL-Angebote) veröffentlichen möchten, müssen Sie diese Informationen nicht hinzufügen. Sie können dieses Informationen später eingeben, es dauert jedoch einige Zeit, bis die Steuerinformationen validiert werden können. Wenn Sie wissen, dass Sie Angebote für den Erwerb veröffentlichen werden, sollten Sie diese Informationen so schnell wie möglich bereitstellen.

**Bankinformationen**

1. Melden Sie mit Ihrem Microsoft-Konto am [Microsoft Developer Center](http://dev.windows.com/registration?accountprogram=azure) an.
2. Klicken Sie im Menü auf der linken Seite auf **Auszahlungskonto**, und klicken Sie unter **Zahlungsmethode wählen** auf **Bankkonto** oder **PayPal**.

   > [!IMPORTANT]
   > Wenn Sie über kommerzielle Angebote verfügen, die Kunden im Marketplace erwerben, wird dieses Konto für die entsprechenden Auszahlungen an Sie verwendet.
   >
   >
3. Geben Sie die Zahlungsinformationen ein, und klicken Sie auf **Speichern** , wenn Sie zufrieden sind.

   > [!IMPORTANT]
   > Wenn Sie das Auszahlungskonto aktualisieren oder ändern müssen, können Sie die obigen Schritte verwenden und die aktuellen Informationen durch neue Informationen ersetzen. Wenn Sie Ihr Zahlungskonto ändern, kann dies dazu führen, dass sich Ihre Zahlungen um maximal einen Zahlungszyklus verzögern. Diese Verzögerung tritt auf, weil wir die Kontoänderung überprüfen müssen. Dies ist der gleiche Vorgang wie bei der ersten Einrichtung des Zahlungskontos. Nachdem das Konto bestätigt wurde, erhalten Sie trotzdem den vollen Betrag. Zahlungen, die für den aktuellen Zahlungszyklus fällig sind, werden im nächsten Zyklus hinzugefügt.
   >
   >
4. Klicken Sie auf **Weiter**.

**Steuerinformationen**

1. Melden Sie mit Ihrem Microsoft-Konto am [Microsoft Developer Center](http://dev.windows.com/registration?accountprogram=azure) an (falls erforderlich).
2. Klicken Sie im Menü auf der linken Seite auf **Steuerprofil** .
3. Wählen Sie auf der Seite **Richten Sie Ihr Steuerformular ein** das Land oder die Region aus, in dem bzw. der sich Ihr ständiger Wohnsitz befindet. Wählen Sie anschließend das Land oder die Region aus, dessen bzw. deren erste Staatsbürgerschaft Sie besitzen. Klicken Sie auf **Weiter**.
4. Geben Sie Ihre Steuerdaten ein, und klicken Sie auf **Weiter**.

> [!WARNING]
> Ohne vollständige Angabe der Steuer- und Bankinformationen in Ihrem Microsoft Developer Center-Konto können Sie Ihre kommerziellen Angebote nicht in die Produktionsumgebung überführen.
>
>

Wenn beim Abschließen der Developer Center-Registrierung Probleme auftreten, öffnen Sie wie nachfolgend beschrieben ein Supportticket:

1. Klicken Sie auf den Supportlink [https://developer.microsoft.com/windows/support](https://developer.microsoft.com/windows/support)
2. Klicken Sie im Abschnitt **Kontakt** auf die Schaltfläche **Vorfall übermitteln** (wie im folgenden Screenshot gezeigt).

  ![Abbildung](./media/cloud-partner-portal-create-dev-center-registration/imgAddTax_02.png)
3. Wählen Sie „Hilfe zu Developer Center“ als **Problemtyp** und „Veröffentlichen und Verwalten von Apps“ als **Kategorie** aus. Klicken Sie anschließend auf die Schaltfläche „E-Mail starten“.

  ![Abbildung](./media/cloud-partner-portal-create-dev-center-registration/imgAddTax_03.png)
4. Ihnen wird eine Anmeldeseite zur Verfügung gestellt. Verwenden Sie eine beliebige Microsoft-Kontenanmeldung. Wenn Sie kein Microsoft-Konto besitzen, erstellen Sie eins mit diesem [Link](https://signup.live.com/signup?uaid=0089f09ccae94043a0f07c2aaf928831&lic=1).
5. Tragen Sie die Details des Problems ein, und übermitteln Sie das Ticket durch Klicken auf die Schaltfläche **Übermitteln** .

  ![Abbildung](./media/cloud-partner-portal-create-dev-center-registration/imgAddTax_05.png)

## <a name="3-register-your-account-in-the-cloud-partner-portal"></a>3. Registrieren Ihres Kontos im Cloudpartnerportal
Das [Cloudpartnerportal](https://cloudpartner.azure.com/) dient zum Veröffentlichen und Verwalten Ihrer Angebote.

1. Öffnen Sie eine neue Chrome-Inkognito- oder Internet Explorer-InPrivate-Browsersitzung, um sicherzustellen, dass Sie nicht bei einem persönlichen Konto angemeldet sind.
2. Wechseln Sie zum [Cloudpartnerportal](https://cloudpartner.azure.com/).
3. Wenn Sie sich als neuer Benutzer zum ersten Mal beim [Cloudpartnerportal](https://cloudpartner.azure.com/) anmelden, müssen Sie sich mit der gleichen E-Mail-ID anmelden, unter der Ihr Developer Center-Konto registriert ist. Auf diese Weise werden Ihr Developer Center-Konto und das Cloudpartnerportal miteinander verknüpft. Sie können später die anderen Mitglieder des Unternehmens, die an der Anwendung arbeiten, mit den folgenden Schritten im Cloudpartnerportal als Mitwirkende oder Besitzer hinzufügen.

Wenn Sie im Cloudpartnerportal als Mitwirkender/Besitzer hinzugefügt werden, können Sie sich mit Ihrem eigenen Konto anmelden.

> [!TIP]
> Die Teilnahmerichtlinien werden auf der [Azure-Website](https://azure.microsoft.com/support/legal/marketplace/participation-policies/)beschrieben.
>
>

## <a name="4-steps-to-manage-users-as-owners-or-contributor-in-the-cloud-partner-portal"></a>4. Schritte zum Verwalten von Benutzern als Besitzer oder Mitwirkende im Cloudpartnerportal

[Schritte zum Verwalten von Benutzern im Cloudpartnerportal](./cloud-partner-portal-manage-users.md)


## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie Ihr Konto erstellt und registriert haben, können Sie den Azure Marketplace-Veröffentlichungsprozess starten.

[img-msalive]:media/cloud-partner-portal-create-dev-center-registration/creating-msa-account-msa-live.jpg
[img-email]:media/cloud-partner-portal-create-dev-center-registration/creating-msa-account-msa-verifyemail.jpg
[img-sd-url]:media/cloud-partner-portal-create-dev-center-registration/seller-dashboard-incognito.jpg
[img-signin]:media/cloud-partner-portal-create-dev-center-registration/seller-dashboard-login.jpg
[img-verify]:media/cloud-partner-portal-create-dev-center-registration/seller-dashboard-verify.jpg
[img-sd-top]:media/cloud-partner-portal-create-dev-center-registration/seller-dashboard-personal-acc-details.jpg
[img-sd-info]:media/cloud-partner-portal-create-dev-center-registration/seller-dashboard-personal.jpg

[img-sd-type]:media/cloud-partner-portal-create-dev-center-registration/seller-dashboard-personal-acc-type.jpg

[img-sd-mktg1]:media/cloud-partner-portal-create-dev-center-registration/seller-dashboard-personal-comp-det1.jpg
[img-sd-mktg2]:media/cloud-partner-portal-create-dev-center-registration/seller-dashboard-personal-comp-det2.jpg
[img-sd-addr]:media/cloud-partner-portal-create-dev-center-registration/seller-dashboard-personal-comp-add.jpg
[img-sd-legal]:media/cloud-partner-portal-create-dev-center-registration/seller-dashboard-personal-cmp.jpg
[img-sd-submit]:media/cloud-partner-portal-create-dev-center-registration/seller-dashboard-approval.jpg


[link-msdndoc]: https://msdn.microsoft.com/library/jj552460.aspx
[link-sellerdashboard]: http://sellerdashboard.microsoft.com/
[link-pubportal]: https://publish.windowsazure.com
[link-single-vm]:marketplace-publishing-vm-image-creation.md
[link-single-vm-prereq]:marketplace-publishing-vm-image-creation-prerequisites.md
[link-multi-vm]:marketplace-publishing-solution-template-creation.md
[link-multi-vm-prereq]:marketplace-publishing-solution-template-creation-prerequisites.md
[link-datasvc]:marketplace-publishing-data-service-creation.md
[link-datasvc-prereq]:marketplace-publishing-data-service-creation-prerequisites.md
[link-devsvc]:marketplace-publishing-dev-service-creation.md
[link-devsvc-prereq]:marketplace-publishing-dev-service-creation-prerequisites.md
[link-pushstaging]:marketplace-publishing-push-to-staging.md
