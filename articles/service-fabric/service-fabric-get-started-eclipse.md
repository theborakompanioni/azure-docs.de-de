---
title: "Erste Schritte mit dem Eclipse-Plug-In für Azure Service Fabric | Microsoft Dokumentation"
description: "Enthält eine Beschreibung der ersten Schritte mit dem Eclipse-Plug-In für Azure Service Fabric."
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: 
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/27/2016
ms.author: saysa
translationtype: Human Translation
ms.sourcegitcommit: 24d86e17a063164c31c312685c0742ec4a5c2f1b
ms.openlocfilehash: 891df38aa685cac7bbfecb71b15c88d557ac493b
ms.lasthandoff: 03/11/2017


---

# <a name="getting-started-with-eclipse-plugin-for-service-fabric-java-application-development"></a>Erste Schritte mit dem Eclipse-Plug-In für die Entwicklung von Service Fabric-Java-Anwendungen
Eclipse ist eine der am häufigsten verwendeten IDEs für Java-Entwickler. In diesem Artikel wird beschrieben, wie Sie Ihre Eclipse-Entwicklungsumgebung für Service Fabric einrichten. Im Artikel erhalten Sie Hilfe beim Installieren des Plug-Ins, Erstellen von Service Fabric-Anwendungen und Bereitstellen Ihrer Service Fabric-Anwendung in der lokalen Umgebung oder im Service Fabric-Remotecluster.

## <a name="install-or-update-service-fabric-plugin-on-eclipse-neon"></a>Installieren oder Aktualisieren des Service Fabric-Plug-Ins unter Eclipse Neon
Service Fabric enthält ein Plug-In für die **Eclipse-IDE für Java-Entwickler**, mit dem die Erstellung und Bereitstellung von Java-Diensten vereinfacht werden kann.

1. Stellen Sie sicher, dass Sie die aktuellen Versionen von Eclipse **Neon** und Buildship (1.0.17 oder höher) installiert haben. Die Version der installierten Komponenten können Sie unter ``Help => Installation Details`` ermitteln. Eine Aktualisierungsanleitung für Buildship finden Sie [hier][buildship-update]. Unter ``Help => Check for Updates`` können Sie prüfen, ob Sie über die aktuelle Version von Eclipse Neon verfügen, und Updates durchführen.

2. Wählen Sie ``Help => Install New Software...``, um das Service Fabric-Plug-In zu installieren.
  1. Geben Sie im Textfeld „Work with“ (Verwenden mit) Folgendes ein: ``http://dl.windowsazure.com/eclipse/servicefabric``.
  2. Klicken Sie auf "Hinzufügen".

  ![Eclipse Neon-Plug-In für Service Fabric][sf-eclipse-plugin-install]

  3. Wählen Sie das Service Fabric-Plug-In aus, und klicken Sie auf „Weiter“.
  4. Fahren Sie mit der Installation fort, und akzeptieren Sie den Endbenutzer-Lizenzvertrag.

Falls Sie das Service Fabric-Eclipse-Plug-In bereits installiert haben, sollten Sie sicherstellen, dass es sich um die aktuelle Version handelt. Unter ``Help => Installation Details`` können Sie prüfen, ob weitere Updates möglich sind. Suchen Sie in der Liste mit den installierten Plug-Ins dann nach „Service Fabric“, und klicken Sie auf „Aktualisieren“. Falls ein ausstehendes Update vorhanden ist, wird es abgerufen und installiert.

> [!NOTE]
> Wenn die Installation oder das Update eines Service Fabric-Eclipse-Plug-Ins für Eclipse lange dauert, hat dies folgenden Grund: Eclipse versucht bei jedem Vorgang, Metadaten mit allen neuen Änderungen für alle Aktualisierungsstandorte abzurufen, die für Ihre Eclipse-Instanz registriert sind. Um diesen Vorgang zu beschleunigen, können Sie einen Trick anwenden: Navigieren Sie zu `Available Software Sites`, und deaktivieren Sie alle Optionen, mit Ausnahme von `http://dl.windowsazure.com/eclipse/servicefabric` für den Speicherort des Service Fabric-Plug-Ins.
>

## <a name="create-service-fabric-application-using-eclipse"></a>Erstellen einer Service Fabric-Anwendung mit Eclipse

1. Wechseln Sie zur Adresse ``File => New => Other``. Wählen Sie die Option ``Service Fabric Project``. Klicken Sie auf ``Next``.

    ![Service Fabric – Neues Projekt – Seite 1][create-application/p1]

2. Geben Sie Ihrem Projekt einen Namen. Klicken Sie auf ``Next``.

    ![Service Fabric – Neues Projekt – Seite 2][create-application/p2]

