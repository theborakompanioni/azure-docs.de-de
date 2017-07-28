---
title: "Veröffentlichen einer Spring Boot-App als Docker-Container mit dem Azure-Toolkit für Eclipse | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie mit dem Azure-Toolkit für Eclipse eine Web-App in Microsoft Azure als Docker-Container veröffentlichen."
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
ms.openlocfilehash: 36d8233b64144d87501cd4d4a9cac539ab143d38
ms.contentlocale: de-de
ms.lasthandoff: 06/23/2017


---

# <a name="publish-a-spring-boot-app-as-a-docker-container-using-the-azure-toolkit-for-eclipse"></a>Veröffentlichen einer Spring Boot-App als Docker-Container mit dem Azure-Toolkit für Eclipse

Das **[Spring Framework]** ist eine Open Source-Lösung, die Java-Entwicklern beim Erstellen von Anwendungen auf Unternehmensebene hilft. Eines der gängigsten Projekte, das auf dieser Plattform aufbaut, ist [Spring Boot]. Es bietet einen vereinfachten Ansatz für das Erstellen eigenständiger Java-Anwendungen.

**[Docker]** ist eine Open Source-Lösung, die Entwicklern beim Automatisieren der Bereitstellung, Skalierung und Verwaltung ihrer Anwendungen hilft, die in Containern ausgeführt werden.

Dieses Tutorial führt Sie durch die Schritte zum Bereitstellen einer Spring Boot-Anwendung als Docker-Container in Microsoft Azure mithilfe des Azure-Toolkits für Eclipse.

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="cloning-the-default-spring-boot-docker-app-repository"></a>Klonen des Standard-Docker-App-Repositorys von Spring Boot

### <a name="importing-the-public-repository"></a>Importieren des öffentlichen Repositorys

Die folgenden Schritte führen Sie durch den Klonprozess des Docker-Repositorys von Spring Boot auf Ihrem lokalen Computer mithilfe von IntelliJ. Wenn Sie eine Befehlszeile verwenden möchten, finden Sie weitere Informationen unter [Bereitstellen einer Spring Boot-Anwendung unter Linux in Azure Container Service][Deploy Spring Boot on Linux in ACS].

1. Öffnen Sie Eclipse.

1. Klicken Sie auf **Datei** und anschließend auf **Importieren**.

   ![Menü „Datei importieren“][CL01]

1. Wenn das Dialogfeld **Importieren** geöffnet wird:

   a. Erweitern Sie **Git**.

   b. Markieren Sie **Projekte aus Git**.
   
   c. Klicken Sie auf **Weiter**.


   ![Dialogfeld „Importieren“][CL02]

1. Auf der Seite **Repositoryquelle auswählen**:

   a. Markieren Sie **URI klonen**.
   
   b. Klicken Sie auf **Weiter**.


   ![Klicken Sie auf „Repositoryquelle“][CL03]

1. Auf der Seite **Quelle Git-Repository**:

   a. Geben Sie `https://github.com/spring-guides/gs-spring-boot-docker.git` für den **URI** des Repositorys ein. Dadurch sollten die Felder **Host** und **Repositorypfad** automatisch mit den korrekten Werten aufgefüllt werden.
   
   b. Da das Spring Boot-Repository öffentlich ist, müssen Sie nicht Ihren Git-Benutzernamen und das Kennwort eingeben.
   
   c. Klicken Sie auf **Weiter**.


   ![Quelle Git-Repository][CL04]

1. Klicken Sie auf der Seite **Verzweigung** auf **Weiter**.

   ![Verzweigung][CL05]

1. Auf der Seite **Lokales Ziel**:

   a. Geben Sie den lokalen Ordner an, in dem Ihr lokales Repository angelegt werden soll.
   
   b. Klicken Sie auf **Weiter**.


   ![Lokales Ziel][CL06]

1. Auf der Seite **Assistent zum Importieren von Projekten auswählen**:

   a. Wählen Sie **Als allgemeines Projekt importieren** aus.
   
   b. Klicken Sie auf **Weiter**.


   ![Lokales Ziel][CL07]

1. Auf der Seite **Projekte importieren**:

   a. Geben Sie Ihren **Projektnamen** an.
   
   b. Klicken Sie auf **Fertig stellen**.

   ![Projekte importieren][CL08]

1. Nachdem das Repository erfolgreich geklont wurde, wird eine Liste aller Dateien in Eclipse angezeigt.

   ![Lokales Repository][CL09]

### <a name="creating-a-maven-project-from-your-local-repository"></a>Erstellen eines Maven-Projekts aus einem lokalen Repository

Das Docker-Repository von Spring Boot enthält ein abgeschlossenes Maven-Projekt, das Sie für dieses Tutorial verwenden können. 

1. Klicken Sie auf **Datei** und anschließend auf **Importieren**.

   ![Menü „Datei importieren“][CL01]

1. Wenn das Dialogfeld **Importieren** geöffnet wird:

   a. Erweitern Sie **Maven**.
   
   b. Markieren Sie **Vorhandene Maven-Projekte**.
   
   c. Klicken Sie auf **Weiter**.


   ![Dialogfeld „Importieren“][MV01]

1. Auf der Seite **Maven-Projekte**:

   a. Geben Sie in Ihrem lokalen Repository den `complete`-Ordner für das **Stammverzeichnis** an.
   
   b. Erweitern Sie den Abschnitt **Erweitert**, und geben Sie einen benutzerdefinierten Namen für die **Namensvorlage** ein.
   
   c. Aktivieren Sie innerhalb des Projekts das Kontrollkästchen für die `pom.xml`-Datei.
   
   d. Klicken Sie auf **Fertig stellen**.

   ![Maven-Projekte][MV02]

