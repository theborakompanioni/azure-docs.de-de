UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP-Adresse für die Datenübertragung] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]

Parameter:

* /ServerMode: Obligatorisch. Gibt an, ob sowohl der Konfigurations- als auch der Prozessserver oder nur der Prozessserver installiert werden soll. Eingabewerte: CS, PS.
* InstallLocation: Obligatorisch. Der Ordner, in den die Komponenten installiert werden.
* /MySQLCredsFilePath. Obligatorisch. Der Dateipfad, unter dem die MySQL Server-Anmeldeinformationen gespeichert werden. Die Datei sollte folgendes Format aufweisen:
* [MySQLCredentials]
* MySQLRootPassword = "<Password>"
* MySQLUserPassword = "<Password>"
* /VaultCredsFilePath. Obligatorisch. Der Speicherort der Datei mit den Anmeldeinformationen für den Tresor.
* /EnvType. Obligatorisch. Der Installationstyp. Werte: VMware, NonVMware
* /PSIP und /CSIP. Obligatorisch. Die IP-Adressen des Prozessservers und des Konfigurationsservers.
* /PassphraseFilePath. Obligatorisch. Der Speicherort der Datei mit der Passphrase.
* /BypassProxy. Optional. Gibt an, dass der Konfigurationsserver die Verbindung mit Azure ohne Proxy herstellt.
* /ProxySettingsFilePath. Optional. Proxyeinstellungen (der standardmäßige Proxy erfordert eine Authentifizierung oder einen benutzerdefinierten Proxy). Die Datei sollte folgendes Format aufweisen:
* [ProxySettings]
* ProxyAuthentication = "Yes/No"
* Proxy IP = "IP Address>"
* ProxyPort = "<Port>"
* ProxyUserName="<User Name>"
* ProxyPassword="<Password>"
* DataTransferSecurePort. Optional. Die Portnummer für Replikationsdaten.
* SkipSpaceCheck. Optional. Überspringt die Überprüfung des Speicherplatzes für Cache.
* AcceptThirdpartyEULA. Obligatorisch. Akzeptiert die Drittanbieterlizenzbedingungen.
* ShowThirdpartyEULA. Obligatorisch. Zeigt Drittanbieterlizenzbedingungen an. Alle anderen Parameter werden ignoriert, wenn sie als Eingabe bereitgestellt werden.
