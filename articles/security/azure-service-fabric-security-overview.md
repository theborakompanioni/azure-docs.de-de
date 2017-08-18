---
title: "Übersicht über die Azure Service Fabric-Sicherheit | Microsoft-Dokumentation"
description: "Dieser Artikel enthält eine Übersicht über die Azure Service Fabric-Sicherheit."
services: security
documentationcenter: na
author: unifycloud
manager: swadhwa
editor: tomsh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/04/2017
ms.author: tomsh
ms.translationtype: HT
ms.sourcegitcommit: 398efef3efd6b47c76967563251613381ee547e9
ms.openlocfilehash: 4cbd2791649c6d2dd005521cedb44c17aa874073
ms.contentlocale: de-de
ms.lasthandoff: 08/11/2017

---
# <a name="azure-service-fabric-security-overview"></a>Übersicht über die Azure Service Fabric-Sicherheit
[Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview) ist eine Plattform für verteilte Systeme, die das Packen, Bereitstellen und Verwalten skalierbarer und zuverlässiger Microservices vereinfacht. Service Fabric bietet einfache Lösungen für die komplexen Herausforderungen bei der Entwicklung und Verwaltung von Cloudanwendungen. Entwickler und Administratoren können komplexe Infrastrukturprobleme vermeiden und sich auf das Implementieren geschäftskritischer, anspruchsvoller Workloads konzentrieren, die skalierbar, zuverlässig und einfach zu verwalten sind.

In diesem Artikel mit der Übersicht über die Azure Service Fabric-Sicherheit werden die folgenden Bereiche behandelt:

-   Sichern Ihres Clusters
-   Überwachung und Diagnose
-   Sichern mit Zertifikaten
-   Rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)
-   Sichern eines Clusters mit Windows-Sicherheit
-   Konfigurieren der Anwendungssicherheit in Service Fabric
-   Sichern der Kommunikation für Dienste über die Azure Service Fabric-Sicherheit

## <a name="securing-your-cluster"></a>Sichern Ihres Clusters
Azure Service Fabric ist ein Orchestrator von Diensten in einem Cluster von Computern. Cluster müssen gesichert werden, um zu verhindern, dass nicht autorisierte Benutzer eine Verbindung mit dem Cluster herstellen können, insbesondere dann, wenn im Cluster Produktionsworkloads ausgeführt werden. Es ist zwar möglich, einen nicht gesicherten Cluster zu erstellen, allerdings kann in diesem Fall jeder anonyme Benutzer eine Verbindung herstellen, wenn der Cluster Verwaltungsendpunkte im öffentlichen Internet verfügbar macht.

Dieser Abschnitt enthält eine Übersicht über die Sicherheitsszenarien für Cluster in Azure oder eigenständige Cluster sowie über die verschiedenen Technologien, mit denen diese Szenarien implementiert werden. Die Szenarien für die Clustersicherheit sind:

-   Knoten-zu-Knoten-Sicherheit
-   Client-zu-Knoten-Sicherheit

### <a name="node-to-node-security"></a>Knoten-zu-Knoten-Sicherheit
Dient zum Schutz der Kommunikation zwischen den (virtuellen) Computern im Cluster. So wird sichergestellt, dass nur Computer, die zum Beitreten zum Cluster berechtigt sind, Anwendungen und Dienste im Cluster hosten können.

