---
title: Continuous Integration und Deployment von Docker-Anwendungen mit mehreren Containern in Azure Container Service | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie eine Docker-App mit mehreren Containern erstellen und in einem Azure Container Service-Cluster mit DC/OS bereitstellen.
services: container-service
documentationcenter: 
author: spboyer
manager: wpickett
editor: 
tags: acs, azure-container-service
keywords: Docker, Container, Microservices, Mesos, Azure
ms.assetid: b16b767a-2eaa-418a-becc-8c032713a1f2
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/14/2016
ms.author: johnsta
translationtype: Human Translation
ms.sourcegitcommit: 71fdc7b13fd3b42b136b4907c3d747887fde1a19
ms.openlocfilehash: cdcb2a8493c6790a395251c4cf05f2a6c0770c8d


---

# <a name="continuous-integration-and-deployment-of-multi-container-docker-applications-to-azure-container-service"></a>Continuous Integration und Deployment von Docker-Anwendungen mit mehreren Containern in Azure Container Service 
In diesem Tutorial wird veranschaulicht, wie Sie eine Docker-App mit mehreren Containern erstellen und in einem Azure Container Service-Cluster mit DC/OS bereitstellen. Die Vorteile von Continuous Integration und Deployment (CI/CD) sind zwar bekannt, bei der Integration von Containern in Ihren Workflow sind jedoch neue Aspekte zu berücksichtigen. Mithilfe der neuen Azure-Containerregistrierung und CLI-Befehle haben wir einen End-to-End-Ablauf entwickelt, den Sie anpassen können.

