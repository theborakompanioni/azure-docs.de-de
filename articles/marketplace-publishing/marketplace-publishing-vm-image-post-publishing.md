---
title: Verwalten eines VM-Images im Azure Marketplace | Microsoft Docs
description: "Eine ausführliche Anleitung dazu, wie Sie Ihr VM-Image nach der ersten Veröffentlichung im Azure Marketplace verwalten"
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
ms.translationtype: Human Translation
ms.sourcegitcommit: c308183ffe6a01f4d4bf6f5817945629cbcedc92
ms.openlocfilehash: e1f90650e71345957c2d353774cb8bef62c1868b
ms.contentlocale: de-de
ms.lasthandoff: 05/17/2017


---
# <a name="post-production-guide-for-virtual-machine-offers-in-the-azure-marketplace"></a>Postproduktionsleitfaden für Virtual Machine-Angebote im Azure Marketplace
In diesem Artikel wird beschrieben, wie Sie ein Virtual Machine-Liveangebot im Azure Marketplace aktualisieren können. Sie werden durch die Schritte zum Hinzufügen einer oder mehrerer neuer SKUs zu einem vorhandenen Angebot geführt. Außerdem wird beschrieben, wie Sie ein VM-Liveangebot oder eine SKU vom Marketplace entfernen.

