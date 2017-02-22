1. Kopieren Sie das Installationsprogramm in einen lokalen Ordner (z.B. „/tmp“) auf dem Server, den Sie schützen möchten, und führen Sie in einem Terminalfenster die folgenden Befehle aus.
  ```
  cd /tmp
  tar -xvzf Microsoft-ASR_UA*release.tar.gz
  ```
2. Jetzt können wir den Mobilitätsdienst mit der folgenden Befehlszeile installieren.

  ```
  sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P MobSvc.passphrase
  ```

#### <a name="mobility-service-installer-command-line-arguments"></a>Befehlszeilenargumente für Mobilitätsdienst-Installationsprogramm

|Parameter|Typ|Beschreibung|Mögliche Werte|
|-|-|-|-|
|-t |Erforderlich|Agent-Typ<br>(Gilt ab der nächsten Version als veraltet.)|*both*|
|-a |Erforderlich|Agent-Konfiguration<br>(Gilt ab der nächsten Version als veraltet.) |*host*|
|-R |Optional|Rolle des Agents|Agent<br>MasterTarget|
|-d angeben, |Optional|Installationsort des Mobilitätsdiensts|/usr/local/ASR|
|-i |Erforderlich|IP-Adresse des Konfigurationsservers.|Beliebige gültige IP-Adresse|
|-p |Erforderlich|Port, über den der Konfigurationsserver auf eingehende Verbindungen lauscht|443|
|-s |Erforderlich|Starten des Diensts nach einer erfolgreichen Installation<br>(Gilt ab der nächsten Version als veraltet.)|*y*|
|-c |Erforderlich|Kommunikationsmodus zwischen Agent und Prozessserver<br>(Gilt ab der nächsten Version als veraltet.) |*https*|
|-P |Erforderlich|Passphrase des Konfigurationsservers|Beliebiger UNC- oder lokaler Dateipfad|


#### <a name="sample-usage"></a>Beispiel für Verwendung
```
sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i 192.168.2.53 -p 443 -s y -c https -P /tmp/MobSvc.passphrase
```


<!--HONumber=Jan17_HO3-->


