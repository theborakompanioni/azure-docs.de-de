<properties 
	pageTitle="Konfigurieren einer App Service-Umgebung" 
	description="Konfiguration, Verwaltung und Überwachung von App Service-Umgebungen" 
	services="app-service" 
	documentationCenter="" 
	authors="ccompy" 
	manager="stefsch" 
	editor=""/>

<tags 
	ms.service="app-service" 
	ms.workload="na" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="06/20/2016" 
	ms.author="ccompy"/>


# Konfigurieren einer App Service-Umgebung #

## Übersicht ##

Allgemein besteht eine App Service-Umgebung aus mehreren Hauptkomponenten:

- Compute-Ressourcen, die im gehosteten Dienst der Azure App-Umgebung ausgeführt werden
- Speicher
- Datenbank
- Ein klassisches „v1“-Virtual Network mit mindestens einem Subnetz
- Subnetz mit darin ausgeführtem gehosteten Dienst der Azure App-Umgebung

### Computeressourcen

Die Computeressourcen werden für Ihre vier Ressourcenpools verwendet. Jede App Service-Umgebung verfügt über eine Gruppe von Front-Ends und drei mögliche Workerpools. Sie müssen nicht alle drei Workerpools verwenden und können auch nur einen oder zwei nutzen. Die Ressourcenpools, Front-Ends und Worker sind für die Mandanten nicht direkt zugänglich. Sie können nicht per RDP darauf zugreifen, ihre Bereitstellung ändern oder als Administrator für sie tätig werden, Sie können jedoch ihre Anzahl und Größe ändern. In einer App Service-Umgebung sind vier Größenoptionen verfügbar (P1 bis P4). Ausführliche Informationen zu diesen Größen und Preisen finden Sie unter [App Service-Preise](../app-service/app-service-value-prop-what-is.md). Es kann jeweils immer nur ein Skalierungsvorgang ausgeführt werden.

**Front-Ends** Die Front-Ends sind die HTTP/HTTPS-Endpunkte für Ihre Apps in Ihrer App Service-Umgebung. Führen Sie keine Workloads auf den Front-Ends aus.

- Eine App Service-Umgebung verfügt anfänglich über zwei (2) P2-Einheiten. Diese Größe ist für Dev/Test-Workloads und einfache Produktionsworkloads ausreichend. P3-Einheiten werden dringend für mittlere bis große Produktionsworkloads empfohlen.
- Für mittlere bis große Produktionsworkloads werden mindestens vier P3-Einheiten empfohlen, um sicherzustellen, dass bei einer geplanten Wartung genügend Front-Ends ausgeführt werden. Bei geplanten Wartungsaktivitäten wird jeweils ein (1) Front-End außer Betrieb genommen, sodass die Gesamtkapazität der Front-Ends während der Wartungsaktivitäten reduziert ist.
- Es ist nicht möglich, umgehend eine neue Front-End-Instanz hinzuzufügen. Die Bereitstellung dieser Instanzen kann 2-3 Stunden dauern.
- Für eine genauere Skalierung sollten Kunden den CPU-Prozentsatz, den Arbeitsspeicher-Prozentsatz und Metriken zu aktiven Anforderungen für den Front-End-Pool überwachen. Wenn der CPU- oder Arbeitsspeicherprozentsatz bei Ausführung von P3-Einheiten die Schwelle von 70 % überschreitet, sollten weitere Front-Ends hinzugefügt werden. Wenn die Anzahl von aktiven Anforderungen durchschnittlich bei 15.000 bis 20.000 Anforderungen pro Front-End liegt, sollten ebenfalls weitere Front-Ends hinzugefügt werden. Bei Ausführung von P3-Einheiten ist das Ziel also, für CPU und Arbeitsspeicher einen Prozentsatz von weniger als 70 % und für aktive Anforderungen im Durchschnitt eine Anzahl von weniger als 15.000 Anforderungen pro Front-End zu erreichen.  

**Worker** In den Workern werden Ihre Apps ausgeführt. Wenn Sie Ihre App Service-Pläne zentral hochskalieren, werden für diesen Vorgang Worker im zugeordneten Workerpool genutzt.

