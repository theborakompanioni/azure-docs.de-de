<properties
   pageTitle="Verwalten eines VM-Images im Azure Marketplace | Microsoft Azure"
   description="Enthält eine ausführliche Anleitung dazu, wie Sie Ihr VM-Image nach der ersten Veröffentlichung im Azure Marketplace verwalten."
   services="Azure Marketplace"
   documentationCenter=""
   authors="HannibalSII"
   manager=""
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="Azure"
   ms.workload="na"
   ms.date="06/01/2016"
   ms.author="hascipio;"/>

# Postproduktionsleitfaden für Virtual Machine-Angebote im Azure Marketplace

In diesem Artikel wird beschrieben, wie Sie ein Virtual Machine-Liveangebot im Azure Marketplace aktualisieren können. Außerdem wird der Prozess zum Hinzufügen von neuen SKUs zu einem vorhandenen Angebot und zum Entfernen eines Virtual Machine-Liveangebots oder einer SKU aus dem Azure Marketplace erläutert.

**Nachdem die SKU im Azure Marketplace „live“ ist, können Sie die unten angegebenen Details nicht mehr aktualisieren:**

- **SKU-ID**
- **Herausgeber-ID**
- **Angebots-ID**
- **Tarifänderung**
- **Abrechnungsmodelländerung**
- **Entfernen von Abrechnungsregionen**


## 1\. Aktualisieren der technischen Details einer SKU

Sie können ein VM-Image aktualisieren und Ihr Angebot erneut veröffentlichen, indem Sie die unten angegebenen Schritte ausführen:

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com) an.
2. Navigieren Sie zur Registerkarte **VIRTUELLE COMPUTER**, und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **VM-IMAGES**.
4. Suchen Sie im Abschnitt **SKUs** der Registerkarte „VM-IMAGES“ nach der SKU, die Sie aktualisieren möchten. Aktualisieren Sie anschließend die Versionsnummer der SKU. Die neue Version sollte das Format X.Y.Z haben, wobei X, Y, Z für ganze Zahlen stehen. Versionsänderungen sollten nur inkrementell sein.
5. Aktualisieren Sie im Feld mit der **URL der Betriebssystem-VHD** den SAS-URI, der für die Betriebssystem-VHD erstellt wurde, und speichern Sie die Änderungen.
6. Navigieren Sie nach dem Vornehmen der Änderungen zur Registerkarte **VERÖFFENTLICHEN**, und klicken Sie auf die Schaltfläche **PUSH TO STAGING** (FÜR STAGINGUMGEBUNG FREIGEBEN). Eine ausführliche Anleitung zum Testen des Angebots in der Stagingumgebung finden Sie unter diesem [Link](marketplace-publishing-vm-image-test-in-staging.md).
7. Navigieren Sie nach dem Testen des Angebots in der Stagingumgebung zur Registerkarte **VERÖFFENTLICHEN** im Veröffentlichungsportal, und klicken Sie auf die Schaltfläche **REQUEST APPROVAL TO PUSH TO PRODUCTION** (GENEHMIGUNG FÜR FREIGABE IN PRODUKTIONSUMGEBUNGEN ANFORDERN), um das Angebot erneut im Azure Marketplace zu veröffentlichen.

    ![Abbildung](media/marketplace-publishing-vm-image-post-publishing/img01.png)

## 2\. Aktualisieren der nicht technischen Details eines Angebots oder einer SKU

Sie können die nicht technischen Details (Marketing, rechtliche Dinge, Support, Kategorien) Ihres Liveangebots der SKU im Azure Marketplace aktualisieren.

### 2\.1 Aktualisieren der Angebotsbeschreibung und der Logos

Sie können die Angebotsdetails aktualisieren und das Angebot erneut veröffentlichen, indem Sie die unten angegebenen Schritte ausführen:

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com) an.
2. Navigieren Sie zur Registerkarte **VIRTUELLE COMPUTER**, und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **MARKETING**.
4. Klicken Sie auf die Schaltfläche **ENGLISCH (USA)** (bzw. ggf. die Option für Ihre Sprache).
5. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **DETAILS**. Im Abschnitt *BESCHREIBUNG* der Registerkarte **DETAILS** können Sie den Angebotstitel, die Angebotsübersicht und die ausführliche Angebotsübersicht aktualisieren und die Änderungen speichern.

    >[AZURE.NOTE] Beachten Sie beim Aktualisieren der SKU-Details Folgendes: **Geben Sie für die Angebotsbeschreibung und die SKU-Beschreibung nicht den gleichen Text ein. Geben Sie für den SKU-Titel und die ausführliche Angebotsübersicht nicht den gleichen Text ein. Geben Sie für den SKU-Titel und die Angebotsübersicht nicht den gleichen Text ein.**

    ![Abbildung](media/marketplace-publishing-vm-image-post-publishing/img02.png)

