<properties
   pageTitle="Schützen eines Service Fabric-Clusters | Microsoft Azure"
   description="Beschreibt die Sicherheitsszenarien für Service Fabric-Cluster und die verschiedenen Technologien, die zum Implementieren dieser Szenarien verwendet werden."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/10/2016"
   ms.author="chackdan"/>

# Szenarien für die Clustersicherheit in Service Fabric

Ein Service Fabric-Cluster ist eine Ressource, die sich in Ihrem Besitz befindet. Um den unbefugten Zugriff auf die Ressource zu verhindern, müssen Sie sie absichern. Dies gilt besonders, wenn darin Workloads für die Produktion ausgeführt werden. Dieser Artikel enthält eine Übersicht über die Sicherheitsszenarien für Cluster unter Azure oder eigenständige Cluster sowie über die verschiedenen Technologien, mit denen diese Szenarien implementiert werden. Die Szenarien für die Clustersicherheit sind:

- Knoten-zu-Knoten-Sicherheit
- Client-zu-Knoten-Sicherheit
- Rollenbasierte Zugriffssteuerung (Role Based Access Control, RBAC)

## Knoten-zu-Knoten-Sicherheit
Dient zum Schutz der Kommunikation zwischen den (virtuellen) Computern im Cluster. So wird sichergestellt, dass nur Computer, die zum Beitreten zum Cluster berechtigt sind, Anwendungen und Dienste im Cluster hosten können.

![Diagramm für die Kommunikation zwischen Knoten][Node-to-Node]

