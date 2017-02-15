---
title: Verwalten der StorSimple-Volumecontainer | Microsoft Docs
description: "Beschreibt, wie Sie die Seite &quot;Volumecontainer&quot; des StorSimple-Manager-Dienstes zum Hinzufügen, Ändern oder Löschen eines Volumecontainers verwenden können."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 1c64ce75-1fd3-4d3b-9304-d4dc0fc2b069
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/24/2016
ms.author: v-sharos
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 4fb0f4e61ec98546e7044bf760d24a1ba932fe5b


---
# <a name="use-the-storsimple-manager-service-to-manage-storsimple-volume-containers"></a>Verwalten von StorSimple-Volumecontainern mithilfe des StorSimple Manager-Diensts
## <a name="overview"></a>Übersicht
In diesem Tutorial wird erläutert, wie Sie den StorSimple-Manager-Dienst zum Erstellen und Verwalten von StorSimple-Volumecontainern verwenden.

Ein Volumecontainer in einem Microsoft Azure StorSimple-Gerät enthält ein oder mehrere Volumes, die gemeinsame Einstellungen für das Speicherkonto, die Verschlüsselung und die Bandbreitenauslastung haben. Ein Gerät kann mehrere Volumecontainer für alle Volumes aufweisen. 

Ein Volumecontainer verfügt über die folgenden Attribute:

* **Volumes** : Die mehrstufigen oder lokalen StorSimple-Volumes, die im Volumecontainer enthalten sind. Ein Volumecontainer kann bis zu 256 StorSimple-Volumes enthalten.
* **Verschlüsselung** – Ein Verschlüsselungsschlüssel, der für jeden Volumecontainer definiert werden kann. Mit diesem Schlüssel werden die vom StorSimple-Gerät an die Cloud gesendeten Daten verschlüsselt. Zusammen mit dem vom Benutzer eingegebenen Schlüssel wird ein AES-256-Bit-Schlüssel (gemäß militärischen Sicherheitsstandards) verwendet. Zum Schutz Ihrer Daten wird empfohlen, immer die Cloudspeicherverschlüsselung zu aktivieren.
* **Speicherkonto** – Das mit dem Anbieter des Cloudspeicherdiensts verknüpfte Speicherkonto. Alle Volumes in einem Volumecontainer nutzen dieses Speicherkonto gemeinsam. Sie können ein Speicherkonto aus einer vorhandenen Liste auswählen oder beim Erstellen des Volumecontainers ein neues Konto erstellen und dann die Anmeldeinformationen für das Konto angeben.
* **Cloudbandbreite** – Die vom Gerät beanspruchte Bandbreite, wenn Daten vom Gerät an die Cloud gesendet werden. Sie können die Bandbreite steuern, indem Sie beim Definieren dieses Containers einen Wert zwischen 1 und 1.000 MBit/s angeben. Wenn das Gerät die gesamte verfügbare Bandbreite nutzen soll, legen Sie dieses Feld auf "Unbegrenzt" fest. Sie können auch eine Bandbreitenvorlage erstellen und anwenden, um Bandbreite basierend auf einem Zeitplan zuzuweisen.

![Seite "Volumecontainer"](./media/storsimple-manage-volume-containers/HCS_VolumeContainersPage.png)

Bei den folgenden Verfahren wird erläutert, wie Sie die Seite **Volumecontainer** verwenden, um die folgenden allgemeinen Vorgänge durchzuführen:

* Hinzufügen eines Volumecontainers 
* Ändern eines Volumecontainers 
* Löschen eines Volumecontainers 

## <a name="add-a-volume-container"></a>Hinzufügen eines Volumecontainers
Führen Sie die folgenden Schritte aus, um einen Volumecontainer hinzuzufügen.

[!INCLUDE [storsimple-add-volume-container](../../includes/storsimple-add-volume-container.md)]

## <a name="modify-a-volume-container"></a>Ändern eines Volumecontainers
Führen Sie die folgenden Schritte aus, um einen Volumecontainer zu ändern.

[!INCLUDE [storsimple-modify-volume-container](../../includes/storsimple-modify-volume-container.md)]

## <a name="delete-a-volume-container"></a>Löschen eines Volumecontainers
In einem Volumecontainer sind Volumes enthalten. Er kann nur gelöscht werden, nachdem alle darin enthaltenen Volumes gelöscht wurden. Führen Sie die folgenden Schritte aus, um einen Volumecontainer zu löschen.

[!INCLUDE [storsimple-delete-volume-container](../../includes/storsimple-delete-volume-container.md)]

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über das [Verwalten von StorSimple-Volumes](storsimple-manage-volumes.md). 
* Erfahren Sie mehr über das [Verwenden Ihres StorSimple-Geräts mithilfe des StorSimple Manager-Diensts](storsimple-manager-service-administration.md).




<!--HONumber=Nov16_HO3-->


