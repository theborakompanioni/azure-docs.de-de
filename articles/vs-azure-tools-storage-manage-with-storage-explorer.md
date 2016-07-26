<properties
	pageTitle="Erste Schritte mit dem Speicher-Explorer (Vorschau) | Microsoft Azure"
	description="Verwalten von Azure-Speicherressourcen mit dem Speicher-Explorer (Vorschauversion)"
	services="visual-studio-online"
	documentationCenter="na"
	authors="TomArcher"
	manager="douge"
	editor="" />

 <tags
	ms.service="storage"
	ms.devlang="multiple"
	ms.topic="get-started-article"
	ms.tgt_pltfrm="na"
	ms.workload="na"
	ms.date="07/14/2016"
	ms.author="tarcher" />

# Erste Schritte mit dem Speicher-Explorer (Vorschau)

## Übersicht 

Bei der Vorschauversion des Microsoft Azure-Speicher-Explorers handelt es sich um eine eigenständige App, über die Sie ganz einfach mit Azure Storage-Daten arbeiten können – unter Windows, OSX und Linux. In diesem Artikel werden die verschiedenen Möglichkeiten zum Verbinden und Verwalten Ihrer Azure-Speicherkonten beschrieben.

## Voraussetzungen

- [Herunterladen und Installieren des Speicher-Explorers (Vorschau)](http://www.storageexplorer.com)

## Herstellen der Verbindung mit einem Speicherkonto oder Dienst

Es gibt viele Möglichkeiten, wie Sie den Speicher-Explorer (Vorschau) mit Speicherkonten verbinden können. Beispiele hierfür sind das Herstellen einer Verbindung mit Speicherkonten, die Ihren Azure-Abonnements zugeordnet sind, das Verbinden mit freigegebenen Speicherkonten und Diensten aus anderen Azure-Abonnements und sogar das Verbindungen mit und Verwalten von lokalem Speicher mit dem Azure-Speicheremulator:

- [Herstellen einer Verbindung mit einem Azure-Abonnement](#connect-to-an-azure-subscription): Verwalten Sie Speicherressourcen, die zu Ihrem Azure-Abonnement gehören.
- [Verwenden von lokalem Entwicklungsspeicher](#work-with-local-development-storage): Verwalten Sie lokalen Speicher mit dem Azure-Speicheremulator.
- [Anfügen an externen Speicher](#attach-or-detach-an-external-storage-account): Verwalten Sie Speicherressourcen eines anderen Azure-Abonnements, indem Sie den Kontonamen und Schlüssel des Speicherkontos verwenden.
- [Anfügen eines Kontos per SAS](#attach-storage-account-using-sas): Verwalten Sie Speicherressourcen eines anderen Azure-Abonnements per Shared Access Signature (SAS).
- [Anfügen eines Diensts per SAS](#attach-service-using-sas): Verwalten Sie einen bestimmten Speicherdienst (Blobcontainer, Warteschlange oder Tabelle) eines anderen Azure-Abonnements per Shared Access Signature (SAS).

## Herstellen einer Verbindung mit einem Azure-Abonnement

> [AZURE.NOTE] Wenn Sie kein Azure-Konto haben, können Sie sich [für eine kostenlose Testversion registrieren](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) oder [Ihre Visual Studio-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

1. Wählen Sie im Speicher-Explorer (Vorschau) die Option **Azure-Kontoeinstellungen**.

	![][0]

1. Im linken Bereich werden jetzt alle Microsoft-Konten angezeigt, an denen Sie angemeldet sind. Wählen Sie zum Herstellen einer Verbindung mit einem anderen Konto die Option **Konto hinzufügen**, und folgen Sie den Schritten in den Dialogfeldern zum Anmelden mit einem Microsoft-Konto, das mindestens einem aktiven Azure-Abonnement zugeordnet ist.

	![][1]

1. Nachdem Sie sich erfolgreich an einem Microsoft-Konto angemeldet haben, werden im linken Bereich die Azure-Abonnements angezeigt, die dem Konto zugeordnet sind. Wählen Sie die Azure-Abonnements aus, die Sie verwenden möchten, und klicken Sie dann auf **Übernehmen**. (Durch die Auswahl von **Alle Abonnements** können Sie alle bzw. keine Azure-Abonnements der Liste auswählen.)

	![][3]

1. Im linken Bereich werden jetzt die Speicherkonten angezeigt, die den ausgewählten Azure-Abonnements zugeordnet sind.

	![][4]

## Verwenden von lokalem Entwicklungsspeicher

Mit dem Speicher-Explorer (Vorschau) können Sie Schritte für den lokalen Speicher ausführen, indem Sie den Azure-Speicheremulator verwenden. So können Sie Code für Speicher schreiben und den Speicher testen, ohne dass unbedingt ein Speicherkonto unter Azure bereitgestellt werden muss (das Speicherkonto wird vom Azure-Speicheremulator emuliert).

>[AZURE.NOTE] Der Azure-Speicheremulator wird derzeit nur für Windows unterstützt.

1. Erweitern Sie im linken Bereich des Speicher-Explorers (Vorschau) den Knoten **Local and Attached** (Lokal und angefügt) > **Speicherkonten** > **(Development)** ((Entwicklung)).

	![][21]

1. Wenn Sie den Azure-Speicheremulator noch nicht installiert haben, werden Sie über eine Infoleiste dazu aufgefordert. Wählen Sie nach dem Anzeigen der Infoleiste die Option **Aktuelle Version herunterladen**, und installieren Sie den Emulator.

	![][22]

1. Nachdem der Emulator installiert wurde, können Sie lokale Blobs, Warteschlangen und Tabellen erstellen und nutzen. Wählen Sie unten den entsprechenden Link, um zu erfahren, wie Sie die einzelnen Speicherkontotypen verwenden:

	- [Verwalten von Azure-Blobspeicherressourcen](./vs-azure-tools-storage-explorer-blobs.md)
	- Verwalten von Azure-Dateifreigabespeicher-Ressourcen – *In Kürze verfügbar*
	- Verwalten von Azure-Warteschlangen-Speicherressourcen – *In Kürze verfügbar*
	- Verwalten von Azure-Tabellenspeicherressourcen – *In Kürze verfügbar*

## Anfügen oder Trennen eines externen Speicherkontos

Mit dem Speicher-Explorer (Vorschau) können Sie das Anfügen an externe Speicherkonten durchführen, damit Speicherkonten auf einfache Weise für die gemeinsame Nutzung freigegeben werden können. In diesem Abschnitt wird das Anfügen an externe Speicherkonten und das spätere Trennen beschrieben.

### Beschaffen der Anmeldeinformationen für Speicherkonten

Um ein externes Speicherkonto freigeben zu können, muss der Besitzer des Kontos zuerst die Anmeldeinformationen (Kontoname und Schlüssel) für das Konto beschaffen und diese Informationen dann an die Person weitergeben, die das Anfügen an das (externe) Konto durchführen möchte. Sie können die Speicherkonto-Anmeldeinformationen abrufen, indem Sie im Azure-Portal die folgenden Schritte ausführen:

1.	Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1.	Wählen Sie **Durchsuchen**.
1.	Wählen Sie **Speicherkonten**.
1.	Wählen Sie auf dem Blatt **Speicherkonten** das gewünschte Speicherkonto.
1.	Wählen Sie auf dem Blatt **Einstellungen** für das ausgewählte Speicherkonto die Option **Zugriffsschlüssel**.

	![][5]
	
1.	Kopieren Sie auf dem Blatt **Zugriffsschlüssel** die Werte **SPEICHERKONTONAME** und **SCHLÜSSEL 1**, die für das Anfügen an das Speicherkonto verwendet werden sollen.

	![][6]

### Anfügen an ein externes Speicherkonto
Zum Anfügen an ein externes Speicherkonto benötigen Sie den Namen und Schlüssel des Kontos. Im Abschnitt *Beschaffen der Anmeldeinformationen für Speicherkonten* wird beschrieben, wie Sie diese Werte über das Azure-Portal abrufen. Beachten Sie aber, dass der Kontoschlüssel im Portal den Namen „key 1“ hat. Wenn der Speicher-Explorer (Vorschau) nach einem Kontoschlüssel fragt, geben Sie den Wert „key 1“ ein bzw. fügen ihn ein.
 
1.	Wählen Sie im Speicher-Explorer (Vorschau) die Option **Connect to Azure storage** (Mit Azure Storage verbinden).

	![][23]

1.	Geben Sie im Dialogfeld **Connect to Azure Storage** (Mit Azure Storage verbinden) den Kontoschlüssel (Wert „key 1“ aus dem Azure-Portal) an, und wählen Sie dann **Weiter**.

	![][24]

1.	Geben Sie im Dialogfeld **Externen Speicher anfügen** den Speicherkontonamen im Feld **Kontoname** ein, geben Sie die anderen gewünschten Einstellungen an, und wählen Sie abschließend **Weiter**.

	![][8]

1.	Überprüfen Sie im Dialogfeld **Verbindungszusammenfassung** die angezeigten Informationen. Wenn Sie Änderungen vornehmen möchten, können Sie **Zurück** wählen und die gewünschten Einstellungen erneut eingeben. Wählen Sie **Verbinden**, wenn Sie fertig sind.

1.	Nach dem Verbinden wird das externe Speicherkonto mit der Zeichenfolge **(Extern)** angezeigt, die an den Speicherkontonamen angehängt ist.

	![][9]

### Trennen von einem externen Speicherkonto

1. 	Klicken Sie mit der rechten Maustaste auf das externe Speicherkonto, das Sie trennen möchten, und wählen Sie im Kontextmenü die Option **Trennen**.

	![][10]

1.	Wählen Sie **Ja**, wenn das Bestätigungsdialogfeld angezeigt wird, um das Trennen vom externen Speicherkonto zu bestätigen.

## Anfügen eines Speicherkontos per SAS

Mit einer [SAS (Shared Access Signature)](storage/storage-dotnet-shared-access-signature-part-1.md) kann der Administrator eines Azure-Abonnements den Zugriff auf ein Speicherkonto vorübergehend gewähren, ohne dafür Anmeldeinformationen für das Azure-Abonnement angeben zu müssen.

Beispiel: Benutzer A ist Administrator eines Azure-Abonnements, und Benutzer A möchte Benutzer B für einen begrenzten Zeitraum Zugriff auf ein Speicherkonto mit bestimmten Berechtigungen gewähren:

1. Benutzer A generiert eine SAS (mit der Verbindungszeichenfolge für das Speicherkonto) für den entsprechenden Zeitraum und mit den gewünschten Berechtigungen.
1. Benutzer A gibt die SAS für die Person frei, die Zugriff auf das Speicherkonto erhalten möchte. In unserem Beispiel ist dies also Benutzer B.
1. Benutzer B verwendet den Speicher-Explorer (Vorschau), um das Anfügen an das Konto von Benutzer A mit der bereitgestellten SAS durchzuführen.

### Beschaffen einer SAS für das freizugebende Konto

1.	Klicken Sie im Speicher-Explorer (Vorschau) mit der rechten Maustaste auf das Speicherkonto, das Sie freigeben möchten, und wählen Sie im Kontextmenü die Option **Get Shared Access Signature** (SAS abrufen).

	![][13]

1. Geben Sie im Dialogfeld **Shared Access Signature** den Zeitrahmen und die gewünschten Berechtigungen für das Konto an, und wählen Sie **Erstellen**.

	![][14]
 
1. Ein zweites Dialogfeld **Shared Access Signature** mit der SAS wird angezeigt. Wählen Sie neben **Verbindungszeichenfolge** die Option **Kopieren**, um sie in die Zwischenablage zu kopieren. Wählen Sie **Schließen**, um das Dialogfeld zu schließen.

### Anfügen an das freigegebene Konto per SAS

1.	Wählen Sie im Speicher-Explorer (Vorschau) die Option **Connect to Azure storage** (Mit Azure Storage verbinden).

	![][23]

1.	Geben Sie im Dialogfeld **Connect to Azure Storage** (Mit Azure Storage verbinden) die Verbindungszeichenfolge an, und wählen Sie **Weiter**.

	![][24]

1.	Überprüfen Sie im Dialogfeld **Verbindungszusammenfassung** die angezeigten Informationen. Wenn Sie Änderungen vornehmen möchten, können Sie **Zurück** wählen und die gewünschten Einstellungen erneut eingeben. Wählen Sie **Verbinden**, wenn Sie fertig sind.

1.	Nach dem Anfügen wird das Speicherkonto mit der Zeichenfolge „(SAS)“ angezeigt, die an den von Ihnen angegebenen Kontonamen angehängt ist.

	![][17]

## Anfügen eines Diensts per SAS

Im Abschnitt [Anfügen eines Speicherkontos per SAS](#attach-storage-account-using-sas) wird beschrieben, wie der Administrator eines Azure-Abonnements vorübergehenden Zugriff auf ein Speicherkonto gewähren kann, indem er eine SAS für das Speicherkonto generiert (und freigibt). Auf ähnliche Weise kann eine SAS für einen bestimmten Dienst (Blobcontainer, Warteschlange oder Tabelle) in einem Speicherkonto generiert werden.

### Generieren einer SAS für den Dienst, den Sie freigeben möchten

In diesem Kontext kann ein Dienst ein Blobcontainer, eine Warteschlange oder eine Tabelle sein. In den folgenden Abschnitten wird beschrieben, wie Sie die SAS für den angegebenen Dienst generieren:

- [Abrufen der SAS für einen Blobcontainer](./vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)
- Abrufen der SAS für eine Dateifreigabe – *In Kürze verfügbar*
- Abrufen der SAS für eine Warteschlange – *In Kürze verfügbar*
- Abrufen der SAS für eine Tabelle – *In Kürze verfügbar*

### Anfügen an den freigegebenen Kontodienst per SAS

1.	Wählen Sie im Speicher-Explorer (Vorschau) die Option **Connect to Azure storage** (Mit Azure Storage verbinden).

	![][23]

1.	Geben Sie im Dialogfeld **Connect to Azure Storage** (Mit Azure Storage verbinden) den SAS-URI an, und wählen Sie **Weiter**.

	![][24]

1.	Überprüfen Sie im Dialogfeld **Verbindungszusammenfassung** die angezeigten Informationen. Wenn Sie Änderungen vornehmen möchten, können Sie **Zurück** wählen und die gewünschten Einstellungen erneut eingeben. Wählen Sie **Verbinden**, wenn Sie fertig sind.

1.	Nach Abschluss des Anfügevorgangs wird der neu angefügte Dienst unter dem Knoten **(Dienst-SAS)** angezeigt.

	![][20]

## Suchen nach Speicherkonten

Wenn Sie über eine lange Liste mit Speicherkonten verfügen, können Sie ein bestimmtes Speicherkonto schnell ermitteln, indem Sie das Suchfeld oben im linken Bereich verwenden.

Wenn Sie Text in das Suchfeld eingeben, werden im linken Bereich nur die Speicherkonten angezeigt, die mit dem Suchwert übereinstimmen, den Sie bis zu diesem Punkt eingegeben haben. Der folgende Screenshot zeigt ein Beispiel, bei dem nach allen Speicherkonten gesucht wird, die im Speicherkontonamen den Text „tarcher“ enthalten.

![][11]
	
Wählen Sie zum Löschen der Suche im Suchfeld die Schaltfläche **x**.

## Nächste Schritte
- [Verwalten von Azure-Blobspeicherressourcen mit dem Speicher-Explorer (Vorschau)](./vs-azure-tools-storage-explorer-blobs.md)

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
[17]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-account-using-sas-finished.png
[20]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-service-using-sas-finished.png
[21]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/local-storage-drop-down.png
[22]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/download-storage-emulator.png
[23]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-icon.png
[24]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-next.png

<!---HONumber=AcomDC_0720_2016-->