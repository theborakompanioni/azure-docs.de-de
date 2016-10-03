<properties
	pageTitle="Azure Active Directory Identity Protection | Microsoft Azure"
	description="Erfahren Sie, wie Sie mit Azure AD Identity Protection für Angreifer die Möglichkeit einschränken können, eine kompromittierte Identität oder ein Gerät auszunutzen, und wie Sie eine Identität oder ein Gerät schützen, das zuvor vermutlich oder mit Sicherheit kompromittiert war."
	services="active-directory"
	keywords="Azure Active Directory Identity Protection, Cloud App Discovery, Verwalten von Anwendungen, Sicherheit, Risiko, Risikostufe, Sicherheitsrisiko, Sicherheitsrichtlinie"
	documentationCenter=""
	authors="markusvi"
	manager="femila"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/20/2016"
	ms.author="markvi"/>

#Azure Active Directory Identity Protection 

Azure Active Directory Identity Protection ist ein Feature der Azure AD Premium P2-Edition, das Ihnen eine konsolidierte Übersicht über Risikoereignisse und potenzielle Sicherheitsrisiken bietet, die für die Identitäten Ihrer Organisation bestehen. Microsoft beschäftigt sich schon seit mehr als einem Jahrzehnt mit dem Schutz cloudbasierter Identitäten, und mit Azure AD Identity Protection stellt Microsoft diese Schutzsysteme jetzt für Unternehmenskunden zur Verfügung. Für Identity Protection werden die vorhandenen Azure AD-Funktionen zur Erkennung von Anomalien genutzt (über die Berichte zu anomalen Aktivitäten von Azure AD), und es werden neue Risikoereignistypen eingeführt, mit denen Anomalien in Echtzeit erkannt werden können.



##Erste Schritte

Die allermeisten Sicherheitsverletzungen kommen vor, wenn Angreifer Zugriff auf eine Umgebung erhalten, indem sie die Identität eines Benutzers stehlen. Angreifer gehen beim Nutzen von Drittanbieter-Sicherheitsverletzungen immer effektiver vor und starten professionelle Phishingangriffe. Nachdem ein Angreifer Zugriff auf ein mit geringen Rechten ausgestattetes Benutzerkonto erhalten hat, ist es für ihn relativ einfach, per Seitwärtsbewegung Zugriff auf wichtige Ressourcen des Unternehmens zu erlangen. Daher ist es wichtig, alle Identitäten zu schützen und bei einer Kompromittierung einer Identität proaktiv zu verhindern, dass die kompromittierte Identität missbraucht wird.

Die Ermittlung von kompromittierten Identitäten ist keine einfache Aufgabe. Glücklicherweise erhalten Sie hierbei Unterstützung von Identity Protection: Identity Protection nutzt adaptive Machine Learning-Algorithmen und heuristische Verfahren, um Anomalien und Risikoereignisse zu erkennen, die auf die Kompromittierung einer Identität hindeuten können.
 
Mit diesen Daten generiert Identity Protection Berichte und Warnungen, damit Sie diese Risikoereignisse untersuchen und entsprechende Aktionen zur Korrektur oder Lösung durchführen können.
 
Azure Active Directory Identity Protection ist aber viel mehr als nur ein Tool für die Überwachung und Berichterstellung. Basierend auf Risikoereignissen berechnet Identity Protection eine Benutzerrisikostufe für jeden Benutzer, sodass Sie risikobasierte Richtlinien konfigurieren können, um die Identitäten Ihrer Organisation automatisch zu schützen. Mit diesen risikobasierten Richtlinien in Verbindung mit anderen Kontrollelementen für den bedingten Zugriff von Azure Active Directory und EMS können adaptive Korrekturaktionen blockiert oder bereitgestellt werden. Hierzu gehören Kennwortzurücksetzungen und die Durchsetzung der Multi-Factor Authentication.

####Funktionen von Identity Protection 

**Erkennen von Risikoereignissen und gefährdeten Konten:**

- Erkennen von sechs Risikoereignistypen per Machine Learning und heuristischen Regeln

- Berechnen von Benutzerrisikostufen

- Bereitstellen von benutzerdefinierten Empfehlungen zur Verbesserung der allgemeinen Sicherheit, indem Sicherheitsrisiken aufgedeckt werden



**Untersuchen von Risikoereignissen:**

- Senden von Benachrichtigungen für Risikoereignisse

- Untersuchen von Risikoereignissen mit relevanten und kontextbezogenen Informationen

- Bereitstellen grundlegender Workflows zum Nachverfolgen von Untersuchungen

