---
title: Bereitstellen eines Azure Container Service-Clusters | Microsoft Docs
description: Erfahren Sie, wie Sie einen Azure Container Service-Cluster mit dem Azure-Portal, der Azure-Befehlszeilenschnittstelle oder PowerShell bereitstellen.
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
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/13/2016
ms.author: rogardle
translationtype: Human Translation
ms.sourcegitcommit: a7d957fd4be4c823077b1220dfb8ed91070a0e97
ms.openlocfilehash: d056b9489eba1f97e8fb87f231b03d104c4cab66


---
# <a name="deploy-an-azure-container-service-cluster"></a>Bereitstellen eines Azure Container Service-Clusters
Azure Container Service ermöglicht eine schnelle Bereitstellung beliebter Open-Source-Lösungen für Container-Clustering und die Orchestrierung. Mithilfe von Azure Container Service können Sie DC/OS-, Kubernetes- und Docker Swarm-Cluster mit Azure Resource Manager-Vorlagen oder im Azure-Portal bereitstellen. Sie stellen diese Cluster mithilfe von Azure VM-Skalierungsgruppen bereit. Für die Cluster werden Azure-Netzwerk- und -Speicherangebote genutzt. Für den Zugriff auf Azure Container Service benötigen Sie ein Azure-Abonnement. Wenn Sie über kein Azure-Abonnement verfügen, können Sie sich für eine [kostenlose Testversion](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935)registrieren.

