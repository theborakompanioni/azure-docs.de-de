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
ms.date: 07/27/2017
ms.author: abnarain
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 9e40eba285aeb1cce6b77311d1b69a6b96967a0b
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---
# <a name="data-management-gateway"></a>Gateway zur Datenverwaltung
Das Datenverwaltungsgateway ist ein Client-Agent, den Sie in Ihrer lokalen Umgebung installieren müssen, um Daten zwischen Ihren Cloudspeichern und lokalen Datenspeichern kopieren zu können. Die lokalen von Data Factory unterstützten Datenspeicher werden im Abschnitt [Unterstützte Datenquellen](data-factory-data-movement-activities.md#supported-data-stores-and-formats) aufgeführt.

Dieser Artikel ergänzt die exemplarische Vorgehensweise im Artikel [Verschieben von Daten zwischen lokalen Datenspeichern und Clouddatenspeichern](data-factory-move-data-between-onprem-and-cloud.md) . In der exemplarischen Vorgehensweise erstellen Sie eine Pipeline, die das Gateway verwendet, um Daten aus einer lokalen SQL Server-Datenbank in einen Azure-Blob zu verschieben. Dieser Artikel enthält ausführliche Informationen zur Verwendung des Datenverwaltungsgateways. 

Sie können ein Datenverwaltungsgateway horizontal hochskalieren, indem Sie ihm mehrere lokale Computer zuordnen. Sie können zentral hochskalieren, indem Sie die Anzahl von Datenverschiebungsaufträgen erhöhen, die auf einem Knoten gleichzeitig ausgeführt werden können. Diese Funktion ist auch für ein logisches Gateway mit einem einzelnen Knoten verfügbar. Ausführliche Informationen hierzu finden Sie im Artikel [Data Management Gateway – high availability and scalability (Preview)](data-factory-data-management-gateway-high-availability-scalability.md) (Datenverwaltungsgateway – Hohe Verfügbarkeit und Skalierbarkeit (Vorschauversion)).

> [!NOTE]
> Das Gateway unterstützt derzeit nur die Kopieraktivität und die Aktivität mit gespeicherten Prozeduren in Data Factory. Das Gateway kann nicht über eine benutzerdefinierte Aktivität verwendet werden, um auf lokale Datenquellen zuzugreifen.      

## <a name="overview"></a>Übersicht
### <a name="capabilities-of-data-management-gateway"></a>Funktionen des Datenverwaltungsgateways
Das Datenverwaltungsgateway bietet die folgenden Funktionen:

* Modellieren von lokalen Datenquellen und Clouddatenquellen innerhalb einer Data Factory und Verschieben von Daten.
* Nutzen einer zentralen Konsole für die Überwachung und Verwaltung mit Informationen zum Gatewaystatus über die Data Factory-Seite.
* Sicheres Verwalten des Zugriffs auf lokale Datenquellen.
  * An der Unternehmensfirewall ist keine Änderung erforderlich. Das Gateway stellt nur ausgehende HTTP-basierte Verbindungen mit dem offenen Internet her.
  * Verschlüsseln Sie die Anmeldeinformationen für Ihre lokalen Datenspeicher mit dem Zertifikat.
* Effizientes Verschieben von Daten – Daten werden parallel verschoben, und zwar mit Ausfallsicherheit bei vorübergehenden Netzwerkproblemen dank automatischer Wiederholungslogik.

### <a name="command-flow-and-data-flow"></a>Befehls- und Datenfluss
Wenn Sie Daten mithilfe einer Kopieraktivität zwischen der lokalen Umgebung und der Cloud kopieren, verwendet diese Aktivität ein Gateway, um die Daten aus einer lokalen Datenquelle in die Cloud und umgekehrt zu übertragen.

Hier ist eine Darstellung des allgemeinen Datenflusses sowie eine Zusammenfassung der Schritte für das Kopieren mit dem Datengateway angegeben: ![Datenfluss über ein Gateway](./media/data-factory-data-management-gateway/data-flow-using-gateway.png)

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
* Sie müssen ein Administrator auf dem Computer sein, um das Datenverwaltungsgateway erfolgreich installieren und konfigurieren zu können. Sie können der lokalen Windows-Gruppe **Datenverwaltungsgateway-Benutzer** zusätzliche Benutzer hinzufügen. Die Mitglieder dieser Gruppe können das Gateway mithilfe des Tools **Datenverwaltungsgateway-Konfigurations-Manager** konfigurieren.

Da die Kopieraktivität mit einer bestimmten Häufigkeit ausgeführt wird, folgt die Ressourcenverwendung (CPU, Arbeitsspeicher) auf dem Computer dem gleichen Muster mit Spitzen- und Leerlaufzeiten. Die Ressourcenverwendung hängt auch stark von der Datenmenge ab, die verschoben wird. Wenn mehrere Kopieraufträge in Bearbeitung sind, steigt die Ressourcenverwendung zu Spitzenzeiten an.

### <a name="installation-options"></a>Installationsoptionen
Das Datenverwaltungsgateway kann auf folgende Weise installiert werden:

* Durch Herunterladen eines MSI-Setuppakets aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717).  Die MSI-Datei kann auch verwendet werden, um ein vorhandenes Datenverwaltungsgateway auf die neueste Version zu aktualisieren, wobei alle Einstellungen beibehalten werden.
* Durch Klicken auf den Link **Datengateway herunterladen und installieren** unter MANUELLES SETUP oder **Direkt auf diesem Computer installieren** unter EXPRESS-SETUP. Im Artikel [Verschieben von Daten zwischen lokalen Quellen und der Cloud](data-factory-move-data-between-onprem-and-cloud.md) finden Sie schrittweise Anweisungen zur Verwendung des Express-Setups. Durch den manuellen Schritt gelangen Sie zum Download Center.  Wie Sie das Gateway aus dem Download Center herunterladen und installieren, erfahren Sie im nächsten Abschnitt.

