---

title: "Beheben von Lizenzproblemen für eine Gruppe in Azure Active Directory | Microsoft-Dokumentation"
description: Vorgehensweise zum Identifizieren und Beheben von Lizenzzuweisungsproblemen bei Verwendung der gruppenbasierten Lizenzierung mit Azure Active Directory
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
ms.date: 06/05/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.openlocfilehash: bfa951a897c9b383072c0d29c9a4266c163fe753
ms.contentlocale: de-de
ms.lasthandoff: 07/08/2017


---

# <a name="identify-and-resolve-license-assignment-problems-for-a-group-in-azure-active-directory"></a>Identifizieren und Beheben von Lizenzzuweisungsproblemen für eine Gruppe in Azure Active Directory

Mit der gruppenbasierten Lizenzierung in Azure Active Directory (Azure AD) wird für Benutzer das Konzept eines Fehlerzustands für die Lizenzierung eingeführt. In diesem Artikel werden die Gründe dafür beschrieben, warum Benutzer sich in diesem Zustand befinden können.

Wenn Sie Lizenzen direkt einzelnen Benutzern zuweisen, ohne die gruppenbasierte Lizenzierung zu verwenden, tritt für den Zuweisungsvorgang ggf. ein Fehler auf. Wenn Sie beispielsweise das PowerShell-Cmdlet `Set-MsolUserLicense` für einen Benutzer ausführen, kann für das Cmdlet aus unterschiedlichen Gründen, die sich auf die Geschäftslogik beziehen, ein Fehler auftreten. Beispielsweise kann die Anzahl von Lizenzen nicht ausreichen, oder es kann ein Konflikt zwischen zwei Dienstplänen bestehen, die nicht gleichzeitig zugewiesen werden können. Das Problem wird Ihnen sofort gemeldet.

Bei Verwendung der gruppenbasierten Lizenzierung können die gleichen Fehler auftreten, aber dies geschieht im Hintergrund, wenn der Azure AD-Dienst Lizenzen zuweist. Daher können Ihnen die Fehler nicht sofort gemeldet werden. Stattdessen werden sie im Benutzerobjekt aufgezeichnet und anschließend über das Verwaltungsportal gemeldet. Beachten Sie, dass das ursprüngliche Ziel der Lizenzierung des Benutzers nicht in Vergessenheit gerät, sondern bei einem Fehlerzustand zur künftigen Untersuchung und Behebung aufgezeichnet wird.

## <a name="how-to-find-license-assignment-errors"></a>Vorgehensweise: Fehlersuche bei der Lizenzzuweisung

1. Um Benutzer, die sich im Fehlerzustand befinden, in einer bestimmten Gruppe zu suchen, öffnen Sie das Blatt der Gruppe. Unter **Lizenzen** wird eine Benachrichtigung angezeigt, falls Benutzer mit Fehlerzustand vorhanden sind.

![Fehlerbenachrichtigung in einer Gruppe](media/active-directory-licensing-group-problem-resolution-azure-portal/group-error-notification.png)

2. Klicken Sie auf die Benachrichtigung, um eine Liste aller betroffenen Benutzer zu öffnen. Sie können jeden Benutzer einzeln anklicken, um weitere Einzelheiten anzuzeigen.

![Liste der Benutzer im Fehlerzustand in einer Gruppe](media/active-directory-licensing-group-problem-resolution-azure-portal/list-of-users-with-errors.png)

3. Um alle Gruppen zu suchen, die mindestens einen Fehler enthalten, klicken Sie auf dem **Azure Active Directory**-Blatt erst auf **Licenses** (Lizenzen) und dann auf **Overview** (Übersicht). Ein Feld mit Informationen wird angezeigt, wenn einige Gruppen Ihre Aufmerksamkeit erfordern.

![Übersicht: Information zu Gruppen im Fehlerzustand](media/active-directory-licensing-group-problem-resolution-azure-portal/group-errors-widget.png)

4. Klicken Sie auf das Feld, um eine Liste aller Gruppen mit Fehlern anzuzeigen. Für weitere Einzelheiten können Sie jede Gruppe einzeln anklicken.

![Übersicht: Liste der Gruppen mit Fehlern](media/active-directory-licensing-group-problem-resolution-azure-portal/list-of-groups-with-errors.png)


Im Folgenden finden Sie eine Beschreibung aller möglichen Probleme und der Möglichkeiten, diese zu beheben.

## <a name="not-enough-licenses"></a>Nicht genügend Lizenzen

**Problem:** Es sind nicht genügend Lizenzen für eines der in der Gruppe angegebenen Produkte verfügbar. Sie müssen entweder weitere Lizenzen für das Produkt erwerben oder von anderen Benutzern oder Gruppen nicht verwendete Lizenzen freigeben.

Um anzuzeigen, wie viele Lizenzen verfügbar sind, wechseln Sie zu **Azure Active Directory** > **Lizenzen** > **Alle Produkte**.