- Bereitstellen des einfachen Zugriffs auf Korrekturaktionen, z. B. Kennwortzurücksetzung



**Risikobasierte Richtlinien für bedingten Zugriff:**

- Richtlinie für Abwehrmaßnahmen bei risikobehafteten Anmeldungen, indem Anmeldungen blockiert oder die Multi-Factor Authentication erzwungen wird.

- Richtlinie zum Blockieren oder Schützen risikobehafteter Benutzerkonten

- Richtlinie zur Durchsetzung der Registrierung von Benutzern für die Multi-Factor Authentication


## Erkennung und Risiko

### Risikoereignisse

Risikoereignisse sind Ereignisse, die von Identity Protection als verdächtig gekennzeichnet wurden und mit denen angegeben wird, dass eine Identität unter Umständen kompromittiert wurde. Eine vollständige Liste der Risikoereignisse finden Sie unter [Arten von Risikoereignissen, die von Azure Active Directory Identity Protection erkannt werden](active-directory-identityprotection-risk-events-types.md).

Einige dieser Risikoereignisse sind über die Berichte zu anomalen Aktivitäten von Azure AD im Azure-Portal verfügbar. Die Tabelle unten enthält die verschiedenen Risikoereignistypen und den entsprechenden **Azure AD-Bericht zu anomalen Aktivitäten**. Microsoft investiert weiter in diesen Bereich und plant, die Erkennungsgenauigkeit von vorhandenen Risikoereignissen ständig weiter zu verbessern und fortlaufend neue Risikoereignistypen hinzuzufügen.



| Typ des Identity Protection-Risikoereignisses | Zugehöriger Azure AD-Bericht zu anomalen Aktivitäten |
| :-- | :-- |
| Kompromittierte Anmeldeinformationen | Benutzer mit kompromittierten Anmeldeinformationen |
| Unmöglicher Ortswechsel zu atypischen Orten |	Irreguläre Anmeldeaktivitäten |
| Anmeldungen von infizierten Geräten | Anmeldungen von möglicherweise infizierten Geräten |
| Anmeldungen von anonymen IP-Adressen | Anmeldungen von unbekannten Quellen |
| Anmeldungen von IP-Adressen mit verdächtigen Aktivitäten |	Anmeldungen von IP-Adressen mit verdächtigen Aktivitäten |
| Anmeldungen von unbekannten Standorten | - | 
| Sperr-Ereignisse | - |

Die folgenden Azure AD-Berichte zu anomalen Aktivitäten sind nicht als Risikoereignisse in Azure AD Identity Protection enthalten und daher auch nicht über Identity Protection verfügbar. Diese Berichte sind zwar noch im Azure-Portal verfügbar, werden aber nach und nach durch Risikoereignisse in Identity Protection ersetzt.

- Anmeldungen nach mehreren Fehlern
- Anmeldungen aus mehreren geografischen Regionen

### Risikostufe

Die Risikostufe für ein Risikoereignis ist ein Hinweis (Hoch, Mittel oder Niedrig) auf den Schweregrad des Risikoereignisses. Die Risikostufe dient Identity Protection-Benutzern als Hilfe beim Priorisieren der Aktionen, die sie durchführen müssen, um das Risiko für ihre Organisation zu mindern. Der Schweregrad des Risikoereignisses steht für die Stärke des Signals, das zum Vorhersagen der Kompromittierung einer Identität dient, kombiniert mit dem damit üblicherweise verbundenen „Rauschen“.

- **Hoch**: Risikoereignis mit hohem Vertraulichkeitsgrad und hohem Schweregrad. Ereignisse dieser Art sind starke Indikatoren dafür, dass die Identität des Benutzers kompromittiert wurde, und für alle betroffenen Benutzerkonten sollten sofort Korrekturmaßnahmen eingeleitet werden.

- **Mittel**: Risikoereignis mit hohem Schweregrad, aber niedrigerem Vertraulichkeitsgrad (oder umgekehrt). Diese Ereignisse sind potenziell risikobehaftet, und für alle betroffenen Benutzerkonten sollten Korrekturmaßnahmen eingeleitet werden.

- **Niedrig**: Risikoereignis mit niedrigem Vertraulichkeits- und Schweregrad. Bei diesem Ereignis ist unter Umständen keine sofortige Aktion erforderlich, aber zusammen mit anderen Risikoereignissen kann auch dies ein starker Hinweis dafür sein, dass die Identität kompromittiert wurde.


![Risikostufe](./media/active-directory-identityprotection/01.png "Risikostufe")

 

