---
title: "Veröffentlichen einer App in einem Remotecluster mit Visual Studio | Microsoft Docs"
description: "Erfahren Sie, wie Sie eine Anwendung mit Visual Studio in einem Service Fabric-Remotecluster veröffentlichen."
services: service-fabric
documentationcenter: na
author: cawams
manager: timlt
editor: 
ms.assetid: faecd892-eb54-4d9c-8023-c67442afb8e8
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 07/29/2016
ms.author: cawa
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: e7bd4398503566684187831bde1e82f334a59f32


---
# <a name="publish-an-application-to-a-remote-cluster-by-using-visual-studio"></a>Veröffentlichen einer Anwendung in einem Remotecluster mit Visual Studio
> [!div class="op_single_selector"]
> * [PowerShell](service-fabric-deploy-remove-applications.md)
> * [Visual Studio](service-fabric-publish-app-remote-cluster.md)
> 
> 

<br/>

Die Azure Service Fabric-Erweiterung für Visual Studio bietet eine einfache, wiederholbare und skriptfähige Möglichkeit zum Veröffentlichen einer Anwendung in einem Service Fabric-Cluster.

## <a name="the-artifacts-required-for-publishing"></a>Für die Veröffentlichung erforderliche Artefakte
### <a name="deploy-fabricapplicationps1"></a>Deploy-FabricApplication.ps1
Dies ist ein PowerShell-Skript, das den Pfad zu einem Veröffentlichungsprofil als Parameter zum Veröffentlichen von Service Fabric-Anwendungen verwendet. Da dieses Skript Teil Ihrer Anwendung ist, können Sie es für Ihre Anwendung gern je nach Bedarf ändern.

### <a name="publish-profiles"></a>Veröffentlichungsprofile
Ein Ordner im Service Fabric-Anwendungsprojekt mit dem Namen **PublishProfiles** enthält XML-Dateien, in denen wichtige Informationen zum Veröffentlichen einer Anwendung gespeichert sind, beispielsweise:

* Service Fabric-Cluster-Verbindungsparameter
* Pfad zu einer Anwendungsparameterdatei
* Upgradeeinstellungen

Standardmäßig enthält Ihre Anwendung zwei Veröffentlichungsprofile: „Local.xml“ und „Cloud.xml“. Sie können weitere Profile hinzufügen, indem Sie eine der Standarddateien kopieren und einfügen.

### <a name="application-parameter-files"></a>Anwendungsparameterdateien
Der Ordner **ApplicationParameters** im Service Fabric-Anwendungsprojekt enthält XML-Dateien für benutzerdefinierte Werte für die Anwendungsmanifestparameter. Anwendungsmanifestdateien können parametrisiert werden, sodass Sie verschiedene Werte für Bereitstellungseinstellungen verwenden können. Weitere Informationen zur Parametrisierung Ihrer Anwendung finden Sie unter [Verwalten mehrerer Umgebungen in Service Fabric](service-fabric-manage-multiple-environment-app-configuration.md).

> [!NOTE]
> Für Actor-Dienste sollten Sie das Projekt zuerst erstellen, bevor Sie versuchen, die Datei in einem Editor oder über das Dialogfeld „Veröffentlichen“ zu bearbeiten. Der Grund ist, dass eine Teil der Manifestdateien während der Erstellung generiert wird.
> 
> 

## <a name="to-publish-an-application-by-using-the-publish-service-fabric-application-dialog-box"></a>So veröffentlichen Sie eine Anwendung mit dem Dialogfeld „Service Fabric-Anwendung veröffentlichen“
Folgende Schritte zeigen die Veröffentlichung einer Anwendung über das Dialogfeld **Service Fabric-Anwendung veröffentlichen** in den Visual Studio Service Fabric-Tools.

1. Wählen Sie im Kontextmenü des Service Fabric-Anwendungsprojekts die Option **Veröffentlichen...**, um das Dialogfeld **Service Fabric-Anwendung veröffentlichen** anzuzeigen.
   
    ![Dialogfeld **Service Fabric-Anwendung veröffentlichen**][0]
   
    In der im Dropdownlistenfeld **Zielprofil** ausgewählten Datei sind alle Einstellungen gespeichert, mit Ausnahme der **Manifestversionen**. Sie können ein vorhandenes Profil verwenden oder ein neues Profil erstellen, indem Sie im Dropdownlistenfeld **Zielprofil** die Option **<Profile verwalten…>** auswählen. Wenn Sie ein Veröffentlichungsprofil auswählen, werden die Inhalte des Profils in den entsprechenden Feldern des Dialogfelds angezeigt. Sie können Ihre Änderungen jederzeit speichern, indem Sie den Link **Profil speichern** auswählen.    
