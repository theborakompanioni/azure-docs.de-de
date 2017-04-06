---
title: Azure Active Directory Identity Protection | Microsoft Docs
description: "Erfahren Sie, wie Sie mit Azure AD Identity Protection für Angreifer die Möglichkeit einschränken können, eine kompromittierte Identität oder ein Gerät auszunutzen, und wie Sie eine Identität oder ein Gerät schützen, das zuvor vermutlich oder mit Sicherheit kompromittiert war."
services: active-directory
keywords: Azure Active Directory Identity Protection, Cloud App Discovery, Verwalten von Anwendungen, Sicherheit, Risiko, Risikostufe, Sicherheitsrisiko, Sicherheitsrichtlinie
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/06/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: bb1ca3189e6c39b46eaa5151bf0c74dbf4a35228
ms.openlocfilehash: f09aa6cffbbaa2a3df7f84edee2d3e03aa23a719
ms.lasthandoff: 03/18/2017


---
# <a name="azure-active-directory-identity-protection"></a>Azure Active Directory Identity Protection

Azure Active Directory Identity Protection ist ein Feature der Azure AD Premium P2-Edition, das Ihnen Folgendes ermöglicht:

- Ermitteln potenzieller Sicherheitsrisiken, die für die Identitäten Ihrer Organisation bestehen

- Konfigurieren automatischer Reaktionen auf erkannte verdächtige Aktionen im Zusammenhang mit den Identitäten Ihrer Organisation  

- Untersuchen verdächtiger Vorfälle und Ergreifen entsprechender Maßnahmen zu deren Behebung   


## <a name="getting-started"></a>Erste Schritte

Microsoft sichert cloudbasierte Identitäten über zehn Jahre lang. Mit Azure Active Directory Identity Protection können Sie in Ihrer Umgebung die gleichen Schutzsysteme einsetzen, die von Microsoft zum Sichern von Identitäten verwendet werden.

Die allermeisten Sicherheitsverletzungen kommen vor, wenn Angreifer Zugriff auf eine Umgebung erhalten, indem sie die Identität eines Benutzers stehlen. Im Laufe der Jahre sind Angreifer beim Nutzen von Drittanbieter-Sicherheitsverletzungen immer effektiver vorgegangen und haben professionelle Phishingangriffe gestartet. Sobald sich ein Angreifer Zugriff auf ein mit geringen Rechten ausgestattetes Benutzerkonto verschafft hat, ist es für ihn relativ einfach, per Seitwärtsbewegung Zugriff auf wichtige Unternehmensressourcen zu erlangen.

Daher müssen Sie:

- Alle Identitäten unabhängig von deren Berechtigungsstufe schützen

- Proaktiv verhindern, dass gefährdete Identitäten missbraucht werden

Die Ermittlung von kompromittierten Identitäten ist keine einfache Aufgabe. Azure Active Directory verwendet adaptive Machine Learning-Algorithmen und -Heuristiken zum Erkennen von Anomalien und verdächtigen Vorfällen, die auf potenziell gefährdete Identitäten hinweisen. Mit diesen Daten generiert Identity Protection Berichte und Warnungen, damit Sie die erkannten Probleme auswerten und entsprechende Aktionen zur Lösung oder Korrektur durchführen können.

Azure Active Directory Identity Protection ist viel mehr als nur ein Tool für die Überwachung und Berichterstellung. Zum Schutz der Identitäten Ihrer Organisation können Sie risikobasierte Richtlinien konfigurieren, die automatisch auf erkannte Probleme reagieren, wenn eine angegebene Risikostufe erreicht wurde. Mit diesen Richtlinien in Verbindung mit anderen Kontrollelementen für den bedingten Zugriff von Azure Active Directory und EMS können adaptive Korrekturaktionen entweder blockiert oder initiiert werden. Hierzu gehören Kennwortzurücksetzungen und die Durchsetzung der Multi-Factor Authentication.


#### <a name="identity-protection-capabilities"></a>Funktionen von Identity Protection

**Erkennen von Sicherheitsrisiken und gefährdeten Konten:**  

* Bereitstellen von benutzerdefinierten Empfehlungen zur Verbesserung der allgemeinen Sicherheit, indem Sicherheitsrisiken aufgedeckt werden
* Berechnen der Risikostufen für die Anmeldung
* Berechnen von Benutzerrisikostufen


**Untersuchen von Risikoereignissen:**

