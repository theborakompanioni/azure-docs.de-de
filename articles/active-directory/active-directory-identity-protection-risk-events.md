---
title: Azure Active Directory-Risikoereignisse | Microsoft-Dokumentation
description: "In diesem Thema erhalten Sie eine ausführliche Übersicht über Risikoereignisse."
services: active-directory
keywords: Azure Active Directory Identity Protection, Sicherheit, Risiko, Risikostufe, Sicherheitsrisiko, Sicherheitsrichtlinie
author: MarkusVi
manager: femila
ms.assetid: ce3b054c-7772-401f-9b06-3d24f6e7ca86
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 4dfd9690f2e3e017ca308649f79a84466d49d844
ms.openlocfilehash: 91ee5da0885fc0b044150e524757eac659a003a4


---
# <a name="azure-active-directory-risk-events"></a>Azure Active Directory-Risikoereignisse

Azure Active Directory hilft Ihnen dabei, Ihre Identitäten zu schützen. Ein Teil des Schutzprozesses ist die Erkennung von verdächtigen Aktivitäten im Zusammenhang mit Ihren Benutzerkonten. Die Erkennung basiert auf adaptiven Machine Learning-Algorithmen und -Heuristiken. Jede erkannte verdächtige Aktion wird in einem Datensatz mit dem Namen *Risikoereignis* gespeichert. 


![Risikoereignis](./media/active-directory-identity-protection-risk-events/91.png)


Derzeit werden von Azure Active Directory sechs Typen von Risikoereignissen erkannt.

