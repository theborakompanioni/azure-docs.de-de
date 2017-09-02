---
title: CI/CD von Jenkins auf virtuellen Azure-Computern mit Team Services | Microsoft-Dokumentation
description: "Einrichten von Continuous Integration (CI) und Continuous Deployment (CD) einer Node.js-App mithilfe von Jenkins auf virtuellen Azure-Computern über Release Management in Visual Studio Team Services (VSTS) oder Microsoft Team Foundation Server (TFS)"
author: ahomer
manager: douge
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/15/2017
ms.author: ahomer
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: a40e26a8681df31fad664e4d1df4c1513311900d
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---

# <a name="deploy-your-app-to-linux-vms-using-jenkins-and-team-services"></a>Bereitstellen Ihrer App auf virtuellen Linux-Computern mithilfe von Jenkins und Team Services

Continuous Integration (CI) und Continuous Deployment (CD) stellen eine Pipeline dar, über die Sie Ihren Code erstellen, freigeben und bereitstellen können. Team Services umfasst eine vollständige Gruppe von CI/CD-Automatisierungstools mit vollem Funktionsumfang für die Bereitstellung in Azure. Jenkins ist ein gängiges serverbasiertes CI/CD-Drittanbietertool, das auch CI/CD-Automatisierung beinhaltet. Sie können beide Tools zusammen verwenden, um die Bereitstellung Ihrer Cloud-Apps oder Clouddienste anzupassen.

In diesem Tutorial verwenden Sie Jenkins zum Erstellen einer **Node.js-Web-App** und Visual Studio Team Services zum Bereitstellen dieser App in einer [Bereitstellungsgruppe](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/), die virtuelle Linux-Computer enthält.

In diesem Tutorial führen Sie folgende Schritte aus:

> [!div class="checklist"]
> * Erstellen der App in Jenkins
> * Konfigurieren von Jenkins für die Team Services-Integration
> * Erstellen einer Bereitstellungsgruppe für die virtuellen Azure-Computer
> * Erstellen einer Freigabedefinition, über die die virtuellen Computer konfiguriert werden und die App bereitgestellt wird

## <a name="before-you-begin"></a>Voraussetzungen

