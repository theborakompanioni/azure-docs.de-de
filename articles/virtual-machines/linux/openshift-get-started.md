---
title: Bereitstellen von OpenShift Origin in Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie OpenShift Origin auf virtuellen Azure-Computern bereitstellen.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: jbinder
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 
ms.author: jbinder
ms.translationtype: Human Translation
ms.sourcegitcommit: 1e6f2b9de47d1ce84c4043f5f6e73d462e0c1271
ms.openlocfilehash: e03da05625e440eab29ccc28a2343d3433fc7607
ms.contentlocale: de-de
ms.lasthandoff: 06/21/2017

---

# <a name="deploy-openshift-origin-to-azure-virtual-machines"></a>Bereitstellen von OpenShift Origin auf virtuellen Azure-Computern 

[OpenShift Origin](https://www.openshift.org/) ist eine Open-Source-Containerplattform auf Basis von [Kubernetes](https://kubernetes.io/). Sie vereinfacht das Bereitstellen, Skalieren und Betreiben von Anwendungen mit mehreren Mandanten. 

Dieser Leitfaden erläutert, wie Sie OpenShift Origin mithilfe der Azure-CLI und mit Azure Resource Manager-Vorlagen auf virtuellen Azure-Computern bereitstellen. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines KeyVault zum Verwalten von SSH-Schlüsseln für den OpenShift-Cluster
> * Bereitstellen eines OpenShift-Clusters auf virtuellen Azure-Computern 
> * Installieren und Konfigurieren der [OpenShift-CLI](https://docs.openshift.org/latest/cli_reference/index.html#cli-reference-index) zum Verwalten des Clusters
> * Anpassen der OpenShift-Bereitstellung

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

Für diesen Schnellstart ist Version 2.0.8 oder höher der Azure CLI erforderlich. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="log-in-to-azure"></a>Anmelden an Azure 
Melden Sie sich mit dem Befehl [az login](/cli/azure/#login) bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm, oder klicken Sie auf **Ausprobieren**, um Cloud Shell zu verwenden.

```azurecli 
az login
```
## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#create) eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. 

Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *eastus*.

```azurecli 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-key-vault"></a>Erstellen eines Schlüsseltresors
Erstellen Sie einen KeyVault zum Speichern der SSH-Schlüssel für den Cluster, indem Sie den Befehl [az keyvault create](/cli/azure/keyvault#create) verwenden.  

```azurecli 
az keyvault create --resource-group myResourceGroup --name myKeyVault \
       --enabled-for-template-deployment true \
       --location eastus
```

## <a name="create-an-ssh-key"></a>Erstellen eines SSH-Schlüssels 
Ein SSH-Schlüssel ist erforderlich, um den Zugriff auf den OpenShift Origin-Cluster sicherzustellen. Erstellen Sie ein SSH-Schlüsselpaar mit dem `ssh-keygen`-Befehl. 
 
 ```bash
ssh-keygen -f ~/.ssh/openshift_rsa -t rsa -N ''
```

> [!NOTE]
> Das erstellte SSH-Schlüsselpaar darf keine Passphrase verwenden.

Weitere Informationen zu SSH-Schlüsseln unter Windows finden Sie unter [Gewusst wie: Verwenden von SSH-Schlüsseln mit Windows in Azure](/azure/virtual-machines/linux/ssh-from-windows).

## <a name="store-ssh-private-key-in-key-vault"></a>Speichern des privaten SSH-Schlüssels im Key Vault
Die OpenShift-Bereitstellung verwendet den erstellten SSH-Schlüssel zum Sicherstellen des Zugriffs auf den OpenShift-Master. Damit die Bereitstellung den SSH-Schlüssel sicher abrufen kann, speichern Sie den Schlüssel im Schlüsseltresor. Verwenden Sie hierzu den folgenden Befehl.

# <a name="enabled-for-template-deployment"></a>Aktivieren für die Vorlagenbereitstellung
```azurecli
az keyvault secret set --vault-name KeyVaultName --name OpenShiftKey --file ~/.ssh/openshift.rsa
```

## <a name="create-a-service-principal"></a>Erstellen eines Dienstprinzipals 
OpenShift kommuniziert mit Azure unter Verwendung von Benutzername und Kennwort oder eines Dienstprinzipals. Ein Azure-Dienstprinzipal ist eine Sicherheitsidentität, die Sie mit Apps, Diensten und Automatisierungstools wie OpenShift verwenden können. Sie steuern und definieren die Berechtigungen hinsichtlich der Vorgänge, die der Dienstprinzipal in Azure ausführen können soll. Zur Erhöhung der Sicherheit über die Bereitstellung eines Benutzernamens und Kennworts hinaus wird in diesem Beispiel ein einfacher Dienstprinzipal erstellt.

Erstellen Sie mit [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac) einen Dienstprinzipal, und geben Sie die Anmeldeinformationen aus, die OpenShift benötigt:

```azurecli
az ad sp create-for-rbac --name openshiftsp \
          --role Contributor --password {strong password} \
          --scopes $(az group show --name myResourceGroup --query id)
```
Notieren Sie die „appId“-Eigenschaft, die der Befehl zurückgibt.
```json
{
  "appId": "a487e0c1-82af-47d9-9a0b-af184eb87646d",
  "displayName": "openshiftsp",
  "name": "http://openshiftsp",
  "password": {strong password},
  "tenant": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
}
```
 > [!WARNING] 
 > Vermeiden Sie es, ein unsicheres Kennwort zu erstellen.  Befolgen Sie die Anleitung unter [Kennwortrichtlinien und -einschränkungen in Azure Active Directory](/azure/active-directory/active-directory-passwords-policy).

Weitere Informationen über Dienstprinzipale finden Sie unter [Erstellen eines Azure-Dienstprinzipals mit Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli).

## <a name="deploy-the-openshift-origin-template"></a>Bereitstellen der OpenShift Origin-Vorlage
Stellen Sie nun OpenShift Origin mithilfe einer Azure Resource Manager-Vorlage bereit. 

> [!NOTE] 
> Der folgende Befehl erfordert Azure CLI 2.0.8 oder höher. Sie können die Azure CLI-Version mit dem `az --version`-Befehl überprüfen. Informationen zum Aktualisieren der CLI-Version finden Sie unter [Installieren der Azure CLI 2.0]( /cli/azure/install-azure-cli).

Verwenden Sie den `appId` Wert des zuvor erstellten Dienstprinzipals für den `aadClientId`-Parameter.

```azurecli 
az group deployment create --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-origin/master/azuredeploy.json \
      --params \ 
        openshiftMasterPublicIpDnsLabel=myopenshiftmaster \
        infraLbPublicIpDnsLabel=myopenshiftlb \
        openshiftPassword=Pass@word!
        sshPublicKey=~/.ssh/openshift_rsa.pub \
        keyVaultResourceGroup=myResourceGroup \
        keyVaultName=myKeyVault \
        keyVaultSecret=OpenShiftKey \
        aadClientId={appId} \
        aadClientSecret={strong password} 
```
Die Bereitstellung kann bis zu 20 Minuten in Anspruch nehmen. Die URL der OpenShift-Konsole und der DNS-Name des OpenShift-Masters werden am Terminal ausgegeben, wenn die Bereitstellung abgeschlossen ist.

```json
{
  "OpenShift Console Uri": "http://openshiftlb.cloudapp.azure.com:8443/console",
  "OpenShift Master SSH": "ocpadmin@myopenshiftmaster.cloudapp.azure.com"
}
```
## <a name="connect-to-the-openshift-cluster"></a>Herstellen einer Verbindung mit dem OpenShift-Cluster
Wenn die Bereitstellung abgeschlossen ist, stellen Sie eine Verbindung mit der OpenShift-Konsole her, indem Sie `OpenShift Console Uri` über den Browser verwenden. Alternativ können Sie eine Verbindung mit dem OpenShift-Master mithilfe des folgenden Befehls herstellen.

```bash
$ ssh ocpadmin@myopenshiftmaster.cloudapp.azure.com
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wenn Ressourcengruppe, OpenShift-Cluster und alle zugehörigen Ressourcen nicht mehr benötigt werden, können Sie sie mit dem Befehl [az group delete](/cli/azure/group#delete) entfernen.

```azurecli 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:
> [!div class="checklist"]
> * Erstellen eines KeyVault zum Verwalten von SSH-Schlüsseln für den OpenShift-Cluster
> * Bereitstellen eines OpenShift-Clusters auf virtuellen Azure-Computern 
> * Installieren und Konfigurieren der [OpenShift-CLI](https://docs.openshift.org/latest/cli_reference/index.html#cli-reference-index) zum Verwalten des Clusters

Der OpenShift Origin-Cluster wurde bereitgestellt. In den OpenShift-Tutorials erfahren Sie, wie Sie Ihre erste Anwendung bereitstellen und die OpenShift-Tools verwenden können. Informationen zu den ersten Schritten finden Sie in [Getting Started with OpenShift Origin](https://docs.openshift.org/latest/getting_started/index.html) (Erste Schritte mit OpenShift Origin). 

