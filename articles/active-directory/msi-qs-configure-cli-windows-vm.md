---
title: Konfigurieren von MSI auf einem virtuellen Azure-Computer mit der Azure-CLI
description: "Schrittweise Anweisungen zum Konfigurieren einer verwalteten Dienstidentität (Managed Service Identity, MSI) auf einem virtuellen Azure-Computer mit der Azure-CLI."
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
ms.openlocfilehash: 78a6164e76f6ceab936874e68bd38bb4eb387e00
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---

# <a name="configure-a-vm-managed-service-identity-msi-using-azure-cli"></a>Konfigurieren einer VM-MSI (Managed Service Identity, verwaltete Dienstidentität) mit der Azure-CLI

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Eine verwaltete Dienstidentität stellt für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory bereit. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein. 

In diesem Artikel erfahren Sie, wie Sie MSI für einen virtuellen Azure-Windows-Computer mit der Azure-CLI aktivieren und entfernen.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [msi-qs-configure-prereqs](../../includes/msi-qs-configure-prereqs.md)]

Um die CLI-Skriptbeispiele auszuführen, haben Sie drei Möglichkeiten:

- Verwenden Sie [Azure Cloud Shell](../cloud-shell/overview.md) aus dem Azure-Portal (siehe nächster Abschnitt).
- Verwenden Sie die eingebettete Azure Cloud Shell, indem Sie die Schaltfläche „Ausprobieren“ in der oberen rechten Ecke jedes Codeblocks verwenden.
- [Installieren Sie die neueste Version von CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 oder höher), wenn Sie lieber eine lokale CLI-Konsole verwenden möchten. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="enable-msi-during-creation-of-an-azure-vm"></a>Aktivieren von MSI beim Erstellen eines virtuellen Azure-Computers

Eine neue, für MSI aktivierte virtuelle Windows-Computerressource wird in einer neuen Ressourcengruppe mit den angegebenen Konfigurationsparametern erstellt. Beachten Sie, dass viele dieser Funktionen unter Umständen mehrere Sekunden bzw. Minuten ausgeführt werden, bevor ein Ergebnis zurückgegeben wird.

1. Wenn Sie nicht Azure Cloud Shell aus dem Azure-Portal verwenden, melden Sie sich zunächst mit [az login](/cli/azure/#login) bei Azure an. Verwenden Sie ein Konto, das dem Azure-Abonnement zugeordnet ist, unter dem Sie den virtuellen Computer bereitstellen möchten:

   ```azurecli-interactive
   az login
   ```

2. Erstellen Sie eine [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md#terminology) für das Einschließen und Bereitstellen des virtuellen Computers und der zugehörigen Ressourcen. Verwenden Sie hierfür [az group create](/cli/azure/group/#create). Sie können diesen Schritt überspringen, wenn Sie bereits über eine Ressourcengruppe verfügen, die Sie stattdessen verwenden möchten:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

3. Erstellen Sie einen virtuellen Computer mit [az vm create](/cli/azure/vm/#create). Im folgenden Beispiel wird ein virtueller Computer mit dem Namen *myVM* mit einer MSI erstellt, wie vom `--assign-identity`-Parameter angefordert. Der `--admin-username`-Parameter und der `--admin-password`-Parameter geben den Namen und das Kennwort des Administratorbenutzers für die Anmeldung am virtuellen Computer an. Aktualisieren Sie diese Werte ggf. mit den entsprechenden Werten für Ihre Umgebung: 

   ```azurecli-interactive 
   az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter --generate-ssh-keys --assign-identity --admin-username azureuser --admin-password myPassword12
   ```

## <a name="enable-msi-on-an-existing-azure-vm"></a>Aktivieren von MSI auf einem vorhandenen virtuellen Azure-Computer

Wenn Sie MSI auf einem vorhandenen virtuellen Computer aktivieren möchten, gehen Sie wie folgt vor:

1. Wenn Sie nicht Azure Cloud Shell aus dem Azure-Portal verwenden, melden Sie sich zunächst mit [az login](/cli/azure/#login) bei Azure an. Verwenden Sie ein Konto, das dem Azure-Abonnement zugeordnet ist, unter dem Sie den virtuellen Computer bereitstellen möchten:

   ```azurecli-interactive
   az login
   ```

2. Verwenden Sie [az vm assign-identity](/cli/azure/vm/#az_vm_assign_identity) mit dem `--assign-identity`-Parameter, um einem vorhandenen virtuellen Computer eine MSI hinzuzufügen:

   ```azurecli-interactive
   az vm assign-identity -g myResourceGroup -n myVm
   ```

## <a name="remove-msi-from-an-azure-vm"></a>Entfernen von MSI von einem virtuellen Azure-Computer

Wenn MSI auf einem virtuellen Computer nicht mehr benötigt wird, gehen Sie wie folgt vor:

1. Wenn Sie nicht Azure Cloud Shell aus dem Azure-Portal verwenden, melden Sie sich zunächst mit [az login](/cli/azure/#login) bei Azure an. Verwenden Sie ein Konto, das dem Azure-Abonnement zugeordnet ist, unter dem Sie den virtuellen Computer bereitstellen möchten:

   ```azurecli-interactive
   az login
   ```

2. Verwenden Sie den `-n ManagedIdentityExtensionForWindows`-Switch mit [az vm extension delete](https://docs.microsoft.com/cli/azure/vm/#assign-identity), um die MSI zu entfernen:

   ```azurecli-interactive
   az vm extension delete --resource-group myResourceGroup --vm-name myVm -n ManagedIdentityExtensionForWindows
   ```

## <a name="related-content"></a>Verwandte Inhalte

- [Übersicht über verwaltete Dienstidentitäten](msi-overview.md)
- Dieser Artikel basiert auf dem Schnellstart [Erstellen eines virtuellen Windows-Computers mit CLI](../virtual-machines/windows/quick-create-cli.md) und wurde durch MSI-spezifische Anweisungen ergänzt. 

Verwenden Sie den folgenden Kommentarabschnitt, um uns Feedback zu senden und uns bei der Verbesserung unserer Inhalte zu unterstützen.

