Klicken Sie auf ein Produkt, um festzustellen, welche Benutzer und Gruppen Lizenzen nutzen. Unter **Lizenzierte Benutzer** werden alle Benutzer angezeigt, denen direkt oder über eine oder mehrere Gruppen Lizenzen zugewiesen wurden. Unter **Lizenzierte Gruppen** sind alle Gruppen aufgeführt, denen Sie das Produkt zugewiesen haben.

**PowerShell:** PowerShell-Cmdlets melden diesen Fehler als _CountViolation_.

## <a name="conflicting-service-plans"></a>In Konflikt stehende Diensteeinstellungen

**Problem:** Eines der in der Gruppe angegebenen Produkte enthält einen Dienstplan, für den ein Konflikt mit einem anderen Dienstplan besteht, der dem Benutzer bereits über ein anderes Produkt zugewiesen ist. Einige Dienstpläne sind so konfiguriert, dass sie demselben Benutzer nicht wie ein anderer verwandter Dienstplan zugewiesen werden können.

Betrachten Sie das folgende Beispiel. Einem Benutzer ist eine Lizenz für Office 365 Enterprise **E1** direkt zugewiesen worden, wobei alle Pläne aktiviert wurden. Der Benutzer wurde einer Gruppe hinzugefügt, der das Produkt Office 365 Enterprise **E3** zugewiesen ist. Dieses Produkt enthält Dienstpläne, die sich mit den in E1 enthaltenen Plänen nicht überlappen dürfen. Daher schlägt die Gruppenlizenzzuweisung mit dem Fehler „Im Konflikt stehende Diensteeinstellungen“ fehl. Bei diesem Beispiel stehen die folgenden Dienstpläne in Konflikt:

-   SharePoint Online (Plan 2) steht in Konflikt mit SharePoint Online (Plan 1).
-   Exchange Online (Plan 2) steht in Konflikt mit Exchange Online (Plan 1).

Eine Möglichkeit zum Lösen dieses Konflikts ist das Deaktivieren dieser beiden Pläne für die E1-Lizenz, die dem Benutzer direkt zugewiesen ist. Eine andere Möglichkeit besteht darin, die gesamte Gruppenlizenzzuweisung zu ändern und die Pläne für die E3-Lizenz zu deaktivieren. Alternativ können Sie dem Benutzer auch die E1-Lizenz entziehen, falls sie im Kontext der E3-Lizenz redundant ist.

Die Entscheidung, wie der Produktlizenzkonflikt zu lösen ist, liegt stets beim Administrator. Azure AD löst Lizenzkonflikte nicht automatisch.

**PowerShell:** PowerShell-Cmdlets melden diesen Fehler als _MutuallyExclusiveViolation_.

## <a name="other-products-depend-on-this-license"></a>Andere Produkte sind von dieser Lizenz abhängig

**Problem:** Eines der in der Gruppe angegebenen Produkte enthält einen Dienstplan, der für einen anderen Dienstplan (in einem anderen Produkt) aktiviert werden muss, um zu funktionieren. Dieser Fehler tritt auf, wenn Azure AD versucht, den zugrunde liegenden Dienstplan zu entfernen. Dies kann beispielsweise passieren, wenn der Benutzer aus der Gruppe entfernt wird.

Zum Lösen dieses Problems müssen Sie sicherstellen, dass der erforderliche Plan Benutzern weiterhin über eine andere Methode zugewiesen ist oder dass die abhängigen Dienste für diese Benutzer deaktiviert sind. Anschließend können Sie die Gruppenlizenz für diese Benutzer richtig entfernen.

**PowerShell:** PowerShell-Cmdlets melden diesen Fehler als _DependencyViolation_.

## <a name="usage-location-isnt-allowed"></a>Verwendungsstandort ist nicht zulässig

**Problem:** Einige Microsoft-Dienste sind aufgrund der vor Ort geltenden Gesetze und Bestimmungen nicht an allen Standorten verfügbar. Bevor Sie eine Lizenz einem Benutzer zuweisen können, müssen Sie die Eigenschaft „Usage location“ (Verwendungsstandort) für den Benutzer angeben. Dies ist im Azure-Portal im Abschnitt unter **Benutzer** > **Profil** > **Einstellungen** möglich.

Wenn Azure AD versucht, einem Benutzer eine Gruppenlizenz zuzuweisen, dessen Verwendungsstandort nicht unterstützt wird, tritt ein Fehler auf, der für den Benutzer aufgezeichnet wird.

Sie können dieses Problem beheben, indem Sie Benutzer aus den nicht unterstützten Standorten der lizenzierten Gruppe entfernen. Alternativ können Sie wie folgt vorgehen: Wenn die aktuellen Werte des Verwendungsstandorts nicht den tatsächlichen Benutzerstandort darstellen, können Sie sie ändern, damit die Lizenzen beim nächsten Mal richtig zugewiesen werden (wenn der neue Standort unterstützt wird).

**PowerShell:** PowerShell-Cmdlets melden diesen Fehler als _ProhibitedInUsageLocationViolation_.

> [!NOTE]
> Wenn von Azure AD Gruppenlizenzen zugewiesen werden, erben alle Benutzer ohne Verwendungsstandort den Standort des Verzeichnisses. Es wird empfohlen, dass Administratoren die richtigen Werte für den Verwendungsstandort von Benutzern festlegen, bevor sie die gruppenbasierte Lizenzierung verwenden, um die vor Ort geltenden Gesetze und Bestimmungen zu erfüllen.