6. Im Abschnitt *LOGOS* der Registerkarte **DETAILS** können Sie die Logos aktualisieren. Stellen Sie aber sicher, dass für die Logos die [Azure Marketplace-Richtlinien](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content) eingehalten werden (siehe Abschnitt „Schritt 1: Bereitstellen von Marketinginhalten im Marketplace -> Details -> Azure Marketplace-Richtlinien für Logos).

    >[AZURE.NOTE] Das Herologo ist optional. Sie können sich auch gegen das Hochladen eines Herologos entscheiden. Wenn aber ein Herologo hochgeladen wurde, kann es nicht mehr aus dem Veröffentlichungsportal gelöscht werden. In diesem Fall müssen Sie sich an die [Richtlinien für Herologos](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content) halten (siehe Abschnitt „Schritt 1: Bereitstellen von Marketinginhalten im Marketplace -> Details -> Zusätzliche Richtlinien für Herologobanner).

7. Navigieren Sie nach dem Vornehmen der Änderungen zur Registerkarte **VERÖFFENTLICHEN**, und klicken Sie auf die Schaltfläche **PUSH TO STAGING** (FÜR STAGINGUMGEBUNG FREIGEBEN). Eine ausführliche Anleitung zum Testen des Angebots in der Stagingumgebung finden Sie unter diesem [Link](marketplace-publishing-vm-image-test-in-staging.md).
8. Navigieren Sie nach dem Testen des Angebots in der Stagingumgebung zur Registerkarte **VERÖFFENTLICHEN** im Veröffentlichungsportal, und klicken Sie auf die Schaltfläche **REQUEST APPROVAL TO PUSH TO PRODUCTION** (GENEHMIGUNG FÜR FREIGABE IN PRODUKTIONSUMGEBUNGEN ANFORDERN), um das Angebot erneut im Azure Marketplace zu veröffentlichen.

    ![Abbildung](media/marketplace-publishing-vm-image-post-publishing/img02.1.png)

### 2\.2. Aktualisieren der SKU-Beschreibung

Sie können die SKU-Details aktualisieren und das Angebot erneut veröffentlichen, indem Sie die unten angegebenen Schritte ausführen:

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com) an.
2. Navigieren Sie zur Registerkarte **VIRTUELLE COMPUTER**, und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **MARKETING**.
4. Klicken Sie auf die Schaltfläche **ENGLISCH (USA)** (bzw. ggf. die Option für Ihre Sprache).
5. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **PLÄNE**. Im Abschnitt *SKUs* der Registerkarte **PLÄNE** können Sie den SKU-Titel, die SKU-Übersicht und die SKU-Beschreibung aktualisieren und die Änderungen speichern.

    >[AZURE.NOTE] Beachten Sie beim Aktualisieren der SKU-Details Folgendes: **Geben Sie für die Angebotsbeschreibung und die SKU-Beschreibung nicht den gleichen Text ein. Geben Sie für den SKU-Titel und die ausführliche Angebotsübersicht nicht den gleichen Text ein. Geben Sie für den SKU-Titel und die Angebotsübersicht nicht den gleichen Text ein.**

6. Navigieren Sie nach dem Vornehmen der Änderungen zur Registerkarte **VERÖFFENTLICHEN**, und klicken Sie auf die Schaltfläche **PUSH TO STAGING** (FÜR STAGINGUMGEBUNG FREIGEBEN). Eine ausführliche Anleitung zum Testen des Angebots in der Stagingumgebung finden Sie unter diesem Link.
7. Navigieren Sie nach dem Testen des Angebots in der Stagingumgebung zur Registerkarte **VERÖFFENTLICHEN** im Veröffentlichungsportal, und klicken Sie auf die Schaltfläche **REQUEST APPROVAL TO PUSH TO PRODUCTION** (GENEHMIGUNG FÜR FREIGABE IN PRODUKTIONSUMGEBUNGEN ANFORDERN), um das Angebot erneut im Azure Marketplace zu veröffentlichen.

    ![Abbildung](media/marketplace-publishing-vm-image-post-publishing/img02.2.png)

