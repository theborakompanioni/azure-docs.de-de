---
title: Bereitstellen einer Spring Boot-Webanwendung unter Linux in Azure Container Service | Microsoft-Dokumentation
description: "In diesem Tutorial werden die Schritte zum Bereitstellen einer Spring Boot-Anwendung als Linux-Webanwendung in Microsoft Azure erläutert."
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
ms.author: asirveda;robmcm
ms.translationtype: HT
ms.sourcegitcommit: f5c887487ab74934cb65f9f3fa512baeb5dcaf2f
ms.openlocfilehash: 6035b0e2fe27b46c441012144caf545eb17e1825
ms.contentlocale: de-de
ms.lasthandoff: 08/08/2017

---

# <a name="deploy-a-spring-boot-application-on-linux-in-azure-container-service"></a>Bereitstellen einer Spring Boot-Anwendung unter Linux in Azure Container Service

[Spring Framework] ist eine Open Source-Lösung, die Java-Entwickler bei der Erstellung professioneller Anwendungen unterstützt. Eines der gängigeren Projekte, das auf Java aufbaut, ist [Spring Boot]. Es bietet einen vereinfachten Ansatz für das Erstellen eigenständiger Java-Anwendungen.

[Docker] ist eine Open Source-Lösung, die Entwickler beim Automatisieren der Bereitstellung, Skalierung und Verwaltung ihrer in Containern ausgeführten Anwendungen unterstützt.

In diesem Tutorial wird die Verwendung von Docker zur Entwicklung und Bereitstellung einer Spring Boot-Anwendung für einen Linux-Host in [Azure Container Service] erläutert.

## <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen der Schritte in diesem Tutorial benötigen Sie Folgendes:

* Ein Azure-Abonnement. Wenn Sie noch kein Azure-Abonnement besitzen, können Sie Ihre [MSDN-Abonnentenvorteile] aktivieren oder sich für ein [kostenloses Azure-Konto] registrieren.
* Die [Azure-Befehlszeilenschnittstelle (CLI)]
* Ein aktuelles [Java Developer Kit (JDK)]
* Das Erstellungstool Apache [Maven] (Version 3)
* Einen [Git-Client]
* Einen [Docker]-Client

> [!NOTE]
>
> Aufgrund der Virtualisierungsanforderungen dieses Tutorials können Sie die Schritte in diesem Artikel nicht auf einem virtuellen Computer ausführen. Stattdessen müssen Sie einen physischen Computer mit aktivierten Virtualisierungsfunktionen verwenden.
>

## <a name="create-the-spring-boot-on-the-docker-getting-started-web-app"></a>Erstellen der Web-App für erste Schritte mit Spring Boot in Docker

Die folgenden Schritte unterstützen Sie beim Erstellen einer einfachen Spring Boot-Webanwendung und beim lokalen Testen.

1. Öffnen Sie eine Eingabeaufforderung. Erstellen Sie ein lokales Verzeichnis zum Speichern Ihrer Anwendung, und wechseln Sie in dieses Verzeichnis. Beispiel:
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   – oder –
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

2. Klonen Sie das Beispielprojekt [Erste Schritte mit Spring Boot in Docker] in das Verzeichnis, das Sie gerade erstellt haben. Beispiel:
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

3. Wechseln Sie in das Verzeichnis mit dem abgeschlossenen Projekt. Beispiel:
   ```
   cd gs-spring-boot-docker/complete
   ```

4. **Optionaler Schritt:** Wenn der eingebettete Tomcat-Server statt am Standardport 8080 an Port 80 ausgeführt werden soll (z.B. wenn Sie Ihr Spring Boot-Projekt lokal testen möchten), konfigurieren Sie den Port mithilfe der folgenden Schritte:

   a. Ändern Sie das Verzeichnis in das Ressourcenverzeichnis. Beispiel:
   ```
   cd src/main/resources
   ```

   b. Öffnen Sie die Datei **application.yml** in einem Texteditor.

   c. Ändern Sie die Einstellung **server:** so, dass der Server an Port 80 ausgeführt wird. Beispiel:
   ```
   server:
      port: 80
   ```

   d. Speichern und schließen Sie die Datei **application.yml**.

   e. Wechseln Sie wieder in das Verzeichnis mit dem Stammordner für das abgeschlossene Projekt. Beispiel:
   ```
   cd ../../..
   ```

