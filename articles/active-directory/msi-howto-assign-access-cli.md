---
title: Zuweisen von MSI-Zugriff zu einer Azure-Ressource mit der Azure-CLI
description: "Schrittweise Anweisungen für das Zuweisen von MSI zu einer Ressource und das Zuweisen des Zugriffs für eine andere Ressource mithilfe der Azure-CLI."
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2017
ms.author: bryanla
ms.translationtype: HT
ms.sourcegitcommit: 47ba7c7004ecf68f4a112ddf391eb645851ca1fb
ms.openlocfilehash: e6eede1c093145894f4330a0c4385969cd4dd7da
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---

# <a name="assign-a-managed-service-identity-msi-access-to-a-resource-using-azure-cli"></a>Zuweisen des MSI-Zugriffs (Managed Service Identity, verwaltete Dienstidentität) auf eine Ressource mit der Azure-CLI

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Nachdem Sie eine Azure-Ressource mit einer MSI konfiguriert haben, können Sie der MSI genau wie bei allen anderen Sicherheitsprinzipalen den Zugriff auf eine andere Ressource erteilen. In diesem Beispiel wird gezeigt, wie Sie der MSI eines virtuellen Azure-Computers mit der Azure-CLI den Zugriff auf ein Azure-Speicherkonto gewähren.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [msi-qs-configure-prereqs](../../includes/msi-qs-configure-prereqs.md)]

Um die CLI-Skriptbeispiele auszuführen, haben Sie drei Möglichkeiten:

- Verwenden Sie [Azure Cloud Shell](../cloud-shell/overview.md) aus dem Azure-Portal (siehe nächster Abschnitt).
- Verwenden Sie die eingebettete Azure Cloud Shell, indem Sie die Schaltfläche „Ausprobieren“ in der oberen rechten Ecke jedes Codeblocks verwenden.
- [Installieren Sie die neueste Version von CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 oder höher), wenn Sie lieber eine lokale CLI-Konsole verwenden möchten. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>Verwenden Sie RBAC, um einer anderen Ressource den MSI-Zugriff zuzuweisen.

Nachdem Sie MSI für eine Azure-Ressource aktiviert haben, [zum Beispiel für einen virtuellen Azure-Computer](msi-qs-configure-cli-windows-vm.md), gehen Sie wie folgt vor: 

1. Wenn Sie nicht Azure Cloud Shell aus dem Azure-Portal verwenden, melden Sie sich zunächst mit [az login](/cli/azure/#login) bei Azure an. Verwenden Sie ein Konto, das dem Azure-Abonnement zugeordnet ist, unter dem Sie den virtuellen Computer bereitstellen möchten:

   ```azurecli-interactive
   az login
   ```

2. In diesem Beispiel wird einem virtuellen Azure-Computer der Zugriff auf ein Speicherkonto gewährt. Zunächst wird [az resource list](/cli/azure/resource/#list) verwendet, um den Dienstprinzipal für den virtuellen Computer mit dem Namen „myVM“ abzurufen, der beim Aktivieren von MSI auf dem virtuellen Computer erstellt wurde:

   ```azurecli-interactive
   $spID=$(az resource list -n myVM --query [*].identity.principalId --out tsv)
   ```

3. Wenn die Dienstprinzipal-ID abgerufen wurde, wird [az role assignment create](/cli/azure/role/assignment#az_role_assignment_create) verwendet, um dem virtuellen Computer „Reader“ den Zugriff auf ein Speicherkonto namens „myStorageAcct“ zu gewähren:

   ```azurecli-interactive
   az role assignment create --assignee $spID --role 'Reader' --scope /subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/myStorageAcct
   ```

## <a name="troubleshooting"></a>Problembehandlung

Wenn die MSI für die Ressource nicht in der Liste der verfügbaren Identitäten angezeigt wird, stellen Sie sicher, dass die MSI ordnungsgemäß aktiviert wurde. In unserem Fall können wir zurück zum virtuellen Azure-Computer im [Azure-Portal](https://portal.azure.com) wechseln und wie folgt vorgehen:

- Überprüfen Sie auf der Seite „Konfiguration“, ob für die MSI-Aktivierung „Ja“ ausgewählt ist.
- Vergewissern Sie sich auf der Seite „Erweiterungen“, dass die MSI-Erweiterung erfolgreich bereitgestellt wurde.

Wenn einer dieser Punkte nicht zutrifft, müssen Sie die MSI möglicherweise erneut für die Ressource bereitstellen oder den Fehler bei der Bereitstellung beheben.

## <a name="related-content"></a>Verwandte Inhalte

- Einen Überblick über MSI finden Sie unter [Übersicht über verwaltete Dienstidentitäten](msi-overview.md).
- Informationen zum Aktivieren von MSI auf einem virtuellen Azure-Computer finden Sie unter [Konfigurieren einer Azure-VM-MSI (Managed Service Identity, verwaltete Dienstidentität) mit der Azure-CLI](msi-qs-configure-cli-windows-vm.md).

Verwenden Sie den folgenden Kommentarabschnitt, um uns Feedback zu senden und uns bei der Verbesserung unserer Inhalte zu unterstützen.