### 2\.3 Ändern der vorhandenen Links oder Hinzufügen neuer Links

Sie können die vorhandenen Links ändern oder neue Links hinzufügen und das Angebot dann erneut veröffentlichen, indem Sie die unten angegebenen Schritte ausführen:

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com) an.
2. Navigieren Sie zur Registerkarte **VIRTUELLE COMPUTER**, und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **MARKETING**.
4. Klicken Sie auf die Schaltfläche **ENGLISCH (USA)** (bzw. ggf. die Option für Ihre Sprache).
5. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **LINKS**.
6. Wenn Sie einen neuen Link hinzufügen möchten, klicken Sie im Abschnitt *Links* auf die Schaltfläche **LINK HINZUFÜGEN**. Das Dialogfeld *Link hinzufügen* wird geöffnet. In diesem Dialogfeld können Sie die Felder „Titel“ und „URL“ des Links hinzufügen und die Änderungen speichern. Sie können einen beliebigen Link mit Informationen eingeben, die für die Kunden hilfreich sind.
7. Wenn Sie einen vorhandenen Link aktualisieren oder löschen möchten, können Sie den gewünschten Link auswählen und auf die Schaltfläche „Bearbeiten“ oder „Löschen“ klicken.

    >[AZURE.NOTE] Stellen Sie sicher, dass die Links, die Sie in diesem Abschnitt eingegeben haben, richtig funktionieren. Während des Produktionsanforderungsprozesses werden diese Links überprüft.

8. Navigieren Sie nach dem Vornehmen der Änderungen zur Registerkarte **VERÖFFENTLICHEN**, und klicken Sie auf die Schaltfläche **PUSH TO STAGING** (FÜR STAGINGUMGEBUNG FREIGEBEN). Eine ausführliche Anleitung zum Testen des Angebots in der Stagingumgebung finden Sie unter diesem [Link](marketplace-publishing-vm-image-test-in-staging.md).
9. Navigieren Sie nach dem Testen des Angebots in der Stagingumgebung zur Registerkarte **VERÖFFENTLICHEN** im Veröffentlichungsportal, und klicken Sie auf die Schaltfläche **REQUEST APPROVAL TO PUSH TO PRODUCTION** (GENEHMIGUNG FÜR FREIGABE IN PRODUKTIONSUMGEBUNGEN ANFORDERN), um das Angebot erneut im Azure Marketplace zu veröffentlichen.

    ![Abbildung](media/marketplace-publishing-vm-image-post-publishing/img02.3.png)

    ![Abbildung](media/marketplace-publishing-vm-image-post-publishing/img02.3-2.png)

### 2\.4 Ändern eines vorhandenen Beispielbilds oder Hinzufügen eines neuen Beispielbilds

Sie können vorhandene Beispielbilder ändern oder neue Beispielbilder hinzufügen und das Angebot dann erneut veröffentlichen, indem Sie die unten angegebenen Schritte ausführen:

>[AZURE.NOTE] Unter [https://portal.azure.com](https://portal.azure.com) wird nur ein Beispielbild angezeigt.

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com) an.
2. Navigieren Sie zur Registerkarte **VIRTUELLE COMPUTER**, und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **MARKETING**.
4. Klicken Sie auf die Schaltfläche **ENGLISCH (USA)** (bzw. ggf. die Option für Ihre Sprache).
5. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **BEISPIELBILDER**.
6. Wenn Sie ein neues Beispielbild hinzufügen möchten, können Sie im Abschnitt *Beispielbilder* auf die Schaltfläche **NEUES BILD HOCHLADEN** klicken und die Änderungen speichern.

    >[AZURE.NOTE] Das Hinzufügen von Beispielbildern ist ein optionaler Schritt.

7. Wenn Sie ein vorhandenes Beispielbild aktualisieren oder löschen möchten, können Sie nach dem gewünschten Beispielbild suchen und dann auf die Schaltfläche **BILD ERSETZEN** oder die Schaltfläche zum Löschen klicken.

