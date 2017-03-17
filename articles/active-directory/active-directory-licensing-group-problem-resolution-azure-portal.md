---

title: "Beheben von Lizenzproblemen für eine Gruppe in Azure Active Directory | Microsoft-Dokumentation"
description: Vorgehensweise zum Bestimmen und Beheben von Lizenzzuweisungsproblemen mithilfe der gruppenbasierten Lizenzierung mit Azure Active Directory
services: active-directory
keywords: "Azure AD-Lizenzierung"
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/28/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: ac3f563828c5fa379f328392a3f5cf7c7932f534
ms.openlocfilehash: 4ed83a1af1c31d41860931d363d93c7d61df9c98
ms.lasthandoff: 03/01/2017


---

# <a name="identifying-and-resolving-license-assignment-problems-when-using-groups-in-azure-active-directory"></a>Bestimmen und Beheben von Lizenzzuweisungsproblemen bei Verwendung von Gruppen in Azure Active Directory


Die gruppenbasierte Lizenzierung in Azure Active Directory (Azure AD) führt das Konzept von Benutzern mit dem Lizenzstatus „Fehler“ ein. In diesem Artikel wird erläutert, warum Benutzer diesen Status aufweisen können. Wenn Lizenzen einzelnen Benutzern direkt, d.h. ohne gruppenbasierte Lizenzierung, zugewiesen werden, kann der Zuweisungsvorgang misslingen. Wenn der Administrator das PowerShell-Cmdlet `Set-MsolUserLicense` auf einen Benutzer anwendet, kann das Cmdlet aus einer Reihe von Gründen der Geschäftslogik fehlschlagen, z.B. eine unzureichende Anzahl von Lizenzen oder ein Konflikt zwischen zwei Serviceplänen, die nicht gleichzeitig zugewiesen werden können. Das Problem wird sofort dem Benutzer gemeldet, der den Befehl ausführt.

Bei Verwendung der gruppenbasierten Lizenzierung können dieselben Fehler auftreten, was jedoch im Hintergrund erfolgt, wenn der Azure AD-Dienst Lizenzen zuweist. Aus diesem Grund können sie nicht unmittelbar dem Administrator gemeldet werden. Stattdessen werden sie im Benutzerobjekt aufgezeichnet und über das Verwaltungsportal gemeldet. Die ursprüngliche Absicht der Lizenzierung des Benutzers kommt nie abhanden, kann aber bei aktivem Status angewendet oder beim Status „Fehler“ zur künftigen Untersuchung und Behebung aufgezeichnet werden.

Um Benutzer mit dem Status „Fehler“ in jeder Gruppe zu suchen, öffnen Sie das Blatt der jeweiligen Gruppe. Unter **Lizenzen** wird eine Benachrichtigung angezeigt, wenn Benutzer mit dem Status „Fehler“ vorliegen. Wählen Sie die Benachrichtigung aus, um eine Ansicht mit allen betroffenen Benutzern zu öffnen, die nacheinander untersucht werden können, um das zugrunde liegende Problem zu verstehen. In diesem Artikel werden alle möglichen Probleme und die Möglichkeit zur Behebung beschrieben.

## <a name="not-enough-licenses"></a>Nicht genügend Lizenzen

Es sind nicht genügend Lizenzen für eines der in der Gruppe angegebenen Produkte verfügbar. Sie müssen entweder weitere Lizenzen für das Produkt erwerben oder von anderen Benutzern oder Gruppen nicht verwendete Lizenzen freigeben.

Um anzuzeigen, wie viele Lizenzen verfügbar sind, wechseln Sie zu **Azure Active Directory &gt; Lizenzen &gt; Alle Produkte**.

Um festzustellen, welche Benutzer und Gruppen Lizenzen haben, klicken Sie auf ein Produkt. Unter **Lizenzierte Benutzer** werden alle Benutzer angezeigt, denen Lizenzen entweder direkt oder über eine oder mehrere Gruppen zugewiesen sind. Unter **Lizenzierte Gruppen** sehen Sie alle Gruppen, denen Sie das Produkt zugewiesen haben.

## <a name="conflicting-service-plans"></a>In Konflikt stehende Diensteeinstellungen

Eines der in der Gruppe angegebenen Produkte enthält einen Serviceplan, der in Konflikt mit einem anderen Serviceplan steht, der dem Benutzer bereits über ein anderes Produkt zugewiesen ist. Einige Servicepläne sind so konfiguriert, dass sie demselben Benutzer nicht wie ein anderer verwandter Serviceplan zugewiesen werden können.

Betrachten Sie das folgende Beispiel: Einem Benutzer ist eine Lizenz für Office 365 Enterprise **E1** direkt zugewiesen worden, wobei alle Pläne aktiviert wurden. Der Benutzer wurde einer Gruppe hinzugefügt, der das Produkt Office 365 Enterprise **E3** zugewiesen ist. Dieses Produkt enthält Servicepläne, die sich zwischen E1 und E3 nicht überschneiden dürfen, weshalb die Gruppenlizenzzuweisung mit dem Fehler „In Konflikt stehende Diensteeinstellungen“ misslingt. Bei diesem Beispiel stehen die folgenden Servicepläne in Konflikt:

