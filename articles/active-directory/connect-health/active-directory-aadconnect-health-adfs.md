---
title: Verwenden von Azure AD Connect Health mit AD FS | Microsoft-Dokumentation
description: "Dies ist die Azure AD Connect Health-Seite, auf der Sie erfahren, wie Sie Ihre lokale AD FS-Infrastruktur überwachen."
services: active-directory
documentationcenter: 
author: karavar
manager: samueld
editor: curtand
ms.assetid: dc0e53d8-403e-462a-9543-164eaa7dd8b3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 2/15/2017
ms.author: vakarand
translationtype: Human Translation
ms.sourcegitcommit: 738f9d5a8af6e1c641ebfab49bd1f01614ecc913
ms.openlocfilehash: 9e4ba619d37e213b581f7d18d605b99474ad711b
ms.lasthandoff: 02/16/2017


---
# <a name="using-azure-ad-connect-health-with-ad-fs"></a>Verwenden von Azure AD Connect Health mit AD FS
Die folgende Dokumentation bezieht sich auf die Überwachung Ihrer AD FS-Infrastruktur mit Azure AD Connect Health. Informationen zum Überwachen von Azure AD Connect (Sync) mit Azure AD Connect Health finden Sie unter [Verwenden von Azure AD Connect Health für die Synchronisierung](active-directory-aadconnect-health-sync.md). Informationen zur Überwachung der Active Directory-Domänendienste mit Azure AD Connect Health finden Sie unter [Verwenden von Azure AD Connect Health mit AD DS](active-directory-aadconnect-health-adds.md).

## <a name="alerts-for-ad-fs"></a>Warnungen für AD FS
Im Abschnitt "Azure AD Connect Health-Warnungen" wird eine Liste der aktiven Warnungen angezeigt. Jede Warnung umfasst relevante Informationen, Lösungsschritte und Links zur verwandten Dokumentation.

Sie können auf eine aktive oder gelöste Warnung doppelklicken, um ein neues Blatt mit weiteren Informationen, Schritten zum Lösen der Warnung und Links zu relevanter Dokumentation zu öffnen. Sie können außerdem Verlaufsdaten zu bereits behobenen Warnungen anzeigen.

![Azure AD Connect Health-Portal](./media/active-directory-aadconnect-health/alert2.png)

## <a name="usage-analytics-for-ad-fs"></a>Nutzungsanalyse für AD FS
In der Azure AD Connect Health-Nutzungsanalyse wird der Authentifizierungsdatenverkehr für Ihre Verbundserver analysiert. Sie können auf das Feld „Nutzungsanalyse“ doppelklicken, um das Blatt „Nutzungsanalyse“ zu öffnen, auf dem mehrere Metriken und Gruppierungen angezeigt werden.