8. Navigieren Sie nach dem Vornehmen der Änderungen zur Registerkarte **VERÖFFENTLICHEN**, und klicken Sie auf die Schaltfläche **PUSH TO STAGING** (FÜR STAGINGUMGEBUNG FREIGEBEN). Eine ausführliche Anleitung zum Testen des Angebots in der Stagingumgebung finden Sie unter diesem [Link](marketplace-publishing-vm-image-test-in-staging.md).
9. Navigieren Sie nach dem Testen des Angebots in der Stagingumgebung zur Registerkarte **VERÖFFENTLICHEN** im Veröffentlichungsportal, und klicken Sie auf die Schaltfläche **REQUEST APPROVAL TO PUSH TO PRODUCTION** (GENEHMIGUNG FÜR FREIGABE IN PRODUKTIONSUMGEBUNGEN ANFORDERN), um das Angebot erneut im Azure Marketplace zu veröffentlichen.

    ![Abbildung](media/marketplace-publishing-vm-image-post-publishing/img02.4.png)

### 2\.5 Aktualisieren von rechtlichen Inhalten

Sie können die rechtlichen Inhalte aktualisieren und das Angebot erneut veröffentlichen, indem Sie die unten angegebenen Schritte ausführen:

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com) an.
2. Navigieren Sie zur Registerkarte **VIRTUELLE COMPUTER**, und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **MARKETING**.
4. Klicken Sie auf die Schaltfläche **ENGLISCH (USA)** (bzw. ggf. die Option für Ihre Sprache).
5. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **LEGAL** (RECHTLICHE HINWEISE). Im Abschnitt *Legal* (Rechtliche Hinweise) können Sie Ihre Richtlinien und Nutzungsbedingungen aktualisieren. Geben oder fügen Sie die Richtlinien/Bedingungen in das Textfeld *Nutzungsbedingungen* ein, und speichern Sie die Änderungen.
6. Die Nutzungsbedingungen dürfen maximal 1.000.000 Zeichen umfassen.
7. Navigieren Sie nach dem Vornehmen der Änderungen zur Registerkarte **VERÖFFENTLICHEN**, und klicken Sie auf die Schaltfläche **PUSH TO STAGING** (FÜR STAGINGUMGEBUNG FREIGEBEN). Eine ausführliche Anleitung zum Testen des Angebots in der Stagingumgebung finden Sie unter diesem [Link](marketplace-publishing-vm-image-test-in-staging.md).
8. Navigieren Sie nach dem Testen des Angebots in der Stagingumgebung zur Registerkarte **VERÖFFENTLICHEN** im Veröffentlichungsportal, und klicken Sie auf die Schaltfläche **REQUEST APPROVAL TO PUSH TO PRODUCTION** (GENEHMIGUNG FÜR FREIGABE IN PRODUKTIONSUMGEBUNGEN ANFORDERN), um das Angebot erneut im Azure Marketplace zu veröffentlichen.

    ![Abbildung](media/marketplace-publishing-vm-image-post-publishing/img02.5.png)

### 2\.6 Aktualisieren der Supportinformationen

Sie können die Supportinformationen aktualisieren und das Angebot erneut veröffentlichen, indem Sie die unten angegebenen Schritte ausführen:

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com) an.
2. Navigieren Sie zur Registerkarte **VIRTUELLE COMPUTER**, und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **SUPPORT**.
4. Im Abschnitt *Engineering-Kontaktdaten* der Registerkarte **SUPPORT** können Sie die Kontaktdaten aktualisieren. Diese Daten werden nur für die interne Kommunikation zwischen dem Partner und Microsoft verwendet.
5. Im Abschnitt *Kundensupport* der Registerkarte **SUPPORT** können Sie die Support-Kontaktdaten wie **Name, E-Mail, Telefon** und die **Support-URL** aktualisieren. Diese Daten werden nur für die interne Kommunikation zwischen dem Partner und Microsoft verwendet.

    >[AZURE.NOTE] Wenn Sie nur E-Mail-Support bereitstellen möchten, können Sie im Abschnitt **Kundensupport** eine Pseudotelefonnummer angeben. In diesem Fall wird stattdessen die von Ihnen angegebene E-Mail-Adresse verwendet.

6. Navigieren Sie nach dem Vornehmen der Änderungen zur Registerkarte **VERÖFFENTLICHEN**, und klicken Sie auf die Schaltfläche **PUSH TO STAGING** (FÜR STAGINGUMGEBUNG FREIGEBEN). Eine ausführliche Anleitung zum Testen des Angebots in der Stagingumgebung finden Sie unter diesem [Link](marketplace-publishing-vm-image-test-in-staging.md).
7. Navigieren Sie nach dem Testen des Angebots in der Stagingumgebung zur Registerkarte **VERÖFFENTLICHEN** im Veröffentlichungsportal, und klicken Sie auf die Schaltfläche **REQUEST APPROVAL TO PUSH TO PRODUCTION** (GENEHMIGUNG FÜR FREIGABE IN PRODUKTIONSUMGEBUNGEN ANFORDERN), um das Angebot erneut im Azure Marketplace zu veröffentlichen.

    ![Abbildung](media/marketplace-publishing-vm-image-post-publishing/img02.7.png)

