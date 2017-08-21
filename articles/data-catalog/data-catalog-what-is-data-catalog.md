---
title: "Einführung in Azure Data Catalog | Microsoft-Dokumentation"
description: "Dieser Artikel enthält eine Übersicht über Microsoft Azure Data Catalog, einschließlich der Features des Diensts und der Probleme, die er beheben soll. Data Catalog ermöglicht Benutzern das Registrieren, Ermitteln, Verstehen und Nutzen von Datenquellen."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: cc733907-17ec-4153-9f0c-5b3754b2db19
ms.service: data-catalog
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.translationtype: Human Translation
ms.sourcegitcommit: 532ff423ff53567b6ce40c0ea7ec09a689cee1e7
ms.openlocfilehash: c0cf2805de958c979def3f21eda59ec97fb91d33
ms.contentlocale: de-de
ms.lasthandoff: 06/05/2017

---
# <a name="what-is-azure-data-catalog"></a>Was ist Azure Data Catalog?
Azure Data Catalog ist ein vollständig verwalteter Clouddienst, dessen Benutzer benötigte Datenquellen ermitteln und gefundene Datenquellen verstehen können. Gleichzeitig hilft Data Catalog Organisationen, einen größeren Nutzen aus ihren vorhandenen Investitionen zu erzielen. 

Mit Data Catalog kann jeder Benutzer (Analytiker, Datenanalysten und Entwickler) Datenquellen ermitteln, verstehen und nutzen. Data Catalog enthält ein Crowdsourcingmodell von Metadaten und Anmerkungen. Es handelt sich um einen zentralen Ort für alle Benutzer einer Organisation, an dem sie ihre Kenntnisse beitragen und eine Community sowie eine Datenkultur aufbauen können.

## <a name="discovery-challenges-for-data-consumers"></a>Aufgaben der Ermittlung für Datennutzer
Das Ermitteln der Datenquellen von Unternehmen war bisher ein natürlicher Prozess, der auf „Stammeswissen“ basierte. In Unternehmen, die aus ihren Informationsobjekten den größtmöglichen Nutzen ziehen möchten, sorgt dieses Konzept für eine Vielzahl von Herausforderungen:

* Benutzern ist möglicherweise nicht bekannt, dass eine Datenquelle vorhanden ist, sofern sie nicht im Rahmen eines anderen Prozesses in Berührung mit ihnen geraten. Es gibt keinen zentralen Speicherort, in dem Datenquellen registriert sind.
* Wenn Benutzer den Speicherort einer Datenquelle nicht kennen, können sie nicht mithilfe einer Clientanwendung eine Verbindung mit den Daten herstellen. Um die Daten nutzen zu können, müssen Benutzer die Verbindungszeichenfolge oder den Pfad kennen.
* Wenn Benutzer den Speicherort der Dokumentation einer Datenquelle nicht kennen, können sie die beabsichtigte Verwendung der Daten nicht verstehen. Datenquellen und Dokumentation befinden sich möglicherweise an unterschiedlichen Orten und können über verschiedene Möglichkeiten genutzt werden.
* Wenn Benutzer Fragen zu einem Informationsobjekt haben, müssen sie den Experten oder das Team suchen, der bzw. das für die Daten verantwortlich ist, und sich offline an diese Personen wenden. Es gibt keine ausdrückliche Verbindung zwischen den Daten und den Experten.
* Ein Benutzer muss den Prozess zum Anfordern des Zugriffs auf die Datenquelle verstehen, da er mit der ermittelten Datenquelle und der zugehörigen Dokumentation allein noch nicht auf die Daten zugreifen kann.

## <a name="discovery-challenges-for-data-producers"></a>Aufgaben der Ermittlung für Datenproduzenten
Trotz der genannten Hürden für die Nutzer der Daten gelten für Benutzer, die für die Erstellung und Verwaltung von Informationsobjekten verantwortlich sind, besondere Anforderungen:

* Das Erstellen von Anmerkungen zu Datenquellen mit beschreibenden Metadaten ist häufig nutzlos. Clientanwendungen ignorieren in der Regel Beschreibungen, die in der Datenquelle gespeichert sind.
* Das Erstellen der Dokumentation für Datenquellen ist häufig nutzlos. Das Synchronisieren der Dokumentation mit Datenquellen ist eine fortlaufende Aufgabe, da Benutzer einer Dokumentation, die als veraltet angesehen wird, möglicherweise nicht vertrauen.
* Das Erstellen und Verwalten der Dokumentation für eine Datenquelle ist komplex und zeitaufwendig. Dies gilt mitunter noch stärker für die Aufgabe, die Dokumentation für alle Personen, die die Datenquelle verwenden, leicht zugänglich zu machen.
* Eine beständige Herausforderung besteht darin, den Zugriff auf die Datenquelle zu beschränken und sicherzustellen, dass die Datennutzer wissen, wie sie den Zugriff anfordern.

