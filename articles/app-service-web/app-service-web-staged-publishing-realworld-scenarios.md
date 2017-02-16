---
title: "Effektive Verwendung der DevOps-Umgebungen für Ihre Web-App | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie Bereitstellungsslots zum Einrichten und Verwalten mehrerer Entwicklungsumgebungen für Ihre Anwendung verwenden."
services: app-service\web
documentationcenter: 
author: sunbuild
manager: yochayk
editor: 
ms.assetid: 16a594dc-61f5-4984-b5ca-9d5abc39fb1e
ms.service: app-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 10/24/2016
ms.author: sumuth
translationtype: Human Translation
ms.sourcegitcommit: 385eb87ec32f5f605b28cc8c76b1c89c7e90bfec
ms.openlocfilehash: 5284022ea473db893800b0f64b5bf4f811d994aa


---
# <a name="use-devops-environments-effectively-for-your-web-apps"></a>Effektive Verwendung der DevOps-Umgebungen für Ihre Web-Apps
In diesem Artikel werden die Einrichtung und Verwaltung von Webanwendungsbereitstellungen erläutert, wenn mehrere Versionen Ihrer Anwendung in unterschiedlichen Umgebungen eingesetzt werden (z.B. Entwicklung, Staging, Qualitätssicherung und Produktion). Jede Version Ihrer Anwendung kann als Entwicklungsumgebung für den jeweiligen Zweck Ihres Bereitstellungsprozesses betrachtet werden. Entwickler können beispielsweise die Qualitätssicherungsumgebung zum Testen der Qualität der Anwendung nutzen, ehe die Änderungen in der Produktionsumgebung übernommen werden.
Mehrere Entwicklungsumgebungen können eine Herausforderung darstellen, da Sie Code nachverfolgen, Ressourcen (Compute, Web-App, Datenbank, Cache usw.) verwalten und Code umgebungsübergreifend bereitstellen müssen.

## <a name="set-up-a-non-production-environment-stage-dev-qa"></a>Einrichten einer Nichtproduktionsumgebung (Staging, Entwicklung, Qualitätssicherung)
Nachdem eine Produktions-Web-App erstellt und in Betrieb genommen wurde, besteht der nächste Schritt darin, eine Nichtproduktionsumgebung zu erstellen. Zur Verwendung von Bereitstellungsslots müssen Sie die App im App Service-Planmodus „Standard“ oder „Premium“ ausführen. Bereitstellungsslots sind Live-Web-Apps mit eigenen Hostnamen. Elemente für Web-App-Inhalte und -Konfigurationen können zwischen zwei Bereitstellungsslots, einschließlich des Produktionsslots, ausgetauscht werden. Die Bereitstellung Ihrer Anwendung in einem Bereitstellungsslot hat die folgenden Vorteile:

