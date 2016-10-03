<properties
	pageTitle="Verwenden von Import/Export zum Übertragen von Daten in den Blob-Speicher | Microsoft Azure"
	description="Erfahren Sie, wie Sie Import- und Exportaufträge im klassischen Azure-Portal erstellen, um Daten in den Blob-Speicher zu übertragen."
	authors="renashahmsft"
	manager="aungoo"
	editor="tysonn"
	services="storage"
	documentationCenter=""/>

<tags
	ms.service="storage"
	ms.workload="storage"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/21/2016"
	ms.author="renash;robinsh"/>


# Verwenden des Microsoft Azure Import/Export-Diensts zum Übertragen von Daten in den Blobspeicher

## Übersicht

Mit dem Import/Export-Dienst von Azure können Sie große Datenmengen auf sichere Weise in den Azure-Blobspeicher übertragen, indem Sie Festplattenlaufwerke an ein Azure-Rechenzentrum schicken. Sie können diesen Dienst auch zum Übertragen von Daten aus dem Azure-Blobspeicher auf Festplattenlaufwerke und zum Schicken an Ihren lokalen Standort nutzen. Dieser Dienst eignet sich für Situationen, in denen Sie mehrere TB an Daten an oder von Azure übertragen möchten und in denen das Hoch- oder Herunterladen über das Netzwerk aufgrund der eingeschränkten Bandbreite oder hoher Netzwerkkosten nicht möglich ist.

Für den Dienst ist es erforderlich, dass Festplattenlaufwerke über eine BitLocker-Verschlüsselung verfügen, damit für die Sicherheit Ihrer Daten gesorgt ist. Der Dienst unterstützt die klassischen Speicherkonten, die in allen Regionen des öffentlichen Azure vorhanden sind. Schicken Sie die Festplattenlaufwerke an einen der unterstützten Standorte, die weiter unten in diesem Artikel angegeben sind.

In diesem Artikel erfahren Sie mehr zum Import/Export-Dienst von Azure und zum Schicken von Laufwerken, um Ihre Daten in und aus Azure-Blobspeichern zu kopieren.

