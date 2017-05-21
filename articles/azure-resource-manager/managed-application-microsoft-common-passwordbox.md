---
title: "Benutzeroberflächenelement „PasswordBox“ in verwalteten Azure-Anwendungen | Microsoft-Dokumentation"
description: "Hier wird das Benutzeroberflächenelement „Microsoft.Common.PasswordBox“ für verwaltete Azure-Anwendungen beschrieben."
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
ms.openlocfilehash: 4646521531e839e4751b4f01f7a66876dac96b57
ms.contentlocale: de-de
ms.lasthandoff: 05/11/2017


---
# <a name="microsoftcommonpasswordbox-ui-element"></a>Benutzeroberflächenelement „Microsoft.Common.PasswordBox“
Ein Steuerelement, das zum Angeben und Bestätigen eines Kennworts verwendet werden kann.

## <a name="ui-sample"></a>Benutzeroberflächenbeispiel
![Microsoft.Common.PasswordBox](./media/managed-application-elements/microsoft.common.passwordbox.png)

## <a name="schema"></a>Schema
```json
{
  "name": "element1",
  "type": "Microsoft.Common.PasswordBox",
  "label": {
    "password": "Password",
    "confirmPassword": "Confirm password"
  },
  "toolTip": "",
  "constraints": {
    "required": true,
    "regex": "",
    "validationMessage": ""
  },
  "options": {
    "hideConfirmation": false
  },
  "visible": true
}
```

## <a name="remarks"></a>Anmerkungen
- Dieses Element unterstützt die `defaultValue`-Eigenschaft nicht.
- Implementierungsdetails zu `constraints` finden Sie unter [Microsoft.Common.TextBox](managed-application-microsoft-common-textbox.md).
- Wenn `options.hideConfirmation` auf **true** festgelegt ist, wird das zweite Textfeld zum Bestätigen des Benutzerkennworts ausgeblendet. Der Standardwert ist **false**.

## <a name="sample-output"></a>Beispielausgabe
```json
"p4ssw0rd"
```

## <a name="next-steps"></a>Nächste Schritte
* Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](managed-application-createuidefinition-overview.md).
* Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](managed-application-createuidefinition-elements.md).

