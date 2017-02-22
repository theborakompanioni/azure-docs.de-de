---
title: "Festlegen einer Bereitstellungsreihenfolge für Azure-Ressourcen | Microsoft-Dokumentation"
description: "Beschreibt, wie während der Bereitstellung festlegt wird, dass eine Ressource von einer anderen Ressource abhängt, um sicherzustellen, dass die Ressourcen in der richtigen Reihenfolge bereitgestellt werden."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: 
ms.assetid: 34ebaf1e-480c-4b4d-9bf6-251bd3f8f2cf
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/03/2017
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: 2a9075f4c9f10d05df3b275a39b3629d4ffd095f
ms.openlocfilehash: 5254ea3f072402e2fa4cffcdc23b2e9eae2aad57


---
# <a name="define-the-order-for-deploying-resources-in-azure-resource-manager-templates"></a>Definieren der Reihenfolge für die Bereitstellung von Ressourcen in Azure Resource Manager-Vorlagen
Vor dem Bereitstellen einer bestimmten Ressource kann es erforderlich sein, dass bereits andere Ressourcen vorhanden sind. Zum Beispiel muss SQL Server vorhanden sein, bevor versucht wird, eine SQL-Datenbank bereitzustellen. Diese Beziehung definieren Sie, indem Sie eine Ressource als von einer anderen Ressource abhängig kennzeichnen. Sie definieren eine Abhängigkeit mit dem **dependsOn**-Element oder mit der **reference**-Funktion. 

Resource Manager wertet die Abhängigkeiten zwischen den Ressourcen aus und stellt sie in der Reihenfolge ihrer Abhängigkeiten bereit. Wenn Ressourcen nicht voneinander abhängig sind, stellt Resource Manager sie parallel bereit. Sie müssen nur Abhängigkeiten für Ressourcen definieren, die in der gleichen Vorlage bereitgestellt werden. 

## <a name="dependson"></a>dependsOn
Innerhalb Ihrer Vorlage bietet das „dependsOn“-Element die Möglichkeit, eine Ressource als von einer oder mehreren Ressourcen abhängig zu definieren. Der Wert kann eine durch Trennzeichen getrennte Liste von Ressourcennamen sein. 

