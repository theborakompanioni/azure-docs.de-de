---
title: Verwenden von vorhandenen lokalen Proxyservern und Azure AD | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie vorhandene lokale Proxyserver verwenden.
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/12/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 2fab64ef8bf07532c4fd2fff4f4af68dc0b44b45
ms.openlocfilehash: 659edc19c9f043d6a388f1bb293c72e379fa6e4a


---

# <a name="work-with-existing-on-premises-proxy-servers"></a>Verwenden von vorhandenen lokalen Proxyservern

In diesem Artikel wird beschrieben, wie Sie den Anwendungsproxyconnector für Proxyserver für ausgehenden Datenverkehr konfigurieren. Er richtet sich an Kunden mit Netzwerkumgebungen, die über vorhandene Proxys verfügen.

Zuerst sehen wir uns die folgenden wichtigsten Bereitstellungsszenarien an:
* Konfigurieren von Connectors zum Umgehen von lokalen Proxys für ausgehenden Datenverkehr
* Konfigurieren von Connectors zum Verwenden eines Proxys für ausgehenden Datenverkehr für den Zugriff auf den Azure AD-Anwendungsproxy

Weitere Informationen zur Funktionsweise von Connectors finden Sie unter [Bereitstellen von sicherem Remotezugriff auf lokale Anwendungen](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-application-proxy-get-started).

## <a name="configure-your-connectors"></a>Konfigurieren von Connectors

Für den Kernconnectordienst wird Azure Service Bus zum Verarbeiten der zugrunde liegenden Kommunikation mit dem Azure AD-Anwendungsproxydienst verwendet. Mit Service Bus sind zusätzliche Konfigurationsanforderungen verbunden.

