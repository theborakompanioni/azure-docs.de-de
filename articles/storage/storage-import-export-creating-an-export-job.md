---
title: "Erstellen eines Exportauftrags für Azure Import/Export | Microsoft Docs"
description: "Es wird beschrieben, wie Sie einen Exportauftrag für den Microsoft Azure Import/Export-Dienst erstellen."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 613d480b-a8ef-4b28-8f54-54174d59b3f4
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
translationtype: Human Translation
ms.sourcegitcommit: 74182c8c357085f186aaa43adfaef80a083d16bb
ms.openlocfilehash: 55a45cf4169056d9426141a533dd1d32fe8064c4
ms.lasthandoff: 02/16/2017


---

# <a name="creating-an-export-job-for-the-azure-importexport-service"></a>Erstellen eines Exportauftrags für den Azure Import/Export-Dienst
Zum Erstellen eines Exportauftrags für den Microsoft Azure Import/Export-Dienst per REST-API sind die folgenden Schritte erforderlich:

-   Auswählen der zu exportierenden Blobs

-   Beschaffen eines Versandziels

-   Erstellen des Exportauftrags

-   Senden Ihrer leeren Laufwerke an Microsoft über einen unterstützten Lieferanten

-   Aktualisieren des Exportauftrags mit den Paketinformationen

-   Zurückerhalten der Laufwerke von Microsoft

 Eine Übersicht über den Import/Export-Dienst und ein Tutorial, in dem die Verwendung des [Azure-Portals](https://portal.azure.com/) zum Erstellen und Verwalten von Import- und Exportaufträgen beschrieben wird, finden Sie unter [Verwenden des Windows Azure Import/Export-Diensts zum Übertragen von Daten an Blobspeicher](storage-import-export-service.md).

## <a name="selecting-blobs-to-export"></a>Auswählen der zu exportierenden Blobs
 Zum Erstellen eines Exportauftrags müssen Sie eine Liste mit Blobs bereitstellen, die Sie aus Ihrem Speicherkonto exportieren möchten. Es gibt ein paar Möglichkeiten, die zu exportierenden Blobs auszuwählen:

-   Sie können einen relativen Blobpfad verwenden, um ein einzelnes Blob und alle zugehörigen Momentaufnahmen auszuwählen.

-   Sie können einen relativen Blobpfad verwenden, um ein einzelnes Blob ohne die zugehörigen Momentaufnahmen auszuwählen.

-   Sie können einen relativen Blobpfad und eine Momentaufnahmezeit verwenden, um eine einzelne Momentaufnahme auszuwählen.

-   Sie können ein Blobpräfix verwenden, um alle Blobs und Momentaufnahmen mit dem angegebenen Präfix auszuwählen.

-   Sie können alle Blobs und Momentaufnahmen im Speicherkonto exportieren.

 Weitere Informationen zum Angeben der zu exportierenden Blobs finden Sie in der Beschreibung des [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate)-Vorgangs.

## <a name="obtaining-your-shipping-location"></a>Beschaffen des Versandziels
Vor dem Erstellen eines Exportauftrags müssen Sie sich den Namen und die Adresse des Versandziels beschaffen, indem Sie den Vorgang [Get Location](https://portal.azure.com) oder [List Locations](/rest/api/storageimportexport/listlocations) aufrufen. `List Locations` gibt eine Liste mit Orten und den dazugehörigen Versandadressen zurück. Sie können einen Ort aus der zurückgegebenen Liste auswählen und Ihre Festplatten an diese Adresse schicken. Sie können auch den Vorgang `Get Location` verwenden, um die Versandadresse für einen bestimmten Ort direkt zu beschaffen.

Führen Sie die unten angegebenen Schritte aus, um das Versandziel zu beschaffen:

-   Ermitteln Sie den Namen des Standorts für Ihr Speicherkonto. Sie finden diesen Wert im Feld **Standort** im **Dashboard** des klassischen Portals für Ihr Speicherkonto, oder Sie können mit dem Vorgang [Get Storage Account Properties](/rest/api/storagerp/storageaccounts#StorageAccounts_GetProperties) der Dienstverwaltungs-API eine Abfrage durchführen.

-   Rufen Sie den Standort ab, der zum Verarbeiten des Speicherkontos verfügbar ist, indem Sie den Vorgang `Get Location` aufrufen.

-   Wenn die `AlternateLocations`-Eigenschaft des Standorts den Standort selbst enthält, können Sie diesen verwenden. Rufen Sie andernfalls den Vorgang `Get Location` mit einem der anderen Standorte erneut auf. Es kann sein, dass der Originalstandort zu Wartungszwecken vorübergehend geschlossen ist.

## <a name="creating-the-export-job"></a>Erstellen des Exportauftrags
 Rufen Sie zum Erstellen des Exportauftrags den Vorgang [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) auf. Sie müssen die folgenden Informationen angeben:

-   Einen Namen für den Auftrag

-   Der Name des Speicherkontos.

-   Den Name des Versandziels aus dem vorherigen Schritt

-   Einen Auftragstyp (Export)

-   Die Adresse für den Rückversand nach Abschluss des Exportauftrags

-   Die Liste der Blobs (oder Blobpräfixe), die exportiert werden sollen

## <a name="shipping-your-drives"></a>Senden der Laufwerke
 Verwenden Sie anschließend das Azure Import/Export-Tool, um basierend auf den Blobs, die exportiert werden sollen, und der Laufwerkgröße die Anzahl der Laufwerke zu ermitteln, die Sie senden müssen. Weitere Informationen finden Sie unter [Azure Import/Export-Tool – Referenz](storage-import-export-tool-how-to-v1.md).

 Verpacken Sie die Laufwerke in ein einziges Paket, und senden Sie diese an die im vorhergehenden Schritt abgerufene Adresse. Notieren Sie die Nachverfolgungsnummer des Pakets für den nächsten Schritt.

> [!NOTE]
>  Es ist erforderlich, Ihre Laufwerke über einen unterstützten Kurierdienst zu versenden, der für Ihr Paket eine Nachverfolgungsnummer bereitstellt.

## <a name="updating-the-export-job-with-your-package-information"></a>Aktualisieren des Exportauftrags mit den Paketinformationen
 Nachdem Sie die Nachverfolgungsnummer erhalten haben, rufen Sie den Vorgang [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) auf, um den Namen des Kurierdiensts und die Nachverfolgungsnummer für den Auftrag zu aktualisieren. Optional können Sie auch die Anzahl von Laufwerken, die Adresse für den Rückversand und das Versanddatum angeben.

## <a name="receiving-the-package"></a>Empfangen des Pakets
 Nach der Verarbeitung des Exportauftrags werden Ihre Laufwerke mit den verschlüsselten Daten an Sie zurückgesendet. Sie können den BitLocker-Schlüssel für jedes Laufwerk durch Aufrufen des [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get)-Vorgangs abrufen. Anschließend können Sie das Laufwerk mit dem Schlüssel entsperren. Die Laufwerkmanifestdatei auf jedem Laufwerk enthält die Liste der Dateien auf dem Laufwerk sowie die ursprüngliche Blobadresse für jede Datei.

## <a name="see-also"></a>Weitere Informationen
 [Verwenden der REST-API des Import/Export-Diensts](storage-import-export-using-the-rest-api.md)

