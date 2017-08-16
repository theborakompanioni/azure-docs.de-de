---
title: "Bewährte Methoden für die Azure Service Fabric-Sicherheit | Microsoft-Dokumentation"
description: "Dieser Artikel enthält bewährte Methoden für die Azure Service Fabric-Sicherheit."
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
ms.sourcegitcommit: 0aae2acfbf30a77f57ddfbaabdb17f51b6938fd6
ms.openlocfilehash: 3132e42aac72c0132c7526ac56d80bc5eec269e7
ms.contentlocale: de-de
ms.lasthandoff: 08/09/2017

---
# <a name="azure-service-fabric-security-best-practices"></a>Bewährte Methoden für die Azure Service Fabric-Sicherheit
Die Bereitstellung einer Anwendung in Azure ist schnell, einfach und kostengünstig. Vor der Bereitstellung der Cloudanwendung in der Produktion ist es sinnvoll, eine bewährte Methode für die Bewertung Ihrer Anwendung anhand einer Liste wichtiger und empfohlener bewährter Methoden zu verwenden.

Azure Service Fabric ist eine Plattform für verteilte Systeme, die das Packen, Bereitstellen und Verwalten skalierbarer und zuverlässiger Microservices vereinfacht. Service Fabric bietet außerdem einfache Lösungen für die komplexen Herausforderungen bei der Entwicklung und Verwaltung von Cloudanwendungen. Entwickler und Administratoren können komplexe Infrastrukturprobleme vermeiden und sich auf das Implementieren geschäftskritischer, anspruchsvoller Workloads konzentrieren, die skalierbar, zuverlässig und einfach zu verwalten sind. 

Für jede bewährte Methode wird Folgendes beschrieben:

-   Wobei es bei der bewährten Methode geht
-   Warum Sie die bewährte Methode nutzen sollten
-   Was die Folge sein könnte, wenn Sie die bewährte Methode nicht aktivieren
-   Wie Sie erfahren können, wie Sie die empfohlenen Vorgehensweisen aktivieren

Wir stellen Ihnen derzeit die folgenden bewährten Methoden für die Azure Service Fabric-Sicherheit zur Verfügung:

-   Verwenden einer Azure Resource Manager-Vorlage (ARM) und des Azure PowerShell-Moduls für Service Fabric zum Erstellen sicherer Cluster
-   Verwenden von X.509-Zertifikaten
-   Konfigurieren von Sicherheitsrichtlinien
-   Reliable Actors-Sicherheitskonfiguration
-   Konfigurieren von SSL für Azure Service Fabric
-   Netzwerkisolation/-sicherheit mit Azure Service Fabric
-   Einrichten eines Schlüsseltresors für die Sicherheit
-   Zuweisen von Benutzern zu Rollen


## <a name="best-practices-for-securing-your-cluster"></a>Bewährte Methoden zum Schützen des Clusters

**Übersicht**

Verwenden Sie immer einen sicheren Cluster
-   Clustersicherheit: Verwenden Sie Zertifikate
-   Clientzugriff (Administrator und schreibgeschützt): Verwenden Sie AAD

Verwenden Sie automatische Bereitstellungen
-   Verwenden Sie Skripts zum Generieren, Bereitstellen und zum Ausführen eines Rollovers von Geheimnissen
-   Belassen Sie die Geheimnisse in KV, verwenden Sie AD für alle anderen Clientzugriffe
-   Kein Benutzer sollte ohne Authentifizierung darauf Zugriff haben.

Berücksichtigen Sie außerdem Folgendes:
-   Erstellen Sie DMZs mit Netzwerksicherheitsgruppen (NSGs)
-   Verwenden Sie Jump-Server für RDP in Cluster-VMs oder für die Verwaltung Ihres Clusters

Cluster müssen immer gesichert werden, um zu verhindern, dass nicht autorisierte Benutzer eine Verbindung mit dem Cluster herstellen können, insbesondere dann, wenn im Cluster Produktionsworkloads ausgeführt werden. Es ist zwar möglich, einen nicht gesicherten Cluster zu erstellen, allerdings kann in diesem Fall jeder anonyme Benutzer eine Verbindung herstellen, wenn der Cluster Verwaltungsendpunkte im öffentlichen Internet verfügbar macht.

