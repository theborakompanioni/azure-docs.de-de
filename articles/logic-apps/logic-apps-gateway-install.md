---
title: "Installieren eines lokalen Datengateways – Azure Logic Apps | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie über Logik-Apps auf lokale Daten zugreifen, indem Sie ein lokales Datengateway installieren."
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
ms.sourcegitcommit: 97acd09d223e59fbf4109bc8a20a25a2ed8ea366
ms.openlocfilehash: 23b806556667539156ee399e90c7bd4af808804f
ms.lasthandoff: 03/10/2017


---
# <a name="install-an-on-premises-data-gateway-for-azure-logic-apps"></a>Installieren eines lokalen Datengateways für Azure Logic Apps

Das lokale Datengateway unterstützt die folgenden Datenquellenverbindungen:

*   BizTalk Server
*    DB2  
*   Dateisystem
*   Informix
*   MQ
*    Oracle-Datenbank 
*   SAP-Anwendungsserver 
*   SAP-Nachrichtenserver
*    SQL Server

Weitere Informationen zu diesen Verbindungen finden Sie unter [Connectors für Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list).

## <a name="installation-and-configuration"></a>Installation und Konfiguration

### <a name="requirements"></a>Anforderungen

Minimum:

* .NET 4.5 Framework
* 64-Bit-Version von Windows 7 oder Windows Server 2008 R2 (oder höher)

Empfohlen:

* 8-Kern-CPU
* 8 GB Arbeitsspeicher
* 64-Bit-Version von Windows 2012 R2 (oder höher)

Damit zusammenhängende Überlegungen:

* Installieren Sie das lokale Datengateway nur auf einem lokalen Computer.
Sie können das Gateway nicht auf einem Domänencontroller installieren.

* Installieren Sie das Gateway nicht auf einem Computer, der ggf. ausgeschaltet wird, in den Ruhezustand versetzt wird oder keine Internetverbindung herstellt, da das Gateway unter diesen Umständen nicht ausgeführt werden kann. Darüber hinaus kann die Gatewayleistung bei Einsatz eines Drahtlosnetzwerks ggf. beeinträchtigt werden.

