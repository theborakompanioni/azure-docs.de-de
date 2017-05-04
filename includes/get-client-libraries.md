### <a name="install-via-composer"></a>Installation mithilfe von Composer
1. [Installieren Sie Git][install-git]. Unter Windows muss die ausführbare Git-Datei zu Ihrer PATH-Umgebungsvariablen hinzugefügt werden. 
2. Erstellen Sie im Stammverzeichnis Ihres Projekts eine Datei namens **composer.json** , und fügen Sie zu dieser den folgenden Code hinzu:
   
    ```
    {
      "require": {
        "microsoft/windowsazure": "^0.4"
      }
    }
    ```
3. Laden Sie **[composer.phar][composer-phar]** in Ihr Projektverzeichnis herunter.
4. Öffnen Sie eine Eingabeaufforderung und führen Sie in Ihrem Projektverzeichnis folgenden Befehl aus
   
    ```
    php composer.phar install
    ```

[php-sdk-github]: http://go.microsoft.com/fwlink/?LinkId=252719
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[download-SDK-PHP]: ../articles/php-download-sdk.md
[composer-phar]: http://getcomposer.org/composer.phar
