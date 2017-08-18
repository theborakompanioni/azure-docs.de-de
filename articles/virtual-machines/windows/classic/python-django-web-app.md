---
title: Django-Web-App auf einer Azure-VM mit Windows Server | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Django-basierte Website in Azure mithilfe einer VM mit Windows Server 2012 R2 Datacenter mit dem klassischen Bereitstellungsmodell hosten.
services: virtual-machines-windows
documentationcenter: python
author: huguesv
manager: wpickett
editor: 
tags: azure-service-management
ms.assetid: e36484d1-afbf-47f5-b755-5e65397dc1c3
ms.service: virtual-machines-windows
ms.workload: web
ms.tgt_pltfrm: vm-windows
ms.devlang: python
ms.topic: article
ms.date: 05/31/2017
ms.author: huvalo
ms.translationtype: HT
ms.sourcegitcommit: 398efef3efd6b47c76967563251613381ee547e9
ms.openlocfilehash: 283a296fb39863c2801be1093cc4f56904786abd
ms.contentlocale: de-de
ms.lasthandoff: 08/11/2017

---
# <a name="django-hello-world-web-app-on-a-windows-server-vm"></a>Django-Web-App „Hello World“ auf einer Windows Server-VM

> [!IMPORTANT] 
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Azure Resource Manager und das klassische Bereitstellungsmodell](../../../resource-manager-deployment-model.md). Dieser Artikel beschreibt das klassische Bereitstellungsmodell. Wir empfehlen für die meisten Neubereitstellungen das Resource Manager-Modell.

In diesem Tutorial wird gezeigt, wie eine Django-basierte Website in Windows Server in Azure Virtual Machines gehostet wird. In diesem Tutorial werden keine Erfahrungen mit Azure vorausgesetzt. Wenn Sie dieses Tutorial abschließen, verfügen Sie über eine aktive Django-basierte Anwendung in der Cloud.

In diesem Artikel werden folgende Themen erläutert:

* Einrichten eines virtuellen Azure-Computers als Host von Django. In diesem Tutorial wird zwar erläutert, wie die Vorgehensweise für **Windows Server** ist, Sie können die Schritte aber auch für eine Linux-VM nutzen, die in Azure gehostet wird.
* Erstellen einer neuen Django-Anwendung in Windows.

Das Tutorial veranschaulicht das Erstellen einer einfachen „Hello World“-Webanwendung. Die Anwendung wird auf einem virtuellen Azure-Computer gehostet.

Der folgende Screenshot zeigt die fertige Anwendung:

![Ein Browserfenster zeigt die Seite „Hello World“ in Azure an][1]

[!INCLUDE [create-account-and-vms-note](../../../../includes/create-account-and-vms-note.md)]

## <a name="create-and-set-up-an-azure-virtual-machine-to-host-django"></a>Erstellen und Einrichten eines virtuellen Azure-Computers zum Hosten von Django

1. Informationen zum Erstellen eines virtuellen Azure-Computers mit der Windows Server 2012 R2 Datacenter-Distribution finden Sie unter [Erstellen eines virtuellen Windows-Computers im Azure-Portal](tutorial.md).
2. Legen Sie in Azure fest, dass der Port 80-Datenverkehr aus dem Web an Port 80 auf dem virtuellen Computer weitergeleitet wird:
   
   1. Wechseln Sie im Azure-Portal zum Dashboard, und wählen Sie den neu erstellten virtuellen Computer aus.
   2. Klicken Sie auf **Endpunkte** und anschließend auf **Hinzufügen**.

     ![Hinzufügen eines Endpunkts](./media/python-django-web-app/django-helloworld-add-endpoint-new-portal.png)

   3. Geben Sie auf der Seite **Endpunkt hinzufügen** für **Name** den Namen **HTTP** ein. Legen Sie den öffentlichen und den privaten TCP-Port auf **80** fest.

     ![Eingeben des Namens und Festlegen des öffentlichen und des privaten Ports](./media/python-django-web-app/django-helloworld-add-endpoint-set-ports-new-portal.png)

   4. Klicken Sie auf **OK**.
     
