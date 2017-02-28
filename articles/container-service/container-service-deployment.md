---
title: Bereitstellen eines Docker-Containerclusters in Azure | Microsoft-Dokumentation
description: Stellen Sie einen Azure Container Service-Cluster mit dem Azure-Portal oder der Azure Resource Manager-Vorlage bereit.
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Docker, Container, Microservices, Mesos, Azure, DC/OS, Swarm, Kubernetes, Azure Container Service, ACS
ms.assetid: 696a736f-9299-4613-88c6-7177089cfc23
ms.service: container-service
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/21/2017
ms.author: rogardle
translationtype: Human Translation
ms.sourcegitcommit: 2a381431acb6436ddd8e13c69b05423a33cd4fa6
ms.openlocfilehash: b9be92498f9daf1d2f964cc689bacb2358b237be
ms.lasthandoff: 02/22/2017


---
# <a name="deploy-an-azure-container-service-cluster"></a>Bereitstellen eines Azure Container Service-Clusters



Azure Container Service ermöglicht eine schnelle Bereitstellung beliebter Open-Source-Lösungen für Container-Clustering und die Orchestrierung. In diesem Dokument werden Sie durch die Bereitstellung eines Azure Container Service-Clusters mit dem Azure-Portal oder einer Azure Resource Manager-Schnellstartvorlage geführt. 

Sie können einen Azure Container Service-Cluster auch mit [Azure CLI 2.0](container-service-create-acs-cluster-cli.md) oder den Azure Container Service-APIs bereitstellen.



## <a name="prerequisites"></a>Voraussetzungen

* [Azure-Abonnement](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935): Falls Sie noch über kein Azure-Abonnement verfügen, können Sie sich für eine **kostenlose Testversion**registrieren.

* **Öffentlicher SSH-RSA-Schlüssel**: Bei der Bereitstellung über das Portal oder eine der Azure-Schnellstartvorlagen müssen Sie den öffentlichen Schlüssel angeben, der zur Authentifizierung für virtuelle Azure Container Service-Computer verwendet werden soll. Informationen zum Erstellen von Secure Shell (SSH)-RSA-Schlüsseln finden Sie in den Anleitungen für [OS X und Linux](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md) oder für [Windows](../virtual-machines/virtual-machines-linux-ssh-from-windows.md). 

* **Client-ID und Clientgeheimnis des Dienstprinzipals** (nur Kubernetes): Weitere Informationen und Anleitungen zum Erstellen eines Dienstprinzipals finden Sie unter [Informationen zum Dienstprinzipal für einen Kubernetes-Cluster](container-service-kubernetes-service-principal.md).



## <a name="create-a-cluster-by-using-the-azure-portal"></a>Erstellen eines Clusters mit dem Azure-Portal
1. Melden Sie sich am Azure-Portal an, wählen Sie die Option **Neu**, und suchen Sie auf dem Azure Marketplace nach **Azure Container Service**.

    ![Azure Container Service im Marketplace](media/container-service-deployment/acs-portal1.png)  <br />

2. Wählen Sie **Azure Container Service**, und klicken Sie auf **Erstellen**.

    ![Erstellen eines Containerdiensts](media/container-service-deployment/acs-portal2.png)  <br />

3. Geben Sie Folgendes ein:

    * **Benutzername**: Dies ist der Benutzername, der für ein Konto auf jedem virtuellen Computer und in jeder Skalierungsgruppe für virtuelle Computer im Azure Container Service-Cluster verwendet wird.
    * **Abonnement**: Wählen Sie ein Azure-Abonnement aus.
    * **Ressourcengruppe**: Wählen Sie eine vorhandene Ressourcengruppe aus, oder erstellen Sie eine neue Ressourcengruppe. Die bewährte Methode besteht darin, für jede Bereitstellung eine neue Ressourcengruppe zu verwenden.
    * **Standort**: Wählen Sie eine Azure-Region für die Azure Container Service-Bereitstellung aus.
    * **Öffentlicher SSH-RSA-Schlüssel**: Fügen Sie den öffentlichen Schlüssel hinzu, der für die Authentifizierung gegenüber virtuellen Azure Container Service-Computern verwendet werden soll. Es ist wichtig, dass dieser Schlüssel keine Zeilenumbrüche enthält und das Präfix `ssh-rsa` aufweist. Das Postfix `username@domain` ist optional. Der Schlüssel sollte in etwa wie folgt aussehen: **ssh-rsa AAAAB3Nz...<...>...UcyupgH azureuser@linuxvm**. 

