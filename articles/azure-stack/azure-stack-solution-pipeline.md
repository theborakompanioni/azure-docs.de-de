---
title: Bereitstellen Ihrer App in Azure und Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie mehr zum Bereitstellen von Apps in Azure und Azure Stack mit einer hybriden CI/CD-Pipeline.
services: azure-stack
documentationcenter: 
author: HeathL17
manager: byronr
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: helaw
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 8f9234fe1f33625685b66e1d0e0024469f54f95c
ms.openlocfilehash: 2a87753a20375fe06b0de54d93437bbb2cee66ac
ms.contentlocale: de-de
ms.lasthandoff: 09/20/2017

---

# <a name="deploy-apps-to-azure-and-azure-stack"></a>Bereitstellen von Apps in Azure und Azure Stack
Eine hybride Pipeline für [Continuous Integration](https://www.visualstudio.com/learn/what-is-continuous-integration/)/[Continuous Delivery](https://www.visualstudio.com/learn/what-is-continuous-delivery/)(CI/CD) ermöglicht Ihnen das Erstellen, Testen und Bereitstellen Ihrer App in mehreren Clouds.  In diesem Tutorial erstellen Sie eine Beispielumgebung, um zu erfahren, wie eine hybride CI/CD-Pipeline Sie bei Folgendem unterstützen kann:
 
> [!div class="checklist"]
> * Initiieren Sie einen neuen Build, der auf den Codecommits Ihres VSTS-Repositorys (Visual Studio Team Services) basiert.
> * Stellen Sie Ihren neu erstellten Code für Benutzerakzeptanztests automatisch in Azure bereit.
> * Sobald Ihr Code den Test bestanden hat, stellen Sie diesen automatisch in Azure Stack bereit. 


## <a name="prerequisites"></a>Voraussetzungen
Es sind einige Komponenten und etwas Vorbereitungszeit erforderlich, um eine hybride CI/CD-Pipeline zu erstellen.  Wenn Sie bereits über einige dieser Komponenten verfügen, stellen Sie vor Beginn sicher, dass diese die Anforderungen erfüllen.

In diesem Thema wird davon ausgegangen, dass Sie bereits über Kenntnisse zu Azure und Azure Stack verfügen. Wenn Sie mehr darüber erfahren möchten, bevor Sie fortfahren, schauen Sie sich diese Themen an:

- [Einführung in Azure](https://docs.microsoft.com/azure/fundamentals-introduction-to-azure)
- [Azure Stack Key Concepts (Wichtige Begriffe für Azure Stack)](azure-stack-key-features.md)

### <a name="azure"></a>Azure
 - Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.
 - Erstellen Sie eine [Web-App](../app-service/environment/app-service-web-how-to-create-a-web-app-in-an-ase.md), und konfigurieren Sie diese für die [FTP-Veröffentlichung](../app-service/app-service-deploy-ftp.md).  Notieren Sie sich die neue URL der Web-App, da Sie diese später benötigen.


### <a name="azure-stack"></a>Azure Stack
 - [Deploy Azure Stack (Bereitstellen von Azure Stack)](azure-stack-run-powershell-script.md).  Der Installationsvorgang dauert in der Regel ein paar Stunden, also planen Sie entsprechend.
 - Stellen Sie PaaS-Dienste als [App Service](azure-stack-app-service-deploy.md) für Azure Stack bereit.
 - Erstellen Sie eine Web-App, und konfigurieren Sie diese für die [FTP-Veröffentlichung](azure-stack-app-service-enable-ftp.md).  Notieren Sie sich die neue URL der Web-App, da Sie diese später benötigen.  

### <a name="developer-tools"></a>Entwicklertools
 - Erstellen Sie einen [VSTS-Arbeitsbereich](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services).  Der Anmeldevorgang erstellt ein Projekt namens „MyFirstProject“.  
 - [Installieren Sie Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio), und [melden Sie sich bei VSTS an](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services#connect-and-share-code-from-visual-studio)
 - Stellen Sie eine Verbindung zum Projekt her, und [klonen Sie dieses lokal](https://www.visualstudio.com/docs/git/gitquickstart).
 - Erstellen Sie einen [Agent-Pool](https://www.visualstudio.com/docs/build/concepts/agents/pools-queues#creating-agent-pools-and-queues) in VSTS.
 - Installieren Sie Visual Studio, und stellen Sie einen [VSTS-Build-Agent](https://www.visualstudio.com/docs/build/actions/agents/v2-windows) für einen virtuellen Computer in Azure Stack bereit. 
 

## <a name="create-app--push-to-vsts"></a>Erstellen einer App und Übertragen an VSTS mithilfe von Push

### <a name="create-application"></a>Erstellen einer Anwendung
In diesem Abschnitt erstellen Sie eine einfache ASP.NET-Anwendung und übertragen diese mithilfe von Push an VSTS.  Diese Schritte stellen den normalen Entwicklerworkflow dar und können an Entwicklertools und -sprachen angepasst werden. 

1.  Öffnen Sie Visual Studio.
2.  Klicken Sie im Bereich **Projektmappen...** von Team Explorer auf **Neu**.
3.  Wählen Sie **Visual C#** > **Web** > **ASP.NET-Webanwendung (.NET Framework)** aus.
4.  Stellen Sie einen Namen für die Anwendung bereit, und klicken Sie auf **OK**.
5.  Behalten Sie auf dem nächsten Bildschirm die Standardwerte (Webformulare) bei, und klicken Sie auf **OK**.

### <a name="commit-and-push-changes-to-vsts"></a>Änderungen mithilfe von Commit und Push an VSTS übertragen
1.  Wählen Sie die Dropdownliste in Team Explorer in Visual Studio aus, und klicken Sie auf **Änderungen**.
2.  Geben Sie eine Commitnachricht ein, und klicken Sie auf **Commit für alle**. Möglicherweise werden Sie dazu aufgefordert, die Projektmappendatei zu speichern. Klicken Sie dann auf „Ja“, um alle zu speichern.
3.  Nachdem der Commit ausgeführt wurde, bietet Visual Studio das Synchronisieren der Änderungen mit Ihrem Projekt an. Klicken Sie auf **Synchronisieren**.

    ![Bild, das den Commit-Bildschirm anzeigt, sobald der Commit abgeschlossen ist](./media/azure-stack-solution-pipeline/image1.png)

4.  Ihr neuer Commit wird Ihnen in der Registerkarte „Synchronisierung“ unter *Ausgehend* angezeigt.  Klicken Sie auf **Push**, um die Änderung mit VSTS zu synchronisieren.

    ![Bild mit Synchronisierungsschritten](./media/azure-stack-solution-pipeline/image2.png)

### <a name="review-code-in-vsts"></a>Überprüfen von Code in VSTS
Sobald Sie eine Änderung mithilfe von Commit und Push an VSTS übertragen haben, überprüfen Sie Ihren Code im VSTS-Portal.  Klicken Sie auf **Code** und dann im Dropdownmenü auf **Dateien**.  Ihnen wird die Projektmappe angezeigt, die Sie erstellt haben.

## <a name="create-build-definition"></a>Erstellen einer Builddefinition
Der Buildprozess definiert durch jeden Commit für Codeänderungen, wie Ihre Anwendung für die Bereitstellung erstellt und verpackt wird. In diesem Beispiel wird die enthaltene Vorlage verwendet, um den Buildprozess für eine ASP.NET-App zu konfigurieren. Diese Konfiguration kann jedoch in Abhängigkeit von Ihrer App angepasst werden.

1.  Melden Sie sich über einen Webbrowser bei Ihrem VSTS-Arbeitsbereich an.
2.  Klicken Sie auf dem Banner auf **Build und Release** und dann auf **Builds**.
3.  Klicken Sie auf **+ New definition** (+ Neue Definition).
4.  Wählen Sie **ASP.NET (Vorschau)** aus der Vorlagenliste aus, und klicken Sie auf **Übernehmen**.
5.  Ändern Sie wie folgt das Feld *MSBuild-Argumente* in *Projektmappe erstellen*:

    `/p:DeployOnBuild=True /p:WebPublishMethod=FileSystem /p:DeployDefaultTarget=WebPublish /p:publishUrl="$(build.artifactstagingdirectory)\\"`

6.  Wählen Sie die Registerkarte **Optionen** aus, und klicken Sie auf die Agent-Warteschlange für den Build-Agent, den Sie für einen virtuellen Computer in Azure Stack bereitgestellt haben. 
7.  Wählen Sie die Registerkarte **Trigger** aus, und aktivieren Sie **Continuous Integration**.
7.  Klicken Sie auf **Speichern und in Warteschlange einreihen**, und wählen Sie dann **Speichern** aus der Dropdownliste aus. 

## <a name="create-release-definition"></a>Erstellen einer Releasedefinition
Der Releaseprozess definiert, wie Builds aus dem vorherigen Schritt für eine Umgebung bereitgestellt werden.  In diesem Tutorial wird die ASP.NET-App mit FTP in einer Azure-Web-App veröffentlicht. Befolgen Sie folgende Schritte, um ein Release in Azure zu konfigurieren:

1.  Klicken Sie auf dem VSTS-Banner auf **Build und Release** und anschließend auf **Releases**.
2.  Klicken Sie auf die grüne Schaltfläche**+ New definition** (+ Neue Definition).
3.  Wählen Sie **Leer** aus, und klicken Sie auf **Weiter**.
4.  Aktivieren Sie das Kontrollkästchen für *Continuous Deployment*, und klicken Sie dann auf **Erstellen**.

Da Sie nun eine leere Releasedefinition erstellt und diese mit dem Build verknüpft haben, werden nun die Schritte für die Azure-Umgebung hinzugefügt:

1.  Klicken Sie auf das grüne Zeichen **+**, um Aufgaben hinzuzufügen.
2.  Wählen Sie **Alle** aus, und fügen Sie dann **FTP-Upload** aus der Liste hinzu. Klicken Sie dann auf **Schließen**.
3.  Wählen Sie die Aufgabe **FTP-Upload** aus, die Sie gerade hinzugefügt haben, und konfigurieren Sie die folgenden Parameter:
    
    | Parameter | Wert |
    | ----- | ----- |
    |Authentifizierungsmethode| Anmeldeinformationen eingeben|
    |Server-URL | FTP-URL der Web-App, die vom Azure-Portal abgerufen wurde |
    |Benutzername | Benutzername, den Sie beim Erstellen der FTP-Anmeldeinformationen für die Web-App konfiguriert haben |
    |Kennwort | Kennwort, das Sie beim Erstellen der FTP-Anmeldeinformationen für die Web-App erstellt haben|
    |Quellverzeichnis | $(System.DefaultWorkingDirectory)\**\ |
    |Remoteverzeichnis | /site/wwwroot/ |
    |Beibehalten von Dateipfaden | Aktiviert (aktiviert)|

4.  Klicken Sie unten auf der Seite auf **Speichern**

Abschließend konfigurieren Sie mithilfe der folgenden Schritte die Releasedefinition, um den Agent-Pool zu verwenden, der den bereitgestellten Agent enthält:
1.  Wählen Sie die Releasedefinition aus, und klicken Sie auf **Bearbeiten**.
2.  Wählen Sie aus der mittleren Spalte **Ausführung mit Agent** aus.  Wählen Sie aus der rechten Spalte die Agent-Warteschlange aus, die den Build-Agent enthält, der auf Azure Stack ausgeführt wird.  
    ![Bild, das die Konfiguration der Releasedefinition zum Verwenden einer bestimmten Warteschlange anzeigt](./media/azure-stack-solution-pipeline/image3.png)


## <a name="deploy-your-app-to-azure"></a>Bereitstellen Ihrer App in Azure
In diesem Schritt wird Ihre neu erstellte CI/CD-Pipeline erstellt, um die ASP.NET-App in Azure bereitzustellen. 

1.  Klicken Sie auf dem Banner in VSTS auf **Build und Release** und dann auf **Builds**.
2.  Klicken Sie in der zuvor erstellten Builddefinition auf **...**, und wählen Sie **Neuen Build in Warteschlange aufnehmen** aus.
3.  Übernehmen Sie die Standardwerte, und klicken Sie auf **OK**.  Der Build beginnt, und der Fortschritt wird angezeigt.
4.  Sobald der Build abgeschlossen ist, können Sie den Status verfolgen, indem Sie auf **Build und Release** und dann auf **Releases** klicken.
5.  Nachdem der Build abgeschlossen ist, besuchen Sie die Website mithilfe der URL, die Sie sich beim Erstellen der Web-App notiert haben.    


## <a name="add-azure-stack-to-pipeline"></a>Hinzufügen von Azure Stack zur Pipeline
Nachdem Sie Ihre CI/CD-Pipeline durch das Bereitstellen in Azure getestet haben, kann Azure Stack nun zur Pipeline hinzugefügt werden.  In den folgenden Schritten erstellen Sie eine neue Umgebung und fügen eine Aufgabe für den FTP-Upload hinzu, um Ihre App in Azure Stack bereitzustellen.  Sie fügen auch eine genehmigende Person für das Release hinzu, die zum Simulieren der „Abmeldung“ einer Codefreigabe in Azure Stack dient.  

1.  Klicken Sie in der Releasedefinition auf **+ Add Environment** (+ Umgebung hinzufügen) und **Neue Umgebung erstellen**.
2.  Wählen Sie **Leer** aus, und klicken Sie auf **Weiter**.
3.  Wählen Sie **Bestimmte Benutzer** aus, und geben Sie Ihr Konto an.  Klicken Sie auf **Erstellen**.
4.  Benennen Sie die Umgebung um, indem Sie den vorhandenen Namen auswählen und *Azure Stack* eingeben.
5.  Wählen Sie nun die Azure Stack-Umgebung aus, und klicken Sie dann auf **Aufgaben hinzufügen**.
6.  Wählen Sie die Aufgabe **FTP-Upload** aus, klicken Sie auf **Hinzufügen** und dann auf **Schließen**.


### <a name="configure-ftp-task"></a>Konfigurieren der FTP-Aufgabe
Da Sie ein Release erstellt haben, können Sie nun die Schritte konfigurieren, die für das Veröffentlichen der Web-App in Azure Stack erforderlich sind.  Konfigurieren Sie die Aufgabe für Azure Stack genau wie die Aufgabe für den FTP-Upload für Azure:

1.  Wählen Sie die Aufgabe **FTP-Upload** aus, die Sie gerade hinzugefügt haben, und konfigurieren Sie die folgenden Parameter:
    
    | Parameter | Wert |
    | -----     | ----- |
    |Authentifizierungsmethode| Anmeldeinformationen eingeben|
    |Server-URL | FTP-URL der Web-App, die vom Azure Stack-Portal abgerufen wurde |
    |Benutzername | Benutzername, den Sie beim Erstellen der FTP-Anmeldeinformationen für die Web-App konfiguriert haben |
    |Kennwort | Kennwort, das Sie beim Erstellen der FTP-Anmeldeinformationen für die Web-App erstellt haben|
    |Quellverzeichnis | $(System.DefaultWorkingDirectory)\**\ |
    |Remoteverzeichnis | /site/wwwroot/|
    |Beibehalten von Dateipfaden | Aktiviert (aktiviert)|

2.  Klicken Sie unten auf der Seite auf **Speichern**

Konfigurieren Sie abschließend mithilfe der folgenden Schritte die Releasedefinition, um den Agent-Pool zu verwenden, der den bereitgestellten Agent enthält:
1.  Wählen Sie die Releasedefinition aus, und klicken Sie auf **Bearbeiten**.
2.  Wählen Sie aus der mittleren Spalte **Ausführung mit Agent** aus. Wählen Sie aus der rechten Spalte die Agent-Warteschlange aus, die den Build-Agent enthält, der auf Azure Stack ausgeführt wird.  
    ![Bild, das die Konfiguration der Releasedefinition zum Verwenden einer bestimmten Warteschlange anzeigt](./media/azure-stack-solution-pipeline/image3.png)

## <a name="deploy-new-code"></a>Neuen Code bereitstellen
Sie können die hybride CI/CD-Pipeline nun im letzten Schritt, der Veröffentlichung in Azure Stack, testen.  In diesem Abschnitt ändern Sie die Fußzeile der Website und beginnen mit der Bereitstellung über die Pipeline.  Sobald der Vorgang abgeschlossen ist, werden Ihnen die in Azure bereitgestellten Änderungen zur Überprüfung angezeigt. Sobald Sie das Release genehmigt haben, werden diese in Azure Stack veröffentlicht.

1. Öffnen Sie in Visual Studio die Datei *site.master*, und ändern Sie diese Zeile:
    
    `
        <p>&copy; <%: DateTime.Now.Year %> - My ASP.NET Application</p>
    `

    Und zwar in diesen Code:

    `
        <p>&copy; <%: DateTime.Now.Year %> - My ASP.NET Application delivered by VSTS, Azure, and Azure Stack</p>
    `
3.  Führen Sie einen Commit für die Änderungen aus, und synchronisieren Sie diese mit VSTS.  
4.  Überprüfen Sie im VSTS-Arbeitsbereich den Buildstatus, indem Sie auf **Build und Release** > **Build** klicken.
5.  Ihnen wird der Buildfortschritt angezeigt.  Doppelklicken Sie auf den Status, um den Buildfortschritt zu beobachten.  Sobald in der Konsole „Finished Build“ (Build abgeschlossen) angezeigt wird, überprüfen Sie das Release über **Build und Release** > **Release**.  Doppelklicken Sie auf das Release.
6.  Sie erhalten eine Benachrichtigung, dass für das Release eine Überprüfung erforderlich ist. Überprüfen Sie die URL der Web-App, und stellen Sie sicher, dass die neuen Änderungen vorhanden sind.  Genehmigen Sie das Release in VSTS.
    ![Bild, das die Konfiguration der Releasedefinition zum Verwenden einer bestimmten Warteschlange anzeigt](./media/azure-stack-solution-pipeline/image4.png)
    
7.  Überprüfen Sie, dass die Veröffentlichung in Azure Stack abgeschlossen ist, indem Sie die Website mithilfe der URL besuchen, die Sie sich beim Erstellen der Web-App notiert haben.
    ![Bild, das die ASP.NET-App mit geänderter Fußzeile zeigt](./media/azure-stack-solution-pipeline/image5.png)


Sie können Ihre neue, hybride CI/CD-Pipeline nun als Baustein für andere hybride Cloudmuster verwenden.

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie das Erstellen einer hybriden CI/CD-Pipeline gelernt, die Folgendes bewirkt:

> [!div class="checklist"]
> * Initiieren eines neuen Builds, der auf den Codecommits an Ihr VSTS-Repository (Visual Studio Team Services) basiert.
> * Automatisches Bereitstellen Ihres neu erstellten Codes für Benutzerakzeptanztests in Azure.
> * Automatisches Bereitstellen in Azure Stack, sobald Ihr Code den Test bestanden hat. 

Da Sie nun über eine hybride CI/CD-Pipeline verfügen, fahren Sie damit fort, das Entwickeln von Apps für Azure Stack zu lernen.

> [!div class="nextstepaction"]
> [Entwickeln für Azure Stack](azure-stack-developer.md)



