<properties
	pageTitle="Erstellen einer Java-Web-App in Azure App Service | Microsoft Azure"
	description="In diesem Tutorial wird gezeigt, wie Sie eine Java-Web-App in Azure App Service bereitstellen."
	services="app-service\web"
	documentationCenter="java"
	authors="rmcmurray"
	manager="wpickett"
	editor=""/>

<tags
	ms.service="app-service-web"
	ms.workload="web"
	ms.tgt_pltfrm="na"
	ms.devlang="Java"
	ms.topic="get-started-article"
	ms.date="08/11/2016"
	ms.author="robmcm"/>

# Erstellen einer Java Web-App in Azure App Service

[AZURE.INCLUDE [Registerkarten](../../includes/app-service-web-get-started-nav-tabs.md)]

Dieses Tutorial zeigt die Erstellung einer [Java-Web-App in Azure App Service] über das [Azure-Portal]. Das Azure-Portal ist eine Weboberfläche, über die Sie Azure-Ressourcen verwalten können.

> [AZURE.NOTE] Sie benötigen ein Microsoft Azure-Konto, um dieses Tutorial auszuführen. Wenn Sie kein Konto haben, können Sie [Ihre Visual Studio-Abonnentenvorteile aktivieren] oder sich [für eine kostenlose Testversion registrieren].
>
> Wenn Sie Azure App Service ausprobieren möchten, ehe Sie sich für ein Azure-Konto anmelden, besuchen Sie [Azure App Service-App erstellen]. Dort können Sie direkt eine kurzlebige Starter-Web-App in App Service erstellen – keine Kreditkarte erforderlich, keine weiteren Verpflichtungen.

## Optionen für Java-Anwendungen

Es gibt verschiedene Möglichkeiten, wie Sie eine Java-Anwendung in einer App Service-Web-App einrichten können.

