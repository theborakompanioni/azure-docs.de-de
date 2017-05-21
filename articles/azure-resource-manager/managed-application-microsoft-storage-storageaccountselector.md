---
title: "Benutzeroberflächenelement „StorageAccountSelector“ in verwalteten Azure-Anwendungen | Microsoft-Dokumentation"
description: "Hier wird das Benutzeroberflächenelement „Microsoft.Storage.StorageAccountSelector“ für verwaltete Azure-Anwendungen beschrieben."
services: azure-resource-manager
documentationcenter: na
author: tabrezm
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2017
ms.author: tabrezm;tomfitz
ms.translationtype: Human Translation
ms.sourcegitcommit: 97fa1d1d4dd81b055d5d3a10b6d812eaa9b86214
ms.openlocfilehash: 2d64cfcd958f17171e9ef1a38777f202bdfda774
ms.contentlocale: de-de
ms.lasthandoff: 05/11/2017


---
# <a name="microsoftstoragestorageaccountselector-ui-element"></a>Benutzeroberflächenelement „Microsoft.Storage.StorageAccountSelector“
Ein Steuerelement zum Auswählen eines neuen oder vorhandenen Speicherkontos.

## <a name="ui-sample"></a>Benutzeroberflächenbeispiel
![Microsoft.Storage.StorageAccountSelector](./media/managed-application-elements/microsoft.storage.storageaccountselector.png)

## <a name="schema"></a>Schema
```json
{
  "name": "element1",
  "type": "Microsoft.Storage.StorageAccountSelector",
  "label": "Storage account",
  "toolTip": "",
  "defaultValue": {
    "name": "storageaccount01",
    "type": "Premium_LRS"
  },
  "constraints": {
    "allowedTypes": [],
    "excludedTypes": []
  },
  "options": {
    "hideExisting": false
  },
  "visible": true
}
```

## <a name="remarks"></a>Anmerkungen
- Wird `defaultValue.name` angegeben, wird das Element automatisch auf Eindeutigkeit überprüft. Wenn der Name des Speicherkontos nicht eindeutig ist, muss der Benutzer einen anderen Namen angeben oder ein vorhandenes Speicherkonto auswählen.
- Der Standardwert für `defaultValue.type` lautet **Premium_LRS**.
- Alle nicht in `constraints.allowedTypes` angegebenen Typen werden ausgeblendet, und nicht in `constraints.excludedTypes` angegebene Typen werden angezeigt.
`constraints.allowedTypes` und `constraints.excludedTypes` sind optional, können jedoch nicht gleichzeitig angegeben werden.
- Wenn **true** für `options.hideExisting` angegeben ist, kann der Benutzer kein vorhandenes Speicherkonto auswählen. Der Standardwert ist **false**.


## <a name="sample-output"></a>Beispielausgabe
```json
{
  "name": "storageaccount01",
  "resourceGroup": "rg01",
  "type": "Premium_LRS",
  "newOrExisting": "new"
}
```

## <a name="next-steps"></a>Nächste Schritte
* Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](managed-application-createuidefinition-overview.md).
* Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](managed-application-createuidefinition-elements.md).

