---
title: Azure Resource Manager-Richtlinien | Microsoft Docs
description: "Beschreibt die Verwendung von Azure-Ressourcen-Manager-Richtlinien, um Verstöße in unterschiedlichen Gültigkeitsbereichen wie Abonnements, Ressourcengruppen oder einzelnen Ressourcen zu verhindern."
services: azure-resource-manager
documentationcenter: na
author: ravbhatnagar
manager: timlt
editor: tysonn
ms.assetid: abde0f73-c0fe-4e6d-a1ee-32a6fce52a2d
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/30/2016
ms.author: gauravbh;tomfitz
translationtype: Human Translation
ms.sourcegitcommit: e841c21a15c47108cbea356172bffe766003a145
ms.openlocfilehash: bdc759341e1f9707ddf688512249c3297d85c29b


---
# <a name="use-policy-to-manage-resources-and-control-access"></a>Verwenden von Richtlinien für Ressourcenverwaltung und Zugriffssteuerung
Sie können nun den Azure-Ressourcen-Manager zum Steuern des Zugriffs über benutzerdefinierte Richtlinien verwenden. Mithilfe von Richtlinien können Sie Benutzer in Ihrer Organisation hindern, gegen Konventionen zu verstoßen, die für das Verwalten der Ressourcen Ihrer Organisation gelten. 

Sie erstellen Richtliniendefinitionen, die die Aktionen oder Ressourcen beschreiben, die spezifisch verweigert werden. Sie weisen diese Richtliniendefinitionen dem gewünschten Ziel zu, z. B. einem Abonnement, einer Ressourcengruppe oder einer einzelnen Ressource. Richtlinien werden von allen untergeordneten Ressourcen geerbt. Wenn also eine Richtlinie auf eine Ressourcengruppe angewendet wird, gilt sie auch für alle Ressourcen in der Ressourcengruppe.

In diesem Artikel wird die grundlegende Struktur der Richtliniendefinitionssprache erläutert, mit der Sie Richtlinien erstellen. Anschließend wird beschrieben, wie Sie diese Richtlinien in verschiedenen Bereichen anwenden.

## <a name="how-is-it-different-from-rbac"></a>Worin unterscheidet sich dies von der rollenbasierten Zugriffssteuerung (RBAC)?
Es gibt einige wichtige Unterschiede zwischen Richtlinien und rollenbasierter Zugriffssteuerung. Als Erstes müssen Sie jedoch verstehen, dass Richtlinien und RBAC zusammenarbeiten. Zum Verwenden von Richtlinien müssen Sie über die RBAC authentifiziert sein. Im Gegensatz zur RBAC stellen Richtlinien ein Standardsystem für das Zulassen und explizite Verweigern dar. 

Die RBAC konzentriert sich auf die Aktionen, die ein **Benutzer** in verschiedenen Bereichen ausführen darf. Beispiel: Ein bestimmter Benutzer wird der Rolle „Mitwirkender“ für eine Ressourcengruppe in einem bestimmten Bereich hinzugefügt, sodass der Benutzer Änderungen an der Ressourcengruppe vornehmen darf. 

Richtlinien konzentrieren sich auf Aktionen für **Ressourcen** in verschiedenen Bereichen. Beispielsweise können Sie über Richtlinien die Arten von Ressourcen steuern, die bereitgestellt werden können, oder die Standorte beschränken, an denen die Ressourcen bereitgestellt werden können.

## <a name="common-scenarios"></a>Gängige Szenarien
Ein allgemeines Szenario ist der Bedarf an Tags auf Abteilungsebene zum Zweck der verbrauchsbasierten Kostenzuteilung. Möglicherweise möchte eine Organisation Vorgänge nur zulassen, wenn die richtige Kostenstelle zugeordnet ist, andernfalls wird die Anfrage abgelehnt. Dann hilft die Richtlinie dabei, die Abrechnung für die durchgeführten Vorgänge mit der richtigen Kostenstelle durchzuführen.

