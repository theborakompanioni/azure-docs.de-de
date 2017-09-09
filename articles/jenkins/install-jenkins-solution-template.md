---
title: Erstellen eines Jenkins-Servers in Azure
description: "Installieren Sie Jenkins auf der Grundlage der Jenkins-Lösungsvorlage auf einem virtuellen Azure-Linux-Computer, und erstellen Sie eine Java-Beispielanwendung."
author: mlearned
manager: douge
ms.service: multiple
ms.workload: web
ms.devlang: java
ms.topic: hero-article
ms.date: 08/21/2017
ms.author: mlearned
ms.custom: Jenkins
ms.translationtype: HT
ms.sourcegitcommit: 7456da29aa07372156f2b9c08ab83626dab7cc45
ms.openlocfilehash: 7bb74f297d52fb25171817175cce64187b397c38
ms.contentlocale: de-de
ms.lasthandoff: 08/28/2017

---

# <a name="create-a-jenkins-server-on-an-azure-linux-vm-from-the-azure-portal"></a>Erstellen eines Jenkins-Servers auf einem virtuellen Azure-Linux-Computer über das Azure-Portal

In dieser Schnellstartanleitung erfahren Sie, wie Sie [Jenkins](https://jenkins.io) auf einem virtuellen Computer unter Ubuntu Linux mit den Tools und Plug-Ins installieren, die für die Arbeit mit Azure konfiguriert sind. Dadurch erhalten Sie einen in Azure ausgeführten Jenkins-Server, der eine Java-Beispielanwendung aus [GitHub](https://github.com) erstellt.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement
* Zugriff auf SSH über die Befehlszeile Ihres Computers (beispielsweise Bash-Shell oder [PuTTY](http://www.putty.org/))

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-the-jenkins-vm-from-the-solution-template"></a>Erstellen des virtuellen Jenkins-Computers auf der Grundlage der Lösungsvorlage

Öffnen Sie in Ihrem Webbrowser das [Marketplace-Image für Jenkins](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.jenkins?tab=Overview), und wählen links auf der Seite **Jetzt anfordern** aus. Sehen Sie sich die Preisdetails an, und wählen Sie **Weiter** aus. Wählen Sie anschließend **Erstellen** aus, um den Jenkins-Server über das Azure-Portal zu konfigurieren. 
   
![Azure-Portal, Dialogfeld](./media/install-jenkins-solution-template/ap-create.png)

Füllen Sie auf der Registerkarte **Grundeinstellungen konfigurieren** die folgenden Felder aus:

![Grundeinstellungen konfigurieren](./media/install-jenkins-solution-template/ap-basic.png)

* Geben Sie unter **Name** den Namen **Jenkins** ein.
* Geben Sie einen Benutzernamen**** ein. Der Benutzername muss [bestimmte Anforderungen](/azure/virtual-machines/linux/faq#what-are-the-username-requirements-when-creating-a-vm) erfüllen.
* Wählen unter **Authentifizierungstyp** die Option **Kennwort** aus, und geben Sie ein Kennwort ein. Das Kennwort muss einen Großbuchstaben, eine Zahl und ein Sonderzeichen enthalten.
* Geben Sie unter **Ressourcengruppe** die Zeichenfolge **myJenkinsResourceGroup** ein.
* Wählen Sie in der Dropdownliste **Standort** die [Azure-Region](https://azure.microsoft.com/regions/) **USA, Osten** aus.

Wählen Sie **OK** aus, um zur Registerkarte **Weitere Optionen konfigurieren** zu gelangen. Geben Sie einen eindeutigen Domänennamen zur Identifizierung des Jenkins-Servers ein, und wählen Sie **OK** aus.

![Einrichten weiterer Optionen](./media/install-jenkins-solution-template/ap-addtional.png)  

 Wählen Sie nach erfolgreicher Überprüfung auf der Registerkarte **Zusammenfassung** erneut **OK** aus. Wählen Sie abschließend **Kaufen** aus, um den virtuellen Jenkins-Computer zu erstellen. Wenn Ihr Server bereit ist, erhalten Sie im Azure-Portal eine entsprechende Benachrichtigung:   

![Bereitschaftsbenachrichtigung für Jenkins](./media/install-jenkins-solution-template/jenkins-deploy-notification-ready.png)

## <a name="connect-to-jenkins"></a>Herstellen einer Verbindung mit Jenkins

Navigieren Sie in Ihrem Webbrowser zu Ihrem virtuellen Computer (Beispiel: http://jenkins2517454.eastus.cloudapp.azure.com/). Auf die Jenkins-Konsole kann nicht über eine unsichere HTTP-Verbindung zugegriffen werden. Daher ist auf der Seite beschrieben, wie Sie von Ihrem Computer aus über einen SSH-Tunnel sicher auf die Jenkins-Konsole zugreifen können.

![Entsperren von Jenkins](./media/install-jenkins-solution-template/jenkins-ssh-instructions.png)

Richten Sie den Tunnel ein, indem Sie den auf der Seite angegebenen Befehl `ssh` über die Befehlszeile ausführen. Ersetzen Sie dabei `username` durch den Namen des VM-Administratorbenutzers, den Sie beim Einrichten des virtuellen Computers auf der Grundlage der Lösungsvorlage gewählt haben.

```bash
ssh -L 127.0.0.1:8080:localhost:8080 jenkinsadmin@jenkins2517454.eastus.cloudapp.azure.com
```

Navigieren Sie nach dem Starten des Tunnels auf Ihrem lokalen Computer zu http://localhost:8080/. 

Führen Sie über die Befehlszeile den folgenden Befehl aus, während eine SSH-Verbindung mit dem virtuellen Jenkins-Computer besteht, um das anfängliche Kennwort abzurufen.

```bash
`sudo cat /var/lib/jenkins/secrets/initialAdminPassword`.
```

Entsperren Sie das Jenkins-Dashboard zunächst mit diesem anfänglichen Kennwort.

![Entsperren von Jenkins](./media/install-jenkins-solution-template/jenkins-unlock.png)

Wählen Sie auf der nächsten Seite **Install suggested plugins** (Vorgeschlagene Plug-Ins installieren) aus, und erstellen Sie einen Jenkins-Administratorbenutzer für den Zugriff auf das Jenkins-Dashboard.

![Jenkins ist bereit!](./media/install-jenkins-solution-template/jenkins-welcome.png)

Der Jenkins-Server ist nun für die Codeerstellung bereit.

## <a name="create-your-first-job"></a>Erstellen Ihres ersten Auftrags

Wählen Sie in der Jenkins-Konsole die Option **Create new jobs** (Neue Aufträge erstellen) aus, und nennen Sie den Auftrag **mySampleApp**. Wählen Sie dann **Freestyle project** (Freestyle-Projekt) und anschließend **OK** aus.

![Erstellen eines neuen Auftrags](./media/install-jenkins-solution-template/jenkins-new-job.png) 

Aktivieren Sie auf der Registerkarte **Source Code Management** (Quellcodeverwaltung) die Option **Git**, und geben Sie im Feld **Repository URL** (Repository-URL) die folgende URL ein: `https://github.com/spring-guides/gs-spring-boot.git`

![Definieren des Git-Repositorys](./media/install-jenkins-solution-template/jenkins-job-git-configuration.png) 

Wählen Sie auf der Registerkarte **Build** die Option **Add build step** (Buildschritt hinzufügen) und anschließend **Invoke Gradle script** (Gradle-Skript aufrufen) aus. Wählen Sie **Use Gradle Wrapper** (Gradle-Wrapper verwenden) aus, und geben Sie dann im Feld **Wrapper location** (Wrapperspeicherort) die Zeichenfolge `complete` und im Feld **Tasks** (Aufgaben) die Zeichenfolge `build` ein.

![Verwenden des Gradle-Wrappers für die Erstellung](./media/install-jenkins-solution-template/jenkins-job-gradle-config.png) 

Wählen Sie **Advanced...** (Erweitert) aus, und geben Sie dann im Feld **Root Build script** (Stammerstellungsskript) die Zeichenfolge `complete` ein. Wählen Sie **Speichern** aus.

![Festlegen erweiterter Einstellungen im Buildschritt des Gradle-Wrappers](./media/install-jenkins-solution-template/jenkins-job-gradle-advances.png) 

## <a name="build-the-code"></a>Erstellen des Codes

Wählen Sie **Build Now** (Jetzt erstellen) aus, um den Code zu kompilieren und das Paket für die Beispiel-App zu erstellen. Wählen Sie nach Abschluss des Buildvorgangs den Link **Workspace** (Arbeitsbereich) für das Projekt aus.

![Navigieren zum Arbeitsbereich, um die JAR-Datei aus dem Build abzurufen](./media/install-jenkins-solution-template/jenkins-access-workspace.png) 

Vergewissern Sie sich unter `complete/build/libs`, dass die Datei `gs-spring-boot-0.1.0.jar` vorhanden ist, um sicherzugehen, dass der Buildvorgang erfolgreich war. Der Jenkins-Server ist nun für die Erstellung Ihrer eigenen Projekte in Azure bereit.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Verwenden Sie Azure-VM-Agents für die Continuous Integration mit Jenkins.](jenkins-azure-vm-agents.md)

