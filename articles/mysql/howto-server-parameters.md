---
title: "Konfigurieren von Serverparametern in Azure-Datenbank für MySQL | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie Sie verfügbare Serverparameter in Azure Database für MySQL mithilfe des Azure-Portals konfigurieren können."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 06/19/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 1500c02fa1e6876b47e3896c40c7f3356f8f1eed
ms.openlocfilehash: 718bbf359253849fbc989c563ffd6d1099f92348
ms.contentlocale: de-de
ms.lasthandoff: 06/30/2017

---

# <a name="how-to-configure-server-parameters-in-azure-database-for-mysql-using-the-azure-portal"></a>Konfigurieren Sie Serverparameter in Azure-Datenbank für MySQL mit dem Azure-Portal

Azure Database für MySQL unterstützt das Konfigurieren einiger Serverparameter. In diesem Artikel wird beschrieben, wie diese Parameter mithilfe des Azure-Portals konfiguriert werden können. Darüber hinaus werden die unterstützten Parameter, die Standardwerte und der gültige Wertebereich genannt. Nicht alle Serverparameter können angepasst werden. Nur die hier aufgeführten Serverparameter werden unterstützt.

## <a name="navigate-to-server-parameters-blade-on-azure-portal"></a>Navigieren Sie im Azure-Portal zum Blatt „Serverparameter“

Melden Sie sich am Azure-Portal an, und klicken Sie dann auf den Servernamen Ihrer Azure Database für MySQL. Klicken Sie im Abschnitt **EINSTELLUNGEN** auf **Serverparameter**, um das Blatt „Serverparameter“ für die Azur Database für MySQL zu öffnen.

![Blatt „Serverparameter“ im Azure-Portal](./media/howto-server-parameters/auzre-portal-server-parameters.png)

## <a name="list-of-configurable-server-parameters"></a>Liste der konfigurierbaren Serverparameter

Der folgenden Tabelle sind die derzeit unterstützten Serverparameter zu entnehmen. Die Parameter können gemäß Ihrer Anwendungsanforderungen konfiguriert werden.

> [!div class="mx-tableFixed"]
|Parametername|Standardwert|Range|Beschreibung|
|---|---|---|---|
|binlog_group_commit_sync_delay|1000|0, 11-1000000|Steuert, wie viele Mikrosekunden das binäre Protokoll-Commit abwartet, bevor die binäre Protokolldatei mit dem Datenträger synchronisiert wird.|
|binlog_group_commit_sync_no_delay_count|0|0-1000000|Maximale Anzahl von Transaktionen, die abgewartet wird, bevor die aktuelle Verzögerung wie in binlog-group-commit-sync-delay spezifiziert abgebrochen wird.|
|character_set_server|LATIN1|BIG5, UTF8MB4, etc.|Verwenden Sie charset_name als den Standardzeichensatz für den Server.|
|div_precision_increment|4|0-30|Anzahl der Dezimalstellen, um die der Ergebnisbereich bei Divisionsvorgängen erhöht wird.|
|group_concat_max_len|1.024|4-16777216|Maximal zulässige Länge von GROUP_CONCAT() in Bytes.|
|innodb_adaptive_hash_index|EIN|EIN, AUS|Gibt an, ob adaptive innodb-Hashindizes aktiviert oder deaktiviert sind.|
|innodb_lock_wait_timeout|50|1-3600|Zeit in Sekunden, die eine InnoDB-Transaktion auf eine Zeilensperre wartet, bevor sie aufgibt.|
|interactive_timeout|1800|10-1800|Zeit in Sekunden, die der Server auf die Aktivität einer interaktiven Verbindung wartet, bevor er diese abschließt.|
|log_bin_trust_function_creators|AUS|EIN, AUS|Diese Variable ist anwendbar, wenn die binäre Protokollierung aktiviert ist. Sie steuert, ob den Erstellern gespeicherter Funktionen vertraut werden kann, dass sie keine gespeicherten Funktionen erstellen, die unsichere Ereignisse in das binäre Protokoll aufnehmen.|
|log_queries_not_using_indexes|AUS|EIN, AUS|Protokollabfragen, von denen das Abrufen aller Zeilen in ein langsames Abfrageprotokoll erwartet wird.|
|log_slow_admin_statements|AUS|EIN, AUS|Einschließen der langsamen Verwaltungsanweisungen in die Anweisungen, die in das langsame Abfrageprotokoll geschrieben werden.|
|log_throttle_queries_not_using_indexes|0|0-4294967295|Begrenzt die Anzahl von Abfragen pro Minute, die in das langsame Abfrageprotokoll geschrieben werden können.|
|long_query_time|10|1-1E + 100|Bei Abfragen Verarbeitungszeit in Sekunden, nach deren Ablauf der Server die Statusvariable Slow_queries inkrementiert.|
|max_allowed_packet|536870912|1024-1073741824|Maximale Größe eines Pakets oder einer beliebigen generierten / Zwischenzeichenfolge oder eines beliebigen Parameters, die von der C-API-Funktion mysql_stmt_send_long_data() gesendet werden.|
|min_examined_row_limit|0|0-18446744073709551615|Protokolliert Abfragen, bei denen die Anzahl der Zeilen die im langsamen Abfrageprotokoll konfigurierte Anzahl von Zeilen übersteigt. |
|server_id|3293747068|1000-4294967295|Server-ID, die für die Replikation verwendet wird, um allen Masters und Slaves eine eindeutige Identität zu geben.|
|slave_net_timeout|60|30-3600|Wartezeit in Sekunden, auf die auf weitere Daten vom Master gewartet wird, bevor der Slave die Verbindung als unterbrochen betrachtet, das Lesen abbricht und versucht, eine neue Verbindung aufzubauen.|
|slow_query_log|AUS|EIN, AUS|Aktivieren oder Deaktivieren des langsamen Abfrageprotokolls.|
|sql_mode|0 ausgewählt|ALLOW_INVALID_DATES, IGNORE_SPACE, etc.|Der aktuelle SQL-Modus des Servers.|
|time_zone|SYSTEM|Beispiele: -8:00, +05:30|Die Zeitzone des Servers.|
|wait_timeout|120|60-240|Wartezeit in Sekunden, die der Server auf eine Aktivität oder eine nicht interaktive Verbindung wartet, bevor er diese unterbricht.|

## <a name="next-steps"></a>Nächste Schritte
- [Datenverbindungsbibliotheken für Azure-Datenbank für MySQL](concepts-connection-libraries.md)

