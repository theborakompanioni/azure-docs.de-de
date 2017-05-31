---
title: "Installieren eines lokalen Datengateways – Azure Logic Apps | Microsoft-Dokumentation"
description: "Bevor Sie auf lokale Datenquellen zugreifen, installieren Sie das lokale Datengateway für schnelle Datenübertragung und Verschlüsselung zwischen den lokalen Datenquellen und den Logik-Apps"
keywords: "Zugreifen auf Daten, lokal, Datenübertragung, Verschlüsselung, Datenquellen"
services: logic-apps
documentationcenter: 
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 47e3024e-88a0-4017-8484-8f392faec89d
ms.service: logic-apps
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 05/5/2017
ms.author: LADocs; dimazaid; estfan
ms.translationtype: Human Translation
ms.sourcegitcommit: c308183ffe6a01f4d4bf6f5817945629cbcedc92
ms.openlocfilehash: 8a1ae2ef790455383118bb55c34f6ca10fe0169e
ms.contentlocale: de-de
ms.lasthandoff: 05/17/2017


---
# <a name="install-the-on-premises-data-gateway-for-azure-logic-apps"></a>Installieren des lokalen Datengateways für Azure-Logik-Apps

Damit Ihre Logik-Apps auf lokale Datenquellen zugreifen können, müssen Sie das lokale Datengateway installieren und einrichten. Das Gateway fungiert als Brücke, die schnelle Datenübertragung und Verschlüsselung zwischen lokalen Systemen und Ihren Logik-Apps bereitstellt. Das Gateway überträgt Daten aus lokalen Quellen durch verschlüsselte Kanäle über Azure Service Bus. Der gesamte Datenverkehr stammt als sicherer ausgehender Datenverkehr vom Gateway-Agent. Erfahren Sie mehr darüber, [wie das Datengateway funktioniert](#gateway-cloud-service).

Das Gateway unterstützt Verbindungen mit diesen lokalen Datenquellen:

*   BizTalk Server
*   DB2  
*   Dateisystem
*   Informix
*   MQ
*   MySQL
*   Oracle-Datenbank
*   PostgreSQL
*   SAP-Anwendungsserver 
*   SAP-Nachrichtenserver
*   SharePoint nur für HTTP, nicht für HTTPS
*   SQL Server
*   Teradata

In diesen Schritten wird gezeigt, wie Sie zunächst das lokale Datengateway installieren, bevor Sie [eine Verbindung zwischen dem Gateway und Ihren Logik-Apps einrichten](./logic-apps-gateway-connection.md). Weitere Informationen zu unterstützten Connectors finden Sie unter [Connectors für Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list). 

Weitere Informationen zu Datengateways für andere Microsoft-Dienste finden Sie in den folgenden Artikeln:

*   [Azure Application Gateway](https://azure.microsoft.com/services/application-gateway/): [Übersicht über Application Gateway](../application-gateway/application-gateway-introduction.md)
*   [Microsoft Power BI | Lokales Datengateway](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem/)
*   [Azure Analysis Services | Lokales Datengateway](../analysis-services/analysis-services-gateway.md)
*   [Microsoft Flow | Lokales Datengateway](https://flow.microsoft.com/documentation/gateway-manage/)

<a name="requirements"></a>
## <a name="requirements"></a>Requirements (Anforderungen)

**Minimum**:

* .NET 4.5 Framework
* 64-Bit-Version von Windows 7 oder Windows Server 2008 R2 (oder höher)

**Empfohlen**:

* 8-Kern-CPU
* 8 GB Arbeitsspeicher
* 64-Bit-Version von Windows 2012 R2 (oder höher)

**Wichtige Hinweise:**:

* Installieren Sie das lokale Datengateway nur auf einem lokalen Computer.
Sie können das Gateway nicht auf einem Domänencontroller installieren.

   > [!TIP]
   > Sie müssen das Gateway nicht auf dem Computer installieren, auf dem sich die Datenquelle befindet. Um Wartezeiten zu minimieren, können Sie das Gateway so nah wie möglich zur Datenquelle oder auf demselben Computer installieren, vorausgesetzt, Sie haben die Berechtigungen.

* Installieren Sie das Gateway nicht auf einem Computer, der ausgeschaltet wird, in den Ruhezustand versetzt wird oder keine Internetverbindung herstellt, da das Gateway unter diesen Umständen nicht ausgeführt werden kann. Darüber hinaus kann die Gatewayleistung bei Einsatz eines Drahtlosnetzwerks ggf. beeinträchtigt werden.

* Sie können sich nur über ein Azure-Konto anmelden, das eine Geschäfts- oder Schul-E-Mail-Adresse hat, die von Azure Active Directory (Azure AD) verwaltet wird. Sie benötigen dieses Konto, um das lokale Datengateway einem Azure-Abonnement für ein Azure AD-basiertes-Konto zuzuordnen.

  > [!TIP] 
  > Wenn Sie ein Microsoft-Kontos haben, etwa @outlook.com, können Sie Ihr Azure-Konto dazu verwenden, [eine Geschäfts- oder Schul-E-Mail-Adresse zu erstellen](../virtual-machines/windows/create-aad-work-id.md#locate-your-default-directory-in-the-azure-classic-portal). Wenn Sie sich für ein Office 365-Angebot registriert und nicht Ihre tatsächliche Geschäfts-E-Mail-Adresse angegeben haben, kann Ihre Anmeldeadresse beispielsweise wie folgt aussehen: jeff@contoso.onmicrosoft.com. 

* Wenn Sie ein vorhandenes Gateway haben, das Sie mit einem Installer eingerichtet haben, dessen Version früher ist als 14.16.6317.4, können Sie den Speicherort Ihres Gateways nicht ändern, indem Sie den neuesten Installer ausführen. Sie können aber die neueste Version von Installer verwenden, um ein neues Gateway mit dem Speicherort einzurichten, den Sie stattdessen wünschen.
  
  Wenn Sie einen Gateway-Installer mit einer Version vor 14.16.6317.4, aber Ihr Gateway noch nicht installiert haben, können Sie den neuesten Installer herunterladen und verwenden.

<a name="install-gateway"></a>
## <a name="install-the-data-gateway"></a>Installieren des Datengateways

1.    [Laden Sie den Gateway-Installer auf einen lokalen Computer herunter, und führen Sie den Installer aus](http://go.microsoft.com/fwlink/?LinkID=820931&clcid=0x409).

2. Lesen und akzeptieren Sie die Nutzungsbedingungen und Datenschutzbestimmungen.

3. Geben Sie den Pfad auf dem lokalen Computer an, in dem Sie das Gateway installieren möchten.

4. Wenn Sie dazu aufgefordert werden, melden Sie sich mit Ihrem Azure-Geschäfts- oder -Schulkonto, nicht mit einem Microsoft-Konto an.

5. Registrieren Sie nun Ihre Gatewayinstallation beim [Gatewayclouddienst](#gateway-cloud-service). 

     Der Gatewayclouddienst verschlüsselt und speichert Ihre Anmeldeinformationen für Datenquellen und Ihre Gatewaysdetails. 
     Der Dienst leitet außerdem Abfragen und deren Ergebnisse zwischen Benutzern in der Cloud, etwa Ihre Logik-App, dem lokalen Datengateway und Ihrer lokalen Datenquelle weiter.

     1. Geben Sie einen Namen für Ihre Gatewayinstallation an, und erstellen Sie einen Wiederherstellungsschlüssel. 
     Bestätigen Sie den Wiederherstellungsschlüssel.

        > [!IMPORTANT] 
        > Der Wiederherstellungsschlüssel muss mindestens acht Zeichen enthalten. Speichern und verwahren Sie den Schlüssel an einem sicheren Ort. Sie benötigen diesen Schlüssel auch, wenn Sie ein vorhandenes Gateway migrieren, wiederherstellen oder übernehmen möchten.

     2. Um die Standardregion für den Gatewayclouddienst und Azure Service Bus zu ändern, die von Ihrer Gatewayinstallation verwendet werden, wählen Sie **Region ändern** aus.

        Beispielsweise bietet es sich an, dieselbe Region wie für Ihre Logik-App oder die Region auszuwählen, die Ihrer lokalen Datenquelle am nächsten liegt, sodass sich die Wartezeit verringern lässt. Ihre Gatewayressource und Ihre Logik-App können unterschiedliche Standorte haben.

        > [!IMPORTANT]
        > Sie können diese Region nach der Installation nicht ändern. Diese Region bewirkt auch, dass der Standort bestimmt und eingeschränkt ist, an dem Sie die Azure-Ressource für Ihr Gateway erstellen können. Daher müssen Sie, wenn Sie Ihre Gatewayressource in Azure erstellen, darauf achten, dass der Standort der Ressource mit der Region übereinstimmt, die Sie bei der Gatewayinstallation ausgewählt haben.
        > 
        > Wenn Sie für Ihr Gateway später eine andere Region verwenden möchten, müssen Sie ein neues Gateway einrichten.

     3. Wenn Sie fertig sind, wählen Sie **Konfigurieren** aus.

6. Führen Sie jetzt diese Schritte im Azure-Portal aus, damit Sie [eine Azure-Ressource für Ihr Gateway erstellen](../logic-apps/logic-apps-gateway-connection.md) können. 

Erfahren Sie mehr darüber, [wie das Datengateway funktioniert](#gateway-cloud-service).

## <a name="migrate-restore-or-take-over-an-existing-gateway"></a>Migrieren, Wiederherstellen oder Übernehmen eines vorhandenen Gateways

Um diese Aufgaben auszuführen, benötigen Sie den Wiederherstellungsschlüssel, der bei der Installation des Gateways angegeben wurde.

1. Wählen Sie im Startmenü des Computers den Eintrag **Lokales Datengateway** aus.
2. Nachdem der Installer geöffnet ist, geben Sie den Wiederherstellungsschlüssel für das Gateway an, das Sie migrieren, wiederherstellen oder übernehmen möchten.

<a name="restart-gateway"></a>
## <a name="restart-the-gateway"></a>Neustarten des Gateways

Das Gateway wird als Windows-Dienst ausgeführt. Sie können den Dienst, so wie jeden anderen Windows-Dienst, auf mehrere Arten starten und beenden. Beispielsweise können Sie eine Eingabeaufforderung mit erhöhten Berechtigungen auf dem Computer öffnen, auf dem das Gateway ausgeführt wird, und einen dieser Befehle ausführen:

* Führen Sie zum Beenden des Diensts den folgenden Befehl aus:
  
    `net stop PBIEgwService`

* Führen Sie zum Starten des Diensts den folgenden Befehl aus:
  
    `net start PBIEgwService`

### <a name="windows-service-account"></a>Windows-Dienstkonto

Das lokale Datengateway wird so eingerichtet, dass es `NT SERVICE\PBIEgwService` für die Anmeldeinformationen des Windows-Diensts verwendet. Standardmäßig hat das Gateway die Berechtigung „Anmelden als Dienst“ für den Computer, auf dem Sie das Gateway installieren.

> [!NOTE]
> Das Windows-Dienstkonto ist weder mit dem Konto, das zum Herstellen von Verbindungen mit den lokalen Datenquellen verwendet wird, noch mit dem Geschäfts-, Schul- oder Unikonto identisch, das Sie zum Anmelden bei Clouddiensten verwenden.

## <a name="configure-a-firewall-or-proxy"></a>Konfigurieren einer Firewall oder eines Proxys

Das Gateway stellt eine ausgehende Verbindung mit dem [Azure Service Bus](https://azure.microsoft.com/services/service-bus/) her. Informationen dazu, wie Sie Proxyinformationen für Ihr Gateway bereitstellen, finden Sie unter [Konfigurieren von Proxyeinstellungen für Power BI Gateways](https://powerbi.microsoft.com/documentation/powerbi-gateway-proxy/).

Um zu überprüfen, ob die Firewall oder der Proxy Verbindungen möglicherweise blockiert, vergewissern Sie sich, dass der Computer tatsächlich eine Verbindung mit dem Internet und dem [Azure Service Bus](https://azure.microsoft.com/services/service-bus/) herstellen kann. Führen Sie an einer PowerShell-Eingabeaufforderung folgenden Befehl aus:

`Test-NetConnection -ComputerName watchdog.servicebus.windows.net -Port 9350`

> [!NOTE]
> Dieser Befehl testet nur die Netzwerkverbindung und die Verbindung mit dem Azure Service Bus. Daher hat der Befehl nichts mit dem Gateway oder Gatewayclouddienst zu tun, der Ihre Anmeldeinformationen und Gatewaydetails verschlüsselt und speichert. 
>
> Außerdem ist dieser Befehl ist nur unter Windows Server 2012 R2 oder höher und Windows 8.1 oder höher verfügbar. In früheren Betriebssystemversionen können Sie Telnet verwenden, um Konnektivität zu testen. Erfahren Sie mehr über [Azure Service Bus und Hybridlösungen](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).

Ihr Ergebnis sollte dem folgenden Beispiel ähneln:

```text
ComputerName           : watchdog.servicebus.windows.net
RemoteAddress          : 70.37.104.240
RemotePort             : 5672
InterfaceAlias         : vEthernet (Broadcom NetXtreme Gigabit Ethernet - Virtual Switch)
SourceAddress          : 10.120.60.105
PingSucceeded          : False
PingReplyDetails (RTT) : 0 ms
TcpTestSucceeded       : True
```

Ist **TcpTestSucceeded** nicht auf **True** festgelegt, werden Verbindungen möglicherweise durch eine Firewall blockiert. Falls Sie alles abdecken möchten, können Sie die Werte von **ComputerName** und **Port** durch die Werte unter [Konfigurieren von Ports](#configure-ports) in diesem Thema ersetzen.

Es kann auch sein, dass die Firewall Verbindungen blockiert, die von Azure Service Bus mit Azure-Rechenzentren hergestellt werden. Trifft dies zu, gehen Sie so vor, dass Sie alle IP-Adressen für diese Rechenzentren in Ihrer Region genehmigen (Blockierung aufheben). Für diese IP-Adressen können Sie [die Azure-IP-Adressenliste hier abrufen](https://www.microsoft.com/download/details.aspx?id=41653).

## <a name="configure-ports"></a>Konfigurieren von Ports

Das Gateway erstellt eine ausgehende Verbindung mit [Azure Service Bus](https://azure.microsoft.com/services/service-bus/) und kommuniziert über Ports für ausgehenden Datenverkehr: TCP 443 (Standard), 5671, 5672, 9350 bis 9354. Das Gateway benötigt keine eingehenden Ports. Erfahren Sie mehr über [Azure Service Bus und Hybridlösungen](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).

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

<a name="gateway-cloud-service"></a>
## <a name="how-does-the-data-gateway-work"></a>Wie funktioniert das Datengateway?

Das Datengateway ermöglicht schnelle und sichere Kommunikation zwischen einem Benutzer in der Cloud, etwa Ihre Logik-App, dem Gatewayclouddienst und Ihrer lokalen Datenquelle. 

![Datenflussdiagramm-für-lokales-Datengateway](./media/logic-apps-gateway-install/how-on-premises-data-gateway-works-flow-diagram.png)

Somit geschieht Folgendes, wenn ein Benutzer in der Cloud mit einem Element interagiert, das mit Ihrer lokalen Datenquelle verbunden ist:

1. Der Gatewayclouddienst erstellt zusammen mit den verschlüsselten Anmeldeinformationen für die Datenquelle eine Abfrage und sendet die Abfrage an die Warteschlange zur Verarbeitung durch das Gateway.

2. Der Gatewayclouddienst analysiert die Abfrage und überträgt die Anforderung per Push an den Azure Service Bus.

3. Das lokale Datengateway fragt den Azure Service Bus nach ausstehenden Anforderungen ab.

4. Das Gateway ruft die Abfrage ab, entschlüsselt die Anmeldeinformationen und stellt mit diesen Anmeldeinformationen eine Verbindung mit der Datenquelle her.

5. Das Gateway sendet die Abfrage zur Ausführung an die Datenquelle.

6. Die Ergebnisse werden aus der Datenquelle zurück an das Gateway und dann an den Gatewayclouddienst gesendet. Der Gatewayclouddienst verwendet dann die Ergebnisse.

<a name="faq"></a>
## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="general"></a>Allgemein

**F**: Benötige ich ein Gateway für Datenquellen in der Cloud, z.B. SQL Azure? <br/>
**A**: Nein. Ein Gateway dient nur der Verbindung mit lokalen Datenquellen.

**F**: Muss das Gateway auf dem gleichen Computer wie die Datenquelle installiert werden? <br/>
**A**: Nein. Das Gateway stellt mithilfe der bereitgestellten Verbindungsinformationen eine Verbindung mit der Datenquelle her. Stellen Sie sich das Gateway in diesem Sinne als Clientanwendung vor. Für das Gateway ist nur die Funktion zum Herstellen der Verbindung mit dem angegebenen Servernamen erforderlich.

**F**: Warum muss ich ein Azure-Geschäfts-, -Schul- oder -Unikonto verwenden, um mich anzumelden? <br/>
**A**: Sie können das lokale Datengateway nur einem Azure-Geschäfts-, -Schul- oder -Unikonto zuordnen. Ihr Anmeldekonto ist in einem Mandanten gespeichert, der von Azure Active Directory (Azure AD) verwaltet wird. Normalerweise entspricht der UPN Ihres Azure AD-Kontos der E-Mail-Adresse.

**F**: Wo werden meine Anmeldeinformationen gespeichert ? <br/>
**A**: Die Anmeldeinformationen, die Sie für eine Datenquelle eingeben, werden verschlüsselt und im Gatewayclouddienst gespeichert. Die Anmeldeinformationen werden im lokalen Datengateway entschlüsselt.

**F**: Gibt es Anforderungen an die Netzwerkbandbreite? <br/>
**A**: Es ist ratsam, dafür zu sorgen, dass die Netzwerkverbindung über einen guten Durchsatz verfügt. Jede Umgebung ist anders, und die Menge der zu sendenden Daten wirkt sich auf die Ergebnisse aus. ExpressRoute könnte ein Durchsatzniveau zwischen lokalen und Azure-Rechenzentren gewährleisten.
Sie können mithilfe des Drittanbietertools Azure Speed Test-App messen, wie hoch der Durchsatz ist.

**F**: Wie lang ist die Wartezeit für das Ausführen von Abfragen bei einer Datenquelle aus dem Gateway? Welche Architektur ist die beste? <br/>
**A**: Um die Netzwerkwartezeit zu reduzieren, installieren Sie das Gateway so nahe wie möglich bei der Datenquelle. Wenn Sie das Gateway auf der tatsächlichen Datenquelle installieren können, wird die Wartezeit durch diese Nähe minimiert. Berücksichtigen Sie auch die Rechenzentren. Wenn für Ihren Dienst beispielsweise das Rechenzentrum „USA, Westen“ verwendet wird und Sie SQL Server auf einer Azure-VM hosten, sollte sich die Azure-VM ebenfalls in der Region „USA, Westen“ befinden. Aufgrund dieser Nähe wird die Wartezeit verringert, und es werden Gebühren für ausgehenden Datenverkehr auf der Azure-VM vermieden.

**F**: Wie werden Ergebnisse an die Cloud zurückgesendet? <br/>
**A**: Die Ergebnisse werden über Azure Service Bus gesendet.

**F**: Gibt es aus der Cloud eingehende Verbindungen mit dem Gateway? <br/>
**A**: Nein. Das Gateway verwendet ausgehende Verbindungen mit dem Azure Service Bus.

**F**: Was geschieht, wenn ich ausgehende Verbindungen blockiere? Was muss ich öffnen? <br/>
**A**: Die Ports und Hosts, die das Gateway verwendet.

**F**: Wie wird der eigentliche Windows-Dienst genannt?<br/>
**A**: In „Dienste“ hat das Gateway den Namen „Power BI Enterprise Gateway-Dienst“.

**F**: Kann der Gateway-Windows-Dienst mit einem Azure Active Directory-Konto ausgeführt werden? <br/>
**A**: Nein. Der Windows-Dienst benötigt ein gültiges Windows-Konto. Standardmäßig wird er mit der Dienst-SID „NT SERVICE\PBIEgwService“ ausgeführt.

### <a name="high-availability-and-disaster-recovery"></a>Hohe Verfügbarkeit und Notfallwiederherstellung

**F**: Welche Optionen sind für die Notfallwiederherstellung verfügbar? <br/>
**A**: Sie können den Wiederherstellungsschlüssel verwenden, um ein Gateway wiederherzustellen oder zu verschieben. Wenn Sie das Gateway installieren, geben Sie den Wiederherstellungsschlüssel an.

**F**: Welchen Vorteil hat der Wiederherstellungsschlüssel? <br/>
**A**: Der Wiederherstellungsschlüssel bietet eine Möglichkeit zum Migrieren oder Wiederherstellen Ihrer Gatewayeinstellungen nach einem Notfall.

**F**: Gibt es Pläne, mit dem Gateway Szenarien mit hoher Verfügbarkeit zu unterstützen? <br/>
**A**: Diese Szenarien sind in der Roadmap enthalten, aber es gibt noch keine Zeitachse.

## <a name="troubleshooting"></a>Problembehandlung

[!INCLUDE [existing-gateway-location-changed](../../includes/logic-apps-existing-gateway-location-changed.md)]

**F**: Wie kann ich feststellen, welche Abfragen an die lokale Datenquelle gesendet werden? <br/>
**A**: Sie können die Abfrageablaufverfolgung aktivieren. Hierin sind die gesendeten Abfragen enthalten. Denken Sie daran, die Abfrageablaufverfolgung nach Abschluss der Problembehandlung wieder auf den ursprünglichen Wert zurückzusetzen. Wenn Sie die Abfrageablaufverfolgung aktiviert lassen, werden größere Protokolle erstellt.

Sie können auch Tools anzeigen, die Ihre Datenquelle für die Verfolgung von Abfrageabläufen bietet. Sie können z.B. Erweiterte Ereignisse oder SQL Profiler für SQL Server und Analysis Services verwenden.

**F**: Wo sind die Gatewayprotokolle? <br/>
**A**: Siehe „Tools“ weiter unten in diesem Thema.

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