Verwendete Technologien zum Implementieren dieser Szenarien. Die [Szenarien für die Clustersicherheit](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security) sind:

-   Knoten-zu-Knoten-Sicherheit: Sichert die Kommunikation zwischen den VMs und den Computern im Cluster. So wird sichergestellt, dass nur Computer, die zum Beitreten zum Cluster berechtigt sind, Anwendungen und Dienste im Cluster hosten können.
Cluster unter Azure oder eigenständige Cluster unter Windows können entweder [Zertifikatsicherheit](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security) oder [Windows-Sicherheit](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-windows-cluster-windows-security) für Windows Server-Computer verwenden.
-   Client-zu-Knoten-Sicherheit: Sichert die Kommunikation zwischen einem Service Fabric-Client und einzelnen Knoten im Cluster.
-   Rollenbasierte Zugriffssteuerung (RBAC): Sie geben die Administrator- und die Benutzerclientrolle zum Zeitpunkt der Clustererstellung an, indem Sie jeweils separate Identitäten (Zertifikate, AAD usw.) bereitstellen.
-   Sicherheitsempfehlungen: Für Azure-Cluster wird die Verwendung der AAD-Sicherheit empfohlen, um Clients und Zertifikate für die Knoten-zu-Knoten-Sicherheit zu authentifizieren.

Informationen zum Konfigurieren des eigenständigen Windows-Clusters finden Sie unter [Konfigurationseinstellungen für eigenständige Windows-Cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest).

Verwenden Sie Azure Resource Manager-Vorlagen und das Azure PowerShell-Modul für Service Fabric zum Erstellen sicherer Cluster.
Eine Schritt-für-Schritt-Anleitung, die die Einrichtung eines sicheren Azure Service Fabric-Clusters in Azure mithilfe von Azure Resource Manager beschreibt, ist [hier](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) verfügbar.

Verwenden Sie die Azure Resource Manager-Vorlage, um Ihren Cluster anzupassen
-   Durch das Setup verwaltete Speicher für VM-VHDs

Verwenden Sie die Azure Resource Manager-Vorlage, um Änderungen in die Ressourcengruppe zu übernehmen
-   Einfache Konfigurationsverwaltung
-   Überwachung

Behandeln Sie die Clusterkonfiguration wie Code
-   Überprüfen Sie die Konfigurationen gründlich, die Sie bereitstellen möchten
-   Vermeiden Sie die Verwendung impliziter Befehle, um Ihre Ressourcen direkt zu optimieren