> [AZURE.IMPORTANT] Sie können Import- und Exportaufträge für den klassischen Speicher mit dem klassischen Portal oder den REST-APIs des Import/Export-Diensts ([Speicher-Import/Export Service REST-API-Referenz](http://go.microsoft.com/fwlink/?LinkID=329099)) erstellen und verwalten. Resource Manager-Speicherkonten werden derzeit nicht unterstützt.

## Wann sollte der Import/Export-Dienst von Azure verwendet werden?

Sie können den Import/Export-Dienst von Azure nutzen, wenn das Hoch- bzw. Herunterladen von Daten über das Netzwerk zu langsam oder das Hinzufügen weiterer Bandbreite aus Kostengründen nicht möglich ist.

Sie können diesen Dienst in folgenden Szenarien verwenden:

- Migrieren von Daten zur Cloud: Verschieben Sie große Datenmengen schnell und kostengünstig nach Azure.
- Inhaltsverteilung: Schicken Sie Daten schnell an Ihre Kundenstandorte.
- Sicherung: Erstellen Sie Sicherungen Ihrer lokalen Daten für die Speicherung im Azure-Blobspeicher.
- Datenwiederherstellung: Stellen Sie große Datenmengen wieder her, die im Blobspeicher gespeichert sind, und lassen Sie sie an Ihren lokalen Standort schicken.

## Voraussetzungen

In diesem Abschnitt sind die Voraussetzungen aufgeführt, die für diesen Dienst erforderlich sind. Lesen Sie sich diese Informationen sorgfältig durch, bevor Sie Ihre Laufwerke verschicken.

### Speicherkonto

Sie müssen über ein Azure-Abonnement und ein oder mehrere **klassische** Speicherkonten verfügen, um den Import/Export-Dienst nutzen zu können. Bei jedem Auftrag können lediglich Daten auf ein oder von einem klassischen Speicherkonto übertragen werden. Anders ausgedrückt: Ein einzelner Import/Export-Auftrag kann nicht mehrere Speicherkonten umfassen. Weitere Informationen zum Erstellen eines neuen Speicherkontos finden Sie unter [Erstellen eines Speicherkontos](storage-create-storage-account.md#create-a-storage-account).

### Blobtypen

Sie können den Import/Export-Dienst von Azure verwenden, um Daten in **Block**blobs oder **Seiten**blobs zu kopieren. Umgekehrt können Sie mit diesem Dienst nur **Block**blobs, **Seiten**blobs oder **Anfüge**blobs aus dem Azure-Speicher exportieren.

### Auftrag

Zunächst erstellen Sie einen Auftrag, um mit dem Importieren oder Exportieren von Blob-Speicher zu beginnen. Ein Auftrag kann ein Importauftrag oder ein Exportauftrag sein:

- Erstellen Sie einen Importauftrag, wenn Sie lokal gespeicherte Daten auf Blobs in Ihrem Azure-Speicherkonto übertragen möchten.
- Erstellen Sie einen Exportauftrag, wenn Sie Daten, die derzeit als Blobs in Ihrem Speicherkonto gespeichert sind, auf Festplatten übertragen möchten, die dann an Sie geschickt werden.

Wenn Sie einen Auftrag erstellen, benachrichtigen Sie den Import-/Export-Dienst, dass Sie eine oder mehrere Festplatten an ein Azure-Datacenter schicken werden.

- Bei einem Importauftrag verschicken Sie Festplatten, die Ihre Daten enthalten.
- Bei einem Exportauftrag verschicken Sie leere Festplatten.
- Sie können bis zu 10 Festplatten pro Auftrag schicken.

Sie können einen Import- oder Exportauftrag erstellen, indem Sie das [klassische Portal](https://manage.windowsazure.com/) oder die [Azure Storage Import/Export-REST-API](http://go.microsoft.com/fwlink/?LinkID=329099) verwenden.

### Clienttool

Der erste Schritt beim Erstellen eines **Import**auftrags ist das Vorbereiten des Laufwerks, das für Importzwecke verschickt werden soll. Zum Vorbereiten des Laufwerks müssen Sie es mit einem lokalen Server verbinden und darauf das Azure Import/Export-Clienttool ausführen. Mit diesem Clienttool können Sie Ihre Daten auf das Laufwerk kopieren, die Daten auf dem Laufwerk mit BitLocker verschlüsseln und die Laufwerkjournaldateien generieren.

In den Journaldateien werden grundlegende Informationen zum Auftrag und Laufwerk gespeichert, z.B. die Seriennummer des Laufwerks und der Speicherkontoname. Diese Journaldatei wird nicht auf dem Laufwerk gespeichert. Sie wird beim Erstellen des Importauftrags verwendet. Eine Schritt-für-Schritt-Anleitung zum Erstellen eines Auftrags folgt später in diesem Artikel.

Das Clienttool ist nur mit dem 64-Bit-Windows-Betriebssystem kompatibel. Informationen zu den unterstützten Betriebssystemversionen finden Sie im Abschnitt [Betriebssystem](#operating-system).

Laden Sie die aktuelle Version des [Azure Import/Export-Clienttools](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409) herunter. Weitere Details zum Microsoft Import/Export-Tool für Azure finden Sie in der [Referenz zum Azure Import/Export-Tool](http://go.microsoft.com/fwlink/?LinkId=329032).

### Festplattenlaufwerke

Für den Import/Export-Dienst werden nur interne 3,5-Zoll-SATA II/III-Festplatten unterstützt. Sie können Festplatten mit bis zu 10TB verwenden. Bei Importaufträgen wird nur das erste Datenvolume auf dem Laufwerk verarbeitet. Das Datenvolume muss mit NTFS formatiert sein. Beim Kopieren von Daten auf Ihre Festplatte können Sie sie direkt mit einem SATA-Connector oder extern mit einem externen SATA II/III-USB-Adapter verbinden. Wir empfehlen Ihnen die Verwendung von einem der folgenden externen SATA II/III-USB-Adapter:

- Anker 68UPSATAA-02BU
- Anker 68UPSHHDS-BU
- Startech SATADOCK22UE
- Sharkoon QuickPort XT HC

Falls Sie einen Konverter haben, der nicht in der Liste enthalten ist, können Sie versuchen, das Import-/Exporttool für Azure mit Ihrem Konverter zur Vorbereitung des Laufwerks auszuführen, bevor Sie einen unterstützten Konverter erwerben.

> [AZURE.IMPORTANT] Externe Festplattenlaufwerke mit einem integrierten USB-Adapter werden von diesem Dienst nicht unterstützt. Außerdem kann der Datenträger im Gehäuse einer externen Festplatte nicht verwendet werden. Senden Sie bitte keine externen Festplatten ein.

### Verschlüsselung

Die Daten auf dem Laufwerk müssen mit BitLocker-Laufwerkverschlüsselung verschlüsselt sein. So sind Ihre Daten beim Transport geschützt.

Für Importaufträge gibt es zwei Möglichkeiten zum Durchführen der Verschlüsselung. Die erste Möglichkeit ist die Verwendung des Parameters „/encrypt“, wenn Sie das Clienttool während der Vorbereitung des Laufwerks ausführen. Die zweite Möglichkeit ist das Aktivieren der BitLocker-Verschlüsselung manuell auf dem Laufwerk und das Angeben des Verschlüsselungsschlüssels bei der Vorbereitung des Laufwerks in der Befehlszeile des Clienttools.

Bei Exportaufträgen wendet der Dienst nach dem Kopieren Ihrer Daten auf die Laufwerke jeweils die BitLocker-Verschlüsselung auf das Laufwerk an, bevor es an Sie zurückgeschickt wird. Der Verschlüsselungsschlüssel wird für Sie über das klassische Portal bereitgestellt.

### Betriebssystem

Sie können eines der folgenden 64-Bit-Betriebssysteme zum Vorbereiten der Festplatte mit dem Azure Import/Export-Tool verwenden, bevor Sie die Festplatte an Azure schicken:

Windows 7 Enterprise, Windows 7 Ultimate, Windows 8 Pro, Windows 8 Enterprise, Windows 8.1 Pro, Windows 8.1 Enterprise, Windows 10<sup>1</sup>, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2. Alle diese Betriebssysteme unterstützen die BitLocker-Laufwerkverschlüsselung.

> [AZURE.IMPORTANT] <sup>1</sup>Laden Sie die neueste Version des Import/Export-Tools für Azure herunter, wenn Sie die Festplatte mit einem Computer unter Windows 10 vorbereiten.

### Standorte

Der Azure Import/Export-Dienst unterstützt das Kopieren von Daten von bzw. in alle öffentlichen Azure-Speicherkonten. Sie können Festplatten an einen der unten angegebenen Standorte schicken. Falls sich Ihr Speicherkonto an einem öffentlichen Azure-Standort befindet, der hier nicht aufgeführt ist, wird ein anderer Zielstandort angegeben, wenn Sie den Auftrag mit dem klassischen Portal oder mit der Import/Export-REST-API erstellen.

Unterstützte Standorte für die Verschickung:

- USA (Ost)

- USA (West)

- USA (Ost) 2

- USA (Mitte)

- USA Nord Mitte

- USA Süd Mitte

- Nordeuropa

- Westeuropa

- Asien (Osten)

- Südostasien

- Australien (Osten)

- Australien (Südosten)

- Japan (Westen)

- Japan Ost

- Indien (Mitte)


### Versand

**Schicken von Festplatten an das Rechenzentrum:**

Beim Erstellen eines Import- oder Exportauftrags erhalten Sie die Versandadresse eines unterstützten Standorts, an den Sie die Festplatten schicken können. Die angegebene Adresse richtet sich nach dem Standort Ihres Speicherkontos, aber sie muss nicht unbedingt dem Standort des Speicherkontos entsprechen.

Sie können Kurierdienste wie FedEx, DHL, UPS oder US Postal Service nutzen, um Festplatten an die Versandadresse zu schicken.

**Schicken von Festplatten aus dem Rechenzentrum:**

Beim Erstellen eines Import- oder Exportauftrags müssen Sie eine Rücksendeadresse angeben, an die Festplatten nach Abschluss des Auftrags von Microsoft zurückgeschickt werden können. Achten Sie darauf, dass Sie eine gültige Rücksendeadresse angeben, um Verzögerungen bei der Verarbeitung zu vermeiden.

Sie müssen auch eine gültige Nummer Ihres Kontos bei FedEx oder DHL angeben, die von Microsoft zum Zurücksenden der Laufwerke verwendet werden kann. Eine FedEx-Kontonummer wird benötigt, um den Rückversand für Festplatten von Standorten in den USA und Europa durchzuführen. Eine DHL-Kontonummer wird benötigt, um den Rückversand für Festplatten von Standorten in Asien und Australien durchzuführen. Sie können ein Konto bei [FedEx](http://www.fedex.com/us/oadr/) (für USA und Europa) oder [DHL](http://www.dhl.com/) (Asien und Australien) erstellen, falls Sie noch nicht über ein solches Konto verfügen. Wenn Sie bereits über die Nummer eines Kontos bei einem Kurierdienst verfügen, sollten Sie überprüfen, ob sie gültig ist.

Beim Versand Ihrer Pakete müssen Sie die Nutzungsbedingungen unter [Microsoft Azure-Nutzungsbedingungen](https://azure.microsoft.com/support/legal/services-terms/) beachten.

> [AZURE.IMPORTANT] Beachten Sie, dass die physischen Medien beim Versand unter Umständen Ländergrenzen überqueren. Sie müssen sicherstellen, dass Ihre physischen Medien und Daten gemäß geltender Gesetze importiert bzw. exportiert werden. Prüfen Sie vor dem Versand der physischen Medien mit Ihren Rechtsberatern, ob Medien und Daten laut Gesetz an das entsprechende Rechenzentrum verschickt werden dürfen. So stellen Sie sicher, dass Ihre Daten zeitnah bei Microsoft eintreffen. Z.B. muss allen Paketen, die Ländergrenzen überschreiten (Grenzüberschreitungen innerhalb der Europäischen Union ausgenommen), eine Handelsrechnung beiliegen. Sie könnten eine ausgefüllte Kopie der Handelsrechnung von der Website des Frachtführers ausdrucken. Beispiele für Handelsrechnungen sind die [DHL-Handelsrechnung](http://invoice-template.com/wp-content/uploads/dhl-commercial-invoice-template.pdf) oder die [FedEx-Handelsrechnung](http://images.fedex.com/downloads/shared/shipdocuments/blankforms/commercialinvoice.pdf). Stellen Sie sicher, dass Microsoft nicht als Exporteur angegeben wurde.

## Wie funktioniert der Azure Import/Export-Dienst?

Sie können Daten zwischen Ihrem lokalen Standort und Azure-Blobspeicher übertragen, indem Sie den Azure Import/Export-Dienst nutzen. Sie erstellen Aufträge und schicken Festplattenlaufwerke an ein Azure-Rechenzentrum. Jedes Festplattenlaufwerk, das Sie verschicken, ist einem einzelnen Auftrag zugeordnet. Jeder Auftrag ist einem einzelnen Speicherkonto zugeordnet. Lesen Sie sich den [Abschnitt zu den Voraussetzungen](#pre-requisites) sorgfältig durch, um sich über die Besonderheiten dieses Diensts zu informieren, z.B. unterstützte Blobtypen, Datenträgertypen, Standorte und Versand.

In diesem Abschnitt werden die allgemeinen Schritte beschrieben, die für Import- und Exportaufträge ausgeführt werden müssen. Weiter unten im [Abschnitt „Schnellstart“](#quick-start) finden Sie eine Schritt-für-Schritt-Anleitung zum Erstellen eines Import- und Exportauftrags.

### Importaufträge

Ein Importauftrag umfasst im Allgemeinen die folgenden Schritte:

- Bestimmen Sie die zu importierenden Daten und die Anzahl der benötigten Festplatten.
- Geben Sie die Zielblobs für Ihre Daten in Blob Storage an.
- Verwenden Sie das Azure Import/Export-Tool, um die Daten auf eine oder mehrere Festplatten zu kopieren und per BitLocker zu verschlüsseln.
- Erstellen Sie einen Importauftrag in Ihrem klassischen Zielspeicherkonto mit dem klassischen Portal oder der Import/Export-REST-API. Laden Sie bei Verwendung des klassischen Portals die Laufwerkjournaldateien hoch.
- Geben Sie die Rücksendeadresse und die Nummer Ihres Kontos beim Kurierdienst an, die für das Zurücksenden der Festplatten verwendet werden können.
- Schicken Sie die Festplatten an die Versandadresse, die Sie während der Erstellung des Auftrags erhalten haben.
- Aktualisieren Sie die Nummer für die Sendungsverfolgung in den Details des Importauftrags, und senden Sie den Importauftrag ab.
- Die Festplatten kommen im Azure-Rechenzentrum an und werden verarbeitet.
- Die Festplatten werden über Ihr Kurierdienstkonto an die Rücksendeadresse geschickt, die im Importauftrag angegeben ist.

	![Abbildung 1: Ablauf eines Importauftrags](./media/storage-import-export-service/importjob.png)


### Exportaufträge

Ein Exportauftrag umfasst im Allgemeinen die folgenden Schritte:

- Bestimmen Sie die zu exportierenden Daten und die Anzahl der benötigten Festplatten.
- Identifizieren Sie die Quellblobs oder Containerpfade Ihrer Daten im Blobspeicher.
- Erstellen Sie einen Exportauftrag in Ihrem Quellspeicherkonto mit dem klassischen Portal oder der Import/Export-REST-API.
- Geben Sie die Quellblobs oder Containerpfade Ihrer Daten im Exportauftrag an.
- Geben Sie die Rücksendeadresse und die Nummer Ihres Kontos beim Kurierdienst an, die für das Zurücksenden der Festplatten verwendet werden können.
- Schicken Sie die Festplatten an die Versandadresse, die Sie während der Erstellung des Auftrags erhalten haben.
- Aktualisieren Sie die Nummer für die Sendungsverfolgung in den Details des Exportauftrags, und senden Sie den Exportauftrag ab.
- Die Festplatten kommen im Azure-Rechenzentrum an und werden verarbeitet.
- Die Festplatten werden mit BitLocker verschlüsselt. Die Schlüssel sind über das klassische Portal verfügbar.
- Die Festplatten werden über Ihr Kurierdienstkonto an die Rücksendeadresse zurückgeschickt, die im Importauftrag angegeben ist.

	![Abbildung 2: Ablauf eines Exportauftrags](./media/storage-import-export-service/exportjob.png)

### Anzeigen des Auftragsstatus

Sie können den Status Ihrer Import- oder Exportaufträge im klassischen Portal nachverfolgen. Navigieren Sie im klassischen Portal zu Ihrem Speicherkonto, und klicken Sie auf die Registerkarte **Import/Export**. Eine Liste Ihrer Aufträge wird auf der Seite angezeigt. Sie können die Liste nach Auftragsstatus, Auftragsname, Auftragstyp oder Tracking-Nummer filtern.

Je nachdem, an welchem Punkt des Prozesses sich Ihre Festplatte befindet, wird einer der folgenden Auftragsstatus angezeigt.

| Auftragsstatus | Beschreibung |
|:-------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Wird erstellt | Ihr Auftrag wurde erstellt, aber Sie haben Ihre Versandinformationen noch nicht angegeben. |
| Versand | Ihr Auftrag wurde erstellt, und Sie haben Ihre Versandinformationen angegeben. **Hinweis**: Nachdem die Festplatte an das Azure-Rechenzentrum geliefert wurde, kann als Status vorerst noch „Versand“ angezeigt werden. Wenn der Dienst mit dem Kopieren der Daten beginnt, ändert sich der Status in „Übertragung wird ausgeführt“. Falls Sie einen genaueren Status für Ihre Festplatte angezeigt bekommen möchten, können Sie die Import/Export-REST-API verwenden. |
| Übertragung wird ausgeführt | Ihre Daten werden von Ihrem Laufwerk (bei einem Importauftrag) oder auf Ihr Laufwerk (bei einem Exportauftrag) übertragen. |
| Verpackung | Die Übertragung Ihrer Daten ist abgeschlossen, und Ihre Festplatte wird für den Rückversand vorbereitet. |
| Abgeschlossen | Ihre Festplatte wurde an Sie zurückgeschickt. |

### Verarbeitungsdauer des Auftrags

Die Verarbeitungsdauer eines Import- oder Exportauftrags kann aufgrund von verschiedenen Faktoren variieren, z.B. Versanddauer, Auftragstyp, Typ und Größe der zu kopierenden Daten und Größe der geschickten Festplatten. Der Import/Export-Dienst verfügt nicht über eine Vereinbarung zum Servicelevel (SLA). Sie können die REST-API verwenden, um den Auftragsstatus genauer verfolgen zu können. Der Vorgang zum Auflisten von Aufträgen enthält einen Parameter für die prozentuale Fertigstellung, mit dem Sie den Status des Kopiervorgangs verfolgen können. Sie können sich gern an uns wenden, falls Sie für einen zeitkritischen Import- oder Exportauftrag einen geschätzten Fertigstellungszeitpunkt benötigen.

### Preise

**Bearbeitungsgebühr für Festplatte**

Für jede Festplatte, die im Rahmen eines Import- oder Exportauftrags verarbeitet wird, wird eine Bearbeitungsgebühr berechnet. Informationen hierzu finden Sie unter [Preise für Import/Export](https://azure.microsoft.com/pricing/details/storage-import-export/).

**Versandkosten**

Wenn Sie Festplatten an Azure schicken, übernehmen Sie die Versandkosten des Kurierdiensts. Wenn die Festplatten von Microsoft an Sie zurückgeschickt werden, werden die Versandkosten Ihrem Kurierdienstkonto belastet, das Sie bei der Erstellung des Auftrags angegeben haben.

**Transaktionskosten**

Für das Importieren von Daten in den Blobspeicher fallen keine Transaktionskosten an. Die standardmäßigen Gebühren für ausgehende Übertragungen fallen an, wenn Daten aus dem Blobspeicher exportiert werden. Weitere Informationen zu Transaktionskosten finden Sie unter [Datenübertragungen – Preisdetails](https://azure.microsoft.com/pricing/details/data-transfers/).

## Schnellstart

Dieser Abschnitt enthält eine Schritt-für-Schritt-Anleitung zum Erstellen eines Import- und Exportauftrags. Stellen Sie sicher, dass alle [Voraussetzungen](#pre-requisites) erfüllt sind, bevor Sie fortfahren.

## Wie kann ich einen Importauftrag erstellen?

Erstellen Sie einen Importauftrag zum Kopieren von Daten von Festplatten in Ihr Azure-Speicherkonto, indem Sie eine oder mehrere Festplatten mit Daten an das angegebene Rechenzentrum schicken. Im Rahmen des Importauftrags werden Details zu Festplattenlaufwerken, die zu kopierenden Daten, das Zielspeicherkonto und Versandinformationen an den Azure Import/Export-Dienst übermittelt. Die Erstellung eines Importauftrags ist ein dreistufiger Prozess. Bereiten Sie die Festplatten zuerst mit dem Azure Import/Export-Clienttool vor. Senden Sie anschließend einen Importauftrag über das klassische Portal. Der dritte Schritt umfasst das Schicken der Festplatten an die Versandadresse, die bei der Erstellung des Auftrags angegeben wurde, und das Aktualisieren der Versandinformationen in den Auftragsdetails.

> [AZURE.IMPORTANT] Sie können nur einen Auftrag pro Speicherkonto senden. Jede verschickte Festplatte kann in ein Speicherkonto importiert werden. Angenommen, Sie möchten Daten in zwei Speicherkonten importieren. In diesem Fall müssen Sie separate Festplattenlaufwerke für jedes Speicherkonto verwenden und pro Speicherkonto einen eigenen Auftrag erstellen.

### Vorbereiten Ihrer Laufwerke

Der erste Schritt beim Importieren von Daten mit dem Azure Import/Export-Dienst ist das Vorbereiten Ihrer Festplatten mit dem Azure Import/Export-Clienttool. Führen Sie die folgenden Schritte aus, um die Festplatten vorzubereiten:

1.	Identifizieren Sie die zu importierenden Daten. Hierbei kann es sich um Verzeichnisse und eigenständige Dateien auf dem lokalen Server oder einer Netzwerkfreigabe handeln.

2.	Bestimmen Sie anhand der Gesamtgröße der Daten die Anzahl von benötigten Festplatten. Beschaffen Sie die erforderliche Anzahl von SATA II/III-Festplatten (3,5 Zoll).

3.	Identifizieren Sie das Zielspeicherkonto, den Container, die virtuellen Verzeichnisse und die Blobs.

4.	Bestimmen Sie die Verzeichnisse bzw. eigenständigen Dateien, die auf jede Festplatte kopiert werden sollen.

5.	Verwenden Sie das [Azure Import/Export-Tool](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409), um Ihre Daten auf eine oder mehrere Festplatten zu kopieren.

	- Mit dem Azure Import/Export-Tool werden Kopiersitzungen zum Kopieren der Daten von der Quelle auf die Festplatten erstellt. In einer Kopiersitzung kann das Tool ein einzelnes Verzeichnis mit seinen Unterverzeichnissen oder eine einzelne Datei kopieren.

	- Unter Umständen benötigen Sie mehrere Kopiersitzungen, wenn Ihre Quelldaten auf mehrere Verzeichnisse verteilt sind.

	- Für jede Festplatte, die Sie vorbereiten, benötigen Sie mindestens eine Kopiersitzung.

6.	Sie können den Parameter „/encrypt“ angeben, um für die Festplatte die BitLocker-Verschlüsselung zu aktivieren. Alternativ dazu können Sie die BitLocker-Verschlüsselung auf der Festplatte auch manuell aktivieren und beim Ausführen des Tools den Schlüssel angeben.

7.	Das Azure Import/Export-Tool generiert bei der Vorbereitung für jedes Laufwerk eine Laufwerkjournaldatei. Die Laufwerkjournaldatei wird auf Ihrem lokalen Computer gespeichert, nicht auf dem Laufwerk selbst. Beim Erstellen des Importauftrags laden Sie die Journaldatei hoch. Eine Laufwerkjournaldatei enthält die Laufwerk-ID und die BitLocker-Schlüssel sowie weitere Informationen über das Laufwerk. **Wichtig**: Für jede vorbereitete Festplatte ergibt sich eine Journaldatei. Wenn Sie den Importauftrag mit dem klassischen Portal erstellen, müssen Sie alle Journaldateien der Festplatten hochladen, die Teil des Importauftrags sind. Festplatten ohne Journaldateien werden nicht verarbeitet.

8.	Ändern Sie die Daten auf den Festplatten oder die Journaldatei nicht mehr, nachdem Sie die Festplattenvorbereitung abgeschlossen haben.

Unten sind die Befehle und Beispiele zum Vorbereiten der Festplatte mit dem Azure Import/Export-Clienttool aufgeführt.

Befehl „PrepImport“ des Azure Import/Export-Clienttools für die erste Kopiersitzung zum Kopieren eines Verzeichnisses:

	WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

**Beispiel:**

Im Beispiel unten werden alle Dateien und Unterverzeichnisse aus „H:\\Video“ auf die Festplatte kopiert, die unter „X:“ bereitgestellt wurde. Die Daten werden in das Zielspeicherkonto, das mit dem Speicherkontoschlüssel angegeben wird, und in den Speichercontainer mit dem Namen „video“ importiert. Wenn der Speichercontainer nicht vorhanden ist, wird er erstellt. Mit diesem Befehl wird auch die Zielfestplatte formatiert und verschlüsselt.

	WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Video1 /logdir:c:\logs /sk:storageaccountkey /t:x /format /encrypt /srcdir:H:\Video1 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt

Befehl „PrepImport“ des Azure Import/Export-Clienttools für nachfolgende Kopiersitzungen zum Kopieren eines Verzeichnisses:

	WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

Geben Sie für nachfolgende Kopiersitzungen für Kopiervorgänge auf dieselbe Festplatte den gleichen Journaldateinamen und eine neue Sitzungs-ID an. Es ist nicht erforderlich, den Speicherkontoschlüssel und das Ziellaufwerk erneut anzugeben, und Sie müssen die Festplatte auch nicht formatieren oder verschlüsseln. In diesem Beispiel kopieren wir den Ordner „H:\\Photo“ und dessen Unterverzeichnisse auf dasselbe Ziellaufwerk in den Speichercontainer mit dem Namen „photo“.

	WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Photo /srcdir:H:\Photo /dstdir:photo/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt

Befehl „PrepImport“ des Azure Import/Export-Clienttools für die erste Kopiersitzung zum Kopieren einer Datei:

	WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcfile:<SourceFile> /dstblob:<DestinationBlobPath> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

Befehl „PrepImport“ des Azure Import/Export-Clienttools für nachfolgende Kopiersitzungen zum Kopieren einer Datei:

	WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /srcfile:<SourceFile> /dstblob:<DestinationBlobPath> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

**Beachten Sie Folgendes**: Die Daten werden standardmäßig als Blockblobs importiert. Sie können den Parameter „/BlobType“ verwenden, um Daten als Seitenblobs zu importieren. Wenn Sie beispielsweise VHD-Dateien importieren, die als Datenträger auf einer Azure VM bereitgestellt werden, müssen Sie sie als Seitenblobs importieren. Falls Sie nicht sicher sind, welchen Blobtyp Sie verwenden sollen, können Sie „/blobType:auto“ angeben. Wir helfen Ihnen dann beim Bestimmen des richtigen Typs. In diesem Fall werden alle VHD- und VHDX-Dateien als Seitenblobs importiert, und der Rest wird als Blockblobs importiert.

Weitere Details zur Verwendung des Azure Import/Export-Clienttools finden Sie unter [Vorbereiten von Festplatten für einen Importauftrag](https://msdn.microsoft.com/library/dn529089.aspx).

Ausführlichere Schritt-für-Schritt-Anleitungen finden Sie unter [Beispielworkflow zur Vorbereitung von Festplatten für einen Importauftrag](https://msdn.microsoft.com/library/dn529097.aspx).

### Importauftrag erstellen

1.	Sobald Sie Ihre Festplatte vorbereitet haben, navigieren Sie zum Speicherkonto im [klassischen Portal](https://manage.windowsazure.com), und zeigen Sie das Dashboard an. Klicken Sie unter **Schnelleinsicht** auf **Create an Import Job**. Sehen Sie sich die Schritte an, und aktivieren Sie das Kontrollkästchen, um anzugeben, dass Sie die Festplatte vorbereitet haben und die Laufwerkjournaldatei verfügbar ist.

2.	Geben Sie im ersten Schritt die Kontaktinformationen des Ansprechpartners für diesen Importauftrag und eine gültige Rücksendeadresse an. Wenn Sie ausführliche Protokolldaten für den Importauftrag speichern möchten, wählen Sie die Option **Save the verbose log in my 'waimportexport' blob container**.

3.	Laden Sie im zweiten Schritt die Protokolldateien hoch, die Sie während der Vorbereitung des Laufwerks erhalten haben. Sie müssen pro vorbereitetem Laufwerk eine Datei hochladen.

	![Importauftrag erstellen – Schritt 3](./media/storage-import-export-service/import-job-03.png)

4.	Geben Sie im dritten Schritt einen beschreibenden Namen für den Importauftrag ein. Beachten Sie, dass der eingegebene Name nur Kleinbuchstaben, Ziffern, Trennstriche und Unterstriche enthalten darf, mit einem Buchstaben beginnen muss und keine Leerzeichen enthalten darf. Mit dem ausgewählten Namen können Sie Ihre Aufträge während und nach der Bearbeitung nachverfolgen.

	Wählen Sie anschließend Ihr Rechenzentrum aus der Liste aus. Unter Rechenzentrumsregion wird das Rechenzentrum und die Adresse angegeben, an die Sie Ihr Paket schicken müssen. Weitere Informationen finden Sie unten in den FAQ.

5. 	Wählen Sie in Schritt 4 Ihren Kurierdienst für die Rücksendung aus, und geben Sie Ihr Kurierdienst-Kundenkonto an. Microsoft verwendet diese Kontodaten, um die Laufwerke nach Abschluss des Importauftrags an Sie zurückzuschicken.

	Falls Sie Ihre Nachverfolgungsnummer haben, können Sie Ihren Kurierdienst in der Liste auswählen und die Nummer eingeben.

	Wenn Sie noch keine Tracking-Nummer haben, wählen Sie **Ich werde die Versanddaten für diesen Importauftrag an, sobald ich mein Paket verschickt habe**, und schließen Sie den Importprozess ab.

6. Um Ihre Nachverfolgungsnummer einzugeben, nachdem Sie das Paket verschickt haben, kehren Sie zur Seite **Import/Export** für Ihr Speicherkonto im klassischen Portal zurück, wählen Sie den Auftrag in der Liste aus, und klicken Sie auf **Versandinformationen**. Navigieren Sie durch den Assistenten und geben Sie Ihre Nachverfolgungsnummer in Schritt 2 ein.

	Wenn die Nachverfolgungsnummer nicht innerhalb von zwei Wochen nach Erstellung des Auftrags aktualisiert wird, läuft der Auftrag ab.

	Falls der Auftrag den Status "Erstellung", "Versand" oder "Übertragung" hat, können Sie außerdem Ihre Kontonummer beim Transportunternehmen in Schritt 2 des Assistenten eingeben. Falls der Auftrag den Status "Verpackung" hat, können Sie Ihre Kontonummer beim Transportunternehmen nicht mehr ändern.

7. Sie können den Status des Auftrags im Dashboard des Portals nachverfolgen. Unter [Anzeigen des Auftragsstatus](#viewing-your-job-status) können Sie ermitteln, was ein Auftragsstatus des vorherigen Abschnitts jeweils bedeutet.

## Wie erstelle ich einen Exportauftrag?

Erstellen Sie einen Exportauftrag, um den Import/Export-Dienst darüber zu informieren, dass Sie ein oder mehrere leere Laufwerke an das Rechenzentrum schicken. So können die Daten von Ihrem Speicherkonto auf die Laufwerke exportiert werden, und die Laufwerke werden Ihnen dann zugeschickt.

### Vorbereiten Ihrer Laufwerke

Zur Vorbereitung von Laufwerken für einen Exportauftrag werden folgende Vorabprüfungen empfohlen:

1. Überprüfen Sie die Anzahl der erforderlichen Datenträger mithilfe des PreviewExport-Befehls aus dem Import/Export-Tool für Azure. Weitere Informationen finden Sie unter [Vorschau der Laufwerknutzung für einen Exportauftrag](https://msdn.microsoft.com/library/azure/dn722414.aspx). Sie können damit eine Vorschau der Festplattenverwendung für Blobs anzeigen, die Sie ausgewählt haben, basierend auf der Größe der Laufwerke, die Sie verwenden möchten.

2. Überprüfen Sie, ob Sie Lese-/Schreibzugriff auf die Festplatte haben, die für den Exportauftrag verschickt werden soll.

### Erstellen des Exportauftrags

1. 	Navigieren Sie zum Speicherkonto im [klassischen Portal](https://manage.windowsazure.com), und zeigen Sie das Dashboard an, um einen Exportauftrag zu erstellen. Klicken Sie unter **Auf einen Blick** auf **Exportauftrag erstellen**, und fahren Sie mit dem Assistenten fort.

2. 	Geben Sie im zweiten Schritt die Kontaktinformationen des Ansprechpartners für diesen Exportauftrag an. Wenn Sie ausführliche Protokolldaten für den Exportauftrag speichern möchten, wählen Sie die Option **Save the verbose log in my 'waimportexport' blob container**.

3.	Legen Sie im dritten Schritt fest, welche Blob-Daten Sie von Ihrem Speicherkonto auf Ihr leeres Laufwerk oder Ihre Laufwerke exportieren möchten. Sie können alle Blob-Daten des Speicherkontos exportieren, oder Sie legen fest, welche Blobs oder Blob-Sätze exportiert werden sollen.

	Verwenden Sie die Auswahl **Equal To**, und geben Sie den relativen Pfad zu dem Blob an, beginnend mit dem Containernamen, um den zu exportierenden Blob festzulegen. Verwenden Sie *$root*, um den Stammcontainer festzulegen.

	Verwenden Sie die Auswahl **Starts With**, und legen Sie das Präfix beginnend mit einem Schrägstrich "/" fest, um alle Blobs festzulegen, die mit einem Präfix beginnen. Bei dem Präfix kann es sich um das Präfix des Containernamens, den vollständigen Containernamen oder den vollständigen Containernamen gefolgt vom Präfix des Blob-Namens handeln.

	Die folgende Tabelle enthält Beispiele für gültige Blob-Pfade:

	Auswahl|Blob-Pfad|Beschreibung
	---|---|---
	Starts With|/|Exportiert alle Blobs im Speicherkonto
	Starts With|/$root/|Exportiert alle Blobs im Stammcontainer
	Starts With|/book|Exportiert alle Blobs in allen Containern mit dem Präfix **book**
	Beginnt mit|/music/|Exportiert alle Blobs im Container **music**
	Starts With|/music/love|Exportiert alle Blobs im Container **music**, die mit dem Präfix **love** beginnen
	Entspricht|$root/logo.bmp|Exportiert das Blob **logo.bmp** im Stammcontainer
	Entspricht|videos/story.mp4|Exportiert das Blob **story.mp4** im Container **videos**

	Sie müssen die Blob-Pfade in gültigen Formaten angeben, um Fehler während der Verarbeitung zu vermeiden. Dies ist in diesem Screenshot dargestellt.

	![Exportauftrag erstellen – Schritt 3](./media/storage-import-export-service/export-job-03.png)


4.	Geben Sie im vierten Schritt einen beschreibenden Namen für den Exportauftrag ein. Der eingegebene Name darf nur Kleinbuchstaben, Ziffern, Trennstriche und Unterstriche enthalten, muss mit einem Buchstaben beginnen und darf keine Leerzeichen enthalten.

	Die Datacenter-Region gibt das Datacenter an, an das Sie Ihr Paket schicken müssen. Weitere Informationen finden Sie unten in den FAQ.

5. 	Wählen Sie in Schritt 5 Ihren Rücktransporteur aus und geben Sie Ihr Kundenkonto beim Transportunternehmen an. Microsoft verwendet diese Kontodaten, um Ihre Laufwerke nach Abschluss des Exportauftrags an Sie zurückzuschicken.

	Falls Sie Ihre Nachverfolgungsnummer haben, können Sie Ihren Kurierdienst in der Liste auswählen und die Nummer eingeben.

	Falls Sie noch keine Nachverfolgungsnummer haben, wählen Sie die Option **Ich werde die Versanddaten für diesen Exportauftrag angeben, sobald ich mein Paket verschickt habe** aus und schließen Sie den Exportvorgang ab.

6. Um Ihre Nachverfolgungsnummer einzugeben, nachdem Sie das Paket verschickt haben, kehren Sie zur Seite **Import/Export** für Ihr Speicherkonto im klassischen Portal zurück, wählen Sie den Auftrag in der Liste aus, und klicken Sie auf **Versandinformationen**. Navigieren Sie durch den Assistenten und geben Sie Ihre Nachverfolgungsnummer in Schritt 2 ein.

	Wenn die Nachverfolgungsnummer nicht innerhalb von zwei Wochen nach Erstellung des Auftrags aktualisiert wird, läuft der Auftrag ab.

	Falls der Auftrag den Status "Erstellung", "Versand" oder "Übertragung" hat, können Sie außerdem Ihre Kontonummer beim Transportunternehmen in Schritt 2 des Assistenten eingeben. Falls der Auftrag den Status "Verpackung" hat, können Sie Ihre Kontonummer beim Transportunternehmen nicht mehr ändern.

	> [AZURE.NOTE] Wenn das zu exportierende Blob während des Kopierens auf die Festplatte verwendet wird, erstellt der Azure Import/Export-Dienst eine Momentaufnahme des Blobs und kopiert die Momentaufnahme.

7.	Sie können den Auftragsstatus im Dashboard im klassischen Portal nachverfolgen. Im vorherigen Abschnitt unter „Anzeigen des Auftragsstatus“ wird beschrieben, was ein Auftragsstatus jeweils bedeutet.

8.	Nachdem Sie die Laufwerke mit den exportierten Daten erhalten haben, können Sie die BitLocker-Schlüssel anzeigen und kopieren, die vom Dienst für Ihre Festplatte generiert wurden. Navigieren Sie im klassischen Portal zu Ihrem Speicherkonto, und klicken Sie auf die Registerkarte „Import/Export“. Wählen Sie Ihren Exportauftrag in der Liste aus, und klicken Sie auf die Schaltfläche Schlüssel anzeigen. Die BitLocker-Schlüssel werden wie hier dargestellt angezeigt:

	![BitLocker-Schlüssel für einen Exportauftrag anzeigen](.\media\storage-import-export-service\export-job-bitlocker-keys.png)

Sehen Sie sich unten den Abschnitt mit den häufig gestellten Fragen an. Darin werden die häufigsten Fragen von Kunden beantwortet, die bei der Nutzung des Diensts entstehen.

## Häufig gestellte Fragen ##


**Wie lange dauert das Kopieren meiner Daten, nachdem meine Festplatten im Rechenzentrum angekommen sind?**

Die Kopierdauer variiert in Abhängigkeit von verschiedenen Faktoren wie Auftragstyp, Typ und Größe der kopierten Daten, Größe der bereitgestellten Festplatten und vorhandene Workload. Je nach diesen Faktoren kann der Vorgang zwischen zwei Tagen und zwei Wochen dauern. Daher ist es schwierig, einen allgemeinen Schätzwert zu nennen. Der Dienst versucht, Ihren Auftrag zu optimieren, indem nach Möglichkeit mehrere Festplatten parallel kopiert werden. Sie können sich gern an uns wenden, falls Sie für einen zeitkritischen Import- oder Exportauftrag eine Schätzung benötigen.

**Wann sollte der Import/Export-Dienst von Azure verwendet werden?** Erwägen Sie, den Import/Export-Dienst von Azure einzusetzen, wenn das Hoch- oder Herunterladen über das Netzwerk grob geschätzt mehr als 7 Tage dauern wird. Die Dauer können Sie mit einem beliebigen Onlinetaschenrechner berechnen, oder Sie können den Rechner [herunterladen](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/archive/master.zip), der sich in unserem Azure Import/Export-REST-API-Beispiel im Azure-Beispielrepository unter [Data Transfer Speed Calculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) (Rechner zum Berechnen der Datenübertragungsgeschwindigkeit) befindet. Dies ist keine genaue Berechnung, sondern nur eine ungefähre Angabe.

**Kann ich den Azure Import/Export-Dienst mit einem Resource Manager-Speicherkonto verwenden?**

Nein. Sie können den Azure Import/Export-Dienst nicht verwenden, um Daten direkt in ein oder aus einem Resource Manager-Speicherkonto zu kopieren. Der Dienst unterstützt nur klassische Speicherkonten. Die Unterstützung von Resource Manager-Speicherkonten wird in Kürze verfügbar sein. Als Alternativlösung können Sie Daten in ein klassisches Speicherkonto importieren und das Konto mit [AzCopy](storage-use-azcopy.md) oder CopyBlob zu Ihrem Resource Manager-Speicherkonto migrieren.

**Kann ich den Azure Import/Export-Dienst verwenden, um Azure Files zu kopieren?**

Nein. Der Azure Import/Export-Dienst unterstützt nur Blockblobs und Seitenblobs. Alle andere Speichertypen, z.B. Azure Files, Tabellen und Warteschlangen, werden nicht unterstützt.

**Ist der Azure Import/Export-Dienst für CSP-Abonnements verfügbar?**

Nein. Der Azure Import/Export-Dienst unterstützt keine CSP-Abonnements. Diese Unterstützung ist für die Zukunft geplant.

**Kann ich den Schritt für die Festplattenvorbereitung für einen Importauftrag überspringen, oder kann ich eine Festplatte ohne Kopiervorgang vorbereiten?**

Alle Festplatten, die Sie zum Importieren von Daten verschicken möchten, müssen mit dem Azure Import/Export-Clienttool vorbereitet werden. Sie müssen das Clienttool verwenden, um die Daten auf die Festplatte zu kopieren.

**Muss ich den Datenträger beim Erstellen eines Exportauftrags vorbereiten?**

Nein, aber es empfiehlt sich, einige Vorabüberprüfungen durchzuführen. Überprüfen Sie die Anzahl der erforderlichen Datenträger mithilfe des PreviewExport-Befehls aus dem Import/Export-Tool für Azure. Weitere Informationen finden Sie unter [Vorschau der Laufwerknutzung für einen Exportauftrag](https://msdn.microsoft.com/library/azure/dn722414.aspx). Sie können damit eine Vorschau der Festplattenverwendung für Blobs anzeigen, die Sie ausgewählt haben, basierend auf der Größe der Laufwerke, die Sie verwenden möchten. Überprüfen Sie außerdem, ob Sie Lese-/Schreibzugriff auf die Festplatte haben, die für den Exportauftrag versendet werden soll.

**Was geschieht, wenn ich aus Versehen eine HDD verschicke, die den unterstützten Anforderungen nicht entspricht?**

Das Azure-Datacenter schickt das Laufwerk, das den unterstützten Anforderungen nicht entspricht, an Sie zurück. Wenn nur einige der Laufwerke in dem Paket die Anforderungen erfüllen, werden diese Laufwerke verarbeitet, und die Laufwerke, die die Anforderungen nicht erfüllen, werden an Sie zurückgeschickt.

**Kann ich meinen Auftrag stornieren?**

Sie können einen Auftrag stornieren, solange dieser den Status "Wird erstellt" oder "Wird versendet" hat.

**Wie lange kann ich den Status abgeschlossener Aufträge im klassischen Portal anzeigen?**

Den Status abgeschlossener Aufträge können Sie bis zu 90 Tage lang anzeigen. Abgeschlossene Aufträge werden nach 90 Tagen gelöscht.

**Was soll ich tun, wenn ich mehr als 10 Laufwerke importieren oder exportieren möchte?**

Jeder Import- oder Exportauftrag kann auf maximal 10 Laufwerke für den Import-/Exportdienst verweisen. Wenn Sie mehr als zehn Laufwerke verschicken möchten, können Sie mehrere Aufträge erstellen. Laufwerke, die demselben Auftrag zugeordnet sind, müssen zusammen in einem Paket verschickt werden.

**Kann ich einen USB-SATA-Adapter verwenden, der nicht in der Liste mit den Empfehlungen aufgeführt ist?**

Falls Sie einen Konverter haben, der nicht in der Liste enthalten ist, können Sie versuchen, das Import-/Exporttool für Azure mit Ihrem Konverter zur Vorbereitung des Laufwerks auszuführen, bevor Sie einen unterstützten Konverter erwerben.

**Werden die Festplatten vom Dienst formatiert, bevor sie zurückgeschickt werden?**

Nein. Alle Festplatten werden mit BitLocker verschlüsselt.

**Kann ich bei Microsoft Laufwerke für Import-/Exportaufträge kaufen?**

Nein, Sie müssen sowohl für den Import als auch den Export Ihre eigenen Laufwerke einsenden.

**Wie sehen meine Daten nach Abschluss des Importauftrags im Speicherkonto aus? Wird meine Verzeichnishierarchie beibehalten?**

Beim Vorbereiten einer Festplatte für einen Importauftrag wird das Ziel mit dem Parameter „/dstdir:“ angegeben. Dies ist der Zielcontainer im Speicherkonto, in den Daten von der Festplatte kopiert werden. In diesem Zielcontainer werden virtuelle Verzeichnisse für Ordner von der Festplatte und Blobs für Dateien erstellt.

**Wenn die Festplatte Dateien enthält, die in meinem Speicherkonto bereits vorhanden sind, werden die entsprechenden Blobs in meinem Speicherkonto vom Dienst dann überschrieben?**

Beim Vorbereiten der Festplatte können Sie angeben, ob die Zieldateien überschrieben oder ignoriert werden sollen. Hierfür verwenden Sie den Parameter „/Disposition:<rename|no-overwrite|overwrite>“. In der Standardeinstellung benennt der Dienst die neuen Dateien um, anstatt vorhandene Blobs zu überschreiben.

**Ist das Azure Import/Export-Clienttool mit 32-Bit-Betriebssystemen kompatibel?** Nein. Das Clienttool ist nur mit 64-Bit-Windows-Betriebssystemen kompatibel. Eine vollständige Liste mit den unterstützten Betriebssystemversionen finden Sie im Abschnitt „Betriebssystem“ unter [Voraussetzungen](#pre-requisites).

**Sollte mein Paket noch andere Dinge als die Festplatte enthalten?**

Bitte verschicken Sie nur Ihre Laufwerke. Legen Sie keine Gegenstände wie z. B. Strom- oder USB-Kabel bei.

**Muss ich meine Festplatten mit FedEx oder DHL schicken?**

Sie können Festplatten mit allen gängigen Kurierdiensten wie FedEx, DHL, UPS oder US Postal Service an das Rechenzentrum schicken. Für den Rückversand der Festplatten aus dem Rechenzentrum müssen Sie aber eine FedEx-Kontonummer (USA und Europa) oder eine DHL-Kontonummer (Asien und Australien) angeben.

**Gelten für den internationalen Versand meiner Festplatte bestimmte Einschränkungen?**

Beachten Sie, dass die physischen Medien beim Versand unter Umständen Ländergrenzen überqueren. Sie müssen sicherstellen, dass Ihre physischen Medien und Daten gemäß geltender Gesetze importiert bzw. exportiert werden. Prüfen Sie vor dem Versand der physischen Medien mit Ihren Rechtsberatern, ob Medien und Daten laut Gesetz an das entsprechende Rechenzentrum verschickt werden dürfen. So stellen Sie sicher, dass Ihre Daten zeitnah bei Microsoft eintreffen.

**Beim Erstellen des Auftrags habe ich festgestellt, dass die Versandadresse ein Standort ist, der sich von meinem Speicherkontostandort unterscheidet. Wie soll ich vorgehen?**

Einige Speicherkontostandorte sind alternativen Standorten für den Versand zugeordnet. Versandstandorte, die bisher verfügbar waren, können vorübergehend auch anderen Standorten zugeordnet werden. Überprüfen Sie immer die Versandadresse, die bei der Erstellung des Auftrags angegeben wurde, bevor Sie die Festplatten verschicken.

**Warum wird als Auftragsstatus im klassischen Portal „Versand“ angezeigt, während auf der Website des Transportunternehmens angezeigt wird, dass mein Paket zugestellt wurde?**

Der Status im klassischen Portal ändert sich von „Versand“ in „Übertragung wird ausgeführt“, wenn die Verarbeitung des Laufwerks beginnt. Wenn das Laufwerk den Standort erreicht hat, aber die Verarbeitung noch nicht gestartet wurde, wird als Auftragsstatus „Versand“ angezeigt.

**Beim Versand der Festplatte fragt der Kurierdienst nach dem Namen und der Telefonnummer des Ansprechpartners im Rechenzentrum. Was muss ich hier angeben?**

Die Telefonnummer erhalten Sie bei der Erstellung des Auftrags. Falls Sie den Namen eines Ansprechpartners benötigen, können Sie sich unter waimportexport@microsoft.com an uns wenden. Wir senden Ihnen dann einen Namen zu.

**Kann ich den Azure Import/Export-Dienst zum Kopieren von PST-Postfächern und SharePoint-Daten in O365 verwenden?**

Informationen hierzu finden Sie unter [Importieren von PST-Dateien oder SharePoint-Daten in Office 365](https://technet.microsoft.com/library/ms.o365.cc.ingestionhelp.aspx).

**Kann ich den Azure Import/Export-Dienst zum Offlinekopieren meiner Sicherungen in den Azure Backup-Dienst verwenden?**

Informationen hierzu finden Sie unter [Workflow zur Offlinesicherung in Azure Backup](../backup/backup-azure-backup-import-export.md).

## Weitere Informationen:

- [Einrichten des Azure-Import-/Exporttools](https://msdn.microsoft.com/library/dn529112.aspx)

- [Übertragen von Daten mit dem Befehlszeilenprogramm AzCopy](storage-use-azcopy.md)

- [Azure Import/Export-REST-API-Beispiel](https://azure.microsoft.com/documentation/samples/storage-dotnet-import-export-job-management/)

<!---HONumber=AcomDC_0921_2016-->