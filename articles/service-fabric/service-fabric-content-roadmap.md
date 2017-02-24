---
title: Informationen zu Azure Service Fabric | Microsoft-Dokumentation
description: "Enthält eine Übersicht und eine Anleitung für die ersten Schritte für Service Fabric und das Konzept der Anwendungen, die sich aus Microservices zusammensetzen, um die Skalierbarkeit und Resilienz sicherzustellen."
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
ms.date: 01/09/2017
ms.author: ryanwi
translationtype: Human Translation
ms.sourcegitcommit: 6c2464b2f4d16f70c2841faf18e2246c8125b60f
ms.openlocfilehash: 9421b8545715def823a4bdafd27c261e159fbbab


---
# <a name="so-you-want-to-learn-about-service-fabric"></a>Sie möchten sich über Service Fabric informieren?
Dieser Schulungsfahrplan hilft Ihnen bei den ersten Schritten, um skalierbare, zuverlässige Anwendungen für Service Fabric zu entwickeln, die sich problemlos verwalten lassen.

## <a name="the-five-minute-overview"></a>Kurze Übersicht
Azure Service Fabric ist eine Plattform für verteilte Systeme, über die skalierbare und zuverlässige Microservices einfach gepackt, bereitgestellt und verwaltet werden können und die Lösungen für die komplexen Herausforderungen bei der Entwicklung und Verwaltung von Cloudanwendungen bietet. Mit Service Fabric gehören komplexe Infrastrukturprobleme der Vergangenheit an. Stattdessen können sich Entwickler und Administratoren auf das Implementieren geschäftskritischer, anspruchsvoller Workloads konzentrieren, die skalierbar, zuverlässig und einfach zu verwalten sind. Service Fabric ist die Middlewareplattform der nächsten Generation für das Erstellen und Verwalten leistungsstarker Tier-1-Anwendungen mit Cloudskalierung.  

In diesem kurzen Channel9-Video werden Service Fabric und Microservices vorgestellt: <center><a target="_blank" href="https://aka.ms/servicefabricvideo">  
<img src="./media/service-fabric-content-roadmap/OverviewVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="the-detailed-overview"></a>Ausführliche Übersicht
Service Fabric ermöglicht das Erstellen und Verwalten skalierbarer und zuverlässiger Anwendungen, die sich aus Microservices zusammensetzen. Diese Microservices werden in einem gemeinsam genutzten Computerpool (als Cluster bezeichnet) mit sehr hoher Dichte ausgeführt. Die Plattform bietet eine ausgereifte Laufzeit für das Erstellen verteilter, skalierbarer zustandsloser und zustandsbehafteter Microservices. Außerdem bietet sie umfassende Anwendungsverwaltungsfunktionen zum Organisieren, Bereitstellen, Überwachen, Upgraden/Patchen und Löschen bereitgestellter Anwendungen.  Weitere Informationen finden Sie unter [Übersicht über Service Fabric](service-fabric-overview.md).

Gründe für einen Entwurfsansatz mit Microservices Alle Anwendungen entwickeln sich im Laufe der Zeit weiter. Erfolgreiche Anwendungen entwickeln sich weiter, indem Sie für die Benutzer nützlich sind. Wie viel wissen Sie über Ihre heutigen Anforderungen und deren zukünftige Entwicklung? In einigen Fällen ist der treibende Faktor, dass eine einfache App als Machbarkeitsstudie erstellt werden soll (in dem Wissen, dass der Entwurf der Anwendung später geändert werden kann). Wenn Unternehmen von der Erstellung für die Cloud sprechen, werden aber auch Wachstum und gute Auslastung erwartet. Das Problem hierbei ist, dass Wachstums- und Skalierungsanforderungen nicht vorhersagbar sind. Prototypen sollen schnell erstellt werden können, und wir möchten sicher sein, dass die App skaliert werden kann, um auf unvorhersehbare Wachstumsphasen und Auslastungen reagieren zu können.  Unter [Was sind Microservices?](service-fabric-overview-microservices.md) wird beschrieben, wie dies mit dem Microservice-Entwurfsansatz erreicht werden kann und wie Sie Microservices erstellen können, die Sie hoch- oder herunterskalieren, testen, bereitstellen und unabhängig verwalten können.