* Sie benötigen Zugriff auf ein Jenkins-Konto. Wenn Sie noch keinen Jenkins-Server erstellt haben, finden Sie entsprechende Informationen in der [Jenkins-Dokumentation](https://jenkins.io/doc/). 

* Melden Sie sich bei Ihrem Team Services-Konto (`https://{youraccount}.visualstudio.com`) an. 
  Sie können ein [kostenloses Team Services-Konto](https://go.microsoft.com/fwlink/?LinkId=307137&clcid=0x409&wt.mc_id=o~msft~vscom~home-vsts-hero~27308&campaign=o~msft~vscom~home-vsts-hero~27308) erstellen.

  > [!NOTE]
  > Weitere Informationen finden Sie unter [Connect to Team Services](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services) (Herstellen einer Verbindung mit Team Services).

* Erstellen Sie ein persönliches Zugriffstoken (PAT) in Ihrem Team Services-Konto, sofern dies noch nicht erfolgt ist. In Jenkins sind diese Informationen für den Zugriff auf Ihr Team Services-Konto erforderlich.
  Informationen zum Generieren eines Tokens finden Sie unter [How do I create a personal access token for Team Services and TFS](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) (Wie erstelle ich ein persönliches Zugriffstoken für Team Services und TFS?).

## <a name="get-the-sample-app"></a>Abrufen der Beispiel-App

Für die Bereitstellung benötigen Sie eine in einem Git-Repository gespeicherte App.
Für dieses Tutorial wird [diese über GitHub verfügbare Beispiel-App](https://github.com/azooinmyluggage/fabrikam-node) empfohlen.

1. Erstellen Sie einen Fork dieser App, und notieren Sie den Speicherort (URL) zur Verwendung in nachfolgenden Schritten in diesem Tutorial.

1. Machen Sie den Fork **öffentlich**, um die spätere Verbindung mit GitHub zu vereinfachen.

> [!NOTE]
> Weitere Informationen finden Sie unter [Fork a repo](https://help.github.com/articles/fork-a-repo/) (Forken eines Repositorys) und [Making a private repository public](https://help.github.com/articles/making-a-private-repository-public/) (Öffentlichmachen eines privaten Repositorys).

> [!NOTE]
> Die App wurde mit [Yeoman](http://yeoman.io/learning/index.html) erstellt, sie verwendet **Express**, **Bower** und **Grunt**, und sie verfügt über einige **npm**-Pakete als Abhängigkeiten.
> Die Beispiel-App enthält eine Gruppe von [Azure Resource Manager-Vorlagen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment), mit denen die virtuellen Computer für die Bereitstellung in Azure dynamisch erstellt werden. Diese Vorlagen werden durch Aufgaben in der [Team Services-Freigabedefinition](https://www.visualstudio.com/docs/build/actions/work-with-release-definitions) verwendet.
> Die Hauptvorlage erstellt eine Netzwerksicherheitsgruppe, einen virtuellen Computer und ein virtuelles Netzwerk. Sie weist eine öffentliche IP-Adresse zu und öffnet den eingehenden Port 80. Sie fügt auch ein Tag hinzu, das von der Bereitstellungsgruppe zum Auswählen der Computer für die Bereitstellung verwendet wird.
>
> Die Beispiel-App enthält zudem ein Skript, über das Nginx eingerichtet und die App bereitgestellt wird. Es wird auf den einzelnen virtuellen Computern ausgeführt. Das Skript installiert Node, Nginx und PM2, konfiguriert Nginx und PM2 und startet dann die Node-App.

## <a name="configure-jenkins-plugins"></a>Konfigurieren von Jenkins-Plug-Ins

Zunächst müssen Sie zwei Jenkins-Plug-Ins für **NodeJS** und die **Integration in Team Services** konfigurieren.

1. Öffnen Sie Ihr Jenkins-Konto, und wählen Sie **Manage Jenkins** (Jenkins verwalten) aus.

1. Wählen Sie auf der Seite **Manage Jenkins** (Jenkins verwalten) die Option **Manage Plugins** (Plug-Ins verwalten) aus.

1. Filtern Sie die Liste, um das **NodeJS**-Plug-In zu suchen, und installieren Sie es ohne Neustart.

   ![Hinzufügen des NodeJS-Plug-Ins zu Jenkins](media/tutorial-build-deploy-jenkins/jenkins-nodejs-plugin.png)

1. Filtern Sie die Liste, um das **Team Foundation Server**-Plug-In zu suchen, und installieren Sie es. (Dieses Plug-In funktioniert bei Team Services und Team Foundation Server.) Jenkins muss nicht neu gestartet werden.

## <a name="configure-jenkins-build-for-nodejs"></a>Konfigurieren des Jenkins-Builds für Node.js

Erstellen Sie in Jenkins ein neues Buildprojekt, und konfigurieren Sie es wie folgt:

1. Geben Sie auf der Registerkarte **General** (Allgemein) einen Namen für das Buildprojekt ein.

1. Wählen Sie auf der Registerkarte **Source Code Management** (Quellcodeverwaltung) die Option **Git** aus, und geben Sie die Details des Repositorys sowie den Branch ein, der Ihren App-Code enthält.

   ![Hinzufügen eines Repositorys zum Build](media/tutorial-build-deploy-jenkins/jenkins-git.png)

   > [!NOTE]
   > Wenn das Repository nicht öffentlich ist, wählen Sie **Add** (Hinzufügen) aus, und geben Sie die Anmeldeinformationen an, um eine Verbindung mit dem Repository herzustellen.

1. Wählen Sie auf der Registerkarte **Build Triggers** (Buildtrigger) die Option **Poll SCM** (SCM abrufen) aus, und geben Sie den Zeitplan `H/03 * * * *` ein, um Änderungen im Git-Repository alle drei Minuten abzurufen. 

1. Wählen Sie auf der Registerkarte **Build Environment** (Buildumgebung) die Option **Provide Node &amp; npm bin/folder PATH** (Knoten angeben & npm bin/PATH-Ordner) aus, und geben Sie `NodeJS` als Wert für die Node JS-Installation ein. Lassen Sie **npmrc file** (NPMRC-Datei) auf „use system default“ (Systemstandard verwenden) festgelegt.

1. Geben Sie auf der Registerkarte **Build** den Befehl `npm install` ein, um sicherzustellen, dass alle Abhängigkeiten aktualisiert werden.

## <a name="configure-jenkins-for-team-services-integration"></a>Konfigurieren von Jenkins für die Team Services-Integration

1. Geben Sie auf der Registerkarte **Post-build Actions** (Postbuildaktionen) für **Files to archive** (Dateien zur Archivierung) den Wert `**/*` ein, um alle Dateien einzuschließen.

1. Geben Sie für **Trigger release in TFS/Team Services** (Freigabe in TFS/Team Services auslösen) die vollständige URL für Ihr Konto (z.B. `https://your-account-name.visualstudio.com`), den Namen des Projekts, einen Namen für die Freigabedefinition (wird später erstellt) und die Anmeldeinformationen für die Verbindung mit Ihrem Konto ein.
   Sie benötigen Ihren Benutzernamen und das zuvor erstellte persönliche Zugriffstoken (PAT). 

   ![Konfigurieren von Jenkins-Postbuildaktionen](media/tutorial-build-deploy-jenkins/trigger-release-from-jenkins.png)

1. Speichern Sie das Buildprojekt.

## <a name="create-a-jenkins-service-endpoint"></a>Erstellen eines Jenkins-Dienstendpunkts

Über einen Dienstendpunkt kann eine Verbindung zwischen Team Services und Jenkins hergestellt werden.

1. Öffnen Sie in Team Services die Seite **Dienste**, öffnen Sie die Liste **Neuer Dienstendpunkt**, und wählen Sie **Jenkins** aus.

   ![Hinzufügen eines Jenkins-Endpunkts](media/tutorial-build-deploy-jenkins/add-jenkins-endpoint.png)

1. Geben Sie einen Namen ein, den Sie zum Verweisen auf diese Verbindung verwenden.

1. Geben Sie die URL Ihres Jenkins-Servers ein, und aktivieren Sie die Option **Nicht vertrauenswürdige SSL-Zertifikate annehmen**.

1. Geben Sie den Benutzernamen und das Kennwort für Ihr Jenkins-Konto ein.

1. Wählen Sie **Verbindung überprüfen** aus, um zu überprüfen, ob die Informationen richtig sind.

1. Wählen Sie **OK** aus, um den Dienstendpunkt zu erstellen.

## <a name="create-a-deployment-group"></a>Erstellen einer Bereitstellungsgruppe

Sie müssen eine [Bereitstellungsgruppe](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) erstellen, die die virtuellen Computer enthält.

1. Öffnen Sie die Registerkarte **Freigaben** auf dem Hub **Build &amp; Release**. Öffnen Sie dann die Registerkarte **Bereitstellungsgruppen**, und wählen Sie **+ Neu** aus.

1. Geben Sie einen Namen und eine optionale Beschreibung für die Bereitstellungsgruppe ein.
   Wählen Sie dann **Erstellen** aus.

Die Aufgabe „Bereitstellung einer Azure-Ressourcengruppe“ erstellt und registriert die virtuellen Computer, wenn sie über die Azure Resource Manager-Vorlage ausgeführt wird.
Sie müssen die virtuellen Computer nicht selbst erstellen und registrieren.

## <a name="create-a-release-definition"></a>Erstellen einer Releasedefinition

Eine Freigabedefinition gibt den Vorgang an, der in Team Services zum Bereitstellen der App durchgeführt wird.
So erstellen Sie die Freigabedefinition in Team Services

1. Öffnen Sie die Registerkarte **Freigaben** auf dem Hub **Build &amp; Release**. Öffnen Sie das Dropdownmenü **+** in der Liste der Freigabedefinitionen, und wählen Sie **Freigabedefinition erstellen** aus. 

1. Wählen Sie die Vorlage **Leer** und dann **Weiter** aus.

1. Klicken Sie im Abschnitt **Artefakte** auf **Artefaktquelle verknüpfen**, und wählen Sie **Jenkins** aus. Wählen Sie Ihre Jenkins-Dienstendpunktverbindung aus. Wählen Sie dann den Jenkins-Quellauftrag und anschließend **Erstellen** aus. 

1. Wählen Sie in der neuen Freigabedefinition die Option **+ Aufgaben hinzufügen** aus, und fügen Sie der Standardumgebung die Aufgabe **Bereitstellung einer Azure-Ressourcengruppe** hinzu.

1. Wählen Sie den Dropdownpfeil neben dem Link **+ Aufgaben hinzufügen** aus, und fügen Sie der Definition eine Bereitstellungsgruppenphase hinzu.

   ![Hinzufügen einer Bereitstellungsgruppenphase](media/tutorial-build-deploy-jenkins/deployment-group-phase-in-release-definition.png) 

1. Öffnen Sie im Aufgabenkatalog den Abschnitt **Hilfsprogramm**, und fügen Sie eine Instanz der Aufgabe **Shellskript** hinzu.

1. Über die in der Aufgabe „Bereitstellung einer Azure-Ressourcengruppe“ verwendete Parametervorlage wird das Administratorkennwort für die Verbindung mit den virtuellen Computern festgelegt.
   Sie geben dieses Kennwort mit der Variable **$(adminpassword)** an:
   
   - Öffnen Sie die Registerkarte **Variablen**, und geben Sie im Abschnitt **Variablen** den Namen `adminpassword` ein.

   - Geben Sie das Administratorkennwort ein.

   - Wählen Sie das Schlosssymbol neben dem Textfeld für den Wert aus, um das Kennwort zu schützen. 

## <a name="configure-the-azure-resource-group-deployment-task"></a>Konfigurieren der Aufgabe „Bereitstellung einer Azure-Ressourcengruppe“

Mit der Aufgabe **Bereitstellung einer Azure-Ressourcengruppe** wird die Bereitstellungsgruppe erstellt. Konfigurieren Sie ihn wie folgt:

* **Azure-Abonnement:** Wählen Sie in der Liste unter **Verfügbare Azure-Dienstverbindungen** eine Verbindung aus. 
  Wenn keine Verbindungen angezeigt werden, wählen Sie **Verwalten**, **Neuer Dienstendpunkt** und dann **Azure Resource Manager** aus, und befolgen Sie die Anweisungen.
  Kehren Sie zu Ihrer Freigabedefinition zurück, aktualisieren Sie die Liste **AzureRM-Abonnement**, und wählen Sie die erstellte Verbindung aus.

* **Ressourcengruppe:** Geben Sie den Namen der zuvor erstellten Ressourcengruppe ein.

* **Speicherort:** Wählen Sie eine Region für die Bereitstellung aus.

  ![Erstellen einer neuen Ressourcengruppe](media/tutorial-build-deploy-jenkins/provision-web-server.png)

* **Speicherort der Vorlage:** `URL of the file`

* **Vorlagenlink:** `{your-git-repo}/ARM-Templates/UbuntuWeb1.json`

* **Vorlagenparameterlink:** `{your-git-repo}/ARM-Templates/UbuntuWeb1.parameters.json`

* **Vorlagenparameter überschreiben:** eine Liste der Außerkraftsetzungswerte, z.B.: `-location {location} -virtualMachineName {machine] -virtualMachineSize Standard_DS1_v2 -adminUsername {username} -virtualNetworkName fabrikam-node-rg-vnet -networkInterfaceName fabrikam-node-websvr1 -networkSecurityGroupName fabrikam-node-websvr1-nsg -adminPassword $(adminpassword) -diagnosticsStorageAccountName fabrikamnodewebsvr1 -diagnosticsStorageAccountId Microsoft.Storage/storageAccounts/fabrikamnodewebsvr1 -diagnosticsStorageAccountType Standard_LRS -addressPrefix 172.16.8.0/24 -subnetName default -subnetPrefix 172.16.8.0/24 -publicIpAddressName fabrikam-node-websvr1-ip -publicIpAddressType Dynamic`.<br />Fügen Sie Ihre eigenen Werte für die {Platzhalter} ein. 

* **Erforderliche Komponenten aktivieren:** `Configure with Deployment Group agent`

* **TFS/VSTS-Endpunkt:** Wählen Sie **Hinzufügen** und im Dialogfeld „Neue Team Foundation Server/Team Services-Verbindung hinzufügen“ die Option **Tokenbasierte Authentifizierung** aus. Geben Sie den Namen der Verbindung und die URL des Teamprojekts ein. Generieren Sie ein [persönliches Zugriffstoken (PAT)]( https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate), und geben Sie es dann ein, um die Verbindung mit Ihrem Teamprojekt zu authentifizieren.

  ![Erstellen eines persönlichen Zugriffstokens](media/tutorial-build-deploy-jenkins/create-a-pat.png)

* **Teamprojekt:** Wählen Sie das aktuelle Projekt aus.

* **Bereitstellungsgruppe:** Geben Sie den Namen der Bereitstellungsgruppe ein, den Sie auch für den Parameter **Ressourcengruppe** verwendet haben.

Die Standardeinstellungen für die Aufgabe „Bereitstellung einer Azure-Ressourcengruppe“ sind das inkrementelle Erstellen oder Aktualisieren einer Ressource. Über die Aufgabe werden bei der ersten Ausführung die virtuellen Computer erstellt und anschließend nur aktualisiert.

## <a name="configure-the-shell-script-task"></a>Konfigurieren der Aufgabe „Shellskript“

Über die Aufgabe **Shellskript** wird die Konfiguration für ein Skript angegeben, das auf jedem Server ausgeführt wird, um Node.js zu installieren und die App zu starten. Konfigurieren Sie ihn wie folgt:

* **Skriptpfad:** `$(System.DefaultWorkingDirectory)/Fabrikam-Node/deployscript.sh`

* **Arbeitsverzeichnis angeben:** `Checked`

* **Arbeitsverzeichnis:** `$(System.DefaultWorkingDirectory)/Fabrikam-Node`
   
## <a name="rename-and-save-the-release-definition"></a>Umbenennen und Speichern der Freigabedefinition

1. Ändern Sie den Namen der Freigabedefinition in den Namen, den Sie auf der Registerkarte **Post-build Actions** (Postbuildaktionen) des Builds in Jenkins angegeben haben. Dieser Name ist in Jenkins erforderlich, damit eine neue Freigabe ausgelöst werden kann, wenn die Quellartefakte aktualisiert werden.

1. Ändern Sie optional den Namen der Umgebung durch Klicken auf den Namen. 

1. Wählen Sie **Speichern** und dann **OK** aus.

## <a name="start-a-manual-deployment"></a>Starten einer manuellen Bereitstellung

1. Wählen Sie **+ Freigabe** und dann **Freigabe erstellen** aus.

1. Wählen Sie den abgeschlossenen Build in der hervorgehobenen Dropdownliste und dann **Erstellen** aus.

1. Wählen Sie den Freigabelink in der Popupmeldung aus. Beispiel: „Freigabe **Freigabe-1** wurde erstellt.“

1. Öffnen Sie die Registerkarte **Protokolle**, um die Konsolenausgabe der Freigabe anzuzeigen.

1. Öffnen Sie im Browser die URL für einen der Server, die Sie der Bereitstellungsgruppe hinzugefügt haben. Geben Sie z.B. `http://{your-server-ip-address}` ein.

## <a name="start-a-cicd-deployment"></a>Starten einer CI/CD-Bereitstellung

1. Deaktivieren Sie in der Freigabedefinition das Kontrollkästchen **Aktiviert** im Abschnitt **Steuerungsoptionen** der Einstellungen für die Aufgabe „Bereitstellung einer Azure-Ressourcengruppe“.
   Für zukünftige Bereitstellungen der vorhandenen Bereitstellungsgruppe müssen Sie diese Aufgabe nicht erneut ausführen.

1. Wechseln Sie zu „source Git repository“ (Git-Quellrepository), und ändern Sie den Inhalt der Überschrift **h1** in der Datei [app/views/index.jade](https://github.com/azooinmyluggage/fabrikam-node/blob/master/app/views/index.jade).

1. Committen Sie die Änderung.

1. Nach einigen Minuten wird auf der Seite **Freigaben** von Team Services oder TFS eine neu erstellte Freigabe angezeigt. Öffnen Sie die Freigabe, um anzuzeigen, wie die Bereitstellung durchgeführt wird. Glückwunsch!

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie die Bereitstellung einer App in Azure mithilfe eines Jenkins-Builds und mit Team Services für die Freigabe automatisiert. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Erstellen der App in Jenkins
> * Konfigurieren von Jenkins für die Team Services-Integration
> * Erstellen einer Bereitstellungsgruppe für die virtuellen Azure-Computer
> * Erstellen einer Freigabedefinition, über die die virtuellen Computer konfiguriert werden und die App bereitgestellt wird

Im nächsten Tutorial erfahren Sie mehr über die Bereitstellung eines LAMP-Stapels (Linux, Apache, MySQL und PHP).

> [!div class="nextstepaction"]
> [Bereitstellen des LAMP-Stapels](tutorial-lamp-stack.md)
