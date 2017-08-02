---
title: Bereitstellen einer Azure Service Fabric-Anwendung mit Continuous Integration (Team Services) | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie mithilfe von Visual Studio Team Services Continuous Integration und Continuous Deployment für eine Service Fabric-Anwendung einrichten.  Stellen Sie eine Anwendung in einem Service Fabric-Cluster in Azure bereit."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/12/2017
ms.author: ryanwi
ms.translationtype: HT
ms.sourcegitcommit: 9afd12380926d4e16b7384ff07d229735ca94aaa
ms.openlocfilehash: c528974951d3b4a83111cb92b931810a91f660f4
ms.contentlocale: de-de
ms.lasthandoff: 07/15/2017

---

# <a name="deploy-an-application-with-cicd-to-a-service-fabric-cluster"></a>Bereitstellen einer Anwendung mit CI/CD in einem Service Fabric-Cluster
In diesem Tutorial wird beschrieben, wie Continuous Integration und Continuous Deployment für eine Azure Service Fabric-Anwendung mithilfe von Visual Studio Team Services eingerichtet werden.  Es ist eine vorhandene Service Fabric-Anwendung erforderlich. Die im Artikel [Erstellen einer .NET-Anwendung](service-fabric-tutorial-create-dotnet-app.md) erstellte Anwendung wird als Beispiel verwendet.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Hinzufügen der Quellcodeverwaltung zum Projekt
> * Erstellen einer Builddefinition in Team Services
> * Erstellen einer Releasedefinition in Team Services
> * Automatisches Bereitstellen und Durchführen von Upgrades einer Anwendung

