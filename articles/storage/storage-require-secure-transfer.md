---
title: "Vorschreiben einer sicheren Übertragung in Azure Storage | Microsoft-Dokumentation"
description: "Hier erhalten Sie Informationen zur Funktion „Sichere Übertragung erforderlich“ für Azure Storage; zudem wird beschrieben, wie die Funktion aktiviert wird."
services: storage
documentationcenter: na
author: fhryo-msft
manager: Jason.Hogg
editor: fhryo-msft
ms.assetid: 
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 06/20/2017
ms.author: fryu
ms.translationtype: Human Translation
ms.sourcegitcommit: 1e6f2b9de47d1ce84c4043f5f6e73d462e0c1271
ms.openlocfilehash: 516618653064fd4e334197bba767a013a805260a
ms.contentlocale: de-de
ms.lasthandoff: 06/21/2017

---
# <a name="require-secure-transfer"></a>Vorschreiben einer sicheren Übertragung

Die Funktion „Sichere Übertragung erforderlich“ steigert die Sicherheit Ihres Speicherkontos, da Anforderungen an das Speicherkonto von sicheren Verbindungen zugelassen werden. Beim Aufrufen von REST-APIs zum Zugreifen auf das Speicherkonto müssen Sie beispielsweise eine Verbindung über HTTPS herstellen. Wenn „Sichere Übertragung erforderlich“ aktiviert ist, werden alle Anforderungen über HTTP zurückgewiesen.

Wenn Sie den Azure Files-Dienst verwenden, schlägt jede Verbindung ohne Verschlüsselung fehl, wenn „Sichere Übertragung erforderlich“ aktiviert ist. Hierzu zählen Szenarien, in denen SMB 2.1, SMB 3.0 ohne Verschlüsselung und einige Varianten des Linux SMB-Clients verwendet werden. 

In der Standardeinstellung ist die Option „Sichere Übertragung erforderlich“ deaktiviert.

> [!NOTE]
> Da Azure Storage keine Unterstützung von HTTPS für benutzerdefinierte Domänennamen bietet, wird diese Option nicht angewendet, wenn ein benutzerdefinierter Domänenname verwendet wird.

## <a name="enable-secure-transfer-required-in-the-azure-portal"></a>Aktivieren von „Sichere Übertragung erforderlich“ im Azure-Portal

Sie können die Einstellung „Sichere Übertragung erforderlich“ beim Erstellen eines Speicherkontos im [Azure-Portal](https://portal.azure.com) sowie für vorhandene Speicherkonten aktivieren.

### <a name="require-secure-transfer-when-you-create-a-storage-account"></a>Vorschreiben einer sicheren Übertragung beim Erstellen eines Speicherkontos

1. Öffnen Sie das Blatt **Speicherkonto erstellen** im Azure-Portal.
1. Wählen Sie unter **Sichere Übertragung erforderlich** die Option **Aktiviert** aus.

  ![Screenshot](./media/storage-require-secure-transfer/secure_transfer_field_in_portal_en_1.png)

### <a name="require-secure-transfer-for-an-existing-storage-account"></a>Vorschreiben einer sicheren Übertragung für ein vorhandenes Speicherkonto

1. Wählen Sie ein vorhandenes Speicherkonto im Azure-Portal aus.
1. Wählen Sie im Menüblatt des Speicherkontos unter **EINSTELLUNGEN** die Option **Konfiguration** aus.
1. Wählen Sie unter **Sichere Übertragung erforderlich** die Option **Aktiviert** aus.

  ![Screenshot](./media/storage-require-secure-transfer/secure_transfer_field_in_portal_en_2.png)

## <a name="next-steps"></a>Nächste Schritte
Azure Storage bietet einen umfassenden Satz von Sicherheitsfunktionen, die Entwicklern das Erstellen sicherer Anwendungen ermöglichen. Weitere Informationen finden Sie im [Azure Storage-Sicherheitsleitfaden](storage-security-guide.md).

