---
title: Debuggen einer Java-Web-App in Azure in IntelliJ | Microsoft-Dokumentation
description: "Dieses Tutorial zeigt das Debuggen einer in Azure ausgeführten Java-Web-App mit dem Azure Toolkit für IntelliJ."
services: app-service\web
documentationcenter: java
author: selvasingh
manager: wpickett
editor: 
ms.assetid: 78148648-5a89-4b7d-98f1-9cf8f38dfe8d
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 12/22/2016
ms.author: asirveda;robmcm
translationtype: Human Translation
ms.sourcegitcommit: ff60ebaddd3a7888cee612f387bd0c50799496ac
ms.openlocfilehash: 50db8f7d1d514d076af453a34ca3a147e357e905


---
# <a name="debug-a-java-web-app-on-azure-in-intellij"></a>Debuggen einer Java-Web-App in Azure in IntelliJ
Dieses Tutorial zeigt das Debuggen einer in Azure ausgeführten Java-Web-App mit dem [Azure-Toolkit für IntelliJ]. Der Einfachheit halber verwenden Sie in diesem Tutorial ein einfaches JSP-Beispiel (Java Server Page). Doch beim Debuggen in Azure sind die Schritte für ein Java-Servlet ähnlich.

Nachdem Sie dieses Tutorial abgeschlossen haben, entspricht Ihre Anwendung, wenn Sie sie in IntelliJ debuggen, etwa der folgenden Abbildung:

![][01]

## <a name="prerequisites"></a>Voraussetzungen
* Ein Java Developer Kit (JDK), Version 1.8 oder höher.
* IntelliJ IDEA Ultimate Edition. Diese können unter <https://www.jetbrains.com/idea/download/index.html> heruntergeladen werden.
* Eine Verteilung eines Java-basierten Webservers oder Anwendungsservers, wie z. B. Apache Tomcat oder Jetty.
* Ein Azure-Abonnement, das Sie unter <https://azure.microsoft.com/de-de/free/> oder <http://azure.microsoft.com/pricing/purchase-options/> erhalten.
* Das Azure-Toolkit für IntelliJ Weitere Informationen finden Sie unter [Installieren des Azure-Toolkits für IntelliJ].
* Ein in Azure App Service erstelltes und bereitgestelltes dynamisches Webprojekt. Ein Beispiel finden Sie unter [Erstellen einer „Hello World“-Web-App für Azure in IntelliJ].

## <a name="to-debug-a-java-web-app-on-azure"></a>Debuggen einer Java-Web-App in Azure
Zum Ausführen der Schritte in diesem Abschnitt können Sie ein vorhandenes dynamisches Webprojekt nutzen, das Sie bereits als Java-Web-App in Azure bereitgestellt haben. Laden Sie ein [Beispiel eines dynamischen Webprojekts] herunter, und befolgen Sie die Anweisungen unter [Erstellen einer „Hello World“-Web-App für Azure in IntelliJ], um diese in Azure bereitzustellen. 

1. Öffnen Sie das Java-Web-App-Projekt, das Sie in Azure in IntelliJ bereitgestellt haben.
2. Klicken Sie auf das Menü **Run** (Ausführen) und dann auf **Edit Configurations...** (Konfigurationen bearbeiten...).
   
    ![][02]
3. Gehen Sie im geöffneten Dialogfeld **Run/Debug Configurations** (Konfigurationen ausführen/debuggen) so vor: 
   
   1. Wählen Sie **Azure Web App**aus.
   2. Klicken Sie auf **+** , um eine neue Konfiguration hinzuzufügen.
   3. Geben Sie einen **Namen** für die Konfiguration an.
   4. Übernehmen Sie die übrigen Standardwerte, die vom Azure-Toolkit vorgeschlagen werden, und klicken Sie dann auf **OK**.
      
       ![][03]
4. Wählen Sie die Azure-Web-App-Debugkonfiguration, die Sie gerade erstellt haben, rechts oben im Menü aus, und klicken Sie auf **Debug**
   
    ![][04]
5. Bei der Frage **Enable remote debugging in the remote Web App now?** (Remotedebuggen in Remote-Web-App jetzt aktivieren?) klicken Sie auf **OK**.
6. Wenn **Your web app is now ready for remote debugging** (Ihre Web-App ist jetzt für das Remotedebuggen bereit) angezeigt wird, klicken Sie auf **OK**.
   
    ![][05]
7. Wählen Sie die Azure-Web-App-Debugkonfiguration, die Sie gerade erstellt haben, rechts oben im Menü aus, und klicken Sie auf **Debug**.
8. Ein Windows-Eingabeaufforderung oder eine Unix-Shell wird geöffnet und die für das Debuggen erforderliche Verbindung vorbereitet. Sie müssen warten, bis die Verbindung mit Ihrer Java-Remote-Web-App hergestellt wurde, ehe Sie fortfahren können. Unter Windows sieht dies wie in der folgenden Abbildung aus:
   
    ![][06]
9. Fügen Sie auf Ihrer JSP-Seite einen Haltepunkt ein, und öffnen Sie dann in einem Browser die URL zu Ihrer Java-Web-App:
   
   1. Öffnen Sie **Azure Explorer** in IntelliJ.
   2. Navigieren Sie zu **Web Apps** und zur Java-Web-App, die Sie debuggen möchten.
   3. Klicken Sie mit der rechten Maustaste auf die Web-App, und klicken Sie dann auf **Open in Browser**(Im Browser öffnen).
   4. IntelliJ wechselt jetzt in den Debugmodus.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Verwenden von Azure mit Java finden Sie im [Azure Java Developer Center].

Weitere Informationen zum Erstellen von Azure-Web-Apps finden Sie unter [Web-Apps – Übersicht].

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Azure-Toolkit für IntelliJ]: ../azure-toolkit-for-intellij.md
[Installieren des Azure-Toolkits für IntelliJ]: ../azure-toolkit-for-intellij-installation.md
[Erstellen einer „Hello World“-Web-App für Azure in IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md
[Beispiel eines dynamischen Webprojekts]: http://go.microsoft.com/fwlink/?LinkId=817337

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Web-Apps – Übersicht]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-debug-java-web-app-in-intellij/01-debug-java-web-app-in-intellij.png
[02]: ./media/app-service-web-debug-java-web-app-in-intellij/02-configure-intellij-remote-debug.png
[03]: ./media/app-service-web-debug-java-web-app-in-intellij/03-debug-configuration.png
[04]: ./media/app-service-web-debug-java-web-app-in-intellij/04-select-debug.png
[05]: ./media/app-service-web-debug-java-web-app-in-intellij/05-ready-for-remote-debugging.png
[06]: ./media/app-service-web-debug-java-web-app-in-intellij/06-windows-command-prompt-connection-successful-to-remote.png



<!--HONumber=Feb17_HO2-->


