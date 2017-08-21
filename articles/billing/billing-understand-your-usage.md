---
title: "Grundlegendes über die detaillierte Nutzung in Azure | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie die Abschnitte Ihrer CSV-Datei mit ausführlichen Nutzungsdaten Ihres Azure-Abonnements anzeigen und verstehen."
services: 
documentationcenter: 
author: tonguyen10
manager: tonguyen
editor: 
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: tonguyen
ms.translationtype: Human Translation
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.openlocfilehash: 0d4f2e19ec3417b7ca111f777e34b1ce66ddb32d
ms.contentlocale: de-de
ms.lasthandoff: 07/08/2017


---
# <a name="understand-terms-on-your-microsoft-azure-detailed-usage-charges"></a>Grundlegendes über Benennungen zu den Gebühren in der Datei mit ausführlichen Nutzungsdaten zu Microsoft Azure 
Die CSV-Datei mit ausführlichen Informationen zu Nutzungsgebühren enthält Gebühren für die tägliche Nutzung und die Nutzung auf der Ebene von Verbrauchseinheiten für den aktuellen Abrechnungszeitraum. 

Informationen zum Abrufen der Datei mit ausführlichen Nutzungsdaten finden Sie unter [Vorgehensweise zum Abrufen von Azure-Rechnungen und Daten zur täglichen Nutzung](billing-download-azure-invoice-daily-usage-date.md).
Sie ist als Datei mit kommagetrennten Werten (CSV) verfügbar, die Sie in einem Tabellenkalkulationsprogramm öffnen können. Wenn zwei Versionen verfügbar sind, laden Sie Version 2 herunter. Das ist das aktuellste Dateiformat.

Nutzungsgebühren sind die **monatlichen** Gesamtgebühren für ein Abonnement. Die Nutzungsgebühren berücksichtigen keine Guthaben oder Rabatte.

## <a name="detailed-terms-and-descriptions-of-your-detailed-usage-file"></a>Detaillierte Erläuterungen zu Benennungen und Beschreibungen der Datei mit ausführlichen Nutzungsdaten
In den folgenden Abschnitten werden die in Version 2 der Datei mit den detaillierten Informationen zur Nutzung verwendeten Benennungen beschrieben.

### <a name="statement"></a>Anweisung
Im obersten Abschnitt der CSV-Datei mit ausführlichen Nutzungsdaten werden die während des Abrechnungszeitraums des Monats verwendeten Dienste angezeigt. Die folgende Tabelle enthält die in diesem Abschnitt aufgeführten Begriffe und Beschreibungen.

| Begriff | Beschreibung |
| --- | --- |
|Billing Period |Der Abrechnungszeitraum, in dem die Verbrauchseinheiten verwendet wurden |
|Meter Category |Gibt den Dienst der obersten Ebene für die Nutzung an |
|Meter Sub-Category |Definiert den Typ des Azure-Diensts und kann sich auf den Tarif auswirken |
|Meter Name |Gibt die Maßeinheit für die genutzte Verbrauchseinheit an |
|Meter Region |Gibt den Standort des Rechenzentrums für bestimmte Dienste an, die basierend auf dem Standort des Rechenzentrums berechnet werden |
|SKU |Gibt den eindeutigen Systembezeichner für jede Azure-Verbrauchseinheit an |
|Einheit |Gibt die Einheit an, in der der Dienst in Rechnung gestellt wird. Beispiele sind GB, Stunden oder 10.000 s. |
|Consumed Quantity |Die Menge der Verbrauchseinheit, die während des Abrechnungszeitraums genutzt wurde |
|Included Quantity |Die Menge der Verbrauchseinheit, die kostenlos in Ihrem Abrechnungszeitraum inbegriffen ist |
|Overage Quantity |Zeigt den Unterschied zwischen der verbrauchten und der enthaltenen Menge an. Dieser Betrag wird Ihnen in Rechnung gestellt. Bei Angeboten mit nutzungsbasierter Bezahlung ohne eine im Angebot enthaltene Menge entspricht der Wert hier dem Wert unter „Verbrauchte Menge“. |
|Within Commitment |Zeigt die Gebühren für Verbrauchseinheiten an, die von Ihrem Verpflichtungsbetrag im Rahmen Ihres 6- oder 12-monatigen Angebots abgezogen werden. Die Gebühren für Verbrauchseinheiten werden in chronologischer Reihenfolge von Ihrem Verpflichtungsbetrag abgezogen. |
|Currency |Die für den aktuellen Abrechnungszeitraum verwendete Währung |
|Überschreitung |Zeigt die Gebühren für Verbrauchseinheiten an, die Ihren Verpflichtungsbetrag im Rahmen Ihres 6- oder 12-monatigen Angebots überschreiten |
|Commitment Rate |Zeigt den Verpflichtungssatz basierend auf dem Gesamtverpflichtungsbetrag im Rahmen Ihres 6- oder 12-monatigen Angebots an |
|Rate |Die Rate, die Ihnen pro abrechenbare Einheit in Rechnung gestellt wird |
|Wert |Zeigt das Ergebnis der Multiplikation der Spalte „Overage Quantity“ mit der Spalte „Rate“ an. Wenn die verbrauchte Menge die enthaltene Menge nicht überschreitet, fallen in dieser Spalte keine Gebühren an. |

