



Je nach Umgebung und Auswahlmöglichkeiten kann mit dem Skript die gesamte Clusterinfrastruktur erstellt werden, z.B. das virtuelle Azure-Netzwerk, Speicherkonten, Clouddienste, Domänencontroller, SQL-Datenbanken (remote oder lokal), Hauptknoten und zusätzliche Clusterknoten. Alternativ dazu kann das Skript auch die bereits vorhandene Azure-Infrastruktur nutzen und nur die HPC-Clusterknoten erstellen.

Hintergrundinformationen zur Planung eines HPC Pack-Clusters finden Sie unter [Produkttest und Planung](https://technet.microsoft.com/library/jj899596.aspx) sowie unter [Erste Schritte](https://technet.microsoft.com/library/jj899590.aspx) in der HPC Pack 2012 R2-TechNet-Bibliothek.

## <a name="prerequisites"></a>Voraussetzungen
* **Azure-Abonnement**: Sie können ein Abonnement entweder unter dem Azure Global- oder dem Azure China-Dienst nutzen. Ihre Abonnementlimits haben Einfluss darauf, wie viele und welche Arten von Knoten Sie bereitstellen können. Informationen finden Sie unter [Grenzwerte, Kontingente und Einschränkungen für Azure-Abonnements und -Dienste](../articles/azure-subscription-service-limits.md).
* **Windows-Clientcomputer mit Azure PowerShell 0.8.10 oder höher (installiert und konfiguriert)**: Weitere Informationen zur Installation und Schritte zum Verknüpfen des Azure-Abonnements finden Sie unter [Erste Schritte mit Azure PowerShell](/powershell/azureps-cmdlets-docs).
* **HPC Pack-IaaS-Bereitstellungsskript**: Laden Sie die neueste Version des Skripts aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949) herunter, und entpacken Sie sie. Führen Sie `New-HPCIaaSCluster.ps1 –Version`aus, um die Version des Skripts zu überprüfen. Dieser Artikel basiert auf der Skriptversion 4.5.2.
* **Skriptkonfigurationsdatei**: Erstellen Sie eine XML-Datei, die vom Skript zum Konfigurieren des HPC-Clusters verwendet wird. Informationen und Beispiele finden Sie in den Abschnitten weiter unten in diesem Artikel sowie in der Datei „Manual.rtf“ für das Bereitstellungsskript.

## <a name="syntax"></a>Syntax
```PowerShell
New-HPCIaaSCluster.ps1 [-ConfigFile] <String> [-AdminUserName]<String> [[-AdminPassword] <String>] [[-HPCImageName] <String>] [[-LogFile] <String>] [-Force] [-NoCleanOnFailure] [-PSSessionSkipCACheck] [<CommonParameters>]
```
> [!NOTE]
> Führen Sie das Skript als Administrator aus.
> 
> 

### <a name="parameters"></a>Parameter
* **ConfigFile**: Gibt den Dateipfad der Konfigurationsdatei zum Beschreiben des HPC-Clusters an. Weitere Informationen zur Konfigurationsdatei finden Sie in diesem Thema oder in der Datei „Manual.rtf“ im Ordner mit dem Skript.
* **AdminUserName**: Gibt den Benutzernamen an. Wenn die Domänengesamtstruktur vom Skript erstellt wird, wird er zum lokalen Administratorbenutzernamen für alle virtuellen Computer und zum Namen des Domänenadministrators. Wenn die Domänengesamtstruktur bereits vorhanden ist, gibt dieser Parameter den Domänenbenutzer als lokalen Administratorbenutzernamen zum Installieren von HPC Pack an.
* **AdminPassword**: Gibt das Kennwort des Administrators an. Wenn der Parameter in der Befehlszeile nicht angegeben ist, werden Sie vom Skript zum Eingeben des Kennworts aufgefordert.
* **HPCImageName** (optional): Gibt den Namen des Image für den virtuellen HPC Pack-Computer an, das zum Bereitstellen des HPC-Clusters verwendet wird. Es muss sich um ein von Microsoft bereitgestelltes HPC Pack-Image aus dem Azure Marketplace handeln. Wenn der Parameter nicht angegeben wird (in den meisten Fällen zu empfehlen), wählt das Skript das zuletzt veröffentlichte [HPC Pack 2012 R2-Image](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) aus. Das neueste Image basiert auf Windows Server 2012 R2 Datacenter mit installiertem HPC Pack 2012 R2 Update 3.
  
  > [!NOTE]
  > Für die Bereitstellung tritt ein Fehler auf, wenn Sie kein gültiges HPC Pack-Image angeben.
  > 
  > 
