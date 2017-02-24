---
title: Azure-Ressourcenrichtlinien | Microsoft-Dokumentation
description: "Beschreibt, wie Sie mithilfe von Azure Resource Manager-Richtlinien sicherstellen, dass bei der Bereitstellung konsistente Ressourceneigenschaften festgelegt werden. Richtlinien können auf das Abonnement oder auf Ressourcengruppen angewendet werden."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: abde0f73-c0fe-4e6d-a1ee-32a6fce52a2d
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/10/2017
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: 6d459e37b8b39f5d76c4ec86ebb7351c783b81fb
ms.openlocfilehash: 64cb4be184e02519a6c496f8639035201ebb60f8


---
# <a name="resource-policy-overview"></a>Übersicht über Ressourcenrichtlinien
Mithilfe von Ressourcenrichtlinien können Sie Konventionen für Ressourcen in Ihrer Organisation einrichten. Durch Definieren von Konventionen können Sie Kosten beeinflussen und Ihre Ressourcen einfacher verwalten. So können Sie beispielsweise angeben, dass nur bestimmte Arten von virtuellen Computern zulässig sind, oder festlegen, dass alle Ressourcen über ein bestimmtes Tag verfügen müssen. Richtlinien werden von allen untergeordneten Ressourcen geerbt. Wenn also eine Richtlinie auf eine Ressourcengruppe angewendet wird, gilt sie auch für alle Ressourcen in der Ressourcengruppe.

Im Zusammenhang mit Richtlinien gibt es zwei wichtige Konzepte:

* Richtliniendefinition: Sie beschreiben, wann die Richtlinie erzwungen wird und welche Aktion ausgeführt werden soll.
* Richtlinienzuweisung: Sie wenden die Richtliniendefinition auf einen Bereich (Abonnement oder Ressourcengruppe) an.

Dieses Thema beschäftigt sich mit der Richtliniendefinition. Informationen zur Richtlinienzuweisung finden Sie unter [Zuweisen und Verwalten von Richtlinien](resource-manager-policy-create-assign.md).

Azure stellt einige integrierte Richtliniendefinitionen bereit, um ggf. die Anzahl der zu definierenden Richtlinien zu verringern. Falls eine dieser integrierten Richtliniendefinitionen für Ihr Szenario geeignet ist, verwenden Sie diese Definition bei der Zuweisung zu einem Bereich.

Richtlinien werden beim Erstellen und Aktualisieren von Ressourcen (PUT- und PATCH-Vorgänge) ausgewertet.

> [!NOTE]
> Die Richtlinie wertet derzeit keine Ressourcentypen aus, die „tags“, „kind“ und „location“ nicht unterstützen, wie etwa der Ressourcentyp „Microsoft.Resources/deployments“. Diese Unterstützung wird zu einem späteren Zeitpunkt hinzugefügt. Um Probleme mit der Abwärtskompatibilität zu vermeiden, sollten Sie beim Erstellen von Richtlinien den Typ explizit angeben. Beispiel: Eine Richtlinie für Tags, in der keine Typen angegeben sind, wird auf alle Typen angewendet. In diesem Fall kann eine Vorlagenbereitstellung fehlschlagen, wenn eine geschachtelte Ressource vorhanden ist, die Tags nicht unterstützt, und der Ressourcentyp der Bereitstellung der Richtlinienauswertung hinzugefügt wurde. 
> 
> 

## <a name="how-is-it-different-from-rbac"></a>Worin unterscheidet sich dies von der rollenbasierten Zugriffssteuerung (RBAC)?
Zwischen einer Richtlinie und der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) gibt es einige entscheidende Unterschiede. Bei RBAC stehen **Benutzeraktionen** in verschiedenen Bereichen im Mittelpunkt. Beispiel: Sie werden der Rolle „Mitwirkender“ für eine Ressourcengruppe in einem bestimmten Bereich hinzugefügt, um Änderungen an der Ressourcengruppe vornehmen zu können. Bei einer Richtlinie stehen **Ressourceneigenschaften** während der Bereitstellung im Mittelpunkt. Beispielsweise können Sie über Richtlinien die Arten von Ressourcen steuern, die bereitgestellt werden können, oder die Standorte beschränken, an denen die Ressourcen bereitgestellt werden können. Im Gegensatz zur RBAC stellen Richtlinien ein Standardsystem für das Zulassen und explizite Verweigern dar. 