In einem weiteren gängigen Szenario möchte die Organisation die Standorte steuern, an denen Ressourcen erstellt werden. Oder sie möchte möglicherweise den Zugriff auf Ressourcen steuern, indem nur bestimmte Arten von Ressourcen bereitgestellt werden dürfen.

Auf ähnliche Weise kann eine Organisation den Dienstkatalog steuern oder die gewünschten Benennungskonventionen für Ressourcen erzwingen.

Mithilfe von Richtlinien können diese Szenarios problemlos umgesetzt werden.

## <a name="policy-definition-structure"></a>Struktur von Richtliniendefinitionen
Richtliniendefinitionen werden mit JSON erstellt. Sie enthalten eine oder mehrere Bedingungen/logische Operatoren, die Aktionen und deren Auswirkungen definieren und damit festlegen, was geschieht, wenn die Bedingungen erfüllt sind. Das Schema wird unter [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json)veröffentlicht. 

Grundsätzlich enthält eine Richtlinie folgende Elemente:

**Bedingung/logische Operatoren:** eine Reihe von Bedingungen, die über einen Satz von logischen Operatoren bearbeitet werden können.

**Auswirkung:** beschreibt die Auswirkungen, wenn die Bedingung erfüllt ist – verweigern oder überwachen. Ein Überwachungseffekt gibt ein Warnereignis im Dienstprotokoll aus. Administratoren können z.B. eine Richtlinie erstellen, die eine Überwachung verursacht, wenn jemand einen großen virtuellen Computer erstellt. Der Administrator kann die Protokolle später überprüfen.

    {
      "if" : {
          <condition> | <logical operator>
      },
      "then" : {
          "effect" : "deny | audit | append"
      }
    }

## <a name="policy-evaluation"></a>Richtlinienauswertung
Richtlinien werden bei der Erstellung von Ressourcen ausgewertet. Für die Bereitstellung von Vorlagen werden Richtlinien während der Erstellung der einzelnen Ressourcen in einer Vorlage ausgewertet. 

> [!NOTE]
> Die Richtlinie wertet derzeit keine Ressourcentypen aus, die „tags“, „kind“ und „location“ nicht unterstützen, wie etwa der Ressourcentyp „Microsoft.Resources/deployments“. Diese Unterstützung wird zu einem späteren Zeitpunkt hinzugefügt. Um Probleme mit der Abwärtskompatibilität zu vermeiden, sollten Sie beim Erstellen von Richtlinien den Typ explizit angeben. Beispiel: Eine Richtlinie für Tags, in der keine Typen angegeben sind, wird auf alle Typen angewendet. In diesem Fall kann eine Vorlagenbereitstellung fehlschlagen, wenn eine geschachtelte Ressource vorhanden ist, die Tags nicht unterstützt, und der Ressourcentyp der Bereitstellung der Richtlinienauswertung hinzugefügt wurde. 
> 
> 

## <a name="logical-operators"></a>Logische Operatoren
Die unterstützten logischen Operatoren sind zusammen mit der Syntax nachfolgend aufgeführt:

| Name des Operators | Syntax |
|:--- |:--- |
| not |"not" : {&lt;Bedingung oder Operator &gt;} |
| und |"allOf" : [ {&lt;Bedingung oder Operator &gt;},{&lt;Bedingung oder Operator &gt;}] |
| oder |"anyOf" : [ {&lt;Bedingung oder Operator &gt;},{&lt;Bedingung oder Operator &gt;}] |

Mit dem Ressourcen-Manager können Sie über geschachtelte Operatoren eine komplexe Logik in Ihrer Richtlinie angeben. Sie können z. B. das Erstellen von Ressourcen an einem bestimmten Speicherort für einen angegebenen Ressourcentyp verweigern. Ein Beispiel für geschachtelte Operatoren ist in diesem Thema dargestellt.

