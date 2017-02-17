---
title: Professionelle WordPress-Websites in Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine leistungsstarke WordPress-Website in Azure App Service hosten.
services: app-service\web
documentationcenter: 
author: sunbuild
manager: yochayk
editor: 
ms.assetid: 22d68588-2511-4600-8527-c518fede8978
ms.service: app-service-web
ms.devlang: php
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 10/24/2016
ms.author: sumuth
translationtype: Human Translation
ms.sourcegitcommit: 541dd7bed5a4a9e306642b2c36dd7c9d45aec690
ms.openlocfilehash: fcaff51b2897b1b0bce8b1c503adfd9425c9fbb4


---
# <a name="enterprise-class-wordpress-on-azure"></a>Professionelle WordPress-Websites in Azure
Azure App Service bietet eine skalierbare, sichere und benutzerfreundliche Umgebung für große unternehmenswichtige [WordPress][wordpress]-Websites. Microsoft selbst betreibt leistungsstarke Websites wie die [Office][officeblog]- und [Bing][bingblog]-Blogs. In diesem Artikel wird erläutert, wie Sie das Web-Apps-Feature von Microsoft Azure App Service verwenden, um eine leistungsfähige, cloudbasierte WordPress-Website einzurichten und zu verwalten, die eine hohe Anzahl von Besuchern bewältigen kann.

## <a name="architecture-and-planning"></a>Architektur und Planung
Für eine grundlegende WordPress-Installation gelten nur zwei Anforderungen:

* **MySQL-Datenbank**: erhältlich über [ClearDB im Azure Marketplace][cdbnstore]. Alternativ dazu können Sie auch in Azure Virtual Machines eine eigene MySQL-Installation unter [Windows][mysqlwindows] oder [Linux][mysqllinux] verwalten.

  > [!NOTE]
  > ClearDB stellt verschiedene MySQL-Konfigurationen bereit. Jede Konfiguration weist unterschiedliche Leistungsmerkmale auf. Im [Azure Store][cdbnstore] finden Sie Informationen zu Angeboten, die entweder über den Azure Store oder direkt wie auf der [ClearDB-Website](http://www.cleardb.com/pricing.view) gezeigt bereitgestellt werden.
  >
  >
* **PHP 5.2.4 oder höher**: Azure App Service stellt derzeit die [PHP-Versionen 5.4, 5.5 und 5.6][phpwebsite] bereit.

  > [!NOTE]
  > Wir empfehlen, stets mit der neuesten Version von PHP zu arbeiten, um sicherzustellen, dass Sie über die neuesten Sicherheitskorrekturen verfügen.
  >
  >

### <a name="basic-deployment"></a>Grundlegende Bereitstellung
Wenn Sie nur die grundlegenden Anforderungen benötigen, können Sie eine Standardlösung innerhalb einer Azure-Region erstellen.

![Eine Azure-Web-App und MySQL-Datenbank, gehostet in einer einzelnen Azure-Region][basic-diagram]

Sie können zwar mehrere Web-Apps-Instanzen der Website erstellen, um Ihre Anwendung horizontal hochzuskalieren. Dennoch wird die gesamte Konfiguration in den Rechenzentren einer bestimmten geografischen Region gehostet. Besucher, die sich außerhalb dieser Region befinden, bemerken möglicherweise lange Antwortzeiten bei Verwendung der Website. Wenn die Rechenzentren in dieser Region ausfallen, ist auch Ihre Anwendung nicht mehr verfügbar.

### <a name="multi-region-deployment"></a>Bereitstellung in mehreren Regionen
Durch Verwendung von Azure [Traffic Manager][trafficmanager] können Sie Ihre WordPress-Website auf mehrere geografische Regionen skalieren und dennoch allen Besuchern die gleiche URL bereitstellen. Alle Besucher durchlaufen Traffic Manager und werden anschließend basierend auf der Lastenausgleichskonfiguration an eine Region weitergeleitet.

![Eine in mehreren Regionen gehostete Azure-Web-App mit CDBR-Hochverfügbarkeitsrouter zum regionsübergreifenden Weiterleiten an MySQL][multi-region-diagram]

Innerhalb jeder Region kann die WordPress-Website weiterhin über mehrere Web-Apps-Instanzen skaliert werden, diese Skalierung ist jedoch regionsspezifisch. Regionen mit hohem und niedrigem Datenverkehrsaufkommen können unterschiedlich skaliert werden.

Um Datenverkehr an mehrere MySQL-Datenbanken zu replizieren und weiterzuleiten, können Sie [ClearDB-Hochverfügbarkeitsrouter (CDBRs)][cleardbscale] (links) oder [MySQL Cluster Carrier Grade Edition (CGE)][cge] verwenden.

### <a name="multi-region-deployment-with-media-storage-and-caching"></a>Bereitstellung in mehreren Regionen mit Medienspeicherung und Zwischenspeichern
Wenn die Website Uploads zulässt oder Mediendateien hostet, verwenden Sie Azure Blob Storage. Wenn Sie einen Cache benötigen, ziehen Sie [Redis Cache][rediscache], [Memcache Cloud](http://azure.microsoft.com/gallery/store/garantiadata/memcached/), [MemCachier](http://azure.microsoft.com/gallery/store/memcachier/memcachier/) oder eines der anderen Cacheangebote im [Azure Store](http://azure.microsoft.com/gallery/store/) in Betracht.

![Eine in mehreren Regionen gehostete Azure-Web-App mit CDBR-Hochverfügbarkeitsrouter für MySQL, Managed Cache, Blob Storage und Content Delivery Network][performance-diagram]

Blobspeicher wird standardmäßig geografisch auf mehrere Regionen verteilt, sodass Sie sich um die Replikation von Dateien auf allen Websites keine Gedanken machen müssen. Sie können auch das Azure [Content Delivery Network (CDN)][cdn] für Blob Storage aktivieren, das Dateien an Endknoten verteilt, die näher bei Ihren Besuchern sind.

### <a name="planning"></a>Planung
#### <a name="additional-requirements"></a>Weitere Anforderungen
| Maßnahme | Option |
| --- | --- |
| **Hochladen oder Speichern großer Dateien** |[WordPress-Plug-In für die Verwendung von Blob Storage][storageplugin]. |
| **Senden von E-Mails** |[SendGrid][storesendgrid] und das [WordPress-Plug-In für die Verwendung von SendGrid][sendgridplugin]. |
| **Benutzerdefinierte Domänennamen** |[Konfigurieren eines benutzerdefinierten Domänennamens in Azure App Service][customdomain]. |
| **HTTPS** |[Aktivieren von HTTPS für eine Web-App in Azure App Service][httpscustomdomain]. |
| **Überprüfung vor Wechsel zur Produktionsumgebung** |[Einrichten von Stagingumgebungen für Web-Apps in Azure App Service][staging]. <p>Beim Verschieben einer Web-App aus der Staging- in die Produktionsumgebung wird auch die WordPress-Konfiguration verschoben. Stellen Sie sicher, dass alle Einstellungen an die Anforderungen Ihrer Produktions-App angepasst werden, bevor Sie die Staging-App in die Produktion verschieben.</p> |
| **Überwachung und Problembehandlung** |[Aktivieren der Diagnoseprotokollierung für Web-Apps in Azure App Service][log] und [Überwachen von Web-Apps in Azure App Service][monitor]. |
| **Bereitstellen Ihrer Website** |[Bereitstellen von Web-Apps in Azure App Service][deploy]. |

#### <a name="availability-and-disaster-recovery"></a>Verfügbarkeit und Notfallwiederherstellung
| Maßnahme | Option |
| --- | --- |
| **Vornehmen eines Lastenausgleichs für Websites** oder **Geografische Verteilung von Websites** |[Weiterleiten von Datenverkehr mit Azure Traffic Manager][trafficmanager]. |
| **Sichern und Wiederherstellen** |[Sichern von Web-Apps in Azure App Service][backup] und [Wiederherstellen einer Web-App in Azure App Service][restore]. |

#### <a name="performance"></a>Leistung
Die Leistung in der Cloud wird primär durch Zwischenspeichern und horizontales Skalieren erzielt. Es sollten aber der Arbeitsspeicher, die Bandbreite sowie weitere Attribute beim Web-Apps-Hosting berücksichtigt werden.

| Maßnahme | Option |
| --- | --- |
| **Grundlagen von App Service-Instanzfunktionen** |[Preisübersicht, einschließlich der Funktionen von App Service-Stufen][websitepricing]. |
| **Cacheressourcen** |[Redis Cache][rediscache], [Memcache Cloud](/gallery/store/garantiadata/memcached/), [MemCachier](/gallery/store/memcachier/memcachier/) oder eines der anderen Cacheangebote im [Azure Store](/gallery/store/). |
| **Skalieren Ihrer Anwendung** |[Skalieren einer Web-App in Azure App Service][websitescale] und [ClearDB-Hochverfügbarkeitsrouting][cleardbscale]. Wenn Sie sich für das Hosten und Verwalten Ihrer eigenen MySQL-Installation entscheiden, sollten Sie für die horizontale Skalierung [MySQL Cluster CGE][cge] in Erwägung ziehen. |

#### <a name="migration"></a>Migration
Es gibt zwei Möglichkeiten, eine vorhandene WordPress-Website zu Azure App Service zu migrieren:

* **[WordPress-Export][export]**: Diese Methode exportiert die Inhalte Ihres Blogs. Anschließend können Sie die Inhalte mithilfe des [WordPress Importer-Plug-Ins][import] in eine neue WordPress-Website in Azure App Service importieren.

  > [!NOTE]
  > Mit diesem Prozess können Sie lediglich Ihre Inhalte migrieren, nicht jedoch Plug-Ins, Designs oder andere Anpassungen. Sie müssen diese Komponenten manuell erneut installieren.
  >
  >
* **Manuelle Migration**: [Sichern Sie Ihre Website][wordpressbackup] und [Datenbank][wordpressdbbackup], und stellen Sie diese dann manuell in einer Web-App in Azure App Service und der zugehörigen MySQL-Datenbank wieder her. Diese Methode eignet sich gut, um hochgradig angepasste Websites zu migrieren, da sie Ihnen die Mühe erspart, Plug-Ins, Themen und andere Anpassungen manuell zu installieren.

## <a name="step-by-step-instructions"></a>Schrittweise Anweisungen
### <a name="create-a-wordpress-site"></a>Erstellen einer WordPress-Website
1. Nutzen Sie den [Azure Marketplace][cdbnstore], um in der Region bzw. den Regionen, in der/denen Ihre Website gehostet wird, eine MySQL-Datenbank mit der im Abschnitt [Architektur und Planung](#planning) ermittelten Größe zu erstellen.
2. Führen Sie die Schritte unter [Erstellen einer WordPress-Web-App in Azure App Service][createwordpress] aus, um eine neue WordPress-Web-App zu erstellen. Wählen Sie beim Erstellen der Web-App die Option **Vorhandene MySQL-Datenbank verwenden** und anschließend die in Schritt 1 erstellte Datenbank aus.

Wenn Sie eine vorhandene WordPress-Website migrieren, helfen Ihnen nach dem Erstellen einer neuen Web-App die Informationen unter [Migrieren einer vorhandenen WordPress-Website in Azure](#Migrate-an-existing-WordPress-site-to-Azure) weiter.

### <a name="migrate-an-existing-wordpress-site-to-azure"></a>Migrieren einer vorhandenen WordPress-Website in Azure
Wie im Abschnitt [Architektur und Planung](#planning) erwähnt, gibt es zwei Möglichkeiten zum Migrieren einer WordPress-Website:

* **Export und Import** für Websites ohne zahlreiche Anpassungen, deren Inhalt Sie lediglich verschieben möchten.
* **Sicherung und Wiederherstellung** für Websites mit zahlreichen Anpassungen, bei denen Sie alles verschieben möchten.

Befolgen Sie die Anweisungen zum Migrieren Ihrer Website in den folgenden Abschnitten.

#### <a name="the-export-and-import-method"></a>Die Export- und Importmethode
1. Verwenden Sie den [WordPress-Export][export] zum Exportieren Ihrer vorhandenen Website.
2. Erstellen Sie anhand der Schritte im Abschnitt [Erstellen einer WordPress-Website](#Create-a-new-WordPress-site) eine Web-App.
3. Melden Sie sich im [Azure-Portal][mgmtportal] bei Ihrer WordPress-Website an, und klicken Sie dann auf **Plug-Ins** > **Neu hinzufügen**. Suchen Sie das Plug-In **WordPress Importer**, und installieren Sie es.
4. Klicken Sie nach der Installation des WordPress Importer-Plug-Ins auf **Extras** > **Importieren**, und klicken Sie dann auf **WordPress**, um das WordPress-Importer-Plug-In zu verwenden.
5. Klicken Sie auf der Seite **WordPress importieren** auf **Datei wählen**. Suchen Sie die aus Ihrer vorhandenen WordPress-Website exportierte WXR-Datei, und klicken Sie auf **Datei hochladen und importieren**.
6. Klicken Sie auf **Senden**. Sie werden informiert, wenn der Import erfolgreich war.
7. Nachdem Sie alle Schritte abgeschlossen haben, starten Sie Ihre Website über das Blatt **App-Dienste** im [Azure-Portal][mgmtportal] neu.

Nach dem Importieren der Website müssen Sie ggf. die folgenden Schritte ausführen, um Einstellungen zu aktivieren, die nicht in der Importdatei enthalten sind.

| Bei Verwendung von | Maßnahme |
| --- | --- |
| **Permalinks** |Klicken Sie auf dem WordPress-Dashboard der neuen Website auf **Einstellungen** > **Permalinks**, und aktualisieren Sie anschließend die Struktur der Permalinks. |
| **Bild-/Medienlinks** |Um Links auf den neuen Speicherort zu aktualisieren, verwenden Sie das Plug-In [Velvet Blues Update URLs][velvet] – ein Tool zum Suchen und Ersetzen –, oder aktualisieren Sie die Links in der Datenbank manuell. |
| **Designs** |Wechseln Sie zu **Darstellung** > **Design**, und aktualisieren Sie das Websitedesign den Anforderungen entsprechend. |
| **Menüs** |Falls Ihr Design Menüs unterstützt, ist in Links zu Ihrer Homepage möglicherweise noch das alte Unterverzeichnis eingebettet. Wechseln Sie zu **Darstellung** > **Menüs**, und aktualisieren Sie diese. |

#### <a name="the-backup-and-restore-method"></a>Die Sicherungs- und Wiederherstellungsmethode
1. Sichern Sie Ihre vorhandene WordPress-Website mithilfe der Informationen unter [WordPress Backups][wordpressbackup].
2. Sichern Sie Ihre vorhandene Datenbank anhand der Informationen unter [Backing up your database][wordpressdbbackup] (Sichern Ihrer Datenbank).
3. Erstellen Sie eine Datenbank, und stellen Sie die Sicherung wieder her.

   1. Erwerben Sie eine neue Datenbank im [Azure Marketplace][cdbnstore], oder richten Sie auf einem virtuellen [Windows][mysqlwindows]- oder [Linux][mysqllinux]-Computer eine MySQL-Datenbank ein.
   2. Stellen Sie mit einem MySQL-Client wie z.B. [MySQL Workbench][workbench] eine Verbindung mit der neuen Datenbank her, und importieren Sie Ihre WordPress-Datenbank.
   3. Ändern Sie in der Datenbank die Domäneneinträge entsprechend Ihrer neuen Azure App Service-Domäne, z.B.: mywordpress.azurewebsites.net. Verwenden Sie das [Skript „Search and Replace for WordPress Databases“][searchandreplace], um alle Instanzen sicher zu ändern.
4. Erstellen Sie im Azure-Portal eine Web-App, und veröffentlichen Sie die WordPress-Sicherung.

   1. Um im [Azure-Portal][mgmtportal] eine neue Web-App mit einer Datenbank zu erstellen, klicken Sie auf **Neu** > **Web + Mobile** > **Azure Marketplace** > **Web-Apps** > **Web-App + SQL** (oder **Web-App + MySQL**) > **Erstellen**. Konfigurieren Sie alle erforderlichen Einstellungen, um eine leere Web-App zu erstellen.
   2. Suchen Sie in Ihrer WordPress-Sicherung die Datei **wp-config.php**, und öffnen Sie sie in einem Editor. Ersetzen Sie die folgenden Einträge durch die Informationen für Ihre neue MySQL-Datenbank:

      * **DB_NAME**: Der Benutzername der Datenbank.
      * **DB_USER**: Der Benutzername für den Zugriff auf die Datenbank.
      * **DB_PASSWORD**: Das Kennwort des Benutzers.

        Nachdem Sie diese Einträge geändert haben, speichern und schließen Sie die Datei **wp-config.php**.
   3. Verwenden Sie die Informationen unter [Bereitstellen einer Web-App in Azure App Service][deploy], um die gewünschte Bereitstellungsmethode zu aktivieren, und stellen Sie anschließend Ihre WordPress-Sicherung in Ihrer Web-App in Azure App Service bereit.
5. Nach der Bereitstellung der WordPress-Website sollten Sie über die URL „*.azurewebsite.net“ auf die neue Website (als App Service-Web-App) zugreifen können.

### <a name="configure-your-site"></a>Konfigurieren Ihrer Website
Befolgen Sie nach dem Erstellen oder Migrieren der WordPress-Website die folgenden Informationen zum Verbessern der Leistung und Ermöglichen zusätzlicher Funktionalität.

| Maßnahme | Option |
| --- | --- |
| **Festlegen von App Service-Planmodus und -Größe und Aktivieren der Skalierung** |[Skalieren einer Web-App in Azure App Service][websitescale]. |
| **Aktivieren dauerhafter Datenbankverbindungen** |WordPress arbeitet standardmäßig nicht mit dauerhaften Datenbankverbindungen, was dazu führen kann, dass Ihre Verbindung mit der Datenbank nach mehreren Verbindungen gedrosselt wird. Um dauerhafte Verbindungen zu ermöglichen, installieren Sie das Plug-In [Persistent Database Connection Updater](https://wordpress.org/plugins/persistent-database-connection-updater/installation/). |
| **Verbessern der Leistung** |<ul><li><p><a href="https://azure.microsoft.com/en-us/blog/disabling-arrs-instance-affinity-in-windows-azure-web-sites/">Deaktivieren Sie das ARR-Cookie</a> – dies kann die Leistung verbessern, wenn WordPress in mehreren Web-Apps-Instanzen ausgeführt wird.</p></li><li><p>Aktivieren von Caching. Sie können <a href="http://msdn.microsoft.com/library/azure/dn690470.aspx">Redis Cache</a> (Preview) mit dem WordPress-Plug-In <a href="https://wordpress.org/plugins/redis-object-cache/">Redis Object Cache</a> oder eines der anderen Cacheangebote im <a href="/gallery/store/">Azure Store</a> verwenden.</p></li><li><p><a href="http://ruslany.net/2010/03/make-wordpress-faster-on-iis-with-wincache-1-1/">Beschleunigen Sie WordPress mit WinCache</a>. WinCache ist standardmäßig für Web-Apps aktiviert.</p></li><li><p>[Skalieren Sie die Web-App in Azure App Service][websitescale], und nutzen Sie das <a href="http://www.cleardb.com/developers/cdbr/introduction">ClearDB-Hochverfügbarkeitsrouting</a> oder <a href="http://www.mysql.com/products/cluster/">MySQL Cluster CGE</a>.</p></li></ul> |
| **Verwenden von Blobs für die Speicherung** |<ol><li><p>[Erstellen Sie ein Azure-Speicherkonto](../storage/storage-create-storage-account.md).</p></li><li><p>Unter [Verwenden von Azure CDN](../cdn/cdn-create-new-endpoint.md) erfahren Sie, wie Sie in Blobs gespeicherte Daten geografisch verteilen.</p></li><li><p>Installieren und konfigurieren Sie das Plug-In <a href="https://wordpress.org/plugins/windows-azure-storage/">Azure Storage for WordPress</a>.</p><p>Detaillierte Informationen zu Einrichtung und Konfiguration des Plug-Ins finden Sie im <a href="http://plugins.svn.wordpress.org/windows-azure-storage/trunk/UserGuide.docx">Benutzerhandbuch</a>.</p> </li></ol> |
| **Aktivieren von E-Mail** |Aktivieren Sie <a href="https://azure.microsoft.com/en-us/marketplace/partners/sendgrid/sendgrid-azure/">SendGrid</a> über den Azure Store. Installieren des <a href="http://wordpress.org/plugins/sendgrid-email-delivery-simplified">SendGrid-Plug-Ins</a> für WordPress |
| **Konfigurieren eines benutzerdefinierten Domänennamens** |[Konfigurieren Sie einen benutzerdefinierten Domänennamen in Azure App Service][customdomain]. |
| **Aktivieren von HTTPS für einen benutzerdefinierten Domänennamen** |[Aktivieren Sie HTTPS für eine Web-App in Azure App Service][httpscustomdomain]. |
| **Lastenausgleich oder geografische Verteilung für Ihre Website** |[Leiten Sie Datenverkehr mit Azure Traffic Manager weiter][trafficmanager]. Wenn Sie eine benutzerdefinierte Domäne verwenden, finden Sie unter [Konfigurieren eines benutzerdefinierten Domänennamens in Azure App Service][customdomain] Informationen zum Verwenden von Traffic Manager mit benutzerdefinierten Domänennamen. |
| **Aktivieren von automatisierten Sicherungen** |[Sichern Sie die Web-App in Azure App Service][backup]. |
| **Aktivieren der Diagnoseprotokollierung** |[Aktivieren Sie die Diagnoseprotokollierung für Web-Apps in Azure App Service][log]. |

## <a name="next-steps"></a>Nächste Schritte
* [WordPress optimization (WordPress-Optimierung, in englischer Sprache)](http://codex.wordpress.org/WordPress_Optimization)
* [Konvertieren von WordPress zu Multisite in Azure App Service](web-sites-php-convert-wordpress-multisite.md)
* [ClearDB upgrade wizard for Azure (ClearDB-Upgrade-Assistent für Azure, in englischer Sprache)](http://www.cleardb.com/store/azure/upgrade)
* [Hosting WordPress in a subfolder of your web app in Azure App Service (Hosten von WordPress in einem Unterordner Ihrer Web-App in Azure App Service, in englischer Sprache)](http://blogs.msdn.com/b/webapps/archive/2013/02/13/hosting-wordpress-in-a-subfolder-of-your-windows-azure-web-site.aspx)
* [Step-By-Step: Create a WordPress site using Azure](http://blogs.technet.com/b/blainbar/archive/2013/08/07/article-create-a-wordpress-site-using-windows-azure-read-on.aspx) (Schritt für Schritt: Erstellen einer WordPress-Website mit Azure)
* [Host your existing WordPress blog on Azure (Hosten Ihres vorhandenen WordPress-Blogs in Azure, in englischer Sprache)](http://blogs.msdn.com/b/msgulfcommunity/archive/2013/08/26/migrating-a-self-hosted-wordpress-blog-to-windows-azure.aspx)
* [Enabling pretty permalinks in WordPress (Verwenden von benutzerfreundlichen Permalinks in WordPress, in englischer Sprache)](http://www.iis.net/learn/extensions/url-rewrite-module/enabling-pretty-permalinks-in-wordpress)
* [How to migrate and run your WordPress blog on Azure App Service (Gewusst wie: Migrieren und Ausführen Ihres WordPress-Blogs in Azure App Service, in englischer Sprache)](http://www.kefalidis.me/2012/06/how-to-migrate-and-run-your-wordpress-blog-on-windows-azure-websites/)
* [How to run WordPress on Azure App Service for free (Gewusst wie: Kostenloses Ausführen von WordPress in Azure App Service, in englischer Sprache)](http://architects.dzone.com/articles/how-run-wordpress-azure)
* [WordPress on Azure in two minutes or less](http://www.sitepoint.com/wordpress-windows-azure-2-minutes-less/) (WordPress in Azure – in maximal zwei Minuten)
* [Moving a WordPress blog to Azure – Part 1: Creating a WordPress blog on Azure (Verschieben eines WordPress-Blogs nach Azure – Teil 1: Erstellen eines WordPress-Blogs in Azure, in englischer Sprache)](http://www.davebost.com/2013/07/10/moving-a-wordpress-blog-to-windows-azure-part-1)
* [Moving a WordPress blog to Azure – Part 2: Transferring your content (Verschieben eines WordPress-Blogs nach Azure – Teil 2: Übertragen der Inhalte, in englischer Sprache)](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-transferring-your-content)
* [Moving a WordPress blog to Azure – Part 3: Setting up your custom domain (Verschieben eines WordPress-Blogs nach Azure – Teil 3: Einrichten der benutzerdefinierten Domäne, in englischer Sprache)](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-3-setting-up-your-custom-domain)
* [Moving a WordPress blog to Azure – Part 4: Pretty permalinks and URL Rewrite rules (Verschieben eines WordPress-Blogs nach Azure – Teil 4: Benutzerfreundliche Permalinks und URL-Rewrite-Regeln, in englischer Sprache)](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-4-pretty-permalinks-and-url-rewrite-rules)
* [Moving a WordPress blog to Azure – Part 5: Moving from a subfolder to the root (Verschieben eines WordPress-Blogs nach Azure – Teil 5: Verschieben aus einem Unterordner in den Stammordner, in englischer Sprache)](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-5-moving-from-a-subfolder-to-the-root)
* [How to set up a WordPress web app in your Azure account (Gewusst wie: Einrichten einer WordPress-Web-App in Ihrem Azure-Konto, in englischer Sprache)](http://www.itexperience.net/2014/01/20/how-to-set-up-a-wordpress-website-in-your-windows-azure-account/)
* [Propping up WordPress on Azure (Unterstützen von WordPress in Azure, in englischer Sprache)](http://www.johnpapa.net/wordpress-on-azure/)
* [Tips for WordPress on Azure (Tipps für WordPress in Azure, in englischer Sprache)](http://www.johnpapa.net/azurecleardbmysql/)

> [!NOTE]
> Wenn Sie Azure App Service ausprobieren möchten, ehe Sie sich für ein Azure-Konto registrieren, können Sie unter [App Service testen](https://azure.microsoft.com/try/app-service/) sofort kostenlos eine nur kurz verfügbare Starter-Web-App in App Service erstellen. Es ist keine Kreditkarte erforderlich, und Sie gehen keine Verpflichtungen ein.
>
>

## <a name="whats-changed"></a>Änderungen
Hinweise zu den Änderungen in App Service im Vergleich zu Websites finden Sie unter [Azure App Service und vorhandene Azure-Dienste](http://go.microsoft.com/fwlink/?LinkId=529714).

<!-- URL List -->

[performance-diagram]: ./media/web-sites-php-enterprise-wordpress/performance-diagram.png
[basic-diagram]: ./media/web-sites-php-enterprise-wordpress/basic-diagram.png
[multi-region-diagram]: ./media/web-sites-php-enterprise-wordpress/multi-region-diagram.png
[wordpress]: http://www.microsoft.com/web/wordpress
[officeblog]: http://blogs.office.com/
[bingblog]: http://blogs.bing.com/
[cdbnstore]: http://www.cleardb.com/store/azure
[storageplugin]: https://wordpress.org/plugins/windows-azure-storage/
[sendgridplugin]: http://wordpress.org/plugins/sendgrid-email-delivery-simplified/
[phpwebsite]: web-sites-php-configure.md
[customdomain]: web-sites-custom-domain-name.md
[trafficmanager]: ../traffic-manager/traffic-manager-overview.md
[backup]: web-sites-backup.md
[restore]: web-sites-restore.md
[rediscache]: https://azure.microsoft.com/documentation/services/redis-cache/
[managedcache]: http://msdn.microsoft.com/library/azure/dn386122.aspx
[websitescale]: web-sites-scale.md
[managedcachescale]: http://msdn.microsoft.com/library/azure/dn386113.aspx
[cleardbscale]: http://www.cleardb.com/developers/cdbr/introduction
[staging]: web-sites-staged-publishing.md
[monitor]: web-sites-monitor.md
[log]: web-sites-enable-diagnostic-log.md
[httpscustomdomain]: web-sites-configure-ssl-certificate.md
[mysqlwindows]: ../virtual-machines/virtual-machines-windows-classic-mysql-2008r2.md
[mysqllinux]: ../virtual-machines/virtual-machines-linux-classic-mysql-on-opensuse.md
[cge]: http://www.mysql.com/products/cluster/
[websitepricing]: /pricing/details/app-service/
[export]: http://en.support.wordpress.com/export/
[import]: http://wordpress.org/plugins/wordpress-importer/
[wordpressbackup]: http://wordpress.org/plugins/wordpress-importer/
[wordpressdbbackup]: http://codex.wordpress.org/Backing_Up_Your_Database
[createwordpress]: web-sites-php-web-site-gallery.md
[velvet]: https://wordpress.org/plugins/velvet-blues-update-urls/
[mgmtportal]: https://portal.azure.com/
[wordpressbackup]: http://codex.wordpress.org/WordPress_Backups
[wordpressdbbackup]: http://codex.wordpress.org/Backing_Up_Your_Database
[workbench]: http://www.mysql.com/products/workbench/
[searchandreplace]: http://interconnectit.com/124/search-and-replace-for-wordpress-databases/
[deploy]: web-sites-deploy.md
[posh]: /powershell/azureps-cmdlets-docs
[Azure CLI]: ../xplat-cli-install.md
[storesendgrid]: https://azure.microsoft.com/marketplace/partners/sendgrid/sendgrid-azure/
[cdn]: ../cdn/cdn-overview.md



<!--HONumber=Jan17_HO4-->


