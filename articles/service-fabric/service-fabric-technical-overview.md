---
title: "Übersicht über Service Fabric-Terminologie | Microsoft Docs"
description: "Eine Terminologieübersicht über Service Fabric. Erläutert wichtige Terminologiekonzepte und Begriffe, die in der weiteren Dokumentation verwendet werden."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: chackdan;subramar
ms.assetid: 3a970679-e19e-43b3-9be8-71773f307c57
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/01/2016
ms.author: ryanwi
translationtype: Human Translation
ms.sourcegitcommit: e0efcabbb876e77e5b23537ce023c30f9fd153cd
ms.openlocfilehash: 884e43e36517c0c894b72ab6f4e8f6aadb1d9bcf


---
# <a name="service-fabric-terminology-overview"></a>Übersicht über Service Fabric-Terminologie
Service Fabric ist eine Plattform für verteilte Systeme, die das Packen, Bereitstellen und Verwalten skalierbarer und zuverlässiger Microservices vereinfacht. In diesem Thema wird die von Service Fabric verwendete Terminologie erläutert, damit Sie die in der Dokumentation verwendeten Begriffe verstehen.

Die in diesem Abschnitt aufgeführten Konzepte werden auch in den folgenden Microsoft Virtual Academy-Videos erörtert: <a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965">Wichtige Konzepte</a>, <a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tlkI046yC_2906218965">Entwurfszeitkonzepte</a> und <a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=x7CVH56yC_1406218965">Laufzeitkonzepte</a>.

## <a name="infrastructure-concepts"></a>Infrastrukturkonzepte
**Cluster:**Per Netzwerk verbundene virtuelle oder physische Computer, auf denen Ihre Microservices bereitgestellt und verwaltet werden.  Cluster können auf Tausende von Computern skaliert werden.

**Knoten:**Ein Computer oder ein virtueller Computer, der Teil eines Clusters ist, wird als Knoten bezeichnet. Jeder Knoten erhält einen Knotennamen (Zeichenfolge). Knoten weisen Merkmale wie etwa Platzierungseigenschaften auf. Jeder Computer oder virtuelle Computer verfügt über einen Windows-Dienst für den automatischen Start (`FabricHost.exe`), der beim Start ausgeführt wird und seinerseits zwei ausführbare Dateien startet: `Fabric.exe` und `FabricGateway.exe`. Diese zwei ausführbaren Dateien bilden zusammen den Knoten. In Testszenarien können Sie mehrere Knoten auf einem einzelnen Computer oder virtuellen Computer hosten, indem Sie mehrere Instanzen von `Fabric.exe` und `FabricGateway.exe` ausführen.

## <a name="application-concepts"></a>Anwendungskonzepte
**Anwendungstyp:**Name und Version, die einer Sammlung von Diensttypen zugewiesen sind. Diese Informationen sind in einer `ApplicationManifest.xml` -Datei definiert und in einem Anwendungspaketverzeichnis eingebettet, das dann in den Imagespeicher des Service Fabric-Clusters kopiert wird. Anschließend können Sie aus diesem Anwendungstyp im Cluster eine benannte Anwendung erstellen.

Weitere Informationen finden Sie im Artikel [Anwendungsmodell](service-fabric-application-model.md) .

**Anwendungspaket:** Ein Datenträgerverzeichnis mit der Datei `ApplicationManifest.xml` des Anwendungstyps. Diese Datei verweist auf die Dienstpakete für jeden Diensttyp, der den Anwendungstyp bildet. Die Dateien im Anwendungspaketverzeichnis werden in den Imagespeicher des Service Fabric-Clusters kopiert. Ein Anwendungspaket für einen E-Mail-Anwendungstyp kann beispielsweise Verweise auf ein Warteschlangendienstpaket, ein Front-End-Dienstpaket und ein Datenbankdienstpaket enthalten.

**Benannte Anwendung:**Nach dem Kopieren eines Anwendungspakets in den Imagespeicher können Sie eine Instanz der Anwendung im Cluster erstellen, indem Sie den Anwendungstyp des Anwendungspakets (unter Verwendung seines Namens/seiner Version) angeben. Jeder Anwendungstypinstanz wird ein URI-Name zugewiesen, der wie folgt aussieht: `"fabric:/MyNamedApp"`. Innerhalb eines Clusters können Sie mehrere benannte Anwendungen aus einem einzelnen Anwendungstyp erstellen. Sie können auch benannte Applikationen aus verschiedenen Anwendungstypen erstellen. Jede benannte Anwendung wird unabhängig verwaltet und versioniert.      

