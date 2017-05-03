---
title: Azure App Service-Hybridverbindungen | Microsoft-Dokumentation
description: "Gewusst wie: Erstellen und Verwenden von Hybridverbindungen für den Zugriff auf Ressourcen in unterschiedlichen Netzwerken"
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: 
ms.assetid: 66774bde-13f5-45d0-9a70-4e9536a4f619
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/22/2017
ms.author: ccompy
translationtype: Human Translation
ms.sourcegitcommit: b0c27ca561567ff002bbb864846b7a3ea95d7fa3
ms.openlocfilehash: fef9e7b280387934cb093f51b4c8aa134a3b87e7
ms.lasthandoff: 04/25/2017


---

# <a name="azure-app-service-hybrid-connections"></a>Azure App Service-Hybridverbindungen #

## <a name="overview"></a>Übersicht ##

Hybridverbindungen sind sowohl ein Dienst in Azure als auch eine Funktion in Azure App Service.  Als Dienst kann es über die Einsatzmöglichkeiten und Funktionen von Azure App Service hinaus genutzt werden.  Weitere Informationen über Hybridverbindungen und dessen Verwendung außerhalb von Azure App Service finden Sie hier: [Azure-Relayhybridverbindungen][HCService]

In Azure App Service kann mithilfe von Hybridverbindungen auf Anwendungsressourcen in anderen Netzwerken zugegriffen werden. Es ermöglicht den Zugriff VON Ihrer App AUF einen Anwendungsendpunkt.  Es bietet keine alternative Möglichkeit für den Zugriff auf Ihre Anwendung.  Bei der Verwendung in App Service entspricht jede Hybridverbindung einer Kombination aus einem einzelnen TCP-Host und einem Port.  Dies bedeutet, dass sich der Hybridverbindungsendpunkt in einem beliebigen Betriebssystem und einer beliebigen Anwendung befinden kann, vorausgesetzt, dass Sie einen TCP-Überwachungsport festlegen. Hybridverbindungen besitzen keine Informationen über das Anwendungsprotokoll oder den abzurufenden Inhalt oder benötigen diese Informationen.  Sie ermöglichen lediglich den Netzwerkzugriff.  


## <a name="how-it-works"></a>So funktioniert's ##
Die Hybridverbindungsfunktion besteht aus zwei ausgehenden Aufrufen an das Service Bus Relay.  Es besteht eine Verbindung mit der Bibliothek auf dem Host, auf dem Ihre App in App Service ausgeführt wird, sowie eine Verbindung zwischen dem Hybrid Connection Manager (HCM) und dem Service Bus Relay.  Der HCM ist ein Relaydienst, den Sie im Rahmen des Netzwerk-Hostings bereitstellen. 

Über die beiden verknüpften Verbindungen erstellt Ihre App einen TCP-Tunnel zu einer festen Host:Port-Kombination auf der anderen Seite des HCM.  Die Verbindung verwendet zum Schutz TLS 1.2 und zur Authentifizierung/Autorisierung SAS-Schlüssel.    

![][1]

Wenn Ihre App eine DNS-Anforderung stellt, die einem konfigurierten Hybridverbindungsendpunkt entspricht, wird der ausgehende TCP-Datenverkehr über die Hybridverbindung umgeleitet.  

> [!NOTE]
> Sie sollten folglich versuchen, stets einen DNS-Namen für Ihre Hybridverbindung zu verwenden.  Einige Clientsoftware führen keine DNS-Suche durch, wenn der Endpunkt stattdessen eine IP-Adresse verwendet.
>
>

Es gibt zwei Arten von Hybridverbindungen: die neuen Hybridverbindungen, die als Dienst im Rahmen von Azure Relay angeboten werden, und die älteren Hybridverbindungen von BizTalk.  Die älteren BizTalk-Hybridverbindungen werden als klassische Hybridverbindungen im Portal bezeichnet.  Weiter unten in diesem Dokument finden Sie weitere Informationen hierzu.

### <a name="app-service-hybrid-connection-benefits"></a>Vorteile der App Service-Hybridverbindungen ###

Die Hybridverbindungsfunktion bietet eine Reihe von Vorteilen wie etwa Folgende:

- Sie ermöglicht den sicheren Zugriff auf Apps auf lokalen Systemen und in lokalen Diensten.
- Für die Funktion ist kein Endpunkt erforderlich, der über das Internet zugänglich ist.
- Sie lässt sich schnell und einfach einrichten.  
- Jede Hybridverbindung entspricht einer einzelnen Host:Port-Kombination, was auch ein hervorragender Sicherheitsvorteil darstellt.
- Es sind normalerweise keine Firewallöffnungen erforderlich, da alle Verbindungen über die Standard-Webports ausgehen.
- Da die Funktion auf Netzwerkebene ausgeführt wird, bedeutet dies auch, dass sie nicht von der Sprache, die von Ihrer App verwendet wird, und von der Technologie, die vom Endpunkt verwendet wird, abhängig ist.
- Sie kann verwendet werden, um über eine einzige App Zugriff in mehreren Netzwerken bereitzustellen. 

