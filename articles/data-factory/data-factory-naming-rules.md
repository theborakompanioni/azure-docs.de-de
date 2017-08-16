---
title: "Regeln für die Benennung von Azure Data Factory-Entitäten | Microsoft-Dokumentation"
description: "Beschreibt die Benennungsregeln für Data Factory-Entitäten."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: bc5e801d-0b3b-48ec-9501-bb4146ea17f1
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: shlo
ms.translationtype: Human Translation
ms.sourcegitcommit: 1e6f2b9de47d1ce84c4043f5f6e73d462e0c1271
ms.openlocfilehash: e8f190ec8f6cffdcc5748454ab7ae765648f2e30
ms.contentlocale: de-de
ms.lasthandoff: 06/21/2017

---
# <a name="azure-data-factory---naming-rules"></a>Azure Data Factory – Benennungsregeln
Die folgende Tabelle enthält Benennungsregeln für Data Factory-Artefakte.

| Name | Eindeutigkeit des Namens | Überprüfungen |
|:--- |:--- |:--- |
| Data Factory |Für Microsoft Azure eindeutig. Bei Namen wird die Groß-/Kleinschreibung nicht berücksichtigt, sodass `MyDF` und `mydf` auf die gleiche Data Factory verweisen. |<ul><li>Jede Data Factory ist an genau ein Azure-Abonnement gebunden.</li><li>Objektnamen müssen mit einem Buchstaben oder einer Zahl beginnen und dürfen nur Buchstaben, Zahlen und Bindestriche (-) enthalten.</li><li>Jedem Bindestrich (-) muss unmittelbar ein Buchstabe oder eine Ziffer vorangestellt werden und er muss von diesen gefolgt sein. Aufeinanderfolgende Bindestriche sind in Containernamen nicht zulässig.</li><li>Namen können 3 bis 63 Zeichen lang sein.</li></ul> |
| Verknüpfte Dienste/Tabellen/Pipelines |Innerhalb einer Data Factory eindeutig. Bei Namen wird die Groß-/Kleinschreibung nicht berücksichtigt. |<ul><li>Maximale Anzahl von Zeichen in einem Tabellennamen: 260.</li><li>Objektnamen müssen mit einem Buchstaben, einer Zahl oder einem Unterstrich (_) beginnen.</li><li>Folgende Zeichen sind nicht zulässig: „.“, „+“, „?“, „/“, „<“, „>“, „*“, „%“, „&“, „:“, „\\“.</li></ul> |
| Ressourcengruppe |Für Microsoft Azure eindeutig. Bei Namen wird die Groß-/Kleinschreibung nicht berücksichtigt. |<ul><li>Maximale Anzahl von Zeichen: 1000.</li><li>Der Name kann Buchstaben, Zahlen und die folgenden Zeichen enthalten: „-“, „_“, „,“ und „.“.</li></ul> |