2. Im Abschnitt **Verbindungsendpunkt** können Sie den Veröffentlichungsendpunkt für einen lokalen Service Fabric-Cluster oder einen Service Fabric-Remotecluster festlegen. Klicken Sie auf die Dropdownliste **Verbindungsendpunkt** , um den Verbindungsendpunkt hinzuzufügen oder zu ändern. In der Liste werden die verfügbaren Verbindungsendpunkte des Service Fabric-Clusters angezeigt, in denen Sie die Veröffentlichung basierend auf Ihren Azure-Abonnements durchführen können. Beachten Sie Folgendes: Wenn Sie nicht bereits an Visual Studio angemeldet sind, werden Sie dazu aufgefordert.
   
    Verwenden Sie das Dialogfeld zum Auswählen der Cluster, um eine Auswahl aus den verfügbaren Abonnements und Clustern zu treffen.
   
    ![Dialogfeld **Service Fabric-Cluster auswählen**][1]
   
   > [!NOTE]
   > Wenn Sie die Veröffentlichung auf einem beliebigen Endpunkt durchführen möchten (z.B. einem Partycluster), helfen Ihnen die Informationen unter **Veröffentlichen auf einem beliebigen Clusterendpunkt** weiter.
   > 
   > 
   
    Nachdem Sie einen Endpunkt ausgewählt haben, überprüft Visual Studio die Verbindung zum ausgewählten Service Fabric-Cluster. Wenn der Cluster nicht sicher ist, kann Visual Studio sofort eine Verbindung herstellen. Wenn der Cluster jedoch sicher ist, müssen Sie ein Zertifikat auf Ihrem lokalen Computer installieren, bevor Sie fortfahren können. Weitere Informationen finden Sie unter [Konfigurieren von sicheren Verbindungen](service-fabric-visualstudio-configure-secure-connections.md) . Wenn Sie fertig sind, klicken Sie auf die Schaltfläche **OK** . Der ausgewählte Cluster wird im Dialogfeld **Service Fabric-Anwendung veröffentlichen** angezeigt.
3. Navigieren Sie im Dropdown-Listenfeld **Anwendungsparameterdatei** zu einer Datei mit Anwendungsparametern. Eine Anwendungsparameterdatei enthält benutzerdefinierte Werte für Parameter in der Anwendungsmanifestdatei. Klicken Sie auf die Schaltfläche **Bearbeiten** , um einen Parameter zu ändern oder hinzuzufügen. Im Raster **Parameter** geben Sie den Wert für einen Parameter ein oder ändern einen Wert. Klicken Sie dann auf die Schaltfläche **Speichern** .
   
    ![Dialogfeld **Parameter bearbeiten**][2]
4. Verwenden Sie das Kontrollkästchen **Anwendung aktualisieren** , um anzugeben, ob es sich bei dieser Veröffentlichungsaktion um ein Upgrade handelt. Aktionen zum Upgrade von Anwendungen unterscheiden sich von normalen Veröffentlichungsaktionen. Unter [Service Fabric-Anwendungsupgrade](service-fabric-application-upgrade.md) finden Sie eine Liste der Unterschiede. Zum Konfigurieren der Upgradeeinstellungen wählen Sie den Link **Upgradeeinstellungen konfigurieren** aus. Der Editor für Upgradeparameter wird angezeigt. Weitere Informationen zu Upgradeparametern finden Sie unter [Konfigurieren des Upgrades einer Service Fabric-Anwendung](service-fabric-visualstudio-configure-upgrade.md) .
5. Wählen Sie die Schaltfläche **Manifestversionen...** aus, um das Dialogfeld **Versionen bearbeiten** anzuzeigen. Sie müssen die Anwendungs- und Dienstversionen aktualisieren, damit ein Upgrade erfolgen kann. Weitere Informationen dazu, wie sich Anwendungs- und Dienstmanifestversionen auf einen Upgradeprozess auswirken, finden Sie unter [Tutorial für Service Fabric-Anwendungsupgrades](service-fabric-application-upgrade-tutorial.md) .
   
    ![Dialogfeld **Versionen bearbeiten**][3]
   
    Wenn bei den Anwendungs- und Dienstversionen semantische Versionsbezeichnungen wie 1.0.0 oder numerische Werte im Format 1.0.0.0 verwendet werden, aktivieren Sie die Option **Anwendungs- und Dienstversionen automatisch aktualisieren** . Bei Auswahl dieser Option werden die Versionsnummern für Dienst und Anwendung automatisch aktualisiert, sobald eine Code-, Konfigurationsdatei- oder Datenpaketversion aktualisiert wird. Wenn Sie die Versionen lieber manuell bearbeiten möchten, deaktivieren Sie das Kontrollkästchen, um diese Funktion auszuschalten.
   
   > [!NOTE]
   > Damit alle Paketeinträge für ein Actor-Projekt angezeigt werden, erstellen Sie das Projekt zuerst, um die Einträge in den Dienstmanifestdateien zu generieren.
   > 
   > 
