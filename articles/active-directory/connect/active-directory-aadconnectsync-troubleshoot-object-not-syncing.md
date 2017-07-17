---
title: Fehlerbehandlung bei einem Objekt, das nicht mit Azure AD synchronisiert wird | Microsoft-Dokumentation
description: 'Problembehandlung: Warum wird ein Objekt nicht mit Azure AD synchronisiert?'
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.translationtype: Human Translation
ms.sourcegitcommit: 2f03ba60d81e97c7da9a9fe61ecd419096248763
ms.openlocfilehash: 3c9642e5c2da92203be2bc23820e5b6f544de660
ms.contentlocale: de-de
ms.lasthandoff: 03/04/2017

---
# Problembehandlung: Ein Objekt wird nicht mit Azure AD synchronisiert
<a id="troubleshoot-an-object-that-is-not-synchronizing-to-azure-ad" class="xliff"></a>

Wenn ein Objekt wie erwartet nicht mit Azure AD synchronisiert wird, kann das verschiedene Ursachen haben. Wenn Sie eine E-Mail mit Fehlerbenachrichtigung von Azure AD erhalten haben, oder Sie die Fehler im Azure AD Connect Health sehen, dann lesen Sie stattdessen [Beheben von Fehlern während der Synchronisierung](active-directory-aadconnect-troubleshoot-sync-errors.md). Wenn Sie ein Problem behandeln möchten, in dem das Objekt nicht in Azure AD ist, dann ist dieses Thema für Sie das Richtige. Es wird beschrieben, wie Fehler in der lokalen Komponente Azure AD Connect-Synchronisierung gefunden werden.

Um den Fehler zu finden, müssen Sie an einigen verschiedenen Stellen in folgender Reihenfolge suchen:

1. Die [Vorgangsprotokolle](#operations) für das Suchen von Fehlern, die während des Importierens und Synchronisierens vom Synchronisierungsmoduls identifiziert werden.
2. Die [Connectorbereich](#connector-space-object-properties) zum Suchen von fehlenden Objekten und Synchronisierungsfehlern.
3. Die [Metaverse](#metaverse-object-properties) für die Suche nach Daten-basierten Problemen.

Starten Sie [Synchronization Service Manager](active-directory-aadconnectsync-service-manager-ui.md) vor dem Beginn der folgenden Schritte.

## Vorgänge
<a id="operations" class="xliff"></a>
Sie sollten mit Ihrer Problembehandlung in der Registerkarte „Vorgänge“ im Synchronization Service Manager beginnen. Die Registerkarte „Vorgänge“ zeigt die Ergebnisse der letzten Vorgänge.  
![Synchronization Service Manager](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/operations.png)  

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

### Fehlerbehandlung in der Registerkarte „Vorgänge“
<a id="troubleshoot-errors-in-operations-tab" class="xliff"></a>
![Synchronization Service Manager](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/errorsync.png)  
Wenn Fehler auftreten, werden sowohl das fehlerhafte Objekt als auch der Fehler selbst als Links dargestellt, über die weitere Informationen abgerufen werden können.

Klicken Sie zunächst auf die Fehlerzeichenfolge (**sync-rule-error-function-triggered** in der Abbildung). Eine Übersicht über das Objekt wird angezeigt. Klicken Sie zum Anzeigen des tatsächlichen Fehlers auf die Schaltfläche **Stapelüberwachung**. Dadurch werden für den Fehler Informationen der Debugebene angezeigt.

Sie können im Feld **Call Stack Information** (Aufruflisteninformationen) mit der rechten Maustaste auf **Select All** (Alle auswählen) klicken, und anschließend **Copy** (Kopieren) auswählen. Sie können dann den Stapel kopieren und den Fehler in Ihrem bevorzugten Editor, z. B. „Notepad“ betrachten.

* Wenn der Fehler aus **SyncRulesEngine**stammt, beginnen die Aufruflisteninformationen mit einer Liste aller Attribute für das Objekt. Scrollen Sie nach unten, bis Sie die Überschrift **InnerException = >** sehen.  
  ![Synchronization Service Manager](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/errorinnerexception.png)  
  Die Zeile danach gibt Aufschluss über den Fehler. Der Fehler in der obigen Abbildung stammt aus einer benutzerdefinierten Synchronisierungsregel, die von Fabrikam erstellt wurde.

Wenn der Fehler selbst nicht genügend Informationen liefert, ist es an der Zeit, sich die Daten selbst anzusehen. Sie können auf den Link mit der Objekt-ID klicken und mit der Problembehandlung der [importierten Objekte des Connectorbereichs](#cs-import) fortfahren.

## Eigenschaften der Objekte des Connectorbereichs
<a id="connector-space-object-properties" class="xliff"></a>
Wenn Sie in der Registerkarte [Vorgänge](#operations) keine Fehler gefunden haben, müssen Sie im nächsten Schritt dem Objekt des Connectorbereichs aus Active Directory zum Metaverse und Azure AD folgen. In diesem Pfad sollten Sie feststellen können, wo das Problem liegt.

### Ein Objekt in der CS suchen
<a id="search-for-an-object-in-the-cs" class="xliff"></a>

Kicken Sie im **Synchronization Service Manager** auf **Connectors**, wählen Sie den Active Directory-Connector und **Connectorbereich durchsuchen** aus.

Wählen Sie im **Bereich** **RDN** (wenn Sie nach dem CN-Attribut suchen möchten) oder **DN oder Anker** (wenn Sie auf dem Attribut „distinguishedName“ suchen möchten) aus. Geben Sie einen Wert ein, und klicken Sie auf **Suchen**.  
![Suche des Connectorbereichs](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/cssearch.png)  

Wenn Sie das gesuchte Objekt nicht finden, wurde es möglicherweise mit der [domänenbasierten Filterung](active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering) oder [Filterung basierend auf der Organisationseinheit](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) gefiltert. Lesen Sie das Thema [Konfigurieren der Filterung](active-directory-aadconnectsync-configure-filtering.md), um sicherzustellen, dass das Filtern wie erwartet konfiguriert ist.

Eine andere nützliche Suche besteht darin, den Azure AD-Connector auszuwählen. Wählen Sie dafür im **Bereich** **Pending Import**(ausstehender Import) aus und anschließend das Kontrollkästchen **Hinzufügen**. Bei dieser Suche werden Ihnen alle synchronisierten Objekte in Azure AD angezeigt, die nicht mit einem lokalen Objekt zugeordnet werden können.  
![Verwaiste Suche des Connectorbereichs](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/cssearchorphan.png)  
Diese Objekte wurden von einem anderen Synchronisierungsmodul oder einem Synchronisierungsmodul mit einer anderen Filterkonfiguration erstellt. In dieser Ansicht wird eine Liste der **verwaisten** Objekte angezeigt, die nicht mehr verwaltet werden. Sie sollten diese Liste überprüfen, und diese Objekte mit den [Azure AD PowerShell](http://aka.ms/aadposh)-Cmdlets entfernen.

### CS Import
<a id="cs-import" class="xliff"></a>
Beim Öffnen eines Connectorbereichsobjekts befinden sich oben mehrere Registerkarten. Auf der Registerkarte **Importieren** werden die Daten angezeigt, die nach einem Import bereitgestellt werden.  
![CS-Objekt](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/csobject.png)    
Unter **Alter Wert** wird dargestellt, was derzeit im System gespeichert ist, und unter **Neuer Wert** wird angezeigt, was aus dem Quellsystem empfangen, aber noch nicht angewendet wurde. Wenn ein Fehler auf dem Objekt vorhanden ist, werden die Änderungen nicht verarbeitet.

**Fehler**  
![CS-Objekt](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/cssyncerror.png)  
Die Registerkarte **Synchronisierungsfehler** wird nur angezeigt, wenn ein Problem mit dem Objekt besteht. Weitere Informationen finden Sie unter [Problembehandlung bei Synchronisierungsfehlern](#troubleshoot-errors-in-operations-tab).

### CS-Herkunft
<a id="cs-lineage" class="xliff"></a>
Auf der Registerkarte für die Herkunft wird gezeigt, wie das Objekt des Connectorbereichs mit dem Metaverseobjekt verknüpft ist. Sie sehen, wann der Connector zuletzt eine Änderung aus dem verbundenen System importiert hat und welche Regeln zum Auffüllen der Daten im Metaverse angewendet wurden.  
![CS-Herkunft](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/cslineage.png)  
In der Spalte **Aktion** sehen Sie eine einzelne Synchronisierungsregel vom Typ **Eingehend** mit der Aktion **Bereitstellen**. Damit wird angegeben, dass das Metaverse-Objekt erhalten bleibt, solange dieses Objekt des Connectorbereichs vorhanden ist. Wenn die Liste mit den Synchronisierungsregeln hingegen eine Synchronisierungsregel mit der Richtung **Outbound** (Ausgehend) und der Aktion **Provision** (Bereitstellen) enthält, wird beim Löschen des Metaverseobjekts auch dieses Objekt gelöscht.  
![Synchronization Service Manager](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/cslineageout.png)  
Darüber hinaus sehen Sie in der Spalte für die Kennwortsynchronisierung (**PasswordSync**), dass der eingehende Connectorbereich Kennwortänderungen beitragen kann, da eine Synchronisierungsregel den Wert **TRUE** besitzt. Dieses Kennwort wird anschließend über die ausgehende Regel an Azure AD gesendet.

Auf der Registerkarte für die Herkunft gelangen Sie zum Metaverse, indem Sie auf [Metaverse Object Properties](#mv-attributes)(Metaverse-Objekteigenschaften) klicken.

Unterhalb der Registerkarten befinden sich zwei Schaltflächen: **Preview** (Vorschau) und **Log** (Protokoll).

### Vorschau
<a id="preview" class="xliff"></a>
Die Vorschauseite wird verwendet, um ein einzelnes Objekt zu synchronisieren. Sie ist nützlich, wenn Sie Probleme mit einigen benutzerdefinierten Synchronisierungsregeln behandeln und die Auswirkungen einer Änderung auf ein einzelnes Objekt sehen möchten. Sie können zwischen **Vollständige Synchronisierung** und **Deltasynchronisierung** auswählen. Außerdem haben Sie die Wahl zwischen **Generate Preview** (Vorschau generieren), um die Änderung nur im Arbeitsspeicher beizubehalten, und **Commit Preview** (Commitvorschau), um die Metaverse zu aktualisieren und alle Änderungen im Zielconnectorbereich bereitzustellen.  
![Synchronization Service Manager](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/preview.png)  
Sie können das Objekt und die für einen bestimmten Attributfluss angewendete Regel prüfen.  
![Synchronization Service Manager](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/previewresult.png)

### Protokoll
<a id="log" class="xliff"></a>
Die Protokollseite wird verwendet, um den Kennwortsynchronisierungsstatus und den Verlauf anzuzeigen. Weitere Informationen finden Sie unter [Problembehandlung bei der Kennwortsynchronisierung](active-directory-aadconnectsync-troubleshoot-password-synchronization.md).

## Objekteigenschaften der Metaverse
<a id="metaverse-object-properties" class="xliff"></a>
Es ist in der Regel besser, mit der Suche im Active Directory-[Connectorbereich](#connector-space) zu starten. Aber Sie können die Suche auch aus der Metaverse starten.

### Ein Objekt in der MV suchen
<a id="search-for-an-object-in-the-mv" class="xliff"></a>
Klicken Sie im **Synchronization Service Manager** auf **Metaverse Search**(Metaverse-Suche). Erstellen Sie eine Abfrage, bei der Sie wissen, dass sie den Benutzer findet. Sie können nach allgemeinen Attributen, z.B. „accountName (sAMAccountName)“ und „userPrincipalName“ suchen. Weitere Informationen finden Sie unter [Metaversesuche](active-directory-aadconnectsync-service-manager-ui-mvsearch.md).
![Synchronization Service Manager](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/mvsearch.png)  

Klicken Sie im Fenster **Suchergebnisse** auf das Objekt.

Wenn Sie das Objekt nicht gefunden haben, dann hat es die Metaverse noch nicht erreicht. Suchen Sie das Objekt weiter im Active Directory-[Connectorbereich](#connector-space-object-properties). Möglicherweise liegt ein Fehler aus der Synchronisierung vor, der das Objekt daran hindert in die Metaverse zu gelangen, oder es könnte ein Filter angewendet werden.

### MV-Attribute
<a id="mv-attributes" class="xliff"></a>
: Auf der Registerkarte „Attribute“ sehen Sie die Werte und den Connector, von dem sie stammen.  
![Synchronization Service Manager](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/mvobject.png)  

Wenn ein Objekt nicht synchronisiert ist, sehen Sie sich die folgenden Attribute im Metaverse an:
- Ist das Attribut **cloudFiltered** vorhanden, und wird es auf **TRUE** festgelegt? Wenn es so ist, dann wurde es anhand der Schritte in der [attributbasierten Filterung](active-directory-aadconnectsync-configure-filtering.md#attribute-based-filtering) gefiltert.
- Ist das Attribut **sourceAnchor** vorhanden? Wenn dies nicht der Fall ist, haben Sie eine Topologie mit Kontoressourcengesamtstruktur? Wenn ein Objekt als ein verknüpftes Postfach identifiziert wird (das Attribut **msExchRecipientTypeDetails** hat den Wert 2), dann wird „sourceAnchor“ von einem aktivierten Konto für Active Directory-Gesamtstruktur bereitgestellt. Stellen Sie sicher, dass das Hauptkonto ordnungsgemäß importiert und synchronisiert wurde. Das Hauptkonto muss bei den [Connectors](#mv-connectors) für das Objekt aufgelistet sein.

### MV-Connectors
<a id="mv-connectors" class="xliff"></a>
: Auf der Registerkarte „Connectors“ werden alle Connectorbereiche angezeigt, die über eine Darstellung des Objekts verfügen.  
![Synchronization Service Manager](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/mvconnectors.png)  
Sie müssen einen Connector haben für:

- Jede Active Directory-Gesamtstruktur, in der der Benutzer dargestellt wird. Diese Darstellung kann foreignSecurityPrincipals und Contact-Objekte enthalten.
- Ein Connector in Azure AD.

Wenn Ihnen der Azure AD-Connector fehlt, dann lesen Sie [MV-Attribute](#MV-attributes), um die Kriterien, die für Azure AD bereitgestellt werden, zu überprüfen.

Auf dieser Registerkarte können Sie auch zum [Connectorbereichsobjekt](#connector-space-object-properties) navigieren. Wählen Sie eine Zeile aus, und klicken Sie auf **Eigenschaften**.

## Nächste Schritte
<a id="next-steps" class="xliff"></a>
Weitere Informationen zur Konfiguration der [Azure AD Connect-Synchronisierung](active-directory-aadconnectsync-whatis.md) .

Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md).

