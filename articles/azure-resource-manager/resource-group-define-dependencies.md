---
title: "Abhängigkeiten in Azure Resource Manager-Vorlagen | Microsoft Docs"
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
ms.date: 11/28/2016
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: 5f810a46db4deed9c31db4f7c072c48b0817ebc4
ms.openlocfilehash: 35ce1f12a3de0a41d400cceebe6aefbadbe51528


---
# <a name="defining-dependencies-in-azure-resource-manager-templates"></a>Definieren von Abhängigkeiten in Azure-Ressourcen-Manager-Vorlagen
Vor dem Bereitstellen einer bestimmten Ressource kann es erforderlich sein, dass bereits andere Ressourcen vorhanden sind. Zum Beispiel muss SQL Server vorhanden sein, bevor versucht wird, eine SQL-Datenbank bereitzustellen. Diese Beziehung definieren Sie, indem Sie eine Ressource als von einer anderen Ressource abhängig kennzeichnen. In der Regel definieren Sie eine Abhängigkeit mit dem Element **dependsOn**, Sie können sie jedoch auch mithilfe der **reference**-Funktion definieren. 

Resource Manager wertet die Abhängigkeiten zwischen den Ressourcen aus und stellt sie in der Reihenfolge ihrer Abhängigkeiten bereit. Wenn Ressourcen nicht voneinander abhängig sind, stellt Resource Manager sie parallel bereit. Sie müssen nur Abhängigkeiten für Ressourcen definieren, die in der gleichen Vorlage bereitgestellt werden. 

## <a name="dependson"></a>dependsOn
Innerhalb Ihrer Vorlage bietet das „dependsOn“-Element die Möglichkeit, eine Ressource als von einer oder mehreren Ressourcen abhängig zu definieren. Der Wert kann eine durch Trennzeichen getrennte Liste von Ressourcennamen sein. 

Das folgende Beispiel zeigt eine VM-Skalierungsgruppe, die abhängig von einem Load Balancer, einem virtuellen Netzwerk und einer Schleife ist, die mehrere Speicherkonten erstellt. Diese anderen Ressourcen werden im folgenden Beispiel nicht gezeigt, müssen jedoch anderswo in der Vorlage vorhanden sein.

    {
      "type": "Microsoft.Compute/virtualMachineScaleSets",
      "name": "[variables('namingInfix')]",
      "location": "[variables('location')]",
      "apiVersion": "2016-03-30",
      "tags": {
        "displayName": "VMScaleSet"
      },
      "dependsOn": [
        "storageLoop",
        "[variables('loadBalancerName')]",
        "[variables('virtualNetworkName')]"
      ],
      ...
    }

Im vorherigen Beispiel ist eine Abhängigkeit für die Ressourcen enthalten, die durch eine Kopierschleife mit dem Namen **storageLoop** erstellt werden. Ein Beispiel finden Sie unter [Erstellen mehrerer Instanzen von Ressourcen im Azure-Ressourcen-Manager](resource-group-create-multiple.md).

Wenn Sie Abhängigkeiten definieren, können Sie Ressourcenanbieter-Namespace und Ressourcentyp einbeziehen, um Mehrdeutigkeiten zu vermeiden. Um z.B. einen Lastenausgleich und ein virtuelles Netzwerk eindeutig darzustellen, die möglicherweise die gleichen Namen wie andere Ressourcen haben, verwenden Sie folgendes Format:

    "dependsOn": [
      "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
      "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
    ] 

Sie könnten geneigt sein, „dependsOn“ zu verwenden, um Beziehungen zwischen Ressourcen zuzuordnen. Es ist jedoch wichtig zu verstehen, warum Sie dies tun, da dadurch die Leistung Ihrer Bereitstellung beeinträchtigt werden kann. Um beispielsweise zu dokumentieren, wie Ressourcen miteinander verbunden sind, ist „dependsOn“ nicht der richtige Ansatz. Nach der Bereitstellung können Sie nicht mehr abfragen, welche Ressourcen im „dependsOn“-Element definiert waren. Bei Verwenden von „dependsOn“ beeinträchtigen Sie möglicherweise die Bereitstellungszeit, da der Resource Manager zwei Ressourcen, die eine Abhängigkeit aufweisen, nicht parallel bereitstellt. Um Beziehungen zwischen Ressourcen zu dokumentieren, sollten Sie die [Ressourcenverknüpfung](resource-group-link-resources.md) nutzen.

## <a name="child-resources"></a>Untergeordnete Ressourcen
Mit der resources-Eigenschaft können Sie untergeordnete Ressourcen angeben, die mit der definierten Ressource verknüpft sind. Untergeordnete Ressourcen können nur mit fünf Ebenen definiert werden. Es ist wichtig zu beachten, dass keine implizite Abhängigkeit zwischen einer untergeordneten Ressource und der übergeordneten Ressource erstellt wird. Wenn die untergeordnete Ressource nach der übergeordneten Ressource bereitgestellt werden muss, müssen Sie diese Abhängigkeit explizit mit der dependsOn-Eigenschaft angeben. 

Jede übergeordnete Ressource akzeptiert nur bestimmte Ressourcentypen als untergeordnete Ressourcen. Die akzeptierten Ressourcentypen werden im [Vorlagenschema](https://github.com/Azure/azure-resource-manager-schemas) der übergeordneten Ressource angegeben. Der Name des untergeordneten Ressourcentyps enthält den Namen des übergeordneten Ressourcentyps. So sind z.B. **Microsoft.Web/sites/config** und **Microsoft.Web/sites/extensions** untergeordnete Ressourcen von **Microsoft.Web/sites**.

Das folgende Beispiel zeigt einen SQL Server und eine SQL-Datenbank. Beachten Sie, dass eine explizite Abhängigkeit zwischen der SQL-Datenbank und SQL Server definiert wird, obwohl die Datenbank ein untergeordnetes Element des Servers ist.

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


## <a name="reference-function"></a>reference-Funktion
Mit der [reference](resource-group-template-functions.md#reference) -Funktion kann ein Ausdruck seinen Wert von anderen Paaren aus JSON-Name und -Wert oder Laufzeitressourcen ableiten. reference-Ausdrücke deklarieren implizit, dass eine Ressource von einer anderen abhängt. 

    reference('resourceName').propertyPath

Sie können dieses Element oder das dependsOn-Element verwenden, um Abhängigkeiten anzugeben, aber Sie müssen nicht beide für dieselbe abhängige Ressource verwenden. Verwenden Sie nach Möglichkeit einen impliziten Verweis, um das versehentliche Hinzufügen einer unnötigen Abhängigkeit zu vermeiden.

Weitere Informationen finden Sie unter [reference-Funktion](resource-group-template-functions.md#reference).

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum Erstellen von Azure-Ressourcen-Manager-Vorlagen finden Sie unter [Erstellen von Vorlagen](resource-group-authoring-templates.md). 
* Eine Liste der verfügbaren Funktionen in einer Vorlage finden Sie unter [Funktionen von Azure Resource Manager-Vorlagen](resource-group-template-functions.md).




<!--HONumber=Nov16_HO4-->