- Es ist nicht möglich, umgehend Worker hinzuzufügen. Unabhängig von der Anzahl von Workern, die hinzugefügt werden, kann die Bereitstellung 2 bis 3 Stunden dauern.
- Die Skalierung der Größe einer Computeressource dauert bei jedem Pool 2-3 Stunden pro Updatedomäne. Eine App Service-Umgebung verfügt über 20 Updatedomänen. Wenn Sie die Computegröße eines Workerpools mit 10 Instanzen skalieren, kann der Vorgang also 20 bis 30 Stunden dauern. 
- Wenn Sie die Größe der Computeressourcen in einem Workerpool ändern, verursacht dies möglicherweise einen Kaltstart der Apps, die in diesem Workerpool ausgeführt werden.

Die Größe von Computeressourcen eines Workerpools, in dem keine Apps ausgeführt werden, lässt sich wie folgt am schnellsten ändern:

- Skalieren Sie die Anzahl von Instanzen zentral auf 0 herunter. Das Aufheben der Zuordnung Ihrer Instanzen dauert ca. 30 Minuten.
- Wählen Sie die neue Computegröße und die Anzahl von Instanzen. Anschließend dauert es noch 2 bis 3 Stunden, bis der Vorgang abgeschlossen ist.

Wenn für Ihre Apps mehr Computeressourcen erforderlich sind, sind die vorstehenden Schritte keine geeignete Lösung. Anstatt die Größe des Workerpools zu ändern, in dem die Apps gehostet werden, können Sie einen anderen Workerpool mit Workern der gewünschten Größe füllen und Ihre Apps in diesen Pool verschieben.

- Erstellen Sie die zusätzlichen Instanzen der erforderlichen Computegröße in einem anderen Workerpool. Dieser Vorgang dauert 2 bis 3 Stunden.
- Weisen Sie Ihre App Service-Pläne, über die die Apps mit den höheren Ressourcenanforderungen gehostet werden, dem neu konfigurierten Workerpool zu. Dies ist ein schneller Vorgang, der in weniger als einer Minute abgeschlossen sein sollte.  
- Skalieren Sie den ersten Workerpool zentral herunter, wenn Sie die nicht genutzten Instanzen nicht mehr benötigen. Dieser Vorgang dauert ca. 30 Minuten.

**Automatische Skalierung** Die automatische Skalierung kann eingesetzt werden, um die Nutzung von Computeressourcen zu verwalten. Dieses Tool kann für Front-Ends oder Workerpools verwendet werden. Sie können z.B. die Instanzen in einem beliebigen Pooltyp morgens erhöhen und abends verringern oder Instanzen hinzufügen, wenn die Anzahl von verfügbaren Workern in einem Workerpool unter einen bestimmten Schwellenwert fällt. Wenn Sie für Metriken eines Computeressourcenpools Regeln für eine automatische Skalierung festlegen möchten, sollten Sie den erforderlichen Zeitaufwand für die Bereitstellung beachten. Weitere Einzelheiten zur automatischen Skalierung von App Service-Umgebungen finden Sie hier: [How to configure autoscale in an App Service Environment (Konfigurieren der automatischen Skalierung in einer App Service-Umgebung)][ASEAutoscale]

### Speicher

Jede App Service-Umgebung ist mit 500 GB Speicherplatz konfiguriert. Dieser Speicherplatz wird von allen Apps in der App Service-Umgebung verwendet. Er ist Teil der App Service-Umgebung und kann derzeit nicht anders genutzt werden (z.B. als Speicherplatz für den Kunden). Wenn Sie Anpassungen am VNET-Routing oder an der VNET-Sicherheit vornehmen, muss sichergestellt sein, dass weiterhin auf Azure Storage zugegriffen werden kann. Andernfalls ist die App Service-Umgebung nicht funktionsfähig.

### Datenbank

Die Datenbank enthält Informationen, die die Umgebung definieren, und auch Details zu den darin ausgeführten Apps. Dies ist ebenfalls Teil des gebuchten Azure-Abonnements. Die Kunden haben hier keine direkten Möglichkeiten zu Änderungen. Wenn Sie Anpassungen am VNET-Routing oder an der VNET-Sicherheit vornehmen, muss sichergestellt sein, dass weiterhin auf Azure SQL-Datenbank zugegriffen werden kann. Andernfalls ist die App Service-Umgebung nicht funktionsfähig.

### Netzwerk

