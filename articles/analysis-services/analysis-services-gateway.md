---
title: Lokales Datengateway | Microsoft Docs
description: Ein lokales Gateway ist erforderlich, wenn der Analysis Services-Server in Azure mit lokalen Datenquellen verbunden wird.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: cd596155-b608-4a34-935e-e45c95d884a9
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/21/2017
ms.author: owend
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: 8c142c5e89c5e4eb45bddd0943a6a130cc876f5a
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---
# <a name="connecting-to-on-premises-data-sources-with-azure-on-premises-data-gateway"></a>Herstellen einer Verbindung mit lokalen Datenquellen mit dem lokalen Azure-Datengateway
Das lokale Datengateway fungiert als Brücke für eine sichere Datenübertragung zwischen lokalen Datenquellen und den Azure Analysis Services-Servern in der Cloud. Zusätzlich zur Verwendung von mehreren Azure Analysis Services-Servern in derselben Region funktioniert die neueste Version des Gateways auch mit Azure Logic Apps, Power BI, Power Apps und Microsoft Flow. Sie können einem einzelnen Gateway mehrere Dienste in derselben Region zuordnen. 

 Azure Analysis Services erfordert eine Gatewayressource in derselben Region. Wenn Sie z.B. über Azure Analysis Services-Server in der Region „USA, Osten 2“ verfügen, benötigen Sie eine Gatewayressource in der Region „USA, Osten 2“. Mehrere Server in der Region „USA, Osten 2“ können aber dasselbe Gateway verwenden.

Das erstmalige Einrichten des Gateways ist ein Prozess mit vier Schritten:

- **Herunterladen und Ausführen des Setupprogramms:** Bei diesem Schritt wird ein Gatewaydienst auf einem Computer in Ihrer Organisation installiert.

- **Registrieren des Gateways:** Bei diesem Schritt geben Sie einen Namen und einen Wiederherstellungsschlüssel für Ihr Gateway ein, wählen eine Region aus und registrieren Ihr Gateway beim Gateway-Clouddienst.

- **Erstellen einer Gatewayressource in Azure:** Bei diesem Schritt erstellen Sie eine Gatewayressource in Ihrem Azure-Abonnement.

- **Verbinden Ihrer Server mit der Gatewayressource:** Nachdem Sie in Ihrem Abonnement eine Gatewayressource eingerichtet haben, können Sie damit beginnen, Ihre Server mit dieser zu verbinden.

Nachdem Sie eine Gatewayressource für Ihr Abonnement konfiguriert haben, können Sie mehrere Server und andere Dienste mit dieser verbinden. Sie müssen nur dann ein weiteres Gateway installieren und zusätzliche Gatewayressourcen erstellen, wenn Sie über Server oder andere Dienste in einer anderen Region verfügen.

Wenn Sie sofort beginnen möchten, lesen Sie unter [Installieren und Konfigurieren eines lokalen Datengateways](analysis-services-gateway-install.md) nach.

## <a name="how-it-works"> </a>Funktionsweise
Das Gateway, das Sie auf einem Computer im Netzwerk Ihrer Organisation installieren, wird als Windows-Dienst mit dem Namen **Lokales Datengateway** ausgeführt. Dieser lokale Dienst wird über Azure Service Bus beim Gateway-Clouddienst registriert. Anschließend erstellen Sie eine Gatewayressource als Gateway-Clouddienst für Ihr Azure-Abonnement. Ihre Azure Analysis Services-Server werden dann mit der Gatewayressource verbunden. Wenn Modelle auf Ihrem Server für Abfragen oder die Verarbeitung mit Ihren lokalen Datenquellen verbunden werden müssen, durchläuft ein Abfrage- und Datenfluss die Gatewayressource, Azure Service Bus, den lokalen Datengateway-Dienst und Ihre Datenquellen. 

