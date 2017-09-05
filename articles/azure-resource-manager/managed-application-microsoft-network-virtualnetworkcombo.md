---
title: "Benutzeroberflächenelement „VirtualNetworkCombo“ in verwalteten Azure-Anwendungen | Microsoft-Dokumentation"
description: "Hier wird das Benutzeroberflächenelement „Microsoft.Network.VirtualNetworkCombo“ für verwaltete Azure-Anwendungen beschrieben."
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
ms.openlocfilehash: 8bb255b76ac5c3de570fa569a1cfb3ee953f9687
ms.contentlocale: de-de
ms.lasthandoff: 05/12/2017

---
# <a name="microsoftnetworkvirtualnetworkcombo-ui-element"></a>Benutzeroberflächenelement „Microsoft.Network.VirtualNetworkCombo“
Eine Gruppe von Steuerelementen zum Auswählen eines neuen oder vorhandenen virtuellen Netzwerks. Dieses Element verwenden Sie beim [Erstellen einer verwalteten Azure-Anwendung](managed-application-publishing.md).

## <a name="ui-sample"></a>Benutzeroberflächenbeispiel
![Microsoft.Network.VirtualNetworkCombo](./media/managed-application-elements/microsoft.network.virtualnetworkcombo.png)

- Im oberen Drahtmodell hat der Benutzer ein neues virtuelles Netzwerk ausgewählt, sodass er den Namen und das Adresspräfix der einzelnen Subnetze anpassen kann. Die Konfiguration von Subnetzen ist in diesem Fall optional.
- Im unteren Drahtmodell hat der Benutzer ein vorhandenes virtuelles Netzwerk ausgewählt, daher muss er jedes von der Bereitstellungsvorlage geforderte Subnetz einem vorhandenen Subnetz zuordnen. Die Konfiguration von Subnetzen ist in diesem Fall erforderlich.

## <a name="schema"></a>Schema
```json
{
  "name": "element1",
  "type": "Microsoft.Network.VirtualNetworkCombo",
  "label": {
    "virtualNetwork": "Virtual network",
    "subnets": "Subnets"
  },
  "toolTip": {
    "virtualNetwork": "",
    "subnets": ""
  },
  "defaultValue": {
    "name": "vnet01",
    "addressPrefixSize": "/16"
  },
  "constraints": {
    "minAddressPrefixSize": "/16"
  },
  "options": {
    "hideExisting": false
  },
  "subnets": {
    "subnet1": {
      "label": "First subnet",
      "defaultValue": {
        "name": "subnet-1",
        "addressPrefixSize": "/24"
      },
      "constraints": {
        "minAddressPrefixSize": "/24",
        "minAddressCount": 12,
        "requireContiguousAddresses": true
      }
    },
    "subnet2": {
      "label": "Second subnet",
      "defaultValue": {
        "name": "subnet-2",
        "addressPrefixSize": "/26"
      },
      "constraints": {
        "minAddressPrefixSize": "/26",
        "minAddressCount": 8,
        "requireContiguousAddresses": true
      }
    }
  },
  "visible": true
}
```

## <a name="remarks"></a>Anmerkungen
- Wenn angegeben, wird das erste Adresspräfix ohne Überschneidung mit der Größe `defaultValue.addressPrefixSize` automatisch basierend auf den vorhandenen virtuellen Netzwerken im Abonnement des Benutzers ermittelt.
- Der Standardwert für `defaultValue.name` und `defaultValue.addressPrefixSize` lautet **null**.
- `constraints.minAddressPrefixSize` muss angegeben werden. Vorhandene virtuelle Netzwerke, deren Adressbereich kleiner als der angegebene Wert ist, können nicht ausgewählt werden.
- `subnets` muss angegeben werden, und für jedes Subnetz muss `constraints.minAddressPrefixSize` angegeben werden.
- Bei der Erstellung eines neuen virtuellen Netzwerks wird das Adresspräfix für jedes Subnetz automatisch basierend auf dem Adresspräfix und dem jeweiligen `addressPrefixSize`-Element des virtuellen Netzwerks berechnet.
- Bei der Verwendung eines vorhandenen virtuellen Netzwerks können keine Subnetze ausgewählt werden, die kleiner als das entsprechende `constraints.minAddressPrefixSize`-Element sind. Darüber hinaus können bei Angabe des Werts Subnetze, die nicht mindestens `minAddressCount` verfügbare Adressen enthalten, nicht ausgewählt werden.
Der Standardwert ist **0**. Um sicherzustellen, dass die verfügbaren Adressen zusammenhängend sind, geben Sie **true** für `requireContiguousAddresses` an. Der Standardwert lautet **true**.
- Die Erstellung von Subnetzen in einem vorhandenen virtuellen Netzwerk wird nicht unterstützt.
- Wenn **true** für `options.hideExisting` angegeben ist, kann der Benutzer kein vorhandenes virtuelles Netzwerk auswählen. Der Standardwert ist **false**.

## <a name="sample-output"></a>Beispielausgabe
```json
{
  "name": "vnet01",
  "resourceGroup": "rg01",
  "addressPrefixes": ["10.0.0.0/16"],
  "newOrExisting": "new",
  "subnets": {
    "subnet1": {
      "name": "subnet-1",
      "addressPrefix": "10.0.0.0/24",
      "startAddress": "10.0.0.1"
    },
    "subnet2": {
      "name": "subnet-2",
      "addressPrefix": "10.0.1.0/26",
      "startAddress": "10.0.1.1"
    }
  }
}
```

## <a name="next-steps"></a>Nächste Schritte
* Eine Einführung in verwaltete Anwendungen finden Sie in der [Übersicht über verwaltete Azure-Anwendungen](managed-application-overview.md).
* Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](managed-application-createuidefinition-overview.md).
* Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](managed-application-createuidefinition-elements.md).

