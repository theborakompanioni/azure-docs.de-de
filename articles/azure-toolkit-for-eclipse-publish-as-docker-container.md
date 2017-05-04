---
title: "Veröffentlichen eines Docker-Containers mit dem Azure-Toolkit für Eclipse | Microsoft-Dokumentation"
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
ms.date: 04/14/2017
ms.author: robmcm
translationtype: Human Translation
ms.sourcegitcommit: 9eafbc2ffc3319cbca9d8933235f87964a98f588
ms.openlocfilehash: f738647fa1dad757b53611c7d4abe2dd99fe8838
ms.lasthandoff: 04/22/2017


---

# <a name="how-to-publish-a-web-app-as-a-docker-container-using-the-azure-toolkit-for-eclipse"></a>Informationen zum Veröffentlichen einer Web-App als Docker-Container mit dem Azure-Toolkit für Eclipse

Docker-Container sind eine weit verbreitete Methode zur Bereitstellung von Webanwendungen, in denen Entwickler alle Projektdateien und Abhängigkeiten in einem einzelnen Paket für die Bereitstellung auf einem Server konsolidieren können. Das Azure-Toolkit für Eclipse vereinfacht diesen Vorgang für Java-Entwickler durch Hinzufügen des Features *Als Docker-Container veröffentlichen* zur Bereitstellung in Microsoft Azure. In diesem Artikel lernen Sie die Schritte kennen, die zum Veröffentlichen Ihrer Anwendungen in Azure als Docker-Container erforderlich sind.

> [!NOTE]
>
> Weitere Informationen zu Docker finden Sie auf der [Docker-Website].
>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="publishing-your-web-app-to-azure-using-a-docker-container"></a>Veröffentlichen Ihre Web-App in Azure mithilfe eines Docker-Containers

1. Öffnen Sie Ihr Web-App-Projekt in Eclipse.

1. Verwenden Sie eine der folgenden Methoden zum Starten des Assistenten „Publish as Docker Container“:

   * Klicken Sie in der Ansicht **Navigator** mit der rechten Maustaste auf Ihr Projekt. Klicken Sie auf **Azure** und dann auf **Publish as Docker Container**:  ![Publish as Docker Container][PUB01]

   * Klicken Sie auf der Eclipse-Symbolleiste auf das Menü **Publish**. Klicken Sie dann auf **Publish as Docker Container**:  ![Publish as Docker Container][PUB02]

1. Sobald der Assistent **Deploy Docker Container on Azure** angezeigt wird, sehen Sie ein Dialogfeld ähnlich wie in der folgenden Abbildung: ![Deploy Docker Container on Azure Wizard][PUB03]

   a. Geben Sie in das Textfeld **Docker ImageName** einen eindeutigen Namen für den Docker-Host ein. (Der Assistent erstellt automatisch einen Namen für Sie, den Sie aber auf Wunsch ändern können.)

   b. Das Fenster **Hosts** zeigt alle Docker-Hosts, die Sie erstellt haben.
      * Wenn Sie keine Docker-Hosts erstellt haben, ist dieser Abschnitt des Dialogfelds leer.
      * Wenn Sie bereits einen Docker-Host erstellt haben, können Sie Ihre Web-App in einem vorhandenen Host bereitstellen. Führen Sie andernfalls die nachstehenden Schritte zum Erstellen eines neuen Docker-Hosts aus.