Cluster unter Azure oder eigenständige Cluster unter Windows können entweder [Zertifikatsicherheit](https://msdn.microsoft.com/library/ff649801.aspx) oder [Windows-Sicherheit](https://msdn.microsoft.com/library/ff649396.aspx) für Windows Server-Computer verwenden.

**Knoten-zu-Knoten-Zertifikatsicherheit**

Service Fabric verwendet X.509-Serverzertifikate, die Sie als Teil der Knotentypkonfigurationen angeben, wenn Sie einen Cluster erstellen. Ein schneller Überblick darüber, was diese Zertifikate sind und [wie Sie sie abrufen oder erstellen können, wird in diesem Artikel bereitgestellt](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/working-with-certificates).

Die Zertifikatsicherheit wird beim Erstellen des Clusters über das Azure-Portal, über Azure Resource Manager-Vorlagen oder über eine eigenständige JSON-Vorlage konfiguriert. Sie können ein primäres Zertifikat und ein optionales sekundäres Zertifikat für Zertifikatrollover angeben. Die angegebenen primären und sekundären Zertifikate müssen sich von den Administratorclientzertifikaten und den schreibgeschützten Clientzertifikaten unterscheiden, die Sie für [Client-zu-Knoten-Sicherheit](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-security) angeben.

### <a name="client-to-node-security"></a>Client-zu-Knoten-Sicherheit
Die Client-zu-Knoten-Sicherheit wird mit Clientidentitäten konfiguriert. Um die Vertrauensstellung zwischen einem Client und dem Cluster herzustellen, müssen Sie den Cluster so konfigurieren, dass ermittelt werden kann, welche Clientidentitäten vertrauenswürdig sind. Dies kann auf zwei unterschiedliche Arten erfolgen:

-   Geben Sie die Domänengruppenbenutzer an, die eine Verbindung herstellen können.
-   Geben Sie die Domänenknotenbenutzer an, die eine Verbindung herstellen können.

Service Fabric unterstützt zwei unterschiedliche Zugriffssteuerungstypen für Clients, die mit einem Service Fabric-Cluster verbunden sind:

-   Administrator
-   Benutzer

Die Zugriffssteuerung ermöglicht es dem Clusteradministrator, den Zugriff auf bestimmte Arten von Clustervorgängen für verschiedene Gruppen von Benutzern einzuschränken, wodurch die Sicherheit des Clusters erhöht wird. Administratoren haben vollständigen Zugriff auf Verwaltungsfunktionen (einschließlich Lese-/Schreibzugriff). Benutzer haben standardmäßig nur Lesezugriff auf Verwaltungsfunktionen (z. B. Abfragefunktionen) sowie die Möglichkeit, Anwendungen und Dienste aufzulösen.

**Client-zu-Knoten-Zertifikatsicherheit**

Client-zu-Knoten-Zertifikatsicherheit wird beim Erstellen des Clusters über das Azure-Portal, über Azure Resource Manager-Vorlagen oder über eine eigenständige JSON-Vorlage durch Angeben eines Administratorclientzertifikats und/oder eines Benutzerclientzertifikats konfiguriert. Die angegebenen Administrator- und Benutzerclientzertifikate müssen sich von den primären und sekundären Zertifikaten unterscheiden, die Sie für Knoten-zu-Knoten-Sicherheit angeben.

Clients, die unter Verwendung des Administratorzertifikats eine Verbindung mit dem Cluster herstellen, haben uneingeschränkten Zugriff auf die Verwaltungsfunktionen. Clients, die unter Verwendung des schreibgeschützten Benutzerclientzertifikats eine Verbindung mit dem Cluster herstellen, haben nur Lesezugriff auf die Verwaltungsfunktionen. Diese Zertifikate werden also für die rollenbasierte Zugriffssteuerung (Role Based Access Control, RBAC) verwendet.

Bei Verwendung von Azure finden Sie Informationen zum Konfigurieren der Zertifikatsicherheit in einem Cluster unter [Einrichten eines Service Fabric-Clusters mit einer Azure Resource Manager-Vorlage](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) .

**Client-zu-Knoten-Sicherheit von Azure Active Directory (AAD) in Azure**

Unter Azure ausgeführte Cluster können den Zugriff auf die Verwaltungsendpunkte auch mit Azure Active Directory (AAD) schützen. Unter [Einrichten eines Service Fabric-Clusters mit einer Azure Resource Manager-Vorlage](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) erfahren Sie, wie Sie die erforderlichen AAD-Artefakte erstellen, sie während der Clustererstellung auffüllen und danach die Verbindung mit diesen Clustern herstellen.

AAD ermöglicht Organisationen (als Mandanten bezeichnet) die Verwaltung des Benutzerzugriffs auf Anwendungen, die in Anwendungen mit einer webbasierten Anmeldebenutzeroberfläche und Anwendungen mit einer nativen Clientbenutzeroberfläche unterteilt sind.

Service Fabric-Cluster bieten unterschiedliche Einstiegspunkte für ihre Verwaltungsfunktionen. Hierzu zählen etwa der webbasierte Service Fabric Explorer und Visual Studio. Daher erstellen Sie zwei AAD-Anwendungen, um den Zugriff auf den Cluster zu steuern: eine Webanwendung und eine native Anwendung.
Für Azure-Cluster wird die Verwendung der AAD-Sicherheit empfohlen, um Clients und Zertifikate für die Knoten-zu-Knoten-Sicherheit zu authentifizieren.

Für eigenständige Windows Server-Cluster wird die Verwendung der Windows-Sicherheit mit gruppenverwalteten Konten (Group Managed Accounts, GMA) empfohlen, wenn Sie über Windows Server 2012 R2 und Active Directory verfügen. Verwenden Sie andernfalls weiterhin die Windows-Sicherheit mit Windows-Konten.

## <a name="monitoring-and-diagnostics-for-azure-service-fabric"></a>Überwachung und Diagnose für Azure Service Fabric
[Überwachung und Diagnose](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-overview) sind wichtig für Entwicklung und Tests sowie die Bereitstellung von Anwendungen und Diensten in allen Umgebungen. Service Fabric-Lösungen funktionieren am besten, wenn Sie die Überwachung und Diagnose planen und implementieren. So stellen Sie sicher, dass Anwendungen und Dienste in einer lokalen Entwicklungsumgebung oder in der Produktionsumgebung wie erwartet ausgeführt werden.

Im Hinblick auf Sicherheit sind die Hauptziele der Überwachung und Diagnose:

-   Erkennen und Diagnostizieren von Hardware- und Infrastrukturproblemen, die auf ein Sicherheitsereignis zurückzuführen sein können.
-   Erkennen von Software- und App-Problemen, die einen Hinweis auf eine Gefährdung (Indicator of Compromise, IoC) darstellen können.
-   Nachvollziehen des Ressourcenverbrauchs zur Verhinderung unbeabsichtigter Denial-of-Service-Angriffe.

Der allgemeine Workflow für die Überwachung und Diagnose besteht aus drei Schritten:

-   **Ereignisgenerierung:** Dies schließt Ereignisse (Protokolle, Ablaufverfolgungen, benutzerdefinierte Ereignisse) auf Infrastruktur- (Cluster-) und Anwendungs-/Dienstebene ein. Weitere Informationen zu den enthaltenen Instrumentierungen und zum Hinzufügen weiterer Instrumentierungen finden Sie unter [Ereignisse auf Infrastrukturebene](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-infra) und [Ereignisse auf Anwendungsebene](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-app).
-   **Ereignisaggregation:** Die generierten Ereignisse müssen gesammelt und aggregiert werden, bevor sie angezeigt werden können. In der Regel empfehlen wir die Verwendung von [Azure-Diagnose](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-aggregation-wad) (ähnelt mehr der agentbasierten Protokollsammlung) oder [EventFlow](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-aggregation-eventflow) (In-Process-Protokollsammlung).
-   **Analyse:** Die Ereignisse müssen visualisiert und in einem Format verfügbar sein, das die Analyse und Anzeige je nach Bedarf ermöglicht. Für die Analyse und Visualisierung von Überwachungs- und Diagnosedaten sind mehrere hervorragende Plattformen auf dem Markt verfügbar. Wir empfehlen [OMS](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-analysis-oms) und [Application Insights](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-analysis-appinsights) aufgrund ihrer besseren Integration in Service Fabric.

Sie können auch [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview) zum Überwachen von vielen Azure-Ressourcen verwenden, aus denen ein Service Fabric-Cluster erstellt wird.

Ein Watchdog ist ein separater Dienst, der die Integrität und die Auslastung von Diensten überwachen und die Integrität für alle Elemente in der Hierarchie des Integritätsmodells melden kann. So können Fehler verhindert werden, die nicht erkannt werden, wenn nur ein einzelner Dienst betrachtet wird. Watchdogs sind auch ein guter Platz zum Hosten von Code, der Aktionen zur Problembehebung ohne Benutzeraktion ausführt (z.B. Bereinigen von Protokolldateien im Speicher in bestimmten Zeitintervallen). Ein Beispiel für eine Watchdog-Dienstimplementierung finden Sie [hier](https://azure.microsoft.com/resources/samples/service-fabric-watchdog-service/).

## <a name="secure-using-certificates"></a>Sichern mit Zertifikaten
Mit Zertifikaten wird festgelegt, wie Sie die Kommunikation zwischen den unterschiedlichen Knoten Ihres eigenständigen Windows-Clusters schützen und wie Clients, die eine Verbindung mit diesem Cluster herstellen, mit X.509-Zertifikaten authentifiziert werden. So wird sichergestellt, dass nur autorisierte Benutzer auf den Cluster und die bereitgestellten Anwendungen zugreifen und Verwaltungsaufgaben durchführen können. Die Zertifikatsicherheit sollte auf dem Cluster aktiviert werden, wenn der Cluster erstellt wird.

### <a name="x509-certificates-and-service-fabric"></a>X.509-Zertifikate und Service Fabric
Digitale X.509-Zertifikate werden in der Regel verwendet, um Clients und Server zu authentifizieren sowie um Nachrichten zu verschlüsseln und digital zu signieren.

In der folgenden Tabelle sind die Zertifikate aufgeführt, die Sie für die Einrichtung des Clusters benötigen:

|Zertifikatinformationseinstellung |Beschreibung|
|-------------------------------|-----------|
|ClusterCertificate|    Dieses Zertifikat ist erforderlich, um die Kommunikation zwischen den Knoten in einem Cluster zu schützen. Sie können zwei verschiedene Zertifikate verwenden: ein primäres Zertifikat und ein sekundäres Zertifikat für Upgrades.|
|ServerCertificate| Dieses Zertifikat wird dem Client angezeigt, wenn versucht wird, eine Verbindung mit diesem Cluster herzustellen. Sie können zwei verschiedene Serverzertifikate verwenden: ein primäres Zertifikat und ein sekundäres Zertifikat für Upgrades.|
|ClientCertificateThumbprints|  Dies ist eine Gruppe von Zertifikaten, die auf den authentifizierten Clients installiert werden sollen.|
|ClientCertificateCommonNames|  Legen Sie den allgemeinen Namen des ersten Clientzertifikats für „CertificateCommonName“ fest. „CertificateIssuerThumbprint“ ist der Fingerabdruck für den Aussteller dieses Zertifikats.|
|ReverseProxyCertificate|   Hierbei handelt es sich um ein optionales Zertifikat, das zum Schutz des [Reverseproxys](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy) angegeben werden kann.|

Klicken Sie für weitere Informationen zum Sichern von Zertifikaten [hier](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security).

## <a name="role-based-access-control-rbac"></a>Rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)
Zugriffssteuerung ermöglicht es dem Clusteradministrator, den Zugriff auf bestimmte Clustervorgänge für verschiedene Gruppen von Benutzern einzuschränken, wodurch die Sicherheit des Clusters erhöht wird. Für Clients, die eine Clusterverbindung herstellen, werden zwei unterschiedliche Zugriffssteuerungen unterstützt: Administratorrolle und Benutzerrolle.

Administratoren haben vollständigen Zugriff auf Verwaltungsfunktionen (einschließlich Lese-/Schreibzugriff). Benutzer haben standardmäßig nur Lesezugriff auf Verwaltungsfunktionen (z. B. Abfragefunktionen) sowie die Möglichkeit, Anwendungen und Dienste aufzulösen.

Die Administrator- und die Benutzerclientrolle werden zum Zeitpunkt der Clustererstellung angegeben, indem Sie jeweils separate Identitäten (Zertifikate, AAD usw.) bereitstellen. Weitere Informationen zu den Standardeinstellungen der Zugriffssteuerung sowie zum Ändern der Standardeinstellungen finden Sie unter [Rollenbasierte Zugriffssteuerung für Service Fabric-Clients](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-roles).

## <a name="secure-standalone-cluster-using-windows-security"></a>Sichern eines eigenständigen Clusters mit Windows-Sicherheit
Um nicht autorisierten Zugriff auf einen Service Fabric-Cluster zu verhindern, müssen Sie den Cluster schützen. Sicherheit ist besonders wichtig, wenn der Cluster Produktionsworkloads ausführt. Es wird beschrieben, wie Knoten-zu-Knoten- und Client-zu-Knoten-Sicherheit mit Windows-Sicherheit in der Datei „ClusterConfig.JSON“ konfiguriert wird.

**Konfigurieren der Windows-Sicherheit mithilfe von gMSA**

Die Knoten-zu-Knoten-Sicherheit wird durch Festlegen von [ClustergMSAIdentity](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-windows-security) konfiguriert, wenn Service Fabric in einem gMSA ausgeführt werden muss. Um Vertrauensstellungen zwischen Knoten erstellen zu können, müssen die Knoten über das Vorhandensein des jeweils anderen Knotens informiert sein.

Die Client-zu-Knoten-Sicherheit wird mit „ClientIdentities“ konfiguriert. Um die Vertrauensstellung zwischen einem Client und dem Cluster herzustellen, müssen Sie den Cluster so konfigurieren, dass ermittelt werden kann, welche Clientidentitäten vertrauenswürdig sind.

**Konfigurieren der Windows-Sicherheit mithilfe einer Computergruppe**

Die Knoten-zu-Knoten-Sicherheit wird durch Festlegen von „ClusterIdentity“ konfiguriert, wenn Sie eine Computergruppe in einer Active Directory-Domäne verwenden möchten. Weitere Informationen finden Sie unter [Erstellen einer Computergruppe in Active Directory](https://msdn.microsoft.com/library/aa545347).

Client-zu-Knoten-Sicherheit wird mit „ClientIdentities“ konfiguriert. Um die Vertrauensstellung zwischen einem Client und dem Cluster herzustellen, müssen Sie den Cluster so konfigurieren, dass ermittelt werden kann, welche Clientidentitäten vertrauenswürdig sind. Sie können Vertrauensstellungen auf zwei verschiedene Arten erstellen:

-   Geben Sie die Domänengruppenbenutzer an, die eine Verbindung herstellen können.
-   Geben Sie die Domänenknotenbenutzer an, die eine Verbindung herstellen können.

## <a name="configure-application-security-in-service-fabric"></a>Konfigurieren der Anwendungssicherheit in Service Fabric
### <a name="managing-secrets-in-service-fabric-applications"></a>Verwalten von geheimen Daten in Service Fabric-Anwendungen
Diese Methode hilft bei der Verwaltung von Geheimnissen in einer Service Fabric-Anwendung. Geheimnisse beinhalten jegliche Art von vertraulichen Informationen (z.B. Speicherverbindungszeichenfolgen, Kennwörter oder andere Werte, die nicht als Nur-Text verarbeitet werden sollen).

Bei diesem Ansatz wird [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) für die Verwaltung von Schlüsseln und Geheimnissen verwendet. Die Verwendung von Geheimnissen in einer Anwendung ist jedoch unabhängig von der Cloudplattform, sodass Anwendungen in einem Cluster bereitgestellt werden können, der an einem beliebigen Standort gehostet wird. Dieser Vorgang besteht im Wesentlichen aus vier Schritten:

-   Abrufen eines Datenverschlüsselungszertifikats
-   Installieren des Zertifikats in Ihrem Cluster
-   Verschlüsseln von Geheimnissen bei der Bereitstellung einer Anwendung mit dem Zertifikat und Einfügen dieser Geheimnisse in die Konfigurationsdatei „Settings.xml“ des Diensts
-   Lesen der verschlüsselten Werte aus der Datei „Settings.xml“, indem diese mit demselben Verschlüsselungszertifikat entschlüsselt werden

>[!Note]
>Weitere Informationen finden Sie unter [Verwalten von Geheimnissen in Service Fabric-Anwendungen](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management).

### <a name="configure-security-policies-for-your-application"></a>Konfigurieren von Sicherheitsrichtlinien für Ihre Anwendung
Durch Verwenden der Azure Service Fabric-Sicherheit können Sie Anwendungen sichern, die im Cluster mit verschiedenen Benutzerkonten ausgeführt werden. Mit der Service Fabric-Sicherheit werden auch die Ressourcen gesichert, die von Anwendungen zum Zeitpunkt der Bereitstellung in den Benutzerkonten genutzt werden, z.B. Dateien, Verzeichnisse und Zertifikate. So lässt sich erreichen, dass ausgeführte Anwendungen auch in einer gemeinsamen gehosteten Umgebung sicher voneinander abgegrenzt sind.
Dazu müssen folgende Schritte ausgeführt werden:

-   Konfigurieren der Richtlinie für einen Setupeinstiegspunkt für Dienste.
-   Starten von PowerShell-Befehlen über einen Setupeinstiegspunkt.
-   Verwenden der Konsolenumleitung für das lokale Debuggen.
-   Konfigurieren einer Richtlinie für Dienstcodepakete.
-   Zuweisen einer Sicherheitszugriffsrichtlinie für HTTP- und HTTPS-Endpunkte.

## <a name="secure-communication-for-services-in-azure-service-fabric-security"></a>Sichern der Kommunikation für Dienste über die Azure Service Fabric-Sicherheit
Sicherheit ist einer der wichtigsten Aspekte der Kommunikation. Das Reliable Services-Anwendungsframework stellt einige fertige Kommunikationsstapel und Tools bereit, die Sie zum Erhöhen der Sicherheit verwenden können.

-   [Unterstützung der Sicherung eines Diensts bei der Verwendung von Dienstremoting](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-secure-communication).
-   [Unterstützung der Sicherung eines Diensts, wenn Sie einen WCF-basierten Kommunikationsstapel verwenden](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-secure-communication#help-secure-a-service-when-youre-using-a-wcf-based-communication-stack).

## <a name="next-steps"></a>Nächste Schritte
- Konzeptionelle Informationen zur Clustersicherheit finden Sie unter [Erstellen eines Service Fabric-Clusters in Azure mithilfe von Azure Resource Manager](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) und im [Azure-Portal](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-portal).
- Informieren Sie sich über die [Service Fabric-Clustersicherheit](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security).