5. Erstellen Sie die JAR-Datei mit Maven. Beispiel:
   ```
   mvn package
   ```

6. Wechseln Sie nach dem Erstellen der Web-App in das Verzeichnis `target` mit der JAR-Datei, und starten Sie die Web-App. Beispiel:
   ```
   cd target
   java -jar gs-spring-boot-docker-0.1.0.jar
   ```

7. Testen Sie die Web-App, indem Sie sie lokal in einem Webbrowser aufrufen. Sie können beispielsweise den folgenden Befehl verwenden, wenn Curl verfügbar ist und Sie den Tomcat-Servers für die Ausführung an Port 80 konfiguriert haben:
   ```
   curl http://localhost
   ```

8. Ihnen sollte die folgende Meldung angezeigt: **Hallo Docker-Welt!**

   ![Lokales Navigieren zur Beispiel-App][SB01]

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a>Erstellen einer Azure-Containerregistrierung, die als private Docker-Registrierung verwendet werden soll

Die folgenden Schritte führen Sie durch die Erstellung einer Azure-Containerregistrierung mithilfe des Azure-Portals.

> [!NOTE]
> Wenn Sie statt des Azure-Portals die Azure CLI verwenden möchten, führen Sie die Schritte unter [Erstellen einer privaten Docker-Containerregistrierung mit der Azure CLI 2.0][1] aus.

1. Melden Sie sich beim [Azure-Portal]an.

    Nachdem Sie sie bei Ihrem Konto im Azure-Portal angemeldet haben, führen Sie die Schritte unter [Erstellen einer privaten Docker-Containerregistrierung mit dem Azure-Portal] aus. Die Schritte aus diesem Artikel sind hier zusammengefasst:

2. Wählen Sie das Menüsymbol für **+ Neu**.

3. Wählen Sie **Container** und dann **Azure Container Registry**.

   ![Erstellen einer neuen Azure-Containerregistrierung][AR01]

4. Wenn die Informationsseite für die Vorlage der Azure-Containerregistrierung angezeigt wird, klicken Sie auf **Erstellen**.

   ![Erstellen einer neuen Azure-Containerregistrierung][AR02]

5. Wenn das Blatt **Containerregistrierung erstellen** angezeigt wird:

   a. Geben Sie **Registrierungsname** und **Ressourcengruppe** ein.  
   
   b. Wählen Sie **Aktivieren** für **Administratorbenutzer**.
   
   c. Klicken Sie auf **Erstellen**.

   ![Konfigurieren der Einstellungen für die Azure-Containerregistrierung][AR03]

6. Nachdem die Containerregistrierung erstellt wurde, rufen Sie sie im Azure-Portal auf. Wählen Sie dann **Zugriffsschlüssel**. Notieren Sie sich den Benutzernamen und das Kennwort für die nächsten Schritte.

   ![Zugriffsschlüssel für die Azure-Containerregistrierung][AR04]

## <a name="configure-maven-to-use-your-azure-container-registry-access-keys"></a>Konfigurieren von Maven für die Verwendung Ihrer Zugriffsschlüssel für die Azure-Containerregistrierung

1. Wechseln Sie zum Konfigurationsverzeichnis für Ihre Maven-Installation. Öffnen Sie anschließend die Datei **settings.xml** mit einem Text-Editor.

2. Fügen Sie Ihre Zugriffseinstellungen für die Azure-Containerregistrierung aus dem vorherigen Abschnitt dieses Tutorials zur Auflistung `<servers>` in der Datei **settings.xml** hinzu. Beispiel:

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

3. Navigieren Sie zum Verzeichnis des abgeschlossenen Projekts für Ihre Spring Boot-Anwendung (z.B. **C:\SpringBoot\gs-spring-boot-docker\complete** oder **/users/robert/SpringBoot/gs-spring-boot-docker/complete**). Öffnen Sie anschließend die Datei **pom.xml** mit einem Text-Editor.