## <a name="conditions"></a>Bedingungen
Eine Bedingung prüft, ob ein **Feld** oder eine **Quelle** bestimmte Kriterien erfüllt. Die Namen unterstützter Bedingungen sind zusammen mit der Syntax nachstehend aufgeführt:

| Name der Bedingung | Syntax |
|:--- |:--- |
| Equals |"equals" : "&lt;value&gt;" |
| Like |"like" : "&lt;value&gt;" |
| Contains |"contains" : "&lt;value&gt;" |
| Geben Sie in |"in" : [ "&lt;value1&gt;","&lt;value2&gt;" ] |
| ContainsKey |"containsKey" : "&lt;keyName&gt;" |
| Exists |"exists" : "&lt;bool&gt;" |

### <a name="fields"></a>Felder
Bedingungen werden mithilfe von Feldern und Quellen gebildet. Ein Feld stellt Eigenschaften in der Anforderungsnutzlast einer Ressource dar, mit der der Zustand der Ressource beschrieben wird. Eine Quelle stellt Merkmale der Anforderung selbst dar. 

Die folgenden Felder und Quellen werden unterstützt:

Felder: **name**, **kind**, **type**, **location**, **tags**, **tags.*** und **property alias**. 

### <a name="property-aliases"></a>Eigenschaftenaliase
Ein Eigenschaftenalias ist ein Name, der in einer Richtliniendefinition für den Zugriff auf die für einen Ressourcentyp spezifischen Eigenschaften (z.B. Einstellungen und SKUs) verwendet werden kann. Er kann in allen API-Versionen verwendet werden, in denen die Eigenschaft vorhanden ist. Sie können Aliase mithilfe der folgenden REST-API abrufen (PowerShell-Unterstützung wird in Zukunft hinzugefügt):

    GET /subscriptions/{id}/providers?$expand=resourceTypes/aliases&api-version=2015-11-01

Das folgende Beispiel zeigt eine Definition eines Alias. Wie Sie sehen können, definiert ein Alias Pfade in verschiedenen API-Versionen, auch wenn sich der Name einer Eigenschaft ändert. 

    "aliases": [
        {
          "name": "Microsoft.Storage/storageAccounts/sku.name",
          "paths": [
            {
              "path": "properties.accountType",
              "apiVersions": [
                "2015-06-15",
                "2015-05-01-preview"
              ]
            },
            {
              "path": "sku.name",
              "apiVersions": [
                "2016-01-01"
              ]
            }
          ]
        }
    ]

Derzeit werden die folgenden Aliase unterstützt:

| Aliasname | Beschreibung |
| --- | --- |
| {resourceType}/sku.name |Unterstützte Ressourcentypen: Microsoft.Compute/virtualMachines,<br />Microsoft.Storage/storageAccounts,<br />Microsoft.Web/serverFarms,<br /> Microsoft.Scheduler/jobcollections,<br />Microsoft.DocumentDB/databaseAccounts,<br />Microsoft.Cache/Redis,<br />Microsoft.CDN/profiles |
| {resourceType}/sku.family |Unterstützter Ressourcentyp: Microsoft.Cache/Redis |
| {resourceType}/sku.capacity |Unterstützter Ressourcentyp: Microsoft.Cache/Redis |
| Microsoft.Compute/virtualMachines/imagePublisher | |
| Microsoft.Compute/virtualMachines/imageOffer | |
| Microsoft.Compute/virtualMachines/imageSku | |
| Microsoft.Compute/virtualMachines/imageVersion | |
| Microsoft.Cache/Redis/enableNonSslPort | |
| Microsoft.Cache/Redis/shardCount | |
| Microsoft.SQL/servers/version | |
| Microsoft.SQL/servers/databases/requestedServiceObjectiveId | |
| Microsoft.SQL/servers/databases/requestedServiceObjectiveName | |
| Microsoft.SQL/servers/databases/edition | |
| Microsoft.SQL/servers/databases/elasticPoolName | |
| Microsoft.SQL/servers/elasticPools/dtu | |
| Microsoft.SQL/servers/elasticPools/edition | |

