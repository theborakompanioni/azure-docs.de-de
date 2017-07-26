---
title: Continuous Integration und Continuous Deployment (CI/CD) mit Azure Container Service und Swarm | Microsoft-Dokumentation
description: Verwenden Sie Azure Container Service mit Docker Swarm, eine Azure-Containerregistrierung und Visual Studio Team Services zum laufenden Bereitstellen einer .NET Core-Anwendung mit mehreren Containern.
services: container-service
documentationcenter: " "
author: jcorioland
manager: pierlag
tags: acs, azure-container-service
keywords: Docker, Container, Microservices, Swarm, Azure, Visual Studio Team Services, DevOps
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/08/2016
ms.author: jucoriol
ms.translationtype: Human Translation
ms.sourcegitcommit: e22bd56e0d111add6ab4c08b6cc6e51c364c7f22
ms.openlocfilehash: 86472a523b457d6fe06940accba0794e2d37395e
ms.contentlocale: de-de
ms.lasthandoff: 05/19/2017


---

# <a name="full-cicd-pipeline-to-deploy-a-multi-container-application-on-azure-container-service-with-docker-swarm-using-visual-studio-team-services"></a>Vollständige CI/CD-Pipeline zum Bereitstellen einer Anwendung mit mehreren Containern in Azure Container Service mit Docker Swarm mithilfe von Visual Studio Team Services

Eine der größten Herausforderungen bei der Entwicklung moderner Anwendungen für die Cloud ist die Fähigkeit, diese Anwendungen fortlaufend bereitzustellen. In diesem Artikel erfahren Sie, wie Sie eine vollständige CI/CD-Pipeline (Continuous Integration/Continuous Deployment) mithilfe von Azure Container Service mit Docker Swarm, Azure-Containerregistrierung sowie den Build- und Release Management-Funktionen von Visual Studio Team Services implementieren.

