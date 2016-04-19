<properties
   pageTitle="Bereitstellen eines Azure Container Service-Clusters | Microsoft Azure"
   description="Erfahren Sie, wie Sie einen Azure Container Service-Cluster mit dem Azure-Portal, der Azure-Befehlszeilenschnittstelle oder PowerShell bereitstellen."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, Container, Microservices, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="02/16/2016"
   ms.author="rogardle"/>

# Bereitstellen eines Azure Container Service-Clusters

Azure Container Service ermöglicht eine schnelle Bereitstellung beliebter Open-Source-Lösungen für Container-Clustering und die Orchestrierung. Mithilfe von Azure Container Service können Sie Marathon Mesos- und Docker Swarm-Cluster mit Azure Resource Manager-Vorlagen oder im Azure-Portal bereitstellen. Sie stellen diese Cluster bereit, indem Sie Azure Virtual Machine-Skalierungsgruppen verwenden. Für die Cluster werden Azure-Netzwerk- und Speicherangebote genutzt. Für den Zugriff auf Azure Container Service benötigen Sie ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement haben, können Sie sich für eine [kostenlose Testversion](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935) registrieren.

In diesem Dokument werden Sie durch die Bereitstellung eines Azure Container Service-Clusters mit dem [Azure-Portal](#creating-a-service-using-the-azure-portal), der [Azure-Befehlszeilenschnittstelle (CLI)](#creating-a-service-using-the-azure-cli) und dem [Azure PowerShell-Modul](#creating-a-service-using-powershell) geführt.

## Erstellen eines Diensts mit dem Azure-Portal

Wählen Sie zum Bereitstellen eines Mesos- oder Docker Swarm-Clusters in GitHub eine der folgenden Vorlagen aus. Beachten Sie, dass diese beiden Vorlagen mit Ausnahme der standardmäßigen Orchestrator-Auswahl identisch sind.

* [Mesos-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-mesos)
* [Swarm-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)

Wenn Sie auf einer Vorlagenseite jeweils die Schaltfläche **In Azure bereitstellen** wählen, wird das Azure-Portal mit einem Formular geöffnet, das in etwa wie folgt aussieht: <br />

![Bereitstellung mit Formular erstellen](media/create-mesos-params.png) <br />

Füllen Sie das Formular mithilfe dieser Anleitung aus, und wählen Sie **OK**, wenn Sie damit fertig sind. <br />

Feld | Beschreibung
----------------|-----------
DNSNAMEPREFIX | Dies muss ein eindeutiger Wert sein. Dieser wird zum Erstellen von DNS-Namen für jeden wichtigen Teil des Diensts verwendet. Weitere Informationen finden Sie weiter unten in diesem Artikel.
AGENTCOUNT | Dies ist die Anzahl der virtuellen Computer, die in der Skalierungsgruppe des Azure Container Service-Agents erstellt werden.
AGENTVMSIZE | Dies ist die Größe der virtuellen Agent-Computer. Wählen Sie eine Größe, die genügend Ressourcen bietet, um die größten Container zu hosten.
ADMINUSERNAME | Dies ist der Benutzername, der für ein Konto auf jedem virtuellen Computer und in jeder Skalierungsgruppe für virtuelle Computer im Azure Container Service-Cluster verwendet wird.
ORCHESTRATORTYPE| Dies ist der Orchestratortyp, der im Azure Container Service-Cluster verwendet wird.
MASTERCOUNT | Dies ist die Anzahl der virtuellen Computer, die als Master für den Cluster konfiguriert werden. Sie können 1 auswählen, aber dies bietet in Ihrem Cluster keinerlei Resilienz und wird daher nur für Testzwecke empfohlen. Für einen Produktionscluster ist der Wert 3 oder 5 zu empfehlen.
SSHRSAPUBLICKEY | Sie müssen die Secure Shell (SSH) für die Authentifizierung gegenüber den virtuellen Computern verwenden. Hier fügen Sie Ihren öffentlichen Schlüssel hinzu. Es ist wichtig, dass Sie den Schlüsselwert sorgfältig in dieses Feld einfügen. Manchmal enthält der Inhalt einen Zeilenumbruch, wodurch der Schlüssel geteilt wird. Stellen Sie sicher, dass Ihr Schlüssel keine Zeilenumbrüche enthält und das Präfix „ssh-rsa“ sowie das Postfix „username@domain“ enthält. Er sollte etwa wie folgt aussehen: „ssh-rsa AAAAB3Nz...SNIPPEDCONTENT...UcyupgH azureuser@linuxvm'“. Wenn Sie einen SSH-Schlüssel erstellen müssen, finden Sie auf der Website mit der Azure-Dokumentation Informationen zu [Windows](../virtual-machines/virtual-machines-linux-ssh-from-windows.md) und [Linux](../virtual-machines/virtual-machines-linux-ssh-from-linux.md).

Wählen Sie **OK**, nachdem Sie die richtigen Werte für Ihre Parameter festgelegt haben. Vergeben Sie als Nächstes einen Namen für die Ressourcengruppe, wählen Sie eine Region aus, lesen Sie die rechtlichen Bedingungen, und stimmen Sie diesen zu.

> [AZURE.NOTE] Während der Vorschauphase werden für Azure Container Service keine Gebühren berechnet. Es fallen nur die standardmäßigen Computegebühren an, z.B. Gebühren für virtuelle Computer, Speicher und Netzwerk.

![Ressourcengruppe auswählen](media/resourcegroup.png)

Wählen Sie abschließend **Erstellen**. Wechseln Sie zurück zum Dashboard. Wenn Sie das Kontrollkästchen **An Dashboard anheften** auf dem Bereitstellungsblatt nicht deaktiviert haben, wird eine animierte Kachel angezeigt, die in etwa wie folgt aussieht:

![Vorlagenkachel bereitstellen](media/deploy.png)

Jetzt lehnen Sie sich zurück und entspannen Sie sich, während der Cluster erstellt wird. Nach der Erstellung des Clusters werden einige Blätter mit den Ressourcen angezeigt, aus denen der Azure Container Service-Cluster besteht.

![Abgeschlossen](media/final.png)

## Erstellen eines Diensts mit der Azure-Befehlszeilenschnittstelle

Um eine Instanz von Azure Container Service über die Befehlszeile zu erstellen, benötigen Sie ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement haben, können Sie sich für eine [kostenlose Testversion](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935) registrieren. Außerdem muss die Azure-Befehlszeilenschnittstelle installiert und konfiguriert sein.

Wählen Sie zum Bereitstellen eines Mesos- oder Docker Swarm-Clusters in GitHub eine der folgenden Vorlagen aus. Beachten Sie, dass diese beiden Vorlagen mit Ausnahme der standardmäßigen Orchestrator-Auswahl identisch sind.

* [Mesos-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-mesos)
* [Swarm-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)

Als Nächstes stellen Sie sicher, dass die Azure-Befehlszeilenschnittstelle mit einem Azure-Abonnement verbunden ist. Hierfür können Sie den folgenden Befehl verwenden:

```bash
Azure account show
```
Wenn ein Azure-Konto nicht zurückgegeben wird, können Sie den folgenden Befehl verwenden, um die Befehlszeilenschnittstelle bei Azure anzumelden.

```bash
azure login -u user@domain.com
```

Konfigurieren Sie anschließend die Azure-Befehlszeilenschnittstellen-Tools für die Verwendung von Azure Resource Manager.

```bash
azure config mode arm
```

Wenn Sie Ihren Cluster in einer neuen Ressourcengruppe erstellen möchten, müssen Sie zunächst die Ressourcengruppe erstellen. Verwenden Sie den folgenden Befehl, bei dem `GROUP_NAME` der Name der zu erstellenden Ressourcengruppe ist und `REGION` für die Region steht, in der Sie die Ressourcengruppe erstellen möchten:

```bash
azure group create GROUP_NAME REGION
```

Nach dem Erstellen der Ressourcengruppe können Sie den Cluster mit diesem Befehl erstellen, wobei Folgendes gilt:

- **RESOURCE\_GROUP** ist der Name der Ressourcengruppe, die für diesen Dienst verwendet werden soll.
- **DEPLOYMENT\_NAME** ist der Name dieser Bereitstellung.
- **TEMPLATE\_URI** ist der Speicherort der Bereitstellungsdatei. Beachten Sie, dass dies die RAW-Datei sein muss, *kein* Zeiger auf die GitHub-UI. Wählen Sie zum Suchen nach dieser URL die Datei „azuredeploy.json“ in GitHub, und klicken Sie auf die Schaltfläche **RAW**.

> [AZURE.NOTE] Beim Ausführen dieses Befehls werden Sie von der Shell aufgefordert, Parameterwerte für die Bereitstellung einzugeben.

```bash
azure group deployment create RESOURCE_GROUP DEPLOYMENT_NAME --template-uri TEMPLATE_URI
```

### Eingeben von Vorlagenparametern

Bei dieser Version des Befehls müssen Sie Parameter interaktiv definieren. Wenn Sie Parameter angeben möchten, z.B. eine Zeichenfolge mit JSON-Formatierung, ist dies mit dem Switch `-p` möglich. Beispiel:

 ```bash
azure group deployment create RESOURCE_GROUP DEPLOYMENT_NAME --template-uri TEMPLATE_URI -p '{ "param1": "value1" … }'
 ```

Alternativ können Sie eine JSON-formatierte Parameterdatei mit dem Switch `-e` angeben:

 ```bash
azure group deployment create RESOURCE_GROUP DEPLOYMENT_NAME --template-uri TEMPLATE_URI -e PATH/FILE.JSON
 ```

Um eine Parameterdatei als Beispiel anzuzeigen, die den Namen `azuredeploy.parameters.json` hat, können Sie über die Azure Container Service-Vorlagen in GitHub danach suchen.

## Erstellen eines Diensts mit PowerShell

Sie können einen Azure Container Service-Cluster auch mit PowerShell bereitstellen. Dieses Dokument basiert auf Version 1.0 des [Azure PowerShell-Moduls](https://azure.microsoft.com/blog/azps-1-0/).

Wählen Sie zum Bereitstellen eines Mesos- oder Docker Swarm-Clusters eine der folgenden Vorlagen aus. Beachten Sie, dass diese beiden Vorlagen mit Ausnahme der standardmäßigen Orchestrator-Auswahl identisch sind.

* [Mesos-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-mesos)
* [Swarm-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)

Prüfen Sie vor dem Erstellen eines Clusters in Ihrem Azure-Abonnement, dass Ihre PowerShell-Sitzung bei Azure angemeldet ist. Hierzu können Sie den Befehl `Get-AzureRMSubscription` verwenden:

```powershell
Get-AzureRmSubscription
```

Wenn Sie sich bei Azure anmelden müssen, verwenden Sie den Befehl `Login-AzureRMAccount`:

```powershell
Login-AzureRmAccount
```

Bei der Bereitstellung in einer neuen Ressourcengruppe müssen Sie zunächst die Ressourcengruppe erstellen. Verwenden Sie zum Erstellen einer neuen Ressourcengruppe den Befehl `New-AzureRmResourceGroup`, und legen Sie einen Namen für die Ressourcengruppe sowie eine Zielregion fest:

```powershell
New-AzureRmResourceGroup -Name GROUP_NAME -Location REGION
```

Nach dem Erstellen einer Ressourcengruppe können Sie Ihren Cluster mit dem folgenden Befehl erstellen. Der URI der gewünschten Vorlage wird für den Parameter `-TemplateUri` festgelegt. Beim Ausführen dieses Befehls werden Sie von PowerShell aufgefordert, Parameterwerte für die Bereitstellung einzugeben.

```powershell
New-AzureRmResourceGroupDeployment -Name DEPLOYMENT_NAME -ResourceGroupName RESOURCE_GROUP_NAME -TemplateUri TEMPLATE_URI
 ```

### Eingeben von Vorlagenparametern

Wenn Sie mit PowerShell vertraut sind, wissen Sie, dass Sie die für ein Cmdlet verfügbaren Parameter durchlaufen können, indem Sie ein Minuszeichen (-) eingeben und die TAB-TASTE drücken. Dieselbe Funktionalität gilt auch für Parameter, die Sie in der Vorlage definieren. Sobald Sie den Vorlagennamen eingeben, ruft das Cmdlet die Vorlage ab, analysiert die Parameter und fügt die Vorlagenparameter dynamisch dem Befehl hinzu. Auf diese Weise wird das Angeben der Vorlagenparameterwerte stark vereinfacht. Wenn Sie einen erforderlichen Parameterwert vergessen haben, fordert PowerShell Sie zur Angabe des Werts auf.

Unten ist der vollständige Befehl mit Parametern angegeben. Sie können Ihre eigenen Werte für die Namen der Ressourcen angeben.

```
New-AzureRmResourceGroupDeployment -ResourceGroupName RESOURCE_GROUP_NAME-TemplateURI TEMPLATE_URI -adminuser value1 -adminpassword value2 ....
```

## Nächste Schritte

Da Sie nun einen funktionierenden Cluster haben, können Sie sich die Artikel mit den Informationen zur Verbindung und Verwaltung ansehen.

- [Verbinden mit einem Azure Container Service-Cluster](./container-service-connect.md)
- [Verwenden von Azure Container Service und Mesos](./container-service-mesos-marathon-rest.md)

<!---HONumber=AcomDC_0413_2016-->