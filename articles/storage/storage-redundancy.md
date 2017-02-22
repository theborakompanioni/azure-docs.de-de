---
title: Datenreplikation in Azure Storage | Microsoft-Dokumentation
description: "Die Daten in Ihrem Microsoft Azure-Speicherkonto werden stets repliziert, um Beständigkeit und hohe Verfügbarkeit sicherzustellen. Die Redundanzoptionen umfassen den lokal redundanten Speicher (LRS), den zonenredundanten Speicher (ZRS), den georedundanten Speicher (GRS) und den georedundanten Speicher mit Lesezugriff (RA-GRS)."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 86bdb6d4-da59-4337-8375-2527b6bdf73f
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: marsma
translationtype: Human Translation
ms.sourcegitcommit: 349be81b5d1d5ccc1510360974b4e3b10471cf7f
ms.openlocfilehash: 13cd31bdce89ae898a6e22a1d27b5aed819ccc0a


---
# <a name="azure-storage-replication"></a>Azure Storage-Replikation
Die Daten in Ihrem Microsoft Azure-Speicherkonto werden stets repliziert, um Beständigkeit und hohe Verfügbarkeit sicherzustellen. Bei der Replikation werden Ihre Daten kopiert, und zwar entweder innerhalb desselben Rechenzentrums, oder in ein zweites Rechenzentrum. Dies hängt davon ab, welche Replikationsoption Sie wählen. Mit der Replikation werden Ihre Daten geschützt, und die Betriebszeit der Anwendung wird hoch gehalten, falls es zu vorübergehenden Hardwareausfällen kommt. Wenn Ihre Daten in einem zweiten Rechenzentrum repliziert werden, werden Ihre Daten außerdem vor einem Ausfall aufgrund einer Katastrophe am primären Standort geschützt.

