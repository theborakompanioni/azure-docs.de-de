---
title: "Installieren des Azure-Toolkits für IntelliJ | Microsoft Docs"
description: "Erfahren Sie, wie Sie das Azure-Toolkit für IntelliJ IDEA installieren."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: c6817c7b-f28c-4c06-8216-41c7a8117de3
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
translationtype: Human Translation
ms.sourcegitcommit: 9eafbc2ffc3319cbca9d8933235f87964a98f588
ms.openlocfilehash: bf11a8580500f78c4a96a02953f221501eeffe6c
ms.lasthandoff: 04/22/2017


---
# <a name="installing-the-azure-toolkit-for-intellij"></a>Installieren des Azure-Toolkits für IntelliJ
Das Azure-Toolkit für IntelliJ stellt Vorlagen und Funktionen bereit, die Ihnen das einfache Erstellen, Entwickeln, Testen und Bereitstellen von Azure-Anwendungen mithilfe der IntelliJ IDEA-Entwicklungsumgebung ermöglichen. Das Azure-Toolkit für IntelliJ ist ein Open-Source-Projekt, dessen Quellcode unter der MIT-Lizenz auf der GitHub-Website des Projekts verfügbar ist:

<https://github.com/microsoft/azure-tools-for-java>

Es gibt zwei Methoden zum Installieren des Azure-Toolkit für IntelliJ: über das Dialogfeld „Einstellungen“ und über das Menü „Konfigurieren“ auf dem Startbildschirm. In den folgenden Schritten werden beide Installationsmethoden veranschaulicht.