Informationen zum Beheben von Azure AD-Konnektivitätsproblemen finden Sie unter [How to troubleshoot Azure AD Application Proxy connectivity problems](https://blogs.technet.microsoft.com/applicationproxyblog/2015/03/24/how-to-troubleshoot-azure-ad-application-proxy-connectivity-problems) (Behandeln von Problemen mit der Azure AD-Anwendungsproxykonnektivität). 

## <a name="configure-the-outbound-proxy"></a>Konfigurieren des Proxys für ausgehenden Datenverkehr

Wenn Sie in Ihrer Umgebung einen Proxy für ausgehenden Datenverkehr verwenden, sollten Sie sicherstellen, dass das für die Installation verwendete Konto für die Verwendung des Proxys für ausgehenden Datenverkehr richtig konfiguriert ist. Da das Installationsprogramm im Kontext des Benutzers ausgeführt wird, von dem die Installation durchgeführt wird, können Sie hierfür Microsoft Edge oder einen anderen Internetbrowser verwenden. 

Navigieren Sie zum Konfigurieren der Proxyeinstellungen mit Microsoft Edge zu „Einstellungen“ > „Erweiterte Einstellungen anzeigen“ > „Proxyeinstellungen öffnen“ > „Manuelle Proxyeinrichtung“. Legen Sie „Proxyserver verwenden“ auf „Ein“ fest, aktivieren Sie die Option „Für lokale Adressen (Intranet) keinen Proxyserver verwenden“, und ändern Sie dann die Adresse und den Port, um Ihren lokalen Proxyserver anzugeben.

Geben Sie die erforderlichen Proxyeinstellungen wie unten im Optionsfeld dargestellt ein.

 ![Azure AD – Umgehen von lokalen Adressen](./media/application-proxy-working-with-proxy-servers/proxy-bypass-local-addresses.png)
 
## <a name="bypass-outbound-proxies"></a>Umgehen von Proxys für ausgehenden Datenverkehr

Standardmäßig wird von den zugrunde liegenden Betriebssystemkomponenten, die vom Connector zum Senden von ausgehenden Anforderungen verwendet werden, automatisch versucht, einen Proxyserver im Netzwerk per WPAD (Web Proxy Auto-Discovery) zu ermitteln, sofern dieser in der Umgebung aktiviert ist.

Hierfür wird normalerweise eine DNS-Suche nach „wpad.domainsuffix“ durchgeführt. Bei der Auflösung im DNS wird dann eine HTTP-Anforderung an die IP-Adresse für „wpad.dat“ gesendet. Dies ist das Skript für die Proxykonfiguration in Ihrer Umgebung. Vom Connector wird dies dann verwendet, um einen Proxyserver für ausgehenden Datenverkehr auszuwählen. Es kann aber sein, dass der Datenverkehr des Connectors trotzdem nicht fließen kann, weil für den Proxy weitere Konfigurationseinstellungen erforderlich sind. 

Im nächsten Abschnitt geht es um die Konfigurationsschritte, die auf dem Proxy für ausgehenden Datenverkehr benötigt werden, damit der Datenverkehr darüber gesendet werden kann. Zuerst wird aber beschrieben, wie Sie den Connector so konfigurieren, dass Ihr lokaler Proxy umgangen wird, und sicherstellen, dass die direkte Verbindung mit den Azure-Diensten verwendet wird. Dies ist die empfohlene Vorgehensweise (sofern Ihre Netzwerkrichtlinie dies zulässt), da dies bedeutet, dass Sie eine Konfiguration weniger pflegen müssen.

Bearbeiten Sie zum Deaktivieren der Proxynutzung für ausgehenden Datenverkehr für den Connector die Datei „C:\Programme\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config“, und fügen Sie den Abschnitt [system.net] aus dem folgenden Codebeispiel hinzu:

```xml
 <?xml version="1.0" encoding="utf-8" ?>
<configuration>
<system.net>
<defaultProxy enabled="false"></defaultProxy>
</system.net>
 <runtime>
<gcServer enabled="true"/>
  </runtime>
  <appSettings>
<add key="TraceFilename" value="AadAppProxyConnector.log" />
  </appSettings>
</configuration>
```
Um zu erreichen, dass der Proxy auch vom Connectorupdatedienst umgangen wird, nehmen Sie eine ähnliche Änderung an der Datei „ApplicationProxyConnectorUpdaterService.exe.config“ unter „C:\Programme\Microsoft AAD App Proxy Connector Updater\ApplicationProxyConnectorUpdaterService.exe.config“ vor.

Achten Sie darauf, Kopien der Originaldateien zu erstellen, falls Sie die CONFIG-Standarddateien später noch benötigen. 

## <a name="use-the-outbound-proxy-server"></a>Verwenden des Proxyservers für ausgehenden Datenverkehr

Wie bereits erwähnt, gilt in einigen Kundenumgebungen die Anforderung, dass der gesamte ausgehende Datenverkehr ausnahmslos über einen Proxy für ausgehenden Datenverkehr gesendet wird. Das Umgehen des Proxys ist also keine Option.

Sie können den Connectordatenverkehr so konfigurieren, dass er wie unten gezeigt über den Proxy für ausgehenden Datenverkehr verläuft.

 ![Azure AD – Umgehen von lokalen Adressen](./media/application-proxy-working-with-proxy-servers/configure-proxy-settings.png)

Da es nur um ausgehenden Datenverkehr geht, muss kein Lastenausgleich zwischen den Connectors eingerichtet und auch kein Zugriff in eingehender Richtung über die Firewalls konfiguriert werden.

Die folgenden Schritte müssen auf jeden Fall ausgeführt werden:
1. Konfigurieren der Route über den Proxy für ausgehenden Datenverkehr für den Connector und den Updatedienst
2. Konfigurieren der Proxyeinstellungen zum Zulassen von Verbindungen mit dem Azure AD-Anwendungsproxydienst

### <a name="step-1-configure-the-connector-and-related-services-to-go-through-the-outbound-proxy"></a>Schritt 1: Konfigurieren der Route über den Proxy für ausgehenden Datenverkehr für den Connector und die dazugehörigen Dienste

Wie oben beschrieben, gilt Folgendes: Wenn WPAD in der Umgebung aktiviert und richtig konfiguriert ist, kann der Connector den Proxyserver für ausgehenden Datenverkehr automatisch ermitteln und versuchen, ihn zu verwenden. Sie können den Connector aber auch explizit so konfigurieren, dass ein Proxy für ausgehenden Datenverkehr verwendet wird. 

Bearbeiten Sie hierfür die Datei „C:\Programme\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config“, und fügen Sie den Abschnitt [system.net] aus dem folgenden Codebeispiel hinzu:

```xml
 <?xml version="1.0" encoding="utf-8" ?>
<configuration>
<system.net>  
    <defaultProxy>   
      <proxy proxyaddress="http://proxyserver:8080" bypassonlocal="True" usesystemdefault="True"/>   
    </defaultProxy>  
</system.net>
  <runtime>
     <gcServer enabled="true"/>
  </runtime>
  <appSettings>
    <add key="TraceFilename" value="AadAppProxyConnector.log" />
  </appSettings>
</configuration>
```

>[!NOTE]
>Ändern Sie _proxyserver:8080_ in den Namen Ihres lokalen Proxyservers bzw. die IP-Adresse und den Port, über den gelauscht wird.
>

Anschließend müssen Sie den Connectorupdatedienst für die Verwendung des Proxys konfigurieren, indem Sie eine ähnliche Änderung an der Datei unter „C:\Programme\Microsoft AAD App Proxy Connector Updater\ApplicationProxyConnectorUpdaterService.exe.config“ vornehmen.

###<a name="step-2-configure-the-proxy-to-allow-traffic-from-the-connector-and-related-services-to-flow-through"></a>Schritt 2: Konfigurieren des Proxys für das Akzeptieren von Datenverkehr vom Connector und den dazugehörigen Diensten

Auf dem Proxy für ausgehenden Datenverkehr sind vier Aspekte zu beachten:
* Proxyregeln für ausgehenden Datenverkehr
* Proxyauthentifizierung
* Proxyports
* SSL-Überprüfung

#### <a name="2a-proxy-outbound-rules"></a>2.A: Proxyregeln für ausgehenden Datenverkehr
Lassen Sie in Bezug auf den Connectordienstzugriff den Zugriff auf die folgenden Endpunkte zu:

* *.msappproxy.net 
* *.servicebus.microsoft.net 

Lassen Sie für die erste Registrierung den Zugriff auf die folgenden Endpunkte zu:

* login.windows.net 
* login.microsoftonline.com 

Für die zugrunde liegenden Service Bus-Steuerungskanäle, die vom Connectordienst verwendet werden, ist eine zusätzliche Verbindung mit bestimmten IP-Adressen erforderlich. Beachten Sie, dass wir mit dem Service Bus-Team zusammenarbeiten, um stattdessen die Umstellung auf einen FQDN durchzuführen. Vorerst haben Sie zwei Optionen: 
 
* Lassen Sie für den Connector den ausgehenden Zugriff für alle Ziele zu.
* Lassen Sie für den Connector den ausgehenden Zugriff auf die Azure Datacenter-IP-Bereiche zu, die unter „https://www.microsoft.com/en-gb/download/details.aspx?id=41653“ angegeben sind.

>[!NOTE]
>Die Schwierigkeit besteht bei Verwendung der Liste mit den Azure Datacenter-IP-Bereichen darin, dass sie wöchentlich aktiviert wird. Sie müssen also durch einen geeigneten Prozess sicherstellen, dass Ihre Zugriffsregeln entsprechend aktualisiert werden.
>

#### <a name="2b-proxy-authentication"></a>2.B: Proxyauthentifizierung

Die Proxyauthentifizierung wird derzeit nicht unterstützt. Unsere aktuelle Empfehlung ist, für den Connector den anonymen Zugriff auf die Internetziele zuzulassen.

#### <a name="2c-proxy-ports"></a>2.C: Proxyports

Der Connector stellt ausgehende SSL-basierte Verbindungen mit der CONNECT-Methode her. Hierbei wird praktisch ein Tunnel durch den Proxy für ausgehenden Datenverkehr eingerichtet. Einige Proxyserver lassen standardmäßig nur ausgehende Tunnel zu SSL-Standardports zu (z.B. 443). In diesem Fall muss der Proxyserver so konfiguriert werden, dass Tunnel zu zusätzlichen Ports zugelassen werden.

Konfigurieren Sie den Proxyserver für die Verwendung von Tunneln zu den nicht dem Standard entsprechenden SSL-Ports 8080, 9090, 9091 und 10100 bis 10120.

>[!NOTE]
>Wenn die Service Bus-Daten per HTTPS gesendet werden, wird Port 443 verwendet. Standardmäßig wird für Service Bus aber versucht, direkte TCP-Verbindungen herzustellen, und HTTPS wird nur verwendet, wenn für die direkte Verbindung ein Fehler auftritt. 
> 

Damit der Service Bus-Datenverkehr auch über den Proxyserver für ausgehenden Datenverkehr gesendet wird, müssen Sie sicherstellen, dass der Connector keine direkte Verbindung mit den Azure-Diensten für die Ports 9350, 9352 und 5671 herstellen kann.

#### <a name="2d-ssl-inspection"></a>2.D: SSL-Überprüfung
Verwenden Sie die SSL-Überprüfung nicht für den Connectordatenverkehr, da dies dafür zu Problemen führt. 

Fertig! Sie sollten jetzt verfolgen können, dass der gesamte Datenverkehr über den Proxy fließt. Bei Problemen helfen Ihnen die unten angegebenen Schritte zur Problembehandlung weiter.

### <a name="troubleshoot-connector-proxy-problems-and-service-connectivity-issues"></a>Problembehandlung für Proxyprobleme des Connectors und Verbindungsprobleme von Diensten

Die beste Möglichkeit zur Identifizierung und Behebung von Problemen mit der Connectorkonnektivität ist die Erstellung einer Netzwerkerfassung für den Connectordienst, während dieser gestartet wird. Da dies eine anspruchsvolle Aufgabe sein kann, erhalten Sie hier einige kurze Tipps zum Erfassen und Filtern von Netzwerkablaufverfolgungen.

Sie können ein Überwachungstool Ihrer Wahl verwenden. Im Rahmen dieses Artikels haben wir den Microsoft-Netzwerkmonitor 3.4 verwendet. Diese Anwendung können Sie unter „https://www.microsoft.com/en-us/download/details.aspx?id=4865“ herunterladen.

Die unten verwendeten Beispiele und Filter gelten für den Netzwerkmonitor, aber die Vorgehensweise kann auf alle Analysetools angewendet werden.

### <a name="take-a-capture-using-microsoft-network-monitor"></a>Durchführen einer Erfassung mit dem Microsoft-Netzwerkmonitor

Öffnen Sie zum Starten eines Erfassungsvorgangs den Netzwerkmonitor, und klicken Sie auf „New Capture“ (Neue Erfassung). Klicken Sie auf die Schaltfläche „Start“, um zu beginnen.

 ![Azure AD-Netzwerkmonitor](./media/application-proxy-working-with-proxy-servers/network-capture.png)

Klicken Sie nach Abschluss einer Erfassung auf die Schaltfläche „Stop“ (Beenden), um den Erfassungsvorgang zu beenden.

### <a name="take-a-capture-of-connector-traffic"></a>Durchführen einer Erfassung des Connectordatenverkehrs

Führen Sie die folgenden Schritte aus, um die erste Problembehandlung durchzuführen: 

1. Beenden Sie über „services.msc“ den Microsoft AAD-Anwendungsproxyconnector-Dienst (unten dargestellt).
2. Starten Sie die Netzwerkerfassung.
3. Starten Sie den Microsoft AAD-Anwendungsproxyconnector-Dienst.
4. Beenden Sie die Netzwerkerfassung.

 ![Azure AD-Netzwerkmonitor](./media/application-proxy-working-with-proxy-servers/services-local.png)

### <a name="look-at-the-connector-to-proxy-server-requests"></a>Anzeigen der Anforderungen vom Connector zum Proxyserver

Nachdem Sie nun eine Netzwerkerfassung erstellt haben, können Sie sie filtern. Der Schlüssel bei der Anzeige der Ablaufverfolgung ist das Verständnis, wie die Erfassung gefiltert werden muss.

Dies ist ein geeigneter Filter (wobei 8080 der Proxydienstport ist): 

(http.Request or http.Response) and tcp.port==8080

Wenn Sie diesen Filtertext im Fenster „Anzeigefilter“ eingeben und „Übernehmen“ wählen, wird der erfasste Datenverkehr basierend auf diesem Filter gefiltert.

Mit dem obigen Filter werden nur die HTTP-Anforderungen und -Antworten zum bzw. vom Proxyport angezeigt. Für einen Connectorstart, bei dem der Connector für die Verwendung eines Proxyservers konfiguriert ist, wird beispielsweise in etwa Folgendes angezeigt:

 ![Azure AD-Netzwerkmonitor](./media/application-proxy-working-with-proxy-servers/http-requests.png)

Sie suchen jetzt präzise nach den CONNECT-Anforderungen, die beweisen, dass wir mit dem Proxyserver kommunizieren. Bei erfolgreicher Durchführung erhalten Sie die Antwort HTTP OK (200).

Wenn andere Antwortcodes angezeigt werden, z.B. 407 oder 502, weist dies darauf hin, dass für den Proxy eine Authentifizierung erforderlich ist oder dass der Datenverkehr aus einem anderen Grund nicht zugelassen wird. An diesem Punkt sollten Sie Ihr Proxyserver-Supportteam zu Rate ziehen.

### <a name="identify-failed-tcp-connection-attempts"></a>Identifizieren von fehlgeschlagenen TCP-Verbindungsversuchen

Ein anderes häufiges Szenario, das für Sie ggf. interessant ist, lautet: Der Connector versucht, eine direkte Verbindung herzustellen, ist damit aber nicht erfolgreich. 

Ein weiterer Filter des Netzwerkmonitors, mit dem Sie dies leicht identifizieren können, lautet:

property.TCPSynRetransmit

Ein SYN-Paket ist das erste Paket, das zum Herstellen einer TCP-Verbindung gesendet wird. Falls keine Antwort zurückgegeben wird, wird ein neuer Versuch für den SYN-Vorgang gestartet. Mit dem obigen Filter können Sie alle erneut übertragenen SYNs anzeigen. Anschließend können Sie prüfen, ob diese den Connectordatenverkehr betreffen. 

Mit dem folgenden Beispiel wird ein fehlgeschlagener Verbindungsversuch für den Service Bus-Port 9352 veranschaulicht:

 ![Azure AD-Netzwerkmonitor](./media/application-proxy-working-with-proxy-servers/failed-connection-attempt.png)

Falls Sie eine Antwort wie die obige sehen, bedeutet dies, das der Connector versucht, direkt mit dem Azure Service Bus-Dienst zu kommunizieren. Wenn Sie erwarten, dass der Connector direkte Verbindungen mit den Azure-Diensten herstellt, ist dies ein eindeutiger Hinweis darauf, dass ein Problem mit dem Netzwerk oder der Firewall besteht.

>[!NOTE]
>Wenn Sie die Verwendung eines Proxyservers konfiguriert haben, kann es sein, dass die Service Bus-Instanz versucht, eine direkte TCP-Verbindung herzustellen, bevor zur versuchten Verbindungsherstellung per HTTPS gewechselt wird. Behalten Sie dies im Hinterkopf.
>

Die Analyse per Netzwerkablaufverfolgung ist nicht für alle Fälle geeignet. Es kann aber ein sehr wertvolles Tool sein, wenn es darum geht, schnell Informationen zu den Abläufen in Ihrem Netzwerk zu erhalten. 

Falls bei Ihnen weiterhin Probleme mit der Connectorkonnektivität auftreten, können Sie ein Ticket für unser Supportteam erstellen, das Ihnen gern bei der weiteren Problembehandlung behilflich ist.

Informationen zum Beheben von Fehlern in Bezug auf den Anwendungsproxyconnector finden Sie unter [Problembehandlung von Anwendungsproxys](https://azure.microsoft.com/en-us/documentation/articles/active-directory-application-proxy-troubleshoot).

## <a name="next-steps"></a>Nächste Schritte

[Grundlegendes zu Azure AD-Anwendungsproxyconnectors](application-proxy-understand-connectors.md)<br>
[Installieren des Azure AD-Anwendungsproxyconnectors im Hintergrund](active-directory-application-proxy-silent-installation.md)







<!--HONumber=Feb17_HO1-->


