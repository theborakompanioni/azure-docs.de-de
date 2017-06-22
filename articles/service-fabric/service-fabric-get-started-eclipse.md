--- 
title: "Azure Service Fabric-Plug-In für Eclipse | Microsoft-Dokumentation"
description: "Enthält eine Beschreibung der ersten Schritte mit dem Service Fabric-Plug-In für Eclipse."
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
ms.date: 05/03/2016
ms.author: saysa
ms.translationtype: Human Translation
ms.sourcegitcommit: c785ad8dbfa427d69501f5f142ef40a2d3530f9e
ms.openlocfilehash: 9cc621380a6b22a6b1da617d76c56e4773bb3dde
ms.contentlocale: de-de
ms.lasthandoff: 05/26/2017


---

# <a name="service-fabric-plug-in-for-eclipse-java-application-development"></a>Service Fabric-Plug-In für die Entwicklung von Eclipse-Java-Anwendungen
Eclipse ist eine der am häufigsten genutzten IDEs (Integrated Development Environments) für Java-Entwickler. In diesem Artikel wird beschrieben, wie Sie Ihre Eclipse-Entwicklungsumgebung für Azure Service Fabric einrichten. Sie erfahren, wie Sie das Service Fabric-Plug-In installieren, eine Service Fabric-Anwendung erstellen und Ihre Service Fabric-Anwendung in einem lokalen Service Fabric-Cluster oder Service Fabric-Remotecluster in Eclipse Neon bereitstellen.

## <a name="install-or-update-the-service-fabric-plug-in-in-eclipse-neon"></a>Installieren oder Aktualisieren des Service Fabric-Plug-Ins in Eclipse Neon
Sie können ein Service Fabric-Plug-In in Eclipse installieren. Das Plug-In kann dazu beitragen, den Prozess der Erstellung und Bereitstellung von Java-Diensten zu vereinfachen.

1.  Stellen Sie sicher, dass Sie die aktuelle Version von Eclipse Neon und die aktuelle Version von Buildship (1.0.17 oder höher) installiert haben:
    -   In Eclipse Neon können Sie unter **Hilfe** > **Installationsdetails** die Versionen der installierten Komponenten überprüfen.
    -   Informationen zur Aktualisierung von Buildship finden Sie unter [Eclipse Buildship: Eclipse Plug-ins for Gradle][buildship-update] (Eclipse Buildship: Eclipse-Plug-Ins für Gradle).
    -   Unter **Hilfe** > **Nach Updates suchen** können Sie nach Updates für Eclipse Neon suchen und diese installieren.

2.  Die Installation des Service Fabric-Plug-Ins können Sie unter **Hilfe** > **Install New Software** (Neue Software installieren) durchführen.
  1.    Geben Sie im Feld **Work with** (Verwenden von) die URL **http://dl.microsoft.com/eclipse** ein.
  2.    Klicken Sie auf **Hinzufügen**.

         ![Service Fabric-Plug-In für Eclipse Neon][sf-eclipse-plugin-install] 
  3.    Wählen Sie das Service Fabric-Plug-In aus, und klicken Sie auf **Weiter**.
  4.    Führen Sie die Schritte für die Installation aus, und akzeptieren Sie die Microsoft-Software-Lizenzbedingungen.

Falls Sie das Service Fabric-Plug-In bereits installiert haben, sollten Sie sicherstellen, dass Sie über die aktuelle Version verfügen. Unter **Hilfe** > **Installationsdetails** können Sie prüfen, ob Updates verfügbar sind. Wählen Sie in der Liste mit den installierten Plug-Ins die Option „Service Fabric“, und klicken Sie auf **Aktualisieren**. Die verfügbaren Updates werden installiert.