Derzeit gilt die Richtlinie nur bei PUT-Anforderungen. 

## <a name="effect"></a>Effekt
Die Richtlinie unterstützt drei Arten von Auswirkungen: **deny**, **audit** und **append**. 

* Mit „deny“ wird ein Ereignis im Überwachungsprotokoll generiert, und die Anforderung schlägt fehl.
* Mit „audit“ wird ein Ereignis im Überwachungsprotokoll generiert, aber die Anforderung schlägt nicht fehl.
* Mit „append“ wird der Anforderung der definierte Satz von Feldern hinzugefügt. 

Für **append**müssen Sie die folgenden Details angeben:

    ....
    "effect": "append",
    "details": [
      {
        "field": "field name",
        "value": "value of the field"
      }
    ]

Der Wert kann entweder eine Zeichenfolge oder ein Objekt im JSON-Format sein. 

## <a name="policy-definition-examples"></a>Beispiele für Richtliniendefinitionen
Jetzt sehen wir uns an, wie die Richtlinie für die oben genannten Szenarios definiert wird.

### <a name="chargeback-require-departmental-tags"></a>Verbrauchsbasierte Kostenzuteilung: Anfordern von Abteilungstags
Die folgende Richtlinie verweigert Anforderungen, die kein Tag mit dem Schlüssel „costCenter“ enthalten.

    {
      "if": {
        "not" : {
          "field" : "tags",
          "containsKey" : "costCenter"
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }

Die folgende Richtlinie fügt ein costCenter-Tag mit einem vordefinierten Wert an, wenn keine Tags vorhanden sind. 

    {
      "if": {
        "field": "tags",
        "exists": "false"
      },
      "then": {
        "effect": "append",
        "details": [
          {
            "field": "tags",
            "value": {"costCenter":"myDepartment" }
          }
        ]
      }
    }

Die folgende Richtlinie fügt ein CostCenter-Tag mit einem vordefinierten Wert an, wenn das CostCenter-Tag nicht vorhanden ist, aber andere Tags vorhanden sind. 

    {
      "if": {
        "allOf": [
          {
            "field": "tags",
            "exists": "true"
          },
          {
            "field": "tags.costCenter",
            "exists": "false"
          }
        ]

      },
      "then": {
        "effect": "append",
        "details": [
          {
            "field": "tags.costCenter",
            "value": "myDepartment"
          }
        ]
      }
    }


### <a name="geo-compliance-ensure-resource-locations"></a>Geografische Kompatibilität: Überprüfen von Ressourcenstandorten
Das folgende Beispiel zeigt eine Richtlinie, die alle Anfragen ablehnt, deren Standort nicht Nordeuropa oder Westeuropa ist.

    {
      "if" : {
        "not" : {
          "field" : "location",
          "in" : ["northeurope" , "westeurope"]
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }

### <a name="service-curation-select-the-service-catalog"></a>Dienstverwaltung: Auswählen des Dienstkatalogs
Das folgende Beispiel zeigt eine Richtlinie, mit der Aktionen nur für Dienste vom Typ „Microsoft.Resources/\*“, „Microsoft.Compute/\*“, „Microsoft.Storage/\*“ und „Microsoft.Network/\*“ zulässig sind. Alle anderen Anforderungen werden verweigert.

    {
      "if" : {
        "not" : {
          "anyOf" : [
            {
              "field" : "type",
              "like" : "Microsoft.Resources/*"
            },
            {
              "field" : "type",
              "like" : "Microsoft.Compute/*"
            },
            {
              "field" : "type",
              "like" : "Microsoft.Storage/*"
            },
            {
              "field" : "type",
              "like" : "Microsoft.Network/*"
            }
          ]
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }

### <a name="use-approved-skus"></a>Verwenden von genehmigten SKUs
Im folgenden Beispiel wird die Verwendung von „property alias“ zum Einschränken von SKUs veranschaulicht. In diesem Beispiel sind nur „Standard_LRS“ und „Standard_GRS“ zur Verwendung für Speicherkonten genehmigt.

    {
      "if": {
        "allOf": [
          {
            "field": "type",
            "equals": "Microsoft.Storage/storageAccounts"
          },
          {
            "not": {
              "allof": [
                {
                  "field": "Microsoft.Storage/storageAccounts/sku.name",
                  "in": ["Standard_LRS", "Standard_GRS"]
                }
              ]
            }
          }
        ]
      },
      "then": {
        "effect": "deny"
      }
    }


### <a name="naming-convention"></a>Benennungskonvention
Das folgende Beispiel veranschaulicht die Verwendung von Platzhalterzeichen, die von der Bedingung „like“ unterstützt werden. Die Bedingung gibt an, dass die Anforderung verweigert wird, wenn der Name nicht mit dem angegebenen Muster (namePrefix\*nameSuffix) übereinstimmt.

    {
      "if" : {
        "not" : {
          "field" : "name",
          "like" : "namePrefix*nameSuffix"
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }

### <a name="tag-requirement-just-for-storage-resources"></a>Taganforderung nur für Speicherressourcen
Das folgende Beispiel zeigt, wie logische Operatoren so verschachtelt werden, dass ein Anwendungstag nur für Speicherressourcen benötigt wird.

    {
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
        "then": {
            "effect": "audit"
        }
    }

## <a name="create-and-assign-a-policy"></a>Erstellen und Zuweisen einer Richtlinie
Das Anwenden einer Richtlinie erfordert das Erstellen einer Richtliniendefinition und das anschließende Anwenden auf einen Bereich. 

### <a name="rest-api"></a>REST-API
Sie können eine Richtlinie mit der REST-API erstellen, wie unter [Policy Definitions](https://docs.microsoft.com/rest/api/resources/policydefinitions)(in englischer Sprache) beschrieben. Die REST-API ermöglicht es Ihnen, Richtliniendefinitionen zu erstellen und zu löschen sowie Informationen zu vorhandenen Definitionen abzurufen.

Zum Erstellen einer Richtlinie führen Sie folgenden Befehl aus:

    PUT https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.authorization/policydefinitions/{policyDefinitionName}?api-version={api-version}

Verwenden Sie als „api-version“ die Einstellung *2016-04-01*. Nehmen Sie einen Anforderungstext auf, der dem im folgenden Beispiel dargestellten ähnelt:

    {
      "properties":{
        "policyType":"Custom",
        "description":"Test Policy",
        "policyRule":{
          "if" : {
            "not" : {
              "field" : "tags",
              "containsKey" : "costCenter"
            }
          },
          "then" : {
            "effect" : "deny"
          }
        }
      },
      "name":"testdefinition"
    }

Sie können die Richtliniendefinition mithilfe von [Policy Assignments](https://docs.microsoft.com/rest/api/resources/policyassignments)(Richtlinienzuweisungen, in englischer Sprache) auf den gewünschten Bereich anwenden. Die REST-API ermöglicht es Ihnen, Richtlinienzuweisungen zu erstellen und zu löschen sowie Informationen zu vorhandenen Zuweisungen abzurufen.

Führen Sie zum Erstellen einer neuen Richtlinienzuweisung folgenden Befehl aus:

    PUT https://management.azure.com /subscriptions/{subscription-id}/providers/Microsoft.authorization/policyassignments/{policyAssignmentName}?api-version={api-version}

"{policy-assignment}" ist der Name der Richtlinienzuweisung. Verwenden Sie als „api-version“ die Einstellung *2016-04-01*. 

Der Anforderungstext sollte dem folgenden Beispiel ähneln:

    {
      "properties":{
        "displayName":"VM_Policy_Assignment",
        "policyDefinitionId":"/subscriptions/########/providers/Microsoft.Authorization/policyDefinitions/testdefinition",
        "scope":"/subscriptions/########-####-####-####-############"
      },
      "name":"VMPolicyAssignment"
    }

### <a name="powershell"></a>PowerShell
Sie können eine Richtliniendefinition mithilfe des Cmdlets „New-AzureRmPolicyDefinition“ erstellen. Im folgenden Beispiel wird eine Richtlinie erstellt, um nur Ressourcen in Nord- und Westeuropa zuzulassen.

    $policy = New-AzureRmPolicyDefinition -Name regionPolicyDefinition -Description "Policy to allow resource creation only in certain regions" -Policy '{    
      "if" : {
        "not" : {
          "field" : "location",
          "in" : ["northeurope" , "westeurope"]
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }'            

Die Ausgabe der Ausführung wird im $policy-Objekt gespeichert und kann später während der Richtlinienzuweisung verwendet werden. Als Richtlinienparameter kann auch der Pfad zu einer JSON-Datei, die die Richtlinie enthält, angegeben werden, anstatt die Richtlinie inline anzugeben.

    New-AzureRmPolicyDefinition -Name regionPolicyDefinition -Description "Policy to allow resource creation only in certain     regions" -Policy "path-to-policy-json-on-disk"

Sie können die Richtlinie auf den gewünschten Bereich anwenden, indem Sie das Cmdlet „New-AzureRmPolicyAssignment“ verwenden:

    New-AzureRmPolicyAssignment -Name regionPolicyAssignment -PolicyDefinition $policy -Scope    /subscriptions/########-####-####-####-############/resourceGroups/<resource-group-name>

Hierbei ist „$policy“ das Richtlinienobjekt, das als Ergebnis der Ausführung des Cmdlets „New-AzureRmPolicyDefinition“ zurückgegeben wurde. Der Bereich ist in diesem Fall der Name der von Ihnen angegebenen Ressourcengruppe.

Um eine Richtlinienzuweisung zu entfernen, verwenden Sie Folgendes:

    Remove-AzureRmPolicyAssignment -Name regionPolicyAssignment -Scope /subscriptions/########-####-####-####-############/resourceGroups/<resource-group-name>

Sie können Richtliniendefinitionen mithilfe der Cmdlets „Get-AzureRmPolicyDefinition“, „Set-AzureRmPolicyDefinition“ und „Remove-AzureRmPolicyDefinition“ abrufen, ändern oder entfernen.

Ähnlich können Sie Richtlinienzuweisungen mithilfe der Cmdlets „Get-AzureRmPolicyAssignment“, „Set-AzureRmPolicyAssignment“ bzw. „Remove-AzureRmPolicyAssignment“ abrufen, ändern oder entfernen.

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle
Sie können eine Richtliniendefinition mit dem entsprechenden Befehl in der Azure-Befehlszeilenschnittstelle erstellen. Im folgenden Beispiel wird eine Richtlinie erstellt, um nur Ressourcen in Nord- und Westeuropa zuzulassen.

    azure policy definition create --name regionPolicyDefinition --description "Policy to allow resource creation only in certain regions" --policy-string '{    
      "if" : {
        "not" : {
          "field" : "location",
          "in" : ["northeurope" , "westeurope"]
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }'    


Sie können auch den Pfad zu einer JSON-Datei angeben, die die Richtlinie enthält, anstatt die Richtlinie inline anzugeben.

    azure policy definition create --name regionPolicyDefinition --description "Policy to allow resource creation only in certain regions" --policy "path-to-policy-json-on-disk"

Sie können die Richtlinie auf den gewünschten Bereich anwenden, indem Sie den Befehl zur Richtlinienzuweisung verwenden:

    azure policy assignment create --name regionPolicyAssignment --policy-definition-id /subscriptions/########-####-####-####-############/providers/Microsoft.Authorization/policyDefinitions/<policy-name> --scope    /subscriptions/########-####-####-####-############/resourceGroups/<resource-group-name>

Der Bereich ist in diesem Fall der Name der von Ihnen angegebenen Ressourcengruppe. Wenn der Wert des Parameters „policy-definition-id“ unbekannt ist, können Sie ihn über die Azure-Befehlszeilenschnittstelle abrufen. 

    azure policy definition show <policy-name>

Um eine Richtlinienzuweisung zu entfernen, verwenden Sie Folgendes:

    azure policy assignment delete --name regionPolicyAssignment --scope /subscriptions/########-####-####-####-############/resourceGroups/<resource-group-name>

Mit den Befehlen „show“, „set“ und „delete“ können Sie Richtliniendefinitionen abrufen, ändern oder entfernen.

Auf ähnliche Weise können Sie über die Befehle „show“ und „delete“ Richtlinienzuweisungen abrufen, ändern und entfernen.

## <a name="policy-audit-events"></a>Überwachungsereignisse der Richtlinie
Nachdem Sie die Richtlinie angewendet haben, werden Sie richtlinienbezogene Ereignisse sehen können. Sie können entweder das Portal, PowerShell oder die Azure-CLI verwenden, um diese Daten abzurufen. 

### <a name="powershell"></a>PowerShell
Zum Anzeigen aller Ereignisse, die mit dem Verweigerungseffekt in Verbindung stehen, können Sie den folgenden PowerShell-Befehl verwenden:

    Get-AzureRmLog | where {$_.OperationName -eq "Microsoft.Authorization/policies/deny/action"} 

Zum Anzeigen aller Ereignisse, die mit dem Überwachungseffekt in Verbindung stehen, können Sie den folgenden Befehl verwenden:

    Get-AzureRmLog | where {$_.OperationName -eq "Microsoft.Authorization/policies/audit/action"} 

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle
Zum Anzeigen aller Ereignisse in einer Ressourcengruppe, die mit dem Verweigerungseffekt in Verbindung stehen, können Sie den folgenden CLI-Befehl verwenden:

    azure group log show ExampleGroup --json | jq ".[] | select(.operationName.value == \"Microsoft.Authorization/policies/deny/action\")"

Zum Anzeigen aller Ereignisse, die mit dem Überwachungseffekt in Verbindung stehen, können Sie den folgenden CLI-Befehl verwenden:

    azure group log show ExampleGroup --json | jq ".[] | select(.operationName.value == \"Microsoft.Authorization/policies/audit/action\")"

## <a name="view-a-policy"></a>Anzeigen einer Richtlinie
Sie verwenden PowerShell, die Azure-Befehlszeilenschnittstelle oder die REST-API, um eine Richtlinie anzuzeigen. Möglicherweise müssen Sie eine Richtlinie anzeigen, wenn es bei einer Bereitstellung zu einem Fehler gekommen ist und Sie die Regel sehen möchten, durch die die Bereitstellung verweigert wurde. Die Fehlermeldung enthält eine ID für die Richtliniendefinition.

### <a name="powershell"></a>PowerShell
Um eine Richtlinie abzurufen, verwenden Sie das folgende Cmdlet:

    (Get-AzureRmPolicyAssignment -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/{definition-name}").Properties.policyRule | ConvertTo-Json

Damit wird die JSON für die Richtliniendefinition zurückgegeben.

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle
Um eine Richtlinie abzurufen, verwenden Sie den folgenden Befehl:

    azure policy definition show {definition-name} --json

### <a name="rest-api"></a>REST-API
Um eine Richtlinie abzurufen, verwenden Sie den Vorgang [Richtliniendefinition abrufen](https://docs.microsoft.com/rest/api/resources/policydefinitions#PolicyDefinitions_Get).

## <a name="next-steps"></a>Nächste Schritte
* Anleitungen dazu, wie Unternehmen Abonnements mit Resource Manager effektiv verwalten können, finden Sie unter [Azure-Unternehmensgerüst - Präskriptive Abonnementgovernance](resource-manager-subscription-governance.md).




<!--HONumber=Nov16_HO3-->


