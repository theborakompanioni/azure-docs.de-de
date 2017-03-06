---
title: "Microsoft Threat Modeling Tool – Azure | Microsoft-Dokumentation"
description: "Hauptseite für das Microsoft Threat Modeling Tool mit Maßnahmen für größten erzeugten Bedrohungen"
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2017
ms.author: rodsan
translationtype: Human Translation
ms.sourcegitcommit: 2ad59f9b463671e2b74708871a9a9faae9bd24eb
ms.openlocfilehash: b75de472508bbf44c6e02e0d751054f37b0e1b8c
ms.lasthandoff: 02/21/2017


---

# <a name="microsoft-threat-modeling-tool"></a>Microsoft Threat Modeling Tool 
Das Threat Modeling Tool ist ein Kernelement im Microsoft Security Development Lifecycle (SDL). Es ermöglicht Softwarearchitekten, potenzielle Sicherheitslücken früh zu identifizieren und zu entschärfen, wenn sie relativ einfach und kostengünstig gelöst werden können. Daher werden mit dem Tool die Gesamtkosten der Entwicklung erheblich reduziert. Außerdem wurde das Tool nicht speziell für Sicherheitsexperten entwickelt. Es erleichtert die Modellierung von Bedrohungen für alle Entwickler, indem klare Leitlinien zum Erstellen und Analysieren von Gefahrenmodellen bereitgestellt werden. 

Das Tool ermöglicht Folgendes:
* Informationen zum Entwurf der Sicherheit von Systemen kommunizieren
* Diese Entwürfe mit bewährten Methoden auf potenzielle Sicherheitsprobleme analysieren
* Gegenmaßnahmen für Sicherheitsprobleme vorschlagen und verwalten

Nachfolgend sind einige Funktionen und Innovationen des Tools aufgeführt:
* **Automatisierung:** Anleitungen und Feedback bei der Erstellung eines Modells
* **STRIDE pro Element:** Geführte Analyse der Bedrohungen und Gegenmaßnahmen
* **Berichte:** Sicherheitsaktivitäten und Tests in der Überprüfungsphase
* **Einzigartige Methodik:** Ermöglicht es Benutzern, Bedrohungen besser zu visualisieren und zu verstehen
* **Speziell für Entwickler konzipiert und auf Software ausgerichtet:** Viele Ansätze zielen auf Objekte oder Angreifer ab. Bei uns steht die Software im Mittelpunkt. Unsere Basis sind Aktivitäten, mit denen alle Softwareentwickler und -architekten vertraut sind, z.B. das Zeichnen von Bildern für ihre Softwarearchitektur.
* **Fokus auf der Entwurfsanalyse:** Der Begriff „Modellierung von Bedrohungen“ kann sich auf eine Technik zur Analyse von Anforderungen oder Entwürfen beziehen. In manchen Fällen bezieht er sich auf eine komplexe Mischung aus beidem. Der Microsoft SDL-Ansatz für die Modellierung von Bedrohungen ist eine fokussierte Entwurfsanalysemethode.

## <a name="threats"></a>Bedrohungen

Mit dem Threat Modeling Tool können Sie Fragen wie die folgenden beantworten:

* Wie kann ein Angreifer die Authentifizierungsdaten ändern?
* Welche Auswirkungen hat es, wenn ein Angreifer die Benutzerprofildaten lesen kann?
* Was geschieht, wenn der Zugriff auf die Benutzerprofildatenbank verweigert wird?

Um Ihnen beim Formulieren dieser Arten von gezielten Fragen besser helfen zu können, verwendet Microsoft das STRIDE-Modell, in dem die verschiedenen Typen von Bedrohungen kategorisiert und Gespräche über die allgemeine Sicherheit vereinfacht werden.

| Kategorie | Beschreibung |
| -------- | ----------- |
| Spoofing | Umfasst unberechtigten Zugriff auf und die anschließende Verwendung der Authentifizierungsinformationen eines anderen Benutzers, z.B. Benutzername und Kennwort. |
| Manipulation | Umfasst die böswillige Änderung von Daten. Beispiele sind nicht autorisierte Änderungen von persistenten Daten, z.B. der Daten in einer Datenbank, und die Änderung von Daten bei der Übertragung zwischen zwei Computern über ein offenes Netzwerk wie das Internet. |
| Nichtanerkennung | Bezieht sich auf Benutzer, die die Ausführung einer Aktion abstreiten, ohne dass das Gegenteil bewiesen werden kann – ein Benutzer führt z.B. einen unzulässigen Vorgang in einem System aus, das keine Möglichkeit bietet, nicht zulässige Vorgänge zu verfolgen. Nichtabstreitbarkeit bezieht sich auf die Fähigkeit eines Systems, auf Bedrohungen durch Nichtanerkennung zu reagieren. Beispiel: Ein Benutzer, der einen Artikel erwirbt, muss möglicherweise für den Empfang des Artikels unterschreiben. Der Anbieter kann dann die unterschriebene Bestätigung als Nachweis verwenden, dass der Benutzer das Paket empfangen hat. |
| Veröffentlichung von Informationen | Umfasst die Veröffentlichung von Informationen für Personen, für die entsprechender Zugriff nicht vorgesehen ist – wenn beispielsweise Benutzer eine Datei lesen können, ohne dass ihnen Zugriff darauf gewährt wurde, oder ein Eindringling Daten während der Übertragung zwischen zwei Computern lesen kann. |
| Denial of Service | Bei DoS-Angriffen (Denial of Service) wird berechtigten Benutzern der Zugriff auf einen Dienst verweigert, indem z.B. ein Webserver vorübergehend nicht verfügbar oder nicht nutzbar gemacht wird. Sie müssen sich vor bestimmten Arten von DoS-Angriffen schützen, einfach nur um die Verfügbarkeit und Zuverlässigkeit des Systems zu verbessern. |
| Rechteerweiterungen | Ein nicht berechtigter Benutzer erhält privilegierten Zugriff und verfügt somit über ausreichende Zugriffsrechte, um das gesamte System zu kompromittieren oder zu zerstören. Zu Bedrohungen durch Rechteerweiterungen gehören Situationen, in denen ein Angreifer effektiv in den gesamten Systemschutz eingedrungen ist und Teil des vertrauenswürdigen Systems selbst geworden ist – eine wirklich gefährliche Situation. |

