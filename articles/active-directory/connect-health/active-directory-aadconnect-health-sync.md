---
title: Verwenden von Azure AD Connect Health mit Synchronisierung | Microsoft-Dokumentation
description: "Auf dieser Seite zu Azure AD Connect Health wird erläutert, wie eine Überwachung mit Azure AD Connect für die Synchronisierung durchgeführt wird."
services: active-directory
documentationcenter: 
author: karavar
manager: femila
ms.assetid: 1dfbeaba-bda2-4f68-ac89-1dbfaf5b4015
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/18/2017
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: b9a3b64d9de48f17a295ca7a9ea58cf26e8f83ed
ms.openlocfilehash: f5d5f238f8f57c8dcf7a3759274f9d565e73ecce
ms.contentlocale: de-de
ms.lasthandoff: 02/28/2017

---
# <a name="monitor-azure-ad-connect-sync-with-azure-ad-connect-health"></a>Überwachen der Azure AD Connect-Synchronisierung mit Azure AD Connect Health
Die folgende Dokumentation bezieht sich auf die Überwachung von Azure AD Connect (Sync) mithilfe von Azure AD Connect Health.  Informationen zum Überwachen von AD FS mit Azure AD Connect Health finden Sie unter [Verwenden von Azure AD Connect Health mit AD FS](active-directory-aadconnect-health-adfs.md). Informationen zur Überwachung der Active Directory-Domänendienste mit Azure AD Connect Health finden Sie unter [Verwenden von Azure AD Connect Health mit AD DS](active-directory-aadconnect-health-adds.md).

![Azure AD Connect Health für die Synchronisierung](./media/active-directory-aadconnect-health-sync/sync-blade.png)

## <a name="alerts-for-azure-ad-connect-health-for-sync"></a>Warnungen für Azure AD Connect Health für die Synchronisierung
Im Abschnitt mit Azure AD Connect Health-Warnungen für die Synchronisierung wird eine Liste der aktiven Warnungen angezeigt. Jede Warnung umfasst relevante Informationen, Lösungsschritte und Links zur verwandten Dokumentation. Durch Auswahl einer aktiven oder behobenen Warnung wird ein neues Blatt angezeigt. Dieses enthält zusätzliche Informationen, Lösungsschritte und Links zu weiterführender Dokumentation. Sie können außerdem Verlaufsdaten zu bereits behobenen Warnungen anzeigen.

Durch Auswahl einer Warnung werden zusätzliche Informationen, Lösungsschritte sowie Links zu weiterführender Dokumentation angezeigt.

![Azure AD Connect-Synchronisierungsfehler](./media/active-directory-aadconnect-health-sync/alert.png)

### <a name="limited-evaluation-of-alerts"></a>Beschränkte Auswertung von Warnungen
Wenn Azure AD Connect die Standardkonfiguration NICHT verwendet (z. B. wenn die Attributfilterung von der Standardkonfiguration in eine benutzerdefinierte Konfiguration geändert wird), lädt der Azure AD Connect Health-Agent die Fehlerereignisse im Zusammenhang mit Azure AD Connect nicht hoch.

Dies schränkt die Auswertung der Warnungen vom Dienst ein. Ihnen wird im Azure-Portal unter Ihrem Dienst ein Banner angezeigt, das auf diese Bedingung hinweist.

![Azure AD Connect Health für die Synchronisierung](./media/active-directory-aadconnect-health-sync/banner.png)

Sie können dies ändern, indem Sie auf „Einstellungen“ klicken und das Hochladen aller Fehlerprotokolle durch den Azure AD Connect Health-Agent erlauben.

![Azure AD Connect Health für die Synchronisierung](./media/active-directory-aadconnect-health-sync/banner2.png)

## <a name="sync-insight"></a>Einblick in die Synchronisierung
Administratoren möchten häufig wissen, wie lange die Synchronisierung von Änderungen für Azure AD dauert und in welcher Menge Änderungen durchgeführt werden. Mit dieser Funktion kann dies leicht visualisiert werden, indem die unten angegebenen Graphen verwendet werden:   

* Latenz von Synchronisierungsvorgängen
* Trends bei der Objektänderung

### <a name="sync-latency"></a>Synchronisierungslatenz
Dieses Feature bietet eine grafische Darstellung des Trends bei Synchronisierungsvorgängen (Import, Export usw.) für Connectors.  So können Sie einerseits schnell und einfach die Latenz bei Ihren Vorgängen überprüfen (sehr nützlich, wenn viele Änderungen durchgeführt werden). Andererseits können Sie Anomalien bei der Latenz aufdecken, die unter Umständen untersucht werden müssen.

![Synchronisierungslatenz](./media/active-directory-aadconnect-health-sync/synclatency02.png)

Standardmäßig wird nur die Latenz des Exportvorgangs für den Azure AD-Connector angezeigt.  Wenn Sie weitere Vorgänge für den Connector oder Vorgänge von anderen Connectors anzeigen möchten, klicken Sie mit der rechten Maustaste auf das Diagramm, wählen „Diagramm bearbeiten“ oder klicken auf die Schaltfläche „Latenzdiagramm bearbeiten“ und wählen den gewünschten Vorgang und die Connectors.