Risikoereignisse werden entweder in **Echtzeit** oder während der Nachbearbeitung identifiziert, nachdem das Risikoereignis bereits stattgefunden hat (offline). Derzeit werden die meisten Risikoereignisse in Identity Protection offline berechnet und innerhalb von zwei bis vier Stunden in Identity Protection angezeigt. Die Echtzeit-Risikoereignisse werden zwar in Echtzeit ausgewertet, aber sie werden in der Identity Protection-Konsole erst nach fünf bis zehn Minuten angezeigt.

Einige ältere Clients unterstützen derzeit keine Erkennung und Prävention von Risikoereignissen in Echtzeit. Daher können Anmeldungen von diesen Clients nicht in Echtzeit erkannt oder verhindert werden.


## Untersuchung
Ihre Navigation durch Identity Protection beginnt normalerweise mit dem Identity Protection-Dashboard.

![Korrektur](./media/active-directory-identityprotection/1000.png "Korrektur")

Über das Dashboard haben Sie Zugriff auf folgende Komponenten:
 
- Berichte, z.B. **Benutzer mit Risikokennzeichnung**, **Risikoereignisse** und **Sicherheitsrisiken**
- Einstellungen, z.B. die Konfiguration von **Sicherheitsrichtlinien**, **Benachrichtigungen** und **Registrierung für mehrstufige Authentifizierung**
 

Hier beginnen Sie in der Regel mit dem Untersuchungsprozess. Dies ist der Prozess der Überprüfung von Aktivitäten, Protokollen und anderen relevanten Informationen zu einem Risikoereignis, um zu entscheiden, ob Schritte zur Fehlerbeseitigung oder Lösung erforderlich sind. Außerdem wird ermittelt, ob und wie die Identität kompromittiert und wie die kompromittierte Identität verwendet wurde.

Sie können Ihre Untersuchungsaktivitäten mit den [Benachrichtigungen](active-directory-identityprotection-notifications.md) verknüpfen, die Azure Active Directory Protection per E-Mail sendet.

Die folgenden Abschnitte enthalten ausführliche Informationen und die Schritte im Zusammenhang mit einer Untersuchung.



## Was ist eine Benutzerrisikostufe?

Eine Benutzerrisikostufe ist ein Hinweis (Hoch, Mittel oder Niedrig) auf die Wahrscheinlichkeit, mit der die Identität des Benutzers kompromittiert wurde. Sie wird basierend auf den Benutzerrisikoereignissen berechnet, die der Identität des Benutzers zugeordnet sind.

Der Status eines Risikoereignisses lautet entweder **Aktiv** oder **Geschlossen**. Nur Risikoereignisse, die den Status **Aktiv** haben, fließen in die Berechnung des Benutzerrisikos ein.

Die Benutzerrisikostufe wird anhand der folgenden Eingaben berechnet:

- Aktive Risikoereignisse des Benutzers
- Risikostufe dieser Ereignisse
- Durchgeführte Korrekturaktionen

![Benutzerrisiken](./media/active-directory-identityprotection/1001.png "Benutzerrisiken")



Sie können die Benutzerrisikostufen verwenden, um Richtlinien für den bedingten Zugriff zu erstellen. So können risikobehaftete Benutzer an der Anmeldung gehindert werden, oder es kann das sichere Ändern des Kennworts erzwungen werden.


## Manuelles Schließen von Risikoereignissen

In den meisten Fällen führen Sie Korrekturaktionen durch, z. B. das sichere Zurücksetzen des Kennworts, um Risikoereignisse automatisch zu schließen. Allerdings ist dies unter Umständen nicht immer möglich. Dies gilt beispielsweise in folgenden Fällen:

- Ein Benutzer mit aktiven Risikoereignissen wurde gelöscht.
- Eine Untersuchung ergibt, dass ein gemeldetes Risikoereignis vom berechtigten Benutzer durchgeführt wurde.

Da Risikoereignisse, die den Status **Aktiv** haben, in die Berechnung des Benutzerrisikos einfließen, müssen Sie eine Risikostufe ggf. manuell herabsetzen, indem Sie Risikoereignisse manuell schließen. Im Zuge der Untersuchung können Sie die folgenden Aktionen durchführen, um den Status eines Risikoereignisses zu ändern:

![Actions](./media/active-directory-identityprotection/34.png "Actions")

- **Lösen:** Wenn Sie nach dem Untersuchen eines Risikoereignisses außerhalb von Identity Protection eine geeignete Korrekturaktion durchgeführt haben und der Meinung sind, dass das Risikoereignis als geschlossen angesehen werden kann, können Sie das Ereignis als „Gelöst“ kennzeichnen. Für Ereignisse mit dem Status „Aufgelöst“ wird der Status auf „Geschlossen“ festgelegt, und das Risikoereignis fließt nicht mehr in das Benutzerrisiko ein.

