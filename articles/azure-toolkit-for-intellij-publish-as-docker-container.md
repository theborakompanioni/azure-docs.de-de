---
title: "Veröffentlichen eines Docker-Containers mit dem Azure-Toolkit für IntelliJ | Microsoft-Dokumentation"
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
ms.date: 04/14/2017
ms.author: robmcm
translationtype: Human Translation
ms.sourcegitcommit: 9eafbc2ffc3319cbca9d8933235f87964a98f588
ms.openlocfilehash: f8ae15f4a5edb3cd84b5bb2302e1a5f709b57f82
ms.lasthandoff: 04/22/2017


---

# <a name="how-to-publish-a-web-app-as-a-docker-container-using-the-azure-toolkit-for-intellij"></a>Informationen zum Veröffentlichen einer Web-App als Docker-Container mit dem Azure-Toolkit für IntelliJ

Docker-Container sind eine weit verbreitete Methode zur Bereitstellung von Webanwendungen, in denen Entwickler alle Projektdateien und -abhängigkeiten in einem einzelnen Paket für die Bereitstellung auf einem Server konsolidieren können. Das Azure-Toolkit für IntelliJ vereinfacht diesen Vorgang für Java-Entwickler durch Hinzufügen der Funktion *Als Docker-Container veröffentlichen* zur Bereitstellung in Microsoft Azure. In diesem Artikel lernen Sie die Schritte kennen, die zum Veröffentlichen Ihrer Anwendungen in Azure als Docker-Container erforderlich sind.

> [!NOTE]
>
> Weitere Informationen zu Docker finden Sie auf der [Docker-Website].
>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="publishing-your-web-app-to-azure-using-a-docker-container"></a>Veröffentlichen Ihrer Web-App in Azure mithilfe eines Docker-Containers