### <a name="installation-best-practices"></a>Bewährte Methoden für die Installation:
1. Konfigurieren Sie den Energiesparplan auf dem Hostcomputer für das Gateway, sodass der Computer nicht in den Ruhezustand wechselt. Wenn sich der Hostcomputer im Ruhezustand befindet, reagiert das Gateway nicht auf Datenanforderungen.
2. Sichern Sie das Zertifikat, das dem Gateway zugeordnet ist.

### <a name="install-the-gateway-from-download-center"></a>Installieren des Gateways aus dem Download Center
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
Um im Portal ein Gateway zu erstellen und auf der Seite **Konfigurieren** den Schlüssel zu ermitteln, führen Sie die Schritte der exemplarischen Vorgehensweise aus dem Artikel [Verschieben von Daten zwischen lokalen Quellen und der Cloud](data-factory-move-data-between-onprem-and-cloud.md) aus.    

#### <a name="if-you-have-already-created-the-logical-gateway-in-the-portal"></a>Wenn Sie bereits ein logisches Gateway im Portal erstellt haben
1. Wechseln Sie im Azure-Portal zur Seite **Data Factory**, und klicken Sie auf die Kachel **Verknüpfte Dienste**.

    ![Seite „Data Factory“](media/data-factory-data-management-gateway/data-factory-blade.png)
2. Wählen Sie auf der Seite **Verknüpfte Dienste** das logische **Gateway** aus, das Sie im Portal erstellt haben.

    ![Logisches Gateway](media/data-factory-data-management-gateway/data-factory-select-gateway.png)  
3. Klicken Sie auf der Seite **Datengateway** auf **Datengateway herunterladen und installieren**.

    ![Link zum Herunterladen im Portal](media/data-factory-data-management-gateway/download-and-install-link-on-portal.png)   