-   SharePoint Online (Plan 2) steht in Konflikt mit SharePoint Online (Plan 1)

-   Exchange Online (Plan 2) steht in Konflikt mit Exchange Online (Plan 1)

Um diesen Konflikt zu lösen, müssen Sie diese beiden Pläne entweder in der E1-Lizenz deaktivieren, die dem Benutzer direkt zugewiesen ist, oder die gesamte Gruppenlizenzzuweisung ändern und die Pläne in der E3-Lizenz deaktivieren. Alternativ könnten Sie die E1-Lizenz dem Benutzer entziehen, falls sie im Kontext der E3-Lizenz redundant ist.

Die Entscheidung, wie der Produktlizenzkonflikt zu lösen ist, liegt stets beim Administrator. Azure AD löst Lizenzkonflikte nicht automatisch.

## <a name="other-products-depend-on-this-license"></a>Andere Produkte sind von dieser Lizenz abhängig

Eines der in der Gruppe angegebenen Produkte enthält einen Serviceplan, der für einen anderen Serviceplan (in einem anderen Produkt) aktiviert werden muss, um zu funktionieren. Dieser Fehler tritt auf, wenn Azure AD versucht, den zugrunde liegenden Serviceplan zu entfernen, z.B. als Ergebnis der Entfernung des Benutzers aus der Gruppe.

## <a name="usage-location-not-allowed"></a>Verwendungsstandort nicht zulässig

Einige Microsoft-Dienste sind aufgrund geltender Gesetze und Vorschriften nicht in allen Regionen verfügbar. Bevor einem Benutzer eine Lizenz zugewiesen werden kann, muss der Administrator die Eigenschaft „Verwendungsstandort“ für den Benutzer angeben. Dies ist im Azure-Portal unter **Benutzer&gt; Profil &gt; Einstellungen** möglich.

Bei Verwendung der Gruppenlizenzzuweisung erben alle Benutzer ohne Verwendungsstandort den Standort des Verzeichnisses. Wenn es Benutzer an Standorten gibt, an denen Pläne nicht verfügbar sind, erwägen Sie das Ändern der Lizenzzuweisung auf Gruppenebene durch Deaktivieren der betroffenen Pläne. Alternativ können Sie diese Benutzer in eine andere Gruppe verschieben, deren Lizenzzuweisungen nicht in Konflikt mit dem Standort stehen.

Wenn sich Ihre Benutzer an unterschiedlichen Standorten befinden, achten Sie darauf, diese in Ihren Benutzerobjekten richtig anzugeben, bevor Sie Gruppen mit Lizenzen Benutzer hinzufügen.

## <a name="what-happens-when-there-is-more-than-1-product-license-on-a-group"></a>Was geschieht, wenn mehr als eine Produktlizenz für eine Gruppe vorhanden ist?

Sie können einer Gruppe mehr als eine Produktlizenz zuweisen. Sie können z.B. Office 365 Enterprise E3 und Enterprise Mobility + Security einer Gruppe zuweisen, um alle enthaltenen Dienste den Benutzern mühelos zur Verfügung zu stellen.

Azure AD versucht, den einzelnen Benutzern alle in der Gruppe angegebenen Lizenzen zuzuweisen. Wenn wir aufgrund von Problemen mit der Geschäftslogik eines der Produkte nicht zuweisen können (z.B. nicht genügend Lizenzen oder Konflikte mit anderen für den Benutzer aktivierten Diensten), weisen wir auch nicht die anderen Lizenzen in der Gruppe zu.

Sie können die Benutzer bestimmen, für die die Zuweisung fehlgeschlagen ist, und prüfen, welche Produkte davon betroffen sind.

## <a name="how-to-force-processing-of-licenses-in-a-group-to-resolve-errors"></a>Wie kann die Verarbeitung von Lizenzen in einer Gruppe zur Fehlerbehebung erzwungen werden?

Je nachdem, welche Maßnahmen ergriffen wurden, um Fehler zu beheben, kann es erforderlich sein, die Verarbeitung einer Gruppe manuell auszulösen, um den Benutzerstatus zu aktualisieren.

Wenn Sie beispielsweise weitere Lizenzen erworben haben, um alle Benutzer zu versorgen, müssen Sie die Verarbeitung vorheriger Gruppen mit Lizenzfehlern auslösen, damit alle Gruppenmitglieder vollständig lizenziert werden. Wechseln Sie hierzu zum Blatt der Gruppe, öffnen Sie **Lizenzen**, und klicken Sie auf der Symbolleiste auf die Schaltfläche **Erneut verarbeiten**.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu anderen Szenarien für die Lizenzverwaltung über Gruppen finden Sie in folgenden Artikeln.

* [Zuweisen von Lizenzen zu einer Gruppe in Azure Active Directory](active-directory-licensing-group-assignment-azure-portal.md)
* [Was ist die gruppenbasierte Lizenzierung in Azure Active Directory?](active-directory-licensing-whatis-azure-portal.md)
* [Migrieren einzelner lizenzierter Benutzer zur gruppenbasierten Lizenzierung in Azure Active Directory](active-directory-licensing-group-migration-azure-portal.md)
* [Gruppenbasierte Azure Active Directory-Lizenzierung – zusätzliche Szenarien](active-directory-licensing-group-advanced.md)

