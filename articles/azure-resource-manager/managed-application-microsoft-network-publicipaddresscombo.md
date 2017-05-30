---
title: "Benutzeroberflächenelement „PublicIpAddressCombo“ in verwalteten Azure-Anwendungen | Microsoft-Dokumentation"
description: "Hier wird das Benutzeroberflächenelement „Microsoft.Network.PublicIpAddressCombo“ für verwaltete Azure-Anwendungen beschrieben."
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
ms.openlocfilehash: 2eb773f5f0cf389fc39bc3a0f5fbf9ac726d1949
ms.contentlocale: de-de
ms.lasthandoff: 05/12/2017


---
# <a name="microsoftnetworkpublicipaddresscombo-ui-element"></a>Benutzeroberflächenelement „Microsoft.Network.PublicIpAddressCombo“
Eine Gruppe von Steuerelementen zum Auswählen einer neuen oder vorhandenen öffentlichen IP-Adresse. Dieses Element verwenden Sie beim [Erstellen einer verwalteten Azure-Anwendung](managed-application-publishing.md).

## <a name="ui-sample"></a>Benutzeroberflächenbeispiel
![Microsoft.Network.PublicIpAddressCombo](./media/managed-application-elements/microsoft.network.publicipaddresscombo.png)

- Wenn der Benutzer für die öffentliche IP-Adresse „Keine“ auswählt, wird das Textfeld für die Domänennamenbezeichnung ausgeblendet.
- Wenn der Benutzer eine vorhandene öffentliche IP-Adresse auswählt, wird das Textfeld für die Domänennamenbezeichnung deaktiviert. Der Wert ist die Domänennamenbezeichnung der ausgewählten IP-Adresse.
- Das Domänennamensuffix (z.B. „westus.cloudapp.azure.com“) wird basierend auf dem ausgewählten Standort automatisch aktualisiert.

## <a name="schema"></a>Schema
```json
{
  "name": "element1",
  "type": "Microsoft.Network.PublicIpAddressCombo",
  "label": {
    "publicIpAddress": "Public IP address",
    "domainNameLabel": "Domain name label"
  },
  "toolTip": {
    "publicIpAddress": "",
    "domainNameLabel": ""
  },
  "defaultValue": {
    "publicIpAddressName": "ip01",
    "domainNameLabel": "foobar"
  },
  "constraints": {
    "required": {
      "domainNameLabel": true
    }
  },
  "options": {
    "hideNone": false,
    "hideDomainNameLabel": false,
    "hideExisting": false
  },
  "visible": true
}
```

## <a name="remarks"></a>Anmerkungen
- Wenn `constraints.required.domainNameLabel` auf **true** festgelegt ist, muss der Benutzer beim Erstellen einer neuen öffentlichen IP-Adresse eine Domänennamenbezeichnung angeben. Vorhandene öffentliche IP-Adressen ohne Bezeichnung stehen nicht zur Auswahl zur Verfügung.
- Wenn `options.hideNone` auf **true** festgelegt ist, wird die Option zum Auswählen von **Keine** für die öffentlichen IP-Adresse ausgeblendet. Der Standardwert ist **false**.
- Wenn `options.hideDomainNameLabel` auf **true** festgelegt ist, wird das Textfeld für die Domänennamenbezeichnung ausgeblendet. Der Standardwert ist **false**.
- Wenn `options.hideExisting` auf „true“ festgelegt ist, kann der Benutzer keine vorhandene öffentliche IP-Adresse auswählen. Der Standardwert ist **false**.

## <a name="sample-output"></a>Beispielausgabe
Wenn der Benutzer keine öffentliche IP-Adresse auswählt, wird die folgende Ausgabe erwartet:
```json
{
  "newOrExistingOrNone": "none"
}
```

Wenn der Benutzer eine neue oder vorhandene öffentliche IP-Adresse auswählt, wird die folgende Ausgabe erwartet:
```json
{
  "name": "ip01",
  "resourceGroup": "rg01",
  "domainNameLabel": "foobar",
  "newOrExistingOrNone": "new"
}
```
- Wenn `options.hideNone` angegeben wird, gibt `newOrExistingOrNone` immer **Keine** zurück.
- Wenn `options.hideDomainNameLabel` angegeben wird, wird `domainNameLabel` nicht deklariert.

## <a name="next-steps"></a>Nächste Schritte
* Eine Einführung in verwaltete Anwendungen finden Sie in der [Übersicht über verwaltete Azure-Anwendungen](managed-application-overview.md).
* Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](managed-application-createuidefinition-overview.md).
* Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](managed-application-createuidefinition-elements.md).