Dieser Artikel basiert auf einer einfachen Anwendung, die auf [GitHub](https://github.com/jcorioland/MyShop/tree/acs-docs) verfügbar ist und mit ASP.NET Core entwickelt wurde. Die Anwendung besteht aus vier Diensten: drei Web-APIs und einem Web-Front-End:

![Beispielanwendung „MyShop“](./media/container-service-docker-swarm-setup-ci-cd/myshop-application.png)

Ziel ist die fortlaufende Bereitstellung dieser Anwendung in einem Docker Swarm-Cluster mithilfe von Visual Studio Team Services. Die folgende Abbildung veranschaulicht diese Continuous Delivery-Pipeline:

![Beispielanwendung „MyShop“](./media/container-service-docker-swarm-setup-ci-cd/full-ci-cd-pipeline.png)

Es folgt eine kurze Erläuterung der Schritte:

1. Für Änderungen am Code erfolgt im Quellcoderepository (in diesem Fall GitHub) ein Commit. 
2. GitHub löst einen Build in Visual Studio Team Services aus. 
3. Visual Studio Team Services ruft die neueste Version des Quellcodes ab und erstellt alle Images, aus denen sich die Anwendung zusammensetzt. 
4. Visual Studio Team Services überträgt jedes Image per Push in eine Docker-Registrierung, die mithilfe des Azure-Containerregistrierungsdiensts erstellt wurde. 
5. Visual Studio Team Services löst ein neues Release aus. 
6. Das Release wendet einige Befehle über SSH auf den Masterknoten des Azure Container Service-Clusters an. 
7. Docker Swarm für den Cluster ruft die neueste Version der Images per Pull ab. 
8. Die neue Version der Anwendung wird mithilfe von Docker Compose bereitgestellt. 

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit diesem Tutorial beginnen, müssen Sie die folgenden Aufgaben ausführen:

- [Erstellen eines Swarm-Clusters in Azure Container Service](container-service-deployment.md)
- [Herstellen einer Verbindung mit dem Swarm-Cluster in Azure Container Service](container-service-connect.md)
- [Erstellen einer Azure-Containerregistrierung](../container-registry/container-registry-get-started-portal.md)
- [Erstellen eines Visual Studio Team Services-Kontos (sofern noch nicht vorhanden) und des Teamprojekts](https://www.visualstudio.com/en-us/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)
- [Verzweigen des GitHub-Repositorys zu Ihrem GitHub-Konto](https://github.com/jcorioland/MyShop/)

[!INCLUDE [container-service-swarm-mode-note](../../includes/container-service-swarm-mode-note.md)]

Darüber hinaus benötigen Sie einen Computer mit Ubuntu (14.04 oder 16.04), auf dem Docker installiert ist. Dieser Computer wird von Visual Studio Team Services während der Build- und Releaseprozesse verwendet. Eine Möglichkeit zum Erstellen dieses Computers ist die Verwendung des in [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/canonicalandmsopentech/dockeronubuntuserver1404lts/) verfügbaren Images. 

## <a name="step-1-configure-your-visual-studio-team-services-account"></a>Schritt 1: Konfigurieren Ihres Visual Studio Team Services-Kontos 

In diesem Abschnitt konfigurieren Sie Ihr Visual Studio Team Services-Konto.

### <a name="configure-a-visual-studio-team-services-linux-build-agent"></a>Konfigurieren eines Linux-Build-Agents für Visual Studio Team Services

Um Docker-Images zu erstellen und diese Images per Push aus einem Visual Studio Team Services-Build in eine Azure-Containerregistrierung zu übertragen, müssen Sie einen Linux-Agent registrieren. Sie haben die folgenden Installationsoptionen:

* [Bereitstellen eines Agents unter Linux](https://www.visualstudio.com/docs/build/admin/agents/v2-linux)

* [Verwenden von Docker zum Ausführen des VSTS-Agents](https://hub.docker.com/r/microsoft/vsts-agent)

### <a name="install-the-docker-integration-vsts-extension"></a>Installieren der VSTS-Erweiterung für die Docker-Integration

Microsoft bietet eine VSTS-Erweiterung zum Arbeiten mit den Build- und Releaseprozessen von Docker. Diese Erweiterung steht im [VSTS Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-vscs-rm.docker) zur Verfügung. Klicken Sie auf **Installieren**, um diese Erweiterung Ihrem VSTS-Konto hinzuzufügen:

![Installieren der Docker-Integration](./media/container-service-docker-swarm-setup-ci-cd/install-docker-vsts.png)

Sie werden aufgefordert, mit Ihren Anmeldeinformationen eine Verbindung mit Ihrem VSTS-Konto herzustellen. 

### <a name="connect-visual-studio-team-services-and-github"></a>Verbinden von Visual Studio Team Services mit GitHub

Richten Sie eine Verbindung zwischen Ihrem VSTS-Projekt und Ihrem GitHub-Konto ein.

1. Klicken Sie in Ihrem Visual Studio Team Services-Projekt auf der Symbolleiste auf das Symbol **Einstellungen**, und wählen Sie **Dienste** aus.

    ![Visual Studio Team Services: Externe Verbindung](./media/container-service-docker-swarm-setup-ci-cd/vsts-services-menu.png)

2. Klicken Sie auf der linken Seite auf **Neuer Dienstendpunkt** > **GitHub**.

    ![Visual Studio Team Services: GitHub](./media/container-service-docker-swarm-setup-ci-cd/vsts-github.png)

3. Um VSTS für das Arbeiten mit Ihrem GitHub-Konto zu autorisieren, klicken Sie auf **Autorisieren** und befolgen die Anweisungen im eingeblendeten Fenster.

    ![Visual Studio Team Services: Autorisieren von GitHub](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-authorize.png)

### <a name="connect-vsts-to-your-azure-container-registry-and-azure-container-service-cluster"></a>Verbinden von VSTS mit Ihrer Azure-Containerregistrierung und dem Azure-Containerdienstcluster

Die letzten Schritte vor dem Starten mit der CI/CD-Pipeline bestehen aus dem Konfigurieren externer Verbindungen mit Ihrer Containerregistrierung und Ihrem Docker Swarm-Cluster in Azure. 

1. Fügen Sie in den Einstellungen für **Dienste** Ihres Visual Studio Team Services-Projekts einen Dienstendpunkt des Typs **Docker-Registrierung** hinzu. 

2. Geben Sie im sich öffnenden Popupfenster die URL und Anmeldeinformationen Ihrer Azure-Containerregistrierung ein.

    ![Visual Studio Team Services: Docker-Registrierung](./media/container-service-docker-swarm-setup-ci-cd/vsts-registry.png)

3. Fügen Sie für den Docker Swarm-Cluster einen Endpunkt des Typs **SSH** hinzu. Geben Sie dann die SSH-Verbindungsinformationen Ihres Swarm-Clusters ein.

    ![Visual Studio Team Services: SSH](./media/container-service-docker-swarm-setup-ci-cd/vsts-ssh.png)

Die Konfiguration ist damit abgeschlossen. Im nächsten Schritt erstellen Sie die CI-CD-Pipeline, mit der die Anwendung erstellt und im Docker Swarm-Cluster bereitgestellt wird. 

## <a name="step-2-create-the-build-definition"></a>Schritt 2: Erstellen der Builddefinition

In diesem Schritt richten Sie eine Builddefinition für Ihr VSTS-Projekt ein und definieren den Buildworkflow für Ihre Containerimages.

### <a name="initial-definition-setup"></a>Anfängliche Einrichtung der Definition

1. Stellen Sie zum Erstellen einer Builddefinition eine Verbindung mit Ihrem Visual Studio Team Services-Projekt her, und klicken Sie auf **Build und Release**. 

2. Klicken Sie im Abschnitt **Builddefinitionen** auf **+ Neu**. Wählen Sie die Vorlage **Leer** aus.

    ![Visual Studio Team Services: Neue Builddefinition](./media/container-service-docker-swarm-setup-ci-cd/create-build-vsts.png)

3. Konfigurieren Sie den neuen Build mit einer GitHub-Repositoryquelle, kreuzen Sie **Continuous Integration** an, und wählen Sie die Agent-Warteschlange aus, in der Sie Ihren Linux-Agent registriert haben. Klicken Sie zum Erstellen der Builddefinition auf **Erstellen**.

    ![Visual Studio Team Services: Erstellen der Builddefinition](./media/container-service-docker-swarm-setup-ci-cd/vsts-create-build-github.png)

4. Öffnen Sie auf der Seite **Builddefinitionen** zuerst die Registerkarte **Repository**, und konfigurieren Sie dann den Build für die Verwendung der Verzweigung des „MyShop“-Projekts, die Sie zuvor zum Erfüllen der Voraussetzungen erstellt haben. Wählen Sie *acs-docs* als **Standardbranch** aus.

    ![Visual Studio Team Services: Konfiguration des Buildrepositorys](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-repo-conf.png)

5. Konfigurieren Sie auf der Registerkarte **Trigger** den Build, der nach jedem Commit ausgelöst werden soll. Wählen Sie **Continuous Integration** und **Batchänderungen** aus.

    ![Visual Studio Team Services: Konfiguration des Buildtriggers](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-trigger-conf.png)

### <a name="define-the-build-workflow"></a>Definieren des Buildworkflows
In den nächsten Schritten definieren Sie den Buildworkflow. Für die Anwendung *MyShop* müssen fünf Containerimages erstellt werden. Jedes Image wird mithilfe der Dockerfile erstellt, die sich im Projektordner befindet:

* ProductsApi
* Proxy
* RatingsApi
* RecommandationsApi
* ShopFront

Sie müssen für jedes Image zwei Docker-Schritte hinzufügen: einen zum Erstellen des Image und einen zweiten zum Übertragen des Image per Push in die Azure-Containerregistrierung. 

1. Um dem Buildworkflow einen Schritt hinzuzufügen, klicken Sie auf **+ Buildschritt hinzufügen** und wählen dann **Docker** aus.

    ![Visual Studio Team Services: Hinzufügen von Buildschritten](./media/container-service-docker-swarm-setup-ci-cd/vsts-build-add-task.png)

2. Konfigurieren Sie für jedes Image einen Schritt, der den Befehl `docker build` verwendet.

    ![Visual Studio Team Services: Docker-Build](./media/container-service-docker-swarm-setup-ci-cd/vsts-docker-build.png)

    Wählen Sie für den Buildvorgang Ihre Azure-Containerregistrierung, die Aktion **Image erstellen** und die Dockerfile aus, die jedes Image definiert. Legen Sie **Buildkontext** auf das Dockerfile-Stammverzeichnis fest, und definieren Sie den **Imagenamen**. 
    
    Wie auf dem vorherigen Bildschirm gezeigt, setzen Sie den URI Ihrer Azure-Containerregistrierung an den Anfang des Imagenamens. (Sie können auch eine Buildvariable verwenden, um das Tag des Images zu parametrisieren, wie z.B. die Build-ID in diesem Beispiel.)

3. Konfigurieren Sie für jedes Image einen zweiten Schritt, der den Befehl `docker push` verwendet.

    ![Visual Studio Team Services: Docker-Pushvorgang](./media/container-service-docker-swarm-setup-ci-cd/vsts-docker-push.png)

    Wählen Sie für den Pushvorgang die Azure-Containerregistrierung und die Aktion zum **Übertragen eines Image per Push** aus. Geben Sie dann den **Imagenamen** ein, der im vorherigen Schritt erstellt wurde.

4. Nachdem Sie die Build- und Pushschritte für jedes der fünf Images erstellt haben, fügen Sie dem Buildworkflow zwei weitere Schritte hinzu.

    a. Eine Befehlszeilenaufgabe, die ein Bash-Skript verwendet, um *BuildNumber* in der Datei „docker-compose.yml“ durch die aktuelle Build-ID zu ersetzen. Die folgenden Abbildungen zeigen Details.

    ![Visual Studio Team Services: Aktualisieren der Compose-Datei](./media/container-service-docker-swarm-setup-ci-cd/vsts-build-replace-build-number.png)

    b. Eine Aufgabe, die die aktualisierte Compose-Datei als Buildartefakt ablegt, damit sie im Release verwendet werden kann. Die folgenden Abbildungen zeigen Details.

    ![Visual Studio Team Services: Veröffentlichen der Compose-Datei](./media/container-service-docker-swarm-setup-ci-cd/vsts-publish-compose.png) 

5. Klicken Sie auf **Speichern**, und benennen Sie die Builddefinition.

## <a name="step-3-create-the-release-definition"></a>Schritt 3: Speichern der Releasedefinition

Mit Visual Studio Team Services können Sie [Releases umgebungsübergreifend verwalten](https://www.visualstudio.com/team-services/release-management/). Sie können Continuous Deployment aktivieren, um sicherzustellen, dass Ihre Anwendung reibungslos in Ihren verschiedenen Umgebungen bereitgestellt wird (z.B. Entwicklungs-, Test-, Präproduktions- und Produktionsumgebung). Sie können eine neue Umgebung erstellen, die Ihren Azure Container Service Docker Swarm-Cluster darstellt.

![Visual Studio Team Services: Freigabe in Azure Container Service](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-acs.png) 

### <a name="initial-release-setup"></a>Einrichtung des ersten Release

1. Um eine Releasedefinition zu erstellen, klicken Sie auf **Releases** > **+ Freigeben**

2. Um die Quelle des Artefakts zu konfigurieren, klicken Sie auf **Artefakte** > **Artefaktquelle verknüpfen**. Verknüpfen Sie diese neue Releasedefinition mit dem Build, den Sie im vorherigen Schritt definiert haben. Dadurch steht die Datei „docker-compose.yml“ im Releaseprozess zur Verfügung.

    ![Visual Studio Team Services: Releaseartefakte](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-artefacts.png) 

3. Um den Releasetrigger zu konfigurieren, klicken Sie auf **Trigger**, und wählen Sie **Continuous Deployment**. Legen Sie den Trigger für dieselbe Artefaktquelle fest. Diese Einstellung stellt sicher, dass ein neues Release gestartet wird, sobald der Build erfolgreich abgeschlossen wurde.

    ![Visual Studio Team Services: Releasetrigger](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-trigger.png) 

### <a name="define-the-release-workflow"></a>Definieren des Releaseworkflows

Der Releaseworkflow besteht aus zwei Aufgaben, die Sie hinzufügen.

1. Konfigurieren Sie eine Aufgabe, um eine Compose-Datei sicher in den Ordner *deploy* auf dem Docker Swarm-Masterknoten mithilfe der zuvor konfigurierten SSH-Verbindung zu kopieren. Die folgenden Abbildungen zeigen Details.

    ![Visual Studio Team Services: Release mit SSH](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-scp.png)

2. Konfigurieren Sie eine zweite Aufgabe zum Ausführen eines Bash-Befehls zum Anwenden der Befehle `docker` und `docker-compose` auf den Masterknoten. Die folgenden Abbildungen zeigen Details.

    ![Visual Studio Team Services: Release mit Bash](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-bash.png)

    Der auf den Masterknoten angewendete Befehl verwendet die Docker CLI und Docker-Compose CLI zum Ausführen der folgenden Aufgaben:

    - Melden Sie sich bei der Azure-Containerregistrierung an (diese verwendet drei Buildvariablen, die auf der Registerkarte **Variablen** definiert sind).
    - Definieren Sie die Variable **DOCKER_HOST** für die Zusammenarbeit mit dem Swarm-Endpunkt (:2375).
    - Navigieren Sie zum Ordner *deploy*, der während der vorherigen sicheren Kopieraufgabe erstellt wurde und die Datei „docker-compose.yml“ enthält. 
    - Führen Sie `docker-compose`-Befehle aus, die die neuen Images per Pull abrufen, die Dienste beenden und entfernen sowie die Container erstellen.

    >[!IMPORTANT]
    > Lassen Sie, wie auf dem vorherigen Bildschirm gezeigt, **Fehler für STDERR** deaktiviert. Dies ist eine wichtige Einstellung, da `docker-compose` in der Standardfehlerausgabe mehrere Diagnosemeldungen ausgibt, z.B. zu beendeten und gelöschten Containern. Wenn Sie das Kontrollkästchen aktivieren, meldet Visual Studio Team Services, dass im Verlauf des Release Fehler aufgetreten sind, obwohl alles wie gewünscht verlaufen ist.
    >
3. Speichern Sie diese neue Releasedefinition.


>[!NOTE]
>Diese Bereitstellung weist eine gewisse Ausfallzeit auf, da die alte Diensten beendet werden und der neue ausgeführt wird. Dies kann durch Ausführen einer Bereitstellung des Typs „Blaugrün“ vermieden werden.
>

## <a name="step-4-test-the-cicd-pipeline"></a>Schritt 4: Testen der CI/CD-Pipeline

Nachdem Sie die Konfiguration abgeschlossen haben, ist es Zeit, diese neue CI/CD-Pipeline zu testen. Die einfachste Möglichkeit zum Testen ist das Aktualisieren des Quellcodes und Ausführen eines Commits für die Änderungen in Ihrem GitHub-Repository. Ein paar Sekunden nach der Übertragung des Codes per Push wird in Visual Studio Team Services ein neuer Build ausgeführt. Nach erfolgreichem Abschluss wird ein neues Release ausgelöst, und die neue Version der Anwendung wird im Azure Container Service-Cluster bereitgestellt.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu CI/CD mit Visual Studio Team Services finden Sie unter in der [Übersicht über den VSTS-Build](https://www.visualstudio.com/docs/build/overview).