**Diensttyp:** Angaben zu Name und Version, die den Code-, Daten- und Konfigurationspaketen eines Diensts zugewiesen werden. Diese Informationen werden in einer `ServiceManifest.xml`-Datei definiert, die in einem Dienstpaketverzeichnis eingebettet ist. Auf das Dienstpaketverzeichnis wird dann in der Datei `ApplicationManifest.xml` eines Anwendungspakets verwiesen. Innerhalb des Clusters kann nach der Erstellung einer benannten Anwendung ein benannter Dienst aus einem der Diensttypen des Anwendungstyps erstellt werden. Die Datei `ServiceManifest.xml` des Diensttyps beschreibt den Dienst.

Weitere Informationen finden Sie im Artikel [Anwendungsmodell](service-fabric-application-model.md) .

Es gibt zwei Arten von Diensten:

* **Zustandslos:** Verwenden Sie einen zustandslosen Dienst, wenn der persistente Zustand des Diensts in einem externen Speicherdienst wie Azure Storage, Azure SQL-Datenbank oder Azure DocumentDB gespeichert ist. Wenn der Dienst über keinen persistenten Speicher verfügt, verwenden Sie einen zustandslosen Dienst. Ein Beispiel hierfür ist ein Rechnerdienst, in dem Werte an den Dienst übergeben werden und eine Berechnung unter Verwendung dieser Werte ausgeführt und ein Ergebnis zurückgegeben wird.
* **Zustandsbehaftet:** Verwenden Sie einen zustandsbehafteten Dienst, wenn Service Fabric zur Verwaltung des Dienstzustands Programmiermodelle mit Reliable Collections oder Reliable Actors verwenden soll. Geben Sie (für die Skalierbarkeit) an, über wie viele Partitionen Sie Ihren Zustand verteilen möchten, wenn Sie einen benannten Dienst erstellen. Geben Sie auch (für die Zuverlässigkeit) an, wie viele Male Ihr Zustand über Knoten repliziert werden soll. Jeder benannte Dienst verfügt über ein einzelnes primäres Replikat und mehrere sekundäre Replikate. Sie ändern den Zustand Ihres benannten Diensts, indem Sie in das primäre Replikat schreiben. Service Fabric repliziert dann diesen Zustand an alle sekundären Replikate und hält so Ihren Zustand synchron. Service Fabric erkennt automatisch, wenn ein Fehler bei einem primären Replikat auftritt, und stuft ein vorhandenes sekundäres Replikat zum primären Replikat hoch. Service Fabric erstellt dann ein neues sekundäres Replikat.  

**Dienstpaket:** Ein Datenträgerverzeichnis mit der Datei `ServiceManifest.xml` des Diensttyps. Diese Datei verweist auf den Code, die statischen Daten und die Konfigurationspakete für den Diensttyp. Auf die Dateien im Dienstpaketverzeichnis wird in der Datei `ApplicationManifest.xml` des Anwendungstyps verwiesen. Ein Dienstpaket kann beispielsweise auf den Code, die statischen Daten und die Konfigurationspakete verweisen, die zusammen einen Datenbankdienst bilden.

**Benannter Dienst:**Nach der Erstellung einer benannten Anwendung können Sie eine Instanz eines Diensttyps im Cluster erstellen, indem Sie den Diensttyp (unter Verwendung seines Namens/seiner Version) angeben. Jeder Diensttypinstanz wird ein URI-Name zugewiesen, der dem URI der benannten Anwendung zugeordnet ist. Wenn Sie beispielsweise den benannten Dienst MyDatabase in der benannten Anwendung MyNamedApp erstellen, sieht der URI folgendermaßen aus: `"fabric:/MyNamedApp/MyDatabase"`. Innerhalb einer benannten Anwendung können Sie mehrere benannte Dienste erstellen. Jeder benannte Dienst kann ein eigenes Partitionsschema und eigene Werte für die Instanzen-/Replikatanzahl besitzen.

