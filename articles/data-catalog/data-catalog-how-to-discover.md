---
title: Ermitteln von Datenquellen in Azure Data Catalog | Microsoft-Dokumentation
description: "Dieser Artikel bietet Informationen zum Ermitteln von Datenobjekten, die in Azure Data Catalog registriert wurden, einschließlich der Verwendung der Funktionen für Suchen und Filtern sowie Treffermarkierung im Azure Data Catalog-Portal."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: f72ae3a3-6573-4710-89a7-f13555e1968c
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 05/15/2017
ms.author: maroche
ms.translationtype: Human Translation
ms.sourcegitcommit: f7479260c7c2e10f242b6d8e77170d4abe8634ac
ms.openlocfilehash: 1cefc0341789a4751d4e3ee687048ccd291559e3
ms.contentlocale: de-de
ms.lasthandoff: 06/21/2017


---
# <a name="how-to-discover-data-sources-in-azure-data-catalog"></a>Ermitteln von Datenquellen in Azure Data Catalog
## <a name="introduction"></a>Einführung
Azure Data Catalog ist ein vollständig verwalteter Clouddienst, der als Registrierungs- und Ermittlungssystem für Datenquellen von Unternehmen dient. Data Catalog unterstützt Benutzer also beim Ermitteln, Nachvollziehen und Verwenden von Datenquellen und Unternehmen dabei, mehr Nutzen aus ihren vorhandenen Daten zu ziehen. Nachdem eine Datenquelle in Data Catalog registriert wurde, werden ihre Metadaten vom Dienst indiziert, sodass Sie benötigte Daten mühelos ermitteln können.

## <a name="searching-and-filtering"></a>Suchen und Filtern
Für die Ermittlung in Data Catalog werden zwei primäre Mechanismen verwendet: Suchen und Filtern.

Die Suchfunktion ist intuitiv und leistungsstark. Suchbegriffe werden standardmäßig mit jeder Eigenschaft im Katalog abgeglichen, auch mit den von Benutzern hinzugefügten Anmerkungen.

Die Filterfunktion dient als Ergänzung der Suchfunktion. Sie können spezifische Merkmale wie Experten, Datenquellentyp, Objekttyp und Tags auswählen. Sie können nur übereinstimmende Datenobjekte anzeigen und Suchergebnisse auf übereinstimmende Objekte beschränken.

Durch eine Kombination von Suchen und Filtern können Sie schnell durch die in Data Catalog registrierten Datenquellen navigieren, um die gewünschten Datenquellen zu ermitteln.

## <a name="search-syntax"></a>Suchsyntax
Obwohl die standardmäßige Freitextsuche einfach und intuitiv ist, können Sie auch die Suchsyntax von Data Catalog verwenden, um die Suchergebnisse noch präziser zu steuern. Die Data Catalog-Suche unterstützt die folgenden Techniken:

| Technik | Zweck | Beispiel |
| --- | --- | --- |
| Standardsuche |Standardsuche mit mindestens einem Suchbegriff. Als Ergebnisse werden alle Datenobjekte zurückgegeben, bei denen eine beliebige Eigenschaft mit den angegebenen Begriffen übereinstimmt. |`sales data` |
| Eigenschaftsbasierte Suche |Es werden nur Datenquellen zurückgegeben, bei denen der Suchbegriff mit der angegebenen Eigenschaft übereinstimmt. |`name:finance` |
| Boolesche Operatoren |Die Suche wird mithilfe von booleschen Operatoren erweitert oder eingegrenzt. |`finance NOT corporate` |
| Gruppierung mit Klammern |Teile der Abfrage werden mithilfe von Klammern gruppiert, um eine logische Isolierung zu erzielen (insbesondere in Verbindung mit booleschen Operatoren). |`name:finance AND (tags:Q1 OR tags:Q2)` |
| Vergleichsoperatoren |Für Eigenschaften mit numerischen Datentypen und Datumsdatentypen können andere Vergleiche als Gleichheit verwendet werden. |`modifiedTime > "11/05/2014"` |

Weitere Informationen zu Data Catalog suchen finden Sie im Artikel zu [Azure Data Catalog](https://msdn.microsoft.com/library/azure/mt267594.aspx).

## <a name="hit-highlighting"></a>Treffermarkierung
Beim Anzeigen von Suchergebnissen werden alle angezeigten Eigenschaften, die mit den angegebenen Suchbegriffen übereinstimmen (z.B. Name des Datenobjekts, Beschreibung und Tags), hervorgehoben, damit leichter zu erkennen ist, weshalb ein bestimmtes Datenobjekt zurückgegeben wurde.

> [!NOTE]
> Um die Treffermarkierung zu deaktivieren, klicken Sie im Data Catalog-Portal auf die Umschaltfläche **Markieren**.
>
>

Beim Anzeigen von Suchergebnissen ist es selbst bei aktivierter Treffermarkierung u.U. nicht immer offensichtlich, warum ein Datenobjekt in den Ergebnissen enthalten ist. Da standardmäßig alle Eigenschaften durchsucht werden, kann ein Datenobjekt aufgrund einer Übereinstimmung mit einer Eigenschaft auf Spaltenebene zurückgegeben werden. Und da mehrere Benutzer registrierten Datenobjekte eigene Anmerkungen (Tags, Beschreibungen usw.) hinzufügen können, werden möglicherweise nicht alle Metadaten in der Liste der Suchergebnisse angezeigt.

In der standardmäßigen Kachelansicht enthält jede in den Suchergebnissen angezeigte Kachel das Symbol **Übereinstimmungen mit dem Suchbegriff anzeigen**, mit dem Sie schnell die Anzahl der Treffer und ihre Position anzeigen und nach Wunsch zu den Treffern springen können.

 ![Treffermarkierung und Suchergebnisse im Azure Data Catalog-Portal](./media/data-catalog-how-to-discover/search-matches.png)

## <a name="summary"></a>Zusammenfassung
Weil bei Registrierung einer Datenquelle in Data Catalog strukturelle und beschreibende Metadaten aus der Datenquelle in den Katalogdienst kopiert werden, lässt sich die Datenquelle leichter ermitteln und verstehen. Nachdem eine Datenquelle registriert wurde, kann sie mithilfe der Filter- und Suchfunktionen im Data Catalog-Portal von Ihnen ermittelt werden.

## <a name="next-steps"></a>Nächste Schritte
* Detaillierte Informationen zum Ermitteln von Datenquellen finden Sie unter [Erste Schritte mit Azure Data Catalog](data-catalog-get-started.md).

