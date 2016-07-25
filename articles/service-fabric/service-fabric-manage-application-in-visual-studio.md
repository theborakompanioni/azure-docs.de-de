<properties
   pageTitle="Verwalten Ihrer Anwendungen in Visual Studio | Microsoft Azure"
   description="Verwenden Sie Visual Studio zum Erstellen, Entwickeln, Packen, Bereitstellen und Debuggen Ihrer Service Fabric-Anwendungen und -Dienste."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/07/2016"
   ms.author="seanmck"/>

# Verwenden von Visual Studio zum Vereinfachen des Schreibens und Verwaltens Ihrer Service Fabric-Anwendung

Sie können Ihre Azure Service Fabric-Anwendungen und -Dienste in Visual Studio verwalten. Sobald Sie die [Einrichtung Ihrer Entwicklungsumgebung](service-fabric-get-started.md) abgeschlossen haben, können Sie mit Visual Studio Service Fabric-Anwendungen erstellen, Dienste hinzufügen und Anwendungen packen, registrieren und im lokalen Entwicklungscluster bereitstellen.

## Bereitstellen der Service Fabric-Anwendung

Beim Bereitstellen einer Anwendung werden die folgenden Schritte standardmäßig zu einem einfachen Vorgang zusammengefasst:

1. Erstellen des Anwendungspakets
2. Hochladen des Anwendungspakets in den Image-Speicher
3. Registrieren des Anwendungstyps
4. Entfernen ausgeführter Anwendungsinstanzen
5. Erstellen einer neuen Anwendungsinstanz

Sie können in Visual Studio auch **F5** drücken, um die Anwendung bereitzustellen und den Debugger an alle Anwendungsinstanzen anzufügen. Sie können mithilfe von **STRG+F5** eine Anwendung ohne Debuggen bereitstellen oder sie in einem lokalen bzw. Remotecluster mit dem Veröffentlichungsprofil veröffentlichen. Weitere Informationen finden Sie unter [Veröffentlichen einer Anwendung in einem Remotecluster mit Visual Studio](service-fabric-publish-app-remote-cluster.md).

### Debugmodus für die Anwendung

Standardmäßig entfernt Visual Studio vorhandene Instanzen Ihres Anwendungstyps, wenn Sie das Debuggen beenden oder (falls Sie die App ohne angefügten Debugger bereitgestellt haben) wenn Sie die Anwendung erneut bereitstellen. In diesem Fall werden sämtliche Daten der Anwendung entfernt. Beim lokalen Debuggen sollen aber unter Umständen bereits erstellte Daten erhalten bleiben, wenn Sie eine neue Version der Anwendung testen. Die Visual Studio Service Fabric-Tools enthalten eine Eigenschaft namens **Application Debug Mode** (Anwendungsdebugmodus), die steuert, ob die Anwendung beim Drücken von F5 deinstalliert wird oder ob sie nach Abschluss einer Debugsitzung erhalten bleiben soll.

#### So legen Sie die „Application Debug Mode“-Eigenschaft fest

1. Wählen Sie im Kontextmenü des Anwendungsprojekts **Eigenschaften** aus (oder drücken Sie **F4**).
2. Legen Sie im Eigenschaftenfenster die **Application Debug Mode**-Eigenschaft entweder auf **Remove** (Entfernen) oder auf **Auto Upgrade** (Automatisches Upgrade) fest.

    ![Festlegen der „Application Debug Mode“-Eigenschaft][debugmodeproperty]

Durch das Festlegen dieses Eigenschaftswerts auf **Auto Upgrade** (Automatisches Upgrade) wird die Anwendung im lokalen Cluster weiter ausgeführt. Bei der nächsten Verwendung von F5 wird die Bereitstellung wie ein Upgrade behandelt, indem der nicht überwachte automatische Modus verwendet wird, um die Anwendung schnell auf eine neuere Version mit angehängter Datumszeichenfolge zu aktualisieren. Beim Upgradevorgang werden alle Daten beibehalten, die Sie in einer vorherigen Debugsitzung eingegeben haben.

![Beispiel für neue Anwendungsversion mit angefügtem Datum (date1)][preservedate]

Daten werden dank der Nutzung der Anwendungsupgradefunktionen von Service Fabric beibehalten, der Optimierungsschwerpunkt liegt jedoch eher auf Leistung und weniger auf Sicherheit. Weitere Informationen zum Upgraden von Anwendungen sowie zur praktischen Durchführung eines Upgrades finden Sie unter [Service Fabric-Anwendungsupgrade](service-fabric-application-upgrade.md).