### <a name="things-you-cannot-do-with-hybrid-connections"></a>Einschränkungen bei Hybridverbindungen ###

Bei Hybridverbindungen sind bestimmte Vorgänge und Funktionen nicht möglich. Hierzu zählt beispielsweise Folgendes:

- Bereitstellung eines Laufwerks
- Verwendung von UDP
- Zugriff auf TCP-basierte Dienste, die dynamische Ports wie der passive Modus für FTP oder der erweiterte passive Modus
- Unterstützung für LDAP, da gelegentlich UDP erforderlich ist
- Unterstützung für Active Directory

## <a name="adding-and-creating-a-hybrid-connection-in-your-app"></a>Hinzufügen und Erstellen einer Hybridverbindung in Ihrer App ##

Hybridverbindungen können über das App-Portal oder über das Hybridverbindungs-Dienstportal erstellt werden.  Es wird dringend empfohlen, die Hybridverbindungen, die für die Verwendung mit Ihrer App vorgesehen sind, über das App-Portal zu erstellen.  Navigieren Sie zum Erstellen einer Hybridverbindung zum [Azure-Portal][portal] und rufen Sie die Benutzeroberfläche für Ihre App auf.  Wählen Sie **Netzwerk > Endpunkte der Hybridverbindung konfigurieren**.  In dieser Ansicht können Sie die Hybridverbindungen sehen, die bei Ihrer App konfiguriert sind.  

![][2]

Um eine neue Hybridverbindung hinzuzufügen, klicken Sie auf „Hybridverbindung hinzufügen“.  Auf der daraufhin geöffneten Benutzeroberfläche werden die Hybridverbindungen aufgeführt, die Sie bereits erstellt haben.  Um eine oder mehrere dieser Hybridverbindungen zu Ihrer App hinzuzufügen, klicken Sie auf diese und anschließend auf **Ausgewählte Hybridverbindung hinzufügen**.  

![][3]

Wenn Sie eine neue Hybridverbindung erstellen möchten, klicken Sie auf **Neue Hybridverbindung erstellen**.  In dieser Ansicht können Sie Folgendes festlegen: 

- Endpunktname
- Endpunkthostname
- Endpunktport
- Der zu verwendende Service Bus-Namespace

![][4]

Jede Hybridverbindung ist mit einem Service Bus-Namespace verbunden. Jeder Service Bus-Namespace befindet sich in einer Azure-Region.  Es ist wichtig, einen Service Bus-Namespace in derselben Region wie Ihre App zu verwenden, um durch das Netzwerk verursachte Latenzen zu vermeiden.

Wenn Sie Ihre Hybridverbindung aus Ihrer App entfernen möchten, klicken Sie mit der rechten Maustaste darauf und wählen Sie **Verbindung trennen**.  

Sobald eine Hybridverbindung zu Ihrer Web-App hinzugefügt wurde, können Sie die Details hierzu anzeigen, indem Sie einfach darauf klicken.  

![][5]

## <a name="hybrid-connections-and-app-service-plans"></a>Hybridverbindungen und App Service-Pläne ##

Die einzigen Hybridverbindungen, die Sie nun erstellen können, sind neue Hybridverbindungen.  Sie sind nur in den Preis-SKUs Basic, Standard, Premium und Isolated verfügbar.  Es gelten Beschränkungen, die am Tarif geknüpft sind.  

| Tarif | Anzahl der im Plan verwendbaren Hybridverbindungen |
|----|----|
| Basic | 5 |
| Standard | 25 |
| Premium | 200 |
| Isolated | 200 |

Da es sich um Einschränkungen des App Service-Plans handelt, ist auch eine Benutzeroberfläche im App Service-Plan vorhanden, auf der angezeigt wird, wie viele Hybridverbindungen und von welchen Apps diese derzeit verwendet werden.  

![][6]

Ebenso wie bei der App-Ansicht können Sie Details zu Ihrer Hybridverbindung sehen, indem Sie darauf klicken.  In den Eigenschaften, die hier gezeigt werden, können Sie alle Informationen sehen, die in der App-Ansicht verfügbar waren, jedoch auch wie viele andere Apps im selben App Service-Plan diese Hybridverbindung verwenden.

![][7]

