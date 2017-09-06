---
title: "Verwenden des Azure Import/Export-Diensts zum Übertragen von Daten in und aus Blob Storage | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Import- und Exportaufträge im Azure-Portal erstellen, um Daten in den und aus Blob Storage zu übertragen."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 668f53f2-f5a4-48b5-9369-88ec5ea05eb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2017
ms.author: muralikk
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 5696c99b719fb1c5ca9c3da7dbf23d365cc64a2e
ms.contentlocale: de-de
ms.lasthandoff: 08/23/2017

---
# <a name="use-the-microsoft-azure-importexport-service-to-transfer-data-to-blob-storage"></a>Verwenden des Microsoft Azure Import/Export-Diensts zum Übertragen von Daten in Blob Storage
Mit dem Import/Export-Dienst von Azure können Sie große Datenmengen auf sichere Weise in Azure Blob Storage übertragen, indem Sie Festplattenlaufwerke an ein Azure-Rechenzentrum schicken. Sie können diesen Dienst auch zum Übertragen von Daten aus Azure Blob Storage auf Festplattenlaufwerke und zum Versand an Ihren lokalen Standort nutzen. Dieser Dienst eignet sich für Situationen, in denen Sie mehrere TB an Daten an oder von Azure übertragen möchten und in denen das Hoch- oder Herunterladen über das Netzwerk aufgrund der eingeschränkten Bandbreite oder hoher Netzwerkkosten nicht möglich ist.

Für den Dienst ist es erforderlich, dass Festplattenlaufwerke über eine BitLocker-Verschlüsselung verfügen, damit für die Sicherheit Ihrer Daten gesorgt ist. Der Dienst unterstützt klassische und Azure Resource Manager-Speicherkonten (standardmäßige und kalte Ebene), die in allen Regionen des öffentlichen Azure vorhanden sind. Schicken Sie die Festplattenlaufwerke an einen der unterstützten Standorte, die weiter unten in diesem Artikel angegeben sind.

In diesem Artikel erfahren Sie mehr zum Import/Export-Dienst von Azure und zum Versand von Laufwerken, um Ihre Daten in und aus Azure Blob Storage zu kopieren.

## <a name="when-should-i-use-the-azure-importexport-service"></a>Wann sollte der Import/Export-Dienst von Azure verwendet werden?
Sie können den Import/Export-Dienst von Azure nutzen, wenn das Hoch- bzw. Herunterladen von Daten über das Netzwerk zu langsam oder das Hinzufügen weiterer Bandbreite aus Kostengründen nicht möglich ist.

Sie können diesen Dienst in folgenden Szenarien verwenden:

* Migrieren von Daten zur Cloud: Verschieben Sie große Datenmengen schnell und kostengünstig nach Azure.
* Inhaltsverteilung: Schicken Sie Daten schnell an Ihre Kundenstandorte.
* Sicherung: Erstellen Sie Sicherungen Ihrer lokalen Daten für die Speicherung in Azure Blob Storage.
* Datenwiederherstellung: Stellen Sie große Datenmengen wieder her, die in Blob Storage gespeichert sind, und lassen Sie sie an Ihren lokalen Standort schicken.

## <a name="prerequisites"></a>Voraussetzungen
In diesem Abschnitt werden die Voraussetzungen zur Nutzung dieses Diensts aufgeführt. Lesen Sie sich diese Informationen sorgfältig durch, bevor Sie Ihre Laufwerke verschicken.

