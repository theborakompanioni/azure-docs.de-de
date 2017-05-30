---
title: "Benutzeroberflächenelement „MultiStorageAccountCombo“ in verwalteten Azure-Anwendungen | Microsoft-Dokumentation"
description: "Hier wird das Benutzeroberflächenelement „Microsoft.Storage.MultiStorageAccountCombo“ für verwaltete Azure-Anwendungen beschrieben."
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
ms.date: 05/12/2017
ms.author: tabrezm;tomfitz
ms.translationtype: Human Translation
ms.sourcegitcommit: afa23b1395b8275e72048bd47fffcf38f9dcd334
ms.openlocfilehash: 27843b116d949899e4eae65f342324f77ebca70b
ms.contentlocale: de-de
ms.lasthandoff: 05/12/2017


---
# <a name="microsoftstoragemultistorageaccountcombo-ui-element"></a>Benutzeroberflächenelement „Microsoft.Storage.MultiStorageAccountCombo“
Eine Gruppe von Steuerelementen zum Erstellen mehrerer Speicherkonten mit Namen, die mit einem allgemeinen Präfix beginnen. Dieses Element verwenden Sie beim [Erstellen einer verwalteten Azure-Anwendung](managed-application-publishing.md).

## <a name="ui-sample"></a>Benutzeroberflächenbeispiel
![Microsoft.Storage.MultiStorageAccountCombo](./media/managed-application-elements/microsoft.storage.multistorageaccountcombo.png)

## <a name="schema"></a>Schema
```json
{
  "name": "element1",
  "type": "Microsoft.Storage.MultiStorageAccountCombo",
  "label": {
    "prefix": "Storage account prefix",
    "type": "Storage account type"
  },
  "toolTip": {
    "prefix": "",
    "type": ""
  },
  "defaultValue": {
    "prefix": "sa",
    "type": "Premium_LRS"
  },
  "constraints": {
    "allowedTypes": [],
    "excludedTypes": []
  },
  "count": 2,
  "visible": true
}
```

## <a name="remarks"></a>Anmerkungen
- Der Wert für `defaultValue.prefix` ist mit einer Ganzzahl oder mehreren Ganzzahlen verkettet, um die Sequenz der Speicherkontonamen zu generieren. Beispiel: Wenn **foobar** für `defaultValue.prefix` und **2** für `count` angegeben ist, werden die Speicherkontonamen **foobar1** und **foobar2** erstellt. Generierte Speicherkontonamen werden automatisch auf Eindeutigkeit überprüft.
- Die Speicherkontonamen werden lexikografisch basierend auf `count` erstellt. Beispiel: Wenn für `count` der Wert „10“ angegeben ist, enden die Speicherkontonamen auf zweistellige Ganzzahlen (01, 02, 03 usw.).
- Der Standardwert für `defaultValue.prefix` lautet **null** und für `defaultValue.type` **Premium_LRS**.
- Alle nicht in `constraints.allowedTypes` angegebenen Typen werden ausgeblendet, und nicht in `constraints.excludedTypes` angegebene Typen werden angezeigt.
`constraints.allowedTypes` und `constraints.excludedTypes` sind optional, können jedoch nicht gleichzeitig angegeben werden.
- Zusätzlich zur Erstellung von Speicherkontonamen wird `count` zum Festlegen des entsprechenden Multiplikators für das Element verwendet. Das Element unterstützt einen statischen Wert wie **2** oder einen dynamischen Wert aus einem anderen Element, etwa `[steps('step1').storageAccountCount]`. Der Standardwert ist **1**.

## <a name="sample-output"></a>Beispielausgabe
```json
{
  "prefix": "sa",
  "count": 2,
  "resourceGroup": "rg01",
  "type": "Premium_LRS"
}
```

## <a name="next-steps"></a>Nächste Schritte
* Eine Einführung in verwaltete Anwendungen finden Sie in der [Übersicht über verwaltete Azure-Anwendungen](managed-application-overview.md).
* Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](managed-application-createuidefinition-overview.md).
* Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](managed-application-createuidefinition-elements.md).