Beim virtuellen Netzwerk, das mit Ihrer App Service-Umgebung verwendet wird, kann es sich um ein Netzwerk handeln, dass Sie beim Erstellen der App Service-Umgebung eingerichtet haben, oder um ein Netzwerk, das Sie bereits vorab erstellt haben. Wenn sich Ihr VNET in einer Ressourcengruppe befinden soll, die von der für Ihre ASE verwendeten getrennt ist, dann müssen Sie Ihr VNET unabhängig vom Arbeitsablauf bei der ASE-Erstellung erstellen. Wenn Sie das Subnetz während der Erstellung der App Service-Umgebung erstellen, muss sich die App Service-Umgebung in derselben Ressourcengruppe befinden wie das VNET.

Für VNETs, die für eine App Service-Umgebung verwendet werden, gelten einige Einschränkungen:

- Zurzeit werden nur klassische V1-VNETs unterstützt
- Das VNET muss ein regionales VNET sein
- Infolge einer im Juni 2016 vorgenommenen Änderung können nun ASEs in virtuellen Netzwerken bereitgestellt werden, die *entweder* öffentliche Adressbereiche *oder* RFC1918-Adressräume (d.h. private Adressen) verwenden. Um ein virtuelles Netzwerk mit einem öffentlichen Adressbereich zu verwenden, müssen Sie das Subnetz vorab erstellen und dann das Subnetz auf der UX für die ASE-Erstellung auswählen.
- Es muss ein Subnetz mit mindestens acht Adressen vorhanden sein, in dem die ASE bereitgestellt wird.
- Sobald ein Subnetz zum Hosten einer App Service-Umgebung verwendet wird, kann der Adressbereich des Subnetzes nicht mehr geändert werden. Um zukünftigem Wachstum der ASE gerecht werden zu können, sollte das Subnetz daher mindestens 64 Adressen umfassen 
- **Das für das Hosting der ASE verwendete Subnetz darf keine anderen Compute-Ressourcen enthalten.**

Anders als beim gehosteten Dienst, der die App Service-Umgebung umfasst, werden das [virtuelle Netzwerk][virtualnetwork] und das Subnetz vom Benutzer gesteuert. Die Verwaltung Ihres VNET erfolgt über die Virtual Network-Benutzeroberfläche oder über PowerShell.

Da diese Funktion den Azure App Service in Ihr VNET versetzt, können die in Ihrer App Service-Umgebung gehosteten Apps jetzt auf Ressourcen zugreifen, die über ExpressRoute oder Site-to-Site-VPNs zur Verfügung gestellt werden. Die Apps in Ihren App Service-Umgebungen benötigen keine zusätzlichen Netzwerkfunktionen für den Zugriff auf Ressourcen, die für das VNET verfügbar sind, das Ihre App Service-Umgebung hostet. Das bedeutet, dass Sie keine VNET-Integration oder Hybridverbindungen verwenden müssen, um Ressourcen in das VNET zu bringen oder mit dem VNET zu verbinden. Sie können jedoch nach wie vor beide Funktionen nutzen, um auf Ressourcen in Netzwerken zuzugreifen, die nicht mit dem VNET verbunden sind. Zum Beispiel können Sie mithilfe der VNET-Integration ein VNET integrieren, das Teil Ihres Abonnements, aber nicht mit dem VNET verbunden ist, in dem sich Ihre App Service-Umgebung befindet. Sie können auch weiterhin Hybridverbindungen verwenden, um ganz normal auf Ressourcen in anderen Netzwerken zuzugreifen.

Falls Ihr VNET mit einem ExpressRoute-VPN konfiguriert ist, müssen Sie einige der Routinganforderungen einer App Service-Umgebung kennen. Es gibt einige Konfigurationen für benutzerdefiniertes Routing (UDR, User Defined Routing), die nicht mit einer App Service-Umgebung kompatibel sind. Weitere Details zum Ausführen einer App Service-Umgebung in einem VNET mit ExpressRoute finden Sie im Dokument [Ausführen einer App Service-Umgebung in einem VNET mit ExpressRoute][ExpressRoute].

Außerdem können Sie den Zugriff auf Ihre Apps jetzt auch mithilfe von Netzwerksicherheitsgruppen steuern. Diese Funktion ermöglicht Ihnen das Sperren Ihrer App Service-Umgebung, sodass nur die von Ihnen angegebenen IP-Adressen darauf zugreifen können. Weitere Informationen hierzu finden Sie unter [Steuern des eingehenden Datenverkehrs in einer App Service-Umgebung](app-service-app-service-environment-control-inbound-traffic.md).

