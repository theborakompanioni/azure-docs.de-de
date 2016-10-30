<properties
   pageTitle="Übersicht über Service Fabric | Microsoft Azure"
   description="Eine Übersicht über Service Fabric und das Konzept der Anwendungen, die sich aus Microservices zusammensetzen, um Skalierung und Resilienz zu bieten. Service Fabric ist eine Plattform für verteilte Systeme und ermöglicht das Erstellen skalierbarer, zuverlässiger und einfach zu verwaltender Anwendungen für die Cloud."
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor="masnider"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/22/2016"
   ms.author="mfussell"/>


# <a name="overview-of-service-fabric"></a>Übersicht über Service Fabric
Service Fabric ist eine Plattform für verteilte Systeme, die das Packen, Bereitstellen und Verwalten skalierbarer und zuverlässiger Microservices vereinfacht. Service Fabric bietet außerdem einfache Lösungen für die komplexen Herausforderungen bei der Entwicklung und Verwaltung von Cloudanwendungen. Entwickler und Administratoren können komplexe Infrastrukturprobleme vermeiden und sich stattdessen auf das Implementieren geschäftskritischer, anspruchsvoller Workloads konzentrieren, die skalierbar, zuverlässig und einfach zu verwalten sind. Service Fabric ist die Middlewareplattform der nächsten Generation für das Erstellen und Verwalten leistungsstarker Tier-1-Anwendungen mit Cloudskalierung.

## <a name="applications-composed-of-microservices"></a>Aus Microservices bestehende Anwendungen
Service Fabric ermöglicht das Erstellen und Verwalten skalierbarer und zuverlässiger Anwendungen, die sich aus Microservices zusammensetzen. Diese Microservices werden in einem gemeinsam genutzten Computerpool (als Cluster bezeichnet) mit sehr hoher Dichte ausgeführt. Die Plattform bietet eine ausgereifte Laufzeit für das Erstellen verteilter, skalierbarer zustandsloser und zustandsbehafteter Microservices sowie umfassende Anwendungsverwaltungsfunktionen zum Organisieren, Bereitstellen, Überwachen, Upgraden/Patchen und Löschen bereitgestellter Anwendungen.

Warum ist ein Microserviceansatz wichtig? Die beiden wichtigsten Gründe sind:

1. Mit Microservices können Sie verschiedene Teile einer Anwendung nach Bedarf skalieren.

2. Entwicklungsteams können beim Einführen von Änderungen flexibler vorgehen und dadurch Funktionen für Ihre Kunden schneller und häufiger bereitstellen.

Service Fabric liegt schon heute zahlreichen Microsoft-Diensten zugrunde. Hierzu zählen unter anderem Azure SQL-Datenbank, Azure DocumentDB, Cortana, Power BI, Microsoft Intune, Azure Event Hubs, Azure IoT, Skype for Business und viele weitere Azure-Kerndienste.

Service Fabric wurde konzipiert, um das Erstellen von Diensten zu ermöglichen, die direkt in der Cloud „entstehen“. Die Dienste können zu Beginn klein ausfallen und je nach Bedarf auf Hunderte oder Tausende von Computern skaliert werden.

Internetdienste setzen sich heutzutage aus Microservices zusammen. Beispiele für Microservices sind etwa Protokollgateways, Benutzerprofile, Einkaufswagen, Bestandsverarbeitungsvorgänge, Warteschlangen und Caches. Service Fabric ist eine Microservices-Plattform, in der jeder Microservice einen eindeutigen Namen erhält und entweder zustandslos oder zustandsbehaftet sein kann.

Service Fabric bietet umfassende Verwaltungsfunktionen für die Laufzeit und den Lebenszyklus der Anwendungen, die sich aus diesen Microservices zusammensetzen. Die in Containern gehosteten Microservices werden im Service Fabric-Cluster bereitgestellt und aktiviert. Die Umstellung von VMs auf Container ermöglicht eine Zunahme der Dichte in größerem Umfang. Eine weitere deutliche Steigerung der Dichte lässt sich dann erzielen, indem die Umstellung von Containern auf Microservices durchgeführt wird. Ein einzelner Azure SQL-Datenbank-Cluster kann beispielsweise aus Hunderten von Rechnern bestehen, die Zehntausende von Containern ausführen, die wiederum Hunderttausende von Datenbanken hosten. Jede Datenbank ist dabei ein zustandsbehafteter Service Fabric-Microservice. Da dasselbe für die anderen erwähnten Dienste gilt, wird der Begriff der „Hyperskalierung“ verwendet, um Service Fabric-Funktionen zu beschreiben. Container bieten eine hohe Dichte, Microservices eine extrem hohe Skalierbarkeit.