Es gibt zwar einen Grenzwert für die Anzahl der Hybridverbindungsendpunkte, die in einem App Service-Plan verwendet werden können, jedoch kann jede verwendete Hybridverbindung über eine beliebige Anzahl von Apps hinweg in diesem App Service-Plan verwendet werden.  Das heißt, wenn eine Hybridverbindung in fünf verschiedenen Apps in einem App Service-Plan verwendet werden würde, gäbe es dennoch nur eine Hybridverbindung.

Es gibt zusätzliche Kosten für Hybridverbindungen, wenn Sie über ihre vorgesehene Verwendung in den SKUs Basic, Standard, Premium oder Isolated verwendet werden.  Ausführliche Informationen zu den Preisen von Hybridverbindungen finden Sie unter: [Service Bus-Preise][sbpricing].

## <a name="hybrid-connection-manager"></a>Hybrid Connection Manager ##

Damit Hybridverbindungen funktionieren, ist ein Relay-Agent im Netzwerk erforderlich, der Ihren Hybridverbindungsendpunkt hostet.  Dieser Relay-Agent wird als Hybrid Connection Manager (HCM) bezeichnet.  Dieses Tool kann über die Benutzeroberfläche unter **Netzwerk > Endpunkte der Hybridverbindung konfigurieren** in Ihrer App im [Azure-Portal][portal] heruntergeladen werden.  

Dieses Tool wird auf Windows Server 2008 R2 und höheren Windows-Versionen ausgeführt.  Nach der Installation wird der HCM als Dienst ausgeführt.  Dieser Dienst stellt basierend auf den konfigurierten Endpunkten eine Verbindung zum Azure Service Bus Relay her.  Bei den Verbindungen vom HCM handelt es sich um ausgehende Verbindungen zu den Ports 80 und 443.    

Der HCM verfügt über eine Benutzeroberfläche für die Konfiguration.  Nach der Installation des HCM können Sie die Benutzeroberfläche aufrufen, indem Sie HybridConnectionManagerUi.exe im Installationsverzeichnis des Hybrid Connection Manager ausführen.  Er kann auch mühelos auf Windows 10 erreicht werden, indem Sie *Hybrid Connection Manager UI* in Ihr Suchfeld eingeben.  

Wenn die Benutzeroberfläche des HCM gestartet wird, wird als Erstes eine Tabelle angezeigt, in der alle Hybridverbindungen, die mit dieser Instanz des HCM konfiguriert sind, aufgeführt werden.  Wenn Sie Änderungen vornehmen möchten, müssen Sie sich mit Azure authentifizieren. 

So fügen Sie Ihrem HCM eine oder mehrere Hybridverbindungen hinzu:

1. Starten der Benutzeroberfläche des HCM
1. Klicken Sie auf „Andere Hybridverbindung konfigurieren“. ![][8]

1. Melden Sie sich mit Ihrem Azure-Konto an.
1. Wählen Sie ein Abonnement.
1. Klicken Sie auf die Hybridverbindungen, die dieser HCM weiterleiten soll. ![][9]

1. Klicken Sie auf Speichern.

An diesem Punkt werden die Hybridverbindungen angezeigt, die Sie hinzugefügt haben.  Sie können auch auf die konfigurierte Hybridverbindung klicken und ausführliche Informationen zu der Verbindung sehen.

![][10]

Damit der HCM die bei ihm konfigurierten Hybridverbindungen unterstützen kann, ist Folgendes erforderlich:

- TCP-Zugriff auf Azure über die Ports 80 und 443
- TCP-Zugriff auf den Hybridverbindungsendpunkt
- Möglichkeit für DNS-Suchvorgänge nach dem Endpunkthost und dem Azure Service Bus-Namespace

Der HCM unterstützt sowohl neue Hybridverbindungen als auch die älteren BizTalk-Hybridverbindungen.

### <a name="redundancy"></a>Redundanz ###

Jeder HCM kann mehrere Hybridverbindungen unterstützen.  Darüber hinaus kann eine bestimmte Hybridverbindung durch mehrere HCMs unterstützt werden.  Das Standardverhalten ist ein Roundrobin-Datenverkehr über die konfigurierten HCMs für einen bestimmten Endpunkt.  Wenn Sie eine hohe Verfügbarkeit für Ihre Hybridverbindungen über Ihr Netzwerk wünschen, instanziieren Sie einfach mehrere HCMs auf unterschiedlichen Computern. 

### <a name="manually-adding-a-hybrid-connection"></a>Manuelles Hinzufügen einer Hybridverbindung ###

Wenn ein Benutzer außerhalb Ihres Abonnements eine HCM-Instanz für eine bestimmte Hybridverbindung hosten soll, können Sie für diesen die Gatewayverbindungszeichenfolge für die Hybridverbindung freigeben.  Diese können Sie in den Eigenschaften einer Hybridverbindung im [Azure-Portal][portal] sehen. Um diese Zeichenfolge zu verwenden, klicken Sie im HCM auf die Schaltfläche **Manuell konfigurieren** und fügen Sie die Gatewayverbindungszeichenfolge ein.


