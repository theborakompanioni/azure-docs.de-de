---
title: Nutzen einer verwalteten Azure-Anwendung | Microsoft-Dokumentation
description: "Hier wird beschrieben, wie ein Kunde eine verwaltete Azure-Anwendung aus den veröffentlichten Dateien erstellt."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 05/11/2017
ms.author: gauravbh; tomfitz
ms.translationtype: Human Translation
ms.sourcegitcommit: 97fa1d1d4dd81b055d5d3a10b6d812eaa9b86214
ms.openlocfilehash: 2763da60fe25f2ca55603ecfcbbcefe3e368c25d
ms.contentlocale: de-de
ms.lasthandoff: 05/11/2017


---
# <a name="consume-an-azure-managed-application"></a>Nutzen einer verwalteten Azure-Anwendung

Wie im Artikel mit der [Übersicht über verwaltete Anwendungen](managed-application-overview.md) beschrieben umfasst der gesamte Vorgang zwei Szenarien. Bei einem steht der Herausgeber oder ISV im Mittelpunkt, der eine verwaltete Anwendung zur Verwendung durch Kunden erstellen möchte. Beim zweiten geht es um den Kunden oder den Verbraucher der verwalteten Anwendung. Dieser Artikel behandelt das zweite Szenario und erläutert, wie ein Endkunde eine von einem ISV bereitgestellte verwaltete Anwendung nutzen kann.

Derzeit können Sie eine verwaltete Anwendung entweder über die Azure CLI oder das Azure-Portal nutzen. 

## <a name="create-the-managed-application-using-cli"></a>Erstellen der verwalteten Anwendung mit der CLI 

Sie müssen die Gerätedefinitions-ID für das Gerät abrufen, das Sie nutzen möchten.

Mit der Azure CLI haben Sie zwei Möglichkeiten zum Erstellen einer verwalteten Anwendung. Bei einer wird der reguläre Befehl für die Vorlagenbereitstellung und bei der anderen ein neuer Befehl verwendet, der nur für diesen Zweck bereitgestellt wird.

### <a name="create-using-template-deployment-command"></a>Erstellen mithilfe des Vorlagenbereitstellungsbefehls

Sie stellen die vom Hersteller erstellte Datei „applianceMainTemplate.json“ bereit.

Sie erstellen zwei Ressourcengruppen. In der ersten Ressourcengruppe wird die Geräteressource erstellt (Microsoft.Solutions/appliances). Die zweite Ressourcengruppe enthält alle in der Datei „mainTemplate.json“ definierten Ressourcen. Diese Ressourcengruppe wird vom ISV verwaltet.

```azurecli
az group create --name mainResourceGroup --location westcentralus    
az group create --name managedResourceGroup --location westcentralus
```

> [!NOTE]
> Verwenden Sie `westcentralus` als Standort der Ressourcengruppe.
>


Als Nächstes stellen Sie mit folgendem Befehl „applianceMainTemplate.json“ in „mainResourceGroup“ bereit:

```azurecli
az group deployment --name managedAppDeployment --resourceGroup mainResourceGroup 
      --templateUri  
```

Wenn die vorangehende Vorlage ausgeführt wird, werden Sie zur Eingabe der Werte der Parameter aufgefordert, die in der Vorlage definiert sind. Zusätzlich zu den Parametern, die für das Bereitstellen von Ressourcen in einer Vorlage erforderlich sind, benötigen Sie zwei Parameterschlüsselwerte:

- ManagedResourceGroupId: Die ID der Ressourcengruppe, in der die in der Datei „applianceMainTemplate.json“ definierten Ressourcen erstellt werden. Das Format der ID ist `/subscriptions/{subscriptionId}/resourceGroups/{resoureGroupName}`. Im vorherigen Beispiel ist dies die ID von `managedResourceGroup`.
- ApplianceDefinitionId: Die ID der Definitionsressource der verwalteten Anwendung. Dieser Wert wird vom ISV bereitgestellt. 

> [!NOTE] 
> Der ISV muss Zugriff auf die Ressourcengruppe gewähren, in der die Gerätedefinitionsressource erstellt wird. Die Gerätedefinitionsressource wird im ISV-Abonnement erstellt. Daher benötigt ein Benutzer, eine Benutzergruppe oder eine Anwendung im Kundenmandanten Lesezugriff auf diese Ressource. 

Nach erfolgreichem Abschluss der Bereitstellung sehen Sie, dass die Geräteressource in **mainResourceGroup** erstellt wird. Die storageAccount-Ressource wird in **managedResourceGroup** erstellt.