Zum Verwenden von Richtlinien müssen Sie über die RBAC authentifiziert sein. Ihr Konto benötigt insbesondere Folgendes:

* Die Berechtigung `Microsoft.Authorization/policydefinitions/write` zum Definieren einer Richtlinie
* Die Berechtigung `Microsoft.Authorization/policyassignments/write` zum Zuweisen einer Richtlinie 

Diese Berechtigungen sind in der Rolle **Mitwirkender** nicht enthalten.

## <a name="policy-definition-structure"></a>Struktur von Richtliniendefinitionen
Eine Richtliniendefinition wird mithilfe von JSON erstellt. Die Richtliniendefinition enthält Elemente für Folgendes:

* Parameter
* Anzeigename
* Beschreibung
* Richtlinienregel
  * Logische Auswertung
  * Wirkung

Das folgende Beispiel zeigt eine Richtlinie, die einschränkt, wo Ressourcen bereitgestellt werden:

```json
{
  "properties": {
    "parameters": {
      "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "The list of locations that can be specified when deploying resources",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
      }
    },
    "displayName": "Allowed locations",
    "description": "This policy enables you to restrict the locations your organization can specify when deploying resources.",
    "policyRule": {
      "if": {
        "not": {
          "field": "location",
          "in": "[parameters('allowedLocations')]"
        }
      },
      "then": {
        "effect": "deny"
      }
    }
  }
}
```

## <a name="parameters"></a>Parameter
Das Verwenden von Parametern vereinfacht die Richtlinienverwaltung, da die Anzahl von Richtliniendefinitionen reduziert wird. Sie definieren eine Richtlinie für eine Ressourceneigenschaft (beispielsweise, um die Orte einzuschränken, an denen Ressourcen bereitgestellt werden können) und schließen Parameter in die Definition ein. Diese Richtliniendefinition verwenden Sie dann auch für andere Szenarien, indem Sie beim Zuweisen der Richtlinie andere Werte (etwa eine Gruppe von Standorten für ein Abonnement) übergeben.

Sie deklarieren die Parameter beim Erstellen von Richtliniendefinitionen.

```json
"parameters": {
  "allowedLocations": {
    "type": "array",
    "metadata": {
      "description": "The list of allowed locations for resources.",
      "displayName": "Allowed locations"
    }
  }
}
```

Der Typ eines Parameters kann Zeichenfolge oder Array sein. Die Metadateneigenschaft wird für Tools wie das Azure-Portal verwendet, um benutzerfreundliche Informationen anzuzeigen. 

In der Richtlinienregel wird die folgende Syntax verwendet, um auf Parameter zu verweisen: 

```json
{ 
    "field": "location",
    "in": "[parameters('allowedLocations')]"
}
```

## <a name="display-name-and-description"></a>Anzeigename und Beschreibung

**displayName** und **description** dienen zum Identifizieren der Richtliniendefinition und geben den Kontext für die Verwendung an.

## <a name="policy-rule"></a>Richtlinienregel

Die Richtlinienregel besteht aus **If**- und **Then**-Blöcken. Im **If**-Block definieren Sie mindestens eine Bedingung, die angibt, wann die Richtlinie erzwungen wird. Auf diese Bedingungen können logische Operatoren angewendet werden, um das Szenario für eine Richtlinie präzise zu definieren. Im **Then**-Block definieren Sie die Wirkung, die eintritt, wenn die **If**-Bedingungen erfüllt sind.

```json
{
  "if": {
    <condition> | <logical operator>
  },
  "then": {
    "effect": "deny | audit | append"
  }
}
```

