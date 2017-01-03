---
title: "Dienstprinzipal für Azure Kubernetes-Cluster | Microsoft-Dokumentation"
description: Erstellen und Verwalten Sie einen Azure Active Directory-Dienstprinzipal in einem Azure Container Service-Cluster mit Kubernetes.
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/20/2016
ms.author: danlep
translationtype: Human Translation
ms.sourcegitcommit: 24e12e4606a5ec4fabf7046fe9847123033bb70a
ms.openlocfilehash: 073a9e66ac68643b27ecdd44a4ecac3ad79ec218


---

# <a name="about-the-azure-active-directory-service-principal-for-a-kubernetes-cluster-in-azure-container-service"></a>Informationen zum Azure Active Directory-Dienstprinzipal für einen Kubernetes-Cluster in Azure Container Service



Für die Interaktion mit Azure-APIs benötigt Kubernetes in Azure Container Service einen [Azure Active Directory-Dienstprinzipal](../active-directory/active-directory-application-objects.md) als Dienstkonto. Der Dienstprinzipal wird für die dynamische Verwaltung von Ressourcen wie [benutzerdefinierten Routen](../virtual-network/virtual-networks-udr-overview.md) und [Azure Load Balancer](../load-balancer/load-balancer-overview.md) (Layer 4) benötigt.