## <a name="troubleshooting"></a>Problembehandlung ##

Wenn Ihre Hybridverbindung mit einer ausgeführten Anwendung eingerichtet ist und mindestens ein HCM vorhanden ist, der mit dieser Hybridverbindung konfiguriert wurde, lautet der Status im Portal **Verbunden**.  Wenn der Status nicht **Verbunden** lautet, bedeutet dies, dass Ihre App ausgefallen ist oder Ihr HCM über die Ports 80 oder 443 keine Verbindung zu Azure herstellen kann.  

Der Hauptgrund, warum Clients keine Verbindung zum jeweiligen Endpunkt herstellen können, ist, dass der Endpunkt mit einer IP-Adresse anstelle eines DNS-Namens angegeben wurde.  Wenn Ihre App den gewünschten Endpunkt nicht erreichen kann und Sie eine IP-Adresse verwendet haben, gehen Sie zur Verwendung eines DNS-Namens über, der auf dem Host, auf dem der HCM ausgeführt wird, gültig ist.  Darüber hinaus sollten Sie prüfen, ob der DNS-Name auf dem Host, auf dem der HCM ausgeführt wird, ordnungsgemäß aufgelöst wird, und eine Verbindung zwischen dem Host, auf dem der HCM ausgeführt wird, und dem Hybridverbindungsendpunkt besteht.  

In App Service ist ein Tool vorhanden, das über die Konsole „tcpping“ aufgerufen werden kann.  Mit diesem Tool können Sie feststellen, ob Sie Zugriff auf einen TCP-Endpunkt haben, jedoch nicht, ob Sie Zugriff auf einen Hybridverbindungsendpunkt haben.  Bei Verwendung in der Konsole für einen Hybridverbindungsendpunkt stellt ein erfolgreicher Ping lediglich fest, ob eine Hybridverbindung für Ihre App, die diese Host:Port-Kombination verwendet, konfiguriert ist.  

## <a name="biztalk-hybrid-connections"></a>BizTalk-Hybridverbindungen ##

Die ältere Funktion der BizTalk-Hybridverbindungen wurde für weitere Erstellungen von BizTalk-Hybridverbindungen gesperrt.  Sie können weiterhin Ihre bereits vorhandenen BizTalk-Hybridverbindungen mit Ihren Apps verwenden, sollten jedoch eine Migration auf den neuen Dienst durchführen.  Der neue Dienst bringt gegenüber der BizTalk-Version folgende Vorteile mit sich:

- Kein zusätzliches BizTalk-Konto erforderlich
- TLS 1.2 statt 1.0 wie bei BizTalk-Hybridverbindungen
- Kommunikation über die Ports 80 und 443, um Azure mithilfe eines DNS-Namens anstelle von IP-Adressen und einer Reihe von zusätzlichen anderen Ports zu erreichen  

Um Ihrer App eine BizTalk-Hybridverbindung hinzuzufügen, navigieren Sie im [Azure-Portal][portal] zu Ihrer App und klicken Sie auf **Netzwerk > Endpunkte der Hybridverbindung konfigurieren**.  Klicken Sie in der Tabelle „Klassische Hybridverbindungen“ auf **Klassische Hybridverbindung hinzufügen**.  In dieser Ansicht sehen Sie eine Liste Ihrer BizTalk-Hybridverbindungen.  


<!--Image references-->
[1]: ./media/app-service-hybrid-connections/hybridconn-connectiondiagram.png
[2]: ./media/app-service-hybrid-connections/hybridconn-portal.png
[3]: ./media/app-service-hybrid-connections/hybridconn-addhc.png
[4]: ./media/app-service-hybrid-connections/hybridconn-createhc.png
[5]: ./media/app-service-hybrid-connections/hybridconn-properties.png
[6]: ./media/app-service-hybrid-connections/hybridconn-aspproperties.png
[7]: ./media/app-service-hybrid-connections/hybridconn-hcm.png
[8]: ./media/app-service-hybrid-connections/hybridconn-hcmadd.png
[9]: ./media/app-service-hybrid-connections/hybridconn-hcmadded.png
[10]: ./media/app-service-hybrid-connections/hybridconn-hcmdetails.png

<!--Links-->
[HCService]: http://docs.microsoft.com/azure/service-bus-relay/relay-hybrid-connections-protocol/
[portal]: http://portal.azure.com/
[oldhc]: http://docs.microsoft.com/azure/biztalk-services/integration-hybrid-connection-overview/
[sbpricing]: http://azure.microsoft.com/pricing/details/service-bus/


