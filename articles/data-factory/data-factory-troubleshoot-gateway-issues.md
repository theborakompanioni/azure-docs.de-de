---
title: "Problembehandlung für das Datenverwaltungsgateway | Microsoft Docs"
description: Hier finden Sie Tipps zur Problembehandlung im Zusammenhang mit dem Datenverwaltungsgateway.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: c6756c37-4e5a-4d1e-ab52-365f149b4128
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2016
ms.author: jingwang
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 4619716238d77caf494c116266c60e7e0c86ca92


---
# <a name="troubleshoot-issues-with-using-data-management-gateway"></a>Behandeln von Problemen bei der Verwendung des Datenverwaltungsgateways
Dieser Artikel enthält Informationen zur Problembehandlung bei der Verwendung des Datenverwaltungsgateways. 

> [!NOTE]
> Ausführliche Informationen zum Gateway finden Sie im Artikel [Datenverwaltungsgateway](data-factory-data-management-gateway.md). Im Artikel [Verschieben von Daten zwischen lokalen Quellen und der Cloud mit dem Datenverwaltungsgateway](data-factory-move-data-between-onprem-and-cloud.md) finden Sie eine exemplarische Vorgehensweise zum Verschieben von Daten aus einer lokalen SQL Server-Datenbank in einen Azure-Blobspeicher mithilfe des Gateways. 
> 
> 

## <a name="failed-to-install-or-register-gateway"></a>Fehler beim Installieren oder Registrieren des Gateways
### <a name="1-problem"></a>1. Problem
Diese Fehlermeldung wird beim Installieren/Registrieren eines Gateways angezeigt, insbesondere beim Herunterladen der Installationsdatei für das Gateway. 

`Unable to connect to the remote server". Please check your local settings (Error Code: 10003).`

### <a name="cause"></a>Ursache
Auf dem Computer, auf dem das Gateway installiert werden soll, konnte die aktuelle Gateway-Installationsdatei aufgrund eines Netzwerkfehlers nicht aus dem Download Center heruntergeladen werden.

