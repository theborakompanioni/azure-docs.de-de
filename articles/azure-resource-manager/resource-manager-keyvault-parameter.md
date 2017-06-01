---
title: "Verwenden eines geheimen Schlüssels aus einem Schlüsseltresor mit Resource Manager-Vorlage | Microsoft Docs"
description: "Informationen zum Übergeben eines geheimen Schlüssels aus einem Schlüsseltresor als Parameter während der Bereitstellung."
services: azure-resource-manager,key-vault
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: c582c144-4760-49d3-b793-a3e1e89128e2
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/09/2016
ms.author: tomfitz
ms.translationtype: Human Translation
ms.sourcegitcommit: c785ad8dbfa427d69501f5f142ef40a2d3530f9e
ms.openlocfilehash: 0bf872a44b8ed7cae53d2659aa7be878902130e9
ms.contentlocale: de-de
ms.lasthandoff: 05/26/2017


---
# <a name="use-key-vault-to-pass-secure-parameter-value-during-deployment"></a>Verwenden von Key Vault zum Übergeben eines sicheren Parameterwerts während der Bereitstellung

Wenn Sie einen zu schützenden Wert (z.B. ein Kennwort) während der Bereitstellung als Parameter übergeben müssen, können Sie den Wert aus einer Instanz von [Azure Key Vault](../key-vault/key-vault-whatis.md) abrufen. Sie rufen den Wert ab, indem Sie den Schlüsseltresor und das Geheimnis in Ihrer Parameterdatei angeben. Der Wert wird nie offengelegt, da Sie nur auf die Schlüsseltresor-ID verweisen. Sie müssen den Wert für das Geheimnis nicht jedes Mal manuell eingeben, wenn Sie die Ressourcen bereitstellen. Der Schlüsseltresor kann unter einem anderen Abonnement als die Ressourcengruppe für die Bereitstellung vorhanden sein. Beim Verweisen auf den Schlüsseltresor geben Sie die Abonnement-ID an.