## Portal

Die Benutzeroberfläche zum Verwalten und Überwachen der App Service-Umgebung finden Sie im Azure-Portal. Wenn für Sie eine App Service-Umgebung vorhanden ist, sehen Sie normalerweise in der Randleiste das App Service-Symbol. Im Azure-Portal werden App Service-Umgebungen mit folgendem Symbol dargestellt:

![][1]

Sie können das Symbol verwenden oder unten in der Randleiste auf das Chevron (Größer-als-Zeichen) klicken und „App Service-Umgebungen“ wählen. Mit beiden Vorgehensweisen erreichen Sie dasselbe: Sie öffnen eine Benutzeroberfläche, die all Ihre App Service-Umgebungen auflistet. Wenn Sie auf eine der aufgelisteten App Service-Umgebungen klicken, wird eine Benutzeroberfläche zum Überwachen und Verwalten der jeweiligen Umgebung angezeigt.

![][2]

Das erste Blatt zeigt einige Eigenschaften der App Service-Umgebung zusammen mit einem Metrikdiagramm pro Ressourcenpool. Einige Eigenschaften im Block „Zusammenfassung“ sind auch Links, mit denen Sie das entsprechende Blatt öffnen können. Sie können z.B. auf den VNET-Namen klicken und dadurch die Benutzeroberfläche für das VNET öffnen, in dem Ihre App Service-Umgebung ausgeführt wird. Mit „App Service-Pläne“ und „Apps“ öffnen Sie jeweils Blätter mit Auflistungen dieser in Ihrer App Service-Umgebung enthaltenen Elemente.

### Überwachung

Mit den Diagrammen können Sie eine Reihe von Leistungsmetriken im jeweiligen Ressourcenpool verfolgen. Überwachen Sie für den Front-End-Pool die durchschnittliche CPU- und Speicherauslastung. Überwachen Sie bei Workerpools die verwendete sowie die verfügbare Anzahl. Die Worker in einem Workerpool können von mehreren App Service-Plänen genutzt werden. Die Workload wird nicht auf dieselbe Weise verteilt wie bei Front-End-Servern. Daher liefern die CPU- und die Arbeitsspeicherauslastung keine vergleichbaren nützlichen Informationen. Es ist wichtiger zu verfolgen, wie viele Worker verwendet wurden und wie viele verfügbar sind. Dies gilt besonders, wenn Sie das System für die Nutzung durch andere verwalten.

Alle Metriken, die sich in den Diagrammen verfolgen lassen, können auch zur Einrichtung von Warnungen verwendet werden. Beim Einrichten von Warnungen gehen Sie genauso vor wie sonst auch im App Service. Sie können eine Warnung einrichten, indem Sie den für Warnungen vorgesehenen Teil der Benutzeroberfläche verwenden oder indem Sie auf einer beliebigen Benutzeroberfläche für Metriken die Details öffnen und auf „Warnung hinzufügen“ klicken.
 
![][3]

Bei den gerade diskutierten Metriken handelt es sich um Metriken für App Service-Umgebungen. Darüber hinaus stehen auch auf der Ebene der App Service-Pläne Metriken zur Verfügung. Dort ist das Überwachen von CPU- und Speicherauslastung besonders sinnvoll. In einer App Service-Umgebung sind alle App Service-Pläne dedizierte App Service-Pläne. Das heißt: Nur die Apps, die auf den zugeordneten Hosts für den jeweiligen App Service-Plan ausgeführt werden, sind Apps in diesem App Service-Plan. Um die Details zu Ihrem App Service-Plan anzuzeigen, rufen Sie den App Service-Plan aus einer beliebigen Liste in der Benutzeroberfläche der App Service-Umgebung heraus auf, oder Sie nutzen die Funktion zum Durchsuchen von App Service-Plänen, die alle Pläne auflistet.

### Einstellungen

Das Blatt „App Service-Umgebung“ umfasst einen Abschnitt „Einstellungen“, auf dem Optionen zu verschiedenen wichtigen Funktionen festgelegt werden können.