### <a name="storage-account"></a>Speicherkonto
Sie müssen über ein Azure-Abonnement und ein oder mehrere Speicherkonten verfügen, um den Import/Export-Dienst nutzen zu können. Bei jedem Auftrag können lediglich Daten auf ein oder von einem Speicherkonto übertragen werden. Anders ausgedrückt: Ein einzelner Import/Export-Auftrag kann nicht mehrere Speicherkonten umfassen. Weitere Informationen zum Erstellen eines neuen Speicherkontos finden Sie unter [Erstellen eines Speicherkontos](storage-create-storage-account.md#create-a-storage-account).

### <a name="blob-types"></a>Blobtypen
Sie können den Import/Export-Dienst von Azure verwenden, um Daten in **Blockblobs** oder **Seitenblobs** zu kopieren. Umgekehrt können Sie mit diesem Dienst nur **Blockblobs**, **Seitenblobs** oder **Anfügeblobs** aus dem Azure-Speicher exportieren.

### <a name="job"></a>Auftrag
Zunächst erstellen Sie einen Auftrag, um mit dem Importieren oder Exportieren von Blob-Speicher zu beginnen. Ein Auftrag kann ein Importauftrag oder ein Exportauftrag sein:

* Erstellen Sie einen Importauftrag, wenn Sie lokal gespeicherte Daten auf Blobs in Ihrem Azure-Speicherkonto übertragen möchten.
* Erstellen Sie einen Exportauftrag, wenn Sie Daten, die aktuell als Blobs in Ihrem Speicherkonto gespeichert sind, auf Festplatten übertragen möchten, die dann an Sie geschickt werden. Wenn Sie einen Auftrag erstellen, benachrichtigen Sie den Import-/Export-Dienst, dass Sie eine oder mehrere Festplatten an ein Azure-Rechenzentrum schicken werden.

* Bei einem Importauftrag verschicken Sie Festplatten, die Ihre Daten enthalten.
* Bei einem Exportauftrag verschicken Sie leere Festplatten.
* Sie können bis zu 10 Festplatten pro Auftrag schicken.

Sie können einen Import- oder Exportauftrag erstellen, indem Sie das Azure-Portal oder die [Azure Storage Import/Export-REST-API](/rest/api/storageimportexport) verwenden.

### <a name="waimportexport-tool"></a>WAImportExport-Tool
Der erste Schritt beim Erstellen eines **Import**-Auftrags ist das Vorbereiten der Laufwerke, die für Importzwecke verschickt werden sollen. Zum Vorbereiten der Laufwerke müssen Sie sie mit einem lokalen Server verbinden und darauf das WAImportExport-Tool ausführen. Mit diesem WAImportExport-Tool können Sie Ihre Daten auf das Laufwerk kopieren, die Daten auf dem Laufwerk mit BitLocker verschlüsseln und die Laufwerkjournaldateien generieren.

In den Journaldateien werden grundlegende Informationen zum Auftrag und Laufwerk gespeichert, z.B. die Seriennummer des Laufwerks und der Speicherkontoname. Diese Journaldatei wird nicht auf dem Laufwerk gespeichert. Sie wird beim Erstellen des Importauftrags verwendet. Eine Schritt-für-Schritt-Anleitung zum Erstellen eines Auftrags folgt später in diesem Artikel.

Das WAImportExport-Tool ist nur mit dem 64-Bit-Windows-Betriebssystem kompatibel. Informationen zu den unterstützten Betriebssystemversionen finden Sie im Abschnitt [Betriebssystem](#operating-system) .

Laden Sie die neueste Version des [WAImportExport-Tools](http://download.microsoft.com/download/3/6/B/36BFF22A-91C3-4DFC-8717-7567D37D64C5/WAImportExportV2.zip) herunter. Weitere Informationen zur Verwendung des WAImportExport-Tools finden Sie unter [Using the WAImportExport Tool](storage-import-export-tool-how-to.md) (Verwenden des WAImportExport-Tools).

>[!NOTE]
>**Vorherige Version:** Sie können [Version WAImportExpot V1 des Tools herunterladen](http://download.microsoft.com/download/0/C/D/0CD6ABA7-024F-4202-91A0-CE2656DCE413/WaImportExportV1.zip) und das [WAImportExpot V1-Benutzerhandbuch](storage-import-export-tool-how-to-v1.md) zu Rate ziehen. Version WAImportExpot V1 des Tools bietet Unterstützung für **die Vorbereitung von Datenträgern, wenn die Daten bereits vorab auf den Datenträger geschrieben werden**. Sie müssen das WAImportExpot V1-Tool auch dann verwenden, wenn der einzige verfügbare Schlüssel der SAS-Schlüssel ist.

>

### <a name="hard-disk-drives"></a>Festplattenlaufwerke
Für den Import/Export-Dienst werden nur 2,5-Zoll-SSD-Laufwerke bzw. interne 2,5-Zoll- oder 3,5-Zoll-SATA II- oder III-HDDs unterstützt. Ein einzelner Import-/Exportauftrag kann maximal 10 HDDs/SSDs umfassen, und jede einzelne HDD/SSD kann eine beliebige Größe aufweisen. Eine große Anzahl von Laufwerken kann über mehrere Aufträge verteilt werden, und es gibt keine Beschränkung hinsichtlich der Anzahl der Aufträge, die erstellt werden können. 

Bei Importaufträgen wird nur das erste Datenvolume auf dem Laufwerk verarbeitet. Das Datenvolume muss mit NTFS formatiert sein.

> [!IMPORTANT]
> Externe Festplattenlaufwerke mit einem integrierten USB-Adapter werden von diesem Dienst nicht unterstützt. Außerdem kann der Datenträger im Gehäuse einer externen Festplatte nicht verwendet werden. Senden Sie bitte keine externen Festplatten ein.
> 
> 

Im Folgenden finden Sie eine Liste externer USB-Adapter, die zum Kopieren von Daten auf interne HDDs verwendet werden. Anker 68UPSATAA-02BU Anker 68UPSHHDS-BU Startech SATADOCK22U, Orico 6628SUS3-C-BK (Modellreihe 6628), Thermaltake BlacX Hot-Swap SATA-Dockingstation für externe Festplatte (USB 2.0 und eSATA)

### <a name="encryption"></a>Verschlüsselung
Die Daten auf dem Laufwerk müssen mit BitLocker-Laufwerkverschlüsselung verschlüsselt sein. So sind Ihre Daten beim Transport geschützt.

Für Importaufträge gibt es zwei Möglichkeiten zum Durchführen der Verschlüsselung. Die erste Möglichkeit besteht darin, die Option bei Verwendung der Dataset-CSV-Datei anzugeben, während das WAImportExport-Tool bei der Vorbereitung des Laufwerks ausgeführt wird. Die zweite Möglichkeit ist das manuelle Aktivieren der BitLocker-Verschlüsselung auf dem Laufwerk und das Angeben des Verschlüsselungsschlüssels in der Driveset-CSV bei der Ausführung der Befehlszeile des WAImportExport-Tools während der Laufwerksvorbereitung.

Bei Exportaufträgen wendet der Dienst nach dem Kopieren Ihrer Daten auf die Laufwerke jeweils die BitLocker-Verschlüsselung auf das Laufwerk an, bevor es an Sie zurückgeschickt wird. Der Verschlüsselungsschlüssel wird für Sie über das Azure Portal bereitgestellt.  

### <a name="operating-system"></a>Betriebssystem
Sie können eines der folgenden 64-Bit-Betriebssysteme zum Vorbereiten der Festplatte mit dem WAImportExport-Tool verwenden, bevor Sie die Festplatte an Azure schicken:

Windows 7 Enterprise, Windows 7 Ultimate, Windows 8 Pro, Windows 8 Enterprise, Windows 8.1 Pro, Windows 8.1 Enterprise, Windows 10<sup>1</sup>, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2. Alle diese Betriebssysteme unterstützen die BitLocker-Laufwerkverschlüsselung.

### <a name="locations"></a>Standorte
Der Azure Import/Export-Dienst unterstützt das Kopieren von Daten von bzw. in alle öffentlichen Azure-Speicherkonten. Sie können Festplatten an einen der unten angegebenen Standorte schicken. Falls sich Ihr Speicherkonto an einem öffentlichen Azure-Standort befindet, der hier nicht aufgeführt ist, wird ein anderer Zielstandort angegeben, wenn Sie den Auftrag mit dem Azure-Portal oder mit der Import/Export-REST-API erstellen.

Unterstützte Standorte für die Verschickung:

* USA, Osten
* USA (West)
* USA (Ost) 2
* USA, Westen 2
* USA (Mitte)
* USA Nord Mitte
* USA (Mitte/Süden)
* USA, Westen-Mitte
* Nordeuropa
* Europa, Westen
* Asien, Osten
* Asien, Südosten
* Australien, Osten
* Australien, Südosten
* Japan, Westen
* Japan, Osten
* Indien (Mitte)
* Indien (Süden)
* Indien, Westen
* Kanada, Mitte
* Kanada, Osten
* Brasilien Süd
* Korea, Mitte
* US Government, Virginia
* US Government, Iowa
* US DoD, Osten
* US DoD, Mitte
* China, Osten
* China, Norden
* UK, Süden

### <a name="shipping"></a>Versand
**Schicken von Festplatten an das Rechenzentrum:**

Beim Erstellen eines Import- oder Exportauftrags erhalten Sie die Versandadresse eines unterstützten Standorts, an den Sie die Festplatten schicken können. Die angegebene Adresse richtet sich nach dem Standort Ihres Speicherkontos, aber sie muss nicht unbedingt dem Standort des Speicherkontos entsprechen.

Sie können Kurierdienste wie FedEx, DHL, UPS oder US Postal Service nutzen, um Festplatten an die Versandadresse zu schicken.

**Schicken von Festplatten aus dem Rechenzentrum:**

Beim Erstellen eines Import- oder Exportauftrags müssen Sie eine Rücksendeadresse angeben, an die Festplatten nach Abschluss des Auftrags von Microsoft zurückgeschickt werden können. Achten Sie darauf, dass Sie eine gültige Rücksendeadresse angeben, um Verzögerungen bei der Verarbeitung zu vermeiden.

Sie können einen Kurierdienst Ihrer Wahl verwenden, um die Festplatte zu senden. Der Kurierdienst muss eine angemessene Nachverfolgung bieten, um die Überwachungskette zu gewährleisten. Sie müssen auch eine gültige Nummer Ihres Kontos bei FedEx oder DHL angeben, die von Microsoft zum Zurücksenden der Laufwerke verwendet werden kann. Eine FedEx-Kontonummer wird benötigt, um den Rückversand für Festplatten von Standorten in den USA und Europa durchzuführen. Eine DHL-Kontonummer wird benötigt, um den Rückversand für Festplatten von Standorten in Asien und Australien durchzuführen. Sie können ein Konto bei [FedEx](http://www.fedex.com/us/oadr/) (für USA und Europa) oder [DHL](http://www.dhl.com/) (Asien und Australien) erstellen, falls Sie noch nicht über ein solches Konto verfügen. Wenn Sie bereits über die Nummer eines Kontos bei einem Kurierdienst verfügen, sollten Sie überprüfen, ob sie gültig ist.

Beim Versand Ihrer Pakete müssen Sie die Nutzungsbedingungen unter [Microsoft Azure-Nutzungsbedingungen](https://azure.microsoft.com/support/legal/services-terms/)beachten.

> [!IMPORTANT]
> Beachten Sie, dass die physischen Medien beim Versand unter Umständen Ländergrenzen überqueren. Sie müssen sicherstellen, dass Ihre physischen Medien und Daten gemäß geltender Gesetze importiert bzw. exportiert werden. Prüfen Sie vor dem Versand der physischen Medien mit Ihren Rechtsberatern, ob Medien und Daten laut Gesetz an das entsprechende Rechenzentrum verschickt werden dürfen. So stellen Sie sicher, dass Ihre Daten zeitnah bei Microsoft eintreffen. Z.B. muss allen Paketen, die Ländergrenzen überschreiten (Grenzüberschreitungen innerhalb der Europäischen Union ausgenommen), eine Handelsrechnung beiliegen. Sie könnten eine ausgefüllte Kopie der Handelsrechnung von der Website des Frachtführers ausdrucken. Beispiele für Handelsrechnungen sind die [DHL-Handelsrechnung](http://invoice-template.com/wp-content/uploads/dhl-commercial-invoice-template.pdf) und die [FedEx-Handelsrechnung](http://images.fedex.com/downloads/shared/shipdocuments/blankforms/commercialinvoice.pdf). Stellen Sie sicher, dass Microsoft nicht als Exporteur angegeben wurde.
> 
> 

## <a name="how-does-the-azure-importexport-service-work"></a>Wie funktioniert der Azure Import/Export-Dienst?
Sie können Daten zwischen Ihrem lokalen Standort und Azure Blob Storage übertragen, indem Sie den Azure Import/Export-Dienst nutzen. Sie erstellen Aufträge und versenden Festplattenlaufwerke an ein Azure-Rechenzentrum. Jedes Festplattenlaufwerk, das Sie verschicken, ist einem einzelnen Auftrag zugeordnet. Jeder Auftrag ist einem einzelnen Speicherkonto zugeordnet. Lesen Sie sich den [Abschnitt zu den Voraussetzungen](#pre-requisites) sorgfältig durch, um sich über die Besonderheiten dieses Diensts zu informieren, z.B. unterstützte Blobtypen, Datenträgertypen, Standorte und Versand.

In diesem Abschnitt werden die allgemeinen Schritte beschrieben, die für Import- und Exportaufträge ausgeführt werden müssen. Weiter unten im [Abschnitt „Schnellstart“](#quick-start)finden Sie eine Schrittanleitung zum Erstellen eines Import- und Exportauftrags.

### <a name="inside-an-import-job"></a>Importaufträge
Ein Importauftrag umfasst im Allgemeinen die folgenden Schritte:

* Bestimmen Sie die zu importierenden Daten und die Anzahl der benötigten Festplatten.
* Geben Sie die Zielblobspeicherorte für Ihre Daten in Blob Storage an.
* Verwenden Sie das the WAImportExport-Tool, um die Daten auf eine oder mehrere Festplatten zu kopieren und per BitLocker zu verschlüsseln.
* Erstellen Sie einen Importauftrag in Ihrem Zielspeicherkonto mit dem Azure-Portal oder der Import/Export-REST-API. Laden Sie bei Verwendung des Azure-Portals die Laufwerkjournaldateien hoch.
* Geben Sie die Rücksendeadresse und die Nummer Ihres Kontos beim Kurierdienst an, die für das Zurücksenden der Festplatten verwendet werden können.
* Schicken Sie die Festplatten an die Versandadresse, die Sie während der Erstellung des Auftrags erhalten haben.
* Aktualisieren Sie die Nummer für die Sendungsverfolgung in den Details des Importauftrags, und senden Sie den Importauftrag ab.
* Die Festplatten kommen im Azure-Rechenzentrum an und werden verarbeitet.
* Die Festplatten werden über Ihr Kurierdienstkonto an die Rücksendeadresse geschickt, die im Importauftrag angegeben ist.
  
    ![Abbildung 1: Ablauf eines Importauftrags](./media/storage-import-export-service/importjob.png)

### <a name="inside-an-export-job"></a>Exportaufträge
Ein Exportauftrag umfasst im Allgemeinen die folgenden Schritte:

* Bestimmen Sie die zu exportierenden Daten und die Anzahl der benötigten Festplatten.
* Identifizieren Sie die Quellblobs oder Containerpfade Ihrer Daten in Blob Storage.
* Erstellen Sie einen Exportauftrag in Ihrem Quellspeicherkonto mit dem Azure-Portal oder der Import/Export-REST-API.
* Geben Sie die Quellblobs oder Containerpfade Ihrer Daten im Exportauftrag an.
* Geben Sie die Rücksendeadresse und die Nummer Ihres Kontos beim Kurierdienst an, die für das Zurücksenden der Festplatten verwendet werden können.
* Schicken Sie die Festplatten an die Versandadresse, die Sie während der Erstellung des Auftrags erhalten haben.
* Aktualisieren Sie die Nummer für die Sendungsverfolgung in den Details des Exportauftrags, und senden Sie den Exportauftrag ab.
* Die Festplatten kommen im Azure-Rechenzentrum an und werden verarbeitet.
* Die Festplatten werden mit BitLocker verschlüsselt. Die Schlüssel sind über das Azure-Portal verfügbar.  
* Die Festplatten werden über Ihr Kurierdienstkonto an die Rücksendeadresse zurückgeschickt, die im Importauftrag angegeben ist.
  
    ![Abbildung 2: Ablauf eines Exportauftrags](./media/storage-import-export-service/exportjob.png)

### <a name="viewing-your-job-and-drive-status"></a>Anzeigen von Auftrags- und Laufwerksstatus
Sie können den Status Ihrer Import- oder Exportaufträge im Azure-Portal nachverfolgen. Klicken Sie auf die Registerkarte **Import/Export**. Eine Liste Ihrer Aufträge wird auf der Seite angezeigt.

![Anzeigen des Auftragsstatus](./media/storage-import-export-service/jobstate.png)

Je nachdem, an welchem Punkt des Prozesses sich Ihre Festplatte befindet, wird einer der folgenden Auftragsstatus angezeigt.

| Auftragsstatus | Beschreibung |
|:--- |:--- |
| Wird erstellt | Nachdem ein Auftrag erstellt wurde, ist sein Status auf „Wird erstellt“ festgelegt. Während der Auftrag sich im Status „Wird erstellt“ befindet, geht der Import/Export-Dienst davon aus, dass die Laufwerke noch nicht an das Rechenzentrum versendet wurden. Ein Auftrag verbleibt bis zu zwei Wochen im Status „Wird erstellt“. Anschließend wird er automatisch vom Dienst gelöscht. |
| Versand | Nachdem Sie Ihr Paket gesendet haben, sollten Sie die Nachverfolgungsinformationen im Azure-Portal aktualisieren.  So erhält der Auftrag den Status „Wird versendet“. Der Auftrag verbleibt bis zu zwei Wochen im Status „Wird versendet“. 
| Empfangen | Nachdem das Rechenzentrum alle Laufwerke empfangen hat, wird der Status des Auftrags auf „Empfangen“ gesetzt. |
| Übertragung wird ausgeführt | Nachdem die Verarbeitung mindestens eines Laufwerks begonnen hat, wird der Status des Auftrags auf „Übertragung wird ausgeführt“ festgelegt. Ausführliche Informationen finden Sie im Abschnitt „Laufwerkstatus“. |
| Verpackung | Nachdem die Verarbeitung aller Laufwerke abgeschlossen ist, wird der Auftrag in den Status „Verpackung“ gesetzt, bis die Laufwerke an den Kunden zurückgesendet werden. |
| Abgeschlossen | Nachdem alle Laufwerke an den Kunden zurückgesendet worden sind, und der Auftrag ohne Fehler abgeschlossen wurde, wird der Auftrag in den Status „Abgeschlossen“ gesetzt. Nach 90 Tagen im Status „Abgeschlossen“ wird der Auftrag automatisch gelöscht. |
| Geschlossen | Nachdem alle Laufwerke an den Kunden zurückgesendet worden sind, und während der Verarbeitung des Auftrags Fehler aufgetreten sind, wird der Auftrag in den Status „Geschlossen“ gesetzt. Nach 90 Tagen im Status „Geschlossen“ wird der Auftrag automatisch gelöscht. |

In der folgenden Tabelle wird der Lebenszyklus eines einzelnen Laufwerks mit seinen Übergängen in einen Import- oder Exportauftrag beschrieben. Der aktuelle Status der einzelnen Laufwerke in einem Auftrag wird jetzt im Azure-Portal angezeigt.
Die folgende Tabelle beschreibt die einzelnen Status, die jedes Laufwerk in einem Auftrag durchlaufen kann.

| Laufwerkstatus | Beschreibung |
|:--- |:--- |
| Angegeben | Für einen Importauftrag, der im Azure-Portal erstellt wurde, befindet sich ein Laufwerk anfänglich im Status „Angegeben“. Für einen Exportauftrag befindet sich ein Laufwerk anfänglich im Status „Empfangen“, da beim Erstellen des Auftrags kein Laufwerk angegeben wird. |
| Empfangen | Das Laufwerk wechselt in den Status „Empfangen“, wenn der Import/Export-Dienstanbieter die Laufwerke verarbeitet hat, die vom Transportunternehmen für einen Importauftrag empfangen wurden. Für einen Exportauftrag ist der anfängliche Laufwerkstatus der Status „Empfangen“. |
| NeverReceived | Das Laufwerk wechselt in den Status „NeverReceived“, wenn das Paket für einen Auftrag eintrifft, aber kein Laufwerk enthält. Ein Laufwerk kann auch in diesen Status verschoben werden, wenn zwei Wochen verstrichen sind, seitdem der Dienst die Versandinformationen erhalten hat, aber das Paket noch nicht im Rechenzentrum eingetroffen ist. |
| Übertragung wird ausgeführt | Ein Laufwerk wechselt in den Status „Übertragung wird ausgeführt“, wenn der Dienst beginnt, Daten aus dem Laufwerk in Microsoft Azure Storage zu übertragen. |
| Abgeschlossen | Ein Laufwerk wechselt in den Status „Abgeschlossen“, wenn der Dienst alle Daten ohne Fehler erfolgreich übertragen hat.
| CompletedMoreInfo | Ein Laufwerk wechselt in den Status „CompletedMoreInfo“, wenn der Dienst beim Kopieren von Daten entweder von dem Laufwerk oder auf das Laufwerk auf Probleme gestoßen ist. Die Informationen können Fehler, Warnungen oder Informationsmeldungen zum Überschreiben von Blobs umfassen.
| ShippedBack | Das Laufwerk wechselt in den Status „ShippedBack“, wenn es vom Rechenzentrum an die Rücksendeadresse zurückgesendet wurde. |

Dieser Screenshot des Azure-Portals zeigt den Laufwerkstatus in einem Beispielauftrag:

![Anzeigen des Laufwerkstatus](./media/storage-import-export-service/drivestate.png)

Die folgende Tabelle beschreibt die Laufwerkfehlerstatus und die Aktionen, die für jeden Status durchgeführt werden.

| Laufwerkstatus | Ereignis | Lösung / Nächster Schritt |
|:--- |:--- |:--- |
| NeverReceived | Ein Laufwerk mit der Kennzeichnung „NeverReceived“ (da es im Rahmen des Versands des Auftrags nicht eingegangen ist) geht in einer anderen Sendung ein. | Das Betriebsteam versetzt das Laufwerk in den Status „Empfangen“. |
| N/V | Ein Laufwerk, das nicht Teil eines Auftrags ist, trifft als Teil eines anderen Auftrags im Rechenzentrum ein. | Das Laufwerk wird als zusätzliches Laufwerk gekennzeichnet und nach Abschluss des Auftrags, der dem Originalpaket zugeordnet ist, an den Kunden zurückgegeben. |

### <a name="time-to-process-job"></a>Verarbeitungsdauer des Auftrags
Die Verarbeitungsdauer eines Import- oder Exportauftrags kann aufgrund von verschiedenen Faktoren variieren, z.B. Versanddauer, Auftragstyp, Typ und Größe der zu kopierenden Daten und Größe der geschickten Festplatten. Der Import/Export-Dienst verfügt nicht über eine SLA. Nach Erhalt der Datenträger versucht der Dienst jedoch, die Kopie in sieben bis zehn Tagen fertigzustellen. Sie können die REST-API verwenden, um den Auftragsstatus genauer verfolgen zu können. Der Vorgang zum Auflisten von Aufträgen enthält einen Parameter für die prozentuale Fertigstellung, mit dem Sie den Status des Kopiervorgangs verfolgen können. Sie können sich gern an uns wenden, falls Sie für einen zeitkritischen Import- oder Exportauftrag einen geschätzten Fertigstellungszeitpunkt benötigen.

### <a name="pricing"></a>Preise
**Bearbeitungsgebühr für Festplatte**

Für jede Festplatte, die im Rahmen eines Import- oder Exportauftrags verarbeitet wird, wird eine Bearbeitungsgebühr berechnet. Informationen hierzu finden Sie unter [Preise für Import/Export](https://azure.microsoft.com/pricing/details/storage-import-export/).

**Versandkosten**

Wenn Sie Festplatten an Azure schicken, übernehmen Sie die Versandkosten des Kurierdiensts. Wenn die Festplatten von Microsoft an Sie zurückgeschickt werden, werden die Versandkosten Ihrem Kurierdienstkonto belastet, das Sie bei der Erstellung des Auftrags angegeben haben.

**Transaktionskosten**

Für das Importieren von Daten in Blob Storage fallen keine Transaktionskosten an. Die standardmäßigen Gebühren für ausgehende Übertragungen fallen an, wenn Daten aus Blob Storage exportiert werden. Weitere Informationen zu Transaktionskosten finden Sie unter [Datenübertragungen – Preisdetails](https://azure.microsoft.com/pricing/details/data-transfers/)

## <a name="quick-start"></a>Schnellstart
Dieser Abschnitt enthält eine Schrittanleitung zum Erstellen eines Import- und Exportauftrags. Stellen Sie sicher, dass alle [Voraussetzungen](#pre-requisites) erfüllt sind, bevor Sie fortfahren.

> [!IMPORTANT]
> Der Dienst unterstützt ein Standardspeicherkonto pro Import- oder Exportauftrag und unterstützt keine Storage Premium-Konten. 
> 
> 
## <a name="create-an-import-job"></a>Erstellen eines Importauftrags
Erstellen Sie einen Importauftrag zum Kopieren von Daten von Festplatten in Ihr Azure-Speicherkonto, indem Sie eine oder mehrere Festplatten mit Daten an das angegebene Rechenzentrum schicken. Im Rahmen des Importauftrags werden Details zu Festplattenlaufwerken, die zu kopierenden Daten, das Zielspeicherkonto und Versandinformationen an den Azure Import/Export-Dienst übermittelt. Die Erstellung eines Importauftrags ist ein dreistufiger Prozess. Bereiten Sie zuerst die Laufwerke mit dem WAImportExport-Tool vor. Senden Sie anschließend einen Importauftrag über das Azure-Portal. Der dritte Schritt umfasst das Schicken der Festplatten an die Versandadresse, die bei der Erstellung des Auftrags angegeben wurde, und das Aktualisieren der Versandinformationen in den Auftragsdetails.   

### <a name="prepare-your-drives"></a>Vorbereiten Ihrer Laufwerke
Der erste Schritt beim Importieren von Daten mit dem Azure Import/Export-Dienst ist das Vorbereiten Ihrer Festplatten mit dem WAImportExport-Tool. Führen Sie die folgenden Schritte aus, um die Festplatten vorzubereiten:

1. Identifizieren Sie die zu importierenden Daten. Hierbei kann es sich um Verzeichnisse und eigenständige Dateien auf dem lokalen Server oder einer Netzwerkfreigabe handeln.  
2. Bestimmen Sie anhand der Gesamtgröße der Daten die Anzahl von benötigten Festplatten. Beschaffen Sie die erforderliche Anzahl von 2,5-Zoll-SSD-Laufwerken oder 2,5-Zoll- bzw. 3,5-Zoll-SATA II- oder -III-Festplatten.
3. Identifizieren Sie das Zielspeicherkonto, den Container, die virtuellen Verzeichnisse und die Blobs.
4.  Bestimmen Sie die Verzeichnisse bzw. eigenständigen Dateien, die auf jede Festplatte kopiert werden sollen.
5.  Erstellen Sie die CSV-Dateien für das Dataset und Driveset.
    
    **Dataset-CSV-Datei**
    
    Im Folgenden sehen Sie ein Beispiel für eine Dataset CSV-Datei:
    
    ```
    BasePath,DstBlobPathOrPrefix,BlobType,Disposition,MetadataFile,PropertiesFile
    "F:\50M_original\100M_1.csv.txt","containername/100M_1.csv.txt",BlockBlob,rename,"None",None
    "F:\50M_original\","containername/",BlockBlob,rename,"None",None 
    ```
   
    Im Beispiel oben wird 100M_1.csv.txt in das Stammverzeichnis des Containers mit dem Namen „Containername“ kopiert. Wenn der Container „Containername“ nicht vorhanden ist, wird er erstellt. Alle Dateien und Ordner in 50M_original werden rekursiv in „Containername“ kopiert. Die Ordnerstruktur wird beibehalten.

    Erfahren Sie mehr über das [Vorbereiten der Dataset-CSV-Datei](storage-import-export-tool-preparing-hard-drives-import.md#prepare-the-dataset-csv-file).
    
    **Beachten Sie Folgendes:** Die Daten werden standardmäßig als Blockblobs importiert. Sie können den Feldwert „BlobType“ verwenden, um Daten als Seitenblobs zu importieren. Wenn Sie beispielsweise VHD-Dateien importieren, die als Datenträger auf einer Azure VM bereitgestellt werden, müssen Sie sie als Seitenblobs importieren.

    **Driveset-CSV-Datei**

    Der Wert des Driveset-Flags ist eine CSV-Datei, die die Liste der Datenträger enthält, denen die Laufwerkbuchstaben zugeordnet sind, damit das Tool richtig die Liste der vorzubereitenden Datenträger auswählt. 

    Im Folgenden finden Sie ein Beispiel der Driveset CSV-Datei:
    
    ```
    DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
    G,AlreadyFormatted,SilentMode,AlreadyEncrypted,060456-014509-132033-080300-252615-584177-672089-411631 |
    H,Format,SilentMode,Encrypt,
    ```

    Im obigen Beispiel wird davon ausgegangen, dass zwei Festplatten angeschlossen sind, und grundlegende NTFS-Volumes mit Volumebuchstaben G:\ und H:\ erstellt wurden. Das Tool formatiert und verschlüsselt den Datenträger, der H:\ hostet, und formatiert und verschlüsselt nicht den Datenträger, der Volume G:\ hostet.

    Erfahren Sie mehr über das [Vorbereiten der Driveset-CSV-Datei](storage-import-export-tool-preparing-hard-drives-import.md#prepare-initialdriveset-or-additionaldriveset-csv-file).

6.  Kopieren Sie Ihre Daten mithilfe des [WAImportExport-Tools](http://download.microsoft.com/download/3/6/B/36BFF22A-91C3-4DFC-8717-7567D37D64C5/WAImportExport.zip) auf eine oder mehrere Festplatten.
7.  Sie können „Encrypt“ im Feld „Encryption“ der Driveset-CSV-Datei angeben, um für die Festplatte die BitLocker-Verschlüsselung zu aktivieren. Alternativ dazu können Sie die BitLocker-Verschlüsselung auf der Festplatte auch manuell aktivieren, „AlreadyEncrypted“ angeben und den Schlüssel beim Ausführen des Tools in der Driveset-CSV-Datei bereitstellen.

8. Ändern Sie die Daten auf den Festplatten oder die Journaldatei nicht mehr, nachdem Sie die Festplattenvorbereitung abgeschlossen haben.

> [!IMPORTANT]
> Für jede vorbereitete Festplatte ergibt sich eine Journaldatei. Wenn Sie den Importauftrag mit dem Azure-Portal erstellen, müssen Sie alle Journaldateien der Festplatten hochladen, die Teil des Importauftrags sind. Festplatten ohne Journaldateien werden nicht verarbeitet.
> 
>

Unten sind die Befehle und Beispiele zum Vorbereiten der Festplatte mit dem WAImportExport-Tool aufgeführt.

Der PrepImport-Befehl des WAImportExport-Tools für die erste Kopiersitzung zum Kopieren von Verzeichnissen und/oder Dateien mit einer neuen Kopiersitzung:

```
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] [/sk:<StorageAccountKey>] [/silentmode] [/InitialDriveSet:<driveset.csv>] DataSet:<dataset.csv>
```

**Importbeispiel 1**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1  /sk:************* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

Um **weitere Laufwerke hinzufügen**, können Sie eine neue Driveset-Datei erstellen und den Befehl wie unten beschrieben ausführen. Geben Sie für nachfolgende Kopiersitzungen mit anderen Laufwerken als den in der InitialDriveset-CSV-Datei angegebenen eine neue Driveset-CSV-Datei an, und geben Sie sie als Wert für den Parameter AdditionalDriveSet an. Verwenden Sie den **gleichen Journaldateinamen**, und geben Sie eine **neue Sitzungs-ID** an. Das Format der AdditionalDriveset-CSV-Datei ist mit dem InitialDriveSet-Format identisch.

```
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> /AdditionalDriveSet:<driveset.csv>
```

**Importbeispiel 2**
```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3  /AdditionalDriveSet:driveset-2.csv
```

Um dem gleichen Driveset zusätzliche Daten hinzuzufügen, kann der PrepImport-Befehl des WAImportExport-Tools für nachfolgende Kopiersitzungen zum Kopieren zusätzlicher Dateien/Verzeichnisse aufgerufen werden: Geben Sie für nachfolgende Kopiersitzungen zum Kopieren auf die gleichen Festplattenlaufwerke in der InitialDriveset-CSV-Datei den **gleichen Journaldateinamen** an, und geben Sie eine **neue Sitzungs-ID** an; den Speicherkontoschlüssel müssen Sie nicht bereitstellen.

```
WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] DataSet:<dataset.csv>
```

**Importbeispiel 3**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /DataSet:dataset-2.csv
```

Weitere Details zur Verwendung des WAImportExport-Tools finden Sie unter [Vorbereiten von Festplatten für einen Importauftrag](storage-import-export-tool-preparing-hard-drives-import.md).

Ausführlichere Schrittanleitungen finden Sie unter [Beispielworkflow für die Vorbereitung von Festplatten für einen Importauftrag](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow.md).  

### <a name="create-the-import-job"></a>Erstellen des Importauftrags
1. Sobald Sie Ihr Laufwerk vorbereitet haben, navigieren Sie zum Speicherkonto im Azure-Portal und zeigen das Dashboard an. Klicken Sie unter **Auf einen Blick** auf **Importauftrag erstellen**. Sehen Sie sich die Schritte an, und aktivieren Sie das Kontrollkästchen, um anzugeben, dass Sie die Festplatte vorbereitet haben und die Laufwerkjournaldatei verfügbar ist.
2. Geben Sie im ersten Schritt die Kontaktinformationen des Ansprechpartners für diesen Importauftrag und eine gültige Rücksendeadresse an. Wenn Sie ausführliche Protokolldaten für den Importauftrag speichern möchten, wählen Sie die Option **Save the verbose log in my 'waimportexport' 	blob container**.
3. Laden Sie im zweiten Schritt die Protokolldateien hoch, die Sie während der Vorbereitung des Laufwerks erhalten haben. Sie müssen pro vorbereitetem Laufwerk eine Datei hochladen.
   
   ![Importauftrag erstellen – Schritt 3](./media/storage-import-export-service/import-job-03.png)
4. Geben Sie im dritten Schritt einen beschreibenden Namen für den Importauftrag ein. Beachten Sie, dass der eingegebene Name nur Kleinbuchstaben, Ziffern, Trennstriche und Unterstriche enthalten darf, mit einem Buchstaben beginnen muss und keine Leerzeichen enthalten darf. Mit dem ausgewählten Namen können Sie Ihre Aufträge während und nach der Bearbeitung nachverfolgen.
   
   Wählen Sie anschließend Ihr Rechenzentrum aus der Liste aus. Unter Rechenzentrumsregion wird das Rechenzentrum und die Adresse angegeben, an die Sie Ihr Paket schicken müssen. Weitere Informationen finden Sie unten in den FAQ.
5. Wählen Sie in Schritt 4 Ihren Kurierdienst für die Rücksendung aus, und geben Sie Ihr Kurierdienst-Kundenkonto an. Microsoft verwendet diese Kontodaten, um die Laufwerke nach Abschluss des Importauftrags an Sie zurückzuschicken.
   
   Falls Sie Ihre Nachverfolgungsnummer haben, können Sie Ihren Kurierdienst in der Liste auswählen und die Nummer eingeben.
   
   Wenn Sie noch keine Tracking-Nummer haben, wählen Sie **I will provide my shipping information for this import job once I have shipped my package**, und schließen Sie den Importprozess ab.
6. Um Ihre Nachverfolgungsnummer einzugeben, nachdem Sie Ihr Paket verschickt haben, kehren Sie zur Seite **Import/Export** für Ihr Speicherkonto im Azure-Portal zurück, wählen Ihren Auftrag in der Liste aus und klicken auf **Versandinformationen**. Navigieren Sie durch den Assistenten und geben Sie Ihre Nachverfolgungsnummer in Schritt 2 ein.
   
    Wenn die Nachverfolgungsnummer nicht innerhalb von zwei Wochen nach Erstellung des Auftrags aktualisiert wird, läuft der Auftrag ab.
   
    Falls der Auftrag den Status "Erstellung", "Versand" oder "Übertragung" hat, können Sie außerdem Ihre Kontonummer beim Transportunternehmen in Schritt 2 des Assistenten eingeben. Falls der Auftrag den Status "Verpackung" hat, können Sie Ihre Kontonummer beim Transportunternehmen nicht mehr ändern.
7. Sie können den Status des Auftrags im Dashboard des Portals nachverfolgen. Unter [Anzeigen des Auftragsstatus](#viewing-your-job-status)können Sie ermitteln, was ein Auftragsstatus des vorherigen Abschnitts jeweils bedeutet.

## <a name="create-an-export-job"></a>Erstellen eines Exportauftrags
Erstellen Sie einen Exportauftrag, um den Import/Export-Dienst darüber zu informieren, dass Sie ein oder mehrere leere Laufwerke an das Rechenzentrum schicken. So können die Daten von Ihrem Speicherkonto auf die Laufwerke exportiert werden, und die Laufwerke werden Ihnen dann zugeschickt.

### <a name="prepare-your-drives"></a>Vorbereiten Ihrer Laufwerke
Zur Vorbereitung von Laufwerken für einen Exportauftrag werden folgende Vorabprüfungen empfohlen:

1. Überprüfen Sie die Anzahl der erforderlichen Datenträger mithilfe des PreviewExport-Befehls aus dem WAImportExport-Tool. Weitere Informationen finden Sie unter [Vorschau der Laufwerknutzung für einen Exportauftrag](https://msdn.microsoft.com/library/azure/dn722414.aspx). Sie können damit eine Vorschau der Festplattenverwendung für Blobs anzeigen, die Sie ausgewählt haben, basierend auf der Größe der Laufwerke, die Sie verwenden möchten.
2. Überprüfen Sie, ob Sie Lese-/Schreibzugriff auf die Festplatte haben, die für den Exportauftrag verschickt werden soll.

### <a name="create-the-export-job"></a>Erstellen des Exportauftrags
1. Navigieren Sie zum Speicherkonto im Azure-Portal, und zeigen Sie das Dashboard an, um einen Exportauftrag zu erstellen. Klicken Sie unter **Auf einen Blick** auf **Exportauftrag erstellen**, und fahren Sie mit dem Assistenten fort.
2. Geben Sie im zweiten Schritt die Kontaktinformationen des Ansprechpartners für diesen Exportauftrag an. Wenn Sie ausführliche Protokolldaten für den Exportauftrag speichern möchten, wählen Sie die Option **Save the verbose log in my 'waimportexport' 	blob container**.
3. Legen Sie im dritten Schritt fest, welche Blob-Daten Sie von Ihrem Speicherkonto auf Ihr leeres Laufwerk oder Ihre Laufwerke exportieren möchten. Sie können alle Blob-Daten des Speicherkontos exportieren, oder Sie legen fest, welche Blobs oder Blob-Sätze exportiert werden sollen.
   
   Verwenden Sie die Auswahl **Equal To** , und geben Sie den relativen Pfad zu dem Blob an, beginnend mit dem Containernamen, um den zu exportierenden Blob festzulegen. Verwenden Sie *$root* , um den Stammcontainer festzulegen.
   
   Verwenden Sie die Auswahl **Starts With** , und legen Sie das Präfix beginnend mit einem Schrägstrich "/" fest, um alle Blobs festzulegen, die mit einem Präfix beginnen. Bei dem Präfix kann es sich um das Präfix des Containernamens, den vollständigen Containernamen oder den vollständigen Containernamen gefolgt vom Präfix des Blob-Namens handeln.
   
   Die folgende Tabelle enthält Beispiele für gültige Blob-Pfade:
   
   | Auswahl | Blob-Pfad | Beschreibung |
   | --- | --- | --- |
   | Starts With |/ |Exportiert alle Blobs im Speicherkonto |
   | Starts With |/$root/ |Exportiert alle Blobs im Stammcontainer |
   | Starts With |/book |Exportiert alle Blobs in allen Containern mit dem Präfix **book** |
   | Starts With |/music/ |Exportiert alle Blobs im Container **music** |
   | Starts With |/music/love |Exportiert alle Blobs im Container **music**, die mit dem Präfix **love** beginnen |
   | Equal To |$root/logo.bmp |Exportiert das Blob **logo.bmp** im Stammcontainer |
   | Equal To |videos/story.mp4 |Exportiert das Blob **story.mp4** im Container **videos** |
   
   Sie müssen die Blob-Pfade in gültigen Formaten angeben, um Fehler während der Verarbeitung zu vermeiden. Dies ist in diesem Screenshot dargestellt.
   
   ![Exportauftrag erstellen – Schritt 3](./media/storage-import-export-service/export-job-03.png)
4. Geben Sie im vierten Schritt einen beschreibenden Namen für den Exportauftrag ein. Der eingegebene Name darf nur Kleinbuchstaben, Ziffern, Trennstriche und Unterstriche enthalten, muss mit einem Buchstaben beginnen und darf keine Leerzeichen enthalten.
   
   Die Datacenter-Region gibt das Datacenter an, an das Sie Ihr Paket schicken müssen. Weitere Informationen finden Sie unten in den FAQ.
5. Wählen Sie in Schritt 5 Ihren Rücktransporteur aus und geben Sie Ihr Kundenkonto beim Transportunternehmen an. Microsoft verwendet diese Kontodaten, um Ihre Laufwerke nach Abschluss des Exportauftrags an Sie zurückzuschicken.
   
   Falls Sie Ihre Nachverfolgungsnummer haben, können Sie Ihren Kurierdienst in der Liste auswählen und die Nummer eingeben.
   
   Falls Sie noch keine Nachverfolgungsnummer haben, wählen Sie die Option **Ich werde die Versanddaten für diesen Exportauftrag angeben, sobald ich mein Paket verschickt habe**aus und schließen Sie den Exportvorgang ab.
6. Um Ihre Nachverfolgungsnummer einzugeben, nachdem Sie Ihr Paket verschickt haben, kehren Sie zur Seite **Import/Export** für Ihr Speicherkonto im Azure-Portal zurück, wählen Ihren Auftrag in der Liste aus und klicken auf **Versandinformationen**. Navigieren Sie durch den Assistenten und geben Sie Ihre Nachverfolgungsnummer in Schritt 2 ein.
   
    Wenn die Nachverfolgungsnummer nicht innerhalb von zwei Wochen nach Erstellung des Auftrags aktualisiert wird, läuft der Auftrag ab.
   
    Falls der Auftrag den Status "Erstellung", "Versand" oder "Übertragung" hat, können Sie außerdem Ihre Kontonummer beim Transportunternehmen in Schritt 2 des Assistenten eingeben. Falls der Auftrag den Status "Verpackung" hat, können Sie Ihre Kontonummer beim Transportunternehmen nicht mehr ändern.
   
   > [!NOTE]
   > Wenn das zu exportierende Blob während des Kopierens auf die Festplatte verwendet wird, erstellt der Azure Import/Export-Dienst eine Momentaufnahme des Blobs und kopiert die Momentaufnahme.
   > 
   > 
7. Sie können den Auftragsstatus im Dashboard im Azure-Portal nachverfolgen. Im vorherigen Abschnitt unter „Anzeigen des Auftragsstatus“ wird beschrieben, was ein Auftragsstatus jeweils bedeutet.
8. Nachdem Sie die Laufwerke mit den exportierten Daten erhalten haben, können Sie die BitLocker-Schlüssel anzeigen und kopieren, die vom Dienst für Ihre Festplatte generiert wurden. Navigieren Sie im Azure-Portal zu Ihrem Speicherkonto, und klicken Sie auf die Registerkarte „Import/Export“. Wählen Sie Ihren Exportauftrag in der Liste aus, und klicken Sie auf die Schaltfläche Schlüssel anzeigen. Die BitLocker-Schlüssel werden wie hier dargestellt angezeigt:
   
   ![BitLocker-Schlüssel für einen Exportauftrag anzeigen](./media/storage-import-export-service/export-job-bitlocker-keys.png)

Sehen Sie sich unten den Abschnitt mit den häufig gestellten Fragen an. Darin werden die häufigsten Fragen von Kunden beantwortet, die bei der Nutzung des Diensts entstehen.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

**Kann ich den Azure Import/Export-Dienst verwenden, um Azure File Storage zu kopieren?**

Nein. Der Azure Import/Export-Dienst unterstützt nur Blockblobs und Seitenblobs. Alle andere Speichertypen, z.B. Azure File Storage, Table Storage und Queue Storage, werden nicht unterstützt.

**Ist der Azure Import/Export-Dienst für CSP-Abonnements verfügbar?**

Der Azure Import/Export-Dienst unterstützt CSP-Abonnements.

**Kann ich den Schritt für die Festplattenvorbereitung für einen Importauftrag überspringen, oder kann ich eine Festplatte ohne Kopiervorgang vorbereiten?**

Alle Festplatten, die Sie zum Importieren von Daten verschicken möchten, müssen mit dem WAImportExport-Tool vorbereitet werden. Sie müssen das WAImportExport-Tool verwenden, um die Daten auf die Festplatte zu kopieren.

**Muss ich den Datenträger beim Erstellen eines Exportauftrags vorbereiten?**

Nein, aber es empfiehlt sich, einige Vorabüberprüfungen durchzuführen. Überprüfen Sie die Anzahl der erforderlichen Datenträger mithilfe des PreviewExport-Befehls aus dem WAImportExport-Tool. Weitere Informationen finden Sie unter [Vorschau der Laufwerknutzung für einen Exportauftrag](https://msdn.microsoft.com/library/azure/dn722414.aspx). Sie können damit eine Vorschau der Festplattenverwendung für Blobs anzeigen, die Sie ausgewählt haben, basierend auf der Größe der Laufwerke, die Sie verwenden möchten. Überprüfen Sie außerdem, ob Sie Lese-/Schreibzugriff auf die Festplatte haben, die für den Exportauftrag versendet werden soll.

**Was geschieht, wenn ich aus Versehen eine HDD verschicke, die den unterstützten Anforderungen nicht entspricht?**

Das Azure-Datacenter schickt das Laufwerk, das den unterstützten Anforderungen nicht entspricht, an Sie zurück. Wenn nur einige der Laufwerke in dem Paket die Anforderungen erfüllen, werden diese Laufwerke verarbeitet, und die Laufwerke, die die Anforderungen nicht erfüllen, werden an Sie zurückgeschickt.

**Kann ich meinen Auftrag stornieren?**

Sie können einen Auftrag stornieren, solange dieser den Status "Wird erstellt" oder "Wird versendet" hat.

**Wie lange kann ich den Status abgeschlossener Aufträge im Azure-Portal anzeigen?**

Den Status abgeschlossener Aufträge können Sie bis zu 90 Tage lang anzeigen. Abgeschlossene Aufträge werden nach 90 Tagen gelöscht.

**Was soll ich tun, wenn ich mehr als 10 Laufwerke importieren oder exportieren möchte?**

Jeder Import- oder Exportauftrag kann auf maximal 10 Laufwerke für den Import-/Exportdienst verweisen. Wenn Sie mehr als zehn Laufwerke verschicken möchten, können Sie mehrere Aufträge erstellen. Laufwerke, die demselben Auftrag zugeordnet sind, müssen zusammen in einem Paket verschickt werden.
Microsoft bietet Anleitungen und Unterstützung für den Fall, dass die Datenkapazität mehrere Datenträgerimportaufträge umfasst. Wenden Sie sich unter bulkimport@microsoft.com an den Support, um weitere Informationen zu erhalten.

**Werden die Festplatten vom Dienst formatiert, bevor sie zurückgeschickt werden?**

Nein. Alle Festplatten werden mit BitLocker verschlüsselt.

**Kann ich bei Microsoft Laufwerke für Import-/Exportaufträge kaufen?**

Nein. Sie müssen sowohl für den Import als auch den Export Ihre eigenen Laufwerke einsenden.

**Wie kann ich auf Daten zugreifen, die von diesem Dienst importiert werden?**

Sie können über das Azure-Portal oder mithilfe eines eigenständigen Tools, dem Speicher-Explorer, auf die Daten in Ihrem Azure-Speicherkonto zugreifen. https://docs.microsoft.com/de-de/azure/vs-azure-tools-storage-manage-with-storage-explorer 

**Wie sehen meine Daten nach Abschluss des Importauftrags im Speicherkonto aus? Wird meine Verzeichnishierarchie beibehalten?**

Beim Vorbereiten einer Festplatte für einen Importauftrag wird das Ziel mit dem Feld DstBlobPathOrPrefix in der Dataset-CSV-Datei angegeben. Dies ist der Zielcontainer im Speicherkonto, in den Daten von der Festplatte kopiert werden. In diesem Zielcontainer werden virtuelle Verzeichnisse für Ordner von der Festplatte und Blobs für Dateien erstellt. 

**Wenn die Festplatte Dateien enthält, die in meinem Speicherkonto bereits vorhanden sind, werden die entsprechenden Blobs in meinem Speicherkonto vom Dienst dann überschrieben?**

Beim Vorbereiten der Festplatte können Sie angeben, ob die Zieldateien überschrieben oder ignoriert werden sollen. Hierfür verwenden Sie das Feld „Disposition:<rename|no-overwrite|overwrite>“ (umbenennen, nicht überschreiben, überschreiben) in der Dataset-CSV-Datei. In der Standardeinstellung benennt der Dienst die neuen Dateien um, anstatt vorhandene Blobs zu überschreiben.

**Ist das WAImportExport-Tool mit dem 32-Bit-Windows-Betriebssystem kompatibel?**
Nr. Das WAImportExport-Tool ist nur mit dem 64-Bit-Windows-Betriebssystem kompatibel. Eine vollständige Liste mit den unterstützten Betriebssystemversionen finden Sie im Abschnitt „Betriebssystem“ unter [Voraussetzungen](#pre-requisites) .

**Sollte mein Paket noch andere Dinge als die Festplatte enthalten?**

Bitte verschicken Sie nur Ihre Laufwerke. Legen Sie keine Gegenstände wie z. B. Strom- oder USB-Kabel bei.

**Muss ich meine Festplatten mit FedEx oder DHL schicken?**

Sie können Festplatten mit allen gängigen Kurierdiensten wie FedEx, DHL, UPS oder US Postal Service an das Rechenzentrum schicken. Für den Rückversand der Festplatten aus dem Rechenzentrum müssen Sie aber eine FedEx-Kontonummer (USA und Europa) oder eine DHL-Kontonummer (Asien und Australien) angeben.

**Gelten für den internationalen Versand meiner Festplatte bestimmte Einschränkungen?**

Beachten Sie, dass die physischen Medien beim Versand unter Umständen Ländergrenzen überqueren. Sie müssen sicherstellen, dass Ihre physischen Medien und Daten gemäß geltender Gesetze importiert bzw. exportiert werden. Prüfen Sie vor dem Versand der physischen Medien mit Ihren Rechtsberatern, ob Medien und Daten laut Gesetz an das entsprechende Rechenzentrum verschickt werden dürfen. So stellen Sie sicher, dass Ihre Daten zeitnah bei Microsoft eintreffen.

**Beim Erstellen des Auftrags habe ich festgestellt, dass die Versandadresse ein Standort ist, der sich von meinem Speicherkontostandort unterscheidet. Wie soll ich vorgehen?**

Einige Speicherkontostandorte sind alternativen Standorten für den Versand zugeordnet. Versandstandorte, die bisher verfügbar waren, können vorübergehend auch anderen Standorten zugeordnet werden. Überprüfen Sie immer die Versandadresse, die bei der Erstellung des Auftrags angegeben wurde, bevor Sie die Festplatten verschicken.

**Beim Versand der Festplatte fragt der Kurierdienst nach der Anschrift und der Telefonnummer des Rechenzentrums. Was muss ich hier angeben?**

Die Telefonnummer und die Adresse des Rechenzentrums werden beim Erstellen des Auftrags angegeben.

**Kann ich den Azure Import/Export-Dienst zum Kopieren von PST-Postfächern und SharePoint-Daten in O365 verwenden?**

Informationen hierzu finden Sie unter [Importieren von PST-Dateien oder SharePoint-Daten in Office 365](https://technet.microsoft.com/library/ms.o365.cc.ingestionhelp.aspx).

**Kann ich den Azure Import/Export-Dienst zum Offlinekopieren meiner Sicherungen in den Azure Backup-Dienst verwenden?**

Informationen hierzu finden Sie unter [Workflow zur Offlinesicherung in Azure Backup](../../backup/backup-azure-backup-import-export.md).

**Wie viele HDDs kann eine Lieferung maximal enthalten?**

Eine Lieferung kann eine beliebige Anzahl von HDDs umfassen, und wenn die Datenträger zu mehreren Aufträgen gehören, wird empfohlen, a) die Datenträger mit den entsprechenden Auftragsnamen zu beschriften, und b) die Aufträge mit einer Nachverfolgungsnummer und dem Suffix -1, -2 usw. zu versehen.
  
**Welche maximale Blockblob- und Seitenblobgröße wird vom Datenträgerimport/-export unterstützt?**

Die maximale Blockblobgröße beträgt ungefähr 4,768 TB oder 5.000.000 MB.
Die maximale Seitenblobgröße beträgt 1 TB.

**Unterstützt der Datenträgerimport/-export die AES-256-Verschlüsselung?**

Der Azure Import/Export-Dienst verwendet standardmäßig die AES-128-BitLocker-Verschlüsselung. Die Verschlüsselung kann jedoch auf AES-256 erhöht werden, indem vor dem Kopieren von Daten die manuelle Verschlüsselung mit BitLocker durchgeführt wird. 

Wenn Sie [WAImportExpot V1](http://download.microsoft.com/download/0/C/D/0CD6ABA7-024F-4202-91A0-CE2656DCE413/WaImportExportV1.zip) verwenden, sehen Sie sich folgenden Beispielbefehl an:
```
WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>] 
```
Geben Sie bei Verwendung des [WAImportExport-Tools](http://download.microsoft.com/download/3/6/B/36BFF22A-91C3-4DFC-8717-7567D37D64C5/WAImportExport.zip) „AlreadyEncrypted“ an, und geben Sie den Schlüssel in der Driveset-CSV-Datei an.
```
DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
G,AlreadyFormatted,SilentMode,AlreadyEncrypted,060456-014509-132033-080300-252615-584177-672089-411631 |
```
## <a name="next-steps"></a>Nächste Schritte

* [Einrichten des WAImportExport-Tools](storage-import-export-tool-how-to.md)
* [Übertragen von Daten mit dem Befehlszeilenprogramm AzCopy](storage-use-azcopy.md)
* [Azure Import/Export-REST-API-Beispiel](https://azure.microsoft.com/documentation/samples/storage-dotnet-import-export-job-management/)


