---
title: "Azure Stack-Speicher: Unterschiede und Überlegungen"
description: Lernen Sie die Unterschiede zwischen dem Azure Stack-Speicher und Azure Storage kennen, und erhalten Sie weitere Informationen zur Bereitstellung von Azure Stack.
services: azure-stack
documentationcenter: 
author: xiaofmao
manager: 
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/22/2017
ms.author: xiaofmao
ms.translationtype: HT
ms.sourcegitcommit: 137671152878e6e1ee5ba398dd5267feefc435b7
ms.openlocfilehash: beb2d26afb8ddc5e85b1828c71de5cbd9e678fe1
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---
# <a name="azure-stack-storage-differences-and-considerations"></a>Azure Stack-Speicher: Unterschiede und Überlegungen
Azure Stack-Speicher ist eine Reihe von Speicherclouddiensten in Microsoft Azure Stack. Der Azure Stack-Speicher bietet Blob-, Tabellen-, Warteschlangen- und Kontoverwaltungsfunktionen mit einer mit Azure konsistenten Semantik.

Dieser Artikel beschreibt die bekannten Unterschiede des Azure Stack-Speichers im Vergleich zu Azure Storage. Außerdem enthält er noch weitere Überlegungen, die Sie bei der Bereitstellung von Azure Stack beachten sollten. Informationen zu allgemeinen Unterschieden zwischen Azure Stack und Azure finden Sie im Thema [Key considerations](azure-stack-considerations.md) (Wichtige Aspekte).

## <a name="cheat-sheet-storage-differences"></a>Spickzettel: Speicherunterschiede

| Feature | Azure (global) | Azure Stack |
| --- | --- | --- |
|File Storage|Cloudbasierte SMB-Dateifreigaben unterstützt|Noch nicht unterstützt
|Verschlüsselung für ruhende Daten|256-Bit-AES-Verschlüsselung|Noch nicht unterstützt
|Speicherkontotyp|Allgemeine Speicherkonten und Azure Blob Storage-Konten|Nur allgemein
|Replikationsoptionen|Lokal redundanter Speicher, georedundanter Speicher, schreibgeschützter georedundanter Speicher und zonenredunanter Speicher|Lokal redundanter Speicher
|Storage Premium|Vollständig unterstützt|Kann bereitgestellt werden, jedoch ohne Leistungsgrenzwerte oder Garantien
|Verwaltete Datenträger|Unterstützung für Premium und Standard|Noch nicht unterstützt
|Blobname|1.024 Zeichen (2.048 Bytes)|880 Zeichen (1.760 Bytes)
|Maximale Blockblobgröße|4,75 TB (100 MB X 50.000 Blöcke)|50.000 x 4 MB (ca. 195 GB)
|Inkrementelle Momentaufnahmenkopie des Seitenblobs|Unterstützung für Premium- und Standard-Azure-Seitenblobs|Noch nicht unterstützt
|Maximale Seitenblobgröße|8 TB|1 TB
|Seitenblob – Seitengröße|512 Bytes|4 KB
|Größe für Tabellenpartitionsschlüssel und Zeilenschlüssel|1.024 Zeichen (2.048 Bytes)|400 Zeichen (800 Bytes)

### <a name="metrics"></a>Metriken
Es gibt auch einige Unterschiede zu Speichermetriken:
* Die Transaktionsdaten in Speichermetriken unterscheiden nicht zwischen interner oder externer Netzwerkbandbreite.
* Die Transaktionsdaten in Speichermetriken beinhalten keinen Zugriff auf die bereitgestellten Datenträger über virtuelle Computer.

## <a name="api-version"></a>API-Version
Die folgenden Versionen werden für Azure Stack-Speicher unterstützt:

* Azure Storage-Datendienste: [REST-API-Version vom 05.04.2015](https://docs.microsoft.com/en-us/rest/api/storageservices/Version-2015-04-05?redirectedfrom=MSDN)
* Azure Storage-Verwaltungsdienste: [Vorschau vom 01.05.2015, 15.06.2015 und 01.01.2016](https://docs.microsoft.com/en-us/rest/api/storagerp/?redirectedfrom=MSDN) 

## <a name="next-steps"></a>Nächste Schritte

* [Get started with Azure Stack Storage development tools](azure-stack-storage-dev.md) (Erste Schritte mit den Azure Stack-Speicherentwicklungstools)
* [Introduction to Azure Stack Storage](azure-stack-storage-overview.md) (Einführung zu Azure Stack-Speicher)


