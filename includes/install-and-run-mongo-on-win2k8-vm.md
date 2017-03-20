Diese schrittweise Anleitung erläutert die Installation und Ausführung von MongoDB auf einem virtuellen Computer, der Windows Server ausführt.

> [!IMPORTANT]
> MongoDB-Sicherheitsfunktionen wie Authentifizierung und IP-Adressen-Anbindung werden nicht standardmäßig aktiviert. Die Sicherheitsfunktionen sollten aktiviert werden, bevor MongoDB in einer Produktionsumgebung eingesetzt wird.  Weitere Informationen finden Sie unter [Sicherheit und Authentifizierung](http://www.mongodb.org/display/DOCS/Security+and+Authentication).
>
>

1. Wenn Sie über Remote Desktop eine Verbindung zum virtuellen Computer aufgebaut haben, öffnen Sie im Menü **Start** des virtuellen Computers den Internet Explorer.
2. Klicken Sie in der oberen rechten Ecke auf **Extras** .  Öffnen Sie unter **Internetoptionen** die Registerkarte **Sicherheit**, wählen Sie anschließend das Symbol **Vertrauenswürdige Sites**, und klicken Sie dann auf die Schaltfläche **Sites**. Fügen Sie *https://\*.mongodb.org* der Liste mit den vertrauenswürdigen Sites hinzu.
3. Wechseln Sie zu [Downloads – MongoDB](https://www.mongodb.com/download-center#community).
4. Suchen Sie für **Community Server** nach **Current Stable Release**, und wählen Sie in der Windows-Spalte die aktuelle **64-Bit**-Version aus. Laden Sie das MSI-Installationsprogramm herunter, und führen Sie es aus.
5. MongoDB wird normalerweise unter „C:\Programme\MongoDB“ installiert. Suchen Sie auf dem Desktop nach "Umgebungsvariablen", und fügen Sie der PATH-Variablen den Pfad der MongoDB-Binärdateien hinzu. Die Binärdateien befinden sich beispielsweise auf Ihrem Computer unter „C:\Programme\MongoDB\Server\3.4\bin“.
6. Erstellen Sie die Daten- und Protokollverzeichnisse für MongoDB auf dem Datenträger (z.B. Laufwerk **F:**), den Sie mit den vorherigen Schritten erstellt haben. Wählen Sie unter **Start** die Option **Eingabeaufforderung**, um ein Eingabeaufforderungsfenster zu öffnen.  Geben Sie Folgendes ein:

        C:\> F:
        F:\> mkdir \MongoData
        F:\> mkdir \MongoLogs
7. Geben Sie Folgendes rein, um die Datenbank auszuführen:

        F:\> C:
        C:\> mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log

    Alle Protokollmeldungen werden in die Datei *F:\MongoLogs\mongolog.log* geleitet, sobald der Server „mongod.exe“ startet und die Journaldateien reserviert. Es kann einige Minuten dauern, bis MongoDB die Journaldateien reserviert hat und mit dem Lauschen auf Verbindungen beginnt. Die Eingabeaufforderung bleibt bei dieser Aufgabe, während die MongoDB-Instanz ausgeführt wird.
8. Starten Sie die MongoDB-Administrator-Shell, indem Sie unter **Start** ein weiteres Befehlsfenster öffnen und folgende Befehle eingeben:

        C:\> cd \my_mongo_dir\bin  
        C:\my_mongo_dir\bin> mongo  
        >db  
        test
        > db.foo.insert( { a : 1 } )  
        > db.foo.find()  
        { _id : ..., a : 1 }  
        > show dbs  
        ...  
        > show collections  
        ...  
        > help  

    Die Datenbank wird vom Insert erstellt.
9. Alternativ können Sie "mongod.exe" als Dienst installieren:

        C:\> mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log --logappend  --install

    Ein Dienst mit dem Namen MongoDB mit der Beschreibung „Mongo DB“ wird installiert. Zur Festlegung einer Protokolldatei muss die Option `--logpath` verwendet werden, da der laufende Dienst kein Befehlsfenster zur Anzeige der Ausgabe hat.  Mit der Option `--logappend` wird festgelegt, dass ein Neustart des Diensts eine Ausgabe erzeugt, die an die vorhandene Protokolldatei angehängt wird.  Die Option `--dbpath` gibt den Ort des Datenverzeichnisses an. Weitere dienstbezogene Befehlszeilenoptionen finden Sie unter [Service-related command line options][MongoWindowsSvcOptions] (Dienstbezogene Befehlszeilenoptionen, in englischer Sprache).

    Führen Sie zum Starten des Diensts den folgenden Befehl aus:

        C:\> net start MongoDB
10. Nach der Installation und Ausführung von MongoDB müssen Sie einen Port in der Windows-Firewall öffnen, um eine Remoteverbindung mit MongoDB herzustellen.  Wählen Sie im Menü **Start** die Option **Verwaltungstools** und dann **Windows-Firewall mit erweiterter Sicherheit**.
11. a) Klicken Sie im linken Bereich auf **Eingangsregeln**.  Wählen Sie rechts im Bereich **Aktionen** die Option **Neue Regel...**.

    ![Windows-Firewall][Image1]

    b) Klicken Sie unter **Assistent für neue eingehende Regel** auf **Port** und anschließend auf **Weiter**.

    ![Windows-Firewall][Image2]

    c) Wählen Sie **TCP** und anschließend **Bestimmte lokale Ports**.  Geben Sie als Port "27017" ein (diesen Standardport verwendet MongoDB als Überwachungsport), und klicken Sie auf **Weiter**.

    ![Windows-Firewall][Image3]

    d) Klicken Sie auf **Verbindung zulassen** und anschließend auf **Weiter**.

    ![Windows-Firewall][Image4]

    e) Klicken Sie erneut auf **Weiter**.

    ![Windows-Firewall][Image5]

    f) Geben Sie einen Namen für die Regel an, z.B. „MongoPort“, und klicken Sie auf **Fertig stellen**.

    ![Windows-Firewall][Image6]

