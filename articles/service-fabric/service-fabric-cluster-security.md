---
title: "Schützen eines Azure Service Fabric-Clusters | Microsoft-Dokumentation"
description: "Lernen Sie Sicherheitsszenarien für einen Azure Service Fabric-Cluster und die verschiedenen Technologien kennen, die Sie verwenden können, um sie zu implementieren."
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 26b58724-6a43-4f20-b965-2da3f086cf8a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: chackdan
ms.translationtype: HT
ms.sourcegitcommit: 4eb426b14ec72aaa79268840f23a39b15fee8982
ms.openlocfilehash: e75929ee5d3f57af77c66910cc294a7c0fb6629a
ms.contentlocale: de-de
ms.lasthandoff: 09/06/2017

---
# <a name="service-fabric-cluster-security-scenarios"></a>Szenarien für die Clustersicherheit in Service Fabric
Ein Azure Service Fabric-Cluster ist eine Ressource, die sich in Ihrem Besitz befindet. Sie müssen Ihre Cluster schützen, um zu verhindern, dass nicht autorisierte Benutzer eine Verbindung mit ihnen herstellen. Ein sicherer Cluster ist besonders wichtig, wenn Sie Produktionsworkloads im Cluster ausführen. Es ist zwar möglich, einen ungeschützten Cluster zu erstellen, doch falls der Cluster Verwaltungsendpunkte im öffentlichen Internet verfügbar macht, können anonyme Benutzer eine Verbindung mit ihm herstellen. 

Dieser Artikel bietet eine Übersicht über Sicherheitsszenarien für Azure-Cluster und eigenständige Cluster sowie die verschiedenen Technologien, die Sie zu deren Implementierung verwenden können:

* Knoten-zu-Knoten-Sicherheit
* Client-zu-Knoten-Sicherheit
* Rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)

## <a name="node-to-node-security"></a>Knoten-zu-Knoten-Sicherheit
Die Knoten-zu-Knoten-Sicherheit dient zum Schutz der Kommunikation zwischen den virtuellen oder physischen Computern in einem Cluster. Dieses Sicherheitsszenario stellt sicher, dass nur Computer, die zum Clusterbeitritt berechtigt sind, Anwendungen und Dienste im Cluster hosten können.

![Diagramm für die Kommunikation zwischen Knoten][Node-to-Node]

