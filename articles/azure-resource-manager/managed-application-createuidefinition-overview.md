---
title: "Grundlegendes zum Erstellen von Benutzeroberflächendefinitionen für verwaltete Azure-Anwendungen | Microsoft-Dokumentation"
description: "Hier wird beschrieben, wie Sie Benutzeroberflächendefinitionen für verwaltete Azure-Anwendungen erstellen."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/11/2017
ms.author: tomfitz
ms.translationtype: Human Translation
ms.sourcegitcommit: 97fa1d1d4dd81b055d5d3a10b6d812eaa9b86214
ms.openlocfilehash: 176b891538f85c5638a2321561c3d8bd377d245b
ms.contentlocale: de-de
ms.lasthandoff: 05/11/2017

---
# <a name="getting-started-with-createuidefinition"></a>Erste Schritte mit „CreateUiDefinition“
In diesem Dokument werden die grundlegenden Konzepte eines CreateUiDefinition-Elements vorgestellt, mit dem das Azure-Portal die Benutzeroberfläche zum Erstellen einer verwalteten Anwendung generiert.

```json
{
   "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json",
   "handler": "Microsoft.Compute.MultiVm",
   "version": "0.1.2-preview",
   "parameters": {
      "basics": [ ],
      "steps": [ ],
      "outputs": { }
   }
}
```

Ein CreateUiDefinition-Element enthält immer drei Eigenschaften: 

* handler
* Version
* Parameter

Bei verwalteten Anwendungen muss für „handler“ immer `Microsoft.Compute.MultiVm` angegeben werden. Die letzte unterstützte Version ist `0.1.2-preview`.

Das Schema der parameters-Eigenschaft hängt von der Kombination aus den angegebenen Werten für „handler“ und „version“ ab. Für verwaltete Anwendungen lauten die unterstützten Eigenschaften `basics`, `steps` und `outputs`. Die Eigenschaften „basics“ und „steps“ enthalten die _Elemente_  (wie Textfelder und Dropdownfelder), die im Azure-Portal angezeigt werden sollen. Mit der outputs-Eigenschaft werden die Ausgabewerte der angegebenen Elemente den Parametern der Azure Resource Manager-Bereitstellungsvorlage zugeordnet.

Die Aufnahme von `$schema` wird empfohlen, ist aber optional. Wenn ein Wert angegeben wird, muss der Wert für `version` der Version im `$schema`-URI entsprechen.

## <a name="basics"></a>Grundlagen
„basics“ ist stets der erste Schritt des Assistenten, der erstellt wird, wenn das Azure-Portal ein CreateUiDefinition-Element analysiert. Das Portal zeigt nicht nur die in `basics` angegebenen Elemente an, sondern fügt zusätzlich Elemente für Benutzer zum Auswählen des Abonnements, der Ressourcengruppe und des Standort für die Bereitstellung ein. Im Allgemeinen sollten Elemente, die bereitstellungsweite Parameter abfragen (wie den Namen eines Clusters oder Administratoranmeldeinformationen), in diesem Schritt enthalten sein.

Falls das Verhalten eines Elements von dem Abonnement, der Ressourcengruppe oder dem Standort eines Benutzers abhängt, kann dieses Element nicht in „basics“ verwendet werden. **Microsoft.Compute.SizeSelector** ist bei der Ermittlung der Liste verfügbarer Größen beispielsweise vom Abonnement und Standort des Benutzers abhängig. Daher kann **Microsoft.Compute.SizeSelector** nur in „steps“ verwendet werden. Im Allgemeinen können nur Elemente im **Microsoft.Common**-Namespace in „basics“ verwendet werden. Dennoch sind einige Elemente in anderen Namespaces (etwa **Microsoft.Compute.Credentials**) zulässig, die nicht vom Kontext des Benutzers abhängig sind.

## <a name="steps"></a>Schritte
Die steps-Eigenschaft kann null oder mehr zusätzliche steps-Elemente für die Anzeige nach „basics“ enthalten, von denen jeder mindestens ein Element enthält. Ziehen Sie das Hinzufügen von steps-Elementen pro Rolle oder Ebene der bereitgestellten Anwendung in Betracht. Fügen Sie beispielsweise ein steps-Element für Eingaben von den Masterknoten und ein steps-Element für die Workerknoten in einem Cluster hinzu.

## <a name="outputs"></a>Ausgaben
Mit der `outputs`-Eigenschaft ordnet das Azure-Portal Elemente aus `basics` und `steps` den Parametern der Azure Resource Manager-Bereitstellungsvorlage zu. Die Schlüssel dieses Wörterbuchs stellen die Namen der Vorlagenparameter dar, und die Werte sind Eigenschaften der Ausgabeobjekte von den referenzierten Elementen.

## <a name="functions"></a>Functions
Ähnlich wie bei [Vorlagenfunktionen](resource-group-template-functions.md) in Azure Resource Manager (sowohl in Bezug auf Syntax als auch auf Funktionalität) stellt das CreateUiDefinition-Element Funktionen zum Arbeiten mit Ein- und Ausgaben von Elementen sowie Features wie konditionelle Abschnitte bereit.

## <a name="next-steps"></a>Nächste Schritte
„CreateUiDefinition“ besitzt selbst ein einfaches Schema. Die tatsächliche Tiefe ergibt sich aus allen unterstützten Elementen und Funktionen, die in den folgenden Dokumenten ausführlich beschrieben werden:

- [CreateUiDefinition-Elemente](managed-application-createuidefinition-elements.md)
- [Funktionen](managed-application-createuidefinition-functions.md)

Ein aktuelles JSON-Schema für „CreateUiDefinition“ ist hier verfügbar: https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json. 

Spätere Versionen werden ebenfalls dort verfügbar sein. Ersetzen Sie den Teil `0.1.2-preview` der URL und den Wert für `version` mit der Versions-ID, die Sie verwenden möchten. Die derzeit unterstützten Versions-IDs sind `0.0.1-preview`, `0.1.0-preview`, `0.1.1-preview` und `0.1.2-preview`.
