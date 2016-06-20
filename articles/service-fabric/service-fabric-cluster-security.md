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
   ms.date="06/01/2016"
   ms.author="chackdan"/>

# Szenarien für die Clustersicherheit in Service Fabric

Ein Service Fabric-Cluster ist eine Ressource, die sich in Ihrem Besitz befindet. Um den unbefugten Zugriff auf die Ressource zu verhindern, müssen Sie sie absichern. Dies gilt besonders, wenn darin Workloads für die Produktion ausgeführt werden. Dieser Artikel enthält eine Übersicht über die Sicherheitszenarien für Cluster unter Azure und Windows Server sowie die verschiedenen Technologien, die zum Implementieren dieser Szenarien verwendet werden. Die Szenarien für die Clustersicherheit sind:

- Knoten-zu-Knoten-Sicherheit
- Client-zu-Knoten-Sicherheit
- Rollenbasierte Zugriffssteuerung (Role Based Access Control, RBAC)

## Knoten-zu-Knoten-Sicherheit
Hierdurch wird die Kommunikation zwischen den virtuellen Computern und den Computern im Cluster gesichert. So wird sichergestellt, dass nur Computer, die zum Beitreten zum Cluster berechtigt sind, Anwendungen und Dienste im Cluster hosten können.

![Diagramm für die Kommunikation zwischen Knoten][Node-to-Node]