## <a name="mitigations"></a>Gegenmaßnahmen

Die Gegenmaßnahmen im Threat Modeling Tool sind gemäß dem Sicherheitsrahmen für Webanwendungen in folgende Kategorien unterteilt:

| Kategorie | Beschreibung |
| -------- | ----------- |
| [Überwachung und Protokollierung](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool-auditing-and-logging) | Wer hat was wann gemacht? Überwachung und Protokollierung beziehen sich darauf, wie Ihre Anwendung Sicherheitsereignisse aufzeichnet. |
| [Authentifizierung](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool-authentication) | Wer sind Sie? Authentifizierung ist der Prozess, in dem eine Entität die Identität einer anderen Entität beweist, in der Regel durch Anmeldeinformationen wie Benutzername und Kennwort. |
| [Autorisierung](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool-authorization) | Was können Sie tun? Die Autorisierung legt fest, wie Ihre Anwendung Zugriffssteuerung für Ressourcen und Vorgänge bereitstellt. | 
| [Kommunikationssicherheit](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool-communication-security) | Mit wem kommunizieren Sie? Durch Kommunikationssicherheit wird gewährleistet, dass die gesamte Kommunikation so sicher wie möglich abläuft. |
| [Konfigurationsverwaltung](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool-configuration-management) | Mit welchem Konto wird die Anwendung ausgeführt? Mit welchen Datenbanken stellt sie Verbindungen her? Wie wird Ihre Anwendung verwaltet? Wie werden diese Einstellungen gesichert? Die Konfigurationsverwaltung bezieht sich darauf, wie Ihre Anwendung diese Betriebsprobleme behandelt. | 
| [Kryptografie](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool-cryptography) | Wie werden geheime Schlüssel aufbewahrt (Vertraulichkeit)? Wie sichern Sie Ihre Daten oder Bibliotheken gegen Manipulationen (Integrität)? Wie stellen Sie Startwerte für Zufallswerte bereit, die kryptografisch sicher sein müssen? Kryptografie bezieht sich darauf, wie die Anwendung Vertraulichkeit und Integrität erzwingt. | 
| [Verwaltung von Ausnahmen](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool-exception-management) | Wie reagiert Ihre Anwendung, wenn beim Aufruf einer Methode in der Anwendung ein Fehler auftritt? Wie viel legen Sie offen? Geben Sie verständliche Fehlerinformationen an Endbenutzer zurück? Geben Sie wertvolle Ausnahmeinformationen an den Aufrufer zurück? Wird die Anwendung ordnungsgemäß abgebrochen? |
| [Eingabeüberprüfung](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool-input-validation) | Woher wissen Sie, dass die Eingaben, die Ihre Anwendung empfängt, gültig und sicher sind? Die Eingabeüberprüfung bezieht sich darauf, wie Ihre Anwendung Eingaben vor der weiteren Verarbeitung filtert, bereinigt oder ablehnt. Sie können Eingaben über Einstiegspunkte einschränken und Ausgaben über Ausgabepunkte codieren. Vertrauen Sie Daten aus Quellen wie Datenbanken und Dateifreigaben? |
| [Sensible Daten](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool-sensitive-data) | Wie werden in Ihrer Anwendung sensible Daten behandelt? Sensible Daten beziehen sich darauf, wie Ihre Anwendung Daten behandelt, die im Arbeitsspeicher, im Netzwerk oder in permanenten Speichern geschützt werden müssen. | 
| [Sitzungsverwaltung](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool-session-management) | Wie verarbeitet und schützt Ihre Anwendung Benutzersitzungen? Eine Sitzung bezieht sich auf eine Reihe von zusammengehörenden Interaktionen zwischen einem Benutzer und Ihre Webanwendung. | 

Damit können Sie Folgendes identifizieren: 

* Wo werden die häufigsten Fehler gemacht
* Wo sind die am besten umsetzbaren Verbesserungen möglich

So können Sie diese Kategorien verwenden, um Ihre Bemühungen um Sicherheit zu konzentrieren und zu priorisieren. Wenn Sie z.B. wissen, dass die häufigsten Sicherheitsprobleme in den Kategorien Eingabeüberprüfung, Authentifizierung und Autorisierung auftreten, können Sie mit diesen Kategorien beginnen. Klicken Sie [hier](https://www.google.com/patents/US7818788), um weitere Informationen zu erhalten.

## <a name="next-steps"></a>Nächste Schritte
Nutzen Sie diese nützlichen Links, um gleich heute zu beginnen:
* [Downloadlink](https://www.microsoft.com/download/details.aspx?id=49168)
* [Erste Schritte](https://msdn.microsoft.com/magazine/dd347831.aspx)
* [Zentrale Schulung](https://www.microsoft.com/download/details.aspx?id=16420)

In diesem Zusammenhang können Sie sich auch ansehen, was einige Threat Modeling Tool-Experten getan haben:
* [Threats Manager](https://simoneonsecurity.com/threatsmanagersetup-v1-5-10/)
* [Blog zum Thema Sicherheit von Simone Curzi](https://simoneonsecurity.com/)
