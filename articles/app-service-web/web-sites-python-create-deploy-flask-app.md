---
title: Erstellen von Web-Apps mit Flask in Azure
description: "In diesem Tutorial werden Sie in die Verwendung einer Python-Web-App in Azure eingeführt."
services: app-service\web
documentationcenter: python
tags: python
author: huguesv
manager: wpickett
editor: 
ms.assetid: b7f4ca3a-0b52-4108-90a7-f7e07ac73da0
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 02/20/2016
ms.author: huvalo
translationtype: Human Translation
ms.sourcegitcommit: b1a633a86bd1b5997d5cbf66b16ec351f1043901
ms.openlocfilehash: f95a85804d009321f77998fafc4700b1c879fcee


---
# <a name="creating-web-apps-with-flask-in-azure"></a>Erstellen von Web-Apps mit Flask in Azure
In diesem Tutorial werden die ersten Schritte für die Verwendung von Python in [Azure App Service-Web-Apps](http://go.microsoft.com/fwlink/?LinkId=529714)beschrieben.  Web-Apps bietet uneingeschränktes kostenloses Hosting und schnelle Bereitstellung, und Sie können Python verwenden!  Wenn die Anwendung größer wird, können Sie zu kostenpflichtigem Hosting wechseln und außerdem alle anderen Azure-Dienste integrieren.

Sie erstellen eine Anwendung mithilfe des Webframeworks Flask. (Alternativ sind auch Versionen für [Django](web-sites-python-create-deploy-django-app.md) und [Bottle](web-sites-python-create-deploy-bottle-app.md) verfügbar.)  Sie erstellen die Website aus dem Azure-Katalog, richten die Git-Bereitstellung ein und klonen das Repository lokal.  Dann führen Sie die Anwendung lokal aus, nehmen Änderungen vor, führen ein Commit aus und übertragen die Änderungen in Azure.  Das Tutorial zeigt, wie dies unter Windows oder Mac/Linux erfolgt.

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> Wenn Sie Azure App Service ausprobieren möchten, ehe Sie sich für ein Azure-Konto anmelden, können Sie unter [App Service testen](https://azure.microsoft.com/try/app-service/)sofort kostenlos eine kurzlebige Starter-Web-App in App Service erstellen. Keine Kreditkarte erforderlich, keine Verpflichtungen.
> 
> 

## <a name="prerequisites"></a>Voraussetzungen
* Windows, Mac oder Linux
* Python 2.7 oder 3.4
* setuptools, pip, virtualenv (nur Python 2.7)
* Git
* [Python Tools für Visual Studio][Python Tools für Visual Studio] (PTVS) – Hinweis: Dies ist optional.

**Hinweis**: TFS-Veröffentlichung wird derzeit für Python-Projekte nicht unterstützt.

### <a name="windows"></a>Windows
Falls Python 2.7 oder 3.4 (32 Bit) noch nicht installiert ist, empfehlen wir die Installation von [Azure SDK für Python 2.7] oder [Azure SDK für Python 3.4] über den Webplattform-Installer.  Dadurch werden die 32-Bit-Version von Python, setuptools, pip, virtualenv usw. installiert (32-Bit-Python ist auf den Azure-Hostcomputern installiert).  Alternativ können Sie Python von [python.org] abrufen.

Für Git empfehlen wir [Git für Windows] oder [GitHub für Windows].  Wenn Sie Visual Studio verwenden, können Sie die integrierte Unterstützung für Git nutzen.

Wir empfehlen auch die Installation von [Python Tools 2.2 für Visual Studio].  Dies ist optional, aber wenn Sie über [Visual Studio]einschließlich des kostenlosen Visual Studio Community 2013 oder Visual Studio Express 2013 für Web verfügen, erhalten Sie eine leistungsfähige Python-IDE.

### <a name="maclinux"></a>Mac/Linux
Sie sollten Python und Git bereits installiert haben, doch stellen Sie sicher, dass Sie über Python 2.7 oder 3.4 verfügen.

## <a name="web-app-create-on-the-azure-portal"></a>Web-App-Erstellung im Azure-Portal
Der erste Schritt beim Erstellen einer App ist das Erstellen der Web-App über das [Azure-Portal](https://portal.azure.com). 

1. Klicken Sie im Azure-Portal in der unteren linken Ecke auf **NEU** . 
2. Klicken Sie auf **Web und mobil**.
3. Geben Sie im Suchfeld "python" ein.
4. Wählen Sie in den Suchergebnissen **Flask** aus, und klicken Sie dann auf **Erstellen**.
5. Konfigurieren Sie die neue Flask-App, indem Sie beispielsweise einen neuen App Service-Plan und eine neue Ressourcengruppe dafür erstellen. Klicken Sie dann auf **Erstellen**.
6. Konfigurieren Sie die Git-Veröffentlichung für Ihre neu erstellte Web-App anhand der Anleitung unter [Lokale Git-Bereitstellung in Azure App Service](app-service-deploy-local-git.md).

## <a name="application-overview"></a>Anwendungsübersicht
### <a name="git-repository-contents"></a>Inhalt des Git-Repositorys
Hier sehen Sie eine Übersicht der Dateien, die sich im Git-Ausgangsrepository befinden, das wir im nächsten Abschnitt klonen.

    \FlaskWebProject\__init__.py
    \FlaskWebProject\views.py
    \FlaskWebProject\static\content\
    \FlaskWebProject\static\fonts\
    \FlaskWebProject\static\scripts\
    \FlaskWebProject\templates\about.html
    \FlaskWebProject\templates\contact.html
    \FlaskWebProject\templates\index.html
    \FlaskWebProject\templates\layout.html

Die Hauptquellen für die Anwendung.  Besteht aus 3 Seiten (Index, Info, Kontakt) mit einem Masterlayout.  Statische Inhalte und Skripts umfassen bootstrap, jquery, modernizr und respond.

    \runserver.py

Unterstützung für lokale Entwicklungsserver. Dient zum lokalen Ausführen der Anwendung.

    \FlaskWebProject.pyproj
    \FlaskWebProject.sln

Projektdateien für die Verwendung mit [Python Tools für Visual Studio].

    \ptvs_virtualenv_proxy.py

IIS-Proxy für virtuelle Umgebungen und PTVS-Unterstützung für Remotedebugging.

    \requirements.txt

Externe Pakete, die von dieser Anwendung benötigt werden. Das Bereitstellungsskript installiert die in dieser Datei aufgelisteten Pakete mit pip.

    \web.2.7.config
    \web.3.4.config

IIS-Konfigurationsdateien.  Das Bereitstellungsskript verwendet die entsprechende Datei "web.x.y.config" und kopiert sie als "web.config".

### <a name="optional-files---customizing-deployment"></a>Optionale Dateien - Anpassung der Bereitstellung
[!INCLUDE [web-sites-python-customizing-deployment](../../includes/web-sites-python-customizing-deployment.md)]

### <a name="optional-files---python-runtime"></a>Optionale Dateien - Python-Laufzeit
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

### <a name="additional-files-on-server"></a>Zusätzliche Dateien auf dem Server
Einige Dateien sind auf dem Server vorhanden, werden jedoch nicht dem Git-Repository hinzugefügt.  Diese werden vom Bereitstellungsskript erstellt.

    \web.config

IIS-Konfigurationsdatei.  Wird anhand von "web.x.y.config" bei jeder Bereitstellung erstellt.

    \env\

Virtuelle Umgebung für Python.  Wird während der Bereitstellung erstellt, sofern nicht bereits eine kompatible virtuelle Umgebung für die App vorhanden ist.  Pakete, die in "requirements.txt" aufgeführt sind, werden mit pip installiert. pip überspringt die Installation, wenn die Pakete bereits installiert sind.

In den nächsten 3 Abschnitten wird beschrieben, wie Sie die Web-App-Entwicklung in 3 unterschiedlichen Umgebungen fortsetzen:

* Windows - mit Python Tools für Visual Studio
* Windows, über die Befehlszeile
* Mac/Linux, über die Befehlszeile

## <a name="web-app-development---windows---python-tools-for-visual-studio"></a>Web-App-Entwicklung – Windows – Python Tools für Visual Studio
### <a name="clone-the-repository"></a>Klonen des Repositorys
Klonen Sie zunächst das Repository mithilfe der URL, die im Azure-Portal bereitgestellt ist. Weitere Informationen finden Sie unter [Lokale Git-Bereitstellung in Azure App Service](app-service-deploy-local-git.md).

Öffnen Sie die Projektmappendatei (.sln), die im Stammverzeichnis des Repositorys enthalten ist.

![](./media/web-sites-python-create-deploy-flask-app/ptvs-solution-flask.png)

### <a name="create-virtual-environment"></a>Erstellen einer virtuellen Umgebung
Jetzt erstellen wir eine virtuelle Umgebung für die lokale Entwicklung.  Klicken Sie mit der rechten Maustaste auf **Python-Umgebungen**, und wählen Sie **Virtuelle Umgebung hinzufügen...** aus.

* Stellen Sie sicher, dass der Name der Umgebung `env`lautet.
* Wählen Sie den Basisinterpreter.  Achten Sie darauf, die gleiche Version von Python zu verwenden, die für Ihre Web-App ausgewählt ist (in runtime.txt oder im Blatt **Anwendungseinstellungen** Ihrer Web-App im Azure-Portal).
* Stellen Sie sicher, dass die Option zum Herunterladen und Installieren von Paketen aktiviert ist.

![](./media/web-sites-python-create-deploy-flask-app/ptvs-add-virtual-env-27.png)

Klicken Sie auf **Erstellen**.  Dadurch wird die virtuelle Umgebung erstellt, und die in "requirements.txt" aufgelisteten Abhängigkeiten werden installiert.

### <a name="run-using-development-server"></a>Ausführen mithilfe des Entwicklungsservers
Drücken Sie F5, um mit dem Debuggen beginnen. Ihr Webbrowser wird automatisch auf der Seite geöffnet, die lokal ausgeführt.

![](./media/web-sites-python-create-deploy-flask-app/windows-browser-flask.png)

Sie können Haltepunkte im Quellcode setzen, die Überwachungsfenster verwenden, usw.  In der [Dokumentation zu Python Tools für Visual Studio] finden Sie weitere Informationen zu den verschiedenen Features.

### <a name="make-changes"></a>Vornehmen von Änderungen
Jetzt können Sie experimentieren, indem Sie Änderungen an der Anwendungsquellen und/oder Vorlagen vornehmen.

Nachdem Sie Ihre Änderungen getestet haben, führen Sie für diese im Git-Repository einen Commit durch:

![](./media/web-sites-python-create-deploy-flask-app/ptvs-commit-flask.png)

### <a name="install-more-packages"></a>Installieren weiterer Pakete
Ihre Anwendung weist möglicherweise Abhängigkeiten über Python und Flask hinaus auf.

Sie können zusätzliche Pakete mit pip installieren.  Klicken Sie zum Installieren eines Pakets mit der rechten Maustaste auf die virtuelle Umgebung, und wählen Sie **Python-Paket installieren**aus.

Geben Sie beispielsweise zum Installieren des Azure SDK für Python, das Ihnen Zugriff auf den Azure-Speicher, den Service Bus und andere Azure-Dienste ermöglicht, `azure`ein:

![](./media/web-sites-python-create-deploy-flask-app/ptvs-install-package-dialog.png)

Klicken Sie mit der rechten Maustaste auf die virtuelle Umgebung, und wählen Sie **Generate requirements.txt** aus, um die Datei "requirements.txt" zu aktualisieren.

Führen Sie anschließend für die Änderungen an "requirements.txt" einen Commit im Git-Repository aus.

### <a name="deploy-to-azure"></a>Bereitstellen in Azure
Klicken Sie auf **Sync** oder **Push**, um eine Bereitstellung auszulösen.  Bei Wahl von "Synchronisierung" erfolgt sowohl ein Push- als auch ein Pull-Vorgang.

![](./media/web-sites-python-create-deploy-flask-app/ptvs-git-push.png)

Die erste Bereitstellung nimmt einige Zeit in Anspruch, da eine virtuelle Umgebung erstellt wird, Pakete installiert werden usw.

Visual Studio zeigt nicht den Fortschritt der Bereitstellung an.  Wenn Sie die Ausgabe prüfen möchten, lesen Sie den Abschnitt [Problembehandlung - Bereitstellung](#troubleshooting-deployment).

Navigieren Sie zur Azure-URL, um die Änderungen anzuzeigen.

## <a name="web-app-development---windows---command-line"></a>Web-App-Entwicklung – Windows - Befehlszeile
### <a name="clone-the-repository"></a>Klonen des Repositorys
Klonen Sie zunächst das Repository mithilfe der URL, die im Azure-Portal bereitgestellt ist, und fügen Sie das Azure-Repository als Remoterepository hinzu. Weitere Informationen finden Sie unter [Lokale Git-Bereitstellung in Azure App Service](app-service-deploy-local-git.md).

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### <a name="create-virtual-environment"></a>Erstellen einer virtuellen Umgebung
Wir erstellen eine neue virtuelle Umgebung zu Entwicklungszwecken (die nicht dem Repository hinzugefügt werden darf).  Virtuelle Umgebungen in Python sind nicht verschiebbar, sodass jeder Entwickler, der an der Anwendung arbeitet, seine eigene lokal erstellen muss.

Achten Sie darauf, die gleiche Version von Python zu verwenden, die für Ihre Web-App ausgewählt ist (in runtime.txt oder im Blatt **Anwendungseinstellungen** Ihrer Web-App im Azure-Portal).

Für Python 2.7:

    c:\python27\python.exe -m virtualenv env

Für Python 3.4:

    c:\python34\python.exe -m venv env

Installieren Sie alle externen Pakete, die Ihre Anwendung benötigt. Sie können die Datei "requirements.txt" im Stammverzeichnis des Repositorys verwenden, um die Pakete in der virtuellen Umgebung zu installieren:

    env\scripts\pip install -r requirements.txt

### <a name="run-using-development-server"></a>Ausführen mithilfe des Entwicklungsservers
Sie können die Anwendung unter einem Entwicklungsserver mit dem folgenden Befehl starten:

    env\scripts\python runserver.py

Die Konsole zeigt die URL und den Port, den der Server abhört:

![](./media/web-sites-python-create-deploy-flask-app/windows-run-local-flask.png)

Öffnen Sie Ihren Webbrowser mit dieser URL.

![](./media/web-sites-python-create-deploy-flask-app/windows-browser-flask.png)

### <a name="make-changes"></a>Vornehmen von Änderungen
Jetzt können Sie experimentieren, indem Sie Änderungen an der Anwendungsquellen und/oder Vorlagen vornehmen.

Nachdem Sie Ihre Änderungen getestet haben, führen Sie für diese im Git-Repository einen Commit durch:

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a>Installieren weiterer Pakete
Ihre Anwendung weist möglicherweise Abhängigkeiten über Python und Flask hinaus auf.

Sie können zusätzliche Pakete mit pip installieren.  Geben Sie beispielsweise zum Installieren des Azure SDK für Python, das Ihnen Zugriff auf Azure-Speicher, den Service Bus und andere Azure-Dienste bereitstellt, Folgendes ein:

    env\scripts\pip install azure

Stellen Sie sicher, dass "requirements.txt" aktualisiert wird:

    env\scripts\pip freeze > requirements.txt

Führen Sie für die Änderungen einen Commit aus:

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a>Bereitstellen in Azure
Um eine Bereitstellung auszulösen, übertragen Sie die Änderungen per Push in Azure:

    git push azure master

Die Ausgabe des Bereitstellungsskripts wird mit Angaben zur Erstellung der virtuellen Umgebung, Installation von Paketen und Erstellung von "web.config" angezeigt.

Navigieren Sie zur Azure-URL, um die Änderungen anzuzeigen.

## <a name="web-app-development---maclinux---command-line"></a>Web-App-Entwicklung – Mac/Linux - Befehlszeile
### <a name="clone-the-repository"></a>Klonen des Repositorys
Klonen Sie zunächst das Repository mithilfe der URL, die im Azure-Portal bereitgestellt ist, und fügen Sie das Azure-Repository als Remoterepository hinzu. Weitere Informationen finden Sie unter [Lokale Git-Bereitstellung in Azure App Service](app-service-deploy-local-git.md).

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### <a name="create-virtual-environment"></a>Erstellen einer virtuellen Umgebung
Wir erstellen eine neue virtuelle Umgebung zu Entwicklungszwecken (die nicht dem Repository hinzugefügt werden darf).  Virtuelle Umgebungen in Python sind nicht verschiebbar, sodass jeder Entwickler, der an der Anwendung arbeitet, seine eigene lokal erstellen muss.

Achten Sie darauf, die gleiche Version von Python zu verwenden, die für Ihre Web-App ausgewählt ist (in runtime.txt oder im Blatt **Anwendungseinstellungen** Ihrer Web-App im Azure-Portal).

Für Python 2.7:

    python -m virtualenv env

Für Python 3.4:

    python -m venv env
oder pyvenv env

Installieren Sie alle externen Pakete, die Ihre Anwendung benötigt. Sie können die Datei "requirements.txt" im Stammverzeichnis des Repositorys verwenden, um die Pakete in der virtuellen Umgebung zu installieren:

    env/bin/pip install -r requirements.txt

### <a name="run-using-development-server"></a>Ausführen mithilfe des Entwicklungsservers
Sie können die Anwendung unter einem Entwicklungsserver mit dem folgenden Befehl starten:

    env/bin/python runserver.py

Die Konsole zeigt die URL und den Port, den der Server abhört:

![](./media/web-sites-python-create-deploy-flask-app/mac-run-local-flask.png)

Öffnen Sie Ihren Webbrowser mit dieser URL.

![](./media/web-sites-python-create-deploy-flask-app/mac-browser-flask.png)

### <a name="make-changes"></a>Vornehmen von Änderungen
Jetzt können Sie experimentieren, indem Sie Änderungen an der Anwendungsquellen und/oder Vorlagen vornehmen.

Nachdem Sie Ihre Änderungen getestet haben, führen Sie für diese im Git-Repository einen Commit durch:

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a>Installieren weiterer Pakete
Ihre Anwendung weist möglicherweise Abhängigkeiten über Python und Flask hinaus auf.

Sie können zusätzliche Pakete mit pip installieren.  Geben Sie beispielsweise zum Installieren des Azure SDK für Python, das Ihnen Zugriff auf Azure-Speicher, den Service Bus und andere Azure-Dienste bereitstellt, Folgendes ein:

    env/bin/pip install azure

Stellen Sie sicher, dass "requirements.txt" aktualisiert wird:

    env/bin/pip freeze > requirements.txt

Führen Sie für die Änderungen einen Commit aus:

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a>Bereitstellen in Azure
Um eine Bereitstellung auszulösen, übertragen Sie die Änderungen per Push in Azure:

    git push azure master

Die Ausgabe des Bereitstellungsskripts wird mit Angaben zur Erstellung der virtuellen Umgebung, Installation von Paketen und Erstellung von "web.config" angezeigt.

Navigieren Sie zur Azure-URL, um die Änderungen anzuzeigen.

## <a name="troubleshooting---package-installation"></a>Problembehandlung - Paketinstallation
[!INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]

## <a name="troubleshooting---virtual-environment"></a>Problembehandlung – virtuelle Umgebung
[!INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="next-steps"></a>Nächste Schritte
Folgen Sie diesen Links, um weitere Informationen zu Flask- und Python Tools für Visual Studio zu erhalten: 

* [Dokumentation zu Flask]
* [Dokumentation zu Python Tools für Visual Studio]

Weitere Informationen zur Verwendung von Azure Table Storage und MongoDB:

* [Flask und MongoDB in Azure mit Python Tools für Visual Studio]
* [Flask und Azure-Tabellenspeicher in Azure mit Python Tools für Visual Studio]

Weitere Informationen finden Sie außerdem im [Python Developer Center](/develop/python/).

## <a name="whats-changed"></a>Änderungen
* Hinweise zu den Änderungen von Websites zum App Service finden Sie unter: [Azure App Service und vorhandene Azure-Dienste](http://go.microsoft.com/fwlink/?LinkId=529714)

<!--Link references-->
[Flask und MongoDB in Azure mit Python Tools für Visual Studio]: https://github.com/microsoft/ptvs/wiki/Flask-and-MongoDB-on-Azure
[Flask und Azure-Tabellenspeicher in Azure mit Python Tools für Visual Studio]: web-sites-python-ptvs-flask-table-storage.md

<!--External Link references-->
[Azure SDK für Python 2.7]: http://go.microsoft.com/fwlink/?linkid=254281
[Azure SDK für Python 3.4]: http://go.microsoft.com/fwlink/?linkid=516990
[python.org]: http://www.python.org/
[Git für Windows]: http://msysgit.github.io/
[GitHub für Windows]: https://windows.github.com/
[Python Tools für Visual Studio]: http://aka.ms/ptvs
[Python Tools 2.2 für Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Visual Studio]: http://www.visualstudio.com/
[Dokumentation zu Python Tools für Visual Studio]: http://aka.ms/ptvsdocs
[Dokumentation zu Flask]: http://flask.pocoo.org/ 




<!--HONumber=Feb17_HO3-->


