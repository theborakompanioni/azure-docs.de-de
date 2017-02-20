---
title: Installieren des Azure AD Connect Health-Agents | Microsoft Docs
description: "Auf dieser Seite zu Azure AD Connect Health wird die Installation des Agents für AD FS und für die Synchronisierung beschrieben."
services: active-directory
documentationcenter: 
author: karavar
manager: samueld
editor: curtand
ms.assetid: 1cc8ae90-607d-4925-9c30-6770a4bd1b4e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/12/2017
ms.author: vakarand
translationtype: Human Translation
ms.sourcegitcommit: 7c320a043322fefea1f58301492d4c5a0567320c
ms.openlocfilehash: 9569a850c6fadd86c408f9e9d4ec6d7d519744e8


---
# <a name="azure-ad-connect-health-agent-installation"></a>Installieren des Azure AD Connect Health-Agents
In diesem Dokument werden Sie durch die Installation und Konfiguration der Azure AD Connect Health-Agents geleitet. Sie können die Agents [hier](active-directory-aadconnect-health.md#download-and-install-azure-ad-connect-health-agent)herunterladen.

## <a name="requirements"></a>Anforderungen
Die folgende Tabelle enthält eine Liste mit Anforderungen für die Verwendung von Azure AD Connect Health.

| Voraussetzung | Beschreibung |
| --- | --- |
| Azure AD Premium |Azure AD Connect Health ist ein Azure AD Premium-Feature und erfordert Azure AD Premium. </br></br>Weitere Informationen finden Sie unter [Erste Schritte mit Azure AD Premium](../active-directory-get-started-premium.md). </br>Informationen zum Starten einer kostenlosen Testversion für 30 Tage finden Sie auf der Seite zum [Aktivieren einer Testversion](https://azure.microsoft.com/trial/get-started-active-directory/). |
| Sie müssen ein globaler Administrator Ihrer Azure AD-Instanz sein, um die ersten Schritte für Azure AD Connect Health durchzuführen |Standardmäßig können nur globale Administratoren die Health-Agents installieren und konfigurieren, um die ersten Schritte auszuführen, auf das Portal zuzugreifen und Vorgänge in Azure AD Connect Health durchzuführen. Weitere Informationen finden Sie unter [Verwalten Ihres Azure AD-Verzeichnisses](../active-directory-administer.md). <br><br> Mit der rollenbasierten Zugriffssteuerung können Sie anderen Benutzern in Ihrer Organisation den Zugriff auf Azure AD Connect Health gewähren. Weitere Informationen finden Sie unter [Rollenbasierte Zugriffssteuerung für Azure AD Connect Health](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control). </br></br>**Wichtig:** Das bei der Installation der Agents verwendete Konto muss ein Geschäfts-, Schul- oder Unikonto sein. Ein Microsoft-Konto kann nicht verwendet werden. Weitere Informationen finden Sie unter [Als Unternehmen für Azure registrieren](../sign-up-organization.md). |
| Der Azure AD Connect Health-Agent ist auf jedem Zielserver installiert. | Damit Azure AD Connect Health Daten empfangen und Überwachungs- und Analysefunktionen bereitstellen kann, müssen die Integritäts-Agents auf Zielservern installiert und konfiguriert sein. </br></br>Um beispielsweise Daten aus Ihrer AD FS-Infrastruktur abzurufen, muss der Agent auf den AD FS- und Webanwendungsproxy-Servern installiert sein. Ebenso muss der Agent auf den Domänencontrollern installiert sein, um Daten zu Ihrer lokalen AD DS-Infrastruktur abzurufen. </br></br> |
| Ausgehende Verbindungen zu den Azure-Dienstendpunkten | Während der Installation und der Laufzeit erfordert der Agent Verbindungen mit den Endpunkten des Azure AD Connect Health-Diensts. Falls ausgehende Verbindungen mithilfe von Firewalls blockiert werden, müssen der Zulassungsliste folgende Endpunkte hinzugefügt werden: </br></br><li>&#42;.blob.core.windows.net </li><li>&#42;.servicebus.windows.net – Port: 5671 </li><li>&#42;.adhybridhealth.azure.com/</li><li>https://management.azure.com </li><li>https://policykeyservice.dc.ad.msft.net/</li><li>https://login.windows.net</li><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li> |
|Ausgehende Verbindungen auf IP-Adressbasis | Informationen zur IP-adressbasierten Firewallfilterung finden Sie in den [IP-Bereichen für Azure](https://www.microsoft.com/en-us/download/details.aspx?id=41653).|
| Die SSL-Überprüfung für ausgehenden Datenverkehr ist gefiltert oder deaktiviert. | Beim Agent-Registrierungsschritt oder bei Datenuploads tritt unter Umständen ein Fehler auf, wenn auf der Netzwerkebene eine SSL-Überprüfung oder Beendigung für ausgehenden Datenverkehr erfolgt. |
| Firewall-Ports auf dem Server, auf dem der Agent ausgeführt wird. |Für den Agent müssen die folgenden Firewallports geöffnet sein, damit er mit den Azure AD Health-Dienstendpunkten kommunizieren kann:</br></br><li>TCP/UDP-Port 443</li><li>TCP/UDP-Port 5671</li> |
| Zulassen der folgenden Websites, wenn die verstärkte IE-Sicherheit aktiviert ist |Wenn die verstärkte IE-Sicherheit aktiviert ist, müssen die folgenden Websites auf dem Server zugelassen werden, auf dem der Agent installiert werden soll:</br></br><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li><li>https://login.windows.net</li><li>Der Verbundserver für Ihre Organisation, dem Azure Active Directory vertraut. Beispiel: https://sts.contoso.com</li> |

## <a name="installing-the-azure-ad-connect-health-agent-for-ad-fs"></a>Installieren des Azure AD Connect Health-Agents für AD FS
Um die Installation des Agents zu starten, doppelklicken Sie auf die EXE-Datei, die Sie heruntergeladen haben. Klicken Sie auf dem ersten angezeigten Bildschirm auf "Installieren".

![Überprüfen von Azure AD Connect Health](./media/active-directory-aadconnect-health-requirements/install1.png)

Klicken Sie nach Abschluss der Installation auf "Jetzt konfigurieren".

![Überprüfen von Azure AD Connect Health](./media/active-directory-aadconnect-health-requirements/install2.png)

Dadurch wird ein PowerShell-Fenster zur Initiierung des Agent-Registrierungsprozesses gestartet. Melden Sie sich nach einer entsprechenden Aufforderung mit einem zugriffsberechtigten Azure AD-Konto an, um die Agent-Registrierung durchzuführen. Das globale Administratorkonto hat standardmäßig Zugriff.

![Überprüfen von Azure AD Connect Health](./media/active-directory-aadconnect-health-requirements/install3.png)

Nach der Anmeldung werden weitere PowerShell-Befehle ausgeführt. Nach deren Ausführung können Sie PowerShell schließen, und die Konfiguration ist abgeschlossen.

An diesem Punkt sollten die Agent-Dienste automatisch gestartet werden, sodass der Agent die erforderlichen Daten sicher in den Clouddienst hochladen kann.

Wenn Sie nicht alle Voraussetzungen erfüllt haben, die in den obigen Schritten beschrieben wurden, werden im PowerShell-Fenster Warnungen angezeigt. Stellen Sie sicher, dass die [Voraussetzungen](active-directory-aadconnect-health-agent-install.md#requirements) erfüllt sind, bevor Sie den Agent installieren. Der folgende Screenshot zeigt ein Beispiel für diese Fehler:

![Überprüfen von Azure AD Connect Health](./media/active-directory-aadconnect-health-requirements/install4.png)

Suchen Sie auf dem Server nach den unten angegebenen Diensten, um sicherzustellen, dass der Agent installiert wurde. Wenn Sie die Konfiguration durchgeführt haben, sollten sie bereits ausgeführt werden. Andernfalls werden die Dienste beendet, bis die Konfiguration abgeschlossen wurde.

* Azure AD Connect Health AD FS Diagnostics Service
* Azure AD Connect Health AD FS Insights Service
* Azure AD Connect Health AD FS Monitoring Service

![Überprüfen von Azure AD Connect Health](./media/active-directory-aadconnect-health-requirements/install5.png)

### <a name="agent-installation-on-windows-server-2008-r2-servers"></a>Installation des Agents auf Windows Server 2008 R2-Servern
Schritte für Windows Server 2008 R2-Server:

1. Stellen Sie sicher, dass auf dem Server Service Pack 1 oder höher ausgeführt wird.
2. Deaktivieren Sie für die Installation des Agents die verstärkte Sicherheit für Internet Explorer:
3. Installieren Sie auf jedem Server Windows PowerShell 4.0, bevor Sie den AD Health-Agent installieren. So installieren Sie Windows PowerShell 4.0
   * Installieren Sie [Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=40779) , indem Sie den folgenden Link zum Herunterladen des Offlineinstallationsprogramms nutzen.
   * Installieren Sie PowerShell ISE (über "Windows-Features").
   * Installieren Sie [Windows Management Framework 4.0](https://www.microsoft.com/download/details.aspx?id=40855)
   * Installieren Sie Internet Explorer 10 oder höher auf dem Server. (Dies ist erforderlich, damit der Health-Dienst mit den Azure-Administratoranmeldeinformationen die Authentifizierung durchführen kann.)
4. Weitere Informationen zum Installieren von Windows PowerShell 4.0 unter Windows Server 2008 R2 finden Sie in [diesem Wiki-Artikel](http://social.technet.microsoft.com/wiki/contents/articles/20623.step-by-step-upgrading-the-powershell-version-4-on-2008-r2.aspx).

### <a name="enable-auditing-for-ad-fs"></a>Aktivieren der Überwachung für AD FS
> [!NOTE]
> Dieser Abschnitt betrifft nur AD FS-Server. Auf den Webanwendungsproxy-Servern müssen die Schritte nicht ausgeführt werden.
>

Um mithilfe der Nutzungsanalyse Daten zu sammeln und zu analysieren, benötigt der Azure AD Connect Health-Agent die in den AD FS-Überwachungsprotokollen enthaltenen Informationen. Diese Protokolle sind nicht standardmäßig aktiviert. Führen Sie die nachstehenden Anweisungen aus, um die AD FS-Überwachung zu aktivieren und die AD FS-Überwachungsprotokolle auf Ihren AD FS-Servern zu ermitteln.

#### <a name="to-enable-auditing-for-ad-fs-on-windows-server-2008-r2"></a>So aktivieren Sie die Überwachung für AD FS unter Windows Server 2008 R2
1. Klicken Sie auf **Start**, zeigen Sie auf **Programme** und dann auf **Verwaltung**, und klicken Sie anschließend auf **Lokale Sicherheitsrichtlinie**.
2. Navigieren Sie zum Ordner **Sicherheitseinstellungen\Lokale Richtlinien\User Rights Management**, und doppelklicken Sie dann auf „Generieren von Sicherheitsüberwachungen“.
3. Stellen Sie auf der Registerkarte **Lokale Sicherheitseinstellung** sicher, dass das AD FS 2.0-Dienstkonto aufgeführt wird. Wenn es nicht vorhanden ist, klicken Sie auf **Benutzer oder Gruppe hinzufügen** und fügen es der Liste hinzu. Klicken Sie dann auf **OK**.
4. Öffnen Sie zum Aktivieren der Überwachung ein Eingabeaufforderungsfenster mit erhöhten Rechten, und führen Sie den folgenden Befehl aus: <code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable</code>
5. Schließen Sie "Lokale Sicherheitsrichtlinie", und öffnen Sie dann das Verwaltungs-Snap-In. Klicken Sie zum Öffnen des Verwaltungs-Snap-Ins auf **Start**, zeigen Sie auf **Programme** und dann auf **Verwaltung**, und klicken Sie anschließend auf „AD FS 2.0 Management“.
6. Klicken Sie im Bereich "Aktionen" auf "Verbunddiensteigenschaften bearbeiten".
7. Klicken Sie im Dialogfeld **Verbunddiensteigenschaften** auf die Registerkarte **Ereignisse**.
8. Aktivieren Sie die Kontrollkästchen **Erfolgreiche Überprüfungen** und **Fehlerüberprüfungen**.
9. Klicken Sie auf **OK**.

#### <a name="to-enable-auditing-for-ad-fs-on-windows-server-2012-r2"></a>So aktivieren Sie die Überwachung für AD FS unter Windows Server 2012 R2
1. Öffnen Sie **Lokale Sicherheitsrichtlinie**, indem Sie den **Server-Manager** auf dem Startbildschirm oder über die Taskleiste auf dem Desktop öffnen, und klicken Sie dann auf **Tools/Lokale Sicherheitsrichtlinie**.
2. Navigieren Sie zum Ordner **Sicherheitseinstellungen\Lokale Richtlinien\Zuweisen von Benutzerrechten**, und doppelklicken Sie dann auf **Generieren von Sicherheitsüberwachungen**.
3. Stellen Sie auf der Registerkarte **Lokale Sicherheitseinstellung** sicher, dass das AD FS-Dienstkonto aufgeführt wird. Wenn es nicht vorhanden ist, klicken Sie auf **Benutzer oder Gruppe hinzufügen** und fügen es der Liste hinzu. Klicken Sie dann auf **OK**.
4. Öffnen Sie zum Aktivieren der Überwachung ein Eingabeaufforderungsfenster mit erweiterten Berechtigungen, und führen Sie den folgenden Befehl aus: ```auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable```.
5. Schließen Sie **Lokale Sicherheitsrichtlinie**, und öffnen Sie dann das AD FS-Verwaltungs-Snap-In**** (klicken Sie hierzu im Server-Manager auf „Tools“, und wählen Sie dann „AD FS Management“ aus).
6. Klicken Sie im Bereich "Aktionen" auf **Verbunddiensteigenschaften bearbeiten**.
7. Klicken Sie im Dialogfeld "Verbunddiensteigenschaften" auf die Registerkarte **Ereignisse** .
8. Aktivieren Sie die Kontrollkästchen **Erfolgreiche Überprüfungen und Fehlerüberprüfungen**, und klicken Sie anschließend auf **OK**.

#### <a name="to-enable-auditing-for-ad-fs-on-windows-server-2016"></a>So aktivieren Sie die Überwachung für AD FS unter Windows Server 2016
1. Öffnen Sie **Lokale Sicherheitsrichtlinie**, indem Sie den **Server-Manager** auf dem Startbildschirm oder über die Taskleiste auf dem Desktop öffnen, und klicken Sie dann auf **Tools/Lokale Sicherheitsrichtlinie**.
2. Navigieren Sie zum Ordner **Sicherheitseinstellungen\Lokale Richtlinien\Zuweisen von Benutzerrechten**, und doppelklicken Sie dann auf **Generieren von Sicherheitsüberwachungen**.
3. Vergewissern Sie sich auf der Registerkarte **Lokale Sicherheitseinstellung**, dass das AD FS-Dienstkonto aufgeführt wird. Ist es nicht vorhanden, klicken Sie auf **Benutzer oder Gruppe hinzufügen**, fügen Sie das AD FS-Dienstkonto der Liste hinzu, und klicken Sie anschließend auf **OK**.
4. Öffnen Sie zum Aktivieren der Überwachung ein Eingabeaufforderungsfenster mit erhöhten Rechten, und führen Sie den folgenden Befehl aus: <code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable.</code>
5. Schließen Sie **Lokale Sicherheitsrichtlinie**, und öffnen Sie dann das AD FS-Verwaltungs-Snap-In**** (klicken Sie hierzu im Server-Manager auf „Tools“, und wählen Sie dann „AD FS Management“ aus).
6. Klicken Sie im Bereich "Aktionen" auf **Verbunddiensteigenschaften bearbeiten**.
7. Klicken Sie im Dialogfeld "Verbunddiensteigenschaften" auf die Registerkarte **Ereignisse** .
8. Aktivieren Sie die Kontrollkästchen **Erfolgreiche Überprüfungen und Fehlerüberprüfungen**, und klicken Sie anschließend auf **OK**. Diese sollten standardmäßig aktiviert sein.
9. Öffnen Sie ein PowerShell-Fenster, und führen Sie den folgenden Befehl aus: ```Set-AdfsProperties -AuditLevel Verbose```.

Standardmäßig ist die Überwachungsebene „basic“ aktiviert. Weitere Informationen zur AD FS-Überwachungserweiterung unter Windows Server 2016 finden Sie [hier](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/operations/auditing-enhancements-to-ad-fs-in-windows-server-2016).


#### <a name="to-locate-the-ad-fs-audit-logs"></a>So ermitteln Sie die AD FS-Überwachungsprotokolle
1. Öffnen Sie die Ereignisanzeige ****.
2. Wechseln Sie zu "Windows-Protokolle", und wählen Sie **Sicherheit**aus.
3. Klicken Sie auf der rechten Seite auf **Aktuelle Protokolle filtern**.
4. Wählen Sie unter "Ereignisquelle" die **AD FS-Überwachung**aus.

![AD FS-Überwachungsprotokolle](./media/active-directory-aadconnect-health-requirements/adfsaudit.png)

> [!WARNING]
> Die AD FS-Überwachung kann mithilfe einer Gruppenrichtlinie deaktiviert werden. Bei deaktivierter AD FS-Überwachung stehen keine Nutzungsanalysen für Anmeldeaktivitäten zur Verfügung. Stellen Sie sicher, dass die AD FS-Überwachung nicht durch eine Gruppenrichtlinie deaktiviert wird.
>


## <a name="installing-the-azure-ad-connect-health-agent-for-sync"></a>Installieren des Azure AD Connect Health-Agents für die Synchronisierung
Der Azure AD Connect Health-Agent für die Synchronisierung wird im neuesten Build von Azure AD Connect automatisch installiert. Damit Sie Azure AD Connect für die Synchronisierung verwenden können, müssen Sie die neueste Version von Azure AD Connect herunterladen und installieren. Sie können die neueste Version [hier](http://www.microsoft.com/download/details.aspx?id=47594)herunterladen.

Suchen Sie auf dem Server nach den unten angegebenen Diensten, um sicherzustellen, dass der Agent installiert wurde. Wenn Sie die Konfiguration durchgeführt haben, sollten sie bereits ausgeführt werden. Andernfalls werden die Dienste beendet, bis die Konfiguration abgeschlossen wurde.

* Azure AD Connect Health Sync Insights-Dienst
* Azure AD Connect Health Sync-Überwachungsdienst

![Überprüfen von Azure AD Connect Health für die Synchronisierung](./media/active-directory-aadconnect-health-sync/services.png)

> [!NOTE]
> Denken Sie daran, dass Sie Azure AD Connect Health nur mit Azure AD Premium nutzen können. Nur wenn Sie über Azure AD Premium verfügen, können Sie die Konfiguration im Azure-Portal abschließen. Weitere Informationen finden Sie auf der [Seite mit den Anforderungen](active-directory-aadconnect-health-agent-install.md#requirements).
>
>

## <a name="manual-azure-ad-connect-health-for-sync-registration"></a>Manuelle Registrierung für Azure AD Connect Health für die Synchronisierung
Wenn die Agent-Registrierung von Azure AD Connect Health für die Synchronisierung fehlschlägt, nachdem Sie Azure AD Connect erfolgreich installiert haben, können Sie den Agent mit dem folgenden PowerShell-Befehl manuell registrieren.

> [!IMPORTANT]
> Die Verwendung dieses PowerShell-Befehls ist nur erforderlich, wenn die Agent-Registrierung nach der Installation von Azure AD Connect fehlschlägt.
>
>

Der folgende PowerShell-Befehl ist NUR erforderlich, wenn die Health-Agent-Registrierung auch nach der erfolgreichen Installation und Konfiguration von Azure AD Connect fehlschlägt. Die Azure AD Connect Health-Dienste werden gestartet, nachdem der Agent erfolgreich registriert wurde.

Sie können den Azure AD Connect Health-Agent für die Synchronisierung mit dem folgenden PowerShell-Befehl manuell registrieren:

`Register-AzureADConnectHealthSyncAgent -AttributeFiltering $false -StagingMode $false`

Der Befehl hat folgende Parameter:

* AttributeFiltering: $true (default) – wenn Azure AD Connect den Standardattributsatz nicht synchronisiert und für die Verwendung eines gefilterten Attributsatzes angepasst wurde. Andernfalls $false.
* StagingMode: $false (Standard) – wenn der Azure AD Connect-Server sich NICHT im Stagingmodus befindet; $true, wenn der Server für den Stagingmodus konfiguriert ist.

Wenn Sie zur Authentifizierung aufgefordert werden, geben Sie das globale Administratorkonto ein (z.B. admin@domain.onmicrosoft.com)), das zum Konfigurieren von Azure AD Connect verwendet wurde.

## <a name="installing-the-azure-ad-connect-health-agent-for-ad-ds"></a>Installieren des Azure AD Connect Health-Agents für AD DS
Um die Installation des Agents zu starten, doppelklicken Sie auf die EXE-Datei, die Sie heruntergeladen haben. Klicken Sie auf dem ersten angezeigten Bildschirm auf "Installieren".

![Überprüfen von Azure AD Connect Health](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install1.png)

Klicken Sie nach Abschluss der Installation auf "Jetzt konfigurieren".

![Überprüfen von Azure AD Connect Health](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install2.png)

Es wird eine Eingabeaufforderung mit PowerShell-Befehlen zum Ausführen von „Register-AzureADConnectHealthADDSAgent“ geöffnet. Führen Sie die Anmeldung durch, wenn Sie von Azure dazu aufgefordert werden.

![Überprüfen von Azure AD Connect Health](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install3.png)

Nach der Anmeldung werden weitere PowerShell-Befehle ausgeführt. Nach deren Ausführung können Sie PowerShell schließen, und die Konfiguration ist abgeschlossen.

An diesem Punkt sollten die Dienste automatisch gestartet werden, sodass der Agent die Daten überwachen und erfassen kann. Wenn Sie nicht alle Voraussetzungen erfüllt haben, die in den obigen Schritten beschrieben wurden, werden im PowerShell-Fenster Warnungen angezeigt. Stellen Sie sicher, dass die [Voraussetzungen](active-directory-aadconnect-health-agent-install.md#requirements) erfüllt sind, bevor Sie den Agent installieren. Der folgende Screenshot zeigt ein Beispiel für diese Fehler:

![Überprüfen von Azure AD Connect Health für AD DS](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install4.png)

Suchen Sie auf dem Domänencontroller nach den unten angegebenen Diensten, um sicherzustellen, dass der Agent installiert wurde.

* Azure AD Connect Health AD DS Insights Service
* Azure AD Connect Health AD DS Monitoring Service

Wenn Sie die Konfiguration durchgeführt haben, sollten diese Dienste ausgeführt werden. Andernfalls werden die Dienste beendet, bis die Konfiguration abgeschlossen wurde.

![Überprüfen von Azure AD Connect Health](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install5.png)


### <a name="agent-registration-using-powershell"></a>Agent-Registrierung mithilfe von PowerShell
Nach der Installation der geeigneten Agent-Setupdatei können Sie abhängig von der Rolle mithilfe der folgenden PowerShell-Befehle den Agent-Registrierungsschritt ausführen. Öffnen Sie ein PowerShell-Fenster, und führen Sie den entsprechenden Befehl aus:

```
    Register-AzureADConnectHealthADFSAgent
    Register-AzureADConnectHealthADDSAgent
    Register-AzureADConnectHealthSyncAgent

```

Diese Befehle akzeptieren „Credential“ als Parameter, um die Registrierung auf nicht interaktive Weise oder auf einem Server Core-Computer durchzuführen.
* Die Anmeldeinformationen können in einer PowerShell-Variablen erfasst werden, die als Parameter übergeben wird.
* Sie können eine beliebige Azure AD-Identität angeben, die über Zugriff für die Agent-Registrierung verfügt und für die MFA NICHT aktiviert ist.
* Globale Administratoren verfügen standardmäßig über Zugriff für die Agent-Registrierung. Die Ausführung dieses Schritts kann auch Identitäten mit weniger Berechtigungen ermöglicht werden. Weitere Informationen finden Sie unter [Verwalten des Zugriffs mit rollenbasierter Zugriffssteuerung](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control).

```
    $cred = Get-Credential
    Register-AzureADConnectHealthADFSAgent -Credential $cred

```

## <a name="configure-azure-ad-connect-health-agents-to-use-http-proxy"></a>Konfigurieren des Azure AD Connect Health-Agents zur Verwendung eines HTTP-Proxys
Sie können den Azure AD Connect Health-Agent für die Arbeit mit einem HTTP-Proxy konfigurieren.

> [!NOTE]
> * Die Verwendung von „Netsh WinHttp set ProxyServerAddress“ wird nicht unterstützt, da der Agent „System.Net“ anstelle der Microsoft Windows-HTTP-Dienste verwendet, um Webanforderungen zu senden.
> * Die konfigurierte HTTP-Proxyadresse wird für das Durchlaufen verschlüsselter HTTPS-Nachrichten verwendet.
> * Authentifizierte Proxys (mit HTTPBasic) werden nicht unterstützt.
>
>

### <a name="change-health-agent-proxy-configuration"></a>Ändern der Health Agent-Proxykonfiguration
Sie haben die folgenden Möglichkeiten zum Konfigurieren des Azure AD Connect Health-Agents für die Verwendung eines HTTP-Proxys.

> [!NOTE]
> Alle Azure AD Connect Health-Agent-Dienste müssen neu gestartet werden, damit die Proxyeinstellungen aktualisiert werden. Führen Sie den folgenden Befehl aus:<br>
> Restart-Service AdHealth*
>
>

#### <a name="import-existing-proxy-settings"></a>Importieren von vorhandenen Proxyeinstellungen
##### <a name="import-from-internet-explorer"></a>Importieren von Internet Explorer
Internet Explorer-HTTP-Proxyeinstellungen können importiert werden, damit sie von den Azure AD Connect Health-Agents verwendet werden können. Führen Sie auf allen Servern, auf denen der Health-Agent ausgeführt wird, den folgenden PowerShell-Befehl aus:

    Set-AzureAdConnectHealthProxySettings -ImportFromInternetSettings

##### <a name="import-from-winhttp"></a>Importieren von WinHTTP
WinHTTP-Proxyeinstellungen können importiert werden, damit sie von den Azure AD Connect Health-Agents verwendet werden können. Führen Sie auf allen Servern, auf denen der Health-Agent ausgeführt wird, den folgenden PowerShell-Befehl aus:

    Set-AzureAdConnectHealthProxySettings -ImportFromWinHttp

#### <a name="specify-proxy-addresses-manually"></a>Manuelles Angeben von Proxyadressen
Sie können einen Proxyserver manuell für jeden Server angeben, auf dem der Health-Agent ausgeführt wird, indem Sie den folgenden PowerShell-Befehl ausführen:

    Set-AzureAdConnectHealthProxySettings -HttpsProxyAddress address:port

Beispiel: *Set-AzureAdConnectHealthProxySettings -HttpsProxyAddress myproxyserver: 443*

* "address" kann ein über DNS auflösbarer Servername oder eine IPv4-Adresse sein.
* "port" kann ausgelassen werden. In diesem Fall wird der Standardport 443 ausgewählt.

#### <a name="clear-existing-proxy-configuration"></a>Löschen der vorhandenen Proxykonfiguration
Sie können die vorhandene Proxykonfiguration löschen, indem Sie den folgenden Befehl ausführen:

    Set-AzureAdConnectHealthProxySettings -NoProxy


### <a name="read-current-proxy-settings"></a>Lesen der aktuellen Proxyeinstellungen
Sie können die aktuell konfigurierten Proxyeinstellungen lesen, indem Sie den folgenden Befehl ausführen:

    Get-AzureAdConnectHealthProxySettings


## <a name="test-connectivity-to-azure-ad-connect-health-service"></a>Testen der Verbindung mit dem Azure AD Connect Health-Dienst
Es können Probleme auftreten, die dazu führen, dass die Verbindung zwischen dem Azure AD Connect Health-Agent und dem Azure AD Connect Health-Dienst unterbrochen wird. Dazu gehören Netzwerkprobleme, Berechtigungsprobleme oder verschiedene andere Ursachen.

Wenn der Agent für mehr als zwei Stunden keine Daten an den Azure AD Connect Health-Dienst senden kann, wird dies im Portal mit der folgenden Warnung angezeigt: „Die Daten des Integritätsdiensts sind nicht aktuell.“ Sie können überprüfen, ob der betroffene Azure AD Connect Health-Agent Daten für den Azure AD Connect Health-Dienst hochladen kann, indem Sie den folgenden PowerShell-Befehl ausführen:

    Test-AzureADConnectHealthConnectivity -Role ADFS

Der Rollenparameter akzeptiert derzeit die folgenden Werte:

* ADFS
* Synchronisierung
* ADDS

Sie können das Flag -ShowResults im Befehl verwenden, um detaillierte Protokolle anzuzeigen. Nehmen Sie das folgende Beispiel:

    Test-AzureADConnectHealthConnectivity -Role Sync -ShowResult

> [!NOTE]
> Um das Konnektivitätstool zu verwenden, müssen Sie zunächst die Agent-Registrierung abschließen. Wenn Sie die Registrierung des Agents nicht abschließen können, sollten Sie sicherstellen, dass alle [Voraussetzungen](active-directory-aadconnect-health-agent-install.md#requirements) für Azure AD Connect Health erfüllt sind. Dieser Verbindungstest wird standardmäßig bei der Agent-Registrierung durchgeführt.
>
>

## <a name="related-links"></a>Verwandte Links
* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Azure AD Connect Health-Vorgänge](active-directory-aadconnect-health-operations.md)
* [Verwenden von Azure AD Connect Health mit AD FS](active-directory-aadconnect-health-adfs.md)
* [Verwenden von Azure AD Connect Health für die Synchronisierung](active-directory-aadconnect-health-sync.md)
* [Verwenden von Azure AD Connect Health mit AD DS](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect Health – FAQ](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect Health: Versionsverlauf](active-directory-aadconnect-health-version-history.md)


<!--HONumber=Feb17_HO2-->


