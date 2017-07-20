---
title: Python-Web-App mit Django auf einer Azure Linux-VM | Microsoft-Dokumentation
description: "Hier erfahren Sie, wie Sie mithilfe eines virtuellen Linux-Computers eine Django-basierte Web-App in Azure hosten können."
services: virtual-machines-linux
documentationcenter: python
author: huguesv
manager: wpickett
editor: 
tags: azure-resource-manager
ms.assetid: 00ad4c2c-4316-4f9a-913f-f7f49b158db7
ms.service: virtual-machines-linux
ms.workload: web
ms.tgt_pltfrm: vm-linux
ms.devlang: python
ms.topic: article
ms.date: 05/31/2017
ms.author: huvalo
ms.translationtype: Human Translation
ms.sourcegitcommit: 6adaf7026d455210db4d7ce6e7111d13c2b75374
ms.openlocfilehash: 6e2ab8c7da7496d0e2b567a4bdc9341adcf01552
ms.contentlocale: de-de
ms.lasthandoff: 06/22/2017


---
# <a name="django-hello-world-web-app-on-a-linux-vm"></a>Django-Web-App „Hallo Welt“ auf einem virtuellen Linux-Computer
> [!div class="op_single_selector"]
> * [Windows](../windows/classic/python-django-web-app.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
> * [Mac/Linux](../windows/classic/python-django-web-app.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
> 
> 

<br>

In diesem Tutorial wird gezeigt, wie eine Django-basierte Website in Linux in Azure Virtual Machines gehostet wird. In diesem Tutorial werden keine Erfahrungen mit Azure vorausgesetzt. Wenn Sie dieses Tutorial abschließen, können Sie eine aktive Django-basierte Anwendung in der Cloud haben.

In diesem Artikel werden folgende Themen erläutert:

* Einrichten eines virtuellen Azure-Computers als Host von Django. Obwohl in diesem Tutorial die Vorgehensweise für **Linux** erläutert wird, können Sie die Schritte auch für eine Windows Server-VM nutzen, die in Azure gehostet wird. 
* Erstellen Sie eine neue Django-Anwendung in Linux.

Das Tutorial veranschaulicht das Erstellen einer einfachen Hallo Welt-Webanwendung. Die Anwendung wird auf einem virtuellen Azure-Computer gehostet.

Der folgende Screenshot zeigt die fertige Anwendung:

![Ein Browserfenster zeigt die Seite „Hello World“ in Azure an](./media/python-django-web-app/mac-linux-django-helloworld-browser.png)

[!INCLUDE [create-account-and-vms-note](../../../includes/create-account-and-vms-note.md)]

## <a name="create-and-set-up-an-azure-virtual-machine-to-host-django"></a>Erstellen und Einrichten eines virtuellen Azure-Computers als Host von Django

1. Informationen zum Erstellen eines virtuellen Azure-Computers mit der Ubuntu Server 14.04 LTS-Distribution finden Sie unter [Erstellen eines virtuellen Linux-Computers im Azure-Portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Sie können auch die Kennwortauthentifizierung auswählen, anstatt einen öffentlichen SSH-Schlüssel zu verwenden.
2. Informationen dazu, wie Sie die Netzwerksicherheitsgruppe so bearbeiten, dass eingehender HTTP-Datenverkehr an Port 80 zugelassen wird, finden Sie unter [Erstellen von Netzwerksicherheitsgruppen im Azure-Portal](../../virtual-network/virtual-networks-create-nsg-arm-pportal.md).
3. (Optional) Standardmäßig hat der neue virtuelle Computer keinen vollständig qualifizierten Domänennamen (FQDN).  Informationen zum Erstellen eines virtuellen Computers mit einem FQDN finden Sie unter [Erstellen eines FQDN im Azure-Portal für eine Windows-VM](../windows/portal-create-fqdn.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Dieser Schritt ist für den Abschluss des Tutorials nicht erforderlich.

## <a id="setup"> </a>Einrichten der Entwicklungsumgebung
> [!NOTE]
> Wenn Sie Python installieren müssen oder die Clientbibliotheken verwenden möchten, finden Sie weitere Informationen im [Installationshandbuch für Python](../../python-how-to-install.md).

Auf der Ubuntu Linux-VM ist Python 2.7 vorinstalliert; dies gilt jedoch nicht für Apache oder Django. Führen Sie die folgenden Schritte aus, um eine Verbindung mit dem virtuellen Computer herzustellen sowie um Apache und Django zu installieren:

1. Öffnen Sie ein neues Terminalfenster.
2. Geben Sie den folgenden Befehl ein, um eine Verbindung mit dem virtuellen Azure-Computer herzustellen. Wenn Sie keinen FQDN erstellt haben, können Sie eine Verbindung mit der öffentlichen IP-Adresse herstellen, die in der Zusammenfassung zum virtuellen Computer im Azure-Portal angezeigt wird.
   
       $ ssh yourusername@yourVmUrl
3. Geben Sie die folgenden Befehle ein, um Django zu installieren:
   
       $ sudo apt-get install python-setuptools python-pip
       $ sudo pip install django
4. Geben Sie den folgenden Befehl ein, um Apache mit mod-wsgi zu installieren:
   
       $ sudo apt-get install apache2 libapache2-mod-wsgi

## <a name="create-a-new-django-app"></a>Erstellen einer neuen Django-App
1. Wenn SSH für den Zugriff auf die VM verwendet werden soll, öffnen Sie das Terminalfenster, das Sie im vorherigen Abschnitt verwendet haben.
2. Geben Sie die folgenden Befehle ein, um ein neues Django-Projekt zu erstellen:
   
       $ cd /var/www
       $ sudo django-admin.py startproject helloworld
   
   Mit dem Skript `django-admin.py` wird eine Grundstruktur für Django-basierte Websites erstellt:
   
   * `helloworld/manage.py` hilft Ihnen, das Hosting für Ihre Django-basierte Website zu starten und zu beenden.
   * `helloworld/helloworld/settings.py` enthält Django-Einstellungen für Ihre Anwendung.
   * `helloworld/helloworld/urls.py` enthält den Zuordnungscode zwischen den einzelnen URLs und deren Ansicht.
3. Erstellen Sie im Verzeichnis „/var/www/helloworld/helloworld“ eine neue Datei mit dem Namen „views.py“. Diese Datei enthält die Ansicht, mit der die Seite „Hello World“ gerendert wird. Geben Sie im Code-Editor die folgenden Befehle ein:
   
       from django.http import HttpResponse
       def home(request):
           html = "<html><body>Hello World!</body></html>"
           return HttpResponse(html)
4. Ersetzen Sie den Inhalt der Datei „urls.py“ durch die folgenden Befehle:
   
       from django.conf.urls import patterns, url
       urlpatterns = patterns('',
           url(r'^$', 'helloworld.views.home', name='home'),
       )

## <a name="set-up-apache"></a>Einrichten von Apache
1. Erstellen Sie im Ordner „/etc/apache2/sites-available/helloworld.conf“ eine virtuelle Apache-Hostkonfigurationsdatei. Legen Sie den Inhalt auf die folgenden Werte fest. Ersetzen Sie *yourVmName* durch den tatsächlichen Namen des verwendeten Computers (z.B. *pyubuntu*).
   
       <VirtualHost *:80>
       ServerName yourVmName
       </VirtualHost>
       WSGIScriptAlias / /var/www/helloworld/helloworld/wsgi.py
       WSGIPythonPath /var/www/helloworld
2. Aktivieren Sie die Website mit dem folgenden Befehl:
   
       $ sudo a2ensite helloworld
3. Starten Sie Apache mit dem folgenden Befehl neu:
   
       $ sudo service apache2 reload
4. Laden Sie die Webseite in Ihrem Browser:
   
   ![Ein Browserfenster zeigt die Seite „Hello World“ in Azure an](./media/python-django-web-app/mac-linux-django-helloworld-browser.png)

## <a name="shut-down-your-azure-virtual-machine"></a>Herunterfahren des virtuellen Azure-Computers
Wenn Sie dieses Tutorial durchgearbeitet haben, empfiehlt es sich, die für das Tutorial erstellte Azure-VM herunterzufahren oder zu entfernen. Dadurch werden Ressourcen für andere Tutorials freigegeben, und Sie können zusätzliche Gebühren für die Azure-Nutzung vermeiden.