### <a name="create-the-managed-application-using-create-command"></a>Erstellen der verwalteten Anwendung mit dem Erstellungsbefehl

Sie können zum Erstellen einer verwalteten Anwendung aus der Definition der verwalteten Anwendung den Befehl `az managedapp create` verwenden. 

```azurecli
az managedapp create --name ravtestappliance401 --location "westcentralus" 
    --kind "Servicecatalog" --resource-group "ravApplianceCustRG401" 
       --managedapp-definition-id "/subscriptions/{guid}/resourceGroups/ravApplianceDefRG401/providers/Microsoft.Solutions/applianceDefinitions/ravtestAppDef401" 
       --managed-rg-id "/subscriptions/{guid}/resourceGroups/ravApplianceCustManagedRG401" 
       --parameters "{\"storageAccountName\": {\"value\": \"ravappliancedemostore1\"}}" 
       --debug
```

**appliance-definition-Id**: Die Ressourcen-ID der Gerätedefinition, die im vorherigen Schritt erstellt wurde. Führen Sie zum Abrufen dieser ID den folgenden Befehl aus:

```azurecli
az appliance definition show -n ravtestAppDef1 -g ravApplianceRG2
```

Dieser Befehl gibt die Gerätedefinition zurück. Sie benötigen den Wert der **Id**-Eigenschaft.

**managed-rg-id**: Der Name der Ressourcengruppe, in der die in der Datei „applianceMainTemplate.json“ definierten Ressourcen erstellt werden. Diese Ressourcengruppe ist die verwaltete Ressourcengruppe und wird vom Herausgeber verwaltet. Ist sie nicht vorhanden, wird sie für Sie erstellt.

**resource-group**: Die Ressourcengruppe, in der die Geräteressource erstellt wird. Die Ressource „Microsoft.Solutions/appliance“ ist in dieser Ressourcengruppe enthalten. 

**parameters**: Die Parameter, die für die in „applianceMainTemplate.json“ definierten Ressourcen erforderlich sind

## <a name="create-the-managed-application-using-portal"></a>Erstellen der verwalteten Anwendung mit dem Portal

Die Nutzung verwalteter Anwendungen, die von den ISVs veröffentlicht werden, wird auch vom Portal unterstützt. Führen Sie die folgenden Schritte aus:

Wählen Sie die verwaltete Anwendung im Azure-Portal auf dem Blatt „Erstellen“ aus:

![](./media/managed-application-consumption/create-blade.png)

Als Nächstes wird die Liste der Angebote von verschiedenen ISVs/Partnern angezeigt. Wählen Sie das zu erstellende Angebot aus, und klicken Sie auf „Erstellen“.

![](./media/managed-application-consumption/select-offer.png)

Geben Sie nach dem Klicken auf „Erstellen“ auf dem daraufhin geöffneten Blatt die Parameter an, die zum Bereitstellen der Ressourcen erforderlich sind. 

![](./media/managed-application-consumption/input-parameters.png)

Klicken Sie nach Eingabe der Werte auf „OK“. Die Vorlage wird anhand der angegebenen Eingaben überprüft. Wenn die Überprüfung erfolgreich ist, startet die Vorlagenbereitstellung. Nach Abschluss der Bereitstellung werden die in der Vorlage definierten Ressourcen in der angegebenen verwalteten Ressourcengruppe bereitgestellt.

## <a name="known-issues"></a>Bekannte Probleme

Bei dieser Vorschauversion treten die folgenden Probleme auf:

* Wenn während der Erstellung des Geräts die Meldung „500 - Interner Serverfehler“ angezeigt wird, handelt es sich wahrscheinlich um ein vorübergehendes Problem. Wiederholen Sie den Vorgang, wenn dieses Problem auftritt.
* Für die verwaltete Ressourcengruppe ist eine neue Ressourcengruppe erforderlich. Die Verwendung einer vorhandenen Ressourcengruppe verursacht einen Fehler bei der Bereitstellung.
* Die Ressourcengruppe, die die Ressource „Microsoft.Solutions/appliances“ enthält, muss am Standort **westcentralus** erstellt werden.

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in verwaltete Anwendungen finden Sie in der [Übersicht über verwaltete Azure-Anwendungen](managed-application-overview.md).
* Grundlegendes zur Anbietererfahrung erfahren Sie unter [Erstellen und Veröffentlichen von verwalteten Azure-Anwendungen](managed-application-publishing.md).
