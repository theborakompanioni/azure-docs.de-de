---
title: "Benutzeroberflächenelement „CredentialsCombo“ in verwalteten Azure-Anwendungen | Microsoft-Dokumentation"
description: "Hier wird das Benutzeroberflächenelement „Microsoft.Compute.CredentialsCombo“ für verwaltete Azure-Anwendungen beschrieben."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/12/2017
ms.author: tomfitz
ms.translationtype: Human Translation
ms.sourcegitcommit: afa23b1395b8275e72048bd47fffcf38f9dcd334
ms.openlocfilehash: 254f383ee6f7cb9f7051fa135d85319a22c3c369
ms.contentlocale: de-de
ms.lasthandoff: 05/12/2017

---
# <a name="microsoftcomputecredentialscombo-ui-element"></a>Benutzeroberflächenelement „Microsoft.Compute.CredentialsCombo“
Eine Gruppe von Steuerelementen mit integrierter Überprüfung für Windows- und Linux-Kennwörter und öffentliche SSH-Schlüssel. Dieses Element verwenden Sie beim [Erstellen einer verwalteten Azure-Anwendung](managed-application-publishing.md).

## <a name="ui-sample"></a>Benutzeroberflächenbeispiel
![Microsoft.Compute.CredentialsCombo](./media/managed-application-elements/microsoft.compute.credentialscombo.png)

## <a name="schema"></a>Schema
Wenn **Windows** für `osPlatform` festgelegt ist, wird das folgende Schema verwendet:
```json
{
  "name": "element1",
  "type": "Microsoft.Compute.CredentialsCombo",
  "label": {
    "password": "Password",
    "confirmPassword": "Confirm password"
  },
  "toolTip": {
    "password": ""
  },
  "constraints": {
    "required": true,
    "customPasswordRegex": "^(?=.*[A-Za-z])(?=.*\\d)[A-Za-z\\d]{8,}$",
    "customValidationMessage": "The password must contain at least 8 characters, with at least 1 letter and 1 number."
  },
  "options": {
    "hideConfirmation": false
  },
  "osPlatform": "Windows",
  "visible": true
}
```

Wenn **Linux** für `osPlatform` festgelegt ist, wird das folgende Schema verwendet:
```json
{
  "name": "element1",
  "type": "Microsoft.Compute.CredentialsCombo",
  "label": {
    "authenticationType": "Authentication type",
    "password": "Password",
    "confirmPassword": "Confirm password",
    "sshPublicKey": "SSH public key"
  },
  "toolTip": {
    "authenticationType": "",
    "password": "",
    "sshPublicKey": ""
  },
  "constraints": {
    "required": true,
    "customPasswordRegex": "^(?=.*[A-Za-z])(?=.*\\d)[A-Za-z\\d]{8,}$",
    "customValidationMessage": "The password must contain at least 8 characters, with at least 1 letter and 1 number."
  },
  "options": {
    "hideConfirmation": false,
    "hidePassword": false
  },
  "osPlatform": "Linux",
  "visible": true
}
```

## <a name="remarks"></a>Anmerkungen
- `osPlatform` muss angegeben werden. Mögliche Optionen: **Windows** oder **Linux**.
- Wenn `constraints.required` auf **true** festgelegt ist, muss für eine erfolgreiche Überprüfung das Textfeld für das Kennwort oder den öffentlichen SSH-Schlüssel Werte enthalten. Der Standardwert lautet **true**.
- Wenn `options.hideConfirmation` auf **true** festgelegt ist, wird das zweite Textfeld zum Bestätigen des Benutzerkennworts ausgeblendet. Der Standardwert ist **false**.
- Wenn `options.hidePassword` auf **true** festgelegt ist, wird die Option zum Verwenden der Kennwortauthentifizierung ausgeblendet. Sie kann nur verwendet werden, wenn für `osPlatform` das Betriebssystem **Linux** angegeben ist. Der Standardwert ist **false**.
- Zusätzliche Einschränkungen für die zulässigen Kennwörter können mithilfe der `customPasswordRegex`-Eigenschaft implementiert werden. Die Zeichenfolge in `customValidationMessage` wird angezeigt, wenn bei der benutzerdefinierten Überprüfung des Kennworts ein Fehler auftritt. Der Standardwert für beide Eigenschaften ist **null**.

## <a name="sample-output"></a>Beispielausgabe
Wenn für `osPlatform` das Betriebssystem **Windows** angegeben ist oder der Benutzer ein Kennwort anstelle eines öffentlichen SSH-Schlüssels eingegeben hat, wird die folgende Ausgabe erwartet:

```json
{
  "authenticationType": "password",
  "password": "p4ssw0rd",
}
```

Wenn der Benutzer einen öffentlichen SSH-Schlüssel angegeben hat, wird die folgende Ausgabe erwartet:
```json
{
  "authenticationType": "sshPublicKey",
  "sshPublicKey": "AAAAB3NzaC1yc2EAAAABIwAAAIEA1on8gxCGJJWSRT4uOrR13mUaUk0hRf4RzxSZ1zRbYYFw8pfGesIFoEuVth4HKyF8k1y4mRUnYHP1XNMNMJl1JcEArC2asV8sHf6zSPVffozZ5TT4SfsUu/iKy9lUcCfXzwre4WWZSXXcPff+EHtWshahu3WzBdnGxm5Xoi89zcE=",
}
```

## <a name="next-steps"></a>Nächste Schritte
* Eine Einführung in verwaltete Anwendungen finden Sie in der [Übersicht über verwaltete Azure-Anwendungen](managed-application-overview.md).
* Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](managed-application-createuidefinition-overview.md).
* Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](managed-application-createuidefinition-elements.md).