In diesem Thema wird gezeigt, wie Sie einen Schlüsseltresor und ein Geheimnis erstellen, den Zugriff auf das Geheimnis für eine Resource Manager-Vorlage konfigurieren und das Geheimnis als Parameter übergeben. Wenn Sie bereits über einen Schlüsseltresor und ein Geheimnis verfügen, aber den Zugriff auf die Vorlage und den Benutzerzugriff überprüfen müssen, helfen Ihnen die Informationen im Abschnitt [Ermöglichen des Zugriffs auf das Geheimnis](#enable-access-to-the-secret) weiter. Falls Sie bereits über den Schlüsseltresor und das Geheimnis verfügen und sicher sind, dass die richtige Konfiguration für den Vorlagen- und Benutzerzugriff vorhanden ist, fahren Sie mit dem Abschnitt [Verweisen auf ein Geheimnis mit einer statischen ID](#reference-a-secret-with-static-id) fort. 

## <a name="deploy-a-key-vault-and-secret"></a>Bereitstellen eines Schlüsseltresors und eines geheimen Schlüssels

Sie können einen Schlüsseltresor und ein Geheimnis mit einer Resource Manager-Vorlage bereitstellen. Ein Beispiel finden Sie unter [Key vault template schema](resource-manager-template-keyvault.md) (Schlüsseltresorvorlage – Schema) und [Key vault secret template schema](resource-manager-template-keyvault-secret.md) (Schlüsseltresorgeheimnis-Vorlage – Schema). Legen Sie beim Erstellen der Schlüsseltresors die **enabledForTemplateDeployment**-Eigenschaft auf **true** fest, damit andere Resource Manager-Vorlagen darauf verweisen können. 

Sie können den Schlüsseltresor und das Geheimnis auch mit dem Azure-Portal erstellen. 

1. Wählen Sie **Neu** -> **Sicherheit und Identität** -> **Schlüsseltresor**.

   ![Erstellen eines neuen Schlüsseltresors](./media/resource-manager-keyvault-parameter/new-key-vault.png)

2. Geben Sie Werte für den Schlüsseltresor an. Sie können die Einstellungen **Zugriffsrichtlinien** und **Erweiterte Zugriffsrichtlinie** vorerst ignorieren. Diese Einstellungen werden im Abschnitt behandelt. Klicken Sie auf **Erstellen**.

   ![Festlegen des Schlüsseltresors](./media/resource-manager-keyvault-parameter/create-key-vault.png)

3. Sie verfügen nun über einen Schlüsseltresor. Wählen Sie diesen Schlüsseltresor aus.

4. Wählen Sie auf dem Schlüsseltresorblatt die Option **Geheimnisse**.

   ![Auswählen von Geheimnissen](./media/resource-manager-keyvault-parameter/select-secret.png)

5. Wählen Sie **Hinzufügen**.

   ![„Hinzufügen“ wählen](./media/resource-manager-keyvault-parameter/add-secret.png)

6. Wählen Sie als Uploadoption **Manuell** aus. Geben Sie einen Namen und einen Wert für das Geheimnis an. Klicken Sie auf **Erstellen**.

   ![Angeben des Geheimnisses](./media/resource-manager-keyvault-parameter/provide-secret.png)

Sie haben jetzt den Schlüsseltresor und das Geheimnis erstellt.

## <a name="enable-access-to-the-secret"></a>Aktivieren des Zugriffs auf das Geheimnis

Stellen Sie unabhängig davon, ob Sie einen neuen oder einen bereits vorhandenen Schlüsseltresor verwenden, Folgendes sicher: Der Benutzer, der die Vorlage bereitstellt, muss auf das Geheimnis zugreifen können. Der Benutzer, von dem eine Vorlage bereitgestellt wird, in der auf ein Geheimnis verwiesen wird, muss über die Berechtigung `Microsoft.KeyVault/vaults/deploy/action` für den Schlüsseltresor verfügen. Die Rollen [Besitzer](../active-directory/role-based-access-built-in-roles.md#owner) und [Mitwirkender](../active-directory/role-based-access-built-in-roles.md#contributor) gewähren diesen Zugriff. Sie können auch eine [benutzerdefinierte Rolle](../active-directory/role-based-access-control-custom-roles.md) erstellen, die diese Berechtigung gewährt, und den Benutzer zu dieser Rolle hinzufügen. Außerdem müssen Sie für den Resource Manager zulassen, dass während der Bereitstellung Zugriff auf den Schlüsseltresor besteht.

Sie können diese Schritte über das Portal prüfen bzw. ausführen.

1. Wählen Sie die Option **Zugriffssteuerung (IAM)**.

   ![Auswählen der Zugriffssteuerung](./media/resource-manager-keyvault-parameter/select-access-control.png)

2. Wenn das Konto, das Sie zum Bereitstellen von Vorlagen verwenden möchten, nicht bereits ein Besitzer oder Mitwirkender ist (oder einer benutzerdefinierten Rolle mit der `Microsoft.KeyVault/vaults/deploy/action`-Berechtigung hinzugefügt wurde), wählen Sie **Hinzufügen**.

   ![Benutzer hinzufügen](./media/resource-manager-keyvault-parameter/add-user.png)

3. Wählen Sie die Rolle „Mitwirkender“ oder „Besitzer“, und suchen Sie nach der Identität, die dieser Rolle zugewiesen werden soll. Wählen Sie **OK**, um das Hinzufügen der Identität zur Rolle abzuschließen.

   ![Benutzer hinzufügen](./media/resource-manager-keyvault-parameter/search-user.png)

4. Wählen Sie die Option **Advanced access control** (Erweiterte Zugriffssteuerung), um den Zugriff aus einer Vorlage während der Bereitstellung zu aktivieren. Wählen Sie die Option **Zugriff auf Azure Resource Manager für Vorlagenbereitstellung aktivieren**.

   ![Aktivieren des Zugriffs auf die Vorlage](./media/resource-manager-keyvault-parameter/select-template-access.png)

## <a name="reference-a-secret-with-static-id"></a>Verweisen auf ein Geheimnis mit einer statischen ID

Sie verweisen über eine **Parameterdatei (nicht die Vorlage)**, die Werte an die Vorlage übergibt, auf das Geheimnis. Sie verweisen auf den geheimen Schlüssel, indem Sie den Ressourcenbezeichner des Schlüsseltresors und den Namen des geheimen Schlüssels übergeben. Im folgenden Beispiel muss das Geheimnis für den Schlüsseltresor bereits vorhanden sein, und Sie geben einen statischer Wert für dessen Ressourcen-ID ein.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "sqlsvrAdminLoginPassword": {
        "reference": {
          "keyVault": {
            "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
          },
          "secretName": "adminPassword"
        }
      },
      "sqlsvrAdminLogin": {
        "value": "exampleadmin"
      }
    }
}
```

In der Vorlage sollte der Parameter, der das Geheimnis annimmt, ein Parameter vom Typ **securestring** sein. Das folgende Beispiel zeigt die relevanten Abschnitte einer Vorlage auf, die einen SQL-Server bereitstellt, für den ein Administratorkennwort erforderlich ist.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "sqlsvrAdminLogin": {
            "type": "string",
            "minLength": 4
        },
        "sqlsvrAdminLoginPassword": {
            "type": "securestring"
        },
        ...
    },
    "resources": [
        {
          "name": "[variables('sqlsvrName')]",
          "type": "Microsoft.Sql/servers",
          "location": "[resourceGroup().location]",
          "apiVersion": "2014-04-01-preview",
          "properties": {
              "administratorLogin": "[parameters('sqlsvrAdminLogin')]",
              "administratorLoginPassword": "[parameters('sqlsvrAdminLoginPassword')]"
          },
          ...
        }
    ],
    "variables": {
        "sqlsvrName": "[concat('sqlsvr', uniqueString(resourceGroup().id))]"
    },
    "outputs": { }
}
```



