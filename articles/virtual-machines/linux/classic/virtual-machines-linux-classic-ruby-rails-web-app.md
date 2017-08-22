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
ms.date: 06/27/2017
ms.author: robmcm
ms.translationtype: Human Translation
ms.sourcegitcommit: 3716c7699732ad31970778fdfa116f8aee3da70b
ms.openlocfilehash: 4735a1789c33b7cc51896e26ec8e079f9b0de7d9
ms.contentlocale: de-de
ms.lasthandoff: 06/30/2017

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

Zur Erstellung des virtuellen Computers können Sie das Azure-Portal oder die Azure-Befehlszeilenschnittstelle (CLI) verwenden.

### <a name="azure-portal"></a>Azure-Portal
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie auf **Neu**, und geben Sie dann „Ubuntu Server 14.04“ in das Suchfeld ein. Klicken Sie auf den Eintrag, der von der Suche zurückgegeben wird. Wählen Sie für das Bereitstellungsmodell **Klassisch** aus, und klicken Sie dann auf „Erstellen“.
3. Geben Sie auf dem Blatt „Basics“ (Grundlagen) die Werte für die Pflichtfelder an: Name (der VM), Benutzername, Authentifizierungstyp und die dazugehörigen Anmeldeinformationen, das Azure-Abonnement, die Ressourcengruppe und den Speicherort.

   ![Erstellen eines neuen Ubuntu-Images](./media/virtual-machines-linux-classic-ruby-rails-web-app/createvm.png)

4. Nachdem der virtuelle Computer bereitgestellt wurde, klicken Sie auf den Namen der VM, und klicken Sie auf **Endpunkte** in der Kategorie **Einstellungen**. Suchen Sie den SSH-Endpunkt, der unter **Eigenständig** aufgeführt ist.

   ![Standardendpunkt](./media/virtual-machines-linux-classic-ruby-rails-web-app/endpointsnewportal.png)

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle
Befolgen Sie die Schritte unter [Erstellen eines virtuellen Linux-Computers][vm-instructions].

Nachdem der virtuelle Computer bereitgestellt wurde, können Sie den SSH-Endpunkt mit dem folgenden Befehl abrufen:

    azure vm endpoint list <vm-name>  

## <a name="install-ruby-on-rails"></a>Installieren von Ruby on Rails
1. Verwenden Sie SSH zum Herstellen einer Verbindung mit dem virtuellen Computer.
2. Verwenden Sie in der SSH-Sitzung die folgenden Befehle zur Installation von Ruby auf dem virtuellen Computer:

        sudo apt-get update -y
        sudo apt-get upgrade -y

        sudo apt-add-repository ppa:brightbox/ruby-ng
        sudo apt-get update
        sudo apt-get install ruby2.4

        > [!TIP]
        > The brightbox repository contains the current Ruby distribution.

    Die Installation kann einige Minuten dauern. Wenn der Vorgang abgeschlossen ist, verwenden Sie folgenden Befehl, um zu überprüfen, dass Ruby installiert ist:

        ruby -v

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
1. Wechseln Sie zum [Azure-Portal] [https://portal.azure.com], und wählen Sie Ihren virtuellen Computer.

2. Klicken Sie unter **Einstellungen** am linken Rand der Seite auf **Endpunkte**.

3. Klicken Sie am oberen Seitenrand auf **Hinzufügen**.

4. Geben Sie im Dialogfeld **Endpunkt hinzufügen** die folgenden Informationen ein:

   * **Name**: HTTP
   * **Protokoll**: TCP
   * **Öffentlicher Port**: 80
   * **Privater Port**: 3000
   * **Fließende IP-Adresse**: Deaktiviert
   * **Zugriffssteuerungsliste – Reihenfolge**: 1001, oder einen anderen Wert, der die Priorität dieser Zugriffsregel festlegt.
   * **Zugriffssteuerungsliste – Name**: allowHTTP
   * **Zugriffssteuerungsliste – Aktion**: Zulassen
   * **Zugriffssteuerungsliste – Remotesubnetz**: 1.0.0.0/16

     Dieser Endpunkt hat den Port 80, der den Datenverkehr an den privaten Port 3000 weiterleitet, den der Rails-Server überwacht. Die Regel für die Zugriffssteuerungslisten lässt öffentlichen Datenverkehr über Port 80 zu.

     ![new-endpoint](./media/virtual-machines-linux-classic-ruby-rails-web-app/createendpoint.png)

5. Klicken Sie zum Speichern der Richtlinie auf OK.

6. Die Meldung **Endpunkt des virtuellen Computers wird gespeichert** sollte angezeigt werden. Nachdem diese Meldung ausgeblendet wurde, ist der Endpunkt aktiv. Sie können nun Ihre Anwendung testen, indem Sie zum DNS-Namen des virtuellen Computers navigieren. Die Website sollte in etwa wie folgt aussehen:

    ![Standard-Rails-Seite][default-rails-cloud]

## <a name="next-steps"></a>Nächste Schritte
In diesem Lernprogramm haben Sie die meisten Schritte manuell ausgeführt. In einer Produktionsumgebung würden Sie die App auf einem Entwicklungscomputer schreiben und in der Azure-VM bereitstellen. Außerdem wird in den meisten Produktionsumgebungen die Rails-Anwendung zusammen mit einem anderen Serverprozess wie Apache oder NginX gehostet, der die Weiterleitung von Anfragen an mehrere Instanzen der Rails-Anwendung und statische Ressourcen bearbeitet. Weitere Informationen finden Sie unter „http://rubyonrails.org/deploy/“.

Weitere Informationen über Ruby on Rails finden Sie in den [Ruby on Rails-Handbüchern][rails-guides].

Informationen zum Verwenden von Azure-Diensten in der Ruby-Anwendung finden Sie unter:

* [Speichern von unstrukturierten Daten mit Blobs][blobs]
* [Speichern von Schlüssel/Wert-Paaren mit Tabellen][tables]
* [Bereitstellen von Inhalten mit hoher Bandbreite über das Content Delivery Network][cdn-howto]

<!-- WA.com links -->
[blobs]:../../../storage/blobs/storage-ruby-how-to-use-blob-storage.md
[cdn-howto]:https://azure.microsoft.com/develop/ruby/app-services/
[tables]:../../../cosmos-db/table-storage-how-to-use-ruby.md
[vm-instructions]:createportal.md

<!-- External Links -->
[rails-guides]:http://guides.rubyonrails.org/
[sqlite3]:http://www.sqlite.org/

<!-- Images -->

[default-rails-cloud]:./media/virtual-machines-linux-classic-ruby-rails-web-app/basicrailscloud.png
[vmlist]:./media/virtual-machines-linux-classic-ruby-rails-web-app/vmlist.png
[endpoints]:./media/virtual-machines-linux-classic-ruby-rails-web-app/endpoints.png
[new-endpoint]:./media/virtual-machines-linux-classic-ruby-rails-web-app/newendpoint.png
[new-endpoint1]:./media/virtual-machines-linux-classic-ruby-rails-web-app/newendpoint1.png

