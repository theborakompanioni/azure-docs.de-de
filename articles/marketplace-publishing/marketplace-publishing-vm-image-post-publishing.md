---
title: Verwalten eines VM-Images im Azure Marketplace | Microsoft Docs
description: "Enthält eine ausführliche Anleitung dazu, wie Sie Ihr VM-Image nach der ersten Veröffentlichung im Azure Marketplace verwalten."
services: Azure Marketplace
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: cc8648d4-59c2-4678-b47d-992300677537
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 08/03/2016
ms.author: hascipio;
translationtype: Human Translation
ms.sourcegitcommit: 5919c477502767a32c535ace4ae4e9dffae4f44b
ms.openlocfilehash: ed2921750f93f344a4c3dbef31d9f523dedc0aae


---
# <a name="post-production-guide-for-virtual-machine-offers-in-the-azure-marketplace"></a>Postproduktionsleitfaden für Virtual Machine-Angebote im Azure Marketplace
In diesem Artikel wird beschrieben, wie Sie ein Virtual Machine-Liveangebot im Azure Marketplace aktualisieren können. Außerdem wird der Prozess zum Hinzufügen von neuen SKUs zu einem vorhandenen Angebot und zum Entfernen eines Virtual Machine-Liveangebots oder einer SKU aus dem Azure Marketplace erläutert.

