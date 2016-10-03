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
	ms.date="08/30/2016" 
	ms.author="spelluru"/>

# Gateway zur Datenverwaltung
Das Datenverwaltungsgateway ist ein Client-Agent, den Sie in Ihrer lokalen Umgebung installieren müssen, um Daten zwischen Ihren Cloudspeichern und lokalen Datenspeichern kopieren zu können. Die lokalen von Data Factory unterstützten Datenspeicher werden im Abschnitt [Unterstützte Datenquellen](data-factory-data-movement-activities.md##supported-data-stores) aufgeführt.

Dieser Artikel ergänzt die exemplarische Vorgehensweise im Artikel [Verschieben von Daten zwischen lokalen Datenspeichern und Clouddatenspeichern](data-factory-move-data-between-onprem-and-cloud.md). In der exemplarischen Vorgehensweise erstellen Sie eine Pipeline, die das Gateway verwendet, um Daten aus einer lokalen SQL Server-Datenbank in einen Azure-Blob zu verschieben. Dieser Artikel bietet ausführliche Informationen zur Verwendung des Datenverwaltungsgateways.

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
Wenn Sie Daten mithilfe einer Kopieraktivität zwischen der lokalen Umgebung und der Cloud kopieren, verwendet diese Aktivität ein Gateway, um die Daten aus einer lokalen Datenquelle in die Cloud und umgekehrt zu übertragen.

Im Anschluss finden Sie eine Darstellung des allgemeinen Datenflusses sowie eine Zusammenfassung der Schritte für das Kopieren mit dem Datengateway: ![Datenfluss über ein Gateway](./media/data-factory-data-management-gateway/data-flow-using-gateway.png)

1.	Der Datenentwickler erstellt entweder über das [Azure-Portal](https://portal.azure.com) oder mit einem [PowerShell-Cmdlet](https://msdn.microsoft.com/library/dn820234.aspx) ein Gateway für Azure Data Factory.
2.	Der Datenentwickler erstellt durch Angabe des Gateways einen verknüpften Dienst für einen lokalen Datenspeicher. Als Teil der Einrichtung des verknüpften Diensts verwendet der Datenentwickler die Anwendung „Anmeldeinformationen festlegen“, um Authentifizierungstypen und Anmeldeinformationen anzugeben. Die Anwendung „Anmeldeinformationen festlegen“ kommuniziert mit dem Datenspeicher, um die Verbindung zu testen, und mit dem Gateway, um die Anmeldeinformationen zu speichern.
3. Das Gateway verschlüsselt die Anmeldeinformationen mit dem Zertifikat, das dem Gateway zugeordnet wurde (bereitgestellt vom Datenentwickler), bevor die Anmeldeinformationen in der Cloud gespeichert werden.
4. Der Data Factory-Dienst kommuniziert für die Planung und Verwaltung von Aufträgen mit dem Gateway. Dazu wird ein Steuerungskanal genutzt, der eine freigegebene Azure Service Bus-Warteschlange verwendet. Wenn ein Auftrag der Kopieraktivität gestartet werden muss, reiht Data Factory die Anforderung zusammen mit Anmeldeinformationen in die Warteschlange ein. Das Gateway startet den Auftrag, nachdem die Warteschlange abgerufen wurde.
5.	Das Gateway entschlüsselt die Anmeldeinformationen mit dem gleichen Zertifikat und stellt dann mit dem richtigen Authentifizierungstyp und den Anmeldeinformationen eine Verbindung mit dem lokalen Datenspeicher her.
6.	Das Gateway kopiert Daten aus einem lokalen Speicher in einen Cloudspeicher oder umgekehrt, je nachdem, wie die Kopieraktivität in der Datenpipeline konfiguriert ist. Für diesen Schritt kommuniziert das Gateway über einen sicheren Kanal (HTTPS) direkt mit einem cloudbasierten Speicherdienst wie z.B. Azure Blob Storage.

### Überlegungen zur Verwendung des Gateways
- Ein einzelne Instanz des Datenverwaltungsgateways kann für mehrere lokale Datenquellen verwendet werden. Allerdings ist **eine einzelne Gatewayinstanz nur an eine Azure Data Factory gebunden** und kann nicht mit einer anderen Data Factory gemeinsam genutzt werden.
- Es darf **nur eine Instanz des Datenverwaltungsgateways** auf einem Computer installiert sein. Wenn Sie beispielsweise über zwei Data Factorys verfügen, die auf lokale Datenquellen zugreifen müssen, dann müssen Sie auf zwei lokalen Computern jeweils ein Gateway installieren. Anders gesagt: Jedes Gateway ist an eine spezifische Data Factory gebunden.
- Das **Gateway muss sich nicht auf dem gleichen Computer befinden wie die Datenquelle**. Wenn Sie das Gateway jedoch in der Nähe der Datenquelle platzieren, verkürzen Sie die Zeit, die das Gateway benötigt, um eine Verbindung mit der Datenquelle herzustellen. Es wird empfohlen, das Gateway auf einem anderen Computer als dem Computer zu installieren, auf dem die lokale Datenquelle gehostet wird. Wenn sich Gateway und Datenquelle auf unterschiedlichen Computern befinden, konkurriert das Gateway nicht mit der Datenquelle um Ressourcen.
- Sie können über **mehrere Gateways auf verschiedenen Computern verfügen, die eine Verbindung mit der gleichen lokalen Datenquelle herstellen**. Sie können z. B. über zwei Gateways verfügen, die zwei Data Factorys mit Daten versorgen, wobei jedoch die lokale Datenquelle für die beiden Data Factorys registriert ist.
- Falls Sie auf Ihrem Computer bereits ein Gateway installiert haben, das für ein **Power BI**-Szenario verwendet wird, installieren Sie auf einem anderen Computer ein **separates Gateway für die Azure Data Factory**.
- Sie müssen das Gateway selbst dann verwenden, wenn Sie **ExpressRoute** einsetzen.
- Behandeln Sie Ihre Datenquelle wie eine lokale Datenquelle (die sich hinter einer Firewall befindet), selbst wenn Sie **ExpressRoute** verwenden. Verwenden Sie das Gateway, um eine Verbindung zwischen dem Dienst und der Datenquelle herzustellen.
- Sie müssen auch dann **das Gateway verwenden**, wenn der Datenspeicher sich in der Cloud auf einer **Azure IaaS-VM** befindet.

## Installation

### Voraussetzungen
- Die unterstützten **Betriebssystemversionen** sind Windows 7, Windows 8/8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012 und Windows Server 2012 R2. Die Installation des Datenverwaltungsgateways auf einem Domänencontroller wird derzeit nicht unterstützt.
- Sie benötigen mindestens .NET Framework 4.5.1. Wenn Sie das Gateway auf einem Windows 7-Computer installieren, installieren Sie mindestens .NET Framework 4.5. Ausführlichere Informationen finden Sie unter [Systemanforderungen für .NET Framework](https://msdn.microsoft.com/library/8z6watww.aspx).
- Die empfohlene **Konfiguration** für den Gatewaycomputer lautet: mindestens 2 GHz, 4 Kerne, 8 GB RAM und 80 GB Festplattenspeicher.
- Wenn sich der Hostcomputer im Ruhezustand befindet, reagiert das Gateway nicht auf Datenanforderungen. Aus diesem Grund sollten Sie vor der Installation des Gateways einen entsprechenden **Energiesparplan** auf dem Computer konfigurieren. Wenn der Computer für den Ruhezustand konfiguriert ist, wird bei der Gatewayinstallation eine Meldung angezeigt.
- Sie müssen ein Administrator auf dem Computer sein, um das Datenverwaltungsgateway erfolgreich installieren und konfigurieren zu können. Sie können der lokalen Windows-Gruppe **Datenverwaltungsgateway-Benutzer** zusätzliche Benutzer hinzufügen. Die Mitglieder dieser Gruppe können das Gateway mithilfe des Datenverwaltungsgateway-Konfigurations-Managers konfigurieren.

Da die Kopieraktivität mit einer bestimmten Häufigkeit ausgeführt wird, folgt die Ressourcenverwendung (CPU, Arbeitsspeicher) auf dem Computer dem gleichen Muster mit Spitzen- und Leerlaufzeiten. Die Ressourcenverwendung hängt auch stark von der Datenmenge ab, die verschoben wird. Wenn mehrere Kopieraufträge in Bearbeitung sind, steigt die Ressourcenverwendung zu Spitzenzeiten an.

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
10. Führen Sie im **Datenverwaltungsgateway-Konfigurations-Manager**, der auf Ihrem Computer ausgeführt wird, auf der Seite **Gateway registrieren** Folgendes aus:
	1. Fügen Sie den Schlüssel in das Textfeld ein.
	2. Optional klicken Sie auf **Gatewayschlüssel anzeigen**, um den Schlüsseltext anzuzeigen.
	3. Klicken Sie auf **Registrieren**.

### Registrieren des Gateways mit dem Schlüssel

#### Wenn Sie nicht bereits ein logisches Gateway im Portal erstellt haben
Um im Portal ein Gateway zu erstellen und auf dem Blatt **Konfigurieren** den Schlüssel zu ermitteln, führen Sie die Schritte der exemplarischen Vorgehensweise aus dem Artikel [Verschieben von Daten zwischen lokalen Quellen und der Cloud](data-factory-move-data-between-onprem-and-cloud.md) aus.

#### Wenn Sie bereits ein logisches Gateway im Portal erstellt haben
1. Wechseln Sie im Azure-Portal zum Blatt **Data Factory**, und klicken Sie auf die Kachel **Verknüpfte Dienste**.

	![Blatt "Data Factory"](media/data-factory-data-management-gateway/data-factory-blade.png)
2. Wählen Sie auf dem Blatt **Verknüpfte Dienste** das logische **Gateway** aus, das Sie im Portal erstellt haben.

	![logisches Gateway](media/data-factory-data-management-gateway/data-factory-select-gateway.png)
2. Klicken Sie auf dem Blatt **Datengateway** auf **Datengateway herunterladen und installieren**.

	![Link zum Herunterladen im Portal](media/data-factory-data-management-gateway/download-and-install-link-on-portal.png)
3. Klicken Sie auf dem Blatt **Konfigurieren** auf **Schlüssel neu erstellen**. Klicken Sie in der Warnmeldung auf „Ja“, nachdem Sie sie sorgfältig gelesen haben.

	![Schlüssel neu erstellen](media/data-factory-data-management-gateway/recreate-key-button.png)
4. Klicken Sie neben dem Schlüssel auf die Schaltfläche „Kopieren“. Der Schlüssel wird in die Zwischenablage kopiert.
	
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

#### Kopieren von Daten aus einem Quelldatenspeicher in einen Senkendatenspeicher

Stellen Sie sicher, dass die Firewallregeln für die Unternehmensfirewall, die Windows-Firewall auf dem Gatewaycomputer und den Datenspeicher selbst richtig aktiviert sind. Indem Sie diese Regeln aktivieren, kann das Gateway sowohl mit der Quelle als auch mit der Senke erfolgreich eine Verbindung herstellen. Aktivieren Sie die Regeln für jeden Datenspeicher, der am Kopiervorgang beteiligt ist.

Um z.B. Daten aus **einem lokalen Datenspeicher in eine Azure SQL-Datenbanksenke oder eine Azure SQL Data Warehouse-Senke** zu kopieren, müssen Sie Folgendes tun:

- Lassen Sie ausgehende **TCP**-Kommunikation an Port **1433** sowohl für die Windows-Firewall als auch die Unternehmensfirewall zu.
- Konfigurieren Sie die Firewall der Azure SQL Server-Instanz, um die IP-Adresse des Gatewaycomputers zur Liste der zulässigen IP-Adressen hinzuzufügen.


### Proxyserver-Aspekte
Wenn die Netzwerkumgebung Ihres Unternehmens einen Proxyserver für den Internetzugriff verwendet, konfigurieren Sie das Datenverwaltungsgateway mit den geeigneten Proxyeinstellungen. Sie können den Proxy während der anfänglichen Registrierungsphase festlegen.

![Einrichten des Proxys während der Registrierung](media/data-factory-data-management-gateway/SetProxyDuringRegistration.png)

Das Gateway verwendet den Proxyserver, um eine Verbindung mit dem Clouddienst herzustellen. Klicken Sie während des Anfangssetups auf **Ändern**. Sie sehen das Dialogfeld **Proxyeinstellung**.

![Einrichten des Proxys mithilfe des Konfigurations-Managers](media/data-factory-data-management-gateway/SetProxySettings.png)

Es gibt drei Konfigurationsoptionen:

- **Proxy nicht verwenden**: Das Gateway verwendet nicht explizit einen Proxy, um eine Verbindung mit den Clouddiensten herzustellen.
- **Systemproxy verwenden**: Das Gateway verwendet die in „diahost.exe.config“ konfigurierten Proxyeinstellungen. Wenn in „diahost.exe.config“ kein Proxy konfiguriert ist, stellt das Gateway die Verbindung zu den Clouddiensten nicht über einen Proxy, sondern direkt her.
- **Benutzerdefinierten Proxy verwenden**: Konfigurieren Sie die für das Gateway zu verwendenden HTTP-Proxyeinstellungen hier statt in der Datei „diahost.exe.config“. Adresse und Port sind erforderlich. Benutzername und Kennwort sind optional, je nach den Authentifizierungseinstellungen Ihres Proxys. Alle Einstellungen werden mit dem Zertifikat für Anmeldeinformationen des Gateways verschlüsselt und lokal auf dem Gatewayhostcomputer gespeichert.

Der Datenverwaltungsgateway-Hostdienst wird automatisch neu gestartet, nachdem Sie die aktualisierten Proxyeinstellungen gespeichert haben.

Wenn Sie die Proxyeinstellungen nach der erfolgreichen Registrierung des Gateways anzeigen oder aktualisieren möchten, verwenden Sie den Datenverwaltungsgateway-Konfigurations-Manager.

1. Starten Sie den Datenverwaltungsgateway-Konfigurations-Manager.
2. Wechseln Sie zur Registerkarte **Einstellungen**.
3. Klicken Sie im Abschnitt **HTTP-Proxy** auf den Link **Ändern**, um das Dialogfeld **HTTP-Proxy festlegen** zu öffnen.
4. Nachdem Sie auf die Schaltfläche **Weiter** geklickt haben, wird ein Dialogfeld mit einer Warnung angezeigt, in dem Sie bestätigen müssen, dass die Proxyeinstellung gespeichert und der Gatewayhostdienst neu gestartet werden soll.

Sie können den HTTP-Proxy im Konfigurations-Manager anzeigen und aktualisieren.

![Einrichten des Proxys mithilfe des Konfigurations-Managers](media/data-factory-data-management-gateway/SetProxyConfigManager.png)

> [AZURE.NOTE] Wenn Sie einen Proxyserver mit NTLM-Authentifizierung einrichten, wird der Gatewayhostdienst im Domänenkonto ausgeführt. Wenn Sie das Kennwort für das Domänenkonto später ändern, denken Sie daran, die Konfigurationseinstellungen für den Dienst entsprechend zu aktualisieren und neu zu starten. Aufgrund dieser Anforderung empfiehlt es sich, ein dediziertes Domänenkonto für den Zugriff auf den Proxyserver zu verwenden, in dem das Kennwort nicht regelmäßig geändert werden muss.

### Konfigurieren der Proxyeinstellungen in „diahost.exe.config“
Wenn Sie die Einstellung **Systemproxy verwenden** für den HTTP-Proxy auswählen, verwendet das Gateway die Proxyeinstellungen in „diahost.exe.config“. Wenn in „diahost.exe.config“ kein Proxy angegeben ist, stellt das Gateway die Verbindung zu den Clouddiensten nicht über einen Proxy, sondern direkt her. Das folgende Verfahren enthält Anweisungen für die Aktualisierung der Konfigurationsdatei.

1.	Erstellen Sie im Datei-Explorer eine sichere Kopie von „C:\\Programme\\Microsoft Data Management Gateway\\2.0\\Shared\\diahost.exe.config“, um die Originaldatei zu sichern.
2.	Starten Sie „Notepad.exe“ als Administrator, und öffnen Sie die Textdatei „C:\\Programme\\Microsoft Data Management Gateway\\2.0\\Shared\\diahost.exe.config“. Sie finden das Standardtag für „system.net“ wie folgt:

			<system.net>
				<defaultProxy useDefaultCredentials="true" />
			</system.net>	

	Anschließend können Sie die Informationen zum Proxyserver wie im folgenden Beispiel gezeigt hinzufügen:

			<system.net>
			      <defaultProxy enabled="true">
			            <proxy bypassonlocal="true" proxyaddress="http://proxy.domain.org:8888/" />
			      </defaultProxy>
			</system.net>

	Zusätzliche Eigenschaften sind im Proxytag zulässig, um die erforderlichen Einstellungen anzugeben, z. B. scriptLocation. Informationen zur Syntax finden Sie unter [<proxy>-Element (Netzwerkeinstellungen)](https://msdn.microsoft.com/library/sa91de1e.aspx).

			<proxy autoDetect="true|false|unspecified" bypassonlocal="true|false|unspecified" proxyaddress="uriString" scriptLocation="uriString" usesystemdefault="true|false|unspecified "/>

3. Speichern Sie die Konfigurationsdatei am ursprünglichen Speicherort, und starten Sie den Datenverwaltungsgateway-Dienst, der die Änderungen übernimmt. Um den Dienst neu zu starten, verwenden Sie das Applet „Dienste“ in der Systemsteuerung, oder klicken Sie im **Datenverwaltungsgateway-Konfigurations-Manager** auf die Schaltfläche **Dienst beenden** und anschließend auf **Dienst starten**. Wenn der Dienst nicht gestartet wird, wurde der bearbeiteten Anwendungskonfigurationsdatei wahrscheinlich eine falsche XML-Tagsyntax hinzugefügt.

Zusätzlich zu den obigen Punkten müssen Sie auch sicherstellen, dass Microsoft Azure in der Whitelist Ihres Unternehmens aufgeführt ist. Sie können die Liste mit den gültigen Microsoft Azure-IP-Adressen im [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=41653) herunterladen.

#### Mögliche Symptome für Probleme im Zusammenhang mit der Firewall und dem Proxyserver
Wenn Sie ähnliche Fehler wie die unten aufgeführten feststellen, liegt dies meist an einer unsachgemäßen Konfiguration der Firewall oder des Proxyservers, die verhindert, dass das Gateway eine Verbindung mit der Data Factory herstellt, um sich zu authentifizieren. Überprüfen Sie den vorherigen Abschnitt, um sicherzustellen, dass die Firewall und der Proxyserver richtig konfiguriert sind.

1.	Wenn Sie versuchen, das Gateway zu registrieren, wird die folgende Fehlermeldung angezeigt: "Fehler beim Registrieren des Gatewayschlüssels. Prüfen Sie, ob sich das Datenverwaltungsgateway im Status "Verbunden" befindet und der Datenverwaltungsgateway-Hostdienst gestartet wurde, bevor Sie versuchen, den Gatewayschlüssel erneut zu registrieren."
2.	Wenn Sie den Konfigurations-Manager öffnen, wird der Status als „Getrennt“ oder „Verbindung wird hergestellt“ angezeigt. Wenn Sie die Windows-Ereignisprotokolle anzeigen, sehen Sie unter „Ereignisanzeige“ > „Anwendungs- und Dienstprotokolle“ > „Datenverwaltungsgateway“ Fehlermeldungen wie die folgende: `Unable to connect to the remote server` `A component of Data Management Gateway has become unresponsive and restarts automatically. Component name: Gateway.`

### Öffnen von Port 8050 für die Verschlüsselung der Anmeldeinformationen 
Die Anwendung **Anmeldeinformationen festlegen** verwendet den eingehenden Port **8050**, um die Anmeldeinformationen an das Gateway weiterzugeben, wenn Sie im Azure-Portal einen lokalen verknüpften Dienst einrichten. Während der Einrichtung des Gateways wird es von der Datenverwaltungsgateway-Installation standardmäßig auf dem Gatewaycomputer geöffnet.
 
Bei Verwendung einer Drittanbieterfirewall können Sie den Port 8050 manuell öffnen. Falls beim Einrichten des Gateways ein Firewallproblem auftritt, können Sie versuchen, den folgenden Befehl zu verwenden, um das Gateway ohne Konfiguration der Firewall zu installieren.

	msiexec /q /i DataManagementGateway.msi NOFIREWALL=1

Falls Sie den Port 8050 auf dem Gatewaycomputer nicht öffnen, verwenden Sie andere Verfahren als die Anwendung **Anmeldeinformationen festlegen**, um Anmeldeinformationen für den Datenspeicher zu konfigurieren. Beispielsweise können Sie das PowerShell-Cmdlet [New-AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) verwenden. Informationen zum Festlegen von Datenspeicher-Anmeldeinformationen finden Sie unter [Festlegen von Anmeldeinformationen und Sicherheit](#set-credentials-and-securityy).

## Aktualisieren 
Standardmäßig wird das Datenverwaltungsgateway automatisch aktualisiert, wenn eine neuere Version des Gateways verfügbar ist. Das Gateway wird erst aktualisiert, nachdem alle geplanten Aufgaben erledigt wurden. Vom Gateway werden keine weiteren Aufgaben verarbeitet, bevor der Aktualisierungsvorgang abgeschlossen ist. Wenn die Aktualisierung fehlschlägt, wird für das Gateway ein Rollback auf die alte Version durchgeführt.

Sie sehen die geplante Aktualisierungszeit an folgenden Stellen:

- Auf dem Blatt mit den Gatewayeigenschaften im Azure-Portal.
- Auf der Startseite des Datenverwaltungsgateway-Konfigurations-Managers.
- In einer Benachrichtigung in der Taskleiste.

Auf der Registerkarte „Startseite“ des Datenverwaltungsgateway-Konfigurations-Managers werden der Aktualisierungszeitplan und der Zeitpunkt der letzten Installation bzw. Aktualisierung des Gateways angezeigt.

![Planen von Updates](media/data-factory-data-management-gateway/UpdateSection.png)

Sie können das Update sofort installieren oder warten, bis das Gateway zum geplanten Zeitpunkt automatisch aktualisiert wird. Die folgende Abbildung zeigt beispielsweise die Benachrichtigung, die im Datenverwaltungsgateway-Konfigurations-Manager zusammen mit der Schaltfläche „Aktualisieren“ angezeigt wird, auf die Sie zum sofortigen Installieren klicken können.

![Update im Datenverwaltungsgateway-Konfigurations-Manager](./media/data-factory-data-management-gateway/gateway-auto-update-config-manager.png)

Die Benachrichtigung in der Taskleiste sieht wie folgt aus:

![Meldung in der Taskleiste](./media/data-factory-data-management-gateway/gateway-auto-update-tray-message.png)

Der Status des Aktualisierungsvorgangs (manuell oder automatisch) wird auf der Taskleiste angezeigt. Wenn Sie den Datenverwaltungsgateway-Konfigurations-Manager das nächste Mal öffnen, wird auf der Benachrichtigungsleiste eine Meldung mit dem Hinweis eingeblendet, dass das Gateway aktualisiert wurde. Außerdem ist ein Link zu [Neuigkeiten](data-factory-gateway-release-notes.md) angegeben.

### So deaktivieren/aktivieren Sie das Feature für die automatische Aktualisierung
Sie können das Feature für die automatische Aktualisierung wie folgt deaktivieren und aktivieren:

1. Starten Sie Windows PowerShell auf dem Gatewaycomputer.
2. Wechseln Sie in den Ordner „C:\\Programme\\Microsoft Data Management Gateway\\2.0\\PowerShellScript“.
3. Führen Sie den folgenden Befehl aus, um das Feature für die automatische Aktualisierung zu deaktivieren.

		.\GatewayAutoUpdateToggle.ps1  -off

4. So aktivieren Sie das Feature wieder
	
		.\GatewayAutoUpdateToggle.ps1  -on  

## Konfigurations-Manager 
Nach der Installation des Gateways können Sie den Datenverwaltungsgateway-Konfigurations-Manager auf eine der folgenden Arten starten:

- Geben Sie im Fenster **Suchen** den Begriff **Datenverwaltungsgateway** ein, um auf dieses Hilfsprogramm zuzugreifen.
- Führen Sie die Datei **ConfigManager.exe** im Ordner **C:\\Programme\\Microsoft Data Management Gateway\\2.0\\Shared** aus.
 
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

- Ausführliche Informationen hierzu finden Sie in den Gatewayprotokollen und Windows-Ereignisprotokollen. Diese finden Sie in der **Ereignisanzeige** von Windows unter **Anwendungs- und Dienstprotokolle** > **Datenverwaltungsgateway**. Wenn Sie Probleme mit dem Gateway beheben müssen, suchen Sie in der Ereignisanzeige nach Ereignissen vom Typ „Fehler“.
- Falls das Gateway nach dem **Ändern des Zertifikats** nicht mehr funktioniert, starten Sie den **Datenverwaltungsgateway-Dienst** neu, indem Sie den Datenverwaltungsgateway-Konfigurations-Manager von Microsoft oder das Applet „Dienste“ in der Systemsteuerung verwenden. Falls weiterhin ein Fehler angezeigt wird, müssen Sie für Benutzer des Datenverwaltungsgateway-Diensts unter Umständen explizite Berechtigungen erteilen, damit sie auf das Zertifikat im Zertifikat-Manager (certmgr.msc) zugreifen können. Das Standardbenutzerkonto für den Dienst lautet **NT Service\\DIAHostService**.
- Wenn die Anwendung **Anmeldeinformationsverwaltung** beim Klicken auf die Schaltfläche „Verschlüsseln“ im Data Factory-Editor die Anmeldeinformationen nicht **verschlüsselt**, vergewissern Sie sich, dass die Anwendung auf dem **Gatewaycomputer** ausgeführt wird. Falls nicht, führen Sie die Anwendung auf dem Gatewaycomputer aus, und versuchen Sie, die Anmeldeinformationen zu verschlüsseln.
- Wenn Sie Fehler bei der Datenspeicherverbindung oder den Datenspeichertreibern bemerken, führen Sie folgende Schritte aus:
	- Starten Sie den **Datenverwaltungsgateway-Konfigurations-Manager** auf dem Gatewaycomputer.
	- Wechseln Sie zur Registerkarte **Diagnose**.
	- Wählen Sie in den Feldern der Gruppe **Testen Sie die Verbindung mit einer lokalen Datenquelle mithilfe dieses Gateways** die geeigneten Werte aus, oder geben Sie die Werte ein.
	- Klicken Sie auf **Verbindung testen**, um festzustellen, ob Sie vom Gatewaycomputer aus mithilfe der Verbindungs- und Anmeldeinformationen eine Verbindung mit der lokalen Datenquelle herstellen können. Wenn das Testen der Verbindung weiter misslingt, nachdem Sie einen Treiber installiert haben, starten Sie das Gateway neu, damit es die letzte Änderung übernimmt.

	![Verbindung testen](./media/data-factory-data-management-gateway/TestConnection.png)

### Senden der Gatewayprotokolle an Microsoft
Wenn Sie sich im Zusammenhang mit Gatewayproblemen an den Microsoft Support wenden, werden Sie möglicherweise aufgefordert, Ihre Gatewayprotokolle freizugeben. In dieser Version des Gateways können Sie die erforderlichen Gatewayprotokolle komfortabel mit nur zwei Mausklicks im Gateway-Konfigurations-Manager freigeben.

1. Wechseln Sie zur Registerkarte **Diagnose** im Gateway-Konfigurations-Manager.
 
	![Datenverwaltungsgateway – Registerkarte „Diagnose“](media/data-factory-data-management-gateway/data-management-gateway-diagnostics-tab.png)
2. Klicken Sie auf den Link **Protokolle senden**, um das folgende Dialogfeld anzuzeigen.

	![Datenverwaltungsgateway – Senden von Protokollen](media/data-factory-data-management-gateway/data-management-gateway-send-logs-dialog.png)
3. (optional) Klicken Sie auf **Protokolle anzeigen**, um Protokolle in der Ereignisanzeige durchzusehen.
4. (optional) Klicken Sie auf **Datenschutz**, um die Datenschutzbestimmungen der Onlinedienste von Microsoft anzuzeigen.
3. Wenn Sie zum Hochladen bereit sind, klicken Sie auf **Protokolle senden**, um Protokolle der letzten sieben Tage zur Problembehandlung an Microsoft zu senden. Daraufhin sollte der Status des Vorgangs „Protokolle senden“ wie in der folgenden Abbildung dargestellt angezeigt werden.

	![Datenverwaltungsgateway – Senden des Status von Protokollen](media/data-factory-data-management-gateway/data-management-gateway-send-logs-status.png)
4. Nach Abschluss des Vorgangs wird ein Dialogfeld wie in der folgenden Abbildung dargestellt angezeigt.
	
	![Datenverwaltungsgateway – Senden des Status von Protokollen](media/data-factory-data-management-gateway/data-management-gateway-send-logs-result.png)
5. Notieren Sie sich die **Berichts-ID**, und teilen Sie sie dem Microsoft Support mit. Die Berichts-ID wird verwendet, um Ihre Gatewayprotokolle zu suchen, die Sie zur Problembehandlung hochgeladen haben. Die Berichts-ID wird auch in der Ereignisanzeige gespeichert, damit Sie darauf zugreifen können. Sie finden Sie anhand der Ereignis-ID „25“ sowie nach Datum und Uhrzeit.
	
	![Datenverwaltungsgateway – Senden der Berichts-ID von Protokollen](media/data-factory-data-management-gateway/data-management-gateway-send-logs-report-id.png)

### Archivieren von Gatewayprotokollen auf dem Gatewayhostcomputer
Es gibt einige Szenarien, in denen Gatewayprobleme auftreten, und Sie Gatewayprotokolle nicht direkt freigeben können:

- Sie installieren und registrieren das Gateway manuell.
- Sie versuchen, das Gateway mit einem neu generierten Schlüssel im Konfigurations-Manager zu registrieren.
- Sie versuchen, Protokolle zu senden, und es kann keine Verbindung mit dem Gatewayhostdienst hergestellt werden.

In solchen Fällen können Sie Gatewayprotokolle als ZIP-Datei speichern und später beim Kontakt mit dem Microsoft Support freigeben. Beispiel: Sie erhalten beim Registrieren des Gateways eine Fehlermeldung wie in der folgenden Abbildung dargestellt:

![Datenverwaltungsgateway – Fehler bei der Registrierung](media/data-factory-data-management-gateway/data-management-gateway-registration-error.png)

Klicken Sie auf den Link **Gatewayprotokolle archivieren**, um Protokolle zu archivieren und zu speichern und dann die ZIP-Datei für den Microsoft Support freizugeben.

![Datenverwaltungsgateway – Archivieren von Protokollen](media/data-factory-data-management-gateway/data-management-gateway-archive-logs.png)

### Gateway ist online mit eingeschränkter Funktionalität 
Sie sehen, dass der Status des Gateways aus einem der folgenden Gründe als **mit eingeschränkter Funktionalität online** angezeigt wird.

- Das Gateway kann nicht über den Servicebus eine Verbindung mit dem Clouddienst herstellen.
- Der Clouddienst kann nicht über den Servicebus eine Verbindung mit dem Gateway herstellen.

Wenn ein Gateway mit eingeschränkter Funktionalität online ist, können Sie möglicherweise nicht mithilfe des Assistenten zum Kopieren von Data Factory Datenpipelines zum Kopieren von Daten in einen bzw. aus einem lokalen Datenspeicher erstellen.

Die Lösung/Umgehung dieses Problems (online mit eingeschränkter Funktionalität) hängt davon ab, ob das Gateway nicht mit dem Clouddienst oder auf andere Art und Weise eine Verbindung herstellen kann. In den folgenden Abschnitte werden diese Umgehungen beschrieben.

#### Das Gateway kann nicht über den Servicebus eine Verbindung mit dem Clouddienst herstellen.
Um das Gateway wieder online zu schalten, gehen Sie wie folgt vor:

1. Aktivieren Sie die ausgehenden Ports 9350-9354 sowohl auf dem Gatewaycomputer als auch auf der Unternehmensfirewall auf der Windows-Firewall. Weitere Informationen finden Sie unter [Ports und Firewall](#ports-and-firewall).
2. Konfigurieren Sie die Proxyeinstellungen auf dem Gateway. Weitere Informationen finden Sie unter [Proxyserver-Aspekte](#proxy-server-considerations).

Um dieses Problem zu umgehen, verwenden Sie den Data Factory-Editor in Azure-Portal (oder) Visual Studio (oder) Azure PowerShell.

#### Fehler: Der Clouddienst kann nicht über den Servicebus eine Verbindung mit dem Gateway herstellen.
Um das Gateway wieder online zu schalten, gehen Sie wie folgt vor:
 
1. Aktivieren Sie die ausgehenden Ports 5671 und 9350-9354 sowohl auf dem Gatewaycomputer als auch auf der Unternehmensfirewall auf der Windows-Firewall. Weitere Informationen finden Sie unter [Ports und Firewall](#ports-and-firewall).
2. Konfigurieren Sie die Proxyeinstellungen auf dem Gateway. Weitere Informationen finden Sie unter [Proxyserver-Aspekte](#proxy-server-considerations).
3. Heben Sie die statische IP-Einschränkung auf dem Proxyserver auf.

Um dieses Problem zu umgehen, können Sie den Data Factory-Editor in Azure-Portal (oder) Visual Studio (oder) Azure PowerShell verwenden.
 
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

1. Starten Sie einen Webbrowser auf dem **Gatewaycomputer**, und navigieren Sie zum [Azure-Portal](http://portal.azure.com). Suchen Sie nach Ihrer Data Factory (falls erforderlich), und öffnen Sie die Data Factory auf dem Blatt **DATA FACTORY**. Klicken Sie anschließend auf **Erstellen und bereitstellen**, um den Data Factory-Editor zu starten.
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

Wenn Sie die Anwendung **Anmeldeinformationen festlegen** verwenden, verschlüsselt das Portal die Anmeldeinformationen mit dem Zertifikat, das Sie auf der Registerkarte **Zertifikat** des **Datenverwaltungsgateway-Konfigurations-Managers** auf dem Gatewaycomputer angegeben haben.

Wenn Sie einen API-basierten Ansatz zum Verschlüsseln der Anmeldeinformationen nutzen möchten, können Sie das PowerShell-Cmdlet [New-AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) verwenden, um Anmeldeinformationen zu verschlüsseln. Das Cmdlet verwendet das Zertifikat, für dessen Verwendung dieses Gateway konfiguriert ist, um die Anmeldeinformationen zu verschlüsseln. Sie können dem Element **EncryptedCredential** von **connectionString** in JSON verschlüsselte Anmeldeinformationen hinzufügen. Verwenden Sie JSON mit dem [New-AzureRmDataFactoryLinkedService](https://msdn.microsoft.com/library/mt603647.aspx)-Cmdlet oder im Data Factory-Editor.

	"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",

Es gibt eine weitere Möglichkeit zum Festlegen der Anmeldeinformationen mit dem Data Factory-Editor. Wenn Sie einen verknüpften SQL Server-Dienst mit dem Editor erstellen und Anmeldeinformationen im Nur-Text-Format eingeben, werden die Anmeldeinformationen mit einem Zertifikat verschlüsselt, das der Data Factory-Dienst besitzt. Es wird NICHT das Zertifikat verwendet, für dessen Verwendung dieses Gateway konfiguriert ist. Dieser Ansatz ist möglicherweise etwas schneller, in einigen Fällen aber weniger sicher. Daher empfehlen wir, dass Sie diesen Ansatz nur für die Entwicklung/Tests verwenden.


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

	
4. Wechseln Sie in Azure PowerShell in den Ordner **C:\\Programme\\Microsoft Data Management Gateway\\2.0\\PowerShellScript**. Führen Sie **RegisterGateway.ps1** mit der lokalen Variablen **$Key** aus, wie im folgenden Befehl gezeigt. Dieses Skript registriert den auf dem Computer installierten Client-Agent bei dem logischen Gateway, das Sie zuvor erstellt haben.

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
- Informationen finden Sie unter [Verschieben von Daten zwischen lokalen Quellen und der Cloud mit dem Datenverwaltungsgateway](data-factory-move-data-between-onprem-and-cloud.md). In der exemplarischen Vorgehensweise erstellen Sie eine Pipeline, die das Gateway verwendet, um Daten aus einer lokalen SQL Server-Datenbank in einen Azure-Blob zu verschieben.

<!---HONumber=AcomDC_0921_2016-->