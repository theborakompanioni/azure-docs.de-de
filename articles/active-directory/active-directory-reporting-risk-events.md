---
title: Azure Active Directory-Risikoereignisse | Microsoft-Dokumentation
description: "In diesem Thema erhalten Sie eine ausführliche Übersicht über Risikoereignisse."
services: active-directory
keywords: Azure Active Directory Identity Protection, Sicherheit, Risiko, Risikostufe, Sicherheitsrisiko, Sicherheitsrichtlinie
author: MarkusVi
manager: femila
ms.assetid: fa2c8b51-d43d-4349-8308-97e87665400b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.translationtype: HT
ms.sourcegitcommit: 1e6fb68d239ee3a66899f520a91702419461c02b
ms.openlocfilehash: 71ab5cb02ac70871fb8207ab9220b45d1c842dde
ms.contentlocale: de-de
ms.lasthandoff: 08/16/2017

---
# <a name="azure-active-directory-risk-events"></a>Azure Active Directory-Risikoereignisse

Die allermeisten Sicherheitsverletzungen kommen vor, wenn Angreifer Zugriff auf eine Umgebung erhalten, indem sie die Identität eines Benutzers stehlen. Die Ermittlung von kompromittierten Identitäten ist keine einfache Aufgabe. Azure Active Directory verwendet adaptive Machine Learning-Algorithmen und -Heuristiken, um verdächtige Aktivitäten im Zusammenhang mit Ihren Benutzerkonten zu erkennen. Jede erkannte verdächtige Aktion wird in einem Datensatz mit dem Namen *Risikoereignis* gespeichert.

Derzeit werden von Azure Active Directory sechs Typen von Risikoereignissen erkannt:

- [Benutzer mit kompromittierten Anmeldeinformationen](#leaked-credentials) 
- [Anmeldungen von anonymen IP-Adressen](#sign-ins-from-anonymous-ip-addresses) 
- [Unmöglicher Ortswechsel zu atypischen Orten](#impossible-travel-to-atypical-locations) 
- [Anmeldungen von unbekannten Standorten](#sign-in-from-unfamiliar-locations)
- [Anmeldungen von infizierten Geräten](#sign-ins-from-infected-devices) 
- [Anmeldungen von IP-Adressen mit verdächtigen Aktivitäten](#sign-ins-from-ip-addresses-with-suspicious-activity) 


![Risikoereignis](./media/active-directory-reporting-risk-events/91.png)

Dieses Thema enthält eine ausführliche Übersicht darüber, was Risikoereignisse sind und wie Sie diese zum Schutz Ihrer Azure AD-Identitäten verwenden können.


## <a name="risk-event-types"></a>Risikoereignistypen

Die Risikoereignistyp-Eigenschaft ist eine Kennung für die verdächtige Aktivität, für die ein Risikoereignis-Datensatz erstellt wurde.  
Die laufenden Investitionen von Microsoft in den Erkennungsprozess führen zu:

- Verbesserungen an der Erkennungsgenauigkeit vorhandener Risikoereignisse 
- Neuen Risikoereignistypen, die künftig hinzugefügt werden

### <a name="leaked-credentials"></a>Kompromittierte Anmeldeinformationen

Wenn Internetkriminelle an gültige Kennwörter von berechtigten Benutzern gelangen, geben sie diese Anmeldeinformationen häufig weiter. Dies erfolgt normalerweise durch eine Veröffentlichung im Dark Web oder auf Past Sites oder durch den Handel oder Verkauf der Anmeldeinformationen auf dem Schwarzmarkt. Der Microsoft-Dienst für offengelegte Anmeldeinformationen sammelt Kombinationen aus Benutzername und Kennwort durch die Überwachung von öffentlichen und Dark Web-Websites und die Zusammenarbeit mit:

- Forschern
- Strafverfolgungsbehörden
- Sicherheitsteams bei Microsoft
- Anderen vertrauenswürdigen Quellen 

Wenn der Dienst Benutzername-Kennwort-Paare erwirbt, werden sie mit aktuell gültigen Anmeldeinformationen von AAD-Benutzern verglichen. Falls eine Übereinstimmung gefunden wird, bedeutet dies, dass das Kennwort eines Benutzers kompromittiert wurde, und es wird ein *Risikoereignis zu offengelegten Anmeldeinformationen* erstellt.

### <a name="sign-ins-from-anonymous-ip-addresses"></a>Anmeldungen von anonymen IP-Adressen

Bei dieser Art von Risikoereignis werden Benutzer identifiziert, die sich erfolgreich unter einer IP-Adresse angemeldet haben, die als anonyme Proxy-IP-Adresse identifiziert wurde. Diese Proxys werden von Personen verwendet, die die IP-Adresse ihres Geräts verbergen möchten, und können in böswilliger Absicht eingesetzt werden.


### <a name="impossible-travel-to-atypical-locations"></a>Unmöglicher Ortswechsel zu atypischen Orten

Mit dieser Art von Risikoereignis werden zwei Anmeldungen identifiziert, die von weit entfernten Orten durchgeführt wurden und bei denen mindestens einer der Orte aufgrund des bisherigen Verhaltens atypisch für den Benutzer ist. Außerdem ist der Zeitraum zwischen den beiden Anmeldungen kürzer als der Zeitraum, der für die Reise des Benutzers vom ersten zum zweiten Ort erforderlich wäre. Dies deutet darauf hin, dass ein anderer Benutzer die gleichen Anmeldeinformationen verwendet. 

Bei diesem Algorithmus für maschinelles Lernen werden offensichtliche „*falsche Positive*“ ignoriert, die zum unmögliche Ortswechsel beitragen, z.B. VPNs und regelmäßig von anderen Benutzern der Organisation verwendete Standorte.  Das System verfügt über einen anfänglichen Lernzeitraum von 14 Tagen, in dem das Anmeldeverhalten des neuen Benutzers erlernt wird.

### <a name="sign-in-from-unfamiliar-locations"></a>Anmeldungen von unbekannten Standorten

Diese Art von Risikoereignis berücksichtigt die Standorte der letzten Anmeldung (IP, Breiten-/Längengrad und ASN), um neue oder unbekannte Standorte zu ermitteln. Im System werden Informationen zu den vorherigen Standorten gespeichert, die von einem Benutzer genutzt wurden, und diese werden als „vertraute“ Standorte angesehen. Das Risikoereignis wird ausgelöst, wenn die Anmeldung von einem Standort aus erfolgt, der in der Liste der vertrauten Standorte noch nicht enthalten ist. Das System benötigt einen anfänglichen Lernzeitraum von 30 Tagen, in dem neue Standorte nicht als unbekannte Orte gekennzeichnet werden. Außerdem ignoriert das System Anmeldungen von vertrauten Geräten und von Standorten aus, die geografisch nahe an einem bekannten Speicherort liegen. 

### <a name="sign-ins-from-infected-devices"></a>Anmeldungen von infizierten Geräten

Bei dieser Art von Risikoereignis werden Anmeldungen von Geräten identifiziert, die mit Schadsoftware infiziert sind und für die bekannt ist, dass sie aktiv mit einem Botserver kommunizieren. Dies wird ermittelt, indem IP-Adressen den Benutzergeräts mit IP-Adressen korreliert werden, die in Kontakt mit einem Botserver gestanden haben. 

### <a name="sign-ins-from-ip-addresses-with-suspicious-activity"></a>Anmeldungen von IP-Adressen mit verdächtigen Aktivitäten
Bei dieser Art von Risikoereignis werden IP-Adressen identifiziert, für die eine hohe Zahl fehlgeschlagener Anmeldeversuche beobachtet wurde, und zwar mit mehreren Benutzerkonten innerhalb eines kurzen Zeitraums. Dies entspricht Datenverkehrsmustern von IP-Adressen, die von Angreifern verwendet werden, und es ist ein starker Hinweis darauf, dass Konten entweder bereits kompromittiert sind oder dass dies kurz bevorsteht. Dies ist ein Algorithmus für maschinelles Lernen, bei dem offensichtliche „*falsche Positive*“ ignoriert werden, z.B. IP-Adressen, die regelmäßig von anderen Benutzern in der Organisation verwendet werden.  Das System benötigt einen anfänglichen Lernzeitraum von 14 Tagen, in dem es das Anmeldeverhalten eines neuen Benutzers und neuen Mandanten erlernt.


## <a name="detection-type"></a>Erkennungstyp

Die Erkennungstyp-Eigenschaft ist ein Hinweis auf den Erkennungszeitrahmen (Echtzeit oder Offline) eines Risikoereignisses.  
Derzeit werden die meisten Risikoereignisse offline in einem Nachverarbeitungsvorgang nach dem Auftreten des Risikoereignisses erkannt.

In der folgenden Tabelle sind die jeweiligen Zeitspannen bis zur Anzeige eines bestimmten Erkennungstyps in einem entsprechenden Bericht aufgelistet:

| Erkennungstyp | Meldungslatenzzeit |
| --- | --- |
| Echtzeit | 5 bis 10 Minuten |
| Offline | 2 bis 4 Stunden |


Für die von Azure Active Directory erkannten Risikoereignistypen werden folgende Erkennungstypen unterstützt:

| Risikoereignistyp | Erkennungstyp |
| :-- | --- | 
| [Benutzer mit kompromittierten Anmeldeinformationen](#leaked-credentials) | Offline |
| [Anmeldungen von anonymen IP-Adressen](#sign-ins-from-anonymous-ip-addresses) | Echtzeit |
| [Unmöglicher Ortswechsel zu atypischen Orten](#impossible-travel-to-atypical-locations) | Offline |
| [Anmeldungen von unbekannten Standorten](#sign-in-from-unfamiliar-locations) | Echtzeit |
| [Anmeldungen von infizierten Geräten](#sign-ins-from-infected-devices) | Offline |
| [Anmeldungen von IP-Adressen mit verdächtigen Aktivitäten](#sign-ins-from-ip-addresses-with-suspicious-activity) | Offline|


## <a name="risk-level"></a>Risikostufe

Die Risikoebenen-Eigenschaft eines Risikoereignisses ist ein Hinweis auf den Schweregrad und die Zuverlässigkeit (Hoch, Mittel oder Niedrig) eines Risikoereignisses. Anhand dieser Eigenschaft können Sie die erforderlichen Maßnahmen priorisieren. 

Der Schweregrad des Risikoereignisses steht für die Stärke des Signals, das zum Vorhersagen der Kompromittierung einer Identität dient.  
Die Zuverlässigkeit gibt die Möglichkeit falsch positiver Ergebnisse an. 

Beispiel: 

* **Hoch:**Risikoereignis mit hohem Vertraulichkeitsgrad und hohem Schweregrad. Ereignisse dieser Art sind starke Indikatoren dafür, dass die Identität des Benutzers kompromittiert wurde, und für alle betroffenen Benutzerkonten sollten sofort Korrekturmaßnahmen eingeleitet werden.

* **Mittel:**Risikoereignis mit hohem Schweregrad, aber niedrigerem Vertraulichkeitsgrad (oder umgekehrt). Diese Ereignisse sind potenziell risikobehaftet, und für alle betroffenen Benutzerkonten sollten Korrekturmaßnahmen eingeleitet werden.

* **Niedrig:**Risikoereignis mit niedrigem Vertraulichkeits- und Schweregrad. Bei diesem Ereignis ist unter Umständen keine sofortige Aktion erforderlich, aber zusammen mit anderen Risikoereignissen kann auch dies ein starker Hinweis dafür sein, dass die Identität kompromittiert wurde.

![Risikostufe](./media/active-directory-reporting-risk-events/01.png)

### <a name="leaked-credentials"></a>Kompromittierte Anmeldeinformationen

Risikoereignisse vom Typ „Kompromittierte Anmeldeinformationen“ werden als **Hoch** eingestuft, da es sich hierbei um einen eindeutigen Hinweis darauf handelt, dass der Benutzername und das Kennwort für Angreifer zur Verfügung stehen.

### <a name="sign-ins-from-anonymous-ip-addresses"></a>Anmeldungen von anonymen IP-Adressen

Die Risikostufe für diese Art von Risikoereignis lautet **Mittel**, da eine anonyme IP-Adresse kein starker Hinweis auf eine Kompromittierung eines Kontos ist.  
Es wird empfohlen, dass Sie sich sofort an den Benutzer wenden, um zu überprüfen, ob dieser die anonyme IP-Adresse verwendet hat.


### <a name="impossible-travel-to-atypical-locations"></a>Unmöglicher Ortswechsel zu atypischen Orten

Der unmögliche Ortswechsel ist normalerweise ein zuverlässiger Hinweis darauf, dass sich ein Hacker erfolgreich anmelden konnte. Es kann aber zu falsch positiven Ergebnissen kommen, wenn ein Benutzer mit einem neuen Gerät unterwegs ist oder eine VPN-Verbindung nutzt, die von anderen Personen der Organisation normalerweise nicht verwendet wird. Eine weitere Quelle für falsch positive Ergebnisse sind Anwendungen, von denen Server-IPs fälschlicherweise als Client-IPs übergeben werden. Hierbei kann der Eindruck entstehen, dass Anmeldungen über das Rechenzentrum durchgeführt werden, in dem das Back-End der Anwendung gehostet wird (häufig sind dies Microsoft-Rechenzentren, die den Eindruck entstehen lassen, dass Anmeldungen über im Besitz von Microsoft befindliche IP-Adressen abgewickelt werden). Aufgrund dieser falsch positiven Ergebnisse ergibt sich die Risikostufe **Mittel** für dieses Risikoereignis.

> [!TIP]
> Sie können die Anzahl der gemeldeten falsch positiven Ergebnisse für diesen Risikoereignistyp verringern, indem Sie [Benannte Orte](active-directory-named-locations.md) konfigurieren. 

### <a name="sign-in-from-unfamiliar-locations"></a>Anmeldungen von unbekannten Standorten

Unbekannte Standorte können ein starker Hinweis darauf sein, dass ein Angreifer in der Lage ist, eine gestohlene Identität zu verwenden. Zu falsch positiven Ergebnissen kann es kommen, wenn ein Benutzer auf Reisen ist, ein neues Gerät ausprobiert oder eine neue VPN-Verbindung verwendet. Aufgrund dieser falsch positiven Ergebnisse ergibt sich als Risikostufe für diese Art von Ereignis **Mittel**.

### <a name="sign-ins-from-infected-devices"></a>Anmeldungen von infizierten Geräten

Bei diesem Risikoereignis werden IP-Adressen identifiziert, keine Benutzergeräte. Wenn sich hinter einer einzelnen IP-Adresse mehrere Geräte verbergen und nur einige davon von einem Botnetzwerk kontrolliert werden, kann es sein, dass dieses Ereignis fälschlicherweise bei Anmeldungen von anderen Geräten ausgelöst wird. Dies ist der Grund dafür, warum dieses Risikoereignis die Einstufung **Niedrig** erhält.  

Sie sollten den Benutzer kontaktieren und alle Geräte des Benutzers überprüfen. Es ist auch möglich, dass das persönliche Gerät eines Benutzers infiziert ist oder dass (wie bereits erwähnt) eine andere Person unter der gleichen IP-Adresse wie der Benutzer ein infiziertes Gerät verwendet hat. Infizierte Geräte sind häufig mit Schadsoftware infiziert, die von Antivirensoftware noch nicht identifiziert wurde. Dies kann auch auf schlechte Angewohnheiten des Benutzers hinweisen, die dazu geführt haben, dass das Gerät infiziert wurde.

Weitere Informationen dazu, wie Sie Infektionen durch Schadsoftware behandeln können, finden Sie im [Microsoft-Center zum Schutz vor Schadsoftware](http://go.microsoft.com/fwlink/?linkid=335773&clcid=0x409).


### <a name="sign-ins-from-ip-addresses-with-suspicious-activity"></a>Anmeldungen von IP-Adressen mit verdächtigen Aktivitäten

Es wird empfohlen, sich an den Benutzer zu wenden und zu überprüfen, ob dieser sich tatsächlich mit einer IP-Adresse angemeldet hat, die als verdächtig gekennzeichnet war. Die Risikostufe für diesen Ereignistyp lautet**Mittel**, da sich hinter einer IP-Adresse mehrere Geräte verbergen können, von denen nur einige Geräte für die verdächtige Aktivität verantwortlich sind. 


 
## <a name="next-steps"></a>Nächste Schritte

Risikoereignisse bilden die Grundlage für den Schutz Ihrer Azure AD-Identitäten. Azure AD kann derzeit sechs Arten von Risikoereignissen erkennen: 


| Risikoereignistyp | Risikostufe | Erkennungstyp |
| :-- | --- | --- |
| [Benutzer mit kompromittierten Anmeldeinformationen](#leaked-credentials) | Hoch | Offline |
| [Anmeldungen von anonymen IP-Adressen](#sign-ins-from-anonymous-ip-addresses) | Mittel | Echtzeit |
| [Unmöglicher Ortswechsel zu atypischen Orten](#impossible-travel-to-atypical-locations) | Mittel | Offline |
| [Anmeldungen von unbekannten Standorten](#sign-in-from-unfamiliar-locations) | Mittel | Echtzeit |
| [Anmeldungen von infizierten Geräten](#sign-ins-from-infected-devices) | Niedrig | Offline |
| [Anmeldungen von IP-Adressen mit verdächtigen Aktivitäten](#sign-ins-from-ip-addresses-with-suspicious-activity) | Mittel | Offline|

Wo finden Sie die Risikoereignisse, die in Ihrer Umgebung erkannt wurden?
Es gibt zwei Bereiche, in denen Sie gemeldete Risikoereignisse überprüfen:

 - **Azure AD-Berichterstellung** – Azure AD-Sicherheitsberichte umfassen Risikoereignisse. Weitere Informationen finden Sie unter [Sicherheitsbericht zu gefährdeten Benutzern](active-directory-reporting-security-user-at-risk.md) und unter [Sicherheitsbericht zu riskanten Anmeldevorgängen](active-directory-reporting-security-risky-sign-ins.md).

 - **Azure AD Identity Protection** – Auch die Berichtsfunktionen von [Azure Active Directory Identity Protection](active-directory-identityprotection.md) umfassen Risikoereignisse.
    

Obwohl die Erkennung von Risikoereignissen bereits ein wichtiger Aspekt zum Schutz Ihrer Identitäten darstellt, haben Sie auch die Möglichkeit, diese entweder manuell zu beheben oder durch Konfiguration von Richtlinien für bedingten Zugriff sogar automatisierte Antworten zu implementieren. Weitere Informationen finden Sie unter [Azure Active Directory Identity Protection](active-directory-identityprotection.md).
 

