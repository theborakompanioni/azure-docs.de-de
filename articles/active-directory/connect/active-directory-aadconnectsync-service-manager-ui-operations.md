---
title: "Azure AD Connect-Synchronisierung: Synchronization Service Manager-Benutzeroberfläche | Microsoft Docs"
description: "Grundlagen zur Registerkarte „Vorgänge“ in Synchronization Service Manager für Azure AD Connect."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 97a26565-618f-4313-8711-5925eeb47cdc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2016
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: 610dab0af17f927d86b677f647acd0dfe2569583
ms.openlocfilehash: 53b98aaf67b874b0af7d0e94e29bcbe23fc6fc5b


---
# <a name="azure-ad-connect-sync-synchronization-service-manager"></a>Azure AD Connect-Synchronisierung: Synchronization Service Manager

![Synchronization Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/operations.png)

Die Registerkarte „Vorgänge“ zeigt die Ergebnisse der letzten Vorgänge. Diese Registerkarte ist sehr wichtig, um Probleme zu verstehen und zu beheben.

## <a name="understand-the-information-visible-in-the-operations-tab"></a>Grundlagen zu den Informationen, die in der Registerkarte „Vorgänge“ angezeigt werden
Die obere Hälfte zeigt alle Ausführungen in chronologischer Reihenfolge. Standardmäßig enthält das Vorgangsprotokoll Informationen der letzten sieben Tage. Diese Einstellung kann mit dem [Scheduler](active-directory-aadconnectsync-feature-scheduler.md) geändert werden. Suchen Sie nach Ausführungen ohne Erfolgsstatus. Die Sortierung kann durch Klicken auf die Kopfzeilen geändert werden.

Die Spalte **Status** zeigt die wichtigste Information und das schwerwiegendste Problem einer Ausführung an. Hier folgt eine kurze Zusammenfassung der häufigsten Status, in der Reihenfolge ihrer Untersuchungspriorität (bei „*“ gibt es mehrere mögliche Fehlerzeichenfolgen).

| Status | Kommentar |
| --- | --- |
| stopped-* |Die Ausführung konnte nicht abgeschlossen werden. Beispielsweise, wenn das Remotesystem ausgefallen ist und nicht kontaktiert werden kann. |
| stopped-error-limit |Es gibt mehr als 5.000 Fehler. Die Ausführung wurde aufgrund der großen Anzahl von Fehlern automatisch beendet. |
| completed-\*-errors |Die Ausführung wurde abgeschlossen, es sind jedoch Fehler (weniger als 5.000) aufgetreten, die untersucht werden sollten. |
| completed-\*-warnings |Die Ausführung wurde abgeschlossen, einige Daten weisen jedoch einen unerwarteten Zustand auf. Wenn Fehler auftreten, ist diese Meldung in der Regel nur ein Symptom. Bis Sie die Fehler behoben haben, sollten Sie keine Warnungen untersuchen. |
| Erfolg |Keine Probleme |

Wenn Sie eine Zeile auswählen, wird der untere Bereich aktualisiert, und die Details dieser Ausführung werden angezeigt. Ganz links neben dem unteren Teil erscheint möglicherweise eine Liste mit der **Schrittnummer**. Diese Liste wird nur angezeigt, wenn Ihre Gesamtstruktur mehrere Domänen enthält und jede Domäne als einzelner Schritt dargestellt wird. Den Domänennamen finden Sie unter der Überschrift **Partition**. Unter **Synchronization Statistics**(Synchronisierungsstatistik) finden Sie weitere Informationen zur Anzahl verarbeiteter Änderungen. Sie können auf die Links klicken, um eine Liste mit den geänderten Objekten anzuzeigen. Sind Objekte mit Fehlern vorhanden, werden diese Fehler unter **Synchronisierungsfehler**angezeigt.

## <a name="troubleshoot-errors-in-operations-tab"></a>Fehlerbehandlung in der Registerkarte „Vorgänge“
![Synchronization Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/errorsync.png)  
Wenn Fehler auftreten, werden sowohl das fehlerhafte Objekt als auch der Fehler selbst als Links dargestellt, über die weitere Informationen abgerufen werden können.

Klicken Sie zunächst auf die Fehlerzeichenfolge (**sync-rule-error-function-triggered** in der Abbildung). Eine Übersicht über das Objekt wird angezeigt. Klicken Sie zum Anzeigen des tatsächlichen Fehlers auf die Schaltfläche **Stapelüberwachung**. Dadurch werden für den Fehler Informationen der Debugebene angezeigt.

**Tipp**: Sie können im Feld **Call Stack Information** (Aufruflisteninformationen) mit der rechten Maustaste auf **Select All** (Alle auswählen) klicken, und dann **Copy** (Kopieren) auswählen. Sie können dann den Stapel kopieren und den Fehler in Ihrem bevorzugten Editor, z. B. „Notepad“ betrachten.

* Wenn der Fehler aus **SyncRulesEngine**stammt, beginnen die Aufruflisteninformationen mit einer Liste aller Attribute für das Objekt. Scrollen Sie nach unten, bis Sie die Überschrift **InnerException = >** sehen.  
  ![Synchronization Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/errorinnerexception.png)  
  Die Zeile danach gibt Aufschluss über den Fehler. Der Fehler in der obigen Abbildung stammt aus einer benutzerdefinierten Synchronisierungsregel, die von Fabrikam erstellt wurde.

Wenn der Fehler selbst nicht genügend Informationen liefert, ist es an der Zeit, sich die Daten selbst anzusehen. Klicken Sie auf den Link mit dem Objektbezeichner, und [verfolgen Sie ein Objekt und seine Daten durch das System](active-directory-aadconnectsync-service-manager-ui-connectors.md#follow-an-object-and-its-data-through-the-system).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Konfiguration der [Azure AD Connect-Synchronisierung](active-directory-aadconnectsync-whatis.md) .

Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md).



<!--HONumber=Jan17_HO2-->


