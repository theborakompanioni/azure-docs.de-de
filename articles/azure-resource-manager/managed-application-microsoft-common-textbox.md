---
title: "Benutzeroberflächenelement „TextBox“ in verwalteten Azure-Anwendungen | Microsoft-Dokumentation"
description: "Hier wird das Benutzeroberflächenelement „Microsoft.Common.TextBox“ für verwaltete Azure-Anwendungen beschrieben."
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
ms.openlocfilehash: 5a0ac5b811812c8c03f7f63aae12b8699d248ebf
ms.contentlocale: de-de
ms.lasthandoff: 05/12/2017


---

# <a name="microsoftcommontextbox-ui-element"></a>Benutzeroberflächenelement „Microsoft.Common.TextBox UI“
Ein Steuerelement, das zum Bearbeiten von unformatiertem Text verwendet werden kann. Dieses Element verwenden Sie beim [Erstellen einer verwalteten Azure-Anwendung](managed-application-publishing.md).

## <a name="ui-sample"></a>Benutzeroberflächenbeispiel
![Microsoft.Common.TextBox](./media/managed-application-elements/microsoft.common.textbox.png)

## <a name="schema"></a>Schema
```json
{
  "name": "element1",
  "type": "Microsoft.Common.TextBox",
  "label": "Some text box",
  "defaultValue": "foobar",
  "toolTip": "Halp!",
  "constraints": {
    "required": true,
    "regex": "^[a-z0-9A-Z]{1,30}$",
    "validationMessage": "Only alphanumeric characters are allowed, and the value must be 1-30 characters long."
  },
  "visible": true
}
```

## <a name="remarks"></a>Anmerkungen
- Wenn `constraints.required` auf **true** festgelegt ist, muss das Textfeld für eine erfolgreiche Überprüfung einen Wert enthalten. Der Standardwert ist **false**.
- `constraints.regex` ist ein Muster für einen regulären JavaScript-Ausdruck. Wenn der Wert des Textfelds angegeben wird, muss er für eine erfolgreiche Überprüfung dem Muster entsprechen. Der Standardwert lautet **null**.
- `constraints.validationMessage` ist eine Zeichenfolge, die angezeigt wird, wenn bei der Überprüfung des Werts des Textfelds ein Fehler auftritt. Wird kein Wert angegeben, werden die integrierten Überprüfungsnachrichten des Textfelds verwendet. Der Standardwert lautet **null**.
- Für `constraints.regex` kann ein Wert angegeben werden, wenn `constraints.required` auf **false** festgelegt ist. In diesem Szenario ist für eine erfolgreiche Überprüfung des Textfelds kein Wert erforderlich. Wird ein Wert angegeben, muss er dem Muster des regulären Ausdrucks entsprechen.

## <a name="sample-output"></a>Beispielausgabe

```json
"foobar"
```

## <a name="next-steps"></a>Nächste Schritte
* Eine Einführung in verwaltete Anwendungen finden Sie in der [Übersicht über verwaltete Azure-Anwendungen](managed-application-overview.md).
* Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](managed-application-createuidefinition-overview.md).
* Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](managed-application-createuidefinition-elements.md).