4. Klicken Sie auf **OK** , um den Vorgang fortzusetzen.

    ![Grundlegende Einstellungen](media/container-service-deployment/acs-portal3.png)  <br />

5. Wählen Sie auf dem Blatt **Framework configuration** (Framework-Konfiguration) eine **Orchestrator configuration** (Orchestratorkonfiguration) aus. Die Optionen lauten:

  * **DC/OS**: Ein DC/OS-Cluster wird bereitgestellt.
  * **Swarm**: Ein Docker Swarm-Cluster wird bereitgestellt.
  * **Kubernetes**: Ein Kubernetes-Cluster wird bereitgestellt.


6. Klicken Sie auf **OK** , um den Vorgang fortzusetzen.

    ![Wählen Sie einen Orchestrator aus.](media/container-service-deployment/acs-portal4-new.png)  <br />

7. Wenn **Kubernetes** in der Dropdownliste ausgewählt wird, müssen Sie die Client-ID (auch als „appId“ bezeichnet) und das Clientgeheimnis (Kennwort) des Dienstprinzipals eingeben. Weitere Informationen finden Sie im Artikel zum [Dienstprinzipal für einen Kubernetes-Cluster](container-service-kubernetes-service-principal.md).

    ![Geben Sie einen Dienstprinzipal für Kubernetes ein.](media/container-service-deployment/acs-portal10.png)  <br />

7. Geben Sie auf dem Blatt mit den Einstellungen für **Azure Container Service** die folgenden Informationen ein:

    * **Masteranzahl**: Die Anzahl von Mastern im Cluster.
    * **Agent-Anzahl**: Für Docker Swarm und Kubernets ist dieser Wert die anfängliche Anzahl von Agents in der Agent-Skalierungsgruppe. Für DC/OS ist dies die anfängliche Anzahl von Agents in einer privaten Skalierungsgruppe. Darüber hinaus wird für DC/OS eine öffentliche Skalierungsgruppe mit einer vorbestimmten Anzahl von Agents erstellt. Die Anzahl der Agents in dieser öffentlichen Skalierungsgruppe wird in Abhängigkeit davon ermittelt, wie viele Master im Cluster erstellt wurden: ein öffentlicher Agent für einen Master und zwei öffentliche Agents für drei oder fünf Master.
    * **Größe der virtuellen Agent-Computer**: Die Größe der virtuellen Agent-Computer.
    * **DNS-Präfix**: Ein weltweit eindeutiger Name, der als Präfix für wichtige Teile des vollqualifizierten Domänennamens für den Dienst verwendet wird.
    * **VM diagnostics** (VM-Diagnose): Für einige Orchestratoren können Sie die VM-Diagnose aktivieren.

8. Klicken Sie auf **OK** , um den Vorgang fortzusetzen.

    ![Container Service-Einstellungen](media/container-service-deployment/acs-portal5.png)  <br />

9. Klicken Sie nach Abschluss der Dienstüberprüfung auf **OK** .

    ![Überprüfen](media/container-service-deployment/acs-portal6.png)  <br />

10. Überprüfen Sie die Bedingungen. Klicken Sie auf **Kaufen**, um den Bereitstellungsprozess zu starten.

    ![Purchase](media/container-service-deployment/acs-portal7.png)  <br />

    Falls Sie die Bereitstellung im Azure-Portal angeheftet haben, wird der Bereitstellungsstatus angezeigt.

    ![Bereitstellungsstatus](media/container-service-deployment/acs-portal8.png)  <br />

Die Bereitstellung kann einige Minuten in Anspruch nehmen. Danach ist der Azure Container Service-Cluster bereit für die Verwendung.



## <a name="create-a-cluster-by-using-a-quickstart-template"></a>Erstellen eines Clusters mit einer Schnellstartvorlage
Azure-Schnellstartvorlagen sind zum Bereitstellen eines Clusters in Azure Container Service verfügbar. Die bereitgestellten Schnellstartvorlagen können auch so geändert werden, dass sie eine zusätzliche oder erweiterte Azure-Konfiguration enthalten. Zum Erstellen eines Azure Container Service-Clusters mithilfe einer Azure-Schnellstartvorlage benötigen Sie ein Azure-Abonnement. Wenn Sie über kein Azure-Abonnement verfügen, registrieren Sie sich für eine [kostenlose Testversion](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935). 

