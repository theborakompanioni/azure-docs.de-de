---
title: "Veröffentlichen einer Spring Boot-App als Docker-Container mit dem Azure-Toolkit für IntelliJ | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie mit dem Azure-Toolkit für IntelliJ eine Web-App in Microsoft Azure als Docker-Container veröffentlichen."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 06/21/2017
ms.author: robmcm
ms.translationtype: Human Translation
ms.sourcegitcommit: 8be2bcb9179e9af0957fcee69680ac803fd3d918
ms.openlocfilehash: 8d1f0f0f95647c85b2b5f3550e306e3ce35e81c3
ms.contentlocale: de-de
ms.lasthandoff: 06/23/2017


---

# <a name="publish-a-spring-boot-app-as-a-docker-container-using-the-azure-toolkit-for-intellij"></a>Veröffentlichen einer Spring Boot-App als Docker-Container mit dem Azure-Toolkit für IntelliJ

Das **[Spring-Framework]** ist eine Open-Source-Lösung, die Java-Entwickler beim Erstellen von Anwendungen auf Unternehmensebene unterstützt. Eines der gängigsten Projekte, das auf dieser Plattform aufbaut, ist [Spring Boot]. Es bietet einen vereinfachten Ansatz für das Erstellen eigenständiger Java-Anwendungen.

**[Docker]** ist eine Open Source-Lösung, die Entwicklern beim Automatisieren der Bereitstellung, Skalieren und Verwalten ihrer Anwendungen hilft, die in Containern ausgeführt werden.

Dieses Tutorial führt Sie durch die Schritte zum Bereitstellen einer Spring Boot-Anwendung als Docker-Container in Microsoft Azure mithilfe des Azure Toolkits für IntelliJ.

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="cloning-the-default-spring-boot-docker-app-repo"></a>Klonen des Spring Boot-Docker-App-Repositorys

Die folgenden Schritte führen Sie durch den Klonprozess des Spring Boot-Docker-Repositorys mithilfe von IntelliJ. Wenn Sie eine Befehlszeile verwenden möchten, finden Sie weitere Informationen unter [Bereitstellen einer Spring Boot-Anwendung unter Linux in Azure Container Service][Deploy Spring Boot on Linux in ACS].

1. Öffnen Sie IntelliJ.

1. Wählen Sie auf der Willkommensseite die Option **GitHub** im Dropdownmenü **Check out from version control** (Von Versionskontrolle auschecken) aus.

   ![Von Versionskontrolle auschecken][CL01]

1. Geben Sie Ihre Anmeldeinformationen ein, wenn Sie dazu aufgefordert werden.

   * Wenn Sie sich mit Benutzername/Kennwort bei GitHub anmelden:

      ![Geben Sie Ihre Daten im Dialogfeld „GitHub-Anmeldeinformationen“ ein][CL02a]

   * Wenn Sie sich per Token bei GitHub anmelden:

      ![Geben Sie Ihre Daten im Dialogfeld „GitHub-Anmeldeinformationen“ ein][CL02b]

1. Geben Sie `https://github.com/spring-guides/gs-spring-boot-docker.git` für die URL des Repositorys ein, geben Sie Ihren lokalen Pfad und Ordnerinformationen an, und klicken Sie auf **Klonen**.

   ![Dialogfeld „Repository klonen“][CL03]

1. Wählen Sie **Nein** aus, wenn Sie aufgefordert werden, ein IntelliJ-Projekt zu erstellen.

   ![Erstellen eines IntelliJ-Projekts][CL04]

1. Klicken Sie auf der Willkommensseite auf **Projekt importieren**.

   ![Importieren des Projekts][CL05]

1. Suchen Sie den Pfad, in den Sie das Spring Boot-Repository geklont haben, markieren Sie den **vollständigen** Ordner unter dem Stammordner, und klicken Sie auf **OK**.

   ![Importieren des Projekts][CL06]

1. Wählen Sie **Create project from existing sources** (Projekt aus vorhandenen Quellen erstellen) aus, wenn Sie dazu aufgefordert werden.

   ![Projekt aus vorhandenen Quellen erstellen][CL07]

1. Geben Sie einen Projektnamen ein, oder übernehmen Sie den Standardnamen, überprüfen Sie den korrekten Pfad für den **vollständigen** Ordner, und klicken Sie auf **Weiter**.

   ![Angeben des Projektnamens][CL08]

1. Passen Sie Verzeichnisse für den Import an, und klicken Sie auf **Weiter**.

   ![Auswahl von Verzeichnissen][CL09]

1. Überprüfen Sie die zu importierenden Bibliotheken, und klicken Sie dann auf **Weiter**.

   ![Überprüfen der Projektbibliotheken][CL10]

1. Überprüfen Sie die Modulstruktur, und klicken Sie dann auf **Weiter**.

   ![Überprüfen der Modulstruktur][CL11]