**Einstellungen > Eigenschaften** Das Blatt „Einstellungen“ wird automatisch angezeigt, wenn Sie das Blatt „App Service-Umgebung“ öffnen. Im oberen Bereich finden Sie „Eigenschaften“. Einige Elemente sind identisch mit dem, was Sie unter „Zusammenfassung“ sehen. Sehr nützlich sind jedoch die VIP-Adresse und die ausgehende IP-Adresse.

![][4]

**Einstellungen > IP-Adressen** Wenn Sie eine IP-SSL-App in Ihrer ASE erstellen, benötigen Sie eine IP-SSL-Adresse. Damit das möglich wird, muss die App Service-Umgebung über eigene IP-SSL-Adressen verfügen, die zugewiesen werden können. Beim Erstellen einer App Service-Umgebung verfügt diese über 1 IP-SSL-Adresse für diesen Zweck. Sie können jedoch weitere Adressen hinzufügen. Für zusätzliche IP-SSL-Adressen fallen Gebühren an. Diese sind im Abschnitt zu SSL-Verbindungen unter [App Service – Preise][AppServicePricing] aufgeführt. Der IP-SSL-Preis ist der zusätzliche Preis.

**Einstellungen > Front-End-Pool/Workerpools** Auf jedem dieser Ressourcenpoolblätter werden nur die Informationen für den jeweiligen Ressourcenpool angezeigt. Außerdem enthalten die Blätter Steuerelemente, mit denen dieser Ressourcenpool skaliert werden kann.

Das Grundlagenblatt für jeden Ressourcenpool enthält ein Diagramm mit den Metriken zu diesem Ressourcenpool. Ähnlich wie bei den Diagrammen im App Service-Umgebungsblatt können Sie auch hier das Diagramm öffnen und wie gewünscht Warnungen einrichten. Ob Sie eine Warnung im App Service-Umgebungsblatt für einen bestimmten Ressourcenpool einrichten oder dafür das Blatt für den Ressourcenpool verwenden, spielt keine Rolle – das Ergebnis ist dasselbe. Im Blatt „Einstellungen“ für den Workerpool können Sie alle Apps oder App Service-Pläne auflisten, die in diesem Workerpool ausgeführt werden.

![][5]

### Skalierungsfunktionen im Portal  

Es gibt drei Skalierungsvorgänge:

- Ändern der Anzahl von IP-Adressen in der App Service-Umgebung, die für die IP-SSL-Nutzung zur Verfügung stehen
- Ändern der Größe der Computeressource, die in einem Ressourcenpool verwendet wird
- Ändern der Anzahl von Computeressourcen in einem Ressourcenpool, entweder manuell oder durch automatische Skalierung

Im Portal gibt es drei Möglichkeiten, mit denen Sie steuern können, wie viele Server sich in Ihren Ressourcenpools befinden

- Mit dem Skalierungsvorgang oben auf dem Hauptblatt für die App Service-Umgebung. Sie können mehrere Änderungen an der Skalierungskonfiguration für Front-Ends und Workerpools vornehmen, und diese werden als einzelner Vorgang angewendet.
- Mit manuellem Skalierungsvorgang unter „Einstellungen“ auf dem Blatt „Skalieren“ für den jeweiligen Resourcenpool
- Mit der automatischen Skalierung, die Sie vom Blatt „Skalieren“ für den jeweiligen Ressourcenpool einrichten

Um den Skalierungsvorgang auf dem Blatt „App Service-Umgebung“ zu verwenden, ziehen Sie den betreffenden Schieberegler auf die gewünschte Menge und speichern die neue Einstellung. Mit dieser Benutzeroberfläche können Sie auch die Größe ändern.

![][6]

Wenn Sie die Funktionen für die manuelle oder die automatische Skalierung in einem bestimmten Ressourcenpool verwenden möchten, wechseln Sie zu *Einstellungen > Front-End-Pool bzw. Workerpools*, und öffnen Sie den Pool, den Sie ändern möchten. Wählen Sie *Einstellungen > Horizontal hochskalieren bzw. Einstellungen > Zentral hochskalieren*. Auf dem Blatt *Horizontal hochskalieren* können Sie die Anzahl von Instanzen steuern. Die Option *Zentral hochskalieren* ermöglicht die Festlegung der Ressourcengröße.

![][7]

## Überlegungen zur Fehlertoleranz