[!INCLUDE [azure-toolkit-for-IntelliJ-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-intellij-from-the-settings-dialog-box"></a>So installieren Sie das Azure-Toolkit für IntelliJ über das Dialogfeld „Einstellungen“
1. Starten Sie IntelliJ IDEA.
2. Wenn IntelliJ IDEA geöffnet wird, klicken Sie auf **File** (Datei), klicken Sie dann auf **Settings** (Einstellungen).
   
    ![Das Dialogfeld „Settings“ (Einstellungen) von IntelliJ IDEA öffnen][01a]
3. Klicken Sie im Dialogfeld „Settings“ (Einstellungen) auf **Plugins**, und klicken Sie dann auf **Browse Repositories** (Repositorys durchsuchen).
   
    ![Dialogfeld „Settings“ (Einstellungen) von IntelliJ IDEA][02a]
4. Geben Sie im Dialogfeld **Browse Repositories** (Repositorys durchsuchen) in das Suchfeld „Azure“ ein. Markieren Sie **Azure Toolkit for IntelliJ** (Azure Toolkit für IntelliJ), und klicken Sie dann auf **Install** (Installieren).
   
    ![Suchen nach dem Azure-Toolkit für IntelliJ][03]
   
    IntelliJ IDEA zeigt den Installationsstatus in einem Dialogfeld an.
   
    ![Installationsstatus][04]
5. Klicken Sie nach Abschluss der Installation auf **Restart IntelliJ IDEA**(IntelliJ IDEA neu starten).
   
    ![Restart IntelliJ IDEA][05]
6. Klicken Sie auf **OK** , um das Dialogfeld „Settings“ (Einstellungen) zu schließen.
   
    ![Das Dialogfeld „Settings“ (Einstellungen) von IntelliJ IDEA schließen][06]
7. Wenn Sie gefragt werden, ob Sie IntelliJ IDEA neu starten oder den Neustart später durchführen möchten, klicken Sie auf **Restart**(Neu starten).
   
    ![Restart IntelliJ IDEA][07]

## <a name="to-install-the-azure-toolkit-for-intellij-from-the-start-screen"></a>So installieren Sie das Azure-Toolkit für IntelliJ über den Startbildschirm
1. Starten Sie IntelliJ IDEA.
2. Wenn der Startbildschirm von IntelliJ IDEA angezeigt wird, klicken Sie auf **Configure** (Konfigurieren), und klicken Sie dann auf **Plugins**.
   
    ![IntelliJ IDEA-Plug-Ins installieren][01b]
3. Klicken Sie im Dialogfeld **Plugins** auf **Browse Repositories** (Repositorys durchsuchen).
   
    ![Die Plug-In-Repositorys von IntelliJ IDEA durchsuchen][02b]
4. Geben Sie im Dialogfeld **Browse Repositories** (Repositorys durchsuchen) in das Suchfeld „Azure“ ein. Markieren Sie **Azure Toolkit for IntelliJ** (Azure Toolkit für IntelliJ), und klicken Sie dann auf **Install** (Installieren).
   
    ![Suchen nach dem Azure-Toolkit für IntelliJ][03]
   
    IntelliJ IDEA zeigt den Installationsstatus in einem Dialogfeld an.
   
    ![Installationsstatus][04]
5. Klicken Sie nach Abschluss der Installation auf **Restart IntelliJ IDEA**(IntelliJ IDEA neu starten).
   
    ![Restart IntelliJ IDEA][05]
6. Wenn Sie gefragt werden, ob Sie IntelliJ IDEA neu starten oder den Neustart später durchführen möchten, klicken Sie auf **Restart**(Neu starten).
   
    ![Restart IntelliJ IDEA][07]

## <a name="see-also"></a>Weitere Informationen
Weitere Informationen zu den Azure-Toolkits für Java-IDEs finden Sie unter den folgenden Links:

* [Azure-Toolkit für Eclipse]
  * [Neuerungen im Azure-Toolkit für Eclipse]
  * [Installieren des Azure-Toolkits für Eclipse]
  * [Anleitung zur Azure-Anmeldung für das Azure-Toolkit für Eclipse]
  * [Erstellen einer „Hello World“-Web-App für Azure in Eclipse]
* [Azure Toolkit für IntelliJ]
  * [Neuerungen im Azure-Toolkit für IntelliJ]
  * *Installieren des Azure-Toolkits für IntelliJ (dieser Artikel)*
  * [Anleitung zur Azure-Anmeldung für das Azure-Toolkit für IntelliJ]
  * [Erstellen einer „Hello World“-Web-App für Azure in IntelliJ]

Weitere Informationen zum Verwenden von Azure mit Java finden Sie im [Azure Java Developer Center].

<!-- URL List -->

[Azure-Toolkit für Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure Toolkit für IntelliJ]: ./azure-toolkit-for-intellij.md
[Erstellen einer „Hello World“-Web-App für Azure in Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Erstellen einer „Hello World“-Web-App für Azure in IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installieren des Azure-Toolkits für Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Installing the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Anleitung zur Azure-Anmeldung für das Azure-Toolkit für Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Anleitung zur Azure-Anmeldung für das Azure-Toolkit für IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Neuerungen im Azure-Toolkit für Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Neuerungen im Azure-Toolkit für IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/

<!-- IMG List -->

[01a]: ./media/azure-toolkit-for-intellij-installation/01-intellij-file-settings.png
[01b]: ./media/azure-toolkit-for-intellij-installation/01-intellij-configure-dropdown.png
[02a]: ./media/azure-toolkit-for-intellij-installation/02-intellij-settings-dialog.png
[02b]: ./media/azure-toolkit-for-intellij-installation/02-intellij-plugins-dialog.png
[03]: ./media/azure-toolkit-for-intellij-installation/03-intellij-browse-repositories.png
[04]: ./media/azure-toolkit-for-intellij-installation/04-install-progress.png
[05]: ./media/azure-toolkit-for-intellij-installation/05-restart-intellij.png
[06]: ./media/azure-toolkit-for-intellij-installation/06-intellij-settings-dialog.png
[07]: ./media/azure-toolkit-for-intellij-installation/07-restart-intellij.png