## <a name="get-started"></a>Erste Schritte
Sie können diese exemplarische Vorgehensweise unter OS X, Windows oder Linux ausführen.
- Sie benötigen ein Azure-Abonnement. Wenn Sie keines besitzen, können Sie sich [für ein Konto registrieren](https://azure.microsoft.com/).
- Installieren Sie die [Azure-Befehlszeilentools](https://github.com/Azure/azure-cli#microsoft-azure-cli-20---preview).

## <a name="what-well-create"></a>Erstellte Elemente
Befassen wir uns mit einigen wichtigen Aspekten der App und dem von uns eingerichteten Bereitstellungsablauf:
* **Die Anwendung setzt sich aus mehreren Diensten zusammen.** Docker-Assets, Docker-Datei und „docker-compose.yml“ werden zum Definieren der in separaten Containern ausgeführten Dienste in unserer App verwendet. Dadurch wird das unabhängige Skalieren einzelner App-Bereiche ermöglicht, und jeder Dienst kann in einer anderen Programmiersprache und einem anderen Programmierframework geschrieben werden. Der Code der App kann in mehreren Git-Quellrepositorys gehostet werden. (Die Tools unterstützen derzeit GitHub und Visual Studio Team Services).

* Die App wird in einem **mit DC/OS konfigurierten ACS-Cluster** ausgeführt. Der Containerorchestrator kann die Integrität des Clusters verwalten und sicherstellen, dass die erforderliche Anzahl von Containerinstanzen weiter ausgeführt werden. 

* Der Prozess zur **Erstellung und Bereitstellung von Containerimages wird ohne Ausfallzeiten vollständig automatisiert**. Entwickler im Team sollen die Übertragung an eine Verzweigung per Git Push ausführen, da dadurch automatisch ein Integrationsvorgang ausgelöst wird. Das bedeutet, dass sie Containerimages erstellen und markieren, Tests für jeden Container ausführen und diese Images per Push an eine private Docker-Registrierung übertragen müssen. Von dort werden neue Images automatisch in einer freigegebenen Präproduktionsumgebung auf einem ACS-Cluster für weitere Tests bereitgestellt.

* **Stufen Sie ein Release von einer Umgebung zur nächsten herauf**, beispielsweise von der Entwicklungsumgebung auf die Test-, Staging- oder Produktionsumgebung. Beim Heraufstufen auf eine Downstream-Umgebung müssen die Containerimages nicht neu erstellt werden, um sicherzustellen, dass die gleichen Images bereitgestellt werden, die in einer vorherigen Umgebung getestet wurden. Diesem Prozess liegt das Konzept der *unveränderlichen Dienste* zugrunde, wodurch sich die Wahrscheinlichkeit verringert, dass sich nicht erkannte Fehler in die Produktion einschleichen.

* Zur möglichst effizienten Nutzung der Computeressourcen im ACS-Cluster verwenden wir denselben Cluster zum Ausführen von Buildaufgaben, um Build- und Bereitstellungsschritte vollständig in Containern auszuführen. Der Cluster hostet darüber hinaus unsere verschiedenen Entwicklungs-/Test- und Produktionsumgebungen.


## <a name="create-an-azure-container-service-cluster-configured-with-dcos"></a>Erstellen eines mit DC/OS konfigurierten Azure Container Service-Clusters

>[!IMPORTANT]
> Zum Erstellen eines sicheren Clusters übergeben Sie die Datei mit dem öffentlichen SSH-Schlüssel beim Aufrufen von `az acs create`. Entweder kann Azure CLI 2.0 die Schlüssel für Sie generieren und gleichzeitig mit der Option `--generate-ssh-keys` übergeben, oder Sie übergeben den Pfad zu Ihren Schlüsseln mit der Option `--ssh-key-value` (Standardspeicherort ist unter Linux `~/.ssh/id_rsa.pub` und unter Windows `%HOMEPATH%\.ssh\id_rsa.pub`, dieser kann sich jedoch ändern). Informationen zum Erstellen von Dateien mit öffentlichen und privaten SSH-Schlüsseln unter Linux finden Sie unter [Erstellen von SSH-Schlüsseln unter Linux und Mac für Linux-VMs in Azure](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md?toc=%2fazure%2fcontainer-services%2ftoc.json). Informationen zum Erstellen von Dateien mit öffentlichen und privaten SSH-Schlüsseln unter Windows finden Sie unter [Verwenden von SSH mit Windows in Azure](../virtual-machines/virtual-machines-linux-ssh-from-windows.md?toc=%2fazure%2fcontainer-services%2ftoc.json). 

1. Geben Sie zunächst den Befehl [az login](/cli/azure/#login) in ein Terminalfenster ein, um sich mit der Azure-CLI bei Ihrem Azure-Abonnement anzumelden: 

    `az login`

1. Erstellen Sie mit [az resource group create](/cli/azure/resource/group#create) eine Ressourcengruppe, in der wir unseren Cluster ablegen:
    
    `az resource group create --name myacs-rg --location westus`

    Geben Sie dabei ggf. die am nächsten gelegene [Azure-Datencenterregion](https://azure.microsoft.com/regions) an. 

1. Erstellen Sie mithilfe von [az acs create](/cli/azure/acs#create) einen ACS-Cluster, und übergeben Sie den Pfad zur Datei mit dem öffentlichen SSH-Schlüssel: 

    ```azurecli
    az acs create \
    --resource-group myacs-rg 
    --name myacs \
    --dns-prefix myacs \
    --ssh-key-value ~/.ssh/id_rsa.pub
    ```
    
Dieser Schritt dauert einige Minuten. Sie können in der Zwischenzeit weiterlesen.  Mit dem Befehl `acs create` werden Informationen zum neu erstellten Cluster zurückgegeben. (Die ACS-Cluster in Ihrem Abonnement können mit `az acs list` aufgelistet werden.) Weitere Optionen für die ACS-Konfiguration werden im Thema zum [Erstellen und Konfigurieren eines ACS-Clusters](container-service-deployment.md) beschrieben.

## <a name="set-up-sample-code"></a>Konfigurieren von Beispielcode
Während der Erstellung des Clusters können wir Beispielcode zur Bereitstellung in ACS erstellen.

1. [Verzweigen](https://help.github.com/articles/fork-a-repo/) Sie das GitHub-Beispielrepository, sodass Sie über eine eigene Kopie verfügen: [https://github.com/azure-samples/container-service-dotnet-continuous-integration-multi-container.git](https://github.com/azure-samples/container-service-dotnet-continuous-integration-multi-container.git). Bei der App handelt es sich im Grunde um eine Version von „Hello World“ mit mehreren Containern.
1. Nachdem Sie eine Verzweigung in Ihrem eigenen GitHub-Konto erstellt haben, klonen Sie das Repository lokal auf Ihrem Computer:

    ```bash
    git clone  https://github.com/your-github-account/container-service-dotnet-continuous-integration-multi-container.git
    cd container-service-dotnet-continuous-integration-multi-container
    ```
    
Sehen wir uns den Code genauer an:
* `/service-a` ist eine Angular.js-basierte Web-App mit einem Node.js-Back-End.
* `/service-b` ist ein .NET Core-Dienst und wird über REST mit `service-a` aufgerufen.
* Sowohl `service-a` als auch `service-b` enthalten ein `Dockerfile`-Element in ihren Verzeichnissen, das jeweils Node.js- und .NET Core-basierte Containerimages beschreibt. 
* `docker-compose.yml` deklariert den Satz von Diensten, die erstellt und bereitgestellt werden.
* Zusätzlich zu `service-a` und `service-b` führt ein dritter Dienst namens `cache` eine Redis Cache-Instanz aus, die von `service-a` verwendet werden kann. `cache` unterscheidet sich von den ersten beiden Diensten insofern, dass unser Quell-Repository keinen Code dafür enthält. Stattdessen wird ein vorgefertigtes `redis:alpine`-Image von Docker Hub abgerufen und in ACS bereitgestellt.
* `/service-a/server.js` enthält Code, in dem `service-a` sowohl `service-b` als auch `cache` aufruft. Beachten Sie, dass `service-a`-Code anhand des jeweiligen Namens in `docker-compose.yml` auf `service-b` und `cache` verweist. Bei der Ausführung dieser Dienste auf dem lokalen Computer über `docker-compose` stellt Docker sicher, dass die Dienste richtig vernetzt sind und sich gegenseitig anhand des Namens ermitteln können. Die Ausführung der Dienste in einer Clusterumgebung mit Netzwerken mit Lastenausgleich macht die Ermittlung in der Regel wesentlich komplexer als die lokale Ausführung. Die gute Nachricht ist, dass mit den Azure-CLI-Befehlen ein CI/CD-Ablauf eingerichtet wird, der sicherstellt, dass dieser einfache Dienstermittlungscode weiterhin unverändert in ACS ausgeführt wird. 

    ![Übersicht über die Beispiel-App mit mehreren Containern](media/container-service-setup-ci-cd/multi-container-sample-app-overview.PNG)

## <a name="set-up-continuous-integration-and-deployment"></a>Einrichten von Continuous Integration und Continuous Deployment
1. Stellen Sie sicher, dass der ACS-Cluster vorbereitet ist: Führen Sie [az acs list](/cli/azure/acs#list) aus, und überprüfen Sie, ob der ACS-Cluster aufgeführt ist. (Hinweis: ACS muss DC/OS 1.8 oder höher ausführen.)

1. [Erstellen Sie ein persönliches GitHub-Zugriffstoken](https://help.github.com/articles/creating-an-access-token-for-command-line-use/), und gewähren Sie ihm mindestens den Zugriff auf den Bereich `repo`.  Kopieren Sie das Token unbedingt in die Zwischenablage, da wir es im nächsten Befehl benötigen (damit wird ein [Webhook](https://help.github.com/articles/about-webhooks/) in unserem GitHub-Repository erstellt). 

1. Legen Sie für das aktuelle Verzeichnis den Stamm des geklonten Quell-Repositorys fest, und erstellen Sie eine Build- und Releasepipeline. Verwenden Sie dazu das soeben erstellte Token _&lt;GitHubPersonalAccessToken&gt;:
    
    `cd container-service-dotnet-continuous-integration-multi-container`

    ```azurecli
    az container release create \
    --target-name myacs \
    --target-resource-group myacs-rg \
    --remote-access-token <GitHubPersonalAccessToken>
    ```

    Dabei steht `--target-name` für den Namen Ihres ACS-Clusters und `--target-resource-group` für den Ressourcengruppennamen des ACS-Clusters.

Bei der ersten Ausführung kann das Abschließen dieses Befehls etwa eine Minute dauern. Anschließend werden wichtige Informationen zur erstellten Build- und Releasepipeline zurückgegeben:
* `sourceRepo`: Für das Quell-Repository wird ein [Webhook](https://help.github.com/articles/about-webhooks/) konfiguriert, sodass die Build- und Releasepipeline jedes Mal automatisch ausgelöst wird, wenn Quellcode per Push an sie übertragen wird.  
* `vstsProject`: [Visual Studio Team Services](https://www.visualstudio.com/team-services/) (VSTS) wird zur *Ausführung* des Workflows konfiguriert (die eigentlichen Build- und Bereitstellungsaufgaben werden in Containern in ACS ausgeführt). Wenn Sie ein bestimmtes VSTS-Konto und -Projekt verwenden möchten, können Sie sie mithilfe der Parameter `--vsts-account-name` und `--vsts-project-name` festlegen.
* `buildDefinition`: Definiert die Aufgaben, die für jeden Build ausgeführt werden. Containerimages werden für jeden in „docker-compose.yml“ definierten Dienst erstellt und anschließend per Push an eine Docker-Containerregistrierung übertragen. 
* `containerRegistry`: Bei der Azure-Containerregistrierung handelt es sich um einen verwalteten Dienst, der eine Docker-Containerregistrierung ausführt. Eine neue Azure-Containerregistrierung mit dem Standardnamen wird erstellt. Alternativ können Sie mit dem `--registry-name`-Parameter einen Namen für die Azure-Containerregistrierung angeben.
* `releaseDefinition`: Definiert die Aufgaben, die für jede Bereitstellung ausgeführt werden. Containerimages für die „docker-compose.yml“ definierten Dienste werden aus der Containerregistrierung abgerufen und im ACS-Cluster bereitgestellt. Standardmäßig werden drei Umgebungen erstellt: *Entwicklung*, *Test* und *Produktion*. Die Releasedefinition ist standardmäßig so konfiguriert, dass sie bei jedem erfolgreichen Abschluss eines Builds automatisch in einer Entwicklungsumgebung **bereitgestellt wird. Ein Release kann manuell ohne Neuerstellung auf *Test* oder *Produktion* heraufgestuft werden. Die Standardablauf kann in VSTS angepasst werden. 
* `containerService`: Der ACS-Zielcluster (mit DC/OS 1.8 oder höher)


Der folgende Codeausschnitt zeigt einen Beispielbefehl, den Sie eingeben, wenn Sie bereits über eine vorhandene Azure-Containerregistrierung mit dem Namen `myregistry` verfügen. Erstellen Sie Releasedefinitionen mit einem VSTS-Konto unter `myvstsaccount.visualstudio.com` und einem vorhandenen VSTS-Projekt `myvstsproject`:
        
```azurecli
az container release create \
--target-name myacs \
--target-resource-group myacs-rg \
--registry-name myregistry \
--vsts-account-name myvstsaccount \
--vsts-project-name myvstsproject \
--remote-access-token <GitHubPersonalAccessToken>
```

## <a name="view-deployment-pipeline-progress"></a>Anzeigen des Bereitstellungspipelinestatus
Nach der Erstellung der Pipeline wird automatisch eine erstmaliger Build- und Bereitstellungsprozess ausgelöst. Nachfolgende Builds werden jedes Mal ausgelöst, wenn Code per Push an das Quell-Repository übertragen wird. Sie können den Status eines Builds und/oder Release überprüfen, indem Sie im Browser die URL der Build- oder Releasedefinition öffnen.

Die einem ACS-Cluster zugeordnete Releasedefinitions-URL kann jederzeit mit folgendem Befehl abgerufen werden:

```azurecli
az container release list \
--target-name myacs \
--target-resource-group myacs-rg
``` 

![VSTS-Build](media/container-service-setup-ci-cd/vsts-build.PNG)

*VSTS-Screenshot mit den CI-Ergebnissen unserer App mit mehreren Containern*

![VSTS-Release](media/container-service-setup-ci-cd/vsts-release.PNG)

*VSTS-Docker-Compose-Release mit mehreren Umgebungen*

## <a name="view-the-application"></a>Anzeigen der Anwendung
Zu diesem Zeitpunkt wird unsere Anwendung in der freigegebenen Entwicklerumgebung, aber nicht öffentlich verfügbar gemacht. Verwenden Sie in der Zwischenzeit das DC/OS-Dashboard, um die Dienste anzuzeigen und zu verwalten und [einen SSH-Tunnel zu den zu DC/OS gehörigen Endpunkten zu erstellen](https://azure.microsoft.com/documentation/articles/container-service-connect/), oder führen Sie einen von der Azure-CLI bereitgestellten praktischen Befehl aus.

> [!IMPORTANT]
> Stellen Sie bei der ersten Bereitstellung sicher, dass das VSTS-Release erfolgreich bereitgestellt wurde, bevor Sie den Vorgang fortsetzen.

> [!NOTE]
> Nur unter Windows: Zum Abschließen dieses Abschnitts müssen Sie [Pageant](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) einrichten.
> 
>* Starten Sie *PuttyGen*, und laden Sie den privaten SSH-Schlüssel, den Sie zum Erstellen des ACS-Clusters verwendet haben (%homepath%\id_rsa).
>* Speichern Sie den privaten SSH-Schlüssel als `id_rsa.ppk` im selben Ordner.
>* Starten Sie *Pageant*. Die Ausführung wird gestartet, und auf der Taskleiste rechts unten wird ein Symbol angezeigt.
>* Klicken Sie mit rechten Maustaste auf das Taskleistensymbol, und wählen Sie *Schlüssel hinzufügen* aus.
>* Fügen Sie die Datei `id_rsa.ppk` hinzu.
> 
> 

1. Öffnen Sie das DC/OS-Dashboard des ACS-Clusters mit dem praktischen Befehl der Azure-CLI:
    
    `az acs dcos browse -g myacs-rg -n myacs`

    * `-g` ist der Name der Ressourcengruppe des ACS-Zielclusters.
    * `-n` ist der Name des ACS-Zielclusters.
    * Unter Umständen werden Sie zur Eingabe des Kennworts für das lokale Konto aufgefordert, da für diesen Befehl Administratorberechtigungen erforderlich sind. Der Befehl erstellt einen SSH-Tunnel zu einem DC/OS-Endpunkt, öffnet den Endpunkt im Standardbrowser und konfiguriert vorübergehend den Webproxy des Browsers. 

    > [!TIP]
    > Wenn Sie den Namen des ACS-Clusters abrufen möchten, können Sie durch die Ausführung von `az acs list` alle ACS-Cluster in Ihrem Abonnement auflisten. 

1. Klicken Sie im DC/OS-Dashboard im linken Navigationsmenü auf **Services** (Dienste) ([http://localhost/#/services](http://localhost/#/services)). Über unsere Pipeline bereitgestellte Dienste werden unter einem Stammordner namens *dev* (benannt nach der Umgebung in der VSTS-Releasedefinition) gruppiert. 

![Marathon-Benutzeroberfläche](media/container-service-setup-ci-cd/marathon-ui.png)

Über das DC/OS-Dashboard können viele nützliche Aufgaben ausgeführt werden.

* Nachverfolgen des Bereitstellungsstatus für jeden Dienst
* Anzeigen von CPU- und Arbeitsspeicheranforderungen
* Anzeigen von Protokollen
* Skalieren der Anzahl von Instanzen für jeden Dienst

**So zeigen Sie die Webanwendung für „service-a“ an**: Navigieren Sie zum Stammordner *dev*, und führen Sie dann in der Ordnerhierarchie einen Drilldown bis zu `service-a` aus. In dieser Ansicht werden die ausgeführten Aufgaben (oder Containerinstanzen) für `service-a` aufgelistet.

![service a](media/container-service-setup-ci-cd/service-a.png)

Klicken Sie auf eine Aufgabe, um ihre Ansicht zu öffnen, und klicken Sie anschließend auf einen der verfügbaren Endpunkte.

![service a-Aufgabe](media/container-service-setup-ci-cd/service-a-task.PNG)

Unsere einfache Web-App ruft `service-a` auf, wodurch wiederum `service-b` aufgerufen wird, und gibt eine Hello World-Nachricht zurück. In Redis wird jedes Mal, wenn eine Anforderung erfolgt, die Anzahl erhöht.

![service a-Web-App](media/container-service-setup-ci-cd/service-a-web-app.PNG)

### <a name="optional-reaching-a-service-from-the-command-line"></a>(Optional) Erreichen eines Diensts über die Befehlszeile
Gehen Sie wie folgt vor, wenn Sie einen Dienst von der Befehlszeile aus über Curl erreichen möchten:

1. Führen Sie `az acs dcos browse --verbose -g myacs-rg -n myacs` aus, und notieren Sie nach Eingabe des Kennworts die Zeile „Proxy running on <IP-Adresse>:<port>“.
1. Geben Sie in einem neuen Terminalfenster Folgendes ein:

    `export http_proxy=http://<web-proxy-service-ip>:<portnumber>`

    Beispiel: `export http_proxy=http://127.0.0.1:55405`

1. Nun können Sie Curl für Ihren Dienstendpunkt `curl http://service-url` ausführen. Dabei steht `service-url` für die Adresse, die angezeigt wird, wenn Sie auf der Marathon-Benutzeroberfläche zum Dienstendpunkt navigieren. Um die Festlegung der Variablen „http_proxy“ in der Befehlszeile aufzuheben, geben Sie `unset http_proxy` ein.
 

## <a name="scale-services"></a>Skalieren von Diensten
Skalieren wir über das DC/OS-Dashboard auch gleich unsere Dienste.
1. Navigieren Sie zu der Anwendung im Unterordner *dev*.
1. Zeigen Sie auf `service-b`, klicken Sie auf das Zahnradsymbol, und wählen Sie **Scale** (Skalieren).

    ![Menü "Aktion"](media/container-service-setup-ci-cd/marathon-ui-action-menu.PNG)

1. Erhöhen Sie die Anzahl auf 3, und klicken Sie auf **Scale Service** (Dienst skalieren).

    ![Skalieren von Diensten](media/container-service-setup-ci-cd/marathon-ui-scale-service.png)

1. Navigieren Sie zurück zur ausgeführten Web-App, und klicken Sie wiederholt auf die Schaltfläche *Say It Again* (Wiederholen). Hinweis: Für `service-b`-Aufrufe wird ein Roundrobin für eine Sammlung von Hostnamen ausgeführt, während die einzelne Instanz von `service-a` weiterhin den gleichen Host meldet.   

## <a name="promote-a-release-to-downstream-environments-without-rebuilding-container-images"></a>Heraufstufen eines Release auf Downstream-Umgebungen ohne Neuerstellung von Containerimages
Von der VSTS-Releasepipeline wurden standardmäßig drei Umgebungen eingerichtet: *Entwicklung*, *Test* und *Produktion*. Bisher ist die Bereitstellung nur in *Entwicklung* erfolgt. Sehen wir uns an, wie wir ein Release auf die nächste Downstream-Umgebung (*Test*) heraufstufen, ohne die Containerimages neu zu erstellen. Mit diesem Workflow wird sichergestellt, dass genau die gleichen Images bereitgestellt werden, die wir in der vorherigen Umgebung getestet haben. Darüber hinaus basiert er auf *unveränderlichen Diensten* und verringert die Wahrscheinlichkeit, dass sich nicht erkannte Fehler in die Produktion einschleichen.

1. Navigieren Sie auf der VSTS-Web-UI zu **Releases**.

    ![VSTS-Menü mit Releases](media/container-service-setup-ci-cd/vsts-releases-menu.PNG)

1. Öffnen Sie das aktuelle Release.

1. Klicken Sie auf der Menüleiste der Releasedefinition auf **Bereitstellen**, und wählen Sie **Test** als die nächste Umgebung, in der eine neue Bereitstellung gestartet werden soll. Dabei sollen die gleichen Images wiederverwendet werden, die zuvor in *Entwicklung* bereitgestellt wurden. Klicken Sie auf **Protokolle**, wenn Sie weitere Details zur Bereitstellung wünschen.

    ![Heraufstufen des Release durch VSTS](media/container-service-setup-ci-cd/vsts-promote-release.PNG)

Nach der erfolgreichen Bereitstellung in *Test*, wird auf der Marathon-UI ein neuer Stammordner namens *test* erstellt, der die ausgeführten Dienste für diese Umgebung enthält. 

![Unterordner für jede Umgebung in DC/OS](media/container-service-setup-ci-cd/marathon-ui-dev-test-environments.png)

## <a name="trigger-a-new-build-and-deployment"></a>Auslösen eines neuen Builds und einer Bereitstellung
Wir simulieren nun, was geschieht, wenn ein Entwickler im Team eine Codeänderung per Push an das Quell-Repository überträgt.

1. Öffnen Sie `service-a/public/index.html` im Code-Editor. 
1. Ändern Sie die Codezeile

    `<h2>Server Says</h2>`

    etwa in

    `<h2>Server Says Hello</h2>`

1. Speichern Sie die Datei, führen Sie dann einen Commit aus, und übertragen Sie die Codeänderung per Push an das Quell-Repository.

    ```bash
    git commit -am 'updated title'
    git push
    ```

Der Commit startet automatisch einen neuen Build und ein neues Release, das in *Entwicklung* bereitgestellt wird. Dienste in Downstream-Umgebungen (*Test* oder *Produktion*) werden nicht geändert, bis wir ein bestimmtes Release auf die gewünschte Umgebung heraufstufen.

Wenn Sie die Builddefinition in VSTS öffnen, sehen Sie ungefähr Folgendes: 

![Durch Git Push ausgelöster neuer Build](media/container-service-setup-ci-cd/new-build.png)



## <a name="expose-public-endpoint-for-production"></a>Verfügbarmachen des öffentlichen Endpunkts für die Produktion

1. Fügen Sie einer neuen Datei namens `docker-compose.env.production.yml` im Stammordner des Quell-Repositorys den folgenden YAML-Code hinzu. Dadurch wird eine Bezeichnung hinzugefügt, die bewirkt, dass ein öffentlicher Endpunkt für `service-a` verfügbar gemacht wird. 
    
    ```yaml
    version: "2"
    services:
      service-a:
        labels:
          com.microsoft.acs.dcos.marathon.vhost: "<FQDN, or custom domain>"
    ```

    * Als Bezeichnungswert können Sie entweder die URL des vollqualifizierten Domänennamens (Fully Qualified Domain Name, FQDN) des ACS-Agents oder eine benutzerdefinierte Domäne (z.B. „app.contoso.com“) angeben. Führen Sie zum Ermitteln des FQDN Ihres ACS-Agents den Befehl `az acs list` aus, und überprüfen Sie die Eigenschaft für `agentPoolProfiles.fqdn`. Beispiel: `myacsagents.westus.cloudapp.azure.com`.
    * Diese Einstellungen folgen der Dateibenennungskonvention „docker-compose.env.*Umgebungsname*.yml“ und betreffen nur die genannte Umgebung (in diesem Fall die Umgebung *Produktion*). Überprüfen Sie die Releasedefinition in VSTS: Jede Bereitstellungsaufgabe der Umgebung ist zum Lesen aus einer anhand dieser Konvention benannten Docker-Compose-Datei konfiguriert.

1. Führen Sie einen Commit aus, und übertragen Sie die Datei per Push an das Masterquell-Repository, um einen weiteren Build zu starten.

    ```bash
    git add .
    git commit -am "expose public port for service-a"
    git push
    ```

1. Warten Sie, bis das Update erstellt und in *Entwicklung* bereitgestellt wurde. Stufen Sie es anschließend auf *Test* und dann auf *Produktion* herauf. (In diesem Tutorial können Sie es direkt in *Produktion* bereitstellen, es wird jedoch empfohlen, die Bereitstellung in der nächsten Downstream-Umgebung zu üben.)

1. (Optional) **Wenn Sie für vhost eine benutzerdefinierte Domäne angegeben haben** (z.B. „app.contoso.com“), fügen Sie in den Einstellungen Ihres Domänenanbieters einen DNS-Eintrag hinzu. Melden Sie sich bei der administrativen Benutzeroberfläche Ihres Domänenanbieters an, und fügen Sie wie folgt einen DNS-Eintrag hinzu:

    * Typ: CNAME
    * Host: Ihre benutzerdefinierte Domäne, beispielsweise „app.contoso.com“
    * Antwort: FQDN des ACS-Agents, beispielsweise „myacsagents.westus.cloudapp.azure.com“
    * TTL (Optional): In manchen Fällen ermöglicht der Domänenanbieter die Bearbeitung der Gültigkeitsdauer (TTL). Ein niedrigerer Wert bewirkt, dass ein DNS-Eintrag schneller weitergegeben wird.   

1. Sobald das Release in der *Produktionsumgebung* bereitgestellt wurde, kann jeder Benutzer auf diese Version zugreifen. Öffnen Sie im Browser die URL, die Sie für die Bezeichnung `com.microsoft.acs.dcos.marathon.vhost` angegeben haben. (Hinweis: Releases in Präproduktionsumgebungen sind weiterhin privat).

## <a name="summary"></a>Zusammenfassung
Glückwunsch! Sie haben gelernt, wie Sie einen ACS-Cluster mit DC/OS erstellen und eine vollständig automatisierte, in Containern enthaltene Build- und Bereitstellungspipeline für eine App mit mehreren Containern einrichten.

Weitere Schritte, die Sie ausprobieren können:
* **Skalieren von VSTS-Agents.** Wenn für ausgeführte Build- und Releaseaufgaben ein höherer Durchsatz erforderlich ist, können Sie die Anzahl von VSTS-Agentinstanzen erhöhen. Navigieren Sie auf dem DC/OS-Dashboard zu **Services** (Dienste), öffnen Sie den Ordner „vsts-agents“, und experimentieren Sie mit der Skalierung der Anzahl von VSTS-Agentinstanzen.
* **Integrieren von Komponententests.** Dieses GitHub-Repository zeigt, wie Sie die Ausführung von Komponenten- und Integrationtests in Containern festlegen und diese in die Buildaufgaben aufnehmen: [https://github.com/mindaro/sample-app](https://github.com/mindaro/sample-app). 
    * Tipp: Sehen Sie sich folgende Dateien im Repository an: `service-a/unit-tests.js`, `service-a/service-tests.js`, `docker-compose.ci.unit-tests.yml` und `docker-compose.ci.service-tests.yml`.

## <a name="clean-up"></a>Bereinigen
Um die im Zusammenhang mit diesem Tutorial entstehenden Computegebühren zu beschränken, führen Sie den folgenden Befehl aus, und notieren Sie sich die Bereitstellungspipelineressourcen für den ACS-Cluster:

```azurecli 
az container release list --resource-name myacs --resource-group myacs-rg
```

Löschen des ACS-Clusters:
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Ermitteln Sie die Ressourcengruppe, die den ACS-Cluster enthält.
1. Öffnen Sie die UI des Blatts mit der Ressourcengruppe, und klicken Sie auf der Befehlsleiste des Blatts auf **Löschen**.

Löschen der Azure-Containerregistrierung:
1. Suchen Sie im Azure-Portal nach der Azure-Containerregistrierung, und löschen Sie sie. 

Das [Visual Studio Team Services-Konto bietet eine kostenlose Basiszugriffsebene für die ersten fünf Benutzer](https://azure.microsoft.com/en-us/pricing/details/visual-studio-team-services/). Sie können die Build- und Releasedefinitionen jedoch löschen.
1. Löschen der VSTS-Builddefinition:
        
    * Öffnen Sie die Builddefinitions-URL im Browser, und klicken Sie dann auf den Link **Builddefinitionen** (neben dem Namen der derzeit angezeigten Builddefinition).
    * Klicken Sie neben der zu löschenden Builddefinition auf das Aktionsmenü, und wählen Sie **Definition löschen**.

    ![Löschen der VSTS-Builddefinition](media/container-service-setup-ci-cd/vsts-delete-build-def.png) 

1. Löschen der VSTS-Releasedefinition:

    * Öffnen Sie die Releasedefinitions-URL im Browser.
    * Klicken Sie in der Liste der Releasedefinitionen auf der linken Seite auf das Dropdownmenü neben der zu löschenden Releasedefinition, und wählen Sie **Löschen**.

    ![Löschen der VSTS-Releasedefinition](media/container-service-setup-ci-cd/vsts-delete-release-def.png)



<!--HONumber=Nov16_HO3-->