* Sie können nur eine Geschäfts-, Schul- oder Uni-E-Mail-Adresse in Azure verwenden, um das lokale Datengateway Ihrem Konto (auf Azure Active Directory-Basis) zuzuordnen.

    Bei Verwendung eines Microsoft-Kontos, z.B. @outlook.com, können Sie mit Ihrem Azure-Konto eine   [Geschäfts-, Schul- oder Uni-E-Mail-Adresse erstellen](../virtual-machines/virtual-machines-windows-create-aad-work-id.md#locate-your-default-directory-in-the-azure-classic-portal).

### <a name="install-the-gateway"></a>Installieren des Gateways

1.    [Laden Sie das Installationsprogramm für das lokale Datengateway hier herunter](http://go.microsoft.com/fwlink/?LinkID=820931&clcid=0x409).

2.    Geben Sie als Modus **Lokales Datengateway** an.

3. Melden Sie sich mit Ihrem Azure-Konto (Geschäfts-, Schul- oder Unikonto) an. 

4. Richten Sie ein neues Gateway ein. Sie können auch ein vorhandenes Gateway migrieren, wiederherstellen oder übernehmen.

    Geben Sie zum Konfigurieren eines Gateways einen Namen für Ihr Gateway und einen Wiederherstellungsschlüssel ein, und wählen Sie dann die Option **Konfigurieren**.
  
    Geben Sie einen Wiederherstellungsschlüssel an, der mindestens acht Zeichen enthält, und bewahren Sie ihn an einem sicheren Ort auf. Sie benötigen diesen Schlüssel, wenn Sie das Gateway migrieren, wiederherstellen oder übernehmen möchten.

    Geben Sie zum Migrieren, Wiederherstellen oder Übernehmen eines vorhandenen Gateways den Wiederherstellungsschlüssel an, der beim Erstellen des Gateways angegeben wurde.

### <a name="restart-the-gateway"></a>Neustarten des Gateways

Das Gateway wird als Windows-Dienst ausgeführt, und Sie können den Dienst wie jeden anderen Windows-Dienst auf mehrere Arten starten und beenden. Beispielsweise können Sie eine Eingabeaufforderung mit erhöhten Berechtigungen auf dem Computer öffnen, auf dem das Gateway ausgeführt wird, und einen dieser Befehle ausführen:

* Führen Sie zum Beenden des Diensts den folgenden Befehl aus:
  
    `net stop PBIEgwService`

* Führen Sie zum Starten des Diensts den folgenden Befehl aus:
  
    `net start PBIEgwService`

### <a name="configure-a-firewall-or-proxy"></a>Konfigurieren einer Firewall oder eines Proxys

Informationen dazu, wie Sie Proxyinformationen für Ihr Gateway bereitstellen, finden Sie unter [Konfigurieren von Proxyeinstellungen für Power BI Gateways](https://powerbi.microsoft.com/documentation/powerbi-gateway-proxy/).

Sie können überprüfen, ob Ihre Firewall oder Ihr Proxy ggf. Verbindungen blockiert, indem Sie den folgenden Befehl an einer PowerShell-Eingabeaufforderung ausführen. Mit diesem Befehl werden die Azure Service Bus-Konnektivität und ansonsten nur die Netzwerkkonnektivität getestet. Der Befehl bezieht sich also nicht auf den Cloudserverdienst oder das Gateway. Mit diesem Test kann ermittelt werden, ob für Ihren Computer eine Internetverbindung hergestellt werden kann.

`Test-NetConnection -ComputerName watchdog.servicebus.windows.net -Port 9350`

Das Ergebnis sollte dem folgenden Beispiel ähneln. Wenn **TcpTestSucceeded** nicht „Wahr“ ist, wird der Vorgang unter Umständen durch eine Firewall blockiert.

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

Falls Sie alles abdecken möchten, können Sie die Werte **ComputerName** und **Port** durch die Werte unter [Konfigurieren von Ports](#configure-ports) in diesem Thema ersetzen.

Es kann auch sein, dass die Firewall Verbindungen blockiert, die von Azure Service Bus mit Azure-Rechenzentren hergestellt werden. Gehen Sie in diesem Fall so vor, dass Sie alle IP-Adressen für diese Rechenzentren in Ihrer Region genehmigen (Blockierung aufheben).
Sie erhalten [hier eine Liste der Azure-IP-Adressen](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="configure-ports"></a>Konfigurieren von Ports
Das Gateway erstellt eine ausgehende Verbindung mit Azure Service Bus und kommuniziert über Ports für ausgehenden Datenverkehr: TCP 443 (Standard), 5671, 5672, 9350 bis 9354. Das Gateway benötigt keine eingehenden Ports.

Hier erfahren Sie mehr über [Hybridlösungen](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).

| DOMÄNENNAMEN | AUSGEHENDE PORTS | Beschreibung |
| --- | --- | --- |
| *. analysis.windows.net | 443 | HTTPS | 
| *.login.windows.net | 443 | HTTPS | 
| *.servicebus.windows.net | 5671-5672 | Advanced Message Queuing Protocol (AMQP) | 
| *.servicebus.windows.net | 443, 9350-9354 | Listener auf Service Bus Relay per TCP (443 für Access Control-Tokenbeschaffung erforderlich) | 
| *.frontend.clouddatahub.net | 443 | HTTPS | 
| *.core.windows.net | 443 | HTTPS | 
| login.microsoftonline.com | 443 | HTTPS | 
| *.msftncsi.com | 443 | Wird verwendet, um die Internetkonnektivität zu testen, wenn der Power BI-Dienst das Gateway nicht erreicht. | 

Wenn Sie anstelle der Domänen IP-Adressen genehmigen müssen, können Sie die [Liste mit den IP-Bereichen für Microsoft Azure Datacenter](https://www.microsoft.com/download/details.aspx?id=41653) herunterladen und verwenden. In einigen Fällen werden die Azure Service Bus-Verbindungen nicht mithilfe von vollständig qualifizierten Domänennamen hergestellt, sondern mit IP-Adressen.

### <a name="sign-in-accounts"></a>Anmeldekonten

Sie können sich mit einem Geschäfts-, Schul- oder Unikonto anmelden, bei dem es sich um ein Konto Ihrer Organisation handelt. Wenn Sie sich für ein Office 365-Angebot registriert und nicht Ihre tatsächliche geschäftliche E-Mail-Adresse angegeben haben, kann Ihre Anmeldeadresse beispielsweise wie folgt aussehen: jeff@contoso.onmicrosoft.com. Ihr Konto in einem Clouddienst wird innerhalb eines Mandanten in Azure Active Directory (Azure AD) gespeichert. Normalerweise entspricht der UPN Ihres Azure AD-Kontos der E-Mail-Adresse.

### <a name="windows-service-account"></a>Windows-Dienstkonto

Für die Anmeldeinformationen für den Windows-Dienst ist das lokale Datengateway für die Verwendung von „NT SERVICE\PBIEgwService“ konfiguriert. Standardmäßig verfügt das Gateway über die Berechtigung „Anmelden als Dienst“ im Kontext des Computers, auf dem Sie das Gateway installieren.

Dieses Dienstkonto ist nicht das Konto, das zum Herstellen der Verbindung mit den lokalen Datenquellen verwendet wird, und auch nicht das Geschäfts-, Schul- oder Unikonto, das Sie zum Anmelden an den Clouddiensten verwenden.

## <a name="how-the-gateway-works"></a>So funktioniert das Gateway
Wenn andere Benutzer mit einem Element interagieren, das mit einer lokalen Datenquelle verbunden ist, geschieht Folgendes:

1. Der Clouddienst erstellt zusammen mit den verschlüsselten Anmeldeinformationen für die Datenquelle eine Abfrage und sendet die Abfrage an die Warteschlange zur Verarbeitung durch das Gateway.
2. Der Dienst analysiert die Abfrage und verschiebt die Anforderung in den Azure Service Bus.
3. Das lokale Datengateway fragt den Azure Service Bus nach ausstehenden Anforderungen ab.
4. Das Gateway ruft die Abfrage ab, entschlüsselt die Anmeldeinformationen und stellt mit diesen Anmeldeinformationen eine Verbindung mit der Datenquelle her.
5. Das Gateway sendet die Abfrage zur Ausführung an die Datenquelle.
6. Die Ergebnisse werden aus der Datenquelle zurück an das Gateway und dann an den Clouddienst gesendet. Der Dienst verwendet dann die Ergebnisse.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="general"></a>Allgemein

**Frage**: Benötige ich ein Gateway für Datenquellen in der Cloud, z.B. SQL Azure? <br/>
**Antwort**: Nein. Ein Gateway dient nur der Verbindung mit lokalen Datenquellen.

**Frage**: Wie wird der eigentliche Windows-Dienst genannt?<br/>
**Antwort**: In den Diensten wird das Gateway „Power BI Enterprise Gateway-Dienst“ genannt.

**Frage**: Gibt es aus der Cloud eingehende Verbindungen mit dem Gateway? <br/>
**Antwort**: Nein. Das Gateway verwendet ausgehende Verbindungen mit dem Azure Service Bus.

**Frage**: Was geschieht, wenn ich ausgehende Verbindungen blockiere? Was muss ich öffnen? <br/>
**Antwort**: Die Ports und Hosts, die das Gateway verwendet.

**Frage**: Muss das Gateway auf dem gleichen Computer wie die Datenquelle installiert werden? <br/>
**Antwort**: Nein. Das Gateway stellt mithilfe der bereitgestellten Verbindungsinformationen eine Verbindung mit der Datenquelle her. Stellen Sie sich das Gateway in diesem Sinne als Clientanwendung vor. Für das Gateway ist nur die Funktion zum Herstellen der Verbindung mit dem angegebenen Servernamen erforderlich.

**Frage**: Wie hoch ist die Latenzzeit für das Ausführen von Abfragen bei einer Datenquelle aus dem Gateway? Welche Architektur ist die beste? <br/>
**Antwort**: Um die Netzwerklatenzzeit zu reduzieren, installieren Sie das Gateway so nahe wie möglich bei der Datenquelle. Wenn Sie das Gateway auf der tatsächlichen Datenquelle installieren können, wird die Wartezeit durch diese Nähe minimiert. Berücksichtigen Sie auch die Rechenzentren. Wenn für Ihren Dienst beispielsweise das Rechenzentrum „USA, Westen“ verwendet wird und Sie SQL Server auf einer Azure-VM hosten, sollte sich die Azure-VM ebenfalls in der Region „USA, Westen“ befinden. Aufgrund dieser Nähe wird die Wartezeit verringert, und es werden Gebühren für ausgehenden Datenverkehr auf der Azure-VM vermieden.

**Frage**: Gibt es Anforderungen an die Netzwerkbandbreite? <br/>
**Antwort**: Es ist ratsam, dafür zu sorgen, dass die Netzwerkverbindung über einen guten Durchsatz verfügt. Jede Umgebung ist anders, und die Menge der zu sendenden Daten wirkt sich auf die Ergebnisse aus. ExpressRoute könnte ein Durchsatzniveau zwischen lokalen und Azure-Rechenzentren gewährleisten.

Sie können mithilfe des Drittanbietertools Azure Speed Test-App messen, wie hoch der Durchsatz ist.

**Frage**: Kann das Gateway den Windows-Dienst mit einem Azure Active Directory-Konto ausführen? <br/>
**Antwort**: Nein. Der Windows-Dienst benötigt ein gültiges Windows-Konto. Standardmäßig wird er mit der Dienst-SID „NT SERVICE\PBIEgwService“ ausgeführt.

**Frage**: Wie werden Ergebnisse an die Cloud zurückgesendet? <br/>
**Antwort**: Die Ergebnisse werden über Azure Service Bus gesendet.

**Frage**: Wo werden meine Anmeldeinformationen gespeichert ? <br/>
**Antwort**: Die Anmeldeinformationen, die Sie für eine Datenquelle eingeben, werden verschlüsselt und im Gatewayclouddienst gespeichert. Die Anmeldeinformationen werden auf dem lokalen Gateway entschlüsselt.

### <a name="high-availabilitydisaster-recovery"></a>Hohe Verfügbarkeit/Notfallwiederherstellung
**Frage**: Gibt es Pläne, mit dem Gateway Szenarien mit hoher Verfügbarkeit zu aktivieren? <br/>
**Antwort**: Diese Szenarien sind in der Roadmap enthalten, aber es gibt noch keine Zeitachse.

**Frage**: Welche Optionen sind für die Notfallwiederherstellung verfügbar? <br/>
**Antwort**: Sie können den Wiederherstellungsschlüssel verwenden, um ein Gateway wiederherzustellen oder zu verschieben. Wenn Sie das Gateway installieren, geben Sie den Wiederherstellungsschlüssel an.

**Frage**: Welchen Vorteil hat der Wiederherstellungsschlüssel? <br/>
**Antwort**: Der Wiederherstellungsschlüssel bietet eine Möglichkeit zum Migrieren oder Wiederherstellen Ihrer Gatewayeinstellungen nach einem Notfall.

## <a name="troubleshooting"></a>Problembehandlung

**Frage**: Wo sind die Gatewayprotokolle? <br/>
**Antwort**: Siehe „Tools“ weiter unten in diesem Thema.

**Frage**: Wie kann ich feststellen, welche Abfragen an die lokale Datenquelle gesendet werden? <br/>
**Antwort**: Sie können die Abfrageablaufverfolgung aktivieren. Hierin sind die gesendeten Abfragen enthalten. Denken Sie daran, die Abfrageablaufverfolgung nach Abschluss der Problembehandlung wieder auf den ursprünglichen Wert zurückzusetzen. Wenn Sie die Abfrageablaufverfolgung aktiviert lassen, werden größere Protokolle erstellt.

Sie können auch Tools anzeigen, die Ihre Datenquelle für die Verfolgung von Abfrageabläufen bietet. Sie können z.B. Erweiterte Ereignisse oder SQL Profiler für SQL Server und Analysis Services verwenden.

### <a name="update-to-the-latest-version"></a>Update auf die aktuelle Version

Es können vermehrt Probleme auftreten, wenn die Gatewayversion veraltet ist. Achten Sie daher am besten darauf, dass Sie immer die aktuelle Version verwenden. Wenn Sie das Gateway für einen Monat oder länger nicht aktualisiert haben, sollten Sie erwägen, die neueste Version des Gateways zu installieren und zu ermitteln, ob Sie das Problem reproduzieren können.

### <a name="error-failed-to-add-user-to-group--2147463168-pbiegwservice-performance-log-users"></a>Fehler: Fehler beim Hinzufügen des Benutzers zur Gruppe. (-2147463168 PBIEgwService Leistungsprotokollbenutzer)

Dieser Fehler wird ggf. angezeigt, wenn Sie versuchen, das Gateway auf einem Domänencontroller zu installieren, der nicht unterstützt wird. Stellen Sie sicher, dass Sie das Gateway auf einem Computer bereitstellen, beim dem es sich nicht um einen Domänencontroller handelt.

## <a name="tools"></a>Tools
### <a name="collect-logs-from-the-gateway-configurer"></a>Erfassen von Protokollen über den Gatewaykonfigurierer

Sie können mehrere Protokolle für das Gateway sammeln. Beginnen Sie immer mit den Protokollen!

#### <a name="installer-logs"></a>Installationsprotokolle

`%localappdata%\Temp\Power_BI_Gateway_–Enterprise.log`

#### <a name="configuration-logs"></a>Konfigurationsprotokolle

`%localappdata%\Microsoft\Power BI Enterprise Gateway\GatewayConfigurator.log`

#### <a name="enterprise-gateway-service-logs"></a>Enterprise-Gatewaydienstprotokolle

`C:\Users\PBIEgwService\AppData\Local\Microsoft\Power BI Enterprise Gateway\EnterpriseGateway.log`

#### <a name="event-logs"></a>Ereignisprotokolle

Sie finden das Datenverwaltungsgateway- und PowerBIGateway-Protokoll unter **Anwendungs- und Dienstprotokolle**.

### <a name="fiddler-trace"></a>Fiddler-Ablaufverfolgung

[Fiddler](http://www.telerik.com/fiddler) ist ein kostenloses Tool von Telerik, das HTTP-Datenverkehr überwacht. Sie können diesen Datenverkehr des Power BI-Diensts auf dem Clientcomputer verfolgen. Für den Dienst werden ggf. Fehler und andere zugehörige Informationen angezeigt.

## <a name="next-steps"></a>Nächste Schritte
    
* [Herstellen einer Verbindung mit dem lokalen Datengateway für Logik-Apps](../logic-apps/logic-apps-gateway-connection.md)
* [Unternehmensintegrationsfeatures](../logic-apps/logic-apps-enterprise-integration-overview.md)
* [Connectors für Azure Logic Apps](../connectors/apis-list.md)