Viele Aspekte des [Service Fabric-Anwendungslebenszyklus](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-lifecycle) können automatisiert werden. Das [Azure PowerShell-Modul für Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications#upload-the-application-package) automatisiert allgemeine Aufgaben zum Bereitstellen, Aktualisieren, Entfernen und Testen von Azure Service Fabric-Anwendungen. Verwaltete und HTTP-APIs für die App-Verwaltung sind auch verfügbar.

## <a name="use-x509-certificates"></a>Verwenden von X.509-Zertifikaten
Cluster sollten immer mit X.509-Zertifikaten oder Windows-Sicherheit geschützt werden. Die Sicherheit wird nur während der Clustererstellung konfiguriert, und nach der Erstellung des Clusters ist es nicht mehr möglich, die Sicherheit zu aktivieren.

Wenn Sie ein [Zertifikat für Cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security) angeben, legen Sie den Wert von „ClusterCredentialType“ auf „X509“ fest. Wenn Sie das Serverzertifikat für externe Verbindungen angeben, legen Sie „ServerCredentialType“ auf „X509“ fest.

-   Zertifikate, die in Clustern mit Produktionsworkloads verwendet werden, müssen entweder mit einem korrekt konfigurierten Windows Server-Zertifikatsdienst erstellt oder über eine genehmigte Zertifizierungsstelle (Certificate Authority, CA) bezogen werden.
-   Verwenden Sie in der Produktion niemals temporäre oder Testzertifikate, die mit Tools wie „MakeCert.exe“ erstellt wurden.
-   Sie können ein selbstsigniertes Zertifikat nur für Testcluster verwenden, aber nicht in der Produktion.

Wenn der Cluster unsicher ist. Da es möglich ist, anonyme Verbindungen damit herzustellen und Verwaltungsvorgänge durchzuführen, sollten Cluster für die Produktion immer mit X.509-Zertifikaten oder per Windows-Sicherheit geschützt werden.

Weitere Informationen zum Aktivieren von Zertifikaten im Service Fabric-Cluster finden Sie unter [Hinzufügen oder Entfernen von Zertifikaten für einen Service Fabric-Cluster in Azure](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-update-certs-azure).

## <a name="configure-security-policies"></a>Konfigurieren von Sicherheitsrichtlinien
Mit Service Fabric werden auch die Ressourcen gesichert, die von Anwendungen zum Zeitpunkt der Bereitstellung in den Benutzerkonten genutzt werden, z.B. Dateien, Verzeichnisse und Zertifikate. So lässt sich erreichen, dass ausgeführte Anwendungen auch in einer gemeinsamen gehosteten Umgebung sicher voneinander abgegrenzt sind.

-   Verwenden Sie eine Active Directory-Domänengruppe oder einen Active Directory-Benutzer: Sie können den Dienst mit den Anmeldeinformationen für einen Active Directory-Benutzer oder ein Active Directory-Gruppenkonto ausführen. Dies bezieht sich auf eine lokale Active Directory-Instanz in Ihrer Domäne, nicht auf Azure Active Directory (Azure AD). Indem Sie einen Domänenbenutzer oder eine -gruppe verwenden, können Sie anschließend auf andere Ressourcen in der Domäne (z.B. Dateifreigaben) zugreifen, für die Berechtigungen gewährt wurden.

-   Weisen Sie eine Sicherheitszugriffsrichtlinie für HTTP- und HTTPS-Endpunkte zu: Wenn Sie eine RunAs-Richtlinie auf einen Dienst anwenden und im Dienstmanifest Endpunktressourcen mit dem HTTP-Protokoll deklariert sind, müssen Sie eine SecurityAccessPolicy angeben. Diese Richtlinie soll sicherstellen, dass Ports, die diesen Endpunkten zugeordnet sind, richtig auf der Zugriffssteuerungsliste für das RunAs-Benutzerkonto eingetragen sind, mit dem der Dienst ausgeführt wird. Andernfalls hat „http.sys“ keinen Zugriff auf den Dienst, sodass bei Aufrufen vom Client Fehler auftreten.
Informationen zum Aktivieren von Sicherheitsrichtlinien in Service Fabric finden Sie unter [Konfigurieren von Sicherheitsrichtlinien für Ihre Anwendung](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-runas-security).

## <a name="reliable-actors-security-configuration"></a>Reliable Actors-Sicherheitskonfiguration
Service Fabric Reliable Actors ist eine Implementierung des Akteurentwurfsmusters. Die Entscheidung, ob ein bestimmtes Muster verwendet werden soll, wird basierend darauf getroffen, ob ein Softwareentwurfsproblem dem Muster entspricht.

Berücksichtigen Sie in Verbindung mit Ihrem Problem oder Szenario das Muster in den folgenden Fällen:
-   Der Problembereich enthält zahlreiche (mehrere Tausend) kleine, unabhängige und isolierte Zustands- und Logikeinheiten.
-   Sie möchten mit Singlethread-Objekten arbeiten, die keine wesentliche Interaktion von externen Komponenten erfordern (etwa, um den Zustand für einen Satz von Akteuren abzufragen).
-   Ihre Akteurinstanzen blockieren Aufrufer durch die Ausgabe von E/A-Vorgängen nicht mit unvorhersagbaren Verzögerungen.

In Service Fabric werden Akteure im Reliable Actors-Framework implementiert. Hierbei handelt es sich um ein auf dem Akteurmuster basierendes Anwendungsframework, das auf [Service Fabric Reliable Services](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-introduction) aufbaut. Jeder von Ihnen geschriebene Reliable Actors-Dienst ist tatsächlich eine partitionierte, zustandsbehaftete Reliable Services-Instanz.
Jeder Akteur wird als Instanz eines Akteurtyps definiert (genau wie ein .NET-Objekt eine Instanz eines .NET-Typs darstellt). Beispielsweise kann es einen Actor-Typ geben, der die Funktionalität eines Rechners implementiert, und es kann viele Actors dieses Typs geben, die auf verschiedenen Knoten in einem Cluster verteilt sind. Jeder solche Actor wird durch eine Actor-ID eindeutig identifiziert.

[Replicator-Sicherheitskonfigurationen](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-kvsactorstateprovider-configuration) werden verwendet, um den während der Replikation verwendeten Kommunikationskanal zu schützen. Dies bedeutet, dass Dienste ihren gegenseitigen Replikationsdatenverkehr nicht erkennen können. Dadurch wird sichergestellt, dass die Daten nicht nur hochverfügbar, sondern auch sicher sind. Standardmäßig wird die Replikationssicherheit durch einen leeren Sicherheitskonfigurationsabschnitt verhindert.
Replicator-Konfigurationen konfigurieren den Replicator, der dafür verantwortlich ist, den Status des Actor-Status-Anbieters hochverfügbar zu machen.

## <a name="configure-ssl-for-azure-service-fabric"></a>Konfigurieren von SSL für Azure Service Fabric

Serverauthentifizierung: [Authentifiziert](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) die Verwaltungsendpunkte des Clusters bei einem Verwaltungsclient, sodass der Verwaltungsclient weiß, dass die Kommunikation tatsächlich aus dem Cluster stammt. Dieses Zertifikat stellt auch [SSL](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-creation-via-arm) für die HTTPS-Verwaltungs-API und für Service Fabric Explorer über HTTPS bereit.
Sie benötigen einen benutzerdefinierten Domänennamen für Ihren Cluster. Wenn Sie ein Zertifikat von einer Zertifizierungsstelle anfordern, muss der Name des Antragstellers für das Zertifikat dem benutzerdefinierten Domänennamen entsprechen, den Sie für Ihren Cluster verwenden.

Sie müssen zuerst ein SSL-Zertifikat beziehen, um SSL für eine Anwendung zu konfigurieren. Dieses muss von einer Zertifizierungsstelle, einem vertrauenswürdigen Dritten, der Zertifikate für diesen Zweck ausgibt, ausgegeben werden. Wenn Sie noch kein Zertifikat haben, müssen Sie eines von einem Unternehmen erwerben, das SSL-Zertifikate verkauft.

Das Zertifikat muss die folgenden Anforderungen für SSL-Zertifikate in Azure erfüllen:
-   Das Zertifikat muss einen privaten Schlüssel enthalten.
-   Das Zertifikat muss für den Schlüsselaustausch erstellt werden und in eine PFX-Datei (Persönlicher Informationsaustausch) exportiert werden können.
-   Der Name des Antragstellers für das Zertifikat muss der Domäne entsprechen, über die auf den Clouddienst zugegriffen wird. Für die Domäne cloudapp.net können Sie kein SSL-Zertifikat von einer Zertifizierungsstelle beziehen. Sie müssen einen benutzerdefinierten Domänennamen erwerben, den Sie für den Zugriff auf Ihren Dienst verwenden können. Wenn Sie ein Zertifikat von einer Zertifizierungsstelle anfordern, muss der Name des Antragstellers für das Zertifikat der Domäne entsprechen, über die auf Ihre Anwendung zugegriffen wird. Wenn beispielsweise der benutzerdefinierte Domänenname „contoso.com“ lautet, fordern Sie von Ihrer Zertifizierungsstelle ein Zertifikat für **.contoso.com** oder **www.contoso.com** an.
-   Das Zertifikat muss mindestens eine 2048-Bit-Verschlüsselung haben.

HTTP ist unsicher und anfällig für Lauschangriffe, da die Daten, die vom Webbrowser an den Webserver oder zwischen anderen Endpunkten übertragen werden, in Klartext übermittelt werden. Dies bedeutet, dass Angreifer sensible Daten, z.B. Kreditkartendaten und Anmeldedaten für Konten, abfangen und anzeigen können. Wenn die Daten über einen Browser mithilfe von HTTPS gesendet oder bereitgestellt werden, wird mit SSL sichergestellt, dass diese Informationen verschlüsselt sind und nicht abgefangen werden können.

Weitere Informationen finden Sie unter [Konfigurieren von SSL für eine Anwendung in Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-configure-ssl-certificate).

## <a name="network-isolationsecurity-with-azure-service-fabric"></a>Netzwerkisolation/-sicherheit mit Azure Service Fabric
Verwenden Sie die [Azure Resource Manager-Vorlage (ARM)](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) als Beispiel für die Einrichtung eines sicheren Clusters mit drei Knoten und zum Steuern des eingehenden und ausgehenden Netzwerkdatenverkehrs mit Netzwerksicherheitsgruppen.

Die Vorlage weist eine Netzwerksicherheitsgruppe für jede VM-Skalierungsgruppe (VMSS) auf, um den Datenverkehr in und aus der VMSS zu steuern. Standardmäßig werden die Regeln so eingerichtet, dass der gesamte von den Systemdiensten benötigte Datenverkehr und die in der Vorlage angegebenen Anwendungsports zugelassen werden. Überprüfen Sie diese Regeln, und ändern Sie sie nach Ihren Bedürfnissen, indem Sie z.B. neue für Ihre Anwendungen hinzufügen.

Weitere Informationen finden Sie unter [Netzwerkmuster für Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-patterns-networking).

## <a name="set-up-a-key-vault-for-security"></a>Einrichten eines Schlüsseltresors für die Sicherheit
Zertifikate werden in Service Fabric zur Authentifizierung und Verschlüsselung verwendet, um verschiedene Aspekte eines Clusters und der zugehörigen Anwendungen zu sichern.

Service Fabric verwendet X.509-Zertifikate, um einen Cluster zu sichern und Sicherheitsfunktionen für Anwendungen bereitzustellen. Azure Key Vault dient zum [Verwalten von Zertifikaten](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-update-certs-azure) für Service Fabric-Cluster in Azure. Wenn ein Cluster in Azure bereitgestellt wird, ruft der für die Erstellung von Service Fabric-Clustern zuständige Azure-Ressourcenanbieter Zertifikate aus Key Vault ab und installiert sie auf den virtuellen Clustercomputern.

Die Beziehung zwischen [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault), einem Service Fabric-Cluster und dem Azure-Ressourcenanbieter, der beim Erstellen eines Clusters die in einem Schlüsseltresor gespeicherten Zertifikate verwendet.

**Erstellen Sie eine Ressourcengruppe:** Im ersten Schritt wird speziell für Ihren Schlüsseltresor eine neue Ressourcengruppe erstellt. Es empfiehlt sich, den Schlüsseltresor in einer eigenen Ressourcengruppe zu platzieren. Dadurch können Sie die Compute- und Speicherressourcengruppen (einschließlich der Ressourcengruppe mit Ihrem Service Fabric-Cluster) entfernen, ohne Ihre Schlüssel und geheimen Schlüssel zu verlieren. Die Ressourcengruppe mit Ihrem Schlüsseltresor muss sich in der gleichen Region befinden wie der Cluster, der den Tresor verwendet.

**Erstellen Sie einen Schlüsseltresor in der neuen Ressourcengruppe:** Der Schlüsseltresor muss für die Bereitstellung aktiviert sein, damit der Computeressourcenanbieter Zertifikate daraus abrufen und ihn in VM-Instanzen installieren kann.
Weitere Informationen zum Einrichten eines Azure-Schlüsseltresors finden Sie unter [Erste Schritte mit dem Azure-Schlüsseltresor](https://docs.microsoft.com/azure/key-vault/key-vault-get-started).

## <a name="assign-users-roles"></a>Zuweisen von Benutzerrollen
Nachdem Sie die Anwendungen für Ihren Cluster erstellt haben, müssen Ihre Benutzer den von Service Fabric unterstützten Rollen zugewiesen werden: „read-only“ (schreibgeschützt) und „admin“ (Administrator). Für die Rollenzuweisung können Sie das klassische Azure-Portal verwenden.

>[!Note]
> Weitere Informationen zu Rollen in Service Fabric finden Sie unter [Rollenbasierte Zugriffssteuerung für Service Fabric-Clients](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-roles).

Azure Service Fabric unterstützt zwei unterschiedliche Zugriffssteuerungstypen für Clients, die mit einem [Service Fabric-Cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) verbunden sind: Administrator und Benutzer. Zugriffssteuerung ermöglicht es dem Clusteradministrator, den Zugriff auf bestimmte Clustervorgänge für verschiedene Gruppen von Benutzern einzuschränken, wodurch die Sicherheit des Clusters erhöht wird.

## <a name="next-steps"></a>Nächste Schritte
- Einrichten der Service Fabric-[Entwicklungsumgebung](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started).
- Informieren Sie sich über [Service Fabric-Supportoptionen](https://docs.microsoft.com/azure/service-fabric/service-fabric-support).


