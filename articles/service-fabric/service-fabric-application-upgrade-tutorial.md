 <properties
   pageTitle="Tutorial für Service Fabric-Anwendungsupgrades | Microsoft Azure"
   description="Dieser Artikel bietet eine exemplarische Vorgehensweise für das Bereitstellen einer Service Fabric-Anwendung, das Ändern des Codes und das Einführen eines Upgrades mithilfe von Visual Studio."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/13/2016"
   ms.author="subramar"/>



# Tutorial für das Upgraden von Service Fabric-Anwendungen mithilfe von Visual Studio

Azure Service Fabric vereinfacht das Upgraden von Cloudanwendungen, indem sichergestellt wird, dass nur geänderte Dienste aktualisiert werden und die Anwendungsintegrität während des gesamten Upgradeprozesses überwacht wird. Falls ein Fehler auftritt, wird die Anwendung automatisch auf die vorherigen Version zurückgesetzt. Upgrades von Service Fabric-Anwendungen erfolgen *ohne Ausfallzeiten*, da die Anwendung ohne Ausfallzeiten aktualisiert werden kann. In diesem Tutorial wird beschrieben, wie ein einfaches paralleles Upgrade über Visual Studio erfolgt.


## Schritt 1: Erstellen und Veröffentlichen des Beispiels Visual Objects

Zu Beginn sollten Sie die Anwendung [Visual Objects](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/master/Actors/VisualObjects) von GitHub herunterladen. Anschließend können Sie die Anwendung erstellen und veröffentlichen, indem Sie wie nachstehend gezeigt mit der rechten Maustaste auf das Anwendungsprojekt > **VisualObjectsApplication** klicken und anschließend im Service Fabric-Menü den Befehl **Veröffentlichen** auswählen.

![Kontextmenü einer Service Fabric-Anwendung][image1]

Hierdurch wird ein weiteres Popup geöffnet, in dem Sie das **Zielprofil** auf **PublishProfiles\\Local.xml** festlegen können. Das Fenster sollte wie folgt aussehen, bevor Sie auf „**Veröffentlichen**“ klicken.

![Veröffentlichen einer Service Fabric-Anwendung][image2]

