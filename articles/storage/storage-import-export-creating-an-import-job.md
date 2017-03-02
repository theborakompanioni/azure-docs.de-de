---
title: "Erstellen eines Importauftrags für Azure Import/Export | Microsoft Docs"
description: "Es wird beschrieben, wie Sie einen Import für den Microsoft Azure Import/Export-Dienst erstellen."
author: muralikk
manager: syadav
editor: syadav
services: storage
documentationcenter: 
ms.assetid: 8b886e83-6148-4149-9d0f-5d48ec822475
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
translationtype: Human Translation
ms.sourcegitcommit: 50fbd0d214c825137c3ac6873be27b9d2d53cee1
ms.openlocfilehash: 84ba5256c3ee485af9a1a6bccc0571f93c9c0ab7
ms.lasthandoff: 02/16/2017


---
# <a name="creating-an-import-job-for-the-azure-importexport-service"></a>Erstellen eines Importauftrags für den Azure Import/Export-Dienst

Zum Erstellen eines Importauftrags für den Microsoft Azure Import/Export-Dienst per REST-API sind die folgenden Schritte erforderlich:

-   Vorbereiten von Laufwerken mit dem Azure Import/Export-Tool

-   Beschaffen der Versandadresse, an die das Laufwerk geschickt wird

-   Erstellen des Importauftrags

-   Senden der Laufwerke an Microsoft über einen unterstützten Kurierdienst

-   Aktualisieren des Importauftrags mit den Versandformationen

 Eine Übersicht über den Import/Export-Dienst und ein Tutorial, in dem die Verwendung des [Azure-Portals](https://portal.azure.com/) zum Erstellen und Verwalten von Import- und Exportaufträgen beschrieben wird, finden Sie unter [Verwenden des Microsoft Azure Import/Export-Diensts zum Übertragen von Daten in den Blobspeicher](storage-import-export-service.md).

## <a name="preparing-drives-with-the-azure-importexport-tool"></a>Vorbereiten von Laufwerken mit dem Azure Import/Export-Tool

Die Schritte zum Vorbereiten von Laufwerken für einen Importauftrag sind für die Erstellung des Auftrags über das Portal und per REST-API identisch.

Unten finden Sie eine kurze Übersicht über die Vorbereitung von Laufwerken. Eine ausführliche Anleitung finden Sie unter [Using the Azure Import/Export Tool (v1)](storage-import-export-tool-how-to-v1.md) (Verwenden des Azure Import/Export-Tools (v1)). Sie können das Azure Import/Export-Tool [hier](http://go.microsoft.com/fwlink/?LinkID=301900) herunterladen.

Die Vorbereitung des Laufwerks umfasst Folgendes:

-   Identifizieren der zu importierenden Daten

-   Identifizieren der Zielblobs in Windows Azure Storage

-   Verwenden des Azure Import/Export-Tools zum Kopieren Ihrer Daten auf eine oder mehrere Festplatten

 Mit dem Azure Import/Export-Tool wird für jedes Laufwerk während der Vorbereitung auch eine Manifestdatei generiert. Eine Manifestdatei enthält Folgendes:

-   Eine Enumeration aller Dateien, die für den Upload bestimmt sind, und die Zuordnungen von diesen Dateien zu Blobs.

-   Prüfsummen der Segmente der einzelnen Dateien.

-   Informationen zu den Metadaten und Eigenschaften, die den einzelnen Blobs zugeordnet werden.

-   Eine Liste mit den durchzuführenden Aktionen, wenn ein Blob, das hochgeladen wird, den gleichen Namen wie ein im Container vorhandenes Blob hat. Mögliche Optionen sind: a) Überschreiben des Blobs durch die Datei, b) Beibehalten des vorhandenen Blobs und Überspringen des Dateiuploads und c) Anfügen eines Suffixes an den Namen, damit es nicht zu einem Konflikt mit anderen Dateien kommt.

## <a name="obtaining-your-shipping-location"></a>Beschaffen der Versandadresse

Vor dem Erstellen eines Importauftrags müssen Sie sich die Versandadresse beschaffen, indem Sie den Vorgang [List Locations](/rest/api/storageimportexport/listlocations) aufrufen. `List Locations` gibt eine Liste mit Orten und den dazugehörigen Versandadressen zurück. Sie können einen Ort aus der zurückgegebenen Liste auswählen und Ihre Festplatten an diese Adresse schicken. Sie können auch den Vorgang `Get Location` verwenden, um die Versandadresse für einen bestimmten Ort direkt zu beschaffen.

 Führen Sie die unten angegebenen Schritte aus, um das Versandziel zu beschaffen:

-   Ermitteln Sie den Namen des Standorts für Ihr Speicherkonto. Sie finden diesen Wert im Feld **Standort** im **Dashboard** des Azure-Portals für Ihr Speicherkonto, oder Sie können mit dem Vorgang [Get Storage Account Properties](/rest/api/storagerp/storageaccounts#StorageAccounts_GetProperties) der Dienstverwaltungs-API eine Abfrage durchführen.

-   Rufen Sie den Standort ab, der zum Verarbeiten des Speicherkontos verfügbar ist, indem Sie den Vorgang `Get Location` aufrufen.

-   Wenn die `AlternateLocations`-Eigenschaft des Standorts den Standort selbst enthält, können Sie diesen verwenden. Rufen Sie andernfalls den Vorgang `Get Location` mit einem der anderen Standorte erneut auf. Es kann sein, dass der Originalstandort zu Wartungszwecken vorübergehend geschlossen ist.

## <a name="creating-the-import-job"></a>Erstellen des Importauftrags
Rufen Sie zum Erstellen des Importauftrags den Vorgang [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) auf. Sie müssen die folgenden Informationen angeben:

-   Einen Namen für den Auftrag

-   Name des Speicherkontos

-   Name des Versandziels aus dem vorherigen Schritt

-   Auftragstyp (Import)

-   Adresse für den Rückversand nach Abschluss des Importauftrags

-   Liste mit den Laufwerken im Auftrag Für jedes Laufwerk müssen Sie die folgenden Informationen angeben, die während der Laufwerkvorbereitung ermittelt wurden:

    -   Laufwerk-ID

    -   BitLocker-Schlüssel

    -   Relativer Pfad der Manifestdatei auf der Festplatte

    -   MD5-Hash der Manifestdatei mit Base16-Codierung

## <a name="shipping-your-drives"></a>Senden der Laufwerke
Sie müssen die Laufwerke an die Adresse senden, die Sie im vorherigen Schritt ermittelt haben, und Sie müssen für den Import/Export-Dienst die Nachverfolgungsnummer des Pakets angeben.

> [!NOTE]
>  Es ist erforderlich, Ihre Laufwerke über einen unterstützten Kurierdienst zu versenden, der für Ihr Paket eine Nachverfolgungsnummer bereitstellt.

## <a name="updating-the-import-job-with-your-shipping-information"></a>Aktualisieren des Importauftrags mit Ihren Versandinformationen
Nachdem Sie die Nachverfolgungsnummer erhalten haben, können Sie den Vorgang [Update Job Properties](/api/storageimportexport/jobs#Jobs_Update) aufrufen, um den Namen des Kurierdiensts, die Nachverfolgungsnummer für den Auftrag und die Nummer des Kontos beim Kurierdienst für den Rückversand zu aktualisieren. Optional können Sie auch die Anzahl von Laufwerken und das Versanddatum angeben.

## <a name="see-also"></a>Weitere Informationen
[Verwenden der REST-API des Import/Export-Diensts](storage-import-export-using-the-rest-api.md)

