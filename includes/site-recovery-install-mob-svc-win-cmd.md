1. Kopieren Sie das Installationsprogramm in einen lokalen Ordner (z.B. „C:\Temp“) auf dem Server, den Sie schützen möchten, und führen Sie an einer Eingabeaufforderung mit erhöhten Rechten die folgenden Befehle aus.

  ```
  cd C:\Temp
  ren Microsoft-ASR_UA*Windows*release.exe MobilityServiceInstaller.exe
  MobilityServiceInstaller.exe /q /xC:\Temp\Extracted
  cd C:\Temp\Extracted.
  ```
2. Jetzt können wir den Mobilitätsdienst mit der folgenden Befehlszeile installieren.

  ```
  UnifiedAgent.exe /Role "Agent" /CSEndpoint "IP Address of Configuration Server" /PassphraseFilePath <Full path to the passphrase file>``
  ```

#### <a name="mobility-service-installer-command-line-arguments"></a>Befehlszeilenargumente für Mobilitätsdienst-Installationsprogramm

```
Usage :
UnifiedAgent.exe [/Role <Agent/MasterTarget>] [/InstallLocation <Installation Directory>] [/CSIP <IP address>] [/PassphraseFilePath <Passphrase file path>] [/LogFilePath <Log File Path>]<br/>
```

  | Parameter|Typ|Beschreibung|Mögliche Werte|
  |-|-|-|-|
  |/Role|Erforderlich|Gibt an, ob der Mobilitätsdienst installiert werden soll.|Agent </br> MasterTarget|
  |/InstallLocation|Erforderlich|Installationsort des Mobilitätsdiensts|Beliebiger Ordner auf dem Computer|
  |/CSIP|Erforderlich|IP-Adresse des Konfigurationsservers| Beliebige gültige IP-Adresse|
  |/PassphraseFilePath|Erforderlich|Speicherort der Passphrase |Beliebiger UNC- oder lokaler Dateipfad|
  |/LogGilePath|Optional|Speicherort für das Installationsprotokoll|Beliebiger gültiger Ordner auf dem Computer|

#### <a name="sample-usage"></a>Beispiel für Verwendung

```
  UnifiedAgent.exe /Role "Agent" /CSEndpoint "I192.168.2.35" /PassphraseFilePath "C:\Temp\MobSvc.passphrase"
```


<!--HONumber=Jan17_HO3-->


