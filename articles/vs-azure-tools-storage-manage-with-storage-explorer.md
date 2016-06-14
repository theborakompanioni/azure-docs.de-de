<properties
	pageTitle="Erste Schritte mit dem Speicher-Explorer (Vorschau) | Microsoft Azure"
	description="Verwalten von Azure-Speicherressourcen mit dem Speicher-Explorer (Vorschauversion)"
	services="visual-studio-online"
	documentationCenter="na"
	authors="TomArcher"
	manager="douge"
	editor="" />

 <tags
	ms.service="visual-studio-online"
	ms.devlang="multiple"
	ms.topic="get-started-article"
	ms.tgt_pltfrm="na"
	ms.workload="na"
	ms.date="06/05/2016"
	ms.author="tarcher" />

# Erste Schritte mit dem Speicher-Explorer (Vorschau)

## Übersicht 

Bei der Vorschauversion des Microsoft Azure-Speicher-Explorers handelt es sich um eine eigenständige App, über die Sie ganz einfach mit Azure Storage-Daten arbeiten können – unter Windows, OSX und Linux. In diesem Artikel werden die verschiedenen Möglichkeiten zum Verbinden und Verwalten Ihrer Azure-Speicherkonten beschrieben.

## Voraussetzungen

- [Herunterladen und Installieren des Speicher-Explorers (Vorschau)](http://go.microsoft.com/fwlink/?LinkId=708343)

## Herstellen der Verbindung mit einem Speicherkonto oder Dienst

Es gibt viele Möglichkeiten, wie Sie den Speicher-Explorer (Vorschau) mit Speicherkonten verbinden können. Beispiele hierfür sind das Herstellen einer Verbindung mit Speicherkonten, die Ihren Azure-Abonnements zugeordnet sind, das Verbinden mit freigegebenen Speicherkonten und Diensten aus anderen Azure-Abonnements und sogar das Verbindungen mit und Verwalten von lokalem Speicher mit dem Azure-Speicheremulator:

- [Herstellen einer Verbindung mit einem Azure-Abonnement](#connect-to-an-azure-subscription): Verwalten Sie Speicherressourcen, die zu Ihrem Azure-Abonnement gehören.
- [Herstellen einer Verbindung mit lokalem Speicher](#connect-to-local-storage): Verwalten Sie lokalen Speicher mit dem Azure-Speicheremulator. 
- [Anfügen an externen Speicher](#attach-or-detach-an-external-storage-account): Verwalten Sie Speicherressourcen eines anderen Azure-Abonnements, indem Sie den Kontonamen und Schlüssel des Speicherkontos verwenden.
- [Anfügen eines Kontos per SAS](#attach-account-using-sas): Verwalten Sie Speicherressourcen eines anderen Azure-Abonnements per Shared Access Signature (SAS).
- [Anfügen eines Diensts per SAS](#attach-service-using-sas): Verwalten Sie einen bestimmten Speicherdienst (Blobcontainer, Warteschlange oder Tabelle) eines anderen Azure-Abonnements per Shared Access Signature (SAS).

## Herstellen einer Verbindung mit einem Azure-Abonnement

> [AZURE.NOTE] Wenn Sie kein Azure-Konto haben, können Sie sich [für eine kostenlose Testversion registrieren](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) oder [Ihre Visual Studio-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

1. Starten Sie den Speicher-Explorer (Vorschau). 

1. Wenn Sie den Speicher-Explorer (Vorschau) zum ersten Mal ausführen oder den Speicher-Explorer (Vorschau) schon einmal ausgeführt haben, aber noch keine Verbindung mit einem Azure-Konto hergestellt haben, wird eine Infoleiste angezeigt. Hierüber können Sie eine Verbindung mit einem Azure-Konto herstellen.

	![][0]
	
1. Wählen Sie die Option **Mit Microsoft Azure verbinden**, und folgen Sie den Schritten in den Dialogfeldern zum Anmelden mit einem Microsoft-Konto, das mindestens einem aktiven Azure-Abonnement zugeordnet ist.

Nachdem Sie sich mit einem Microsoft-Konto erfolgreich angemeldet haben, wird der linke Bereich des Speicher-Explorers (Vorschau) mit allen Speicherkonten aufgefüllt, die allen Azure-Abonnements des Microsoft-Kontos zugeordnet sind.

### Filtern von Azure-Abonnements

Mit dem Speicher-Explorer (Vorschau) können Sie filtern, für welche Azure-Abonnements, die dem angemeldeten Microsoft-Konto (bzw. mehreren Konten) zugeordnet sind, die Speicherkonten im linken Bereich aufgeführt werden.

1. Wählen Sie das Symbol **Einstellungen** (Zahnrad).

	![][1]

1. 	Oben links im Bereich wird eine Dropdownliste mit allen Microsoft-Konten angezeigt, an denen Sie sich angemeldet haben.

	![][3]
	 
1.	Wählen Sie Pfeil nach unten neben der Dropdownliste, um alle angemeldeten Microsoft-Konten sowie einen Link zum Hinzufügen (Anmelden) weiterer Microsoft-Konten anzuzeigen.

	![][4]

1.	Wählen Sie in der Dropdownliste das gewünschte Microsoft-Konto aus.

1. 	Im linken Bereich werden alle Azure-Abonnements angezeigt, die dem ausgewählten Microsoft-Konto zugeordnet sind. Mit dem Kontrollkästchen links neben einem Azure-Abonnement können Sie jeweils angeben, ob der Speicher-Explorer (Vorschau) alle Speicherkonten auflisten soll, die dem Azure-Abonnement zugeordnet sind. Durch das Aktivieren bzw. Deaktivieren von **Alle Abonnements** können Sie alle bzw. keine Azure-Abonnements der Liste auswählen.

	![][2]

1.	Wählen Sie **Übernehmen**, wenn Sie die Auswahl der zu verwaltenden Azure-Abonnements abgeschlossen haben. Der linke Bereich wird aktualisiert, um alle Speicherkonten für jedes ausgewählte Azure-Abonnement für das aktuelle Microsoft-Konto aufzulisten.

### Hinzufügen zusätzlicher Microsoft-Konten

In den folgenden Schritten werden Sie durch das Herstellen einer Verbindung mit zusätzlichen Microsoft-Konten zum Anzeigen der Azure-Abonnements und Speicherkonten der einzelnen Konten geführt.

1.	Wählen Sie das Symbol **Einstellungen** (Zahnrad).

	![][1]

1. 	Oben links im Bereich wird eine Dropdownliste mit allen derzeit verbundenen Microsoft-Konten angezeigt.

	![][3]
	 
1.	Wählen Sie den Pfeil nach unten neben der Dropdownliste, um alle angemeldeten Microsoft-Konten sowie einen Link zum Hinzufügen (Anmelden) weiterer Microsoft-Konten anzuzeigen.

	![][4]

1.	Wählen Sie **Konto hinzufügen**, und folgen Sie den Schritten in den Dialogfeldern, um sich an einem Konto anzumelden, das mindestens einem aktiven Azure-Abonnement zugeordnet ist.

1.	Aktivieren Sie die Kontrollkästchen für die Azure-Abonnements, die Sie durchsuchen möchten.

	![][2]

1.	Wählen Sie **Übernehmen**.

### Wechseln zwischen Microsoft-Konten

Sie können zwar eine Verbindung mit mehreren Microsoft-Konten herstellen, aber im linken Bereich werden nur die Speicherkonten angezeigt, die den Abonnements eines Microsoft-Kontos (aktuelles Konto) zugeordnet sind. Wenn Sie eine Verbindung mit mehreren Microsoft-Konten herstellen, können Sie zwischen den Konten umschalten, indem Sie die folgenden Schritte ausführen:

1.	Wählen Sie das Symbol **Einstellungen** (Zahnrad).

	![][1]

1. 	Oben links im Bereich wird eine Dropdownliste mit allen derzeit verbundenen Microsoft-Konten angezeigt.

	![][3]
	 
1.	Wählen Sie den Pfeil nach unten neben der Dropdownliste, um alle angemeldeten Microsoft-Konten sowie einen Link zum Hinzufügen (Anmelden) weiterer Microsoft-Konten anzuzeigen.

	![][4]

1.	Wählen Sie das gewünschte Microsoft-Konto aus.

1.	Aktivieren Sie die Kontrollkästchen für die Azure-Abonnements, die Sie durchsuchen möchten.

	![][2]

1.	Wählen Sie **Übernehmen**.
  
## Herstellen einer Verbindung mit lokalem Speicher

Mit dem Speicher-Explorer (Vorschau) können Sie Schritte für den lokalen Speicher ausführen, indem Sie den Azure-Speicheremulator verwenden. So können Sie Code für Speicher schreiben und den Speicher testen, ohne dass unbedingt ein Speicherkonto unter Azure bereitgestellt werden muss (das Speicherkonto wird vom Azure-Speicheremulator emuliert).

>[AZURE.NOTE] Der Azure-Speicheremulator wird derzeit nur für Windows unterstützt.

1. Starten Sie den Speicher-Explorer (Vorschau). 

1. Erweitern Sie im linken Bereich den Knoten **(Entwicklung)**.

	![][21]

1. Wenn Sie den Azure-Speicheremulator noch nicht installiert haben, werden Sie über eine Infoleiste dazu aufgefordert. Wählen Sie nach dem Anzeigen der Infoleiste die Option **Aktuelle Version herunterladen**, und installieren Sie den Emulator.

	![][22]

1. Nachdem der Emulator installiert wurde, können Sie lokale Blobs, Warteschlangen und Tabellen erstellen und nutzen. Wählen Sie unten den entsprechenden Link, um zu erfahren, wie Sie die einzelnen Speicherkontotypen verwenden:

	- [Verwalten von Azure-Blobspeicherressourcen](./vs-azure-tools-storage-explorer-blobs.md)
	- Verwalten von Azure-Warteschlangen-Speicherressourcen – *In Kürze verfügbar*
	- Verwalten von Azure-Tabellenspeicherressourcen – *In Kürze verfügbar*

## Anfügen oder Trennen eines externen Speicherkontos

Mit dem Speicher-Explorer (Vorschau) können Sie das Anfügen an externe Speicherkonten durchführen, damit Speicherkonten auf einfache Weise für die gemeinsame Nutzung freigegeben werden können. In diesem Abschnitt wird das Anfügen an externe Speicherkonten und das spätere Trennen beschrieben.

### Beschaffen der Anmeldeinformationen für Speicherkonten

Um ein externes Speicherkonto freigeben zu können, muss der Besitzer des Kontos zuerst die Anmeldeinformationen (Kontoname und Schlüssel) für das Konto beschaffen und diese Informationen dann an die Person weitergeben, die das Anfügen an das (externe) Konto durchführen möchte. Sie können die Speicherkonto-Anmeldeinformationen abrufen, indem Sie im Azure-Portal die folgenden Schritte ausführen:

1.	Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1.	Wählen Sie **Durchsuchen**.
1.	Wählen Sie **Speicherkonten**.
1.	Wählen Sie im Blatt **Speicherkonten** das gewünschte Speicherkonto.
1.	Wählen Sie im Blatt **Einstellungen** für das ausgewählte Speicherkonto die Option **Zugriffsschlüssel**.

	![][5]
	
1.	Kopieren Sie im Blatt **Zugriffsschlüssel** die Werte **SPEICHERKONTONAME** und **SCHLÜSSEL 1**, die für das Anfügen an das Speicherkonto verwendet werden sollen.

	![][6]

### Anfügen an ein externes Speicherkonto

1.	Klicken Sie im Speicher-Explorer (Vorschau) mit der rechten Maustaste auf **Speicherkonten**, und wählen Sie im Kontextmenü die Option **Externen Speicher anfügen**.

	![][7]
	
1.	Im Abschnitt *Beschaffen der Anmeldeinformationen für Speicherkonten* wird beschrieben, wie Sie die Werte für „Speicherkontoname“ und „Schlüssel 1“ abrufen. Diese Werte werden in diesem Schritt verwendet. Geben Sie im Dialogfeld **Externen Speicher anfügen** den Speicherkontonamen im Feld **Kontoname** und den Wert von „Schlüssel 1“ im Feld **Kontoschlüssel** ein. Wählen Sie nach Abschluss des Vorgangs **OK**.

	![][8]

	Nach dem Anfügen wird das externe Speicherkonto mit der Zeichenfolge **(Extern)** angezeigt, die an den Speicherkontonamen angehängt ist.

	![][9]

### Trennen von einem externen Speicherkonto

1. 	Klicken Sie mit der rechten Maustaste auf das externe Speicherkonto, das Sie trennen möchten, und wählen Sie im Kontextmenü die Option **Trennen**.

	![][10]

1.	Wählen Sie **Ja**, wenn das Bestätigungsdialogfeld angezeigt wird, um das Trennen vom externen Speicherkonto zu bestätigen.

	![][12]

## Anfügen eines Kontos per SAS

Mit einer SAS (Shared Access Signature) kann der Administrator eines Azure-Abonnements den Zugriff auf ein Speicherkonto vorübergehend gewähren, ohne dafür Anmeldeinformationen für das Azure-Abonnement angeben zu müssen.

Beispiel: Benutzer A ist Administrator eines Azure-Abonnements, und Benutzer A möchte Benutzer B für einen begrenzten Zeitraum Zugriff auf ein Speicherkonto mit bestimmten Berechtigungen gewähren:

1. Benutzer A generiert eine SAS (mit der Verbindungszeichenfolge für das Speicherkonto) für den entsprechenden Zeitraum und mit den gewünschten Berechtigungen.
1. Benutzer A gibt die SAS für die Person frei, die Zugriff auf das Speicherkonto erhalten möchte. In unserem Beispiel ist dies also Benutzer B.  
1. Benutzer B verwendet den Speicher-Explorer (Vorschau), um das Anfügen an das Konto von Benutzer A mit der bereitgestellten SAS durchzuführen. 

### Beschaffen einer SAS für das freizugebende Konto

1.	Öffnen Sie den Speicher-Explorer (Vorschau).
1.	Klicken Sie im linken Bereich mit der rechten Maustaste auf das Speicherkonto, das Sie freigeben möchten, und wählen Sie im Kontextmenü die Option **Get Shared Access Signature** (SAS abrufen).

	![][13]

1. Geben Sie im Dialogfeld **Shared Access Signature** den Zeitrahmen und die gewünschten Berechtigungen für das Konto an, und wählen Sie **Erstellen**.

	![][14]
 
1. Ein zweites Dialogfeld **Shared Access Signature** mit der SAS wird angezeigt. Wählen Sie neben **Verbindungszeichenfolge** die Option **Kopieren**, um sie in die Zwischenablage zu kopieren. Wählen Sie **Schließen**, um das Dialogfeld zu schließen.

### Anfügen an das freigegebene Konto per SAS

1.	Öffnen Sie den Speicher-Explorer (Vorschau).
1.	Klicken Sie im linken Bereich mit der rechten Maustaste auf **Speicherkonten**, und wählen Sie im Kontextmenü die Option **Attach account using SAS** (Konto per SAS anfügen). ![][15]

1. Gehen Sie im Dialogfeld **Attach Account using SAS** (Konto per SAS anfügen) wie folgt vor:

	- **Kontoname:** Geben Sie den Namen ein, den Sie diesem Konto zuordnen möchten. **HINWEIS:** Der Kontoname muss nicht mit dem ursprünglichen Speicherkontonamen übereinstimmen, für den die SAS generiert wurde. 
 	- **Verbindungszeichenfolge:** Fügen Sie die zuvor kopierte Verbindungszeichenfolge ein.
 	- Wählen Sie nach Abschluss des Vorgangs **OK**.
	
	![][16]

Nach dem Anfügen wird das Speicherkonto mit der Zeichenfolge „(SAS)“ angezeigt, die an den von Ihnen angegebenen Kontonamen angehängt ist.

![][17]

## Anfügen eines Diensts per SAS

Im Abschnitt [Anfügen eines Kontos per SAS](#attach-account-using-sas) wird beschrieben, wie der Administrator eines Azure-Abonnements vorübergehenden Zugriff auf ein Speicherkonto gewähren kann, indem er eine SAS für das Speicherkonto generiert (und freigibt). Auf ähnliche Weise kann eine SAS für einen bestimmten Dienst (Blobcontainer, Warteschlange oder Tabelle) in einem Speicherkonto generiert werden.

### Generieren einer SAS für den Dienst, den Sie freigeben möchten

In diesem Kontext kann ein Dienst ein Blobcontainer, eine Warteschlange oder eine Tabelle sein. In den folgenden Abschnitten wird beschrieben, wie Sie die SAS für den angegebenen Dienst generieren:

- [Abrufen der SAS für einen Blobcontainer](./vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)
- Abrufen der SAS für eine Warteschlange – *In Kürze verfügbar*
- Abrufen der SAS für eine Tabelle – *In Kürze verfügbar*

### Anfügen an den freigegebenen Kontodienst per SAS

1.	Öffnen Sie den Speicher-Explorer (Vorschau).
1.	Klicken Sie im linken Bereich mit der rechten Maustaste auf **Speicherkonten**, und wählen Sie im Kontextmenü die Option **Attach service using SAS** (Dienst per SAS anfügen). ![][18]

1. Fügen Sie im Dialogfeld **Attach service using SAS** (Dienst per SAS anfügen) den zuvor kopierten SAS-URI ein, und wählen Sie **OK**.

	![][19]

Nach Abschluss des Anfügevorgangs wird der neu angefügte Dienst unter dem Knoten **(Dienst-SAS)** angezeigt.

![][20]

## Suchen nach Speicherkonten

Wenn Sie über eine lange Liste mit Speicherkonten verfügen, können Sie ein bestimmtes Speicherkonto schnell ermitteln, indem Sie das Suchfeld oben im linken Bereich verwenden.

Wenn Sie Text in das Suchfeld eingeben, werden im linken Bereich nur die Speicherkonten angezeigt, die mit dem Suchwert übereinstimmen, den Sie bis zu diesem Punkt eingegeben haben. Der folgende Screenshot zeigt ein Beispiel, bei dem nach allen Speicherkonten gesucht wird, die im Speicherkontonamen den Text „tarcher“ enthalten.

![][11]
	
Wählen Sie zum Löschen der Suche im Suchfeld die Schaltfläche **x**.

## Nächste Schritte
- [Verwalten von Azure-Blobspeicherressourcen mit dem Speicher-Explorer (Vorschau)](./vs-azure-tools-storage-explorer-blobs.md)

[0]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure.png
[1]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/settings-gear.png
[2]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/filter-subscriptions.png
[3]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/filter-accounts.png
[4]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/accounts-drop-down.png
[5]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys.png
[6]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys-copy.png
[7]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-external-storage.png
[8]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-external-storage-dlg.png
[9]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/external-storage-account.png
[10]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage.png
[11]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-account-search.png
[12]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage-confirmation.png
[13]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-context-menu.png
[14]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-dlg1.png
[15]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-account-using-sas-context-menu.png
[16]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-account-using-sas-dlg.png
[17]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-account-using-sas-finished.png
[18]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-service-using-sas-context-menu.png
[19]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-service-using-sas-dlg.png
[20]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-service-using-sas-finished.png
[21]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/local-storage-drop-down.png
[22]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/download-storage-emulator.png

<!---HONumber=AcomDC_0608_2016-->