### <a name="sync-object-changes"></a>Änderungen bei Synchronisierungsobjekten
Dieses Feature bietet eine grafische Darstellung des Trends bei der Anzahl von ausgewerteten und nach Azure AD exportierten Änderungen.  Derzeit ist es schwierig, diese Informationen aus den Synchronisierungsprotokollen zu entnehmen.  Mit diesem Diagramm erhalten Sie nicht nur eine einfachere Möglichkeit zum Überwachen der Anzahl der Änderungen in Ihrer Umgebung, sondern auch einen visuellen Überblick über die aufgetretenen Fehler.

![Synchronisierungslatenz](./media/active-directory-aadconnect-health-sync/syncobjectchanges02.png)

## <a name="object-level-synchronization-error-report-preview"></a>Fehlerbericht zur Synchronisierung auf Objektebene (Vorschau)
Mit diesem Feature kann ein Bericht zu Synchronisierungsfehlern erstellt werden, die auftreten können, wenn Identitätsdaten zwischen Windows Server AD und Azure AD mit Azure AD Connect synchronisiert werden.

* Der Bericht deckt Fehler ab, die mit dem Synchronisierungsclient aufgezeichnet werden (Azure AD Connect-Version 1.1.281.0 oder höher).
* Er enthält die Fehler, die beim letzten Synchronisierungsvorgang auf dem Synchronisierungsmodul aufgetreten sind. („Export“ auf den Azure AD-Connector.)
* Der Azure AD Connect Health-Agent für die Synchronisierung muss über eine ausgehende Verbindung mit den erforderlichen Endpunkten verfügen, damit der Bericht die neuesten Daten enthält.
* Der Bericht wird **alle 30 Minuten aktualisiert**, indem die Daten verwendet werden, die vom Azure AD Connect Health-Agent für die Synchronisierung hochgeladen werden.
  Folgende Hauptfunktionen sind verfügbar:

  * Kategorisierung von Fehlern
  * Liste der Objekte mit Fehler nach Kategorie
  * Alle Daten zu den Fehlern an einem Ort
  * Direkter Vergleich von Objekten mit Fehlern aufgrund eines Konflikts
  * Fehlerbericht-Download als CSV (in Kürze verfügbar)

### <a name="categorization-of-errors"></a>Kategorisierung von Fehlern
Im Bericht werden die vorhandenen Synchronisierungsfehler nach den folgenden Kategorien unterteilt:

| Kategorie | Beschreibung |
| --- | --- |
| Doppeltes Attribut |Fehler beim Versuch von Azure AD Connect, Objekte mit doppelten Werten von einem oder mehreren Attributen in Azure AD zu erstellen oder zu aktualisieren, die in einem Mandanten eindeutig sein müssen, z.B. proxyAddresses, UserPrincipalName. |
| Datenkonflikt |Fehler, wenn beim Soft Match-Vorgang für Objekte keine Übereinstimmung erzielt werden kann und dies zu Synchronisierungsfehlern führt. |
| Fehler bei der Datenüberprüfung |Fehler aufgrund von ungültigen Daten, z.B. nicht unterstützten Zeichen in kritischen Attributen wie UserPrincipalName, und Formatfehler, die die Überprüfung vor dem Schreiben in Azure AD nicht bestehen. |
| Großes Attribut |Fehler, wenn mindestens ein Attribut größer als die zulässige Größe, Länge oder Anzahl ist. |
| Sonstige |Alle anderen Fehler, die nicht in die obigen Kategorien passen. Diese Kategorie wird basierend auf dem Feedback in Unterkategorien unterteilt. |

![Zusammenfassung des Synchronisierungsfehlerberichts](./media/active-directory-aadconnect-health-sync/errorreport01.png)
![Kategorien des Synchronisierungsfehlerberichts](./media/active-directory-aadconnect-health-sync/errorreport02.png)

### <a name="list-of-objects-with-error-per-category"></a>Liste der Objekte mit Fehler nach Kategorie
Für die einzelnen Kategorien können Sie jeweils die Liste mit den Objekten anzeigen, die den Fehler in der Kategorie aufweisen.
![Liste des Synchronisierungsfehlerberichts](./media/active-directory-aadconnect-health-sync/errorreport03.png)

### <a name="error-details"></a>Fehlerdetails
Die folgenden Daten stehen für jeden Fehler in der ausführlichen Ansicht zur Verfügung:

* Bezeichner für das beteiligte *AD-Objekt*
* Bezeichner für das beteiligte *Azure AD-Objekt* (falls zutreffend)
* Fehlerbeschreibung und -behebung
* Verwandte Artikel

![Details zum Synchronisierungsfehlerbericht](./media/active-directory-aadconnect-health-sync/errorreport04.png)

### <a name="download-the-error-report-as-csv"></a>Fehlerbericht-Download als CSV
Durch Auswählen der Schaltfläche „Export“ können Sie eine CSV-Datei mit allen Details zu allen Fehlern herunterladen.

## <a name="related-links"></a>Verwandte Links
* [Troubleshooting Errors during synchronization](../connect/active-directory-aadconnect-troubleshoot-sync-errors.md) (Beheben von Fehlern während der Synchronisierung)
* [Resilienz bei doppelten Attributen](../connect/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md)
* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Installieren des Azure AD Connect Health-Agents](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect Health-Vorgänge](active-directory-aadconnect-health-operations.md)
* [Verwenden von Azure AD Connect Health mit AD FS](active-directory-aadconnect-health-adfs.md)
* [Verwenden von Azure AD Connect Health mit AD DS](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect Health – FAQ](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect Health: Versionsverlauf](active-directory-aadconnect-health-version-history.md)
