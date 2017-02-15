---
title: Konvertieren von WordPress in Multisite in Azure App Service
description: "Erfahren Sie, wie Sie eine vorhandene WordPress-Web-App, die über die Galerie in Azure erstellt wurde, in eine WordPress Multisite konvertieren."
services: app-service\web
documentationcenter: php
author: rmcmurray
manager: erikre
editor: 
ms.assetid: fe52dbf4-179c-42f1-adf9-d6a9af920c39
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 11/01/2016
ms.author: robmcm
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 5a0d7d5143879eaf0ee42a70a04d865a33879733


---
# <a name="convert-wordpress-to-multisite-in-azure-app-service"></a>Konvertieren von WordPress in Multisite in Azure App Service
## <a name="overview"></a>Übersicht
*Von [Ben Lobaugh][ben-lobaugh], [Microsoft Open Technologies Inc.][ms-open-tech]*

In diesem Tutorial erfahren Sie, wie Sie eine vorhandene WordPress-Web-App, die über den Katalog in Azure erstellt wurde, in eine WordPress Multisite-Installation konvertieren. Darüber hinaus erfahren Sie, wie Sie den einzelnen Unterwebsites eine benutzerdefinierte Domäne in Ihrer Installation zuweisen.

Es wird davon ausgegangen, dass Sie WordPress installiert haben. Wenn dies nicht der Fall ist, folgen Sie der Anleitung unter [Erstellen einer WordPress-Website über den Katalog in Azure][website-from-gallery].