> [!NOTE]
>
> Um Ihre Web-App zu veröffentlichen, müssen Sie ein einsatzbereites Artefakt erstellen. Weitere Informationen finden Sie unter [Zusätzliche Informationen zum Erstellen von Artefakten](#artifacts) weiter unten in diesem Artikel.
>
> Nachdem Sie den Bereitstellungs-Assistenten mindestens einmal durchgeführt haben, werden die meisten Einstellungen, die Sie in dieser exemplarischen Vorgehensweise festlegen, darüber hinaus als Standardwerte für die erneute Ausführung des Assistenten verwendet.
>

1. Öffnen Sie Ihr Web-App-Projekt in IntelliJ.

1. Verwenden Sie eine der folgenden Methoden zum Starten des Assistenten „Als Docker-Container veröffentlichen“:

   * Klicken Sie im **Projekt**-Toolfenster mit der rechten Maustaste auf Ihr Projekt. Klicken Sie anschließend auf **Azure** und dann auf **Als Docker-Container veröffentlichen**:  ![Als Docker-Container veröffentlichen][PUB01]

   * Klicken Sie auf der IntelliJ-Symbolleiste auf das Menü **Gruppe veröffentlichen**. Klicken Sie dann auf **Als Docker-Container veröffentlichen**:  ![Als Docker-Container veröffentlichen][PUB02]

1. Sobald der Assistent **Docker-Container in Azure bereitstellen** angezeigt wird, sehen Sie ein Dialogfeld ähnlich wie in der folgenden Abbildung: ![Docker-Container im Azure-Assistenten bereitstellen][PUB03]

   a. Geben Sie in das Textfeld **Name des Docker-Images** einen eindeutigen Namen für den Docker-Host ein. (Der Assistent erstellt automatisch einen Namen für Sie, den Sie aber bei Bedarf ändern können.)

   b. Das Fenster **Hosts** zeigt alle Docker-Hosts an, die Sie erstellt haben.
      * Wenn Sie keine Docker-Hosts erstellt haben, wird dieser Abschnitt des Dialogfelds leer sein.
      * Wenn Sie bereits einen Docker-Host erstellt haben, können Sie Ihre Web-App in einem vorhandenen Host bereitstellen. Führen Sie andernfalls die nachstehenden Schritte zum Erstellen eines neuen Docker-Hosts aus.

1. Um einen neuen Docker-Host zu erstellen, klicken Sie auf das grüne Pluszeichen („**+**“). Dadurch wird das Dialogfeld **Docker-Host erstellen** geöffnet.
      ![Assistent „Docker-Container in Azure bereitstellen“][PUB04a]

   a. Legen Sie die folgenden Optionen für den Docker-Host fest. (Der Assistent generiert automatisch die meisten Optionen für Sie. Sie können aber nach Wunsch alle Optionen anpassen.)

      * **Name**: Dies ist ein eindeutiger Name für den Docker-Host. (Dieser ist nicht mit dem Namen des Docker-Images identisch, den Sie zuvor angegeben haben.)

      * **Abonnement**: Geben Sie das Azure-Abonnement an, das Sie für den Host verwenden möchten.
      
      * **Region**: Geben Sie die geografische Region an, in der sich der Host befindet.
      
      * Auf der Registerkarte **BS und Größe**:
         * **Hostbetriebssystem**: Geben Sie das Betriebssystem des virtuellen Computers an, auf dem sich Ihr Host befinden soll.
         * **Größe**: Geben Sie die Größe des virtuellen Computers für Ihren Host an.

      * Auf der Registerkarte **Ressourcengruppe**:
         * **Neue Ressourcengruppe**: Ermöglicht Ihnen die Erstellung einer neuen Ressourcengruppe für den Host.
         * **Vorhandene Ressourcengruppe**: Erlaubt Ihnen das Festlegen einer vorhandenen Ressourcengruppe in Ihrem Azure-Konto.

      * Auf der Registerkarte **Netzwerk**:
         * **Neues virtuelles Netzwerk**: Ermöglicht Ihnen das Erstellen eines neuen virtuellen Netzwerks für Ihren Host.
         * **Vorhandenes virtuelles Netzwerk**: Erlaubt Ihnen das Festlegen eines vorhandenen virtuellen Netzwerks in Ihrem Azure-Konto.

      * Auf der Registerkarte **Speicher**:
         * **Neues Speicherkonto**: Ermöglicht Ihnen die Erstellung eines neuen Speicherkontos für den Host.
         * **Vorhandenes Speicherkonto**: Erlaubt Ihnen das Festlegen eines vorhandenen Speicherkontos in Ihrem Azure-Konto.

   b. Nachdem Sie die oben genannten Optionen festgelegt haben, klicken Sie auf **Weiter**.

   c. Wählen Sie eine der folgenden Optionen für die Anmeldung beim virtuellen Computer Ihres Docker-Hosts:    ![Docker-Host erstellen][PUB05]

      * **Anmeldeinformationen aus Azure Key Vault importieren**: Ermöglicht Ihnen das Festlegen zuvor gespeicherter Anmeldeinformationen, die in Ihrem Azure-Abonnement gespeichert sind.

      > [!NOTE]
      > Auf Azure Key Vault, das in einem bestimmten Konto oder Dienstprinzipal erstellt wird, kann kein anderes Konto und kein anderer Dienstprinzipal, das bzw. der dasselbe Abonnement nutzt, automatisch zugreifen. Damit ein anderes Konto oder ein anderer Dienstprinzipal Key Vault verwenden kann, müssen Sie im Azure-Portal das Konto oder den Dienstprinzipal hinzufügen.

      * **Neue Anmeldeinformationen**: Ermöglicht Ihnen das Erstellen neuer Anmeldeinformationen, wofür Sie die folgenden Optionen auf der Registerkarte **VM-Anmeldeinformationen** festlegen müssen:
         * **Benutzername**: Gibt den Benutzernamen für die Anmeldung bei Ihrem virtuellen Computer an.
         * **Kennwort** und **Bestätigen**: Gibt das Kennwort für die Anmeldung bei Ihrem virtuellen Computer an.
         * **SSH**: Gibt die SSH-Einstellungen (Secure Shell) für den Docker-Host an. Sie können aus den folgenden Optionen wählen:
            * **Keine**: Gibt an, dass Ihr virtueller Computer keine SSH-Verbindungen zulässt.
            * **Automatisch generieren**: Mithilfe dieser Option werden die erforderlichen Einstellungen zum Herstellen einer Verbindung über SSH automatisch erstellt.
            * **Aus Verzeichnis importieren**: Ermöglicht Ihnen das Angeben eines Verzeichnisses, dass zuvor gespeicherte SSH-Einstellungen enthält. Genauer gesagt, muss das Verzeichnis die folgenden beiden Dateien enthalten:
               * *id_rsa*: Diese Datei enthält die RSA-Kennung für einen Benutzer.
               * *id_rsa.pub*: Diese Datei enthält den öffentlichen RSA-Schlüssel, der für die Authentifizierung verwendet wird.
        
      * Geben Sie auf der Registerkarte **Docker-Daemon-Zugriff** die folgenden Optionen an: ![Docker-Host erstellen][PUB06]

         * **Docker-Daemon-Port**: Gibt den eindeutigen TCP-Port für den Docker-Host an.
         * **TLS-Sicherheit**: Gibt die Transport Layer Security-Einstellungen für den Docker-Host an. Sie können aus den folgenden Optionen wählen:
            * **Keine**: Gibt an, dass Ihr virtueller Computer keine TLS-Verbindungen zulässt.
            * **Automatisch generieren**: Mithilfe dieser Option werden die erforderlichen Einstellungen zum Herstellen einer Verbindung über TLS automatisch erstellt.
            * **Aus Verzeichnis importieren**: Ermöglicht Ihnen das Angeben eines Verzeichnisses, dass zuvor gespeicherte TLS-Einstellungen enthält. Genauer gesagt muss das Verzeichnis die folgenden sechs Dateien enthalten:
               * *ca.pem* und *ca-key.pem*: Diese Dateien enthalten das Zertifikat und den öffentlichen Schlüssel für die TLS-Zertifizierungsstelle.
               * *cert.pem* und *key.pem*: Diese Dateien enthalten das Clientzertifikat und einen öffentlichen Schlüssel für die TLS-Authentifizierung.
               * *server.pem* und *server-key.pem*: Diese Dateien enthalten das Serverzertifikat und den öffentlichen Schlüssel für den Host.

   d. Nachdem Sie alle oben genannten Optionen eingegeben haben, klicken Sie auf **Fertig stellen**.

1. Wenn der Assistent **Docker-Container in Azure bereitstellen** erneut angezeigt wird, klicken Sie auf **Weiter**.
   ![Assistent „Docker-Container in Azure bereitstellen“][PUB07]

1. Legen Sie auf der letzten Seite des Assistenten die folgenden Optionen fest:

   * **Name des Docker-Containers**: Dies ist der eindeutige Name des Docker-Containers.

   * Wählen Sie eines der folgenden Docker-Images aus:

      * **Vordefiniertes Docker-Image**: Ermöglicht Ihnen die Angabe eines bereits in Azure vorhandenen Docker-Images.

      > [!NOTE]
      > Die Liste der Docker-Images in diesem Dropdownmenü besteht aus mehreren Images, für deren Patching das Azure-Toolkit konfiguriert ist, damit Ihr Artefakt automatisch bereitgestellt wird.

      * **Benutzerdefinierte Docker-Datei**: Ermöglicht Ihnen das Angeben einer zuvor auf Ihrem lokalen Computer gespeicherten Docker-Datei.

      > [!NOTE]
      > Dies ist eine komplexere Funktion für Entwickler, die ihre eigene Docker-Datei bereitstellen möchten. Es ist jedoch Aufgabe der Entwickler, die diese Option wählen, dafür zu sorgen, dass ihre Docker-Datei ordnungsgemäß erstellt wurde. Das Azure-Toolkit überprüft nicht den Inhalt in einer benutzerdefinierten Docker-Datei, sodass die Bereitstellung fehlschlagen kann, wenn die Docker-Datei Probleme verursacht. Darüber hinaus erwartet das Azure-Toolkit, dass die benutzerdefinierte Docker-Datei ein Web-App-Artefakt enthält, und versucht, eine HTTP-Verbindung zu öffnen. Wenn Entwickler einen anderen Typ von Artefakt veröffentlichen, erhalten sie nach der Bereitstellung ggf. harmlose Fehlermeldungen.

   * **Porteinstellungen**: Gibt die eindeutige TCP-Portbindung für den Docker-Container an.
   ![Assistent „Docker-Container in Azure bereitstellen“][PUB08]

1. Nachdem Sie alle oben genannten Schritte ausgeführt haben, klicken Sie auf **Fertig stellen**.

Das Azure-Toolkit stellt Ihre Web-App in Azure nun in einem Docker-Container bereit. Sofern Sie IntelliJ nicht für die Bereitstellung im Hintergrund konfiguriert haben, wird ein Dialogfeld angezeigt, das den Bereitstellungsstatus anzeigt. 
![Bereitstellungsstatus][PUB09]

<a name="artifacts"></a>
## <a name="additional-information-about-creating-artifacts"></a>Zusätzliche Informationen zum Erstellen von Artefakten

Um ein bereitstellungsfähiges Artefakt zu erstellen, führen Sie folgende Schritte durch:

1. Öffnen Sie Ihr Web-App-Projekt in IntelliJ.

1. Klicken Sie auf **Datei** und dann auf **Projektstruktur**.
   ![Menü „Projektstruktur“][ART01]

1. Klicken Sie auf das grüne Pluszeichen („**+**“), um ein Artefakt hinzuzufügen, und klicken Sie dann auf **Webanwendung: Artefakt**.
   ![Artefakt hinzufügen][ART02]

1. Vergeben Sie einen Namen für Ihr Artefakt und achten Sie dabei darauf, dass die Erweiterung „.war“ nicht hinzugefügt wird. Klicken Sie dann auf **OK**.
   ![Eigenschaften des Artefakts][ART03]

Weitere Informationen zum Erstellen von Artefakten in IntelliJ finden Sie auf der JetBrains-Website unter [Konfigurieren von Artefakten].

## <a name="see-also"></a>Weitere Informationen
Weitere Informationen zu den Azure-Toolkits für Java-IDEs finden Sie unter den folgenden Links:

* [Azure-Toolkit für Eclipse]
  * [Neuerungen im Azure-Toolkit für Eclipse]
  * [Installieren des Azure-Toolkits für Eclipse]
  * [Anleitung zur Anmeldung für das Azure-Toolkit für Eclipse]
  * [Erstellen einer „Hello World“-Web-App für Azure in Eclipse]
* [Azure Toolkit für IntelliJ]
  * [Neuerungen im Azure-Toolkit für IntelliJ]
  * [Installieren des Azure Toolkit für IntelliJ]
  * [Anleitung zur Anmeldung für das Azure-Toolkit für IntelliJ]
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
[Anleitung zur Anmeldung für das Azure-Toolkit für Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Anleitung zur Anmeldung für das Azure-Toolkit für IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Neuerungen im Azure-Toolkit für Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Neuerungen im Azure-Toolkit für IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Java-Tools für Visual Studio Team Services]: https://java.visualstudio.com/

[Docker-Website]: https://www.docker.com/
[Konfigurieren von Artefakten]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html

<!-- IMG List -->

[PUB01]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB01.png
[PUB02]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB02.png
[PUB03]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB03.png
[PUB04a]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04a.png
[PUB04b]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04b.png
[PUB04c]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04c.png
[PUB04d]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04d.png
[PUB05]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB05.png
[PUB06]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB06.png
[PUB07]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB07.png
[PUB08]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB08.png
[PUB09]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB09.png

[ART01]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/ART01.png
[ART02]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/ART02.png
[ART03]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/ART03.png

