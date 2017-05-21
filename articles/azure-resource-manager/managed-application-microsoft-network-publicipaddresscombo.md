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
ms.date: 05/09/2017
ms.author: tabrezm;tomfitz
ms.translationtype: Human Translation
ms.sourcegitcommit: 97fa1d1d4dd81b055d5d3a10b6d812eaa9b86214
ms.openlocfilehash: 71d8249046ed74f67514a93ece4efc2996ad9b4c
ms.contentlocale: de-de
ms.lasthandoff: 05/11/2017


---
# <a name="microsoftnetworkpublicipaddresscombo-ui-element"></a>Benutzeroberflächenelement „Microsoft.Network.PublicIpAddressCombo“
Eine Gruppe von Steuerelementen zum Auswählen einer neuen oder vorhandenen öffentlichen IP-Adresse.

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
* Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](managed-application-createuidefinition-overview.md).
* Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](managed-application-createuidefinition-elements.md).

