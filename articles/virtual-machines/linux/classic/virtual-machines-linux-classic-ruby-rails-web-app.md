---
title: Hosten einer Ruby on Rails-Website auf einem virtuellen Linux-Computer | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie eine Ruby on Rails-basierte Website auf Azure mithilfe eines virtuellen Linux-Computers einrichten und hosten.
services: virtual-machines-linux
documentationcenter: ruby
author: rmcmurray
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: aad32685-3550-4bff-9c73-beb8d70b3291
ms.service: virtual-machines-linux
ms.workload: web
ms.tgt_pltfrm: vm-linux
ms.devlang: ruby
ms.topic: article
ms.date: 12/22/2016
ms.author: robmcm
translationtype: Human Translation
ms.sourcegitcommit: ff60ebaddd3a7888cee612f387bd0c50799496ac
ms.openlocfilehash: 7b3c6da0e158c2824a5feb084a13eafe265762ce


---
# <a name="ruby-on-rails-web-application-on-an-azure-vm"></a>Ruby on Rails-Webanwendung auf Azure VM
In diesem Lernprogramm wird gezeigt, wie Sie eine Ruby on Rails-Website auf Azure mit einem virtuellen Linux-Computer hosten.  

Dieses Lernprogramm wurde mit Ubuntu Server 14.04 LTS getestet. Wenn Sie eine andere Linux-Distribution verwenden, müssen Sie die Schritte zur Rails-Installation möglicherweise anpassen.

> [!IMPORTANT]
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager-Bereitstellung und klassische Bereitstellung](../../../azure-resource-manager/resource-manager-deployment-model.md).  Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells.
> 
> 

## <a name="create-an-azure-vm"></a>Erstellen einer Azure-VM
Beginnen Sie, indem Sie eine Azure-VM mit einem Linux-Image erstellen.

Zur Erstellung des virtuellen Computers können Sie das klassische Azure-Portal oder die Azure-Befehlszeilenschnittstelle (CLI) verwenden.

