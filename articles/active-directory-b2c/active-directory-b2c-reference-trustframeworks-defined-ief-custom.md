---
title: "Azure Active Directory B2C: Referenz – Vertrauensframeworks | Microsoft-Dokumentation"
description: Thema zu benutzerdefinierten Azure Active Directory B2C-Richtlinien
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/25/2017
ms.author: joroja
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: 019dd66d5abe4242ffb6cdc3276929dcd7f04466
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017


---

# <a name="defining-trust-frameworks-with-azure-ad-b2c-identity-experience-framework"></a>Definieren von Vertrauensframeworks mit Azure AD B2C Identity Experience Framework

Mit benutzerdefinierten Azure AD B2C-Richtlinien, für die das Identity Experience Framework genutzt wird, erhält Ihre Organisation einen zentralisierten Dienst, mit dem die Komplexität des Identitätsverbunds für eine große Interessengemeinschaft auf eine einzelne Vertrauensstellung und einen einzelnen Metadatenaustausch reduziert wird.

Hierfür ist es erforderlich, dass benutzerdefinierte Azure AD B2C-Richtlinien, für die das Identity Experience Framework genutzt wird, Ihnen die Beantwortung der folgenden Fragen ermöglichen:

- *Wie lauten die Richtlinien, die sich auf rechtliche Fragen, die Sicherheit, den Datenschutz und den Schutz von Daten beziehen und eingehalten werden müssen?*
- *Wer sind die Kontakte, und wie werden Benutzer zu anerkannten Teilnehmern?*
- *Wer sind die anerkannten Anbieter von Identitätsinformationen (Anspruchsanbieter), und was wird angeboten?*
- *Wer sind die anerkannten vertrauenden Seiten [und was ist dafür erforderlich (optional)]?*
- *Was sind die technischen „On the Wire“-Interoperabilitätsanforderungen für Teilnehmer?*
- *Wie lauten die betriebsbezogenen Runtimeregeln, die zum Austauschen von Informationen zur digitalen Identität durchgesetzt werden müssen?*

Zur Beantwortung all dieser Fragen wird für die benutzerdefinierte Azure AD B2C-Richtlinien, für die das Identity Experience Framework eingesetzt wird, das Vertrauensframework-Konstrukt (Trust Framework, TF) genutzt. Wir sehen und dieses Konstrukt und den damit erzielten Beitrag nun näher an.

## <a name="understanding-the-trust-framework-and-federation-management-foundation"></a>Grundlegendes zum Vertrauensframework und zur zugrunde liegenden Verbundverwaltung

Ein Konstrukt dieser Art sollte als geschriebene Spezifikation der Richtlinien für Identität, Sicherheit, Datenschutz und Schutz von Daten verstanden werden, die von Teilnehmern einer Interessengemeinschaft eingehalten werden müssen.

Die Verbundidentität ist eine Basis zur Erzielung von Endbenutzer-Identitätszusicherung auf Internetniveau.  Indem die Identitätsverwaltung an Drittanbieter delegiert wird, kann eine einzelne digitale Identität für einen Endbenutzer mit mehreren vertrauenden Seiten wiederverwendet werden.  

Für die Sicherstellung der Identität ist es erforderlich, dass Identitätsanbieter (IdPs) und Attributanbieter (AtPs) bestimmte Richtlinien und Methoden in Bezug auf die Sicherheit, den Datenschutz und den Betrieb einhalten.  Da keine direkten Untersuchungen durchgeführt werden können, müssen vertrauende Seiten (Relying Parties, RPs) Vertrauensstellungen mit den IdPs und AtPs entwickeln, die jeweils ausgewählt werden.  Da die Zahl von Konsumenten und Anbietern von Informationen zur digitalen Identität ständig steigt, ist es nicht mehr haltbar, für diese Vertrauensstellungen die Verwaltung als Paare fortzusetzen. Dies gilt auch für den paarweisen Austausch der technischen Metadaten, die für die Netzwerkverbindung benötigt werden.  Mit Verbund-Hubs konnten diese Probleme nur in eingeschränktem Maße gelöst werden.

Vertrauensframeworks (TFs) sind der Dreh- und Angelpunkt des OIX-Vertrauensframework-Modells (Open Identity Exchange), bei dem jede Interessengemeinschaft mit einer bestimmten Vertrauensframework-Spezifikation gesteuert wird. Mit einer TF-Spezifikation dieser Art wird Folgendes definiert:

- Die Sicherheits- und Datenschutzmetriken für die Interessengemeinschaft mit der folgenden Definition:
    - Die von Teilnehmern angebotenen bzw. benötigten Zusicherungsebenen (Levels of Assurance, LOA), also eine sortierte Gruppe mit Vertrauensbewertungen für die Authentizität der Informationen zur digitalen Identität.
    - Die von Teilnehmern angebotenen bzw. benötigten Schutzebenen (Levels of Protection, LOP), also eine sortierte Gruppe mit Vertrauensbewertungen für den Schutz der Informationen zur digitalen Identität, die von Teilnehmern der Interessengemeinschaft verwaltet wird.
- Die Beschreibung der Informationen zur digitalen Identität, die von Teilnehmern angeboten bzw. benötigt werden.
- Die technischen Richtlinien für die Erzeugung und Nutzung von Informationen zur digitalen Identität und somit zum Messen von LOA und LOP. Diese schriftlichen Richtlinien umfassen normalerweise die folgenden Kategorien von Richtlinien:
    - Richtlinien zur Identitätsüberprüfung: *Wie streng werden die Identitätsinformationen einer Person überprüft?*
    - Sicherheitsrichtlinien: *Wie gut ist die Integrität und Vertraulichkeit der Informationen geschützt?*
    - Datenschutzrichtlinien: *Welches Maß an Kontrolle hat ein Benutzer über seine personenbezogenen Informationen (Personally Identifiable Information, PII)*?
    - Richtlinien zur „Überlebensfähigkeit“ (Survivability): Kontinuität und Schutz von personenbezogenen Informationen, wenn ein Anbieter den Betrieb einstellt.
- Die technischen Profile für die Erzeugung und Nutzung von Informationen zur digitalen Identität. Diese Profile dienen den folgenden Zwecken:
    - Festlegen des Umfangs von Schnittstellen, für die Informationen zur digitalen Identität auf der angegebenen Zusicherungsebene (LOA) verfügbar sind
    - Beschreiben der technischen Anforderungen für die „On the Wire“-Interoperabilität
- Die Beschreibungen der verschiedenen Rollen, die Teilnehmer in der Gemeinschaft ausüben können (zusammen mit den Qualifikationen, die zum Ausüben dieser Rollen erforderlich sind)

Daher wird mit einer TF-Spezifikation gesteuert, wie Identitätsinformationen zwischen den Teilnehmern der Interessengemeinschaft ausgetauscht werden: vertrauende Seiten, Identitäts- und Attributanbieter und Attributverifizierer.

Im Zusammenhang mit diesem OIX-TF-Modell umfasst eine TF-Spezifikation ein oder mehrere Dokumente, die als Referenz für die Steuerung der Interessengemeinschaft dienen und die Durchsetzung und Nutzung von Informationen zur digitalen Identität innerhalb der Gemeinschaft regeln. Es handelt sich also um einen Satz von Richtlinien und Verfahren, die für das Herstellen von Vertrauen in die digitalen Identitäten für Onlinetransaktionen zwischen Mitgliedern einer Interessengemeinschaft ausgelegt sind.  **Mit einer TF-Spezifikation werden die Regeln zum Erstellen eines wettbewerbsfähigen Verbundökosystems für die Identität einer Gemeinschaft definiert.**

Heutzutage wird der Vorteil dieses Ansatzes eigentlich nicht mehr in Frage gestellt. Es bestehen keinerlei Zweifel, dass Vertrauensframework-Spezifikationen die Entwicklung von Ökosystemen für die digitale Identität mit verifizierbaren Sicherheits-, Zusicherungs- und Datenschutzmerkmalen ermöglichen, die dann für eine größere Zahl von Interessengemeinschaften wiederverwendet werden können.

Aus diesem Grund wird die Spezifikation für benutzerdefinierte Azure AD B2C-Richtlinien, für die das Identity Experience Framework genutzt wird, im Kern als Grundlage der Datendarstellung für ein TF verwendet, um die Interoperabilität zu vereinfachen.  