12. Wenn Sie beim Erstellen des virtuellen Computers keinen Endpunkt für MongoDB konfiguriert haben, können Sie dies jetzt tun. Sie benötigen sowohl die Firewall-Regel als auch den Endpunkt, um eine Remote-Verbindung zu MongoDB herstellen zu können.

  Klicken Sie im Azure-Portal auf **Virtuelle Computer (klassisch)**, auf den Namen des neuen virtuellen Computers und dann auf **Endpunkte**.

    ![Endpunkte][Image7]

13. Klicken Sie auf **Hinzufügen**.

14. Fügen Sie einen Endpunkt mit dem Namen „Mongo“ hinzu, wählen Sie als Protokoll **TCP**, und setzen Sie die beiden Ports **Öffentlich** und **Privat** auf „27017“. Das Öffnen dieses Ports bewirkt, dass MongoDB per Remotezugriff erreichbar ist.

    ![Endpunkte][Image9]

> [!NOTE]
> Port 27017 wird in MongoDB als Standardport verwendet. Sie können diesen Standartport ändern, indem Sie beim Starten des mongod.exe-Servers den Parameter `--port` angeben. Achten Sie darauf, dass Sie in der Firewall die gleiche Portnummer und den „Mongo“-Endpunkt gemäß den Anweisungen oben angeben.
>
>

[MongoDownloads]: http://www.mongodb.org/downloads

[MongoWindowsSvcOptions]: http://www.mongodb.org/display/DOCS/Windows+Service


[Image1]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall1.png
[Image2]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall2.png
[Image3]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall3.png
[Image4]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall4.png
[Image5]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall5.png
[Image6]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall6.png
[Image7]: ./media/install-and-run-mongo-on-win2k8-vm/menusendpointadd.png
<!-- Removed 03/08/2017. Not in new portal. -->
<!-- [Image8]: ./media/install-and-run-mongo-on-win2k8-vm/WinVmAddEndpoint2.png
-->
[Image9]: ./media/install-and-run-mongo-on-win2k8-vm/newendpointdetails.png