Das folgende Beispiel zeigt eine VM-Skalierungsgruppe, die abhängig von einem Load Balancer, einem virtuellen Netzwerk und einer Schleife ist, die mehrere Speicherkonten erstellt. Diese anderen Ressourcen werden im folgenden Beispiel nicht gezeigt, müssen jedoch anderswo in der Vorlage vorhanden sein.

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "name": "[variables('namingInfix')]",
  "location": "[variables('location')]",
  "apiVersion": "2016-03-30",
  "tags": {
    "displayName": "VMScaleSet"
  },
  "dependsOn": [
    "[variables('loadBalancerName')]",
    "[variables('virtualNetworkName')]",
    "storageLoop",
  ],
  ...
}
```

Im vorherigen Beispiel ist eine Abhängigkeit für die Ressourcen enthalten, die durch eine Kopierschleife mit dem Namen **storageLoop** erstellt werden. Ein Beispiel finden Sie unter [Erstellen mehrerer Instanzen von Ressourcen im Azure-Ressourcen-Manager](resource-group-create-multiple.md).

Wenn Sie Abhängigkeiten definieren, können Sie Ressourcenanbieter-Namespace und Ressourcentyp einbeziehen, um Mehrdeutigkeiten zu vermeiden. Um z.B. einen Lastenausgleich und ein virtuelles Netzwerk eindeutig darzustellen, die möglicherweise die gleichen Namen wie andere Ressourcen haben, verwenden Sie folgendes Format:

```json
"dependsOn": [
  "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
  "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
]
``` 

Sie können Beziehungen zwischen Ihren Ressourcen mithilfe von „dependsOn“ zuordnen, dabei sollte Ihnen jedoch bewusst sein, welchen Zweck diese Zuordnung erfüllt. Um beispielsweise zu dokumentieren, wie Ressourcen miteinander verbunden sind, ist „dependsOn“ nicht der richtige Ansatz. Nach der Bereitstellung können Sie nicht mehr abfragen, welche Ressourcen im „dependsOn“-Element definiert waren. Bei Verwenden von „dependsOn“ beeinträchtigen Sie möglicherweise die Bereitstellungszeit, da der Resource Manager zwei Ressourcen, die eine Abhängigkeit aufweisen, nicht parallel bereitstellt. Um Beziehungen zwischen Ressourcen zu dokumentieren, sollten Sie die [Ressourcenverknüpfung](resource-group-link-resources.md) nutzen.

## <a name="child-resources"></a>Untergeordnete Ressourcen
Mit der resources-Eigenschaft können Sie untergeordnete Ressourcen angeben, die mit der definierten Ressource verknüpft sind. Untergeordnete Ressourcen können nur mit fünf Ebenen definiert werden. Es ist wichtig zu beachten, dass keine implizite Abhängigkeit zwischen einer untergeordneten Ressource und der übergeordneten Ressource erstellt wird. Wenn die untergeordnete Ressource nach der übergeordneten Ressource bereitgestellt werden muss, müssen Sie diese Abhängigkeit explizit mit der dependsOn-Eigenschaft angeben. 

Jede übergeordnete Ressource akzeptiert nur bestimmte Ressourcentypen als untergeordnete Ressourcen. Die akzeptierten Ressourcentypen werden im [Vorlagenschema](https://github.com/Azure/azure-resource-manager-schemas) der übergeordneten Ressource angegeben. Der Name des untergeordneten Ressourcentyps enthält den Namen des übergeordneten Ressourcentyps. So sind z.B. **Microsoft.Web/sites/config** und **Microsoft.Web/sites/extensions** untergeordnete Ressourcen von **Microsoft.Web/sites**.

Das folgende Beispiel zeigt einen SQL Server und eine SQL-Datenbank. Beachten Sie, dass eine explizite Abhängigkeit zwischen der SQL-Datenbank und SQL Server definiert wird, obwohl die Datenbank ein untergeordnetes Element des Servers ist.

```json
"resources": [
  {
    "name": "[variables('sqlserverName')]",
    "type": "Microsoft.Sql/servers",
    "location": "[resourceGroup().location]",
    "tags": {
      "displayName": "SqlServer"
    },
    "apiVersion": "2014-04-01-preview",
    "properties": {
      "administratorLogin": "[parameters('administratorLogin')]",
      "administratorLoginPassword": "[parameters('administratorLoginPassword')]"
    },
    "resources": [
      {
        "name": "[parameters('databaseName')]",
        "type": "databases",
        "location": "[resourceGroup().location]",
        "tags": {
          "displayName": "Database"
        },
        "apiVersion": "2014-04-01-preview",
        "dependsOn": [
          "[variables('sqlserverName')]"
        ],
        "properties": {
          "edition": "[parameters('edition')]",
          "collation": "[parameters('collation')]",
          "maxSizeBytes": "[parameters('maxSizeBytes')]",
          "requestedServiceObjectiveName": "[parameters('requestedServiceObjectiveName')]"
        }
      }
    ]
  }
]
```

## <a name="reference-function"></a>reference-Funktion
Mit der [reference](resource-group-template-functions.md#reference) -Funktion kann ein Ausdruck seinen Wert von anderen Paaren aus JSON-Name und -Wert oder Laufzeitressourcen ableiten. reference-Ausdrücke deklarieren implizit, dass eine Ressource von einer anderen abhängt. Das allgemeine Format lautet wie folgt:

```json
reference('resourceName').propertyPath
```

Im folgenden Beispiel hängt ein CDN-Endpunkt explizit vom CDN-Profil und implizit von einer Web-App ab.

```json
{
    "name": "[variables('endpointName')]",
    "type": "endpoints",
    "location": "[resourceGroup().location]",
    "apiVersion": "2016-04-02",
    "dependsOn": [
            "[variables('profileName')]"
    ],
    "properties": {
        "originHostHeader": "[reference(variables('webAppName')).hostNames[0]]",
        ...
    }
```

Sie können dieses Element oder das dependsOn-Element verwenden, um Abhängigkeiten anzugeben, aber Sie müssen nicht beide für dieselbe abhängige Ressource verwenden. Verwenden Sie nach Möglichkeit einen impliziten Verweis, um das Hinzufügen einer unnötigen Abhängigkeit zu vermeiden.

Weitere Informationen finden Sie unter [reference-Funktion](resource-group-template-functions.md#reference).

## <a name="recommendations-for-setting-dependencies"></a>Empfehlungen für das Festlegen von Abhängigkeiten

Verwenden Sie die folgenden Richtlinien für die Entscheidung, welche Abhängigkeiten festgelegt werden sollen:

* Legen Sie möglichst wenige Abhängigkeiten fest.
* Legen Sie für eine untergeordnete Ressource eine Abhängigkeit von der übergeordneten Ressource fest.
* Verwenden Sie die **reference**-Funktion zum Festlegen impliziter Abhängigkeiten zwischen den Ressourcen, die Eigenschaften gemeinsam nutzen müssen. Fügen Sie keine explizite Abhängigkeit (**DependsOn**) hinzu, wenn Sie bereits eine implizite Abhängigkeit definiert haben. Dieser Ansatz reduziert das Risiko unnötiger Abhängigkeiten. 
* Legen Sie eine Abhängigkeit fest, wenn eine Ressource nicht ohne Funktionen einer anderen Ressource **erstellt** werden kann. Legen Sie keine Abhängigkeit fest, wenn die Ressourcen erst nach der Bereitstellung interagieren.
* Lassen Sie die Abhängigkeiten ohne explizites Festlegen überlappen. Beispiel: Ihr virtueller Computer hängt von einer virtuellen Netzwerkschnittstelle ab, die wiederum von einem virtuellen Netzwerk und einer öffentlichen IP-Adressen abhängt. Deshalb wird der virtuelle Computer nach allen drei Ressourcen bereitgestellt, aber nicht explizit festgelegt, dass er von allen drei Ressourcen abhängig ist. Dieser Ansatz verdeutlicht die Reihenfolge der Abhängigkeiten und vereinfacht spätere Änderungen an der Vorlage.
* Falls vor der Bereitstellung ein Wert bestimmt werden kann, versuchen Sie, die Ressource ohne Abhängigkeit bereitzustellen. Beispiel: Wenn ein Konfigurationswert den Namen einer anderen Ressource benötigt, ist möglicherweise keine Abhängigkeit erforderlich. Dieser Tipp funktioniert nicht immer, da einige Ressourcen das Vorhandensein der anderen Ressource überprüfen. Wenn Sie einen Fehler erhalten, fügen Sie eine Abhängigkeit hinzu. 

Resource Manager kennzeichnet Ringabhängigkeiten während der Überprüfung der Vorlage. Falls Sie einen Fehler erhalten, der besagt, dass eine Ringabhängigkeit vorhanden ist, bewerten Sie Ihre Vorlage, um festzustellen, ob bestimmte Abhängigkeiten nicht erforderlich sind und möglicherweise entfernt werden können. Wenn das Entfernen der Abhängigkeiten nicht funktioniert, können Sie Ringabhängigkeiten vermeiden, indem Sie einige Bereitstellungsvorgänge in untergeordnete Ressourcen verschieben, die nach den Ressourcen mit Ringabhängigkeit bereitgestellt werden. Nehmen wir beispielsweise an, Sie stellen zwei virtuelle Computer bereit, müssen aber Eigenschaften festlegen, die auf den jeweils anderen verweisen. Sie können diese in der folgenden Reihenfolge bereitstellen:

1. VM1
2. VM2
3. Die Erweiterung auf VM1 hängt von VM1 und VM2 ab. Die Erweiterung legt Werte auf VM1 fest, die sie von VM2 abruft.
4. Die Erweiterung auf VM2 hängt von VM1 und VM2 ab. Die Erweiterung legt Werte auf VM2 fest, die sie von VM1 abruft.

Informationen zur Bewertung der Bereitstellungsreihenfolge und zur Behebung von Abhängigkeitsfehlern finden Sie unter [Überprüfen der Bereitstellungssequenz](resource-manager-common-deployment-errors.md#check-deployment-sequence).

## <a name="next-steps"></a>Nächste Schritte
* Informationen zur Behebung von Abhängigkeiten während der Bereitstellung finden Sie unter [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](resource-manager-common-deployment-errors.md).
* Weitere Informationen zum Erstellen von Azure-Ressourcen-Manager-Vorlagen finden Sie unter [Erstellen von Vorlagen](resource-group-authoring-templates.md). 
* Eine Liste der verfügbaren Funktionen in einer Vorlage finden Sie unter [Funktionen von Azure Resource Manager-Vorlagen](resource-group-template-functions.md).




<!--HONumber=Feb17_HO3-->


