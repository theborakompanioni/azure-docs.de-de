---
title: "Benutzeroberflächenelement „Section“ in verwalteten Azure-Anwendungen | Microsoft-Dokumentation"
description: "Hier wird das Benutzeroberflächenelement „Microsoft.Common.Section“ für verwaltete Azure-Anwendungen beschrieben."
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
ms.openlocfilehash: 7111b02432a774e056b5a5e13f93ae7f11fb6848
ms.contentlocale: de-de
ms.lasthandoff: 05/11/2017


---
# <a name="microsoftcommonsection-ui-element"></a>Benutzerobeflächenelement „Microsoft.Common.Section“
Ein Steuerelement, das Elemente unter einer Überschrift zusammenfasst.

## <a name="ui-sample"></a>Benutzeroberflächenbeispiel
![Microsoft.Common.Section](./media/managed-application-elements/microsoft.common.section.png)

## <a name="schema"></a>Schema
```json
{
  "name": "section1",
  "type": "Microsoft.Common.Section",
  "label": "Some section",
  "elements": [
    {
      "name": "element1",
      "type": "Microsoft.Common.TextBox",
      "label": "Some text box 1"
    },
    {
      "name": "element2",
      "type": "Microsoft.Common.TextBox",
      "label": "Some text box 2"
    }
  ],
  "visible": true
}
```

## <a name="remarks"></a>Anmerkungen
- `elements` muss mindestens ein Element und darf alle Elementtypen außer `Microsoft.Common.Section` enthalten.
- Dieses Element unterstützt die `toolTip`-Eigenschaft nicht.

## <a name="sample-output"></a>Beispielausgabe
Verwenden Sie zum Zugreifen auf die Ausgabewerte von Elementen in `elements` die Funktion [basics()](managed-application-createuidefinition-functions.md#basics) oder [steps()](managed-application-createuidefinition-functions.md#steps) und die Punktnotation:

```json
basics('section1').element1
```

Elemente des Typs `Microsoft.Common.Section` haben selbst keine Ausgabewerte.

## <a name="next-steps"></a>Nächste Schritte
* Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](managed-application-createuidefinition-overview.md).
* Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](managed-application-createuidefinition-elements.md).