6. Wenn Sie alle erforderlichen Einstellungen festgelegt haben, klicken Sie auf die Schaltfläche **Veröffentlichen** , um die Anwendung im ausgewählten Service Fabric-Cluster zu veröffentlichen. Die von Ihnen festgelegten Einstellungen werden auf den Veröffentlichungsprozess angewendet.

## <a name="publish-to-an-arbitrary-cluster-endpoint-including-party-clusters"></a>Veröffentlichen auf einem beliebigen Clusterendpunkt (einschließlich Party Cluster)
Die Veröffentlichungsoberfläche von Visual Studio ist zum Veröffentlichen auf Remoteclustern optimiert, das einem Ihrer Azure-Abonnements zugeordnet ist. Es ist aber möglich, die Veröffentlichung auf beliebigen Endpunkten durchzuführen (z. B. Service Fabric-Party Clustern), indem die XML-Datei mit dem Veröffentlichungsprofil direkt bearbeitet wird. Wie oben beschrieben, werden standardmäßig zwei Veröffentlichungsprofile bereitgestellt: **Local.xml** und **Cloud.xml**. Sie können aber gern weitere Profile für unterschiedliche Umgebungen erstellen. Es kann beispielsweise sein, dass Sie ein Profil für die Veröffentlichung auf Partyclustern erstellen möchten, z.B. **Party.xml**.

Wenn Sie eine Verbindung mit einem nicht geschützten Cluster herstellen, benötigen Sie lediglich den Cluster-Verbindungsendpunkt, z. B. `partycluster1.eastus.cloudapp.azure.com:19000`. In diesem Fall sieht der Verbindungsendpunkt im Veröffentlichungsprofil etwa wie folgt aus:

```XML
<ClusterConnectionParameters ConnectionEndpoint="partycluster1.eastus.cloudapp.azure.com:19000" />
```

  Wenn Sie eine Verbindung mit einem geschützten Cluster herstellen, müssen Sie auch die Details des Clientzertifikats aus dem lokalen Speicher angeben, das für die Authentifizierung verwendet wird. Weitere Informationen finden Sie unter [Konfigurieren sicherer Verbindungen mit einem Service Fabric-Cluster](service-fabric-visualstudio-configure-secure-connections.md).

  Nachdem Sie das Veröffentlichungsprofil eingerichtet haben, können Sie im Veröffentlichungsdialogfeld wie unten dargestellt darauf verweisen.

  ![Neues Veröffentlichungsprofil im Dialogfeld „Veröffentlichen“][4]

  Beachten Sie in diesem Fall, dass das neue Veröffentlichungsprofil auf eine der standardmäßigen Anwendungsparameterdateien verweist. Dies ist richtig, wenn Sie eine Anwendungskonfiguration in mehreren Umgebungen veröffentlichen möchten. Falls Sie für jede Umgebung, für die die Veröffentlichung durchgeführt werden soll, eine andere Konfiguration verwenden möchten, sollten Sie eine entsprechende Anwendungsparameterdatei erstellen.

## <a name="next-steps"></a>Nächste Schritte
Informationen zur Automatisierung des Veröffentlichungsprozesses in einer Continuous Integration-Umgebung finden Sie unter [Einrichten von Continuous Integration für Service Fabric](service-fabric-set-up-continuous-integration.md).

[0]: ./media/service-fabric-publish-app-remote-cluster/PublishDialog.png
[1]: ./media/service-fabric-publish-app-remote-cluster/SelectCluster.png
[2]: ./media/service-fabric-publish-app-remote-cluster/EditParams.png
[3]: ./media/service-fabric-publish-app-remote-cluster/EditVersions.png
[4]: ./media/service-fabric-publish-app-remote-cluster/publish-to-party-cluster.png



<!--HONumber=Feb17_HO2-->


