---
title: "Wiederherstellen eines Servers in Azure-Datenbank für MySQL | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie Sie mit dem Azure-Portal einen Server in Azure-Datenbank für MySQL wiederherstellen."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonh
ms.assetid: 
ms.service: mysql-database
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: portal
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 0603119da20e74b423072ce6afdb8c9f20830383
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---

# <a name="how-to-backup-and-restore-a-server-in-azure-database-for-mysql-using-the-azure-portal"></a>Sichern und Wiederherstellen eines Servers in Azure-Datenbank für MySQL mit dem Azure-Portal

## <a name="backup-happens-automatically"></a>Automatische Sicherung
Bei der Verwendung von Azure-Datenbank für MySQL erstellt der Datenbankdienst automatisch alle fünf Minuten eine Sicherung des Diensts. 

Die Sicherungen sind im Basic-Tarif sieben Tage und im Standard-Tarif 35 Tage lang verfügbar. Weitere Informationen finden Sie im Artikel zu den [Dienstebenen für Azure-Datenbank für MySQL](concepts-service-tiers.md).

Mithilfe dieses automatischen Sicherungsfeatures können Sie einen früheren Zustand des Servers und aller seiner Datenbanken wiederherstellen.

## <a name="restore-in-the-azure-portal"></a>Wiederherstellen im Azure-Portal
Mit Azure-Datenbank für MySQL können Sie den Zustand des Servers zu einem bestimmten Zeitpunkt und auf einer neuen Kopie des Servers wiederherstellen. Sie können diesen neuen Server zur Wiederherstellung Ihrer Daten verwenden. 

Beispiel: Wenn eine Tabelle heute um 12 Uhr versehentlich gelöscht wurde, können Sie den Zustand von kurz vor 12 Uhr wiederherstellen und die fehlende Tabelle und die Daten von dieser neuen Kopie des Servers abrufen.

Mithilfe der folgenden Schritte wird der Status des Beispielservers zu einem bestimmten Zeitpunkt wiederhergestellt:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

2. Suchen Sie den Server für Azure-Datenbank für MySQL. Wählen Sie im linken Bereich **All resources** (Alle Ressourcen) und anschließend den Server in der Liste aus.

3.    Klicken Sie oben auf dem Serverübersichtsblatt auf der Symbolleiste auf **Wiederherstellen**. Das Blatt „Wiederherstellen“ wird geöffnet.
![Klicken auf die Schaltfläche „Wiederherstellen“](./media/howto-restore-server-portal/click-restore-button.png)

4. Geben Sie im Formular „Wiederherstellen“ die erforderlichen Informationen ein:

- **Wiederherstellungspunkt (UTC)**: Wählen Sie mithilfe der Datums- und Zeitauswahl einen Zeitpunkt aus, dessen Zustand wiederhergestellt werden soll. Die Zeit wird im UTC-Format angegeben, daher müssen Sie die Ortszeit wahrscheinlich in UTC umrechnen.
- **Auf neuem Server wiederherstellen**: Geben Sie den Namen eines neuen Servers an, auf dem der vorhandene Server wiederhergestellt werden soll.
- **Standort**: Die Region wird automatisch mit der Region des Quellservers ausgefüllt und kann nicht geändert werden.
- **Tarif**: Als Tarif wird automatisch der gleiche Tarif wie für den Quellserver festgelegt, und er kann hier nicht geändert werden. 
![PITR-Wiederherstellung](./media/howto-restore-server-portal/pitr-restore.png)

5. Klicken Sie auf **OK**, um den Zustand eines Servers zu einem bestimmten Zeitpunkt wiederherzustellen. 

6. Suchen Sie nach Abschluss der Wiederherstellung den neuen erstellten Server, um zu überprüfen, ob die Datenbanken wie erwartet wiederhergestellt wurden.

## <a name="next-steps"></a>Nächste Schritte
- [Datenverbindungsbibliotheken für die Azure-Datenbank für MySQL](concepts-connection-libraries.md)
