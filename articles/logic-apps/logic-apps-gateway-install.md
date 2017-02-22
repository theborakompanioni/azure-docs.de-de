---
title: "Installieren des lokalen Datengateways für Logik-Apps | Microsoft-Dokumentation"
description: "Informationen zum Installieren des lokalen Datengateways für die Verwendung in einer Logik-App."
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 47e3024e-88a0-4017-8484-8f392faec89d
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/05/2016
ms.author: jehollan
translationtype: Human Translation
ms.sourcegitcommit: dc8c9eac941f133bcb3a9807334075bfba15de46
ms.openlocfilehash: 4c7cd42f6cbe9519e23e781a82c017e0119a28a5


---
# <a name="install-the-on-premises-data-gateway-for-logic-apps"></a>Installieren des lokalen Datengateways für Logik-Apps
## <a name="installation-and-configuration"></a>Installation und Konfiguration
### <a name="prerequisites"></a>Voraussetzungen
Minimum:

* .NET 4.5 Framework
* 64-Bit-Version von Windows 7 oder Windows Server 2008 R2 (oder höher)

Empfohlen:

* 8-Kern-CPU
* 8 GB Arbeitsspeicher
* 64-Bit-Version von Windows 2012 R2 (oder höher)

Damit zusammenhängende Überlegungen:

* Sie können kein Gateway auf einem Domänencontroller installieren.
* Installieren Sie kein Gateway auf einem Computer, wie z.B. einem Laptop, der ausgeschaltet werden, sich im Energiesparzustand befinden oder nicht mit dem Internet verbunden sein könnte, da das Gateway unter diesen Umständen nicht ausgeführt werden kann. Darüber hinaus könnte die Gatewayleistung bei Einsatz eines drahtlosen Netzwerks beeinträchtigt werden.

