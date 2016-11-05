---
title: Häufig gestellte Fragen zu Azure Data Catalog | Microsoft Docs
description: Häufig gestellte Fragen zu Azure Data Catalog, einschließlich Funktionen für die Ermittlung von Datenquellen, Anmerkungen und Verwaltung.
services: data-catalog
documentationcenter: ''
author: steelanddata
manager: NA
editor: ''
tags: ''

ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 10/04/2016
ms.author: maroche

---
# <a name="azure-data-catalog-frequently-asked-questions"></a>Häufig gestellte Fragen zu Azure Data Catalog
Dieser Artikel enthält Antworten auf häufig gestellte Fragen im Zusammenhang mit dem **Microsoft Azure Data Catalog** -Dienst.

## <a name="q:-what-is-azure-data-catalog?"></a>F: Was ist Azure Data Catalog?
Microsoft Azure Data Catalog ist ein vollständig verwalteter Dienst, der in der Microsoft Azure-Cloud gehostet wird. Er dient als Registrierungs- und Ermittlungssystem für die Datenquellen von Unternehmen. Azure Data Catalog enthält Funktionen, mit denen alle Benutzer – von Analysten über Datenwissenschaftler bis zu Entwicklern – Datenquellen registrieren, ermitteln, verstehen und nutzen können.

## <a name="q:-what-customer-challenges-does-azure-data-catalog-solve?"></a>F: Welche Kundenprobleme werden mit Azure Data Catalog gelöst?
Mit Azure Data Catalog werden Probleme bei der Ermittlung von Datenquellen und „obskuren Daten“ gelöst, indem Benutzer Datenquellen von Unternehmen ermitteln und verstehen können.

## <a name="q:-who-are-the-target-audiences-for-azure-data-catalog?"></a>F: Was sind die Zielgruppen von Azure Data Catalog?
Azure Data Catalog enthält Funktionen für technisch versierte und andere Benutzer, z. B.:

* Datenentwickler, BI- und Analyseexperten: Personen, die für die Produktion von Daten- und Analyseinhalten zur Verwendung durch andere Benutzer verantwortlich sind
* Data Stewards: Personen, die über die Kenntnisse zu den Daten verfügen, ihre Bedeutung kennen und über die beabsichtigte Nutzung und den Zweck Bescheid wissen
* Datennutzer: Personen, die Daten leicht ermitteln und verstehen und Verbindungen damit herstellen müssen, um ihre Arbeit mit dem Tool ihrer Wahl zu erledigen
* Zentrale IT-Abteilung: Personen, die Hunderte von Datenquellen für die Ermittlung durch geschäftliche Benutzer bereitstellen und die überwachen müssen, wie und von wem Daten verwendet werden

## <a name="q:-what-is-the-azure-data-catalog-region-availability?"></a>F: In welchen Regionen ist Azure Data Catalog verfügbar?
Azure Data Catalog-Dienste sind derzeit in den folgenden Rechenzentren verfügbar:

* Westen USA
* USA (Ost)
* Westeuropa
* Nordeuropa
* Australien (Osten)
* Südostasien

## <a name="q:-what-are-the-limits-on-the-number-of-data-assets-in-azure-data-catalog?"></a>F: Welche Grenzwerte gelten für die Anzahl der Datenassets in Azure Data Catalog?
Die Free Edition von Azure Data Catalog ist auf 5.000 registrierte Datenassets begrenzt.

Die Standard Edition von Azure Data Catalog unterstützt bis zu 100.000 registrierte Datenassets.

## <a name="q:-what-are-the-supported-data-source-and-asset-types?"></a>F: Welche Datenquellen und Assettypen werden unterstützt?
Eine Liste mit den derzeit unterstützten Datenquellen finden Sie in der [Datenquellenreferenz (DSR) für Data Catalog](data-catalog-dsr.md) .

## <a name="q:-how-do-i-request-support-for-another-data-source?"></a>F: Wie fordere ich die Unterstützung für eine andere Datenquelle an?
Für Anfragen zu Features und anderes Feedback können Sie das [Azure Data Catalog-Forum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409)verwenden.

## <a name="q:-how-do-i-get-started-with-azure-data-catalog?"></a>F: Wie beginne ich mit der Nutzung von Azure Data Catalog?
Befolgen Sie am besten die Anleitung unter [Erste Schritte mit Data Catalog](data-catalog-get-started.md). Dieser Artikel enthält eine umfassende Übersicht der Funktionen des Diensts.

## <a name="q:-how-do-i-register-my-data?"></a>F: Wie registriere ich meine Daten?
Starten Sie zum Registrieren Ihrer Daten in Azure Data Catalog das Azure Data Catalog-Registrierungstool, indem Sie im Azure Data Catalog-Portal den Bereich „Veröffentlichen“ verwenden. Melden Sie sich in der Azure Data Catalog-Veröffentlichungsanwendung mit denselben Anmeldeinformationen wie für das Azure Data Catalog-Portal an, und wählen Sie die Datenquelle und das spezielle Asset aus, die bzw. das Sie registrieren möchten.