## <a name="what-happens-when-theres-more-than-one-product-license-on-a-group"></a>Was geschieht, wenn mehr als eine Produktlizenz für eine Gruppe vorhanden ist?

Sie können einer Gruppe mehr als eine Produktlizenz zuweisen. Beispielsweise können Sie Office 365 Enterprise E3 und Enterprise Mobility + Security einer Gruppe zuweisen, um alle enthaltenen Dienste den Benutzern mühelos zur Verfügung zu stellen.

Azure AD versucht, den einzelnen Benutzern alle in der Gruppe angegebenen Lizenzen zuzuweisen. Wenn Azure AD eines der Produkte nicht zuweisen kann, weil Probleme mit der Geschäftslogik vorliegen (z.B. wenn die Lizenzen nicht ausreichen oder wenn Konflikte mit anderen Diensten bestehen, die vom Benutzer aktiviert wurden), werden die anderen Lizenzen in der Gruppe auch nicht zugewiesen.

Sie können die Benutzer anzeigen, für die die Zuweisung fehlgeschlagen ist, und prüfen, welche Produkte davon betroffen sind.

## <a name="license-assignment-fails-silently-for-a-user-due-to-duplicate-proxy-addresses-in-exchange-online"></a>Die Lizenzzuweisung zu einem Benutzer schlägt ohne Fehlermeldung fehl wegen doppelter Proxyadressen in Exchange Online

Wenn Sie Exchange Online verwenden, sind u.U. einige Benutzer in Ihrem Mandanten fälschlicherweise mit demselben Proxyadresswert konfiguriert. Wenn durch die gruppenbasierte Lizenzierung versucht wird, einem solchen Benutzer eine Lizenz zuzuweisen, schlägt dies fehl ohne einen Fehler zu erfassen (anders als bei den anderen oben beschriebenen Fehlern). Dabei handelt es sich um eine Einschränkung in der Vorschauversion, die vor der *allgemeinen Verfügbarkeit* behoben wird.

> [!TIP]
> Wenn Sie feststellen, dass einige Benutzer keine Lizenz erhalten haben und kein Fehler bei diesen Benutzern aufgezeichnet wurde, überprüfen Sie zunächst, ob bei diesen doppelte Proxyadressen vorhanden sind.
> Dies kann durch die Ausführung des folgenden PowerShell-Cmdlets in Exchange Online erfolgen:
```
Run Get-Recipient | where {$_.EmailAddresses -match "user@contoso.onmicrosoft.com"} | fL Name, RecipientType,emailaddresses
```
> [This article (Dieser Artikel)](https://support.microsoft.com/help/3042584/-proxy-address-address-is-already-being-used-error-message-in-exchange-online) enthält weitere Einzelheiten zu diesem Problem, einschließlich der Informationen, [how to connect to Exchange Online using remote PowerShell (wie eine Verbindung zu Exchange Online mithilfe von Remote-PowerShell hergestellt wird)](https://technet.microsoft.com/library/jj984289.aspx).

Sobald die Probleme mit den Proxyadressen bei den betroffenen Benutzern gelöst wurden, erzwingen Sie die Lizenzverarbeitung für diese Gruppe, um sicherzustellen, dass Lizenzen nun wieder zugewiesen werden können.

## <a name="how-do-you-force-license-processing-in-a-group-to-resolve-errors"></a>Wie können Sie die Lizenzverarbeitung für eine Gruppe durchsetzen, um Fehler zu beheben?

Je nachdem, welche Maßnahmen Sie ergriffen haben, um Fehler zu beheben, müssen Sie die Verarbeitung einer Gruppe ggf. manuell auslösen, um den Benutzerstatus zu aktualisieren.

Wenn Sie beispielsweise einige Lizenzen freigegeben haben, indem Sie direkte Zuweisungen von Lizenzen für Benutzer entfernt haben, müssen Sie die Verarbeitung von Gruppen auslösen, für die zuvor ein Fehler aufgetreten ist. Nur so erreichen Sie die vollständige Versorgung von Benutzern mit Lizenzen. Wechseln Sie hierzu zum Blatt der Gruppe, öffnen Sie die Option **Lizenzen**, und klicken Sie auf der Symbolleiste auf die Schaltfläche **Erneut verarbeiten**.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu anderen Szenarien für die Lizenzverwaltung über Gruppen finden Sie unter:

* [Zuweisen von Lizenzen zu einer Gruppe in Azure Active Directory](active-directory-licensing-group-assignment-azure-portal.md)
* [Was ist die gruppenbasierte Lizenzierung in Azure Active Directory?](active-directory-licensing-whatis-azure-portal.md)
* [Migrieren einzelner lizenzierter Benutzer zur gruppenbasierten Lizenzierung in Azure Active Directory](active-directory-licensing-group-migration-azure-portal.md)
* [Gruppenbasierte Azure Active Directory-Lizenzierung – zusätzliche Szenarien](active-directory-licensing-group-advanced.md)