- **Als falsch positives Ergebnis kennzeichnen:** In einigen Fällen kann die Untersuchung eines Risikoereignisses ergeben, dass es fälschlicherweise als risikobehaftet gekennzeichnet wurde. Sie können zur Reduzierung der Anzahl solcher Vorfälle beitragen, indem Sie das Risikoereignis als falsch positives Ergebnis kennzeichnen. Auf diese Weise können die Machine Learning-Algorithmen die Klassifizierung ähnlicher Ereignisse in Zukunft verbessern. Der Status der falsch positiven Ereignisse wird auf **Geschlossen** festgelegt, und sie fließen nicht mehr in die Berechnung des Benutzerrisikos ein.

- **Ignorieren:** Falls Sie keine Korrekturaktion durchgeführt haben und das Risikoereignis trotzdem aus der Liste „Aktiv“ entfernen möchten, können Sie ein Risikoereignis mit „Ignorieren“ kennzeichnen. Der Ereignisstatus wird dann in „Geschlossen“ geändert. Ignorierte Ereignisse gehen nicht in das Benutzerrisiko ein. Diese Option sollte nur unter ungewöhnlichen Umständen verwendet werden.

- **Reaktivieren:** Risikoereignisse, die manuell geschlossen wurden (durch Auswahl von **Lösen**, **Falsch positiv** oder **Ignorieren**) können reaktiviert werden. Hierfür wird der Ereignisstatus zurück auf **Aktiv** gesetzt. Reaktivierte Risikoereignisse fließen in die Berechnung des Benutzerrisikos ein. Risikoereignisse, die per Korrekturmaßnahme (z. B. das sichere Zurücksetzen des Kennworts) geschlossen werden, können nicht reaktiviert werden.




**So öffnen Sie das entsprechende Konfigurationsdialogfeld:**

1. Klicken Sie auf dem Blatt **Azure AD Identity Protection** unter **Untersuchen** auf **Risk events** (Risikoereignisse).

	![Manuelles Zurücksetzen des Kennworts](./media/active-directory-identityprotection/1002.png "Manuelles Zurücksetzen des Kennworts")

2. Klicken Sie in der Liste **Risk events** (Risikoereignisse) auf ein Risiko.

	![Manuelles Zurücksetzen des Kennworts](./media/active-directory-identityprotection/1003.png "Manuelles Zurücksetzen des Kennworts")

2. Klicken Sie auf dem Risikoblatt mit der rechten Maustaste auf einen Benutzer.

	![Manuelles Zurücksetzen des Kennworts](./media/active-directory-identityprotection/1004.png "Manuelles Zurücksetzen des Kennworts")



### Manuelles Schließen aller Risikoereignisse für einen Benutzer

Damit Sie die Risikoereignisse für einen Benutzer nicht einzeln manuell schließen müssen, bietet Azure Active Directory Identity Protection eine Methode, mit der Sie alle Ereignisse für einen Benutzer mit nur einem Mausklick schließen können.


![Actions](./media/active-directory-identityprotection/2222.png "Actions")

Wenn Sie auf **Dismiss all events** (Alle Ereignisse schließen) klicken, werden alle Ereignisse geschlossen, und der betroffene Benutzer ist nicht mehr gefährdet.



## Bereinigen von Benutzerrisikoereignissen

Eine Korrektur bzw. Bereinigung ist eine Aktion zum Schützen einer Identität oder eines Geräts, die bzw. das zuvor wahrscheinlich oder mit Sicherheit kompromittiert war. Bei einer Korrekturaktion wird die Identität bzw. das Gerät wieder in einen sicheren Zustand versetzt, und vorherige Risikoereignisse, die der Identität oder dem Gerät zugeordnet sind, werden gelöst.

Sie können wie folgt vorgehen, um Benutzerrisikoereignisse zu bereinigen:

- Durchführen einer sicheren Kennwortzurücksetzung, um Benutzerrisikoereignisse manuell zu beseitigen

- Konfigurieren einer Sicherheitsrichtlinie für das Benutzerrisiko, um Benutzerrisikoereignisse automatisch zu minimieren oder zu beseitigen

- Ein neues Image des infizierten Geräts erstellen


### Manuelle Kennwortzurücksetzung

Eine sichere Kennwortzurücksetzung ist eine effektive Korrektur für viele Risikoereignisse. Wenn sie durchgeführt wird, werden diese Risikoereignisse automatisch geschlossen, und die Benutzerrisikoebene wird neu berechnet. Sie können das Identity Protection-Dashboard verwenden, um für einen risikobehafteten Benutzer eine Kennwortzurücksetzung zu initiieren.