3. Wählen Sie Ihre VM im Dashboard aus. Um das Remotedesktopprotokoll (RDP) für eine Remoteanmeldung beim neu erstellten virtuellen Azure-Computer zu verwenden, klicken Sie auf **Verbinden**.  

> [!IMPORTANT] 
> Die folgenden Anweisungen setzen voraus, dass Sie sich ordnungsgemäß am virtuellen Computer angemeldet haben. Sie setzen außerdem voraus, dass Sie Befehle auf dem virtuellen Computer und nicht auf dem lokalen Computer ausgeben.

## <a id="setup"> </a>Installieren von Python, Django und WFastCGI
> [!NOTE]
> Zum Herunterladen mithilfe von Internet Explorer müssen Sie möglicherweise die Internet Explorer-Einstellungen unter **Verstärkte Sicherheitskonfiguration** konfigurieren. Klicken Sie hierzu auf **Starten** > **Verwaltung** > **Server-Manager** > **Lokaler Server**. Klicken Sie auf **Verstärkte Sicherheitskonfiguration für IE**, und wählen Sie dann **Aus** aus.

1. Installieren Sie die neuesten Versionen von Python 2.7 oder Python 3.4 von [python.org][python.org].
2. Installieren Sie die Wfastcgi- und Django-Pakete mithilfe von PIP.
   
    Verwenden Sie für Python 2.7 den folgenden Befehl:
   
        c:\python27\scripts\pip install wfastcgi
        c:\python27\scripts\pip install django
   
    Verwenden Sie für Python 3.4 den folgenden Befehl:
   
        c:\python34\scripts\pip install wfastcgi
        c:\python34\scripts\pip install django

## <a name="install-iis-with-fastcgi"></a>Installieren von IIS mit FastCGI
* Installieren Sie Internetinformationsdienste (Internet Information Services, IIS) mit FastCGI-Unterstützung. Die Ausführung nimmt möglicherweise einige Minuten in Anspruch.
   
        start /wait %windir%\System32\PkgMgr.exe /iu:IIS-WebServerRole;IIS-WebServer;IIS-CommonHttpFeatures;IIS-StaticContent;IIS-DefaultDocument;IIS-DirectoryBrowsing;IIS-HttpErrors;IIS-HealthAndDiagnostics;IIS-HttpLogging;IIS-LoggingLibraries;IIS-RequestMonitor;IIS-Security;IIS-RequestFiltering;IIS-HttpCompressionStatic;IIS-WebServerManagementTools;IIS-ManagementConsole;WAS-WindowsActivationService;WAS-ProcessModel;WAS-NetFxEnvironment;WAS-ConfigurationAPI;IIS-CGI

## <a name="create-a-new-django-application"></a>Erstellen einer neuen Django-Anwendung
1. Geben Sie in „C:\inetpub\wwwroot“ den folgenden Befehl ein, um ein neues Django-Projekt zu erstellen:
   
   Verwenden Sie für Python 2.7 den folgenden Befehl:
   
       C:\Python27\Scripts\django-admin.exe startproject helloworld
   
   Verwenden Sie für Python 3.4 den folgenden Befehl:
   
       C:\Python34\Scripts\django-admin.exe startproject helloworld
   
   ![Ergebnis des Befehls 'New-AzureService'](./media/python-django-web-app/django-helloworld-cmd-new-azure-service.png)
2. Mit dem Befehl `django-admin` wird eine Grundstruktur für Django-basierte Websites erstellt:
   
   * `helloworld\manage.py` hilft Ihnen, das Hosting für Ihre Django-basierte Website zu starten und zu beenden.
   * `helloworld\helloworld\settings.py` enthält Django-Einstellungen für Ihre Anwendung.
   * `helloworld\helloworld\urls.py` enthält den Zuordnungscode zwischen den einzelnen URLs und deren Ansicht.