### 2\.7 Aktualisieren der Kategorien

Sie können den Abschnitt „Kategorien“ für das Angebot aktualisieren und das Angebot erneut veröffentlichen, indem Sie die unten angegebenen Schritte ausführen:

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com) an.
2. Navigieren Sie zur Registerkarte **VIRTUELLE COMPUTER**, und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **KATEGORIEN**.
4. Im Abschnitt *Kategorien* können Sie die Kategorien für das Angebot aktualisieren und die Änderungen speichern. Sie können bis zu fünf Kategorien für den Azure Marketplace-Katalog auswählen.
5. Navigieren Sie nach dem Vornehmen der Änderungen zur Registerkarte **VERÖFFENTLICHEN**, und klicken Sie auf die Schaltfläche **PUSH TO STAGING** (FÜR STAGINGUMGEBUNG FREIGEBEN). Eine ausführliche Anleitung zum Testen des Angebots in der Stagingumgebung finden Sie unter diesem [Link](marketplace-publishing-vm-image-test-in-staging.md).
6. Navigieren Sie nach dem Testen des Angebots in der Stagingumgebung zur Registerkarte **VERÖFFENTLICHEN** im Veröffentlichungsportal, und klicken Sie auf die Schaltfläche **REQUEST APPROVAL TO PUSH TO PRODUCTION** (GENEHMIGUNG FÜR FREIGABE IN PRODUKTIONSUMGEBUNGEN ANFORDERN), um das Angebot erneut im Azure Marketplace zu veröffentlichen.

    ![Abbildung](media/marketplace-publishing-vm-image-post-publishing/img02.7.png)

## 3\. Hinzufügen einer neuen SKU unter einem Liveangebot

Sie können unter Ihrem Liveangebot eine neue SKU hinzufügen, indem Sie die unten angegebenen Schritte ausführen:

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com) an.
2. Navigieren Sie zur Registerkarte **VIRTUELLE COMPUTER**, und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **SKUs**. Klicken Sie anschließend auf die Schaltfläche **ADD A SKU** (SKU HINZUFÜGEN). Ein neues Dialogfeld wird geöffnet. Geben Sie eine SKU-ID in Kleinbuchstaben ein. Aktivieren Sie das Kontrollkästchen für das Abrechnungsmodell „Bring-Your-Own-License“ (BYOL), wenn Sie die neue SKU mit dem BYOL-Abrechnungsmodell veröffentlichen möchten. Andernfalls können Sie das Kontrollkästchen für BYOL deaktivieren. Klicken Sie anschließend auf das Häkchen im Dialogfeld, um eine neue SKU zu erstellen. Wenn Sie sich in Bezug auf die neue SKU gegen das BYOL-Abrechnungsmodell entschieden haben, wird das Abrechnungsmodell für die neue SKU automatisch auf „Stündlich“ festgelegt. Klicken Sie für „Is a free trial available?“ (Ist eine kostenlose Testversion verfügbar?) auf die Option „Ein Monat“, wenn Sie für das Abrechnungsmodell „Stündlich“ die kostenlose Testversion mit einer Dauer von 30 Tagen aktivieren möchten. Wählen Sie andernfalls die Option „KEINE TESTVERSION“. [Hinweis: Die Option „Is a free trial available?“ (Ist eine kostenlose Testversion verfügbar?) wird nur angezeigt, wenn Sie NICHT beim Erstellen der neuen SKU die Option BYOL gewählt haben.]

    >[AZURE.IMPORTANT] Die Option „Hide this SKU from the Marketplace because it should always be bought via a solution template“ (SKU für Marketplace ausblenden, da sie immer über eine Lösungsvorlage erworben werden sollte) sollte nur dann auf „JA“ festgelegt werden, wenn Sie über die Genehmigung zum Veröffentlichen eines Lösungsvorlagenangebots im Azure Marketplace verfügen. Andernfalls sollte diese Option immer auf „NEIN“ festgelegt werden.