1. Geben Sie Ihr JDK, an und klicken Sie dann auf **Weiter**.

   ![JDK angeben][CL12]

1. Klicken Sie auf **Fertig stellen**.

   ![Finish][CL13]

1. IntilliJ importiert die Spring Boot-App als Projekt und zeigt die Struktur an, sobald der Import abgeschlossen ist.

   ![Spring Boot-App in IntelliJ][CL14]

## <a name="building-your-spring-boot-app"></a>Erstellen einer Spring Boot-App

### <a name="build-the-app-using-the-maven-pom"></a>Erstellen der App mithilfe der Maven-POM-Datei

1. Öffnen Sie, falls noch nicht geschehen, das Maven-Toolfenster. Klicken Sie hierfür auf **Anzeige**, **Toolfenster** und anschließend auf **Maven Projects** (Maven-Projekte).

   ![Ansicht Maven-Toolfenster][BU01]

1. Klicken Sie im Maven-Toolfenster mit der rechten Maustaste auf **Paket**, und wählen Sie **Run Maven Build** (Maven-Build ausführen) aus. (Wenn das Maven-Projekt nicht automatisch angezeigt wird, müssen Sie möglicherweise auf das Symbol **Reimport** (Erneut importieren) auf der Maven-Symbolleiste klicken.)

   ![Ausführen des Maven-Build][BU02]

1. IntelliJ sollte die Meldung BUILD SUCCESS (Builderfolg) anzeigen, wenn Ihre Spring Boot-App erfolgreich erstellt wurde.

   ![Builderfolg][BU03]

### <a name="create-a-deployment-ready-artifact"></a>Erstellen eines bereitstellungsfähigen Artefakts

Um Ihre Spring Boot-App zu veröffentlichen, müssen Sie ein bereitstellungsfähiges Artefakt erstellen. Führen Sie dazu die folgenden Schritte aus:

1. Öffnen Sie Ihr Web-App-Projekt in IntelliJ.

1. Klicken Sie auf **Datei** und dann auf **Projektstruktur**.

   ![Menü „Projektstruktur“][ART01]

1. Klicken Sie auf das grüne Pluszeichen („**+**“), um ein Artefakt hinzuzufügen, klicken Sie auf **JAR** und anschließend auf **Leer**.

   ![Artefakt hinzufügen][ART02]

1. Benennen Sie Ihr Artefakt und stellen Sie dabei sicher, dass Sie nicht die Erweiterung „.jar“ anfügen. Geben Sie anschließend den Zielordner für die Maven-Ausgabe an.

   ![Angeben der Eigenschaften des Artefakts][ART03]

1. OPTIONAL: Erstellen eines Manifests für Ihr Artefakt:

   a. Klicken Sie auf **Create Manifest** (Manifest erstellen).

      ![Angeben des Pfads des Artefakts][ART04a]

   b. Wählen Sie den Standardpfad für das Artefakt aus, und klicken Sie dann auf **OK**.

      ![Angeben des Pfads des Artefakts][ART04b]

   c. Klicken Sie auf die Auslassungspunkte **...**, um die Hauptklasse anzugeben.

      ![Suchen der Hauptklasse][ART04c]

   d. Wählen Sie die Hauptklasse aus, und klicken Sie anschließend auf **OK**.

      ![Angeben der Hauptklasse][ART04d]

1. Klicken Sie auf **OK**.

   ![Dialogfeld „Eigenschaften des Artefakts“ schließen][ART05]

> [!NOTE]
>
> Weitere Informationen zum Erstellen von Artefakten in IntelliJ finden Sie auf der JetBrains-Website unter [Konfigurieren von Artefakten].
>

### <a name="build-the-artifact-for-deployment"></a>Erstellen des Artefakts für die Bereitstellung

1. Klicken Sie auf **Erstellen** und anschließend auf **Artefakte**.

   ![Menü „Artefakt erstellen“][BU04]

1. Klicken Sie auf **Erstellen**, wenn das Kontextmenü **Build Artifact** (Artefakt erstellen) angezeigt wird.

   ![Kontextmenü „Artefakt erstellen“][BU05]

1. IntelliJ sollte das vollständige Artefakt für Ihre Spring Boot-App im Toolfenster „Projekt“ anzeigen.

   ![Erstelltes Artefakt][BU06]

## <a name="publishing-your-web-app-to-azure-using-a-docker-container"></a>Veröffentlichen Ihrer Web-App in Azure mithilfe eines Docker-Containers

1. Falls noch nicht geschehen, melden Sie sich beim Azure-Konto gemäß den Anweisungen im Artikel [Anleitung zur Anmeldung für das Azure-Toolkit für IntelliJ][Azure Sign In for IntelliJ] an.

1. Klicken Sie im Toolfenster „Project Explorer“ mit der rechten Maustaste auf das Projekt, und wählen Sie dann **Azure** > **Publish as Docker Container** (Als Docker-Container veröffentlichen) aus.

   ![Als Docker-Container veröffentlichen][PU01]