| Risikoereignistyp | Risikostufe | Erkennungstyp |
| :-- | --- | --- |
| [Benutzer mit kompromittierten Anmeldeinformationen](#leaked-credentials) | Hoch | Offline |
| [Anmeldungen von anonymen IP-Adressen](#sign-ins-from-anonymous-ip-addresses) | Mittel | Echtzeit |
| [Unmöglicher Ortswechsel zu atypischen Orten](#impossible-travel-to-atypical-locations) | Mittel | Offline |
| [Anmeldungen von unbekannten Standorten](#sign-in-from-unfamiliar-locations) | Mittel | Echtzeit |
| [Anmeldungen von infizierten Geräten](#sign-ins-from-infected-devices) | Niedrig | Offline |
| [Anmeldungen von IP-Adressen mit verdächtigen Aktivitäten](#sign-ins-from-ip-addresses-with-suspicious-activity) | Mittel | Offline|

## <a name="risk-level"></a>Risikostufe

Die Risikoebenen-Eigenschaft ist ein Hinweis auf den Schweregrad (Hoch, Mittel oder Niedrig) eines Risikoereignisses. Anhand dieser Eigenschaft können Sie die erforderlichen Maßnahmen priorisieren. 

Der Schweregrad des Risikoereignisses steht für die Stärke des Signals, das zum Vorhersagen der Kompromittierung einer Identität dient, kombiniert mit dem damit üblicherweise verbundenen „Rauschen“.

* **Hoch:**Risikoereignis mit hohem Vertraulichkeitsgrad und hohem Schweregrad. Ereignisse dieser Art sind starke Indikatoren dafür, dass die Identität des Benutzers kompromittiert wurde, und für alle betroffenen Benutzerkonten sollten sofort Korrekturmaßnahmen eingeleitet werden.

* **Mittel:**Risikoereignis mit hohem Schweregrad, aber niedrigerem Vertraulichkeitsgrad (oder umgekehrt). Diese Ereignisse sind potenziell risikobehaftet, und für alle betroffenen Benutzerkonten sollten Korrekturmaßnahmen eingeleitet werden.

* **Niedrig:**Risikoereignis mit niedrigem Vertraulichkeits- und Schweregrad. Bei diesem Ereignis ist unter Umständen keine sofortige Aktion erforderlich, aber zusammen mit anderen Risikoereignissen kann auch dies ein starker Hinweis dafür sein, dass die Identität kompromittiert wurde.

![Risikostufe](./media/active-directory-identity-protection-risk-events/01.png)


## <a name="detection-type"></a>Erkennungstyp

Die Erkennungstyp-Eigenschaft ist ein Hinweis auf den Erkennungszeitrahmen (Echtzeit, Offline) eines Risikoereignisses.  
Derzeit werden die meisten Risikoereignisse offline in einem Nachverarbeitungsvorgang nach dem Auftreten des Risikoereignisses erkannt.

In der folgenden Tabelle sind die jeweiligen Zeitspannen bis zur Anzeige eines bestimmten Erkennungstyps in einem entsprechenden Bericht aufgelistet.

| Erkennungstyp | Meldungslatenzzeit |
| --- | --- |
| Echtzeit | 5 bis 10 Minuten |
| Offline | 2 bis 4 Stunden |



## <a name="risk-event-types"></a>Risikoereignistypen

Die Risikoereignistyp-Eigenschaft ist eine Kennung für die verdächtige Aktivität, für die ein Risikoereignis-Datensatz erstellt wurde.  
Derzeit werden von Azure Active Directory sechs Ereignistypen erkannt.

Die laufenden Investitionen von Microsoft in den Erkennungsprozess führen zu:

- Verbesserungen an der Erkennungsgenauigkeit vorhandener Risikoereignisse 
- Neuen Risikoereignistypen, die künftig hinzugefügt werden

### <a name="leaked-credentials"></a>Kompromittierte Anmeldeinformationen
Kompromittierte Anmeldeinformationen sind Anmeldeinformationen, für die von unseren Microsoft-Sicherheitsexperten herausgefunden wird, dass sie öffentlich im Dark Web gepostet werden. Diese Anmeldeinformationen sind dort normalerweise in Klartext zu finden. Sie werden mit Azure AD-Anmeldeinformationen verglichen. Wenn sich eine Übereinstimmung ergibt, werden sie in Identity Protection als „Kompromittierte Anmeldeinformationen“ gemeldet.

Risikoereignisse vom Typ „Kompromittierte Anmeldeinformationen“ werden als Risikoereignisse mit dem Schweregrad „Hoch“ eingestuft, da es sich hierbei um einen eindeutigen Hinweis darauf handelt, dass der Benutzername und das Kennwort für Angreifer zur Verfügung stehen.

### <a name="sign-ins-from-anonymous-ip-addresses"></a>Anmeldungen von anonymen IP-Adressen
Bei dieser Art von Risikoereignis werden Benutzer identifiziert, die sich erfolgreich unter einer IP-Adresse angemeldet haben, die als anonyme Proxy-IP-Adresse identifiziert wurde. Diese Proxys werden von Personen verwendet, die die IP-Adresse ihres Geräts verbergen möchten, und können in böswilliger Absicht eingesetzt werden.

Es wird empfohlen, dass Sie sich sofort an den Benutzer wenden, um zu überprüfen, ob dieser die anonyme IP-Adresse verwendet hat. Die Risikostufe für diese Art von Risikoereignis lautet**Mittel**, da eine anonyme IP-Adresse kein starker Hinweis auf eine Kompromittierung eines Kontos ist.


### <a name="impossible-travel-to-atypical-locations"></a>Unmöglicher Ortswechsel zu atypischen Orten
Mit dieser Art von Risikoereignis werden zwei Anmeldungen identifiziert, die von weit entfernten Orten durchgeführt wurden und bei denen mindestens einer der Orte aufgrund des bisherigen Verhaltens atypisch für den Benutzer ist. Außerdem ist der Zeitraum zwischen den beiden Anmeldungen kürzer als der Zeitraum, der für die Reise des Benutzers vom ersten zum zweiten Ort erforderlich wäre. Dies deutet darauf hin, dass ein anderer Benutzer die gleichen Anmeldeinformationen verwendet. 

Bei diesem Algorithmus für maschinelles Lernen werden offensichtliche „*falsche Positive*“ ignoriert, die zum unmögliche Ortswechsel beitragen, z.B. VPNs und regelmäßig von anderen Benutzern der Organisation verwendete Standorte.  Das System verfügt über einen anfänglichen Lernzeitraum von 14 Tagen, in dem das Anmeldeverhalten des neuen Benutzers erlernt wird.

Der unmögliche Ortswechsel ist normalerweise ein zuverlässiger Hinweis darauf, dass sich ein Hacker erfolgreich anmelden konnte. Es kann aber zu falsch positiven Ergebnissen kommen, wenn ein Benutzer mit einem neuen Gerät unterwegs ist oder eine VPN-Verbindung nutzt, die von anderen Personen der Organisation normalerweise nicht verwendet wird. Eine weitere Quelle für falsch positive Ergebnisse sind Anwendungen, von denen Server-IPs fälschlicherweise als Client-IPs übergeben werden. Hierbei kann der Eindruck entstehen, dass Anmeldungen über das Rechenzentrum durchgeführt werden, in dem das Back-End der Anwendung gehostet wird (häufig sind dies Microsoft-Rechenzentren, die den Eindruck entstehen lassen, dass Anmeldungen über im Besitz von Microsoft befindliche IP-Adressen abgewickelt werden). Aufgrund dieser falsch positiven Ergebnisse ergibt sich die Risikostufe **Mittel** für das Risikoereignis.

### <a name="sign-in-from-unfamiliar-locations"></a>Anmeldungen von unbekannten Standorten
Bei dieser Art von Risikoereignis handelt es sich um ein Verfahren zur Echtzeitauswertung von Anmeldungen, bei dem die letzten Standorte (IP, Breiten-/Längengrad und ASN) berücksichtigt werden, um neue oder unbekannte Orte zu ermitteln. Im System werden Informationen zu den vorherigen Standorten gespeichert, die von einem Benutzer genutzt wurden, und diese werden als „vertraute“ Standorte angesehen. Das Risikoereignis wird ausgelöst, wenn die Anmeldung von einem Standort aus erfolgt, der in der Liste der vertrauten Standorte noch nicht enthalten ist. Das System benötigt einen anfänglichen Lernzeitraum von 14 Tagen, in dem neue Standorte nicht als unbekannte Orte gekennzeichnet werden. Außerdem ignoriert das System Anmeldungen von vertrauten Geräten und von Standorten aus, die geografisch nahe an einem bekannten Speicherort liegen. 

Unbekannte Standorte können ein starker Hinweis darauf sein, dass ein Angreifer versucht, eine gestohlene Identität zu verwenden. Zu falsch positiven Ergebnissen kann es kommen, wenn ein Benutzer auf Reisen ist, ein neues Gerät ausprobiert oder eine neue VPN-Verbindung verwendet. Aufgrund dieser falsch positiven Ergebnisse ergibt sich als Risikostufe für diese Art von Ereignis**Mittel**.



### <a name="sign-ins-from-infected-devices"></a>Anmeldungen von infizierten Geräten
Bei dieser Art von Risikoereignis werden Anmeldungen von Geräten identifiziert, die mit Schadsoftware infiziert sind und für die bekannt ist, dass sie aktiv mit einem Botserver kommunizieren. Dies wird ermittelt, indem IP-Adressen den Benutzergeräts mit IP-Adressen korreliert werden, die in Kontakt mit einem Botserver gestanden haben. 

Bei diesem Risikoereignis werden IP-Adressen identifiziert, keine Benutzergeräte. Wenn sich hinter einer einzelnen IP-Adresse mehrere Geräte verbergen und nur einige davon von einem Botnetzwerk kontrolliert werden, kann es sein, dass dieses Ereignis bei Anmeldungen von anderen Geräten fälschlicherweise ausgelöst wird. Dies der Grund dafür, warum dieses Risikoereignis die Einstufung **Niedrig** erhält.  

Sie sollten den Benutzer kontaktieren und alle Geräte des Benutzers überprüfen. Es ist auch möglich, dass das persönliche Gerät eines Benutzers infiziert ist oder dass (wie bereits erwähnt) eine andere Person unter der gleichen IP-Adresse wie der Benutzer ein infiziertes Gerät verwendet hat. Infizierte Geräte sind häufig mit Schadsoftware infiziert, die von Antivirensoftware noch nicht identifiziert wurde. Dies kann auch auf schlechte Angewohnheiten des Benutzers hinweisen, die dazu geführt haben, dass das Gerät infiziert wurde.

Weitere Informationen dazu, wie Sie Infektionen durch Schadsoftware behandeln können, finden Sie im [Microsoft-Center zum Schutz vor Schadsoftware](http://go.microsoft.com/fwlink/?linkid=335773&clcid=0x409).


### <a name="sign-ins-from-ip-addresses-with-suspicious-activity"></a>Anmeldungen von IP-Adressen mit verdächtigen Aktivitäten
Bei dieser Art von Risikoereignis werden IP-Adressen identifiziert, für die eine hohe Zahl fehlgeschlagener Anmeldeversuche beobachtet wurde, und zwar mit mehreren Benutzerkonten innerhalb eines kurzen Zeitraums. Dies entspricht Datenverkehrsmustern von IP-Adressen, die von Angreifern verwendet werden, und es ist ein starker Hinweis darauf, dass Konten entweder bereits kompromittiert sind oder dass dies kurz bevorsteht. Dies ist ein Algorithmus für maschinelles Lernen, bei dem offensichtliche „*falsche Positive*“ ignoriert werden, z.B. IP-Adressen, die regelmäßig von anderen Benutzern in der Organisation verwendet werden.  Das System benötigt einen anfänglichen Lernzeitraum von 14 Tagen, in dem es das Anmeldeverhalten eines neuen Benutzers und neuen Mandanten erlernt.

Es wird empfohlen, sich an den Benutzer zu wenden und zu überprüfen, ob dieser sich tatsächlich mit einer IP-Adresse angemeldet hat, die als verdächtig gekennzeichnet war. Die Risikostufe für diesen Ereignistyp lautet**Mittel**, da sich hinter einer IP-Adresse mehrere Geräte verbergen können, von denen nur einige Geräte für die verdächtige Aktivität verantwortlich sind. 





## <a name="azure-ad-anomalous-activity-reports"></a>Azure AD-Berichte zu anomalen Aktivitäten

Im klassischen Azure-Portal sind einige der Risikoereignisse bereits über die Azure AD-Berichte zu anomalen Aktivitäten verfügbar. 

Die Tabelle unten enthält die verschiedenen Risikoereignistypen und den entsprechenden **Azure AD-Bericht zu anomalen Aktivitäten** . 

| Typ des Identity Protection-Risikoereignisses | Zugehöriger Azure AD-Bericht zu anomalen Aktivitäten |
|:--- |:--- |
| Kompromittierte Anmeldeinformationen |Benutzer mit kompromittierten Anmeldeinformationen |
| Unmöglicher Ortswechsel zu atypischen Orten |Irreguläre Anmeldeaktivitäten |
| Anmeldungen von infizierten Geräten |Anmeldungen von möglicherweise infizierten Geräten |
| Anmeldungen von anonymen IP-Adressen |Anmeldungen von unbekannten Quellen |
| Anmeldungen von IP-Adressen mit verdächtigen Aktivitäten |Anmeldungen von IP-Adressen mit verdächtigen Aktivitäten |
| Anmeldungen von unbekannten Standorten |- |
| Sperrereignisse |- |

Die folgenden Azure AD-Berichte zu anomalen Aktivitäten sind nicht als Risikoereignisse in Azure AD Identity Protection enthalten und daher auch nicht über Identity Protection verfügbar. Diese Berichte sind im klassischen Azure-Portal weiterhin verfügbar. Sie werden jedoch künftig nach und nach durch Risikoereignisse in Identity Protection ersetzt.

* Anmeldungen nach mehreren Fehlern
* Anmeldungen aus mehreren geografischen Regionen






## <a name="next-steps"></a>Nächste Schritte
* [Azure Active Directory Identity Protection](active-directory-identityprotection.md)




<!--HONumber=Jan17_HO3-->


