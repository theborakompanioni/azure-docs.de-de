<properties
   pageTitle="Häufig gestellte Fragen"
   description="Häufig gestellte Fragen zu Power BI Embedded"
   services="power-bi-embedded"
   documentationCenter=""
   authors="minewiskan"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="06/28/2016"
   ms.author="owend"/>

# Häufig gestellte Fragen zu Power BI Embedded

1. **Was wurde während der Buildphase zu Power BI angekündigt?**

    Microsoft hat angekündigt, dass die öffentliche Vorschau zu Microsoft Power BI Embedded jetzt verfügbar ist.

2.	**Was ist Microsoft Power BI Embedded?**

    Microsoft Power BI Embedded ist ein Azure-Dienst, mit dessen Hilfe Anwendungsentwickler beeindruckende, vollständig interaktive Berichte und Visualisierungen in die für Kunden zugänglichen Apps einbinden können, ohne die Zeit und die Kosten für die Erstellung eigener, ganz neuer Steuerelemente aufwenden zu müssen. Sie können aus einer Vielzahl von Datenvisualisierungen wählen, die im Paket enthalten sind, oder ganz leicht benutzerdefinierte Visualisierungen erstellen, die den spezifischen Anforderungen Ihrer Anwendung entsprechen. Power BI Embedded ist für ISV-/Anwendungsentwickler vorgesehen, die ihren Kunden BI als Bestandteil ihrer größeren Anwendung bereitstellen möchten.