Das zugehörige Dialogfeld bietet zwei verschiedene Methoden zum Zurücksetzen eines Kennworts:

**Kennwort zurücksetzen:** Aktivieren Sie die Option **Benutzer zur Kennwortzurücksetzung auffordern**, damit Benutzer die Wiederherstellung selbst durchführen können, wenn sie sich für die Multi-Factor Authentication registriert haben. Während der nächsten Anmeldung des Benutzers muss der Benutzer einen Multi-Factor Authentication-Ablauf erfolgreich bewältigen und wird dann gezwungen, das Kennwort zu ändern. Diese Option ist nicht verfügbar, wenn das Benutzerkonto noch nicht für Multi-Factor Authentication registriert ist.

**Temporäres Kennwort:** Aktivieren Sie die Option **Generiert ein temporäres Kennwort**, um das vorhandene Kennwort sofort ungültig zu machen und ein neues temporäres Kennwort für den Benutzer zu erstellen. Senden Sie das neue temporäre Kennwort an eine andere E-Mail-Adresse des Benutzers oder an den Manager des Benutzers. Da das Kennwort temporär ist, wird der Benutzer aufgefordert, das Kennwort bei der Anmeldung zu ändern.


![Richtlinie](./media/active-directory-identityprotection/1005.png "Richtlinie")


**So öffnen Sie das entsprechende Konfigurationsdialogfeld:**

1. Klicken Sie auf dem Blatt **Azure AD Identity Protection** auf **Users flagged for risk** (Benutzer mit Risikokennzeichnung).

	![Manuelles Zurücksetzen des Kennworts](./media/active-directory-identityprotection/1006.png "Manuelles Zurücksetzen des Kennworts")


2. Wählen Sie in der Benutzerliste einen Benutzer mit mindestens einem Risikoereignis aus.

	![Manuelles Zurücksetzen des Kennworts](./media/active-directory-identityprotection/1007.png "Manuelles Zurücksetzen des Kennworts")


2. Klicken Sie auf dem Benutzerblatt auf **Kennwort zurücksetzen**.

	![Manuelles Zurücksetzen des Kennworts](./media/active-directory-identityprotection/1008.png "Manuelles Zurücksetzen des Kennworts")





## Benutzerrisiko-Sicherheitsrichtlinie

Eine Benutzerrisiko-Sicherheitsrichtlinie ist eine Richtlinie für bedingten Zugriff, mit der die Risikostufe für einen bestimmten Benutzer ausgewertet wird und Korrektur- und Lösungsmaßnahmen basierend auf vordefinierten Bedingungen und Regeln angewendet werden.


![Benutzerrisiko-Richtlinie](./media/active-directory-identityprotection/1009.png "Benutzerrisiko-Richtlinie")


Azure AD Identity Protection dient Ihnen als Unterstützung beim Verwalten der Korrektur- und Lösungsmaßnahmen für Benutzer, die als risikobehaftet eingestuft sind. Sie haben folgende Möglichkeiten:

- Festlegen der Benutzer und Gruppen, für die die Richtlinie gilt:

	![Benutzerrisiko-Richtlinie](./media/active-directory-identityprotection/1010.png "Benutzerrisiko-Richtlinie")


- Festlegen des Schwellenwerts für die Benutzerrisikostufe (niedrig, mittel oder hoch), der die Richtlinie auslöst:

	![Benutzerrisiko-Richtlinie](./media/active-directory-identityprotection/1011.png "Benutzerrisiko-Richtlinie")


- Festlegen der Kontrollen, die durchgeführt werden sollen, wenn die Richtlinie ausgelöst wird:

	![Benutzerrisiko-Richtlinie](./media/active-directory-identityprotection/1012.png "Benutzerrisiko-Richtlinie")


- Ändern des Zustands Ihrer Richtlinie:

	![Benutzerrisiko-Richtlinie](./media/active-directory-identityprotection/403.png "MFA-Registrierung")


- Überprüfen und Bewerten der Auswirkungen einer Änderung vor deren Aktivierung:

	![Benutzerrisiko-Richtlinie](./media/active-directory-identityprotection/1013.png "Benutzerrisiko-Richtlinie")


Wenn Sie den Schwellenwert **Hoch** auswählen, wird die Richtlinie weniger häufig ausgelöst, und die Auswirkungen für Benutzer verringern sich. Benutzer mit der Einstufung **Niedrig** und **Mittel**, die als risikobehaftet gekennzeichnet sind, werden aber aus der Richtlinie ausgeschlossen. Dies trägt unter Umständen nicht zum Schutz von Identitäten oder Geräten bei, die zuvor vermutlich oder mit Sicherheit kompromittiert waren.