### <a name="logical-operators"></a>Logische Operatoren
Folgende logische Operatoren werden unterstützt:

* `"not": {condition  or operator}`
* `"allOf": [{condition or operator},{condition or operator}]`
* `"anyOf": [{condition or operator},{condition or operator}]`

Die **not**-Syntax kehrt das Ergebnis der Bedingung um. Die **allOf**-Syntax gleicht der logischen **And**-Operation und erfordert, dass alle Bedingungen erfüllt sind. Die **anyOf**-Syntax gleicht der logischen **Or**-Operation und erfordert, dass mindestens eine Bedingung erfüllt ist.

Logische Operatoren können geschachtelt werden. Das folgende Beispiel zeigt eine geschachtelte **not**-Operation innerhalb einer **And**-Operation. 

```json
"if": {
  "allOf": [
    {
      "not": {
        "field": "tags",
        "containsKey": "application"
      }
    },
    {
      "field": "type",
      "equals": "Microsoft.Storage/storageAccounts"
    }
  ]
},
```

### <a name="conditions"></a>Bedingungen
Die Bedingung überprüft, ob ein **Feld** bestimmte Kriterien erfüllt. Folgende Bedingungen werden unterstützt:

* `"equals": "value"`
* `"like": "value"`
* `"contains": "value"`
* `"in": ["value1","value2"]`
* `"containsKey": "keyName"`
* `"exists": "bool"`

Bei Verwendung der Bedingung **like** können Sie im Wert einen Platzhalter (*) angeben.

### <a name="fields"></a>Felder
Bedingungen werden mithilfe von Feldern gebildet. Ein Feld stellt Eigenschaften in der Anforderungsnutzlast einer Ressource dar, mit der der Zustand der Ressource beschrieben wird.  

Folgende Felder werden unterstützt:

* `name`
* `kind`
* `type`
* `location`
* `tags`
* `tags.*` 
* Eigenschaftenaliase

Eigenschaftenaliase dienen zum Zugreifen auf bestimmte Eigenschaften für einen Ressourcentyp. Folgende Aliase werden unterstützt:

* Microsoft.CDN/profiles/sku.name
* Microsoft.Compute/virtualMachines/imageOffer
* Microsoft.Compute/virtualMachines/imagePublisher
* Microsoft.Compute/virtualMachines/sku.name
* Microsoft.Compute/virtualMachines/imageSku 
* Microsoft.Compute/virtualMachines/imageVersion
* Microsoft.SQL/servers/databases/edition
* Microsoft.SQL/servers/databases/elasticPoolName
* Microsoft.SQL/servers/databases/requestedServiceObjectiveId
* Microsoft.SQL/servers/databases/requestedServiceObjectiveName
* Microsoft.SQL/servers/elasticPools/dtu
* Microsoft.SQL/servers/elasticPools/edition
* Microsoft.SQL/servers/version
* Microsoft.Storage/storageAccounts/accessTier
* Microsoft.Storage/storageAccounts/enableBlobEncryption
* Microsoft.Storage/storageAccounts/sku.name
* Microsoft.Web/serverFarms/sku.name

### <a name="effect"></a>Wirkung
Für eine Richtlinie werden drei Arten von Wirkungen unterstützt: `deny`, `audit` und `append`. 

* **Deny** generiert ein Ereignis im Überwachungsprotokoll, und die Anforderung ist nicht erfolgreich.
* **Audit** generiert eine Warnung im Überwachungsprotokoll, und die Anforderung ist erfolgreich.
* **Append** fügt der Anforderung den definierten Satz von Feldern hinzu. 

Für **append**müssen Sie die folgenden Details angeben:

```json
"effect": "append",
"details": [
  {
    "field": "field name",
    "value": "value of the field"
  }
]
```

Der Wert kann entweder eine Zeichenfolge oder ein Objekt im JSON-Format sein. 

## <a name="policy-examples"></a>Beispiele für Richtlinien

Die folgenden Themen enthalten Beispiele für Richtlinien:

