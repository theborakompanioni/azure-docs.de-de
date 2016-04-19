<properties
   pageTitle="Abhängigkeiten in Azure Resource Manager-Vorlagen | Microsoft Azure"
   description="Beschreibt, wie während der Bereitstellung festlegt wird, dass eine Ressource von einer anderen Ressource abhängt, um sicherzustellen, dass die Ressourcen in der richtigen Reihenfolge bereitgestellt werden."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/04/2016"
   ms.author="tomfitz"/>

# Definieren von Abhängigkeiten in Azure-Ressourcen-Manager-Vorlagen

Vor dem Bereitstellen einer bestimmten Ressource kann es erforderlich sein, dass bereits andere Ressourcen vorhanden sind. Zum Beispiel muss SQL Server vorhanden sein, bevor versucht wird, eine SQL-Datenbank bereitzustellen. Diese Beziehung definieren Sie, indem Sie eine Ressource als von einer anderen Ressource abhängig kennzeichnen. In der Regel definieren Sie eine Abhängigkeit mit dem Element **dependsOn**, Sie können sie jedoch auch mithilfe der **reference**-Funktion definieren.

Resource Manager wertet die Abhängigkeiten zwischen den Ressourcen aus und stellt sie in der Reihenfolge ihrer Abhängigkeiten bereit. Wenn Ressourcen nicht voneinander abhängig sind, stellt Resource Manager sie parallel bereit.

## dependsOn

Innerhalb Ihrer Vorlage bietet das dependsOn-Element die Möglichkeit, eine Ressource als von einer oder mehreren Ressourcen abhängig zu definieren. Der Wert kann eine durch Trennzeichen getrennte Liste von Ressourcennamen sein.

Das folgende Beispiel zeigt eine VM-Skalierungsgruppe, die abhängig ist von einem Lastenausgleich, einem virtuellen Netzwerk und einer Schleife, die mehrere Speicherkonten erstellt. Diese anderen Ressourcen werden nachstehend nicht angezeigt, jedoch müssten sie anderswo in der Vorlage vorhanden sein.

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
        "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
      ],
      ...
    }

Wenn Sie eine Abhängigkeit zwischen einer Ressource und Ressourcen definieren möchten, die über eine Kopierschleife erstellt werden (wie oben gezeigt), können Sie das dependsOn-Element auf den Namen der Schleife festlegen. Ein Beispiel finden Sie unter [Erstellen mehrerer Instanzen von Ressourcen im Azure-Ressourcen-Manager](resource-group-create-multiple.md).

Sie könnten geneigt sein, dependsOn zu verwenden, um Abhängigkeiten zwischen Ressourcen zuzuordnen, es ist jedoch wichtig zu verstehen, warum Sie es tun, da es sich auf die Leistung Ihrer Bereitstellung auswirken kann. Beispiel: Wenn Sie so dokumentieren möchten, wie Ressourcen miteinander verbunden sind, ist dependsOn nicht der richtige Ansatz. Der Lebenszyklus von dependsOn dient nur für die Bereitstellung und ist nach der Bereitstellung nicht verfügbar. Einmal bereitgestellt, gibt es keine Möglichkeit, diese Abhängigkeiten abzufragen. Durch die Verwendung von dependsOn besteht das Risiko einer Beeinträchtigung der Leistung, indem Sie versehentlich das Bereitstellungsmodul veranlassen könnten, keine Parallelität zu verwenden, wo dies andernfalls möglicherweise der Fall wäre. Verwenden Sie stattdessen [Ressourcenverknüpfungen](resource-group-link-resources.md) zum Dokumentieren und Bereitstellen von Abfragefunktionen für Beziehungen zwischen Ressourcen.

## Untergeordnete Ressourcen

Mit der resources-Eigenschaft können Sie untergeordnete Ressourcen angeben, die mit der definierten Ressource verknüpft sind. Untergeordnete Ressourcen können nur mit fünf Ebenen definiert werden. Es ist wichtig zu beachten, dass keine implizite Abhängigkeit zwischen einer untergeordneten Ressource und der übergeordneten Ressource erstellt wird. Wenn die untergeordnete Ressource nach der übergeordneten Ressource bereitgestellt werden muss, müssen Sie diese Abhängigkeit explizit mit der dependsOn-Eigenschaft angeben.

Jede übergeordnete Ressource akzeptiert nur bestimmte Ressourcentypen als untergeordnete Ressourcen. Die akzeptierten Ressourcentypen werden im [Vorlagenschema](https://github.com/Azure/azure-resource-manager-schemas) der übergeordneten Ressource angegeben. Der Name des untergeordneten Ressourcentyps enthält den Namen des übergeordneten Ressourcentyps. So sind z. B. **Microsoft.Web/sites/config** und **Microsoft.Web/sites/extensions** untergeordnete Ressourcen von **Microsoft.Web/sites**.

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


## reference-Funktion

Mit der reference-Funktion kann ein Ausdruck seinen Wert von anderen JSON-Name/Wertpaaren oder Laufzeitressourcen ableiten. reference-Ausdrücke deklarieren implizit, dass eine Ressource von einer anderen abhängt. Die durch **propertyPath** unten dargestellte Eigenschaft ist optional. Wenn sie nicht angegeben ist, wird auf die Ressource verwiesen.

    reference('resourceName').propertyPath

Sie können dieses Element oder das dependsOn-Element verwenden, um Abhängigkeiten anzugeben, aber Sie müssen nicht beide für dieselbe abhängige Ressource verwenden. Es empfiehlt sich, den impliziten Verweis zu verwenden, um das Risiko zu vermeiden, dass versehentlich ein unnötiges dependsOn-Element das Bereitstellungsmodul davon abhält, Aspekte der Bereitstellung parallel auszuführen.

Weitere Informationen finden Sie unter [reference-Funktion](../resource-group-template-functions/#reference).

## Nächste Schritte

- Weitere Informationen zum Erstellen von Azure-Ressourcen-Manager-Vorlagen finden Sie unter [Erstellen von Vorlagen](resource-group-authoring-templates.md). 
- Eine Liste der verfügbaren Funktionen in einer Vorlage finden Sie unter [Funktionen von Azure Resource Manager-Vorlagen](resource-group-template-functions.md).

<!---HONumber=AcomDC_0406_2016-->