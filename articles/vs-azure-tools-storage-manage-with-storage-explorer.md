---
title: Erste Schritte mit dem Speicher-Explorer (Vorschau) | Microsoft Docs
description: Verwalten von Azure-Speicherressourcen mit dem Speicher-Explorer (Vorschauversion)
services: storage
documentationcenter: na
author: TomArcher
manager: douge
editor: 
ms.assetid: 1ed0f096-494d-49c4-ab71-f4164ee19ec8
ms.service: storage
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/18/2016
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: a538ceef7d9aeaca0e6f54443f3a5dabd06c22a1
ms.openlocfilehash: 9914051f5f509a657e91aa66c1efb99ceb9f4817


---
# <a name="getting-started-with-storage-explorer-preview"></a>Erste Schritte mit dem Speicher-Explorer (Vorschau)
## <a name="overview"></a>Übersicht
Bei der Vorschauversion des Microsoft Azure-Speicher-Explorers handelt es sich um eine eigenständige App, über die Sie unter Windows, OS X und Linux komfortabel mit Azure Storage-Daten arbeiten können. In diesem Artikel werden die verschiedenen Möglichkeiten zum Verbinden und Verwalten Ihrer Azure-Speicherkonten beschrieben.

![Microsoft Azure-Speicher-Explorer (Vorschau)][15]