Cluster unter Azure oder eigenständige Cluster unter Windows können entweder [Zertifikatssicherheit](https://msdn.microsoft.com/library/ff649801.aspx) oder [Windows-Sicherheit](https://msdn.microsoft.com/library/ff649396.aspx) verwenden. Zertifikatssicherheit wird beim Erstellen des Clusters (entweder über das Azure-Portal oder über ARM-Vorlagen) durch Angeben eines primären Zertifikats und eines optionalen sekundären konfiguriert. Die primären und sekundären Zertifikate, die Sie angeben, müssen sich von den Admin-Client- und schreibgeschützten Clientzertifikaten unterscheiden, die Sie für [Client-zu-Knoten-Sicherheit](#client-to-node-security) angeben. Unter [Secure a Service Fabric cluster on Azure using certificates](service-fabric-secure-azure-cluster-with-certs.md) (Sichern eines Service Fabric-Clusters in Azure mithilfe von Zertifikaten) oder [Einrichten eines Service Fabric-Clusters mit einer Azure-Ressourcen-Manager-Vorlage](service-fabric-cluster-creation-via-arm.md) finden Sie Informationen zum Konfigurieren der Zertifikatssicherheit in einem Cluster, das in Azure ausgeführt wird.

## Client-zu-Knoten-Sicherheit
Hierdurch werden Clients authentifiziert und die Kommunikation zwischen einem Client und einzelnen Knoten im Cluster gesichert. Bei dieser Art von Sicherheit wird die Clientkommunikation authentifiziert und geschützt. So wird sichergestellt, dass nur autorisierte Benutzer auf den Cluster und die im Cluster bereitgestellten Anwendungen zugreifen können. Clients werden entweder über ihre Windows-Sicherheitsanmeldeinformationen oder ihre Zertifikat-Sicherheitsanmeldeinformationen eindeutig identifiziert.

![Diagramm für die Kommunikation zwischen Client und Knoten][Client-to-Node]

Cluster unter Azure oder eigenständige Cluster unter Windows können entweder [Zertifikatssicherheit](https://msdn.microsoft.com/library/ff649801.aspx) oder [Windows-Sicherheit](https://msdn.microsoft.com/library/ff649396.aspx) verwenden. Client-zu-Knoten-Zertifikatssicherheit wird beim Erstellen des Clusters (entweder über das Azure-Portal oder über ARM-Vorlagen) durch Angeben eines Admin-Clientzertifikats und/oder eines schreibgeschützten Clientzertifikats konfiguriert. Das Admin-Client- und schreibgeschützte Clientzertifikat, die Sie angeben, müssen sich von den primären und sekundären Zertifikaten unterscheiden, die Sie für [Knoten-zu-Knoten-Sicherheit](#node-to-node-security) angeben. Clients, die mit dem Admin-Zertifikat oder primären Zertifikat eine Verbindung mit dem Cluster herstellen, haben vollen Zugriff auf die Verwaltungsfunktionen. Clients, die mit dem schreibgeschützten Clientzertifikat eine Verbindung mit dem Cluster herstellen, haben nur Lesezugriff auf die Verwaltungsfunktionen. Unter [Secure a Service Fabric cluster on Azure using certificates](service-fabric-secure-azure-cluster-with-certs.md) (Sichern eines Service Fabric-Clusters in Azure mithilfe von Zertifikaten) oder [Einrichten eines Service Fabric-Clusters mit einer Azure-Ressourcen-Manager-Vorlage](service-fabric-cluster-creation-via-arm.md) finden Sie Informationen zum Konfigurieren der Zertifikatssicherheit in einem Cluster, das in Azure ausgeführt wird.

Service Fabric verwendet X.509-Serverzertifikate, die Sie als Teil der Knotentypkonfigurationen angeben, wenn Sie einen Cluster erstellen. Ein schneller Überblick darüber, was diese Zertifikate sind und wie Sie sie abrufen oder erstellen können, wird am Ende dieses Artikels bereitgestellt.

Unter Azure ausgeführte Cluster können den Zugriff auf die Verwaltungsendpunkte auch mit Azure Active Directory (AAD) schützen. Unter [Erstellen eines Service Fabric-Clusters mit Azure Active Directory für die Clientauthentifizierung](service-fabric-cluster-security-client-auth-with-aad.md) erfahren Sie, wie Sie die erforderlichen AAD-Artefakte erstellen, sie während der Clustererstellung auffüllen und danach die Verbindung mit diesen Clustern herstellen.

## Rollenbasierte Zugriffssteuerung (Role Based Access Control, RBAC)
Zugriffssteuerung ermöglicht es dem Clusteradministrator, den Zugriff auf bestimmte Clustervorgänge für verschiedene Gruppen von Benutzern einzuschränken, wodurch die Sicherheit des Clusters erhöht wird. Für Clients, die die Verbindung mit einem Cluster herstellen, werden zwei unterschiedliche Zugriffssteuerungen unterstützt: Administrator und Benutzer. Administratoren haben vollständigen Zugriff auf Verwaltungsfunktionen (einschließlich Lese-/Schreibzugriff). Benutzer haben standardmäßig nur Lesezugriff auf Verwaltungsfunktionen (z. B. Abfragefunktionen) sowie die Möglichkeit, Anwendungen und Dienste aufzulösen. Sie geben die Rollen Administrator und Benutzerclient zum Zeitpunkt der Clustererstellung an, indem Sie für jede separate Zertifikate bereitstellen. Weitere Informationen über die Standardeinstellungen der Zugriffssteuerung und wie sie geändert werden finden Sie unter [Rollenbasierte Zugriffssteuerung für Service Fabric-Clients](service-fabric-cluster-security-roles.md).


## X.509-Zertifikate und Service Fabric
Digitale X.509-Zertifikate werden in der Regel verwendet, um Clients und Server zu authentifizieren sowie um Nachrichten zu verschlüsseln und digital zu signieren. Weitere Informationen zu diesen Zertifikaten finden Sie unter [Working with certificates](http://msdn.microsoft.com/library/ms731899.aspx) (Arbeiten mit Zertifikaten).

Einige wichtige Punkte sind zu beachten:

- Zertifikate, die in Clustern mit Produktionsworkloads verwendet werden, müssen entweder mit einem korrekt konfigurierten Windows Server-Zertifikatsdienst erstellt oder über eine genehmigte [Zertifizierungsstelle](https://en.wikipedia.org/wiki/Certificate_authority) bezogen werden.
- Verwenden Sie in der Produktion niemals temporäre oder Testzertifikate, die mit Tools wie „MakeCert.exe“ erstellt wurden.
- Sie können ein selbstsigniertes Zertifikat nur für Testcluster verwenden, aber nicht in der Produktion.

### X.509-Serverzertifikate

Serverzertifikate müssen primär einen Server (Knoten) für Clients oder einen Server (Knoten) für einen Server (Knoten) authentifizieren. Eine der ersten Prüfungen bei der Authentifizierung eines Knotens durch einen Client oder Knoten besteht darin, den Wert des allgemeinen Namens im Feld „Antragsteller“ zu prüfen. Dieser allgemeine Name oder einer der alternativen Antragstellernamen der Zertifikate muss in der Liste der zulässigen allgemeinen Namen vorhanden sein.

Im folgenden Artikel erfahren Sie, wie Zertifikate mit alternativen Antragstellernamen (SAN) generiert werden: [How to add a subject alternative name to a secure LDAP certificate](http://support.microsoft.com/kb/931351) (Hinzufügen eines alternativen Antragstellernamens zu einem gesicherten LDAP-Zertifikat).

Das Antragstellerfeld kann mehrere Werte enthalten. Jeder davon ist mit einer Initialisierung versehen, die den Typ des Werts als Präfix angibt. In den meisten Fällen verwendet die Initialisierung „CN“ für den allgemeinen Namen, z. B. „CN = www.contoso.com“. Das Feld für den Antragsteller kann auch leer gelassen werden. Wenn das optionale Feld für den alternativen Antragstellernamen ausgefüllt ist, muss es sowohl den allgemeinen Namen des Zertifikats als auch einen Eintrag pro alternativen Antragstellernamen enthalten. Diese werden als Werte für DNS-Namen eingegeben.

Der Wert des Felds „Beabsichtigte Zwecke“ des Zertifikats muss einen entsprechenden Wert enthalten, z. B. „Serverauthentifizierung“ oder „Clientauthentifizierung“.

### X.509-Clientzertifikate

Clientzertifikate werden in der Regel nicht von einer Drittanbieter-Zertifizierungsstelle ausgestellt. Stattdessen enthält der persönliche Speicher des aktuellen Benutzerspeicherorts in der Regel Clientzertifikate, die von einer Stammzertifizierungsstelle mit dem beabsichtigten Zweck „Clientauthentifizierung“ platziert werden. Der Client kann ein solches Zertifikat verwenden, wenn eine gegenseitige Authentifizierung erforderlich ist.

>[AZURE.NOTE] Für sämtliche Verwaltungsvorgänge für einen Service Fabric-Cluster sind Serverzertifikate erforderlich. Clientzertifikate können für Verwaltungsaufgaben nicht verwendet werden.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->


## Nächste Schritte
Nachdem Sie nun Ihren Cluster eingerichtet haben, erhalten Sie Informationen über Clusterupgrades:

- [Upgrade von Service Fabric-Clustern](service-fabric-cluster-upgrade.md)

Weitere Informationen zur Anwendungssicherheit:

- [RunAs: Ausführen einer Service Fabric-Anwendung mit verschiedenen Sicherheitsberechtigungen](service-fabric-application-runas-security.md)

- [Unterstützung der Kommunikationssicherung für Dienste in Azure Service Fabric](service-fabric-reliable-services-secure-communication.md)

<!--Image references-->
[Node-to-Node]: ./media/service-fabric-cluster-security/node-to-node.png
[Client-to-Node]: ./media/service-fabric-cluster-security/client-to-node.png

<!---HONumber=AcomDC_0608_2016-->