>[AZURE.NOTE] Diese Eigenschaft ist erst ab Version 1.1 der Service Fabric-Tools für Visual Studio verfügbar. Verwenden Sie in Versionen vor 1.1 die **Preserve Data On Start**-Eigenschaft, um das gleiche Ergebnis zu erzielen.

## Fügen Sie der Service Fabric-Anwendung einen Dienst hinzu.

Sie können der Anwendung neue Dienste hinzufügen, um den Funktionsumfang der Anwendung zu erweitern. Um sicherzustellen, dass der Dienst in das Anwendungspaket aufgenommen wird, fügen Sie den Dienst über das Menüelement **Neuer Fabric-Dienst** hinzu.

![Neuen Fabric-Dienst zur Anwendung hinzufügen][newservice]

Wählen Sie den Service Fabric-Projekttyp aus, der zur Anwendung hinzugefügt werden soll, und geben Sie einen Namen für den Dienst an. Lesen Sie die Informationen in [Auswählen eines Frameworks für den Dienst](service-fabric-choose-framework.md), um zu entscheiden, welcher Diensttyp verwendet werden soll.

![Projekttyp des Fabric-Diensts auswählen, das zur Anwendung hinzugefügt werden soll][addserviceproject]

Der neue Dienst wird zur Projektmappe und dem vorhandenen Anwendungspaket hinzugefügt. Die Dienstverweise und eine Standarddienstinstanz werden zum Anwendungsmanifest hinzugefügt. Der Dienst wird erstellt und mit der nächsten Bereitstellung der Anwendung gestartet.

![Der neue Dienst wird zum Anwendungsmanifest hinzugefügt][newserviceapplicationmanifest]

## Packen der Service Fabric-Anwendung

Um die Anwendung und die zugehörigen Dienste in einem Cluster bereitzustellen, müssen Sie ein Anwendungspaket erstellen. Das Paket ordnet das Anwendungsmanifest, das bzw. die Dienstmanifeste und andere erforderliche Dateien in einem bestimmten Layout an. Visual Studio richtet das Paket im Projektordner der Anwendung im Verzeichnis "Pkg" ein und verwaltet es. Klicken Sie zum Erstellen oder Aktualisieren des Anwendungspakets im Kontextmenü der **Anwendung** auf **Packen**. Dies ist möglicherweise erforderlich, falls Sie die Anwendung mithilfe benutzerdefinierter PowerShell-Skripts bereitstellen möchten.

## Entfernen von Anwendungen und Anwendungstypen mit dem Cloud-Explorer

Einfache Clusterverwaltungsvorgänge können in Visual Studio mit dem Cloud-Explorer ausgeführt werden, den Sie über das Menü **Ansicht** starten können. Damit können Sie beispielsweise Anwendung löschen und die Bereitstellung von Anwendungstypen in lokalen Clustern oder Remoteclustern aufheben.

![Entfernen einer Anwendung](./media/service-fabric-manage-application-in-visual-studio/removeapplication.png)

>[AZURE.TIP] Informationen zu komplexeren Clusterverwaltungsfunktionen finden Sie unter [Visualisieren Ihres Clusters mit Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## Nächste Schritte

- [Service Fabric-Anwendungsmodell](service-fabric-application-model.md)
- [Service Fabric-Anwendungsbereitstellung](service-fabric-deploy-remove-applications.md)
- [Verwalten von Anwendungsparametern für mehrere Umgebungen](service-fabric-manage-multiple-environment-app-configuration.md)
- [Debuggen einer Service Fabric-Anwendung](service-fabric-debugging-your-application.md)
- [Visualisieren des Clusters mit Service Fabric-Explorer](service-fabric-visualizing-your-cluster.md)

<!--Image references-->
[addserviceproject]: ./media/service-fabric-manage-application-in-visual-studio/addserviceproject.png
[manageservicefabric]: ./media/service-fabric-manage-application-in-visual-studio/manageservicefabric.png
[newservice]: ./media/service-fabric-manage-application-in-visual-studio/newservice.png
[newserviceapplicationmanifest]: ./media/service-fabric-manage-application-in-visual-studio/newserviceapplicationmanifest.png
[preservedata]: ./media/service-fabric-manage-application-in-visual-studio/preservedata.png
[preservedate]: ./media/service-fabric-manage-application-in-visual-studio/preservedate.png
[debugmodeproperty]: ./media/service-fabric-manage-application-in-visual-studio/debugmodeproperty.png

<!---HONumber=AcomDC_0713_2016-->