Bei benutzerdefinierten Azure AD B2C-Richtlinien, für die das Identity Experience Framework genutzt wird, wird eine TF-Spezifikation als Mischung aus für Menschen und für Maschinen lesbaren Daten dargestellt: Einige Abschnitte dieses Modells (normalerweise Abschnitte, die sich eher auf die Steuerung beziehen) sind als Verweise auf veröffentlichte Dokumentation zu Sicherheits- und Datenschutzrichtlinien und die dazugehörigen Verfahren (falls zutreffend) dargestellt. In anderen Abschnitten werden ausführlich die Konfigurationsmetadaten und Runtimeregeln zum Durchführen der betriebsbezogenen Automatisierung beschrieben.

## <a name="understanding-trust-framework-policies"></a>Grundlegendes zu Vertrauensframework-Richtlinien

Aus Implementierungssicht besteht die TF-Spezifikation aus benutzerdefinierten Azure AD B2C-Richtlinien, für die das Identity Experience Framework in einer Gruppe von Richtlinien genutzt wird, die eine vollständige Kontrolle über Identitätsverhalten und -oberflächen ermöglichen.  Mit benutzerdefinierten Azure AD B2C-Richtlinien in Verbindung mit dem Identity Experience Framework können Sie Ihr eigenes Vertrauensframework erstellen, indem Sie deklarative Richtlinien nutzen, mit denen Folgendes definiert und konfiguriert werden kann:

- Die Dokumentverweise, mit denen das Verbundökosystem für die Identität einer Gemeinschaft für das Vertrauensframework definiert wird. Dies sind Links zur Dokumentation zu Vertrauensframeworks. Die (vordefinierten) betriebsbezogenen Runtimeregeln oder die User Journeys, mit denen der Austausch und die Nutzung von Ansprüchen automatisiert bzw. gesteuert wird. Diese User Journeys sind einer Zusicherungsebene (LOA) und einer Schutzebene (LOP) zugeordnet. Eine Richtlinie kann daher über User Journeys mit unterschiedlichen LOAs (und LOPs) verfügen.
- Die Identitäts- und Attributanbieter oder die Anspruchsanbieter in der Interessengemeinschaft und die unterstützten technischen Profile zusammen mit der LOA/LOP-Akkreditierung („Out of Band“), die sich darauf bezieht.
- Die Integration mit Attributverifizierern oder Anspruchsanbietern.
- Die vertrauenden Seiten in der Gemeinschaft (nach Rückschluss).
- Die Metadaten zum Einrichten der Netzwerkkommunikation zwischen Teilnehmern. Diese Metadaten werden zusammen mit den technischen Profilen im Verlauf einer Transaktion verwendet, um die „On the Wire“-Interoperabilität zwischen der vertrauenden Seite und anderen Teilnehmern der Gemeinschaft herzustellen.
- Die Protokollkonvertierung, falls vorhanden (SAML, OAuth2, WS-Federation und OpenID Connect).
- Die Authentifizierungsanforderungen.
- Die Multi-Factor-Orchestrierung, falls vorhanden.
- Ein gemeinsames Schema für alle verfügbaren Ansprüche und Zuordnungen zu Teilnehmern einer Interessengemeinschaft.
- Alle Anspruchstransformationen – zusammen mit der möglichen Datenminimierung in diesem Kontext – zur Bewältigung des Austauschs und der Nutzung von Ansprüchen.
- Die Bindung und die Verschlüsselung.
- Der Anspruchsspeicher.

> [!NOTE]
> Wir bezeichnen alle möglichen Typen von Identitätsinformationen, die ausgetauscht werden können, als Ansprüche: Ansprüche in Bezug auf die Anmeldeinformationen eines Endbenutzers für die Authentifizierung, Identitätsüberprüfung, Kommunikationsgerät, physischer Standort, personenbezogene Attribute usw.  
>
> Wir verwenden den Ausdruck *Ansprüche* – anstelle von „Attribute“, wobei es sich um eine Teilmenge handelt –, da dies im Fall von Onlinetransaktionen keine Fakten sind, die von der vertrauenden Seite direkt verifiziert werden können. Es handelt sich eher um „Assertionen“ bzw. Ansprüche zu Fakten, für die die vertrauende Seite ein ausreichendes Maß an Vertrauen entwickeln muss, um die angeforderte Transaktion für den Endbenutzer zu gewähren.  
>
> Ein weiterer Grund ist die Tatsache, dass benutzerdefinierte Azure AD B2C-Richtlinien, die das Identity Experience Framework nutzen, für die Vereinfachung des Austauschs aller Arten von Informationen zur digitalen Identität auf einheitliche Weise ausgelegt sind. Dies gilt unabhängig davon, ob das zugrunde liegende Protokoll für die Benutzerauthentifizierung oder den Attributabruf definiert ist.  Ebenso verwenden wir den Ausdruck „Anspruchsanbieter“ zur allgemeinen Bezeichnung von Identitätsanbietern, Attributanbietern und Attributverifizierern, wenn nicht zwischen den jeweiligen spezifischen Funktionen unterschieden werden soll.   