1. Nachdem das Maven-Projekt erfolgreich geöffnet wurde, wird ein zweites Projekt in Eclipse aufgelistet.

   ![Lokales Maven-Projekt][MV03]

## <a name="building-your-spring-boot-app-with-maven"></a>Erstellen einer Spring Boot-App mit Maven

1. Markieren Sie im Projekt-Explorer von Eclipse das Maven-Projekt.

1. Klicken Sie auf **Ausführen**, auf **Ausführen als** und dann auf **Maven-Build**.

   ![Ausführen als Maven-Build][BU01]

1. Nachdem die Anwendung erfolgreich erstellt wurde, wird im Konsolenfenster der Status aufgelistet.

   ![Erfolgreicher Maven-Build][BU02]

## <a name="publishing-your-web-app-to-azure-using-a-docker-container"></a>Veröffentlichen Ihrer Web-App in Azure mithilfe eines Docker-Containers

1. Markieren Sie im Projekt-Explorer von Eclipse das Maven-Projekt.

1. Klicken Sie auf das Azure-Menü **Veröffentlichen**, und klicken Sie anschließend auf **Als Docker-Container veröffentlichen**.

   ![Als Docker-Container veröffentlichen][PU01]

1. Wenn das Dialogfeld **Docker-Container in Azure bereitstellen** angezeigt wird:

   a. Geben Sie einen benutzerdefinierten **Docker-Imagenamen** ein.
   
   b. Geben Sie für das **Bereitzustellende Artefakt** den Pfad zur `gs-spring-boot-docker-0.1.0.jar`-Datei an, die Sie soeben erstellt haben.

   ![Angeben von Docker-Optionen][PU02]

   Alle vorhandenen Docker-Hosts werden angezeigt. Wenn Sie in einem vorhandenen Host bereitstellen möchten, fahren Sie mit Schritt 4 fort. Andernfalls gehen Sie folgendermaßen vor, um einen neuen Host zu erstellen:

   a. Klicken Sie auf **Hinzufügen**.

      ![Hinzufügen eines neuen Docker-Hosts][PU03]

   b. Wenn das Dialogfeld **Erstellen eines Docker-Hosts** angezeigt wird, können Sie wahlweise die Standardeinstellungen übernehmen oder beliebige benutzerdefinierte Einstellungen für den neuen Docker-Host angeben. (Eine ausführliche Beschreibung der verschiedenen Einstellungen finden Sie im Artikel [Veröffentlichen einer Web-App als Docker-Container mit dem Azure-Toolkit für IntelliJ][Publish Container with Azure Toolkit].) Nachdem Sie die Einstellungen angegeben haben, die Sie verwenden möchten, klicken Sie auf **Weiter**.

      ![Angeben von Docker-Hostoptionen][PU04]

   c. Sie können wahlweise vorhandene Anmeldeinformationen aus einem Azure-Schlüsseltresor verwenden oder neue Docker-Anmeldeinformationen eingeben. Nachdem Sie Ihre Optionen angegeben haben, klicken Sie auf **Fertig stellen**.

      ![Angeben von Docker-Hostanmeldeinformationen][PU05]

1. Markieren Sie Ihren Docker-Host, und klicken Sie dann auf **Weiter**.

   ![Auswählen des Docker-Hosts, der verwendet werden soll][PU06]

1. Auf der letzten Seite des Dialogfelds **Docker-Container in Azure bereitstellen** müssen Sie die folgenden Optionen angeben:

   a. Sie können wahlweise einen benutzerdefinierten Namen für den Container angeben, der Ihren Docker-Container hostet, oder Sie können die Standardeinstellung übernehmen.

   b. Sie müssen die TCP-Ports für Ihren Docker-Host mithilfe der folgenden Syntax eingeben: „*[externer Port]*:*[interner Port]*“. „80:8080“ gibt z.B. einen externen Port „80“ und den internen Spring Boot-Standardport „8080“ an.
   
      Wenn Sie den internen Port angepasst haben (z.B. durch Bearbeiten der Datei *application.yml*), müssen Sie die Portnummer für das richtige Routing in Azure angeben.

   c. Nachdem Sie diese Optionen konfiguriert haben, klicken Sie auf **Fertig stellen**.

   ![Bereitstellen des Docker-Containers in Azure][PU07]

1. Nachdem die Veröffentlichung des Azure-Toolkits abgeschlossen ist, zeigt das Azure-Aktivitätsprotokoll den Status **Veröffentlicht** an.

   ![Docker-Host erfolgreich bereitgestellt][PU08]

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->

[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Deploy Spring Boot on Linux in ACS]: ./container-service/container-service-deploy-spring-boot-app-on-linux.md
[Docker]: https://www.docker.com/
[Publish Container with Azure Toolkit]: ./azure-toolkit-for-intellij-publish-as-docker-container.md
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[CL01]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL01.png
[CL02]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL02.png
[CL03]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL03.png
[CL04]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL04.png
[CL05]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL05.png
[CL06]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL06.png
[CL07]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL07.png
[CL08]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL08.png
[CL09]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL09.png

[MV01]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/MV01.png
[MV02]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/MV02.png
[MV03]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/MV03.png

[BU01]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/BU01.png
[BU02]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/BU02.png

[PU01]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU01.png
[PU02]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU02.png
[PU03]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU03.png
[PU04]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU04.png
[PU05]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU05.png
[PU06]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU06.png
[PU07]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU07.png
[PU08]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU08.png