## <a name="q:-what-properties-are-extracted-for-data-assets-that-are-registered?"></a>F: Welche Eigenschaften werden für Datenassets extrahiert, die registriert werden?
Die spezifischen Eigenschaften variieren je nach Datenquelle, aber im Allgemeinen werden mit dem Azure Data Catalog-Veröffentlichungsdienst die folgenden Informationen extrahiert:

* Assetname
* Assettyp
* Assetbeschreibung
* Namen von Attributen/Spalten
* Datentypen von Attributen/Spalten
* Beschreibung von Attributen/Spalten

> [!IMPORTANT]
> Durch das Registrieren von Datenassets mit Azure Data Catalog werden Ihre Daten nicht in die Cloud verschoben oder kopiert. Beim Registrieren von Assets einer Datenquelle werden die Metadaten des Assets nach Azure kopiert, aber die eigentlichen Daten verbleiben am vorhandenen Speicherort der Datenquelle. Die einzige Ausnahme von dieser Regel ist, wenn ein Benutzer sich beim Registrieren von Assets zum Hochladen von Vorschaudatensätzen oder eines Datenprofils entscheidet. Beim Hinzufügen einer Vorschau werden bis zu 20 Datensätze aus jedem Asset kopiert und als Momentaufnahme in Azure Data Catalog gespeichert. Beim Einschließen eines Datenprofils werden aggregierte Informationen (z. B. die Größe von Tabellen, der Prozentsatz von NULL-Werten pro Spalte und die niedrigsten, höchsten und durchschnittlichen Werte für Spalten) berechnet und in die im Katalog gespeicherten Metadaten eingeschlossen.
> 
> 

<br/>