**Codepaket:** Ein Datenträgerverzeichnis mit den ausführbaren Dateien des Diensttyps (in der Regel EXE-/DLL-Dateien). Auf die Dateien im Codepaketverzeichnis wird in der Datei `ServiceManifest.xml` des Diensttyps verwiesen. Wenn ein benannter Dienst erstellt wird, wird das Codepaket auf den bzw. die zur Ausführung des benannten Diensts ausgewählten Knoten kopiert. Dann beginnt die Ausführung des Codes. Es gibt zwei Arten von ausführbaren Dateien für Codepakete:

* **Ausführbare Gastdateien:** Ausführbare Dateien, die ohne Änderungen auf dem Hostbetriebssystem (Windows oder Linux) ausgeführt werden. Das bedeutet, dass diese ausführbaren Dateien nicht mit Service Fabric-Laufzeitdateien verknüpft sind oder auf solche Dateien verweisen und daher keine Service Fabric-Programmiermodelle verwenden. Diese ausführbaren Dateien können manche Service Fabric-Funktionen, z.B. den Naming Service für die Endpunktermittlung, nicht verwenden. Ausführbare Gastdateien können keine spezifischen Auslastungsmetriken für jede Dienstinstanz berichten.
* **Ausführbare Dateien des Diensthosts:**Ausführbare Dateien, die Service Fabric-Programmiermodelle nutzen, indem eine Verknüpfung mit Service Fabric-Laufzeitdateien hergestellt wird und so Service Fabric-Features aktiviert werden. Eine Instanz eines benannten Diensts kann beispielsweise Endpunkte im Service FabricNaming-Dienst registrieren und Lastmetriken melden.      

**Datenpaket:** Ein Datenträgerverzeichnis mit den statischen, schreibgeschützten Datendateien eines Diensttyps (in der Regel Foto-, Audio- und Videodateien). Auf die Dateien im Datenpaketverzeichnis wird in der Datei `ServiceManifest.xml` des Diensttyps verwiesen. Wenn ein benannter Dienst erstellt wird, wird das Datenpaket auf den bzw. die zur Ausführung des benannten Diensts ausgewählten Knoten kopiert.  Die Ausführung des Codes beginnt, und er kann jetzt auf die Datendateien zugreifen.

**Konfigurationspaket:** Ein Datenträgerverzeichnis mit den statischen, schreibgeschützten Konfigurationsdateien eines Diensttyps (in der Regel Textdateien). Auf die Dateien im Konfigurationspaketverzeichnis wird in der Datei `ServiceManifest.xml` des Diensttyps verwiesen. Wenn ein benannter Dienst erstellt wird, werden die Dateien im Konfigurationspaket auf den bzw. die zur Ausführung des benannten Diensts ausgewählten Knoten kopiert. Dann beginnt die Ausführung des Codes, und er kann jetzt auf die Konfigurationsdateien zugreifen.

**Container:** Standardmäßig werden Dienste von Service Fabric als Prozesse bereitgestellt und aktiviert. Service Fabric kann Dienste auch in Containerimages bereitstellen. Container sind eine Virtualisierungstechnologie, bei der das zugrunde liegende Betriebssystem von Anwendungen virtualisiert wird. Eine Anwendung und ihre Laufzeit, Abhängigkeiten und Systembibliotheken werden in einem Container mit privatem Vollzugriff auf die eigene isolierte Containersicht von Betriebssystemkonstrukten ausgeführt. Service Fabric unterstützt Docker-Container in Linux- und Windows Server-Containern.  Weitere Informationen finden Sie unter [Service Fabric und Container](service-fabric-containers-overview.md).

**Partitionsschema:** Bei der Erstellung eines benannten Diensts geben Sie ein Partitionsschema an. Dienste mit großen Zustandsdatenmengen teilen die Daten auf verschiedene Partitionen auf, wodurch sie auf die Knoten des Clusters verteilt werden. Dadurch wird die Skalierung des Zustands des benannten Diensts ermöglicht. Innerhalb einer Partition besitzen zustandslose benannte Dienste Instanzen, wohingegen zustandsbehaftete benannte Dienste Replikate besitzen. In der Regel verfügen zustandslose benannte Dienste immer nur über eine einzelne Partition, da sie keinen internen Zustand aufweisen. Die Partitionsinstanzen sorgen für Verfügbarkeit. Wenn eine Instanz ausfällt, werden andere Instanzen weiterhin normal ausgeführt, und Service Fabric erstellt eine neue Instanz. Zustandsbehaftete benannte Dienste behalten ihren Zustand in Replikaten bei, und jede Partition verfügt über eine eigene Replikatgruppe, in der alle Zustände synchronisiert werden. Fällt ein Replikat aus, erstellt Service Fabric aus den vorhandenen Replikaten ein neues Replikat.

