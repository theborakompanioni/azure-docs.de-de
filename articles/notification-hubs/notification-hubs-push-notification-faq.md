---
title: "Azure Notification Hubs: Häufig gestellte Fragen (FAQs) | Microsoft-Dokumentation"
description: "Häufig gestellte Fragen zum Entwerfen/Implementieren von Lösungen in Notification Hubs"
services: notification-hubs
documentationcenter: mobile
author: ysxu
manager: erikre
keywords: Pushbenachrichtigung, Pushbenachrichtigungen, iOS-Pushbenachrichtigungen, Android-Pushbenachrichtigungen, iOS-Push, Android-Push
editor: 
ms.assetid: 7b385713-ef3b-4f01-8b1f-ffe3690bbd40
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 01/19/2017
ms.author: yuaxu
ms.translationtype: Human Translation
ms.sourcegitcommit: 532ff423ff53567b6ce40c0ea7ec09a689cee1e7
ms.openlocfilehash: c8592ea38d62213327549583a1e931ea682ee56e
ms.contentlocale: de-de
ms.lasthandoff: 06/05/2017


---
# <a name="push-notifications-with-azure-notification-hubs-frequently-asked-questions"></a>Pushbenachrichtigungen mit Azure Notification Hubs: Häufig gestellte Fragen
## <a name="general"></a>Allgemein
### <a name="what-is-the-resource-structure-of-notification-hubs"></a>Wie ist die Ressourcenstruktur von Notification Hubs aufgebaut?

Azure Notification Hubs umfasst zwei Ressourcenebenen: Hubs und Namespaces. Ein Hub ist eine zentrale Ressource für Pushbenachrichtigungen, die plattformübergreifende Push-Informationen einer App enthalten kann. Ein Namespace ist eine Sammlung von Hubs in einem Bereich.

Bei der empfohlenen Zuordnung wird ein einzelner Namespace mit einer einzelnen App verknüpft. Ein Namespace kann einen Produktionshub enthalten, der mit Ihrer Produktions-App zusammenarbeitet, einen Test-Hub, der mit Ihrer Test-App zusammenarbeitet, usw.

### <a name="what-is-the-price-model-for-notification-hubs"></a>Welches Preismodell gilt für Notification Hubs?
Aktuelle Preisinformationen finden Sie auf der Seite [Notification Hubs – Preise]. Die Abrechnung für Notification Hubs erfolgt auf Namespaceebene. (Die Definition des Begriffs „Namespace“ finden Sie unter „Wie ist die Ressourcenstruktur von Notification Hubs aufgebaut?“.) Für Notification Hubs stehen drei Tarife zur Verfügung:

* **Free:** Dieser Einstiegstarif eignet sich sehr gut, um sich mit Pushfunktionen vertraut zu machen. Für Produktions-Apps ist er nicht zu empfehlen. Der Tarif beinhaltet 500 Geräte und eine Million Pushbenachrichtigungen pro Namespace und Monat. Eine SLA-Garantie (Service Level Agreement, Vereinbarung zum Servicelevel) ist nicht enthalten.
* **Basic:** Dieser Tarif (oder der Standard-Tarif) wird für kleinere Produktions-Apps empfohlen. Er beinhaltet 200.000 Geräte und zehn Millionen Pushbenachrichtigungen pro Namespace und Monat als Basis. Außerdem stehen Optionen zur Erhöhung des Kontingents zur Verfügung.
* **Standard:** Dieser Tarif wird für mittelgroße bis große Produktions-Apps empfohlen. Er beinhaltet zehn Millionen Geräte und zehn Millionen Pushbenachrichtigungen pro Namespace und Monat als Basis. Außerdem stehen Optionen zur Erhöhung des Kontingents sowie umfangreiche Telemetriefunktionen zur Verfügung.

