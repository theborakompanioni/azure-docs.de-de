<properties
	pageTitle="Verwalten von Azure-Blobspeicherressourcen mit dem Speicher-Explorer (Vorschau) | Microsoft Azure"
	description="Verwalten von Azure-Blobcontainern und Blobs mit dem Speicher-Explorer (Vorschau)"
	services="storage"
	documentationCenter="na"
	authors="TomArcher"
	manager="douge"
	editor="" />

 <tags
	ms.service="storage"
	ms.devlang="multiple"
	ms.topic="article"
	ms.tgt_pltfrm="na"
	ms.workload="na"
	ms.date="08/17/2016"
	ms.author="tarcher" />

# Verwalten von Azure-Blobspeicherressourcen mit dem Speicher-Explorer (Vorschau)

## Übersicht

Der Azure-Blobspeicher ([Azure Blob Storage](./storage/storage-dotnet-how-to-use-blobs.md)) ist ein Dienst zur Speicherung großer Mengen unstrukturierter Daten, z.B. Text- oder Binärdaten, auf die von überall auf der Welt über HTTP oder HTTPS zugegriffen werden kann. Sie können den Blobspeicher verwenden, um Daten öffentlich auf der ganzen Welt zur Verfügung zu stellen oder um Anwendungsdaten privat zu speichern. In diesem Artikel wird beschrieben, wie Sie den Speicher-Explorer (Vorschau) für Blobcontainer und Blobs verwenden.

## Voraussetzungen

Damit Sie die in diesem Artikel aufgeführten Schritte ausführen können, benötigen Sie Folgendes:

- [Herunterladen und Installieren des Speicher-Explorers (Vorschau)](http://www.storageexplorer.com)
- [Herstellen der Verbindung mit einem Azure-Speicherkonto oder -Dienst](./vs-azure-tools-storage-manage-with-storage-explorer.md#connect-to-a-storage-account-or-service)

## Erstellen eines Blobcontainers

Alle Blobs müssen sich in einem Blobcontainer befinden. Ein Blobcontainer ist einfach eine logische Gruppierung von Blobs. Ein Konto kann eine unbegrenzte Anzahl von Containern enthalten, und in jedem Container kann eine unbegrenzte Anzahl von Blobs gespeichert werden.

Die folgenden Schritte veranschaulichen, wie Sie einen Blobcontainer im Speicher-Explorer (Vorschau) erstellen.

1.	Öffnen Sie den Speicher-Explorer (Vorschau).
1.	Erweitern Sie im linken Bereich das Speicherkonto, in dem Sie den Blobcontainer erstellen möchten.
1.	Klicken Sie mit der rechten Maustaste auf **BLOB-Container**, und wählen Sie im Kontextmenü die Option **BLOB-Container erstellen**.

	![Kontextmenü „BLOB-Container erstellen“][0]

1.	Unter dem Ordner **BLOB-Container** wird ein Textfeld angezeigt. Geben Sie den Namen für den Blobcontainer ein. Eine Liste mit den Regeln und Einschränkungen für die Benennung von Blobcontainern finden Sie unter [Benennungsregeln für Container](./storage/storage-dotnet-how-to-use-blobs.md#create-a-container).

	![Textfeld „BLOB-Container erstellen“][1]

1.	Drücken Sie die **EINGABETASTE**, wenn Sie mit dem Erstellen des Blobcontainers fertig sind, oder drücken Sie **ESC**, um den Vorgang abzubrechen. Nach der erfolgreichen Erstellung des Blobcontainers wird er im Ordner **BLOB-Container** für das ausgewählte Speicherkonto angezeigt.

	![Blobcontainer wurde erstellt][2]

## Anzeigen des Inhalts eines Blobcontainers

Blobcontainer enthalten Blobs und Ordner (die ebenfalls Blobs enthalten können).

Die folgenden Schritte veranschaulichen, wie Sie den Inhalt eines Blobcontainers im Speicher-Explorer (Vorschau) erstellen:

1.	Öffnen Sie den Speicher-Explorer (Vorschau).
1.	Erweitern Sie im linken Bereich das Speicherkonto, das den gewünschten Blobcontainer enthält.
1.	Erweitern Sie die Option **BLOB-Container** des Speicherkontos.
1.	Klicken Sie mit der rechten Maustaste auf den Blobcontainer, den Sie anzeigen möchten, und wählen Sie im Kontextmenü die Option **BLOB-Container-Editor öffnen**. Sie können auch auf den Blobcontainer doppelklicken, den Sie anzeigen möchten.

	![Kontextmenü „BLOB-Container-Editor öffnen“][19]

1.	Im Hauptfenster wird der Inhalt des Blobcontainers angezeigt.

	![BLOB-Container-Editor][3]

## Löschen eines Blob-Containers

Blobcontainer können nach Bedarf einfach erstellt und gelöscht werden. (Informationen zum Löschen einzelner Blobs finden Sie im Abschnitt [Verwalten von Blobs in einem Blobcontainer](./#managing-blobs-in-a-blob-container).)

Die folgenden Schritte veranschaulichen, wie Sie einen Blobcontainer im Speicher-Explorer (Vorschau) löschen:

1.	Öffnen Sie den Speicher-Explorer (Vorschau).
1.	Erweitern Sie im linken Bereich das Speicherkonto, das den gewünschten Blobcontainer enthält.
1.	Erweitern Sie die Option **BLOB-Container** des Speicherkontos.
1.	Klicken Sie mit der rechten Maustaste auf den Blobcontainer, den Sie löschen möchten, und wählen Sie im Kontextmenü die Option **Löschen**. Sie können auch die ENTF-TASTE drücken, um den derzeit ausgewählten Blobcontainer zu löschen.

	![Kontextmenü „Delete blob Container“ (Blobcontainer löschen)][4]

1.	Wählen Sie im Bestätigungsdialogfeld die Option **Ja**.

	![Bestätigung „Delete blob Container“ (Blobcontainer löschen)][5]

## Kopieren eines Blobcontainers

Mit dem Speicher-Explorer (Vorschau) können Sie einen Blobcontainer in die Zwischenablage kopieren und diesen Blobcontainer dann in ein anderes Speicherkonto einfügen. (Informationen zum Kopieren einzelner Blobs finden Sie im Abschnitt [Verwalten von Blobs in einem Blobcontainer](./#managing-blobs-in-a-blob-container).)

Die folgenden Schritte veranschaulichen, wie Sie einen Blobcontainer aus einem Speicherkonto in ein anderes kopieren.

1.	Öffnen Sie den Speicher-Explorer (Vorschau).
1.	Erweitern Sie im linken Bereich das Speicherkonto, das den gewünschten Blobcontainer enthält.
1.	Erweitern Sie die Option **BLOB-Container** des Speicherkontos.
1.	Klicken Sie mit der rechten Maustaste auf den Blobcontainer, den Sie kopieren möchten, und wählen Sie im Kontextmenü die Option **Copy Blob Container** (Blobcontainer kopieren).

	![Kontextmenü „Blobcontainer kopieren“][6]

1.	Klicken Sie mit der rechten Maustaste auf das gewünschte Zielspeicherkonto, in das Sie den Blobcontainer einfügen möchten, und wählen Sie im Kontextmenü die Option **Paste Blob Container** (Blobcontainer einfügen).

	![Kontextmenü „Blobcontainer einfügen“][7]

## Abrufen der SAS für einen Blobcontainer

Eine [Shared Access Signature (SAS)](./storage/storage-dotnet-shared-access-signature-part-1.md) ermöglicht den delegierten Zugriff auf Ressourcen in Ihrem Speicherkonto. Sie haben die Möglichkeit, einem Client für einen bestimmten Zeitraum spezielle eingeschränkte Berechtigungen für Objekte in Ihrem Speicherkonto zu erteilen, ohne Ihre Konto-Zugriffsschlüssel weitergeben zu müssen.

Die folgenden Schritte veranschaulichen, wie Sie eine SAS für einen Blobcontainer erstellen:

1.	Öffnen Sie den Speicher-Explorer (Vorschau).
1.	Erweitern Sie im linken Bereich das Speicherkonto mit dem Blobcontainer, für den Sie die SAS abrufen möchten.
1.	Erweitern Sie die Option **BLOB-Container** des Speicherkontos.
1.	Klicken Sie mit der rechten Maustaste auf den gewünschten Blobcontainer, und wählen Sie im Kontextmenü die Option **Get Shared Access Signature** (SAS abrufen).

	![Kontextmenü „SAS abrufen“][8]

1.	Geben Sie im Dialogfeld **Shared Access Signature** die Richtlinie, das Start- und Ablaufdatum, die Zeitzone und die Zugriffsebenen für die Ressource an.

	![SAS abrufen – Optionen][9]

1.	Wählen Sie **Erstellen**, wenn Sie das Angeben der SAS-Optionen abgeschlossen haben.

1.	In einem zweiten Dialogfeld **Shared Access Signature** wird dann der Blobcontainer zusammen mit der URL und den Abfragezeichenfolgen angezeigt, die Sie zum Zugreifen auf die Speicherressource verwenden können. Wählen Sie neben der URL, die Sie in die Zwischenablage kopieren möchten, die Option **Kopieren**.

	![SAS-URLs kopieren][10]

1.	Wählen Sie abschließend die Option **Schließen**.

## Verwalten von Zugriffsrichtlinien für einen Blobcontainer

Die folgenden Schritte veranschaulichen, wie Sie Zugriffsrichtlinien für einen Blobcontainer verwalten (hinzufügen und entfernen):

1.	Öffnen Sie den Speicher-Explorer (Vorschau).
1.	Erweitern Sie im linken Bereich das Speicherkonto mit dem Blobcontainer, dessen Zugriffsrichtlinien Sie verwalten möchten.
1.	Erweitern Sie die Option **BLOB-Container** des Speicherkontos.
1.	Wählen Sie den gewünschten Blobcontainer, und wählen Sie im Kontextmenü die Option **Manage Access Policies** (Zugriffsrichtlinien verwalten).

	![Kontextmenü „Manage Access Policies“ (Zugriffsrichtlinien verwalten)][11]

1.	Im Dialogfeld **Zugriffsrichtlinien** werden alle Zugriffsrichtlinien aufgeführt, die für den ausgewählten Blobcontainer bereits erstellt wurden.

	![Zugriffsrichtlinie – Optionen][12]

1.	Führen Sie diese Schritte je nach der Verwaltungsaufgabe der Zugriffsrichtlinie aus:

	- **Hinzufügen einer neuen Zugriffsrichtlinie**: Wählen Sie die Option **Hinzufügen**. Nach der Erstellung wird die neu hinzugefügte Zugriffsrichtlinie (mit Standardeinstellungen) im Dialogfeld **Zugriffsrichtlinien** angezeigt.
	- **Bearbeiten einer Zugriffsrichtlinie**: Nehmen Sie die gewünschten Bearbeitungen vor, und wählen Sie die Option **Speichern**.
	- **Entfernen einer Zugriffsrichtlinie**: Wählen Sie neben der Zugriffsrichtlinie, die Sie entfernen möchten, die Option **Entfernen**.

## Festlegen der öffentlichen Zugriffsebene für einen Blobcontainer

Standardmäßig wird für jeden Blobcontainer „Kein öffentlicher Zugriff“ festgelegt.

Die folgenden Schritte veranschaulichen, wie Sie eine öffentliche Zugriffsebene für einen Blobcontainer angeben.

1.	Öffnen Sie den Speicher-Explorer (Vorschau).
1.	Erweitern Sie im linken Bereich das Speicherkonto mit dem Blobcontainer, dessen Zugriffsrichtlinien Sie verwalten möchten.
1.	Erweitern Sie die Option **BLOB-Container** des Speicherkontos.
1.	Wählen Sie den gewünschten Blobcontainer, und wählen Sie im Kontextmenü die Option **Set Public Access Level** (Öffentliche Zugriffsebene festlegen).

	![Kontextmenü „Set Public Access Level“ (Öffentliche Zugriffsebene festlegen)][13]

1.	Geben Sie im Dialogfeld **Set Container Public Access Level** (Öffentliche Zugriffsebene für Container festlegen) die gewünschte Zugriffsebene an.

	![Set Public Access Level (Öffentliche Zugriffsebene festlegen) – Optionen][14]

1.	Wählen Sie **Übernehmen**.

## Verwalten von Blobs in einem Blobcontainer

Nach dem Erstellen eines Blobcontainers können Sie ein Blob in den Blobcontainer hochladen, ein Blob auf den lokalen Computer herunterladen, ein Blob auf dem lokalen Computer öffnen und vieles mehr.

Die folgenden Schritte veranschaulichen, wie Sie die Blobs (und Ordner) in einem Blobcontainer verwalten.

1.	Öffnen Sie den Speicher-Explorer (Vorschau).
1.	Erweitern Sie im linken Bereich das Speicherkonto, das den gewünschten Blobcontainer enthält.
1.	Erweitern Sie die Option **BLOB-Container** des Speicherkontos.
1.	Doppelklicken Sie auf den Blobcontainer, den Sie anzeigen möchten.
1.	Im Hauptfenster wird der Inhalt des Blobcontainers angezeigt.

	![BLOB-Container anzeigen][3]

1.	Im Hauptfenster wird der Inhalt des Blobcontainers angezeigt.

1.	Führen Sie die folgenden Schritte für die gewünschte Aufgabe aus:

	- **Hochladen von Dateien in einen Blobcontainer**

		1.	Wählen Sie in der Symbolleiste des Hauptbereichs die Option **Hochladen** und dann im Dropdownmenü die Option **Dateien hochladen**.

			![Menü „Dateien hochladen“][15]

		1.	Wählen Sie im Dialogfeld **Dateien hochladen** rechts neben dem Feld **Dateien** die Schaltfläche mit den Auslassungspunkten (**...**) aus, um die hochzuladenden Dateien auszuwählen.

			![Dateien hochladen – Optionen][16]

		1.	Geben Sie als Typ **BLOB-Typ** an. Im Artikel [Erste Schritte mit Azure Blob Storage mit .NET](./storage/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) werden die Unterschiede zwischen den unterschiedlichen Blobtypen erläutert.

		1.	Optional können Sie einen Zielordner angeben, in den die ausgewählten Dateien hochgeladen werden. Falls der Zielordner noch nicht vorhanden ist, wird er erstellt.

		1.	Wählen Sie die Option **Hochladen**.

	- **Hochladen eines Ordners in einen Blobcontainer**

		1.	Wählen Sie in der Symbolleiste des Hauptbereichs die Option **Hochladen** und dann im Dropdownmenü die Option **Upload Folder** (Ordner hochladen).

			![Menü „Upload folder“ (Ordner hochladen)][17]

		1.	Wählen Sie im Dialogfeld **Upload folder** (Ordner hochladen) rechts vom Textfeld **Ordner** die Schaltfläche mit den Auslassungszeichen (**…**), um den Ordner auszuwählen, dessen Inhalt Sie hochladen möchten.

			![Upload folder (Ordner hochladen) – Optionen][18]

		1.	Geben Sie als Typ **BLOB-Typ** an. Im Artikel [Erste Schritte mit Azure Blob Storage mit .NET](./storage/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) werden die Unterschiede zwischen den einzelnen Blobtypen erläutert.

		1.	Optional können Sie einen Zielordner angeben, in den der Inhalt des ausgewählten Ordners hochgeladen wird. Falls der Zielordner noch nicht vorhanden ist, wird er erstellt.

		1.	Wählen Sie die Option **Hochladen**.

	- **Herunterladen eines Blobs auf den lokalen Computer**

		1.	Wählen Sie das Blob aus, das Sie herunterladen möchten.

		1.	Wählen Sie in der Symbolleiste des Hauptbereichs die Option **Herunterladen**.

		1.	Geben Sie im Dialogfeld **Specify where to save the downloaded blob** (Downloadspeicherort für Blob angeben) den Speicherort, an den das Blob heruntergeladen werden soll, und den Namen dafür an.

		1.	Wählen Sie **Speichern** aus.

	- **Öffnen eines Blobs auf dem lokalen Computer**

		1.	Wählen Sie das Blob aus, das Sie öffnen möchten.

		1.	Wählen Sie in der Symbolleiste des Hauptbereichs die Option **Öffnen**.

		1.	Das Blob wird heruntergeladen und mit der Anwendung geöffnet, die dem zugrunde liegenden Dateityp des Blobs zugeordnet ist.

	- **Kopieren eines Blobs in die Zwischenablage**

		1.	Wählen Sie das Blob aus, das Sie kopieren möchten.

		1.	Wählen Sie in der Symbolleiste des Hauptbereichs die Option **Kopieren**.

		1.	Navigieren Sie im linken Bereich zu einem anderen Blobcontainer, und doppelklicken Sie darauf, um ihn im Hauptbereich anzuzeigen.

		1.	Wählen Sie in der Symbolleiste des Hauptbereichs die Option **Einfügen**, um eine Kopie des Blobs zu erstellen.

	- **Löschen eines Blobs**

		1.	Wählen Sie das Blob aus, das Sie löschen möchten.

		1.	Wählen Sie in der Symbolleiste des Hauptbereichs die Option **Löschen**.

		1.	Wählen Sie im Bestätigungsdialogfeld die Option **Ja**.

## Nächste Schritte

- Sehen Sie sich die [neuesten Versionsanmerkungen und Videos zum Speicher-Explorer (Vorschau)](http://www.storageexplorer.com) an.
- Informieren Sie sich, wie Sie [Anwendungen mit Azure-Blobs, -Tabellen, -Warteschlangen und -Dateien erstellen](https://azure.microsoft.com/documentation/services/storage/).

[0]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-create-context-menu.png
[1]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create.png
[2]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create-done.png
[3]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-editor.png
[4]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-context-menu.png
[5]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-confirmation.png
[6]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-copy-context-menu.png
[7]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-paste-context-menu.png
[8]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-context-menu.png
[9]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-options.png
[10]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-urls.png
[11]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-context-menu.png
[12]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-options.png
[13]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-context-menu.png
[14]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-options.png
[15]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-menu.png
[16]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-options.png
[17]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-menu.png
[18]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-options.png
[19]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-open-editor-context-menu.png

<!---HONumber=AcomDC_0817_2016-->