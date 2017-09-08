---
title: Nutzen einer verwalteten Azure-Anwendung | Microsoft-Dokumentation
description: "Hier wird beschrieben, wie ein Kunde eine verwaltete Azure-Anwendung aus veröffentlichten Dateien erstellt."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 08/23/2017
ms.author: gauravbh; tomfitz
ms.translationtype: HT
ms.sourcegitcommit: 646886ad82d47162a62835e343fcaa7dadfaa311
ms.openlocfilehash: ed8fbaf2a4546c8e31eeced11cd0b5627fd62c0c
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---
# <a name="consume-an-internal-managed-application"></a>Nutzen einer internen verwalteten Anwendung

Sie können [verwaltete Azure-Anwendungen](managed-application-overview.md) nutzen, die für Mitglieder Ihrer Organisation vorgesehen sind. Sie können beispielsweise verwaltete Anwendungen Ihrer IT-Abteilung auswählen, die die Konformität mit Ihren Unternehmensstandards gewährleisten. Diese verwalteten Anwendungen stehen über den Dienstkatalog, aber nicht über den Azure Marketplace zur Verfügung.

Bevor Sie mit diesem Artikel fortfahren, benötigen Sie eine verwaltete Anwendung, die im Dienstkatalog Ihres Abonnements zur Verfügung steht. Wenn noch niemand in Ihrer Organisation eine verwaltete Anwendung erstellt hat, lesen Sie unter [Veröffentlichen einer verwaltete Anwendung für die interne Nutzung](managed-application-publishing.md) nach.

Derzeit können Sie eine verwaltete Anwendung entweder über die Azure CLI oder das Azure-Portal nutzen.

## <a name="create-the-managed-application-by-using-the-portal"></a>Erstellen der verwalteten Anwendung mit dem Portal

Um eine verwaltete Anwendung über das Portal bereitzustellen, gehen Sie folgendermaßen vor:

1. Öffnen Sie das Azure-Portal. Suchen Sie nach **Verwaltete Dienstkataloganwendung**.

   ![Verwaltete Dienstkataloganwendung](./media/managed-application-consumption/create-service-catalog-managed-application.png)

1. Wählen Sie die verwaltete Anwendung, die Sie erstellen möchten, in der Liste der verfügbaren Lösungen aus. Klicken Sie auf **Erstellen**.

   ![Auswahl der verwalteten Anwendungen](./media/managed-application-consumption/select-offer.png)

1. Geben Sie die zur Bereitstellung der Ressourcen erforderlichen Parameter ein. Wählen Sie als Standort **USA, Westen-Mitte** aus. Klicken Sie auf **OK**.

   ![Parameter für verwaltete Anwendungen](./media/managed-application-consumption/input-parameters.png)

1. Die Vorlage überprüft die bereitgestellten Werte. Wählen Sie bei erfolgreicher Validierung **OK** aus, um die Bereitstellung zu starten.

   ![Validierung verwalteter Anwendungen](./media/managed-application-consumption/validation.png)

Nach Abschluss der Bereitstellung werden die in der Vorlage definierten Ressourcen in der angegebenen verwalteten Ressourcengruppe bereitgestellt.

## <a name="create-the-managed-application-by-using-azure-cli"></a>Erstellen der verwalteten Anwendung mit der Azure CLI

Mit der Azure CLI haben Sie zwei Möglichkeiten zum Erstellen einer verwalteten Anwendung:

* Verwenden Sie den Befehl zum Erstellen von verwalteten Anwendungen.
* Mithilfe des regulären Vorlagenbereitstellungsbefehls

### <a name="use-the-template-deployment-command"></a>Verwenden des Vorlagenbereitstellungsbefehls

Stellen Sie die vom Hersteller erstellte Datei „applianceMainTemplate.json“ bereit.

Erstellen Sie dann zwei Ressourcengruppen. In der ersten Ressourcengruppe wird die Ressource für die verwaltete Anwendung erstellt: „Microsoft.Solutions/appliances“. Die zweite Ressourcengruppe enthält alle in der Datei „mainTemplate.json“ definierten Ressourcen. Diese Ressourcengruppe wird vom ISV verwaltet.

```azurecli
az group create --name mainResourceGroup --location westcentralus
az group create --name managedResourceGroup --location westcentralus
```

> [!NOTE]
> Verwenden Sie `westcentralus` als Standort der Ressourcengruppe.
>

Verwenden Sie zur Bereitstellung von „applianceMainTemplate.json“ in „mainResourceGroup“ den folgenden Befehl:

```azurecli
az group deployment create --name managedAppDeployment --resourceGroup mainResourceGroup --templateUri
```

Nachdem die vorangehende Vorlage ausgeführt wird, werden Sie zur Eingabe der Werte der Parameter aufgefordert, die in der Vorlage definiert sind. Zusätzlich zu den Parametern, die für die Bereitstellung von Ressourcen in einer Vorlage erforderlich sind, benötigen Sie zwei Parameterschlüsselwerte:

- **managedResourceGroupId:** Die ID der Ressourcengruppe mit den in der Datei „applianceMainTemplate.json“ definierten Ressourcen. Das Format der ID ist `/subscriptions/{subscriptionId}/resourceGroups/{resoureGroupName}`. Im vorherigen Beispiel ist dies die ID von `managedResourceGroup`.
- **applianceDefinitionId**: Die ID der Definitionsressource der verwalteten Anwendung. Dieser Wert wird vom ISV bereitgestellt.

> [!NOTE]
> Der Herausgeber muss Zugriff auf die Ressourcengruppe mit der Definition der verwalteten Anwendung erteilen. Die Definitionsressource wird im Herausgeberabonnement erstellt. Daher benötigt ein Benutzer, eine Benutzergruppe oder eine Anwendung im Kundenmandanten Lesezugriff auf diese Ressource.

Nach erfolgreichem Abschluss der Bereitstellung sehen Sie, dass die verwaltete Anwendung in mainResourceGroup erstellt wurde. Die storageAccount-Ressource wird in „managedResourceGroup“ erstellt.

### <a name="use-the-create-command"></a>Verwenden des Befehls zum Erstellen

Sie können zum Erstellen einer verwalteten Anwendung aus der Definition der verwalteten Anwendung den Befehl `az managedapp create` verwenden.

```azurecli
az managedapp create --name ravtestappliance401 --location "westcentralus"
    --kind "Servicecatalog" --resource-group "ravApplianceCustRG401"
    --managedapp-definition-id "/subscriptions/{guid}/resourceGroups/ravApplianceDefRG401/providers/Microsoft.Solutions/applianceDefinitions/ravtestAppDef401"
    --managed-rg-id "/subscriptions/{guid}/resourceGroups/ravApplianceCustManagedRG401"
    --parameters "{\"storageAccountName\": {\"value\": \"ravappliancedemostore1\"}}"
    --debug
```

* **appliance-definition-Id:** Die Ressourcen-ID der Definition für die verwaltete Anwendung, die im vorherigen Schritt erstellt wurde. Führen Sie zum Abrufen dieser ID den folgenden Befehl aus:

  ```azurecli
  az appliance definition show -n ravtestAppDef1 -g ravApplianceRG2
  ```

  Dieser Befehl gibt die Definition für die verwaltete Anwendung zurück. Sie benötigen den Wert der ID-Eigenschaft.

* **managed-rg-id:** Der Name der Ressourcengruppe mit allen in der Datei „applianceMainTemplate.json“ definierten Ressourcen. Bei dieser Ressourcengruppe handelt es sich um die verwaltete Ressourcengruppe. Sie wird vom Herausgeber verwaltet. Wenn Sie nicht vorhanden ist, wird eine für Sie erstellt.
* **resource-group:** Die Ressourcengruppe, in der die Ressource für die verwaltete Anwendung erstellt wird. Die Ressource „Microsoft.Solutions/appliance“ ist in dieser Ressourcengruppe enthalten.
* **parameters**: Die Parameter, die für die in „applianceMainTemplate.json“ definierten Ressourcen erforderlich sind.

## <a name="known-issues"></a>Bekannte Probleme

Bei dieser Vorschauversion treten die folgenden Probleme auf:

* Bei der Erstellung der verwalteten Anwendung wird die Meldung „500 – Interner Serverfehler“ angezeigt. Hierbei handelt es sich aller Wahrscheinlichkeit nach um ein zeitweiliges Problem. Wiederholen Sie den Vorgang.
* Für die verwaltete Ressourcengruppe ist eine neue Ressourcengruppe erforderlich. Wenn Sie eine vorhandene Ressourcengruppe verwenden, tritt bei der Bereitstellung ein Fehler auf.
* Die Ressourcengruppe, die die Ressource „Microsoft.Solutions/appliances“ enthält, muss am Standort **USA, Westen-Mitte** erstellt werden.

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in verwaltete Anwendungen finden Sie in der [Übersicht über verwaltete Anwendungen](managed-application-overview.md).
* Informationen zum Veröffentlichen einer verwalteten Dienstkataloganwendung finden Sie unter [Erstellen und Veröffentlichen einer verwalteten Dienstkataloganwendung](managed-application-publishing.md).
* Informationen zum Veröffentlichen von verwalteten Anwendungen im Azure Marketplace finden Sie unter [Verwaltete Azure-Anwendungen im Marketplace](managed-application-author-marketplace.md).
* Informationen zur Nutzung einer verwalteten Anwendung aus dem Marketplace finden Sie unter [Nutzen verwalteter Azure-Anwendungen im Marketplace](managed-application-consume-marketplace.md).