1. Erstellen einer App und anschließendes Konfigurieren der **Anwendungseinstellungen**

	App Service bietet verschiedene Tomcat- und Jetty-Versionen mit Standardkonfigurationen. Wenn die zu hostende Anwendung für eine der integrierten Versionen geeignet ist, ist diese Methode zur Einrichtung eines Webcontainers die einfachste Möglichkeit und auch die richtige Wahl, wenn Sie lediglich eine WAR-Datei in einen Webcontainer hochladen möchten. Bei dieser Methode erstellen Sie eine App im Azure-Portal und öffnen dann das Blatt **Anwendungseinstellungen** für Ihre App, um Ihre Version von Java und den gewünschten Java-Webcontainer auszuwählen. Bei Verwendung dieser Methode werden sowohl der Java-Code als auch Ihr Webcontainer über Programmdateien ausgeführt. Bei anderen Methoden werden der Webcontainer und ggf. auch die JVM in Ihren Festplattenspeicher eingefügt. Beim Einsatz dieses Modells fehlt Ihnen der Zugriff, um Dateien in diesem Teil des Dateisystems zu bearbeiten. Das heißt, Sie können z.B. nicht die Datei *server.xml* konfigurieren oder Bibliotheksdateien im Ordner */lib* ablegen. Weitere Informationen finden Sie weiter unten in diesem Tutorial unter [Erstellen und Konfigurieren einer Java-Web-App](#appsettings).
	
2. Verwenden einer Vorlage aus dem Azure Marketplace

	Der Azure Marketplace bietet Vorlagen, die automatisch Java-Web-Apps mit Tomcat- oder Jetty-Webcontainern erstellen und konfigurieren. Die durch die Vorlagen erstellten Webcontainer sind konfigurierbar. Weitere Informationen finden Sie in diesem Tutorial im Abschnitt [Verwenden einer Java-Vorlage aus dem Azure Marketplace](#marketplace).
  
3. Erstellen einer App und manuelles Kopieren und Bearbeiten der Konfigurationsdateien

	Unter Umständen möchten Sie eine benutzerdefinierte Java-Anwendung hosten, die sich in keinem der von App Service angebotenen Webcontainern bereitstellen lässt. Beispiel:
	
	* Ihre Java-Anwendung erfordert eine Version von Tomcat oder Jetty, die nicht direkt von App Service unterstützt oder nicht im Katalog angeboten wird.
	* Ihre Java-Anwendung nimmt HTTP-Anforderungen an und lässt sich nicht als WAR (Web Application Archive) in einem bereits vorhandenen Webcontainer bereitstellen.
	* Sie möchten den Webcontainer von Grund auf selbst konfigurieren.
	* Sie möchten eine Version von Java verwenden, die nicht von App Service unterstützt wird, und Sie möchten sie selbst hochladen.

	Für Fälle wie diese können Sie im Azure-Portal eine App erstellen und anschließend die geeigneten Laufzeitdateien manuell bereitstellen. Dabei werden die Dateien auf die Speicherplatzkontingente für Ihren App Service-Plan angerechnet. Weitere Informationen erhalten Sie unter [Hochladen einer benutzerdefinierten Java-Web-App in Azure].

## <a name="portal"></a> Erstellen und Konfigurieren einer Java-Web-App

In diesem Abschnitt wird gezeigt, wie Sie eine Web-App erstellen und mit dem Blatt **Anwendungseinstellungen** des Portals für Java konfigurieren.

1. Melden Sie sich beim [Azure-Portal] an.

2. Klicken Sie auf **Neu > Web und mobil > Web-App**.

	![Neue Web-App][newwebapp]

4. Geben Sie im Feld **Web-App** einen Namen für die Web-App ein.

	Der Name muss innerhalb der Domäne "azurewebsites.net" eindeutig sein, da die URL der Web-App "{name}.azurewebsites.net" lauten wird. Wenn der eingegebene Name nicht eindeutig ist, wird im Textfeld ein rotes Ausrufezeichen angezeigt.

5. Wählen Sie eine **Ressourcengruppe** aus, oder erstellen Sie eine neue.

	Weitere Informationen zu Ressourcengruppen finden Sie unter [Verwenden des Azure-Portals zum Verwalten Ihrer Azure-Ressourcen].

6. Wählen Sie einen **App Service-Plan/-Standort** aus, oder erstellen Sie einen neuen Plan und Standort.

	Weitere Informationen zu App Service-Plänen finden Sie unter [Azure App Service-Pläne – Detaillierte Übersicht].

7. Klicken Sie auf **Erstellen**.

	![Web-App erstellen][newwebapp2]
 
8. Nachdem die Web-App erstellt wurde, klicken Sie auf **Web-Apps > {Ihre Web-App}**.
 
	![Web-App auswählen][selectwebapp]

9. Klicken Sie auf dem Blatt **Web-App** auf **Einstellungen**.

10. Klicken Sie auf **Anwendungseinstellungen**.

11. Klicken Sie auf die gewünschte **Java-Version**.

12. Wählen Sie die gewünschte **Java-Nebenversion** aus. Wenn Sie **Neueste** auswählen, verwendet Ihre App die neueste Nebenversion, die in App Service für die betreffende Java-Hauptversion zur Verfügung steht. Das Element **Neueste** ist für Java-Apps eindeutig, die über die **Anwendungseinstellungen** erstellt werden. Wenn Sie Ihre Java-App über den Katalog erstellen, müssen Sie Ihren eigenen Container und die JVM-Änderungen verwalten.

12. Wählen Sie den gewünschten **Webcontainer** aus. Wenn Sie einen Containernamen auswählen, der mit **Neueste** beginnt, wird Ihre App auf der neuesten Version der Webcontainer-Hauptversion gehalten, die in App Service zur Verfügung steht.

	![Webcontainerversionen][versions]

13. Klicken Sie auf **Speichern**.

	In wenigen Augenblicken basiert Ihre Web-App auf Java und ist für die Verwendung des ausgewählten Webcontainers konfiguriert.

14. Klicken Sie auf **Web-Apps > {Ihre neue Web-App}**.

15. Klicken Sie auf die **URL**, um zur neuen Website zu wechseln.

	Diese Webseite bestätigt, dass Sie eine auf Java basierende Web-App erstellt haben.

## <a name="marketplace"></a> Verwenden einer Java-Vorlage aus dem Azure Marketplace

Dieser Abschnitt zeigt, wie der Azure Marketplace zum Erstellen einer Java-Web-App eingesetzt wird. Derselbe allgemeine Ablauf kann auch verwendet werden, um eine Java-basierte mobile App oder eine API-App zu erstellen.

1. Melden Sie sich beim [Azure-Portal] an.

2. Klicken Sie auf **Neu > Marketplace**.

	![Neuer Marketplace][newmarketplace]

3. Klicken Sie auf **Web und mobil**.

	Sie müssen möglicherweise nach links scrollen, um das Blatt **Marketplace** anzuzeigen, auf dem Sie **Web und mobil** auswählen können.

4. Geben Sie im Suchfeld den Namen eines Java-Anwendungsservers ein, beispielsweise **Apache Tomcat** oder **Jetty**, und drücken Sie dann die EINGABETASTE.

5. Klicken Sie in den Suchergebnissen auf den Java-Anwendungsserver.

	![Jetty: Web und mobil][webmobilejetty]

6. Klicken Sie im ersten **Apache Tomcat**- oder **Jetty**-Blatt auf **Erstellen**.

	![Blatt für das Jetty-Portal][jettyblade]

7. Geben Sie im nächsten **Apache Tomcat**- oder **Jetty**-Blatt im Feld **Web-App** einen Namen für die Web-App ein.

	Der Name muss innerhalb der Domäne "azurewebsites.net" eindeutig sein, da die URL der Web-App "{name}.azurewebsites.net" lauten wird. Wenn der eingegebene Name nicht eindeutig ist, wird im Textfeld ein rotes Ausrufezeichen angezeigt.

8. Wählen Sie eine **Ressourcengruppe** aus, oder erstellen Sie eine neue.

	Weitere Informationen zu Ressourcengruppen finden Sie unter [Verwenden des Azure-Portals zum Verwalten Ihrer Azure-Ressourcen].

9. Wählen Sie einen **App Service-Plan/-Standort** aus, oder erstellen Sie einen neuen Plan und Standort.

	Weitere Informationen zu App Service-Plänen finden Sie unter [Azure App Service-Pläne – Detaillierte Übersicht].

10. Klicken Sie auf **Erstellen**.

	![Jetty-Portal – Erstellen][jettyportalcreate2]

	Azure beendet die Erstellung der neuen Web-App innerhalb kurzer Zeit, üblicherweise in weniger als einer Minute.

11. Klicken Sie auf **Web-Apps > {Ihre neue Web-App}**.

12. Klicken Sie auf die **URL**, um zur neuen Website zu wechseln.

	![Jetty-URL][jettyurl]

	Tomcat wird mit einer Reihe von Standardseiten ausgeliefert. Wenn Sie Tomcat wählen, wird daher eine ähnliche Seite wie im folgenden Beispiel angezeigt:

	![Web-App mit Apache Tomcat][tomcat]

	Wenn Sie Jetty wählen, wird eine Seite ähnlich der folgenden angezeigt. Für Jetty ist keine Standardseite eingerichtet. Daher wird dieselbe JSP wiederverwendet, die für eine leere Java-Site verwendet wird.

	![Web-App mit Jetty][jetty]

Nachdem Sie nun die Web-App mit einem App-Container erstellt haben, finden Sie im Abschnitt [Nächste Schritte](#next-steps) weitere Informationen zum Hochladen Ihrer Anwendung in die Web-App.

## Nächste Schritte

Sie führen nun Ihren Java-Anwendungsserver in Ihrer Java-Web-App in Azure App Service aus. Informationen zum Bereitstellen Ihres eigenen Codes für die Web-App finden Sie unter [Hinzufügen einer Anwendung oder Webseite zu Ihrer Java-Web-App].

Weitere Informationen zum Entwickeln von Java-Anwendungen in Azure finden Sie im [Java Developer Center].

<!-- URL List -->

[Hinzufügen einer Anwendung oder Webseite zu Ihrer Java-Web-App]: ./web-sites-java-add-app.md
[Azure App Service-Pläne – Detaillierte Übersicht]: ../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md
[Azure-Portal]: https://portal.azure.com/
[Ihre Visual Studio-Abonnentenvorteile aktivieren]: http://go.microsoft.com/fwlink/?LinkId=623901
[für eine kostenlose Testversion registrieren]: http://go.microsoft.com/fwlink/?LinkId=623901
[Azure App Service-App erstellen]: http://go.microsoft.com/fwlink/?LinkId=523751
[Java-Web-App in Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Java Developer Center]: /develop/java/
[Verwenden des Azure-Portals zum Verwalten Ihrer Azure-Ressourcen]: ../azure-portal/resource-group-portal.md
[Hochladen einer benutzerdefinierten Java-Web-App in Azure]: ./web-sites-java-custom-upload.md

<!-- IMG List -->

[newwebapp]: ./media/web-sites-java-get-started/newwebapp.png
[newwebapp2]: ./media/web-sites-java-get-started/newwebapp2.png
[selectwebapp]: ./media/web-sites-java-get-started/selectwebapp.png
[versions]: ./media/web-sites-java-get-started/versions.png
[newmarketplace]: ./media/web-sites-java-get-started/newmarketplace.png
[webmobilejetty]: ./media/web-sites-java-get-started/webmobilejetty.png
[jettyblade]: ./media/web-sites-java-get-started/jettyblade.png
[jettyportalcreate2]: ./media/web-sites-java-get-started/jettyportalcreate2.png
[jettyurl]: ./media/web-sites-java-get-started/jettyurl.png
[tomcat]: ./media/web-sites-java-get-started/tomcat.png
[jetty]: ./media/web-sites-java-get-started/jetty.png

<!---HONumber=AcomDC_0817_2016-->