Das Konvertieren einer vorhandenen WordPress-Einzelwebsite in eine Multisite-Installation ist i.Allg. sehr einfach. Viele der hier aufgeführten Schritte stammen direkt von der Seite [Create A Network][wordpress-codex-create-a-network] (Erstellen eines Netzwerks) auf [WordPress Codex](http://codex.wordpress.org).

Lassen Sie uns anfangen.

## <a name="allow-multisite"></a>Zulassen von Multisite
Sie müssen Multisite zunächst über die Datei `wp-config.php` mit der Konstanten **WP\_ALLOW\_MULTISITE** aktivieren. Es gibt zwei Möglichkeiten zum Bearbeiten von Web-App-Dateien: über FTP und über Git. Wenn Sie sich weder mit der Einrichtung der einen noch der anderen Methode auskennen, finden Sie weitere Informationen in den folgenden Lernprogrammen:

* [PHP-Website mit MySQL und FTP][website-w-mysql-and-ftp-ftp-setup]
* [PHP-Website mit MySQL und Git][website-w-mysql-and-git-git-setup]

Öffnen Sie die Datei `wp-config.php` in einem Editor Ihrer Wahl, und fügen Sie über der Zeile `/* That's all, stop editing! Happy blogging. */` Folgendes hinzu.

    /* Multisite */

    define( 'WP_ALLOW_MULTISITE', true );

Vergessen Sie nicht, die Datei zu speichern und erneut auf den Server zu laden!

## <a name="network-setup"></a>Netzwerkeinrichtung
Melden Sie sich beim *wp-admin*-Bereich Ihrer Web-App an. Im Menü **Extras** sollte ein neues Element mit dem Namen **Netzwerkinstallation** angezeigt werden. Klicken Sie auf **Netzwerkinstallation**, und geben Sie die Details zu Ihrem Netzwerk ein.

![Bildschirm zur Netzwerkeinrichtung][wordpress-network-setup]

In diesem Tutorial wird das Websiteschema *Unterverzeichnisse* verwendet, da dies immer funktionieren sollte. Später in diesem Tutorial richten wir benutzerdefinierte Domänen für die einzelnen Unterwebsites ein. Es sollte jedoch möglich sein, eine Unterdomäneninstallation einzurichten, wenn Sie eine Domäne über das [Azure-Portal](https://portal.azure.com) zuweisen und Platzhalter-DNS ordnungsgemäß einrichten.

Weitere Informationen zu Unterdomäneneinrichtungen im Vergleich zu Unterverzeichniseinrichtungen finden Sie im Artikel [Types of multisite network][wordpress-codex-types-of-networks] (Typen von Multisite-Netzwerken) im WordPress Codex.

## <a name="enable-the-network"></a>Aktivieren des Netzwerks
Das Netzwerk ist jetzt in der Datenbank konfiguriert. Es muss noch ein weiterer Schritt durchgeführt werden, um die Netzwerkfunktionalität zu aktivieren. Schließen Sie die `wp-config.php`-Einstellungen ab, und stellen Sie sicher, dass `web.config` die einzelnen Websites ordnungsgemäß weiterleitet.

Nachdem Sie auf der Seite **Netzwerkinstallation** auf die Schaltfläche `wp-config.php`Installieren`web.config` geklickt haben, versucht WordPress, die Dateien * und * zu aktualisieren. Sie sollten jedoch stets überprüfen, ob die Dateien erfolgreich aktualisiert wurden. Wenn nicht, werden in diesem Bildschirm die erforderlichen Aktualisierungen angezeigt. Bearbeiten und speichern Sie die Dateien.

Nachdem Sie diese Aktualisierungen durchgeführt haben, müssen Sie sich abmelden und erneut beim wp-admin-Dashboard anmelden.

Auf der Verwaltungsleiste sollte sich nun ein weiteres Menü mit der Bezeichnung **My Sites**befinden. Mit diesem Menü können Sie das neue Netzwerk über das **Network Admin** -Dashboard steuern.

## <a name="adding-custom-domains"></a>Hinzufügen von benutzerdefinierten Domänen
Mit dem Plug-In [WordPress MU Domain Mapping][wordpress-plugin-wordpress-mu-domain-mapping] können Sie einer beliebigen Website in Ihrem Netzwerk ganz einfach benutzerdefinierte Domänen hinzufügen. Damit das Plug-In ordnungsgemäß funktioniert, müssen Sie im Portal sowie bei der Domänenregistrierungsstelle einige zusätzliche Einrichtungen vornehmen.

## <a name="enable-domain-mapping-to-the-web-app"></a>Aktivieren der Domänenzuordnung zur Web-App
Mit dem **Kostenlos** [Kostenlos](http://go.microsoft.com/fwlink/?LinkId=529714) wird das Hinzufügen benutzerdefinierter Domänen zu Web-Apps nicht unterstützt. Sie müssen in den Modus **Shared** oder**Standard** wechseln. Gehen Sie dazu folgendermaßen vor:

* Melden Sie sich beim Azure-Portal an, und suchen Sie Ihre Web-App. 
* Klicken Sie in den **Einstellungen** auf die Registerkarte **Zentral hochskalieren**.
* Wählen Sie unter **Allgemein** entweder *SHARED* oder *STANDARD* aus.
* Klicken Sie unten auf der Seite auf **Speichern**

Es wird möglicherweise eine Meldung angezeigt, in der Sie aufgefordert werden, die Änderung zu bestätigen und anzuerkennen, dass die Web-App jetzt Kosten verursachen kann, je nach Nutzung und anderen Konfigurationen, die Sie festlegen.

Die Verarbeitung der neuen Einstellungen dauert einige Sekunden, sodass Sie nun mit dem Einrichten der Domäne beginnen können.

## <a name="verify-your-domain"></a>Überprüfen der Domäne
Bevor Azure-Web-Apps zulassen, dass Sie eine Domäne mit der Website verknüpfen können, müssen Sie zunächst sicherstellen, dass Sie über die Autorisierung zum Zuordnen verfügen. Hierzu müssen Sie dem DNS-Eintrag einen neuen CNAME-Datensatz hinzufügen.

* Melden Sie sich beim DNS-Manager Ihrer Domäne an.
* Erstellen Sie den neuen CNAME *awverify*
* Zeigen Sie mit *awverify* auf *awverify.IHRE_DOMÄNE.azurewebsites.net*.

Es kann einige Zeit dauern, bis die DNS-Änderungen vollständig übernommen werden. Wenn die folgenden Schritte daher nicht sofort funktionieren, warten Sie eine Weile, und versuchen Sie es erneut.

## <a name="add-the-domain-to-the-web-app"></a>Hinzufügen der Domäne zur Web-App
Navigieren Sie im Azure-Portal zurück zur Web-App, klicken Sie auf **Einstellungen** und dann auf **Benutzerdefinierte Domänen und SSL**.

Wenn die *SSL-Einstellungen* angezeigt werden, sehen Sie auch alle Felder, in die Sie sämtliche Domänen eintragen können, die Sie Ihrer Web-App zuordnen möchten. Wenn hier eine Domäne nicht aufgeführt wird, kann Sie nicht in WordPress zugeordnet werden, unabhängig von der Einrichtung des Domänen-DNS.

![Dialogfeld zum Verwalten benutzerdefinierter Domänen][wordpress-manage-domains]

Nachdem Sie die Domäne in das Textfeld eingegeben haben, überprüft Azure den CNAME-Eintrag , den Sie zuvor erstellt haben. Wenn das DNS nicht vollständig ausgefüllt wurde, wird eine rote Markierung angezeigt. Wenn der Vorgang erfolgreich abgeschlossen wurde, wird ein grünes Häkchen angezeigt. 

Notieren Sie sich die IP-Adresse, die unten im Dialogfeld aufgeführt wird. Sie benötigen diese IP-Adresse zum Einrichten des A-Datensatzes für Ihre Domäne.

## <a name="setup-the-domain-a-record"></a>Einrichten des A-Datensatzes der Domäne
Wenn die anderen Schritte erfolgreich durchgeführt wurden, können Sie Ihrer Azure-Web-App nun über einen DNS-A-Datensatz die Domäne zuordnen. 

Hierbei ist es wichtig zu beachten, dass Azure-Web-Apps sowohl CNAME- als auch A-Datensätze akzeptieren, wobei Sie jedoch einen A-Datensatz verwenden *müssen* , um die ordnungsgemäße Domänenzuordnung zu ermöglichen. Ein CNAME kann nicht an einen anderen CNAME weitergeleitet werden, den Azure für Sie mit "IHRE_DOMÄNE.azurewebsites.net" erstellt hat.

Kehren Sie mithilfe der IP-Adresse aus dem vorhergehenden Schritt zu Ihrem DNS-Manager zurück, und richten Sie den A-Datensatz so ein, das er auf diese IP verweist.

## <a name="install-and-setup-the-plugin"></a>Installieren und Einrichten des Plug-Ins
WordPress Multisite verfügt derzeit nicht über eine integrierte Methode zum Zuordnen von benutzerdefinierten Domänen. Es steht jedoch das Plug-In [WordPress MU Domain Mapping][wordpress-plugin-wordpress-mu-domain-mapping] zur Verfügung, mit dem diese Funktionalität hinzugefügt werden kann. Melden Sie sich im Bereich "Network Admin" der Website an, und installieren Sie das Plug-In **WordPress MU Domain Mapping** .

Nachdem Sie das Plug-In installiert und aktiviert haben, rufen Sie **Zentral hochskalieren** > **Domain Mapping** auf, um das Plug-In zu konfigurieren. Geben Sie im ersten Textfeld, *Server IP Address*, die IP-Adresse ein, die Sie zum Einrichten das A-Datensatzes für die Domäne verwendet haben. Legen Sie unter *Domain Options* die gewünschten Optionen für die Domäne fest (die Standardwerte sind in der Regel ausreichend), und klicken Sie auf **Save**.

## <a name="map-the-domain"></a>Zuordnen der Domäne
Rufen Sie das **Dashboard** der Website auf, der Sie die Domäne zuordnen möchten. Klicken Sie auf **Extras** > **Domain Mapping** (Domänenzuordnung), und geben Sie im Textfeld den Namen der neuen Domäne ein. Klicken Sie dann auf **Hinzufügen**.

Die neue Domäne wird standardmäßig neu in die automatisch generierte Standortdomäne geschrieben. Wenn der gesamte Datenverkehr an die neue Domäne gesendet werden soll, aktivieren Sie vor dem Speichern das Kontrollkästchen *Primary domain for this blog* . Sie können einer Website eine unbegrenzte Anzahl von Domänen hinzufügen, wobei jedoch nur eine die primäre sein kann.

## <a name="do-it-again"></a>Noch einmal
Azure-Web-Apps ermöglichen Ihnen, eine unbegrenzte Anzahl von Domänen zu einer Web-App hinzuzufügen. Um eine weitere Domäne hinzuzufügen, müssen Sie die in den Abschnitten **Überprüfen der Domäne** und **Einrichten des A-Datensatzes der Domäne** aufgeführten Schritte für jede Domäne erneut durchführen.    

> [!NOTE]
> Wenn Sie Azure App Service ausprobieren möchten, ehe Sie sich für ein Azure-Konto anmelden, können Sie unter [App Service testen](http://go.microsoft.com/fwlink/?LinkId=523751)sofort kostenlos eine kurzlebige Starter-Web-App in App Service erstellen. Keine Kreditkarte erforderlich, keine Verpflichtungen.
> 
> 

## <a name="whats-changed"></a>Änderungen
* Hinweise zu den Änderungen von Websites zum App Service finden Sie unter: [Azure App Service und vorhandene Azure-Dienste](http://go.microsoft.com/fwlink/?LinkId=529714)

[ben-lobaugh]: http://ben.lobaugh.net
[ms-open-tech]: http://msopentech.com
[website-from-gallery]: https://www.windowsazure.com/develop/php/tutorials/website-from-gallery/
[wordpress-codex-create-a-network]: http://codex.wordpress.org/Create_A_Network
[website-w-mysql-and-ftp-ftp-setup]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-ftp/#header-0
[website-w-mysql-and-git-git-setup]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-git/#header-1
[wordpress-network-setup]: ./media/web-sites-php-convert-wordpress-multisite/wordpress-network-setup.png
[wordpress-codex-types-of-networks]: http://codex.wordpress.org/Before_You_Create_A_Network#Types_of_multisite_network
[wordpress-plugin-wordpress-mu-domain-mapping]: http://wordpress.org/extend/plugins/wordpress-mu-domain-mapping/

[wordpress-manage-domains]: ./media/web-sites-php-convert-wordpress-multisite/wordpress-manage-domains.png





<!--HONumber=Nov16_HO3-->