Das Tutorial ist in drei Artikel aufgeteilt, von denen dieser Artikel der dritte in der Reihe ist.

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Tutorial beginnen können, benötigen Sie Folgendes:
- Wenn Sie kein Azure-Abonnement besitzen, erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Installieren Sie Visual Studio 2017](https://www.visualstudio.com/) und die Workloads **Azure-Entwicklung** und **ASP.NET und Webentwicklung**.
- [Installieren Sie das Service Fabric-SDK](service-fabric-get-started.md).
- Erstellen Sie eine Service Fabric-Anwendung, z.B. durch das [Ausführen der Schritte in diesem Tutorial](service-fabric-tutorial-create-dotnet-app.md). 
- Erstellen Sie einen Windows Service Fabric-Cluster in Azure, z.B. durch das [Ausführen der Schritte in diesem Tutorial](service-fabric-tutorial-create-cluster-azure-ps.md).
- Erstellen Sie ein [Team Services-Konto](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services).

## <a name="prepare-a-publish-profile"></a>Erstellen eines Veröffentlichungsprofils
Nachdem Sie [eine Anwendung erstellt](service-fabric-tutorial-create-dotnet-app.md) und [die Anwendung in Azure bereitgestellt](service-fabric-tutorial-deploy-app-to-party-cluster.md) haben, können Sie Continuous Integration einrichten.  Erstellen Sie zunächst in der Anwendung ein Veröffentlichungsprofil für den Bereitstellungsprozess, der in Team Services ausgeführt wird.  Das Veröffentlichungsprofil muss für den Cluster konfiguriert werden, den Sie zuvor erstellt haben.  Starten Sie Visual Studio, und öffnen Sie ein vorhandenes Service Fabric-Anwendungsprojekt.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die Anwendung, und wählen Sie **Veröffentlichen** aus.

Wählen Sie ein Zielprofil im Anwendungsprojekt aus, das Sie für den Continuous Integration-Workflow verwenden möchten, z.B. „Cloud“.  Geben Sie den Clusterverbindungsendpunkt an.  Aktivieren Sie das Kontrollkästchen **Upgrade der Anwendung ausführen**, damit für jede Bereitstellung in Team Services ein Upgrade der Anwendung erfolgt.  Klicken Sie auf den Link **Speichern**, um die Einstellungen im Veröffentlichungsprofil zu speichern, und klicken Sie dann auf **Abbrechen**, um das Dialogfeld zu schließen.  

![Profil pushen][publish-app-profile]

## <a name="share-your-visual-studio-solution-to-a-new-team-services-git-repo"></a>Freigeben der Visual Studio-Projektmappe in einem neuen Git-Repository von Team Services
Geben Sie die Anwendungsquelldateien in einem Teamprojekt in Team Services frei, damit Sie Builds generieren können.  

Erstellen Sie ein neues lokales Git-Repository für das Projekt, indem Sie auf der Statusleiste in der unteren rechten Ecke von Visual Studio **Zur Quellcodeverwaltung hinzufügen** -> **Git** auswählen. 

Klicken Sie in der Ansicht **Push** in **Team Explorer** unter **Per Push in Visual Studio Team Services übertragen** auf die Schaltfläche **Git-Repository veröffentlichen**.

![Git-Repository pushen][push-git-repo]

Überprüfen Sie Ihre E-Mail-Adresse, und wählen Sie in der Dropdownliste **Team Services-Domäne** Ihr Konto aus. Geben Sie den Repositorynamen ein, und wählen Sie **Repository veröffentlichen** aus.

![Git-Repository pushen][publish-code]

Durch das Veröffentlichen des Repositorys wird in Ihrem Konto ein neues Teamprojekt mit dem gleichen Namen wie das lokale Repository erstellt. Um das Repository in einem vorhandenen Teamprojekt zu erstellen, klicken Sie neben **Repositoryname** auf **Erweitert**, und wählen Sie ein Teamprojekt aus. Sie können den Code im Web anzeigen, indem Sie **Im Web anzeigen** auswählen.

## <a name="configure-continuous-delivery-with-vsts"></a>Konfigurieren von Continuous Delivery mit VSTS
Verwenden Sie den integrierten Assistenten in Visual Studio, um Continuous Delivery mit VSTS zu konfigurieren.

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Anwendungsprojekt **MyApplication**, und wählen Sie dann **Hinzufügen** -> **Continuous Delivery mit VSTS** aus. Ein Dialogfeld wird angezeigt, in dem Sie die erforderliche Konfiguration eingeben können.

    ![Continuous Delivery mit VSTS][continuous-delivery-with-VSTS]

    Die Werte für **Konto**, **Projekt** und **Git-Repository** werden automatisch eingegeben.

2. Wählen Sie als Agentwarteschlange **Hosted VS2017** aus.

3. Wählen Sie in der Dropdownliste **Clusterverbindung** den Eintrag **Verbindung erstellen** aus. Dadurch wird eine Website zum Konfigurieren eines Dienstendpunkts in VSTS geöffnet. 

4. Wählen Sie im Browserfenster **Neuer Dienstendpunkt** -> **Service Fabric** aus.

    ![Neuer Dienstendpunkt][new-service-endpoint]

    Ein Dialogfeld wird angezeigt, in dem Sie eine neue Service Fabric-Verbindung hinzuzufügen können.
    
5. Wählen Sie **Zertifikatbasiert** aus, und füllen Sie die Felder im Dialogfeld aus:

    ![Dialogfeld „Neuer Dienstendpunkt“][new-service-endpoint-dialog]

    1. Geben Sie einen **Verbindungsnamen** ein.
    2. Geben Sie im Feld **Clusterendpunkt** den Verwaltungsendpunkt des Clusters ein (z.B. tcp://mycluster.eastus.cloudapp.azure.com:19000). Wählen Sie als Protokoll „tcp://“ aus.  Der Standardport für den Verwaltungsendpunkt lautet 19000.
    3. Geben Sie den **Zertifikatfingerabdruck des Servers** ein.  Sie erhalten den Fingerabdruck durch Öffnen des Service Fabric Explorer für den Cluster (https://mycluster.eastus.cloudapp.azure.com:19080).
        - Wählen Sie in der Strukturansicht den Knoten **Cluster** und im rechten Bereich die Registerkarte **Manifest**.
        - Suchen Sie in der XML-Datei das **<ServerCertificate>**-Element, und kopieren Sie den Wert des **X509FindValue**-Attributs.
    4. Geben Sie im Feld **Clientzertifikat** das Clientzertifikat als Base64-codierte Zeichenfolge ein, damit das Zertifikat im Build-Agent installiert wird:
        - Stellen Sie sicher, dass Sie das Zertifikat als PFX-Datei zur Verfügung haben.
        - Führen Sie den folgenden PowerShell-Befehl aus, um die PFX-Datei als Base64-codierte Zeichenfolge in die Zwischenablage auszugeben:`[System.Convert]::ToBase64String([System.IO.File]::ReadAllBytes(C:\path\to\certificate.pfx)) | clip`
        - Fügen Sie den Wert aus der Zwischenablage in das Feld im Dialogfeld ein (STRG+V).
    5. Geben Sie im Feld **Kennwort** das **Zertifikatkennwort** ein, und klicken Sie auf **OK**.

6. Wählen Sie in Visual Studio im Feld **Clusterverbindung** des Dialogfelds **Continuous Delivery mit VSTS****<Refresh>** aus. In der Dropdownliste wird jetzt der Clusterendpunkt angezeigt, den Sie gerade erstellt haben.

7. Wählen Sie die Standardnamen für die Build- und Releasedefinition, oder ändern Sie die im Dialogfeld vorgeschlagenen Namen. Wenn Sie fertig sind, klicken Sie auf **OK**.

Nach einer Weile wird in Visual Studio ein Dialogfeld mit der Frage angezeigt, ob Sie die Build- und Releasedefinition im Browser öffnen möchten. Sie können diese Option auswählen, um ihre Konfiguration zu überprüfen, dies ist jedoch nicht erforderlich, um das Tutorial abzuschließen.

- Eine Team Services-Builddefinition beschreibt einen Workflow, der aus verschiedenen Buildschritten besteht, die nacheinander ausgeführt werden. Erstellen Sie eine Builddefinition, die ein Service Fabric-Anwendungspaket und andere Elemente erzeugt, um sie in einem Service Fabric-Cluster bereitzustellen. Weitere Informationen zu [Team Services-Builddefinitionen](https://www.visualstudio.com/docs/build/define/create).
- Eine Team Services-Releasedefinition beschreibt einen Workflow, der ein Anwendungspaket in einem Cluster bereitstellt. Bei gemeinsamer Verwendung führen die Builddefinition und Releasedefinition den gesamten Workflow aus, und zwar beginnend mit den Quelldateien und endend mit einer im Cluster ausgeführten Anwendung. Erfahren Sie mehr zu Team Services- [Releasedefinitionen](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition).

## <a name="commit-and-push-changes-trigger-a-release"></a>Committen und Pushen von Änderungen, Auslösen eines Release
Wir checken einige Codeänderungen in Team Services ein, um zu überprüfen, ob die Continuous Integration-Pipeline ordnungsgemäß ausgeführt wird.    

Während Sie den Code schreiben, werden Ihre Änderungen automatisch von Visual Studio nachverfolgt. Führen Sie den Commit der Änderungen in das lokale Git-Repository aus, indem Sie auf der Statusleiste unten rechts das Symbol „Ausstehende Änderungen“ (![Ausstehend][pending]) auswählen.

Fügen Sie in der Ansicht **Änderungen** in Team Explorer eine Nachricht hinzu, die die Aktualisierung beschreibt, und führen Sie den Commit der Änderungen aus.

![Commit für alle][changes]

Wählen Sie das Statusleistensymbol für nicht veröffentlichte Änderungen (![nicht unveröffentlichte Änderungen][unpublished-changes]) oder die Synchronisierungsansicht in Team Explorer aus. Wählen Sie **Push** aus, um den Code in Team Services/TFS zu aktualisieren.

![Änderungen pushen][push]

Durch das Pushen der Änderungen nach Team Services wird automatisch ein Build ausgelöst.  Nach erfolgreichem Abschluss der Builddefinition wird automatisch ein Release erstellt, und es wird ein Upgrade der Anwendung im Cluster gestartet.

Um den Buildstatus zu überprüfen, wechseln Sie in Visual Studio in **Team Explorer** zur Registerkarte **Builds**.  Nachdem Sie überprüft haben, ob der Build erfolgreich ausgeführt wird, legen Sie eine Releasedefinition fest, mit der die Anwendung in einem Cluster bereitgestellt wird.

Überprüfen Sie, ob die Bereitstellung erfolgreich war und die Anwendung im Cluster ausgeführt wird.  Öffnen Sie einen Webbrowser, und navigieren Sie zu [http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/).  Beachten Sie, dass die Anwendungsversion in diesem Beispiel „1.0.0.20170616.3“ lautet.

![Service Fabric Explorer][sfx1]

## <a name="update-the-application"></a>Aktualisieren der Anwendung
Nehmen Sie in der Anwendung Codeänderungen vor.  Speichern und committen Sie die Änderungen, indem Sie die vorherigen Schritte ausführen.

Sobald das Upgrade der Anwendung gestartet wird, können Sie in Service Fabric Explorer den Upgradestatus beobachten:

![Service Fabric Explorer][sfx2]

Das Anwendungsupgrade kann mehrere Minuten dauern. Wenn das Upgrade abgeschlossen ist, wird die nächste Version der Anwendung ausgeführt.  In diesem Beispiel ist dies „1.0.0.20170616.4“.

![Service Fabric Explorer][sfx3]

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Hinzufügen der Quellcodeverwaltung zum Projekt
> * Erstellen einer Builddefinition
> * Erstellen einer Releasedefinition
> * Automatisches Bereitstellen und Durchführen von Upgrades einer Anwendung

Nachdem Sie jetzt eine Anwendung bereitgestellt und Continuous Integration konfiguriert haben, können Sie folgende Aufgaben ausführen:
- [Ausführen des Upgrades einer App](service-fabric-application-upgrade.md)
- [Testen einer App](service-fabric-testability-overview.md) 
- [Überwachen und Diagnostizieren](service-fabric-diagnostics-overview.md)


<!-- Image References -->
[publish-app-profile]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishAppProfile.png
[push-git-repo]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishGitRepo.png
[publish-code]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishCode.png
[select-build-template]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SelectBuildTemplate.png
[add-task]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/AddTask.png
[new-task]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewTask.png
[set-continuous-integration]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SetContinuousIntegration.png
[add-cluster-connection]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/AddClusterConnection.png
[sfx1]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX1.png
[sfx2]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX2.png
[sfx3]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX3.png
[pending]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Pending.png
[changes]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Changes.png
[unpublished-changes]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/UnpublishedChanges.png
[push]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Push.png
[continuous-delivery-with-VSTS]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/VSTS-Dialog.png
[new-service-endpoint]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewServiceEndpoint.png
[new-service-endpoint-dialog]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewServiceEndpointDialog.png