Mit der Replikation wird sichergestellt, dass Ihr Speicherkonto auch bei Ausfällen die [Servicelevelvereinbarung (SLA) für Storage](https://azure.microsoft.com/support/legal/sla/storage/) erfüllt. Die Servicelevelvereinbarung enthält Informationen zu Azure Storage-Garantien in Bezug auf Dauerhaftigkeit und Verfügbarkeit.

Wenn Sie ein Speicherkonto erstellen, können Sie eine der folgenden Replikationsoptionen auswählen:

* [Lokal redundanter Speicher (LRS)](#locally-redundant-storage)
* [Zonenredundanter Speicher (ZRS)](#zone-redundant-storage)
* [Georedundanter Speicher (GRS)](#geo-redundant-storage)
* [Georedundanter Speicher mit Lesezugriff (RA-GRS)](#read-access-geo-redundant-storage)

Georedundanter Speicher mit Lesezugriff (RA-GRS) ist die Standardoption bei der Erstellung eines neuen Speicherkontos.

Die folgende Tabelle bietet eine schnelle Übersicht über die Unterschiede zwischen LRS, ZRS, GRS und RA-GRS. In den nachfolgenden Abschnitten werden die einzelnen Replikationstypen ausführlicher behandelt.

| Replikationsstrategie | LRS | ZRS | GRS | RA-GRS |
|:--- |:--- |:--- |:--- |:--- |
| Daten werden in mehreren Datencentern repliziert. |Nein |Ja |Ja |Ja |
| Daten können vom sekundären Standort sowie vom primären Standort aus gelesen werden. |Nein |Nein |Nein |Ja |
| Anzahl von Datenkopien, die auf separaten Knoten aufbewahrt werden. |3 |3 |6 |6 |

Informationen zu den Preisen für die verschiedenen Redundanzoptionen finden Sie unter [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/) .

> [!NOTE]
> Storage Premium unterstützt nur lokal redundanten Speicher (Locally Redundant Storage, LRS). Informationen zu Storage Premium finden Sie unter [Storage Premium: Hochleistungsspeicher für Workloads virtueller Azure-Computer](storage-premium-storage.md).
>
>

## <a name="locally-redundant-storage"></a>Lokal redundanter Speicher
Bei Lokal redundanter Speicher (LRS) werden Ihre Daten dreimal in einer Speicherskalierungseinheit repliziert, die in einem Datencenter in der Region gehostet wird, in der Sie Ihr Speicherkonto erstellt haben. Eine Schreibanforderung wird nur einmal erfolgreich zurückgegeben, nachdem sie in alle drei Replikate geschrieben wurde. Diese drei Replikate befinden sich in separaten Fehler- und Upgradedomänen innerhalb einer Speicherskalierungseinheit.

Eine Speicherskalierungseinheit ist eine Sammlung von Speicherknotengestellen (Racks). Eine Fehlerdomäne (FD) ist eine Gruppe von Knoten, die eine physische Fehlereinheit darstellen und als Knoten angesehen werden können, die zum gleichen physischen Gestell gehören. Eine Upgradedomäne (UD) ist eine Gruppe von Knoten, die während des Vorgangs eines Dienstupgrades (Rollouts) gemeinsam aktualisiert werden. Die drei Replikate sind auf UDs und FDs in einer Speicherskalierungseinheit verteilt, um sicherzustellen, dass Daten verfügbar sind, auch wenn ein Hardwarefehler sich auf ein einzelnes Gestell auswirkt oder wenn Knoten während eines Rollouts aktualisiert werden.

LRS ist die kostengünstigste Option und bietet im Vergleich mit anderen Optionen die geringste Dauerhaftigkeit. Bei einem Katastrophenfall auf Datencenterebene (Feuer, Überschwemmung usw.) gehen alle drei Replikate unter Umständen verloren oder können nicht mehr wiederhergestellt werden. Zur Verringerung dieses Risikos ist für die meisten Anwendungen der georedundante Speicher (GRS) zu empfehlen.

Lokal redundanter Speicher kann für bestimmte Szenarien aber trotzdem geeignet sein:

* Er bietet die höchste maximale Bandbreite von Azure Storage-Replikationsoptionen.
* Wenn Ihre Anwendung Daten speichert, die problemlos wiederhergestellt werden können, können Sie sich für LRS entscheiden.
* Einige Anwendungen sind aufgrund von Datenvorschriften auf die Replikation von Daten innerhalb eines Lands beschränkt. Eine gekoppelte Region kann sich in einem anderen Land befinden. Informationen zu Regionspaaren finden Sie unter [Azure-Regionen](https://azure.microsoft.com/regions/).

## <a name="zone-redundant-storage"></a>Zonenredundanter Speicher
Beim zonenredundanten Speicher (ZRS) werden Ihre Daten zusätzlich zur Speicherung von drei Replikaten (ähnlich wie bei LRS) asynchron in Datencentern in einer oder zwei Regionen repliziert. Dies führt zu einer höheren Dauerhaftigkeit als bei LRS. Per ZRS gespeicherte Daten bleiben auch dann dauerhaft vorhanden, wenn das primäre Datencenter nicht verfügbar oder nicht mehr wiederherstellbar ist.
Kunden, die die Verwendung von ZRS planen, sollten Folgendes beachten:

* ZRS ist nur für Blockblobs in Speicherkonten für allgemeine Zwecke verfügbar und wird nur für die Speicherdienstversionen 2014-02-14 und höher unterstützt.
* Da eine asynchrone Replikation eine Verzögerung einschließt, gehen bei einem lokalen Notfall Änderungen, die noch nicht am sekundären Ort repliziert wurden, unter Umständen verloren, wenn die Daten am primären Ort nicht wiederhergestellt werden können.
* Das Replikat ist ggf. erst wieder verfügbar, wenn von Microsoft das Failover an den sekundären Ort initiiert wird.
* ZRS-Konten können später nicht in LRS oder GRS konvertiert werden. Ebenso kann ein vorhandenes LRS- oder GRS-Konto nicht in ein ZRS-Konto konvertiert werden.
* ZRS-Konten verfügen nicht über Metriken oder eine Protokollierungsfunktion.

## <a name="geo-redundant-storage"></a>Georedundanter Speicher
Georedundanter Speicher (GRS) repliziert Ihre Daten in eine sekundäre Region, die Hunderte von Kilometern von der primären Region entfernt ist. Wenn für Ihr Speicherkonto GRS aktiviert ist, sind Ihre Daten beständig gespeichert, selbst bei einem regionalen Komplettausfall oder einem Notfall, nach dem die primäre Region nicht mehr wiederherstellbar ist.

Bei ein Speicherkonto mit aktiviertem GRS wird für ein Update zuerst ein Commit in der primären Region ausgeführt, in der es drei Mal repliziert wird. Anschließend wird das Update asynchron in der sekundären Region repliziert, wo es ebenfalls dreimal repliziert wird.

Bei GRS werden Replikate sowohl in der primären als auch der sekundären Region über separate Fehlerdomänen und Upgradedomänen hinweg in einer Speicherskalierungseinheit verwaltet (wie bei LRS beschrieben).

Überlegungen:

* Da eine asynchrone Replikation eine Verzögerung einschließt, gehen bei einem regionalen Notfall Änderungen, die noch nicht in der sekundären Region repliziert wurden, unter Umständen verloren, wenn die Daten nicht aus der primären Region wiederhergestellt werden können.
* Das Replikat ist nicht verfügbar, sofern von Microsoft kein Failover in die sekundäre Region initiiert wird. Wenn Microsoft ein Failover auf die sekundäre Region initiiert, erhalten Sie nach Abschluss des Failovers Lese- und Schreibzugriff auf diese Daten. Weitere Informationen finden Sie im [Leitfaden zur Notfallwiederherstellung](storage-disaster-recovery-guidance.md). 
* Wenn eine Anwendung Daten aus der sekundären Region lesen können soll, muss der Benutzer RA-GRS aktivieren.

Wenn Sie ein Speicherkonto erstellen, wählen Sie die primäre Region für das Konto aus. Die sekundäre Region wird basierend auf der primären Region bestimmt und kann nicht geändert werden. In der folgenden Tabelle werden die Paare primärer und sekundärer Regionen gezeigt:

| Primär | Sekundär |
| --- | --- |
| USA (Mitte/Norden) |USA, Süden-Mitte |
| USA, Süden-Mitte |USA (Mitte/Norden) |
| USA (Ost) |USA (West) |
| USA (West) |USA (Ost) |
| USA (Ost 2) |USA (Mitte) |
| USA (Mitte) |USA (Ost 2) |
| Nordeuropa |Europa, Westen |
| Europa, Westen |Nordeuropa |
| Südostasien |Ostasien |
| Ostasien |Südostasien |
| Ostchina |Nordchina |
| Nordchina |Ostchina |
| Japan Ost |Japan (Westen) |
| Japan (Westen) |Japan Ost |
| Brasilien Süd |USA, Süden-Mitte |
| Australien (Osten) |Australien, Südosten |
| Australien, Südosten |Australien (Osten) |
| Indien, Süden |Indien, Mitte |
| Indien, Mitte |Indien, Süden |
| US Government, Iowa |US Government, Virginia |
| US Government, Virginia |US Government, Iowa |
| Kanada, Mitte |Kanada, Osten |
| Kanada, Osten |Kanada, Mitte |
| UK, Westen |UK, Süden |
| UK, Süden |UK, Westen |
| Deutschland, Mitte |Deutschland, Nordosten |
| Deutschland, Nordosten |Deutschland, Mitte |
| USA, Westen 2 |USA, Westen-Mitte |
| USA, Westen-Mitte |USA, Westen 2 |

Aktuelle Informationen zu von Azure unterstützten Regionen finden Sie unter [Azure-Regionen](https://azure.microsoft.com/regions/).

## <a name="read-access-geo-redundant-storage"></a>Georedundanter Speicher mit Lesezugriff
Georedundanter Speicher mit Lesezugriff (RA-GRS) maximiert die Verfügbarkeit für das Speicherkonto, indem ein schreibgeschützter Zugriff auf Daten am sekundären Standort zusätzlich zur von GRS gebotenen Replikation in zwei Regionen bereitgestellt wird.

Wenn Sie den schreibgeschützten Zugriff auf Ihre Daten in der sekundären Region aktivieren, sind Ihre Daten zusätzlich zum primären Endpunkt für Ihr Speicherkonto auf einem sekundären Endpunkt verfügbar. Der sekundäre Endpunkt ähnelt dem primären Endpunkt, wobei das Suffix `–secondary` an den Kontonamen angefügt wird. Wenn Ihr primärer Endpunkt für den Blob-Dienst z. B. `myaccount.blob.core.windows.net` ist, dann ist Ihr sekundärer Endpunkt `myaccount-secondary.blob.core.windows.net`. Die Zugriffsschlüssel für das Speicherkonto sind für die primären und sekundären Endpunkte identisch.

Überlegungen:

* Von Ihrer Anwendung muss verwaltet werden, mit welchem Endpunkt sie bei Verwendung von RA-GRS interagieren muss.
* Da eine asynchrone Replikation eine Verzögerung einschließt, gehen bei einem regionalen Notfall Änderungen, die noch nicht in der sekundären Region repliziert wurden, unter Umständen verloren, wenn die Daten nicht aus der primären Region wiederhergestellt werden können.
* Wenn Microsoft ein Failover auf die sekundäre Region initiiert, erhalten Sie nach Abschluss des Failovers Lese- und Schreibzugriff auf diese Daten. Weitere Informationen finden Sie im [Leitfaden zur Notfallwiederherstellung](storage-disaster-recovery-guidance.md). 
* RA-GRS ist für hohe Verfügbarkeit ausgelegt. Eine Anleitung zur Skalierbarkeit finden Sie in der [Checkliste zur Leistung](storage-performance-checklist.md).

## <a name="next-steps"></a>Nächste Schritte
* [Entwerfen hochverfügbarer Anwendungen mithilfe des RA-GRS-Speichers](storage-designing-ha-apps-with-ragrs.md)
* [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/)
* [Informationen zu Azure-Speicherkonten](storage-create-storage-account.md)
* [Skalierbarkeits- und Leistungsziele für Azure-Speicher](storage-scalability-targets.md)
* [Microsoft Azure Storage Redundancy Options and Read Access Geo Redundant Storage (in englischer Sprache) ](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)
* [SOSP Paper - Azure Storage: A Highly Available Cloud Storage Service with Strong Consistency (SOSP-Dokument – Azure Storage: ein hochverfügbarer Cloudspeicherdienst mit starker Konsistenz)](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)




<!--HONumber=Jan17_HO4-->