Zusammen stellen diese Probleme eine erhebliche Hürde für Unternehmen dar, die die Nutzung und das Verständnis der Unternehmensdaten fördern möchten.

## <a name="azure-data-catalog-can-help"></a>Hilfe von Azure Data Catalog
Data Catalog ist für die Lösung dieser Probleme ausgelegt und hilft Unternehmen dabei, mit den vorhandenen Datenressourcen den größten Nutzen zu erzielen. Data Catalog gestaltet Datenquellen einfach ermittelbar und verständlich für die Benutzer, die die verwalteten Daten benötigen.

Data Catalog verfügt über einen cloudbasierten Dienst, unter dem eine Datenquelle registriert werden kann. Die Daten verbleiben an ihrem vorhandenen Speicherort, aber eine Kopie der Metadaten wird Data Catalog hinzugefügt, zusammen mit einem Verweis auf den Speicherort der Datenquelle. Die Metadaten werden außerdem indiziert, damit jede Datenquelle per Suchfunktion einfach ermittelt werden kann und für die Benutzer, die sie ermitteln, verständlich ist.

Nachdem eine Datenquelle registriert wurde, können die zugehörigen Metadaten erweitert werden. Dies kann entweder von dem Benutzer übernommen werden, der sie registriert hat, oder von anderen Benutzern des Unternehmens. Alle Benutzer können eine Datenquelle mit Anmerkungen versehen, indem Beschreibungen, Tags oder andere Metadaten bereitgestellt werden, z. B. Dokumentation und Prozesse für das Anfordern des Datenquellenzugriffs. Diese beschreibenden Metadaten sind eine Erweiterung der strukturellen Metadaten (z.B. Spaltennamen und Datentypen), die für die Datenquelle registriert wurden.

Das Ermitteln und Verstehen von Datenquellen und ihrer Nutzung sind der Hauptzweck bei der Registrierung von Quellen. Enterprisebenutzer benötigen möglicherweise Daten für Business Intelligence, Anwendungsentwicklung, Data Science oder andere Aufgaben, für die spezifische Daten erforderlich sind. Sie können über die Ermittlung mit Data Catalog schnell die ihren Anforderungen entsprechenden Daten suchen, die Daten verstehen, damit sie ihre Eignung für den jeweiligen Zweck einordnen können, und die Daten nutzen, indem sie die Datenquelle im Tool ihrer Wahl öffnen. 

Gleichzeitig können Benutzer Beiträge zum Katalog leisten, indem sie bereits registrierte Datenquellen mit Tags versehen, dokumentieren und kommentieren. Sie können auch neue Datenquellen registrieren, die dann ermittelt, verstanden, und von der Community der Katalogbenutzer genutzt werden können.

![Funktionen von Data Catalog](./media/data-catalog-what-is-data-catalog/data-catalog-capabilities.png)

## <a name="learn-more-about-data-catalog"></a>Weitere Informationen zu Data Catalog
Weitere Informationen zu den Funktionen von Data Catalog finden Sie unter:

* [Registrieren von Datenquellen](data-catalog-how-to-register.md)
* [Ermitteln von Datenquellen](data-catalog-how-to-discover.md)
* [Hinzufügen von Anmerkungen zu Datenquellen](data-catalog-how-to-annotate.md)
* [So dokumentieren Sie Datenquellen](data-catalog-how-to-documentation.md)
* [Herstellen einer Verbindung mit Datenquellen](data-catalog-how-to-connect.md)
* [Arbeiten mit großen Datenquellen in Azure Data Catalog](data-catalog-how-to-big-data.md)
* [Verwalten von Datenassets](data-catalog-how-to-manage.md)
* [Einrichten des Unternehmensglossars](data-catalog-how-to-business-glossary.md)
* [Häufig gestellte Fragen](data-catalog-frequently-asked-questions.md)

## <a name="next-steps"></a>Nächste Schritte
Für die ersten Schritte mit Data Catalog besuchen Sie:
* [Microsoft Azure Data Catalog](https://www.azuredatacatalog.com)
* [Erste Schritte mit Azure Data Catalog](data-catalog-get-started.md)