### <a name="azure-management-portal"></a>Azure-Verwaltungsportal
1. Melden Sie sich beim [klassischen Azure-Portal](http://manage.windowsazure.com)
2. Klicken Sie auf **Neu** > **Compute** > **Virtueller Computer** > **Schnellerfassung**. Wählen Sie ein Linux-Image aus.
3. Geben Sie ein Kennwort ein.

Nachdem der virtuelle Computer bereitgestellt wurde, klicken Sie auf den Namen des virtuellen Computers und dann auf **Dashboard**. Suchen Sie den SSH-Endpunkt, der unter **SSH-Details**aufgeführt ist.

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle
Befolgen Sie die Schritte unter [Erstellen eines virtuellen Linux-Computers][vm-instructions].

Nachdem der virtuelle Computer bereitgestellt wurde, können Sie den SSH-Endpunkt mit dem folgenden Befehl abrufen:

    azure vm endpoint list <vm-name>  

## <a name="install-ruby-on-rails"></a>Installieren von Ruby on Rails
1. Verwenden Sie SSH zum Herstellen einer Verbindung mit dem virtuellen Computer.
2. Verwenden Sie in der SSH-Sitzung die folgenden Befehle zur Installation von Ruby auf dem virtuellen Computer:
   
        sudo apt-get update -y
        sudo apt-get upgrade -y
        sudo apt-get install ruby ruby-dev build-essential libsqlite3-dev zlib1g-dev nodejs -y
   
    Die Installation kann einige Minuten dauern. Wenn der Vorgang abgeschlossen ist, verwenden Sie folgenden Befehl, um zu überprüfen, dass Ruby installiert ist:
   
        ruby -v
   
    Dadurch wird die installierte Version von Ruby zurückgegeben.
3. Geben Sie den folgenden Befehl für die Installation von Rails ein:
   
        sudo gem install rails --no-rdoc --no-ri -V
   
    Verwenden Sie die Flags "--no-rdoc" und "--no-ri", um zur Beschleunigung des Vorgangs die Installation der Dokumentation zu überspringen.
    Die Ausführung dieses Befehls dauert wahrscheinlich etwas länger. Durch Hinzufügen des Befehls „-V“ können Sie Informationen zum Installationsstatus anzeigen.

## <a name="create-and-run-an-app"></a>Erstellen und Ausführen einer App
Wenn Sie noch über SSH angemeldet sind, führen Sie die folgenden Befehle aus:

    rails new myapp
    cd myapp
    rails server -b 0.0.0.0 -p 3000

Der Befehl [new](http://guides.rubyonrails.org/command_line.html#rails-new) erstellt eine neue Rails-App. Der Befehl [server](http://guides.rubyonrails.org/command_line.html#rails-server) startet den WEBrick-Webserver, der mit Rails bereitgestellt wird. (Für die Verwendung in einer Produktionsumgebung sollten Sie wahrscheinlich einen anderen Server, z. B. Unicorn oder Passenger, verwenden.)

Eine Ausgabe ähnlich der folgenden sollte angezeigt werden.

    => Booting WEBrick
    => Rails 4.2.1 application starting in development on http://0.0.0.0:3000
    => Run `rails server -h` for more startup options
    => Ctrl-C to shutdown server
    [2015-06-09 23:34:23] INFO  WEBrick 1.3.1
    [2015-06-09 23:34:23] INFO  ruby 1.9.3 (2013-11-22) [x86_64-linux]
    [2015-06-09 23:34:23] INFO  WEBrick::HTTPServer#start: pid=27766 port=3000

## <a name="add-an-endpoint"></a>Hinzufügen eines Endpunkts
1. Öffnen Sie das [klassische Azure-Portal][management-portal], und wählen Sie Ihren virtuellen Computer aus.
   
    ![Liste der virtuellen Computer][vmlist]
2. Wählen Sie oben auf der Seite **ENDPUNKTE** aus, und klicken Sie dann unten auf **ENDPUNKT HINZUFÜGEN**.
   
    ![Seite "Endpunkte"][endpoints]
3. Wählen Sie im Dialogfeld **ENDPUNKT HINZUFÜGEN** die Option „Add a standalone endpoint“ (Eigenständigen Endpunkt hinzufügen) aus, und klicken Sie auf die Pfeilschaltfläche **Weiter**.
   
    ![Dialogfeld für neuen Endpunkt][new-endpoint1]
4. Geben Sie im nächsten Dialogfeld die folgenden Informationen ein:
   
   * **NAME**: HTTP
   * **PROTOKOLL**: TCP
   * **ÖFFENTLICHER PORT**: 80
   * **PRIVATER PORT**: 3000
     
     Damit wird der öffentliche Port 80 erstellt, der den Datenverkehr an den privaten Port 3000 weiterleitet, den der Rails-Server überwacht.
     
     ![Dialogfeld für neuen Endpunkt][new-endpoint]
5. Klicken Sie auf das Häkchen, um den Endpunkt zu speichern.
6. Es wird eine Meldung angezeigt, dass die **Aktualisierung in Bearbeitung**ist. Nachdem diese Meldung ausgeblendet wurde, ist der Endpunkt aktiv. Sie können nun Ihre Anwendung testen, indem Sie zum DNS-Namen des virtuellen Computers navigieren. Die Website sollte in etwa wie folgt aussehen:
   
    ![Standard-Rails-Seite][default-rails-cloud]

## <a name="next-steps"></a>Nächste Schritte
In diesem Lernprogramm haben Sie die meisten Schritte manuell ausgeführt. In einer Produktionsumgebung würden Sie die App auf einem Entwicklungscomputer schreiben und in der Azure-VM bereitstellen. Außerdem wird in den meisten Produktionsumgebungen die Rails-Anwendung zusammen mit einem anderen Serverprozess wie Apache oder NginX gehostet, der die Weiterleitung von Anfragen an mehrere Instanzen der Rails-Anwendung und statische Ressourcen bearbeitet. Weitere Informationen finden Sie unter „http://rubyonrails.org/deploy/“.

Weitere Informationen über Ruby on Rails finden Sie in den [Ruby on Rails-Handbüchern][rails-guides].

Informationen zum Verwenden von Azure-Diensten in der Ruby-Anwendung finden Sie unter:

* [Speichern von unstrukturierten Daten mit Blobs][blobs]
* [Speichern von Schlüssel/Wert-Paaren mit Tabellen][tables]
* [Bereitstellen von Inhalten mit hoher Bandbreite über das Content Delivery Network][cdn-howto]

<!-- WA.com links -->
[blobs]: ../../../storage/storage-ruby-how-to-use-blob-storage.md
[cdn-howto]: https://azure.microsoft.com/develop/ruby/app-services/
[management-portal]: https://manage.windowsazure.com/
[tables]: ../../../storage/storage-ruby-how-to-use-table-storage.md
[vm-instructions]: ../../virtual-machines-linux-classic-createportal.md

<!-- External Links -->
[rails-guides]: http://guides.rubyonrails.org/
[sqlite3]: http://www.sqlite.org/

<!-- Images -->

[default-rails-cloud]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/basicrailscloud.png
[vmlist]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/vmlist.png
[endpoints]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/endpoints.png
[new-endpoint]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/newendpoint.png
[new-endpoint1]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/newendpoint1.png



<!--HONumber=Jan17_HO1-->