* Senden von Benachrichtigungen für Risikoereignisse
* Untersuchen von Risikoereignissen mit relevanten und kontextbezogenen Informationen
* Bereitstellen grundlegender Workflows zum Nachverfolgen von Untersuchungen
* Bereitstellen des einfachen Zugriffs auf Korrekturaktionen, z. B. Kennwortzurücksetzung

**Risikobasierte Richtlinien für bedingten Zugriff:**

* Richtlinie für Abwehrmaßnahmen bei risikobehafteten Anmeldungen, indem Anmeldungen blockiert oder die Multi-Factor Authentication erzwungen wird.
* Richtlinie zum Blockieren oder Schützen risikobehafteter Benutzerkonten
* Richtlinie zur Durchsetzung der Registrierung von Benutzern für die Multi-Factor Authentication

## <a name="detection"></a>Erkennung

### <a name="vulnerabilities"></a>Sicherheitsrisiken

Azure Active Directory Identity Protection analysiert Ihre Konfiguration und erkennt Sicherheitsrisiken, die sich auf die Identitäten Ihrer Benutzers auswirken können. Weitere Details finden Sie unter [Von Azure Active Directory Identity Protection erkannte Sicherheitsrisiken](active-directory-identityprotection-vulnerabilities.md).

### <a name="risk-events"></a>Risikoereignisse

Azure Active Directory verwendet adaptive Machine Learning-Algorithmen und -Heuristiken, um verdächtige Aktivitäten im Zusammenhang mit den Identitäten Ihrer Benutzer zu erkennen. Das System erstellt für jede erkannte verdächtige Aktivität einen Datensatz. Diese Datensätze werden auch als Risikoereignisse bezeichnet.  
Weitere Details finden Sie unter [Azure Active Directory-Risikoereignisse](active-directory-identity-protection-risk-events.md).


## <a name="investigation"></a>Untersuchung
Ihre Navigation durch Identity Protection beginnt normalerweise mit dem Identity Protection-Dashboard.

![Wiederherstellung](./media/active-directory-identityprotection/1000.png "Wiederherstellung")

Über das Dashboard haben Sie Zugriff auf folgende Komponenten:

* Berichte, z.B. **Benutzer mit Risikokennzeichnung**, **Risikoereignisse** und **Sicherheitsrisiken**
* Einstellungen, z.B. die Konfiguration von **Sicherheitsrichtlinien**, **Benachrichtigungen** und **Registrierung für mehrstufige Authentifizierung**

Hier beginnen Sie in der Regel mit dem Untersuchungsprozess. Dies ist der Prozess der Überprüfung von Aktivitäten, Protokollen und anderen relevanten Informationen zu einem Risikoereignis, um zu entscheiden, ob Schritte zur Fehlerbeseitigung oder Lösung erforderlich sind. Außerdem ermitteln Sie, ob und wie die Identität kompromittiert und wie die kompromittierte Identität verwendet wurde.

Sie können Ihre Untersuchungsaktivitäten mit den [Benachrichtigungen](active-directory-identityprotection-notifications.md) verknüpfen, die Azure Active Directory Protection per E-Mail sendet.

Die folgenden Abschnitte enthalten ausführliche Informationen und die Schritte im Zusammenhang mit einer Untersuchung.  


## <a name="risky-sign-ins"></a>Riskante Anmeldungen

