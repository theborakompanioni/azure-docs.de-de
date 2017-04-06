---
title: Problembehandlung von Web-Apps in Azure App Service in Visual Studio
description: "Erfahren Sie mehr über die Problembehandlung für eine Azure-Web-App mithilfe von Remotedebugging-, Ablaufverfolgungs- und Protokollierungstools, die in Visual Studio 2013 integriert sind."
services: app-service
documentationcenter: .net
author: tdykstra
manager: erikre
editor: 
ms.assetid: def8e481-7803-4371-aa55-64025d116c97
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/29/2016
ms.author: rachelap
translationtype: Human Translation
ms.sourcegitcommit: fcbd9e10e4cc336dc6ea37f84201249e14b1af91
ms.openlocfilehash: d22c9e2026c3efc63c5a3baa7ad4505aa269e31d
ms.lasthandoff: 02/16/2017


---
# <a name="troubleshoot-a-web-app-in-azure-app-service-using-visual-studio"></a>Problembehandlung von Web-Apps in Azure App Service in Visual Studio
## <a name="overview"></a>Übersicht
In diesem Tutorial lernen Sie die Visual Studio-Tools zum Debuggen von Web-Apps in [App Service](http://go.microsoft.com/fwlink/?LinkId=529714) kennen. Sie führen die App entweder remote im [Debugmodus](http://www.visualstudio.com/get-started/debug-your-app-vs.aspx) aus oder arbeiten mit Anwendungs- und Webserverprotokollen.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

Sie lernen Folgendes:

* Welche Azure-Web-App-Verwaltungsfunktionen sind in Visual Studio verfügbar?
* Verwenden der Remoteansicht in Visual Studio für schnelle Änderungen an Remote-Web-Apps
* Ausführen des Debugmodus im Remotebetrieb, während ein Projekt in Azure ausgeführt wird, sowohl für eine Web-App als auch für einen WebJob
* Erstellen von Anwendungs-Ablaufprotokollen und Live-Anzeige dieser Protokolle.
* Anzeige von Webserverprotokollen inklusive detaillierter Fehlermeldungen und Verfolgung fehlgeschlagener Anforderungen.
* Senden von Diagnoseprotokollen an ein Azure-Speicherkonto und Anzeige der Protokolle.

Wenn Sie über Visual Studio Ultimate verfügen, können Sie auch [IntelliTrace](http://msdn.microsoft.com/library/vstudio/dd264915.aspx) zum Debuggen verwenden. IntelliTrace wird in diesem Lernprogramm nicht behandelt.

## <a name="prerequisites"></a>Voraussetzungen
Dieses Tutorial verwendet die Entwicklungsumgebung, das Webprojekt und die Azure-Web-App, die Sie unter [Erste Schritte mit Azure und ASP.NET][GetStarted] eingerichtet haben. Für die Abschnitte zu Webaufträgen benötigen Sie die Anwendung, die Sie in [Erste Schritte mit dem Azure WebJobs SDK][GetStartedWJ] erstellen.

Die Codebeispiele in diesem Lernprogramm stammen aus einer C# MVC-Webanwendung. Die Prozeduren gelten jedoch auch für die Problembehandlung in Visual Basic- und Web Forms-Anwendungen.

Dieses Tutorial setzt voraus, dass Sie Visual Studio 2015 oder 2013 verwenden. Wenn Sie Visual Studio 2013 verwenden, ist für die WebJobs-Features [Update 4](http://go.microsoft.com/fwlink/?LinkID=510314) oder höher erforderlich.

Die Streamingprotokoll-Funktion funktioniert nur für Anwendungen, die das .NET Framework 4 oder später verwenden.

## <a name="sitemanagement"></a>Web-App-Konfiguration und -Verwaltung
Visual Studio bietet Zugriff auf einen Teil der Web-App-Verwaltungsfunktionen und Konfigurationseinstellungen, die im [Azure-Portal](http://go.microsoft.com/fwlink/?LinkId=529715)verfügbar sind. In diesem Abschnitt lernen Sie die Optionen kennen, die bei Verwendung des **Server-Explorers**verfügbar sind. Um die neuesten Azure-Integrationsfeatures kennen zu lernen, probieren Sie auch den **Cloud-Explorer** aus. Sie können beide Fenster über das Menü **Ansicht** öffnen.

1. Falls Sie noch nicht mit Visual Studio in Azure angemeldet sind, klicken Sie auf die Schaltfläche **Mit Azure verbinden** im **Server-Explorer**.

    Als Alternative können Sie ein Verwaltungszertifikat installieren, das den Zugriff auf Ihr Konto ermöglicht. Wenn Sie ein Zertifikat installieren möchten, klicken Sie im **Server-Explorer** mit der rechten Maustaste auf den Knoten **Azure**, und klicken Sie im Kontextmenü auf **Abonnements verwalten und filtern**. Klicken Sie im Dialogfeld **Azure-Abonnements verwalten** auf die Registerkarte **Zertifikate** und dann auf **Importieren**. Befolgen Sie die Anweisungen zum Herunterladen und Importieren einer Abonnementdatei (auch *.publishsettings* -Datei genannt) für Ihr Azure-Konto.

   > [!NOTE]
   > Wenn Sie eine Abonnementdatei herunterladen, sollten Sie diese in einem Ordner außerhalb Ihrer Quellcodeverzeichnisse speichern (beispielsweise im Ordner "Downloads") und nach Abschluss des Importvorgangs löschen. Böswillige Benutzer, die Zugriff auf die Abonnementdatei erlangen, können Ihre Azure-Services bearbeiten, erstellen und löschen.
   >
   >

    Weitere Informationen für Verbindungen zwischen Visual Studio und Azure-Ressourcen finden Sie unter [Verwalten von Konten, Abonnements und Administratorrollen](http://go.microsoft.com/fwlink/?LinkId=324796#BKMK_AccountVCert).
2. Erweitern Sie im **Server-Explorer** den Knoten **Azure**, und erweitern Sie dann **App Service**.
3. Erweitern Sie die Ressourcengruppe, die die Web-App enthält, die Sie in [Erste Schritte mit Azure und ASP.NET][GetStarted] erstellt haben, klicken Sie mit der rechten Maustaste auf den Web-App-Knoten, und klicken Sie dann auf **Anzeigeeinstellungen**.

    ![Anzeigeeinstellungen im Server-Explorer](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-viewsettings.png)

    Die Registerkarte **Azure-Web-App** wird angezeigt, und Sie sehen dort die in Visual Studio verfügbaren Aufgaben für die Web-App-Verwaltung und -Konfiguration.

    ![Fenster "Azure-Web-App"](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-configtab.png)

    In diesem Lernprogramm werden Sie die Auswahllisten für Protokollierung und Ablaufverfolgung verwenden. Außerdem werden Sie Remotedebugging verwenden, allerdings wird diese Option auf andere Weise aktiviert.

    Weitere Informationen zu den Feldern für App-Einstellungen und Verbindungszeichenfolgen in diesem Fenster finden Sie unter [Azure-Web-Apps: Funktionsweise von Anwendungs- und Verbindungszeichenfolgen](http://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx).

    Falls Sie eine Web-App-Verwaltungsaufgabe ausführen möchten, die in diesem Fenster nicht ausgeführt werden kann, klicken Sie auf **Im Verwaltungsportal öffnen** klicken, um ein Browserfenster für das Azure-Portal zu öffnen.

## <a name="remoteview"></a>Zugreifen auf Web-App-Dateien im Server-Explorer
Das `customErrors`-Kennzeichen in der Datei "Web.config" ist bei der Bereitstellung eines Webprojekts üblicherweise auf `On` oder `RemoteOnly` festgelegt. Das bedeutet, dass Sie bei einem Problem keine erklärende Fehlermeldung erhalten. Für viele Fehler bekommen Sie eine der folgenden Seiten angezeigt.

**Serverfehler in '/'-Anwendung:**

![Wenig hilfreiche Fehlerseite](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror.png)

**Ein Fehler ist aufgetreten:**

![Wenig hilfreiche Fehlerseite](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror1.png)

**Die Website kann die Seite nicht anzeigen**

![Wenig hilfreiche Fehlerseite](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror2.png)

Häufig ist der einfachste Weg für die Suche nach der Fehlerursache die Aktivierung detaillierter Fehlermeldungen. Im ersten der vorherigen Screenshots wird dies erläutert. Dazu ist eine Änderung an der bereitgestellten Web.config-Datei erforderlich. Sie können die Datei *Web.config* entweder im Projekt bearbeiten und das Projekt neu bereitstellen oder eine [Web.config-Transformation](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/web-config-transformations) erstellen und eine Debugversion bereitstellen. Es gibt aber noch einen einfacheren Weg: Im **Projektmappen-Explorer** können Sie Dateien in der Remote-Web-App mithilfe des *Remoteansicht*-Features direkt anzeigen und bearbeiten.

1. Erweitern Sie im **Server-Explorer** den Knoten **Azure**, erweitern Sie **App Service**, erweitern Sie die Ressourcengruppe, in der sich Ihre Web-App befindet, und erweitern Sie dann den Knoten für Ihre Web-App.

    Daraufhin werden Knoten angezeigt, über die Sie auf die Inhalts- und Protokolldateien der Web-App zugreifen können.
2. Erweitern Sie den Knoten **Dateien** und doppelklicken Sie auf die Datei *Web.config* .

    ![Öffnen der Datei Web.config](./media/web-sites-dotnet-troubleshoot-visual-studio/webconfig.png)

    Visual Studio öffnet die Datei "Web.config" in der Remote-Web-App und zeigt den Text "[Remote]" neben dem Dateinamen in der Titelleiste an.
3. Fügen Sie dem `system.web` -Element die folgende Zeile hinzu:

    `<customErrors mode="Off"></customErrors>`

    ![Bearbeiten der Datei Web.config](./media/web-sites-dotnet-troubleshoot-visual-studio/webconfigedit.png)
4. Aktualisieren Sie den Browser, der die wenig hilfreiche Fehlermeldung anzeigt, und Sie erhalten eine detailliertere Fehlermeldung, wie im folgenden Beispiel gezeigt:

    ![Detaillierte Fehlermeldung](./media/web-sites-dotnet-troubleshoot-visual-studio/detailederror.png)

    (Dieser Fehler wurde durch Hinzufügen der in rot angezeigten Zeile zu *Views\Home\Index.cshtml* erstellt.)

Änderungen an der Datei "Web.config" sind nur eines der Szenarien, in denen die Möglichkeit zum Lesen und Bearbeiten der Dateien in Ihrer Azure-Web-App die Problembehandlung erleichtert.

## <a name="remotedebug"></a>Remotedebuggen von Web-Apps
Falls die detaillierte Fehlermeldung nicht genügend Informationen liefert und sich der Fehler nicht lokal reproduzieren lässt, können Sie die Website remote im Debugmodus ausführen. Im Debugmodus können Sie Breakpoints setzen, den Speicher direkt manipulieren, Code schrittweise durchlaufen und sogar den Codepfad ändern.

Remotedebuggen funktioniert nicht in den Express-Editionen von Visual Studio.

Dieser Abschnitt zeigt, wie Sie das in [Erste Schritte mit Azure und ASP.NET][GetStarted] erstellte Projekt remote debuggen können.

1. Öffnen Sie das Webprojekt, das Sie in [Erste Schritte mit Azure und ASP.NET][GetStarted] erstellt haben.
2. Öffnen Sie *Controllers\HomeController.cs*.
3. Löschen Sie die `About()` -Methode, und fügen Sie stattdessen den folgenden Code ein.

        public ActionResult About()
        {
            string currentTime = DateTime.Now.ToLongTimeString();
            ViewBag.Message = "The current time is " + currentTime;
            return View();
        }
4. [Setzen Sie einen Haltepunkt](http://www.visualstudio.com/get-started/debug-your-app-vs.aspx) in der Zeile: `ViewBag.Message`.
5. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und klicken Sie anschließend auf **Veröffentlichen**.
6. Wählen Sie in der Dropdownliste **Profil** dasselbe Profil aus, das Sie in [Erste Schritte mit Azure und ASP.NET][GetStarted] verwendet haben.
7. Wechseln Sie zur Registerkarte **Einstellungen**, ändern Sie **Konfiguration** in **Debug**, und klicken Sie anschließend auf **Veröffentlichen**.

    ![Veröffentlichen im Debugmodus](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-publishdebug.png)
8. Nachdem die Bereitstellung abgeschlossen ist und Ihr Browser die Azure-URL Ihrer Web-App öffnet, schließen Sie den Browser.
9. Klicken Sie im **Server-Explorer** mit der rechten Maustaste auf Ihre Web-App, und klicken Sie dann auf **Debugger anfügen**.

    ![Debugger anfügen](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-attachdebugger.png)

    Der Browser öffnet automatisch Ihre Startseite in Azure. Möglicherweise müssen Sie ca. 20 Sekunden warten, während Azure die den Server zum Debuggen einrichtet. Diese Verzögerung tritt nur bei der ersten Ausführung einer Web-App im Debugmodus auf. Bei weiteren Ausführungen in den folgenden 48 Stunden tritt keine Verzögerung auf.

    **Hinweis:** Wenn beim Starten des Debuggers Probleme auftreten, versuchen Sie es über den **Cloud-Explorer** anstelle des **Server-Explorers**.
10. Klicken Sie im Menü auf **Info** .

     Visual Studio hält am Breakpoint an, wobei der Code nicht auf Ihrem lokalen Computer läuft, sondern unter Azure.
11. Zeigen Sie auf die Variable `currentTime` , um den Zeitwert anzuzeigen.

     ![Anzeigen von Variablen im Debugmodus in Azure](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-debugviewinwa.png)

     Bei der angezeigten Zeit handelt es sich um die Azure-Serverzeit, deren Zeitzone sich von Ihrer lokalen Einstellung unterscheiden kann.
12. Geben Sie einen neuen Wert für die Variable `currentTime` ein, z. B. "Ausführung unter Azure".
13. Drücken Sie F5, um die Ausführung fortzusetzen.

     Die Info-Seite unter Azure zeigt daraufhin den neuen Wert an, den Sie für die Variable currentTime eingegeben haben.

     ![Info-Seite mit neuem Wert](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-debugchangeinwa.png)

## <a name="remotedebugwj"></a> Remotedebuggen von WebJobs
Dieser Abschnitt zeigt, wie Sie das Projekt und die Web-App, die Sie in [Erste Schritte mit dem Azure WebJobs SDK](websites-dotnet-webjobs-sdk.md)erstellen, remote debuggen können.

Die in diesem Abschnitt dargestellten Funktionen sind nur in Visual Studio 2013 mit Update 4 oder höher verfügbar.

Remotedebuggen funktioniert nur bei kontinuierlichen WebJobs. Geplante und bedarfsabhängige WebJobs unterstützen Debuggen nicht.

1. Öffnen Sie das Webprojekt, das Sie ihn [Erste Schritte mit dem Azure WebJobs SDK][GetStartedWJ] erstellt haben.
2. Öffnen Sie *Functions.cs*im Projekt "ContosoAdsWebJob".
3. [Legen Sie einen Haltepunkt](http://www.visualstudio.com/get-started/debug-your-app-vs.aspx) für die erste Anweisung in der `GnerateThumbnail`-Methode fest.

    ![Haltepunkt setzen](./media/web-sites-dotnet-troubleshoot-visual-studio/wjbreakpoint.png)
4. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Webprojekt (nicht das WebJob-Projekt), und klicken Sie auf **Veröffentlichen**.
5. Wählen Sie in der Dropdownliste **Profil** das gleiche Profil aus, das Sie in [Erste Schritte mit dem Azure WebJobs SDK](websites-dotnet-webjobs-sdk.md)verwendet haben.
6. Wechseln Sie zur Registerkarte **Einstellungen**, ändern Sie **Konfiguration** in **Debug**, und klicken Sie anschließend auf **Veröffentlichen**.

    Visual Studio stellt die Web- und WebJob-Projekte bereit, und in Ihrem Browser wird die Azure-URL Ihrer Web-App angezeigt.
7. Erweitern Sie im **Server-Explorer** nacheinander **Azure > App Service > Ihre Ressourcengruppe > Ihre Web-App > WebJobs > Fortlaufend**, und klicken Sie dann mit der rechten Maustaste auf **ContosoAdsWebJob**.
8. Klicken Sie auf **Debugger anfügen**.

    ![Debugger anfügen](./media/web-sites-dotnet-troubleshoot-visual-studio/wjattach.png)

    Der Browser öffnet automatisch Ihre Startseite in Azure. Möglicherweise müssen Sie ca. 20 Sekunden warten, während Azure die den Server zum Debuggen einrichtet. Diese Verzögerung tritt nur bei der ersten Ausführung einer Web-App im Debugmodus auf. Wenn Sie den Debugger das nächste Mal anfügen, gibt es keine Verzögerung, wenn dies innerhalb der nächsten 48 Stunden geschieht.
9. Erstellen Sie in dem Webbrowser, in dem die Contoso Ads-Startseite geöffnet ist, eine neue Anzeige.

    Durch Erstellen einer Anzeige wird eine Warteschlangennachricht erstellt, die vom WebJob übernommen und bearbeitet wird. Wenn das WebJobs SDK die Funktion zur Verarbeitung der Warteschlangennachricht aufruft, erreicht der Code den Breakpoint.
10. Wenn der Debugger am Breakpoint unterbrochen wird, können Sie Variablenwerte überprüfen und ändern, während das Programm die Cloud ausführt. In der folgenden Abbildung zeigt der Debugger den Inhalt des blobInfo-Objekts an, das an die GenerateThumbnail-Methode übergeben wurde.

     ![BlobInfo-Objekt im Debugger](./media/web-sites-dotnet-troubleshoot-visual-studio/blobinfo.png)
11. Drücken Sie F5, um die Ausführung fortzusetzen.

     Die "GenerateThumbnail"-Methode schließt die Erstellung der Miniaturansicht ab.
12. Aktualisieren Sie im Browser die Indexseite, um die Miniaturansicht anzuzeigen.
13. Drücken Sie in Visual Studio UMSCHALT + F5, um das Debuggen zu beenden.
14. Klicken Sie im **Server-Explorer** mit der rechten Maustaste auf den Knoten „ContosoAdsWebJob“, und klicken Sie dann auf **Dashboard anzeigen**.
15. Melden Sie sich mit Ihren Azure-Anmeldeinformationen an, und klicken Sie dann auf den WebJob-Namen, um zu der Seite für den WebJob zu wechseln.

     ![Auf ContosoAdsWebJob klicken](./media/web-sites-dotnet-troubleshoot-visual-studio/clickcaw.png)

     Das Dashboard zeigt, dass die "GenerateThumbnail"-Funktion vor kurzem ausgeführt wurde.

     (Wenn Sie nächstes Mal auf **Dashboard anzeigen**klicken, müssen Sie sich nicht anmelden, und der Browser wechselt direkt zur Seite für Ihren WebJob.)
16. Klicken Sie auf den Funktionsnamen, um Details zur Ausführung der Funktion anzuzeigen.

     ![Funktionsdetails](./media/web-sites-dotnet-troubleshoot-visual-studio/funcdetails.png)

Wenn die Funktion [Protokolle geschrieben hat](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs), können Sie auf **ToggleOutput** klicken, um sie anzuzeigen.

## <a name="notes-about-remote-debugging"></a>Hinweise zum Remotedebuggen
* Vermeiden Sie es, den Debugmodus in Produktion einzusetzen. Wenn Ihre Produktions-Web-App nicht auf mehrere Serverinstanzen dezentral skaliert wird, verhindert das Debuggen, dass der Webserver auf andere Anforderungen reagiert. Falls Sie mehrere Webserverinstanzen betreiben, erhalten Sie beim Anhängen des Debuggers eine zufällige Instanz, und Sie können nicht garantieren, dass Ihre Browseranforderungen an die richtige Instanz gehen. Außerdem ist es unüblich, Debugversionen in Produktion bereitzustellen, und Compileroptimierungen für Releaseversionen verhindern die zeilenweise Anzeige der Vorgänge in Ihrem Quellcode. Die beste Möglichkeit zur Problembehandlung von Produktionsproblemen sind Ablaufverfolgung und Webserverprotokolle.
* Vermeiden Sie es beim Remotedebuggen, lange an Breakpoints anzuhalten. Azure behandelt Prozesse, die länger als einige Minuten angehalten sind, als nicht reagierend und beendet diese.
* Beim Debuggen schickt der Server Daten an Visual Studio und verursacht möglicherweise zusätzliche Kosten für Bandbreite. Weitere Informationen zu Bandbreitentarifen finden Sie unter [Azure Pricing](https://azure.microsoft.com/pricing/calculator/)(Azure-Preisübersicht, in englischer Sprache).
* Stellen Sie sicher, dass das `debug`-Attribut im `compilation`-Element in der Datei *Web.config* auf „true“ festgelegt ist. Dieses Attribut ist beim Veröffentlichen einer Debug-Buildkonfiguration standardmäßig "true".

        <system.web>
          <compilation debug="true" targetFramework="4.5" />
          <httpRuntime targetFramework="4.5" />
        </system.web>
* Falls der Debugger den gewünschten Code nicht schrittweise ausführt, müssen Sie möglicherweise die Einstellung "Nur eigenen Code" ändern.  Weitere Informationen finden Sie unter [Schrittweises Durchlaufen für "Nur mein Code" beschränken](http://msdn.microsoft.com/library/vstudio/y740d9d3.aspx#BKMK_Restrict_stepping_to_Just_My_Code).
* Bei Aktivierung der Remotedebuggen-Funktion startet ein Timer auf dem Server, und die Funktion wird nach 48 Stunden automatisch abgeschaltet. Dieses Limit von 48 Stunden existiert aus Sicherheits- und Leistungsgründen. Sie können die Funktion jederzeit und beliebig oft aktivieren. Wenn Sie nicht aktiv debuggen, sollten Sie die Funktion jedoch deaktivieren.
* Sie können den Debugger manuell an einen beliebigen Prozess anfügen, nicht nur an den Web-App-Prozess (w3wp.exe). Weitere Informationen zum Debugmodus in Visual Studio finden Sie unter [Debuggen in Visual Studio](http://msdn.microsoft.com/library/vstudio/sc65sadd.aspx).

## <a name="logsoverview"></a>Übersicht über Diagnoseprotokolle
ASP.NET-Anwendungen in Azure-Web-Apps können die folgenden Arten von Protokollen generieren:

* **Anwendungsnachverfolgungsprotokolle**<br/>
  Anwendungen erzeugen diese Protokolle, indem sie Methoden der Klasse [System.Diagnostics.Trace](http://msdn.microsoft.com/library/system.diagnostics.trace.aspx) aufrufen.
* **Webserverprotokolle**<br/>
  Der Webserver erstellt einen Protokolleintrag für jede HTTP-Anforderung an die Web-App.
* **Ausführliche Fehlerprotokolle**<br/>
  Der Webserver erstellt eine HTML-Seite mit zusätzlichen Informationen für fehlgeschlagene HTTP-Anforderungen (Anforderungen mit einem Statuscode 400 oder höher).
* **Nachverfolgungsprotokolle für Anforderungen mit Fehlern**<br/>
  Der Webserver erstellt eine XML-Datei mit detaillierten Ablaufverfolgungsinformationen für fehlgeschlagene HTTP-Anforderungen. Der Webserver liefert außerdem eine XSL-Datei zur Formatierung der XML-Datei in einem Browser.

Protokollierung kann die Leistung von Web-Apps beeinträchtigen. Daher können Sie die verschiedenen Protokolltypen unter Azure bei Bedarf einzeln aktivieren und deaktivieren. Für Anwendungsprotokolle können Sie angeben, dass nur Protokolleinträge oberhalb eines bestimmten Schweregrads geschrieben werden sollen. Bei der Erstellung neuer Web-Apps ist sämtliche Protokollierung standardmäßig deaktiviert.

Die Protokolle werden in den Ordner *LogFiles* im Dateisystem Ihrer Web-App geschrieben und sind über FTP zugänglich. Webserver- und Anwendungsprotokolle können auch in ein Azure-Speicherkonto geschrieben werden. Speicherkonten bieten mehr Kapazität für Protokolle als das Dateisystem. Protokolle im Dateisystem sind beschränkt auf 100 Megabyte. (Protokolle im Dateisystem werden nur für kurze Zeit aufbewahrt. Azure löscht alte Protokolldateien, um Platz für neue Dateien zu machen, wenn das Limit erreicht ist.)  

## <a name="apptracelogs"></a>Erstellen und Anzeigen von Anwendungs-Ablaufprotokollen
In diesem Abschnitt werden Sie die folgenden Aufgaben ausführen:

* Fügen Sie dem Webprojekt, das Sie in [Erste Schritte mit Azure und ASP.NET][GetStarted] erstellt haben, Ablaufverfolgungsanweisungen hinzu.
* Anzeigen der Protokolle, wenn Sie das Projekt lokal ausführen.
* Anzeigen der Protokolle, während diese von der Anwendung unter Azure generiert werden.

Informationen zum Erstellen von Anwendungsprotokollen in WebJobs finden Sie unter [Verwenden des Azure-Warteschlangenspeichers mithilfe des WebJobs SDK - Schreiben von Protokollen](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs). Die folgenden Anweisungen für das Anzeigen von Protokollen und das Speichern in Azure gelten auch für Anwendungsprotokolle, die von WebJobs erstellt werden.

### <a name="add-tracing-statements-to-the-application"></a>Hinzufügen von Ablaufverfolgungs-Anweisungen zur Anwendung
1. Öffnen Sie *Controllers\HomeController.cs*, und ersetzen Sie die Methoden `Index`, `About`, und `Contact` durch den folgenden Code, um `Trace`-Anweisungen und eine `using`-Anweisung für `System.Diagnostics` hinzuzufügen:

        public ActionResult Index()
        {
            Trace.WriteLine("Entering Index method");
            ViewBag.Message = "Modify this template to jump-start your ASP.NET MVC application.";
            Trace.TraceInformation("Displaying the Index page at " + DateTime.Now.ToLongTimeString());
            Trace.WriteLine("Leaving Index method");
            return View();
        }

        public ActionResult About()
        {
            Trace.WriteLine("Entering About method");
            ViewBag.Message = "Your app description page.";
            Trace.TraceWarning("Transient error on the About page at " + DateTime.Now.ToShortTimeString());
            Trace.WriteLine("Leaving About method");
            return View();
        }

        public ActionResult Contact()
        {
            Trace.WriteLine("Entering Contact method");
            ViewBag.Message = "Your contact page.";
            Trace.TraceError("Fatal error on the Contact page at " + DateTime.Now.ToLongTimeString());
            Trace.WriteLine("Leaving Contact method");
            return View();
        }        
2. Fügen Sie oben in der Datei eine `using System.Diagnostics;` -Anweisung hinzu.

### <a name="view-the-tracing-output-locally"></a>Lokale Anzeige der Ablaufverfolgungs-Ausgabe
1. Drücken Sie F5, um die Anwendung im Debugmodus auszuführen.

    Der Standard-Ablaufverfolgungs-Listener schreibt sämtliche Ausgaben in das **Ausgabefenster** , zusammen mit anderen Debugausgaben. Die folgende Abbildung zeigt die Ausgaben der Ablaufverfolgungsanweisungen, die Sie der `Index` -Methode hinzugefügt haben.

    ![Ablaufverfolgung im Debugfenster](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-debugtracing.png)

    Anhand der folgenden Schritte können Sie Ablaufverfolgungs-Ausgaben in einer Webseite anzeigen, ohne im Debugmodus zu kompilieren.
2. Öffnen Sie die Web.config-Datei der Anwendung (die Datei im Projektordner) und fügen Sie das Element `<system.diagnostics>` am Ende der Datei direkt vor dem abschließenden `</configuration>`-Element ein:

          <system.diagnostics>
            <trace>
              <listeners>
                <add name="WebPageTraceListener"
                    type="System.Web.WebPageTraceListener,
                    System.Web,
                    Version=4.0.0.0,
                    Culture=neutral,
                    PublicKeyToken=b03f5f7f11d50a3a" />
              </listeners>
            </trace>
          </system.diagnostics>

    Über `WebPageTraceListener` können Sie die Ausgabe der Ablaufverfolgung anzeigen, indem Sie zu `/trace.axd` navigieren.
3. Fügen Sie ein <a href="http://msdn.microsoft.com/library/vstudio/6915t83k(v=vs.100).aspx">trace-Element</a> unter `<system.web>` in die Web.config-Datei ein, wie im folgenden Beispiel gezeigt:

        <trace enabled="true" writeToDiagnosticsTrace="true" mostRecent="true" pageOutput="false" />
4. Drücken Sie STRG+F5, um die Anwendung auszuführen.
5. Fügen Sie der URL in der Adressleiste *trace.axd* hinzu, und drücken Sie die Eingabetaste (die URL sollte dieser ähneln: „http://localhost:53370/trace.axd“).
6. Klicken Sie auf der Seite **Anwendungsablaufverfolgung** auf **Details anzeigen** in der ersten Zeile (nicht in der BrowserLink-Zeile).

    ![trace.axd](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-traceaxd1.png)

    Daraufhin wird die Seite **Anforderungsdetails** angezeigt, und im Bereich **Überwachungsinformationen** sehen Sie die Ausgabe der Ablaufverfolgungs-Anweisungen, die Sie der `Index`-Methode hinzugefügt haben.

    ![trace.axd](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-traceaxd2.png)

    `trace.axd` ist standardmäßig nur lokal verfügbar. Sie können `localOnly="false"` dem `trace`-Element in der Datei *Web.config* hinzufügen, um die Datei in einer Remote-Web-App verfügbar zu machen, wie im folgenden Beispiel gezeigt:

        <trace enabled="true" writeToDiagnosticsTrace="true" localOnly="false" mostRecent="true" pageOutput="false" />

    Es wird jedoch aus Sicherheitsgründen in der Regel nicht empfohlen, `trace.axd` in Produktions-Web-Apps zu aktivieren. In den folgenden Abschnitten lernen Sie einen einfacheren Weg kennen, um Ablaufverfolgungsprotokolle in Azure-Web-Apps zu lesen.

### <a name="view-the-tracing-output-in-azure"></a>Anzeige der Ablaufverfolgungs-Ausgabe in Azure
1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Webprojekt und anschließend auf **Veröffentlichen**.
2. Klicken Sie im Dialogfeld **Web veröffentlichen** auf **Veröffentlichen**.

    Nachdem Visual Studio Ihr Update veröffentlicht hat, wird ein Browserfenster mit Ihrer Startseite geöffnet (sofern Sie **Ziel-URL** auf der Registerkarte **Verbindung** nicht gelöscht haben).
3. Klicken Sie im **Server-Explorer** mit der rechten Maustaste auf Ihre Web-App, und wählen Sie **Streamingprotokolle anzeigen** aus.

    ![Anzeigen der Streamingprotokolle im Kontextmenü](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-viewlogsmenu.png)

    Das **Ausgabefenster** zeigt an, dass Sie mit dem Protokollstreamingdienst verbunden sind und fügt eine Benachrichtigungszeile für jede Minute hinzu, in der kein anzuzeigendes Protokoll eingeht.

    ![Anzeigen der Streamingprotokolle im Kontextmenü](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-nologsyet.png)
4. Klicken Sie im Browserfenster, in dem die Startseite Ihrer Anwendung geöffnet ist, auf **Contact**.

    Innerhalb weniger Sekunden wird die Ausgabe der Ablaufverfolgung mit dem Schweregrad "Fehler", die Sie der `Contact` -Methode hinzugefügt haben, im **Ausgabefenster** angezeigt.

    ![Fehler-Ablaufverfolgung im Ausgabefenster](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-errortrace.png)

    Visual Studio zeigt standardmäßig nur Ablaufverfolgungsprotokolle mit dem Schweregrad Fehler an, wenn der Protokollüberwachungsdienst gestartet wird. Bei der Erstellung einer neuen Azure-Web-App ist sämtliche Protokollierung standardmäßig deaktiviert, wie Sie zuvor auf der Einstellungsseite gesehen haben:

    ![Anwendungsprotokollierung deaktiviert](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-apploggingoff.png)

    Bei Auswahl von **Streamingprotokolle anzeigen** wurde jedoch von Visual Studio automatisch **Anwendungsprotokollierung (Dateisystem)** auf **Fehler** festgelegt, sodass Protokolle mit dem Schweregrad „Fehler“ gemeldet werden. Sie können diese Einstellung auf **Ausführlich**ändern, falls Sie sämtliche Ablaufverfolgungs-Protokolle sehen möchten. Wenn Sie einen Schweregrad unterhalb von Fehler auswählen, werden die Protokolleinträge der höheren Schweregrade ebenfalls geschrieben. Wenn Sie also ausführlich auswählen, werden die Protokolleinträge für Information, Warnung und Fehler geschrieben.  

1. Klicken Sie im **Server-Explorer** mit der rechten Maustaste auf die Web-App, und klicken Sie anschließend wie zuvor auf **Anzeigeeinstellungen**.
2. Ändern Sie **Anwendungsprotokollierung (Dateisystem)** in **Ausführlich**, und klicken Sie auf **Speichern**.

    ![Einstellen des Ablaufverfolgungs-Schweregrads auf Ausführlich](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-applogverbose.png)
3. Klicken Sie im Browserfenster, in dem Ihre **Kontakt**-Seite angezeigt wird, auf **Start**, anschließend auf **Info** und dann auf **Kontakt**.

    Innerhalb weniger Sekunden wird im **Ausgabefenster** Ihre gesamte Ablaufverfolgungsausgabe angezeigt.

    ![Ablaufverfolgungs-Schweregrad Ausführlich](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-verbosetraces.png)

    In diesem Abschnitt haben Sie die Protokollierung mithilfe der Azure-Web-App-Einstellungen aktiviert bzw. deaktiviert. Sie können die Ablaufverfolgungs-Listener auch über die Datei Web.config aktivieren bzw. deaktivieren. Wenn Sie jedoch die Datei "Web.config" ändern, wird die App-Domäne neu gestartet, was bei Aktivierung der Protokollierung über die Web-App-Konfiguration nicht der Fall ist. Falls das Problem schwer zu reproduzieren ist oder nur zeitweilig auftritt, kann es passieren, dass das Problem beim Neustart der Domäne verschwindet und Sie warten müssen, bis es erneut auftritt. Beim Aktivieren der Diagnose in Azure ist dies nicht der Fall, und Sie können sofort mit der Erfassung der Fehlerinformationen beginnen.

### <a name="output-window-features"></a>Funktionen des Ausgabefensters
Die Registerkarte **Azure-Protokolle** im **Ausgabefenster** enthält verschiedene Schaltflächen und ein Textfeld:

![Schaltflächen auf der Registerkarte Protokolle](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-icons.png)

Diese Schaltflächen bieten die folgenden Funktionen:

* Löschen des **Ausgabefensters** .
* Aktivieren bzw. Deaktivieren des Zeilenumbruchs.
* Starten bzw. Stoppen der Protokollüberwachung.
* Angeben, welche Protokolle überwacht werden sollen.
* Herunterladen von Protokollen.
* Filtern von Protokollen anhand von Suchzeichenfolgen oder regulären Ausdrücken.
* Schließen des **Ausgabefensters** .

Wenn Sie eine Suchzeichenfolge oder einen regulären Ausdruck eingeben, filtert Visual Studio die Protokollinformationen clientseitig. Daher können Sie die Kriterien eingeben, nachdem die Protokolle im **Ausgabefenster** angezeigt wurden, und Sie können die Filterkriterien ändern, ohne die Protokolle neu generieren zu müssen.

## <a name="webserverlogs"></a>Erstellen von Webserverprotokollen
Webserverprotokolle zeichnen sämtliche HTTP-Aktivitäten für die Web-App auf. Um sie im **Ausgabefenster** anzeigen zu können, müssen Sie diese Protokolle für die Web-App aktivieren und Visual Studio mitteilen, dass Sie sie überwachen möchten.

1. Ändern Sie auf der Registerkarte **Azure Web App Configuration** (Azure-Web-App-Konfiguration), die Sie im **Server-Explorer** geöffnet haben, die Webserverprotokollierung in **Ein**, und klicken Sie dann auf **Speichern**.

    ![Aktivieren der Webserverprotokollierung](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-webserverloggingon.png)
2. Klicken Sie im **Ausgabefenster** auf die Schaltfläche **Specify which Azure logs to monitor** (Angeben, welche Azure-Protokolle überwacht werden sollen).

    ![Angeben, welche Azure-Protokolle überwacht werden sollen](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-specifylogs.png)
3. Wählen Sie im Dialogfeld **Azure-Protokollierungsoptionen** die Option **Webserverprotokolle** aus, und klicken Sie auf **OK**.

    ![Überwachen von Webserverprotokollen](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-monitorwslogson.png)
4. Klicken Sie im Browserfenster, in dem Ihre Web-App angezeigt wird, auf **Start**, anschließend auf **Info** und dann auf **Kontakt**.

    Normalerweise werden die Anwendungsprotokolle zuerst angezeigt, gefolgt von den Webserverprotokollen. Möglicherweise müssen Sie kurz warten, bis die Protokolle angezeigt werden.

    ![Webserverprotokolle im Ausgabefenster](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-wslogs.png)

Bei der ersten Aktivierung der Webserverprotokolle in Visual Studio schreibt Azure die Protokolle standardmäßig in das Dateisystem. Alternativ können Sie im Azure-Portal angeben, dass die Webserverprotokolle in einen Blobcontainer in einem Speicherkonto geschrieben werden sollen.

Wenn Sie die Webserverprotokollierung für ein Azure-Speicherkonto im Portal aktivieren und die Protokollierung anschließend in Visual Studio deaktivieren, werden die Speicherkontoeinstellungen bei der nächsten Aktivierung wiederhergestellt.

## <a name="detailederrorlogs"></a>Anzeigen detaillierter Fehlermeldungsprotokolle
Die detaillierten Fehlerprotokolle liefern zusätzliche Informationen über HTTP-Anforderungen, die zu einer Fehlerantwort geführt haben (400 oder höher). Sie müssen diese Protokolle für die Web-App aktivieren und Visual Studio mitteilen, dass Sie diese überwachen möchten, um sie im **Ausgabefenster** anzeigen zu können.

1. Ändern Sie auf der Registerkarte **Azure Web App Configuration** (Azure-Web-App-Konfiguration), die Sie im **Server-Explorer** geöffnet haben, die Option **Detaillierte Fehlermeldungen** in **Ein**, und klicken Sie dann auf **Speichern**.

    ![Aktivieren der detaillierten Fehlermeldungen](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-detailedlogson.png)
2. Klicken Sie im **Ausgabefenster** auf die Schaltfläche **Specify which Azure logs to monitor** (Angeben, welche Azure-Protokolle überwacht werden sollen).
3. Klicken Sie im Dialogfeld **Azure-Protokollierungsoptionen** auf die Option **Alle Protokolle**, und klicken Sie dann auf **OK**.

    ![Überwachen aller Protokolle](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-monitorall.png)
4. Fügen Sie in der Adressleiste Ihres Browserfensters ein zusätzliches Zeichen an die URL an, um einen 404-Fehler zu verursachen (z. B. `http://localhost:53370/Home/Contactx`), und drücken Sie die EINGABETASTE.

    Nach wenigen Sekunden erscheint das detaillierte Fehlerprotokoll im **Ausgabefenster** von Visual Studio.

    ![Detailliertes Fehlerprotokoll im Ausgabefenster](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-detailederrorlog.png)

    Klicken Sie bei gedrückter STRG-Taste auf den Link, um das formatierte Protokoll im Browser anzuzeigen:

    ![Detailliertes Fehlerprotokoll im Browserfenster](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-detailederrorloginbrowser.png)

## <a name="downloadlogs"></a>Herunterladen von Dateisystemprotokollen
Alle Protokolle, die Sie im **Ausgabefenster** überwachen können, lassen sich auch als *.zip* -Datei herunterladen.

1. Klicken Sie im **Ausgabefenster** auf **Streamingprotokolle herunterladen**.

    ![Schaltflächen auf der Registerkarte Protokolle](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-downloadicon.png)

    Der Datei-Explorer öffnet Ihren *Downloads* -Ordner, und die heruntergeladene Datei ist ausgewählt.

    ![Heruntergeladene Datei](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-downloadedfile.png)
2. Wenn Sie die *.zip* -Datei extrahieren, sehen Sie die folgende Ordnerstruktur:

    ![Heruntergeladene Datei](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-logfilefolders.png)

   * Ablaufverfolgungsprotokolle von Anwendungen befinden sich in *TXT*-Dateien im Ordner *LogFiles\Application*.
   * Webserverprotokolle befinden sich in *LOG*-Dateien im Ordner *LogFiles\http\RawLogs*. Sie können diese Dateien mit Werkzeugen wie [Log Parser](http://www.microsoft.com/download/details.aspx?displaylang=en&id=24659) anzeigen und bearbeiten.
   * Ausführliche Fehlerprotokolle befinden sich in *HTML*-Dateien im Ordner *LogFiles\DetailedErrors*.

     (Der Ordner *deployments* enthält Dateien der Quellcodeverwaltung und hat nichts mit der Veröffentlichung in Visual Studio zu tun. Der Ordner *Git* enthält Ablaufverfolgungsprotokolle für die Quellcodeverwaltung und den Protokollstreamingdienst.)  

## <a name="storagelogs"></a>Anzeigen von Speicherprotokollen
Ablaufverfolgungsprotokolle von Anwendungen können auch an Azure-Speicherkonten geschickt und anschließend in Visual Studio angezeigt werden. Dazu müssen Sie ein Speicherkonto erstellen, Speicherprotokolle im klassischen Portal aktivieren und diese anschließend auf der Registerkarte **Protokolle** im Fenster **Azure-Web-App** anzeigen.

Sie können die Protokolle an eines oder alle dieser drei Ziele schicken:

* Das Dateisystem.
* Speicherkonto-Tabellen.
* Speicherkonto-Blobs.

Sie können unterschiedliche Schweregrade für die einzelnen Ziele angeben.

Tabellen erleichtern die Anzeige von Protokolldetails online und unterstützen die Streamingfunktion: Sie können Protokolle aus Tabellen anzeigen und sehen neue Protokolleinträge, sobald diese erstellt werden. Blobs erleichtern den Download der Protokolle in Dateiform und deren Analyse mithilfe von HDInsight, da HDInsight mit dem Blobformat umgehen kann. Weitere Informationen finden Sie im Abschnitt **Hadoop und MapReduce** unter [Data Storage Options (Building Real-World Cloud Apps with Azure)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options)(Datenspeicheroptionen (Erstellen von Cloud-Anwendungen mit Azure), in englischer Sprache).

Der Schweregrad Ihrer Dateisystemprotokolle ist momentan auf Ausführlich eingestellt. In den folgenden Schritten lernen Sie, wie Sie Protokolle mit dem Schweregrad Information in Speicherkontotabellen leiten können. Der Schweregrad "Information" bedeutet, dass alle Protokolle angezeigt werden die durch Aufruf von `Trace.TraceInformation`, `Trace.TraceWarning` und `Trace.TraceError` erstellt werden. Dies gilt nicht für Protokolle, die durch Aufruf von`Trace.WriteLine` erstellt werden.

Speicherkonten bieten mehr Speicherplatz und längere Aufbewahrungszeiten für Protokolle als das Dateisystem. Außerdem erhalten Sie bei der Aufbewahrung von Ablaufverfolgungsprotokollen in Speicherkonten zusätzliche Informationen, die bei Dateisystemprotokollen entfallen.

1. Klicken Sie mit der rechten Maustaste unter dem Azure-Knoten auf **Speicher**, und klicken Sie dann auf **Speicherkonto erstellen**.

![Speicherkonto erstellen](./media/web-sites-dotnet-troubleshoot-visual-studio/createstor.png)

1. Geben Sie im Dialogfeld **Speicherkonto erstellen** einen Namen für das Speicherkonto ein.

    Der Name muss eindeutig sein (kein anderes Azure Storage-Konto kann den gleichen Namen haben). Wenn der eingegebene Name bereits verwendet wird, haben Sie die Möglichkeit, ihn zu ändern.

    Die URL für den Zugriff auf Ihr Speicherkonto lautet *{Name}*.core.windows.net.
2. Wählen Sie in der Dropdownliste **Region oder Affinitätsgruppe** den Ihnen am nächsten gelegenen Ort aus.

    Diese Einstellung legt fest, in welchem Azure-Datencenter Ihr Speicherkonto gehostet wird. Für dieses Lernprogramm macht Ihre Auswahl keinen spürbaren Unterschied. Für Produktions-Web-Apps sollten Ihr Webserver und Ihr Speicherkonto jedoch in der gleichen Region liegen, um Latenz und Kosten für die Datenübertragung zu minimieren. Die Web-App (die Sie später erstellen) sollte sich möglichst in einer Region nahe an den Browsern befinden, die auf die Web-App zugreifen, um die Latenz zu minimieren.
3. Wählen Sie in der Dropdownliste **Replikation** den Wert **Lokal redundant** aus.
   
    Wenn Georeplikation für ein Speicherkonto aktiviert ist, werden dessen Inhalte an ein zweites Datencenter repliziert, um im Katastrophenfall eine Failover-Instanz an diesem zweiten Standort zur Verfügung zu haben. Für die Georeplikation können zusätzliche Kosten anfallen. Für Test- und Entwicklungskonten macht es wenig Sinn, für Georeplikation zu bezahlen. Weitere Informationen finden Sie unter [Erstellen, Verwalten oder Löschen eines Speicherkontos](../storage/storage-create-storage-account.md).
4. Klicken Sie auf **Erstellen**.

    ![Neues Speicherkonto](./media/web-sites-dotnet-troubleshoot-visual-studio/newstorage.png)    
5. Klicken Sie in Visual Studio im Fenster **Azure-Web-App** auf die Registerkarte **Protokolle** und anschließend auf **Protokollierung im Verwaltungsportal konfigurieren**.

    <!-- todo:screenshot of new portal if the VS page link goes to new portal -->
    ![Konfigurieren der Protokollierung](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-configlogging.png)

    Damit öffnen Sie im klassischen Portal die Registerkarte **Konfigurieren** für Ihre Web-App.
6. Scrollen Sie auf der Registerkarte **Konfigurieren** des klassischen Portals nach unten bis zum Bereich „Anwendungsdiagnose“, und ändern Sie den Wert von **Anwendungsprotokollierung (Tabellenspeicher)** in **Ein**.
7. Ändern Sie **Protokollierungsstufe** in **Informationen**.
8. Klicken Sie auf **Tabellenspeicher verwalten**.

    ![Klicken Sie auf Tabellenspeicher verwalten](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-stgsettingsmgmtportal.png)

    Im Feld **Tabellenspeicher für Anwendungsdiagnose verwalten** können Sie Ihr Speicherkonto auswählen, falls Sie mehr als eines verwenden. Sie können eine neue Tabelle erstellen oder eine existierende Tabelle verwenden.

    ![Tabellenspeicher verwalten](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-choosestorageacct.png)
9. Setzen Sie das Häkchen im Feld **Tabellenspeicher für Anwendungsdiagnose verwalten** , um das Fenster zu schließen.
10. Klicken Sie auf der Registerkarte **Konfigurieren** des klassischen Portals auf **Speichern**.
11. Klicken Sie in dem Browserfenster, in dem die Anwendung der Web-App angezeigt wird, auf **Start**, anschließend auf **Info** und dann auf **Kontakt**.

     Die von diesen Webseiten produzierten Protokollierungsinformationen werden daraufhin in Ihr Speicherkonto geschrieben.
12. Klicken Sie in Visual Studio auf der Registerkarte **Protokolle** im Fenster **Azure-Web-App** unter **Diagnosezusammenfassung** auf **Aktualisieren**.

     ![Klicken Sie auf "Aktualisieren".](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-refreshstorage.png)

     Im Bereich **Diagnosezusammenfassung** werden standardmäßig die Protokolle der vergangenen 15 Minuten angezeigt. Sie können das Zeitintervall ändern, um weitere Protokolleinträge anzuzeigen.

     (Wenn der Fehler „Tabelle nicht gefunden“ angezeigt wird, vergewissern Sie sich, dass Sie die entsprechenden Seiten aufgerufen haben, nachdem Sie **Anwendungsprotokollierung (Tabellenspeicher)** aktiviert und auf **Speichern** geklickt haben.)

     ![Speicherprotokolle](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-storagelogs.png)

     In dieser Ansicht werden die **Prozess-ID** und die **Thread-ID** für die einzelnen Protokolle angezeigt. Diese Informationen sind in den Dateisystemprotokollen nicht enthalten. Sie können zusätzliche Felder anzeigen, indem Sie die Azure-Speichertabelle direkt öffnen.
13. Klicken Sie auf **Alle Anwendungsprotokolle anzeigen**.

     Daraufhin wird die Ablaufprotokolltabelle in der Azure-Speichertabellenansicht angezeigt.

     (Wenn der Fehler „Sequenz enthält keine Elemente“ angezeigt wird, öffnen Sie den **Server-Explorer**, erweitern Sie den Knoten Ihres Speicherkontos unterhalb des Knotens **Azure**, klicken Sie mit der rechten Maustaste auf **Tabellen**, und klicken Sie auf **Aktualisieren**.)

     ![Speicherprotokolle in der Tabellenansicht](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-tracelogtableview.png)

     Diese Ansicht enthält zusätzliche Felder, die in keiner der anderen Ansichten angezeigt werden. Außerdem können Sie die Protokolle in dieser Ansicht mithilfe der Abfrage-Generator-GUI filtern. Weitere Informationen finden Sie im Abschnitt "Working with Table Resources - Filtering Entities" (Arbeiten mit Tabellenressourcen - Entitätenfilter" unter [Browsing Storage Resources with Server Explorer](http://msdn.microsoft.com/library/ff683677.aspx)(Durchsuchen von Speicherressourcen im Server-Explorer, in englischer Sprache).
14. Um die Details für eine einzelne Zeile anzuzeigen, doppelklicken Sie auf eine der Zeilen.

     ![Ablaufverfolgungstabelle im Server-Explorer](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-tracetablerow.png)

## <a name="failedrequestlogs"></a>Anzeigen von fehlgeschlagenen Anforderungsablaufverfolgungsprotokollen
Anhand der Protokolle für fehlgeschlagene Anforderungen können Sie im Detail herausfinden, wie IIS HTTP-Anforderungen bearbeitet, z. B. in Szenarien mit URL-Neuschreibung oder bei Authentifizierungsproblemen.

Azure-Web-Apps verwenden die gleiche Funktion zum Verfolgen fehlgeschlagener Anforderungen, die in IIS 7.0 und höher verfügbar ist. Sie haben jedoch keinen Zugriff auf die IIS-Einstellungen, in denen festgelegt wird, welche Fehler protokolliert werden. Wenn Sie die Verfolgung fehlgeschlagener Anforderungen aktivieren, werden alle Fehler erfasst.

Sie können die Protokolle für fehlgeschlagene Anforderungen in Visual Studio aktivieren, allerdings lassen sich diese Protokolle nicht in Visual Studio anzeigen. Diese Protokolle liegen in Form von XML-Dateien vor. Der Streamingprotokolldienst überwacht nur Dateien, die im Nur-Text-Modus lesbar sind: *TXT*-, *HTML*- und *LOG*-Dateien.

Sie können die Protokolle für fehlgeschlagene Anforderungen entweder direkt über FTP im Browser anzeigen oder mit einem FTP-Client auf Ihren lokalen Computer herunterladen. In diesem Abschnitt werden Sie die Protokolle direkt im Browser anzeigen.

1. Ändern Sie auf der Registerkarte **Konfiguration** im Fenster **Azure-Web-App**, das Sie über den**Server-Explorer** geöffnet haben, die Option **Ablaufverfolgung für Anforderungsfehler** in **Ein**, und klicken Sie dann auf **Speichern**.

    ![Aktivieren der Verfolgung fehlgeschlagener Anforderungen](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-failedrequeston.png)
2. Fügen Sie ein zusätzliches Zeichen zu der URL Ihrer Web-App in der Adressleiste hinzu und drücken Sie die EINGABETASTE, um einen 404-Fehler auszulösen.

    Daraufhin wird ein Protokoll für die fehlgeschlagene Anforderung erstellt. In den folgenden Schritten lernen Sie, wie Sie dieses Protokoll anzeigen oder herunterladen können.
3. Klicken Sie in Visual Studio auf der Registerkarte **Konfiguration** im Fenster **Azure-Web-App** auf **Im Verwaltungsportal öffnen**.
4. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf dem Blatt **Einstellungen** für Ihre Web-App auf **Anmeldeinformationen für die Bereitstellung**, und geben Sie dann einen neuen Benutzernamen und ein neues Kennwort ein.

    ![Neuer FTP-Benutzername und neues Passwort](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-enterftpcredentials.png)

    **Wenn Sie sich anmelden, müssen Sie den vollständigen Benutzernamen mit dem vorangestellten Namen der Web-App verwenden. Angenommen, Sie geben „meine-id" als einen Benutzernamen ein, und die Website ist „meinbeispiel“, dann melden Sie sich als „meinbeispiel\meine-id“ an.
5. Öffnen Sie in einem neuen Browserfenster die URL, die auf dem Blatt **Web-App** für Ihre Web-App unter **FTP-Hostname** oder unter **FTPS-Hostname** angezeigt wird.
6. Melden Sie sich mit den zuvor erstellten FTP-Anmeldeinformationen an (inklusive dem vor dem Benutzernamen vorangestelltem Namen der Web-App).

    Daraufhin wird im Browser der Stammordner der Web-App angezeigt.
7. Öffnen Sie den Ordner *LogFiles* .

    ![Öffnen Sie den Ordner LogFiles](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-logfilesfolder.png)
8. Öffnen Sie den Ordner mit dem Namen W3SVC plus nachgestelltem numerischem Wert.

    ![Öffnen Sie den Ordner W3SVC](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-w3svcfolder.png)

    Der Ordner enthält XML-Dateien für sämtliche protokollierten Fehler, nachdem Sie die Protokollierung fehlgeschlagener Anforderungen aktiviert haben, sowie eine XSL-Datei für die XML-Formatierung im Browser.

    ![Ordner W3SVC](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-w3svcfoldercontents.png)
9. Klicken Sie auf die XML-Datei für die fehlgeschlagene Anforderung, deren Ablaufverfolgungsinformationen Sie anzeigen möchten.

    Die folgende Abbildung zeigt einen Teil der Ablaufverfolgungsinformationen für einen Beispielfehler.

    ![Verfolgung fehlgeschlagener Anforderungen im Browser](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-failedrequestinbrowser.png)

## <a name="nextsteps"></a>Nächste Schritte
Sie haben gelernt, wie Visual Studio die Anzeige der Protokolle von Azure-Web-Apps erleichtert. Die folgenden Abschnitte enthalten Links zu weiteren Ressourcen zu verwandten Themen:

* Problembehandlung von Azure-Web-Apps
* Debuggen in Visual Studio
* Remotedebuggen in Azure
* Ablaufverfolgung in ASP.NET-Anwendungen
* Analyse von Webserverprotokollen
* Analyse der Ablaufverfolgung fehlgeschlagener Anforderungen
* Debuggen von Cloud-Diensten.

### <a name="azure-web-app-troubleshooting"></a>Problembehandlung von Azure-Web-Apps
Weitere Informationen zur Problembehandlung von Web-Apps in Azure App Service finden Sie in den folgenden Ressourcen:

* [Überwachen von Web-Apps](/manage/services/web-sites/how-to-monitor-websites/)
* [Untersuchen von Speicherlecks in Azure-Web-Apps mit Visual Studio 2013](http://blogs.msdn.com/b/visualstudioalm/archive/2013/12/20/investigating-memory-leaks-in-azure-web-sites-with-visual-studio-2013.aspx). Microsoft ALM-Blogbeiträge über Visual Studio-Funktionen für die Untersuchung von Problemen mit verwaltetem Speicher.
* [Onlinetools für Azure -Web-Apps, die Sie kennen sollten](https://azure.microsoft.com/blog/2014/03/28/windows-azure-websites-online-tools-you-should-know-about-2/). Blogbeitrag von Amit Apple.

Falls Sie spezifische Fragen zur Problembehandlung haben, können Sie diese in einem der folgenden Foren stellen:

* [Das Azure-Forum auf der ASP.NET-Website](http://forums.asp.net/1247.aspx/1?Azure+and+ASP+NET).
* [Das Azure-Forum auf MSDN](http://social.msdn.microsoft.com/Forums/windowsazure/).
* [StackOverflow.com](http://www.stackoverflow.com).

### <a name="debugging-in-visual-studio"></a>Debuggen in Visual Studio
Weitere Informationen zum Debugmodus in Visual Studio finden Sie im MSDN-Thema [Debuggen in Visual Studio](http://msdn.microsoft.com/library/vstudio/sc65sadd.aspx) und unter [Debugging Tips with Visual Studio 2010](http://weblogs.asp.net/scottgu/archive/2010/08/18/debugging-tips-with-visual-studio-2010.aspx) (Tipps zum Debuggen in Visual Studio 2010).

### <a name="remote-debugging-in-azure"></a>Remotedebuggen in Azure
Weitere Informationen zum Remotedebuggen für Azure-Web-Apps und WebJobs finden Sie in den folgenden Ressourcen:

* [Einführung in das Remotedebuggen von Azure App Service-Web-Apps](https://azure.microsoft.com/blog/2014/05/06/introduction-to-remote-debugging-on-azure-web-sites/).
* [Einführung in das Remotedebuggen von Azure App Service-Web-Apps Teil 2 - Einblick in das Remotedebuggen](https://azure.microsoft.com/blog/2014/05/07/introduction-to-remote-debugging-azure-web-sites-part-2-inside-remote-debugging/)
* [Einführung in das Remotedebuggen von Azure App Service-Web-Apps Teil 3 - Mehrinstanzenumgebung und GIT](https://azure.microsoft.com/blog/2014/05/08/introduction-to-remote-debugging-on-azure-web-sites-part-3-multi-instance-environment-and-git/)
* [Debuggen von WebJobs (Video)](https://www.youtube.com/watch?v=ncQm9q5ZFZs&list=UU_SjTh-ZltPmTYzAybypB-g&index=1)

Falls Ihre Web-App eine Azure-Web-API oder ein Mobile Services-Back-End verwendet und Sie diese Komponenten debuggen möchten, finden Sie weitere Informationen unter [Debuggen von .NET Back-End in Visual Studio](http://blogs.msdn.com/b/azuremobile/archive/2014/03/14/debugging-net-backend-in-visual-studio.aspx).

### <a name="tracing-in-aspnet-applications"></a>Ablaufverfolgung in ASP.NET-Anwendungen
Momentan sind keine vollständigen und aktuellen Einführungen zur Ablaufverfolgung in ASP.NET im Internet verfügbar. Beginnen Sie daher mit den älteren Einführungen für Web Forms, als MVC noch nicht existierte, und ergänzen Sie diese Lektüre mit neueren Blogeinträgen zu speziellen Themen. Die folgenden Ressourcen bieten gute Einstiegspunkte:

* [Überwachung und Telemetrie (Erstellen realer Cloud-Apps mit Azure).](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry)<br>
  Dieses E-Book-Kapitel enthält Empfehlungen zur Ablaufverfolgung in Azure-Cloudanwendungen.
* [ASP.NET-Ablaufverfolgung](http://msdn.microsoft.com/library/ms972204.aspx)<br/>
  Ein älterer Artikel, der sich jedoch immer noch gut als Einstieg in das Thema eignet.
* [Ablaufverfolgungslistener](http://msdn.microsoft.com/library/4y5y10s7.aspx)<br/>
  Informationen zu Ablaufverfolgungslistenern, [WebPageTraceListener](http://msdn.microsoft.com/library/system.web.webpagetracelistener.aspx) wird jedoch nicht erwähnt.
* Integrieren der ASP.NET-Ablaufverfolgung mit der System.Diagnostics-[Ablaufverfolgung](http://msdn.microsoft.com/library/b0ectfxd.aspx)<br/>
  Dieser Artikel ist ebenfalls älteren Datums, enthält jedoch zusätzliche Informationen, die in der Einführung nicht behandelt werden.
* [Ablaufverfolgung in ASP.NET MVC-Razoransichten](http://blogs.msdn.com/b/webdev/archive/2013/07/16/tracing-in-asp-net-mvc-razor-views.aspx)<br/>
  (Ablaufverfolgung in den ASP.NET MVC-Razoransichten, in englischer Sprache) Dieser Blogeintrag behandelt neben der Ablaufverfolgung in Razoransichten auch die Erstellung von Fehlerfiltern zur Protokollierung aller Ausnahmefehler in MVC-Anwendungen. Informationen zur Protokollierung aller Ausnahmefehler in Web Forms-Anwendungen finden Sie im Global.asax-Beispiel unter [Vollständiges Beispiel für Fehlerhandler](http://msdn.microsoft.com/library/bb397417.aspx) auf MSDN. Falls Sie in MVC oder Web Forms bestimmte Ausnahmen protokollieren möchten, diese Ausnahmen jedoch vom Standard-Framework behandelt werden sollen, können Sie diese wie im folgenden Beispiel abfangen und erneut auslösen:

        try
        {
           // Your code that might cause an exception to be thrown.
        }
        catch (Exception ex)
        {
            Trace.TraceError("Exception: " + ex.ToString());
            throw;
        }
* [Ablaufprotokollierung und Streamingdiagnose über die Azure-Befehlszeile (plus Glimpse)](http://www.hanselman.com/blog/StreamingDiagnosticsTraceLoggingFromTheAzureCommandLinePlusGlimpse.aspx)<br/>
  Verwenden der Befehlszeile für die Visual Studio-Aktionen aus diesem Lernprogramm. [Glimpse](http://www.hanselman.com/blog/IfYoureNotUsingGlimpseWithASPNETForDebuggingAndProfilingYoureMissingOut.aspx) ist ein Tool zum Debuggen von ASP.NET-Anwendungen.
* [Using Web Apps Logging and Diagnostics – with David Ebbo](/documentation/videos/azure-web-site-logging-and-diagnostics/) (Verwenden der Web-App-Protokollierung – mit David Ebbo) und [Streaming Logs from Web Apps – with David Ebbo](/documentation/videos/log-streaming-with-azure-web-sites/) (Protokollstreaming aus Web-Apps – mit David Ebbo)<br>
  Videos von Scott Hanselman und David Ebbo.

Für die Fehlerprotokollierung können Sie Open-Source-Protokollframeworks wie [ELMAH](http://nuget.org/packages/elmah/) verwenden, anstatt Ihren eigenen Ablaufverfolgungscode zu schreiben. Weitere Informationen finden Sie in [Scott Hanselmans Blogbeiträgen zu ELMAH](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx).

Sie müssen nicht unbedingt die Ablaufverfolgung von ASP.NET oder System.Diagnostics verwenden, um Streamingprotokolle aus Azure zu erhalten. Der Streamingprotokolldienst von Azure-Web-Apps ist in der Lage, beliebige *TXT-*, *HTML*- oder *LOG*-Dateien aus dem Ordner *LogFiles* zu streamen. Sie können also Ihr eigenes Protokollsystem erstellen und in das Dateisystem der Web-App schreiben, und Ihre Dateien werden automatisch gestreamt und heruntergeladen. Dazu müssen Sie nur den Anwendungscode schreiben, der Dateien im Ordner *D:\Home\LogFiles* erstellt.

### <a name="analyzing-web-server-logs"></a>Analyse von Webserverprotokollen
Weitere Informationen zur Analyse von Webserverprotokollen finden Sie in den folgenden Ressourcen:

* [LogParser](http://www.microsoft.com/download/details.aspx?id=24659)<br/>
  Ein Tool zum Anzeigen von Daten in Webserverprotokollen (*.log* -Dateien).
* [Problembehandlung bei IIS-Leistungsproblemen oder Anwendungsfehlern mithilfe von LogParser](http://www.iis.net/learn/troubleshoot/performance-issues/troubleshooting-iis-performance-issues-or-application-errors-using-logparser)<br/>
  (Problembehandlung von IIS-Leistungsproblemen oder Anwendungsfehlern mithilfe von LogParser, in englischer Sprache) Eine Einführung in das LogParser-Tool, das Sie bei der Analyse von Webserverprotokollen unterstützt.
* [Blogbeiträge von Robert McMurray zur Verwendung von LogParser](http://blogs.msdn.com/b/robert_mcmurray/archive/tags/logparser/)<br/>
* [HTTP-Statuscodes in IIS 7.0, IIS 7.5 und IIS 8.0](http://support.microsoft.com/kb/943891)

### <a name="analyzing-failed-request-tracing-logs"></a>Analyse der Ablaufverfolgung fehlgeschlagener Anforderungen
Die Microsoft TechNet-Website enthält einen Abschnitt zum Thema [Ablaufverfolgung von Anforderungen mit Fehlerrückgabe](http://www.iis.net/learn/troubleshoot/using-failed-request-tracing), der für das Verständnis dieser Protokolle hilfreich ist. Diese Dokumentation konzentriert sich jedoch hauptsächlich auf die Ablaufverfolgung fehlgeschlagener Anforderungen in IIS. Diese Option ist für Azure-Web-Apps nicht verfügbar.

[GetStarted]: app-service-web-get-started-dotnet.md
[GetStartedWJ]: websites-dotnet-webjobs-sdk.md

