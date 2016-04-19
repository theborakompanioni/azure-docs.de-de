



Je nach Umgebung und Auswahlmöglichkeiten kann mit dem Skript die gesamte Clusterinfrastruktur erstellt werden, z. B. das virtuelle Azure-Netzwerk, Speicherkonten, Clouddienste, Domänencontroller, SQL-Datenbanken (remote oder lokal), Hauptknoten, Brokerknoten, Computeknoten und Azure-Clouddienstknoten („Burst“ oder PaaS). Alternativ dazu kann für das Skript auch bereits vorhandene Azure-Infrastruktur mit anschließender Erstellung des HPC-Clusterhauptknotens und der Brokerknoten, Computeknoten und Azure-Burstknoten verwendet werden.


Hintergrundinformationen zur Planung eines HPC Pack-Clusters finden Sie unter [Produkttest und Planung](https://technet.microsoft.com/library/jj899596.aspx) und [Erste Schritte](https://technet.microsoft.com/library/jj899590.aspx) in der HPC Pack-TechNet-Bibliothek.



## Voraussetzungen

* **Azure-Abonnement:** Sie können ein Abonnement entweder im Azure Global- oder Azure China-Dienst nutzen. Ihre Abonnementlimits haben Einfluss darauf, wie viele und welche Arten von Knoten Sie bereitstellen können. Informationen finden Sie unter [Grenzwerte, Kontingente und Einschränkungen für Azure-Abonnements und -Dienste](../articles/azure-subscription-service-limits.md).


* **Windows-Clientcomputer mit Azure PowerShell 0.8.7 oder höher (installiert und konfiguriert):** Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../articles/powershell-install-configure.md). Das Skript wird im Azure Service Management-Modus ausgeführt.


* **HPC Pack-IaaS-Bereitstellungsskript:** Laden Sie die neueste Version des Skripts aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949) herunter, und entpacken Sie sie. Führen Sie `New-HPCIaaSCluster.ps1 –Version` aus, um die Version des Skripts zu überprüfen. Dieser Artikel basiert auf der Skriptversion 4.4.0.

* **Skriptkonfigurationsdatei:** Sie müssen eine XML-Datei erstellen, die vom Skript zum Konfigurieren des HPC-Clusters verwendet wird. Informationen und Beispiele finden Sie in den Abschnitten weiter unten in diesem Artikel.


## Syntax

```
New-HPCIaaSCluster.ps1 [-ConfigFile] <String> [-AdminUserName]<String> [[-AdminPassword] <String>] [[-HPCImageName] <String>] [[-LogFile] <String>] [-Force] [-NoCleanOnFailure] [-PSSessionSkipCACheck] [<CommonParameters>]
```
>[AZURE.NOTE]Sie müssen das Skript als Administrator ausführen.

### Parameter

