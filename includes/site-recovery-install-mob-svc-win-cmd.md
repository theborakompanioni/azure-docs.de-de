1. Kopieren Sie das Installationsprogramm in einen lokalen Ordner (z.B. „C:\Temp“) auf dem Server, den Sie schützen möchten. Führen Sie die folgenden Befehle an einer Eingabeaufforderung als Administrator aus:

  ```
  cd C:\Temp
  ren Microsoft-ASR_UA*Windows*release.exe MobilityServiceInstaller.exe
  MobilityServiceInstaller.exe /q /x:C:\Temp\Extracted
  cd C:\Temp\Extracted.
  ```
2. Führen Sie den folgenden Befehl aus, um den Mobilitätsdienst zu installieren:

  ```
  UnifiedAgent.exe /Role "Agent" /CSEndpoint "IP address of the configuration server" /PassphraseFilePath <Full path to the passphrase file>``
  ```

#### <a name="mobility-service-installer-command-line-arguments"></a>Befehlszeilenargumente für Mobilitätsdienst-Installationsprogramm

```
Usage :
UnifiedAgent.exe [/Role <Agent/MasterTarget>] [/InstallLocation <Installation directory>] [/CSIP <IP address>] [/PassphraseFilePath <Passphrase file path>] [/LogFilePath <Log file path>]<br/>
```

  | Parameter|Typ|Beschreibung|Mögliche Werte|
  |-|-|-|-|
  |/Role|Erforderlich|Gibt an, ob der Mobilitätsdienst installiert werden soll|Agent </br> MasterTarget|
  |/InstallLocation|Erforderlich|Installationsort des Mobilitätsdiensts|Beliebiger Ordner auf dem Computer|
  |/CSIP|Erforderlich|IP-Adresse des Konfigurationsservers| Beliebige gültige IP-Adresse|
  |/PassphraseFilePath|Erforderlich|Speicherort der Passphrase |Beliebiger UNC- oder lokaler Dateipfad|
  |/LogFilePath|Optional|Speicherort des Installationsprotokolls|Beliebiger gültiger Ordner auf dem Computer|

#### <a name="example"></a>Beispiel

```
  UnifiedAgent.exe /Role "Agent" /CSEndpoint "I192.168.2.35" /PassphraseFilePath "C:\Temp\MobSvc.passphrase"
```
