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
ms.translationtype: HT
ms.sourcegitcommit: 9569f94d736049f8a0bb61beef0734050ecf2738
ms.openlocfilehash: 96c641672ce6515fad3abc3fc0b8a6af037de346
ms.contentlocale: de-de
ms.lasthandoff: 08/31/2017

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

## <a name="enable-secure-transfer-required-programmatically"></a>Programmgesteuertes Aktivieren der Einstellung „Sichere Übertragung erforderlich“

Der Einstellungsname lautet in Speicherkontoeigenschaften _supportsHttpsTrafficOnly_. Sie können ihn mit der REST-API, mit Tools oder Bibliotheken aktivieren:

* [REST-API](https://docs.microsoft.com/en-us/rest/api/storagerp/storageaccounts) (Version: 2016-12-01)
* [PowerShell](https://docs.microsoft.com/en-us/powershell/module/azurerm.storage/set-azurermstorageaccount?view=azurermps-4.1.0) (Version: 4.1.0)
* [CLI](https://pypi.python.org/pypi/azure-cli-storage/2.0.11) (Version: 2.0.11)
* [NodeJS](https://www.npmjs.com/package/azure-arm-storage/) (Version: 1.1.0)
* [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/6.3.0-preview) (Version: 6.3.0)
* [Python SDK](https://pypi.python.org/pypi/azure-mgmt-storage/1.1.0) (Version: 1.1.0)
* [Ruby SDK](https://rubygems.org/gems/azure_mgmt_storage) (Version: 0.11.0)

### <a name="enable-secure-transfer-required-setting-with-rest-api"></a>Aktivieren der Einstellung „Sichere Übertragung erforderlich“ mit der REST-API

Um die Tests mit der REST-API zu vereinfachen, können Sie anhand von [ArmClient](https://github.com/projectkudu/ARMClient) einen Aufruf über die Befehlszeile durchführen.

 Anhand der nachfolgenden Befehlszeile können Sie die Einstellung mit der REST-API überprüfen:

```
# Login Azure and proceed with your credentials
> armclient login

> armclient GET  /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{accountName}?api-version=2016-12-01
```

In der Antwort finden Sie die Einstellung _supportsHttpsTrafficOnly_. Beispiel:

```Json

{
  "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{accountName}",
  "kind": "Storage",
  ...
  "properties": {
    ...
    "supportsHttpsTrafficOnly": false
  },
  "type": "Microsoft.Storage/storageAccounts"
}

```

Anhand der nachfolgenden Befehlszeile können Sie die Einstellung mit der REST-API aktivieren:

```

# Login Azure and proceed with your credentials
> armclient login

> armclient PUT /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{accountName}?api-version=2016-12-01 < Input.json

```

Beispiel für Input.json:
```Json

{
  "location": "westus",
  "properties": {
    "supportsHttpsTrafficOnly": true
  }
}

```

## <a name="next-steps"></a>Nächste Schritte
Azure Storage bietet einen umfassenden Satz von Sicherheitsfunktionen, die Entwicklern das Erstellen sicherer Anwendungen ermöglichen. Weitere Informationen finden Sie im [Azure Storage-Sicherheitsleitfaden](storage-security-guide.md).