### <a name="install-a-gateway"></a>Installieren eines Gateways
Sie erhalten [hier das Installationsprogramm für das lokale Datengateway](http://go.microsoft.com/fwlink/?LinkID=820931&clcid=0x409).

Geben Sie **Lokales Datengateway** als Modus an, melden Sie sich mit Ihrem Geschäfts-, Schul- oder Unikonto an, und konfigurieren Sie ein neues Gateway. Sie können auch ein vorhandenes Gateway migrieren, wiederherstellen oder übernehmen.

* Geben Sie zum Konfigurieren eines Gateways einen **Namen** und einen **Wiederherstellungsschlüssel** dafür ein, und klicken oder tippen Sie auf **Konfigurieren**.
  
    Geben Sie einen Wiederherstellungsschlüssel an, der mindestens acht Zeichen enthält, und bewahren Sie eine Notiz davon an einem sicheren Ort auf. Sie benötigen diesen Schlüssel, wenn Sie das zugehörige Gateway migrieren, wiederherstellen oder übernehmen möchten.
* Geben Sie zum Migrieren, Wiederherstellen oder Übernehmen eines vorhandenen Gateways den Wiederherstellungsschlüssel an, der beim Erstellen des Gateways angegeben wurde.

### <a name="restart-the-gateway"></a>Neustarten des Gateways
Das Gateway wird als Windows-Dienst ausgeführt, und wie jeden anderen Windows-Dienst können Sie es auf mehrere Arten starten und beenden. Sie können z.B. eine Eingabeaufforderung mit erhöhten Berechtigungen auf dem Computer öffnen, auf dem das Gateway ausgeführt wird, und einen dieser Befehle ausführen:

* Führen Sie zum Beenden des Diensts den folgenden Befehl aus:
  
    `net stop PBIEgwService`
* Führen Sie zum Starten des Diensts den folgenden Befehl aus:
  
    `net start PBIEgwService`

### <a name="configure-a-firewall-or-proxy"></a>Konfigurieren einer Firewall oder eines Proxys
Informationen dazu, wie Sie Proxyinformationen für Ihr Gateway bereitstellen, finden Sie unter [Konfigurieren von Proxyeinstellungen für Power BI Gateways](https://powerbi.microsoft.com/en-us/documentation/powerbi-gateway-proxy/).

Sie können überprüfen, ob Ihre Firewall oder Ihr Proxy vielleicht Verbindungen blockiert, indem Sie den folgenden Befehl an einer PowerShell-Eingabeaufforderung ausführen. Hiermit wird die Verbindung mit dem Azure Service Bus getestet. Dieser Test prüft nur die Netzwerkkonnektivität und hat nichts mit dem Cloudserverdienst oder dem Gateway zu tun. Er unterstützt die Feststellung, ob Ihr Computer tatsächlich mit dem Internet verbunden ist.

`Test-NetConnection -ComputerName watchdog.servicebus.windows.net -Port 9350`

Das Ergebnis sollte dem folgenden Beispiel ähneln. Wenn **TcpTestSucceeded** nicht „Wahr“ ist, werden Sie möglicherweise durch eine Firewall blockiert.

```
ComputerName           : watchdog.servicebus.windows.net
RemoteAddress          : 70.37.104.240
RemotePort             : 5672
InterfaceAlias         : vEthernet (Broadcom NetXtreme Gigabit Ethernet - Virtual Switch)
SourceAddress          : 10.120.60.105
PingSucceeded          : False
PingReplyDetails (RTT) : 0 ms
TcpTestSucceeded       : True
```

Wenn Sie alles abdecken möchten, können Sie die Werte **ComputerName** und **Port** durch die Werte unter [Konfigurieren von Ports](#configure-ports) weiter unten in diesem Thema ersetzen.

Die Firewall könnte auch die Verbindungen blockieren, die Azure Service Bus zu Azure-Rechenzentren ermöglicht. Wenn dies der Fall ist, sollten Sie eine Positivliste aller (zugelassenen) IP-Adressen für Ihre Region für diese Rechenzentren aufstellen. Sie erhalten [hier eine Liste der Azure-IP-Adressen](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="configure-ports"></a>Konfigurieren von Ports
Das Gateway stellt eine ausgehende Verbindung mit dem Azure Service Bus her. Es kommuniziert über ausgehende Ports: TCP 443 (Standard), 5671 5672, 9350 bis 9354. Das Gateway benötigt keine eingehenden Ports.

Hier erfahren Sie mehr über [Hybridlösungen](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).

| DOMÄNENNAMEN | AUSGEHENDE PORTS | Beschreibung |
| --- | --- | --- |
| *. analysis.windows.net |443 |HTTPS |
| *.login.windows.net |443 |HTTPS |
| *.servicebus.windows.net |5671-5672 |Advanced Message Queuing Protocol (AMQP) |
| *.servicebus.windows.net |443, 9350-9354 |Listener auf Service Bus Relay per TCP (443 für Access Control-Tokenbeschaffung erforderlich) |
| *.frontend.clouddatahub.net |443 |HTTPS |
| *.core.windows.net |443 |HTTPS |
| login.microsoftonline.com |443 |HTTPS |
| *.msftncsi.com |443 |Wird verwendet, um die Internetkonnektivität zu testen, falls der Power BI-Dienst das Gateway nicht erreicht. |

Wenn Sie eine Positivliste der IP-Adressen anstelle der Domänen aufstellen müssen, können Sie die [Microsoft Azure Datacenter IP-Bereicheliste](https://www.microsoft.com/download/details.aspx?id=41653)herunterladen und verwenden. In einigen Fällen werden die Azure Service Bus-Verbindungen mit IP-Adressen anstelle der vollständig qualifizierten Domänennamen hergestellt.

### <a name="sign-in-account"></a>Anmeldekonto
Benutzer melden sich entweder mit einem Unternehmens- oder Schulkonto an. Dies ist Ihr Organisationskonto. Wenn Sie sich für ein Office 365-Angebot registriert und nicht Ihre tatsächliche geschäftliche E-Mail-Adresse angeben haben, sieht sie möglicherweise ähnlich aus wie jeff@contoso.onmicrosoft.com. Ihr Konto in einem Clouddienst wird innerhalb eines Mandanten in Azure Active Directory (AAD) gespeichert. In den meisten Fällen entspricht der UPN Ihres AAD-Kontos der E-Mail-Adresse.

### <a name="windows-service-account"></a>Windows-Dienstkonto
Das lokale Datengateway ist zur Verwendung von NT SERVICE\PBIEgwService für die Anmeldeinformationen für den Windows-Dienst konfiguriert. Standardmäßig hat es das Recht zur Anmeldung als Dienst. Dies ist im Kontext des Computers, auf dem Sie das Gateway installieren.

Es ist nicht das Konto zum Herstellen der Verbindung mit lokalen Datenquellen oder dem Unternehmens- bzw. Schulkonto, mit dem Sie sich bei Clouddiensten anmelden.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen
### <a name="general"></a>Allgemein
**Frage**: Welche Datenquellen unterstützt das Gateway?<br/>
**Antwort**: Zum Zeitpunkt der Erstellung dieses Dokuments SQL Server.

**Frage**: Benötige ich ein Gateway für Datenquellen in der Cloud, z.B. SQL Azure? <br/>
**Antwort**: Nein. Ein Gateway dient nur der Verbindung mit lokalen Datenquellen.

**Frage**: Wie wird der eigentliche Windows-Dienst genannt?<br/>
**Antwort**: In den Diensten wird das Gateway „Power BI Enterprise Gateway-Dienst“ genannt.

**Frage**: Gibt es aus der Cloud eingehende Verbindungen mit dem Gateway? <br/>
**Antwort**: Nein. Das Gateway verwendet ausgehende Verbindungen mit dem Azure Service Bus.

**Frage**: Was geschieht, wenn ich ausgehende Verbindungen blockiere? Was muss ich öffnen? <br/>
**Antwort**: Die Ports und Hosts, die das Gateway verwendet.

**Frage**: Muss das Gateway auf dem gleichen Computer wie die Datenquelle installiert werden? <br/>
**Antwort**: Nein. Das Gateway stellt mithilfe der bereitgestellten Verbindungsinformationen eine Verbindung mit der Datenquelle her. Stellen Sie sich das Gateway in diesem Sinne als Clientanwendung vor. Es muss lediglich die Möglichkeit haben, eine Verbindung mit dem angegebenen Servernamen herstellen zu können.

**Frage**: Wie hoch ist die Latenzzeit für das Ausführen von Abfragen bei einer Datenquelle aus dem Gateway? Welche Architektur ist die beste? <br/>
**Antwort**: Um die Netzwerklatenzzeit zu reduzieren, installieren Sie das Gateway so nahe wie möglich bei der Datenquelle. Wenn Sie das Gateway auf der tatsächlichen Datenquelle installieren können, wird die Latenzzeit minimiert. Berücksichtigen Sie auch die Rechenzentren. Wenn Ihr Dienst z.B. das Rechenzentrum USA, Westen nutzt, und Sie SQL Server auf einer Azure-VM gehostet haben, sollte sich die Azure-VM ebenfalls in USA, Westen befinden. Dies minimiert Latenzzeiten und vermeidet Ausgangsgebühren auf der Azure-VM.

**Frage**: Gibt es Anforderungen an die Netzwerkbandbreite? <br/>
**Antwort**: Ihre Netzwerkverbindung sollte einen guten Durchsatz aufweisen. Jede Umgebung ist anders, und die Menge der zu sendenden Daten wirkt sich auf die Ergebnisse aus. ExpressRoute könnte ein Durchsatzniveau zwischen lokalen und Azure-Rechenzentren gewährleisten.

Sie können mithilfe des Drittanbietertools Azure Speed Test-App messen, wie hoch der Durchsatz ist.

**Frage**: Kann das Gateway den Windows-Dienst mit einem Azure Active Directory-Konto ausführen? <br/>
**Antwort**: Nein. Der Windows-Dienst benötigt ein gültiges Windows-Konto. Standardmäßig wird er mit der Dienst-SID, NT SERVICE\PBIEgwService ausgeführt.

**Frage**: Wie werden Ergebnisse an die Cloud zurückgesendet? <br/>
**Antwort**: Dies erfolgt über den Azure Service Bus. Weitere Informationen erhalten Sie, wenn Sie die Funktionsweise betrachten.

**Frage**: Wo werden meine Anmeldeinformationen gespeichert ? <br/>
**Antwort**: Die Anmeldeinformationen, die Sie für eine Datenquelle eingeben, werden verschlüsselt im Gatewayclouddienst gespeichert. Die Anmeldeinformationen werden lokal im Gateway entschlüsselt.

### <a name="high-availabilitydisaster-recovery"></a>Hohe Verfügbarkeit/Notfallwiederherstellung
**Frage**: Gibt es Pläne, mit dem Gateway Szenarien mit hoher Verfügbarkeit zu aktivieren? <br/>
**Antwort**: Dies ist in der Roadmap enthalten, aber wir haben dafür noch keinen Zeitplan.

**Frage**: Welche Optionen sind für die Notfallwiederherstellung verfügbar? <br/>
**Antwort**: Sie können den Wiederherstellungsschlüssel verwenden, um ein Gateway wiederherzustellen oder zu verschieben. Wenn Sie das Gateway installieren, geben Sie den Wiederherstellungsschlüssel an.

**Frage**: Welchen Vorteil hat der Wiederherstellungsschlüssel? <br/>
**Antwort**: Er bietet eine Möglichkeit zum Migrieren oder Wiederherstellen Ihrer Gatewayeinstellungen nach einem Notfall.

### <a name="troubleshooting"></a>Problembehandlung
**Frage**: Wo sind die Gatewayprotokolle? <br/>
**Antwort**: Siehe „Tools“ weiter unten in diesem Thema.

**Frage**: Wie kann ich feststellen, welche Abfragen an die lokale Datenquelle gesendet werden? <br/>
**Antwort**: Sie können die Abfrageablaufverfolgung aktivieren, die die gesendeten Abfragen beinhaltet. Denken Sie daran, sie nach Abschluss der Problembehandlung wieder auf den ursprünglichen Wert zurückzusetzen. Wenn Sie die Abfrageablaufverfolgung aktiviert lassen, werden die Protokolle größer.

Sie können auch Tools anzeigen, die Ihre Datenquelle für die Verfolgung von Abfrageabläufen bietet. Sie können z.B. Erweiterte Ereignisse oder SQL Profiler für SQL Server und Analysis Services verwenden.

## <a name="how-the-gateway-works"></a>So funktioniert das Gateway
Wenn ein Benutzer mit einem Element interagiert, das mit einer lokalen Datenquelle verbunden ist, geschieht Folgendes:

1. Der Clouddienst erstellt zusammen mit den verschlüsselten Anmeldeinformationen für die Datenquelle eine Abfrage und sendet die Abfrage an die Warteschlange zur Verarbeitung durch das Gateway.
2. Der Dienst analysiert die Abfrage und verschiebt die Anforderung in den Azure Service Bus.
3. Das lokale Datengateway fragt den Azure Service Bus nach ausstehenden Anforderungen ab.
4. Das Gateway ruft die Abfrage ab, entschlüsselt die Anmeldeinformationen und stellt mit diesen Anmeldeinformationen eine Verbindung mit der/den Datenquelle(n) her.
5. Das Gateway sendet die Abfrage zur Ausführung an die Datenquelle.
6. Die Ergebnisse werden aus der Datenquelle zurück an das Gateway und dann an den Clouddienst gesendet. Der Dienst verwendet dann die Ergebnisse.

## <a name="troubleshooting"></a>Problembehandlung
### <a name="update-to-the-latest-version"></a>Update auf die aktuelle Version
Eine Vielzahl von Problemen kann auftreten, wenn die Gatewayversion veraltet ist.  Es hat sich allgemein bewährt, sicherzustellen, dass Sie die aktuelle Version installiert haben.  Wenn Sie das Gateway für einen Monat oder länger nicht aktualisiert haben, sollten Sie in Betracht ziehen, die neueste Version des Gateways zu installieren und festzustellen, ob Sie das Problem reproduzieren können.

### <a name="error-failed-to-add-user-to-group--2147463168-pbiegwservice-performance-log-users-"></a>Fehler: Fehler beim Hinzufügen des Benutzers zur Gruppe. (-2147463168 PBIEgwService Leistungsprotokollbenutzer)
Dieser Fehler kann angezeigt werden, wenn Sie versuchen, das Gateway auf einem Domänencontroller zu installieren, der nicht unterstützt wird. Sie müssen das Gateway auf einem Computer bereitstellen, der kein Domänencontroller ist.

## <a name="tools"></a>Tools
### <a name="collecting-logs-from-the-gateway-configurator"></a>Sammeln von Protokollen aus dem Gatewaykonfigurator
Sie können mehrere Protokolle für das Gateway sammeln. Beginnen Sie immer mit den Protokollen!

#### <a name="installer-logs"></a>Installationsprotokolle
`%localappdata%\Temp\Power_BI_Gateway_–Enterprise.log`

#### <a name="configuration-logs"></a>Konfigurationsprotokolle
`%localappdata%\Microsoft\Power BI Enterprise Gateway\GatewayConfigurator.log`

#### <a name="enterprise-gateway-service-logs"></a>Enterprise-Gatewaydienstprotokolle
`C:\Users\PBIEgwService\AppData\Local\Microsoft\Power BI Enterprise Gateway\EnterpriseGateway.log`

#### <a name="event-logs"></a>Ereignisprotokolle
Datenverwaltungsgateway- und PowerBIGateway-Protokoll befinden sich unter **Anwendungs- und Dienstprotokolle**.

### <a name="fiddler-trace"></a>Fiddler-Ablaufverfolgung
[Fiddler](http://www.telerik.com/fiddler) ist ein kostenloses Tool von Telerik, das HTTP-Datenverkehr überwacht.  Sie können den ein- und ausgehenden Datenverkehr des Power BI-Diensts auf dem Clientcomputer verfolgen. Dies kann Fehler und andere in Zusammenhang stehende Informationen anzeigen.

## <a name="next-steps"></a>Nächste Schritte
* [Herstellen einer lokalen Verbindung mit Logik-Apps](../logic-apps/logic-apps-gateway-connection.md)
* [Unternehmensintegrationsfeatures](../logic-apps/logic-apps-enterprise-integration-overview.md)
* [Logik-Apps-Connectors](../connectors/apis-list.md)




<!--HONumber=Jan17_HO3-->