Führen Sie zum Bereitstellen eines Clusters mithilfe einer Vorlage und von Azure CLI 2.0 die folgenden Schritte aus. (Weitere Informationen finden Sie in der [Installations- und Einrichtungsanleitung](/cli/azure/install-az-cli2.md).)

> [!NOTE] 
> In einem Windows-System können Sie ähnliche Schritte zum Bereitstellen einer Vorlage mithilfe von Azure PowerShell verwenden. Die Schritte dazu sind weiter unten in diesem Abschnitt beschrieben. Sie können eine Vorlage auch über das [Portal](../azure-resource-manager/resource-group-template-deploy-portal.md) oder mithilfe anderer Methoden bereitstellen.

1. Wählen Sie zum Bereitstellen eines DC/OS-, Docker Swarm- oder Kubernetes-Clusters in GitHub eine der verfügbaren Schnellstartvorlagen aus. Unten ist eine unvollständige Liste angegeben. Beachten Sie, dass die DC/OS- und Swarm-Vorlagen mit Ausnahme der standardmäßigen Orchestrator-Auswahl identisch sind.

    * [DC/OS-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
    * [Swarm-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)
    * [Kubernetes-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes)

2. Melden Sie sich bei Ihrem Azure-Konto (`az login`) an, und stellen Sie sicher, dass das Azure-CLI mit Ihrem Azure-Abonnement verbunden ist. Führen Sie zum Anzeigen des Standardabonnements den folgenden Befehl aus:

    ```azurecli
    az account show
    ```
    
    Wenn Sie über mehrere Abonnements verfügen und ein anderes Standardabonnement festlegen möchten, führen Sie `az account set --subscription` aus, und geben Sie die Abonnement-ID oder den Namen an.

3. Für die Bereitstellung hat es sich bewährt, eine neue Ressourcengruppe zu verwenden. Verwenden Sie zum Erstellen einer Ressourcengruppe den Befehl `az group create`, und geben Sie einen Namen für die Ressourcengruppe sowie einen Speicherort an: 

    ```azurecli
    az group create --name "RESOURCE_GROUP" --location "LOCATION"
    ```

4. Erstellen Sie eine JSON-Datei, die die erforderlichen Vorlagenparameter enthält. Laden Sie die Parameterdatei mit dem Namen `azuredeploy.parameters.json` herunter, die Sie zusammen mit der Azure Container Service-Vorlage `azuredeploy.json` in GitHub finden. Geben Sie die erforderlichen Parameterwerte für Ihren Cluster ein. 

    Wenn Sie beispielsweise die [DC/OS-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos) verwenden möchten, geben Sie die Parameterwerte für `dnsNamePrefix` und `sshRSAPublicKey` ein. Weitere Informationen finden Sie in den Beschreibungen in `azuredeploy.json` und den Optionen für andere Parameter.  
 

5. Erstellen Sie einen Container Service-Cluster, indem Sie die Bereitstellungsparameterdatei mithilfe des folgenden Befehls übergeben, wobei Folgendes gilt:

    * **RESOURCE_GROUP** ist der Name der Ressourcengruppe, die Sie im vorherigen Schritt erstellt haben.
    * **DEPLOYMENT_NAME** (optional) ist der Name, den Sie für die Bereitstellung vergeben haben.
    * **TEMPLATE_URI** ist der Speicherort der Bereitstellungsdatei `azuredeploy.json`. Dieser URI muss eine Rohdatendatei sein. Er darf kein Zeiger auf die GitHub-UI sein. Wählen Sie zum Ermitteln dieses URI in GitHub die Datei `azuredeploy.json` aus, und klicken Sie auf die Schaltfläche **Raw**.  

    ```azurecli
    az group deployment create -g RESOURCE_GROUP -n DEPLOYMENT_NAME --template-uri TEMPLATE_URI --parameters @azuredeploy.parameters.json
    ```

    Sie können Parameter auch als eine Zeichenfolge mit JSON-Formatierung in der Befehlszeile angeben. Führen Sie einen Befehl aus, der dem folgenden ähnelt:

    ```azurecli
    az group deployment create -g RESOURCE_GROUP -n DEPLOYMENT_NAME --template-uri TEMPLATE_URI --parameters "{ \"param1\": {\"value1\"} … }"
    ```

    > [!NOTE]
    > Die Bereitstellung kann einige Minuten in Anspruch nehmen.
    > 

### <a name="equivalent-powershell-commands"></a>Entsprechende PowerShell-Befehle
Sie können eine Azure Container Service-Clustervorlage auch mit PowerShell bereitstellen. Dieses Dokument basiert auf Version 1.0 des [Azure PowerShell-Moduls](https://azure.microsoft.com/blog/azps-1-0/).

1. Wählen Sie zum Bereitstellen eines DC/OS-, Docker Swarm- oder Kubernetes-Clusters in GitHub eine der verfügbaren Schnellstartvorlagen aus. Unten ist eine unvollständige Liste angegeben. Beachten Sie, dass die DC/OS- und Swarm-Vorlagen mit Ausnahme der standardmäßigen Orchestrator-Auswahl identisch sind.

    * [DC/OS-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
    * [Swarm-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)
    * [Kubernetes-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes)

2. Vergewissern sich Sie vor dem Erstellen eines Clusters in Ihrem Azure-Abonnement, dass Ihre PowerShell-Sitzung bei Azure angemeldet ist. Hierfür können Sie den Befehl `Get-AzureRMSubscription` verwenden:

    ```powershell
    Get-AzureRmSubscription
    ```

3. Wenn Sie sich bei Azure anmelden müssen, verwenden Sie den Befehl `Login-AzureRMAccount` :

    ```powershell
    Login-AzureRmAccount
    ```

4. Für die Bereitstellung hat es sich bewährt, eine neue Ressourcengruppe zu verwenden. Verwenden Sie zum Erstellen einer Ressourcengruppe den Befehl `New-AzureRmResourceGroup` , und geben Sie einen Namen für die Ressourcengruppe sowie eine Zielregion an:

    ```powershell
    New-AzureRmResourceGroup -Name GROUP_NAME -Location REGION
    ```

5. Nach dem Erstellen einer Ressourcengruppe können Sie Ihren Cluster mit dem folgenden Befehl erstellen. Der URI der gewünschten Vorlage wird für den Parameter `-TemplateUri` festgelegt. Beim Ausführen dieses Befehls werden Sie von PowerShell aufgefordert, Parameterwerte für die Bereitstellung einzugeben.

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DEPLOYMENT_NAME -ResourceGroupName RESOURCE_GROUP_NAME -TemplateUri TEMPLATE_URI
    ```

#### <a name="provide-template-parameters"></a>Eingeben von Vorlagenparametern
Wenn Sie mit PowerShell vertraut sind, wissen Sie, dass Sie die für ein Cmdlet verfügbaren Parameter durchlaufen können, indem Sie ein Minuszeichen (-) eingeben und die TAB-TASTE drücken. Dieselbe Funktionalität gilt auch für Parameter, die Sie in der Vorlage definieren. Sobald Sie den Vorlagennamen eingeben, ruft das Cmdlet die Vorlage ab, analysiert die Parameter und fügt die Vorlagenparameter dynamisch dem Befehl hinzu. Auf diese Weise wird das Angeben der Vorlagenparameterwerte stark vereinfacht. Wenn Sie einen erforderlichen Parameterwert vergessen haben, fordert PowerShell Sie zur Angabe des Werts auf.

Unten ist der vollständige Befehl mit Parametern angegeben. Sie können Ihre eigenen Werte für die Namen der Ressourcen angeben.

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName RESOURCE_GROUP_NAME-TemplateURI TEMPLATE_URI -adminuser value1 -adminpassword value2 ....
```

## <a name="next-steps"></a>Nächste Schritte
Da Sie nun einen funktionierenden Cluster haben, können Sie sich die folgenden Dokumente durchlesen, um Informationen zur Verbindung und Verwaltung zu erhalten:

* [Verbinden mit einem Azure Container Service-Cluster](container-service-connect.md)
* [Verwenden von Azure Container Service und DC/OS](container-service-mesos-marathon-rest.md)
* [Verwenden von Azure Container Service und Docker Swarm](container-service-docker-swarm.md)
* [Microsoft Azure Container Service Engine - Kubernetes Walkthrough (Microsoft Azure Container Service-Modul – Exemplarische Vorgehensweise für Kubernetes)](container-service-kubernetes-walkthrough.md)

