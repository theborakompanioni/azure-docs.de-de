---
title: Wichtige Features und Konzepte in Azure Stack | Microsoft-Dokumentation
description: "Erfahren Sie etwas über die wichtigen Features und Konzepte in Azure Stack."
services: azure-stack
documentationcenter: 
author: Heathl17
manager: byronr
editor: 
ms.assetid: 09ca32b7-0e81-4a27-a6cc-0ba90441d097
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: helaw
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: c361d5b47a22689e7bf6749067a39e12df25c84a
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---
# <a name="key-features-and-concepts-in-azure-stack"></a>Wichtige Features und Konzepte in Azure Stack
Wenn Sie mit Microsoft Azure Stack noch nicht vertraut sind, können diese Begriffe und Featurebeschreibungen möglicherweise hilfreich sein.

## <a name="personas"></a>Personas
Es gibt zwei Arten von Benutzern für Microsoft Azure Stack: den Cloudbetreiber (Anbieter) und den Mandanten (Verbraucher).

* Ein **Cloudbetreiber** kann Azure Stack konfigurieren und Angebote, Pläne, Dienste, Kontingente und Preise verwalten, um Ressourcen für ihre Mandanten bereitzustellen.  Cloudbetreiber können auch die Kapazität verwalten und auf Warnungen reagieren.  
* Ein **Mandant** (auch als „Benutzer“ bezeichnet) nutzt Dienste, die der Cloudadministrator anbietet. Mandanten können Dienste, die sie abonniert haben – z.B. Web-Apps, Speicher und virtuelle Computer – bereitstellen, überwachen und verwalten.

## <a name="portal"></a>Portal
Die Hauptmethoden der Interaktion mit Microsoft Azure Stack sind das Administratorportal, das Benutzerportal und PowerShell.

Alle Azure Stack-Portale basieren jeweils auf separaten Azure Resource Manager-Instanzen.  Ein Cloudbetreiber verwendet das Administratorportal zum Verwalten von Azure Stack und für Aktionen wie das Erstellen von Mandantenangeboten.  Das Benutzerportal (auch als „Mandantenportal“ bezeichnet) bietet eine Self-Service-Benutzeroberfläche für die Nutzung von Cloudressourcen wie virtuelle Computern, Speicherkonten und Web-Apps. Weitere Informationen finden Sie unter [Using the Azure Stack administrator and user portals](azure-stack-manage-portals.md) (Verwenden der Azure Stack-Administrator und -Benutzerportale).

## <a name="identity"></a>Identity 
Azure Stack verwendet entweder Azure Active Directory (AAD) oder Active Directory-Verbunddienste (AD FS) als Identitätsanbieter.  

### <a name="azure-active-directory"></a>Azure Active Directory
Azure Active Directory ist der cloudbasierte, mehrinstanzenfähige Identitätsanbieter von Microsoft.  Die meisten Hybridszenarien verwenden Azure Active Directory als Identitätsspeicher.

### <a name="active-directory-federation-services"></a>Active Directory-Verbunddienste (AD FS)
Sie können Active Directory-Verbunddienste (AD FS) für nicht verbundene Bereitstellungen von Azure Stack verwenden.  Azure Stack, Ressourcenanbieter und anderen Anwendungen verwenden AD FS ähnlich wie Azure Active Directory. Azure Stack umfasst eine eigene AD FS- und Active Directory-Instanz sowie eine Active Directory Graph-API. Azure Stack Development Kit unterstützt die folgenden AD FS-Szenarien:

- Anmelden bei der Bereitstellung mithilfe von AD FS
- Erstellen eines virtuellen Computers mit Geheimnissen in Key Vault
- Erstellen eines Tresors für das Speichern von/den Zugriff auf Geheimnisse
- Erstellen von benutzerdefinierten RBAC-Rollen im Abonnement
- Zuweisen von Benutzern zu RBAC-Rollen für Ressourcen
- Erstellen von systemweiten RBAC-Rollen über Azure PowerShell
- Anmelden als Benutzer über Azure PowerShell
- Erstellen von Dienstprinzipalen für die Anmeldung bei Azure PowerShell