Cluster unter Azure oder eigenständige Cluster unter Windows können entweder [Zertifikatsicherheit](https://msdn.microsoft.com/library/ff649801.aspx) oder [Windows-Sicherheit](https://msdn.microsoft.com/library/ff649396.aspx) für Windows Server-Computer verwenden.
### Zertifikatsicherheit (Knoten zu Knoten)
Service Fabric verwendet X.509-Serverzertifikate, die Sie als Teil der Knotentypkonfigurationen angeben, wenn Sie einen Cluster erstellen. Ein schneller Überblick darüber, was diese Zertifikate sind und wie Sie sie abrufen oder erstellen können, wird am Ende dieses Artikels bereitgestellt.

Die Zertifikatsicherheit wird beim Erstellen des Clusters über das Azure-Portal, über Azure Resource Manager-Vorlagen oder über eine eigenständige JSON-Vorlage konfiguriert. Sie können ein primäres Zertifikat und ein optionales sekundäres Zertifikat für Zertifikatrollover angeben. Die angegebenen primären und sekundären Zertifikate müssen sich von den Administratorclientzertifikaten und den schreibgeschützten Clientzertifikaten unterscheiden, die Sie für [Client-zu-Knoten-Sicherheit](#client-to-node-security) angeben.

Bei Verwendung von Azure finden Sie Informationen zum Konfigurieren der Zertifikatsicherheit in einem Cluster unter [Schützen eines Service Fabric-Clusters in Azure mit Zertifikaten](service-fabric-secure-azure-cluster-with-certs.md) sowie unter [Einrichten eines Service Fabric-Clusters mit einer Azure Resource Manager-Vorlage](service-fabric-cluster-creation-via-arm.md).

Bei Verwendung eines eigenständigen Clusters unter Windows Server finden Sie weitere Informationen unter [Schützen des eigenständigen Windows-Clusters mit Zertifikaten](service-fabric-windows-cluster-x509-security.md).

### Windows-Sicherheit (Knoten zu Knoten)
Bei Verwendung eines eigenständigen Clusters unter Windows Server finden Sie weitere Informationen unter [Schützen des eigenständigen Windows-Clusters mit Zertifikaten](service-fabric-windows-cluster-windows-security.md).

## Client-zu-Knoten-Sicherheit
Hierdurch werden Clients authentifiziert und die Kommunikation zwischen einem Client und einzelnen Knoten im Cluster gesichert. Bei dieser Art von Sicherheit wird die Clientkommunikation authentifiziert und geschützt. So wird sichergestellt, dass nur autorisierte Benutzer auf den Cluster und die im Cluster bereitgestellten Anwendungen zugreifen können. Clients werden entweder über ihre Windows-Sicherheitsanmeldeinformationen oder ihre Zertifikat-Sicherheitsanmeldeinformationen eindeutig identifiziert.

![Diagramm für die Kommunikation zwischen Client und Knoten][Client-to-Node]

Cluster unter Azure oder eigenständige Cluster unter Windows können entweder [Zertifikatsicherheit](https://msdn.microsoft.com/library/ff649801.aspx) oder [Windows-Sicherheit](https://msdn.microsoft.com/library/ff649396.aspx) verwenden.

### Zertifikatsicherheit (Client zu Knoten)
 Client-zu-Knoten-Zertifikatsicherheit wird beim Erstellen des Clusters über das Azure-Portal, über Azure Resource Manager-Vorlagen oder über eine eigenständige JSON-Vorlage durch Angeben eines Administratorclientzertifikats und/oder eines Benutzerclientzertifikats konfiguriert. Die angegebenen Administrator- und Benutzerclientzertifikate müssen sich von den primären und sekundären Zertifikaten unterscheiden, die Sie für [Knoten-zu-Knoten-Sicherheit](#node-to-node-security) angeben.

Clients, die unter Verwendung des Administratorzertifikats eine Verbindung mit dem Cluster herstellen, haben uneingeschränkten Zugriff auf die Verwaltungsfunktionen. Clients, die unter Verwendung des schreibgeschützten Benutzerclientzertifikats eine Verbindung mit dem Cluster herstellen, haben nur Lesezugriff auf die Verwaltungsfunktionen. Diese Zertifikate werden also für die weiter unten beschriebene rollenbasierte Zugriffssteuerung (Role Based Access Control, RBAC) verwendet.

Um zu erfahren, wie Sie Zertifikatsicherheit in einem Azure-Cluster konfigurieren, lesen Sie [Schützen eines Service Fabric-Clusters in Azure mit Zertifikaten](service-fabric-secure-azure-cluster-with-certs.md) sowie [Einrichten eines Service Fabric-Clusters mit einer Azure Resource Manager-Vorlage](service-fabric-cluster-creation-via-arm.md).

Bei Verwendung eines eigenständigen Clusters unter Windows Server finden Sie weitere Informationen unter [Schützen des eigenständigen Windows-Clusters mit Zertifikaten](service-fabric-windows-cluster-x509-security.md).

### AAD-Sicherheit (Azure Active Directory) in Azure (Client zu Knoten)
Unter Azure ausgeführte Cluster können den Zugriff auf die Verwaltungsendpunkte auch mit Azure Active Directory (AAD) schützen. Unter [Erstellen eines Service Fabric-Clusters mit Azure Active Directory für die Clientauthentifizierung](service-fabric-cluster-security-client-auth-with-aad.md) erfahren Sie, wie Sie die erforderlichen AAD-Artefakte erstellen, sie während der Clustererstellung auffüllen und danach die Verbindung mit diesen Clustern herstellen.

## Sicherheitsempfehlungen
Für Azure-Cluster wird die Verwendung der AAD-Sicherheit empfohlen, um Clients und Zertifikate für die Knoten-zu-Knoten-Sicherheit zu authentifizieren.

Für eigenständige Windows Server-Cluster wird die Verwendung der Windows-Sicherheit mit gruppenverwalteten Konten empfohlen (Group Managed Accounts, GMA), wenn Sie über Windows Server 2012 R2 und Active Directory verfügen. Verwenden Sie andernfalls weiterhin die Windows-Sicherheit mit Windows-Konten.

## Rollenbasierte Zugriffssteuerung (Role Based Access Control, RBAC)
Zugriffssteuerung ermöglicht es dem Clusteradministrator, den Zugriff auf bestimmte Clustervorgänge für verschiedene Gruppen von Benutzern einzuschränken, wodurch die Sicherheit des Clusters erhöht wird. Für Clients, die eine Clusterverbindung herstellen, werden zwei unterschiedliche Zugriffssteuerungen unterstützt: Administratorrolle und Benutzerrolle.

Administratoren haben vollständigen Zugriff auf Verwaltungsfunktionen (einschließlich Lese-/Schreibzugriff). Benutzer haben standardmäßig nur Lesezugriff auf Verwaltungsfunktionen (z. B. Abfragefunktionen) sowie die Möglichkeit, Anwendungen und Dienste aufzulösen.

Die Administrator- und die Benutzerclientrolle werden zum Zeitpunkt der Clustererstellung angegeben, indem Sie jeweils separate Identitäten (Zertifikate, AAD usw.) bereitstellen. Weitere Informationen zu den Standardeinstellungen der Zugriffssteuerung sowie zum Ändern der Standardeinstellungen finden Sie unter [Rollenbasierte Zugriffssteuerung für Service Fabric-Clients](service-fabric-cluster-security-roles.md).


## X.509-Zertifikate und Service Fabric
Digitale X.509-Zertifikate werden in der Regel verwendet, um Clients und Server zu authentifizieren sowie um Nachrichten zu verschlüsseln und digital zu signieren. Weitere Informationen zu diesen Zertifikaten finden Sie unter [Verwenden von Zertifikaten](http://msdn.microsoft.com/library/ms731899.aspx).

Einige wichtige Punkte sind zu beachten:

- Zertifikate, die in Clustern mit Produktionsworkloads verwendet werden, müssen entweder mit einem korrekt konfigurierten Windows Server-Zertifikatsdienst erstellt oder über eine genehmigte [Zertifizierungsstelle](https://en.wikipedia.org/wiki/Certificate_authority) bezogen werden.
- Verwenden Sie in der Produktion niemals temporäre oder Testzertifikate, die mit Tools wie „MakeCert.exe“ erstellt wurden.
- Sie können ein selbstsigniertes Zertifikat nur für Testcluster verwenden, aber nicht in der Produktion.

### X.509-Serverzertifikate

Serverzertifikate müssen primär einen Server (Knoten) für Clients oder einen Server (Knoten) für einen Server (Knoten) authentifizieren. Eine der ersten Prüfungen bei der Authentifizierung eines Knotens durch einen Client oder Knoten besteht darin, den Wert des allgemeinen Namens im Feld „Antragsteller“ zu prüfen. Dieser allgemeine Name oder einer der alternativen Antragstellernamen der Zertifikate muss in der Liste der zulässigen allgemeinen Namen vorhanden sein.

Im folgenden Artikel erfahren Sie, wie Zertifikate mit alternativen Antragstellernamen (Subject Alternative Names, SANs) generiert werden: [How to add a subject alternative name to a secure LDAP certificate](http://support.microsoft.com/kb/931351) (Hinzufügen eines alternativen Antragstellernamens zu einem gesicherten LDAP-Zertifikat).

Das Antragstellerfeld kann mehrere Werte enthalten. Jeder davon ist mit einer Initialisierung versehen, die den Typ des Werts als Präfix angibt. In den meisten Fällen verwendet die Initialisierung „CN“ für den allgemeinen Namen, z. B. „CN = www.contoso.com“. Das Feld für den Antragsteller kann auch leer gelassen werden. Wenn das optionale Feld für den alternativen Antragstellernamen ausgefüllt ist, muss es sowohl den allgemeinen Namen des Zertifikats als auch einen Eintrag pro alternativen Antragstellernamen enthalten. Diese werden als Werte für DNS-Namen eingegeben.

Der Wert des Felds „Beabsichtigte Zwecke“ des Zertifikats muss einen entsprechenden Wert enthalten, z. B. „Serverauthentifizierung“ oder „Clientauthentifizierung“.

### X.509-Clientzertifikate

Clientzertifikate werden in der Regel nicht von einer Drittanbieter-Zertifizierungsstelle ausgestellt. Stattdessen enthält der persönliche Speicher des aktuellen Benutzerspeicherorts in der Regel Clientzertifikate, die von einer Stammzertifizierungsstelle mit dem beabsichtigten Zweck „Clientauthentifizierung“ platziert werden. Der Client kann ein solches Zertifikat verwenden, wenn eine gegenseitige Authentifizierung erforderlich ist.

>[AZURE.NOTE] Für sämtliche Verwaltungsvorgänge für einen Service Fabric-Cluster sind Serverzertifikate erforderlich. Clientzertifikate können für Verwaltungsaufgaben nicht verwendet werden.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->


## Nächste Schritte

Erfahren Sie, wie Sie einen sicheren Cluster einrichten:

- [Schützen eines Service Fabric-Clusters in Azure mit Zertifikaten](service-fabric-secure-azure-cluster-with-certs.md)

Nachdem Sie nun Ihren Cluster eingerichtet haben, erhalten Sie Informationen über Clusterupgrades:

- [Upgrade von Service Fabric-Clustern](service-fabric-cluster-upgrade.md)
- [Hinzufügen oder Entfernen von Zertifikaten für einen Service Fabric-Cluster in Azure](service-fabric-cluster-security-update-certs-azure.md)

Weitere Informationen zur Anwendungssicherheit:

- [RunAs: Ausführen einer Service Fabric-Anwendung mit verschiedenen Sicherheitsberechtigungen](service-fabric-application-runas-security.md)

- [Unterstützung der Kommunikationssicherung für Dienste in Azure Service Fabric](service-fabric-reliable-services-secure-communication.md)

<!--Image references-->
[Node-to-Node]: ./media/service-fabric-cluster-security/node-to-node.png
[Client-to-Node]: ./media/service-fabric-cluster-security/client-to-node.png

<!---HONumber=AcomDC_0817_2016-->