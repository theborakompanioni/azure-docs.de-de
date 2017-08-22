---
title: 'Azure CLI-Skriptbeispiel: Erstellen von ACS-Windows-Kubernetes-Cluster | Microsoft-Dokumentation'
description: 'Azure CLI-Skriptbeispiel: Erstellen von ACS-Windows-Kubernetes-Cluster'
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Docker, Container, Microservices, Kubernetes, DC/OS, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/30/2017
ms.author: nepeters
ms.translationtype: HT
ms.sourcegitcommit: bfd49ea68c597b109a2c6823b7a8115608fa26c3
ms.openlocfilehash: 9ca289817b54c39c59271f35a0af26bad2811da6
ms.contentlocale: de-de
ms.lasthandoff: 07/25/2017

---

# <a name="create-an-azure-container-service-kubernetes-windows-cluster"></a>Erstellen Sie ein Azure Container Service-Kubernetes-Windows-Cluster

In diesem Beispiel wird ein Azure Container Service-Cluster mit Kubernetes für Windows-basierte Container ausgeführt.

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

```azurecli
az group create --name myResourceGroup --location eastus

az acs create \
  --orchestrator-type kubernetes \
  --resource-group myResourceGroup \
  --name myK8SCluster \
  --generate-ssh-keys \
  --admin-username azureuser \
  --admin-password Password12 \
  --windows
```

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen zu entfernen.

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle zum Erstellen der Bereitstellung. Jedes Element in der Tabelle ist mit der befehlsspezifischen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az acs create](https://docs.microsoft.com/cli/azure/acs#create) | Erstellt einen ACS-Cluster. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Weitere CLI-Skriptbeispiele von Azure Container Service finden Sie in der [Dokumentation zu Azure Container Service](../cli-samples.md).

