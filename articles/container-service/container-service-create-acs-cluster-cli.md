---
title: "Bereitstellen eines Docker-Containerclusters – Azure CLI | Microsoft-Dokumentation"
description: Bereitstellen eines Azure Container Service-Clusters per Azure CLI 2.0
services: container-service
documentationcenter: 
author: sauryadas
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: 
ms.assetid: 8da267e8-2aeb-4c24-9a7a-65bdca3a82d6
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/03/2017
ms.author: saudas
translationtype: Human Translation
ms.sourcegitcommit: 2a381431acb6436ddd8e13c69b05423a33cd4fa6
ms.openlocfilehash: 792150afa1c4f91c4bf6175037c7708d8f7bfabe
ms.lasthandoff: 02/22/2017


---
# <a name="use-the-azure-cli-20-to-create-an-azure-container-service-cluster"></a>Erstellen eines Azure Container Service-Clusters mit Azure CLI 2.0

Verwenden Sie die `az acs`-Befehle in Azure CLI 2.0, um Cluster in Azure Container Service zu erstellen und zu verwalten. Sie können einen Azure Container Service-Cluster auch mit dem [Azure-Portal](container-service-deployment.md) oder den Azure Container Service-APIs bereitstellen.

Wenn Sie Hilfe bei `az acs`-Befehlen benötigen, übergeben Sie den `-h`-Parameter an einen Befehl. Beispiel: `az acs create -h`.



## <a name="prerequisites"></a>Voraussetzungen
Zum Erstellen eines Azure Container Service-Clusters mit Azure CLI 2.0 ist Folgendes erforderlich:
* Azure-Konto ([kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/))
* Installation und Einrichtung von [Azure CLI 2.0](/cli/azure/install-az-cli2)

## <a name="get-started"></a>Erste Schritte 
### <a name="log-in-to-your-account"></a>Anmelden am Konto
```azurecli
az login 
```

Befolgen Sie die Anweisungen für die interaktive Anmeldung. Weitere Anmeldevarianten finden Sie unter [Erste Schritte mit Azure CLI 2.0](/cli/azure/get-started-with-az-cli2).

### <a name="set-your-azure-subscription"></a>Festlegen Ihres Azure-Abonnements

Wenn Sie mehrere Azure-Abonnements besitzen, legen Sie das Standardabonnement fest. Beispiel:

```
az account set --subscription "f66xxxxx-xxxx-xxxx-xxx-zgxxxx33cha5"
```


### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Erstellen Sie für jeden Cluster eine Ressourcengruppe. Legen Sie eine Azure-Region fest, in der der Azure Container Service [verfügbar](https://azure.microsoft.com/en-us/regions/services/) ist. Beispiel:

```azurecli
az group create -n acsrg1 -l "westus"
```
Die Ausgabe sieht in etwa wie folgt aus:

![Erstellen einer Ressourcengruppe](media/container-service-create-acs-cluster-cli/rg-create.png)


## <a name="create-an-azure-container-service-cluster"></a>Erstellen eines Azure Container Service-Clusters

Verwenden Sie `az acs create`, um einen Cluster zu erstellen.
Sie müssen Namen für den Cluster und für die im vorherigen Schritt erstellte Ressourcengruppe eingeben. 

Andere Eingaben werden auf die Standardwerte festgelegt (siehe Abbildung), sofern sie nicht durch die entsprechenden Switches überschrieben werden. Beispielsweise ist für den Orchestrator standardmäßig DC/OS festgelegt. Wenn Sie keinen Namen eingeben, wird ein DNS-Namenspräfix basierend auf den Namen des Clusters erstellt.

![Verwenden von „az acs create“](media/container-service-create-acs-cluster-cli/create-help.png)


### <a name="quick-acs-create-using-defaults"></a>Schnelles `acs create` mit Standardwerten
Wenn Sie einen öffentlichen SSH-RSA-Schlüssel `id_rsa.pub` am Standardspeicherort besitzen (oder einen für [OS X und Linux](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md) oder [Windows](../virtual-machines/virtual-machines-linux-ssh-from-windows.md) erstellt haben), verwenden Sie beispielsweise den folgenden Befehl:

```azurecli
az acs create -n acs-cluster -g acsrg1 -d applink789
```
Wenn Sie keinen einen öffentlichen SSH-Schlüssel besitzen, verwenden Sie diesen zweiten-Befehl. Dieser Befehl mit dem `--generate-ssh-keys`-Switch erstellt einen Schlüssel für Sie.

```azurecli
az acs create -n acs-cluster -g acsrg1 -d applink789 --generate-ssh-keys
```

Warten Sie nach dem Eingeben des Befehls ca. 10 Minuten, bis der Cluster erstellt wurde. Die Befehlsausgabe enthält die vollqualifizierten Domänennamen (FQDNs) der Master und Agent-Knoten und einen SSH-Befehl für die Verbindung mit dem ersten Master. Hier sehen Sie die abgekürzte Ausgabe:

![Abbildung: acs create](media/container-service-create-acs-cluster-cli/cluster-create.png)

> [!TIP]
> In der [exemplarischen Vorgehensweise für Kubernetes](container-service-kubernetes-walkthrough.md) erfahren Sie, wie Sie `az acs create` mit Standardwerten verwenden, um einen Kubernetes.-Cluster zu erstellen.
>

## <a name="manage-acs-clusters"></a>Verwalten von ACS-Clustern

Verwenden Sie zusätzliche `az acs`-Befehle, um Ihren Cluster zu verwalten. Hier einige Beispiele.

### <a name="list-clusters-under-a-subscription"></a>Auflisten von Clustern unter einem Abonnement

```azurecli
az acs list --output table
```

### <a name="list-clusters-in-a-resource-group"></a>Auflisten von Clustern in einer Ressourcengruppe

```azurecli
az acs list -g acsrg1 --output table
```

![acs list](media/container-service-create-acs-cluster-cli/acs-list.png)


### <a name="display-details-of-a-container-service-cluster"></a>Anzeigen der Details eines Container Service-Clusters

```azurecli
az acs show -g acsrg1 -n acs-cluster --output list
```

![acs show](media/container-service-create-acs-cluster-cli/acs-show.png)


### <a name="scale-the-cluster"></a>Skalieren des Clusters
Weder das horizontale Herunterskalieren noch das horizontale Hochskalieren sind zulässig. Der Parameter `new-agent-count` ist die neue Anzahl von Agents im ACS-Cluster.

```azurecli
az acs scale -g acsrg1 -n acs-cluster --new-agent-count 4
```

![acs scale](media/container-service-create-acs-cluster-cli/acs-scale.png)

## <a name="delete-a-container-service-cluster"></a>Löschen eines Container Service-Clusters
```azurecli
az acs delete -g acsrg1 -n acs-cluster 
```
Mit diesem Befehl werden nicht alle Ressourcen (Netzwerk und Speicher) gelöscht, die beim Erstellen des Container Service erstellt wurden. Um alle Ressourcen einfach löschen zu können, empfehlen wir, jeden Cluster in einer unterschiedlichen Ressourcengruppe bereitzustellen. Löschen Sie dann die Ressourcengruppe, wenn der Cluster nicht mehr benötigt wird.

## <a name="next-steps"></a>Nächste Schritte
Da Sie nun einen funktionierenden Cluster haben, können Sie sich die folgenden Dokumente durchlesen, um Informationen zur Verbindung und Verwaltung zu erhalten:

* [Verbinden mit einem Azure Container Service-Cluster](container-service-connect.md)
* [Verwenden von Azure Container Service und DC/OS](container-service-mesos-marathon-rest.md)
* [Verwenden von Azure Container Service und Docker Swarm](container-service-docker-swarm.md)
* [Microsoft Azure Container Service Engine - Kubernetes Walkthrough (Microsoft Azure Container Service-Modul – Exemplarische Vorgehensweise für Kubernetes)](container-service-kubernetes-walkthrough.md)