3. Wählen Sie aus den verfügbaren Vorlagen die Dienstvorlage aus (Akteur, Zustandslos, Container oder ausführbare Gastanwendungsdatei). Klicken Sie auf ``Next``.

    ![Service Fabric – Neues Projekt – Seite 3][create-application/p3]

4. Geben Sie den Dienstnamen bzw. die relevanten Dienstdetails auf dieser Seite ein, und klicken Sie auf ``Finish``.

    ![Service Fabric – Neues Projekt – Seite 4][create-application/p4]

5. Wenn Sie Ihr erstes Service Fabric-Projekt erstellen, werden Sie gefragt, ob Sie die Service Fabric-Perspektive festlegen möchten. Wählen Sie ``yes``.

    ![Service Fabric – Neues Projekt – Seite 5][create-application/p5]

6. Nach der erfolgreichen Erstellung sieht das Projekt wie folgt aus:

    ![Service Fabric – Neues Projekt – Seite 6][create-application/p6]

## <a name="build-and-deploy-the-service-fabric-application-using-eclipse"></a>Erstellen und Bereitstellen der Service Fabric-Anwendung mit Eclipse

* Klicken Sie mit der rechten Maustaste auf die Service Fabric-Anwendung, die Sie soeben erstellt haben. Wählen Sie im Kontextmenü die Option ``Service Fabric``. Ein Untermenü mit mehreren Optionen wird angezeigt. Dies sieht wie folgt aus:

    ![Service Fabric-Kontextmenü][publish/RightClick]

  Nachdem Sie auf die Optionen zum Erstellen, erneuten Erstellen und Bereinigen geklickt haben, werden die gewünschten Aktionen durchgeführt.
  - Mit ``Build Application`` wird die Anwendung ohne Bereinigung erstellt.
  - Mit ``Rebuild Application`` wird eine bereinigte Erstellung der Anwendung durchgeführt.
  - Mit ``Clean Application`` wird die Anwendung in Bezug auf die Buildartefakte bereinigt.


* In diesem Menü können Sie die Anwendung auch bereitstellen und veröffentlichen und die Bereitstellung dafür aufheben.
  - Mit ``Deploy Application`` wird die Bereitstellung im lokalen Cluster durchgeführt.
  - Mit ``Publish Application...`` wird gefragt, welches Veröffentlichungsprofil Sie wählen möchten (``Local.json`` und ``Cloud.json``). Diese JSON-Dateien werden zum Speichern von Informationen verwendet (z.B. Verbindungsendpunkte und Sicherheitsinformationen), die zum Herstellen der Verbindung mit dem lokalen Cluster oder Cloudcluster (Azure) benötigt werden.

  ![Service Fabric-Kontextmenü][publish/Publish]

* Dies ist eine Alternative zur Bereitstellung Ihrer Service Fabric-Anwendung mithilfe von Konfigurationen für die Eclipse-Ausführung.

  1. Wählen Sie die Option ``Run => Run Configurations``. Wählen Sie unter ``Grade Project`` die Ausführungskonfiguration ``ServiceFabricDeployer``.
  2. Geben Sie rechts auf der Registerkarte ``Arguments`` als ``publishProfile`` die Option **local** oder **cloud** an. Standardmäßig ist **local** (lokal) angegeben. Wählen Sie zur Bereitstellung für einen Remote-/Cloudcluster die Option **cloud**.
  3. Stellen Sie sicher, dass die richtigen Informationen in den Veröffentlichungsprofilen eingefügt werden, indem Sie `Local.json` oder `Cloud.json` entsprechend ändern und die Endpunktdetails und, falls vorhanden, Sicherheitsanmeldeinformationen angeben.
  4. Vergewissern Sie sich, dass ``Working Directory`` im rechten Bereich unter ``Grade Project`` auf die Anwendung verweist, die Sie bereitstellen möchten. Wenn dies nicht der Fall ist, können Sie auf die Schaltfläche ``Workspace...`` klicken und die gewünschte Anwendung auswählen.
  5. Klicken Sie auf **Anwenden** und **Ausführen**.

Ihre Anwendung wird innerhalb weniger Augenblicke erstellt und bereitgestellt. Der Status kann über Service Fabric Explorer verfolgt werden.  

## <a name="add-new-service-fabric-service-to-your-service-fabric-application"></a>Hinzufügen eines neuen Service Fabric-Diensts zur Service Fabric-Anwendung

Sie können einer vorhandenen Service Fabric-Anwendung einen neuen Service Fabric-Dienst hinzufügen, indem Sie die folgenden Schritte verwenden:

1. Klicken Sie mit der rechten Maustaste auf das Projekt, dem Sie einen Dienst hinzufügen möchten, öffnen Sie das Kontextmenü, und wählen Sie die Option „Service Fabric“. Ein Untermenü mit mehreren Optionen wird angezeigt.

    ![Service Fabric – Dienst hinzufügen – Seite 1][add-service/p1]

