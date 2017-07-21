---
title: "Wiederherstellen eines Servers in Azure-Datenbank für PostgreSQL | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie Sie mit dem Azure-Portal einen Server in Azure-Datenbank für PostgreSQL wiederherstellen."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.topic: article
ms.date: 07/20/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 49d1a893f4c7c7d99bf30ac7f7665c05019b02f4
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---

# <a name="how-to-backup-and-restore-a-server-in-azure-database-for-postgresql-using-the-azure-portal"></a>Sichern und Wiederherstellen eines Servers in Azure-Datenbank für PostgreSQL mit dem Azure-Portal

## <a name="backup-happens-automatically"></a>Automatische Sicherung
Bei der Verwendung von Azure-Datenbank für PostgreSQL erstellt der Datenbankdienst automatisch alle fünf Minuten eine Sicherung des Diensts. 

Die Sicherungen sind im Basic-Tarif sieben Tage und im Standard-Tarif 35 Tage lang verfügbar. Weitere Informationen finden Sie im Artikel zu den [Dienstebenen für Azure-Datenbank für PostgreSQL](concepts-service-tiers.md).

Mithilfe dieses automatischen Sicherungsfeatures können Sie einen früheren Zustand des Servers und aller seiner Datenbanken wiederherstellen.

## <a name="restore-in-the-azure-portal"></a>Wiederherstellen im Azure-Portal
Mit Azure-Datenbank für PostgreSQL können Sie den Zustand des Servers zu einem bestimmten Zeitpunkt und auf einer neuen Kopie des Servers wiederherstellen. Sie können diesen neuen Server zur Wiederherstellung Ihrer Daten verwenden. 

Beispiel: Wenn eine Tabelle heute um 12 Uhr versehentlich gelöscht wurde, können Sie den Zustand von kurz vor 12 Uhr wiederherstellen und die fehlende Tabelle und die Daten von dieser neuen Kopie des Servers abrufen.

Mithilfe der folgenden Schritte wird der Status des Beispielservers zu einem bestimmten Zeitpunkt wiederhergestellt:
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Suchen Sie den Server für Azure-Datenbank für PostgreSQL. Klicken Sie im Azure-Portal im linken Menü auf **All Resources** (Alle Ressourcen), und geben Sie den Namen ein (z.B. **mypgserver-20170401**), um nach dem vorhandenen Server zu suchen. Klicken Sie auf den Servernamen in den Suchergebnissen. Die Seite **Übersicht** für Ihren Server wird geöffnet und enthält Optionen für die weitere Konfiguration.

   ![Azure-Portal – Suchen nach Ihrem Server](media/postgresql-howto-restore-server-portal/1-locate.png)

3. Klicken Sie oben auf dem Serverübersichtsblatt auf der Symbolleiste auf **Wiederherstellen**. Das Blatt „Wiederherstellen“ wird geöffnet.

   ![Azure-Datenbank für PostgreSQL – Übersicht – Schaltfläche „Wiederherstellen“](./media/postgresql-howto-restore-server-portal/2_server.png)

4. Geben Sie im Formular „Wiederherstellen“ die erforderlichen Informationen ein:

   ![Azure-Datenbank für PostgreSQL – Wiederherstellungsinformationen ](./media/postgresql-howto-restore-server-portal/3_restore.png)
  - **Wiederherstellungspunkt**: Wählen Sie einen Zeitpunkt vor der Änderung des Servers aus.
  - **Zielserver**: Geben Sie einen neuen Servernamen für die Wiederherstellung ein.
  - **Speicherort**: Sie können die Region nicht auswählen. Standardmäßig ist dieser Wert mit dem Quellserver identisch.
  - **Tarif**: Sie können diesen Wert beim Wiederherstellen eines Servers nicht ändern. Er ist mit dem Wert für den Quellserver identisch. 

5. Klicken Sie auf **OK**, um den Zustand eines Servers zu einem bestimmten Zeitpunkt wiederherzustellen. 

6. Suchen Sie nach Abschluss der Wiederherstellung den neuen erstellten Server, um zu überprüfen, ob die Daten wie erwartet wiederhergestellt wurden.

## <a name="next-steps"></a>Nächste Schritte
- [Datenverbindungsbibliotheken für die Azure-Datenbank für PostgreSQL](concepts-connection-libraries.md)