Cluster unter Azure und eigenständige Cluster unter Windows können beide entweder [Zertifikatsicherheit](https://msdn.microsoft.com/library/ff649801.aspx) oder [Windows-Sicherheit](https://msdn.microsoft.com/library/ff649396.aspx) für Windows Server-Computer verwenden.

### <a name="node-to-node-certificate-security"></a>Zertifikatsicherheit (Knoten zu Knoten)
Service Fabric verwendet X.509-Serverzertifikate, die Sie als Teil der Knotentypkonfiguration angeben, wenn Sie einen Cluster erstellen. Ein kurzer Überblick darüber, was diese Zertifikate sind und wie Sie sie abrufen oder erstellen können, wird am Ende dieses Artikels bereitgestellt.

Richten Sie die Zertifikatsicherheit beim Erstellen des Clusters im Azure-Portal, über Azure Resource Manager-Vorlagen oder eine eigenständige JSON-Vorlage ein. Sie können ein primäres Zertifikat und ein optionales sekundäres Zertifikat für Zertifikatrollover festlegen. Die angegebenen primären und sekundären Zertifikate müssen sich von den Administratorclientzertifikaten und den schreibgeschützten Clientzertifikaten unterscheiden, die Sie für die [Client-zu-Knoten-Sicherheit](#client-to-node-security) angeben.

Informationen zum Einrichten der Zertifikatsicherheit in einem Cluster für Azure finden Sie unter [Einrichten eines Clusters mit einer Azure Resource Manager-Vorlage](service-fabric-cluster-creation-via-arm.md).

Informationen zum Einrichten der Zertifikatsicherheit in einem Cluster für einen eigenständigen Windows Server-Cluster finden Sie unter [Schützen eines eigenständigen Windows-Clusters mithilfe von X.509-Zertifikaten](service-fabric-windows-cluster-x509-security.md).

### <a name="node-to-node-windows-security"></a>Windows-Sicherheit (Knoten zu Knoten)
Informationen zum Einrichten der Windows-Sicherheit für einen eigenständigen Windows Server-Cluster finden Sie unter [Schützen eines eigenständigen Windows-Clusters mit Windows-Sicherheit](service-fabric-windows-cluster-windows-security.md).

## <a name="client-to-node-security"></a>Client-zu-Knoten-Sicherheit
Client-zu-Knoten-Sicherheit authentifiziert Clients und schützt die Kommunikation zwischen einem Client und einzelnen Knoten im Cluster. Diese Art von Sicherheit sorgt dafür, dass nur autorisierte Benutzer auf den Cluster und die im Cluster bereitgestellten Anwendungen zugreifen können. Clients werden entweder über ihre Windows-Sicherheitsanmeldeinformationen oder ihre Zertifikat-Sicherheitsanmeldeinformationen eindeutig identifiziert.

![Diagramm für die Kommunikation zwischen Client und Knoten][Client-to-Node]

Cluster unter Azure und eigenständige Cluster unter Windows können entweder [Zertifikatsicherheit](https://msdn.microsoft.com/library/ff649801.aspx) oder [Windows-Sicherheit](https://msdn.microsoft.com/library/ff649396.aspx) verwenden.

### <a name="client-to-node-certificate-security"></a>Zertifikatsicherheit (Client zu Knoten)
Richten Sie die Client-zu-Knoten-Zertifikatsicherheit beim Erstellen des Clusters im Azure-Portal, über Azure Resource Manager-Vorlagen oder eine eigenständige JSON-Vorlage ein. Um das Zertifikat zu erstellen, geben Sie ein Clientzertifikat für einen Administrator oder einen Benutzer an. Als bewährte Methode gilt: Die angegebenen Administrator- und Benutzerclientzertifikate müssen sich von den primären und sekundären Zertifikaten unterscheiden, die Sie für [Knoten-zu-Knoten-Sicherheit](#node-to-node-security) angeben. Standardmäßig werden die Clusterzertifikate für Knoten-zu-Knoten-Sicherheit der Liste zulässiger Clientadministratorzertifikate hinzugefügt.

Clients, die unter Verwendung des Administratorzertifikats eine Verbindung mit dem Cluster herstellen, haben uneingeschränkten Zugriff auf die Verwaltungsfunktionen. Clients, die unter Verwendung des schreibgeschützten Benutzerclientzertifikats eine Verbindung mit dem Cluster herstellen, haben nur Lesezugriff auf die Verwaltungsfunktionen. Diese Zertifikate werden für die später in diesem Artikel beschriebenen RBAC verwendet.

Informationen zum Einrichten der Zertifikatsicherheit in einem Cluster für Azure finden Sie unter [Einrichten eines Clusters mit einer Azure Resource Manager-Vorlage](service-fabric-cluster-creation-via-arm.md).

Informationen zum Einrichten der Zertifikatsicherheit in einem Cluster für einen eigenständigen Windows Server-Cluster finden Sie unter [Schützen eines eigenständigen Windows-Clusters mithilfe von X.509-Zertifikaten](service-fabric-windows-cluster-x509-security.md).

### <a name="client-to-node-azure-active-directory-security-on-azure"></a>Client-zu-Knoten-Sicherheit von Azure Active Directory in Azure
Für in Azure ausgeführte Cluster können Sie den Zugriff auf die Verwaltungsendpunkte auch mit Azure Active Directory (Azure AD) schützen. Wie Sie die erforderlichen Azure AD-Artefakte erstellen, sie während der Clustererstellung auffüllen und danach die Verbindung mit diesen Clustern herstellen, erfahren Sie unter [Erstellen eines Service Fabric-Clusters in Azure mithilfe von Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).

## <a name="security-recommendations"></a>Sicherheitsempfehlungen
Für Azure-Cluster wird für Knoten-zu Knoten-Sicherheit die Verwendung der Azure AD-Sicherheit empfohlen, um Clients und Zertifikate zu authentifizieren.

Für eigenständige Windows Server-Cluster sollten Sie die Windows-Sicherheit mit gruppenverwalteten Dienstkonten verwenden, wenn Sie über Windows Server 2012 R2 und Windows Active Directory verfügen. Verwenden Sie andernfalls die Windows-Sicherheit mit Windows-Konten.

## <a name="role-based-access-control-rbac"></a>Rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)
Mit der Zugriffssteuerung können Sie den Zugriff auf bestimmte Clustervorgänge für verschiedene Gruppen von Benutzern einschränken. Dadurch wird der Cluster sicherer. Für Clients, die eine Clusterverbindung herstellen, werden zwei Zugriffssteuerungen unterstützt: Administratorrolle und Benutzerrolle.

Benutzer mit Administratorrolle haben vollständigen Zugriff auf Verwaltungsfunktionen (einschließlich Lese-/Schreibzugriff). Benutzer mit Benutzerrolle haben standardmäßig nur Lesezugriff auf Verwaltungsfunktionen (z.B. Abfragefunktionen). Sie können auch Anwendungen und Dienste auflösen.

Legen Sie die Administrator- und Benutzerclientrollen bei der Erstellung des Clusters fest. Weisen Sie Rollen zu, indem Sie verschiedene Identitäten für jeden Rollentyp bereitstellen (z.B. durch Verwendung von Zertifikaten oder Azure AD). Weitere Informationen zu den Standardeinstellungen der Zugriffssteuerung sowie zum Ändern der Standardeinstellungen finden Sie unter [Rollenbasierte Zugriffssteuerung für Service Fabric-Clients](service-fabric-cluster-security-roles.md).

## <a name="x509-certificates-and-service-fabric"></a>X.509-Zertifikate und Service Fabric
Digitale X.509-Zertifikate werden in der Regel verwendet, um Clients und Server zu authentifizieren. Sie werden auch zum Verschlüsseln und digitalen Signieren von Nachrichten verwendet. Weitere Informationen zu digitalen X. 509-Zertifikaten finden Sie unter [Verwenden von Zertifikaten](http://msdn.microsoft.com/library/ms731899.aspx).

Einige wichtige Punkte sind zu beachten:

* Um Zertifikate für Cluster zu erstellen, die Produktionsworkloads ausführen, verwenden Sie einen korrekt konfigurierten Windows Server-Zertifikatsdienst oder den einer genehmigten [Zertifizierungsstelle](https://en.wikipedia.org/wiki/Certificate_authority) (Certificate Authority, CA).
* Verwenden Sie niemals temporäre oder Testzertifikate, die Sie mithilfe von Tools wie MakeCert.exe in einer Produktionsumgebung erstellen.
* Selbstsignierte Zertifikate können Sie nur in einem Testcluster verwenden. Verwenden Sie kein selbstsigniertes Zertifikat in der Produktion.

### <a name="server-x509-certificates"></a>X.509-Serverzertifikate
Serverzertifikate müssen primär einen Server (Knoten) für Clients oder einen Server (Knoten) für einen Server (Knoten) authentifizieren. Bei der Authentifizierung eines Knotens durch einen Client oder Knoten besteht eine der ersten Prüfungen darin, den Wert des allgemeinen Namens im Feld **Antragsteller** zu prüfen. Dieser allgemeine Name oder einer der alternativen Antragstellernamen (Subject Alternative Names, SANs) der Zertifikate muss in der Liste der zulässigen allgemeinen Namen vorhanden sein.

Informationen zum Generieren von Zertifikaten mit SANs finden Sie unter [How to add a Subject Alternative Name to a secure LDAP certificate](http://support.microsoft.com/kb/931351) (Hinzufügen eines alternativen Antragstellernamens zu einem geschützten LDAP-Zertifikat).

Das Feld **Antragsteller** kann mehrere Werte enthalten. Der Typ jedes Werts wird durch ein Initialisierungspräfix angegeben. In der Regel lautet die Initialisierung **CN** (für *Common Name*, den allgemeinen Namen); z.B. **CN = www.contoso.com**. Das Feld **Antragsteller** kann leer sein. Wenn das optionale Feld **Alternativer Antragstellername** ausgefüllt ist, muss es sowohl den allgemeinen Namen des Zertifikats als auch einen Eintrag pro SAN enthalten. Diese werden als Werte für **DNS-Name** eingegeben.

Der Wert des Felds **Beabsichtigte Zwecke** des Zertifikats muss einen entsprechenden Wert enthalten, z.B. **Serverauthentifizierung** oder **Clientauthentifizierung**.

### <a name="client-x509-certificates"></a>X.509-Clientzertifikate
Clientzertifikate werden in der Regel nicht von einer Drittanbieter-Zertifizierungsstelle ausgestellt. Stattdessen enthält der persönliche Speicher des aktuellen Benutzerspeicherorts in der Regel Clientzertifikate, die von einer Stammzertifizierungsstelle mit dem Wert **Beabsichtigte Zwecke** der **Clientauthentifizierung** platziert werden. Der Client kann dieses Zertifikat verwenden, wenn eine gegenseitige Authentifizierung erforderlich ist.

> [!NOTE]
> Für sämtliche Verwaltungsvorgänge für einen Service Fabric-Cluster sind Serverzertifikate erforderlich. Clientzertifikate können für Verwaltungsaufgaben nicht verwendet werden.

## <a name="next-steps"></a>Nächste Schritte
* [Erstellen eines Service Fabric-Clusters in Azure mithilfe von Azure Resource Manager](service-fabric-cluster-creation-via-arm.md) 
* [Erstellen eines Clusters mit dem Azure-Portal](service-fabric-cluster-creation-via-portal.md)

<!--Image references-->
[Node-to-Node]: ./media/service-fabric-cluster-security/node-to-node.png
[Client-to-Node]: ./media/service-fabric-cluster-security/client-to-node.png