In diesem Dokument werden Sie durch die Bereitstellung eines Azure Container Service-Clusters mit dem [Azure-Portal](#creating-a-service-using-the-azure-portal), der [Azure-Befehlszeilenschnittstelle (CLI)](#creating-a-service-using-the-azure-cli) und dem [Azure PowerShell-Modul](#creating-a-service-using-powershell) geführt.  

## <a name="create-a-service-by-using-the-azure-portal"></a>Erstellen eines Diensts mit dem Azure-Portal
Melden Sie sich am Azure-Portal an, wählen Sie die Option **Neu**, und suchen Sie auf dem Azure Marketplace nach **Azure Container Service**.

![Bereitstellung erstellen 1](media/acs-portal1.png)  <br />

Wählen Sie **Azure Container Service**, und klicken Sie auf **Erstellen**.

![Bereitstellung erstellen 2](media/acs-portal2.png)  <br />

Geben Sie Folgendes ein:

* **Benutzername**: Dies ist der Benutzername, der für ein Konto auf jedem virtuellen Computer und in jeder Skalierungsgruppe für virtuelle Computer im Azure Container Service-Cluster verwendet wird.
* **Abonnement**: Wählen Sie ein Azure-Abonnement aus.
* **Ressourcengruppe**: Wählen Sie eine vorhandene Ressourcengruppe aus, oder erstellen Sie eine neue Ressourcengruppe.
* **Standort**: Wählen Sie eine Azure-Region für die Azure Container Service-Bereitstellung aus.
* **Öffentlicher SSH-Schlüssel**: Fügen Sie den öffentlichen Schlüssel hinzu, der für die Authentifizierung gegenüber Azure Container Dienst Virtual Machines verwendet werden soll. Es ist sehr wichtig, dass dieser Schlüssel keine Zeilenumbrüche enthält und das Präfix „ssh-rsa“ sowie das Postfix 'username@domain' aufweist. Er sollte in etwa wie folgt aussehen: **ssh-rsa AAAAB3Nz...<...>...UcyupgH azureuser@linuxvm**. Eine Anleitung zum Erstellen von SSH-Schlüsseln (Secure Shell) finden Sie in den Artikeln zu [Linux](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-ssh-from-linux/) und [Windows](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-ssh-from-windows/).

Klicken Sie auf **OK** , um den Vorgang fortzusetzen.

![Bereitstellung erstellen 3](media/acs-portal3.png)  <br />

Wählen Sie einen Orchestrierungstyp. Die Optionen sind:

* **DC/OS**: Ein DC/OS-Cluster wird bereitgestellt.
* **Swarm**: Ein Docker Swarm-Cluster wird bereitgestellt.
* **Kubernetes**: Ein Kubernetes-Cluster wird bereitgestellt.

Klicken Sie auf **OK** , um den Vorgang fortzusetzen.

![Bereitstellung erstellen 4](media/acs-portal4-new.png)  <br />

Wenn **Kubernetes** im Dropdownfeld ausgewählt wird, müssen Sie die Dienstprinzipalclient-ID sowie ein Dienstprinzipal-Clientgeheimnis eingeben.
Weitere Informationen zum Erstellen eines Dienstprinzipals finden Sie auf [dieser](https://github.com/Azure/acs-engine/blob/master/docs/serviceprincipal.md) Seite 

![Bereitstellung 4.5 erstellen](media/acs-portal10.PNG)  <br />

Geben Sie Folgendes ein:

* **Masteranzahl**: Die Anzahl von Mastern im Cluster. Wenn „Kubernetes“ ausgewählt ist, wird die Anzahl von Mastern auf einen Standardwert von 1 festgelegt.
* **Agent-Anzahl**: Für Docker Swarm und Kubernets ist dies die anfängliche Anzahl von Agents in der Agent-Skalierungsgruppe. Für DC/OS ist dies die anfängliche Anzahl von Agents in einer privaten Skalierungsgruppe. Darüber hinaus wird eine öffentliche Skalierungsgruppe mit einer vorbestimmten Anzahl von Agents erstellt. Die Anzahl der Agents in dieser öffentlichen Skalierungsgruppe wird in Abhängigkeit davon ermittelt, wie viele Master im Cluster erstellt wurden: ein öffentlicher Agent für einen Master und zwei öffentliche Agents für drei oder fünf Master.
* **Größe der virtuellen Agent-Computer**: Die Größe der virtuellen Agent-Computer.
* **DNS-Präfix**: Ein weltweit eindeutiger Name, der als Präfix für wichtige Teile des vollqualifizierten Domänennamens für den Dienst verwendet wird.

Klicken Sie auf **OK** , um den Vorgang fortzusetzen.

![Bereitstellung erstellen 5](media/acs-portal5.png)  <br />

Klicken Sie nach Abschluss der Dienstüberprüfung auf **OK** .

![Bereitstellung erstellen 6](media/acs-portal6.png)  <br />

Klicken Sie auf **Erstellen** , um den Bereitstellungsprozess zu starten.

![Bereitstellung erstellen 7](media/acs-portal7.png)  <br />

Falls Sie die Bereitstellung im Azure-Portal angeheftet haben, wird der Bereitstellungsstatus angezeigt.

![Bereitstellung erstellen 8](media/acs-portal8.png)  <br />

Nachdem die Bereitstellung abgeschlossen ist, ist der Azure Container Service-Cluster bereit für die Verwendung.

## <a name="create-a-service-by-using-the-azure-cli"></a>Erstellen eines Diensts mit der Azure-Befehlszeilenschnittstelle
Um eine Instanz von Azure Container Service über die Befehlszeile zu erstellen, benötigen Sie ein Azure-Abonnement. Wenn Sie über kein Azure-Abonnement verfügen, können Sie sich für eine [kostenlose Testversion](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935)registrieren. Außerdem muss die Azure-Befehlszeilenschnittstelle [installiert](../xplat-cli-install.md) und [konfiguriert](../xplat-cli-connect.md) sein.

Wählen Sie zum Bereitstellen eines DC/OS- oder Docker Swarm- oder Kubernetes-Clusters in GitHub eine der folgenden Vorlagen aus. 

* [DC/OS-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
* [Swarm-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)
* [Kubernetes-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes)

Vergewissern Sie sich als Nächstes, dass die Azure-Befehlszeilenschnittstelle mit einem Azure-Abonnement verbunden ist. Hierfür können Sie den folgenden Befehl verwenden:

```bash
azure account show
```
Wenn ein Azure-Konto nicht zurückgegeben wird, können Sie den folgenden Befehl verwenden, um die Befehlszeilenschnittstelle bei Azure anzumelden.

```bash
azure login -u user@domain.com
```

Konfigurieren Sie anschließend die Azure-Befehlszeilenschnittstellen-Tools für die Verwendung von Azure Resource Manager.

```bash
azure config mode arm
```

Erstellen Sie mit dem folgenden Befehl eine Azure-Ressourcengruppe und einen Container Service-Cluster. Hierbei gilt Folgendes:

* **RESOURCE_GROUP** ist der Name der Ressourcengruppe, die für diesen Dienst verwendet werden soll.
* **LOCATION** ist die Azure-Region, in der die Ressourcengruppe und die Azure Container Service-Bereitstellung erstellt werden.
* **TEMPLATE_URI** ist der Speicherort der Bereitstellungsdatei. Beachten Sie, dass dies die RAW-Datei sein muss, kein Zeiger auf die GitHub-UI. Wählen Sie zum Ermitteln dieser URL in GitHub die Datei „azuredeploy.json“ aus, und klicken Sie auf die Schaltfläche **Raw** .

> [!NOTE]
> Beim Ausführen dieses Befehls werden Sie von der Shell aufgefordert, Parameterwerte für die Bereitstellung einzugeben.
> 
> 

```bash
azure group create -n RESOURCE_GROUP DEPLOYMENT_NAME -l LOCATION --template-uri TEMPLATE_URI
```

### <a name="provide-template-parameters"></a>Eingeben von Vorlagenparametern
Bei dieser Version des Befehls müssen Sie Parameter interaktiv definieren. Parameter (beispielsweise eine Zeichenfolge mit JSON-Formatierung) können mit dem Switch `-p` angegeben werden. Beispiel:

 ```bash
azure group deployment create RESOURCE_GROUP DEPLOYMENT_NAME --template-uri TEMPLATE_URI -p '{ "param1": "value1" … }'
```

Alternativ können Sie mit dem Switch `-e` eine JSON-formatierte Parameterdatei angeben:

```bash
azure group deployment create RESOURCE_GROUP DEPLOYMENT_NAME --template-uri TEMPLATE_URI -e PATH/FILE.JSON
```

Eine Beispielparameterdatei namens `azuredeploy.parameters.json`können Sie sich über die Azure Container Service-Vorlagen in GitHub ansehen.

## <a name="create-a-service-by-using-powershell"></a>Erstellen eines Diensts mit PowerShell
Sie können einen Azure Container Service-Cluster auch mit PowerShell bereitstellen. Dieses Dokument basiert auf Version 1.0 des [Azure PowerShell-Moduls](https://azure.microsoft.com/blog/azps-1-0/).

Wählen Sie zum Bereitstellen eines DC/OS- oder Docker Swarm- oder Kubernetes-Clusters eine der folgenden Vorlagen aus. Beachten Sie, dass diese beiden Vorlagen mit Ausnahme der standardmäßigen Orchestrator-Auswahl identisch sind.

* [DC/OS-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
* [Swarm-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)
* [Kubernetes-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes)

Vergewissern sich Sie vor dem Erstellen eines Clusters in Ihrem Azure-Abonnement, dass Ihre PowerShell-Sitzung bei Azure angemeldet ist. Hierfür können Sie den Befehl `Get-AzureRMSubscription` verwenden:

```powershell
Get-AzureRmSubscription
```

Wenn Sie sich bei Azure anmelden müssen, verwenden Sie den Befehl `Login-AzureRMAccount` :

```powershell
Login-AzureRmAccount
```

Bei der Bereitstellung in einer neuen Ressourcengruppe müssen Sie zunächst die Ressourcengruppe erstellen. Verwenden Sie zum Erstellen einer neuen Ressourcengruppe den Befehl `New-AzureRmResourceGroup` , und geben Sie einen Namen für die Ressourcengruppe sowie eine Zielregion an:

```powershell
New-AzureRmResourceGroup -Name GROUP_NAME -Location REGION
```

Nach dem Erstellen einer Ressourcengruppe können Sie Ihren Cluster mit dem folgenden Befehl erstellen. Der URI der gewünschten Vorlage wird für den Parameter `-TemplateUri` festgelegt. Beim Ausführen dieses Befehls werden Sie von PowerShell aufgefordert, Parameterwerte für die Bereitstellung einzugeben.

```powershell
New-AzureRmResourceGroupDeployment -Name DEPLOYMENT_NAME -ResourceGroupName RESOURCE_GROUP_NAME -TemplateUri TEMPLATE_URI
```

### <a name="provide-template-parameters"></a>Eingeben von Vorlagenparametern
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




<!--HONumber=Nov16_HO5-->