Sobald ein Angebot/eine SKU im [Azure-Portal](http://portal.azure.com)bereitgestellt wird, können Sie die unten angegebenen Felder nicht ändern:

* **Angebotsbezeichner:** [Veröffentlichungsportal > „Virtuelle Computer“ > wählen Sie Ihr Angebot > Registerkarte „VM-Images“ > „Angebotsbezeichner“]
* **SKU-Bezeichner:** [Veröffentlichungsportal > „Virtual Machines“ > Ihr Angebot > Registerkarte „SKUs“ > „SKU hinzufügen“]
* **Herausgebernamespace:** [Veröffentlichungsportal > „Virtual Machines“ > Registerkarte „Vorgehensweise“ > „Informationen zu Ihrem Unternehmen“ (unter „Schritt 2: Registrieren Sie Ihr Unternehmen“) > „Herausgebernamespace“ > „Namespace“]

Sobald ein Angebot/eine SKU im [Azure Marketplace aufgeführt](http://azure.microsoft.com/marketplace)wird, können Sie die unten angegebenen Felder nicht ändern:

* **Angebotsbezeichner:** [Veröffentlichungsportal > „Virtual Machines“ > wählen Sie Ihr Angebot > Registerkarte „VM-Images“ > „Angebotsbezeichner“]
* **SKU-Bezeichner:** [Veröffentlichungsportal > „Virtual Machines“ > Ihr Angebot > Registerkarte „SKUs“ > „SKU hinzufügen“]
* **Herausgebernamespace:** [Veröffentlichungsportal > „Virtual Machines“ > Registerkarte „Vorgehensweise“ > „Informationen zu Ihrem Unternehmen“ (unter „Schritt 2: Registrieren“) > „Herausgebernamespace“ > „Namespace“]
* **Ports:** [Veröffentlichungsportal > „Virtual Machines“ > wählen Sie Ihr Angebot > Registerkarte „VM-Images“ > „Offene Ports “]
* **Preisänderung bei aufgelisteten SKUs**
* **Abrechnungsmodelländerung bei aufgelisteten SKUs**
* **Entfernen von Abrechnungsregionen bei aufgelisteten SKUs**
* **Ändern der Anzahl der Datenträger bei aufgelisteten SKUs**

## <a name="1-how-to-update-the-technical-details-of-a-sku"></a>1. Aktualisieren der technischen Details einer SKU
Sie können der aufgeführten SKU eine neue Version hinzufügen und Ihr Angebot erneut veröffentlichen, indem Sie die unten angegebenen Schritte ausführen:

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com)an.
2. Navigieren Sie zur Registerkarte **VIRTUELLE COMPUTER** , und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **VM-IMAGES** .
4. Suchen Sie im Abschnitt **SKUs** der Registerkarte **VM-IMAGES** nach der SKU, die Sie aktualisieren möchten.
5. Fügen Sie anschließend eine neue Versionsnummer der SKU hinzu, und klicken Sie auf die Schaltfläche **+** . Die neue Version sollte das Format X.Y.Z haben, wobei X, Y, Z für ganze Zahlen stehen. Versionsänderungen sollten nur inkrementell sein.
6. Fügen Sie im Feld mit der **URL der Betriebssystem-VHD** den SAS-URI hinzu, der für die Betriebssystem-VHD erstellt wurde, und speichern Sie die Änderungen.
   
   > [!IMPORTANT]
   > Sie können die Anzahl der Datenträger einer aufgelisteten SKU nicht herauf-/herabsetzen. Sie müssen in diesem Fall eine neue SKU erstellen. Eine ausführliche Anleitung finden Sie im Abschnitt [3. Hinzufügen einer neuen SKU unter einem aufgelisteten Angebot](#3-how-to-add-a-new-sku-under-a-live-offer).
   > 
   > 
7. Navigieren Sie nach dem Vornehmen der Änderungen zur Registerkarte **VERÖFFENTLICHEN**, und klicken Sie auf die Schaltfläche **PUSH TO STAGING** (Für Stagingumgebung freigeben). Eine ausführliche Anleitung zum Testen des Angebots in der Stagingumgebung finden Sie unter diesem [Link](marketplace-publishing-vm-image-test-in-staging.md)
8. Navigieren Sie nach dem Testen des Angebots in der Stagingumgebung zur Registerkarte **VERÖFFENTLICHEN** im Veröffentlichungsportal, und klicken Sie auf die Schaltfläche **GENEHMIGUNG FÜR FREIGABE IN PRODUKTIONSUMGEBUNGEN ANFORDERN**, um das Angebot erneut im Azure Marketplace zu veröffentlichen.
   
    ![Abbildung](media/marketplace-publishing-vm-image-post-publishing/img01_07.png)

## <a name="2-how-to-update-the-non-technical-details-of-an-offer-or-a-sku"></a>2. Aktualisieren der nicht technischen Details eines Angebots oder einer SKU
Sie können die nicht technischen Details (Marketing, rechtliche Dinge, Support, Kategorien) Ihres Liveangebots der SKU im Azure Marketplace aktualisieren.

### <a name="21-update-the-offer-description-and-logos"></a>2.1 Aktualisieren der Angebotsbeschreibung und der Logos
Sie können die Angebotsdetails aktualisieren und das Angebot erneut veröffentlichen, indem Sie die unten angegebenen Schritte ausführen:

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com)an.
2. Navigieren Sie zur Registerkarte **VIRTUELLE COMPUTER** , und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **MARKETING** .
4. Klicken Sie auf die Schaltfläche **ENGLISCH (USA)** (bzw. ggf. die Option für Ihre Sprache).
5. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **DETAILS** . Im Abschnitt *BESCHREIBUNG* der Registerkarte **DETAILS** können Sie den Angebotstitel, die Angebotsübersicht und die ausführliche Angebotsübersicht aktualisieren und die Änderungen speichern.
   
   > [!NOTE]
   > Beachten Sie beim Aktualisieren der SKU-Details Folgendes: 
   > **Geben Sie für die Angebotsbeschreibung und die SKU-Beschreibung nicht den gleichen Text ein. Geben Sie für den SKU-Titel und die ausführliche Angebotsübersicht nicht den gleichen Text ein. Geben Sie für den SKU-Titel und die Angebotsübersicht nicht den gleichen Text ein.**
   > 
   > 
   
    ![Abbildung](media/marketplace-publishing-vm-image-post-publishing/img02.1_05.png)
6. Im Abschnitt *LOGOS* der Registerkarte **DETAILS** können Sie die Logos aktualisieren. Stellen Sie aber sicher, dass für die Logos die [Azure Marketplace-Richtlinien](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content) eingehalten werden (siehe Abschnitt „Schritt 1: Bereitstellen von Marketinginhalten im Marketplace > Details > Azure Marketplace-Richtlinien für Logos“).
   
   > [!NOTE]
   > Das Herologo ist optional. Sie können sich auch gegen das Hochladen eines Herologos entscheiden. Wenn aber ein Herologo hochgeladen wurde, kann es nicht mehr aus dem Veröffentlichungsportal gelöscht werden. In diesem Fall müssen Sie sich an die [Richtlinien für Herologos](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content) halten (siehe Abschnitt „Schritt 1: Bereitstellen von Marketinginhalten im Marketplace > Details > Zusätzliche Richtlinien für Herologobanner“).
   > 
   > 
7. Navigieren Sie nach dem Vornehmen der Änderungen zur Registerkarte **VERÖFFENTLICHEN**, und klicken Sie auf die Schaltfläche **PUSH TO STAGING** (Für Stagingumgebung freigeben). Eine ausführliche Anleitung zum Testen des Angebots in der Stagingumgebung finden Sie unter diesem [Link](marketplace-publishing-vm-image-test-in-staging.md).
8. Navigieren Sie nach dem Testen des Angebots in der Stagingumgebung zur Registerkarte **VERÖFFENTLICHEN** im Veröffentlichungsportal, und klicken Sie auf die Schaltfläche **GENEHMIGUNG FÜR FREIGABE IN PRODUKTIONSUMGEBUNGEN ANFORDERN**, um das Angebot erneut im Azure Marketplace zu veröffentlichen.
   
    ![Abbildung](media/marketplace-publishing-vm-image-post-publishing/img02.1_08.png)

### <a name="22-update-the-sku-description"></a>2.2. Aktualisieren der SKU-Beschreibung
Sie können die SKU-Details aktualisieren und das Angebot erneut veröffentlichen, indem Sie die unten angegebenen Schritte ausführen:

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com)
2. Navigieren Sie zur Registerkarte **VIRTUELLE COMPUTER** , und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **MARKETING** .
4. Klicken Sie auf die Schaltfläche **ENGLISCH (USA)** (bzw. ggf. die Option für Ihre Sprache).
5. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **PLÄNE** . Im Abschnitt *SKUs* der Registerkarte **PLÄNE** können Sie den SKU-Titel, die SKU-Übersicht und die SKU-Beschreibung aktualisieren und die Änderungen speichern.
   
   > [!NOTE]
   > Beachten Sie beim Aktualisieren der SKU-Details Folgendes:  **Geben Sie für die Angebotsbeschreibung und die SKU-Beschreibung nicht den gleichen Text ein. Geben Sie für den SKU-Titel und die ausführliche Angebotsübersicht nicht den gleichen Text ein. Geben Sie für den SKU-Titel und die Angebotsübersicht nicht den gleichen Text ein.**
   > 
   > 