Service Fabric bietet eine zuverlässige und flexible Plattform, mit der Sie verschiedenste Geschäftsanwendungen und -dienste erstellen und ausführen können.  Sie können auch eine Ihrer vorhandenen Anwendungen ausführen (geschrieben in einer beliebigen Sprache).  Diese Anwendungen und Microservices können zustandslos oder zustandsbehaftet sein, und ihre Ressourcen werden zur Maximierung der Effizienz auf die virtuellen Maschinen verteilt. In der einzigartigen Architektur von Service Fabric können Sie Echtzeitdatenanalysen, In-Memory-Berechnungen, parallele Transaktionen und die Ereignisverarbeitung in Ihren Anwendungen ausführen. Sie können Ihre Anwendungen je nach den sich ändernden Ressourcenanforderungen problemlos zentral (bzw. horizontal oder vertikal) hoch- oder herunterskalieren. Lesen Sie [Anwendungsszenarien](service-fabric-application-scenarios.md), um sich über die Kategorien der Anwendungen und Dienste, die Sie erstellen können, und Fallstudien von Kunden zu informieren.

In diesem etwas längeren Microsoft Virtual Academy-Video werden wichtige Kernkonzepte von Service Fabric erläutert: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965">  
<img src="./media/service-fabric-content-roadmap/CoreConceptsVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="get-started-and-create-your-first-app"></a>Erste Schritte und Erstellen Ihrer ersten App 
Mit den Service Fabric-SDKs und -Tools können Sie Apps in Windows-, Linux- oder MacOS-Umgebungen entwickeln und diese Apps in Clustern bereitstellen, die unter Windows oder Linux ausgeführt werden.  Mit den folgenden Leitfäden können Sie eine App innerhalb von wenigen Minuten bereitstellen.  Laden Sie nach dem Ausführen Ihrer ersten Anwendung einige unserer [Beispiel-Apps](http://aka.ms/servicefabricsamples) herunter, und führen Sie sie aus.

### <a name="on-windows"></a>Unter Windows
Das Service Fabric-SDK enthält ein Add-In für Visual Studio, das Vorlagen und Tools zum Erstellen, Bereitstellen und Debuggen von Service Fabric-Anwendungen bietet. In diesen Themen werden das Erstellen Ihrer ersten Anwendung in Visual Studio und das Ausführen auf dem Entwicklungscomputer ausführlich beschrieben.

[Einrichten Ihrer Entwicklungsumgebung](service-fabric-get-started.md)
[Erstellen Ihrer ersten App (C#)](service-fabric-create-your-first-application-in-visual-studio.md)

Arbeiten Sie diese umfassende [praktische Übung](https://msdnshared.blob.core.windows.net/media/2016/07/SF-Lab-Part-I.docx) durch, um sich mit dem End-to-End-Entwicklungsablauf für Service Fabric vertraut zu machen.  Informieren Sie sich, wie Sie einen zustandslosen Dienst erstellen, Überwachungs- und Integritätsberichte konfigurieren und ein Anwendungsupgrade durchführen. 

Im folgenden Channel9-Video wird die Erstellung einer C#-App in Visual Studio ausführlich beschrieben:  
<center><a target="_blank" href="https://channel9.msdn.com/Blogs/Azure/Creating-your-first-Service-Fabric-application-in-Visual-Studio">  
<img src="./media/service-fabric-content-roadmap/first-app-vid.png" WIDTH="360" HEIGHT="244">  
</a></center>

### <a name="on-linux"></a>Unter Linux
Service Fabric bietet SDKs, mit denen sich Dienste unter Linux sowohl in .NET Core als auch in Java erstellen lassen. In diesen Themen werden das Erstellen Ihrer ersten Java- oder C#-Anwendung unter Linux und das Ausführen auf dem Entwicklungscomputer ausführlich beschrieben.
[Einrichten Ihrer Entwicklungsumgebung](service-fabric-get-started-linux.md)
[Erstellen Ihrer ersten App (Java)](service-fabric-create-your-first-linux-application-with-java.md)
[Erstellen Ihrer ersten App (C#)](service-fabric-create-your-first-linux-application-with-csharp.md)

Im folgenden Microsoft Virtual Academy-Video wird die Vorgehensweise zum Erstellen einer Java-App unter Linux veranschaulicht:  
<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=DOX8K86yC_206218965">  
<img src="./media/service-fabric-content-roadmap/LinuxVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

### <a name="on-macos"></a>Unter MacOS
Sie können Service Fabric-Anwendungen unter MacOS X für die Ausführung auf Linux-Clustern erstellen. In diesen Artikeln wird beschrieben, wie Sie Ihren Macintosh für die Entwicklung einrichten, und Sie werden durch die Erstellung Ihrer ersten Java-App unter MacOS und die Ausführung auf einem virtuellen Ubuntu-Computer geführt.
[Einrichten Ihrer Entwicklungsumgebung](service-fabric-get-started-mac.md)
[Erstellen Ihrer ersten App (Java)](service-fabric-create-your-first-linux-application-with-java.md)

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

Eine Diensttyp ist der Name oder die Version, der bzw. die den Code-, Daten- und Konfigurationspaketen eines Diensts zugewiesen wird. Diese Informationen werden in der Datei „ServiceManifest.xml“ definiert, die in ein Dienstpaketverzeichnis eingebettet ist. Auf das Dienstpaketverzeichnis wird dann in der Datei „ApplicationManifest.xml“ eines Anwendungspakets verwiesen. Innerhalb des Clusters kann nach der Erstellung einer benannten Anwendung ein benannter Dienst aus einem der Diensttypen des Anwendungstyps erstellt werden. Ein Diensttyp wird von der dazugehörigen Datei „ServiceManifest.xml“ beschrieben und besteht aus Dienstkonfigurationseinstellungen mit ausführbarem Code, die zur Laufzeit geladen werden, sowie aus statischen Daten, die vom Dienst genutzt werden.

![Service Fabric-Anwendungstypen und Diensttypen][cluster-imagestore-apptypes]

Das Anwendungspaket ist ein Datenträgerverzeichnis mit der Datei „ApplicationManifest.xml“ des Anwendungstyps, in der auf die Dienstpakete für jeden Diensttyp verwiesen wird, aus dem der Anwendungstyp besteht. Ein Anwendungspaket für einen E-Mail-Anwendungstyp kann beispielsweise Verweise auf ein Warteschlangendienstpaket, ein Front-End-Dienstpaket und ein Datenbankdienstpaket enthalten. Die Dateien im Anwendungspaketverzeichnis werden in den Imagespeicher des Service Fabric-Clusters kopiert. 

Ein Dienstpaket ist ein Datenträgerverzeichnis mit der Datei „ServiceManifest.xml“ des Diensttyps, in der auf die Pakete mit dem Code, den statischen Daten und der Konfiguration für den Diensttyp verwiesen wird. Auf die Dateien im Dienstpaketverzeichnis wird in der Datei „ApplicationManifest.xml“ des Anwendungstyps verwiesen. Ein Dienstpaket kann beispielsweise auf den Code, die statischen Daten und die Konfigurationspakete verweisen, die zusammen einen Datenbankdienst bilden.

### <a name="run-time-clusters-and-nodes-named-apps-named-services-partitions-and-replicas"></a>Laufzeit: Cluster und Knoten, benannte Apps, benannte Dienste, Partitionen und Replikate
Ein [Service Fabric-Cluster](service-fabric-deploy-anywhere.md) enthält eine per Netzwerk verbundene Gruppe von virtuellen oder physischen Computern, auf denen Ihre Microservices bereitgestellt und verwaltet werden. Cluster können auf Tausende von Computern skaliert werden.

Ein Computer oder ein virtueller Computer, der Teil eines Clusters ist, wird als Knoten bezeichnet. Jeder Knoten erhält einen Knotennamen (Zeichenfolge). Knoten weisen Merkmale wie etwa Platzierungseigenschaften auf. Jeder Computer bzw. virtuelle Computer verfügt über einen automatisch gestarteten Windows-Dienst (FabricHost.exe), für den die Ausführung beim Hochfahren gestartet wird und mit dem dann zwei ausführbare Dateien gestartet werden: „Fabric.exe“ und „FabricGateway.exe“. Diese zwei ausführbaren Dateien bilden zusammen den Knoten. Für Entwicklungs- oder Testszenarien können Sie mehrere Knoten auf einem einzelnen Computer oder virtuellen Computer hosten, indem Sie mehrere Instanzen von „Fabric.exe“ und „FabricGateway.exe“ ausführen.

Eine benannte Anwendung ist eine Sammlung von benannten Diensten, die eine bzw. mehrere bestimmte Funktionen ausführen. Ein Dienst führt eine vollständige und eigenständige Funktion aus (er kann unabhängig von anderen Diensten gestartet und ausgeführt werden) und besteht aus Code, Konfiguration und Daten. Nach dem Kopieren eines Anwendungspakets in den Imagespeicher können Sie eine Instanz der Anwendung im Cluster erstellen, indem Sie den Anwendungstyp des Anwendungspakets (unter Verwendung seines Namens/seiner Version) angeben. Jeder Anwendungstypinstanz wird ein URI-Name zugewiesen, der wie folgt aussieht: *fabric:/MyNamedApp*. Innerhalb eines Clusters können Sie mehrere benannte Anwendungen aus einem einzelnen Anwendungstyp erstellen. Sie können auch benannte Applikationen aus verschiedenen Anwendungstypen erstellen. Jede benannte Anwendung wird unabhängig verwaltet und versioniert.

Nach der Erstellung einer benannten Anwendung können Sie eine Instanz eines Diensttyps (benannter Dienst) im Cluster erstellen, indem Sie den Diensttyp (unter Verwendung seines Namens/seiner Version) angeben. Jeder Diensttypinstanz wird ein URI-Name zugewiesen, der dem URI der benannten Anwendung zugeordnet ist. Wenn Sie beispielsweise den benannten Dienst „MyDatabase“ in der benannten Anwendung „MyNamedApp“ erstellen, sieht der URI wie folgt aus: *fabric:/MyNamedApp/MyDatabase*. Innerhalb einer benannten Anwendung können Sie einen oder mehrere benannte Dienste erstellen. Jeder benannte Dienst kann ein eigenes Partitionsschema und eigene Werte für die Instanzen-/Replikatanzahl besitzen. 

Es gibt zwei Arten von Diensten: zustandslos und zustandsbehaftet.  Verwenden Sie einen zustandslosen Dienst, wenn der persistente Zustand des Diensts in einem externen Speicherdienst wie Azure Storage, Azure SQL-Datenbank oder Azure DocumentDB gespeichert ist. Wenn der Dienst über keinen persistenten Speicher verfügt, verwenden Sie einen zustandslosen Dienst. Bei einem zustandsbehafteten Dienst wird Service Fabric verwendet, um den Dienstzustand über die Programmiermodelle mit Reliable Collections oder Reliable Actors zu verwalten.  

Bei der Erstellung eines benannten Diensts geben Sie ein Partitionsschema an. Dienste mit großen Zustandsdatenmengen teilen die Daten auf verschiedene Partitionen auf, wodurch sie auf die Knoten des Clusters verteilt werden. Dadurch wird die Skalierung des Zustands des benannten Diensts ermöglicht. Innerhalb einer Partition besitzen zustandslose benannte Dienste Instanzen, wohingegen zustandsbehaftete benannte Dienste Replikate besitzen. In der Regel verfügen zustandslose benannte Dienste immer nur über eine einzelne Partition, da sie keinen internen Zustand aufweisen. Die Partitionsinstanzen sorgen für Verfügbarkeit. Wenn eine Instanz ausfällt, werden andere Instanzen weiterhin normal ausgeführt, und Service Fabric erstellt eine neue Instanz. Zustandsbehaftete benannte Dienste behalten ihren Zustand in Replikaten bei, und jede Partition verfügt über eine eigene Replikatgruppe, in der alle Zustände synchronisiert werden. Fällt ein Replikat aus, erstellt Service Fabric aus den vorhandenen Replikaten ein neues Replikat.

Die folgende schematische Darstellung zeigt die Beziehung zwischen Anwendungen und Dienstinstanzen, Partitionen und Replikaten.

![Partitionen und Replikate in einem Dienst][cluster-application-instances]

## <a name="supported-programming-models"></a>Unterstützte Programmiermodelle
Service Fabric bietet verschiedene Methoden zum Schreiben und Verwalten von Diensten. Ein Dienst kann die Service Fabric-APIs verwenden, um alle Vorteile der Features und Anwendungsframeworks der Plattform zu nutzen. Bei einem Dienst kann es sich aber auch einfach um ein kompiliertes ausführbares Programm handeln, das in einer beliebigen Sprache geschrieben wurde und auf einem Service Fabric-Cluster gehostet wird. Weitere Informationen finden Sie unter [Unterstützte Programmiermodelle](service-fabric-choose-framework.md).

### <a name="guest-executables"></a>Ausführbare Gastanwendungsdateien
Bei einer [ausführbaren Gastanwendungsdatei](service-fabric-deploy-existing-app.md) handelt es sich um eine beliebige ausführbare Datei, die in einer beliebigen Sprache geschrieben sein kann. Sie können also Ihre vorhandenen Anwendungen auf einem Service Fabric-Cluster neben anderen Diensten hosten. Ausführbare Gastanwendungsdateien lassen sich aber nicht direkt in Service Fabric-APIs integrieren, sodass sie nicht von allen Features profitieren, die die Plattform bietet. Beispiele: benutzerdefinierte Berichterstellung zu Integrität und Auslastung, Registrierung von Dienstendpunkten und zustandsbehaftete Berechnung.

### <a name="containers"></a>Container
Standardmäßig werden Dienste von Service Fabric als Prozesse bereitgestellt und aktiviert. Service Fabric kann Dienste auch in [Containerimages](service-fabric-containers-overview.md) bereitstellen. Wichtig ist, dass Sie in derselben Anwendung Dienste in Prozessen und Dienste in Containern mischen können.  Service Fabric unterstützt derzeit die Bereitstellung von Docker-Containern in Linux- und Windows Server-Containern unter Windows Server 2016. Im Service Fabric-Anwendungsmodell stellt ein Container einen Anwendungshost dar, auf dem mehrere Dienstreplikate angeordnet werden.  Sie können vorhandene Anwendungen, zustandslose Dienste oder zustandsbehaftete Dienste in einem Container mit Service Fabric bereitstellen.  

### <a name="reliable-services"></a>Reliable Services
[Reliable Services](service-fabric-reliable-services-introduction.md) ist ein schlankes Framework zum Schreiben von Diensten, die sich in die Service Fabric-Plattform integrieren lassen und von allen Features der Plattform profitieren. Reliable Services können zustandslos sein (ähnlich wie die meisten Dienstplattformen, z.B. Webserver oder Workerrollen in Azure Cloud Services). Der Zustand wird in einer externen Lösung, z.B. Azure DB oder Azure Table Storage, gespeichert. Reliable Services können auch zustandsbehaftet sein, wobei der Zustand mithilfe von Reliable Collections direkt im Dienst gespeichert wird. Der Status wird durch Replikation als „hochverfügbar“ festgelegt und durch Partitionierung verteilt. Die Verwaltung erfolgt automatisch durch Service Fabric.

### <a name="reliable-actors"></a>Reliable Actors
Das [Reliable Actor](service-fabric-reliable-actors-introduction.md)-Anwendungsframework setzt auf Reliable Services auf und implementiert das „Virtual Actor“-Muster basierend auf dem Entwurfsmuster für Akteure. Das Reliable Actor-Framework verwendet unabhängige Compute- und Statuseinheiten mit Singlethreadausführung, die als Akteure bezeichnet werden. Das Reliable Actor-Framework bietet integrierte Kommunikation für Akteure sowie voreingestellte Statuspersistenz und horizontal hochskalierbare Konfigurationen.

## <a name="next-steps"></a>Nächste Schritte
* Informieren Sie sich, wie Sie einen [Cluster in Azure](service-fabric-cluster-creation-via-portal.md) oder einen [eigenständigen Cluster in Windows](service-fabric-cluster-creation-for-windows-server.md) erstellen.
* Versuchen Sie, einen Dienst zu erstellen, indem Sie das Programmiermodell [Reliable Services](service-fabric-reliable-services-quick-start.md) oder [Reliable Actors](service-fabric-reliable-actors-get-started.md) verwenden.
* Informieren Sie sich über den [Anwendungslebenszyklus](service-fabric-application-lifecycle.md).
* Informieren Sie sich über das [Untersuchen der Anwendungs- und Clusterintegrität](service-fabric-health-introduction.md).
* Informieren Sie sich über [Lokales Überwachen und Diagnostizieren von Diensten](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). 
* Informieren Sie sich über [Service Fabric-Supportoptionen](service-fabric-support.md).


[cluster-application-instances]: media/service-fabric-content-roadmap/cluster-application-instances.png
[cluster-imagestore-apptypes]: ./media/service-fabric-content-roadmap/cluster-imagestore-apptypes.png



<!--HONumber=Feb17_HO3-->