## <a name="prerequisites"></a>Voraussetzungen
* [Herunterladen und Installieren des Speicher-Explorers (Vorschau)](http://www.storageexplorer.com)

## <a name="connect-to-a-storage-account-or-service"></a>Herstellen der Verbindung mit einem Speicherkonto oder Dienst
Es gibt viele Möglichkeiten, wie Sie den Speicher-Explorer (Vorschau) mit Speicherkonten verbinden können. Beispiele hierfür wären das Herstellen einer Verbindung mit Speicherkonten, die Ihren Azure-Abonnements zugeordnet sind, das Verbinden mit freigegebenen Speicherkonten und Diensten aus anderen Azure-Abonnements und sogar das Herstellen von Verbindungen mit und Verwalten von lokalem Speicher mit dem Azure-Speicheremulator. Darüber hinaus können Sie mit Speicherkonten in Azure (global und national) arbeiten:

* [Herstellen einer Verbindung mit einem Azure-Abonnement](#connect-to-an-azure-subscription) : Verwalten Sie Speicherressourcen, die zu Ihrem Azure-Abonnement gehören.
* [Verwenden von lokalem Entwicklungsspeicher](#work-with-local-development-storage) : Verwalten Sie lokalen Speicher mit dem Azure-Speicheremulator.
* [Anfügen an externen Speicher:](#attach-or-detach-an-external-storage-account) Verwalten Sie Speicherressourcen eines anderen Azure-Abonnements oder in nationalen Azure-Clouds unter Verwendung von Name, Schlüssel und Endpunkten des Speicherkontos.
* [Anfügen eines Kontos per SAS](#attach-storage-account-using-sas) : Verwalten Sie Speicherressourcen eines anderen Azure-Abonnements per Shared Access Signature (SAS).
* [Anfügen eines Diensts per SAS](#attach-service-using-sas) : Verwalten Sie einen bestimmten Speicherdienst (Blobcontainer, Warteschlange oder Tabelle) eines anderen Azure-Abonnements per Shared Access Signature (SAS).

## <a name="connect-to-an-azure-subscription"></a>Herstellen einer Verbindung mit einem Azure-Abonnement
> [!NOTE]
> Wenn Sie kein Azure-Konto haben, können Sie sich [für eine kostenlose Testversion registrieren](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) oder Ihre [Visual Studio-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).
>
>

1. Wählen Sie im Speicher-Explorer (Vorschau) die Option **Azure-Kontoeinstellungen**.

    ![Azure-Kontoeinstellungen][0]
2. Im linken Bereich werden jetzt alle Microsoft-Konten angezeigt, an denen Sie angemeldet sind. Wählen Sie zum Herstellen einer Verbindung mit einem anderen Konto die Option **Konto hinzufügen**aus, und führen Sie die Schritte in den Dialogfeldern durch, um sich mit einem Microsoft-Konto anzumelden, das mindestens einem aktiven Azure-Abonnement zugeordnet ist.
> [!NOTE]
>Eine anmeldungsbasierte Verbindungsherstellung mit nationalem Azure wie etwa Black Forest Azure, Fairfax Azure und Mooncake Azure wird momentan nicht unterstützt. Informationen zum Herstellen einer Verbindung mit nationalen Azure-Speicherkonten finden Sie im Abschnitt **Anfügen oder Trennen eines externen Speicherkontos**.

3. Nach erfolgreicher Anmeldung mit einem Microsoft-Konto werden im linken Bereich die dem Konto zugeordneten Azure-Abonnements angezeigt. Wählen Sie die Azure-Abonnements aus, die Sie verwenden möchten, und klicken Sie dann auf **Übernehmen**. (Durch Auswählen von **Alle Abonnements** können Sie alle bzw. keine Azure-Abonnements in der Liste auswählen.)

    ![Azure-Abonnements auswählen][3]
4. Im linken Bereich werden jetzt die Speicherkonten angezeigt, die den ausgewählten Azure-Abonnements zugeordnet sind.

    ![Ausgewählte Azure-Abonnements][4]

## <a name="work-with-local-development-storage"></a>Verwenden von lokalem Entwicklungsspeicher
Mit dem Speicher-Explorer (Vorschau) können Sie Schritte für den lokalen Speicher ausführen, indem Sie den Azure-Speicheremulator verwenden. So können Sie Code für Speicher schreiben und den Speicher testen, ohne dass unbedingt ein Speicherkonto unter Azure bereitgestellt werden muss (das Speicherkonto wird vom Azure-Speicheremulator emuliert).

> [!NOTE]
> Der Azure-Speicheremulator wird derzeit nur für Windows unterstützt.
>
>

1. Erweitern Sie im linken Bereich des Speicher-Explorers (Vorschau) den Knoten **Lokal und angefügt** > **Speicherkonten** > **(Entwicklung)**.

    ![Lokaler Entwicklungsknoten][21]
2. Wenn Sie den Azure-Speicheremulator noch nicht installiert haben, werden Sie über eine Infoleiste dazu aufgefordert. Wählen Sie nach dem Anzeigen der Infoleiste die Option **Aktuelle Version herunterladen**, und installieren Sie den Emulator.

    ![Eingabeaufforderung zum Herunterladen des Azure-Speicheremulators][22]
3. Nachdem der Emulator installiert wurde, können Sie lokale Blobs, Warteschlangen und Tabellen erstellen und nutzen. Wählen Sie unten den entsprechenden Link, um zu erfahren, wie Sie die einzelnen Speicherkontotypen verwenden:

   * [Verwalten von Azure-Blobspeicherressourcen](vs-azure-tools-storage-explorer-blobs.md)
   * Verwalten von Azure-Dateifreigabespeicher-Ressourcen – *In Kürze verfügbar*
   * Verwalten von Azure Queue Storage-Ressourcen – *In Kürze verfügbar*
   * Verwalten von Azure Table Storage-Ressourcen – *In Kürze verfügbar*

## <a name="attach-or-detach-an-external-storage-account"></a>Anfügen oder Trennen eines externen Speicherkontos
Mit dem Speicher-Explorer (Vorschau) können Sie das Anfügen an externe Speicherkonten durchführen, damit Speicherkonten auf einfache Weise für die gemeinsame Nutzung freigegeben werden können. In diesem Abschnitt wird das Anfügen an externe Speicherkonten und das spätere Trennen beschrieben.

### <a name="get-the-storage-account-credentials"></a>Beschaffen der Anmeldeinformationen für Speicherkonten
Um ein externes Speicherkonto freigeben zu können, muss der Besitzer des Kontos zuerst die Anmeldeinformationen (Kontoname und Schlüssel) für das Konto beschaffen und diese Informationen dann an die Person weitergeben, die das Anfügen an das (externe) Konto durchführen möchte. Sie können die Speicherkonto-Anmeldeinformationen abrufen, indem Sie im Azure-Portal die folgenden Schritte ausführen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Wählen Sie **Durchsuchen**.
3. Wählen Sie **Speicherkonten**.
4. Wählen Sie auf dem Blatt **Speicherkonten** das gewünschte Speicherkonto aus.
5. Wählen Sie auf dem Blatt **Einstellungen** für das ausgewählte Speicherkonto die Option **Zugriffsschlüssel**.

   ![Option „Zugriffsschlüssel“][5]
6. Kopieren Sie auf dem Blatt **Zugriffsschlüssel** die Werte **SPEICHERKONTONAME** und **SCHLÜSSEL 1**, die für das Anfügen an das Speicherkonto verwendet werden sollen.

   ![Zugriffsschlüssel][6]

### <a name="attach-to-an-external-storage-account"></a>Anfügen an ein externes Speicherkonto
Zum Anfügen an ein externes Speicherkonto benötigen Sie den Namen und Schlüssel des Kontos. Im Abschnitt *Beschaffen der Anmeldeinformationen für Speicherkonten* wird beschrieben, wie Sie diese Werte über das Azure-Portal abrufen. Beachten Sie aber, dass der Kontoschlüssel im Portal den Namen „key 1“ hat. Wenn der Speicher-Explorer (Vorschau) nach einem Kontoschlüssel fragt, geben Sie den Wert „key 1“ ein bzw. fügen ihn ein.

1. Wählen Sie im Speicher-Explorer (Vorschau) die Option **Connect to Azure storage**(Mit Azure Storage verbinden).

   ![Option „Mit Azure Storage verbinden“][23]
2. Geben Sie im Dialogfeld **Mit Azure Storage verbinden** den Kontoschlüssel (Wert „key 1“ aus dem Azure-Portal) an, und wählen Sie dann **Weiter**.
> [!NOTE]
> Sie können die Storage-Verbindungszeichenfolge aus einem Speicherkonto in nationalem Azure eingeben. Geben Sie beispielsweise Verbindungszeichenfolgen wie die folgende ein, um eine Verbindung mit Azure Black Forest-Speicherkonten herzustellen: DefaultEndpointsProtocol=https;AccountName=cawatest03;AccountKey=<Speicherkontoschlüssel>;EndpointSuffix=core.cloudapi.de. Die Verbindungszeichenfolge können Sie über das Azure-Portal ermitteln. Verwenden Sie hierzu die unter **Beschaffen der Anmeldeinformationen für Speicherkonten** beschriebene Vorgehensweise.

   ![Dialogfeld „Mit Azure Storage verbinden“][24]

3. Geben Sie im Dialogfeld **Externen Speicher anfügen** den Speicherkontonamen im Feld **Kontoname** ein, geben Sie weitere gewünschte Einstellungen an, und wählen Sie abschließend **Weiter**.

   ![Dialogfeld „Externen Speicher anfügen“][8]
4. Überprüfen Sie im Dialogfeld **Verbindungszusammenfassung** die angezeigten Informationen. Wenn Sie Änderungen vornehmen möchten, können Sie **Zurück** wählen und die gewünschten Einstellungen erneut eingeben. Wählen Sie **Verbinden**, wenn Sie fertig sind.
5. Nach dem Verbinden wird das externe Speicherkonto mit der Zeichenfolge **(Extern)** angezeigt, die an den Speicherkontonamen angehängt ist.

   ![Ergebnis der Verbindung mit einem externen Speicherkonto][9]

### <a name="detach-from-an-external-storage-account"></a>Trennen von einem externen Speicherkonto
1. Klicken Sie mit der rechten Maustaste auf das externe Speicherkonto, das Sie trennen möchten, und wählen Sie im Kontextmenü die Option **Trennen**.

   ![Option zum Trennen vom Speicher][10]
2. Wählen Sie **Ja** , wenn das Bestätigungsdialogfeld angezeigt wird, um das Trennen vom externen Speicherkonto zu bestätigen.

## <a name="attach-storage-account-using-sas"></a>Anfügen eines Kontos per SAS
Mit einer [SAS (Shared Access Signature)](storage/storage-dotnet-shared-access-signature-part-1.md) kann der Administrator eines Azure-Abonnements den Zugriff auf ein Speicherkonto vorübergehend gewähren, ohne dafür Anmeldeinformationen für das Azure-Abonnement angeben zu müssen.

Beispiel: Benutzer A ist Administrator eines Azure-Abonnements, und Benutzer A möchte Benutzer B für einen begrenzten Zeitraum Zugriff auf ein Speicherkonto mit bestimmten Berechtigungen gewähren:

1. Benutzer A generiert eine SAS (mit der Verbindungszeichenfolge für das Speicherkonto) für den entsprechenden Zeitraum und mit den gewünschten Berechtigungen.
2. Benutzer A gibt die SAS für die Person frei, die Zugriff auf das Speicherkonto erhalten möchte. In unserem Beispiel ist dies also Benutzer B.  
3. Benutzer B verwendet den Speicher-Explorer (Vorschau), um das Anfügen an das Konto von Benutzer A mit der bereitgestellten SAS durchzuführen.

### <a name="get-a-sas-for-the-account-you-want-to-share"></a>Beschaffen einer SAS für das freizugebende Konto
1. Klicken Sie im Speicher-Explorer (Vorschau) mit der rechten Maustaste auf das Speicherkonto, das Sie freigeben möchten, und wählen Sie im Kontextmenü die Option **Get Shared Access Signature**(SAS abrufen).

   ![Kontextmenüoption „SAS abrufen“][13]
2. Geben Sie im Dialogfeld **Shared Access Signature** den Zeitrahmen und die gewünschten Berechtigungen für das Konto an, und wählen Sie **Erstellen**.

    ![Dialogfeld „SAS abrufen“][14]
3. Ein zweites Dialogfeld **Shared Access Signature** mit der SAS wird angezeigt. Wählen Sie neben **Verbindungszeichenfolge** die Option **Kopieren**, um sie in die Zwischenablage zu kopieren. Wählen Sie **Schließen** , um das Dialogfeld zu schließen.

### <a name="attach-to-the-shared-account-using-the-sas"></a>Anfügen an das freigegebene Konto per SAS
1. Wählen Sie im Speicher-Explorer (Vorschau) die Option **Connect to Azure storage**(Mit Azure Storage verbinden).

   ![Option „Mit Azure Storage verbinden“][23]
2. Geben Sie im Dialogfeld **Mit Azure Storage verbinden** die Verbindungszeichenfolge an, und wählen Sie **Weiter**.

   ![Dialogfeld „Mit Azure Storage verbinden“][24]
3. Überprüfen Sie im Dialogfeld **Verbindungszusammenfassung** die angezeigten Informationen. Wenn Sie Änderungen vornehmen möchten, können Sie **Zurück** wählen und die gewünschten Einstellungen erneut eingeben. Wählen Sie **Verbinden**, wenn Sie fertig sind.
4. Nach dem Anfügen wird das Speicherkonto mit der Zeichenfolge „(SAS)“ angezeigt, die an den von Ihnen angegebenen Kontonamen angehängt ist.

   ![Ergebnis des Anfügens an ein Konto per SAS][17]

## <a name="attach-service-using-sas"></a>Anfügen eines Diensts per SAS
Im Abschnitt [Anfügen eines Speicherkontos per SAS](#attach-storage-account-using-sas) wird beschrieben, wie der Administrator eines Azure-Abonnements vorübergehenden Zugriff auf ein Speicherkonto gewähren kann, indem er eine SAS für das Speicherkonto generiert (und teilt). Auf ähnliche Weise kann eine SAS für einen bestimmten Dienst (Blobcontainer, Warteschlange oder Tabelle) in einem Speicherkonto generiert werden.  

### <a name="generate-a-sas-for-the-service-you-want-to-share"></a>Generieren einer SAS für den Dienst, den Sie freigeben möchten
In diesem Kontext kann ein Dienst ein Blobcontainer, eine Warteschlange oder eine Tabelle sein. In den folgenden Abschnitten wird beschrieben, wie Sie die SAS für den angegebenen Dienst generieren:

* [Abrufen der SAS für einen Blobcontainer](vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)
* Abrufen der SAS für eine Dateifreigabe – *In Kürze verfügbar*
* Abrufen der SAS für eine Warteschlange – *In Kürze verfügbar*
* Abrufen der SAS für eine Tabelle – *In Kürze verfügbar*

### <a name="attach-to-the-shared-account-service-using-the-sas"></a>Anfügen an den freigegebenen Kontodienst per SAS
1. Wählen Sie im Speicher-Explorer (Vorschau) die Option **Connect to Azure storage**(Mit Azure Storage verbinden).

   ![Option „Mit Azure Storage verbinden“][23]
2. Geben Sie im Dialogfeld **Mit Azure Storage verbinden** den SAS-URI an, und wählen Sie **Weiter**.

   ![Dialogfeld „Mit Azure Storage verbinden“][24]
3. Überprüfen Sie im Dialogfeld **Verbindungszusammenfassung** die angezeigten Informationen. Wenn Sie Änderungen vornehmen möchten, können Sie **Zurück** wählen und die gewünschten Einstellungen erneut eingeben. Wählen Sie **Verbinden**, wenn Sie fertig sind.
4. Nach Abschluss des Anfügevorgangs wird der neu angefügte Dienst unter dem Knoten **(Dienst-SAS)** angezeigt.

   ![Ergebnis des Anfügens an einen gemeinsamen Dienst per SAS][20]

## <a name="search-for-storage-accounts"></a>Suchen nach Speicherkonten
Wenn Sie über eine lange Liste mit Speicherkonten verfügen, können Sie ein bestimmtes Speicherkonto schnell ermitteln, indem Sie das Suchfeld oben im linken Bereich verwenden.

Wenn Sie Text in das Suchfeld eingeben, werden im linken Bereich nur die Speicherkonten angezeigt, die mit dem Suchwert übereinstimmen, den Sie bis zu diesem Punkt eingegeben haben. Der folgende Screenshot zeigt ein Beispiel, bei dem nach allen Speicherkonten gesucht wird, die im Speicherkontonamen den Text „tarcher“ enthalten.

![Speicherkontosuche][11]

Wählen Sie zum Löschen der Suche im Suchfeld die Schaltfläche **x** .

## <a name="next-steps"></a>Nächste Schritte
* [Verwalten von Azure-Blobspeicherressourcen mit dem Speicher-Explorer (Vorschau)](vs-azure-tools-storage-explorer-blobs.md)

[0]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/settings-icon.png
[1]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-account-link.png
[3]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/subscriptions-list.png
[4]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-accounts-list.png
[5]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys.png
[6]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys-copy.png
[8]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-external-storage-dlg.png
[9]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/external-storage-account.png
[10]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage.png
[11]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-account-search.png
[12]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage-confirmation.png
[13]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-context-menu.png
[14]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-dlg1.png
[15]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/mase.png
[17]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-account-using-sas-finished.png
[20]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-service-using-sas-finished.png
[21]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/local-storage-drop-down.png
[22]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/download-storage-emulator.png
[23]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-icon.png
[24]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-next.png



<!--HONumber=Dec16_HO3-->