1. Um einen neuen Docker-Host zu erstellen, klicken Sie auf die Schaltfläche **Add**. Dadurch wird das Dialogfeld **Create Docker Host** geöffnet.
      ![Assistent „Deploy Docker Container on Azure“][PUB04]

   a. Geben Sie die folgenden Optionen für den Docker-Host an. (Der Assistent generiert die meisten Optionen automatisch für Sie. Doch können nach Wunsch alle Optionen anpassen.)

      * **Name**: Dies ist ein eindeutiger Name für den Docker-Host. (Dieser ist nicht identisch mit dem Namen des Docker-Images, den Sie zuvor angegeben haben.)

      * **Subscription**: Geben Sie das Azure-Abonnement an, das Sie für den Host verwenden möchten.
      
      * **Region**: Geben Sie die geografische Region an, in der sich der Host befindet.
      
      * Auf der Registerkarte **OS and Size**:
         * **Host OS**: Geben Sie das Betriebssystem des virtuellen Computers an, auf dem sich Ihr Host befinden soll.
         * **Size**: Geben Sie die Größe des virtuellen Computers für Ihren Host an.

      * Auf der Registerkarte **Resource Group**:
         * **New resource group**: Ermöglicht Ihnen die Erstellung einer neuen Ressourcengruppe für den Host.
         * **Existing resource group**: Erlaubt das Angeben einer in Ihrem Azure-Konto vorhandenen Ressourcengruppe.

      * Auf der Registerkarte **Network**:
         * **New virtual network**: Ermöglicht Ihnen das Erstellen eines neuen virtuellen Netzwerks für Ihren Host.
         * **Existing virtual network**: Erlaubt das Angeben eines in Ihrem Azure-Konto vorhandenen virtuellen Netzwerks.

      * Auf der Registerkarte **Storage**:
         * **New storage account**: Ermöglicht Ihnen die Erstellung eines neuen Speicherkontos für den Host.
         * **Existing storage account**: Erlaubt das Angeben eines in Ihrem Azure-Konto vorhandenen Speicherkontos.

   b. Nachdem Sie die oben genannten Optionen angegeben haben, klicken Sie auf **Weiter**.

   c. Wählen Sie eine der folgenden Optionen für die VM-Anmeldung Ihres Docker-Hosts:    ![Docker-Host erstellen][PUB05]

      * **Import credentials from Azure Key Vault**: Ermöglicht Ihnen das Angeben zuvor gespeicherter Anmeldeinformationen, die in Ihrem Azure-Abonnement gespeichert sind.

      > [!NOTE]
      > Auf einen Azure Key Vault, der in einem bestimmten Konto oder Dienstprinzipal erstellt wird, kann ein anderes Konto oder ein anderer Dienstprinzipal, das bzw. der dasselbe Abonnement nutzt, nicht automatisch zugreifen. Damit ein anderes Konto oder ein anderer Dienstprinzipal den Schlüsseltresor verwenden kann, müssen Sie im Azure-Portal das Konto oder den Dienstprinzipal hinzufügen.

      * **New log in credentials**: Ermöglicht Ihnen das Erstellen neuer Anmeldeinformationen, wofür Sie die folgenden Optionen auf der Registerkarte **VM Credentials** angeben müssen:
         * **Username**: Gibt den Benutzernamen für die Anmeldung bei Ihrer VM an.
         * **Password** und **Confirm**: Gibt das Kennwort für die Anmeldung bei Ihrer VM an.
         * **SSH**: Gibt die SSH-Einstellungen (Secure Shell) für den Docker-Host an. Sie können aus den folgenden Optionen wählen:
            * **None**: Gibt an, dass Ihr virtueller Computer keine SSH-Verbindungen zulässt.
            * **Auto-generate**: Mithilfe dieser Option werden die erforderlichen Einstellungen zum Herstellen einer Verbindung über SSH automatisch erstellt.
            * **Import from directory**: Ermöglicht Ihnen das Angeben eines Verzeichnisses, das zuvor gespeicherte SSH-Einstellungen enthält. Genauer gesagt, muss das Verzeichnis die folgenden beiden Dateien enthalten:
               * *id_rsa*: Diese Datei enthält die RSA-Kennung eines Benutzers.
               * *id_rsa.pub*: Diese Datei enthält den öffentlichen RSA-Schlüssel, der für die Authentifizierung verwendet werden soll.
        
      * Geben Sie auf der Registerkarte **Docker Daemon Access** die folgenden Optionen an: ![Docker-Host erstellen][PUB06]

         * **Docker Daemon port**: Gibt den eindeutigen TCP-Port für den Docker-Host an.
         * **TLS Security**: Gibt die Transport Layer Security-Einstellungen für den Docker-Host an. Sie können aus den folgenden Optionen wählen:
            * **None**: Gibt an, dass Ihr virtueller Computer keine TLS-Verbindungen zulässt.
            * **Auto-generate**: Mithilfe dieser Option werden die erforderlichen Einstellungen zum Herstellen einer Verbindung über TLS automatisch erstellt.
            * **Import from directory**: Ermöglicht Ihnen das Angeben eines Verzeichnisses, das zuvor gespeicherte TLS-Einstellungen enthält. Genauer gesagt, muss das Verzeichnis die folgenden sechs Dateien enthalten:
               * *ca.pem* und *ca-key.pem*: Diese Dateien enthalten das Zertifikat und den öffentlichen Schlüssel für die TLS-Zertifizierungsstelle.
               * *cert.pem* und *key.pem*: Diese Dateien enthalten das Clientzertifikat und einen öffentlichen Schlüssel für die TLS-Authentifizierung.
               * *server.pem* und *server-key.pem*: Diese Dateien enthalten das Serverzertifikat und den öffentlichen Schlüssel für den Host.

   d. Nachdem Sie alle oben genannten Optionen eingegeben haben, klicken Sie auf **Fertig stellen**.