Weitere Informationen zum Microservice-Ansatz finden Sie unter [Gründe für einen Microservice-Ansatz zum Erstellen von Anwendungen](service-fabric-overview-microservices.md)

## <a name="create-service-fabric-clusters-anywhere"></a>Erstellen von Service Fabric-Clustern – überall
Sie können Service Fabric-Cluster in vielen Umgebungen erstellen, z.B. unter Azure oder lokal, unter Windows Server oder unter Linux. Darüber hinaus ist die Entwicklungsumgebung im SDK identisch mit der Produktionsumgebung ohne Beteiligung von Emulatoren. Anders ausgedrückt: Falls das SDK in Ihrem lokalen Entwicklungscluster ausgeführt wird, wird es für den gleichen Cluster in anderen Umgebungen bereitgestellt.

Weitere Informationen zum lokalen Erstellen von Clustern finden Sie unter [Erstellen von eigenständigen Service Fabric-Clustern unter Windows Server oder Linux](service-fabric-deploy-anywhere.md). Informationen zum Erstellen von Azure-Clustern finden Sie unter [Erstellen eines Service Fabric-Clusters in Azure über das Azure-Portal](service-fabric-cluster-creation-via-portal.md).

![Service Fabric-Plattform][Image1]

## <a name="stateless-and-stateful-service-fabric-microservices"></a>Zustandsbehaftete und zustandslose Service Fabric-Microservices

Service Fabric ermöglicht es Ihnen, Anwendungen zu erstellen, die aus Microservices bestehen. Zustandslose Microservices (Protokollgateways, Webproxys und Ähnliches) behalten über die Anforderung und ihre Antwort vom Dienst hinaus keinen veränderbaren Zustand bei. Ein zustandsloser Dienst ist zum Beispiel die Workerrolle in Azure Cloud Services. Zustandsbehaftete Microservices (Benutzerkonten, Datenbanken, Geräte, Einkaufswagen, Warteschlangen und Ähnliches) behalten einen veränderbaren, autorisierenden Zustand über die Anforderung und ihre Antwort hinaus bei. Heutige Internetanwendungen bestehen aus einer Kombination aus zustandslosen und zustandsbehafteten Microservices.

Warum sollte man sowohl zustandsbehaftete als auch zustandslose Microservices haben? Die beiden wichtigsten Gründe sind:

1. Die Möglichkeit zum Erstellen fehlertoleranter OLTP-Dienste (Online Transaction Processing) mit hohem Durchsatz und niedriger Latenz, indem Code und Daten nah beieinander auf dem gleichen Computer vorgehalten werden. Beispiele hierfür sind interaktive Geschäfte, Suchfunktionen, IoT-Systeme (Internet der Dinge), Handelssysteme, Kreditkartenverarbeitungssysteme, Betrugserkennungssysteme und Verwaltung personenbezogener Datensätze.

2. Vereinfachung des Anwendungsentwurfs: Mit zustandsbehafteten Microservices werden zusätzliche Warteschlangen und Caches überflüssig, die bislang erforderlich waren, um Verfügbarkeits- und Latenzanforderungen einer rein zustandslosen Anwendung zu erfüllen. Zustandsbehaftete Dienste sind normalerweise bereits hochverfügbar und bieten eine niedrige Latenz, sodass der Verwaltungsaufwand in Ihrer Anwendung insgesamt geringer ausfällt.

Weitere Informationen zu Anwendungsmustern mit Service Fabric finden Sie unter [Service Fabric-Anwendungsszenarien](service-fabric-application-scenarios.md) und [Auswählen eines Frameworks für den Dienst](service-fabric-choose-framework.md).

## <a name="application-lifecycle-management"></a>Anwendungslebenszyklusverwaltung
Service Fabric bietet erstklassige Unterstützung für die gesamte Anwendungslebenszyklusverwaltung (Application Lifecycle Management, ALM) von Cloudanwendungen: von der Entwicklung über die Bereitstellung, die tägliche Verwaltung und die Wartung bis hin zur endgültigen Außerbetriebnahme.

Dank der ALM-Funktionen von Service Fabric profitieren Anwendungsadministratoren/IT-Operatoren von einfache Workflows, um Anwendungen mit geringem Arbeitsaufwand vorzubereiten, bereitzustellen, zu patchen und zu überwachen. Diese integrierten Workflows entlasten IT-Operatoren bei der Gewährleistung der kontinuierlichen Anwendungsverfügbarkeit erheblich.