* **ConfigFile:** Gibt den Dateipfad der Konfigurationsdatei zum Beschreiben des HPC-Clusters an. Weitere Informationen finden Sie unter [Konfigurationsdatei](#Configuration-file) in diesem Thema oder in der Datei „Manual.rtf“ im Ordner mit dem Skript.

* **AdminUserName:** Gibt den Benutzernamen an. Wenn die Domänengesamtstruktur vom Skript erstellt wird, wird er zum lokalen Administratorbenutzernamen für alle virtuellen Computer und zum Namen des Domänenadministrators. Wenn die Domänengesamtstruktur bereits vorhanden ist, gibt dieser Parameter den Domänenbenutzer als lokalen Administratorbenutzernamen zum Installieren von HPC Pack an.

* **AdminPassword:** Gibt das Kennwort des Administrators an. Wenn der Parameter in der Befehlszeile nicht angegeben ist, werden Sie vom Skript zum Eingeben des Kennworts aufgefordert.

* **HPCImageName** (optional): Gibt den Namen des Images für den virtuellen HPC Pack-Computer an, das zum Bereitstellen des HPC-Clusters verwendet wird. Es muss sich um ein von Microsoft bereitgestelltes HPC Pack-Image aus dem Azure Marketplace handeln. Wenn der Parameter nicht angegeben wird (in den meisten Fällen zu empfehlen), wählt das Skript das zuletzt veröffentlichte HPC Pack-Image aus.

    >[AZURE.NOTE] Für die Bereitstellung tritt ein Fehler auf, wenn Sie kein gültiges HPC Pack-Image angeben.

* **LogFile** (optional): Gibt den Pfad der Bereitstellungsprotokolldatei an. Wenn der Parameter nicht angegeben ist, erstellt das Skript eine Protokolldatei im Verzeichnis „temp“ des Computers, von dem das Skript ausgeführt wird.

* **Force** (optional): Unterdrückt alle Bestätigungsaufforderungen.

* **NoCleanOnFailure** (optional): Gibt an, dass die virtuellen Azure-Computer, deren Bereitstellung nicht erfolgreich ist, nicht entfernt werden. Sie müssen diese virtuellen Computer manuell entfernen, bevor Sie das Skript zum Fortsetzen der Bereitstellung erneut ausführen. Andernfalls schlägt die Bereitstellung fehl.

* **PSSessionSkipCACheck** (optional): Für jeden Clouddienst mit von diesem Skript bereitgestellten virtuellen Computern wird von Azure automatisch ein selbst signiertes Zertifikat generiert. Alle VMs im Clouddienst verwenden dieses Zertifikat für die Windows-Remoteverwaltung (WinRM) als das Standardzertifikat. Zum Bereitstellen von HPC-Funktionen in diesen virtuellen Azure-Computern installiert das Skript standardmäßig vorübergehend diese Zertifikate im Speicher „Lokaler Computer\\Vertrauenswürdige Stammzertifizierungsstellen“ des Clientcomputers, um den Sicherheitsfehler „Keine vertrauenswürdige Zertifizierungsstelle“ während der Ausführung des Skripts zu unterdrücken. Die Zertifikate werden entfernt, wenn das Skript beendet wird. Wenn dieser Parameter angegeben wird, werden die Zertifikate nicht auf dem Clientcomputer installiert, und die Sicherheitswarnung wird unterdrückt.

    >[AZURE.IMPORTANT] Dieser Parameter wird für Bereitstellungen in der Produktion nicht empfohlen.

### Beispiel

Im folgenden Beispiel wird mit der Konfigurationsdatei *MyConfigFile.xml* ein neuer HPC Pack-Cluster erstellt, und es werden administrative Anmeldeinformationen zum Installieren des Clusters angegeben.

```
New-HPCIaaSCluster.ps1 –ConfigFile MyConfigFile.xml -AdminUserName <username> –AdminPassword <password>
```

### Zusätzliche Überlegungen

* Das Skript verwendet das HPC Pack-VM-Image des Azure Marketplace zum Erstellen des Clusterhauptknotens. Das neueste Image basiert auf Windows Server 2012 R2 Datacenter mit installiertem HPC Pack 2012 R2 Update 3.

* Das Skript kann die Auftragsübermittlung optional über das HPC Pack-Webportal oder die HPC Pack-REST-API aktivieren.

* Das Skript kann optional benutzerdefinierte Skripts für Schritte vor und nach der Konfiguration auf dem Hauptknoten ausführen, falls Sie zusätzliche Software installieren oder andere Einstellungen konfigurieren möchten.


## Konfigurationsdatei

Die Konfigurationsdatei für das Bereitstellungsskript ist eine XML-Datei. Die Schemadatei „HPCIaaSClusterConfig.xsd“ befindet sich im Ordner des HPC Pack-IaaS-Bereitstellungsskripts. **IaaSClusterConfig** ist das Stammelement der Konfigurationsdatei mit den untergeordneten Elementen. Dies ist in der Datei „Manual.rtf“ im Ordner des Bereitstellungsskripts ausführlich beschrieben. Beispieldateien für unterschiedliche Szenarien finden Sie unter [Beispielkonfigurationsdateien](#Example-configuration-files) in diesem Artikel.

<!---HONumber=AcomDC_0406_2016-->