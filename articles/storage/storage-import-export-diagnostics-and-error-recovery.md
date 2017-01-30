---
title: "Diagnose und Fehlerbehebung für Azure Import/Export-Aufträge | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie die ausführliche Protokollierung für die Aufträge des Microsoft Azure Import/Export-Diensts aktivieren."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 096cc795-9af6-4335-9fe8-fffa9f239a17
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: muralikk
translationtype: Human Translation
ms.sourcegitcommit: 22e37e26fec913a7638c71b2547c38f5efacd10b
ms.openlocfilehash: 060e8cbbd812ed2495d1c589f29760d2453af51b


---

# <a name="diagnostics-and-error-recovery-for-import-export-jobs"></a>Diagnose und Fehlerbehebung für Import/Export-Aufträge
Für jeden verarbeiteten Datenträger wird vom Azure Import/Export-Dienst im zugeordneten Speicherkonto ein Fehlerprotokoll erstellt. Sie können die ausführliche Protokollierung auch aktivieren, indem Sie die `LogLevel`-Eigenschaft `Verbose` festlegen, wenn Sie die Vorgänge [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) oder [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) aufrufen.

 Standardmäßig werden Protokolle in einen Container mit dem Namen `waimportexport` geschrieben. Sie können einen anderen Namen angeben, indem Sie beim Aufrufen der Vorgänge `Put Job` oder `Update Job Properties` die `DiagnosticsPath`-Eigenschaft festlegen. Die Protokolle werden mit der folgenden Benennungskonvention als Blockblobs gespeichert: `waies/jobname_driveid_timestamp_logtype.xml`.

 Sie können den URI der Protokolle für einen Auftrag abrufen, indem Sie den Vorgang [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) aufrufen. Der URI für das ausführliche Protokoll wird in der `VerboseLogUri`-Eigenschaft für jedes Laufwerk zurückgegeben, während der URI für das Fehlerprotokoll in der `ErrorLogUri`-Eigenschaft zurückgegeben wird.

Sie können die Protokollierungsdaten verwenden, um die folgenden Probleme zu identifizieren:

**Laufwerkfehler**

-   Fehler beim Zugreifen auf oder Lesen der Manifestdatei

-   Fehlerhafte BitLocker-Schlüssel

-   Lese-/Schreibfehler des Laufwerks

**Blobfehler**

-   Falsche oder in Konflikt stehende Blobs oder Namen

-   Fehlende Dateien

-   Nicht gefundenes Blob

-   Abgeschnittene Dateien (die Dateien auf dem Datenträger sind kleiner als im Manifest angegeben)

-   Beschädigter Dateiinhalt (für Importaufträge, Erkennung per MD5-Prüfsummenfehler)

-   Beschädigte Blobmetadaten und Eigenschaftsdateien (Erkennung per MD5-Prüfsummenfehler)

-   Fehlerhaftes Schema für die Blobeigenschaften bzw. Metadatendateien

Es kann vorkommen, dass einige Teile eines Import- oder Exportauftrags nicht vollständig abgeschlossen werden, während der Gesamtauftrag trotzdem abgeschlossen wird. In diesem Fall können Sie die fehlenden Teile der Daten über das Netzwerk hoch- oder herunterladen, oder Sie können einen neuen Auftrag erstellen, um die Daten zu übertragen. Informationen dazu, wie Sie die Daten über das Netzwerk reparieren, finden Sie unter [Azure Import-Export Tool Reference](storage-import-export-tool-how-to-v1.md) (Azure Import/Export-Tool – Referenz).

## <a name="see-also"></a>Weitere Informationen
[Using the Azure Import-Export Service REST API](storage-import-export-using-the-rest-api.md) (Verwenden der REST-API des Azure Import/Export-Diensts)



<!--HONumber=Dec16_HO3-->