## <a name="reference-a-secret-with-dynamic-id"></a>Verweisen auf einen geheimen Schlüssel mit dynamischer ID

Im vorherigen Abschnitt wurde für das Geheimnis des Schlüsseltresors eine statische Ressourcen-ID übergeben. Manchmal muss jedoch auf einen geheimen Schlüsseltresorschlüssel verwiesen werden, der je nach aktueller Bereitstellung variiert. In einem solchen Fall kann die Ressourcen-ID nicht in der Parameterdatei hartcodiert werden. Da in der Parameterdatei keine Vorlagenausdrücke zulässig sind, kann die Ressourcen-ID leider nicht dynamisch in der Parameterdatei generiert werden.

Wenn die Ressourcen-ID für das Geheimnis eines Schlüsseltresors dynamisch generiert werden muss, müssen Sie die Ressource, die das Geheimnis benötigt, in eine geschachtelte Vorlage verschieben. Die geschachtelte Vorlage wird der Mastervorlage hinzugefügt, und es wird ein Parameter mit der dynamisch generierten Ressourcen-ID übergeben.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "vaultName": {
        "type": "string"
      },
      "secretName": {
        "type": "string"
      }
    },
    "resources": [
    {
      "apiVersion": "2015-01-01",
      "name": "nestedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
        "mode": "incremental",
        "templateLink": {
          "uri": "https://www.contoso.com/AzureTemplates/newVM.json",
          "contentVersion": "1.0.0.0"
        },
        "parameters": {
          "adminPassword": {
            "reference": {
              "keyVault": {
                "id": "[concat(resourceGroup().id, '/providers/Microsoft.KeyVault/vaults/', parameters('vaultName'))]"
              },
              "secretName": "[parameters('secretName')]"
            }
          }
        }
      }
    }],
    "outputs": {}
}
```

## <a name="next-steps"></a>Nächste Schritte
* Allgemeine Informationen zu Schlüsseltresoren finden Sie unter [Erste Schritte mit dem Azure-Schlüsseltresor](../key-vault/key-vault-get-started.md).
* Vollständige Beispiele für Verweise auf geheime Schlüssel finden Sie unter [Key Vault examples](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples)(in englischer Sprache).