> [!NOTE]
> Um die Nutzungsanalyse mit AD FS verwenden zu können, müssen Sie sicherstellen, dass die AD FS-Überwachung aktiviert ist. Weitere Informationen finden Sie unter [Aktivieren der Überwachung für AD FS](active-directory-aadconnect-health-agent-install.md#enable-auditing-for-ad-fs).
>
>

![Azure AD Connect Health-Portal](./media/active-directory-aadconnect-health/report1.png)

Um zusätzliche Metriken auszuwählen, einen Zeitraum anzugeben oder die Gruppierung zu ändern, klicken Sie mit der rechten Maustaste auf das Diagramm zur Nutzungsanalyse und wählen „Diagramm bearbeiten“. Anschließend können Sie den Zeitraum angeben, eine andere Metrik auswählen und die Gruppierung ändern. Sie können die Verteilung des Authentifizierungsdatenverkehrs basierend auf verschiedenen Metriken anzeigen und jede Metrik über die relevanten Parameter gruppieren. Diese Parameter werden im folgenden Abschnitt beschrieben:

**Metrik : Anforderungen insgesamt**: Gibt die Gesamtzahl von Anforderungen an, die von AD FS-Servern verarbeitet wurden.

|Gruppieren nach | Bedeutung und Nutzen der Gruppierung |
| --- | --- |
| Alle | Zeigt die Gesamtzahl von Anforderungen an, die von allen AD FS-Servern verarbeitet wurden.|
| Anwendung | Gruppiert die Gesamtzahl von Anforderungen basierend auf der Zielanwendung (vertrauende Seite). Diese Gruppierung ist nützlich, wenn Sie die prozentuale Verteilung des gesamten Datenverkehrs auf die verschiedenen Anwendungen anzeigen möchten. |
|  Server |Gruppiert die Gesamtzahl von Anforderungen basierend auf dem Server, der die Anforderung verarbeitet hat. Diese Gruppierung ist nützlich, um die Lastverteilung für den gesamten Datenverkehr anzuzeigen.
| In den Arbeitsplatz eingebunden |Gruppiert die Gesamtzahl von Anforderungen danach, ob sie von Geräten stammen, die mit einem Arbeitsbereich verknüpft (bekannt) sind. Diese Gruppierung ist nützlich, wenn auf Ihre Ressourcen über Geräte zugegriffen wird, die der Identitätsinfrastruktur unbekannt sind. |
|  Authentication Method (Authentifizierungsmethode) | Gruppiert die Gesamtzahl von Anforderungen basierend auf der Authentifizierungsmethode. Diese Gruppierung ist nützlich, wenn Sie wissen möchten, welche Authentifizierungsmethode in der Regel für die Authentifizierung verwendet wird. Folgende Authentifizierungsmethoden sind möglich:  <ol> <li>Integrierte Windows-Authentifizierung (Windows)</li> <li>Formularbasierte Authentifizierung (Forms)</li> <li>Einmaliges Anmelden (SSO)</li> <li>X509-Zertifikatauthentifizierung (Zertifikat)</li> <br>Wenn die Verbundserver die Anforderung mit einem SSO-Cookie empfangen, wird diese Anforderung als SSO-Anforderung eingestuft. In diesen Fällen wird, sofern es sich um ein gültiges Cookie handelt, der Benutzer nicht zur Eingabe von Anmeldeinformationen aufgefordert, sondern erhält nahtlos Zugriff auf die Anwendung. Dieses Verhalten ist üblich, wenn mehrere vertrauende Seiten von den Verbundservern geschützt werden. |
| Netzwerkadresse | Gruppiert die Gesamtzahl von Anforderungen basierend auf der Netzwerkadresse des Benutzers. Dies kann entweder ein Intranet oder Extranet sein. Diese Gruppierung ist nützlich, wenn Sie die prozentuale Verteilung des eingehenden Datenverkehrs für Intranet und Extranet anzeigen möchten. |


**Metrik: Anforderungen mit Fehlern gesamt**: Die Gesamtzahl von Anforderungen mit Fehlern, die vom Verbunddienst verarbeitet wurden. (Diese Metrik steht nur in AD FS für Windows Server 2012 R2 zur Verfügung.)

|Gruppieren nach | Bedeutung und Nutzen der Gruppierung |
| --- | --- |
| Fehlertyp | Zeigt die Anzahl von Fehlern basierend auf den vordefinierten Fehlertypen an. Diese Gruppierung ist nützlich, um die allgemeinen Arten von Fehlern zu verstehen. <ul><li>Falscher Benutzername oder falsches Kennwort: Fehler, die durch einen falschen Benutzernamen oder ein falsches Kennwort ausgelöst werden.</li> <li>„Extranetsperrung“: Fehler durch Anforderungen von einem Benutzer, der für das Extranet gesperrt wurde. </li><li> „Kennwort abgelaufen“: Fehler durch Benutzer, die sich mit einem abgelaufenen Kennwort anmelden.</li><li>„Konto deaktiviert“: Fehler durch Benutzer, die sich mit einem deaktivierten Konto anmelden.</li><li>„Authentifizierung mit Gerät“: Fehler durch Benutzer, bei denen Probleme bei der Geräteauthentifizierung auftreten.</li><li>„Authentifizierung mit Benutzerzertifikat“: Fehler durch Benutzer, bei denen die Authentifizierung aufgrund eines ungültigen Zertifikats nicht möglich ist.</li><li>„MFA“: Fehler durch Benutzer, die sich nicht per Multi-Factor Authentication authentifizieren können.</li><li>„Andere Anmeldeinformationen“: „Ausstellungsautorisierung“: Fehler aufgrund von Autorisierungsproblemen.</li><li>„Ausstellungsdelegierung“: Fehler aufgrund von Problemen bei der Ausstellungsdelegierung.</li><li>„Tokenannahme“: Fehler durch das Ablehnen von externen Identitätsanbietern durch AD FS.</li><li>„Protokoll“: Fehler aufgrund von Protokollproblemen.</li><li>„Unbekannt“: Fängt alle weiteren Fehler ab. Alle anderen Fehler, die nicht in die definierten Kategorien fallen.</li> |
| Server | Gruppiert die Fehler basierend auf dem Server. Diese Gruppierung ist nützlich, um die Fehlerverteilung auf die Server anzuzeigen. Eine ungleichmäßige Verteilung könnte darauf hinweisen, dass sich ein Server in fehlerhaftem Zustand befindet. |
| Netzwerkadresse | Gruppiert die Fehler basierend auf der Netzwerkadresse der Anforderungen (Intranet oder Extranet). Diese Gruppierung ist nützlich, um die Art der Anforderungen herauszufinden, für die Fehler auftreten. |
|  Anwendung | Gruppiert die Fehler basierend auf der Zielanwendung (vertrauende Seite). Mithilfe dieser Gruppierung können Sie anzeigen, für welche Zielanwendung die meisten Fehler auftreten. |

**Metrik: Benutzeranzahl**: Durchschnittliche Anzahl von eindeutigen Benutzern, die aktiv eine Authentifizierung mit AD FS durchführen.

|Gruppieren nach | Bedeutung und Nutzen der Gruppierung |
| --- | --- |
|Alle |Mit dieser Metrik wird die durchschnittliche Anzahl von Benutzern bereitgestellt, die den Verbunddienst im ausgewählten Zeitraum verwendet haben. Die Benutzer werden nicht gruppiert. <br>Die durchschnittliche Anzahl richtet sich nach dem ausgewählten Zeitraum. |
| Anwendung |Gruppiert die durchschnittliche Anzahl von Benutzern basierend auf der Zielanwendung (vertrauende Seite). Über diese Gruppierung können Sie anzeigen, wie viele Benutzer welche Anwendung nutzen. |

## <a name="performance-monitoring-for-ad-fs"></a>Leistungsüberwachung für AD FS
Die Azure AD Connect Health-Leistungsüberwachung liefert Überwachungsinformationen zu verschiedenen Metriken. Wenn Sie das Kontrollkästchen aktivieren, wird ein neues Blatt mit ausführlichen Informationen zu den Metriken geöffnet.

![Azure AD Connect Health-Portal](./media/active-directory-aadconnect-health/perf1.png)

Wenn Sie die Filteroption oben im Blatt auswählen, können Sie eine Filterung nach Server vornehmen, um Metriken zu einzelnen Servern anzuzeigen. Um Metriken zu ändern, klicken Sie mit der rechten Maustaste auf das Überwachungsdiagramm unterhalb des Überwachungsblatts und klicken dann auf „Diagramm bearbeiten“ (oder wählen Sie die Schaltfläche „Diagramm bearbeiten“). Auf dem dann geöffneten neuen Blatt können Sie in der Dropdownliste zusätzliche Metriken auswählen und einen Zeitbereich für die Anzeige von Leistungsdaten angeben.

## <a name="reports-for-ad-fs"></a>Berichte für AD FS
Azure AD Connect Health bietet Berichte über die Aktivität und Leistung von AD FS. Mithilfe dieser Berichte können Administratoren Einblicke in Aktivitäten auf ihren AD FS-Servern gewinnen.

### <a name="top-50-users-with-failed-usernamepassword-logins"></a>50 Benutzer mit den meisten fehlgeschlagenen Anmeldungen aufgrund eines falschen Benutzernamens/Kennworts
Einer der häufigsten Gründe für das Fehlschlagen von Authentifizierungsanforderungen bei einem AD FS-Server ist die Verwendung ungültiger Anmeldeinformationen, d. h. eines falschen Benutzernamens oder Kennworts. Dies passiert Benutzern normalerweise aufgrund von komplexen Kennwörtern, vergessenen Kennwörtern oder Schreibfehlern.

Aber es gibt noch weitere Gründe, die zu einer unerwartet hohen Anzahl von derartigen Anforderungen bei Ihren AD FS-Servern führen können, z.B. von einer Anwendung zwischengespeicherte abgelaufene Benutzeranmeldeinformationen oder ein böswilliger Benutzer, der versucht, sich mit einer Reihe von bekannten Kennwörtern bei einem Konto anzumelden. Diese beiden Beispiele sind plausible Gründe für einen Anstieg der Anforderungsanzahl.

Azure AD Connect Health für AD FS bietet einen Bericht über die 50 Benutzer, bei denen Anmeldeversuche am häufigsten aufgrund ungültiger Benutzernamen oder Kennwörter fehlschlagen. Zur Erstellung dieses Berichts werden die von allen AD FS-Servern in den Farmen generierten Überwachungsereignisse verarbeitet.

![Azure AD Connect Health-Portal](./media/active-directory-aadconnect-health-adfs/report1a.png)

In diesem Bericht können Sie ganz einfach auf die folgenden Informationen zugreifen:

* Gesamtanzahl fehlgeschlagener Anforderungen mit falschem Benutzernamen/Kennwort in den letzten 30 Tagen
* Durchschnittliche Anzahl von Benutzern pro Tag, bei denen die Anmeldung wegen eines ungültigen Benutzernamens/Kennworts fehlschlägt.

Wenn Sie auf diesen Bereich klicken, gelangen Sie zum Hauptberichtblatt, das zusätzliche Details enthält. Dieses Blatt enthält einen Graphen mit Trendinformationen zum Ermitteln einer Baseline für Anforderungen mit falschem Benutzernamen oder Kennwort. Außerdem wird die Liste mit den Top 50 der Benutzer mit Fehlversuchen angegeben.

Der Graph zeigt die folgenden Informationen:

* Gesamtanzahl fehlgeschlagener Anmeldeversuche aufgrund eines ungültigen Benutzernamens/Kennworts pro Tag
* Gesamtanzahl eindeutiger Benutzer, bei denen die Anmeldung fehlgeschlagen ist, pro Tag.
* Client-IP-Adresse der letzten Anforderung

![Azure AD Connect Health-Portal](./media/active-directory-aadconnect-health-adfs/report3a.png)

Der Bericht enthält die folgenden Informationen:

| Berichtselement | Beschreibung |
| --- | --- |
| Benutzer-ID |Die verwendete Benutzer-ID. Dies ist der Wert mit der Eingabe des Benutzers, also in einigen Fällen die Verwendung der falschen Benutzer-ID. |
| Fehlgeschlagene Anmeldeversuche |Die Gesamtanzahl fehlgeschlagener Anmeldeversuche für die jeweilige Benutzer-ID. Die Tabelle ist nach der höchsten Anzahl fehlgeschlagener Versuche in absteigender Reihenfolge sortiert. |
| Letzter Fehler |Zeigt den Zeitstempel des letzten Fehlers an. |
| Letzte Fehler-IP-Adresse |Zeigt die Client-IP-Adresse der letzten fehlerhaften Anforderung an. |

> [!NOTE]
> Der Bericht wird automatisch alle zwei Stunden mit den neuen gesammelten Informationen aktualisiert. Anmeldeversuche innerhalb der letzten zwei Stunden sind daher unter Umständen nicht im Bericht enthalten.
>
>

## <a name="related-links"></a>Verwandte Links
* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Installieren des Azure AD Connect Health-Agents](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect Health-Vorgänge](active-directory-aadconnect-health-operations.md)
* [Verwenden von Azure AD Connect Health für die Synchronisierung](active-directory-aadconnect-health-sync.md)
* [Verwenden von Azure AD Connect Health mit AD DS](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect Health – FAQ](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect Health: Versionsverlauf](active-directory-aadconnect-health-version-history.md)