6. Navigieren Sie nach dem Vornehmen der Änderungen zur Registerkarte **VERÖFFENTLICHEN**, und klicken Sie auf die Schaltfläche **PUSH TO STAGING** (Für Stagingumgebung freigeben). Eine ausführliche Anleitung zum Testen des Angebots in der Stagingumgebung finden Sie unter diesem Link.
7. Navigieren Sie nach dem Testen des Angebots in der Stagingumgebung zur Registerkarte **VERÖFFENTLICHEN** im Veröffentlichungsportal, und klicken Sie auf die Schaltfläche **GENEHMIGUNG FÜR FREIGABE IN PRODUKTIONSUMGEBUNGEN ANFORDERN**, um das Angebot erneut im Azure Marketplace zu veröffentlichen.
   
    ![Abbildung](media/marketplace-publishing-vm-image-post-publishing/img02.2_07.png)

### <a name="23-change-the-existing-links-or-add-new-links"></a>2.3 Ändern der vorhandenen Links oder Hinzufügen neuer Links
Sie können die vorhandenen Links ändern oder neue Links hinzufügen und das Angebot dann erneut veröffentlichen, indem Sie die unten angegebenen Schritte ausführen:

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com)
2. Navigieren Sie zur Registerkarte **VIRTUELLE COMPUTER** , und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **MARKETING** .
4. Klicken Sie auf die Schaltfläche **ENGLISCH (USA)** (bzw. ggf. die Option für Ihre Sprache).
5. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **LINKS** .
6. Wenn Sie einen neuen Link hinzufügen möchten, klicken Sie im Abschnitt *Links* auf die Schaltfläche **LINK HINZUFÜGEN** . Das Dialogfeld *Link hinzufügen* wird geöffnet. In diesem Dialogfeld können Sie die Felder „Titel“ und „URL“ des Links hinzufügen und die Änderungen speichern. Sie können einen beliebigen Link mit Informationen eingeben, die für die Kunden hilfreich sind.
7. Wenn Sie einen vorhandenen Link aktualisieren oder löschen möchten, können Sie den gewünschten Link auswählen und auf die Schaltfläche „Bearbeiten“ oder „Löschen“ klicken.
   
   > [!NOTE]
   > Stellen Sie sicher, dass die Links, die Sie in diesem Abschnitt eingegeben haben, richtig funktionieren. Während des Produktionsanforderungsprozesses werden diese Links überprüft.
   > 
   > 
8. Navigieren Sie nach dem Vornehmen der Änderungen zur Registerkarte **VERÖFFENTLICHEN**, und klicken Sie auf die Schaltfläche **PUSH TO STAGING** (Für Stagingumgebung freigeben). Eine ausführliche Anleitung zum Testen des Angebots in der Stagingumgebung finden Sie unter diesem [Link](marketplace-publishing-vm-image-test-in-staging.md).
9. Navigieren Sie nach dem Testen des Angebots in der Stagingumgebung zur Registerkarte **VERÖFFENTLICHEN** im Veröffentlichungsportal, und klicken Sie auf die Schaltfläche **GENEHMIGUNG FÜR FREIGABE IN PRODUKTIONSUMGEBUNGEN ANFORDERN**, um das Angebot erneut im Azure Marketplace zu veröffentlichen.
   
    ![Abbildung](media/marketplace-publishing-vm-image-post-publishing/img02.3_09-01.png)
   
    ![Abbildung](media/marketplace-publishing-vm-image-post-publishing/img02.3-2.png)

### <a name="24-change-an-existing-sample-image-or-add-a-new-sample-image"></a>2.4 Ändern eines vorhandenen Beispielbilds oder Hinzufügen eines neuen Beispielbilds
Sie können vorhandene Beispielbilder ändern oder neue Beispielbilder hinzufügen und das Angebot dann erneut veröffentlichen, indem Sie die unten angegebenen Schritte ausführen:

> [!NOTE]
> Unter [https://portal.azure.com](https://portal.azure.com)wird nur ein Beispielbild angezeigt.
> 
> 

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com)
2. Navigieren Sie zur Registerkarte **VIRTUELLE COMPUTER** , und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **MARKETING** .
4. Klicken Sie auf die Schaltfläche **ENGLISCH (USA)** (bzw. ggf. die Option für Ihre Sprache).
5. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **BEISPIELBILDER** .
6. Wenn Sie ein neues Beispielbild hinzufügen möchten, können Sie im Abschnitt *Beispielbilder* auf die Schaltfläche **NEUES BILD HOCHLADEN** klicken und die Änderungen speichern.
   
   > [!NOTE]
   > Das Hinzufügen von Beispielbildern ist ein optionaler Schritt.
   > 
   > 
7. Wenn Sie ein vorhandenes Beispielbild aktualisieren oder löschen möchten, können Sie nach dem gewünschten Beispielbild suchen und dann auf die Schaltfläche **BILD ERSETZEN** oder die Schaltfläche zum Löschen klicken.
8. Navigieren Sie nach dem Vornehmen der Änderungen zur Registerkarte **VERÖFFENTLICHEN**, und klicken Sie auf die Schaltfläche **PUSH TO STAGING** (Für Stagingumgebung freigeben). Eine ausführliche Anleitung zum Testen des Angebots in der Stagingumgebung finden Sie unter diesem [Link](marketplace-publishing-vm-image-test-in-staging.md).
9. Navigieren Sie nach dem Testen des Angebots in der Stagingumgebung zur Registerkarte **VERÖFFENTLICHEN** im Veröffentlichungsportal, und klicken Sie auf die Schaltfläche **GENEHMIGUNG FÜR FREIGABE IN PRODUKTIONSUMGEBUNGEN ANFORDERN**, um das Angebot erneut im Azure Marketplace zu veröffentlichen.
   
    ![Abbildung](media/marketplace-publishing-vm-image-post-publishing/img02.4_09.png)

### <a name="25-update-the-legal-content"></a>2.5 Aktualisieren von rechtlichen Inhalten
Sie können die rechtlichen Inhalte aktualisieren und das Angebot erneut veröffentlichen, indem Sie die unten angegebenen Schritte ausführen:

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com)
2. Navigieren Sie zur Registerkarte **VIRTUELLE COMPUTER** , und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **MARKETING** .
4. Klicken Sie auf die Schaltfläche **ENGLISCH (USA)** (bzw. ggf. die Option für Ihre Sprache).
5. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **RECHTLICHE HINWEISE** . Im Abschnitt *Rechtliche Hinweise* können Sie Ihre Richtlinien und Nutzungsbedingungen aktualisieren. Geben oder fügen Sie die Richtlinien/Bedingungen in das Textfeld *Nutzungsbedingungen* ein, und speichern Sie die Änderungen.
6. Die Nutzungsbedingungen dürfen maximal 1.000.000 Zeichen umfassen.
7. Navigieren Sie nach dem Vornehmen der Änderungen zur Registerkarte **VERÖFFENTLICHEN**, und klicken Sie auf die Schaltfläche **PUSH TO STAGING** (Für Stagingumgebung freigeben). Eine ausführliche Anleitung zum Testen des Angebots in der Stagingumgebung finden Sie unter diesem [Link](marketplace-publishing-vm-image-test-in-staging.md)
8. Navigieren Sie nach dem Testen des Angebots in der Stagingumgebung zur Registerkarte **VERÖFFENTLICHEN** im Veröffentlichungsportal, und klicken Sie auf die Schaltfläche **GENEHMIGUNG FÜR FREIGABE IN PRODUKTIONSUMGEBUNGEN ANFORDERN**, um das Angebot erneut im Azure Marketplace zu veröffentlichen.
   
    ![Abbildung](media/marketplace-publishing-vm-image-post-publishing/img02.5_08.png)

### <a name="26-update-the-support-information"></a>2.6 Aktualisieren der Supportinformationen
Sie können die Supportinformationen aktualisieren und das Angebot erneut veröffentlichen, indem Sie die unten angegebenen Schritte ausführen:

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com)
2. Navigieren Sie zur Registerkarte **VIRTUELLE COMPUTER** , und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **SUPPORT** .
4. Im Abschnitt *Engineering-Kontaktdaten* der Registerkarte **SUPPORT** können Sie die Kontaktdaten aktualisieren. Diese Daten werden nur für die interne Kommunikation zwischen dem Partner und Microsoft verwendet.
5. Im Abschnitt *Kundensupport* der Registerkarte **SUPPORT** können Sie die Support-Kontaktdaten wie **Name, E-Mail, Telefon** und die **Support-URL** aktualisieren. Diese Daten werden nur für die interne Kommunikation zwischen dem Partner und Microsoft verwendet.
   
   > [!NOTE]
   > Wenn Sie nur E-Mail-Support bereitstellen möchten, können Sie im Abschnitt **Kundensupport** eine Pseudotelefonnummer angeben. In diesem Fall wird stattdessen die von Ihnen angegebene E-Mail-Adresse verwendet.
   > 
   > 
