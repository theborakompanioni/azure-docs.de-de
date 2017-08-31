---
title: Erste Schritte mit dem Speicher-Explorer (Vorschau) | Microsoft-Dokumentation
description: Verwalten von Azure-Speicherressourcen mit dem Speicher-Explorer (Vorschauversion)
services: storage
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 1ed0f096-494d-49c4-ab71-f4164ee19ec8
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/17/2017
ms.author: kraigb
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 0abc33bb5b0ffe46397baa5a2d53637bc3f2984c
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---
# <a name="get-started-with-storage-explorer-preview"></a>Erste Schritte mit dem Speicher-Explorer (Vorschau)
## <a name="overview"></a>Übersicht
Bei der Vorschauversion des Azure-Speicher-Explorers handelt es sich um eine eigenständige App, über die Sie unter Windows, macOS und Linux komfortabel mit Azure Storage-Daten arbeiten können. In diesem Artikel werden die verschiedenen Möglichkeiten zum Verbinden und Verwalten Ihrer Azure-Speicherkonten beschrieben.

![Microsoft Azure-Speicher-Explorer (Vorschau)][15]

## <a name="prerequisites"></a>Voraussetzungen
* [Herunterladen und Installieren des Speicher-Explorers (Vorschau)](http://www.storageexplorer.com)

## <a name="connect-to-a-storage-account-or-service"></a>Herstellen der Verbindung mit einem Speicherkonto oder Dienst
Es gibt verschiedene Möglichkeiten, wie Sie den Speicher-Explorer (Vorschau) mit Speicherkonten verbinden können. Dazu zählen z. B.:
* Verbinden mit Speicherkonten, die Ihren Azure-Abonnements zugeordnet sind
* Verbinden mit freigegebenen Speicherkonten und Diensten aus anderen Azure-Abonnements
* Verbinden mit und Verwalten von lokalem Speicher mithilfe des Azure-Speicheremulators 

Darüber hinaus können Sie mit Speicherkonten in Azure (global und national) arbeiten:

* [Herstellen einer Verbindung mit einem Azure-Abonnement:](#connect-to-an-azure-subscription) Verwalten Sie Speicherressourcen, die zu Ihrem Azure-Abonnement gehören.
* [Verwenden von lokalem Entwicklungsspeicher:](#work-with-local-development-storage) Verwalten Sie lokalen Speicher mit dem Azure-Speicheremulator.
* [Anfügen an externen Speicher:](#attach-or-detach-an-external-storage-account) Verwalten Sie Speicherressourcen eines anderen Azure-Abonnements oder in nationalen Azure-Clouds unter Verwendung von Name, Schlüssel und Endpunkten des Speicherkontos.
* [Anfügen eines Speicherkontos unter Verwendung einer SAS:](#attach-storage-account-using-sas) Verwalten Sie Speicherressourcen eines anderen Azure-Abonnements unter Verwendung einer Shared Access Signature (SAS).
* [Anfügen eines Diensts unter Verwendung einer SAS:](#attach-service-using-sas) Verwalten Sie einen bestimmten Speicherdienst (Blobcontainer, Warteschlange oder Tabelle) eines anderen Azure-Abonnements unter Verwendung einer SAS.

## <a name="connect-to-an-azure-subscription"></a>Herstellen einer Verbindung mit einem Azure-Abonnement
> [!NOTE]
> Wenn Sie kein Azure-Konto haben, können Sie sich [für eine kostenlose Testversion registrieren](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) oder Ihre [Visual Studio-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).
>
>

1. Wählen Sie im Speicher-Explorer (Vorschau) die Option **Azure-Kontoeinstellungen**.

    ![Azure-Kontoeinstellungen][0]

2. Im linken Bereich werden alle Microsoft-Konten angezeigt, bei denen Sie angemeldet sind. Wählen Sie zum Herstellen einer Verbindung mit einem anderen Konto die Option **Konto hinzufügen** aus, und führen Sie die Anweisungen aus, um sich mit einem Microsoft-Konto anzumelden, das mindestens einem aktiven Azure-Abonnement zugeordnet ist.

    >[!NOTE]
    >Eine anmeldungsbasierte Verbindungsherstellung mit einer nationalen Azure-Instanz (etwa mit Azure Deutschland, Azure Government oder Azure China) wird momentan nicht unterstützt. Informationen zum Herstellen einer Verbindung mit nationalen Azure-Speicherkonten finden Sie im Abschnitt „Anfügen oder Trennen eines externen Speicherkontos“.

3. Nach erfolgreicher Anmeldung mit einem Microsoft-Konto werden im linken Bereich die dem Konto zugeordneten Azure-Abonnements angezeigt. Wählen Sie die Azure-Abonnements aus, die Sie verwenden möchten, und klicken Sie dann auf **Übernehmen**. (Durch Auswählen von **Alle Abonnements** können Sie alle bzw. keine Azure-Abonnements in der Liste auswählen.)

    ![Azure-Abonnements auswählen][3]  
    Im linken Bereich werden die Speicherkonten angezeigt, die mit den ausgewählten Azure-Abonnements verknüpft sind.

    ![Ausgewählte Azure-Abonnements][4]

## <a name="connect-to-an-azure-stack-subscription"></a>Herstellen einer Verbindung mit einem Azure Stack-Abonnement

Informationen zum Herstellen der Verbindung mit einem Azure Stack-Abonnement finden Sie unter [Connect Storage Explorer to an Azure Stack subscription](azure-stack/azure-stack-storage-connect-se.md) (Verbinden des Speicher-Explorers mit einem Azure Stack-Abonnement).

## <a name="work-with-local-development-storage"></a>Verwenden von lokalem Entwicklungsspeicher
Mit dem Speicher-Explorer (Vorschau) können Sie unter Verwendung des Azure-Speicheremulators Schritte für den lokalen Speicher ausführen. Dadurch können Sie Code für Speicher schreiben und Speicher testen, ohne dass unbedingt ein Speicherkonto in Azure bereitgestellt werden muss, da das Speicherkonto vom Azure-Speicheremulator emuliert wird.

> [!NOTE]
> Der Azure-Speicheremulator wird derzeit nur für Windows unterstützt.
>
>

1. Erweitern Sie im linken Bereich des Speicher-Explorers (Vorschau) den Knoten **(Lokal und angefügt)** > **Speicherkonten** > **(Entwicklung)**.

    ![Lokaler Entwicklungsknoten][21]

2. Wenn Sie den Azure-Speicheremulator noch nicht installiert haben, werden Sie über eine Infoleiste dazu aufgefordert. Falls die Infoleiste angezeigt wird, wählen Sie **Aktuelle Version herunterladen** aus, und installieren Sie den Emulator.

    ![Eingabeaufforderung zum Herunterladen des Azure-Speicheremulators][22]

3. Nach der Installation des Emulators können Sie lokale Blobs, Warteschlangen und Tabellen erstellen und nutzen. Weitere Informationen zur Verwendung der einzelnen Speicherkontotypen finden Sie in den folgenden Themen:

    * [Verwalten von Azure-Blobspeicherressourcen](vs-azure-tools-storage-explorer-blobs.md)
    * Verwalten von Azure-Dateifreigabespeicher-Ressourcen: *In Kürze verfügbar*
    * Verwalten von Azure Queue Storage-Ressourcen: *In Kürze verfügbar*
    * Verwalten von Azure Table Storage-Ressourcen: *In Kürze verfügbar*

## <a name="attach-or-detach-an-external-storage-account"></a>Anfügen oder Trennen eines externen Speicherkontos
Mit dem Speicher-Explorer (Vorschau) können Sie das Anfügen an externe Speicherkonten durchführen, damit Speicherkonten auf einfache Weise für die gemeinsame Nutzung freigegeben werden können. In diesem Abschnitt wird das Anfügen an externe Speicherkonten und das spätere Trennen beschrieben.

### <a name="get-the-storage-account-credentials"></a>Beschaffen der Anmeldeinformationen für Speicherkonten
Um ein externes Speicherkonto freigeben zu können, muss der Besitzer des Kontos zuerst die Anmeldeinformationen (Kontoname und Schlüssel) für das Konto ermitteln und diese Informationen dann an die Person weitergeben, die das Anfügen an das (externe) Konto durchführen möchte. Gehen Sie im Azure-Portal wie folgt vor, um die Anmeldeinformationen für das Speicherkonto abzurufen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

2. Wählen Sie **Durchsuchen**.

3. Wählen Sie **Speicherkonten**.

4. Wählen Sie auf dem Blatt **Speicherkonten** das gewünschte Speicherkonto aus.

5. Wählen Sie auf dem Blatt **Einstellungen** für das ausgewählte Speicherkonto die Option **Zugriffsschlüssel** aus.

    ![Option „Zugriffsschlüssel“][5]

6. Kopieren Sie auf dem Blatt **Zugriffsschlüssel** die Werte **Speicherkontoname** und **Schlüssel1** für das Anfügen an das Speicherkonto.

    ![Zugriffsschlüssel][6]

### <a name="attach-to-an-external-storage-account"></a>Anfügen an ein externes Speicherkonto
Zum Anfügen an ein externes Speicherkonto benötigen Sie den Namen und Schlüssel des Kontos. Im Abschnitt „Beschaffen der Anmeldeinformationen für Speicherkonten“ erfahren Sie, wie Sie diese Werte über das Azure-Portal abrufen. Im Portal heißt der Kontoschlüssel allerdings **Schlüssel1**. Wenn im Speicher-Explorer (Vorschau) also ein Kontoschlüssel verlangt wird, geben Sie den Wert von **Schlüssel1** ein.

1. Wählen Sie im Speicher-Explorer (Vorschau) die Option **Connect to Azure storage**(Mit Azure Storage verbinden).

    ![Option „Mit Azure Storage verbinden“][23]

2. Geben Sie im Dialogfeld **Verbindung mit Azure-Speicher herstellen** den Kontoschlüssel (Wert von **Schlüssel1** aus dem Azure-Portal) an, und wählen Sie anschließend **Weiter** aus.

    > [!NOTE]
    > Sie können die Storage-Verbindungszeichenfolge aus einem Speicherkonto in nationalem Azure eingeben. Wenn Sie also etwa eine Verbindung mit Speicherkonten in Azure Deutschland herstellen möchten, können Sie Verbindungszeichenfolgen wie die folgenden eingeben: 
    >
    >* DefaultEndpointsProtocol=https
    >* AccountName=cawatest03
    >* AccountKey=<Speicherkontoschlüssel>
    >* EndpointSuffix=core.cloudapi.de
    
    >Die Verbindungszeichenfolge können Sie wie im Abschnitt „Beschaffen der Anmeldeinformationen für Speicherkonten“ beschrieben aus dem Azure-Portal abrufen.

    ![Dialogfeld „Verbindung mit Azure-Speicher herstellen“][24]

3. Geben Sie im Dialogfeld **Externen Speicher anfügen** im Feld **Kontoname** den Speicherkontonamen ein, geben Sie weitere gewünschte Einstellungen an, und wählen Sie anschließend **Weiter** aus.

    ![Dialogfeld „Externen Speicher anfügen“][8]

4. Überprüfen Sie die Angaben im Dialogfeld **Verbindungszusammenfassung**. Wenn Sie Änderungen vornehmen möchten, wählen Sie **Zurück** aus und geben die gewünschten Einstellungen erneut ein. 

5. Wählen Sie **Verbinden**aus.

6. Nach erfolgreicher Verbindungsherstellung wird das externe Speicherkonto angezeigt. (Dabei wird **(Extern)** an den Speicherkontonamen angehängt.)

    ![Ergebnis der Verbindung mit einem externen Speicherkonto][9]

### <a name="detach-from-an-external-storage-account"></a>Trennen von einem externen Speicherkonto
1. Klicken Sie mit der rechten Maustaste auf das externe Speicherkonto, das Sie trennen möchten, und wählen Sie anschließend **Trennen** aus.

    ![Option zum Trennen vom Speicher][10]

2. Wählen Sie in der Bestätigungsmeldung die Option **Ja** aus, um das Trennen vom externen Speicherkonto zu bestätigen.

## <a name="attach-a-storage-account-by-using-an-sas"></a>Anfügen eines Speicherkontos unter Verwendung einer SAS
Mithilfe einer [SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md) kann ein Administrator eines Azure-Abonnements einem Speicherkonto ohne Angabe von Anmeldeinformationen für das Azure-Abonnement temporären Zugriff auf ein Speicherkonto gewähren.

Ein Beispiel: Benutzer A ist Administrator eines Azure-Abonnements, und Benutzer A möchte Benutzer B für einen begrenzten Zeitraum und mit bestimmten Berechtigungen Zugriff auf ein Speicherkonto gewähren:

1. Benutzer A generiert eine SAS (mit der Verbindungszeichenfolge für das Speicherkonto) für den entsprechenden Zeitraum und mit den gewünschten Berechtigungen.

2. Benutzer A gibt die SAS an die Person weiter, die auf das Speicherkonto zugreifen möchte (in unserem Beispiel also an Benutzer B).  

3. Benutzer B verwendet den Speicher-Explorer (Vorschau), um das Anfügen an das Konto von Benutzer A mit der bereitgestellten SAS durchzuführen.

### <a name="get-an-sas-for-the-account-you-want-to-share"></a>Beschaffen einer SAS für das freizugebende Konto
1. Klicken Sie im Speicher-Explorer (Vorschau) mit der rechten Maustaste auf das Speicherkonto, das Sie freigeben möchten, und wählen Sie **Get Shared Access Signature**(SAS abrufen) aus.

    ![Kontextmenüoption „SAS abrufen“][13]

2. Geben Sie im Dialogfeld **Shared Access Signature** den Zeitrahmen und die gewünschten Berechtigungen für das Konto an, und wählen Sie anschließend **Erstellen** aus.

    ![Dialogfeld zum Abrufen der SAS][14]  
    Das Dialogfeld **Shared Access Signature** wird geöffnet und zeigt die SAS an.

3. Wählen Sie neben **Verbindungszeichenfolge** die Option **Kopieren** aus, um die Verbindungszeichenfolge in die Zwischenablage zu kopieren, und wählen Sie anschließend **Schließen** aus.

### <a name="attach-to-the-shared-account-by-using-the-sas"></a>Anfügen an das freigegebene Konto unter Verwendung der SAS
1. Wählen Sie im Speicher-Explorer (Vorschau) die Option **Connect to Azure storage**(Mit Azure Storage verbinden).

    ![Option „Mit Azure Storage verbinden“][23]

2. Geben Sie im Dialogfeld **Verbindung mit Azure-Speicher herstellen** die Verbindungszeichenfolge an, und wählen Sie anschließend **Weiter** aus.

    ![Dialogfeld „Verbindung mit Azure-Speicher herstellen“][24]

3. Überprüfen Sie die Angaben im Dialogfeld **Verbindungszusammenfassung**. Wenn Sie etwas ändern möchten, wählen Sie **Zurück** aus, und geben Sie die gewünschten Einstellungen ein. 

4. Wählen Sie **Verbinden**aus.

5. Nach dem Anfügen wird das Speicherkonto angezeigt. (Dabei wird **(SAS)** an den von Ihnen angegebenen Kontonamen angehängt.)

    ![Ergebnis des Anfügens an ein Konto unter Verwendung der SAS][17]

## <a name="attach-a-service-by-using-an-sas"></a>Anfügen eines Diensts unter Verwendung einer SAS
Im Abschnitt „Anfügen eines Speicherkontos unter Verwendung einer SAS“ wird beschrieben, wie ein Administrator eines Azure-Abonnements vorübergehenden Zugriff auf ein Speicherkonto gewähren kann, indem er eine SAS für das Speicherkonto generiert und weitergibt. Analog dazu kann eine SAS auch für einen bestimmten Dienst (Blobcontainer, Warteschlange oder Tabelle) in einem Speicherkonto generiert werden.  

### <a name="generate-an-sas-for-the-service-that-you-want-to-share"></a>Generieren einer SAS für den Dienst, den Sie freigeben möchten
In diesem Kontext kann ein Dienst ein Blobcontainer, eine Warteschlange oder eine Tabelle sein. Informationen zum Generieren der SAS für einen aufgeführten Dienst finden Sie in den folgenden Themen:

* [Abrufen der SAS für einen Blobcontainer](vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)
* Abrufen der SAS für eine Dateifreigabe: *In Kürze verfügbar*
* Abrufen der SAS für eine Warteschlange: *In Kürze verfügbar*
* Abrufen der SAS für eine Tabelle: *In Kürze verfügbar*

### <a name="attach-to-the-shared-account-service-by-using-the-sas"></a>Anfügen an den freigegebenen Kontodienst unter Verwendung der SAS
1. Wählen Sie im Speicher-Explorer (Vorschau) die Option **Connect to Azure storage**(Mit Azure Storage verbinden).

    ![Option „Mit Azure Storage verbinden“][23]

2. Geben Sie im Dialogfeld **Verbindung mit Azure-Speicher herstellen** den SAS-URI an, und wählen Sie anschließend **Weiter** aus.

    ![Dialogfeld „Verbindung mit Azure-Speicher herstellen“][24]

3. Überprüfen Sie die Angaben im Dialogfeld **Verbindungszusammenfassung**. Wenn Sie etwas ändern möchten, wählen Sie **Zurück** aus, und geben Sie die gewünschten Einstellungen ein. 

4. Wählen Sie **Verbinden**aus.

5. Nach Abschluss des Anfügevorgangs wird der neu angefügte Dienst unter dem Knoten **(Dienst-SAS)** angezeigt.

    ![Ergebnis des Anfügens an einen freigegebenen Dienst unter Verwendung einer SAS][20]

## <a name="search-for-storage-accounts"></a>Suchen nach Speicherkonten
Wenn Sie über eine lange Liste mit Speicherkonten verfügen, können Sie ein bestimmtes Speicherkonto schnell ermitteln, indem Sie das Suchfeld oben im linken Bereich verwenden.

Wenn Sie Text in das Suchfeld eingeben, werden im linken Bereich die dem bislang eingegebenen Suchwert entsprechenden Speicherkonten angezeigt. Der folgende Screenshot zeigt beispielsweise eine Suche nach allen Speicherkonten, deren Name **tarcher** enthält:

![Speicherkontosuche][11]

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
[25]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-certificate-azure-stack.png
[26]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/export-root-cert-azure-stack.png
[27]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/import-azure-stack-cert-storage-explorer.png
[28]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/select-target-azure-stack.png
[29]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-azure-stack-account.png
[30]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/select-accounts-azure-stack.png
[31]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/azure-stack-storage-account-list.png