Mit diesen Anbietern wird gesteuert, wie Identitätsinformationen zwischen einer vertrauenden Seite, Identitäts- und Attributanbietern und Attributverifizierern ausgetauscht werden. Sie steuern, welche Identitäts- und Attributanbieter für die Authentifizierung einer vertrauenden Seite erforderlich sind. Dies kann als domänenspezifische Sprache angesehen werden. Hierbei handelt es sich um eine Computersprache, die für eine bestimmte Anwendungsdomäne mit Vererbung, *if*-Anweisungen und Polymorphie spezialisiert ist.

Diese Richtlinien stellen den maschinenlesbaren Teil des TF-Konstrukts von benutzerdefinierten Azure AD B2C-Richtlinien dar, für die das Identity Experience Framework genutzt wird. Er enthält alle betriebsbezogenen Details, z.B. Metadaten und technische Profile von Anspruchsanbietern, Anspruchsschemadefinitionen, Anspruchstransformationsfunktionen und User Journeys. Diese Details werden eingefügt, um die Orchestrierung und Automatisierung zu vereinfachen.  

Es soll sich hierbei um *lebende Dokumente* in benutzerdefinierten Azure AD B2C-Richtlinien handeln, für die das Identity Experience Framework genutzt wird. Es ist nämlich relativ wahrscheinlich, dass sich der Inhalt im Laufe der Zeit im Hinblick auf die aktiven Teilnehmer ändert, die in den Richtlinien deklariert werden, und in einigen Situationen auch im Hinblick auf die Bedingungen für Teilnehmer.  
Das Einrichten und Verwalten des Verbunds wird erheblich vereinfacht, indem vertrauende Seiten gegenüber laufenden Neukonfigurationen von Vertrauensstellungen und der Konnektivität abgeschirmt werden, wenn unterschiedliche Anspruchsanbieter/-verifizierer dem Richtliniensatz (der die Gemeinschaft repräsentiert) beitreten oder diesen verlassen.

Die Interoperabilität stellt eine weitere signifikante Herausforderung dar, da zusätzliche Anspruchsanbieter/-verifizierer integriert werden müssen, weil vertrauende Seiten wahrscheinlich nicht alle erforderlichen Protokolle unterstützen. Mit benutzerdefinierten Azure AD B2C-Richtlinien, für die das Identity Experience Framework genutzt wird, wird dieses Problem gelöst, indem Branchenstandardprotokolle unterstützt und bestimmte User Journeys angewendet werden. Sie werden zum Umstellen von Anforderungen verwendet, wenn vertrauende Seiten und Attributanbieter nicht das gleiche Protokoll unterstützen.  

Users Journeys enthalten Protokollprofile und Metadaten, um die „On the Wire“-Interoperabilität zwischen der vertrauenden Seite und anderen Teilnehmern zu installieren.  Außerdem sind Regeln für die betriebsbezogene Runtime vorhanden, die auf Anforderung/Antwort-Nachrichten für den Austausch von Identitätsinformationen angewendet werden, um die Konformität mit veröffentlichten Richtlinien im Rahmen der TF-Spezifikation durchzusetzen. Das Konzept der User Journeys ist für die Anpassung der Kundenerfahrung von entscheidender Bedeutung und verdeutlicht, wie das System auf Protokollebene funktioniert.

Auf dieser Basis können Anwendungen und Portale der vertrauenden Seite je nach Kontext benutzerdefinierte Azure AD B2C Custom-Richtlinien mit Nutzung des Identity Experience Framework aufrufen, indem der Name einer bestimmten Richtlinie übergeben wird. So können genau das gewünschte Verhalten und der gewünschte Informationsaustausch erzielt werden, ohne dass hoher Aufwand anfällt oder ein Risiko besteht.

