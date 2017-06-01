---
title: "Datenverwaltungsgateway für Data Factory | Microsoft Docs"
description: Richten Sie ein Datengateway ein, um Daten zwischen dem lokalen Speicher und der Cloud zu verschieben. Verwenden Sie das Datenverwaltungsgateway in Azure Data Factory zum Verschieben Ihrer Daten.
services: data-factory
documentationcenter: 
author: nabhishek
manager: jhubbard
editor: monicar
ms.assetid: b9084537-2e1c-4e96-b5bc-0e2044388ffd
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: abnarain
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: 01f3ef6d0e8e43e702b8292a7c215d3df58817f2
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017


---
# <a name="data-management-gateway"></a>Gateway zur Datenverwaltung
Das Datenverwaltungsgateway ist ein Client-Agent, den Sie in Ihrer lokalen Umgebung installieren müssen, um Daten zwischen Ihren Cloudspeichern und lokalen Datenspeichern kopieren zu können. Die lokalen von Data Factory unterstützten Datenspeicher werden im Abschnitt [Unterstützte Datenquellen](data-factory-data-movement-activities.md#supported-data-stores-and-formats) aufgeführt.

> [!NOTE]
> Das Gateway unterstützt derzeit nur die Kopieraktivität und die Aktivität mit gespeicherten Prozeduren in Data Factory. Das Gateway kann nicht über eine benutzerdefinierte Aktivität verwendet werden, um auf lokale Datenquellen zuzugreifen.
>
>

Dieser Artikel ergänzt die exemplarische Vorgehensweise im Artikel [Verschieben von Daten zwischen lokalen Datenspeichern und Clouddatenspeichern](data-factory-move-data-between-onprem-and-cloud.md) . In der exemplarischen Vorgehensweise erstellen Sie eine Pipeline, die das Gateway verwendet, um Daten aus einer lokalen SQL Server-Datenbank in einen Azure-Blob zu verschieben. Dieser Artikel bietet ausführliche Informationen zur Verwendung des Datenverwaltungsgateways.   

## <a name="overview"></a>Übersicht
### <a name="capabilities-of-data-management-gateway"></a>Funktionen des Datenverwaltungsgateways
Das Datenverwaltungsgateway bietet die folgenden Funktionen:

* Modellieren von lokalen Datenquellen und Clouddatenquellen innerhalb einer Data Factory und Verschieben von Daten.
* Nutzen einer zentralen Konsole für die Überwachung und Verwaltung mit Informationen über den Gatewaystatus über das Data Factory-Blatt.
* Sicheres Verwalten des Zugriffs auf lokale Datenquellen.
  * An der Unternehmensfirewall ist keine Änderung erforderlich. Das Gateway stellt nur ausgehende HTTP-basierte Verbindungen mit dem offenen Internet her.
  * Verschlüsseln Sie die Anmeldeinformationen für Ihre lokalen Datenspeicher mit dem Zertifikat.
* Effizientes Verschieben von Daten – Daten werden parallel verschoben, und zwar mit Ausfallsicherheit bei vorübergehenden Netzwerkproblemen dank automatischer Wiederholungslogik.

### <a name="command-flow-and-data-flow"></a>Befehls- und Datenfluss
Wenn Sie Daten mithilfe einer Kopieraktivität zwischen der lokalen Umgebung und der Cloud kopieren, verwendet diese Aktivität ein Gateway, um die Daten aus einer lokalen Datenquelle in die Cloud und umgekehrt zu übertragen.

Im Anschluss finden Sie eine Darstellung des allgemeinen Datenflusses sowie eine Zusammenfassung der Schritte für das Kopieren mit dem Datengateway: ![Datenfluss über ein Gateway](./media/data-factory-data-management-gateway/data-flow-using-gateway.png)

1. Der Datenentwickler erstellt entweder über das [Azure-Portal](https://portal.azure.com) oder mit einem [PowerShell-Cmdlet](https://msdn.microsoft.com/library/dn820234.aspx) ein Gateway für Azure Data Factory.
2. Der Datenentwickler erstellt durch Angabe des Gateways einen verknüpften Dienst für einen lokalen Datenspeicher. Als Teil der Einrichtung des verknüpften Diensts verwendet der Datenentwickler die Anwendung „Anmeldeinformationen festlegen“, um Authentifizierungstypen und Anmeldeinformationen anzugeben.  Die Anwendung „Anmeldeinformationen festlegen“ kommuniziert mit dem Datenspeicher, um die Verbindung zu testen, und mit dem Gateway, um die Anmeldeinformationen zu speichern.
3. Das Gateway verschlüsselt die Anmeldeinformationen mit dem Zertifikat, das dem Gateway zugeordnet wurde (bereitgestellt vom Datenentwickler), bevor die Anmeldeinformationen in der Cloud gespeichert werden.
4. Der Data Factory-Dienst kommuniziert für die Planung und Verwaltung von Aufträgen mit dem Gateway. Dazu wird ein Steuerungskanal genutzt, der eine freigegebene Azure Service Bus-Warteschlange verwendet. Wenn ein Auftrag der Kopieraktivität gestartet werden muss, reiht Data Factory die Anforderung zusammen mit Anmeldeinformationen in die Warteschlange ein. Das Gateway startet den Auftrag, nachdem die Warteschlange abgerufen wurde.
5. Das Gateway entschlüsselt die Anmeldeinformationen mit dem gleichen Zertifikat und stellt dann mit dem richtigen Authentifizierungstyp und den Anmeldeinformationen eine Verbindung mit dem lokalen Datenspeicher her.
6. Das Gateway kopiert Daten aus einem lokalen Speicher in einen Cloudspeicher oder umgekehrt, je nachdem, wie die Kopieraktivität in der Datenpipeline konfiguriert ist. Für diesen Schritt kommuniziert das Gateway über einen sicheren Kanal (HTTPS) direkt mit einem cloudbasierten Speicherdienst wie z.B. Azure Blob Storage.

### <a name="considerations-for-using-gateway"></a>Überlegungen zur Verwendung des Gateways
* Ein einzelne Instanz des Datenverwaltungsgateways kann für mehrere lokale Datenquellen verwendet werden. Allerdings ist **eine einzelne Gatewayinstanz nur an eine Azure Data Factory gebunden** und kann nicht mit einer anderen Data Factory gemeinsam genutzt werden.
* Es darf **nur eine Instanz des Datenverwaltungsgateways** auf einem Computer installiert sein. Wenn Sie beispielsweise über zwei Data Factorys verfügen, die auf lokale Datenquellen zugreifen müssen, dann müssen Sie auf zwei lokalen Computern jeweils ein Gateway installieren. Anders gesagt: Jedes Gateway ist an eine spezifische Data Factory gebunden.
* Das **Gateway muss sich nicht auf dem gleichen Computer befinden wie die Datenquelle**. Wenn Sie das Gateway jedoch in der Nähe der Datenquelle platzieren, verkürzen Sie die Zeit, die das Gateway benötigt, um eine Verbindung mit der Datenquelle herzustellen. Es wird empfohlen, das Gateway auf einem anderen Computer als dem Computer zu installieren, auf dem die lokale Datenquelle gehostet wird. Wenn sich Gateway und Datenquelle auf unterschiedlichen Computern befinden, konkurriert das Gateway nicht mit der Datenquelle um Ressourcen.
* Sie können über **mehrere Gateways auf verschiedenen Computern verfügen, die eine Verbindung mit der gleichen lokalen Datenquelle herstellen**. Sie können z. B. über zwei Gateways verfügen, die zwei Data Factorys mit Daten versorgen, wobei jedoch die lokale Datenquelle für die beiden Data Factorys registriert ist.
* Falls Sie auf Ihrem Computer bereits ein Gateway installiert haben, das für ein **Power BI**-Szenario verwendet wird, installieren Sie auf einem anderen Computer ein **separates Gateway für die Azure Data Factory**.
* Sie müssen das Gateway selbst dann verwenden, wenn Sie **ExpressRoute**einsetzen.
* Behandeln Sie Ihre Datenquelle wie eine lokale Datenquelle (die sich hinter einer Firewall befindet), selbst wenn Sie **ExpressRoute**verwenden. Verwenden Sie das Gateway, um eine Verbindung zwischen dem Dienst und der Datenquelle herzustellen.
* Sie müssen auch dann **das Gateway verwenden**, wenn der Datenspeicher sich in der Cloud auf einer **Azure IaaS-VM** befindet.

## <a name="installation"></a>Installation
### <a name="prerequisites"></a>Voraussetzungen
* Die unterstützten **Betriebssystemversionen** sind Windows 7, Windows 8/8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012 und Windows Server 2012 R2. Die Installation des Datenverwaltungsgateways auf einem Domänencontroller wird derzeit nicht unterstützt.
* Sie benötigen mindestens .NET Framework 4.5.1. Wenn Sie das Gateway auf einem Windows 7-Computer installieren, installieren Sie mindestens .NET Framework 4.5. Ausführlichere Informationen finden Sie unter [Systemanforderungen für .NET Framework](https://msdn.microsoft.com/library/8z6watww.aspx) .
* Die empfohlene **Konfiguration** für den Gatewaycomputer lautet: mindestens 2 GHz, 4 Kerne, 8 GB RAM und 80 GB Festplattenspeicher.
* Wenn sich der Hostcomputer im Ruhezustand befindet, reagiert das Gateway nicht auf Datenanforderungen. Aus diesem Grund sollten Sie vor der Installation des Gateways einen entsprechenden **Energiesparplan** auf dem Computer konfigurieren. Wenn der Computer für den Ruhezustand konfiguriert ist, wird bei der Gatewayinstallation eine Meldung angezeigt.
* Sie müssen ein Administrator auf dem Computer sein, um das Datenverwaltungsgateway erfolgreich installieren und konfigurieren zu können. Sie können der lokalen Windows-Gruppe **Datenverwaltungsgateway-Benutzer** zusätzliche Benutzer hinzufügen. Die Mitglieder dieser Gruppe können das Gateway mithilfe des Datenverwaltungsgateway-Konfigurations-Managers konfigurieren.

Da die Kopieraktivität mit einer bestimmten Häufigkeit ausgeführt wird, folgt die Ressourcenverwendung (CPU, Arbeitsspeicher) auf dem Computer dem gleichen Muster mit Spitzen- und Leerlaufzeiten. Die Ressourcenverwendung hängt auch stark von der Datenmenge ab, die verschoben wird. Wenn mehrere Kopieraufträge in Bearbeitung sind, steigt die Ressourcenverwendung zu Spitzenzeiten an.

### <a name="installation-options"></a>Installationsoptionen
Das Datenverwaltungsgateway kann auf folgende Weise installiert werden:

* Durch Herunterladen eines MSI-Setuppakets aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717).  Die MSI-Datei kann auch verwendet werden, um ein vorhandenes Datenverwaltungsgateway auf die neueste Version zu aktualisieren, wobei alle Einstellungen beibehalten werden.
* Durch Klicken auf den Link **Datengateway herunterladen und installieren** unter MANUELLES SETUP oder **Direkt auf diesem Computer installieren** unter EXPRESS-SETUP. Im Artikel [Verschieben von Daten zwischen lokalen Quellen und der Cloud](data-factory-move-data-between-onprem-and-cloud.md) finden Sie schrittweise Anweisungen zur Verwendung des Express-Setups. Durch den manuellen Schritt gelangen Sie zum Download Center.  Wie Sie das Gateway aus dem Download Center herunterladen und installieren, erfahren Sie im nächsten Abschnitt.

### <a name="installation-best-practices"></a>Bewährte Methoden für die Installation:
1. Konfigurieren Sie den Energiesparplan auf dem Hostcomputer für das Gateway, sodass der Computer nicht in den Ruhezustand wechselt. Wenn sich der Hostcomputer im Ruhezustand befindet, reagiert das Gateway nicht auf Datenanforderungen.
2. Sichern Sie das Zertifikat, das dem Gateway zugeordnet ist.

### <a name="install-gateway-from-download-center"></a>Installieren des Gateways aus dem Download Center
1. Navigieren Sie zur [Downloadseite des Microsoft-Datenverwaltungsgateways](https://www.microsoft.com/download/details.aspx?id=39717).
2. Klicken Sie auf **Herunterladen**, wählen Sie die entsprechende Version (**32-Bit** oder **64-Bit**) aus, und klicken Sie auf **Weiter**.
3. Führen Sie **MSI** direkt aus, oder speichern Sie es zur späteren Ausführung auf Ihrer Festplatte.
4. Wählen Sie auf der **Willkommensseite** eine **Sprache** aus, und klicken Sie auf **Weiter**.
5. **Akzeptieren** Sie den Endbenutzer-Lizenzvertrag, und klicken Sie auf **Weiter**.
6. Wählen Sie den **Ordner** zur Installation des Gateways aus, und klicken Sie auf **Weiter**.
7. Klicken Sie auf der Seite **Bereit zur Installation** auf **Installieren**.
8. Klicken Sie auf **Fertig stellen** , um die Installation abzuschließen.
9. Ermitteln Sie den Schlüssel im Azure-Portal. Im nächsten Abschnitt finden Sie schrittweise Anweisungen.
10. Führen Sie im **Konfigurations-Manager des Datenverwaltungsgateways**, der auf Ihrem Computer ausgeführt wird, auf der Seite **Gateway registrieren** die folgenden Schritte aus:
    1. Fügen Sie den Schlüssel in das Textfeld ein.
    2. Optional klicken Sie auf **Gatewayschlüssel anzeigen** , um den Schlüsseltext anzuzeigen.
    3. Klicken Sie auf **Registrieren**.

### <a name="register-gateway-using-key"></a>Registrieren des Gateways mit dem Schlüssel
#### <a name="if-you-havent-already-created-a-logical-gateway-in-the-portal"></a>Wenn Sie nicht bereits ein logisches Gateway im Portal erstellt haben
Um im Portal ein Gateway zu erstellen und auf dem Blatt **Konfigurieren** den Schlüssel zu ermitteln, führen Sie die Schritte der exemplarischen Vorgehensweise aus dem Artikel [Verschieben von Daten zwischen lokalen Quellen und der Cloud](data-factory-move-data-between-onprem-and-cloud.md) aus.    

#### <a name="if-you-have-already-created-the-logical-gateway-in-the-portal"></a>Wenn Sie bereits ein logisches Gateway im Portal erstellt haben
1. Wechseln Sie im Azure-Portal zum Blatt **Data Factory**, und klicken Sie auf die Kachel **Verknüpfte Dienste**.

    ![Blatt "Data Factory"](media/data-factory-data-management-gateway/data-factory-blade.png)
2. Wählen Sie auf dem Blatt **Verknüpfte Dienste** das logische **Gateway** aus, das Sie im Portal erstellt haben.

    ![logisches Gateway](media/data-factory-data-management-gateway/data-factory-select-gateway.png)  
3. Klicken Sie auf dem Blatt **Datengateway** auf **Datengateway herunterladen und installieren**.

    ![Link zum Herunterladen im Portal](media/data-factory-data-management-gateway/download-and-install-link-on-portal.png)   
4. Klicken Sie auf dem Blatt **Konfigurieren** auf **Schlüssel neu erstellen**. Klicken Sie in der Warnmeldung auf „Ja“, nachdem Sie sie sorgfältig gelesen haben.

    ![Schlüssel neu erstellen](media/data-factory-data-management-gateway/recreate-key-button.png)
5. Klicken Sie neben dem Schlüssel auf die Schaltfläche „Kopieren“. Der Schlüssel wird in die Zwischenablage kopiert.

    ![Kopieren des Schlüssels](media/data-factory-data-management-gateway/copy-gateway-key.png)

### <a name="system-tray-icons-notifications"></a>Symbole und Benachrichtigungen in der Taskleiste
Die folgende Abbildung enthält einige Symbole, die auf der Taskleiste angezeigt werden.

![Taskleistensymbole](./media/data-factory-data-management-gateway/gateway-tray-icons.png)

Wenn Sie den Cursor auf der Taskleiste auf das Symbol oder die Benachrichtigung bewegen, werden in einem Popupfenster Details zum Zustand des Gateways bzw. des Aktualisierungsvorgangs angezeigt.

### <a name="ports-and-firewall"></a>Ports und Firewall
Zwei Firewalls müssen berücksichtigt werden: Die **Unternehmensfirewall**, die auf dem zentralen Router des Unternehmens ausgeführt wird, und die **Windows-Firewall**, die als Daemon auf dem lokalen Computer konfiguriert ist, auf dem das Gateway installiert ist.  

![Firewalls](./media/data-factory-data-management-gateway/firewalls2.png)

Auf Ebene der Unternehmensfirewall müssen Sie die folgenden Domänen und ausgehenden Ports konfigurieren:

| Domänennamen | Ports | Beschreibung |
| --- | --- | --- |
| *.servicebus.windows.net |443, 80 |Wird für die Kommunikation mit dem Back-End für den Datenverschiebungsdienst verwendet |
| *.core.windows.net |443 |Wird für das gestaffelte Kopieren mit einem Azure-Blob (sofern konfiguriert) verwendet|
| *.frontend.clouddatahub.net |443 |Wird für die Kommunikation mit dem Back-End für den Datenverschiebungsdienst verwendet |


Auf Ebene der Windows-Firewall sind diese ausgehenden Ports normalerweise aktiviert. Falls nicht, können Sie die Domänen und Ports auf dem Gatewaycomputer entsprechend konfigurieren.

> [!NOTE]
> 1. Basierend auf Ihrer Datenquelle und den Senken müssen unter Umständen der Whitelist Ihrer Unternehmens-/Windows-Firewall zusätzliche Domänen und ausgehende Ports hinzufügen.
> 2. Für einige Clouddatenbanken (z.b. [Azure SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-configure-firewall-settings), [Azure Data Lake](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-secure-data#set-ip-address-range-for-data-access) usw.) müssen Sie möglicherweise der Whitelist in der Firewallkonfiguration die IP-Adresse des Gatewaycomputers hinzufügen.
>
>


#### <a name="copy-data-from-a-source-data-store-to-a-sink-data-store"></a>Kopieren von Daten aus einem Quelldatenspeicher in einen Senkendatenspeicher
Stellen Sie sicher, dass die Firewallregeln für die Unternehmensfirewall, die Windows-Firewall auf dem Gatewaycomputer und den Datenspeicher selbst richtig aktiviert sind. Indem Sie diese Regeln aktivieren, kann das Gateway sowohl mit der Quelle als auch mit der Senke erfolgreich eine Verbindung herstellen. Aktivieren Sie die Regeln für jeden Datenspeicher, der am Kopiervorgang beteiligt ist.

Führen Sie die folgenden Schritte aus, um z. B. Daten aus **einem lokalen Datenspeicher in eine Azure SQL-Datenbanksenke oder eine Azure SQL Data Warehouse-Senke** zu kopieren:

* Lassen Sie ausgehende **TCP**-Kommunikation an Port **1433** sowohl für die Windows-Firewall als auch die Unternehmensfirewall zu.
* Konfigurieren Sie die Firewall der Azure SQL Server-Instanz, um die IP-Adresse des Gatewaycomputers zur Liste der zulässigen IP-Adressen hinzuzufügen.

> [!NOTE]
> Wenn Ihre Firewall den ausgehenden Port 1433 nicht zulässt, kann das Gateway nicht direkt auf Azure SQL zugreifen. In diesem Fall können Sie das [gestaffelte Kopieren](https://docs.microsoft.com/azure/data-factory/data-factory-copy-activity-performance#staged-copy) in Azure SQL-Datenbank oder Azure SQL Data Warehouse verwenden. Sie benötigen in einem solchen Szenario nur HTTPS (Port 443) für das Verschieben der Daten.
>
>


### <a name="proxy-server-considerations"></a>Proxyserver-Aspekte
Wenn die Netzwerkumgebung Ihres Unternehmens einen Proxyserver für den Internetzugriff verwendet, konfigurieren Sie das Datenverwaltungsgateway mit den geeigneten Proxyeinstellungen. Sie können den Proxy während der anfänglichen Registrierungsphase festlegen.

![Einrichten des Proxys während der Registrierung](media/data-factory-data-management-gateway/SetProxyDuringRegistration.png)

Das Gateway verwendet den Proxyserver, um eine Verbindung mit dem Clouddienst herzustellen. Klicken Sie während des Anfangssetups auf **Ändern** . Sie sehen das Dialogfeld **Proxyeinstellung** .

![Einrichten des Proxys mithilfe des Konfigurations-Managers](media/data-factory-data-management-gateway/SetProxySettings.png)

Es gibt drei Konfigurationsoptionen:

* **Proxy nicht verwenden**: Das Gateway verwendet nicht explizit einen Proxy, um eine Verbindung mit den Clouddiensten herzustellen.
* **Systemproxy verwenden**: Das Gateway verwendet die in „diahost.exe.config“ und „diawp.exe.config“ konfigurierten Proxyeinstellungen.  Wenn in „diahost.exe.config“ und „diawp.exe.config“ kein Proxy konfiguriert ist, stellt das Gateway die Verbindung mit den Clouddiensten nicht über einen Proxy, sondern direkt her.
* **Benutzerdefinierten Proxy verwenden**: Konfigurieren Sie die für das Gateway zu verwendenden HTTP-Proxyeinstellungen hier statt in den Dateien „diahost.exe.config“ und „diawp.exe.config“.  Adresse und Port sind erforderlich.  Benutzername und Kennwort sind optional, je nach den Authentifizierungseinstellungen Ihres Proxys.  Alle Einstellungen werden mit dem Zertifikat für Anmeldeinformationen des Gateways verschlüsselt und lokal auf dem Gatewayhostcomputer gespeichert.

Der Datenverwaltungsgateway-Hostdienst wird automatisch neu gestartet, nachdem Sie die aktualisierten Proxyeinstellungen gespeichert haben.

Wenn Sie die Proxyeinstellungen nach der erfolgreichen Registrierung des Gateways anzeigen oder aktualisieren möchten, verwenden Sie den Datenverwaltungsgateway-Konfigurations-Manager.

1. Starten Sie den Datenverwaltungsgateway-Konfigurations-Manager.
2. Wechseln Sie zur Registerkarte **Einstellungen** .
3. Klicken Sie im Abschnitt **HTTP-Proxy** auf den Link **Ändern**, um das Dialogfeld **HTTP-Proxy festlegen** zu öffnen.  
4. Nachdem Sie auf die Schaltfläche **Weiter** geklickt haben, wird ein Dialogfeld mit einer Warnung angezeigt, in dem Sie bestätigen müssen, dass die Proxyeinstellung gespeichert und der Gatewayhostdienst neu gestartet werden soll.

Sie können den HTTP-Proxy im Konfigurations-Manager anzeigen und aktualisieren.

![Einrichten des Proxys mithilfe des Konfigurations-Managers](media/data-factory-data-management-gateway/SetProxyConfigManager.png)

> [!NOTE]
> Wenn Sie einen Proxyserver mit NTLM-Authentifizierung einrichten, wird der Gatewayhostdienst im Domänenkonto ausgeführt. Wenn Sie das Kennwort für das Domänenkonto später ändern, denken Sie daran, die Konfigurationseinstellungen für den Dienst entsprechend zu aktualisieren und neu zu starten. Aufgrund dieser Anforderung empfiehlt es sich, ein dediziertes Domänenkonto für den Zugriff auf den Proxyserver zu verwenden, in dem das Kennwort nicht regelmäßig geändert werden muss.
>
>

### <a name="configure-proxy-server-settings"></a>Konfigurieren von Proxyservereinstellungen
Wenn Sie die Einstellung **Systemproxy verwenden** für den HTTP-Proxy auswählen, verwendet das Gateway die Proxyeinstellungen in „diahost.exe.config“ und „diawp.exe.config“.  Wenn in „diahost.exe.config“ und „diawp.exe.config“ kein Proxy angegeben ist, stellt das Gateway die Verbindung mit den Clouddiensten nicht über einen Proxy, sondern direkt her. Das folgende Verfahren enthält Anweisungen für die Aktualisierung der Datei „diahost.exe.config“.  

1. Erstellen Sie im Datei-Explorer eine sichere Kopie von „C:\Programme\Microsoft Data Management Gateway\2.0\Shared\diahost.exe.config“, um die Originaldatei zu sichern.
2. Starten Sie „Notepad.exe“ als Administrator, und öffnen Sie die Textdatei „C:\Programme\Microsoft Data Management Gateway\2.0\Shared\diahost.exe.config“. Sie finden das Standardtag für „system.net“, wie im folgenden Code gezeigt:

         <system.net>
             <defaultProxy useDefaultCredentials="true" />
         </system.net>    

   Anschließend können Sie die Informationen zum Proxyserver wie im folgenden Beispiel gezeigt hinzufügen:

         <system.net>
               <defaultProxy enabled="true">
                     <proxy bypassonlocal="true" proxyaddress="http://proxy.domain.org:8888/" />
               </defaultProxy>
         </system.net>

   Zusätzliche Eigenschaften sind im Proxytag zulässig, um die erforderlichen Einstellungen anzugeben, z. B. scriptLocation. Informationen zur Syntax finden Sie unter [-proxy-Element (Netzwerkeinstellungen)](https://msdn.microsoft.com/library/sa91de1e.aspx).

         <proxy autoDetect="true|false|unspecified" bypassonlocal="true|false|unspecified" proxyaddress="uriString" scriptLocation="uriString" usesystemdefault="true|false|unspecified "/>
3. Speichern Sie die Konfigurationsdatei am ursprünglichen Speicherort, und starten Sie den Datenverwaltungsgateway-Dienst, der die Änderungen übernimmt. Um den Dienst neu zu starten, verwenden Sie das Applet „Dienste“ in der Systemsteuerung, oder klicken Sie im **Datenverwaltungsgateway-Konfigurations-Manager** auf die Schaltfläche **Dienst beenden** und anschließend auf **Dienst starten**. Wenn der Dienst nicht gestartet wird, wurde der bearbeiteten Anwendungskonfigurationsdatei wahrscheinlich eine falsche XML-Tagsyntax hinzugefügt.

> [!IMPORTANT]
> Vergessen Sie nicht, **beide** Dateien („diahost.exe.config“ und „diawp.exe.config“) zu konfigurieren.  


Zusätzlich zu den obigen Punkten müssen Sie auch sicherstellen, dass Microsoft Azure in der Whitelist Ihres Unternehmens aufgeführt ist. Sie können die Liste mit den gültigen Microsoft Azure-IP-Adressen im [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=41653)herunterladen.

#### <a name="possible-symptoms-for-firewall-and-proxy-server-related-issues"></a>Mögliche Symptome für Probleme im Zusammenhang mit der Firewall und dem Proxyserver
Wenn Sie ähnliche Fehler wie die unten aufgeführten feststellen, liegt dies meist an einer unsachgemäßen Konfiguration der Firewall oder des Proxyservers, die verhindert, dass das Gateway eine Verbindung mit der Data Factory herstellt, um sich zu authentifizieren. Überprüfen Sie den vorherigen Abschnitt, um sicherzustellen, dass die Firewall und der Proxyserver richtig konfiguriert sind.

1. Wenn Sie versuchen, das Gateway zu registrieren, wird die folgende Fehlermeldung angezeigt: "Fehler beim Registrieren des Gatewayschlüssels. Prüfen Sie, ob sich das Datenverwaltungsgateway im Status "Verbunden" befindet und der Datenverwaltungsgateway-Hostdienst gestartet wurde, bevor Sie versuchen, den Gatewayschlüssel erneut zu registrieren."
2. Wenn Sie den Konfigurations-Manager öffnen, wird der Status als „Getrennt“ oder „Verbindung wird hergestellt“ angezeigt. Wenn Sie Windows-Ereignisprotokolle anzeigen, sehen Sie unter „Ereignisanzeige“ > „Anwendungs- und Dienstprotokolle“ > „Datenverwaltungsgateway“ Fehlermeldungen wie die folgende: `Unable to connect to the remote server`
   `A component of Data Management Gateway has become unresponsive and restarts automatically. Component name: Gateway.`

### <a name="open-port-8050-for-credential-encryption"></a>Öffnen von Port 8050 für die Verschlüsselung der Anmeldeinformationen
Die Anwendung **Anmeldeinformationen festlegen** verwendet den eingehenden Port **8050**, um die Anmeldeinformationen an das Gateway weiterzugeben, wenn Sie im Azure-Portal einen lokalen verknüpften Dienst einrichten. Während der Einrichtung des Gateways wird es von der Datenverwaltungsgateway-Installation standardmäßig auf dem Gatewaycomputer geöffnet.

Bei Verwendung einer Drittanbieterfirewall können Sie den Port 8050 manuell öffnen. Falls beim Einrichten des Gateways ein Firewallproblem auftritt, können Sie versuchen, den folgenden Befehl zu verwenden, um das Gateway ohne Konfiguration der Firewall zu installieren.

    msiexec /q /i DataManagementGateway.msi NOFIREWALL=1

Falls Sie den Port 8050 auf dem Gatewaycomputer nicht öffnen, verwenden Sie andere Verfahren als die Anwendung **Anmeldeinformationen festlegen** , um Anmeldeinformationen für den Datenspeicher zu konfigurieren. Beispielsweise können Sie das PowerShell-Cmdlet [New-AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) verwenden. Informationen zum Festlegen von Datenspeicher-Anmeldeinformationen finden Sie unter [Festlegen von Anmeldeinformationen und Sicherheit](#set-credentials-and-securityy) .

## <a name="update"></a>Aktualisieren
Standardmäßig wird das Datenverwaltungsgateway automatisch aktualisiert, wenn eine neuere Version des Gateways verfügbar ist. Das Gateway wird erst aktualisiert, nachdem alle geplanten Aufgaben erledigt wurden. Vom Gateway werden keine weiteren Aufgaben verarbeitet, bevor der Aktualisierungsvorgang abgeschlossen ist. Wenn die Aktualisierung fehlschlägt, wird für das Gateway ein Rollback auf die alte Version durchgeführt.

Sie sehen die geplante Aktualisierungszeit an folgenden Stellen:

* Auf dem Blatt mit den Gatewayeigenschaften im Azure-Portal.
* Auf der Startseite des Datenverwaltungsgateway-Konfigurations-Managers.
* In einer Benachrichtigung in der Taskleiste.

Auf der Registerkarte „Startseite“ des Datenverwaltungsgateway-Konfigurations-Managers werden der Aktualisierungszeitplan und der Zeitpunkt der letzten Installation bzw. Aktualisierung des Gateways angezeigt.

![Planen von Updates](media/data-factory-data-management-gateway/UpdateSection.png)

Sie können das Update sofort installieren oder warten, bis das Gateway zum geplanten Zeitpunkt automatisch aktualisiert wird. Die folgende Abbildung zeigt beispielsweise die Benachrichtigung, die im Datenverwaltungsgateway-Konfigurations-Manager zusammen mit der Schaltfläche „Aktualisieren“ angezeigt wird, auf die Sie zum sofortigen Installieren klicken können.

![Update im Datenverwaltungsgateway-Konfigurations-Manager](./media/data-factory-data-management-gateway/gateway-auto-update-config-manager.png)

Die Benachrichtigung in der Taskleiste sieht wie in der folgenden Abbildung aus:

![Meldung in der Taskleiste](./media/data-factory-data-management-gateway/gateway-auto-update-tray-message.png)

Der Status des Aktualisierungsvorgangs (manuell oder automatisch) wird auf der Taskleiste angezeigt. Wenn Sie den Datenverwaltungsgateway-Konfigurations-Manager das nächste Mal öffnen, wird auf der Benachrichtigungsleiste eine Meldung mit dem Hinweis eingeblendet, dass das Gateway aktualisiert wurde. Außerdem ist ein Link zu [Neuigkeiten](data-factory-gateway-release-notes.md) angegeben.

### <a name="to-disableenable-auto-update-feature"></a>So deaktivieren/aktivieren Sie das Feature für die automatische Aktualisierung
Sie können das Feature für die automatische Aktualisierung wie folgt deaktivieren und aktivieren:

1. Starten Sie Windows PowerShell auf dem Gatewaycomputer.
2. Wechseln Sie in den Ordner „C:\Programme\Microsoft Data Management Gateway\2.0\PowerShellScript“.
3. Führen Sie den folgenden Befehl aus, um das Feature für die automatische Aktualisierung zu deaktivieren.   

    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -off
    ```
4. So aktivieren Sie das Feature wieder

    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -on  
    ```
## <a name="configuration-manager"></a>Konfigurations-Manager
Nach der Installation des Gateways können Sie den Datenverwaltungsgateway-Konfigurations-Manager auf eine der folgenden Arten starten:

* Geben Sie im Fenster **Suchen** den Begriff **Datenverwaltungsgateway** ein, um auf dieses Hilfsprogramm zuzugreifen.
* Führen Sie die Datei **ConfigManager.exe** im Ordner **C:\Programme\Microsoft Data Management Gateway\2.0\Shared** aus.

### <a name="home-page"></a>Startseite
Auf der Startseite können Sie die folgenden Aktionen ausführen:

* Anzeigen des Gatewaystatus (mit Clouddienst verbunden oder Ähnliches)
* **Registrieren** mithilfe eines Schlüssels aus dem Portal
* **Beenden** und Starten des **Datenverwaltungsgateway-Hostdiensts** auf dem Gatewaycomputer
* **Planen von Updates** zu einem bestimmten Zeitpunkt an den Tagen
* Anzeigen des Datums, zu dem das Gateway **zuletzt aktualisiert**wurde

### <a name="settings-page"></a>Seite "Einstellungen"
Auf der Seite „Einstellungen“ können Sie die folgenden Aktionen ausführen:

* Anzeigen, Ändern und Exportieren des **Zertifikats**, das vom Gateway verwendet wird Dieses Zertifikat dient zum Verschlüsseln von Anmeldeinformationen für Datenquellen.
* Ändern des **HTTPS-Ports** für den Endpunkt Das Gateway öffnet einen Port für die Festlegung der Datenquellen-Anmeldeinformationen.
* **Status** des Endpunkts
* **SSL-Zertifikat anzeigen** wird für die SSL-Kommunikation zwischen Portal und Gateway zum Festlegen von Anmeldeinformationen für Datenquellen verwendet.  

### <a name="diagnostics-page"></a>Seite „Diagnose“
Auf der Seite „Diagnose“ können Sie die folgenden Aktionen ausführen:

* Aktivieren der ausführlichen **Protokollierung**, Anzeigen von Protokollen in der Ereignisanzeige und Senden von Protokollen an Microsoft, falls ein Fehler aufgetreten ist
* **Testen der Verbindung** mit einer Datenquelle  

### <a name="help-page"></a>Hilfeseite
Auf der Seite „Hilfe“ werden die folgenden Informationen angezeigt:  

* Kurzbeschreibung des Gateways.
* Versionsnummer
* Links zu Onlinehilfe, Datenschutzbestimmungen und Lizenzvertrag  

## <a name="troubleshooting-gateway-issues"></a>Problembehandlung bei Gateways
Im Artikel [Troubleshoot issues with using Data Management Gateway (Problembehandlung bei der Verwendung des Datenverwaltungsgateways)](data-factory-troubleshoot-gateway-issues.md) finden Sie Informationen/Tipps zur Behandlung von Problemen mit dem Datenverwaltungsgateway.  

## <a name="move-gateway-from-a-machine-to-another"></a>Verschieben eines Gateways von einem Computer auf einen anderen
Dieser Abschnitt enthält Anweisungen zum Verschieben des Gatewayclients von einem Computer auf einen anderen.

1. Wechseln Sie im Portal zur **Data Factory-Startseite**, und klicken Sie auf die Kachel **Verknüpfte Dienste**.

    ![Link „Datengateways“](./media/data-factory-data-management-gateway/DataGatewaysLink.png)
2. Wählen Sie das Gateway im Abschnitt **DATENGATEWAYS** des Blatts **Verknüpfte Dienste** aus.

    ![Blatt „Verknüpfte Dienste“ mit ausgewähltem Gateway](./media/data-factory-data-management-gateway/LinkedServiceBladeWithGateway.png)
3. Klicken Sie auf dem Blatt **Datengateway** auf **Datengateway herunterladen und installieren**.

    ![Link „Gateway herunterladen“](./media/data-factory-data-management-gateway/DownloadGatewayLink.png)
4. Klicken Sie auf dem Blatt **Konfigurieren** auf **Datengateway herunterladen und installieren**, und befolgen Sie die Anweisungen, um das Datengateway auf dem Computer zu installieren.

    ![Blatt „Konfigurieren“](./media/data-factory-data-management-gateway/ConfigureBlade.png)
5. Lassen Sie den **Microsoft-Datenverwaltungsgateway-Konfigurations-Manager** geöffnet.

    ![Konfigurations-Manager](./media/data-factory-data-management-gateway/ConfigurationManager.png)    
6. Klicken Sie im Portal auf dem Blatt **Konfigurieren** auf der Befehlsleiste auf **Schlüssel neu erstellen** und anschließend in der Warnmeldung auf **Ja**. Klicken Sie auf die Schaltfläche **Kopieren** neben dem Schlüsseltext, um den Schlüssel in die Zwischenablage zu kopieren. Sobald Sie den Schlüssel neu erstellen, funktioniert das Gateway auf dem alten Computer nicht mehr.  

    ![Schlüssel neu erstellen](./media/data-factory-data-management-gateway/RecreateKey.png)
7. Fügen Sie auf Ihrem Computer im **Datenverwaltungsgateway-Konfigurations-Manager** auf der Seite **Gateway registrieren** den **Schlüssel** in das Textfeld ein. (Optional) Aktivieren Sie das Kontrollkästchen **Gatewayschlüssel anzeigen**, um den Schlüsseltext anzuzeigen.

    ![Schlüssel kopieren und registrieren](./media/data-factory-data-management-gateway/CopyKeyAndRegister.png)
8. Klicken Sie auf **Registrieren** , um das Gateway beim Clouddienst zu registrieren.
9. Klicken Sie auf der Registerkarte **Einstellungen** auf **Ändern**, um das gleiche Zertifikat auszuwählen, das mit dem alten Gateway verwendet wurde. Geben Sie dann das **Kennwort** ein, und klicken Sie auf **Fertig stellen**.

   ![Zertifikat angeben](./media/data-factory-data-management-gateway/SpecifyCertificate.png)

   Ein Zertifikat des alten Gateways können Sie wie folgt exportieren: Starten Sie auf dem alten Computer den Konfigurations-Manager des Datenverwaltungsgateways, wechseln Sie zur Registerkarte **Zertifikat**, klicken Sie auf **Exportieren**, und befolgen Sie die Anweisungen.
10. Nach erfolgreicher Registrierung des Gateways sollten auf der Startseite des Gateway-Konfigurations-Managers **Registrierung** auf **Registriert** und die Option **Status** auf **Gestartet** festgelegt sein.

## <a name="encrypting-credentials"></a>Verschlüsseln der Anmeldeinformationen
Zum Verschlüsseln der Anmeldeinformationen im Data Factory-Editor gehen Sie wie folgt vor:

1. Starten Sie auf dem **Gatewaycomputer** einen Webbrowser, und navigieren Sie zum [Azure-Portal](http://portal.azure.com). Suchen Sie ggf. nach Ihrer Data Factory, und öffnen Sie die Data Factory auf dem Blatt **DATA FACTORY**. Klicken Sie anschließend auf **Erstellen und bereitstellen**, um den Data Factory-Editor zu starten.   
2. Klicken Sie in der Strukturansicht auf einen vorhandenen **verknüpften Dienst** , um dessen JSON-Definition anzuzeigen, oder erstellen Sie einen verknüpften Dienst, der ein Datenverwaltungsgateway erfordert (etwa SQL Server oder Oracle).
3. Geben Sie im JSON-Editor für die **gatewayName** -Eigenschaft den Namen des Gateways ein.
4. Geben Sie den Servernamen für die **Data Source**-Eigenschaft in **connectionString** ein.
5. Geben Sie den Datenbanknamen für die **Initial Catalog**-Eigenschaft in **connectionString** ein.    
6. Klicken Sie auf der Befehlsleiste auf **Verschlüsseln**, um die ClickOnce-Anwendung **Anmeldeinformationsverwaltung** zu starten. Das Dialogfeld **Anmeldeinformationen festlegen** wird angezeigt.
    ![Dialogfeld „Anmeldeinformationen festlegen“](./media/data-factory-data-management-gateway/setting-credentials-dialog.png)
7. Gehen Sie im Dialogfeld **Anmeldeinformationen festlegen** folgendermaßen vor:  
   1. Wählen Sie die **Authentifizierung** aus, die der Data Factory-Dienst für die Verbindung mit der Datenbank verwenden soll.
   2. Geben Sie für die Einstellung **BENUTZERNAME** den Namen des Benutzers ein, der auf die Datenbank zugreifen kann.
   3. Geben Sie für die Einstellung **KENNWORT** das Kennwort für den Benutzer ein.  
   4. Klicken Sie auf **OK** , um Anmeldeinformationen zu verschlüsseln, und schließen Sie das Dialogfeld.
8. Sie sollten nun in **connectionString** eine **encryptedCredential**-Eigenschaft sehen.        

    ```JSON
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
    ```
Wenn Sie auf das Portal auf einem Computer zugreifen, der sich vom Computer mit dem Gateway unterscheidet, müssen Sie sicherstellen, dass die Anwendung "Anmeldeinformations-Manager" eine Verbindung mit dem Gatewaycomputer herstellen kann. Falls die Anwendung den Computer mit dem Gateway nicht erreichen kann, können Sie keine Anmeldeinformationen für die Datenquelle festlegen und die Verbindung mit der Datenquelle nicht testen.  

Wenn Sie die Anwendung **Anmeldeinformationen festlegen** verwenden, verschlüsselt das Portal die Anmeldeinformationen mit dem Zertifikat, das Sie auf der Registerkarte **Zertifikat** des **Datenverwaltungsgateway-Konfigurations-Managers** auf dem Gatewaycomputer angegeben haben.

Wenn Sie einen API-basierten Ansatz zum Verschlüsseln der Anmeldeinformationen nutzen möchten, können Sie das PowerShell-Cmdlet [New-AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) verwenden, um Anmeldeinformationen zu verschlüsseln. Das Cmdlet verwendet das Zertifikat, für dessen Verwendung dieses Gateway konfiguriert ist, um die Anmeldeinformationen zu verschlüsseln. Sie können dem Element **EncryptedCredential** von **connectionString** in JSON verschlüsselte Anmeldeinformationen hinzufügen. Verwenden Sie JSON mit dem [New-AzureRmDataFactoryLinkedService](https://msdn.microsoft.com/library/mt603647.aspx) -Cmdlet oder im Data Factory-Editor.

```JSON
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```

Es gibt eine weitere Möglichkeit zum Festlegen der Anmeldeinformationen mit dem Data Factory-Editor. Wenn Sie einen verknüpften SQL Server-Dienst mit dem Editor erstellen und Anmeldeinformationen im Nur-Text-Format eingeben, werden die Anmeldeinformationen mit einem Zertifikat verschlüsselt, das der Data Factory-Dienst besitzt. Es wird NICHT das Zertifikat verwendet, für dessen Verwendung dieses Gateway konfiguriert ist. Dieser Ansatz ist möglicherweise etwas schneller, in einigen Fällen aber weniger sicher. Daher empfehlen wir, dass Sie diesen Ansatz nur für die Entwicklung/Tests verwenden.

## <a name="powershell-cmdlets"></a>PowerShell-Cmdlets
Dieser Abschnitt beschreibt das Erstellen und Registrieren eines Gateways mit Azure PowerShell-Cmdlets.

1. Starten Sie **Azure PowerShell** im Administratormodus.
2. Melden Sie sich bei Ihrem Azure-Konto an, indem Sie den folgenden Befehl ausführen und Ihre Azure-Anmeldeinformationen eingeben.

    ```PowerShell
    Login-AzureRmAccount
    ```
3. Verwenden Sie das **New-AzureRmDataFactoryGateway** -Cmdlet, um wie folgt ein logisches Gateway zu erstellen:

    ```PowerShell
    $MyDMG = New-AzureRmDataFactoryGateway -Name <gatewayName> -DataFactoryName <dataFactoryName> -ResourceGroupName ADF –Description <desc>
    ```
    **Beispiel für eine Befehlszeile und Ausgabe**:

    ```
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
    ```

1. Wechseln Sie in Azure PowerShell zum Ordner **C:\Programme\Microsoft Data Management Gateway\2.0\PowerShellScript\**. Führen Sie **RegisterGateway.ps1** mit der lokalen Variable **$Key** aus, wie im folgenden Befehl gezeigt. Dieses Skript registriert den auf dem Computer installierten Client-Agent bei dem logischen Gateway, das Sie zuvor erstellt haben.

    ```PowerShell
    PS C:\> .\RegisterGateway.ps1 $MyDMG.Key
    ```
    ```
    Agent registration is successful!
    ```
    Sie können das Gateway auf einem Remotecomputer registrieren, indem Sie den Parameter „IsRegisterOnRemoteMachine“ verwenden. Beispiel:

    ```PowerShell
    .\RegisterGateway.ps1 $MyDMG.Key -IsRegisterOnRemoteMachine true
    ```
2. Mit dem **Get-AzureRmDataFactoryGateway** -Cmdlet können Sie die Liste mit Gateways in Ihrer Data Factory abrufen. Wenn der **Status** **online**angezeigt wird, bedeutet dies, dass Ihr Gateway einsatzbereit ist.

    ```PowerShell        
    Get-AzureRmDataFactoryGateway -DataFactoryName <dataFactoryName> -ResourceGroupName ADF
    ```
Sie können ein Gateway mit dem **Remove-AzureRmDataFactoryGateway**-Cmdlet entfernen und die Beschreibung für das Gateway mithilfe der **Set-AzureRmDataFactoryGateway**-Cmdlets aktualisieren. Syntax und andere Details zu diesen Cmdlets finden Sie in der Data Factory-Cmdlet-Referenz.  

### <a name="list-gateways-using-powershell"></a>Auflisten von Gateways mit PowerShell

```PowerShell
Get-AzureRmDataFactoryGateway -DataFactoryName jasoncopyusingstoredprocedure -ResourceGroupName ADF_ResourceGroup
```

### <a name="remove-gateway-using-powershell"></a>Entfernen von Gateways mit PowerShell

```PowerShell
Remove-AzureRmDataFactoryGateway -Name JasonHDMG_byPSRemote -ResourceGroupName ADF_ResourceGroup -DataFactoryName jasoncopyusingstoredprocedure -Force
```


## <a name="next-steps"></a>Nächste Schritte
* Informationen finden Sie unter [Verschieben von Daten zwischen lokalen Quellen und der Cloud mit dem Datenverwaltungsgateway](data-factory-move-data-between-onprem-and-cloud.md) . In der exemplarischen Vorgehensweise erstellen Sie eine Pipeline, die das Gateway verwendet, um Daten aus einer lokalen SQL Server-Datenbank in einen Azure-Blob zu verschieben.  