Eine App Service-Umgebung kann für die Verwendung von bis zu 55 Compute-Ressourcen insgesamt konfiguriert werden. Von diesen 55 Compute-Ressourcen können nur 50 zum Hosten von Workloads eingesetzt werden. Dafür gibt es zwei Gründe. Es sind mindestens zwei Front-End-Compute-Ressourcen erforderlich. So bleiben bis zu 53 zur Unterstützung der Workerpoolzuweisung übrig. Um Fehlertoleranz bereitzustellen, muss nach den folgenden Regeln eine zusätzliche Computeressource zugewiesen werden:

- Jeder Workerpool benötigt mindestens eine weitere Computeressource, der keine Workload zugewiesen werden kann.
- Wenn die Menge von Computeressourcen in einem Workerpool einen bestimmten Wert überschreitet, ist aus Gründen der Fehlertoleranz eine weitere Computeressource erforderlich. Im Front-End-Pool ist dies nicht der Fall.

Im Rahmen der Fehlertoleranzanforderungen gilt innerhalb eines einzelnen Workerpools Folgendes für einen bestimmten Wert von X Ressourcen, die einem Workerpool zugewiesen sind:

- Wenn X zwischen 2 und 20 liegt, beträgt die Anzahl der Compute-Ressourcen, die für Workloads verwendet werden können, X-1
- Wenn X zwischen 21 und 40 liegt, beträgt die Anzahl der Compute-Ressourcen, die für Workloads verwendet werden können, X-2
- Wenn X zwischen 41 und 53 liegt, beträgt die Anzahl der Compute-Ressourcen, die für Workloads verwendet werden können, X-3

Es sind mindestens zwei Front-End-Server und zwei Worker erforderlich. Die vorherigen Angaben lassen sich durch ein paar Beispiele gut veranschaulichen.

- Wenn sich 30 Worker in einem einzigen Pool befinden, können 28 für Workloads verwendet werden. 
- Wenn sich zwei Worker in einem einzigen Pool befinden, kann einer für Workloads verwendet werden.
- Wenn sich 20 Worker in einem einzigen Pool befinden, können 19 für Workloads verwendet werden.  
- Wenn sich 21 Worker in einem einzigen Pool befinden, können immer noch nur 19 für Workloads verwendet werden.  

Der Aspekt der Fehlertoleranz ist wichtig, und Sie müssen ihn im Auge behalten, wenn Sie bei der Skalierung bestimmte Schwellenwerte überschreiten. Wenn Sie ausgehend von 20 Instanzen mehr Kapazität hinzufügen möchten, sollten Sie 22 oder mehr Instanzen wählen, weil bei 21 die Kapazität nicht steigt. Dasselbe trifft zu, wenn Sie 40 Instanzen überschreiten. Die nächste Anzahl, bei der die Kapazität steigt, ist 42.

## Löschen einer App Service-Umgebung ##

Wenn Sie eine App Service-Umgebung löschen möchten, verwenden Sie einfach die Aktion "Löschen" im oberen Bereich des Blatts "App Service-Umgebung". Wenn Sie dies tun, werden Sie aufgefordert, den Namen Ihrer App Service-Umgebung einzugeben. Auf diese Weise bestätigen Sie, dass Sie diesen Schritt wirklich ausführen möchten. HINWEIS: Beim Löschen einer App Service-Umgebung wird auch deren gesamter Inhalt gelöscht.

![][9]

## Erste Schritte
Alle Artikel und Anleitungen zu App Service-Umgebungen stehen in der [Dokumentation zu App Service-Umgebungen](../app-service/app-service-app-service-environments-readme.md) zur Verfügung.

Informationen zum Einstieg in App Service-Umgebungen finden Sie unter [Erstellen einer App Service-Umgebung](app-service-web-how-to-create-an-app-service-environment.md)

Weitere Informationen zur Azure App Service-Plattform finden Sie unter [Azure App Service](../app-service/app-service-value-prop-what-is.md).

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-web-configure-an-app-service-environment/ase-icon.png
[2]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-aseblade.png
[3]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolchart.png
[4]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-properties.png
[5]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolblade.png
[6]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-scalecommand.png
[7]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolscale.png
[8]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-pricingtiers.png
[9]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-deletease.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoScale]: http://azure.microsoft.com/documentation/articles/app-service-web-scale-a-web-app-in-an-app-service-environment/
[ControlInbound]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ExpressRoute]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/

<!---HONumber=AcomDC_0622_2016-->