3.	**Für wen bietet sich die Verwendung von Microsoft Power BI Embedded an, und warum?**

    Microsoft Power BI Embedded ist für Anwendungsentwickler konzipiert, die ihren Benutzern beeindruckende und interaktive Benutzeroberflächen zur Datenvisualisierung auf allen Geräten bieten möchten, ohne sie selbst erstellen zu müssen. Mit Power BI Embedded können Entwickler immer aktuelle Ansichten mit **DirectQuery** bereitstellen. Zudem können Entwickler Power BI mithilfe der Azure-ARM-APIs und der Power BI-APIs programmgesteuert bereitstellen, verwalten und automatisieren. Wie alle Power BI-Komponenten wird der Embedded-Dienst automatisch skaliert, um der Nutzung und den Anforderungen Ihrer Anwendung gerecht zu werden. Der Power BI Embedded-Dienst umfasst ein Preismodell mit nutzungsbasierter Bezahlung. Informationen zu den Preisen finden Sie unter [Wie wird dieser Dienst abgerechnet?](#price).

4.	**In welchem Zusammenhang stehen Power BI Embedded und der Power BI-Dienst?**

    Power BI Embedded und der Power BI-Dienst werden getrennt voneinander angeboten. Power BI Embedded umfasst ein Preismodell mit nutzungsbasierter Bezahlung, wird über das Azure-Portal bereitgestellt und dient dazu, ISVs das Einbetten von Datenvisualisierungen in Kundenanwendungen zu ermöglichen. Der Power BI-Dienst wird über das O365-Portal abgerechnet und bereitgestellt. Dieser Dienst ist ein eigenständiges allgemeines BI-Angebot, das in erster Linie für die interne Verwendung innerhalb von Unternehmen bereitgestellt wird. Weitere Informationen zum Power BI-Dienst finden Sie unter [www.powerbi.com](www.powerbi.com).

5.	**Inwiefern wird meine App durch Power BI Embedded verbessert?**

    Anwendungen sind deutlich leistungsfähiger, wenn Benutzerentscheidungen anhand von Datenvisualisierungen direkt in der Anwendung beeinflusst werden können. Mit Power BI Embedded können Sie Ihre App durch interaktive, stets aktuelle und umfassende Datenvisualisierungen erweitern, sodass Sie die Zweckmäßigkeit der App sowie Kundenzufriedenheit und -treue erhöhen und mühelos kontextbezogene Analysen auf jedem Gerät bereitstellen können.

6.	**Gibt es Regeln oder Einschränkungen für die Verwendung von Power BI Embedded in meiner App?**

    Sie dürfen den Power BI Embedded-Dienst innerhalb einer von Ihnen entwickelten Anwendung nur dann nutzen, wenn Ihre Anwendung (1) grundlegende und wichtige Funktionalität zu Power BI Embedded hinzufügt und nicht in erster Linie ein Ersatz für einen Power BI-Dienst ist, und (2) ausschließlich für Benutzer außerhalb Ihres Unternehmens bereitgestellt wird. Der Power BI Embedded-Dienst darf nicht innerhalb interner Geschäftsanwendungen verwendet werden.

7.	**Kann Power BI Embedded zum Erstellen interner Anwendungen verwendet werden?**

    Nein, Power BI Embedded ist nur für die Verwendung durch externe Benutzer gedacht und kann nicht innerhalb interner Geschäftsanwendungen verwendet werden. Um Power BI-Inhalte zur Verwendung in internen Geschäftsanwendungen einzubetten, sollten Sie den Power BI-Dienst nutzen. Außerdem müssen alle Benutzer, die diese Inhalte verwenden, über eine gültige Lizenz im Rahmen eines Power BI Free- oder Power BI Pro-Benutzerabonnements verfügen. Weitere Informationen zur internen Einbettung mithilfe des Power BI-Diensts sind unter [https://dev.powerbi.com](https://dev.powerbi.com) verfügbar.

8.	**Ist dieser Dienst global verfügbar?**

    Der Power BI Embedded-Dienst ist in Nordamerika ab dem Zeitpunkt unserer Ankündigung von BUILD 2016 (im Rechenzentrum USA, Süden-Mitte) verfügbar. Dieser Dienst wird gleich im Anschluss auf die übrigen Azure-Rechenzentren ausgedehnt.

9. **Welche SLA ist für den Dienst verfügbar?**

    Power BI Embedded steht jetzt als Azure-Vorschaudienst ohne formale SLA zur Verfügung. Eine SLA wird bereitgestellt, wenn der Dienst von der Vorschauversion in die allgemeine Verfügbarkeit wechselt.

10. <a name="price"/>**Wie wird dieser Dienst abgerechnet?**

    Power BI Embedded befindet sich zurzeit in der Vorschauphase und wird bis zum 1. Mai 2016 kostenlos angeboten. Ab dem 1. Mai 2016 wird der Dienst pro Renderobjekt abgerechnet. Kunden können den Dienst über zwei primäre Lizenzierungswege erwerben: das Microsoft Online Subscription-Programm (MOSP) oder das Volumenlizenzierungsprogramm für Unternehmen.

    Sie dürfen den Power BI Embedded-Dienst innerhalb einer von Ihnen entwickelten Anwendung nur dann nutzen, wenn Ihre Anwendung (1) grundlegende und wichtige Funktionalität zu unserem Dienst hinzufügt und nicht in erster Linie ein Ersatz für einen Power BI-Dienst ist, und (2) ausschließlich für externe Benutzer bereitgestellt wird. Der Power BI Embedded-Dienst darf nicht innerhalb interner Geschäftsanwendungen verwendet werden.


   ![](media\power-bi-embedded-faq\price.png)

11. **Was ist ein Renderobjekt, und wie wird es abgerechnet?**

    Ein Renderobjekt ist ein Visualisierungselement, das einem Endbenutzer angezeigt wird und das eine Abfrage an den Dienst erfordert. Der Power BI-Dienst versucht, gerenderte Inhalte nach Möglichkeit zwischenzuspeichern, um die Anzahl der Renderobjekte zu reduzieren, die Ihrer Anwendung berechnet werden. Allerdings können Benutzeraktionen wie das Filtern zu einer Abfrage an den Dienst führen, sodass ein neues Renderobjekt entsteht.

    Wenn ein Benutzer beispielsweise einen Bericht mit vier visuellen Elementen anzeigt, kann dies zu vier Renderobjekten führen. Wenn der Benutzer den Bericht aktualisiert und weitere Abfragen an den Dienst gesendet werden, hat dies vier weitere Renderobjekte zur Folge. Um die Kosten zu deckeln bzw. in statischen Datenszenarien zu minimieren, kann der Dienstbesitzer steuern, in welchem Umfang Endbenutzer neue Abfragen generieren können, die zahlungspflichtige Renderobjekte zur Folge haben.

    Die Abrechnung erfolgt pro 1.000 Renderobjekte. Wenn weniger als 1.000 Renderobjekte in Anspruch genommen werden, erfolgt die Abrechnung anteilsmäßig. Pro Monat erhalten Kunden ein kostenloses Kontingent von 1.000 Renderobjekten. Kunden, die ihre Lösung über die Volumenlizenzierung erwerben, erhalten Preisinformationen von ihrem Microsoft-Partner oder -Händler.

12. **Gibt es Tools oder Anleitungen, die mir helfen, die zu erwartende Anzahl von Renderobjekten abzuschätzen? Wie kann ich feststellen, wie viele Renderobjekte abgeschlossen sind?**

    Das Azure-Portal bietet Abrechnungsinformationen dazu, wie viele Renderobjekte für Ihr Abonnement ausgeführt wurden.

13. **Benötige ich zum Entwickeln von Anwendungen mit Power BI Embedded ein Power BI-Abonnement? Wie fange ich an?**

    Als Anwendungsentwickler brauchen Sie kein Power BI-Abonnement, um die Berichte und Visualisierungen zu erstellen, die Sie in Ihrer Anwendung verwenden möchten. Sie benötigen ein Microsoft Azure-Abonnement und die kostenlose Power BI Desktop-Anwendung.

    Einzelheiten zur Verwendung des Power BI Embedded-Diensts finden Sie in der Dokumentation zum Dienst.

14. **Ich habe ein Azure-Abonnement. Kann ich Power BI Embedded mit meinem vorhandenen Abonnement verwenden?**

    Ja. Sie können den Microsoft Power BI Embedded-Dienst mit Ihrem vorhandenen Azure-Abonnement bereitstellen und verwenden.

15. **Benötigt ein Endbenutzer meiner Anwendung eine Power BI-Lizenz?**

    Nein. Endbenutzer Ihrer Anwendung müssen kein eigenes Power BI-Abonnement kaufen, um auf die In-App-Datenvisualisierungen zuzugreifen. Im Power BI Embedded-Modell wird der Dienst dem Anwendungsanbieter über den Azure-Verbrauchszähler in Rechnung gestellt. Weitere Details finden Sie auf der [Seite mit Preis- und Lizenzierungsinformationen](http://go.microsoft.com/fwlink/?LinkId=760527).

16. **Wie funktioniert die Benutzerauthentifizierung bei Power BI Embedded?**

    Der Power BI Embedded-Dienst nutzt App-Token zur Authentifizierung und Autorisierung anstelle der expliziten Endbenutzerauthentifizierung. Im App-Token-Modell verwaltet Ihre Anwendung die Authentifizierung und Autorisierung für Ihre Endbenutzer. Bei Bedarf erstellt und sendet Ihre App dann App-Token, die unseren Dienst anweisen, den angeforderten Bericht zu rendern. Bei diesem Konzept muss Ihre App nicht Azure Active Directory für die Authentifizierung und Autorisierung der Benutzer verwenden, auch wenn dies möglich ist. Weitere Informationen zu App-Token finden Sie auf der Dokumentationsseite zu [App-Token](https://azure.microsoft.com/de-DE/documentation/articles/power-bi-embedded-get-started-sample/#key-flow).

17. **Welche Datenquellen werden derzeit mit Power BI Embedded unterstützt?**

    Während der öffentlichen Vorschau des Diensts unterstützen wir den Zugriff auf Clouddatenquellen, die die Basisanmeldeinformationen über DirectQuery verwenden. Das bedeutet, dass Quellen wie Azure SQL-Datenbank, HDInsight Spark und Azure SQL Data Warehouse zurzeit unterstützt werden. In den nächsten Monaten fügen wir Unterstützung für andere Datenquellen und Zugriffstypen hinzu. Neue unterstützte Datenquellen werden auf der Power BI-Entwicklerwebsite unter [http://dev.powerbi.com](http://dev.powerbi.com/) angekündigt.

18. **Wie funktioniert das Mandantenmodell für Power BI Embedded?**

    Im Power BI Embedded-Modell müssen Ihre Kunden nicht unbedingt in Azure Active Directory-Mandanten (Azure AD) verwaltet werden. Sie können festlegen, ob Azure AD für Ihre Kunden erforderlich ist oder nicht. Daher richtet sich das für Power BI Embedded erforderliche Mandantenmodell nach der Architektur Ihrer Anwendung und Ihrer Infrastruktur.

    Entwickler/Mitarbeiter, die Ihre Anwendung erstellen oder daran arbeiten, benötigen ein Azure AD-Benutzerkonto, damit sie Ihr Azure-Abonnement und Ihre Arbeitsbereichssammlungen über das Azure-Portal verwalten können. Programmgesteuerte APIs, mit denen Entwickler Berichte importieren, Verbindungszeichenfolgen ändern und Einbettungs-URLs abrufen können, verwenden stattdessen App-Token zur Authentifizierung und brauchen Azure AD daher nicht. Details zur Verwendung unserer APIs und des Azure-Portals finden Sie auf der [Power BI Embedded-Dokumentationsseite auf Azure.com](https://azure.microsoft.com/de-DE/documentation/services/power-bi-embedded/).

19. **Wo kann ich mehr erfahren?**

    Besuchen Sie die [Power BI Embedded-Dokumentationsseite](http://go.microsoft.com/fwlink/?LinkId=760526). Sie können zu diesem Dienst auf dem neuesten Stand bleiben, indem Sie den [Power BI-Entwicklerblog](http://blogs.msdn.com/powerbidev) oder das Power BI Developer Center unter „dev.powerbi.com“ besuchen. Unter [Stackoverflow](http://stackoverflow.com/questions/tagged/powerbi) können Sie auch Fragen stellen.

20. **Wie fange ich an?**

    Sie können jetzt kostenlos beginnen! Wenn Sie ein Azure-Abonnement besitzen, können Sie Power BI Embedded jetzt direkt über das Azure-Portal bereitstellen. Sie können auch ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) erstellen. Sobald Sie den Power BI Embedded-Dienst bereitgestellt haben, können Sie problemlos Power BI-REST-APIs direkt einsetzen oder das Developer SDK auf [GitHub](http://go.microsoft.com/fwlink/?LinkID=746472) verwenden. Es werden Beispiele für die Nutzung des Developer SDK bereitgestellt.

## Weitere Informationen

- [Was ist Microsoft Power BI Embedded](power-bi-embedded-what-is-power-bi-embedded.md)
- [Erste Schritte mit der Vorschau von Microsoft Power BI Embedded](power-bi-embedded-get-started.md)

<!---HONumber=AcomDC_0629_2016-->