Azure Active Directory erkennt einige [Risikoereignistypen](active-directory-reporting-risk-events.md#risk-event-types) in Echtzeit. Alle Echtzeit-Risikoereignisse, die während der Anmeldung eines Benutzers erkannt wurden, tragen zu einem logischen Konzept bei, das als *risikobehaftete Anmeldung* bezeichnet wird. Eine risikobehaftete Anmeldung ist ein Hinweis auf einen Anmeldeversuch, der nicht vom rechtmäßigen Besitzer eines Benutzerkontos durchgeführt wurde. Der Lebenszyklus einer risikobehafteten Anmeldung endet, wenn sich ein Benutzer abmeldet.

### <a name="sign-in-risk-level"></a>Risikostufe für die Anmeldung

Eine Risikostufe für die Anmeldung ist ein Hinweis auf die Wahrscheinlichkeit (hoch, mittel oder niedrig), dass ein Anmeldeversuch nicht vom rechtmäßigen Besitzer eines Benutzerkontos durchgeführt wurde.

### <a name="mitigating-sign-in-risk-events"></a>Lösen von Anmelderisikoereignissen

Eine Lösung ist eine entschärfende Aktion, mit der für einen Angreifer die Möglichkeit eingeschränkt wird, eine kompromittierte Identität oder ein Gerät auszunutzen, ohne für die Identität oder das Gerät wieder einen sicheren Zustand herzustellen. Bei einer Lösung werden keine vorherigen Anmelderisikoereignisse behoben, die der Identität oder dem Gerät zugeordnet sind.

Um risikobehaftete Anmeldungen automatisch zu entschärfen, können Sie Sicherheitsrichtlinien für das Anmelderisiko konfigurieren. Mit diesen Richtlinien berücksichtigen Sie die Risikostufe des Benutzers oder der Anmeldung, um risikobehaftete Anmeldungen zu blockieren oder den Benutzer zur Durchführung der Multi-Factor Authentication zu zwingen. Mit diesen Aktionen kann unter Umständen verhindert werden, dass ein Angreifer eine gestohlene Identität nutzt, um Schaden anzurichten. Außerdem haben Sie ggf. Zeit, um die Identität zu schützen.

### <a name="sign-in-risk-security-policy"></a>Anmelderisiko-Sicherheitsrichtlinie
Eine Anmelderisiko-Sicherheitsrichtlinie ist eine Richtlinie für bedingten Zugriff, mit der das Risiko für eine bestimmte Anmeldung ausgewertet wird und Lösungen basierend auf vordefinierten Bedingungen und Regeln angewendet werden.

![Richtlinie zum Anmelderisiko](./media/active-directory-identityprotection/1014.png "Richtlinie zum Anmelderisiko")

Azure AD Identity Protection unterstützt Sie beim Verwalten der Entschärfung risikobehafteter Anmeldungen, indem Folgendes ermöglicht wird:

* Festlegen der Benutzer und Gruppen, für die die Richtlinie gilt:

    ![Richtlinie zum Anmelderisiko](./media/active-directory-identityprotection/1015.png "Richtlinie zum Anmelderisiko")
* Festlegen des Schwellenwerts für die Anmelderisikostufe (niedrig, mittel oder hoch), der die Richtlinie auslöst:

    ![Richtlinie zum Anmelderisiko](./media/active-directory-identityprotection/1016.png "Richtlinie zum Anmelderisiko")
* Festlegen der Kontrollen, die durchgeführt werden sollen, wenn die Richtlinie ausgelöst wird:  

    ![Richtlinie zum Anmelderisiko](./media/active-directory-identityprotection/1017.png "Richtlinie zum Anmelderisiko")
* Ändern des Zustands Ihrer Richtlinie:

    ![MFA-Registrierung](./media/active-directory-identityprotection/403.png "MFA-Registrierung")
* Überprüfen und Bewerten der Auswirkungen einer Änderung vor deren Aktivierung:

    ![Richtlinie zum Anmelderisiko](./media/active-directory-identityprotection/1018.png "Richtlinie zum Anmelderisiko")

#### <a name="what-you-need-to-know"></a>Wichtige Informationen
Sie können eine Sicherheitsrichtlinie für das Anmelderisiko konfigurieren, um Multi-Factor Authentication zu erzwingen:

![Richtlinie zum Anmelderisiko](./media/active-directory-identityprotection/1017.png "Richtlinie zum Anmelderisiko")

Aus Sicherheitsgründen gilt diese Einstellung jedoch nur für Benutzer, die bereits für Multi-Factor Authentication registriert wurden. Wenn für einen Benutzer Multi-Factor Authentication erforderlich ist, dieser jedoch noch nicht für Multi-Factor Authentication registriert ist, wird der Benutzer blockiert.

Es wird empfohlen, beim Erzwingen von Multi-Factor Authentication für risikobehaftete Anmeldungen Folgendes zu beachten:

1. Aktivieren Sie die [Multi-Factor Authentication-Registrierungsrichtlinie](#multi-factor-authentication-registration-policy) für die entsprechenden Benutzer.
2. Fordern Sie die betroffenen Benutzer auf, sich bei einer nicht riskanten Sitzung anzumelden, um eine MFA-Registrierung auszuführen.

Mit diesen Schritten wird sichergestellt, dass die Multi-Factor Authentication für eine risikobehaftete Anmeldung erzwungen wird.

#### <a name="best-practices"></a>Bewährte Methoden
Wenn Sie den Schwellenwert **Hoch** auswählen, wird die Richtlinie weniger häufig ausgelöst, und die Auswirkungen für Benutzer verringern sich.  

Hierbei werden aber Anmeldungen mit dem Status **Niedrig** und **Mittel**, die als risikobehaftet gekennzeichnet sind, aus der Richtlinie ausgeschlossen. Für einen Angreifer wird das Ausnutzen einer kompromittierten Identität so ggf. nicht blockiert.

Gehen Sie beim Festlegen der Richtlinie wie folgt vor:

* Schließen Sie die Benutzer aus, die nicht über eine mehrstufige Authentifizierung verfügen bzw. für die dies nicht möglich ist.
* Schließen Sie die Benutzer nach Gebietsschemas aus, für die die Aktivierung der Richtlinie keinen praktischen Nutzen hat (z. B. kein Zugriff auf den Helpdesk).
* Schließen Sie die Benutzer aus, die voraussichtlich viele falsch positive Ergebnisse generieren (Entwickler, Sicherheitsanalysten).
* Verwenden Sie während des anfänglichen Rollouts der Richtlinie (oder wenn Sie die Auswirkungen für Endbenutzer verringern möchten) den Schwellenwert **Hoch** .
* Verwenden Sie den Schwellenwert **Niedrig**, falls für Ihre Organisation eine höhere Sicherheitsebene erforderlich ist. Wenn Sie für den Schwellenwert **Niedrig** auswählen, erhöht sich der Aufwand für die Benutzer bei der Anmeldung, aber die Sicherheit wird erhöht.

Die empfohlene Standardvorgehensweise für die meisten Organisationen ist die Konfiguration einer Regel für den Schwellenwert **Mittel** , um eine gute Balance zwischen Benutzerfreundlichkeit und Sicherheit zu erzielen.

Für die Richtlinie zum Anmelderisiko gilt Folgendes:

* Sie wird auf den gesamten Browserdatenverkehr und alle Anmeldungen angewendet, für den bzw. die eine moderne Authentifizierung genutzt wird.
* Sie wird nicht auf Anwendungen angewendet, für die ältere Sicherheitsprotokolle verwendet werden, indem der WS-Trust-Endpunkt beim Verbund-IDP, z. B. ADFS, deaktiviert wird.

Auf der Seite **Risikoereignisse** in der Identity Protection-Konsole werden alle Ereignisse aufgeführt:

* Die Richtlinie wurde angewendet auf
* Sie können die Aktivität überprüfen und ermitteln, ob die Aktion geeignet war oder nicht.

Eine Übersicht über die zugehörige Benutzeroberfläche finden Sie unter:

* [Wiederherstellung risikobehafteter Anmeldungen](active-directory-identityprotection-flows.md#risky-sign-in-recovery)
* [Risikobehaftete Anmeldung blockiert](active-directory-identityprotection-flows.md#risky-sign-in-blocked)  
* [Anmeldeverfahren von Azure AD Identity Protection](active-directory-identityprotection-flows.md)  

**So öffnen Sie das entsprechende Konfigurationsdialogfeld:**

- Klicken Sie auf dem Blatt **Azure AD Identity Protection** im Abschnitt **Konfigurieren** auf **Richtlinie zum Anmelderisiko**.

    ![Benutzerrisiko-Richtlinie](./media/active-directory-identityprotection/1014.png "Benutzerrisiko-Richtlinie")



## <a name="users-flagged-for-risk"></a>Benutzer mit Risikokennzeichnung

Alle [Risikoereignisse](active-directory-identity-protection-risk-events.md), die von Azure Active Directory für einen Benutzer erkannt wurden, tragen zu einem logischen Konzept bei, das als *Benutzer mit Risikokennzeichnung* bezeichnet wird. Ein *Benutzer mit Risikomarkierung* oder *risikobehafteter Benutzer* ist ein Hinweis auf ein möglicherweise kompromittiertes Benutzerkonto.   

![Benutzer mit Risikokennzeichnung](./media/active-directory-identityprotection/1200.png)


### <a name="user-risk-level"></a>Benutzerrisikostufe

Eine Benutzerrisikostufe ist ein Hinweis (Hoch, Mittel oder Niedrig) auf die Wahrscheinlichkeit, mit der die Identität des Benutzers kompromittiert wurde. Sie wird basierend auf den Benutzerrisikoereignissen berechnet, die der Identität eines Benutzers zugeordnet sind.

Der Status eines Risikoereignisses lautet entweder **Aktiv** oder **Geschlossen**. Nur Risikoereignisse, die den Status **Aktiv** haben, fließen in die Berechnung des Risikos auf Benutzerebene ein.

Die Benutzerrisikostufe wird anhand der folgenden Eingaben berechnet:

* Aktive Risikoereignisse des Benutzers
* Risikostufe dieser Ereignisse
* Durchgeführte Korrekturaktionen

![Benutzerrisiken](./media/active-directory-identityprotection/1031.png "Benutzerrisiken")

Sie können die Benutzerrisikostufen verwenden, um Richtlinien für den bedingten Zugriff zu erstellen. So können risikobehaftete Benutzer an der Anmeldung gehindert werden, oder das sichere Ändern des Kennworts kann erzwungen werden.

### <a name="closing-risk-events-manually"></a>Manuelles Schließen von Risikoereignissen

In den meisten Fällen führen Sie Korrekturaktionen durch, z. B. das sichere Zurücksetzen des Kennworts, um Risikoereignisse automatisch zu schließen. Allerdings ist dies unter Umständen nicht immer möglich.  
Dies gilt beispielsweise in folgenden Fällen:

* Ein Benutzer mit aktiven Risikoereignissen wurde gelöscht.
* Eine Untersuchung ergibt, dass ein gemeldetes Risikoereignis vom berechtigten Benutzer durchgeführt wurde.

Da Risikoereignisse, die den Status **Aktiv** haben, in die Berechnung des Benutzerrisikos einfließen, müssen Sie eine Risikostufe ggf. manuell herabsetzen, indem Sie Risikoereignisse manuell schließen.  
Im Zuge der Untersuchung können Sie die folgenden Aktionen durchführen, um den Status eines Risikoereignisses zu ändern:

![Aktionen](./media/active-directory-identityprotection/34.png "Aktionen")

* **Lösen:** Wenn Sie nach dem Untersuchen eines Risikoereignisses außerhalb von Identity Protection eine geeignete Korrekturaktion durchgeführt haben und der Meinung sind, dass das Risikoereignis als geschlossen angesehen werden kann, können Sie das Ereignis als „Gelöst“ kennzeichnen. Für Ereignisse mit dem Status „Aufgelöst“ wird der Status auf „Geschlossen“ festgelegt, und das Risikoereignis fließt nicht mehr in das Benutzerrisiko ein.
* **Als falsch positives Ergebnis kennzeichnen:** In einigen Fällen kann die Untersuchung eines Risikoereignisses ergeben, dass es fälschlicherweise als risikobehaftet gekennzeichnet wurde. Sie können zur Reduzierung der Anzahl solcher Vorfälle beitragen, indem Sie das Risikoereignis als falsch positives Ergebnis kennzeichnen. Auf diese Weise können die Machine Learning-Algorithmen die Klassifizierung ähnlicher Ereignisse in Zukunft verbessern. Der Status der falsch positiven Ereignisse wird auf **Geschlossen** festgelegt, und sie fließen nicht mehr in die Berechnung des Benutzerrisikos ein.
* **Ignorieren:** Falls Sie keine Korrekturaktion durchgeführt haben und das Risikoereignis trotzdem aus der Liste „Aktiv“ entfernen möchten, können Sie ein Risikoereignis mit „Ignorieren“ kennzeichnen. Der Ereignisstatus wird dann in „Geschlossen“ geändert. Ignorierte Ereignisse gehen nicht in das Benutzerrisiko ein. Diese Option sollte nur unter ungewöhnlichen Umständen verwendet werden.
* **Reaktivieren:** Risikoereignisse, die manuell geschlossen wurden (durch Auswahl von **Lösen**, **Falsch positiv** oder **Ignorieren**) können reaktiviert werden. Hierfür wird der Ereignisstatus zurück auf **Aktiv** gesetzt. Reaktivierte Risikoereignisse fließen in die Berechnung des Benutzerrisikos ein. Risikoereignisse, die per Korrekturmaßnahme (z. B. das sichere Zurücksetzen des Kennworts) geschlossen werden, können nicht reaktiviert werden.

**So öffnen Sie das entsprechende Konfigurationsdialogfeld:**

1. Klicken Sie auf dem Blatt **Azure AD Identity Protection** unter **Untersuchen** auf **Risikoereignisse**.

    ![Manuelles Zurücksetzen des Kennworts](./media/active-directory-identityprotection/1002.png "Manuelles Zurücksetzen des Kennworts")
2. Klicken Sie in der Liste **Risk events** (Risikoereignisse) auf ein Risiko.

    ![Manuelles Zurücksetzen des Kennworts](./media/active-directory-identityprotection/1003.png "Manuelles Zurücksetzen des Kennworts")
3. Klicken Sie auf dem Risikoblatt mit der rechten Maustaste auf einen Benutzer.

    ![Manuelles Zurücksetzen des Kennworts](./media/active-directory-identityprotection/1004.png "Manuelles Zurücksetzen des Kennworts")

### <a name="closing-all-risk-events-for-a-user-manually"></a>Manuelles Schließen aller Risikoereignisse für einen Benutzer
Damit Sie die Risikoereignisse für einen Benutzer nicht einzeln manuell schließen müssen, bietet Azure Active Directory Identity Protection eine Methode, mit der Sie alle Ereignisse für einen Benutzer mit nur einem Mausklick schließen können.

![Aktionen](./media/active-directory-identityprotection/2222.png "Aktionen")

Wenn Sie auf **Dismiss all events**(Alle Ereignisse schließen) klicken, werden alle Ereignisse geschlossen, und der betroffene Benutzer ist nicht mehr gefährdet.

### <a name="remediating-user-risk-events"></a>Bereinigen von Benutzerrisikoereignissen

Eine Korrektur bzw. Bereinigung ist eine Aktion zum Schützen einer Identität oder eines Geräts, die bzw. das zuvor wahrscheinlich oder mit Sicherheit kompromittiert war. Bei einer Korrekturaktion wird die Identität bzw. das Gerät wieder in einen sicheren Zustand versetzt, und vorherige Risikoereignisse, die der Identität oder dem Gerät zugeordnet sind, werden gelöst.

Sie können wie folgt vorgehen, um Benutzerrisikoereignisse zu bereinigen:

* Durchführen einer sicheren Kennwortzurücksetzung, um Benutzerrisikoereignisse manuell zu beseitigen
* Konfigurieren einer Sicherheitsrichtlinie für das Benutzerrisiko, um Benutzerrisikoereignisse automatisch zu minimieren oder zu beseitigen
* Ein neues Image des infizierten Geräts erstellen  

#### <a name="manual-secure-password-reset"></a>Manuelle Kennwortzurücksetzung
Eine sichere Kennwortzurücksetzung ist eine effektive Korrektur für viele Risikoereignisse. Wenn sie durchgeführt wird, werden diese Risikoereignisse automatisch geschlossen, und die Benutzerrisikoebene wird neu berechnet. Sie können das Identity Protection-Dashboard verwenden, um für einen risikobehafteten Benutzer eine Kennwortzurücksetzung zu initiieren.

Das zugehörige Dialogfeld bietet zwei verschiedene Methoden zum Zurücksetzen eines Kennworts:

**Kennwort zurücksetzen:** Aktivieren Sie die Option **Benutzer zur Kennwortzurücksetzung auffordern**, damit Benutzer die Wiederherstellung selbst durchführen können, wenn sie sich für die Multi-Factor Authentication registriert haben. Während der nächsten Anmeldung des Benutzers muss der Benutzer einen Multi-Factor Authentication-Ablauf erfolgreich bewältigen und wird dann gezwungen, das Kennwort zu ändern. Diese Option ist nicht verfügbar, wenn das Benutzerkonto noch nicht für Multi-Factor Authentication registriert ist.

**Temporäres Kennwort:** Aktivieren Sie die Option **Generiert ein temporäres Kennwort**, um das vorhandene Kennwort sofort ungültig zu machen und ein neues temporäres Kennwort für den Benutzer zu erstellen. Senden Sie das neue temporäre Kennwort an eine andere E-Mail-Adresse des Benutzers oder an den Manager des Benutzers. Da das Kennwort temporär ist, wird der Benutzer aufgefordert, das Kennwort bei der Anmeldung zu ändern.

![Richtlinie](./media/active-directory-identityprotection/1005.png "Richtlinie")

**So öffnen Sie das entsprechende Konfigurationsdialogfeld:**

1. Klicken Sie auf dem Blatt **Azure AD Identity Protection** auf **Benutzer mit Risikomarkierung**.

    ![Manuelles Zurücksetzen des Kennworts](./media/active-directory-identityprotection/1006.png "Manuelles Zurücksetzen des Kennworts")
2. Wählen Sie in der Benutzerliste einen Benutzer mit mindestens einem Risikoereignis aus.

    ![Manuelles Zurücksetzen des Kennworts](./media/active-directory-identityprotection/1007.png "Manuelles Zurücksetzen des Kennworts")
3. Klicken Sie auf dem Benutzerblatt auf **Kennwort zurücksetzen**.

    ![Manuelles Zurücksetzen des Kennworts](./media/active-directory-identityprotection/1008.png "Manuelles Zurücksetzen des Kennworts")

### <a name="user-risk-security-policy"></a>Benutzerrisiko-Sicherheitsrichtlinie
Eine Benutzerrisiko-Sicherheitsrichtlinie ist eine Richtlinie für bedingten Zugriff, mit der die Risikostufe für einen bestimmten Benutzer ausgewertet wird und Korrektur- und Lösungsmaßnahmen basierend auf vordefinierten Bedingungen und Regeln angewendet werden.

![Benutzerrisiko-Richtlinie](./media/active-directory-identityprotection/1009.png "Benutzerrisiko-Richtlinie")

Azure AD Identity Protection dient Ihnen als Unterstützung beim Verwalten der Korrektur- und Lösungsmaßnahmen für Benutzer, die als risikobehaftet eingestuft sind. Sie haben folgende Möglichkeiten:

* Festlegen der Benutzer und Gruppen, für die die Richtlinie gilt:

    ![Benutzerrisiko-Richtlinie](./media/active-directory-identityprotection/1010.png "Benutzerrisiko-Richtlinie")
* Festlegen des Schwellenwerts für die Benutzerrisikostufe (niedrig, mittel oder hoch), der die Richtlinie auslöst:

    ![Benutzerrisiko-Richtlinie](./media/active-directory-identityprotection/1011.png "Benutzerrisiko-Richtlinie")
* Festlegen der Kontrollen, die durchgeführt werden sollen, wenn die Richtlinie ausgelöst wird:

    ![Benutzerrisiko-Richtlinie](./media/active-directory-identityprotection/1012.png "Benutzerrisiko-Richtlinie")
* Ändern des Zustands Ihrer Richtlinie:

    ![Benutzerrisiko-Richtlinie](./media/active-directory-identityprotection/403.png "MFA-Registrierung")
* Überprüfen und Bewerten der Auswirkungen einer Änderung vor deren Aktivierung:

    ![Benutzerrisiko-Richtlinie](./media/active-directory-identityprotection/1013.png "Benutzerrisiko-Richtlinie")

Wenn Sie den Schwellenwert **Hoch** auswählen, wird die Richtlinie weniger häufig ausgelöst, und die Auswirkungen für Benutzer verringern sich.
Benutzer mit der Einstufung **Niedrig** und **Mittel**, die als risikobehaftet gekennzeichnet sind, werden aber aus der Richtlinie ausgeschlossen. Dies trägt unter Umständen nicht zum Schutz von Identitäten oder Geräten bei, die zuvor vermutlich oder mit Sicherheit kompromittiert waren.

Gehen Sie beim Festlegen der Richtlinie wie folgt vor:

* Schließen Sie die Benutzer aus, die voraussichtlich viele falsch positive Ergebnisse generieren (Entwickler, Sicherheitsanalysten).
* Schließen Sie die Benutzer nach Gebietsschemas aus, für die die Aktivierung der Richtlinie keinen praktischen Nutzen hat (z. B. kein Zugriff auf den Helpdesk).
* Verwenden Sie während des anfänglichen Rollouts der Richtlinie (oder wenn Sie die Auswirkungen für Endbenutzer verringern möchten) den Schwellenwert **Hoch** .
* Verwenden Sie den Schwellenwert **Niedrig** , falls für Ihre Organisation eine höhere Sicherheitsebene erforderlich ist. Wenn Sie für den Schwellenwert **Niedrig** auswählen, erhöht sich der Aufwand für die Benutzer bei der Anmeldung, aber die Sicherheit wird erhöht.

Die empfohlene Standardvorgehensweise für die meisten Organisationen ist die Konfiguration einer Regel für den Schwellenwert **Mittel** , um eine gute Balance zwischen Benutzerfreundlichkeit und Sicherheit zu erzielen.

Eine Übersicht über die zugehörige Benutzeroberfläche finden Sie unter:

* [Wiederherstellung für kompromittierte Konten](active-directory-identityprotection-flows.md#compromised-account-recovery)  
* [Blockierter Ablauf bei kompromittierten Konten](active-directory-identityprotection-flows.md#compromised-account-blocked)  

**So öffnen Sie das entsprechende Konfigurationsdialogfeld:**

- Klicken Sie auf dem Blatt **Azure AD Identity Protection** im Abschnitt **Konfigurieren** auf **Richtlinie zum Benutzerrisiko**.

    ![Benutzerrisiko-Richtlinie](./media/active-directory-identityprotection/1009.png "Benutzerrisiko-Richtlinie")

### <a name="mitigating-user-risk-events"></a>Lösen von Benutzerrisikoereignissen
Administratoren können eine Sicherheitsrichtlinie für das Benutzerrisiko festlegen, um Benutzer je nach Risikostufe bei der Anmeldung zu blockieren.

Das Sperren einer Anmeldung bewirkt Folgendes:

* Verhindert die Generierung neuer Benutzerrisikoereignisse für den betroffenen Benutzer
* Ermöglicht Administratoren die manuelle Bereinigung der Risikoereignisse, die die Identität des Benutzers beeinträchtigen, und stellen wieder einen sicheren Identitätszustand her



## <a name="multi-factor-authentication-registration-policy"></a>Multi-Factor Authentication-Registrierungsrichtlinie
Azure Multi-Factor Authentication ist eine Methode zur Identitätsüberprüfung, bei der mehr als nur ein Benutzername und ein Kennwort benötigt wird. Sie bietet eine zweite Sicherheitsebene für Benutzeranmeldungen und -transaktionen.  
Die Erzwingung von Azure Multi-Factor Authentication für Benutzeranmeldungen empfiehlt sich aus folgenden Gründen:

* Sie bietet eine leistungsfähige Authentifizierung mit einer Auswahl von einfachen Überprüfungsoptionen.
* Sie spielt eine wichtige Rolle bei der Vorbereitung Ihres Unternehmens zum Schutz und der Wiederherstellung von kompromittierten Konten.

![Benutzerrisiko-Richtlinie](./media/active-directory-identityprotection/1019.png "Benutzerrisiko-Richtlinie")

Weitere Informationen finden Sie unter [Was ist Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)

Azure AD Identity Protection unterstützt Sie beim Verwalten des Rollouts der Multi-Factor Authentication-Registrierung, indem über eine konfigurierte Richtlinie Folgendes ermöglicht wird:

* Festlegen der Benutzer und Gruppen, für die die Richtlinie gilt:

    ![MFA-Richtlinie](./media/active-directory-identityprotection/1020.png "MFA-Richtlinie")
* Festlegen der Kontrollen, die durchgeführt werden sollen, wenn die Richtlinie ausgelöst wird:  

    ![MFA-Richtlinie](./media/active-directory-identityprotection/1021.png "MFA-Richtlinie")
* Ändern des Zustands Ihrer Richtlinie:

    ![MFA-Richtlinie](./media/active-directory-identityprotection/403.png "MFA-Richtlinie")
* Anzeigen des aktuellen Registrierungsstatus:

    ![MFA-Richtlinie](./media/active-directory-identityprotection/1022.png "MFA-Richtlinie")

Eine Übersicht über die zugehörige Benutzeroberfläche finden Sie unter:

* [Registrierung für Multi-Factor Authentication](active-directory-identityprotection-flows.md#multi-factor-authentication-registration)  
* [Anmeldeverfahren von Azure AD Identity Protection](active-directory-identityprotection-flows.md)  

**So öffnen Sie das entsprechende Konfigurationsdialogfeld:**

- Klicken Sie auf dem Blatt **Azure AD Identity Protection** im Abschnitt **Konfigurieren** auf **Registrierung für mehrstufige Authentifizierung**.

    ![MFA-Richtlinie](./media/active-directory-identityprotection/1019.png "MFA-Richtlinie")

## <a name="next-steps"></a>Nächste Schritte
* [Channel 9: Azure AD and Identity Show: Identity Protection Preview (Channel 9: Azure AD- und Identity-Show: Vorschau von Identity Protection)](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

* [Aktivieren von Azure Active Directory Identity Protection](active-directory-identityprotection-enable.md)

* [Von Azure Active Directory Identity Protection erkannte Sicherheitsrisiken](active-directory-identityprotection-vulnerabilities.md)

* [Azure Active Directory-Risikoereignisse](active-directory-identity-protection-risk-events.md)

* [Azure Active Directory Identity Protection-Benachrichtigungen](active-directory-identityprotection-notifications.md)

* [Azure Active Directory Identity Protection-Playbook](active-directory-identityprotection-playbook.md)

* [Azure Active Directory Identity Protection – Glossar](active-directory-identityprotection-glossary.md)

* [Anmeldeverfahren von Azure AD Identity Protection](active-directory-identityprotection-flows.md)

* [Azure Active Directory Identity Protection – Aufheben der Blockierung von Benutzern](active-directory-identityprotection-unblock-howto.md)

* [Erste Schritte mit Azure Active Directory Identity Protection und Microsoft Graph](active-directory-identityprotection-graph-getting-started.md)