* Beispiele für Tagrichtlinien finden Sie unter [Anwenden von Ressourcenrichtlinien für Tags](resource-manager-policy-tags.md).
* Beispiele für Speicherrichtlinien finden Sie unter [Anwenden von Ressourcenrichtlinien auf Speicherkonten](resource-manager-policy-storage.md).
* Beispiele für VM-Richtlinien finden Sie unter [Anwenden von Sicherheit und Richtlinien auf virtuelle Linux-Computer mit Azure Resource Manager](../virtual-machines/virtual-machines-linux-policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json) sowie unter [Anwenden von Sicherheit und Richtlinien auf virtuelle Windows-Computer mit Azure Resource Manager](../virtual-machines/virtual-machines-windows-policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json).

### <a name="allowed-resource-locations"></a>Zulässige Ressourcenstandorte
Informationen zum Angeben der zulässigen Standorte finden Sie im Beispiel des Abschnitts [Struktur von Richtliniendefinitionen](#policy-definition-structure). Verwenden Sie zum Hinzufügen dieser Richtliniendefinition die integrierte Richtlinie mit der Ressourcen-ID `/providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c`.

### <a name="not-allowed-resource-locations"></a>Unzulässige Ressourcenstandorte
Mit der folgenden Richtliniendefinition können Sie angeben, welche Standorte nicht zulässig sind:

```json
{
  "properties": {
    "parameters": {
      "notAllowedLocations": {
        "type": "array",
        "metadata": {
          "description": "The list of locations that are not allowed when deploying resources",
          "strongType": "location",
          "displayName": "Not allowed locations"
        }
      }
    },
    "displayName": "Not allowed locations",
    "description": "This policy enables you to block locations that your organization can specify when deploying resources.",
    "policyRule": {
      "if": {
        "field": "location",
        "in": "[parameters('notAllowedLocations')]"
      },
      "then": {
        "effect": "deny"
      }
    }
  }
}
```

### <a name="allowed-resource-types"></a>Zulässige Ressourcentypen
Das folgende Beispiel zeigt eine Richtlinie, die nur Bereitstellungen für die Ressourcentypen „Microsoft.Resources“, „Microsoft.Compute“, „Microsoft.Storage“ und „Microsoft.Network“ zulässt. Alle anderen werden abgelehnt:

```json
{
  "if": {
    "not": {
      "anyOf": [
        {
          "field": "type",
          "like": "Microsoft.Resources/*"
        },
        {
          "field": "type",
          "like": "Microsoft.Compute/*"
        },
        {
          "field": "type",
          "like": "Microsoft.Storage/*"
        },
        {
          "field": "type",
          "like": "Microsoft.Network/*"
        }
      ]
    }
  },
  "then": {
    "effect": "deny"
  }
}
```

### <a name="set-naming-convention"></a>Festlegen der Benennungskonvention
Das folgende Beispiel veranschaulicht die Verwendung von Platzhalterzeichen, die von der Bedingung **like** unterstützt werden. Die Bedingung gibt an, dass die Anforderung verweigert wird, wenn der Name nicht mit dem angegebenen Muster (namePrefix\*nameSuffix) übereinstimmt:

```json
{
  "if": {
    "not": {
      "field": "name",
      "like": "namePrefix*nameSuffix"
    }
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="next-steps"></a>Nächste Schritte
* Nach dem Definieren einer Richtlinienregel weisen Sie sie einem Bereich zu. Informationen zur Richtlinienzuweisung finden Sie unter [Zuweisen und Verwalten von Richtlinien](resource-manager-policy-create-assign.md).
* Anleitungen dazu, wie Unternehmen Abonnements mit Resource Manager effektiv verwalten können, finden Sie unter [Azure-Unternehmensgerüst - Präskriptive Abonnementgovernance](resource-manager-subscription-governance.md).
* Das Richtlinienschema wird unter [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json)veröffentlicht. 




<!--HONumber=Feb17_HO3-->