Weitere Informationen finden Sie unter [Partitionieren von Service Fabric Reliable Services](service-fabric-concepts-partitioning.md) .

## <a name="system-services"></a>Systemdienste
In jedem Cluster werden Systemdienste erstellt, die die Plattformfunktionen von Service Fabric bereitstellen.

**Naming Service**: Jeder Service Fabric-Cluster verfügt über einen Naming Service, der Dienstnamen in einen Speicherort im Cluster auflöst. Ähnlich wie bei einem Internet-Domänennamendienst (Domain Name Service, DNS) verwalten Sie die Dienstnamen und Eigenschaften für den Cluster. Mithilfe des Naming Service kommunizieren Clients sicher mit allen Knoten im Cluster, um einen Dienstnamen und seinen Speicherort aufzulösen.  Clients rufen die tatsächliche Computer-IP-Adresse und den Port ab, wo er derzeit ausgeführt wird. Sie können Dienste und Clients entwickeln, die in der Lage sind, den aktuellen Netzwerkstandort aufzulösen, auch wenn Anwendungen innerhalb des Clusters verschoben werden (beispielsweise aufgrund von Fehlern, im Zuge eines Ressourcenausgleichs oder bei einer Größenanpassung des Clusters).

Weitere Informationen zu Client- und Dienstkommunikations-APIs, die mit Naming Service zusammenarbeiten, finden Sie unter [Kommunikation mit Diensten](service-fabric-connect-and-communicate-with-services.md) .

**Imagespeicherdienst**: Jeder Service Fabric-Cluster verfügt über einen Imagespeicherdienst, in dem bereitgestellte Anwendungspakete mit Versionsangabe aufbewahrt werden. Kopieren Sie ein Anwendungspaket in den Imagespeicher, und registrieren Sie anschließend den in diesem Anwendungspaket enthaltenen Anwendungstyp. Erstellen Sie nach der Bereitstellung des Anwendungstyps daraus benannte Anwendungen. Sie können die Registrierung eines Anwendungstyps im Imagespeicherdienst aufheben, wenn alle seine benannten Anwendungen gelöscht wurden.

Weitere Informationen zum Bereitstellen von Anwendungen an den Imagespeicherdienst finden Sie unter [Bereitstellen von Anwendungen](service-fabric-deploy-remove-applications.md) .

## <a name="built-in-programming-models"></a>Integrierte Programmiermodelle
Zum Erstellen von Service Fabric-Diensten stehen .NET Framework-Programmiermodelle zur Verfügung:

**Reliable Services:** Eine API zum Erstellen zustandsloser und zustandsbehafteter Dienste. Zustandsbehaftete Dienste speichern ihren Zustand in Reliable Collections (z. b. in einem Wörter oder einer Warteschlange). Sie können auch verschiedene Kommunikationsstapel verknüpfen, z. B. Web-API und Windows Communication Foundation (WCF).

**Reliable Actors**: Eine API zum Erstellen zustandsloser und zustandsbehafteter Objekte über das Programmiermodell mit virtuellen Actors. Dieses Modell kann bei zahlreichen unabhängigen Berechnungs-/Zustandseinheiten nützlich sein. Da dieses Modell ein rundenbasiertes Threadingmodell verwendet, sollten Sie Code vermeiden, der andere Actors oder Dienste aufruft, weil ein einzelner Actor andere eingehende Anforderungen erst verarbeiten kann, wenn alle ausgehenden Anforderungen abgeschlossen sind.

Weitere Informationen finden Sie im Artikel [Auswählen eines Frameworks für den Dienst](service-fabric-choose-framework.md) .

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Service Fabric:

* [Übersicht über Service Fabric](service-fabric-overview.md)
* [Gründe für einen Microservice-Ansatz zum Erstellen von Anwendungen](service-fabric-overview-microservices.md)
* [Anwendungsszenarien](service-fabric-application-scenarios.md)




<!--HONumber=Dec16_HO2-->


