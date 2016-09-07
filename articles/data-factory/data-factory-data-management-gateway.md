<properties 
	pageTitle="Datenverwaltungsgateway für Data Factory | Microsoft Azure"
	description="Richten Sie ein Datengateway ein, um Daten zwischen dem lokalen Speicher und der Cloud zu verschieben. Verwenden Sie das Datenverwaltungsgateway in Azure Data Factory zum Verschieben Ihrer Daten." 
	services="data-factory" 
	documentationCenter="" 
	authors="spelluru" 
	manager="jhubbard" 
	editor="monicar"/>

<tags 
	ms.service="data-factory" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="08/05/2016" 
	ms.author="spelluru"/>

# Gateway zur Datenverwaltung
Das Datenverwaltungsgateway ist ein Client-Agent, den Sie in Ihrer lokalen Umgebung installieren müssen, um die Verschiebung von Daten zwischen der Cloud und lokalen Datenspeichern zu aktivieren, die durch die [Kopieraktivität unterstützt werden](data-factory-data-movement-activities.md##supported-data-stores).

Dieser Artikel ergänzt den Artikel [Verschieben von Daten zwischen lokalen Quellen und der Cloud mit dem Datenverwaltungsgateway](data-factory-move-data-between-onprem-and-cloud.md), der eine exemplarische Vorgehensweise für das Erstellen einer Data Factory-Pipeline darstellt, wobei das Gateway verwendet wird, um Daten aus einer lokalen SQL Server-Datenbank in ein Azure-Blob zu verschieben. Dieser Artikel bietet ausführliche Informationen zur Verwendung des Datenverwaltungsgateways.

## Übersicht

### Funktionen des Datenverwaltungsgateways
Das Datenverwaltungsgateway bietet die folgenden Funktionen:

- Modellieren von lokalen Datenquellen und Clouddatenquellen innerhalb einer Data Factory und Verschieben von Daten.
- Nutzen einer zentralen Konsole für die Überwachung und Verwaltung mit Informationen über den Gatewaystatus über das Data Factory-Blatt.
- Sicheres Verwalten des Zugriffs auf lokale Datenquellen.
	- An der Unternehmensfirewall ist keine Änderung erforderlich. Das Gateway stellt nur ausgehende HTTP-basierte Verbindungen mit dem offenen Internet her.
	- Verschlüsseln Sie die Anmeldeinformationen für Ihre lokalen Datenspeicher mit dem Zertifikat.
- Effizientes Verschieben von Daten – Daten werden parallel verschoben, und zwar mit Ausfallsicherheit bei vorübergehenden Netzwerkproblemen dank automatischer Wiederholungslogik.

### Befehls- und Datenfluss
Wenn Sie eine Kopieraktivität in einer Datenpipeline verwenden, um lokale Daten in der Cloud zur weiteren Verarbeitung zu erfassen oder um Ergebnisdaten in der Cloud wieder in einen lokalen Datenspeicher zu exportieren, verwendet die Kopieraktivität intern ein Gateway zum Übertragen von Daten aus der lokalen Datenquelle in die Cloud und umgekehrt.

Im Anschluss finden Sie eine Darstellung des allgemeinen Datenflusses sowie eine Zusammenfassung der Schritte für das Kopieren mit dem Datengateway: ![Datenfluss über ein Gateway](./media/data-factory-data-management-gateway/data-flow-using-gateway.png)

1.	Der Datenentwickler erstellt entweder über das [Azure-Portal](https://portal.azure.com) oder mit einem [PowerShell-Cmdlet](https://msdn.microsoft.com/library/dn820234.aspx) ein Gateway für Azure Data Factory.
2.	Der Datenentwickler erstellt durch Angabe des Gateways einen verknüpften Dienst für einen lokalen Datenspeicher. Als Teil der Einrichtung des verknüpften Diensts verwendet der Datenentwickler die Anwendung „Anmeldeinformationen festlegen“, um Authentifizierungstypen und Anmeldeinformationen anzugeben. Die Anwendung „Anmeldeinformationen festlegen“ kommuniziert mit dem Datenspeicher, um die Verbindung zu testen, und mit dem Gateway, um die Anmeldeinformationen zu speichern.
3. Das Gateway verschlüsselt die Anmeldeinformationen mit dem Zertifikat, das dem Gateway zugeordnet wurde (bereitgestellt vom Datenentwickler), bevor die Anmeldeinformationen in der Cloud gespeichert werden.
4. Der Data Factory-Dienst kommuniziert für die Planung und Verwaltung von Aufträgen mit dem Gateway. Dazu wird ein Steuerungskanal genutzt, der eine freigegebene Azure Service Bus-Warteschlange verwendet. Wenn ein Auftrag der Kopieraktivität gestartet werden muss, reiht Data Factory die Anforderung zusammen mit Anmeldeinformationen in die Warteschlange ein. Das Gateway startet den Auftrag, nachdem die Warteschlange abgerufen wurde.
5.	Das Gateway entschlüsselt die Anmeldeinformationen mit dem gleichen Zertifikat und stellt dann mit dem richtigen Authentifizierungstyp und den Anmeldeinformationen eine Verbindung mit dem lokalen Datenspeicher her.
6.	Das Gateway kopiert Daten aus dem lokalen Speicher in einen Cloudspeicher oder aus einem Cloudspeicher in einen lokalen Datenspeicher. Dies ist abhängig von der Konfiguration der Kopieraktivität in der Datenpipeline. Hinweis: Für diesen Schritt kommuniziert das Gateway über einen sicheren Kanal (HTTPS) direkt mit dem cloudbasierten Speicherdienst (beispielsweise Azure-Blob, Azure SQL oder Ähnliches).

### Überlegungen zur Verwendung des Gateways
- Eine einzelne Instanz des Datenverwaltungsgateways kann zwar für mehrere lokale Datenquellen verwendet werden, eine **einzelne Gatewayinstanz ist jedoch nur an eine Azure Data Factory gebunden** und kann nicht gemeinsam mit einer anderen Data Factory genutzt werden.
- Es darf **nur eine Instanz des Datenverwaltungsgateways** auf einem Computer installiert sein. Angenommen, Sie haben zwei Data Factorys, die Zugriff auf lokale Datenquellen benötigen, dann müssen Sie die Gateways auf zwei lokalen Computern installieren, wobei jedes Gateway an eine separate Daten Factory gebunden ist.
- Das **Gateway muss sich nicht auf dem gleichen Computer wie die Datenquelle befinden**, aber die Nähe zur Datenquelle verkürzt die Verbindungszeit des Gateways mit der Datenquelle. Es wird empfohlen, das Gateway auf einem anderen Computer als dem Computer zu installieren, auf dem die lokale Datenquelle gehostet wird. So konkurriert das Gateway nicht mit der Datenquelle um Ressourcen.
- Sie können über **mehrere Gateways auf verschiedenen Computern verfügen, die eine Verbindung mit der gleichen lokalen Datenquelle herstellen**. Sie können z. B. über zwei Gateways verfügen, die zwei Data Factorys mit Daten versorgen, wobei jedoch die lokale Datenquelle für die beiden Data Factorys registriert ist.
- Falls Sie auf Ihrem Computer bereits ein Gateway installiert haben, das für ein **Power BI**-Szenario verwendet wird, installieren Sie auf einem anderen Computer ein **separates Gateway für die Azure Data Factory**.
- Sie müssen **das Gateway selbst bei der Verwendung von ExpressRoute verwenden**.
- Auch bei Verwendung von **ExpressRoute** und **des Gateways**, um eine Verbindung zwischen dem Dienst und der Datenquelle herzustellen, sollten Sie Ihre Datenquelle wie eine lokale Datenquelle behandeln (die sich hinter einer Firewall befindet).
- Sie müssen auch dann **das Gateway verwenden**, wenn der Datenspeicher sich in der Cloud auf einer **Azure IaaS-VM** befindet.

## Installation

### Voraussetzungen
- Die unterstützten **Betriebssystemversionen** sind Windows 7, Windows 8/8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012 und Windows Server 2012 R2. Die Installation des Datenverwaltungsgateways auf einem Domänencontroller wird derzeit nicht unterstützt.
- Sie benötigen mindestens .NET Framework 4.5.1. Wenn Sie das Gateway auf einem Windows 7-Computer installieren, installieren Sie mindestens .NET Framework 4.5. Ausführlichere Informationen finden Sie unter [Systemanforderungen für .NET Framework](https://msdn.microsoft.com/library/8z6watww.aspx).
- Die empfohlene **Konfiguration** für den Gatewaycomputer lautet: mindestens 2 GHz, 4 Kerne, 8 GB RAM und 80 GB Festplattenspeicher.
- Wenn der Hostcomputer in den Ruhezustand wechselt, kann das Gateway nicht auf Datenanforderungen reagieren. Aus diesem Grund sollten Sie vor der Installation des Gateways einen entsprechenden **Energiesparplan** auf dem Computer konfigurieren. Bei der Gatewayinstallation wird eine Meldung angezeigt, wenn der Computer für den Ruhezustand konfiguriert ist.
- Sie müssen ein Administrator auf dem Computer sein, um das Datenverwaltungsgateway erfolgreich installieren und konfigurieren zu können. Sie können der lokalen Windows-Gruppe **Datenverwaltungsgateway-Benutzer** zusätzliche Benutzer hinzufügen. Die Mitglieder dieser Gruppe können das Gateway mithilfe des Datenverwaltungsgateway-Konfigurations-Managers konfigurieren.

Da die Kopieraktivität mit einer bestimmten Häufigkeit ausgeführt wird, folgt die Ressourcenverwendung (CPU, Arbeitsspeicher) auf dem Computer dem gleichen Muster mit Spitzen- und Leerlaufzeiten. Die Ressourcenverwendung hängt auch stark von der Datenmenge ab, die verschoben wird. Wenn mehrere Kopieraufträge in Bearbeitung sind, steigt die Ressourcenverwendung zu Spitzenzeiten an. Bei der oben angegebenen Konfiguration handelt es sich um die Mindestkonfiguration. Es empfiehlt sich jedoch immer, eine Konfiguration mit mehr Ressourcen zu verwenden. Ausschlaggebend ist hierbei jeweils Ihre spezifische Auslastung für Datenverschiebungen.

### Installationsoptionen
Das Datenverwaltungsgateway kann auf folgende Weise installiert werden:

- Durch Herunterladen eines MSI-Setuppakets aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717). Die MSI-Datei kann auch verwendet werden, um ein vorhandenes Datenverwaltungsgateway auf die neueste Version zu aktualisieren, wobei alle Einstellungen beibehalten werden.
- Durch Klicken auf den Link **Datengateway herunterladen und installieren** unter MANUELLES SETUP oder **Direkt auf diesem Computer installieren** unter EXPRESS-SETUP. Im Artikel [Verschieben von Daten zwischen lokalen Quellen und der Cloud](data-factory-move-data-between-onprem-and-cloud.md) finden Sie schrittweise Anweisungen zur Verwendung des Express-Setups. Durch den manuellen Schritt gelangen Sie zum Download Center. Wie Sie das Gateway aus dem Download Center herunterladen und installieren, erfahren Sie im nächsten Abschnitt.

### Bewährte Methoden für die Installation:
1.	Konfigurieren Sie den Energiesparplan auf dem Hostcomputer für das Gateway, sodass der Computer nicht in den Ruhezustand wechselt. Wenn sich der Hostcomputer im Ruhezustand befindet, reagiert das Gateway nicht auf Datenanforderungen.
2.	Sichern Sie das Zertifikat, das dem Gateway zugeordnet ist.

### Installieren des Gateways aus dem Download Center
1. Navigieren Sie zur [Downloadseite des Microsoft-Datenverwaltungsgateways](https://www.microsoft.com/download/details.aspx?id=39717).
2. Klicken Sie auf **Herunterladen**, wählen Sie die entsprechende Version (**32-Bit** oder **64-Bit**) aus, und klicken Sie auf **Weiter**.
3. Führen Sie **MSI** direkt aus, oder speichern Sie es zur späteren Ausführung auf Ihrer Festplatte.
4. Wählen Sie auf der Seite **Willkommen** eine **Sprache** aus, und klicken Sie auf **Weiter**.
5. **Akzeptieren** Sie die Lizenzbedingungen, und klicken Sie auf **Weiter**.
6. Wählen Sie den **Ordner** zur Installation des Gateways aus, und klicken Sie auf **Weiter**.
7. Klicken Sie auf der Seite **Bereit zur Installation** auf **Installieren**.
8. Klicken Sie auf **Fertig stellen**, um die Installation abzuschließen.
9. Ermitteln Sie den Schlüssel im Azure-Portal. Im nächsten Abschnitt finden Sie schrittweise Anweisungen.
10. Fügen Sie den Schlüssel in das Textfeld auf der Seite **Gateway registrieren** des **Datenverwaltungsgateway-Konfigurations-Managers** auf Ihrem Computer ein. Optional: Klicken Sie auf **Gatewayschlüssel anzeigen**, um den Text des Schlüssels einzublenden, und dann auf **Registrieren**.

### Registrieren des Gateways mit dem Schlüssel

#### Wenn Sie nicht bereits ein logisches Gateway im Portal erstellt haben
Führen Sie die Schritte der exemplarischen Vorgehensweise aus dem Artikel [Verschieben von Daten zwischen lokalen Quellen und der Cloud](data-factory-move-data-between-onprem-and-cloud.md) aus, um im Portal ein Gateway zu erstellen, und ermitteln Sie den Schlüssel auf dem Blatt **Konfigurieren**.

#### Wenn Sie bereits ein logisches Gateway im Portal erstellt haben
1. Wechseln Sie im Azure-Portal zum Blatt **Data Factory**, und klicken Sie auf die Kachel **Verknüpfte Dienste**.

	![Blatt "Data Factory"](media/data-factory-data-management-gateway/data-factory-blade.png)
2. Wählen Sie auf dem Blatt **Verknüpfte Dienste** das logische **Gateway** aus, das Sie im Portal erstellt haben.

	![logisches Gateway](media/data-factory-data-management-gateway/data-factory-select-gateway.png)
2. Klicken Sie auf dem Blatt **Datengateway** auf **Datengateway herunterladen und installieren**.

	![Link zum Herunterladen im Portal](media/data-factory-data-management-gateway/download-and-install-link-on-portal.png)
3. Klicken Sie auf dem Blatt **Konfigurieren** auf **Schlüssel neu erstellen**. Klicken Sie in der Warnmeldung auf „Ja“, nachdem Sie sie sorgfältig gelesen haben.

	![Schlüssel neu erstellen](media/data-factory-data-management-gateway/recreate-key-button.png)
4. Klicken Sie auf die Schaltfläche „Kopieren“ neben dem Schlüssel, um ihn in die Zwischenablage zu kopieren.
	
	![Kopieren des Schlüssels](media/data-factory-data-management-gateway/copy-gateway-key.png)

### Symbole und Benachrichtigungen in der Taskleiste
Die folgende Abbildung enthält einige Symbole, die auf der Taskleiste angezeigt werden.

![Taskleistensymbole](./media/data-factory-data-management-gateway/gateway-tray-icons.png)

Wenn Sie den Cursor auf der Taskleiste auf das Symbol oder die Benachrichtigung bewegen, werden in einem Popupfenster Details zum Zustand des Gateways bzw. des Aktualisierungsvorgangs angezeigt.

### Ports und Firewall
Es gibt zwei Firewalls, die berücksichtigt werden müssen: Die **Unternehmensfirewall**, die auf dem zentralen Router des Unternehmens ausgeführt wird, und die **Windows-Firewall**, die als Daemon auf dem lokalen Computer konfiguriert ist, auf dem das Gateway installiert ist.

![Firewalls](./media/data-factory-data-management-gateway/firewalls.png)

Auf Ebene der Unternehmensfirewall müssen Sie die folgenden Domänen und ausgehenden Ports konfigurieren:

| Domänennamen | Ports | Beschreibung |
| ------ | --------- | ------------ |
| *.servicebus.windows.net | 443, 80 | Listener auf Service Bus Relay per TCP (443 für Access Control-Tokenbeschaffung erforderlich) | 
| *.servicebus.windows.net | 9350-9354, 5671 | Optionales Service Bus Relay per TCP | 
| *.core.windows.net | 443 | HTTPS | 
| *.clouddatahub.net | 443 | HTTPS | 
| graph.windows.net | 443 | HTTPS |
| login.windows.net | 443 | HTTPS | 

Auf Ebene der Windows-Firewall sind diese ausgehenden Ports normalerweise aktiviert. Falls nicht, können Sie die Domänen und Ports auf dem Gatewaycomputer entsprechend konfigurieren.

#### So kopieren Sie Daten aus einem Quelldatenspeicher in einen Senkendatenspeicher

Sie müssen sicherstellen, dass die Firewallregeln für die Unternehmensfirewall, die Windows-Firewall auf dem Gatewaycomputer und den Datenspeicher selbst richtig aktiviert sind. So kann das Gateway sowohl mit der Quelle als auch mit der Senke erfolgreich eine Verbindung herstellen. Sie müssen Regeln für jeden Datenspeicher aktivieren, der am Kopiervorgang beteiligt ist.

Zum Kopieren aus **einem lokalen Datenspeicher in eine Azure SQL-Datenbanksenke oder eine Azure SQL Data Warehouse-Senke** müssen Sie beispielsweise für die Windows-Firewall und die Unternehmensfirewall die ausgehende **TCP**-Kommunikation über Port **1433** zulassen. Außerdem müssen Sie die Firewalleinstellungen des Azure SQL-Servers konfigurieren, um die IP-Adresse des Gatewaycomputers der Liste mit den zulässigen IP-Adressen hinzuzufügen.


### Proxyservereinstellungen
Standardmäßig greift das Datenverwaltungsgateway auf die Proxyeinstellungen von Internet Explorer zurück und verwendet Standardanmeldeinformationen für den Zugriff darauf. Falls dies für Sie nicht geeignet ist, können Sie die **Proxyservereinstellungen** wie unten gezeigt weiter konfigurieren, um sicherzustellen, dass das Gateway eine Verbindung mit Azure Data Factory herstellen kann:

1.	Erstellen Sie nach dem Installieren des Datenverwaltungsgateways im Datei-Explorer eine sichere Kopie von „C:\\Programme\\Microsoft Data Management Gateway\\1.0\\Shared\\diahost.exe.config“, um die Originaldatei zu sichern.
2.	Starten Sie „Notepad.exe“ als Administrator, und öffnen Sie die Textdatei „C:\\Programme\\Microsoft Data Management Gateway\\1.0\\Shared\\diahost.exe.config“. Sie finden das Standardtag für „system.net“ wie folgt:

			<system.net>
				<defaultProxy useDefaultCredentials="true" />
			</system.net>	

	Anschließend können Sie die Proxyserverdetails (etwa die Proxyadresse) in diesem übergeordneten Tag hinzufügen. Beispiel:

			<system.net>
			      <defaultProxy enabled="true">
			            <proxy bypassonlocal="true" proxyaddress="http://proxy.domain.org:8888/" />
			      </defaultProxy>
			</system.net>

	Zusätzliche Eigenschaften sind im Proxytag zulässig, um die erforderlichen Einstellungen anzugeben, z. B. scriptLocation. Informationen zur Syntax finden Sie unter [<proxy>-Element (Netzwerkeinstellungen)](https://msdn.microsoft.com/library/sa91de1e.aspx).

			<proxy autoDetect="true|false|unspecified" bypassonlocal="true|false|unspecified" proxyaddress="uriString" scriptLocation="uriString" usesystemdefault="true|false|unspecified "/>

3. Speichern Sie die Konfigurationsdatei am ursprünglichen Speicherort, und starten Sie den Datenverwaltungsgateway-Dienst, um die Änderungen zu übernehmen. Dies ist über **Start** > **Services.msc** möglich, oder klicken Sie im **Datenverwaltungsgateway-Konfigurations-Manager** auf die Schaltfläche **Dienst beenden** und anschließend auf **Dienst starten**. Wenn der Dienst nicht gestartet wird, wurde der bearbeiteten Anwendungskonfigurationsdatei wahrscheinlich eine falsche XML-Tagsyntax hinzugefügt.

Zusätzlich zu den obigen Punkten müssen Sie auch sicherstellen, dass Microsoft Azure in der Positivliste Ihres Unternehmens aufgeführt ist. Sie können die Liste mit den gültigen Microsoft Azure-IP-Adressen im [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=41653) herunterladen.

#### Mögliche Symptome für Probleme im Zusammenhang mit der Firewall und dem Proxyserver
Wenn Sie auf die unten aufgeführten Fehler treffen, liegt dies meist an der unsauberen Konfiguration der Firewall oder des Proxyservers, da für das Datenverwaltungsgateway die Verbindungsherstellung mit Azure Data Factory für die Authentifizierung blockiert wird. Verwenden Sie den obigen Abschnitt, um sicherzustellen, dass die Firewall und der Proxyserver richtig konfiguriert sind.

1.	Wenn Sie versuchen, das Gateway zu registrieren, wird die folgende Fehlermeldung angezeigt: "Fehler beim Registrieren des Gatewayschlüssels. Prüfen Sie, ob sich das Datenverwaltungsgateway im Status "Verbunden" befindet und der Datenverwaltungsgateway-Hostdienst gestartet wurde, bevor Sie versuchen, den Gatewayschlüssel erneut zu registrieren."
2.	Wenn Sie den Konfigurations-Manager öffnen, wird der Status als "Getrennt" oder "Verbindung wird hergestellt" angezeigt. Beim Anzeigen der Windows-Ereignisprotokolle unter „Ereignisanzeige“ > „Anwendungs- und Dienstprotokolle“ > „Datenverwaltungsgateway“ werden Fehlermeldungen wie „Die Verbindung mit dem Remoteserver kann nicht hergestellt werden.“ oder „Eine Komponente des Datenverwaltungsgateways reagiert nicht mehr und wird automatisch neu gestartet. Komponentenname: Gateway." angezeigt.

### Öffnen von Port 8050 für die Verschlüsselung der Anmeldeinformationen 
Der eingehende Port **8050** wird von der Anwendung **Anmeldeinformationen festlegen** genutzt, um die Anmeldeinformationen an das Gateway weiterzugeben, wenn Sie im Azure-Portal einen lokalen verknüpften Dienst einrichten. (Ausführlichere Informationen folgen weiter unten in diesem Artikel.) Während der Einrichtung des Gateways wird es von der Datenverwaltungsgateway-Installation standardmäßig auf dem Gatewaycomputer geöffnet.
 
Bei Verwendung einer Drittanbieterfirewall können Sie den Port 8050 manuell öffnen. Falls beim Einrichten des Gateways ein Firewallproblem auftritt, können Sie versuchen, den folgenden Befehl zum Installieren des Gateways ohne Konfiguration der Firewall zu verwenden.

	msiexec /q /i DataManagementGateway.msi NOFIREWALL=1

Falls Sie den Port 8050 auf dem Gatewaycomputer nicht öffnen, müssen Sie zum Einrichten eines lokalen verknüpften Diensts andere Verfahren als die Anwendung **Anmeldeinformationen festlegen** nutzen, um die Anmeldeinformationen für den Datenspeicher zu konfigurieren. Beispielsweise können Sie das PowerShell-Cmdlet [New-AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) verwenden. Informationen zum Festlegen von Datenspeicher-Anmeldeinformationen finden Sie unter [Festlegen von Anmeldeinformationen und Sicherheit](#set-credentials-and-securityy).

## Aktualisieren 
Standardmäßig wird das Datenverwaltungsgateway automatisch aktualisiert, wenn eine neuere Version des Gateways verfügbar ist. Das Gateway wird erst aktualisiert, nachdem alle geplanten Aufgaben erledigt wurden. Vom Gateway werden keine weiteren Aufgaben verarbeitet, bevor der Aktualisierungsvorgang abgeschlossen ist. Wenn die Aktualisierung fehlschlägt, wird für das Gateway ein Rollback auf die alte Version durchgeführt.

Die geplante Aktualisierungszeit wird im Portal auf dem Blatt mit den Gatewayeigenschaften, auf der Startseite des Datenverwaltungsgateway-Konfigurations-Managers und in der Benachrichtigung auf der Taskleiste angezeigt.

Auf der Registerkarte „Startseite“ des Datenverwaltungsgateway-Konfigurations-Managers werden der Aktualisierungszeitplan und der Zeitpunkt der letzten Installation bzw. Aktualisierung des Gateways angezeigt.

![Planen von Updates](media/data-factory-data-management-gateway/UpdateSection.png)

Sie können das Update sofort installieren oder warten, bis das Gateway zum geplanten Zeitpunkt automatisch aktualisiert wird. Der folgende Screenshot zeigt beispielsweise die Benachrichtigung, die im Datenverwaltungsgateway-Konfigurations-Manager zusammen mit der Schaltfläche „Aktualisieren“ angezeigt wird, auf die Sie zum sofortigen Installieren klicken.

![Update im Datenverwaltungsgateway-Konfigurations-Manager](./media/data-factory-data-management-gateway/gateway-auto-update-config-manager.png)

Die Benachrichtigung in der Taskleiste sieht wie folgt aus:

![Meldung in der Taskleiste](./media/data-factory-data-management-gateway/gateway-auto-update-tray-message.png)

Der Status des Aktualisierungsvorgangs (manuell oder automatisch) wird auf der Taskleiste angezeigt. Wenn Sie den Datenverwaltungsgateway-Konfigurations-Manager das nächste Mal öffnen, wird auf der Benachrichtigungsleiste eine Meldung mit dem Hinweis eingeblendet, dass das Gateway aktualisiert wurde. Außerdem ist ein Link zu [Neuigkeiten](data-factory-gateway-release-notes.md) angegeben.

### So deaktivieren/aktivieren Sie das Feature für die automatische Aktualisierung
Sie können das Feature für die automatische Aktualisierung wie folgt deaktivieren und aktivieren:

1. Starten Sie Windows PowerShell auf dem Gatewaycomputer.
2. Wechseln Sie in den Ordner „C:\\Programme\\Microsoft Data Management Gateway\\1.0\\PowerShellScript“.
3. Führen Sie den folgenden Befehl aus, um das Feature für die automatische Aktualisierung zu deaktivieren.

		.\GatewayAutoUpdateToggle.ps1  -off

4. So aktivieren Sie das Feature wieder
	
		.\GatewayAutoUpdateToggle.ps1  -on  

## Konfigurations-Manager 
Nach der Installation des Gateways können Sie den Datenverwaltungsgateway-Konfigurations-Manager auf eine der folgenden Arten starten:

- Geben Sie im Fenster **Suchen** den Begriff **Datenverwaltungsgateway** ein, um auf dieses Hilfsprogramm zuzugreifen.
- Führen Sie die Datei **ConfigManager.exe** im Ordner **C:\\Programme\\Microsoft Data Management Gateway\\1.0\\Shared** aus.
 
### Startseite
Auf der Startseite können Sie die folgenden Schritte ausführen:

- Anzeigen des Gatewaystatus (mit Clouddienst verbunden oder Ähnliches)
- **Registrieren** mithilfe eines Schlüssels aus dem Portal
- **Beenden** und Starten des **Datenverwaltungsgateway-Hostdiensts** auf dem Gatewaycomputer
- **Planen von Updates** zu einem bestimmten Zeitpunkt an den Tagen
- Anzeigen des Datums, zu dem das Gateway **zuletzt aktualisiert** wurde

### Seite "Einstellungen"
Auf der Seite „Einstellungen“ können Sie die folgenden Schritte ausführen:

- Anzeigen, Ändern und Exportieren des **Zertifikats**, das vom Gateway verwendet wird Dieses Zertifikat dient zum Verschlüsseln von Anmeldeinformationen für Datenquellen.
- Ändern des **HTTPS-Ports** für den Endpunkt Das Gateway öffnet einen Port für die Festlegung der Datenquellen-Anmeldeinformationen.
- **Status** des Endpunkts
- **SSL-Zertifikat anzeigen** wird für die SSL-Kommunikation zwischen Portal und Gateway zum Festlegen von Anmeldeinformationen für Datenquellen verwendet.

### Seite „Diagnose“
Auf der Seite „Diagnose“ können Sie die folgenden Schritte ausführen:

- Aktivieren der ausführlichen **Protokollierung**, Anzeigen von Protokollen in der Ereignisanzeige und Senden von Protokollen an Microsoft, falls ein Fehler aufgetreten ist
- **Testen der Verbindung** mit einer Datenquelle

### Hilfeseite
Auf der Seite „Hilfe“ werden die folgenden Informationen angezeigt:

- Kurzbeschreibung des Gateways.
- Versionsnummer
- Links zu Onlinehilfe, Datenschutzbestimmungen und Lizenzvertrag

## Problembehandlung

- Ausführliche Informationen hierzu finden Sie in den Gatewayprotokollen und Windows-Ereignisprotokollen. Sie befinden sich in der **Ereignisanzeige** von Windows unter **Anwendungs- und Dienstprotokolle** > **Datenverwaltungsgateway**. Suchen Sie bei der Behandlung von Problemen mit Gateways in der Ereignisanzeige nach Fehlerereignissen.
- Falls das Gateway nach dem **Ändern des Zertifikats** nicht mehr funktioniert, starten Sie den **Datenverwaltungsgateway-Dienst** neu (beenden und starten), indem Sie das Tool Microsoft-Datenverwaltungsgateway-Konfigurations-Manager oder das Applet Dienste in der Systemsteuerung verwenden. Falls weiterhin ein Fehler angezeigt wird, müssen Sie für Benutzer des Datenverwaltungsgateway-Diensts unter Umständen explizite Berechtigungen erteilen, damit sie auf das Zertifikat im Zertifikat-Manager (certmgr.msc) zugreifen können. Das Standardbenutzerkonto für den Dienst lautet **NT Service\\DIAHostService**.
- Wenn die Anwendung **Anmeldeinformationsverwaltung** beim Klicken auf die Schaltfläche „Verschlüsseln“ im Data Factory-Editor die Anmeldeinformationen nicht **verschlüsselt**, vergewissern Sie sich, dass die Anwendung auf dem Computer ausgeführt wird, auf dem das Gateway installiert ist. Falls nicht, führen Sie die Anwendung auf dem Gatewaycomputer aus, und versuchen Sie, die Anmeldeinformationen zu verschlüsseln.
- Wenn Fehler in Bezug auf die Datenspeicherverbindung oder Treiber auftreten, starten Sie auf dem Gatewaycomputer den **Datenverwaltungsgateway-Konfigurations-Manager**. Wechseln Sie zur Registerkarte **Diagnose**, auf der Sie entsprechende Werte für die Felder in der Gruppe **Testen Sie die Verbindung mit einer lokalen Datenquelle mithilfe dieses Gateways** auswählen bzw. eingeben. Klicken Sie anschließend auf **Verbindung testen**, um zu prüfen, ob Sie auf dem Gatewaycomputer mithilfe der Verbindungs- und Anmeldeinformationen eine Verbindung mit einer lokalen Datenquelle herstellen können. Wenn das Testen der Verbindung weiter misslingt, nachdem Sie einen Treiber installiert haben, starten Sie das Gateway neu, damit es die letzte Änderung übernimmt.

	![Verbindung testen](./media/data-factory-data-management-gateway/TestConnection.png)

### Senden der Gatewayprotokolle an Microsoft
Falls Sie sich im Zusammenhang mit Gatewayproblemen an den Microsoft Support wenden müssen, werden Sie möglicherweise aufgefordert, Ihre Gatewayprotokolle freizugeben. In dieser Version des Gateways können Sie die erforderlichen Gatewayprotokolle komfortabel mit nur zwei Mausklicks im Gateway-Konfigurations-Manager freigeben.

1. Wechseln Sie zur Registerkarte **Diagnose** im Gateway-Konfigurations-Manager.
 
	![Datenverwaltungsgateway – Registerkarte „Diagnose“](media/data-factory-data-management-gateway/data-management-gateway-diagnostics-tab.png)
2. Klicken Sie auf den Link **Protokolle senden**, um das folgende Dialogfeld anzuzeigen.

	![Datenverwaltungsgateway – Senden von Protokollen](media/data-factory-data-management-gateway/data-management-gateway-send-logs-dialog.png)
3. (optional) Klicken Sie auf **Protokolle anzeigen**, um Protokolle in der Ereignisanzeige durchzusehen.
4. (optional) Klicken Sie auf **Datenschutz**, um die Datenschutzbestimmungen der Onlinedienste von Microsoft anzuzeigen.
3. Wenn Sie zum Hochladen bereit sind, klicken Sie auf **Protokolle senden**, um Protokolle der letzten sieben Tage zur Problembehandlung an Microsoft zu senden. Daraufhin sollte der Status des Vorgangs „Protokolle senden“ wie unten dargestellt angezeigt werden.

	![Datenverwaltungsgateway – Senden des Status von Protokollen](media/data-factory-data-management-gateway/data-management-gateway-send-logs-status.png)
4. Nach Abschluss des Vorgangs erscheint ein Dialogfeld wie das folgende:
	
	![Datenverwaltungsgateway – Senden des Status von Protokollen](media/data-factory-data-management-gateway/data-management-gateway-send-logs-result.png)
5. Notieren Sie sich die **Berichts-ID**, und teilen Sie sie dem Microsoft Support mit. Die Berichts-ID wird verwendet, um Ihre Gatewayprotokolle zu suchen, die Sie zur Problembehandlung hochgeladen haben. Die Berichts-ID wird auch in der Ereignisanzeige gespeichert, damit Sie darauf zugreifen können. Sie finden Sie anhand der Ereignis-ID „25“ sowie nach Datum und Uhrzeit.
	
	![Datenverwaltungsgateway – Senden der Berichts-ID von Protokollen](media/data-factory-data-management-gateway/data-management-gateway-send-logs-report-id.png)

### Archivieren von Gatewayprotokollen auf dem Gatewayhostcomputer
Es gibt einige Szenarien, in denen Gatewayprobleme auftreten, und Sie Gatewayprotokolle nicht direkt freigeben können:

- wenn Benutzer das Gateway manuell installieren und registrieren;
- wenn Benutzer versuchen, das Gateway mit einem neu generierten Schlüssel im Konfigurations-Manager zu registrieren;
- wenn Benutzer versuchen, Protokolle zu senden, aber keine Verbindung mit dem Gatewayhostdienst hergestellt werden kann.

In solchen Fällen können Sie Gatewayprotokolle als ZIP-Datei speichern und später beim Kontakt mit dem Microsoft Support freigeben. Wenn Sie z.B. beim Registrieren des Gateways eine Fehlermeldung wie unten dargestellt erhalten:

![Datenverwaltungsgateway – Fehler bei der Registrierung](media/data-factory-data-management-gateway/data-management-gateway-registration-error.png)

Klicken Sie auf den Link **Gatewayprotokolle archivieren**, um Protokolle zu archivieren und zu speichern und dann die ZIP-Datei für den Microsoft Support freizugeben.

![Datenverwaltungsgateway – Archivieren von Protokollen](media/data-factory-data-management-gateway/data-management-gateway-archive-logs.png)

 
## Verschieben eines Gateways von einem Computer auf einen anderen
Dieser Abschnitt enthält Anweisungen zum Verschieben des Gatewayclients von einem Computer auf einen anderen.

2. Wechseln Sie im Portal zu **Data Factory home page**, und klicken Sie auf die Kachel **Verknüpfte Dienste**.

	![Link „Datengateways“](./media/data-factory-data-management-gateway/DataGatewaysLink.png)
3. Wählen Sie das Gateway im Abschnitt **DATENGATEWAYS** des Blatts **Verknüpfte Dienste** aus.
	
	![Blatt „Verknüpfte Dienste“ mit ausgewähltem Gateway](./media/data-factory-data-management-gateway/LinkedServiceBladeWithGateway.png)
4. Klicken Sie auf dem Blatt **Datengateway** auf **Datengateway herunterladen und installieren**.
	
	![Link „Gateway herunterladen“](./media/data-factory-data-management-gateway/DownloadGatewayLink.png)
5. Klicken Sie auf dem Blatt **Konfigurieren** auf **Datengateway herunterladen und installieren**, und befolgen Sie die Anweisungen, um das Datengateway auf dem Computer zu installieren.

	![Blatt „Konfigurieren“](./media/data-factory-data-management-gateway/ConfigureBlade.png)
6. Lassen Sie den **Microsoft-Datenverwaltungsgateway-Konfigurations-Manager** geöffnet.
 
	![Konfigurations-Manager](./media/data-factory-data-management-gateway/ConfigurationManager.png)
7. Klicken Sie im Portal auf dem Blatt **Konfigurieren** auf der Befehlsleiste auf **Schlüssel neu erstellen** und anschließend in der Warnmeldung auf **Ja**. Klicken Sie auf die Schaltfläche **Kopieren** neben dem Schlüsseltext, um den Schlüssel in die Zwischenablage zu kopieren. Sobald Sie den Schlüssel neu erstellen, funktioniert das Gateway auf dem alten Computer nicht mehr.
	
	![Schlüssel neu erstellen](./media/data-factory-data-management-gateway/RecreateKey.png)
	 
8. Fügen Sie den **Schlüssel** in das Textfeld auf der Seite **Gateway registrieren** des Datenverwaltungsgateway-Konfigurations-Managers auf Ihrem Computer ein. (Optional:) Aktivieren Sie das Kontrollkästchen **Gatewayschlüssel anzeigen**, um den Text des Schlüssels einzublenden.
 
	![Schlüssel kopieren und registrieren](./media/data-factory-data-management-gateway/CopyKeyAndRegister.png)
9. Klicken Sie auf **Registrieren**, um das Gateway beim Clouddienst zu registrieren.
10. Klicken Sie auf der Registerkarte **Einstellungen** auf **Ändern**, um das gleiche Zertifikat auszuwählen, das mit dem alten Gateway verwendet wurde, geben Sie das **Kennwort** ein, und klicken Sie auf **Fertig stellen**.
 
	![Zertifikat angeben](./media/data-factory-data-management-gateway/SpecifyCertificate.png)

	Ein Zertifikat des alten Gateways können Sie wie folgt exportieren: Starten Sie auf dem alten Computer den Datenverwaltungsgateway-Konfigurations-Manager, wechseln Sie zur Registerkarte **Zertifikat**, klicken Sie auf **Exportieren**, und befolgen Sie die Anweisungen.
10. Nach erfolgreicher Registrierung des Gateways sollten auf der Startseite des Gateway-Konfigurations-Managers **Registrierung** auf **Registriert** und die Option **Status** auf **Gestartet** festgelegt sein.

## Verschlüsseln der Anmeldeinformationen 
Zum Verschlüsseln der Anmeldeinformationen im Data Factory-Editor gehen Sie wie folgt vor:

1. Starten Sie den Webbrowser auf dem **Gatewaycomputer**, navigieren Sie zum [Azure-Portal](http://portal.azure.com), suchen Sie nach Ihrer Data Factory (falls erforderlich), und öffnen Sie die Data Factory auf dem Blatt **DATA FACTORY**. Klicken Sie anschließend auf **Verfassen und bereitstellen**, um den Data Factory-Editor zu starten.
1. Klicken Sie in der Strukturansicht auf einen vorhandenen **verknüpften Dienst**, um dessen JSON-Definition anzuzeigen, oder erstellen Sie einen verknüpften Dienst, der ein Datenverwaltungsgateway erfordert (etwa SQL Server oder Oracle).
2. Geben Sie im JSON-Editor für die **gatewayName**-Eigenschaft den Namen des Gateways ein.
3. Geben Sie den Servernamen für die **Data Source**-Eigenschaft in **connectionString** ein.
4. Geben Sie den Datenbanknamen für die **Initial Catalog**-Eigenschaft in **connectionString** ein.
5. Klicken Sie auf der Befehlsleiste auf **Verschlüsseln**, um die ClickOnce-Anwendung **Anmeldeinformationsverwaltung** zu starten. Das Dialogfeld **Anmeldeinformationen festlegen** wird angezeigt. ![Dialogfeld "Anmeldeinformationen festlegen"](./media/data-factory-data-management-gateway/setting-credentials-dialog.png)
6. Gehen Sie im Dialogfeld **Anmeldeinformationen festlegen** folgendermaßen vor:
	1.	Wählen Sie die **Authentifizierung** aus, die der Data Factory-Dienst für die Verbindung mit der Datenbank verwenden soll.
	2.	Geben Sie für die Einstellung **BENUTZERNAME** den Namen des Benutzers ein, der auf die Datenbank zugreifen kann.
	3.	Geben Sie für die Einstellung **KENNWORT** das Kennwort für den Benutzer ein.
	4.	Klicken Sie auf **OK**, um Anmeldeinformationen zu verschlüsseln, und schließen Sie das Dialogfeld.
5.	Sie sollten nun eine **encryptedCredential**-Eigenschaft in **connectionString** sehen.
		
			{
	    		"name": "SqlServerLinkedService",
		    	"properties": {
		        	"type": "OnPremisesSqlServer",
			        "description": "",
		    	    "typeProperties": {
		    	        "connectionString": "data source=myserver;initial catalog=mydatabase;Integrated Security=False;EncryptedCredential=eyJDb25uZWN0aW9uU3R",
		            	"gatewayName": "adftutorialgateway"
		        	}
		    	}
			}

Wenn Sie auf das Portal auf einem Computer zugreifen, der sich vom Computer mit dem Gateway unterscheidet, müssen Sie sicherstellen, dass die Anwendung "Anmeldeinformations-Manager" eine Verbindung mit dem Gatewaycomputer herstellen kann. Falls die Anwendung den Computer mit dem Gateway nicht erreichen kann, können Sie keine Anmeldeinformationen für die Datenquelle festlegen und die Verbindung mit der Datenquelle nicht testen.

Wenn Sie die über das Azure-Portal gestartete Anwendung **Anmeldeinformationen festlegen** verwenden, um Anmeldeinformationen für eine lokale Datenquelle festzulegen, verschlüsselt das Portal die Anmeldeinformationen mit dem Zertifikat, das Sie auf der Registerkarte **Zertifikat** des **Datenverwaltungsgateway-Konfigurations-Managers** auf dem Gatewaycomputer angegeben haben.

Wenn Sie einen API-basierten Ansatz zum Verschlüsseln der Anmeldeinformationen nutzen möchten, können Sie das PowerShell-Cmdlet [New-AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) verwenden, um Anmeldeinformationen zu verschlüsseln. Das Cmdlet verwendet das Zertifikat, für dessen Verwendung dieses Gateway konfiguriert ist, um die Anmeldeinformationen zu verschlüsseln. Sie können die verschlüsselten Anmeldeinformationen dem **EncryptedCredential**-Element der Verbindungszeichenfolge (**connectionString**) in der JSON-Datei hinzufügen, die Sie mit dem Cmdlet [New-AzureRmDataFactoryLinkedService](https://msdn.microsoft.com/library/mt603647.aspx) verwenden, oder sie im Portal mithilfe des Data Factory-Editors in den JSON-Ausschnitt einfügen.

	"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",

Es gibt eine weitere Möglichkeit zum Festlegen der Anmeldeinformationen mit dem Data Factory-Editor. Wenn Sie einen verknüpften SQL Server-Dienst mit dem Editor erstellen und Anmeldeinformationen im Nur-Text-Format eingeben, werden die Anmeldeinformationen mit einem Zertifikat verschlüsselt, das der Data Factory-Dienst besitzt, NICHT mit dem Zertifikat, für dessen Verwendung dieses Gateway konfiguriert ist. Dieser Ansatz ist möglicherweise etwas schneller, in einigen Fällen aber weniger sicher. Daher empfehlen wir, dass Sie diesen Ansatz nur für die Entwicklung/Tests verwenden.


## PowerShell-Cmdlets 
Dieser Abschnitt beschreibt das Erstellen und Registrieren eines Gateways mit Azure PowerShell-Cmdlets.

1. Starten Sie **Azure PowerShell** im Administratormodus.
2. Melden Sie sich bei Ihrem Azure-Konto an, indem Sie den folgenden Befehl ausführen und Ihre Azure-Anmeldeinformationen eingeben.

	Login-AzureRmAccount
2. Verwenden Sie das **New-AzureRmDataFactoryGateway**-Cmdlet, um wie folgt ein logisches Gateway zu erstellen:

		$MyDMG = New-AzureRmDataFactoryGateway -Name <gatewayName> -DataFactoryName <dataFactoryName> -ResourceGroupName ADF –Description <desc>

	**Beispiel für eine Befehlszeile und Ausgabe**:


		PS C:\> $MyDMG = New-AzureRmDataFactoryGateway -Name MyGateway -DataFactoryName $df -ResourceGroupName ADF –Description “gateway for walkthrough”

		Name              : MyGateway
		Description       : gateway for walkthrough
		Version           :
		Status            : NeedRegistration
		VersionStatus     : None
		CreateTime        : 9/28/2014 10:58:22
		RegisterTime      :
		LastConnectTime   :
		ExpiryTime        :
		ProvisioningState : Succeeded
		Key               : ADF#00000000-0000-4fb8-a867-947877aef6cb@fda06d87-f446-43b1-9485-78af26b8bab0@4707262b-dc25-4fe5-881c-c8a7c3c569fe@wu#nfU4aBlq/heRyYFZ2Xt/CD+7i73PEO521Sj2AFOCmiI

	
4. Wechseln Sie in Azure PowerShell zum Ordner **C:\\Programme\\Microsoft Data Management Gateway\\1.0\\PowerShellScript**, und führen Sie das Skript **RegisterGateway.ps1** aus, das mit der lokalen Variablen **$Key** verknüpft ist, wie im folgenden Befehl dargestellt. Sie registrieren damit den Client-Agent auf dem Computer mit dem logischen Gateway, das Sie zuvor erstellt haben.

		PS C:\> .\RegisterGateway.ps1 $MyDMG.Key
		
		Agent registration is successful!

	Sie können das Gateway auf einem Remotecomputer registrieren, indem Sie den Parameter „IsRegisterOnRemoteMachine“ verwenden. Beispiel:
		
		.\RegisterGateway.ps1 $MyDMG.Key -IsRegisterOnRemoteMachine true

5. Mit dem **Get-AzureRmDataFactoryGateway**-Cmdlet können Sie die Liste mit Gateways in Ihrer Data Factory abrufen. Wenn **online** als **Status** angezeigt wird, bedeutet dies, dass Ihr Gateway einsatzbereit ist.

		Get-AzureRmDataFactoryGateway -DataFactoryName <dataFactoryName> -ResourceGroupName ADF

Sie können ein Gateway mit dem **Remove-AzureRmDataFactoryGateway**-Cmdlet entfernen und die Beschreibung für das Gateway mithilfe der **Set-AzureRmDataFactoryGateway**-Cmdlets aktualisieren. Syntax und andere Details zu diesen Cmdlets finden Sie in der Data Factory-Cmdlet-Referenz.

### Auflisten von Gateways mit PowerShell

	Get-AzureRmDataFactoryGateway -DataFactoryName jasoncopyusingstoredprocedure -ResourceGroupName ADF_ResourceGroup

### Entfernen von Gateways mit PowerShell
	
	Remove-AzureRmDataFactoryGateway -Name JasonHDMG_byPSRemote -ResourceGroupName ADF_ResourceGroup -DataFactoryName jasoncopyusingstoredprocedure -Force 


## Nächste Schritte
- Ausführliche Informationen zum Gateway finden Sie im Artikel [Datenverwaltungsgateway](data-factory-data-management-gateway.md).

<!---HONumber=AcomDC_0824_2016-->