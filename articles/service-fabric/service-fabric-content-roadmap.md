---
title: Informationen zu Azure Service Fabric | Microsoft-Dokumentation
description: "Eine Übersicht und ein Leitfaden zu den ersten Schritten für Azure Service Fabric. Informationen zu Service Fabric und den ersten Schritten, um skalierbare, zuverlässige und einfach zu verwaltende Anwendungen zu entwickeln, die aus Microservices bestehen."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/18/2017
ms.author: ryanwi
translationtype: Human Translation
ms.sourcegitcommit: db7cb109a0131beee9beae4958232e1ec5a1d730
ms.openlocfilehash: 94b697d01d31463d1744d7e8d4f1169a2f366242
ms.lasthandoff: 04/18/2017


---
# <a name="so-you-want-to-learn-about-service-fabric"></a>Sie möchten sich über Service Fabric informieren?
Diese Einführung bietet eine kurze Übersicht über Service Fabric, eine Einführung in die grundlegenden Konzepte und Begriffe, einen Leitfaden zu den ersten Schritten sowie eine Übersicht über die einzelnen Bereiche von Service Fabric.  Diese Einführung enthält keine umfassende Liste der Inhalte, aber Links zu Artikeln zur Übersicht und zu den ersten Schritten für die einzelnen Bereiche von Service Fabric. 

## <a name="the-five-minute-overview"></a>Kurze Übersicht
Azure Service Fabric ist eine Plattform für verteilte Systeme, die das Packen, Bereitstellen und Verwalten skalierbarer und zuverlässiger Microservices vereinfacht.  Service Fabric bietet einfache Lösungen für die komplexen Herausforderungen bei der Entwicklung und Verwaltung von Cloudanwendungen. Mithilfe von Service Fabric können Entwickler und Administratoren vermeiden, komplexe Infrastrukturprobleme zu lösen.  Stattdessen können Sie sich auf die Implementierung unternehmenskritischer, anspruchsvoller Workloads konzentrieren und dabei wissen, dass diese skalierbar, zuverlässig und verwaltbar sind. Service Fabric ist die Middlewareplattform der nächsten Generation für das Erstellen und Verwalten leistungsstarker Tier-1-Anwendungen mit Cloudskalierung.  

In diesem kurzen Channel9-Video werden Service Fabric und Microservices vorgestellt: <center><a target="_blank" href="https://aka.ms/servicefabricvideo">  
<img src="./media/service-fabric-content-roadmap/OverviewVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="the-detailed-overview"></a>Ausführliche Übersicht
Service Fabric ermöglicht das Erstellen und Verwalten skalierbarer und zuverlässiger Anwendungen, die aus Microservices bestehen.  Diese Microservices werden mit sehr hoher Dichte in einem freigegebenen Pool von Computern ausgeführt, der als Cluster bezeichnet wird. Die Plattform bietet eine ausgereifte Laufzeit für das Erstellen verteilter, skalierbarer zustandsloser und zustandsbehafteter Microservices. Außerdem bietet sie umfassende Anwendungsverwaltungsfunktionen zum Organisieren, Bereitstellen, Überwachen, Upgraden/Patchen und Löschen bereitgestellter Anwendungen.  Weitere Informationen finden Sie unter [Übersicht über Service Fabric](service-fabric-overview.md).

Gründe für einen Entwurfsansatz mit Microservices Alle Anwendungen entwickeln sich im Laufe der Zeit weiter. Erfolgreiche Anwendungen entwickeln sich weiter, indem Sie für die Benutzer nützlich sind. Wie viel wissen Sie über Ihre heutigen Anforderungen und deren zukünftige Entwicklung? In einigen Fällen ist der treibende Faktor, dass eine einfache App als Machbarkeitsstudie erstellt werden soll (in dem Wissen, dass der Entwurf der Anwendung zu einem späteren Zeitpunkt geändert werden kann). Wenn Unternehmen von der Erstellung für die Cloud sprechen, werden aber auch Wachstum und gute Auslastung erwartet. Das Problem hierbei ist, dass Wachstums- und Skalierungsanforderungen nicht vorhersagbar sind. Entwickler möchten Prototypen schnell erstellen können und dabei sicher sein, dass die App skaliert werden kann, um auf unvorhersehbare Wachstumsphasen und Auslastungen reagieren zu können.  Unter [Was sind Microservices?](service-fabric-overview-microservices.md) wird beschrieben, wie dies mit dem Microservice-Entwurfsansatz erreicht werden kann und wie Sie Microservices erstellen können, die Sie hoch- oder herunterskalieren, testen, bereitstellen und unabhängig verwalten können.

Service Fabric bietet eine zuverlässige und flexible Plattform, mit der Sie verschiedenste Geschäftsanwendungen und -dienste erstellen und ausführen können.  Sie können auch eine Ihrer vorhandenen Anwendungen ausführen (geschrieben in einer beliebigen Sprache).  Diese Anwendungen und Microservices können zustandslos oder zustandsbehaftet sein, und ihre Ressourcen werden zur Maximierung der Effizienz auf die virtuellen Maschinen verteilt. In der einzigartigen Architektur von Service Fabric können Sie Echtzeitdatenanalysen, In-Memory-Berechnungen, parallele Transaktionen und die Ereignisverarbeitung in Ihren Anwendungen ausführen. Sie können Ihre Anwendungen je nach den sich ändernden Ressourcenanforderungen problemlos zentral (bzw. horizontal oder vertikal) [hoch- oder herunterskalieren](service-fabric-concepts-scalability.md). Lesen Sie [Anwendungsszenarien](service-fabric-application-scenarios.md) und [Muster und Szenarien](service-fabric-patterns-and-scenarios.md), um sich über die Kategorien der Anwendungen und Dienste, die Sie erstellen können, und Fallstudien von Kunden zu informieren.

