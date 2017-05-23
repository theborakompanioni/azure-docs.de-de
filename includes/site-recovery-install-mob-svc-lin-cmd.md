1. Kopieren Sie das Installationsprogramm in einen lokalen Ordner (z.B. „/tmp“) auf dem Server, den Sie schützen möchten. Führen Sie an einem Terminal die folgenden Befehle aus:
  ```
  cd /tmp
  tar -xvzf Microsoft-ASR_UA*release.tar.gz
  ```
2. Führen Sie den folgenden Befehl aus, um den Mobilitätsdienst zu installieren:

  ```
  sudo ./install -d <Install Location> -r MS -v VmWare -q
  ```
3. Nach Abschluss der Installation muss Mobility Service auf dem Konfigurationsserver registriert werden. Führen Sie den folgenden Befehl aus, um Mobility Service beim Konfigurationsserver zu registrieren.

  ```
  /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <CSIP> -P /var/passphrase.txt
  ```

#### <a name="mobility-service-installer-command-line"></a>Befehlszeile für das Mobility Service-Installationsprogramm

```
Usage:
./install -d <Install Location> -r <MS|MT> -v VmWare -q
```

|Parameter|Typ|Beschreibung|Mögliche Werte|
|-|-|-|-|
|-r |Erforderlich|Gibt an, ob Mobility Service (MS) oder MasterTarget (MT) installiert werden soll|MS </br> MT|
|-d angeben, |Optional|Installationsort des Mobilitätsdiensts|/usr/local/ASR|
|-v|Erforderlich|Gibt die Plattform an, auf der Mobility Service installiert wird </br> </br>- **VMware**: Verwenden Sie diesen Wert für die Installation von Mobility Service auf einem virtuellen Computer, der auf *VMware vSphere ESXi-Hosts*, *Hyper-V-Hosts* und *physischen Servern* ausgeführt wird </br> - **Azure**: Verwenden Sie diesen Wert, wenn Sie den Agent auf einem virtueller Azure-IaaS-Computer installieren| VMware </br> Azure|
|-q|Optional|Gibt an, dass das Installationsprogramm im unbeaufsichtigten Modus ausgeführt werden soll| N/V|


#### <a name="mobility-service-configuration-command-line"></a>Befehlszeile für die Mobility Service-Konfiguration

```
Usage:
cd /usr/local/ASR/Vx/bin
UnifiedAgentConfigurator.sh -i <CSIP> -P <PassphraseFilePath>
```

|Parameter|Typ|Beschreibung|Mögliche Werte|
|-|-|-|-|
|-i |Erforderlich|IP des Konfigurationsservers|Beliebige gültige IP-Adresse|
|-P |Erforderlich|Vollständiger Dateipfad der Datei, in der die Passphrase der Verbindung gespeichert ist|Ein beliebiger gültiger Ordner|
