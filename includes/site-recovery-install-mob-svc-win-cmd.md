1. Kopieren Sie das Installationsprogramm in einen lokalen Ordner (z.B. „C:\Temp“) auf dem Server, den Sie schützen möchten. Führen Sie die folgenden Befehle an einer Eingabeaufforderung als Administrator aus:

  ```
  cd C:\Temp
  ren Microsoft-ASR_UA*Windows*release.exe MobilityServiceInstaller.exe
  MobilityServiceInstaller.exe /q /x:C:\Temp\Extracted
  cd C:\Temp\Extracted.
  ```
2. Führen Sie den folgenden Befehl aus, um den Mobilitätsdienst zu installieren:

  ```
  UnifiedAgent.exe /Role "MS" /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /Platform "VmWare" /Silent
  ```
3. Nun muss der Agent beim Konfigurationsserver registriert werden.

  ```
  cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
  UnifiedAgentConfigurator.exe”  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
  ```

#### <a name="mobility-service-installer-command-line-arguments"></a>Befehlszeilenargumente für Mobilitätsdienst-Installationsprogramm

```
Usage :
UnifiedAgent.exe /Role <MS|MT> /InstallLocation <Install Location> /Platform “VmWare” /Silent
```

| Parameter|Typ|Beschreibung|Mögliche Werte|
|-|-|-|-|
|/Role|Erforderlich|Gibt an, ob Mobility Service (MS) oder MasterTarget (MT) installiert werden soll|MS </br> MT|
|/InstallLocation|Optional|Installationsort des Mobilitätsdiensts|Beliebiger Ordner auf dem Computer|
|/Platform|Erforderlich|Gibt die Plattform an, auf der Mobility Service installiert wird </br> </br>- **VMware**: Verwenden Sie diesen Wert für die Installation von Mobility Service auf einem virtuellen Computer, der auf *VMware vSphere ESXi-Hosts*, *Hyper-V-Hosts* und *physischen Servern* ausgeführt wird </br> - **Azure**: Verwenden Sie diesen Wert, wenn Sie den Agent auf einem virtueller Azure-IaaS-Computer installieren| VMware </br> Azure|
|/Silent|Optional|Gibt an, dass das Installationsprogramm im unbeaufsichtigten Modus ausgeführt werden soll| NA|

>[!TIP]
> Die Setupprotokolle finden Sie unter: %ProgramData%\ASRSetupLogs\ASRUnifiedAgentInstaller.log

#### <a name="mobility-service-registration-command-line-arguments"></a>Befehlszeilenargumente für die Mobility Service-Registrierung

```
Usage :
UnifiedAgentConfigurator.exe”  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
```

  | Parameter|Typ|Beschreibung|Mögliche Werte|
  |-|-|-|-|
  |/CSEndPoint |Erforderlich|IP-Adresse des Konfigurationsservers| Beliebige gültige IP-Adresse|
  |/PassphraseFilePath|Erforderlich|Speicherort der Passphrase |Beliebiger UNC- oder lokaler Dateipfad|


>[!TIP]
> Die AgentConfiguration-Protokolle finden Sie unter: %ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log