* **LogFile** (optional): Gibt den Pfad der Bereitstellungsprotokolldatei an. Wenn der Parameter nicht angegeben ist, erstellt das Skript eine Protokolldatei im Verzeichnis „temp“ des Computers, von dem das Skript ausgeführt wird.
* **Force** (optional): Unterdrückt alle Bestätigungsaufforderungen.
* **NoCleanOnFailure** (optional): Gibt an, dass die virtuellen Azure-Computer, deren Bereitstellung nicht erfolgreich ist, nicht entfernt werden. Entfernen Sie diese virtuellen Computer manuell, bevor Sie das Skript zum Fortsetzen der Bereitstellung erneut ausführen. Andernfalls schlägt die Bereitstellung fehl.
* **PSSessionSkipCACheck** (optional): Für jeden Clouddienst mit von diesem Skript bereitgestellten virtuellen Computern wird von Azure automatisch ein selbstsigniertes Zertifikat generiert. Alle virtuellen Computer im Clouddienst verwenden dieses Zertifikat als Standardzertifikat für die Windows-Remoteverwaltung (WinRM). Zum Bereitstellen von HPC-Funktionen für diese virtuellen Azure-Computer installiert das Skript diese Zertifikate standardmäßig vorübergehend unter „Lokaler Computer\\Vertrauenswürdige Stammzertifizierungsstellen“ auf dem Clientcomputer, um den Sicherheitsfehler „Keine vertrauenswürdige Zertifizierungsstelle“ während der Ausführung des Skripts zu unterdrücken. Die Zertifikate werden entfernt, wenn das Skript beendet wird. Wenn dieser Parameter angegeben wird, werden die Zertifikate nicht auf dem Clientcomputer installiert, und die Sicherheitswarnung wird unterdrückt.
  
  > [!IMPORTANT]
  > Dieser Parameter wird für Bereitstellungen in der Produktion nicht empfohlen.
  > 
  > 

### <a name="example"></a>Beispiel
Im folgenden Beispiel wird mit der Konfigurationsdatei *MyConfigFile.xml* ein HPC Pack-Cluster erstellt, und es werden Administrator-Anmeldeinformationen zum Installieren des Clusters angegeben.

```PowerShell
.\New-HPCIaaSCluster.ps1 –ConfigFile MyConfigFile.xml -AdminUserName <username> –AdminPassword <password>
```

### <a name="additional-considerations"></a>Zusätzliche Überlegungen
* Das Skript kann die Auftragsübermittlung optional über das HPC Pack-Webportal oder die HPC Pack-REST-API aktivieren.
* Das Skript kann optional benutzerdefinierte Skripts für Schritte vor und nach der Konfiguration auf dem Hauptknoten ausführen, falls Sie zusätzliche Software installieren oder andere Einstellungen konfigurieren möchten.

## <a name="configuration-file"></a>Konfigurationsdatei
Die Konfigurationsdatei für das Bereitstellungsskript ist eine XML-Datei. Die Schemadatei „HPCIaaSClusterConfig.xsd“ befindet sich im Ordner des HPC Pack-IaaS-Bereitstellungsskripts. **IaaSClusterConfig** ist das Stammelement der Konfigurationsdatei mit den untergeordneten Elementen. Diese werden in der Datei „Manual.rtf“ im Ordner des Bereitstellungsskripts ausführlich beschrieben.

