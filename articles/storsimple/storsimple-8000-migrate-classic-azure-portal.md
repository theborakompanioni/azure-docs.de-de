---
title: "Migrieren von Speicherkonten und Abonnements für den StorSimple-Geräte-Manager-Dienst | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Abonnements und Speicherkonten für Ihren StorSimple 8000-Geräte-Manager-Dienst migrieren."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/12/2017
ms.author: alkohli
ms.translationtype: HT
ms.sourcegitcommit: fda37c1cb0b66a8adb989473f627405ede36ab76
ms.openlocfilehash: a64cf0ba2a4646a03c864193fb93de4f54f38039
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---
# <a name="migrate-subscriptions-and-storage-accounts-associated-with-storsimple-device-manager-service"></a>Migrieren von Abonnements und Speicherkonten, die dem StorSimple-Geräte-Manager-Dienst zugeordnet sind

Sie müssen möglicherweise den StorSimple-Dienst zu einer neuen Registrierung oder einem neuen Abonnement verschieben. Diese Migrationsszenarien sind entweder Kontoänderungen oder Rechenzentrumsänderungen. Die folgende Tabelle informiert Sie inklusive detaillierter Schrittbeschreibungen zum Verschieben darüber, welche dieser Szenarien unterstützt werden.

## <a name="account-changes"></a>Kontoänderungen

| Können Sie Folgendes verschieben?| Unterstützt| Ausfallzeit| Azure-Supportprozess| Vorgehensweise|
|-----|-----|-----|-----|-----|
| Ein gesamtes Abonnement (einschließlich StorSimple-Dienst und Speicherkonten) zu einer anderen Registrierung | Ja       | Nein       | **Übertragung der Registrierung**<br>Verwendung:<li>Wenn Sie eine neue Azure-Verpflichtung im Rahmen eines neuen Vertrags erwerben.</li><li>Sie möchten alle Konten und Abonnements von der alten Registrierung zur neuen migrieren. Dies schließt alle Azure-Dienste unter dem alten Abonnement ein.</li> | **Schritt 1: Öffnen Sie ein Azure Enterprise Operation Support-Ticket.**<li>Wechseln Sie zu [http://aka.ms/AzureEnt](http://aka.ms/AzureEnt).</li><li> Wählen Sie **Registrierungsverwaltung** und dann **Übertragung von einer Registrierung zu einer neuen Registrierung**.<br>**Schritt 2: Geben Sie die angeforderten Informationen an**<br>Inklusive:<li>Quellenregistrierungsnummer</li><li> Zielregistrierungsnummer</li><li>Datum des Inkrafttretens der Übertragung|
| StorSimple-Dienst aus einem vorhandenen Konto zu einer neuen Registrierung    | Ja       | Nein       | **Kontoübertragung**<br>Verwendung:<li>Wenn Sie keine vollständige Registrierungsübertragung wünschen.</li><li>Sie möchten nur bestimmte Konten zu einer neuen Registrierung verschieben.</li>| **Schritt 1: Öffnen Sie ein Azure Enterprise Operation Support-Ticket.**<li>Wechseln Sie zu [http://aka.ms/AzureEnt](http://aka.ms/AzureEnt).</li><li>Wählen Sie **Registrierungsverwaltung** und dann **Übertragung eines EA-Kontos zu einer neuen Registrierung**.<br>**Schritt 2: Geben Sie die angeforderten Informationen an**<br>Inklusive:<li>Quellenregistrierungsnummer</li><li> Zielregistrierungsnummer</li><li>Datum des Inkrafttretens der Übertragung|
| StorSimple-Dienst von einem Abonnement zu einem anderen Abonnement      | Nein        |    Ja         | Weder noch, dies ist ein manueller Prozess.|<li>Migrieren Sie die Daten aus dem StorSimple-Gerät.</li><li>Führen Sie eine Zurücksetzung des Geräts auf die Werkseinstellungen durch. Damit werden alle lokalen Daten auf dem Gerät gelöscht.</li><li>Registrieren Sie das Gerät mit dem neuen Abonnement bei einem StorSimple-Geräte-Manager-Dienst.</li><li>Migrieren Sie die Daten zurück zum Gerät.|
| StorSimple-Gerät von einem StorSimple-Geräte-Manager-Dienst zu einem anderen Dienst in einer anderen Region      | Nein        | Ja            | Weder noch, dies ist ein manueller Prozess. |Wie oben.|

## <a name="datacenter-changes"></a>Rechenzentrumsänderungen

| Können Sie Folgendes verschieben?| Unterstützt|Ausfallzeit| Azure-Supportprozess| Vorgehensweise|
|-----|-----|-----|-----|-----|-----|
| Einen StorSimple-Dienst aus einem Azure-Rechenzentrum zu einem anderen | Nein | Ja |Weder noch, dies ist ein manueller Prozess.  |<li>Migrieren Sie die Daten aus dem StorSimple-Gerät.</li><li>Führen Sie eine Zurücksetzung des Geräts auf die Werkseinstellungen durch. Damit werden alle lokalen Daten auf dem Gerät gelöscht.</li><li>Registrieren Sie das Gerät mit dem neuen Abonnement bei einem neuen StorSimple-Geräte-Manager-Dienst.</li><li>Migrieren Sie die Daten zurück zum Gerät.|
| Ein Speicherkonto aus einem Azure-Rechenzentrum zu einem anderen | Nein |Ja  |Weder noch, dies ist ein manueller Prozess.  | Wie oben.|

## <a name="next-steps"></a>Nächste Schritte

* [Bereitstellen des StorSimple-Geräte-Manager-Diensts für Geräte der StorSimple 8000-Serie](storsimple-8000-manage-service.md)
* [Bereitstellen lokaler StorSimple-Geräte (Update 3 und höher)](storsimple-8000-deployment-walkthrough-u2.md)