Sie können nun im Dialogfeld auf „**Veröffentlichen**“ klicken. Nun können Sie im [Service Fabric-Explorer den Cluster und die Anwendung anzeigen](service-fabric-visualizing-your-cluster.md). Die Anwendung „Visual Objects“ beinhaltet einen Webdienst, zu dem Sie in Ihrem Browser wechseln können, indem Sie in die Adressleiste [http://localhost:8082/visualobjects/](http://localhost:8082/visualobjects/) eingeben. Sie sollten 10 unverankerte visuelle Objekte sehen, die sich auf dem Bildschirm bewegen.

## Schritt 2: Aktualisieren des Beispiels „Visual Objects“

Sie werden feststellen, dass sich die visuellen Objekte mit der Version, die in Schritt 1 bereitgestellt wurde, nicht drehen. Wir aktualisieren diese Anwendung so, dass sich die visuellen Objekte drehen.

Wählen Sie das Projekt „VisualObjects.ActorService“ in der Projektmappe „VisualObjects“ aus, und öffnen Sie die Datei **VisualObjectActor.cs**. Navigieren Sie in der Datei zur `MoveObject`-Methode, kommentieren Sie `visualObject.Move(false)` aus, und heben Sie die Auskommentierung für `visualObject.Move(true)` auf. Diese Änderung bewirkt, dass sich die Objekte nach dem Upgrade des Diensts drehen. **Jetzt können Sie die Projektmappe erstellen (nicht neu erstellen)**, die die geänderten Projekte erstellt. Wenn Sie die Option *Alles neu erstellen* auswählen, müssen Sie die Versionen für alle Projekte aktualisieren.

Wir müssen unsere Anwendung auch mit einer Version versehen. Sie können die Visual Studio-Option **Manifestversionen bearbeiten** verwenden, um die Versionen zu ändern, nachdem Sie mit einem Rechtsklick das **VisualObjects**-Projekt ausgewählt haben. Hierdurch wird das Dialogfeld zum Bearbeiten von Versionen wie folgt angezeigt:

![Dialogfeld Versionsverwaltung][image3]

Aktualisieren Sie die Versionen für die geänderten Projekte und ihre Codepakete zusammen mit der Anwendung auf Version 2.0.0. Nach diesen Änderungen sollte das Manifest wie folgt aussehen (Änderungen sind fett hervorgehoben):

![Aktualisieren von Versionen][image4]

Die Visual Studio-Tools können automatische Rollups von Versionen durchführen, wenn Sie die Option **Anwendung und Dienstversionen automatisch aktualisieren** auswählen. Wenn Sie [SemVer](http://www.semver.org) verwenden, müssen Sie den Code und/oder die Version der Paketkonfiguration aktualisieren, wenn diese Option ausgewählt ist.

Speichern Sie die Änderungen, und aktivieren Sie nun das Kontrollkästchen **Anwendungs- und Dienstversionen automatisch aktualisieren**.


## Schritt 3: Upgrade der Anwendung

Bitte machen Sie sich mit den [Parametern für Anwendungsupgrades](service-fabric-application-upgrade-parameters.md) und dem [Upgradeprozess](service-fabric-application-upgrade.md) vertraut, um ein tieferes Verständnis der verschiedenen anwendbaren Upgradeparameter, Timeouts und Integritätskriterien zu erhalten. In dieser exemplarischen Vorgehensweise behalten wir die Standardwerte der Evaluierungskriterien für die Dienstintegrität bei (nicht überwachter Modus). Sie können diese Einstellungen konfigurieren, indem Sie **Upgradeeinstellungen konfigurieren** auswählen und die Parameter anschließend wie gewünscht ändern.

Nun können wir das Anwendungsupgrade über die Schaltfläche **Veröffentlichen** starten. Dadurch wird die Anwendung auf Version 2.0.0 aktualisiert, in der sich die Objekte drehen. Sie werden feststellen, dass Service Fabric Updatedomänen nacheinander aktualisiert (einige Objekte werden zuerst aktualisiert, dann folgen andere). Auf den Dienst kann während dieses Zeitraums über Ihren Client (Browser) zugegriffen werden.


Sie können den Fortschritt des Upgradevorgangs mithilfe von Service Fabric Explorer über die Registerkarte **Upgrades in Progress** (Laufende Upgrades) der jeweiligen Anwendung überwachen.

Alle Updatedomänen sollten binnen wenigen Minuten aktualisiert sein und im Visual Studio-Ausgabefenster sollte angezeigt werden, dass das Upgrade abgeschlossen ist. Sie werden feststellen, dass sich jetzt *alle* visuellen Objekte in Ihrem Browserfenster drehen!

Sie können auch versuchen, als Übung Version 2.0.0 in Version 3.0.0 oder gar Version 2.0.0 zurück in Version 1.0.0 zu ändern. Experimentieren Sie mit den Timeouts und Integritätsrichtlinien, um sich schrittweise mit ihnen vertraut zu machen. Wenn Sie einen Azure-Cluster bereitstellen, werden sich die verwendeten Parameter von den Parametern unterscheiden, die Sie verwenden, wenn Sie einen lokalen Cluster bereitstellen möchten; Es wird empfohlen, dass Sie die Timeouts konservativ festlegen.


## Nächste Schritte

[Ihre Anwendung mit PowerShell upgraden](service-fabric-application-upgrade-tutorial-powershell.md) beschreibt das Upgraden von Anwendungen mit PowerShell.

Steuern Sie mithilfe von [Upgradeparametern](service-fabric-application-upgrade-parameters.md), wie Ihre Anwendung upgegradet wird.

Gewährleisten Sie die Kompatibilität Ihrer Anwendungsupgrades, indem Sie sich mit der [Datenserialisierung](service-fabric-application-upgrade-data-serialization.md) vertraut machen.

Beschäftigen Sie sich mit den [weiterführenden Themen](service-fabric-application-upgrade-advanced.md), um zu erfahren, wie Sie erweiterte Funktionen beim Upgrade Ihrer Anwendung nutzen.

Informationen zum Beheben gängiger Probleme bei Anwendungsupgrades finden Sie in den Anweisungen unter [Problembehandlung bei Anwendungsupgrades](service-fabric-application-upgrade-troubleshooting.md).



[image1]: media/service-fabric-application-upgrade-tutorial/upgrade7.png
[image2]: media/service-fabric-application-upgrade-tutorial/upgrade1.png
[image3]: media/service-fabric-application-upgrade-tutorial/upgrade5.png
[image4]: media/service-fabric-application-upgrade-tutorial/upgrade6.png

<!---HONumber=AcomDC_0518_2016-->