Features des Standard-Tarifs:
* **Umfangreiche Telemetrie:** Mit der Telemetrie auf Nachrichtenbasis von Notification Hubs können Sie Pushanforderungen und Feedback des Plattformbenachrichtigungssystems zu Debuggingzwecken nachverfolgen.
* **Mehrmandantenfähigkeit:** Sie können Anmeldeinformationen des Plattformbenachrichtigungssystems auf Namespaceebene verwenden. Dadurch können Sie Mandanten innerhalb eines Namespaces problemlos in Hubs unterteilen.
* **Geplante Pushbenachrichtigungen:** Sie können das Senden von Benachrichtigungen für einen beliebigen Zeitpunkt planen.

### <a name="what-is-the-notification-hubs-sla"></a>Was ist die Notification Hubs-SLA?
Bei den Notification Hubs-Tarifen „Basic“ und „Standard“ können ordnungsgemäß konfigurierte Anwendungen in mindestens 99,9 Prozent der Fälle Pushbenachrichtigungen senden oder Registrierungsverwaltungsvorgänge ausführen. Weitere Informationen zur Vereinbarung zum Servicelevel finden Sie auf der Seite [SLA für Notification Hubs](https://azure.microsoft.com/support/legal/sla/notification-hubs/).

> [!NOTE]
> Da Pushbenachrichtigungen auf Plattformbenachrichtigungssysteme von Drittanbietern (etwa APNS von Apple oder FCM von Google) angewiesen sind, wird die Übermittlung derartiger Nachrichten nicht per SLA garantiert. Nachdem Notification Hubs die Batches an Plattformbenachrichtigungssysteme gesendet hat (was per SLA garantiert wird), sind die Plattformbenachrichtigungssysteme für die Zustellung der Pushbenachrichtigungen zuständig (nicht per SLA garantiert).

### <a name="which-customers-are-using-notification-hubs"></a>Welche Kunden verwenden Notification Hubs?
Notification Hubs wird von vielen Kunden verwendet. Im Anschluss finden Sie ein paar namhafte Bespiele:

* Sochi 2014: Hunderte Interessengruppen, über drei Millionen Geräte und über 150 Millionen verschickte Benachrichtigungen in zwei Wochen. [Fallstudie: Sochi]
* Skanska: [Fallstudie: Skanska]
* Seattle Times: [Fallstudie: Seattle Times]
* Mural.ly: [Fallstudie: Mural.ly]
* 7Digital: [Fallstudie: 7Digital]
* Bing Apps: Mehrere Millionen Geräte senden täglich drei Millionen Benachrichtigungen.

### <a name="how-do-i-upgrade-or-downgrade-my-hub-or-namespace-to-a-different-tier"></a>Wie stufe ich meinen Hub oder meinen Namespace auf einen anderen Tarif hoch oder herab?
Navigieren Sie zu **[Azure-Portal]** > **Notification Hubs-Namespaces** oder **Notification Hubs**. Wählen Sie die Ressource aus, die Sie aktualisieren möchten, und navigieren Sie zu **Tarif**. Beachten Sie folgende Punkte:

* Der aktualisierte Tarif gilt für *alle* Hubs in dem Namespace, mit dem Sie arbeiten.
* Falls die Anzahl von Geräten den Grenzwert des niedrigeren Tarifs übersteigt, zu dem Sie wechseln möchten, müssen Sie zuerst Geräte löschen.


## <a name="design-and-development"></a>Entwurf und Entwicklung
### <a name="which-server-side-platforms-do-you-support"></a>Welche dienstseitigen Plattformen werden unterstützt?
Server-SDKs sind für .NET, Java, Node.js, PHP und Python verfügbar. Die APIs von Notification Hubs basieren auf REST-Schnittstellen, sodass Sie direkt mit REST-APIs arbeiten können, wenn Sie andere Plattformen verwenden oder weitere Abhängigkeiten vermeiden möchten. Weitere Informationen finden Sie auf der [Seite zu REST-APIs von Notification Hubs].

### <a name="which-client-platforms-do-you-support"></a>Welche Clientplattformen werden unterstützt?
Pushbenachrichtigungen werden für [iOS](notification-hubs-ios-apple-push-notification-apns-get-started.md), [Android](notification-hubs-android-push-notification-google-gcm-get-started.md), [Windows Universal](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md), [Windows Phone](notification-hubs-windows-mobile-push-notifications-mpns.md), [Kindle](notification-hubs-kindle-amazon-adm-push-notification.md), [Android China (via Baidu)](notification-hubs-baidu-china-android-notifications-get-started.md), Xamarin ([iOS](xamarin-notification-hubs-ios-push-notification-apns-get-started.md) und [Android](xamarin-notification-hubs-push-notifications-android-gcm.md)), [Chrome Apps](notification-hubs-chrome-push-notifications-get-started.md) und [Safari](https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSafari) unterstützt. Weitere Informationen finden Sie auf der [Seite mit Einführungstutorials für Notification Hubs].

### <a name="do-you-support-text-message-email-or-web-notifications"></a>Werden SMS-, E-Mail- oder Webbenachrichtigungen unterstützt?
Notification Hubs wurde primär zum Senden von Benachrichtigungen an mobile Apps entwickelt. E-Mail- oder SMS-Funktionen stehen nicht zur Verfügung. Sie können allerdings Drittanbieterplattformen mit diesen Funktionen in Notification Hubs integrieren, um native Pushbenachrichtigungen unter Verwendung von [Mobile Apps] zu senden.

Notification Hubs bietet auch keinen vorgefertigten Zustelldienst für Browserpushbenachrichtigungen. Kunden können dieses Feature implementieren, indem sie zusätzlich zu den unterstützten serverseitigen Plattformen SignalR verwenden. Wenn Sie Benachrichtigungen an Browser-Apps in der Chrome Sandbox senden möchten, sehen Sie sich das Tutorial [Erste Schritte mit Notification Hubs für Chrome-Apps] an.

### <a name="how-are-mobile-apps-and-azure-notification-hubs-related-and-when-do-i-use-them"></a>Wie hängen Mobile Apps und Azure Notification Hubs zusammen, und wann verwende ich sie?
Wenn Sie über ein vorhandenes mobiles App-Back-End verfügen und nur die Funktion zum Senden von Pushbenachrichtigungen hinzufügen möchten, können Sie Azure Notification Hubs verwenden. Wenn Sie Ihr mobiles App-Back-End ganz neu einrichten möchten, empfiehlt sich unter Umständen die Verwendung des Mobile Apps-Features von Azure App Service. Eine mobile App stellt automatisch einen Notification Hub bereit, damit Sie problemlos Pushbenachrichtigungen über das mobile App-Back-End senden können. Die Preise für Mobile Apps beinhalten die Grundgebühr für einen Notification Hub. Sie bezahlen nur, wenn Sie die im Preis enthaltenen Pushbenachrichtigungen überschreiten. Ausführlichere Informationen zu den Kosten finden Sie auf der Seite [App Service – Preise].

### <a name="how-many-devices-can-i-support-if-i-send-push-notifications-via-notification-hubs"></a>Wie viele Geräte kann ich unterstützen, wenn ich Pushbenachrichtigungen über Notification Hubs sende?
Auf der Seite [Notification Hubs – Preise] finden Sie ausführliche Informationen zur Anzahl unterstützter Geräte.

Sollten Sie mehr als zehn Millionen registrierte Geräte unterstützen müssen, nehmen Sie mit uns [Kontakt](https://azure.microsoft.com/overview/contact-us/) auf, damit wir Ihnen beim Skalieren Ihrer Lösung helfen können.

### <a name="how-many-push-notifications-can-i-send-out"></a>Wie viele Pushbenachrichtigungen kann ich senden?
Je nach gewähltem Tarif erfolgt die Skalierung von Azure Notification Hubs automatisch auf der Grundlage der Anzahl von Benachrichtigungen, die das System durchlaufen.

> [!NOTE]
> Die Gesamtkosten für die Nutzung können sich abhängig von der Anzahl verarbeiteter Pushbenachrichtigungen erhöhen. Machen Sie sich daher auf der Seite [Notification Hubs – Preise] mit den Grenzwerten der einzelnen Tarife vertraut.
> 
> 

Unsere Kunden senden mit Notification Hubs täglich mehrere Millionen von Pushbenachrichtigungen. Solange Sie Azure Notification Hubs verwenden, müssen Sie sich nicht extra um die Skalierung der Reichweite Ihrer Pushbenachrichtigungen kümmern.

### <a name="how-long-does-it-take-for-sent-push-notifications-to-reach-my-device"></a>Wie lange dauert das Senden der Pushbenachrichtigungen auf mein Gerät?
In einem normalen Nutzungsszenario mit konstanter Eingangslast ohne nennenswerte Spitzen kann Azure Notification Hubs *pro Minute mindestens eine Million Sendevorgänge für Pushbenachrichtigungen* verarbeiten. Diese Rate kann abhängig von der Anzahl von Tags, der Art der eingehenden Sendevorgänge und anderen externen Faktoren variieren.

Während der geschätzten Zustellzeit berechnet der Dienst die Ziele pro Plattform und leitet Nachrichten auf der Grundlage der registrierten Tags oder Tagausdrücke an den entsprechenden Pushbenachrichtigungsdienst (Push Notification Service, PNS) weiter. Der PNS hat die Aufgabe, die Benachrichtigungen an das Gerät zu senden.

Die Zustellung von Benachrichtigungen durch den PNS wird nicht per SLA garantiert. Die meisten Pushbenachrichtigungen werden allerdings nach dem Senden an Notification Hubs innerhalb weniger Minuten (in der Regel innerhalb von zehn Minuten) an die Zielgeräte übermittelt. Bei einigen Benachrichtigungen dauert es unter Umständen etwas länger.

> [!NOTE]
> Azure Notification Hubs verfügt über eine Richtlinie, nach der Pushbenachrichtigungen verworfen werden, wenn sie nicht innerhalb von 30 Minuten an den PNS übermittelt werden können. Diese Verzögerung kann aus verschiedenen Gründen auftreten. Meistens ist sie jedoch darauf zurückzuführen, dass der PNS Ihre Anwendung drosselt.
> 
> 

### <a name="is-there-any-latency-guarantee"></a>Gibt es garantierte Latenzzeiten?
Da Pushbenachrichtigungen von einem externen, plattformspezifischen PNS übermittelt werden, können keine Latenzzeiten garantiert werden. Die Mehrzahl der Pushbenachrichtigungen wird jedoch innerhalb weniger Minuten zugestellt.

### <a name="what-do-i-need-to-consider-when-designing-a-solution-with-namespaces-and-notification-hubs"></a>Welche Aspekte müssen beim Entwerfen einer Lösung mit Namespaces und Notification Hubs berücksichtigt werden?
#### <a name="mobile-appenvironment"></a>Mobile App/Umgebung
* Verwenden Sie pro mobiler App und Umgebung jeweils einen Notification Hub.
* In einem Szenario mit mehreren Mandanten sollte jeder Mandant über einen separaten Hub verfügen.
* Verwenden Sie einen Notification Hub niemals gleichzeitig für Produktions- und Testumgebungen. Das kann beim Senden von Benachrichtigungen zu Problemen führen. (Apple bietet Pushendpunkte für Sandbox und Produktion an, die jeweils über separate Anmeldeinformationen verfügen.)
* Standardmäßig können Sie über das Azure-Portal oder über die integrierte Azure-Komponente in Visual Studio Testbenachrichtigungen an registrierte Geräte senden. Der Schwellenwert wird auf 10 Geräte festgelegt, die nach dem Zufallsprinzip aus dem Registrierungspool ausgewählt werden.

> [!NOTE]
> Wenn Ihr Hub ursprünglich mit einem Apple-Sandbox-Zertifikat konfiguriert war und später mit einem Apple-Produktionszertifikat konfiguriert wurde, sind die ursprünglichen Gerätetoken ungültig. Ungültige Token können bei Pushbenachrichtigungen zu Fehlern führen. Trennen Sie Ihre Produktions- und Testumgebungen, und verwenden Sie unterschiedliche Hubs für unterschiedliche Umgebungen.
> 
> 

#### <a name="pns-credentials"></a>PNS-Anmeldeinformationen
Wenn eine mobile App beim Entwicklerportal einer Plattform (etwa Apple oder Google) registriert wird, werden eine App-ID und Sicherheitstoken gesendet. Das App-Back-End stellt diese Token für den PNS der Plattform bereit, damit Pushbenachrichtigungen an Geräte gesendet werden können. Sicherheitstoken können als Zertifikate (beispielsweise für Apple iOS oder Windows Phone) oder als Sicherheitsschlüssel (beispielsweise für Google Android oder Windows) vorliegen. Sie müssen in Notification Hubs konfiguriert werden. Die Konfiguration erfolgt üblicherweise auf Notification Hub-Ebene, kann in einem Szenario mit mehreren Mandanten jedoch auch auf Namespaceebene durchgeführt werden.

#### <a name="namespaces"></a>Namespaces
Namespaces können für eine Bereitstellungsgruppierung eingesetzt werden. In einem Szenario mit mehreren Mandanten können sie auch alle Notification Hubs für alle Mandanten der gleichen App darstellen.

#### <a name="geo-distribution"></a>Geografische Verteilung
Die geografische Verteilung ist bei Szenarios mit Pushbenachrichtigungen nicht immer ein wichtiger Aspekt. Verschiedene Pushbenachrichtigungsdienste (etwa APNS oder GCM), die Pushbenachrichtigungen an Geräte übermitteln, sind nicht gleichmäßig verteilt.

Wenn Sie über eine weltweit verwendete Anwendung verfügen, können Sie den Notification Hubs-Dienst in unterschiedlichen Azure-Regionen auf der ganzen Welt verwenden und so Hubs in unterschiedlichen Namespaces erstellen.

> [!NOTE]
> Dieser Ansatz wird jedoch nicht empfohlen, da er mit höheren Verwaltungskosten (insbesondere für Registrierungen) verbunden ist. Er sollte daher nur in Ausnahmefällen verwendet werden.
> 
> 

### <a name="should-i-do-registrations-from-the-app-back-end-or-directly-through-client-devices"></a>Soll ich Registrierungen über das App-Back-End oder direkt über Clientgeräte ausführen?
Registrierungen über das App-Back-End sind hilfreich, wenn Sie Clients vor der Registrierung authentifizieren müssen. Sie sind auch hilfreich, wenn Sie über Tags verfügen, die vom App-Back-End auf der Grundlage der App-Logik erstellt oder geändert werden müssen. Weitere Informationen finden Sie im [Leitfaden zur Back-End-Registrierung] und im [Leitfaden zur Back-End-Registrierung 2].

### <a name="what-is-the-push-notification-delivery-security-model"></a>Welches Sicherheitsmodell gilt für die Übermittlung von Pushbenachrichtigungen?
Azure Notification Hubs verwendet ein auf [Shared Access Signatures (SAS)](../storage/storage-dotnet-shared-access-signature-part-1.md) basierendes Sicherheitsmodell. Sie können die SAS-Token auf der Namespace-Stammebene oder auf der granularen Notification Hub-Ebene verwenden. Für SAS-Token können verschiedene Autorisierungsregeln festgelegt werden, um beispielsweise Nachrichtenberechtigungen zu senden oder auf Benachrichtigungsberechtigungen zu lauschen. Weitere Informationen finden Sie im [Dokument zum Sicherheitsmodell von Notification Hubs].

### <a name="how-should-i-handle-sensitive-payload-in-push-notifications"></a>Wie sollte ich vertrauliche Nutzlasten in Pushbenachrichtigungen behandeln?
Alle Benachrichtigungen werden über den PNS der Plattform an die Zielgeräte übermittelt. Wenn eine Benachrichtigung an Azure Notification Hubs gesendet wird, wird sie verarbeitet und an den jeweiligen PNS übergeben.

Bei allen Verbindungen zwischen Absender, Azure Notification Hubs und PNS wird HTTPS verwendet.

> [!NOTE]
> Die Nutzlast der Nachrichten wird von Azure Notification Hubs in keiner Weise protokolliert.
> 
> 

Für den Versand vertraulicher Nutzlasten empfehlen wir die Verwendung sicherer Pushbenachrichtigungen. Der Absender übermittelt eine Pingbenachrichtigung mit einer Nachrichten-ID und ohne die vertrauliche Nutzlast an das Gerät. Wenn die App auf dem Gerät die Nutzlast empfängt, ruft sie direkt eine sichere API auf, um die Nachrichtendetails abzurufen. Eine Implementierungsanleitung für dieses Muster finden Sie im [Tutorial zu sicheren Pushbenachrichtigungen mit Notification Hubs].

## <a name="operations"></a>Vorgänge
### <a name="what-support-is-provided-for-disaster-recovery"></a>Welche Unterstützung steht für die Notfallwiederherstellung zur Verfügung?
Wir bieten eine Notfallwiederherstellung der Metadaten (Notification Hubs-Name, Verbindungszeichenfolge und andere wichtige Informationen). Wenn eine Notfallwiederherstellung ausgelöst wird, gehen *nur* die Registrierungsdaten der Notification Hubs-Infrastruktur verloren. Sie benötigen eine Lösung, die Ihren neuen Hub nach der Wiederherstellung wieder mit diesen Daten auffüllt:

1. Erstellen Sie einen sekundären Notification Hub in einem anderen Datencenter. Es empfiehlt sich, gleich zu Anfang einen sekundären Notification Hub zu erstellen, um vor Notfallwiederherstellungsereignissen geschützt zu sein, die Ihre Verwaltungsmöglichkeiten beeinträchtigen. Sie können den sekundären Notification Hub aber auch erst zum Zeitpunkt des Notfallwiederherstellungsereignisses erstellen.

2. Füllen Sie die Registrierungen aus Ihrem primären Notification Hub im sekundären Notification Hub auf. Wir raten davon ab, die Registrierungen auf beiden Hubs zu verwalten und zu synchronisieren, wenn neue Registrierungen hinzukommen. Das funktioniert nicht sonderlich gut, da Registrierungen grundsätzlich dazu tendieren, auf der PNS-Seite abzulaufen. Notification Hubs bereinigt Registrierungen, wenn es PNS-Feedback zu abgelaufenen oder ungültigen Registrierungen empfängt.  

Zwei Empfehlungen für App-Back-Ends:

* Verwenden Sie ein App-Back-End, das auf seiner Seite eine bestimmte Gruppe von Registrierungen verwaltet. Das ermöglicht die Durchführung einer Masseneinfügung in den sekundären Notification Hub.

* Verwenden Sie ein App-Back-End, das regelmäßig eine Sicherungskopie der Registrierungen aus dem primären Notification Hub abruft. Das ermöglicht die Durchführung einer Masseneinfügung in den sekundären Notification Hub.

> [!NOTE]
> Informationen zu Export-/Importfunktionen für Registrierungen im Tarif „Standard“ finden Sie unter [Vorgehensweise: Massenhaftes Exportieren und Ändern von Registrierungen].
> 
> 

Falls Sie über kein Back-End verfügen und die App auf Zielgeräten gestartet wird, führen die Geräte eine neue Registrierung beim sekundären Notification Hub durch. Irgendwann sind dann alle aktiven Geräte beim sekundären Notification Hub registriert.

Innerhalb eines gewissen Zeitraums erhalten Geräte mit nicht geöffneten Apps keine Benachrichtigungen.

### <a name="is-there-audit-log-capability"></a>Steht ein Überwachungsprotokoll zur Verfügung?
Alle Notification Hubs-Verwaltungsvorgänge werden in Vorgangsprotokollen erfasst, die im [klassischen Azure-Portal]zur Verfügung stehen.

## <a name="monitoring-and-troubleshooting"></a>Überwachung und Problembehandlung
### <a name="what-troubleshooting-capabilities-are-available"></a>Welche Funktionen für die Problembehandlung stehen zur Verfügung?
Azure Notification Hubs bietet verschiedene Problembehandlungsfeatures – insbesondere für das häufig auftretende Problem verworfener Benachrichtigungen. Ausführliche Informationen finden Sie im [Whitepaper zur Problembehandlung für Notification Hubs].

### <a name="what-telemetry-features-are-available"></a>Welche Telemetriefunktionen stehen zur Verfügung?
Azure Notification Hubs ermöglicht die Anzeige von Telemetriedaten im [klassischen Azure-Portal]. Ausführliche Informationen zu den Metriken finden Sie auf der Seite [Metrics].

> [!NOTE]
> Wenn Benachrichtigungen erfolgreich waren, bedeutet das einfach, dass Pushbenachrichtigungen an den externen PNS (beispielsweise APNS für Apple oder GCM für Google) übermittelt wurden. Der PNS hat die Aufgabe, die Benachrichtigungen an Zielgeräte zu übermitteln. In der Regel macht der PNS Übermittlungsmetriken nicht für Dritte verfügbar.  
> 
> 

Wir bieten außerdem die Möglichkeit, Telemetriedaten programmgesteuert zu exportieren (im Standard-Tarif). Ausführliche Informationen finden Sie im [Metrikbeispiel für Notification Hubs].

[klassischen Azure-Portal]: https://manage.windowsazure.com
[Notification Hubs – Preise]: http://azure.microsoft.com/pricing/details/notification-hubs/
[Notification Hubs SLA]: http://azure.microsoft.com/support/legal/sla/
[Fallstudie: Sochi]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=7942
[Fallstudie: Skanska]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=5847
[Fallstudie: Seattle Times]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=8354
[Fallstudie: Mural.ly]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=11592
[Fallstudie: 7Digital]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=3684
[Seite zu REST-APIs von Notification Hubs]: https://msdn.microsoft.com/library/azure/dn530746.aspx
[Seite mit Einführungstutorials für Notification Hubs]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
[Erste Schritte mit Notification Hubs für Chrome-Apps]: http://azure.microsoft.com/documentation/articles/notification-hubs-chrome-get-started/
[Mobile Services Pricing]: http://azure.microsoft.com/pricing/details/mobile-services/
[Leitfaden zur Back-End-Registrierung]: https://msdn.microsoft.com/library/azure/dn743807.aspx
[Leitfaden zur Back-End-Registrierung 2]: https://msdn.microsoft.com/library/azure/dn530747.aspx
[Dokument zum Sicherheitsmodell von Notification Hubs]: https://msdn.microsoft.com/library/azure/dn495373.aspx
[Tutorial zu sicheren Pushbenachrichtigungen mit Notification Hubs]: http://azure.microsoft.com/documentation/articles/notification-hubs-aspnet-backend-ios-secure-push/
[Whitepaper zur Problembehandlung für Notification Hubs]: http://azure.microsoft.com/documentation/articles/notification-hubs-diagnosing/
[Metrics]: https://msdn.microsoft.com/library/dn458822.aspx
[Metrikbeispiel für Notification Hubs]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel
[Vorgehensweise: Massenhaftes Exportieren und Ändern von Registrierungen]: https://msdn.microsoft.com/library/dn790624.aspx
[Azure-Portal]: https://portal.azure.com
[complete samples]: https://github.com/Azure/azure-notificationhubs-samples
[Mobile Apps]: https://azure.microsoft.com/services/app-service/mobile/
[App Service – Preise]: https://azure.microsoft.com/pricing/details/app-service/