1. Aktualisieren Sie die Auflistung `<properties>` in der Datei **pom.xml**. Verwenden Sie den Wert des Anmeldeservers für Ihre Azure-Containerregistrierung aus dem vorherigen Abschnitt dieses Tutorials. Beispiel:

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. Aktualisieren Sie die Auflistung `<plugins>` in der Datei **pom.xml** so, dass `<plugin>` die Adresse des Anmeldeservers und den Registrierungsnamen für Ihre Azure-Containerregistrierung aus dem vorherigen Abschnitt dieses Tutorials enthält. Beispiel:

   ```xml
   <plugin>
      <groupId>com.spotify</groupId>
      <artifactId>docker-maven-plugin</artifactId>
      <version>0.4.11</version>
      <configuration>
         <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         <dockerDirectory>src/main/docker</dockerDirectory>
         <resources>
            <resource>
               <targetPath>/</targetPath>
               <directory>${project.build.directory}</directory>
               <include>${project.build.finalName}.jar</include>
            </resource>
         </resources>
         <serverId>wingtiptoysregistry</serverId>
         <registryUrl>https://wingtiptoysregistry.azurecr.io</registryUrl>
      </configuration>
   </plugin>
   ```

1. Navigieren Sie zu dem Verzeichnis des abgeschlossenen Projekts für Ihre Spring Boot-Anwendung. Führen Sie dann den folgenden Befehl aus, um die Anwendung neu zu erstellen und den Container per Push in die Azure-Containerregistrierung zu übertragen:

   ```
   mvn package docker:build -DpushImage
   ```

> [!NOTE]
>
> Wenn Sie Ihren Docker-Container in Azure übertragen, erhalten Sie möglicherweise eine Fehlermeldung, auch wenn Ihr Docker-Container erfolgreich erstellt wurde. Sie ähnelt einer der folgenden Fehlermeldungen:
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: no basic auth credentials`
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: Incomplete Docker registry authorization credentials. Please provide all of username, password, and email or none.`
>
> In diesem Fall müssen Sie sich möglicherweise über die Docker-Befehlszeile bei Azure anmelden. Beispiel:
>
> `docker login -u wingtiptoysregistry -p "AbCdEfGhIjKlMnOpQrStUvWxYz" wingtiptoysregistry.azurecr.io`
>
> Anschließend können Sie Ihren Container über die Befehlszeile übertragen. Beispiel:
>
> `docker push wingtiptoysregistry.azurecr.io/gs-spring-boot-docker`
>

## <a name="create-a-web-app-on-linux-on-azure-app-service-by-using-your-container-image"></a>Erstellen einer Web-App unter Linux in Azure App Service über Ihr Containerimage

1. Melden Sie sich beim [Azure-Portal]an.

1. Klicken Sie auf das Menüsymbol für **+ Neu** und dann auf **Web + Mobil**.

2.  Klicken Sie auf **Web-App unter Linux**.

   ![Erstellen einer neuen Web-App im Azure-Portal][LX01]

3. Wenn das Blatt **Web-App unter Linux** angezeigt wird, führen Sie folgende Schritte aus:

   a. Geben Sie einen eindeutigen Namen für **App-Name** ein, z.B. *wingtiptoyslinux*.

   b. Wählen Sie Ihr **Abonnement** aus der Dropdown-Liste aus.

   c. Wählen Sie eine vorhandene **Ressourcengruppe** aus, oder geben Sie einen Namen ein, um eine neue Ressourcengruppe zu erstellen.

   d. Klicken Sie auf **Container konfigurieren**, und geben Sie die folgenden Informationen ein:

      * Wählen Sie **Private Registrierung** aus.

      * **Image und optionales Tag**: Geben Sie Ihren vorherigen Containernamen an, z.B. *wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*.

      * **Server-URL**: Geben Sie Ihre vorherige Registrierungs-URL an, z.B. *https://wingtiptoysregistry.azurecr.io*.

      * **Benutzername für Anmeldung** und **Kennwort**: Geben Sie die Anmeldeinformationen Ihrer **Zugriffsschlüssel** an, die Sie in den vorherigen Schritten verwendet haben.

   e. Nachdem Sie alle vorherigen Informationen eingegeben haben, klicken Sie auf **OK**.

   ![Konfigurieren von Einstellungen für Web-Apps][LX02]

1. Klicken Sie auf **Erstellen**.