In diesem Artikel werden verschiedene Optionen zum Angeben eines Dienstprinzipals für Ihren Kubernetes-Cluster gezeigt. Wenn Sie beispielsweise [Azure CLI 2.0 (Vorschau)](https://docs.microsoft.com/cli/azure/install-az-cli2) installiert und eingerichtet haben, können Sie durch Ausführen des Befehls [`az acs create`](https://docs.microsoft.com/en-us/cli/azure/acs#create) sowohl den Kubernetes-Cluster als auch den Dienstprinzipal erstellen.

> [!NOTE]
> Die Kubernetes-Unterstützung in Azure Container Service befindet sich derzeit in der Vorschauphase.


## <a name="requirements-for-the-service-principal"></a>Anforderungen für den Dienstprinzipal

Im Anschluss folgen die Anforderungen für den Azure Active Directory-Dienstprinzipal in einem Kubernetes-Cluster in Azure Container Service. 

* **Bereich:** Das Azure-Abonnement, in dem der Cluster bereitgestellt wird.

* **Rolle:** -  **Mitwirkender**

* **Clientgeheimnis:** Muss ein Kennwort sein. Derzeit kann kein für die Zertifikatauthentifizierung eingerichteter Dienstprinzipal verwendet werden.

> [!NOTE]
> Jeder Dienstprinzipal wird einer Azure Active Directory-Anwendung zugeordnet. Der Dienstprinzipal für einen Kubernetes-Cluster kann einem beliebigen gültigen Azure Active Directory-Anwendungsnamen zugeordnet werden.
> 


## <a name="service-principal-options-for-a-kubernetes-cluster"></a>Dienstprinzipaloptionen für einen Kubernetes-Cluster

### <a name="option-1-pass-the-service-principal-client-id-and-client-secret"></a>1. Option: Übergeben von Client-ID und Clientgeheimnis des Dienstprinzipals

Geben Sie beim Erstellen des Kubernetes-Clusters die **Client-ID** – häufig `appId` (Anwendungs-ID) genannt – und das **Clientgeheimnis** (`password`) eines vorhandenen Dienstprinzipals als Parameter an. Vergewissern Sie sich bei Verwendung eines vorhandenen Dienstprinzipals, dass er die im vorherigen Abschnitt genannten Anforderungen erfüllt. Wenn Sie einen Dienstprinzipal erstellen müssen, finden Sie weitere Informationen unter [Erstellen eines Dienstprinzipals](#create-a-service-principal-in-azure-active-directory) (weiter unten in diesem Artikel).

Diese Parameter können beim [Bereitstellen des Kubernetes-Clusters](./container-service-deployment.md) über das Portal, über die Azure-Befehlszeilenschnittstelle oder über Azure PowerShell angegeben werden.

>[!TIP] 
>Geben Sie als **Client-ID** nicht den `ObjectId`-Wert, sondern den `appId`-Wert des Dienstprinzipals an.
>

Das folgende Beispiel zeigt eine der Methoden, mit denen Sie die Parameter über die [Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md) im [Resource Manager-Modus](../xplat-cli-connect.md) übergeben können. In diesem Beispiel wird die [Kubernetes-Schnellstartvorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes) verwendet.

1. [Laden Sie die Vorlagenparameterdatei „azuredeploy.parameters.json“ von GitHub herunter.](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-acs-kubernetes/azuredeploy.parameters.json)

2. Geben Sie zum Angeben des Dienstprinzipals Werte für `servicePrincipalClientId` und `servicePrincipalClientSecret` in der Datei ein. (Geben Sie außerdem eigene Werte für `dnsNamePrefix` und `sshRSAPublicKey` an. Bei Letzterem handelt es sich um den öffentlichen SSH-Schlüssel für den Zugriff auf den Cluster.) Speichern Sie die Datei.

    ![Übergeben von Dienstprinzipalparametern](./media/container-service-kubernetes-service-principal/service-principal-params.png)

3. Führen Sie den folgenden Befehl aus, und legen Sie dabei mithilfe des `-e`-Parameters den Pfad der Datei „azuredeploy.parameters.json“ fest. Mit diesem Befehl wird der Cluster in einer vorhandenen Ressourcengruppe namens `myResourceGroup` bereitgestellt.

    ```CLI
    azure group deployment create -n myClusterName -g myResourceGroup --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-acs-kubernetes/azuredeploy.json" -e azuredeploy.parameters.json
    ```


### <a name="option-2-generate-the-service-principal-when-creating-the-cluster-with-the-azure-cli-20-preview"></a>2. Option: Generieren des Dienstprinzipals beim Erstellen des Clusters mit Azure CLI 2.0 (Vorschau)

Wenn Sie [Azure CLI 2.0 (Vorschau)](https://docs.microsoft.com/cli/azure/install-az-cli2) installiert und eingerichtet haben, können Sie durch Ausführen des Befehls [`az acs create`](https://docs.microsoft.com/en-us/cli/azure/acs#create) den [Cluster erstellen](./container-service-create-acs-cluster-cli.md).

Analog zu anderen Erstellungsoptionen für Kubernetes-Cluster können Sie beim Ausführen von `az acs create` Parameter für einen vorhandenen Dienstprinzipal angeben. Ohne Angabe dieser Parameter wird von Azure Container Service automatisch ein Dienstprinzipal erstellt. Hierbei handelt es sich um einen transparenten Vorgang während der Bereitstellung. 

Der folgende Befehl erstellt einen Kubernetes-Cluster und generiert sowohl SSH-Schlüssel als auch Dienstprinzipal-Anmeldeinformationen:

```console
az acs create -n myClusterName -d myDNSPrefix -g myResourceGroup --generate-ssh-keys --orchestrator-type kubernetes
```

## <a name="create-a-service-principal-in-azure-active-directory"></a>Erstellen eines Dienstprinzipals in Azure Active Directory

Wenn Sie in Azure Active Directory einen Dienstprinzipal für die Verwendung in Ihrem Kubernetes-Cluster erstellen möchten, bietet Azure dazu mehrere Möglichkeiten. 

Bei den folgenden Beispielbefehlen wird [Azure CLI 2.0 (Vorschau)](https://docs.microsoft.com/cli/azure/install-az-cli2) verwendet. Alternativ können Sie einen Dienstprinzipal auch mit der [Azure-Befehlszeilenschnittstelle](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md), mithilfe von [Azure PowerShell](../azure-resource-manager/resource-group-authenticate-service-principal.md) oder über das [klassische Portal](../azure-resource-manager/resource-group-create-service-principal-portal.md) erstellen.

> [!IMPORTANT]
> Bedenken Sie dabei immer die Anforderungen für den Dienstprinzipal (weiter oben in diesem Artikel).
>

```console
az login

az account set --subscription="mySubscriptionID"

az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/mySubscriptionID"
```

Die zurückgegebene Ausgabe sieht in etwa wie folgt aus (redigierte Darstellung):

![Erstellen eines Dienstprinzipals](./media/container-service-kubernetes-service-principal/service-principal-creds.png)

Hervorgehoben sind die **Client-ID** (`appId`) und das **Clientgeheimnis** (`password`), die Sie als Dienstprinzipalparameter für die Clusterbereitstellung verwenden.


Überprüfen Sie in einer neuen Shell Ihren Dienstprinzipal. Führen Sie dazu die folgenden Befehle aus, und ersetzen Sie dabei `appId`, `password` und `tenant`:

```console 
az login --service-principal -u yourClientID -p yourClientSecret --tenant yourTenant

az vm list-sizes --location westus
```

## <a name="additional-considerations"></a>Zusätzliche Überlegungen


* Wenn Sie die **Client-ID** des Dienstprinzipals angeben, können Sie den Wert von `appId` (wie in diesem Artikel gezeigt) oder den entsprechenden Dienstprinzipalnamen (`name`; beispielsweise `https://www.contoso.org/example`) verwenden.

* Wenn Sie den Dienstprinzipal mithilfe des Befehls `az acs create` automatisch generieren, werden die Dienstprinzipal-Anmeldeinformationen auf dem Computer, auf dem der Befehl ausgeführt wird, in die Datei „~/.azure/acsServicePrincipal.json“ geschrieben.

* Auf dem virtuellen Mastercomputer und den virtuellen Knotencomputern des Kubernetes-Clusters werden die Dienstprinzipal-Anmeldeinformationen in der Datei „/etc/kubernetes/azure.json“ gespeichert.

## <a name="next-steps"></a>Nächste Schritte

* [Erste Schritte mit Kubernetes](container-service-kubernetes-walkthrough.md) in Ihrem Container Service-Cluster



<!--HONumber=Jan17_HO1-->