2. Wählen Sie die Option `Add ServiceFabric Service`. Sie werden durch die Schritte zum Hinzufügen eines Diensts zum Projekt geführt.
3. Wählen Sie die Dienstvorlage aus, die Sie dem Projekt hinzufügen möchten, und klicken Sie auf „Weiter“.

    ![Service Fabric – Dienst hinzufügen – Seite 2][add-service/p2]

4. Geben Sie den Dienstnamen ein (und weitere Details, falls erforderlich), und klicken Sie unten auf die Schaltfläche „Dienst hinzufügen“.  

    ![Service Fabric – Dienst hinzufügen – Seite 3][add-service/p3]

5. Nachdem der Dienst erfolgreich hinzugefügt wurde, sieht die gesamte Projektstruktur in etwa wie folgt aus:

    ![Service Fabric – Dienst hinzufügen – Seite 4][add-service/p4]

## <a name="upgrade-your-service-fabric-java-application"></a>Aktualisieren der Service Fabric-Java-Anwendung

Angenommen, Sie haben das Projekt ``App1`` mit dem Service Fabric-Eclipse-Plug-In erstellt und bereitgestellt, um eine Anwendung mit dem Namen ``fabric:/App1Application``, dem Anwendungstyp ``App1AppicationType`` und der Anwendungsversion 1.0 zu erstellen. Nun möchten Sie die Anwendung aktualisieren, ohne dass es zu einem Ausfall kommt.

Ändern Sie Ihre Anwendung, und erstellen Sie den geänderten Dienst neu.  Aktualisieren Sie die Manifestdatei des geänderten Diensts (``ServiceManifest.xml``) mit den aktualisierten Versionen für den Dienst (und ggf. mit aktualisiertem Code, aktualisierter Konfiguration oder aktualisierten Daten). Passen Sie auch das Manifest der Anwendung (``ApplicationManifest.xml``) mit der aktualisierten Versionsnummer für die Anwendung und dem geänderten Dienst an.  

Zum Aktualisieren Ihrer Anwendung mit Eclipse können Sie eine duplizierte Ausführungskonfiguration erstellen und verwenden, um die Anwendung wie gewünscht zu aktualisieren. Führen Sie hierzu die folgenden Schritte aus:
1. Wählen Sie die Option ``Run => Run Configurations``. Klicken Sie im linken Bereich auf den kleinen Pfeil links neben ``Grade Project``.
2. Klicken Sie mit der rechten Maustaste auf ``ServiceFabricDeployer``, und wählen Sie ``Duplicate``. Geben Sie dieser Konfiguration einen neuen Namen, z.B. ``ServiceFabricUpgrader``.
3. Ändern Sie im rechten Bereich auf der Registerkarte ``Arguments`` den Text ``-Pconfig='deploy'`` in ``-Pconfig=upgrade``, und klicken Sie auf ``Apply``.
4. Sie haben jetzt eine Ausführungskonfiguration zum Aktualisieren Ihrer Anwendung erstellt und gespeichert, die Sie bei Bedarf mit ``Run`` ausführen können. Bei diesem Vorgang wird dann die aktuelle Version mit aktualisiertem Anwendungstyp aus der Anwendungsmanifestdatei abgerufen.

Sie können die Anwendungsaktualisierung nun mit Service Fabric Explorer überwachen. Die Aktualisierung der Anwendung dauert nur wenige Minuten.

<!-- Images -->

[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png

[create-application/p1]:./media/service-fabric-get-started-eclipse/create-application/p1.png
[create-application/p2]:./media/service-fabric-get-started-eclipse/create-application/p2.png
[create-application/p3]:./media/service-fabric-get-started-eclipse/create-application/p3.png
[create-application/p4]:./media/service-fabric-get-started-eclipse/create-application/p4.png
[create-application/p5]:./media/service-fabric-get-started-eclipse/create-application/p5.png
[create-application/p6]:./media/service-fabric-get-started-eclipse/create-application/p6.png

[publish/Publish]: ./media/service-fabric-get-started-eclipse/publish/Publish.png
[publish/RightClick]: ./media/service-fabric-get-started-eclipse/publish/RightClick.png

[add-service/p1]: ./media/service-fabric-get-started-eclipse/add-service/p1.png
[add-service/p2]: ./media/service-fabric-get-started-eclipse/add-service/p2.png
[add-service/p3]: ./media/service-fabric-get-started-eclipse/add-service/p3.png
[add-service/p4]: ./media/service-fabric-get-started-eclipse/add-service/p4.png

<!-- Links -->
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