1. Wenn der Assistent **Deploy Docker Container on Azure** erneut angezeigt wird, klicken Sie auf **Next**.
   ![Assistent „Deploy Docker Container on Azure“][PUB07]

1. Legen Sie auf der letzten Seite des Assistenten die folgenden Optionen fest:

   * **Docker container name**: Dies ist der eindeutige Name des Docker-Containers.

   * Wählen Sie eines der folgenden Docker-Images aus:

      * **Predefined Docker image**: Ermöglicht Ihnen die Angabe eines bereits in Azure vorhandenen Docker-Images.

      > [!NOTE]
      > Die Liste der Docker-Images in diesem Dropdownmenü besteht aus mehrere Images, für deren Patching das Azure-Toolkit konfiguriert ist, damit Ihr Artefakt automatisch bereitgestellt wird.

      * **Custom Dockerfile**: Ermöglicht Ihnen das Angeben einer zuvor auf Ihrem lokalen Computer gespeicherten Dockerfile.

      > [!NOTE]
      > Dies ist ein komplexeres Feature für Entwickler, die ihre eigenen Dockerfile bereitstellen möchten. Es ist jedoch Aufgabe von Entwickler, die diese Option wählen, dafür zu sorgen, dass ihre Dockerfile ordnungsgemäß erstellt wurde. Das Azure-Toolkit überprüft nicht den Inhalt einer benutzerdefinierten Dockerfile, sodass die Bereitstellung fehlschlagen kann, wenn die Dockerfile Probleme verursacht. Darüber hinaus erwartet das Azure Toolkit, dass die benutzerdefinierte Dockerfile ein Web-App-Artefakt enthält, und versucht, eine HTTP-Verbindung zu öffnen. Wenn Entwickler einen anderen Typ von Artefakt veröffentlichen, erhalten sie nach der Bereitstellung ggf. harmlose Fehlermeldungen.

   * **Port settings**: Gibt die eindeutige TCP-Portbindung für den Docker-Container an.
   ![Assistent „Deploy Docker Container on Azure“][PUB08]

1. Nachdem Sie alle zuvor genannten Schritte ausgeführt haben, klicken Sie auf **Finish**.

Das Azure-Toolkit beginnt, Ihre Web-App in einem Docker-Container in Azure bereitzustellen. 

## <a name="see-also"></a>Weitere Informationen
Weitere Informationen zu den Azure-Toolkits für Java-IDEs finden Sie unter den folgenden Links:

* [Azure-Toolkit für Eclipse]
  * [Neuerungen im Azure-Toolkit für Eclipse]
  * [Installieren des Azure-Toolkits für Eclipse]
  * [Anleitung zur Azure-Anmeldung für das Azure-Toolkit für Eclipse]
  * [Erstellen einer „Hello World“-Web-App für Azure in Eclipse]
* [Azure Toolkit für IntelliJ]
  * [Neuerungen im Azure-Toolkit für IntelliJ]
  * [Installieren des Azure Toolkit für IntelliJ]
  * [Anleitung zur Azure-Anmeldung für das Azure-Toolkit für IntelliJ]
  * [Erstellen einer „Hello World“-Web-App für Azure in IntelliJ]

Weitere Informationen zum Verwenden von Azure mit Java finden Sie im [Azure Java Developer Center] und in den [Java-Tools für Visual Studio Team Services].

Zusätzliche Ressourcen für Docker finden Sie auf der offiziellen [Docker-Website].

<!-- URL List -->

[Azure-Toolkit für Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure Toolkit für IntelliJ]: ./azure-toolkit-for-intellij.md
[Erstellen einer „Hello World“-Web-App für Azure in Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Erstellen einer „Hello World“-Web-App für Azure in IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installieren des Azure-Toolkits für Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Installieren des Azure Toolkit für IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Anleitung zur Azure-Anmeldung für das Azure-Toolkit für Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Anleitung zur Azure-Anmeldung für das Azure-Toolkit für IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Neuerungen im Azure-Toolkit für Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Neuerungen im Azure-Toolkit für IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Java-Tools für Visual Studio Team Services]: https://java.visualstudio.com/

[Docker-Website]: https://www.docker.com/

<!-- IMG List -->

[PUB01]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB01.png
[PUB02]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB02.png
[PUB03]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB03.png
[PUB04a]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04a.png
[PUB04b]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04b.png
[PUB04c]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04c.png
[PUB04d]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04d.png
[PUB05]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB05.png
[PUB06]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB06.png
[PUB07]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB07.png
[PUB08]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB08.png