## <a name="regions-services-plans-offers-and-subscriptions"></a>Regionen, Dienste, Pläne, Angebote und Abonnements
Dienste werden in Azure Stack mithilfe von Regionen, Abonnements, Angeboten und Plänen an Mandanten übermittelt. Die Mandanten können mehrere Angebote abonnieren. Angebote können einen oder mehrere Pläne enthalten und die Pläne wiederum einen oder mehrere Dienste.

![](media/azure-stack-key-features/image4.png)

Beispielhierarchie eines Mandantenabonnements mit Angeboten, die jeweils unterschiedliche Pläne und Dienste umfassen.

### <a name="regions"></a>Regionen
Azure Stack-Regionen sind ein grundlegendes Element der Skalierung und Verwaltung. Eine Organisation kann über mehrere Regionen mit verfügbaren Ressourcen in jeder Region verfügen. In Regionen können auch andere Dienstangebote verfügbar sein. Im Azure Stack Development Kit wird nur eine einzelne Region unterstützt, die automatisch den Namen *lokal* erhält.

### <a name="services"></a>Dienste
Microsoft Azure Stack ermöglicht Anbietern das Bereitstellen einer Vielzahl von Diensten und Anwendungen, z.B. virtuelle Computer, SQL Server-Datenbanken, SharePoint, Exchange und vieles mehr.

### <a name="plans"></a>Pläne
Pläne sind Gruppen von einem oder mehreren Diensten. Als Anbieter erstellen Sie Pläne, die Sie Ihren Mandanten anbieten. Im Gegenzug abonnieren Ihre Mandanten Ihre Angebote, um die Pläne und Dienste, die sie damit enthalten, zu verwenden.

Jeder Dienst, der einem Plan hinzugefügt wird, kann mit Kontingenteinstellungen konfiguriert werden, die Ihnen beim Verwalten Ihrer Cloudkapazität helfen. Kontingente können Einschränkungen umfassen, z.B. für virtuelle Computer, RAM und CPU, und gelten pro Benutzerabonnement. Kontingente können nach Standort unterschieden werden. Beispielsweise kann für einen Plan mit Computediensten in Region A ein Kontingent von zwei virtuellen Computern, 4 GB RAM und 10 CPU-Kernen gelten.

Beim Erstellen eines Angebots kann der Dienstadministrator einen **Basisplan** aufnehmen. Diese Basispläne sind standardmäßig enthalten, wenn ein Mandant dieses Angebot abonniert. Wenn Benutzer ein Abonnement abschließt (und das Abonnement erstellt wurde), hat der Benutzer Zugriff auf alle Ressourcenanbieter, die in diesen Basisplänen enthalten sind (inklusive der entsprechenden Kontingente).

Der Dienstadministrator kann auch **Add-On-Pläne** in ein Angebot einfügen. Add-On-Pläne sind nicht standardmäßig im Abonnement enthalten. Add-On-Pläne sind zusätzliche Pläne (Kontingente) in einem Angebot, die ein Abonnementbesitzer seinem Abonnement hinzufügen kann.

### <a name="offers"></a>Angebote
Angebote sind Gruppen von einem oder mehreren Plänen, die Anbieter Mandanten zum Erwerb (Abonnieren) anbieten. Beispielsweise könnte Angebot „Alpha“ den Plan A mit einem Satz von Computediensten und Plan B mit einem Satz von Speicher- und Netzwerkdiensten enthalten.

Ein Angebot enthält einen Satz von Basisplänen, und Dienstadministratoren können Add-On-Pläne erstellen, die Mandanten ihrem Abonnement hinzufügen können.

### <a name="subscriptions"></a>Abonnements
Über ein Abonnement erwerben Mandanten Ihre Angebote. Ein Abonnement ist eine Kombination aus einem Mandanten und einem Angebot. Ein Mandant kann mehrere Angebote abonnieren. Jedes Abonnement gilt nur für ein Angebot. Die Abonnements eines Mandanten legen fest, auf welche Pläne und Dienste er zugreifen kann.

Abonnements helfen Anbietern beim Organisieren von und dem Zugriff auf Cloudressourcen.

Für den Administrator wird während der Bereitstellung ein Standardanbieterabonnement erstellt. Dieses Abonnement kann zum Verwalten von Azure Stack, zum Bereitstellen weiterer Ressourcenanbieter sowie zum Erstellen von Plänen und Angeboten für Mandanten verwendet werden. Es darf nicht zum Ausführen von Workloads und Anwendungen von Kunden verwendet werden. 

