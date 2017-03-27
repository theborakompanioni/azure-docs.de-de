1. Kopieren Sie das Installationsprogramm in einen lokalen Ordner (z.B. „/tmp“) auf dem Server, den Sie schützen möchten. Führen Sie an einem Terminal die folgenden Befehle aus:
  ```
  cd /tmp
  tar -xvzf Microsoft-ASR_UA*release.tar.gz
  ```
2. Führen Sie den folgenden Befehl aus, um den Mobilitätsdienst zu installieren:

  ```
  sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P MobSvc.passphrase
  ```

#### <a name="mobility-service-installer-command-line-arguments"></a>Befehlszeilenargumente für Mobilitätsdienst-Installationsprogramm

|Parameter|Typ|Beschreibung|Mögliche Werte|
|-|-|-|-|
|-t |Erforderlich|Agent-Typ<br>(gilt ab einer der nächsten Versionen als veraltet)|*both*|
|-a |Erforderlich|Agent-Konfiguration<br>(gilt ab einer der nächsten Versionen als veraltet) |*host*|
|-R |Optional|Rolle des Agents|Agent<br>MasterTarget|
|-d angeben, |Optional|Installationsort des Mobilitätsdiensts|/usr/local/ASR|
|-i |Erforderlich|IP-Adresse des Konfigurationsservers|Beliebige gültige IP-Adresse|
|-p |Erforderlich|Port, über den der Konfigurationsserver auf eingehende Verbindungen lauscht|443|
|-s |Erforderlich|Startet den Dienst nach einer erfolgreichen Installation<br>(gilt ab einer der nächsten Versionen als veraltet)|*y*|
|-c |Erforderlich|Kommunikationsmodus zwischen Agent und Prozessserver<br>(gilt ab einer der nächsten Versionen als veraltet) |*https*|
|-P |Erforderlich|Passphrase des Konfigurationsservers|Beliebiger UNC- oder lokaler Dateipfad|


#### <a name="example"></a>Beispiel
```
sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i 192.168.2.53 -p 443 -s y -c https -P /tmp/MobSvc.passphrase
```
