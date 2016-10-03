<properties 
	pageTitle="Debuggen einer Java-Web-App in Azure in Eclipse | Microsoft Azure" 
	description="Dieses Tutorial zeigt das Debuggen einer in Azure ausgeführten Java-Web-App mit dem Azure Toolkit für Eclipse." 
	services="app-service\web" 
	documentationCenter="java" 
	authors="selvasingh" 
	manager="wpickett" 
	editor=""/>

<tags 
	ms.service="app-service-web" 
	ms.workload="web" 
	ms.tgt_pltfrm="na" 
	ms.devlang="Java" 
	ms.topic="article" 
	ms.date="09/20/2016" 
	ms.author="asirveda;robmcm"/>

# Debuggen einer Java-Web-App in Azure in Eclipse

Dieses Tutorial zeigt das Debuggen einer in Azure ausgeführten Java-Web-App mit dem [Azure Toolkit für Eclipse]. Der Einfachheit halber verwenden Sie in diesem Tutorial ein einfaches JSP-Beispiel (Java Server Page). Doch beim Debuggen in Azure sind die Schritte für ein Java-Servlet ähnlich.

Nachdem Sie dieses Tutorial abgeschlossen haben, entspricht Ihre Anwendung, wenn Sie sie in Eclipse debuggen, etwa der folgenden Abbildung:

![][01]
 
## Voraussetzungen

* Ein Java Developer Kit (JDK), Version 1.8 oder höher.
* Eclipse IDE für Java EE-Entwickler, Indigo oder höher. Dies kann von <http://www.eclipse.org/downloads/> heruntergeladen werden.
* Eine Verteilung eines Java-basierten Webservers oder Anwendungsservers, wie z. B. Apache Tomcat oder Jetty.
* Ein Azure-Abonnement, das von <https://azure.microsoft.com/free/> oder <http://azure.microsoft.com/pricing/purchase-options/> bezogen werden kann.
* Das Azure-Toolkit für Eclipse. Weitere Informationen finden Sie unter [Installation des Azure Toolkit für Eclipse].
* Ein in Azure App Service erstelltes und bereitgestelltes dynamisches Webprojekt. Ein Beispiel finden Sie unter [Erstellen einer „Hello World“-Web-App für Azure in Eclipse].

## Debuggen einer Java-Web-App in Azure

Zum Ausführen der Schritte in diesem Abschnitt können Sie ein vorhandenes dynamisches Webprojekt nutzen, das Sie bereits als Java-Web-App in Azure bereitgestellt haben. Laden Sie ein [Beispiel eines dynamischen Webprojekts] herunter, und befolgen Sie die Anweisungen unter [Erstellen einer „Hello World“-Web-App für Azure in Eclipse], um diese in Azure bereitzustellen.

1. Öffnen Sie Eclipse.

1. Konfigurieren Sie Timeouts für das Remotedebuggen:

    1. Klicken Sie in Eclipse auf das Menü **Windows** und dann auf **Preferences** (Voreinstellungen).
    1. Erweitern Sie den Knoten **Java**, wählen Sie dann **Debug** aus.
    1. Legen Sie die Einstellungen **Debugger timeout (ms)** (Debuggertimeout) und **Launch timeout (ms)** (Starttimeout) auf `120000` fest.

        ![][02]

    1. Klicken Sie auf **OK**, um das Dialogfeld **Preferences** (Voreinstellungen) zu schließen.

1. Klicken Sie in Eclipse in der Projektexplorer-Ansicht mit der rechten Maustaste auf das dynamische Webprojekt, das Sie in Azure bereitgestellt haben. Wählen Sie im eingeblendeten Kontextmenü **Debug as** (Debuggen als) aus, und klicken Sie dann auf **Azure Web App**.

    ![][03]

1. Wenn Sie Ihr dynamisches Webprojekt erstmals debuggen, wird das Dialogfeld **Debug Configurations** (Debugkonfigurationen) geöffnet. Sie können die vom Toolkit angegeben Standardwerte übernehmen, die auf der Registerkarte **Connect** (Verbinden) angegeben sind. Klicken Sie auf der Registerkarte **Source** (Quelle) auf **Add** (Hinzufügen). Wählen Sie zunächst **Java project** (Java-Projekt) und dann **Dynamic Web Project** (Dynamisches Webprojekt) aus, und klicken Sie auf **OK**. Nachdem Sie diese Schritte abgeschlossen haben, klicken Sie auf **Debug**.

    ![][04]

1. Bei der Frage **Enable remote debugging in the remote Web App now?** (Remotedebuggen in Remote-Web-App jetzt aktivieren?) klicken Sie auf **OK**.

1. Wenn **Your web app is now ready for remote debugging** (Ihre Web-app ist jetzt für das Remotedebuggen bereit) angezeigt wird, klicken Sie auf **OK**.

    ![][05]

1. Wenn das Dialogfeld **Debug Configurations** (Debugkonfigurationen) erneut angezeigt wird, klicken Sie auf **Debug**.

1. Ein Windows-Eingabeaufforderung oder eine Unix-Shell wird geöffnet und die für das Debuggen erforderliche Verbindung vorbereitet. Sie müssen warten, bis die Verbindung mit Ihrer Java-Remote-Web-App hergestellt wurde, ehe Sie fortfahren können. Unter Windows sieht dies wie in der folgenden Abbildung aus.

    ![][06]

1. Fügen Sie auf Ihrer JSP-Seite einen Haltepunkt ein, und öffnen Sie dann in einem Browser die URL zu Ihrer Java-Web-App:

    1. Öffnen Sie **Azure Explorer** in Eclipse.
    1. Navigieren Sie zu **Web Apps** und zur Java-Web-App, die Sie debuggen möchten.
    1. Klicken Sie mit der rechten Maustaste auf die Web-App, und klicken Sie dann auf **Open in Browser** (Im Browser öffnen).
    1. Eclipse wechselt jetzt in den Debugmodus.

## Nächste Schritte

Weitere Informationen zum Verwenden von Azure mit Java finden Sie im [Azure Java Developer Center].

Weitere Informationen zum Erstellen von Azure-Web-Apps finden Sie unter [Web-Apps – Übersicht].

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Azure Toolkit für Eclipse]: ../azure-toolkit-for-eclipse.md
[Installation des Azure Toolkit für Eclipse]: ../azure-toolkit-for-eclipse-installation.md
[Erstellen einer „Hello World“-Web-App für Azure in Eclipse]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Beispiel eines dynamischen Webprojekts]: http://go.microsoft.com/fwlink/?LinkId=817337

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Web-Apps – Übersicht]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-debug-java-web-app-in-eclipse/01-debug-java-web-app-in-eclipse.png
[02]: ./media/app-service-web-debug-java-web-app-in-eclipse/02-configure-eclipse-remote-debug.png
[03]: ./media/app-service-web-debug-java-web-app-in-eclipse/03-debug-as.png
[04]: ./media/app-service-web-debug-java-web-app-in-eclipse/04-debug-configurations.png
[05]: ./media/app-service-web-debug-java-web-app-in-eclipse/05-ready-for-remote-debugging.png
[06]: ./media/app-service-web-debug-java-web-app-in-eclipse/06-windows-command-prompt-connection-successful-to-remote.png

<!---HONumber=AcomDC_0921_2016-->