Die meisten Anwendungen bestehen aus einer Kombination aus zustandslosen und zustandsbehafteten Microservices und anderen ausführbaren Dateien/Laufzeiten, die gemeinsam bereitgestellt werden. Dank leistungsstarker Typen für Anwendungen und gepackter Microservices ermöglicht Service Fabric die Bereitstellung mehrerer Anwendungsinstanzen. Jede Instanz kann jeweils unabhängig voneinander verwaltet und aktualisiert werden. Maßgeblich dabei ist, dass Service Fabric *jede beliebige* ausführbare Datei oder Laufzeit zuverlässig bereitstellen kann. Mit Service Fabric können beispielsweise ASP.NET Core 1, Node.js, Java-VMs, Skripts und alle anderen beliebigen Komponenten bereitgestellt werden, aus denen sich Ihre Anwendung zusammensetzt.

Weitere Informationen zur Verwaltung des Anwendungslebenszyklus finden Sie unter [Lebenszyklus der Service Fabric-Anwendung](service-fabric-application-lifecycle.md) und zum Bereitstellen eines beliebigen Codes unter [Bereitstellen einer ausführbaren Gastanwendungsdatei in Service Fabric](service-fabric-deploy-existing-app.md).

## <a name="key-capabilities"></a>Wichtige Funktionen
Mit Service Fabric können Sie Folgendes tun:

- Hochgradig skalierbare Anwendungen mit Selbstreparatur entwickeln.

- Anwendungen entwickeln, die aus Microservices bestehen, indem Sie das Service Fabric-Programmiermodell verwenden. Oder hosten Sie einfach ausführbare Gastanwendungsdateien und andere Anwendungsframeworks Ihrer Wahl, z.B. ASP.NET Core 1 oder Node.js.

- Sehr zuverlässige zustandslose und zustandsbehaftete Microservices entwickeln.

- Den Anwendungsentwurf durch die Nutzung zustandsbehafteter Microservices anstelle von Caches und Warteschlangen vereinfachen.

- Anwendungen ohne Codeänderungen in Azure oder lokalen Clouds mit Windows Server oder Linux bereitstellen. Anwendungen einmal schreiben und an beliebiger Stelle in einem beliebigen Service Fabric-Cluster bereitstellen.

- Anwendungen nach dem Modell „Rechenzentrum auf dem eigenen Computer“ entwickeln: Der Code für die lokale Entwicklungsumgebung ist der gleiche Code, der auch in den Azure-Rechenzentren verwendet wird.

- Anwendungen in Sekunden bereitstellen.

- Anwendungen mit höherer Dichte als bei Verwendung von virtuellen Computern bereitstellen; es können Hunderte oder Tausende von Anwendungen pro Computer bereitgestellt werden.

- Verschiedene Versionen derselben Anwendung gleichzeitig bereitstellen, die jeweils unabhängig voneinander aktualisiert werden können.

- Lebenszyklus zustandsbehafteter Anwendungen ohne Ausfallzeiten verwalten – einschließlich Upgrades mit und ohne Funktionsbeeinträchtigung.

- Anwendungen mit .NET-APIs, PowerShell oder REST-Schnittstellen verwalten.

- Microservices innerhalb der Anwendungen unabhängig voneinander aktualisieren und patchen.

- Integrität der Anwendungen überwachen und diagnostizieren und Richtlinien für automatische Reparaturen festlegen.

- Service Fabric-Cluster auf einfache Weise skalieren und dabei sicher sein, dass die Skalierung der Anwendungen unter Berücksichtigung der verfügbaren Ressourcen erfolgt.

- Den selbstreparierenden Resource Balancer beim Organisieren einer erneuten Bereitstellung der Anwendungen im Cluster überwachen. Service Fabric führt nach Fehlern eine Wiederherstellung durch und optimiert die Lastenverteilung basierend auf den verfügbaren Ressourcen.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen finden Sie unter:
    * [Gründe für einen Microservice-Ansatz zum Erstellen von Anwendungen](service-fabric-overview-microservices.md)
    * [Technische Übersicht](service-fabric-technical-overview.md)
* Einrichten der Service Fabric- [Entwicklungsumgebung](service-fabric-get-started.md)  
* [Auswählen eines Frameworks](service-fabric-choose-framework.md) für Ihren Dienst

[Bild1]: media/service-fabric-overview/Service-Fabric-Overview.png



<!--HONumber=Oct16_HO2-->