### <a name="resolution"></a>Lösung
Überprüfen Sie die Firewall-/Proxyservereinstellungen, um festzustellen, ob die Einstellungen die Netzwerkverbindung zwischen dem Computer und dem [Download Center](https://download.microsoft.com/) blockieren, und aktualisieren Sie sie ggf. entsprechend. 

Alternativ können Sie die Installationsdatei für das aktuelle Gateway aus dem [Download Center](https://www.microsoft.com/en-US/download/details.aspx?id=39717) auf andere Computer herunterladen, die auf das Download Center zugreifen können, die Installationsdatei auf den Hostcomputer für das Gateway kopieren und sie dann manuell ausführen, um das Gateway zu installieren/aktualisieren.

### <a name="2-problem"></a>2. Problem
Dieser Fehler wird angezeigt, wenn Sie versuchen, ein Gateway zu installieren, indem Sie im Azure-Portal auf „Direkt auf diesem Computer installieren“ klicken. 

`Error:  Abort installing a new gateway on this computer because this computer has an existing installed gateway and a computer without any installed gateway is required for installing a new gateway.`  

### <a name="cause"></a>Ursache
Auf dem Computer wurde bereits ein Gateway installiert. 

### <a name="resolution"></a>Lösung
Deinstallieren Sie das vorhandene Gateway auf dem Computer, und klicken Sie erneut auf den Link „Direkt auf diesem Computer installieren“.

### <a name="3-problem"></a>3. Problem
Dieser Fehler kann angezeigt werden, wenn Sie ein neues Gateway registrieren.

`Error: The gateway has encountered an error during registration.`

### <a name="cause"></a>Ursache
Diese Nachricht wird aus einem der folgenden Gründe angezeigt:

* Das Format des Gatewayschlüssels ist ungültig.
* Der Gatewayschlüssel ist ungültig geworden.
* Der Gatewayschlüssel wurde vom Portal erneut generiert.  

### <a name="resolution"></a>Lösung
Überprüfen Sie, ob Sie den richtigen Gatewayschlüssel aus dem Azure-Portal verwenden. Generieren Sie ggf. den Schlüssel erneut, und registrieren Sie das Gateway mit dem Schlüssel.

### <a name="4-problem"></a>4. Problem
Möglicherweise wird beim Registrieren eines Gateways die folgende Fehlermeldung angezeigt: 

`Error: The content or format of the gateway key "{gatewayKey}" is invalid, please go to azure portal to create one new gateway or regenerate the gateway key.`

Der folgende Screenshot zeigt ein Beispiel:

![Inhalt oder Format des Schlüssels ist ungültig](media/data-factory-troubleshoot-gateway-issues/invalid-format-gateway-key.png)

### <a name="cause"></a>Ursache
Der Inhalt oder das Format des eingegebenen Gatewayschlüssels ist falsch. Mögliche Gründe sind, dass Sie nur einen Teil des Texts aus dem Portal kopiert haben oder einen ungültigen Schlüssel verwenden.

### <a name="resolution"></a>Lösung
Generieren Sie einen Gatewayschlüssel im Azure-Portal, und verwenden Sie die Schaltfläche „Kopieren“, um den gesamten Schlüssel zu kopieren. Fügen Sie ihn dann in diesem Fenster ein, um das Gateway zu registrieren.

### <a name="5-problem"></a>5. Problem
Möglicherweise wird beim Registrieren eines Gateways die folgende Fehlermeldung angezeigt:

`Error: The gateway key is invalid or empty. Specify a valid gateway key from the portal.`

![Gatewayschlüssel ist ungültig oder leer](media/data-factory-troubleshoot-gateway-issues/gateway-key-is-invalid-or-empty.png)

### <a name="cause"></a>Ursache
Der Gatewayschlüssel wurde neu generiert, oder das Gateway wurde im Azure-Portal gelöscht.

### <a name="resolution"></a>Lösung
Wenn das Gateway noch vorhanden ist, generieren Sie einen Gatewayschlüssel im Azure-Portal, und verwenden Sie die Schaltfläche „Kopieren“, um den gesamten Schlüssel zu kopieren. Fügen Sie ihn dann in diesem Fenster ein, um das Gateway zu registrieren. Andernfalls erstellen Sie das Gateway neu, und beginnen Sie erneut.

### <a name="6-problem"></a>6. Problem
Möglicherweise wird beim Registrieren eines Gateways die folgende Fehlermeldung angezeigt:

`Error: Gateway has been online for a while, then shows “Gateway is not registered” with the status “Gateway key is invalid”`

![Gatewayschlüssel ist ungültig oder leer](media/data-factory-troubleshoot-gateway-issues/gateway-not-registered-key-invalid.png)

### <a name="cause"></a>Ursache
Dieser Fehler kann auftreten, wenn das Gateway gelöscht wurde oder der zugehörige Gatewayschlüssel neu generiert wurde.

### <a name="resolution"></a>Lösung
Wenn das Gateway gelöscht wurde, erstellen Sie das Gateway über das Portal erneut, klicken Sie auf „Registrieren“, kopieren Sie den Schlüssel aus dem Portal, fügen Sie ihn ein, und wiederholen Sie das Registrieren des Gateways.

Wenn das Gateway noch vorhanden ist, aber der Schlüssel neu generiert wurde, verwenden Sie den neuen Schlüssel, um das Gateway zu registrieren. Wenn Sie nicht über den Schlüssel verfügen, generieren Sie den Schlüssel erneut über das Portal. 

### <a name="7-problem"></a>7. Problem
Wenn Sie ein Gateway registrieren, müssen Sie möglicherweise den Pfad und das Kennwort für ein Zertifikat angeben. 

![Zertifikat angeben](media/data-factory-troubleshoot-gateway-issues/specify-certificate.png)

### <a name="cause"></a>Ursache
Das Gateway wurde zuvor auf anderen Computern registriert. Bei der ersten Registrierung eines Gateways wurde ihm ein Verschlüsselungszertifikat zugeordnet. Das Zertifikat kann durch das Gateway selbst generiert oder vom Benutzer bereitgestellt werden.  Mit diesem Zertifikat werden die Anmeldeinformationen des Datenspeichers (verknüpfter Dienst) verschlüsselt.  

![Zertifikat exportieren](media/data-factory-troubleshoot-gateway-issues/export-certificate.png)

Beim Wiederherstellen des Gateways auf einem anderen Hostcomputer fragt der Registrierungsassistent nach diesem Zertifikat zum Entschlüsseln von Anmeldeinformationen, die zuvor mit diesem Zertifikat verschlüsselt wurden.  Ohne dieses Zertifikat können die Anmeldeinformationen nicht vom neuen Gateway entschlüsselt werden, und das Ausführen von nachfolgenden Kopiervorgängen im Zusammenhang mit diesem neuen Gateway führt zu Fehlern.  

### <a name="resolution"></a>Lösung
Wenn Sie das Zertifikat für die Anmeldeinformationen vom ursprünglichen Gatewaycomputer über die Schaltfläche „Exportieren“ der Registerkarte **Einstellungen** des **Gateway-Konfigurations-Managers** exportiert haben, verwenden Sie das Zertifikat hier.

Sie können diesen Schritt beim Wiederherstellen eines Gateways nicht überspringen. Wenn das Zertifikat nicht vorhanden ist, müssen Sie das Gateway aus dem Portal löschen und ein neues Gateway erstellen.  Darüber hinaus müssen alle verknüpften Dienste im Zusammenhang mit dem Gateway aktualisiert werden, indem Sie ihre Anmeldeinformationen eingeben. 

### <a name="8-problem"></a>8. Problem
Möglicherweise erhalten Sie die folgende Fehlermeldung: 

`Error: The remote server returned an error: (407) Proxy Authentication Required.`

### <a name="cause"></a>Ursache
Dieser Fehler tritt auf, wenn das Gateway sich in einer Umgebung befindet, in der ein HTTP-Proxy für den Zugriff auf Internetressourcen benötigt wird, oder wenn das Authentifizierungskennwort des Proxys geändert, aber nicht entsprechend im Gateway aktualisiert wurde. 

### <a name="resolution"></a>Lösung
Führen Sie die Anweisungen im Abschnitt [Proxyserver-Aspekte](#proxy-server-considerations) in diesem Dokument aus, und konfigurieren Sie die Proxyeinstellungen mit dem Konfigurations-Manager.

## <a name="gateway-is-online-with-limited-functionality"></a>Gateway ist online mit eingeschränkter Funktionalität
### <a name="1-problem"></a>1. Problem
Als Status des Gateways wird **online mit eingeschränkter Funktionalität** angezeigt. 

### <a name="cause"></a>Ursache
Sie sehen, dass der Status des Gateways aus einem der folgenden Gründe als online mit eingeschränkter Funktionalität angezeigt wird:

* Das Gateway kann nicht über den Servicebus eine Verbindung mit dem Clouddienst herstellen.
* Der Clouddienst kann nicht über den Servicebus eine Verbindung mit dem Gateway herstellen.

Wenn ein Gateway mit eingeschränkter Funktionalität online ist, können Sie möglicherweise nicht mithilfe des Assistenten zum Kopieren von Data Factory Datenpipelines zum Kopieren von Daten in einen bzw. aus einem lokalen Datenspeicher erstellen. Um dieses Problem zu umgehen, können Sie den Data Factory-Editor in Azure-Portal (oder) Visual Studio (oder) Azure PowerShell verwenden.

### <a name="resolution"></a>Lösung
Die Lösung dieses Problems (online mit eingeschränkter Funktionalität) hängt davon ab, ob das Gateway nicht mit dem Clouddienst oder auf andere Art und Weise eine Verbindung herstellen kann. In den folgenden Abschnitten werden diese Lösungen beschrieben. 

### <a name="2-problem"></a>2. Problem
Sie erhalten den folgenden Fehler: 

`Error: Gateway cannot connect to cloud service through service bus`

![Das Gateway kann keine Verbindung mit dem Clouddienst herstellen.](media/data-factory-troubleshoot-gateway-issues/gateway-cannot-connect-to-cloud-service.png)

### <a name="cause"></a>Ursache
Das Gateway kann nicht über den Servicebus eine Verbindung mit dem Clouddienst herstellen.

### <a name="resolution"></a>Lösung
Um das Gateway wieder online zu schalten, gehen Sie wie folgt vor:

* Lassen Sie Regeln für ausgehende IP-Adressen auf dem Gatewaycomputer und in der Unternehmensfirewall zu. Sie finden die IP-Adressen im Windows-Ereignisprotokoll (ID == 401): Der Zugriff auf einen Socket war aufgrund der Zugriffsrechte des Sockets unzulässig XX.XX.XX.XX:9350
* Konfigurieren Sie die Proxyeinstellungen auf dem Gateway. Weitere Informationen finden Sie unter [Proxyserver-Aspekte](#proxy-server-considerations) .
* Aktivieren Sie die ausgehenden Ports 5671 und 9350-9354 sowohl auf dem Gatewaycomputer als auch auf der Unternehmensfirewall auf der Windows-Firewall. Weitere Informationen finden Sie unter [Ports und Firewall](#ports-and-firewall) . Dieser Schritt ist optional, wird jedoch für die Leistungsoptimierung empfohlen.

### <a name="3-problem"></a>3. Problem
Sie erhalten den folgenden Fehler: 

`Error: Cloud service cannot connect to gateway through service bus.`

### <a name="cause"></a>Ursache
Ein vorübergehender Fehler bei der Netzwerkkonnektivität.

### <a name="resolution"></a>Lösung
Um das Gateway wieder online zu schalten, gehen Sie wie folgt vor:

* Warten Sie einige Minuten. Die Verbindung wird automatisch wiederhergestellt, wenn der Fehler nicht mehr vorhanden ist.
* Wenn der Fehler weiterhin auftritt, starten Sie den Gatewaydienst neu.

## <a name="failed-to-author-linked-service"></a>Fehler beim Erstellen des verknüpften Diensts
### <a name="problem"></a>Problem
Dieser Fehler wird möglicherweise angezeigt, wenn Sie versuchen, mit der Anmeldeinformationsverwaltung im Portal Anmeldeinformationen für einen neuen verknüpften Dienst einzugeben oder die Anmeldeinformationen für einen vorhandenen verknüpften Dienst zu aktualisieren.

`Error: The data store '<Server>/<Database>' cannot be reached. Check connection settings for the data source.`

Wenn dieser Fehler angezeigt wird, kann die Seite „Einstellungen“ z.B. wie folgt aussehen:

![Die Datenbank wurde nicht gefunden](media/data-factory-troubleshoot-gateway-issues/database-cannot-be-reached.png)

### <a name="cause"></a>Ursache
Möglicherweise ist das SSL-Zertifikat auf dem Gatewaycomputer nicht mehr vorhanden. Das derzeit für die SSL-Verschlüsselung verwendete Zertifikat kann vom Gateway nicht geladen werden. Außerdem wird möglicherweise im Ereignisprotokoll die folgende oder eine ähnliche Fehlermeldung angezeigt: „Die Gatewayeinstellungen können nicht vom Clouddienst abgerufen werden. Überprüfen Sie den Gatewayschlüssel und die Netzwerkverbindung. (Zertifikat mit Fingerabdruck kann nicht geladen werden.)“

### <a name="resolution"></a>Lösung
Führen Sie die folgenden Schritte aus, um das Problem zu beheben:

1. Starten Sie den **Datenverwaltungsgateway-Konfigurations-Manager**.
2. Wechseln Sie zur Registerkarte **Einstellungen** .  
3. Klicken Sie auf die Schaltfläche **Ändern**, um das SSL-Zertifikat zu ändern.
   
   ![Schaltfläche „Zertifikat ändern“](media/data-factory-troubleshoot-gateway-issues/change-button-ssl-certificate.png) 
4. Wählen Sie ein neues Zertifikat als SSL-Zertifikat aus. (Sie können ein beliebiges SSL-Zertifikat verwenden, das von Ihnen oder einer Organisation generiert wurde).
   
   ![Zertifikat angeben](media/data-factory-troubleshoot-gateway-issues/specify-http-end-point.png)

## <a name="copy-activity-fails"></a>Fehler bei Kopieraktivität
### <a name="1-problem"></a>1. Problem
Möglicherweise bemerken Sie den folgenden UserErrorFailedToConnectToSqlserver-Fehler nach dem Einrichten einer Pipeline im Azure-Portal. 

`Error: Copy activity encountered a user error: ErrorCode=UserErrorFailedToConnectToSqlServer,'Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,Message=Cannot connect to SQL Server`

### <a name="cause"></a>Ursache
Dies kann aus verschiedenen Gründen geschehen, und die Fehlerbehebung variiert entsprechend. 

### <a name="resolution"></a>Lösung
Erlauben Sie ausgehende TCP-Verbindungen über Port TCP/1433 auf der Clientseite des Datenverwaltungsgateways, bevor Sie eine Verbindung mit einer SQL-Datenbank herstellen. 

Wenn die Zieldatenbank eine Azure SQL-Datenbank ist, überprüfen Sie auch die Firewalleinstellungen für Azure SQL Server.

Im folgenden Abschnitt finden Sie die Schritte zum Testen der Verbindung mit dem lokalen Datenspeicher. 

## <a name="data-store-connection-or-driver-related-errors"></a>Fehler bei der Datenspeicherverbindung oder den Datenspeichertreibern
Wenn Sie Fehler bei der Datenspeicherverbindung oder den Datenspeichertreibern bemerken, führen Sie folgende Schritte aus: 

1. Starten Sie den **Datenverwaltungsgateway-Konfigurations-Manager** auf dem Gatewaycomputer.
2. Wechseln Sie zur Registerkarte **Diagnose** .
3. Wählen Sie in den Feldern der Gruppe **Testen Sie die Verbindung mit einer lokalen Datenquelle mithilfe dieses Gateways** die geeigneten Werte aus, oder geben Sie die Werte ein.
4. Klicken Sie auf **Verbindung testen** , um festzustellen, ob Sie vom Gatewaycomputer aus mithilfe der Verbindungs- und Anmeldeinformationen eine Verbindung mit der lokalen Datenquelle herstellen können. Wenn das Testen der Verbindung weiter misslingt, nachdem Sie einen Treiber installiert haben, starten Sie das Gateway neu, damit es die letzte Änderung übernimmt.

![„Verbindung testen“ auf der Registerkarte „Diagnose“](media/data-factory-troubleshoot-gateway-issues/test-connection-in-diagnostics-tab.png)

## <a name="gateway-logs"></a>Gatewayprotokolle
### <a name="send-gateway-logs-to-microsoft"></a>Senden der Gatewayprotokolle an Microsoft
Wenn Sie sich im Zusammenhang mit Gatewayproblemen an den Microsoft Support wenden, werden Sie möglicherweise aufgefordert, Ihre Gatewayprotokolle freizugeben. In dieser Version des Gateways können Sie die erforderlichen Gatewayprotokolle komfortabel mit nur zwei Mausklicks im Gateway-Konfigurations-Manager freigeben.    

1. Wechseln Sie zur Registerkarte **Diagnose** im Gateway-Konfigurations-Manager.
   
    ![Datenverwaltungsgateway – Registerkarte „Diagnose“](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-diagnostics-tab.png)
2. Klicken Sie auf den Link **Protokolle senden** , um das folgende Dialogfeld anzuzeigen: 
   
    ![Datenverwaltungsgateway – Senden von Protokollen](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-dialog.png)
3. (optional) Klicken Sie auf **Protokolle anzeigen** , um Protokolle in der Ereignisanzeige durchzusehen.
4. (optional) Klicken Sie auf **Datenschutz** , um die Datenschutzbestimmungen der Onlinedienste von Microsoft anzuzeigen. 
5. Wenn Sie zum Hochladen bereit sind, klicken Sie auf **Protokolle senden** , um Protokolle der letzten sieben Tage zur Problembehandlung an Microsoft zu senden. Daraufhin sollte der Status des Vorgangs „Protokolle senden“ wie in der folgenden Abbildung angezeigt werden:
   
    ![Datenverwaltungsgateway – Senden des Status von Protokollen](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-status.png)
6. Nach Abschluss des Vorgangs wird ein Dialogfeld wie in der folgenden Abbildung angezeigt:
   
    ![Datenverwaltungsgateway – Senden des Status von Protokollen](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-result.png)
7. Notieren Sie sich die **Berichts-ID** , und teilen Sie sie dem Microsoft Support mit. Die Berichts-ID wird verwendet, um Ihre Gatewayprotokolle zu suchen, die Sie zur Problembehandlung hochgeladen haben.  Die Berichts-ID wird auch in der Ereignisanzeige gespeichert, damit Sie darauf zugreifen können.  Sie finden Sie anhand der Ereignis-ID „25“ sowie nach Datum und Uhrzeit.
   
    ![Datenverwaltungsgateway – Senden der Berichts-ID von Protokollen](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-report-id.png)    

### <a name="archive-gateway-logs-on-gateway-host-machine"></a>Archivieren von Gatewayprotokollen auf dem Gatewayhostcomputer
Es gibt einige Szenarien, in denen Gatewayprobleme auftreten, und Sie Gatewayprotokolle nicht direkt freigeben können: 

* Sie installieren und registrieren das Gateway manuell.
* Sie versuchen, das Gateway mit einem neu generierten Schlüssel im Konfigurations-Manager zu registrieren. 
* Sie versuchen, Protokolle zu senden, und es kann keine Verbindung mit dem Gatewayhostdienst hergestellt werden.

In solchen Fällen können Sie Gatewayprotokolle als ZIP-Datei speichern und später beim Kontakt mit dem Microsoft Support freigeben. Beispiel: Sie erhalten beim Registrieren des Gateways eine Fehlermeldung wie in der folgenden Abbildung dargestellt:   

![Datenverwaltungsgateway – Fehler bei der Registrierung](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-registration-error.png)

Klicken Sie auf den Link **Gatewayprotokolle archivieren** , um Protokolle zu archivieren und zu speichern und dann die ZIP-Datei für den Microsoft Support freizugeben. 

![Datenverwaltungsgateway – Archivieren von Protokollen](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-archive-logs.png)

### <a name="locate-gateway-logs"></a>Suchen von Gatewayprotokollen
Ausführliche Informationen hierzu finden Sie in den Gatewayprotokollen und Windows-Ereignisprotokollen. Diese finden Sie in der **Ereignisanzeige** von Windows unter **Anwendungs- und Dienstprotokolle** > **Datenverwaltungsgateway**. Wenn Sie Probleme mit dem Gateway beheben müssen, suchen Sie in der Ereignisanzeige nach Ereignissen vom Typ „Fehler“.

![Datenverwaltungsgateway – Protokolle in der Ereignisanzeige](media/data-factory-troubleshoot-gateway-issues/gateway-logs-event-viewer.png)




<!--HONumber=Nov16_HO3-->


