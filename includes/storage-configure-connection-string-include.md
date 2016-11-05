## Einrichten einer Speicherverbindungszeichenfolge
Die Azure-Speicherclientbibliothek für .NET unterstützt die Verwendung einer Speicherverbindungszeichenfolge zum Konfigurieren von Endpunkten und Anmeldeinformationen für den Zugriff auf Speicherdienste. Am einfachsten lässt sich die Speicherverbindungszeichenfolge in einer Konfigurationsdatei verwalten.

Weitere Informationen zu Verbindungszeichenfolgen finden Sie unter [Konfigurieren von Verbindungszeichenfolgen für Azure Storage](../articles/storage/storage-configure-connection-string.md).

> [!NOTE]
> Ihr Speicherkontoschlüssel ähnelt dem Stammkennwort für das Speicherkonto. Achten Sie darauf, den Speicherkontoschlüssel immer gut zu schützen. Geben Sie ihn nicht an andere Benutzer weiter, vermeiden Sie das Hartcodieren, und speichern Sie ihn nicht in einer Klartextdatei, auf die andere Benutzer zugreifen können. Generieren Sie Ihren Schlüssel mithilfe des Azure-Portals neu, wenn Sie der Meinung sind, dass er nicht mehr sicher ist.
> 
> 

### Bestimmen der Zielumgebung
Es gibt für die Ausführung der Beispiele in diesem Leitfaden zwei Umgebungsoptionen:

* Sie können den Code über den Azure-Speicheremulator ausführen. Der Speicheremulator ist eine lokale Umgebung, in der ein Azure Storage-Konto in der Cloud emuliert wird. Der Emulator ist eine kostenlose Option zum Testen und Debuggen von Code, während sich Ihre Anwendung in der Entwicklung befindet. Für den Emulator wird ein bekanntes Konto mit Schlüssel verwendet. Weitere Informationen finden Sie unter [Einsatz des Azure-Speicheremulators für Entwicklung und Tests](../articles/storage/storage-use-emulator.md).
* Sie können den Code über ein Azure Storage-Konto in der Cloud ausführen. 

Wenn Sie ein Speicherkonto in der Cloud verwenden möchten, müssen Sie den primären Zugriffsschlüssel für Ihr Speicherkonto aus dem Azure-Portal kopieren. Weitere Informationen finden Sie unter [Anzeigen und Kopieren von Speicherzugriffsschlüsseln](../articles/storage/storage-create-storage-account.md#view-and-copy-storage-access-keys).

> [!NOTE]
> Sie können den Speicheremulator als Ziel festlegen, um jegliche Kosten im Zusammenhang mit Azure Storage zu vermeiden. Wenn jedoch ein Azure-Speicherkonto in der Cloud das Ziel sein soll, sind die Kosten für das Ausführen dieses Lernprogramms vernachlässigbar.
> 
> 

### Konfigurieren der Verbindungszeichenfolge per .NET-Konfiguration
Wenn Ihre Anwendung auf einem Desktopgerät oder mobilen Gerät, einem virtuellen Azure-Computer oder in einer Azure-Web-App ausgeführt wird, sollten Sie die Verbindungszeichenfolge per .NET-Konfiguration speichern (z.B. in der Datei `web.config` oder `app.config` der Anwendung). Speichern Sie die Verbindungszeichenfolge wie folgt mit dem `<appSettings>`-Element: Ersetzen Sie `account-name` durch den Namen Ihres Speicherkontos und `account-key` durch den Zugriffsschlüssel des Kontos:

    <configuration>
          <appSettings>
            <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key" />
          </appSettings>
    </configuration>

Ihre Konfigurationseinstellung kann beispielsweise wie folgt lauten:

    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=account-key" />

Wenn Sie den Speicheremulator verwenden möchten, können Sie eine Verknüpfung nutzen, mit der eine Zuordnung zum bekannten Kontonamen und dem Schlüssel hergestellt wird. In diesem Fall lautet die Verbindungszeichenfolge wie folgt:

    <add key="StorageConnectionString" value="UseDevelopmentStorage=true;" />

### Konfigurieren der Verbindungszeichenfolge für einen Azure-Clouddienst
Wenn Ihre Anwendung in einem Azure-Clouddienst ausgeführt wird, speichern Sie die Verbindungszeichenfolge mit den Azure-Dienstkonfigurationsdateien (`*.csdef`- und `*.cscfg`-Dateien). Weitere Informationen zur Azure-Clouddienstkonfiguration finden Sie unter [Erstellen und Bereitstellen eines Clouddiensts](../articles/cloud-services/cloud-services-how-to-create-deploy.md).

<!---HONumber=AcomDC_0406_2016-->