![So funktioniert's](./media/analysis-services-gateway/aas-gateway-how-it-works.png)

Abfragen und Datenfluss:

1. Vom Clouddienst wird eine Abfrage mit den verschlüsselten Anmeldeinformationen für die lokale Datenquelle erstellt. Sie wird dann an eine Warteschlange für die Verarbeitung durch das Gateway gesendet.
2. Der Gatewayclouddienst analysiert die Abfrage und überträgt die Anforderung per Push an den [Azure Service Bus](https://azure.microsoft.com/documentation/services/service-bus/).
3. Das lokale Datengateway fragt den Azure Service Bus nach ausstehenden Anforderungen ab.
4. Das Gateway ruft die Abfrage ab, entschlüsselt die Anmeldeinformationen und stellt anhand dieser Anmeldeinformationen eine Verbindung mit den Datenquellen her.
5. Das Gateway sendet die Abfrage zur Ausführung an die Datenquelle.
6. Die Ergebnisse werden aus der Datenquelle zurück an das Gateway und dann an den Clouddienst und Ihren Server gesendet.

## <a name="windows-service-account"> </a>Windows-Dienstkonto
Das lokale Datengateway ist zur Verwendung von *NT SERVICE\PBIEgwService* für die Anmeldeinformationen für den Windows-Dienst konfiguriert. Standardmäßig verfügt es über das Recht „Anmelden als Dienst“ im Kontext des Computers, auf dem Sie das Gateway installieren. Diese Anmeldeinformationen sind nicht mit denen des Kontos identisch, das zum Verbinden mit lokalen Datenquellen oder dem Azure-Konto verwendet wird.  

Wenn beim Proxyserver Authentifizierungsprobleme auftreten, sollten Sie das Windows-Dienstkonto in ein Domänenbenutzerkonto oder verwaltetes Dienstkonto ändern.

## <a name="ports"> </a>Ports
Das Gateway stellt eine ausgehende Verbindung mit dem Azure Service Bus her. Es kommuniziert über ausgehende Ports: TCP 443 (Standard), 5671, 5672, 9350 bis 9354.  Das Gateway benötigt keine eingehenden Ports.

Es wird empfohlen, die IP-Adressen für Ihren Datenbereich in die Whitelist der Firewall aufzunehmen. Sie können die [Liste der IP-Bereiche des Microsoft Azure-Rechenzentrums](https://www.microsoft.com/download/details.aspx?id=41653) (in englischer Sprache) herunterladen. Diese Liste wird wöchentlich aktualisiert.

> [!NOTE]
> Die IP-Adressen in der Liste der IP-Bereiche des Azure-Rechenzentrums sind in der CIDR-Notation angegeben. Beispielsweise bedeutet 10.0.0.0/24 nicht 10.0.0.0 bis 10.0.0.24. Informieren Sie sich über die [CIDR-Notation](http://whatismyipaddress.com/cidr).
>
>

Nachfolgend sind die vollqualifizierten Domänennamen aufgeführt, die vom Gateway verwendet werden.

| Domänennamen | Ausgehende Ports | Beschreibung |
| --- | --- | --- |
| *.powerbi.com |80 |Zum Herunterladen des Installers wird HTTP verwendet. |
| *.powerbi.com |443 |HTTPS |
| *. analysis.windows.net |443 |HTTPS |
| *.login.windows.net |443 |HTTPS |
| *.servicebus.windows.net |5671-5672 |Advanced Message Queuing Protocol (AMQP) |
| *.servicebus.windows.net |443, 9350-9354 |Listener auf Service Bus Relay per TCP (443 für Access Control-Tokenbeschaffung erforderlich) |
| *.frontend.clouddatahub.net |443 |HTTPS |
| *.core.windows.net |443 |HTTPS |
| login.microsoftonline.com |443 |HTTPS |
| *.msftncsi.com |443 |Wird verwendet, um die Internetkonnektivität zu testen, falls der Power BI-Dienst das Gateway nicht erreicht. |
| *.microsoftonline-p.com |443 |Wird je nach Konfiguration für die Authentifizierung verwendet. |

### <a name="force-https"></a>Erzwingen von HTTPS-Kommunikation mit Azure Service Bus
Sie können erzwingen, dass das Gateway mit Azure Service Bus über HTTPS (und nicht direkt über TCP) kommuniziert. Dies kann allerdings erheblich die Leistung beeinträchtigen. Sie können die Datei *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config* ändern, indem Sie den Wert von `AutoDetect` in `Https` ändern. Diese Datei befindet sich standardmäßig in *C:\Programme\On-premises data gateway*.

```
<setting name="ServiceBusSystemConnectivityModeString" serializeAs="String">
    <value>Https</value>
</setting>
```

## <a name="faq"></a>Häufig gestellte Fragen

### <a name="general"></a>Allgemein

**F**: Benötige ich ein Gateway für Datenquellen in der Cloud, z.B. SQL Azure? <br/>
**A**: Nein. Ein Gateway dient nur der Verbindung mit lokalen Datenquellen.

**F**: Muss das Gateway auf dem gleichen Computer wie die Datenquelle installiert werden? <br/>
**A**: Nein. Das Gateway stellt mithilfe der bereitgestellten Verbindungsinformationen eine Verbindung mit der Datenquelle her. Stellen Sie sich das Gateway in diesem Sinne als Clientanwendung vor. Für das Gateway ist nur die Funktion zum Herstellen der Verbindung mit dem angegebenen Servernamen erforderlich.

<a name="why-azure-work-school-account"></a>

**F**: Warum muss ich für die Anmeldung ein Geschäfts-, Schul- oder Unikonto verwenden? <br/>
**A**: Sie können beim Installieren des lokalen Datengateways nur ein Azure-Geschäfts-, Schul- oder Unikonto verwenden. Ihr Anmeldekonto ist in einem Mandanten gespeichert, der von Azure Active Directory (Azure AD) verwaltet wird. Normalerweise entspricht der Benutzerprinzipalname (UPN) Ihres Azure AD-Kontos der E-Mail-Adresse.

**F**: Wo werden meine Anmeldeinformationen gespeichert ? <br/>
**A**: Die Anmeldeinformationen, die Sie für eine Datenquelle eingeben, werden verschlüsselt und im Gateway-Clouddienst gespeichert. Die Anmeldeinformationen werden im lokalen Datengateway entschlüsselt.

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

### <a name="high-availability"></a>Hohe Verfügbarkeit und Notfallwiederherstellung

**F**: Welche Optionen sind für die Notfallwiederherstellung verfügbar? <br/>
**A**: Sie können den Wiederherstellungsschlüssel verwenden, um ein Gateway wiederherzustellen oder zu verschieben. Wenn Sie das Gateway installieren, geben Sie den Wiederherstellungsschlüssel an.

**F**: Welchen Vorteil hat der Wiederherstellungsschlüssel? <br/>
**A**: Der Wiederherstellungsschlüssel bietet eine Möglichkeit zum Migrieren oder Wiederherstellen Ihrer Gatewayeinstellungen nach einem Notfall.

## <a name="troubleshooting"> </a>Problembehandlung

**F**: Wie kann ich feststellen, welche Abfragen an die lokale Datenquelle gesendet werden? <br/>
**A**: Sie können die Abfrageablaufverfolgung aktivieren. Hierin sind die gesendeten Abfragen enthalten. Denken Sie daran, die Abfrageablaufverfolgung nach Abschluss der Problembehandlung wieder auf den ursprünglichen Wert zurückzusetzen. Wenn Sie die Abfrageablaufverfolgung aktiviert lassen, werden größere Protokolle erstellt.

Sie können auch Tools anzeigen, die Ihre Datenquelle für die Verfolgung von Abfrageabläufen bietet. Sie können z.B. Erweiterte Ereignisse oder SQL Profiler für SQL Server und Analysis Services verwenden.

**F**: Wo sind die Gatewayprotokolle? <br/>
**A**: Siehe „Tools“ weiter unten in diesem Thema.

### <a name="update"></a>Update auf die aktuelle Version

Es können vermehrt Probleme auftreten, wenn die Gatewayversion veraltet ist. Achten Sie daher am besten darauf, dass Sie immer die aktuelle Version verwenden. Wenn Sie das Gateway für einen Monat oder länger nicht aktualisiert haben, sollten Sie erwägen, die neueste Version des Gateways zu installieren und zu ermitteln, ob Sie das Problem reproduzieren können.

### <a name="error-failed-to-add-user-to-group--2147463168-pbiegwservice-performance-log-users"></a>Fehler: Fehler beim Hinzufügen des Benutzers zur Gruppe. (-2147463168 PBIEgwService Leistungsprotokollbenutzer)

Dieser Fehler wird ggf. angezeigt, wenn Sie versuchen, das Gateway auf einem Domänencontroller zu installieren, der nicht unterstützt wird. Stellen Sie sicher, dass Sie das Gateway auf einem Computer bereitstellen, beim dem es sich nicht um einen Domänencontroller handelt.

## <a name="logs"></a>Protokolle

Protokolldateien sind eine wichtige Ressource bei der Problembehandlung.

#### <a name="enterprise-gateway-service-logs"></a>Enterprise-Gatewaydienstprotokolle

`C:\Users\PBIEgwService\AppData\Local\Microsoft\On-premises data gateway\<yyyyymmdd>.<Number>.log`

#### <a name="configuration-logs"></a>Konfigurationsprotokolle

`C:\Users\<username>\AppData\Local\Microsoft\On-premises data gateway\GatewayConfigurator.log`




#### <a name="event-logs"></a>Ereignisprotokolle

Sie finden das Datenverwaltungsgateway- und PowerBIGateway-Protokoll unter **Anwendungs- und Dienstprotokolle**.


## <a name="telemetry"></a>Telemetrie
Telemetrie kann zur Überwachung und Problembehandlung verwendet werden. Standardeinstellung

**So aktivieren Sie Telemetrie**

1.  Überprüfen Sie das Verzeichnis des lokalen Datengateway-Clients auf dem Computer. Es lautet in der Regel **%systemdrive%\Programme\On-premises data gateway**. Alternativ können Sie die Konsole „Dienste“ öffnen und den Pfad zur ausführbaren Datei überprüfen: eine Eigenschaft des Diensts Lokales Datengateway.
2.  In der Datei „Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config“ im Clientverzeichnis. Ändern Sie die Einstellung „SendTelemetry“ in „true“.
        
    ```
        <setting name="SendTelemetry" serializeAs="String">
                    <value>true</value>
        </setting>
    ```

3.  Speichern Sie die Änderungen, und starten Sie den folgenden Windows-Dienst neu: Lokales Datengateway.




## <a name="next-steps"></a>Nächste Schritte
* [Verwalten von Analysis Services](analysis-services-manage.md)
* [Abrufen von Daten aus Azure Analysis Services](analysis-services-connect.md)