4. Klicken Sie auf der Seite **Konfigurieren** auf **Schlüssel neu erstellen**. Klicken Sie in der Warnmeldung auf „Ja“, nachdem Sie sie sorgfältig gelesen haben.

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
> 1. Basierend auf Ihrer Datenquelle und den Senken müssen Sie unter Umständen der Whitelist Ihrer Unternehmens-/Windows-Firewall zusätzliche Domänen und ausgehende Ports hinzufügen.
> 2. Für einige Clouddatenbanken (z.B. [Azure SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-configure-firewall-settings), [Azure Data Lake](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-secure-data#set-ip-address-range-for-data-access) usw.) müssen Sie die IP-Adresse des Gatewaycomputers für die Firewallkonfiguration ggf. auf eine Whitelist setzen.
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

1. Starten Sie den **Datenverwaltungsgateway-Konfigurations-Manager**.
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

1. Wenn Sie versuchen, das Gateway zu registrieren, wird die folgende Fehlermeldung angezeigt: „Fehler beim Registrieren des Gatewayschlüssels. Prüfen Sie, ob sich das Datenverwaltungsgateway im Status „Verbunden“ befindet und der Datenverwaltungsgateway-Hostdienst gestartet wurde, bevor Sie versuchen, den Gatewayschlüssel erneut zu registrieren.“
2. Wenn Sie den Konfigurations-Manager öffnen, wird der Status als „Getrennt“ oder „Verbindung wird hergestellt“ angezeigt. Wenn Sie Windows-Ereignisprotokolle anzeigen, sehen Sie unter „Ereignisanzeige“ > „Anwendungs- und Dienstprotokolle“ > „Datenverwaltungsgateway“ Fehlermeldungen wie die folgende: `Unable to connect to the remote server`
   `A component of Data Management Gateway has become unresponsive and restarts automatically. Component name: Gateway.`

### <a name="open-port-8050-for-credential-encryption"></a>Öffnen von Port 8050 für die Verschlüsselung der Anmeldeinformationen
Die Anwendung **Anmeldeinformationen festlegen** verwendet den eingehenden Port **8050**, um die Anmeldeinformationen an das Gateway weiterzugeben, wenn Sie im Azure-Portal einen lokalen verknüpften Dienst einrichten. Während der Einrichtung des Gateways wird es von der Gatewayinstallation standardmäßig auf dem Gatewaycomputer geöffnet.

Bei Verwendung einer Drittanbieterfirewall können Sie den Port 8050 manuell öffnen. Falls beim Einrichten des Gateways ein Firewallproblem auftritt, können Sie versuchen, den folgenden Befehl zu verwenden, um das Gateway ohne Konfiguration der Firewall zu installieren.

    msiexec /q /i DataManagementGateway.msi NOFIREWALL=1

Falls Sie den Port 8050 auf dem Gatewaycomputer nicht öffnen, verwenden Sie andere Verfahren als die Anwendung **Anmeldeinformationen festlegen** , um Anmeldeinformationen für den Datenspeicher zu konfigurieren. Beispielsweise können Sie das PowerShell-Cmdlet [New-AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) verwenden. Informationen zum Festlegen von Datenspeicher-Anmeldeinformationen finden Sie unter [Festlegen von Anmeldeinformationen und Sicherheit](#set-credentials-and-securityy) .

## <a name="update"></a>Aktualisieren
Standardmäßig wird das Datenverwaltungsgateway automatisch aktualisiert, wenn eine neuere Version des Gateways verfügbar ist. Das Gateway wird erst aktualisiert, nachdem alle geplanten Aufgaben erledigt wurden. Vom Gateway werden keine weiteren Aufgaben verarbeitet, bevor der Aktualisierungsvorgang abgeschlossen ist. Wenn die Aktualisierung fehlschlägt, wird für das Gateway ein Rollback auf die alte Version durchgeführt.

Sie sehen die geplante Aktualisierungszeit an folgenden Stellen:

* Auf der Seite mit den Gatewayeigenschaften im Azure-Portal.
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

[Für Gateways mit einem Knoten]
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
[[Für hoch verfügbare und skalierbare Gateways mit mehreren Knoten (Vorschauversion)](data-factory-data-management-gateway-high-availability-scalability.md)]
1. Starten Sie Windows PowerShell auf dem Gatewaycomputer.
2. Wechseln Sie in den Ordner „C:\Programme\Microsoft Data Management Gateway\2.0\PowerShellScript“.
3. Führen Sie den folgenden Befehl aus, um das Feature für die automatische Aktualisierung zu deaktivieren.   

    Für ein Gateway mit hoher Verfügbarkeit (Vorschauversion) ist ein zusätzlicher AuthKey-Parameter erforderlich.
    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -off -AuthKey <your auth key>
    ```
4. So aktivieren Sie das Feature wieder

    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -on -AuthKey <your auth key> 


## Configuration Manager
Once you install the gateway, you can launch Data Management Gateway Configuration Manager in one of the following ways:

1. In the **Search** window, type **Data Management Gateway** to access this utility.
2. Run the executable **ConfigManager.exe** in the folder: **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared**

### Home page
The Home page allows you to do the following actions:

* View status of the gateway (connected to the cloud service etc.).
* **Register** using a key from the portal.
* **Stop** and start the **Data Management Gateway Host service** on the gateway machine.
* **Schedule updates** at a specific time of the days.
* View the date when the gateway was **last updated**.

### Settings page
The Settings page allows you to do the following actions:

* View, change, and export **certificate** used by the gateway. This certificate is used to encrypt data source credentials.
* Change **HTTPS port** for the endpoint. The gateway opens a port for setting the data source credentials.
* **Status** of the endpoint
* View **SSL certificate** is used for SSL communication between portal and the gateway to set credentials for data sources.  

### Diagnostics page
The Diagnostics page allows you to do the following actions:

* Enable verbose **logging**, view logs in event viewer, and send logs to Microsoft if there was a failure.
* **Test connection** to a data source.  

### Help page
The Help page displays the following information:  

* Brief description of the gateway
* Version number
* Links to online help, privacy statement, and license agreement.  

## Monitor gateway in the portal
In the Azure portal, you can view near-real time snapshot of resource utilization (CPU, memory, network(in/out), etc.) on a gateway machine.  

1. In Azure portal, navigate to the home page for your data factory, and click **Linked services** tile. 

    ![Data factory home page](./media/data-factory-data-management-gateway/monitor-data-factory-home-page.png) 
2. Select the **gateway** in the **Linked services** page.

    ![Linked services page](./media/data-factory-data-management-gateway/monitor-linked-services-blade.png)
3. In the **Gateway** page, you can see the memory and CPU usage of the gateway.

    ![CPU and memory usage of gateway](./media/data-factory-data-management-gateway/gateway-simple-monitoring.png) 
4. Enable **Advanced settings** to see more details such as network usage.
    
    ![Advanced monitoring of gateway](./media/data-factory-data-management-gateway/gateway-advanced-monitoring.png)

The following table provides descriptions of columns in the **Gateway Nodes** list:  

Monitoring Property | Description
:------------------ | :---------- 
Name | Name of the logical gateway and nodes associated with the gateway. Node is an on-premises Windows machine that has the gateway installed on it. For information on having more than one node (up to four nodes) in a single logical gateway, see [Data Management Gateway - high availability and scalability](data-factory-data-management-gateway-high-availability-scalability.md).    
Status | Status of the logical gateway and the gateway nodes. Example: Online/Offline/Limited/etc. For information about these statuses, See [Gateway status](#gateway-status) section. 
Version | Shows the version of the logical gateway and each gateway node. The version of the logical gateway is determined based on version of majority of nodes in the group. If there are nodes with different versions in the logical gateway setup, only the nodes with the same version number as the logical gateway function properly. Others are in the limited mode and need to be manually updated (only in case auto-update fails). 
Available memory | Available memory on a gateway node. This value is a near real-time snapshot. 
CPU utilization | CPU utilization of a gateway node. This value is a near real-time snapshot. 
Networking (In/Out) | Network utilization of a gateway node. This value is a near real-time snapshot. 
Concurrent Jobs (Running/ Limit) | Number of jobs or tasks running on each node. This value is a near real-time snapshot. Limit signifies the maximum concurrent jobs for each node. This value is defined based on the machine size. You can increase the limit to scale up concurrent job execution in advanced scenarios, where CPU/memory/network is under-utilized, but activities are timing out. This capability is also available with a single-node gateway (even when the scalability and availability feature is not enabled).  
Role | There are two types of roles in a multi-node gateway – Dispatcher and worker. All nodes are workers, which means they can all be used to execute jobs. There is only one dispatcher node, which is used to pull tasks/jobs from cloud services and dispatch them to different worker nodes (including itself).

In this page, you see some settings that make more sense when there are two or more nodes (scale out scenario) in the gateway. See [Data Management Gateway - high availability and scalability](data-factory-data-management-gateway-high-availability-scalability.md) for details about setting up a multi-node gateway.

### Gateway status
The following table provides possible statuses of a **gateway node**: 

Status  | Comments/Scenarios
:------- | :------------------
Online | Node connected to Data Factory service.
Offline | Node is offline.
Upgrading | The node is being auto-updated.
Limited | Due to Connectivity issue. May be due to HTTP port 8050 issue, service bus connectivity issue, or credential sync issue. 
Inactive | Node is in a configuration different from the configuration of other majority nodes.<br/><br/> A node can be inactive when it cannot connect to other nodes. 


The following table provides possible statuses of a **logical gateway**. The gateway status depends on statuses of the gateway nodes. 

Status | Comments
:----- | :-------
Needs Registration | No node is yet registered to this logical gateway
Online | Gateway Nodes are online
Offline | No node in online status.
Limited | Not all nodes in this gateway are in healthy state. This status is a warning that some node might be down! <br/><br/>Could be due to credential sync issue on dispatcher/worker node. 

## Scale up gateway
You can configure the number of **concurrent data movement jobs** that can run on a node to scale up the capability of moving data between on-premises and cloud data stores. 

When the available memory and CPU are not utilized well, but the idle capacity is 0, you should scale up by increasing the number of concurrent jobs that can run on a node. You may also want to scale up when activities are timing out because the gateway is overloaded. In the advanced settings of a gateway node, you can increase the maximum capacity for a node. 
  

## Troubleshooting gateway issues
See [Troubleshooting gateway issues](data-factory-troubleshoot-gateway-issues.md) article for information/tips for troubleshooting issues with using the data management gateway.  

## Move gateway from one machine to another
This section provides steps for moving gateway client from one machine to another machine.

1. In the portal, navigate to the **Data Factory home page**, and click the **Linked Services** tile.

    ![Data Gateways Link](./media/data-factory-data-management-gateway/DataGatewaysLink.png)
2. Select your gateway in the **DATA GATEWAYS** section of the **Linked Services** page.

    ![Linked Services page with gateway selected](./media/data-factory-data-management-gateway/LinkedServiceBladeWithGateway.png)
3. In the **Data gateway** page, click **Download and install data gateway**.

    ![Download gateway link](./media/data-factory-data-management-gateway/DownloadGatewayLink.png)
4. In the **Configure** page, click **Download and install data gateway**, and follow instructions to install the data gateway on the machine.

    ![Configure page](./media/data-factory-data-management-gateway/ConfigureBlade.png)
5. Keep the **Microsoft Data Management Gateway Configuration Manager** open.

    ![Configuration Manager](./media/data-factory-data-management-gateway/ConfigurationManager.png)    
6. In the **Configure** page in the portal, click **Recreate key** on the command bar, and click **Yes** for the warning message. Click **copy button** next to key text that copies the key to the clipboard. The gateway on the old machine stops functioning as soon you recreate the key.  

    ![Recreate key](./media/data-factory-data-management-gateway/RecreateKey.png)
7. Paste the **key** into text box in the **Register Gateway** page of the **Data Management Gateway Configuration Manager** on your machine. (optional) Click **Show gateway key** check box to see the key text.

    ![Copy key and Register](./media/data-factory-data-management-gateway/CopyKeyAndRegister.png)
8. Click **Register** to register the gateway with the cloud service.
9. On the **Settings** tab, click **Change** to select the same certificate that was used with the old gateway, enter the **password**, and click **Finish**.

   ![Specify Certificate](./media/data-factory-data-management-gateway/SpecifyCertificate.png)

   You can export a certificate from the old gateway by doing the following steps: launch Data Management Gateway Configuration Manager on the old machine, switch to the **Certificate** tab, click **Export** button and follow the instructions.
10. After successful registration of the gateway, you should see the **Registration** set to **Registered** and **Status** set to **Started** on the Home page of the Gateway Configuration Manager.

## Encrypting credentials
To encrypt credentials in the Data Factory Editor, do the following steps:

1. Launch web browser on the **gateway machine**, navigate to [Azure portal](http://portal.azure.com). Search for your data factory if needed, open data factory in the **DATA FACTORY** page and then click **Author & Deploy** to launch Data Factory Editor.   
2. Click an existing **linked service** in the tree view to see its JSON definition or create a linked service that requires a data management gateway (for example: SQL Server or Oracle).
3. In the JSON editor, for the **gatewayName** property, enter the name of the gateway.
4. Enter server name for the **Data Source** property in the **connectionString**.
5. Enter database name for the **Initial Catalog** property in the **connectionString**.    
6. Click **Encrypt** button on the command bar that launches the click-once **Credential Manager** application. You should see the **Setting Credentials** dialog box.

    ![Setting credentials dialog](./media/data-factory-data-management-gateway/setting-credentials-dialog.png)
7. In the **Setting Credentials** dialog box, do the following steps:
   1. Select **authentication** that you want the Data Factory service to use to connect to the database.
   2. Enter name of the user who has access to the database for the **USERNAME** setting.
   3. Enter password for the user for the **PASSWORD** setting.  
   4. Click **OK** to encrypt credentials and close the dialog box.
8. You should see a **encryptedCredential** property in the **connectionString** now.

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