Gehen Sie beim Festlegen der Richtlinie wie folgt vor:

- Schließen Sie die Benutzer aus, die voraussichtlich viele falsch positive Ergebnisse generieren (Entwickler, Sicherheitsanalysten).

- Schließen Sie die Benutzer nach Gebietsschemas aus, für die die Aktivierung der Richtlinie keinen praktischen Nutzen hat (z. B. kein Zugriff auf den Helpdesk).

- Verwenden Sie während des anfänglichen Rollouts der Richtlinie (oder wenn Sie die Auswirkungen für Endbenutzer verringern möchten) den Schwellenwert **Hoch**.

- Verwenden Sie den Schwellenwert **Niedrig**, falls für Ihre Organisation eine höhere Sicherheitsebene erforderlich ist. Wenn Sie für den Schwellenwert **Niedrig** auswählen, erhöht sich der Aufwand für die Benutzer bei der Anmeldung, aber die Sicherheit wird erhöht.

Die empfohlene Standardvorgehensweise für die meisten Organisationen ist die Konfiguration einer Regel für den Schwellenwert **Mittel**, um eine gute Balance zwischen Benutzerfreundlichkeit und Sicherheit zu erzielen.

Eine Übersicht über die zugehörige Benutzeroberfläche finden Sie unter:

- [Wiederherstellung für kompromittierte Konten](active-directory-identityprotection-flows.md#compromised-account-recovery)

- [Blockierter Ablauf bei kompromittierten Konten](active-directory-identityprotection-flows.md#compromised-account-blocked)


**So öffnen Sie das entsprechende Konfigurationsdialogfeld:**

1. Klicken Sie auf dem Blatt **Azure AD Identity Protection** im Abschnitt **Konfigurieren** auf **User risk policy** (Benutzerrisikorichtlinie).

	![Benutzerrisiko-Richtlinie](./media/active-directory-identityprotection/1009.png "Benutzerrisiko-Richtlinie")






## Lösen von Benutzerrisikoereignissen
Administratoren können eine Sicherheitsrichtlinie für das Benutzerrisiko festlegen, um Benutzer je nach Risikostufe bei der Anmeldung zu blockieren.

Das Sperren einer Anmeldung bewirkt Folgendes:
 
- Verhindert die Generierung neuer Benutzerrisikoereignisse für den betroffenen Benutzer

- Ermöglicht Administratoren die manuelle Bereinigung der Risikoereignisse, die die Identität des Benutzers beeinträchtigen, und stellen wieder einen sicheren Identitätszustand her



## Was ist eine Risikostufe für die Anmeldung?

Die Risikostufe für die Anmeldung ist eine Einstufung (Hoch, Mittel oder Niedrig) der Wahrscheinlichkeit, dass bei einer bestimmten Anmeldung eine andere Person versucht, eine Authentifizierung mit der Identität des Benutzers durchzuführen. Die Risikostufe für die Anmeldung wird zum Zeitpunkt der Anmeldung bewertet. Dabei werden Risikoereignisse und Indikatoren für diese Anmeldung in Echtzeit ausgewertet.

## Lösen von Anmelderisikoereignissen 
Eine Lösung ist eine entschärfende Aktion, mit der für einen Angreifer die Möglichkeit eingeschränkt wird, eine kompromittierte Identität oder ein Gerät auszunutzen, ohne für die Identität oder das Gerät wieder einen sicheren Zustand herzustellen. Bei einer Lösung werden keine vorherigen Anmelderisikoereignisse behoben, die der Identität oder dem Gerät zugeordnet sind.

Sie können den bedingten Zugriff in Azure AD Identity Protection verwenden, um Anmelderisikoereignisse automatisch zu lösen. Mit diesen Richtlinien berücksichtigen Sie die Risikostufe des Benutzers oder der Anmeldung, um risikobehaftete Anmeldungen zu blockieren oder den Benutzer zur Durchführung der Multi-Factor Authentication zu zwingen. Mit diesen Aktionen kann unter Umständen verhindert werden, dass ein Angreifer eine gestohlene Identität nutzt, um Schaden anzurichten. Außerdem haben Sie ggf. Zeit, um die Identität zu schützen.


## Anmelderisiko-Sicherheitsrichtlinie

Eine Anmelderisiko-Sicherheitsrichtlinie ist eine Richtlinie für bedingten Zugriff, mit der das Risiko für eine bestimmte Anmeldung ausgewertet wird und Lösungen basierend auf vordefinierten Bedingungen und Regeln angewendet werden.

![Richtlinie zum Anmelderisiko](./media/active-directory-identityprotection/1014.png "Richtlinie zum Anmelderisiko")


Azure AD Identity Protection unterstützt Sie beim Verwalten der Entschärfung risikobehafteter Anmeldungen, indem Folgendes ermöglicht wird:

- Festlegen der Benutzer und Gruppen, für die die Richtlinie gilt:

	![Richtlinie zum Anmelderisiko](./media/active-directory-identityprotection/1015.png "Richtlinie zum Anmelderisiko")


- Festlegen des Schwellenwerts für die Anmelderisikostufe (niedrig, mittel oder hoch), der die Richtlinie auslöst:

	![Richtlinie zum Anmelderisiko](./media/active-directory-identityprotection/1016.png "Richtlinie zum Anmelderisiko")


- Festlegen der Kontrollen, die durchgeführt werden sollen, wenn die Richtlinie ausgelöst wird:

	![Richtlinie zum Anmelderisiko](./media/active-directory-identityprotection/1017.png "Richtlinie zum Anmelderisiko")


- Ändern des Zustands Ihrer Richtlinie:

	![MFA-Registrierung](./media/active-directory-identityprotection/403.png "MFA-Registrierung")

- Überprüfen und Bewerten der Auswirkungen einer Änderung vor deren Aktivierung:

	![Richtlinie zum Anmelderisiko](./media/active-directory-identityprotection/1018.png "Richtlinie zum Anmelderisiko")

 
Wenn Sie den Schwellenwert **Hoch** auswählen, wird die Richtlinie weniger häufig ausgelöst, und die Auswirkungen für Benutzer verringern sich.
 
Hierbei werden aber Anmeldungen mit dem Status **Niedrig** und **Mittel**, die als risikobehaftet gekennzeichnet sind, aus der Richtlinie ausgeschlossen. Für einen Angreifer wird das Ausnutzen einer kompromittierten Identität so ggf. nicht blockiert.

Gehen Sie beim Festlegen der Richtlinie wie folgt vor:

- Schließen Sie die Benutzer aus, die nicht über eine mehrstufige Authentifizierung verfügen bzw. für die dies nicht möglich ist.

- Schließen Sie die Benutzer nach Gebietsschemas aus, für die die Aktivierung der Richtlinie keinen praktischen Nutzen hat (z. B. kein Zugriff auf den Helpdesk).

- Schließen Sie die Benutzer aus, die voraussichtlich viele falsch positive Ergebnisse generieren (Entwickler, Sicherheitsanalysten).

- Verwenden Sie während des anfänglichen Rollouts der Richtlinie (oder wenn Sie die Auswirkungen für Endbenutzer verringern möchten) den Schwellenwert **Hoch**.

- Verwenden Sie den Schwellenwert **Niedrig**, falls für Ihre Organisation eine höhere Sicherheitsebene erforderlich ist. Wenn Sie für den Schwellenwert **Niedrig** auswählen, erhöht sich der Aufwand für die Benutzer bei der Anmeldung, aber die Sicherheit wird erhöht.

Die empfohlene Standardvorgehensweise für die meisten Organisationen ist die Konfiguration einer Regel für den Schwellenwert **Mittel**, um eine gute Balance zwischen Benutzerfreundlichkeit und Sicherheit zu erzielen.

 
Für die Richtlinie zum Anmelderisiko gilt Folgendes:

- Sie wird auf den gesamten Browserdatenverkehr und alle Anmeldungen angewendet, für den bzw. die eine moderne Authentifizierung genutzt wird.
- Sie wird nicht auf Anwendungen angewendet, für die ältere Sicherheitsprotokolle verwendet werden, indem der WS-Trust-Endpunkt beim Verbund-IDP, z. B. ADFS, deaktiviert wird.

Auf der Seite **Risikoereignisse** in der Identity Protection-Konsole werden alle Ereignisse aufgeführt:

- Die Richtlinie wurde angewendet auf
- Sie können die Aktivität überprüfen und ermitteln, ob die Aktion geeignet war oder nicht.

Eine Übersicht über die zugehörige Benutzeroberfläche finden Sie unter:

- [Wiederherstellung risikobehafteter Anmeldungen](active-directory-identityprotection-flows.md#risky-sign-in-recovery)

- [Risikobehaftete Anmeldung blockiert](active-directory-identityprotection-flows.md#risky-sign-in-blocked)

- [Multi-Factor Authentication-Registrierung während einer risikobehafteten Anmeldung](active-directory-identityprotection-flows.md#multi-factor-authentication-registration-during-a-risky-sign-in)





**So öffnen Sie das entsprechende Konfigurationsdialogfeld:**

1. Klicken Sie auf dem Blatt **Azure AD Identity Protection** im Abschnitt **Konfigurieren** auf **Sign-in risk policy** (Anmelderisikorichtlinie).

	![Benutzerrisiko-Richtlinie](./media/active-directory-identityprotection/1014.png "Benutzerrisiko-Richtlinie")





## Multi-Factor Authentication-Registrierungsrichtlinie

Azure Multi-Factor Authentication ist eine Methode zur Identitätsüberprüfung, bei der mehr als nur ein Benutzername und ein Kennwort benötigt wird. Sie bietet eine zweite Sicherheitsebene für Benutzeranmeldungen und -transaktionen. Die Erzwingung von Azure Multi-Factor Authentication für Benutzeranmeldungen empfiehlt sich aus folgenden Gründen:

- Sie bietet eine leistungsfähige Authentifizierung mit einer Auswahl von einfachen Überprüfungsoptionen.

- Sie spielt eine wichtige Rolle bei der Vorbereitung Ihres Unternehmens zum Schutz und der Wiederherstellung von kompromittierten Konten.

![Benutzerrisiko-Richtlinie](./media/active-directory-identityprotection/1019.png "Benutzerrisiko-Richtlinie")



Weitere Informationen finden Sie unter [Was ist Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md).


Azure AD Identity Protection unterstützt Sie beim Verwalten des Rollouts der Multi-Factor Authentication-Registrierung, indem über eine konfigurierte Richtlinie Folgendes ermöglicht wird:



- Festlegen der Benutzer und Gruppen, für die die Richtlinie gilt:

	![MFA-Richtlinie](./media/active-directory-identityprotection/1020.png "MFA-Richtlinie")



- Festlegen der Kontrollen, die durchgeführt werden sollen, wenn die Richtlinie ausgelöst wird:

	![MFA-Richtlinie](./media/active-directory-identityprotection/1021.png "MFA-Richtlinie")


- Ändern des Zustands Ihrer Richtlinie:

	![MFA-Richtlinie](./media/active-directory-identityprotection/403.png "MFA-Richtlinie")

- Anzeigen des aktuellen Registrierungsstatus:

	![MFA-Richtlinie](./media/active-directory-identityprotection/1022.png "MFA-Richtlinie")


Eine Übersicht über die zugehörige Benutzeroberfläche finden Sie unter:

- [Registrierung für Multi-Factor Authentication](active-directory-identityprotection-flows.md#multi-factor-authentication-registration)

- [Multi-Factor Authentication-Registrierung während einer risikobehafteten Anmeldung](active-directory-identityprotection-flows.md#multi-factor-authentication-registration-during-a-risky-sign-in)





**So öffnen Sie das entsprechende Konfigurationsdialogfeld:**

1. Klicken Sie auf dem Blatt **Azure AD Identity Protection** im Abschnitt **Konfigurieren** auf **Multi-factor authentication registration** (Registrierung für Multi-Factor Authentication).

	![MFA-Richtlinie](./media/active-directory-identityprotection/1019.png "MFA-Richtlinie")





## Nächste Schritte

 - [Channel 9: Azure AD and Identity Show: Identity Protection Preview (Channel 9: Azure AD- und Identity-Show: Vorschau von Identity Protection)](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)
 - [Arten von Risikoereignissen, die von Azure Active Directory Identity Protection erkannt werden](active-directory-identityprotection-risk-events-types.md)
 - [Von Azure Active Directory Identity Protection erkannte Sicherheitsrisiken](active-directory-identityprotection-vulnerabilities.md)
 - [Azure Active Directory Identity Protection-Benachrichtigungen](active-directory-identityprotection-notifications.md)
 - [Azure Active Directory Identity Protection-Abläufe](active-directory-identityprotection-flows.md)
 - [Azure Active Directory Identity Protection-Playbook](active-directory-identityprotection-playbook.md)
 - [Azure Active Directory Identity Protection – Glossar](active-directory-identityprotection-glossary.md)

 - [Aktivieren von Azure Active Directory Identity Protection](active-directory-identityprotection-enable.md)
 - [Azure Active Directory Identity Protection – Aufheben der Blockierung von Benutzern](active-directory-identityprotection-unblock-howto.md)

 - [Erste Schritte mit Azure Active Directory Identity Protection und Microsoft Graph](active-directory-identityprotection-graph-getting-started.md)

<!---HONumber=AcomDC_0921_2016-->