6. Navigieren Sie nach dem Vornehmen der Änderungen zur Registerkarte **VERÖFFENTLICHEN**, und klicken Sie auf die Schaltfläche **PUSH TO STAGING** (Für Stagingumgebung freigeben). Eine ausführliche Anleitung zum Testen des Angebots in der Stagingumgebung finden Sie unter diesem [Link](marketplace-publishing-vm-image-test-in-staging.md)
7. Navigieren Sie nach dem Testen des Angebots in der Stagingumgebung zur Registerkarte **VERÖFFENTLICHEN** im Veröffentlichungsportal, und klicken Sie auf die Schaltfläche **GENEHMIGUNG FÜR FREIGABE IN PRODUKTIONSUMGEBUNGEN ANFORDERN**, um das Angebot erneut im Azure Marketplace zu veröffentlichen.
   
    ![Abbildung](media/marketplace-publishing-vm-image-post-publishing/img02.6_07.png)

### <a name="27-update-the-categories"></a>2.7 Aktualisieren der Kategorien
Sie können den Abschnitt „Kategorien“ für das Angebot aktualisieren und das Angebot erneut veröffentlichen, indem Sie die unten angegebenen Schritte ausführen:

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com)
2. Navigieren Sie zur Registerkarte **VIRTUELLE COMPUTER** , und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **KATEGORIEN** .
4. Im Abschnitt *Kategorien* können Sie die Kategorien für das Angebot aktualisieren und die Änderungen speichern. Sie können bis zu fünf Kategorien für den Azure Marketplace-Katalog auswählen.
5. Navigieren Sie nach dem Vornehmen der Änderungen zur Registerkarte **VERÖFFENTLICHEN**, und klicken Sie auf die Schaltfläche **PUSH TO STAGING** (Für Stagingumgebung freigeben). Eine ausführliche Anleitung zum Testen des Angebots in der Stagingumgebung finden Sie unter diesem [Link](marketplace-publishing-vm-image-test-in-staging.md)
6. Navigieren Sie nach dem Testen des Angebots in der Stagingumgebung zur Registerkarte **VERÖFFENTLICHEN** im Veröffentlichungsportal, und klicken Sie auf die Schaltfläche **GENEHMIGUNG FÜR FREIGABE IN PRODUKTIONSUMGEBUNGEN ANFORDERN**, um das Angebot erneut im Azure Marketplace zu veröffentlichen.
   
    ![Abbildung](media/marketplace-publishing-vm-image-post-publishing/img02.7_06.png)

## <a name="3-how-to-add-a-new-sku-under-a-listed-offer"></a>3. Hinzufügen einer neuen SKU unter einem aufgelisteten Angebot
Sie können unter Ihrem Liveangebot eine neue SKU hinzufügen, indem Sie die unten angegebenen Schritte ausführen:

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com)an.
2. Navigieren Sie zur Registerkarte **VIRTUELLE COMPUTER** , und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **SKUs** . Klicken Sie anschließend auf die Schaltfläche **SKU HINZUFÜGEN**.  Ein neues Dialogfeld wird geöffnet. Geben Sie eine SKU-ID in Kleinbuchstaben ein. Aktivieren Sie das Kontrollkästchen für das Abrechnungsmodell „Bring-Your-Own-License“ (BYOL), wenn Sie die neue SKU mit dem BYOL-Abrechnungsmodell veröffentlichen möchten. Andernfalls können Sie das Kontrollkästchen für BYOL deaktivieren. Klicken Sie anschließend auf das Häkchen im Dialogfeld, um eine neue SKU zu erstellen. Wenn Sie sich in Bezug auf die neue SKU gegen das BYOL-Abrechnungsmodell entschieden haben, wird das Abrechnungsmodell für die neue SKU automatisch auf „Stündlich“ festgelegt. Klicken Sie für „Is a free trial available?“ (Ist eine kostenlose Testversion verfügbar?) auf die Option „Ein Monat“, wenn Sie für das Abrechnungsmodell „Stündlich“ die kostenlose Testversion mit einer Dauer von 30 Tagen aktivieren möchten. Wählen Sie andernfalls „KEINE TESTVERSION“. [Hinweis: Die Option „Is a free trial available?“ (Ist eine kostenlose Testversion verfügbar?)  wird nur angezeigt, wenn Sie NICHT beim Erstellen der neuen SKU die Option BYOL gewählt haben.]
   
   > [!IMPORTANT]
   > Die Option „Hide this SKU from the Marketplace because it should always be bought via a solution template“ (SKU für Marketplace ausblenden, da sie immer über eine Lösungsvorlage erworben werden sollte) sollte nur dann auf „JA“ festgelegt werden, wenn Sie über die Genehmigung zum Veröffentlichen eines Lösungsvorlagenangebots im Azure Marketplace verfügen. Andernfalls sollte diese Option immer auf „NEIN“ festgelegt werden.
   > 
   > 