4. Klicken Sie im Menü auf der linken Seite jetzt auf die Registerkarte **VM-IMAGES**, und ermitteln Sie die neue SKU, die Sie erstellt haben.
5. Eine Anleitung zum Einrichten der neuen SKU finden Sie in SCHRITT 5 unter diesem [Link](marketplace-publishing-vm-image-creation.md#5-obtain-certification-for-your-vm-image).
6. Informationen zum Hinzufügen des Marketingmaterials für die neue SKU finden Sie im Abschnitt „Schritt 1: Bereitstellen von Marketinginhalten im Marketplace -> Details -> Punkte 2 bis 5“ unter diesem [Link](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content).
7. Informationen zum Hinzufügen der Preisinformationen für die neue SKU finden Sie im Abschnitt 2.1. Informationen zur Festlegung der VM-Preise finden Sie unter diesem [Link](marketplace-publishing-push-to-staging.md#step-2-set-your-prices).
8. Navigieren Sie nach dem Vornehmen der Änderungen zur Registerkarte **VERÖFFENTLICHEN**, und klicken Sie auf die Schaltfläche **PUSH TO STAGING** (FÜR STAGINGUMGEBUNG FREIGEBEN). Eine ausführliche Anleitung zum Testen des Angebots in der Stagingumgebung finden Sie unter diesem [Link](marketplace-publishing-vm-image-test-in-staging.md).
9. Navigieren Sie nach dem Testen des Angebots in der Stagingumgebung zur Registerkarte **VERÖFFENTLICHEN** im Veröffentlichungsportal, und klicken Sie auf die Schaltfläche **REQUEST APPROVAL TO PUSH TO PRODUCTION** (GENEHMIGUNG FÜR FREIGABE IN PRODUKTIONSUMGEBUNGEN ANFORDERN), um das Angebot erneut im Azure Marketplace zu veröffentlichen.

    ![Abbildung](media/marketplace-publishing-vm-image-post-publishing/img03.1-1.png)

    ![Abbildung](media/marketplace-publishing-vm-image-post-publishing/img03.1-2.png)

## 4\. Löschen eines Liveangebots oder einer SKU aus dem Azure Marketplace

Bei einer Anforderung zum Entfernen eines Liveangebots müssen verschiedene Aspekte berücksichtigt werden. Führen Sie die unten angegebenen Schritte aus, um vom Supportteam Hilfe beim Entfernen eines Liveangebots bzw. einer SKU aus dem Azure Marketplace zu erhalten:

1.	Erstellen Sie über diesen [Link](https://support.microsoft.com/de-DE/getsupport?wf=0&tenant=ClassicCommercial&oaspworkflow=start_1.0.0.0&locale=de-DE&supportregion=de-DE&pesid=15635&ccsid=635993707583706681) ein Supportticket.
2.	Wählen Sie als Problemtyp die Option **„Angebotsverwaltung“** und als Kategorie **„Modifying an offer and/or SKU already in production“** (Ändern eines Angebots/einer SKU in der Produktion).
3.	Senden Sie die Anforderung.

Das Supportteam führt Sie durch den Vorgang zum Löschen des Angebots bzw. der SKU.

>[AZURE.NOTE] Sie können das Angebot jederzeit löschen, während es sich im Entwurfsstatus befindet (also nicht im Status „STAGING“ oder „PRODUKTION“), indem Sie auf der Registerkarte **VERLAUF** auf die Schaltfläche **ENTWURF VERWERFEN** klicken.

## Weitere Informationen
- [Erste Schritte: Veröffentlichen eines Angebots im Azure Marketplace](marketplace-publishing-getting-started.md)
- [Grundlegendes zu den Verkäufer-Insights-Berichten](marketplace-publishing-report-seller-insights.md)
- [Grundlegendes zu den Auszahlungsberichten](marketplace-publishing-report-payout.md)
- [Gewusst wie: Ändern Ihres „Reseller Incentive“ für Cloud-Lösungsanbieter](marketplace-publishing-csp-incentive.md)
- [Beheben häufiger Probleme bei der Veröffentlichung im Marketplace](marketplace-publishing-support-common-issues.md)
- [Zugriff auf die Publisher-Unterstützung](marketplace-publishing-get-publisher-support.md)
- [Erstellen eines lokalen VM-Images](marketplace-publishing-vm-image-creation-on-premise.md)
- [Erstellen eines virtuellen Computers unter Windows im Azure-Vorschauportal](../virtual-machines/virtual-machines-windows-hero-tutorial.md)

<!---HONumber=AcomDC_0713_2016-->