### <a name="daily-usage"></a>Tägliche Nutzung

Im Abschnitt „Tägliche Nutzung“ der CSV-Datei werden Nutzungsdetails angezeigt, die sich auf die Fakturierungshäufigkeiten auswirken. Die folgende Tabelle enthält die in diesem Abschnitt aufgeführten Begriffe und Beschreibungen.

| Begriff | Beschreibung |
| --- | --- |
|Usage Date |Das Datum, an dem die Verbrauchseinheit genutzt wurde |
|Meter Category |Gibt den Dienst der obersten Ebene an, zu dem dieser Verbrauch gehört |
|Meter ID |Der Bezeichner der berechneten Verbrauchseinheit, der für die Preisfeststellung für die Nutzung verwendet wird |
|Meter Sub-Category |Definiert den Typ des Azure-Diensts und kann sich auf den Tarif auswirken |
|Meter Name |Gibt die Maßeinheit für die genutzte Verbrauchseinheit an |
|Meter Region |Gibt den Standort des Rechenzentrums für bestimmte Dienste an, die basierend auf dem Standort des Rechenzentrums berechnet werden |
|Einheit |Gibt die Einheit an, in der die Verbrauchseinheit in Rechnung gestellt wird. Beispiele sind GB, Stunden oder 10.000 s. |
|Consumed Quantity |Die Menge der Verbrauchseinheit, die an diesem Tag verbraucht wurde |
|Resource Location |Gibt das Rechenzentrum an, in dem die Verbrauchseinheit ausgeführt wird |
|Consumed Service |Der verwendete Azure-Plattformdienst |
|Ressourcengruppe |Die Ressourcengruppe, in der die bereitgestellte Verbrauchseinheit ausgeführt wird. <br/><br/>Weitere Informationen finden Sie unter [Übersicht über den Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview). |
|Instance ID | Der Bezeichner für die Verbrauchseinheit. <br/><br/> Der Bezeichner enthält den Namen, den Sie für die Verbrauchseinheit bei der Erstellung angegeben haben. Es ist entweder der Name der Ressource oder die vollqualifizierte Ressourcen-ID. Weitere Informationen finden Sie unter [Azure Resource Manager-API](https://docs.microsoft.com/rest/api/resources/resources). |
|Tags | Tag, den Sie der Verbrauchseinheit zuweisen. Verwenden Sie Tags zum Gruppieren von Abrechnungsdatensätzen.<br/><br/>Beispielsweise können Sie Tags verwenden, um Kosten nach den Abteilungen zu unterteilen, die die Verbrauchseinheit nutzen. Dienste, die das Ausgeben von Tags unterstützen, sind virtuelle Computer, Speicher und Netzwerkdienste, die mit der [Azure Resource Manager-API](https://docs.microsoft.com/rest/api/resources/resources) bereitgestellt werden. Weitere Informationen finden Sie unter [Organisieren von Azure-Ressourcen mit Tags](http://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/). |
|Additional Info |Dienstspezifische Metadaten. Dies kann beispielsweise ein Imagetyp für einen virtuellen Computer sein. |
|Service Info 1 |Der Name des Projekts, zu dem der Dienst in Ihrem Abonnement gehört |
|Service Info 2 |Ein Legacyfeld, in dem optionale dienstspezifische Metadaten erfasst werden |

## <a name="how-do-i-make-sure-that-the-charges-in-my-detailed-usage-file-are-correct"></a>Wie stelle ich sicher, dass die in meiner Datei mit detaillierten Nutzungsdaten aufgeführten Gebühren richtig sind?
Wenn Sie mehr über eine Gebühr in Ihrer Datei mit detaillierten Nutzungsdaten erfahren möchten, lesen Sie den Abschnitt [Grundlegendes über Ihre Rechnung für Microsoft Azure](./billing-understand-your-bill.md).

## <a name="external"></a>Wie verhält es sich mit Gebühren für externe Dienste?
Externe Dienste (auch als „Marketplace-Bestellungen“ bezeichnet) werden von unabhängigen Dienstanbietern bereitgestellt und separat in Rechnung gestellt. Die Gebühren werden nicht in der Azure-Rechnung aufgeführt. Weitere Informationen finden Sie unter [Grundlegendes zu Azure-Gebühren für externe Dienste](billing-understand-your-azure-marketplace-charges.md).

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Wenden Sie sich an den Support.
[Wenden Sie sich an den Support](https://portal.azure.com/?), falls Sie weitere Hilfe benötigen, um das Problem schnell beheben zu lassen.