4. Klicken Sie im Menü auf der linken Seite jetzt auf die Registerkarte **VM-IMAGES** , und ermitteln Sie die neue SKU, die Sie erstellt haben.
5. Eine Anleitung zum Einrichten der neuen SKU finden Sie in SCHRITT 5 unter diesem [Link](marketplace-publishing-vm-image-creation.md#5-obtain-certification-for-your-vm-image) .
6. Informationen zum Hinzufügen des Marketingmaterials für die neue SKU finden Sie im Abschnitt „Schritt 1: Bereitstellen von Marketinginhalten im Marketplace > Details > Punkte 2 bis 5“ unter diesem [Link](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content).
7. Informationen zum Hinzufügen der Preisinformationen für die neue SKU finden Sie im Abschnitt 2.1. Legen Sie Ihre VM-Preise unter diesem [Link](marketplace-publishing-push-to-staging.md#step-2-set-your-prices)
8. Navigieren Sie nach dem Vornehmen der Änderungen zur Registerkarte **VERÖFFENTLICHEN**, und klicken Sie auf die Schaltfläche **PUSH TO STAGING** (Für Stagingumgebung freigeben). Eine ausführliche Anleitung zum Testen des Angebots in der Stagingumgebung finden Sie unter diesem [Link](marketplace-publishing-vm-image-test-in-staging.md)
9. Navigieren Sie nach dem Testen des Angebots in der Stagingumgebung zur Registerkarte **VERÖFFENTLICHEN** im Veröffentlichungsportal, und klicken Sie auf die Schaltfläche **GENEHMIGUNG FÜR FREIGABE IN PRODUKTIONSUMGEBUNGEN ANFORDERN**, um das Angebot erneut im Azure Marketplace zu veröffentlichen.
   
    ![Abbildung](media/marketplace-publishing-vm-image-post-publishing/img03_09-01.png)
   
    ![Abbildung](media/marketplace-publishing-vm-image-post-publishing/img03_09-02.png)

## <a name="4-how-to-change-the-data-disk-count-for-a-listed-sku"></a>4. Ändern der Anzahl der Datenträger für eine aufgelistete SKU
Sie können die Anzahl der Datenträger einer aufgelisteten SKU nicht herauf-/herabsetzen. Sie müssen in diesem Fall eine neue SKU erstellen. Eine ausführliche Anleitung finden Sie im Abschnitt [3. Hinzufügen einer neuen SKU unter einem Liveangebot](#3-how-to-add-a-new-sku-under-a-live-offer).

## <a name="5----how-to-delete-a-listed-offer-from-the-azure-marketplace"></a>5.    Löschen eines aufgelisteten Angebots aus dem Azure Marketplace
Bei einer Anforderung zum Entfernen eines Liveangebots müssen verschiedene Aspekte berücksichtigt werden. Führen Sie die unten angegebenen Schritte aus, um vom Supportteam Hilfe beim Entfernen eines aufgelisteten Angebots aus dem Azure Marketplace zu erhalten:

1. Erstellen Sie über diesen [Link](https://support.microsoft.com/en-us/getsupport?wf=0&tenant=ClassicCommercial&oaspworkflow=start_1.0.0.0&locale=en-us&supportregion=en-us&pesid=15635&ccsid=635993707583706681) ein Supportticket.
2. Wählen Sie als Problemtyp die Option **Angebotsverwaltung** und als Kategorie **Modifying an offer and/or SKU already in production** (Ändern eines Angebots und/oder einer SKU in der Produktion).
3. Senden Sie die Anforderung.

Das Supportteam führt Sie durch den Vorgang zum Löschen des Angebots bzw. der SKU.

> [!NOTE]
> Sie können das Angebot jederzeit löschen, während es sich im Entwurfsstatus befindet (also nicht im Status STAGING oder PRODUKTION), indem Sie auf der Registerkarte **VERLAUF** auf die Schaltfläche **ENTWURF VERWERFEN** klicken.
> 
> 

## <a name="6-how-to-delete-a-listed-sku-from-the-azure-marketplace"></a>6. Löschen einer aufgelisteten SKU aus dem Azure Marketplace
Sie können eine aufgelistete SKU mit den unten angegebenen Schritten aus dem Azure Marketplace löschen:

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com)an.
2. Navigieren Sie zur Registerkarte **VIRTUELLE COMPUTER** , und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **SKUs** .
4. Wählen Sie die SKU aus, die Sie löschen möchten, und klicken Sie auf die Schaltfläche „Löschen“ für diese SKU.
5. Navigieren Sie anschließend zur Registerkarte VERÖFFENTLICHEN im Veröffentlichungsportal, und klicken Sie auf die Schaltfläche **REQUEST APPROVAL TO PUSH TO PRODUCTION** (Genehmigung für Freigabe in Produktionsumgebungen anfordern), um das Angebot erneut im Azure Marketplace zu veröffentlichen.
6. Sobald das Angebot erneut im Azure Marketplace veröffentlicht wird, wird die SKU aus dem Azure Marketplace und Azure-Portal gelöscht.

## <a name="7-how-to-delete-the-current-version-of-a-listed-sku-from-the-azure-marketplace"></a>7. Löschen der aktuellen Version einer aufgelisteten SKU aus dem Azure Marketplace
Sie können die aktuelle Version einer aufgelisteten SKU mit den unten angegebenen Schritten aus dem Azure Marketplace löschen. Sobald der Prozess abgeschlossen ist, wird die SKU auf die vorherige Version zurückgesetzt.

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com)an.
2. Navigieren Sie zur Registerkarte **VIRTUELLE COMPUTER** , und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **VM-IMAGES** .
4. Wählen Sie die SKU aus, deren aktuelle Version Sie löschen möchten, und klicken Sie auf die Schaltfläche „Löschen“ für diese Version.
5. Navigieren Sie anschließend zur Registerkarte **VERÖFFENTLICHEN** im Veröffentlichungsportal, und klicken Sie auf die Schaltfläche **REQUEST APPROVAL TO PUSH TO PRODUCTION** (Genehmigung für Freigabe in Produktionsumgebungen anfordern), um das Angebot erneut im Azure Marketplace zu veröffentlichen.
6. Sobald das Angebot erneut im Azure Marketplace veröffentlicht wird, wird die aktuelle Version der aufgelisteten SKU aus dem Azure Marketplace und Azure-Portal gelöscht. Die SKU wird auf die vorherige Version zurückgesetzt.

## <a name="8-how-to-revert-listing-price-to-production-values"></a>8. Zurücksetzen des Listenpreises auf Produktionswerte
Ich habe den Preis einer aufgelisteten SKU geändert (oder die Abrechnungsregionen einer aufgelisteten SKU entfernt). Da dies in Azure Marketplace nicht unterstützt wird, möchte ich meine Änderungen auf die Produktionswerte zurücksetzen. Wie gehe ich dabei vor?

Bitte führen Sie die folgenden Schritte aus:

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com)an.
2. Navigieren Sie zur Registerkarte **VIRTUELLE COMPUTER** , und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **PREISE** .
4. Wählen Sie auf der Registerkarte „Preise“ eine Region, deren Preis Sie zurücksetzen möchten.
   
    ![Abbildung](media/marketplace-publishing-vm-image-post-publishing/img08-04.png)
5. Setzen Sie im Falle von SKUs mit stündlichem Abrechnungsmodell für die ausgewählte Region die Preise für alle Kerne auf die Produktionswerte zurück. Machen Sie im Falle von SKUs mit BYOL-Abrechnungsmodell die SKU durch Aktivieren des Kontrollkästchens für die SKU unter dem Abschnitt EXTERNALLY-LICENSED (BYOL) SKU AVAILABILITY (Extern lizenzierte SKU-Verfügbarkeit [BYOL]) in der Region verfügbar (siehe Screenshot unten).
   
    ![Abbildung](media/marketplace-publishing-vm-image-post-publishing/img08-05.png)
6. Klicken Sie nun auf die Schaltfläche **AUTOPRICE OTHER MARKETS BASED ON PRICES IN UNITED STATES**(Preise für andere Märkte basierend auf Preisen in den USA automatisch festlegen).
   
   > [!NOTE]
   > Die Beschriftung der Schaltfläche kann je nach der von Ihnen ausgewählten Region unterschiedlich sein. Da wir während der Erstellung dieses Dokuments die USA ausgewählt haben, trägt die Schaltfläche im Screenshot unten die Beschriftung „Auto price other markets based on prices in United States“.
   > 
   > 
   
    ![Abbildung](media/marketplace-publishing-vm-image-post-publishing/img08-06.png)
7. Der Assistent für automatische Preisfestlegung wird geöffnet. Die erste Seite zeigt die Auswahl für den Basismarkt an. Treffen Sie Ihre Wahl, und gehen Sie durch Klicken auf die Schaltfläche **„->“** zur nächsten Seite.
   
    ![Abbildung](media/marketplace-publishing-vm-image-post-publishing/img08-07.png)
8. Die Option zum Auswählen der Kerne und Pläne wird auf Seite 2 angezeigt. Wählen Sie die gewünschten Pläne und Kerne aus, und klicken Sie auf die Schaltfläche „->“.
   
    ![Abbildung](media/marketplace-publishing-vm-image-post-publishing/img08-08.png)
9. Seite 3 zeigt die Märkte bzw. Regionen an. Klicken Sie auf die Schaltfläche „Alle umschalten“, um alle Regionen auszuwählen, oder aktivieren Sie die Kontrollkästchen für die Regionen manuell. Klicken Sie auf die Schaltfläche „->“, um zur nächsten Seite zu wechseln.
   
    ![Abbildung](media/marketplace-publishing-vm-image-post-publishing/img08-09.png)
10. Seite 4 zeigt die Wechselkurse an. Klicken Sie auf die Schaltfläche „Fertig stellen“, um die Schritte auszuführen. Der Assistent setzt die Preise entsprechend Ihrer Auswahl zurück.
11. Wechseln Sie jetzt zur Preiseregisterkarte, und klicken Sie auf die Schaltfläche ÜBERSICHT UND ÄNDERUNGEN ANZEIGEN.
    Wählen Sie „Entwurf“ im Abschnitt „Version anzeigen“ und „Produktion“ im Abschnitt „Vergleichen mit“ (siehe Screenshot unten). Wenn kein Preisunterschied angezeigt wird, weist dies darauf hin, dass die Preise erfolgreich auf die Produktionswerte zurückgesetzt wurden.
    
    ![Abbildung](media/marketplace-publishing-vm-image-post-publishing/img08-11.png)
12. Navigieren Sie nach dem Vornehmen der Änderungen zur Registerkarte VERÖFFENTLICHEN, und klicken Sie auf die Schaltfläche **PUSH TO STAGING**(Für Stagingumgebung freigeben). Eine ausführliche Anleitung zum Testen des Angebots in der Stagingumgebung finden Sie unter diesem [Link](marketplace-publishing-vm-image-test-in-staging.md)
13. Navigieren Sie nach dem Testen des Angebots in der Stagingumgebung zur Registerkarte VERÖFFENTLICHEN im Veröffentlichungsportal, und klicken Sie auf die Schaltfläche **GENEHMIGUNG FÜR FREIGABE IN PRODUKTIONSUMGEBUNGEN ANFORDERN** (Genehmigung für Freigabe in Produktionsumgebungen anfordern), um das Angebot erneut im Azure Marketplace zu veröffentlichen.

## <a name="9-how-to-revert-billing-model-to-production-values"></a>9. Zurücksetzen des Abrechnungsmodells auf Produktionswerte
Ich habe das Abrechnungsmodell einer aufgelisteten SKU geändert. Da dies in Azure Marketplace nicht unterstützt wird, möchte ich meine Änderungen auf die Produktionswerte zurücksetzen. Wie gehe ich dabei vor?

Führen Sie die folgenden Schritte aus:

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com)an.
2. Navigieren Sie zur Registerkarte **VIRTUELLE COMPUTER** , und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **SKUs** .
4. Klicken Sie auf die Schaltfläche BEARBEITEN, um das Abrechnungsmodell zurückzusetzen. Daraufhin wird ein Fenster geöffnet. Aktivieren bzw. deaktivieren Sie das Kontrollkästchen **Abrechnung und Lizenzierung erfolgt extern durch Azure (auch bekannt als Bring-Your-Own-License)** entsprechend.
   
    ![Abbildung](media/marketplace-publishing-vm-image-post-publishing/img09-04.png)