## <a name="azure-resource-manager"></a>Azure Resource Manager
Mithilfe von Azure Resource Manager können Sie in einem vorlagenbasierten, deklarativen Modell mit Ihren Infrastrukturressourcen arbeiten.   Er bietet eine zentrale Benutzeroberfläche, auf der Sie Ihre Lösungskomponenten bereitstellen und verwalten können. Umfassende Informationen und Anleitungen finden Sie in der [Übersicht über den Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

### <a name="resource-groups"></a>Ressourcengruppen
Ressourcengruppen sind Sammlungen von Ressourcen, Diensten und Anwendungen. Jede Ressource weist einen Typ auf, z.B. virtueller Computer, virtuelles Netzwerk öffentliche IP-Adresse, Speicherkonto oder Website. Jede Ressource muss einer Ressourcengruppe angehören, damit Ressourcen z.B. nach Workload oder Standort logisch organisiert werden können.  In Microsoft Azure Stack werden Ressourcen wie Pläne und Angebote auch in Ressourcengruppen verwaltet.
 
### <a name="azure-resource-manager-templates"></a>Azure-Ressourcen-Manager-Vorlagen
Mit Azure Resource Manager können Sie eine Vorlage (im JSON-Format) erstellen, mit der die Bereitstellung und Konfiguration Ihrer Anwendung definiert wird. Diese Vorlage wird als Azure Resource Manager-Vorlage bezeichnet und ist eine deklarative Möglichkeit zum Definieren der Bereitstellung. Mit einer Vorlage können Sie die Anwendung während des gesamten App-Lebenszyklus wiederholt bereitstellen und sicher sein, dass Ihre Ressourcen einheitlich bereitgestellt werden.

## <a name="resource-providers-rps"></a>Ressourcenanbieter (Resource Providers, RPs)
Ressourcenanbieter (Resource Provider, RP) sind Webdienste, die die Grundlage für alle Azure-basierten IaaS- und PaaS-Dienste bilden. Azure Resource Manager stützt sich auf verschiedene Ressourcenanbieter, um den Zugriff auf Dienste zu ermöglichen.

Es gibt vier grundlegende RPs: Netzwerk, Speicher, Compute und KeyVault. Mit diesen Ressourcenanbietern können Sie die zugehörigen Ressourcen konfigurieren und steuern. Dienstadministratoren können auch neue benutzerdefinierte Ressourcenanbieter hinzufügen.

### <a name="compute-rp"></a>Compute-RP
Mit dem Compute-Ressourcenanbieter (Compute Resource Provider, CRP) können Azure Stack-Mandanten eigene virtuelle Computer erstellen. Der CRP bietet die Möglichkeit, virtuelle Computer sowie Erweiterungen für virtuelle Computer zu erstellen. Der Erweiterungsdienst für virtuelle Computer unterstützt die Bereitstellung von IaaS-Funktionen für virtuelle Computer mit Windows und Linux.  Beispielsweise können Sie den CRP zum Bereitstellen eines virtuellen Linux-Computers verwenden und während der Bereitstellung Bash-Skripts ausführen, um den virtuellen Computer zu konfigurieren.

### <a name="network-rp"></a>Network-RP
Der Netzwerkressourcenanbieter (NRP) bietet eine Reihe von SDN (Software Defined Networking)- und NFV (Network Function Virtualization)-Funktionen für die private Cloud.  Mit dem NRP können Sie Ressourcen wie Software-Lastenausgleichsmodule, öffentliche IP-Adressen, Netzwerksicherheitsgruppen und virtuelle Netzwerke erstellen.

### <a name="storage-rp"></a>Speicher-RP
Der Speicherressourcenanbieter bietet vier mit Azure konsistente Speicherdienste: Blob, Tabelle, Warteschlange und Kontoverwaltung. Er bietet außerdem einen Speichercloud-Verwaltungsdienst, um die Dienstanbieterverwaltung von mit Azure konsistenten Speicherdiensten zu vereinfachen. Azure Storage bietet die Flexibilität zum Speichern und Abrufen großer Mengen von unstrukturierten Daten, z.B. Dokumente und Mediendateien mit Azure-Blobs und strukturierte NoSQL-basierte Daten mit Azure-Tabellen. Weitere Informationen zu Azure Storage finden Sie unter [Einführung in Microsoft Azure Storage](../storage/common/storage-introduction.md).

#### <a name="blob-storage"></a>Blob Storage
Der Blob-Speicher speichert beliebige DataSets. Bei einem Blob kann es sich um einen beliebigen Text- oder Binärdatentyp handeln – also beispielsweise um ein Dokument, eine Mediendatei oder einen Anwendungs-Installer. Der Tabellenspeicher speichert strukturierte DataSets. Table Storage ist ein Schlüssel-/Attribut-basierter NoSQL-Datenspeicher, der eine rasche Entwicklung und schnellen Zugriff auf große Datenmengen erlaubt. Der Warteschlangenspeicher bietet zuverlässiges Messaging für die Workflowverarbeitung und für die Kommunikation zwischen Komponenten von Clouddiensten.

Jedes Blob ist unter einem Container organisiert. Container sind auch eine praktische Möglichkeit, um Gruppen von Objekten Sicherheitsrichtlinien zuzuordnen. Ein Speicherkonto kann eine beliebige Anzahl von Containern enthalten, und ein Container kann eine beliebige Anzahl von Blobs enthalten. Die Speicherkapazität eines Speicherkontos beträgt maximal 500 TB. Blob Storage bietet drei Arten von Blobs: Blockblobs, Anfügeblobs und Seitenblobs (Festplatten). Blockblobs sind für das Streamen und Speichern von Cloudobjekten optimiert und eine gute Wahl für die Speicherung von Dokumenten, Mediendateien, Sicherungskopien und Ähnlichem. Anfügeblobs ähneln Blockblobs, sind aber für Anfügevorgänge optimiert. Zum Aktualisieren eines Anfügeblobs muss an dessen Ende ein neuer Block hinzugefügt werden. Somit eignen sich Anfügeblobs sehr gut für Szenarien wie die Protokollierung, da hier neue Daten nur am Ende des Blobs geschrieben werden müssen. Seitenblobs sind für die Darstellung von IaaS-Festplatten und für die Unterstützung zufallsgesteuerter Schreibzugriffe optimiert und können bis zu 1 TB groß sein. Bei einer mit dem Netzwerk für virtuelle Azure-Computer verbundenen IaaS-Festplatte handelt es sich um eine als Seitenblob gespeicherte virtuelle Festplatte.

#### <a name="table-storage"></a>Table Storage
Table Storage ist ein NoSQL-Schlüssel-/Attributspeicher von Microsoft, der sich durch sein schemaloses Design von herkömmlichen relationalen Datenbanken unterscheidet. Da Datenspeicher keine Schemas aufweisen, können Sie Ihre Daten problemlos an die sich verändernden Anforderungen Ihrer Anwendung anpassen. Die hohe Benutzerfreundlichkeit von Table Storage ermöglicht Entwicklern eine rasche Anwendungserstellung. Bei Table Storage handelt es sich um einen Schlüssel-/Attribut-basierten Speicher. Das bedeutet, dass jeder Wert in einer Tabelle mit einem typisierten Eigenschaftennamen gespeichert wird. Der Eigenschaftenname kann für Filter und Auswahlkriterien verwendet werden. Eine Sammlung mit Eigenschaften und deren Werte bilden eine Entität. Da der Tabellenspeicher schemalos ist, können zwei Entitäten in der gleichen Tabelle unterschiedliche Sammlungen von Eigenschaften enthalten, und diese Eigenschaften können jeweils einen anderen Typ haben. Mit Table Storage können Sie flexible Datasets wie Benutzerdaten für Webanwendungen, Adressbücher, Geräteinformationen und jegliche Art von Metadaten speichern, die Ihr Dienst erfordert. Sie können eine beliebige Anzahl von Entitäten in einer Tabelle speichern, und ein Speicherkonto kann eine beliebige Anzahl von Tabellen enthalten (bis zur Speicherkapazitätsgrenze eines Speicherkontos).

#### <a name="queue-storage"></a>Queue Storage
Azure Queue Storage ermöglicht Cloud-Messaging zwischen Anwendungskomponenten. Bei der Entwicklung skalierbarer Anwendungen werden häufig einzelne Anwendungskomponenten entkoppelt, um eine unabhängige Skalierung zu ermöglichen. Queue Storage bietet asynchrones Messaging für die Kommunikation zwischen Anwendungskomponenten, egal ob diese in der Cloud, auf dem Desktop, auf einem lokalen Server oder einem mobilen Gerät ausgeführt werden. Queue Storage unterstützt auch die Verwaltung asynchroner Aufgaben und den Aufbau von Prozessworkflows.

### <a name="keyvault"></a>KeyVault
Der KeyVault-Ressourcenanbieter stellt die Verwaltung und Überwachung von Geheimnissen wie Kennwörtern und Zertifikaten bereit. Beispielsweise kann ein Mandant den KeyVault-Ressourcenanbieter während der Bereitstellung eines virtuellen Computers zum Angeben von Administratorkennwörtern oder Schlüsseln verwenden.

## <a name="role-based-access-control-rbac"></a>Rollenbasierte Zugriffssteuerung
Mit der rollenbasierten Zugriffssteuerung (Role Based Access Control, RBAC) können Sie autorisierten Benutzern, Gruppen und Diensten Systemzugriff gewähren, indem Sie ihnen Rollen auf Abonnement-, Ressourcengruppen- oder Ressourcenebene zuweisen. Jede Rolle definiert die Zugriffsebene, die ein Benutzer, eine Gruppe oder ein Dienst auf Microsoft Azure Stack-Ressourcen hat.

Die rollenbasierte Zugriffssteuerung in Azure verfügt über drei grundlegende Rollen, die für alle Ressourcentypen gelten: Besitzer, Mitwirkender und Leser. Besitzer verfügen über vollständigen Zugriff auf alle Ressourcen, einschließlich des Rechts, den Zugriff an andere Personen zu delegieren. Mitwirkende können alle Arten von Azure-Ressourcen erstellen und verwalten, aber keinen anderen Personen Zugriff gewähren. Leser können nur vorhandene Azure-Ressourcen anzeigen. Die verbleibenden RBAC-Rollen in Azure ermöglichen die Verwaltung von bestimmten Azure-Ressourcen. Beispielsweise ermöglicht die Rolle "Mitwirkender für virtuelle Computer" die Erstellung und Verwaltung von virtuellen Computern, nicht jedoch des virtuellen Netzwerks oder des Subnetzes, mit dem der virtuelle Computer eine Verbindung herstellt.

## <a name="usage-data"></a>Nutzungsdaten
Microsoft Azure Stack erfasst und aggregiert Nutzungsdaten für sämtliche Ressourcenanbieter und übermittelt sie an Azure für die Verarbeitung durch Azure Commerce. Die in Azure Stack gesammelten Nutzungsdaten können über eine REST-API angezeigt werden. Es gibt eine mit Azure übereinstimmende Mandanten-API sowie APIs für Anbieter und delegierte Anbieter, mit denen die Nutzungsdaten für alle Mandantenabonnements abgerufen werden können. Diese Daten können in ein externes Tool oder einen Dienst integriert oder für die Abrechnung oder verbrauchsbasierte Kostenzuteilung verwendet werden. Sobald die Nutzungsdaten von Azure Commerce verarbeitet wurden, können sie im Azure-Abrechnungsportal angezeigt werden.

## <a name="in-development-build-of-azure-stack-development-kit"></a>In der Entwicklung befindlicher Build des Azure Stack Development Kits
Anhand von Builds, die sich noch in der Entwicklung befinden, können Erstanwender die neueste Version des Azure Stack Development Kits bewerten. Dabei handelt es sich um inkrementelle Builds, die auf der letzten Hauptversion basieren. Während Hauptversionen weiterhin alle paar Monate veröffentlicht werden, erfolgt die Veröffentlichung von Builds in der Entwicklung zeitweise zwischen den Hauptversionen.

In der Entwicklung befindliche Builds bieten die folgenden Vorteile:
- Fehlerbehebungen
- Neue Funktionen
- Weitere Verbesserungen

## <a name="next-steps"></a>Nächste Schritte
[Bereitstellen des Azure Stack Development Kits](azure-stack-deploy.md)