> [!NOTE]
> Wenn das Installieren oder Aktualisieren des Service Fabric-Plug-Ins lange dauert, kann dies an einer Eclipse-Einstellung liegen. In Eclipse werden Metadaten zu allen Änderungen von Updatewebsites gesammelt, die für Ihre Eclipse-Instanz registriert sind. Navigieren Sie zu **Available Software Sites** (Verfügbare Softwarestandorte), um das Vorhandensein eines Updates für das Service Fabric-Plug-In zu prüfen und das Update zu installieren. Deaktivieren Sie die Kontrollkästchen für alle Websites, mit Ausnahme des Kontrollkästchens, das auf den Speicherort des Service Fabric-Plug-Ins (http://dl.microsoft.com/eclipse/azure/servicefabric) verweist.

## <a name="create-a-service-fabric-application-in-eclipse"></a>Erstellen einer Service Fabric-Anwendung in Eclipse

1.  Navigieren Sie in Eclipse Neon zu **Datei** > **Neu** > **Other** (Andere). Wählen Sie die Option **Service Fabric-Projekt**, und klicken Sie dann auf **Weiter**.

    ![Service Fabric – Neues Projekt – Seite 1][create-application/p1]

2.  Geben Sie einen Namen für Ihr Projekt ein, und klicken Sie auf **Weiter**.

    ![Service Fabric – Neues Projekt – Seite 2][create-application/p2]

3.  Wählen Sie in der Liste mit den Vorlagen die Option **Dienstvorlage**. Wählen Sie den Typ der Dienstvorlage (Akteur, Zustandslos, Container oder Binäre Gastdatei), und klicken Sie auf **Weiter**.

    ![Service Fabric – Neues Projekt – Seite 3][create-application/p3]

4.  Geben Sie den Dienstnamen und die Dienstdetails ein, und klicken Sie auf **Fertig stellen**.

    ![Service Fabric – Neues Projekt – Seite 4][create-application/p4]

5. Klicken Sie beim Erstellen Ihres ersten Service Fabric-Projekts im Dialogfeld **Open Associated Perspective** (Zugeordnete Perspektive öffnen) auf **Ja**.

    ![Service Fabric – Neues Projekt – Seite 5][create-application/p5]

6.  Das neue Projekt sieht wie folgt aus:

    ![Service Fabric – Neues Projekt – Seite 6][create-application/p6]

## <a name="build-and-deploy-a-service-fabric-application-in-eclipse"></a>Erstellen und Bereitstellen einer Service Fabric-Anwendung in Eclipse

1.  Klicken Sie mit der rechten Maustaste auf Ihre neue Service Fabric-Anwendung, und wählen Sie dann die Option **Service Fabric**.

    ![Service Fabric-Kontextmenü][publish/RightClick]

2. Wählen Sie im Untermenü die gewünschte Option:
    -   Klicken Sie auf **Build Application** (Anwendung erstellen), um die Anwendung ohne Bereinigung zu erstellen.
    -   Klicken Sie auf **Rebuild Application** (Anwendung erneut erstellen), um eine bereinigte Erstellung der Anwendung durchzuführen.
    -   Klicken Sie auf **Clean Application** (Anwendung bereinigen), um die Anwendung in Bezug auf die Buildartefakte zu bereinigen.

3.  Über dieses Menü können Sie Ihre Anwendung auch bereitstellen und veröffentlichen und die Bereitstellung aufheben:
    -   Klicken Sie auf **Deploy Application** (Anwendung bereitstellen), um die Anwendung im lokalen Cluster bereitzustellen.
    -   Wählen Sie im Dialogfeld **Publish Application** (Anwendung veröffentlichen) ein Veröffentlichungsprofil:
        -  **Local.json**
        -  **Cloud.json**

     In diesen JSON-Dateien (JavaScript Object Notation) werden Informationen gespeichert (z.B. Verbindungsendpunkte und Sicherheitsinformationen), die erforderlich sind, um eine Verbindung mit Ihrem lokalen Cluster oder Cluster in der Cloud (Azure) herzustellen.

  ![Menü „Veröffentlichen“ von Service Fabric][publish/Publish]

Eine andere Möglichkeit zum Bereitstellen Ihrer Service Fabric-Anwendung ist die Verwendung von Eclipse-Ausführungskonfigurationen.

  1.    Navigieren Sie zu **Ausführen** > **Run Configurations** (Ausführungskonfigurationen).
  2.    Wählen Sie unter **Gradle Project** (Gradle-Projekt) die Ausführungskonfiguration **ServiceFabricDeployer** aus.
  3.    Wählen Sie im rechten Bereich auf der Registerkarte**Argumente** für **publishProfile** die Option **local** oder **cloud**.  Der Standardwert lautet **local**. Wählen Sie zur Bereitstellung für einen Remote- oder Cloudcluster die Option **cloud**.
  4.    Um sicherzustellen, dass die richtigen Informationen in die Veröffentlichungsprofile eingefügt werden, können Sie **Local.json** bzw. **Cloud.json** wie gewünscht bearbeiten. Sie können Endpunktdetails und Sicherheitsanmeldeinformationen hinzufügen oder aktualisieren.
  5.    Stellen Sie sicher, dass für **Arbeitsverzeichnis** auf die Anwendung verwiesen wird, die Sie bereitstellen möchten. Klicken Sie zum Ändern der Anwendung auf die Schaltfläche **Arbeitsbereich**, und wählen Sie die gewünschte Anwendung aus.
  6.    Klicken Sie auf **Übernehmen** und anschließend auf **Ausführen**.

Ihre Anwendung wird innerhalb weniger Augenblicke erstellt und bereitgestellt. Sie können den Bereitstellungsstatus in Service Fabric Explorer überwachen.  

## <a name="add-a-service-fabric-service-to-your-service-fabric-application"></a>Hinzufügen eines Service Fabric-Diensts zur Service Fabric-Anwendung

Führen Sie die folgenden Schritte aus, um einer vorhandenen Service Fabric-Anwendung einen Service Fabric-Dienst hinzuzufügen:

1.  Klicken Sie mit der rechten Maustaste auf das Projekt, dem Sie einen Dienst hinzufügen möchten, und klicken Sie anschließend auf **Service Fabric**.

    ![Service Fabric – Dienst hinzufügen – Seite 1][add-service/p1]

2.  Klicken Sie auf **Add Service Fabric Service** (Service Fabric-Dienst hinzufügen), und führen Sie die Schritte zum Hinzufügen eines Diensts zum Projekt aus.
3.  Wählen Sie die Dienstvorlage aus, die Sie dem Projekt hinzufügen möchten, und klicken Sie auf **Weiter**.

    ![Service Fabric – Dienst hinzufügen – Seite 2][add-service/p2]

4.  Geben Sie den Dienstnamen (und bei Bedarf weitere Details) ein, und klicken Sie anschließend auf die Schaltfläche **Dienst hinzufügen**.  

    ![Service Fabric – Dienst hinzufügen – Seite 3][add-service/p3]

5.  Nachdem der Dienst hinzugefügt wurde, sieht die Gesamtstruktur des Projekts in etwa wie im folgenden Projekt aus:

    ![Service Fabric – Dienst hinzufügen – Seite 4][add-service/p4]

## <a name="edit-manifest-versions-of-your-service-fabric-java-application"></a>Bearbeiten von Manifestversionen Ihrer Service Fabric-Java-Anwendung

Klicken Sie zum Bearbeiten von Manifestversionen mit der rechten Maustaste auf das Projekt, navigieren Sie zu **Service Fabric**, und wählen Sie in der Dropdownliste des Menüs die Option **Manifestversionen bearbeiten...**. Im Assistenten können Sie die Manifestversionen für das Anwendungsmanifest, das Dienstmanifest und die Versionen für die Pakete **Code**, **Config** und **Data** aktualisieren.

Wenn Sie die Option **Anwendung und Dienstversionen automatisch aktualisieren** aktivieren und dann eine Version aktualisieren, werden die Manifestversionen automatisch aktualisiert. Beispiel: Sie aktivieren zuerst das Kontrollkästchen und aktualisieren dann die Version von **Code** von 0.0.0 auf 0.0.1 und klicken auf **Fertig stellen**. Die Versionen des Dienstmanifests und des Anwendungsmanifests werden dann automatisch auf 0.0.1 aktualisiert.

## <a name="upgrade-your-service-fabric-java-application"></a>Aktualisieren der Service Fabric-Java-Anwendung

Angenommen, Sie haben für ein Upgradeszenario das Projekt **App1** erstellt, indem Sie das Service Fabric-Plug-In in Eclipse verwendet haben. Sie haben es bereitgestellt, indem Sie mit dem Plug-In eine Anwendung mit dem Namen **fabric:/App1Application** erstellt haben. Der Anwendungstyp lautet **App1ApplicationType**, und die Anwendungsversion ist 1.0. Nun möchten Sie die Anwendung aktualisieren, ohne die Verfügbarkeit zu unterbrechen.

Nehmen Sie zuerst Änderungen an Ihrer Anwendung vor, und erstellen Sie den geänderten Dienst anschließend neu. Aktualisieren Sie die Manifestdatei des geänderten Diensts (ServiceManifest.xml) mit den aktualisierten Versionen für den Dienst (und ggf. mit aktualisiertem Code, aktualisierter Konfiguration oder aktualisierten Daten). Passen Sie auch das Manifest der Anwendung (ApplicationManifest.xml) mit der aktualisierten Versionsnummer für die Anwendung und dem geänderten Dienst an.  

Zum Durchführen eines Upgrades für Ihre Anwendung mit Eclipse Neon können Sie ein dupliziertes Profil einer Ausführungskonfiguration erstellen. Verwenden Sie es anschließend, um die Anwendung wie gewünscht zu aktualisieren.

1.  Navigieren Sie zu **Ausführen** > **Run Configurations** (Ausführungskonfigurationen). Klicken Sie im linken Bereich auf den kleinen Pfeil links neben **Gradle Project** (Gradle-Projekt).
2.  Klicken Sie mit der rechten Maustaste auf **ServiceFabricDeployer**, und wählen Sie die Option **Duplizieren**. Geben Sie einen neuen Namen für diese Konfiguration ein, z.B. **ServiceFabricUpgrader**.
3.  Ändern Sie im rechten Bereich auf der Registerkarte **Argumente** den Wert **-Pconfig='deploy'** in **-Pconfig=upgrade**, und klicken Sie dann auf **Übernehmen**.

Mit diesem Prozess wird ein Profil für die Ausführungskonfiguration erstellt und gespeichert, das Sie jederzeit zum Aktualisieren Ihrer Anwendung verwenden können. Bei diesem Vorgang wird außerdem die aktuelle Version mit aktualisiertem Anwendungstyp aus der Anwendungsmanifestdatei abgerufen.

Das Upgrade der Anwendung dauert einige Minuten. Sie können die Anwendungsaktualisierung mit Service Fabric Explorer überwachen.

<!-- Images -->

[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-eclipse/service-fabric-eclipse-plugin.png

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