- Sie können Änderungen an einer Web-App in einem Stagingbereitstellungsslot überprüfen, bevor Sie die Web-App in den Produktionsslot überführen.
- Wenn Sie eine Web-App zuerst in einem Slot bereitstellen und sie dann in den Produktionsslot überführen, werden alle Instanzen erst nach einer Anlaufzeit in den Produktionsslot übernommen. Durch diesen Vorgang vermeiden Sie Ausfallzeit bei der Bereitstellung der Web-App. Die Datenverkehrsweiterleitung ist nahtlos, und es werden keine Anfragen aufgrund von Austauschvorgängen gelöscht. Um diesen gesamten Workflow zu automatisieren, konfigurieren Sie [Automatisch tauschen](web-sites-staged-publishing.md#configure-auto-swap), wenn keine Überprüfung vor dem Austauschen erforderlich ist.
- Nach einem Austausch befindet sich im Slot mit der zuvor bereitgestellten Web-App jetzt die vorherige Produktions-Web-App. Wenn die in den Produktionsslot überführten Änderungen nicht Ihren Erwartungen entsprechen, können Sie denselben Austausch sofort noch einmal vornehmen, um die „letzte als funktionierend bekannte Web-App“ wiederherzustellen.

Informationen zum Einrichten eines Stagingbereitstellungsslots finden Sie unter [Einrichten von Stagingumgebungen für Web-Apps in Azure App Service](web-sites-staged-publishing.md). Jede Umgebung muss einen eigenen Satz von Ressourcen enthalten. Wenn Ihre Web-App beispielsweise eine Datenbank verwendet, sollten die Produktions- und die Staging-Web-App unterschiedliche Datenbanken verwenden. Fügen Sie zum Einrichten Ihrer Staging- und Entwicklungsumgebung Staging- und Entwicklungsumgebungsressourcen wie z.B. Datenbank, Speicher oder Cache hinzu.

## <a name="examples-of-using-multiple-development-environments"></a>Beispiele für die Verwendung mehrerer Entwicklungsumgebungen
Jedes Projekt sollte der Quellcodeverwaltung mit mindestens zwei Umgebungen folgen: Entwicklung und Produktion. Bei Verwendung von Content Management-Systemen (CMS), Anwendungsframeworks usw. wird dieses Szenario möglicherweise nicht ohne Anpassung von der Anwendung unterstützt. Dies gilt für einige beliebte Frameworks, die in den folgenden Abschnitten behandelt werden. Wenn Sie mit CMS oder Frameworks arbeiten, sind viele Fragen zu klären:

- Wie verteilen Sie den Inhalt auf verschiedene Umgebungen?
- Welche Dateien können Sie ändern, ohne Versionsupdates für Frameworks zu beeinträchtigen?
- Wie verwalten Sie die Konfigurationen nach Umgebung?
- Wie verwalten Sie Versionsupdates für Module, Plug-Ins und das Kernframework?

Es gibt viele Möglichkeiten, mehrere Umgebungen für Ihr Projekt festzulegen. Die folgenden Beispiele zeigen eine Methode für jede Anwendung.

### <a name="wordpress"></a>WordPress
In diesem Abschnitt erfahren Sie, wie Sie einen Bereitstellungsworkflow mit Slots für WordPress einrichten. Wie die meisten CMS-Lösungen unterstützt WordPress die Verwendung mehrerer Entwicklungsumgebungen nicht ohne Anpassung. Das Web-Apps-Feature von Azure App Service verfügt über einige Features, die das Speichern von Konfigurationseinstellungen außerhalb des Codes vereinfachen.

1. Bevor Sie einen Stagingslot erstellen, richten Sie den Anwendungscode zur Unterstützung mehrerer Umgebungen ein. Um mehrere Umgebungen in WordPress zu unterstützen, müssen Sie `wp-config.php` in Ihrer lokalen Entwicklungs-Web-App bearbeiten und den folgenden Code am Anfang der Datei hinzufügen. Durch diesen Vorgang kann die Anwendung basierend auf der ausgewählten Umgebung die richtige Konfiguration auswählen.

    ```
    // Support multiple environments
    // set the config file based on current environment
    if (strpos($_SERVER['HTTP_HOST'],'localhost') !== false) {
    // local development
     $config_file = 'config/wp-config.local.php';
    }
    elseif ((strpos(getenv('WP_ENV'),'stage') !== false) || (strpos(getenv('WP_ENV'),'prod' )!== false ))
    //single file for all azure development environments
     $config_file = 'config/wp-config.azure.php';
    }
    $path = dirname(__FILE__). '/';
    if (file_exists($path. $config_file)) {
    // include the config file if it exists, otherwise WP is going to fail
    require_once $path. $config_file;
    ```

2. Erstellen Sie im Web-App-Stammverzeichnis einen Ordner namens `config`, und fügen Sie die Dateien `wp-config.azure.php` und `wp-config.local.php` hinzu, die die Azure-Umgebung bzw. Ihre lokale Umgebung darstellen.

3. Kopieren Sie den folgenden Code in `wp-config.local.php`:

    ```
    <?php
    // MySQL settings
    /** The name of the database for WordPress */

    define('DB_NAME', 'yourdatabasename');

    /** MySQL database username */
    define('DB_USER', 'yourdbuser');

    /** MySQL database password */
    define('DB_PASSWORD', 'yourpassword');

    /** MySQL hostname */
    define('DB_HOST', 'localhost');
    /**
     * For developers: WordPress debugging mode.
     * * Change this to true to enable the display of notices during development.
     * It is strongly recommended that plugin and theme developers use WP_DEBUG
     * in their development environments.
     */
    define('WP_DEBUG', true);

    //Security key settings
    define('AUTH_KEY', 'put your unique phrase here');
    define('SECURE_AUTH_KEY','put your unique phrase here');
    define('LOGGED_IN_KEY','put your unique phrase here');
    define('NONCE_KEY', 'put your unique phrase here');
    define('AUTH_SALT', 'put your unique phrase here');
    define('SECURE_AUTH_SALT', 'put your unique phrase here');
    define('LOGGED_IN_SALT', 'put your unique phrase here');
    define('NONCE_SALT', 'put your unique phrase here');

    /**
     * WordPress Database Table prefix.
     *
     * You can have multiple installations in one database if you give each a unique
     * prefix. Only numbers, letters, and underscores please!
     */
    $table_prefix = 'wp_';
    ```

    Indem Sie die Sicherheitsschlüssel wie im vorherigen Code gezeigt festlegen, können Sie verhindern, dass Ihre Web-App gehackt wird. Verwenden Sie daher eindeutige Werte. Wenn Sie die Zeichenfolge für die im Code genannten Sicherheitsschlüssel generieren müssen, können Sie [den automatischen Generator](https://api.wordpress.org/secret-key/1.1/salt) zum Erstellen neuer Schlüssel-Wert-Paare verwenden.

4. Kopieren Sie den folgenden Code in `wp-config.azure.php`:

    ```    
    <?php
    // MySQL settings
    /** The name of the database for WordPress */

    define('DB_NAME', getenv('DB_NAME'));

    /** MySQL database username */
    define('DB_USER', getenv('DB_USER'));

    /** MySQL database password */
    define('DB_PASSWORD', getenv('DB_PASSWORD'));

    /** MySQL hostname */
    define('DB_HOST', getenv('DB_HOST'));

    /**
    * For developers: WordPress debugging mode.
    *
    * Change this to true to enable the display of notices during development.
    * It is strongly recommended that plugin and theme developers use WP_DEBUG
    * in their development environments.
    * Turn on debug logging to investigate issues without displaying to end user. For WP_DEBUG_LOG to
    * do anything, WP_DEBUG must be enabled (true). WP_DEBUG_DISPLAY should be used in conjunction
    * with WP_DEBUG_LOG so that errors are not displayed on the page */

    */
    define('WP_DEBUG', getenv('WP_DEBUG'));
    define('WP_DEBUG_LOG', getenv('TURN_ON_DEBUG_LOG'));
    define('WP_DEBUG_DISPLAY',false);

    //Security key settings
    /** If you need to generate the string for security keys mentioned above, you can go the automatic generator to create new keys/values: https://api.wordpress.org/secret-key/1.1/salt **/
    define('AUTH_KEY',getenv('DB_AUTH_KEY'));
    define('SECURE_AUTH_KEY', getenv('DB_SECURE_AUTH_KEY'));
    define('LOGGED_IN_KEY', getenv('DB_LOGGED_IN_KEY'));
    define('NONCE_KEY', getenv('DB_NONCE_KEY'));
    define('AUTH_SALT', getenv('DB_AUTH_SALT'));
    define('SECURE_AUTH_SALT', getenv('DB_SECURE_AUTH_SALT'));
    define('LOGGED_IN_SALT',  getenv('DB_LOGGED_IN_SALT'));
    define('NONCE_SALT',  getenv('DB_NONCE_SALT'));

    /**
    * WordPress Database Table prefix.
    *
    * You can have multiple installations in one database if you give each a unique
    * prefix. Only numbers, letters, and underscores please!
    */
    $table_prefix = getenv('DB_PREFIX');
```

#### <a name="use-relative-paths"></a>Verwenden relativer Pfade
Zuletzt müssen Sie in WordPress noch relative Pfade konfigurieren. WordPress speichert URL-Informationen in der Datenbank. Durch diesen Speicher lassen sich Inhalte nur schwierig aus einer Umgebung in eine andere verschieben. Sie müssen die Datenbank jedes Mal aktualisieren, wenn Sie Inhalte aus der lokalen Umgebung in die Stagingumgebung oder umgekehrt verschieben. Zur Vermeidung von Problemen, die durch das Bereitstellen der Datenbank bei jeder umgebungsübergreifenden Bereitstellung entstehen können, verwenden Sie das [Plug-In für relative Stammlinks](https://wordpress.org/plugins/root-relative-urls/), das über das WordPress-Administratordashboard installiert werden kann.

Fügen Sie Ihrer Datei `wp-config.php` die folgenden Einträge vor dem Kommentar `That's all, stop editing!` hinzu:

```

  define('WP_HOME', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_SITEURL', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_CONTENT_URL', '/wp-content');
    define('DOMAIN_CURRENT_SITE', filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
```

Aktivieren Sie das Plug-In über das Menü `Plugins` im WordPress-Administratordashboard. Speichern Sie die Permalink-Einstellungen für die WordPress-App.

#### <a name="the-final-wp-configphp-file"></a>Die fertige Datei `wp-config.php`
Aktualisierungen des WordPress-Kerns haben keine Auswirkung auf Ihre Dateien `wp-config.php`, `wp-config.azure.php` und `wp-config.local.php`. Hier ist eine endgültige Version der Datei `wp-config.php`:

```
<?php
/**
 * The base configurations of the WordPress.
 *
 * This file has the following configurations: MySQL settings, Table Prefix,
 * Secret Keys, and ABSPATH. You can find more information by visiting
 *
 * Codex page. You can get the MySQL settings from your web host.
 *
 * This file is used by the wp-config.php creation script during the
 * installation. You don't have to use the web web app, you can just copy this file
 * to "wp-config.php" and fill in the values.
 *
 * @package WordPress
 */

// Support multiple environments
// set the config file based on current environment
if (strpos($_SERVER['HTTP_HOST'],'localhost') !== false) { // local development
  $config_file = 'config/wp-config.local.php';
}
elseif ((strpos(getenv('WP_ENV'),'stage') !== false) ||(strpos(getenv('WP_ENV'),'prod' )!== false )){
  $config_file = 'config/wp-config.azure.php';
}


$path = dirname(__FILE__). '/';
if (file_exists($path. $config_file)) {
  // include the config file if it exists, otherwise WP is going to fail
  require_once $path. $config_file;
}

/** Database Charset to use in creating database tables. */
define('DB_CHARSET', 'utf8');

/** The Database Collate type. Don't change this if in doubt. */
define('DB_COLLATE', '');


/* That's all, stop editing! Happy blogging. */

define('WP_HOME', 'http://'. $_SERVER['HTTP_HOST']);
define('WP_SITEURL', 'http://'. $_SERVER['HTTP_HOST']);
define('WP_CONTENT_URL', '/wp-content');
define('DOMAIN_CURRENT_SITE', $_SERVER['HTTP_HOST']);

/** Absolute path to the WordPress directory. */
if ( !defined('ABSPATH') )
    define('ABSPATH', dirname(__FILE__). '/');

/** Sets up WordPress vars and included files. */
require_once(ABSPATH. 'wp-settings.php');
```

#### <a name="set-up-a-staging-environment"></a>Einrichten einer Stagingumgebung
1. Wenn Sie bereits über eine in Ihrem Azure-Abonnement ausgeführte WordPress-Web-App verfügen, melden Sie sich beim [Azure-Portal](http://portal.azure.com) an, und wechseln Sie dann zu Ihrer WordPress-Web-App. Wenn Sie keine WordPress-Web-App besitzen, können Sie im Azure Marketplace eine App erstellen. Weitere Informationen finden Sie unter [Erstellen einer WordPress-Web-App in Azure App Service](web-sites-php-web-site-gallery.md).
Klicken Sie auf **Einstellungen** > **Bereitstellungsslots** > **Hinzufügen**, um einen Bereitstellungsslot mit dem Namen *stage* zu erstellen. Ein Bereitstellungsslot ist eine weitere Webanwendung, die die gleichen Ressourcen wie die zuvor erstellte primäre Web-App nutzt.

    ![Erstellen des Bereitstellungslots „stage“](./media/app-service-web-staged-publishing-realworld-scenarios/1setupstage.png)

2. Fügen Sie der Ressourcengruppe `wordpressapp-group` eine weitere MySQL-Datenbank hinzu, z.B. `wordpress-stage-db`.

    ![Hinzufügen einer MySQL-Datenbank zur Ressourcengruppe](./media/app-service-web-staged-publishing-realworld-scenarios/2addmysql.png)

3. Aktualisieren Sie die Verbindungszeichenfolgen für Ihren Bereitstellungsslot „stage“, um auf die neue Datenbank `wordpress-stage-db` zu verweisen. Ihre Produktions-Web-App `wordpressprodapp` und die Staging-Web-App `wordpressprodapp-stage` müssen auf unterschiedliche Datenbanken verweisen.

#### <a name="configure-environment-specific-app-settings"></a>Konfigurieren umgebungsspezifischer App-Einstellungen 
Entwickler können Schlüssel-Wert-Zeichenfolgenpaare in Azure als Teil der zu einer Web-App gehörenden Konfigurationsinformationen speichern, die als **App-Einstellungen** bezeichnet werden. Web-Apps rufen diese Werte zur Laufzeit automatisch ab und machen sie für den in Ihrer Web-App ausgeführten Code verfügbar. Im Hinblick auf die Sicherheit ist dies ein angenehmer Nebeneffekt, da vertrauliche Informationen wie Datenbank-Verbindungszeichenfolgen mit Kennwörtern nie als Klartext in einer Datei wie `wp-config.php` angezeigt werden.

Dieser Vorgang, der in den folgenden Abschnitten beschrieben wird, ist nützlich, da er sowohl Dateiänderungen als auch Datenbankänderungen für die WordPress-App berücksichtigt:

* WordPress-Versionsupgrade
* Hinzufügen neuer Plug-Ins oder Bearbeiten bzw. Aktualisieren von Plug-Ins
* Hinzufügen neuer Designs oder Bearbeiten bzw. Aktualisieren von Designs

Konfigurieren Sie App-Einstellungen für Folgendes:

* Datenbankinformationen
* Aktivieren/Deaktivieren der WordPress-Protokollierung
* WordPress-Sicherheitseinstellungen

![App-Einstellungen für die WordPress-Web-App](./media/app-service-web-staged-publishing-realworld-scenarios/3configure.png)

Stellen Sie sicher, dass Sie die folgenden App-Einstellungen für die Produktions-Web-App und den Slot „stage“ hinzufügen. Beachten Sie, dass die Produktions-Web-App und die Staging-Web-App unterschiedliche Datenbanken verwenden.

1. Deaktivieren Sie das Kontrollkästchen **Sloteinstellung** für alle Einstellungsparameter außer WP_ENV. Durch diesen Vorgang wird die Konfiguration für Ihre Web-App, Dateiinhalte und die Datenbank ausgetauscht. Wenn die **Sloteinstellung** aktiviert ist, werden die App-Einstellungen und die Verbindungszeichenfolgen-Konfiguration der Web-App bei einem **Austauschvorgang** *nicht* umgebungsübergreifend verschoben. Vorhandene Datenbankänderungen verursachen keine Unterbrechung Ihrer Produktions-Web-App.

2. Stellen Sie die Web-App in der lokalen Entwicklungsumgebung mithilfe von WebMatrix oder Tools Ihrer Wahl (z.B. FTP, Git oder PhpMyAdmin) für die Staging-Web-App und -Datenbank bereit.

    ![WebMatrix-Veröffentlichungsdialogfeld für die WordPress-Web-App](./media/app-service-web-staged-publishing-realworld-scenarios/4wmpublish.png)

3. Durchsuchen und testen Sie Ihre Staging-Web-App. Das folgende Beispiel zeigt die Staging-Web-App für ein Szenario, in dem das Design der Web-App aktualisiert werden soll.

    ![Durchsuchen der Staging-Web-App vor dem Austauschen von Slots](./media/app-service-web-staged-publishing-realworld-scenarios/5wpstage.png)

4. Wenn alles Ihren Vorstellungen entspricht, klicken Sie in Ihrer Staging-Web-App auf die Schaltfläche **Austauschen**, um den Inhalt in die Produktionsumgebung zu verschieben. In diesem Fall tauschen Sie die Web-App und die Datenbank bei jedem **Austauschvorgang** zwischen den Umgebungen aus.

    ![Austauschen von Vorschauänderungen für WordPress](./media/app-service-web-staged-publishing-realworld-scenarios/6swaps1.png)

    > [!NOTE]
    > Wenn in Ihrem Szenario nur Dateien (keine Datenbankupdates) mithilfe von Push übertragen werden müssen, aktivieren Sie vor dem **Austausch** im Azure-Portal auf dem Blatt mit den **Web-App-Einstellungen** das Kontrollkästchen **Sloteinstellung** für alle datenbankbezogenen *App-Einstellungen* und *Verbindungszeichenfolgen-Einstellungen*. In diesem Fall sollten DB_NAME, DB_HOST, DB_PASSWORD, DB_USER und die standardmäßigen Verbindungszeichenfolgen-Einstellungen bei einem **Austausch** nicht in der Vorschau der Änderungen angezeigt werden. Nach Abschluss des **Austauschvorgangs** enthält die WordPress-Web-App nur die aktualisierten Dateien.
    >
    >

    Hier sehen Sie die WordPress-Produktions-Web-App vor dem **Austauschvorgang**.
    ![Produktions-Web-App vor dem Austauschen von Slots](./media/app-service-web-staged-publishing-realworld-scenarios/7bfswap.png)

    Nach dem **Austauschvorgang** ist das Design Ihrer Produktions-Web-App aktualisiert.

    ![Produktions-Web-App nach dem Austauschen von Slots](./media/app-service-web-staged-publishing-realworld-scenarios/8afswap.png)

5. Falls Sie einen Rollback ausführen müssen, können Sie zu den Produktions-Web-**App-Einstellungen** wechseln und auf die Schaltfläche **Austauschen** klicken, um für die Web-App und die Datenbank den Produktions- gegen den Stagingslot auszutauschen. Wenn der Vorgang **Austauschen** Datenbankänderungen umfasst, beachten Sie, dass Sie bei der nächsten Bereitstellung in der Staging-Web-App die Datenbankänderungen in der aktuellen Datenbank für Ihre Staging-Web-App bereitstellen müssen. Die aktuelle Datenbank kann die vorherige Produktionsdatenbank oder die Stagingdatenbank sein.

#### <a name="summary"></a>Zusammenfassung
Es folgt eine verallgemeinerte Vorgehensweise für jede Anwendung, die eine Datenbank verwendet:

1. Installieren Sie die Anwendung in Ihrer lokalen Umgebung.
2. Fügen Sie die umgebungsspezifische Konfiguration hinzu (lokale und Azure-Web-Apps).
3. Richten Sie Ihre Staging- und Ihre Produktionsumgebung für Web-Apps ein.
4. Wenn Sie bereits eine auf Azure ausgeführte Produktionsanwendung haben, synchronisieren Sie Ihre Produktionsinhalte (Dateien, Code und Datenbank) mit der lokalen Umgebung und der Stagingumgebung.
5. Entwickeln Sie Ihre Anwendung in Ihrer lokalen Umgebung.
6. Versetzen Sie die Produktions-Web-App in den Wartungs- oder gesperrten Modus, und synchronisieren Sie Datenbankinhalte aus der Produktionsumgebung mit der Staging- und der Entwicklungsumgebung.
7. Stellen Sie die App in der Stagingumgebung bereit, und testen Sie sie.
8. Stellen Sie die App in der Produktionsumgebung bereit.
9. Wiederholen Sie die Schritte 4 bis 6.

### <a name="umbraco"></a>Umbraco
In diesem Abschnitt erfahren Sie, wie Umbraco CMS ein benutzerdefiniertes Modul für die übergreifende Bereitstellung in einer Umgebung mit mehreren DevOps verwendet. In diesem Beispiel wird ein anderer Ansatz zum Verwalten mehrerer Entwicklungsumgebungen vorgestellt.

[Umbraco CMS](http://umbraco.com/) ist eine beliebte .NET-CMS-Lösung, die von vielen Entwicklern verwendet wird. Sie bietet das Modul [Courier2](http://umbraco.com/products/more-add-ons/courier-2), um die Bereitstellung von der Entwicklungs- in der Staging- und schließlich in der Produktionsumgebungen durchzuführen. Sie können mit Visual Studio oder WebMatrix problemlos eine lokale Entwicklungsumgebung für eine Umbraco CMS-Web-App erstellen.

- [Erstellen einer Umbraco-Web-App mit Visual Studio](https://our.umbraco.org/documentation/Installation/install-umbraco-with-nuget)
- [Erstellen einer Umbraco-Web-App mit WebMatrix](http://umbraco.tv/videos/umbraco-v7/implementor/fundamentals/installation/creating-umbraco-site-from-webmatrix-web-gallery/)

Denken Sie immer daran, den Ordner `install` unter Ihrer Anwendung zu entfernen. Laden Sie ihn niemals in Staging- oder Produktions-Web-Apps hoch. In diesem Lernprogramm wird WebMatrix verwendet.

#### <a name="set-up-a-staging-environment"></a>Einrichten einer Stagingumgebung
1. Sofern Sie bereits eine Umbraco CMS-Web-App erstellt haben und ausführen, erstellen Sie wie bereits erläutert einen Bereitstellungsslot für die Umbraco CMS-Web-App. Andernfalls können Sie eine App über den Marketplace erstellen.
2. Aktualisieren Sie die Verbindungszeichenfolge für Ihren Bereitstellungsslot „stage“, um auf die neue Datenbank **umbraco-stage-db** zu verweisen. Ihre Produktions-Web-App (umbraositecms-1) und Ihre Staging-Web-App (umbracositecms-1-stage) *müssen* auf unterschiedliche Datenbanken verweisen.

    ![Aktualisieren der Verbindungszeichenfolge für die Staging-Web-App mit der neuen Stagingdatenbank](./media/app-service-web-staged-publishing-realworld-scenarios/9umbconnstr.png)

3. Klicken Sie für den Bereitstellungsslot **stage** auf **Veröffentlichungseinstellungen abrufen**. Durch diesen Vorgang wird eine Datei mit Veröffentlichungseinstellungen heruntergeladen, die alle Informationen speichert, die Visual Studio oder WebMatrix benötigt, um Ihre lokale Entwicklungs-Web-App in der Azure-Web-App zu veröffentlichen.

    ![Abrufen der Veröffentlichungseinstellungen der Staging-Web-App](./media/app-service-web-staged-publishing-realworld-scenarios/10getpsetting.png)
4. Öffnen Sie die lokale Entwicklungs-Web-App in WebMatrix oder Visual Studio. In diesem Lernprogramm wird WebMatrix verwendet. Als Erstes müssen Sie die Datei mit den Veröffentlichungseinstellungen für Ihre Staging-Web-App importieren.

    ![Importieren der Veröffentlichungseinstellungen für Umbraco mit WebMatrix](./media/app-service-web-staged-publishing-realworld-scenarios/11import.png)

5. Überprüfen Sie die Änderungen im Dialogfeld, und stellen Sie Ihre lokale Web-App in Ihrer Azure-Web-App *umbracositecms-1-stage* bereit. Wenn Sie Dateien direkt in der Staging-Web-App bereitstellen, werden alle Dateien im Ordner `~/app_data/TEMP/` ausgelassen, da diese beim erstmaligen Start der Staging-Web-App neu generiert werden. Sie sollten auch die Datei `~/app_data/umbraco.config` auslassen, die ebenfalls neu generiert wird.

    ![Überprüfen der Veröffentlichungseinstellungen in WebMatrix](./media/app-service-web-staged-publishing-realworld-scenarios/12umbpublish.png)

6. Nachdem Sie die lokale Umbraco-Web-App erfolgreich in der Staging-Web-App veröffentlicht haben, wechseln Sie zu Ihrer Staging-Web-App, und führen Sie einige Tests aus, um Probleme auszuschließen.

#### <a name="set-up-the-courier2-deployment-module"></a>Einrichten des Bereitstellungsmoduls „Courier2“
Beim Modul [Courier2](http://umbraco.com/products/more-add-ons/courier-2) können Sie einfach mit der rechten Maustaste klicken, um Inhalte, Stylesheets und Entwicklungsmodule aus einer Staging-Web-App in eine Produktions-Web-App zu verschieben. Durch diesen Vorgang wird das Risiko verringert, dass Ihre Produktions-Web-App beim Bereitstellen eines Updates beschädigt wird.
Erwerben Sie eine Lizenz für „Courier2“ für die Domäne `*.azurewebsites.net` und Ihre benutzerdefinierte Domäne (z.B. http://abc.com). Nachdem Sie die Lizenz erworben haben, legen Sie die heruntergeladene Lizenz (LIC-Datei) im Ordner `bin` ab.

![Speichern der Lizenzdatei im Ordner „bin“](./media/app-service-web-staged-publishing-realworld-scenarios/13droplic.png)

1. [Laden Sie das Courier2-Paket herunter](https://our.umbraco.org/projects/umbraco-pro/umbraco-courier-2/). Melden Sie sich bei Ihrer Staging-Web-App an, z.B. http://umbracocms-site-stage.azurewebsites.net/umbraco, und klicken Sie im Menü **Developer** auf **Packages** > **Install local package**.

    ![Umbraco-Paketinstaller](./media/app-service-web-staged-publishing-realworld-scenarios/14umbpkg.png)

2. Laden Sie das Courier2-Paket mithilfe des Installers hoch.

    ![Hochladen des Pakets für das Courier-Modul](./media/app-service-web-staged-publishing-realworld-scenarios/15umbloadpkg.png)

3. Zum Konfigurieren des Pakets müssen Sie die Datei „courier.config“ im Ordner **Config** Ihrer Web-App aktualisieren.

    ```xml
    <!-- Repository connection settings -->
     <!-- For each site, a custom repository must be configured, so Courier knows how to connect and authenticate-->
     <repositories>
        <!-- If a custom Umbraco Membership provider is used, specify login & password + set the passwordEncoding to clear: -->
        <repository name="production web app" alias="stage" type="CourierWebserviceRepositoryProvider" visible="true">
          <url>http://umbracositecms-1.azurewebsites.net</url>
          <user>0</user>
          <!--<login>user@email.com</login> -->
          <!-- <password>user_password</password>-->
          <!-- <passwordEncoding>Clear</passwordEncoding>-->
          </repository>
     </repositories>
     ```

4. Geben Sie unter `<repositories>`die URL der Produktionswebsite und die Benutzerinformationen ein.
    Geben Sie bei Verwendung des Standardmitgliedschaftsanbieters für Umbraco die ID des Administratorbenutzers im Abschnitt &lt;user&gt; ein.
    Wenn Sie einen benutzerdefinierten Mitgliedschaftsanbieter für Umbraco verwenden, verwenden Sie `<login>`,`<password>` im Courier2-Modul, um eine Verbindung mit der Produktionswebsite herzustellen.
    Weitere Informationen finden Sie in der [Dokumentation für das Courier2-Modul](http://umbraco.com/help-and-support/customer-area/courier-2-support-and-download/developer-documentation).

5. Installieren Sie auf ähnliche Weise das Courier2-Modul auf der Produktionswebsite, und konfigurieren Sie es in der betreffenden Datei „courier.config“ so, dass es auf die Staging-Web-App zeigt.

    ```xml
     <!-- Repository connection settings -->
     <!-- For each site, a custom repository must be configured, so Courier knows how to connect and authenticate-->
     <repositories>
        <!-- If a custom Umbraco Membership provider is used, specify login & password + set the passwordEncoding to clear: -->
        <repository name="Stage web app" alias="stage" type="CourierWebserviceRepositoryProvider" visible="true">
          <url>http://umbracositecms-1-stage.azurewebsites.net</url>
          <user>0</user>
          </repository>
     </repositories>
    ```

6. Klicken Sie im Dashboard der Umbraco CMS-Web-App auf die Registerkarte **Courier2**, und klicken Sie dann auf **Locations**. Der Name des Repositorys sollte wie in `courier.config`erwähnt angezeigt werden. Führen Sie diesen Vorgang für die Produktions- und die Staging-Web-App aus.

    ![Anzeigen des Zielrepositorys der Web-App](./media/app-service-web-staged-publishing-realworld-scenarios/16courierloc.png)

7. Um Inhalte von der Stagingsite in der Produktionssite bereitzustellen, wechseln Sie zu **Content**, und wählen Sie eine vorhandene Seite aus, oder erstellen Sie eine neue Seite. Ich wähle eine vorhandene Seite aus meiner Web-App aus, deren Titel **Getting Started – new** lautet, und klicke dann auf **Save and Publish**.

    ![Ändern des Titels der Seite und Veröffentlichen](./media/app-service-web-staged-publishing-realworld-scenarios/17changepg.png)

8. Klicken Sie mit der rechten Maustaste auf die geänderte Seite, um alle Optionen anzuzeigen. Klicken Sie auf **Courier**, um das Dialogfeld **Bereitstellung** zu öffnen. Klicken Sie auf **Bereitstellen**, um die Bereitstellung zu initiieren.

    ![Bereitstellungsdialogfeld für das Courier-Modul](./media/app-service-web-staged-publishing-realworld-scenarios/18dialog1.png)

9. Überprüfen Sie die Änderungen, und klicken Sie dann auf **Weiter**.

    ![Überprüfen der Änderungen im Bereitstellungsdialogfeld für das Courier-Modul](./media/app-service-web-staged-publishing-realworld-scenarios/19dialog2.png)

    Im Bereitstellungsprotokoll wird angezeigt, ob die Bereitstellung erfolgreich war.

     ![Anzeigen der Bereitstellungsprotokolle des Courier-Moduls](./media/app-service-web-staged-publishing-realworld-scenarios/20successdlg.png)

10. Durchsuchen Sie Ihre Produktions-Web-App, um zu überprüfen, ob die Änderungen angewendet wurden.

     ![Durchsuchen der Produktions-Web-App](./media/app-service-web-staged-publishing-realworld-scenarios/21umbpg.png)

Weitere Informationen zur Verwendung von Courier finden Sie in der Dokumentation.

#### <a name="how-to-upgrade-the-umbraco-cms-version"></a>Aktualisieren der Umbraco CMS-Version
Courier bietet keine Hilfe beim Aktualisieren der Umbraco CMS-Version. Wenn Sie die Umbraco CMS-Version aktualisieren, müssen Sie überprüfen, ob Inkompatibilitäten mit Ihren benutzerdefinierten Modulen oder mit Modulen von Partnern und den Umbraco-Kernbibliotheken vorliegen. Bewährte Methoden:

* Sichern Sie immer Ihre Web-App und Ihre Datenbank, bevor Sie ein Upgrade durchführen. Für Azure-Web-Apps können Sie mithilfe der Sicherungsfunktion automatische Sicherungen für Ihre Websites einrichten und Ihre Website bei Bedarf mit der Wiederherstellungsfunktion wiederherstellen. Weitere Informationen finden Sie unter [Sichern Ihrer Web-App](web-sites-backup.md) und [Wiederherstellen Ihrer Web-App](web-sites-restore.md).
* Überprüfen Sie, ob Pakete von Partnern mit der Version kompatibel sind, auf die Sie aktualisieren. Überprüfen Sie auf der Downloadseite des Pakets die Projektkompatibilität mit der Umbraco CMS-Version.

Weitere Informationen zum lokalen Upgrade Ihrer Web-App [finden Sie in der allgemeinen Upgradeanleitung](https://our.umbraco.org/documentation/getting-started/setup/upgrading/general).

Nach dem Aktualisieren Ihrer lokalen Entwicklungswebsite veröffentlichen Sie die Änderungen in der Staging-Web-App. Testen Sie Ihre Anwendung. Wenn alles Ihren Vorstellungen entspricht, klicken Sie auf die Schaltfläche **Swap**, um Ihre Stagingsite und die Produktions-Web-App auszutauschen. Wenn Sie den **Austauschvorgangs** verwenden, können Sie die Änderungen anzeigen, die in Ihrer Web-App-Konfiguration betroffen sind. Bei diesem **Austauschvorgang** werden die Web-Apps und Datenbanken ausgetauscht. Daher verweist die Produktions-Web-App nach dem **Austausch** auf die Datenbank „umbraco-stage-db“, und die Staging-Web-App verweist auf die Datenbank „umbraco-prod-db“.

![Austauschvorschau für die Bereitstellung von Umbraco CMS](./media/app-service-web-staged-publishing-realworld-scenarios/22umbswap.png)

Das Austauschen der Web-App und der Datenbank bietet folgende Vorteile:

* Sie können mit einem weiteren **Austauschvorgang** einen Rollback auf die vorherige Version der Web-App ausführen, wenn in der Anwendung Probleme auftreten.
* Bei einem Upgrade müssen Sie Dateien und die Datenbank von der Staging-Web-App in der Produktions-Web-App und der Datenbank bereitstellen. Beim Bereitstellen von Dateien und Datenbanken kann viel passieren. Durch das Verwenden des Features zum **Austauschen** von Slots wird das Risiko von Ausfallzeiten während eines Upgrades und von Fehlern bei der Bereitstellung von Änderungen reduziert.
* Mithilfe des Features [Test-in-Produktion](https://azure.microsoft.com/documentation/videos/introduction-to-azure-websites-testing-in-production-with-galin-iliev/) können Sie **A/B-Tests** durchführen.

Dieses Beispiel veranschaulicht die Flexibilität der Plattform. Sie ermöglicht es Ihnen, benutzerdefinierte Module wie das Umbraco Courier-Modul zur umgebungsübergreifenden Verwaltung der Bereitstellung zu erstellen.

## <a name="references"></a>Referenzen
[Agile Softwareentwicklung mit Azure App Service](app-service-agile-software-development.md)

[Einrichten von Stagingumgebungen für Web-Apps in Azure App Service](web-sites-staged-publishing.md)

[Blockieren des Webzugriffs auf Nicht-Produktionsslots](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)



<!--HONumber=Dec16_HO3-->


