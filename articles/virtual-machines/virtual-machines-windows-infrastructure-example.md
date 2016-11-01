<properties
	pageTitle="Exemplarische Vorgehensweise für eine Beispielinfrastruktur | Microsoft Azure"
	description="Erfahren Sie mehr über die wichtigsten Entwurfs- und Implementierungsrichtlinien für die Bereitstellung einer Beispielinfrastruktur in Azure."
	documentationCenter=""
	services="virtual-machines-windows"
	authors="iainfoulds"
	manager="timlt"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/08/2016"
	ms.author="iainfou"/>

# Exemplarische Vorgehensweise für eine Azure-Beispielinfrastruktur

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

In diesem Artikel wird das Erstellen einer Beispielanwendungsinfrastruktur erläutert. Wir beschreiben das Entwerfen einer Infrastruktur für einen einfachen Onlineshop, wobei alle Richtlinien und Entscheidungen hinsichtlich der Namenskonventionen, Verfügbarkeit, virtuellen Netzwerke und Lastenausgleichsmodule relevant sind, und das eigentliche Bereitstellen Ihrer virtuellen Computer (VMs).


## Beispielworkload

Adventure Works Cycles möchte eine Anwendung für einen Onlineshop in Azure erstellen, die aus Folgendem besteht:

- Zwei IIS-Servern, auf denen der Client-Front-End in einer Webebene ausgeführt wird
- Zwei IIS-Servern, auf denen Daten und Aufträge in einer Anwendungsebene verarbeitet werden
- Zwei Microsoft SQL Server-Instanzen mit AlwaysOn-Verfügbarkeitsgruppen (zwei SQL-Server und ein Mehrheitsknotenzeuge) zum Speichern von Produktdaten und -aufträgen in einer Datenbankebene
- Zwei Active Directory-Domänencontroller für Kundenkonten und Lieferanten in einer Authentifizierungsebene
- Alle Server befinden sich in zwei Subnetzen:
	- einem Front-End-Subnetz für die Webserver
	- einem Back-End-Subnetz für die Anwendungsserver, den SQL-Cluster und die Domänencontroller

![Diagramm der verschiedenen Ebenen für die Anwendungsinfrastruktur](./media/virtual-machines-common-infrastructure-service-guidelines/example-tiers.png)

Eingehender sicherer Webdatenverkehr erfordert einen Lastenausgleich zwischen den Webservern, wenn die Kunden den Onlineshop durchsuchen. Datenverkehr für die Auftragsverarbeitung in Form von HTTP-Anforderungen der Webserver muss zwischen den Anwendungsservern ausgeglichen werden. Darüber hinaus muss die Infrastruktur für hohe Verfügbarkeit entworfen werden.

Der Entwurf muss Folgendes umfassen:

- Ein Azure-Abonnement und -Konto
- Eine einzelne Ressourcengruppe
- Speicherkonten
- Ein virtuelles Netzwerk mit zwei Subnetzen
- Verfügbarkeitsgruppen für die virtuellen Computer mit gleichen Rollen
- Virtuelle Computer

Alle oben aufgeführten Elemente werden anhand der folgenden Namenskonventionen benannt:

- Adventure Works Cycles verwendet **[IT-Workload]-[Standort]-[Azure-Ressource]** als Präfix.
	- In diesem Beispiel ist **azos** (Azure-Onlineshop) der Name der IT-Workload und **use** (USA, Osten 2) der Standort
- Speicherkonten folgen der Konvention „adventureazosusesa**[Beschreibung]**“.
	- Zur Eindeutigkeit wurde „adventure“ zum Präfix hinzugefügt. Speicherkontennamen dürfen keine Bindestriche enthalten.
- Virtuelle Netzwerke folgen der Konvention „AZOS-USE-VN**[Nummer]**“.
- Verfügbarkeitsgruppen folgen der Konvention „azos-use-as-**[Rolle]**“.
- Die Namen der virtuellen Computer folgen der Konvention „azos-use-vm-**[VM-Name]**“.


## Azure-Abonnements und -Konten

Adventure Works Cycles verwendet das Enterprise-Abonnement mit dem Namen „Adventure Works-Enterprise-Abonnement“ zur Abrechnung dieser IT-Workload.


## Speicherkonten

Adventure Works Cycles hat festgestellt, dass zwei Speicherkonten erforderlich sind:

- **adventureazosusesawebapp** für den Standardspeicher der Webserver, Anwendungsserver und Domänencontroller und deren Datenträger
- **adventureazosusesasql** für den Premium-Speicher der virtuellen SQL Server-Computer und deren Datenträger


## Virtuelles Netzwerk und Subnetze

Da das virtuelle Netzwerk keine permanente Verbindung mit dem lokalen Netzwerk von Adventure Work Cycles benötigt, fiel die Entscheidung auf ein virtuelles Netzwerk auf ausschließlicher Cloudbasis.

Sie haben ein virtuelles Netzwerk auf ausschließlicher Cloudbasis mit den folgenden Einstellungen über das Azure-Portal erstellt:

- Name: AZOS-USE-VN01
- Standort: East US 2
- Adressraum des virtuellen Netzwerks: 10.0.0.0/8
- Erstes Subnetz:
	- Name: FrontEnd
	- Adressraum: 10.0.1.0/24
- Zweites Subnetz:
	- Name: BackEnd
	- Adressraum: 10.0.2.0/24


## Verfügbarkeitsgruppen

Um hohe Verfügbarkeit für alle vier Ebenen des Onlineshops zu gewährleisten, entschied sich Adventure Works Cycles für vier Verfügbarkeitsgruppen:

- **azos-use-as-web** für die Webserver
- **azos-use-as-app** für die Anwendungsserver
- **azos-use-as-sql** für die SQL-Server
- **azos-use-as-dc** für Domänencontroller


## Virtuelle Computer

Adventure Works Cycles hat sich für die folgenden Namen für die virtuellen Azure-Computer entschieden:

- **azos-use-vm-web01** für den ersten Webserver
- **azos-use-vm-web02** für den zweiten Webserver
- **azos-use-vm-app01** für den ersten Anwendungsserver
- **azos-use-vm-app02** für den zweiten Anwendungsserver
- **azos-use-vm-sql01** für den ersten SQL Server im Cluster
- **azos-use-vm-sql02** für den zweiten SQL Server im Cluster
- **azos-use-vm-dc01** für den ersten Domänencontroller
- **azos-use-vm-dc02** für den zweiten Domänencontroller

Die resultierende Konfiguration sieht folgendermaßen aus.

![Endgültige in Azure bereitgestellte Anwendungsinfrastruktur](./media/virtual-machines-common-infrastructure-service-guidelines/example-config.png)

Diese Konfiguration umfasst:

- Ein virtuelles Netzwerk auf ausschließlicher Cloudbasis mit zwei Subnetzen (Front-End- und Back-End)
- Zwei Speicherkonten
- Vier Verfügbarkeitsgruppen, eine für jede Ebene des Onlineshops
- Die virtuellen Computer für die vier Ebenen
- Eine externe Lastenausgleichsgruppe für HTTPS-basierten Webdatenverkehr aus dem Internet an die Webserver
- Eine interne Lastenausgleichsgruppe für unverschlüsselten Webdatenverkehr von den Webservern an die Anwendungsserver
- Eine einzelne Ressourcengruppe


## Nächste Schritte

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]

<!---HONumber=AcomDC_0914_2016-->