3. Erstellen Sie im Verzeichnis „C:\inetpub\wwwroot\helloworld\helloworld“ eine neue Datei mit dem Namen „views.py“. Diese Datei enthält die Ansicht, mit der die Seite „Hello World“ gerendert wird. Geben Sie im Code-Editor die folgenden Befehle ein:
   
       from django.http import HttpResponse
       def home(request):
           html = "<html><body>Hello World!</body></html>"
           return HttpResponse(html)
4. Ersetzen Sie den Inhalt der Datei „urls.py“ durch die folgenden Befehle:
   
       from django.conf.urls import patterns, url
       urlpatterns = patterns('',
           url(r'^$', 'helloworld.views.home', name='home'),
       )

## <a name="set-up-iis"></a>Einrichten von IIS
1. Entsperren Sie den Abschnitt für Handler in der globalen Datei „applicationhost.config“.  Dadurch kann die Datei „web.config“ den Python-Handler verwenden. Fügen Sie diesen Befehl hinzu:
   
        %windir%\system32\inetsrv\appcmd unlock config -section:system.webServer/handlers
2. Aktivieren Sie WFastCGI. Dadurch wird der globalen Datei „applicationhost.config“ eine Anwendung hinzugefügt, die auf die ausführbare Datei des Python-Interpreters und das Skript „wfastcgi.py“ verweist.
   
    In Python 2.7:
   
        C:\python27\scripts\wfastcgi-enable
   
    In Python 3.4:
   
        C:\python34\scripts\wfastcgi-enable
3. Erstellen Sie in „C:\inetpub\wwwroot\helloworld“ eine web.config-Datei. Der Wert des `scriptProcessor`-Attributs sollte mit der Ausgabe des vorherigen Schritts übereinstimmen. Weitere Informationen über die wfastcgi-Einstellung finden Sie unter [pypi wfastcgi][wfastcgi].
   
   In Python 2.7:
   
        <configuration>
          <appSettings>
            <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
            <add key="PYTHONPATH" value="C:\inetpub\wwwroot\helloworld" />
            <add key="DJANGO_SETTINGS_MODULE" value="helloworld.settings" />
          </appSettings>
          <system.webServer>
            <handlers>
                <add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\Python27\python.exe|C:\Python27\Lib\site-packages\wfastcgi.pyc" resourceType="Unspecified" />
            </handlers>
          </system.webServer>
        </configuration>
   
   In Python 3.4:
   
        <configuration>
          <appSettings>
            <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
            <add key="PYTHONPATH" value="C:\inetpub\wwwroot\helloworld" />
            <add key="DJANGO_SETTINGS_MODULE" value="helloworld.settings" />
          </appSettings>
          <system.webServer>
            <handlers>
                <add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\Python34\python.exe|C:\Python34\Lib\site-packages\wfastcgi.py" resourceType="Unspecified" />
            </handlers>
          </system.webServer>
        </configuration>
4. Aktualisieren Sie den Speicherort der IIS-Standardwebsite auf den Django-Projektordner:
   
        %windir%\system32\inetsrv\appcmd set vdir "Default Web Site/" -physicalPath:"C:\inetpub\wwwroot\helloworld"
5. Laden Sie die Webseite in Ihrem Browser.

![Ein Browserfenster zeigt die Seite „Hello World“ in Azure an][1]

## <a name="shut-down-your-azure-virtual-machine"></a>Herunterfahren des virtuellen Azure-Computers
Wenn Sie dieses Tutorial durchgearbeitet haben, empfiehlt es sich, die für das Tutorial erstellte Azure-VM herunterzufahren oder zu entfernen. Dadurch werden Ressourcen für andere Tutorials freigegeben, und Sie können zusätzliche Gebühren für die Azure-Nutzung vermeiden.

[1]: ./media/python-django-web-app/django-helloworld-browser-azure.png

[port80]: ./media/python-django-web-app/django-helloworld-port80.png

[Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[python.org]: https://www.python.org/downloads/
[wfastcgi]: https://pypi.python.org/pypi/wfastcgi

