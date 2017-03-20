---
title: Definieren einer untergeordneten Ressource in einer Azure-Vorlage | Microsoft Docs
description: "Erfahren Sie, wie Ressourcentyp und -name für eine untergeordnete Ressource in einer Azure Resource Manager-Vorlage festgelegt werden."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/02/2017
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: cea53acc33347b9e6178645f225770936788f807
ms.openlocfilehash: d7560b689d7cea56d40ffa2db9542f74a649f9c1
ms.lasthandoff: 03/03/2017


---
# <a name="set-name-and-type-for-child-resource-in-resource-manager-template"></a>Festlegen von Namen und Typ für eine untergeordnete Ressource in einer Resource Manager-Vorlage
Beim Erstellen einer Vorlage müssen Sie häufig eine untergeordnete Ressource einfügen, die einer übergeordneten Ressource zugeordnet ist. Die Vorlage kann beispielsweise eine SQL Server-Instanz und eine Datenbank enthalten. Dann ist die SQL Server-Instanz die übergeordnete Ressource und die Datenbank die untergeordnete Ressource. 

Der Typ der untergeordneten Ressource weist folgendes Format auf: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`

Der Name der untergeordneten Ressource weist folgendes Format auf: `{parent-resource-name}/{child-resource-name}`

Der Typ und der Name einer untergeordneten Ressource werden jedoch in einer Vorlage unterschiedlich angegeben, je nachdem, ob die untergeordnete Ressource in der übergeordneten Ressource geschachtelt oder eigenständig auf der obersten Ebene definiert ist. In diesem Thema werden beide Fälle beschrieben.

## <a name="nested-child-resource"></a>Geschachtelte untergeordnete Ressource
Eine untergeordnete Ressource lässt sich am einfachsten definieren, indem sie in der übergeordneten Ressource geschachtelt wird. Das folgende Beispiel zeigt eine in einer SQL Server-Instanz geschachtelte SQL-Datenbank.

```json
{
  "name": "exampleserver",
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2014-04-01",
  ...
  "resources": [
    {
      "name": "exampledatabase",
      "type": "databases",
      "apiVersion": "2014-04-01",
      ...
    }
  ]
}
```

Der Typ der untergeordneten Ressource ist auf `databases` festgelegt, ihr vollständiger Ressourcentyp ist jedoch `Microsoft.Sql/servers/databases`. `Microsoft.Sql/servers/` wird nicht angegeben, da dieser Teil vom übergeordneten Ressourcentyp übernommen wird. Der Name der untergeordneten Ressource ist auf `exampledatabase` festgelegt, der vollständige Name enthält jedoch auch den Namen der übergeordneten Ressource. `exampleserver` wird nicht angegeben, da dieser Teil von der übergeordneten Ressource übernommen wird.

## <a name="top-level-child-resource"></a>Untergeordnete Ressource der obersten Ebene
Sie können die untergeordnete Ressource auf der obersten Ebene definieren. So können Sie vorgehen, wenn die übergeordnete Ressource nicht in derselben Vorlage bereitgestellt wird oder wenn Sie mithilfe von `copy` mehrere untergeordnete Ressourcen erstellen möchten. In diesem Fall müssen Sie den vollständigen Ressourcentyp angeben und den Namen der übergeordneten Ressource in den Namen der untergeordneten Ressource einfügen.

```json
{
  "name": "exampleserver",
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2014-04-01",
  "resources": [ 
  ],
  ...
},
{
  "name": "exampleserver/exampledatabase",
  "type": "Microsoft.Sql/servers/databases",
  "apiVersion": "2014-04-01",
  ...
}
```

Die Datenbank ist eine untergeordnete Ressource des Servers, auch wenn sie in der Vorlage auf der gleichen Ebene definiert sind.

## <a name="next-steps"></a>Nächste Schritte
* Empfehlungen zum Erstellen von Vorlagen finden Sie unter [Bewährte Methoden für das Erstellen von Azure Resource Manager-Vorlagen](resource-manager-template-best-practices.md).
* Ein Beispiel zum Erstellen mehrerer untergeordneter Ressourcen finden Sie unter [Bereitstellen mehrerer Instanzen von Ressourcen in Azure Resource Manager-Vorlagen](resource-group-create-multiple.md).