1. Wenn das Dialogfeld **Deploy Docker Container on Azure** (Bereitstellen des Docker-Containers in Azure) angezeigt wird, werden alle vorhandenen Docker-Hosts angezeigt. Wenn Sie in einem vorhandenen Host bereitstellen möchten, fahren Sie mit Schritt 4 fort. Gehen Sie andernfalls folgendermaßen vor, um einen neuen Host zu erstellen:

   a. Klicken Sie auf das grüne Plussymbol („**+**“).

      ![Hinzufügen eines neuen Docker-Hosts][PU02]

   b. Wenn das Dialogfeld **Erstellen eines Docker-Hosts** angezeigt wird, können Sie wahlweise die Standardeinstellungen übernehmen oder beliebige benutzerdefinierte Einstellungen für den neuen Docker-Host angeben. (Eine ausführliche Beschreibung der verschiedenen Einstellungen finden Sie im Artikel [Veröffentlichen einer Web-App als Docker-Container mit dem Azure-Toolkit für IntelliJ][Publish Container with Azure Toolkit].) Klicken Sie auf **Weiter**, nachdem Sie die gewünschten Einstellungen eingegeben haben.

      ![Angeben von Docker-Hostoptionen][PU03a]

   c. Sie können wahlweise vorhandene Anmeldeinformationen aus einem Azure-Schlüsseltresor verwenden oder neue Docker-Anmeldeinformationen eingeben. Klicken Sie auf **Fertig stellen**, nachdem Sie Ihre Optionen angegeben haben.

      ![Angeben von Docker-Hostanmeldeinformationen][PU03b]

1. Markieren Sie Ihren Docker-Host, und klicken Sie dann auf **Weiter**.

   ![Auswählen des Docker-Hosts, der verwendet werden soll][PU04]

1. Auf der letzten Seite des Dialogfelds **Deploy Docker Container on Azure** (Docker-Container in Azure bereitstellen) müssen Sie die folgenden Optionen angeben:

   a. Sie können wahlweise einen benutzerdefinierten Namen für den Container angeben, der Ihren Docker-Container hostet, oder Sie können die Standardeinstellung übernehmen.

   b. Sie müssen die TCP-Ports für Ihren Docker-Host mithilfe der folgenden Syntax eingeben: „*[externer Port]*:*[interner Port]*“. „80:8080“ gibt z.B. einen externen Port „80“ und den internen Spring Boot-Standardport „8080“ an.
   
      Wenn Sie den internen Port angepasst haben (z.B. durch Bearbeiten der Datei *application.yml*), müssen Sie die Portnummer für das richtige Routing in Azure angeben.

   c. Nachdem Sie diese Optionen konfiguriert haben, klicken Sie auf **Fertig stellen**.

   ![Bereitstellen des Docker-Containers in Azure][PU05]

1. Nachdem die Veröffentlichung des Azure-Toolkits abgeschlossen ist, zeigt das Azure-Aktivitätsprotokoll den Status **Veröffentlicht** an.

   ![Docker-Host erfolgreich bereitgestellt][PU06]

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

Auf der JetBrain-Website finden Sie unter [Creating Spring Boot Projects (Erstellen von Spring Boot-Projekten)](https://www.jetbrains.com/help/idea/creating-spring-boot-projects.html) Informationen zu weiteren Methoden zum Erstellen von Spring Boot-Anwendungen mithilfe von IntelliJ.

<!-- URL List -->

[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Sign In for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Konfigurieren von Artefakten]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html
[Deploy Spring Boot on Linux in ACS]: ./container-service/container-service-deploy-spring-boot-app-on-linux.md
[Docker]: https://www.docker.com/
[Publish Container with Azure Toolkit]: ./azure-toolkit-for-intellij-publish-as-docker-container.md
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring-Framework]: https://spring.io/

<!-- IMG List -->

[CL01]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL01.png
[CL02a]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL02a.png
[CL02b]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL02b.png
[CL03]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL03.png
[CL04]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL04.png
[CL05]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL05.png
[CL06]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL06.png
[CL07]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL07.png
[CL08]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL08.png
[CL09]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL09.png
[CL10]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL10.png
[CL11]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL11.png
[CL12]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL12.png
[CL13]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL13.png
[CL14]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL14.png

[ART01]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART01.png
[ART02]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART02.png
[ART03]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART03.png
[ART04a]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04a.png
[ART04b]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04b.png
[ART04c]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04c.png
[ART04d]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04d.png
[ART05]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART05.png

[BU01]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU01.png
[BU02]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU02.png
[BU03]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU03.png
[BU04]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU04.png
[BU05]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU05.png
[BU06]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU06.png

[PU01]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU01.png
[PU02]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU02.png
[PU03a]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU03a.png
[PU03b]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU03b.png
[PU04]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU04.png
[PU05]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU05.png
[PU06]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU06.png