5. Beachten Sie bitte anschließend die Antwort auf Frage 8 in diesem Dokument zum Zurücksetzen der Preise.
6. Navigieren Sie anschließend zur Registerkarte **VERÖFFENTLICHEN** im Veröffentlichungsportal, und überführen Sie das Angebot zum Testen in die Stagingumgebung. Klicken Sie nach dem Testen des Angebots auf die Schaltfläche **REQUEST APPROVAL TO PUSH TO PRODUCTION** (Genehmigung für Freigabe in Produktionsumgebungen anfordern), um das Angebot erneut im Azure Marketplace zu veröffentlichen.

## <a name="10-how-to-revert-visibility-setting-of-a-listed-sku-to-the-production-value"></a>10. Zurücksetzen der Sichtbarkeitseinstellung einer aufgelisteten SKU auf den Produktionswert
Führen Sie die folgenden Schritte aus:

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com)an.
2. Navigieren Sie zur Registerkarte **VIRTUELLE COMPUTER** , und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **SKUs** .
4. Wählen Sie Ihre SKU, und setzen Sie die Sichtbarkeitseinstellung der SKU auf den Produktionswert zurück.
   
    ![Abbildung](media/marketplace-publishing-vm-image-post-publishing/img10-04.png)
5. Klicken Sie nach den Änderungen auf die Schaltfläche **REQUEST APPROVAL TO PUSH TO PRODUCTION** (Genehmigung für Freigabe in Produktionsumgebungen anfordern), um das Angebot erneut im Azure Marketplace zu veröffentlichen.

## <a name="see-also"></a>Siehe auch
* [Erste Schritte: Veröffentlichen eines Angebots im Azure Marketplace](marketplace-publishing-getting-started.md)
* [Grundlegendes zu den Verkäufer-Insights-Berichten](marketplace-publishing-report-seller-insights.md)
* [Grundlegendes zu den Auszahlungsberichten](marketplace-publishing-report-payout.md)
* [Gewusst wie: Ändern Ihres „Reseller Incentive“ für Cloud-Lösungsanbieter](marketplace-publishing-csp-incentive.md)
* [Beheben häufiger Probleme bei der Veröffentlichung im Marketplace](marketplace-publishing-support-common-issues.md)
* [Zugriff auf die Publisher-Unterstützung](marketplace-publishing-get-publisher-support.md)
* [Erstellen eines lokalen VM-Images](marketplace-publishing-vm-image-creation-on-premise.md)
* [Erstellen eines virtuellen Computers unter Windows im Azure-Vorschauportal](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)




<!--HONumber=Nov16_HO3-->