Sobald ein Angebot/eine SKU im [Azure-Portal](http://portal.azure.com) bereitgestellt worden ist, können Sie die folgenden Textfelder nicht mehr ändern:

* **Angebotsbezeichner**: Navigieren Sie im Veröffentlichungsportal zu **Virtual Machines**, und wählen Sie Ihr Angebot aus. Klicken Sie dann auf **VM-IMAGES** > **Angebotsbezeichner**.
* **SKU-Bezeichner**: Navigieren Sie im Veröffentlichungsportal zu **Virtual Machines**, und wählen Sie Ihr Angebot aus. Klicken Sie dann auf **SKUS** > **SKU hinzufügen**.
* **Herausgebernamespace:**: Navigieren Sie im Veröffentlichungsportal zu **Virtual Machines** > **Vorgehensweise** > **Informationen zu Ihrem Unternehmen** (unter „Schritt 2: Registrieren Sie Ihr Unternehmen“)**Herausgebernamespace** > **Namespace**.

Sobald ein Angebot/eine SKU im [Marketplace](http://azure.microsoft.com/marketplace) aufgeführt ist, können Sie die folgenden Textfelder nicht mehr ändern:

* **Angebotsbezeichner**: Navigieren Sie im Veröffentlichungsportal zu **Virtual Machines**, und wählen Sie Ihr Angebot aus. Klicken Sie dann auf **VM-IMAGES** > **Angebotsbezeichner**.
* **SKU-Bezeichner**: Navigieren Sie im Veröffentlichungsportal zu **Virtual Machines**, und wählen Sie Ihr Angebot aus. Klicken Sie dann auf **SKUS** > **SKU hinzufügen**.
* **Herausgebernamespace:**: Navigieren Sie im Veröffentlichungsportal zu **Virtual Machines** > **Vorgehensweise** > **Informationen zu Ihrem Unternehmen** (unter „Schritt 2: Registrieren“)**Herausgebernamespace** > **Namespace**.
* **Ports**: Navigieren Sie im Veröffentlichungsportal zu **Virtual Machines**, und wählen Sie Ihr Angebot aus. Klicken Sie dann auf **VM-IMAGES** > **Offene Ports**.
* **Preisänderung bei aufgelisteten SKUs**
* **Abrechnungsmodelländerung bei aufgelisteten SKUs**
* **Entfernen von Abrechnungsregionen bei aufgelisteten SKUs**
* **Ändern der Anzahl der Datenträger bei aufgelisteten SKUs**

## <a name="update-the-technical-details-of-a-sku"></a>Aktualisieren der technischen Details einer SKU
Um eine neue Version der aufgelisteten SKU hinzuzufügen und Ihr Angebot erneut zu veröffentlichen, gehen Sie folgendermaßen vor:

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com) an.
2. Navigieren Sie zur Registerkarte **Virtual Machines** , und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **VM-IMAGES**.
4. Suchen Sie im Abschnitt **SKUs** nach der SKU, die Sie aktualisieren möchten.
5. Fügen Sie eine neue Versionsnummer für die SKU hinzu, und klicken Sie auf die Schaltfläche **+**. Die neue Version sollte das Format X.Y.Z haben, wobei X, Y und Z für ganze Zahlen stehen. Versionsänderungen sollten nur inkrementell sein.
6. Geben Sie im Feld mit der **URL der Betriebssystem-VHD** den SAS-URI ein, der für die Betriebssystem-VHD erstellt wurde, und speichern Sie die Änderungen.

   > [!IMPORTANT]
   > Sie können die Anzahl der Datenträger einer aufgelisteten SKU nicht herauf-/herabsetzen. Sie müssen in diesem Fall eine neue SKU erstellen. Ausführliche Anweisungen finden Sie im Abschnitt [Hinzufügen einer neuen SKU unter einem aufgelisteten Angebot](#add-a-new-sku-under-a-listed-offer).
   >
   >
7. Wechseln Sie zur Registerkarte **VERÖFFENTLICHEN**, und klicken Sie auf **FÜR STAGINGUMGEBUNG FREIGEBEN**. Eine ausführliche Anleitung zum Testen Ihres Angebots in der Stagingumgebung finden Sie unter [Testen Ihres VM-Angebots für Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
8. Nachdem Sie Ihr Angebot in der Stagingumgebung getestet haben, wechseln Sie im Veröffentlichungsportal zur Registerkarte **VERÖFFENTLICHEN**. Klicken Sie auf **GENEHMIGUNG FÜR FREIGABE IN PRODUKTIONSUMGEBUNG ANFORDERN**, um Ihr Angebot im Marketplace erneut zu veröffentlichen.

    ![VM-Images](media/marketplace-publishing-vm-image-post-publishing/img01_07.png)

## <a name="update-the-nontechnical-details-of-an-offer-or-a-sku"></a>Aktualisieren der nicht technischen Details eines Angebots oder einer SKU
Sie können die nicht technischen Details (Marketing, rechtliche Dinge, Support, Kategorien) Ihres Liveangebots oder Ihrer SKU im Marketplace aktualisieren.

### <a name="update-the-offer-description-and-logos"></a>Aktualisieren der Angebotsbeschreibung und der Logos
Gehen Sie zum Aktualisieren der Angebotsdetails und zum erneuten Veröffentlichen Ihres Angebots folgendermaßen vor:

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com) an.
2. Navigieren Sie zur Registerkarte **Virtual Machines**, und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **MARKETING**.
4. Klicken Sie auf **English (US)**.
5. Klicken Sie auf die Registerkarte **DETAILS**. Aktualisieren Sie im Abschnitt **Beschreibung** den **TITEL**, die **ÜBERSICHT** und die **AUSFÜHRLICHE ÜBERSICHT** für das Angebot, und speichern Sie die Einstellungen.

   > [!NOTE]
   > Wenn Sie die Details für die SKU aktualisieren, beachten Sie diese Einschränkungen: 
   * Geben Sie für die Angebotsbeschreibung und die SKU-Beschreibung nicht den gleichen Text ein.
   * Geben Sie für den SKU-Titel und die ausführliche Angebotsübersicht nicht den gleichen Text ein. 
   * Geben Sie für den SKU-Titel und die Angebotsübersicht nicht den gleichen Text ein.
   >
   >

    ![Details](media/marketplace-publishing-vm-image-post-publishing/img02.1_05.png)
6. Aktualisieren Sie im Abschnitt **LOGOS** der Registerkarte **DETAILS** die Logos. Stellen Sie sicher, dass die Logos die [Azure Marketplace-Richtlinien](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content) erfüllen.

   > [!NOTE]
   > Ein Herologo ist optional. Sie können sich auch gegen das Hochladen eines Herologos entscheiden. Wenn aber ein Herologo hochgeladen wurde, kann es nicht mehr aus dem Veröffentlichungsportal gelöscht werden. Befolgen Sie die [Richtlinien für Herologos](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content).
   >
   >
7. Wechseln Sie zur Registerkarte **VERÖFFENTLICHEN**, und klicken Sie auf **FÜR STAGINGUMGEBUNG FREIGEBEN**. Eine ausführliche Anleitung zum Testen Ihres Angebots in der Stagingumgebung finden Sie unter [Testen Ihres VM-Angebots für Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
8. Nachdem Sie Ihr Angebot in der Stagingumgebung getestet haben, wechseln Sie im Veröffentlichungsportal zur Registerkarte **VERÖFFENTLICHEN**. Klicken Sie auf **GENEHMIGUNG FÜR FREIGABE IN PRODUKTIONSUMGEBUNG ANFORDERN**, um Ihr Angebot im Marketplace erneut zu veröffentlichen.

    ![Logos](media/marketplace-publishing-vm-image-post-publishing/img02.1_08.png)

### <a name="update-the-sku-description"></a>Aktualisieren der SKU-Beschreibung
Gehen Sie zum Aktualisieren der SKU-Details und zum erneuten Veröffentlichen Ihres Angebots folgendermaßen vor:

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com) an.
2. Navigieren Sie zur Registerkarte **Virtual Machines**, und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **MARKETING**.
4. Klicken Sie auf **English (US)**.
5. Klicken Sie auf die Registerkarte **PLÄNE**. Aktualisieren Sie im Abschnitt **SKUs** den **TITEL**, die **ÜBERSICHT** und die **BESCHREIBUNG** der SKU, und speichern Sie die Änderungen.

   > [!NOTE]
   > Wenn Sie die Details für die SKU aktualisieren, beachten Sie diese Einschränkungen: 
   * Geben Sie für die Angebotsbeschreibung und die SKU-Beschreibung nicht den gleichen Text ein. 
   * Geben Sie für den SKU-Titel und die ausführliche Angebotsübersicht nicht den gleichen Text ein. 
   * Geben Sie für den SKU-Titel und die Angebotsübersicht nicht den gleichen Text ein.
   >
   >
6. Wechseln Sie zur Registerkarte **VERÖFFENTLICHEN**, und klicken Sie auf **FÜR STAGINGUMGEBUNG FREIGEBEN**. Eine ausführliche Anleitung zum Testen Ihres Angebots in der Stagingumgebung finden Sie unter [Testen Ihres VM-Angebots für Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
7. Nachdem Sie Ihr Angebot in der Stagingumgebung getestet haben, wechseln Sie im Veröffentlichungsportal zur Registerkarte **VERÖFFENTLICHEN**. Klicken Sie auf **GENEHMIGUNG FÜR FREIGABE IN PRODUKTIONSUMGEBUNG ANFORDERN**, um Ihr Angebot im Marketplace erneut zu veröffentlichen.

    ![Pläne](media/marketplace-publishing-vm-image-post-publishing/img02.2_07.png)

### <a name="change-existing-links-or-add-new-links"></a>Ändern vorhandener Links oder Hinzufügen neuer Links
Gehen Sie zum Ändern vorhandener Links oder zum Hinzufügen neuer Links und zum anschließenden erneuten Veröffentlichen Ihres Angebots folgendermaßen vor:

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com) an.
2. Navigieren Sie zur Registerkarte **Virtual Machines**, und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **MARKETING**.
4. Klicken Sie auf **English (US)**.
5. Klicken Sie auf die Registerkarte **LINKS**.
6. Um einen neuen Link hinzuzufügen, klicken Sie im Abschnitt **Links** auf **+ LINK HINZUFÜGEN**. Geben Sie im Dialogfeld **Link hinzufügen** den **TITEL** und die **URL** für den Link ein, und speichern Sie die Änderungen. Sie können einen beliebigen Link mit Informationen eingeben, die für die Kunden hilfreich sein können.
7. Um einen vorhandenen Link zu aktualisieren oder zu löschen, wählen Sie den gewünschten Link aus, und klicken Sie auf die Schaltfläche **Bearbeiten** oder **Löschen**.

   > [!NOTE]
   > Stellen Sie sicher, dass die Links, die Sie in diesem Abschnitt eingegeben haben, richtig funktionieren. Während des Produktionsanforderungsprozesses werden diese Links überprüft.
   >
   >
8. Wechseln Sie zur Registerkarte **VERÖFFENTLICHEN**, und klicken Sie auf **FÜR STAGINGUMGEBUNG FREIGEBEN**. Eine ausführliche Anleitung zum Testen Ihres Angebots in der Stagingumgebung finden Sie unter [Testen Ihres VM-Angebots für Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
9. Nachdem Sie Ihr Angebot in der Stagingumgebung getestet haben, wechseln Sie im Veröffentlichungsportal zur Registerkarte **VERÖFFENTLICHEN**. Klicken Sie auf **GENEHMIGUNG FÜR FREIGABE IN PRODUKTIONSUMGEBUNG ANFORDERN**, um Ihr Angebot im Marketplace erneut zu veröffentlichen.

    ![Links](media/marketplace-publishing-vm-image-post-publishing/img02.3_09-01.png)

    ![Hinzufügen eines Links](media/marketplace-publishing-vm-image-post-publishing/img02.3-2.png)

### <a name="change-an-existing-sample-image-or-add-a-new-sample-image"></a>Ändern eines vorhandenen Beispielbilds oder Hinzufügen eines neuen Beispielbilds
Um vorhandene Beispielbilder zu ändern oder neue Beispielbilder hinzuzufügen und das Angebot dann erneut zu veröffentlichen, führen Sie die folgenden Schritte aus:

> [!NOTE]
> Im [Azure-Portal](https://portal.azure.com)wird nur ein Beispielbild angezeigt.
>
>

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com) an.
2. Navigieren Sie zur Registerkarte **Virtual Machines**, und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **MARKETING**.
4. Klicken Sie auf **English (US)**.
5. Klicken Sie auf die Registerkarte **BEISPIELBILDER**.
6. Klicken Sie zum Hinzufügen eines neuen Beispielbilds im Abschnitt **Beispielbilder** auf **NEUES BILD HOCHLADEN**, und speichern Sie die Änderungen.

   > [!NOTE]
   > Das Hinzufügen von Beispielbildern ist optional.
   >
7. Wechseln Sie zur Registerkarte **VERÖFFENTLICHEN**, und klicken Sie auf **FÜR STAGINGUMGEBUNG FREIGEBEN**. Eine ausführliche Anleitung zum Testen Ihres Angebots in der Stagingumgebung finden Sie unter [Testen Ihres VM-Angebots für Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
8. Nachdem Sie Ihr Angebot in der Stagingumgebung getestet haben, wechseln Sie im Veröffentlichungsportal zur Registerkarte **VERÖFFENTLICHEN**. Klicken Sie auf **GENEHMIGUNG FÜR FREIGABE IN PRODUKTIONSUMGEBUNG ANFORDERN**, um Ihr Angebot im Marketplace erneut zu veröffentlichen.

    ![Beispielbilder](media/marketplace-publishing-vm-image-post-publishing/img02.4_09.png)

### <a name="update-the-legal-content"></a>Aktualisieren von rechtlichen Inhalten
Gehen Sie zum Aktualisieren von rechtlichen Inhalten und zum erneuten Veröffentlichen Ihres Angebots folgendermaßen vor:

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com) an.
2. Navigieren Sie zur Registerkarte **Virtual Machines**, und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **MARKETING**.
4. Klicken Sie auf **English (US)**.
5. Klicken Sie auf die Registerkarte **RECHTLICHE HINWEISE**. Aktualisieren Sie im Abschnitt **Rechtliche Hinweise** Ihre Richtlinien und Nutzungsbedingungen. Geben oder fügen Sie die Richtlinien/Bedingungen in das Textfeld **NUTZUNGSBEDINGUNGEN** ein, und speichern Sie die Änderungen.
6. Die Nutzungsbedingungen dürfen maximal 1 Million Zeichen umfassen.
7. Wechseln Sie zur Registerkarte **VERÖFFENTLICHEN**, und klicken Sie auf **FÜR STAGINGUMGEBUNG FREIGEBEN**. Eine ausführliche Anleitung zum Testen Ihres Angebots in der Stagingumgebung finden Sie unter [Testen Ihres VM-Angebots für Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
8. Nachdem Sie Ihr Angebot in der Stagingumgebung getestet haben, wechseln Sie im Veröffentlichungsportal zur Registerkarte **VERÖFFENTLICHEN**. Klicken Sie auf **GENEHMIGUNG FÜR FREIGABE IN PRODUKTIONSUMGEBUNG ANFORDERN**, um Ihr Angebot im Marketplace erneut zu veröffentlichen.

    ![Rechtliche Hinweise](media/marketplace-publishing-vm-image-post-publishing/img02.5_08.png)

### <a name="update-the-support-information"></a>Aktualisieren der Supportinformationen
Gehen Sie zum Aktualisieren der Supportinformationen und zum erneuten Veröffentlichen Ihres Angebots folgendermaßen vor:

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com) an.
2. Navigieren Sie zur Registerkarte **Virtual Machines**, und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **SUPPORT**.
4. Aktualisieren Sie im Abschnitt **Engineering-Kontaktdaten** die betreffenden Daten. Diese Daten werden nur für die interne Kommunikation zwischen dem Partner und Microsoft verwendet.
5. Aktualisieren Sie im Abschnitt **Kundensupport** die entsprechenden Kontaktdaten und die **SUPPORT-URL**. Diese Daten werden nur für die interne Kommunikation zwischen dem Partner und Microsoft verwendet.

   > [!NOTE]
   > Wenn Sie nur E-Mail-Support bereitstellen möchten, geben Sie im Abschnitt **Kundensupport** eine Pseudotelefonnummer ein. In diesem Fall wird stattdessen die eingegebene E-Mail-Adresse verwendet.
   >
   >
6. Wechseln Sie zur Registerkarte **VERÖFFENTLICHEN**, und klicken Sie auf **FÜR STAGINGUMGEBUNG FREIGEBEN**. Eine ausführliche Anleitung zum Testen Ihres Angebots in der Stagingumgebung finden Sie unter [Testen Ihres VM-Angebots für Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
7. Nachdem Sie Ihr Angebot in der Stagingumgebung getestet haben, wechseln Sie im Veröffentlichungsportal zur Registerkarte **VERÖFFENTLICHEN**. Klicken Sie auf **GENEHMIGUNG FÜR FREIGABE IN PRODUKTIONSUMGEBUNG ANFORDERN**, um Ihr Angebot im Marketplace erneut zu veröffentlichen.

    ![Support](media/marketplace-publishing-vm-image-post-publishing/img02.6_07.png)

### <a name="update-the-categories"></a>Aktualisieren der Kategorien
Um die Kategorien für das Angebot zu aktualisieren und das Angebot erneut zu veröffentlichen, führen Sie die folgenden Schritte aus:

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com) an.
2. Navigieren Sie zur Registerkarte **Virtual Machines**, und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **KATEGORIEN**.
4. Aktualisieren Sie im Abschnitt **Kategorien** die Kategorien für das Angebot, und speichern Sie die Änderungen. Sie können bis zu fünf Kategorien für den Azure Marketplace-Katalog auswählen.
5. Wechseln Sie zur Registerkarte **VERÖFFENTLICHEN**, und klicken Sie auf **FÜR STAGINGUMGEBUNG FREIGEBEN**. Eine ausführliche Anleitung zum Testen Ihres Angebots in der Stagingumgebung finden Sie unter [Testen Ihres VM-Angebots für Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
6. Nachdem Sie Ihr Angebot in der Stagingumgebung getestet haben, wechseln Sie im Veröffentlichungsportal zur Registerkarte **VERÖFFENTLICHEN**. Klicken Sie auf **GENEHMIGUNG FÜR FREIGABE IN PRODUKTIONSUMGEBUNG ANFORDERN**, um Ihr Angebot im Marketplace erneut zu veröffentlichen.

    ![Categories](media/marketplace-publishing-vm-image-post-publishing/img02.7_06.png)

## <a name="add-a-new-sku-under-a-listed-offer"></a>Hinzufügen einer neuen SKU unter einem aufgelisteten Angebot
Gehen Sie zum Hinzufügen einer neuen SKU in Ihrem Liveangebot folgendermaßen vor:

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com) an.
2. Navigieren Sie zur Registerkarte **Virtual Machines**, und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **SKUS**. Klicken Sie anschließend auf **SKU hinzufügen**. 
4. Geben Sie im Dialogfeld einen **SKU-Bezeichner** in Kleinbuchstaben ein. Aktivieren Sie das Kontrollkästchen für das **Abrechnungsmodell „Bring-Your-Own-License“ (BYOL)**, wenn Sie die neue SKU mit dem BYOL-Abrechnungsmodell veröffentlichen möchten. Andernfalls deaktivieren Sie das Kontrollkästchen. Klicken Sie auf das Häkchen, um eine neue SKU zu erstellen. Wenn Sie das Abrechnungsmodell BYOL nicht ausgewählt haben, wird das Abrechnungsmodell automatisch auf stündlich festgelegt. Wenn Sie die kostenlose 30-Tage-Testversion für das stündliche Abrechnungsmodell verwenden möchten, wählen Sie **Ein Monat** für **Ist eine kostenlose Testversion verfügbar?** Wählen Sie andernfalls **Keine Testversion**. (**Ist eine kostenlose Testversion verfügbar?** wird nur angezeigt, wenn Sie beim Erstellen der neuen SKU BYOL ausgewählt haben.)

   > [!IMPORTANT]
   > Die Option **SKU für Marketplace ausblenden, da sie immer über eine Lösungsvorlage erworben werden sollte** sollte *nur dann* auf **Ja** festgelegt werden, wenn Sie über die Genehmigung zum Veröffentlichen einer Lösungsvorlage verfügen. Andernfalls sollte diese Option immer auf **Nein** festgelegt werden.
   >
   >
4. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **VM-IMAGES**, und suchen Sie die neue SKU, die Sie erstellt haben.
5. Informationen zum Einrichten der neuen SKU finden Sie unter [Erwerb der Zertifizierung für das VM-Image](marketplace-publishing-vm-image-creation.md#5-obtain-certification-for-your-vm-image).
6. Informationen zum Hinzufügen von Marketingmaterial für die neue SKU finden Sie unter [Bereitstellen von Marketinginhalten im Marketplace](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content).
7. Informationen zum Hinzufügen von Preisinformationen für die neue SKU finden Sie unter [Festlegen der Preise](marketplace-publishing-push-to-staging.md#step-2-set-your-prices).
8. Wechseln Sie zur Registerkarte **VERÖFFENTLICHEN**, und klicken Sie auf **FÜR STAGINGUMGEBUNG FREIGEBEN**. Eine ausführliche Anleitung zum Testen Ihres Angebots in der Stagingumgebung finden Sie unter [Testen Ihres VM-Angebots für Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
9. Nachdem Sie Ihr Angebot in der Stagingumgebung getestet haben, wechseln Sie im Veröffentlichungsportal zur Registerkarte **VERÖFFENTLICHEN**. Klicken Sie auf **GENEHMIGUNG FÜR FREIGABE IN PRODUKTIONSUMGEBUNG ANFORDERN**, um Ihr Angebot im Marketplace erneut zu veröffentlichen.

    ![SKUs](media/marketplace-publishing-vm-image-post-publishing/img03_09-01.png)

    ![Hinzufügen einer SKU](media/marketplace-publishing-vm-image-post-publishing/img03_09-02.png)

## <a name="change-the-data-disk-count-for-a-listed-sku"></a>Ändern der Anzahl der Datenträger für eine aufgelistete SKU
Sie können die Anzahl der Datenträger einer aufgelisteten SKU nicht herauf-/herabsetzen. Sie müssen in diesem Fall eine neue SKU erstellen. Ausführliche Anweisungen finden Sie im Abschnitt [Hinzufügen einer neuen SKU unter einem aufgelisteten Angebot](#add-a-new-sku-under-a-listed-offer).

## <a name="delete-a-listed-offer-from-the-marketplace"></a>Löschen eines aufgelisteten Angebots aus dem Marketplace
Bei einer Anforderung zum Entfernen eines Liveangebots müssen verschiedene Aspekte berücksichtigt werden. Führen Sie die unten angegebenen Schritte aus, um vom Supportteam Hilfe beim Entfernen eines aufgelisteten Angebots aus dem Marketplace zu erhalten:

1. Erstellen Sie auf der Seite [Create an incident](https://support.microsoft.com/en-us/getsupport?wf=0&tenant=ClassicCommercial&oaspworkflow=start_1.0.0.0&locale=en-us&supportregion=en-us&pesid=15635&ccsid=635993707583706681) (Serviceanfrage erstellen) ein Supportticket.

2. Wählen Sie als **Problem type** (Problemtyp) die Option **Managing offers** (Angebotsverwaltung) aus, und legen Sie als **Category** (Kategorie) **Modifying an offer and/or SKU already in production** (Ändern eines Angebots und/oder einer SKU in der Produktion) fest.
3. Senden Sie die Anforderung.

Das Supportteam führt Sie durch den Vorgang zum Löschen des Angebots bzw. der SKU.

> [!NOTE]
> Solange das Angebot den Status „Entwurf“ (nicht „Staging“ oder „Produktion“) hat, kann es jederzeit gelöscht werden. Klicken Sie auf der Registerkarte **VERLAUF** auf **ENTWURF VERWERFEN**.
>
>

## <a name="delete-a-listed-sku-from-the-marketplace"></a>Löschen einer aufgelisteten SKU aus dem Marketplace
Gehen Sie zum Löschen einer aufgelisteten SKU aus dem Marketplace folgendermaßen vor:

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com) an.

2. Navigieren Sie zur Registerkarte **Virtual Machines**, und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Bereich auf der linken Seite auf die Registerkarte **SKUS**.
4. Wählen Sie die SKU aus, die Sie löschen möchten, und klicken Sie auf die Schaltfläche **Löschen**.
5. Wechseln Sie im Veröffentlichungsportal zur Registerkarte **VERÖFFENTLICHEN**. Klicken Sie auf **GENEHMIGUNG FÜR FREIGABE IN PRODUKTIONSUMGEBUNG ANFORDERN**, um das Angebot im Marketplace erneut zu veröffentlichen.
6. Sobald das Angebot erneut im Marketplace veröffentlicht wird, wird die SKU aus dem Marketplace und dem Azure-Portal gelöscht.

## <a name="delete-the-current-version-of-a-listed-sku-from-the-marketplace"></a>Löschen der aktuellen Version einer aufgelisteten SKU aus dem Marketplace
Gehen Sie zum Löschen der aktuellen Version einer aufgelisteten SKU aus dem Marketplace folgendermaßen vor: 

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com) an.

2. Navigieren Sie zur Registerkarte **Virtual Machines**, und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **VM-IMAGES**.
4. Wählen Sie die SKU aus, deren aktuelle Version Sie löschen möchten, und klicken Sie auf die Schaltfläche **Löschen**.
5. Wechseln Sie im Veröffentlichungsportal zur Registerkarte **VERÖFFENTLICHEN**. Klicken Sie auf **GENEHMIGUNG FÜR FREIGABE IN PRODUKTIONSUMGEBUNG ANFORDERN**, um das Angebot im Marketplace erneut zu veröffentlichen.
6. Sobald das Angebot erneut im Marketplace veröffentlicht wird, wird die aktuelle Version der aufgelisteten SKU aus dem Marketplace und dem Azure-Portal gelöscht. Die SKU wird dann auf die vorherige Version zurückgesetzt.

## <a name="revert-the-listing-price-to-production-values"></a>Zurücksetzen des Listenpreises auf Produktionswerte
Gehen Sie zum Zurücksetzen des Listenpreises auf Produktionswerte folgendermaßen vor:

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com) an.
2. Navigieren Sie zur Registerkarte **Virtual Machines**, und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **PREISE**.
4. Wählen Sie eine Region aus, deren Preis Sie zurücksetzen möchten.

    ![Preisregionen](media/marketplace-publishing-vm-image-post-publishing/img08-04.png)
5. Setzen Sie für SKUs mit einem stündlichen Abrechnungsmodell für die ausgewählte Region die Preise für alle Kerne auf die Produktionswerte zurück. Machen Sie im Falle von SKUs mit BYOL-Abrechnungsmodell die SKU durch Aktivieren des Kontrollkästchens für die SKU im Abschnitt **EXTERNALLY-LICENSED (BYOL) SKU AVAILABILITY** (Extern lizenzierte SKU-Verfügbarkeit [BYOL]) in der Region verfügbar.

    ![Preismodelle](media/marketplace-publishing-vm-image-post-publishing/img08-05.png)
6. Klicken Sie auf **AUTOPRICE OTHER MARKETS BASED ON PRICES IN UNITED STATES**(Preise für andere Märkte basierend auf Preisen in den USA automatisch festlegen).

   > [!NOTE]
   > Die Beschriftung der Schaltfläche kann je nach der von Ihnen ausgewählten Region unterschiedlich sein. Da wir die USA ausgewählt haben, heißt die Schaltfläche **AUTOPRICE OTHER MARKETS BASED ON PRICES IN UNITED STATES**.
   >
   >

    ![Autoprice (Preise automatisch festlegen)](media/marketplace-publishing-vm-image-post-publishing/img08-06.png)
7. Wählen Sie auf Seite 1 des Autoprice-Assistenten den Basismarkt aus, und klicken Sie auf die Schaltfläche mit dem **Pfeil**.

    ![Basismarkt](media/marketplace-publishing-vm-image-post-publishing/img08-07.png)
8. Wählen Sie auf Seite 2 Diensttarife und Verbrauchseinheiten (Kerne) aus, und klicken Sie auf die Schaltfläche mit dem **Pfeil**.

    ![Diensttarife und Verbrauchseinheiten](media/marketplace-publishing-vm-image-post-publishing/img08-08.png)
9. Klicken Sie auf Seite 3 auf **Toggle all** (Alle umschalten), um alle Regionen auszuwählen. Sie können auch die Kontrollkästchen für bestimmte Regionen manuell aktivieren. Klicken Sie auf die Schaltfläche mit dem **Pfeil**.

    ![Auswählen von Märkten](media/marketplace-publishing-vm-image-post-publishing/img08-09.png)
10. Überprüfen Sie auf Seite 4 die Wechselkurse, und klicken Sie auf **Fertig stellen**. Der Assistent setzt die Preise entsprechend Ihrer Auswahl zurück.

11. Klicken Sie auf der Registerkarte **PREISE** auf **ZUSAMMENFASSUNG UND ÄNDERUNGEN ANZEIGEN**.
    Wählen Sie für **Version anzeigen** die Option **Entwurf** und für **Vergleichen mit** die Option **Produktion**. Wenn kein Preisunterschied angezeigt wird, wurden die Preise erfolgreich auf die Produktionswerte zurückgesetzt.

    ![Anzeigen der Zusammenfassung und Änderungen](media/marketplace-publishing-vm-image-post-publishing/img08-11.png)
12. Wechseln Sie zur Registerkarte **VERÖFFENTLICHEN**, und klicken Sie auf **FÜR STAGINGUMGEBUNG FREIGEBEN**. Eine ausführliche Anleitung zum Testen Ihres Angebots in der Stagingumgebung finden Sie unter [Testen Ihres VM-Angebots für Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
13. Nachdem Sie Ihr Angebot in der Stagingumgebung getestet haben, wechseln Sie im Veröffentlichungsportal zur Registerkarte **VERÖFFENTLICHEN**. Klicken Sie auf **GENEHMIGUNG FÜR FREIGABE IN PRODUKTIONSUMGEBUNG ANFORDERN**, um Ihr Angebot im Marketplace erneut zu veröffentlichen.

## <a name="revert-the-billing-model-to-production-values"></a>Zurücksetzen des Abrechnungsmodells auf Produktionswerte
Gehen Sie zum Zurücksetzen des Abrechnungsmodells auf Produktionswerte folgendermaßen vor:

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com) an.

2. Navigieren Sie zur Registerkarte **Virtual Machines**, und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **SKUS**.
4. Klicken Sie auf die Schaltfläche **Bearbeiten**, um das Abrechnungsmodell zurückzusetzen. Aktivieren bzw. deaktivieren Sie im daraufhin geöffneten Fenster das Kontrollkästchen **Abrechnung und Lizenzierung erfolgt extern durch Azure (auch bekannt als Bring-Your-Own-License)**.

    ![Bearbeiten des Abrechnungsmodells](media/marketplace-publishing-vm-image-post-publishing/img09-04.png)
5. Führen Sie die Schritte aus, die in diesem Artikel unter „Zurücksetzen des Listenpreises auf Produktionswerte“ beschrieben sind.
6. Wechseln Sie zur Registerkarte **VERÖFFENTLICHEN**, und klicken Sie auf **FÜR STAGINGUMGEBUNG FREIGEBEN**. Eine ausführliche Anleitung zum Testen Ihres Angebots in der Stagingumgebung finden Sie unter [Testen Ihres VM-Angebots für Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
7. Nachdem Sie Ihr Angebot in der Stagingumgebung getestet haben, wechseln Sie im Veröffentlichungsportal zur Registerkarte **VERÖFFENTLICHEN**. Klicken Sie auf **GENEHMIGUNG FÜR FREIGABE IN PRODUKTIONSUMGEBUNG ANFORDERN**, um Ihr Angebot im Marketplace erneut zu veröffentlichen.

## <a name="revert-the-visibility-setting-of-a-listed-sku-to-the-production-value"></a>Zurücksetzen der Sichtbarkeitseinstellung einer aufgelisteten SKU auf den Produktionswert
Gehen Sie zum Zurücksetzen der Sichtbarkeitseinstellung einer aufgelisteten SKU auf den Produktionswert folgendermaßen vor:

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com) an.

2. Navigieren Sie zur Registerkarte **Virtual Machines**, und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **SKUS**.
4. Wählen Sie Ihre SKU aus, und setzen Sie die Sichtbarkeitseinstellung der SKU auf den Produktionswert zurück.

    ![Sichtbarkeit](media/marketplace-publishing-vm-image-post-publishing/img10-04.png)
5. Wenn Sie die gewünschten Änderungen vorgenommen haben, klicken Sie auf **GENEHMIGUNG FÜR FREIGABE IN PRODUKTIONSUMGEBUNG ANFORDERN**, um Ihr Angebot im Marketplace erneut zu veröffentlichen.

## <a name="see-also"></a>Weitere Informationen
* [Erste Schritte: Veröffentlichen eines Angebots im Azure Marketplace](marketplace-publishing-getting-started.md)
* [Grundlegendes zu den Auszahlungsberichten](marketplace-publishing-report-payout.md)
* [Anzeigen und Ändern Ihres „Reseller Incentive“ für Cloud-Lösungsanbieter im Azure Marketplace](marketplace-publishing-csp-incentive.md)
* [Beheben häufiger Probleme bei der Veröffentlichung im Marketplace](marketplace-publishing-support-common-issues.md)
* [Zugriff auf die Publisher-Unterstützung](marketplace-publishing-get-publisher-support.md)
* [Erstellen eines lokalen VM-Images](marketplace-publishing-vm-image-creation-on-premise.md)
* [Erstellen eines virtuellen Computers unter Windows im Azure-Vorschauportal](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

