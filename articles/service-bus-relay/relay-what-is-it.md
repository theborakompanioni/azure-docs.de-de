---
title: "Übersicht: Definition und Vorteile von Azure Relay | Microsoft-Dokumentation"
description: "Übersicht über Azure Relay"
services: service-bus-relay
documentationcenter: .net
author: banisadr
manager: timlt
editor: 
ms.assetid: 1e3e971d-2a24-4f96-a88a-ce3ea2b1a1cd
ms.service: service-bus-relay
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: get-started-article
ms.date: 03/09/2017
ms.author: babanisa;sethm
translationtype: Human Translation
ms.sourcegitcommit: 8a531f70f0d9e173d6ea9fb72b9c997f73c23244
ms.openlocfilehash: 9fd40892c77630bd4f0b7abf5c3458a6dc200402
ms.lasthandoff: 03/10/2017


---
# <a name="what-is-azure-relay"></a>Was ist Azure Relay?
Der Azure Relay-Dienst unterstützt Hybridanwendungen, indem er Ihnen die Möglichkeit bietet, WCF-Dienste in einem Unternehmensnetzwerk sicher in der öffentlichen Cloud bereitzustellen, ohne dass eine Firewallverbindung geöffnet werden muss oder tiefgreifende Änderungen an der unternehmensinternen Netzwerkinfrastruktur erforderlich werden. Relay unterstützt eine Vielzahl verschiedener Transportprotokolle und Webdienststandards.

Der Relaydienst unterstützt herkömmlichen, unidirektionalen, Anforderungs-/Antwort und Peer-to-Peer-Verkehr. Er unterstützt außerdem Ereignisverteilung im Internetumfang, um Veröffentlichen-/Abonnieren-Szenarios und bidirektionale Socketkommunikation für erhöhte Punkt-Effizienz zu ermöglichen. 

Im Relay des Datenübertragungsmusters stellt ein lokaler Dienst eine Verbindung zu dem Relaydienst über einen ausgehenden Port her und erstellt einen bidirektionalen Socket für die mit einer bestimmten Rendezvous-Adresse gekoppelte Kommunikation. Der Client kann dann mit dem lokalen Dienst durch das Senden von Verkehr an den Relaydienst unter Verwendung der Rendezvousadresse kommunizieren. Der Relaydienst „vermittelt“ dann Daten an den lokalen Dienst über einen bidirektionalen Socket, der für jeden Client dezidiert ist. Der Client benötigt keine direkte Verbindung zum lokalen Dienst, muss nicht wissen, wo der Dienst angesiedelt ist, und für den lokalen Dienst sind keine geöffneten eingehenden Ports in der Firewall notwendig.

Die Schlüsselfunktionselemente, die von Relay bereitgestellt werden, sind die bidirektionale, ungepufferte Kommunikation über Netzwerkgrenzen mit TCP-ähnlicher Drosselung, die Ermittlung von Endpunkten, der Konnektivitätsstatus und die überlagerte Endpunktsicherheit. Die Relayfunktionen weichen von Integrationstechnologien auf Netzwerkebene, z.B. VPN, darin ab, dass Relay einem einzelnen Anwendungsendpunkt auf einem einzelnen Computer zugeordnet werden kann. Die VPN-Technologie ist deutlich intrusiver, da sie mit einer Änderung der Netzwerkumgebung verbunden ist.

Azure Relay verfügt über zwei Funktionen:

1. [Hybridverbindungen](#hybrid-connections): Verwenden die offenen Standardwebsockets, die plattformübergreifende Szenarios zulassen.
2. [WCF-Relays](#wcf-relays): Verwenden Windows Communication Foundation, um Remoteprozeduraufrufe zuzulassen. WCF-Relay ist das ältere Relayangebot, das viele Kunden bereits mit ihren WCF-Programmiermodellen verwenden.

Hybridverbindungen und WCF-Relays aktivieren beide die sichere Verbindung mit Assets, die innerhalb eines Unternehmensnetzwerks vorhanden sind. Welcher Ansatz verwendet wird, richtet sich nach den jeweiligen Anforderungen. Dies ist in der folgenden Tabelle beschrieben:

|  | WCF-Relay | Hybridverbindungen |
| --- |:---:|:---:|
| **WCF** |x | |
| **.NET Core** | |x |
| **.NET Framework** |x |x |
| **JavaScript/NodeJS** | |x |
| **Standardbasierte offene Protokolle** | |x |
| **Mehrere RPC-Programmiermodelle** | |x |

## <a name="hybrid-connections"></a>Hybridverbindungen
Die [Hybridverbindungsfunktion von Azure Relay](relay-hybrid-connections-protocol.md) ist eine sichere Weiterentwicklung der vorhandenen Relay-Features mit offenem Protokoll, die auf jeder beliebigen Plattform und in jeder beliebigen Sprache mit einer grundlegenden WebSocket-Funktion implementiert werden kann. Dies schließt explizit die WebSocket-API in gängigen Webbrowsern ein. Hybridverbindungen basieren auf HTTP und WebSockets.

## <a name="wcf-relays"></a>WCF-Relays
Das WCF-Relay funktioniert für das gesamte .NET Framework (NETFX) sowie für WCF. Sie initiieren die Verbindung zwischen dem lokalen Dienst und dem Relaydienst mithilfe einer Sammlung von WCF-Bindungen vom Typ „Relay“. Im Prinzip werden die Relaybindungen neuen Transportbindungselementen zugeordnet, die entwickelt wurden, um WCF-Kanalkomponenten zu erstellen, die mit Service Bus in der Cloud integriert werden.

## <a name="service-history"></a>Dienstverlauf
Hybridverbindungen ersetzen die ehemalige ähnliche benannte Funktion „BizTalk Services“, die auf dem Azure Service Bus-WCF-Relay erstellt wurde. Die neue Funktion „Hybridverbindungen“ ergänzt die vorhandene WCF-Relayfunktion, und diese beiden Dienstfunktionen werden parallel im Relaydienst für die nächste Zeit vorhanden sein. Sie teilen sich ein allgemeines Gateway, sind jedoch andererseits verschiedene Implementierungen.

## <a name="next-steps"></a>Nächste Schritte:
* [Relay – Häufig gestellte Fragen](relay-faq.md)
* [Erstellen eines Namespaces](relay-create-namespace-portal.md)
* [Erste Schritte mit .NET](relay-hybrid-connections-dotnet-get-started.md)
* [Erste Schritte mit Node](relay-hybrid-connections-node-get-started.md)