> [!NOTE]
> Azure ordnet dem eingebetteten Tomcat-Server, der an dem Standardport 80 oder 8080 ausgeführt wird, automatisch Internetanforderungen zu. Wenn Sie Ihren eingebetteten Tomcat-Server jedoch so konfiguriert haben, dass er an einem benutzerdefinierten Port ausgeführt wird, müssen Sie eine Umgebungsvariable zu Ihrer Web-App hinzufügen, die den Port für Ihren eingebetteten Tomcat-Server definiert. Führen Sie dazu die folgenden Schritte aus:
>
>1. Melden Sie sich beim [Azure-Portal]an.

>2. Klicken Sie auf das Symbol für **App Services**. (Siehe Element 1 in der folgenden Abbildung)

>3. Wählen Sie Ihre Web-App aus der Liste aus. (Siehe Element 2 in der folgenden Abbildung)

>4. Klicken Sie auf **Anwendungseinstellungen**. (Siehe Element 3 in der folgenden Abbildung)

>5. Fügen Sie im Abschnitt **App-Einstellungen** eine neue Umgebungsvariable mit dem Namen **PORT** hinzu. Geben Sie dann Ihre benutzerdefinierte Portnummer als Wert ein. (Siehe Element 4 in der folgenden Abbildung)

>6. Wählen Sie **Speichern** aus. (Siehe Element 5 in der folgenden Abbildung)

> ![Speichern einer benutzerdefinierten Portnummer im Azure-Portal][LX03]
>

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung von Spring Boot in Azure finden Sie in den folgenden Artikeln:

- [Bereitstellen von Spring Boot-Anwendungen in Azure App Service][2]


- [Ausführen einer Spring Boot-Anwendung in einem Kubernetes-Cluster in Azure Container Service](container-service-deploy-spring-boot-app-on-kubernetes.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Weitere Informationen zum Verwenden von Azure mit Java finden Sie im [Azure Java Developer Center] und in den [Java-Tools für Visual Studio Team Services].

Weitere Informationen zum Spring Boot-Beispielprojekt in Docker finden Sie unter [Spring Boot on Docker Getting Started] (Erste Schritte mit Spring Boot in Docker).

Hilfe zu den ersten Schritten mit eigenen Spring Boot-Anwendungen finden Sie im [Spring Initializr](https://start.spring.io/).

Weitere Informationen zu den ersten Schritten beim Erstellen einer einfachen Spring Boot-Anwendung finden Sie im [Spring Initializr](https://start.spring.io/).

Weitere Beispiele zur Vorgehensweise bei der Verwendung benutzerdefinierter Docker-Images mit Azure finden Sie unter [Verwenden eines benutzerdefinierten Docker-Images für Azure-Web-Apps unter Linux].

<!-- URL List -->

[Azure-Befehlszeilenschnittstelle (CLI)]: /cli/azure/overview
[Azure Container Service]: https://azure.microsoft.com/services/container-service/
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Azure-Portal]: https://portal.azure.com/
[Erstellen einer privaten Docker-Containerregistrierung mit dem Azure-Portal]: /azure/container-registry/container-registry-get-started-portal
[Verwenden eines benutzerdefinierten Docker-Images für Azure-Web-Apps unter Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[kostenloses Azure-Konto]: https://azure.microsoft.com/pricing/free-trial/
[Git-Client]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java-Tools für Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN-Abonnentenvorteile]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Erste Schritte mit Spring Boot in Docker]: https://github.com/spring-guides/gs-spring-boot-docker (Erste Schritte mit Spring Boot in Docker)
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/container-service-deploy-spring-boot-app-on-linux/SB01.png
[SB02]: ./media/container-service-deploy-spring-boot-app-on-linux/SB02.png

[AR01]: ./media/container-service-deploy-spring-boot-app-on-linux/AR01.png
[AR02]: ./media/container-service-deploy-spring-boot-app-on-linux/AR02.png
[AR03]: ./media/container-service-deploy-spring-boot-app-on-linux/AR03.png
[AR04]: ./media/container-service-deploy-spring-boot-app-on-linux/AR04.png

[LX01]: ./media/container-service-deploy-spring-boot-app-on-linux/LX01.png
[LX02]: ./media/container-service-deploy-spring-boot-app-on-linux/LX02.png
[LX03]: ./media/container-service-deploy-spring-boot-app-on-linux/LX03.png

<!--Reference links in article-->
[1]: https://docs.microsoft.com/azure/container-registry/container-registry-get-started-azure-cli/
[2]: https://docs.microsoft.com/azure/app-service/app-service-deploy-spring-boot-web-app-on-azure/


---