In diesem etwas längeren Microsoft Virtual Academy-Video werden wichtige Kernkonzepte von Service Fabric erläutert: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965">  
<img src="./media/service-fabric-content-roadmap/CoreConceptsVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="get-started-and-create-your-first-app"></a>Erste Schritte und Erstellen Ihrer ersten App 
Mit den Service Fabric-SDKs und -Tools können Sie Apps in Windows-, Linux- oder MacOS-Umgebungen entwickeln und diese Apps in Clustern bereitstellen, die unter Windows oder Linux ausgeführt werden.  Mit den folgenden Leitfäden können Sie eine App innerhalb von wenigen Minuten bereitstellen.  Laden Sie nach dem Ausführen Ihrer ersten Anwendung einige unserer [Beispiel-Apps](http://aka.ms/servicefabricsamples) herunter, und führen Sie sie aus. Beginnen Sie insbesondere mit den [Beispielen für erste Schritte](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started).

### <a name="on-windows"></a>Unter Windows
Das Service Fabric-SDK enthält ein Add-In für Visual Studio, das Vorlagen und Tools zum Erstellen, Bereitstellen und Debuggen von Service Fabric-Anwendungen bietet. In diesen Themen werden das Erstellen Ihrer ersten Anwendung in Visual Studio und das Ausführen auf dem Entwicklungscomputer ausführlich beschrieben.

[Einrichten Ihrer Entwicklungsumgebung](service-fabric-get-started.md)
[Erstellen Ihrer ersten App (C#)](service-fabric-create-your-first-application-in-visual-studio.md)

#### <a name="practical-hands-on-labs"></a>Praktische Hands on Labs
Arbeiten Sie diese umfassende [praktische Übung, Teil 1](https://msdnshared.blob.core.windows.net/media/2016/07/SF-Lab-Part-I.docx) durch, um sich mit dem End-to-End-Entwicklungsablauf für Service Fabric vertraut zu machen.  Informieren Sie sich, wie Sie einen zustandslosen Dienst erstellen, Überwachungs- und Integritätsberichte konfigurieren und ein Anwendungsupgrade durchführen. Bearbeiten Sie nach Teil 1 die [praktische Übung, Teil 2](http://aka.ms/sflab2), bei der zustandsbehaftete Dienste behandelt werden.

Im folgenden Channel9-Video wird die Erstellung einer C#-App in Visual Studio ausführlich beschrieben:  
<center><a target="_blank" href="https://channel9.msdn.com/Blogs/Azure/Creating-your-first-Service-Fabric-application-in-Visual-Studio">  
<img src="./media/service-fabric-content-roadmap/first-app-vid.png" WIDTH="360" HEIGHT="244">  
</a></center>

### <a name="on-linux"></a>Unter Linux
Service Fabric bietet SDKs, mit denen sich Dienste unter Linux sowohl in .NET Core als auch in Java erstellen lassen. In diesen Themen werden das Erstellen Ihrer ersten Java- oder C#-Anwendung unter Linux und das Ausführen auf dem Entwicklungscomputer ausführlich beschrieben: [Einrichten Ihrer Entwicklungsumgebung](service-fabric-get-started-linux.md), [Ihre erste App erstellen (Java)](service-fabric-create-your-first-linux-application-with-java.md) und [Ihre erste App erstellen (C#)](service-fabric-create-your-first-linux-application-with-csharp.md).

Im folgenden Microsoft Virtual Academy-Video wird die Vorgehensweise zum Erstellen einer Java-App unter Linux veranschaulicht:  
<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=DOX8K86yC_206218965">  
<img src="./media/service-fabric-content-roadmap/LinuxVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

### <a name="on-macos"></a>Unter MacOS
Sie können Service Fabric-Anwendungen unter MacOS X für die Ausführung auf Linux-Clustern erstellen. In diesen Artikeln wird beschrieben, wie Sie Ihren Macintosh für die Entwicklung einrichten, und Sie werden durch die Erstellung Ihrer ersten Java-App unter MacOS und die Ausführung auf einem virtuellen Ubuntu-Computer geführt: [Einrichten Ihrer Entwicklungsumgebung](service-fabric-get-started-mac.md) und [Ihre erste App erstellen (Java)](service-fabric-create-your-first-linux-application-with-java.md).

## <a name="core-concepts"></a>Wichtige Konzepte
Die Artikel [Übersicht über Service Fabric-Terminologie](service-fabric-technical-overview.md), [Modellieren von Anwendungen in Service Fabric](service-fabric-application-model.md) und [Übersicht über die Service Fabric-Programmiermodelle](service-fabric-choose-framework.md) enthalten weitere Konzepte und Beschreibungen, aber hier geht es um die Grundlagen.

<table><tr><th>Wichtige Konzepte</th><th>Entwurfszeit</th><th>Laufzeit</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965">
<img src="./media/service-fabric-content-roadmap/CoreConceptsVid.png" WIDTH="240" HEIGHT="162"></a></td>
<td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tlkI046yC_2906218965"><img src="./media/service-fabric-content-roadmap/RunTimeVid.png" WIDTH="240" HEIGHT="162"></a></td>
<td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=x7CVH56yC_1406218965">
<img src="./media/service-fabric-content-roadmap/RunTimeVid.png" WIDTH="240" HEIGHT="162"></a></td></tr>
</table>

### <a name="design-time-app-type-service-type-app-package-and-manifest-service-package-and-manifest"></a>Entwurfszeit: App-Typ, Diensttyp, App-Paket und -Manifest, Dienstpaket und -manifest
Ein Anwendungstyp ist der Name oder die Version, der bzw. die einer Sammlung von Diensttypen zugewiesen ist. Diese Informationen sind in der Datei „ApplicationManifest.xml“ definiert und in ein Anwendungspaketverzeichnis eingebettet, das dann in den Imagespeicher des Service Fabric-Clusters kopiert wird. Anschließend können Sie aus diesem Anwendungstyp eine benannte Anwendung erstellen, die dann im Cluster ausgeführt wird. 

Eine Diensttyp ist der Name oder die Version, der bzw. die den Code-, Daten- und Konfigurationspaketen eines Diensts zugewiesen wird. Diese Informationen werden in der Datei „ServiceManifest.xml“ definiert, die in ein Dienstpaketverzeichnis eingebettet ist. Auf das Dienstpaketverzeichnis wird dann in der Datei „ApplicationManifest.xml“ eines Anwendungspakets verwiesen. Innerhalb des Clusters kann nach der Erstellung einer benannten Anwendung ein benannter Dienst aus einem der Diensttypen des Anwendungstyps erstellt werden. Ein Diensttyp wird von der dazugehörigen Datei „ServiceManifest.xml“ beschrieben.  Er besteht aus Dienstkonfigurationseinstellungen mit ausführbarem Code, die zur Laufzeit geladen werden, sowie aus statischen Daten, die vom Dienst genutzt werden.

![Service Fabric-Anwendungstypen und Diensttypen][cluster-imagestore-apptypes]

Das Anwendungspaket ist ein Datenträgerverzeichnis mit der Datei „ApplicationManifest.xml“ des Anwendungstyps, in der auf die Dienstpakete für jeden Diensttyp verwiesen wird, aus dem der Anwendungstyp besteht. Ein Anwendungspaket für einen E-Mail-Anwendungstyp kann beispielsweise Verweise auf ein Warteschlangendienstpaket, ein Front-End-Dienstpaket und ein Datenbankdienstpaket enthalten. Die Dateien im Anwendungspaketverzeichnis werden in den Imagespeicher des Service Fabric-Clusters kopiert. 

Ein Dienstpaket ist ein Datenträgerverzeichnis mit der Datei „ServiceManifest.xml“ des Diensttyps, in der auf die Pakete mit dem Code, den statischen Daten und der Konfiguration für den Diensttyp verwiesen wird. Auf die Dateien im Dienstpaketverzeichnis wird in der Datei „ApplicationManifest.xml“ des Anwendungstyps verwiesen. Ein Dienstpaket kann beispielsweise auf den Code, die statischen Daten und die Konfigurationspakete verweisen, die zusammen einen Datenbankdienst bilden.

### <a name="run-time-clusters-and-nodes-named-apps-named-services-partitions-and-replicas"></a>Laufzeit: Cluster und Knoten, benannte Apps, benannte Dienste, Partitionen und Replikate
Ein [Service Fabric-Cluster](service-fabric-deploy-anywhere.md) enthält eine per Netzwerk verbundene Gruppe von virtuellen oder physischen Computern, auf denen Ihre Microservices bereitgestellt und verwaltet werden. Cluster können auf Tausende von Computern skaliert werden.

Ein Computer oder ein virtueller Computer, der Teil eines Clusters ist, wird als Knoten bezeichnet. Jeder Knoten erhält einen Knotennamen (Zeichenfolge). Knoten weisen Merkmale wie etwa Platzierungseigenschaften auf. Jeder Computer bzw. virtuelle Computer verfügt über einen automatisch gestarteten Windows-Dienst (FabricHost.exe), für den die Ausführung beim Hochfahren gestartet wird und mit dem dann zwei ausführbare Dateien gestartet werden: „Fabric.exe“ und „FabricGateway.exe“. Diese zwei ausführbaren Dateien bilden zusammen den Knoten. Für Entwicklungs- oder Testszenarien können Sie mehrere Knoten auf einem einzelnen Computer oder virtuellen Computer hosten, indem Sie mehrere Instanzen von „Fabric.exe“ und „FabricGateway.exe“ ausführen.

Eine benannte Anwendung ist eine Sammlung von benannten Diensten, die eine bzw. mehrere bestimmte Funktionen ausführen. Ein Dienst führt eine vollständige und eigenständige Funktion aus (er kann unabhängig von anderen Diensten gestartet und ausgeführt werden) und besteht aus Code, Konfiguration und Daten. Nach dem Kopieren eines Anwendungspakets in den Imagespeicher können Sie eine Instanz der Anwendung im Cluster erstellen, indem Sie den Anwendungstyp des Anwendungspakets (unter Verwendung seines Namens/seiner Version) angeben. Jeder Anwendungstypinstanz wird ein URI-Name zugewiesen, der wie folgt aussieht: *fabric:/MyNamedApp*. Innerhalb eines Clusters können Sie mehrere benannte Anwendungen aus einem einzelnen Anwendungstyp erstellen. Sie können auch benannte Applikationen aus verschiedenen Anwendungstypen erstellen. Jede benannte Anwendung wird unabhängig verwaltet und versioniert.

Nach der Erstellung einer benannten Anwendung können Sie eine Instanz eines Diensttyps (benannter Dienst) im Cluster erstellen, indem Sie den Diensttyp (unter Verwendung seines Namens/seiner Version) angeben. Jeder Diensttypinstanz wird ein URI-Name zugewiesen, der dem URI der benannten Anwendung zugeordnet ist. Wenn Sie beispielsweise den benannten Dienst „MyDatabase“ in der benannten Anwendung „MyNamedApp“ erstellen, sieht der URI wie folgt aus: *fabric:/MyNamedApp/MyDatabase*. Innerhalb einer benannten Anwendung können Sie einen oder mehrere benannte Dienste erstellen. Jeder benannte Dienst kann ein eigenes Partitionsschema und eigene Werte für die Instanzen-/Replikatanzahl besitzen. 

Es gibt zwei Arten von Diensten: zustandslos und zustandsbehaftet.  Zustandslose Dienste speichern den persistente Zustand in einem externen Speicherdienst wie Azure Storage, Azure SQL-Datenbank oder Azure DocumentDB. Wenn der Dienst über keinen persistenten Speicher verfügt, verwenden Sie einen zustandslosen Dienst. Bei einem zustandsbehafteten Dienst wird Service Fabric verwendet, um den Dienstzustand über die Programmiermodelle mit Reliable Collections oder Reliable Actors zu verwalten.  

Bei der Erstellung eines benannten Diensts geben Sie ein Partitionsschema an. Dienste mit großen Zustandsdatenmengen teilen die Daten auf verschiedene Partitionen auf.  Jede Partition ist für einen Teil der gesamten Zustands des Diensts verantwortlich, der auf die Knoten des Clusters verteilt wird.  Innerhalb einer Partition besitzen zustandslose benannte Dienste Instanzen, wohingegen zustandsbehaftete benannte Dienste Replikate besitzen. In der Regel verfügen zustandslose benannte Dienste immer nur über eine einzelne Partition, da sie keinen internen Zustand aufweisen. Zustandsbehaftete benannte Dienste behalten ihren Zustand in Replikaten bei, und jede Partition verfügt über eine eigene Replikatgruppe.  Lese- und Schreibvorgänge erfolgen im selben Replikat (primäres Replikat). Änderungen des Zustands aufgrund von Schreibvorgängen werden in mehreren anderen Replikaten (aktive sekundäre Replikate) repliziert.  

Die folgende schematische Darstellung zeigt die Beziehung zwischen Anwendungen und Dienstinstanzen, Partitionen und Replikaten.

![Partitionen und Replikate in einem Dienst][cluster-application-instances]

### <a name="partitioning-scaling-and-availability"></a>Partitionierung, Skalierung und Verfügbarkeit
Die [Partitionierung](service-fabric-concepts-partitioning.md) ist nicht nur auf Service Fabric beschränkt. Eine bekannte Form der Partitionierung ist die Datenpartitionierung bzw. das Sharding. Zustandsbehaftete Dienste mit großen Zustandsdatenmengen teilen die Daten auf verschiedene Partitionen auf.  Jede Partition ist für einen Teil der gesamten Zustands des Diensts verantwortlich. 

Die Replikate der einzelnen Partitionen sind auf die Clusterknoten verteilt, wodurch der Zustand Ihres benannten Diensts [skaliert](service-fabric-concepts-scalability.md) werden kann. Wenn die Daten zunehmen müssen, werden die Partitionen vergrößert, und Service Fabric verteilt die Partitionen neu über die Knoten, um die Hardwareressourcen effektiv zu nutzen. Wenn Sie dem Cluster neue Knoten hinzufügen, verteilt Service Fabric die Partitionsreplikate auf die erhöhte Anzahl von Knoten neu, um sie auszugleichen.  Die Gesamtleistung der Anwendung verbessert sich und Konflikte beim Speicherzugriff werden reduziert.  Wenn die Knoten im Cluster nicht effizient genutzt werden, können Sie die Anzahl der Knoten im Cluster verringern.  Service Fabric gleicht die Partitionsreplikate erneut über die verringerte Anzahl von Knoten aus, um die Hardware der einzelnen Knoten besser zu nutzen.

Innerhalb einer Partition besitzen zustandslose benannte Dienste Instanzen, wohingegen zustandsbehaftete benannte Dienste Replikate besitzen. In der Regel verfügen zustandslose benannte Dienste immer nur über eine einzelne Partition, da sie keinen internen Zustand aufweisen. Die Partitionsinstanzen sorgen für [Verfügbarkeit](service-fabric-availability-services.md).  Wenn eine Instanz ausfällt, werden andere Instanzen weiterhin normal ausgeführt, und Service Fabric erstellt eine neue Instanz. Zustandsbehaftete benannte Dienste behalten ihren Zustand in Replikaten bei, und jede Partition verfügt über eine eigene Replikatgruppe.  Lese- und Schreibvorgänge erfolgen im selben Replikat (primäres Replikat). Änderungen des Zustands aufgrund von Schreibvorgängen werden in mehreren anderen Replikaten (aktive sekundäre Replikate) repliziert.  Fällt ein Replikat aus, erstellt Service Fabric aus den vorhandenen Replikaten ein neues Replikat.

## <a name="supported-programming-models"></a>Unterstützte Programmiermodelle
Service Fabric bietet verschiedene Methoden zum Schreiben und Verwalten von Diensten. Dienste können die Service Fabric-APIs verwenden, um Features und Anwendungsframeworks der Plattform umfassend zu nutzen.  Dienste können ein beliebiges kompiliertes ausführbares Programm sein, das in einer beliebigen Sprache geschrieben und auf einem Service Fabric-Cluster gehostet wird. Weitere Informationen finden Sie unter [Unterstützte Programmiermodelle](service-fabric-choose-framework.md).

### <a name="guest-executables"></a>Ausführbare Gastanwendungsdateien
Bei einer [ausführbaren Gastanwendungsdatei](service-fabric-deploy-existing-app.md) handelt es sich um eine vorhandene, beliebig ausführbare Datei (die in einer beliebigen Sprache geschrieben sein kann), die neben anderen Diensten auf einem Service Fabric-Cluster gehostet wird. Ausführbare Gastanwendungsdateien lassen sich aber nicht direkt in Service Fabric-APIs integrieren.  Ausführbare Gastanwendungsdateien profitieren nicht von allen Features, die die Plattform bietet. Beispiele: benutzerdefinierte Berichterstellung zu Integrität und Auslastung, Registrierung von Dienstendpunkten und zustandsbehaftetes Compute.

### <a name="containers"></a>Container
Standardmäßig werden Dienste von Service Fabric als Prozesse bereitgestellt und aktiviert. Service Fabric kann Dienste auch in [Containerimages](service-fabric-containers-overview.md) bereitstellen. Wichtig ist, dass Sie in derselben Anwendung Dienste in Prozessen und Dienste in Containern mischen können.  Service Fabric unterstützt derzeit die Bereitstellung von Docker-Containern in Linux- und Windows Server-Containern unter Windows Server 2016. Im Service Fabric-Anwendungsmodell stellt ein Container einen Anwendungshost dar, auf dem mehrere Dienstreplikate angeordnet werden.  Sie können vorhandene Anwendungen, zustandslose Dienste oder zustandsbehaftete Dienste in einem Container mit Service Fabric bereitstellen.  

### <a name="reliable-services"></a>Reliable Services
[Reliable Services](service-fabric-reliable-services-introduction.md) ist ein schlankes Framework zum Schreiben von Diensten, die sich in die Service Fabric-Plattform integrieren lassen und von allen Features der Plattform profitieren. Reliable Services können zustandslos sein (ähnlich wie die meisten Dienstplattformen, z. B. Webserver oder Workerrollen in Azure Cloud Services). Der Zustand wird in einer externen Lösung, z. B. Azure DB oder Azure Table Storage, gespeichert. Reliable Services können auch zustandsbehaftet sein, wobei der Zustand mithilfe von Reliable Collections direkt im Dienst gespeichert wird. Der Status wird durch Replikation als [hochverfügbar](service-fabric-availability-services.md) festgelegt und durch [Partitionierung](service-fabric-concepts-partitioning.md) verteilt. Die Verwaltung erfolgt automatisch durch Service Fabric.

### <a name="reliable-actors"></a>Reliable Actors
Das [Reliable Actor](service-fabric-reliable-actors-introduction.md)-Anwendungsframework setzt auf Reliable Services auf und implementiert das „Virtual Actor“-Muster basierend auf dem Entwurfsmuster für Akteure. Das Reliable Actor-Framework verwendet unabhängige Compute- und Statuseinheiten mit Singlethreadausführung, die als Akteure bezeichnet werden. Das Reliable Actor-Framework bietet integrierte Kommunikation für Akteure sowie voreingestellte Statuspersistenz und horizontal hochskalierbare Konfigurationen.

## <a name="app-lifecycle"></a>App-Lebenszyklus
Ähnlich wie auf anderen Plattformen durchläuft eine Anwendung auf Service Fabric normalerweise die folgenden Phasen: Entwurf, Entwicklung, Test, Bereitstellung, Upgrade, Wartung und Deinstallation. Service Fabric bietet erstklassige Unterstützung für den gesamten Anwendungslebenszyklus von Cloudanwendungen: von der Entwicklung über die Bereitstellung, die tägliche Verwaltung und die Wartung bis zur endgültigen Außerbetriebnahme. Das Dienstmodell ermöglicht die unabhängige Beteiligung verschiedener Rollen am Anwendungslebenszyklus. [Service Fabric-Anwendungslebenszyklus](service-fabric-application-lifecycle.md) bietet eine Übersicht über die APIs und wie sie von den verschiedenen Rollen während der Phasen des Service Fabric-Anwendungslebenszyklus verwendet werden.  

Der gesamte App-Lebenszyklus kann mithilfe von [PowerShell-Cmdlets](/powershell/module/ServiceFabric/), [C#-APIs](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient), [Java-APIs](/java/api/system.fabric._application_management_client) und [REST-APIs](/rest/api/servicefabric/) verwaltet werden. Sie können auch kontinuierliche Integrations-/kontinuierliche Bereitstellungspipelines mit Tools wie [Visual Studio Team Services](service-fabric-set-up-continuous-integration.md) oder [Jenkins](service-fabric-cicd-your-linux-java-application-with-jenkins.md) einrichten.

Das folgende Microsoft Virtual Academy-Video beschreibt die Verwaltung des Anwendungslebenszyklus: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=My3Ka56yC_6106218965">
<img src="./media/service-fabric-content-roadmap/AppLifecycleVid.png" WIDTH="360" HEIGHT="244">
</a></center>

## <a name="test-apps-and-services"></a>Testen von Apps und Diensten
Zum Erstellen echter Dienste für die Cloud müssen Sie unbedingt sicherstellen, dass Ihre Apps und Dienste für die in der Praxis auftretenden Fehler gerüstet sind.  Der Fault Analysis Service ist auf das Testen von Diensten ausgelegt, die in Service Fabric erstellt werden. Mit dem [Fault Analysis Service](service-fabric-testability-overview.md) können Sie aussagekräftige Fehler auslösen und vollständige Testszenarien für Ihre Anwendungen ausführen. Mit diesen Fehlern und Szenarien werden die verschiedenen Zustände und Übergänge durchgespielt und überprüft, die während der Lebensdauer eines Diensts auftreten – und zwar auf kontrollierte, sichere und einheitliche Weise.

[Aktionen](service-fabric-testability-actions.md) sind auf einen Dienst zum Testen ausgerichtet, bei dem individuelle Fehler verwendet werden. Ein Dienstentwickler kann sie als Bausteine zum Schreiben komplizierter Szenarien verwenden. Beispiele für simulierte Fehler sind:

* Starten Sie einen Knoten neu, um eine beliebige Anzahl von Situationen zu simulieren, in denen ein Computer oder ein virtueller Computer neu gestartet wird.
* Verschieben Sie ein Replikat Ihres zustandsbehafteten Diensts, um Lastenausgleich, Failover oder ein Anwendungsupgrade zu simulieren.
* Lösen Sie einen Quorumverlust für einen zustandsbehafteten Dienst aus, um zu bewirken, dass Schreibvorgänge nicht fortgesetzt werden können, weil nicht genügend Backupreplikate oder sekundäre Replikate zum Aufnehmen neuer Daten vorhanden sind.
* Lösen Sie einen Datenverlust für einen zustandsbehafteten Dienst aus, um eine Situation zu schaffen, in der alle In-Memory-Zustände vollständig entfernt werden.

[Szenarien](service-fabric-testability-scenarios.md) sind komplexe Vorgänge, die aus einem oder mehreren Vorgängen bestehen. Der Fault Analysis Services stellt zwei integrierte, vollständige Szenarien bereit:

* Mithilfe von [Chaosszenarien](service-fabric-controlled-chaos.md) werden im Cluster über längere Zeiträume hinweg fortlaufende verschachtelte Fehler simuliert (sowohl ordnungsgemäß als auch nicht ordnungsgemäß).
* Das [Failovertestszenario](service-fabric-testability-scenarios.md#failover-test) ist eine Version des Chaostestszenarios für eine bestimmte Dienstpartition, während andere Dienste nicht betroffen sind.

## <a name="clusters"></a>Cluster
Ein [Service Fabric-Cluster](service-fabric-deploy-anywhere.md) enthält eine per Netzwerk verbundene Gruppe von virtuellen oder physischen Computern, auf denen Ihre Microservices bereitgestellt und verwaltet werden. Cluster können auf Tausende von Computern skaliert werden. Ein Computer oder ein virtueller Computer, der Teil eines Clusters ist, wird als Clusterknoten bezeichnet. Jeder Knoten erhält einen Knotennamen (Zeichenfolge). Knoten weisen Merkmale wie etwa Platzierungseigenschaften auf. Jeder Computer bzw. virtuelle Computer verfügt über einen automatisch gestarteten Dienst (FabricHost.exe), für den die Ausführung beim Hochfahren gestartet wird und mit dem dann zwei ausführbare Dateien gestartet werden: „Fabric.exe“ und „FabricGateway.exe“. Diese zwei ausführbaren Dateien bilden zusammen den Knoten. Für Testszenarien können Sie mehrere Knoten auf einem einzelnen Computer oder virtuellen Computer hosten, indem Sie mehrere Instanzen von „Fabric.exe“ und „FabricGateway.exe“ ausführen.

Service Fabric-Cluster können auf beliebigen virtuellen oder physischen Computern erstellt werden, auf denen Windows Server oder Linux ausgeführt wird. Es lassen sich Service Fabric-Anwendungen in jeder lokalen oder cloudbasierten Umgebung oder in einer Microsoft Azure-Umgebung bereitstellen und ausführen, in der sich miteinander verbundene Computer mit Windows Server oder Linux befinden.

In dem folgenden Microsoft Virtual Academy-Video werden Service Fabric-Konzepte erläutert: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965">
<img src="./media/service-fabric-content-roadmap/ClusterOverview.png" WIDTH="360" HEIGHT="244">
</a></center>

### <a name="clusters-on-azure"></a>Cluster in Azure
Das Ausführen von Service Fabric-Clustern unter Azure ermöglicht die Integration anderer Azure-Features und -Dienste, die den Betrieb und die Verwaltung des Clusters vereinfachen und zuverlässiger machen.  Ein Cluster ist eine Azure Resource Manager-Ressource, daher können Sie Cluster wie alle anderen Ressourcen in Azure modellieren. Resource Manager ermöglicht auch eine einfache Verwaltung aller Ressourcen, die vom Cluster als einzelne Einheit verwendet werden.  Cluster unter Azure werden mit Azure-Diagnose und Log Analytics integriert.  Clusterknotentypen sind [Skalierungsgruppen für virtuelle Computer](/azure/virtual-machine-scale-sets/index), daher sind automatische Skalierungsfunktionen bereits integriert.

Sie können einen Cluster über das [Azure-Portal](service-fabric-cluster-creation-via-portal.md), mithilfe einer [Vorlage](service-fabric-cluster-creation-via-arm.md) oder mit [Visual Studio](service-fabric-cluster-creation-via-visual-studio.md) in Azure erstellen.

Die Vorschauversion von Service Fabric unter Linux ermöglicht Ihnen, hoch verfügbare und skalierbare Anwendungen unter Linux (genauso wie unter Windows) zu erstellen, bereitzustellen und zu verwalten. Neben C# (.NET Core) stehen Service Fabric-Frameworks (Reliable Services und Reliable Actors) in Java unter Linux zur Verfügung.  Sie können auch [ausführbare Gastdienste](service-fabric-deploy-existing-app.md) mit jeder beliebigen Sprache und jedem beliebigen Framework erstellen. Die Vorschau unterstützt darüber hinaus das Orchestrieren von Docker-Containern. Docker-Container können ausführbare Gastanwendungsdateien oder native Service Fabric-Dienste ausführen, die die Service Fabric-Frameworks nutzen. Weitere Informationen finden Sie unter [Service Fabric unter Linux](service-fabric-linux-overview.md).

Da es sich bei Service Fabric unter Linux um eine Vorschauversion handelt, werden einige Features zwar unter Windows unterstützt, aber nicht unter Linux. Weitere Informationen finden Sie unter [Unterschiede zwischen Service Fabric unter Linux und Windows](service-fabric-linux-windows-differences.md).

### <a name="standalone-clusters"></a>Eigenständige Cluster
Service Fabric verfügt über ein Installationspaket, mit dem Sie eigenständige Service Fabric-Cluster lokal oder bei einem Cloudanbieter erstellen können.  Eigenständige Cluster bieten Ihnen die Möglichkeit zum Hosten von Clustern an beliebigen Orten.  Wenn Ihre Daten der Konformität oder gesetzlichen Einschränkungen unterliegen oder Sie Ihre Daten lokal speichern möchten, können Sie Ihre eigenen Cluster und Apps hosten.  Service Fabric-Apps können ohne Änderungen in mehreren Hostumgebungen ausgeführt werden. Daher werden Ihre Kenntnisse über das Erstellen von Apps von einer Hostumgebung in die andere übernommen. 

[Erstellen Ihres ersten eigenständigen Service Fabric-Clusters](service-fabric-get-started-standalone-cluster.md)

Eigenständige Linux-Clustern werden noch nicht unterstützt.

### <a name="cluster-security"></a>Clustersicherheit
Cluster müssen immer gesichert werden, um zu verhindern, dass nicht autorisierte Benutzer eine Verbindung mit dem Cluster herstellen können, insbesondere dann, wenn im Cluster Produktionsworkloads ausgeführt werden. Es ist zwar möglich, einen nicht gesicherten Cluster zu erstellen, allerdings kann in diesem Fall jeder anonyme Benutzer eine Verbindung herstellen, wenn der Cluster Verwaltungsendpunkte im öffentlichen Internet verfügbar macht.  Es ist nicht möglich, die Sicherheit auf einem ungesicherten Cluster später zu aktivieren: Clustersicherheit wird zum Zeitpunkt der Erstellung des Clusters aktiviert.

Die Szenarien für die Clustersicherheit sind:
* Knoten-zu-Knoten-Sicherheit
* Client-zu-Knoten-Sicherheit
* Rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)

Weitere Informationen hierzu finden Sie unter [Schützen eines Clusters](service-fabric-cluster-security.md).

### <a name="scaling"></a>Skalieren
Wenn Sie dem Cluster neue Knoten hinzufügen, verteilt Service Fabric die Partitionsreplikate und Instanzen auf die erhöhte Anzahl von Knoten neu, um sie auszugleichen.  Die Gesamtleistung der Anwendung verbessert sich und Konflikte beim Speicherzugriff werden reduziert.  Wenn die Knoten im Cluster nicht effizient genutzt werden, können Sie die Anzahl der Knoten im Cluster verringern.  Service Fabric gleicht die Partitionsreplikate und Instanzen erneut über die verringerte Anzahl von Knoten aus, um die Hardware der einzelnen Knoten besser zu nutzen.  Sie können Cluster in Azure entweder [manuell](service-fabric-cluster-scale-up-down.md) oder [programmgesteuert](service-fabric-cluster-programmatic-scaling.md) skalieren.  Eigenständige Cluster können [manuell](service-fabric-cluster-windows-server-add-remove-nodes.md) skaliert werden.

### <a name="cluster-upgrades"></a>Clusterupgrades
Es werden Regelmäßig neue Versionen der Service Fabric-Runtime veröffentlicht.  Führen Sie Runtime-Upgrades (oder Fabric-Upgrades) auf Ihrem Cluster aus, damit Sie immer eine [unterstützte Version](service-fabric-support.md) ausführen.  Zusätzlich zu Fabric-Upgrades können Sie auch die Clusterkonfiguration aktualisieren, z. B. Zertifikate oder Anwendungsports.

Ein Service Fabric-Cluster ist eine Ressource, die Sie besitzen, die jedoch teilweise von Microsoft verwaltet wird.  Microsoft ist für das Patchen des zugrunde liegenden Betriebssystems und das Durchführen von Fabric-Upgrades auf dem Cluster zuständig.  Sie können Ihren Cluster so konfigurieren, dass er Fabric-Upgrades entweder automatisch erhält oder wenn Microsoft eine neue Version veröffentlicht. Alternativ können Sie eine gewünschte unterstützte Fabric-Version auswählen.  Fabric- und Konfigurationsupgrades können über das Azure-Portal oder über Resource Manager festgelegt werden.  Weitere Informationen finden Sie unter [Upgrade von Service Fabric-Clustern](service-fabric-cluster-upgrade.md).  

Ein eigenständiger Cluster ist eine Ressource, die Sie vollständig besitzen.  Sie sind für das Patchen des zugrunde liegenden Betriebssystems und das Starten von Fabric-Upgrades zuständig.  Wenn Ihr Cluster eine Verbindung mit [https://www.microsoft.com/download](https://www.microsoft.com/download) herstellen kann, können Sie für Ihren Cluster das automatische Herunterladen und Bereitstellen des neuen Service Fabric Runtime-Pakets festlegen.  Sie würden dann das Upgrade starten.  Wenn Ihr Cluster nicht auf [https://www.microsoft.com/download](https://www.microsoft.com/download) zugreifen kann, können Sie das neue Runtimepaket manuell auf einen mit dem Internet verbundenen Computer herunterladen und dann das Upgrade starten.  Weitere Informationen finden Sie unter [Upgrade eines eigenständigen Service Fabric-Clusters](service-fabric-cluster-upgrade-windows-server.md).

## <a name="health-monitoring"></a>Systemüberwachung
Service Fabric bietet ein [Integritätsmodell](service-fabric-health-introduction.md), das fehlerhafte Cluster- und Anwendungsbedingungen auf bestimmten Entitäten kennzeichnet (z. B. Clusterknoten und Dienstreplikate). Das Integritätsmodell verwendet Integritäts-Reporter (Systemkomponenten und Watchdogs). Das Ziel ist die einfache und schnelle Diagnose und Reparatur. Dienstgeneratoren müssen sich im Vorfeld Gedanken zur Integrität und zum [Design der Integritätsberichte](service-fabric-report-health.md#design-health-reporting) machen. Jede Bedingung, die zur Beeinträchtigung der Integrität führen kann, sollte gemeldet werden, insbesondere wenn damit die Ursachen von Problemen ermittelt werden können. Diese Integritätsinformationen können beim Debuggen und Untersuchen Zeit und Arbeit sparen, sobald der Dienst ordnungsgemäß in der Produktionsumgebung ausgeführt wird.

Die Service Fabric-Berichterstatter überwachen Bedingungen, die von Bedeutung sind. Sie erstatten basierend auf ihrer lokalen Anzeige Bericht über diese Bedingungen. Der [Integritätsspeicher](service-fabric-health-introduction.md#health-store) aggregiert die von allen Reportern gesendeten Integritätsdaten, um die globale Integrität der Entitäten zu ermitteln. Das Modell ist umfassend, flexibel und einfach anzuwenden. Die Qualität der Integritätsberichte bestimmt die Genauigkeit der Integritätsanzeige des Clusters. Falsch positive Ergebnisse, die irrtümlicherweise Fehler anzeigen, können Upgrades oder andere Dienste beeinträchtigen, die Integritätsdaten verwenden. Beispiele für solche Dienste sind Reparaturdienste und Alarmmechanismen. Daher sind einige Überlegungen erforderlich, um Bericht bereitzustellen, die wichtige Bedingungen auf bestmögliche Weise erfassen.

Die Berichterstellung kann von folgenden Orten aus erfolgen:
* Vom überwachten Service Fabric-Dienstreplikat oder einer Instanz.
* Von internen Watchdogs, die als Service Fabric-Dienste bereitgestellt werden (etwa ein zustandsloser Service Fabric-Dienst, der Bedingungen überwacht und Berichte ausgibt). Watchdogs können auf allen Knoten bereitgestellt oder dem überwachten Dienst zugeordnet werden.
* Von internen Watchdogs, die auf den Service Fabric-Knoten ausgeführt werden, jedoch nicht als Service Fabric-Dienste implementiert werden.
* Von externen Watchdogs, die die Ressource von außerhalb des Service Fabric-Clusters überwachen (etwa ein Überwachungsdienst wie Gomez).

Für Service Fabric-Komponenten werden für alle Entitäten im Cluster standardmäßig Integritätsberichte erstellt.  Die [Systemintegritätsberichte](service-fabric-understand-and-troubleshoot-with-system-health-reports.md) sorgen für Transparenz in Bezug auf die Cluster- und Anwendungsfunktionen und weisen auf Probleme mit der Integrität hin. Für Anwendungen und Dienste wird mit den Systemintegritätsberichten überprüft, ob Entitäten implementiert sind und sich aus Sicht der Service Fabric-Runtime richtig verhalten. Die Berichte umfassen keine Integritätsüberwachung der Geschäftslogik des Diensts und keine Erkennung von hängenden Prozessen. [Implementieren Sie benutzerdefinierte Integritätsberichte](service-fabric-report-health.md) in Ihre Dienste, um für Ihre Dienstlogik spezifische Integritätsinformationen hinzuzufügen.

Service Fabric bietet mehrere Möglichkeiten, um [Integritätsberichte anzuzeigen](service-fabric-view-entities-aggregated-health.md), die im Integritätsspeicher aggregiert werden:
* [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) oder andere Visualisierungstools/
* Integritätsabfragen (über [PowerShell](/powershell/module/ServiceFabric/), [FabricClient-APIs von C#](/api/system.fabric.fabricclient.healthclient) und [FabricClient-APIs von Java](/java/api/system.fabric._health_client) oder [REST-APIs](/rest/api/servicefabric)).
* Allgemeine Abfragen, die eine Liste mit Entitäten zurückgeben, von denen eine der Eigenschaften die Integrität ist (über PowerShell, die API oder REST).

Im folgende Microsoft Virtual Academy-Video werden das Service Fabric-Integritätsmodell und seine Verwendung beschrieben: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tevZw56yC_1906218965">
<img src="./media/service-fabric-content-roadmap/HealthIntroVid.png" WIDTH="360" HEIGHT="244">
</a></center>

## <a name="next-steps"></a>Nächste Schritte
* Informieren Sie sich, wie Sie einen [Cluster in Azure](service-fabric-cluster-creation-via-portal.md) oder einen [eigenständigen Cluster in Windows](service-fabric-cluster-creation-for-windows-server.md) erstellen.
* Versuchen Sie, einen Dienst zu erstellen, indem Sie das Programmiermodell [Reliable Services](service-fabric-reliable-services-quick-start.md) oder [Reliable Actors](service-fabric-reliable-actors-get-started.md) verwenden.
* Informieren Sie sich über das [Migrieren aus Cloud Services](service-fabric-cloud-services-migration-differences.md).
* Informieren Sie sich über [Lokales Überwachen und Diagnostizieren von Diensten](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). 
* Informieren Sie sich über das [Testen von Apps und Diensten.](service-fabric-testability-overview.md)
* Informieren Sie sich über das [Verwalten und Orchestrieren von Clusterressourcen](service-fabric-cluster-resource-manager-introduction.md).
* Betrachten Sie die verschiedenen [Service Fabric-Beispiele](http://aka.ms/servicefabricsamples).
* Informieren Sie sich über [Service Fabric-Supportoptionen](service-fabric-support.md).
* Lesen Sie Artikel und Ankündigungen im [Teamblog](https://blogs.msdn.microsoft.com/azureservicefabric/).


[cluster-application-instances]: media/service-fabric-content-roadmap/cluster-application-instances.png
[cluster-imagestore-apptypes]: ./media/service-fabric-content-roadmap/cluster-imagestore-apptypes.png