> [!NOTE]
> Für Datenquellen wie SQL Server Analysis Services, die über eine leistungsstarke **Description** -Eigenschaft verfügen, wird dieser Eigenschaftswert von der Azure Data Catalog-Veröffentlichungsanwendung extrahiert. Für relationale SQL Server-Datenbanken ohne leistungsstarke **Description**-Eigenschaft extrahiert die Azure Data Catalog-Veröffentlichungsanwendung den Wert aus der erweiterten Eigenschaft „ms_description“ für Objekte und Spalten. Weitere Informationen finden Sie bei TechNet unter [Verwenden von erweiterten Eigenschaften für Datenbankobjekte](https://technet.microsoft.com/library/ms190243%28v=sql.105%29.aspx).
> 
> 

## <a name="q:-how-long-should-it-take-for-newly-registered-assets-to-appear-in-azure-data-catalog?"></a>F: Wie lange sollte es dauern, bis neu registrierte Assets in Azure Data Catalog angezeigt werden?
Nach dem Registrieren von Assets mit Azure Data Catalog kann es fünf bis zehn Sekunden dauern, bevor sie im Azure Data Catalog-Portal angezeigt werden.

## <a name="q:-how-do-i-annotate-and-enrich-the-metadata-for-my-registered-data-assets?"></a>F: Wie kann ich die Metadaten für meine registrierten Datenassets mit Anmerkungen versehen und erweitern?
Die einfachste Möglichkeit, Metadaten für registrierte Assets bereitzustellen, ist das Asset im Azure Data Catalog-Portal auszuwählen und anschließend die Metadatenwerte im Eigenschaften- oder Schemabereich für das ausgewählte Objekt einzugeben.

Sie können auch während des Registrierungsvorgangs einige Metadaten angeben, z. B. Experten und Tags. Die im Azure Data Catalog-Veröffentlichungsdienst angegebenen Werte gelten für alle Assets, die zum jeweiligen Zeitpunkt registriert werden. Um die kürzlich registrierten Objekte im Portal anzuzeigen und ihnen neue Anmerkungen hinzuzufügen, wählen Sie auf dem letzten Bildschirm der Azure Data Catalog-Veröffentlichungsanwendung die Schaltfläche **Portal anzeigen**.

## <a name="q:-how-do-i-delete-my-registered-data-objects?"></a>F: Wie lösche ich meine registrierten Datenobjekte?
Sie können ein Objekt aus Azure Data Catalog löschen, indem Sie das Objekt im Portal auswählen und dann auf die Schaltfläche **Löschen** klicken. Die Metadaten des Objekts werden dann aus Azure Data Catalog entfernt. Dies wirkt sich jedoch nicht auf die zugrunde liegende Datenquelle aus.

## <a name="q:-what-is-an-expert?"></a>F: Was ist ein Experte?
Ein Experte ist eine Person, die über fundiertes Wissen zu einem Datenobjekt verfügt. Ein Objekt kann mehrere Experten haben. Ein Experte muss nicht unbedingt der Besitzer eines Objekts sein. Es handelt sich lediglich um eine Person, die weiß, wie die Daten verwendet werden können und sollten.

## <a name="q:-how-do-i-share-information-with-the-azure-data-catalog-team-if-i-encounter-problems?"></a>F: Wie stelle ich Informationen für das Azure Data Catalog-Team bereit, wenn Probleme auftreten?
Nutzen Sie bitte das Azure Data Catalog-Forum, um Probleme zu melden, Informationen zu teilen und Fragen zu stellen. Das Forum finden Sie unter „http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409“.

## <a name="q:-does-azure-data-catalog-work-with-this-other-data-source-i’m-interested-in?"></a>F: Funktioniert Azure Data Catalog auch mit anderen Datenquellen, an denen ich interessiert bin?
Wir arbeiten aktiv daran, weitere Datenquellen zu Azure Data Catalog hinzuzufügen. Wenn Sie sich für eine bestimmte Datenquelle Unterstützung wünschen, können Sie dies im [Azure Data Catalog-Forum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409) vorschlagen (oder eine ggf. bereits vorhandene Anfrage unterstützen).

## <a name="q:-how-is-azure-data-catalog-related-to-the-data-catalog-in-power-bi-for-office-365?"></a>F: Welche Verbindung besteht zwischen Azure Data Catalog und Data Catalog in Power BI für Office 365?
Sie können sich Azure Data Catalog als Weiterentwicklung von Data Catalog vorstellen. Azure Data Catalog verfügt über ähnliche Funktionen zur Veröffentlichung und Ermittlung von Datenquellen, ist aber auf weiter gefasste Szenarios ausgerichtet und nicht von Office 365 abhängig. Kurz nach der allgemeinen Verfügbarkeit von Azure Data Catalog werden die beiden Kataloge zu einem einzelnen Dienst zusammengefasst.

## <a name="q:-what-permissions-does-a-user-need-to-register-assets-with-azure-data-catalog?"></a>F: Welche Berechtigungen benötigt ein Benutzer, um Assets unter Azure Data Catalog registrieren zu können?
Der Benutzer, der das Azure Data Catalog-Registrierungstool ausführt, benötigt für die Datenquelle Berechtigungen, die ihm das Auslesen von Metadaten aus der Quelle ermöglichen. Wenn der Benutzer auch die Einbindung einer Vorschau auswählt, muss er auch über Berechtigungen verfügen, mit denen er die Daten aus den registrierten Objekten einlesen kann.

## <a name="q:-will-azure-data-catalog-be-made-available-for-on-premises-deployment-as-well?"></a>F: Wird Azure Data Catalog auch für die lokale Bereitstellung verfügbar gemacht?
Azure Data Catalog ist ein Clouddienst, der sowohl Clouddatenquellen als auch lokale Datenquellen verarbeiten kann. Es handelt sich also um eine Hybridlösung zur Ermittlung von Datenquellen. Es ist derzeit keine Version des Azure Data Catalog-Diensts geplant, die lokal ausgeführt wird.

## <a name="q:-can-we-extract-more-/-richer-metadata-from-the-data-sources-we-register?"></a>F: Können aus den registrierten Datenquellen auch mehr oder umfangreichere Metadaten extrahiert werden?
Wir arbeiten derzeit daran, die Funktionen von Azure Data Catalog zu erweitern. Wenn zusätzliche Metadaten vorhanden sind, die während der Registrierung aus der Datenquelle extrahiert werden sollen, können Sie dies im [Azure Data Catalog-Forum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409)vorschlagen (oder einen bereits vorhandenen Vorschlag unterstützen). In Zukunft werden auch Drittanbieter berechtigt sein, neue Datenquellentypen per Erweiterbarkeits-API hinzuzufügen.

## <a name="q:-how-do-i-restrict-the-visibility-of-registered-data-assets,-so-that-only-certain-people-can-discover-them?"></a>F: Wie kann ich die Sichtbarkeit der registrierten Datenassets so einschränken, dass nur bestimmte Personen sie ermitteln können?
Wählen Sie die Datenassets in Azure Data Catalog aus, und klicken Sie auf die Schaltfläche „In Besitz nehmen“. Besitzer von Datenassets in Azure Data Catalog können die Einstellungen zur Sichtbarkeit ändern, um entweder für alle Katalogbenutzer die Ermittlung der im Besitz befindlichen Ressourcen zuzulassen oder die Sichtbarkeit auf bestimmte Benutzer zu beschränken.

## <a name="q:-how-do-i-update-the-registration-for-a-data-asset-to-that-changes-in-the-data-source-are-reflected-in-the-catalog?"></a>F: Wie aktualisiere ich die Registrierung für ein Datenasset so, dass Änderungen in der Datenquelle im Katalog widergespiegelt werden?
Zum Aktualisieren der Metadaten für Datenassets, die im Katalog bereits registriert sind, registrieren Sie die Datenquelle mit den Ressourcen einfach erneut. Alle Änderungen in der Datenquelle, z. B. hinzugefügte oder entfernte Spalten in Tabellen oder Ansichten, werden im Katalog aktualisiert, aber alle von Benutzern angegebenen Anmerkungen werden beibehalten.

## <a name="q:-my-question-isn’t-answered-here-–-what-should-i-do?"></a>F: Meine Frage wird hier nicht beantwortet. Was kann ich tun?
Wechseln Sie in das [Azure Data Catalog-Forum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409). Fragen, die dort gestellt werden, werden in diesen Artikel aufgenommen.

<!--HONumber=Oct16_HO2-->


